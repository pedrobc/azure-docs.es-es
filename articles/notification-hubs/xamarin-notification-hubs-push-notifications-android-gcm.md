---
title: "Introducción a los Notification Hubs para las aplicaciones Xamarin.Android | Microsoft Docs"
description: "En este tutorial, obtenga información acerca de cómo usar Centros de notificaciones de Azure para enviar notificaciones push a una aplicación Xamarin Android."
author: ysxu
manager: erikre
editor: 
services: notification-hubs
documentationcenter: xamarin
ms.assetid: 0be600fe-d5f3-43a5-9e5e-3135c9743e54
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 09/25/2017
ms.author: yuaxu
ms.openlocfilehash: f9fef96b71e0db7b15ff5208e9bd1a0b4ecf7211
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-notification-hubs-with-xamarin-for-android"></a>Introducción a Centros de notificaciones con Xamarin para Android
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Información general
Este tutorial muestra cómo puede usar Centros de notificaciones de Azure para enviar notificaciones push a una aplicación Xamarin.Android. Creará una aplicación Xamarin.Android en blanco que reciba notificaciones push mediante el servicio de mensajería en la nube de Google (GCM). Cuando haya finalizado, podrá usar el centro de notificaciones para difundir notificaciones push a todos los dispositivos que ejecutan su aplicación. El código acabado está disponible en el ejemplo de la [aplicación NotificationHubs][GitHub].

En este tutorial se demuestra el escenario de difusión sencillo con centros de notificaciones.

## <a name="before-you-begin"></a>Antes de empezar
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

El código completo de este tutorial se puede encontrar en GitHub [aquí](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid).

## <a name="prerequisites"></a>Requisitos previos
Este tutorial requiere lo siguiente:

* Las instrucciones de instalación completas de Visual Studio con Xamarin en Windows o Xamarin Studio en Mac OS X se encuentran en [Configuración e instalación](https://msdn.microsoft.com/library/mt613162.aspx).
* Cuenta de Google activa
* [Componente de mensajería de Azure]
* [Componente del Cliente de mensajería en la nube de Google]

Completar este tutorial es un requisito previo para todos los demás tutoriales de Centros de notificaciones para aplicaciones Xamarin.Android.

> [!IMPORTANT]
> Para completar este tutorial, deberá tener una cuenta de Azure activa. En caso de no tener ninguna, puede crear una cuenta de evaluación gratuita en tan solo unos minutos. Para obtener más información, consulte [Evaluación gratuita de Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).
> 
> 

## <a name="enable-google-cloud-messaging"></a>Habilitación del servicio de mensajería en la nube de Google
[!INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

## <a name="configure-your-notification-hub"></a>Configuración de su Centro de notificaciones
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="5">

<li><p>Haga clic en la pestaña <b>Configurar</b> en la parte superior, escriba el valor <b>Clave de API</b> que obtuvo en la sección anterior y, a continuación, seleccione <b>Guardar</b>.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)

El centro de notificaciones ya está configurado para funcionar con GCM y dispone de las cadenas de conexión para registrar la aplicación para que reciba notificaciones y para enviar notificaciones de inserción.

## <a name="connect-your-app-to-the-notification-hub"></a>Conexión de la aplicación al Centro de notificaciones
En primer lugar, cree un nuevo proyecto.
1. En Xamarin Studio, elija **Nueva solución** > **Aplicación Android** y, a continuación, seleccione **Siguiente**.
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png)

2. Escriba su **Nombre de la aplicación** e **Identificador**. Elija la **plataforma de destino** que quiere admitir y luego elija **Siguiente** y **Crear**.
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png)

    Esto crea un nuevo proyecto de Android.

3. Abra las propiedades del proyecto. Para ello, haga clic con el botón derecho en el nuevo proyecto en la vista Solución y elija **Opciones**. Seleccione el elemento **Android Application** (Aplicación Android) en la sección **Build** (Compilación).
   
    Asegúrese de que la primera letra de **Package name** (Nombre del paquete) sea minúscula.
   
   > [!IMPORTANT]
   > La primera letra del nombre del paquete debe ser minúscula. De lo contrario, recibirá errores de manifiesto de la aplicación al registrar **BroadcastReceiver** y **IntentFilter** para las notificaciones push siguientes.
   > 
   > 
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub--xamarin-android-app-options.png)
4. De forma opcional, establezca el valor de **Minimum Android version** (Versión mínima de Android) en otro nivel de API.
5. Establezca **Versión Android de destino** en la versión de API a la que desea dirigirse (debe ser el nivel 8 de API o superior).
6. Haga clic en **Aceptar** y cierre el cuadro de diálogo Opciones del proyecto.

