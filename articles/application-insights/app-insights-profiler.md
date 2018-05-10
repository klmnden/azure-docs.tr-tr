---
title: Azure uygulama Öngörüler Profil Oluşturucu ile canlı web uygulamalarında profil | Microsoft Docs
description: Web sunucu kodunuzdaki etkin yolunuzda ayak izini düşük Profil Oluşturucu ile tanımlayın.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/08/2018
ms.author: mbullwin
ms.openlocfilehash: 34824401ec8d21949c5c5036a11197a09e240bd7
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="profile-live-azure-web-apps-with-application-insights"></a>Application Insights ile profil Canlı Azure web uygulamaları

*Azure Application Insights'ın bu özelliği Azure App Service Web Apps özelliği için genellikle kullanılabilir ve Azure işlem kaynakları önizlemede. Bilgi için ilgili [şirket içi profil oluşturucu kullanımını](https://docs.microsoft.com/azure/application-insights/enable-profiler-compute#enable-profiler-on-on-premises-servers).*

Bu makalede kullanırken, canlı web uygulamanızın her bir yöntemin harcanan süreyi ele [Application Insights](app-insights-overview.md). Uygulama Öngörüler Profil Oluşturucu aracı, uygulamanız tarafından sunulduğunu Canlı istekleri ayrıntılı profillerini görüntüler. Profil Oluşturucu vurgular *etkin yolunuzda* en uzun süre kullanır. Çeşitli yanıt sürelerini istekleriyle örnekleme temelinde profili. Çeşitli teknikler kullanarak, uygulama ile ilişkili olan ek yükü en aza indirebilirsiniz.

Profil Oluşturucu şu anda Web uygulamaları çalıştıran ASP.NET ve ASP.NET Core web uygulamaları için geçerlidir. Temel Hizmet katmanını veya üzeri profil oluşturucu kullanmak için gereklidir.

