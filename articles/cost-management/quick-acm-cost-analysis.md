---
title: Hızlı Başlangıç - Maliyet analiziyle Azure maliyetlerini keşfetme | Microsoft Docs
description: Bu hızlı başlangıç, Azure kurumsal maliyetlerinizi keşfetmek ve analiz etmek için maliyet analizini kullanmanıza yardımcı olur.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/14/2019
ms.topic: quickstart
ms.service: cost-management
manager: micflan
ms.custom: seodec18
ms.openlocfilehash: b4302713188237b97ffbe8473f6a37edd6741b36
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65793086"
---
# <a name="quickstart-explore-and-analyze-costs-with-cost-analysis"></a>Hızlı Başlangıç: Maliyet Analizi ile maliyetleri analiz

Azure maliyetlerinizi düzgün bir şekilde denetlemeden ve iyileştirmeden önce maliyetlerin kuruluşunuzun neresinden kaynaklandığını anlamanız gerekir. Hizmetlerinizin tutarının ne kadar olacağını bilmek, ortamlarınızı ve sistemlerinizi desteklemek için de yararlıdır. Maliyetlerin tüm kapsamıyla görünür olması kuruluşun harcama desenlerini doğru anlamak için önemlidir. Harcama desenleri, bütçeler gibi maliyet denetim mekanizmalarını güçlendirmek için kullanılabilir.

Bu hızlı başlangıçta, kurumsal maliyetlerinizi keşfetmek ve analiz etmek için maliyet analizini kullanırsınız. Maliyetlerin zaman içinde nerede oluştuğunu anlamak ve harcama eğilimlerini tanımlamak için kuruluşa göre toplanmış maliyetleri görüntüleyebilirsiniz. Bütçeye göre aylık, üç aylık, hatta yıllık maliyet eğilimlerini tahmin etmek için zaman içinde tahakkuk eden maliyetleri görüntüleyebilirsiniz. Bütçe, mali kısıtlamalara uymaya yardımcı olur. Bütçe, harcama düzensizliklerini yalıtmak amacıyla günlük veya aylık maliyetleri görüntülemek için de kullanılır. Dahası, geçerli raporun verilerini daha fazla analiz etmek için veya dış sistemde kullanmak üzere indirebilirsiniz.

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

- Maliyet analizinde maliyetleri gözden geçirme
- Maliyet görünümlerini özelleştirme
- Maliyet analizi verilerini indirme


## <a name="prerequisites"></a>Önkoşullar

Maliyet analizi, Azure hesap türleri için farklı türde destekler. Desteklenen bir hesap türleri için tam listesini görüntülemek için bkz: [anlamak maliyet Yönetimi verilerine](understand-cost-mgt-data.md). Maliyet verilerini görüntülemek için bir Azure hesabınız için en azından okuma erişimi gerekir.

İçin [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) müşteriler, okuma olması gerekir en az bir veya daha fazla maliyet verilerini görüntülemek için aşağıdaki kapsamları erişim.

- Fatura hesabı
- Bölüm
- Kayıt hesabı
- Yönetim grubu
- Abonelik
- Kaynak grubu

Maliyet Yönetimi verilerine erişim atama hakkında daha fazla bilgi için bkz. [verilerine erişim atama](assign-access-acm-data.md).

## <a name="sign-in-to-azure"></a>Oturum açın: Azure

- https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="review-costs-in-cost-analysis"></a>Maliyet analizinde maliyetleri gözden geçirme

Maliyetlerinizi maliyet analizi gözden geçirmek için Azure portal ve select kapsamı açın **maliyet analizi** menüsünde. Örneğin, gitmek **abonelikleri**listeden aboneliği seçin ve ardından **maliyet analizi** menüsünde. Kullanım **kapsam** zehirli maliyet analizi farklı bir kapsam penceresine geçin. Kapsamlar hakkında daha fazla bilgi için bkz: [anlayın ve kapsamlı iş](understand-work-scopes.md).

Seçtiğiniz kapsam maliyet yönetimi, veri birleştirme sağlamak ve maliyet bilgilerini erişimi denetlemek için kullanılır. Kapsamları kullandığınızda, birden çok kapsam seçemezsiniz. Bunun yerine, başkalarının kadar geri alma ve daha sonra ihtiyacınız iç içe geçmiş kapsamlar aşağı filtre daha büyük bir kapsama seçin. Bu yaklaşım bazı kişiler birden çok iç içe kapsam kapsayan ve tek bir üst kapsam için erişiminiz olmayabilir beri anlamak önemlidir.

İlk maliyet analizi görünümünde aşağıdaki alanlar bulunur:

