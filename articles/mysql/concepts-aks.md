---
title: Azure Kubernetes Service'i (AKS), MySQL için Azure veritabanı ile bağlanma
description: MySQL için Azure veritabanı ile Azure Kubernetes hizmeti bağlama hakkında bilgi edinin
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 11/28/2018
ms.openlocfilehash: d9f2e26a2bc89329ca9038c666c0d960289e2670
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60790530"
---
# <a name="connecting-azure-kubernetes-service-and-azure-database-for-mysql"></a>MySQL için Azure Kubernetes hizmeti ve Azure veritabanına bağlanma

Azure Kubernetes Service (AKS), Azure'da kullanabileceğiniz yönetilen bir Kubernetes kümesi sağlar. Bir uygulama oluşturmak için AKS ve MySQL için Azure veritabanı'nın birlikte kullanımı sırasında dikkate alınması gereken bazı seçenekler altındadır.


## <a name="accelerated-networking"></a>Hızlandırılmış ağ iletişimi
Hızlandırılmış ağ etkin arka plandaki sanal makinelerinin AKS kümenizin kullanın. Bir VM'de hızlandırılmış ağ etkin olduğunda, daha düşük gecikme süresi, daha az değişim ve düşük CPU kullanımı VM'deki yoktur. Hızlandırılmış ağ çalışır, desteklenen bir işletim sistemi sürümleri ve sanal makine örnekleri için desteklenen nasıl hakkında daha fazla bilgi [Linux](../virtual-network/create-vm-accelerated-networking-cli.md).

Kasım 2018 tarihinden sonra AKS bu desteklenen VM örneklerinde hızlandırılmış ağ iletişimi destekler. Hızlandırılmış ağ, bu sanal makineler kullanan yeni AKS kümelerde varsayılan olarak etkindir.

AKS kümenizi accelerated networking olup olmadığını doğrulayabilirsiniz:
1. Azure portalına gidin ve AKS kümenizi seçin.
2. Özellikler sekmesini seçin.
3. Adı Kopyala **altyapı kaynak grubu**.
4. Altyapı kaynak grubu bulup portal arama çubuğunu kullanın.
5. Bu kaynak grubunda bir VM seçin.
6. Sanal makinenin Git **ağ** sekmesi.
7. Onayla olmadığını **hızlandırılmış** 'Etkin.'

Veya aşağıdaki iki komutu Azure CLI kullanarak:
```azurecli
az aks show --resource-group myResourceGroup --name myAKSCluster --query "nodeResourceGroup"
```
Çıktı, AKS ağ arabirimi içeren oluşturduğu oluşturulan kaynak grubu olacaktır. "NodeResourceGroup" adını alın ve sonraki komutu kullanın. **EnableAcceleratedNetworking** true veya false olur:
```azurecli
az network nic list --resource-group nodeResourceGroup -o table
```

## <a name="open-service-broker-for-azure"></a>Azure için Açık Hizmet Aracısı 
[Azure için hizmet Aracısı'nı açın](https://github.com/Azure/open-service-broker-azure/blob/master/README.md) (OSBA), Azure hizmetlerinden doğrudan Kubernetes veya Cloud Foundry sağlama olanak sağlar. Bu bir [açık hizmet Aracısı API](https://www.openservicebrokerapi.org/) Azure için uygulama.

OSBA ile MySQL sunucusu için Azure veritabanı oluşturma ve AKS kümenizi Kubernetes doğal dil kullanarak bağlayın. OSBA ve Azure veritabanı için MySQL birlikte üzerinde nasıl kullanılacağı hakkında bilgi edinin [OSBA GitHub sayfasına](https://github.com/Azure/open-service-broker-azure/blob/master/docs/modules/mysql.md). 



## <a name="next-steps"></a>Sonraki adımlar
- [Azure Kubernetes Service kümesi oluşturma](../aks/kubernetes-walkthrough.md)
- Bilgi edinmek için nasıl [MySQL için Azure veritabanını ve OSBA kullanarak bir Helm grafiği yükle Wordpress'ten](../aks/integrate-azure.md)
