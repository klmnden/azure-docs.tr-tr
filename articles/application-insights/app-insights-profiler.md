---
title: Azure'da Application Insights Profiler ile canlı web uygulamalarının profilini | Microsoft Docs
description: Web sunucusu kodunuza etkin yolu ile düşük Ayak izi profil oluşturucu tanımlar.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: 6048a17bf50ecac691c7cf687f87e454c54ee9d9
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39521892"
---
# <a name="profile-live-azure-web-apps-with-application-insights"></a>Application Insights ile canlı Azure web apps profili

Azure Application Insights'ın bu özelliği için Azure App Service'in Web Apps özelliği genel kullanıma sunulmuştur ve Azure işlem kaynakları için önizlemeye sunulmuştur. Bilgi için ilgili [profil oluşturucu şirket içi kullanımı ile ilgili](https://docs.microsoft.com/azure/application-insights/enable-profiler-compute#enable-profiler-on-on-premises-servers).

Bu makalede ele alınmaktadır kullanırken, canlı web uygulamanızın her bir yöntemin harcanan süreyi [Application Insights](app-insights-overview.md). Application Insights Profiler araç, uygulamanız tarafından sunulduğunu dinamik isteklerin ayrıntılı profilleri görüntüler. Profiler vurgular *etkin yolu* , en çok zaman kullanır. Yanıt süresi, çeşitli istekleri örnekleme olarak profillenen. Çeşitli teknikler kullanarak uygulama ile ilişkili ek yükü en aza indirebilirsiniz.

Profiler şu anda Web Apps üzerinde çalışan ASP.NET ve ASP.NET Core web uygulamaları için çalışır. Temel Hizmet katmanını veya üzeri Profiler'ı kullanmak için gerekli değildir.

## <a id="installation"></a> Web uygulamalarınız için Profiler'ı etkinleştir

Kaynak kodunda App Insights SDK'sı dahil ettiyseniz bağımsız olarak, bir Web uygulamasını dağıttıktan sonra aşağıdakileri yapın:

1. Git **uygulama hizmetleri** bölmesinde Azure portalında.
1. Gidin **Ayarları > İzleme** bölmesi.

   ![Uygulama Hizmetleri portalında App ınsights'ı etkinleştirme](./media/app-insights-profiler/AppInsights-AppServices.png)

1. Ya da bölmesinde web uygulamanızı izlemek için var olan App Insights kaynağı seçin veya yeni bir kaynak oluşturmak için yönergeleri izleyin. Tüm varsayılan seçenekleri kabul edin. **Kod düzeyi tanılama** varsayılan olarak açıktır ve Profiler sağlar.

   ![App Insights site uzantısı Ekle][Enablement UI]

1. Profiler olan App Insights site uzantısı artık yüklüdür ve uygulama hizmetleri uygulama ayarı kullanılarak etkinleştirilir.

    ![Profiler uygulama ayarı][profiler-app-setting]

### <a name="enable-profiler-for-azure-compute-resources-preview"></a>Profiler etkinleştirme için Azure işlem kaynakları (Önizleme)

Bilgi için [Profiler önizleme sürümünü Azure işlem kaynakları](https://go.microsoft.com/fwlink/?linkid=848155).

## <a name="view-profiler-data"></a>Profil Oluşturucu verileri görüntüle

Trafiği uygulamanız aldığından emin olun. Bir denemeyi yapıyorsanız, kullanarak, web uygulama isteklerini oluşturabilirsiniz [Application Insights performans testi](https://docs.microsoft.com/vsts/load-test/app-service-web-app-performance-test). Yeni Profiler'ı etkinleştirdiyseniz, yaklaşık 15 profiler izlemeleri oluşturan dakika, kısa bir yük testi çalıştırabilirsiniz. Profiler zaten bir süredir etkin olsaydı, Canlı aklınızda Profiler rastgele iki kez her saat çalışır ve her iki dakikalık bir süre boyunca çalışır. İlk örnek profil oluşturucu izlemeleri almak emin olmak bir saatlik yük testi çalıştırmanızı öneririz.

Bazı traffic uygulamanızı aldıktan sonra Git **performans** bölmesinde **eylemleri** profiler izlemeleri görüntülemek ve ardından **Profiler izlemeleri** düğmesi.

![Application Insights performans bölmesi Önizleme Profiler izlemeleri][performance-blade-v2-examples]

Bir istek yürütülürken harcanan süre kod düzeyinde dökümünü görüntülemek için bir örnek seçin.

![Application Insights izleme Gezgini][trace-explorer]

İzleme Gezgini, aşağıdaki bilgileri görüntüler:

* **Etkin yolu Göster**: açılır en büyük, düğüm yaprak veya en az bir şey kapatın. Çoğu durumda, bu düğüm için bir performans sorununun bitişik.
* **Etiket**: işlev veya olay adı. Ağaç, kod ve (SQL ve HTTP olayları gibi) oluşan olaylar bir karışımını görüntüler. Üst olay genel istek süresini temsil eder.
* **Geçen**: işleminin başlangıç ve bitiş işlemin arasındaki zaman aralığı.
* **Zaman**: işlev veya olay çalışırken ilişkisinde diğer işlevlere zaman.

## <a name="how-to-read-performance-data"></a>Performans verileri okuma

Microsoft Hizmet Profil Oluşturucu örnekleme yöntemleri ve araçları, uygulamanızın performansını çözümlemek için kullanır. Ayrıntılı koleksiyonu devam ederken, hizmet profil oluşturucu her makine CPU yönerge işaretçisini her bir milisaniyeden kısa örnekler. Her örnek, şu anda yürütülmekte olan iş parçacığının tam çağrı yığınını yakalar. Bu iş parçacığı, yüksek bir düzeyde hem düşük bir düzeyde soyutlama yapmakta olduğu ayrıntılı bilgi verir. Hizmet Profil Oluşturucu, aynı zamanda etkinlik bağıntısı ve nedensellik ilişkilerini bağlam olayları, görev paralel kitaplığı (TPL) olayları ve iş parçacığı havuzu olayları değiştirme dahil olmak üzere, izlemek için diğer olayları toplar.

Zaman Çizelgesi Görünümü'nde görüntülenen çağrı yığınını izleme ve örnekleme sonucudur. Her örnek, iş parçacığının tam çağrı yığınını yakaladığından, başvuru diğer çerçeveler ve Microsoft .NET Framework kodu içerir.

### <a id="jitnewobj"></a>Nesne ayırma (clr! JIT\_yeni veya clr! JIT\_Newarr1)

**CLR! JIT\_yeni** ve **clr! JIT\_Newarr1** .NET Framework yönetilen yığından bellek ayırma yardımcı işlevleri. **CLR! JIT\_yeni** nesne ayrıldığında çağrılır. **CLR! JIT\_Newarr1** bir nesne dizisi ayrıldığında çağrılır. Bu iki işlev, genellikle hızlı ve süresi görece küçük miktarlarda gerçekleştirin. Görürseniz **clr! JIT\_yeni** veya **clr! JIT\_Newarr1** önemli miktarda zaman, zaman çizelgesinde almak, kod olarak çok sayıda nesne ayırma ve önemli miktarda bellek tüketen olduğunu gösterir.

### <a id="theprestub"></a>Kod (clr! yükleniyor ThePreStub)

**CLR! ThePreStub** ilk kez yürütülecek kodu hazırlar .NET Framework'teki yardımcı işlevdir. Bu genellikle içerir, ancak just-in-time (JIT) derleme için sınırlı değildir. Her bir C# yöntemin, **clr! ThePreStub** işlem ömrü boyunca en fazla bir kez çağrılmalıdır.

Varsa **clr! ThePreStub** oldukça fazla alan için bir istek süresini bu istek bu yöntem yürütür birinci olduğunu gösterir. İlk yöntem yüklemek .NET Framework çalışma zamanı için zaman büyük/küçük harf önemlidir. Önce kullanıcılarınızın erişim veya Native Image Generator (ngen.exe) çalıştırmayı göz önünde bulundurun, derlemelerini söz konusu bölümü kod yürüten bir Isınma sürecinden göz önünde bulundurabilirsiniz.

### <a id="lockcontention"></a>Kilit çakışması (clr! JITutil\_MonContention veya clr! JITutil\_MonEnterWorker)

**CLR! JITutil\_MonContention** veya **clr! JITutil\_MonEnterWorker** geçerli iş parçacığının bir kilidi serbest bırakılması beklediğini belirtir. C# yürüttüğünüzde genellikle bu metni görüntülenir **kilit** çağrılırken deyimi **Monitor.Enter** yöntemi veya bir metodu çağrılırken **Methodımploptions.Synchronized** özniteliği. Genellikle kilit çakışması oluşur, iş parçacığı _A_ kilit ve iş parçacığı edinme _B_ iş parçacığı önce aynı kilit almaya çalıştığında _A_ serbest.

### <a id="ngencold"></a>Kod ([SOĞUK]) yükleniyor

Yöntem adı içeriyorsa **[SOĞUK]**, gibi **mscorlib.ni! [ COLD]System.Reflection.CustomAttribute.IsDefined**, .NET Framework çalışma zamanı tarafından optimize edilmemiş ilk kez kodu yürüten <a href="https://msdn.microsoft.com/library/e7k32f4k.aspx">profil temelli iyileştirme</a>. Her yöntem için en fazla işlem yaşam süresi boyunca görüntülenmelidir.

Kod yükleme isteğini zamanında önemli miktarda alırsa bu istek yöntemi iyileştirilmemiş bölümünü yürütmek için birinci olduğunu gösterir. Kullanıcılarınızın erişim önce söz konusu bölümü kod yürüten bir Isınma işlem kullanmayı düşünün.

### <a id="httpclientsend"></a>HTTP isteği gönder

Yöntemleri ister **HttpClient.Send** kod tamamlanması için bir HTTP isteği beklediğini belirtir.

### <a id="sqlcommand"></a>Veritabanı işlemi

Yöntemleri ister **SqlCommand.Execute** kod veritabanı işlemin tamamlanmasını beklediğini belirtir.

### <a id="await"></a>Bekleniyor (AWAIT\_saat)

**AWAIT\_zaman** kod başka bir görevin tamamlanmasını beklediğini belirtir. Bu genellikle gerçekleşir C# ile **AWAIT** deyimi. Ne zaman yaptığını C# **AWAIT**, iş parçacığı geriye doğru alır ve denetimi iş parçacığı havuzuna döndürür ve bekleyen engellenmiş iş parçacığı yok **AWAIT** tamamlanması. Ancak, mantıksal olarak, iş parçacığı, yaptığınız **AWAIT** "engellendi" ve işlemin tamamlanmasını bekliyor. **AWAIT\_zaman** deyimi görevin sonlanmasını bekleyen engellenen süreyi gösterir.

### <a id="block"></a>Engellenme süresi

**BLOCKED_TIME** kullanılabilir olması başka bir kaynak kodu beklediğini belirtir. Örneğin, bir eşitleme nesnesi, kullanılabilir olması için bir iş parçacığı veya son isteği bekliyor olabilir.

### <a id="cpu"></a>CPU süresi

CPU yönerge Yürütülüyor meşgul.

### <a id="disk"></a>Disk Zamanı

Disk işlemleri uygulama gerçekleştiriyor.

### <a id="network"></a>Ağ süresi

Uygulama ağ işlemlerini gerçekleştiriyor.

### <a id="when"></a>Zaman sütunu

**Olduğunda** bir düğüm için toplanan kapsamlı örnek zaman içinde nasıl farklılık, bir görselleştirme sütundur. Toplam istek aralığını 32 zaman demetlerin içine ayrılmıştır. Bu düğüm için kapsamlı örnekler 32 bu demetleri içinde toplanır. Her bir demete çubuğu olarak temsil edilir. Çubuk Yüksekliği ölçeklendirilmiş bir değeri temsil eder. İşaretlenmiş düğümleri için **CPU_TIME** veya **BLOCKED_TIME**, veya tüketen bir kaynağa (CPU, disk veya iş parçacığı gibi) için açık bir ilişkisi olduğu çubuğu tüketimi birini temsil eder Bu demet dönemi sırasında kaynaklar. Bu ölçümler için birden fazla kaynak tüketmeye tarafından yüzde 100'den büyük bir değer almak mümkündür. Örneğin, ortalama olarak, bir aralık boyunca iki CPU kullanırsanız yüzde 200 alın.

## <a name="limitations"></a>Sınırlamalar

Varsayılan veri saklama süresi, beş gündür. Gün başına alınan en fazla veri 10 GB'tır.

Profiler bu hizmeti kullanmak için herhangi bir ücreti yoktur. Profiler hizmeti kullanmak için web uygulamanızı olmalıdır en az temel katman Web Apps barındırılan.

## <a name="overhead-and-sampling-algorithm"></a>Ek yükü ve örnekleme algoritması

Profiler rastgele Profiler izlemeleri yakalama için etkinleştirilmiş olan bir uygulama her sanal makinede iki dakika başı çalıştırır. Profiler çalışırken %5 yüzde 15 CPU ek yükünü sunucusuna ekler.

Daha fazla sunucu, daha az Profiler genel uygulama performansına etkisi uygulamasını barındırmak için kullanılabilir. Herhangi bir zamanda yalnızca yüzde 5 sunucuları çalıştıran Profiler örnekleme algoritması sonuçlanır olmasıdır. Daha fazla sunucu Profiler çalıştırarak nedeniyle Sunucu yükünü dengelemek için isteklerine hizmet vermek kullanılabilir.

## <a name="disable-profiler"></a>Profiler Devre Dışı Bırak

Durdurmak veya Profiler altında bir tek tek web uygulamasının örneği için yeniden **Web işleri**Web Apps kaynak gidin. Profiler'ı silmek için Git **uzantıları**.

![Bir web işi için Profiler devre dışı bırak][disable-profiler-webjob]

Profiler herhangi bir performans sorunu mümkün olduğunca erken bulmak için tüm web apps'de etkin olmasını öneririz.

Değişiklikleri web uygulamanıza dağıtılacak WebDeploy kullanırsanız, dağıtım sırasında silinen App_Data klasöründe hariç emin olun. Aksi takdirde, Profiler uzantının dosyaları web uygulamasını azure'a dağıtma başlatıldığında silinir.


## <a id="troubleshooting"></a>Sorun giderme

### <a name="too-many-active-profiling-sessions"></a>Profil oluşturma çok fazla etkin oturumlar

Şu anda en fazla dört Azure web uygulamaları ve aynı hizmet planında çalıştırdığınız dağıtım yuvalarını Profiler etkinleştirebilirsiniz. Profiler web işi profil oluşturma çok fazla etkin oturum yapıyorsa, bazı web uygulamaları, farklı hizmet planına taşıyın.

### <a name="how-do-i-determine-whether-application-insights-profiler-is-running"></a>Application Insights Profiler azalmadığını nasıl belirlerim?

Profiler web uygulamasında sürekli web işi çalıştırır. Web uygulaması kaynak açabileceğiniz [Azure portalında](https://portal.azure.com). İçinde **WebJobs** bölmesinde, durumunu **ApplicationInsightsProfiler**. Çalışmıyorsa, açık **günlükleri** daha fazla bilgi için.

### <a name="why-cant-i-find-any-stack-examples-even-though-profiler-is-running"></a>Profiler çalışıyor olmasına rağmen neden herhangi bir yığın örnekler bulamıyor musunuz?

Denetleyebilirsiniz bazı işlemler aşağıda verilmiştir:

* Daha yüksek veya web uygulama hizmet planınız temel katman olduğundan emin olun.
* Daha sonra etkin veya web uygulamanızı Application Insights SDK'sını 2.2 Beta sahip olduğundan emin olun.
* Web uygulamanızı olduğundan emin olun **appınsıghts_ınstrumentatıonkey** ayar Application Insights SDK'sı tarafından kullanılan aynı izleme anahtarı ile yapılandırılmış.
* Web uygulamanızı .NET Framework 4.6 üzerinde çalıştığından emin olun.
* Web uygulamanız için bir ASP.NET Core uygulaması ise, en az çalıştırmalıdır ASP.NET Core 2.0.

Profiler başlatıldıktan sonra bir kısa Isınma dönemi sırasında Profiler etkin bir şekilde birkaç performans izlemelerini toplar yoktur. Bundan sonra Profiler saatte iki dakika boyunca performans izlemelerini toplar.

> [!NOTE]
> İzlemeleri ASP.NET Core 2.1 üzerinde çalışan uygulamalardan alınan karşıya yükleme engelleyen Profil Oluşturucu aracı bir hata yoktur. Biz bir düzeltme üzerinde çalışıyoruz ve sahip olur, hazır olan en kısa sürede.

### <a name="i-was-using-azure-service-profiler-what-happened-to-it"></a>Ben Azure Service profiler kullanıyordu. Dala ne?

Application Insights Profiler ' ı etkinleştirdiğinizde, Azure Service profiler Aracısı devre dışı bırakıldı.

### <a id="double-counting"></a>Çift paralel iş parçacığı sayımı

Bazı durumlarda, toplam süre ölçümü yığın Görüntüleyicisi'nde istek süresi büyük.

İki veya daha fazla iş parçacığı bir istekle ilişkilendirilen ve paralel olarak çalışan bu durum oluşabilir. Bu durumda, toplam iş parçacığı geçen süre uzun süredir. Bir iş parçacığından diğerine tamamlanması bekliyor olabilir. Görüntüleyici bu algılamaya çalışır ve sizi ilgilendirmeyen bekleme atlar, ancak çok fazla bilgileri görüntüleme kenarındaki errs yerine atlamak ne kritik bilgiler olabilir.

Paralel iş parçacıkları, izlemelerinde gördüğünüzde, istek için kritik yolu olmadığından emin olmak için hangi iş parçacıkları bekleyen belirleyin. Çoğu durumda, iş parçacığı hızlı bir şekilde bir bekleme durumuna geçtiğinde yalnızca diğer iş parçacıkları üzerinde bekliyor. Diğer iş parçacıkları hakkında yoğunlaşabilirsiniz ve bekleyen iş parçacıklarının sürede yoksay.

### <a id="issue-loading-trace-in-viewer"></a>Profil oluşturma veri yok

Denetleyebilirsiniz bazı işlemler aşağıda verilmiştir:

* Görüntülemeye çalıştığınız veriler birkaç haftadır eskiyse, zaman filtrenizi görüntülemeyi deneyin ve yeniden deneyin.
* Olun proxy'leri veya bir güvenlik duvarı olması erişimi engelledi https://gateway.azureserviceprofiler.net.
* Uygulamanızda kullandığınız Application Insights izleme anahtarı oluşturmayı etkinleştirmek için kullanılan Application Insights kaynağı ile aynı olduğundan emin olun. Anahtarı Applicationınsights.config dosyasında genellikle olduğu halde web.config veya app.config dosyasında da olabilir.

### <a name="error-report-in-the-profiling-viewer"></a>Profil oluşturma Görüntüleyicisi'nde hata raporu

Portalında bir destek bileti gönderin. Hata iletisindeki bağıntı Kimliğini eklediğinizden emin olun.

### <a name="deployment-error-directory-not-empty-dhomesitewwwrootappdatajobs"></a>Dağıtım hatası: Dizin boş değil ' D:\\giriş\\site\\wwwroot\\App_Data\\işlerin

Profiler etkin ile web uygulamanıza Web Apps kaynak dağıtıyorsanız, aşağıdaki gibi bir ileti görebilirsiniz:

*Dizin boş değil ' D:\\giriş\\site\\wwwroot\\App_Data\\işlerin*

Web dağıtımı betikleri veya Visual Studio Team Services dağıtım ardışık çalıştırırsanız, bu hata oluşur. Web dağıtımı görevi aşağıdaki ek dağıtım parametreleri eklemek için bir çözümdür:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

Bu parametreler, Application Insights Profiler ' ı tarafından kullanılan ve yeniden dağıtma işlemi engellemesini klasörü silin. Bunlar şu anda çalışıyor Profiler örneği etkilemez.

## <a name="manual-installation"></a>El ile yükleme

Profiler'ı yapılandırırken, güncelleştirmeler web uygulamasının Ayarlar hale getirilir. Ortamınızı gerektiriyorsa, güncelleştirmelerin el ile uygulayabilirsiniz. Örnek uygulamanız PowerApps için Web Apps ortamda çalışıyor olabilir.

1. İçinde **Web uygulaması denetimi** bölmesi açık **ayarları**.
1. Ayarlama **.Net Framework sürümü** için **v4.6**.
1. Ayarlama **her zaman açık** için **üzerinde**.
1. Ekleme **appınsıghts_ınstrumentatıonkey** uygulama ayarı ve değeri SDK'sı tarafından kullanılan aynı izleme anahtarını ayarlayın.
1. Açık **Gelişmiş Araçlar**.
1. Seçin **Git** Kudu Web sitesini açın.
1. Kudu Web sitesinde seçin **Site uzantıları**.
1. Yükleme **Application Insights** Azure Web uygulamaları Galerisi.
1. Web uygulamasını yeniden başlatın.

## <a id="profileondemand"></a> Profiler el ile tetikleme

Profiler el ile bir düğme tıklatma ile tetiklenebilir. Web performans testi çalıştırdığınızı varsayın. Web uygulamanızı yük altında nasıl performans gösterdiğini anlamak için izlemeleri gerekir. Yük testi çalıştırma, ancak bu rastgele bir örnekleme aralığı kaçırabilirsiniz bildiğiniz izlemeleri olduğunda yakalandığını denetime sahip olmak önemlidir.
Aşağıdaki adımlar, bu senaryoda nasıl çalıştığını göstermektedir:

### <a name="optional-step-1-generate-traffic-to-your-web-app-by-starting-a-web-performance-test"></a>(İsteğe bağlı) 1. adım: web performans testi başlatarak trafiğin web uygulamanıza oluşturur.

Web uygulamanıza gelen trafik varsa veya trafiği el ile oluşturmak istiyorsanız, bu bölümü atlayın ve 2. adım devam edin.

Application Insights portalına gidin **yapılandırma > performans testi**. Yeni performans testi başlatmak için yeni düğmesine tıklayın.
![Yeni performans testi oluşturma][create-performance-test]

İçinde **yeni performans testi** bölmesinde test hedef URL'yi yapılandırın. Tüm varsayılan ayarları kabul edin ve yük testi çalıştırmaya başlayın.

![Yük testi Yapılandır][configure-performance-test]

Yeni test durumu sürüyor ardından ilk olarak sıraya alınır görürsünüz.

![Yük testi gönderildi ve kuyruğa alındı][load-test-queued]

![devam eden yük testi çalışırken][load-test-in-progress]

### <a name="step-2-start-profiler-on-demand"></a>2. adım: Profil Oluşturucu üzerine Başlat

Yük testi çalışmaya başladığında, biz yük alırken web uygulamasında izlemeleri yakalamak amacıyla profil oluşturucu başlayabilirsiniz.
Yapılandırma Profiler bölmesine gidin:

![Profil Oluşturucu bölmesi girişinin yapılandırın][configure-profiler-entry]

Üzerinde **Profiler yapılandırma bölmesinde**, var. bir **profili artık** profil oluşturucu bağlantılı web uygulamaları tüm örneklerinde tetiklemek için düğme. Ayrıca, profil oluşturucu geçmişte çalıştırırken üzerinde görünürlük sağlanır.

![Profiler isteğe bağlı][profiler-on-demand]

Bildirim ve çalıştırma durumu profil oluşturucu değiştirme durumunu görürsünüz.

### <a name="step-3-view-traces"></a>3. adım: Görünüm izlemeleri

Profil Oluşturucu çalışmayı tamamladıktan sonra performans dikey penceresini ve görünüm izlemeleri gitmek için bildirim yönergeleri izleyin.

### <a name="troubleshooting-on-demand-profiler"></a>İsteğe bağlı profil oluşturucu sorunlarını giderme

Bazen bir isteğe bağlı oturum sonra Profiler zaman aşımı hata iletisini görebilirsiniz:

![Profiler zaman aşımı hatası][profiler-timeout]

Bu hatayı neden gördüğünüz iki nedeni olabilir:

1. İsteğe bağlı Profil Oluşturucu oturumu başarılı oldu ancak Application Insights toplanan verileri işlemek için uzun sürdü. Verileri 15 dakika içinde işlenmekte olan tamamlanmadı, portal bir zaman aşımı iletisi görüntüler. Bir süre sonra ancak Profiler izlemeleri gösterilir. Bu durumda, lütfen yalnızca hata iletisi şimdilik yoksayın. Etkin bir düzeltme üzerinde çalışıyoruz

1. Web uygulamanızın isteğe bağlı özellik yok Profiler Aracısı daha eski bir sürümü vardır. Application Insights profilinde önceden etkinleştirilmişse, isteğe bağlı özelliği kullanmaya başlamak için Profiler aracınızı güncelleştirmeye gerek duyduğunuz yüksektir.
  
En son Profiler'ı yüklemek ve denetlemek için aşağıdaki adımları izleyin:

1. Uygulama Hizmetleri uygulama ayarlarına gidin ve aşağıdaki ayarları ayarlanıp ayarlanmadığını denetleyin:
    * **Appınsıghts_ınstrumentatıonkey**: Application Insights için uygun bir izleme anahtarı ile değiştirin.
    * **APPINSIGHTS_PORTALINFO**: ASP.NET
    * **APPINSIGHTS_PROFILERFEATURE_VERSION**: 1.0.0 Bu ayarlardan herhangi birini ayarlanmazsa, en son site uzantısını yüklemek için Application ınsights'ı etkinleştirme bölmesine gidin.

1. Uygulama Hizmetleri Portalı'nda Application Insights bölmesine gidin.

    ![Uygulama Hizmetleri portalından Application Insights'ı etkinleştir][enable-app-insights]

1. Aşağıdaki sayfada bir 'Güncelleştirme' düğmesi görürseniz, en son Profiler aracısı yükler, Application Insights site uzantısını güncelleştirmek için tıklayın.
![Güncelleştirme site uzantısı][update-site-extension]

1. Ardından **değiştirme** emin olmak için Profiler select açık olduğundan ve **Tamam** değişiklikleri kaydetmek için.

    ![App ınsights kaydedin ve değiştirme][change-and-save-appinsights]

1. Geri Git **uygulama ayarları** App Service uygulama ayarları aşağıdakileri denetleyin sekmesinde ayarlanır:
    * **Appınsıghts_ınstrumentatıonkey**: application ınsights için uygun bir izleme anahtarı ile değiştirin.
    * **APPINSIGHTS_PORTALINFO**: ASP.NET
    * **APPINSIGHTS_PROFILERFEATURE_VERSION**: 1.0.0

    ![Profil Oluşturucu için uygulama ayarları][app-settings-for-profiler]

1. İsteğe bağlı olarak, uzantı sürümü ve kullanılabilir güncelleştirme yok sağlamaktan denetleyin.

    ![Uzantı güncelleştirme denetleme][check-for-extension-update]

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio'da Application Insights ile çalışma](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[appinsights-in-appservices]:./media/app-insights-profiler/AppInsights-AppServices.png
[Enablement UI]: ./media/app-insights-profiler/Enablement_UI.png
[profiler-app-setting]:./media/app-insights-profiler/profiler-app-setting.png
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
[create-performance-test]: ./media/app-insights-profiler/new-performance-test.png
[configure-performance-test]: ./media/app-insights-profiler/configure-performance-test.png
[load-test-queued]: ./media/app-insights-profiler/load-test-queued.png
[load-test-in-progress]: ./media/app-insights-profiler/load-test-inprogress.png
[profiler-on-demand]: ./media/app-insights-profiler/Profiler-on-demand.png
[configure-profiler-entry]: ./media/app-insights-profiler/configure-profiler-entry.png
[enable-app-insights]: ./media/app-insights-profiler/enable-app-insights-blade-01.png
[update-site-extension]: ./media/app-insights-profiler/update-site-extension-01.png
[change-and-save-appinsights]: ./media/app-insights-profiler/change-and-save-appinsights-01.png
[app-settings-for-profiler]: ./media/app-insights-profiler/appsettings-for-profiler-01.png
[check-for-extension-update]: ./media/app-insights-profiler/check-extension-update-01.png
[profiler-timeout]: ./media/app-insights-profiler/profiler-timeout.png
