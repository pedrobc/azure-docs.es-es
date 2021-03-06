---
title: "Guía de solución de problemas de implementación de Azure Machine Learning | Microsoft Docs"
description: "Guía de solución de problemas para la implementación y creación de servicios"
services: machine-learning
author: raymondl
ms.author: raymondl
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 10/09/2017
ms.openlocfilehash: b9287c7151c96aaccbcda81c111cfe36ead5ab38
ms.sourcegitcommit: 1131386137462a8a959abb0f8822d1b329a4e474
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2017
---
# <a name="troubleshooting-service-deployment-and-environment-setup"></a>Solución de problemas de implementación del servicio y configuración del entorno
La siguiente información puede ayudar a determinar la causa de errores al configurar el entorno de Administración de modelos.

## <a name="model-management-environment"></a>Entorno de Administración de modelos
### <a name="owner-permission-required"></a>Permiso de propietario necesario
Debe tener permiso de propietario en la suscripción de Azure para registrar el proceso de Machine Learning.

También necesita permiso de propietario para configurar un clúster para la implementación de los servicios web.

### <a name="resource-availability"></a>Disponibilidad de recursos
Debe tener suficientes recursos disponibles en su suscripción, de forma que pueda aprovisionar los recursos del entorno.

### <a name="subscription-caps"></a>Límites de suscripción
La suscripción puede tener un límite de facturación que podrían impedir el aprovisionamiento de los recursos del entorno. Elimine ese límite para permitir el aprovisionamiento.

### <a name="enable-debug-and-verbose-options"></a>Habilitación de las opciones de depuración y detallado
Use las marcas `--debug` y  `--verbose` en el comando de configuración para mostrar información de depuración y seguimiento a medida que se aprovisiona el entorno.

```
az ml env setup -l <loation> -n <name> -c --debug --verbose 
```

## <a name="service-deployment"></a>Implementación del servicio
La siguiente información le puede ayudar a determinar la causa de los errores que se producen durante la implementación o al llamar al servicio web.

### <a name="service-logs"></a>Registros de servicios
La opción `logs` de la CLI del servicio proporciona datos de registro de Docker y Kubernetes.

```
az ml service logs realtime -i <web service id>
```

Para una configuración adicional del registro, use la opción `--help` (o `-h`).

```
az ml service logs realtime -h
```

### <a name="debug-and-verbose-options"></a>Opciones de depuración y detallado
Use la marca `--debug` para mostrar los registros de depuración a medida que se implementa el servicio.

```
az ml service create realtime -m <modelfile>.pkl -f score.py -n <service name> -r python --debug
```

Use la marca `--verbose` para consultar detalles adicionales a medida que se implementa el servicio.

```
az ml service create realtime -m <modelfile>.pkl -f score.py -n <service name> -r python --verbose
```

### <a name="enable-request-logging-in-app-insights"></a>Habilitación del registro de solicitudes en App Insights
Establezca la marca `-l` en true cuando cree un servicio web para habilitar el registro en el nivel de solicitud. Los registros de solicitudes se escriben en la instancia de App Insights del entorno en Azure. Busque esta instancia mediante el nombre del entorno que utilizó al usar el comando `az ml env setup`.

- Establezca `-l` en true al crear el servicio.
- Abra App Insights en Azure Portal. Use el nombre del entorno para encontrar la instancia de App Insights.
- Una vez en App Insights, haga clic en Buscar en el menú superior para ver los resultados.
- O vaya a `Analytics` > `Exceptions` > `exceptions take | 10`.


### <a name="add-error-handling-in-scoring-script"></a>Adición de control de errores en el script de puntuación
Use el control de excepciones en la función **run** del script `scoring.py` para devolver el mensaje de error como parte de la salida del servicio web.

Ejemplo de Python:
```
    try:
        <code to load model and score>
   except Exception as e:
        return(str(e))
```

## <a name="other-common-problems"></a>Otros problemas comunes
- Si el comando `env setup` produce un error, asegúrese de que tiene suficientes núcleos disponibles en su suscripción.
- No utilice el guión bajo (_) en el nombre del servicio web (como en *my_webservice*).
- Vuelva a intentarlo si se produce un error **502 - Puerta de enlace no válida** al llamar al servicio web. Normalmente esto significa que el contenedor no se ha implementado todavía en el clúster.
- Si obtiene el error **CrashLoopBackOff** al crear un servicio, compruebe los registros. Normalmente es el resultado de dependencias que faltan en la función **init**.
