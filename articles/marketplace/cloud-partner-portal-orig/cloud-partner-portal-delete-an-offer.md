---
title: Azure Marketi'nden bir teklif veya SKU silme | Microsoft Docs
description: Bir teklif veya SKU silmek için adımlar.
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
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: cc172e35e8964fad3b1a1410d1f1f3240c423ab3
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48811448"
---
<a name="delete-an-offer-or-sku-from-azure-marketplace"></a>Azure Marketi'nden bir teklif veya SKU Sil
==========================================

Çeşitli nedenlerden dolayı teklifinizi Market’ten kaldırmaya karar verebilirsiniz. Teklif Kaldırma işlemi yeni müşterilerin teklifinizi artık satın alamamasını veya dağıtamamasını sağlar, ancak mevcut müşterileri etkilemez.
Teklif Sonlandırma, sizinle mevcut müşterileriniz arasındaki hizmet ve/veya lisans sözleşmesini iptal etme işlemidir. Yönergeler ve teklif kaldırma ve sonlandırmayla ilgili ilkeleri tarafından yönetilir [Microsoft Market yayımcı anlaşması](http://go.microsoft.com/fwlink/?LinkID=699560) (bölümüne bakın.
7) ve [katılım ilkeleri](https://azure.microsoft.com/support/legal/marketplace/participation-policies/) (Bölüm 6.2 bakın). Bu makalede Bahsediyor farklı desteklenen senaryolar ve bunlar için atabileceğiniz adımları silin.

<a name="delete-a-live-sku-from-azure-marketplace"></a>Azure Marketi'nden bir canlı SKU Sil
----------------------------------------

Aşağıdaki adımları kullanarak Azure Market'ten bir canlı SKU silebilirsiniz:

1.  Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).

2.  Teklifinizi gelen seçin **tüm teklifleri** sekmesi.

3.  Ekranın sol tarafındaki bölmede seçin **SKU'ları** sekmesi.

4.  Bu SKU için Sil düğmesine tıklayın ve silmek istediğiniz SKU'yu seçin.

5.  [Yeniden yayımlamanız](./cloud-partner-portal-make-offer-live-on-Azure-Marketplace.md) Azure Marketi'nde teklif.

Azure Marketi'nde teklif Canlı olduktan sonra Azure Market ve Azure portalından SKU silinir.

<a name="roll-back-to-a-previous-sku-version"></a>Önceki bir SKU sürüme geri alma
----------------------------------

Buradaki adımları izleyerek Azure Marketi'nden bir canlı SKU'ın geçerli sürümü silebilirsiniz. İşlem tamamlandığında, SKU önceki sürümüne geri alınacak.

1.  Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).

2.  Teklifinizi gelen seçin **tüm teklifleri** sekmesi.

3.  Ekranın sol tarafındaki bölmede seçin **SKU'ları** sekmesi.

4.  Sonuncuyu Sil **Disk sürümü** yayımlanan disk sürümleri listesinden.

5.  [Yeniden yayımlamanız](./cloud-partner-portal-make-offer-live-on-Azure-Marketplace.md) Azure Marketi'nde teklif.

Azure Marketi'nde teklif Canlı olduktan sonra listelenen SKU'ın geçerli sürümü, Azure Market ve Azure portalından silinir.
SKU, önceki sürümüne geri alınacak.

<a name="delete-a-live-offer"></a>Canlı bir teklifi Sil
-------------------

Canlı bir teklifin kaldırılması için istekte durumunda dikkate edilmesi gereken çeşitli unsur vardır. Lütfen destek ekibinden Canlı teklifini Azure Marketi'nden kaldırma yönergeleri almak için aşağıdaki adımları izleyin:

1.  Bunu kullanarak bir destek bileti yükseltmek [bağlantı](https://go.microsoft.com/fwlink/?linkid=844975), veya destek bulut iş ortağı portalının sağ alt köşesinde tıklayarak.

2.  Belirli teklif türünüzü seçin **sorun türü** listesi ve **yayımlanan bir teklifi kaldırabilir** içinde **kategori** listesi.

3.  İstek gönderin.

Destek ekibine teklif silme işleminde size yol gösterecektir.

>[!NOTE]
>SKU/teklifi satın alımlarında geçerli bir SKU/teklif siliniyor etkilemez. Bu SKU'ları / teklifleri, önceki gibi çalışmaya devam eder. Ancak, bunlar ileride yeni satın alımlarda kullanılabilir olmayacaktır.
