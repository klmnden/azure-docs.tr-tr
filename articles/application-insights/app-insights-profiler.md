---
title: "Azure uygulama Öngörüler Profil Oluşturucu ile canlı web uygulamalarında profil | Microsoft Docs"
description: "Web sunucu kodunuzdaki etkin yolunuzda ayak izini düşük Profil Oluşturucu ile tanımlayın."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/08/2018
ms.author: mbullwin
ms.openlocfilehash: 80792a82adbb93e80c94b4829b704b70d2a8ed23
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="profile-live-azure-web-apps-with-application-insights"></a>Application Insights ile profil Canlı Azure web uygulamaları

*Bu özellik Application Insights, Azure App Service için genel olarak kullanılabilir ve Azure işlem kaynakları önizlemede.*

Ne kadar süre canlı web uygulamanızın her bir yöntemin kullanırken harcanan Bul [Application Insights](app-insights-overview.md). Aracı profili oluşturma Application Insights, uygulamanız tarafından sunulduğunu Canlı istekleri ayrıntılı profillerini gösterir ve vurgular *etkin yolunuzda* en uzun süre kullanır. Farklı yanıt sürelerini istekleriyle örnekleme temelinde profili. Uygulama yükünü çeşitli teknikler kullanılarak en aza indirilir.

Profil Oluşturucu şu anda Azure uygulama hizmeti üzerinde çalışan ASP.NET ve ASP.NET Core web uygulamaları için geçerlidir. **Temel** hizmet katmanı veya üzeri profil oluşturucu kullanmak için gereklidir.

## <a id="installation"></a>Uygulama Hizmetleri Web uygulaması için profil oluşturucu etkinleştir
Zaten bir uygulama hizmetleri için yayımlanan uygulamaya sahip ancak Application Insights kullanmak için Azure portalında uygulama hizmetleri bölmesine gidin kaynak kodunda herhangi bir şey yapmadıysanız, Git **izleme | Application Insights**, bölmesinde yeni bir kaynak oluşturmak veya mevcut bir Application Insights kaynağı Web uygulamanızı izlemek üzere seçmek için yönergeleri izleyin.

