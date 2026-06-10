<!-- Colocar esta imagen al inicio de cada lección -->
<img src="../../logos/illustracio1.png" alt="Digital circuits" style="float: left; border-radius: 8px; height: 120px;"/>
<img src="../../logos/LogoCatedraCHIPBlanc.jpg" alt="CHIP Chair Logo" style="float: right; border-radius: 8px; height: 120px;"/>
<div style="clear: both;"></div>
<br>



## Ejercicios simples
En este tema verás cómo se relacionan el álgebra de Boole, las tablas de verdad y las puertas lógicas en una serie de ejercicios simples.

## Ejemplo: Sistema de control de acceso para trabajadores
En este ejemplo nos proponemos diseñar un circuito lógico que determine si a un empleado se le permite el acceso a una zona restringida del centro de datos.

Hay tres factores clave que determinan la decisión:

* **Tarjeta de acceso válida ($T$)**: El empleado ha de pasar la tarjeta por el lector y que esta sea reconocida como válida.
* **Huella dactilar reconocida ($H$)**: El empleado ha de escanear su huella dactilar y que coincida con la registrada.
* **Autorización de seguridad ($S$)**: El empleado ha de tener una autorización de seguridad activa para esta zona.

Las reglas para conceder el acceso son:

* **R1** - La tarjeta es válida y la huella dactilar es reconocida.
* **R2** - La autorización de seguridad está activa y la huella dactilar es reconocida.
* **R3** - La autorización de seguridad no está activa y la tarjeta es válida.

Necesitamos diseñar un circuito con tres entradas ($T$, $H$, $S$) y una salida ($A$). La salida $A$ determina si se concede el acceso, tomando el valor $1$ para acceso concedido y $0$ para acceso denegado.

En primer lugar, construimos la tabla de verdad de la aplicación:

|**$T$**|**$H$**|**$S$**|**$R1$**|**$R2$**|**$R3$**|**$A$**|
|------|------|------|------|------|------|------|
|0|0|0|0|0|0|0|
|0|0|1|0|0|0|0|
|0|1|0|0|1|0|0|
|0|1|1|0|1|0|1|
|1|0|0|0|0|1|1|
|1|0|1|0|0|0|0|
|1|1|0|1|0|1|1|
|1|1|1|1|1|0|1|

Seguidamente expresamos la función booleana de la operación:

$$
A = R_1 + R_2 + R_3 = (TH) + (SH) + (\overline{S}T)
$$

A continuación dibujamos el circuito digital:

<img src="../../CircCombin/exsimplesexemple.png" alt="ejemplo" style="display:block; height:200px; margin:0 auto; border-radius:8px; background-color:white; padding:4px;"/>
<center><i>Circuito digital de control de acceso para trabajadores</i></center>

## Ejercicios en Jutge.org:[Introduction to Digital Circuit Design](https://jutge.org/courses/JordiCortadella:IntroCircuits)

- [El baile de la embajada](https://jutge.org/problems/X64345_en)
- [Sistema de calefacción y enfriamiento](https://jutge.org/problems/X04334_en)
- [Sistema de calefacción y enfriamiento energéticamente eficiente](https://jutge.org/problems/X89493_en)
- [Robot con sensores de luz](https://jutge.org/problems/X74930_en)


<small>*Recuerda que para acceder a los ejercicios y para que el Jutge valore tus soluciones debes estar inscrito en el [curso](https://jutge.org/courses/JordiCortadella:IntroCircuits). Encontrarás todas las instrucciones [aquí](../Inici/instrucciones.md).* </small>


<!-- Esta imagen debe ir al final de cada lección, ya sea con esta línea o dentro de la firma. Dejar comentado si ya está a la firma-->
<br><br><img src="../../logos/TotsLogosBlanc.png" alt="CHIP Chair Logos" width="100%" style="display:block; margin:0 auto; border-radius:8px;"/>
<Autors autors="xcasas fmadrid"/>
