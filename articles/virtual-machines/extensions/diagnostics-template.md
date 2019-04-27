---
title: Bir Azure sanal makinesi için izleme ve tanılama ekleyin | Microsoft Docs
description: Azure tanılama uzantısı ile yeni bir Windows sanal makine oluşturmak için bir Azure Resource Manager şablonu kullanın.
services: virtual-machines-windows
documentationcenter: ''
author: sbtron
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 8cde8fe7-977b-43d2-be74-ad46dc946058
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: saurabh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 00b4a145da9104cab410c5a07f6d7ec5ded5c45d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60800006"
---
# <a name="use-monitoring-and-diagnostics-with-a-windows-vm-and-azure-resource-manager-templates"></a>İzleme ve tanılama bir Windows VM ve Azure Resource Manager şablonları ile kullanma
Azure tanılama uzantısı, bir Windows tabanlı Azure sanal makinesinde izleme ve tanılama özellikleri sağlar. Bu özellikler sanal makinede uzantı Azure Resource Manager şablonunun bir parçası dahil ederek etkinleştirebilirsiniz. Bkz: [VM uzantıları içeren Azure Resource Manager şablonları yazma](../windows/template-description.md#extensions) uzantıyı, bir sanal makine şablonunun bir parçası dahil olmak üzere daha fazla bilgi için. Bu makalede, Azure tanılama uzantısını bir windows sanal makine şablonu nasıl ekleyebileceğinizi açıklar.  

## <a name="add-the-azure-diagnostics-extension-to-the-vm-resource-definition"></a>Azure tanılama uzantısını VM kaynak tanımına ekleyin.
Bir Windows sanal makine üzerinde tanılama uzantısını etkinleştirmek için VM kaynak Resource Manager şablonu olarak uzantısı eklemeniz gerekir.

İçin basit bir Resource Manager tabanlı sanal makine eklemek için uzantı Yapılandırması *kaynakları* dizi sanal makine için: 

```json
"resources": [
    {
        "name": "Microsoft.Insights.VMDiagnosticsSettings",
        "type": "extensions",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-06-15",
        "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
        ],
        "tags": {
            "displayName": "AzureDiagnostics"
        },
        "properties": {
            "publisher": "Microsoft.Azure.Diagnostics",
            "type": "IaaSDiagnostics",
            "typeHandlerVersion": "1.5",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), variables('vmName'), variables('wadcfgxend')))]",
                "storageAccount": "[parameters('existingdiagnosticsStorageAccountName')]"
            },
            "protectedSettings": {
                "storageAccountName": "[parameters('existingdiagnosticsStorageAccountName')]",
                "storageAccountKey": "[listkeys(variables('accountid'), '2015-05-01-preview').key1]",
                "storageAccountEndPoint": "https://core.windows.net"
            }
        }
    }
]
```

Başka bir genel kural uzantı Yapılandırması altında sanal makinenin kaynakları düğümü tanımlamak yerine şablon kök kaynak düğümde eklemektir. Bu yaklaşımda, uzantısı ile sanal makine arasındaki hiyerarşik bir ilişki açıkça belirtmek zorunda *adı* ve *türü* değerleri. Örneğin: 

```json
"name": "[concat(variables('vmName'),'Microsoft.Insights.VMDiagnosticsSettings')]",
"type": "Microsoft.Compute/virtualMachines/extensions",
```

Uzantının her zaman sanal makineyle ilişkili olduğundan, ya da doğrudan, sanal makinenin kaynak düğümü altında doğrudan tanımlayabilir veya temel düzeyde tanımlayın ve sanal makine ile ilişkilendirilecek hiyerarşik bir adlandırma kuralını kullanın.

Sanal makine ölçek kümeleri için uzantıları yapılandırma belirtilen *extensionprofile öğesine* özelliği *VirtualMachineProfile*.

*Yayımcı* özellik değeriyle **Microsoft.Azure.Diagnostics** ve *türü* özellik değeriyle **IaaSDiagnostics**Azure tanılama uzantısını benzersiz olarak tanımlanabilmesi.

Değerini *adı* özelliği, kaynak grubundaki uzantısı başvurmak için kullanılabilir. Özellikle çok ayar **Microsoft.Insights.VMDiagnosticsSettings** kolayca izleme show yukarı doğru Azure portalında grafikleri, sağlama Azure portal tarafından tanımlanması etkinleştirir.

*TypeHandlerVersion* kullanmak istediğiniz uzantısı sürümünü belirtir. Ayarı *autoUpgradeMinorVersion* için alt sürüm **true** kullanılabilir uzantısı küçük en son sürümünü elde etmeniz sağlanır. Her zaman ayarlamanız önerilir *autoUpgradeMinorVersion* her zaman olacak şekilde **true** böylece her zaman tüm yeni özellikler ve hata düzeltmeleri ile en son kullanılabilir tanılama uzantısını kullanmak alın. 

*Ayarları* öğesi ayarlayın ve tekrar (bazen genel yapılandırma olarak adlandırılır) uzantısı okuma uzantısı için yapılandırma özelliklerini içerir. *Xmlcfg* özelliği içeren xml tabanlı yapılandırma için tanılama günlükleri tanılama aracısı tarafından toplanan performans sayaçlarını vs. Bkz: [tanılama yapılandırma şeması](https://msdn.microsoft.com/library/azure/dn782207.aspx) xml şeması hakkında daha fazla bilgi. Yaygın Azure Resource Manager şablonu bir değişken olarak gerçek xml yapılandırmasını depolamak ve ardından birleştirmek için bir uygulamadır ve base64 kodlama değeri ayarlamak için bunları *xmlcfg*. Bölümüne [tanılama yapılandırma değişkenleri](#diagnostics-configuration-variables) için xml değişkenleri içinde saklamak hakkında daha fazla bilgi edinin. *StorageAccount* özelliği için hangi tanılama veri aktarılır depolama hesabının adını belirtir. 

Özelliklerinde *protectedSettings* (bazen başvurulan özel olarak yapılandırma) ayarlanabilir, ancak geri ayarlanan sonra okunamıyor. Salt yazılır doğasını *protectedSettings* yararlı gizli dizileri gibi depolama hesabı anahtarını depolamak için tanılama veri yazıldığı kolaylaştırır.    

## <a name="specifying-diagnostics-storage-account-as-parameters"></a>Tanılama depolama hesabı parametreleri olarak belirtme
Yukarıdaki tanılama uzantısı json parçacığı, iki parametre kabul eder *existingdiagnosticsStorageAccountName* ve *existingdiagnosticsStorageResourceGroup* tanılama depolama belirtmek için Tanılama verilerinin nerede depolanacağını hesabı. Tanılama depolama hesabı belirten bir parametre farklı ortamlar genelinde tanılama depolama hesabı değiştirmek kolaylaştırır gibi örneğin test etmek için farklı tanılama depolama hesabı ve farklı bir için kullanmak istediğiniz, Üretim dağıtımı.  

```json
"existingdiagnosticsStorageAccountName": {
    "type": "string",
    "metadata": {
"description": "The name of an existing storage account to which diagnostics data is transfered."
    }
},
"existingdiagnosticsStorageResourceGroup": {
    "type": "string",
    "metadata": {
"description": "The resource group for the storage account specified in existingdiagnosticsStorageAccountName"
    }
}
```

Sanal makine için kaynak grubundan farklı bir kaynak grubunda bir tanılama depolama hesabı belirtmek için iyi bir uygulamadır. Bir kaynak grubu, kendi yaşam süresi ile bir dağıtım birimi olarak kabul edilebilir, bir sanal makine dağıtılabilir ve kendisine yapılan yeni yapılandırmalar güncelleştirmeleri ancak arasında aynı depolama hesabındaki tanılama verilerini depolamak isteyebileceğiniz yeniden dağıtıldı Bu sanal makine dağıtımları. Depolama hesabı farklı bir kaynağa sahip çeşitli sürümleri arasında sorunlarını gidermek kolaylaştıran çeşitli sanal makine dağıtımları verileri kabul etmek için depolama hesabı sağlar.

> [!NOTE]
> Visual Studio'dan bir windows sanal makine şablonu oluşturursanız, varsayılan depolama hesabını burada sanal makine VHD karşıya aynı depolama hesabını kullanmak için ayarlanmış olabilir. Bu sanal makinenin ilk kurulum kolaylaştırmaktır. İçindeki bir parametre olarak geçirilebilen farklı bir depolama hesabı kullanılacak şablonu yeniden faktörü. 
> 
> 

## <a name="diagnostics-configuration-variables"></a>Tanılama yapılandırma değişkenleri
Önceki tanılama uzantısı json parçacığı tanımlayan bir *AccountID* tanılama depolama için depolama hesabı anahtarını alma basitleştirmek için değişkeni:   

```json
"accountid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/',parameters('existingdiagnosticsStorageResourceGroup'), '/providers/','Microsoft.Storage/storageAccounts/', parameters('existingdiagnosticsStorageAccountName'))]"
```

*Xmlcfg* tanılama uzantısını özelliği birlikte birleştirilmiş birden fazla değişken kullanılarak tanımlanır. Bu değişkenlerin değerleri xml biçiminde olduğundan doğru json değişkenleri ayarlarken Atlanan gerekir.

Aşağıdaki örnek, bazı windows olay günlükleri ve Tanılama Altyapısı günlükleri ile birlikte standart sistem düzeyi performans sayaçlarını toplayan tanılama yapılandırma XML'i açıklar. Bu olduğundan kaçış ve böylece yapılandırmasını, şablon değişkenleri bölümüne doğrudan yapıştırılabilir hatalı biçimlendirilmiş. Bkz: [tanılama yapılandırma şeması](https://msdn.microsoft.com/library/azure/dn782207.aspx) yapılandırma XML'i daha fazla insan tarafından okunabilir bir örneğin.

```json
"wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
"wadperfcounters1": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Privileged Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU privileged time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% User Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU user time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor Information(_Total)\\Processor Frequency\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"CPU frequency\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\System\\Processes\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Processes\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Threads\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Handle Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Handles\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\% Committed Bytes In Use\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Memory usage\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Available Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory available\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Committed Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory committed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Commit Limit\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory commit limit\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active time\" locale=\"en-us\"/></PerformanceCounterConfiguration>",
"wadperfcounters2": "<PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Read Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active read time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Write Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active write time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Transfers/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Reads/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk read operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Writes/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk write operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Read Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk read speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Write Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk write speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\LogicalDisk(_Total)\\% Free Space\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk free space (percentage)\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
"wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters1'), variables('wadperfcounters2'), '<Metrics resourceId=\"')]",
"wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name , '/providers/', 'Microsoft.Compute/virtualMachines/')]",
"wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>"
```

Yukarıdaki yapılandırma ölçüm tanımı xml düğümünde nasıl performans sayaçlarını önceki xml dosyasında tanımlanan tanımlar önemli yapılandırma öğesi aynıdır *PerformanceCounter* düğüm toplanır ve depolanır. 

> [!IMPORTANT]
> Bu ölçümler, Azure portalında izleme grafikleri ve Uyarıları sürücü.  **Ölçümleri** düğümle *ResourceId* ve **MetricAggregation** verilerinde izleme VM görmek istiyorsanız, VM tanılama yapılandırmasında eklenmelidir Azure portalı. 
> 
> 

Aşağıdaki örnek, ölçüm tanımları için xml gösterir: 

```xml
<Metrics resourceId="/subscriptions/subscription().subscriptionId/resourceGroups/resourceGroup().name/providers/Microsoft.Compute/virtualMachines/vmName">
    <MetricAggregation scheduledTransferPeriod="PT1H"/>
    <MetricAggregation scheduledTransferPeriod="PT1M"/>
</Metrics>
```

*ResourceId* özniteliği, aboneliğinizdeki sanal makineyi benzersiz olarak tanımlar. Şablonu dağıtım yaptığınız kaynak grubuna ve aboneliğe göre bu değerleri otomatik olarak güncelleştirilir. böylece subscription() ve resourceGroup() işlevleri kullandığınızdan emin olun.

Bir döngüde birden çok sanal makine oluşturuyorsanız, doldurmak zorunda *ResourceId* tek tek her VM doğru bir şekilde ayırt etmek için bir copyındex () işlevi ile değeri. *XmlCfg* değeri, bunu şu şekilde desteklemek için güncelleştirilebilir:  

```json
"xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), concat(parameters('vmNamePrefix'), copyindex()), variables('wadcfgxend')))]", 
```

MetricAggregation değerini *PT1M* ve *PT1H* sırasıyla bir dakika içinde bir toplama ve bir saat içinde bir toplama türünü belirtir.

## <a name="wadmetrics-tables-in-storage"></a>Depolama WADMetrics tablolarında
Yukarıdaki ölçümleri yapılandırma, tanılama depolama hesabınızda aşağıdaki adlandırma kuralları ile tablolar oluşturur:

* **WADMetrics**: Tüm WADMetrics tablolar için standart ön eki
* **PT1H** veya **PT1M**: Tablo üzerinde 1 saat veya 1 dakika veri toplama içerdiğini gösterir.
* **P10D**: Tablo veri toplama başladığında tablo verilerini 10 gün için içerecek gösterir
* **V2S**: Dize sabiti
* **yyyymmdd**: Hangi veri toplamayı tablo başlangıç tarihi

Örnek: *WADMetricsPT1HP10DV2S20151108* üzerinde 11-Kasım 2015'ten 10 gün boyunca bir saat içinde toplanan ölçümler verileri içerir    

Her WADMetrics tablo şu sütunları içerir:

* **PartitionKey**: Bölüm anahtarına göre oluşturulan *ResourceId* VM kaynağı benzersiz şekilde tanımlamak için değeri. Örneğin, `002Fsubscriptions:<subscriptionID>:002FresourceGroups:002F<ResourceGroupName>:002Fproviders:002FMicrosoft:002ECompute:002FvirtualMachines:002F<vmName>`  
* **RowKey**: Biçim izleyen `<Descending time tick>:<Performance Counter Name>`. Azalan zaman değer çizgisi hesaplaması toplama dönemini başlangıcının zaman eksi maksimum süre ticks ' dir. Örneğin örnek süre 10-Kasım-2015'te başlatılan ve 00:00Hrs UTC sonra hesaplama olacaktır: `DateTime.MaxValue.Ticks - (new DateTime(2015,11,10,0,0,0,DateTimeKind.Utc).Ticks)`. İçin kullanılabilir bellek bayt performans sayacı satır anahtarı şöyle görünecektir: `2519551871999999999__:005CMemory:005CAvailable:0020Bytes`
* **CounterName**: Performans sayacı adıdır. Bu eşleşen *counterSpecifier* xml yapılandırması tanımlanmış.
* **En fazla**: Performans sayacı toplama dönemdeki maksimum değer.
* **En az**: Performans sayacı toplama süre minimum değer.
* **Toplam**: Toplama süresi içinde performans sayacı tüm değerlerin toplamını bildirdi.
* **Sayısı**: Performans sayacı için bildirilen değerleri toplam sayısı.
* **Ortalama**: Performans sayacı toplama dönemdeki ortalama (toplam /) değeri.

## <a name="next-steps"></a>Sonraki Adımlar
* Tanılama uzantısı ile Windows sanal makine için bir tam örnek şablon bkz [201-vm-izleme-tanılama-extension](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-monitoring-diagnostics-extension)   
* Azure Resource Manager şablonu kullanarak dağıtma [Azure PowerShell](../windows/ps-template.md) veya [Azure komut satırı](../linux/create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* Daha fazla bilgi edinin [Azure Resource Manager şablonları yazma](../../resource-group-authoring-templates.md)
