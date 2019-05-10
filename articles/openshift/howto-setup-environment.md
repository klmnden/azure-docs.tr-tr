---
title: Azure Red Hat OpenShift geliştirme ortamınızı ayarlama | Microsoft Docs
description: Microsoft Azure Red Hat OpenShift ile çalışmak için Önkoşullar aşağıda verilmiştir.
services: openshift
keywords: Red hat openshift Kurulum ayarlama
author: TylerMSFT
ms.author: twhitney
ms.date: 05/06/2019
ms.topic: conceptual
ms.service: openshift
manager: jeconnoc
ms.openlocfilehash: 3c265d6695af7ba1bc5833db59966a626cb29cb9
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65416054"
---
# <a name="set-up-your-azure-red-hat-openshift-dev-environment"></a>Azure Red Hat OpenShift geliştirme ortamınızı ayarlama

Microsoft Azure Red Hat OpenShift uygulamalar oluşturup çalıştırın, sizin için yapmanız gerekir:

* Ayrılmış Azure sanal makine örnekleri satın alın.
* Sürüm 2.0.64 yüklemek (veya üzeri) Azure CLI (veya Azure Cloud Shell kullanın).
* Kaydolun `openshiftmanagedcluster` özellik ve ilişkili kaynak sağlayıcıları.
* Azure Active Directory (Azure AD) kiracısı oluşturun.
* Azure AD uygulama nesnesi oluşturun.
* Bir Azure AD kullanıcısı oluşturun.

Aşağıdaki yönergeler bu Önkoşullar tümünde size yol gösterir.

## <a name="purchase-azure-virtual-machine-reserved-instances"></a>Ayrılmış Azure sanal makine örnekleri satın alın

Azure Red Hat OpenShift kullanabilmeniz için ayrılmış Azure sanal makine örnekleri satın almanız gerekir.

Bir Azure müşterisi, burada ait olması durumunda nasıl [satın alma Azure sanal makine örnekleri ayrılmış](https://aka.ms/openshift/buy). Rezervasyon azaltır, tam olarak yönetilen Azure Hizmetleri için önceden ödeme yaparak ayırın. Başvurmak [ *Azure ayırmaları nelerdir* ](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations) ayırmaları ve nasıl bunlar tasarruf hakkında daha fazla bilgi edinmek için.

Bir Azure müşterisi değilseniz [satış birimiyle iletişime geçin](https://aka.ms/openshift/contact-sales) ve işlemini başlatmak için sayfanın alt kısmındaki Satış formunu doldurun.

## <a name="install-the-azure-cli"></a>Azure CLI'yı yükleme

Azure Red Hat OpenShift sürümünü 2.0.64 gerektirir veya Azure CLI'ın daha yüksek. Azure CLI'yı zaten yüklediyseniz, çalıştırarak olması hangi sürümünü kontrol edebilirsiniz:

```bash
az --version
```

Çıkış ilk satırını CLI sürümünü örneğin olur `azure-cli (2.0.64)`.

Yönergeler için şunlardır [Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) yeni bir yükleme veya yükseltme gerekiyorsa.

Alternatif olarak, kullanabileceğiniz [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview). Azure Cloud Shell'i kullanırken seçtiğinizden emin olun **Bash** birlikte ilerleyin planlıyorsanız, ortam [oluşturun ve bir Azure Red Hat OpenShift kümesini yönetme](tutorial-create-cluster.md) öğretici serisi.

## <a name="register-providers-and-features"></a>Sağlayıcılar ve özellikleri kaydetme

`Microsoft.ContainerService openshiftmanagedcluster` Özelliği `Microsoft.Solutions`, ve `Microsoft.Network` sağlayıcıları, ilk Azure Red Hat OpenShift kümenizi dağıtmadan önce el ile aboneliğinize kaydedilmelidir.

Bu sağlayıcıları ve özellikleri el ile kaydetmek için aşağıdaki yönergeleri CLI'yı yüklediyseniz bir Bash kabuğundan veya Azure Cloud Shell'i (Bash) oturumunda, Azure portalında kullanın:.
1. Birden çok Azure aboneliğiniz varsa, ilgili abonelik Kimliğini belirtin:

    ```bash
    az account set --subscription <SUBSCRIPTION ID>
    ```

2. Microsoft.ContainerService openshiftmanagedcluster özelliği Kaydet:

    ```bash
    az feature register --namespace Microsoft.ContainerService -n openshiftmanagedcluster
    ```

3. Microsoft.Solutions sağlayıcısını kaydedin:

    ```bash
    az provider register -n Microsoft.Solutions --wait
    ```

4. Microsoft.Network sağlayıcı kaydedin:

    ```bash
    az provider register -n Microsoft.Network --wait
    ```

5. Microsoft.KeyVault sağlayıcısını kaydedin:

    ```bash
    az provider register -n Microsoft.KeyVault --wait
    ```

6. Microsoft.ContainerService kaynak sağlayıcısının kaydını Yenile:

    ```bash
    az provider register -n Microsoft.ContainerService --wait
    ```

## <a name="create-an-azure-active-directory-azure-ad-tenant"></a>Azure Active Directory (Azure AD) kiracısı oluşturma

Azure Red Hat OpenShift hizmet Microsoft'a kuruluşunuz ile ilişkisini temsil eden bir ilişkili Azure Active Directory (Azure AD) kiracısı gerektirir. Azure AD kiracınızı kaydetmek, yapı ve uygulamaları yönetme, hem de diğer Azure hizmetlerini kullanmak sağlar.

Azure Red Hat OpenShift kümeniz için Kiracı olarak kullanılacak bir Azure AD yoksa veya test etmek için bir kiracı oluşturmak istiyorsanız, yönergeleri izleyin [Azure Red Hat OpenShift kümeniz için Azure AD kiracısı oluşturma](howto-create-tenant.md) önce Bu kılavuz ile devam ediliyor.

## <a name="create-an-azure-ad-application-object-and-user"></a>Bir Azure AD uygulama nesnesi ve kullanıcı oluşturma

Azure Red Hat OpenShift depolama yapılandırma, kümenizde görevleri gerçekleştirmek için izinler gerektirir. Bu izinleri ile temsil edilen bir [hizmet sorumlusu](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals#service-principal-object) ve Azure üzerinde Red Hat OpenShift barındırmak için istediğinize iş yükünü temsil eden bir Azure AD uygulaması kaydettiğinizde oluşturulur. Azure Red Hat OpenShift kümenizde çalışan uygulamaları test etmek için yeni bir Active Directory kullanıcısı oluşturmak isteyebilirsiniz.

Bölümündeki yönergeleri [bir Azure AD uygulama nesnesi ve kullanıcı oluşturma](howto-aad-app-configuration.md) hizmet sorumlusu oluşturma hakkında bilgi edinmek için bir istemci gizli anahtarı ve kimlik doğrulama geri çağırma URL'si için uygulamanızı oluşturmak ve test etmek için yeni bir Active Directory kullanıcısı oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure Red Hat OpenShift kullanmaya hazırsınız!

Öğreticiyi deneyin:
> [!div class="nextstepaction"]
> [Azure Red Hat OpenShift kümesi oluşturma](tutorial-create-cluster.md)

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli