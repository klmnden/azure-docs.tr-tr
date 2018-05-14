---
title: Windows için Azure performans tanılama VM uzantısı | Microsoft Docs
description: Windows için Azure performans tanılama VM uzantısı tanıtır.
services: virtual-machines-windows'
documentationcenter: ''
author: genlin
manager: cshepard
editor: na
tags: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: 9ea7f4652aff07282c9c106f3894db807f341210
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="azure-performance-diagnostics-vm-extension-for-windows"></a>Windows için Azure performans tanılama VM uzantısı

Azure performans tanılama VM uzantısı, Windows Vm'lerden performans tanılama verisi toplama yardımcı olur. Uzantısı çözümleme yapar ve rapor bulgularını ve sanal makinede performans sorunları tanımlamaya ve çözümlemeye için öneriler sağlar. Bu uzantı adı verilen bir sorun giderme aracı yükler [PerfInsights](http://aka.ms/perfinsights).

## <a name="prerequisites"></a>Önkoşullar

Bu uzantı, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 ve Windows Server 2016 yüklenebilir. Ayrıca Windows 8.1 ve Windows 10 yüklenebilir.

## <a name="extension-schema"></a>Uzantı şeması
Aşağıdaki JSON şeması Azure performans tanılama VM uzantısı için gösterir. Bu uzantı adı ve tanılama çıkış ve raporu depolamak için bir depolama hesabı anahtarı gerektirir. Bu değerleri büyük/küçük harfe duyarlıdır. Depolama hesabı anahtarı korumalı ayarını yapılandırma içinde depolanması gerekir. Azure VM uzantısının korumalı ayarı veri şifrelenir ve hedef sanal makinede yalnızca şifresi çözülür. Unutmayın **storageAccountName** ve **storageAccountKey** büyük küçük harfe duyarlıdır. Diğer gerekli parametreleri aşağıdaki bölümünde listelenir.

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
            "storageAccountName": "[parameters('storageAccountName')]",
            "performanceScenario": "[parameters('performanceScenario')]",
            "traceDurationInSeconds": "[parameters('traceDurationInSeconds')]",
            "perfCounterTrace": "[parameters('perfCounterTrace')]",
            "networkTrace": "[parameters('networkTrace')]",
            "xperfTrace": "[parameters('xperfTrace')]",
            "storPortTrace": "[parameters('storPortTrace')]",
            "srNumber": "[parameters('srNumber')]",
            "requestTimeUtc":  "[parameters('requestTimeUtc')]"
        },
        "protectedSettings": {
            "storageAccountKey": "[parameters('storageAccountKey')]"        
        }
      }
    }
