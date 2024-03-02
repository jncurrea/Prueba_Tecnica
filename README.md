# Prueba Tecnica MeLi
## Desarrollado Por Jose Currea

### Objetivo
La meta en esta prueba técnica fue desarrollar una solución análitica para el equipo comercial con el fin de realizar estrategias focalizadas para los sellers. Este proceso se llevó a cabo en 3 partes, exploración y limpieza, maseo de los datos y exploración por seller, y construcción de la solución.

### Parte 1 - Exploración
En esta primera fase del desarrollo de la solución, se realizo una exploración a los datos recogidos desde el API de MeLi con el fin de identificar la información que podría ser de ayuda. 

#### 1.1 Exploración API categorías y API items
Esta fase de la exploración la dividí en dos partes, la primera fue explorar el API de categorías con el fin de ver las distintas disponibles y así poder seleccionar una para mi solución. En este caso, tras explorar las diferentes categorías, me decidí por la MCO1276 la cual hace referencia a Deporte y Fitness en Colombia. Decidí elegir esta ya que soy aficionado a esto. Algunas de las categorías que se pueden ver explorando el API son las siguientes:

<div>
<img src="https://github.com/jncurrea/Prueba_Tecnica/blob/main/Reference_Images/Screenshot%202024-03-01%20at%205.41.19%20PM.png" alt="Imágen Referencia Categorias" width="500"/>
</div>

Posterior a la exploración de las categorias, iteré sobre el API de items, ya con mi categoría seleccionada, y cree el Data Frame con el que iba a empezar a trabajar para extraer información. A continuación se ven imágenes de referencia de como se ve el Data Frame y las columnas con las que contaba inicialmente.

<div>
<img src="https://github.com/jncurrea/Prueba_Tecnica/blob/main/Reference_Images/Screenshot%202024-03-01%20at%205.41.33%20PM.png" alt="Imágen Referencia DataFrame" width="500"/>
</div>

<div>
<img src="https://github.com/jncurrea/Prueba_Tecnica/blob/main/Reference_Images/Screenshot%202024-03-01%20at%205.41.48%20PM.png" alt="Imágen Columnas Iniciales" width="500"/>
</div>

### 1.2 Generación de Nuevas Columnas
Al realizar la exploración del DataFrame de items, pude evidenciar que algunas columnas de este dataframe inicial venían en formato JSON. Decidí expandirlas con el fin de obtener un Data Frame más robusto que me diera la oportunidad de disponer de más información para el desarrollo de la solución.

Identifique tres columnas que podrīan ser de ayuda para el desarrollo de mi solución de cara a los sellers, la columna seller que identificaba al seller de cada producto y sería mi punto de partida,la columna shipping que incluía campos tales como free_shipping y store_pick_up, y la columna installments que incluía informacion referente a ventas.

Seleccioné la columna shipping ya que asumí que si un seller ofrece envío gratis en uno de sus productos, sería indicativo de un plus que ofrece el seller para el cliente siendo así un seller relevante para el negocio. Por otra parte, seleccionê la columna installments ya que con ella asumí que podría identificar metricas claves como cantidad de articulos vendidos (por item). A continuación, las columnas del nuevo DataFrame.

<div>
<img src="https://github.com/jncurrea/Prueba_Tecnica/blob/main/Reference_Images/Screenshot%202024-03-01%20at%206.18.16%20PM.png" alt="Imágen Columnas tras expansión de columnas JSON" width="500"/>
</div>

### Parte 2 - Maseo de los datos y exploración por seller

En esta segunda fase, ya me enfoque más en la información de cara a sellers. Esta fase la dividí en 2, agriupaciòn de variables a nivel de seller y construcción de DataFrame a nivel de sellers.

#### 2.1 Agrupación de variables por seller
La primera parte fue agrupar por seller algunas de las variables que consideré importantes con el fin de identificar información como total de productos ofrecidos por seller, referencias unicas ofrecidas por seller, cantidad vendida por seller, cantidad disponible de items por selles, entre otras. Realicé estas agrupaciones ya que el tener una idea de como se vería la información realizando estas agrupaciones me daría una visual de información que podria tener en un DataFrame de sellers. A continuación algunos ejemplos como items distintos ofrecidos por seller e items que aceptan MercadoPago por seller.

