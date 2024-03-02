# Prueba Tecnica MeLi
## Desarrollado Por Jose Currea

### Objetivo
La meta en esta prueba técnica fue desarrollar una solución análitica para el equipo comercial con el fin de realizar estrategias focalizadas para los sellers. Este proceso se llevó a cabo en 3 partes, exploración, limpieza y y maseo de los datos, y construcción de la solución.

### Parte 1 - Exploración
En esta primera fase del desarrollo de la solución, se realizo una exploración a los datos recogidos desde el API de MeLi con el fin de identificar la información que podría ser de ayuda. 

#### 1.1 Exploración API categorías y API items
Esta fase de la exploración la dividí en dos partes, la primera fue explorar el API de categorías con el fin de ver las distintas disponibles y así poder seleccionar una para mi solución. En este caso, tras explorar las diferentes categorías, me decidí por la MCO1276 la cual hace referencia a Deporte y Fitness en Colombia. Decidí elegir esta ya que soy aficionado a esto. Algunas de las categorías que se pueden ver explorando el API son las siguientes:

<img src="https://github.com/jncurrea/Prueba_Tecnica/blob/main/Reference_Images/Screenshot%202024-03-01%20at%205.41.19%20PM.png" alt="Imágen Referencia Categorias" width="300"/>

Posterior a la exploración de las categorias, iteré sobre el API de items, ya con mi categoría seleccionada, y cree el Data Frame con el que iba a empezar a trabajar para extraer información. A continuación se ven imágenes de referencia de como se ve el Data Frame y las columnas con las que contaba inicialmente.

<img src="https://github.com/jncurrea/Prueba_Tecnica/blob/main/Reference_Images/Screenshot%202024-03-01%20at%205.41.33%20PM.png" alt="Imágen Referencia DataFrame" width="300"/>

<img src="https://github.com/jncurrea/Prueba_Tecnica/blob/main/Reference_Images/Screenshot%202024-03-01%20at%205.41.48%20PM.png" alt="Imágen Columnas Iniciales" width="300"/>

### 1.2 Generación de Nuevas Columnas
Al realizar la exploración del DataFrame de items, pude evidenciar que algunas columnas de este dataframe inicial venían en formato JSON. Decidí expandirlas con el fin de obtener un Data Frame más robusto que me diera la oportunidad de disponer de más información para el desarrollo de la solución.

Identifique tres columnas que podrīan ser de ayuda para el desarrollo de mi solución de cara a los sellers, la columna seller que identificaba al seller de cada producto y sería mi punto de partida,la columna shipping que incluía campos tales como free_shipping y store_pick_up, y la columna installments que incluía informacion referente a ventas.

Seleccioné la columna shipping ya que asumí que si un seller ofrece envío gratis en uno de sus productos, sería indicativo de un plus que ofrece el seller para el cliente siendo así un seller relevante para el negocio. Por otra parte, seleccionê la columna installments ya que con ella asumí que podría identificar metricas claves como cantidad de articulos vendidos (por item).

<img src="https://github.com/jncurrea/Prueba_Tecnica/blob/main/Reference_Images/Screenshot%202024-03-01%20at%206.18.16%20PM.png" alt="Imágen Columnas tras expansión de columnas JSON" width="300"/>
