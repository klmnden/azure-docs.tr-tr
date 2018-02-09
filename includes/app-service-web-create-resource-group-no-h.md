Cloud Shell içinde [`az group create`](/cli/azure/group?view=azure-cli-latest#az_group_create) komutuyla bir kaynak grubu oluşturun.

[!INCLUDE [resource group intro text](resource-group.md)]

Aşağıdaki örnek *Batı Avrupa* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur. App Service için desteklenen tüm konumları görüntülemek için `az appservice list-locations` komutunu çalıştırın.

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Genellikle kaynak grubunuzu ve kaynakları kendinize yakın bir bölgede oluşturursunuz. 
