---
title: Market teklifleri güncelleştir | Azure Market
description: Güncelleştirme Azure ve bulut iş ortağı portalını kullanarak AppSource Marketlerden sunar.
services: Azure, AppSource, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 01/11/2019
ms.author: pabutler
ms.openlocfilehash: 73e2812c678dca7e21089ee9cc091db756d7e25a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64942399"
---
# <a name="update-azure-marketplace-and-appsource-offers"></a>Azure Market ve AppSource teklifler güncelleştir

Güncelleştirmeleri yayımlandıktan sonra teklifiniz için uygulayabileceğiniz çeşitli tür vardır.  [Bulut iş ortağı portalı](https://cloudpartner.azure.com/) bir teklif özniteliklerini değiştirme düzgün bir şekilde yönetmenize yardımcı olan dahil olmak üzere:

-  Yeni sanal makine (VM) bir görüntüyü veya paketi sürümü için mevcut bir SKU ekleme
-  Bir SKU içinde kullanılabilir bölgeleri değiştirme
-  Yeni SKU'ları ekleme
-  Öneriler veya SKU'ları için Market meta verilerini güncelleştirme 
-  Kullandıkça Öde aboneliğine güncelleştirme fiyatlandırma sunar

Portal özelliklerini karşılaştırın ve değişiklikler yönetmeye yardımcı özellikleri bir teklif için bir geçmişini görüntüleme olanağı gibi özellikleri de vardır.  Bir teklif veya SKU değiştirdikten sonra değişiklikleri "canlı" geçmeden önce yayımlanmalıdır.  Bu makalede, Market teklifi güncelleştiriliyor farklı yönlerini gösterilmektedir.

## <a name="unpermitted-changes-to-an-offersku"></a>Bir teklif/SKU unpermitted değişiklikler

Bir teklif ya da Market'te yayımlandıktan sonra değiştirilemez SKU'sunu bazı öznitelikleri vardır.  Karşılık gelen alanlara devre dışı **Düzenleyicisi** portal, örnek için sekmesinde:  

- Kimliği ve yayımcı kimliği sunar
- SKU KİMLİĞİ 
- Veri diski sayısı mevcut SKU'ları
- Faturalama/lisans modeli değişiklikler mevcut SKU'ları
- Sürüm, örneğin etiketler: `1.0.1`


## <a name="common-update-operations"></a>Genel güncelleştirme işlemleri

Aşağıdaki bölümlerde bazı çoğu güncelleştirme işlemleri gerçekleştirmek nasıl açıklanmaktadır.  Bu işlemler, tüm teklif türleri için kullanılamaz.  Bu işlemlerden birini başlatmak için bulut iş ortağı portalına oturum açmanız gerekir.


### <a name="update-offer-contacts"></a>Teklif kişilerini güncelleştirme

Teklifiniz için destek kişileri güncelleştirmek için aşağıdaki adımları kullanın.
1. İçinde **tüm sunar** sayfasında, bir teklif seçin.
2. Seçin **kişiler** sekmesi. Kişilerinizi güncelleştirin.
3. **Kaydet** düğmesini seçin.
4. Seçin **Yayımla** yayımlama işlemini başlatmak için.


### <a name="change-regions-an-offer-or-sku-is-available-in"></a>Bir teklif veya SKU kullanılabilir bölgeleri değiştirme

Zaman içinde daha fazla bölgede teklifini/SKU'yu kullanılabilir hale getirmek isteyebilirsiniz.
Alternatif olarak, belirli bir bölgede teklifini/SKU'yu desteğini durduracak isteyebilirsiniz.
Bu değişiklikleri uygulamak için aşağıdaki adımları izleyin.

1. İçinde **tüm teklifleri** sayfasında, güncelleştirmek istediğiniz teklif bulun.

Azure Marketi teklifleri için:

1. Seçin **SKU'ları** sekmesi.  SKU'ları değiştirmek için seçin.
1. Tıklayın **ülkeleri seçin** düğmesini **ülke/bölge kullanılabilirliği** alan.
1. Bölge kullanılabilirliği iletişim kutusunda, bölge ekleme veya bu SKU için kaldırma.

AppSource teklifleri için:

1. Seçin **mağaza ayrıntıları** sekmesi.
1. Yanındaki **desteklenen ülkeler/bölgeler** etiket, tıklayın **desteklenen ülkeler/bölgeler**. 
1. Desteklenen ülkeler/bölgeler iletişim kutusunda, bölge ekleme veya bu teklif için kaldırma.

Ya da Market'te:

1. Tıklayın **Yayımla** yayımlama işlemini başlatmak için. 

Bir SKU yeni bölgede kullanılabilir yapıldığı belirli bu bölge için fiyatlandırma belirtme olanağı varsa **fiyatlandırma verilerini dışarı aktar** işlevselliği. Önceden kullanılabilen bir bölge geri ekliyorsanız, fiyatlandırma değişiklikleri verilmeyen olduğundan fiyatlandırma güncelleştirilemiyor.


### <a name="add-a-new-sku"></a>Yeni bir SKU ekleyin 

Yeni bir SKU'ya var olan bir teklif için kullanılabilir hale getirmek için aşağıdaki adımları kullanın:

1. İçinde **tüm teklifleri** sayfasında, teklif bulun.
3. Altında **SKU'ları** formunda, tıklayın **yeni bir SKU ekleyin** ve sağlayan bir **SKU kimliği** pencerede.
4. Ayrıntılı adımları izlemeden [bir sanal makine teklifi yayımlama](../virtual-machine/cpp-publish-offer.md).
5. Tıklayın **Yayımla** yayımlama işlemini başlatmak için.


### <a name="update-offer-marketplace-assets"></a>Teklif Market varlıklarını güncelleştirme

Burada, metin tabanlı Market güncelleştirmeniz gerekir ve görüntü varlıkları, bu tür, şirket logoları, açıklama, vb. sunmak senaryolar sahip olabilir. Bu varlıklar güncelleştirmek için aşağıdaki adımları kullanın.

1. İçinde **tüm teklifleri** sayfasında, teklifinizi bulun. 
2. Seçin **Market** sekmesini ve teklife ilişkin yönergeleri *Marketi sekmesinden* konu.
3. Tıklayın **Yayımla** yayımlama işlemini başlatmak için.


### <a name="update-pricing-on-published-offers"></a>Yayımlanan teklifler fiyatlandırmasını güncelleştir

Kullandıkça Öde teklifinizi yayımlandıktan sonra mevcut bir SKU fiyatı artıramıyor.  Bunun yerine, aynı teklifi kapsamındaki bir SKU oluşturma, eski SKU silin ve sonra teklifinizi yeniden yayımlayın. Fiyata göre önceden yayımlanan teklifler düşürebilir. Teklif fiyatınızın azaltmak için:

1. SKU'ları fiyatlandırma azaltmak istiyorsanız seçin.
2. İlk olarak kullanılan aynı mekanizması tarafından daha düşük bir fiyatla ayarlamanız gerekir: doğrudan portal kullanıcı arabirimini veya içeri/dışarı aktarma elektronik ile.
3. **Kaydet**’e tıklayın.
4. Tıklayın **Yayımla** yayımlama işlemini başlatmak için.

Fiyatlandırma Market'te canlıdır ve tüm yeni müşteriler sonra yeni azalan fiyatını ödersiniz sonra yeni müşteriler için görünür durumdadır.  Var olan müşterileri için fiyat düşüşü fiyat düşüşü etkili dönüştü fatura döngüsü başlangıcına kadar geriye dönük olarak yansıtılır.  Bir fiyat düşüşü sırasında oluşan döngüsü için önceden faturalandırılmış, bunlar düşük fiyat kapsayacak şekilde kendi sonraki fatura döneminde para iadesi alacaksınız.


## <a name="compare-feature"></a>Özellik karşılaştırması

Yayımlanan bir teklifi üzerinde değişiklik yaptığınızda kullanabilirsiniz *karşılaştırma* değişiklikleri denetleme özelliği. Bu özelliği kullanmak için:

1. Düzenleme işlemi içinde herhangi bir noktada tıklayabilirsiniz **karşılaştırma** düğmesine **Düzenleyicisi** teklifiniz için sekmesinde.
2. Bir karşılaştırma pencere Market teklifi karşılaştırıldığında Bu teklif için kaydedilen değişiklikleri sürümlerini yan yana görüntüler. 

![Teklif düğmesi Düzenleyici sekmesinde karşılaştırın](./media/offer-compare-button.png)


## <a name="history-of-publishing-actions"></a>Eylemleri yayımlama geçmişi

Geçmiş yayımlama etkinlikleri görüntülemek için seçin **geçmişi** bulut iş ortağı portalının Sol dikey menü sekmesindedir.  Geçmişi sayfası esnek çeşitli özelliklere göre filtreleme sağlar ve sütun sırasını destekler.  Zaman damgalı her yayımlama etkinliğidir.  Daha fazla bilgi için [denetim geçmişi sayfası](../portal-tour/cpp-history-page.md).


## <a name="next-steps"></a>Sonraki adımlar

Bulut iş ortağı portalını da kullanabilirsiniz [yayımlanan bir SKU silin veya teklif](./cpp-delete-offer.md).
