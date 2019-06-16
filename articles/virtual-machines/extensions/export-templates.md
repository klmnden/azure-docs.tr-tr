---
title: VM uzantıları içeren Azure kaynak grupları dışarı aktarma | Microsoft Docs
description: Sanal makine uzantıları içeren Resource Manager şablonlarını dışarı aktarma.
services: virtual-machines-windows
documentationcenter: ''
author: roiyz-msft
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 7f4e2ca6-f1c7-4f59-a2cc-8f63132de279
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/05/2016
ms.author: roiyz
ms.openlocfilehash: f56cfeeede393dbdb9632ea4120d3a81e89f3f7c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61484049"
---
# <a name="exporting-resource-groups-that-contain-vm-extensions"></a>VM uzantıları içeren kaynak grupları dışarı aktarma

Azure kaynak grupları daha sonra yeniden yeni bir Resource Manager şablonuna dışarı aktarılabilir. Verme işlemi var olan kaynakları yorumlar ve Resource Manager şablonu dağıtırken oluşturur benzer bir kaynak grubunda sonuçlanır. Sanal makine uzantıları içeren bir kaynak grubu karşı kaynak grubunu dışarı aktarma seçeneği kullanılırken, çeşitli öğeleri uzantısı uyumluluğu gibi ele alınması gereken ve ayarların korumalı.

Bu belge ayrıntıları listesi dahil olmak üzere, sanal makine uzantıları ile ilgili kaynak grubunu dışarı aktarma işleminin nasıl çalıştığı uzantıları desteklenen ve veri işleme hakkında ayrıntılı bilgi güvenliği.

## <a name="supported-virtual-machine-extensions"></a>Desteklenen sanal makine uzantıları

Çok sayıda sanal makine uzantıları artık kullanılabilir. Tüm Uzantılar "Otomasyon betiği" özelliğini kullanarak bir Resource Manager şablonu aktarılabilir. Sanal makine uzantısı desteklenmiyor, el ile dışarı aktarılan şablonun yerleştirilmesi gerekir.

Aşağıdaki uzantılar Otomasyon betiği özelliğiyle dışarı aktarılabilir.

| Dahili numara ||||
|---|---|---|---|
| Acronis yedekleme | Datadog Windows Aracısı | Linux için düzeltme eki uygulama işletim sistemi | VM anlık görüntüsü Linux
| Acronis yedekleme Linux | Docker uzantısı | Puppet aracı |
| BG bilgileri | DSC Uzantısı | Site 24 x 7 Apm İçgörüleri |
| BMC CTM aracı Linux | Dynatrace Linux | Site 24 x 7 Linux sunucusu |
| BMC CTM Aracısı Windows | Dynatrace Windows | Site 24 x 7 Windows sunucusu |
| Chef istemci | HPE güvenlik uygulama Defender | Trend Micro DSA |
| Özel Betik | Iaas kötü amaçlı yazılımdan koruma | Trend Micro DSA Linux |
| Özel Betik Uzantısı | Iaas tanılama | Linux için VM erişimi |
| Linux özel betiği | Linux Chef istemci | Linux için VM erişimi |
| Datadog Linux Aracısı | Linux tanılama | Sanal Makine Anlık Görüntüsü |

## <a name="export-the-resource-group"></a>Kaynak grubunu dışarı aktarma

Bir kaynak grubu yeniden kullanılabilir bir şablona dışarı aktarmak için aşağıdaki adımları tamamlayın:

1. Azure portalında oturum açın
2. Hub menüsünde kaynak grupları
3. Hedef kaynak grubu listesinden seçin.
4. Kaynak grubu dikey penceresinde, Otomasyon betiği'a tıklayın.

![Şablonu dışarı aktarma](./media/export-templates/template-export.png)

Azure Resource Manager otomasyonları betik, bir Resource Manager şablonu, bir parametre dosyası ve PowerShell ve Azure CLI gibi birkaç örnek dağıtım betikleri oluşturur. Bu noktada, dışarı aktarılan şablon, yeni bir şablon olarak Şablon Kitaplığı'na eklenen veya Dağıt düğmesini kullanarak yeniden Yükle düğmesini kullanarak yüklenebilir.

## <a name="configure-protected-settings"></a>Korumalı ayarlarını yapılandırma

