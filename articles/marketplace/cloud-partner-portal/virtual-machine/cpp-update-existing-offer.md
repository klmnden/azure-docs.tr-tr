---
title: Azure Marketi'nde bulunan mevcut bir VM teklifi güncelleştirme
description: Mevcut bir VM teklifi Azure Marketi'nde güncelleştirme açıklanmaktadır.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: article
ms.date: 08/27/2018
ms.author: Ankit.Sud
ms.openlocfilehash: 4a75d706d55512201786b2b74376047ff75380a7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64938132"
---
# <a name="update-an-existing-vm-offer-on-azure-marketplace"></a>Azure Marketi'nde mevcut bir VM teklifi güncelleştirme

Bu makale, sanal makine (VM) teklifiniz güncelleştirme farklı yönlerini size [bulut iş ortağı portalı](https://cloudpartner.azure.com/) ve sonra teklifin yeniden yayımlanması. 

Teklifiniz, güncelleştirmenizi sıradan nedenlerle birçok dahil olmak üzere:

-  Mevcut SKU'lara yeni bir VM görüntüsü sürüm ekleme
-  Değişiklik bölgeleri SKU kullanılabilir
-  Yeni SKU'ları Ekle
-  Market teklifine veya bireysel SKU'ları meta verilerini güncelleştir
-  Kullandıkça Öde teklifleri üzerinde fiyatlandırmasını güncelleştir

Bu değişiklikler yardımcı olmak için portal sunar **karşılaştırma** ve **geçmişi** özellikleri.  

>[!Note]
>Bulut çözümü sağlayıcıları (CSP) iş ortağı kanalı katılımı kullanıma sunuldu.  Lütfen [bulut çözüm sağlayıcıları](../../cloud-solution-providers.md) teklifinizi Microsoft CSP aracılığıyla pazarlama hakkında daha fazla bilgi için iş ortağı kanalı.

## <a name="unpermitted-changes-to-vm-offer-or-sku"></a>VM teklifi veya SKU unpermitted değişiklikler

Bir VM teklifi veya teklif Azure Market'teki çoğunlukla Canlı olduğunda değiştirilemez SKU'sunu bazı öznitelikleri vardır:

-  **Teklif kimliği** ve **yayımcı kimliği** teklifi
-  **SKU kimliği** mevcut SKU'lar
-  Veri diski sayısı mevcut SKU'ları
-  Mevcut SKU'lara faturalandırma/lisans modeli değiştirir
-  Yayımlanan bir SKU'ya fiyat artırır


## <a name="common-update-operations"></a>Genel güncelleştirme işlemleri

Çok çeşitli özellikler üzerinde bir VM teklifi değiştirebilirsiniz olsa da, aşağıdaki işlemleri yaygındır.

### <a name="update-the-vm-image-version-for-a-sku"></a>VM görüntü sürümü için SKU güncelleştirme

Güvenlik düzeltme ekleri, ek özellikler ve benzeri düzenli olarak güncelleştirilmesi için bir VM görüntüsü yaygındır.  Böyle senaryolarda, aşağıdaki adımları kullanarak, SKU'nuzu başvuran VM görüntüsünü güncelleştirmek istediğiniz:

1.  Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).

2.  Altında **tüm teklifleri**, güncelleştirme teklifini bulun.

3.  İçinde **SKU'ları** sekmesini ve ardından güncelleştirmek için VM görüntüsü ile ilişkilendirilen SKU üzerinde.

4.  Altında **Disk sürümü**, tıklayarak **+ yeni Disk sürümü** yeni bir VM görüntüsü eklemek için.

