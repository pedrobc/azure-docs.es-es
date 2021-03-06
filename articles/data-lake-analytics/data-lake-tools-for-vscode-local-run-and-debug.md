---
title: "Herramientas de Azure Data Lake: Ejecución y depuración locales de U-SQL con Visual Studio Code | Microsoft Docs"
description: "Obtenga información sobre cómo usar Herramientas de Azure Data Lake para Visual Studio Code para la ejecución y depuración locales."
Keywords: "VSCode, Herramientas de Azure Data Lake, ejecución local, depuración local, archivo de almacenamiento de versión preliminar, cargar en la ruta de acceso de almacenamiento"
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: DJ
editor: jejiang
tags: azure-portal
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: big-data
ms.date: 07/14/2017
ms.author: jejiang
ms.openlocfilehash: 78a5efb19f73192dcc95103abc70c14a3ce2e29a
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="u-sql-local-run-and-local-debug-for-windows-with-visual-studio-code"></a>Ejecución y depuración locales de U-SQL para Windows con Visual Studio Code
En este documento, aprenderá a ejecutar trabajos de U-SQL en un equipo de desarrollo local para acelerar la primeras fases de codificación o depurar código localmente en Visual Studio Code. Para consultar instrucciones sobre las Herramientas de Azure Data Lake para Visual Studio Code, vea [Uso de Herramientas de Azure Data Lake para Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md). 


## <a name="set-up-the-u-sql-local-run-environment"></a>Configuración del entorno de ejecución local de U-SQL

1. Seleccione Ctrl+Mayús+P para abrir la paleta de comandos y, luego, escriba **ADL: Download Local Run Dependency** para descargar los paquetes.  

   ![Descarga de los paquetes de dependencia de ADL LocalRun](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/DownloadLocalRun.png)

2. Busque los paquetes de dependencia de la ruta de acceso que se muestra en el panel **Salida** y, luego, instale BuildTools y Win10SDK 10240. A continuación se muestra una ruta de acceso de ejemplo:  
`C:\Users\xxx\.vscode\extensions\usqlextpublisher.usql-vscode-ext-x.x.x\LocalRunDependency
`  
  ![Ubicación de los paquetes de dependencia](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/LocateDependencyPath.png)

   2.1 Para instalar **BuildTools**, haga clic en visualcppbuildtools_full.exe en la carpeta LocalRunDependency y después siga las instrucciones del asistente.   

    ![Instalación de BuildTools](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallBuildTools.png)

   2.2 Para instalar **Win10SDK 10240**, haga clic en sdksetup.exe en la carpeta LocalRunDependency/Win10SDK_10.0.10240_2 y después siga las instrucciones del asistente.  

    ![Instalación de Win10SDK 10240](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallWin10SDK.png)

3. Configure la variable de entorno. Establezca la variable de entorno **SCOPE_CPP_SDK** en:  
`C:\Users\xxx\.vscode\extensions\usqlextpublisher.usql-vscode-ext-x.x.x\LocalRunDependency\CppSDK_3rdparty
`  
4. Reinicie el SO para asegurarse de que la configuración de la variable de entorno surta efecto.  

   ![Asegúrese de que la variable de entorno SCOPE_CPP_SDK está instalada](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/ConfigScopeCppSDk.png)

## <a name="start-the-local-run-service-and-submit-the-u-sql-job-to-a-local-account"></a>Inicio del servicio de ejecución local y envío del trabajo de U-SQL a la cuenta local 
Para los usuarios nuevos, use **ADL: Download Local Run Dependency** para descargar los paquetes de ejecución locales, si no ha [configurado el entorno de ejecución local de U-SQL](#set-up-the-u-sql-local-run-environment).

1. Seleccione Ctrl+Mayús+P para abrir la paleta de comandos y, luego, escribir **ADL: Start Local Run Service**.   
2. Seleccione **Aceptar** para aceptar los Términos de licencia del software de Microsoft por primera vez. 

   ![Aceptación de los Términos de licencia del software de Microsoft](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/AcceptEULA.png)   
3. Se abre la consola cmd. Para los usuarios nuevos, tiene que especificar **3** y asignar la ruta de acceso a la carpeta local para la entrada y salida de datos. Para otras opciones, puede usar los valores predeterminados. 

   ![Cmd de ejecución local de Data Lake Tools para Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-cmd.png)
4. Seleccione Ctrl+Mayús+P para abrir la paleta de comandos, escriba **ADL: Submit Job** y, luego, seleccione **Local** para enviar el trabajo a la cuenta local.

   ![Selección local de Herramientas de Data Lake para Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-select-local.png)
5. Una vez que envía el trabajo, puede ver los detalles del envío. Para hacerlo, seleccione **jobUrl** en la ventana **Salida**. También puede ver el estado de envío del trabajo en la consola cmd. Escriba **7** en la consola cmd si desea más información sobre los detalles del trabajo.

   ![Salida d ejecución local de Herramientas de Data Lake para Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-result.png)
   ![Herramientas de Data Lake para el estado de cmd de ejecución local de Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-localrun-cmd-status.png) 


## <a name="start-a-local-debug-for-the-u-sql-job"></a>Inicio de una depuración local para el trabajo de U-SQL  
Para los usuarios nuevos:

1. Use **ADL: Download Local Run Dependency** para descargar los paquetes de ejecución locales, si no ha [configurado el entorno de ejecución local de U-SQL](#set-up-the-u-sql-local-run-environment).
2. Instale .NET Core SDK 2.0 como se sugiere en el cuadro de mensaje, si no está instalado.
 
  ![recordatorio de instalación de Dotnet](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/remind-install-dotnet.png)
3. Instale C# para Visual Studio Code como se sugiere en el cuadro de mensaje, si no está instalado. Haga clic en **Instalar** para continuar y después reinicie VSCode.

    ![Recordatorio de instalación de C#](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/install-csharp.png)

Siga estos pasos para realizar la depuración local:
  
1. Seleccione Ctrl+Mayús+P para abrir la paleta de comandos y, luego, escribir **ADL: Start Local Run Service**. Se abre la consola cmd. Asegúrese de que se establece **DataRoot**.
2. Establezca un punto de interrupción en el código subyacente de C#.
3. Vuelva al editor de scripts, haga clic con el botón derecho y seleccione **ADL: Local Debug**.
    
   ![Resultado de la depuración local de Herramientas de Data Lake para Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-debug-result.png)


## <a name="next-steps"></a>Pasos siguientes
- Para usar Herramientas de Azure Data Lake para Visual Studio, consulte [Use the Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md) (Uso de Herramientas de Azure Data Lake para Visual Studio Code).
- Para información detallada sobre cómo empezar a trabajar con Data Lake Analytics, consulte [Tutorial: Introducción a Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).
- Para información sobre Herramientas de Data Lake para Visual Studio, consulte [Tutorial: Desarrollo de scripts U-SQL mediante Herramientas de Data Lake para Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Para obtener información sobre el desarrollo de ensamblados, consulte [Desarrollo de ensamblados U-SQL para trabajos de Azure Data Lake Analytics](data-lake-analytics-u-sql-develop-assemblies.md).