```

### <a name="property-values"></a>Özellik değerleri

|   **Ad**   |**Değer / örnek**|       **Açıklama**      |
|--------------|-------------------|----------------------------|
|apiVersion|2015-06-15|API sürümü.
|Yayımcı|Microsoft.Azure.Performance.Diagnostics|Yayımcı için ad alanı uzantı.
|type|AzurePerformanceDiagnostics|VM uzantısı türü.
|typeHandlerVersion|1.0|Uzantı işleyicisi sürümü.
|performanceScenario|temel|Veri yakalamak üzere performans senaryosu. Geçerli değerler: **temel**, **vmslow**, **geçirme**, ve **özel**.
|traceDurationInSeconds|300|İzleme, izleme seçeneklerinden herhangi birini seçtiyseniz süresi.
|perfCounterTrace|P|Performans sayacı izlemeyi Etkinleştir seçeneği. Geçerli değerler **p** veya değer boş. Bu izleme yakalamak istemiyorsanız, değeri olarak boş bırakın.
|networkTrace|n|Ağ izlemeyi etkinleştirmek için seçeneği. Geçerli değerler **n** veya değer boş. Bu izleme yakalamak istemiyorsanız, değeri olarak boş bırakın.
|xperfTrace|x|XPerf'in izlemeyi Etkinleştir seçeneği. Geçerli değerler **x** veya değer boş. Bu izleme yakalamak istemiyorsanız, değeri olarak boş bırakın.
|storPortTrace|s|StorPort izlemeyi Etkinleştir seçeneği. Geçerli değerler **s** veya değer boş. Bu izleme yakalamak istemiyorsanız, değeri olarak boş bırakın.
|srNumber|123452016365929|Destek bileti numarası, varsa. Değer olarak, yoksa boş bırakın.
|requestTimeUtc|2017-09-28T22:08:53.736Z|Geçerli tarih zamanı, Utc. Bu uzantıyı yüklemek için portalı kullanıyorsanız, bu değer sağlamanız gerekmez.
|storageAccountName|mystorageaccount|Tanılama günlüklerini ve sonuçları depolamak için depolama hesabı adı.
|storageAccountKey|lDuVvxuZB28NNP…hAiRF3voADxLBTcc==|Depolama hesabı anahtarı.

## <a name="install-the-extension"></a>Uzantıyı yükleme

Windows sanal makinelerde uzantıyı yüklemek için bu yönergeleri izleyin:

1. [Azure Portal](http://portal.azure.com)’da oturum açın.
2. Bu uzantıyı yüklemek istediğiniz sanal makineyi seçin.

    ![Vurgulanan sanal makinelerle ekran görüntüsü, Azure portalı](media/performance-diagnostics-vm-extension/select-the-virtual-machine.png)
3. Seçin **uzantıları** dikey penceresinde ve select **Ekle**.

    ![Vurgulanan Ekle ekran görüntüsü, uzantıları dikey penceresi](media/performance-diagnostics-vm-extension/select-extensions.png)
4. Seçin **Azure Performans Tanılama**hüküm ve koşulları gözden geçirme ve seçme **oluşturma**.

    ![Azure performans vurgulanmış Tanılama ile yeni ekran kaynak ekranı](media/performance-diagnostics-vm-extension/create-azure-performance-diagnostics-extension.png)
5. Yükleme için parametre değerlerini sağlayın ve seçin **Tamam** uzantıyı yüklemek için. Desteklenen senaryolar hakkında daha fazla bilgi için bkz: [PerfInsights kullanmayı](how-to-use-perfInsights.md#supported-troubleshooting-scenarios). 

    ![Genişletme iletişim kutusu, yükleyin ekran görüntüsü](media/performance-diagnostics-vm-extension/install-the-extension.png)
6. Yükleme başarılı olduğunda, bu durum belirten bir ileti görürsünüz.

    ![Ekran başarılı bir şekilde sağlanmasını iletisi](media/performance-diagnostics-vm-extension/provisioning-succeeded-message.png)

    > [!NOTE]
    > Sağlama başarılı olduktan sonra uzantısı çalıştırır. İki dakika sürer veya temel senaryo için tamamlamak için daha az. Diğer senaryolar için yükleme sırasında belirtilen süre üzerinden çalışır.

## <a name="remove-the-extension"></a>Uzantıyı kaldırın
Uzantı sanal makineden kaldırmak için aşağıdaki adımları izleyin:

1. Oturum [Azure portal](http://portal.azure.com), istediğiniz bu uzantıyı kaldırın ve ardından sanal makineyi seçin **uzantıları** dikey. 
2. Seçin (**...** ) performans tanılama uzantısını girişi listelemek ve seçmek için **kaldırma**.

    ![Ekran görüntüsü, uzantıları dikey, vurgulanmış Kaldır](media/performance-diagnostics-vm-extension/uninstall-the-extension.png)

    > [!NOTE]
    > Ayrıca, bir uzantı girişini seçin ve Seç **kaldırma** seçeneği.

## <a name="template-deployment"></a>Şablon dağıtımı
Azure sanal makine uzantıları Azure Resource Manager şablonları ile dağıtılabilir. Önceki bölümde ayrıntılı JSON şeması bir Azure Resource Manager şablonu kullanılabilir. Bu, bir Azure Resource Manager şablon dağıtımı sırasında Azure performans tanılama VM uzantısı çalıştırır. Örnek şablon şöyledir:

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
            "storageAccountName": "[parameters('storageAccountName')]",
            "performanceScenario": "[parameters('performanceScenario')]",
            "traceDurationInSeconds": "[parameters('traceDurationInSeconds')]",
            "perfCounterTrace": "[parameters('perfCounterTrace')]",
            "networkTrace": "[parameters('networkTrace')]",
            "xperfTrace": "[parameters('xperfTrace')]",
            "storPortTrace": "[parameters('storPortTrace')]",
            "srNumber": "[parameters('srNumber')]",
            "requestTimeUtc":  "[parameters('requestTimeUtc')]"
        },
        "protectedSettings": {            
            "storageAccountKey": "[parameters('storageAccountKey')]"        
        }
      }
    }
  ]
}
````

## <a name="powershell-deployment"></a>PowerShell dağıtım
`Set-AzureRmVMExtension` Komutu, Azure performans tanılama VM uzantısı olan bir sanal makineyi dağıtmak için kullanılabilir.

PowerShell

````
$PublicSettings = @{ "storageAccountName"="mystorageaccount";"performanceScenario"="basic";"traceDurationInSeconds"=300;"perfCounterTrace"="p";"networkTrace"="";"xperfTrace"="";"storPortTrace"="";"srNumber"="";"requestTimeUtc"="2017-09-28T22:08:53.736Z" }
$ProtectedSettings = @{"storageAccountKey"="mystoragekey" }

Set-AzureRmVMExtension -ExtensionName "AzurePerformanceDiagnostics" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.Azure.Performance.Diagnostics" `
    -ExtensionType "AzurePerformanceDiagnostics" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location WestUS
