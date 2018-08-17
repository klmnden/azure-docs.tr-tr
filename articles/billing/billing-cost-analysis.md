---
title: Maliyet Analizi ile Azure maliyetlerini keşfedin | Microsoft Docs
description: Bu makalede, Azure Kurumsal maliyetlerinizi analiz için maliyet analizi kullanmanıza yardımcı olur.
services: billing
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 08/16/2018
ms.topic: conceptual
ms.service: billing
manager: dougeby
ms.custom: ''
ms.openlocfilehash: eeaf02853f8ffe9ca67dbf31afc687afb7dee242
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2018
ms.locfileid: "40180916"
---
# <a name="explore-and-analyze-costs-with-cost-analysis"></a>Maliyet Analizi ile maliyetleri analiz

Düzgün bir şekilde kontrol edebilir ve Azure iyileştirmek önce maliyetleri kuruluşunuz içinde nereden geldiğini anlamak gerekir. Ayrıca, hizmetlerinizi, maliyet ve hangi ortamları ve sistemleri desteklemek üzere ne kadar tasarruf bilmek de yararlı olabilir. Maliyetleri, tam spektrumlu görünürlük doğru bütçelerini gibi maliyet denetimi düzenekleri zorlamak için kullanılan kuruluş harcama desenlerini anlamak önemlidir.

Bu makalede, kuruluş maliyetlerinizi analiz için maliyet analizi kullanın. Maliyetleri burada doğan anlamak ve harcama eğilimleri belirleme konusunda kuruluşunuz tarafından toplanan maliyetleri görebilirsiniz. Üç aylık veya yıllık bile eğilimleri bir bütçeyle karşılaştırmalı maliyet birikmiş maliyetlerini aylık olarak tahmin etmek için zaman içinde görüntüleyin. Bütçe harcama sürdürmenin yalıtmak için günlük veya aylık maliyetleri görüntülemek veya mali kısıtlamalara uyması yardımcı olur. Ve daha fazla analiz için veya bir dış sistemde geçerli rapor verilerinin indirebilirsiniz.

## <a name="requirements"></a>Gereksinimler

Maliyet analizi, tüm Kurumsal Sözleşme (EA) müşterileri tarafından kullanılabilir. Maliyet verilerini görüntülemek için aşağıdaki kapsamları en az biri için okuma erişimi olmalıdır.

- Azure Kurumsal Anlaşma fatura hesap (kayıt)
- Azure EA aboneliği
- Azure EA aboneliği kaynak grubu

## <a name="review-costs-in-cost-analysis"></a>Maliyet analizi maliyetlerini gözden geçirin

Maliyet Analizi ile maliyetlerinizi gözden geçirmek için Azure portalını açın ardından gidin **maliyet Yönetimi + faturalandırma** &gt; **faturalama hesaplarının** &gt; Faturalama hesabı EAseçin&gt; maliyet Yönetimi altında seçin **maliyet analizi**.

Başlangıç maliyeti analiz görünümü aşağıdaki alanları içerir:

**Toplam** : geçerli ay için toplam maliyetleri göstermektedir.

**Bütçe** – planlı harcama sınırını seçilen kapsamın varsa gösterir.

