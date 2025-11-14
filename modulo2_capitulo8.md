# Capítulo 8. Control de Flujo, Errores y Congestión + HDLC (Deep Dive, sin ejercicios)

Objetivo
Comprender con claridad:
- Qué regulan el control de flujo (1→1), el control de errores (fiabilidad del enlace) y el control de congestión (1→N, salud de la red).
- Cómo se dimensionan ventanas, timeouts y numeraciones.
- Cómo operan ARQ (Stop‑and‑Wait, Go‑Back‑N, Selective‑Repeat).
- Qué resuelven CRC y checksums y qué NO resuelven.
- Cómo funciona HDLC: tramas, campos, bit stuffing, P/F, y su relación con GBN/SR.
- Cómo controlan la congestión TCP y la red (AQM, ECN), y qué significa “fairness”.

Convenciones SI
- Tasas: R_b (bit/s)
- Tiempos: s (ms, µs si se expresa así)
- Distancia: m; Velocidad de propagación v_prop (m/s)
- Tamaños: bits, bytes (1 B = 8 bits)
- Probabilidades: adimensional
- dB: adimensional (relaciones logarítmicas)

8.1 Diferencias esenciales (qué regula cada mecanismo)
- Control de flujo (Flow control, 1→1):
  - Evita desbordar el buffer del receptor.
  - Regula cuántas unidades pueden estar “en vuelo”.
  - Magnitudes clave: ventana (W), RTT aproximado, producto banda‑retardo (BDP).
- Control de errores (Error control):
  - Detecta/corrige errores de bits/ráfagas introducidos por el canal.
  - Técnicas: detección (CRC/checksum) + ARQ (retransmisión), o bien FEC (Forward Error Correction) cuando conviene.
- Control de congestión (Congestion control, 1→N):
  - Evita que la red se colapse cuando la suma de ofertas supera la capacidad de los nodos/enlaces.
  - Señales: pérdidas, colas largas (RTT sube), marcas explícitas (ECN).
  - Actúa a nivel de transporte (TCP) y/o de red (AQM en routers).

8.2 Parámetros de tiempo y “bits en vuelo”
- Tiempo de transmisión de una unidad (trama/segmento): t_tx = n_bits / R_b
- Tiempo de propagación: t_prop = d / v_prop
- RTT aproximado (enlace simple): RTT ≈ 2 · t_prop + procesados
- Relación adimensional: a = t_prop / t_tx (mide latencia de propagación frente a la de transmisión)
- Producto banda‑retardo (BDP): BDP = R_b · t_prop (bits). Intuición: “longitud del tubo” en bits.
- Regla de ventana para saturar sin errores: se necesita mantener al menos ≈ BDP bits en vuelo (más 1 trama), lo que se traduce en:
  - W_min (en tramas) ≈ (BDP + n_bits_trama) / n_bits_trama
  - En términos de a: W_min ≈ 2a + 1 (aprox. para enlace simétrico y SW/ventana ideal)

8.3 ARQ y control de flujo por ventanas
8.3.1 Stop‑and‑Wait (SW)
- Operación: 1 trama y esperar ACK. Sencillo, pero ineficiente si a es grande.
- Eficiencia sin errores (utilización del enlace): η_SW = 1 / (1 + 2a)
- Con errores (aprox., suponiendo ACKs sin error): η_SW ≈ (1 − p_frame) / (1 + 2a)
- Uso típico: enlaces de RTT muy corto o sistemas extremadamente simples donde la eficiencia no es crítica.

8.3.2 Ventana deslizante (Sliding Window)
- Permite tener varias tramas sin ACK “en vuelo”.
- Ventana máxima W (en tramas): impuesta por memoria/buffers y por la numeración disponible.
- Eficiencia sin errores:
  - Si W < 2a + 1: η = W / (1 + 2a)
  - Si W ≥ 2a + 1: η → 1 (saturas el enlace)

Numeración (Q bits) y límites de W
- Los números de secuencia van módulo 2^Q.
- Go‑Back‑N (GBN): W ≤ 2^Q − 1
- Selective‑Repeat (SR): W ≤ 2^{Q−1}
  - Motivo: con SR deben evitarse ambigüedades entre tramas nuevas y antiguas aún en tránsito/ACK.

Piggybacking
- Concepto de eficiencia: incluir acuses (ACKs) en las tramas de datos que ya iban a enviarse en el sentido opuesto (campo de recepción N(R)).
- Reduce el número de tramas de supervisión (S) dedicadas solo a ACK.

