# Índice de Movilidad VisaMundial (IMV) 2026

**Requisitos de visado para 199 pasaportes y 199 destinos, en español, con foco en América Latina.**
Datos abiertos bajo licencia CC-BY 4.0.

🔗 Índice publicado: <https://visamundial.com/indice/>
📐 Metodología: <https://visamundial.com/indice/metodologia/>

---

## Qué es el IMV

El IMV mide a cuántos destinos puede entrar el titular de un pasaporte **sin tramitar una visa
antes de salir de casa**.

```
IMV = destinos sin visa + destinos con visa a la llegada + destinos con autorización electrónica (eTA)
```

**Excluimos la e-visa a propósito.** Casi todos los rankings de pasaportes la incluyen; nosotros no,
porque una e-visa es una solicitud de visa: se rellena un formulario, se paga, se suben documentos y
se espera una respuesta que puede ser negativa. Contarla infla la cifra y no refleja lo que el
viajero vive. El recuento de e-visa está igualmente en los datos, en su propia columna, para quien
prefiera la definición amplia.

## Ficheros

| Fichero | Qué contiene |
|---|---|
| [`data/ranking-imv.csv`](data/ranking-imv.csv) | 199 pasaportes con su IMV y el desglose por categoría |
| [`data/matriz-tidy.csv`](data/matriz-tidy.csv) | 39.402 pares pasaporte→destino con el requisito de entrada |
| [`data/indice-movilidad.json`](data/indice-movilidad.json) | El ranking completo en JSON, con metadatos |

### `ranking-imv.csv`

```
puesto,iso2,pasaporte,imv,sin_visa,visa_llegada,eta,evisa,visa_previa,sin_admision
1,AE,Emiratos Árabes Unidos,169,127,34,8,18,11,0
2,KR,Corea del Sur,163,114,38,11,21,14,0
...
```

### `matriz-tidy.csv`

Tres columnas: `pasaporte` (ISO-3166-1 alfa-2), `destino` (ídem) y `requisito`.

```
pasaporte,destino,requisito
MX,ES,sin visa
MX,GB,eta
MX,US,visa previa
```

Valores de `requisito`: `sin visa`, `visa a la llegada`, `eta`, `e-visa`, `visa previa`, `sin admision`.

## API en vivo

El índice también se sirve como API pública, sin clave ni registro:

```bash
curl https://visamundial.com/wp-json/vm/v1/imv        # ranking completo
curl https://visamundial.com/wp-json/vm/v1/imv/MX     # un pasaporte con todos sus destinos
curl https://visamundial.com/indice/datos.csv         # CSV del ranking
```

## Ejemplo rápido (Python)

```python
import pandas as pd

rank = pd.read_csv("data/ranking-imv.csv")
latam = ["MX","CO","VE","PE","AR","CL","EC","BO","PY","UY",
         "BR","CR","PA","GT","HN","SV","NI","DO","CU"]

print(rank[rank.iso2.isin(latam)][["puesto","pasaporte","imv"]].head(10))
```

## Procedencia de los datos

La matriz base procede de [`imorte/passport-index-data`](https://github.com/imorte/passport-index-data)
(licencia MIT). Sobre esa base aplicamos nuestra propia clasificación en seis categorías, el cálculo
del IMV y la nomenclatura en español, y la contrastamos con las guías de VisaMundial verificadas
contra fuentes oficiales: Departamento de Estado de EE. UU., IRCC de Canadá, la Comisión Europea,
gov.uk y los ministerios de exteriores de cada país.

Lo decimos abiertamente porque es lo honesto: no partimos de cero. Partimos de datos abiertos y les
aportamos la clasificación, el enfoque latinoamericano y la verificación editorial.

## Límites que conviene conocer

- El índice describe **pasaportes, no personas**. Tu caso puede cambiar por residencia, doble
  nacionalidad, antecedentes o motivo del viaje.
- Cubre **turismo de corta estancia**. Trabajar, estudiar o residir tiene otras reglas.
- Las políticas migratorias cambian sin aviso. **Antes de comprar un vuelo, verifica en la fuente
  oficial del destino.**

## Actualizaciones

Revisión trimestral, con nota pública de qué cambió. Versión actual: **2026.2** (2026-08-06).

### Qué cambió en la 2026.2

Contrastamos los 21 pasaportes más consultados contra una segunda fuente independiente, y donde
las dos discrepaban resolvimos el caso consultando la fuente oficial del destino. Resultado:
**3.714 de 3.852 pares verificados (96,4 %)** y **108 clasificaciones corregidas**. Ejemplos:
Irlanda exige visado a los nicaragüenses desde el 15 de junio de 2026; Cabo Verde sustituyó la
visa a la llegada por un registro previo obligatorio; Costa de Marfil tramita e-visa y no una
simple autorización electrónica.

Los **138 pares que siguen abiertos** no llevan fecha de verificación: o la política depende de la
nacionalidad concreta, o no encontramos fuente oficial. Preferimos marcarlos como pendientes.

## Cómo citar

> Índice de Movilidad VisaMundial 2026, VisaMundial.com, actualizado el 6 de agosto de 2026.
> Disponible en https://visamundial.com/indice/ bajo licencia CC-BY 4.0.

## Errores y correcciones

¿Ves un dato equivocado? Abre una *issue* indicando pasaporte, destino y la fuente oficial que lo
contradice, o escribe a <editorial@visamundial.com>. Corregimos y lo dejamos anotado.

## Licencia

[CC-BY 4.0](LICENSE). Puedes reutilizar estos datos, incluso comercialmente, citando a VisaMundial
y enlazando a <https://visamundial.com/indice/>.

---

<details>
<summary><strong>English summary</strong></summary>

**VisaMundial Mobility Index (IMV) 2026** — travel visa requirements for 199 passports and 199
destinations, in Spanish, focused on Latin America. CC-BY 4.0.

`IMV = visa-free + visa on arrival + electronic travel authorisation (eTA)`

Unlike most passport rankings, **e-visas are deliberately excluded**: an e-visa is still a visa
application, with forms, fees, documents and a possible rejection. The e-visa count is provided in
its own column for anyone preferring the broader definition.

Base matrix from [`imorte/passport-index-data`](https://github.com/imorte/passport-index-data) (MIT),
reclassified and editorially verified against official government sources.

Live API: `https://visamundial.com/wp-json/vm/v1/imv`

</details>
