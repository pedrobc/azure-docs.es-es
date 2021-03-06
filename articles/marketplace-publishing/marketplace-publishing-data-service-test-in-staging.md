---
title: Prueba de la oferta de Data Service para Marketplace | Microsoft Docs
description: "Obtenga información acerca de cómo probar la oferta del Servicio de datos para Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: e861bd11-f74d-4d77-b4b5-23fb463644ad
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 56a8aad7484fed18b74200ffa7acf22363625a15
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2017
---
# <a name="testing-your-data-service-offer-in-staging"></a>Probar su oferta de Servicio de datos en Ensayo
> [!IMPORTANT]
> **En este momento, ya no se pueden incorporar nuevos editores del Servicio de datos. No se aprobarán nuevos servicios de datos para mostrarse en lista.** Si tiene una aplicación de negocio de SaaS que desea publicar en AppSource, puede encontrar más información [aquí](https://appsource.microsoft.com/partners). Si tiene aplicaciones IaaS o un servicio de desarrollo que quiera publicar en Azure Marketplace, puede encontrar más información [aquí](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

Después de completar los dos primeros pasos de [Crear una cuenta de desarrollador de Microsoft](marketplace-publishing-accounts-creation-registration.md) y [Crear la oferta de Data Service en el portal de publicación](marketplace-publishing-data-service-creation.md), está listo para que su oferta se encuentre disponible en Azure Marketplace. Este tema le guiará a través del primer paso intermedio denominado "Ensayo".

En la etapa de ensayo se implementa la oferta en un “espacio aislado” privado, donde puede probar y comprobar su funcionalidad antes pasarla a producción. La oferta aparecerá en un entorno de ensayo tal y como lo haría para un cliente que la implementase.

## <a name="step-1-pushing-your-offer-to-staging"></a>Paso 1. Traslado de su oferta a un almacenamiento provisional
El traslado de la oferta a un almacenamiento provisional permite probar la oferta antes de que esté disponible para suscriptores futuros.  Puede ver cómo aparecerá y funcionará la oferta para aquellos que se suscriban a sus datos.  

  ![dibujo](media/marketplace-publishing-data-service-test-in-staging/step-1.1.png)

1. Inicie sesión en el [Portal de publicación](https://publish.windowsazure.com)
2. Seleccione **Servicios de datos** en la ventana de navegación izquierda
3. Seleccione la oferta que desee trasladar al almacenamiento provisional. Verá la pantalla anterior.
4. Haga clic en el botón **Trasladar a almacenamiento provisional** .  
5. Si hay problemas con la oferta que deban solucionarse antes de su traslado al entorno de ensayo, se incluirán en una lista.  Para corregirlos, haga clic en cada elemento de la lista. Cuando se hayan efectuado todas las correcciones, haga clic en el botón **Trasladar a almacenamiento provisional** nuevamente.

Si no hay ningún problema con su oferta, se visualizará la ventana emergente que aparece a continuación.  

Si no ha planeado o no está autorizado a exponer su oferta en el Portal de Azure (actualmente dispone de una capacidad limitada), simplemente cierre la ventana emergente.

Para probar el Servicio de datos en el Portal de Azure (además del portal de DataMarket), necesitará un identificador de una suscripción de Azure con el que efectuar la prueba.  Este identificador de suscripción identificará la cuenta que podrá probar su oferta.  

Corte y pegue el identificador de suscripción y haga clic en la marca de verificación para continuar.

  ![dibujo](media/marketplace-publishing-data-service-test-in-staging/step-1.2.png)

> [!NOTE]
> Estos identificadores de suscripciones a Azure solo son necesarios para las pruebas y el almacenamiento provisional en el [Portal de administración de Azure](https://manage.windowsazure.com). No se requieren para efectuar pruebas en Azure Marketplace.
> 
> 

En la siguiente pantalla que aparece se muestra que se está efectuando la publicación mediante el icono "En curso" resaltado en amarillo a continuación. La inserción en el almacenamiento provisional dura entre 10 y 15 minutos.  Si tarda más, primero actualice su explorador (presione F5 en Internet Explorer).  Aunque es extraño que suceda, si la oferta se sigue insertando en el entorno de ensayo después de una hora, haga clic en el vínculo "Póngase en contacto con nosotros" para hacernos saber que hay un problema.

  ![dibujo](media/marketplace-publishing-data-service-test-in-staging/step-1.3.png)

Cuando finalice la inserción en el almacenamiento provisional, el icono "En curso" dejará de moverse y el estado se actualizará a "Almacenado provisionalmente".  Ahora ya estará preparado para probar su oferta.  

## <a name="step-2-test-your-staged-offer-in-datamarket"></a>Paso 2: Probar su oferta almacenada provisionalmente en DataMarket
Haga clic en el vínculo que aparece detrás del texto **"Ver su oferta de servicio en..."** para mostrar la pantalla que verá el suscriptor cuando la oferta pase a producción y aparecerá en DataMarket.

  ![dibujo](media/marketplace-publishing-data-service-test-in-staging/step-2.2.png)

Pruebe o compruebe cada uno de los 12 elementos marcados anteriormente para asegurarse de que todos los logotipos, los precios, las transacciones, el texto, las imágenes, la documentación y los vínculos son correctos y funcionan bien.  Se trata de un buen momento para asegurarse de que los valores de prueba introducidos al crear la oferta se han reemplazado por valores reales.

1. Logotipo de la oferta
2. Nombre de la oferta
3. Nombre del publicador/vínculo al sitio web de su compañía
4. Categorías de búsqueda para su oferta
5. Vínculo de soporte técnico de su oferta para ayudar a los suscriptores
6. Descripción contextual de la oferta
7. Plan de oferta que describe los detalles de la facturación
8. Vínculo al código de implementación
9. Imágenes de ejemplo que ilustran el uso de los datos de la oferta
10. Metadatos de entrada y salida para cada servicio de la oferta
11. Términos de uso de la oferta
12. Vista previa de los datos de la oferta

Por último, compruebe que el servicio funcione a través de Datamarket haciendo clic en el vínculo "EXPLORAR ESTE CONJUNTO DE DATOS".  Se abrirá una nueva ventana de la herramienta llamada "Explorador de servicios" para que pueda obtener una vista previa de los resultados de una consulta a su servicio.  En esta ventana, puede especificar los parámetros necesarios y ver los resultados que aparecen en una consulta a su servicio.   También se muestra la dirección URL de la consulta.  

> [!NOTE]
> Asegúrese de revisar la descripción textual del servicio que se muestra en la parte superior.  Y si su oferta consta de más de una llamada de servicio, haga clic en las pestañas de la parte inferior para pasar al siguiente servicio para revisar y probar.
> 
> 

## <a name="next-step"></a>Paso siguiente
Si está teniendo problemas y necesita ayuda para resolverlos, póngase en contacto con el [soporte técnico para publicadores de Azure](http://go.microsoft.com/fwlink/?LinkId=272975).

Si está satisfecho y listo para publicar su oferta, lea la documentación [Solicitud de aprobación para la inserción en producción](marketplace-publishing-push-to-production.md) .

## <a name="see-also"></a>Otras referencias
* [Introducción: cómo publicar una oferta en Azure Marketplace](marketplace-publishing-getting-started.md)