<div>
<img src="https://github.com/jncurrea/Prueba_Tecnica/blob/main/Reference_Images/Screenshot%202024-03-01%20at%206.42.39%20PM.png" alt="Imágen Columnas tras expansión de columnas JSON" width="500"/>
</div>

<div>
<img src="https://github.com/jncurrea/Prueba_Tecnica/blob/main/Reference_Images/Screenshot%202024-03-01%20at%206.42.32%20PM.png" alt="Imágen Columnas tras expansión de columnas JSON" width="500"/>
</div>

#### 2.2 Generación Data Frame sellers
En esta segunda parte, construí un DataFrame base a nivel de sellers con el fin de identificar caracteristicas de los sellers tales como la aceptación de MercadoPago, envío gratis, items vendidos, entre otras. Este DataFrame fue construido con base a la exploración y agrupación de las variables en la parte anterior. Las columnas de este DataFrame base son las siguientes:

<div>
<img src="https://github.com/jncurrea/Prueba_Tecnica/blob/main/Reference_Images/Screenshot%202024-03-01%20at%206.52.40%20PM.png" alt="Imágen Columnas tras expansión de columnas JSON" width="500"/>
</div>

### Parte 3 - Construcción de la solución

#### 3.1 - Modelos Supervisados
En esta primera fase realicé pruebas de dos modelos supervisados como solución y poder identificar los sellers de buen perfil. Teniendo en cuenta que para los modelos supervisados se necesita una variable Y que permita evaluar la predicción, cree la variable good_profile manualmente basado en ciertas caracteristicas que asumí serian identificadoras de un buen seller. Esto lo hice teniendo en cuenta la información proporcionada por el comando describe() del DataFrame de sellers. 

La columna se construyó con las siguientes caracteristicas:
```python
final_seller_df['good_profile'] = (final_seller_df['accepts_mercadopago'] == True) & 
                                  (final_seller_df['store_pick_up'] == False) & 
                                  (final_seller_df['sold_quantity'] >= 400) & 
                                  (final_seller_df['quantity_new_items'] >= 14) &
                                  (final_seller_df['free_shipping'] == True) & 
                                  (final_seller_df['quantity_distinct_products'] >= 3)
```

La información del describe() fue la siguiente:

<div>
<img src="https://github.com/jncurrea/Prueba_Tecnica/blob/main/Reference_Images/Screenshot%202024-03-01%20at%206.55.26%20PM.png" alt="Imágen Columnas tras expansión de columnas JSON" width="500"/>
</div>

##### 3.1.1 - Random Forest Classifier
Posterior a la construcción de la variable Y, realicé pruebas con dos modelos. El primer modelo fuen un Random Forest Classifier que alcanzó un **accuracy del 98%.** Esto es un hito positivo ya que, basado en la matriz de confusión, tan solo hubo 1 seller que no fue categorizado correctamente y quedó dentro de la categoría de "falso positivo", es decir, el modelo lo clasificó como un seller con buen perfil, sin embargo la columna good_profile no lo consideraba como tal.

A continuación se ve los resultados del accuracy y la matriz de confusión:
<div>
<img src="https://github.com/jncurrea/Prueba_Tecnica/blob/main/Reference_Images/Screenshot%202024-03-01%20at%206.55.37%20PM.png" alt="Imágen Columnas tras expansión de columnas JSON" width="500"/>
</div>

A continuación, las variables consideradas importantes por el modelo:
<div>
<img src="https://github.com/jncurrea/Prueba_Tecnica/blob/main/Reference_Images/Screenshot%202024-03-01%20at%206.55.45%20PM.png" alt="Imágen Columnas tras expansión de columnas JSON" width="500"/>
</div>

##### 3.1.2 - K Nearest Neighbours (KNN)
Como segundo modelo supervisado, utilicé un modelo KNN. Este modelo alcanzó **un accuracy del 85%**. A pesar de tener una menor accuracy, este modelo aún se puede considerar como bueno ya que de los 70 sellers tan solo clasificó "erroneamente" a 10 sellers, es decir tan solo un 14%.

A continuación la matriz de confusión del modelo KNN:
<div>
<img src="https://github.com/jncurrea/Prueba_Tecnica/blob/main/Reference_Images/Screenshot%202024-03-01%20at%206.55.54%20PM.png" alt="Imágen Columnas tras expansión de columnas JSON" width="500"/>
</div>

