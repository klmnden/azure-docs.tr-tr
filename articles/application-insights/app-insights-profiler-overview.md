---
title: Azure Application Insights Profiler ile üretim uygulamalarında profil | Microsoft Docs
description: Web sunucusu kodunuza etkin yolu ile düşük Ayak izi profil oluşturucu tanımlar.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: 7780c10233a0ce256ee6e9015f40ea789516c25b
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2018
ms.locfileid: "52726908"
---
# <a name="profile-production-applications-in-azure-with-application-insights"></a>Azure Application Insights ile profil üretim uygulamaları
## <a name="enable-profiler-for-your-application"></a>Uygulamanız için Profiler'ı etkinleştir

Application Insights Profiler, üretim ortamında çalışan uygulamalar için performans izleme sağlar. Verileri otomatik olarak uygun ölçekte, son kullanıcılarınız olumsuz biçimde etkilemeden yakalar. Profiler zaman belirli bir Web isteği uzun sürüyor "etkin" kod yolu belirlemenize yardımcı olur. 

Profil Oluşturucu, aşağıdaki Azure hizmetleri üzerinde dağıtılan .net uygulamaları ile birlikte çalışır. Her hizmet türü için Profiler'ı etkinleştirmek için özel yönergeler aşağıdaki bağlantılarda içindir.

* [Uygulama Hizmetleri](app-insights-profiler.md?toc=/azure/azure-monitor/toc.json)
* [Cloud Services](app-insights-profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Service Fabric uygulamaları](app-insights-profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Sanal makineler ve sanal makine Scalesets](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Profiler etkinleştirdiniz, ancak izlemeleri görmüyorsanız, denetleyin bizim [sorun giderme kılavuzu.](app-insights-profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json)

Profiler şirket içinde çalışan resmi olarak desteklenmez, ancak bazı sahibiz [yönergeleri deneyebilirsiniz.](https://docs.microsoft.com/azure/application-insights/enable-profiler-compute#enable-profiler-on-on-premises-servers)

## <a name="view-profiler-data"></a>Profil Oluşturucu verileri görüntüle

Profil Oluşturucu izlemeleri yüklenecek için sırada etkin bir şekilde işleme uygulamanızı ister. Bir denemeyi yapıyorsanız, web app kullanarak istekleri oluşturabilir [Application Insights performans testi](https://docs.microsoft.com/vsts/load-test/app-service-web-app-performance-test). Yeni Profiler'ı etkinleştirdiyseniz, kısa bir yük testi çalıştırabilirsiniz. Yük testi çalışırken, basın **profili artık** düğmesine [ **Profiler Ayarları sayfası**](app-insights-profiler-settings.md#profiler-settings-page). Profil Oluşturucu çalışmaya başladıktan sonra yaklaşık olarak her saatte bir kez ve iki dakikalık bir süre boyunca rastgele profil. Uygulama istekleri gitmenize işliyorsa, Profiler izlemeleri saatte yükleyin.

Uygulamanız bazı trafik alır ve profil oluşturucu trances karşıya yüklemek için zaman bulana sonra görüntülemek üzere izlemesi olmalıdır. Bu işlem, 5-10 dakika sürebilir. İzlemeleri görüntülemek için Git **performans** bölmesinde **eylemleri** profiler izlemeleri görüntülemek ve ardından **Profiler izlemeleri** düğmesi.

![Application Insights performans bölmesi Önizleme Profiler izlemeleri][performance-blade]

Bir istek yürütülürken harcanan süre kod düzeyinde dökümünü görüntülemek için bir örnek seçin.

![Application Insights izleme Gezgini][trace-explorer]

İzleme Gezgini, aşağıdaki bilgileri görüntüler:

* **Etkin yolu Göster**: açılır en büyük, düğüm yaprak veya en az bir şey kapatın. Çoğu durumda bu, bir performans sorununun düğümüdür.
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

Varsa **clr! ThePreStub** büyük bir miktarını alır bir istek için süresini bu istek bu yöntem yürütür birinci olduğunu gösterir. İlk yöntem yüklemek .NET Framework çalışma zamanı için zaman büyük/küçük harf önemlidir. Önce kullanıcılarınızın erişim veya Native Image Generator (ngen.exe) çalıştırmayı göz önünde bulundurun, derlemelerini söz konusu bölümü kod yürüten bir Isınma sürecinden göz önünde bulundurabilirsiniz.

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

**AWAIT\_zaman** kod başka bir görevin tamamlanmasını beklediğini belirtir. Bu genellikle gerçekleşir C# ile **AWAIT** deyimi. Ne zaman yaptığını bir C# **AWAIT**, iş parçacığı geriye doğru alır ve denetimi iş parçacığı havuzuna döndürür ve bekleyen engellenmiş iş parçacığı yok **AWAIT** tamamlanması. Ancak, mantıksal olarak, iş parçacığı, yaptığınız **AWAIT** "engellendi" ve işlemin tamamlanmasını bekliyor. **AWAIT\_zaman** deyimi görevin sonlanmasını bekleyen engellenen süreyi gösterir.

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

## <a name="next-steps"></a>Sonraki Adımlar
Azure uygulamanız için Application Insights Profiler ' ı etkinleştir
* [Uygulama Hizmetleri](app-insights-profiler.md?toc=/azure/azure-monitor/toc.json)
* [Cloud Services](app-insights-profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Service Fabric uygulamaları](app-insights-profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Sanal makineler ve sanal makine Scalesets](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)


[performance-blade]: ./media/app-insights-profiler/performance-blade-v2-examples.png
[trace-explorer]: ./media/app-insights-profiler/trace-explorer.png
