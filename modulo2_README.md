# Módulo 2 — Arquitectura de Redes (Deep Dive para principiantes)

Objetivo
Comprender cómo se organiza una red en capas, cómo se encapsulan y encaminan los datos, y cómo se gobiernan el flujo, los errores y la congestión. También verás un protocolo “clásico” de enlace (HDLC) para aterrizar conceptos de ventana, numeración y tramas.

Cómo usar este material
1) Empieza por el Cap. 6: qué es “una red”, tipos de medios y métricas de rendimiento (throughput, goodput, latencia, jitter) desde la óptica de arquitectura.
2) Cap. 7: modelos por capas (TCP/IP y OSI), encapsulado, MTU/MSS, y cómo realmente se “mueven” los paquetes (switching y routing).
3) Cap. 8: control de flujo (1→1), control de errores (códigos + ARQ), control de congestión (1→N), y HDLC como caso de estudio.

Convenciones SI (siempre)
- Tasas: R_b (bit/s), R_s (símbolos/s), ancho de banda B (Hz)
- Tiempo: s (acepta ms, µs para lectura), Distancia: m, Velocidad de propagación v_prop (m/s)
- Potencia: W; Energía por bit E_b (J); Densidad de ruido N_0 (W/Hz)
- Ganancias/pérdidas: dB (adimensional). Probabilidades, eficiencias, ventanas: adimensional

Puente con el estilo de examen
- QoS de aplicaciones (tiempo de respuesta y jitter) → Cap. 6
- Encapsulado, MTU/MSS, “¿en qué capa va Manchester?” → Cap. 7 (Manchester es capa Física)
- Forwarding en routers modernos (plano de datos HW vs CPU) → Cap. 7
- Piggybacking vs bit stuffing, GBN vs SR, tamaño de ventana y numeración → Cap. 8 (HDLC y ARQ)