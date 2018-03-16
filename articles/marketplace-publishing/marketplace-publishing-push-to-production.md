---
title: "Teklifiniz için Azure Marketi dağıtma | Microsoft Docs"
description: "Hakkında bilgi edinin ve Azure Marketi teklifiniz--dağıtmak için yönergeleri sanal makine görüntüsü, geliştirici hizmeti, veri hizmeti, vb.--yol."
services: marketplace-publishing
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: 8f79b891-84e2-4f41-ba0d-66420e2c6b2e
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2016
ms.author: mbaldwin
ms.openlocfilehash: 8df7b0e49e17612743b02596e99f7d1fbe8c6803
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="deploy-your-offer-to-the-azure-marketplace"></a>Teklifiniz için Azure Marketi dağıtma
Olduğunuzda teklifiniz ile (diğer bir deyişle, müşteri senaryoları, pazarlama içeriği, vb. test ettiğiniz) ve başlatma, istek için hazır **anında üretime** üzerinde **Yayımla** sekmesi.  

1. Dört adım İZLENECEK altında sayfa yayımlama portal tamamlanmış ve yeşil olması gerekir. Sanal makine teklifleri için aşağıdaki kılavuzları izlenir emin olun.
   
    ![Çizim][img-pubportal-walkthru-checked]
2. Seçin **Yayımla** sol tarafındaki listeden sekmesi.
   
    ![Çizim][img-pubportal-menu-publish]
3. Düğmesini **üretime gönderme için onay isteği**. İstek yapıldığında, son gözden onay takım yürütür ve teklifiniz Azure Marketi'nde sonra kullanılabilir olacak.
   
    ![Çizim][img-pubportal-publish-pushproduction]

> [!IMPORTANT]
> Sanal makineler durumunda üretime göndermeyi isteği onay düğmesine tıkladığınızda aşağıdaki adımları Sahne gerçekleştirilir. Yayımlama Yayımla sekmesi altında her adımı ilerlemesini görüntülemek kuramaz portal. ("Listelendi" durumu gösterilir) kadar bu sayfayı düzenli aralıklarla, uçtan düzeltmesi gereken tüm hata bilgileri için işaretlemeniz gerekir.
> 
> * İlk olarak, üretim isteğinizi vhd doğrulamak sertifika takımın gider. Ancak, önceden listelenen teklifiniz güncelleştirdiğiniz ve istek değişiklik yalnızca pazarlama var, daha sonra sertifika adım atlanır.
> * Sonraki adımda, istek sunumu pazarlama içeriği doğrulayın içerik doğrulama takımın gelir.
> * Yukarıdaki adımları başarılı olursa, teklif üretimde onaylanır. Şu anda durumu hale "listelenen" yayımlama portalında. Ancak, bu "Listelendi" durum işlemin tamamlandığını göstermez. Aşağıdaki adımları teklif Azure Marketi'nde kullanılabilir olmadan önce tamamlanması gerekir.
> * Yukarıdaki adımı üretimde teklif isteğiniz onaylandıktan sonra çoğaltma önerinin tüm Azure veri merkezi arasında başlatın. Genellikle, 24-48hours çoğaltmanın tamamlanmasını alır ancak vhd öğesinin boyutuna bağlı olarak bir hafta sürebilir. Ancak, önceden listelenen teklifiniz güncelleştirdiğiniz ve değişiklik yalnızca pazarlama var, daha sonra çoğaltma daha hızlıdır.
> * Çoğaltma işlemi tamamlandıktan sonra teklif Azure Marketi'nde kullanılabilir.
> 
> Her zaman olarak kullanılırken teklif silebilirsiniz bir **taslak** durumu (yani, hiçbir zaman **anında hazırlama için** veya **anında üretime**). Üzerinde **geçmişi** sekmesini tıklatın, **atmak taslak** bir taslak silmek için sayfanın altındaki düğmesini.
> 
> 

## <a name="production-checklist-for-all-virtual-machine-offers"></a>Tüm sanal makine teklifleri üretim denetim listesi
* Bir Azure Microsoft Sertifikalı İş ortağı olduğundan emin olun
* Yalnızca SKU bir çözüm şablonu parçası ise SKU'ları sekmesi altındaki "gizle seçeneği bu SKU marketten, her zaman bir çözüm şablonu satın alınması çünkü" Evet olarak işaretlenmelidir. Tüm diğer durumlarda, bu seçenek her zaman Hayır işaretlenmelidir
* Unutmayın:, SKU listelenen sonra SKU görünürlük ayarlama değiştirmemeniz gerekir. Bu işlev desteklemez.
* Logo aşağıda verilen Azure Marketi logosu yönergelere uyması emin olun.
* Teklif ve SKU Açıklama aynı olmamalıdır.
* SKU'in başlık ve teklif uzun Özet aynı olmamalıdır.
* SKU başlık ve Özet sunan aynı olmamalıdır.
* SKU başlıkları ile birden çok SKU için bir teklif aynı olmamalıdır.

