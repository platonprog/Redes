# Capítulo 0. Modelo de Comunicaciones (Deep Dive)

Idea central (analogía)
Comunicar es como enviar paquetes por un sistema logístico: preparas el paquete (transmisor), lo envías por una “carretera” (medio), sufre retrasos y perturbaciones (latencia, jitter, ruido) y alguien lo desempaqueta (receptor).

Cadena funcional
- Fuente → Transductor → Transmisor → Medio (guiado/no guiado) → Receptor → Transductor → Destino.
- Perturbaciones típicas: atenuación (pérdida de potencia), distorsión (amplitud/fase), ruido (térmico, interferencias), multicamino (radio).

Métricas clave (qué significan, cómo se calculan)
- Throughput T (bit/s): tasa efectiva a la que llegan bits en la capa analizada (p. ej., L2, L3 o fin a fin).
  - Enlace con protocolo de ventana: T = R_b · η (η = eficiencia adimensional, depende del protocolo).
  - Diferéncialo de la tasa nominal del enlace (R_b).
- Goodput (bit/s): tasa de datos útiles de aplicación (excluye cabeceras y retransmisiones).
  - Aproximación útil: Goodput ≈ T · payload_por_PDU / tamaño_total_de_PDU.
- Latencia L (s): tiempo extremo a extremo.
  - L = t_tx + t_prop + t_proc + t_cola.
  - t_tx = n_bits / R_b. t_prop = d / v_prop.
- Jitter J (s): variación de L (p. ej., desviación típica).
- Disponibilidad/fiabilidad: BER (probabilidad de bit erróneo), PER (por unidad), porcentaje de tiempo operativo.

Qué suele pedir el examen (intuición de QoS)
- Aplicaciones interactivas (videollamada, control domótico en tiempo real): exigentes en tiempo de respuesta y jitter; toleran pequeños errores.
- Transferencia de ficheros/backup: prioriza fiabilidad; tolera latencia y jitter.
- Una app de control doméstico sin alarmas: “exigente en respuesta y jitter, moderada en fiabilidad extrema”.

Unidades y órdenes de magnitud
- v_prop: ≈ 2·10^8 m/s en fibra/cobre, ≈ 3·10^8 m/s en aire.
- Enlaces modernos: 100 Mb/s, 1 Gb/s, 10 Gb/s… (ojo: eso es R_b nominal).

Errores de concepto típicos
- Confundir ancho de banda (B, Hz) con bit rate (R_b, bit/s).
- Ignorar overhead al hablar de “velocidad real”.
- Suponer que L ≈ t_prop (en redes de paquetes, colas/procesado importan).

Resumen operativo
- Para comparar tecnologías/protocolos, piensa siempre en las cuatro: T, Goodput, L, J.
- Para preguntas de QoS, “qué importa más” depende de la app. Interactivo ⇒ L y J; batch ⇒ T y fiabilidad.