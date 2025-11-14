# Capítulo 1. Teoría de la Información

Medida de información
- Información de suceso de probabilidad p: $I=-\log_2 p$ [bit].
- Fuente equiprobable M símbolos: $I=\log_2 M$ [bit/símbolo].

Entropía
- $H=-\sum_i p_i \log_2 p_i$ [bit/símbolo]; $H_{\max}=\log_2 M$.
- Binaria: $H_2(p)=-p\log_2 p-(1-p)\log_2(1-p)$ ≤ 1 bit.

Codificación de fuente
- Límite de Shannon: $H \le \bar L < H+\epsilon$ (bits por símbolo codificado).
- Kraft–McMillan: $\sum 2^{-l_i} \le 1$ (códigos prefijo/unívocamente descifrables).
- Huffman (óptimo escalar si sin memoria); Lempel–Ziv (universal, diccionario).

Canales discretos
- Información mutua: $I(X;Y)=H(X)-H(X|Y)=H(Y)-H(Y|X)$ [bit].
- Capacidad: $C=\max_{p(x)} I(X;Y)$ [bit/símbolo]; con símbolos por segundo → bit/s.
- Codificación de canal (idea): añadir redundancia para combatir errores.

Unidades
- Tasas (capacidad/bit rate): bit/s; probabilidades adimensional.