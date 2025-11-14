# Capítulo 7. Modelos de Capas y Conmutación (Deep Dive)

7.1 Por qué capas (intuición)
Dividir el problema en capas es como dividir un proyecto en módulos: cada capa ofrece un servicio “limpio” a la superior y oculta su complejidad interna. Esto permite mezclar tecnologías (fibra, radio) sin cambiar la web o el email.

7.2 Modelos de referencia
- TCP/IP (modelo operativo de Internet, 4 capas en la formulación de Cisco):
  1) Aplicación (HTTP, DNS, …)
  2) Transporte (TCP, UDP, SCTP): extremo a extremo (host↔host): puertos, fiabilidad/orden (TCP), no fiabilidad (UDP).
  3) Internet (IP v4/v6): enrutamiento multi‑salto (datagramas).
  4) Acceso a red / Interfaz de red (Enlace + Física): tramas y señales.
- OSI (modelo teórico de 7 capas):
  - Aplicación, Presentación, Sesión, Transporte, Red, Enlace, Física.
  - Correspondencia práctica: TCP/IP “colapsa” Presentación+Sesión dentro de Aplicación y agrupa Enlace+Física como Acceso a red; además, explicita la capa “Internet”.

Nota típica de examen
- La codificación Manchester es Capa Física (codificación de línea es parte eléctrica/temporal de la señal, no L2).

7.3 PDU y encapsulado (del mensaje a la señal)
- Aplicación: mensaje (datos de app)
- Transporte: segmento (cabecera TCP/UDP + datos)
- Internet: paquete IP (cabecera IP + segmento)
- Enlace: trama (cabecera L2 + paquete IP + tráiler L2 con FCS)
- Física: símbolos/ondas en el medio

7.4 MTU y MSS (y por qué importan)
- MTU (bytes): payload máximo de la trama a nivel L2 (p. ej., Ethernet clásica: 1500 B, sin contar cabecera L2/CRC).
- MSS (bytes): payload máximo del segmento TCP (datos de aplicación en TCP).
  - Típico sin opciones en IPv4: MSS ≈ 1500 − 20 (IP) − 20 (TCP) = 1460 B.
  - En IPv6: cabecera de 40 B → MSS típico 1500 − 40 − 20 = 1440 B.
- Fragmentación:
  - IPv4 puede fragmentar en routers (no deseable).
  - IPv6 NO fragmenta en routers; el origen debe ajustar tamaño (PMTUD).
- Eficiencia por trama (idea): eficiencia ≈ MSS / (MSS + overhead_IP+TCP + overhead_L2 si lo cuentas).
- Buenas prácticas: ajustar MSS para evitar fragmentación en el camino.

7.5 Switching y Routing (qué hace realmente la red)
- Switch (L2):
  - Decide por direcciones MAC (locales al enlace). Mantiene tabla de forwarding L2 aprendida por tráfico (MAC learning).
  - Suele soportar VLANs (802.1Q) y puede trabajar en cut‑through o store‑and‑forward.
- Router (L3):
  - Decide por direcciones IP (globales). Resuelve “siguiente salto” y reenvía.
  - En routers modernos, el forwarding lo hace el plano de datos (ASICs/NPUs) usando la FIB; la CPU (plano de control) calcula rutas (OSPF, BGP…), gestiona ARP/ND, ICMP, etc.
  - Nota de examen: “La CPU ejecuta la función de reenvío”: FALSO en routers modernos; el forwarding está en hardware, la CPU gobierna el control.

7.6 Planos, colas y latencia por salto
- Plano de control: protocolos de enrutamiento, resolución de vecinos (ARP/ND), construcción de la FIB.
- Plano de datos: reenvío a velocidad de línea, colas de entrada/salida, planificación (FIFO, WFQ/DRR), AQM (RED/CoDel) para evitar colas largas.
- Retardo por salto (modelo simple): L_salto ≈ t_tx + t_prop + τ_proc + t_cola. En congestión, t_cola domina.

7.7 Conceptos afines que suelen aparecer
- ARP (IPv4)/ND (IPv6): resuelven IP ↔ MAC en un enlace.
- NAT: traducción de direcciones (no es parte de “modelo puro”, pero omnipresente).
- VLAN (802.1Q): separación lógica L2 en el mismo switch (añade etiqueta: +4 B).
- STP/RSTP: evita bucles L2 construyendo un árbol de expansión.

Trampas frecuentes
- Confundir MTU con MSS.
- Olvidar que IPv6 no permite fragmentación en ruta (el origen debe adaptar tamaño).
- Atribuir al router decisiones por “direcciones locales”: los routers encaminan por direcciones de red (IP), no por “direcciones locales” (eso es labor del switch L2).

