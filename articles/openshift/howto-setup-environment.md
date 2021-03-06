---
title: Azure Red Hat OpenShift geliştirme ortamınızı ayarlama | Microsoft Docs
description: Microsoft Azure Red Hat OpenShift ile çalışmak için Önkoşullar aşağıda verilmiştir.
services: openshift
keywords: Red hat openshift Kurulum ayarlama
author: jimzim
ms.author: jzim
ms.date: 05/10/2019
ms.topic: conceptual
ms.service: container-service
manager: jeconnoc
ms.openlocfilehash: a31655e8c8805505bdcc5e90bf25191590d35c18
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67672514"
---
# <a name="set-up-your-azure-red-hat-openshift-dev-environment"></a>Azure Red Hat OpenShift geliştirme ortamınızı ayarlama

Microsoft Azure Red Hat OpenShift uygulamalar oluşturup çalıştırın, sizin için yapmanız gerekir:

* Ayrılmış Azure sanal makine örnekleri satın alın.
* Sürüm 2.0.65 yüklemek (veya üzeri) Azure CLI (veya Azure Cloud Shell kullanın).
* Kaydolun `AROGA` özellik ve ilişkili kaynak sağlayıcıları.
* Azure Active Directory (Azure AD) kiracısı oluşturun.
* Azure AD uygulama nesnesi oluşturun.
* Bir Azure AD kullanıcısı oluşturun.

Aşağıdaki yönergeler bu Önkoşullar tümünde size yol gösterir.

## <a name="purchase-azure-red-hat-openshift-application-nodes-reserved-instances"></a>Azure Red Hat OpenShift uygulama düğümleri ayrılmış örnekler satın alma

Azure Red Hat OpenShift kullanabilmeniz için önce en az 4 Azure Red Hat OpenShift ayrılmış uygulama düğümünden daha sonra küme sağlama için mümkün olacaktır, satın almanız gerekir.

Bir Azure müşterisi olduğunuz [Azure Red Hat OpenShift ayrılmış örnekler satın alma](https://aka.ms/openshift/buy) Azure portalı üzerinden. Satın aldıktan sonra aboneliğinizi 24 saat içinde etkinleştirilecektir.

Bir Azure müşterisi değilseniz [satış birimiyle iletişime geçin](https://aka.ms/openshift/contact-sales) ve işlemini başlatmak için sayfanın alt kısmındaki Satış formunu doldurun.

Başvurmak [Azure Red Hat OpenShift fiyatlandırma sayfası](https://aka.ms/openshift/pricing) daha fazla bilgi için.

## <a name="install-the-azure-cli"></a>Azure CLI'yı yükleme

Azure Red Hat OpenShift sürümünü 2.0.65 gerektirir veya Azure CLI'ın daha yüksek. Azure CLI'yı zaten yüklediyseniz, çalıştırarak olması hangi sürümünü kontrol edebilirsiniz:

```bash
az --version
```

Çıkış ilk satırını CLI sürümünü örneğin olur `azure-cli (2.0.65)`.

Yönergeler için şunlardır [Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) yeni bir yükleme veya yükseltme gerekiyorsa.

Alternatif olarak, kullanabileceğiniz [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview). Azure Cloud Shell'i kullanırken seçtiğinizden emin olun **Bash** birlikte ilerleyin planlıyorsanız, ortam [oluşturun ve bir Azure Red Hat OpenShift kümesini yönetme](tutorial-create-cluster.md) öğretici serisi.

## <a name="register-providers-and-features"></a>Sağlayıcılar ve özellikleri kaydetme

`Microsoft.ContainerService AROGA` Özelliği `Microsoft.Solutions`, `Microsoft.Compute`, `Microsoft.Storage`, `Microsoft.KeyVault` ve `Microsoft.Network` sağlayıcıları, ilk Azure Red Hat OpenShift kümenizi dağıtmadan önce el ile aboneliğinize kaydedilmelidir.

Bu sağlayıcıları ve özellikleri el ile kaydetmek için aşağıdaki yönergeleri CLI'yı yüklediyseniz bir Bash kabuğundan veya Azure Cloud Shell'i (Bash) oturumunda, Azure portalında kullanın:

1. Birden çok Azure aboneliğiniz varsa, ilgili abonelik Kimliğini belirtin:

    ```bash
    az account set --subscription <SUBSCRIPTION ID>
    ```

1. Microsoft.ContainerService AROGA özelliği Kaydet:

    ```bash
    az feature register --namespace Microsoft.ContainerService -n AROGA
    ```

1. Microsoft.Storage sağlayıcısını kaydedin:

    ```bash
    az provider register -n Microsoft.Storage --wait
    ```
    
1. Microsoft.Compute sağlayıcısı kaydedin:

    ```bash
    az provider register -n Microsoft.Compute --wait
    ```

1. Microsoft.Solutions sağlayıcısını kaydedin:

    ```bash
    az provider register -n Microsoft.Solutions --wait
    ```

1. Microsoft.Network sağlayıcı kaydedin:

    ```bash
    az provider register -n Microsoft.Network --wait
    ```

1. Microsoft.KeyVault sağlayıcısını kaydedin:

    ```bash
    az provider register -n Microsoft.KeyVault --wait
    ```

1. Microsoft.ContainerService kaynak sağlayıcısının kaydını Yenile:

    ```bash
    az provider register -n Microsoft.ContainerService --wait
    ```

## <a name="create-an-azure-active-directory-azure-ad-tenant"></a>Azure Active Directory (Azure AD) kiracısı oluşturma

Azure Red Hat OpenShift hizmet Microsoft'a kuruluşunuz ile ilişkisini temsil eden bir ilişkili Azure Active Directory (Azure AD) kiracısı gerektirir. Azure AD kiracınızı kaydetmek, yapı ve uygulamaları yönetme, hem de diğer Azure hizmetlerini kullanmak sağlar.

Azure Red Hat OpenShift kümeniz için Kiracı olarak kullanılacak bir Azure AD yoksa veya test etmek için bir kiracı oluşturmak istiyorsanız, yönergeleri izleyin [Azure Red Hat OpenShift kümeniz için Azure AD kiracısı oluşturma](howto-create-tenant.md) önce Bu kılavuz ile devam ediliyor.

## <a name="create-an-azure-ad-user-security-group-and-application-object"></a>Bir Azure AD kullanıcı, güvenlik grubu ve uygulama oluşturma nesnesi

Azure Red Hat OpenShift depolama yapılandırma, kümenizde görevleri gerçekleştirmek için izinler gerektirir. Bu izinleri ile temsil edilen bir [hizmet sorumlusu](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals#service-principal-object). Azure Red Hat OpenShift kümenizde çalışan uygulamaları test etmek için yeni bir Active Directory kullanıcısı oluşturmak isteyebilirsiniz.

Bölümündeki yönergeleri [bir Azure AD uygulama nesnesi ve kullanıcı oluşturma](howto-aad-app-configuration.md) bir hizmet sorumlusu oluşturmak için bir istemci gizli anahtarı ve kimlik doğrulama geri çağırma URL'si için uygulamanızı oluşturmak ve yeni bir Azure AD güvenlik grubu ve kullanıcı erişmek için Küme.

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure Red Hat OpenShift kullanmaya hazırsınız!

Öğreticiyi deneyin:
> [!div class="nextstepaction"]
> [Azure Red Hat OpenShift kümesi oluşturma](tutorial-create-cluster.md)

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli
