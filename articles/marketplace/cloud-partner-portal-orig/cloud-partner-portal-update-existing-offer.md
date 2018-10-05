---
title: Mevcut bir teklifi Azure Marketi için güncelleştirme | Microsoft Docs
description: Bulut iş ortağı portalını kullanarak mevcut bir Azure Marketi teklifi güncelleştirme açıklanmaktadır.
services: Azure, Marketplace, Cloud Partner Portal,
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
ms.date: 09/18/2018
ms.author: pbutlerm
ms.openlocfilehash: bfe2b0c70c8b912f6489ed461f5bcf6f233ed60d
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48811388"
---
<a name="update-an-existing-offer-for-azure-marketplace"></a>Mevcut bir teklifi Azure Marketi için güncelleştirme
==============================================

Çeşitli Canlı gittiğini sonra teklifinizi uygulayabilirsiniz güncelleştirmeleri vardır.   [Bulut iş ortağı portalı](https://cloudpartner.azure.com/) değişiklikler de dahil olmak üzere doğru olduğundan emin olmak için bir teklif düzenlenmesine yardımcı olmak için çeşitli özelliklere sahiptir:

-  Yeni sanal makine (VM) görüntü sürümü için mevcut bir SKU ekleme
-  Bir SKU içinde kullanılabilir bölgeleri değiştirme
-  Yeni SKU'ları ekleme
-  Öneriler veya SKU'ları için Market meta verilerini güncelleştirme 
-  Kullandıkça Öde aboneliğine güncelleştirme fiyatlandırma sunar
-  Özellikleri karşılaştırma
-  Özelliklerin geçmişi

Bir teklif veya SKU değiştirdikten sonra değişiklikleri kullanıma sunulmadan önce yayımlanmalıdır.
Bu makalede, VM teklifi güncelleştirme farklı yönleriyle gösterilmektedir.


<a name="unpermitted-changes-to-vm-offersku"></a>VM teklifini/SKU'yu unpermitted değişiklikler
-----------------------------------

Bir VM teklifi veya teklif Azure Marketi'nde Canlı olduğunda değiştirilemez SKU'sunu bazı öznitelikleri vardır.

-  Teklif kimliği ve teklif yayımcı kimliği.
-  Mevcut bir SKU'ları SKU kimliği.
-  Mevcut bir SKU'ları sayısı veri diski.
-  Mevcut SKU'lara model değişiklikleri faturalama veya lisans.


<a name="updating-the-vm-image-version-for-a-sku"></a>VM görüntü sürümü için bir SKU güncelleştiriliyor
---------------------------------------

VM görüntü teklifinizin bir SKU için güncelleştirilmiş olabilir ek özellikler, güvenlik düzeltme ekleri, vb. ile. Böyle senaryolarda, SKU'nuzu başvuran VM görüntüsünü güncelleştirmek isteyebilirsiniz. Bir VM görüntüsünü güncelleştirmek için aşağıdaki adımları kullanın. 

1.  Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).
2.  Altında **tüm teklifleri**, güncelleştirmek istediğiniz teklif bulun.
3.  Altında **SKU'ları** formu olan VM görüntü SKU tıklayarak güncellemek istediğiniz.
4.  Altında **Disk sürümü**, tıklayarak **+ yeni Disk sürümü** yeni bir VM görüntüsü eklemek için.
5.  Yeni VM görüntüleri **Disk sürümü**. Disk sürümü izlemesi gereken [semantik sürüm](http://semver.org/) biçimi. Sürümleri X.Y.Z, burada X, Y ve Z tamsayılardır biçiminde olmalıdır. Sağladığınız yeni sürümü önceki sürümlerden daha büyükse, başka yeni sürüm numarasını Azure portalında gösterilmez veya bağlı Azure Market yayımladığınızda emin olun.
6.  İçin **işletim sistemi VHD URL'si**, işletim sistemi VHD'si için oluşturulan paylaşılan erişim imzası (SAS) URI'si girin. Veri diski sayısı SKU farklı sürümleri arasında değiştiremeyeceğinizi unutmayın. Önceki sürümler yapılandırılmış veri diskleri varsa, bu yeni sürümün yapılandırılmış veri diskleri de olması gerekir.
7.  Tıklayarak **yayımlama** için Azure Market ve Azure portalında kullanılabilmesi için yeni VM sürümünüzü yayımlama iş akışı hız kazandırın.


<a name="changing-regions-a-sku-is-available-in"></a>Değiştirme bölgeler bir SKU içinde kullanılabilir
--------------------------------------

Zaman içinde daha fazla bölgede teklifini/SKU'yu kullanılabilir hale getirmek isteyebilirsiniz.
Alternatif olarak, belirli bir bölgede teklifini/SKU'yu desteğini durduracak isteyebilirsiniz.
Bu değişiklikleri uygulamak için aşağıdaki adımları izleyin.

1.  Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).
2.  Altında **tüm teklifleri** güncelleştirmek istediğiniz teklif bulun.
3.  Altında **SKU'ları** formunda, bölge kullanılabilirliği güncelleştirmek istediğiniz SKU'DA'a tıklayın.
4.  Tıklayarak **ülkeleri seçin** düğmesini **ülke/bölge kullanılabilirliği** alan.
5.  Açılır bölge kullanılabilirliği bölge Ekle/bu SKU için istediğiniz Kaldır.
6.  Tıklayarak **Yayımla** başlatmasını, SKU'ları bölge kullanılabilirliği güncelleştirilecek yayımlama iş akışını devre dışı.