## <a id="installation"></a> Web Apps web uygulamanız için profil oluşturucu etkinleştir
Zaten bir web uygulaması için yayımlanan uygulamaya sahip ancak Application Insights kullanmak için kaynak kodundaki herhangi bir şey yapmadıysanız, aşağıdakileri yapın:
1. Git **uygulama hizmetleri** Azure portalında bölmesi.
2. Altında **izleme**seçin **Application Insights**ve ardından ya da yönergeleri izleyerek yeni bir kaynak oluşturmak veya mevcut bir Application Insights kaynağı web izlemek üzere seçmek için bölmesi uygulama.

   ![Uygulama Hizmetleri portalında App Insights'ı etkinleştir][appinsights-in-appservices]

3. Proje kaynak kodunuz için erişiminiz varsa [Application Insights yükleme](app-insights-asp-net.md).  
   Zaten yüklüyse, en son sürüme sahip olduğundan emin olun. Çözüm Gezgini'nde, en son sürümünü denetlemek için projenize sağ tıklayın ve ardından **Manage NuGet paketleri** > **güncelleştirmeleri** > **Tümünü Güncelleştir paketleri**. Ardından, uygulamanızı dağıtın.

ASP.NET Core uygulamaları Profil Oluşturucu ile çalışmak için Microsoft.ApplicationInsights.AspNetCore NuGet Paketi 2.1.0-beta6 veya daha sonra yüklemesini gerektirir. 27 Haziran 2017 itibariyle, önceki sürümlerinde desteklenmez.

1. İçinde [Azure portalı](https://portal.azure.com), web uygulamanız için Application Insights kaynağı açın. 
2. Seçin **performans** > **etkinleştirmek uygulama Öngörüler profil oluşturucu**.

   ![Etkinleştirme profil oluşturucu başlığını seçin][enable-profiler-banner]

3. Alternatif olarak, seçebileceğiniz **profil oluşturucu** durumunu görüntüleyin ve etkinleştirmek veya profil oluşturucu devre dışı bırakmak için yapılandırma.

   ![Profil Oluşturucu yapılandırmasını seçin][performance-blade]

   Application Insights ile yapılandırılmış olan web apps listelenmiştir **profil oluşturucu** yapılandırma bölmesi. Yukarıdaki adımları izlediyseniz, profil oluşturucu aracısı yüklü olmalıdır. 

4. İçinde **profil oluşturucu** yapılandırma bölmesinde, **etkinleştirmek profil oluşturucu**.

5. Gerekirse, profil oluşturucu Aracısı'nı yüklemek için yönergeleri izleyin. Application Insights ile web uygulamaları yapılandırıldıysa seçin **bağlantılı uygulama Ekle**.

   ![Bölmesi seçenekleri yapılandırın][linked app services]

Web Apps planları barındırılan web uygulamalarından farklı Azure işlem kaynakları (örneğin, Azure sanal makineler, sanal makine ölçek kümeleri, Azure Service Fabric veya Azure bulut Hizmetleri) barındırılan uygulamalara doğrudan Azure tarafından yönetilen değil. Bu durumda, bağlantı sağlamak için hiçbir web uygulaması yoktur. Bir uygulamaya bağlama yerine seçin **etkinleştirmek profil oluşturucu** düğmesi.

### <a name="enable-profiler-for-azure-compute-resources-preview"></a>Profil Oluşturucu Azure işlem kaynakları (Önizleme) etkinleştir

Bilgi için bkz: [Azure işlem kaynakları için profil oluşturucu önizleme sürümünü](https://go.microsoft.com/fwlink/?linkid=848155).

## <a name="view-profiler-data"></a>Profil Oluşturucu verilerini görüntüleme

Uygulamanızı trafiği aldığından emin olun. Bir deneme yapıyorsanız, web app kullanarak için istekleri oluşturabilir [uygulama Öngörüler performans testi](https://docs.microsoft.com/vsts/load-test/app-service-web-app-performance-test). Profil Oluşturucu yeni etkinleştirdiyseniz, yaklaşık 15 profil oluşturucu izlemeleri oluşturan dakika boyunca kısa yük testi çalıştırabilirsiniz. Profil Oluşturucu zaten bir süredir etkin olsaydı, Canlı Profil Oluşturucu iki kez saatte rastgele çalıştırır unutmayın ve her zaman, iki dakikalık bir süre için çalışır. İlk örnek profil oluşturucu izlemelerini almak emin olmak bir saat için yük testi çalıştırma öneririz.

Uygulamanızı bir miktar trafik aldıktan sonra Git **performans** bölmesinde, **eylemleri** profil oluşturucu izlemeleri görüntülemek ve daha sonra seçmek için **profil oluşturucu izlemeleri** düğmesi.

![Profil Oluşturucu uygulama Öngörüler performans bölmesi Önizleme izler][performance-blade-v2-examples]

Zamanın isteğini işleyen kodu düzeyi dökümünü görüntülemek için bir örnek seçin.

![Uygulama Öngörüler izleme Gezgini][trace-explorer]

İzleme explorer aşağıdaki bilgileri görüntüler:

* **Sık kullanılan yolu Göster**: açılır biggest, düğüm yaprak ya da en az bir şey kapatın. Çoğu durumda bu düğüm için bir performans düşüklüğü bitişik.
* **Etiket**: işlevi veya olay adı. Ağaç kodu ve (SQL ve HTTP olayları gibi) oluşan olaylarla bir karışımını görüntüler. Üst olay genel istek süresini temsil eder.
* **Geçen**: İşlem başlangıcı ve işlem sonunda arasındaki zaman aralığı.
* **Zaman**: zaman işlevi veya olay çalışıyordu ilişkisinde diğer işlevleri zaman.

## <a name="how-to-read-performance-data"></a>Performans verileri okumak nasıl

Microsoft Hizmet Profil Oluşturucu örnekleme yöntemleri ve araçları birleşimi, uygulamanızın performansını çözümlemek için kullanır. Ayrıntılı toplama devam ederken, hizmet profil oluşturucu her makine CPU yönerge işaretçisi her milisaniyelik örnekleri. Her örnek şu anda yürütülmekte iş parçacığının tam çağrı yığını yakalar. Ne o iş parçacığı, yüksek bir düzeyde hem soyutlama düşük düzeydeki yapmakta olduğu hakkında ayrıntılı bilgi sağlar. Hizmet Profil Oluşturucu aynı zamanda etkinlik bağıntısı ve causality bağlam olayları, görev paralel kitaplığı (TPL) olayları ve iş parçacığı havuzu olayları değiştirme dahil olmak üzere, izlemek için diğer olayları toplar.

Zaman çizelgesi görünümünde görüntülenen çağrı yığını izleme ve örnekleme sonucudur. İş parçacığının tam çağrı yığını her örnek yakalar olduğundan, Microsoft .NET Framework ve başvuru diğer çerçeveler kodu içerir.

### <a id="jitnewobj"></a>Nesne ayırma (clr! JIT\_yeni veya clr! JIT\_Newarr1)
**CLR! JIT\_yeni** ve **clr! JIT\_Newarr1** yönetilen yığınından bellek yardımcı işlevleri .NET Framework'teki. **CLR! JIT\_yeni** bir nesne ayrılmış olduğunda çağrılır. **CLR! JIT\_Newarr1** bir nesne dizisinin atandığında çağrılır. Bu iki işlevler genellikle hızlı ve zaman görece küçük miktarda gerçekleştirin. Görürseniz **clr! JIT\_yeni** veya **clr! JIT\_Newarr1** önemli bir süre içinde zaman çizelgeniz alın, kodu olarak birçok nesne ayırma ve önemli miktarda bellek tüketerek olduğunu gösterir.

### <a id="theprestub"></a>Yükleme kodu (clr! ThePreStub)
**CLR! ThePreStub** ilk kez yürütmek için kodu hazırlar .NET Framework yardımcı işlevdir. Bu genellikle içerir, ancak tam zamanında (JIT) derleme için sınırlı değildir. Her C# yönteminde **clr! ThePreStub** bir işlem ömrü boyunca en fazla bir kez çağrılmalıdır.

Varsa **clr! ThePreStub** yeterli planlamanın yapılması gereken bir istek için zaman bu isteği bu yöntem yürütür birincisini olduğunu gösterir. İlk yöntem yüklemek .NET Framework çalışma zamanının süredir önemlidir. Kullanıcılarınız erişmek veya yerel Görüntü Oluşturucu (ngen.exe) çalıştırmayı düşünün önce kodu bu kısmı, derlemelerini yürütür bir Isınma işlemi kullanarak düşünebilirsiniz.

### <a id="lockcontention"></a>Kilit çakışması (clr! JITutil\_MonContention veya clr! JITutil\_MonEnterWorker)
**CLR! JITutil\_MonContention** veya **clr! JITutil\_MonEnterWorker** geçerli iş parçacığı bir kilidi serbest bırakılacak beklediğini belirtir. Bu metin genellikle C# yürüttüğünüzde görüntülenen **kilit** çağrılırken deyimi **Monitor.Enter** yöntemi, veya bir yöntemle çağrılırken, **MethodImplOptions.Synchronized** özniteliği. Kilit çakışması genellikle oluşur zaman iş parçacığı _A_ bir kilit ve iş parçacığı edinir _B_ iş parçacığı önce aynı kilidi dener _A_ bırakması.

### <a id="ngencold"></a>Yükleme kodu ([SOĞUK])
Yöntem adı içeriyorsa, **[SOĞUK]**, gibi **mscorlib.ni! [ COLD]System.Reflection.CustomAttribute.IsDefined**, .NET Framework çalışma zamanı tarafından optimize edilmemiş ilk kez kod yürütüyor <a href="https://msdn.microsoft.com/library/e7k32f4k.aspx">profil temelli iyileştirme</a>. Her yöntem için en fazla işlem ömrü boyunca görüntülenmelidir.

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
**Zaman** bir düğüm için toplanan dahil örnekleri zaman içinde nasıl farklılık, bir görsel öğe bir sütundur. İstek toplam aralığı 32 zaman demet ayrılmıştır. Bu düğüm için kapsayıcı örnekleri bu 32 demet toplanır. Her demet çubuğu olarak temsil edilir. Çubuğunun yüksekliği ölçeklendirilmiş bir değeri temsil eder. İşaretlenen düğümleri için **CPU_TIME** veya **BLOCKED_TIME**, veya bir kaynak (CPU, disk veya iş parçacığı gibi) kullanma için belirgin bir ilişki olduğu çubuk tüketilen birinin temsil eder Bu demet dönemde kaynaklar. Bu ölçümleri, birden fazla kaynak tüketen tarafından yüzde 100'den büyük bir değer almak mümkündür. Örneğin, ortalama olarak bir aralık boyunca iki CPU kullanırsanız yüzde 200 alın.

## <a name="limitations"></a>Sınırlamalar

Varsayılan veri saklama dönemi beş gündür. Gün başına alınan en fazla veri 10 GB'tır.

Profil Oluşturucu bu hizmeti kullanmak için hiçbir ücret vardır. Profil Oluşturucu hizmetini kullanmak için web uygulamanızın olmalıdır en azından Web uygulamalarının temel katmana barındırılan.

## <a name="overhead-and-sampling-algorithm"></a>Ek yükü ve örnekleme algoritması

Profil Oluşturucu iki dakika saatte izlemelerini yakalama için etkinleştirilmiş profil oluşturucu olan uygulamayı barındıran her sanal makineye rastgele çalışır. Profil Oluşturucu çalıştırırken, yüzde 5 ' yüzde 15 CPU ek yükünü sunucusuna ekler.

Daha fazla sunucular, daha az profil oluşturucu genel uygulama performansına etkisi uygulamasını barındırmak için kullanılabilir. Herhangi bir anda yalnızca yüzde 5'inden sunucularının çalıştıran profil oluşturucu örnekleme algoritması sonuçlanır olmasıdır. Daha fazla sunucu profil oluşturucu çalıştırarak neden Sunucu yükünü dengelemek için web isteklerine hizmet vermek kullanılabilir.

## <a name="disable-profiler"></a>Profiler Devre Dışı Bırak
Altında tek tek web apps örneği için profil oluşturucu yeniden başlatmak veya durdurmak için **Web işleri**, Web uygulamaları kaynak gidin. Profil Oluşturucu silmek için Git **uzantıları**.

![Profil Oluşturucu için bir web işi devre dışı bırak][disable-profiler-webjob]

Profil Oluşturucu performans sorunları olabildiğince erken bulmak için tüm web uygulamaları üzerinde etkin olmasını öneririz.

WebDeploy değişiklikleri web uygulamanızı dağıtmak için kullanırsanız, dağıtım sırasında silinmiş App_Data klasöründe hariç emin olun. Aksi takdirde, profil oluşturucu uzantının dosyaları Azure web uygulamasına dağıtma başlatıldığında silinir.


## <a id="troubleshooting"></a>Sorun giderme

### <a name="too-many-active-profiling-sessions"></a>Çok fazla etkin profil oluşturma oturumu

Şu anda en fazla dört Azure web uygulamaları ve aynı hizmeti planında çalıştıran dağıtım yuvası üzerinde profil oluşturucu etkinleştirebilirsiniz. Profil Oluşturucu web işi çok fazla etkin profil oluşturma oturumu raporlama, bazı web uygulamaları için farklı hizmet planı taşıyın.

### <a name="how-do-i-determine-whether-application-insights-profiler-is-running"></a>Uygulama Öngörüler profil oluşturucu çalışır durumda olup olmadığını nasıl belirlerim?

Profil Oluşturucu web uygulamasında bir sürekli web işi olarak çalıştırır. Web uygulaması kaynak açabilirsiniz [Azure portal](https://portal.azure.com). İçinde **WebJobs** bölmesinde, durumunu denetleme **ApplicationInsightsProfiler**. Çalışıyor durumda değilse, açmak **günlükleri** daha fazla bilgi almak için.

### <a name="why-cant-i-find-any-stack-examples-even-though-profiler-is-running"></a>Profil Oluşturucu çalışıyor olsa bile neden herhangi yığını örnekler bulamadınız mı?

Kontrol edebilirsiniz bazı noktalar şunlardır:

* Daha yüksek veya web uygulama hizmet planınız temel katmana olduğundan emin olun.
* Daha sonra etkin veya web uygulamanızın uygulama Öngörüler SDK 2.2 Beta sahip olduğundan emin olun.
* Web uygulamanızı sahip olduğundan emin olun **appınsıghts_ınstrumentatıonkey** Application Insights SDK'sı tarafından kullanılan aynı izleme anahtarı ile yapılandırılmış ayarı.
* Web uygulamanız .NET Framework 4.6 üzerinde çalıştığından emin olun.
* Web uygulamanızı bir ASP.NET Core uygulamasıysa denetleyin [gerekli bağımlılıkları](#aspnetcore).

Profil Oluşturucu başlatıldıktan sonra hangi sırasında profil oluşturucu birkaç performans izlemeleri etkin olarak toplayan bir kısa Isınma Süresi yoktur. Bundan sonra Profil Oluşturucu performans izlemeleri iki dakika saatte toplar.

### <a name="i-was-using-azure-service-profiler-what-happened-to-it"></a>Azure Hizmet Profil Oluşturucu tarafından kullanılan. Ona ne?

Uygulama Öngörüler profil oluşturucu etkinleştirdiğinizde, Azure hizmet profil oluşturucu Aracısı devre dışı bırakılır.

### <a id="double-counting"></a>Çift paralel iş parçacığı sayımı

Bazı durumlarda, toplam süre yığını Görüntüleyicisi'nde birden çok istek süresince ölçümüdür.

İki veya daha fazla iş parçacığı isteği ile ilişkili olan ve paralel olarak çalıştıklarını bu durum oluşabilir. Bu durumda, toplam iş parçacığı süresi geçen süreyi büyük. Bir iş parçacığı diğer bağlı tamamlanmasını bekliyor olabilir. Görüntüleyici bu algılamaya çalışır ve sizi ilgilendirmeyen bekleme atlar, ancak çok fazla bilgi görüntüleme yan tarafında errs yerine ne önemli bilgiler olabilir atlayın.

Paralel iş parçacıkları, izlemeleri gördüğünüzde, istek için kritik yolu olmadığından emin olmak için hangi iş parçacığı bekleyen belirler. Çoğu durumda, hızlı bir şekilde bir bekleme durumuna geçtiğinde iş parçacığı üzerinde başka bir iş parçacığı basitçe bekliyor. Diğer iş parçacıklarında yoğunlaşabilirsiniz ve bekleyen iş parçacıklarının zamanında yok sayın.

### <a id="issue-loading-trace-in-viewer"></a>Profil oluşturma veri yok

Kontrol edebilirsiniz bazı noktalar şunlardır:

* Görüntülemeye çalıştığınız veri birkaç haftalık eski ise, zaman filtresi görüntülemeyi deneyin ve yeniden deneyin.
* Olun proxy'leri veya bir güvenlik duvarı sahip olmadığını erişimi engelledi https://gateway.azureserviceprofiler.net.
* Uygulamanızı kullanarak Application Insights izleme anahtarı etkinleştirilmiş profil oluşturma için kullanılan Application Insights kaynağı ile aynı olduğundan emin olun. Applicationınsights.config dosyasında anahtarıdır genellikle, ancak web.config veya app.config dosyasında olabilir.

### <a name="error-report-in-the-profiling-viewer"></a>Profil oluşturma Görüntüleyicisi'nde hata raporu

Bir destek bileti portalında gönderin. Hata iletisi bağıntı kimliği eklediğinizden emin olun.

### <a name="deployment-error-directory-not-empty-dhomesitewwwrootappdatajobs"></a>Dağıtım hatası: dizini boş değil ' D:\\ev\\site\\wwwroot\\App_Data\\işlerin

Web uygulamanızı Web Apps kaynak için etkinleştirilmiş Profil Oluşturucu ile dağıtarak, aşağıdakine benzer bir ileti görebilirsiniz:

*Dizini boş değil ' D:\\ev\\site\\wwwroot\\App_Data\\işlerin*

Web dağıtımı komut dosyaları ya da Visual Studio Team Services dağıtım ardışık çalıştırırsanız bu hata oluşur. Web dağıtımı görevi aşağıdaki ek dağıtım parametreleri eklemek için çözümüdür:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

Bu parametreler uygulama Öngörüler Profil Oluşturucu tarafından kullanılır ve yeniden dağıtın işlem engellemesini klasörünü silin. Bunlar şu anda çalışan profil oluşturucu örneği etkilemez.


## <a name="manual-installation"></a>El ile yükleme

Profil Oluşturucu yapılandırdığınızda güncelleştirmeler web uygulamanızın ayarlar hale getirilir. Ortamınızı gerektiriyorsa, güncelleştirmeleri el ile uygulayabilirsiniz. Uygulamanız için PowerApps bir Web Apps ortamında çalışan bir örnek olabilir.

1. İçinde **Web uygulama denetimi** bölmesinde, açık **ayarları**.
2. Ayarlama **.Net Framework sürüm** için **v4.6**.
3. Ayarlama **her zaman açık** için **üzerinde**.
4. Ekleme **appınsıghts_ınstrumentatıonkey** uygulama ayarlama ve SDK'sı tarafından kullanılan aynı izleme anahtarı için değer ayarlayın.
5. Açık **Gelişmiş Araçlar**.
6. Seçin **Git** Kudu Web sitesini açın.
7. Kudu Web sitesinde seçin **Site uzantıları**.
8. Yükleme **Application Insights** Azure Web Apps galerisinden.
9. Web uygulaması yeniden başlatın.

## <a id="profileondemand"></a> El ile tetikleyici Profil Oluşturucu
Biz profil oluşturucu geliştirilen, böylece biz profil oluşturucu app Services'de test edebilirsiniz bir komut satırı arabirimi eklediğimiz. Bu aynı arabirimi kullanarak, kullanıcılar ayrıca profil oluşturucu nasıl başlatır özelleştirebilirsiniz. Yüksek bir düzeyde profil oluşturucu arka planda profil yönetmek için Web Apps Kudu sistemi kullanır. Uygulama Insights uzantısını yüklediğinizde, profil oluşturucu barındıran bir sürekli web işi oluşturuyoruz. Bu aynı teknoloji gereksinimlerinize uyacak şekilde özelleştirebilirsiniz yeni bir web işi oluşturmak için kullanırız.

Bu bölümde nasıl yapılır:

* Profil Oluşturucu iki dakika bir düğmesine basın ile başlatmak için bir web işi oluşturun.
* Çalıştırmak için profil oluşturucu zamanlayabilirsiniz bir web işi oluşturun.
* Profil oluşturucu bağımsız değişkenleri ayarlayın.


### <a name="set-up"></a>Kurulum
İlk olarak, web işin paneliyle edinin. Altında **ayarları**seçin **WebJobs** sekmesi.

![Web işleri dikey penceresi](./media/app-insights-profiler/webjobs-blade.png)

Gördüğünüz gibi Bu panoyu sitenizde şu an yüklü olan tüm web işleri görüntüler. Çalışan profil oluşturucu işin ApplicationInsightsProfiler2 web işi görebilirsiniz. Burada yeni web işleri el ile ve zamanlanmış profil oluşturma için oluşturuyoruz budur.

Gereksinim duyduğunuz ikili dosyaları indirmek için aşağıdakileri yapın:

1.  Kudu sitesinde üzerinde **geliştirme araçları** sekmesine **Gelişmiş Araçlar** sekmesinde Kudu Logonuzla ve ardından **Git**.  
   Yeni bir site açar ve otomatik olarak imzalanmış.
2.  Dosya Gezgini gidin profil oluşturucu ikilileri indirmek için **hata ayıklama Konsolu'nda** > **CMD**, sayfanın en üstünde bulunduğu.
3.  Seçin **Site** > **wwwroot** > **App_Data** > **işleri**  >   **Sürekli**.  
   Adlı bir klasör görmelisiniz *ApplicationInsightsProfiler2*. 
4. Klasör sol tarafında seçin **karşıdan** simgesi.  
   Bu eylem indirmeleri *ApplicationInsightsProfiler2.zip* dosya. Bu zip arşivini taşımak için temiz bir dizin oluşturmanızı öneririz.

### <a name="setting-up-the-web-job-archive"></a>Web işi arşivi ayarlama
Azure Web sitesine yeni bir web işi eklediğinizde, aslında zip arşivini ile oluşturduğunuz bir *run.cmd* içinde dosya. *Run.cmd* dosya söyler web işi sistemi web işi çalıştırdığınızda yapmanız gerekenler.

1.  Yeni bir klasör oluşturun (örneğin, *RunProfiler2Minutes*).
2.  Dosyaları ayıklanan kopyalayın *ApplicationInsightProfiler2* bu yeni klasör klasörüne.
3.  Yeni bir *run.cmd* dosya.  
    Başlamadan önce kolaylık olması için Visual Studio kodda çalışma klasörü açabilirsiniz.
4.  Komut dosyasında ekleme `ApplicationInsightsProfiler.exe start --engine-mode immediate --single --immediate-profiling-duration 120`. Komutları aşağıda açıklanmıştır:

    * `start`: Başlatmak için profil oluşturucu söyler.  
    * `--engine-mode immediate`: Profil hemen başlamak için profil oluşturucu söyler.  
    * `--single`: Çalıştırmak ve otomatik olarak durdurmak için profil oluşturucu söyler.  
    * `--immediate-profiling-duration 120`: 120 saniye veya 2 dakika çalıştırmak için profil oluşturucu söyler.

5.  Yaptığınız değişiklikleri kaydedin.
6.  Klasöre sağ tıklayıp ardından seçerek arşiv **göndermek** > **sıkıştırılmış (daraltılmış) klasör**.  
   Bu eylem klasörünüzün adını kullanan bir .zip dosyası oluşturur.

![Profil oluşturucu komut Başlat](./media/app-insights-profiler/start-profiler-command.png)

Sitenizdeki web işler ayarlamak için kullanabileceğiniz bir web işi .zip dosyası oluşturdunuz.

### <a name="add-a-new-web-job"></a>Yeni bir web işi ekleme
Bu bölümde, yeni bir web işi sitenizde ekleyin. Aşağıdaki örnek, bir el ile Tetiklenmiş web işi eklemek gösterilmiştir. El ile Tetiklenmiş web işi ekledikten sonra işlemi neredeyse zamanlanmış web işi için aynıdır.

1.  Git **Web işleri** Pano.
2.  Araç çubuğunda seçin **Ekle**.
3.  Web işinizin bir ad verin.  
    Daha anlaşılır olması için bu, arşiv adı ile eşleşmesi için ve sürümleri çeşitli kaydınızı açmak için yardımcı olabilir *run.cmd* dosya.
4.  İçinde **karşıya dosya yükleme** select form alanının **açık dosya** simgesini ve sonra önceki bölümde oluşturduğunuz .zip dosyasını arayın.

    a.  İçinde **türü** kutusunda **Triggered**.  
    b.  İçinde **Tetikleyicileri** kutusunda **el ile**.

5.  **Tamam**’ı seçin.

![Profil oluşturucu komut Başlat](./media/app-insights-profiler/create-webjob.png)

### <a name="run-profiler"></a>Profil Oluşturucu çalıştırın

El ile tetikleyebilir yeni bir web işi sahip olduğunuza göre bu bölümdeki yönergeleri izleyerek çalıştırmak deneyebilirsiniz.

Yalnızca bir sahip tasarım gereği, *ApplicationInsightsProfiler.exe* herhangi bir anda bir makine üzerinde çalışan işlemi. Bu nedenle, başlamadan önce devre dışı bırakma *sürekli* bu panosundan web işi. 
1. Yeni web işi içeren satırı seçin ve ardından **durdurmak**. 
2. Araç çubuğunda seçin **yenileme**ve durumu iş durduruluncaya gösterir onaylayın.
3. Yeni web işi içeren satırı seçin ve ardından **çalıştırmak**.
4. Halen seçiliyken, araç çubuğunda, satır seçin **günlükleri** komutu.  
    Bu eylem yeni web işi için web işleri Pano açar ve en son çalıştırır ve sonuçları listeler.
5. Yalnızca başladıktan Çalıştır örneğini seçin.  
    Yeni web işi başarıyla tetiklendi profil başlatıldığını onaylayın profil oluşturucu gelen bazı tanılama günlükleri görüntüleyebilirsiniz.

### <a name="things-to-consider"></a>Göz önünde bulundurmanız gerekenler

Bu yöntem görece basit olmasına rağmen aşağıdakileri göz önünde bulundurun:

* Web işinizin hizmetimizi tarafından yönetilmediğinden, biz web işinizin Aracısı ikili dosyaları güncelleştirmek için bir yolu yoktur. Uzantınızı güncelleştirme ve ondan kapmasını tarafından son ikili dosyaları indirmek için tek yolu olacak şekilde biz şu anda bir kararlı indirme sayfası bizim ikili dosyaları yok *sürekli* klasörü olarak, önceki adımlarda vermedi.

* Bu işlem, başlangıçta son kullanıcılar yerine geliştiricileri için tasarlanmış olan komut satırı bağımsız değişkenleri kullandığından, bağımsız değişkenler gelecekte değişebilir. Yükseltme sırasında olası değişikliklerden haberdar olun. Bir web işi eklemek, çalıştırmak ve çalıştığından emin olmak için test etmek için bir sorun çoğunu olmamalıdır. Sonuç olarak, biz bu olmadan el ile işlem işlemek için bir kullanıcı Arabirimi oluşturacaksınız.

* Web işleri, Web uygulamaları benzersiz özelliğidir. Web işi çalıştığında, işleminizi aynı ortam değişkenleri ve Web sitenizi olan uygulama ayarlarını sahip olmasını sağlar. Başka bir deyişle, geçiş için profil oluşturucu komut satırı aracılığıyla izleme anahtarı gerekmez. Profil Oluşturucu izleme anahtarı ortamından seçmeniz gerekir. Ancak, profil oluşturucu geliştirme kutunuzda veya Web Apps dışında bir makinede çalıştırmak istiyorsanız, bir izleme anahtarı sağlamanız gerekir. Bir bağımsız değişken geçirerek yapabilirsiniz `--ikey <instrumentation-key>`. Bu değer, uygulamanızı kullanarak izleme anahtarını eşleşmelidir. Profil Oluşturucu günlük çıktısı profil oluşturucu kullanmaya hangi ikey ve profil sırada olup, izleme anahtarını etkinliğinden algıladık söyler.

* El ile Tetiklenmiş web işleri Web kancası tetiklenebilir. Web işi Panoda sağ tıklayarak ve özelliklerini görüntüleyerek bu URL'yi elde edebilirsiniz. Veya, araç çubuğunda, seçtiğiniz **özellikleri** tabloda web işi seçtikten sonra. Bu yaklaşım CI/CD hattınızı (gibi VSTS) veya Microsoft Flow şöyle profil oluşturucu tetikleme gibi sınırsız olasılıklar yukarı açar (https://flow.microsoft.com/en-us/). Sonuç olarak, tercih ettiğiniz nasıl karmaşık, yapmak isteyip istememenize bağlıdır, *run.cmd* dosyası (da olabilen bir *run.ps1* dosyası), ancak esneklik yok.

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
