---
title: "Azure Kubernetes kümesi için hizmet sorumlusu | Microsoft Belgeleri"
description: "Kubernetes ile bir Azure Container Service kümesinde Azure Active Directory hizmet sorumlusu oluşturma ve yönetme"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/21/2017
ms.author: danlep
ms.translationtype: Human Translation
ms.sourcegitcommit: f6006d5e83ad74f386ca23fe52879bfbc9394c0f
ms.openlocfilehash: b76020e3e5855a63c416851d9b9adefdbdc5874a
ms.contentlocale: tr-tr
ms.lasthandoff: 05/03/2017


---

# <a name="about-the-azure-active-directory-service-principal-for-a-kubernetes-cluster-in-azure-container-service"></a>Azure Container Service içindeki bir Kubernetes kümesi için Azure Active Directory hizmet sorumlusu hakkında



Azure Container Service'te Kubernetes'in, Azure API'leri ile etkileşime geçmek için bir hizmet hesabı olarak [Azure Active Directory hizmet sorumlusuna](../active-directory/active-directory-application-objects.md) ihtiyacı vardır. Hizmet sorumlusu, [kullanıcı tanımlı yollar](../virtual-network/virtual-networks-udr-overview.md) ve [4. Katman Azure Load Balancer](../load-balancer/load-balancer-overview.md) gibi kaynakları dinamik olarak yönetmek için gereklidir.

