# Capítulo 7. Modelos de Capas

## 7.1 Arquitectura genérica por capas
- Descomposición del problema “aplicación A en host A → aplicación B en host B”.
- Capa de aplicación (mensajes), transporte (segmentación, secuenciación, control de flujo/errores; segmentos), acceso a red/enlace (tramas), e interconexión/Internet (paquetes IP).
- Protocolos: diálogo, sintaxis (cabeceras), semántica.

Funciones clave:
- Segmentación y secuenciación (números de secuencia).
- Control de flujo (buffers en RX) y de errores (detección/corrección + ARQ).
- Encaminamiento local (acceso a red) vs global (Internet).
- Direcciones: locales (enlace) vs globales (IP).

Encapsulado:
- Trama = cabecera_enlace + [paquete IP = cabecera_IP + (segmento = cabecera_transporte + datos_app)] + tráiler_enlace(FCS).
- Relación 1:1: cada trama contiene 1 paquete, cada paquete 1 segmento.

MTU y MSS:
- MTU (byte): máximo campo de datos de la trama (p.ej., Ethernet: 1500 B).
- MSS (byte): máximo campo de datos del segmento TCP.
- Fórmula: MSS = MTU − (IP_header + TCP_header). Típico: 1500 − 20 − 20 = 1460 B (IPv4/TCP sin opciones).

Unidades: bytes (B) y bits (1 B = 8 bit).

## 7.2 Arquitectura TCP/IP (modelo Cisco 4 capas)
- Aplicación (HTTP, DNS, …)
- Transporte (TCP, UDP, SCTP): extremo-a-extremo (host-to-host).
- Internet (IP v4/v6): enrutamiento multi-salto (host–router–…–router–host).
- Acceso a red / Interfaz de red: enlace físico (host-to-network). Suele ir en NIC (hardware).

Observaciones:
- Control de flujo/errores en transporte (end-to-end) y también en enlace (host-to-network).
- Modelos de 5 capas separan “Enlace” (lógico) y “Física” (parámetros físicos: R_b, codificación/modulación, B en Hz, potencia en W, alcance en m).

Sockets:
- Interfaz app–transporte (TCP/UDP). El socket se identifica globalmente por (IP, puerto).
- WebSocket: API eficiente sobre TCP para apps web interactivas.

Routers (estructura):
- Plano de control (CPU, OS de red): algoritmos/protocolos de enrutamiento → genera la tabla de encaminamiento.
- Plano de datos (hardware: line cards, switching fabric): forwarding de paquetes a alta velocidad.
- Buffers de entrada/salida (paquetes), operación store-and-forward.

## 7.3 Arquitectura OSI (comparativa)
- 7 capas: Aplicación, Presentación, Sesión, Transporte, Red, Enlace, Física.
- TCP/IP agrupa (Aplicación=Aplicación+Presentación+Sesión) y (Acceso a red=Enlace+Física), e introduce explícitamente la capa de Internet (multi-red).
- Paquete es la PDU de “Internet/Red”; Trama es PDU de “Acceso a red/Enlace”.

## 7.4 Conmutación de paquetes
- Store-and-forward: cada nodo recibe la PDU entera (t_tx en s), la procesa (τ_proc en s) y reenvía; añade retardo por salto.
- Cut-through: reenvía con solo cabecera; menos usado por errores y colas.
- Multiplexado estadístico: compartir enlaces en FIFO por llegada de paquetes.

Circuito virtual vs datagrama:
- CV: establecimiento–transferencia–terminación; misma ruta, orden preservado (X.25, FR, ATM).
- Datagramas: sin conexión; rutas/orden pueden variar (IP/Internet).

## 7.5 Anatomía de una sesión web
- DNS (resolución de nombre) → 3-way handshake TCP (SYN, SYN-ACK, ACK) → HTTP GET → HTTP 200 OK (+ contenido).
- TCP fragmenta por MSS; el receptor emite ACK (acuses) a nivel TCP.
- Encapsulado en IP y luego en tramas (cabecera/tráiler según red de acceso).
- Unidades prácticas: tiempos en s (ms), tamaños en bytes, tasas en bit/s.

Fórmulas/utilidades:
- t_tx = n_bits / R_b (s); t_prop = d / v_prop (s).
- RTT ≈ 2·t_prop + τ_proc (s).
- Goodput ≈ MSS/(MSS+overhead) · Throughput (bit/s).