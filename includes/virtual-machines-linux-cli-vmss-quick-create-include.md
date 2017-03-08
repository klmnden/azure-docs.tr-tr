## <a name="prerequisites"></a>Ön koşullar

Henüz yapmadıysanız [Azure aboneliği ücretsiz deneme sürümüne](https://azure.microsoft.com/pricing/free-trial/) kaydolun ve [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2)'ı yükleyin.

## <a name="create-the-scale-set"></a>Ölçek kümesini oluşturma

İlk olarak ölçek kümesini dağıtacağınız kaynak grubunu oluşturun:

```azurecli
az group create --location westus --name myResourceGroup
```

Şimdi `az vmss create` komutunu kullanarak ölçek kümenizi oluşturabilirsiniz. Aşağıdaki örnek, `myrg` adlı kaynak grubunun içinde `myvmss` adlı Linux ölçek kümesi oluşturur:

```azurecli
az vmss create --resource-group myResourceGroup --name myVmss \
    --image UbuntuLTS --admin-username azureuser \
    --authentication-type password --admin-password P4$$w0rd
```

Aşağıdaki örnek aynı yapılandırmayla bir Windows ölçek kümesi oluşturur:

```azurecli
az vmss create --resource-group myResourceGroup --name myVmss \
    --image Win2016Datacenter --admin-username azureuser \
    --authentication-type password --admin-password P4$$w0rd
```

Farklı bir işletim sistemi görüntüsü seçmek isterseniz `az vm image list` veya `az vm image list --all` komutuyla uygun görüntüleri görebilirsiniz. Ölçek kümesindeki VM'lerin bağlantı bilgilerini görmek için `az vmss list_instance_connection_info` komutunu kullanın:

```azurecli
az vmss list_instance_connection_info --resource-group myResourceGroup --name myVmss
```