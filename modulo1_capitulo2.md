# Capítulo 2. Teoría de Señal (Deep Dive)

Qué es “señal”
Variación temporal de una magnitud física (voltaje, luz, presión) que transporta información y energía.

Sinusoidal de referencia
- $s(t)=A\cos(2\pi f t + \varphi)$
  - A (V), f (Hz), $\varphi$ (rad), periodo $T=1/f$, longitud de onda $\lambda = v/f$ (m).
- Potencia media de una sinusoidal pura en resistencia R:
  - $V_{\text{rms}} = A/\sqrt{2}$; $P = V_{\text{rms}}^2 / R = A^2/(2R)$ [W].

Suma de sinusoides: periodicidad y potencia
- Periodicidad:
  - La suma es periódica si la razón de frecuencias es racional (sus periodos son conmensurables).
  - Periodo fundamental $T_0=\operatorname{mcm}(T_1,T_2,\dots)$ (en tiempo).
  - Cuidado con unidades: si el enunciado usa t en ms, una expresión $\sin(2\pi\cdot 14 \cdot t)$ significa f = 14 kHz (porque 1/ms = kHz).
- Potencia:
  - Para sinusoides de diferente frecuencia, la potencia media total es la suma de potencias de cada componente (ortogonalidad).
  - Si $v(t)=\sum_k A_k \sin(\dots)$ sobre R, entonces $P=\sum_k A_k^2/(2R)$.
  - Pasar a dBm: $P(\mathrm{dBm})=10\log_{10}(P/1\mathrm{mW})$.

Representación espectral y PSD
- Transformada de Fourier: descompone señal en frecuencias.
- PSD S(f) [W/Hz]: potencia por Hz; ruido térmico blanco ideal: PSD plana con densidad $N_0$.
- Potencia de ruido en banda B (pasa-bajo): $P_n=N_0 B$ → SNR $=P_s/P_n$.

Decibelios (dB)
- Relación de potencias: $G_{dB}=10\log_{10}(P_2/P_1)$.
- Relación de tensiones sobre misma R: $20\log_{10}(V_2/V_1)$.

Muestreo y aliasing
- Teorema de muestreo: $f_s \ge 2B$ (muestras por segundo).
- Si $f_s<2B$ aparece “aliasing”: componentes altas se pliegan a bajas.

Qué suele aparecer en examen (ejemplo conceptual)
- “¿Es periódica la suma?” → verifica unidades de t, deduce frecuencias, toma mcm de periodos.
- “Potencia en 50 Ω y dBm” → calcula potencias de cada seno y suma; convierte a dBm.

Errores típicos
- Olvidar convertir ms ↔ s (o considerar 14 como Hz en lugar de 14 kHz si t está en ms).
- Sumar tensiones RMS de sinusoides de diferente f (no procede); se suman potencias medias.
- Usar 10 log en tensiones en lugar de 20 log (cuando comparas amplitudes con misma R).