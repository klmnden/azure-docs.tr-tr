Cloud Shell içinde [az group create](/cli/azure/group#az_group_create) komutuyla bir kaynak grubu oluşturun.

[!INCLUDE [resource group intro text](resource-group.md)]

Aşağıdaki örnek *Batı Avrupa* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur. Uygulama hizmeti için desteklenen tüm konumları görmek için Çalıştır `az appservice list-locations` komutu.

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Genellikle kaynak grubunuzu ve kaynakları kendinize yakın bir bölgede oluşturursunuz. 
