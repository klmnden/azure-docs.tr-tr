---
title: Hızlı Başlangıç - Maliyet analiziyle Azure maliyetlerini keşfetme | Microsoft Docs
description: Bu hızlı başlangıç, Azure kurumsal maliyetlerinizi keşfetmek ve analiz etmek için maliyet analizini kullanmanıza yardımcı olur.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 10/19/2018
ms.topic: quickstart
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 6b935322c9d892793f3695e0922d15f5886c7e25
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49471297"
---
# <a name="quickstart-explore-and-analyze-costs-with-cost-analysis"></a>Hızlı Başlangıç: Maliyet analiziyle maliyetleri keşfetme ve analiz etme

Azure maliyetlerinizi düzgün bir şekilde denetlemeden ve iyileştirmeden önce maliyetlerin kuruluşunuzun neresinden kaynaklandığını anlamanız gerekir. Hizmetlerinizin tutarının ne kadar olacağını bilmek, ortamlarınızı ve sistemlerinizi desteklemek için de yararlıdır. Maliyetlerin tüm kapsamıyla görünür olması kuruluşun harcama desenlerini doğru anlamak için önemlidir. Harcama desenleri, bütçeler gibi maliyet denetim mekanizmalarını güçlendirmek için kullanılabilir.

Bu hızlı başlangıçta, kurumsal maliyetlerinizi keşfetmek ve analiz etmek için maliyet analizini kullanırsınız. Maliyetlerin zaman içinde nerede oluştuğunu anlamak ve harcama eğilimlerini tanımlamak için kuruluşa göre toplanmış maliyetleri görüntüleyebilirsiniz. Bütçeye göre aylık, üç aylık, hatta yıllık maliyet eğilimlerini tahmin etmek için zaman içinde tahakkuk eden maliyetleri görüntüleyebilirsiniz. Bütçe, mali kısıtlamalara uymaya yardımcı olur. Bütçe, harcama düzensizliklerini yalıtmak amacıyla günlük veya aylık maliyetleri görüntülemek için de kullanılır. Dahası, geçerli raporun verilerini daha fazla analiz etmek için veya dış sistemde kullanmak üzere indirebilirsiniz.

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

- Maliyet analizinde maliyetleri gözden geçirme
- Maliyet görünümlerini özelleştirme
- Maliyet analizi verilerini indirme


## <a name="prerequisites"></a>Ön koşullar

