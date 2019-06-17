---
title: Azure İzleyici'veri deposu için bir Windows sanal makine (Klasik) konuk işletim sistemi ölçümleri gönderme
description: Azure İzleyici'veri deposu için bir Windows sanal makine (Klasik) konuk işletim sistemi ölçümleri gönderme
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ancav
ms.subservice: ''
ms.openlocfilehash: 57212da1a8da7ee6c57faf2413b88a413df04817
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66129572"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-data-store-for-a-windows-virtual-machine-classic"></a>Azure İzleyici'veri deposu için bir Windows sanal makine (Klasik) konuk işletim sistemi ölçümleri gönderme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure İzleyici [tanılama uzantısını](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) ("WAD" veya "Tanılama" olarak bilinir), ölçüm ve günlükleri bir sanal makine, bulut hizmeti veya Service Fabric bir parçası olarak çalışan konuk işletim sistemi (konuk OS) toplamanıza olanak sağlar Küme. Uzantı için telemetri gönderebilir [birçok farklı konumlarda.](https://docs.microsoft.com/azure/monitoring/monitoring-data-collection?toc=/azure/azure-monitor/toc.json)

Bu makalede Azure İzleyici ölçüm depoya bir Windows sanal makine (Klasik) konuk işletim sistemi performans ölçümleri gönderme işlemi açıklanır. Tanılama 1.11 sürüm ile başlayarak, doğrudan Azure ölçümleri mağazasından, burada standart platform zaten toplanan ölçümler izleyiciye ölçümleri yazabilirsiniz. 

Bunları bu konumda depolama, platform ölçümler için yaptığınız gibi aynı eylemleri erişmenize olanak sağlar. Eylemler, uyarı verme, grafik, yönlendirme, neredeyse gerçek zamanlı bir REST API ve daha fazla erişim içerir. Geçmişte, Azure depolama, ancak Azure İzleyici'veri deposu tanılama uzantısını yazıldı. 

Bu makalede açıklanan işlem, yalnızca Windows işletim sistemi çalıştıran Klasik sanal makineler üzerinde çalışır.

## <a name="prerequisites"></a>Önkoşullar

- Siz bir [Hizmet Yöneticisi veya ortak yönetici](../../billing/billing-add-change-azure-subscription-administrator.md) Azure aboneliğinize. 

- Aboneliğiniz ile kaydedilmelidir [Microsoft.Insights](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services). 

- Ya da gerek [Azure PowerShell](/powershell/azure) veya [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) yüklü.

## <a name="create-a-classic-virtual-machine-and-storage-account"></a>Klasik sanal makine ve depolama hesabı oluşturma

1. Azure portalını kullanarak klasik bir sanal makine oluşturun.
   ![Klasik VM oluşturma](./media/collect-custom-metrics-guestos-vm-classic/create-classic-vm.png)

1. Bu sanal makine oluştururken, yeni bir Klasik depolama hesabı oluşturmak için bu seçeneği seçin. Sonraki adımlarda bu depolama hesabı kullanırız.

1. Azure portalında Git **depolama hesapları** kaynak dikey penceresi. Seçin **anahtarları**, depolama hesabı adı ve depolama hesabı anahtarını not alın. Sonraki adımlarda bu bilgileri gerekir.
   ![Depolama erişim anahtarlarını](./media/collect-custom-metrics-guestos-vm-classic/storage-access-keys.png)

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Bölümündeki yönergeleri kullanarak Azure Active Directory kiracınızda bir hizmet ilkesi oluşturma [hizmet sorumlusu oluşturma](../../active-directory/develop/howto-create-service-principal-portal.md). Bu süreç boyunca çalışırken aşağıdakileri unutmayın: 
- Bu uygulama için yeni istemci gizli anahtarı oluşturun.
- Sonraki adımlarda, anahtar ve kullanmak için istemci kimliği kaydedin.

Bu uygulama, ölçümleri karşı yayma istediğiniz kaynağa "İzleme ölçümleri Publisher" izinlerini verin. Bir kaynak grubu veya tüm aboneliği kullanabilirsiniz.  

