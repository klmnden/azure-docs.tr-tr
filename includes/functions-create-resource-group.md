## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#az_group_create) ile bir kaynak grubu oluşturun. Azure kaynak grubu; işlev uygulamaları, veritabanları ve depolama hesapları gibi Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek `myResourceGroup` adlı bir kaynak grubu oluşturur.  
Bulut Kabuk kullanmıyorsanız, ilk oturum `az login`.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```
Genellikle kaynak grubunuzu ve kaynakları kendinize yakın bir bölgede oluşturursunuz. Uygulama hizmeti planları için desteklenen tüm konumları görmek için Çalıştır [az appservice listesi-konumları](/cli/azure/appservice#az_appservice_list_locations) komutu.