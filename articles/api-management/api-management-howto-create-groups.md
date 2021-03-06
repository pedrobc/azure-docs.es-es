---
title: "Administración de cuentas de desarrollador mediante grupos en Azure API Management | Microsoft Docs"
description: "Obtenga información acerca de cómo administrar cuentas de desarrollador mediante los grupos en Administración de API de Azure."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 33660b45-442f-44be-9a4a-1899d7f699b0
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 00322340d67fd064a0dc9149790e031b94390709
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2017
---
# <a name="how-to-create-and-use-groups-to-manage-developer-accounts-in-azure-api-management"></a>Creación y uso de grupos para administrar cuentas de desarrollador en Administración de API de Azure
En Administración de API, los grupos se usan para administrar la visibilidad de productos para los desarrolladores. Los productos son visibles en primer lugar para los grupos y luego los desarrolladores de dichos grupos pueden ver y suscribirse a los productos asociados a los grupos. 

Administración de API tiene los siguientes grupos invariables del sistema.

* **Administradores** : los administradores de la suscripción de Azure son miembros de este grupo. Los administradores controlan las instancias del servicio Administración de API y crean las API, las operaciones y los productos que usan los desarrolladores.
* **Desarrolladores** : los usuarios del portal para desarrolladores autenticados se incluyen en este grupo. Los desarrolladores son los clientes que compilan aplicaciones con sus API. Los desarrolladores, después de que se les concede acceso al portal para desarrolladores, crean aplicaciones que llaman a las operaciones de una API.
* **Invitados** : a este grupo pertenecen los usuarios del portal para desarrolladores no autenticados como, por ejemplo, clientes potenciales que visitan el portal para desarrolladores de una instancia de Administración de API. Se les concede determinado acceso de solo lectura, como por ejemplo la posibilidad de ver API pero no llamarlas.

Además de estos grupos del sistema, los administradores pueden crear grupos personalizados o [aprovechar los grupos externos en inquilinos de Azure Active Directory asociados][leverage external groups in associated Azure Active Directory tenants]. Los grupos personalizados y externos pueden usarse junto con grupos del sistema en la concesión a los desarrolladores de visibilidad y acceso a productos de la API. Por ejemplo, podría crear un grupo personalizado para los desarrolladores afiliados a una organización asociada específica y permitirles el acceso a las API a partir de un producto que contenga solo las API relevantes. Un usuario puede ser miembro de más de un grupo.

En esta guía se muestra cómo los administradores de una instancia de Administración de API pueden agregar nuevos grupos y asociarlos a productos y desarrolladores.

> [!NOTE]
> Además de crear y administrar grupos en el portal del editor, puede crear y administrar sus grupos mediante la entidad de [grupo](https://msdn.microsoft.com/library/azure/dn776329.aspx) de la API de REST de administración.
> 
> 

## <a name="create-group"></a>Creación de un grupo
Para crear un nuevo grupo, haga clic en **Publisher portal** (Portal del publicador) en Azure Portal para su servicio de API Management. De este modo, se abre el portal del publicador de Administración de API.

![Portal del publicador][api-management-management-console]

> Si aún no ha creado ninguna instancia del servicio de API Management, consulte [Creación de una instancia del servicio API Management][Create an API Management service instance] en el tutorial [Introducción a Azure API Management][Get started with Azure API Management].
> 
> 

Haga clic en **Grupos** en el menú **API Management** de la izquierda y haga clic en **Agregar grupo**.

![Add new group][api-management-add-group]

Especifique un nombre único para el grupo y una descripción opcional y haga clic en **Guardar**.

![Add new group][api-management-add-group-window]

El nuevo grupo se muestra en la pestaña Grupos. Para editar el **Nombre** o la **Descripción** del grupo, haga clic en el nombre del grupo en la lista. Para eliminar el grupo, haga clic en **Eliminar**.

![Group added][api-management-new-group]

Ahora que se ha creado el grupo, se puede asociar a productos y desarrolladores.

## <a name="associate-group-product"></a>Asociación de un grupo a un producto
Para asociar un grupo a un producto, haga clic en **Productos** en el menú **API Management** de la izquierda y haga clic en el nombre del producto que desee.

![Set visibility][api-management-add-group-to-product]

Seleccione la pestaña **Visibilidad** para agregar y quitar grupos, así como para ver los grupos actuales para el producto. Para agregar o quitar grupos, active o desactive las casillas correspondientes a los grupos que desee y haga clic en **Guardar**.

![Set visibility][api-management-add-group-to-product-visibility]

> [!NOTE]
> Para agregar grupos de Azure Active Directory, consulte [Autorización de las cuentas de desarrollador con Azure Active Directory en Administración de API de Azure](api-management-howto-aad.md).
> 
> Para configurar grupos en la pestaña **Visibilidad** para un producto, haga clic en **Administrar grupos**.
> 
> 

Una vez asociado un producto a un grupo, los desarrolladores del grupo pueden ver el producto y suscribirse a él.

## <a name="associate-group-developer"></a>Asociación de grupos a desarrolladores
Para asociar grupos a desarrolladores, haga clic en **Usuarios** en el menú **API Management** de la izquierda y active la casilla situada junto a los desarrolladores que desee asociar a un grupo.

![Add developer to group][api-management-add-group-to-developer]

Una vez seleccionados los desarrolladores que desee, haga clic en el grupo que desee en la lista desplegable **Agregar a grupo** . Los desarrolladores se pueden quitar de los grupos con la lista desplegable **Quitar de grupo** . 

![Desarrolladores][api-management-add-group-to-developer-saved]

Una vez agregada, la asociación entre el desarrollador y el grupo se puede ver en la pestaña **Usuarios** .

## <a name="next-steps"></a>Pasos siguientes
* Una vez agregado a un grupo, un desarrollador puede ver los productos asociados al grupo y suscribirse a ellos. Para obtener más información, consulte [Creación y publicación de un producto en Azure API Management][How create and publish a product in Azure API Management].
* Además de crear y administrar grupos en el portal del editor, puede crear y administrar sus grupos mediante la entidad de [grupo](https://msdn.microsoft.com/library/azure/dn776329.aspx) de la API de REST de administración.

[api-management-management-console]: ./media/api-management-howto-create-groups/api-management-management-console.png
[api-management-add-group]: ./media/api-management-howto-create-groups/api-management-add-group.png
[api-management-add-group-window]: ./media/api-management-howto-create-groups/api-management-add-group-window.png
[api-management-new-group]: ./media/api-management-howto-create-groups/api-management-new-group.png
[api-management-add-group-to-product]: ./media/api-management-howto-create-groups/api-management-add-group-to-product.png
[api-management-add-group-to-product-visibility]: ./media/api-management-howto-create-groups/api-management-add-group-to-product-visibility.png
[api-management-add-group-to-developer]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer.png
[api-management-add-group-to-developer-saved]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer-saved.png

[api-management-]: ./media/api-management-howto-create-groups/api-management-.png

[Create a group]: #create-group
[Associate a group with a product]: #associate-group-product
[Associate groups with developers]: #associate-group-developer
[Next steps]: #next-steps

[How create and publish a product in Azure API Management]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[leverage external groups in associated Azure Active Directory tenants]: api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group
