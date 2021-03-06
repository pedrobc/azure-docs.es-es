---
title: "Tutorial: Configuración de ZenDesk para aprovisionar automáticamente usuarios con Azure Active Directory | Microsoft Docs"
description: "Obtenga información sobre cómo configurar Azure Active Directory para aprovisionar y cancelar automáticamente el aprovisionamiento de cuentas de usuario de ZenDesk."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: asmalser-msft
ms.openlocfilehash: 1a1414eefd20e6d7c025da08cfd5ae7c45daad33
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-configuring-zendesk-for-automatic-user-provisioning"></a>Tutorial: Configuración de ZenDesk para aprovisionar automáticamente usuarios


El objetivo de este tutorial es explicar los pasos que debe realizar en ZenDesk y Azure AD para aprovisionar y cancelar automáticamente el aprovisionamiento de cuentas de usuario de Azure AD para ZenDesk. 

## <a name="prerequisites"></a>Requisitos previos

En la situación descrita en este tutorial se supone que ya cuenta con los elementos siguientes:

*   Un inquilino de Azure Active Directory
*   Un inquilino de ZenDesk con el [plan Enterprise](https://www.zendesk.com/product/pricing/) o uno superior habilitado 
*   Una cuenta de usuario de ZenDesk con permisos de administrador 

> [!NOTE]
> La integración del aprovisionamiento de Azure AD se basa en la [API de ZenDesk de REST](https://developer.zendesk.com/rest_api/docs/core/introduction#the-api), que está disponible para los equipos de ZenDesk con el plan Essential o uno superior.

## <a name="assigning-users-to-zendesk"></a>Asignación de usuarios a ZenDesk

Azure Active Directory usa un concepto que se denomina "asignaciones" para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto de aprovisionamiento automático de cuentas de usuario, solo se sincronizarán los usuarios y grupos que se han "asignado" a una aplicación en Azure AD. 

Antes de configurar y habilitar el servicio de aprovisionamiento, debe decidir qué usuarios o grupos de Azure AD representan a los usuarios que necesitan acceso a la aplicación ZenDesk. Una vez decidido, puede asignar estos usuarios a la aplicación de ZenDesk siguiendo estas instrucciones:

[Asignar un usuario o grupo a una aplicación empresarial](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-zendesk"></a>Sugerencias importantes para asignar usuarios a ZenDesk

*   Se recomienda asignar un único usuario de Azure AD a ZenDesk para probar la configuración de aprovisionamiento. Más tarde, se pueden asignar otros usuarios o grupos.

*   Al asignar un usuario a ZenDesk, debe seleccionar el rol **Usuario** u otro válido específico de la aplicación (si está disponible) en el cuadro de diálogo de asignación. El rol **Acceso predeterminado** no funciona para realizar el aprovisionamiento y estos usuarios se omiten.

> [!NOTE]
> Como característica agregada, el servicio de aprovisionamiento lee los roles personalizados definidos en Zendesk y los importa en Azure AD, donde se pueden seleccionar en el cuadro de diálogo Seleccionar rol. Estos roles se verán en Azure Portal después de habilitar el servicio de aprovisionamiento y de completarse un ciclo de sincronización.

## <a name="configuring-user-provisioning-to-zendesk"></a>Configuración del aprovisionamiento de usuarios en ZenDesk 

Esta sección lo guía a través de los pasos necesarios para conectar Azure AD a la API de aprovisionamiento de su cuenta de usuario de ZenDesk, así como para configurar el servicio de aprovisionamiento con el fin de crear, actualizar y deshabilitar cuentas de usuario asignadas de ZenDesk en función de la asignación de grupos y usuarios en Azure AD.

> [!TIP] 
> También puede habilitar el inicio de sesión único basado en SAML para ZenDesk siguiendo las instrucciones de [Azure Portal](https://portal.azure.com). El inicio de sesión único puede configurarse independientemente del aprovisionamiento automático, aunque estas dos características se complementan entre sí.


### <a name="configure-automatic-user-account-provisioning-to-zendesk-in-azure-ad"></a>Configuración el aprovisionamiento automático de cuentas de usuario para ZenDesk en Azure AD


1. En [Azure Portal](https://portal.azure.com), vaya a la sección **Azure Active Directory > Aplicaciones empresariales > Todas las aplicaciones**.

2. Si ya ha configurado ZenDesk para el inicio de sesión único, busque la instancia de ZenDesk mediante el campo de búsqueda. En caso contrario, seleccione **Agregar** y busque **ZenDesk** en la galería de aplicaciones. Seleccione ZenDesk en los resultados de búsqueda y agréguelo a la lista de aplicaciones.

3. Seleccione la instancia de ZenDesk y, después, seleccione la pestaña **Aprovisionamiento**.

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Aprovisionamiento de ZenDesk](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk1.png)

5. En la sección **Credenciales de administrador**, escriba el valor de **Admin Username&tokenkey&Domain** (nombre de usuario administrador&clave de token&dominio) generado por su cuenta de ZenDesk (puede encontrar el token en su cuenta: **Administrador** > **API** > **Configuración**). 

    ![Aprovisionamiento de ZenDesk](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk2.png)

6. En Azure Portal, haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a la aplicación de ZenDesk. Si la conexión no se establece, asegúrese de que la cuenta de ZenDesk tiene permisos de administrador de equipo e intente el paso 5 de nuevo.

7. Escriba la dirección de correo electrónico de una persona o grupo que debe recibir las notificaciones de error de aprovisionamiento en el campo **Correo electrónico de notificación** y active la casilla "Enviar una notificación por correo electrónico cuando se produzca un error".

8. Haga clic en **Guardar**. 

9. En la sección Asignaciones, seleccione **Synchronize Azure Active Directory Users to ZenDesk** (Sincronizar usuarios de Azure Active Directory con ZenDesk).

10. En la sección **Attribute Mappings** (Asignaciones de atributos), revise los atributos de usuario que se sincronizan entre Azure AD y ZenDesk. Los atributos seleccionados como propiedades de **Matching** (Coincidencia) se usan para buscar coincidencias con las cuentas de usuario de ZenDesk para las operaciones de actualización. Seleccione el botón Guardar para confirmar los cambios.

11. Para habilitar el servicio de aprovisionamiento de Azure AD para ZenDesk, cambie el **Estado de aprovisionamiento** a **Activado** en la sección **Configuración**.

12. Haga clic en **Guardar**. 

Esta operación inicia la sincronización inicial de todos los usuarios y grupos asignados a ZenDesk en la sección Usuarios y grupos. La sincronización inicial tardará más tiempo en realizarse que las posteriores, que se producen aproximadamente cada 20 minutos, si se está ejecutando el servicio. Puede usar la sección **Detalles de sincronización** para supervisar el progreso y hacer clic en los vínculos a los informes de actividad de aprovisionamiento, que describen todas las acciones que ha llevado a cabo el servicio de aprovisionamiento.

Para más información sobre cómo leer los registros de aprovisionamiento de Azure AD, consulte [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).


## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](active-directory-enterprise-apps-manage-provisioning.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](active-directory-saas-provisioning-reporting.md)