**Birikmiş maliyetini** – tahakkuk edilen toplam günlük harcama, ayın en baştan başlatmayı gösterir. Varsa [bütçe oluşturulan](billing-cost-management-budget-scenario.md#create-the-azure-budget) , Faturalama hesabı veya aboneliği için bütçe ile ilgili olarak, harcama eğilimi hızlı bir şekilde görebilirsiniz. Bir tarih birikmiş maliyeti o gün için üzerine gelin.

**Özet (halka) grafikler** – dinamik özetleri sağlar. Bunlar, toplam maliyeti ortak bir standart özellikler kümesi tarafından ayırmanız. Geçerli ay için tahakkuk en az maliyet grafiklerini gösterir. Farklı bir Özet'i seçerek Özet grafiklerin dilediğiniz zaman değiştirebilirsiniz. Varsayılan olarak, maliyetleri hizmeti (ölçüm kategorisi), konum (bölge) ve alt kapsam (örneğin, fatura hesapları altında kayıt hesapları, abonelikleri altında kaynak grupları ve kaynaklar altında kaynak grupları) tarafından ayrılır.

![Başlangıç görünümü maliyet analizi](./media/billing-cost-analysis/cost-analysis-01.png)



## <a name="customizing-cost-views"></a>Maliyet görünümlerini özelleştirme

Varsayılan görünüm gibi sık sorulan sorulara hızlı yanıtlar sağlar:

- Ne kadar miyim harcama?
- Benim bir bütçe içinde açık kalsın mı?

Ancak, çoğu durumda, daha ayrıntılı analiz gereken vardır. Tarih Seçimi sayfasının üst kısmındaki özelleştirme başlatır.

Varsayılan olarak, maliyet analizi, geçerli ay için verileri gösterir. Hızlıca son ay, bu ay, bu takvim çeyreği, bu takvim yılı veya tercih ettiğiniz özel bir tarih aralığı geçiş tarih seçiciyi kullanın. Geçen ay seçilmesi, en son Azure faturanızı çözümlemek ve kolayca ücretleri mutabık kılmak için en hızlı yoludur. Geçerli bir üç aylık ve yıllık seçenekler daha uzun vadeli bütçelerini maliyetlerinizle izlemenize yardımcı olur. Ya da tek bir günde son yedi gün veya herhangi bir şey bir yıl önce geçerli ay'a kadar bir daha hassas ve geniş tarih aralığını seçebilirsiniz.

![Tarih Seçici](./media/billing-cost-analysis/date-selector.png)

Ayrıca varsayılan olarak, maliyet analizi gösterir **birikmiş** maliyetlerini. Birikmiş maliyetleri, günlük, onaylamanızın maliyetlerinizi sürekli büyüyen bir görünümünü önceki gün ek olarak her gün için tüm maliyetler içerir. Bu görünüm, nasıl seçili zaman aralığı için bir bütçeyle karşılaştırmalı bir popüler göstermek için optimize edilmiştir.

Ayrıca **günlük** görünümü. Bu, her gün için maliyetleri gösterir ve büyüme eğilimi göstermez. Günlük görünümü sürdürmenin maliyetleri ani göstermek veya gün gün DIP için optimize edilmiştir. Bütçe seçtiğinizde, günlük görünümü gibi günlük bütçenizi görünebilir tahmini de gösterir. Günlük maliyetlerinizi tahmini günlük bütçe tutarlı bir şekilde varsa, aylık bütçenizi aştığında bekleyebilirsiniz. Tahmini günlük bütçe bütçenizi daha düşük bir düzeyde görselleştirmenize yardımcı olmak için yalnızca bir yoludur. Günlük maliyetlerin dalgalanmalar olduğunda, bütçenizi aylık tahmini günlük bütçe karşılaştırma daha doğru olur.

![Günlük görüntüleme](./media/billing-cost-analysis/daily-view.png)

Yapabilecekleriniz **gruplandırma ölçütü** üst toplam alan grafikte görüntülenen verileri değiştirmek için bir grubu kategorisi seçin. Gruplandırma nasıl harcamalarınızı kaynak türüne göre kategorilere ayrılmıştır hızlı bir şekilde görmenize olanak tanır. Azure hizmet maliyetlerini son bir ay görünüm için bir görünümünü aşağıdadır.

![Gruplandırılmış günlük birikmiş görünümü](./media/billing-cost-analysis/grouped-daily-accum-view.png)

Özet grafiklerin altında üst toplam Görünüm ' dir. Farklı gruplandırma ve filtreleme kategorileri için görünümleri görmek için bunları kullanın. Herhangi bir grubu kategori seçtiğinizde, tam veri toplam görünüm için Görünüm alt kısmında kümesidir. Kaynak grupları için bir örnek aşağıda verilmiştir.

![Geçerli Görünüm için tam veri](./media/billing-cost-analysis/full-data-set.png)

### <a name="download-cost-analysis-data"></a>Maliyet analizi verilerini indir

Olduğunda, **indirme** bilgilerinden maliyet analizi, şu anda Azure portalında gösterilen tüm veriler için bir CSV dosyası oluşturulur. Ardından, herhangi bir filtre veya gruplandırma uyguladıysanız, bunlar dosyasına dahil. Etkin olarak portalda görüntülenmez üst toplam grafik için bazı temel alınan verileri de CSV dosyasında dahil edilir.

## <a name="next-steps"></a>Sonraki adımlar

Diğer görüntülemek [Azure'da faturalandırma ve maliyet Yönetimi belgeleri](billing-cost-management-budget-scenario.md).
