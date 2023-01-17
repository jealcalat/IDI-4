# Checklist para proyecto de Ciencia de Datos

- [x] [Definir el problema](#definir-el-problema) y/o formular un panorama general para dicho problema.
- [x] [Obtener los datos](#obtener-los-datos).
- [x] [Explorar los datos](#explorar-datos) para obtener ideas, intuiciones, y armar un plan de trabajo.
- [x] [Preparar los datos](#preparar-los-datos) para exponer patrones subyacentes usando algoritmos.
- [ ] [Explorar diferentes modelos](#explorar-diferentes-modelos), y realizar una lista escogiendo los mejores de acuerdo a alguna métrica (e.g., RMSE).
- [ ] [Afinar los modelos](#afinar-los-modelos) (e.g., reentrenamiento haciendo una búsqueda exhaustiva en un rango de parámetros) y combinarlos en una solución mayor.
- [ ] [Presentar la solución](#presentar-la-solución).

## Definir el problema

1. Definir el objetivo para una audiencia amplia, por ejemplo, en términos de negocios. Esto servirá como set-point. Tu solución debe estar tan cerca de este objetivo como sea posible, y harás bien en tenerlo presente siempre.
2. Cómo será usada la solución.
3. Cuáles son las soluciones actuales alternativas (de haberlas).
4. Definición del problema propiamente, es decir, formular los aspectos de forma abstracta con lenguaje de matemáticas. 
5. Restringe el rango de soluciones para el problema. Por ejemplo, ¿dirías que el problema es supervisado/no supervisado, online/offline?
6. Cómo deberás medir la ejecución del modelo.
7. ¿La forma de medir la ejecución se alinea con los objetivos del negocio?
8. ¿Cuál es el nivel de ejecución mínima para cumplir con el objetivo del negocio?
9. ¿Se puede usar la solución en problemas *comparables*? ¿Se pueden usar la experiencia o las herramientas?
10. ¿Existe experticia humana disponible para el problema?
11. Lista las asunciones que has hecho hasta ahora.
12. Verifica si las asunciones se cumplen.

## Obtener los datos

La obtención de datos suele ser un cuello de botella. Nececitas datos limpios, recientes, lo más sencillos de conseguir que puedas. Comenzamos con un paso 0 que deberías tener en cuenta desde el momento en el que comienzas a buscar datos. 

0. Automatiza tanto como puedas la obtención de datos, para tener datos frescos.
1. Documenta cómo obtuviste los datos.
2. Revisa cuánto espacio ocuparán.
3. Revisa las obligaciones legales de los datos, u obtén autorización para usarlos y/o publicar derivados de ellos.
4. Crea un espacio de trabajo (workspace) con espacio suficiente para almacenar los datos. 
5. Obtén los datos.
6. Convierte los datos en un formato que puedas manipular fácilmente (e.g., csv) sin cambiar los datos en sí.
7. **Asegúrate** de que la información sensible ha sido omitida (nombres, direcciones, etc; anonimízalos).
8. Revisa qué tipo de datos tienes (¿son series de tiempo, datos geográficos, una combinación de diferentes tipos?).
9. Toma una muestra de prueba (test set), separarla y no la uses. 

## Explorar datos

Si existen expertos en este tipo de datos, no dudes en buscar su ayuda.

1. Crea una copia de los datos para su exploración. Si tienes un conjunto muy pesado, toma una muestra (aleatoria) de un tamaño manejable. En esta parte solo querrás hacerte una idea de qué es lo que tienes, y una muestra aleatoria de tamaño manejable asegura que las propiedades estadísticas de los datos son representativas del conjunto original.
2. Crea una notebook de Jupyter o un archivo de Quarto para la exploración. Es preferible una combinación de texto y código para documentar el proceso.
3. Estudia todos los atributos
   1. Nombres
   2. Tipos de datos (categóricos/numéricos, enteros/puno flotante, restringidos o no restringidos, estructurados o no estructurados, etc).
   3. Porcentaje de datos perdidos.
   4. Ruido y su tipo (meramente estocástico, debido a error de redondeo, outliers posibles, o imposibles como pesos negativos, edades de 200 años, fechas imposibles, etc.)
   5. Tipo de distribución de los datos.
4. Para algoritmos de supervisión, **identifica** la/las variables objetivo.
5. Visualiza los datos.
6. Estudia la correlación (u otras dependencias no lineales; por ejemplo [esto](http://www.exploredata.net/)) entre atributos.
7. Estudia cómo podrías resolver el problema manualmente.
8. Estudia las transformaciones que podrías aplicar (e.g., si una log-transformación vuelve normales los datos, sería mejor).
9. Identifica si obtener más datos podría ser útil.
10. Documenta lo que has aprendido sobre los datos

## Preparar los datos

>Notas
> - Trabaja en copias de los datos (deja el original intacto).
> - Escribir funciones para todas las transformaciones que apliques, de tal manera que:
>     - Puedas aplicarlo fácilmente la próxima vez que tengas datos frescos
>     - Apliques las mismas transformaciones en futuros proyectos
>     - Limpies y prepares el conjunto de prueba

1. Limpieza de datos
   1. Arreglar o remover outliers
   2. Rellenar datos perdidos (e.g., usando imputación múltiple, la media, o la mediana), o quitar las filas o columnas con NAs
2. Selección/ingeniería de características 
   1. Remueve los atributos/variables que no proveen información para la tarea (opcional)
   2. Discretiza variables continuas, etc.
   3. Descompón características
   4. Añade transformaciones prometedoras (e.g., log-transformar, transformar con raíz cuadrada, etc., para volver normal)
3. Normaliza o estandariza las características
   1. E.g., Unit-based normalization.
   2. Esto es importante cuando tienes variables de diferentes escalas

## Explorar diferentes modelos
>Notas
> - Si el conjunto de datos es largo y tienes más de un modelo, quizá sea mejor muestrear un conjunto más pequeño primero, de manera que puedas entrenarlos en un tiempo razonable.
> - Lo anterior penaliza modelos que requieren muchos datos, como las redes neurales.
> - Para este curso **no uses más de dos modelos**, dado que no es razonable terminar de entrenarlos, afinarlos y presentar resultados en 4 meses.

1. Entrena los modelos usando parámetros estándar (e.g., por defecto).
2. Mide y compara su ejecución.
   1. Para cada modelo, puedes usar validación cruzada $k$-fold. Calcula la media y la desviación estándar de la medida de ejecución que hayas elegido.
3. Analiza las variables más significativas de cada algoritmo.
4. Analiza los errores que cometiste o cometió el modelo
   1. Si puedes identificar qué tipo de datos pudieran haber hecho que ese/esos errores no sucediera/n, tómalo en cuenta y retroalimenta el proceso para la siguiente ocasión.
5. Ten una ronda rápida de selección e ingeniería de características.
6. Realiza una o dos iteraciones rápidas de los pasos 1-5.
7. Escoge el modelo más prometedor.

## Afinar los modelos
> Notas
> - En este paso usa tantos datos como sea posible.

1. Ajusta los hiperparámetros usando CV.
   1. Trata las elecciones de transformación de datos como hiperparámetros, especialmente cuando no estás seguro acerca de ellos (por ejemplo, ¿reemplazarías valores perdidos con cero, la mediana, usando imputación o solo retirando los valores?). 
   2. Escoge cuidadosamente qué clase de experimentos harás para evaluar los mejores resultados. De preferencia, piensa con antelación las variaciones que harás y ejecuta el proceso simultáneamente para que no pierdas tiempo
   3. A menos de que tengas muy pocos hiperparámetros, prefiere la búsqueda aleatoria en vez de la búsqueda en *grid* (ésta última crece la cantidad de pasos de forma exponencial). 
2. Una vez que tengas suficiente confianza en tu modelo, mide su ejecución en el conjunto de prueba para estimar el error de generalización.
3. **No hagas modificaciones después de medir el error de generalización**, esto solo hará que sobreajustes en el conjunto de prueba.

## Presentar la solución

1. Documenta todo lo que hayas hecho: las decisiones que tomaste, qué criterios usaste para tomarlas, etc.
2. Escoge con cuidado la mejor narrativa para tu solución:
   1. Tablas resumidas de los principales resultados (e.g., ejecución del modelo, porcentaje de predicción, precisión, etc).
   2. Tus resultados deben tener una calidad adecuada para presentarla frente a un grupo de personas educadas y críticas. Por ejemplo, cuida los colores de los gráficos, el tamaño de la letra, etc. No atiborres de estadísticos ni de datos las tablas que vayas a presentar. 
   3. No presentes resultados en una forma que sea complicada o imposible de entender, por mucho atractivo visual que pudiera tener. 
   4. Que tu set-point sea un objetivo de negocio. Pregúntate: ¿este resultado, o esta forma de presentar el resultado le importará a mi cliente?
3. Una vez que hayas elegido gráficos, tablas, etc., de la calidad que te satisfaga, escríbelos en tu reporte técnico.
4. Adicionalmente, realiza una presentación *ejecutiva* con tus resultados principales. De nuevo, teniendo en mente la pregunta 2.4.: ¿cómo le presentarás a tus clientes una solución de ciencia de datos?