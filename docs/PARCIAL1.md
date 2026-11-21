# Verificación del primer parcial

El recorrido usa Sicomoro como ejemplo de **KhypuPyme 2.0**, un sistema de control operativo para micro y pequeños negocios bolivianos. La misma aplicación comparte Caja, ventas, historial y comprobantes, y adapta la captura a cada rubro. El estudio [Operación por rubro](OPERACION-POR-RUBRO.md) describe otros flujos y preguntas para probarlos con comerciantes.

## Resultado del 20 de noviembre de 2026

Microsoft Edge ejecutó la web compilada en Windows con la API .NET 10 y PostgreSQL 18 en **khypupyme2_test**. Se usó una cuenta desechable verificada con un buzón local. La compra y la venta se confirmaron desde el navegador.

| Criterio y vínculo | Verificación | Resultado observado |
|---|---|---|
| CA-01 · RF-02 y RF-04 | Abrir Caja con Bs 1.000. Comprar 10 tablas de 2″ × 6″ × 10′ a Bs 5 por pie tablar. Vender dos a Bs 10 por pie tablar y recargar. | Compra Bs 500. Una venta por Bs 200 en historial y comprobante no fiscal **VEN-2026-000003**. Ocho piezas disponibles y Bs 700 en Caja. |
| CA-02 · RF-04 | Pedir nueve de la misma medida, abrir Revisar y comprobar el estado de Confirmar. Enviar la operación desde el comprobador y comparar los registros antes y después. | La pantalla mostró **Pedido 9 · Disponible 8 · Faltan 1** y mantuvo deshabilitada la confirmación. La API devolvió HTTP 409. Quedaron ocho piezas, Bs 700, una venta y los mismos dos movimientos de inventario. |

La compilación .NET en Release terminó con cero advertencias y cero errores. ESLint y TypeScript finalizaron correctamente. Vite compiló y emitió un aviso de tamaño para los módulos de PDF y fuentes. Ambos criterios se ejecutaron con **Test-Parcial1.ps1** y el PDF se verificó por número, importe y aviso no fiscal. La guía para repetir la comprobación está en el [README](../README.md#primer-parcial-de-práctica-interna).

La venta pasa de **TradePage** al endpoint de comandos, que comprueba el contexto del dueño y delega en **TradeOperations**. **CommerceService** coordina la transacción. **StockReads** atiende la consulta previa. El historial y el PDF se basan en el documento ya confirmado.

Esta comprobación cubre el recorrido de maderas en el entorno local. La eficiencia en comercios reales y la aceptación de los otros rubros se evaluarán con comerciantes, tomando tiempos y registrando diferencias de stock.

## Comprobaciones de rubros adicionales

En la misma base de prueba se ejecutaron los recorridos API **Test-Block7a.ps1** y **Test-Block7d.ps1**. En abarrotes, dos cajas de 24 menos cinco unidades dejaron 43 vendibles. La salida tomó el lote vendible de vencimiento más próximo y excluyó el lote vencido. En peluquería, se rechazaron las citas que se cruzaban y el segundo cobro de una cita ya cobrada. Los bloques también completaron los casos de bebidas, cosméticos, mascotas, limpieza, servicios profesionales, taller y artesanía.

Las comprobaciones dirigidas usan datos ficticios. Los resultados de estos dos bloques corresponden a la API. El recorrido de Sicomoro se ejecutó en Edge y puede regenerar el PDF y las capturas de evidencia en **artifacts/parcial1**.
