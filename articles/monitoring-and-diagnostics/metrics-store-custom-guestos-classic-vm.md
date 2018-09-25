---
title: Azure İzleyici'veri deposu için bir Windows sanal makine (Klasik) konuk işletim sistemi ölçümleri gönderme
description: Azure İzleyici'veri deposu için bir Windows sanal makine (Klasik) konuk işletim sistemi ölçümleri gönderme
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ancav
ms.component: ''
ms.openlocfilehash: cb803450f7765ae62292ff3afb7f32209b437f78
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46978939"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-data-store-for-a-windows-virtual-machine-classic"></a>Azure İzleyici'veri deposu için bir Windows sanal makine (Klasik) konuk işletim sistemi ölçümleri gönderme

Azure İzleyici [Windows Azure tanılama uzantısını](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) (WAD), konuk işletim sistemi (konuk OS) çalışan bir sanal makine, bulut hizmeti veya Service Fabric kümesinin parçası olarak ölçüm ve günlükleri toplamanızı sağlar. Uzantı, daha önce bağlı makalede listelenen birçok farklı konumlara telemetri gönderebilir.

Bu makalede, bir Windows sanal makine için (Klasik) Azure İzleyici ölçüm deposuna Gönder konuk işletim sistemi performans ölçümlerine işlemi açıklanmaktadır. Sürüm 1.11 WAD ile başlayarak, doğrudan Azure İzleyici ölçümleri mağazaya standart platform ölçümleri zaten burada toplanan ölçümler yazabilirsiniz. Bunları bu konumda depolama, platform ölçümleri için kullanılabilen aynı eylemleri erişmenize olanak sağlar.  Eylemler, neredeyse gerçek zamanlı uyarı verme, grafik, yönlendirme dahil, REST API ve daha fazlasını erişim.  Geçmişte, Azure depolama, ancak Azure İzleyici'veri deposu WAD uzantısı yazıldı. 

Bu makalede açıklanan işlemi yalnızca klasik sanal makineleri Windows işletim sistemini çalıştıran çalışır.

## <a name="pre-requisites"></a>Ön koşullar

