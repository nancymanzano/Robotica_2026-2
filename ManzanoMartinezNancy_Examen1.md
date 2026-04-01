
#  **1er Examen Parcial \- Robótica** 
### **Modelado de un Robot Manipulador SCARA**

**Nancy Manzano Martínez**

# Objetivos

**Objetivo General**


Desarrollar, analizar y documentar el modelo matemático completo de un manipulador robótico tipo SCARA que describa su comportamiento cinemático y dinámico, mediante la formulación de los modelos directo e inverso de la postura, velocidades y aceleraciones, así como del modelo dinámico basado en el formalismo de Euler\-Lagrange, con el fin de comprender el movimiento del sistema en aplicaciones de robótica.


 **Objetivos Particulares** 

-  Desarrollar el modelo cinemático Para calcuj la postura del robot, obteniendo tanto la cinemática directa como la cinemática inversa, con el fin de determinar la posición y orientación del efector final a partir de las variables articulares, y viceversa. 
-  Desarrollar el modelo cinemático de las velocidades del manipulador, obteniendo el modelo directo e inverso mediante el uso del Jacobiano, para establecer la relación entre las velocidades articulares y las velocidades lineales y angulares del efector final.  
-  Desarrollar el modelo cinemático de las aceleraciones del robot, incluyendo el modelo directo e inverso, incorporando la derivada temporal del Jacobiano, con el propósito de describir completamente el comportamiento del movimiento en términos de aceleraciones.  
-  Obtener el modelo dinámico del manipulador, formulando tanto el modelo dinámico directo como el inverso mediante el formalismo de Euler\-Lagrange, con el fin de determinar las fuerzas y torques necesarios para generar el movimiento del sistema. 
# Introducción

La robótica moderna juega un papel fundamental en la automatización de procesos industriales hoy en día, permitiendo realizar tareas con alta precisión, repetibilidad y eficiencia. Es por esto que, el estudio de manipuladores robóticos resulta esencial, ya que estos sistemas permiten interactuar con el entorno mediante el control de su posición, velocidad y fuerza. Para lograrlo, es necesario desarrollar modelos matemáticos que describan su comportamiento de manera precisa.


El análisis y modelado de estos robots tipo SCARA (Selective Compliance Assembly Robot Arm) constituye una parte fundamental en el estudio de la robótica, permite describir de manera matemática el comportamiento del robot y predecir su movimiento en el espacio. En particular, el modelado cinemático y dinámico proporciona las herramientas necesarias para relacionar las variables articulares con la posición, velocidad y aceleración del efector final, lo cual es esencial para tareas de simulación, control y planificación de trayectorias.


El presente reporte documenta de manera estructurada el desarrollo del modelo de un manipulador robótico plano, abarcando desde la descripción de su postura hasta el análisis de su comportamiento dinámico. Para ello, se parte del modelado cinemático directo, el cual permite determinar la posición y orientación del efector final a partir de las variables articulares, así como del modelado cinemático inverso, que resuelve el problema contrario.


Asimismo, se aborda el modelado dinámico del robot, el cual considera los efectos de fuerzas y torques involucrados en el movimiento, permitiendo describir de manera más completa el comportamiento físico del sistema. Este análisis resulta indispensable para aplicaciones reales, donde no solo interesa conocer la posición del robot, sino también las condiciones necesarias para generar dicho movimiento.


El desarrollo de este trabajo se basa en los conceptos estudiados en clase, y se utiliza matlab como herramienta para generar código y modelos simbólicos, con el fin de obtener expresiones generales que faciliten la implementación y simulación del sistema, para cualesquiera valores de entrada. 


Por último, la elaboración de un reporte técnico que documente de manera detallada el desarrollo del modelado cinemático y dinámico permite organizar, validar y comunicar de forma clara los resultados obtenidos. La documentación no sólo facilita la reproducibilidad del análisis, sino que también sirve como referencia para futuras implementaciones, modificaciones o mejoras del sistema. Para el aprendizaje del modelado de un robot como el que se estudia aquí, contar con un registro del proceso de modelado asegura que las relaciones matemáticas y metodologías empleadas son comprendidas y pueden ser utilizadas en aplicaciones futuras.

## Notación usada en los modelos cinemático y dinámico

Para el desarrollo del presente trabajo, se utiliza la notación empleada para describir la postura y el movimiento del robot, así como las convenciones utilizadas en el manejo de vectores y matrices.


La postura de un robot se representa mediante un conjunto de variables articulares, las cuales se agrupan en un vector general definido como:

 $$ q^T =\left\lbrack q_1 \;q_2 \;q_3 \right\rbrack $$ 

donde cada q\_i corresponde a una variable articular. Para este caso dichas variables representan desplazamientos angulares en cada una de las articulaciones.


Para describir la posición y la orientación de la punta del robot (es decir, el punto final del último eslabón), se utiliza un sistema de referencia cartesiano que permite expresar su ubicación respecto a un sistema inercial o sistema cero {0}.


Dado que el robot opera en el plano, la posición de este punto final se puede expresar como:

 $$ {}^iP^T_P=\left\lbrack {}^ix_P\;{}^iy_P\;{}^iz_P\right\rbrack $$ 

mientras que su orientación se define mediante un ángulo:

 $$ {}^i{\theta }^T_P=\left\lbrack {}^i{\gamma }_P\;{}^i{\beta }_P\;{}^i{\alpha }_P\right\rbrack $$ 

A partir de esto, se define el vector de postura como:

 $$ {}^i{\xi }^T_P=\left\lbrack {}^iP_P\;{}^i{\theta }_P\;\right\rbrack $$ 

Este vector describe completamente dónde está y cómo está orientada la punta del robot en el plano. Además, se encuentra relacionado con las variables articulares "q" mediante las ecuaciones de la cinemática directa.


Adicionalmente, para el manejo de vectores y matrices de transformación, se adopta una convención de índices que permite identificar claramente los sistemas de referencia involucrados. En esta convención, el superíndice izquierdo indica el sistema de referencia desde el cual se observa, mientras que el subíndice derecho corresponde al sistema o punto descrito. Bajo este criterio, un vector de posición expresado como ${}^iP_j$ representa la posición del punto "j" respecto al sistema de referencia "i". De manera similar, una matriz de transformación homogénea ${}^iT_j$ describe tanto la orientación como la posición del sistema "j" con respecto al sistema "i".


Esta notación será utilizada de forma consistente a lo largo del documento para el desarrollo de los modelos cinemáticos y dinámicos del robot.

## Variables simbólicas utilizadas en el documento

A lo largo de este documento se desarrollan los moldelos matemáticos siempre de manera simbólica, es decir, no se usan valores concretos, sino las variables propias que el robot requiere, estas variables son:

-  ${}^ix_j$ escrita en códgio como x\_i\_j 
-  ${}^iy_j$ escrita en códgio como y\_i\_j 
-  ${}^iz_j$ escrita en códgio como z\_i\_j 

Estas son las coordenadas de posición de un sistema de referencia j con respecto a un sistema de referencia i.

-  ${}^i{\gamma }_j$ escrita en códgio como g\_i\_j 
-  ${}^i{\beta }_j$ escrita en códgio como b\_i\_j 
-  ${}^i{\alpha }_j$ escrita en códgio como a\_i\_j 

Los anteriores son los ángulos de rotación sobre los ejes principales.Se definen de la siguiente forma: "g" refiere a gamma, ángulo de rotación sobre el eje x; "b" refiere a betha, ángulo de rotación sobre el eje y; "a" refiere a alpha, ángulo de rotación sobre el eje z.


A parte existen los ángulos de orientación de un sistema con respecto a otro, es decir, de un eslabón con respecto a otro. Por ejemplo:

-  ${}^0{\theta }_1$ escrita en códgio como  theta\_O\_1:  Ángulo del primer eslabón con respecto al sistema inercial (Origen)  
-  ${}^1{\theta }_2$ escrita en códgio como  theta\_1\_2:  Ángulo del segundo eslabón con respecto al primer eslabón  
-  ${}^2{\theta }_3$ escrita en códgio como  theta\_2\_3:  Ángulo del tercer eslabón con respecto al segundo eslabón 

Cabe mencionar que el punto P donde se encuentra el griper, se le asigna un sistema de referencia {P}, que es por conveniencia paralelo al sistema de referencia {3} del tercer eslabón. 


Por último, también se definen las literales L para describir las longitudes de los eslabones del robot:

-  $L_1$ escrita en códgio como L\_1: Longitud del primer eslabón 
-  $L_2$ escrita en códgio como L\_2: : Longitud del segundo eslabón 
-  $L_3$ escrita en códgio como L\_3:  Longitud del tercer eslabón 
# Definición simbólica de la matriz de transformación homogénea general

Para describir por completo la ubicación de un brazo manipulador se requiere información sobre su posicióin y sobre su orientación. La matriz de transformación homogénea (T) es una matriz que proporciona información sobre la posición y orientación del robot, y representa la descripción de un eje coordinado con respecto a otro, está formada por la matriz de rotación (R), el vector de posición (P), un vector de perspectiva nulo (0) y un factor de escala (1). De esta maqnera, la matriz de transformación homogénea en términos de su composición verctorial es de la siguiente forma:

 $$ ^i T_j =\left\lbrack \begin{array}{cc} ^i R_j  & ^i p_j \newline 0^T  & 1 \end{array}\right\rbrack $$ 

Para la cual, la definición de manera simbólica de cada vector es la siguiente:

```matlab

%Deficición de la función de manera simbolica
syms Tij(x_i_j,y_i_j,z_i_j,gi_j,bi_j,ai_j)

%Definición de la transformación homógenea general
Tij(x_i_j,y_i_j,z_i_j,gi_j,bi_j,ai_j) = [cos(ai_j)*cos(bi_j) cos(ai_j)*sin(bi_j)*sin(gi_j)-sin(ai_j)*cos(gi_j) sin(ai_j)*sin(gi_j)+cos(ai_j)*sin(bi_j)*cos(gi_j) x_i_j; sin(ai_j)*cos(bi_j) cos(ai_j)*cos(gi_j)+sin(ai_j)*sin(bi_j)*sin(gi_j) sin(ai_j)*sin(bi_j)*cos(gi_j)-cos(ai_j)*sin(gi_j) y_i_j; -sin(bi_j) cos(bi_j)*sin(gi_j) cos(bi_j)*cos(gi_j) z_i_j; 0 0 0 1]
```
Tij(x_i_j, y_i_j, z_i_j, gi_j, bi_j, ai_j) = 

  $$ \displaystyle \left(\begin{array}{cccc} \cos \left({\textrm{ai}}_j \right)\,\cos \left({\textrm{bi}}_j \right) & \cos \left({\textrm{ai}}_j \right)\,\sin \left({\textrm{bi}}_j \right)\,\sin \left({\textrm{gi}}_j \right)-\cos \left({\textrm{gi}}_j \right)\,\sin \left({\textrm{ai}}_j \right) & \sin \left({\textrm{ai}}_j \right)\,\sin \left({\textrm{gi}}_j \right)+\cos \left({\textrm{ai}}_j \right)\,\cos \left({\textrm{gi}}_j \right)\,\sin \left({\textrm{bi}}_j \right) & x_{i,j} \newline \cos \left({\textrm{bi}}_j \right)\,\sin \left({\textrm{ai}}_j \right) & \cos \left({\textrm{ai}}_j \right)\,\cos \left({\textrm{gi}}_j \right)+\sin \left({\textrm{ai}}_j \right)\,\sin \left({\textrm{bi}}_j \right)\,\sin \left({\textrm{gi}}_j \right) & \cos \left({\textrm{gi}}_j \right)\,\sin \left({\textrm{ai}}_j \right)\,\sin \left({\textrm{bi}}_j \right)-\cos \left({\textrm{ai}}_j \right)\,\sin \left({\textrm{gi}}_j \right) & y_{i,j} \newline -\sin \left({\textrm{bi}}_j \right) & \cos \left({\textrm{bi}}_j \right)\,\sin \left({\textrm{gi}}_j \right) & \cos \left({\textrm{bi}}_j \right)\,\cos \left({\textrm{gi}}_j \right) & z_{i,j} \newline 0 & 0 & 0 & 1 \end{array}\right) $$ 
 

# Modelado del un robot SCARA

![image_0.png](./ManzanoMartinezNancy_Examen1_media/image_0.png)

# Modelo cinemático de la postura 
## Modelo cinemático directo de la posición 
## $$ {}^0{\xi }_p={}^0{\xi }_p\left(q\right) $$

Del lado izquierdo de la expresión se muestra el vector de pose, y del lado derecho de la expresión se muestra el vector de postura. El primero indica cómo debe de estar un objeto cualquiera para que pueda ser tomado por el robot. El segundo indica cómo debe estar el brazo rbótico para que se pueda tomar este objeto. 


La mtariz de transformación que relaciona al sistema P con respecto al sistema inercial es:

 $$ {}^0T_P $$ 

Y se compone de la multiplicación de las matrices de transformación de los sistemas intermedios entre 0 y P.


A continuación se muestran las matrices de transformación homogénea para las transformaciones del sistema 1 con respecto a 0, 2 con respecto a 1, 3 con respecto a 2 y P con respecto a 3. Todas estas matrices implican una rotación sobre el eje Z.

```matlab
syms x_O_1 y_O_1 theta_O_1 L_2 theta_1_2 L_3 theta_2_3 L_1

T_O_1 = Tij(x_O_1,y_O_1,0,0,0,theta_O_1)
```
T_O_1 = 

  $$ \displaystyle \left(\begin{array}{cccc} \cos \left(\theta_{O,1} \right) & -\sin \left(\theta_{O,1} \right) & 0 & x_{O,1} \newline \sin \left(\theta_{O,1} \right) & \cos \left(\theta_{O,1} \right) & 0 & y_{O,1} \newline 0 & 0 & 1 & 0\newline 0 & 0 & 0 & 1 \end{array}\right) $$ 
 

La matriz anterior representa la transformación del sistema cero o inercial al sistema 1, correspondiente al primer eslabón, otorga la posicón del primer eslabón con respecto al origen, así como su orientación. En este caso "x\_O\_1" y "y\_O\_1" son las cooredenadas del vector P\_O\_1, que representa el desplazamiento del sistema {1} con respecto al origen o sistema {0}. 

```matlab
T_1_2 = Tij(L_1,0,0,0,0,theta_1_2)
```
T_1_2 = 

  $$ \displaystyle \left(\begin{array}{cccc} \cos \left(\theta_{1,2} \right) & -\sin \left(\theta_{1,2} \right) & 0 & L_1 \newline \sin \left(\theta_{1,2} \right) & \cos \left(\theta_{1,2} \right) & 0 & 0\newline 0 & 0 & 1 & 0\newline 0 & 0 & 0 & 1 \end{array}\right) $$ 
 

```matlab
T_2_3 = Tij(L_2,0,0,0,0,theta_2_3)
```
T_2_3 = 

  $$ \displaystyle \left(\begin{array}{cccc} \cos \left(\theta_{2,3} \right) & -\sin \left(\theta_{2,3} \right) & 0 & L_2 \newline \sin \left(\theta_{2,3} \right) & \cos \left(\theta_{2,3} \right) & 0 & 0\newline 0 & 0 & 1 & 0\newline 0 & 0 & 0 & 1 \end{array}\right) $$ 
 

Las dos matrices de transformación anteriores correponden a la del sistema {2} con respecto al {1} y la del sistema {3} con respecto al {2}, respectivamente. Estas dos matrices, tienen, en el espacio correspondientes al vector de posición, una logitud, sólo en el eje X. Estas longitudes son L1 y L2, correspondientes a las longitudes de los eslabones 1 y 2 repectivamente, estas representan los desplazamientos que tiene un sistema de referencia con respecto al anterior, y existen sólo en coordenada X porque los ejes de referencia fueron colocados, por conveniencia, con el eje X paralelo al vector formado por ambas juntas de cada eslabón.

```matlab
T_3_P = Tij(L_3,0,0,0,0,0)
```
T_3_P = 

  $$ \displaystyle \left(\begin{array}{cccc} 1 & 0 & 0 & L_3 \newline 0 & 1 & 0 & 0\newline 0 & 0 & 1 & 0\newline 0 & 0 & 0 & 1 \end{array}\right) $$ 
 

Nótese que la matriz de transformación del sistema {3} a {P} presenta una matriz identidad en el espacio correspondiente a la matriz de rotación, esto sucede ya que ambos sistemas son paralelos, y no existe cambio de orientación entre ellos.


La cinemática directa consiste en determinar la posición y orientación del punto P del robot a partir de las variables articulares. Esto se logra multiplicando las matrices de transformación de cada eslabón, se acumulan los efectos de cada rotación y desplazamiento a lo largo de la cadena cinemática. Una vez definidas las matrices de transformación anteriores se puede obtener el resultado simbólico de la matriz de transformación de P con respecto a 0. Esta matriz es la transformación directa desde el sistema inercial hasta el efector final o punto P.

```matlab
T_O_P = simplify(T_O_1*T_1_2*T_2_3*T_3_P)
```
T_O_P = 

  $$ \displaystyle \begin{array}{l} \left(\begin{array}{cccc} \sigma_2  & -\sigma_1  & 0 & x_{O,1} +L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)+L_1 \,\cos \left(\theta_{O,1} \right)+L_3 \,\sigma_2 \newline \sigma_1  & \sigma_2  & 0 & y_{O,1} +L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)+L_1 \,\sin \left(\theta_{O,1} \right)+L_3 \,\sigma_1 \newline 0 & 0 & 1 & 0\newline 0 & 0 & 0 & 1 \end{array}\right)\\\mathrm{}\\\textrm{where}\\\mathrm{}\\\;\;\sigma_1 =\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_2 =\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\end{array} $$ 
 

**Vector de postura del robot**


El vector ξ describe la posición y orientación de la punta del robot o punto P, de esta forma, representa la postura del robot en el espacio cartesiano. A su vez este vector se compone de otros dos vectores, un vector de posición y un vector de orientación. El primero describe la ubicación del punto final "P" del robot en el espacio, mientras que el segundo indica cómo está orientado dicho punto. A continuación se muestra el vector de postura general de un robot. 

 $$ {}^0{\xi }_P\;\left(q\right)=\left\lbrack \begin{array}{c} {}^0P_P\;\left(q\right)\newline {}^0{\theta }_P\left(q\right) \end{array}\right\rbrack $$ 

En estrictos términos los vectores de posición y orientación tienen ambos 3 componentes: El vector de posición se compone de las tres coordenas espaciales X, Y y Z, y el vector de orientación se compone de los tres ángulos de rotación sobre los tres ejes. Para este caso especial de análisis, donde el robot opera en el plano, el vector de posición se define mediante las coordenadas X y Y, mientras que la orientación se describe mediante un único ángulo θ, correspondiente a la rotación alrededor del eje perpendicular al plano, es decir, eje Z. 

 $$ {}^0{\xi }_P\;\left(q\right)=\left\lbrack \begin{array}{c} {}^0X_P\left(q\right)\newline {}^0Y_P\left(q\right)\newline {}^0{\theta }_P\left(q\right) \end{array}\right\rbrack $$ 

La orientación del punto final del robot ( ${}^0{\theta }_P\left(q\right)$ ) se obtiene como la suma de los ángulos articulares debido a que todas las articulaciones generan rotaciones alrededor del mismo eje Z. Cada articulación produce una rotación en el plano, por lo que la orientación de cada eslabón se define respecto al anterior. Como consecuencia, la orientación total del sistema corresponde a la acumulación de dichas rotaciones.


**Cálculo modelo cinemático directo de la posición**


A continuación se muestra el vector de postura simbólico del robot en estudio. Los dos primeros términos corresponden a la posición en el plano (x,y), mientras que el tercer término representa la orientación total, obtenida como la suma de los ángulos articulares.

```matlab
xi_O_P = [T_O_P(1,4);T_O_P(2,4);theta_O_1+theta_1_2+theta_2_3]
```
xi_O_P = 

  $$ \displaystyle \left(\begin{array}{c} x_{O,1} +L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)+L_1 \,\cos \left(\theta_{O,1} \right)+L_3 \,\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\newline y_{O,1} +L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)+L_1 \,\sin \left(\theta_{O,1} \right)+L_3 \,\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\newline \theta_{1,2} +\theta_{2,3} +\theta_{O,1}  \end{array}\right) $$ 
 
## Modelo cinemático inverso de la posición

El modelo cinemático directo de la posición tiene como objetivo calcular la posición y orientación final del efector terminal a partir de valores conocidos para cada articulación (q\_1, q\_2, q\_3). Matemáticamente, se resuelve mediante la propagación de transformaciones homogéneas, que resulta en una solución analítica única y directa.


El modelo cinemático inverso de la posición consiste en determinar las variables articulares del robot a partir de una posición y orientación deseada de la punta del mismo, punto P. A diferencia del modelo directo, este enfrenta ecuaciones no lineales y trascendentes que pueden presentar múltiples soluciones o, en su caso, carecer de ellas si el punto está fuera del espacio de trabajo, en este trabajo la solución del problema inverso se obtiene mediante una descomposición geométrica del sistema, tal como lo plantea Craig J. en su libro Introduction to robotics \[1\].


![image_1.png](./ManzanoMartinezNancy_Examen1_media/image_1.png)


