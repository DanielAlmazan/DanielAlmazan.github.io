# Resumen Tests

En este proyecto se han realizado una serie de 
[tests](https://github.com/DanielAlmazan/hotel-nest/blob/main/test/axios/axiosTests.mjs).
Para ello, se ha utilizado axios.

1. Obtener limpiezas de una habitación (getRoomCleanings):
   - Verifica que se puedan obtener las limpiezas de una habitación específica y que la respuesta tenga el estado HTTP correcto (200) y una cantidad mínima de registros.

2. Verificar si una habitación está limpia (isRoomClean):
   - Comprueba si una habitación específica está limpia, esperando un estado HTTP 200 y un valor booleano ok que indique la limpieza.

3. Obtener la lista de habitaciones limpiadas hoy (getCleanedRooms):
   - Verifica que se pueda obtener una lista de habitaciones limpiadas hoy, esperando un estado HTTP 200 y al menos un registro.

4. Login incorrecto (incorrectLogin):
   - Intenta realizar un login con credenciales incorrectas y espera recibir un error 401.

5. Login correcto (correctLogin):
   - Realiza un login con credenciales correctas y espera un estado HTTP 201.

6. Insertar limpieza sin login (insertCleaningNoLogin):
   - Intenta insertar una limpieza sin haber iniciado sesión y espera un error 401.

7. Insertar limpieza con login correcto (insertCleaning):
   - Inserta una limpieza después de haber iniciado sesión correctamente y espera un estado HTTP 201.

8. Actualizar limpieza sin login (updateCleaningNoLogin):
   - Intenta actualizar una limpieza sin haber iniciado sesión y espera un error 401.

9. Actualizar limpieza con login correcto (updateCleaning):
   - Actualiza una limpieza después de haber iniciado sesión correctamente y espera un estado HTTP 200.

10. Ejecutar todos los tests (executeTests):
    - Ejecuta todos los tests anteriores en secuencia y cuenta cuántos tests fueron exitosos y cuántos fallaron, mostrando el resultado final.

```Bash
node test/axios/axiosTests.
```
```Ini
OK - Inserción de limpieza sin login
OK - Actualización de limpieza sin login
OK - Login incorrecto
OK - Login correcto
OK - Inserción de limpieza con login
OK - Actualización de limpieza con login
OK - Habitaciones limpiadas hoy
OK - Limpiezas de habitación
OK - Habitación limpia
OK - Habitación sucia
Tests finalizados
Tests exitosos: 10
Tests fallidos: 0
```

Para más detalles, visitar el [Hotel Nest](https://github.com/DanielAlmazan/hotel-nest/tree/main) en GitHub.
