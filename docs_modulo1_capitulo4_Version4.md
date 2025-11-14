# Capítulo 4. Medios de Transmisión (Versión Extendida)

## 4.1 Objetivo
Caracterizar medios físicos guiados y no guiados, sus limitaciones y parámetros de diseño.

## 4.2 Medios guiados
Par trenzado:
- Categorías (Cat5e, Cat6, Cat6A, Cat7...).  
- Atenuación aumenta con f y longitud.  
- Diafonía (NEXT, FEXT) mitigada por trenzado.

Coaxial:
- Impedancia característica (50 Ω data / 75 Ω video).
- Mejor apantallamiento, mayor banda que par trenzado.

Fibra óptica:
- Monomodo (SM): núcleo pequeño, baja dispersión modal → largas distancias.
- Multimodo (MM): núcleo mayor, más barato, dispersión modal → distancias cortas.
- Ventanas: 850 nm, 1310 nm, 1550 nm (mínimos de atenuación).
- Atenuación típica: 0.2–0.35 dB/km (1550 nm mejor).

## 4.3 Atenuación y pérdidas
Atenuación lineal: $P_{out}=P_{in}e^{-\alpha d}$  
En dB: $A_{dB} = 10\log_{10}(P_{in}/P_{out}) = \alpha_{dB} d$  
Ejemplo fibra: α=0.25 dB/km, d=40 km → A=10 dB (aprox).

## 4.4 Medio no guiado (radio)
Propagación:
- Espacio libre: $L_{fs}(dB)=20\log_{10}(4\pi d/\lambda)$
- Shadowing (log-normal): fluctuaciones lentas.
- Fading rápido: variaciones en ms por multicamino.

Friis (potencias):
$P_r = P_t G_t G_r (\lambda/4\pi d)^2$ (ideal LOS sin obstáculos).

## 4.5 Interferencias y mitigación
- Selección de frecuencia (evitar bandas congestionadas).
- Filtrado selectivo y diversidad (espacial/frecuencia/tiempo).
- MIMO: múltiples antenas para multiplexar espacialmente y combatir fading.

## 4.6 Satélites
Órbitas:
- LEO (bajo retardo ~5–30 ms por salto; muchos satélites).
- MEO (retardos medios).
- GEO (retardo ~250 ms ida, ~500 ms RTT; cobertura amplia).

Retardo geopol: ~36,000 km → t_prop (un salto) ≈ 36,000,000 m / 3e8 ≈ 120 ms.

## 4.7 Ejemplos numéricos
1. Pérdida espacio libre a 2.4 GHz, d=100 m:
λ = c/f ≈ 0.125 m → L_fs=20 log10(4π*100/0.125) ≈ 20 log10(10053) ≈ 80 dB.
2. Fibra 80 km, α=0.22 dB/km → A=17.6 dB.
3. Enlace coaxial 500 m con α=0.05 dB/m → A=25 dB.

## 4.8 Tabla comparativa
| Medio | Ventajas | Desventajas | Uso |
|-------|----------|-------------|-----|
| Par trenzado | Bajo coste | Atenuación & diafonía | LAN cobre |
| Coaxial | Mejor apantallamiento | Menos flexible | Backbone antiguo / CATV |
| Fibra SM | Larga distancia | Costo transceptores | Core / larga distancia |
| Fibra MM | Barato en distancias cortas | Dispersión modal | Datacenters cortos |
| Radio (WiFi) | Movilidad | Interferencias | Acceso local |
| Satélite GEO | Cobertura global | Alta latencia | Broadcast TV, datos remotos |

## 4.9 Advertencias
- Pensar que más potencia siempre soluciona: saturación/no linealidad/regulaciones.
- No considerar dispersión cromática en fibra (afecta pulsos cortos).
- Ignorar fading → sobreestimar enlace radio.

## 4.10 Glosario
- Diafonía: interferencia entre pares próximos.
- Fading: variación rápida de amplitud por multicamino.
- Shadowing: variación lenta por obstáculos grandes.

## 4.11 Ejercicios
1. Calcular L_fs a 5 GHz y d=2 km.  
2. Fibra 120 km con α=0.25 dB/km: ¿cuánta amplificación para recuperar 0 dB referencia?  
3. Explica la ventaja de diversidad de frecuencia frente a fading selectivo.

(Fin capítulo 4 extendido)