---
title: Azure İzleyicisi'nde olayları için IIS ve tablo depolama için BLOB Depolama kullanma | Microsoft Docs
description: Azure İzleyici, tablo depolama için tanılama yazma Azure Hizmetleri için günlükleri veya BLOB depolamaya yazılan IIS günlüklerini okuyabilir.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: bf444752-ecc1-4306-9489-c29cb37d6045
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/12/2017
ms.author: magoedte
ms.openlocfilehash: 901544886e0a0c90c29e83fc71f7a7a25ffc6862
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66244896"
---
# <a name="collect-azure-diagnostic-logs-from-azure-storage"></a>Azure Depolama'dan Azure tanılama günlükleri Topla

Azure İzleyici, tablo depolama için tanılama yazma aşağıdaki hizmetlerin günlükleri veya BLOB depolamaya yazılan IIS günlükler okuyabilirsiniz:

* Service Fabric kümeleri (Önizleme)
* Virtual Machines
* Web/çalışan rolleri

Azure Tanılama'yı Azure İzleyici, bu kaynaklar için bir Log Analytics çalışma alanına veri toplayabilmek için önce etkinleştirilmesi gerekir.

Tanılama etkinleştirildikten sonra Azure portalını kullanabilirsiniz veya PowerShell günlükleri toplamak için çalışma alanı yapılandırın.

Azure Tanılama, bir çalışan rolü, web rolü veya sanal makine Azure'da çalışan Tanılama verileri toplamanızı sağlayan Azure bir uzantısıdır. Veriler bir Azure depolama hesabında depolanır ve Azure İzleyici tarafından toplanabilir.

Azure İzleyici'nın bu Azure tanılama günlükleri toplamak günlükleri şu konumlarda olmalıdır:

| Günlük türü | Kaynak Türü | Konum |
| --- | --- | --- |
| IIS günlükleri |Virtual Machines <br> Web rolleri <br> Çalışan rolleri |wad-IIS-logfiles (Blob Depolama) |
| Syslog |Virtual Machines |LinuxsyslogVer2v0 (Tablo depolama) |
| Service Fabric çalışma olayları |Service Fabric düğümleri |WADServiceFabricSystemEventTable |
| Service Fabric güvenilir aktör olayları |Service Fabric düğümleri |WADServiceFabricReliableActorEventTable |
| Service Fabric Reliable Services olayları |Service Fabric düğümleri |WADServiceFabricReliableServiceEventTable |
| Windows Olay günlükleri |Service Fabric düğümleri <br> Virtual Machines <br> Web rolleri <br> Çalışan rolleri |WADWindowsEventLogsTable (Tablo depolama) |
| Windows ETW günlükleri |Service Fabric düğümleri <br> Virtual Machines <br> Web rolleri <br> Çalışan rolleri |WADETWEventTable (Tablo depolama) |

> [!NOTE]
> IIS günlüklerini Azure Web siteleri şu anda desteklenmemektedir.
>
>

Sanal makineler için yükleme seçeneğiniz [Log Analytics aracısını](../../azure-monitor/learn/quick-collect-azurevm.md) ek Öngörüler etkinleştirmek için sanal makine. IIS günlükleri ve olay günlüklerini analiz etme olanağına olmasının yanı sıra yapılandırma değişiklik izleme SQL değerlendirmesi ve güncelleştirme değerlendirmesi de dahil olmak üzere ek analiz gerçekleştirebilirsiniz.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a>Azure Tanılama'da bir sanal makine etkinleştirme koleksiyon olay günlüğü ve IIS günlüğü için

Azure Tanılama'da bir sanal makine için Microsoft Azure portalını kullanarak olay günlüğü ve IIS günlük koleksiyonu etkinleştirmek için aşağıdaki yordamı kullanın.

### <a name="to-enable-azure-diagnostics-in-a-virtual-machine-with-the-azure-portal"></a>Azure portalıyla bir sanal makinede Azure tanılamayı etkinleştirme

