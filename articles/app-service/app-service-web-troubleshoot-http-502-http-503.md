---
title: Düzeltme 502 hatalı ağ geçidi, 503 Hizmet kullanılamıyor hatalarıyla | Microsoft Docs
description: 502 hatalı ağ geçidi ve Azure App Service'te barındırılan web uygulamanıza 503 Hizmet kullanılamıyor hatalarıyla ilgili sorunları giderme.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
tags: top-support-issue
keywords: 502 hatalı ağ geçidi, 503 Hizmet kullanılamıyor, 503 hatası, 502 hata
ms.assetid: 51cd331a-a3fa-438f-90ef-385e755e50d5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.openlocfilehash: 1a64e40b13b05fc7f9fdb6f5aa99c8d8cc47c471
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51251619"
---
# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-your-azure-web-apps"></a>"502 hatalı ağ geçidi" ve "503 Hizmet kullanılamıyor" Azure web uygulamalarınızda HTTP hatalarını giderme
"502 hatalı ağ geçidi" ve "503 Hizmet kullanılamıyor" olan barındırılan web uygulamanızda sık karşılaşılan [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714). Bu makalede bu hataları gidermeye yardımcı olur.

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) tıklayın **destek al**.

## <a name="symptom"></a>Belirti
Web uygulamasına göz attığınızda, bir HTTP döndürür "502 hatalı ağ geçidi" hata ya da bir HTTP "503 Hizmet kullanılamıyor" hatası.

## <a name="cause"></a>Nedeni
Bu sorun genellikle gibi uygulama düzeyinde sorunlarını tarafından kaynaklanır:

* uzun süren istekleri
* yüksek bellek/CPU kullanarak uygulama
* Uygulama bir özel durum nedeniyle kilitlenme.

## <a name="troubleshooting-steps-to-solve-502-bad-gateway-and-503-service-unavailable-errors"></a>"502 hatalı ağ geçidi" ve "503 Hizmet kullanılamıyor" hataları çözmek için sorun giderme adımları
Sorun giderme sırayla üç ayrı görevlere ayrılabilir:

