---
title: Teklifinizin Azure Market'te dağıtma | Microsoft Docs
description: Azure Marketi'nde teklifinizi--dağıtmak için yönergeler sanal makine görüntüsü, geliştirici hizmeti, veri hizmeti, vb.--yol ve bilgi edinin.
services: marketplace-publishing
documentationcenter: ''
author: v-miclar
manager: hascipio
editor: ''
ms.assetid: 8f79b891-84e2-4f41-ba0d-66420e2c6b2e
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2016
ms.author: hascipio
ROBOTS: NOINDEX
ms.openlocfilehash: 058f50853795453617593a6a07e2951f15f28174
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54076120"
---
# <a name="deploy-your-offer-to-the-azure-marketplace"></a>Teklifinizin Azure Market'te dağıtma
Olduğunuzda teklifinizle (diğer bir deyişle, müşteri senaryoları, pazarlama içeriği, vb. test ettiğiniz) ve başlatma, istek hazır **anında üretime** üzerinde **Yayımla** sekmesi.  

1. İzlenecek yol altında dört adımı sayfasında Yayımlama Portalı, tamamlanan ve yeşil olmalıdır. Sanal makine teklifleri için aşağıdaki yönergeleri'nin izlendiğinden emin olun.
   
    ![Çizim][img-pubportal-walkthru-checked]
2. Seçin **Yayımla** sol taraftaki listenin sekmesinden.
   
    ![Çizim][img-pubportal-menu-publish]
3. Düğmesini **üretime göndermeye onay isteği**. İstek yapıldıktan sonra son bir gözden geçirme onayı takım yürütür ve teklifinizin Azure Market'te sonra kullanıma sunulacaktır.
   
    ![Çizim][img-pubportal-publish-pushproduction]

> [!IMPORTANT]
> Sanal makineleri olması durumunda, üretime göndermeye isteği onay düğmesine tıkladığınızda aşağıdaki adımları Sahne gerçekleştirilir. Yayımlama yayımlama sekmesindeki her bir adımın ilerleme durumunu görüntülemek mümkün olacaktır portalı. ("Listelendi" durum gösterilene kadar), sona düzeltme gereken tüm hata bilgileri için bu sayfayı düzenli aralıklarla denetlemelisiniz.
> 
> * İlk başta, vhd doğrulamak sertifika takımın üretim isteğiniz gider. Ancak, teklifinizi zaten listelenmiş güncelleştiriyorsanız ve istek değişiklik yalnızca pazarlama got, sonra onaylama adımı atlanır.
> * Sonraki adımda, istek teklifi pazarlama içeriği doğrulayın içerik doğrulama takımın gelir.
> * Ardından, yukarıdaki adımları başarılı olursa, teklif üretimde onaylanır. Şu anda durum haline gelir "listelenen" Yayımlama Portalı'nda. Ancak, bu "Listelendi" durum işlem tamamlandıktan göstermez. Aşağıdaki adımlar Azure Marketi'nde teklif kullanılabilir hale gelmeden önce tamamlanması gerekir.
> * Yukarıdaki adımda üretimde teklif onaylandıktan sonra çoğaltma teklifi tüm Azure veri merkezleri arasında başlatır. Genellikle, 24-48hours çoğaltmanın tamamlanmasını alır ancak VHD'nin boyutuna bağlı olarak bir hafta sürebilir. Ancak, teklifinizi zaten listelenmiş güncelleştiriyorsanız ve yalnızca değişiklik pazarlama got, çoğaltma daha hızlı olur.
> * Çoğaltma tamamlandığında, daha sonra teklifin Azure Marketi'nde kullanılabilir.
> 
> Olarak kullanılırken, teklif her zaman silebilirsiniz bir **taslak** durumu (yani, hiçbir zaman **hazırlama için anında iletme** veya **anında üretime**). Üzerinde **geçmişi** sekmesinde **taslağı at** taslak silmek için sayfanın alt kısmındaki düğmesi.
> 
> 

## <a name="production-checklist-for-all-virtual-machine-offers"></a>Tüm sanal makine teklifleri için üretim denetim listesi
* Bir Microsoft Azure sertifikalı iş ortağı olduğundan emin olun
* Yalnızca SKU çözüm şablonunun bir parçası ise SKU'ları sekmesinde seçeneği "gizleme marketten bu SKU, her zaman bir çözüm şablonu satın alınması çünkü" Evet işaretlenmelidir. Diğer tüm durumlarda, bu seçenek her zaman Hayır işaretlenmelidir
* Unutmayın: SKU listelenen sonra SKU görünürlük ayarlama değiştirmemelisiniz. Bu işlev desteklemiyoruz.
* Aşağıda logoların aşağıda verilen Azure Marketplace logosu yönergelere uyması emin olun.
* Teklifinizi ve SKU'nuzu açıklaması aynı olmamalıdır.
* SKU'ları başlık ve teklif uzun özeti aynı olmamalıdır.
* SKU başlık ve teklif özeti, aynı olmamalıdır.
* SKU başlıkları ile birden çok SKU'nun bir teklif için aynı olmamalıdır.