**Toplam** – Geçerli ayın toplam maliyetlerini gösterir.

**Bütçe** – Varsa, seçilen kapsam için planlanan harcama limitini gösterir.

**Birikmiş maliyetini** – toplam toplam günlük harcama, ayın en baştan başlatmayı gösterir. Fatura hesabınız veya aboneliğiniz için [bütçe oluşturduktan](tutorial-acm-create-budgets.md) sonra, bütçeye göre harcama eğiliminizi hemen görebilirsiniz. Tarihin üzerine gelerek o gün için birikmiş maliyeti görüntüleyebilirsiniz.

**Özet (halka) grafikler** – Toplam maliyeti ortak bir standart özellikler kümesine ayırarak dinamik özetler sağlar. Bunlar en az masraflı geçerli ay için gösterir. İstediğiniz zaman farklı bir özet seçerek özet grafikleri değiştirebilirsiniz. Maliyetler varsayılan olarak şu kategorilere ayrılır: hizmet (ölçüm kategorisi), konum (bölge) ve alt kapsam. Örneğin, fatura hesapları altında kayıt hesapları, abonelikler altında kaynak grupları ve kaynak grupları altında kaynaklar.

![Azure portalında maliyet analizi başlangıç görünümü](./media/quick-acm-cost-analysis/cost-analysis-01.png)

## <a name="customize-cost-views"></a>Maliyet görünümlerini özelleştirme

Maliyet analizi en yaygın hedefler için en iyi duruma getirilmiş dört yerleşik görünümleri sahiptir:

Görünüm | Aşağıdaki gibi sorulara yanıt...
--- | ---
Birikmiş maliyet | Bu ay şimdiye kadar kullanmış olduğunuz? Bütçemin dışına çıkar mıyım?
Günlük maliyet | Son 30 gün için günde maliyetlerinde herhangi bir artış var. neydi?
Hizmete göre maliyet | Geçtiğimiz farklılık aylık kullanımı nasıl sahip 3 faturalar?
Kaynağa göre maliyet | Hangi kaynakların en kadar bu ay maliyeti?

![Bu ay için bir örnek seçimi gösteren Görünüm Seçici](./media/quick-acm-cost-analysis/view-selector.png)

Öte yandan, birçok durumda daha derin analizler gerekir. Özelleştirme, seçilen tarihle sayfanın en üstünde başlatılır.

Maliyet analizi, varsayılan olarak geçerli ayın verilerini gösterir. Genel Tarih aralıklarına kolayca geçiş yapmak için tarih seçiciyi kullanın. Son yedi gün, geçtiğimiz ay, yıl veya özel bir tarih aralığı birkaç örnek verilebilir. Kullandıkça Öde Abonelikleri, Takvim ayına son fatura ve geçerli fatura dönemi gibi bağlı olmayan, fatura dönemi göre tarih aralıkları de içerir. Kullanım **< önceki** ve **İleri >** sırasıyla önceki veya sonraki dönemini atlamak için menünün üst bağlantılar. Örneğin, **< önceki** son yedi günden 8-14 gün önce ve sonra 15-21 gün önce geçiş yapar.

![Bu ay için bir örnek seçimi gösteren tarih seçici](./media/quick-acm-cost-analysis/date-selector.png)

Maliyet analizi varsayılan olarak **birikmiş** maliyetleri gösterir. Ek olarak önceki gün günlük toplama maliyetlerinizi sürekli büyüyen bir görünümünü her gün için tüm maliyetler birikmiş maliyetlerini içerir. Bu görünüm, seçilen zaman aralığı için bütçeye göre nasıl bir eğilim gösterdiğinizi ortaya koymak için iyileştirilmiştir.

Ayrıca, her günün maliyetlerini gösteren bir **günlük** görünüm vardır. Günlük görünüm büyüme eğilimini göstermez. Görünüm, günden güne maliyet sıçrama yaptığında veya iyice düştüğünde ortaya çıkan düzensizlikleri gösterecek şekilde tasarlanmıştır. Bütçe seçtiyseniz, günlük görünüm günlük bütçenizin neye benzeyeceğine ilişkin bir tahmin de gösterir. Günlük maliyetleriniz tutarlı olarak tahmini günlük bütçenin üzerinde olduğunda aylık bütçenizin aşılacağını öngörebilirsiniz. Tahmini günlük bütçe, bütçenizi alt düzeyde görselleştirmeye yardımcı olan bir araçtan başka bir şey değildir. Günlük maliyetlerinizde dalgalanmalar olduğunda tahmini günlük bütçenin aylık bütçeyle karşılaştırılması daha az kesinlik sağlar.

