# Movimiento browniano y pinzas ópticas — Laboratorio 5

Código de análisis del trabajo de pinzas ópticas de Laboratorio 5 (cátedra Larotonda),
Departamento de Física, FCEyN, Universidad de Buenos Aires.

**Autoras:** Anna Benedetti, Milagros Paseri y María Dolores Sirera.

Estudiamos el movimiento browniano de microesferas de látex en una mezcla de agua y
glicerina y caracterizamos una pinza óptica mediante equipartición y fuerza de arrastre.
Las posiciones de las partículas se obtienen de videos del microscopio con `trackpy`.

## Contenido

| Notebook | Qué hace | Resultado principal |
|---|---|---|
| [`01_movimiento_browniano.ipynb`](notebooks/01_movimiento_browniano.ipynb) | Sigue partículas libres, calcula el MSD y obtiene el coeficiente de difusión y la viscosidad del medio. | $D = (0{,}057 \pm 0{,}007)\ \mu\mathrm{m^2/s}$, $\eta = (2{,}4 \pm 0{,}3)$ mPa s |
| [`02_equiparticion.ipynb`](notebooks/02_equiparticion.ipynb) | Sigue una partícula atrapada y estima la rigidez de la trampa por equipartición. | $k_x = (4{,}0 \pm 0{,}3)$ pN/µm, $k_y = (2{,}3 \pm 0{,}2)$ pN/µm; cotas inferiores sin corrección del error de localización |
| [`03_fuerza_arrastre.ipynb`](notebooks/03_fuerza_arrastre.ipynb) | Sigue una partícula libre y otra atrapada durante el aumento del flujo; estima la fuerza máxima de la trampa en el instante de escape. | $F_{\max}=6\pi\eta Rv_{\mathrm{esc}}$; estima $k_{\mathrm{eff}}=F_{\max}/\Delta r$ si el desplazamiento supera tres veces el ruido |

### 01 — Movimiento browniano

Con el láser apagado, las microesferas se mueven libremente. El notebook prepara cada
frame mediante una serie de pasos, detecta las partículas, trackea su movimiento y con
ello calcula el desplazamiento cuadrático medio de cada trayectoria. El ajuste
$\mathrm{MSD}(\tau) = 4\sigma^2 + 4D\tau + (v\tau)^2$ separa la difusión del error de
localización $\sigma$ y de la deriva $v$. Promediando las trayectorias que pasan ciertos
criterios de calidad se obtiene $D$ y, con la relación de Stokes–Einstein, la
viscosidad del medio. También incluye un análisis de sensibilidad a los parámetros de
seguimiento.

### 02 — Calibración por equipartición

Suponiendo un potencial armónico cerca del centro de la trampa y equilibrio térmico,
se cumple $k\langle x^2\rangle = k_BT$ en cada dirección. El notebook
sigue el núcleo brillante de la partícula, quita la deriva lenta y obtiene $k_x$ y
$k_y$ a partir de las varianzas. Como verificación reconstruye el pozo de potencial
$U = -k_BT\ln P(x)$ y lo compara con la parábola que predicen las $k$ medidas. En la
configuración guardada no se usa una referencia inmóvil para corregir el error de
localización: por eso las rigideces calculadas son cotas inferiores.

### 03 — Fuerza máxima por arrastre

Se acelera el fluido de forma aproximadamente constante mientras una partícula
permanece atrapada por el láser. El notebook sigue una partícula libre y la atrapada
mediante puntos de referencia marcados a mano y detecciones refinadas con `trackpy`.
El instante de escape se identifica en el video. Un ajuste lineal de la posición de la
partícula libre en los 20 frames anteriores permite estimar la velocidad del flujo
$v_{\mathrm{esc}}$ en ese instante.

En el umbral de escape se supone que la fuerza viscosa iguala la fuerza máxima de
atrapamiento. Para una esfera de radio $R$ en un fluido de viscosidad $\eta$, la ley
de Stokes da $F_{\max}=6\pi\eta Rv_{\mathrm{esc}}$. El notebook usa la viscosidad
obtenida del análisis browniano y el radio configurado para la esfera atrapada.
Comprueba la sensibilidad a la ventana del ajuste y compara la velocidad de la
partícula escapada con la de la partícula libre. También estima una rigidez efectiva
$k_{\mathrm{eff}}=F_{\max}/\Delta r$ a partir del
desplazamiento de la partícula atrapada respecto del centro de la trampa, siempre que
ese desplazamiento supere tres veces el ruido de localización. Esta interpretación
supone una respuesta aproximadamente lineal antes del escape. El código usa el módulo
de $\Delta r$ en lugar de su proyección sobre la dirección del arrastre.

## Cómo correrlo

```bash
pip install -r requirements.txt
```

Los videos no están en el repositorio por su tamaño. Los notebooks buscan los archivos
`video1.avi`, `muestratrapada.avi` y `muestraperdida.avi`, respectivamente, desde el
directorio de trabajo. El nombre del video y los parámetros se cambian al comienzo de
cada notebook; en el de arrastre también deben revisarse el frame de escape, las anclas
de seguimiento y los valores usados en la estimación de rigidez efectiva. Las figuras
se guardan en `figuras/` y `figuras_pinza/`.
