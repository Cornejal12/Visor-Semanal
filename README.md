# Visor-Semanal
Es una solución en Power BI que permite proyectar los días restantes de la semana según la fecha seleccionada. 
Utiliza segmentación única por fecha para visualizar de manera dinámica los próximos días hábiles (Lunes a Domingo), optimizando el análisis y la planificación.
 
![Captura de pantalla 2025-03-09 230738](https://github.com/user-attachments/assets/ed0644ca-5fe9-458b-b887-f75b814f5193)

## Objetos Visuales ocupados 
- Segmentación de datos
- New card
- Cuadro de texto

## Modelo Relacional 
![Captura de pantalla 2025-03-09 230958](https://github.com/user-attachments/assets/57fac74c-877c-41df-99e9-77e638f65082)

### Dias Semana (Tabla dimensión)
![Captura de pantalla 2025-03-09 231314](https://github.com/user-attachments/assets/ee182162-adfc-4bb2-8961-0e3630400046)

Es importante crear esta tabla ya que permite que por medio de codigo dax se mapee los dias de la semana para retornar la fecha asi como las cantidades. 
No se puede ocupar una columna calculada que retorne los dias de la semana del calendario, ya que solo filtra el dia seleccionado. 
Los valores del campo "Semana" se plasmaran en los multiplos pequeños de la "New card". 
El campo "orden" permite ordenar los valores del campo "semana".

### Calendario (Tabla dimensión)
![Captura de pantalla 2025-03-09 231305](https://github.com/user-attachments/assets/9827cd75-8a37-4442-b23a-c2a2465e221e)

Crear una tabla calendario para concentrar las fechas y permitir el análisis en fechas sin datos.

### Sheet1 (Tabla hechos)
![Captura de pantalla 2025-03-09 235450](https://github.com/user-attachments/assets/1f9e8632-8797-4a1e-85cd-5f6440fa9d81)

Informacion sobre los pedidos programados y cantidades a entregar.

## DAX
Medida que muestra el periodo de fecha a proyectar.
```
Periodo_Fecha = 
var _Fecha_Inicio = SELECTEDVALUE(Calendario[Date])
var _Fecha_Termino = _Fecha_Inicio + 6
RETURN
_Fecha_Inicio&" al "&_Fecha_Termino
```
Medida que mapea los valores del campo "Semana" de la tabla "Dias_semana" para retornar la fecha correspondiente a la semana.
```
Imprimir_Fecha = 
var _Fecha_Inicio = SELECTEDVALUE(Calendario[Date]) --Selecciona el dia
var _Fecha_Termino = _Fecha_Inicio + 6 --_Fecha_Inicio = 1 , se suma 6 para completar los 7 dias de la semana a proyectar
--+6 se puede reemplezar por un parametro para el usuario ingrese los dias a proyectar.
var _Lunes = CALCULATE(SELECTEDVALUE(Calendario[Date]), Calendario[Semana]="lunes", Calendario[Date] >= _Fecha_Inicio, Calendario[Date]<= _Fecha_Termino )
var _Martes = CALCULATE(SELECTEDVALUE(Calendario[Date]), Calendario[Semana]="martes", Calendario[Date] >= _Fecha_Inicio, Calendario[Date]<= _Fecha_Termino )
var _Miercoles= CALCULATE(SELECTEDVALUE(Calendario[Date]), Calendario[Semana]="miércoles", Calendario[Date] >= _Fecha_Inicio, Calendario[Date]<= _Fecha_Termino )
var _Jueves = CALCULATE(SELECTEDVALUE(Calendario[Date]), Calendario[Semana]="jueves", Calendario[Date] >= _Fecha_Inicio, Calendario[Date]<= _Fecha_Termino )
var _Viernes = CALCULATE(SELECTEDVALUE(Calendario[Date]), Calendario[Semana]="viernes", Calendario[Date] >= _Fecha_Inicio, Calendario[Date]<= _Fecha_Termino )
var _Sabado = CALCULATE(SELECTEDVALUE(Calendario[Date]), Calendario[Semana]="sábado", Calendario[Date] >= _Fecha_Inicio, Calendario[Date]<= _Fecha_Termino )
var _Domingo = CALCULATE(SELECTEDVALUE(Calendario[Date]), Calendario[Semana]="domingo", Calendario[Date] >= _Fecha_Inicio, Calendario[Date]<= _Fecha_Termino )
RETURN
SWITCH(
    SELECTEDVALUE(Dias_Semana[Semana]),
    "Lunes", IF(ISBLANK(_Lunes), "--", _Lunes),
    "Martes", IF(ISBLANK(_Martes),"--", _Martes),
    "Miércoles", IF(ISBLANK(_Miercoles), "--", _Miercoles),
    "Jueves", IF(ISBLANK(_Jueves), "--", _Jueves),
    "Viernes", IF(ISBLANK(_Viernes), "--", _Viernes),
    "Sábado", IF(ISBLANK(_Sabado), "--", _Sabado),
    "Domingo", IF(ISBLANK(_Domingo), "--", _Domingo)
)
```

Medida que mapea los valores del campo "Semana" de la tabla "Dias_semana" para retornar las cantidades correspondientes al dia de la semana.
```
Cantidad_Articulos = 
var _Fecha_Inicio = SELECTEDVALUE(Calendario[Date]) --Selecciona el dia
var _Fecha_Termino = _Fecha_Inicio + 6 --_Fecha_Inicio = 1 , se suma 6 para completar los 7 dias de la semana a proyectar
--+6 se puede reemplezar por un parametro para el usuario ingrese los dias a proyectar.
var _Lunes = CALCULATE(SUM(Sheet1[Cantidad]), Calendario[Semana]="lunes", Calendario[Date] >= _Fecha_Inicio, Calendario[Date]<= _Fecha_Termino )
var _Martes = CALCULATE(SUM(Sheet1[Cantidad]), Calendario[Semana]="martes", Calendario[Date] >= _Fecha_Inicio, Calendario[Date]<= _Fecha_Termino )
var _Miercoles= CALCULATE(SUM(Sheet1[Cantidad]), Calendario[Semana]="miércoles", Calendario[Date] >= _Fecha_Inicio, Calendario[Date]<= _Fecha_Termino )
var _Jueves = CALCULATE(SUM(Sheet1[Cantidad]), Calendario[Semana]="jueves", Calendario[Date] >= _Fecha_Inicio, Calendario[Date]<= _Fecha_Termino )
var _Viernes = CALCULATE(SUM(Sheet1[Cantidad]), Calendario[Semana]="viernes", Calendario[Date] >= _Fecha_Inicio, Calendario[Date]<= _Fecha_Termino )
var _Sabado = CALCULATE(SUM(Sheet1[Cantidad]), Calendario[Semana]="sábado", Calendario[Date] >= _Fecha_Inicio, Calendario[Date]<= _Fecha_Termino )
var _Domingo = CALCULATE(SUM(Sheet1[Cantidad]), Calendario[Semana]="domingo", Calendario[Date] >= _Fecha_Inicio, Calendario[Date]<= _Fecha_Termino )
var _Cantidad_Total = _Lunes+_Martes+_Miercoles+_Jueves+_Viernes+_Sabado+_Domingo
RETURN
SWITCH(
    SELECTEDVALUE(Dias_Semana[Semana]),
    "Lunes", _Lunes,
    "Martes", _Martes,
    "Miércoles", _Miercoles,
    "Jueves", _Jueves,
    "Viernes", _Viernes,
    "Sábado", _Sabado,
    "Domingo", _Domingo,
    "Total", _Cantidad_Total
)
```
## Objeto card new 
Se puede cambiar el orden de las medidas.
![image](https://github.com/user-attachments/assets/5732f8e0-00ef-492c-8a89-d95668b89f1a)

## Estilos Aplicados
### Diseño de Múltiplos pequeños 
![Captura de pantalla 2025-03-10 000808](https://github.com/user-attachments/assets/0dbceade-5a10-4ca8-a1f1-b0114f2d03b4)
![image](https://github.com/user-attachments/assets/d4cb51bd-a8b2-4182-b9ef-6ab978eca95d)

### Encabezado de Múltiplos pequeños 
![image](https://github.com/user-attachments/assets/728bcab6-1627-4328-b638-cbf4214bb044)
![image](https://github.com/user-attachments/assets/a2ce430b-2136-4221-9bd1-047d2120ccc5)

### Presentación
![image](https://github.com/user-attachments/assets/a53e860b-0473-4b2e-b702-3b7f52675773)
![image](https://github.com/user-attachments/assets/96ef1861-02e6-4368-9924-4cfb914d9986)

### Valores de llamada
#### Fechas
![image](https://github.com/user-attachments/assets/7d958ed1-b2f3-46a8-b652-7bc130894ce0)
![image](https://github.com/user-attachments/assets/c8456b82-63b9-43ff-a40f-764ed802063a)

#### Cantidad_Articulos
![image](https://github.com/user-attachments/assets/8e8f1f72-062d-4031-9c32-0fd92d9e70a0)
![image](https://github.com/user-attachments/assets/0c7c2673-05be-4413-9fbb-95e46d4e28ee)
![image](https://github.com/user-attachments/assets/1ab9ba2e-bfb1-4219-8d62-0b03f4dd509f)

### Tarjetas
"Aplicar configuración a" 
Serie 
- Fechas
- Cantidad_Articulos

![Captura de pantalla 2025-03-10 000728](https://github.com/user-attachments/assets/cf443f7c-7708-4414-ad7c-57b61bb0c785)