Para el caso directo se cuenta con las entradas X, Y y  θ, que se observan perfectamente en la matriz de transformación final. Para el caso indirecto se desean obtener estas variables, es decir, que se necesita encontrar la solución al sistema de ecuaciones siguiente: 

 $$ \left\lbrack \begin{array}{c} {}^0X_1+L_1 \cos \left({}^0{\theta }_1\right)+L_2 \cos \left({}^0{\theta }_1+{}^1{\theta }_2\right)+L_3 \cos \left({}^0{\theta }_1+{}^1{\theta }_2+{}^2{\theta }_3\right)\newline {}^0Y_1+L_1 \textrm{sen}\left({}^0{\theta }_1\right)+L_2 \textrm{sen}\left({}^0{\theta }_1+{}^1{\theta }_2\right)+L_3 \textrm{sen}\left({}^0{\theta }_1+{}^1{\theta }_2+{}^2{\theta }_3\right)\newline {}^0{\theta }_1+{}^1{\theta }_2+{}^2{\theta }_3 \end{array}\right\rbrack =\left\lbrack \begin{array}{c} {}^0X_P\newline {}^0Y_P\newline {}^0{\theta }_P \end{array}\right\rbrack $$ 

Para realizar la descomposión geométrica, se obtiene, en primer lugar, la posición del centro de la articulación de la muñeca (origen del sistema {3}) para simplificar el análisis de la cadena cinemática a sólo 2 eslabones, eliminando el efecto de la longitud final L\_3. Esta  posición se obtiene de:

 $$ {}^0X_3={}^0X_P-L_3 \;\cos \left({}^0{\theta }_P\right) $$ 

 $$ {}^0Y_3={}^0Y_P-L_3 \;\textrm{sen}\left({}^0{\theta }_P\right) $$ 
```matlab
syms x_O_3 y_O_3 x_O_P y_O_P theta_O_P

x_O_3 = x_O_P - L_3*cos(theta_O_P)
```
x_O_3 = 
 $\displaystyle x_{O,P} -L_3 \,\cos \left(\theta_{O,P} \right)$
 

```matlab
y_O_3 = y_O_P - L_3*sin(theta_O_P)
```
y_O_3 = 
 $\displaystyle y_{O,P} -L_3 \,\sin \left(\theta_{O,P} \right)$
 

Una vez posicionado el sistema {3} el problema se reduce a un análisis de geometría plana para encontrar los ángulos $\theta_1$ y $\theta_2$, el cual puede resolverse mediante relaciones geométricas.  


 **Obtención de** ${}^0{\theta }_1$ 


Analizando los ángulo se sabe que:

 $$ {}^0{\theta }_1=\alpha +\psi $$ 

-  $\displaystyle \psi =\textrm{arctan2}\;\left(\frac{{}^0Y_3}{{}^0X_3}\right)$ 
-  $\displaystyle \alpha =\textrm{arcos}\;\left(\frac{L_1^2 +{\left\|{}^1P_3\right\|}^2 -L_2^2 }{2L_1 \;\left\|{}^1P_3\right\|}\right):\textrm{Según}\;\textrm{el}\;\textrm{despeje}\;\textrm{de}\;\alpha \;\textrm{de}\;\textrm{la}\;\textrm{ley}\;\textrm{de}\;\textrm{cosenos}$ 

 **Obtención de** ${}^1{\theta }_2$ 

 $$ {}^1{\theta }_2=\pi -\beta $$ 

-  $\displaystyle \beta =\textrm{arcsen}\;\left(\frac{\textrm{sen}\left(\alpha \right)\;\left\|{}^1P_3\right\|}{L_2 }\right):\textrm{Según}\;\textrm{el}\;\textrm{despeje}\;\textrm{de}\;\beta \;\textrm{de}\;\textrm{la}\;\textrm{ley}\;\textrm{de}\;\textrm{senos}$ 

 **Obtención de** ${}^2{\theta }_3$ 


Este último ángulo se obtiene de la relación directa de la ecuación 3 del sistema de ecuaciones:


Si: ${}^0{\theta }_1+{}^1{\theta }_2+{}^2{\theta }_3={}^0{\theta }_P$ 


Entonces: ${}^2{\theta }_3={}^0{\theta }_P-{}^0{\theta }_1-{}^1{\theta }_2$ 


**Código para obtención de ángulos**

```matlab
% distancia entre el sistema {3} y {1}
syms norm_1_3 theta_O_1_inv theta_1_2_inv theta_2_3_inv theta_O_P_inv

% Cálculo de theta_0_1 
psi = atan2(y_O_3, x_O_3); 
alfa = acos((L_1^2 + norm_1_3^2 - L_2^2) / (2 * L_1 * norm_1_3));
theta_O_1_inv = psi + alfa; 

% cálculo de tehta_1_2:
beta = asin((sin(alfa) * norm_1_3) / L_2);
theta_1_2_inv = pi - beta;

% cálculo de thata_2_3
theta_2_3_inv = theta_O_P_inv - theta_O_1_inv - theta_1_2_inv;

% Mostrar resultados
theta_O_1_inv= simplify(theta_O_1_inv)
```
theta_O_1_inv = 
 $\displaystyle \textrm{acos}\left(\frac{{L_1 }^2 -{L_2 }^2 +{{\textrm{norm}}_{1,3} }^2 }{2\,L_1 \,{\textrm{norm}}_{1,3} }\right)+\textrm{atan2}\left(y_{O,P} -L_3 \,\sin \left(\theta_{O,P} \right),x_{O,P} -L_3 \,\cos \left(\theta_{O,P} \right)\right)$
 

```matlab
theta_1_2_inv= simplify(theta_1_2_inv)
```
theta_1_2_inv = 
 $\displaystyle \pi -\textrm{asin}\left(\frac{{\textrm{norm}}_{1,3} \,\sqrt{1-\frac{{{\left({L_1 }^2 -{L_2 }^2 +{{\textrm{norm}}_{1,3} }^2 \right)}}^2 }{4\,{L_1 }^2 \,{{\textrm{norm}}_{1,3} }^2 }}}{L_2 }\right)$
 

```matlab
theta_2_3_inv= simplify(theta_2_3_inv)
```
theta_2_3_inv = 
 $\displaystyle \theta_{O,P,\textrm{inv}} -\pi -\textrm{acos}\left(\frac{{L_1 }^2 -{L_2 }^2 +{{\textrm{norm}}_{1,3} }^2 }{2\,L_1 \,{\textrm{norm}}_{1,3} }\right)-\textrm{atan2}\left(y_{O,P} -L_3 \,\sin \left(\theta_{O,P} \right),x_{O,P} -L_3 \,\cos \left(\theta_{O,P} \right)\right)+\textrm{asin}\left(\frac{{\textrm{norm}}_{1,3} \,\sqrt{1-\frac{{{\left({L_1 }^2 -{L_2 }^2 +{{\textrm{norm}}_{1,3} }^2 \right)}}^2 }{4\,{L_1 }^2 \,{{\textrm{norm}}_{1,3} }^2 }}}{L_2 }\right)$
 

# Modelo Cinemático de las velocidades 
## Modelo cinemático directo de las velocidades

El modelo cinemático directo de velocidades define la relación lineal entre las velocidades de las articulaciones del robot $\dot{q}$ y la velocidad resultante del efector terminal en el espacio cartesiano $\dot{\xi}$. Esta relación se expresa mediante la matriz jacobiana J, que actúa como un operador de transformación que depende de la configuración actual del robot (q). De cierta forma, al igual que en el modelo cinemático directo de la posición, se desea conocer cómo se mueve la punta del robot o punto P, a partir de que ya se conoce cómo se mueven las articulaciones del brazo robótico. Es decir que, se tienen las siguientes entradas y salidas:

-  Entradas: Velocidades de las articulaciones ( ${}^0{\dot{\theta} }_1,\;{}^1{\dot{\theta} }_2,\;{}^2{\dot{\theta} }_3$ ). 
-  Salida: Velocidad lineal ( ${}^0{\dot{x} }_P$, ${}^0{\dot{y} }_P$ ) y angular ( ${}^0{\dot{\theta} }_P$ ) de la punta del robot, o efector (P). 

El modelo cinemático directo de las velocidades es el siguiente: 

## $$ {}^0{\dot{\xi} }_P=J_{\theta } \left(q\right)\;\dot{q} $$

Para el cual se cumple que:

 $$ {}^0{\dot{\xi} }_P=\left\lbrack \begin{array}{c} {}^0{\dot{x} }_P\newline {}^0{\dot{y} }_P\newline {}^0{\dot{\theta} }_P \end{array}\right\rbrack $$ 

que surge de derivar el vector de pose de un robot. Y también se cumple que:

 $$ \dot{q} =\left\lbrack \begin{array}{c} {}^0{\dot{\theta} }_1\newline {}^1{\dot{\theta} }_2\newline {}^2{\dot{\theta} }_3 \end{array}\right\rbrack $$ 
```matlab
syms theta_dot_O_1 theta_dot_1_2 theta_dot_2_3
q_dot = [theta_dot_O_1; theta_dot_1_2; theta_dot_2_3]
```
q_dot = 

  $$ \displaystyle \left(\begin{array}{c} {\dot{\theta} }_{O,1} \newline {\dot{\theta} }_{1,2} \newline {\dot{\theta} }_{2,3}  \end{array}\right) $$ 
 

Es decir, para este modelo cinemático de las velocidades, $\dot{\xi}$ representa el vector de velocidades en el espacio cartesiano, mientras que $\dot{q}$ � corresponde a las velocidades articulares. 


**Jacobiano**


El jacobiano se construye a partir de las derivadas parciales de las ecuaciones de posición y orientación con respecto a las variables articulares. El vector de posición y orientación puede expresarse como una función de las variables articulares. Al derivar estas ecuaciones, se obtiene una relación lineal entre las velocidades articulares y las velocidades del efector del robot. De esta forma, mediante el jacobiano se convierten las velocidades de las articulaciones del robot, en velocidades del efector del robot.

 $$ J=\frac{\partial \xi }{\partial q} $$ 

En este caso, el Jacobiano permite determinar cómo influyen los movimientos individuales de cada articulación en el desplazamiento total del robot. Es decir, describe la contribución de cada variable articular a la velocidad del sistema.

```matlab
J_theta = jacobian(xi_O_P,[theta_O_1, theta_1_2, theta_2_3])
```
J_theta = 

  $$ \displaystyle \begin{array}{l} \left(\begin{array}{ccc} -L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)-L_1 \,\sin \left(\theta_{O,1} \right)-\sigma_1  & -L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)-\sigma_1  & -\sigma_1 \newline L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)+L_1 \,\cos \left(\theta_{O,1} \right)+\sigma_2  & L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)+\sigma_2  & \sigma_2 \newline 1 & 1 & 1 \end{array}\right)\\\mathrm{}\\\textrm{where}\\\mathrm{}\\\;\;\sigma_1 =L_3 \,\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_2 =L_3 \,\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\end{array} $$ 
 

**Cálculo del modelo directo de las velocidades**

```matlab
xi_dot_O_P = simplify(J_theta*q_dot)
```
xi_dot_O_P = 

  $$ \displaystyle \begin{array}{l} \left(\begin{array}{c} -{\dot{\theta} }_{O,1} \,{\left(\sigma_3 +L_1 \,\sin \left(\theta_{O,1} \right)+\sigma_1 \right)}-{\dot{\theta} }_{1,2} \,{\left(\sigma_3 +\sigma_1 \right)}-L_3 \,{\dot{\theta} }_{2,3} \,\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\newline {\dot{\theta} }_{1,2} \,{\left(\sigma_4 +\sigma_2 \right)}+{\dot{\theta} }_{O,1} \,{\left(\sigma_4 +L_1 \,\cos \left(\theta_{O,1} \right)+\sigma_2 \right)}+L_3 \,{\dot{\theta} }_{2,3} \,\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\newline {\dot{\theta} }_{1,2} +{\dot{\theta} }_{2,3} +{\dot{\theta} }_{O,1}  \end{array}\right)\\\mathrm{}\\\textrm{where}\\\mathrm{}\\\;\;\sigma_1 =L_3 \,\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_2 =L_3 \,\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_3 =L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_4 =L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\end{array} $$ 
 
## Modelo cinemático inverso de las velocidades

El modelo inverso (o indirecto) de velocidades permite calcular qué tan rápido deben moverse las articulaciones para conseguir una velocidad específica en la punta del robot. Esto es crítico para el control de trayectorias suaves. Matemáticamente, si la matriz Jacobiana es cuadrada y no singular, se resuelve mediante la matriz inversa:

## $$ \dot{q} =J_{\theta }^{-1} \left(q\right)\;{}^0{\dot{\xi} }_P $$

En el modelo inverso de las velocidades del robot se quiere obtener $\dot{q}$. Es decir, calcular qué velocidades deben tener las articulaciones para que el efector final se mueva con una velocidad específica. Este modelo permite usarse en aplicaciones donde se necesita que el robot siga una línea recta a velocidad constante, ya que permite ajustar dinámicamente la velocidad de cada motor.

-  Entradas: Velocidad lineal ( ${}^0{\dot{x} }_P$, ${}^0{\dot{y} }_P$ ) y angular ( ${}^0{\dot{\theta} }_P$ ) de la punta del robot, o efector (P). 
-  Salida:Velocidades de las articulaciones ( ${}^0{\dot{\theta} }_1,\;{}^1{\dot{\theta} }_2,\;{}^2{\dot{\theta} }_3$ ). 

 **Inverso del jacobiano** 


Para resolver el problema inverso de velocidades, en el cual se desea calcular las velocidades articulares necesarias para lograr un movimiento específico en el espacio cartesiano, se invierte el jacobiano, siempre y cuando este sea invertible.


Cabe mencionar que existen configuraciones en las que el Jacobiano pierde rango, lo que implica que no puede invertirse. Estas configuraciones se conocen como singularidades y representan condiciones en las que el robot pierde la capacidad de moverse en ciertas direcciones o requiere velocidades articulares muy elevadas para generar un movimiento determinado.

