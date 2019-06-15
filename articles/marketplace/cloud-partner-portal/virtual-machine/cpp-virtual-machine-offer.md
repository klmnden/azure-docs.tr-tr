---
title: Azure marketi'ndeki sanal makine teklifi
description: Azure Marketi'nde bir VM teklifi yayımlama işlemine genel bakış.
services: Azure, Marketplace, Cloud Partner Portal
author: v-miclar
ms.service: marketplace
ms.topic: article
ms.date: 12/04/2018
ms.author: pabutler
ms.openlocfilehash: fed0f47c963edf40883c432f5476bd7fe5720abb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64938052"
---
# <a name="virtual-machine-offer"></a>Sanal makine teklifi

|    |    |
|-----------------------------------------------------------------|------------------------------------------|
| Bu bölümde yeni bir sanal makine teklifi yayımlama açıklanmaktadır [Azure Marketi](https://azuremarketplace.microsoft.com). Hem Windows hem de Linux tabanlı sanal bir işletim sistemi sanal sabit disk (VHD) ve sıfır veya daha fazla veri VHD'leri içeren makineler için destek sağlanır. | ![sanal makine simgesi](./media/virtual-machine-icon.png)  |


## <a name="publishing-overview"></a>Yayımlama genel bakış

Aşağıdaki videoda [en iyi duruma getirme bilgisayarınızı Azure Market teklifi](https://channel9.msdn.com/Events/Build/2017/P4026?ocid=player), kullanıcı deneyimini iyileştirmek nasıl (bir sanal makine çözümü kullanarak), bu Market'te yayımlama da dahil olmak üzere Azure marketi, geniş kapsamlı bir bakış sunar Ürün sayfası ve isteğe bağlı Test Sürüşü deneyimi sayesinde nasıl kullanıcı müşteri adayı oluşturulur ve nasıl bunları tüketme ve müşteri bağlılığı iyileştirin.

> [!VIDEO https://channel9.msdn.com/Events/Build/2017/P4026/player]


## <a name="vm-publishing-process-flow"></a>VM yayımlama işlem akışı

Aşağıdaki diyagramda bir VM teklifi yayımlama üst düzey adımları gösterilmektedir. 

![VM yayımlama işlemi](./media/publishvm_001.png)

1. Teklif - tüm ayrıntılarını oluşturun ve teklif hakkında bilgi, pazarlama materyalleri, yasal, destek bilgileri ve varlık belirtimleri teklif açıklaması dahil olmak üzere yapılandırılır.

2. İş ve teknik varlıkları oluşturma - iş varlıkları (yasal belgeler ve pazarlama malzemeleri) ve ilişkili bir çözüm (burada, Vm'leri ve bağlı diskleri) için teknik varlıkları oluşturun. 

3. SKU oluşturma - teklifi ile ilişkilendirilmiş ilgili fiyatlarını oluşturmak ve bunları gönderin.  Yayımlamayı planlama her görüntü için benzersiz bir SKU gereklidir. 
 
4. Onayla ve - teklifi yayımlama teklif ve teknik varlıkları tamamladıktan sonra teklifi gönderebilirsiniz. Bu gönderi, çözüm, doğrulanmış sertifikalı, ardından "Market'te etkin hale gelir" sınanır yayımlama işlemi başlar.  

## <a name="next-steps"></a>Sonraki adımlar

Bu adımları düşünmeden önce karşılamanız gereken [teknik ve işle ilgili gereksinimleriniz](./cpp-prerequisites.md) bir VM, Microsoft Azure Marketi'nde yayımlama. 