Bir SKU yeni bölgede kullanılabilir yaptıysanız, belirli bir bölgenin fiyatlandırma belirtmek için imkanına sahip olursunuz **fiyatlandırma verilerini dışarı aktar** işlevselliği. Önce kullanılabilir olduktan sonra olan bir bölge geri ekliyorsanız, fiyatlandırma değişiklikleri verilmeyen olduğundan fiyatlandırma güncelleştirmeniz mümkün olmayacaktır.


<a name="adding-a-new-sku"></a>Yeni bir SKU'ya ekleme
----------------

Yeni bir SKU'ya mevcut teklifte için kullanılabilir hale getirmek için izleyin aşağıdaki adımları.

1.  Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).
2.  Altında **tüm teklifleri** güncelleştirmek istediğiniz teklif bulun.
3.  Altında **SKU'ları** formu, temel clik **yeni bir SKU ekleyin** ve bir **SKU kimliği** içinde açılır.
4.  Ayrıntılı adımları izlemeden [bir sanal makine Azure Marketi'nde yayımlayabileceğiniz](./cloud-partner-portal-publish-virtual-machine.md).
5.  Tıklayarak **Yayımla** başlatmasını yayınlayın yeni SKU'nuz için yayımlama iş akışını devre dışı.


<a name="updating-offer-marketplace-metadata"></a>Teklif Market meta verileri güncelleştiriliyor.
------------------------------------

Güncelleştirme şirket logoları, vb. gibi teklifinizle ilişkili Market meta verilerini güncelleştirmek için gerek duyduğunuz senaryolara sahip. Ürününüzün meta verilerini güncelleştirmek için aşağıdaki adımları kullanın.

1.  Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).
2.  Altında **tüm teklifleri** güncelleştirmek istediğiniz teklif bulun.
3.  Goto **Market** form ve değişiklik yapmak için yönergeleri izleyin.
4.  Tıklayarak **Yayımla** başlatmasını yaptığınız değişiklikleri yayınlayın için yayımlama iş akışını devre dışı.


<a name="updating-pricing-on-published-offers"></a>Yayımlanan tekliflere ilişkin fiyatlandırma güncelleştiriliyor
------------------------------------

Kullandıkça Öde teklifinizi yayımlanır, mevcut bir SKU fiyatı artıramıyor, ancak aynı Teklif altında yeni bir SKU'ya oluşturabilirsiniz sonra eski SKU silin ve sonra teklifinizi yeniden yayımlayın. Ayrıca, fiyata göre zaten yayımlanan teklifler azaltmak izin veriyoruz. Teklif fiyatınızın azaltmak için:

1.  Fiyatlandırma azaltmak istediğiniz SKU üzerinde tıklayın.
2.  Fiyatlandırma 1 x 1 ayarladıysanız, GUI UI fiyatına doğrudan değiştirebilirsiniz. İçeri/dışarı aktarma elektronik fiyatlandırma ayarlarsanız, içeri/dışarı aktarma özelliği üzerinden fiyatlar yalnızca düşürebilir.
3.  **Kaydet**’e tıklayın.
4.  Teklifinizi yeniden yayımlayın ve yayınlayın.

Fiyatlandırma, Web sitesinde tümüyle canlı ve tüm yeni müşteriler yeni azalan fiyatını ödersiniz sonra yeni müşterilere görünür olacaktır.

Var olan müşterileri için fiyat düşüşü fiyat düşüşü etkili dönüştü fatura döngüsü başlangıcına kadar geriye dönük olarak yansıtılır.
Bir fiyat düşüşü sırasında oluşan döngüsü için önceden faturalandırılmış, bunlar düşük fiyat kapsayacak şekilde kendi sonraki fatura döneminde para iadesi alacaksınız.


<a name="simplified-currency-pricing"></a>Basitleştirilmiş bir para birimi fiyatlandırması
---------------------------

Eylül 2018'den başlayarak, yeni bir bölüm adı verilen **Basitleştirilmiş para birimi fiyatlandırma** için görünür olur. Microsoft Azure Marketi iş daha öngörülebilir fiyatlandırma ve müşterilerinizin koleksiyonları dünya genelindeki etkinleştirerek hızlandırma. Bu hızlandırma biz müşterilerinizin Fatura para birimi sayısını azaltarak artar.