1. [İnceleyin ve uygulama davranışını izleme](#observe)
2. [Veri toplama](#collect)
3. [Sorunu gidermek](#mitigate)

[App Service Web Apps](app-service-web-overview.md) her adımda çeşitli seçenekler sunar.

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a>1. İnceleyin ve uygulama davranışını izleme
#### <a name="track-service-health"></a>Hizmet durumu izleme
Microsoft Azure hizmet kesintisi veya performans düşüşü olduğundan her zaman publicizes. Hizmetinin durumunu takip edebilirsiniz [Azure portalı](https://portal.azure.com/). Daha fazla bilgi için [hizmet durumunu izleme](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>Web uygulamanızı izleme
Bu seçenek, uygulamanızı herhangi bir sorun olması durumunda öğrenmek sağlar. Web uygulamanızın dikey penceresinde **istekler ve hatalar** Döşe. **Ölçüm** dikey olarak ekleyebileceğiniz tüm ölçümler, gösterilir.

Bazı web uygulamanızı izlemek için isteyebilirsiniz ölçümleri

* Ortalama bellek çalışma kümesi
* Ortalama yanıt süresi
* CPU süresi
* Bellek çalışma kümesi
* İstekler

![502 hatalı ağ geçidi, 503 Hizmet kullanılamıyor HTTP hataları çözmeye doğru web uygulamasını izleme](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

Daha fazla bilgi için bkz.

* [Azure App Service'te Web uygulamalarını izleme](web-sites-monitor.md)
* [Uyarı bildirimleri alma](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />

### <a name="2-collect-data"></a>2. Veri toplama
#### <a name="use-the-azure-app-service-support-portal"></a>Azure uygulama hizmeti destek portalı kullanın
Web uygulamaları, web uygulamanız için HTTP günlükleri, olay günlükleri, işlem dökümleri ve diğer bakarak ilgili sorunları giderme olanağı sağlar. Destek portalımızı kullanarak tüm bu bilgilere erişebilirsiniz **http://&lt;uygulama adınız >.scm.azurewebsites.net/Support**

Azure uygulama hizmeti destek portalı üç adımı yaygın bir sorun giderme senaryoları desteklemek için ayrı, üç sekme sağlar:

1. Geçerli davranışını gözlemleyin
2. Tanılama bilgilerini toplama ve yerleşik Çözümleyicileri çalışan analiz edin
3. Azaltma

Sorun şu anda karşılaşıyorsanız tıklayın **Çözümle** > **tanılama** > **şimdi Tanıla** sizin için bir tanılama oturumu oluşturmak için toplar HTTP günlükleri, Olay Görüntüleyici günlüklerinde, bellek dökümleri, PHP Hata günlüklerini ve PHP rapor işleme.

Veriler toplandıktan sonra ayrıca veriler üzerinde analiz çalıştırın ve sahip bir HTML raporunu sağlar.

Varsayılan olarak verileri indirmek istediğiniz durumlarda bu D:\home\data\DaaS klasöründe depolanır.

Azure uygulama hizmeti destek Portalı hakkında daha fazla bilgi için bkz. [destek Site uzantısı, Azure Web siteleri için yeni güncelleştirmeler](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).

#### <a name="use-the-kudu-debug-console"></a>Kudu hata ayıklama konsolunu kullanma
Web Apps, hata ayıklama için keşfetmek, ortamınız hakkında bilgi almak için JSON uç noktaları yanı sıra, dosyaları karşıya yükleme için kullanabileceğiniz bir hata ayıklama konsoluna ile birlikte gelir. Bu adlandırılır *Kudu konsolunu* veya *SCM Pano* web uygulamanız için.

Bağlantı giderek bu panoya erişebilirsiniz **https://&lt;uygulama adınız >.scm.azurewebsites.net/**.

Kudu sağlayan şeylerden bazıları şunlardır:

* Uygulamanız için ortam ayarları
* günlük akışı
* Tanılama dökümü
* hata ayıklama konsolunu Powershell cmdlet'leri ve temel DOS komutlarını çalıştırabilirsiniz.

Başka bir kullanışlı Kudu durumunda, uygulamanız, ilk şans özel durumları atma Kudu kullanabilirsiniz ve bellek oluşturmak için Procdump SysInternals aracı dökümleri, özelliğidir. Bu bellek dökümlerini işleminin anlık görüntüleri ve genellikle web uygulamanızla daha karmaşık sorunları gidermenize yardımcı olabilir.

Kudu içinde kullanılabilen özellikler hakkında daha fazla bilgi için bkz. [bilmeniz gereken Azure Web siteleri çevrimiçi araçları](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />

### <a name="3-mitigate-the-issue"></a>3. Sorunu gidermek
#### <a name="scale-the-web-app"></a>Web uygulamasını ölçeklendirme
Azure App Service'te daha yüksek performans ve aktarım hızı, uygulamanızı çalıştıran ölçeği ayarlayabilirsiniz. Bir web uygulamasını ölçeklendirme iki ilgili eylemleri içerir: App Service planınızın değiştirilmesi daha yüksek bir fiyatlandırma katmanına ve daha yüksek bir fiyatlandırma katmanına geçirildikten sonra belirli ayarları yapılandırma.

Ölçeklendirme hakkında daha fazla bilgi için bkz. [web uygulamasını Azure App Service'te ölçeklendirme](web-sites-scale.md).

Ayrıca, birden fazla örneğinde uygulamanızı çalıştırmak seçebilirsiniz. Bu yalnızca ile daha fazla işleme yeteneği sağlar ancak aynı zamanda, belirli bir miktarda hataya dayanıklılık sağlar. İşlemin bir örneği üzerinde kalırsa, diğer örneğe isteklerine hizmet sürdürecektir.

El ile veya otomatik olarak ölçeklendirme ayarlayabilirsiniz.

#### <a name="use-autoheal"></a>AutoHeal kullanın
AutoHeal (yapılandırma değişiklikleri, istekler, bellek tabanlı sınırları veya bir isteğin yürütülmesi için gereken süre gibi) seçtiğiniz ayarlara bağlı uygulamanız için çalışan işlemi geri dönüştürür. Çoğu zaman, Geri Dönüşüm işlemi bir sorundan en hızlı yoludur. Her zaman doğrudan Azure portalındaki web uygulamasından yeniden başlatabilirsiniz, ancak AutoHeal bunu otomatik olarak sizin için yapar. Tek yapmak için ihtiyacınız olan kök Web.config dosyasında web uygulamanız için bazı tetikleyiciler ekleyin. Bu ayarlar, uygulamanızın bir .net olmasa bile aynı şekilde çalışır unutmayın.

Daha fazla bilgi için [Azure Web siteleri otomatik onarım](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).

#### <a name="restart-the-web-app"></a>Web uygulamasını yeniden başlatın
Bu genellikle tek seferlik sorunları en basit yoludur. Üzerinde [Azure portalı](https://portal.azure.com/), web uygulamanızın dikey penceresinde, durdurmak veya uygulamanızı yeniden seçeneğiniz vardır.

 ![502 hatalı ağ geçidi, 503 Hizmet kullanılamıyor HTTP hataları çözmek için uygulamayı yeniden başlatın](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

Web uygulamanızı Azure Powershell kullanarak da yönetebilirsiniz. Daha fazla bilgi için bkz. [Azure PowerShell'i Azure Resource Manager ile kullanma](../powershell-azure-resource-manager.md).

