---
title: Azure marketi'ndeki sanal makine teklifi | Microsoft Docs
description: Azure Marketi'nde bir VM teklifi yayımlama işlemine genel bakış.
services: Azure, Marketplace, Cloud Partner Portal
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 09/28/2018
ms.author: pbutlerm
ms.openlocfilehash: d3682d18fb849b2d851bae0986f9e61f216aaf2c
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2018
ms.locfileid: "49639946"
---
# <a name="virtual-machine-offer"></a>Sanal makine teklifi

Bu bölümde, bir sanal Makine'de (VM) yayımlama öğeleri özetler ve yayımcı için bir kılavuz olarak tasarlanmıştır [Azure Marketi](https://azuremarketplace.microsoft.com).  Bu bakış açısından, aşağıdaki ana bölümlere ayrılmıştır:

- [Önkoşullar](./cpp-prerequisites.md) -oluşturma veya VM teklifi yayımlama önce teknik ve işle ilgili gereksinimleriniz listeler
- [VM teklifi oluşturma](./cpp-create-offer.md) -listeleri yeni bir VM oluşturmak için gerekli adımlar teklif girişini kullanarak [bulut iş ortağı portalı](https://cloudpartner.azure.com)
- [VM teknik varlıkları oluşturma](./cpp-create-technical-assets.md) - teknik oluşturmak için bir VM çözümü ve bu paket VM olarak nasıl yapılandıracağınızı öğrenmek için varlıklar Azure Marketi'nde teklif nasıl
- [VM teklifi yayımlama](./cpp-publish-offer.md) -teklifi Azure Marketi yayımlama için gönderme


## <a name="vm-publishing-process-flow"></a>VM yayımlama işlem akışı

Aşağıdaki diyagramda bir VM teklifi yayımlama üst düzey adımları gösterilmektedir. 

![VM yayımlama işlemi](./media/publishvm_001.png)

1. Teklif - tüm ayrıntılarını oluşturun ve teklif hakkında bilgi, pazarlama materyalleri, yasal, destek bilgileri ve varlık belirtimleri teklif açıklaması dahil olmak üzere yapılandırılır.

2. İş ve teknik varlıkları oluşturma - iş varlıkları (yasal belgeler ve pazarlama malzemeleri) ve ilişkili bir çözüm (burada, Vm'leri ve bağlı diskleri) için teknik varlıkları oluşturun. 

3. SKU oluşturma - teklifi ile ilişkilendirilmiş ilgili fiyatlarını oluşturmak ve bunları gönderin.  Yayımlamayı planlama her görüntü için benzersiz bir SKU gereklidir. 
 
4. Onayla ve - teklifi yayımlama teklif ve teknik varlıkları tamamladıktan sonra teklifi gönderebilirsiniz. Bu gönderi, çözüm, doğrulanmış sertifikalı, ardından "Market'te etkin hale gelir" sınanır yayımlama işlemi başlar.  

## <a name="next-steps"></a>Sonraki adımlar

Bu adımları düşünmeden önce karşılamanız gereken [teknik ve işle ilgili gereksinimleriniz](./cpp-prerequisites.md) bir VM, Microsoft Azure Marketi'nde yayımlama. 
