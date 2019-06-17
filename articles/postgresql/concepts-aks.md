---
title: Azure Kubernetes Service'i (AKS) - tek bir sunucu PostgreSQL için Azure veritabanı ile bağlanma
description: Azure Kubernetes hizmeti - tek bir sunucu PostgreSQL için Azure veritabanı ile bağlama hakkında bilgi edinin
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.date: 5/6/2019
ms.topic: conceptual
ms.openlocfilehash: a98d9b89db0406d67d1b067c3e53eb5c3dae7957
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65068949"
---
# <a name="connecting-azure-kubernetes-service-and-azure-database-for-postgresql---single-server"></a>Azure Kubernetes hizmeti ve Azure veritabanı, PostgreSQL - tek bir sunucu için bağlama

Azure Kubernetes Service (AKS), Azure'da kullanabileceğiniz yönetilen bir Kubernetes kümesi sağlar. Bir uygulama oluşturmak için AKS ve PostgreSQL için Azure veritabanı'nın birlikte kullanımı sırasında dikkate alınması gereken bazı seçenekler altındadır.


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

OSBA ile PostgreSQL için Azure veritabanı oluşturma ve AKS kümenizi Kubernetes doğal dil kullanarak bağlayın. OSBA ve Azure veritabanı için PostgreSQL birlikte üzerinde nasıl kullanılacağı hakkında bilgi edinin [OSBA GitHub sayfasına](https://github.com/Azure/open-service-broker-azure/blob/master/docs/modules/postgresql.md). 


## <a name="connection-pooling"></a>Bağlantı havuzu
Bir bağlantı havuzlayıcı oluşturma ve yeni veritabanı bağlantılarını kapatma ile ilişkili saat ve maliyeti en aza indirir. Havuzu, yeniden kullanılabilir bağlantılar koleksiyonudur. 

PostgreSQL ile kullanabileceğiniz birden fazla bağlantı poolers vardır. Bunlardan biri [PgBouncer](https://pgbouncer.github.io/). Microsoft kapsayıcı kayıt defterinde sepet havuzu bağlantısı AKS için Azure veritabanı'ndan PostgreSQL için kullanılabilecek basit bir kapsayıcı PgBouncer sunuyoruz. Ziyaret [docker hub sayfası](https://hub.docker.com/r/microsoft/azureossdb-tools-pgbouncer/) erişmek ve bu görüntüyü kullanma hakkında bilgi edinmek için. 


## <a name="next-steps"></a>Sonraki adımlar
-  [Azure Kubernetes Service kümesi oluşturma](../aks/kubernetes-walkthrough.md)
