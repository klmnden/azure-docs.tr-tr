---
title: Araştırmak ve kullanım verilerini Azure Application Insights etkileşimli çalışma kitaplarında paylaşma | Microsoft docs
description: Kullanıcılar, web uygulamanızın demografik analizini.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 06/12/2017
ms.author: mbullwin; daviste
ms.openlocfilehash: a871378b3e2cc0b34c925593c6f01952de3aa08e
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="investigate-and-share-usage-data-with-interactive-workbooks-in-application-insights"></a>Araştırmak ve kullanım verilerini Application Insights etkileşimli çalışma kitaplarında paylaşın

Çalışma kitapları birleştirmek [Azure Application Insights](app-insights-overview.md) veri görselleştirmeleri [analitik sorguları](app-insights-analytics.md)ve etkileşimli belgeleri metin. Çalışma kitapları aynı Azure kaynak erişimi olan diğer takım üyeleri tarafından düzenlenebilir. Bu, sorgular ve bir çalışma kitabı oluşturmak için kullanılan denetimleri, keşfetme, genişletmenizi ve hataları olup olmadığını denetleyin kolaylaşır çalışma kitabı okuma diğer kişilerin kullanılabilir anlamına gelir.

Çalışma kitapları gibi senaryolar için yararlı olur:

* İlgilenilen ölçümleri önceden tanımadığınız olduğunda, uygulamanızın kullanımını keşfetme: kullanıcılar, saklama hızları, dönüştürme oranları vb. sayısı. Application ınsights'ta diğer kullanım analiz araçları çalışma kitapları, Görselleştirme ve çözümlemeleri de serbest biçimli araştırması bu tür için harika yapmadan birden çok türde birleştirmek olanak tanır.
* Ekibiniz için yeni yayımlanan bir özelliğin nasıl gerçekleştirmekte açıklanması, gösteren kullanıcı tarafından anahtar etkileşimleri ve diğer ölçümleri sayar.
* Bir A sonuçlarını paylaşımı / B denemeler ekibinizin diğer üyeleriyle, uygulamanızda. Denemeyi metinle amaçlarını açıklayan sonra her bir kullanım ölçümü ve Temizle çağrı-her ölçümü üstüne veya altına-hedefi olup için aşımı ayarlarına birlikte deneme değerlendirmek için kullanılan Analytics sorgu göster.
* Bir kesinti etkisini veri, açıklama metnini ve gelecekte kesintileri önlemek için sonraki adımlar tartışması birleştirme uygulamanızı kullanımı hakkında raporlama.

