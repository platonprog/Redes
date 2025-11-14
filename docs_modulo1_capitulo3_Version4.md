# Capítulo 3. Transmisión Digital (Versión Extendida)

## 3.1 Objetivo
Comprender códigos de línea y modulaciones para mapear bits a formas de onda optimizando sincronismo, eficiencia y BER.

## 3.2 Códigos de línea (banda base)
Características deseadas:
- Reloj embebido (transiciones suficientes).
- Ausencia de componente DC (para acoplo capacitivo).
- Buena eficiencia espectral.
- Simplicidad de implementación.
- Robustez ante errores y diafonía.

Ejemplos:
- NRZ (alto=1, bajo=0): simple pero problema de largas secuencias iguales.
- Manchester: cada bit tiene transición media (sincronismo excelente; eficiencia espectral menor: r efectivo 0.5).
- AMI (Alternate Mark Inversion): ‘1’ alterna polaridad, ‘0’ cero → reduce DC.
- 4B5B / 8B10B: añade bits extra para garantizar densidad de transiciones.
- MLT-3: cicla niveles para reducir ancho de banda (Ethernet 100Base-TX).
- 2B1Q / PAM-M: usa niveles múltiples para codificar más bits por símbolo.

Compromiso: más niveles → menor distancia mínima entre símbolos → mayor BER para mismo SNR.

## 3.3 Modulación paso banda
Mapeo de bits a amplitud/frecuencia/fase:
- ASK, FSK, PSK.
- QAM: combinación cuadratura (I/Q) → constelaciones M-QAM (M=16,64...).
Bits por símbolo ideal: r = log₂ M.

Ejemplo: 64-QAM → r=6; si R_s = 1 Msímb/s → R_b=6 Mb/s.

## 3.4 Sincronización
Separar R_b y R_s: R_b=R_s·r.  
Mayor r (M grande) permite aumentar R_b sin subir R_s (dentro de límites de ISI).  
Necesario recuperación de reloj de la señal recibida (PLL, extrayendo transiciones).

## 3.5 Muestreo y cuantificación (A/D)
$f_s \ge 2B$; cuantificación con n bits → $SNR_q(dB)\approx 6.02n + 1.76$.  
Companding (ley μ/A): mejora relación señal-ruido percibida en señales de voz de baja amplitud.

Ejemplo: n=12 bits → 6.02·12+1.76 ≈ 74 dB teóricos.

## 3.6 Eficiencia espectral
$\eta_{spec}=R_b/B$.  
Códigos que duplican transiciones (Manchester) reducen r efectivo → menor η_spec.

Ejemplo comparativo:
- NRZ: r=1 (suponiendo M=2) → η_spec≈R_s/B (ideal)
- Manchester: cada bit ocupa dos semiperiodos → η_spec se reduce a ~50%.

## 3.7 Ejemplos numéricos
1. 16-QAM (r=4), B=2 MHz, β=0.25 → R_s=2B/(1+β)=4e6/1.25=3.2e6 símbolos/s; R_b=12.8 Mb/s; η_spec=12.8/2=6.4 bit/(s·Hz).
2. NRZ vs Manchester: Si R_b=10 Mb/s con NRZ y B≈R_b/2=5 MHz; con Manchester para mismo R_b se requiere símbolo doble → B efectiva mayor (~10 MHz).
3. Cuantificador: n=8 bits → SNR_q≈6.02*8+1.76=49.92 dB. Para audio de alta fidelidad se requieren ≥16 bits (≈98 dB).

## 3.8 Tabla comparativa (códigos de línea)
| Código | Ventajas | Desventajas | Uso típico |
|--------|----------|-------------|-----------|
| NRZ | Simplicidad, alta eficiencia | Sincronismo pobre con largas secuencias | Antiguo, enlaces simples |
| Manchester | Sincronismo excelente | Mitad eficiencia | Ethernet clásico (10 Mb/s) |
| AMI | Menos DC | ‘0’ largos → sincronismo | T1/E1 |
| 4B5B | Garantiza transiciones | Overhead 25% | Fast Ethernet (100 Mb/s) |
| MLT-3 | Reduce banda | Complejidad | 100Base-TX |
| 8B10B | Balance DC, transiciones | Overhead 25% | Gigabit links serie |
| PAM-5/4 | Más bits por símbolo | Menor distancia entre niveles | Ethernet >100 Mb/s |

## 3.9 Advertencias
- No asumir misma BER para QPSK y 16-QAM con igual SNR.
- Olvidar overhead de codificaciones (4B5B añade 1 bit por cada 4).
- Confundir symbol rate con bit rate cuando M>2.

## 3.10 Glosario
- Constelación: conjunto de puntos I/Q.
- Eye diagram: visualización temporal superpuesta para analizar ISI.
- Companding: compresión de amplitud antes de cuantificar.

## 3.11 Ejercicios
1. Calcular η_spec para 64-QAM si B=5 MHz, β=0.2, R_s=2B/(1+β).  
2. Comparar SNR_q de n=10 vs n=14 bits.  
3. ¿Por qué PAM-M en cobre puede limitar distancia máxima?

(Fin capítulo 3 extendido)