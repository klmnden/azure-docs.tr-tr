---
title: "Azure Application Insights kullanarak performans sorunlarını tanılama | Microsoft Docs"
description: "Azure Application Insights kullanarak uygulamanızdaki performans sorunlarını bulma ve tanılama hakkındaki öğretici."
services: application-insights
keywords: 
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/18/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 26f5acf369dd80d7877ab760806e0e08a49cfe6d
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2018
---
# <a name="find-and-diagnose-performance-issues-with-azure-application-insights"></a>Azure Application Insights ile performans sorunlarını bulma ve tanılama

Azure Application Insights, uygulamanızdan çalışma ve performans analizine yardımcı olan telemetri verileri toplar.  Bu bilgileri kullanarak gerçekleşmekte olan sorunları belirleyebilir veya uygulamanın geliştirilmesi durumunda kullanıcıları en çok etkileyecek özelliklerini belirleyebilirsiniz.  Bu öğreticide hem uygulamanızın sunucu bileşenlerinin performansını hem de istemcinin bakış açısını analiz etme süreci adım adım gösterilir.  Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Sunucu tarafı işlemlerin performansını belirleme
> * Düşük performansın kök nedenini belirlemek için sunucu işlemlerini analiz etme
> * En yavaş istemci tarafı işlemleri belirleme
> * Sorgu dilini kullanarak sayfa görüntülemelerinin ayrıntılarını analiz etme


## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

