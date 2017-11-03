---
title: "Bir Java web projesinde Application Insights ilgili sorunları giderme"
description: "Sorun giderme kılavuzu - Application Insights ile canlı Java uygulamalarını izleme."
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: ef602767-18f2-44d2-b7ef-42b404edd0e9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: mbullwin
ms.openlocfilehash: 5a729139e122693b4199607919c876bda45fd4b5
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java"></a>Java için Application Insights Sorun Giderme, Soru ve Yanıt
Sorular veya sorunlar [Java'da Azure Application Insights][java]? Burada, bazı ipuçları verilmektedir.

## <a name="build-errors"></a>Derleme hataları
**Eclipse'te, Maven veya Gradle aracılığıyla Application Insights SDK'sı eklerken, derleme veya sağlama toplamı doğrulama hataları alıyorum.**

* Varsa bağımlılık <version> öğesi bir desen ile joker karakterler kullanarak (örneğin (Maven) `<version>[1.0,)</version>` veya (Gradle) `version:'1.0.+'`), belirli bir sürümü gibi yerine belirtmeyi deneyin `1.0.2`. Bkz: [sürüm notları](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) son sürüm için.

## <a name="no-data"></a>Veri yok
**Application Insights başarıyla eklendi ve Uygulamam çalıştı, ancak hiç veri portalında gördüğünüze göre.**

* Bir dakika bekleyip Yenile'ye tıklayın. Grafikler kendilerini düzenli olarak yeniler, ancak aynı zamanda el ile yenileyebilirsiniz. Yenileme aralığını grafiğin zaman aralığını bağlıdır.
* Applicationınsights.xml dosyasında (projenizin kaynaklar klasörüne) tanımlanan bir izleme anahtarı olup olmadığını denetleyin
* Olduğunu doğrulayın hiçbir `<DisableTelemetry>true</DisableTelemetry>` xml dosyasında düğümü.
* Güvenlik Duvarı'nda 80 ve 443 dc.services.visualstudio.com giden trafik için TCP bağlantı noktalarını açmanız gerekebilir. Bkz: [güvenlik duvarı özel durumlarını tam listesi](app-insights-ip-addresses.md)
* Microsoft Azure'da Panosu başlatmak, hizmet durumu haritası bakın. Bazı uyarı göstergeleri varsa, bunlar Tamam olarak döndürmüş kapatın ve, Application Insights uygulama dikey penceresini yeniden açın kadar bekleyin.
* Ekleyerek IDE konsol penceresine günlüğünü açmak bir `<SDKLogger />` öğesi kök düğümü altına Applicationınsights.xml dosyasını (projenizin kaynaklar klasörüne) ve [Hata] başında girişlerini denetleyin.
* Doğru Applicationınsights.xml dosyasını başarıyla Java SDK tarafından konsolun çıktı iletileri bir "yapılandırma dosyası başarıyla bulundu" deyimi için bakarak yüklendi olduğundan emin olun.
* Yapılandırma dosyası bulunamazsa, burada yapılandırma dosyası için Aranmakta olan görmek için çıkış iletilerini denetleyin ve Applicationınsights.XML arama konumların birinde konumlandığından emin olun. Altın kural, uygulama Insights SDK Jar'lar yapılandırma dosyası yerleştirebilirsiniz. Örneğin: Tomcat'te bu WEB-INF/lib klasörüne anlamına gelir.

#### <a name="i-used-to-see-data-but-it-has-stopped"></a>Verileri görmek için kullandım, ancak bunu durduruldu
* Denetleme [durum blog](http://blogs.msdn.com/b/applicationinsights-status/).
* Veri noktalarının aylık kota ulaşıp? Ayarları/kota ve fiyatlandırma öğrenmek için açın. Bu durumda, planınızı yükseltmek veya ek kapasite için ödeme yaparsınız. Bkz: [düzeni fiyatlandırma](https://azure.microsoft.com/pricing/details/application-insights/).

#### <a name="i-dont-see-all-the-data-im-expecting"></a>Bekleniyor tüm verileri görmüyorum
* Kotalar açın ve olup fiyatlandırma dikey ve onay [örnekleme](app-insights-sampling.md) içinde bir işlemdir. (% 100 iletim örnekleme işleminde değil anlamına gelir.) Application Insights hizmeti, uygulamanızdan ulaşan telemetriyi yalnızca bir kısmı kabul edecek şekilde ayarlayabilirsiniz. Bu telemetrinin aylık kota içinde tutmanıza yardımcı olur. 

## <a name="no-usage-data"></a>Kullanım verisi yok
**İstek ve yanıt süreleri, ancak hiçbir sayfa görünümü, tarayıcı hakkındaki verileri veya kullanıcı verileri görüyorum.**

Başarılı bir şekilde telemetri sunucusundan göndermek için uygulamanızı ayarlayın. Sonraki adımınız için şimdi [web sayfalarınıza telemetri web tarayıcısından göndermesi için ayarlanmasının][usage].

Alternatif olarak, istemci bir uygulamada ise bir [telefon veya başka bir aygıt][platforms], buradan telemetri gönderebilir. 

Hem istemci hem de sunucu telemetri ayarlamak için aynı izleme anahtarını kullanın. Verileri aynı Application Insights kaynağı görünür ve istemci ve sunucu olayları ilişkilendirmenize yapabileceksiniz.


## <a name="disabling-telemetry"></a>Telemetri devre dışı bırakma
**Telemetri koleksiyonunu nasıl devre dışı bırakabilirim?**

Kod:

```Java

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);
```

**Veya** 

Applicationınsights.XML (projenizin kaynaklar klasöründe) güncelleştirin. Aşağıdaki kök düğümü altına ekleyin:

```XML

    <DisableTelemetry>true</DisableTelemetry>
```

XML yöntemi kullanarak, değeri değiştirdiğinizde uygulamayı yeniden başlatmanız gerekir.

## <a name="changing-the-target"></a>Hedef değiştirme
**Proje için verileri gönderir hangi Azure kaynak nasıl değiştirebilir miyim?**

* [Yeni kaynağın izleme anahtarını alır.][java]
* Eclipse için Azure Araç Seti kullanarak projenize Application Insights eklediyseniz, web projenize sağ tıklayın, seçin **Azure**, **yapılandırma Application Insights**ve kayıt anahtarını değiştirin.
* Aksi takdirde, projenizin kaynaklar klasörüne Applicationınsights.XML anahtarında güncelleştirin.

## <a name="debug-data-from-the-sdk"></a>Veri SDK'dan gelen hata ayıklama

**Ne SDK yaptığını nasıl bulabilirim?**

API neler olduğunu hakkında daha fazla bilgi için add `<SDKLogger/>` Applicationınsights.XML yapılandırma dosyasının kök düğümü altında.

Ayrıca bir dosyasına çıkarmak için Günlükçü yönergesi de verebilirsiniz:

```XML

    <SDKLogger type="FILE">
      <enabled>True</enabled>
      <UniquePrefix>JavaSDKLog</UniquePrefix>
    </SDKLogger>
```

Dosyaları altında bulunan `%temp%\javasdklogs` veya `java.io.tmpdir` Tomcat sunucusunu durumunda.


## <a name="the-azure-start-screen"></a>Azure başlangıç ekranı
**Konumundaki arayan [Azure portalı](https://portal.azure.com). Harita bir şey Uygulamam hakkında bilgi ver mu?**

Hayır, dünyanın Azure sunucularının durumunu gösterir.

*Azure başlangıç Panosu (giriş ekranı) nasıl veri Uygulamam hakkında bulabilirim?*

Varsayılmıştır [Application Insights için uygulamanızı ayarlayın][java], Gözat'ı tıklatın, Application Insights'ı seçin ve uygulamanız için oluşturduğunuz uygulama kaynağı seçin. Alınacak var. daha hızlı gelecekte, uygulamanızı başlangıç Panosu sabitleyebilirsiniz.

## <a name="intranet-servers"></a>İntranet sunucuları
**İntranetimde bulunan bir sunucu izleyebilir mi?**

Evet, sunucunuza Application Insights portalı genel internet üzerinden telemetri gönderebilir sağlanır. 

Güvenlik Duvarı'nda 80 ve 443 dc.services.visualstudio.com ve f5.services.visualstudio.com giden trafik için TCP bağlantı noktalarını açmanız gerekebilir.

## <a name="data-retention"></a>Veri saklama
**Ne kadar süreyle verileri portalda Tutuluyor? Güvenli mi?**

Bkz: [veri saklama ve gizlilik][data].

## <a name="next-steps"></a>Sonraki adımlar
**Java sunucu Uygulamam için Application Insights'ı ayarlarım. Başka ne yapabilirim?**

* [Web sayfalarınıza kullanılabilirliğini izleyin][availability]
* [Web sayfası kullanımını izleme][usage]
* [Cihaz uygulamalarınızdaki sorunlarını tanılamak ve kullanımını izleme][platforms]
* [Uygulamanızın kullanımını izlemek için kod yazma][track]
* [Tanılama günlüklerini yakalama][javalogs]

## <a name="get-help"></a>Yardım alın
* [Stack Overflow](http://stackoverflow.com/questions/tagged/ms-application-insights)

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[data]: app-insights-data-retention-privacy.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[platforms]: app-insights-platforms.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-javascript.md

