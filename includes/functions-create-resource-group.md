## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#az_group_create) ile bir kaynak grubu oluşturun. Azure kaynak grubu; işlev uygulamaları, veritabanları ve depolama hesapları gibi Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek `myResourceGroup` adlı bir kaynak grubu oluşturur.  
Cloud Shell kullanmıyorsanız önce `az login` kullanarak oturum açın.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```
Genellikle kaynak grubunuzu ve kaynakları kendinize yakın bir bölgede oluşturursunuz. App Service planları için desteklenen tüm konumları görüntülemek için [az appservice list-locations](/cli/azure/appservice#az_appservice_list_locations) komutunu çalıştırın.