---
title: Azure Application Insights anlık görüntü hata ayıklayıcısı için .NET uygulamaları | Microsoft Docs
description: Hata ayıklama anlık görüntülerini üretim .NET uygulamalarında özel durumlar oluşturulduğunda otomatik olarak toplanır
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: brahmnes
ms.date: 03/07/2019
ms.author: mbullwin
ms.openlocfilehash: 669b4d65798a553188a2b99080b72ffc7cd9e898
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60783678"
---
# <a name="debug-snapshots-on-exceptions-in-net-apps"></a>.NET uygulamalarında özel durumlarda anlık görüntü hata ayıklama
Bir özel durum oluştuğunda, hata ayıklama anlık görüntüsünü canlı web uygulamanızı otomatik olarak toplayabilirsiniz. Anlık görüntü, özel durumun oluştuğu şu anda kaynak kodu ve değişkenleri durumunu gösterir. Snapshot Debugger (Önizleme) içinde [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) web uygulamanızdan özel telemetri izler. Böylece, üretim sorunlarını tanılamak ihtiyacınız olan bilgileri sahip anlık görüntüleri, üst özel durum atma özel durumları toplar. Dahil [Snapshot collector NuGet paketini](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) uygulamanızda ve isteğe bağlı olarak koleksiyon parametrelerinde yapılandırma [Applicationınsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md). Anlık görüntüler görüntülenerek [özel durumları](../../azure-monitor/app/asp-net-exceptions.md) Application Insights portalında.

Hata ayıklama anlık görüntülerini portalda görüntüleyerek çağrı yığınını görebilir ve her bir çağrı yığını çerçevesinde değişkenleri inceleyebilirsiniz. Kaynak koduyla birlikte daha güçlü bir hata ayıklama deneyimi elde etmek için Visual Studio 2017 Enterprise ile anlık görüntüleri açmak. Visual Studio'da ayrıca [etkileşimli anlık görüntülerini almak için anlık görüntü noktaları ayarlamak](https://aka.ms/snappoint) olmadan için bir özel durum bekleniyor.

Hata ayıklama anlık görüntüleri yedi gün boyunca saklanır. Bu bekletme ilkesi, bir uygulama başına temelinde ayarlanır. Bu değeri arttırmak gerekiyorsa, Azure portalında bir destek talebi açarak artışı isteyebilirsiniz.

## <a name="enable-application-insights-snapshot-debugger-for-your-application"></a>Uygulamanız için Application Insights Snapshot Debugger'ı etkinleştir
Anlık görüntü koleksiyonu için kullanılabilir:
* .NET Framework 4.5 veya üzeri çalışan .NET framework ve ASP.NET uygulamaları.
* Windows üzerinde çalışan .NET core 2.0 ve ASP.NET Core 2.0 uygulamaları.

Şu ortamlarda desteklenir:

* [Azure App Service](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json)
* [Azure bulut Hizmetleri](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json) işletim sistemi ailesi 4 veya sonraki sürümlerini çalıştırıyor
* [Azure Service Fabric Hizmetleri](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json) Windows Server 2012 R2 veya sonraki sürümlerde çalışan
* [Azure sanal makineler ve sanal makine ölçek kümeleri](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json) çalıştıran Windows Server 2012 R2 veya üzeri
* [Şirket içi sanal veya fiziksel makinelere](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json) çalıştıran Windows Server 2012 R2 veya üzeri

> [!NOTE]
> İstemci uygulamaları (örneğin, WPF, Windows Forms veya UWP) desteklenmez.

Snapshot Debugger etkinleştirildi, ancak anlık görüntüleri görmüyorsanız, kontrol bizim [sorun giderme kılavuzu](snapshot-debugger-troubleshoot.md?toc=/azure/azure-monitor/toc.json).

## <a name="grant-permissions"></a>İzinleri verme

Anlık görüntüleri erişim, rol tabanlı erişim denetimi (RBAC) tarafından korunur. Bir anlık görüntü incelemek için önce gerekli rol için bir abonelik sahibi tarafından eklenmelidir.

> [!NOTE]
> Otomatik olarak sahipleri ve katkıda bulunanların bu role sahip değilsiniz. Bunlar anlık görüntüleri görüntülemek istiyorsanız, kendilerini rolüne eklemelisiniz.

Abonelik sahipleri Ata `Application Insights Snapshot Debugger` rol kullanıcılara anlık görüntülerini inceler. Bu rol, bireysel kullanıcılar veya gruplar için Application Insights kaynağı hedef abonelik sahipleri tarafından veya kendi kaynak grubuna veya aboneliğe atanabilir.

1. Azure portalında Application Insights kaynağına gidin.
1. Tıklayın **erişim denetimi (IAM)**.
1. Tıklayın **+ rol ataması Ekle** düğmesi.
1. Seçin **Application Insights Snapshot Debugger** gelen **rolleri** aşağı açılan listesi.
1. Arayın ve kullanıcı eklemek için bir ad girin.
1. Tıklayın **Kaydet** rolüne kullanıcı eklemek için Ekle düğmesine.


