---
title: Bir Java web projesinde Application Insights sorunlarını giderme
description: Sorun giderme kılavuzu - Application Insights ile canlı Java uygulamalarını izleme.
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: ef602767-18f2-44d2-b7ef-42b404edd0e9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: mbullwin
ms.openlocfilehash: c55828244d73e612da7a7da2d050252cce04aa2c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67061148"
---
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java"></a>Java için Application Insights Sorun Giderme, Soru ve Yanıt
Sorular veya sorunlar [Azure Application Insights Java][java]? Burada, bazı ipuçları verilmektedir.

## <a name="build-errors"></a>Derleme hataları
**Eclipse veya Intellij Idea, Maven veya Gradle ile Application Insights SDK'sı eklerken, derleme veya sağlama toplamı doğrulama hataları alıyorum.**

* Bağımlılık `<version>` öğesi kullanarak bir desen ile joker karakterler (örn.) (Maven) `<version>[2.0,)</version>` veya (Gradle) `version:'2.0.+'`), bunun yerine gibi belirli bir sürümü belirtmeyi deneyin `2.0.1`. Bkz: [sürüm notları](https://github.com/Microsoft/ApplicationInsights-Java/releases) en son sürümü.

## <a name="no-data"></a>Veri yok
**Application Insights'ı başarıyla eklendi ve uygulamamı çalıştı, ancak hiçbir zaman verileri portalında gördüğünüze göre.**

* Bir dakika bekleyin ve Yenile'ye tıklayın. Grafikler kendilerini düzenli aralıklarla yeniler, ancak ayrıca el ile yenileyebilirsiniz. Yenileme aralığı grafiğin zaman aralığına bağlıdır.
* Applicationınsights.xml dosyası (içinde projenizin kaynaklar klasörüne) tanımlanan veya ortam değişkeni yapılandırılmış bir izleme anahtarı olup olmadığını denetleyin.
* Olduğunu doğrulayın hiçbir `<DisableTelemetry>true</DisableTelemetry>` xml dosyasındaki düğümü.
* Güvenlik Duvarı'nda 80 ve 443 giden trafiği dc.services.visualstudio.com için TCP bağlantı noktalarını açmanız gerekebilir. Bkz: [güvenlik duvarı özel durumlarını tam listesi](../../azure-monitor/app/ip-addresses.md)
* Microsoft Azure başlangıç Panosu, hizmet durumu eşlemesi bakın. Bazı uyarı göstergelerden varsa, bunlar için Tamam döndürmüş ve ardından kapatın ve, Application Insights uygulama dikey pencereyi yeniden açın kadar bekleyin.
* IDE konsol penceresine günlüğünü açmak için bir `<SDKLogger />` öğesi kök düğümü altına Applicationınsights.xml dosyasını (projenizin kaynaklar klasörüne) ve yapay ZEKA ile başında girdileri denetle: BİLGİLERİ/uyarı/hata şüpheli tüm günlükler için.
* Doğru Applicationınsights.xml dosyasını başarıyla Java SDK tarafından konsolun çıkış iletileri bir "yapılandırma dosyası başarıyla bulundu" deyimi için bakarak yüklenmemiş olduğunu doğrulayın.
* Yapılandırma dosyası bulunamazsa, burada yapılandırma dosyası için aranacak görmek için çıkış iletileri denetleyin ve Applicationınsights.XML bu arama konumlardan birinde bulunduğundan emin olun. Bir kural karşısında, Application Insights SDK'sı Jar'lar yapılandırma dosyasına yerleştirebilirsiniz. Örneğin: Tomcat'te bu WEB-INF/sınıflar klasörü anlamına gelir. Geliştirme sırasında web projenizin kaynaklar klasörüne Applicationınsights.XML yerleştirebilirsiniz.
* Lütfen da göz [GitHub sorunlar sayfasında](https://github.com/Microsoft/ApplicationInsights-Java/issues) SDK'sı ile ilgili bilinen sorunlar için.
* Sürüm çakışması sorunlardan kaçınmak için Application ınsights'ı core, web, aracı ve appenders günlüğü aynı sürümünü kullanmak için lütfen emin olun.

#### <a name="i-used-to-see-data-but-it-has-stopped"></a>Verileri görmek için kullandım, ancak bu durdurdu
* Denetleme [durumu blog](https://blogs.msdn.com/b/applicationinsights-status/).
* Veri noktalarının aylık kota ulaşmış olabilirsiniz? Ayarlar/kota ve fiyatlandırma öğrenmek için açın. Bu durumda, planınızı yükseltin veya ek kapasite için ödeme yaparsınız. Bkz: [düzeni fiyatlandırma](https://azure.microsoft.com/pricing/details/application-insights/).
* En son SDK'nızı yükselttikten? Lütfen yalnızca benzersiz SDK jar dosyaları dışındaki içinde proje dizini mevcut olduğundan emin olun. İki farklı sürümlerini SDK mevcut olmamalıdır.
* Doğru yapay ZEKA kaynak mı arıyorsunuz? Lütfen uygulamanızın telemetri burada beklediğiniz kaynağına ait iKey eşleştirin. Bunlar aynı olmalıdır.

#### <a name="i-dont-see-all-the-data-im-expecting"></a>Bekleniyor verilerini göremiyorum
* Kullanım açın ve olup tahmini maliyet sayfası ve onay [örnekleme](../../azure-monitor/app/sampling.md) işleminde. (% 100 iletim örnekleme işlemi olmadığı anlamına gelir.) Application Insights hizmetine ulaşan uygulamanızdan telemetri yalnızca bir bölümünü kabul edecek şekilde ayarlayabilirsiniz. Bu telemetrinin aylık kota içinde tutmanıza yardımcı olur.
* SDK'sı açık örnekleme var mı? Yanıt Evet ise, veri uygulanabilir tüm türleri için belirtilen oranlar örneklenen.
* Java SDK'sı daha eski bir sürümünü kullanıyorsunuz? 2\.0.1 sürümünden itibaren yerel sürücülerde veri kalıcılığı yanı sıra aralıklı ağ ve arka uç hataları işlemek için hataya dayanıklılık mekanizması ekledik.
* Kaynaklarınızın azaltılıp nedeniyle aşırı telemetri? BİLGİLERİNİ günlüğe kaydetmeyi varsa, bir günlük göreceksiniz "Uygulamayı kısıtladığı" iletisi. Geçerli packfile 32 bin telemetri öğelerin saniye başına ' dir.

### <a name="java-agent-cannot-capture-dependency-data"></a>Java aracı bağımlılık verileri yakalayamazsınız
* Java aracı izleyerek yapılandırdığınız [Java aracı yapılandırma](java-agent.md) ?
* Java aracı jar dosyasını hem de yapay ZEKA Agent.xml dosya aynı klasörde yerleştirildiğinden emin olun.
* Otomatik toplamak için çalıştığınız bağımlılık otomatik koleksiyonu için desteklendiğinden emin olun. Şu anda yalnızca MySQL, MsSQL, Oracle DB ve Azure önbelleği için Redis bağımlılık toplama destekliyoruz.
* JDK 1.7 veya 1.8 kullanıyorsunuz? Şu anda bağımlılık koleksiyonunu JDK 9'da desteklemiyoruz.

## <a name="no-usage-data"></a>Kullanım verisi yok
**İstek ve yanıt süreleri, ancak hiçbir sayfa görünümü, tarayıcı hakkındaki verileri veya kullanıcı verileri görüyorum.**

Başarıyla sunucudan telemetri göndermek için uygulamanızı ayarlayın. Sonraki adımınız için artık [web tarayıcısından telemetri göndermek için web sayfalarınıza ayarlamak][usage].

Alternatif olarak, istemci bir uygulamada ise bir [telefon veya diğer cihaz][platforms], buradan telemetri gönderebilir.

Hem istemci hem de sunucu telemetri ayarlamak için aynı izleme anahtarını kullanın. Veriler aynı Application Insights kaynağı görünür ve istemci ve sunucu olayları ilişkilendirmenize mümkün olacaktır.


## <a name="disabling-telemetry"></a>Telemetri devre dışı bırakma
**Telemetri koleksiyonunu nasıl devre dışı bırakabilirim?**

Kod:

```Java

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);
```

**veya**

(İçinde projenizin kaynaklar klasörüne) applicationınsights.xml dosyasını güncelleştirin. Aşağıdaki kök düğümü altına ekleyin:

```XML

    <DisableTelemetry>true</DisableTelemetry>
```

XML yöntemi kullanarak, değeri değiştirdiğinizde, uygulamayı yeniden başlatmanız gerekir.

## <a name="changing-the-target"></a>Değiştirme hedefi
**Hangi Azure kaynak Projem için veri gönderen nasıl değiştirebilirim?**

* [Yeni kaynağın izleme anahtarını alın.][java]
* Eclipse için Azure Araç Seti'ni kullanarak projenize Application Insights eklediyseniz, web projenize sağ tıklayın, seçin **Azure**, **Application ınsights'ı Yapılandır**ve kayıt anahtarını değiştirin.
* Lütfen ortam değişkeni bir izleme anahtarı yapılandırmışsa ortam değişkeninin değeri ile yeni ikey değerini güncelleştirin.
* Aksi halde, anahtar, projenizin kaynaklar klasörüne applicationınsights.xml dosyasını güncelleştirin.

## <a name="debug-data-from-the-sdk"></a>SDK'yı verilerden hata ayıklama

**Ne SDK yaptığını nasıl bulabilirim?**

API'de olup bitenler hakkında daha fazla bilgi almak için ekleyin `<SDKLogger/>` Applicationınsights.XML yapılandırma dosyasının kök düğümü altında.

### <a name="applicationinsightsxml"></a>ApplicationInsights.xml

Günlükçü bir dosyasına çıkarmak isteyebilirsiniz:

```XML
  <SDKLogger type="FILE">
    <Level>TRACE</Level>
    <UniquePrefix>AI</UniquePrefix>
    <BaseFolderPath>C:/agent/AISDK</BaseFolderPath>
</SDKLogger>
```

### <a name="spring-boot-starter"></a>Spring Boot Başlatıcı

Application Insights Spring Boot Başlatıcı'ı kullanarak Spring önyükleme uygulamaları ile SDK günlüğünü etkinleştirmek için aşağıdaki ekleyin `application.properties` dosya.:

```yaml
azure.application-insights.logger.type=file
azure.application-insights.logger.base-folder-path=C:/agent/AISDK
azure.application-insights.logger.level=trace
```

### <a name="java-agent"></a>Java aracı

Güncelleştirme Aracısı JVM günlüğü etkinleştirmek için [AI Agent.xml dosya](java-agent.md).

```xml
<AgentLogger type="FILE">
    <Level>TRACE</Level>
    <UniquePrefix>AI</UniquePrefix>
    <BaseFolderPath>C:/agent/AIAGENT</BaseFolderPath>
</AgentLogger>
```

## <a name="the-azure-start-screen"></a>Azure başlangıç ekranı
**Göz atan [Azure portalında](https://portal.azure.com). Harita bana bir Uygulamam hakkında anlatıyor?**

Hayır, dünyanın dört bir yanındaki Azure sunucularının durumunu gösterir.

*Azure başlangıç panosundan (giriş ekranı) nasıl veri Uygulamam hakkında bulabilirim?*

Varsayılmıştır [Application Insights için uygulamanızı ayarlayın][java], Gözat'a tıklayın, Application Insights'ı seçin ve uygulamanız için oluşturduğunuz uygulama kaynağı seçin. Almak için var. daha hızlı gelecekte uygulamanızın başlangıç panosuna sabitleyebilirsiniz.

## <a name="intranet-servers"></a>İntranet sunucuları
**İntranetimde bulunan bir sunucu izleyebilir miyim?**

Evet, sunucunuzun genel internet üzerinden Application Insights portalına telemetri gönderebilir sağlanır.

Güvenlik Duvarı'nda 80 ve 443 dc.services.visualstudio.com ve f5.services.visualstudio.com giden trafik için TCP bağlantı noktalarını açmanız gerekebilir.

## <a name="data-retention"></a>Veri saklama
**Veri, portalda ne kadar süreyle tutulduğunu? Güvenli mi?**

Bkz: [veri saklama ve gizlilik][data].

## <a name="debug-logging"></a>Hata ayıklama günlüğü
Application Insights'ı kullanan `org.apache.http`. Bu içinde Application Insights temel jar dosyaları dışındaki ad alanı altında konumlandırıldı `com.microsoft.applicationinsights.core.dependencies.http`. Bu senaryolar farklı yerlerde işlemek Application ınsights'ı etkinleştirir aynı sürümleri `org.apache.http` temel bir kodda mevcut.

>[!NOTE]
>Uygulamadaki tüm ad alanları için hata ayıklama düzeyinde günlüğe kaydetme etkinleştirirseniz, bu da dahil olmak üzere tüm yürütme modülleri tarafından kullanılacaktır `org.apache.http` olarak yeniden adlandırıldı `com.microsoft.applicationinsights.core.dependencies.http`. Application Insights günlük araması Apache kitaplığı tarafından yapıldığı için bu çağrılar için filtre uygulamak mümkün olmayacaktır. Hata ayıklama düzeyinde günlüğe kaydetme, günlük verilerini önemli miktarda üretmek ve canlı üretim örnekleri için önerilmez.


## <a name="next-steps"></a>Sonraki adımlar
**Application ınsights'ı Java sunucu Uygulamam için ayarlıyorum. Başka ne yapabilirim?**

* [Web sayfalarınıza kullanılabilirliğini izleyin][availability]
* [Web sayfası kullanımını izleme][usage]
* [Cihaz uygulamalarınızı sorunları tanılayın ve kullanımı izleyin][platforms]
* [Uygulamanızın kullanımını izlemek için kod yazma][track]
* [Tanılama günlükleri tutmak][javalogs]

## <a name="get-help"></a>Yardım alın
* [Stack Overflow](https://stackoverflow.com/questions/tagged/ms-application-insights)
* [Github'da sorun kaydedebilir](https://github.com/Microsoft/ApplicationInsights-Java/issues)

<!--Link references-->

[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[data]: ../../azure-monitor/app/data-retention-privacy.md
[java]: java-get-started.md
[javalogs]: java-trace-logs.md
[platforms]: ../../azure-monitor/app/platforms.md
[track]: ../../azure-monitor/app/api-custom-events-metrics.md
[usage]: javascript.md

