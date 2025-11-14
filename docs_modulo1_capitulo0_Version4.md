# Capítulo 0. Modelo de Comunicaciones (Versión Extendida)

## 0.1 Objetivo
Comprender los elementos físicos y funcionales que participan en el proceso de transmitir información desde una fuente hasta un destinatario con la calidad requerida (fiabilidad, latencia, jitter y throughput).

## 0.2 Cadena de comunicación
1. Fuente de información: genera mensaje (texto, voz, datos).
2. Transductor: convierte naturaleza (micrófono: acústica→eléctrica; LED: eléctrica→óptica).
3. Transmisor: adapta el mensaje a una señal según canal (codificación de línea, modulación, nivel de potencia).
4. Medio de transmisión: guiado (par trenzado, fibra) o no guiado (radio, óptico libre).
5. Receptor: procesa señal recibida → detecta símbolos → reconstruye datos.
6. Destinatario: interpreta semántica original.

Flujo de energía e información: la señal transporta ambos; la calidad depende de degradaciones (atenuación, ruido, interferencias, distorsión, errores).

## 0.3 Perturbaciones y efectos
- Atenuación (dB): pérdida de potencia con la distancia y frecuencia.
- Distorsión de amplitud/fase: diferentes componentes espectrales se atenúan/desfasean distinto → ISI.
- Ruido: térmico (proporcional a T), fuga de otros sistemas, interferencia impulsiva.
- Diafonía (crosstalk) en cables multi-par.
- Multicamino (radio): copias retardadas → desvanecimiento selectivo.

## 0.4 Abstracción de capas
Razón: minimiza complejidad. Cada capa ofrece un servicio a la superior y oculta detalles internos (encapsulación). Permite:
- Independencia tecnológica (fibra vs radio sin cambiar capa aplicación).
- Interoperabilidad y escalabilidad.

## 0.5 Conectividad y escalado
Si se intentara conectar n dispositivos “todos con todos”:
Número de enlaces = n(n−1)/2 → coste y número de interfaces crece O(n²).
Solución: nodos de conmutación + direccionamiento → coste O(n).

## 0.6 Métricas de desempeño
- Latencia L (s): tiempo total entre emisión y recepción completa (transmisión + propagación + cola + procesado).
- Jitter J (s): variación estadística de L (desv. típica).
- Throughput T (bit/s): tasa efectiva de bits entregados (incluye overhead si no se especifica goodput).
- Goodput (bit/s): datos útiles sin overhead.
- Fiabilidad: BER (prob. de bit erróneo) o PER (prob. de unidad errónea).
- Disponibilidad: porcentaje de tiempo operativo.

## 0.7 Relación calidad–aplicación
- Streaming interactivo (videollamada): requiere baja L (<150 ms) y bajo J. BER moderada tolerable (compresión con corrección residual).
- Archivo grande (backup): throughput y fiabilidad dominan; latencia puntual irrelevante.
- Trading financiero: latencia extrema crítica; jitter y fiabilidad también.

## 0.8 Ejemplos numéricos
1. Latencia de 1500 B a 100 Mb/s sobre 2000 m fibra (v=2·10⁸ m/s):
   - t_tx = (1500·8)/(100·10⁶) = 0.00012 s = 0.12 ms
   - t_prop = 2000 / 2e8 = 10 µs
   - L ≈ 0.13 ms (sin colas/procesado).
2. Si el mismo paquete viaja por 10 saltos store-and-forward y cada salto añade 50 µs de procesado:
   - Retardo procesado total = 10·50 µs = 0.5 ms
   - L_total ≈ 0.12 + 0.01 + 0.5 ≈ 0.63 ms.
3. Throughput vs goodput:
   - Segmento TCP de 1460 B datos + 40 B cabecera IP+TCP → total 1500 B
   - Eficiencia datos = 1460/1500 ≈ 0.9733 → Goodput ≈ 0.9733·Throughput.

## 0.9 Advertencias típicas (examen)
- Confundir ancho de banda (Hz) con bit rate (bit/s).
- Olvidar incluir overhead en cálculo de tiempo de transmisión.
- Asumir latencia sólo de propagación.

## 0.10 Glosario
- Bit rate: tasa de bits enviados por segundo.
- Propagación: desplazamiento físico de la señal.
- Transductor: conversión de naturaleza física.
- Jitter: variabilidad de retardo.

## 0.11 Ejercicios
1. Calcular latencia de 3000 B a 50 Mb/s sobre 500 m de cable (v=2·10⁸ m/s) sin procesado.  
2. ¿Qué throughput de datos útiles se obtiene si el overhead por PDU es 64 B y se envían 1024 B de datos?  
3. Explica por qué la diafonía aumenta con la frecuencia y la proximidad de pares.

(Fin capítulo 0 extendido)