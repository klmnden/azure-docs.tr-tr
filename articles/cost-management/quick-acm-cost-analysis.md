---
title: Hızlı Başlangıç - Maliyet analiziyle Azure maliyetlerini keşfetme | Microsoft Docs
description: Bu hızlı başlangıç, Azure kurumsal maliyetlerinizi keşfetmek ve analiz etmek için maliyet analizini kullanmanıza yardımcı olur.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 03/13/2019
ms.topic: quickstart
ms.service: cost-management
manager: dougeby
ms.custom: seodec18
ms.openlocfilehash: 55407ec1846a0fe2eb037756dc2e97d8b05e7330
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60312330"
---
# <a name="quickstart-explore-and-analyze-costs-with-cost-analysis"></a>Hızlı Başlangıç: Maliyet Analizi ile maliyetleri analiz

Azure maliyetlerinizi düzgün bir şekilde denetlemeden ve iyileştirmeden önce maliyetlerin kuruluşunuzun neresinden kaynaklandığını anlamanız gerekir. Hizmetlerinizin tutarının ne kadar olacağını bilmek, ortamlarınızı ve sistemlerinizi desteklemek için de yararlıdır. Maliyetlerin tüm kapsamıyla görünür olması kuruluşun harcama desenlerini doğru anlamak için önemlidir. Harcama desenleri, bütçeler gibi maliyet denetim mekanizmalarını güçlendirmek için kullanılabilir.

Bu hızlı başlangıçta, kurumsal maliyetlerinizi keşfetmek ve analiz etmek için maliyet analizini kullanırsınız. Maliyetlerin zaman içinde nerede oluştuğunu anlamak ve harcama eğilimlerini tanımlamak için kuruluşa göre toplanmış maliyetleri görüntüleyebilirsiniz. Bütçeye göre aylık, üç aylık, hatta yıllık maliyet eğilimlerini tahmin etmek için zaman içinde tahakkuk eden maliyetleri görüntüleyebilirsiniz. Bütçe, mali kısıtlamalara uymaya yardımcı olur. Bütçe, harcama düzensizliklerini yalıtmak amacıyla günlük veya aylık maliyetleri görüntülemek için de kullanılır. Dahası, geçerli raporun verilerini daha fazla analiz etmek için veya dış sistemde kullanmak üzere indirebilirsiniz.

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

- Maliyet analizinde maliyetleri gözden geçirme
- Maliyet görünümlerini özelleştirme
- Maliyet analizi verilerini indirme


## <a name="prerequisites"></a>Önkoşullar

Maliyet analizi, çeşitli Azure hesabı türlerini destekler. Desteklenen bir hesap türleri için tam listesini görüntülemek için bkz: [anlamak maliyet Yönetimi verilerine](understand-cost-mgt-data.md). Maliyet verilerini görüntülemek için bir Azure hesabınız için en azından okuma erişimi gerekir.

İçin [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) müşteriler, okuma olması gerekir en az bir veya daha fazla maliyet verilerini görüntülemek için aşağıdaki kapsamları erişim.

- Fatura hesabı
- Bölüm
- Kayıt hesabı
- Yönetim grubu
- Abonelik
- Kaynak grubu