Bu makalede Kubernetes kümeniz için hizmet sorumlusu belirtmek üzere kullanabileceğini farklı seçenekler gösterilmektedir. Örneğin, [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) yüklemesini ve kurulumunu yaptıysanız, [`az acs create`](https://docs.microsoft.com/en-us/cli/azure/acs#create) komutunu çalıştırarak Kubernetes kümesini ve hizmet sorumlusunu aynı anda oluşturabilirsiniz.



## <a name="requirements-for-the-service-principal"></a>Hizmet sorumlusu için gereksinimler

Azure Container Service içindeki bir Kubernetes kümesi için Azure Active Directory hizmet sorumlusu gereksinimleri aşağıda belirtilmiştir. 

* **Kapsam**: Kümenin dağıtıldığı kaynak grubu

* **Rol**: **Katkıda bulunan**

* **Gizli anahtar**: Bir parola olmalıdır. Şu anda sertifika kimlik doğrulaması için ayarlanmış bir hizmet sorumlusunu kullanamazsınız.

> [!NOTE]
> Her hizmet sorumlusunun bir Azure Active Directory uygulamasıyla ilişkilendirilmiş olması gerekir. Bir Kubernetes kümesinin hizmet sorumlusu, geçerli herhangi bir Azure Active Directory uygulama adıyla ilişkilendirilebilir.
> 


## <a name="service-principal-options-for-a-kubernetes-cluster"></a>Kubernetes kümesi için hizmet sorumlusu seçenekleri

### <a name="option-1-pass-the-service-principal-client-id-and-client-secret"></a>1. Seçenek: Hizmet sorumlusu istemci kimliğini ve gizli anahtarını iletin

Kubernetes kümesini oluştururken, mevcut bir hizmet sorumlusunun **istemci kimliğini** (Uygulama kimliği anlamına gelen `appId` olarak da bilinir) ve **gizli anahtarını** (`password`) parametre olarak belirtin. Var olan bir hizmet sorumlusunu kullanıyorsanız önceki bölümde anlatılan gereksinimleri karşıladığından emin olun. Bir hizmet sorumlusu oluşturmanız gerekiyorsa, bu makalenin ilerleyen bölümlerindeki [Hizmet sorumlusu oluşturma](#create-a-service-principal-in-azure-active-directory) bölümüne bakın.

[Kubernetes kümesini dağıtırken](./container-service-deployment.md) bu parametreleri portal, Azure Komut Satırı Arabirimi (CLI) 2.0, Azure PowerShell veya diğer yöntemleri kullanarak belirtebilirsiniz.

>[!TIP] 
>**İstemci kimliğini** belirtirken, hizmet sorumlusunun `ObjectId` değerini değil `appId` değerini kullandığınızdan emin olun.
>

Aşağıdaki örnekte Azure CLI 2.0 ile parametreleri iletme yollarından biri gösterilmektedir ([yükleme ve kurulum yönergelerine bakın](/cli/azure/install-az-cli2)). Bu örnekte [Kubernetes hızlı başlangıç şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes) kullanılmıştır.

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


### <a name="option-2-generate-the-service-principal-when-creating-the-cluster-with-the-azure-cli-20"></a>2. Seçenek: Hizmet sorumlusunu Azure CLI 2.0 ile kümeyi oluştururken oluşturma

[Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) yüklemesini ve kurulumunu yaptıysanız [`az acs create`](https://docs.microsoft.com/en-us/cli/azure/acs#create) komutunu çalıştırarak [kümeyi oluşturabilirsiniz](./container-service-create-acs-cluster-cli.md).

Diğer Kubernetes kümesi oluşturma seçeneklerinde olduğu gibi, `az acs create` çalıştırırken mevcut bir hizmet sorumlusunun parametrelerini belirtebilirsiniz. Ancak, bu parametreleri atlarsanız Azure Container Service otomatik olarak bir hizmet sorumlusu oluşturur. Bu işlem dağıtım sırasında saydam bir şekilde gerçekleştirilir. 

Aşağıdaki komut, bir Kubernetes kümesi oluşturmaya ek olarak hem SSH anahtarlarını hem de hizmet sorumlusu kimlik bilgilerini üretir:

```console
az acs create -n myClusterName -d myDNSPrefix -g myResourceGroup --generate-ssh-keys --orchestrator-type kubernetes
```

## <a name="create-a-service-principal-in-azure-active-directory"></a>Azure Active Directory'de hizmet sorumlusu oluşturma

Azure, Azure Active Directory'de Kubernetes kümenizde kullanmak üzere bir hizmet sorumlusu oluşturmak için kullanabileceğiniz birçok seçenek sunmaktadır. 

Aşağıdaki örnek komut bunu [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) ile nasıl gerçekleştirebileceğinizi göstermektedir. Veya [Azure PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md), [klasik portal](../azure-resource-manager/resource-group-create-service-principal-portal.md) ya da diğer yöntemleri kullanarak bir hizmet sorumlusu oluşturabilirsiniz.

> [!IMPORTANT]
> Bu makalenin önceki bölümlerinde yer alan hizmet sorumlusu gereksinimlerini incelemeyi unutmayın.
>

```azurecli
az login

az account set --subscription "mySubscriptionID"

az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/mySubscriptionID"
```

Bu komut aşağıdakine benzer bir çıktı döndürür (burada gösterilen sonuç kısaltılmıştır):

![Hizmet sorumlusu oluşturma](./media/container-service-kubernetes-service-principal/service-principal-creds.png)

Vurgulanmış olan **istemci kimliği** (`appId`) ve **gizli anahtar** (`password`), küme dağıtımı için hizmet sorumlusu parametreleri olarak kullandığınız değerlerdir.


Yeni bir kabuk açarak hizmet sorumlunuzu onaylayın aşağıdaki komutları çalıştırın (`appId`, `password` ve `tenant` yerine kendi değerlerinizi yazın):

```azurecli 
az login --service-principal -u yourClientID -p yourClientSecret --tenant yourTenant

az vm list-sizes --location westus
```

## <a name="additional-considerations"></a>Diğer konular


* Hizmet sorumlusu **istemci kimliğini** belirtirken `appId` değerini (bu makalede gösterilen şekilde) veya karşılık gelen hizmet sorumlusu `name` değerini (örneğin, `https://www.contoso.org/example`) kullanabilirsiniz.

* Hizmet sorumlusunu otomatik olarak oluşturmak için `az acs create` komutunu kullanırsanız, hizmet sorumlusu kimlik bilgileri komutun çalıştırıldığı bilgisayarda ~/.azure/acsServicePrincipal.json dosyasına yazılır.

* Kubernetes kümesindeki ana VM’lerde ve düğüm VM'lerinde hizmet sorumlusu kimlik bilgileri, /etc/kubernetes/azure.json dosyasında saklanır.

## <a name="next-steps"></a>Sonraki adımlar

* Kapsayıcı hizmeti kümenizde [Kubernetes ile çalışmaya başlama](container-service-kubernetes-walkthrough.md).

