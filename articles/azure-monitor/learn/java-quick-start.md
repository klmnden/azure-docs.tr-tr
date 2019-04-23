---
title: Azure Application Insights ile Hızlı Başlangıç | Microsoft Docs
description: Application Insights ile izleme için bir Java Web uygulaması hızlı bir şekilde ayarlamak için yönergeler sağlar
services: application-insights
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.reviewer: lagayhar
ms.date: 04/18/2019
ms.service: application-insights
ms.custom: mvc
ms.topic: quickstart
manager: carmonm
ms.openlocfilehash: e1574b55f9f14daba1831ba7f73b7f9ebde4c7f6
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60006905"
---
# <a name="start-monitoring-your-java-web-application"></a>Java Web Uygulamanızı İzlemeye Başlama

Azure Application Insights ile web uygulamanızı kullanılabilirlik, performans ve kullanım bakımından kolayca izleyebilirsiniz. Ayrıca, bir kullanıcının bildirmesini beklemeden uygulamanızdaki hataları hızlıca tanımlayıp tespit edebilirsiniz. Application Insights Java SDK ile MongoDB, MySQL ve Redis dahil olmak üzere yaygın üçüncü taraf paketleri izleyebilirsiniz.

Bu hızlı başlangıç, Application Insights SDK'sını var olan bir Java Dynamic Web Projesine eklerken size kılavuzluk eder.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için:

- JRE 1.7 veya 1.8 yükleme
- [Ücretsiz Java EE Geliştiricileri için Eclipse IDE](https://www.eclipse.org/downloads/) yükleyin. Bu hızlı başlangıçta Eclipse Oxygen (4.7) kullanılır.
- Bir Azure Aboneliği ve var olan bir Java Dynamic Web Projesi gerekir
 
Bir Java Dynamic Web Projeniz yoksa [Java web uygulaması oluşturma hızlı başlangıcı](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-java) ile bir tane oluşturabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

Spring framework tercih ediyorsanız [Application Insights Kılavuzu kullanmak için bir Spring Boot Başlatıcı uygulamasını yapılandırma](https://docs.microsoft.com/java/azure/spring-framework/configure-spring-boot-java-applicationinsights)

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="enable-application-insights"></a>Application Insights'ı etkinleştirme

Application Insights, şirket içinde veya bulutta çalışmasından bağımsız olarak İnternet’e bağlı herhangi bir uygulamadan telemetri verilerini toplayabilir. Bu verileri görüntülemeyi başlatmak için aşağıdaki adımları kullanın.

1. **Kaynak oluştur** > **Geliştirici araçları** > **Application Insights** seçeneğini belirleyin.

   ![Application Insights Kaynağı ekleme](./media/java-quick-start/1createresourseappinsights.png)

   ![Application Insights Kaynağı ekleme](./media/java-quick-start/2createjavaapp.png)

   Bir yapılandırma kutusu görünür. Giriş alanlarını doldurmak için aşağıdaki tabloyu kullanın.

    | Ayarlar        | Değer           | Açıklama  |
   | ------------- |:-------------|:-----|
   | **Ad**      | Genel Olarak Benzersiz Değer | İzlemekte olduğunuz uygulamayı tanımlayan ad |
   | **Uygulama Türü** | Java web uygulaması | İzlemekte olduğunuz uygulamanın türü |
   | **Kaynak Grubu**     | myResourceGroup      | App Insights verilerini barındıran yeni kaynak grubunun adı |
   | **Konum** | Doğu ABD | Yakınınızda bulunan veya uygulamanızın barındırıldığı konumun yakınında olan bir konum seçin |

2. **Oluştur**’a tıklayın.

## <a name="install-app-insights-plugin"></a>App Insights Eklentisini yükleme

1. **Eclipse**’i başlatın > **Yardım**’a tıklayın > **Yeni Yazılım Yükle**’yi seçin.

   ![Yeni App Insights kaynağı formu](./media/java-quick-start/000-j.png)

2. "Birlikte Çalış" alanına ```https://dl.microsoft.com/eclipse``` değerini kopyalayın > **Java için Azure Araç Seti**’ni işaretleyin > **Java için Application Insights Eklentisi**’ni seçin >  "Gerekli yazılımı bulmak için yükleme sırasında tüm güncelleştirme siteleriyle iletişime geçin" seçeneğinin işaretini kaldırın.

3. Yükleme tamamlandıktan sonra **Eclipse’i Yeniden Başlatmanız** istenir.

## <a name="configure-app-insights-plugin"></a>App Insights Eklentisini Yapılandırma

1. **Eclipse**’i başlatın > **Projenizi** açın > **Proje Gezgini**’nde proje adına sağ tıklayın > **Azure**’u seçin > **Oturum Aç**’a tıklayın.

2. **Etkileşimli** Kimlik Doğrulama Yöntemini seçin > **Oturum Aç**’a tıklayın > Sorulduğunda **Azure kimlik bilgilerinizi** girin > **Azure Aboneliğinizi** seçin.

3. **Proje Gezgini**’nde projenizin adına sağ tıklayın > **Azure**’u seçin > **Application Insights’ı Yapılandır**’a tıklayın.

4. **Application Insights ile telemetriyi etkinleştir**’i işaretleyin > App Insights kaynağını ve Java uygulamanıza bağlamak istediğiniz **İzleme Anahtarı**’nı seçin.

   ![Eclipse Azure Yapılandırma Menüsü](./media/java-quick-start/0007-j.png)

5. Application Insights eklentisi yapılandırdıktan sonra yapmanız [yayımlama/yeniden yayımlama](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-java#publish-the-web-app-to-azure) uygulamanızı yeniden önce onu telemetri göndermeye başlaması mümkün olacaktır.

> [!NOTE]
> Java için Application Insights SDK’sı canlı ölçümleri yakalama ve görselleştirme özelliğine sahiptir, ancak telemetri koleksiyonunuzu ilk kez etkinleştirdiğinizde verilerin portalda görünmeye başlaması birkaç dakika sürebilir. Bu uygulama düşük trafikli bir test uygulaması ise, çoğu ölçümün yalnızca etkin istek veya işlem olduğunda yakalandığını aklınızda bulundurun.

## <a name="start-monitoring-in-the-azure-portal"></a>Azure portalında izlemeyi başlatma

1. Artık Application ınsights'ı yeniden açabilirsiniz **genel bakış** şu anda çalışan uygulamanızın hakkında ayrıntıları görüntülemek için Azure portalında sayfası.

   ![Application Insights’a Genel Bakış Menüsü](./media/java-quick-start/3overview.png)

2. Uygulama bileşenleriniz arasındaki bağımlılık ilişkilerinin görsel düzeni için **Uygulama haritası**’na tıklayın. Her bileşen yük, performans, hatalar ve uyarılar gibi KPI'leri gösterir.

   ![Uygulama Eşlemesi](./media/java-quick-start/4appmap.png)

3.  Tıklayarak **uygulama analizi** simgesi ![Uygulama Haritası simgesi](./media/java-quick-start/006.png) **analytics'te görüntüle**.  Bu işlem, Application Insights tarafından toplanan tüm verileri analiz etmeye yönelik zengin bir sorgu dili sağlayan **Application Insights Analizi**’ni açar. Bu örnekte, istek sayısını grafik olarak işleyen bir sorgu oluşturulur. Diğer verileri çözümlemek için kendi sorgularınızı yazabilirsiniz.

   ![Belirli bir süre içindeki kullanıcı isteklerinin analiz grafiği](./media/java-quick-start/5analytics.png)

4. **Genel Bakış** sayfasına geri dönüp KPI graflarını inceleyin. Bu pano, gelen istek sayısı, bu isteklerin süresi ve oluşan hatalar dahil olmak üzere uygulamanızın sistem durumu hakkında istatistikler sağlar.

   ![Sistem Durumuna Genel Bakış zaman çizelgesi grafikleri](./media/java-quick-start/6kpidashboards.png)

   **Sayfa Görünümü Yükleme Süresi** grafiğini **istemci tarafı telemetri** verileriyle doldurmak üzere etkinleştirmek için, bu betiği izlemek istediğiniz her sayfaya ekleyin:

   ```HTML
   <!-- 
   To collect user behavior analytics about your application, 
   insert the following script into each page you want to track.
   Place this code immediately before the closing </head> tag,
   and before any other scripts. Your first data will appear 
   automatically in just a few seconds.
   -->
   <script type="text/javascript">
     var appInsights=window.appInsights||function(config){
     function i(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},u=document,e=window,o="script",s="AuthenticatedUserContext",h="start",c="stop",l="Track",a=l+"Event",v=l+"Page",y=u.createElement(o),r,f;y.src=config.url||"https://az416426.vo.msecnd.net/scripts/a/ai.0.js";u.getElementsByTagName(o)[0].parentNode.appendChild(y);try{t.cookie=u.cookie}catch(p){}for(t.queue=[],t.version="1.0",r=["Event","Exception","Metric","PageView","Trace","Dependency"];r.length;)i("track"+r.pop());return i("set"+s),i("clear"+s),i(h+a),i(c+a),i(h+v),i(c+v),i("flush"),config.disableExceptionTracking||(r="onerror",i("_"+r),f=e[r],e[r]=function(config,i,u,e,o){var s=f&&f(config,i,u,e,o);return s!==!0&&t["_"+r](config,i,u,e,o),s}),t
    }({
        instrumentationKey:"<instrumentation key>"
    });

    window.appInsights=appInsights;
    appInsights.trackPageView();
   </script>
    ```

5. **Canlı Akış**’a tıklayın. Burada, Java web uygulamanızın performansıyla ilgili canlı ölçümleri bulabilirsiniz. **Canlı Ölçüm Akışı**, gelen istek sayısı, bu isteklerin süresi ve oluşan her türlü hata ile ilgili veriler içerir. Ayrıca, işlemci ve bellek gibi önemli performans ölçümlerini gerçek zamanlı olarak izleyebilirsiniz.

   ![Sunucu ölçüm grafikleri](./media/java-quick-start/7livemetrics.png)

Java izleme hakkında daha fazla bilgi için [ek App Insights Java belgelerine](./../../azure-monitor/app/java-get-started.md) bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

İşiniz bittiğinde test, kaynak grubunu silebilirsiniz ve tüm ilgili kaynakları. İçin aşağıdaki adımları izleyin.

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**’na tıklayın ve ardından **myResourceGroup**’a tıklayın.
2. Kaynak grubu sayfanızda, **Sil**’e tıklayın, metin kutusuna **myResourceGroup** yazın ve ardından **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Performans sorunlarını bulma ve tanılama](https://docs.microsoft.com/azure/application-insights/app-insights-analytics)