- Siz bir [Hizmet Yöneticisi veya ortak yönetici](https://docs.microsoft.com/azure/billing/billing-add-change-azure-subscription-administrator.md) Azure aboneliğinize 

- Aboneliğiniz ile kaydedilmelidir [Microsoft.Insights](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) 

- Ya da gerek [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) yüklü veya [Azure CloudShell](https://docs.microsoft.com/azure/cloud-shell/overview.md) 

## <a name="create-a-classic-virtual-machine-and-storage-account"></a>Klasik sanal makine ve depolama hesabı oluşturma

1. Klasik Azure portalı kullanarak VM oluşturma ![Klasik VM oluştur](./media/metrics-store-custom-guestos-classic-vm/create-classic-vm.png)

1. Yeni bir Klasik depolama hesabı oluşturmak bu sanal makine oluşturulurken'ı seçin. Sonraki adımlarda bu depolama hesabı kullanırız.

1. Azure portalında depolama hesabı kaynak dikey penceresine gidin ve seçin **anahtarları** Not depolama hesabı adı ve depolama hesabı anahtarını alın. Sonraki adımlarda bu anahtarları ihtiyacınız ![depolama erişim anahtarlarını](./media/metrics-store-custom-guestos-classic-vm/storage-access-keys.png)

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Azure Active Directory kiracınızdaki konusunda bulunan yönergeleri kullanarak bir hizmet ilkesi oluşturma [hizmet sorumlusu oluşturma](../azure-resource-manager/resource-group-create-service-principal-portal.md). Bu süreç boyunca çalışırken aşağıdakileri unutmayın: 
- Bu uygulama için yeni istemci gizli anahtarı oluşturma  
- Sonraki adımlarda, anahtar ve kullanmak için istemci kimliği kaydedin.

Bu uygulama, ölçümleri karşı yayma istediğinizden kaynağına "İzleme ölçümleri Publisher" izinlerini verin. Bir kaynak grubu veya bütün bir aboneliği kullanabilir.  

> [!NOTE]
> Tanılama uzantısını Azure İzleyici karşı kimliğini doğrulamak ve klasik sanal Makineniz için ölçümleri yaymak için hizmet sorumlusu kullanın.

## <a name="author-diagnostics-extension-configuration"></a>Yazar tanılama uzantısı yapılandırma

1. WAD tanılama uzantısı yapılandırma dosyasını hazırlayın. Bu dosya, hangi günlükleri ve performans sayaçları tanılama uzantısını Klasik sanal Makineniz için toplama belirler. Bir örneği aşağıda verilmiştir.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <WadCfg>
        <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
            <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
            <Directories scheduledTransferPeriod="PT1M">
                <IISLogs containerName="wad-iis-logfiles" />
                <FailedRequestLogs containerName="wad-failedrequestlogs" />
            </Directories>
            <PerformanceCounters scheduledTransferPeriod="PT1M">
                <PerformanceCounterConfiguration counterSpecifier="\Processor(*)\% Processor Time" sampleRate="PT15S" />
                <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT15S" />
                <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" />
                <PerformanceCounterConfiguration counterSpecifier="\Memory\% Committed Bytes" sampleRate="PT15S" />
                <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(*)\Disk Read Bytes/sec" sampleRate="PT15S" />
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
            <Metrics resourceId="/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicCompute/virtualMachines/MyClassicVM">
                <MetricAggregation scheduledTransferPeriod="PT1M" />
                <MetricAggregation scheduledTransferPeriod="PT1H" />
            </Metrics>
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
1. Tanılama dosyanızın "SinksConfig" bölümünde yeni bir Azure Monitor havuzu tanımlayın:

    ```xml
    <SinksConfig>
        <Sink name="AzMonSink">
            <AzureMonitor>
                <ResourceId>Provide your Classic VM’s Resource ID </ResourceId>
                <Region>Region your VM is deployed in</Region>
            </AzureMonitor>
        </Sink>
    </SinksConfig>
    ```

1. Toplanacak performans sayaçlarının listesi nerede listeleniyor yapılandırma dosyanızı kısmında, Azure İzleyici havuz için "AzMonSink" performans sayaçlarını yol.

    ```xml
    <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="AzMonSink">
        <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" />
    ...
    </PerformanceCounters>
    ```

1. Özel yapılandırma Azure İzleyici hesabı tanımlayın ve ölçümleri yaymak üzere kullanmak için hizmet sorumlusu bilgilerini ekleyin.

    ```xml
    <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <StorageAccount name="" endpoint="" />
        <AzureMonitorAccount>
            <ServicePrincipalMeta>
                <PrincipalId>clientId for your service principal</PrincipalId>
                <Secret>client secret of your service principal</Secret>
            </ServicePrincipalMeta>
        </AzureMonitorAccount>
    </PrivateConfig>
    ```

1. Bu dosyayı yerel olarak kaydedin.

## <a name="deploy-diagnostics-extension-to-your-cloud-service"></a>Bulut hizmetinize tanılama uzantısını dağıtma

1. PowerShell'i başlatın ve oturum açın

    ```powershell
    Login-AzureRmAccount
    ```

1. Klasik sanal makinenizde bağlamı başlayın

    ```powershell
    $VM = Get-AzureVM -ServiceName <VM’s Service_Name> -Name <VM Name>
    ```

1. Sanal makine ile oluşturulan Klasik depolama hesabı bağlamını ayarlayın.

    ```powershell
    $StorageContext = New-AzureStorageContext -StorageAccountName <name of your storage account from earlier steps> -storageaccountkey "<storage account key from earlier steps>"
    ```

1.  Bir değişken kullanarak tanılama dosya yolu ayarla aşağıdaki komutu.

    ```powershell
    $diagconfig = “<path of the diagnostics configuration file with the Azure Monitor sink configured>”
    ```

1.  Klasik sanal makinenize tanılama dosyasıyla güncelleştirme yapılandırılmış Azure Monitor havuzu ile hazırlama

    ```powershell
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $diagconfig -VM $VM -StorageContext $Storage_Context
    ```

1.  Aşağıdaki komutu çalıştırarak sanal makinenizde güncelleştirme dağıtma

    ```powershell
    Update-AzureVM -ServiceName "ClassicVMWAD7216" -Name "ClassicVMWAD" -VM $VM_Update.VM
    ```

> [!NOTE]
> Tanılama uzantısını yüklemesinin bir parçası olarak bir depolama hesabı sağlamak için hala zorunludur. Tüm günlükleri ve/veya tanılama yapılandırma dosyasında belirtilen performans sayaçları, belirtilen depolama hesabına yazılır.

## <a name="plot-the-metrics-in-the-azure-portal"></a>Azure portalında ölçümleri Çiz

1.  Azure portalına gidin

1.  Sol taraftaki menüden tıklatın İzleyicisi

1.  İzleyici dikey penceresinde tıklayın **ölçümleri**
   ![ölçümleri gidin](./media/metrics-store-custom-guestos-classic-vm/navigate-metrics.png)

1. Kaynak açılan Klasik VM'nizi seçin

1. Ad alanları açılır seçin **azure.vm.windows.guest**

1. Ölçümler, açılan menü, seçin **bellek\kaydedilmiş bayt**
   ![çizim ölçümleri](./media/metrics-store-custom-guestos-classic-vm/plot-metrics.png)


## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [özel ölçümler](metrics-custom-overview.md).
