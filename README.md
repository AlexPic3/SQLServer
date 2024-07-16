# SQLServer
Trining in SQL Server and in relational databases La Violeta 2024

```SQL
CREATE PROCEDURE TransferirDinero3
	@idOrigen int, @idDestino int, @importe money
AS
BEGIN
	SET NOCOUNT ON;

	-- empezamos la transacción, para asegurarnos que se ejeucta bien todo, o nada.
	BEGIN TRAN

	BEGIN TRY
		-- condiciones para que la TF se pueda hacer
		IF(@importe>0 AND (select saldo from cuenta 
							WHERE idCuenta = @idOrigen) > @importe)
		BEGIN
			-- sacamos de la cuenta origen
			UPDATE Cuenta
				SET Saldo = saldo - @importe
			WHERE idCuenta = @idOrigen
			print 'Dinero sacado de la cuenta de ORIG'


			-- esto provocará un error, ya que el campo es NOT NULL
			/*UPDATE Cuenta
				SET NumCuenta = NULL
			WHERE idCuenta = @idOrigen
			*/

			-- ingresamos en la cuenta de destino
			UPDATE Cuenta
				SET Saldo = saldo + @importe
			WHERE idCuenta = @idDestino
			print 'Transferencia completa!'

			-- validamos la transacción
			COMMIT TRAN
		END
		ELSE
		BEGIN
			print 'No se ha podido hacer la transferencia...'

			-- no se ha hecho nada, pero tenemos que 'cerrar' la transacción
			ROLLBACK TRAN
		END
	END TRY
	BEGIN CATCH
		print 'HA HABIDO UN ERROR'
		print 'No se habrá realizado ningun cambio'

		-- anulamos lo hecho hasta ahora
		ROLLBACK TRAN
	END CATCH
END
```


![alt text for screen readers](/path/to/image.png "Text to show on mouseover").

Example:

![ER TEST](\La meva unitat\Cifo Violeta Cursos DC\CursSQL\SQL PNG Diagrames ER\image.png "TEST ER")
