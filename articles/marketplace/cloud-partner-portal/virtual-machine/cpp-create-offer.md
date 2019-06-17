---
title: Azure Marketi'nde sanal makine teklifi oluşturma
description: Listeleri yeni bir sanal makine (VM) oluşturmak için gerekli adımlar Azure Market sunar.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: article
ms.date: 10/19/2018
ms.author: pabutler
ms.openlocfilehash: 4cd635c6f664a5260b79e62ea72bbb86fc4e1e4f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64938368"
---
# <a name="create-virtual-machine-offer"></a>Sanal makine teklifi oluşturma

Bu bölümde, Azure Market'te yeni bir sanal makine (VM) teklif isteği oluşturmak için gereken adımlar listelenir.  Her teklif, Azure Market'te kendi varlık olarak görünür ve bir veya daha fazla SKU'ları ile ilişkilidir.  VM teklifi, varlıkları ve destekleyici hizmetler aşağıdaki gruplandırmalarını oluşur: 

![Bir sanal makine teklifi için varlıklar](./media/publishvm_002.png)

burada:

|  **Varlık grubu**   |  **Açıklama**  |
|  ---------------   |  ---------------  |
|    SKU'ları            |  En düşük satın alınabilir birimi bir teklif. Tek bir teklifi (ürün sınıfı), desteklenen özellikler, sanal makine görüntü türleri ve faturalama modelleri arasında ayırt etmek için birden çok SKU ile ilişkili olabilir. |
|  Market       | Pazarlama, yasal ve müşteri adayı yönetim varlıkları ve özellikleri içerir.  <ul><li> Teklif adı, açıklama ve logoları pazarlama varlıkları içerir</li> <li> Gizlilik İlkesi, kullanım koşullarını ve diğer yasal belgeler yasal varlıkları içerir</li>  <li> Azure Market Son Kullanıcı Portalı'ndan nasıl işleneceğini belirtmenizi müşteri adayları sağlama yönetim ilkesi sağlar.</li> </ul> |
| Destek            | Destek ilgili kişisi ve ilke bilgilerini içerir |
| Test Sürüşü         | Teklifiniz, satın almadan önce test etmek son kullanıcıların olanak tanıyan varlıklar tanımlayan |
|  |  |


## <a name="new-offer-form"></a>Yeni Teklif formu

Bir kez oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/), tıklayın **+ yeni teklif** üzerinde sol menü öğesi. Ortaya çıkan menüden tıklayarak **sanal makineler** görüntülemek için **yeni teklif** oluşturmak ve yeni bir sanal makine teklifi için varlıklar tanımlayan işlemini başlatın. 
<!-- not all publishers see corevm or azure apps test, you need to be whitelisted to see them. we should hide those in these images. -->

![Yeni sanal makine teklifi kullanıcı arabirimi seçimi](./media/publishvm_003.png)

> [!WARNING]
> Varsa **sanal makineler** seçenek gösterilmez veya etkin değil, ardından hesabınızı bu Teklif türü oluşturma izniniz yok.  Lütfen tüm karşıladığınızdan denetleyin [önkoşulları](./cpp-prerequisites.md) bir geliştirici hesabına kaydolmadan dahil olmak üzere bu Teklif türü için.


## <a name="next-steps"></a>Sonraki adımlar

Bu bölümün sonraki konularda sekmeleri yansıtma **yeni teklif** sayfa (için bir VM Teklif türü).  Her makalede, varlık gruplarını ve yeni VM teklifiniz için destek hizmetleri tanımlamak için ilişkili sekmesini kullanmayı açıklar.

- [Teklif Ayarları sekmesi](./cpp-offer-settings-tab.md)
- [SKU'lar sekmesi](./cpp-skus-tab.md)
- [Test Sürüşü sekmesi](./cpp-test-drive-tab.md)
- [Market sekmesi](./cpp-marketplace-tab.md)
- [Destek sekmesi](./cpp-support-tab.md)
