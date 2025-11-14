# Módulo 1 — Comunicación de Datos (Deep Dive para principiantes)

Objetivo
Entender a fondo cómo medimos la información, cómo la transportan las señales, cómo se digitaliza, qué técnicas básicas usamos para transmitir bits y cuáles son los límites físicos y matemáticos que no podemos superar.

Cómo usar este material
1) Empieza por el Cap. 0 para fijar métrica y vocabulario (throughput, latencia, jitter).
2) Cap. 1 te da la base matemática de “cantidad de información” (entropía, capacidad).
3) Cap. 2 explica la señal en tiempo/frecuencia, decibelios y ruido.
4) Cap. 3 conecta bits con formas de onda (códigos de línea y modulaciones).
5) Cap. 4 describe medios (cobre, fibra, radio) y sus pérdidas.
6) Cap. 5 integra todo con los grandes límites (Nyquist y Shannon) y BER.

Convenciones SI (siempre)
- Tasas: R_b (bit/s), R_s (símbolos/s), B (Hz)
- Tiempo: s (puedes expresar en ms, µs); Distancia: m; Velocidad de propagación v_prop (m/s)
- Potencia: W; Energía por bit E_b (J); Densidad de ruido N_0 (W/Hz)
- Ganancias/pérdidas en dB (adimensional); Probabilidades y eficiencias (adimensional)

Mapa rápido con estilo de preguntas de examen
- QoS de aplicaciones (tiempo de respuesta y jitter) → Cap. 0
- Eficiencia de codificación de fuente (Huffman) → Cap. 1
- Periodicidad de suma de sinusoides y potencia/dBm → Cap. 2
- Ubicación de Manchester en OSI (capa física), piggybacking, etc. → Cap. 3 (y nota de arquitectura)
- Capacidad C con B (desde corte superior) y SNR(dB) → Cap. 5