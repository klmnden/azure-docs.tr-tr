---
title: "Azure günlük analizi olayları için IIS ve tablo depolama için BLOB storage kullanma | Microsoft Docs"
description: "Günlük analizi tablo depolama için tanılama yazma Azure Hizmetleri için günlükleri veya blob depolamaya yazılabilir IIS günlüklerini okuyabilir."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: bf444752-ecc1-4306-9489-c29cb37d6045
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 459ef90ca1d76bada6565bfefd7b4bd1086197d5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-blob-storage-for-iis-and-azure-table-storage-for-events-with-log-analytics"></a>Günlük analizi olan olaylar için IIS ve Azure tablo depolaması için Azure blob storage kullanma

Günlük analizi tablo depolama için tanılama yazma aşağıdaki hizmetler için günlükleri veya blob depolamaya yazılabilir IIS günlüklerini okuyabilirsiniz:

* Service Fabric kümeleri (Önizleme)
* Virtual Machines
* Web/çalışan rolleri

Azure tanılama günlük analizi bu kaynakları için veri toplayabilmek için önce etkinleştirilmesi gerekir.

Tanılama etkinleştirildikten sonra Azure portalını kullanabilir veya PowerShell günlükleri toplamak için günlük analizi yapılandırın.

Azure tanılama çalışan rolü, web rolü ya da sanal makine Azure'da çalışan Tanılama verileri toplayacak şekilde sağlayan Azure uzantısıdır. Veriler bir Azure depolama hesabında depolanır ve günlük analizi tarafından toplanabilir.

Günlük analizi'nın bu Azure tanılama günlükleri toplamak günlükleri şu konumlarda olması gerekir:

| Günlük türü | Kaynak Türü | Konum |
| --- | --- | --- |
| IIS günlükleri |Virtual Machines <br> Web rolleri <br> Çalışan rolleri |wad IIS logfiles (Blob Depolama) |
| Syslog |Virtual Machines |LinuxsyslogVer2v0 (Tablo depolama) |
| Service Fabric çalışma olayları |Service Fabric düğümler |WADServiceFabricSystemEventTable |
| Service Fabric güvenilir aktör olayları |Service Fabric düğümler |WADServiceFabricReliableActorEventTable |
| Service Fabric güvenilir hizmeti olayları |Service Fabric düğümler |WADServiceFabricReliableServiceEventTable |
| Windows olay günlükleri |Service Fabric düğümler <br> Virtual Machines <br> Web rolleri <br> Çalışan rolleri |WADWindowsEventLogsTable (Tablo depolama) |
| Windows ETW günlükleri |Service Fabric düğümler <br> Virtual Machines <br> Web rolleri <br> Çalışan rolleri |WADETWEventTable (Tablo depolama) |

> [!NOTE]
> Azure Web siteleri IIS günlükleri şu anda desteklenmemektedir.
>
>

Sanal makineler için yükleme seçeneğiniz [günlük analizi aracı](log-analytics-azure-vm-extension.md) ek Öngörüler etkinleştirmek için sanal makinenize içine. IIS günlüklerini ve olay günlüklerini analiz edebilirsiniz olmaya ek olarak, izleme yapılandırma değişikliği, SQL değerlendirmesi ve güncelleştirme değerlendirme dahil olmak üzere ek analiz gerçekleştirebilir.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a>Bir sanal makinede Azure tanılamayı etkinleştirin koleksiyonu olay günlüğünü ve IIS günlüğü için
Bir sanal makinede Microsoft Azure Portalı'nı kullanarak olay günlüğünü ve IIS günlük toplama için Azure Tanılama'yı etkinleştirmek için aşağıdaki yordamı kullanın.

### <a name="to-enable-azure-diagnostics-in-a-virtual-machine-with-the-azure-portal"></a>Bir sanal makinede Azure portal ile Azure tanılama etkinleştirmek için
1. Bir sanal makine oluşturduğunuzda VM aracısı yükleyin. Sanal makine zaten varsa, VM Aracısı zaten yüklü olduğunu doğrulayın.

   * Azure portalında sanal makineye gidin **isteğe bağlı yapılandırma**, ardından **tanılama** ve **durum** için **üzerinde**.

     Tamamlandığında, VM yüklü ve çalışan Azure tanılama uzantısına sahiptir. Tanılama verilerinin toplanması için bu uzantıyı sorumludur.
2. İzlemeyi etkinleştirmek ve olay günlüğü var olan bir VM yapılandırın. Tanılama VM düzeyinde etkinleştirebilirsiniz. Tanılamayı etkinleştirin ve olay günlüğünü yapılandırmak için aşağıdaki adımları gerçekleştirin:

   1. VM seçin.
   2. Tıklatın **izleme**.
   3. Tıklatın **tanılama**.
   4. Ayarlama **durum** için **ON**.
   5. Toplamak istediğiniz her Tanılama Günlüğü'nü seçin.
   6. **Tamam** düğmesine tıklayın.

## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a>Web rolü IIS günlüğü ve olay toplama için Azure tanılama etkinleştir
Başvurmak [nasıl için tanılamayı etkinleştir bir bulut hizmetinde](../cloud-services/cloud-services-dotnet-diagnostics.md) Azure tanılama etkinleştirme genel adımlar için. Aşağıdaki yönergelerde bu bilgileri kullanın ve günlük analizi ile kullanılmak üzere özelleştirin.

