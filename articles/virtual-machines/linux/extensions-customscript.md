---
title: "Özel komut dosyaları Linux VM'ler için Azure'da çalışan | Microsoft Docs"
description: "Özel betik uzantısının kullanarak Linux VM yapılandırma görevleri otomatikleştirme"
services: virtual-machines-linux
documentationcenter: 
author: danielsollondon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: cf17ab2b-8d7e-4078-b6df-955c6d5071c2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: danis
ms.openlocfilehash: 53a241f12373acdb5d40575915d8d6c2f3c86b9a
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="using-the-azure-custom-script-extension-with-linux-virtual-machines"></a>Azure özel betik uzantısı ile Linux sanal makineleri kullanma
Özel betik uzantısının indirir ve Azure sanal makinelerde komut dosyaları çalıştırılır. Bu uzantı dağıtım sonrası yapılandırma, yazılım yükleme veya diğer yapılandırma/yönetim görevleri için kullanışlıdır. Komut dosyaları Azure depolama veya diğer erişilebilir Internet konumdan indirilen veya çalışma zamanı uzantısı sağlanan. Özel betik uzantısı, Azure Resource Manager şablonları ile tümleşir ve Azure CLI, PowerShell, Azure portalında veya Azure sanal makine REST API'sini kullanarak da çalıştırılabilir.

Bu belgede Azure CLI ve Azure Resource Manager şablonu özel betik uzantısı kullanma ayrıntıları ve ayrıca Linux sistemlerde sorun giderme adımlarını ayrıntıları.

## <a name="extension-configuration"></a>Uzantı yapılandırması
Özel betik uzantısı yapılandırma komut dosyası konumunu ve çalıştırılacak komut gibi belirtir. Bu yapılandırma, komut satırında veya bir Azure Resource Manager şablonunda belirtilen yapılandırma dosyalarında depolanabilir. Duyarlı veri şifrelenir ve yalnızca sanal makine içinde şifresi bir korumalı bir yapılandırma depolanabilir. Korumalı yapılandırma, bir parola gibi gizli yürütme komutu içerir yararlıdır.

### <a name="public-configuration"></a>Genel yapılandırma
Şema:

**Not** -bu özellik adları büyük/küçük harfe duyarlıdır. Dağıtım sorunlarını önlemek için aşağıda görüldüğü gibi adları kullanın.

* **commandToExecute**: (gerekli, string) yürütmek için giriş noktası komut dosyası
* **fileUris**: (isteğe bağlı, dize dizisi) yüklenecek dosyaları için URL'leri.
* **zaman damgası** (isteğe bağlı, tamsayı) yalnızca bu alanın değerini değiştirerek yeniden Çalıştır komut dosyasının tetiklemek için bu alanı kullanabilirsiniz.

```json
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a>Korumalı yapılandırma
Şema:

**Not** -bu özellik adları büyük/küçük harfe duyarlıdır. Dağıtım sorunlarını önlemek için aşağıda görüldüğü gibi adları kullanın.

* **commandToExecute**: (isteğe bağlı, dize) yürütmek için giriş noktası komut dosyası. Parolalar gibi gizli komutunuzu içeriyorsa, bu alan kullanın.
* **storageAccountName**: (isteğe bağlı, dize) depolama hesabının adı. Depolama kimlik belirtirseniz, tüm fileUris Azure BLOB'ları için URL'leri olması gerekir.
* **storageAccountKey**: (isteğe bağlı, dize) depolama hesabının erişim anahtarı.

```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a>Azure CLI
Özel betik uzantısının çalıştırmak için Azure CLI kullanarak, bir yapılandırma dosyası veya dosya URI'si ve komut dosyası yürütme komutu en az içeren dosyaları oluşturun.

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

İsteğe bağlı olarak ayarları komutta JSON biçimli dize olarak belirtilebilir. Yürütme sırasında ve ayrı yapılandırma dosyası olmadan belirtilmesi için yapılandırmasını sağlar.

```azurecli
az vm extension set '
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"],"commandToExecute": "./config-music.sh"}'
```

### <a name="azure-cli-examples"></a>Azure CLI örnekleri

**Örnek 1** -komut dosyası ile genel yapılandırması.

```json
{
  "fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"],
  "commandToExecute": "./config-music.sh"
}
```

Azure CLI komutu:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

**Örnek 2** -herhangi bir komut dosyası ile genel yapılandırması.

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Azure CLI komutu:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

**Örnek 3** - bir ortak yapılandırma dosyası URI komut dosyasını belirtmek için kullanılır ve korumalı yapılandırma dosyası çalıştırılacak komutu belirtmek için kullanılır.

Genel yapılandırma dosyası:

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"]
}
```

Korumalı yapılandırma dosyası:  

```json
{
  "commandToExecute": "./hello.sh <password>"
}
```

Azure CLI komutu:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json --protected-settings ./protected-config.json
```

## <a name="resource-manager-template"></a>Resource Manager Şablonu
Azure özel betik uzantısının Resource Manager şablonu kullanarak sanal makine dağıtımı aynı anda çalıştırabilirsiniz. Bunu yapmak için doğru biçimlendirilmiş JSON Dağıtım şablonuna ekleyin.

### <a name="resource-manager-examples"></a>Resource Manager örnekleri
**Örnek 1** -genel yapılandırması.

```json
{
    "name": "scriptextensiondemo",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-15",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('scriptextensiondemoName'))]"
    ],
    "tags": {
        "displayName": "scriptextensiondemo"
    },
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
      "settings": {
        "fileUris": [
          "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
        ],
        "commandToExecute": "sh hello.sh"
      }
    }
}
```

**Örnek 2** -korumalı yapılandırmasında yürütme komutu.

```json
{
  "name": "config-app",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
      ]              
    },
    "protectedSettings": {
      "commandToExecute": "sh hello.sh <password>"
    }
  }
}
```

.Net görmek için tam bir örnek - çekirdek müzik deposu Demo [müzik deposu Demo](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="troubleshooting"></a>Sorun giderme
Özel betik uzantısının çalıştığında, komut dosyası oluşturulur veya aşağıdaki örneğe benzer bir dizine indirilir. Komut çıktısı da bu dizinine kaydedilir `stdout` ve `stderr` dosya.

```bash
/var/lib/waagent/custom-script/download/0/
```

Azure betik uzantısı burada bulunabilir bir günlük üretir.

```bash
/var/log/azure/custom-script/handler.log
```

Özel betik uzantısının yürütme durumu ayrıca Azure CLI ile alınabilir.

```azurecli
az vm extension list -g myResourceGroup --vm-name myVM
```

Çıktı aşağıdaki metni gibi görünür:

```azurecli
info:    Executing command vm extension get
+ Looking up the VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a>Sonraki Adımlar
Diğer VM betik uzantıları hakkında daha fazla bilgi için bkz: [Linux için Azure betik uzantısı genel bakış](extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

