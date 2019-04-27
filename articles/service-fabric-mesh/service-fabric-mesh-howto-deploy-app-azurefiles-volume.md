---
title: Bir Service Fabric Mesh uygulaması içinde bir Azure tabanlı dosyaları birimi kullanın | Microsoft Docs
description: Azure CLI kullanarak bir hizmet içindeki bir Azure tabanlı dosyaları birimin bağlayarak durumu bir Azure Service Fabric Mesh uygulamada depolamayı öğrenin.
services: service-fabric-mesh
documentationcenter: .net
author: dkkapur
manager: chakdan
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/21/2018
ms.author: dekapur
ms.custom: mvc, devcenter
ms.openlocfilehash: fa078f17768d4885403f2f3e3d6b91251f0aaced
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60419382"
---
# <a name="mount-an-azure-files-based-volume-in-a-service-fabric-mesh-application"></a>Azure dosyaları bağlama dayalı bir Service Fabric Mesh uygulaması birimi 

Bu makalede, bir Service Fabric Mesh uygulamasının bir hizmetteki bir Azure tabanlı dosyaları birimi bağlamak açıklar.  Azure dosyaları birim sürücüsü, bir kapsayıcı hizmeti durumu kalıcı hale getirmek için kullandığınız bir Azure dosya paylaşımını bağlayabilmeniz için kullanılan bir Docker birim sürücüsüdür. Birimleri, genel amaçlı dosya depolama sağlar ve normal bir disk g/ç dosya API'lerini kullanarak dosya okuma/yazma olanak sağlar.  Birimler ve uygulama verilerini depolamak için seçenekleri hakkında daha fazla bilgi edinmek için [durumu depolamak üzere](service-fabric-mesh-storing-state.md).

