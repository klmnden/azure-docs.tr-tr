---
title: "Veri bulunmama sorunlarını giderme - .NET için Application Insights"
description: "Azure Application ınsights'ta verileri görmesini değil mi? Burada deneyin."
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: e231569f-1b38-48f8-a744-6329f41d91d3
ms.service: application-insights
ms.workload: mobile
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: 951a3217d795df6360cd3cfa2d47db08c11f978e
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="troubleshooting-no-data---application-insights-for-net"></a>Veri bulunmama sorunlarını giderme - .NET için Application Insights
## <a name="some-of-my-telemetry-is-missing"></a>My telemetri bazıları eksik
*Application Insights ' yalnızca bir kesir Uygulamam tarafından oluşturulan olayların görüyorum.*

* Tutarlı olarak aynı kesir görüyorsanız, büyük olasılıkla nedeniyle Uyarlamalı olan [örnekleme](app-insights-sampling.md). Bunu doğrulamak için arama (genel bakış dikey penceresinden) açın ve bir istek veya başka bir olay örneği bakın. Özellikler bölümü altında tıklatın tam özelliği ayrıntıları için "...". Varsa sayısı > 1 isteyebilir ve sonra örnekleme olduğundan işlem. 
* Aksi takdirde, basarsa olası bir [veri hızı sınırı](app-insights-pricing.md#limits-summary) fiyatlandırma planınızın. Bu sınırlar dakika başına uygulanır.

## <a name="no-data-from-my-server"></a>My sunucusundan veri yok
*Uygulamam my web sunucusunda yüklü ve şimdi herhangi telemetrisinden göremiyorum. Geliştirme Makinem Tamam çalıştınız.*

* Güvenlik Duvarı sorunu büyük olasılıkla. [Application Insights'ın veri göndermek güvenlik duvarı özel durumlarını ayarlamak](app-insights-ip-addresses.md).
* IIS Server bazı Önkoşullar eksik olabilir: .NET genişletilebilirliği 4.5 ve ASP.NET 4.5.

*I [Durum İzleyicisi yüklü](app-insights-monitor-performance-live-website-now.md) mevcut uygulamalarını izlemek için web sunucuma. Sonuç görmek yok.*

* Bkz: [Durum İzleyicisi sorun giderme](app-insights-monitor-performance-live-website-now.md#troubleshooting-runtime-configuration-of-application-insights). 

## <a name="q01"></a>Visual Studio'da 'Application Insights Ekle' seçeneği
*Çözüm Gezgini'nde varolan bir projeye sağ tıklattığınızda, herhangi bir Application Insights seçeneği görmüyorum.*

* .NET projenin tüm türleri araçları tarafından desteklenir. Web ve WCF projeleri desteklenir. Masaüstü ya da hizmet uygulamalar gibi diğer proje türleri için yine [bir Application Insights SDK'sı projenize el ile eklemek](app-insights-windows-desktop.md).
* Olduğundan emin olun [Visual Studio 2013 güncelleştirme 3 veya sonraki sürümü](http://go.microsoft.com/fwlink/?LinkId=397827). Application Insights SDK'sı sağlayan Geliştirici analiz araçları ile önceden yüklenmiş olarak gelir.
* Seçin **Araçları**, **Uzantılar ve güncelleştirmeler** ve denetleyin **Geliştirici analiz araçları** yüklü ve etkin. Öyleyse **güncelleştirmeleri** kullanılabilir bir güncelleştirme olup olmadığını görmek için.
* Yeni Proje iletişim kutusunu açın ve ASP.NET Web uygulaması seçin. Application Insights seçeneği yok görürseniz, Araçları yüklenir. Aksi durumda, kaldırma ve Application Insights araçları yeniden yüklemeyi deneyin.

## <a name="q02"></a>Application Insights ekleme başarısız oldu
*Varolan bir projeye Application Insights eklemeye çalıştığınızda bir hata iletisine bakın.*

Olası nedenler:

* Application Insights portalındaki ile iletişimin başarısız; veya
* Azure hesabınızla ilgili bazı sorunları vardır;
* Yalnızca [abonelik veya burada denemeye yeni kaynak oluşturmak için gruba okuma erişimi](app-insights-resources-roles-access-control.md).

Düzeltme:

* Oturum açma kimlik bilgileri sağ Azure hesabı için sağlanan denetleyin. 
* Tarayıcınızda, erişimi olup olmadığını kontrol edin [Azure portal](https://portal.azure.com). Ayarları'nı açın ve herhangi bir kısıtlama olup olmadığına bakın.
* [Varolan projenize Application Insights ekleyin](app-insights-asp-net.md): Çözüm Gezgini'nde projenize sağ tıklayın ve "Application Insights ekleyin." seçin
* Hala çalışmıyorsa, izleyin [el ile yordamı](app-insights-windows-services.md) portalında bir kaynak ekleyin ve ardından SDK'yı projenize ekleyin. 

## <a name="emptykey"></a>"İzleme anahtarını boş olamaz" hata alıyorum
Application Insights veya belki de günlüğe kaydetme bağdaştırıcısı yükleme sırada bir hata oluştuğundan gibi görünüyor.

Çözüm Gezgini'nde, projenize sağ tıklayın ve seçin **Application Insights > Application Insights yapılandırma**. Azure'da oturum açmanız başvurulmasını bir iletişim kutusu elde edersiniz ve ya da bir Application Insights kaynağı oluşturun veya var olan bir yeniden kullanın.

## <a name="NuGetBuild"></a>"NuGet paketleri yapı sunucum eksik"
*Her şeyi Tamam my geliştirme makinenizde hata ayıklama, ancak derleme sunucuda bir NuGet hatası alıyorum oluşturur.*

Lütfen bakın [NuGet paketi geri yüklemesi](http://docs.nuget.org/Consume/Package-Restore) ve [otomatik paket geri yükleme](http://docs.nuget.org/Consume/package-restore/migrating-to-automatic-package-restore).

## <a name="missing-menu-command-to-open-application-insights-from-visual-studio"></a>Visual Studio Application Insights açmak için eksik menü komutu
*Çözüm Gezgini proje sağ tıklattığınızda, herhangi bir Application Insights komut görmüyorum ya da bir açık Application Insights komutu görmüyorum.*

Olası nedenler:

* Application Insights kaynağını el ile oluşturulan ya da proje Application Insights araçları tarafından desteklenmeyen bir türde ise.
* Geliştirici analiz araçları, Visual Studio'da devre dışı bırakılır. 
* Visual Studio 2013 güncelleştirme 3'den daha eski olduğunda.

Düzeltme:

* Visual Studio sürümünüze 2013 güncelleştirme 3 veya sonrası olduğundan emin olun.
* Seçin **Araçları**, **Uzantılar ve güncelleştirmeler** ve denetleyin **Geliştirici analiz araçları** yüklü ve etkin. Öyleyse **güncelleştirmeleri** kullanılabilir bir güncelleştirme olup olmadığını görmek için.
* Çözüm Gezgini'nde projenize sağ tıklayın. Komut görürseniz **Application Insights > Application Insights yapılandırma**, projenize Application Insights hizmeti kaynak bağlanmak için kullanın.

Aksi takdirde, proje türü doğrudan Application Insights araçları tarafından desteklenmiyor. Telemetrinizi görmek için oturumu [Azure portal](https://portal.azure.com), sol gezinti çubuğunda Application Insights'ı seçin ve uygulamanızı seçin.

## <a name="access-denied-on-opening-application-insights-from-visual-studio"></a>'Erişim engellendi' Visual Studio Application Insights açarken
*'Açık Application Insights' menü komutu bana Azure portalına götürür, ancak 'erişim engellendi' hatası alın.*

![](./media/app-insights-asp-net-troubleshoot-no-data/access-denied.png)

Varsayılan tarayıcınızı en son kullanılan Microsoft oturum açma için erişime sahip değil [Application Insights bu uygulamaya eklediğinizde, oluşturulan kaynak](app-insights-asp-net.md). İki olası nedenleri şunlardır: 

* Birden fazla Microsoft hesabı - belki de bir iş ve kişisel bir Microsoft hesabı var? Erişimi olandan farklı bir hesap için varsayılan tarayıcınızı en son kullanılan oturum açma edildi [Application Insights projeye ekleyin](app-insights-asp-net.md). 
  
  * Düzeltme: adınızı tıklatın tarayıcı penceresinin sağ üst ve oturumu kapatın. Daha sonra erişimi olan bir hesapla oturum açın. Ardından sol gezinti çubuğunda Application Insights'ı tıklatın ve uygulamanızı seçin.
* Başka birinin Application Insights projeye eklendi ve size unuttunuz [kaynak grubuna erişim](app-insights-resources-roles-access-control.md) onu oluşturulduğu. 
  
  * Düzeltme: Kurumsal bir hesap kullandıysanız, bunlar, ekibine ekleyebilirsiniz; veya, kaynak grubu için tek tek erişim izni verebilir.

## <a name="asset-not-found-on-opening-application-insights-from-visual-studio"></a>'Varlık bulunamadı' Visual Studio Application Insights açarken
*'Açık Application Insights' menü komutu bana Azure portalına götürür, ancak 'varlık bulunamadı' hata alın.*

Olası nedenler:

* Uygulamanız için Application Insights kaynağı silinmiş; veya
* İzleme anahtarını ayarlamak veya bunu doğrudan proje dosyasını güncelleştirmeden düzenleyerek Applicationınsights.Config'de değiştirildi. 

Burada telemetri gönderildiği Applicationınsights.config denetimlerinde izleme anahtarı. Visual Studio'da komutunu kullandığınızda hangi kaynağın açıldığında proje dosyasındaki bir satır denetler. 

Düzeltme:

* Çözüm Gezgini'nde projeye sağ tıklayın ve Application Insights, yapılandırma Application Insights'ı seçin. İletişim kutusunda, ya da mevcut bir kaynağı telemetri göndermesine veya yeni bir tane oluşturmak seçebilirsiniz. Ya da:
* Kaynağı doğrudan açın. Oturum [Azure portalı](https://portal.azure.com), sol gezinti çubuğunda Application Insights'ı tıklatın ve ardından uygulamanızı seçin.

## <a name="where-do-i-find-my-telemetry"></a>My telemetri nereden bulabilirim?
*I açan [Microsoft Azure portal](https://portal.azure.com), ve Azure Giriş Panosu'nda görmek. Bu nedenle burada Application Insights verilerimi bulabilirim?*

* Sol gezinti çubuğunda Application Insights ve ardından, uygulama adı'ı tıklatın. İçin gereken tüm projeleri var. yoksa, [eklemek veya web projenize Application Insights yapılandırmak](app-insights-asp-net.md).
  
    Var. bazı Özet grafikleri görürsünüz. Daha fazla ayrıntı için aralarında tıklatabilirsiniz.
* Uygulamanızın hatalarını ayıkladığınız sırada Visual Studio'da Application Insights düğmesine tıklayın.

## <a name="q03"></a>Hiçbir sunucu verisi (veya hiç veri yok)
*Uygulamam çalıştıran ve daha sonra Microsoft Azure'da Application Insights hizmeti açılır, ancak '... nasıl toplanacağını öğrenin' veya 'Yapılandırılmamış.' tüm grafikler Göster* Veya, *sayfa görünümü ve kullanıcı verilerini, ancak hiçbir sunucu verisi.*

* Uygulamanızı (F5) Visual Studio'da hata ayıklama modunda çalıştırın. Uygulamayı birkaç telemetri oluşturmak amacıyla kullanın. Visual Studio çıktı penceresinde günlüğe kaydedilen olayları görebilirsiniz denetleyin. 
  
    ![](./media/app-insights-asp-net-troubleshoot-no-data/output-window.png)
* Application Insights Portalı'nda açmak [tanılama arama](app-insights-diagnostic-search.md). Veri genellikle burada ilk görünür.
* Yenile düğmesini tıklatın. Dikey kendisini düzenli aralıklarla yeniler, ancak aynı zamanda bunu el ile yapabilirsiniz. Daha büyük zaman aralıkları için yenileme aralığını uzun.
* İzleme anahtarı eşleşen denetleyin. Application Insights portalında, uygulamanız için ana dikey olarak **Essentials** açılan listesinde, bakmak **izleme anahtarını**. Ardından, projenizi Visual Studio'da Applicationınsights.config açın ve Bul `<instrumentationkey>`. İki anahtar eşit olup olmadığını denetleyin. Aksi takdirde:
  
  * Portalda, Application Insights'ı tıklatın ve uygulama kaynak için doğru anahtarla bakın. veya
  * Visual Studio Çözüm Gezgini'nde projeye sağ tıklayın ve Application Insights, Yapılandır'ı seçin. Telemetri sağ kaynağa göndermek için uygulamasını sıfırlayın.
  * Eşleşen anahtarları bulamazsanız, aynı oturum açma kimlik bilgileri olarak Visual Studio'da portalına kullandığınızdan emin olun.
    
    ![](./media/app-insights-asp-net-troubleshoot-no-data/ikey-check.png)
* İçinde [Microsoft Azure giriş Pano](https://portal.azure.com), hizmet durumu harita arayın. Bazı uyarı göstergeleri varsa, bunlar Tamam olarak döndürmüş kapatın ve, Application Insights uygulama dikey penceresini yeniden açın kadar bekleyin.
* Ayrıca kontrol [bizim durum blog](http://blogs.msdn.com/b/applicationinsights-status/).
* Herhangi bir kod yazdım [sunucu tarafı SDK](app-insights-api-custom-events-metrics.md) izleme anahtarını değişebilir `TelemetryClient` örnekleri veya `TelemetryContext`? Veya, yazdım bir [filtre veya örnekleme yapılandırma](app-insights-api-filtering-sampling.md) filtrelemesi çok fazla çıkışı?
* Applicationınsights.config düzenlediyseniz yapılandırmasını dikkatle denetleyin [TelemetryInitializers ve TelemetryProcessors](app-insights-api-filtering-sampling.md). Bir hatalı adlı türü veya parametresi hiçbir veri göndermek için SDK neden olabilir.

## <a name="q04"></a>Sayfa görünümleri, tarayıcılar kullanımını veri yok
*Sunucu yanıt süresi ve sunucu istekleri grafiklerinde veri, ancak hiçbir veri sayfa görünümü yükleme süresi veya tarayıcı veya kullanım Kanatlar görüyorum.*

Web sayfalarındaki komut dosyalarından veri gelir. 

* Varolan bir web projesine Application Insights eklediyseniz [komut dosyalarını el ile eklemek zorunda](app-insights-javascript.md).
* Internet Explorer, site uyumluluk modunda görüntüleme olmadığından emin olun.
* (Bazı tarayıcılarda F12 sonra seçin ağ) tarayıcının hata ayıklama özelliği için veri gönderiliyor doğrulamak için kullanın `dc.services.visualstudio.com`.

## <a name="no-dependency-or-exception-data"></a>Hiçbir bağımlılık veya özel durum verileri
Bkz: [bağımlılık telemetrisi](app-insights-asp-net-dependencies.md) ve [özel durum telemetrisi](app-insights-asp-net-exceptions.md).

## <a name="no-performance-data"></a>Hiçbir performans verisi
Performans verilerini (CPU, g/ç hızı vb.) için kullanılabilir [Java web Hizmetleri](app-insights-java-collectd.md), [Windows Masaüstü uygulamaları](app-insights-windows-desktop.md), [IIS, uygulama ve hizmetlere Durum İzleyicisi yüklerseniz web](app-insights-monitor-performance-live-website-now.md), ve [Azure Cloud Services](app-insights-azure.md). Sunucuları ayarlar altında bulabilirsiniz.

## <a name="no-server-data-since-i-published-the-app-to-my-server"></a>I uygulama sunucuma yayımlandığı tarihten sonra hiçbir (sunucu) verisi
* Tüm Microsoft gerçekte kopyalanan denetleyin. Sunucuya Microsoft.Diagnostics.Instrumentation.Extensions.Intercept.dll birlikte Applicationınsights DLL'leri
* Güvenlik Duvarı'nda, gerekebilir [bazı TCP bağlantı noktalarını açmak](app-insights-ip-addresses.md).
* Şirket ağınıza dışında Gönder ayarlamak üzere bir proxy kullanmak varsa [defaultProxy](https://msdn.microsoft.com/library/aa903360.aspx) Web.config dosyasında
* Windows Server 2008: aşağıdaki güncelleştirmeleri yüklediğinizden emin olun: [KB2468871](https://support.microsoft.com/kb/2468871), [KB2533523](https://support.microsoft.com/kb/2533523), [KB2600217](https://support.microsoft.com/kb/2600217).

## <a name="i-used-to-see-data-but-it-has-stopped"></a>Verileri görmek için kullandım, ancak bunu durduruldu
* Denetleme [durum blog](http://blogs.msdn.com/b/applicationinsights-status/).
* Veri noktalarının aylık kota ulaşıp? Fiyatlandırma ve ayarları/kota öğrenmek için açın. Bu durumda, planınızı yükseltmek veya ek kapasite için ödeme yaparsınız. Bkz: [düzeni fiyatlandırma](https://azure.microsoft.com/pricing/details/application-insights/).

## <a name="i-dont-see-all-the-data-im-expecting"></a>Bekleniyor tüm verileri görmüyorum
Uygulamanız çok miktarda veri gönderir ve ASP.NET sürüm 2.0.0-beta3 veya daha sonra Application Insights SDK kullanıyorsanız [Uyarlamalı örnekleme](app-insights-sampling.md) özellik çalışır ve yalnızca bir yüzdesini telemetrinizi gönderme. 

Devre dışı bırakabilir, ancak bu önerilmez. Örnekleme ilgili telemetri doğru tanılama amacıyla aktarılan şekilde tasarlanmıştır. 

## <a name="wrong-geographical-data-in-user-telemetry"></a>Kullanıcı telemetri içinde yanlış coğrafi veriler
Şehir, bölge ve ülke boyutları IP adreslerinden türetilmiş ve her zaman doğru değil.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>Azure Cloud Services’da çalıştırma üzerine “yöntem bulunamadı” özel durumu
.NET 4.6 için mi oluşturdunuz? 4.6 sürümü Azure Cloud Services rollerinde otomatik olarak desteklenmez. Uygulamanızı çalıştırmadan önce [her role 4.6 sürümünü yükleyin](../cloud-services/cloud-services-dotnet-install-dotnet.md).

## <a name="still-not-working"></a>Hala çalışmıyor...
* [Uygulama Öngörüler Forumu](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)

