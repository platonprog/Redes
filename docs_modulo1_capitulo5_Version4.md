# Capítulo 5. Limitaciones de la Transmisión Digital (Versión Extendida)

(Contenido ampliado según demo previa; se reestructura en secciones numeradas.)

## 5.1 Objetivo
Entender límites teóricos (Nyquist, Shannon), eficiencia espectral y relación con E_b/N_0 y BER.

## 5.2 Relaciones fundamentales
R_b = r·R_s (bit/s); r = bits por símbolo.  
B con roll-off β: $B = (R_s/2)(1+β)$ → $R_s = 2B/(1+β)$ → $R_b = r·2B/(1+β)$

## 5.3 ISI y condición de Nyquist
Pulso de Nyquist: cero en múltiplos de T_s excepto en origen → evita solapamiento.  
Raised Cosine: control de β (0 ≤ β ≤ 1). Trade-off: menor β → mayor eficiencia espectral pero mayor longitud temporal.

## 5.4 Capacidad Shannon AWGN
$C = B\log_2(1+SNR)$; $SNR=(E_b/N_0)(R_b/B)$.  
Si $R_b > C$, imposible tasa fiable.

## 5.5 Eficiencia espectral vs robustez
Incrementar M (QAM, PSK) eleva r → eleva η_spec, pero reduce distancia mínima entre puntos → mayor E_b/N_0 requerido para mismos objetivos BER.

Tabla orientativa (BER ~10^-5 sin FEC potente):
| Modulación | r | E_b/N_0 requerido (dB) |
|------------|---|------------------------|
| BPSK/QPSK | 1/2 | 9–10 |
| 16-QAM | 4 | 13–14 |
| 64-QAM | 6 | 18–20 |
| 256-QAM | 8 | 24–26 |

## 5.6 BER formulaciones clave
BPSK/QPSK: $P_b = Q(\sqrt{2E_b/N_0})$  
M-QAM (aprox): $P_b \approx \frac{4}{\log_2 M}(1-\frac{1}{\sqrt{M}}) Q\left(\sqrt{\frac{3\log_2 M}{M-1}\frac{E_b}{N_0}}\right)$

## 5.7 Ejemplos numéricos
1. B=5 MHz, β=0.25, 16-QAM (r=4):
   R_s=8e6/1.25=6.4e6; R_b=25.6 Mb/s; η_spec=25.6/5=5.12 bit/(s·Hz).
2. Necesario E_b/N_0 para R_b=30 Mb/s y B=5 MHz con C≥R_b:
   log₂(1+(E_b/N_0)(30/5)) ≥ 30/5 ⇒ log₂(1+6(E_b/N_0)) ≥ 6 ⇒ 1+6(E_b/N_0) ≥ 64 ⇒ E_b/N_0 ≥ 63/6=10.5 (≈10.2 dB).
3. 64-QAM BER aproximada a E_b/N_0=18 dB (≈63.1 lineal):
   Termo dentro Q: sqrt( (3·6)/(64−1) ·63.1 ) ≈ sqrt(18/63 *63.1) ≈ sqrt(18.03) ≈ 4.246 → Q(4.246)≈1.09·10^-5; factor prefactor ≈ (4/6)(1−1/8)=0.6667 * 0.875=0.5833 → P_b≈0.5833·1.09·10^-5≈6.4·10^-6.

## 5.8 Estrategias de optimización
- Ajustar β para compromiso robustez/η_spec.
- Adaptar M según SNR (AMC).
- FEC para reducir E_b/N_0 requerido (acercarse a Shannon).
- OFDM para reducir ISI en canales con dispersión.

## 5.9 Errores frecuentes
- Usar fórmula de BPSK para 16-QAM sin ajuste.
- Confundir SNR con E_b/N_0 (olvida factor R_b/B).
- Asumir β=0 en cálculos prácticos sin aclararlo.

## 5.10 Glosario
- ISI: Interferencia entre símbolos.
- Roll-off: exceso de banda sobre ideal.
- AMC: Adaptive Modulation and Coding.

## 5.11 Ejercicios
1. Para B=10 MHz, β=0.2, 64-QAM: calcular R_b y η_spec.  
2. Si E_b/N_0=12 dB y esquema 16-QAM, estimar BER aproximada.  
3. Encontrar mínimo E_b/N_0 para R_b=40 Mb/s, B=8 MHz sin exceder C.

(Fin capítulo 5 extendido)