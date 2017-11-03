---
title: "Windows için Azure performans tanılama VM uzantısı | Microsoft Docs"
description: "Windows için Azure performans tanılama VM uzantısı tanıtır."
services: virtual-machines-windows'
documentationcenter: 
author: genlin
manager: cshepard
editor: na
tags: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/29/2017
ms.author: genli
ms.openlocfilehash: 85d4764534c77ea0e4d999e249abe456d0234d75
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2017
---
# <a name="azure-performance-diagnostics-vm-extension-for-windows"></a>Windows için Azure performans tanılama VM uzantısı

## <a name="summary"></a>Özet
Azure performans tanılama VM uzantısı Windows Vm'lerden performans tanılama verisi toplama yardımcı olur, çözümleme yapar ve rapor bulgularını & tanımlamak ve sanal makinede performans sorunlarını gidermek için öneriler sağlar. Bu uzantı adı verilen bir sorun giderme aracı yükler [PerfInsights](http://aka.ms/perfinsights).

## <a name="prerequisites"></a>Ön koşullar
### <a name="operating-systems"></a>İşletim Sistemleri
Bu uzantı, 2016 2012 R2, 2012, Windows Server 2008 R2 üzerinde yüklenebilir; Windows 8.1 ve Windows 10 işletim sistemleri.

## <a name="extension-schema"></a>Uzantı Şeması
Aşağıdaki JSON şeması için Azure performans tanılama uzantısını gösterir. Bu uzantı adı ve depolama tanılama çıkış için bir depolama hesabı için anahtar ve rapor gerektirir. Bu değerler duyarlıdır ve korumalı ayarını yapılandırma içinde depolanması gerekir. Azure VM uzantısının korumalı ayarı veri şifrelenir ve yalnızca hedef sanal makineye şifresi. StorageAccountName ve storageAccountKey büyük küçük harfe duyarlı olduğunu unutmayın. Diğer gerekli Parametreler bölümünde listelenir.

```JSON
    {
      "name": "[concat(parameters('vmName'),'/AzurePerformanceDiagnostics')]",
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "location": "[parameters('location')]",
      "apiVersion": "2015-06-15",
      "properties": {
        "publisher": "Microsoft.Azure.Performance.Diagnostics",
        "type": "AzurePerformanceDiagnostics",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "performanceScenario": "[parameters('performanceScenario')]",
                  "traceDurationInSeconds": "[parameters('traceDurationInSeconds')]",
                  "diagnosticsTrace": "[parameters('diagnosticsTrace')]",
                  "perfCounterTrace": "[parameters('perfCounterTrace')]",
                  "networkTrace": "[parameters('networkTrace')]",
                  "xperfTrace": "[parameters('xperfTrace')]",
                  "storPortTrace": "[parameters('storPortTrace')]",
            "srNumber": "[parameters('srNumber')]",
            "requestTimeUtc":  "[parameters('requestTimeUtc')]"
        },
          "protectedSettings": {
            "storageAccountName": "[parameters('storageAccountName')]",
            "storageAccountKey": "[parameters('storageAccountKey')]"        
            }
      }
    }
```

### <a name="property-values"></a>Özellik değerleri

|   **Ad**   |**Değer / örnek**|       **Açıklama**      |
|--------------|-------------------|----------------------------|
|apiVersion|2015-06-15|API sürümü
|Yayımcı|Microsoft.Azure.Performance.Diagnostics|Yayımcı ad uzantısı
|type|AzurePerformanceDiagnostics|VM uzantısı türü
|typeHandlerVersion|1.0|Uzantı işleyicisi sürümü
|performanceScenario|Temel|Verilerini yakalamak için performans senaryosu. Geçerli değerler: **temel**, **vmslow**, **geçirme**, ve **özel**.
|traceDurationInSeconds|300|İzleme seçeneklerinden herhangi birini seçtiyseniz izlemeleri süresi.
|DiagnosticsTrace|D|Tanılama izleme etkinleştirmek için seçeneği. Geçerli değerler **d** veya değer boş. Yalnızca bu izleme yakalamak istemiyorsanız, değeri olarak boş bırakın.
|perfCounterTrace|P|Performans sayacı izlemeyi Etkinleştir seçeneği. Geçerli değerler **p** veya değer boş. Yalnızca bu izleme yakalamak istemiyorsanız, değeri olarak boş bırakın.
|networkTrace|n|Netmon izlemeyi Etkinleştir seçeneği. Geçerli değerler  **n**  veya değer boş. Yalnızca bu izleme yakalamak istemiyorsanız, değeri olarak boş bırakın.
|xperfTrace|x|XPerf'in izlemeyi Etkinleştir seçeneği. Geçerli değerler **x** veya değer boş. Yalnızca bu izleme yakalamak istemiyorsanız, değeri olarak boş bırakın.
|storPortTrace|s|StorPort izlemeyi Etkinleştir seçeneği. Geçerli değerler s ya da boş bir değer var. Yalnızca bu izleme yakalamak istemiyorsanız, değeri olarak boş bırakın.
|srNumber|123452016365929|Bilet numarası varsa destekler. Elinizde yoksa boş bırakın.
|requestTimeUtc|2/9/2017 11:06:00 PM|Geçerli tarih zamanı, Utc. Bu uzantıyı yüklemek için portalı kullanıyorsanız, bu değer sağlamanız gerekmez.
|storageAccountName|mystorageaccount|Tanılama günlüklerini ve sonuçları depolamak için depolama hesabının adı.
|storageAccountKey|lDuVvxuZB28NNP... hAiRF3voADxLBTcc ==|Depolama hesabı anahtarı.