Yeni bir bölüm bu yeni para biriminde fiyatlandırmayı kazanır. Bu yeni bir kapatma para tüm müşterilerin geçişi tamamlandıktan sonra özgün fiyatlandırma bölümünde kullanımdan kaldırılacak ve yalnızca "Para birimi fiyatlandırma Basitleştirilmiş" bölümü kalır.

Kasım 1 yeni fiyat bölgeleri için kapatma para birimi gibi ayarlanacak 2018 ile değişiyor kadar olacaktır. Burada görüntülerle kapatma para birimi değişmiyor bölgeleri için fiyat artırmanın mümkün olmayacaktır. Para birimi gibi değişiyor bölgeleri şunlardır:

| Ülke                  | ISO2 kod | Basitleştirilmiş bir fatura para birimi |
|--------------------------|-----------|-----------------------------|
| **Cezayir**              | DZ        | USD                         |
| **Arjantin**            | AR        | USD                         |
| **Bahreyn**              | BH        | USD                         |
| **Belarus**              | TARAFINDAN        | USD                         |
| **Brezilya**               | BR        | BRL                         |
| **Bulgaristan**             | BG        | EUR                         |
| **Şili**                | CL        | USD                         |
| **Kolombiya**             | ORTAK        | USD                         |
| **Kosta Rika**           | CR        | USD                         |
| **Hırvatistan**              | İK        | EUR                         |
| **Çek Cumhuriyeti**       | CZ        | EUR                         |
| **Mısır**                | ÖRNEĞİN        | USD                         |
| **Guatemala**            | GT        | USD                         |
| **Hong Kong**            | HK        | USD                         |
| **Macaristan**              | HU        | EUR                         |
| **İzlanda**              | OLDUĞU        | EUR                         |
| **Endonezya**            | Kimlik        | USD                         |
| **İsrail**               | IL        | USD                         |
| **Ürdün**               | JO        | USD                         |
| **Kazakistan**           | KZ        | USD                         |
| **Kenya**                | YAP        | USD                         |
| **Kuveyt**               | KW        | USD                         |
| **Liechtenstein**        | LI        | EUR                         |
| **Makedonya (EYC)**     | MK        | USD                         |
| **Malezya**             | BİLGİSAYARIM        | USD                         |
| **Meksika**               | MX        | USD                         |
| **Karadağ**           | BANA        | USD                         |
| **Fas**              | MA        | USD                         |
| **Nijerya**              | NG        | USD                         |
| **Umman**                 | OM        | USD                         |
| **Pakistan**             | BA        | USD                         |
| **Paraguay**             | PY        | USD                         |
| **Peru**                 | PE        | USD                         |
| **Filipinler**          | PH        | USD                         |
| **Polonya\***             | PL        | EUR                         |
| **Katar**                | QA        | USD                         |
| **Romanya**              | RO        | EUR                         |
| **Suudi Arabistan**         | SA        | USD                         |
| **Sırbistan**               | RS        | USD                         |
| **Singapur**            | SG        | USD                         |
| **Güney Afrika**         | ZA        | USD                         |
| **Tayland**             | TH        | USD                         |
| **Trinidad ve Tobago**  | TT        | USD                         |
| **Tunus**              | TN        | USD                         |
| **Türkiye**               | TR        | USD                         |
| **Ukrayna**              | UA        | USD                         |
| **Birleşik Arap Emirlikleri** | AE        | USD                         |
| **Uruguay**              | UY        | USD                         |
|  |  |  |

> [!NOTE]
> Teklifinizi yayımlamaya API'leri kullanırsanız, teklif adlı yeni bir bölümündeki görebilir **JSON**. Bu olarak açıklamalı `virtualMachinePricingV2` veya `monthlyPricingV2`teklif türüne bağlı olarak.

Bu değişiklik etrafında herhangi bir sorunuz varsa, Azure Market desteğe başvurun.


<a name="compare-feature"></a>Özellik karşılaştırması
---------------

Zaten yayımlanan bir teklifi üzerinde değişiklik yaptığınızda kullanabilirsiniz *karşılaştırma* yapmış olduğunuz değişiklikleri denetleme özelliği. Karşılaştırma kullanmak için:

1.  Düzenleme işlemi içinde herhangi bir noktada tıklayabilirsiniz **karşılaştırma** teklifinizi düğmesi.

2.  Yan yana sürümlerini pazarlama varlıkları ve meta verileri görüntüleyin.


<a name="history-of-publishing-actions"></a>Eylemleri yayımlama geçmişi
-----------------------------

Tüm geçmiş yayımlama etkinliğini görüntülemek için tıklayın **geçmişi** bulut iş ortağı portalının sol gezinti bölmesindeki sekmesi. Burada Azure Marketi Teklifleriniz ömrü boyunca gerçekleştirilen zaman damgalı eylemleri görüntüleme olanağınız olacaktır.
