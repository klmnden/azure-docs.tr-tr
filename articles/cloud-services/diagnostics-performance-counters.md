---
title: "Azure bulut Hizmetleri performans sayaçları toplama | Microsoft Docs"
description: "Bul, kullanın ve performans sayaçları Azure Diagnostics ve Application Insights ile birlikte bulut hizmetlerini oluşturma hakkında bilgi edinin."
services: cloud-services
documentationcenter: .net
author: thraka
manager: timlt
editor: 
ms.assetid: 
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/18
ms.author: adegeo
ms.openlocfilehash: 3e0af48c172fa912f0ac9e05b7b761dd7eaad795
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="collect-performance-counters-for-your-azure-cloud-service"></a>Azure bulut hizmetiniz için performans sayaçlarını Topla

Performans sayaçları uygulamanız ve ana bilgisayar ne kadar iyi performans gösterdiğini izlemek bir yol sağlar. Windows Server donanım, uygulamalar, işletim sistemi ve daha fazla bilgi için ilgili birçok farklı performans sayaçları sağlar. Toplama ve performans sayaçları için Azure gönderme daha iyi kararlar almanıza yardımcı olmak için bu bilgileri analiz edebilirsiniz. 

## <a name="discover-available-counters"></a>Kullanılabilir sayaçlar Bul

Bir performans sayacı kümesi adı (kategori olarak da bilinir) ve bir veya daha fazla sayaç olmak üzere iki bölümden oluşur. Kullanılabilir performans sayaçları listesini almak için PowerShell kullanın:

```PowerShell
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

`CounterSetName` Özellik kümesi (veya kategori) temsil eder ve performans sayaçları için ilişkili iyi göstergesidir. `Paths` Özellik kümesi için sayaçları koleksiyonunu temsil eder. Ayrıca alabilir `Description` özelliği sayaç kümesi hakkında daha fazla bilgi için.

Tüm sayaçları için bir kümesi almak için `CounterSetName` değer ve genişletin `Paths` koleksiyonu. Her bir yol öğesi Sorgulayabileceğiniz bir sayacıdır. Örneğin, ilgili kullanılabilir sayaçlar almak için `Processor` ayarlayın, Genişlet `Paths` koleksiyonu:

```PowerShell
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

