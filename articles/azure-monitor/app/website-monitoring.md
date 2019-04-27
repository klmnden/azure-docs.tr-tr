---
title: 'Hızlı başlangıç: Azure İzleyici Application Insights ile web sitelerini izleme | Microsoft Docs'
description: Azure İzleyici Application Insights hizmetiyle hızlı bir şekilde istemci/tarayıcı tarafı web sitesi izleme kurulumu gerçekleştirme yönergeleri
services: application-insights
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.date: 10/29/2018
ms.service: application-insights
ms.custom: mvc
ms.topic: quickstart
manager: carmonm
ms.openlocfilehash: a35f4d8c1d5bf5943ecba02ff262fbc7fc0730fe
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60783882"
---
# <a name="start-monitoring-your-website"></a>Web sitenizi izlemeye başlama

Azure İzleyici Application Insights ile web sitenizi kullanılabilirlik, performans ve kullanım bakımından kolayca izleyebilirsiniz. Ayrıca, bir kullanıcının bildirmesini beklemeden uygulamanızdaki hataları hızlıca tanımlayıp tespit edebilirsiniz. Application Insights hem sunucu tarafı izleme hem de istemci/tarayıcı tarafı izleme özellikleri sunar.

Bu hızlı başlangıçta, eklerken size kılavuzluk eder [açık kaynak Application Insights JavaScript SDK'sı](https://github.com/Microsoft/ApplicationInsights-JS) sitenizi ziyaret edenler için tarayıcı/istemci-tarafı deneyimini anlama sağlar.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için:

- Bir Azure Aboneliğine sahip olmanız gerekir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="enable-application-insights"></a>Application Insights'ı etkinleştirme

Application Insights, şirket içinde veya bulutta çalışan İnternet’e bağlı herhangi bir uygulamadan telemetri verilerini toplayabilir. Bu verileri görüntülemeyi başlatmak için aşağıdaki adımları kullanın.

1. **Kaynak oluştur** > **Yönetim araçları** > **Application Insights** seçeneğini belirleyin.

   Bir yapılandırma kutusu görünür. Giriş alanlarını doldurmak için aşağıdaki tabloyu kullanın.

    | Ayarlar        | Değer           | Açıklama  |
   | ------------- |:-------------|:-----|
   | **Ad**      | Genel Olarak Benzersiz Değer | İzlemekte olduğunuz uygulamayı tanımlayan ad |
   | **Uygulama Türü** | Genel Uygulama | İzlemekte olduğunuz uygulamanın türü |
   | **Kaynak Grubu**     | myResourceGroup      | App Insights verilerini barındıran yeni kaynak grubunun adı |
   | **Konum** | Doğu ABD | Yakınınızda bulunan veya uygulamanızın barındırıldığı konumun yakınında olan bir konum seçin |

2. **Oluştur**’a tıklayın.

## <a name="create-an-html-file"></a>HTML dosyası oluşturma

1. Yerel bilgisayarınızda ``hello_world.html`` adlı bir dosya oluşturun. Bu örnekte dosya, C: sürücüsünün köküne eklenecek ve ``C:\hello_world.html`` yoluna sahip olacaktır.
2. Aşağıdaki betiği ``hello_world.html`` dosyasına kopyalayın:

    ```html
    <!DOCTYPE html>
    <html>
    <head>
    <title>Azure Monitor Application Insights</title>
    </head>
    <body>
    <h1>Azure Monitor Application Insights Hello World!</h1>
    <p>You can use the Application Insights JavaScript SDK to perform client/browser-side monitoring of your website. To learn about more advanced JavaScript SDK configurations visit the <a href="https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md" title="API Reference">API reference</a>.</p>
    </body>
    </html>
    ```

## <a name="configure-app-insights-sdk"></a>App Insights SDK’sını Yapılandırma

1. **Genel Bakış** > **Temel Bilgiler**’i seçin > Uygulamanızın **İzleme Anahtarı**’nı kopyalayın.

   ![Yeni App Insights kaynağı formu](media/website-monitoring/instrumentation-key-001.png)

2. Aşağıdaki betiği ``hello_world.html`` dosyanıza, ``</head>`` kapanış etiketinin öncesine ekleyin:

   ```javascript
      <script type="text/javascript">
        var appInsights=window.appInsights||function(a){
            function b(a){c[a]=function(){var b=arguments;c.queue.push(function(){c[a].apply(c,b)})}}var c={config:a},d=document,e=window;setTimeout(function(){var b=d.createElement("script");b.src=a.url||"https://az416426.vo.msecnd.net/scripts/a/ai.0.js",d.getElementsByTagName("script")[0].parentNode.appendChild(b)});try{c.cookie=d.cookie}catch(a){}c.queue=[];for(var f=["Event","Exception","Metric","PageView","Trace","Dependency"];f.length;)b("track"+f.pop());if(b("setAuthenticatedUserContext"),b("clearAuthenticatedUserContext"),b("startTrackEvent"),b("stopTrackEvent"),b("startTrackPage"),b("stopTrackPage"),b("flush"),!a.disableExceptionTracking){f="onerror",b("_"+f);var g=e[f];e[f]=function(a,b,d,e,h){var i=g&&g(a,b,d,e,h);return!0!==i&&c["_"+f](a,b,d,e,h),i}}return c
        }({
            instrumentationKey: "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx"
        });
        
        window.appInsights=appInsights,appInsights.queue&&0===appInsights.queue.length&&appInsights.trackPageView();
   </script>
   ```

3. ``hello_world.html`` dosyasını düzenleyerek izleme anahtarınızı ekleyin.

4. ``hello_world.html`` dosyasını yerel tarayıcı oturumunda açın. Tek sayfalı bir görünüm oluşturulur. Tarayıcınızı yenileyerek birden fazla test sayfası görünümü oluşturabilirsiniz.

## <a name="start-monitoring-in-the-azure-portal"></a>Azure portalında izlemeyi başlatma

1. Artık izleme anahtarınızı aldığınız Application Insights **Genel Bakış** sayfasını yeniden açarak o anda çalışan uygulamanıza ilişkin ayrıntıları görüntüleyebilirsiniz. Genel bakış sayfasındaki dört varsayılan grafik, sunucu tarafı uygulama verilerini kapsar. JavaScript SDK ile istemci/tarayıcı tarafı etkileşimleri izlediğimiz için sunucu tarafı SDK'sı yüklü olmadığı sürece bu görünüm işimize yaramayacaktır.

2. ![Uygulama Haritası simgesi](media/website-monitoring/006.png) **Analiz** öğesine tıklayın.  Bu işlem, Application Insights tarafından toplanan tüm verileri analiz etmeye yönelik zengin bir sorgu dili sağlayan **Analiz** sayfasını açar. İstemci tarafı tarayıcı istekleriyle ilgili verileri görüntülemek için aşağıdaki sorguyu çalıştırın:

    ```kusto
    // average pageView duration by name
    let timeGrain=1s;
    let dataset=pageViews
    // additional filters can be applied here
    | where timestamp > ago(15m)
    | where client_Type == "Browser" ;
    // calculate average pageView duration for all pageViews
    dataset
    | summarize avg(duration) by bin(timestamp, timeGrain)
    | extend pageView='Overall'
    // render result in a chart
    | render timechart
    ```

   ![Belirli bir süre içindeki kullanıcı isteklerinin analiz grafiği](./media/website-monitoring/analytics-query.png)

3. **Genel Bakış** sayfasına geri gidin. **Araştır** başlığındaki **Tarayıcı** girişine tıklayın ve **Performans**'ı seçin. Burada web sitenizle ilgili ölçümleri ve performans verilerini görebilirsiniz. Ayrıca web sitenizdeki hataları ve özel durumları analiz etmek için kullanabileceğiniz bir görünüm de bulunur. **Örnekler**'e tıklayarak işlem ayrıntılarına inebilirsiniz. Açılan sayfadan [uçtan uca işlem ayrıntıları](../../azure-monitor/app/transaction-diagnostics.md) deneyimine ulaşabilirsiniz.

   ![Sunucu ölçüm grafiği](./media/website-monitoring/browser-performance.png)

4. [Kullanıcı davranışı analiz araçlarını](../../azure-monitor/app/usage-overview.md) keşfetmeye başlamak için ana Application Insights menüsündeki **Kullanım** başlığından [**Kullanıcılar**](../../azure-monitor/app/usage-segmentation.md) girişini seçin. Tek bir makineden test gerçekleştirdiğimiz için yalnızca bir kullanıcıya ait veriler görünecektir. Canlı bir web sitesi için kullanıcı dağılımı şu şekilde görünebilir:

     ![Kullanıcı grafiği](./media/website-monitoring/usage-users.png)

5. Birden fazla sayfaya sahip olan daha karmaşık bir web sitesini izliyor olsaydık [**Kullanıcı Akışları**](../../azure-monitor/app/usage-flows.md) aracından da faydalanabilirdik. **Kullanıcı Akışları** aracıyla ziyaretçilerin web sitenizin farklı bölümlerinde gerçekleştirdiği işlemleri görebilirsiniz.

   ![Kullanıcı Akışları görselleştirmesi](./media/website-monitoring/user-flows.png)

Web sitelerini izlemek için kullanabileceğiniz daha gelişmiş yapılandırmalar hakkında bilgi edinmek için [JavaScript SDK API başvurusunu](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md) inceleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki hızlı başlangıçlar veya öğreticilerle devam etmeyi planlıyorsanız, bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız Azure portalında bu hızlı başlangıç ile oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**’na tıklayın ve ardından **myResourceGroup**’a tıklayın.
2. Kaynak grubu sayfanızda, **Sil**’e tıklayın, metin kutusuna **myResourceGroup** yazın ve ardından **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Performans sorunlarını bulma ve tanılama](https://docs.microsoft.com/azure/application-insights/app-insights-analytics)
