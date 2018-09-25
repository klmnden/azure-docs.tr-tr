---
title: Klasik bulut hizmetini Azure İzleyici ölçüm için konuk işletim sistemi ölçümleri göndermek
description: Klasik bulut hizmetini Azure İzleyici ölçüm için konuk işletim sistemi ölçümleri göndermek
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: howto
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: be27ff3f8dda3209a011c3ad79d1a7a1f1d259fe
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46986923"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-metric-store-classic-cloud-service"></a>Klasik bulut hizmetini Azure İzleyici ölçüm için konuk işletim sistemi ölçümleri göndermek

Azure İzleyici [Windows Azure tanılama uzantısını](azure-diagnostics.md) (WAD), konuk işletim sisteminden (guestOS) ölçüm ve günlükleri toplamak bir sanal makine, bulut hizmeti veya Service Fabric kümesinin bir parçası çalışan sağlar.  Uzantı, daha önce bağlı makalede listelenen birçok farklı konumlara telemetri gönderebilir.  

Bu makalede, Klasik Azure Cloud Services Azure İzleyici ölçüm deposuna Gönder konuk işletim sistemi performans ölçümlerine işlemi açıklanmaktadır. Sürüm 1.11 WAD ile başlayarak, doğrudan Azure İzleyici ölçümleri mağazaya standart platform ölçümleri zaten burada toplanan ölçümler yazabilirsiniz. Bunları bu konumda depolama, platform ölçümleri için kullanılabilen aynı eylemleri erişmenize olanak sağlar.  Eylemler, neredeyse gerçek zamanlı uyarı verme, grafik, yönlendirme dahil, REST API ve daha fazlasını erişim.  Geçmişte, Azure depolama, ancak Azure İzleyici'veri deposu WAD uzantısı yazıldı.  

Bu makalede açıklanan işlemi, performans sayaçları için yalnızca Azure Cloud Services üzerinde çalışır. Diğer özel ölçümler için çalışmaz. 
   

## <a name="pre-requisites"></a>Ön koşullar