```matlab
inv(J_theta)
```
ans = 

  $$ \displaystyle \begin{array}{l} \left(\begin{array}{ccc} -\frac{\cos \left(\theta_{1,2} +\theta_{O,1} \right)}{\sigma_2 } & -\frac{\sin \left(\theta_{1,2} +\theta_{O,1} \right)}{\sigma_2 } & \frac{L_3 \,\sigma_4 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)-L_3 \,\sigma_3 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)}{\sigma_2 }\newline \frac{L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)+L_1 \,\cos \left(\theta_{O,1} \right)}{\sigma_1 } & \frac{L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)+L_1 \,\sin \left(\theta_{O,1} \right)}{\sigma_1 } & -\frac{L_1 \,L_3 \,\sigma_4 \,\sin \left(\theta_{O,1} \right)-L_1 \,L_3 \,\sigma_3 \,\cos \left(\theta_{O,1} \right)+L_2 \,L_3 \,\sigma_4 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)-L_2 \,L_3 \,\sigma_3 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)}{\sigma_1 }\newline -\frac{\cos \left(\theta_{O,1} \right)}{\sigma_6 -\sigma_5 } & -\frac{\sin \left(\theta_{O,1} \right)}{\sigma_6 -\sigma_5 } & \frac{\sigma_6 -\sigma_5 +L_3 \,\sigma_4 \,\sin \left(\theta_{O,1} \right)-L_3 \,\sigma_3 \,\cos \left(\theta_{O,1} \right)}{\sigma_6 -\sigma_5 } \end{array}\right)\\\mathrm{}\\\textrm{where}\\\mathrm{}\\\;\;\sigma_1 =L_1 \,L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\,\sin \left(\theta_{O,1} \right)-L_1 \,L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\,\cos \left(\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_2 =L_1 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\,\sin \left(\theta_{O,1} \right)-L_1 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\,\cos \left(\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_3 =\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_4 =\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_5 =L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\,\cos \left(\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_6 =L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\,\sin \left(\theta_{O,1} \right)\end{array} $$ 
 

**Cálculo del modelo inverso de las velocidades**


Se hace la multiplicación del jacobiano por el vector ${}^O{\dot{\xi} }_P\;$. Se han declarado variables nueva para no sobreescribir valores sobre los resultados del modelo directo de las velocidades. 

```matlab
syms x_dot_O_P_inv y_dot_O_P_inv theta_dot_O_P_inv
xi_dot_O_P_inv = [x_dot_O_P_inv; y_dot_O_P_inv; theta_dot_O_P_inv]
```
xi_dot_O_P_inv = 

  $$ \displaystyle \left(\begin{array}{c} {\dot{x} }_{O,P,\textrm{inv}} \newline {\dot{y} }_{O,P,\textrm{inv}} \newline {\dot{\theta} }_{O,P,\textrm{inv}}  \end{array}\right) $$ 
 

```matlab
q_dot_inv= inv(J_theta)*xi_dot_O_P_inv
```
q_dot_inv = 

  $$ \displaystyle \begin{array}{l} \left(\begin{array}{c} \frac{{\dot{\theta} }_{O,P,\textrm{inv}} \,{\left(L_3 \,\sigma_4 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)-L_3 \,\sigma_3 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\right)}}{\sigma_2 }-\frac{{\dot{x} }_{O,P,\textrm{inv}} \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)}{\sigma_2 }-\frac{{\dot{y} }_{O,P,\textrm{inv}} \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)}{\sigma_2 }\newline \frac{{\dot{y} }_{O,P,\textrm{inv}} \,{\left(L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)+L_1 \,\sin \left(\theta_{O,1} \right)\right)}}{\sigma_1 }-\frac{{\dot{\theta} }_{O,P,\textrm{inv}} \,{\left(L_1 \,L_3 \,\sigma_4 \,\sin \left(\theta_{O,1} \right)-L_1 \,L_3 \,\sigma_3 \,\cos \left(\theta_{O,1} \right)+L_2 \,L_3 \,\sigma_4 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)-L_2 \,L_3 \,\sigma_3 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\right)}}{\sigma_1 }+\frac{{\dot{x} }_{O,P,\textrm{inv}} \,{\left(L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)+L_1 \,\cos \left(\theta_{O,1} \right)\right)}}{\sigma_1 }\newline \frac{{\dot{\theta} }_{O,P,\textrm{inv}} \,{\left(\sigma_6 -\sigma_5 +L_3 \,\sigma_4 \,\sin \left(\theta_{O,1} \right)-L_3 \,\sigma_3 \,\cos \left(\theta_{O,1} \right)\right)}}{\sigma_6 -\sigma_5 }-\frac{{\dot{x} }_{O,P,\textrm{inv}} \,\cos \left(\theta_{O,1} \right)}{\sigma_6 -\sigma_5 }-\frac{{\dot{y} }_{O,P,\textrm{inv}} \,\sin \left(\theta_{O,1} \right)}{\sigma_6 -\sigma_5 } \end{array}\right)\\\mathrm{}\\\textrm{where}\\\mathrm{}\\\;\;\sigma_1 =L_1 \,L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\,\sin \left(\theta_{O,1} \right)-L_1 \,L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\,\cos \left(\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_2 =L_1 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\,\sin \left(\theta_{O,1} \right)-L_1 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\,\cos \left(\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_3 =\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_4 =\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_5 =L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\,\cos \left(\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_6 =L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\,\sin \left(\theta_{O,1} \right)\end{array} $$ 
 

**Análisis de singularidades**


Las singularidades corresponden a configuraciones en las que el Jacobiano pierde rango y no puede invertirse. Esto ocurre cuando su determinante es igual a cero:

 $$ \det \;\left(J\right)=0 $$ 

En estas condiciones, el robot pierde la capacidad de moverse en ciertas direcciones o requiere velocidades articulares infinitas para lograr un movimiento determinado. En el caso de este robot Scara, una singularidad típica ocurre cuando los eslabones se encuentran alineados, es decir, cuando θ\_1\_2=0 o múltiplos de π, es decir, a 180°. De esta manera, existe singularidad si el ángulo relativo entre ellos cumple:

 $$ {}^1{\theta }_2=k\pi \;,k\in \mathbb{Z} $$ 

es decir, cuando ${}^1{\theta }_2=0,\pi ,2\pi \;\ldotp \ldotp \ldotp$ 


En estas configuraciones, los eslabones quedan colineales, ya sea completamente extendidos o plegados sobre sí mismos. Esto se puede observar en la imagen inferior, en donde el caso 1 representa a los eslabones con ${}^1{\theta }_2=0$, y el caso 2 representa la singularidad que se presenta cuando los eslabones están plegados.


![image_2.png](./ManzanoMartinezNancy_Examen1_media/image_2.png)


Desde el punto de vista matemático, esto provoca que términos como $\sin \left({}^1{\theta }_2\right)$ se anulen, haciendo que el determinante del Jacobiano sea cero, lo que implica que la matriz pierde rango y no puede invertirse. Físicamente, esto significa que el robot pierde la capacidad de generar movimiento en ciertas direcciones, ya que las contribuciones de las articulaciones dejan de ser independientes. Como consecuencia, algunos movimientos cartesianos requerirían velocidades articulares infinitas, lo cual no es físicamente posible.


**Cálculo de manipulabilidad**


El índice de manipulabilidad es una medida que permite evaluar la capacidad del robot para moverse en diferentes direcciones del espacio, a partir de una configuración articular específica.

 $$ W=\left|J_{\theta } \left(q\right)\right|=L_1 L_2 \;\sin \;\left({}^1{\theta }_2\right) $$ 

Desde un punto de vista físico, el índice de manipulabilidad representa qué tan uniformemente el robot puede moverse en todas las direcciones. Valores altos de W indican configuraciones en las que el robot tiene buena capacidad de movimiento y control, mientras que valores bajos indican limitaciones en ciertas direcciones.


Cuando el índice de manipulabilidad es igual a cero, el robot se encuentra en una configuración singular. En este caso, el Jacobiano pierde rango, lo que implica que el robot no puede generar movimiento en una o más direcciones del espacio, o bien requiere velocidades articulares infinitas para lograrlo.

```matlab
w = simplify(det(J_theta))
```
w = 
 $\displaystyle L_1 \,L_2 \,\sin \left(\theta_{1,2} \right)$
 
# Modelo cinemático de las aceleaciones
## Modelo cinemático directo de las aceleraciones
## $$ {}^0{\ddot{\xi} }_P={\dot{J} }_{\theta } \;\dot{q} +J_{\theta } \;\ddot{q} $$

Este modelo surge de la derivada del modelo cinemático de las velocidades:

 $$ {}^0{\ddot{\xi} }_P=\frac{d}{\textrm{dt}}\left(J_{\theta } \left(q\right)\;\dot{q} \right)\; $$ 

Para el cual se definen las aceleraciones articulares:

```matlab
syms theta_ddot_O_1 theta_ddot_1_2 theta_ddot_2_3
q_ddot = [theta_ddot_O_1; theta_ddot_1_2; theta_ddot_2_3]
```
q_ddot = 

  $$ \displaystyle \left(\begin{array}{c} {\ddot{\theta} }_{O,1} \newline {\ddot{\theta} }_{1,2} \newline {\ddot{\theta} }_{2,3}  \end{array}\right) $$ 
 

El modelo de las aceleraciones resulta en una suma de partes, donde realmente el segundo sumando representa a la parte directa. Es decir, a partir de aceleraciones articulares, se obtiene la aceleración del efector final. 

 $$ J_{\theta } \;\ddot{q} $$ 

Sin embargo, debido a que el Jacobiano depende de la configuración del robot, y esta a su vez es función del tiempo, el Jacobiano no es constante durante el movimiento. Por esto, el primer sumando aparece en el modelo porque el jacobiano cambia con el tiempo, y depende de la posición ( $q$ ) y de la velocidad ( $\dot{q}$ ).

 $$ {\dot{J} }_{\theta } \;\dot{q} $$ 

En consecuencia, la derivada del jacobiano es distinta de cero cuando el robot se encuentra en movimiento. El primer sumando depende tanto de la pstura del robot, como de las velocidades articulares. Desde el punto de vista físico, este término representa aceleraciones que surgen debido al cambio continuo en la geometría del sistema mientras el robot se mueve.  


En particular, si $\ddot{q} =0$ pero $\dot{q} \not= 0$, la aceleración de {P}  está determinada únicamente por el término ${\dot{J} }_{\theta } \;\dot{q}$, lo que refleja efectos asociados al movimiento curvilíneo del efector final. Por otro lado,  $\ddot{q} \not= 0$ pero $\dot{q} =0$, la aceleración depende exclusivamente de las aceleraciones articulares. Aunque las velocidades articulares sean constantes, y no exista su derivada, la configuración del sistema cambia continuamente en el tiempo, lo que modifica la dirección del movimiento del efector final.


En términos físicos, la aceleración no solo depende del cambio en la magnitud de la velocidad, sino también del cambio en su dirección. Por lo tanto, un movimiento con velocidad constante pero trayectoria curva implica necesariamente la existencia de aceleración. Por esta razón, el efector final puede experimentar aceleraciones incluso cuando las aceleraciones articulares son nulas, siempre que exista velocidad en las articulaciones.


Un ejemplo de este fenómeno es la aceleración centrípeta. Esta ocurre cuando el efector se desplaza a lo largo de una trayectoria circular o curvilínea, manteniendo constante la magnitud de su velocidad, pero cambiando continuamente su dirección. Como resultado, aunque no exista aceleración angular en las articulaciones, el efector final experimenta una aceleración dirigida hacia el centro de la trayectoria, es decir, una aceleración centrípeta.


**Cálculo derivada del jacobiano**


Esta expresión corresponde al cálculo de la derivada temporal del Jacobiano. Pero como ya se mencionó, esta depende de la posición ( $q$ ) y de la velocidad ( $\dot{q}$ ), su derivada respecto al tiempo se obtiene aplicando la regla de la cadena:

 $$ \dot{J} \left(q\right)=\frac{\partial \;J\left(q\right)}{\partial {}^0{\theta }_1}{}^0{\dot{\theta} }_1+\frac{\partial \;J\left(q\right)}{\partial {}^1{\theta }_2}{}^1{\dot{\theta} }_2+\frac{\partial \;J\left(q\right)}{\partial {}^2{\theta }_3}{}^2{\dot{\theta} }_3 $$ 

Cada término representa cómo cambia el Jacobiano con respecto a una articulación, ponderado por la velocidad de dicha articulación.


El uso de `simplify` permite reducir la expresión simbólica a una forma más compacta.

```matlab
J_dot_theta = simplify(diff(J_theta,theta_O_1)*theta_dot_O_1 + diff(J_theta,theta_1_2)*theta_dot_1_2 + diff(J_theta,theta_2_3)*theta_dot_2_3)
```
J_dot_theta = 

  $$ \displaystyle \begin{array}{l} \left(\begin{array}{ccc} -\sigma_1 -{\dot{\theta} }_{O,1} \,{\left(\sigma_4 +L_1 \,\cos \left(\theta_{O,1} \right)+L_3 \,\sigma_3 \right)}-L_3 \,{\dot{\theta} }_{2,3} \,\sigma_3  & -\sigma_1 -{\dot{\theta} }_{O,1} \,{\left(\sigma_4 +L_3 \,\sigma_3 \right)}-L_3 \,{\dot{\theta} }_{2,3} \,\sigma_3  & -L_3 \,\sigma_3 \,{\left({\dot{\theta} }_{1,2} +{\dot{\theta} }_{2,3} +{\dot{\theta} }_{O,1} \right)}\newline -{\dot{\theta} }_{O,1} \,{\left(\sigma_6 +L_1 \,\sin \left(\theta_{O,1} \right)+L_3 \,\sigma_5 \right)}-\sigma_2 -L_3 \,{\dot{\theta} }_{2,3} \,\sigma_5  & -\sigma_2 -{\dot{\theta} }_{O,1} \,{\left(\sigma_6 +L_3 \,\sigma_5 \right)}-L_3 \,{\dot{\theta} }_{2,3} \,\sigma_5  & -L_3 \,\sigma_5 \,{\left({\dot{\theta} }_{1,2} +{\dot{\theta} }_{2,3} +{\dot{\theta} }_{O,1} \right)}\newline 0 & 0 & 0 \end{array}\right)\\\mathrm{}\\\textrm{where}\\\mathrm{}\\\;\;\sigma_1 ={\dot{\theta} }_{1,2} \,{\left(\sigma_4 +L_3 \,\sigma_3 \right)}\\\mathrm{}\\\;\;\sigma_2 ={\dot{\theta} }_{1,2} \,{\left(\sigma_6 +L_3 \,\sigma_5 \right)}\\\mathrm{}\\\;\;\sigma_3 =\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_4 =L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_5 =\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_6 =L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\end{array} $$ 
 

**Cálculo del modelo de aceleración directa**

```matlab
xi_ddot_O_P = J_theta*q_ddot + J_dot_theta*q_dot
```
xi_ddot_O_P = 

  $$ \displaystyle \begin{array}{l} \left(\begin{array}{c} -{\ddot{\theta} }_{O,1} \,\sigma_1 -{\dot{\theta} }_{1,2} \,{\left(\sigma_3 +{\dot{\theta} }_{O,1} \,{\left(\sigma_6 +L_3 \,\sigma_5 \right)}+L_3 \,{\dot{\theta} }_{2,3} \,\sigma_5 \right)}-{\ddot{\theta} }_{1,2} \,{\left(\sigma_8 +L_3 \,\sigma_7 \right)}-{\dot{\theta} }_{O,1} \,{\left(\sigma_3 +{\dot{\theta} }_{O,1} \,\sigma_2 +L_3 \,{\dot{\theta} }_{2,3} \,\sigma_5 \right)}-L_3 \,{\ddot{\theta} }_{2,3} \,\sigma_7 -L_3 \,{\dot{\theta} }_{2,3} \,\sigma_5 \,{\left({\dot{\theta} }_{1,2} +{\dot{\theta} }_{2,3} +{\dot{\theta} }_{O,1} \right)}\newline {\ddot{\theta} }_{1,2} \,{\left(\sigma_6 +L_3 \,\sigma_5 \right)}-{\dot{\theta} }_{O,1} \,{\left({\dot{\theta} }_{O,1} \,\sigma_1 +\sigma_4 +L_3 \,{\dot{\theta} }_{2,3} \,\sigma_7 \right)}-{\dot{\theta} }_{1,2} \,{\left(\sigma_4 +{\dot{\theta} }_{O,1} \,{\left(\sigma_8 +L_3 \,\sigma_7 \right)}+L_3 \,{\dot{\theta} }_{2,3} \,\sigma_7 \right)}+{\ddot{\theta} }_{O,1} \,\sigma_2 +L_3 \,{\ddot{\theta} }_{2,3} \,\sigma_5 -L_3 \,{\dot{\theta} }_{2,3} \,\sigma_7 \,{\left({\dot{\theta} }_{1,2} +{\dot{\theta} }_{2,3} +{\dot{\theta} }_{O,1} \right)}\newline {\ddot{\theta} }_{1,2} +{\ddot{\theta} }_{2,3} +{\ddot{\theta} }_{O,1}  \end{array}\right)\\\mathrm{}\\\textrm{where}\\\mathrm{}\\\;\;\sigma_1 =\sigma_8 +L_1 \,\sin \left(\theta_{O,1} \right)+L_3 \,\sigma_7 \\\mathrm{}\\\;\;\sigma_2 =\sigma_6 +L_1 \,\cos \left(\theta_{O,1} \right)+L_3 \,\sigma_5 \\\mathrm{}\\\;\;\sigma_3 ={\dot{\theta} }_{1,2} \,{\left(\sigma_6 +L_3 \,\sigma_5 \right)}\\\mathrm{}\\\;\;\sigma_4 ={\dot{\theta} }_{1,2} \,{\left(\sigma_8 +L_3 \,\sigma_7 \right)}\\\mathrm{}\\\;\;\sigma_5 =\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_6 =L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_7 =\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_8 =L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\end{array} $$ 
 

```matlab
xi_ddot_O_P = simplify(J_theta*q_ddot + J_dot_theta*q_dot)
```
xi_ddot_O_P = 

  $$ \displaystyle \begin{array}{l} \left(\begin{array}{c} -L_1 \,{\ddot{\theta} }_{O,1} \,\sin \left(\theta_{O,1} \right)-L_2 \,{{\dot{\theta} }_{1,2} }^2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)-L_2 \,{{\dot{\theta} }_{O,1} }^2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)-L_3 \,{\ddot{\theta} }_{1,2} \,\sigma_1 -L_3 \,{\ddot{\theta} }_{2,3} \,\sigma_1 -L_3 \,{\ddot{\theta} }_{O,1} \,\sigma_1 -L_1 \,{{\dot{\theta} }_{O,1} }^2 \,\cos \left(\theta_{O,1} \right)-L_3 \,{{\dot{\theta} }_{1,2} }^2 \,\sigma_2 -L_3 \,{{\dot{\theta} }_{2,3} }^2 \,\sigma_2 -L_3 \,{{\dot{\theta} }_{O,1} }^2 \,\sigma_2 -L_2 \,{\ddot{\theta} }_{1,2} \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)-L_2 \,{\ddot{\theta} }_{O,1} \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)-2\,L_2 \,{\dot{\theta} }_{1,2} \,{\dot{\theta} }_{O,1} \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)-2\,L_3 \,{\dot{\theta} }_{1,2} \,{\dot{\theta} }_{2,3} \,\sigma_2 -2\,L_3 \,{\dot{\theta} }_{1,2} \,{\dot{\theta} }_{O,1} \,\sigma_2 -2\,L_3 \,{\dot{\theta} }_{2,3} \,{\dot{\theta} }_{O,1} \,\sigma_2 \newline L_3 \,{\ddot{\theta} }_{1,2} \,\sigma_2 -L_2 \,{{\dot{\theta} }_{O,1} }^2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)-L_2 \,{{\dot{\theta} }_{1,2} }^2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)+L_3 \,{\ddot{\theta} }_{2,3} \,\sigma_2 +L_3 \,{\ddot{\theta} }_{O,1} \,\sigma_2 -L_1 \,{{\dot{\theta} }_{O,1} }^2 \,\sin \left(\theta_{O,1} \right)-L_3 \,{{\dot{\theta} }_{1,2} }^2 \,\sigma_1 -L_3 \,{{\dot{\theta} }_{2,3} }^2 \,\sigma_1 -L_3 \,{{\dot{\theta} }_{O,1} }^2 \,\sigma_1 +L_2 \,{\ddot{\theta} }_{1,2} \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)+L_2 \,{\ddot{\theta} }_{O,1} \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)+L_1 \,{\ddot{\theta} }_{O,1} \,\cos \left(\theta_{O,1} \right)-2\,L_2 \,{\dot{\theta} }_{1,2} \,{\dot{\theta} }_{O,1} \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)-2\,L_3 \,{\dot{\theta} }_{1,2} \,{\dot{\theta} }_{2,3} \,\sigma_1 -2\,L_3 \,{\dot{\theta} }_{1,2} \,{\dot{\theta} }_{O,1} \,\sigma_1 -2\,L_3 \,{\dot{\theta} }_{2,3} \,{\dot{\theta} }_{O,1} \,\sigma_1 \newline {\ddot{\theta} }_{1,2} +{\ddot{\theta} }_{2,3} +{\ddot{\theta} }_{O,1}  \end{array}\right)\\\mathrm{}\\\textrm{where}\\\mathrm{}\\\;\;\sigma_1 =\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_2 =\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\end{array} $$ 
 
## Modelo cinemático indirecto de las aceleraciones 

El modelo cinemático inverso de las aceleraciones tiene como objetivo determinar las aceleraciones articulares $\ddot{q}$ a partir de la aceleración cartesiana del efector final $\ddot{\xi}$.

## $$ \;\ddot{q} =J_{\theta }^{-1} \;\left({}^0{\ddot{\xi} }_P-{\dot{J} }_{\theta } \;\dot{q} \right) $$

Este modelo solo es válido cuando el jacobiano es invertible, es decir, cuando el manipulador no se encuentra en una  configuració singular como las mencionadas en la sección anterior. En caso contrario, se utiliza la pseudoinversa del Jacobiano:

 $$ \ddot{q} =J_{\theta }^+ \;\left({}^0{\ddot{\xi} }_P-{\dot{J} }_{\theta } \;\dot{q} \right) $$ 

En el modelo: 

-  ${}^0{\ddot{\xi} }_P$ : Es la aceleración cartesiana deseada del efector final. 
-  $J_{\theta }^{-1}$: Transforma la aceleración del espacio cartesiano al espacio articular, obteniendo las aceleraciones necesarias en cada articulación. 
-  ${\dot{J} }_{\theta } \;\dot{q}$: Representa una aceleración interna del sistema, causada por el movimiento de las articulaciones. Este término incluye efectos como los vistos en la sección anterior: aceleraciones centrípetas y cambios en la dirección del movimiento 

Se define el vector ${}^0{\ddot{\xi} }_P$. se han definido variables nuevas para no sobreescribir sobre los resultados obtenidos en el modelo directo de las aceleraciones.

```matlab
syms x_ddot_O_P_inv y_ddot_O_P_inv theta_ddot_O_P_inv
xi_ddot_O_P_inv = [x_ddot_O_P_inv; y_ddot_O_P_inv; theta_ddot_O_P_inv]
```
xi_ddot_O_P_inv = 

  $$ \displaystyle \left(\begin{array}{c} {\ddot{x} }_{O,P,\textrm{inv}} \newline {\ddot{y} }_{O,P,\textrm{inv}} \newline {\ddot{\theta} }_{O,P,\textrm{inv}}  \end{array}\right) $$ 
 

