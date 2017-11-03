---
title: "Azure Application Insights kullanarak çalışma zamanı özel durumları tanılama | Microsoft Docs"
description: "Azure Application Insights kullanarak uygulamanızı çalışma zamanı özel durumları tanılamak ve Bul Öğreticisi."
services: application-insights
keywords: 
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/19/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 11e0f2f19acc843f1c558b5d0cfe84291035a6a5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="find-and-diagnose-run-time-exceptions-with-azure-application-insights"></a>Bulma ve Azure Application Insights ile çalışma zamanı özel durumları tanılama

Azure Application Insights telemetriyi uygulamanızdan belirlemenize ve çalışma zamanı özel durumları tanılamanıza yardımcı olmak için toplar.  Bu öğretici, uygulamanız bu işlemle gösterir.  Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Özel durum izleme etkinleştirmek için projenizi değiştirme
> * Farklı bileşenler için özel durumlar, uygulamanızın tanımlayın
> * Bir özel durum ayrıntıları görüntüle
> * Hata ayıklama için özel durumun bir anlık görüntü için Visual Studio indirme
> * Sorgu dili kullanarak başarısız isteklerin ayrıntıları Çözümle
> * Hatalı kodu düzeltmek için yeni bir iş öğesi oluşturma


## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

- [Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi aşağıdaki iş yükleri ile yükleyin:
    - ASP.NET ve web geliştirme
    - Azure geliştirme
- İndirme ve yükleme [Visual Studio anlık görüntü hata ayıklayıcısı](http://aka.ms/snapshotdebugger).
- Etkinleştirme [Visual Studio hata ayıklayıcısı anlık görüntüsünü alın](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-snapshot-debugger)
- .NET uygulaması azure'a dağıtma ve [Application Insights SDK'sı etkinleştirmek](app-insights-asp-net.md). 
- Öğretici, uygulamanızdaki bir özel durum kimliği izler, böylece kodunuzu bir özel durum oluşturmak için geliştirme veya test ortamına değiştirin. 

## <a name="log-in-to-azure"></a>Azure'da oturum açma
Oturum açtığınızda Azure portalında [https://portal.azure.com](https://portal.azure.com).


## <a name="analyze-failures"></a>Hataları çözümleme
Application Insights, uygulamanızda hatalar toplar ve en yüksek etkiye sahip olanlar çabalarınız odaklanmak yardımcı olması için farklı işlemler arasında kendi sıklığı görüntülemenize olanak tanır.  Ardından kök nedenini belirlemek için bu hatalar ayrıntılarına.   

1. Seçin **Application Insights** ve ardından, aboneliğinizin.  
1. Açmak için **hataları** ya da seçin panel **hataları** altında **Araştır** tıklayın veya menü **başarısız istekler** grafik.

    ![Başarısız istekler](media/app-insights-tutorial-runtime-exceptions/failed-requests.png)

2. **Başarısız istekler** paneli başarısız isteklerin ve sayısı, uygulama için her bir işlemin etkilenen kullanıcı sayısını gösterir.  Bu bilgileri kullanıcıya göre sıralayarak çoğu kullanıcıları etkileyen bu hataları tanımlayabilirsiniz.  Bu örnekte, **alma çalışanlar/Oluştur** ve **müşteriler/ayrıntıları almak** kendi sayıda hataları ve etkilenen kullanıcıların nedeniyle araştırmak için büyük olasılıkla adaylardır.  Bir işlem seçerek daha fazla sağ bölmede, bu işlem hakkında bilgi gösterir.

    ![İstekleri paneli başarısız oldu](media/app-insights-tutorial-runtime-exceptions/failed-requests-blade.png)

3. Burada bir depo hata oranı gösterir dönemi yakınlaştırmak için zaman penceresi azaltın.

    ![Başarısız istekler penceresi](media/app-insights-tutorial-runtime-exceptions/failed-requests-window.png)

4. Tıklatın **ayrıntıları görüntüle** işleminin ayrıntılarını görmek için.  Bu, topluca neredeyse yarım saniyenin tamamlamak için geçen iki başarısız bağımlılıklarının gösteren bir Gantt Grafik içerir.  Öğreticiyi izleyerek performans sorunlarını çözümleme hakkında daha fazla bilgi bulabilirsiniz [bulmak ve Azure Application Insights ile performans sorunlarını tanılamanıza](app-insights-tutorial-performance.md).

    ![Başarısız istek ayrıntıları](media/app-insights-tutorial-runtime-exceptions/failed-requests-details.png)

5. Operations ayrıntı hataya neden olan görülen bir FormatException da gösterir.  Özel durum'ı tıklatın veya **üst 3 özel durum türleri** ayrıntılarını görüntülemek için sayısı.  Geçersiz bir posta kodu nedeniyle olduğunu görebilirsiniz.

    ![Özel durum ayrıntıları](media/app-insights-tutorial-runtime-exceptions/failed-requests-exception.png)



## <a name="identify-failing-code"></a>Başarısız olan kodunu belirleyin
Anlık görüntü hata ayıklayıcı üretimde kök nedenini tanılamaya yardımcı olması için uygulamanızın en sık rastlanan özel durumlar anlık görüntüleri toplar.  Yığın ve her çağrı yığını çerçevesi en değişkenlerle incelemek çağrı görmek için Portalı'nda hata ayıklama anlık görüntüleri görüntüleyebilirsiniz. Kaynak kodu, anlık görüntü indirip Visual Studio 2017 içinde açma sonra ayıklayabilirsiniz.

1. Özel durum özelliklerinde tıklatın **açık hata ayıklama anlık görüntü**.
2. **Hata ayıklama anlık görüntü** paneli açılır istek için çağrı yığınına sahip.  İstek aynı anda tüm yerel değişkenlerin değerleri görüntülemek için herhangi bir yöntemini'ı tıklatın.  Bu örnekte top yöntemi başlayarak, değere sahip olmayan yerel değişkenler görebiliriz.

    ![Anlık görüntü hata ayıklama](media/app-insights-tutorial-runtime-exceptions/debug-snapshot-01.png)

4. Geçerli değerlere sahip ilk çağrısı **ValidZipCode**, ve bir posta kodu ile tamsayı çevrilmesi mümkün değil harfleri sağlandı görebiliriz.  Bu hata düzeltilmelidir kod gibi görünüyor.

    ![Anlık görüntü hata ayıklama](media/app-insights-tutorial-runtime-exceptions/debug-snapshot-02.png)

5. Bu anlık görüntü nerede biz bulabilir düzeltilmesi gereken gerçek bir kod Visual Studio'ya indirmek için tıklayın **karşıdan anlık görüntü**.
6. Anlık görüntü Visual Studio'ya yüklenir.
7. Visual Studio'da özel duruma neden kod satırı ile hızlı bir şekilde tanımlayan bir hata ayıklama oturumu şimdi çalıştırabilirsiniz.

    ![Özel durum kodu](media/app-insights-tutorial-runtime-exceptions/exception-code.png)


## <a name="use-analytics-data"></a>Kullanım analizi veri
Application Insights tarafından toplanan tüm veriler, çeşitli yollarla verileri çözümlemek sağlayan zengin bir sorgu dili sağlayan Azure günlük analizi depolanır.  Biz Araştırdığınız özel durum oluşturdu istekleri çözümlemek için size bu verileri kullanabilirsiniz. 

8. Application Insights tarafından sağlanan telemetri görüntülemek için kodu yukarıdaki CodeLens bilgi tıklatın.

    ![Kod](media/app-insights-tutorial-runtime-exceptions/codelens.png)

9. Tıklatın **etkilerini analiz** uygulama Öngörüler Analytics açın.  Başarısız isteklerde etkilenen kullanıcılar, tarayıcılar ve bölge gibi ayrıntıları sağlayan birkaç sorguları ile doldurulur.<br><br>![Analizler](media/app-insights-tutorial-runtime-exceptions/analytics.png)<br>

## <a name="add-work-item"></a>İş öğesi ekleme
Visual Studio Team Services veya GitHub gibi izleme sistemine Application Insights bağlanıyorsanız, Application Insights ' doğrudan bir iş öğesi oluşturabilirsiniz.

1. Geri dönüp **özel durumu özellikleri** Application Insights panelinde.
2. Tıklatın **yeni iş öğesi**.
3. **Yeni iş öğesi** paneli açılır zaten doldurulmuş özel durum ayrıntıları ile.  Kaydetmeden önce ek bilgiler ekleyebilirsiniz.

    ![Yeni iş öğesi](media/app-insights-tutorial-runtime-exceptions/new-work-item.png)

## <a name="next-steps"></a>Sonraki adımlar
Çalışma zamanı özel durumları tanımlamak nasıl öğrendiğinize göre performans sorunlarını tanılamak ve belirlemek öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Performans sorunlarını belirle](app-insights-tutorial-performance.md)