> [!IMPORTANT]
> Anlık görüntüler, potansiyel olarak değişkeni ve parametre değerlerini kişisel ve hassas bilgileri içerebilir.

## <a name="view-snapshots-in-the-portal"></a>Portalda anlık görüntüleri Göster

Uygulamanızda bir özel durum oluştu ve anlık görüntü oluşturulduktan sonra görüntülemek için anlık görüntüleri olması gerekir. Uygulamanın hazır ve görüntülenebilir bir anlık görüntüye portaldan oluşan bir özel durumdan 5-10 dakika sürebilir. Anlık görüntüleri görüntülemek için **hatası** bölmesinde **Operations** düğmesini görüntülerken **Operations** sekmesinde veya seçin **özel durumları**düğmesini görüntülerken **özel durumları** sekmesinde:

![Sayfa hataları](./media/snapshot-debugger/failures-page.png)

Bir işlem veya özel durum açmak için sağ bölmede seçin **uçtan uca işlem ayrıntıları** bölmesi ve özel durum olayı seçin. Belirtilen özel durum için bir anlık görüntü varsa bir **Aç hata ayıklama anlık görüntüsü** düğmesinin sağ bölmede ayrıntılarıyla [özel durum](../../azure-monitor/app/asp-net-exceptions.md).

![Özel durum hata ayıklama anlık görüntüsü düğmesinde Aç](./media/snapshot-debugger/e2e-transaction-page.png)

Anlık görüntü hata ayıklama Görünümü'nde, çağrı yığını ve değişkenleri bölmesini görürsünüz. Çağrı yığını Bölmesi'nde çerçeve çağrı yığınının seçtiğinizde, yerel değişkenleri görüntüleyebilir ve bu işlevin parametreleri değişkenleri Bölmesi'nde çağırın.