Genel olarak, veri ya da bildirimler tüketilen kaynaklar için 8-12 saat içinde görmeyi bekleyebilirsiniz.

![Örnek, geçerli ay için günlük maliyetlerin gösteren günlük görünümü](./media/quick-acm-cost-analysis/daily-view.png)

**Gruplandırma ölçütü** maliyetleri de azaltın Kes ve tanımlamak için ortak özellikler, Katkıda Bulunanlar üst. Kaynak etiketlerine göre gruplandırmak için örneği için gruplandırma ölçütü istediğiniz etiket anahtarı seçin. Maliyetleri uygulanan bir etiketi olmayan kaynaklar için ek bir segment ile her bir etiket değeri tarafından ayrılır.

Çoğu [destek Azure kaynakları etiketleme](../azure-resource-manager/tag-support.md), ancak bazı etiketler, faturalandırma ve maliyet Yönetimi'nde kullanılabilir değildir. Ayrıca, kaynak grubu etiketleri desteklenmez. Maliyet yönetimi, etiketler kaynağa doğrudan uygulanan tarihten itibaren kaynak etiketleri yalnızca destekler. İzleme [Azure maliyet yönetimi ile etiketi ilkeleri gözden geçirmek nasıl](https://www.youtube.com/watch?v=nHQYcYGKuyw) video maliyet veri görünürlüğünü artırmak için Azure etiketi ilke kullanma hakkında bilgi edinin.

Burada, geçen ayın görünümü için Azure hizmet maliyetlerinin bir görünümü yer alır.

![Örnek Azure hizmet maliyetlerini geçen aya ait gösteren gruplandırılmış günlük birikmiş görünümü](./media/quick-acm-cost-analysis/grouped-daily-accum-view.png)

Özet grafiklerin filtreleri ve seçilen zaman aralığı için genel maliyetleri, daha geniş bir resmini vermek için ana grafiğin Göster farklı gruplandırmaları altında. Bir özellik ya da herhangi bir boyuta göre toplanmış maliyetleri görüntülemek üzere etiketi seçin.


![Kaynak grubu adları gösteren geçerli görünüm için tam veri](./media/quick-acm-cost-analysis/full-data-set.png)

Önceki resimde kaynak grubunun adları gösterilir. Etiket başına toplam maliyetleri görüntülemek üzere etikete göre gruplandırabilirsiniz, ancak kaynak veya kaynak grubu başına tüm etiketleri görüntüleme görünümlerden herhangi birinde maliyet analizi içinde kullanılabilir değil.

Belirli bir öznitelik tarafından maliyetleri gruplandırma, üst 10 maliyete katkıda yüksekten düşüğe doğru gösterilmektedir. 10'dan fazla varsa, dokuz top maliyete katkıda ile gösterilen bir **başkalarının** tüm geri kalan grupların birlikte kapsayan bir grup. Etiketlere göre gruplandırma olduğunda da görebilirsiniz bir **Untagged** uygulanan etiket anahtarı yoksa maliyetleri için Grup. **Etiketlenmemiş** etiketlenmemiş maliyetleri etiketli maliyetlerinden daha yüksek olsa bile her zaman en son olur. Etiketlenmemiş maliyetleri parçası olması **başkalarının**, 10 veya daha fazla etiket değeri varsa.

*Klasik* sanal makineler, ağ ve depolama kaynaklarını ayrıntılı fatura veri paylaşım yok. Olarak birleştirilmiş **Klasik Hizmetleri** maliyetleri gruplandırırken.

Herhangi bir görünüm için tam veri kümesini görüntüleyebilirsiniz. Seçtiğiniz seçimleri veya uyguladığınız filtreler sunulan verileri etkiler. Veri kümesini görmek için tıklayın **grafik türü** listeleyin ve ardından **tablo** görünümü.

![Geçerli görünümde bir tablo için verileri görüntüleme](./media/quick-acm-cost-analysis/chart-type-table-view.png)


## <a name="download-cost-analysis-data"></a>Maliyet analizi verilerini indirme

Maliyet analizinden bilgileri **indirerek**, Azure portalda şu anda gösterilen tüm veriler için CSV dosyası oluşturabilirsiniz. Uyguladığınız tüm filtreler ve gruplandırmalar dosyada yer alır. Etkin olarak görüntülenmeyen en yukarıdaki Toplam grafiğine ilişkin temel alınan veriler CSV dosyasına eklenir.

## <a name="next-steps"></a>Sonraki adımlar

Bütçelerin nasıl oluşturulduğunu ve yönetildiğini öğrenmek için ilk öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Bütçe oluşturma ve yönetme](tutorial-acm-create-budgets.md)
