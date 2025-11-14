# Capítulo 6. Introducción a las Redes de Computadores (Deep Dive)

Intuición
Conectar cada par de dispositivos con un cable dedicado crece como n²: inviable. La solución real son redes conmutadas: muchos enlaces punto a punto unidos por nodos (switches/routers) que reenvían unidades de datos. Así, con pocos enlaces por dispositivo, cualquiera puede hablar con cualquiera.

6.1 Qué es una red y cómo las clasificamos
- Red: sistema que interconecta dispositivos dispersos para permitir comunicación entre cualquier par con reglas claras (protocolos).
- Dos grandes familias:
  1) Medio compartido (broadcast físico): la señal llega a todos los nodos del dominio; se necesita un mecanismo de acceso (MAC) para turnarse y evitar colisiones o interferencias lógicas. Ejemplos: bus coaxial clásico, Wi‑Fi.
  2) Redes conmutadas (conmutación de tramas/paquetes): enlaces punto a punto y nodos que reenvían; cada enlace ve solo su propio tráfico. Dominante hoy (Ethernet conmutada, redes IP).
- Por cobertura y medio:
  - Guiadas (cobre/fibra): LAN (edificio/campus), MAN (ciudad), WAN (regional/global).
  - Inalámbricas: WPAN (cm–m), WLAN (10–1000 m), WMAN (km–decenas km), WWAN y satélite (decenas–miles km).
- Topologías lógicas típicas: estrella (switch en el centro), árbol jerárquico, malla (varios caminos), anillo (menos común hoy).

6.2 Métricas de comportamiento (qué significan, cómo pensarlas)
- Throughput T (bit/s): tasa efectiva en la capa observada (no confundir con la tasa nominal R_b).
  - Enlace con protocolo de ventana: T = R_b · η (η es eficiencia sin unidades).
- Goodput (bit/s): tasa de datos útiles de aplicación. Aproximación práctica:
  - Goodput ≈ T · MSS / (MSS + overhead_L4+L3+L2).
- Latencia L (s): tiempo de extremo a extremo. Descomposición:
  - L = t_tx + t_prop + t_proc + t_cola.
  - t_tx = n_bits / R_b; t_prop = d / v_prop; t_proc y t_cola dependen de nodos/colas.
- Jitter J (s): cuánta varía L (p. ej., desviación típica).
- Fiabilidad: BER/PER (probabilidad de error).

Interpretación por tipo de aplicación (intuición de QoS)
- Interactivas y en tiempo (casi) real (voz/vídeo en tiempo real, control domótico en vivo): muy sensibles a tiempo de respuesta y jitter; suele tolerar algunos errores puntuales (compresión/ocultación de pérdidas).
- Transferencia de ficheros, backups: sensibilidad alta a fiabilidad/integridad; latencia más tolerable; busca throughput sostenido.
- Computación distribuida/servicios críticos: todo importa (L, J, T, fiabilidad).

6.3 Direcciones y modos de tráfico
- Unicast (1→1), Multicast (1→grupo), Broadcast (1→todos en dominio lógico).
- Direccionalidad: simplex (una vía), half‑duplex (alterno), full‑duplex (ambas a la vez).
- Direcciones por capa:
  - Enlace (L2): direcciones locales del medio (p. ej., MAC en Ethernet).
  - Red (L3): direcciones globales (IP v4/v6) para enrutamiento multi‑salto.
- No confundir “enlace broadcast” físico (medio compartido) con tráfico broadcast lógico en una red conmutada (se reenvía a todos los puertos del dominio L2, no al mundo entero).

6.4 Bandwidth‑Delay Product (BDP) y dimensionamiento de ventanas
- BDP = R_b · t_prop (bit) = bits “en vuelo” en un enlace.
- Heurística para saturar el enlace (sin errores): la ventana efectiva (en bits) debe cubrir al menos los bits en vuelo más una trama:
  - W_min (en tramas) ≈ (BDP + n_bits_trama) / n_bits_trama.
- A más distancia o R_b (si n_bits fijo), mayor BDP ⇒ ventanas mayores para llenar el “tubo”.

6.5 Store‑and‑Forward vs Cut‑Through (visión de latencia)
- Store‑and‑Forward: el nodo recibe la PDU completa, verifica integridad (p. ej., FCS), decide y reenvía. Más robusto, añade t_tx por salto.
- Cut‑Through: lee cabecera y empieza a reenviar antes de recibir la cola. Menor latencia, menos robusto ante errores de cola y congestión. Típico en switches L2 de muy baja latencia; routers IP suelen ser store‑and‑forward.

6.6 Trampas frecuentes en examen
- Usar “ancho de banda” (Hz) como sinónimo de “velocidad” (bit/s). No lo son: Hz mide espectro; bit/s, tasa de datos.
- Olvidar que el throughput fin‑a‑fin depende del eslabón más lento (cuello de botella) y de la eficiencia del protocolo.
- Suponer que reducir la distancia siempre reduce L notablemente: en redes de paquetes, el retardo de cola/procesado puede dominar.

Resumen operativo
- Para priorizar requisitos, pregúntate: ¿interactivo? → L y J; ¿transferencia? → T y fiabilidad; ¿crítico? → todo.
- Mide “qué es real” (throughput/goodput) y “qué percibe el usuario” (L y J), no solo R_b nominal.