### <a name="add-the-required-components-to-your-project"></a>Incorporación de los componentes necesarios al proyecto
El Cliente de mensajería en la nube de Google disponible en el Almacén de componentes Xamarin simplifica el proceso de compatibilidad con las notificaciones de inserción en Xamarin.Android.

1. En la aplicación Xamarin.Android, haga clic con el botón derecho en la carpeta Components (Componentes) y elija **Get More Components**(Obtener más componentes).
2. Busque el componente **Azure Messaging** (Mensajería de Azure) y agréguelo al proyecto.
3. Busque el componente **Google Cloud Messaging Client** (Cliente de mensajería en la nube de Google) y agréguelo al proyecto.

### <a name="set-up-notification-hubs-in-your-project"></a>Configuración de los centros de notificaciones en su proyecto
1. Recopile la siguiente información para el centro de notificaciones y la aplicación Android:
   
   * **GoogleProjectNumber**: obtenga este valor de número de proyecto de la información general de la aplicación en el Portal para desarrolladores de Google. Ha tomado nota de este valor anteriormente al crear la aplicación en el portal.
   * **Cadena de conexión de escucha**: en el panel del [Portal de Azure clásico], haga clic en **Ver cadenas de conexión**. Copie la cadena de conexión *DefaultListenSharedAccessSignature* para este valor.
   * **Nombre del centro**: este es el nombre del centro en el [Portal de Azure clásico]. Por ejemplo, *mynotificationhub2*.
     
2. Cree una clase **Constants.cs** para el proyecto Xamarin y defina los siguientes valores de constante en la clase. Reemplace los marcadores de posición por sus valores.
     
        public static class Constants
        {
     
           public const string SenderID = "<GoogleProjectNumber>"; // Google API Project Number
           public const string ListenConnectionString = "<Listen connection string>";
           public const string NotificationHubName = "<hub name>";
        }
3. Agregue las siguientes instrucciones a **MainActivity.cs**:
   
        using Android.Util;
        using Gcm.Client;
4. Agregue una variable de instancia a la clase `MainActivity` que se usará para mostrar un cuadro de diálogo de alerta cuando se ejecuta la aplicación:
   
        public static MainActivity instance;
5. Cree el método siguiente en la clase **MainActivity** :
   
        private void RegisterWithGCM()
        {
            // Check to ensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);
   
            // Register for push notifications
            Log.Info("MainActivity", "Registering...");
            GcmClient.Register(this, Constants.SenderID);
        }
6. En el método `OnCreate` de **MainActivity.cs**, inicialice la variable `instance` y agregue una llamada a `RegisterWithGCM`:
   
        protected override void OnCreate (Bundle bundle)
        {
            instance = this;
   
            base.OnCreate (bundle);
   
            // Set your view from the "main" layout resource
            SetContentView (Resource.Layout.Main);
   
            // Get your button from the layout resource,
            // and attach an event to it
            Button button = FindViewById<Button> (Resource.Id.myButton);
   
            RegisterWithGCM();
        }
7. Cree una nueva clase, **MyBroadcastReceiver**.
   
   > [!NOTE]
   > Más adelante, se explicará cómo crear una clase **BroadcastReceiver** desde el principio. Sin embargo, una alternativa rápida a la creación manual de **MyBroadcastReceiver.cs** es hacer referencia al archivo **GcmService.cs** que se encuentra en el proyecto Xamarin.Android de ejemplo incluido con los [ejemplos de NotificationHubs][GitHub]. Duplicar **GcmService.cs** y cambiar los nombres de las clases también puede ser un buen punto de partida.
   > 
   > 
8. Agregue las siguientes instrucciones using a **MyBroadcastReceiver.cs** (en referencia al componente y al ensamblado que se agregaron anteriormente):
   
        using System.Collections.Generic;
        using System.Text;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Gcm.Client;
        using WindowsAzure.Messaging;
9. En **MyBroadcastReceiver.cs** agregue las siguientes solicitudes de permiso entre las instrucciones **using** y la declaración **namespace**:
   
        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
   
        //GET_ACCOUNTS is needed only for Android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
