---
title: Azure Application Insights uygulamasında müşterilerinizin anlama | Microsoft Docs
description: Müşteriler, uygulamanızın nasıl kullandığını anlamak için Azure Application Insights kullanma Öğreticisi.
keywords: ''
services: application-insights
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/20/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: db61c300ad82270e59d315fa3372d9e4390c7a21
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2017
ms.locfileid: "24099030"
---
# <a name="use-azure-application-insights-to-understand-how-customers-are-using-your-application"></a>Müşteriler, uygulamanızın nasıl kullandığını anlamak için Azure Application Insights kullanın

Azure Application Insights, kullanıcılarınızın uygulamanızla nasıl etkileşim anlamanıza yardımcı olmak üzere kullanım bilgilerini toplar.  Bu öğreticide, bu bilgileri çözümlemek için kullanılabilir olan farklı kaynaklar açıklanmaktadır.  Şunları öğreneceksiniz nasıl yapılır:

> [!div class="checklist"]
> * Uygulamanızı erişen kullanıcılar hakkındaki ayrıntıları Çözümle
> * Müşteriler, uygulamanızın kullanımını analiz etmek için oturum bilgilerini kullanın
> * İstenen kullanıcı etkinliklerinizi gerçek etkinliklerini karşılaştırmanıza olanak tanıyan funnels tanımlayın 
> * Görselleştirme ve sorguları tek bir belgeye birleştirmek için bir çalışma kitabı oluşturun
> * Birlikte çözümlemek için benzer kullanıcı grubu
> * Hangi kullanıcıların uygulamanıza döndürme öğrenin
> * Kullanıcılarınızın uygulamanız aracılığıyla nasıl gezindiğini inceleyin.


## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