```matlab
q_ddot=inv(J_theta)*(xi_ddot_O_P_inv-J_dot_theta*q_dot)
```
q_ddot = 

  $$ \displaystyle \begin{array}{l} \left(\begin{array}{c} \frac{{\ddot{\theta} }_{O,P,\textrm{inv}} \,{\left(L_3 \,\sigma_9 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)-L_3 \,\sigma_{11} \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\right)}}{\sigma_2 }-\frac{\sin \left(\theta_{1,2} +\theta_{O,1} \right)\,\sigma_6 }{\sigma_2 }-\frac{\cos \left(\theta_{1,2} +\theta_{O,1} \right)\,\sigma_5 }{\sigma_2 }\newline \frac{{\left(\sigma_{10} +L_1 \,\cos \left(\theta_{O,1} \right)\right)}\,\sigma_5 }{\sigma_1 }-\frac{{\ddot{\theta} }_{O,P,\textrm{inv}} \,{\left(L_1 \,L_3 \,\sigma_9 \,\sin \left(\theta_{O,1} \right)-L_1 \,L_3 \,\sigma_{11} \,\cos \left(\theta_{O,1} \right)+L_2 \,L_3 \,\sigma_9 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)-L_2 \,L_3 \,\sigma_{11} \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\right)}}{\sigma_1 }+\frac{{\left(\sigma_{12} +L_1 \,\sin \left(\theta_{O,1} \right)\right)}\,\sigma_6 }{\sigma_1 }\newline \frac{{\ddot{\theta} }_{O,P,\textrm{inv}} \,{\left(\sigma_4 -\sigma_3 +L_3 \,\sigma_9 \,\sin \left(\theta_{O,1} \right)-L_3 \,\sigma_{11} \,\cos \left(\theta_{O,1} \right)\right)}}{\sigma_4 -\sigma_3 }-\frac{\cos \left(\theta_{O,1} \right)\,\sigma_5 }{\sigma_4 -\sigma_3 }-\frac{\sin \left(\theta_{O,1} \right)\,\sigma_6 }{\sigma_4 -\sigma_3 } \end{array}\right)\\\mathrm{}\\\textrm{where}\\\mathrm{}\\\;\;\sigma_1 =L_1 \,L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\,\sin \left(\theta_{O,1} \right)-L_1 \,L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\,\cos \left(\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_2 =L_1 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\,\sin \left(\theta_{O,1} \right)-L_1 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\,\cos \left(\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_3 =L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\,\cos \left(\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_4 =L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\,\sin \left(\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_5 ={\ddot{x} }_{O,P,\textrm{inv}} +{\dot{\theta} }_{1,2} \,{\left(\sigma_7 +{\dot{\theta} }_{O,1} \,{\left(\sigma_{10} +L_3 \,\sigma_9 \right)}+L_3 \,{\dot{\theta} }_{2,3} \,\sigma_9 \right)}+{\dot{\theta} }_{O,1} \,{\left(\sigma_7 +{\dot{\theta} }_{O,1} \,{\left(\sigma_{10} +L_1 \,\cos \left(\theta_{O,1} \right)+L_3 \,\sigma_9 \right)}+L_3 \,{\dot{\theta} }_{2,3} \,\sigma_9 \right)}+L_3 \,{\dot{\theta} }_{2,3} \,\sigma_9 \,{\left({\dot{\theta} }_{1,2} +{\dot{\theta} }_{2,3} +{\dot{\theta} }_{O,1} \right)}\\\mathrm{}\\\;\;\sigma_6 ={\ddot{y} }_{O,P,\textrm{inv}} +{\dot{\theta} }_{O,1} \,{\left({\dot{\theta} }_{O,1} \,{\left(\sigma_{12} +L_1 \,\sin \left(\theta_{O,1} \right)+L_3 \,\sigma_{11} \right)}+\sigma_8 +L_3 \,{\dot{\theta} }_{2,3} \,\sigma_{11} \right)}+{\dot{\theta} }_{1,2} \,{\left(\sigma_8 +{\dot{\theta} }_{O,1} \,{\left(\sigma_{12} +L_3 \,\sigma_{11} \right)}+L_3 \,{\dot{\theta} }_{2,3} \,\sigma_{11} \right)}+L_3 \,{\dot{\theta} }_{2,3} \,\sigma_{11} \,{\left({\dot{\theta} }_{1,2} +{\dot{\theta} }_{2,3} +{\dot{\theta} }_{O,1} \right)}\\\mathrm{}\\\;\;\sigma_7 ={\dot{\theta} }_{1,2} \,{\left(\sigma_{10} +L_3 \,\sigma_9 \right)}\\\mathrm{}\\\;\;\sigma_8 ={\dot{\theta} }_{1,2} \,{\left(\sigma_{12} +L_3 \,\sigma_{11} \right)}\\\mathrm{}\\\;\;\sigma_9 =\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_{10} =L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_{11} =\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_{12} =L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\end{array} $$ 
 

El modelo inverso de aceleraciones indica que para lograr una aceleración deseada en el efector final, no basta con considerar únicamente dicha aceleración, sino que también es necesario compensar los efectos producidos por el movimiento actual del robot.


Es decir:

-  Si el robot ya está en movimiento, se deben aplicar aceleraciones adicionales para contrarrestar efectos como la aceleración centrípeta.  
-  Si no hay velocidad articular, el modelo se simplifica a: 

 $$ \ddot{q} =J_{\theta }^{-1} *{}^0{\ddot{\xi} }_P $$ 

# Modelo dinámico por ecuaciones de Eüler\-Lagrange
## Modelo dinámico directo del robot

Para el desarrollo del modelo dinámico del robot, se emplea el formalismo de Euler\-Lagrange, el cual se basa en el análisis de la energía del sistema. De aquí se obtienen ecuaciones de movimiento en función de las variables articulares, sus velocidades y aceleraciones.

## $$ \tau_{\theta } =M\left(q\right)+V\left(q,\dot{q} \right)+G\left(q\right)+J_{\theta } {\left(q\right)}^T F_{\textrm{ext}} $$

Para el modelo anterior, $\tau_{\theta }$ es el vector de esfuerzos o torques articulares, y depende de las variables articules del robot. Está compuesto por los esfuerzos totales de cada eslabón. 

 $$ \tau_i =\left\lbrack \begin{array}{c} \tau_{\theta 1} \newline \tau_{\theta 2} \newline \tau_{\theta 3}  \end{array}\right\rbrack $$ 

Para calcular los pares del sistema se aplica la ecuación de par de Euler\-Lagrange para cada q\_i. Este paso genera un conjunto de ecuaciones diferenciales que describen el comportamiento dinámico del robot:

 $$ \tau_{i\;} =\frac{d}{\textrm{dt}}\left(\frac{\partial }{\partial \dot{q_i } }\;\Gamma \right)-\frac{\partial }{\partial q_{i\;} }\;\Gamma $$ 

donde:

-  $\tau_{i\;}$ es el torque aplicado en la articulación i. 
-  $q_{i\;,\;} \dot{q_i \;} y\;\ddot{q_i }$ son posición, velocidad y aceleración articular  

Para la ecuación de Par, el lagrangiano se define a partir de las energías cinética y potencial:

 $$ \Gamma =\sum_{i=1}^n k_{i\;} -\sum_{i=1}^n u_{i\;} $$ 

Que para este sistema en específico, de tres eslabones, la suma es únicamente de tres valores:

 $$ \Gamma =\left(k_{1\;} +k_{2\;} +k_{3\;} \right)-\left(u_{1\;} +u_{2\;} +u_{3\;} \right) $$ 

Esta función permite describir completamente la dinámica del sistema en términos de energía.


**Energía cinética y potencial para el cálculo del lagrangiano**


Se empleará las siguiente ecuación para el cálculo de la energía cinética:

 $$ k_i =\frac{m_i }{2}{\mathbf{v}}_{C_i }^T {\mathbf{v}}_{C_i } +\frac{1}{2}{\mathbf{\omega }}_{C_i }^T {\mathbf{I}}_{C_i } {\mathbf{\omega }}_{C_i } $$ 

Donde: 

-  $m_i$: masa del eslabón  
-  $V_{\textrm{ci}\;}$: velocidad del centro de masa  
-  $\omega_{\textrm{ci}}$: velocidad angular  
-  $I_{\textrm{ci}}$: tensor de inercia respecto al centro de masa 

Y para el cálculo de la energía potencial:

 $$ u_{i\;} =-m_{i\;} g^{T\;} {}^0P_{\textrm{c1}} $$ 

Donde:

-  $m_i$: masa del eslabón  
-  $g$: vector de gravedad  
-  ${}^0P_{\textrm{ci}}$: posición del centro de masa del eslabón 

Ahora, para poder obtener el lagrangiano y este permita obtener la ecuación de par, primero se necesitan obtener los parámetros involucrados en las ecuaciones de energía cinética y potencial.


```matlab
syms x_1_C1 theta_dot_O_1

v_C1_C1 = [0;x_1_C1*theta_dot_O_1;0]
```
v_C1_C1 = 

  $$ \displaystyle \left(\begin{array}{c} 0\newline {\dot{\theta} }_{O,1} \,x_{1,\textrm{C1}} \newline 0 \end{array}\right) $$ 
 

```matlab

v_O_C1 = [-x_1_C1*sin(theta_O_1)*theta_dot_O_1;x_1_C1*cos(theta_O_1)*theta_dot_O_1;0]
```
v_O_C1 = 

  $$ \displaystyle \left(\begin{array}{c} -{\dot{\theta} }_{O,1} \,x_{1,\textrm{C1}} \,\sin \left(\theta_{O,1} \right)\newline {\dot{\theta} }_{O,1} \,x_{1,\textrm{C1}} \,\cos \left(\theta_{O,1} \right)\newline 0 \end{array}\right) $$ 
 

```matlab
transpose(v_C1_C1)*v_C1_C1
```
ans = 
 $\displaystyle {{\dot{\theta} }_{O,1} }^2 \,{x_{1,\textrm{C1}} }^2 $
 

```matlab
simplify(transpose(v_O_C1)*v_O_C1)
```
ans = 
 $\displaystyle {{\dot{\theta} }_{O,1} }^2 \,{x_{1,\textrm{C1}} }^2 $
 


# Cálculo de la posición de los centros de masa

El cálculo de los centros de masa de cada eslabón se realiza con el objetivo de determinar la posición de la masa concentrada en el espacio. Para ello, se define inicialmente la posición del centro de masa en el sistema de referencia local de cada eslabón, y posteriormente se transforma al sistema de referencia inercial mediante matrices de transformación homogénea. Esta representación es fundamental para el desarrollo del modelo dinámico, ya que permite calcular las energías cinética y potencial del sistema.


A continuación se define la matriz de transformación local para el centro de masa del eslabón 1, y posteriormente se premultiplica por la matriz de transformación del eslabón 1 con respecto al inercial, que fue definida previamente al incio de este documento. De esta multiplicación se obtiene la matriz de transformación del centro de masa 1 con respecto al sistema inercial.

```matlab
syms x_1_C1 x_2_C2 x_3_C3

T_1_C1 = Tij(x_1_C1,0,0,0,0,0)
```
T_1_C1 = 

  $$ \displaystyle \left(\begin{array}{cccc} 1 & 0 & 0 & x_{1,\textrm{C1}} \newline 0 & 1 & 0 & 0\newline 0 & 0 & 1 & 0\newline 0 & 0 & 0 & 1 \end{array}\right) $$ 
 

```matlab
T_O_C1 = T_O_1*T_1_C1
```
T_O_C1 = 

  $$ \displaystyle \left(\begin{array}{cccc} \cos \left(\theta_{O,1} \right) & -\sin \left(\theta_{O,1} \right) & 0 & x_{O,1} +x_{1,\textrm{C1}} \,\cos \left(\theta_{O,1} \right)\newline \sin \left(\theta_{O,1} \right) & \cos \left(\theta_{O,1} \right) & 0 & y_{O,1} +x_{1,\textrm{C1}} \,\sin \left(\theta_{O,1} \right)\newline 0 & 0 & 1 & 0\newline 0 & 0 & 0 & 1 \end{array}\right) $$ 
 

El siguiente cálculo corresponde a la matriz de transformación local para el centro de masa del eslabón 2, que posteriormente se premultiplica por la matriz de transformación del eslabón 2 con respecto al 1, y del eslabón 1 con respeto al inercial. De esta multiplicación se obtiene la matriz de transformación del centro de masa 2 con respecto al sistema inercial.

```matlab
T_2_C2 = Tij(x_2_C2,0,0,0,0,0)
```
T_2_C2 = 

  $$ \displaystyle \left(\begin{array}{cccc} 1 & 0 & 0 & x_{2,\textrm{C2}} \newline 0 & 1 & 0 & 0\newline 0 & 0 & 1 & 0\newline 0 & 0 & 0 & 1 \end{array}\right) $$ 
 

```matlab
T_O_C2 = T_O_1*T_1_2*T_2_C2
```
T_O_C2 = 

  $$ \displaystyle \begin{array}{l} \left(\begin{array}{cccc} \sigma_1  & -\cos \left(\theta_{1,2} \right)\,\sin \left(\theta_{O,1} \right)-\cos \left(\theta_{O,1} \right)\,\sin \left(\theta_{1,2} \right) & 0 & x_{O,1} +x_{2,\textrm{C2}} \,\sigma_1 +L_1 \,\cos \left(\theta_{O,1} \right)\newline \sigma_2  & \sigma_1  & 0 & y_{O,1} +x_{2,\textrm{C2}} \,\sigma_2 +L_1 \,\sin \left(\theta_{O,1} \right)\newline 0 & 0 & 1 & 0\newline 0 & 0 & 0 & 1 \end{array}\right)\\\mathrm{}\\\textrm{where}\\\mathrm{}\\\;\;\sigma_1 =\cos \left(\theta_{1,2} \right)\,\cos \left(\theta_{O,1} \right)-\sin \left(\theta_{1,2} \right)\,\sin \left(\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_2 =\cos \left(\theta_{1,2} \right)\,\sin \left(\theta_{O,1} \right)+\cos \left(\theta_{O,1} \right)\,\sin \left(\theta_{1,2} \right)\end{array} $$ 
 

Por último se calcula la matriz de transformación del centro de masa 3 con respecto al sistema inercial. Para esto, la matriz de transformación local para el centro de masa del eslabón 3, se premultiplica por la matriz de transformación del eslabón 3 con respecto al 2, del eslabón 2 con respecto al 1, y del eslabón 1 con respeto al inercial.

```matlab
T_3_C3 = Tij(x_3_C3,0,0,0,0,0)
```
T_3_C3 = 

  $$ \displaystyle \left(\begin{array}{cccc} 1 & 0 & 0 & x_{3,\textrm{C3}} \newline 0 & 1 & 0 & 0\newline 0 & 0 & 1 & 0\newline 0 & 0 & 0 & 1 \end{array}\right) $$ 
 

```matlab
T_O_C3 = T_O_1*T_1_2*T_2_3*T_3_C3
```
T_O_C3 = 

  $$ \displaystyle \begin{array}{l} \left(\begin{array}{cccc} \sigma_1  & -\cos \left(\theta_{2,3} \right)\,\sigma_4 -\sin \left(\theta_{2,3} \right)\,\sigma_3  & 0 & x_{O,1} +L_2 \,\sigma_3 +L_1 \,\cos \left(\theta_{O,1} \right)+x_{3,\textrm{C3}} \,\sigma_1 \newline \sigma_2  & \sigma_1  & 0 & y_{O,1} +L_2 \,\sigma_4 +L_1 \,\sin \left(\theta_{O,1} \right)+x_{3,\textrm{C3}} \,\sigma_2 \newline 0 & 0 & 1 & 0\newline 0 & 0 & 0 & 1 \end{array}\right)\\\mathrm{}\\\textrm{where}\\\mathrm{}\\\;\;\sigma_1 =\cos \left(\theta_{2,3} \right)\,\sigma_3 -\sin \left(\theta_{2,3} \right)\,\sigma_4 \\\mathrm{}\\\;\;\sigma_2 =\cos \left(\theta_{2,3} \right)\,\sigma_4 +\sin \left(\theta_{2,3} \right)\,\sigma_3 \\\mathrm{}\\\;\;\sigma_3 =\cos \left(\theta_{1,2} \right)\,\cos \left(\theta_{O,1} \right)-\sin \left(\theta_{1,2} \right)\,\sin \left(\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_4 =\cos \left(\theta_{1,2} \right)\,\sin \left(\theta_{O,1} \right)+\cos \left(\theta_{O,1} \right)\,\sin \left(\theta_{1,2} \right)\end{array} $$ 
 

**Vectores de posición de los centros de masa**


Recordando que las matrices de transformación contienen en ellas a un vector de posición, y ya una vez obtenido las tres matrices de transformación correspondientes a los tres centros de masa del sistema, ahora se pueden obtener los vectores de posición de cada centro de masa. Todos estos vectores se definen con respecto al origen. 

```matlab
%Vectores de posición
p_O_C1 = [T_O_C1(1,4);T_O_C1(2,4);T_O_C1(3,4)]
```
p_O_C1 = 

  $$ \displaystyle \left(\begin{array}{c} x_{O,1} +x_{1,\textrm{C1}} \,\cos \left(\theta_{O,1} \right)\newline y_{O,1} +x_{1,\textrm{C1}} \,\sin \left(\theta_{O,1} \right)\newline 0 \end{array}\right) $$ 
 

```matlab
p_O_C2 = simplify([T_O_C2(1,4);T_O_C2(2,4);T_O_C2(3,4)])
```
p_O_C2 = 

  $$ \displaystyle \left(\begin{array}{c} x_{O,1} +x_{2,\textrm{C2}} \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)+L_1 \,\cos \left(\theta_{O,1} \right)\newline y_{O,1} +x_{2,\textrm{C2}} \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)+L_1 \,\sin \left(\theta_{O,1} \right)\newline 0 \end{array}\right) $$ 
 

```matlab
p_O_C3 = simplify([T_O_C3(1,4);T_O_C3(2,4);T_O_C3(3,4)])
```
p_O_C3 = 

  $$ \displaystyle \left(\begin{array}{c} x_{O,1} +L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)+L_1 \,\cos \left(\theta_{O,1} \right)+x_{3,\textrm{C3}} \,\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\newline y_{O,1} +x_{3,\textrm{C3}} \,\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)+L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)+L_1 \,\sin \left(\theta_{O,1} \right)\newline 0 \end{array}\right) $$ 
 

Nótese que para obetener estos vectores de posición, se extraen los valores de las posicones (1,4),(2,4) y (3,4) de cada matriz de transformación de cada centro de masa. Estas posiciones extraídas corresponden exactamente con el vector de posición interno de una matriz de transformación, y corresponden también a la componente X, a la componente en Y y a la componente en Z, respectivamente. 

# Cálculo de las velocidades de los centros de masa 

Las velocidades de los centros de masa se obtienen a partir de la derivada temporal de sus posiciones. Debido a que dichas posiciones están expresadas en función de las variables articulares, se aplica la regla de la cadena, lo que permite expresar la velocidad como el producto del Jacobiano lineal por el vector de velocidades articulares. Este procedimiento es fundamental en el modelado dinámico, ya que las velocidades de los centros de masa intervienen directamente en el cálculo de la energía cinética del sistema.

 $$ {}^0{\dot{\xi} }_{\textrm{ci}}=\frac{\partial }{\partial \theta }{}^0{\xi }_{\textrm{ci}}\dot{\theta} $$ 

En este caso de desean calcular ${}^0{\dot{\xi} }_{\textrm{c1}}$, ${}^0{\dot{\xi} }_{\textrm{c2}}$ y ${}^0{\dot{\xi} }_{\textrm{c3}}$.


Ahora, aunque cada centro de masa de cada eslabón existe en el espacio, su velocidad realemente depende de las articulaciones del robot, por esta razón se deriva respecto a cada articulación, luego se multiplica por la velocidad correspondiente, y finalmente se suma.

```matlab
syms theta_dot_O_1 theta_dot_1_2 theta_dot_2_3

v_O_C1 = diff(p_O_C1,theta_O_1)*theta_dot_O_1+diff(p_O_C1,theta_1_2)*theta_dot_1_2+diff(p_O_C1,theta_2_3)*theta_dot_2_3
```
v_O_C1 = 

  $$ \displaystyle \left(\begin{array}{c} -{\dot{\theta} }_{O,1} \,x_{1,\textrm{C1}} \,\sin \left(\theta_{O,1} \right)\newline {\dot{\theta} }_{O,1} \,x_{1,\textrm{C1}} \,\cos \left(\theta_{O,1} \right)\newline 0 \end{array}\right) $$ 
 

```matlab
v_O_C2 = diff(p_O_C2,theta_O_1)*theta_dot_O_1+diff(p_O_C2,theta_1_2)*theta_dot_1_2+diff(p_O_C2,theta_2_3)*theta_dot_2_3
```
v_O_C2 = 

  $$ \displaystyle \left(\begin{array}{c} -{\dot{\theta} }_{O,1} \,{\left(x_{2,\textrm{C2}} \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)+L_1 \,\sin \left(\theta_{O,1} \right)\right)}-{\dot{\theta} }_{1,2} \,x_{2,\textrm{C2}} \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\newline {\dot{\theta} }_{O,1} \,{\left(x_{2,\textrm{C2}} \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)+L_1 \,\cos \left(\theta_{O,1} \right)\right)}+{\dot{\theta} }_{1,2} \,x_{2,\textrm{C2}} \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\newline 0 \end{array}\right) $$ 
 

