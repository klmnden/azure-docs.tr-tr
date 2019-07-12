---
title: Azure Container ınstances'da bir Azure dosya birimi bağlama
description: Azure Container Instances ile durum kalıcı hale getirmek için bir Azure dosya birimi bağlama hakkında bilgi edinin
services: container-instances
author: dlepow
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 07/08/2019
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: bc09aa500743d608c0a3a7a379fe9584c9c55e9b
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657634"
---
# <a name="mount-an-azure-file-share-in-azure-container-instances"></a>Azure Container ınstances'da bir Azure dosya paylaşımını bağlama

Varsayılan olarak, Azure Container Instances, durum bilgisi bulunmaz. Kapsayıcı kilitleniyor veya durdurur, durumuna kaybolur. Kapsayıcı ömür ötesinde durumunu kalıcı hale getirmek için bir dış depodan bir birimi bağlamak gerekir. Bu makalede ile oluşturulmuş bir Azure dosya paylaşımını bağlama işlemi gösterilmektedir [Azure dosyaları](../storage/files/storage-files-introduction.md) Azure Container Instances ile kullanım için. Azure Dosyaları bulutta tamamen yönetilen dosya paylaşımları sunar. Bu dosyalara sektör standardı olan Sunucu İleti Bloğu (SMB) protokolü aracılığıyla erişilebilir. Azure Container Instances ile bir Azure dosya paylaşımı kullanarak bir Azure dosya paylaşımı ile Azure sanal makinelerini kullanmaya benzer dosya paylaşım özellikleri sağlar.

