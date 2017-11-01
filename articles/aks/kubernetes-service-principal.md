---
title: "Azure Kubernetes kümesi için hizmet sorumlusu | Microsoft Belgeleri"
description: "AKS içindeki bir Kubernetes kümesi için Azure Active Directory hizmet sorumlusu oluşturma ve yönetme"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: aks, azure-container-service, kubernetes
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: a532c8f69bfb19d26538aafe7c74f062dee06d9f
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="service-principals-with-azure-container-service-aks"></a>Azure Container Service (AKS) ile hizmet sorumluları

AKS kümesi, Azure API'leri ile etkileşime geçmek için [Azure Active Directory hizmet sorumlusu](../active-directory/develop/active-directory-application-objects.md) gerektirir. Hizmet sorumlusu, [kullanıcı tanımlı yollar](../virtual-network/virtual-networks-udr-overview.md) ve [4. Katman Azure Load Balancer](../load-balancer/load-balancer-overview.md) gibi kaynakları dinamik olarak yönetmek için gereklidir.

Bu makalede AKS'deki Kubernetes kümeniz için hizmet sorumlusu ayarlamak üzere kullanabileceğiniz farklı seçenekler gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu belgedeki adımlarda bir AKS kümesi oluşturduğunuz ve kümeyle bir kubectl bağlantısı kurduğunuz kabul edilmektedir. Bu öğelere gereksiniminiz varsa bkz. [AKS hızlı başlangıç](./kubernetes-walkthrough.md).

Azure AD hizmet sorumlusu oluşturmak için, Azure AD kiracınızla bir uygulamayı kaydetme ve uygulamanızı aboneliğinizdeki bir role atama izinlerinizin olması gerekir. Gerekli izinlere sahip değilseniz Azure AD veya abonelik yöneticinizden gerekli izinleri atamasını istemeniz veya Kubernetes kümesi için bir hizmet sorumlusu oluşturma ön işlemlerini tamamlamanız gerekebilir.

Ayrıca Azure CLI sürüm 2.0.20 veya üzerini yüklemiş ve yapılandırmış olmanız gerekir. Sürümü bulmak için az --version komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli).

## <a name="create-sp-with-aks-cluster"></a>AKS kümesi ile hizmet sorumlusu oluşturma

`az aks create` komutuyla bir AKS kümesi dağıtırken otomatik olarak bir hizmet sorumlusu oluşturma seçeneğine sahip olursunuz.

Aşağıdaki örnekte bir AKS kümesi oluşturulmakta ve var olan bir hizmet sorumlusu belirtilmediğinden küme için bir hizmet sorumlusu oluşturulmaktadır. Bu işlemi tamamlamak için hesabınızın gerekli hizmet sorumlusu oluşturma haklarına sahip olması gerekir. 

```azurecli
az aks create -n myClusterName -d myDNSPrefix -g myResourceGroup --generate-ssh-keys
```

## <a name="use-an-existing-sp"></a>Var olan bir hizmet sorumlusunu kullanma

Var olan bir Azure AD hizmet sorumlusu AKS kümesiyle kullanılabilir veya kullanılmak üzere ön oluşturma işlemleri gerçekleştirilebilir. Bu durum hizmet sorumlusu bilgilerini belirtmeniz gereken Azure portalı küme dağıtım süreçleri için faydalıdır.

Var olan bir hizmet sorumlusunu kullanıyorsanız aşağıdaki gereksinimleri karşılaması gerekir:

- Kapsam: Kümeyi dağıtmak için kullanılan abonelik
- Rol: Katkıda bulunan
- Gizli anahtar: Bir parola olmalıdır

## <a name="pre-create-a-new-sp"></a>Yeni bir hizmet sorumlusunu önceden oluşturma

Hizmet sorumlusunu Azure CLI ile oluşturmak için [az ad sp create-for-rbac]() komutunu kullanın.

```azurecli
id=$(az account show --query id --output tsv)
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/$id"
```

Çıktı aşağıdakine benzer olacaktır. `appId` ve `password` değerlerini not edin. Bu değerler AKS kümesi oluşturulurken kullanılır.

```
{
  "appId": "7248f250-0000-0000-0000-dbdeb8400d85",
  "displayName": "azure-cli-2017-10-15-02-20-15",
  "name": "http://azure-cli-2017-10-15-02-20-15",
  "password": "77851d2c-0000-0000-0000-cb3ebc97975a",
  "tenant": "72f988bf-0000-0000-0000-2d7cd011db47"
}
```

## <a name="use-an-existing-sp"></a>Var olan bir hizmet sorumlusunu kullanma

Önceden oluşturulmuş bir hizmet sorumlusunu kullanırken `appId` ve `password` değerlerini `az aks create` komutunda bağımsız değişken değerleri olarak girin.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myK8SCluster --service-princal <appId> ----client-secret <password>
```

AKS kümesini Azure portalından dağıtıyorsanız bu değerleri AKS kümesi yapılandırma formuna girin.

![Azure Vote’a göz atma görüntüsü](media/container-service-kubernetes-service-principal/sp-portal.png)

## <a name="additional-considerations"></a>Diğer konular

AKS ve Azure AD hizmet sorumlularıyla çalışırken aşağıdaki noktalara dikkat edin.

* Kubernetes için hizmet sorumlusu, küme yapılandırmasının bir parçasıdır. Ancak, kümeyi dağıtmak için kimlik kullanmayın.
* Her hizmet sorumlusunun bir Azure AD uygulamasıyla ilişkilendirilmiş olması gerekir. Bir Kubernetes kümesinin hizmet sorumlusu, geçerli herhangi bir Azure AD uygulama adıyla ilişkilendirilebilir (örneğin: `https://www.contoso.org/example`). Uygulama URL'sinin gerçek bir uç nokta olması gerekmez.
* Hizmet sorumlusu **İstemci Kimliğini** belirtirken `appId` değerini (bu makalede gösterilen şekilde) veya karşılık gelen hizmet sorumlusu `name` değerini (örneğin, `https://www.contoso.org/example`) kullanabilirsiniz.
* Kubernetes kümesindeki ana VM’lerde ve düğüm VM'lerinde hizmet sorumlusu kimlik bilgileri, /etc/kubernetes/azure.json dosyasında saklanır.
* Hizmet sorumlusunu otomatik olarak oluşturmak için `az aks create` komutunu kullandığınızda, hizmet sorumlusu kimlik bilgileri komutun çalıştırıldığı bilgisayarda ~/.azure/acsServicePrincipal.json dosyasına yazılır.
* `az aks create` komutu ile hizmet sorumlusunu otomatik olarak oluşturduğunuzda, hizmet sorumlusu aynı abonelikte oluşturulan bir [Azure kapsayıcı kayıt defteri](../container-registry/container-registry-intro.md) ile de kimlik doğrulaması yapabilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Active Directory hizmet sorumluları hakkında daha fazla bilgi için Azure AD uygulamaları belgelerine bakın.

> [!div class="nextstepaction"]
> [Uygulama ve hizmet sorumlusu nesneleri](../active-directory/develop/active-directory-application-objects.md)
