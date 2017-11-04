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
ms.date: 05/04/2017
ms.author: mbullwin
ms.openlocfilehash: e66dc2af18785c6c8e83815129c8bca5b877d25b
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="profile-live-azure-web-apps-with-application-insights"></a>Application Insights ile profil Canlı Azure web uygulamaları

*Bu özellik Application Insights, Azure App Service için genel olarak kullanılabilir ve Azure işlem kaynakları önizlemede.*

Ne kadar süre her yöntem, canlı web uygulamanızda kullanarak harcanan Bul [uygulama Öngörüler profil oluşturucu](app-insights-overview.md). Aracı profili oluşturma Application Insights, uygulamanız tarafından sunulduğunu Canlı istekleri ayrıntılı profillerini gösterir ve vurgular *etkin yolunuzda* en uzun süre kullanır. Profil Oluşturucu farklı yanıt sürelerini sahip örnekler otomatik olarak seçer ve ek yükü en aza indirmek için çeşitli teknikler kullanır.

Profil Oluşturucu şu anda Azure App Service üzerinde en az temel hizmet katmanını çalışan ASP.NET web uygulamaları için çalışır.

## <a id="installation"></a>Profil Oluşturucu etkinleştir

[Application Insights yükleme](app-insights-asp-net.md) kodunuzda. Zaten yüklüyse, en son sürüme sahip olduğundan emin olun. Çözüm Gezgini'nde, en son sürümünü denetlemek için projenize sağ tıklayın ve ardından **Manage NuGet paketleri** > **güncelleştirmeleri** > **Tümünü Güncelleştir paketleri**. Ardından, uygulamanızı yeniden dağıtın.

