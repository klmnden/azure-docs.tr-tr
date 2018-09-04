---
title: Azure Application Insights kullanarak çalışma zamanı özel durumlarını tanılama | Microsoft Docs
description: Azure Application Insights kullanarak uygulamanızdaki çalışma zamanı özel durumlarını bulma ve tanılama hakkındaki öğretici.
services: application-insights
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/19/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 7c2e67605cd2489f2c8d9da5ac80386056464afa
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42815122"
---
# <a name="find-and-diagnose-run-time-exceptions-with-azure-application-insights"></a>Azure Application Insights ile çalışma zamanı özel durumlarını bulma ve tanılama

Azure Application Insights, uygulamanızdan çalışma zamanı özel durumlarının belirlenip tanılanmasına yardımcı olan telemetri verileri toplar.  Bu öğreticide, uygulamanızda bu işlemi gerçekleştirme adımları gösterilir.  Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Özel durum izlemeyi etkinleştirmek için projenizi değiştirme
> * Uygulamanızın farklı bileşenlerine yönelik özel durumları belirleme
> * Bir özel durumun ayrıntılarını görüntüleme
> * Hata ayıklama için özel durumun anlık görüntüsünü Visual Studio’ya indirme
> * Sorgu dilini kullanarak başarısız isteklerin ayrıntılarını analiz etme
> * Hatalı kodu düzeltmek için yeni iş öğesi oluşturma


## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