> [!NOTE]
> Azure dosyaları paylaşımı bağlayarak, Linux kapsayıcıları için şu anda sınırlıdır. Tüm özellikleri Windows kapsayıcılarına getirmek için çalışıyoruz, ancak geçerli platform farklılıklarını içinde bulabilirsiniz [genel bakış](container-instances-overview.md#linux-and-windows-containers).

## <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma

Azure Container Instances ile bir Azure dosya paylaşımı kullanmadan önce oluşturmanız gerekir. Dosya Paylaşımı ve Paylaşım barındırmak için bir depolama hesabı oluşturmak için aşağıdaki betiği çalıştırın. Betik rastgele bir değeri temel dizesi olarak ekler. Bu nedenle, depolama hesabı adı genel olarak benzersiz olmalıdır.

```azurecli-interactive
# Change these four parameters as needed
ACI_PERS_RESOURCE_GROUP=myResourceGroup
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
ACI_PERS_LOCATION=eastus
ACI_PERS_SHARE_NAME=acishare

# Create the storage account with the parameters
az storage account create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --location $ACI_PERS_LOCATION \
    --sku Standard_LRS

# Create the file share
az storage share create --name $ACI_PERS_SHARE_NAME --account-name $ACI_PERS_STORAGE_ACCOUNT_NAME
```

## <a name="get-storage-credentials"></a>Depolama kimlik bilgilerini alma

Azure Container ınstances'da bir birimi olarak Azure dosya paylaşımını bağlayabilmeniz için üç değer gerekir: depolama hesabı adı, paylaşım adı ve depolama erişim anahtarı.

Yukarıdaki betik kullandıysanız, depolama hesabı adı $ACI_PERS_STORAGE_ACCOUNT_NAME değişkeninde depolanan. Hesap adı görmek için şunu yazın:

```console
echo $ACI_PERS_STORAGE_ACCOUNT_NAME
```

Paylaşım adı zaten bilinen (olarak tanımlanan *acishare* yukarıdaki komut dosyasında), bu nedenle tüm kalır olduğu aşağıdaki komutu kullanarak bulunabilir depolama hesabı anahtarı:

```azurecli-interactive
STORAGE_KEY=$(az storage account keys list --resource-group $ACI_PERS_RESOURCE_GROUP --account-name $ACI_PERS_STORAGE_ACCOUNT_NAME --query "[0].value" --output tsv)
echo $STORAGE_KEY
```

## <a name="deploy-container-and-mount-volume---cli"></a>Kapsayıcı dağıtın ve birimi - CLI

Azure CLI kullanarak bir kapsayıcıdaki bir birim olarak Azure dosya paylaşımını bağlayabilmeniz için paylaşım ve birim bağlama noktası kapsayıcı ile oluşturduğunuzda belirtin [az kapsayıcı oluşturma][az-container-create]. Önceki adımları izlediyseniz bir kapsayıcı oluşturmak için aşağıdaki komutu kullanarak daha önce oluşturduğunuz paylaşımı bağlayabilir:

```azurecli-interactive
az container create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name hellofiles \
    --image mcr.microsoft.com/azuredocs/aci-hellofiles \
    --dns-name-label aci-demo \
    --ports 80 \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name $ACI_PERS_SHARE_NAME \
    --azure-file-volume-mount-path /aci/logs/
```

`--dns-name-label` Değer kapsayıcı örneğini oluşturduğunuz Azure bölgesi içinde benzersiz olmalıdır. Alırsanız önceki komutta değeri güncelleştirin bir **DNS ad etiketi** komutu yürütürken bir hata iletisi.

## <a name="manage-files-in-mounted-volume"></a>Takılan birimin dosyalarını yönetme

Kapsayıcı başlatıldığında sonra Microsoft dağıtılan basit web uygulaması kullanabilirsiniz [Acı hellofiles][aci-hellofiles] image to create small text files in the Azure file share at the mount path you specified. Obtain the web app's fully qualified domain name (FQDN) with the [az container show][az-container-show] komutu:

```azurecli-interactive
az container show --resource-group $ACI_PERS_RESOURCE_GROUP --name hellofiles --query ipAddress.fqdn --output tsv
```

Kullanabileceğiniz uygulamayı kullanarak metin kaydettikten sonra [Azure portalında][portal] or a tool like the [Microsoft Azure Storage Explorer][storage-explorer] almak ve dosya paylaşımına yazılıp dosyasını inceleyin.

## <a name="deploy-container-and-mount-volume---yaml"></a>Kapsayıcı dağıtın ve birim - YAML bağlama

Ayrıca bir kapsayıcı grubu dağıtmak ve Azure CLI ile bir kapsayıcıdaki bir birimi bağlamak ve bir [YAML şablonu](container-instances-multi-container-yaml.md). YAML şablonu tarafından dağıtma tercih edilen kapsayıcı grupları birden çok kapsayıcılardan oluşan dağıtırken yöntemidir.

Aşağıdaki YAML şablonu ile oluşturulan bir kapsayıcısı ile kapsayıcı grubunu tanımlar `aci-hellofiles` görüntü. Azure dosya paylaşımının kapsayıcı bağlar *acishare* birim olarak önceden oluşturulmuş. Belirtilen yerlerde, dosya paylaşımı barındıran depolama hesabının adını ve depolama anahtarını girin. 

CLI örneği olduğu gibi `dnsNameLabel` değer kapsayıcı örneğini oluşturduğunuz Azure bölgesi içinde benzersiz olmalıdır. YAML dosyası değeri gerekirse güncelleştirin.

```yaml
apiVersion: '2018-10-01'
location: eastus
name: file-share-demo
properties:
  containers:
  - name: hellofiles
    properties:
      environmentVariables: []
      image: mcr.microsoft.com/azuredocs/aci-hellofiles
      ports:
      - port: 80
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
      volumeMounts:
      - mountPath: /aci/logs/
        name: filesharevolume
  osType: Linux
  restartPolicy: Always
  ipAddress:
    type: Public
    ports:
      - port: 80
    dnsNameLabel: aci-demo
  volumes:
  - name: filesharevolume
    azureFile:
      sharename: acishare
      storageAccountName: <Storage account name>
      storageAccountKey: <Storage account key>
tags: {}
type: Microsoft.ContainerInstance/containerGroups
```

YAML şablonu ile dağıtmak için önceki YAML adlı bir dosyaya kaydedin `deploy-aci.yaml`, ardından yürütme [az kapsayıcı oluşturma][az-container-create] komutunu `--file` parametresi:

```azurecli
# Deploy with YAML template
az container create --resource-group myResourceGroup --file deploy-aci.yaml
```
## <a name="deploy-container-and-mount-volume---resource-manager"></a>Birim kapsayıcısı ve takma - Resource Manager dağıtma

CLI ve YAML dağıtım ek olarak, bir kapsayıcı grubu dağıtma ve Azure'ı kullanarak bir kapsayıcıdaki bir birimi [Resource Manager şablonu](/azure/templates/microsoft.containerinstance/containergroups).

İlk olarak, doldurmak `volumes` kapsayıcı grubu dizisinde `properties` şablon bölümü. 

Sonra istediğiniz bir birimi bağlamak için her kapsayıcı doldurmanız `volumeMounts` içindeki dizi `properties` kapsayıcı tanımının bölümü.

Aşağıdaki Resource Manager şablonu ile oluşturulan bir kapsayıcı ile kapsayıcı grubunu tanımlar `aci-hellofiles` görüntü. Azure dosya paylaşımının kapsayıcı bağlar *acishare* birim olarak önceden oluşturulmuş. Belirtilen yerlerde, dosya paylaşımı barındıran depolama hesabının adını ve depolama anahtarını girin. 

Önceki örneklerde olduğu gibi `dnsNameLabel` değer kapsayıcı örneğini oluşturduğunuz Azure bölgesi içinde benzersiz olmalıdır. Şablon değeri gerekirse güncelleştirin.

```JSON
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "variables": {
    "container1name": "hellofiles",
    "container1image": "mcr.microsoft.com/azuredocs/aci-hellofiles"
  },
  "resources": [
    {
      "name": "file-share-demo",
      "type": "Microsoft.ContainerInstance/containerGroups",
      "apiVersion": "2018-10-01",
      "location": "[resourceGroup().location]",
      "properties": {
        "containers": [
          {
            "name": "[variables('container1name')]",
            "properties": {
              "image": "[variables('container1image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              },
              "ports": [
                {
                  "port": 80
                }
              ],
              "volumeMounts": [
                {
                  "name": "filesharevolume",
                  "mountPath": "/aci/logs"
                }
              ]
            }
          }
        ],
        "osType": "Linux",
        "ipAddress": {
          "type": "Public",
          "ports": [
            {
              "protocol": "tcp",
              "port": "80"
            }
          ],
          "dnsNameLabel": "aci-demo"
        },
        "volumes": [
          {
            "name": "filesharevolume",
            "azureFile": {
                "shareName": "acishare",
                "storageAccountName": "<Storage account name>",
                "storageAccountKey": "<Storage account key>"
            }
          }
        ]
      }
    }
  ]
}
```

Resource Manager şablonu ile dağıtmak için önceki JSON adlı bir dosyaya Kaydet `deploy-aci.json`, ardından yürütme [az grubu dağıtımı oluşturmak][az-group-deployment-create] komutunu `--template-file` parametresi:

```azurecli
# Deploy with Resource Manager template
az group deployment create --resource-group myResourceGroup --template-file deploy-aci.json
```


## <a name="mount-multiple-volumes"></a>Birden çok birim bağlama

Birden çok birim bir kapsayıcı örneğine bağlanacak kullanarak dağıtmalısınız bir [Azure Resource Manager şablonu](/azure/templates/microsoft.containerinstance/containergroups) veya bir YAML dosyası. Bir şablon veya YAML dosyası kullanmak için paylaşım ayrıntılarını sağlayın ve doldurarak birimleri tanımlama `volumes` içindeki dizi `properties` şablon bölümü. 

Örneğin, adlı iki Azure dosya paylaşımını oluşturduğunuz *share1* ve *share2* depolama hesabındaki *myStorageAccount*, `volumes` dizi bir Kaynak Yöneticisi'nde Şablon aşağıdaki gibi görünür:

```JSON
"volumes": [{
  "name": "myvolume1",
  "azureFile": {
    "shareName": "share1",
    "storageAccountName": "myStorageAccount",
    "storageAccountKey": "<storage-account-key>"
  }
},
{
  "name": "myvolume2",
  "azureFile": {
    "shareName": "share2",
    "storageAccountName": "myStorageAccount",
    "storageAccountKey": "<storage-account-key>"
  }
}]
```

Ardından, içine istediğiniz bağlama birimleri kapsayıcı grubundaki her kapsayıcı için doldurma `volumeMounts` içindeki dizi `properties` kapsayıcı tanımının bölümü. Örneğin, bu iki birimi bağlar *myvolume1* ve *myvolume2*, önceden tanımlanmış:

```JSON
"volumeMounts": [{
  "name": "myvolume1",
  "mountPath": "/mnt/share1/"
},
{
  "name": "myvolume2",
  "mountPath": "/mnt/share2/"
}]
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Container ınstances'da diğer birim türleri bağlama işlemleri gerçekleştirmeyi öğreneceksiniz:

* [Azure kapsayıcı durumlarda emptyDir birim](container-instances-volume-emptydir.md)
* [Azure Container ınstances'da bir gitRepo birimi](container-instances-volume-gitrepo.md)
* [Azure Container ınstances'da bir gizli birimi](container-instances-volume-secret.md)

<!-- LINKS - External -->
[aci-hellofiles]: https://hub.docker.com/_/microsoft-azuredocs-aci-hellofiles 
[portal]: https://portal.azure.com
[storage-explorer]: https://storageexplorer.com

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-show]: /cli/azure/container#az-container-show
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create