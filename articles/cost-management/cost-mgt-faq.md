---
title: "Preguntas más frecuentes sobre Azure Cost Management | Microsoft Docs"
description: "En este artículo se ofrecen respuestas a algunas de las preguntas frecuentes sobre Azure Cost Management."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/23/2017
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: 
ms.openlocfilehash: 693f0541376f54d1f702b285775ebae6c5fcbc0c
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2017
---
# <a name="frequently-asked-questions-for-azure-cost-management"></a>Preguntas más frecuentes sobre Azure Cost Management


En este artículo se tratan algunas preguntas comunes sobre Azure Cost Management. Si tiene preguntas acerca de Cost Management, puede formularlas en [FAQs for Azure Cost Management by Cloudyn](https://social.msdn.microsoft.com/Forums/en-US/231bf072-2c71-4121-8339-ac9d868137b9/faqs-for-azure-cost-management-by-cloudyn?forum=Cloudyn) (Preguntas más frecuentes sobre Azure Cost Management en Cloudyn).

## <a name="how-can-i-resolve-common-indirect-enterprise-set-up-errors"></a>¿Cómo se resuelven los errores de configuración empresariales indirectos comunes?

Si es un usuario EA o CSP cuando utiliza el portal de Cloudyn por primera vez, es posible que observe los siguientes errores:

- **The specified API key is not a top level enrollment key** (La clave de API especificada no es una clave de registro de nivel superior) aparece en el Asistente para la configuración de Azure Cost Management.
- **Direct Enrollment – No** (Inscripción directa: no) aparece en el portal de EA.
- **No usage data was found for the last 30 days. Please contact your distributor to make sure markup was enabled for your Azure account**  (No se encontraron datos de uso de los últimos 30 días. Póngase en contacto con su distribuidor para asegurarse de que se habilitó el margen de beneficio para su cuenta de Azure) aparece en el portal de Cloudyn.

Los errores anteriores indican que adquirió un Contrato Enterprise de Azure a través de un revendedor o CSP. El CSP o el revendedor deben habilitar el _margen de beneficio_ para su cuenta de Azure a fin de que pueda ver sus datos en Cloudyn.

Aquí se muestra cómo corregir los errores:

1. El revendedor debe habilitar el _margen de beneficio_ para su cuenta. Consulte las instrucciones en la [guía de incorporación para clientes indirectos ](https://ea.azure.com/api/v3Help/v2IndirectCustomerOnboardingGuide) (en inglés).
2. Debe generar la clave de Azure EA para su uso con Cloudyn. Consulte las instrucciones de [Adding Your Azure EA](https://support.cloudyn.com/hc/en-us/articles/210429585-Adding-Your-AZURE-EA) (Agregar clave de Azure EA) o [How to Find Your EA Enrollment ID and API Key](https://youtu.be/u_phLs_udig) (Cómo buscar el identificador de inscripción de EA y la clave de API).

Solo un administrador de servicios de Azure puede habilitar Cost Management. Los permisos de coadministrador son insuficientes.

Habilite la API de facturación de Azure para poder generar la clave de API de Azure EA para configurar Cloudyn siguiendo las instrucciones que aparecen en:

- [Introducción a las API de informes para clientes de Enterprise](../billing/billing-enterprise-api.md)
- [API de informes de Microsoft Azure Enterprise Portal](https://ea.azure.com/helpdocs/reportingAPI) en _Habilitación del acceso de datos a la API_.


Es posible que también deba conceder permisos a administradores de departamento, propietarios de cuentas y administradores de empresa para _ver cargos_ con la API de facturación.

## <a name="how-do-i-enable-suspended-or-locked-out-users"></a>¿Cómo se habilitan los usuarios suspendidos o bloqueados?

Si recibe una alerta que solicita permitir el acceso para un usuario, debe activar la cuenta de usuario.

Para activar la cuenta de usuario:

1. Inicie sesión en Cloudyn con la cuenta de usuario administrativo de Azure que usó para configurar Cloudyn. O bien, inicie sesión con una cuenta de usuario a la que se haya concedido acceso de administrador.
2. Haga clic en el símbolo de engranaje en la esquina superior derecha y seleccione **User Management** (Administración de usuarios).
3. Busque el usuario y, a continuación, haga clic en el símbolo de lápiz y edite el usuario.
4. En **User status** (Estado del usuario), cambie el estado de **Suspended** (Suspendido) a **Active** (Activo).

Las cuentas de usuario de Cloudyn se conectan mediante el inicio de sesión único de Azure. Si un usuario escribe mal su contraseña, podría bloquearse en Cloudyn, aunque podría seguir accediendo a Azure.

Si cambia la dirección de correo electrónico en Cloudyn de la dirección predeterminada de Azure, es posible que su cuenta se bloquee. Puede que aparezca el error: _status initiallySuspended_. Si su cuenta de usuario se bloquea, póngase en contacto con el administrador alternativo para restablecer su cuenta.

Se recomienda crear al menos dos de cuentas de administrador de Cloudyn por si una de ellas se bloquea.

Si no puede iniciar sesión en el portal de Cloudyn, asegúrese de que está utilizando la dirección URL de Azure Cost Management correcta para iniciar sesión en Cloudyn. Use una de las direcciones URL siguientes:

- https://azure.cloudyn.com
- https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/CloudynMainBlade

Evite usar la dirección URL directa de Cloudyn https://app.cloudyn.com.

## <a name="how-to-activate-unactivated-accounts-with-azure-credentials"></a>Cómo activar cuentas no activadas con las credenciales de Azure

Tan pronto como Cloudyn detecte las cuentas de Azure, los datos de costos de proporcionarán inmediatamente en informes basados en costos. Sin embargo, para que Cloudyn proporcione datos de uso y rendimiento, debe registrar sus credenciales de Azure para las cuentas. Siga las instrucciones de [Adding Azure Resource Manager](https://support.cloudyn.com/hc/en-us/articles/212784085-Adding-Azure-Resource-Manager) (Agregar Azure Resource Manager).

Para agregar las credenciales de Azure de una cuentan, en el portal de Cloudyn, haga clic en el símbolo de edición a la derecha del nombre de cuenta, no de la suscripción.

Hasta que sus credenciales de Azure se agreguen a Cloudyn, la cuenta aparecerá como _inactiva_.

## <a name="how-do-i-add-multiple-accounts-and-entities-to-an-existing-subscription"></a>¿Cómo agrego varias cuentas y entidades a una suscripción existente?

En los vínculos siguientes se describe cómo agregar entidades adicionales. Las entidades adicionales se usan para agregar contratos Enterprise adicionales a una suscripción de Cloudyn:

- Artículo [Adding an Entity](https://support.cloudyn.com/hc/en-us/articles/212016145-Adding-an-Entity) (Agregar una entidad)
- Vídeo [Defining your hierarchy with Cost Entities](https://support.cloudyn.com/hc/en-us/articles/115005142529-Video-Defining-your-hierarchy-with-Cost-Entities) (Definir la jerarquía con entidades de costos)

Para los CSP:

Para agregar cuentas de CSP adicionales a una entidad, seleccione **MSP Access** (Acceso de MSP) en lugar de **Enterprise** al crear la nueva entidad. Si su cuenta está registrada como un contrato Enterprise y desea agregar credenciales de CSP, es posible que el personal de soporte técnico de Cloudyn tenga que modificar la configuración de su cuenta. Si es un suscriptor de pago de Azure, puede crear una nueva solicitud de soporte técnico en Azure Portal. Haga clic en **Ayuda y soporte técnico** y, a continuación, en **Nueva solicitud de soporte técnico**.

## <a name="how-do-i-change-the-currency-symbol-used-in-cloudyn"></a>¿Cómo se cambia el símbolo de moneda usado en Cloudyn?

Cuando todas las cuentas de Azure en una sola entidad usan la misma moneda, se detecta automáticamente la moneda empleada. Sin embargo, el símbolo de divisa se muestra incorrectamente como **$** para cualquiera de las monedas siguientes:

- GBP = libra esterlina del Reino Unido
- EUR = euro
- INR = rupia india
- NOK = corona noruega

Aunque es posible que se muestre el símbolo de moneda **$** para dólares estadounidenses, los valores de costos se muestran en la moneda correcta. Por ejemplo, si todas las cuentas utilizan euros en la misma entidad, los _valores_ que se muestran en Cloudyn son en euros, aunque el símbolo **$** aparezca de forma errónea.

Si es un cliente de Azure EA, el personal de soporte técnico de Cloudyn puede cambiar el símbolo de moneda que se muestra en los informes de costos de $. Puede crear una nueva solicitud de soporte técnico en Azure Portal. Haga clic en **Ayuda y soporte técnico** y, a continuación, en **Nueva solicitud de soporte técnico**.

Si es un cliente CSP, no puede cambiar el símbolo de moneda. Cloudyn solo admite listas de tarifas en dólares estadounidenses. Cloudyn está valorando la opción para admitir listas de tarifas en distintas monedas.

## <a name="what-are-cloudyn-data-refresh-timelines"></a>¿Qué son las escalas de tiempo de actualización de datos de Cloudyn?

Cloudyn tiene las escalas de tiempo de actualización de datos siguientes:

- Inicial: se pueden tardar hasta 24 horas en ver los datos de costos en Cloudyn después de la configuración. Cloudyn puede tardar hasta 10 días en recopilar datos suficientes para mostrar recomendaciones de ajuste de tamaño.
- Diaria: desde el décimo día hasta el final de cada mes, Cloudyn deben mostrar sus datos actualizados del día anterior después de UTC+3 aproximadamente al día siguiente.
- Mensual: desde el primer día hasta el décimo de cada mes, es posible que Cloudyn solo muestre los datos hasta el final del mes anterior.

Cloudyn procesa los datos del día anterior cuando todos los datos del día anterior están disponibles. Los datos del día anterior están disponibles normalmente en Cloudyn hacia la hora UTC+3 cada día. Algunos datos, como las etiquetas, pueden tardar 24 horas más en procesarse.

Los datos del mes actual no están disponibles para su recopilación al principio de cada mes. Durante el período, los proveedores de servicios finalizan su facturación para el mes anterior. Los datos del mes anterior aparecen en Cloudyn entre cinco y diez días después del inicio de cada mes. Durante este tiempo, podría ver solo los costos amortizados del mes anterior. Es posible que no vea los datos de facturación o uso diarios. Cuando los datos están disponibles, Cloudyn los procesa con carácter retroactivo. Después del procesamiento, se muestran todos los datos mensuales entre los días 5 y 10 de cada mes.

Si se produce un retraso al enviar datos de Azure a Cloudyn, los datos se siguen registrando en Azure. Los datos se transfieren a Cloudyn cuando se restaura la conexión.

## <a name="how-can-a-direct-csp-configure-cloudyn-access-for-indirect-csp-customers-or-partners"></a>¿Cómo debe configurar un CSP directo el acceso a Cloudyn para asociados o clientes de CSP indirectos?

Consulte [Configuración del acceso indirecto a CSP en Cloudyn](quick-register-csp.md#configure-indirect-csp-access-in-cloudyn) para obtener instrucciones.
