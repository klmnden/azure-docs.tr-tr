## <a name="create-an-azure-storage-account"></a>Azure Depolama hesabı oluşturma

İşlevler durumu ve işlevlerinizi ilgili diğer bilgileri korumak için Azure depolama alanında genel amaçlı bir hesabı kullanır. Genel amaçlı depolama hesabı kullanılarak oluşturulan kaynak grubunda oluşturma [az depolama hesabı oluşturma](/cli/azure/storage/account#create) komutu.

Aşağıdaki komutta, gördüğünüz bir genel benzersiz depolama hesabı adı yerine `<storage_name>` yer tutucu. Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir.

```azurecli-interactive
az storage account create --name <storage_name> --location westeurope --resource-group myResourceGroup --sku Standard_LRS
```

Depolama hesabı oluşturulduktan sonra Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

```json
{
  "creationTime": "2017-04-15T17:14:39.320307+00:00",
  "id": "/subscriptions/bbbef702-e769-477b-9f16-bc4d3aa97387/resourceGroups/myresourcegroup/...",
  "kind": "Storage",
  "location": "westeurope",
  "name": "myfunctionappstorage",
  "primaryEndpoints": {
    "blob": "https://myfunctionappstorage.blob.core.windows.net/",
    "file": "https://myfunctionappstorage.file.core.windows.net/",
    "queue": "https://myfunctionappstorage.queue.core.windows.net/",
    "table": "https://myfunctionappstorage.table.core.windows.net/"
  },
     ....
    // Remaining output has been truncated for readability.
}
```