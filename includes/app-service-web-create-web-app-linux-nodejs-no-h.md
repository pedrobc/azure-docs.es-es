En Cloud Shell, cree una [aplicación web](../articles/app-service/containers/app-service-linux-intro.md) en el plan de App Service `myAppServicePlan` con el comando [az webapp create](/cli/azure/webapp#create). No olvide reemplazar `<app name>` por un nombre de aplicación único.

En el siguiente comando, el entorno de tiempo de ejecución se establece en `NODE|8.1`. Para ver todos los entornos de tiempo de ejecución admitidos, ejecute [az webapp list-runtimes](/cli/azure/webapp#list-runtimes). 

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --runtime "NODE|8.1" --deployment-local-git
```

Cuando se haya creado la aplicación web, la CLI de Azure mostrará información similar a la del ejemplo siguiente:

```json
Local git is configured with url of 'https://<username>@<app name>.scm.azurewebsites.net/<app name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app name>.scm.azurewebsites.net/<app name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

Ha creado una nueva aplicación web vacía en un contenedor de Linux, con la implementación de Git habilitada.

> [!NOTE]
> La dirección URL del repositorio de Git remoto se muestra en la propiedad `deploymentLocalGitUrl`, con el formato `https://<username>@<app name>.scm.azurewebsites.net/<app name>.git`. Guarde esta dirección URL porque la necesitará más adelante.
>