Bu tek tek sayaç yolları tanılama çerçevesi eklenebilir, bulut hizmeti kullanır. Nasıl bir performans sayacı yolunu oluşturulan hakkında daha fazla bilgi için bkz: [sayaç yolu belirterek](https://msdn.microsoft.com/library/windows/desktop/aa373193(v=vs.85)).

## <a name="collect-a-performance-counter"></a>Bir performans sayacı Topla

Bir performans sayacı Azure Tanılama veya Application Insights bulut hizmetinizin eklenebilir.

### <a name="application-insights"></a>Application Insights

Bulut Hizmetleri için Azure Application Insights toplamak istediğiniz hangi performans sayaçlarını belirttiğiniz sağlar. Çalıştırdıktan sonra [projenize Application Insights ekleyin](../application-insights/app-insights-cloudservices.md#sdk), adlı bir yapılandırma dosyası **Applicationınsights.config** Visual Studio projenize eklenir. Bu yapılandırma dosyası, hangi tür bilgileri Application Insights toplar ve Azure'a gönderilmeden tanımlar.

Açık **Applicationınsights.config** dosya ve Bul **Applicationınsights** > **TelemetryModules** öğesi. Her `<Add>` alt öğesi yapılandırmasıyla birlikte toplanacak telemetri türünü tanımlar. Performans sayacı telemetri modül türü `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector`. Bu öğe zaten tanımlanmış olması durumunda, ikinci kez eklemeyin. Her performans sayacı toplamak için adlı düğümün altında tanımlanan `<Counters>`. Sürücü performans sayaçlarını toplayan örnek aşağıda verilmiştir:

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

Her performans sayacı olarak temsil edilen bir `<Add>` öğesinin altında `<Counters>`. `PerformanceCounter` Özniteliği toplamak için performans sayacı tanımlar. `ReportAs` Performans sayacı için Azure portalında görüntülenecek olan başlık bir özniteliktir. Topladığınız tüm performans sayacı adlı bir kategoriye put **özel** Portalı'nda. Azure Tanılama, bu performans sayaçlarını toplanan ve Azure'a gönderilen aralığı ayarlanamıyor. Application Insights ile performans sayaçları toplanır ve dakikada gönderilen. 

Application Insights otomatik olarak aşağıdaki performans sayaçlarını toplar:

* \Process(??APP_WIN32_PROC??)\% İşlemci Süresi
* \Memory\Available Bytes
* \.NET CLR Exceptions(??APP_CLR_PROC??)\# of Exceps Thrown / sec
* \Process(??APP_WIN32_PROC??)\Private Bytes
* \Process(??APP_WIN32_PROC??)\IO Data Bytes/sec
* \Processor(_Total)\% Processor Time

Daha fazla bilgi için bkz: [sistem performans sayaçları Application ınsights'ta](../application-insights/app-insights-performance-counters.md) ve [Azure bulut Hizmetleri için Application Insights](../application-insights/app-insights-cloudservices.md#performance-counters).

### <a name="azure-diagnostics"></a>Azure Tanılama

> [!IMPORTANT]
> Bu veriler storage hesabına toplanır olsa da, portal mu **değil** grafik verileri için yerel bir yol sağlar. Uygulamanıza Application Insights gibi başka bir tanılama hizmeti tümleştirmek önerilir.

Bulut Hizmetleri için Azure tanılama uzantısını toplamak istediğiniz hangi performans sayaçlarını belirttiğiniz sağlar. Azure tanılama ayarlamak için bkz: [bulut hizmetini izleme genel görünümü](cloud-services-how-to-monitor.md#setup-diagnostics-extension).

Toplamak istediğiniz performans sayaçlarını tanımlanan **diagnostics.wadcfgx** dosya. Visual Studio'da (Bu rol tanımlanmış) bu dosyayı açın ve Bul **DiagnosticsConfiguration** > **PublicConfig** > **WadCfg**  >  **DiagnosticMonitorConfiguration** > **performans sayaçları** öğesi. Yeni bir ekleme **PerformanceCounterConfiguration** alt öğesi olarak. Bu öğe iki özniteliklere sahiptir: `counterSpecifier` ve `sampleRate`. `counterSpecifier` Öznitelik tanımlar hangi sistem performans sayacı toplamak için (önceki bölümde Anahatlı) ayarlayın. `sampleRate` Değeri gösterir bu değeri ne sıklıkta sorgulanan. Bir bütün olarak tüm performans sayaçlarının göre üst Azure'a aktarılır `PerformanceCounters` öğenin `scheduledTransferPeriod` öznitelik değeri.

Hakkında daha fazla bilgi için `PerformanceCounters` schema öğesi için bkz: [Azure tanılama şema](../monitoring-and-diagnostics/azure-diagnostics-schema-1dot3-and-later.md#performancecounters-element).

Tarafından tanımlanan süre `sampleRate` XML süresi veri türü performans sayacı ne sıklıkta sorgulanan belirtmek için özniteliğini kullanır. Aşağıdaki örnek hızı ayarlamak `PT3M`, anlamına `[P]eriod[T]ime[3][M]inutes`: üç dakikada.

Nasıl hakkında daha fazla bilgi için `sampleRate` ve `scheduledTransferPeriod` olan tanımlanan bkz **süre veri türü** bölümüne [W3 XML tarih ve saat tarih türleri](https://www.w3schools.com/XML/schema_dtypes_date.asp) Öğreticisi.

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

Yeni bir performans sayacı oluşturulur ve kodunuz tarafından kullanılır. Yeni bir performans sayacı oluşturan kodunuzu, aksi takdirde başarısız olur, yükseltilmiş çalıştırması gerekir. Bulut hizmetinizi `OnStart` başlatma kodunu rolü yükseltilmiş bağlamında gerek performans sayacı oluşturabilirsiniz. Veya yükseltilmiş çalıştırır ve performans sayacı oluşturan bir başlangıç görevi oluşturabilirsiniz. Başlangıç görevleri hakkında daha fazla bilgi için bkz: [nasıl yapılandırılacağı ve bir bulut hizmeti için başlangıç görevleri çalıştırmak](cloud-services-startup-tasks.md).

Yükseltilmiş çalıştırmak için rolünü yapılandırmak için Ekle bir `<Runtime>` öğesine [.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) dosya.

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

Oluşturun ve birkaç kod satırıyla, yeni bir performans sayacı kaydedin. Kullanım `System.Diagnostics.PerformanceCounterCategory.Create` kategoriye ve sayaç oluşturur yöntemi aşırı yüklemesini. Kategori varsa ve eksikse, kategoriye ve sayaç oluşturur, aşağıdaki kodu ilk denetler.

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

            // Define the cateogry and counter names.
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

Uygulamanız, özel sayaç kullanır, Azure Tanılama veya Application Insights sayaç izlemek için yapılandırmanız gerekir.


### <a name="application-insights"></a>Application Insights

Daha önce belirtildiği gibi performans sayaçlarını Application Insights tanımlanan için **Applicationınsights.config** dosya. Açık **Applicationınsights.config** ve Bul **Applicationınsights** > **TelemetryModules** > **Ekle**  >  **Sayaçları** öğesi. Oluşturma bir `<Add>` alt öğesi ve kümesi `PerformanceCounter` kodunuzda oluşturduğunuz performans sayacının adını ve Kategori özniteliği. Ayarlama `ReportAs` portalında görmek istediğiniz bir kolay ad özniteliği.

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

Daha önce belirtildiği gibi toplamak istediğiniz performans sayaçlarını tanımlanan **diagnostics.wadcfgx** dosya. Visual Studio'da (Bu rol tanımlanmış) bu dosyayı açın ve Bul **DiagnosticsConfiguration** > **PublicConfig** > **WadCfg**  >  **DiagnosticMonitorConfiguration** > **performans sayaçları** öğesi. Yeni bir ekleme **PerformanceCounterConfiguration** alt öğesi olarak. Ayarlama `counterSpecifier` kodunuzda oluşturduğunuz performans sayacının adını ve Kategori özniteliği. 

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

- [Azure bulut Hizmetleri için Application Insights](../application-insights/app-insights-cloudservices.md#performance-counters)
- [Application Insights sistem performans sayaçları](../application-insights/app-insights-performance-counters.md)
- [Sayaç yolu belirtme](https://msdn.microsoft.com/library/windows/desktop/aa373193(v=vs.85))
- [Azure tanılama şema - performans sayaçları](../monitoring-and-diagnostics/azure-diagnostics-schema-1dot3-and-later.md#performancecounters-element)