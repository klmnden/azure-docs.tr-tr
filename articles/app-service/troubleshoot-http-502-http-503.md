---
title: Düzeltme 502 hatalı ağ geçidi, 503 Hizmet kullanılamıyor hatalarıyla - Azure App Service | Microsoft Docs
description: 502 hatalı ağ geçidi ve Azure App Service'te barındırılan uygulamanızda 503 Hizmet kullanılamıyor hatalarıyla ilgili sorunları giderme.
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
ms.custom: seodec18
ms.openlocfilehash: 5edd3e51e83b5ab324d1e110a1882b20d935a9b5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60833076"
---
# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-azure-app-service"></a>"502 hatalı ağ geçidi" ve "503 Hizmet kullanılamıyor" Azure App Service'te HTTP hatalarını giderme
"502 hatalı ağ geçidi" ve "503 Hizmet kullanılamıyor" olan barındırılan uygulamanızda sık karşılaşılan [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714). Bu makalede bu hataları gidermeye yardımcı olur.

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) tıklayın **destek al**.

## <a name="symptom"></a>Belirti
Uygulamaya göz atma, bir HTTP döndürür "502 hatalı ağ geçidi" hata ya da bir HTTP "503 Hizmet kullanılamıyor" hatası.

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

[App Service](overview.md) her adımda çeşitli seçenekler sunar.

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a>1. İnceleyin ve uygulama davranışını izleme
#### <a name="track-service-health"></a>Hizmet durumu izleme
Microsoft Azure hizmet kesintisi veya performans düşüşü olduğundan her zaman publicizes. Hizmetinin durumunu takip edebilirsiniz [Azure portalı](https://portal.azure.com/). Daha fazla bilgi için [hizmet durumunu izleme](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-app"></a>Uygulamanızı izleme
Bu seçenek, uygulamanızı herhangi bir sorun olması durumunda öğrenmek sağlar. Uygulamanızın dikey penceresinde **istekler ve hatalar** Döşe. **Ölçüm** dikey olarak ekleyebileceğiniz tüm ölçümler, gösterilir.

Uygulamanız için izleme isteyebilirsiniz ölçümlerin bazıları

* Ortalama bellek çalışma kümesi
* Ortalama yanıt süresi
* CPU süresi
* Bellek çalışma kümesi
* İstekler

![502 hatalı ağ geçidi, 503 Hizmet kullanılamıyor HTTP hataları çözmeye doğru uygulamasını izleme](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

Daha fazla bilgi için bkz.

* [Azure App Service'te uygulamaları izleme](web-sites-monitor.md)
* [Uyarı bildirimleri alma](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />

### <a name="2-collect-data"></a>2. Veri toplama
#### <a name="use-the-diagnostics-tool"></a>Tanılama aracını kullanma
App Service uygulamanızı yapılandırma gerektirmeden gidermenize yardımcı olması için akıllı ve etkileşimli bir deneyim sağlar. Uygulamanızla bir sorunla karşılaşırsanız çalıştırdığınızda, Tanılama aracını daha kolay ve hızlı bir şekilde ve sorunu gidermek için doğru bilgileri size yol göstermesi sorunun ne olduğunu gösterir.

App Service tanılamasını erişmek için App Service uygulamanızı veya App Service Ortamı'nda gidin [Azure portalında](https://portal.azure.com). Sol gezinti bölmesinde, tıklayarak **tanılayın ve sorunlarını çözmek**.

#### <a name="use-the-kudu-debug-console"></a>Kudu hata ayıklama konsolunu kullanma
App Service, hata ayıklama için keşfetmek, ortamınız hakkında bilgi almak için JSON uç noktaları yanı sıra, dosyaları karşıya yükleme için kullanabileceğiniz bir hata ayıklama konsoluna ile birlikte gelir. Bu adlandırılır *Kudu konsolunu* veya *SCM Pano* uygulamanız için.

Bağlantı giderek bu panoya erişebilirsiniz **https://&lt;uygulama adınız >.scm.azurewebsites.net/** .

Kudu sağlayan şeylerden bazıları şunlardır:

* Uygulamanız için ortam ayarları
* günlük akışı
* Tanılama dökümü
* hata ayıklama konsolunu Powershell cmdlet'leri ve temel DOS komutlarını çalıştırabilirsiniz.

Başka bir kullanışlı Kudu durumunda, uygulamanız, ilk şans özel durumları atma Kudu kullanabilirsiniz ve bellek oluşturmak için Procdump SysInternals aracı dökümleri, özelliğidir. Bu bellek dökümlerini işleminin anlık görüntüler ve genellikle uygulamanızla daha karmaşık sorunları gidermenize yardımcı olabilir.

Kudu içinde kullanılabilen özellikler hakkında daha fazla bilgi için bkz. [bilmeniz gereken Azure Web siteleri çevrimiçi araçları](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />

### <a name="3-mitigate-the-issue"></a>3. Sorunu gidermek
#### <a name="scale-the-app"></a>Uygulamasını ölçeklendirme
Azure App Service'te daha yüksek performans ve aktarım hızı, uygulamanızı çalıştıran ölçeği ayarlayabilirsiniz. Uygulama ölçeklendirme iki ilgili eylemleri içerir: App Service planınızın değiştirilmesi daha yüksek bir fiyatlandırma katmanına ve daha yüksek bir fiyatlandırma katmanına geçirildikten sonra belirli ayarları yapılandırma.

Ölçeklendirme hakkında daha fazla bilgi için bkz. [bir uygulamasını Azure App Service'te ölçeklendirme](web-sites-scale.md).

Ayrıca, birden fazla örneğinde uygulamanızı çalıştırmak seçebilirsiniz. Bu yalnızca ile daha fazla işleme yeteneği sağlar ancak aynı zamanda, belirli bir miktarda hataya dayanıklılık sağlar. İşlemin bir örneği üzerinde kalırsa, diğer örneğe isteklerine hizmet sürdürecektir.

El ile veya otomatik olarak ölçeklendirme ayarlayabilirsiniz.

#### <a name="use-autoheal"></a>AutoHeal kullanın
AutoHeal (yapılandırma değişiklikleri, istekler, bellek tabanlı sınırları veya bir isteğin yürütülmesi için gereken süre gibi) seçtiğiniz ayarlara bağlı uygulamanız için çalışan işlemi geri dönüştürür. Çoğu zaman, Geri Dönüşüm işlemi bir sorundan en hızlı yoludur. Uygulamayı doğrudan Azure portalı içinden her zaman yeniden başlatabilirsiniz, ancak AutoHeal bunu otomatik olarak sizin için yapar. Tek yapmak için ihtiyacınız olan kök Web.config dosyasında, uygulamanız için bazı tetikleyiciler ekleyin. Bu ayarlar, uygulamanızın bir .NET olmasa bile aynı şekilde çalışır unutmayın.

Daha fazla bilgi için [Azure Web siteleri otomatik onarım](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).

#### <a name="restart-the-app"></a>Uygulamayı yeniden başlatın
Bu genellikle tek seferlik sorunları en basit yoludur. Üzerinde [Azure portalı](https://portal.azure.com/), uygulamanızın dikey penceresinde, durdurmak veya uygulamanızı yeniden seçeneğiniz vardır.

 ![502 hatalı ağ geçidi, 503 Hizmet kullanılamıyor HTTP hataları çözmek için uygulamayı yeniden başlatın](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

Ayrıca, uygulamanızı Azure Powershell kullanarak da yönetebilirsiniz. Daha fazla bilgi için bkz. [Azure PowerShell'i Azure Resource Manager ile kullanma](../powershell-azure-resource-manager.md).