**Azure Market logo yönergeleri**

* Azure tasarım basit renk paletini sahiptir. Sayı birincil ve ikincil renkleri logonuzu düşük tutun.
* Azure portalının Tema renkleri beyaz ve siyah. Bu nedenle bu renkler, logolar arka plan rengi olarak kullanmaktan kaçının. Logolar Azure portalında belirgin hale getirecektir bazı renk kullanın. Basit birincil renkleri öneririz. Saydam arka plan kullanıyorsanız, logo/metin beyaz veya siyah olmadığından emin olun.
* Gradyan arka planı logosunu kullanmayın.
* Metin, bile, şirketiniz veya marka adı logosunu yerleştirmez.
* Logonuzun Görünüm ve yapısını 'düz' olmalı ve gradyan kaçınmalısınız.
* Logo uzatılabilir değil.

**Ek yönergeleri kahramanı logo:**

* Kahramanı logosu isteğe bağlıdır. Yayımcı kahramanı logosu yüklemek değil seçebilirsiniz. **Ancak bir kez karşıya yüklenen kahramanı simgesi yayımlama silinemiyor portal. O anda iş ortağı üretime kahramanı simgeler başka teklif Onaylanmadı için Azure Marketi yönergeleri izlemeniz gerekir.**
* Yayımcı görünen adı, SKU başlık ve uzun Özet teklif beyaz yazı tipi rengi görüntülenir. Bu nedenle açık bir renk kahramanı simgesi arka planda tutma kaçınmalısınız. Siyah, beyaz ve saydam arka plan kahramanı simgelerini izin verilmiyor.
* Yayımcı adı, SKU başlık, uzun Özet teklif görüntülemek ve Oluştur düğmesine katıştırılmış program aracılığıyla kahramanı logosunun içinde teklif listelenen gider sonra. Bu nedenle kahramanı logosu tasarlarken herhangi bir metin girmemeniz. Yalnızca (örn. yayımcı görünen adı, SKU başlık, uzun Özet teklif) metni program aracılığıyla tarafımızca orada dahil edilir çünkü boş alanı sağ tarafta bırakın. Metni boş alanı 415 x 100 sağdaki olmalıdır (ve 370px soluna göre uzaklığını).

## <a name="additional-production-checklist-for-already-listed-virtual-machine-offers"></a>Önceden listelenen sanal makine için ek üretim denetim sunar
* Bir teklif şirketinizdeki aynı Teklif adla zaten varsa bulunmadığını denetleyin. Yanıt Evet ise, yeni bir yinelenen teklif oluşturmak yerine mevcut teklifte SKU yeni bir sürümünü eklemeniz gerekir.
* Veri diski aynı SKU iki sürümü arasında değiştirmemeniz gerekir.
* Mevcut müşterileri faturalama etkiler gibi Azure Marketi fiyatlandırma değişikliği listelenen SKU'ları desteklemez. Listelenen SKU'ları SKU kullanılabilir olduğu bölgelerdeki fiyatlandırma değiştirmeyin emin olun. Ancak, yeni SKU'ları ekleyebilir veya var olan bir SKU'ya yeni bölgeler ekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Teklif Canlı gider sonra tüm üretim ortamında işlevselliği ve sözleşmeleri'nın düzgün çalışması test ve hazırlık ortamında doğrulanmış doğrulamak için müşteri senaryoları sınayın.

## <a name="see-also"></a>Ayrıca bkz.
* [Başlarken: bir teklifi Azure Marketinde yayımlama](marketplace-publishing-getting-started.md)

[img-pubportal-walkthru-checked]:media/marketplace-publishing-push-to-production/pubportal-walkthru-checked.png
[img-pubportal-menu-publish]:media/marketplace-publishing-push-to-production/pubportal-menu-publish.png
[img-pubportal-publish-pushproduction]:media/marketplace-publishing-push-to-production/pubportal-publish-pushproduction.png
