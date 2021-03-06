---
title: "Azure Portal: enmascaramiento dinámico de datos de SQL Database | Microsoft Docs"
description: "Introducción al uso del enmascaramiento dinámico de datos de SQL Database en Azure Portal"
services: sql-database
documentationcenter: 
author: ronitr
manager: jhubbard
editor: 
ms.assetid: "2"
ms.service: sql-database
ms.custom: security
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 11/22/2016
ms.author: ronitr; ronmat
ms.openlocfilehash: 15184e14d4e1e23b56126bbf9f972c1619dcba80
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-sql-database-dynamic-data-masking-with-the-azure-portal"></a>Empiece a usar el enmascaramiento dinámico de datos de SQL Database con Azure Portal

En este tema se muestra cómo implementar el [enmascaramiento dinámico de datos](sql-database-dynamic-data-masking-get-started.md) con Azure Portal. También puede implementar el enmascaramiento dinámico de datos mediante [cmdlets de Azure SQL Database](https://msdn.microsoft.com/library/azure/mt574084.aspx) o la [API de REST](https://msdn.microsoft.com/library/dn505719.aspx).


## <a name="set-up-dynamic-data-masking-for-your-database-using-the-azure-portal"></a>Configurar el enmascaramiento de datos dinámicos para la base de datos mediante el Portal de Azure
1. Inicie el Portal de Azure en [https://portal.azure.com](https://portal.azure.com).
2. Desplácese hasta la hoja de configuración de la base de datos que incluye los datos confidenciales que desea enmascarar.
3. Haga clic en el icono **Enmascaramiento de datos dinámicos**, con lo que se iniciará la hoja de configuración **Enmascaramiento de datos dinámicos**.
   
   * Como alternativa, puede desplazarse hacia abajo hasta la sección **Operaciones** y hacer clic en **Enmascaramiento de datos dinámicos**.
     
     ![Panel de navegación](./media/sql-database-dynamic-data-masking-get-started/4_ddm_settings_tile.png)<br/><br/>
4. En la hoja de **Enmascaramiento de datos dinámicos** puede ver algunas columnas de base de datos que el motor de recomendaciones ha marcado para enmascaramiento. Para aceptar las recomendaciones, solo tiene que hacer clic en **Agregar máscara** para una o varias columnas y se creará una máscara en función del tipo predeterminado para esta columna. Para cambiar la función de enmascaramiento, haga clic en la regla de enmascaramiento y edite formato de campo de enmascaramiento en el formato diferente que elija. Asegúrese de hacer clic en **Guardar** para guardar la configuración.
   
    ![Panel de navegación](./media/sql-database-dynamic-data-masking-get-started/5_ddm_recommendations.png)<br/><br/>
5. Para agregar una máscara para cualquier columna de la base de datos, en la parte superior de la hoja de configuración **Enmascaramiento de datos dinámicos**, haga clic en **Agregar máscara** para abrir la hoja de configuración **Agregar regla de enmascaramiento**.
   
    ![Panel de navegación](./media/sql-database-dynamic-data-masking-get-started/6_ddm_add_mask.png)<br/><br/>
6. Seleccione el **esquema**, la **tabla** y la **columna** para definir el campo designado para enmascararse.
7. Elija un **Formato de campo de enmascaramiento** en la lista de categorías de enmascaramiento de datos confidenciales.
   
    ![Panel de navegación](./media/sql-database-dynamic-data-masking-get-started/7_ddm_mask_field_format.png)<br/><br/>        
8. Haga clic en **Guardar** en la hoja de regla de enmascaramiento de datos para actualizar el conjunto de reglas de enmascaramiento en la directiva de enmascaramiento dinámico.
9. Escriba los usuarios SQL o las identidades AAD que deben excluirse del enmascaramiento y tengan acceso a los datos confidenciales sin máscara. Esto debe ser una lista separada por puntos y coma de usuarios. Tenga en cuenta que los usuarios con privilegios de administrador siempre tienen acceso a los datos originales sin máscara.
   
    ![Panel de navegación](./media/sql-database-dynamic-data-masking-get-started/8_ddm_excluded_users.png)
   
   > [!TIP]
   > Para hacer que el nivel de aplicación pueda mostrar datos confidenciales para los usuarios con privilegios de la aplicación, agregue el usuario SQL o identidad AAD que la aplicación usa para consultar la base de datos. Se recomienda que esta lista incluya un número mínimo de usuarios con privilegio para minimizar la exposición de los datos confidenciales.
   > 
   > 
10. Haga clic en **Guardar** en la hoja de configuración de enmascaramiento de datos para guardar la directiva de enmascaramiento nueva o actualizada.


## <a name="next-steps"></a>Pasos siguientes

* Para obtener información general sobre el enmascaramiento dinámico de datos, consulte [este artículo](sql-database-dynamic-data-masking-get-started.md).
* También puede implementar el enmascaramiento dinámico de datos mediante [cmdlets de Azure SQL Database](https://msdn.microsoft.com/library/azure/mt574084.aspx) o la [API de REST](https://msdn.microsoft.com/library/dn505719.aspx).