A continuación los puntos reales VS las predicciones del modelo:
<div>
<img src="https://github.com/jncurrea/Prueba_Tecnica/blob/main/Reference_Images/Screenshot%202024-03-01%20at%206.56.03%20PM.png" alt="Imágen Columnas tras expansión de columnas JSON" width="500"/>
</div>

#### 3.2 - Modelo No Supervisado
Decidí realizar pruebas tambien con un modelo No Supervisado. Para este caso, probé el modelo Kmeans. Debido a que el modelo no supervisado no requiere una variable Y, hice un drop de la columna good_profile. Este modelo tan solo alcanzó **un accuracy del 12%** indicando que este modelo no realizò un buen trabajo  identificando a los sellers de buen perfil. A continuación, en la grafica, se evidencia como este modelo realizó un mejor trabajo prediciendo sellers de mal perfil que los de buen perfil ya que la distancia al cluster 0 (idica que el seller no tiene un perfil atractivo) es de 2 unidades, mientras que la distancia al cluster 1 (indicando un seller de buen perfil) es de 10. Recordemos que entre mas cercanía haya al cluster, mejor será la predicción.

<div>
<img src="https://github.com/jncurrea/Prueba_Tecnica/blob/main/Reference_Images/Screenshot%202024-03-01%20at%206.56.34%20PM.png" alt="Imágen Columnas tras expansión de columnas JSON" width="500"/>
</div>


### Resumen, conclusiones y pasos a seguir

1. El problema que se intenta resolver, es la necesidad de una clasificación que distinga a los sellers con buen perfil para que el equipo comercial pueda dirigir sus esfuerzos estratégicos de manera más efectiva.
2. Se utilizó informacion referente a los sellers con base en la información obtenida en el API de items. Como se mencionó anteriormente, identifique tres columnas que podrīan ser de ayuda para el desarrollo de mi solución de cara a los sellers, la columna seller que identificaba al seller,la columna shipping y la columna installments, adicional a la información ya dispuesta en el API de items.
    2.1. La hipotesis que me llevo a seleccionar shipping, fue que era posible que un seller sería más relevante si ofrecía envíos gratis.
    2.2. La hipótesis que me llevo a seleccionar installments fue que la cantidad de items vendidos me permitiria identificar la relevancia de cada seller.
3. Validando mi hipótesis, la variable más significativa al identificar un seller relevante es la cantidad de items vendidos como se puede evidenciar en el numeral 3.1.1, adicional a esto la cantidad de items en estado nuevo tambiên fue una variable relevante al identificar sellers de buen perfil (también dispobnible en el numeral 3.1.1). Curiosamente, un seller que acepte mercado pago, aparentemente no es significativo al momento de ser relevante
4. La solución que escogí fue un modelo de clasificación que permitiera identificar si un seller tenia un buen perfil o no. Inicialmente implementé dos modelos de clasificación supervisados y uno no supervisado para poder comparar métricas de evaluación tales como el accuracy, la precisión y el recall en el caso de los modelos supervisados y la inercia en el caso del modelo Kmeans.
5. Mi solución final es un modelo Random Forest Classifier el cual alcanzó un **accuracy del 98%**, siendo este el modelo que mejor se comportó. Este se basa en generar caracteristicas por el usuario que, teniendo en cuenta el factor humano del usuario, identifiquen si el seller tiene un buen perfil o no. Teniendo en cuenta esta caracterización el modelo realizara una predicción e identificara si cada uno de los sellers tiene un buen perfil o no basado en las caracteristicas importantes. Esto le permite al equipo comercial identificar a los diferentes sellers con el fin de realizar estrategias focalizadas para los sellers de buen perfil y los sellers de mal perfil.
6. En conclusión se desarrollo un modelo que puede ser utilizado por el equipo comercial, permitiendoles identificar sellers con buen perfil y con mal perfil para el desarrollo de sus estrategias focalizadas. Los pasos a seguir son:
    6.1. Desplegar el modelo para que pueda ser consumido por el equipo comercial.
    6.2. Desarrollar las estrategias focalizadas basadas en el análisis y las predicciones del modelo.
    6.3. Realizar iteraciones mensuales, con el fin de reevaluar y mejorar el modelo y las estrategias a medida que se obtiene más información y se recopilan más datos.

# Muchas gracias!
