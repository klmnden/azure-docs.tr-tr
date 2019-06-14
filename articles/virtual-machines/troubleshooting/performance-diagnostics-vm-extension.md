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
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 769305cc3d838832f8f445ac9623a1724603f968
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60307936"
---
# <a name="azure-performance-diagnostics-vm-extension-for-windows"></a>Windows için Azure performans tanılama VM uzantısı

Azure performans tanılama VM uzantısı, Windows sanal makinelerinden performans tanılama verilerini toplama yardımcı olur. Uzantı analiz gerçekleştirir ve bulguları ve öneriler belirlemek ve sanal makine üzerindeki performans sorunlarını çözmek için bir rapor sağlar. Bir sorun giderme aracı olarak adlandırılan bu uzantıyı yükler [Perfınsights](https://aka.ms/perfinsights).

> [!NOTE]
> Tanılama sanal makinenizde Azure portalında Klasik olmayan VM'ler için çalıştırmak istiyorsanız, yeni deneyimi kullanmak için önerilir. Daha fazla bilgi için [Azure sanal makineler için performans tanılama](performance-diagnostics.md) 

## <a name="prerequisites"></a>Önkoşullar

Bu uzantı, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 ve Windows Server 2016 üzerinde yüklenebilir. Ayrıca Windows 8.1 ve Windows 10'da yüklenebilir.

## <a name="extension-schema"></a>Uzantı şeması
Aşağıdaki JSON şema Azure performans tanılama VM uzantısı için gösterir. Bu uzantı, rapor ve tanılama çıkışı depolamak bir depolama hesabı için anahtarı ve adını gerektirir. Bu değerler büyük/küçük harfe duyarlıdır. Depolama hesabı anahtarı içinde korumalı ayar bir yapılandırma depolanması gerekir. Azure VM uzantısının korumalı ayarı veriler şifrelenir ve yalnızca hedef sanal makinede şifresi çözülür. Unutmayın **storageAccountName** ve **storageAccountKey** büyük küçük harfe duyarlıdır. Diğer gerekli parametreleri aşağıdaki bölümünde listelenir.

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
            "requestTimeUtc":  "[parameters('requestTimeUtc')]",
            "resourceId": "[resourceId('Microsoft.Compute/virtualMachines', parameters('vmName'))]"
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
|publisher|Microsoft.Azure.Performance.Diagnostics|Uzantı yayımcısı ad alanı.
|türü|AzurePerformanceDiagnostics|VM uzantısı türü.
|typeHandlerVersion|1.0|Uzantı işleyici sürümü.
|performanceScenario|Temel|Veri yakalamak istediğiniz performans senaryo. Geçerli değerler: **temel**, **vmslow**, **depolamasını Azure dosyalarına**, ve **özel**.
|traceDurationInSeconds|300|İzleme, izleme seçeneklerden herhangi biri seçtiyseniz süresi.
|perfCounterTrace|p|Performans sayacını izlemeyi etkinleştirmek için seçenek. Geçerli değerler **p** veya değer boş. Bu izleme yakalamak istemiyorsanız değer olarak boş bırakın.
|networkTrace|n|Ağ izlemeyi etkinleştirmek için seçenek. Geçerli değerler **n** veya değer boş. Bu izleme yakalamak istemiyorsanız değer olarak boş bırakın.
|xperfTrace|x|XPerf izlemeyi etkinleştirmek için seçenek. Geçerli değerler **x** veya değer boş. Bu izleme yakalamak istemiyorsanız değer olarak boş bırakın.
|storPortTrace|s|StorPort izlemeyi etkinleştirmek için seçenek. Geçerli değerler **s** veya değer boş. Bu izleme yakalamak istemiyorsanız değer olarak boş bırakın.
|srNumber|123452016365929|Destek bileti numarası, varsa. Değeri, yoksa boş bırakın.
|requestTimeUtc|2017-09-28T22:08:53.736Z|Geçerli tarih saat (Utc). Bu uzantıyı yüklemek için portalı kullanıyorsanız, bu değer sağlamanız gerekmez.
|resourceId|/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}|Bir VM benzersiz tanımlayıcısı.
|storageAccountName|mystorageaccount|Tanılama günlükleri ve sonuçlarını depolamak için depolama hesabı adı.
|storageAccountKey|lDuVvxuZB28NNP…hAiRF3voADxLBTcc==|Depolama hesabı anahtarı.

## <a name="install-the-extension"></a>Uzantıyı yükleme

Windows sanal makinelerinde uzantıyı yüklemek için aşağıdaki yönergeleri izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Bu uzantıyı yüklemek istediğiniz sanal makineyi seçin.

    ![Azure portalının ekran görüntüsü, sanal makinelerle vurgulanmış](media/performance-diagnostics-vm-extension/select-the-virtual-machine.png)
3. Seçin **uzantıları** dikey penceresinde ve select **Ekle**.

    ![Ekran görüntüsü, uzantıları dikey penceresinde vurgulanır Ekle](media/performance-diagnostics-vm-extension/select-extensions.png)
4. Seçin **Azure Performans Tanılama**, hüküm ve koşulları gözden geçirin ve seçin **Oluştur**.

    ![Vurgulanmış olan Azure Performans Tanılama ile yeni ekran kaynak ekranı](media/performance-diagnostics-vm-extension/create-azure-performance-diagnostics-extension.png)