Maliyet analizi tüm [Kurumsal Sözleşme (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) müşterileri tarafından kullanılabilir. Maliyet verilerini görüntülemek için aşağıdaki kapsamlardan birine veya daha fazlasına en azından yazma erişiminiz olmalıdır.


|**Kapsam**|**Tanımlanma yeri**|**Kapsam maliyetlerini analiz etmek için gereken erişim**|**Önkoşul EA ayarı**|**Fatura verilerini şurada bir araya getirir**|
|---                |---                  |---                   |---            |---           |
|Faturalama hesabı<sup>1</sup>|[https://ea.azure.com ](https://ea.azure.com )|Kuruluş Yöneticisi|None|Kurumsal sözleşmedeki tüm abonelikler|
|Bölüm|[https://ea.azure.com ](https://ea.azure.com )|Bölüm Yöneticisi|DA ücretleri görüntüleme etkinleştirildi|Bölüme bağlı olan kayıt hesabına ait olan tüm abonelikler|
|Kayıt hesabı<sup>2</sup2>|[https://ea.azure.com ](https://ea.azure.com )|Hesap Sahibi|AO ücretleri görüntüleme etkinleştirildi|Kayıt hesabındaki tüm abonelikler|
|Yönetim grubu|[https://portal.azure.com ](https://portal.azure.com )|Maliyet Yönetimi Okuyucusu (veya Okuyucu)|AO ücretleri görüntüleme etkinleştirildi|Yönetim grubu altındaki tüm abonelikler|
|Abonelik|[https://portal.azure.com ](https://portal.azure.com )|Maliyet Yönetimi Okuyucusu (veya Okuyucu)|AO ücretleri görüntüleme etkinleştirildi|Abonelikteki tüm kaynaklar/kaynak grupları|
|Kaynak grubu|[https://portal.azure.com ](https://portal.azure.com )|Maliyet Yönetimi Okuyucusu (veya Okuyucu)|AO ücretleri görüntüleme etkinleştirildi|Kaynak grubundaki tüm kaynaklar|

<sup>1</sup>Fatura hesabı genellikle Kurumsal Sözleşme veya Kayıt olarak nitelenir.

<sup>2</sup>Kayıt hesabı genellikle hesap sahibi olarak nitelenir.

**DA ücretleri görüntüleme** ve **AO ücretleri görüntüleme** ayarları hakkında daha fazla bilgi için bkz. [Maliyet erişimini etkinleştirme](../billing/billing-enterprise-mgmt-grp-troubleshoot-cost-view.md#enabling-access-to-costs).





## <a name="sign-in-to-azure"></a>Azure'da oturum açma

- http://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="review-costs-in-cost-analysis"></a>Maliyet analizinde maliyetleri gözden geçirme

Maliyet analiziyle maliyetlerinizi gözden geçirmek için Azure portalda **Maliyet Yönetimi + Fatura** &gt; **Maliyet Yönetimi** &gt; **Kapsam değiştir**’e gidip bir kapsam seçin ve ardından **Seç**’e tıklayın.

Veri birleştirmesi sağlamak ve maliyet bilgilerine erişimi denetlemek için seçtiğiniz kapsam Maliyet Yönetimi’nin tamamında kullanılır. Kapsamları kullandığınızda, birden çok kapsam seçemezsiniz. Bunun yerine, diğerlerinin toplandığı büyük bir kapsam seçer ve neleri istediğinize bağlı olarak filtre uygulayıp kapsamı daraltırsınız. Bazı kişilere alt kapsamların toplandığı üst kapsama erişim verilmemesi gerektiğinden, bunu anlamak önemlidir.

**Maliyet analizini aç**’a tıklayın.

İlk maliyet analizi görünümünde aşağıdaki alanlar bulunur:

**Toplam** – Geçerli ayın toplam maliyetlerini gösterir.

**Bütçe** – Varsa, seçilen kapsam için planlanan harcama limitini gösterir.

**Birikmiş maliyet** – Ayın başından başlayarak toplam tahakkuk eden günlük harcamayı gösterir. Fatura hesabınız veya aboneliğiniz için [bütçe oluşturduktan](tutorial-acm-create-budgets.md) sonra, bütçeye göre harcama eğiliminizi hemen görebilirsiniz. Tarihin üzerine gelerek o gün için birikmiş maliyeti görüntüleyebilirsiniz.

**Özet (halka) grafikler** – Toplam maliyeti ortak bir standart özellikler kümesine ayırarak dinamik özetler sağlar. Geçerli ay için en fazladan en aza doğru tahakkuk eden maliyeti gösterir. İstediğiniz zaman farklı bir özet seçerek özet grafikleri değiştirebilirsiniz. Maliyetler varsayılan olarak şu kategorilere ayrılır: hizmet (ölçüm kategorisi), konum (bölge) ve alt kapsam. Örneğin, fatura hesapları altında kayıt hesapları, abonelikler altında kaynak grupları ve kaynak grupları altında kaynaklar.

![Maliyet analizinin ilk görünümü](./media/quick-acm-cost-analysis/cost-analysis-01.png)

## <a name="customize-cost-views"></a>Maliyet görünümlerini özelleştirme

Varsayılan görünüm şunlar gibi sorulara hızlı yanıtlar sağlar:

- Ne kadar harcadım?
- Bütçemin dışına çıkar mıyım?

Öte yandan, birçok durumda daha derin analizler gerekir. Özelleştirme, seçilen tarihle sayfanın en üstünde başlatılır.

Maliyet analizi, varsayılan olarak geçerli ayın verilerini gösterir. Tarih seçiciyi kullanarak geçen aya, bu aya, bu takvim çeyreğine, bu takvim yılına veya seçtiğiniz özel bir veri aralığına hızla geçebilirsiniz. Geçen ayın seçilmesi en son Azure faturanızı analiz etmenin ve kolayca ücretlerde mutabık kalmanın en hızlı yoludur. Geçerli çeyrek ve yıl seçenekleri daha uzun vadeli bütçelere göre maliyetlerin izlenmesine yardımcı olur. Farklı bir tarih aralığı da seçebilirsiniz. Örneğin, tek bir günü, son yedi günü veya geçerli aydan önce bir yıl geriye giderek istediğiniz tarihleri seçmeniz mümkündür.

![Tarih seçici](./media/quick-acm-cost-analysis/date-selector.png)

Maliyet analizi varsayılan olarak **birikmiş** maliyetleri gösterir. Birikmiş maliyetler, her günün yanı sıra önceki günlerin de tüm maliyetlerini içerdiğinden, günlük tahakkuk eden maliyetlerinizin sürekli büyüyen bir görünümü elde edilir. Bu görünüm, seçilen zaman aralığı için bütçeye göre nasıl bir eğilim gösterdiğinizi ortaya koymak için iyileştirilmiştir.

Ayrıca, her günün maliyetlerini gösteren bir **günlük** görünüm vardır. Günlük görünüm büyüme eğilimini göstermez. Görünüm, günden güne maliyet sıçrama yaptığında veya iyice düştüğünde ortaya çıkan düzensizlikleri gösterecek şekilde tasarlanmıştır. Bütçe seçtiyseniz, günlük görünüm günlük bütçenizin neye benzeyeceğine ilişkin bir tahmin de gösterir. Günlük maliyetleriniz tutarlı olarak tahmini günlük bütçenin üzerinde olduğunda aylık bütçenizin aşılacağını öngörebilirsiniz. Tahmini günlük bütçe, bütçenizi alt düzeyde görselleştirmeye yardımcı olan bir araçtan başka bir şey değildir. Günlük maliyetlerinizde dalgalanmalar olduğunda tahmini günlük bütçenin aylık bütçeyle karşılaştırılması daha az kesinlik sağlar.

![Günlük görünüm](./media/quick-acm-cost-analysis/daily-view.png)

Grup kategorisi seçip en üstteki toplam alan grafiğinde görüntülenen verileri değiştirmek için **Gruplandır** seçeneğini kullanabilirsiniz. Gruplandırma, harcamanızın kaynak türüne göre nasıl kategorilere ayrıldığını hemen görmenizi sağlar. Burada, geçen ayın görünümü için Azure hizmet maliyetlerinin bir görünümü yer alır.

![Gruplandırılmış günlük birikmiş görünümü](./media/quick-acm-cost-analysis/grouped-daily-accum-view.png)

En yukarıdaki Toplam görünümü altında yer alan özet grafikler farklı gruplandırma ve filtreleme kategorilerinin görünümlerini gösterir. Herhangi bir grup kategorisi seçtiğinizde, toplam görünümü için tam veri kümesi görünümün en altında bulunur. Burada kaynak gruplarının örneğini görebilirsiniz.

![Geçerli görünüm için tüm veriler](./media/quick-acm-cost-analysis/full-data-set.png)

Önceki resimde kaynak grubunun adları gösterilir. Maliyet analizi görünümlerinin, filtrelerin veya gruplandırmaların hiçbirinde kaynaklara ilişkin etiketlerin görünümü sağlanmaz.

Maliyetler belirli bir özniteliğe göre gruplanırken maliyet açısından ilk on katkıda bulunan en yüksekten en düşüğe doğru gösterilir. Toplamda ondan fazla grup varsa maliyet açısından ilk dokuz katkıda bulunana ek olarak kalan tüm grupları içeren bir **Diğer** grubu gösterilir.

*Klasik* (Azure Service Management veya ASM) sanal makineleri, ağ ve depolama kaynakları ayrıntılı fatura bilgisi paylaşmaz. Bunlar maliyet gruplarında **Klasik hizmetler** olarak gösterilir.


## <a name="download-cost-analysis-data"></a>Maliyet analizi verilerini indirme

Maliyet analizinden bilgileri **indirerek**, Azure portalda şu anda gösterilen tüm veriler için CSV dosyası oluşturabilirsiniz. Uyguladığınız tüm filtreler ve gruplandırmalar dosyada yer alır. Etkin olarak görüntülenmeyen en yukarıdaki Toplam grafiğine ilişkin temel alınan veriler CSV dosyasına eklenir.

## <a name="next-steps"></a>Sonraki adımlar

Bütçelerin nasıl oluşturulduğunu ve yönetildiğini öğrenmek için ilk öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Bütçe oluşturma ve yönetme](tutorial-acm-create-budgets.md)
