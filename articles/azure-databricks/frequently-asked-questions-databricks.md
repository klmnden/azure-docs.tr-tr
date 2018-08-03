---
title: 'Azure Databricks: Sık kullanılan sorular ve Yardım | Microsoft Docs'
description: Sık sorulan sorular ve sorun giderme bilgileri Azure Databricks hakkında yanıtlarını alın.
services: azure-databricks
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: c3ba235c60480c38a21ee3264c54b4a4dcdea340
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39434610"
---
# <a name="frequently-asked-questions-about-azure-databricks"></a>Azure Databricks hakkında sık sorulan sorular

Bu makalede, Azure Databricks için ilgili en sık kullanılan sorgular listelenir. Ayrıca, Databricks kullanırken sahip olabileceğiniz bazı yaygın sorunlar listelenir. Daha fazla bilgi için [Azure Databricks nedir](what-is-azure-databricks.md). 

## <a name="can-i-use-my-own-keys-for-local-encryption"></a>Yerel şifreleme için kendi anahtarlarımı kullanabilir miyim? 
Azure Key vault'tan kendi anahtarlarınızı kullanarak geçerli sürümde desteklenmiyor. 

## <a name="can-i-use-azure-virtual-networks-with-databricks"></a>Databricks ile Azure sanal ağları kullanabilir miyim?
Databricks sağlama bir parçası olarak yeni bir sanal ağ oluşturulur. Bu sürümde, kendi Azure sanal ağı kullanamazsınız.

## <a name="how-do-i-access-azure-data-lake-store-from-a-notebook"></a>Azure Data Lake Store not defterinden nasıl erişebilirim? 

Şu adımları uygulayın:
1. Azure Active Directory'de (Azure AD), bir hizmet sorumlusu sağlayın ve anahtarıyla kaydedin.
1. Data Lake Store içinde hizmet sorumlusuna gerekli izinleri atayın.
1. Data Lake Store dosyasında erişmek için not defterinde hizmet sorumlusu kimlik bilgileri kullanın.

Daha fazla bilgi için [kullanım Data Lake Store ile Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake.html).

## <a name="fix-common-problems"></a>Sık karşılaşılan sorunları giderin

Databricks ile karşılaşabileceğiniz bazı sorunlar aşağıda verilmiştir.

### <a name="issue-this-subscription-is-not-registered-to-use-the-namespace-microsoftdatabricks"></a>Sorun: Bu abonelik 'Microsoft.Databricks' ad alanını kullanacak şekilde kaydedilmemiş

#### <a name="error-message"></a>Hata iletisi

"Bu aboneliği 'Microsoft.Databricks' ad alanını kullanacak şekilde kaydedilmemiş. Bkz: https://aka.ms/rps-not-found abonelikleri kaydedileceğini öğrenmek için. (Code: MissingSubscriptionRegistration) "

#### <a name="solution"></a>Çözüm

