---
title: "Performans sayaçları Azure Tanılama'kullanma | Microsoft Docs"
description: "Azure bulut Hizmetleri ya da sanal makineyi performans sayaçları engelleri bulun ve performansı ayarlamak için kullanın."
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: tysonn
ms.assetid: fc4c61e9-d49d-4ed9-a32c-b91cdf851909
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/29/2016
ms.author: robb
ms.openlocfilehash: 2cf765cb034725199127c547a9b8b997a4a6089c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-use-performance-counters-in-an-azure-application"></a>Oluşturma ve bir Azure uygulamasında performans sayaçlarını kullanma
Bu makalede, avantajları ve Azure uygulamanıza performans sayaçlarını yerleştirme açıklanır. Verileri toplamak için performans sorunlarını bulup sistem ve uygulama performansı ayarlamak için bunları kullanabilirsiniz.

Windows Server, IIS ve ASP.NET için kullanılabilen performans sayaçlarının de toplanır ve Azure web rolleri, çalışan rolleri ve sanal makinelerin durumunu belirlemek için kullanılır. Ayrıca, oluşturabilir ve özel performans sayaçları kullanın.  

Performans sayacı verilerini inceleyebilirsiniz

1. Doğrudan uygulama konakta Uzak Masaüstü kullanarak erişilen Performans İzleyicisi aracıyla
2. Azure Yönetim Paketi System Center Operations Manager ile kullanma
3. Tanılama veri erişim diğer izleme araçları ile Azure depolama alanına aktarılır. Bkz: [deposu ve görünüm tanılama verilerini Azure storage'da](https://msdn.microsoft.com/library/azure/hh411534.aspx) daha fazla bilgi için.  

Uygulamanızda performansını izleme hakkında daha fazla bilgi için [Azure portal](http://portal.azure.com/), bkz: [İzleyici bulut hizmetlerini](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/).

Bir günlüğe kaydetme ve stratejisi izleme ve sorunlarını gidermek ve Azure uygulamalarını en iyi duruma getirmek için tanılama ve başka teknikler kullanarak oluşturma hakkında ek ayrıntılı yönergeler için bkz: [Azure uygulamaları geliştirmek için en iyi uygulamaları sorunlarını giderme](https://msdn.microsoft.com/library/azure/hh771389.aspx).

## <a name="enable-performance-counter-monitoring"></a>Performans sayacı izlemeyi etkinleştir
Performans sayaçları varsayılan olarak etkin değildir. Uygulamanızın veya bir başlangıç görevi, her bir rol örneği için izlemek istediğiniz belirli performans sayaçları dahil etmek için varsayılan Tanılama Aracı yapılandırmasını değiştirmeniz gerekir.

### <a name="performance-counters-available-for-microsoft-azure"></a>Microsoft Azure için kullanılabilen performans sayaçlarının
Azure, Windows Server, IIS ve ASP.NET yığını için kullanılabilen performans sayaçlarının kümesini sağlar. Aşağıdaki tabloda bazı özellikle ilgisini çeken Azure uygulamaları için performans sayaçları listeler.

| Sayacı Kategori: Nesnesi (örneği) | Sayaç adı | Başvuru |
| --- | --- | --- |
| .NET CLR özel durumları (*genel*) |# Durum / sn |Özel durum performans sayaçları |
| .NET CLR bellek (*genel*) |GC % zaman |Bellek performansı sayaçları |
| ASP.NET |Uygulama yeniden başlatma |ASP.NET için performans sayaçları |
| ASP.NET |İstek Yürütme Süresi |ASP.NET için performans sayaçları |
| ASP.NET |Bağlantısı kesilen istek sayısı |ASP.NET için performans sayaçları |
| ASP.NET |Çalışan işlemi yeniden başlatılır |ASP.NET için performans sayaçları |
| ASP.NET uygulamaları (**toplam**) |Toplam istek sayısı |ASP.NET için performans sayaçları |
| ASP.NET uygulamaları (**toplam**) |İsteği/sn |ASP.NET için performans sayaçları |
| ASP.NET v4.0.30319 |İstek Yürütme Süresi |ASP.NET için performans sayaçları |
| ASP.NET v4.0.30319 |İstek Bekleme süresi |ASP.NET için performans sayaçları |
| ASP.NET v4.0.30319 |İstek geçerli |ASP.NET için performans sayaçları |
| ASP.NET v4.0.30319 |Sıraya alınan istek sayısı |ASP.NET için performans sayaçları |
| ASP.NET v4.0.30319 |Reddedilen istek sayısı |ASP.NET için performans sayaçları |
| Bellek |Kullanılabilir MBayt |Bellek performansı sayaçları |
| Bellek |Kaydedilmiş Bayt |Bellek performansı sayaçları |
| İşlemci(_Toplam) |% İşlemci zamanı |ASP.NET için performans sayaçları |
| TCPv4 |Bağlantı hataları |TCP nesnesi |
| TCPv4 |Kurulan bağlantılar |TCP nesnesi |
| TCPv4 |Bağlantıları Sıfırla |TCP nesnesi |
| TCPv4 |Kesim gönderilen/sn |TCP nesnesi |
| Ağ Interface(*) |Alınan Bayt/sn |Ağ arabirimi nesnesi |
| Ağ Interface(*) |Gönderilen bayt/sn |Ağ arabirimi nesnesi |
| Ağ arabirimi (Microsoft sanal makine veri yolu ağ bağdaştırıcısı _2) |Alınan Bayt/sn |Ağ arabirimi nesnesi |
| Ağ arabirimi (Microsoft sanal makine veri yolu ağ bağdaştırıcısı _2) |Gönderilen bayt/sn |Ağ arabirimi nesnesi |
| Ağ arabirimi (Microsoft sanal makine veri yolu ağ bağdaştırıcısı _2) |Toplam Bayt/sn |Ağ arabirimi nesnesi |

## <a name="create-and-add-custom-performance-counters-to-your-application"></a>Oluşturun ve uygulamanıza özel performans sayaçları ekleyin
Azure oluşturmak ve web rolleri ve çalışan rolleri için özel performans sayaçları değiştirmek için desteğe sahiptir. Sayaç, izleme ve uygulamaya özgü davranışı izlemek için kullanılabilir. Oluşturun ve bir başlangıç görevi, web rolü ve çalışan rolü yükseltilmiş izinleri olan özel performans sayacı kategorileri ve tanımlayıcıları silin.

> [!NOTE]
> Özel performans sayaçları için değişiklikleri yapar kodu çalıştırmak için yükseltilmiş izinleri olan gerekir. Kodu bir web rolü veya çalışan rolü ise, rol etiketi içermelidir <Runtime executionContext="elevated" /> ServiceDefinition.csdef dosyasında rolü düzgün başlatılamadı.
>
>

Özel performans sayacı verilerini Azure depolama Tanılama aracı kullanarak gönderebilirsiniz.

Standart performans sayacı verilerini Azure işlemler tarafından oluşturulur. Özel performans sayacı verilerini, web rolü ya da çalışan rolü uygulamanız tarafından oluşturulmuş olması gerekir. Bkz: [performans sayacı türleri](https://msdn.microsoft.com/library/z573042h.aspx) özel performans sayaçları depolanan veri türleri hakkında bilgi için. Bkz: [performans sayaçları örnek](http://code.msdn.microsoft.com/azure/) oluşturur ve bir web rolünde özel performans sayacı verilerini ayarlayan bir örnek.

## <a name="store-and-view-performance-counter-data"></a>Depolama ve görünüm performans sayacı verileri
Azure tanılama diğer bilgilerle performans sayacı verileri önbelleğe alır. Bu veriler, Performans İzleyicisi gibi araçları görüntülemek üzere Uzak Masaüstü erişimi kullanarak rol örneği çalışırken, Uzaktan izleme için kullanılabilir. Rol örneği dışında veri kalıcı hale getirmek için tanılama aracı verileri Azure depolama alanına aktarmanız gerekir. Önbelleğe alınan performans sayacı verilerini boyutunda Tanılama Aracı yapılandırılabilir veya tüm Tanılama verileri için paylaşılan bir sınır parçası olarak yapılandırılabilir. Arabellek boyutu ayarlama hakkında daha fazla bilgi için bkz: [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) ve [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx). Bkz: [deposu ve görünüm tanılama verilerini Azure storage'da](https://msdn.microsoft.com/library/azure/hh411534.aspx) bir depolama hesabı için veri aktarmak için tanılama aracınızı ayarlama genel bir bakış için.

Her yapılandırılmış performans sayacı örneği belirtilen örnekleme hızında kaydedilir ve örneklenen verileri depolama hesabına zamanlanmış aktarım isteği veya isteğe bağlı aktarım isteği aktarılır. Otomatik aktarımları olarak dakikada bir kez sıklıkta zamanlanabilir. Performans sayacı verilerini Tanılama aracı tarafından aktarılan bir tablo, WADPerformanceCountersTable, depolama hesabında depolanır. Bu tablo erişilen ve standart Azure depolama API yöntemleriyle sorgulanan. Bkz: [Microsoft Azure performans sayaçları örnek](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) sorgulama ve performans sayacı verilerini WADPerformanceCountersTable tablosundan görüntüleyen bir örnek.

> [!NOTE]
> Tanılama Aracı aktarımı sıklık ve sıra gecikme bağlı olarak, en son performans sayacı verilerini depolama hesabındaki güncel birkaç dakika olabilir.
>
>

## <a name="enable-performance-counters-using-diagnostics-configuration-file"></a>Tanılama yapılandırma dosyası kullanarak performans sayaçları sağlar
Performans sayaçları Azure uygulamanızı etkinleştirmek için aşağıdaki yordamı kullanın.

## <a name="prerequisites"></a>Ön koşullar
Bu bölümde, uygulamanıza Tanılama izleme alınan ve tanılama yapılandırma dosyası, Visual Studio çözümünüz (diagnostics.wadcfg SDK 2.4 ve altı veya diagnostics.wadcfgx SDK 2.5 ve üstü) eklenen varsayar. Adım 1 ve 2'de bkz [Azure Cloud Services ve sanal makineleri etkinleştirme tanılamada](cloud-services-dotnet-diagnostics.md)) daha fazla bilgi için.

## <a name="step-1-collect-and-store-data-from-performance-counters"></a>1. adım: Toplamak ve performans sayaçlarını veri depolama
Visual Studio çözümünüzü tanılama dosya ekledikten sonra bir Azure uygulamasında toplama ve performans sayacı verilerinin depolanması yapılandırabilirsiniz. Bu tanılama dosyasına performans sayaçlarını ekleyerek yapılır. Tanılama verilerini, performans sayaçları dahil olmak üzere, ilk örneğinde toplanır. Ayrıca, uygulamanızda depolama hesabı belirtmeniz gerekir böylece verileri Azure tablo hizmeti WADPerformanceCountersTable tablosuna kalıcıdır. Uygulamanızı yerel olarak işlem Öykünücüde test ettiğiniz, aynı zamanda tanılama verilerini yerel olarak depolama öykünücüsünde saklayabilirsiniz. Tanılama verileri depolamak önce ilk gitmeniz gerekir [Azure portal](http://portal.azure.com/) ve klasik depolama hesabı oluşturun. Depolama hesabınız aynı coğrafi konum olarak Azure uygulamanızı bulun en iyi bir uygulamadır. Azure uygulama ve depolama hesabı aynı coğrafi-konumdaysa tutma tarafından harici bant genişliği maliyetlerini ödeme yapmaktan kaçınmak ve gecikme süresini azaltmak.

### <a name="add-performance-counters-to-the-diagnostics-file"></a>Performans sayaçları tanılama dosyasına ekleyin
Kullanabileceğiniz birçok sayaç vardır. Aşağıdaki örnek, web ve çalışan rolü izleme için önerilen birkaç performans sayaçlarını gösterir.

Tanılama dosyasını (diagnostics.wadcfg SDK 2.4 ve altı veya diagnostics.wadcfgx SDK 2.5 ve üzeri) açın ve aşağıdaki DiagnosticMonitorConfiguration öğesine ekleyin:

```xml
<PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
    <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT30S" />

    <!-- Use the Process(w3wp) category counters in a web role -->

    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\% Processor Time" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Private Bytes" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Thread Count" sampleRate="PT30S" />

    <!-- Use the Process(WaWorkerHost) category counters in a worker role.
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\% Processor Time" sampleRate="PT30S" />
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Private Bytes" sampleRate="PT30S" />
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Thread Count" sampleRate="PT30S" />
    -->

    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Interop(_Global_)\# of marshalling" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Loading(_Global_)\% Time Loading" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR LocksAndThreads(_Global_)\Contention Rate / sec" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Memory(_Global_)\# Bytes in all Heaps" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Networking(_Global_)\Connections Established" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Remoting(_Global_)\Remote Calls/sec" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Jit(_Global_)\% Time in Jit" sampleRate="PT30S" />
</PerformanceCounters>
```

BufferQuotaInMB özniteliği (Azure günlükleri, IIS günlükleri, vb.) veri toplama türü için kullanılabilir olan dosya sistemi depolama alanının üst sınırını belirtir. Varsayılan değer 0'dır. Kotasına ulaşıldığında, yeni veriler eklendikçe eski veriler silinir. Tüm bufferQuotaInMB özellikleri toplamını OverallQuotaInMB öznitelik değerinden büyük olmalıdır. Ne kadar depolama tanılama verilerini toplama için gerekli olacak belirleme daha ayrıntılı bilgi için Kurulum WAD bölümüne bakın [Azure uygulamaları geliştirmek için en iyi uygulamaları sorunlarını giderme](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx).

Zamanlanmış veri aktarımlarını arasındaki aralığı belirtir, scheduledTransferPeriod özniteliği için en yakın dakika yuvarlanan. Aşağıdaki örneklerde, onu (30 dakika) PT30M için ayarlanır. Transfer süresi 1 dakika gibi küçük bir değere ayarlamak üretimde uygulamanızın performansını olumsuz yönde etkileyen ancak test hızlı bir şekilde çalışmaya görme tanılama için yararlı olabilir. Zamanlanmış transfer süresi tanılama veri örneğinde büyüklükte uygulamanızın performansını etkilemez ancak yazılmaz emin olmak için küçük olmalıdır.

CounterSpecifier özniteliği toplamak için performans sayacı belirtir. SampleRate özniteliği, performans sayacı, bu durumda 30 saniye örneklenen hızını belirtir.

Toplamak istediğiniz performans sayaçlarını ekledikten sonra yaptığınız değişiklikleri tanılama dosyasına kaydedin. Ardından, tanılama verilerini için kalıcı depolama hesabı belirtmeniz gerekir.

### <a name="specify-the-storage-account"></a>Depolama hesabı belirtin
Tanılama bilgilerinizi Azure depolama hesabınıza kalıcı hale getirmek için hizmet yapılandırma (ServiceConfiguration.cscfg) dosyasında bir bağlantı dizesi belirtmeniz gerekir.

Azure SDK 2.5 için depolama hesabı diagnostics.wadcfgx dosyasında belirtilebilir.

> [!NOTE]
> Bu yönergeler yalnızca Azure SDK 2.4 ve altında geçerlidir. Azure SDK 2.5 için depolama hesabı diagnostics.wadcfgx dosyasında belirtilebilir.
>
>

Bağlantı dizeleri ayarlamak için:

1. Sık kullandığınız metin düzenleyiciyi kullanarak ServiceConfiguration.Cloud.cscfg dosyasını açın ve depolama için bağlantı dizesini ayarlayın. *AccountName* ve *AccountKey* değerleri depolama hesabı panosundaki erişim tuşları altında Azure portalında bulundu.

    ```xml
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>"/>
    </ConfigurationSettings>
    ```
2. ServiceConfiguration.Cloud.cscfg dosyasını kaydedin.
3. ServiceConfiguration.Local.cscfg dosyasını açın ve UseDevelopmentStorage olarak ayarlandığını doğrulayın true.

    ```xml
    <ConfigurationSettings>
      <Settingname="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true"/>
    </ConfigurationSettings>
    ```
   Bağlantı dizelerini ayarlayın, uygulamanızın dağıtıldığında, uygulamanızın tanılama verilerini depolama hesabınıza korunur.
4. Kaydet ve projenizi yapılandırın ve ardından uygulamanızı dağıtın.

## <a name="step-2-optional-create-custom-performance-counters"></a>2. adım: (İsteğe bağlı) oluşturma özel performans sayaçları
Önceden tanımlanmış performans sayaçları ek olarak, web veya çalışan rolleri izlemek için kendi özel performans sayaçları ekleyebilirsiniz. Özel performans sayaçları, izleme ve uygulamaya özgü davranışı izlemek ve oluşturulabilir veya bir başlangıç görevi, web rolü ya da yükseltilmiş izinleri olan çalışan rolü silinmiş için kullanılabilir.

Azure Tanılama Aracı performans sayacı yapılandırması başlattıktan bir dakika .wadcfg dosyasından yeniler.  Azure Tanılama Aracı bunları yüklemeye çalıştığında OnStart yönteminde özel performans sayaçları oluşturursanız ve başlangıç görevleri çalıştırmak için bir dakikadan daha uzun sürer, özel performans sayaçları oluşturulmadı.  Bu senaryoda, Azure tanılama doğru özel performans sayaçları dışındaki tüm tanılama verilerini yakalar görürsünüz.  Bu sorunu çözmek için performans sayaçlarını bir başlangıç görevi oluşturmak veya performans sayaçlarını oluşturduktan sonra başlangıç görevi bazıları OnStart yöntemi iş taşıyın.

Sayaç "\MyCustomCounterCategory\MyButton1Counter" adlı basit bir özel performans oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Uygulamanız için hizmet tanımı dosyası (CSDEF) açın.
2. Çalışma zamanı WebRole için yükseltilmiş ayrıcalıklarla yürütülmesine izin vermek için örn öğesi veya ekleyin:

    ```xml
    <runtime executioncontext="elevated"/>
    ```
3. Dosyayı kaydedin.
4. Tanılama dosyasını (diagnostics.wadcfg SDK 2.4 ve altı veya diagnostics.wadcfgx SDK 2.5 ve üzeri) açın ve aşağıdaki DiagnosticMonitorConfiguration ekleyin

    ```xml
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
      <PerformanceCounterConfiguration counterSpecifier="\MyCustomCounterCategory\MyButton1Counter" sampleRate="PT30S"/>
    </PerformanceCounters>
    ```
5. Dosyayı kaydedin.
6. Özel performans sayacı kategorisi temel çağırmadan önce rolü OnStart yönteminde oluşturun. OnStart. Aşağıdaki C# örnek, zaten yoksa, özel bir kategori oluşturur:

    ```csharp
    public override bool OnStart()
    {
      if (!PerformanceCounterCategory.Exists("MyCustomCounterCategory"))
      {
         CounterCreationDataCollection counterCollection = new CounterCreationDataCollection();

         // add a counter tracking user button1 clicks
         CounterCreationData operationTotal1 = new CounterCreationData();
         operationTotal1.CounterName = "MyButton1Counter";
         operationTotal1.CounterHelp = "My Custom Counter for Button1";
         operationTotal1.CounterType = PerformanceCounterType.NumberOfItems32;
         counterCollection.Add(operationTotal1);

         PerformanceCounterCategory.Create(
           "MyCustomCounterCategory",
           "My Custom Counter Category",
           PerformanceCounterCategoryType.SingleInstance, counterCollection);

         Trace.WriteLine("Custom counter category created.");
      }
      else {
        Trace.WriteLine("Custom counter category already exists.");
      }

    return base.OnStart();
    }
    ```
7. Sayaçlar, uygulamanızda güncelleştirin. Aşağıdaki örnekte bir özel performans sayacı Button1_Click olayları güncelleştirir:

    ```csharp
    protected void Button1_Click(object sender, EventArgs e)
    {
      PerformanceCounter button1Counter = new PerformanceCounter(
        "MyCustomCounterCategory",
        "MyButton1Counter",
        string.Empty,
        false);
      button1Counter.Increment();
      this.Button1.Text = "Button 1 count: " +
        button1Counter.RawValue.ToString();
    }
    ```
8. Dosyayı kaydedin.  

Özel performans sayacı verilerini artık Azure tanılama İzleyici tarafından toplanacaktır.

## <a name="step-3-query-performance-counter-data"></a>3. adım: performans sayacı verileri Sorgulama
Uygulamanız dağıtıldıktan sonra çalışmaya, Tanılama izleme başlayacak performans sayaçlarını toplama ve bu verileri Azure depolama kalıcı yapma. Visual Studio Araçları Sunucu Gezgini gibi kullandığınız [Azure Storage Gezgini](http://azurestorageexplorer.codeplex.com/), veya [Azure tanılama Manager](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) görüntülemek için Cerebrata tarafından WADPerformanceCountersTable tablodaki verileri performans sayaçları. Tablo hizmetini kullanarak program aracılığıyla da sorgulayabilirsiniz [C#](../cosmos-db/table-storage-how-to-use-dotnet.md), [Java](../cosmos-db/table-storage-how-to-use-java.md), [Node.js](../cosmos-db/table-storage-how-to-use-nodejs.md), [Python](../cosmos-db/table-storage-how-to-use-python.md), [Ruby](../cosmos-db/table-storage-how-to-use-ruby.md), veya [PHP](../cosmos-db/table-storage-how-to-use-php.md).

Aşağıdaki C# örnek WADPerformanceCountersTable tabloda temel bir sorgu gösterir ve tanılama verilerini bir CSV dosyasına kaydeder. Performans sayaçlarını bir CSV dosyasına kaydedildikten sonra verileri görselleştirmek için Microsoft Excel veya başka bir aracı grafik özelliklerini kullanabilirsiniz. Ekim 2012 .NET için Azure SDK'sındaki ve sonraki sürümlerinde olduğu Microsoft.WindowsAzure.Storage.dll, başvuru eklediğinizden emin olun. Derleme % Program Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version-num\ref\ dizinine yüklenir.

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Table;
...

// Get the connection string. When using Microsoft Azure Cloud Services, it is recommended
// you store your connection string using the Microsoft Azure service configuration
// system (*.csdef and *.cscfg files). You can you use the CloudConfigurationManager type
// to retrieve your storage connection string.  If you're not using Cloud Services, it's
// recommended that you store the connection string in your web.config or app.config file.
// Use the ConfigurationManager type to retrieve your storage connection string.

string connectionString = Microsoft.WindowsAzure.CloudConfigurationManager.GetSetting("StorageConnectionString");
//string connectionString = System.Configuration.ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString;

// Get a reference to the storage account using the connection string.  You can also use the development
// storage account (Storage Emulator) for local debugging.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
//CloudStorageAccount storageAccount = CloudStorageAccount.DevelopmentStorageAccount;

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "WADPerformanceCountersTable" table.
CloudTable table = tableClient.GetTableReference("WADPerformanceCountersTable");

// Create the table query, filter on a specific CounterName, DeploymentId and RoleInstance.
TableQuery<PerformanceCountersEntity> query = new TableQuery<PerformanceCountersEntity>()
  .Where(
    TableQuery.CombineFilters(
      TableQuery.GenerateFilterCondition("CounterName", QueryComparisons.Equal, @"\Processor(_Total)\% Processor Time"),
      TableOperators.And,
      TableQuery.CombineFilters(
      TableQuery.GenerateFilterCondition("DeploymentId", QueryComparisons.Equal, "ec26b7a1720447e1bcdeefc41c4892a3"),
      TableOperators.And,
      TableQuery.GenerateFilterCondition("RoleInstance", QueryComparisons.Equal, "WebRole1_IN_0")
    )
  )
);

// Execute the table query.
IEnumerable<PerformanceCountersEntity> result = table.ExecuteQuery(query);

// Process the query results and build a CSV file.
StringBuilder sb = new StringBuilder("TimeStamp,EventTickCount,DeploymentId,Role,RoleInstance,CounterName,CounterValue\n");

foreach (PerformanceCountersEntity entity in result)
{
  sb.Append(entity.Timestamp + "," + entity.EventTickCount + "," + entity.DeploymentId + ","
    + entity.Role + "," + entity.RoleInstance + "," + entity.CounterName + "," + entity.CounterValue+"\n");
}

StreamWriter sw = File.CreateText(@"C:\temp\PerfCounters.csv");
sw.Write(sb.ToString());
sw.Close();
```

Varlıkları eşleme türetilen özel bir sınıf kullanarak C# nesnelere **TableEntity**. Aşağıdaki kod bir performans sayacının temsil eden bir varlık sınıfı tanımlar **WADPerformanceCountersTable** tablo.

```csharp
public class PerformanceCountersEntity : TableEntity
{
  public long EventTickCount { get; set; }
  public string DeploymentId { get; set; }
  public string Role { get; set; }
  public string RoleInstance { get; set; }
  public string CounterName { get; set; }
  public double CounterValue { get; set; }
}
```

## <a name="next-steps"></a>Sonraki Adımlar
[Azure tanılama üzerinde ek makaleleri görüntüleme](../azure-diagnostics.md)
