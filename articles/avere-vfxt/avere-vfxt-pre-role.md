---
title: Denetleyici yok - Azure için Avere vFXT Avere rolü oluşturun
description: Bir VM kümesi denetleyicisi olmadan gerekli RBAC rolü oluşturmak için yöntemi
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 547a0e0ac5254408a4064d627ec4c1e7c354a433
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50634530"
---
# <a name="create-the-avere-vfxt-cluster-runtime-access-role-without-a-controller"></a>Bir denetleyici olmadan Avere vFXT küme çalışma zamanı erişim rolü oluşturma

Bu belge, küme denetleyicisi VM oluşturmadan önce komut satırından küme düğümü erişim rolünü oluşturmak için bir yöntemi gösterir. 

Küme denetleyicisi oluşturmak için okuma [küme düğümü erişim rolünü oluşturma](avere-vfxt-deploy.md#create-the-cluster-node-access-role). Denetleyici resmi bir rolü prototip dosyası içerir. Abonelik Kimliğiniz ile dosyasını güncelleştirin ve rol VM denetleyicisinde yerel olarak tanımlamak için kullanın.

## <a name="create-an-azure-rbac-role"></a>Azure RBAC rolü oluşturun

Avere vFXT sisteminin kullandığı [rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/) vFXT küme düğümlerine gerekli görevleri gerçekleştirmek için yetkilendirmek üzere (RBAC).  

Normal vFXT küme işlemi bir parçası olarak bireysel vFXT düğümleri gibi şeyleri yapmanıza gerek Azure kaynak özelliklerini okumak, depolama alanını yönet ve diğer düğümlere ağ arabirimi ayarları denetler. 

Bu rol yalnızca vFXT düğümleri için değil küme denetleyicisi için VM kullanılır.  

Denetleyici önce rolü oluşturmak istiyorsanız, şu adımları izleyin: 

1. Azure portalında Azure Cloud Shell'i açın veya göz atın [ https://shell.azure.com ](https://shell.azure.com).

1. VFXT aboneliğinize geçiş yapmak için Azure CLI komutunu kullanın:

   ```az account set --subscription YOUR_SUBSCRIPTION_ID```

1. Rol tanımı Market görüntüsünden indirin ve düzenlemek için şu komutları kullanın. 

   ```bash
   wget -O- https://averedistribution.blob.core.windows.net/public/vfxtdistdoc.tgz | tar zxf - avere-cluster.json
   vi avere-cluster.json
   ``` 

4. Abonelik Kimliğinizi ekleyin ve yukarıdaki satırı silmek için dosyayı düzenleyin. Dosyayı Farklı Kaydet ``avere-cluster.json``.

   ![Metin düzenleyici, abonelik kimliği ve "remove bu satırı" silinmek üzere seçilen gösteren konsol](media/avere-vfxt-edit-role.png)

5. Rol oluşturun:  

   ```bash
   az role definition create --role-definition /avere-cluster.json
   ```

Kümeyi oluşturduğunuzda, rolün adını girin (Bu durumda, `avere-cluster`). Küme oluşturma komut, yeni oluşturulan küme düğümlerine rolü atar. 

## <a name="sample-cluster-node-runtime-role-definition"></a>Örnek Küme düğümü çalışma zamanı rol tanımı

> [!IMPORTANT] 
> Bu örnek tanımı, bir ürün GA öncesi sürümünden alınmıştır. Sürümü ile geçerli ürün dağıtımı Al farklı ise, bunun yerine bu sürümünü kullanın.

```json

{
    "AssignableScopes": [
        "/subscriptions/YOUR_SUBSCRIPTION_ID_GOES_HERE"
    ],
    "Name": "avere-cluster",
    "IsCustom": "true",
    "Description": "Avere cluster runtime role",
    "NotActions": [],
    "Actions": [
        "Microsoft.Compute/virtualMachines/read",
        "Microsoft.Network/networkInterfaces/read",
        "Microsoft.Network/networkInterfaces/write",
        "Microsoft.Network/virtualNetworks/subnets/read",
        "Microsoft.Network/virtualNetworks/subnets/join/action",
        "Microsoft.Network/networkSecurityGroups/join/action",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/write"
    ],
    "DataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write"
    ]
}

```