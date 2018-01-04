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
ms.date: 11/17/2017
ms.author: nitinme
<<<<<<< HEAD
ms.openlocfilehash: d8257056fddda408b622d3da11c707ff39e180db
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: HT
=======
ms.openlocfilehash: fb77ec001f9f52e0a974f8765f458f831fb63908
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
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

Daha fazla bilgi için bkz: [kullanım Data Lake Store ile Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-storage.html#azure-data-lake-store).

## <a name="fix-common-problems"></a>Sık karşılaşılan sorunları giderin

İle Databricks karşılaşabileceğiniz bazı sorunlar şunlardır.

### <a name="this-subscription-is-not-registered-to-use-the-namespace-microsoftdatabricks"></a>Bu abonelik 'Microsoft.Databricks' ad alanını kullanmak için kayıtlı değil

#### <a name="error-message"></a>Hata iletisi

"Bu aboneliği 'Microsoft.Databricks' ad alanını kullanmak için kayıtlı değil. Abonelikleri nasıl https://aka.MS/RPS-not-found bakın. (Code: MissingSubscriptionRegistration) "

#### <a name="solution"></a>Çözüm

1. [Azure Portal](https://portal.azure.com) gidin.
2. Seçin **abonelikleri**, kullanmakta olduğunuz, abonelik ve ardından **kaynak sağlayıcıları**. 
3. Kaynak sağlayıcıları listesinden karşı **Microsoft.Databricks**seçin **kaydetmek**. Kayıt kaynak sağlayıcısı için abonelikte katılımcı veya sahibi rolü olmalıdır.


### <a name="your-account-email-does-not-have-the-owner-or-contributor-role-on-the-databricks-workspace-resource-in-the-azure-portal"></a>Hesabınızı {e-posta} sahibi veya katkıda bulunan rolü Azure portalında Databricks çalışma kaynak yok

#### <a name="error-message"></a>Hata iletisi

"{E-posta} hesabınızı sahibi veya katkıda bulunan rolü Azure portalında Databricks çalışma kaynakta yok. Konuk kullanıcı Kiracı olarak varsa bu hatayı da oluşabilir. Erişim ya da doğrudan Databricks çalışma alanında bir kullanıcı olarak eklediğiniz vermek için yöneticinize başvurun." 

#### <a name="solution"></a>Çözüm

Bu sorun için çözüm birkaç şunlardır:

* Kiracı başlatmak için Konuk kullanıcı olarak değil Kiracı normal bir kullanıcı olarak oturum açmanız gerekir. Ayrıca katkıda bulunan rolü Databricks çalışma kaynak üzerinde sahip olmalıdır. Bir kullanıcının erişim verebilirsiniz **erişim denetimi (IAM)** Azure portalında Databricks çalışma alanınızı sekmede.

* E-posta etki alanı adınızı Azure AD içinde birden fazla dizine atanırsa, bu hata ayrıca ortaya çıkabilir. Bu sorunu çözmek için yeni bir kullanıcı Databricks çalışma alanınızı abonelikle içeren dizin oluşturun.

    a. Azure portalında Azure AD'ye gidin. Seçin **kullanıcılar ve gruplar** > **kullanıcı ekleme**.

    b. Bir kullanıcı eklemek bir `@<tenant_name>.onmicrosoft.com` yerine e-posta `@<your_domain>` e-posta. Bu konuda bulabilirsiniz **özel etki alanları**, Azure portalında Azure AD altında.
    
    c. Bu yeni kullanıcı izni **katkıda bulunan** Databricks çalışma Kaynak rolü.
    
    d. Yeni kullanıcı ile Azure portalında oturum açın ve Databricks çalışma alanını bulun.
    
    e. Bu kullanıcı olarak Databricks çalışma alanı başlatın.


### <a name="your-account-email-has-not-been-registered-in-databricks"></a>Hesabınızı {e-posta} Databricks içinde kayıtlı değil 

#### <a name="solution"></a>Çözüm

Çalışma alanı oluşturmadınız ve bir kullanıcı olarak eklenir, çalışma alanı oluşturan kişiye başvurun. Bu kişi Azure Databricks yönetici konsolunu kullanarak Ekle vardır. Yönergeler için bkz: [ekleme ve kullanıcıları yönetme](https://docs.azuredatabricks.net/administration-guide/admin-settings/users.html). Çalışma alanı oluşturduysanız ve bu hatayı almaya devam seçmeyi deneyin **başlatma çalışma** Azure portalından.

### <a name="cloud-provider-launch-failure-while-setting-up-the-cluster"></a>Sağlayıcı başlatma hatası kümeyi ayarlanırken bulut

#### <a name="error-message"></a>Hata iletisi

"Bulut sağlayıcısı başlatma hatası: kümeyi ayarlanırken bir bulut sağlayıcısı hatasıyla karşılaşıldı. Daha fazla bilgi için Databricks Kılavuzu'na bakın. Azure hata kodu: PublicIPCountLimitReached. Azure hata iletisi: Bu bölgede Bu abonelik için birden fazla 60 genel IP adresleri oluşturamıyor. "

#### <a name="solution"></a>Çözüm

Databricks küme düğümü başına bir genel IP adresi kullanın. Aboneliğiniz zaten tüm genel IP'ler kullanıldıysa, gereken [Kotayı artırmak için istek](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request). Seçin **kota** olarak **sorun türü**, ve **ağ: ARM** olarak **kota türü**. İçinde **ayrıntıları**, bir ortak IP adresi kota artışı isteği. Örneğin, sınırınızı şu anda 60'tır ve 100 düğümlü bir küme oluşturmak isterseniz, 160 için sınırın artırılmasını istemek.

### <a name="a-second-type-of-cloud-provider-launch-failure-while-setting-up-the-cluster"></a>Kümeyi ayarlamadan sırasında bulut sağlayıcısı başlatma hatası ikinci türü

#### <a name="error-message"></a>Hata iletisi

"Bulut sağlayıcısı başlatma hatası: kümeyi ayarlanırken bir bulut sağlayıcısı hatasıyla karşılaşıldı. Daha fazla bilgi için Databricks Kılavuzu'na bakın.
Azure hata kodu: MissingSubscriptionRegistration Azure hata iletisi: Abonelik 'Microsoft.Compute' ad alanını kullanmak için kayıtlı değil. Https://aka.MS/RPS-not-found nasıl abonelikleri kaydetmek bkz."

#### <a name="solution"></a>Çözüm

1. [Azure Portal](https://portal.azure.com) gidin.
2. Seçin **abonelikleri**, kullanmakta olduğunuz, abonelik ve ardından **kaynak sağlayıcıları**. 
3. Kaynak sağlayıcıları listesinden karşı **Microsoft.Compute**seçin **kaydetmek**. Kayıt kaynak sağlayıcısı için abonelikte katılımcı veya sahibi rolü olmalıdır.

Daha ayrıntılı yönergeler için bkz: [kaynak sağlayıcıları ve türleri](../azure-resource-manager/resource-manager-supported-services.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Hızlı Başlangıç: Azure Databricks ile çalışmaya başlama](quickstart-create-databricks-workspace-portal.md)
- [Azure Databricks nedir?](what-is-azure-databricks.md)