- [Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi aşağıdaki iş yükleri ile yükleyin:
    - ASP.NET ve web geliştirme
    - Azure geliştirme
- İndirme ve yükleme [Visual Studio anlık görüntü hata ayıklayıcısı](http://aka.ms/snapshotdebugger).
- .NET uygulaması azure'a dağıtma ve [Application Insights SDK'sı etkinleştirmek](app-insights-asp-net.md). 
- [Uygulamanızdan telemetri göndermesine](app-insights-usage-overview.md#send-telemetry-from-your-app) özel olaylar/sayfa görünümleri eklemek için
- Gönderme [kullanıcı bağlamı](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context) ne zaman içinde bir kullanıcı yaptığını izlemek ve kullanım özellikleri tam olarak kullanma.

## <a name="log-in-to-azure"></a>Azure'da oturum açma
Oturum açtığınızda Azure portalında [https://portal.azure.com](https://portal.azure.com).

## <a name="get-information-about-your-users"></a>Kullanıcılarınızın hakkında bilgi edinin
**Kullanıcılar** paneli, çeşitli yollarla, kullanıcılarınızın hakkında önemli ayrıntıları anlamanıza olanak sağlar. Where gibi bilgileri anlamak için bu panoyu kullanabilir, kullanıcılarınızın kendi istemci ayrıntılarını, bağlanan ve uygulamanızın hangi alanlarda bunlar eriştiğiniz. 

1. Seçin **Application Insights** ve aboneliğinizi seçin.
2. Seçin **kullanıcılar** menüde.
3. Varsayılan görünüm uygulamanız son 24 saat üzerinden bağlı benzersiz kullanıcıların sayısını gösterir.  Zaman penceresi değiştirin ve bu bilgileri filtrelemek için çeşitli diğer ölçütleri belirleyin.

    ![Sorgu Oluşturucusu](media\app-insights-tutorial-users\QueryBuilder.png)

6. Tıklatın **sırasında** açılır ve zaman penceresini 7 gün olarak değiştirin.  Bu, farklı grafiklerinde panelinde dahil edilen veri artırır.

    ![Zaman aralığını Değiştir](media\app-insights-tutorial-users\TimeRange.png)

4. Tıklatın **bölme** dökümünü kullanıcı özelliği tarafından grafiğe eklemek için açılır.  Seçin **ülke veya bölge**.  Grafik aynı veri içerir, ancak her ülkedeki kullanıcı sayısı dökümünü görüntülemenize izin verir.

    ![Ülke veya bölge grafiği](media\app-insights-tutorial-users\CountryorRegion.png)

5. İmleç grafikte farklı çubukları üzerine getirin ve sayısı her ülke için yalnızca bu çubuğu tarafından temsil edilen zaman penceresi gösterdiğine dikkat edin.
6. Bakın **Insights** kullanıcı verilerinizi çözümlemesi sağ sütun.  Bu kullanıcı verilerinin önemli oluşturan ortak özellikleri ile benzersiz içinde oturum sayısını kaydeder ve süre gibi bilgiler sağlar 

    ![Öngörüler sütun](media\app-insights-tutorial-users\insights.png)


## <a name="analyze-user-sessions"></a>Kullanıcı oturumlarının çözümleme
**Oturumları** Masası benzer **kullanıcılar** paneli.  Burada **kullanıcılar** , uygulamanızın erişen kullanıcılar hakkındaki ayrıntıları anlamanıza yardımcı olur **oturumları** kullanıcılarla uygulamanızın kullanımını anlamanıza yardımcı olur.  

1. Seçin **oturumları** menüde.
2. Grafik bakın ve filtrelemek ve verileri olarak bölmek için aynı seçeneklere sahip Not **kullanıcılar** paneli.

    ![Oturumları Sorgu Oluşturucusu](media\app-insights-tutorial-users\SessionsBuilder.png)

3. **Bu oturumlar örneği** sağdaki bölmesinde çok fazla sayıda olayı içerir oturumları listeler.  Analiz etmek için ilginç oturumları bunlar.

    ![Bu oturumlar örneği](media\app-insights-tutorial-users\SessionsSample.png)

4. Görüntülemek için oturumları birini tıklatın, **oturum zaman çizelgesi**, oturumlarında her eylem gösterir.  Bu, çok sayıda özel durumlar oturumlarıyla gibi bilgileri belirlemenize yardımcı olabilir.

    ![Oturumları zaman çizelgesi](media\app-insights-tutorial-users\SessionsTimeline.png)

## <a name="group-together-similar-users"></a>Birbirine benzer kullanıcı grubu
A **Kohort** kullanıcılar groupd benzer özelliklere üzerinde kümesidir.  Belirli kullanıcı grupları analiz etmenize izin vererek diğer paneller filtre verilere cohorts kullanabilirsiniz.  Örneğin, yalnızca bir satın alma tamamlandı kullanıcılar çözümlemek isteyebilirsiniz.

1.  Seçin **Cohorts** menüde.
2.  Tıklatın **yeni** yeni kohort oluşturmak için.
3.  Seçin **kimin kullanılan** açılır ve bir eylem seçin.  Yalnızca bu zaman penceresi içinde raporun gerçekleştirdiğini kullanıcılar dahil edilir.

    ![Belirtilen eylemleri kohort](media\app-insights-tutorial-users\CohortsDropdown.png)

4.  Seçin **kullanıcılar** menüde.
5.  İçinde **Göster** açılan listesinde, oluşturulan yeni kohort seçin.  Grafik verileri bu kullanıcılara sınırlıdır.

    ![Kullanıcıların aracında kohort](media\app-insights-tutorial-users\UsersCohort.png)


## <a name="compare-desired-activity-to-reality"></a>Gerçekte istenen etkinliğe Karşılaştır
Ne uygulamanızın kullanıcılarının yaptığını üzerinde önceki paneller odaklanmış sırada **Funnels** odaklanmak yapmak için kullanıcıların istiyor.  Bir huni uygulamanızda adımları kümesini ve adımları arasında taşıma kullanıcı yüzdesini temsil eder.  Örneğin, ürün arama uygulamanıza bağlanan kullanıcılar yüzdesini ölçer Huni oluşturabilirsiniz.  Daha sonra bu ürüne alışveriş sepeti ekleme kullanıcı yüzdesi ve bir satın almanızı tamamlayın olanlar yüzdesini görebilirsiniz.

1. Seçin **Funnels** menü ve ardından **yeni**. 

    ![](media\app-insights-tutorial-users\funnelsnew.png)

2. Yazın bir **Huni adı**.
3. Bir huni her adımı için bir eylem seçerek en az iki adımlara oluşturun.  Eylemlerin listesini Application Insights tarafından toplanan kullanım verileri oluşturulur.

    ![](media\app-insights-tutorial-users\funnelsedit.png)

4. Tıklatın **kaydetmek** Huni kaydedin ve ardından sonuçları görüntüleyin.  Huni sağındaki penceresine ilk etkinlik önce ve sonra kullanıcı tendencies belirli dizisi geçici anlamanıza yardımcı olması için son etkinlik en yaygın olayları gösterir.

    ![](media\app-insights-tutorial-users\funnelsright.png)


## <a name="learn-which-customers-return"></a>Hangi müşterilerin iade öğrenin
**Bekletme** , hangi kullanıcıların gelmeye uygulamanıza anlamanıza yardımcı olur.  

1. Seçin **bekletme** menüde.
2. Varsayılan olarak, herhangi bir eylem gerçekleştirilen ve herhangi bir eylemi gerçekleştirmek için döndürülen kullanıcıları çözümlenen bilgileri içerir.  Bu filtre, tüm dahil, örneğin, yalnızca bir satın alma tamamladıktan sonra döndürülen kullanıcılar değiştirebilirsiniz.

    ![](media\app-insights-tutorial-users\retentionquery.png)

3. Ölçütlere uyan döndürmeyi kullanıcılar grafik gösterilir ve form için farklı süreler tablo.  Tipik bir düzen zamanla kullanıcılar döndürme, aşamalı bir açılan içindir.  Bir süre ani bir açılan sonraki ilgili bir sorun tetikleyebilir. 

    ![](media\app-insights-tutorial-users\retentiongraph.png)

## <a name="analyze-user-navigation"></a>Kullanıcı Gezinti Çözümle
A **kullanıcı akışı** kullanıcılarınızın uygulamanızın özelliklerini ve sayfaları arasında nasıl gezindiğini visualizes.  Bu, burada kullanıcıların belirli bir sayfadan genellikle taşımak için nasıl uygulamanızı'bunlar genellikle çıkın ve düzenli aralıklarla yinelenen herhangi bir eylem olup olmadığını gibi soruları yanıtlamanıza yardımcı olur.

1.  Seçin **kullanıcı akışları** menüde.
2.  Tıklatın **yeni** yeni bir kullanıcı akışı oluşturun ve ardından **Düzenle** ayrıntılarını düzenlemek için.
3.  Artırmak **zaman aralığı** 7 gün ve ilk olay seçin.  Akış bu olayla Başlat kullanıcı oturumları izler.

    ![](media\app-insights-tutorial-users\flowsedit.png)

4.  Kullanıcı akışı görüntülenir ve farklı kullanıcı yollarını görebilir ve kullanıcıların oturumlarını sayar.  Mavi çizgiler kullanıcı sonra geçerli gerçekleştirilecek eylemi belirtir.  Kırmızı bir çizgi kullanıcı oturumunu sona gösterir.

    ![](media\app-insights-tutorial-users\flows.png)

5.  Bir olay akışından kaldırmak için tıklatın **x** eylemi ve ardından köşesindeki **grafik oluşturma**.  Grafik kaldırıldı bu olay'nın tüm örneklerini çizilir.  Tıklatın **Düzenle** olay şimdi eklendiğini görmeyi **olayları dışlanan**.

    ![](media\app-insights-tutorial-users\flowsexclude.png)

## <a name="consolidate-usage-data"></a>Kullanım verileri birleştirme
**Çalışma kitapları** veri görselleştirmeleri, analitik sorguları ve metin etkileşimli belgelere birleştirir.  Çalışma kitapları, ortak kullanım bilgilerini gruplamak, belirli bir olay bilgilerinden birleştirebilir veya geri ekibinizin uygulamanızın kullanımı raporu için kullanabilirsiniz.

1.  Seçin **çalışma kitaplarını** menüde.
2.  Tıklatın **yeni** yeni bir çalışma kitabı oluşturulamadı.
3.  Bir sorgu zaten bir çubuk grafiği görüntülenen son gün içinde tüm kullanım verilerini içeren sağlanır.  Bu sorgu, el ile düzenlemeden veya kullanabilirsiniz tıklatın **örnek sorguları** diğer yararlı sorgularından seçmek için.

    ![](media\app-insights-tutorial-users\samplequeries.png)

4.  Tıklatın **düzenleme Bitti**.
5.  Tıklatın **Düzenle** çalışma kitabını üstündeki metnini düzenlemek için üst bölmede.  Bu, markdown kullanarak biçimlendirilir.

    ![](media\app-insights-tutorial-users\markdown.png)

6.  Tıklatın **kullanıcıları eklemek** kullanıcı bilgilerini içeren bir grafik eklemek için.  İsterseniz ve ardından grafiği ayrıntılarını Düzenle **düzenleme Bitti** kaydetmek için.


## <a name="next-steps"></a>Sonraki adımlar
Kullanıcılarınızın analiz etme öğrendiğinize göre diğer yararlı verileri bu bilgilerle uygulamanız ile ilgili bir araya özel panolar oluşturmayı öğrenmek için sonraki öğretici için ilerleyin.

> [!div class="nextstepaction"]
> [Özel panolar oluşturun](app-insights-tutorial-dashboards.md)