Azure Tanılama ile etkin:

* IIS günlüklerini scheduledTransferPeriod aktarımı aralıklarla günlük verileri varsayılan olarak depolanır.
* Windows olay günlüklerini varsayılan olarak aktarılmaz.

### <a name="to-enable-diagnostics"></a>Tanılama etkinleştirmek için
Windows olay günlüklerini etkinleştirmek ya da scheduledTransferPeriod değiştirmek için XML yapılandırma dosyası (diagnostics.wadcfg) kullanarak Azure tanılama gösterildiği gibi yapılandırmak [4. adım: Tanılama yapılandırma dosyanızı oluşturun ve uzantısını yükle](../cloud-services/cloud-services-dotnet-diagnostics.md)

Aşağıdaki örnek yapılandırma dosyası, uygulama ve sistem günlüklerini IIS günlüklerini ve tüm olayları toplar:

```
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

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

**AccountName** ve **AccountKey** değerleri depolama hesabı panosundaki erişim anahtarlarını Yönet altında Azure portalında bulundu. Bağlantı dizesi için Protokolü olmalıdır **https**.

Güncelleştirilmiş tanılama yapılandırması bulut hizmetinize uygulanır ve Azure depolama için tanılama yazıyor sonra daha sonra günlük analizi yapılandırmaya hazırsınız demektir.

## <a name="use-the-azure-portal-to-collect-logs-from-azure-storage"></a>Azure depolama biriminden günlükleri toplamak için Azure portalını kullanma
Aşağıdaki Azure Hizmetleri için günlükleri toplamak için günlük analizi yapılandırmak için Azure portalını kullanabilirsiniz:

* Service Fabric kümeleri
* Virtual Machines
* Web/çalışan rolleri

Azure portalında günlük analizi çalışma alanına gidin ve aşağıdaki görevleri gerçekleştirin:

1. Tıklatın *depolama hesapları günlükleri*
2. Tıklatın *Ekle* görevi
3. Tanılama günlükleri içeren depolama hesabı seçin
   * Bu hesap, Klasik depolama hesabı veya bir Azure Resource Manager depolama hesabı olabilir.
4. Günlüklerini toplamak istediğiniz veri türünü seçin
   * IIS günlüklerini seçimlerdir; Olayları; Syslog (Linux); ETW günlükleri; Hizmeti yapı olayları
5. Kaynak için değer veri türüne göre otomatik olarak doldurulur ve değiştirilemez
6. Yapılandırmayı kaydetmek için Tamam'ı tıklatın

Ek depolama hesapları ve toplamak için günlük analizi istediğiniz veri türleri için 2-6 adımlarını yineleyin.

Yaklaşık 30 dakika içinde günlük analizi depolama hesabında verileri görmek kullanabilirsiniz. Yalnızca yapılandırma uygulandıktan sonra depolama alanına yazılır veri görürsünüz. Günlük analizi depolama hesabından önceden mevcut verileri okuyamadı.

> [!NOTE]
> Portal kaynağı depolama hesabında yok veya yeni veriler yazılır doğrulamaz.
>
>

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a>Bir sanal makinede Azure tanılamayı etkinleştirin koleksiyonu PowerShell kullanarak olay günlüğünü ve IIS günlüğü için
İçindeki adımları kullanın [Azure tanılama dizini oluşturmak için günlük analizi yapılandırma](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) tablo depolama alanına yazılır Azure tanılama okuma için PowerShell kullanma.

Azure PowerShell kullanarak Azure depolama alanına yazılır olayları daha kesin olarak belirtebilirsiniz.
Daha fazla bilgi için bkz: [etkinleştirme tanılama Azure Virtual Machines'de](../virtual-machines-dotnet-diagnostics.md).

Etkinleştirme ve aşağıdaki PowerShell Betiği kullanılarak Azure tanılama güncelleştirin.
Bu komut dosyasını bir özel günlük kaydı yapılandırmasıyla de kullanabilirsiniz.
Depolama hesabı, hizmet adı ve sanal makine adı ayarlamak için komut dosyasını değiştirin.
Komut dosyası Klasik sanal makineleri için cmdlet'leri kullanır.

Aşağıdaki betik örneğinde gözden geçirin, kopyalayın, gerektiği şekilde değiştirin, örnek bir PowerShell komut dosyası olarak kaydetmek ve sonra komut dosyasını çalıştırın.

```
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

    $wad_storage_account_key = (Get-AzureStorageKey $wad_storage_account_name).Primary
    $wad_private_config = [string]::Format("{{""storageAccountName"":""{0}"",""storageAccountKey"":""{1}""}}",$wad_storage_account_name,$wad_storage_account_key)

    #Enable Diagnostics Extension for Virtual Machine

    $wad_extension_name = "IaaSDiagnostics"
    $wad_publisher = "Microsoft.Azure.Diagnostics"
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of the extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM
```


## <a name="next-steps"></a>Sonraki adımlar
* [Günlük ve Azure Hizmetleri için ölçümleri toplamak](log-analytics-azure-storage.md) desteklenen Azure Hizmetleri.
* [Çözümlerle](log-analytics-add-solutions.md) verileri bir anlayış sağlamak için.
* [Arama sorguları kullanmak](log-analytics-log-searches.md) verileri çözümlemek için.
