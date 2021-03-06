=======
Magento
=======

.. inheritref:: magento/magento:section:pedidos

Pedidos
=======

La importación de pedidos de Magento a Tryton se puede hacer de dos formas:

* **Importación manual**: A |menu_sale_shop| dispone del botón **Importar
  pedidos**.
* **Importación automática**: En la configuración de la tienda active el
  campo **Planificador de tareas**. Importa los pedidos según el intervalo de
  ejecución del cron (cada 30 minutos, 20 minutos,...)

.. inheritref:: magento/magento:section:importar_pedidos

Importar pedidos
================

En el menú |menu_sale_shop| dispone del botón **Importar pedidos**. Es importante
que los usuarios que van a importar pedidos tengan a sus preferencias las tiendas
que se permiten vender (accede a Administración/Usuarios para gestionar los usuarios
y las tiendas que podrán gestionar).

La importación de pedidos se realiza según intervalo de fechas:

Un ejemplo de fecha "04/10/2010 00:00:00" se importarán los pedidos del día 4
de octubre a partir de las 00 de la noche hasta el dia y hora que estamos
ejecutando la acción en el caso que la fecha final esté vacía.

Si especifica una fecha final, por ejemplo "04/10/2010 10:00:00", se importarán
los pedidos como antes, pero hasta las 10 de la mañana del mismo dia.

Si a Magento dispone de muchos pedidos de venta a partir de una fecha, una buena
opción es ir importando los pedidos en bloques y evitar la importación en masa.

El tiempo de importación de pedidos vendrá decidido según la cantidad de pedidos
a procesar.

.. note:: Si no gestiona los productos en el ERP, al recibir un pedido de venta
          se buscará un producto por el código. Si el producto existe, se usará
          este producto. Si el producto no existe, se creará un nuevo producto.
          Los datos del producto a crear son los valores del producto que disponga
          a Magento. En el caso de los impuestos, se buscará el impuesto que tenga
          añadido a Magento y buscará el equivalente al ERP según el país por defecto
          definido en la tienda.

Si un pedido de venta ya se ha importado, este pedido de venta no se volverá a crear.
Si por cualquier motivo desea volver a importar el pedido de venta, puede eliminar el
pedido de venta del ERP y volver a importar por el rango de fechas.
Como que el pedido no se encontrará por número de referencia y por tienda, se volverá
a crear.

La importación de pedidos de venta creará juntamente con el pedido de venta el tercero
y las direcciones. Si el tercero o la dirección ya existen estas no se volverán a crear.

Creación del tercero
--------------------

Antes de crear un tercero se buscará un existente con una de estas condiciones:

* Código país + Número NIF/CIF
* Número NIF/CIF
* Correo electrónico (pestaña eSale)
* Correo electrónico por defecto del tercero

La información del tercero nunca se modificará a partir de nuevos datos del Magento.

Reglas de impuestos
-------------------

En el caso que disponga de reglas de impuestos (para Melilla, Islas Canarias o un país
que no sea España), deberá crear un listado que relacione las reglas de impuestos según
subdivisiones o códigos postales que equivale por cada regla de impuesto especial. La configuración
de esta rejilla la podrá crear en el apartado de la configuración de la tienda.

Si el pedido de venta dispone en la dirección de facturación una región, se buscará
la subdivisión en la configuración de "Regla de impuestos" y los reglas
de impuesto equivalentes. En el caso que no se disponga de región, se buscará el
rango por código postal (inicio y final del código postal) si se dispone de una regla
de impuesto en este caso (los códigos postales deben ser numéricos).

En el caso que no disponga ni de región o código postal numérico, se usará la primera
regla de impuesto que se disponga por el país.

En el caso que la dirección de facturación de Magento del tercero encuentre uno de estos casos
esmentados, cuando se crea el tercero se le asignará una regla de impuestos en el tercero
y en el pedido de venta se le crearán los impuestos correspondientes a la regla de impuesto.

Creación de la dirección
------------------------