````

## <a name="information-on-the-data-captured"></a>Yakalanan veriler hakkında bilgi
PerfInsights aracı çeşitli günlükleri, yapılandırma ve seçilen senaryoya bağlı olarak tanılama verilerini toplar. Daha fazla bilgi için bkz: [PerfInsights belgelerine](http://aka.ms/perfinsights).

## <a name="view-and-share-the-results"></a>Görüntülemek ve sonuçları paylaşmak

Uzantı çıktısı, yükleme sırasında belirtilen depolama hesabı karşıya ve 30 gün boyunca kullanarak paylaşılan bir zip dosyasında bulunabilir [paylaşılan erişim imzaları (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md). Bu zip dosyasını tanılama günlüklerini ve raporla bulgularını ve önerileri içerir. Çıktı zip dosyası SAS bağlantı adlı bir metin dosyası içinde bulunabilir *zipfilename*klasörü altında _saslink.txt **C:\Packages\Plugins\Microsoft.Azure.Performance.Diagnostics.AzurePerformanceDiagnostics \\ \<sürüm >**. Bu bağlantıya sahip olan herkes zip dosyası indirebilirsiniz.

Üzerinde destek bileti çalışma destek mühendisine yardımcı olmak için Microsoft bu SAS bağlantı tanılama verilerini yüklemek için kullanabilirsiniz.

Raporu görüntülemek için zip dosyasını ayıklayın ve açın **PerfInsights raporu.HTML** dosya.

Zip dosyası doğrudan portalından uzantısı seçerek yükleyebilirsiniz olmalıdır.

![Performans Tanılama ekran ayrıntılı durumu](media/performance-diagnostics-vm-extension/view-detailed-status.png)

> [!NOTE]
> Portalda görüntülenen SAS bağlantı bazen çalışmayabilir. Bu tarafından hatalı biçimlendirilmiş bir URL kodlama ve kod çözme işlemleri sırasında neden olabilir. Bunun yerine, doğrudan VM'den *_saslink.txt dosyasından bağlantı alabilirsiniz.

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

- Uzantı başarıyla kaynak sağlandı olsa bile uzantı dağıtım durumunu (bildirim alanındaki) "Dağıtımı devam ediyor" gösterebilir.

    Uzantısı başarıyla hazırlandı uzantı durumunu gösteren sürece bu sorunu güvenle yoksayılabilir.
- Uzantı günlükleri'ni kullanarak yükleme sırasında bazı sorunlar ele alabilir. Uzantı yürütme çıktısı aşağıdaki dizinde bulunan dosyalara kaydedilir:

        C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Performance.Diagnostics.AzurePerformanceDiagnostics\<version>

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/)seçip **alma desteği**. Azure destek kullanma hakkında daha fazla bilgi için okuma [Microsoft Azure desteği ile ilgili SSS](https://azure.microsoft.com/support/faq/).
