---
title: 502 hatalı ağ geçidi düzeltin, 503 Hizmet kullanılamıyor hatalarıyla | Microsoft Docs
description: 502 hatalı ağ geçidi ve Azure App Service içinde barındırılan web uygulamanızda 503 Hizmet kullanılamıyor hatalarıyla ilgili sorunları giderme.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
tags: top-support-issue
keywords: 502 hatalı ağ geçidi 503 Hizmet kullanılamıyor hatası 503, hata 502
ms.assetid: 51cd331a-a3fa-438f-90ef-385e755e50d5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.openlocfilehash: 397a6aaf7dc27adfa0fc0e722b8a2be5cc1d75f0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23836521"
---
# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-your-azure-web-apps"></a>"502 hatalı ağ geçidi" ve "503 Hizmet kullanılamıyor", Azure web uygulamalarında HTTP hatalarını giderme
"502 hatalı ağ geçidi" ve "503 Hizmet kullanılamıyor" olan yaygın hatalar içinde barındırılan web uygulamanızda [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). Bu makalede bu hataları gidermenize yardımcı olur.

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumlar](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve tıklayın **destek alın**.

## <a name="symptom"></a>Belirti
Web uygulaması'na göz attığınızda, bir HTTP döndürür "502 hatalı ağ geçidi" hatası ya da bir HTTP "503 Hizmet kullanılamıyor" hatasını.

## <a name="cause"></a>Nedeni
Bu sorun sık sık uygulama düzeyi sorunları tarafından gibi neden olur:

* uzun süren istekleri
* yüksek bellek/CPU kullanarak uygulama
* bir özel durum nedeniyle kilitlenen uygulama.

## <a name="troubleshooting-steps-to-solve-502-bad-gateway-and-503-service-unavailable-errors"></a>"502 hatalı ağ geçidi" ve "503 Hizmet kullanılamıyor" hataları gidermek için sorun giderme adımları
Sorun giderme sıralı bir düzende üç farklı görevler ayrılabilir:

1. [İnceleyin ve uygulama davranışı izleme](#observe)
2. [Veri toplama](#collect)
3. [Bu sorunu azaltmak](#mitigate)

[App Service Web Apps](/services/app-service/web/) her adımda çeşitli seçenekler sunar.

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a>1. İnceleyin ve uygulama davranışı izleme
#### <a name="track-service-health"></a>Hizmet durumu izleme
Microsoft Azure hizmet kesintisi veya performans düşüşü olduğundan her zaman publicizes. Hizmet durumunu takip edebilirsiniz [Azure Portal](https://portal.azure.com/). Daha fazla bilgi için bkz: [hizmet durumu izleme](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>Web uygulamanızı izlemek
Bu seçenek, uygulamanız herhangi bir sorun olması durumunda öğrenmek sağlar. Web uygulamanızın dikey penceresinde tıklayın **istekleri ve hataları** döşeme. **Ölçüm** dikey penceresinde, ekleyebileceğiniz tüm ölçümlerini gösterir.

Web uygulamanızı izlemek için isteyebilirsiniz ölçümleri bazıları

* Ortalama bellek çalışma kümesi
* Ortalama yanıt süresi
* CPU süresi
* Bellek çalışma kümesi
* İstekler

![HTTP hataları hatalı ağ geçidi 502 ve 503 Hizmet kullanılamıyor çözme doğrultusunda web uygulaması izleme](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

Daha fazla bilgi için bkz.

* [Azure App Service'te Web uygulamalarını izleme](web-sites-monitor.md)
* [Uyarı bildirimleri alma](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />

### <a name="2-collect-data"></a>2. Veri toplama
#### <a name="use-the-azure-app-service-support-portal"></a>Azure uygulama hizmeti destek Portalı'nı kullanın
Web uygulamaları, web uygulamanızın HTTP günlükleri, olay günlükleri, işlem dökümleri ve daha fazla bakarak ilgili sorunları giderme olanağı sağlar. Bizim destek portalında kullanarak tüm bu bilgilere erişebilen **http://&lt;, uygulama adı >.scm.azurewebsites.net/Support**

Azure uygulama Hizmeti'ni destekleyen portal yaygın bir sorun giderme senaryo üç adımdan desteklemek için ayrı, üç sekme sağlar:

1. Şu anki davranışı inceleyin
2. Tanılama bilgilerini toplama ve yerleşik çözümleyiciler çalıştıran analiz eder.
3. Azaltmak

Sorun şu anda oluşuyorsa tıklatın **Çözümle** > **tanılama** > **şimdi tanılamak** sizin için bir tanılama oturumu oluşturmak için hangi HTTP toplar, Olay Görüntüleyicisi'ni, bellek dökümleri, PHP Hata günlüklerini ve PHP işlem raporu günlükleri.

Veriler toplandıktan sonra ayrıca veriler üzerinde analiz çalıştırmak ve bir HTML raporu ile sağlayın.

Varsayılan olarak verileri indirmek istediğiniz durumda D:\home\data\DaaS klasöründe depolanması.

Azure uygulama hizmeti desteği portal hakkında daha fazla bilgi için bkz: [Azure Web siteleri için destek Site uzantısı için yeni güncelleştirmeler](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).

#### <a name="use-the-kudu-debug-console"></a>Kudu hata ayıklama konsolunu kullanın
Web uygulamaları, hata ayıklama, keşfetme, ortamınız hakkında bilgi almak için JSON bitiş noktaları yanı sıra, dosyaları karşıya yükleme için kullanabileceğiniz bir hata ayıklama konsolu ile birlikte gelir. Bu adlı *Kudu konsol* veya *SCM Pano* web uygulamanız için.

Bağlantısını giderek bu panoya erişebilir **https://&lt;, uygulama adı >.scm.azurewebsites.net/**.

Kudu sağlar şeylerden bazıları şunlardır:

* Uygulamanız için ortam ayarları
* günlük akışı
* Tanılama dökümü
* hata ayıklama konsolunu Powershell cmdlet'leri ve temel DOS komutları çalıştırabilirsiniz.

Başka bir yararlı Kudu uygulamanızı ilk fırsat özel durum atma durumunda, Kudu kullanabilirsiniz ve bellek oluşturmak için Procdump SysInternals aracı dökümleri, özelliğidir. Bu bellek dökümlerini işleminin anlık görüntüleri ve genellikle web uygulamanızı daha karmaşık sorunları gidermenize yardımcı olabilir.

Kudu içinde kullanılabilir özellikler hakkında daha fazla bilgi için bkz: [Azure Web siteleri çevrimiçi araçlar hakkında bilmeniz](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />

### <a name="3-mitigate-the-issue"></a>3. Bu sorunu azaltmak
#### <a name="scale-the-web-app"></a>Web uygulama ölçeklendirme
Azure App Service'te performansı artırmak ve üretilen işi için uygulamanızı çalıştıran ölçeği ayarlayabilirsiniz. Bir web uygulamasını kurup ölçeklendirmeyi kapsar iki eylemlerin: uygulama hizmeti planınızın değiştirilmesi için daha yüksek bir fiyatlandırma katmanı ve daha yüksek fiyatlandırma katmanı geçtikten sonra belirli ayarlarını yapılandırma.

Ölçeklendirme ile ilgili daha fazla bilgi için bkz: [bir web uygulamasını Azure App Service'te ölçeklendirme](web-sites-scale.md).

Ayrıca, birden çok örneğinde, uygulamayı çalıştırmayı seçebilirsiniz. Bu yalnızca ile daha fazla işleme yeteneği sağlar, ancak ayrıca miktar hataya dayanıklılık sağlar. İşlem bir örneğinde devre dışı kalırsa, diğer örneğe hala isteklere hizmet vermek devam eder.

El ile veya otomatik olarak ölçeklendirme ayarlayabilirsiniz.

#### <a name="use-autoheal"></a>AutoHeal kullanın
AutoHeal (yapılandırma değişiklikleri, istekleri, bellek tabanlı sınırları veya bir isteğin yürütülmesi için gereken süre gibi) seçtiğiniz ayarlara göre uygulamanız için çalışan işlemi geri dönüştürüldüğünde. Çoğu zaman, Geri Dönüşüm işlemi bir sorundan için en hızlı yoludur. Her zaman doğrudan Azure portalındaki web uygulamasından yeniden başlatabilirsiniz olsa AutoHeal onu otomatik olarak sizin için yapar. Yapmanız gereken tek şey, web uygulamanız için kök web.config dosyasında bazı tetikleyicileri ekleyin. Bu ayarlar, uygulamanızın bir .net olsa bile aynı şekilde çalışabilen unutmayın.

Daha fazla bilgi için bkz: [Azure Web siteleri otomatik düzeltme](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).

#### <a name="restart-the-web-app"></a>Web uygulaması yeniden
Bu tek seferlik sorunları kurtarmak için en basit yolu görülür. Üzerinde [Azure Portal](https://portal.azure.com/), web uygulamanızın dikey penceresinde, uygulamanızı yeniden başlatmak veya durdurmak için seçeneğiniz vardır.

 ![HTTP hataları 502 hatalı ağ geçidi 503 Hizmet kullanılamıyor ve çözmek için uygulamayı yeniden başlatın](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

Web uygulamanızı Azure Powershell kullanarak da yönetebilirsiniz. Daha fazla bilgi için bkz. [Azure PowerShell'i Azure Resource Manager ile kullanma](../powershell-azure-resource-manager.md).

