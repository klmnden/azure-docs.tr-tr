---
title: Klasik bulut Hizmetleri için Azure İzleyici ölçüm ölçümleri konuk işletim sistemi göndermek
description: Bulut Hizmetleri için Azure İzleyici ölçüm ölçümleri konuk işletim sistemi göndermek
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: 90e841628d989a16f504d2efd7a2c7b18335ff48
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66129496"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-metric-store-classic-cloud-services"></a>Klasik bulut Hizmetleri için Azure İzleyici ölçüm ölçümleri konuk işletim sistemi göndermek 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure İzleyici ile [tanılama uzantısını](diagnostics-extension-overview.md), ölçüm ve günlükleri bir sanal makine, bulut hizmeti veya Service Fabric kümesinin bir parçası olarak çalışan konuk işletim sistemi (konuk OS) toplayabilir. Uzantı için telemetri gönderebilir [birçok farklı konumlarda.](https://docs.microsoft.com/azure/monitoring/monitoring-data-collection?toc=/azure/azure-monitor/toc.json)

Bu makalede, konuk işletim sistemi performans ölçümlerini Klasik Azure bulut Hizmetleri için Azure İzleyici ölçüm mağazaya göndermek için işlemi açıklanmaktadır. Tanılama 1.11 sürüm ile başlayarak, doğrudan Azure ölçümleri mağazasından, burada standart platform zaten toplanan ölçümler izleyiciye ölçümleri yazabilirsiniz. 

Bunları bu konumda depolamak için platform ölçümler için aynı eylemleri erişmenize olanak sağlar. Eylemler, uyarı verme, grafik, yönlendirme, neredeyse gerçek zamanlı bir REST API ve daha fazla erişim içerir.  Geçmişte, Azure depolama, ancak Azure İzleyici'veri deposu tanılama uzantısını yazıldı.  

Bu makalede geliştirilme yalnızca Azure bulut Hizmetleri'nde performans sayaçları için ana hatlarıyla açıklanan işlemi. Bu diğer özel ölçümler için çalışmaz. 

## <a name="prerequisites"></a>Önkoşullar

- Siz bir [Hizmet Yöneticisi veya ortak yönetici](~/articles/billing/billing-add-change-azure-subscription-administrator.md) Azure aboneliğinize. 

- Aboneliğiniz ile kaydedilmelidir [Microsoft.Insights](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services). 

- Ya da gerek [Azure PowerShell](/powershell/azure) veya [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) yüklü.

## <a name="provision-a-cloud-service-and-storage-account"></a>Bir bulut hizmeti ve depolama hesabı sağlayın 

1. Oluşturun ve klasik bulut hizmetini dağıtın. Bir örnek Klasik bulut Hizmetleri uygulama ve dağıtım sırasında bulunabilir [Azure Cloud Services ve ASP.NET kullanmaya başlama](../../cloud-services/cloud-services-dotnet-get-started.md). 

2. Mevcut bir depolama hesabını kullanabilir veya yeni bir depolama hesabı dağıtın. Depolama hesabı oluşturduğunuz Klasik bulut hizmetiyle aynı bölgede olması durumunda en iyisidir. Azure portalında Git **depolama hesapları** kaynak dikey penceresini ve ardından **anahtarları**. Depolama hesabı adı ve depolama hesabı anahtarını not edin. Sonraki adımlarda bu bilgileri gerekir.

   ![Depolama hesabı anahtarları](./media/collect-custom-metrics-guestos-vm-cloud-service-classic/storage-keys.png)

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma 

Bölümündeki yönergeleri kullanarak Azure Active Directory kiracınızda bir hizmet ilkesi oluşturma [Azure Active Directory kaynaklarına erişmek uygulama ve hizmet sorumlusu oluşturmak için portalı kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal). Bu süreçte oluşturacağız ancak aşağıdakilere dikkat edin: 

- Herhangi bir URL'de oturum açma URL'si girebilirsiniz.  
- Bu uygulama için yeni istemci gizli anahtarı oluşturun.  
- Sonraki adımlarda, anahtar ve kullanmak için istemci kimliği kaydedin.  

Önceki adımda oluşturduğunuz uygulama vermek *izleme ölçümleri yayımcı* ölçümleri karşı yayma istediğiniz kaynak izni. Özel ölçümler birçok kaynağa karşı yaymak için uygulamayı kullanmayı planlıyorsanız, kaynak grubu veya abonelik düzeyinde bu izinleri verebilir.  

> [!NOTE]
> Tanılama uzantısını hizmet sorumlusunu Azure İzleyici karşı kimlik doğrulaması ve bulut hizmetiniz için ölçümleri yaymak için kullanır.

## <a name="author-diagnostics-extension-configuration"></a>Tanılama uzantı yapılandırmasını yazma 

Tanılama uzantısı yapılandırma dosyanızı hazırlayın. Bu dosya, hangi günlükleri ve performans sayaçları tanılama uzantısını bulut hizmetinizin toplamak belirler. Örnek tanılama yapılandırma dosyası aşağıda verilmiştir:  

```XML
<?xml version="1.0" encoding="utf-8"?> 
<DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"> 
  <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"> 
    <WadCfg> 
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096"> 
        <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" /> 
        <Directories scheduledTransferPeriod="PT1M"> 
          <IISLogs containerName="wad-iis-logfiles" /> 
          <FailedRequestLogs containerName="wad-failedrequestlogs" /> 
        </Directories> 
        <PerformanceCounters scheduledTransferPeriod="PT1M"> 
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" /> 
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT15S" /> 
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" /> 
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Page Faults/sec" sampleRate="PT15S" /> 
        </PerformanceCounters> 
        <WindowsEventLog scheduledTransferPeriod="PT1M"> 
          <DataSource name="Application!*[System[(Level=1 or Level=2 or Level=3)]]" /> 
          <DataSource name="Windows Azure!*[System[(Level=1 or Level=2 or Level=3 or Level=4)]]" /> 
        </WindowsEventLog> 
        <CrashDumps> 
          <CrashDumpConfiguration processName="WaIISHost.exe" /> 
          <CrashDumpConfiguration processName="WaWorkerHost.exe" /> 
          <CrashDumpConfiguration processName="w3wp.exe" /> 
        </CrashDumps> 
        <Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Error" /> 
      </DiagnosticMonitorConfiguration> 
      <SinksConfig> 
      </SinksConfig> 
    </WadCfg> 
    <StorageAccount /> 
  </PublicConfig> 
  <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"> 
    <StorageAccount name="" endpoint="" /> 
</PrivateConfig> 
  <IsEnabled>true</IsEnabled> 
</DiagnosticsConfiguration> 
```

Tanılama dosyanızın "SinksConfig" bölümünde yeni bir Azure Monitor havuzu tanımlayın: 

```XML
  <SinksConfig> 
    <Sink name="AzMonSink"> 
    <AzureMonitor> 
      <ResourceId>-Provide ClassicCloudService’s Resource ID-</ResourceId> 
      <Region>-Azure Region your Cloud Service is deployed in-</Region> 
    </AzureMonitor> 
    </Sink> 
  </SinksConfig> 
```

Azure Monitor havuzu yapılandırma dosyanızı toplamak için performans sayaçları listesi burada bölümünde ekleyin. Bu giriş, ölçümleriniz Azure İzleyici için belirttiğiniz tüm performans sayaçlarını yönlendirilmesini sağlar. Ekleyebilir veya ihtiyaçlarınıza göre performans sayaçları kaldırın. 

```xml
    <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="AzMonSink">
        <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" />
    ...
    </PerformanceCounters>
```

Son olarak, özel yapılandırmasında ekleme bir *Azure İzleyici hesabı* bölümü. Hizmet sorumlusu istemci kimliği ve daha önce oluşturduğunuz parolayı girin. 

```XML
<PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"> 
  <StorageAccount name="" endpoint="" /> 
    <AzureMonitorAccount> 
      <ServicePrincipalMeta> 
        <PrincipalId>clientId from step 3</PrincipalId> 
        <Secret>client secret from step 3</Secret> 
      </ServicePrincipalMeta> 
    </AzureMonitorAccount> 
</PrivateConfig> 
```

Bu tanılama dosyasını yerel olarak kaydedin.  

## <a name="deploy-the-diagnostics-extension-to-your-cloud-service"></a>Bulut hizmetinize tanılama uzantısını dağıtma 

PowerShell'i başlatın ve Azure'da oturum açın. 

```powershell
Login-AzAccount 
```

Daha önce oluşturduğunuz depolama hesabı ayrıntılarını depolamak için aşağıdaki komutları kullanın. 

```powershell
$storage_account = <name of your storage account from step 3> 
$storage_keys = <storage account key from step 3> 
```

Benzer şekilde, tanılama dosya yolu, aşağıdaki komutu kullanarak bir değişkene ayarlayın:

```powershell
$diagconfig = “<path of the Diagnostics configuration file with the Azure Monitor sink configured>” 
```

Tanılama uzantısını Azure Monitor havuzu aşağıdaki komutu kullanarak yapılandırılmış ile tanılama dosyasıyla bulut hizmetinize dağıtın:  

```powershell
Set-AzureServiceDiagnosticsExtension -ServiceName <classicCloudServiceName> -StorageAccountName $storage_account -StorageAccountKey $storage_keys -DiagnosticsConfigurationPath $diagconfig 
```

> [!NOTE] 
> Tanılama uzantısını yüklemesinin bir parçası olarak bir depolama hesabı sağlamak için hala zorunludur. Tüm günlükleri veya tanılama yapılandırma dosyasında belirtilen performans sayaçları belirtilen depolama hesabına yazılır.  

## <a name="plot-metrics-in-the-azure-portal"></a>Azure portalında ölçümleri Çiz 

1. Azure portalına gidin. 

   ![Ölçümleri Azure portalı](./media/collect-custom-metrics-guestos-vm-cloud-service-classic/navigate-metrics.png)

2. Sol menüden **İzleyici.**

3. Üzerinde **İzleyici** dikey penceresinde **ölçümleri Önizleme** sekmesi.

4. Kaynak açılan menüsünde, Klasik bulut hizmetinizi seçin.

5. Ad alanları açılır menüde **azure.vm.windows.guest**. 

6. Ölçümleri açılan menüde **bellek\kaydedilmiş bayt**. 

Filtreleme ve özellikleri bölme boyutu, belirli bir rolü veya rol örneği tarafından kullanılan toplam bellek görüntülemek için kullanın. 

 ![Ölçümleri Azure portalı](./media/collect-custom-metrics-guestos-vm-cloud-service-classic/metrics-graph.png)

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [özel ölçümler](metrics-custom-overview.md).