```matlab
v_O_C3 = diff(p_O_C3,theta_O_1)*theta_dot_O_1+diff(p_O_C3,theta_1_2)*theta_dot_1_2+diff(p_O_C3,theta_2_3)*theta_dot_2_3
```
v_O_C3 = 

  $$ \displaystyle \begin{array}{l} \left(\begin{array}{c} -{\dot{\theta} }_{O,1} \,{\left(\sigma_1 +\sigma_3 +L_1 \,\sin \left(\theta_{O,1} \right)\right)}-{\dot{\theta} }_{1,2} \,{\left(\sigma_1 +\sigma_3 \right)}-{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\newline {\dot{\theta} }_{O,1} \,{\left(\sigma_4 +L_1 \,\cos \left(\theta_{O,1} \right)+\sigma_2 \right)}+{\dot{\theta} }_{1,2} \,{\left(\sigma_4 +\sigma_2 \right)}+{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\newline 0 \end{array}\right)\\\mathrm{}\\\textrm{where}\\\mathrm{}\\\;\;\sigma_1 =x_{3,\textrm{C3}} \,\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_2 =x_{3,\textrm{C3}} \,\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_3 =L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_4 =L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\end{array} $$ 
 
# Cálculo de las velocidades angulares

Las velocidades angulares de los eslabones se determinan considerando la contribución acumulativa de las velocidades articulares anteriores a ese eslabón. Es decir, cada eslabón hereda la rotación de los eslabones anteriores, lo que da lugar a una suma de términos asociados a las velocidades articulares. Estas velocidades son fundamentales para el cálculo de la energía cinética rotacional.


Cada una de las velocidades angulares se calculan de la siguiente forma:

 $$ {}^{i+1}{\omega }_{i+1}={}^{i+1}R_i{}^i{\omega }_i+{}^{i+1}{\hat{z} }_{i+1}{\dot{\theta} }_{i+1} $$ 

La expresión anterior  considera tanto la rotación heredada, transformada al nuevo sistema de referencia mediante una matriz de rotación, como la contribución propia de la articulación actual. Es decir, el rpiemer sumando corresponde al efecto que veiene del eslabón anterior, y el segundo sumando corresponde al efecto provocando por ele eslabón en cuestión.


 **Velocidad angular** ${}^1{\omega }_1$ **:**

```matlab
syms omega_1_1 omega_2_2 omega_3_3
%Propagación para el primer cuerpo
omega_1_1
```
omega_1_1 = 
 $\displaystyle \omega_{1,1} $
 

La velocidad angular del origen se define en cero, porque no existe rotación del sistema inercial.

```matlab
omega_O_O = [0;0;0]
```

```matlabTextOutput
omega_O_O = 3x1
     0
     0
     0

```


Además se define un vector perpendicular al plano XY del robot, para poder después definir las rotaciones sobre el eje Z.

```matlab
n_1_1 = [0;0;1]
```

```matlabTextOutput
n_1_1 = 3x1
     0
     0
     1

```


Se definen la matriz de rotación del primer cuerpo con respecto al sistema inercial, y su transpuesta.

```matlab
R_O_1 = [T_O_1(1,1),T_O_1(1,2),T_O_1(1,3);T_O_1(2,1),T_O_1(2,2),T_O_1(2,3);T_O_1(3,1),T_O_1(3,2),T_O_1(3,3)]
```
R_O_1 = 

  $$ \displaystyle \left(\begin{array}{ccc} \cos \left(\theta_{O,1} \right) & -\sin \left(\theta_{O,1} \right) & 0\newline \sin \left(\theta_{O,1} \right) & \cos \left(\theta_{O,1} \right) & 0\newline 0 & 0 & 1 \end{array}\right) $$ 
 

```matlab
R_1_O = transpose(R_O_1)
```
R_1_O = 

  $$ \displaystyle \left(\begin{array}{ccc} \cos \left(\theta_{O,1} \right) & \sin \left(\theta_{O,1} \right) & 0\newline -\sin \left(\theta_{O,1} \right) & \cos \left(\theta_{O,1} \right) & 0\newline 0 & 0 & 1 \end{array}\right) $$ 
 

Con lo anterior ya definido, se puede obtener la velocidad angular 1:

```matlab
%Ecuación de propagación
omega_1_1 = R_1_O*omega_O_O+n_1_1*theta_dot_O_1
```
omega_1_1 = 

  $$ \displaystyle \left(\begin{array}{c} 0\newline 0\newline {\dot{\theta} }_{O,1}  \end{array}\right) $$ 
 

 **Velocidad angular** ${}^2{\omega }_2$ **:**


De la misma forma que se obtiene la propagación de velocidad del primer eslabón, se obtiene ahora el segundo:

```matlab
%Propagación para el segundo cuerpo
omega_2_2
```
omega_2_2 = 
 $\displaystyle \omega_{2,2} $
 

```matlab
n_2_2 = [0;0;1]
```

```matlabTextOutput
n_2_2 = 3x1
     0
     0
     1

```

```matlab
R_1_2 = [T_1_2(1,1),T_1_2(1,2),T_1_2(1,3);T_1_2(2,1),T_1_2(2,2),T_1_2(2,3);T_1_2(3,1),T_1_2(3,2),T_1_2(3,3)]
```
R_1_2 = 

  $$ \displaystyle \left(\begin{array}{ccc} \cos \left(\theta_{1,2} \right) & -\sin \left(\theta_{1,2} \right) & 0\newline \sin \left(\theta_{1,2} \right) & \cos \left(\theta_{1,2} \right) & 0\newline 0 & 0 & 1 \end{array}\right) $$ 
 

```matlab
R_2_1 = transpose(R_1_2)
```
R_2_1 = 

  $$ \displaystyle \left(\begin{array}{ccc} \cos \left(\theta_{1,2} \right) & \sin \left(\theta_{1,2} \right) & 0\newline -\sin \left(\theta_{1,2} \right) & \cos \left(\theta_{1,2} \right) & 0\newline 0 & 0 & 1 \end{array}\right) $$ 
 

```matlab

%Ecuación de propagación
omega_2_2 = R_2_1*omega_1_1+n_2_2*theta_dot_1_2
```
omega_2_2 = 

  $$ \displaystyle \left(\begin{array}{c} 0\newline 0\newline {\dot{\theta} }_{1,2} +{\dot{\theta} }_{O,1}  \end{array}\right) $$ 
 

```matlab

%Propagación para el tercer cuerpo
omega_3_3
```
omega_3_3 = 
 $\displaystyle \omega_{3,3} $
 

```matlab
n_3_3 = [0;0;1]
```

```matlabTextOutput
n_3_3 = 3x1
     0
     0
     1

```

```matlab
R_2_3 = [T_2_3(1,1),T_2_3(1,2),T_2_3(1,3);T_2_3(2,1),T_2_3(2,2),T_2_3(2,3);T_2_3(3,1),T_2_3(3,2),T_2_3(3,3)]
```
R_2_3 = 

  $$ \displaystyle \left(\begin{array}{ccc} \cos \left(\theta_{2,3} \right) & -\sin \left(\theta_{2,3} \right) & 0\newline \sin \left(\theta_{2,3} \right) & \cos \left(\theta_{2,3} \right) & 0\newline 0 & 0 & 1 \end{array}\right) $$ 
 

```matlab
R_3_2 = transpose(R_2_3)
```
R_3_2 = 

  $$ \displaystyle \left(\begin{array}{ccc} \cos \left(\theta_{2,3} \right) & \sin \left(\theta_{2,3} \right) & 0\newline -\sin \left(\theta_{2,3} \right) & \cos \left(\theta_{2,3} \right) & 0\newline 0 & 0 & 1 \end{array}\right) $$ 
 

 **Velocidad angular** ${}^3{\omega }_3$ **:**


Por último, de misma forma que se obtiene la propagación de velocidad del segundo eslabón, se obtiene el último eslabón:

```matlab
%Ecuación de propagación
omega_3_3 = R_3_2*omega_2_2+n_3_3*theta_dot_2_3
```
omega_3_3 = 

  $$ \displaystyle \left(\begin{array}{c} 0\newline 0\newline {\dot{\theta} }_{1,2} +{\dot{\theta} }_{2,3} +{\dot{\theta} }_{O,1}  \end{array}\right) $$ 
 

Nótese que la velocidad angular del tercer eslabón depende de las articulaciones anteriores a este, y que para todos los eslabones su velocidad angular existe sólo en el eje Z, pues el robot se encuentra en el plano XY.

# Defición de los elementos de inercia 

Los elementos de inercia del sistema describen cómo se distribuye la masa de cada eslabón respecto a sus ejes de rotación. Cada eslabón posee un tensor de inercia que depende de su geometría y de la ubicación de su centro de masa. Dado que este análisis del robot se está haciendo en el plano, el tensor de inercia puede simplificarse considerando únicamente el momento de inercia respecto al eje perpendicular al plano de movimiento, el eje Z.


En esta sección se aprovecha para definir al vector gravedad en eleje Y negativo. Esto es así porque el sistema es planar.

```matlab
syms g I_xx1 I_yy1 I_zz1 I_xx2 I_yy2 I_zz2 I_xx3 I_yy3 I_zz3
%vector de gravedad

g_v = [0;-g;0]
```
g_v = 

  $$ \displaystyle \left(\begin{array}{c} 0\newline -g\newline 0 \end{array}\right) $$ 
 

A continuación se observa que, los tensores de inercia obtenidos presentan una forma de matriz diagonal, lo cual indica que los productos de inercia son nulos y que los ejes principales de los eslabones coinciden con los ejes del sistema de referencia. Esta simplificación es válida debido a la naturaleza simétrica de los eslabones y al carácter planar del robot.

```matlab
I_C1 = [I_xx1,0,0;0,I_yy1,0;0,0,I_zz1]
```
I_C1 = 

  $$ \displaystyle \left(\begin{array}{ccc} I_{\textrm{xx1}}  & 0 & 0\newline 0 & I_{\textrm{yy1}}  & 0\newline 0 & 0 & I_{\textrm{zz1}}  \end{array}\right) $$ 
 

```matlab
I_C2 = [I_xx2,0,0;0,I_yy2,0;0,0,I_zz2]
```
I_C2 = 

  $$ \displaystyle \left(\begin{array}{ccc} I_{\textrm{xx2}}  & 0 & 0\newline 0 & I_{\textrm{yy2}}  & 0\newline 0 & 0 & I_{\textrm{zz2}}  \end{array}\right) $$ 
 

```matlab
I_C3 = [I_xx3,0,0;0,I_yy3,0;0,0,I_zz3]
```
I_C3 = 

  $$ \displaystyle \left(\begin{array}{ccc} I_{\textrm{xx3}}  & 0 & 0\newline 0 & I_{\textrm{yy3}}  & 0\newline 0 & 0 & I_{\textrm{zz3}}  \end{array}\right) $$ 
 

# Cáculo de energías cinética y potencial

Primero se calculan cada uno de los elementos necesarios para sustituir en la fórmula del lagrangiano, para esto se sustituyen los valores previamente calculados en las fórmulas de energía cinética y potencial.

```matlab

syms m_1 m_2 m_3
%energía cinética de cada uno de los cuerpos

k_1 = simplify((m_1/2)*transpose(v_O_C1)*v_O_C1+(1/2)*transpose(omega_1_1)*I_C1*omega_1_1)
```
k_1 = 
 $\displaystyle \frac{{{\dot{\theta} }_{O,1} }^2 \,{\left(m_1 \,{x_{1,\textrm{C1}} }^2 +I_{\textrm{zz1}} \right)}}{2}$
 

```matlab

k_2 = simplify((m_2/2)*transpose(v_O_C2)*v_O_C2+(1/2)*transpose(omega_2_2)*I_C2*omega_2_2)
```
k_2 = 
 $\displaystyle \frac{m_2 \,{L_1 }^2 \,{{\dot{\theta} }_{O,1} }^2 }{2}+m_2 \,\cos \left(\theta_{1,2} \right)\,L_1 \,{\dot{\theta} }_{1,2} \,{\dot{\theta} }_{O,1} \,x_{2,\textrm{C2}} +m_2 \,\cos \left(\theta_{1,2} \right)\,L_1 \,{{\dot{\theta} }_{O,1} }^2 \,x_{2,\textrm{C2}} +\frac{m_2 \,{{\dot{\theta} }_{1,2} }^2 \,{x_{2,\textrm{C2}} }^2 }{2}+\frac{I_{\textrm{zz2}} \,{{\dot{\theta} }_{1,2} }^2 }{2}+m_2 \,{\dot{\theta} }_{1,2} \,{\dot{\theta} }_{O,1} \,{x_{2,\textrm{C2}} }^2 +I_{\textrm{zz2}} \,{\dot{\theta} }_{1,2} \,{\dot{\theta} }_{O,1} +\frac{m_2 \,{{\dot{\theta} }_{O,1} }^2 \,{x_{2,\textrm{C2}} }^2 }{2}+\frac{I_{\textrm{zz2}} \,{{\dot{\theta} }_{O,1} }^2 }{2}$
 

```matlab

k_3 = simplify((m_3/2)*transpose(v_O_C3)*v_O_C3+(1/2)*transpose(omega_3_3)*I_C3*omega_3_3)
```
k_3 = 

  $$ \displaystyle \begin{array}{l} \frac{m_3 \,{{\left({\dot{\theta} }_{O,1} \,{\left(\sigma_2 +L_1 \,\cos \left(\theta_{O,1} \right)+x_{3,\textrm{C3}} \,\sigma_4 \right)}+{\dot{\theta} }_{1,2} \,{\left(\sigma_2 +x_{3,\textrm{C3}} \,\sigma_4 \right)}+{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_4 \right)}}^2 }{2}+\frac{m_3 \,{{\left({\dot{\theta} }_{O,1} \,{\left(x_{3,\textrm{C3}} \,\sigma_3 +\sigma_1 +L_1 \,\sin \left(\theta_{O,1} \right)\right)}+{\dot{\theta} }_{1,2} \,{\left(x_{3,\textrm{C3}} \,\sigma_3 +\sigma_1 \right)}+{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_3 \right)}}^2 }{2}+I_{\textrm{zz3}} \,{\left({\dot{\theta} }_{1,2} +{\dot{\theta} }_{2,3} +{\dot{\theta} }_{O,1} \right)}\,{\left(\frac{{\dot{\theta} }_{1,2} }{2}+\frac{{\dot{\theta} }_{2,3} }{2}+\frac{{\dot{\theta} }_{O,1} }{2}\right)}\newline \mathrm{}\newline \textrm{where}\newline \mathrm{}\newline \;\;\sigma_1 =L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_2 =L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_3 =\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_4 =\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right) \end{array} $$ 
 

```matlab
% Cáclulo de la energía potencial de cada cuerpo

u_1 = -m_1*transpose(p_O_C1)*g_v
```
u_1 = 
 $\displaystyle g\,m_1 \,{\left(y_{O,1} +x_{1,\textrm{C1}} \,\sin \left(\theta_{O,1} \right)\right)}$
 

```matlab
u_2 = -m_2*transpose(p_O_C2)*g_v
```
u_2 = 
 $\displaystyle g\,m_2 \,{\left(y_{O,1} +x_{2,\textrm{C2}} \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)+L_1 \,\sin \left(\theta_{O,1} \right)\right)}$
 

```matlab
u_3 = -m_3*transpose(p_O_C3)*g_v
```
u_3 = 
 $\displaystyle g\,m_3 \,{\left(y_{O,1} +x_{3,\textrm{C3}} \,\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)+L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)+L_1 \,\sin \left(\theta_{O,1} \right)\right)}$
 

# Cálculo del Lagrangeano

Se hace la resta de las energías cinéticas menos las energías potenciales, para obtener así al lagrangiano.

```matlab

La = (k_1+k_2+k_3)-(u_1+u_2+u_3)
```
La = 

  $$ \displaystyle \begin{array}{l} \frac{I_{\textrm{zz2}} \,{{\dot{\theta} }_{1,2} }^2 }{2}+\frac{I_{\textrm{zz2}} \,{{\dot{\theta} }_{O,1} }^2 }{2}+\frac{{{\dot{\theta} }_{O,1} }^2 \,{\left(m_1 \,{x_{1,\textrm{C1}} }^2 +I_{\textrm{zz1}} \right)}}{2}+\frac{m_3 \,{{\left({\dot{\theta} }_{O,1} \,{\left(\sigma_2 +L_1 \,\cos \left(\theta_{O,1} \right)+x_{3,\textrm{C3}} \,\sigma_4 \right)}+{\dot{\theta} }_{1,2} \,{\left(\sigma_2 +x_{3,\textrm{C3}} \,\sigma_4 \right)}+{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_4 \right)}}^2 }{2}+\frac{m_3 \,{{\left({\dot{\theta} }_{O,1} \,{\left(x_{3,\textrm{C3}} \,\sigma_3 +\sigma_1 +L_1 \,\sin \left(\theta_{O,1} \right)\right)}+{\dot{\theta} }_{1,2} \,{\left(x_{3,\textrm{C3}} \,\sigma_3 +\sigma_1 \right)}+{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_3 \right)}}^2 }{2}-g\,m_3 \,{\left(y_{O,1} +x_{3,\textrm{C3}} \,\sigma_3 +\sigma_1 +L_1 \,\sin \left(\theta_{O,1} \right)\right)}-g\,m_2 \,{\left(y_{O,1} +x_{2,\textrm{C2}} \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)+L_1 \,\sin \left(\theta_{O,1} \right)\right)}+I_{\textrm{zz3}} \,{\left({\dot{\theta} }_{1,2} +{\dot{\theta} }_{2,3} +{\dot{\theta} }_{O,1} \right)}\,{\left(\frac{{\dot{\theta} }_{1,2} }{2}+\frac{{\dot{\theta} }_{2,3} }{2}+\frac{{\dot{\theta} }_{O,1} }{2}\right)}+\frac{{L_1 }^2 \,m_2 \,{{\dot{\theta} }_{O,1} }^2 }{2}+\frac{m_2 \,{{\dot{\theta} }_{1,2} }^2 \,{x_{2,\textrm{C2}} }^2 }{2}+\frac{m_2 \,{{\dot{\theta} }_{O,1} }^2 \,{x_{2,\textrm{C2}} }^2 }{2}+I_{\textrm{zz2}} \,{\dot{\theta} }_{1,2} \,{\dot{\theta} }_{O,1} -g\,m_1 \,{\left(y_{O,1} +x_{1,\textrm{C1}} \,\sin \left(\theta_{O,1} \right)\right)}+m_2 \,{\dot{\theta} }_{1,2} \,{\dot{\theta} }_{O,1} \,{x_{2,\textrm{C2}} }^2 +L_1 \,m_2 \,{{\dot{\theta} }_{O,1} }^2 \,x_{2,\textrm{C2}} \,\cos \left(\theta_{1,2} \right)+L_1 \,m_2 \,{\dot{\theta} }_{1,2} \,{\dot{\theta} }_{O,1} \,x_{2,\textrm{C2}} \,\cos \left(\theta_{1,2} \right)\newline \mathrm{}\newline \textrm{where}\newline \mathrm{}\newline \;\;\sigma_1 =L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_2 =L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_3 =\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_4 =\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right) \end{array} $$ 
 
# Cálculo de los pares

Una vez obtenido el lagrangiano, se pueden calcular los pares del vector de esfuerzos. Cabe mencionar que en esta secció se divide el cálculo de cada par porque matlab no permite realizar la operación completa en un solo cálculo.

 $$ \tau_{i\;} =\frac{d}{\textrm{dt}}\left(\frac{\partial }{\partial \dot{q_i } }\;\Gamma \right)-\frac{\partial }{\partial q_{i\;} }\;\Gamma $$ 

Debido a la complejidad del modelo dinámico, el cálculo directo de la derivada temporal del Lagrangiano no es realizado automáticamente por MATLAB como una operación única. Esto se debe a que el Lagrangiano depende simultáneamente de múltiples variables articulares y sus derivadas, lo que implica la aplicación de la regla de la cadena en varias dimensiones. Por esta razón, el procedimiento se descompone en una serie de derivadas parciales respecto a cada coordenada generalizada y sus velocidades, multiplicadas por sus correspondientes derivadas temporales. Esta descomposición permite estructurar el cálculo de manera sistemática y manejable dentro del entorno simbólico, asegurando que se consideren correctamente todas las contribuciones de posición, velocidad y aceleración en la obtención de los pares articulares.


 **Cálculo del Par 1,** $\tau_1$ **:**

