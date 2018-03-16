---
title: "Azure Databricks: Ortak sorular ve Yardım | Microsoft Docs"
description: "Sık sorulan sorular ve sorun giderme bilgilerini Azure Databricks yanıtlarını alın."
services: azure-databricks
documentationcenter: 
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: nitinme
ms.openlocfilehash: 5da6ffc346cc0e7f0f83bf4a4c33600b668a17ca
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2018
---
# <a name="frequently-asked-questions-about-azure-databricks"></a>Azure Databricks hakkında sık sorulan sorular

Bu makalede Azure Databricks ilgili en sık kullanılan sorguların listelenmektedir. Ayrıca, Databricks kullanırken karşılaşabileceğiniz bazı ortak sorunları listeler. Daha fazla bilgi için bkz: [Azure Databricks nedir](what-is-azure-databricks.md). 

## <a name="can-i-use-my-own-keys-for-local-encryption"></a>Kendi anahtarları yerel şifreleme için kullanabilir miyim? 
Kendi anahtarları Azure anahtar kasası kullanarak geçerli sürümde desteklenmiyor. 

## <a name="can-i-use-azure-virtual-networks-with-databricks"></a>Azure sanal ağlar ile Databricks kullanabilir miyim?
Yeni bir sanal ağ Databricks sağlama bir parçası olarak oluşturulur. Bu sürümde, kendi Azure sanal ağı kullanamazsınız.

## <a name="how-do-i-access-azure-data-lake-store-from-a-notebook"></a>Bir dizüstü bilgisayarınızı Azure Data Lake Store nasıl erişirim? 

Şu adımları uygulayın:
1. Azure Active Directory (Azure AD), bir hizmet sorumlusu sağlamak ve anahtarıyla kaydedin.
2. Data Lake Store'da hizmet sorumlusu için gerekli izinleri atayın.
3. Data Lake Store'da bir dosyaya erişmek için not defterinde hizmet asıl kimlik bilgilerini kullanın.

Daha fazla bilgi için bkz: [kullanım Data Lake Store ile Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake.html).

## <a name="fix-common-problems"></a>Sık karşılaşılan sorunları giderin

İle Databricks karşılaşabileceğiniz bazı sorunlar şunlardır.

### <a name="issue-this-subscription-is-not-registered-to-use-the-namespace-microsoftdatabricks"></a>Sorun: Bu abonelik 'Microsoft.Databricks' ad alanını kullanmak için kayıtlı değil

#### <a name="error-message"></a>Hata iletisi

"Bu aboneliği 'Microsoft.Databricks' ad alanını kullanmak için kayıtlı değil. Bkz: https://aka.ms/rps-not-found abonelikleri kaydetme için. (Code: MissingSubscriptionRegistration)"

#### <a name="solution"></a>Çözüm

