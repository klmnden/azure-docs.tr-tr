---
title: Azure bulut Hizmetleri'nde performans sayaçları toplamak | Microsoft Docs
description: Bulma, kullanma ve bulut hizmetlerinde Azure Diagnostics ve Application Insights ile performans sayaçları oluşturma hakkında bilgi edinin.
services: cloud-services
documentationcenter: .net
author: jpconnock
manager: timlt
editor: ''
ms.assetid: ''
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2018
ms.author: jeconnoc
ms.openlocfilehash: 68101be211335d51eb4bf99361ea36b73fa19218
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58485416"
---
# <a name="collect-performance-counters-for-your-azure-cloud-service"></a>Azure bulut hizmetiniz için performans sayaçlarını Topla

Performans sayaçları, uygulamanızı ve ana bilgisayarın ne kadar iyi gerçekleştiriyorsanız izlemek bir yol sağlar. Windows Server donanım, uygulamalar, işletim sistemi ve daha fazla bilgi için ilgili pek çok farklı performans sayaçları sağlar. Toplama ve performans sayaçları Azure'a göndermek için daha iyi kararlar vermemize yardımcı olmak için bu bilgileri analiz edebilirsiniz. 

## <a name="discover-available-counters"></a>Kullanılabilir sayaçları keşfedin

Performans sayaç kümesi adı (kategori olarak da bilinir) ve bir veya daha fazla sayaç olmak üzere iki bölümden oluşur. Performans sayaçlarının bir listesini almak için PowerShell kullanabilirsiniz:

```powershell
Get-Counter -ListSet * | Select-Object CounterSetName, Paths | Sort-Object CounterSetName

CounterSetName                                  Paths
--------------                                  -----
.NET CLR Data                                   {\.NET CLR Data(*)\SqlClient...
.NET CLR Exceptions                             {\.NET CLR Exceptions(*)\# o...
.NET CLR Interop                                {\.NET CLR Interop(*)\# of C...
.NET CLR Jit                                    {\.NET CLR Jit(*)\# of Metho...
.NET Data Provider for Oracle                   {\.NET Data Provider for Ora...
.NET Data Provider for SqlServer                {\.NET Data Provider for Sql...
.NET Memory Cache 4.0                           {\.NET Memory Cache 4.0(*)\C...
AppV Client Streamed Data Percentage            {\AppV Client Streamed Data ...
ASP.NET                                         {\ASP.NET\Application Restar...
ASP.NET Apps v4.0.30319                         {\ASP.NET Apps v4.0.30319(*)...
ASP.NET State Service                           {\ASP.NET State Service\Stat...
ASP.NET v2.0.50727                              {\ASP.NET v2.0.50727\Applica...
ASP.NET v4.0.30319                              {\ASP.NET v4.0.30319\Applica...
Authorization Manager Applications              {\Authorization Manager Appl...

#... results cut to save space ...
```

`CounterSetName` Özellik kümesi (veya kategori) temsil eder ve performans sayaçları için ilişkili iyi göstergedir. `Paths` Özelliğini sayaç kümesi için bir koleksiyonunu temsil eder. Ayrıca Al `Description` özelliği sayaç kümesi hakkında daha fazla bilgi için.

Tüm sayaçları için bir kümesi almak için kullanın `CounterSetName` genişletin ve değer `Paths` koleksiyonu. Her yol öğesi Sorgulayabileceğiniz bir sayacıdır. Örneğin, ilgili kullanılabilir sayaçları almak için `Processor` ayarlayın, genişletme `Paths` koleksiyonu:

```powershell
Get-Counter -ListSet * | Where-Object CounterSetName -eq "Processor" | Select -ExpandProperty Paths

\Processor(*)\% Processor Time
\Processor(*)\% User Time
\Processor(*)\% Privileged Time
\Processor(*)\Interrupts/sec
\Processor(*)\% DPC Time
\Processor(*)\% Interrupt Time
\Processor(*)\DPCs Queued/sec
\Processor(*)\DPC Rate
\Processor(*)\% Idle Time
\Processor(*)\% C1 Time
\Processor(*)\% C2 Time
\Processor(*)\% C3 Time
\Processor(*)\C1 Transitions/sec
\Processor(*)\C2 Transitions/sec
\Processor(*)\C3 Transitions/sec
```

