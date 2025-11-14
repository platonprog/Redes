# Capítulo 5. Limitaciones de la Transmisión Digital

Relación Rs, Rb, B
- Banda base ideal rectangular: $B\approx R_s/2$ (Hz). 
- $R_b=R_s \cdot r$ (bit/s), con $r$ según código de línea o modulación (en M-aria $r=\log_2 M$ ideal).

Nyquist (sin ISI)
- Paso-bajo ideal: $R_s \le 2B$ (símbolos/s). Con pulsos de Nyquist (raised cosine, roll-off β), $B = \frac{R_s}{2}(1+\beta)$.

Shannon (capacidad AWGN)
- $C=B\log_2(1+SNR)$ [bit/s], con $SNR=P_s/(N_0 B)$.
- En términos de $E_b/N_0$: $SNR=(E_b/N_0)\cdot (R_b/B)$ → $C=B\log_2\!\left(1+(E_b/N_0)\frac{R_b}{B}\right)$.

Eficiencia espectral
- $\eta_{spec}=R_b/B$ [bit/(s·Hz)]. Para alcanzar una BER dada, mayor $\eta_{spec}$ requiere mayor $E_b/N_0$ (menor robustez).

BER (AWGN, coherente)
- BPSK/QPSK: $P_b = Q\!\big(\sqrt{2\,E_b/N_0}\big)$ (adim).
- M-PSK (aprox. alta M): $P_b \approx \frac{2}{\log_2 M} Q\!\left(\sqrt{2\,\frac{E_s}{N_0}}\sin\frac{\pi}{M}\right)$, con $E_s=E_b \log_2 M$.
- Square M-QAM: $P_b \approx \frac{4}{\log_2 M}\!\left(1-\frac{1}{\sqrt{M}}\right) Q\!\left(\sqrt{\frac{3 \log_2 M}{M-1}\frac{E_b}{N_0}}\right)$.
- Nota: Q(x) adimensional, $E_b/N_0$ adimensional (dB en presentación).

Códigos de línea y BER
- La BER efectiva depende del filtrado/pulso y del detector; con pulsos de Nyquist y umbral óptimo, se mapea a la BER del esquema binario equivalente (BPSK en baseband), misma fórmula que BPSK.

Relaciones útiles
- $E_b = P_s/R_b$ (J), $E_s=E_b\log_2 M$ (J).
- Productos banda–retardo y ventanas afectarán throughput pero no los límites de Shannon/Nyquist.

Unidades
- B en Hz, tasas en bit/s o símbolos/s, $E_b$ en J, $N_0$ en W/Hz, probabilidades adimensional, dB para presentación.