- Siz bir [Hizmet Yöneticisi veya ortak yönetici](https://docs.microsoft.com/azure/billing/billing-add-change-azure-subscription-administrator.md) Azure aboneliğinize 

- Aboneliğiniz ile kaydedilmelidir [Microsoft.Insights](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) 

- Ya da gerek [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) yüklü veya [Azure CloudShell](https://docs.microsoft.com/azure/cloud-shell/overview.md) 


## <a name="provision-cloud-service-and-storage-account"></a>Bulut hizmeti ve depolama hesabı sağlayın 

1. Klasik bir bulut hizmeti oluşturma ve dağıtma (örnek Klasik bulut hizmetini uygulama ve dağıtım bulunabilir [burada](../cloud-services/cloud-services-dotnet-get-started.md). 

2. Mevcut bir depolama hesabını kullanabilir veya yeni bir depolama hesabı dağıtın. Yeni oluşturduğunuz Klasik bulut hizmetiyle aynı bölgede depolama hesabı seçiliyse en iyisidir. Azure portalında depolama hesabı kaynak dikey penceresine gidin ve seçin **anahtarları**. Unutmayın depolama hesabı adı ve depolama hesabı tuşunu, bunlar sonraki adımda gerekli olacaktır.

   ![Depolama Hesabı Anahtarları](./media/metrics-store-custom-guestos-classic-cloud-service/storage-keys.png)



## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma 

Azure Active Directory kiracınızdaki konusunda bulunan yönergeleri kullanarak bir hizmet ilkesi oluşturma... / azure/azure-resource-manager/resource-group-create-service-principal-portal. Bu süreç boyunca çalışırken aşağıdakileri unutmayın: 
  - Herhangi bir URL için oturum açma URL'si girebilirsiniz.  
  - Bu uygulama için yeni istemci gizli anahtarı oluşturma  
  - Sonraki adımlarda, anahtar ve kullanmak için istemci kimliği kaydedin.  

Önceki adımda oluşturduğunuz uygulama vermek *izleme ölçümleri yayımcı* istediğiniz ölçümleri karşı yaymak için kaynak izni. Özel ölçümler birçok kaynağa karşı yaymak için uygulamayı kullanmayı planlıyorsanız, kaynak grubu veya abonelik düzeyinde bu izinleri verebilir.  

> [!NOTE]
> Tanılama uzantısı, Azure İzleyici karşı kimlik doğrulaması ve bulut hizmetiniz için ölçümleri yaymak için hizmet sorumlusu kullanır. 

## <a name="author-diagnostics-extension-configuration"></a>Yazar tanılama uzantısı yapılandırma 

WAD tanılama uzantısı yapılandırma dosyasını hazırlayın. Bu dosya, hangi günlükleri ve performans sayaçları tanılama uzantısını bulut hizmetinizin toplamak belirler. Örnek tanılama yapılandırma dosyası aşağıda verilmiştir.  

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

Azure İzleyici havuz yapılandırma dosyanızı nereye Toplanacak performans listesi sayaçları bölümünde ekleyin. Bu giriş, belirtilen tüm performans sayaçlarını ölçümleriniz Azure İzleyici yönlendirilir sağlar. Gereksinimlerinize göre performans sayaçlarını Ekle/Kaldır çekinmeyin. 

```XML
<PerformanceCounters scheduledTransferPeriod="PT1M" sinks="AzMonSink"> 
 <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" /> 
  … 
</PerformanceCounters> 
```
Son olarak, özel yapılandırmasında ekleme bir *Azure İzleyici hesabı* bölümü. Hizmet sorumlusu istemci kimliği ve önceki adımda oluşturulan parolasını girin. 

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

PowerShell ve Azure'da oturum başlatma 

```PowerShell
Login-AzureRmAccount 
```

Değişkenleri aşağıdaki komutları kullanarak bir önceki adımda oluşturulan depolama hesabı ayrıntıları hakkındaki ayrıntıları Store. 

```PowerShell
$storage_account = <name of your storage account from step 3> 
$storage_keys = <storage account key from step 3> 
```
 
Benzer şekilde, bir değişken kullanarak tanılama dosya yolunu ayarlayın. aşağıdaki komutu. 

```PowerShell
$diagconfig = “<path of the diagnostics configuration file with the Azure Monitor sink configured>” 
```
 
Aşağıdaki komutu kullanarak yapılandırılmış Azure Monitor havuzu ile bulut hizmetinize tanılama dosyasıyla tanılama uzantısını dağıtma 

```PowerShell
Set-AzureServiceDiagnosticsExtension -ServiceName <classicCloudServiceName> -StorageAccountName $storage_account -StorageAccountKey $storage_keys -DiagnosticsConfigurationPath $diagconfig 
```
 
> [!NOTE] 
> Tanılama uzantısını yüklemesinin bir parçası olarak bir depolama hesabı sağlamak için hala zorunludur. Tüm günlükleri ve/veya tanılama yapılandırma dosyasında belirtilen performans sayaçları, belirtilen depolama hesabına yazılır.  

## <a name="plot-metrics-in-the-azure-portal"></a>Azure portalında ölçümleri Çiz 

Azure portalına gidin 

 ![Ölçümleri Azure portalı](./media/metrics-store-custom-guestos-classic-cloud-service/navigate-metrics.png)

1. Sol taraftaki menüde İzleyicisi 

1. İzleyici dikey penceresinde, ölçümleri Önizleme sekmesini tıklatın. 

1. Kaynak açılan menü, Klasik bulut hizmetinizi seçin 

1. Aşağı açılan ad alanlarında seçin **azure.vm.windows.guest** 

1. Ölçümler, açılan menü, seçin *bellek\kaydedilmiş bayt yüzdesi* 

Boyut filtresi kullanarak ve özellikleri bölme belirli bir rolü ve her rol örneği tarafından kullanılan toplam bellek görüntülemeyi seçebilirsiniz. 

 ![Ölçümleri Azure portalı](./media/metrics-store-custom-guestos-classic-cloud-service/metrics-graph.png)

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [özel ölçümler](metrics-custom-overview.md).



