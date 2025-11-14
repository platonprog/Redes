# Capítulo 1. Teoría de la Información (Versión Extendida)

## 1.1 Objetivo
Cuantificar la información, definir límites teóricos de compresión y de transmisión fiable sobre canales ruidosos.

## 1.2 Información individual
Información de suceso con probabilidad p: $I=-\log_2 p$ (bit).  
Implica: sucesos raros aportan más bits; sucesos seguros aportan 0.

Ejemplo:
- p=1/2 → I=1 bit
- p=1/4 → I=2 bits
- p=1/8 → I=3 bits

## 1.3 Entropía de fuente discreta
$H(X)= -\sum_i p_i \log_2 p_i$ (bit/símbolo). Máximo alcanzado en fuente equiprobable (uniforme).  
Propiedades:
- H ≥ 0
- H uniforme = log₂ M
- Distribución sesgada reduce H.

Ejemplo numérico:
Fuente con símbolos A,B,C,D: p=[0.4,0.3,0.2,0.1]
H = −(0.4·−1.3219 + 0.3·−1.7370 + 0.2·−2.3219 + 0.1·−3.3219) ≈ 1.846 bits/símbolo.

## 1.4 Fuente binaria
$H_2(p)=-p\log_2 p-(1-p)\log_2(1-p)$  
Simétrica: H₂(0.2)=H₂(0.8). Máximo 1 bit en p=0.5.

## 1.5 Compresión y límites
Longitud media L̄ de un código prefijo óptimo cumple: $H \le L̄ < H+1$.  
Huffman garantiza proximidad a H; Lempel–Ziv (LZ77/LZ78/LZW) universal: se aproxima a H sin conocer distribución exacta.

Ejemplo comparación:
Fuente anterior (H≈1.846). Si codificamos con longitud media 1.95 bits/símbolo → Redundancia relativa ≈ (1.95−1.846)/1.846 ≈ 5.6%.

## 1.6 Kraft–McMillan
Para código prefijo con longitudes {l_i}: $\sum_i 2^{-l_i} \le 1$.  
Permite validar factibilidad de conjunto de longitudes antes de asignar bitstrings.

Ejemplo:
Longitudes [1,2,3,3]: 2^-1 + 2^-2 + 2^-3 + 2^-3 = 0.5 + 0.25 + 0.125 + 0.125 = 1 → válido.

## 1.7 Información mutua y canal
$I(X;Y)=H(X)-H(X|Y)$ = reducción de incertidumbre en X al conocer Y.  
Capacidad canal discreto: $C=\max_{p(x)} I(X;Y)$.

Ejemplo canal binario simétrico (crossover prob q):
- H(X)=1 (entrada equiprobable)
- H(X|Y)=H₂(q)
- I(X;Y)=1 − H₂(q)
- C = 1 − H₂(q) (bits/símbolo) (máximo porque distribución equiprobable es óptima)

Si q=0.1 → H₂(0.1)≈0.469 ⇒ C≈0.531 bit/símbolo.

## 1.8 Teorema de codificación de canal (Shannon)
Para canal con capacidad C:
- Si R (bit/símbolo) < C, existe código que hace BER → 0 (con longitud de bloque grande).
- Si R > C, BER no puede hacerse arbitrariamente pequeña.

Importancia: orienta diseño de códigos (LDPC, Turbo, Polar) para acercarse a C.

## 1.9 Ejemplos numéricos
1. Fuente binaria p=0.1 → H₂(0.1)≈0.469 bits/símbolo.
2. Canal binario simétrico q=0.2 → C=1 − H₂(0.2)≈1 − 0.722=0.278 bits/símbolo.
3. Si se quiere transmitir R=0.25 bits/símbolo sobre canal q=0.2, es factible (R<C); R=0.3 no (R>C).

## 1.10 Tabla resumen
| Concepto | Fórmula | Unidad |
|----------|---------|--------|
| Información suceso | -log₂ p | bit |
| Entropía | -Σ p_i log₂ p_i | bit/símbolo |
| Fuente binaria | H₂(p) | bit/símbolo |
| Información mutua | H(X)-H(X|Y) | bit |
| Capacidad canal bin. simétrico | 1 - H₂(q) | bit/símbolo |

## 1.11 Advertencias
- No confundir entropía con información de un suceso aislado.
- Igualar siempre R_b (bit/s) = R_s·r; capacidad en bit/s = C_símbolo·R_s.
- “Redundancia” puede referirse a (L̄ − H) o a bits añadidos en codificación de canal (distinguir).

## 1.12 Glosario
- Entropía: promedio de información.
- Capacidad: máximo rendimiento fiable.
- Información mutua: dependencia estadística útil para transmisión.
- Código prefijo: ningún código es prefijo de otro.

## 1.13 Ejercicios
1. Fuente con p=[0.5,0.25,0.125,0.125]. Calcular H.  
2. Canal binario simétrico q=0.05. ¿Cuál es C?  
3. ¿Qué sucede si se intenta transmitir a R=0.9 bits/símbolo en canal q=0.3? Justifica con números.  

(Fin capítulo 1 extendido)