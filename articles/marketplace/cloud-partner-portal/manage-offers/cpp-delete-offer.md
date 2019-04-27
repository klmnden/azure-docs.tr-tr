---
title: Azure Market Market teklifleri - silme | Microsoft Docs
description: Azure ve bulut iş ortağı portalını kullanarak AppSource Marketlerden teklifler Sil
services: Azure, AppSource, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: pbutlerm
ms.openlocfilehash: 1d5d02d65dd3dcf5978639818fba4ebe36ffaaff
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60825158"
---
# <a name="delete-azure-marketplace-and-appsource-offers-or-skus"></a>Azure Market ve AppSource teklifler veya SKU'ları silme

Çeşitli nedenlerden dolayı teklifinizi iki biçimde olabilir, Microsoft Market'ten geri karar verebilir:

- *Teklif kaldırma* yeni müşteriler artık satın alabilir veya teklifiniz, dağıtma, sağlar ancak kim lisans sözleşmenizi ve ilgili yasalara göre desteklemelidir mevcut müşteriler hiçbir etkisi yoktur. 
- *Teklif sonlandırma* hizmet sonlandırma ve/veya lisans sözleşmesi mevcut müşterileriniz arasındaki işlemidir. 

Yönergeler ve teklif kaldırma ve sonlandırmayla ilgili ilkeleri tarafından yönetilir [Microsoft Market yayımcı anlaşması](https://go.microsoft.com/fwlink/?LinkID=699560) ve [katılım ilkeleri](https://azure.microsoft.com/support/legal/marketplace/participation-policies/) (bölüm [teklifi askıya alma ve kaldırma](https://docs.microsoft.com/en-us/legal/marketplace/participation-policy#offering-suspension-and-removal)). 

Bu makalede Bahsediyor farklı desteklenen silme senaryoları ve her gerçekleştirmek için gerekli adımlar.  

> [!NOTE]
> Yalnızca'i seçerek yayımlanmamış bir teklif silebilirsiniz **Sil** düğme araç **Düzenleyicisi** sekmesi.


## <a name="delete-a-published-sku-from-the-azure-marketplace"></a>Azure Market'te yayımlanmış bir SKU Sil

Aşağıdaki adımları kullanarak Azure Market'te yayımlanmış bir SKU silebilirsiniz:

1.  Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).
2.  İçinde **tüm teklifleri** sayfasında, teklifinizi seçin.  Teklifinizi görüntüleneceğini **Düzenleyicisi** sekmesi.
3.  Sol araç çubuğundaki seçin **SKU'ları** sekmesi. 
4.  ' A tıklayın ve silmek istediğiniz SKU'yu seçin **Sil** düğmesi.
5.  [Yeniden yayımlamanız](./cpp-publish-offer.md) Azure Marketi'nde teklif.

Değiştirilen teklif, Azure Marketi'nde yayımladıktan sonra seçilen SKU artık Azure Market'te hem de Azure portalında listelenir.


## <a name="roll-back-to-a-previous-sku-version"></a>Önceki bir SKU sürüme geri alma

Buradaki adımları kullanarak, Azure Market'te yayımlanmış bir SKU'ın geçerli sürümü silebilirsiniz. İşlem tamamlandığında, SKU önceki sürümüne geri alınır.

1. Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).
2. İçinde **tüm teklifleri** sayfasında, teklifinizi seçin.  Teklifinizi görüntüleneceğini **Düzenleyicisi** sekmesi.
3. Sol araç çubuğundaki seçin **SKU'ları** sekmesi. 
4. En son sürümünü ilişkili çözüm varlık disk sürümler listeden silin.  Teklif türüne bağlı olarak, bu alan olabilir **Disk sürümü**, **paket sürümlerini**, veya benzer bir varlık. 
5. [Yeniden yayımlamanız](./cpp-publish-offer.md) Azure Marketi'nde teklif.

Değiştirilen teklif Market theAzure üzerinde yayımlandıktan sonra listelenen SKU'ın geçerli sürümü artık listelenir. Azure Marketi hem de Azure portalında.  SKU, önceki sürümüne geri alınır.


## <a name="delete-a-live-offer"></a>Canlı bir teklifi Sil

Çeşitli yordam, iş ve canlı bir teklifi kaldırma gereken yasal unsur vardır. Destek ekibinden Canlı teklifini Azure Marketi'nden kaldırma yönergeleri almak için aşağıdaki adımları izleyin:

1.  Kullanarak bir destek bileti yükseltmek [bir olay](https://go.microsoft.com/fwlink/?linkid=844975) tıklayabilir veya sayfa **Destek** sağ alt köşesindeki [bulut iş ortağı portalı](https://cloudpartner.azure.com/).

2.  Belirli teklif türünüzü seçin **sorun türü** listesinde ve seçin **yayımlanan bir teklifi kaldırabilir** içinde **kategori** listesi.

3.  İstek gönderin.

Destek ekibine teklif silme işleminde size kılavuzluk eder.

> [!NOTE]
> Bu teklif (veya SKU) geçerli satın alma işlemleri bir teklif (veya SKU) silme etkilemez. Bu satın alma işlemleri, önceki gibi çalışmaya devam eder. Ancak, silinen teklifler veya SKU'ları sonraki tüm satın almalarda kullanılabilir olmayacaktır.


## <a name="next-steps"></a>Sonraki adımlar

Teklifler yönetmek için kullanılan temel işlemleri ile ilgili bilgi sahibi olduktan sonra Microsoft bir örneğini oluşturmak hazır olursunuz [Market teklifi](../cpp-marketplace-offers.md).
