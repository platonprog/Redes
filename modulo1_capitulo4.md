# Capítulo 4. Medios de Transmisión (Deep Dive)

Medios guiados
- Par trenzado (UTP/STP)
  - Barato y ubicuo; limitaciones por atenuación y diafonía (NEXT/FEXT).
  - Categorías (Cat5e/6/6A/7) especifican anchos máximos y distancias.
- Coaxial
  - Impedancia característica típica 50/75 Ω; buen apantallamiento; menos flexible.
- Fibra óptica
  - Monomodo (SM): núcleo pequeño, larga distancia, baja dispersión modal.
  - Multimodo (MM): más barato para distancias cortas, más dispersión modal.
  - Ventanas típicas: 850/1310/1550 nm; atenuaciones ~0.2–0.35 dB/km (mejor a 1550 nm).

Medios no guiados (radio)
- Modos de propagación: línea de vista (LOS), difracción, reflexión, multicamino.
- Pérdida en espacio libre:
  - $L_{fs}(\mathrm{dB})=20\log_{10}\!\left(\frac{4\pi d}{\lambda}\right)$.
- Enlace básico (Friis, caso ideal LOS):
  - $P_r = P_t G_t G_r \left(\frac{\lambda}{4\pi d}\right)^2$.
- Fading y shadowing: variabilidad rápida (multicamino) y lenta (obstáculos grandes).

Atenuación y unidades
- Lineal: $P_{out}=P_{in}\,e^{-\alpha d}$.
- En dB: $A_{dB}=\alpha_{dB}\cdot d = 10\log_{10}(P_{in}/P_{out})$.
- Conversión potencia ↔ dBm: $P(\mathrm{dBm})=10\log_{10}(P/1\mathrm{mW})$.

Qué suele pedirse
- “Perdida de espacio libre a f, d” → usa L_fs con $\lambda=c/f$ (o $4\pi d f/c$).
- “Atenuación total en fibra/coax” → multiplica coeficiente por distancia; si necesitas potencia de salida, aplica relación lineal.

Errores típicos
- Confundir ganancia (dB) con potencia absoluta (dBm).
- Ignorar acoplos/impedancias (comparar tensiones sin misma R).