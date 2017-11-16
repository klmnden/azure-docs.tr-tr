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
ms.date: 11/15/2017
ms.author: nitinme
ms.openlocfilehash: b6b001087cba5f8550d4fea3e4a2f7c1c865beae
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="azure-databricks-preview-common-questions-and-help"></a>Azure Databricks önizleme: Ortak sorular ve Yardım

Bu makalede Azure Databricks sahip olabileceğiniz sorguları ilişkili üst listelenmektedir. Ayrıca, Azure Databricks kullanırken içine çalışabilir bazı ortak sorunlar listelenmiştir. Azure Databricks hakkında daha fazla bilgi için bkz: [Azure Databricks nedir?](what-is-azure-databricks.md) 

## <a name="common-questions"></a>Sık sorulan sorular

Bu bölümde Azure Databricks ilgili ortak sorular listelenir.

### <a name="can-i-use-my-own-keys-for-local-encryption"></a>Kendi anahtarları yerel şifreleme için kullanabilir miyim? 
Azure Keyvault kendi anahtarları kullanarak geçerli sürümde desteklenmiyor. 

### <a name="can-i-use-azure-vnets-with-azure-databricks"></a>Azure sanal ağlar ile Azure Databricks kullanabilir miyim?
Yeni bir VNET Azure Databricks sağlama bir parçası olarak oluşturulur. Bu sürümde bir parçası olarak, Azure sanal kullanamazsınız.

### <a name="how-do-i-access-azure-data-lake-store-from-a-notebook"></a>Bir dizüstü bilgisayarınızı Azure Data Lake Store nasıl erişirim? 

1. Azure Active Directory'de Hizmet sorumlusu sağlamak ve anahtarıyla kaydedin.
2. İçin hizmet sorumlusu Azure Data Lake Store'da gerekli izinleri atayın.
3. Azure Data Lake Store bir dosyaya erişmek için not defterinde hizmet asıl kimlik bilgilerini kullanın.

Daha fazla bilgi için bkz: [kullanım Data Lake Store ile Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-storage.html#azure-data-lake-store).

## <a name="troubleshooting"></a>Sorun giderme

Bu bölümde Azure Databricks ile ortak sorunlarının nasıl giderileceği açıklanmaktadır.

### <a name="issue-your-account-email-does-not-have-owner-or-contributor-role-on-the-databricks-workspace-resource-in-the-azure-portal"></a>Sorun: Hesabınızı {e-posta} sahibi veya katkıda bulunan rolü Azure portalında Databricks çalışma kaynakta yok.

**Hata iletisi**

Hesabınızı {e-posta} sahibi veya katkıda bulunan rolü Azure portalında Databricks çalışma kaynakta yok. Konuk kullanıcı Kiracı olarak varsa bu hatayı da oluşabilir. Erişim ya da doğrudan Databricks çalışma alanında bir kullanıcı olarak eklediğiniz vermek için yöneticinize başvurun. 

**Çözüm**

Kiracı başlatmak için normal bir Konuk kullanıcı Kiracı Kullanıcı oturum açmanız gerekir. Ayrıca katkıda bulunan rolü Databricks çalışma kaynak üzerinde sahip olmalıdır. Bir kullanıcının erişim verebilirsiniz **erişim denetimi (IAM)** Azure portalında Azure Databricks çalışma alanınızı sekmede.

### <a name="issue-your-account-email-has-not-been-registered-in-databricks"></a>Sorun: Hesabınızı {e-posta} Databricks içinde kayıtlı değil 

**Çözüm**

Çalışma alanı oluşturmadınız ve çalışma alanında bir kullanıcı olarak eklenir, Azure Databricks Yönetici Konsolu'nu kullanarak eklemek için çalışma alanı oluşturulduğunda kişiye başvurun. Yönergeler için bkz: [ekleme ve kullanıcıları yönetme](https://docs.azuredatabricks.net/administration-guide/admin-settings/users.html). Çalışma alanı oluşturduysanız ve bu hatayı almaya devam "Çalışma alanı başlatılamıyor" Azure portalından yeniden tıklatmayı deneyin.

### <a name="issue-cloud-provider-launch-failure-a-cloud-provider-error-was-encountered-while-setting-up-the-cluster"></a>Sorun: Bulut sağlayıcısı başlatma hatası: kümeyi ayarlanırken bir bulut sağlayıcısı hatasıyla karşılaşıldı.

**Hata iletisi**

Bulut sağlayıcısı başlatma hatası: Kümeyi ayarlanırken bir bulut sağlayıcısı hatasıyla karşılaşıldı. Daha fazla bilgi için Databricks Kılavuzu'na bakın. Azure hata kodu: PublicIPCountLimitReached. Azure hata iletisi: Bu bölgede Bu abonelik için birden fazla 60 genel IP adresleri oluşturulamıyor.

**Çözüm**

Azure Databricks küme düğümü başına bir genel IP adresi kullanın. Aboneliğiniz zaten tüm genel IP'ler kullanıldıysa, gereken [Kotayı artırmak için istek](https://docs.microsoft.com/en-us/azure/azure-supportability/resource-manager-core-quotas-request). Seçin **kota** olarak **sorun türü**, **ağ ARM** olarak **kota türü**ve içindebirgenelIPadresikotaartışıisteği **Ayrıntılar** (örneğin, istek tarihi sınırınızı şu anda 60'tır ve 100 düğümlü bir küme oluşturmak isterseniz, sınırın artırılmasını 160 için).

## <a name="next-steps"></a>Sonraki adımlar
2. sürümün data factory oluşturmak adım adım yönergeler için aşağıdaki öğreticiler bakın:

- [Hızlı Başlangıç: Azure Databricks ile çalışmaya başlama](quickstart-create-databricks-workspace-portal.md)
- [Azure Databricks nedir?](what-is-azure-databricks.md)

