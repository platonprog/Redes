# Capítulo 5. Limitaciones de la Transmisión Digital (Deep Dive)

Panorama
Dos límites gobiernan lo que “cabe” por un canal: 
- Nyquist: evita interferencia entre símbolos (ISI) con pulsos adecuados; fija relación $R_s$–$B$.
- Shannon: capacidad máxima con ruido para una BER arbitrariamente baja, si usas codificación ideal.

Relaciones básicas
- Bits por símbolo ideales (sin codificación extra): $r=\log_2 M$ (M-aria).
- Tasa de símbolos y ancho de banda (pulsos de Nyquist con roll-off $\beta$):
  - $B = \frac{R_s}{2}(1+\beta)$ ⇒ $R_s=\frac{2B}{1+\beta}$.
- Tasa de bits: $R_b=R_s \cdot r$.
- Eficiencia espectral: $\eta_{spec}=R_b/B$ [bit/(s·Hz)].

Límite de Nyquist (sin ISI)
- Idea: usar pulsos cuya suma en los instantes de muestreo anule vecinos (pulsos de Nyquist).
- Raised Cosine (RC/RRC) controla excesos de banda con $\beta\in[0,1]$:
  - Menor $\beta$ ⇒ mayor eficiencia espectral pero pulsos más “largos” en tiempo (más sensibles a desalineaciones temporales).

Límite de Shannon (AWGN)
- Capacidad: $C = B \log_2(1 + \mathrm{SNR})$ [bit/s], con $\mathrm{SNR}=P_s/P_n$.
- Relación con $E_b/N_0$:
  - $P_n=N_0 B$, $P_s=E_b R_b$ ⇒ $\mathrm{SNR}=\frac{E_b}{N_0}\frac{R_b}{B}$.
- Lectura práctica:
  - Si piden capacidad y te dan “frecuencia de corte superior de un pasa-bajo en banda base” $f_{c,\text{sup}}$: para un canal ideal pasa-bajo, toma $B \approx f_{c,\text{sup}}$ (Hz) como ancho de banda útil (ojo a cómo lo define el enunciado).
  - Si te dan SNR en dB: $\mathrm{SNR}=10^{(\mathrm{SNR}_{dB}/10)}$.

BER (visión rápida para relacionar robustez)
- BPSK/QPSK coherente: $P_b = Q\big(\sqrt{2 E_b/N_0}\big)$.
- M-QAM (cuadrada, aproximada): 
  - $P_b \approx \frac{4}{\log_2 M}\!\left(1-\frac{1}{\sqrt{M}}\right) Q\!\left(\sqrt{\frac{3\log_2 M}{M-1}\frac{E_b}{N_0}}\,\right)$.
- Moraleja: constelaciones más densas (mayor M) necesitan mayor $E_b/N_0$ para misma BER.

Cómo razonar preguntas típicas
- “Capacidad con $f_{c,\text{sup}}$ y SNR(dB)”:
  1) Pon $B=f_{c,\text{sup}}$ (si el canal es pasa-bajo de banda base y así lo define el enunciado).
  2) Convierte SNR(dB) a lineal: $10^{(\mathrm{dB}/10)}$.
  3) $C=B\log_2(1+\mathrm{SNR})$.
  4) Cuidado con unidades: devuelve en bit/s y, si el test da Kbps, divide entre 1000 (si piden k=10^3).
- “¿Cabe R_b?”:
  - Calcula C con tu B y SNR; si $R_b\le C$, teóricamente es posible con codificación potente.
- “Eficiencia espectral deseada”:
  - $\eta_{spec}=R_b/B$; mayor η suele exigir mayor $E_b/N_0$ para la misma BER (menos robusto).

Errores típicos
- Usar $B=2f_{c,\text{sup}}$ en un canal definido ya como pasa-bajo en banda base (si el enunciado define $f_{c,\text{sup}}$ como ancho de banda unilátero, B≃f_c,sup).
- Mezclar dB (relaciones) con dBm (potencias absolutas).
- Olvidar que $E_b/N_0$ y SNR se relacionan por $R_b/B$.