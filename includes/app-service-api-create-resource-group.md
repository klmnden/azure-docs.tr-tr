[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun.

[!INCLUDE [resource group intro text](resource-group.md)]

Aşağıdaki örnek *westeurope* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

Seçebileceğiniz konumları görmek için `az appservice list-locations` komutunu çalıştırın. Genellikle kaynakları kendinize yakın bir bölgede oluşturmanız önerilir.