5.  Yeni VM görüntüleri **Disk sürümü**. Disk sürümü izlemesi gereken [semantik sürüm](https://semver.org/) biçimi. Sürümleri X.Y.Z, burada X, Y ve Z tamsayılardır biçiminde olmalıdır. Sağladığınız yeni sürümü önceki sürümlerin tümü büyük olduğundan emin olun; Aksi takdirde yeniden yayımlanması sonra yeni sürümü portalında veya Azure Marketi'nde görüntülenmez.

6.  İçin **işletim sistemi VHD URL'si**, girin [paylaşılan erişim imzası (SAS) URI](./cpp-get-sas-uri.md) işletim sistemi VHD'si için oluşturulan. 

    > [!WARNING] 
    > Veri diski sayısı SKU farklı sürümleri arasında geçiş yapamazsınız. Önceki sürümler yapılandırılmış veri diskleri varsa, bu yeni sürümü de aynı sayıda veri diski olması gerekir.

7.  Tıklayarak **Yayımla** yeni VM sürümü, Azure Marketi'nde içerik yayımlamak için iş akışını başlatmak için.


### <a name="change-region-availability-of-a-sku"></a>Bir SKU'ların değişiklik bölge kullanılabilirliği

Zaman içinde daha fazla bölgede teklifini/SKU'yu kullanılabilir hale getirmek isteyebilirsiniz.  Alternatif olarak, belirli bir bölgede teklifini/SKU'yu desteğini durduracak isteyebilirsiniz.
Kullanılabilirlik değiştirmek için aşağıdaki adımları kullanın:

1.  Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).

2.  Altında **tüm teklifleri** güncelleştirmek istediğiniz teklif bulun.

3.  İçinde **SKU'ları** sekmesini ve ardından kullanılabilirliğini değiştirmek istediğiniz SKU üzerinde.

4.  Tıklayarak **ülkeleri seçin** düğmesini **ülke/bölge kullanılabilirliği** alan.

5.  Bölge kullanılabilirlik açılır, bölge ekleme veya bu SKU için kaldırma.

6.  Tıklayarak **Yayımla** , SKU'ları bölge kullanılabilirliği güncelleştirmek için yayımlama iş akışını başlatmak için.

Bir SKU yeni bölgede kullanılabilir yaptıysanız, belirli bir bölgenin fiyatlandırma belirtmek için imkanına sahip olursunuz **fiyatlandırma verilerini dışarı aktar** işlevselliği. Önce kullanılabilir olduktan sonra olan bir bölge geri ekliyorsanız, fiyatlandırma değişiklikleri verilmeyen olduğundan fiyatlandırma güncelleştirmeniz mümkün olmayacaktır.


### <a name="add-a-new-sku"></a>Yeni bir SKU ekleyin

Yeni bir SKU'ya mevcut teklifte için kullanılabilir hale getirmek için aşağıdaki adımları kullanın: 

1.  Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).

2.  Altında **tüm teklifleri** güncelleştirmek istediğiniz teklif bulun.

3.  Altında **SKU'ları** sekmesinde, üzerinde **yeni bir SKU ekleyin** ve sağlayan bir **SKU kimliği** pencerede.

4.  VM makalesinde ayrıntılı olarak yeniden yayımlamanız [bir sanal makine Azure Marketi'nde yayımlayabileceğiniz](./cpp-publish-offer.md).

5.  Tıklayarak **Yayımla** yeni SKU'nuz yayımlamak için iş akışını başlatmak için.


### <a name="update-offer-marketplace-metadata"></a>Teklif Market meta verilerini güncelleştir

Market meta verilerini güncelleştirmek için aşağıdaki adımları kullanın: şirket adı, logolar, vb. — teklifinizle ilişkili: 

1.  Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).

2.  Altında **tüm teklifleri** güncelleştirmek istediğiniz teklif bulun.

3.  Goto **Market** sekmesine, ardından makalesindeki yönergeleri izleyin [bir sanal makine Azure Marketi'nde yayımlayabileceğiniz](./cpp-publish-offer.md) meta veri değişiklikleri yapma.

4.  Tıklayarak **Yayımla** yaptığınız değişiklikleri yayımlamak için iş akışını başlatmak için.


### <a name="update-pricing-on-published-offers"></a>Yayımlanan teklifler fiyatlandırmasını güncelleştir

Kullandıkça Öde teklifinizi yayımlandıktan sonra doğrudan fiyatlandırma SKU artıramıyor.  (Ancak, aynı Teklif altında yeni bir SKU oluşturma, eski SKU silebilir ve ardından yeni müşterilerin teklifinizi yeniden yayımlayın.)  Buna karşılık, aşağıdaki adımları kullanarak yayımlanan bir teklifi fiyatı azaltabilirsiniz:

1.  Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).