10. En **MyBroadcastReceiver.cs**, cambie la clase **MyBroadcastReceiver** para que coincida con lo siguiente:
   
        [BroadcastReceiver(Permission=Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        public class MyBroadcastReceiver : GcmBroadcastReceiverBase<PushHandlerService>
        {
            public static string[] SENDER_IDS = new string[] { Constants.SenderID };
   
            public const string TAG = "MyBroadcastReceiver-GCM";
        }
11. Agregue otra clase en **MyBroadcastReceiver.cs** llamada **PushHandlerService** que derive de **GcmServiceBase**. Asegúrese de aplicar el atributo **Service** a la clase:
    
         [Service] // Must use the service tag
         public class PushHandlerService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }
             private NotificationHub Hub { get; set; }
    
             public PushHandlerService() : base(Constants.SenderID)
                {
                 Log.Info(MyBroadcastReceiver.TAG, "PushHandlerService() constructor");
             }
         }
12. **GcmServiceBase** implements methods **OnRegistered()**, **OnUnRegistered()**, **OnMessage()**, **OnRecoverableError()** y **OnError()**. Nuestra clase de implementación **PushHandlerService** debe reemplazar estos métodos, los cuales se activarán en respuesta a la interacción con el centro de notificaciones.
13. Reemplace el método **OnRegistered()** en **PushHandlerService** mediante el código siguiente:
    
         protected override void OnRegistered(Context context, string registrationId)
         {
             Log.Verbose(MyBroadcastReceiver.TAG, "GCM Registered: " + registrationId);
             RegistrationID = registrationId;
    
             createNotification("PushHandlerService-GCM Registered...",
                                 "The device has been Registered!");
    
             Hub = new NotificationHub(Constants.NotificationHubName, Constants.ListenConnectionString,
                                         context);
             try
             {
                 Hub.UnregisterAll(registrationId);
             }
             catch (Exception ex)
             {
                 Log.Error(MyBroadcastReceiver.TAG, ex.Message);
             }
    
             //var tags = new List<string>() { "falcons" }; // create tags if you want
             var tags = new List<string>() {};
    
             try
             {
                 var hubRegistration = Hub.Register(registrationId, tags.ToArray());
             }
             catch (Exception ex)
             {
                 Log.Error(MyBroadcastReceiver.TAG, ex.Message);
             }
         }
    
    > [!NOTE]
    > En el código **OnRegistered()** anterior, tenga en cuenta la posibilidad de especificar etiquetas para registrar canales de mensajería específicos.
    > 
    > 
14. Reemplace el método **OnMessage** en **PushHandlerService** mediante el código siguiente:
    
        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info(MyBroadcastReceiver.TAG, "GCM Message Received!");
    
            var msg = new StringBuilder();
    
            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }
    
            string messageText = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty (messageText))
            {
                createNotification ("New hub message!", messageText);
            }
            else
            {
                createNotification ("Unknown message details", msg.ToString ());
            }
        }
15. Agregue los métodos **createNotification** y **dialogNotify** siguientes para que **PushHandlerService** notifique a los usuarios cuando se recibe una notificación.
    
    > [!NOTE]
    > El diseño de notificaciones en Android versión 5.0 y posteriores representa un cambio significativo respecto al de las versiones anteriores. Si la prueba en Android 5.0 o posterior, la aplicación deberá ejecutarse para recibir la notificación. Para más información, consulte [Notificaciones de Android](http://go.microsoft.com/fwlink/?LinkId=615880).
    > 
    > 
    
        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;
    
            //Create an intent to show UI
            var uiIntent = new Intent(this, typeof(MainActivity));
    
            //Create the notification
            var notification = new Notification(Android.Resource.Drawable.SymActionEmail, title);
    
            //Auto-cancel will remove the notification once the user touches it
            notification.Flags = NotificationFlags.AutoCancel;
    
            //Set the notification info
            //we use the pending intent, passing our ui intent over, which will get called
            //when the notification is tapped.
            notification.SetLatestEventInfo(this, title, desc, PendingIntent.GetActivity(this, 0, uiIntent, 0));
    
            //Show the notification
            notificationManager.Notify(1, notification);
            dialogNotify (title, desc);
        }
    
        protected void dialogNotify(String title, String message)
        {
    
            MainActivity.instance.RunOnUiThread(() => {
                AlertDialog.Builder dlg = new AlertDialog.Builder(MainActivity.instance);
                AlertDialog alert = dlg.Create();
                alert.SetTitle(title);
                alert.SetButton("Ok", delegate {
                    alert.Dismiss();
                });
                alert.SetMessage(message);
                alert.Show();
            });
        }
