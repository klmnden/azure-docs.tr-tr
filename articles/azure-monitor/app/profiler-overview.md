---
title: Azure Application Insights Profiler ile üretim uygulamalarında profil | Microsoft Docs
description: Web sunucusu kodunuza etkin yolu ile düşük Ayak izi profil oluşturucu tanımlar.
services: application-insights
documentationcenter: ''
author: cweining
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 08/06/2018
ms.author: cweining
ms.openlocfilehash: c07b325f3de6cd2cf3aaa436736786d2cdc42881
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58498137"
---
# <a name="profile-production-applications-in-azure-with-application-insights"></a>Azure Application Insights ile profil üretim uygulamaları
## <a name="enable-application-insights-profiler-for-your-application"></a>Uygulamanız için Application Insights Profiler ' ı etkinleştir

Azure Application Insights Profiler, üretim ortamında çalışan uygulamalar için performans izleme sağlar. Profiler, kullanıcılarınız olumsuz biçimde etkilemeden uygun ölçekte otomatik olarak verileri yakalar. Profiler zaman belirli bir web isteği işleme uzun sürüyor "etkin" kod yolu belirlemenize yardımcı olur. 

Profiler aşağıdaki Azure hizmetleri üzerinde dağıtılan .NET uygulamalarıyla çalışır. Her hizmet türü için Profiler'ı etkinleştirmek için özel yönergeler aşağıdaki bağlantılarda içindir.

* [Azure App Service](profiler.md?toc=/azure/azure-monitor/toc.json)
* [Azure Cloud Services](profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric](profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Azure sanal makineler ve sanal makine ölçek kümeleri](profiler-vm.md?toc=/azure/azure-monitor/toc.json)
* [**Önizleme** ASP.NET Core Azure Linux Web uygulamaları](profiler-aspnetcore-linux.md?toc=/azure/azure-monitor/toc.json) 

Profiler etkinleştirildi, ancak izlemeleri görmüyorsanız, kontrol bizim [sorun giderme kılavuzu](profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json).

## <a name="view-profiler-data"></a>Profiler verileri görüntüleme