Antes de crear una dirección del tercero se buscará una existente con las condiciones:

* Tercero
* Calle
* Código postal

Las direcciones se crean con carácteres alfanuméricos (az09) (eliminando accentos y
carácteres que las API's de transporte se debe evitar).

Si la dirección creada desde Magento desea modificarla, recuerda también de modificar
la dirección en el cliente a Magento para que si el cliente la vuelve a usar no
se vuelva a crear una de nueva.

La información de la dirección nunca se modificará a partir de nuevos datos del Magento.
Si la dirección cambia de datos, se creará una nueva dirección con los nuevos datos.

Líneas
------

Cuando importe un pedido de venta se crearán las líneas del pedido. Es importante que
los productos de Magento esten creados también al ERP con el mismo código o SKU.
Si el producto no está creado al ERP (no se encuentra), se creará un nuevo producto.

El precio siempre es el que proviene de Magento y no se calculará un nuevo precio
cuando se genere el pedido de venta.

.. inheritref:: magento/magento:section:exportar_estado

Exportar estado
===============

En el menú |menu_sale_shop| dispone del botón de **Exportar estados** el cual
sincroniza los estados de Magento con los del ERP (complete, canceled,
processing,...) de los pedidos a partir de la fecha especificada (fecha de
modificación del pedido).

.. |menu_sale_shop| tryref:: sale_shop.menu_sale_shop/complete_name

.. inheritref:: magento/magento:section:configuracion_app

Configuración APP
=================

La configuración inicial es técnica y se efectuará en el momento de dar de alta
un servidor Magento en el ERP. Para configurar el servidor de Magento acceda a
|menu_magento_app|.

.. |menu_magento_app| tryref:: magento.menu_magento_app_form/complete_name

* Nombre

  * Nombre informativo del servidor de Magento

* General

  * Store View por defecto (disponible después de importar Magento Store)
  * Grupo de clientes por defecto (disponible después de importar grupo de
    clientes)

* Autenticación

  * URI del servidor Magento (con / al final).
  * Usuario webservices de Magento.
  * Password webservices de Magento.

* Importar

  * Importar Magento Store: Importa toda la estructura de las tiendas de
    Magento (website/store/view) y genera una tienda Magento en |menu_sale_shop|.
  * Importar grupo de clientes: Importa todos los grupos de clientes de Magento.

* Países

  * Países: Países que queremos importar regiones de Magento para los pedidos
    de venta.
  * Regiones: Asocia las regiones de Magento con las subdivisiones de Tryton.

* Tiendas

  * Información de nuestro Magento APP con la estructura de website/store/view

.. figure:: images/tryton-magento.png

.. note:: Recuerde que deberá instalar el módulo que amplia los webservices de
          Magento. Dispone del botón **Test conexión** para testear si los
          datos introducidos son correctos.

.. inheritref:: magento/magento:section:configuracion_tienda

Configuración de la tienda
==========================

A |menu_sale_shop| configure los valores de la tienda Magento. Fíjese que en
las tiendas Magento, el campo **APP tienda** marcará que es una tienda Magento.

En la configuración de la tienda esale, dispone de una pestaña más referente a
la configuración de la tienda Prestashop. De todos modos, revise la configuración
de todos los campos relacionados con la tienda.

* **Referencia Magento:** Usar el número de pedido de Magento
* **Precio global:** Para los multiestores, si se usa precio global o no (sólo
  para actualizaciones de precio)
* **Estados importación:** A partir del estado del pedido a Magento, podemos
  activar el pedido a Tryton si se confirma o se cancela.
* **Exportar estados:** Según el estado de Tryton, marcar el estado a Magento
  y/o notificar al cliente.
* **Métodos de pago:** Relaciona los pagos de Magento con los pagos de Tryton
* **Categoría:** Categoría por defecto. **Importante** que esta categoría tenga una
  cuenta a pagar y una cuenta a cobrar marcada.

.. figure:: images/tryton-magento-tienda-conf.png