Kimlik bilgileri ve yapılandırma dizeleri gibi hassas verileri şifreler korumalı ayarlarını yapılandırma, birçok Azure sanal makine uzantıları içerir. Otomasyon betiği ile korunan ayarları dışa aktarılmaz. Gerekirse, korumalı ayarları yeniden gerekiyorsa, içinde dışarı aktarılan şablonu.

### <a name="step-1---remove-template-parameter"></a>Adım 1 - Remove şablon parametresi

Kaynak grubunu dışarı aktarılırken dışarı aktarılan korumalı ayarlar için bir değer sağlamak için tek bir şablon parametresi oluşturulur. Bu parametre kaldırılabilir. Parametre kaldırmak için parametre listesinde arayın ve bu JSON örneğe benzer parametreyi silin.

```json
"extensions_extensionname_protectedSettings": {
    "defaultValue": null,
    "type": "SecureObject"
}
```

### <a name="step-2---get-protected-settings-properties"></a>Adım 2 - Get korumalı ayarları özellikleri

Her korunan ayarın gerekli özellikler kümesi olduğundan, bu özelliklerin bir listesi gerekir toplanması. Her korunan ayarları yapılandırma parametresi bulunabilir [github'daki Azure Resource Manager şema](https://raw.githubusercontent.com/Azure/azure-resource-manager-schemas/master/schemas/2015-08-01/Microsoft.Compute.json). Bu şema, bu belgenin genel bakış bölümünde listelenen uzantıları için parametre kümeleri yalnızca içerir. 

Şema depo içinde bu örnek için istenen uzantısı için aramak `IaaSDiagnostics`. Uzantıları `protectedSettings` nesne bulunan, her parametresinin not alın. Örneğinde `IaasDiagnostic` uzantısı, Parametreler gerektiren `storageAccountName`, `storageAccountKey`, ve `storageAccountEndPoint`.

```json
"protectedSettings": {
    "type": "object",
    "properties": {
        "storageAccountName": {
            "type": "string"
        },
        "storageAccountKey": {
            "type": "string"
        },
        "storageAccountEndPoint": {
            "type": "string"
        }
    },
    "required": [
        "storageAccountName",
        "storageAccountKey",
        "storageAccountEndPoint"
    ]
}
```

### <a name="step-3---re-create-the-protected-configuration"></a>3\. adım - korumalı yapılandırmayı yeniden oluşturma

Dışarı aktarılan şablona arama `protectedSettings` ve gerekli bir uzantı parametrelerinden ve her biri için bir değer içeren yeni bir tane dışarı aktarılan korumalı ayar nesnesini değiştirin.

Örneğinde `IaasDiagnostic` uzantısı, yeni korumalı ayarı yapılandırması aşağıdaki örnekteki gibi görünür:

```json
"protectedSettings": {
    "storageAccountName": "[parameters('storageAccountName')]",
    "storageAccountKey": "[parameters('storageAccountKey')]",
    "storageAccountEndPoint": "https://core.windows.net"
}
```

Son uzantısı kaynağın aşağıdaki JSON örneğe benzer:

```json
{
    "name": "Microsoft.Insights.VMDiagnosticsSettings",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "[variables('apiVersion')]",
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
            "storageAccountName": "[parameters('storageAccountName')]",
            "storageAccountKey": "[parameters('storageAccountKey')]",
            "storageAccountEndPoint": "https://core.windows.net"
        }
    }
}
```

Şablon parametreleri özellik değerlerini sağlamak için kullanıyorsanız, bunlar oluşturulması gerekir. Şablon parametreleri değerlerini ayarlama korumalı oluştururken kullandığınızdan emin olun `SecureString` parametre türü hassas değerleri güvenli hale getirilir. Parametreleri kullanma hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonları yazma](../../resource-group-authoring-templates.md).

Örneğinde `IaasDiagnostic` uzantısı, aşağıdaki parametreleri Resource Manager şablonu parametreleri bölümünde oluşturulacaktır.

```json
"storageAccountName": {
    "defaultValue": null,
    "type": "SecureString"
},
"storageAccountKey": {
    "defaultValue": null,
    "type": "SecureString"
}
```

Bu noktada, şablon, herhangi bir şablon dağıtım yöntemini kullanılarak dağıtılabilir.