> [!NOTE]
> Application Insights kaynağınıza, sayfa görünümleri veya çalışma kitaplarını kullanmak için özel olaylar içermesi gerekir. [Sayfa görünümleri Application Insights JavaScript SDK'sı ile otomatik olarak toplamak için uygulamanızı ayarlayın öğrenin](app-insights-javascript.md).
> 
> 

## <a name="editing-rearranging-cloning-and-deleting-workbook-sections"></a>Düzenleme, yeniden düzenleme, kopyalama ve çalışma kitabını bölümleri silme

Bir çalışma kitabı bölümlerini yapılmış bir durumda: bağımsız olarak düzenlenebilir kullanım görselleştirmeleri, grafikler, tablolar, metin veya Analytics sorgu sonuçları.

Çalışma kitabı bölümünün içeriğini düzenlemek için tıklatın **Düzenle** aşağıda ve çalışma kitabı bölümü sağındaki düğmesini.

![Düzenleme denetimleri uygulama Öngörüler çalışma kitaplarını bölümü](./media/app-insights-usage-workbooks/editing-controls.png)

1. Bitirdiğinizde bölüm düzenleme, tıklatın **yapılan düzenleme** bölümünün sol alt köşedeki içinde.

2. Bir bölümün bir kopyasını oluşturmak için tıklatın **bu bölümü kopyalama** simgesi. Yinelenen bölümleri oluşturma mükemmel bir sorguyu temel önceki yineleme kaybetmeden yinelemek şekilde bağlıdır.

3. Bir çalışma kitabı bölümünde yukarı taşımak için tıklatın **Yukarı Taşı** veya **Aşağı Taşı** simgesi.

4. Bir bölümün kalıcı olarak kaldırmak için tıklatın **kaldırmak** simgesi.

## <a name="adding-usage-data-visualization-sections"></a>Kullanım verileri görselleştirme bölümleri ekleme

Çalışma kitapları yerleşik kullanım analizi görselleştirmeleri dört tür sunar. Her bir soru kullanımı hakkında uygulamanızın yanıtlar. Tablolar ve grafikler dışında bu dört bölüm eklemek için Analytics sorgu bölümleri (aşağıda görebilirsiniz) ekleyin.

Bir kullanıcı eklemek için oturum, olayları veya bekletme bölümünde kitabınıza kullanım **Kullanıcı Ekle** veya diğer karşılık gelen bir düğme çalışma kitabının altındaki ya da herhangi bir bölümü altındaki.

![Kullanıcılar bölümünde çalışma kitapları](./media/app-insights-usage-workbooks/users-section.png)

**Kullanıcıların** bölümleri yanıt "kaç kullanıcının bazı sayfa görüntülenemez veya Sitem bazı özelliği kullanılan?"

**Oturumları** bölümleri yanıt "kaç oturumları kullanıcılar bazı sayfa görüntüleme veya Sitem bazı özelliğini kullanarak harcamanız?"

**Olayları** bölümleri yanıt "kaç kez kullanıcılar bazı sayfasını görüntüleyebilir veya Sitem bazı özelliğini kullanın?"

Bu üç bölüm türleri denetimleri ve görselleştirmeleri aynı kümesi sunar:

* [Kullanıcıları, oturumlar ve olayları bölümleri düzenleme hakkında daha fazla bilgi edinin](app-insights-usage-segmentation.md)
* Ana grafik, çubuk grafik kılavuzları, otomatik Öngörüler ve örnek kullanıcılar görselleştirmeleri kullanarak geçiş **Göster grafik**, **Göster kılavuz**, **Göster Öngörüler**ve **Bu kullanıcılar örnek** onay kutularını her bölümün üstünde.

![Çalışma kitapları bekletme bölümünde](./media/app-insights-usage-workbooks/retention-section.png)

**Bekletme** bölümleri yanıt "bazı sayfa görüntülenemez veya bir gün veya hafta bazı özellik kullanılan kişiler kaç tane geri bir sonraki gün veya hafta gelen?"

* [Bekletme bölümleri düzenleme hakkında daha fazla bilgi edinin](app-insights-usage-retention.md)
* İsteğe bağlı genel saklama grafiğiyle geçiş **Göster genel saklama grafik** bölümünün üst onay kutusu.

## <a name="adding-application-insights-analytics-sections"></a>Uygulama Öngörüler Analytics bölümleri ekleme

![Çalışma kitapları Analytics bölümünde](./media/app-insights-usage-workbooks/analytics-section.png)

Çalışma kitabınıza bir uygulama Öngörüler Analytics sorgu bölümü eklemek için kullanın **eklemek Analytics sorgu** çalışma kitabının altındaki ya da herhangi bir bölümü altındaki düğmesi.

Analytics sorgu bölümleri rasgele sorguları Application Insights verilerinizi çalışma kitaplarına eklemenize olanak sağlar. Bu esneklik, siteniz kullanıcıları, oturumlar, olayları ve bekletme, gibi yukarıda dört dışında hakkında tüm sorulara yanıt verilmesi için gidilecek Analytics sorgu bölümleri olmalıdır anlamına gelir:

* Kaç tane özel durumlar Reddet kullanımı ile aynı süre boyunca, sitenizi oluşturma?
* Sayfa yükleme sürelerinin kullanıcılar bazı sayfasını görüntülemek için dağıtım neydi?
* Kaç kullanıcının bazı sayfalar kümesi sitenizde görüntülenebilir, ancak olmayan bazı diğer sayfalarında ayarlamak? Bu, sitenizin işlevselliğin farklı alt kümelerini kullanan kullanıcılar, kümeleri olup olmadığını anlamak kullanışlı olabilir (kullanmak `join` işleciyle `kind=leftanti` günlük analizi sorgu dili değiştiricisi).

Kullanım [günlük analizi sorgu dili başvurusu](https://docs.loganalytics.io/) sorguları yazma hakkında daha fazla bilgi edinmek için.

## <a name="adding-text-and-markdown-sections"></a>Metin ve Markdown bölümleri ekleme

Başlıklar, açıklamalar ve yorumlar, çalışma kitaplarına ekleme yardımcı tablolar ve grafikler kümesi biçiminde anlatı açın. Çalışma kitapları destek metin bölümlerde [Markdown söz dizimi](https://daringfireball.net/projects/markdown/) başlıklar, kalın, italik ve madde işaretli listeler gibi biçimlendirme metin.

Çalışma kitabınıza metin bölümü eklemek için kullanın **metin eklemek** çalışma kitabının altındaki ya da herhangi bir bölümü altındaki düğmesi.

## <a name="saving-and-sharing-workbooks-with-your-team"></a>Kaydetme ve takımınızla çalışma kitaplarını paylaşma

Çalışma kitapları, bir Application Insights kaynağı içindeki kaydedilir **raporlarım** size veya içinde özel bölümü **paylaşılan raporları** erişimi olan herkes için erişilebilir bölümü Uygulama Insights kaynağıdır. Kaynak tüm çalışma kitaplarını görüntülemek için **açık** eylem çubuğunda düğmesi.

Şu anda kullanımda bir çalışma kitabını paylaşmak için **raporlarım**:

1. Tıklatın **açık** eylem çubuğunda
2. Paylaşmak istediğiniz çalışma kitabının yanındaki "..." düğmesini tıklatın
3. Tıklatın **taşımak için paylaşılan raporları**.

Bir çalışma kitabı bir bağlantıyla ya da e-posta ile paylaşmak için tıklatın **paylaşmak** eylem çubuğunda. Bağlantının alıcıları çalışma kitabını görüntülemek için Azure Portalı'nda bu kaynağa erişim gerektiğini aklınızda bulundurun. Gereken en az alıcılar düzenlemeleri yapmak için kaynak için katkıda bulunan izinleri.

Bir Azure Panoya bir çalışma kitabı bağlantısını sabitlemek için:

1. Tıklatın **açık** eylem çubuğunda
2. Sabitlemek istediğiniz çalışma kitabının yanındaki "..." düğmesini tıklatın
3. Tıklatın **panoya Sabitle**.

## <a name="next-steps"></a>Sonraki adımlar

## <a name="next-steps"></a>Sonraki adımlar
- Kullanımı deneyimleri etkinleştirmek için göndermeye Başla [özel olaylar](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) veya [sayfa görünümleri](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Özel olaylar veya sayfa görünümleri zaten gönderirseniz, kullanıcıların hizmetinizin kullanımını öğrenmek için kullanım araçları keşfedin.
    - [Kullanıcılar, Oturumlar, Etkinlikler](app-insights-usage-segmentation.md)
    - [Huniler](usage-funnels.md)
    - [Bekletme](app-insights-usage-retention.md)
    - [Kullanıcı Akışları](app-insights-usage-flows.md)
    - [Kullanıcı bağlamı Ekle](app-insights-usage-send-user-context.md)
    