*ASP.NET Core kullanarak? Alma [daha fazla bilgi](#aspnetcore).*

İçinde [Azure portalı](https://portal.azure.com), web uygulamanız için Application Insights kaynağı açın. Seçin **performans** > **etkinleştirmek uygulama Öngörüler profil oluşturucu**.

![Etkinleştirme profil oluşturucu başlığını seçin][enable-profiler-banner]

Alternatif olarak, seçebileceğiniz **yapılandırma** durumunu görüntüleyin ve etkinleştirme veya profil oluşturucu devre dışı.

![Configure performans altında'ı seçin][performance-blade]

Application Insights ile yapılandırılmış olan web apps altında listelenen **yapılandırma**. Gerekirse profil oluşturucu aracıyı yüklemek için yönergeleri izleyin. Application Insights ile web uygulamaları yapılandırıldıysa seçin **bağlantılı uygulama Ekle**.

Profil Oluşturucu, bağlantılı web uygulamalarını denetim için **yapılandırma** bölmesinde, **etkinleştirmek profil oluşturucu** veya **devre dışı profil oluşturucu**.

![Bölmesi seçenekleri yapılandırın][linked app services]

Uygulama hizmeti planları barındırılan web uygulamalarından farklı Azure işlem kaynakları (örneğin, Azure sanal makineler, sanal makine ölçek kümeleri, Azure Service Fabric veya Azure bulut Hizmetleri) barındırılan uygulamalara doğrudan Azure tarafından yönetilen değil. Bu durumda, bağlantı sağlamak için hiçbir web uygulaması yoktur. Bir uygulamaya bağlama yerine seçin **etkinleştirmek profil oluşturucu** düğmesi.

## <a name="disable-the-profiler"></a>Profil Oluşturucu devre dışı bırak
Tek bir uygulama hizmeti örneği için profil oluşturucu altında yeniden başlatmak veya durdurmak için **Web işleri**, uygulama hizmeti kaynak gidin. Profil Oluşturucu silmek için Git **uzantıları**.

![Web işleri için profil oluşturucu devre dışı bırak][disable-profiler-webjob]

Tüm web uygulamaları üzerinde mümkün olduğunca performans sorunları erken bulmak için etkinleştirilmiş profil oluşturucu sahip olmasını öneririz.

WebDeploy değişiklikleri web uygulamanızı dağıtmak için kullanırsanız, dağıtım sırasında silinmiş App_Data klasöründe hariç emin olun. Aksi takdirde, profil oluşturucu uzantının dosyaları Azure web uygulamasına dağıtma başlatıldığında silinir.

### <a name="using-profiler-with-azure-vms-and-azure-compute-resources-preview"></a>Profil Oluşturucu Azure Vm'leri ve Azure işlem kaynakları (Önizleme) ile kullanma

Olduğunda, [Application Insights Azure App Service için çalışma zamanında etkinleştir](app-insights-azure-web-apps.md#run-time-instrumentation-with-application-insights), uygulama Öngörüler profil oluşturucu otomatik olarak kullanılabilir. Application Insights kaynağı için zaten etkinleştirdiyseniz, Yapılandırma Sihirbazı'nı kullanarak en son sürüme güncelleştirmeniz gerekebilir.

Hakkında bilgi almak bir [Azure işlem kaynakları için profil oluşturucu önizleme sürümünü](https://go.microsoft.com/fwlink/?linkid=848155).


## <a name="limitations"></a>Sınırlamalar

Varsayılan veri saklama beş gündür. Gün başına alınan en fazla 10 GB veridir.

Profil Oluşturucu bu hizmeti kullanmak için hiçbir ücret vardır. Profil Oluşturucu hizmeti kullanmak için web uygulamanızı olmalıdır en az App Service'in temel katmana barındırılan.

## <a name="overhead-and-sampling-algorithm"></a>Ek yükü ve örnekleme algoritması

Profil Oluşturucu iki dakika saatte izlemelerini yakalama için etkinleştirilmiş profil oluşturucu olan uygulamayı barındıran her sanal makineye rastgele çalışır. Profil Oluşturucu çalıştırırken, %5 ile % 15 arasında CPU ek yükünü sunucusuna ekler.
Daha fazla sunucular, daha az profil oluşturucu genel uygulama performansına etkisi uygulamasını barındırmak için kullanılabilir. Herhangi bir anda yalnızca sunucuların % 5 üzerinde çalışan profil oluşturucu örnekleme algoritması sonuçlanır olmasıdır. Daha fazla sunucu profil oluşturucu çalıştırarak neden Sunucu yükünü dengelemek için web isteklerine hizmet vermek kullanılabilir.


## <a name="view-profiler-data"></a>Profil Oluşturucu verilerini görüntüleme

Git **performans** bölmesinde ve ardından işlemlerin listesini kaydırın.

![Uygulama Öngörüler performans bölmesi örnekler sütun][performance-blade-examples]

İşlemleri tablo şu sütunları vardır:

* **COUNT**: zaman aralığını bu isteklerin sayısı **sayısı** bölmesi.
* **ORTANCA**: saat tipik bir isteğine yanıt vermek için uygulama alır. Tüm yanıtları yarısı bu değerden daha hızlı.
* **95**: bu değerden daha hızlı yanıt % 95. Bu değer ORTANCA önemli ölçüde farklı ise, uygulamanızı aralıklı bir sorun olabilir. (Veya önbelleğe alma gibi bir tasarım özelliği tarafından açıklanan.)
* **Profil OLUŞTURUCU İZLEMELERİ**: Profil Oluşturucu bu işlem için Yığın izlemeleri yakalandı simge gösterir.

Seçin **Görünüm** izleme Gezgini'ni açmak için. Profil Oluşturucu yakalanan olduğunu birkaç örnek Gezgini gösterir yanıt süresine göre sınıflandırılmış.

Kullanıyorsanız **Önizleme performans** bölmesinde, Git **eylemleri** profil oluşturucu izlemeleri görüntülemek için sayfanın bölümünü. Seçin **profil oluşturucu izlemeleri** düğmesi.

![Uygulama Öngörüler performans bölmesi Önizleme profil oluşturucu izlemelerini][performance-blade-v2-examples]

Zamanın isteğini işleyen kodu düzeyi dökümünü göstermek için bir örnek seçin.

![Uygulama Öngörüler izleme Gezgini][trace-explorer]

İzleme Gezgini aşağıdaki bilgileri gösterir:

* **Sık kullanılan yolu Göster**: açılır biggest, düğüm yaprak ya da en az bir şey kapatın. Çoğu durumda bu düğüm için bir performans düşüklüğü bitişik.
* **Etiket**: işlevi veya olay adı. Ağaç kodu ve (SQL ve HTTP olayları gibi) oluşan olaylarla bir karışımını gösterir. Üst olay genel istek süresini temsil eder.
* **Geçen**: İşlem başlangıcı ve işlem sonunda arasındaki zaman aralığı.
* **Zaman**: zaman işlevi veya olay çalışıyordu ilişkisinde diğer işlevleri.

## <a name="how-to-read-performance-data"></a>Performans verileri okumak nasıl

Microsoft Hizmet Profil Oluşturucu örnekleme yöntemleri ve araçları birleşimi, uygulamanızın performansını çözümlemek için kullanır. Ayrıntılı toplama devam ederken, hizmet profil oluşturucu her birinin her milisaniyelik makinenin CPU yönerge işaretçisi örnekleri. Her örnek şu anda yürütülmekte iş parçacığının tam çağrı yığını yakalar. Bu iş parçacığı, yüksek bir düzeyde hem soyutlama düşük düzeydeki yapmakta olduğu ayrıntılı ve faydalı bilgileri verir. Hizmet Profil Oluşturucu aynı zamanda etkinlik bağıntısı ve causality bağlam olayları, görev paralel kitaplığı (TPL) olayları ve iş parçacığı havuzu olayları değiştirme dahil olmak üzere, izlemek için diğer olayları toplar.

Zaman çizelgesi görünümünde gösterilen çağrı yığını izleme ve örnekleme sonucudur. İş parçacığının tam çağrı yığını her örnek yakalar olduğundan, Microsoft .NET Framework ve başvuru diğer çerçeveler kodu içerir.

### <a id="jitnewobj"></a>Nesne ayırma (clr! JIT\_yeni veya clr! JIT\_Newarr1)
**CLR! JIT\_yeni** ve **clr! JIT\_Newarr1** yönetilen yığınından bellek yardımcı işlevleri .NET Framework'teki. **CLR! JIT\_yeni** bir nesne ayrılmış olduğunda çağrılır. **CLR! JIT\_Newarr1** bir nesne dizisinin atandığında çağrılır. Bu iki işlevler tipik olarak çok hızlı ve zaman görece küçük miktarda gerçekleştirin. Görürseniz **clr! JIT\_yeni** veya **clr! JIT\_Newarr1** önemli bir süre içinde zaman çizelgeniz alın, bunu kodu olarak birçok nesne ayırma ve önemli miktarda bellek tüketerek olduğunu göstergesidir.

### <a id="theprestub"></a>Yükleme kodu (clr! ThePreStub)
**CLR! ThePreStub** ilk kez yürütmek için kodu hazırlar .NET Framework yardımcı işlevdir. Bu genellikle içerir, ancak tam zamanında (JIT) derleme için sınırlı değildir. Her C# yönteminde **clr! ThePreStub** bir işlem ömrü boyunca en fazla bir kez çağrılmalıdır.

Varsa **clr! ThePreStub** yeterli planlamanın yapılması gereken bir istek için zaman bu isteği bu yöntem yürütür birincisini olduğunu gösterir. Bu yöntem yüklemek .NET Framework çalışma zamanının süredir önemlidir. Kullanıcılarınız erişmek veya yerel Görüntü Oluşturucu (ngen.exe) çalıştırmayı düşünün önce kodu bu kısmı, derlemelerini yürütür bir Isınma işlemi kullanarak düşünebilirsiniz.

### <a id="lockcontention"></a>Kilit çakışması (clr! JITutil\_MonContention veya clr! JITutil\_MonEnterWorker)
**CLR! JITutil\_MonContention** veya **clr! JITutil\_MonEnterWorker** geçerli iş parçacığı bir kilidi serbest bırakılacak beklediğini belirtir. Bu genellikle C# yürütülürken görüntülenir **kilit** çağrılırken deyimi **Monitor.Enter** yöntemi, veya bir yöntemle çağrılırken **MethodImplOptions.Synchronized**özniteliği. Kilit çakışması genellikle bir kilit iş parçacığı A alır ve iş parçacığı B iş parçacığı A bırakması önce aynı kilidi denediğinde oluşur.

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

Profil Oluşturucu başlatıldıktan sonra hangi sırasında profil oluşturucu birkaç performans izlemeleri etkin olarak toplayan bir kısa Isınma Süresi yoktur. Bundan sonra Profil Oluşturucu performans izlemeleri iki dakika içinde her saat için toplar.  

### <a name="i-was-using-azure-service-profiler-what-happened-to-it"></a>Azure Hizmet Profil Oluşturucu tarafından kullanılan. Ona ne?  

Uygulama Öngörüler profil oluşturucu etkinleştirdiğinizde, Azure hizmet profil oluşturucu Aracısı'nı devre dışı bırakılır.

### <a id="double-counting"></a>Çift paralel iş parçacığı sayımı

Bazı durumlarda, toplam süre yığını Görüntüleyicisi'nde birden çok istek süresince ölçümüdür.

Bu istekle ilişkili iki veya daha fazla iş parçacığı vardır ve paralel olarak çalıştıklarını oluşabilir. Bu durumda, toplam iş parçacığı süresi geçen süreyi büyük. Bir iş parçacığı diğer bağlı tamamlanmasını bekliyor olabilir. Görüntüleyici bu algılamaya çalışır ve sizi ilgilendirmeyen bekleme atlar, ancak ne önemli bilgiler olabilir atlama yerine çok fazla gösteren yan tarafında errs.  

Paralel iş parçacıkları, izlemeleri gördüğünüzde, böylece istek kritik yolunu belirleyebilir hangi iş parçacığı bekleyen belirler. Çoğu durumda, hızlı bir şekilde bir bekleme durumuna geçtiğinde iş parçacığı üzerinde başka bir iş parçacığı basitçe bekliyor. Diğer iş parçacıklarında yoğunlaşabilirsiniz ve bekleyen iş parçacıklarının zamanında yok sayın.

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

Web dağıtımı komut dosyaları ya da Visual Studio Team Services dağıtım ardışık düzen çalıştırırsanız bu hata oluşur. Web dağıtımı görevi aşağıdaki ek dağıtım parametreleri eklemek için çözümüdür:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

Bu parametreler uygulama Öngörüler Profil Oluşturucu tarafından kullanılır ve yeniden dağıtın işlem engellemesini klasörünü silin. Bunlar şu anda çalışan profil oluşturucu örneği etkilemez.


## <a name="manual-installation"></a>El ile yükleme

Profil Oluşturucu yapılandırdığınızda güncelleştirmeler web uygulamanızın ayarlar hale getirilir. Ortamınızı gerektiriyorsa, güncelleştirmeleri el ile uygulayabilirsiniz. Örneğin, uygulamanız için PowerApps uygulama hizmeti ortamında çalışıyorsa.

1. Web uygulama Denetim Masası'nda açmak **ayarları**.
2. Ayarlama **.Net Framework sürüm** için **v4.6**.
3. Ayarlama **her zaman açık** için **üzerinde**.
4. Ekleme __appınsıghts_ınstrumentatıonkey__ uygulama ayarlama ve SDK'sı tarafından kullanılan aynı izleme anahtarı için değer ayarlayın.
5. Açık **Gelişmiş Araçlar**.
6. Seçin **Git** Kudu Web sitesini açın.
7. Kudu Web sitesinde seçin **Site uzantıları**.
8. Yükleme __Application Insights__ Azure Web Apps galerisinden.
9. Web uygulaması yeniden başlatın.

## <a id="aspnetcore"></a>ASP.NET çekirdeği desteği

Bir ASP.NET Core uygulama Microsoft.ApplicationInsights.AspNetCore NuGet Paketi 2.1.0-beta6 yüklemek veya daha sonra Profil Oluşturucu ile çalışmak için gerekir. Önceki sürümlerde 27 Haziran 2017 itibariyle desteklemiyoruz.

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio'da Application Insights ile çalışma](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

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