İzlemeleri karşıya yüklemek Profiler için uygulamanızı etkin bir şekilde işleme isteklerinin olması gerekir. Bir deney yapıyorsanız, istekleri web uygulamanıza kullanarak oluşturabileceğiniz [Application Insights performans testi](https://docs.microsoft.com/vsts/load-test/app-service-web-app-performance-test). Yeni Profiler'ı etkinleştirdiyseniz, kısa bir yük testi çalıştırabilirsiniz. Yük testi çalışırken seçin **profili artık** düğmesini [ **Profiler ayarları** bölmesinde](profiler-settings.md#profiler-settings-pane). Profiler çalışırken rastgele yaklaşık saatte bir ve iki dakikalık bir süre boyunca profilleri. Uygulama istekleri gitmenize işliyorsa, Profiler izlemeleri saatte yükler.

Uygulamanız bazı trafik alır ve Profiler izlemeleri karşıya yüklemek için zaman bulana sonra görüntülemek üzere izlemesi olmalıdır. Bu işlem, 5-10 dakika sürebilir. Görüntülenecek izlemeler, buna **performans** bölmesinde **eylemleri**ve ardından **Profiler izlemeleri** düğmesi.

![Application Insights performans bölmesi Önizleme Profiler izlemeleri][performance-blade]

Bir istek yürütülürken harcanan süre kod düzeyinde dökümünü görüntülemek için bir örnek seçin.

![Application Insights izleme Gezgini][trace-explorer]

İzleme Gezgini, aşağıdaki bilgileri görüntüler:

* **Etkin yolu Göster**: Açılır en büyük düğüm yaprak veya en az bir şey kapatın. Çoğu durumda bu, bir performans sorununun düğümüdür.
* **Etiket**: İşlev veya olayın adı. Ağaç karışık kod ve, SQL gibi oluşan olaylar ve HTTP olayları görüntüler. Üst olay genel istek süresini temsil eder.
* **Geçen**: İşlem başlangıcı ve işleminin sonunu arasındaki zaman aralığı.
* **Zaman**: İşlev veya olay ilişkisinde diğer işlevlere çalıştığı süre.

## <a name="how-to-read-performance-data"></a>Performans verileri okuma

Microsoft Hizmet Profil Oluşturucu örnekleme yöntemleri ve araçları, uygulamanızın performansını çözümlemek için kullanır. Ayrıntılı koleksiyonu devam ederken, hizmet profil oluşturucu her makine CPU yönerge işaretçisini her bir milisaniyeden kısa örnekler. Her örnek, şu anda yürütülmekte olan iş parçacığının tam çağrı yığınını yakalar. Bu iş parçacığı, hem yüksek düzeyde hem de düşük düzeyde soyutlama neler hakkında ayrıntılı bilgi verir. Hizmet Profil Oluşturucu, aynı zamanda etkinlik bağıntısı ve nedensellik ilişkilerini bağlam olayları, görev paralel kitaplığı (TPL) olayları ve iş parçacığı havuzu olayları değiştirme dahil olmak üzere, izlemek için diğer olayları toplar.

Zaman Çizelgesi Görünümü'nde görüntülenen çağrı yığınını izleme ve örnekleme sonucudur. Her örnek, iş parçacığının tam çağrı yığınını yakaladığından, Microsoft .NET Framework ve başvuru diğer çerçeveler kod içerir.

### <a id="jitnewobj"></a>Nesne ayırma (clr! JIT\_yeni veya clr! JIT\_Newarr1)

**CLR! JIT\_yeni** ve **clr! JIT\_Newarr1** .NET Framework yönetilen yığından bellek ayırma yardımcı işlevleri. **CLR! JIT\_yeni** nesne ayrıldığında çağrılır. **CLR! JIT\_Newarr1** bir nesne dizisi ayrıldığında çağrılır. Bu iki işlev, genellikle hızlı ve süresi görece küçük miktarlarda gerçekleştirin. Varsa **clr! JIT\_yeni** veya **clr! JIT\_Newarr1** çok zaman zaman çizelgesi, kod içinde sürer birçok nesne ayırma ve önemli miktarda bellek kullanma.

### <a id="theprestub"></a>Kod (clr! yükleniyor ThePreStub)

**CLR! ThePreStub** .NET Framework'teki ilk kez yürütülecek kodu hazırlar yardımcı işlevdir. Bu yürütme genellikle içerir, ancak tam zamanında (JIT) derleme için sınırlı değildir. Her C# yöntemi **clr! ThePreStub** işlemi sırasında en fazla bir kez çağrılmalıdır.

Varsa **clr! ThePreStub** uzun sürüyorsa bir istek, istek için olan bu yöntemin ilk öğe. İlk yöntem yüklemek .NET Framework çalışma zamanı için zaman büyük/küçük harf önemlidir. Önce kullanıcılarınızın erişim veya Native Image Generator (ngen.exe) çalıştırmayı göz önünde bulundurun, derlemelerini söz konusu bölümü kod yürüten bir Isınma sürecinden göz önünde bulundurabilirsiniz.

### <a id="lockcontention"></a>Kilit çakışması (clr! JITutil\_MonContention veya clr! JITutil\_MonEnterWorker)

**CLR! JITutil\_MonContention** veya **clr! JITutil\_MonEnterWorker** geçerli iş parçacığının bir kilidi serbest bırakılması beklediğini belirtir. Yürüttüğünüzde genellikle bu metni görüntülenir bir C# **kilit** deyimi, çağırma **Monitor.Enter** yöntemi veya yöntem ile çağrılacak **Methodımploptions.Synchronized**özniteliği. Genellikle kilit çakışması oluşur, iş parçacığı _A_ kilit ve iş parçacığı edinme _B_ iş parçacığı önce aynı kilit almaya çalıştığında _A_ serbest.

### <a id="ngencold"></a>Kod ([SOĞUK]) yükleniyor

Yöntem adı içeriyorsa **[SOĞUK]**, gibi **mscorlib.ni! [ COLD]System.Reflection.CustomAttribute.IsDefined**, .NET Framework çalışma zamanı tarafından iyileştirilmedi ilk kez kod yürütüyordur [profil temelli iyileştirme](/cpp/build/profile-guided-optimizations). Her yöntem için en fazla bir kez işlemi sırasında görüntülenmesi gerekir.

Bir isteğin süresi önemli miktarda kod yükleniyor aldığı durumlarda yöntemi iyileştirilmemiş bölümünü yürütmek için birinci isteğidir. Kullanıcılarınızın erişim önce söz konusu bölümü kod yürüten bir Isınma işlem kullanmayı düşünün.

### <a id="httpclientsend"></a>HTTP isteği gönder

Yöntemler gibi **HttpClient.Send** kod tamamlanması için bir HTTP isteği beklediğini belirtir.

### <a id="sqlcommand"></a>Veritabanı işlemi

Yöntemler gibi **SqlCommand.Execute** kod veritabanı işlemin tamamlanmasını beklediğini belirtir.

### <a id="await"></a>Bekleniyor (AWAIT\_saat)

**AWAIT\_zaman** kod başka bir görevin tamamlanmasını beklediğini belirtir. Bu gecikme ile genellikle gerçekleşir C# **AWAIT** deyimi. Ne zaman yaptığını bir C# **AWAIT**, iş parçacığı geriye doğru alır ve denetimi iş parçacığı havuzuna döndürür ve bekleyen engellenmiş iş parçacığı yok **AWAIT** tamamlanması. Ancak, mantıksal olarak, iş parçacığı, yaptığınız **AWAIT** "engellenmiş" ve işlemin tamamlanmasını bekliyor. **AWAIT\_zaman** deyimi görevin sonlanmasını bekleyen engellenen süreyi gösterir.

### <a id="block"></a>Engellenme süresi

**BLOCKED_TIME** kullanılabilir olması başka bir kaynak kodu beklediğini belirtir. Örneğin, bir eşitleme nesnesi, kullanılabilir olması için bir iş parçacığı veya son isteği bekliyor olabilir.

### <a name="unmanaged-async"></a>Yönetilmeyen Asenkron

.NET framework ETW olayları yayar ve böylece iş parçacıkları arasında zaman uyumsuz çağrılar izlenebilir etkinlik kimlikleri iş parçacıkları arasında geçirir. Profil Oluşturucu hangi iş parçacığı ve iş parçacığında hangi işlevleriniz gerektirdikçe bildiremez şekilde yönetilmeyen kod (yerel kod için) ve bazı eski stil zaman uyumsuz kod bu olayları ve etkinlik kimliği eksik. Bu, çağrı yığınında 'Yönetilmeyen Async' etiketlenir. ETW dosyayı karşıdan yükleyin, kullanmanız mümkün olabilir [PerfView](https://github.com/Microsoft/perfview/blob/master/documentation/Downloading.md) neler olduğunu daha fazla bilgiler alın.

### <a id="cpu"></a>CPU süresi

CPU yönerge Yürütülüyor meşgul.

### <a id="disk"></a>Disk Zamanı

Disk işlemleri uygulama gerçekleştiriyor.

### <a id="network"></a>Ağ süresi

Uygulama ağ işlemlerini gerçekleştiriyor.

### <a id="when"></a>Zaman sütunu

**Olduğunda** bir düğüm için toplanan kapsamlı örnek zaman içinde nasıl farklılık, bir görselleştirme sütundur. Toplam istek aralığını 32 zaman demetlerin içine ayrılmıştır. Bu düğüm için kapsamlı örnekler 32 bu demetleri içinde toplanır. Her bir demete çubuğu olarak temsil edilir. Çubuk Yüksekliği ölçeklendirilmiş bir değeri temsil eder. İşaretlenmiş düğümleri için **CPU_TIME** veya **BLOCKED_TIME**, veya tüketen bir kaynağa (CPU, disk veya iş parçacığı gibi) için açık bir ilişkisi olduğu çubuğu tüketimi birini temsil eder Demet sırasında kaynaklar. Bu ölçümler için birden fazla kaynak tüketmeye tarafından yüzde 100'den büyük bir değer almak mümkündür. Örneğin, ortalama olarak, bir aralık boyunca iki CPU kullanırsanız yüzde 200 alın.

## <a name="limitations"></a>Sınırlamalar

Varsayılan veri saklama süresi, beş gündür. Gün başına alınan en fazla veri 10 GB'tır.

Profiler bu hizmeti kullanmak için herhangi bir ücreti yoktur. Bunu kullanmak için web uygulamanızı olmalıdır en az temel katmanı, Azure App Service'in Web Apps özelliğinde barındırılan.

## <a name="overhead-and-sampling-algorithm"></a>Ek yükü ve örnekleme algoritması

Profiler rastgele Profiler izlemeleri yakalama için etkinleştirilmiş olan bir uygulama her sanal makinede iki dakika başı çalıştırır. Profiler çalıştırırken sunucu yüzde 15 CPU yükü 5'ten ekler.

## <a name="next-steps"></a>Sonraki adımlar
Application Insights Profiler, Azure uygulamanız için etkinleştirin. Ayrıca bkz:
* [Uygulama Hizmetleri](profiler.md?toc=/azure/azure-monitor/toc.json)
* [Azure Cloud Services](profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric](profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Azure sanal makineler ve sanal makine ölçek kümeleri](profiler-vm.md?toc=/azure/azure-monitor/toc.json)


[performance-blade]: ./media/profiler-overview/performance-blade-v2-examples.png
[trace-explorer]: ./media/profiler-overview/trace-explorer.png
