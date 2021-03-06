---
title: "Tutorial: Configuración de Samanage para aprovisionar automáticamente usuarios con Azure Active Directory | Microsoft Docs"
description: "Obtenga información sobre cómo configurar Azure Active Directory para aprovisionar y cancelar automáticamente el aprovisionamiento de cuentas de usuario de Samanage."
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
ms.openlocfilehash: 278ebf464fbe815568fbe332f80d5ea6b29e1811
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-configuring-samanage-for-automatic-user-provisioning"></a>Tutorial: Configuración de Samanage para aprovisionar a los usuarios automáticamente


El objetivo de este tutorial es explicar los pasos que debe realizar en Samanage y Azure AD para aprovisionar y cancelar automáticamente el aprovisionamiento de cuentas de usuario de Azure AD para Samanage. 

## <a name="prerequisites"></a>Requisitos previos

En la situación descrita en este tutorial se supone que ya cuenta con los elementos siguientes:

*   Un inquilino de Azure Active Directory
*   Un inquilino de Samanage con el [plan Professional](https://www.samanage.com/pricing/) habilitado o uno superior 
*   Una cuenta de usuario de Samanage con permisos de administrador 

> [!NOTE]
> La integración del aprovisionamiento de Azure AD se basa en la [API de Samanage de REST](https://www.samanage.com/api/), que está disponible para los equipos de Samanage con el plan Professional o uno superior.

## <a name="assigning-users-to-samanage"></a>Asignación de usuarios a Samanage

Azure Active Directory usa un concepto que se denomina "asignaciones" para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto del aprovisionamiento automático de cuentas de usuario, solo se sincronizarán los usuarios y grupos que se han "asignado" a una aplicación de Azure AD. 

Antes de configurar y habilitar el servicio de aprovisionamiento, debe decidir qué usuarios o grupos de Azure AD representan a los usuarios que necesitan acceso a la aplicación Samanage. Una vez decidido, puede asignar estos usuarios a la aplicación de Samanage siguiendo estas instrucciones:

[Asignar un usuario o grupo a una aplicación empresarial](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-samanage"></a>Sugerencias importantes para asignar usuarios a Samanage

*   Se recomienda asignar un único usuario de Azure AD a Samanage para probar la configuración de aprovisionamiento. Más tarde, se pueden asignar otros usuarios o grupos.

*   Al asignar un usuario a Samanage, debe seleccionar el rol **Usuario** u otro válido específico de la aplicación (si está disponible) en el cuadro de diálogo de asignación. El rol **Acceso predeterminado** no funciona para realizar el aprovisionamiento y estos usuarios se omiten.

> [!NOTE]
> Como característica agregada, el servicio de aprovisionamiento lee los roles personalizados definidos en Samanage y los importa en Azure AD, donde se pueden seleccionar en el cuadro de diálogo Seleccionar rol. Estos roles se verán en Azure Portal después de habilitar el servicio de aprovisionamiento y de completarse un ciclo de sincronización.

## <a name="configuring-user-provisioning-to-samanage"></a>Configuración del aprovisionamiento de usuarios en Samanage 

Esta sección lo guía a través de los pasos necesarios para conectar la API de aprovisionamiento de su cuenta de usuario de Samanage, así como para configurar el servicio de aprovisionamiento con el fin de crear, actualizar y deshabilitar cuentas de usuario asignadas de Samanage en función de la asignación de grupos y usuarios Azure AD.

> [!TIP]
> También puede decidir habilitar el inicio de sesión único basado en SAML para Samanage siguiendo las instrucciones de [Azure Portal](https://portal.azure.com). El inicio de sesión único puede configurarse independientemente del aprovisionamiento automático, aunque estas dos características se complementan entre sí.


### <a name="configure-automatic-user-account-provisioning-to-samanage-in-azure-ad"></a>Para configurar el aprovisionamiento de cuentas de usuario automático para Samanage en Azure AD, siga estos pasos:


1. En [Azure Portal](https://portal.azure.com), vaya a la sección **Azure Active Directory > Aplicaciones empresariales > Todas las aplicaciones**.

2. Si ya ha configurado Samanage para el inicio de sesión único, busque la instancia de Samanage mediante el campo de búsqueda. En caso contrario, seleccione **Agregar** y busque **Samanage** en la galería de aplicaciones. Seleccione Samanage en los resultados de la búsqueda y agréguelo a la lista de aplicaciones.

3. Seleccione la instancia de Samanage y, después, seleccione la pestaña **Aprovisionamiento**.

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Aprovisionamiento de Samanage](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage1.png)

5. En la sección **Credenciales de administrador**, escriba **el nombre de usuario y la contraseña de administrador** de la cuenta de Samanage. 

6. En Azure Portal, haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a la aplicación Samanage. Si la conexión no se establece, asegúrese de que la cuenta de Samanage tiene permisos de administrador de equipo e intente el paso 5 de nuevo.

7. Escriba la dirección de correo electrónico de una persona o grupo que debe recibir las notificaciones de error de aprovisionamiento en el campo **Correo electrónico de notificación** y active la casilla "Enviar una notificación por correo electrónico cuando se produzca un error".

8. Haga clic en **Guardar**. 

9. En la sección Asignaciones, seleccione **Synchronize Azure Active Directory Users to** (Sincronizar usuarios de Azure Active Directory con Samanage).

10. En la sección **Attribute Mappings** (Asignaciones de atributos), revise los atributos de usuario que se sincronizan entre Azure AD y Samanage. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de Samanage con el objetivo de realizar operaciones de actualización. Seleccione el botón Guardar para confirmar los cambios.

11. Para habilitar el aprovisionamiento del servicio de aprovisionamiento de Azure AD para Samanage, cambie el **estado de aprovisionamiento** a **Activado** en la sección **Configuración**.

12. Haga clic en **Guardar**. 

Esta operación inicia la sincronización inicial de todos los usuarios y grupos asignados a Samanage en la sección Usuarios y grupos. La sincronización inicial tardará más tiempo en realizarse que las posteriores, que se producen aproximadamente cada 20 minutos si se está ejecutando el servicio. Puede usar la sección **Detalles de sincronización** para supervisar el progreso y hacer clic en los vínculos a los informes de actividad de aprovisionamiento, que describen todas las acciones que ha llevado a cabo el servicio de aprovisionamiento.

Para más información sobre cómo leer los registros de aprovisionamiento de Azure AD, consulte [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).


## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](active-directory-enterprise-apps-manage-provisioning.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](active-directory-saas-provisioning-reporting.md)