## <a name="install-the-extension"></a>Uzantıyı yükleme

Windows sanal makinelerde VM uzantısı yüklemek için aşağıdaki adımları izleyin:

1. [Azure Portal](http://portal.azure.com) oturum açın.
2. Bu uzantıyı yüklemek istediğiniz sanal makineyi seçin.

    ![Sanal makineyi seçin](media/performance-diagnostics-vm-extension/select-the-virtual-machine.png)
3. Seçin **uzantıları** tıklayın ve dikey **Ekle** düğmesi.

    ![Uzantıları seçme](media/performance-diagnostics-vm-extension/select-extensions.png)
4. Azure performans tanılama uzantısını seçin, hüküm ve koşulları gözden geçirin ve **oluşturma** düğmesi.

    ![Azure performans tanılama uzantısı oluşturma](media/performance-diagnostics-vm-extension/create-azure-performance-diagnostics-extension.png)
5. ' I tıklatın ve yükleme için parametre değerlerini sağlayın **Tamam** uzantıyı yüklemek için. Desteklenen sorun giderme senaryoları hakkında daha fazla bilgi bulabilirsiniz [burada](how-to-use-perfInsights.md#supported-troubleshooting-scenarios). 

    ![Uzantıyı yükleme](media/performance-diagnostics-vm-extension/install-the-extension.png)
6. Yükleme başarılı olduktan sonra başarılı sağlama belirten bir ileti görürsünüz.

    ![İleti sağlama başarılı oldu](media/performance-diagnostics-vm-extension/provisioning-succeeded-message.png)

    > [!NOTE]
    > Uzantı yürütme sağlama başarılı oldu ve işlemin birkaç dakika veya temel senaryo için yürütme tamamlamak için daha az sürecek sonra başlar. Diğer senaryolar için yükleme sırasında belirtilen süre aracılığıyla çalışacaktır.

## <a name="remove-the-extension"></a>Uzantıyı kaldırın
Uzantı sanal makineden kaldırmak için aşağıdaki adımları izleyin:

1. Oturum [Azure portal](http://portal.azure.com), istediğiniz bu uzantıyı kaldırmak ve uzantıları dikey seçmek için sanal makineyi seçin. 
2. ' İ tıklatın (**...** ) performans tanılama uzantısını girişinin listesi ve Kaldır seçeneğini belirleyin.

    ![Uzantısını Kaldır](media/performance-diagnostics-vm-extension/uninstall-the-extension.png)

    > [!NOTE]
    > Ayrıca, bir uzantı girişini seçin ve Kaldır seçeneğini belirleyin.

## <a name="template-deployment"></a>Şablon dağıtımı
Azure VM uzantıları, Azure Resource Manager şablonları ile dağıtılabilir. Önceki bölümde ayrıntılı JSON şeması bir Azure Resource Manager şablonunda bir Azure Resource Manager şablon dağıtımı sırasında Azure performans tanılama uzantısını çalıştırmak için kullanılabilir. Şablon dağıtımı ile kullanılan bir örnek şablonu şöyledir:

````
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "vmName": {
      "type": "string",
      "defaultValue": "yourVMName"
    },
    "location": {
      "type": "string",
      "defaultValue": "southcentralus"
    },
    "storageAccountName": {
      "type": "securestring"
      "defaultValue": "yourStorageAccount"
    },
    "storageAccountKey": {
      "type": "securestring"
      "defaultValue": "yourStorageAccountKey"
    },
    "performanceScenario": {
      "type": "string",
      "defaultValue": "basic"
    },
    "srNumber": {
      "type": "string",
      "defaultValue": ""
    },
    "traceDurationInSeconds": {
      "type": "int",
    "defaultValue": 300
    },
    "diagnosticsTrace": {
      "type": "string",
      "defaultValue": "d"
    },
    "perfCounterTrace": {
      "type": "string",
      "defaultValue": "p"
    },
    "networkTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "xperfTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "storPortTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "requestTimeUtc": {
      "type": "string",
      "defaultValue": "10/2/2017 11:06:00 PM"
    }       
  },
  "resources": [
    {
      "name": "[concat(parameters('vmName'),'/AzurePerformanceDiagnostics')]",
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "location": "[parameters('location')]",
      "apiVersion": "2015-06-15",
      "properties": {
        "publisher": "Microsoft.Azure.Performance.Diagnostics",
        "type": "AzurePerformanceDiagnostics",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "performanceScenario": "[parameters('performanceScenario')]",
                  "traceDurationInSeconds": "[parameters('traceDurationInSeconds')]",
                  "diagnosticsTrace": "[parameters('diagnosticsTrace')]",
                  "perfCounterTrace": "[parameters('perfCounterTrace')]",
                  "networkTrace": "[parameters('networkTrace')]",
                  "xperfTrace": "[parameters('xperfTrace')]",
                  "storPortTrace": "[parameters('storPortTrace')]",
            "srNumber": "[parameters('srNumber')]",
            "requestTimeUtc":  "[parameters('requestTimeUtc')]"
        },
          "protectedSettings": {
            "storageAccountName": "[parameters('storageAccountName')]",
            "storageAccountKey": "[parameters('storageAccountKey')]"        
            }
      }
    }
  ]
}
````