2.  Altında **tüm teklifleri**, güncelleştirme teklifini bulun.

3.  Fiyatlandırma azaltmak istediğiniz SKU üzerinde tıklayın.

4.  Fiyatlandırma 1 x 1 ayarladıysanız, GUI UI fiyatına doğrudan değiştirebilirsiniz. İçeri/dışarı aktarma elektronik fiyatlandırma ayarlarsanız, içeri/dışarı aktarma özelliği üzerinden fiyatlar yalnızca düşürebilir.

3.  **Kaydet**’e tıklayın.

4.  Tıklayarak **Yayımla** yaptığınız değişiklikleri yayımlamak için iş akışını başlatmak için.

Canlı Web sitesinde olduğunda yeni azalan fiyatlandırma yeni müşterilere görünür olacaktır.  Bu yeni fiyat müşterilerinizin aşağıdaki şekillerde etkiler:

- Yeni müşteriler, bu yeni fiyat ücretlendirilir. 
- Var olan müşterileri için fiyat düşüşü fiyat düşüşü etkili dönüştü fatura döngüsü başlangıcına kadar geriye dönük olarak yansıtılır.
Bir fiyat düşüşü sırasında oluşan döngüsü için önceden faturalandırılmış, bunlar düşük fiyat kapsayacak şekilde kendi sonraki fatura döneminde para iadesi alacaksınız.


<!-- TD: This has been implemented, need to change the SKU Tab topic to reflect and move this section there. -->
### <a name="simplified-currency-pricing"></a>Basitleştirilmiş bir para birimi fiyatlandırması

1 Eylül 2018'den başlayarak, yeni bir bölüm adı verilen **Basitleştirilmiş para birimi fiyatlandırma** portalına eklenmesi. Microsoft Azure Marketi iş daha öngörülebilir fiyatlandırma ve müşterilerinizin koleksiyonları dünya genelindeki etkinleştirerek hızlandırma. Bu hızlandırma biz müşterilerinizin Fatura para birimi sayısını azaltmayı içerir.

Yeni bir bölüm bu yeni para biriminde fiyatlandırmayı kazanır. Bu yeni bir kapatma para tüm müşterilerin geçişi tamamlandıktan sonra özgün fiyatlandırma bölümünde kullanımdan kaldırılacak ve yalnızca Basitleştirilmiş para birimi fiyatlandırma bölümünde kalır.

Kasım 1 yeni fiyat bölgeleri için kapatma para birimi gibi ayarlanacak 2018 ile değişiyor kadar olacaktır. Burada görüntülerle kapatma para birimi değişmiyor bölgeleri için fiyat artırmanın mümkün olmayacaktır.

> [!NOTE] 
> Teklifinizi yayımlamaya API'leri kullanan yeni bir bölüm teklif JSON içinde görebilirsiniz. Bu olarak açıklamalı `virtualMachinePricingV2` veya `monthlyPricingV2`teklif türüne bağlı olarak. 

Bu değişiklik hakkında sorularınız varsa, kişi [Azure Marketi Destek](../../support-azure-marketplace.md).


## <a name="compare-feature"></a>Özellik karşılaştırması

Zaten yayımlanan bir teklifi üzerinde değişiklik yaptığınızda yararlanabileceğiniz **karşılaştırma** yapmış olduğunuz değişiklikleri denetleme özelliği. Bu özelliği kullanmak için:

1.  Düzenleme işlemi içinde herhangi bir noktada tıklayın **karşılaştırma** teklifinizi düğmesi.

    ![Özellik düğmesi karşılaştırın](./media/publishvm_037.png)


2.  Yan yana sürümlerini pazarlama varlıkları ve meta verileri görüntüleyin.


## <a name="history-of-publishing-actions"></a>Eylemleri yayımlama geçmişi

Tüm geçmiş yayımlama etkinliğini görüntülemek için tıklayın **geçmişi** bulut iş ortağı portalı sol gezinti menü öğesi. Burada Azure Marketi Teklifleriniz ömrü boyunca gerçekleştirilen zaman damgalı eylemleri görüntüleme olanağınız olacaktır.  
<!-- TD: Add after section authored: For more information, see [History page](../portal-tour/cpp-history-page.md). -->