> [!NOTE]
> Tanılama uzantısını hizmet sorumlusunu Azure İzleyici karşı kimliğini doğrulamak ve klasik sanal Makineniz için ölçümleri yaymak için kullanır.

## <a name="author-diagnostics-extension-configuration"></a>Tanılama uzantı yapılandırmasını yazma

1. Tanılama uzantısı yapılandırma dosyanızı hazırlayın. Bu dosya, hangi günlükleri ve performans sayaçları tanılama uzantısını Klasik sanal Makineniz için toplama belirler. Bir örneği verilmiştir:

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
1. Tanılama dosyanızın "SinksConfig" bölümünde yeni bir Azure Monitor havuzu, aşağıdaki gibi tanımlayın:

    ```xml
    <SinksConfig>
        <Sink name="AzMonSink">
            <AzureMonitor>
                <ResourceId>Provide the resource ID of your classic VM </ResourceId>
                <Region>The region your VM is deployed in</Region>
            </AzureMonitor>
        </Sink>
    </SinksConfig>
    ```

1. Toplanacak performans sayaçlarının listesi nerede listeleniyor yapılandırma dosyanızı bölümünde Azure Monitor havuzu "AzMonSink" için performans sayaçlarını yol.

    ```xml
    <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="AzMonSink">
        <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" />
    ...
    </PerformanceCounters>
    ```

1. Özel yapılandırmada, Azure İzleyici hesabı tanımlayın. Ardından ölçümleri yaymak üzere kullanmak için hizmet sorumlusu bilgilerini ekleyin.

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

## <a name="deploy-the-diagnostics-extension-to-your-cloud-service"></a>Bulut hizmetinize tanılama uzantısını dağıtma

1. PowerShell'i başlatın ve oturum açın.

    ```powershell
    Login-AzAccount
    ```

1. Klasik sanal Makineniz için bağlam ayarlayarak başlatın.

    ```powershell
    $VM = Get-AzureVM -ServiceName <VM’s Service_Name> -Name <VM Name>
    ```

1. Sanal makine ile oluşturulan Klasik depolama hesabı bağlamını ayarlayın.

    ```powershell
    $StorageContext = New-AzStorageContext -StorageAccountName <name of your storage account from earlier steps> -storageaccountkey "<storage account key from earlier steps>"
    ```

1.  Tanılama dosya yolu, aşağıdaki komutu kullanarak bir değişkene ayarlayın:

    ```powershell
    $diagconfig = “<path of the diagnostics configuration file with the Azure Monitor sink configured>”
    ```

1.  Klasik sanal Makineniz için güncelleştirme yapılandırılmış Azure Monitor havuzu tanılama dosyasıyla hazırlayın.

    ```powershell
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $diagconfig -VM $VM -StorageContext $Storage_Context
    ```

1.  Güncelleştirme, sanal makinenizde aşağıdaki komutu çalıştırarak dağıtın:

    ```powershell
    Update-AzureVM -ServiceName "ClassicVMWAD7216" -Name "ClassicVMWAD" -VM $VM_Update.VM
    ```

> [!NOTE]
> Tanılama uzantısını yüklemesinin bir parçası olarak bir depolama hesabı sağlamak için hala zorunludur. Tüm günlükleri veya tanılama yapılandırma dosyasında belirtilen performans sayaçları, belirtilen depolama hesabına yazılır.

## <a name="plot-the-metrics-in-the-azure-portal"></a>Azure portalında ölçümleri Çiz

1.  Azure portalına gidin. 

1.  Sol menüden **İzleyici.**

1.  Üzerinde **İzleyici** dikey penceresinde **ölçümleri**.

    ![Ölçümleri gidin](./media/collect-custom-metrics-guestos-vm-classic/navigate-metrics.png)

1. Kaynak açılan menüsünde, Klasik VM'nizi seçin.

1. Ad alanları açılır menüde **azure.vm.windows.guest**.

1. Ölçümleri açılan menüde **bellek\kaydedilmiş bayt**.
   ![Çizim ölçümleri](./media/collect-custom-metrics-guestos-vm-classic/plot-metrics.png)


## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [özel ölçümler](metrics-custom-overview.md).