Bir hizmetteki bir birimi bağlamak için Service Fabric Mesh uygulamanızda birim kaynağı oluşturun ve sonra birim hizmetinizdeki başvurun.  Birim kaynağına bildirme ve hizmet kaynağa başvuran yapılabilir ya da [YAML tabanlı kaynak dosyalarını](#declare-a-volume-resource-and-update-the-service-resource-yaml) veya [JSON tabanlı bir dağıtım şablonu](#declare-a-volume-resource-and-update-the-service-resource-json). Birim bağlamadan önce öncelikle bir Azure depolama hesabı oluşturun ve bir [Azure dosyaları'nda dosya paylaşımı](/azure/storage/files/storage-how-to-create-file-share).

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlamak için Azure Cloud Shell veya Azure CLI'ın yerel bir yüklemesi'ni kullanabilirsiniz. 

Azure CLI'yı yerel olarak bu makalede ile kullanmak için emin olun `az --version` en az döndürür `azure-cli (2.0.43)`.  İzleyerek Azure Service Fabric CLI'sını Mesh uzantısı modülü yüklemek (veya güncelleştirmek) [yönergeleri](service-fabric-mesh-howto-setup-cli.md).

Azure'da oturum açın ve aboneliğinizi ayarlamak için:

```azurecli
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-a-storage-account-and-file-share-optional"></a>Bir depolama hesabına ve dosya paylaşımı oluşturma (isteğe bağlı)
Bir Azure dosya birimi bağlama, bir depolama hesabına ve dosya paylaşımı gerektirir.  Mevcut bir Azure depolama hesabı ve dosya paylaşımını kullanın veya kaynakları oluşturun:

```azurecli-interactive
az group create --name myResourceGroup --location eastus

az storage account create --name myStorageAccount --resource-group myResourceGroup --location eastus --sku Standard_LRS --kind StorageV2

$current_env_conn_string=$(az storage account show-connection-string -n myStorageAccount -g myResourceGroup --query 'connectionString' -o tsv)

az storage share create --name myshare --quota 2048 --connection-string $current_env_conn_string
```

## <a name="get-the-storage-account-name-and-key-and-the-file-share-name"></a>Depolama hesabı adı ve anahtarı ve dosya paylaşım adı
Depolama hesabı adı, depolama hesabı anahtarını ve dosya paylaşımı adı olarak başvurulan `<storageAccountName>`, `<storageAccountKey>`, ve `<fileShareName>` aşağıdaki bölümlerde. 

Depolama hesaplarınızı listelemek ve kullanmak istediğiniz depolama hesabına dosya paylaşımı ile adını alın:
```azurecli-interactive
az storage account list
```

Dosya paylaşımının adını alın:
```azurecli-interactive
az storage share list --account-name <storageAccountName>
```

("Anahtar1") depolama hesabı anahtarını alın:
```azurecli-interactive
az storage account keys list --account-name <storageAccountName> --query "[?keyName=='key1'].value"
```

Ayrıca bu değerleri bulabilirsiniz [Azure portalında](https://portal.azure.com):
* `<storageAccountName>` -Altında **depolama hesapları**, dosya paylaşımını oluşturmak için kullanılan depolama hesabının adı.
* `<storageAccountKey>` -Altında depolama hesabınızı seçin **depolama hesapları** seçip **erişim anahtarları** ve değerin altında **key1**.
* `<fileShareName>` -Altında depolama hesabınızı seçin **depolama hesapları** seçip **dosyaları**. Kullanılacak adı, dosya paylaşımını oluşturduğunuz adıdır.

## <a name="declare-a-volume-resource-and-update-the-service-resource-json"></a>Birim kaynağına bildirme ve Hizmet kaynağı (JSON) güncelleştirme

Parametrelerini ekleyin `<fileShareName>`, `<storageAccountName>`, ve `<storageAccountKey>` bir önceki adımda bulduğunuz değerleri. 

Bir birim kaynak uygulama kaynağı eşi oluşturun. Bir ad ve sağlayıcı ("Azure tabanlı dosyaları birimi kullanın SFAzureFile") belirtin. İçinde `azureFileParameters`, parametreleri için workingdirectory belirtmeniz `<fileShareName>`, `<storageAccountName>`, ve `<storageAccountKey>` bir önceki adımda bulduğunuz değerleri.

Hizmet Birimi bağlamak için ekleme bir `volumeRefs` için `codePackages` hizmetinin öğesi.  `name` Birim (veya birim kaynak için dağıtım şablonu parametresi) için kaynak kimliği ve volume.yaml kaynak dosyası adı biriminin bildirilmiş.  `destinationPath` Birim bağlanan yerel bir dizin var.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "location": {
      "defaultValue": "EastUS",
      "type": "String",
      "metadata": {
        "description": "Location of the resources."
      }
    },
    "fileShareName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Azure Files file share that provides the volume for the container."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Azure storage account that contains the file share."
      }
    },
    "storageAccountKey": {
      "type": "securestring",
      "metadata": {
        "description": "Access key for the Azure storage account that contains the file share."
      }
    },
    "stateFolderName": {
      "type": "string",
      "defaultValue": "TestVolumeData",
      "metadata": {
        "description": "Folder in which to store the state. Provide an empty value to create a unique folder for each container to store the state. A non-empty value will retain the state across deployments, however if more than one applications are using the same folder, the counter may update more frequently."
      }
    }
  },
  "resources": [
    {
      "apiVersion": "2018-09-01-preview",
      "name": "VolumeTest",
      "type": "Microsoft.ServiceFabricMesh/applications",
      "location": "[parameters('location')]",
      "dependsOn": [
        "Microsoft.ServiceFabricMesh/networks/VolumeTestNetwork",
        "Microsoft.ServiceFabricMesh/volumes/testVolume"
      ],
      "properties": {
        "services": [
          {
            "name": "VolumeTestService",
            "properties": {
              "description": "VolumeTestService description.",
              "osType": "Windows",
              "codePackages": [
                {
                  "name": "VolumeTestService",
                  "image": "volumetestservice:dev",
                  "volumeRefs": [
                    {
                      "name": "[resourceId('Microsoft.ServiceFabricMesh/volumes', 'testVolume')]",
                      "destinationPath": "C:\\app\\data"
                    }
                  ],
                  "environmentVariables": [
                    {
                      "name": "ASPNETCORE_URLS",
                      "value": "http://+:20003"
                    },
                    {
                      "name": "STATE_FOLDER_NAME",
                      "value": "[parameters('stateFolderName')]"
                    }
                  ],
                  ...
                }
              ],
              ...
            }
          }
        ],
        "description": "VolumeTest description."
      }
    },
    {
      "apiVersion": "2018-09-01-preview",
      "name": "testVolume",
      "type": "Microsoft.ServiceFabricMesh/volumes",
      "location": "[parameters('location')]",
      "dependsOn": [],
      "properties": {
        "description": "Azure Files storage volume for the test application.",
        "provider": "SFAzureFile",
        "azureFileParameters": {
          "shareName": "[parameters('fileShareName')]",
          "accountName": "[parameters('storageAccountName')]",
          "accountKey": "[parameters('storageAccountKey')]"
        }
      }
    }
    ...
  ]
}
```

## <a name="declare-a-volume-resource-and-update-the-service-resource-yaml"></a>Birim kaynağına bildirme ve Hizmet kaynağı (YAML) güncelleştirme

Yeni bir *volume.yaml* dosyası *uygulama kaynaklarını* uygulamanız için dizin.  Bir ad ve sağlayıcı ("Azure tabanlı dosyaları birimi kullanın SFAzureFile") belirtin. `<fileShareName>`, `<storageAccountName>`, ve `<storageAccountKey>` bir önceki adımda bulduğunuz değerlerdir.

```yaml
volume:
  schemaVersion: 1.0.0-preview2
  name: testVolume
  properties:
    description: Azure Files storage volume for counter App.
    provider: SFAzureFile
    azureFileParameters: 
        shareName: <fileShareName>
        accountName: <storageAccountName>
        accountKey: <storageAccountKey>
```

Güncelleştirme *service.yaml* dosyası *hizmet kaynakları* hizmetinizdeki birimi bağlamak için dizin.  Ekleme `volumeRefs` öğesine `codePackages` öğesi.  `name` Birim (veya birim kaynak için dağıtım şablonu parametresi) için kaynak kimliği ve volume.yaml kaynak dosyası adı biriminin bildirilmiş.  `destinationPath` Birim bağlanan yerel bir dizin var.

```yaml
## Service definition ##
application:
  schemaVersion: 1.0.0-preview2
  name: VolumeTest
  properties:
    services:
      - name: VolumeTestService
        properties:
          description: VolumeTestService description.
          osType: Windows
          codePackages:
            - name: VolumeTestService
              image: volumetestservice:dev
              volumeRefs:
                - name: "[resourceId('Microsoft.ServiceFabricMesh/volumes', 'testVolume')]"
                  destinationPath: C:\app\data
              endpoints:
                - name: VolumeTestServiceListener
                  port: 20003
              environmentVariables:
                - name: ASPNETCORE_URLS
                  value: http://+:20003
                - name: STATE_FOLDER_NAME
                  value: TestVolumeData
              resources:
                requests:
                  cpu: 0.5
                  memoryInGB: 1
          replicaCount: 1
          networkRefs:
            - name: VolumeTestNetwork
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure dosyaları toplu örnek uygulamayı görünümünde [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter).
- Service Fabric Kaynak Modeli hakkında daha fazla bilgi için bkz. [Service Fabric Mesh Kaynak Modeli](service-fabric-mesh-service-fabric-resources.md).
- Service Fabric Mesh hakkında daha fazla bilgi edinmek için [Service Fabric Mesh’e genel bakış](service-fabric-mesh-overview.md) makalesini okuyun.
