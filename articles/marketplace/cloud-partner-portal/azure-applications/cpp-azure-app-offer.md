---
title: Azure uygulama teklifi | Microsoft Docs
description: Bir Azure uygulaması yayımlama işlemine genel bakış Azure Marketi'nde sunar.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 02/06/2019
ms.author: pbutlerm
ms.openlocfilehash: 82d072cc6f86ae758bd0fdd4d02b68b1ac1de53a
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56097159"
---
# <a name="azure-application-offer"></a>Azure uygulama teklifi

|    |    |
|-----------------------------------------------------------------|------------------------------------------|
| <div class="body"> Bu bölümde yeni bir Azure uygulama teklifi yayımlama açıklanmaktadır [Azure Marketi](https://azuremarketplace.microsoft.com).  Azure her uygulama, genellikle bir veya daha fazla sanal makine ve diğer destekleyici Azure veya Web tabanlı hizmetleri içeren uygulama tarafından kullanılan tüm teknik varlıklar tanımlayan bir Azure Resource Manager şablonu içerir. Tüm Azure app teklifleri aracılığıyla erişim güvenliği etkinleştirmelisiniz [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/).  </div> | ![Azure uygulama simgesi](./media/azureapp-icon1.png)  |

## <a name="publishing-overview"></a>Yayımlama genel bakış

Aşağıdaki videoda [yapı çözüm şablonları ve yönetilen uygulamalar, Azure Market'te](https://channel9.msdn.com/Events/Build/2018/BRK3603), giriş niteliğindedir: ne teklif türleri kullanılabilir, teknik varlıkları nelerdir gerekli, nasıl Azure Resource Manager Yazar geliştirme ve test kullanıcı Arabirimi, uygulama şablonu, uygulama teklifini ve uygulama İnceleme süreci yayımlama.

>[!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK3603/player]


## <a name="types-of-azure-applications"></a>Azure uygulama türleri

Azure uygulamaları iki tür vardır: yönetilen uygulamaları ve çözüm şablonları. 

- Çözüm şablonları Market'te çözüm yayımlama için temel yollarından biridir. Çözümünüzü ek dağıtım ve yapılandırma tek bir sanal makine (VM) otomasyonundan gerektirdiğinde bu Teklif türü kullanılır. Bir çözüm şablonu kullanarak birden fazla VM sağlama otomatik hale getirebilirsiniz. Bu Otomasyon karmaşık Iaas çözümleri sağlamak için ağ ve depolama kaynaklarının sağlanmasını içerir. Çözüm şablonu gereksinimleri ve faturalandırma modeli genel bakış için bkz: [Azure uygulamalarını: Çözüm şablonları](https://docs.microsoft.com/azure/marketplace/marketplace-solution-templates).

- Yönetilen uygulamalar, çözüm şablonları, önemli bir fark ile benzerdir. Yönetilen bir uygulamada kaynaklar, uygulamanın yayımcısı tarafından yönetilen bir kaynak grubuna dağıtılır. Kaynak grubu, tüketicinin aboneliğinde mevcuttur ancak yayımcının kiracısındaki bir kimlik, kaynak grubuna erişime sahiptir. Yayımcı olarak, çözümün sürekli desteği için maliyeti belirtirsiniz. Azure yönetilen uygulamaları kolayca oluşturup müşterilerinize tam olarak yönetilen, kullanıma hazır uygulamalar sunmak için kullanır.

Azure Marketi ek olarak, hizmet Kataloğu yönetilen uygulamalarda sunabilir. Hizmet kataloğu, bir kuruluştaki kullanıcılar için onaylı çözümleri içeren bir dahili katalogdur. Katalog grupları bir kuruluşta çözümleri sunmaya devam ederken kuruluş standartları karşılamak üzere kullanın. Çalışanlar, kataloğu kullanarak BT bölümleri tarafından önerilen ve onaylanan uygulamaları kolayca bulabilir.

Avantajları ve yönetilen uygulama türleri hakkında daha fazla bilgi için bkz: [Azure yönetilen uygulamalara genel bakış](https://docs.microsoft.com/azure/managed-applications/overview).


## <a name="publishing-process-workflow"></a>Yayımlama işlemi iş akışı

Aşağıdaki diyagramda bir Azure uygulaması teklif yayımlamak için üst düzey bir işlem gösterilir.

![Yayımlama teklif için iş akışı](./media/new-offer-process.png)

Bir Azure uygulaması teklif yayımlamak için üst düzey adımlar şunlardır:

0. Karşılamak [önkoşulları](./cpp-prerequisites.md) - (gösterilmez) iş ve bir Azure uygulamasına Azure Market'te yayımlama için teknik gereksinimlerini karşıladığınızdan emin olun. 

1. [Teklif oluşturma](./cpp-create-offer.md) -teklifi hakkında ayrıntılı bilgi sağlar. Bu bilgiler içerir: pazarlama malzemeleri, destek bilgileri ve varlık belirtimleri teklif açıklaması.

2. [Oluşturun veya var olan iş ve teknik varlıkları toplamak](./cpp-create-technical-assets.md) -iş varlıkları (yasal belgeler ve pazarlama malzemeleri) ve ilişkili çözümü için teknik varlıkları oluşturun.

3. [SKU oluşturma](./cpp-skus-tab.md) -teklifle ilgili fiyatlarını oluşturun. Yayımlama planlıyorsanız her görüntü için benzersiz bir SKU gereklidir.

4. Onayla ve [teklifi yayımlama](./cpp-publish-offer.md) -teklif ve teknik varlıkları tamamlandıktan sonra teklifi gönderebilirsiniz. Bu gönderi, yayımlama işlemi başlar. Bu işlem sırasında çözümü, doğrulanmış sertifikalı, ardından "Azure Marketi'nde etkin hale gelir" test edilir.


## <a name="next-steps"></a>Sonraki adımlar

Bu adımları düşünmeden önce karşılamanız gereken [teknik ve işle ilgili gereksinimleriniz](./cpp-prerequisites.md) Microsoft Azure Market'te bir yönetilen uygulama yayımlama.