![Portalı'nda hata ayıklama anlık görüntüsünü görüntüle](./media/snapshot-debugger/open-snapshot-portal.png)

Anlık görüntüler, hassas bilgiler içerebilir ve görüntülenebilir olmayan varsayılan olarak. Anlık görüntüleri görüntülemek için olmalıdır `Application Insights Snapshot Debugger` size atanan rol.

## <a name="view-snapshots-in-visual-studio-2017-enterprise-or-above"></a>Anlık görüntüleri Göster Visual Studio 2017 Enterprise veya üzeri
1. Tıklayın **anlık görüntüyü indir** indirmek için düğmeye bir `.diagsession` dosyasını Visual Studio 2017 Enterprise tarafından açılabilir.

2. Açmak için `.diagsession` dosya anlık görüntü hata ayıklayıcısı VS bileşeninin yüklü olması gerekir. Anlık görüntü hata ayıklayıcı bileşeni ASP.net iş yükünü VS gerekli bir bileşenidir ve VS yükleyici tek tek bileşenler listesinden seçilebilir. Bir Visual Studio 2017 sürüm 15.5 önce kullanıyorsanız uzantısını yüklemeniz gerekir [VS Market'te](https://aka.ms/snapshotdebugger).

3. Anlık görüntü dosyası açtıktan sonra Visual Studio'da mini döküm hata ayıklama sayfası görüntülenir. Tıklayın **hata ayıklama yönetilen kodu** anlık görüntü hata ayıklama başlatılamıyor. Anlık görüntü geçerli işlemin durumunu ayıklayabilirsiniz, burada özel durumun oluştuğu kod satırına açılır.

    ![Visual Studio'da hata ayıklama anlık görüntüsünü görüntüle](./media/snapshot-debugger/open-snapshot-visualstudio.png)

İndirilen anlık görüntü, web uygulama sunucunuzda bulunan herhangi bir sembol dosyalarını içerir. Bu sembol dosyaları, kaynak kod ile anlık görüntü verileri ilişkilendirmek için gereklidir. App Service uygulamaları için web uygulamalarınızı yayımladığınızda, sembol dağıtımını etkinleştirmek emin olun.

## <a name="how-snapshots-work"></a>Anlık görüntüleri nasıl çalışır?

Anlık görüntü toplayıcının olarak uygulanan bir [Application Insights Telemetri İşlemci](../../azure-monitor/app/configuration-with-applicationinsights-config.md#telemetry-processors-aspnet). Uygulamanız çalışırken, anlık görüntü toplayıcısı Telemetri işlemci, uygulamanızın telemetri ardışık düzenine eklenen.
Her zaman uygulama çağrılarınızı [TrackException](../../azure-monitor/app/asp-net-exceptions.md#exceptions), oluşturulan özel durum ve oluşturma yöntemini türünden bir sorun kimliği anlık görüntü toplayıcının hesaplar.
Uygulamanızı çağırır TrackException, her zaman uygun sorun kimliği için bir sayaç artırılır Sayaç ulaştığında `ThresholdForSnapshotting` değeri, sorun kimliği, bir koleksiyon planı'na eklenir.

Anlık görüntü toplayıcının bunlar abone tarafından oluşturulan gibi ayrıca özel durumları izleyen [AppDomain.CurrentDomain.FirstChanceException](https://docs.microsoft.com/dotnet/api/system.appdomain.firstchanceexception) olay. Bu olayı tetikler, özel durum sorun kimliği hesaplanan ve koleksiyon planı içinde sorun kimlikleri karşı karşılaştırılır.
Ardından bir eşleşme varsa, çalışan işlem anlık görüntüsünü oluşturulur. Anlık görüntü benzersiz bir tanımlayıcı atanır ve özel durum tanımlayıcı ile damgalandı. FirstChanceException işleyici döndürdükten sonra oluşan özel durum normal şekilde işlenir. Sonuç olarak, özel durum yeniden TrackException yöntemi, Application Insights anlık görüntü tanımlayıcısı ile birlikte, burada bildirilir ulaşır.

Ana işlem, çalıştırmak ve trafiğin az kesinti ile kullanıcılara hizmet devam eder. Bu arada, anlık görüntü kapatmak için anlık görüntü yükleyici işlemi devredildiği. Anlık görüntü karşıya yükleyen bir mini döküm dosyası oluşturur ve tüm ilgili sembol (.pdb) dosyalarını yanı sıra Application ınsights'ı yükler.

> [!TIP]
> - Çalışan işlemi askıya alınmış bir kopyasını işlem anlık görüntüsüdür.
> - Anlık görüntü oluşturmak yaklaşık 10-20 milisaniye cinsinden alır.
> - İçin varsayılan değer `ThresholdForSnapshotting` 1'dir. Bu ayrıca en küçük değerdir. Bu nedenle, aynı özel durum tetiklemek uygulamanızı sahip **iki kez** anlık görüntüsünü oluşturmadan önce.
> - Ayarlama `IsEnabledInDeveloperMode` Visual Studio'da hata ayıklama sırasında anlık görüntü oluşturmak istiyorsanız True.
> - Anlık görüntü oluşturma oranı sınırlıdır `SnapshotsPerTenMinutesLimit` ayarı. Varsayılan olarak, bir anlık görüntü her on dakikada bir sınırdır.
> - Günde en fazla 50 anlık görüntüleri karşıya yüklenmeyebilir.

## <a name="limitations"></a>Sınırlamalar

Varsayılan veri saklama süresi 7 gündür. Her bir Application Insights örneği için 50 anlık görüntü sayısı üst sınırı, gün başına izin verilir.

### <a name="publish-symbols"></a>Yayım simgeleri
Snapshot Debugger, değişkenleri çözülecek ve Visual Studio'da hata ayıklama bir deneyim sağlamak üzere üretim sunucusundaki sembol dosyalarını gerektiri.
Sürüm 15.2 (veya üstü) App Service'te yayımlarken, varsayılan olarak sürüm yapıları için Visual Studio 2017 için semboller yayımlar. Önceki sürümlerde, aşağıdaki satırı yayımlama profilinizi eklemeniz gereken `.pubxml` sembolleri yayın modunda yayımlanır böylece dosya:

```xml
    <ExcludeGeneratedDebugSymbol>False</ExcludeGeneratedDebugSymbol>
```

Azure işlem ve diğer türleri için Sembol dosyaları ana uygulama .dll aynı klasörde olduğundan emin olun (genellikle `wwwroot/bin`) ya da geçerli yolda kullanılabilir.

### <a name="optimized-builds"></a>En iyi duruma getirilmiş derlemeleri
Bazı durumlarda, yerel değişkenler, JIT derleyicisi tarafından uygulanan en iyi duruma getirme nedeniyle sürüm yapılarında görüntülenemiyor.
Ancak, Azure uygulama hizmetleri, anlık görüntü toplayıcının koleksiyon planı parçası olan oluşturma yöntemleri deoptimize.

> [!TIP]
> Application Insights Site uzantısının deoptimization destek almak için App Service içinde yükleyin.

## <a name="next-steps"></a>Sonraki adımlar
Uygulamanız için Application Insights Snapshot Debugger etkinleştir:

* [Azure App Service](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json)
* [Azure Cloud Services](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric Hizmetleri](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure sanal makineler ve sanal makine ölçek kümeleri](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Şirket içi sanal veya fiziksel makineler](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Snapshot Debugger:
 
* [Anlık görüntü noktaları kodunuzda ayarlayın](https://docs.microsoft.com/visualstudio/debugger/debug-live-azure-applications) için bir özel durum beklemeden anlık görüntüler alınacak.
* [Web uygulamalarınızda özel durumları tanılama](../../azure-monitor/app/asp-net-exceptions.md) daha fazla özel durum Application Insights tarafından görülebilmesi açıklanmaktadır.
* [Akıllı algılama](../../azure-monitor/app/proactive-diagnostics.md) performans anormalliklerini otomatik olarak bulur.
