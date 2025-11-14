# Capítulo 6. Introducción a las Redes de Computadores

## 6.1 Red de computadores: definición y tipología
- Red: sistema que interconecta computadores dispersos para permitir comunicación entre cualquier par.
- “Todos con todos”: enlaces = n(n−1)/2 (adimensional) → inviable por coste e interfaces.
- Dos grandes categorías:
  1) Conmutadas (estructura mallada: nodos de conmutación + enlaces). Enlaces de acceso (dedicados) vs. intermedios (multiplexados).
  2) Medio compartido (señal llega a todos; requiere arbitraje de acceso).

Topologías sobre medio compartido:
- Bus (coaxial; obsoleta), Anillo (par trenzado/fibra; declive), Estrella/Árbol con hub (declive) o con conmutador (esto ya es red conmutada),
- Inalámbrica (celular: celdas con punto de acceso/estación base/satélite + backbone).

Escala/cobertura (típica):
- Guiadas: LAN (edificio/campus), MAN (ciudad), WAN (regional/continental).
- Inalámbricas (radio de celda): WPAN (≈0.1–10 m), WLAN (≈10 m–1 km), WMAN (≈1–50 km), WWAN (>50 km), planetaria (satélite/internetworking).

Notas:
- Enlaces intermedios agregan tráfico → requieren mayor capacidad (bit/s) que los de acceso.
- Ethernet conmutada domina en guiadas desde LAN hasta WAN.
- Cobertura vs velocidad (tendencia inversa por C = B·log2(1+SNR)), mitigada con diversidad, MIMO, modulación avanzada.

## 6.2 Requisitos de diseño
Métricas de comportamiento (usar unidades SI):
- Throughput T: bit/s.
- Goodput: bit/s (solo datos útiles).
- Latencia L: s; se descompone en transmisión (n_bits/R_b), propagación (d/v_prop), procesado (s), espera (cola).
- Jitter J: s (desviación típica de L).

Otros ejes:
- Escalabilidad: crecer en usuarios/cobertura manteniendo T/L/J aceptables.
- Fiabilidad: operación ante fallos (control de flujo/errores/congestión, enrutamiento robusto).
- Seguridad: confidencialidad, integridad, autenticación.

Requisitos de QoS por aplicación (tendencias):
- Transferencia de ficheros: fiabilidad alta; tolera L/J.
- Voz/Vídeo interactivo: sensibles a L/J; toleran algo de errores.
- Computación distribuida: exigente en todo.

## 6.3 Caracterización de conexiones
Tipos de enlace:
- Punto a punto (un TX–RX) vs. Broadcast (medio compartido).
- Inalámbricas: enlace ascendente (uplink) y descendente (downlink).

Direccionalidad:
- Simplex, Half-duplex, Full-duplex. Velocidad de transferencia (bit/s) en full-duplex es suma de ambas direcciones.

Modo de direccionamiento:
- Unicast, Multicast, Broadcast (lógico). No confundir con “enlace broadcast” físico.

Unidades SI clave
- R_b (bit/s), d (m), v_prop (m/s), L y J (s), T (bit/s).