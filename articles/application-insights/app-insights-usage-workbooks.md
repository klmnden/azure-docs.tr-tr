---
title: Araştırmak ve etkileşimli çalışma kitapları Azure Application ınsights ile kullanım verilerini paylaşma | Microsoft docs
description: Web uygulamanızın kullanıcılarının demografik analizi.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: conceptual
ms.date: 06/12/2017
ms.reviewer: daviste
ms.author: mbullwin
ms.openlocfilehash: 016a26acc153fba1c38d926fd5389d02755c2ff5
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35650919"
---
# <a name="investigate-and-share-usage-data-with-interactive-workbooks-in-application-insights"></a>Araştırmak ve etkileşimli çalışma kitapları Application ınsights ile kullanım verilerini Paylaş

Çalışma kitapları birleştirmek [Azure Application Insights](app-insights-overview.md) veri görselleştirmeleri [analiz sorguları](app-insights-analytics.md)ve etkileşimli belgeler metin. Çalışma kitapları, aynı Azure kaynağına erişimi olan diğer ekip üyeleri tarafından düzenlenebilir. Başka bir deyişle, sorgular ve bir çalışma kitabı oluşturmak için kullanılan denetimleri keşfedin, genişletin ve hataları olup olmadığını denetleyin kolaylaştırma çalışma kitabını okuma diğer kişiler tarafından kullanılabilir.

Çalışma kitapları gibi senaryolar için yararlıdır:

* İlgilendiğiniz ölçümleri önceden bilmediğinizde uygulamanızın kullanımını keşfetme: kullanıcılar, elde tutma oranları, dönüştürme oranlarını vb. sayısı. Application ınsights'ta başka kullanım analizi Araçları çalışma kitapları, birden çok türde görselleştirmeler ve analizleri, bu tür bir serbest biçimli araştırması için harika yönetilmelerini birleştirmek olanak tanır.
* Ekibinizin yeni kullanıma sunulan bir özellik performansıyla açıklanması, anahtar etkileşimleri ve diğer ölçümleri gösteren bir kullanıcı tarafından sayar.
* Bir A sonuçların paylaşılması / B deneme takımınızın diğer üyeleriyle uygulamanızda. Hedefler için metin ile deneme açıklar ve her bir kullanım ölçümü ve her ölçümü yukarıda veya aşağıda-hedefi olup için açık çağrı-çıkarmayı birlikte deneme değerlendirmek için kullanılan bir Analytics sorgusunu göster.
* Kesinti etkisini veri, açıklama metnini ve gelecekte kesintileri önlemek için sonraki adımlar ayrıntılı bir birleştirme uygulamanızın kullanım raporlama.

