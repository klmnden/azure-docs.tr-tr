---
title: Azure Marketi'nden bir teklifini/SKU'yu silme
description: Azure Marketi'nden bir teklifini/SKU'yu silme
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: qianw211
manager: pbutlerm
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 02b4ad8aea3904ba0e99a5ea4d4d035f970e171a
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48811759"
---
<a name="delete-an-offersku-from-azure-marketplace"></a>Azure Marketi'nden bir teklifini/SKU'yu silme 
==========================================

Çeşitli nedenlerden dolayı teklifinizi Market’ten kaldırmaya karar verebilirsiniz. Yeni müşteriler artık satın alan veya teklifiniz dağıtmak, ancak mevcut müşteriler etkilenmeyecektir.
Teklif Sonlandırma, sizinle mevcut müşterileriniz arasındaki hizmet ve/veya lisans sözleşmesini iptal etme işlemidir. Yönergeler ve teklif kaldırma ve sonlandırmayla ilgili ilkeleri tarafından yönetilir [Microsoft Market yayımcı anlaşması](http://go.microsoft.com/fwlink/?LinkID=699560) (bölümüne bakın.
7) ve [katılım ilkeleri](https://azure.microsoft.com/support/legal/marketplace/participation-policies/) (Bölüm 6.2 bakın). Desteklenen farklı senaryolar silin ve uygulayabileceğiniz adımları ele alınmıştır.

<a name="delete-a-live-sku-from-azure-marketplace"></a>Azure Marketi'nden bir canlı SKU Sil 
----------------------------------------

Aşağıdaki adımları izleyerek Azure Marketi'nden bir canlı SKU silebilirsiniz:

1.  Oturum [bulut iş ortağı portalı](http://cloudpartner.azure.com).
2.  Teklifinizi tüm teklifler sekmesinden seçin.
3.  Ekranın sol tarafındaki bölmede SKU'ları sekmesini seçin.
4.  Bu SKU için Sil düğmesine tıklayın ve silmek istediğiniz SKU'yu seçin.
5.  Azure Marketi'nde teklif yeniden yayımlayın.

Azure Marketi'nde teklif Canlı olduktan sonra Azure Market ve Azure portalından SKU silinir.

<a name="roll-back-to-a-previous-sku-version"></a>Önceki bir SKU sürüme geri alma 
----------------------------------

Buradaki adımları izleyerek Azure Marketi'nden bir canlı SKU'ın geçerli sürümü silebilirsiniz. İşlem tamamlandığında, SKU önceki sürümüne geri alınacak.

1.  Oturum [bulut iş ortağı portalı](http://cloudpartner.azure.com).
2.  Teklifinizi tüm teklifler sekmesinden seçin.
3.  Ekranın sol tarafındaki bölmede SKU'ları sekmesini seçin.
4.  En son Paket sürümü yayımlanmış paketleri listesinden silin.
5.  Azure Marketi'nde teklif yeniden yayımlayın.

Azure Marketi'nde teklif Canlı olduktan sonra listelenen SKU'ın geçerli sürümü, Azure Market ve Azure portalından silinir.
SKU, önceki sürümüne geri alınacak.

<a name="delete-a-live-offer"></a>Canlı bir teklifi Sil 
-------------------

Lütfen destek ekibinden Canlı teklifini Azure Marketi'nden kaldırma yönergeleri almak için aşağıdaki adımları izleyin:

1.  Bunu kullanarak bir destek bileti yükseltmek [bağlantı](https://go.microsoft.com/fwlink/?linkid=844975)
2.  Sorun seçin yönetme teklif listesi, üretim Kategori listesinden bir teklif ve/veya SKU'ın zaten değiştirme yazın.
3.  İstek gönderin.

Destek ekibine teklif silme işleminde size yol gösterecektir.

Bir SKU/teklif silinmesini zaten yapmış SKU/teklifi satın alımlarında geçerli etkilemez. Önceki gibi çalışmaya devam eder, ancak bundan sonra tüm yeni satın alımları için kullanılamaz.