5. Yükleme için parametre değerlerini sağlayın ve seçin **Tamam** uzantıyı yüklemek için. Desteklenen senaryolar hakkında daha fazla bilgi için bkz: [Perfınsights kullanma](how-to-use-perfInsights.md#supported-troubleshooting-scenarios). 

    ![Ekran yükleme genişletme iletişim kutusu](media/performance-diagnostics-vm-extension/install-the-extension.png)
6. Yükleme başarılı olduğunda, bu durum belirten bir ileti görürsünüz.

    ![İleti başarılı oldu, ekran görüntüsü hazırlama](media/performance-diagnostics-vm-extension/provisioning-succeeded-message.png)

    > [!NOTE]
    > Uzantı sağlama başarılı olduğunda çalışır. İki dakika sürdüğünü veya tamamlamak için temel senaryo için daha az. Diğer senaryolar için yükleme sırasında belirtilen süresi çalıştırır.

## <a name="remove-the-extension"></a>Uzantıyı kaldırın
Uzantıyı sanal makineden kaldırmak için bu adımları izleyin:

1. Oturum [Azure portalında](https://portal.azure.com), bu uzantıyı kaldırın ve ardından istediğiniz sanal makineyi seçin **uzantıları** dikey penceresi. 
2. Seçin ( **...** ) seçin ve liste performans tanılama uzantısını girişinin **kaldırma**.

    ![Ekran görüntüsü, uzantıları dikey penceresinde vurgulanır Kaldır](media/performance-diagnostics-vm-extension/uninstall-the-extension.png)

    > [!NOTE]
    > Ayrıca, bir uzantı girişini seçin ve seçin **kaldırma** seçeneği.

## <a name="template-deployment"></a>Şablon dağıtımı
Azure sanal makine uzantıları, Azure Resource Manager şablonları ile dağıtılabilir. Önceki bölümde açıklanan JSON şema, bir Azure Resource Manager şablonu kullanılabilir. Bu, bir Azure Resource Manager şablon dağıtımı sırasında Azure performans tanılama VM uzantısı çalıştırır. Örnek şablonu aşağıda verilmiştir:

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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
            "requestTimeUtc":  "[parameters('requestTimeUtc')]",
            "resourceId": "[resourceId('Microsoft.Compute/virtualMachines', parameters('vmName'))]"
        },
        "protectedSettings": {
            "storageAccountKey": "[parameters('storageAccountKey')]"
        }
      }
    }
  ]
}
```

## <a name="powershell-deployment"></a>PowerShell dağıtım
`Set-AzVMExtension` Komutu, Azure performans tanılama VM uzantısı için mevcut bir sanal makine dağıtmak için kullanılabilir.

PowerShell

```
$PublicSettings = @{ "storageAccountName"="mystorageaccount";"performanceScenario"="basic";"traceDurationInSeconds"=300;"perfCounterTrace"="p";"networkTrace"="";"xperfTrace"="";"storPortTrace"="";"srNumber"="";"requestTimeUtc"="2017-09-28T22:08:53.736Z";"resourceId"="VMResourceId" }
$ProtectedSettings = @{"storageAccountKey"="mystoragekey" }

Set-AzVMExtension -ExtensionName "AzurePerformanceDiagnostics" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.Azure.Performance.Diagnostics" `
    -ExtensionType "AzurePerformanceDiagnostics" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location WestUS
```

## <a name="information-on-the-data-captured"></a>Yakalanan veriler hakkında bilgi
Perfınsights araç çeşitli günlükler, yapılandırma ve seçilen senaryoya bağlı olarak, tanılama verilerini toplar. Daha fazla bilgi için [Perfınsights belgeleri](https://aka.ms/perfinsights).

## <a name="view-and-share-the-results"></a>Görüntüleyebilir ve sonuçları paylaşın

Çıkış uzantı yüklemesi sırasında belirtilen depolama hesabı için yüklenmiş ve 30 gün boyunca kullanarak paylaşılan bir zip dosyası bulunabilir [paylaşılan erişim imzaları (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md). Bu zip dosyası, tanılama günlükleri ve bulguları ve öneriler içeren bir rapor içerir. Çıkış zip dosyası için bir SAS bağlantı adlı bir metin dosyası içinde bulunabilir *zipfilename*klasörü altında _saslink.txt **C:\Packages\Plugins\Microsoft.Azure.Performance.Diagnostics.AzurePerformanceDiagnostics \\ \<sürüm >** . Bu bağlantıya sahip olan herkes, zip dosyası indirebilirsiniz.

Destek biletiniz çalışma destek mühendisi yardımcı olmak için Microsoft bu SAS bağlantısı Tanılama verileri indirmek için kullanabilirsiniz.

Raporu görüntülemek için zip dosyasını ayıklayın ve açık **Perfınsights raporu.HTML** dosya.

Zip dosyası uzantısı seçerek doğrudan portaldan indirebilmek için olmalıdır.

![Performans Tanılama ekran ayrıntılı durumu](media/performance-diagnostics-vm-extension/view-detailed-status.png)

> [!NOTE]
> Portalda görüntülenen SAS bağlantısı bazen çalışmayabilir. Bu kodlama ve kod çözme işlemleri sırasında hatalı biçimlendirilmiş bir URL tarafından kaynaklanabilir. Bunun yerine, doğrudan VM'den *_saslink.txt dosyasından bağlantı elde edebilirsiniz.

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

- Uzantı başarıyla sağlanmış olsa bile uzantı dağıtım durumu (bildirim alanındaki) "Dağıtım devam ediyor" gösterebilir.

    Uzantı başarıyla hazırlanana uzantı durumunun gösterir sürece bu sorunu güvenle yoksayılabilir.
- Uzantı günlükleri'ni kullanarak yükleme sırasında bazı sorunlar ele alabilirsiniz. Uzantı yürütme çıktısı aşağıdaki dizinde bulunan dosyalara kaydedilir:

        C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Performance.Diagnostics.AzurePerformanceDiagnostics\<version>

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/)seçip **Destek**. Azure desteği'ni kullanma hakkında daha fazla bilgi için okuma [Microsoft Azure desteği SSS](https://azure.microsoft.com/support/faq/).