Maliyet Yönetimi verilerine erişim atama hakkında daha fazla bilgi için bkz. [verilerine erişim atama](assign-access-acm-data.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

- https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="review-costs-in-cost-analysis"></a>Maliyet analizinde maliyetleri gözden geçirme

Maliyetlerinizi maliyet analizi gözden geçirmek için Azure portal ve select istenen kapsama açın **maliyet analizi** menüsünde. Örneğin, gitmek **abonelikleri**listeden aboneliği seçin ve ardından **maliyet analizi** menüsünde. Kullanım **kapsam** zehirli maliyet analizi farklı bir kapsam penceresine geçin. Kapsamlar hakkında daha fazla bilgi için bkz: [anlayın ve kapsamlı iş](understand-work-scopes.md).

Veri birleştirmesi sağlamak ve maliyet bilgilerine erişimi denetlemek için seçtiğiniz kapsam Maliyet Yönetimi’nin tamamında kullanılır. Kapsamları kullandığınızda, birden çok kapsam seçemezsiniz. Bunun yerine, başkalarının kadar geri alma ve daha sonra filtre istediğinize aşağı daha büyük bir kapsam seçin. Bu, bazı kişiler, alt kapsamlar aktarma hedefi bir üst kapsama erişimi olmaması nedeniyle anlamak önemlidir.

**Maliyet analizini aç**’a tıklayın.

İlk maliyet analizi görünümünde aşağıdaki alanlar bulunur:

**Toplam** – Geçerli ayın toplam maliyetlerini gösterir.

**Bütçe** – Varsa, seçilen kapsam için planlanan harcama limitini gösterir.

**Birikmiş maliyet** – Ayın başından başlayarak toplam tahakkuk eden günlük harcamayı gösterir. Fatura hesabınız veya aboneliğiniz için [bütçe oluşturduktan](tutorial-acm-create-budgets.md) sonra, bütçeye göre harcama eğiliminizi hemen görebilirsiniz. Tarihin üzerine gelerek o gün için birikmiş maliyeti görüntüleyebilirsiniz.

**Özet (halka) grafikler** – Toplam maliyeti ortak bir standart özellikler kümesine ayırarak dinamik özetler sağlar. Geçerli ay için en fazladan en aza doğru tahakkuk eden maliyeti gösterir. İstediğiniz zaman farklı bir özet seçerek özet grafikleri değiştirebilirsiniz. Maliyetler varsayılan olarak şu kategorilere ayrılır: hizmet (ölçüm kategorisi), konum (bölge) ve alt kapsam. Örneğin, fatura hesapları altında kayıt hesapları, abonelikler altında kaynak grupları ve kaynak grupları altında kaynaklar.

![Azure portalında maliyet analizi başlangıç görünümü](./media/quick-acm-cost-analysis/cost-analysis-01.png)

## <a name="customize-cost-views"></a>Maliyet görünümlerini özelleştirme

Varsayılan görünüm şunlar gibi sorulara hızlı yanıtlar sağlar:

- Ne kadar harcadım?
- Bütçemin dışına çıkar mıyım?

Öte yandan, birçok durumda daha derin analizler gerekir. Özelleştirme, seçilen tarihle sayfanın en üstünde başlatılır.

Maliyet analizi, varsayılan olarak geçerli ayın verilerini gösterir. Tarih seçiciyi kullanarak geçen aya, bu aya, bu takvim çeyreğine, bu takvim yılına veya seçtiğiniz özel bir veri aralığına hızla geçebilirsiniz. Geçen ayın seçilmesi en son Azure faturanızı analiz etmenin ve kolayca ücretlerde mutabık kalmanın en hızlı yoludur. Geçerli çeyrek ve yıl seçenekleri daha uzun vadeli bütçelere göre maliyetlerin izlenmesine yardımcı olur. Farklı bir tarih aralığı da seçebilirsiniz. Örneğin, tek bir günü, son yedi günü veya geçerli aydan önce bir yıl geriye giderek istediğiniz tarihleri seçmeniz mümkündür.

![Bu ay için bir örnek seçimi gösteren tarih seçici](./media/quick-acm-cost-analysis/date-selector.png)

Maliyet analizi varsayılan olarak **birikmiş** maliyetleri gösterir. Birikmiş maliyetler, her günün yanı sıra önceki günlerin de tüm maliyetlerini içerdiğinden, günlük tahakkuk eden maliyetlerinizin sürekli büyüyen bir görünümü elde edilir. Bu görünüm, seçilen zaman aralığı için bütçeye göre nasıl bir eğilim gösterdiğinizi ortaya koymak için iyileştirilmiştir.

Ayrıca, her günün maliyetlerini gösteren bir **günlük** görünüm vardır. Günlük görünüm büyüme eğilimini göstermez. Görünüm, günden güne maliyet sıçrama yaptığında veya iyice düştüğünde ortaya çıkan düzensizlikleri gösterecek şekilde tasarlanmıştır. Bütçe seçtiyseniz, günlük görünüm günlük bütçenizin neye benzeyeceğine ilişkin bir tahmin de gösterir. Günlük maliyetleriniz tutarlı olarak tahmini günlük bütçenin üzerinde olduğunda aylık bütçenizin aşılacağını öngörebilirsiniz. Tahmini günlük bütçe, bütçenizi alt düzeyde görselleştirmeye yardımcı olan bir araçtan başka bir şey değildir. Günlük maliyetlerinizde dalgalanmalar olduğunda tahmini günlük bütçenin aylık bütçeyle karşılaştırılması daha az kesinlik sağlar.

Genel olarak, veri ya da bildirimler tüketilen kaynaklar için sekiz saat içinde görmeyi bekleyebilirsiniz.

![Örnek, geçerli ay için günlük maliyetlerin gösteren günlük görünümü](./media/quick-acm-cost-analysis/daily-view.png)

Grup kategorisi seçip en üstteki toplam alan grafiğinde görüntülenen verileri değiştirmek için **Gruplandır** seçeneğini kullanabilirsiniz. Gruplandırma nasıl harcamalarınızı ortak kaynak ve kullanım özellikler, kaynak grubu veya kaynak etiketleri gibi tarafından kategorilere ayrılmıştır hızlı bir şekilde görmenize olanak tanır. Etiketlere göre gruplandırmak için gruplandırma ölçütü istediğiniz etiketi anahtarı seçin. Her bir değer bu etiket için uygulanan bir etiketi olmayan kaynaklar için ek bir segment tarafından ayrılmış maliyetleri görürsünüz.

Çoğu [destek Azure kaynakları etiketleme](../azure-resource-manager/tag-support.md), ancak bazı etiketler, faturalandırma ve maliyet Yönetimi'nde kullanılabilir değildir. Ayrıca, kaynak grubu etiketleri desteklenmez. Maliyet yönetimi, etiketler kaynağa doğrudan uygulanan tarihten itibaren kaynak etiketleri yalnızca destekler.

Burada, geçen ayın görünümü için Azure hizmet maliyetlerinin bir görünümü yer alır.

![Örnek Azure hizmet maliyetlerini geçen aya ait gösteren gruplandırılmış günlük birikmiş görünümü](./media/quick-acm-cost-analysis/grouped-daily-accum-view.png)

Özet grafiklerin filtreleri ve seçilen zaman aralığı için genel maliyetleri, daha geniş bir resmini vermek için ana grafiğin Göster farklı gruplandırmaları altında. Bir özellik ya da herhangi bir boyuta göre toplanmış maliyetleri görüntülemek üzere etiketi seçin.


![Kaynak grubu adları gösteren geçerli görünüm için tam veri](./media/quick-acm-cost-analysis/full-data-set.png)

Önceki resimde kaynak grubunun adları gösterilir. Etiket başına toplam maliyetleri görüntülemek üzere etikete göre gruplandırabilirsiniz, ancak kaynak veya kaynak grubu başına tüm etiketleri görüntüleme görünümlerden herhangi birinde maliyet analizi içinde kullanılabilir değil.

Maliyetler belirli bir özniteliğe göre gruplanırken maliyet açısından ilk on katkıda bulunan en yüksekten en düşüğe doğru gösterilir. Ondan fazla grubu olması halinde en çok dokuz maliyet katkıda bulunanları gösterilmektedir. Ayrıca gösterildiği gibidir bir **başkalarının** tüm geri kalan grupların birlikte kapsayan bir grup. Etiketlere göre gruplandırma olduğunda da görebilirsiniz bir **Untagged** uygulanan etiket anahtarı yoksa maliyetleri için Grup. **Etiketlenmemiş** etiketlenmemiş maliyetleri etiketli maliyetlerinden daha fazla olduğunda bile her zaman en son olur. On veya daha fazla etiket değeri varsa, etiketlenmemiş maliyetleri parçası olacak **başkalarının**.

*Klasik* (Azure Hizmet Yönetimi veya ASM) sanal makineler, ağ ve depolama kaynaklarını ayrıntılı fatura veri paylaşım yok. Olarak birleştirilmiş **Klasik Hizmetleri** maliyetleri gruplandırırken.

Herhangi bir görünüm için tam veri kümesini görüntüleyebilirsiniz. Seçtiğiniz seçimleri veya uyguladığınız filtreler sunulan verileri etkiler. Veri kümesini görmek için tıklayın **grafik türü** listeleyin ve ardından **tablo** görünümü.

![Geçerli görünümde bir tablo için verileri görüntüleme](./media/quick-acm-cost-analysis/chart-type-table-view.png)


## <a name="download-cost-analysis-data"></a>Maliyet analizi verilerini indirme

Maliyet analizinden bilgileri **indirerek**, Azure portalda şu anda gösterilen tüm veriler için CSV dosyası oluşturabilirsiniz. Uyguladığınız tüm filtreler ve gruplandırmalar dosyada yer alır. Etkin olarak görüntülenmeyen en yukarıdaki Toplam grafiğine ilişkin temel alınan veriler CSV dosyasına eklenir.

## <a name="next-steps"></a>Sonraki adımlar

Bütçelerin nasıl oluşturulduğunu ve yönetildiğini öğrenmek için ilk öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Bütçe oluşturma ve yönetme](tutorial-acm-create-budgets.md)