- [Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi aşağıdaki iş yükleri ile yükleyin:
    - ASP.NET ve web geliştirme
    - Azure geliştirme
- [Visual Studio Snapshot Debugger](http://aka.ms/snapshotdebugger)’ı indirin ve yükleyin.
- [Visual Studio Snapshot Debugger](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger)’ı etkinleştirme
- Azure’a .NET uygulaması dağıtma ve [Application Insights SDK’sını etkinleştirme](app-insights-asp-net.md). 
- Bu öğretici uygulamanızdaki bir özel durumun belirlenmesini izlediğinden, geliştirme veya test ortamındaki kodunuzu özel durum oluşturacak şekilde değiştirin. 

## <a name="log-in-to-azure"></a>Azure'da oturum açma
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.


## <a name="analyze-failures"></a>Hataları analiz etme
Application Insights, uygulamanızdaki tüm hataları toplar ve bunların farklı işlemlerdeki sıklığını görüntüleyerek etki düzeyi en yüksek olan hatalara odaklanmanıza yardımcı olur.  Daha sonra bu hataların ayrıntılarına inerek kök nedeni belirleyebilirsiniz.   

1. **Application Insights**’ı ve sonra aboneliğinizi seçin.  
2. **Hatalar** panelini açmak için **Araştır** menüsü altından **Hatalar**’ı seçin veya **Başarısız istekler** grafiğine tıklayın.

    ![Başarısız istekler](media/app-insights-tutorial-runtime-exceptions/failed-requests.png)

3. **Başarısız istekler** panelinde, uygulama için başarısız istek sayısı ve her işlemden etkilenen kullanıcı sayısı gösterilir.  Bu bilgileri kullanıcı sayısına göre sıralayarak kullanıcıları en çok etkileyen hataları bulabilirsiniz.  Bu örnekte, olası araştırma adayları hata sayısı ve etkilenen kullanıcı sayısı yüksek olan **GET Employees/Create** ve **GET Customers/Details** işlemleridir.  Bir işlem seçildiğinde, sağdaki panelde işlemle ilgili daha fazla bilgi gösterilir.

    ![Başarısız istekler paneli](media/app-insights-tutorial-runtime-exceptions/failed-requests-blade.png)

4. Hata oranında ani bir artışın yaşandığı döneme odaklanmak için zaman aralığını daraltın.

    ![Başarısız istekler penceresi](media/app-insights-tutorial-runtime-exceptions/failed-requests-window.png)

5. Filtrelenmiş sonuç sayısının bulunduğu düğmeye tıklayarak ilgili örneklere göz atın. "Önerilen" örnekler, örnekleme herhangi birinde etkili olmuş olsa bile tüm bileşenler arasından ilgili telemetriye sahiptir. Hata ayrıntılarını görmek için bir arama sonucuna tıklayın.

    ![Başarısız istek örnekleri](media/app-insights-tutorial-runtime-exceptions/failed-requests-search.png)

6. Başarısız istek ayrıntılarında Gannt grafiği görüntülenir. Grafikte, bu işlemde, işlemin toplam süresinin % 50'sinden fazlasını oluşturan iki adet bağımlılık hatası olduğu gösterilmektedir. Bu deneyim, dağıtılmış bir uygulamanın bileşenleri arasında bu işlem kimliğiyle ilişkili tüm telemetrileri sunar. [Yeni deneyim hakkında daha fazla bilgi edinin](app-insights-transaction-diagnostics.md). Sağ tarafta, ayrıntılarını görmek istediğiniz öğelerden herhangi birini seçebilirsiniz. 

    ![Başarısız istek ayrıntıları](media/app-insights-tutorial-runtime-exceptions/failed-request-details.png)

7. İşlemlerin ayrıntısında hataya yol açmış gibi görünen bir FormatException da gösterilir.  Sorunun geçersiz bir posta kodundan kaynaklandığını görebilirsiniz. Visual Studio'da kod düzeyinde hata ayıklama bilgilerini görmek için hata ayıklama anlık görüntüsünü açabilirsiniz.

    ![Özel durum ayrıntıları](media/app-insights-tutorial-runtime-exceptions/failed-requests-exception.png)

## <a name="identify-failing-code"></a>Başarısız olan kodu belirleme
Snapshot Debugger, uygulamanızda en sık karşılaşılan özel durumların anlık görüntülerini toplayarak üretimde sorunun kök nedenini tanılamanıza yardımcı olur.  Hata ayıklama anlık görüntülerini portalda görüntüleyerek çağrı yığınını görebilir ve her bir çağrı yığını çerçevesinde değişkenleri inceleyebilirsiniz. Daha sonra anlık görüntüyü indirip Visual Studio 2017’de açarak kaynak kodundaki hataları ayıklayabilirsiniz.

1. Özel durumun özelliklerinden **Hata ayıklama anlık görüntüsünü aç**’a tıklayın.
2. İsteğe yönelik çağrı yığınıyla birlikte **Hata Ayıklama Anlık Görüntüsü** paneli açılır.  Tüm yerel değişkenlerin istek sırasında sahip olduğu değerleri görüntülemek için herhangi bir metoda tıklayın.  Başta bu örnekte en çok kullanılan metot olmak üzere değeri olmayan yerel değişkenleri görebiliriz.

    ![Hata ayıklama anlık görüntüsü](media/app-insights-tutorial-runtime-exceptions/debug-snapshot-01.png)

4. Geçerli değerlere sahip olan ilk çağrı **ValidZipCode** çağrısıdır ve posta kodunun bir tamsayıya çevrilemeyen harflerle sağlandığını görebiliriz.  Kodda düzeltilmesi gereken hata bu gibi görünüyor.

    ![Hata ayıklama anlık görüntüsü](media/app-insights-tutorial-runtime-exceptions/debug-snapshot-02.png)

5. Bu anlık görüntüyü düzeltilmesi gereken kodun kendisini bulabileceğimiz Visual Studio’ya indirmek için **Anlık Görüntüyü İndir**’e tıklayın.
6. Anlık görüntü Visual Studio'ya yüklenir.
7. Artık Visual Studio’da özel duruma yol açan kod satırının hızlıca belirlendiği bir hata ayıklama oturumu çalıştırabilirsiniz.

    ![Kodda özel durum](media/app-insights-tutorial-runtime-exceptions/exception-code.png)


## <a name="use-analytics-data"></a>Analiz verilerini kullanma
Application Insights tarafından toplanan tüm veriler, bunları çeşitli yollarla analiz edebilmeniz için zengin bir sorgu dili sağlayan Azure Log Analytics’te depolanır.  Bu verileri kullanarak araştırmakta olduğumuz özel durumu oluşturan istekleri analiz edebiliriz. 

8. Application Insights tarafından sağlanan telemetri verilerini görüntülemek için kodun üzerindeki CodeLens bilgilerine tıklayın.

    ![Kod](media/app-insights-tutorial-runtime-exceptions/codelens.png)

9. **Etkiyi çözümleyin**’e tıklayarak Application Insights Analytics’i açın.  Analytics, başarısız isteklerle ilgili olarak etkilenen kullanıcılar, tarayıcılar ve bölgeler gibi ayrıntıları sağlayan çeşitli sorgularla doldurulur.<br><br>![Analizler](media/app-insights-tutorial-runtime-exceptions/analytics.png)<br>

## <a name="next-steps"></a>Sonraki adımlar
Artık çalışma zamanı özel durumlarının nasıl belirleneceğini öğrendiğinize göre, performans sorunlarını belirlemeyi ve tanılamayı öğrenmek için bir sonraki öğreticiye ilerleyebilirsiniz.

> [!div class="nextstepaction"]
> [Performans sorunlarını belirleme](app-insights-tutorial-performance.md)