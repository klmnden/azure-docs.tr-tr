---
title: VM uzantıları içeren Azure kaynak gruplarını dışa aktarma | Microsoft Docs
description: Sanal makine uzantıları dahil Resource Manager şablonları dışarı aktarın.
services: virtual-machines-windows
documentationcenter: ''
author: danielsollondon
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
ms.author: danis
ms.openlocfilehash: 3c54b77f52dfc7acf10dc26d4c00e9c14a296774
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="exporting-resource-groups-that-contain-vm-extensions"></a>VM uzantıları içeren kaynak grupları dışarı aktarma

Azure kaynak gruplarını daha sonra yeniden dağıtılması yeni bir Resource Manager şablonuna aktarılabilir. Dışa aktarma işlemi var olan kaynakların yorumlar ve Resource Manager şablonu dağıtıldığında oluşturan benzer bir kaynak grubunda neden olur. Sanal makine uzantıları içeren bir kaynak grubu karşı kaynak grubunu dışarı aktarma seçeneğini kullanırken, birden çok öğe uzantısı uyumluluk gibi ele alınması gereken ve ayarların korumalı.

Bu belge ayrıntıları listesi dahil olmak üzere sanal makine uzantıları ile ilgili kaynak grubunu dışarı aktarma işleminin nasıl çalıştığı uzantıları desteklenir ve işleme ayrıntıları verilerin güvenli.

## <a name="supported-virtual-machine-extensions"></a>Desteklenen sanal makine uzantıları

Çok sayıda sanal makine uzantıları kullanılabilir. Tüm uzantıları "Otomasyon betiğini" özelliğini kullanarak bir Resource Manager şablonu aktarılabilir. Bir sanal makine uzantısı desteklenmiyorsa, dışarı aktarılan şablona el ile yerleştirilmesi gerekiyor.

Aşağıdaki uzantılar Otomasyon betik özelliğiyle aktarılabilir.

| Dahili numara ||||
|---|---|---|---|
| Acronis yedekleme | Datadog Windows Aracısı | Linux için düzeltme eki uygulama işletim sistemi | VM anlık görüntü Linux
| Acronis yedekleme Linux | Docker uzantısı | Puppet aracı |
| BG bilgisi | DSC Uzantısı | Site 7 x 24 Apm Insight |
| BMC CTM Aracısı Linux | Dynatrace Linux | Site 7 x 24 Linux sunucusu |
| BMC CTM Aracısı Windows | Dynatrace Windows | Site 7 x 24 Windows sunucusu |
| Chef istemci | HPE güvenlik uygulama Defender | Eğilim mikro DSA |
| Özel bir komut dosyası | Iaas kötü amaçlı yazılımdan koruma | Eğilim mikro DSA Linux |
| Özel Betik Uzantısı | Iaas tanılama | Linux VM erişim |
| Linux için özel bir komut dosyası | Linux Chef istemci | Linux VM erişim |
| Datadog Linux Aracısı | Linux tanılama | VM anlık görüntü |

## <a name="export-the-resource-group"></a>Kaynak grubunu dışarı aktarma

Bir kaynak grubu içinde yeniden kullanılabilir bir şablonu dışarı aktarmak için aşağıdaki adımları tamamlayın:

1. Azure portalında oturum açın
2. Hub menüsünde, kaynak grupları'nı tıklatın.
3. Hedef kaynak grubu listeden seçin
4. Kaynak grubu dikey penceresinde Otomasyon betiğini tıklayın

![Şablonu dışarı aktarma](./media/export-templates/template-export.png)

Azure Resource Manager otomasyonlara betik Resource Manager şablonu, bir parametre dosyası ve PowerShell ve Azure CLI gibi birkaç örnek dağıtım betikleri oluşturur. Bu aşamada, dışarı aktarılan şablon, yeni bir şablon olarak Şablon Kitaplığı'na eklenen veya Dağıt düğmesi kullanılarak imzalanmasını karşıdan yükleme düğmesini kullanarak indirilebilir.

## <a name="configure-protected-settings"></a>Korumalı ayarlarını yapılandırma

Kimlik bilgileri ve yapılandırma dizeleri gibi hassas verileri şifreler korumalı ayarlarını yapılandırma, çok sayıda Azure sanal makine uzantıları içerir. Korumalı ayarları ile Otomasyon betiğini dışarı aktarılmaz. Gerekirse, korumalı ayarlarını yeniden gerekiyorsa dışa aktarılan içine şablonlu.

### <a name="step-1---remove-template-parameter"></a>1. adım - Kaldır şablon parametresi

Kaynak grubunu dışarı aktardığınızda tek şablon parametresi dışarı aktarılan korumalı ayarlar için bir değer sağlamak için oluşturulur. Bu parametre kaldırılabilir. Parametre kaldırmak için parametre listesine bakın ve bu JSON örneğe benzer parametre silin.

```json
"extensions_extensionname_protectedSettings": {
    "defaultValue": null,
    "type": "SecureObject"
}
```

### <a name="step-2---get-protected-settings-properties"></a>2. adım - Get ayarları özellikleri korumalı

Her korunan ayarın gerekli özellikler kümesi olduğundan, bu özelliklerin listesini gerekir toplanması. Korumalı ayarlarını yapılandırma, her bir parametreyi bulunabilir [github'da Azure Resource Manager şema](https://raw.githubusercontent.com/Azure/azure-resource-manager-schemas/master/schemas/2015-08-01/Microsoft.Compute.json). Bu şema, bu belgenin genel bakış bölümünde listelenen uzantıları için parametre kümeleri yalnızca içerir. 

Gelen istenen uzantısı, bu örnek için şema deposu içinde arama `IaaSDiagnostics`. Bir kez uzantıları `protectedSettings` nesne bulunduğu açıldı, her bir parametreyi not edin. Örneğinde `IaasDiagnostic` uzantısı, Parametreler gerektiren `storageAccountName`, `storageAccountKey`, ve `storageAccountEndPoint`.

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

### <a name="step-3---re-create-the-protected-configuration"></a>3. adım - korumalı yapılandırmayı yeniden oluşturma

Dışarı aktarılan şablona arama `protectedSettings` ve dışarı aktarılan korumalı ayar nesnesini gerekli uzantısı parametreleri ve her biri için bir değer içeren yeni bir tane ile değiştirin.

Örneğinde `IaasDiagnostic` uzantısı, yeni korumalı ayarını yapılandırma uygulamamız görünecektir aşağıdaki örnekteki gibi:

```json
"protectedSettings": {
    "storageAccountName": "[parameters('storageAccountName')]",
    "storageAccountKey": "[parameters('storageAccountKey')]",
    "storageAccountEndPoint": "https://core.windows.net"
}
```

Son uzantısı kaynak aşağıdaki JSON örneğe benzer:

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

Şablon parametreleri özellik değerlerini sağlamak için kullanıyorsanız, bunlar oluşturulması gerekir. Şablon parametreleri değerlerini ayarlama korumalı oluştururken kullandığınızdan emin olun `SecureString` parametre türü hassas değerleri güvenli hale getirilir. Parametreleri kullanma hakkında daha fazla bilgi için bkz: [Azure Resource Manager şablonları yazma](../../resource-group-authoring-templates.md).

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

Bu noktada, şablonu herhangi bir şablon dağıtım yöntemini kullanılarak dağıtılabilir.