- [Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi aşağıdaki iş yükleri ile yükleyin:
    - ASP.NET ve web geliştirme
    - Azure geliştirme
- Azure’a .NET uygulaması dağıtma ve [Application Insights SDK’sını etkinleştirme](app-insights-asp-net.md).
- Uygulamanız için [Application Insights profil oluşturucuyu etkinleştirme](app-insights-profiler.md#installation).

## <a name="log-in-to-azure"></a>Azure'da oturum açma
[https://portal.azure.com](https://portal.azure.com) adresinde Azure portalında oturum açın.

## <a name="identify-slow-server-operations"></a>Yavaş sunucu işlemlerini belirleme
Application Insights, uygulamanızdaki farklı işlemlerin performans ayrıntılarını toplar.  Süresi en uzun olan işlemleri belirleyerek olası sorunları veya sürmekte olan geliştirme sürecinizin en iyi hedefini tanılayabilir ve uygulamanın genel performansını geliştirebilirsiniz.

1. **Application Insights**’ı ve sonra aboneliğinizi seçin.  
1. **Performans** panelini açmak için **Araştır** menüsü altından **Performans**’ı seçin veya **Sunucu Yanıt Süresi** grafiğine tıklayın.

    ![Performans](media/app-insights-tutorial-performance/performance.png)

2. **Performans** panelinde uygulamaya ait her işlemin sayısı ve ortalama süresi gösterilir.  Bu bilgileri kullanarak kullanıcıları en çok etkileyen işlemleri bulabilirsiniz. Bu örnekte, olası araştırma adayları süresi ve çağrı sayısı nispeten yüksek olan **GET Customers/Details** ve **GET Home/Index** işlemleridir.  Süresi yüksek olsa da nadiren çağrıldığı için geliştirilmesinin çok etkili olmayacağı başka işlemler olabilir.  

    ![Performans paneli](media/app-insights-tutorial-performance/performance-blade.png)

3. Grafikte şu anda tüm çağrıların zaman içindeki ortalama süresi gösterilmektedir.  İlgilendiğiniz işlemleri grafiğe sabitleyerek ekleyin.  Burada, araştırmaya değer bazı tepe noktaları olduğu görülmektedir.  Grafiğin zaman aralığını kısaltarak sorunu daha iyi yalıtın.

    ![İşlemleri sabitleme](media/app-insights-tutorial-performance/pin-operations.png)

4.  Bir işleme tıklayarak işlemin sağ tarafta açılan performans panelini görüntüleyin. Bu panelde farklı istekler için süre dağılımı gösterilir.  Kullanıcılar genellikle yaklaşık yarım saniyelik bir düşük performans fark ettiğinden, istek aralığını 500 milisaniyenin üzerine düşürün.  

    ![Süre dağılımı](media/app-insights-tutorial-performance/duration-distribution.png)

5.  Bu örnekte, önemli sayıda isteğin işlenmesinin bir saniyeden uzun sürdüğünü görebilirsiniz. **İşlem ayrıntıları**’na tıklayarak bu işlemin ayrıntılarını görebilirsiniz.

    ![İşlem ayrıntıları](media/app-insights-tutorial-performance/operation-details.png)

    > [!NOTE]
    Tek bir tam ekran görünümünde istekler, bağımlılıklar, özel durumlar, izlemeler, olaylar, vb. sunucu tarafı telemetri verilerinin tamamını görmek için "Unified details: E2E Transaction Diagnostics" (Birleşik ayrıntılar: E2E İşlem Tanılama) [önizleme deneyimini](app-insights-previews.md) etkinleştirin. 

    Önizleme etkinleştirildiğinde, bağımlılık çağrılarına harcanan sürenin yanı sıra herhangi bir hatayı veya özel durumu birleşik bir deneyimde görebilirsiniz. Bileşenler arası işlemler için Gantt grafiğinin yanı sıra ayrıntılar bölmesi, sorunun kök nedeni olan bileşeni, bağımlılığı veya özel durumu hızlıca tanılamanıza yardımcı olabilir. Alttaki bölümü genişleterek seçili bileşen işlemi için toplanan tüm izlemelerin veya olayların zaman sıralamasını görebilirsiniz. [Yeni deneyim hakkında daha fazla bilgi edinin](app-insights-transaction-diagnostics.md)  

    ![İşlem tanılamaları](media/app-insights-tutorial-performance/e2e-transaction-preview.png)


6.  Şimdiye kadar topladığınız bilgiler yalnızca performansın yavaş olduğunu gösterir, ancak kök nedenin bulunması konusunda pek yararlı değildir.  **Profil oluşturucu**, işlem için çalıştırılan kodun kendisini ve her adım için gereken süreyi göstererek bu konuda yardımcı olur. Profil oluşturucu belirli aralıklarla çalıştığından, bazı işlemlerin izlemesi olmayabilir.  Zamanla daha fazla işlemin izlemesi olmalıdır.  İşlem için profil oluşturucuyu başlatmak için **Profiler izlemeleri**’ne tıklayın.
5.  İzlemede her işleme yönelik olaylar tek tek gösterildiğinden, genel işlem süresinin kök nedenini tanılayabilirsiniz.  Üstteki en uzun süreye sahip örneklerden birine tıklayın.
6.  Toplam işlem süresinin en büyük bölümünü oluşturan olaylara özgü yolu vurgulamak için **Etkin Yolu Göster**’e tıklayın.  Bu örnekte, en yavaş çağrının *FabrikamFiberAzureStorage.GetStorageTableData* metodundan geldiğini görebilirsiniz. En çok zaman alan bölüm *CloudTable.CreateIfNotExist* metodudur. İşlev her çağrıldığında bu kod satırı yürütülürse gereksiz ağ çağrısı ve CPU kaynağı tüketilir. Kodunuzu düzeltmenin en iyi yolu, bu satırı yalnızca bir kere yürütülen bir başlangıç yöntemine eklemektir. 

    ![Profil oluşturucu ayrıntıları](media/app-insights-tutorial-performance/profiler-details.png)

7.  Ekranın üstündeki **Performans İpucu**, sürenin uzun olmasının beklemekten kaynaklandığı değerlendirmesini destekliyor.  Farklı olay türlerinin yorumlanmasına yönelik belgeler için **bekliyor** bağlantısına tıklayın.

    ![Performans ipucu](media/app-insights-tutorial-performance/performance-tip.png)

8.  Daha fazla analiz için **.etl izlemesini indir**’e tıklayarak izlemeyi Visual Studio’ya indirebilirsiniz.

## <a name="use-analytics-data-for-server"></a>Sunucuya yönelik analiz verilerini kullanma
Application Insights Analytics, Application Insights tarafından toplanan tüm verileri analiz etmeye yönelik zengin bir sorgu dili sağlar.  Bunu kullanarak istek ve performans verileri üzerinde ayrıntılı analiz gerçekleştirebilirsiniz.

1. İşlem ayrıntıları paneline dönün ve Analytics düğmesine tıklayın.

    ![Analytics düğmesi](media/app-insights-tutorial-performance/server-analytics-button.png)

2. Paneldeki görünümlerin her birine yönelik bir sorgu içeren Application Insights Analytics açılır.  Bu sorguları olduğu gibi çalıştırabilir veya gereksinimlerinize göre özelleştirebilirsiniz.  İlk sorgu, zaman içinde bu işlemin süresini gösterir.

    ![Analiz](media/app-insights-tutorial-performance/server-analytics.png)


## <a name="identify-slow-client-operations"></a>Yavaş istemci işlemlerini belirleme
Application Insights, iyileştirilecek sunucu işlemlerini belirlemeye ek olarak istemci tarayıcılarının bakış açısını da analiz edebilir.  Bu, istemci bileşenlerine yönelik olası geliştirme fırsatlarını, hatta farklı tarayıcı veya konumlarla ilgili sorunları belirlemenize yardımcı olabilir.

1. Tarayıcı özetini açmak için **Araştır** bölümünden **Tarayıcı**’yı seçin.  Burada, tarayıcının bakış açısından uygulamanızın çeşitli telemetri verilerinin görsel bir özetini sağlar.

    ![Tarayıcı özeti](media/app-insights-tutorial-performance/browser-summary.png)

2.  Ekranı aşağı kaydırarak **En yavaş sayfalarım hangileri?** bölümüne gidin.  Burada, uygulamanızın istemciler tarafından yüklenmesi en uzun süren sayfalarının bir listesi gösterilir.  Bu bilgileri kullanarak kullanıcıları en çok etkileyen sayfalara öncelik verebilirsiniz.
3.  Sayfalardan birine tıklayarak **Sayfa görüntüleme** panelini açın.  Örnekte, **/FabrikamProd** sayfasının ortalama süresinin aşırı uzun olduğu görülmektedir.  **Sayfa görüntüleme** panelinde, bu sayfayla ilgili olarak farklı süre aralıklarının dökümünü de içeren ayrıntılar sağlanır.

    ![Sayfa görüntüleme](media/app-insights-tutorial-performance/page-view.png)

4.  Bu isteklerin ayrıntılarını incelemek için en yüksek süreye tıklayın.  Sonra, tarayıcı türü ve konumu dahil olmak üzere sayfa isteğinde bulunan istemcinin ayrıntılarını görüntülemek için ilgili isteğe tıklayın.  Bu bilgiler, belirli istemci türleriyle ilgili performans sorunları olup olmadığını belirlemenize yardımcı olabilir.

    ![İstek ayrıntıları](media/app-insights-tutorial-performance/request-details.png)

## <a name="use-analytics-data-for-client"></a>İstemci için analiz verilerini kullanma
Sunucu performansı için toplanan verilerde olduğu gibi, Application Insights tüm istemci verilerinin Analytics ile ayrıntılı analiz gerçekleştirmek için kullanılmasına imkan tanır.

1. Tarayıcı özetine dönün ve Analytics simgesine tıklayın.

    ![Analytics simgesi](media/app-insights-tutorial-performance/client-analytics-icon.png)

2. Paneldeki görünümlerin her birine yönelik bir sorgu içeren Application Insights Analytics açılır. İlk sorgu, zaman içinde farklı sayfa görüntüleme işlemlerinin süresini gösterir.

    ![Analiz](media/app-insights-tutorial-performance/client-analytics.png)

3.  Akıllı Tanılama, Application Insights Analytics’in verilerdeki benzersiz desenleri tanımlayan bir özelliğidir.  Çizgi grafikte Akıllı Tanılama noktasına tıkladığınızda, aynı sorgu anomaliye yol açan kayıtlar olmadan çalıştırılır.  Sürenin aşırı uzun olmasına yol açan sayfa görüntülemelerini tanımlayabilmeniz için sorgunun açıklama bölümünde bu kayıtların ayrıntıları gösterilir.

    ![Akıllı Tanılama](media/app-insights-tutorial-performance/client-smart-diagnostics.png)


## <a name="next-steps"></a>Sonraki adımlar
Artık çalışma zamanı özel durumlarının nasıl belirleneceğini öğrendiğinize göre, hatalara yanıt olarak uyarı oluşturmayı öğrenmek için bir sonraki öğreticiye ilerleyebilirsiniz.

> [!div class="nextstepaction"]
> [Uygulama durumuyla ilgili uyarılar](app-insights-tutorial-alert.md)
