# Visor-Semanal
Es una solución en Power BI que permite proyectar los días restantes de la semana según la fecha seleccionada. 
Utiliza segmentación única por fecha para visualizar de manera dinámica los próximos días hábiles (Lunes a Domingo), optimizando el análisis y la planificación.

Demostración 
![Captura de pantalla 2025-03-09 230738](https://github.com/user-attachments/assets/ed0644ca-5fe9-458b-b887-f75b814f5193)

## Objetos Visuales ocupados 
- Segmentación de datos
- New card
- Cuadro de texto

## Modelo Relacional 
![Captura de pantalla 2025-03-09 230958](https://github.com/user-attachments/assets/57fac74c-877c-41df-99e9-77e638f65082)

### Dias Semana (Tabla dimensión)
![Captura de pantalla 2025-03-09 231314](https://github.com/user-attachments/assets/ee182162-adfc-4bb2-8961-0e3630400046)
Es importante crear esta tabla ya que permite que por medio de codigo dax se mapee los dias que tiene que retornar el valor. 
No se puede ocupar una columna calculada que retorne los dias de la semana del calendario, ya que solo filtra el dia seleccionado. 
Los valores del campo "Semana" se plasmaran en los multiplos pequeños de la "New card". 
El campo "orden" permite ordenar los valores del campo "semana".

### Calendario (Tabla dimensión)
![Captura de pantalla 2025-03-09 231305](https://github.com/user-attachments/assets/9827cd75-8a37-4442-b23a-c2a2465e221e)
Crear una tabla calendario para concentrar las fechas y permitir el análisis en fechas sin datos.

### Sheet1 (Tabla hechos)
![Captura de pantalla 2025-03-09 231250](https://github.com/user-attachments/assets/bc421237-3f50-471a-8b29-11c82f77251c)
Informacion sobre los pedidos programados y cantidades a entregar.


