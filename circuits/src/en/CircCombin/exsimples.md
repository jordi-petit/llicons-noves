<!-- Posar aquesta imatge al començament de cada lliçó -->
<img src="../../logos/illustracio1.png" alt="Digital circuits" style="float: left; border-radius: 8px; height: 120px;"/>
<img src="../../logos/LogoCatedraCHIPBlanc.jpg" alt="CHIP Chair Logo" style="float: right; border-radius: 8px; height: 120px;"/>
<div style="clear: both;"></div>
<br>



# Simple exercises
In this topic you will see how Boolean algebra, truth tables and logic gates relate in a series of simple exercises.


## Example: Employee access control system
In this example we aim to design a logic circuit that determines whether an employee is permitted access to a restricted area of the data centre.


There are three key factors that determine the decision:


* **Valid access card ($T$)**: The employee must present the card to the reader and it must be recognised as valid.
* **Recognised fingerprint ($H$)**: The employee must scan their fingerprint and it must match the registered one.
* **Security clearance ($S$)**: The employee must hold an active security clearance for this area.


The rules for granting access are:


* **$R_1$** - The card is valid and the fingerprint is recognised.
* **$R_2$** - The security clearance is active and the fingerprint is recognised.
* **$R_3$** - The security clearance is not active and the card is valid.


We need to design a circuit with three inputs ($T$, $H$, $S$) and one output ($A$). The output $A$ determines whether access is granted, taking the value 1 for granted access and 0 for denied access.


First, we construct the truth table of the application:


|**$T$**|**$H$**|**$S$**|**$R_1$**|**$R_2$**|**$R_3$**|**$A$**|
|------|------|------|------|------|------|------|
|0|0|0|0|0|0|0|
|0|0|1|0|0|0|0|
|0|1|0|0|1|0|0|
|0|1|1|0|1|0|1|
|1|0|0|0|0|1|1|
|1|0|1|0|0|0|0|
|1|1|0|1|0|1|1|
|1|1|1|1|1|0|1|


Next we express the Boolean function of the operation:


$$
A = R_1 + R_2 + R_3 = (TH) + (SH) + (\overline{S}T)
$$


Next we draw the digital circuit:


<img src="../../CircCombin/exsimplesexemple.png" alt="example" style="display:block; height:200px; margin:0 auto; border-radius:8px; background-color:white; padding:4px;"/>
<center><i>Digital circuit for employee access control</i></center>


## Exercises on Jutge.org: [Introduction to Digital Circuit Design](https://jutge.org/courses/JordiCortadella:IntroCircuits)

- [The Embassy Ball](https://jutge.org/problems/X64345_en)
- [Heating-cooling system](https://jutge.org/problems/X04334_en)
- [Energy-efficient heating-cooling system](https://jutge.org/problems/X89493_en)
- [Robot with light sensors](https://jutge.org/problems/X74930_en)


<small>*Remember that to access the exercises and for the Jutge to assess your solutions you must be enrolled in the course. You will find all the instructions here.*</small>


<!-- This image should go at the end of each lesson, either with this line or within the signature. Leave commented if it is already in the signature-->
<br><br><img src="../../logos/TotsLogosBlanc.png" alt="CHIP Chair Logos" width="100%" style="display:block; margin:0 auto; border-radius:8px;"/>
<Autors autors="xcasas fmadrid"/>