Resumen operativo
- Encapsulado “en cebolla”: cada capa añade su cabecera (y posible tráiler) y la capa inferior no “entiende” la superior.
- Para eficiencia y fiabilidad: elige bien MSS, evita fragmentación, y recuerda que el plano de datos es hardware especializado.
```

````markdown name=docs/modulo2/capitulo8.md
# Capítulo 8. Control de Flujo, Errores y Congestión + HDLC (Deep Dive)

Panorama
- Control de flujo: 1→1. Evita desbordar el receptor (ajusta cuántas tramas/segmentos pueden ir “en vuelo”).
- Control de errores: detectar/corregir errores debidos al canal (códigos + ARQ).
- Control de congestión: 1→N. Evita que la red (nodos/intermedios) colapse cuando hay exceso de tráfico.
- HDLC: protocolo de enlace clásico “orientado a bit” que materializa ventanas, numeración y tramas (útil para practicar SR/GBN, P/F, piggybacking y bit stuffing).

8.1 Parámetros básicos y magnitudes (para todo el capítulo)
- R_b (bit/s), n_bits (bit por trama), d (m), v_prop (m/s).
- t_tx = n_bits / R_b (s), t_prop = d / v_prop (s), $a = t_{prop} / t_{tx}$ (adimensional).
- Throughput T = R_b · η (η = eficiencia adimensional).
- p_b: prob. de error de bit; con errores independientes:
  - $p_{frame} = 1 - (1 - p_b)^{n_{bits}}$ (prob. de que una trama salga mal).

8.2 Control de flujo (capa de enlace, análogo en transporte)
- Stop‑and‑Wait (SW):
  - Idea: envío 1 trama y espero ACK; simple pero ineficiente a largas distancias.
  - Sin errores: $\eta_{SW} = \frac{1}{1 + 2a}$.
  - Con errores: $\eta_{SW} \approx \frac{1 - p_{frame}}{1 + 2a}$ (aprox. con ACK “perfectos”).
- Ventana deslizante (Sliding Window), tamaño máximo W (en tramas):
  - Permite hasta W tramas no confirmadas en vuelo.
  - Sin errores:
    - Si $W < 2a + 1$: $\eta = \frac{W}{1 + 2a}$.
    - Si $W \ge 2a + 1$: $\eta \to 1$ (saturas el enlace).
  - Numeración con Q bits:
    - go‑back‑N (GBN): $W \le 2^{Q} - 1$.
    - selective‑repeat (SR): $W \le 2^{Q-1}$ (evita ambigüedad de ACKs atrasados).
- Lectura práctica:
  - A mayor distancia o mayor R_b (si el tamaño de trama no cambia), aumenta a ⇒ necesitas mayor W para mantener η alta.
  - SR suele ser más eficiente que GBN para el mismo Q cuando hay errores, porque retransmite solo lo que falla.

8.3 Control de errores (códigos y ARQ)
- Códigos de detección/corrección:
  - CRC: división polinómica módulo‑2 con generador G(x) (grado r). Excelente detección (fallo ≈ $2^{-r}$ para errores aleatorios) y muy buena en ráfagas. Campo FCS habitual: 16/32 bit.
  - Checksums: suma complemento a 1 (16/32 bit). Menos potente que CRC, muy barato de calcular.
  - Códigos bloque (n,k): tasa $R_c = k/n$; distancia mínima $d_{min}$ determina detección/corrección.
- ARQ (retransmisión):
  - go‑back‑N (GBN): ante pérdida de una trama i, retransmite i y todas las posteriores no confirmadas.
    - Eficiencia típica con errores y $W \ge 2a + 1$:
      - $\eta_{GBN} \approx \frac{1 - p_{frame}}{1 + 2a \cdot p_{frame}}$.
  - selective‑repeat (SR): solo retransmite las tramas perdidas; requiere buffers y numeración “segura” (W ≤ 2^{Q-1}).
    - Con $W \ge 2a + 1$ y errores moderados: $\eta_{SR} \approx 1 - p_{frame}$ (mejor que GBN).
- Timeouts:
  - Deben superar el RTT estimado con margen: $T_{timeout} > \alpha \cdot RTT$ (α>1). Si es muy corto, provoca retransmisiones espurias; si es muy largo, reduce reactividad.

8.4 Control de congestión (visión TCP/IP y en la red)
- Diferencia clave:
  - Flujo (1→1): regula el emisor para no desbordar al receptor.
  - Congestión (1→N): ajusta las fuentes cuando la red muestra signos de saturación (colas largas, pérdidas).
- Señalización:
  - Implícita (TCP “clásico”): infiere congestión por pérdidas o crecimiento de RTT.
  - Explícita: ECN (IP/TCP), marcas en cabecera en lugar de perder paquetes; en tecnologías antiguas: FECN/BECN (FR/ATM).
- Algoritmos TCP (muy resumidos):
  - Slow Start: arranca con cwnd pequeño y duplica por RTT (exponencial) hasta ssthresh.
  - Congestion Avoidance: incremento aditivo (~1 MSS por RTT).
  - Reacción a pérdidas: multiplicative decrease (cwnd/2), fast retransmit/recovery (según variante).
  - Variantes: Reno/NewReno, CUBIC (Linux por defecto en muchos sistemas), BBR (control de tasa por modelado de entrega).
- En nodos intermedios:
  - AQM (RED, CoDel): gestionan colas para evitar latencias enormes y señalizan congestión antes de desbordar.

8.5 HDLC (caso de estudio de enlace orientado a bit)
- Formato de trama:
  - Flag (8 bit, 0x7E) | Address (8 bit, extensible) | Control (8/16 bit) | Information (variable) | FCS (16/32 bit) | Flag.
- Tipos de trama:
  - I (Information): llevan datos y numeración N(S) (send) y N(R) (receive) para ACK por piggybacking.
  - S (Supervision): RR (Ready to Receive), RNR (Receiver Not Ready), REJ (GBN), SREJ (Selective Reject para SR).
  - U (Unnumbered): gestión (SABM/SABME establecimiento, UA confirmación, DISC desconexión, XID, TEST…).
- P/F (Poll/Final):
  - Bit en cabecera que, según sentido y rol, “solicita respuesta” (Poll) o “marca fin” (Final).
- Bit stuffing (muy importante diferenciar):
  - Técnica para que la secuencia de bandera 0x7E (01111110) no aparezca dentro de los datos: tras cinco ‘1’ seguidos, se inserta un ‘0’. ¡No es lo mismo que piggybacking!
- Piggybacking:
  - Insertar ACKs (N(R)) en tramas I que viajan en el sentido opuesto, para no gastar tramas S dedicadas.
- Ventanas y numeración:
  - Igual que en ARQ: GBN/SR se implementan con límites de W según los bits de numeración Q (véase 8.2).

8.6 Cómo razonar escenarios tipo “enlace entre dos nodos” (p. ej., drones)
- Elegir mecanismo de errores:
  - Si errores son raros pero existen, SR ahorra retransmisiones frente a GBN (mejor η para el mismo Q).
  - Si el camino tiene RTT grande (a grande), conviene W ≥ 2a+1 para saturar; si Q no da, la eficiencia cae.
- Calcular throughput y goodput bajo saturación:
  - Con $W \ge 2a + 1$ y errores moderados:
    - SR: $\eta \approx 1 - p_{frame}$; $T \approx R_b \cdot \eta$.
    - GBN: usar $\eta_{GBN}$ de arriba (peor si p_frame crece).
  - Goodput = T · (payload útil por trama / tamaño total de trama).
  - Recuerda incluir la sobrecarga por cabeceras y tráiler (p. ej., 14 B L2 según estándar del enunciado).
- Secuenciación y Q bits:
  - Para mejorar aún más la eficiencia, aumenta Q para permitir $W \ge 2a + 1$ sin ambigüedades:
    - GBN: $W \le 2^Q - 1$; SR: $W \le 2^{Q-1}$.

Trampas frecuentes de examen
- Confundir piggybacking con bit stuffing: piggybacking es “meter ACKs en tramas I”; bit stuffing es insertar ‘0’ tras cinco ‘1’ para proteger la bandera.
- Elegir $W > 2^{Q-1}$ en SR: crea ambigüedad (ACK de trama antigua puede parecer de una nueva).
- Olvidar que $p_{frame}$ crece con el tamaño de trama: a mismo p_b, tramas grandes tienen más probabilidad de error.
- Suponer que el reenvío lo hace la CPU en routers modernos (no: lo hace el plano de datos en hardware).

Resumen operativo
- Flujo: dimensiona W con a; si a es grande, SW será muy ineficiente; usa ventana amplia.
- Errores: si hay pérdidas esporádicas, SR ≈ (1 − p_frame); GBN empeora con p_frame·a.
- Congestión: TCP regula de forma distribuida; ECN permite “marcar” en vez de perder.
- HDLC: recuerda el formato de trama, P/F, piggybacking y el porqué del bit stuffing.