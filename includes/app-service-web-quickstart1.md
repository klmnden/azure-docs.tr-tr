## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

 [Azure kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups), web uygulamaları ve veritabanları gibi kaynakların yönetildiği bir mantıksal kapsayıcıdır. Bir kaynak grubu içindeki kaynaklar için dağıtma, güncelleştirme ve silme işlemlerini gerçekleştirebilirsiniz.

[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun.

```azurecli
az group create --name myResourceGroup --location westeurope
```

Seçebileceğiniz konumları görmek için `az appservice list-locations` komutunu kullanın. Genellikle kaynakları kendinize yakın bir bölgede oluşturmanız önerilir.

## <a name="create-an-azure-app-service-plan"></a>Azure App Service planı oluşturma

[az appservice plan create](/cli/azure/appservice/plan#create) komutuyla "ÜCRETSİZ" [App Service planı](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) oluşturun.

[!INCLUDE [app-service-plan](app-service-plan.md)]

Aşağıdaki komut, **Ücretsiz** fiyatlandırma katmanını kullanarak `quickStartPlan` adlı bir App Service planı oluşturur.

```azurecli
az appservice plan create --name quickStartPlan --resource-group myResourceGroup --sku FREE
```

App Service planı oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "quickStartPlan",
  "geoRegion": "West Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/quickStartPlan",
  "kind": "app",
  "location": "West Europe",
  "maximumNumberOfWorkers": 1,
  "name": "quickStartPlan",
  < JSON data removed for brevity. >
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
} 
```

## <a name="create-a-web-app"></a>Web Uygulaması oluşturma