16. Reemplace los miembros abstractos **OnUnRegistered()**, **OnRecoverableError()** y **OnError()** para que el código se compile:
    
        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Unregistered: " + registrationId);
    
            createNotification("GCM Unregistered...", "The device has been unregistered!");
        }
    
        protected override bool OnRecoverableError(Context context, string errorId)
        {
            Log.Warn(MyBroadcastReceiver.TAG, "Recoverable Error: " + errorId);
    
            return base.OnRecoverableError (context, errorId);
        }
    
        protected override void OnError(Context context, string errorId)
        {
            Log.Error(MyBroadcastReceiver.TAG, "GCM Error: " + errorId);
        }

## <a name="run-your-app-in-the-emulator"></a>Ejecución de su aplicación en el emulador
Si ejecuta esta aplicación en el emulador, asegúrese de utilizar un dispositivo virtual Android (AVD) compatible con las API de Google.

> [!IMPORTANT]
> Para recibir notificaciones push, debe configurar una cuenta de Google en su dispositivo virtual Android. (En el emulador, navegue a **Configuración** y haga clic en **Agregar cuenta**). Además, asegúrese de que el emulador esté conectado a Internet.
> 
> [!NOTE]
> El diseño de notificaciones en Android versión 5.0 y posteriores representa un cambio significativo respecto al de las versiones anteriores. Para más información, consulte [Notificaciones de Android](http://go.microsoft.com/fwlink/?LinkId=615880).
> 
> 

1. En **Tools** (Herramientas), elija **Open Android Emulator Manager** (Abrir Administrador de emulador Android), seleccione su dispositivo y, a continuación, elija **Edit** (Editar).
   
      ![][18]
2. Seleccione **API de Google** en **Destino** y, luego, elija **Aceptar**.
   
      ![][19]
3. En la barra de herramientas de la parte superior, haga clic en **Run**(Ejecutar) y, a continuación, seleccione la aplicación. Esto inicia el emulador y ejecuta la aplicación.
   
   La aplicación recupera el valor de *registrationId* de GCM y se registra con el Centro de notificaciones.

## <a name="send-notifications-from-your-backend"></a>Envío de notificaciones desde su backend
Para probar la recepción de notificaciones en su aplicación, envíe notificaciones en el [Portal de Azure clásico] mediante la pestaña de depuración en el centro de notificaciones, tal como se muestra en la pantalla que aparece a continuación.

![][30]

Las notificaciones push se envían normalmente en un servicio back-end como Servicios móviles o ASP.NET mediante una biblioteca compatible. También puede usar la API de REST directamente para enviar mensajes de notificación si no hay disponible una biblioteca para su back-end.

A continuación, presentamos una lista de algunos otros tutoriales que podría interesarle revisar para enviar notificaciones:

* ASP.NET: consulte [Uso de los Centros de notificaciones para insertar notificaciones para los usuarios].
* SDK de Java para Centros de notificaciones de Azure: consulte [Uso de los Centros de notificaciones desde Java](notification-hubs-java-push-notification-tutorial.md) para enviar notificaciones desde Java. Esto se probó en Eclipse para el desarrollo de Android.
* PHP: consulte [Uso de los Centros de notificaciones desde PHP](notification-hubs-php-push-notification-tutorial.md).

En las subsecciones siguientes de este tutorial, podrá enviar notificaciones con una aplicación de consola .NET y con un servicio móvil mediante un script de Node.

#### <a name="optional-send-notifications-by-using-a-net-app"></a>Envío de notificaciones mediante una aplicación .NET (opcional)
En esta sección enviará notificaciones con una aplicación de consola .NET.

1. Cree una aplicación de consola nueva de Visual C#:
   
      ![][20]
2. En Visual Studio, seleccione **Herramientas** > **Administrador de paquetes NuGet** y **Consola del Administrador de paquetes**.
   
    Esto muestra la Consola del administrador de paquetes en Visual Studio.
3. En la ventana de la Consola del Administrador de paquetes, seleccione en **Proyecto predeterminado** el nuevo proyecto de aplicación de consola y, a continuación, ejecute el siguiente comando en la ventana de la consola:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Así se agrega una referencia al SDK de Azure Notification Hubs mediante el <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">paquete NuGet Microsoft.Azure.Notification Hubs</a>.
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. Abra el archivo Program.cs y agregue la siguiente instrucción `using` :
   
        using Microsoft.Azure.NotificationHubs;
5. En la clase `Program`, agregue el método siguiente: Actualice el texto del marcador de posición con la cadena de conexión *DefaultFullSharedAccessSignature* y el nombre del centro en el [Portal de Azure clásico].
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            await hub.SendGcmNativeNotificationAsync("{ \"data\" : {\"message\":\"Hello from Azure!\"}}");
        }