## <a name="powershell-deployment"></a>PowerShell dağıtım
`Set-AzureRmVMExtension` Komutu, Azure performans tanılama sanal makine uzantısı olan bir sanal makineyi dağıtmak için kullanılabilir. Komutu çalıştırmadan önce genel ve özel yapılandırmaları bir PowerShell karma tablosunda depolanması gerekir.

PowerShell

````
$PublicSettings = @{ "performanceScenario" = "basic"; "traceDurationInSeconds" = 300; "diagnosticsTrace" = "d"; "perfCounterTrace" = "p"; "networkTrace" = ""; "xperfTrace" = ""; "storPortTrace" = ""; "srNumber" = ""; "requestTimeUtc" = "2017-09-28T22:08:53.736Z" }
$ProtectedSettings = @{"storageAccountName" = "mystorageaccount" ; "storageAccountKey" = "mystoragekey"}

Set-AzureRmVMExtension -ExtensionName "AzurePerformanceDiagnostics" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.Azure.Performance.Diagnostics" `
    -ExtensionType "AzurePerformanceDiagnostics" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location WestUS `
````

## <a name="information-on-the-data-captured"></a>Yakalanan veriler hakkında bilgi
PerfInsights aracı çeşitli günlükleri, yapılandırma, tanılama veri vb. seçilen senaryoya bağlı olarak toplar. Veriler hakkında daha fazla bilgi senaryo ziyaret Lütfen toplanan [PerfInsights belgelerine](http://aka.ms/perfinsights).

## <a name="view-and-share-the-results"></a>Görüntülemek ve sonuçları paylaşmak

Uzantı çıktısı varsayılan olarak Temp sürücüsü (genellikle D:\log_collection) altındaki log_collection adlı bir klasör içinde depolanır. Bu klasörü altında tanılama günlüklerini ve bulguları ve öneriler içeren bir rapor içeren zip dosyaların görebilirsiniz.

Oluşturulan ZIP dosyası yükleme sırasında sağlanan depolama hesabı da karşıya ve 30 gün boyunca kullanarak paylaşılan [paylaşılan erişim imzaları (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md). Adlı bir metin dosyası *zipfilename*_saslink.txt de log_collection klasöründe oluşturulur. Bu dosya zip dosyasını karşıdan yüklemek için oluşturulan SAS bağlantı içerir. Bu bağlantıya sahip olan herkes, ZIP dosyası indirebilirsiniz olacaktır.

Microsoft, destek bileti çalışma destek mühendisi tarafından daha fazla araştırma için tanılama veri indirmek için bu SAS bağlantı kullanabilir.

Raporu görüntülemek için yalnızca zip dosyasını ayıklayın ve açın **PerfInsights raporu.HTML** dosya.

ZIP dosyasının uzantısı seçerek doğrudan portalından karşıdan yüklemek mümkün olabilir.

![Ayrıntılı durumunu görüntüleme](media/performance-diagnostics-vm-extension/view-detailed-status.png)

> [!NOTE]
> Portalda görüntülenen SAS bağlantı bazen hatalı biçimlendirilmiş Url (tarafından özel karakterler neden) nedeniyle kodlama ve kod çözme işlemi sırasında çalışmayabilir. VM *_saslink.txt dosyasından doğrudan bağlantı almak için geçici bir çözüm değildir.

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek
### <a name="troubleshoot"></a>Sorun giderme

- Uzantı başarıyla kaynak sağlandı olsa bile uzantı dağıtım durumunu (bildirim alanındaki) "Dağıtımı devam ediyor" gösterebilir.

    Uzantısı başarıyla hazırlandı uzantı durumunu gösteren sürece bu sorunu güvenle yoksayılabilir.
- Yükleme sırasında bazı sorunlar olması sorun giderme uzantısı günlüklerini kullanma. Uzantı yürütme çıktısı aşağıdaki dizinde bulunan dosyalara kaydedilir:

        C:\Packages\Plugins\Microsoft.Azure.Performance.Diagnostics.AzurePerformanceDiagnostics

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Get destek seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği ile ilgili SSS](https://azure.microsoft.com/support/faq/).
