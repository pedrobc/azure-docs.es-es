En Cloud Shell, cree una [aplicación web](../articles/app-service/app-service-web-overview.md) en el plan de App Service `myAppServicePlan` con el comando [az webapp create](/cli/azure/webapp#create). 

La aplicación web ofrece un espacio de hospedaje para el código y proporciona una dirección URL para que pueda ver la aplicación implementada.

En el siguiente comando, reemplace  *\<app_name >* por un nombre único (los caracteres válidos son `a-z`, `0-9` y `-`). Si `<app_name>` no es único, obtendrá el mensaje de error "Ya existe un sitio web con el nombre especificado <app_name>". La dirección URL predeterminada de la aplicación web es `https://<app_name>.azurewebsites.net`. 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan --deployment-local-git
```

Cuando se ha creado la aplicación web, la CLI de Azure muestra información similar a la del ejemplo siguiente:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "<app_name>.azurewebsites.net",
    "<app_name>.scm.azurewebsites.net"
  ],
  "gatewaySiteName": null,
  "hostNameSslStates": [
    {
      "hostType": "Standard",
      "name": "<app_name>.azurewebsites.net",
      "sslState": "Disabled",
      "thumbprint": null,
      "toUpdate": null,
      "virtualIp": null
    }
    < JSON data removed for brevity. >
}
```

Vaya al sitio para ver la aplicación web recién creada.

```bash
http://<app_name>.azurewebsites.net
```