1. [Azure Portal](https://portal.azure.com) gidin.
2. Seçin **abonelikleri**, kullanmakta olduğunuz, abonelik ve ardından **kaynak sağlayıcıları**. 
3. Kaynak sağlayıcıları listesinden karşı **Microsoft.Databricks**seçin **kaydetmek**. Kayıt kaynak sağlayıcısı için abonelikte katılımcı veya sahibi rolü olmalıdır.


### <a name="issue-your-account-email-does-not-have-the-owner-or-contributor-role-on-the-databricks-workspace-resource-in-the-azure-portal"></a>Sorun: Hesabınızı {e-posta} sahibi veya katkıda bulunan rolü Azure portalında Databricks çalışma kaynakta yok

#### <a name="error-message"></a>Hata iletisi

"{E-posta} hesabınızı sahibi veya katkıda bulunan rolü Azure portalında Databricks çalışma kaynakta yok. Konuk kullanıcı Kiracı olarak varsa bu hatayı da oluşabilir. Erişim ya da doğrudan Databricks çalışma alanında bir kullanıcı olarak eklediğiniz vermek için yöneticinize başvurun." 

#### <a name="solution"></a>Çözüm

Bu sorun için çözüm birkaç şunlardır:

* Kiracı başlatmak için Konuk kullanıcı olarak değil Kiracı normal bir kullanıcı olarak oturum açmanız gerekir. Ayrıca katkıda bulunan rolü Databricks çalışma kaynak üzerinde sahip olmalıdır. Bir kullanıcının erişim verebilirsiniz **erişim denetimi (IAM)** Azure portalında Databricks çalışma alanınızı sekmede.

* E-posta etki alanı adınızı Azure AD içinde birden fazla dizine atanırsa, bu hata ayrıca ortaya çıkabilir. Bu sorunu çözmek için yeni bir kullanıcı Databricks çalışma alanınızı abonelikle içeren dizin oluşturun.

    a. Azure portalında Azure AD'ye gidin. Seçin **kullanıcılar ve gruplar** > **kullanıcı ekleme**.

    b. Bir kullanıcı eklemek bir `@<tenant_name>.onmicrosoft.com` yerine e-posta `@<your_domain>` e-posta. Bu seçenek bulabilirsiniz **özel etki alanları**, Azure portalında Azure AD altında.
    
    c. Bu yeni kullanıcı izni **katkıda bulunan** Databricks çalışma Kaynak rolü.
    
    d. Yeni kullanıcı ile Azure portalında oturum açın ve Databricks çalışma alanını bulun.
    
    e. Bu kullanıcı olarak Databricks çalışma alanı başlatın.


### <a name="issue-your-account-email-has-not-been-registered-in-databricks"></a>Sorun: Hesabınızı {e-posta} Databricks içinde kayıtlı değil 

#### <a name="solution"></a>Çözüm

Çalışma alanı oluşturmadınız ve bir kullanıcı olarak eklenir, çalışma alanı oluşturan kişiye başvurun. Bu kişi Azure Databricks yönetici konsolunu kullanarak Ekle vardır. Yönergeler için bkz: [ekleme ve kullanıcıları yönetme](https://docs.azuredatabricks.net/administration-guide/admin-settings/users.html). Çalışma alanı oluşturduysanız ve bu hatayı almaya devam seçmeyi deneyin **başlatma çalışma** Azure portalından.

### <a name="issue-cloud-provider-launch-failure-while-setting-up-the-cluster-publicipcountlimitreached"></a>Sorun: (PublicIPCountLimitReached) kümeyi ayarlamadan sırasında bulut sağlayıcısı başlatma hatası

#### <a name="error-message"></a>Hata iletisi

"Bulut sağlayıcısı başlatma hatası: kümeyi ayarlanırken bir bulut sağlayıcısı hatasıyla karşılaşıldı. Daha fazla bilgi için Databricks kılavuzuna bakın. Azure hata kodu: PublicIPCountLimitReached. Azure hata iletisi: Bu bölgede Bu abonelik için birden fazla 60 genel IP adresleri oluşturamıyor. "

#### <a name="solution"></a>Çözüm

Databricks küme düğümü başına bir genel IP adresi kullanın. Aboneliğiniz zaten tüm genel IP'ler kullanıldıysa, gereken [Kotayı artırmak için istek](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request). Seçin **kota** olarak **sorun türü**, ve **ağ: ARM** olarak **kota türü**. İçinde **ayrıntıları**, bir ortak IP adresi kota artışı isteği. Örneğin, sınırınızı şu anda 60'tır ve 100 düğümlü bir küme oluşturmak isterseniz, 160 için sınırın artırılmasını istemek.

### <a name="issue-a-second-type-of-cloud-provider-launch-failure-while-setting-up-the-cluster-missingsubscriptionregistration"></a>Sorun: Bir ikinci türü (MissingSubscriptionRegistration) kümeyi ayarlamadan sırasında bulut sağlayıcısı başlatma hatası

#### <a name="error-message"></a>Hata iletisi

"Bulut sağlayıcısı başlatma hatası: kümeyi ayarlanırken bir bulut sağlayıcısı hatasıyla karşılaşıldı. Daha fazla bilgi için Databricks kılavuzuna bakın.
Azure hata kodu: MissingSubscriptionRegistration Azure hata iletisi: Abonelik 'Microsoft.Compute' ad alanını kullanmak için kayıtlı değil. Bkz: https://aka.ms/rps-not-found abonelikleri kaydetmek nasıl. "

#### <a name="solution"></a>Çözüm

1. [Azure Portal](https://portal.azure.com) gidin.
2. Seçin **abonelikleri**, kullanmakta olduğunuz, abonelik ve ardından **kaynak sağlayıcıları**. 
3. Kaynak sağlayıcıları listesinden karşı **Microsoft.Compute**seçin **kaydetmek**. Kayıt kaynak sağlayıcısı için abonelikte katılımcı veya sahibi rolü olmalıdır.

Daha ayrıntılı yönergeler için bkz: [kaynak sağlayıcıları ve türleri](../azure-resource-manager/resource-manager-supported-services.md).

### <a name="issue-azure-databricks-needs-permissions-to-access-resources-in-your-organization-that-only-an-admin-can-grant"></a>Sorun: Azure Databricks yalnızca bir yönetici izni verebilir, kuruluşunuzdaki kaynaklara erişim izinleri gerekir.

#### <a name="background"></a>Arka plan

Azure Databricks Azure AD ile tümleşiktir. Bu, Azure Databricks içinde izinleri (örneğin, dizüstü bilgisayarlar veya kümeleri) ayarlamak Azure AD kullanıcıların belirterek sağlar. Azure Databricks'ın, Azure AD'den kullanıcılarının adlarını listelemek bu bilgileri okuma izni gerektirir. Bu, bir onay gerektirir. Onay kullanılabilir değilse, hata bakın.

#### <a name="solution"></a>Çözüm

Azure portalına genel yönetici olarak oturum açın. Azure Active Directory için Git **kullanıcı ayarları** sekmesinde ve emin olun **kullanıcıların şirket adına şirket verilerine erişen uygulamaları izin** ayarlanır **Evet**.

## <a name="next-steps"></a>Sonraki adımlar

- [Hızlı Başlangıç: Azure Databricks ile çalışmaya başlama](quickstart-create-databricks-workspace-portal.md)
- [Azure Databricks nedir?](what-is-azure-databricks.md)

