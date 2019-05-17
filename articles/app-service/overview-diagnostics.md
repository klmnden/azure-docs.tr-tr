---
title: Azure App Service tanılama genel bakış | Microsoft Docs
description: App Service tanılamasını uygulamanızla ile sorunları nasıl giderebileceğinizi öğrenin.
keywords: App service, azure app service, tanılama, destek, web uygulaması, sorun giderme, kendi kendine yardım
services: app-service
documentationcenter: ''
author: jen7714
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2017
ms.author: jennile
ms.custom: seodec18
ms.openlocfilehash: 3e304df51133d53adad50e672249bde6c9960712
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65539908"
---
# <a name="azure-app-service-diagnostics-overview"></a>Azure App Service tanılama genel bakış

Bir web uygulaması çalıştırırken, sitenizin kapalı olması, kullanıcılara bildiren 500 hataları kaynaklanabilecek sorunları için hazırlıklı olmak istiyorsunuz. App Service tanılamasını yapılandırma gerektirmeden uygulamanızın gidermenize yardımcı olması için akıllı ve etkileşimli bir deneyimdir. Uygulamanızla bir sorunla karşılaşırsanız çalıştırdığınızda, App Service tanılamasını daha kolay ve hızlı bir şekilde ve sorunu gidermek için doğru bilgileri size yol göstermesi sorunun ne olduğunu gösterir.

Son 24 saat içinde uygulamanızla sorunları yaşıyorsanız, bir bu deneyim en yararlı olsa da, tüm tanılama grafikleri her zaman sizin için analiz etmek kullanılabilir.