> [!NOTE]
> Application Insights kaynağınıza sayfa görüntüleme veya çalışma kitaplarını kullanmak için özel olaylar içermelidir. [Sayfa görüntülemeleri Application Insights JavaScript SDK'sı ile otomatik olarak toplamak için uygulamanızı ayarlayın öğrenin](app-insights-javascript.md).
> 
> 

## <a name="editing-rearranging-cloning-and-deleting-workbook-sections"></a>Düzenleme, yeniden düzenleme, kopyalama ve çalışma kitabı bölümleri silme

Bir çalışma kitabı bölümlerini yapılmış bir durumda: bağımsız olarak düzenlenebilir kullanım görselleştirmeler, grafikler, tablolar, metin veya Analytics sorgu sonuçları.

Bir çalışma kitabı bölümü içeriğini düzenlemek için tıklayın **Düzenle** aşağı ve çalışma kitabı bölümünün sağ düğmesi.

![Application Insights çalışma kitapları bölümünde düzenleme denetimleri](./media/app-insights-usage-workbooks/editing-controls.png)

1. Bitirdiğinizde bir bölümünü Düzenleyen, tıklayın **düzenleme Bitti** bölümünün sol alt köşedeki.

2. Bir bölümün bir kopyasını oluşturmak için tıklayın **bu bölümü kopyalayın** simgesi. Yinelenen bölümleri oluşturmak, önceki yinelemelerin kaybetmeden bir sorgu üzerinde gezinmek için yol için harika bir işlemdir.

3. Bir çalışma kitabı bir bölümde yukarı taşımak için tıklatın **Yukarı Taşı** veya **Aşağı Taşı** simgesi.

4. Bir bölümün kalıcı olarak kaldırmak için tıklayın **Kaldır** simgesi.

## <a name="adding-usage-data-visualization-sections"></a>Kullanım verileri görselleştirme bölümleri ekleme

Çalışma kitapları, dört türde yerleşik kullanım analizi Görselleştirmelerini sunar. Her uygulamanızın kullanımıyla ilgili yaygın soru yanıtlar. Tablolar ve grafikler bu dört bölüm dışında eklemek için Analytics sorgu bölümleri (aşağıda görebilirsiniz) ekleyin.

Bir kullanıcı eklemek için oturumlar, etkinlikler veya bekletme bölümünde, çalışma kitabı kullanım **Add Users** veya çalışma kitabını alt kısmındaki ya da herhangi bir bölümü altındaki ilgili diğer düğme.

![Kullanıcılar bölümünde, çalışma kitapları](./media/app-insights-usage-workbooks/users-section.png)

**Kullanıcılar** bölümleri yanıt "kaç kullanıcının bazı sayfası görüntülenebilir veya bazı özelliği Sitem kullanılan?"

**Oturumlarının** bölümleri yanıt "kaç oturum kullanıcılar sayfa görüntüleme veya bazı Sitem özelliğini kullanarak harcama?"

**Olayları** bölümleri yanıt "kaç kez kullanıcılar bazı sayfasını görüntülemek veya bazı özelliğini Sitem?"

Bu üç bölüm türlerinin her biri, denetimleri ve görselleştirmeler aynı kümeleri sunar:

* [Kullanıcılar, oturumlar ve olaylar bölümleri düzenleme hakkında daha fazla bilgi edinin](app-insights-usage-segmentation.md)
* Ana hesap, histogram kılavuzlar, otomatik Öngörüler ve kullanarak örnek kullanıcılar görselleştirmeler geçiş **grafik**, **Göster kılavuz**, **Göster Insights**ve **Bu kullanıcılar örnek** her bölümün üst kısmındaki onay kutularını.

![Bekletme bölümünde çalışma kitapları](./media/app-insights-usage-workbooks/retention-section.png)

**Bekletme** bölümleri yanıt "bazı sayfası görüntülenebilir veya bazı özellik bir gün veya hafta kullanılan kişiler kaç geri bir sonraki gün veya hafta içinde gelen?"

* [Bekletme bölümleri düzenleme hakkında daha fazla bilgi edinin](app-insights-usage-retention.md)
* İsteğe bağlı toplam elde tutma süresi grafiği ile iki durumlu **toplam elde tutma grafiğini göster** bölümün üst kısmındaki onay kutusu.

## <a name="adding-application-insights-analytics-sections"></a>Application Insights Analytics bölümleri ekleme

![Analytics bölümünde çalışma kitapları](./media/app-insights-usage-workbooks/analytics-section.png)

Çalışma kitabınıza bir Application Insights Analytics sorgu bölümü eklemek için **ekleme Analytics sorgusu** çalışma kitabını sayfanın alt kısmında veya herhangi bir bölümü altındaki düğmeyi.

Analytics sorgu bölümler rastgele sorguları Application Insights verileriniz üzerinde çalışma kitabına eklemenizi sağlar. Bu esnekliğin Analytics sorgu bölümler, yukarıda kullanıcılar, oturumlar, olayları ve bekletme, gibi listelenen dört dışında sitenizin tüm sorulara yanıt vermek için gidilecek olmalıdır anlamına gelir:

* Kaç özel bir reddetme kullanımı ile aynı süre boyunca sitenizin durumunu oluşturamadı?
* Sayfa yükleme sürelerinin sayfa görüntüleme kullanıcılar için dağıtım neydi?
* Kaç kullanıcının bazı sayfalar kümesi sitenizde görüntülenebilir, ancak olmayan bazı diğer sayfalarında ayarlanmış? Bu kümeleri farklı alt kümelerinde sitenizin işlevselliği kullanan kullanıcı olup olmadığını anlamak yararlı olabilir (kullanın `join` işleciyle `kind=leftanti` Log Analytics sorgu dili değiştiricisi).

Kullanım [Log Analytics sorgu dili başvurusu](https://docs.loganalytics.io/) sorguları yazma hakkında daha fazla bilgi edinmek için.

## <a name="adding-text-and-markdown-sections"></a>Metin ve Markdown bölümleri ekleme

Başlıklarını, açıklamaları ve yorum, çalışma kitaplarına ekleme yardımcı tablolar ve grafikler bir dizi bir anlatım açın. Çalışma kitapları destek metin bölümlerinde [Markdown söz dizimi](https://daringfireball.net/projects/markdown/) başlıklar, kalın, italik ve madde işaretli listeler gibi biçimlendirme metin.

Bir metin bölümüne çalışma kitabınıza eklemek için **metin eklemek** çalışma kitabını sayfanın alt kısmında veya herhangi bir bölümü altındaki düğmeyi.

## <a name="saving-and-sharing-workbooks-with-your-team"></a>Kaydetme ve çalışma kitapları takımınızla paylaşma

Çalışma kitapları, içinde bir Application Insights kaynağı kaydedilir **raporlarım** , hem de özel bölümü **paylaşılmış olan raporlar** erişimi olan herkesin erişebileceği bölümü Application Insights kaynağı. Kaynakta tüm çalışma kitaplarını görüntülemek için tıklayın **açık** eylem çubuğunda düğme.

Şu anda kullanımda bir çalışma kitabını paylaşmak için **raporlarım**:

1. Tıklayın **açık** eylem çubuğunda
2. Paylaşmak istediğiniz çalışma kitabını yanındaki "…" düğmesine tıklayın
3. Tıklayın **paylaşılan Raporlar'a Taşı**.

Bir çalışma kitabı bağlantısını içeren bir ya da e-posta ile paylaşmak için tıklatın **paylaşmak** eylem çubuğunda. Bağlantının alıcıları çalışma kitabını görüntülemek için Azure portalında bu kaynağa erişimi gerektiğini aklınızda bulundurun. Gereken en az alıcılar düzenlemeler yapmak için kaynak için katkıda bulunan izinleri.

Azure panosu için bir çalışma kitabı bağlantısını sabitlemek için:

1. Tıklayın **açık** eylem çubuğunda
2. Sabitlemek istediğiniz çalışma kitabının yanındaki "…" düğmesine tıklayın
3. Tıklayın **panoya Sabitle**.

## <a name="next-steps"></a>Sonraki adımlar

## <a name="next-steps"></a>Sonraki adımlar
- Kullanım deneyimlerini etkinleştirmek için göndermeye başlayın [özel olaylar](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) veya [sayfa görünümleri](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Özel olay veya sayfa görüntülemesi zaten gönderirseniz, kullanıcıların hizmetinizin nasıl öğrenmek için kullanım araçları keşfedin.
    - [Kullanıcılar, Oturumlar, Etkinlikler](app-insights-usage-segmentation.md)
    - [Huniler](usage-funnels.md)
    - [Bekletme](app-insights-usage-retention.md)
    - [Kullanıcı Akışları](app-insights-usage-flows.md)
    - [Kullanıcı bağlamı Ekle](app-insights-usage-send-user-context.md)
    
