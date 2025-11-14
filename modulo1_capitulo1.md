# Capítulo 1. Teoría de la Información (Deep Dive)

Intuición
La información es “sorpresa”: cuanto menos probable es un suceso, más información aporta. La entropía mide la sorpresa media de una fuente. La capacidad dice cuánta información por segundo podemos transportar (con ruido) sin errores arbitrariamente grandes si codificamos bien.

Definiciones fundamentales
- Información de un suceso p: $I=-\log_2 p$ [bit].
- Entropía de una fuente discreta: $H=-\sum_i p_i \log_2 p_i$ [bit/símbolo].
  - Máxima cuando todos los símbolos son equiprobables: $H_{\max}=\log_2 M$ (M símbolos).
- Fuente binaria: $H_2(p)=-p\log_2 p-(1-p)\log_2(1-p)$.
- Información mutua: $I(X;Y)=H(X)-H(X|Y)=H(Y)-H(Y|X)$ [bit]; “lo que Y nos dice de X”.
- Capacidad de canal (símbolo a símbolo): $C=\max_{p(x)} I(X;Y)$ [bit/símbolo]; si multiplicas por símbolos/s obtienes bit/s.

Compresión (codificación de fuente)
- Límite: $H \le \bar L < H+1$ (longitud media de código por símbolo).
- Códigos prefijo (instantáneos): Kraft–McMillan $\sum 2^{-l_i} \le 1$.
- Huffman: óptimo escalar (sin memoria) para p_i conocidos.
  - Eficiencia típica de examen: Eficiencia = $H/\bar L$ (o su inversa “redundancia”).

Canales discretos con ruido (visión)
- Canal binario simétrico (BSC, prob. de cruce q): $C = 1 - H_2(q)$ [bit/símbolo].
- Teorema de Shannon (codificación de canal): si la tasa R < C, existen códigos que hacen BER → 0 para bloques suficientemente grandes (no dice cómo encontrarlos, solo que existen).

Cómo leer/contestar preguntas clásicas
- “Eficiencia de Huffman” con p dadas:
  1) Calcula H.
  2) Construye Huffman (o te dan $\bar L$). 
  3) Eficiencia = H / $\bar L$. Redondea como pidan.
- “¿Se puede transmitir a R?” en un BSC con q:
  - Calcula C = 1 − H₂(q). Si R ≤ C, teóricamente sí (con codificación potente); si R > C, no.

Notas de unidades y práctica
- H y C “por símbolo” → para bit/s multiplícalos por símbolos/s.
- La codificación de fuente (Huffman) reduce tamaño medio; la de canal añade redundancia para combatir errores (objetivos distintos).

Errores típicos
- Confundir I (suceso) con H (promedio de la fuente).
- Mezclar compresión (fuente) con protección contra errores (canal).