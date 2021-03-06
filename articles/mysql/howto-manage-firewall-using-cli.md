---
title: "Creación y administración de reglas de firewall de Azure Database for MySQL mediante la CLI de Azure | Microsoft Docs"
description: "Este artículo describe cómo crear y administrar reglas de firewall de Azure Database for MySQL mediante la línea de comandos de la CLI de Azure."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 09/15/2017
ms.openlocfilehash: bc7763981c27a3d37cc1bd16c0f8efc0b4c01ce0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-by-using-the-azure-cli"></a>Creación y administración de reglas de firewall de Azure Database for MySQL mediante la CLI de Azure
Las reglas de firewall de nivel de servidor permiten a los administradores administrar el acceso a un servidor de Azure Database for MySQL desde una dirección IP o desde un intervalo de direcciones IP especificado. Con los comandos de la CLI de Azure adecuados, puede crear, actualizar, eliminar, enumerar y mostrar reglas de firewall para administrar el servidor. Para obtener información general sobre los firewalls de Azure Database for MySQL, consulte [Reglas de firewall de un servidor de Azure Database for MySQL](./concepts-firewall-rules.md).

## <a name="prerequisites"></a>Requisitos previos
* [Instale la CLI de Azure 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).
* Instale el SDK de Python para Azure para los servicios PostgreSQL y MySQL.
* Instale el componente de la CLI de Azure para los servicios PostgreSQL y MySQL.
* Cree de un servidor de Azure Database for MySQL.

## <a name="firewall-rule-commands"></a>Comandos de reglas de firewall:
El comando **az mysql server firewall-rule** se utiliza desde la CLI de Azure para crear, eliminar, enumerar, mostrar y actualizar reglas de firewall.

Comandos:
- **create**: crear una regla de firewall de servidor de Azure para MySQL.
- **delete**: eliminar una regla de firewall de servidor de Azure para MySQL.
- **list: enumerar las reglas de firewall de servidor de Azure para MySQL.
- **show: mostrar los detalles de una regla de firewall de servidor de Azure para MySQL.
- **update**: actualizar una regla de firewall de servidor de Azure para MySQL.

## <a name="log-in-to-azure-and-list-your-azure-database-for-mysql-servers"></a>Inicio de sesión en Azure y enumeración de los servidores de Azure Database for MySQL
Conéctese de forma segura a la CLI de Azure con su cuenta de Azure mediante el comando **az login**.

1. Desde la línea de comando, ejecute el siguiente comando:
```azurecli
az login
```
Este comando generará un código que se usará en el paso siguiente.

2. Use un explorador web para abrir la página [https://aka.ms/devicelogin](https://aka.ms/devicelogin) y escriba el código.

3. En el mensaje, inicie sesión con sus credenciales de Azure.

4. Después de que se autorice el inicio de sesión, se imprimirá una lista de suscripciones en la consola. Copie el identificador de la suscripción deseada para establecer la suscripción actual que se va a usar.
   ```azurecli-interactive
   az account set --subscription {your subscription id}
   ```

5. Si no está seguro de los nombres, muestre los servidores de Azure Database for MySQL para su suscripción y grupo de recursos.

   ```azurecli-interactive
   az mysql server list --resource-group myResourceGroup
   ```

   Anote el valor del atributo name de la lista, que se necesitará para especificar con qué servidor MySQL desea trabajar. Si es necesario, confirme los detalles de dicho servidor con el atributo name para confirmar que el nombre es correcto:

   ```azurecli-interactive
   az mysql server show --resource-group myResourceGroup --name mysqlserver4demo
   ```

## <a name="list-firewall-rules-on-azure-database-for-mysql-server"></a>Lista de reglas de firewall en el servidor de Azure Database for MySQL 
Utilizando el nombre del servidor y el nombre del grupo de recursos, enumere las reglas de firewall existentes en el servidor. Tenga en cuenta que el atributo de nombre de servidor se especifica en el modificador **--server** y no en el modificador **--name**.
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo
```
La salida enumera las reglas existentes, si existen, en formato JSON (de forma predeterminada). Puede usar el modificador **--output table** para obtener los resultados en un formato de la tabla más legible.
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo --output table
```
## <a name="create-a-firewall-rule-on-azure-database-for-mysql-server"></a>Creación de una regla de firewall en el servidor de Azure Database for MySQL
Con el nombre del servidor de Azure MySQL y el nombre del grupo de recursos, cree una nueva regla de firewall en el servidor. Dé un nombre a la regla y a las direcciones IP inicial y final (para proporcionar acceso a un intervalo de direcciones IP) para la regla.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```
Para permitir el acceso a una única dirección IP, proporcione la misma dirección IP como la dirección IP inicial y final, como en este ejemplo.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  
--server mysql --name "Firewall Rule with a Single Address" --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
```
Si se realiza correctamente, la salida del comando muestra los detalles de la regla de firewall que ha creado, en formato JSON (de forma predeterminada). Si se produce un error, la salida muestra el texto del mensaje de error.

## <a name="update-a-firewall-rule-on-azure-database-for-mysql-server"></a>Actualización de una regla de firewall en el servidor de Azure Database for MySQL 
Con el nombre del servidor de Azure MySQL y el nombre del grupo de recursos, actualice una regla de firewall existente en el servidor. Proporcione el nombre de una regla de firewall existente como entrada, así como los atributos de dirección IP inicial y final que se van a actualizar.
```azurecli-interactive
az mysql server firewall-rule update --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
Cuando se realiza correctamente, la salida del comando muestra los detalles de la regla de firewall que ha actualizado, de forma predeterminada en formato JSON. Si se produce un error, la salida muestra el texto del mensaje de error.

> [!NOTE]
> Si la regla de firewall no existe, el comando update la crea.

## <a name="show-firewall-rule-details-on-azure-database-for-mysql-server"></a>Presentación de los detalles de las reglas de firewall en el servidor de Azure Database for MySQL
Con el nombre del servidor de Azure MySQL y el nombre del grupo de recursos, muestre los detalles de la regla de firewall existente en el servidor. Proporcione como entrada el nombre de una regla de firewall existente.
```azurecli-interactive
az mysql server firewall-rule show --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
Cuando se realiza correctamente, la salida del comando muestra los detalles de la regla de firewall que ha especificado, en formato JSON (de forma predeterminada). Si se produce un error, la salida muestra el texto del mensaje de error.

## <a name="delete-a-firewall-rule-on-azure-database-for-mysql-server"></a>Eliminación de una regla de firewall en el servidor de Azure Database for MySQL
Con el nombre del servidor de Azure MySQL y el nombre del grupo de recursos, quite una regla de firewall existente del servidor. Proporcione el nombre de una regla de firewall existente.
```azurecli-interactive
az mysql server firewall-rule delete --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
Cuando se realiza correctamente, no hay ninguna salida. En caso de error, se muestra el texto del mensaje de error.

## <a name="next-steps"></a>Pasos siguientes
- Para más información, consulte [Reglas de firewall del servidor de Azure Database para MySQL](./concepts-firewall-rules.md).
- [Creación y administración de reglas de firewall de Azure Database for MySQL mediante Azure Portal](./howto-manage-firewall-using-portal.md).