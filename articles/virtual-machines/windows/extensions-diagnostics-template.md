---
title: Bir Azure sanal makinesi için izleme ve tanılama ekleme | Microsoft Docs
description: Azure tanılama uzantısını ile yeni bir Windows sanal makine oluşturmak için Azure Resource Manager şablonunu kullanın.
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
ms.openlocfilehash: e54060769f19546ad3ccb8c52df928eeebf03776
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="use-monitoring-and-diagnostics-with-a-windows-vm-and-azure-resource-manager-templates"></a>İzleme ve tanılama Windows VM ve Azure Resource Manager şablonları ile kullanın.
Azure tanılama uzantısını bir Windows tabanlı Azure sanal makinede izleme ve tanılama yetenekleri sağlar. Bu özellikler sanal makinede uzantı Azure Resource Manager şablonu bir parçası olarak dahil ederek etkinleştirebilirsiniz. Bkz: [VM uzantıları ile Azure Resource Manager şablonları yazma](template-description.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#extensions) uzantıyı bir sanal makine şablonunun parçası olarak dahil olmak üzere daha fazla bilgi. Bu makalede, Azure tanılama uzantısını bir windows sanal makine şablonu nasıl ekleyebileceğiniz açıklanmaktadır.  

## <a name="add-the-azure-diagnostics-extension-to-the-vm-resource-definition"></a>Azure tanılama uzantısını VM kaynak tanımına ekleme
Üzerinde Windows sanal makinesi tanılama uzantısını etkinleştirmek için bir VM kaynak Resource Manager şablonu olarak uzantısı eklemeniz gerekir.

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

Başka bir ortak kök kaynakları düğümü sanal makinenin kaynakları düğümünde tanımlama yerine şablonun uzantı yapılandırması eklemek için kuraldır. Bu yaklaşımda, uzantısı ve sanal makineyi arasındaki hiyerarşik ilişkiyi açıkça belirtmek zorunda *adı* ve *türü* değerleri. Örneğin: 

```json
"name": "[concat(variables('vmName'),'Microsoft.Insights.VMDiagnosticsSettings')]",
"type": "Microsoft.Compute/virtualMachines/extensions",
```

Uzantı her zaman sanal makineyle ilişkili olduğundan, ya da doğrudan bu sanal makinenin kaynak düğümü altında doğrudan tanımlayabilir veya temel düzeyde tanımlamak ve sanal makine ile ilişkilendirmek için hiyerarşik adlandırma kuralını kullanın.

Sanal makine ölçek kümeleri için uzantıları yapılandırma belirtilen *extensionProfile* özelliği *VirtualMachineProfile*.

*Yayımcı* özellik değeri ile **Microsoft.Azure.Diagnostics** ve *türü* özellik değeri ile **IaaSDiagnostics**Azure tanılama uzantısını benzersiz şekilde tanımlar.

Değeri *adı* özelliği, kaynak grubu uzantı başvurmak için kullanılabilir. Özellikle çok ayarı **Microsoft.Insights.VMDiagnosticsSettings** izleme Göster yukarı doğru Azure portalında grafikleri emin olduktan Azure portal ile kolayca tanımlanan sağlar.

*TypeHandlerVersion* kullanmak istediğiniz uzantıları sürümünü belirtir. Ayarı *autoUpgradeMinorVersion* ikincil sürüme **true** kullanılabilir uzantısı'nın en son alt sürüm edinmenizi sağlar. Her zaman ayarlamanız önerilir *autoUpgradeMinorVersion* her zaman olacak şekilde **true** böylece her zaman tüm yeni özellikler ve hata düzeltmeleri ile en son kullanılabilir tanılama uzantısını kullanmak alın. 

*Ayarları* öğesi ayarladığınız ve geri (bazen ortak yapılandırma olarak adlandırılır) uzantısı okuma uzantısı yapılandırmaları özellikleri içerir. *Xmlcfg* özelliği içeren xml tabanlı yapılandırma tanılama günlükleri için tanılama aracı tarafından toplanan performans sayaçlarını vs. Bkz: [tanılama yapılandırma şeması](https://msdn.microsoft.com/library/azure/dn782207.aspx) xml şeması hakkında daha fazla bilgi. Bir değişken Azure Resource Manager şablonu olarak gerçek xml yapılandırmasını depolamak ve ardından birleştirmek için ortak bir uygulama olduğundan ve base64 kodlama bunları değerini ayarlamak için *xmlcfg*. Bölümüne bakarak [tanılama yapılandırma değişkenleri](#diagnostics-configuration-variables) değişkenleri xml depolamak nasıl daha iyi anlamak için. *StorageAccount* özelliği için hangi tanılama veri aktarılır depolama hesabının adını belirtir. 

Özelliklerinde *protectedSettings* (bazen başvurulan özel olarak yapılandırma) ayarlanabilir, ancak geri ayarlanmasından sonra okunamıyor. Salt yazılır yapısına *protectedSettings* depolama hesabı anahtarı gibi parolaları depolamak için yararlı tanılama veri yazıldığı kolaylaştırır.    

## <a name="specifying-diagnostics-storage-account-as-parameters"></a>Parametre olarak tanılama depolama hesabı belirtme
Yukarıdaki tanılama uzantısını json parçacığında iki parametre varsayar *existingdiagnosticsStorageAccountName* ve *existingdiagnosticsStorageResourceGroup* tanılama depolama belirtmek için Tanılama verilerinin depolandığı hesabı. Tanılama depolama hesabı belirten bir parametre farklı ortamlar genelinde tanılama depolama hesabı değiştirmek kolaylaştırır gibi örneğin, test etmek için farklı tanılama depolama hesabı ve farklı bir için kullanmak isteyebilirsiniz, Üretim dağıtımı.  

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

Sanal makine için kaynak grubundan farklı bir kaynak grubundaki bir tanılama depolama hesabı belirtmek için en iyi bir uygulamadır. Bir kaynak grubu, kendi ömrü sahip bir dağıtım birimi olarak kabul edilebilir, bir sanal makine dağıtılabilir ve kendisine yapılan yeni yapılandırmaları güncelleştirmeleri ancak arasında aynı depolama hesabındaki Tanılama verileri depolamak devam etmek istiyor olarak imzalanması Bu sanal makine dağıtımları. Farklı bir kaynak olarak depolama hesabına sahip çeşitli sürümleri arasında sorunlarını giderme kolaylaşır çeşitli sanal makine dağıtımlarının verileri kabul etmek için depolama hesabının sağlar.

> [!NOTE]
> Visual Studio'dan bir windows sanal makine şablonu oluşturursanız, varsayılan depolama hesabı sanal makine VHD burada karşıya aynı depolama hesabını kullanmak üzere ayarlanmış olabilir. Bu VM ilk kurulumu kolaylaştırmaktır. Parametre olarak geçirilebilir farklı depolama hesabı için kullanılacak şablonu yeniden öğeli. 
> 
> 

## <a name="diagnostics-configuration-variables"></a>Tanılama yapılandırma değişkenleri
Yukarıdaki tanılama uzantısını json parçacığında tanımlayan bir *AccountID* tanılama depolama için depolama hesabı anahtarı alma basitleştirmek için değişkeni:   

```json
"accountid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/',parameters('existingdiagnosticsStorageResourceGroup'), '/providers/','Microsoft.Storage/storageAccounts/', parameters('existingdiagnosticsStorageAccountName'))]"
```

*Xmlcfg* tanılama uzantısını özelliği birlikte birleştirilmiş birden çok değişkenleri kullanılarak tanımlanır. Json değişkenleri ayarlarken düzgün kaçış gerekir böylece bu değişkenlerin XML'de değerlerdir.

Bazı windows olay günlüklerini ve tanılama altyapı günlükleri ile birlikte standart sistem düzeyinde performans sayaçlarını toplar tanılama yapılandırması xml açıklar. Bunu olduğundan kaçışlı ve böylece yapılandırmasını, şablon değişkenleri bölüme doğrudan yapıştırılabilmesi için doğru biçimlendirilmiş. Bkz: [tanılama yapılandırma şeması](https://msdn.microsoft.com/library/azure/dn782207.aspx) yapılandırma XML'i İnsan daha okunabilir bir örneğin.

```json
"wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
"wadperfcounters1": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Privileged Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU privileged time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% User Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU user time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor Information(_Total)\\Processor Frequency\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"CPU frequency\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\System\\Processes\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Processes\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Threads\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Handle Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Handles\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\% Committed Bytes In Use\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Memory usage\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Available Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory available\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Committed Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory committed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Commit Limit\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory commit limit\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active time\" locale=\"en-us\"/></PerformanceCounterConfiguration>",
"wadperfcounters2": "<PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Read Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active read time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Write Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active write time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Transfers/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Reads/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk read operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Writes/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk write operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Read Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk read speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Write Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk write speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\LogicalDisk(_Total)\\% Free Space\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk free space (percentage)\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
"wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters1'), variables('wadperfcounters2'), '<Metrics resourceId=\"')]",
"wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name , '/providers/', 'Microsoft.Compute/virtualMachines/')]",
"wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>"
```

Yukarıdaki yapılandırma ölçümleri tanım xml düğümünde nasıl performans sayaçlarını önceki xml dosyasında tanımlanan tanımlayan bir önemli yapılandırma öğesi aynıdır *PerformanceCounter* düğümü toplanır ve depolanır. 

> [!IMPORTANT]
> Bu ölçümleri izleme grafikleri ve Uyarıları Azure portalında sürücü.  **Ölçümleri** düğümle *ResourceId* ve **MetricAggregation** verileri izleme VM görmek istiyorsanız, VM için tanılamayı yapılandırmasında eklenmelidir Azure portalı. 
> 
> 

Ölçümleri tanımları için xml örneği verilmiştir: 

```xml
<Metrics resourceId="/subscriptions/subscription().subscriptionId/resourceGroups/resourceGroup().name/providers/Microsoft.Compute/virtualMachines/vmName">
    <MetricAggregation scheduledTransferPeriod="PT1H"/>
    <MetricAggregation scheduledTransferPeriod="PT1M"/>
</Metrics>
```

*ResourceId* özniteliği benzersiz olarak tanımlayan aboneliğinizde sanal makine. Şablon otomatik olarak abonelik ve dağıttığınız kaynak grubunu temel alarak bu değerleri güncelleştirilebilmesi için subscription() ve resourceGroup() işlevleri kullandığınızdan emin olun.

Birden çok sanal makine bir döngüde oluşturuyorsanız, doldurmak zorunda *ResourceId* doğru tek tek her VM ayırt etmek için bir copyındex () işlev ile değer. *XmlCfg* değeri, bunu şu şekilde desteklemek için güncelleştirilebilir:  

```json
"xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), concat(parameters('vmNamePrefix'), copyindex()), variables('wadcfgxend')))]", 
```

MetricAggregation değeri *PT1M* ve *PT1H* bir dakika içinde bir toplama ve bir saat içinde bir toplama sırasıyla bitişini işaret eder.

## <a name="wadmetrics-tables-in-storage"></a>Depolama WADMetrics tabloları
Yukarıdaki ölçümleri yapılandırma aşağıdaki adlandırma kuralları ile tanılama depolama hesabınızdaki tablolar oluşturur:

* **WADMetrics**: tüm WADMetrics tablolar için standart bir önek
* **PT1H** veya **PT1M**: Tablo üzerinde 1 saat veya 1 dakika birleşik verileri içerdiğini belirtir
* **P10D**: tabloyu tablonun veri toplama başladığında gelen 10 gün için veri içerecek belirtir
* **V2S**: dize sabiti
* **yyyyaagg**: Tablo başladığı verileri toplama tarihi

Örnek: *WADMetricsPT1HP10DV2S20151108* 11-Kas-2015 tarihinde başlayan 10 günlük bir saate üzerinden toplanan ölçümleri veri içermez    

Her WADMetrics tablo şu sütunları içerir:

* **PartitionKey**: bölüm anahtarı göre oluşturulan *ResourceId* VM kaynak benzersiz şekilde tanımlamak için değer. Örneğin: 002Fsubscriptions:<subscriptionID>: 002FresourceGroups:002F<ResourceGroupName>: 002Fproviders:002FMicrosoft:002ECompute:002FvirtualMachines:002F<vmName>  
* **RowKey**: biçimdedir `<Descending time tick>:<Performance Counter Name>`. Azalan zaman onay hesaplaması max zaman çizgilerine toplama süresi başlangıç zamanı eksi olur. Örneğin örnek süresi 10-Kas-2015 tarihinde başlatılan ve 00:00Hrs UTC sonra hesaplama olacaktır: `DateTime.MaxValue.Ticks - (new DateTime(2015,11,10,0,0,0,DateTimeKind.Utc).Ticks)`. İçin bellek kullanılabilir bayt performans sayacı satır anahtarını gibi görünür: `2519551871999999999__:005CMemory:005CAvailable:0020Bytes`
* **CounterName**: performans sayacı adıdır. Bu eşleşen *counterSpecifier* xml yapılandırma dosyasında tanımlanmış.
* **En fazla**: performans sayacı toplama süre boyunca en büyük değeri.
* **En düşük**: performans sayacı toplama süre boyunca en küçük değeri.
* **Toplam**: performans sayacı tüm değerlerin toplamını toplama süresi içinde bildirdi.
* **Count**: performans sayacı için bildirilen değerler toplam sayısı.
* **Ortalama**: performans sayacı toplama dönemdeki ortalama (toplam/sayısı) değeri.

## <a name="next-steps"></a>Sonraki Adımlar
* Tanılama uzantısını sahip bir Windows sanal makine için bir tam örnek şablon bkz [201-vm-monitoring-tanılama-uzantısı](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-monitoring-diagnostics-extension)   
* Azure Resource Manager şablonunu kullanarak dağıtın [Azure PowerShell](ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) veya [Azure komut satırı](../linux/create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* Daha fazla bilgi edinmek [Azure Resource Manager şablonları yazma](../../resource-group-authoring-templates.md)