Timeouts
- Deben cubrir al menos un RTT estimado con margen: T_timeout > α · RTT, con α > 1 (típico ≈ 2).
- Demasiado corto: falsas retransmisiones; demasiado largo: lentitud en recuperar pérdidas.

8.4 Mecanismos ARQ
8.4.1 Go‑Back‑N (GBN)
- Al perder la trama i, se retransmiten i y todas las posteriores no confirmadas.
- Simple en receptor (acepta solo orden estricto).
- Eficiencia con errores (para W ≥ 2a + 1, aproximación estándar):
  - η_GBN ≈ (1 − p_frame) / (1 + 2a · p_frame)
- Pros/Contras:
  - + Simplicidad receptora, buffers pequeños en RX.
  - − Penaliza fuerte cuando p_frame o a crecen (mucho desperdicio por arrastre).

8.4.2 Selective‑Repeat (SR)
- Solo se retransmiten las tramas perdidas; receptor acepta fuera de orden y reordena.
- Requiere más buffers en el receptor y cuidado con numeración (W ≤ 2^{Q−1}).
- Con W ≥ 2a + 1 y errores moderados: η_SR ≈ 1 − p_frame (mejor que GBN).
- Pros/Contras:
  - + Mucho más eficiente en presencia de errores.
  - − Mayor complejidad y requisitos de memoria en RX.

Relación con tamaño de trama y p_b
- p_frame depende del tamaño de la trama y de p_b: p_frame = 1 − (1 − p_b)^{n_bits}
- A igual p_b, tramas grandes → p_frame mayor → más pérdidas a nivel de trama → peor η (sobre todo en GBN).

8.5 Detección/corrección de errores
8.5.1 CRC (Cyclic Redundancy Check)
- Detección potente de errores de ráfaga y aleatorios.
- Procedimiento: el emisor divide el mensaje (polinomio M(x)) por un generador G(x) (grado r) en GF(2) y envía el residuo (r bits) como FCS. El receptor verifica que el resto sea 0.
- Capacidad de detección:
  - Detecta todos los errores de longitud ≤ r.
  - Prob. de no detección de un error aleatorio ≈ 2^{−r}.
- Ejemplos de generadores (orientativos): CRC‑16, CRC‑32 (diferentes polinomios estandarizados).

8.5.2 Checksum
- Suma en complemento a uno de palabras de m bits (p. ej., 16 o 32). Menos potente que CRC, barata de calcular. Útil para detectar “errores suaves”.

8.5.3 Códigos bloque y FEC (visión)
- Códigos (n,k): de k bits de información a n bits codificados; tasa R_c = k/n.
- Distancia mínima d_min:
  - Detección segura hasta q errores si d_min ≥ q + 1.
  - Corrección segura hasta t errores si d_min ≥ 2t + 1.
- FEC útil cuando la retransmisión es cara o imposible (difusión, satélite de gran RTT, tiempo real estricto).

8.6 HDLC (High‑level Data Link Control)
8.6.1 Formato de trama
- Flag (8 bit, 0x7E)
- Address (8 bit; puede extenderse)
- Control (8/16 bit, depende de modo/modo extendido)
- Information (longitud variable, opcional según tipo)
- FCS (16/32 bit; CRC)
- Flag (8 bit, 0x7E)

8.6.2 Tipos de trama
- I (Information): datos de capa superior, con N(S) (número de envío) y N(R) (número de recepción, ACK por piggybacking).
- S (Supervision): RR (Ready to Receive), RNR (Receiver Not Ready), REJ (reject, para GBN), SREJ (Selective Reject, para SR).
- U (Unnumbered): gestión (SABM/SABME: establecimiento, UA: reconocimiento, DISC: desconexión, XID, TEST…)

8.6.3 Bit P/F (Poll/Final)
- En comandos desde la estación “maestra” (o en enlaces balanceados, según rol), P solicita respuesta inmediata; en respuestas, F marca “respuesta final”.
- Útil para diálogo de control y para forzar respuestas rápidas.

8.6.4 Bit stuffing (muy importante, no confundir con piggybacking)
- Como las tramas empiezan/terminan con 0x7E (01111110), el emisor inserta un ‘0’ automáticamente tras cualquier secuencia de cinco ‘1’ consecutivos en el campo Information/Control para evitar que aparezca la bandera dentro del contenido.
- En recepción, tras cinco ‘1’ seguidos, se descarta el ‘0’ siguiente (destuffing).
- Piggybacking, en cambio, es “trasladar” ACKs dentro de tramas I; no tiene que ver con insertar ‘0’.