1. [Azure Portal](https://portal.azure.com) gidin.
1. Seçin **abonelikleri**, kullanmakta olduğunuz, abonelik ve ardından **kaynak sağlayıcıları**. 
1. Kaynak sağlayıcıları listesinden karşı **Microsoft.Databricks**seçin **kaydetme**. Kaynak sağlayıcısını kaydetmek için Abonelik üzerinde katkıda bulunan veya sahip rolü olmalıdır.


### <a name="issue-your-account-email-does-not-have-the-owner-or-contributor-role-on-the-databricks-workspace-resource-in-the-azure-portal"></a>Sorun: Hesabınızı {email} sahibi veya katkıda bulunan rolü Azure portalında Databricks çalışma alanı kaynağına sahip değil

#### <a name="error-message"></a>Hata iletisi

"Hesabınızı {email} sahibi veya katkıda bulunan rolü Azure portalında Databricks çalışma alanı kaynağına sahip değil. Konuk kullanıcı kiracıda olduğunda bu hatayı da meydana gelebilir. Erişim ya da doğrudan bir Databricks çalışma alanında bir kullanıcı olarak eklediğiniz vermek için yöneticinize başvurun." 

#### <a name="solution"></a>Çözüm

Bu sorunun çözümü birkaç şunlardır:

* Kiracı başlatmak için Konuk kullanıcı olarak değil kiracının normal bir kullanıcı olarak oturum açmanız gerekir. Ayrıca, katkıda bulunan rolü Databricks çalışma alanı kaynağı olmalıdır. Bir kullanıcı erişimine karşı verebilirsiniz **erişim denetimi (IAM)** Azure portalında Databricks çalışma alanınız içine sekmesi.

* Bu hata, e-posta etki alanı adınızı Azure AD'ye için birden çok dizini atanırsa de oluşabilir. Bu sorunu çözmek için Databricks çalışma alanınız abonelikle içeren dizine yeni bir kullanıcı oluşturun.

    a. Azure portalında Azure AD'ye gidin. Seçin **kullanıcılar ve gruplar** > **kullanıcı ekleme**.

    b. Olan bir kullanıcı bir `@<tenant_name>.onmicrosoft.com` yerine e-posta `@<your_domain>` e-posta. Bu seçeneği bulabilirsiniz **özel etki alanları**, Azure portalında Azure AD'ye altında.
    
    c. Bu yeni kullanıcı izni **katkıda bulunan** Databricks çalışma alanı Kaynak rolü.
    
    d. Azure portalında yeni kullanıcı ile oturum açın ve Databricks çalışma alanı bulun.
    
    e. Databricks çalışma alanı, bu kullanıcı olarak başlatın.


### <a name="issue-your-account-email-has-not-been-registered-in-databricks"></a>Sorun: Hesabınızı {email} Databricks'te kaydedilmedi 

#### <a name="solution"></a>Çözüm

Çalışma alanı oluşturma ve bir kullanıcı olarak eklenir, çalışma alanını oluşturan kişiye başvurun. Azure Databricks yönetici konsolunu kullanarak ekleme söz konusu kişinin sahip. Yönergeler için [ekleme ve kullanıcıları yönetme](https://docs.azuredatabricks.net/administration-guide/admin-settings/users.html). Çalışma alanı oluşturulur ve bu hatayı almaya devam ediyorsanız, seçmeyi deneyin **çalışma alanını Başlat** Azure portalından.

### <a name="issue-cloud-provider-launch-failure-while-setting-up-the-cluster-publicipcountlimitreached"></a>Sorun: (PublicIPCountLimitReached) kümesi oluşturma sırasında bulut sağlayıcısı başlatma hatası

#### <a name="error-message"></a>Hata iletisi

"Bulut sağlayıcısı başlatma hatası: küme ayarlanırken bir bulut sağlayıcısı hatasıyla karşılaşıldı. Daha fazla bilgi için Databricks kılavuzuna bakın. Azure hata kodu: PublicIPCountLimitReached. Azure hata iletisi: Bu bölgede Bu abonelik için 60'tan fazla genel IP adresleri oluşturulamıyor. "

#### <a name="solution"></a>Çözüm

Databricks küme düğümü başına bir genel IP adresini kullanın. Aboneliğinizi zaten tüm genel IP'ler kullandıysa, aşağıdakileri yapmalısınız [Kotayı artırmak için istek](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request). Seçin **kota** olarak **sorun türü**, ve **ağ: ARM** olarak **kota türü**. İçinde **ayrıntıları**, bir genel IP adresi kota artışı isteyebilir. Örneğin, sınırınızı 60 şu anda ise ve 100 düğümlü bir küme oluşturmak istediğiniz, sınır artışı 160 için istek.

### <a name="issue-a-second-type-of-cloud-provider-launch-failure-while-setting-up-the-cluster-missingsubscriptionregistration"></a>Sorun: Bir ikinci tür küme (MissingSubscriptionRegistration) ayarlanırken bulut sağlayıcısı başlatma hatası

#### <a name="error-message"></a>Hata iletisi

"Bulut sağlayıcısı başlatma hatası: küme ayarlanırken bir bulut sağlayıcısı hatasıyla karşılaşıldı. Daha fazla bilgi için Databricks kılavuzuna bakın.
Azure hata kodu: MissingSubscriptionRegistration Azure hata iletisi: Abonelik 'Microsoft.Compute' ad alanını kullanacak şekilde kaydedilmemiş. Bkz: https://aka.ms/rps-not-found abonelikleri kaydedileceğini öğrenmek için. "

#### <a name="solution"></a>Çözüm

1. [Azure Portal](https://portal.azure.com) gidin.
1. Seçin **abonelikleri**, kullanmakta olduğunuz, abonelik ve ardından **kaynak sağlayıcıları**. 
1. Kaynak sağlayıcıları listesinden karşı **Microsoft.Compute**seçin **kaydetme**. Kaynak sağlayıcısını kaydetmek için Abonelik üzerinde katkıda bulunan veya sahip rolü olmalıdır.

Daha ayrıntılı yönergeler için bkz. [kaynak sağlayıcıları ve türleri](../azure-resource-manager/resource-manager-supported-services.md).

### <a name="issue-azure-databricks-needs-permissions-to-access-resources-in-your-organization-that-only-an-admin-can-grant"></a>Sorun: Azure Databricks yöneticisinin yalnızca kuruluşunuzdaki kaynaklara erişmek için izinler gerekiyor.

#### <a name="background"></a>Arka plan

Azure Databricks, Azure AD ile tümleşiktir. Bu, Azure databricks'te izinleri (örneğin, dizüstü bilgisayarlar veya kümeleri) ayarlamak Azure AD'den kullanıcı belirterek sağlar. Azure Databricks'ın, Azure ad kullanıcılarının adlarını listelemek bu bilgileri okuma izni gerektirir. Bu, bir onay gerektirir. Onay zaten mevcut değilse hatayı görürsünüz.

#### <a name="solution"></a>Çözüm

Azure portalına genel yönetici olarak oturum açın. Azure Active Directory için Git **kullanıcı ayarları** emin olun ve sekme **kullanıcılar uygulamalara kendileri adına şirket verilerine erişme izni verebilir** ayarlanır **Evet**.

## <a name="next-steps"></a>Sonraki adımlar

- [Hızlı Başlangıç: Azure Databricks ile çalışmaya başlama](quickstart-create-databricks-workspace-portal.md)
- [Azure Databricks nedir?](what-is-azure-databricks.md)