6. Agregue las siguientes líneas al método **Main** :
   
         SendNotificationAsync();
         Console.ReadLine();
7. Presione la tecla F5 para ejecutar la aplicación. Debería recibir una notificación en la aplicación.
   
      ![][21]

#### <a name="optional-send-notifications-by-using-a-mobile-service"></a>Envío de notificaciones mediante un servicio móvil (opcional)
1. Siga [Introducción a Servicios móviles].
2. Inicie sesión en el [Portal de Azure clásico]y seleccione su servicio móvil.
3. Seleccione la pestaña **Programador** en la parte superior.
   
      ![][22]
4. Cree un nuevo trabajo programado, inserte un nombre y seleccione **A petición**.
   
      ![][23]
5. Cuando se cree el trabajo, seleccione el nombre del trabajo. Luego, elija la pestaña **Script** de la barra superior.
6. Inserte el siguiente script en su función de programador. Asegúrese de reemplazar los marcadores de posición por su nombre del centro de notificaciones y la cadena de conexión para *DefaultFullSharedAccessSignature* que obtuvo anteriormente. Elija **Guardar**.
   
        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string>');
        notificationHubService.gcm.send(null,'{"data":{"message" : "Hello from Mobile Services!"}}',
          function (error)
          {
            if (!error) {
               console.warn("Notification successful");
            }
            else
            {
              console.warn("Notification failed" + error);
            }
          }
        );
7. Elija **Ejecutar una vez** en la barra inferior. Debería recibir una notificación del sistema.

## <a name="next-steps"></a>Pasos siguientes
En este sencillo ejemplo, difunde notificaciones a todos sus dispositivos Android. Para dirigirse a usuarios específicos, consulte el tutorial [Uso de los Centros de notificaciones para insertar notificaciones para los usuarios]. Si desea segmentar a sus usuarios por grupos de interés, puede leer [Uso de Centros de notificaciones para enviar noticias de último minuto]. Para más información acerca del uso de Notification Hubs, consulte [Introducción a los Notification Hubs] y [Procedimiento: Azure Notification Hubs (aplicaciones Android)].

<!-- Anchors. -->
[Enable Google Cloud Messaging]: #register
[Configure your Notification Hub]: #configure-hub
[Connecting your app to the Notification Hub]: #connecting-app
[Run your app with the emulator]: #run-app
[Send notifications from your back-end]: #send
[Next steps]:#next-steps

<!-- Images. -->

[11]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-configure-android.png

[13]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app1.png
[15]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app3.png

[18]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app7.png
[19]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app8.png

[20]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-android-toast.png
[22]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler2.png

[30]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Introducción a Servicios móviles]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[Portal de Azure clásico]: https://manage.windowsazure.com/
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Introducción a los Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[Procedimiento: Azure Notification Hubs (aplicaciones Android)]: http://msdn.microsoft.com/library/dn282661.aspx

[Uso de los Centros de notificaciones para insertar notificaciones para los usuarios]: /manage/services/notification-hubs/notify-users-aspnet
[Uso de Centros de notificaciones para enviar noticias de último minuto]: /manage/services/notification-hubs/breaking-news-dotnet
[GCMClient Component page]: http://components.xamarin.com/view/GCMClient
[Xamarin.NotificationHub GitHub page]: https://github.com/SaschaDittmann/Xamarin.NotificationHub
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Componente del Cliente de mensajería en la nube de Google]: http://components.xamarin.com/view/GCMClient/
[Componente de mensajería de Azure]: http://components.xamarin.com/view/azure-messaging