8.6.5 Modos y ventanas
- Configuraciones: no balanceada (Primaria/Secundarias, NRM/ARM) y balanceada (ABM).
- Ventanas y numeración: HDLC soporta tanto GBN (REJ) como SR (SREJ) según el modo/implementación; límites de W vienen de Q bits (modulo 8 o 128 en extendido).

8.7 Control de congestión
8.7.1 Qué es congestión
- No es que el receptor se desborde, sino que la red (nodos intermedios) acumula colas por exceso de tráfico. Síntomas: pérdidas (por colas llenas), subida de RTT (colas largas), variabilidad del retardo.

8.7.2 Señalización de congestión
- Implícita (TCP clásico): deduce congestión por pérdidas y/o aumento de RTT.
- Explícita:
  - ECN (Explicit Congestion Notification): routers marcan en IP (ECT/ECN‑CE) y TCP reflejan en flags (ECE/CWR) evitando perder paquetes.
  - Tecnologías antiguas: FECN/BECN (Frame Relay/ATM).

8.7.3 TCP (visión necesaria)
- Ventana de congestión (cwnd) y umbral (ssthresh).
- Slow Start: cwnd crece exponencialmente (≈ duplica por RTT) hasta ssthresh o pérdida.
- Congestion Avoidance: crecimiento lineal (≈ 1 MSS por RTT).
- Pérdidas y recuperación:
  - Fast retransmit/recovery (tres ACK duplicados): cwnd reduce multiplicativamente (≈ mitad) y entra en recuperación rápida.
  - Timeout: cwnd colapsa (reinicia lento).
- Variantes:
  - Reno/NewReno: AIMD clásico.
  - CUBIC: crecimiento cúbico (predeterminado en Linux modernos).
  - BBR: modela tasa de entrega y RTT mínimo para fijar una tasa objetivo (diferente filosofía).
- Throughput TCP “ecuación de Mathis” (intuición):
  - T ≲ C · MSS / (RTT · √p) (C ≈ 1…1.22 según versión), donde p es prob. de pérdida. No la memorices al detalle, entiende que más pérdidas/RTT ⇒ menor throughput.

8.7.4 Gestión activa de colas (AQM) en routers
- Tail‑drop (sin AQM): deja entrar hasta llenar; después, todo lo que llega se pierde → colas grandes (bufferbloat) y pérdidas en ráfagas.
- RED (Random Early Detection): empieza a descartar/“marcar” antes de llenar para evitar colas enormes.
- CoDel: controla el tiempo de permanencia en cola (tiempo en cola objetivo), reduciendo bufferbloat.
- ECN + AQM: preferible “marcar” que perder cuando los extremos soportan ECN.

8.7.5 Fairness (equidad)
- Idealmente, flujos comparten recursos de forma “justa”. Métrica clásica: índice de Jain.
- En la práctica, distintas variantes (CUBIC/BBR) y RTTs distintos pueden romper la equidad perfecta.

8.8 Guía de decisiones (cómo pensar sin números)
- ¿RTT grande (a alto)? ⇒ SW será ineficiente; usa ventana deslizante con W ≥ 2a + 1.
- ¿Errores esporádicos? ⇒ SR típicamente mejor que GBN (retransmites solo lo perdido).
- ¿Q bits insuficientes para W deseada? ⇒ aumenta Q o reduce tamaño de trama para disminuir a (si es posible); con SR debes respetar W ≤ 2^{Q−1}.
- ¿Dudas entre piggybacking y bit stuffing? ⇒ piggybacking = meter ACKs en tramas I; bit stuffing = insertar ‘0’ tras cinco ‘1’ para proteger 0x7E.
- ¿Congestión vs flujo? ⇒ flujo protege al receptor; congestión protege a la red. TCP regula por pérdidas/RTT; ECN permite regular sin perder.

8.9 Unidades, escalas y advertencias
- Mantén SI: bit/s, s, m, W, J, W/Hz, dB (adim).
- dB vs dBm: dB es relación; dBm es potencia absoluta (referida a 1 mW).
- p_b (bit) vs p_frame (trama): ¡no son lo mismo!
- Cuidado con modularidad: ARQ en enlace + TCP arriba pueden interactuar (retransmisión doble si el RTT percibido se infla por ARQ).

Conclusión
- Flujo: dimensiona la ventana con BDP y a; sin W suficiente no saturas el enlace.
- Errores: elige SR si el canal introduce pérdidas esporádicas; usa CRC para detectar y ARQ para recuperar.
- Congestión: TCP se adapta; ayudar con AQM/ECN reduce pérdidas y colas largas.
- HDLC resume y aterriza estos conceptos: ventanas, numeración, piggybacking, bit stuffing y FCS en un mismo protocolo de enlace.
