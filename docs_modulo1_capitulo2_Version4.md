# Capítulo 2. Teoría de Señal (Versión Extendida)

## 2.1 Objetivo
Modelar señales como funciones del tiempo/frecuencia para analizar transmisión, pérdidas y ruido.

## 2.2 Señales básicas
Sinusoidal: $s(t)=A\cos(2\pi f t + \varphi)$  
Parámetros: A (amplitud), f (Hz), φ (rad), periodo T=1/f, λ=v/f.  
Frecuencia angular: ω=2πf.

## 2.3 Representaciones
- Temporal: variación instantánea.
- Espectral: distribución de energía/potencia en frecuencias (Fourier).

Transformada de Fourier (aperiódica):
$S(f)=\int_{-\infty}^{\infty} s(t) e^{-j2\pi f t} dt$

Serie de Fourier (periódica): suma de armónicos kf₀.

## 2.4 Potencia y energía
Energía señal finita: $E=\int |s(t)|^2 dt$ (Joule).  
Potencia media periódica: $P=(1/T)\int_0^T |s(t)|^2 dt$ (W).  
Relación dB: $G_{dB}=10\log_{10}(P_2/P_1)$.

## 2.5 Densidad espectral de potencia (PSD)
$S_s(f)$ (W/Hz): facilita análisis de ancho de banda ocupado y ruido añadido.  
Ruido térmico blanco: PSD constante N₀/2 por rama (real), total N₀.

## 2.6 Relación señal–ruido (SNR)
$SNR=P_s/P_n$; en banda limitada: $P_n=N_0 B$ → $SNR=P_s/(N_0 B)$.  
En dB: $SNR_{dB}=10\log_{10}(SNR)$.

## 2.7 Filtrado y distorsión
Filtro pasa-bajo ideal no realizable (respuesta sinc → infinito).  
Filtros realizables (Butterworth, Chebyshev, RC) introducen fase no lineal → distorsión de grupo.  
Distorsión de retardo: tiempo de grupo variable → ISI en banda base.

## 2.8 Aliasing y muestreo
Muestreo ideal $f_s \ge 2B$ (Teorema de Nyquist).  
Si $f_s < 2B$, frecuencias altas se pliegan (aliasing).  
Solución: filtrado antialias antes de muestrear.

## 2.9 Ejemplos numéricos
1. Ruido en B=10 MHz, N₀=10^-20 W/Hz:
   P_n = N_0 B = 10^-20 · 10^7 = 10^-13 W
2. Si P_s=10^-9 W → SNR=10^-9 /10^-13=10^4=40 dB.
3. Disminuye B a 5 MHz manteniendo potencia y N₀ → P_n=5·10^-14, SNR=20000 ≈ 43 dB (menos ruido al reducir banda).

## 2.10 Tabla comparativa
| Concepto | Símbolo | Unidad |
|----------|---------|--------|
| Potencia media | P | W |
| PSD señal | S_s(f) | W/Hz |
| PSD ruido rama | N₀/2 | W/Hz |
| SNR lineal | P_s/P_n | adim |
| SNR dB | 10 log10(SNR) | dB |

## 2.11 Advertencias
- Confundir B (Hz) con R_b (bit/s).
- Olvidar factor B al pasar de E_b/N_0 a SNR: $SNR=(E_b/N_0)(R_b/B)$.
- Considerar el ruido siempre “blanco” aunque canal real pueda introducir coloración.

## 2.12 Glosario
- PSD: distribución de potencia por Hz.
- Aliasing: plegado de espectro por muestreo insuficiente.
- Filtro antialias: limita banda antes de muestrear.

## 2.13 Ejercicios
1. Calcular SNR si P_s=5·10^-8 W, B=2 MHz, N_0=2·10^-20 W/Hz.  
2. ¿Por qué la fase lineal del filtro es importante para evitar distorsión de grupo?  
3. Diseña un criterio para elegir f_s dado un ancho de banda de entrada variable entre 3 y 4 MHz.

(Fin capítulo 2 extendido)