**Azure Market logo yönergeleri**

* Azure tasarımının basit bir renk paleti vardır. Logonuzu düşük tutmak için ikincil renk ve birincil sayısı.
* Azure portal'ın Tema renkleri beyaz ve siyah. Bu nedenle bu renkler, logolar arka plan rengi kullanmaktan kaçının. Logo, Azure portalında belirgin hale getirir bazı renk kullanın. Basit birincil renkleri öneririz. Saydam arka plan kullanıyorsanız, logo/metin beyaz veya siyah olmadığından emin olun.
* Arka plan gradyan logosunu kullanmayın.
* Metin, hatta şirket, yerleştirmekten kaçının veya adına logo marka.
* Logonuzu Görünüm ve yapısını 'düz' olmalıdır ve gradyanları kaçınmalıdır.
* Logo uzatılması gerektiğini değil.

**Ek yönergeler Hero logo:**

* Hero logosu isteğe bağlıdır. Yayımcı Hero logoyu karşıya yükleyin değil seçebilirsiniz. **Ancak bir kez karşıya yüklenen hero simgesi silinemez Yayımlama Portalı. Bu sırada, iş ortağı üretime Hero simgeler başka teklif henüz onaylanmamış için Azure Marketi yönergelere uyması gerekir.**
* Yayımcı görünen adı, SKU başlık ve teklif uzun özeti beyaz renkte görüntülenir. Bu nedenle herhangi bir ışık rengi Hero simgesinin arka planda tutarak kaçınmanız gerekir. Siyah, beyaz ve saydam bir arka plan için Hero simgeler izin verilmez.
* Yayımcı görünen adı, SKU başlık, uzun Özet teklif ve Oluştur düğmesine katıştırıldığı program aracılığıyla içinde Hero logosu teklif listelenen gider sonra. Bu nedenle Hero logosu tasarlarken herhangi bir metin girmemelisiniz. Yalnızca metin (diğer bir deyişle, yayımcı görünen adı, SKU başlık, uzun Özet teklif) programlı olarak bizim tarafımızdan orada dahil edilir çünkü sağ tarafta boşluk bırakın. 415 x 100 sağdaki metin için boş alan olmalıdır (ve 370 göre uzaklığını soldan piksel).

## <a name="additional-production-checklist-for-already-listed-virtual-machine-offers"></a>Önceden listelenen sanal makine için ek üretim denetim sunar.
* Şirketinizdeki aynı Teklif ada sahip bir teklif zaten varsa bulunmadığını denetleyin. Evet ise, yeni bir yinelenen teklifi oluşturmak yerine var olan bir teklif, SKU yeni bir sürümünü eklemeniz gerekir.
* Veri diski, aynı SKU iki sürümü değiştirmemesi gerekir.
* Azure Marketi faturalandırma mevcut müşterilerin etkiler listelenen SKU'ları fiyatlandırma değişikliği desteklemiyor. Listelenen SKU'ları SKU kullanılabildiği bölgelerdeki fiyatlandırma değiştirmemenizi emin olun. Ancak, yeni SKU'ları ekleyebilir veya yeni bölgeler için mevcut bir SKU ekleyin.

## <a name="next-steps"></a>Sonraki adımlar
Teklif etkin hale gelir, sonra tüm işlevselliği ve sözleşmeleri düzgün üretim ortamında çalışması test ve hazırlık ortamında doğrulanması doğrulamak için müşteri senaryoları test edin.

## <a name="see-also"></a>Ayrıca bkz.
* [Başlarken: Nasıl bir teklifi Azure Marketinde yayımlama](marketplace-publishing-getting-started.md)

[img-pubportal-walkthru-checked]:media/marketplace-publishing-push-to-production/pubportal-walkthru-checked.png
[img-pubportal-menu-publish]:media/marketplace-publishing-push-to-production/pubportal-menu-publish.png
[img-pubportal-publish-pushproduction]:media/marketplace-publishing-push-to-production/pubportal-publish-pushproduction.png