1. Bir sanal makine oluşturduğunuzda, VM aracısını yükleyin. Sanal makine zaten varsa, VM Aracısı zaten yüklü olduğunu doğrulayın.

   * Azure portalında sanal makineye gidin **isteğe bağlı yapılandırma**, ardından **tanılama** ayarlayıp **durumu** için **üzerinde** .

     Tamamlandıktan sonra VM yüklü ve çalışır Azure tanılama uzantısına sahiptir. Bu uzantı, tanılama verilerinin toplanması için sorumludur.
2. İzlemeyi etkinleştirmek ve olay günlüğünü var olan bir VM yapılandırın. VM düzeyinde tanılamayı etkinleştirebilirsiniz. Tanılamayı etkinleştirin ve sonra olay günlüğünü yapılandırmak için aşağıdaki adımları gerçekleştirin:

   1. VM’yi seçin.
   2. Tıklayın **izleme**.
   3. Tıklayın **tanılama**.
   4. Ayarlama **durumu** için **ON**.
   5. Toplamak istediğiniz her Tanılama Günlüğü'nü seçin.
   6. **Tamam** düğmesine tıklayın.

## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a>IIS günlük ve olay koleksiyonu için bir Web rolünde Azure tanılamayı etkinleştirin

Başvurmak [nasıl için tanılamayı etkinleştir bir bulut hizmetinde](../../cloud-services/cloud-services-dotnet-diagnostics.md) için Azure Tanılama'yı etkinleştirme hakkında daha genel adımları. Aşağıdaki yönergeler, bu bilgileri kullanın ve Log Analytics ile kullanılmak üzere özelleştirin.

Azure tanılaması etkin:

* IIS günlükler scheduledTransferPeriod aktarım aralığı günlük verileri varsayılan olarak depolanır.
* Windows olay günlükleri, varsayılan olarak aktarılmaz.

### <a name="to-enable-diagnostics"></a>Tanılamayı etkinleştirme

Windows olay günlüklerini etkinleştirmek veya scheduledTransferPeriod değiştirmek için gösterildiği gibi XML yapılandırma dosyası (diagnostics.wadcfg) kullanarak Azure tanılama yapılandırma [4. adım: Tanılama yapılandırma dosyanızı oluşturun ve uzantıyı yükleme](../../cloud-services/cloud-services-dotnet-diagnostics.md)

Aşağıdaki örnek yapılandırma dosyası, uygulama ve sistem günlüklerinden IIS günlükler ve tüm olayları toplar:

```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"
          configurationChangePollInterval="PT1M"
          overallQuotaInMB="4096">

      <Directories bufferQuotaInMB="0"
         scheduledTransferPeriod="PT10M">  
        <!-- IISLogs are only relevant to Web roles -->
        <IISLogs container="wad-iis" directoryQuotaInMB="0" />
      </Directories>

      <WindowsEventLog bufferQuotaInMB="0"
         scheduledTransferLogLevelFilter="Verbose"
         scheduledTransferPeriod="PT10M">
        <DataSource name="Application!*" />
        <DataSource name="System!*" />
      </WindowsEventLog>

    </DiagnosticMonitorConfiguration>
```

ConfigurationSettings aşağıdaki örnekteki gibi bir depolama hesabı belirttiğinden emin olun:

