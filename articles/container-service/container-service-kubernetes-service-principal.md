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
ms.date: 12/20/2016
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: 24e12e4606a5ec4fabf7046fe9847123033bb70a
ms.openlocfilehash: 073a9e66ac68643b27ecdd44a4ecac3ad79ec218


---

# <a name="about-the-azure-active-directory-service-principal-for-a-kubernetes-cluster-in-azure-container-service"></a>Azure Container Service içindeki bir Kubernetes kümesi için Azure Active Directory hizmet sorumlusu hakkında



Azure Container Service'te Kubernetes'in, Azure API'leri ile etkileşime geçmek için bir hizmet hesabı olarak [Azure Active Directory hizmet sorumlusuna](../active-directory/active-directory-application-objects.md) ihtiyacı vardır. Hizmet sorumlusu, [kullanıcı tanımlı yollar](../virtual-network/virtual-networks-udr-overview.md) ve 4. Katman [Azure Load Balancer](../load-balancer/load-balancer-overview.md) gibi kaynakları dinamik olarak yönetmek için gereklidir.

Bu makalede Kubernetes kümeniz için hizmet sorumlusu belirtmek üzere kullanabileceğini farklı seçenekler gösterilmektedir. Örneğin, [Azure CLI 2.0 (Önizleme)](https://docs.microsoft.com/cli/azure/install-az-cli2) yüklemesini ve kurulumunu yaptıysanız, [`az acs create`](https://docs.microsoft.com/en-us/cli/azure/acs#create) komutunu çalıştırarak Kubernetes kümesini ve hizmet sorumlusunu aynı anda oluşturabilirsiniz.

> [!NOTE]
> Azure Container Service'teki Kubernetes desteği şu anda önizleme aşamasındadır.


## <a name="requirements-for-the-service-principal"></a>Hizmet sorumlusu için gereksinimler

Azure Container Service içindeki bir Kubernetes kümesi için Azure Active Directory hizmet sorumlusu gereksinimleri aşağıda belirtilmiştir. 

* **Kapsam** - Kümenin dağıtıldığı Azure aboneliği

* **Rol** - **Katkıda Bulunan**

* **Gizli anahtar** - Bir parola olmalıdır. Şu anda sertifika kimlik doğrulaması için ayarlanmış bir hizmet sorumlusunu kullanamazsınız.

> [!NOTE]
> Her hizmet sorumlusunun bir Azure Active Directory uygulamasıyla ilişkilendirilmiş olması gerekir. Bir Kubernetes kümesinin hizmet sorumlusu, geçerli herhangi bir Azure Active Directory uygulama adıyla ilişkilendirilebilir.
> 


## <a name="service-principal-options-for-a-kubernetes-cluster"></a>Kubernetes kümesi için hizmet sorumlusu seçenekleri

### <a name="option-1-pass-the-service-principal-client-id-and-client-secret"></a>1. Seçenek: Hizmet sorumlusu istemci kimliğini ve gizli anahtarını iletin

Kubernetes kümesini oluştururken, mevcut bir hizmet sorumlusunun **istemci kimliğini** (genelde, Uygulama kimliği anlamına gelen `appId` olarak bilinir) ve **gizli anahtarını** (`password`) parametre olarak belirtin. Var olan bir hizmet sorumlusunu kullanıyorsanız önceki bölümde anlatılan gereksinimleri karşıladığından emin olun. Bir hizmet sorumlusu oluşturmanız gerekiyorsa, bu makalenin ilerleyen bölümlerindeki [Hizmet sorumlusu oluşturma](#create-a-service-principal-in-azure-active-directory) bölümüne bakın.

Bu parametreleri portalı, Azure Komut Satırı Arabirimini (CLI) veya Azure PowerShell'i kullanarak [Kubernetes kümesini dağıtırken](./container-service-deployment.md) belirtebilirsiniz.

>[!TIP] 
>**İstemci kimliğini** belirtirken, hizmet sorumlusunun `ObjectId` değerini değil `appId` değerini kullandığınızdan emin olun.
>

Aşağıdaki örnekte [Azure CLI](../xplat-cli-install.md) ile [Resource Manager modunda](../xplat-cli-connect.md) parametreleri iletme yollarından biri gösterilmektedir. Bu örnekte [Kubernetes hızlı başlangıç şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes) kullanılmıştır.

1. Şablon parametreleri dosyası olan azuredeploy.parameters.json dosyasını GitHub'dan [indirin](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.parameters.json).

2. Hizmet sorumlusunu belirtmek için dosyada `servicePrincipalClientId` ve `servicePrincipalClientSecret` değerlerini girin. (Ayrıca `dnsNamePrefix` ve `sshRSAPublicKey` için kendi değerlerinizi de belirtmeniz gerekir. İkincisi, kümeye erişmek için kullanılacak SSH ortak anahtarıdır.) Dosyayı kaydedin.

    ![Hizmet sorumlusu parametrelerini iletme](./media/container-service-kubernetes-service-principal/service-principal-params.png)

3. Aşağıdaki komutu `-e` parametresiyle çalıştırarak azuredeploy.parameters.json dosyasının yolunu belirtin. Bu komut, kümeyi `myResourceGroup` adlı var olan bir kaynak grubuna dağıtır.

    ```CLI
    azure group deployment create -n myClusterName -g myResourceGroup --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.json" -e azuredeploy.parameters.json
    ```


### <a name="option-2-generate-the-service-principal-when-creating-the-cluster-with-the-azure-cli-20-preview"></a>2. Seçenek: Azure CLI 2.0 Önizleme ile kümeyi oluştururken hizmet sorumlusunu oluşturun

[Azure CLI 2.0 (Önizleme)](https://docs.microsoft.com/cli/azure/install-az-cli2) yüklemesini ve kurulumunu yaptıysanız [`az acs create`](https://docs.microsoft.com/en-us/cli/azure/acs#create) komutunu çalıştırarak [kümeyi oluşturabilirsiniz](./container-service-create-acs-cluster-cli.md).

Diğer Kubernetes kümesi oluşturma seçeneklerinde olduğu gibi, `az acs create` çalıştırırken mevcut bir hizmet sorumlusunun parametrelerini belirtebilirsiniz. Ancak, bu parametreleri atlarsanız Azure Container Service otomatik olarak bir hizmet sorumlusu oluşturur. Bu işlem dağıtım sırasında saydam bir şekilde gerçekleştirilir. 

Aşağıdaki komut, bir Kubernetes kümesi oluşturmaya ek olarak hem SSH anahtarlarını hem de hizmet sorumlusu kimlik bilgilerini üretir:

```console
az acs create -n myClusterName -d myDNSPrefix -g myResourceGroup --generate-ssh-keys --orchestrator-type kubernetes
```

## <a name="create-a-service-principal-in-azure-active-directory"></a>Azure Active Directory'de hizmet sorumlusu oluşturma

Azure, Azure Active Directory'de Kubernetes kümenizde kullanmak üzere bir hizmet sorumlusu oluşturmak için kullanabileceğiniz birçok seçenek sunmaktadır. 

Aşağıdaki örnek komut bunu [Azure CLI 2.0 (Önizleme)](https://docs.microsoft.com/cli/azure/install-az-cli2) ile nasıl gerçekleştirebileceğinizi göstermektedir. Alternatif olarak [Azure Komut Satırı Arabirimi](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md), [Azure PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md) veya [klasik portal](../azure-resource-manager/resource-group-create-service-principal-portal.md) kullanarak da hizmet sorumlusu oluşturabilirsiniz.

> [!IMPORTANT]
> Bu makalenin önceki bölümlerinde yer alan hizmet sorumlusu gereksinimlerini incelemeyi unutmayın.
>

```console
az login

az account set --subscription="mySubscriptionID"

az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/mySubscriptionID"
```

Bu komut aşağıdakine benzer bir çıktı döndürür (burada gösterilen sonuç kısaltılmıştır):

![Hizmet sorumlusu oluşturma](./media/container-service-kubernetes-service-principal/service-principal-creds.png)

Vurgulanmış olan **istemci kimliği** (`appId`) ve **gizli anahtar** (`password`), küme dağıtımı için hizmet sorumlusu parametreleri olarak kullandığınız değerlerdir.


Yeni bir kabuk açarak hizmet sorumlunuzu onaylayın aşağıdaki komutları çalıştırın (`appId`, `password` ve `tenant` yerine kendi değerlerinizi yazın):

```console 
az login --service-principal -u yourClientID -p yourClientSecret --tenant yourTenant

az vm list-sizes --location westus
```

## <a name="additional-considerations"></a>Diğer konular


* Hizmet sorumlusu **istemci kimliğini** belirtirken `appId` değerini (bu makalede gösterilen şekilde) veya karşılık gelen hizmet sorumlusu `name` değerini (örneğin, `https://www.contoso.org/example`) kullanabilirsiniz.

* Hizmet sorumlusunu otomatik olarak oluşturmak için `az acs create` komutunu kullanırsanız, hizmet sorumlusu kimlik bilgileri komutun çalıştırıldığı bilgisayarda ~/.azure/acsServicePrincipal.json dosyasına yazılır.

* Kubernetes kümesindeki ana VM’lerde ve düğüm VM'lerinde hizmet sorumlusu kimlik bilgileri, /etc/kubernetes/azure.json dosyasında saklanır.

## <a name="next-steps"></a>Sonraki adımlar

* Kapsayıcı hizmeti kümenizde [Kubernetes ile çalışmaya başlama](container-service-kubernetes-walkthrough.md).



<!--HONumber=Jan17_HO1-->


