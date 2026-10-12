# Movimiento browniano y pinzas ópticas — Laboratorio 5

Código de análisis del trabajo de pinzas ópticas de Laboratorio 5 (cátedra Larotonda),
Departamento de Física, FCEyN, Universidad de Buenos Aires.

**Autoras:** Anna Benedetti, Milagros Paseri y María Dolores Sirera.

Estudiamos el movimiento browniano de microesferas de látex en una mezcla de agua y
glicerina y lo usamos para caracterizar una pinza óptica. Las posiciones de las
partículas se obtienen de videos del microscopio con `trackpy`.

## Contenido

| Notebook | Qué hace | Resultado principal |
|---|---|---|
| [`01_movimiento_browniano.ipynb`](notebooks/01_movimiento_browniano.ipynb) | Sigue partículas libres, calcula el MSD y obtiene el coeficiente de difusión y la viscosidad del medio. | $D = (0{,}057 \pm 0{,}007)\ \mu\mathrm{m^2/s}$, $\eta = (2{,}4 \pm 0{,}3)$ mPa s |
| [`02_equiparticion.ipynb`](notebooks/02_equiparticion.ipynb) | Sigue una partícula atrapada y obtiene la rigidez de la trampa por equipartición. | $k_x = (4{,}0 \pm 0{,}3)$ pN/µm, $k_y = (2{,}33 \pm 0{,}15)$ pN/µm |

### 01 — Movimiento browniano

Con el láser apagado, las microesferas se mueven libremente. El notebook prepara cada
frame mediante una serie de pasos, detecta las
partículas, trackea su movimiento y con ello calcula el desplazamiento cuadrático medio de cada trayectoria. El ajuste
$\mathrm{MSD}(\tau) = 4\sigma^2 + 4D\tau + (v\tau)^2$ separa la difusión del error de
localización $\sigma$ y de la deriva $v$. Promediando las trayectorias que pasan cierto
criterios de calidad se obtiene $D$ y, con la relación de Stokes–Einstein, la
viscosidad del medio. También incluye un análisis de sensibilidad a los parámetros de
seguimiento.

### 02 — Calibración por equipartición

Con una partícula atrapada, el potencial cerca del centro de la trampa es armónico y
en equilibrio térmico $k\langle x^2\rangle = k_BT$ en cada dirección. El notebook
sigue el núcleo brillante de la partícula, quita la deriva lenta y obtiene $k_x$ y
$k_y$ a partir de las varianzas. Como verificación reconstruye el pozo de potencial
$U = -k_BT\ln P(x)$ y lo compara con la parábola que predicen las $k$ medidas.

## Cómo correrlo

```bash
pip install -r requirements.txt
```

Los videos no están en el repositorio por su tamaño. Cada notebook busca su video en
la misma carpeta (`video1.avi` y `muestratrapada.avi`); el nombre se cambia en la
celda de configuración, junto con la calibración, la temperatura y los parámetros de
detección. Las figuras se guardan en `figuras/` y `figuras_pinza/`.
