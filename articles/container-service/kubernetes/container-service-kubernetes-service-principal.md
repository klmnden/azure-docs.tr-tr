---
title: "Azure Kubernetes kümesi için hizmet sorumlusu | Microsoft Belgeleri"
description: "Azure Container Service içindeki bir Kubernetes kümesi için Azure Active Directory hizmet sorumlusu oluşturma ve yönetme"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/26/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 14975454cbc0afcfbdbd3aa6b52983be4d4b1785
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="set-up-an-azure-ad-service-principal-for-a-kubernetes-cluster-in-container-service"></a>Container Service’te bir Kubernetes kümesi için Azure AD hizmet sorumlusu oluşturma


Azure Container Service'te Kubernetes kümesi, Azure API'leri ile etkileşime geçmek için [Azure Active Directory hizmet sorumlusu](../../active-directory/develop/active-directory-application-objects.md) gerektirir. Hizmet sorumlusu, [kullanıcı tanımlı yollar](../../virtual-network/virtual-networks-udr-overview.md) ve [4. Katman Azure Load Balancer](../../load-balancer/load-balancer-overview.md) gibi kaynakları dinamik olarak yönetmek için gereklidir.


Bu makalede Kubernetes kümeniz için hizmet sorumlusu ayarlamak üzere kullanabileceğiniz farklı seçenekler gösterilmektedir. Örneğin, [Azure CLI 2.0](/cli/azure/install-az-cli2) yüklemesini ve kurulumunu yaptıysanız, [`az acs create`](/cli/azure/acs#create) komutunu çalıştırarak Kubernetes kümesini ve hizmet sorumlusunu aynı anda oluşturabilirsiniz.


## <a name="requirements-for-the-service-principal"></a>Hizmet sorumlusu için gereksinimler

Aşağıdaki gereksinimleri karşılayan mevcut bir Azure AD hizmet sorumlusunu kullanabilir veya yeni bir tane oluşturabilirsiniz.

* **Kapsam**: kümeyi dağıtmak için kullanılan abonelik.

* **Rol**: **Katkıda bulunan**

* **Gizli anahtar**: Bir parola olmalıdır. Şu anda sertifika kimlik doğrulaması için ayarlanmış bir hizmet sorumlusunu kullanamazsınız.

> [!IMPORTANT]
> Bir hizmet sorumlusu oluşturmak için, Azure AD kiracınızla bir uygulamayı kaydetme ve uygulamanızı aboneliğinizdeki bir role atama izinlerinizin olması gerekir. Gerekli izinlere sahip olup olmadığınızı görmek için [Portal’dan denetleyin](../../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions).
>

## <a name="option-1-create-a-service-principal-in-azure-ad"></a>1. Seçenek: Azure AD'de hizmet sorumlusu oluşturma

Kubernetes kümenizi dağıtmadan önce bir Azure AD hizmet sorumlusu oluşturmak isterseniz, Azure birkaç yöntem sunar.

Aşağıdaki örnek komut bunu [Azure CLI 2.0](../../azure-resource-manager/resource-group-authenticate-service-principal-cli.md) ile nasıl gerçekleştirebileceğinizi göstermektedir. Veya [Azure PowerShell](../../azure-resource-manager/resource-group-authenticate-service-principal.md), [portal](../../azure-resource-manager/resource-group-create-service-principal-portal.md) ya da diğer yöntemleri kullanarak bir hizmet sorumlusu oluşturabilirsiniz.

```azurecli
az login

az account set --subscription "mySubscriptionID"

az group create --name "myResourceGroup" --location "westus"

az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/mySubscriptionID"
```

Aşağıdakine benzer bir çıktı döndürülür (burada gösterilen sonuç kısaltılmıştır):

![Hizmet sorumlusu oluşturma](./media/container-service-kubernetes-service-principal/service-principal-creds.png)

Vurgulanmış olan **istemci kimliği** (`appId`) ve **gizli anahtar** (`password`), küme dağıtımı için hizmet sorumlusu parametreleri olarak kullandığınız değerlerdir.


### <a name="specify-service-principal-when-creating-the-kubernetes-cluster"></a>Kubernetes kümesi oluştururken hizmet sorumlusu belirtme

Kubernetes kümesini oluştururken, mevcut bir hizmet sorumlusunun **istemci kimliğini** (Uygulama kimliği anlamına gelen `appId` olarak da bilinir) ve **gizli anahtarını** (`password`) parametre olarak belirtin. Hizmet sorumlusunun bu makalenin başında verilen gereksinimleri karşıladığından emin olun.

Kubernetes kümesini dağıtırken, bu parametreleri [Azure Komut Satırı Arabirimi (CLI) 2.0](container-service-kubernetes-walkthrough.md), [Azure portalı](../dcos-swarm/container-service-deployment.md) veya diğer yöntemleri kullanarak belirtebilirsiniz.

>[!TIP]
>**İstemci kimliğini** belirtirken, hizmet sorumlusunun `ObjectId` değerini değil `appId` değerini kullandığınızdan emin olun.
>

Aşağıdaki örnekte Azure CLI 2.0 ile parametreleri iletme yollarından biri gösterilmektedir. Bu örnekte [Kubernetes hızlı başlangıç şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes) kullanılmıştır.

1. `azuredeploy.parameters.json` şablon parametre dosyasını GitHub’dan [indirin](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.parameters.json).

2. Hizmet sorumlusunu belirtmek için dosyada `servicePrincipalClientId` ve `servicePrincipalClientSecret` değerlerini girin. (Ayrıca `dnsNamePrefix` ve `sshRSAPublicKey` için kendi değerlerinizi de belirtmeniz gerekir. İkincisi, kümeye erişmek için kullanılacak SSH ortak anahtarıdır.) Dosyayı kaydedin.

    ![Hizmet sorumlusu parametrelerini iletme](./media/container-service-kubernetes-service-principal/service-principal-params.png)

3. Aşağıdaki komutu `--parameters` ile çalıştırarak azuredeploy.parameters.json dosyasının yolunu belirtin. Bu komut, kümeyi Batı ABD bölgesinde `myResourceGroup` adıyla oluşturduğunuz bir kaynak grubuna dağıtır.

    ```azurecli
    az login

    az account set --subscription "mySubscriptionID"

    az group create --name "myResourceGroup" --location "westus"

    az group deployment create -g "myResourceGroup" --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.json" --parameters @azuredeploy.parameters.json
    ```


## <a name="option-2-generate-a-service-principal-when-creating-the-cluster-with-az-acs-create"></a>2. Seçenek: `az acs create` ile küme oluştururken hizmet sorumlusu oluşturma

Kubernetes kümesini oluşturmak için [`az acs create`](/cli/azure/acs#create) komutunu çalıştırırsanız, otomatik olarak bir hizmet sorumlusu oluşturma seçeneğine sahip olursunuz.

Diğer Kubernetes kümesi oluşturma seçeneklerinde olduğu gibi, `az acs create` çalıştırırken mevcut bir hizmet sorumlusunun parametrelerini belirtebilirsiniz. Ancak, bu parametreleri atlarsanız Azure CLI, Container Service ile kullanılacak bir hizmet sorumlusunu otomatik olarak oluşturur. Bu işlem dağıtım sırasında saydam bir şekilde gerçekleştirilir.

Aşağıdaki komut, bir Kubernetes kümesi oluşturmaya ek olarak hem SSH anahtarlarını hem de hizmet sorumlusu kimlik bilgilerini üretir:

```console
az acs create -n myClusterName -d myDNSPrefix -g myResourceGroup --generate-ssh-keys --orchestrator-type kubernetes
```

> [!IMPORTANT]
> Hesabınızda Azure AD ve hizmet sorumlusu oluşturmaya yönelik abonelik izinleri yoksa, komut `Insufficient privileges to complete the operation.` benzeri bir hata oluşturur
>

## <a name="additional-considerations"></a>Diğer konular

* Aboneliğinizde hizmet sorumlusu oluşturma izniniz yoksa, Azure AD veya abonelik yöneticinizden gerekli izinleri atamasını istemeniz veya Azure Container Service ile kullanılacak hizmet sorumlusunu sormanız gerekebilir.

* Kubernetes için hizmet sorumlusu, küme yapılandırmasının bir parçasıdır. Ancak, kümeyi dağıtmak için kimlik kullanmayın.

* Her hizmet sorumlusunun bir Azure AD uygulamasıyla ilişkilendirilmiş olması gerekir. Bir Kubernetes kümesinin hizmet sorumlusu, geçerli herhangi bir Azure AD uygulama adıyla ilişkilendirilebilir (örneğin: `https://www.contoso.org/example`). Uygulama URL'sinin gerçek bir uç nokta olması gerekmez.

* Hizmet sorumlusu **İstemci Kimliğini** belirtirken `appId` değerini (bu makalede gösterilen şekilde) veya karşılık gelen hizmet sorumlusu `name` değerini (örneğin, `https://www.contoso.org/example`) kullanabilirsiniz.

* Kubernetes kümesindeki ana ve aracı VM’lerde hizmet sorumlusu kimlik bilgileri, /etc/kubernetes/azure.json dosyasında saklanır.

* Hizmet sorumlusunu otomatik olarak oluşturmak için `az acs create` komutunu kullandığınızda, hizmet sorumlusu kimlik bilgileri komutun çalıştırıldığı bilgisayarda ~/.azure/acsServicePrincipal.json dosyasına yazılır.

* `az acs create` komutu ile hizmet sorumlusunu otomatik olarak oluşturduğunuzda, hizmet sorumlusu aynı abonelikte oluşturulan bir [Azure kapsayıcı kayıt defteri](../../container-registry/container-registry-intro.md) ile de kimlik doğrulaması yapabilir.

## <a name="next-steps"></a>Sonraki adımlar

* Kapsayıcı hizmeti kümenizde [Kubernetes ile çalışmaya başlama](container-service-kubernetes-walkthrough.md).

* Kubernetes hizmet sorumlusu sorunlarını gidermek için bkz. [ACS Altyapısı belgeleri](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes.md#troubleshooting).