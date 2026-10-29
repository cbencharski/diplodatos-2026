# Notas de repaso: Exploración y Curación de Datos

## Entregable 1: curación de datos

- **Dato faltante "disfrazado":** un `0` o un valor fuera de rango puede ser en realidad un dato no registrado. Hay que decidir con criterio del dominio si es real o si debe tratarse como faltante. Por ejemplo, un terreno de 0 m² no existe, pero una casa sin cochera sí.
- **Variables redundantes:** si dos variables tienen una correlación muy alta (como `Rooms` y `Bedroom2`, 0,95), aportan casi la misma información. Conviene quedarse con la más confiable.
- **One-hot encoding:** convierte una variable categórica en varias columnas binarias (0/1), una por categoría. Es necesario porque los modelos trabajan con números, y asignarle un número a cada categoría inventaría un orden que no existe.
- **Alta cardinalidad:** variables con cientos o miles de categorías (`Suburb`, `Address`) generan demasiadas columnas con one-hot. Se pueden descartar, agrupar las categorías poco frecuentes o usar otra codificación.
- **Imputación iterativa con KNN:** estima cada valor faltante a partir de las filas más parecidas (los "vecinos"), usando todas las demás variables. Es más precisa que reemplazar por la media, porque aprovecha las relaciones entre variables.
- **¿Por qué estandarizar antes de KNN y PCA?** Los dos métodos dependen de distancias o varianzas. Sin estandarizar, una variable como `Price` (millones) dominaría frente a `Rooms` (de 1 a 10) solo por su escala.
- **PCA (análisis de componentes principales):** combina las variables en nuevas direcciones ordenadas según cuánta variabilidad de los datos explican. Sirve para reducir dimensiones o para crear nuevas características.

## Entregable 2: SQL y combinación de datos

- **`GROUP BY` + funciones de agregación** (`COUNT`, `AVG`): agrupan filas y calculan un valor por grupo.
- **`WHERE` vs `HAVING`:** `WHERE` filtra filas **antes** de agrupar; `HAVING` filtra grupos **después** de agregar. Por ejemplo, `HAVING COUNT(*) >= 10` descarta barrios con pocas ventas, cuyo promedio no sería confiable.
- **`ORDER BY` + `LIMIT`:** ordenan el resultado y se quedan con las primeras filas (por ejemplo, un top 5).
- **`LEFT JOIN` vs `INNER JOIN`:** el `LEFT JOIN` conserva todas las filas de la tabla de la izquierda, aunque no tengan coincidencia en la derecha (quedan con `NULL`). El `INNER JOIN` descarta las filas sin coincidencia. Ojo: `pandas.merge` hace un *inner join* por defecto.
- **Validación de calidad después de un JOIN:** controlar la cantidad de filas, los nulos, los duplicados y los rangos de valores. Un JOIN mal hecho puede duplicar filas o perder datos sin dar ningún error.
- **¿Mediana o media?** La mediana es **robusta** frente a valores extremos: unas pocas publicaciones de lujo pueden subir mucho la media de un código postal, pero casi no mueven la mediana.
- **Fuga de información (*data leakage*):** usar como variable algo calculado a partir del valor que se quiere predecir. El modelo parece funcionar muy bien, pero en la práctica falla, porque ese dato no estaría disponible. En este caso, `PC1` y `PC2` se calcularon incluyendo `Price`.
