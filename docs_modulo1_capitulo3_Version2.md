# Capítulo 3. Transmisión Digital

Banda base vs modulada
- Banda base: códigos de línea (NRZ, RZ, Manchester, AMI, B8ZS/HDB3, MLT-3, 4B5B, 8B10B, 2B1Q, PAM-M).
- Modulación: ASK/FSK/PSK/QAM; M-aria (QPSK=M=4, M-PSK, M-QAM). Constellations (I/Q).

Requisitos de códigos de línea
- Sincronismo, ausencia de DC, baja energía a bajas f, baja BER, buena eficiencia espectral $\eta_{spec}$ [bit/(s·Hz)], simplicidad.

A/D y cuantificación
- Muestreo: $f_s \ge 2B$ (Hz). Cuantificación → ruido: $SNR_q(dB)\approx 6.02n+1.76$ para cuantificador uniforme (n bits).
- Companding (Ley A/μ): mejora error relativo en niveles bajos.

Tasas
- Vel. de símbolos $R_s=1/T_s$ (símbolos/s).
- Bits por símbolo r: $R_b=R_s \cdot r$ (bit/s); en M-aria ideal $r=\log_2 M$ (bit/símbolo).
- Eficiencia espectral: $\eta_{spec}=R_b/B$ [bit/(s·Hz)].