---
title: Azure Kubernetes ağ ilkelerini | Microsoft Docs
description: Kubernetes hakkında Kubernetes kümenizin güvenliğini sağlamak için ağ ilkelerini öğrenin.
services: virtual-network
documentationcenter: na
author: aanandr
manager: NarayanAnnamalai
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 9/25/2018
ms.author: aanandr
ms.custom: ''
ms.openlocfilehash: a5c367402bd1e61485095fd1d565a8582acc3a9e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60824902"
---
# <a name="azure-kubernetes-network-policies-overview"></a>Azure Kubernetes ağ ilkelerine genel bakış

Yalnızca ağ güvenlik grupları (Nsg'ler) VM'ler için mikro segmentasyona sağlayın. gibi ağ ilkeleri için pod'ların mikro segmentasyona sağlar. Azure ağ ilkesi uygulaması, standart Kubernetes Ağ İlkesi belirtimi destekler. Pod'ların bir grup seçin ve bu pod'ların gelen ve izin verilen trafik türünü belirten giriş ve çıkış kuralları listesini tanımlamak için etiketleri kullanabilirsiniz. Kubernetes ağ ilkeleri hakkında daha fazla bilgi [Kubernetes belgeleri](https://kubernetes.io/docs/concepts/services-networking/network-policies/).

![Kubernetes ağ ilkelerine genel bakış](./media/kubernetes-network-policies/kubernetes-network-policies-overview.png)

Azure ağ ilkeleri, kapsayıcılar için sanal ağ tümleştirme sağlayan Azure CNI ile birlikte çalışır. Yalnızca Linux düğümlerinde bugün desteklenir. Uygulamaları, trafiği filtreleme zorlamak için tanımlanan ilkelere dayalı Linux IP tablosu kurallarını yapılandırın.

## <a name="planning-security-for-your-kubernetes-cluster"></a>Kubernetes kümeniz için güvenlik planlama
Kümeniz için güvenlik uygularken, diğer bir deyişle, Kuzey-Güney trafiği filtrelemek için ağ güvenlik grupları (Nsg'ler) girerek ve küme, alt ağdan ayrılan trafik ve Kubernetes ağ ilkeleri olan Doğu-Batı trafiği için kullanın. Kümenizde pod'ları arasındaki trafiği.

## <a name="using-azure-kubernetes-network-policies"></a>Azure Kubernetes ağ ilkelerini kullanma
Azure ağ ilkeleri için pod'ların mikro segmentasyona sağlamak için aşağıdaki yollarla kullanılabilir.

### <a name="acs-engine"></a>ACS-engine
ACS-Engine Azure'da bir Kubernetes kümesinin dağıtımı için bir Azure Resource Manager şablonu oluşturan bir araçtır. Küme yapılandırması, şablon oluşturma sırasında araca geçirilen bir JSON dosyasında belirtilir. Desteklenen küme ayarları listesi ve açıklamaları hakkında daha fazla bilgi için Microsoft Azure Container Service altyapısı - Küme tanımı bakın.

Acs-engine kullanılarak dağıtılan kümelerinde ilkelerini etkinleştirmek için "azure" olması küme tanım dosyasında networkPolicy ayarın değerini belirtin.

#### <a name="example-configuration"></a>Örnek yapılandırma

JSON Aşağıda örnek yapılandırma bir yeni sanal ağ ve alt ağ oluşturur ve Azure CNI ile içindeki bir Kubernetes kümesi dağıtır. JSON dosyasını düzenlemek için "Not" kullanmanızı öneririz. 
```json
{
  "apiVersion": "vlabs",
  "properties": {
    "orchestratorProfile": {
      "orchestratorType": "Kubernetes",
      “kubernetesConfig”: {
         "networkPolicy": "azure"
       }
    },
    "masterProfile": {
      "count": 1,
      "dnsPrefix": "<specify a cluster name>",
      "vmSize": "Standard_D2s_v3"
    },
    "agentPoolProfiles": [
      {
        "name": "agentpool",
        "count": 2,
        "vmSize": "Standard_D2s_v3",
        "availabilityProfile": "AvailabilitySet"
      }
    ],
   "linuxProfile": {
      "adminUsername": "<specify admin username>",
      "ssh": {
        "publicKeys": [
          {
            "keyData": "<cut and paste your ssh key here>"
          }
        ]
      }
    },
    "servicePrincipalProfile": {
      "clientId": "<enter the client ID of your service principal here >",
      "secret": "<enter the password of your service principal here>"
    }
  }
}

```
### <a name="creating-your-own-kubernetes-cluster-in-azure"></a>Azure'da kendi Kubernetes kümesi oluşturma
Uygulama ağ ilkeleri ACS-Engine gibi araçları bağlı kalmadan kendiniz dağıtmak için pod'ların Kubernetes kümelerini sağlamak için kullanılabilir. Bu durumda, ilk eklenti CNI yükleyin ve kümesindeki her sanal makinedeki etkinleştirin. Ayrıntılı yönergeler için bkz. [Eklentiyi kendi dağıttığınız Kubernetes kümesi için dağıtma](deploy-container-networking.md#deploy-plug-in-for-a-kubernetes-cluster).

Aşağıdaki komutu çalıştırın, küme dağıtıldıktan sonra `kubectl` indirin ve Azure ağ ilkesi uygulamak için komut *daemonset* kümeye.

  ```
  kubectl apply -f https://raw.githubusercontent.com/Azure/acs-engine/master/parts/k8s/addons/kubernetesmasteraddons-azure-npm-daemonset.yaml

  ```
Çözüm ayrıca açık kaynaklıdır ve kodu [Azure kapsayıcı ağ iletişimi depo](https://github.com/Azure/azure-container-networking/tree/master/npm).



## <a name="next-steps"></a>Sonraki adımlar
- Hakkında bilgi edinin [Azure Kubernetes hizmeti](../aks/intro-kubernetes.md).
-  Hakkında bilgi edinin [kapsayıcı ağ iletişimi](container-networking-overview.md).
- [Eklenti dağıtma](deploy-container-networking.md) Kubernetes kümelerini veya Docker kapsayıcıları için.