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
ms.date: 12/07/2018
ms.author: pbutlerm
ms.openlocfilehash: 63b7ee4e0d9cb9d0d1f26119fe73573b124d04e8
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53197337"
---
# <a name="azure-application-offer"></a>Azure uygulama teklifi

Bu bölümde, Microsoft yeni bir Azure uygulaması teklif yayımlamak açıklanmaktadır <a href="https://azuremarketplace.microsoft.com">Azure Marketi</a>.
|    |    |
|-----------------------------------------------------------------|------------------------------------------|
| Azure her uygulama, genellikle bir veya daha fazla sanal makine ve diğer destekleyici Azure veya Web tabanlı hizmetleri içeren uygulama tarafından kullanılan tüm teknik varlıklar tanımlayan bir Azure Resource Manager şablonu içerir. | ![Azure uygulama simgesi](./media/azureapp-icon1.png)  |

## <a name="benefits"></a>Avantajlar

Uygulamalarınızı Microsoft Marketi'nde listeleme avantajlarından bazıları şunlardır:

* 100 milyon Azure Active Directory Kullanıcıları, Office 365 ve Dynamics 365 üzerinden ulaşma.

* Satış ekibinize genişletme: Dünya çapındaki iş kullanıcılarına ulaşmak ve ilgilenir. son kullanıcılar, müşteri adayları oluşturmanıza yardımcı olur ve yeni müşteriler olan sektörler arasında başlatan bir satış kanalını elde edin.

* Eyleme dönüştürülebilir Öngörüler alınıyor: biz uygulamanızı Appsource'ta nasıl performans gösterdiğini, ne kadar iyi çalışır ve satış yordamlarınızın daha da geliştirmek nasıl Öngörüler paylaşır.

## <a name="types-of-azure-applications"></a>Azure uygulama türleri

Azure uygulamaları iki tür vardır: bir yönetilen uygulamayı ve bir çözüm şablonu. Benzer olsa da, bazı önemli farklılıklar vardır.

### <a name="solution-template"></a>Çözüm şablonu

Çözüm şablonları Market'te çözüm yayımlama için temel yollarından biridir. Çözümünüzü ek dağıtım ve yapılandırma tek bir sanal makine (VM) otomasyonundan gerektirdiğinde bu Teklif türü kullanılır. Bir çözüm şablonu kullanarak birden fazla VM sağlama otomatik hale getirebilirsiniz. Bu, karmaşık Iaas çözümleri sağlamak için ağ ve depolama kaynaklarının sağlanmasını içerir. Çözüm şablonu gereksinimleri ve faturalandırma modeli genel bakış için bkz: [Azure uygulamalarını: Çözüm şablonları](https://docs.microsoft.com/azure/marketplace/marketplace-solution-templates).

### <a name="managed-application"></a>Yönetilen uygulama

Yönetilen bir uygulama, tek bir önemli farkla, Market’teki bir çözüm şablonuna benzer. Yönetilen bir uygulamada kaynaklar, uygulamanın yayımcısı tarafından yönetilen bir kaynak grubuna dağıtılır. Kaynak grubu, tüketicinin aboneliğinde mevcuttur ancak yayımcının kiracısındaki bir kimlik, kaynak grubuna erişime sahiptir. Yayımcı olarak, çözümün sürekli desteği için maliyeti belirtirsiniz. Azure yönetilen uygulamaları kolayca oluşturup müşterilerinize tam olarak yönetilen, kullanıma hazır uygulamalar sunmak için kullanır.

Market ek olarak, hizmet Kataloğu yönetilen uygulamalarda sunabilir. Hizmet kataloğu, bir kuruluştaki kullanıcılar için onaylı çözümleri içeren bir dahili katalogdur. Katalog grupları bir kuruluşta çözümleri sunmaya devam ederken kuruluş standartları karşılamak üzere kullanın. Çalışanlar, kataloğu kullanarak BT bölümleri tarafından önerilen ve onaylanan uygulamaları kolayca bulabilir.

Avantajları ve yönetilen uygulama türleri hakkında daha fazla bilgi için bkz: [Azure yönetilen uygulamalara genel bakış](https://docs.microsoft.com/azure/managed-applications/overview).

## <a name="publishing-overview"></a>Yayımlama genel bakış

Aşağıdaki videoda [yapı çözüm şablonları ve yönetilen uygulamalar, Azure Market'te](https://channel9.msdn.com/Events/Build/2018/BRK3603), bir Azure uygulama çözümü ve sonra nasıl tanımlamak için bir Azure Resource Manager şablonu yazmak nasıl bir genel bakıştır Daha sonra uygulama teklifi Azure Marketinde yayımlama.

>[!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK3603/player]

## <a name="publishing-process-workflow"></a>Yayımlama işlemi iş akışı

Aşağıdaki diyagramda bir Azure uygulaması teklif yayımlamak için üst düzey bir işlem gösterilir.

![Yayımlama teklif için iş akışı](./media/new-offer-process.png)

## <a name="offer-components"></a>Teklif bileşenleri

Bu bölümde, bir yönetilen uygulama teklif yayımlamanın öğeleri özetler ve Azure Marketi yayımcı için bir kılavuz olarak tasarlanmıştır. Yayımlama'nın aşağıdaki ana bölümlere: 

* [Önkoşullar](./cpp-prerequisites.md) -oluşturma veya bir yönetilen uygulama teklifini yayımlamanın önce teknik ve iş gereksinimleri listelenir. 
* [Teklif oluşturma](./cpp-create-offer.md) -bulut iş ortağı portalını kullanarak bir yönetilen uygulama Teklif girişi oluşturmak için gerekli adımları sağlar. 
* [Teklifi yayımlama](./cpp-publish-offer.md)-teklifi Azure Marketi yayımlama için gönderme işlemini açıklamaktadır.

## <a name="steps-in-the-publishing-process"></a>Yayımlama işlemi adımları

Bir Azure uygulaması teklif yayımlamak için üst düzey adımlar şunlardır:

1. Teklif oluşturma - teklifi hakkında ayrıntılı bilgi sağlar. Bu bilgiler içerir: pazarlama malzemeleri, destek bilgileri ve varlık belirtimleri teklif açıklaması.

2. İş ve teknik varlıkları oluşturma - iş varlıkları (yasal belgeler ve pazarlama malzemeleri) ve ilişkili çözümü için teknik varlıkları oluşturun.

3. SKU Oluştur - teklifle ilgili fiyatlarını oluşturun. Yayımlama planlıyorsanız her görüntü için benzersiz bir SKU gereklidir.

4. Onayla ve - teklifi yayımlama teklif ve teknik varlıkları tamamlandıktan sonra teklifi gönderebilirsiniz. Bu gönderi, yayımlama işlemi başlar. Bu işlem sırasında çözümü, doğrulanmış sertifikalı, ardından "Azure Marketi'nde etkin hale gelir" test edilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu adımları düşünmeden önce karşılamanız gereken [teknik ve işle ilgili gereksinimleriniz](./cpp-prerequisites.md) Microsoft Azure Market'te bir yönetilen uygulama yayımlama.