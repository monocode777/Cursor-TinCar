# Cursor-TinCar

Tenemos una tabla de Parqueaderos y queremos usar un cursor para recorrer todos los parqueaderos que estén disponibles e imprimir su información.

![image](https://github.com/user-attachments/assets/3d60849f-3837-49af-b616-5d85f62c21b7)


# Cursor

              DELIMITER //
              
              CREATE PROCEDURE ListarParqueaderosDisponibles()
              BEGIN
                  -- Declaracion variables 
                  DECLARE estado INT DEFAULT FALSE;
                  DECLARE p_id INT;
                  DECLARE p_direccion VARCHAR(255);
                  
                  -- Declaracion de cursor para seleccionar parqueaderos disponibles
                  DECLARE parqueadero_cursor CURSOR FOR
                      SELECT id_parqueadero, direccion
                      FROM Parqueaderos
                      WHERE estaDisponible = TRUE;
              
                  -- Manejo del final del cursor
                  DECLARE CONTINUE HANDLER FOR NOT FOUND SET estado = TRUE;
              
                  -- Abrir el cursor
                  OPEN parqueadero_cursor;
              
                  -- Bucle para recorrer las filas obtenidas por el cursor
                  read_loop: LOOP
                      -- Obtener los valores de la fila actual
                      FETCH parqueadero_cursor INTO p_id, p_direccion;
                      
                      -- Si no hay más filas, salir del bucle
                      IF estado THEN
                          LEAVE read_loop;
                      END IF;
              
                      -- Imprimir la información del parqueadero
                      SELECT CONCAT('Parqueadero ID: ', p_id, ' - Dirección: ', p_direccion) AS InfoParqueadero;
                  END LOOP;
              
                  -- Cerrar el cursor
                  CLOSE parqueadero_cursor;
              END //
              
              DELIMITER ;

CALL ListarParqueaderosDisponibles();




