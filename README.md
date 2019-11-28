Instrucciones Proyecto Final

Propósito:
Diseñar una estrategia de trading con base a la reacción del precio ante el comunicado de indicadores macroeconómicos (Análisis fundamental).

Pasos en general
1.	Utiliza el archivo de precios historicos 
2.	Utiliza el archivo de calendario economico
3.	Divide los datos de la siguiente manera: 
o	Periodo de prueba: 01/ene/2019
o	Periodo de entrenamiento: 01/mayo/2009 - 01/dic/2018

4.	Crea un data frame llamado df_escenarios, como el siguiente ejemplo
timestamp	escenario	direccion*	pip_alcistas	pips_bajistas	volatilidad
01/05/2009  09:30:00	A	1	55	25	86
01/12/2009 07:00	C	-1	100	80	50

*asigna el numero 1 cuando Close > Open y -1 cuando Close < Open
5.	Con los datos anteriores, utilizando los datos del periodo de entrenamiento, diseña una estrategia para colocar ordenes según el escenario, de tal manera que tengas un data frame de decisiones llamado df_decisiones como el siguiente ejemplo:
escenario	operacion	sl	tp	unidades
A	compra	20	40	1000
C	venta	40	80	2000

6.	Con el dataframe df_decisiones y el dataframe df_escearios, crea un nuevo dataframe llamado df_backtest, realiza el backtest de la estrategia en el periodo de prueba, es decir, una vez que tengas definida la operación que se colocará dependiendo el resultado del calendario económico, haz el cálculo de lo que habría pasado con cada operación de haberse colocado. Considera el sigiuente ejemplo:

timestamp	escenario	Direccion*	pip_alcistas	pips_bajistas	volatilidad	resultado	pips	capital
01/05/2009  09:30:00	A	1	55	25	86	ganada	40	$400
01/12/2009 07:00	C	-1	100	80	50	perdida	-40	-$200

7.	Con la columna de capital del dataframe df_backtest, grafica el resultado acumulado del capital en una gráfica de linea con plotly


Guia conceptual

Definir estrategia de trading
•	Ahora que se tiene definido que se clasifica la "reacción del precio” en escenarios según la regla:
o	A Actual >= Consensus >= Previous
o	B Actual >= Consensus < Previous
o	C Actual < Consensus >= Previous
o	D Actual < Consensus < Previous
•	También se tiene identificado en la historia la cantidad de ocurrencias de tales escenarios, por ejemplo: 

o	A: 15 veces
o	B: 0 Veces
o	C: 40 veces
o	D: 5 veces

•	Y para cada escenario, se tienen sus métricas:

o	(Dirección) Close (t_30) - Open(t_0)
o	(Pips Alcistas) High(t_0 : t_30) – Open(t_0)
o	(Pips Bajistas) Open(t_0) – Low(t_0 : t_30)
o	(Volatilidad) High(t_-30 : t_30) ,  - mínimo low (t_-30:t_30)

•	Realiza entonces un análisis del valor (Dirección) para cada escenario que sí se presentó, por ejemplo, si hubo un total de 15 veces que se presentó escenario A, 20 veces esceario C y 5 veces escenario D, para cada uno de esos cuenta cuantas veces la dirección fue positiva (subió el precio) y cuántas veces fue negativa (bajó el precio), así como el valor en pips que fue tal movimiento. 

•	Considera el supuesto de que cada que se comunica el indicador, invariablemete, colocas una operación de mercado para el instrumento Eur/Usd, con un TP y SL fijos. Estos últimos son los que deberás de encontrar una forma para determinarlos dentro de la estrategia de tal manera que logres un rendimiento positivo (cualquier cantidad por encima de 0 se considera como útil en este caso).