```xml
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

**AccountName** ve **AccountKey** değerleri, Azure portalında depolama hesabı Panosu, erişim tuşlarını Yönet altında bulunur. Protokolü için bağlantı dizesi olmalıdır **https**.

Güncelleştirilen Tanılama yapılandırmasını bulut hizmetinize uygulanır ve Azure depolama için tanılama yazıyor sonra ardından Log Analytics çalışma alanı yapılandırmaya hazır olursunuz.

## <a name="use-the-azure-portal-to-collect-logs-from-azure-storage"></a>Azure Depolama'dan günlükleri toplamak için Azure portalını kullanma

Azure İzleyici, aşağıdaki Azure Hizmetleri için günlükleri toplamak için bir Log Analytics çalışma alanı yapılandırmak için Azure portalını kullanabilirsiniz:

* Service Fabric kümeleri
* Virtual Machines
* Web/çalışan rolleri

Azure portalında Log Analytics çalışma alanınıza gidin ve aşağıdaki görevleri gerçekleştirin:

1. Tıklayın *depolama hesabı günlükleri*
2. Tıklayın *Ekle* görevi
3. Tanılama günlükleri içeren depolama hesabını seçin
   * Bu hesap, Klasik depolama hesabı veya bir Azure Resource Manager depolama hesabı olabilir.
4. Günlüklerini toplamak istediğiniz veri türünü seçin
   * Seçenekler, IIS günlükleri şunlardır; Olayları; Syslog (Linux); ETW günlükleri Service Fabric olayları
5. Kaynak değeri veri türüne göre otomatik olarak doldurulur ve değiştirilemez
6. Yapılandırmayı kaydetmek için Tamam'a tıklayın

Ek depolama hesapları ve çalışma alanınıza toplamak istediğiniz veri türleri için 2-6 adımlarını yineleyin.

Yaklaşık 30 dakika içerisinde Log Analytics çalışma alanındaki depolama hesabına ait verileri görebildiğine olursunuz. Yalnızca yapılandırma uygulandıktan sonra depolama alanına yazılan verileri görürsünüz. Çalışma alanı, önceden mevcut olan verileri depolama hesabından okumaz.

> [!NOTE]
> Portal, kaynak depolama hesabında mevcut veya yeni veriler yazılır doğrulamaz.
>
>

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a>Azure Tanılama'da bir sanal makine etkinleştirme PowerShell kullanarak koleksiyon olay günlüğü ve IIS günlüğü için

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

İçindeki adımları kullanın [Azure tanılama dizini oluşturmak için Azure İzleyici yapılandırma](powershell-workspace-configuration.md#configuring-log-analytics-workspace-to-collect-azure-diagnostics-from-storage) tablo Depolama'ya yazılan Azure tanılama verilerini okumak üzere PowerShell kullanma.

Azure PowerShell kullanarak Azure Depolama'ya yazılan olayları daha kesin olarak belirtebilirsiniz.
Daha fazla bilgi için [Azure sanal Makineler'de tanılamayı etkinleştirme](/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines).

Etkinleştirebilir ve aşağıdaki PowerShell betiğini kullanarak Azure tanılama güncelleştirin.
Bu betik bir özel günlük kaydı yapılandırmasıyla de kullanabilirsiniz.
Depolama hesabı, hizmet adı ve sanal makine adı ayarlamak için komut dosyasını değiştirin.
Betik cmdlet'leri için Klasik sanal makineleri kullanır.

Aşağıdaki kod örneği gözden geçirin, kopyalayın, gerektiği gibi değiştirin, örnek bir PowerShell komut dosyası kaydedin ve ardından komut dosyasını çalıştırın.

```powershell
    #Connect to Azure
    Add-AzureAccount

    # settings to change:
    $wad_storage_account_name = "myStorageAccount"
    $service_name = "myService"
    $vm_name = "myVM"

    #Construct Azure Diagnostics public config and convert to config format

    # Collect just system error events:
    $wad_xml_config = "<WadCfg><DiagnosticMonitorConfiguration><WindowsEventLog scheduledTransferPeriod=""PT1M""><DataSource name=""System!* "" /></WindowsEventLog></DiagnosticMonitorConfiguration></WadCfg>"

    $wad_b64_config = [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($wad_xml_config))
    $wad_public_config = [string]::Format("{{""xmlCfg"":""{0}""}}",$wad_b64_config)

    #Construct Azure diagnostics private config

    $wad_storage_account_key = (Get-AzStorageKey $wad_storage_account_name).Primary
    $wad_private_config = [string]::Format("{{""storageAccountName"":""{0}"",""storageAccountKey"":""{1}""}}",$wad_storage_account_name,$wad_storage_account_key)

    #Enable Diagnostics Extension for Virtual Machine

    $wad_extension_name = "IaaSDiagnostics"
    $wad_publisher = "Microsoft.Azure.Diagnostics"
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of the extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM
```


## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri ve Azure Hizmetleri için ölçümleri toplamak](collect-azure-metrics-logs.md) desteklenen Azure Hizmetleri.
* [Çözümlerle](../../azure-monitor/insights/solutions.md) veri Öngörüler sağlar.
* [Arama sorguları kullanılır](../../azure-monitor/log-query/log-query-overview.md) verileri çözümlemek için.