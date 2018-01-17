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
ms.openlocfilehash: 53adef0f512c54e036a981dbaa0d08453db6b194
ms.sourcegitcommit: a0d2423f1f277516ab2a15fe26afbc3db2f66e33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2018
---
# <a name="use-the-azure-custom-script-extension-with-linux-virtual-machines"></a>Azure özel betik uzantısı ile Linux sanal makineleri kullanma
Özel betik uzantısının indirir ve Azure sanal makinelerde betikleri çalıştırır. Bu uzantı, dağıtım sonrası yapılandırma, yazılım yükleme veya başka bir yapılandırma/yönetim görevi için yararlıdır. Azure Storage veya başka bir erişilebilir Internet konum komut dosyaları indirebilir veya uzantısı çalışma zamanına sağlayabilir. 

Özel betik uzantısının Azure Resource Manager şablonları ile tümleşir. Ayrıca Azure CLI, PowerShell, Azure portalında veya Azure sanal makineleri REST API'sini kullanarak çalıştırabilirsiniz.

Bu makalede Azure clı'dan özel betik uzantısının kullanmayı ve Azure Resource Manager şablonu kullanarak uzantısı çalıştırmaya nasıl ayrıntıları. Bu makalede, ayrıca Linux sistemleri için sorun giderme adımları sağlar.

## <a name="extension-configuration"></a>Uzantı yapılandırması
Özel betik uzantısı yapılandırma komut dosyası konumunu ve çalıştırılacak komut gibi belirtir. Bu yapılandırma yapılandırma dosyalarını depolamak, komut satırında belirtin veya bir Azure Resource Manager şablonu belirtin. 

Duyarlı veri şifrelenir ve yalnızca sanal makine içinde şifresi bir korumalı bir yapılandırma depolayabilirsiniz. Korumalı yapılandırma, bir parola gibi gizli yürütme komutu içerir yararlıdır.

### <a name="public-configuration"></a>Genel yapılandırma
Genel yapılandırma Şeması aşağıdaki gibidir.

>[!NOTE]
>Bu özellik adları büyük/küçük harfe duyarlıdır. Dağıtım sorunlarını önlemek için aşağıda gösterildiği gibi adları kullanın.

* **commandToExecute** (gerekli, string): çalıştırmak için giriş noktası komut dosyası.
* **fileUris** (isteğe bağlı, dize dizisi): dosyaların indirilmesi URL.
* **zaman damgası** (isteğe bağlı, tamsayı): betik zaman damgası. Yeniden çalıştırılan komut dosyasının tetiklemek istiyorsanız bu alanın değeri değiştirin.

```json
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a>Korumalı yapılandırma
Korumalı yapılandırma Şeması aşağıdaki gibidir.

>[!NOTE]
>Bu özellik adları büyük/küçük harfe duyarlıdır. Dağıtım sorunlarını önlemek için aşağıda gösterildiği gibi adları kullanın.

* **commandToExecute** (isteğe bağlı, dize): çalıştırmak için giriş noktası komut dosyası. Parolalar gibi gizli komutunuzu içeriyorsa, bu alanı kullanın.
* **storageAccountName** (isteğe bağlı, dize): depolama hesabı adı. Depolama kimlik belirtirseniz, tüm dosya URI için Azure BLOB'ları URL'leri olması gerekir.
* **storageAccountKey** (isteğe bağlı, dize): depolama hesabının erişim anahtarı.

```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a>Azure CLI
Özel betik uzantısının çalıştırmak için Azure CLI kullanırken bir yapılandırma dosyası veya dosya oluşturun. En azından, dosyanın URI ve komut yürütme yapılandırma dosyalarını içerir.

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

İsteğe bağlı olarak, JSON biçimli dize olarak komutta ayarları belirtebilirsiniz. Yürütme sırasında ve ayrı yapılandırma dosyası olmadan belirtilmesi için yapılandırmasını sağlar.

```azurecli
az vm extension set '
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"],"commandToExecute": "./config-music.sh"}'
```

### <a name="azure-cli-examples"></a>Azure CLI örnekleri

#### <a name="public-configuration-with-script-file"></a>Komut dosyası ile ortak yapılandırma

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

#### <a name="public-configuration-with-no-script-file"></a>Herhangi bir komut dosyası ile ortak yapılandırma

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Azure CLI komutu:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

#### <a name="public-and-protected-configuration-files"></a>Genel ve korumalı yapılandırma dosyaları

URI komut dosyasını belirtmek için bir ortak yapılandırma dosyası kullanın. Çalıştırılacak komutu belirtmek için korumalı yapılandırma dosyası kullanın.

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

## <a name="resource-manager-template"></a>Resource Manager şablonu
Resource Manager şablonu kullanarak sanal makine dağıtımı sırasında Azure özel betik uzantısının çalıştırabilirsiniz. Bunu yapmak için doğru biçimlendirilmiş JSON Dağıtım şablonuna ekleyin.

### <a name="resource-manager-examples"></a>Resource Manager örnekleri

#### <a name="public-configuration"></a>Genel yapılandırma

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

#### <a name="execution-command-in-protected-configuration"></a>Korumalı yapılandırmasında yürütme komutu

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

Tam bir örnek için bkz: [.NET müzik deposu demo](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="troubleshooting"></a>Sorun giderme
Özel betik uzantısının çalıştığında, komut dosyası oluşturulur veya aşağıdaki örneğe benzer bir dizine indirilir. Komut çıktısı da bu dizinine kaydedilir `stdout` ve `stderr` dosyaları.

```bash
/var/lib/waagent/custom-script/download/0/
```

Azure betik uzantısı burada bulabilirsiniz bir günlük üretir:

```bash
/var/log/azure/custom-script/handler.log
```

Azure CLI kullanarak özel betik uzantısının yürütme durumunu da alabilir:

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

## <a name="next-steps"></a>Sonraki adımlar
Diğer VM betik uzantıları hakkında daha fazla bilgi için bkz: [Linux Azure betik uzantısı genel bakış](extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