Bu ayrı ayrı sayaç yolları için tanılama çerçevesi eklenebilir, bulut hizmetinizin kullanır. Nasıl bir performans sayacı yol oluşturulan hakkında daha fazla bilgi için bkz. [sayaç yolunu belirterek](https://msdn.microsoft.com/library/windows/desktop/aa373193(v=vs.85)).

## <a name="collect-a-performance-counter"></a>Bir performans sayacı Topla

Bulut hizmetiniz Azure Tanılama veya Application Insights için bir performans sayacı eklenebilir.

### <a name="application-insights"></a>Application Insights

Bulut Hizmetleri için Azure Application Insights, belirttiğiniz toplamak istediğiniz hangi performans sayaçlarını sağlar. Çalıştırdıktan sonra [projenize Application Insights ekleme](../azure-monitor/app/cloudservices.md#sdk), adlı bir yapılandırma dosyası **Applicationınsights.config** Visual Studio projenize eklenir. Bu yapılandırma dosyası, hangi türde bilgileri Application ınsights'ı toplar ve Azure'a gönderilmeden tanımlar.

Açık **Applicationınsights.config** dosya ve bulma **Applicationınsights** > **TelemetryModules** öğesi. Her `<Add>` alt öğesi yapılandırmasıyla birlikte toplanacak telemetri türü tanımlar. Performans sayacı telemetri modülü türü `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector`. Bu öğe zaten tanımlanmış olması durumunda, ikinci kez eklemeyin. Adlı bir düğüm altında her performans sayacı toplamak için tanımlanan `<Counters>`. Sürücü performans sayaçlarını toplayan bir örnek aşağıda verilmiştir:

```xml
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">

  <TelemetryModules>

    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <Add PerformanceCounter="\LogicalDisk(C:)\Disk Write Bytes/sec" ReportAs="Disk write (C:)" />
        <Add PerformanceCounter="\LogicalDisk(C:)\Disk Read Bytes/sec" ReportAs="Disk read (C:)" />
      </Counters>
    </Add>

  </TelemetryModules>

<!-- ... cut to save space ... -->
```

Her performans sayacı olarak temsil edilen bir `<Add>` öğesi altında `<Counters>`. `PerformanceCounter` Özniteliği toplamak için performans sayacı tanımlar. `ReportAs` Özniteliktir performans sayacı için Azure Portalı'nda görüntülenecek başlığı. Topladığınız herhangi bir performans sayacı adlı bir kategori yerleştirilir **özel** portalında. Azure Tanılama, bu performans sayaçlarını toplanır ve gönderilen aralığı ayarlanamıyor. Application Insights ile performans sayaçları toplanır ve dakikada gönderilen. 

Application Insights, aşağıdaki performans sayaçları otomatik olarak toplar:

* \Process(??APP_WIN32_PROC??)\% İşlemci Süresi
* \Memory\Available Bytes
* \.NET CLR Exceptions(??APP_CLR_PROC??)\# of Exceps Thrown / sec
* \Process(??APP_WIN32_PROC??)\Private Bytes
* \Process(??APP_WIN32_PROC??)\IO Data Bytes/sec
* \Processor(_Total)\% Processor Time

Daha fazla bilgi için [sistem performans sayaçları Application ınsights'ta](../azure-monitor/app/performance-counters.md) ve [Azure Cloud Services için Application Insights](../azure-monitor/app/cloudservices.md#performance-counters).

### <a name="azure-diagnostics"></a>Azure Tanılama

> [!IMPORTANT]
> Tüm bu veriler depolama hesabında toplanır, ancak portalda mu **değil** verilerin grafiğini oluşturmak için yerel bir yol sağlar. Uygulamanıza Application Insights gibi başka bir Tanılama Hizmeti Tümleştirme önemle tavsiye edilir.

Bulut Hizmetleri için Azure tanılama uzantısı, toplamak istediğiniz hangi performans sayaçlarını belirttiğiniz sağlar. Azure Tanılama'yı ayarlamak için bkz [bulut hizmeti izleme genel görünümü](cloud-services-how-to-monitor.md#setup-diagnostics-extension).

Toplamak istediğiniz performans sayaçlarını tanımlanan **diagnostics.wadcfgx** dosya. Visual Studio'da (tanımlanmış olduğu her rolü) bu dosyayı açın ve bulun **DiagnosticsConfiguration** > **PublicConfig** > **WadCfg**  >  **DiagnosticMonitorConfiguration** > **PerformanceCounters** öğesi. Yeni bir **PerformanceCounterConfiguration** alt öğesi olarak. Bu öğe iki öznitelikleri: `counterSpecifier` ve `sampleRate`. `counterSpecifier` Özniteliği, hangi sistem performansı sayaç (önceki bölümde belirtilen) toplayacak tanımlar. `sampleRate` Değeri bu değer ne sıklıkta sorgulandığı zamanı gösterir. Bir bütün olarak göre üst Azure'a tüm performans sayaçlarını aktarılan `PerformanceCounters` öğenin `scheduledTransferPeriod` öznitelik değeri.

Hakkında daha fazla bilgi için `PerformanceCounters` şema öğesi bkz [Azure tanılama şeması](../azure-monitor/platform/diagnostics-extension-schema-1dot3.md#performancecounters-element).

Tarafından tanımlanan süre `sampleRate` XML süre veri türü ne sıklıkla performans sayacı sorgulandığı zamanı belirtmek için özniteliği kullanır. Aşağıdaki örnekte, oranını ayarlamak `PT3M`, yani `[P]eriod[T]ime[3][M]inutes`: üç dakikada bir.

Nasıl hakkında daha fazla bilgi için `sampleRate` ve `scheduledTransferPeriod` olan tanımlanan, bkz: **süresi veri türü** konusundaki [W3 XML tarih ve saat tarih türleri](https://www.w3schools.com/XML/schema_dtypes_date.asp) öğretici.

```xml
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration  xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig>
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096">

        <!-- ... cut to save space ... -->

        <PerformanceCounters scheduledTransferPeriod="PT1M">
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />

          <!-- This is a new perf counter which will track the C: disk read activity in bytes per second, every minute -->
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(C:)\Disk Read Bytes/sec" sampleRate="PT1M" />

        </PerformanceCounters>
      </DiagnosticMonitorConfiguration>
    </WadCfg>
    
    <!-- ... cut to save space ... -->

  </PublicConfig>
</DiagnosticsConfiguration>
```

## <a name="create-a-new-perf-counter"></a>Yeni bir performans sayacı oluşturun

Yeni bir performans sayacı, oluşturulur ve kodunuz tarafından kullanılır. Kodunuzu yeni bir performans sayacı oluşturur, aksi takdirde işlem başarısız olur, yükseltilmiş çalıştırılması gerekir. Bulut hizmetinizin `OnStart` başlatma kodunu, yükseltilmiş bir bağlamda rolünü çalıştırmak zorunlu kılma, performans sayacı oluşturabilirsiniz. Veya yükseltilmiş çalıştırır ve performans sayacı oluşturan bir başlangıç görevi oluşturabilirsiniz. Başlangıç görevleri hakkında daha fazla bilgi için bkz: [nasıl yapılandırılacağı ve bir bulut hizmeti için başlangıç görevleri çalıştırma](cloud-services-startup-tasks.md).

Rolünüz yükseltilmiş ayrıcalıklarla çalıştır'ı yapılandırmak için Ekle bir `<Runtime>` öğesine [.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) dosya.

```xml
<ServiceDefinition name="CloudServiceLoadTesting" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRoleWithSBQueue1" vmsize="Large">

    <!-- ... cut to save space ... -->

    <Runtime executionContext="elevated">
      
    </Runtime>

    <!-- ... cut to save space ... -->

  </WorkerRole>
</ServiceDefinition>
```

Oluşturun ve birkaç kod satırıyla yeni bir performans sayacı kaydedin. Kullanım `System.Diagnostics.PerformanceCounterCategory.Create` kategori hem sayaç oluşturur yöntemi aşırı yüklemesi. Aşağıdaki kod ilk kategori varsa ve eksikse, kategori hem sayaç oluşturur, denetler.

```csharp
using System.Diagnostics;
using Microsoft.WindowsAzure;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRoleWithSBQueue1
{
    public class WorkerRole : RoleEntryPoint
    {
        // Perf counter variable representing times service was used.
        private PerformanceCounter counterServiceUsed;

        public override bool OnStart()
        {
            // ... Other startup code here ...

            // Define the category and counter names.
            string perfCounterCatName = "MyService";
            string perfCounterName = "Times Used";

            // Create the counter if needed. Our counter category only has a single counter.
            // Both the category and counter are created in the same method call.
            if (!PerformanceCounterCategory.Exists(perfCounterCatName))
            {
                PerformanceCounterCategory.Create(perfCounterCatName, "Collects information about the cloud service.", 
                                                  PerformanceCounterCategoryType.SingleInstance, 
                                                  perfCounterName, "How many times the cloud service was used.");
            }

            // Get reference to our counter
            counterServiceUsed = new PerformanceCounter(perfCounterCatName, perfCounterName);
            counterServiceUsed.ReadOnly = false;
            
            return base.OnStart();
        }

        // ... cut class code to save space
    }
}
```

Sayaç kullanmak istediğinizde, çağrı `Increment` veya `IncrementBy` yöntemi.

```csharp
// Increase the counter by 1
counterServiceUsed.Increment();
```

Uygulamanızın kullandığı özel sayaç, sayaç izlemek için Azure Tanılama veya Application Insights'ı yapılandırmanız gerekir.


### <a name="application-insights"></a>Application Insights

Daha önce belirtildiği gibi performans sayaçlarını Application Insights içinde tanımlı **Applicationınsights.config** dosya. Açık **Applicationınsights.config** ve bulma **Applicationınsights** > **TelemetryModules** > **Ekle**  >  **Sayaçları** öğesi. Oluşturma bir `<Add>` alt öğesi ve kümesi `PerformanceCounter` özniteliği kategorisi ve kodunuzda oluşturduğunuz performans sayacının adı. Ayarlama `ReportAs` portalında görmek istediğiniz bir kolay ad özniteliği.

```xml
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">

  <TelemetryModules>

    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <!-- ... cut other perf counters to save space ... -->

        <!-- This new perf counter matches the [category name]\[counter name] defined in your code -->
        <Add PerformanceCounter="\MyService\Times Used" ReportAs="Service used counter" />
      </Counters>
    </Add>

  </TelemetryModules>

<!-- ... cut to save space ... -->
```

### <a name="azure-diagnostics"></a>Azure Tanılama

Daha önce belirtildiği gibi toplamak istediğiniz performans sayaçlarını tanımlanan **diagnostics.wadcfgx** dosya. Visual Studio'da (tanımlanmış olduğu her rolü) bu dosyayı açın ve bulun **DiagnosticsConfiguration** > **PublicConfig** > **WadCfg**  >  **DiagnosticMonitorConfiguration** > **PerformanceCounters** öğesi. Yeni bir **PerformanceCounterConfiguration** alt öğesi olarak. Ayarlama `counterSpecifier` özniteliği kategorisi ve kodunuzda oluşturduğunuz performans sayacının adı. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration  xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig>
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096">

        <!-- ... cut to save space ... -->

        <PerformanceCounters scheduledTransferPeriod="PT1M">
          <!-- ... cut other perf counters to save space ... -->
          
          <!-- This new perf counter matches the [category name]\[counter name] defined in your code -->
          <PerformanceCounterConfiguration counterSpecifier="\MyService\Times Used" sampleRate="PT1M" />

        </PerformanceCounters>
      </DiagnosticMonitorConfiguration>
    </WadCfg>
    
    <!-- ... cut to save space ... -->

  </PublicConfig>
</DiagnosticsConfiguration>
```

## <a name="more-information"></a>Daha fazla bilgi

- [Azure Cloud Services için Application Insights](../azure-monitor/app/cloudservices.md#performance-counters)
- [Application ınsights'ta sistem performans sayaçları](../azure-monitor/app/performance-counters.md)
- [Bir sayaç yolunu belirtme](https://msdn.microsoft.com/library/windows/desktop/aa373193(v=vs.85))
- [Azure tanılama şeması - performans sayaçları](../azure-monitor/platform/diagnostics-extension-schema-1dot3.md#performancecounters-element)