App Service tanılamasını çalışır yalnızca, uygulama Windows üzerinde ancak uygulamalar için de [Linux/kapsayıcılar](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro), [App Service ortamı](https://docs.microsoft.com/azure/app-service/environment/intro), ve [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-overview).

## <a name="open-app-service-diagnostics"></a>App Service tanılamasını Aç

App Service tanılamasını erişmek için App Service web uygulaması veya App Service Ortamı'nda gidin [Azure portalında](https://portal.azure.com). Sol gezinti bölmesinde, tıklayarak **tanılayın ve sorunlarını çözmek**.

Azure işlevleri için işlev uygulamanıza ve üst gezinti gidin, tıklayarak **Platform özellikleri**seçip **Tanıla ve problemleri çözmenize** gelen **kaynakyönetimi** bölümü.

App Service tanılama giriş sayfası, her giriş sayfası kutucuğunda anahtar sözcüklerini kullanarak uygulamanızı sorun en iyi açıklayan kategoriyi seçebilirsiniz. Ayrıca, bulabileceğiniz bu sayfa, **tanılama araçları** Windows uygulamaları için. Bkz: [Tanılama Araçları (yalnızca Windows uygulaması)](#diagnostic-tools-only-for-windows-app).

![Giriş sayfası](./media/app-service-diagnostics/app-service-diagnostics-homepage-1.png)

## <a name="interactive-interface"></a>Etkileşimli arabirimi

Uygulamanızın sorun en iyi şekilde eşleşen bir giriş sayfası kategori seçtikten sonra App Service tanılamasını etkileşimli arabirimi, Genie, tanılama ve çözme uygulamanızla size rehberlik sağlayabilir. İlgili bir sorun kategorisi tam Tanılama raporunu görüntülemek için Genie tarafından sağlanan kutucuk kısayolları kullanabilirsiniz. Kutucuk kısayolları tanılama ölçümlerinizi erişmek için doğrudan bir yol sağlar.

![Kutucuk kısayolları](./media/app-service-diagnostics/tile-shortcuts-2.png)

Bu kutucuklar tıklandıktan sonra kutucuğu açıklanan sorunu ilgili konuların listesini görebilirsiniz. Bu konular, kod parçacıkları tam rapordan önemli bilgileri sağlar. Daha fazla sorunları araştırmak için aşağıdaki konulardan birini tıklayabilirsiniz. Ayrıca, tıklayabilirsiniz **tam raporu görüntüle** tek bir sayfada tüm konu başlıklarını keşfedin.

![Konular](./media/app-service-diagnostics/application-logs-insights-3.png)

![Tam raporu görüntüle](./media/app-service-diagnostics/view-full-report-4.png)

## <a name="diagnostic-report"></a>Tanılama raporu

Bir konu tıklayarak sorunu daha fazla araştırmak seçtikten sonra genellikle graflar ve markdowns ile takıma konu hakkında daha fazla ayrıntı görüntüleyebilirsiniz. Tanılama raporu, uygulamanızı sorun sunulan için güçlü bir araca olabilir.

![Tanılama raporu](./media/app-service-diagnostics/full-diagnostic-report-5.png)

## <a name="health-checkup"></a>Sistem durumu kontrolü

Uygulamanızı nerede olduğunu bilmiyorsanız veya sorunlarınızı giderme araştırmaya nereden başlayacağınızı bilmiyorsanız, sistem durumu kontrolü başlatmak için iyi bir yerdir. Sistem durumu kontrolü sağlıklı nedir ve sorunu araştırmak için aranacağı belirten, sorunun ne olduğunu işaret eden bir hızlı, etkileşimli genel size uygulamalarınızı analiz eder. Akıllı ve etkileşimli arabirimi, sorun giderme işlemi boyunca rehberlik sağlar. Sistem durumu kontrolü için Windows uygulamaları ve web uygulaması tanılama aşağı Genie deneyimiyle tümleştirilmiş rapor Linux uygulamaları için.

### <a name="health-checkup-graphs"></a>Sistem durumu kontrolü grafikleri

Sistem durumu kontrolü içinde dört farklı grafik vardır.

- **istekler ve hatalar:** HTTP sunucu hataları yanı sıra son 24 saat içinde yapılan isteklerin sayısını gösteren bir grafik.
- **uygulama performansını:** Çeşitli yüzdebirlik grupları için son 24 saat içinde yanıt süresi gösteren bir grafik.
- **CPU kullanımı:** Son 24 saat boyunca örnek başına yüzde genel CPU kullanımını gösteren bir grafik.  
- **bellek kullanımı:** Son 24 saat boyunca örnek başına genel yüzde fiziksel bellek kullanımını gösteren bir grafik.

![Sistem durumu kontrolü](./media/app-service-diagnostics/health-checkup-6.png)

### <a name="investigate-application-code-issues-only-for-windows-app"></a>(Yalnızca Windows uygulaması için) uygulama kod sorunlarını araştırmak

Birçok uygulama sorunları için uygulama kodunuzdaki sorunları birbiriyle ilişkili olduğundan, App Service tanılamasını ile tümleşir [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) özel durumları ve seçili kapalı kalma süresi ile ilişkilendirmek için bağımlılık sorunlarını vurgulayın. Application Insights sahip ayrı olarak etkinleştirilecek.

![Application Insights](./media/app-service-diagnostics/application-insights-7.png)

Application Insights özel durumları ve bağımlılıklarını görüntülemek için seçin **web uygulamasını** veya **web uygulaması yavaş** kutucuğuna kısayolları.

### <a name="troubleshooting-steps-only-for-windows-app"></a>Sorun giderme adımlarını (yalnızca Windows uygulaması için)

Bir sorun, son 24 saat içinde belirli bir sorun kategorisi ile algılanırsa, tam tanılama raporu görüntüleyebilir ve daha fazla sorun giderme tavsiyeleri ve daha destekli bir deneyim için sonraki adımları görüntülemek için App Service tanılamasını isteyebilir.

![Application Insights ve sorun giderme ve sonraki adımlar](./media/app-service-diagnostics/troubleshooting-and-next-steps-8.png)

## <a name="diagnostic-tools-only-for-windows-app"></a>Tanılama Araçları (yalnızca Windows uygulaması)

Tanılama araçları, uygulamayı inceleyin Yardım konuları, yavaşlık, bağlantı dizelerini ve diğer kod daha gelişmiş tanılama araçları içerir. ve CPU kullanımı, istekleri ve bellek ile ilgili sorunları proaktif yardımcı olacak araçlar azaltın.

### <a name="proactive-cpu-monitoring"></a>CPU proaktif izleme

Proaktif izleme CPU uygulama veya alt işlemi uygulamanız için yüksek CPU kaynaklarının tüketildiğinde eylemde bulunmak için kolay, öngörülebilir bir yol sağlar. Beklenmeyen bir sorunla gerçek nedeni bulunana kadar yüksek bir CPU sorunu geçici olarak gidermek için kendi CPU eşik kuralları ayarlayabilirsiniz.

![CPU proaktif izleme](./media/app-service-diagnostics/proactive-cpu-monitoring-9.png)

### <a name="proactive-auto-healing"></a>Proaktif otomatik onarma

Proaktif otomatik onarma CPU proaktif izleme gibi beklenmeyen davranışlara uygulamanızın azaltma için kolay ve proaktif bir yaklaşım sunar. İstek sayısı, yavaş istek, bellek sınırını ve HTTP durum kodu tetikleyici risk azaltma eylemleri için temel alarak kendi kurallar ayarlayabilirsiniz. Bu araç, beklenmeyen bir davranışla gerçek sorunun nedeni bulunana kadar geçici olarak gidermek için kullanılabilir. Proaktif otomatik onarma hakkında daha fazla bilgi için ziyaret [app service tanılama Deneyimi İyileştirme yeni Otomatik Duyurusu](https://azure.github.io/AppService/2018/09/10/Announcing-the-New-Auto-Healing-Experience-in-App-Service-Diagnostics.html).

![Proaktif otomatik onarma](./media/app-service-diagnostics/proactive-auto-healing-10.png)

## <a name="change-analysis"></a>Değişiklik analizi

Hızlı ve bol demo'lu geliştirme ortamında, bazen uygulamanıza yapılan tüm değişiklikleri izlemek ve tek başına iyi durumda olmayan bir davranışa neden bir değişiklik Pinpoint'te zor olabilir. Değişiklik analizi uygulamanıza trouble-shooting deneyimini kolaylaştırmak için yapılan değişiklikleri daraltmak yardımcı olabilir. Değişiklik analiz gömüldüğü tanılama raporda gibi **uygulama kilitlenmeleri** eşzamanlı diğer ölçümleri kullanabilirsiniz.

![Değişiklik analiz varsayılan sayfası](./media/app-service-diagnostics/change-analysis-default-page-11.png)

![Fark görünümü](./media/app-service-diagnostics/diff-view-12.png)

Değişiklik analiz özelliği kullanmadan önce etkinleştirilmesi gerekir. Değişiklik çözümleme hakkında daha fazla bilgi için ziyaret [App Service tanılama yeni değişiklik analiz deneyimi ile tanışın](https://azure.github.io/AppService/2019/05/07/Announcing-the-new-change-analysis-experience-in-App-Service-Diagnostics-Analysis.html).