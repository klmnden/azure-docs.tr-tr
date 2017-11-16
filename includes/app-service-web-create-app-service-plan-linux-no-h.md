Cloud Shell içinde [az appservice plan create](/cli/azure/appservice/plan#create) komutu ile bir App Service planı oluşturun.

[!INCLUDE [app-service-plan](app-service-plan-linux.md)]

Aşağıdaki örnek, **Standart** fiyatlandırma katmanını kullanarak bir Linux kapsayıcısı içinde `myAppServicePlan` adlı bir App Service planı oluşturur:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku S1 --is-linux
```

App Service planı oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "West Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "linux",
  "location": "West Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  < JSON data removed for brevity. >
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
} 
```