![Uygulama Hizmetleri portalında App Insights'ı etkinleştir][appinsights-in-appservices]

Proje kaynak kodunuz için erişiminiz varsa [Application Insights yükleme](app-insights-asp-net.md). Zaten yüklüyse, en son sürüme sahip olduğundan emin olun. Çözüm Gezgini'nde, en son sürümünü denetlemek için projenize sağ tıklayın ve ardından **Manage NuGet paketleri** > **güncelleştirmeleri** > **Tümünü Güncelleştir paketleri**. Ardından, uygulamanızı dağıtın.

ASP.NET Core uygulamaları Profil Oluşturucu ile çalışmak için Microsoft.ApplicationInsights.AspNetCore NuGet Paketi 2.1.0-beta6 veya daha sonra yüklemesini gerektirir. 27 Haziran 2017 itibariyle, önceki sürümlerinde desteklenmez.

İçinde [Azure portalı](https://portal.azure.com), web uygulamanız için Application Insights kaynağı açın. Seçin **performans** > **etkinleştirmek uygulama Öngörüler profil oluşturucu**.

![Etkinleştirme profil oluşturucu başlığını seçin][enable-profiler-banner]

Alternatif olarak, seçebileceğiniz **profil oluşturucu** durumunu görüntüleyin ve etkinleştirmek veya profil oluşturucu devre dışı bırakmak için yapılandırma.

![Performans altında profil oluşturucu yapılandırmasını seçin][performance-blade]

Application Insights ile yapılandırılmış olan web apps listelenmiştir **profil oluşturucu** yapılandırma bölmesi. Yukarıdaki adımları izlediyseniz, Profil Oluşturucu aracı zaten yüklü olmalıdır. Seçin **etkinleştirmek profil oluşturucu** içinde **profil oluşturucu** yapılandırma bölmesi.

Gerekirse profil oluşturucu aracıyı yüklemek için yönergeleri izleyin. Application Insights ile web uygulamaları yapılandırıldıysa seçin **bağlantılı uygulama Ekle**.

![Bölmesi seçenekleri yapılandırın][linked app services]

Uygulama hizmeti planları barındırılan web uygulamalarından farklı Azure işlem kaynakları (örneğin, Azure sanal makineler, sanal makine ölçek kümeleri, Azure Service Fabric veya Azure bulut Hizmetleri) barındırılan uygulamalara doğrudan Azure tarafından yönetilen değil. Bu durumda, bağlantı sağlamak için hiçbir web uygulaması yoktur. Bir uygulamaya bağlama yerine seçin **etkinleştirmek profil oluşturucu** düğmesi.

### <a name="enable-the-profiler-for-azure-compute-resources-preview"></a>Profil Oluşturucu Azure işlem kaynakları (Önizleme) etkinleştir

Hakkında bilgi almak bir [Azure işlem kaynakları için profil oluşturucu önizleme sürümünü](https://go.microsoft.com/fwlink/?linkid=848155).

## <a name="view-profiler-data"></a>Profil Oluşturucu verilerini görüntüleme

**Uygulamanızı trafiği aldığından emin olun.** Bir deneme yapıyorsanız, istekleri kullanarak Web uygulaması oluşturabileceğiniz [uygulama Öngörüler performans testi](https://docs.microsoft.com/en-us/vsts/load-test/app-service-web-app-performance-test). Profil Oluşturucu yeni etkinleştirilirse, yaklaşık 15 profil oluşturucu izlemeleri oluşturan dakika boyunca kısa yük testi çalıştırabilirsiniz. Profil Oluşturucu zaten bir süredir etkin olsaydı, Canlı Profil Oluşturucu iki kez saatte rastgele çalıştırır unutmayın ve her zaman, iki dakikalık bir süre için çalışır. İlk örnek profil oluşturucu izlemelerini almak emin olmak bir saat için yük testi çalıştırma öneririz.

Uygulamanızı bir miktar trafik aldıktan sonra Git **performans** dikey > **eylemleri** profil oluşturucu görüntülemek üzere izler. Seçin **profil oluşturucu izlemeleri** düğmesi.

![Uygulama Öngörüler performans bölmesi Önizleme profil oluşturucu izlemelerini][performance-blade-v2-examples]

Zamanın isteğini işleyen kodu düzeyi dökümünü göstermek için bir örnek seçin.

![Uygulama Öngörüler izleme Gezgini][trace-explorer]

İzleme Gezgini aşağıdaki bilgileri gösterir:

* **Sık kullanılan yolu Göster**: açılır biggest, düğüm yaprak ya da en az bir şey kapatın. Çoğu durumda bu düğüm için bir performans düşüklüğü bitişik.
* **Etiket**: işlevi veya olay adı. Ağaç kodu ve (SQL ve HTTP olayları gibi) oluşan olaylarla bir karışımını gösterir. Üst olay genel istek süresini temsil eder.
* **Geçen**: İşlem başlangıcı ve işlem sonunda arasındaki zaman aralığı.
* **Zaman**: zaman işlevi veya olay çalışıyordu ilişkisinde diğer işlevleri.

## <a name="how-to-read-performance-data"></a>Performans verileri okumak nasıl

Microsoft Hizmet Profil Oluşturucu örnekleme yöntemleri ve araçları birleşimi, uygulamanızın performansını çözümlemek için kullanır. Ayrıntılı toplama devam ederken, hizmet profil oluşturucu her makinenin CPU yönerge işaretçisi her milisaniyelik örnekleri. Her örnek şu anda yürütülmekte iş parçacığının tam çağrı yığını yakalar. Ne o iş parçacığı, yüksek bir düzeyde hem soyutlama düşük düzeydeki yapmakta olduğu hakkında ayrıntılı bilgi sağlar. Hizmet Profil Oluşturucu aynı zamanda etkinlik bağıntısı ve causality bağlam olayları, görev paralel kitaplığı (TPL) olayları ve iş parçacığı havuzu olayları değiştirme dahil olmak üzere, izlemek için diğer olayları toplar.

Zaman çizelgesi görünümünde gösterilen çağrı yığını izleme ve örnekleme sonucudur. İş parçacığının tam çağrı yığını her örnek yakalar olduğundan, Microsoft .NET Framework ve başvuru diğer çerçeveler kodu içerir.

### <a id="jitnewobj"></a>Nesne ayırma (clr! JIT\_yeni veya clr! JIT\_Newarr1)
**CLR! JIT\_yeni** ve **clr! JIT\_Newarr1** yönetilen yığınından bellek yardımcı işlevleri .NET Framework'teki. **CLR! JIT\_yeni** bir nesne ayrılmış olduğunda çağrılır. **CLR! JIT\_Newarr1** bir nesne dizisinin atandığında çağrılır. Bu iki işlevler genellikle hızlı ve zaman görece küçük miktarda gerçekleştirin. Görürseniz **clr! JIT\_yeni** veya **clr! JIT\_Newarr1** önemli bir süre içinde zaman çizelgeniz alın, bunu kodu olarak birçok nesne ayırma ve önemli miktarda bellek tüketerek olduğunu göstergesidir.

### <a id="theprestub"></a>Yükleme kodu (clr! ThePreStub)
**CLR! ThePreStub** ilk kez yürütmek için kodu hazırlar .NET Framework yardımcı işlevdir. Bu genellikle içerir, ancak tam zamanında (JIT) derleme için sınırlı değildir. Her C# yönteminde **clr! ThePreStub** bir işlem ömrü boyunca en fazla bir kez çağrılmalıdır.

Varsa **clr! ThePreStub** yeterli planlamanın yapılması gereken bir istek için zaman bu isteği bu yöntem yürütür birincisini olduğunu gösterir. İlk yöntem yüklemek .NET Framework çalışma zamanının süredir önemlidir. Kullanıcılarınız erişmek veya yerel Görüntü Oluşturucu (ngen.exe) çalıştırmayı düşünün önce kodu bu kısmı, derlemelerini yürütür bir Isınma işlemi kullanarak düşünebilirsiniz.

### <a id="lockcontention"></a>Kilit çakışması (clr! JITutil\_MonContention veya clr! JITutil\_MonEnterWorker)
**CLR! JITutil\_MonContention** veya **clr! JITutil\_MonEnterWorker** geçerli iş parçacığı bir kilidi serbest bırakılacak beklediğini belirtir. Bu genellikle C# yürütülürken görüntülenir **kilit** çağrılırken deyimi **Monitor.Enter** yöntemi, veya bir yöntemle çağrılırken **MethodImplOptions.Synchronized**özniteliği. Kilit çakışması genellikle oluşur zaman iş parçacığı _A_ bir kilit ve iş parçacığı edinir _B_ iş parçacığı önce aynı kilidi dener _A_ bırakması.

### <a id="ngencold"></a>Yükleme kodu ([SOĞUK])
Yöntem adı içeriyorsa, **[SOĞUK]**, gibi **mscorlib.ni! [ COLD]System.Reflection.CustomAttribute.IsDefined**, .NET Framework çalışma zamanı tarafından optimize edilmemiş ilk kez kod yürütüyor <a href="https://msdn.microsoft.com/library/e7k32f4k.aspx">profil temelli iyileştirme</a>. Her yöntem için bu işlemin süresi boyunca en fazla bir kez gösterilmesi gerekir.

Kod yüklenirken önemli miktarda süre bir istek alırsa, istek yöntemi iyileştirilmemiş kısmı yürütmek için birinci olduğunu gösterir. Kullanıcılarınızın erişebilmesi kodu bölümünü yürüten bir Isınma işlemi kullanmayı düşünün.

### <a id="httpclientsend"></a>HTTP isteği gönder
Yöntemleri ister **HttpClient.Send** kodu tamamlanması bir HTTP isteği için beklediğini gösterir.

### <a id="sqlcommand"></a>Veritabanı işlemi
Yöntemleri ister **SqlCommand.Execute** kodu tamamlamak bir veritabanı işlemi bekliyor gösterir.

### <a id="await"></a>Bekleyen (bekleme\_saat)
**BEKLEME\_zaman** kodu başka bir görev için beklediğini belirtir. Bu genellikle gerçekleşir C# ile **bekleme** deyimi. Ne zaman kodu yapar C# **bekleme**, iş parçacığı unwinds ve iş parçacığı havuzuna denetim döndürür ve bekleyen engellenmiş iş parçacığı yok **bekleme** tamamlamak için. Ancak, mantıksal olarak, iş parçacığı, vermedi **bekleme** "engellenir" ve işlemin tamamlanması bekleniyor. **Bekleme\_zaman** deyimi görevin tamamlanmasını beklerken engellenen zamanı gösterir.

### <a id="block"></a>Engellenen zaman
**BLOCKED_TIME** kodu için başka bir kaynak kullanılabilir olmasını bekliyor gösterir. Örneğin, bu eşitleme nesnesi, kullanılabilir olması için bir iş parçacığı veya son isteği bekliyor olabilir.

### <a id="cpu"></a>CPU süresi
CPU yönergeleri çalıştırmakla meşgul.

### <a id="disk"></a>Disk Zamanı
Uygulama disk işlemleri gerçekleştiriyor.

### <a id="network"></a>Ağ süresi
Uygulama ağ işlemlerini gerçekleştirme.

### <a id="when"></a>Zaman sütun
**Zaman** bir düğüm için toplanan dahil örnekleri zaman içinde nasıl farklılık, bir görsel öğe bir sütundur. İstek toplam aralığı 32 zaman demet ayrılmıştır. Bu düğüm için kapsayıcı örnekleri bu 32 demet toplanır. Her demet çubuğu olarak temsil edilir. Çubuğunun yüksekliği ölçeklendirilmiş bir değeri temsil eder. İşaretlenen düğümleri için **CPU_TIME** veya **BLOCKED_TIME**, veya bir kaynağa (CPU, disk, iş parçacığı) kullanan, belirgin bir ilişki olduğu çubuk temsil eder Bu kaynaklardan biri süre için kullanma Bu demet zamanı. Bu ölçümleri, birden fazla kaynak tüketen tarafından % 100'den büyük bir değer almak mümkündür. Örneğin, ortalama iki CPU bir zaman aralığı boyunca kullanırsanız, % 200 alın.

## <a name="limitations"></a>Sınırlamalar

Varsayılan veri saklama beş gündür. Gün başına alınan en fazla 10 GB veridir.

Profil Oluşturucu bu hizmeti kullanmak için hiçbir ücret vardır. Profil Oluşturucu hizmeti kullanmak için web uygulamanızı olmalıdır en az Azure App Service'in temel katmana barındırılan.

## <a name="overhead-and-sampling-algorithm"></a>Ek yükü ve örnekleme algoritması

Profil Oluşturucu iki dakika saatte izlemelerini yakalama için etkinleştirilmiş profil oluşturucu olan uygulamayı barındıran her sanal makineye rastgele çalışır. Profil Oluşturucu çalıştırırken, %5 ile % 15 arasında CPU ek yükünü sunucusuna ekler.
Daha fazla sunucular, daha az profil oluşturucu genel uygulama performansına etkisi uygulamasını barındırmak için kullanılabilir. Herhangi bir anda yalnızca sunucuların % 5 üzerinde çalışan profil oluşturucu örnekleme algoritması sonuçlanır olmasıdır. Daha fazla sunucu profil oluşturucu çalıştırarak neden Sunucu yükünü dengelemek için web isteklerine hizmet vermek kullanılabilir.

## <a name="disable-the-profiler"></a>Profil Oluşturucu devre dışı bırak
Tek bir uygulama hizmeti örneği için profil oluşturucu altında yeniden başlatmak veya durdurmak için **Web işleri**, uygulama hizmeti kaynak gidin. Profil Oluşturucu silmek için Git **uzantıları**.

![Web işleri için profil oluşturucu devre dışı bırak][disable-profiler-webjob]

Tüm web uygulamaları üzerinde mümkün olduğunca performans sorunları erken bulmak için etkinleştirilmiş profil oluşturucu sahip olmasını öneririz.

WebDeploy değişiklikleri web uygulamanızı dağıtmak için kullanırsanız, dağıtım sırasında silinmiş App_Data klasöründe hariç emin olun. Aksi takdirde, profil oluşturucu uzantının dosyaları Azure web uygulamasına dağıtma başlatıldığında silinir.


## <a id="troubleshooting"></a>Sorun giderme

### <a name="too-many-active-profiling-sessions"></a>Çok fazla etkin profil oluşturma oturumu

Şu anda en fazla dört Azure web uygulamaları ve aynı hizmeti planında çalıştıran dağıtım yuvası üzerinde profil oluşturucu etkinleştirebilirsiniz. Profil Oluşturucu web işi çok fazla etkin profil oluşturma oturumu raporlama, bazı web uygulamaları için farklı hizmet planı taşıyın.

### <a name="how-do-i-determine-whether-application-insights-profiler-is-running"></a>Uygulama Öngörüler profil oluşturucu çalışır durumda olup olmadığını nasıl belirlerim?

Profil Oluşturucu web uygulamasında bir sürekli web işi olarak çalıştırır. Web uygulaması kaynak açabilirsiniz [Azure portalı](https://portal.azure.com). İçinde **WebJobs** bölmesinde, durumunu denetleme **ApplicationInsightsProfiler**. Çalışıyor durumda değilse, açmak **günlükleri** daha fazla bilgi almak için.

### <a name="why-cant-i-find-any-stack-examples-even-though-the-profiler-is-running"></a>Profil Oluşturucu çalışıyor olsa bile neden herhangi yığını örnekler bulamadınız mı?

Kontrol edebilirsiniz bazı noktalar şunlardır:

* Web uygulama hizmet planınız temel katmana olduğundan emin olun veya üstü.
* Daha sonra etkin veya web uygulamanızın uygulama Öngörüler SDK 2.2 Beta sahip olduğundan emin olun.
* Web uygulamanızı sahip olduğundan emin olun **appınsıghts_ınstrumentatıonkey** Application Insights SDK'sı tarafından kullanılan aynı izleme anahtarı ile yapılandırılmış ayarı.
* Web uygulamanız .NET Framework 4.6 üzerinde çalıştığından emin olun.
* Web uygulamanızı bir ASP.NET Core uygulamasıysa denetleyin [gerekli bağımlılıkları](#aspnetcore).

Profil Oluşturucu başlatıldıktan sonra hangi sırasında profil oluşturucu birkaç performans izlemeleri etkin olarak toplayan bir kısa Isınma Süresi yoktur. Bundan sonra Profil Oluşturucu performans izlemeleri iki dakika saatte toplar.

### <a name="i-was-using-azure-service-profiler-what-happened-to-it"></a>Azure Hizmet Profil Oluşturucu tarafından kullanılan. Ona ne?

Uygulama Öngörüler profil oluşturucu etkinleştirdiğinizde, Azure hizmet profil oluşturucu Aracısı'nı devre dışı bırakılır.

### <a id="double-counting"></a>Çift paralel iş parçacığı sayımı

Bazı durumlarda, toplam süre yığını Görüntüleyicisi'nde birden çok istek süresince ölçümüdür.

Bu istekle ilişkili iki veya daha fazla iş parçacığı vardır ve paralel olarak çalıştıklarını oluşabilir. Bu durumda, toplam iş parçacığı süresi geçen süreyi büyük. Bir iş parçacığı diğer bağlı tamamlanmasını bekliyor olabilir. Görüntüleyici bu algılamaya çalışır ve sizi ilgilendirmeyen bekleme atlar, ancak ne önemli bilgiler olabilir atlama yerine çok fazla gösteren yan tarafında errs.

Paralel iş parçacıkları, izlemeleri gördüğünüzde, istek için kritik yolu olmadığından emin olmak için hangi iş parçacığı bekleyen belirler. Çoğu durumda, hızlı bir şekilde bir bekleme durumuna geçtiğinde iş parçacığı üzerinde başka bir iş parçacığı basitçe bekliyor. Diğer iş parçacıklarında yoğunlaşabilirsiniz ve bekleyen iş parçacıklarının zamanında yok sayın.

### <a id="issue-loading-trace-in-viewer"></a>Profil oluşturma veri yok

Kontrol edebilirsiniz bazı noktalar şunlardır:

* Görüntülemeye çalıştığınız veri birkaç haftalık eski ise, zaman filtresi görüntülemeyi deneyin ve yeniden deneyin.
* Proxy'leri veya bir güvenlik duvarı https://gateway.azureserviceprofiler.net erişim engellemediğinizden denetleyin.
* Uygulamanızı kullanarak Application Insights izleme anahtarı etkinleştirilmiş profil oluşturma için kullanılan Application Insights kaynağı ile aynı olup olmadığını denetleyin. Anahtar genellikle Applicationınsights.Config'de adıdır, ancak aynı zamanda web.config veya app.config dosyasında bulunabilir.

### <a name="error-report-in-the-profiling-viewer"></a>Profil oluşturma Görüntüleyicisi'nde hata raporu

Bir destek bileti portalında gönderin. Hata iletisi bağıntı kimliği eklediğinizden emin olun.

### <a name="deployment-error-directory-not-empty-dhomesitewwwrootappdatajobs"></a>Dağıtım hatası: dizini boş değil ' D:\\ev\\site\\wwwroot\\App_Data\\işlerin

Web uygulamanıza bir uygulama hizmeti kaynak etkin profil oluşturucu ile dağıtarak, aşağıdakine benzer bir ileti görebilirsiniz:

Dizini boş değil ' D:\\ev\\site\\wwwroot\\App_Data\\işlerin

Web dağıtımı komut dosyaları ya da Visual Studio Team Services dağıtım ardışık çalıştırırsanız bu hata oluşur. Web dağıtımı görevi aşağıdaki ek dağıtım parametreleri eklemek için çözümüdür:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

Bu parametreler uygulama Öngörüler Profil Oluşturucu tarafından kullanılır ve yeniden dağıtın işlem engellemesini klasörünü silin. Bunlar şu anda çalışan profil oluşturucu örneği etkilemez.


## <a name="manual-installation"></a>El ile yükleme

Profil Oluşturucu yapılandırdığınızda güncelleştirmeler web uygulamanızın ayarlar hale getirilir. Ortamınızı gerektiriyorsa, güncelleştirmeleri el ile uygulayabilirsiniz. Örneğin, uygulamanız için PowerApps bir uygulama hizmeti ortamında çalışıyorsa.

1. Web uygulama Denetim Masası'nda açmak **ayarları**.
2. Ayarlama **.Net Framework sürüm** için **v4.6**.
3. Ayarlama **her zaman açık** için **üzerinde**.
4. Ekleme __appınsıghts_ınstrumentatıonkey__ uygulama ayarlama ve SDK'sı tarafından kullanılan aynı izleme anahtarı için değer ayarlayın.
5. Açık **Gelişmiş Araçlar**.
6. Seçin **Git** Kudu Web sitesini açın.
7. Kudu Web sitesinde seçin **Site uzantıları**.
8. Yükleme __Application Insights__ Azure Web Apps galerisinden.
9. Web uygulaması yeniden başlatın.

## <a id="profileondemand"></a>El ile tetikleyici Profil Oluşturucu
Biz profil oluşturucu geliştirilen profil oluşturucu uygulama hizmetleri üzerinde test edebilirsiniz böylece biz bir komut satırı arabirimi eklenmesi. Bu aynı arabirimi kullanıcılar kullanarak profil oluşturucu nasıl başlatır de özelleştirebilirsiniz. Yüksek bir düzeyde profil oluşturucu App Service'in Kudu sistemi arka planda profil yönetmek için kullanır. Uygulama Insights uzantısını yüklediğinizde, profil oluşturucu barındıran bir sürekli web işi oluşturuyoruz. Bu aynı teknoloji gereksinimlerinize uyacak şekilde özelleştirebilirsiniz yeni bir web işi oluşturmak için kullanırız.

Bu bölümde nasıl yapılır:

1. Profil Oluşturucu iki dakika bir düğmesine basın ile başlatmak için bir web işi oluşturun.
2. Çalıştırmak için profil oluşturucu zamanlayabilirsiniz bir web işi oluşturun.
3. Profil oluşturucu bağımsız değişkenleri ayarlayın.


### <a name="set-up"></a>Kurulum
İlk şimdi içeren web işin Pano hakkında bilgi edinin. WebJobs sekmesi altında tıklatın ayarlar.

![Web işleri dikey penceresi](./media/app-insights-profiler/webjobs-blade.png)

Bu panoyu görebileceğiniz gibi tüm sitenizde şu anda yüklü web işleri gösterir. Çalışan profil oluşturucu işin hangi ApplicationInsightsProfiler2 web işi görebilirsiniz. Burada el ile ve zamanlanmış profil oluşturma için bizim yeni web işleri oluşturuluyor yukarı bitiş budur.

İlk şimdi ihtiyacımız ikili dosyalarını alın.

1.  Kudu sitesine gidin. Kudu logosu "Gelişmiş araçlar" sekmesinde geliştirme araçları sekmesi altında tıklatın. "Üzerinde Git" seçeneğine tıklayın. Bu yeni bir siteye sürer ve otomatik olarak kaydeder.
2.  Profil Oluşturucu ikilileri indirmek için ihtiyacımız İleri. Hata ayıklama Konsolu aracılığıyla dosya Gezgini'ne gidin sayfanın en üstünde bulunan CMD ->.
3.  Site tıklatıldığında -> wwwroot App_Data -> işler -> -> sürekli. Bir klasörü "ApplicationInsightsProfiler2" görmeniz gerekir. Klasör solundaki indirme simgeyi tıklatın. Bu, "ApplicationInsightsProfiler2.zip" dosyasını indirir.
4.  Bu, gereken tüm dosyaları indirir. I geçmeden önce bu zip arşivini taşımak için temiz bir dizin oluşturulması önerilir.

### <a name="setting-up-the-web-job-archive"></a>Web işi arşivi ayarlama
Yeni bir web işi azure Web sitesine temelde eklediğinizde, içinde bir run.cmd ile zip arşivini oluşturun. Run.cmd web işi sisteme web işi çalıştırdığınızda yapmanız gerekenler söyler.

1.  Başlamak için yeni bir klasör oluşturun, örneğimizde "RunProfiler2Minutes" olarak adlandırılır.
2.  Dosyaları ayıklanan ApplicationInsightProfiler2 klasöründen bu yeni bir klasöre kopyalayın.
3.  Yeni bir run.cmd dosyası oluşturun. (Bu çalışma klasörü VS Code'da kolaylık sağlamak için başlatmadan önce açabilirsiniz.)
4.  Ekle komutu `ApplicationInsightsProfiler.exe start --engine-mode immediate --single --immediate-profiling-duration 120`ve dosyayı kaydedin.
a.  `start` Komutu başlatmak için profil oluşturucu bildirir.
b.  `--engine-mode immediate`Profil Oluşturucu hemen başla istiyoruz söyler.
c.  `--single`çalıştırmak için anlamına gelir ve ardından stop otomatik olarak d.  `--immediate-profiling-duration 120`Profil Oluşturucu 120 saniye veya 2 dakika çalıştırmak anlamına gelir.
5.  Bu dosyayı kaydedin.
6.  Bu klasörü arşive, klasörü sağ tıklatın ve sıkıştırılmış klasörü için Gönder'i seçin. Bu klasörünüzün adını kullanarak bir .zip dosyası oluşturur.

![Profil oluşturucu komut Başlat](./media/app-insights-profiler/start-profiler-command.png)

Şimdi web işler bizim sitede ayarlamak kullanırız web işi .zip sunuyoruz.

### <a name="add-a-new-web-job"></a>Yeni bir web işi ekleme
Sonraki yeni bir web işi bizim sitede ekleriz. Bu örnek nasıl el ile Tetiklenmiş web işi ekleneceğini gösterir. Bunu yapmak için sonra neredeyse tam olarak aynı zamanlanmış işlemidir.

1.  Web işleri panoya gidin.
2.  Araç çubuğundan Ekle komutuna tıklayın.
3.  Web işinizin bir ad verin. Daha anlaşılır olması için arşiv adı ile eşleşmesi için ve run.cmd farklı sürümlerine sahip için açmak için yardımcı olabilir.
4.  Form kısmını dosyayı karşıya yükleme, Dosya Aç simgesine tıklayın ve yukarıda gerçekleştirdiğiniz .zip dosyasını bulun.
5.  Türü için Triggered seçin.
6.  Tetikleyici el ile seçin.
7.  Kaydetmek için Tamam'ı tıklatın.

![Profil oluşturucu komut Başlat](./media/app-insights-profiler/create-webjob.png)

### <a name="run-the-profiler"></a>Profil Oluşturucu çalıştırın

Biz biz el ile tetikleyebilir yeni bir web işi sahip olduğunuza göre biz çalıştırmayı deneyebilirsiniz.

1. Tasarım gereği, yalnızca belirli bir zamanda bir makine üzerinde çalışan bir ApplicationInsightsProfiler.exe işlem olabilir. Bu nedenle başlatmak için bu panosundan sürekli web işi devre dışı bırakmak emin olun. Satırındaki'ı tıklatın ve "Durdur" düğmesine basın. Ardından araç çubuğundaki Yenile seçin ve durumu işi durduruldu gösterir onaylayın.
2. Eklediğiniz yeni web işi satırla ve tuşuna Çalıştır tıklatın.
3. Araç çubuğundaki günlükleri komutunda satır hala seçili tıklatmayla Bu, bir web işleri panoya başlattığınız web işi için getirir. En son çalıştırır ve sonuçları listeler.
4. Yalnızca başladıktan Çalıştır örneğinde'ı tıklatın.
5. Tüm iyi olursa biz profil başlattığınız onaylayan profil oluşturucu gelen bazı tanılama günlüklerini görmeniz gerekir.

### <a name="things-to-consider"></a>Göz önünde bulundurmanız gerekenler

Bu yöntem görece basit olsa da, dikkate alınması gereken bazı şeyler vardır.

- Bu bizim hizmeti tarafından yönetilmiyor olduğundan, biz web işinizin Aracısı ikili dosyaları güncelleştiriliyor bir yolu yoktur. En son almanın tek yolu, uzantısının güncelleştirilmesi ve önceki adımlarda yaptığımız gibi sürekli klasöründen ele geçirme nedenle biz şu anda kararlı indirme sayfası bizim ikili dosyaları yok.

- Bu kullanan son kullanıcı kullanmak yerine, geliştirici kullanmak için tasarlanmış komut satırı bağımsız değişkenleri, bu bağımsız değişkenler değişiklik gelecekte, böylece yalnızca yükseltirken, duyarlı olabilir. Bir web işi, çalıştırma ve çalıştığını test eklemek için bir sorun çoğunu olmamalıdır. Sonunda Biz bu olmadan el ile işlem işlemek için bir kullanıcı Arabirimi oluşturacaksınız.

- Web işi çalıştırıldığında işleminizi aynı ortam değişkenleri ve web sitenizi sahip sona erer uygulama ayarlarını sahip olmasını sağlar Web işleri özelliği uygulama hizmetleri için benzersizdir. Başka bir deyişle, geçiş için profil oluşturucu komut satırı aracılığıyla izleme anahtarı gerekmez. İzleme anahtarı Ortamı'ndan yalnızca seçmeniz gerekir. Ancak, profil oluşturucu geliştirme kutunuzun veya uygulama hizmetleri dışındaki bir makine üzerinde çalıştırmak istiyorsanız, bir izleme anahtarı sağlamanız gerekir. Bu bağımsız değişken geçirerek yapabilirsiniz `--ikey <instrumentation-key>`. Bu değer, uygulamanızı kullanarak izleme anahtarını eşleşmesi gerekir. Profil Oluşturucu günlük çıktısı profil oluşturucu kullanmaya hangi ikey olduğunu söyler ve bu izleme anahtarını şu hatayla etkinliğinden algıladık, profil.

- El ile Tetiklenmiş web işleri Web kancası gerçekte tetiklenebilir. Web işi panodan sağ tıklayarak ve özelliklerini görüntüleyerek bu URL'yi elde edebilirsiniz. Ya da web işi tablosundan seçtikten sonra araç çubuğundaki özellikler seçerek. Bu profil oluşturucu CI/CD hattınızı (gibi VSTS) veya Microsoft Flow (https://flow.microsoft.com/en-us/) gibi bir şey tetikleme gibi sınırsız olasılıklar açar. Sonuç olarak, bu nasıl karmaşık (olabilen ayrıca bir run.ps1), run.cmd yapmak istediğiniz üzerinde bağlıdır, ancak esneklik vardır.

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio'da Application Insights ile çalışma](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[appinsights-in-appservices]:./media/app-insights-profiler/AppInsights-AppServices.png
[performance-blade]: ./media/app-insights-profiler/performance-blade.png
[performance-blade-examples]: ./media/app-insights-profiler/performance-blade-examples.png
[performance-blade-v2-examples]:./media/app-insights-profiler/performance-blade-v2-examples.png
[trace-explorer]: ./media/app-insights-profiler/trace-explorer.png
[trace-explorer-toolbar]: ./media/app-insights-profiler/trace-explorer-toolbar.png
[trace-explorer-hint-tip]: ./media/app-insights-profiler/trace-explorer-hint-tip.png
[trace-explorer-hot-path]: ./media/app-insights-profiler/trace-explorer-hot-path.png
[enable-profiler-banner]: ./media/app-insights-profiler/enable-profiler-banner.png
[disable-profiler-webjob]: ./media/app-insights-profiler/disable-profiler-webjob.png
[linked app services]: ./media/app-insights-profiler/linked-app-services.png