```matlab
syms theta_ddot_O_1 theta_ddot_1_2 theta_ddot_2_3

D_theta1 = diff(La,theta_dot_O_1)
```
D_theta1 = 

  $$ \displaystyle \begin{array}{l} I_{\textrm{zz2}} \,{\dot{\theta} }_{1,2} +I_{\textrm{zz2}} \,{\dot{\theta} }_{O,1} +{\dot{\theta} }_{O,1} \,{\left(m_1 \,{x_{1,\textrm{C1}} }^2 +I_{\textrm{zz1}} \right)}+\frac{I_{\textrm{zz3}} \,{\left({\dot{\theta} }_{1,2} +{\dot{\theta} }_{2,3} +{\dot{\theta} }_{O,1} \right)}}{2}+I_{\textrm{zz3}} \,{\left(\frac{{\dot{\theta} }_{1,2} }{2}+\frac{{\dot{\theta} }_{2,3} }{2}+\frac{{\dot{\theta} }_{O,1} }{2}\right)}+m_2 \,{\dot{\theta} }_{1,2} \,{x_{2,\textrm{C2}} }^2 +m_2 \,{\dot{\theta} }_{O,1} \,{x_{2,\textrm{C2}} }^2 +m_3 \,\sigma_1 \,{\left({\dot{\theta} }_{O,1} \,\sigma_1 +{\dot{\theta} }_{1,2} \,{\left(\sigma_3 +x_{3,\textrm{C3}} \,\sigma_4 \right)}+{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_4 \right)}+m_3 \,\sigma_2 \,{\left({\dot{\theta} }_{O,1} \,\sigma_2 +{\dot{\theta} }_{1,2} \,{\left(x_{3,\textrm{C3}} \,\sigma_6 +\sigma_5 \right)}+{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_6 \right)}+{L_1 }^2 \,m_2 \,{\dot{\theta} }_{O,1} +L_1 \,m_2 \,{\dot{\theta} }_{1,2} \,x_{2,\textrm{C2}} \,\cos \left(\theta_{1,2} \right)+2\,L_1 \,m_2 \,{\dot{\theta} }_{O,1} \,x_{2,\textrm{C2}} \,\cos \left(\theta_{1,2} \right)\newline \mathrm{}\newline \textrm{where}\newline \mathrm{}\newline \;\;\sigma_1 =\sigma_3 +L_1 \,\cos \left(\theta_{O,1} \right)+x_{3,\textrm{C3}} \,\sigma_4 \newline \mathrm{}\newline \;\;\sigma_2 =x_{3,\textrm{C3}} \,\sigma_6 +\sigma_5 +L_1 \,\sin \left(\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_3 =L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_4 =\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_5 =L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_6 =\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right) \end{array} $$ 
 

```matlab

% Cálculo de relación

tao_1 = diff(D_theta1,theta_O_1)*theta_dot_O_1 + diff(D_theta1,theta_1_2)*theta_dot_1_2 + diff(D_theta1,theta_2_3)*theta_dot_2_3 + diff(D_theta1,theta_dot_O_1)*theta_ddot_O_1+ diff(D_theta1,theta_dot_1_2)*theta_ddot_1_2+ diff(D_theta1,theta_dot_2_3)*theta_ddot_2_3-diff(La,theta_O_1)
```
tao_1 = 

  $$ \displaystyle \begin{array}{l} {\ddot{\theta} }_{O,1} \,{\left(I_{\textrm{zz1}} +I_{\textrm{zz2}} +I_{\textrm{zz3}} +m_3 \,{\sigma_5 }^2 +{L_1 }^2 \,m_2 +m_3 \,{\sigma_3 }^2 +m_1 \,{x_{1,\textrm{C1}} }^2 +m_2 \,{x_{2,\textrm{C2}} }^2 +2\,L_1 \,m_2 \,x_{2,\textrm{C2}} \,\cos \left(\theta_{1,2} \right)\right)}-{\dot{\theta} }_{1,2} \,{\left(m_3 \,{\left(\sigma_4 +{\dot{\theta} }_{O,1} \,{\left(x_{3,\textrm{C3}} \,\sigma_7 +\sigma_8 \right)}+{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_7 \right)}\,\sigma_5 -m_3 \,\sigma_3 \,{\left(\sigma_6 +{\dot{\theta} }_{O,1} \,{\left(\sigma_{10} +x_{3,\textrm{C3}} \,\sigma_9 \right)}+{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_9 \right)}-m_3 \,{\left(\sigma_{10} +x_{3,\textrm{C3}} \,\sigma_9 \right)}\,\sigma_1 +m_3 \,{\left(x_{3,\textrm{C3}} \,\sigma_7 +\sigma_8 \right)}\,\sigma_2 +L_1 \,m_2 \,{\dot{\theta} }_{1,2} \,x_{2,\textrm{C2}} \,\sin \left(\theta_{1,2} \right)+2\,L_1 \,m_2 \,{\dot{\theta} }_{O,1} \,x_{2,\textrm{C2}} \,\sin \left(\theta_{1,2} \right)\right)}+{\ddot{\theta} }_{1,2} \,{\left(m_2 \,{x_{2,\textrm{C2}} }^2 +L_1 \,m_2 \,\cos \left(\theta_{1,2} \right)\,x_{2,\textrm{C2}} +I_{\textrm{zz2}} +I_{\textrm{zz3}} +m_3 \,{\left(\sigma_{10} +x_{3,\textrm{C3}} \,\sigma_9 \right)}\,\sigma_5 +m_3 \,{\left(x_{3,\textrm{C3}} \,\sigma_7 +\sigma_8 \right)}\,\sigma_3 \right)}-{\dot{\theta} }_{2,3} \,{\left(m_3 \,\sigma_5 \,{\left({\dot{\theta} }_{1,2} \,x_{3,\textrm{C3}} \,\sigma_7 +{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_7 +{\dot{\theta} }_{O,1} \,x_{3,\textrm{C3}} \,\sigma_7 \right)}-m_3 \,\sigma_3 \,{\left({\dot{\theta} }_{1,2} \,x_{3,\textrm{C3}} \,\sigma_9 +{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_9 +{\dot{\theta} }_{O,1} \,x_{3,\textrm{C3}} \,\sigma_9 \right)}-m_3 \,x_{3,\textrm{C3}} \,\sigma_9 \,\sigma_1 +m_3 \,x_{3,\textrm{C3}} \,\sigma_7 \,\sigma_2 \right)}+{\ddot{\theta} }_{2,3} \,{\left(I_{\textrm{zz3}} +m_3 \,x_{3,\textrm{C3}} \,\sigma_9 \,\sigma_5 +m_3 \,x_{3,\textrm{C3}} \,\sigma_7 \,\sigma_3 \right)}+g\,m_3 \,\sigma_5 +g\,m_2 \,{\left(x_{2,\textrm{C2}} \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)+L_1 \,\cos \left(\theta_{O,1} \right)\right)}+g\,m_1 \,x_{1,\textrm{C1}} \,\cos \left(\theta_{O,1} \right)\newline \mathrm{}\newline \textrm{where}\newline \mathrm{}\newline \;\;\sigma_1 ={\dot{\theta} }_{O,1} \,\sigma_3 +\sigma_4 +{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_7 \newline \mathrm{}\newline \;\;\sigma_2 ={\dot{\theta} }_{O,1} \,\sigma_5 +\sigma_6 +{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_9 \newline \mathrm{}\newline \;\;\sigma_3 =x_{3,\textrm{C3}} \,\sigma_7 +\sigma_8 +L_1 \,\sin \left(\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_4 ={\dot{\theta} }_{1,2} \,{\left(x_{3,\textrm{C3}} \,\sigma_7 +\sigma_8 \right)}\newline \mathrm{}\newline \;\;\sigma_5 =\sigma_{10} +L_1 \,\cos \left(\theta_{O,1} \right)+x_{3,\textrm{C3}} \,\sigma_9 \newline \mathrm{}\newline \;\;\sigma_6 ={\dot{\theta} }_{1,2} \,{\left(\sigma_{10} +x_{3,\textrm{C3}} \,\sigma_9 \right)}\newline \mathrm{}\newline \;\;\sigma_7 =\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_8 =L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_9 =\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_{10} =L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right) \end{array} $$ 
 

 **Cálculo del Par 2,** $\tau_2$ **:**

```matlab
D_theta2 = diff(La,theta_dot_1_2)
```
D_theta2 = 

  $$ \displaystyle \begin{array}{l} I_{\textrm{zz2}} \,{\dot{\theta} }_{1,2} +I_{\textrm{zz2}} \,{\dot{\theta} }_{O,1} +\frac{I_{\textrm{zz3}} \,{\left({\dot{\theta} }_{1,2} +{\dot{\theta} }_{2,3} +{\dot{\theta} }_{O,1} \right)}}{2}+I_{\textrm{zz3}} \,{\left(\frac{{\dot{\theta} }_{1,2} }{2}+\frac{{\dot{\theta} }_{2,3} }{2}+\frac{{\dot{\theta} }_{O,1} }{2}\right)}+m_2 \,{\dot{\theta} }_{1,2} \,{x_{2,\textrm{C2}} }^2 +m_2 \,{\dot{\theta} }_{O,1} \,{x_{2,\textrm{C2}} }^2 +m_3 \,{\left(x_{3,\textrm{C3}} \,\sigma_3 +\sigma_1 \right)}\,{\left({\dot{\theta} }_{O,1} \,{\left(x_{3,\textrm{C3}} \,\sigma_3 +\sigma_1 +L_1 \,\sin \left(\theta_{O,1} \right)\right)}+{\dot{\theta} }_{1,2} \,{\left(x_{3,\textrm{C3}} \,\sigma_3 +\sigma_1 \right)}+{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_3 \right)}+m_3 \,{\left(\sigma_2 +x_{3,\textrm{C3}} \,\sigma_4 \right)}\,{\left({\dot{\theta} }_{O,1} \,{\left(\sigma_2 +L_1 \,\cos \left(\theta_{O,1} \right)+x_{3,\textrm{C3}} \,\sigma_4 \right)}+{\dot{\theta} }_{1,2} \,{\left(\sigma_2 +x_{3,\textrm{C3}} \,\sigma_4 \right)}+{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_4 \right)}+L_1 \,m_2 \,{\dot{\theta} }_{O,1} \,x_{2,\textrm{C2}} \,\cos \left(\theta_{1,2} \right)\newline \mathrm{}\newline \textrm{where}\newline \mathrm{}\newline \;\;\sigma_1 =L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_2 =L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_3 =\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_4 =\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right) \end{array} $$ 
 

```matlab
tao_2 = diff(D_theta2,theta_O_1)*theta_dot_O_1 + diff(D_theta2,theta_1_2)*theta_dot_1_2 + diff(D_theta2,theta_2_3)*theta_dot_2_3 + diff(D_theta2,theta_dot_O_1)*theta_ddot_O_1+ diff(D_theta2,theta_dot_1_2)*theta_ddot_1_2+ diff(D_theta2,theta_dot_2_3)*theta_ddot_2_3-diff(La,theta_1_2)
```
tao_2 = 

  $$ \displaystyle \begin{array}{l} {\ddot{\theta} }_{1,2} \,{\left(I_{\textrm{zz2}} +I_{\textrm{zz3}} +m_2 \,{x_{2,\textrm{C2}} }^2 +m_3 \,{{\left(\sigma_{12} +x_{3,\textrm{C3}} \,\sigma_{11} \right)}}^2 +m_3 \,{{\left(x_{3,\textrm{C3}} \,\sigma_9 +\sigma_{10} \right)}}^2 \right)}-{\dot{\theta} }_{1,2} \,{\left(m_3 \,{\left(\sigma_{12} +x_{3,\textrm{C3}} \,\sigma_{11} \right)}\,\sigma_3 -m_3 \,{\left(x_{3,\textrm{C3}} \,\sigma_9 +\sigma_{10} \right)}\,\sigma_4 -m_3 \,{\left(\sigma_{12} +x_{3,\textrm{C3}} \,\sigma_{11} \right)}\,\sigma_1 +m_3 \,{\left(x_{3,\textrm{C3}} \,\sigma_9 +\sigma_{10} \right)}\,\sigma_2 +L_1 \,m_2 \,{\dot{\theta} }_{O,1} \,x_{2,\textrm{C2}} \,\sin \left(\theta_{1,2} \right)\right)}+{\ddot{\theta} }_{O,1} \,{\left(m_2 \,{x_{2,\textrm{C2}} }^2 +L_1 \,m_2 \,\cos \left(\theta_{1,2} \right)\,x_{2,\textrm{C2}} +I_{\textrm{zz2}} +I_{\textrm{zz3}} +m_3 \,{\left(\sigma_{12} +x_{3,\textrm{C3}} \,\sigma_{11} \right)}\,\sigma_6 +m_3 \,{\left(x_{3,\textrm{C3}} \,\sigma_9 +\sigma_{10} \right)}\,\sigma_5 \right)}+{\dot{\theta} }_{2,3} \,{\left(m_3 \,{\left(x_{3,\textrm{C3}} \,\sigma_9 +\sigma_{10} \right)}\,{\left({\dot{\theta} }_{1,2} \,x_{3,\textrm{C3}} \,\sigma_{11} +{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_{11} +{\dot{\theta} }_{O,1} \,x_{3,\textrm{C3}} \,\sigma_{11} \right)}-m_3 \,{\left(\sigma_{12} +x_{3,\textrm{C3}} \,\sigma_{11} \right)}\,{\left({\dot{\theta} }_{1,2} \,x_{3,\textrm{C3}} \,\sigma_9 +{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_9 +{\dot{\theta} }_{O,1} \,x_{3,\textrm{C3}} \,\sigma_9 \right)}+m_3 \,x_{3,\textrm{C3}} \,\sigma_{11} \,\sigma_1 -m_3 \,x_{3,\textrm{C3}} \,\sigma_9 \,\sigma_2 \right)}+{\ddot{\theta} }_{2,3} \,{\left(I_{\textrm{zz3}} +m_3 \,x_{3,\textrm{C3}} \,\sigma_{11} \,{\left(\sigma_{12} +x_{3,\textrm{C3}} \,\sigma_{11} \right)}+m_3 \,x_{3,\textrm{C3}} \,\sigma_9 \,{\left(x_{3,\textrm{C3}} \,\sigma_9 +\sigma_{10} \right)}\right)}+m_3 \,\sigma_3 \,\sigma_2 -m_3 \,\sigma_1 \,\sigma_4 +g\,m_3 \,{\left(\sigma_{12} +x_{3,\textrm{C3}} \,\sigma_{11} \right)}+g\,m_2 \,x_{2,\textrm{C2}} \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)+L_1 \,m_2 \,{{\dot{\theta} }_{O,1} }^2 \,x_{2,\textrm{C2}} \,\sin \left(\theta_{1,2} \right)+L_1 \,m_2 \,{\dot{\theta} }_{1,2} \,{\dot{\theta} }_{O,1} \,x_{2,\textrm{C2}} \,\sin \left(\theta_{1,2} \right)\newline \mathrm{}\newline \textrm{where}\newline \mathrm{}\newline \;\;\sigma_1 ={\dot{\theta} }_{O,1} \,\sigma_5 +\sigma_7 +{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_9 \newline \mathrm{}\newline \;\;\sigma_2 ={\dot{\theta} }_{O,1} \,\sigma_6 +\sigma_8 +{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_{11} \newline \mathrm{}\newline \;\;\sigma_3 =\sigma_7 +{\dot{\theta} }_{O,1} \,{\left(x_{3,\textrm{C3}} \,\sigma_9 +\sigma_{10} \right)}+{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_9 \newline \mathrm{}\newline \;\;\sigma_4 =\sigma_8 +{\dot{\theta} }_{O,1} \,{\left(\sigma_{12} +x_{3,\textrm{C3}} \,\sigma_{11} \right)}+{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_{11} \newline \mathrm{}\newline \;\;\sigma_5 =x_{3,\textrm{C3}} \,\sigma_9 +\sigma_{10} +L_1 \,\sin \left(\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_6 =\sigma_{12} +L_1 \,\cos \left(\theta_{O,1} \right)+x_{3,\textrm{C3}} \,\sigma_{11} \newline \mathrm{}\newline \;\;\sigma_7 ={\dot{\theta} }_{1,2} \,{\left(x_{3,\textrm{C3}} \,\sigma_9 +\sigma_{10} \right)}\newline \mathrm{}\newline \;\;\sigma_8 ={\dot{\theta} }_{1,2} \,{\left(\sigma_{12} +x_{3,\textrm{C3}} \,\sigma_{11} \right)}\newline \mathrm{}\newline \;\;\sigma_9 =\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_{10} =L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_{11} =\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_{12} =L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right) \end{array} $$ 
 

 **Cálculo del Par 3,** $\tau_3$ **:**

```matlab
D_theta3 = diff(La,theta_dot_2_3)
```
D_theta3 = 

  $$ \displaystyle \begin{array}{l} \frac{I_{\textrm{zz3}} \,{\left({\dot{\theta} }_{1,2} +{\dot{\theta} }_{2,3} +{\dot{\theta} }_{O,1} \right)}}{2}+I_{\textrm{zz3}} \,{\left(\frac{{\dot{\theta} }_{1,2} }{2}+\frac{{\dot{\theta} }_{2,3} }{2}+\frac{{\dot{\theta} }_{O,1} }{2}\right)}+m_3 \,x_{3,\textrm{C3}} \,\sigma_1 \,{\left({\dot{\theta} }_{O,1} \,{\left(x_{3,\textrm{C3}} \,\sigma_1 +\sigma_3 +L_1 \,\sin \left(\theta_{O,1} \right)\right)}+{\dot{\theta} }_{1,2} \,{\left(x_{3,\textrm{C3}} \,\sigma_1 +\sigma_3 \right)}+{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_1 \right)}+m_3 \,x_{3,\textrm{C3}} \,\sigma_2 \,{\left({\dot{\theta} }_{O,1} \,{\left(\sigma_4 +L_1 \,\cos \left(\theta_{O,1} \right)+x_{3,\textrm{C3}} \,\sigma_2 \right)}+{\dot{\theta} }_{1,2} \,{\left(\sigma_4 +x_{3,\textrm{C3}} \,\sigma_2 \right)}+{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_2 \right)}\newline \mathrm{}\newline \textrm{where}\newline \mathrm{}\newline \;\;\sigma_1 =\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_2 =\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_3 =L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_4 =L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right) \end{array} $$ 
 

```matlab
tao_3 = diff(D_theta3,theta_O_1)*theta_dot_O_1 + diff(D_theta3,theta_1_2)*theta_dot_1_2 + diff(D_theta3,theta_2_3)*theta_dot_2_3 + diff(D_theta3,theta_dot_O_1)*theta_ddot_O_1+ diff(D_theta3,theta_dot_1_2)*theta_ddot_1_2+ diff(D_theta3,theta_dot_2_3)*theta_ddot_2_3-diff(La,theta_2_3)
```
tao_3 = 

  $$ \displaystyle \begin{array}{l} {\dot{\theta} }_{2,3} \,{\left(m_3 \,x_{3,\textrm{C3}} \,\sigma_9 \,\sigma_4 +m_3 \,x_{3,\textrm{C3}} \,\sigma_{11} \,\sigma_1 -m_3 \,x_{3,\textrm{C3}} \,\sigma_{11} \,\sigma_3 -m_3 \,x_{3,\textrm{C3}} \,\sigma_9 \,\sigma_2 \right)}+{\ddot{\theta} }_{1,2} \,{\left(I_{\textrm{zz3}} +m_3 \,x_{3,\textrm{C3}} \,\sigma_{11} \,{\left(\sigma_{12} +x_{3,\textrm{C3}} \,\sigma_{11} \right)}+m_3 \,x_{3,\textrm{C3}} \,\sigma_9 \,{\left(x_{3,\textrm{C3}} \,\sigma_9 +\sigma_{10} \right)}\right)}+{\ddot{\theta} }_{2,3} \,{\left(m_3 \,{x_{3,\textrm{C3}} }^2 \,{\sigma_{11} }^2 +m_3 \,{x_{3,\textrm{C3}} }^2 \,{\sigma_9 }^2 +I_{\textrm{zz3}} \right)}+{\dot{\theta} }_{1,2} \,{\left(m_3 \,x_{3,\textrm{C3}} \,\sigma_{11} \,\sigma_1 +m_3 \,x_{3,\textrm{C3}} \,\sigma_9 \,{\left(\sigma_8 +{\dot{\theta} }_{O,1} \,{\left(\sigma_{12} +x_{3,\textrm{C3}} \,\sigma_{11} \right)}+{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_{11} \right)}-m_3 \,x_{3,\textrm{C3}} \,\sigma_{11} \,{\left(\sigma_6 +{\dot{\theta} }_{O,1} \,{\left(x_{3,\textrm{C3}} \,\sigma_9 +\sigma_{10} \right)}+{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_9 \right)}-m_3 \,x_{3,\textrm{C3}} \,\sigma_9 \,\sigma_2 \right)}+{\ddot{\theta} }_{O,1} \,{\left(I_{\textrm{zz3}} +m_3 \,x_{3,\textrm{C3}} \,\sigma_{11} \,\sigma_7 +m_3 \,x_{3,\textrm{C3}} \,\sigma_9 \,\sigma_5 \right)}-m_3 \,\sigma_4 \,\sigma_1 +m_3 \,\sigma_2 \,\sigma_3 +g\,m_3 \,x_{3,\textrm{C3}} \,\sigma_{11} \newline \mathrm{}\newline \textrm{where}\newline \mathrm{}\newline \;\;\sigma_1 ={\dot{\theta} }_{O,1} \,\sigma_5 +\sigma_6 +{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_9 \newline \mathrm{}\newline \;\;\sigma_2 ={\dot{\theta} }_{O,1} \,\sigma_7 +\sigma_8 +{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_{11} \newline \mathrm{}\newline \;\;\sigma_3 ={\dot{\theta} }_{1,2} \,x_{3,\textrm{C3}} \,\sigma_9 +{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_9 +{\dot{\theta} }_{O,1} \,x_{3,\textrm{C3}} \,\sigma_9 \newline \mathrm{}\newline \;\;\sigma_4 ={\dot{\theta} }_{1,2} \,x_{3,\textrm{C3}} \,\sigma_{11} +{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_{11} +{\dot{\theta} }_{O,1} \,x_{3,\textrm{C3}} \,\sigma_{11} \newline \mathrm{}\newline \;\;\sigma_5 =x_{3,\textrm{C3}} \,\sigma_9 +\sigma_{10} +L_1 \,\sin \left(\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_6 ={\dot{\theta} }_{1,2} \,{\left(x_{3,\textrm{C3}} \,\sigma_9 +\sigma_{10} \right)}\newline \mathrm{}\newline \;\;\sigma_7 =\sigma_{12} +L_1 \,\cos \left(\theta_{O,1} \right)+x_{3,\textrm{C3}} \,\sigma_{11} \newline \mathrm{}\newline \;\;\sigma_8 ={\dot{\theta} }_{1,2} \,{\left(\sigma_{12} +x_{3,\textrm{C3}} \,\sigma_{11} \right)}\newline \mathrm{}\newline \;\;\sigma_9 =\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_{10} =L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_{11} =\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\newline \mathrm{}\newline \;\;\sigma_{12} =L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right) \end{array} $$ 
 

```matlab

tao = [tao_1;tao_2;tao_3]
```
tao = 

  $$ \displaystyle \begin{array}{l} \left(\begin{array}{c} {\ddot{\theta} }_{O,1} \,{\left(I_{\textrm{zz1}} +I_{\textrm{zz2}} +I_{\textrm{zz3}} +m_3 \,{\sigma_{12} }^2 +{L_1 }^2 \,m_2 +m_3 \,{\sigma_{14} }^2 +m_1 \,{x_{1,\textrm{C1}} }^2 +m_2 \,{x_{2,\textrm{C2}} }^2 +2\,L_1 \,m_2 \,x_{2,\textrm{C2}} \,\cos \left(\theta_{1,2} \right)\right)}-{\dot{\theta} }_{1,2} \,{\left(m_3 \,\sigma_2 \,\sigma_{12} -m_3 \,\sigma_{14} \,\sigma_1 -\sigma_9 +\sigma_8 +L_1 \,m_2 \,{\dot{\theta} }_{1,2} \,x_{2,\textrm{C2}} \,\sin \left(\theta_{1,2} \right)+2\,L_1 \,m_2 \,{\dot{\theta} }_{O,1} \,x_{2,\textrm{C2}} \,\sin \left(\theta_{1,2} \right)\right)}+{\ddot{\theta} }_{1,2} \,\sigma_3 -{\dot{\theta} }_{2,3} \,{\left(m_3 \,\sigma_{12} \,\sigma_5 -m_3 \,\sigma_{14} \,\sigma_4 -m_3 \,x_{3,\textrm{C3}} \,\sigma_{16} \,\sigma_{11} +m_3 \,x_{3,\textrm{C3}} \,\sigma_{18} \,\sigma_{10} \right)}+{\ddot{\theta} }_{2,3} \,\sigma_7 +g\,m_3 \,\sigma_{12} +g\,m_2 \,{\left(x_{2,\textrm{C2}} \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)+L_1 \,\cos \left(\theta_{O,1} \right)\right)}+g\,m_1 \,x_{1,\textrm{C1}} \,\cos \left(\theta_{O,1} \right)\newline {\ddot{\theta} }_{1,2} \,{\left(I_{\textrm{zz2}} +I_{\textrm{zz3}} +m_2 \,{x_{2,\textrm{C2}} }^2 +m_3 \,{{\left(\sigma_{17} +x_{3,\textrm{C3}} \,\sigma_{16} \right)}}^2 +m_3 \,{{\left(x_{3,\textrm{C3}} \,\sigma_{18} +\sigma_{19} \right)}}^2 \right)}-{\dot{\theta} }_{1,2} \,{\left(m_3 \,{\left(\sigma_{17} +x_{3,\textrm{C3}} \,\sigma_{16} \right)}\,\sigma_2 -m_3 \,{\left(x_{3,\textrm{C3}} \,\sigma_{18} +\sigma_{19} \right)}\,\sigma_1 -\sigma_9 +\sigma_8 +L_1 \,m_2 \,{\dot{\theta} }_{O,1} \,x_{2,\textrm{C2}} \,\sin \left(\theta_{1,2} \right)\right)}+{\ddot{\theta} }_{O,1} \,\sigma_3 +{\dot{\theta} }_{2,3} \,{\left(m_3 \,{\left(x_{3,\textrm{C3}} \,\sigma_{18} +\sigma_{19} \right)}\,\sigma_4 -m_3 \,{\left(\sigma_{17} +x_{3,\textrm{C3}} \,\sigma_{16} \right)}\,\sigma_5 +m_3 \,x_{3,\textrm{C3}} \,\sigma_{16} \,\sigma_{11} -m_3 \,x_{3,\textrm{C3}} \,\sigma_{18} \,\sigma_{10} \right)}+{\ddot{\theta} }_{2,3} \,\sigma_6 +m_3 \,\sigma_2 \,\sigma_{10} -m_3 \,\sigma_{11} \,\sigma_1 +g\,m_3 \,{\left(\sigma_{17} +x_{3,\textrm{C3}} \,\sigma_{16} \right)}+g\,m_2 \,x_{2,\textrm{C2}} \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)+L_1 \,m_2 \,{{\dot{\theta} }_{O,1} }^2 \,x_{2,\textrm{C2}} \,\sin \left(\theta_{1,2} \right)+L_1 \,m_2 \,{\dot{\theta} }_{1,2} \,{\dot{\theta} }_{O,1} \,x_{2,\textrm{C2}} \,\sin \left(\theta_{1,2} \right)\newline {\dot{\theta} }_{2,3} \,{\left(m_3 \,x_{3,\textrm{C3}} \,\sigma_{18} \,\sigma_4 +m_3 \,x_{3,\textrm{C3}} \,\sigma_{16} \,\sigma_{11} -m_3 \,x_{3,\textrm{C3}} \,\sigma_{16} \,\sigma_5 -m_3 \,x_{3,\textrm{C3}} \,\sigma_{18} \,\sigma_{10} \right)}+{\ddot{\theta} }_{1,2} \,\sigma_6 +{\ddot{\theta} }_{2,3} \,{\left(m_3 \,{x_{3,\textrm{C3}} }^2 \,{\sigma_{16} }^2 +m_3 \,{x_{3,\textrm{C3}} }^2 \,{\sigma_{18} }^2 +I_{\textrm{zz3}} \right)}+{\dot{\theta} }_{1,2} \,{\left(m_3 \,x_{3,\textrm{C3}} \,\sigma_{16} \,\sigma_{11} +m_3 \,x_{3,\textrm{C3}} \,\sigma_{18} \,\sigma_1 -m_3 \,x_{3,\textrm{C3}} \,\sigma_{16} \,\sigma_2 -m_3 \,x_{3,\textrm{C3}} \,\sigma_{18} \,\sigma_{10} \right)}+{\ddot{\theta} }_{O,1} \,\sigma_7 -m_3 \,\sigma_4 \,\sigma_{11} +m_3 \,\sigma_{10} \,\sigma_5 +g\,m_3 \,x_{3,\textrm{C3}} \,\sigma_{16}  \end{array}\right)\\\mathrm{}\\\textrm{where}\\\mathrm{}\\\;\;\sigma_1 =\sigma_{13} +{\dot{\theta} }_{O,1} \,{\left(\sigma_{17} +x_{3,\textrm{C3}} \,\sigma_{16} \right)}+{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_{16} \\\mathrm{}\\\;\;\sigma_2 =\sigma_{15} +{\dot{\theta} }_{O,1} \,{\left(x_{3,\textrm{C3}} \,\sigma_{18} +\sigma_{19} \right)}+{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_{18} \\\mathrm{}\\\;\;\sigma_3 =m_2 \,{x_{2,\textrm{C2}} }^2 +L_1 \,m_2 \,\cos \left(\theta_{1,2} \right)\,x_{2,\textrm{C2}} +I_{\textrm{zz2}} +I_{\textrm{zz3}} +m_3 \,{\left(\sigma_{17} +x_{3,\textrm{C3}} \,\sigma_{16} \right)}\,\sigma_{12} +m_3 \,{\left(x_{3,\textrm{C3}} \,\sigma_{18} +\sigma_{19} \right)}\,\sigma_{14} \\\mathrm{}\\\;\;\sigma_4 ={\dot{\theta} }_{1,2} \,x_{3,\textrm{C3}} \,\sigma_{16} +{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_{16} +{\dot{\theta} }_{O,1} \,x_{3,\textrm{C3}} \,\sigma_{16} \\\mathrm{}\\\;\;\sigma_5 ={\dot{\theta} }_{1,2} \,x_{3,\textrm{C3}} \,\sigma_{18} +{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_{18} +{\dot{\theta} }_{O,1} \,x_{3,\textrm{C3}} \,\sigma_{18} \\\mathrm{}\\\;\;\sigma_6 =I_{\textrm{zz3}} +m_3 \,x_{3,\textrm{C3}} \,\sigma_{16} \,{\left(\sigma_{17} +x_{3,\textrm{C3}} \,\sigma_{16} \right)}+m_3 \,x_{3,\textrm{C3}} \,\sigma_{18} \,{\left(x_{3,\textrm{C3}} \,\sigma_{18} +\sigma_{19} \right)}\\\mathrm{}\\\;\;\sigma_7 =I_{\textrm{zz3}} +m_3 \,x_{3,\textrm{C3}} \,\sigma_{16} \,\sigma_{12} +m_3 \,x_{3,\textrm{C3}} \,\sigma_{18} \,\sigma_{14} \\\mathrm{}\\\;\;\sigma_8 =m_3 \,{\left(x_{3,\textrm{C3}} \,\sigma_{18} +\sigma_{19} \right)}\,\sigma_{10} \\\mathrm{}\\\;\;\sigma_9 =m_3 \,{\left(\sigma_{17} +x_{3,\textrm{C3}} \,\sigma_{16} \right)}\,\sigma_{11} \\\mathrm{}\\\;\;\sigma_{10} ={\dot{\theta} }_{O,1} \,\sigma_{12} +\sigma_{13} +{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_{16} \\\mathrm{}\\\;\;\sigma_{11} ={\dot{\theta} }_{O,1} \,\sigma_{14} +\sigma_{15} +{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_{18} \\\mathrm{}\\\;\;\sigma_{12} =\sigma_{17} +L_1 \,\cos \left(\theta_{O,1} \right)+x_{3,\textrm{C3}} \,\sigma_{16} \\\mathrm{}\\\;\;\sigma_{13} ={\dot{\theta} }_{1,2} \,{\left(\sigma_{17} +x_{3,\textrm{C3}} \,\sigma_{16} \right)}\\\mathrm{}\\\;\;\sigma_{14} =x_{3,\textrm{C3}} \,\sigma_{18} +\sigma_{19} +L_1 \,\sin \left(\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_{15} ={\dot{\theta} }_{1,2} \,{\left(x_{3,\textrm{C3}} \,\sigma_{18} +\sigma_{19} \right)}\\\mathrm{}\\\;\;\sigma_{16} =\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_{17} =L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_{18} =\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_{19} =L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\end{array} $$ 
 
# Cálculo de la matriz de inercia

Esta matriz de inercia del sistema se obtiene a partir de las expresiones de los pares articulares calculados mediante el método de Lagrange, identificando los coeficientes que multiplican a las aceleraciones articulares. En esta sección, se organiza el modelo dinámico separando los términos que dependen de $\ddot{\theta}$, lo cual permite construir la matriz M(θ), que representa la inercia del manipulador en función de su configuración. Esta matriz es simétrica y depende únicamente de las variables articulares, reflejando cómo la distribución de masa del sistema afecta su resistencia al movimiento.


Para caulcular la matriz se aislan dichos coeficientes dentro de las expresiones de los torques, lo que permite estructurar de forma matricial la dinámica del robot.

```matlab
% Cálculo de la matriz de inercia

M1 = subs(tao,[theta_ddot_O_1,theta_ddot_1_2,theta_ddot_2_3,theta_dot_O_1,theta_dot_1_2,theta_dot_2_3,g],[1,0,0,0,0,0,0])
```
M1 = 

  $$ \displaystyle \begin{array}{l} \left(\begin{array}{c} I_{\textrm{zz1}} +I_{\textrm{zz2}} +I_{\textrm{zz3}} +m_3 \,{\sigma_1 }^2 +{L_1 }^2 \,m_2 +m_3 \,{\sigma_2 }^2 +m_1 \,{x_{1,\textrm{C1}} }^2 +m_2 \,{x_{2,\textrm{C2}} }^2 +2\,L_1 \,m_2 \,x_{2,\textrm{C2}} \,\cos \left(\theta_{1,2} \right)\newline m_2 \,{x_{2,\textrm{C2}} }^2 +L_1 \,m_2 \,\cos \left(\theta_{1,2} \right)\,x_{2,\textrm{C2}} +I_{\textrm{zz2}} +I_{\textrm{zz3}} +m_3 \,{\left(\sigma_3 +x_{3,\textrm{C3}} \,\sigma_4 \right)}\,\sigma_1 +m_3 \,{\left(x_{3,\textrm{C3}} \,\sigma_6 +\sigma_5 \right)}\,\sigma_2 \newline I_{\textrm{zz3}} +m_3 \,x_{3,\textrm{C3}} \,\sigma_4 \,\sigma_1 +m_3 \,x_{3,\textrm{C3}} \,\sigma_6 \,\sigma_2  \end{array}\right)\\\mathrm{}\\\textrm{where}\\\mathrm{}\\\;\;\sigma_1 =\sigma_3 +L_1 \,\cos \left(\theta_{O,1} \right)+x_{3,\textrm{C3}} \,\sigma_4 \\\mathrm{}\\\;\;\sigma_2 =x_{3,\textrm{C3}} \,\sigma_6 +\sigma_5 +L_1 \,\sin \left(\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_3 =L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_4 =\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_5 =L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_6 =\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\end{array} $$ 
 

```matlab
M2 = subs(tao,[theta_ddot_O_1,theta_ddot_1_2,theta_ddot_2_3,theta_dot_O_1,theta_dot_1_2,theta_dot_2_3,g],[0,1,0,0,0,0,0])
```
M2 = 

  $$ \displaystyle \begin{array}{l} \left(\begin{array}{c} m_2 \,{x_{2,\textrm{C2}} }^2 +L_1 \,m_2 \,\cos \left(\theta_{1,2} \right)\,x_{2,\textrm{C2}} +I_{\textrm{zz2}} +I_{\textrm{zz3}} +m_3 \,{\left(\sigma_2 +x_{3,\textrm{C3}} \,\sigma_4 \right)}\,{\left(\sigma_2 +L_1 \,\cos \left(\theta_{O,1} \right)+x_{3,\textrm{C3}} \,\sigma_4 \right)}+m_3 \,{\left(x_{3,\textrm{C3}} \,\sigma_3 +\sigma_1 \right)}\,{\left(x_{3,\textrm{C3}} \,\sigma_3 +\sigma_1 +L_1 \,\sin \left(\theta_{O,1} \right)\right)}\newline I_{\textrm{zz2}} +I_{\textrm{zz3}} +m_2 \,{x_{2,\textrm{C2}} }^2 +m_3 \,{{\left(\sigma_2 +x_{3,\textrm{C3}} \,\sigma_4 \right)}}^2 +m_3 \,{{\left(x_{3,\textrm{C3}} \,\sigma_3 +\sigma_1 \right)}}^2 \newline I_{\textrm{zz3}} +m_3 \,x_{3,\textrm{C3}} \,\sigma_4 \,{\left(\sigma_2 +x_{3,\textrm{C3}} \,\sigma_4 \right)}+m_3 \,x_{3,\textrm{C3}} \,\sigma_3 \,{\left(x_{3,\textrm{C3}} \,\sigma_3 +\sigma_1 \right)} \end{array}\right)\\\mathrm{}\\\textrm{where}\\\mathrm{}\\\;\;\sigma_1 =L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_2 =L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_3 =\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_4 =\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\end{array} $$ 
 

```matlab
M3 = subs(tao,[theta_ddot_O_1,theta_ddot_1_2,theta_ddot_2_3,theta_dot_O_1,theta_dot_1_2,theta_dot_2_3,g],[0,0,1,0,0,0,0])
```
M3 = 

  $$ \displaystyle \begin{array}{l} \left(\begin{array}{c} I_{\textrm{zz3}} +m_3 \,x_{3,\textrm{C3}} \,\sigma_2 \,{\left(\sigma_4 +L_1 \,\cos \left(\theta_{O,1} \right)+x_{3,\textrm{C3}} \,\sigma_2 \right)}+m_3 \,x_{3,\textrm{C3}} \,\sigma_1 \,{\left(x_{3,\textrm{C3}} \,\sigma_1 +\sigma_3 +L_1 \,\sin \left(\theta_{O,1} \right)\right)}\newline I_{\textrm{zz3}} +m_3 \,x_{3,\textrm{C3}} \,\sigma_2 \,{\left(\sigma_4 +x_{3,\textrm{C3}} \,\sigma_2 \right)}+m_3 \,x_{3,\textrm{C3}} \,\sigma_1 \,{\left(x_{3,\textrm{C3}} \,\sigma_1 +\sigma_3 \right)}\newline m_3 \,{x_{3,\textrm{C3}} }^2 \,{\sigma_2 }^2 +m_3 \,{x_{3,\textrm{C3}} }^2 \,{\sigma_1 }^2 +I_{\textrm{zz3}}  \end{array}\right)\\\mathrm{}\\\textrm{where}\\\mathrm{}\\\;\;\sigma_1 =\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_2 =\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_3 =L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_4 =L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\end{array} $$ 
 

```matlab

M_theta = collect([M1 M2 M3],[m_1,m_2,m_3])
```
M_theta = 

  $$ \displaystyle \begin{array}{l} \left(\begin{array}{ccc} {x_{1,\textrm{C1}} }^2 \,m_1 +{\left({L_1 }^2 +2\,\cos \left(\theta_{1,2} \right)\,L_1 \,x_{2,\textrm{C2}} +{x_{2,\textrm{C2}} }^2 \right)}\,m_2 +{\left({\sigma_4 }^2 +{\sigma_5 }^2 \right)}\,m_3 +I_{\textrm{zz1}} +I_{\textrm{zz2}} +I_{\textrm{zz3}}  & \sigma_1  & \sigma_2 \newline \sigma_1  & {x_{2,\textrm{C2}} }^2 \,m_2 +{\left({{\left(\sigma_7 +x_{3,\textrm{C3}} \,\sigma_6 \right)}}^2 +{{\left(x_{3,\textrm{C3}} \,\sigma_8 +\sigma_9 \right)}}^2 \right)}\,m_3 +I_{\textrm{zz2}} +I_{\textrm{zz3}}  & \sigma_3 \newline \sigma_2  & \sigma_3  & {\left({x_{3,\textrm{C3}} }^2 \,{\sigma_6 }^2 +{x_{3,\textrm{C3}} }^2 \,{\sigma_8 }^2 \right)}\,m_3 +I_{\textrm{zz3}}  \end{array}\right)\\\mathrm{}\\\textrm{where}\\\mathrm{}\\\;\;\sigma_1 ={\left({x_{2,\textrm{C2}} }^2 +L_1 \,\cos \left(\theta_{1,2} \right)\,x_{2,\textrm{C2}} \right)}\,m_2 +{\left({\left(\sigma_7 +x_{3,\textrm{C3}} \,\sigma_6 \right)}\,\sigma_4 +{\left(x_{3,\textrm{C3}} \,\sigma_8 +\sigma_9 \right)}\,\sigma_5 \right)}\,m_3 +I_{\textrm{zz2}} +I_{\textrm{zz3}} \\\mathrm{}\\\;\;\sigma_2 ={\left(x_{3,\textrm{C3}} \,\sigma_6 \,\sigma_4 +x_{3,\textrm{C3}} \,\sigma_8 \,\sigma_5 \right)}\,m_3 +I_{\textrm{zz3}} \\\mathrm{}\\\;\;\sigma_3 ={\left(x_{3,\textrm{C3}} \,\sigma_6 \,{\left(\sigma_7 +x_{3,\textrm{C3}} \,\sigma_6 \right)}+x_{3,\textrm{C3}} \,\sigma_8 \,{\left(x_{3,\textrm{C3}} \,\sigma_8 +\sigma_9 \right)}\right)}\,m_3 +I_{\textrm{zz3}} \\\mathrm{}\\\;\;\sigma_4 =\sigma_7 +L_1 \,\cos \left(\theta_{O,1} \right)+x_{3,\textrm{C3}} \,\sigma_6 \\\mathrm{}\\\;\;\sigma_5 =x_{3,\textrm{C3}} \,\sigma_8 +\sigma_9 +L_1 \,\sin \left(\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_6 =\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_7 =L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_8 =\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_9 =L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\end{array} $$ 
 
# Cálculo V y G 

El cálculo de V\_theta implica separar ahora los elementos dependientes de $\dot{q} \ldotp$ 

```matlab
V_theta = subs(tao,[theta_ddot_O_1,theta_ddot_1_2,theta_ddot_2_3,theta_dot_O_1,theta_dot_1_2,theta_dot_2_3,g],[0,0,0,theta_dot_O_1,theta_dot_1_2,theta_dot_2_3,0])
```
V_theta = 

  $$ \displaystyle \begin{array}{l} \left(\begin{array}{c} -{\dot{\theta} }_{1,2} \,{\left(m_3 \,\sigma_2 \,\sigma_9 -m_3 \,\sigma_{11} \,\sigma_1 -\sigma_6 +\sigma_5 +L_1 \,m_2 \,{\dot{\theta} }_{1,2} \,x_{2,\textrm{C2}} \,\sin \left(\theta_{1,2} \right)+2\,L_1 \,m_2 \,{\dot{\theta} }_{O,1} \,x_{2,\textrm{C2}} \,\sin \left(\theta_{1,2} \right)\right)}-{\dot{\theta} }_{2,3} \,{\left(m_3 \,\sigma_9 \,\sigma_4 -m_3 \,\sigma_{11} \,\sigma_3 -m_3 \,x_{3,\textrm{C3}} \,\sigma_{13} \,\sigma_8 +m_3 \,x_{3,\textrm{C3}} \,\sigma_{15} \,\sigma_7 \right)}\newline {\dot{\theta} }_{2,3} \,{\left(m_3 \,{\left(x_{3,\textrm{C3}} \,\sigma_{15} +\sigma_{16} \right)}\,\sigma_3 -m_3 \,{\left(\sigma_{14} +x_{3,\textrm{C3}} \,\sigma_{13} \right)}\,\sigma_4 +m_3 \,x_{3,\textrm{C3}} \,\sigma_{13} \,\sigma_8 -m_3 \,x_{3,\textrm{C3}} \,\sigma_{15} \,\sigma_7 \right)}-{\dot{\theta} }_{1,2} \,{\left(m_3 \,{\left(\sigma_{14} +x_{3,\textrm{C3}} \,\sigma_{13} \right)}\,\sigma_2 -m_3 \,{\left(x_{3,\textrm{C3}} \,\sigma_{15} +\sigma_{16} \right)}\,\sigma_1 -\sigma_6 +\sigma_5 +L_1 \,m_2 \,{\dot{\theta} }_{O,1} \,x_{2,\textrm{C2}} \,\sin \left(\theta_{1,2} \right)\right)}+m_3 \,\sigma_2 \,\sigma_7 -m_3 \,\sigma_8 \,\sigma_1 +L_1 \,m_2 \,{{\dot{\theta} }_{O,1} }^2 \,x_{2,\textrm{C2}} \,\sin \left(\theta_{1,2} \right)+L_1 \,m_2 \,{\dot{\theta} }_{1,2} \,{\dot{\theta} }_{O,1} \,x_{2,\textrm{C2}} \,\sin \left(\theta_{1,2} \right)\newline {\dot{\theta} }_{2,3} \,{\left(m_3 \,x_{3,\textrm{C3}} \,\sigma_{15} \,\sigma_3 +m_3 \,x_{3,\textrm{C3}} \,\sigma_{13} \,\sigma_8 -m_3 \,x_{3,\textrm{C3}} \,\sigma_{13} \,\sigma_4 -m_3 \,x_{3,\textrm{C3}} \,\sigma_{15} \,\sigma_7 \right)}+{\dot{\theta} }_{1,2} \,{\left(m_3 \,x_{3,\textrm{C3}} \,\sigma_{13} \,\sigma_8 +m_3 \,x_{3,\textrm{C3}} \,\sigma_{15} \,\sigma_1 -m_3 \,x_{3,\textrm{C3}} \,\sigma_{13} \,\sigma_2 -m_3 \,x_{3,\textrm{C3}} \,\sigma_{15} \,\sigma_7 \right)}-m_3 \,\sigma_3 \,\sigma_8 +m_3 \,\sigma_7 \,\sigma_4  \end{array}\right)\\\mathrm{}\\\textrm{where}\\\mathrm{}\\\;\;\sigma_1 =\sigma_{10} +{\dot{\theta} }_{O,1} \,{\left(\sigma_{14} +x_{3,\textrm{C3}} \,\sigma_{13} \right)}+{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_{13} \\\mathrm{}\\\;\;\sigma_2 =\sigma_{12} +{\dot{\theta} }_{O,1} \,{\left(x_{3,\textrm{C3}} \,\sigma_{15} +\sigma_{16} \right)}+{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_{15} \\\mathrm{}\\\;\;\sigma_3 ={\dot{\theta} }_{1,2} \,x_{3,\textrm{C3}} \,\sigma_{13} +{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_{13} +{\dot{\theta} }_{O,1} \,x_{3,\textrm{C3}} \,\sigma_{13} \\\mathrm{}\\\;\;\sigma_4 ={\dot{\theta} }_{1,2} \,x_{3,\textrm{C3}} \,\sigma_{15} +{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_{15} +{\dot{\theta} }_{O,1} \,x_{3,\textrm{C3}} \,\sigma_{15} \\\mathrm{}\\\;\;\sigma_5 =m_3 \,{\left(x_{3,\textrm{C3}} \,\sigma_{15} +\sigma_{16} \right)}\,\sigma_7 \\\mathrm{}\\\;\;\sigma_6 =m_3 \,{\left(\sigma_{14} +x_{3,\textrm{C3}} \,\sigma_{13} \right)}\,\sigma_8 \\\mathrm{}\\\;\;\sigma_7 ={\dot{\theta} }_{O,1} \,\sigma_9 +\sigma_{10} +{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_{13} \\\mathrm{}\\\;\;\sigma_8 ={\dot{\theta} }_{O,1} \,\sigma_{11} +\sigma_{12} +{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_{15} \\\mathrm{}\\\;\;\sigma_9 =\sigma_{14} +L_1 \,\cos \left(\theta_{O,1} \right)+x_{3,\textrm{C3}} \,\sigma_{13} \\\mathrm{}\\\;\;\sigma_{10} ={\dot{\theta} }_{1,2} \,{\left(\sigma_{14} +x_{3,\textrm{C3}} \,\sigma_{13} \right)}\\\mathrm{}\\\;\;\sigma_{11} =x_{3,\textrm{C3}} \,\sigma_{15} +\sigma_{16} +L_1 \,\sin \left(\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_{12} ={\dot{\theta} }_{1,2} \,{\left(x_{3,\textrm{C3}} \,\sigma_{15} +\sigma_{16} \right)}\\\mathrm{}\\\;\;\sigma_{13} =\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_{14} =L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_{15} =\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_{16} =L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\end{array} $$ 
 

Este vector aisla la gravedad, y con la definición de este vector ya se tienen todas las herramientas necesarias para definir al vector de pares total del sistema.

```matlab
G_theta = subs(tao,[theta_ddot_O_1,theta_ddot_1_2,theta_ddot_2_3,theta_dot_O_1,theta_dot_1_2,theta_dot_2_3,g],[0,0,0,0,0,0,g])
```
G_theta = 

  $$ \displaystyle \begin{array}{l} \left(\begin{array}{c} g\,m_3 \,{\left(\sigma_2 +L_1 \,\cos \left(\theta_{O,1} \right)+\sigma_1 \right)}+g\,m_2 \,{\left(x_{2,\textrm{C2}} \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)+L_1 \,\cos \left(\theta_{O,1} \right)\right)}+g\,m_1 \,x_{1,\textrm{C1}} \,\cos \left(\theta_{O,1} \right)\newline g\,m_3 \,{\left(\sigma_2 +\sigma_1 \right)}+g\,m_2 \,x_{2,\textrm{C2}} \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\newline g\,m_3 \,x_{3,\textrm{C3}} \,\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right) \end{array}\right)\\\mathrm{}\\\textrm{where}\\\mathrm{}\\\;\;\sigma_1 =x_{3,\textrm{C3}} \,\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_2 =L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\end{array} $$ 
 
# Cálculo del jacobiano inverso por el vector de fuerzas externas

El vector de fuerzas externas estas formado a su vez por una componente de fuerzas y una componente de momentos. En este caso existen dos componentes de fuerza ejercidas en el plano, correspondientes a la fuerza en X y a la fuerza en Y. Además, al tratarse de un sistema planar, el único eje sobre el que existen los momentos es el eje Z. De esta manera el vector de fuerzas externas queda definido como:

 $$ F_{\textrm{ext}\;} =\left\lbrack \begin{array}{c} F_x \newline F_{y\;} \newline M_z  \end{array}\right\rbrack $$ 

Por lo que el último sumando necesario para obtener el vector de esfuerzos se calcula de: 

```matlab
syms F_ext F_x F_y M_z
F_ext=[F_x;F_y;M_z]
```
F_ext = 

  $$ \displaystyle \left(\begin{array}{c} F_x \newline F_y \newline M_z  \end{array}\right) $$ 
 

```matlab
transpose(J_theta)*F_ext
```
ans = 

  $$ \displaystyle \begin{array}{l} \left(\begin{array}{c} M_z +F_y \,{\left(\sigma_4 +L_1 \,\cos \left(\theta_{O,1} \right)+\sigma_2 \right)}-F_x \,{\left(\sigma_3 +L_1 \,\sin \left(\theta_{O,1} \right)+\sigma_1 \right)}\newline M_z +F_y \,{\left(\sigma_4 +\sigma_2 \right)}-F_x \,{\left(\sigma_3 +\sigma_1 \right)}\newline M_z +F_y \,L_3 \,\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)-F_x \,L_3 \,\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right) \end{array}\right)\\\mathrm{}\\\textrm{where}\\\mathrm{}\\\;\;\sigma_1 =L_3 \,\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_2 =L_3 \,\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_3 =L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_4 =L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\end{array} $$ 
 

## Modelo dinámico inverso del robot
## $$ \;F_{\textrm{ext}} =J_{\theta } {\left(q\right)\;}^{-T} \left({\;\tau }_{\theta } -M\left(q\right)-V\left(q,\dot{q} \right)-G\left(q\right)\right) $$

En la dinámica inversa del robot se determian ahora los componentes dle vector de fuerzas externas, es decir, los torques o fuerzas que deben aplicarse en cada una de sus articulaciones para que el sistema siga una trayectoria deseada de posición, velocidad y aceleración. 


Matemáticamente, la dinámica inversa se expresa como una relación entre el vector de torques articulares y los términos dinámicos del sistema: la matriz de inercia que depende de la configuración del robot, el vector que agrupa los efectos de velocidad y el vector de gravedad. De esta forma, al proporcionar las trayectorias articulares deseadas, es posible calcular directamente los esfuerzos requeridos en cada articulación para lograr dicho movimiento.


Una vez que se tiene el análisis de la dinámica directa de un robot, es bástante más sencillo el cálculo inverso, ya que $M\left(q\right),\;V\left(q,\dot{q} \right)\;y\;G\left(q\right)\;$ mantienen sus valores iguales, pues no dependen del vector de esfuerzos. Dicho lo anterior, queda claro que el vector de fuerzas externas depende de la nueva definición del vector de esfuerzos.

```matlab
syms tau_inv tau_1_inv tau_2_inv tau_3_inv 
syms Fex x_ddot_O_P_inv y_ddot_O_P_inv theta_ddot_O_P_inv
tau_inv = [tau_1_inv;tau_2_inv;tau_3_inv]
```
tau_inv = 

  $$ \displaystyle \left(\begin{array}{c} \tau_{1,\textrm{inv}} \newline \tau_{2,\textrm{inv}} \newline \tau_{3,\textrm{inv}}  \end{array}\right) $$ 
 

```matlab
q_ddot_inv = [x_ddot_O_P_inv;y_ddot_O_P_inv;theta_dot_O_P_inv]
```
q_ddot_inv = 

  $$ \displaystyle \left(\begin{array}{c} {\ddot{x} }_{O,P,\textrm{inv}} \newline {\ddot{y} }_{O,P,\textrm{inv}} \newline {\dot{\theta} }_{O,P,\textrm{inv}}  \end{array}\right) $$ 
 

```matlab
Fex = inv(transpose(J_theta))*(tau_inv-M_theta*q_ddot-V_theta-G_theta)
```
Fex = 

  $$ \displaystyle \begin{array}{l} \left(\begin{array}{c} \frac{\cos \left(\theta_{O,1} \right)\,\sigma_3 }{\sigma_{32} -\sigma_{31} }-\frac{\cos \left(\theta_{1,2} +\theta_{O,1} \right)\,\sigma_2 }{\sigma_{25} }-\frac{\sigma_{21} \,\sigma_1 }{\sigma_{18} }\newline \frac{\sin \left(\theta_{O,1} \right)\,\sigma_3 }{\sigma_{32} -\sigma_{31} }-\frac{\sin \left(\theta_{1,2} +\theta_{O,1} \right)\,\sigma_2 }{\sigma_{25} }-\frac{\sigma_{20} \,\sigma_1 }{\sigma_{18} }\newline \frac{\sigma_{26} \,\sigma_2 }{\sigma_{25} }+\frac{\sigma_{19} \,\sigma_1 }{\sigma_{18} }-\frac{\sigma_{22} \,\sigma_3 }{\sigma_{32} -\sigma_{31} } \end{array}\right)\\\mathrm{}\\\textrm{where}\\\mathrm{}\\\;\;\sigma_1 =\sigma_7 \,{\left(m_2 \,{x_{2,\textrm{C2}} }^2 +I_{\textrm{zz2}} +I_{\textrm{zz3}} +m_3 \,{\left({{\left(\sigma_{36} +x_{3,\textrm{C3}} \,\sigma_{35} \right)}}^2 +{{\left(x_{3,\textrm{C3}} \,\sigma_{37} +\sigma_{38} \right)}}^2 \right)}\right)}-{\dot{\theta} }_{1,2} \,{\left(m_3 \,{\left(\sigma_{36} +x_{3,\textrm{C3}} \,\sigma_{35} \right)}\,\sigma_{11} -m_3 \,{\left(x_{3,\textrm{C3}} \,\sigma_{37} +\sigma_{38} \right)}\,\sigma_{10} -\sigma_5 +\sigma_6 +L_1 \,m_2 \,{\dot{\theta} }_{O,1} \,x_{2,\textrm{C2}} \,\sin \left(\theta_{1,2} \right)\right)}-\sigma_{14} \,\sigma_8 -\tau_{2,\textrm{inv}} +{\dot{\theta} }_{2,3} \,{\left(m_3 \,{\left(x_{3,\textrm{C3}} \,\sigma_{37} +\sigma_{38} \right)}\,\sigma_{13} -m_3 \,{\left(\sigma_{36} +x_{3,\textrm{C3}} \,\sigma_{35} \right)}\,\sigma_{12} +m_3 \,x_{3,\textrm{C3}} \,\sigma_{35} \,\sigma_{16} -m_3 \,x_{3,\textrm{C3}} \,\sigma_{37} \,\sigma_{17} \right)}-\sigma_9 \,\sigma_4 +m_3 \,\sigma_{11} \,\sigma_{17} -m_3 \,\sigma_{16} \,\sigma_{10} +g\,m_3 \,{\left(\sigma_{36} +x_{3,\textrm{C3}} \,\sigma_{35} \right)}+g\,m_2 \,x_{2,\textrm{C2}} \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)+L_1 \,m_2 \,{{\dot{\theta} }_{O,1} }^2 \,x_{2,\textrm{C2}} \,\sin \left(\theta_{1,2} \right)+L_1 \,m_2 \,{\dot{\theta} }_{1,2} \,{\dot{\theta} }_{O,1} \,x_{2,\textrm{C2}} \,\sin \left(\theta_{1,2} \right)\\\mathrm{}\\\;\;\sigma_2 =\tau_{1,\textrm{inv}} +{\dot{\theta} }_{1,2} \,{\left(m_3 \,\sigma_{11} \,\sigma_{29} -m_3 \,\sigma_{27} \,\sigma_{10} -\sigma_5 +\sigma_6 +L_1 \,m_2 \,{\dot{\theta} }_{1,2} \,x_{2,\textrm{C2}} \,\sin \left(\theta_{1,2} \right)+2\,L_1 \,m_2 \,{\dot{\theta} }_{O,1} \,x_{2,\textrm{C2}} \,\sin \left(\theta_{1,2} \right)\right)}-\sigma_7 \,\sigma_4 +\sigma_{15} \,\sigma_8 +{\dot{\theta} }_{2,3} \,{\left(m_3 \,\sigma_{29} \,\sigma_{12} -m_3 \,\sigma_{27} \,\sigma_{13} -m_3 \,x_{3,\textrm{C3}} \,\sigma_{35} \,\sigma_{16} +m_3 \,x_{3,\textrm{C3}} \,\sigma_{37} \,\sigma_{17} \right)}+\sigma_9 \,{\left(m_1 \,{x_{1,\textrm{C1}} }^2 +I_{\textrm{zz1}} +I_{\textrm{zz2}} +I_{\textrm{zz3}} +m_3 \,{\left({\sigma_{29} }^2 +{\sigma_{27} }^2 \right)}+m_2 \,{\left({L_1 }^2 +2\,\cos \left(\theta_{1,2} \right)\,L_1 \,x_{2,\textrm{C2}} +{x_{2,\textrm{C2}} }^2 \right)}\right)}-g\,m_3 \,\sigma_{29} -g\,m_2 \,{\left(x_{2,\textrm{C2}} \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)+L_1 \,\cos \left(\theta_{O,1} \right)\right)}-g\,m_1 \,x_{1,\textrm{C1}} \,\cos \left(\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_3 ={\dot{\theta} }_{2,3} \,{\left(m_3 \,x_{3,\textrm{C3}} \,\sigma_{37} \,\sigma_{13} +m_3 \,x_{3,\textrm{C3}} \,\sigma_{35} \,\sigma_{16} -m_3 \,x_{3,\textrm{C3}} \,\sigma_{35} \,\sigma_{12} -m_3 \,x_{3,\textrm{C3}} \,\sigma_{37} \,\sigma_{17} \right)}-\sigma_{15} \,\sigma_9 -{\left(I_{\textrm{zz3}} +m_3 \,{\left({x_{3,\textrm{C3}} }^2 \,{\sigma_{35} }^2 +{x_{3,\textrm{C3}} }^2 \,{\sigma_{37} }^2 \right)}\right)}\,\sigma_8 -\tau_{3,\textrm{inv}} +\sigma_{14} \,\sigma_7 +{\dot{\theta} }_{1,2} \,{\left(m_3 \,x_{3,\textrm{C3}} \,\sigma_{35} \,\sigma_{16} +m_3 \,x_{3,\textrm{C3}} \,\sigma_{37} \,\sigma_{10} -m_3 \,x_{3,\textrm{C3}} \,\sigma_{35} \,\sigma_{11} -m_3 \,x_{3,\textrm{C3}} \,\sigma_{37} \,\sigma_{17} \right)}-m_3 \,\sigma_{13} \,\sigma_{16} +m_3 \,\sigma_{17} \,\sigma_{12} +g\,m_3 \,x_{3,\textrm{C3}} \,\sigma_{35} \\\mathrm{}\\\;\;\sigma_4 =I_{\textrm{zz2}} +I_{\textrm{zz3}} +m_2 \,{\left({x_{2,\textrm{C2}} }^2 +L_1 \,\cos \left(\theta_{1,2} \right)\,x_{2,\textrm{C2}} \right)}+m_3 \,{\left({\left(\sigma_{36} +x_{3,\textrm{C3}} \,\sigma_{35} \right)}\,\sigma_{29} +{\left(x_{3,\textrm{C3}} \,\sigma_{37} +\sigma_{38} \right)}\,\sigma_{27} \right)}\\\mathrm{}\\\;\;\sigma_5 =m_3 \,{\left(\sigma_{36} +x_{3,\textrm{C3}} \,\sigma_{35} \right)}\,\sigma_{16} \\\mathrm{}\\\;\;\sigma_6 =m_3 \,{\left(x_{3,\textrm{C3}} \,\sigma_{37} +\sigma_{38} \right)}\,\sigma_{17} \\\mathrm{}\\\;\;\sigma_7 =\frac{\sigma_{21} \,\sigma_{23} }{\sigma_{18} }-\frac{{\ddot{\theta} }_{O,P,\textrm{inv}} \,\sigma_{19} }{\sigma_{18} }+\frac{\sigma_{20} \,\sigma_{24} }{\sigma_{18} }\\\mathrm{}\\\;\;\sigma_8 =\frac{\cos \left(\theta_{O,1} \right)\,\sigma_{23} }{\sigma_{32} -\sigma_{31} }-\frac{{\ddot{\theta} }_{O,P,\textrm{inv}} \,\sigma_{22} }{\sigma_{32} -\sigma_{31} }+\frac{\sin \left(\theta_{O,1} \right)\,\sigma_{24} }{\sigma_{32} -\sigma_{31} }\\\mathrm{}\\\;\;\sigma_9 =\frac{\sin \left(\theta_{1,2} +\theta_{O,1} \right)\,\sigma_{24} }{\sigma_{25} }-\frac{{\ddot{\theta} }_{O,P,\textrm{inv}} \,\sigma_{26} }{\sigma_{25} }+\frac{\cos \left(\theta_{1,2} +\theta_{O,1} \right)\,\sigma_{23} }{\sigma_{25} }\\\mathrm{}\\\;\;\sigma_{10} =\sigma_{30} +{\dot{\theta} }_{O,1} \,{\left(\sigma_{36} +x_{3,\textrm{C3}} \,\sigma_{35} \right)}+{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_{35} \\\mathrm{}\\\;\;\sigma_{11} =\sigma_{28} +{\dot{\theta} }_{O,1} \,{\left(x_{3,\textrm{C3}} \,\sigma_{37} +\sigma_{38} \right)}+{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_{37} \\\mathrm{}\\\;\;\sigma_{12} ={\dot{\theta} }_{1,2} \,x_{3,\textrm{C3}} \,\sigma_{37} +{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_{37} +{\dot{\theta} }_{O,1} \,x_{3,\textrm{C3}} \,\sigma_{37} \\\mathrm{}\\\;\;\sigma_{13} ={\dot{\theta} }_{1,2} \,x_{3,\textrm{C3}} \,\sigma_{35} +{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_{35} +{\dot{\theta} }_{O,1} \,x_{3,\textrm{C3}} \,\sigma_{35} \\\mathrm{}\\\;\;\sigma_{14} =I_{\textrm{zz3}} +m_3 \,{\left(x_{3,\textrm{C3}} \,\sigma_{35} \,{\left(\sigma_{36} +x_{3,\textrm{C3}} \,\sigma_{35} \right)}+x_{3,\textrm{C3}} \,\sigma_{37} \,{\left(x_{3,\textrm{C3}} \,\sigma_{37} +\sigma_{38} \right)}\right)}\\\mathrm{}\\\;\;\sigma_{15} =I_{\textrm{zz3}} +m_3 \,{\left(x_{3,\textrm{C3}} \,\sigma_{35} \,\sigma_{29} +x_{3,\textrm{C3}} \,\sigma_{37} \,\sigma_{27} \right)}\\\mathrm{}\\\;\;\sigma_{16} ={\dot{\theta} }_{O,1} \,\sigma_{27} +\sigma_{28} +{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_{37} \\\mathrm{}\\\;\;\sigma_{17} ={\dot{\theta} }_{O,1} \,\sigma_{29} +\sigma_{30} +{\dot{\theta} }_{2,3} \,x_{3,\textrm{C3}} \,\sigma_{35} \\\mathrm{}\\\;\;\sigma_{18} =L_1 \,L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\,\sin \left(\theta_{O,1} \right)-L_1 \,L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\,\cos \left(\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_{19} =L_1 \,L_3 \,\sigma_{35} \,\sin \left(\theta_{O,1} \right)-L_1 \,L_3 \,\sigma_{37} \,\cos \left(\theta_{O,1} \right)+L_2 \,L_3 \,\sigma_{35} \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)-L_2 \,L_3 \,\sigma_{37} \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_{20} =\sigma_{38} +L_1 \,\sin \left(\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_{21} =\sigma_{36} +L_1 \,\cos \left(\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_{22} =\sigma_{32} -\sigma_{31} +L_3 \,\sigma_{35} \,\sin \left(\theta_{O,1} \right)-L_3 \,\sigma_{37} \,\cos \left(\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_{23} ={\ddot{x} }_{O,P,\textrm{inv}} +{\dot{\theta} }_{1,2} \,{\left(\sigma_{33} +{\dot{\theta} }_{O,1} \,{\left(\sigma_{36} +L_3 \,\sigma_{35} \right)}+L_3 \,{\dot{\theta} }_{2,3} \,\sigma_{35} \right)}+{\dot{\theta} }_{O,1} \,{\left(\sigma_{33} +{\dot{\theta} }_{O,1} \,{\left(\sigma_{36} +L_1 \,\cos \left(\theta_{O,1} \right)+L_3 \,\sigma_{35} \right)}+L_3 \,{\dot{\theta} }_{2,3} \,\sigma_{35} \right)}+L_3 \,{\dot{\theta} }_{2,3} \,\sigma_{35} \,{\left({\dot{\theta} }_{1,2} +{\dot{\theta} }_{2,3} +{\dot{\theta} }_{O,1} \right)}\\\mathrm{}\\\;\;\sigma_{24} ={\ddot{y} }_{O,P,\textrm{inv}} +{\dot{\theta} }_{O,1} \,{\left({\dot{\theta} }_{O,1} \,{\left(\sigma_{38} +L_1 \,\sin \left(\theta_{O,1} \right)+L_3 \,\sigma_{37} \right)}+\sigma_{34} +L_3 \,{\dot{\theta} }_{2,3} \,\sigma_{37} \right)}+{\dot{\theta} }_{1,2} \,{\left(\sigma_{34} +{\dot{\theta} }_{O,1} \,{\left(\sigma_{38} +L_3 \,\sigma_{37} \right)}+L_3 \,{\dot{\theta} }_{2,3} \,\sigma_{37} \right)}+L_3 \,{\dot{\theta} }_{2,3} \,\sigma_{37} \,{\left({\dot{\theta} }_{1,2} +{\dot{\theta} }_{2,3} +{\dot{\theta} }_{O,1} \right)}\\\mathrm{}\\\;\;\sigma_{25} =L_1 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\,\sin \left(\theta_{O,1} \right)-L_1 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\,\cos \left(\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_{26} =L_3 \,\sigma_{35} \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)-L_3 \,\sigma_{37} \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_{27} =x_{3,\textrm{C3}} \,\sigma_{37} +\sigma_{38} +L_1 \,\sin \left(\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_{28} ={\dot{\theta} }_{1,2} \,{\left(x_{3,\textrm{C3}} \,\sigma_{37} +\sigma_{38} \right)}\\\mathrm{}\\\;\;\sigma_{29} =\sigma_{36} +L_1 \,\cos \left(\theta_{O,1} \right)+x_{3,\textrm{C3}} \,\sigma_{35} \\\mathrm{}\\\;\;\sigma_{30} ={\dot{\theta} }_{1,2} \,{\left(\sigma_{36} +x_{3,\textrm{C3}} \,\sigma_{35} \right)}\\\mathrm{}\\\;\;\sigma_{31} =L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\,\cos \left(\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_{32} =L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\,\sin \left(\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_{33} ={\dot{\theta} }_{1,2} \,{\left(\sigma_{36} +L_3 \,\sigma_{35} \right)}\\\mathrm{}\\\;\;\sigma_{34} ={\dot{\theta} }_{1,2} \,{\left(\sigma_{38} +L_3 \,\sigma_{37} \right)}\\\mathrm{}\\\;\;\sigma_{35} =\cos \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_{36} =L_2 \,\cos \left(\theta_{1,2} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_{37} =\sin \left(\theta_{1,2} +\theta_{2,3} +\theta_{O,1} \right)\\\mathrm{}\\\;\;\sigma_{38} =L_2 \,\sin \left(\theta_{1,2} +\theta_{O,1} \right)\end{array} $$ 
 

# Conclusiones

En el presente trabajo se desarrolló de manera integral el modelado matemático de un manipulador robótico tipo SCARA, abarcando la cinemática y dinámica del sistema. A lo largo del reporte se lograron formular los modelos cinemáticos directo e inverso de la postura, lo que permitió establecer la relación entre las variables articulares y la posición y orientación del efector final, cumpliendo así con el primer objetivo planteado.


Asimismo, se obtuvo el modelo cinemático de las velocidades mediante el uso del Jacobiano, herramienta fundamental para relacionar el espacio articular con el espacio cartesiano. Este análisis permitió comprender cómo las velocidades articulares influyen directamente en el movimiento del efector final, así como identificar condiciones críticas como las singularidades del robot, donde el manipulador pierde capacidad de movimiento en algunas direcciones. De igual forma, se desarrolló el modelo cinemático de las aceleraciones, incorporando la derivada temporal del Jacobiano, lo que permitió describir de manera más completa el comportamiento del robot, incluyendo efectos asociados al movimiento como lo es la aceleración centrípeta.


En cuanto al modelado dinámico, se logró formular tanto el modelo directo como el inverso mediante el formalismo de Euler\-Lagrange, obteniendo las ecuaciones de movimiento del sistema. Estas ecuaciones permiten determinar los torques necesarios para generar un movimiento específico, así como predecir la respuesta dinámica del manipulador ante diferentes condiciones de operación.


El uso de herramientas de cálculo simbólico en Matlab facilitó la obtención de expresiones generales, lo cual resulta de gran utilidad para la simulación y validación del modelo bajo distintas configuraciones del robot. Además, la documentación detallada del proceso permitió estructurar y comprender cada una de las etapas del modelado, reforzando los conocimientos adquiridos previamente en clase.


En conclusión, el desarrollo de este reporte, y examen, permitió no sólo cumplir con los objetivos planteados, sino también consolidar la comprensión del comportamiento cinemático y dinámico de manipuladores robóticos tipo SCARA. Este tipo de análisis constituye una base fundamental para aplicaciones futuras, y posiblemente más avanzadas ,en el desarrollo de soluciones en el ámbito de la robótica.

# Referencias

\[1\] J. J. Craig, *Introduction to Robotics: Mechanics and Control*, 3.ª ed. Upper Saddle River, NJ, EE. UU.: Pearson/Prentice Hall, 2005.

