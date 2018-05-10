---
title: Özel komut dosyaları Linux VM'ler için Azure'da çalışan | Microsoft Docs
description: Özel betik uzantısının v1 kullanarak Linux VM yapılandırma görevleri otomatikleştirme
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/25/2018
ms.author: danis
ms.openlocfilehash: eac64a5b456eb040bcb1ac01c3c86dfde0847e57
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="use-the-azure-custom-script-extension-version-1-with-linux-virtual-machines"></a>Linux sanal makineleri ile Azure özel betik uzantısı sürüm 1'i kullanın
Özel betik uzantısı sürüm 1 indirir ve Azure sanal makinelerde betikleri çalıştırır. Bu uzantı, dağıtım sonrası yapılandırma, yazılım yükleme veya başka bir yapılandırma/yönetim görevi için yararlıdır. Azure Storage veya başka bir erişilebilir Internet konum komut dosyaları indirebilir veya uzantısı çalışma zamanına sağlayabilir. 

Özel betik uzantısının Azure Resource Manager şablonları ile tümleşir. Ayrıca Azure CLI, PowerShell, Azure portalında veya Azure sanal makineleri REST API'sini kullanarak çalıştırabilirsiniz.

Bu makalede Azure clı'dan özel betik uzantısının kullanmayı ve Azure Resource Manager şablonu kullanarak uzantısı çalıştırmaya nasıl ayrıntıları. Bu makalede, ayrıca Linux sistemleri için sorun giderme adımları sağlar.


İki Linux özel betik uzantısı vardır:
* Sürüm 1 - Microsoft.OSTCExtensions.CustomScriptForLinux
* Sürüm 2 - Microsoft.Azure.Extensions.CustomScript

Lütfen yeni sürümü kullanmak için yeni ve varolan dağıtımları geçin ([Microsoft.Azure.Extensions.CustomScript](\custom-script-linux.md)) yerine. Yeni sürüm içeri kayma değiştirme olması amaçlanmıştır. Bu nedenle, geçiş adı ve sürümü değiştirirken kadar kolaydır, uzantısı yapılandırmanızı değiştirmeniz gerekmez.

 

### <a name="operating-system"></a>İşletim Sistemi
Desteklenen Linux dağıtımları:

- CentOS 6.5 ve üzeri
- Debian 8 ve üzeri
    - Debian 8.7 son görüntülerinde CustomScriptForLinux kıran Python2 ile gelmez.
- FreeBSD
- OpenSUSE 13.1 ve üzeri
- Oracle Linux 6.4 ve üzeri
- SUSE Linux Enterprise Server 11 SP3 ve üzeri
- Ubuntu 12.04 ve üzeri

### <a name="script-location"></a>Komut dosyası konumu

Azure Blob depolama alanına erişmek için Azure Blob Depolama kimlik bilgilerinizi kullanılacak uzantı kullanabilirsiniz. Alternatif olarak, VM iç dosya sunucusu vb. gibi GitHub, o uç noktasına yönlendirebilir sürece komut dosyası konumu herhangi where, olabilir.

### <a name="internet-connectivity"></a>Internet bağlantısı
GitHub ya da Azure depolama gibi harici olarak bir komut dosyası karşıdan yüklemeniz gerekiyorsa, ek güvenlik duvarı/ağ güvenlik grubu bağlantı noktalarının açılması gerekir. Örneğin, komut dosyası Azure depolama alanında bulunuyorsa, size izin verebilir Azure NSG hizmet etiketleri kullanarak erişim [depolama](https://docs.microsoft.com/en-us/azure/virtual-network/security-overview#service-tags).

Yerel bir sunucuda komut dosyanızı olduğu sonra ek güvenlik duvarı/ağ güvenlik hala gerekebilir grup bağlantı noktalarının açılması gerekir.

### <a name="tips-and-tricks"></a>İpuçları ve Püf Noktaları
* Bu uzantı için en yüksek hata oranı test hata, komut dosyasını çalıştırır komut dosyasında sözdizimi hataları nedeni ve ayrıca başarısız olduğu bulmayı kolaylaştırmak için komut dosyasına bir günlük daha yerleştirecek.
* Idempotent, olan komut dosyaları yazmak için bunlar yeniden birden fazla kez yanlışlıkla çalıştırırsanız, bu sistem değişiklikleri neden olmaz.
* Çalıştırdıklarında betikleri kullanıcı girişi gerektirmeyen emin olun.
* Betik çalıştırmak izin verilen 90 dakika, başarısız bir uzantı sağlama içinde daha uzun bir şey neden olur.
* Betik içinde yeniden başlatmalar koymayın, bu sorunları yüklenmekte olan diğer uzantılarıyla neden olur ve sonrası yeniden başlatma, uzantısı yeniden başlatma sonrasında devam etmez. 
* Yeniden başlatma neden olacak bir komut dosyası varsa, uygulamaları yüklemek ve betikler vb. çalıştırın. Cron işi veya DSC veya Chef, Puppet uzantıları gibi araçları kullanarak yeniden zamanlamanız gerekir.
* Kullanabileceğiniz sonra bir komut dosyası her önyükleme üzerinde çalıştırmak istiyorsanız, uzantısı yalnızca bir komut dosyası bir kez çalışır [bulut init görüntü](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/using-cloud-init) ve bir [başına komut dosyaları önyükleme](https://cloudinit.readthedocs.io/en/latest/topics/modules.html#scripts-per-boot) modülü. Alternatif olarak, bir Systemd hizmet birim oluşturmak için komut dosyasını kullanabilirsiniz.
* Bir komut dosyası çalışacağı zamanlamak isterseniz, bir Cron işi oluşturmak için uzantı kullanmanız gerekir. 
* Komut dosyası çalıştırılırken yalnızca Azure portal veya CLI 'geçirme' uzantı durumunu görürsünüz. Çalışan bir komut dosyasının daha sık durum güncelleştirmeleri istiyorsanız, kendi çözüm oluşturmanız gerekir.
* Özel betik uzantısı yerel olarak desteklemez proxy sunucuları, ancak komut dosyanızın içinde proxy sunucuları gibi destekleyen bir dosya aktarımı aracı kullanabilir *Curl*. 
* Betikleri veya komutları dayanabileceği varsayılan olmayan directory konumlarını unutmayın, bu durumu çözmek için mantığı vardır.



## <a name="extension-schema"></a>Uzantı şeması

Özel betik uzantısı yapılandırma komut dosyası konumunu ve çalıştırılacak komut gibi belirtir. Bu yapılandırma yapılandırma dosyalarını depolamak, komut satırında belirtin veya bir Azure Resource Manager şablonu belirtin. 

Duyarlı veri şifrelenir ve yalnızca sanal makine içinde şifresi bir korumalı bir yapılandırma depolayabilirsiniz. Korumalı yapılandırma, bir parola gibi gizli yürütme komutu içerir yararlıdır.

Bu öğeler hassas verisi olarak kabul edilir ve uzantıları korumalı ayarı yapılandırmasında belirtilen. Azure VM uzantısının korumalı ayarı veri şifrelenir ve yalnızca hedef sanal makineye şifresi.

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
    "publisher": "Microsoft.OSTCExtensions",
    "type": "CustomScriptForLinux",
    "typeHandlerVersion": "1.5",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "<url>"
      ],
      "enableInternalDNSCheck": true
    },
    "protectedSettings": {
      "storageAccountName": "<storage-account-name>",
      "storageAccountKey": "<storage-account-key>",
      "commandToExecute": "<command>"
    }
  }
}
```

### <a name="property-values"></a>Özellik değerleri

| Ad | Değer / örnek | Veri Türü | 
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | tarih |
| Yayımcı | Microsoft.OSTCExtensions | dize |
| type | CustomScriptForLinux | dize |
| typeHandlerVersion | 1,5 | Int |
| fileUris (örneğin) | https://github.com/MyProject/Archive/MyPythonScript.py | array |
| commandToExecute (örneğin) | Python MyPythonScript.py < param1 my > | dize |
| enableInternalDNSCheck | true | boole |
| storageAccountName (örneğin) | examplestorageacct | dize |
| storageAccountKey (örneğin) | TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg== | dize |

### <a name="property-value-details"></a>Özellik değeri ayrıntıları
* `fileUris`: (isteğe bağlı, dize dizisi) komut dosyaları URI listesi
* `enableInternalDNSCheck`: (isteğe bağlı, bool) varsayılan değer True, DNS denetimi devre dışı bırakmak için False olarak ayarlayın.
* `commandToExecute`: (isteğe bağlı, dize) yürütmek için giriş noktası komut dosyası
* `storageAccountName`: (isteğe bağlı, dize) depolama hesabı adı
* `storageAccountKey`: (isteğe bağlı, dize) depolama hesabının erişim anahtarı

Aşağıdaki değerleri ortak ya da korumalı ayarlarında ayarlanabilir, hem genel hem de korumalı ayarlarında bu değerleri kümesi altında olmamalıdır.
* `commandToExecute`

Hata ayıklama, ancak için genel ayarları belki de yararlı kullanarak korumalı ayarları kullanmanız önerilir.

Genel ayarları burada betik yürütülecek VM düz metin olarak gönderilir.  Korumalı ayarları yalnızca Azure ve VM bildiği bir anahtar kullanılarak şifrelenmiş. Gönderildikleri olarak ayarlar VM kaydedilir, ayarları şifrelediyseniz yani şifrelenmiş VM kaydedilirler. Şifrelenmiş değerler şifresini çözmek için kullanılan sertifika, VM üzerinde depolanır ve çalışma zamanında ayarlarını (gerekiyorsa) şifresini çözmek için kullanılan.


## <a name="template-deployment"></a>Şablon dağıtımı
Azure VM uzantıları, Azure Resource Manager şablonları ile dağıtılabilir. Önceki bölümde ayrıntılı JSON şeması bir Azure Resource Manager şablonunda bir Azure Resource Manager şablon dağıtımı sırasında özel betik uzantısının çalıştırmak için kullanılabilir. 


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
    "publisher": "Microsoft.OSTCExtensions",
    "type": "CustomScriptForLinux",
    "typeHandlerVersion": "1.5",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": ["http://MyAccount.blob.core.windows.net/vhds/MyShellScript.sh"]
    },
    "protectedSettings": {
      "storageAccountName": "MyAccount",
      "storageAccountKey": "<storage-account-key>",
      "commandToExecute": "sh MyShellScript.sh"
    }
  }
}
```

>[!NOTE]
>Bu özellik adları büyük/küçük harfe duyarlıdır. Dağıtım sorunlarını önlemek için aşağıda gösterildiği gibi adları kullanın.

## <a name="azure-cli"></a>Azure CLI
Özel betik uzantısının çalıştırmak için Azure CLI kullanırken bir yapılandırma dosyası veya dosya oluşturun. En azından 'commandToExecute' olması gerekir.

```azurecli
az vm extension set -n VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.5 \
  --vm-name MyVm --resource-group MyResourceGroup \
  --protected-settings '{"commandToExecute": "echo hello"}'
```

İsteğe bağlı olarak, JSON biçimli dize olarak komutta ayarları belirtebilirsiniz. Yürütme sırasında ve ayrı yapılandırma dosyası olmadan belirtilmesi için yapılandırmasını sağlar.

```azurecli
az vm extension set \
  --resource-group exttest \
  --vm-name exttest \
  --name CustomScriptForLinux \
  --publisher Microsoft.OSTCExtensions \
  --settings '{"fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"],"commandToExecute": "./config-music.sh"}'
```

### <a name="azure-cli-examples"></a>Azure CLI örnekleri

#### <a name="public-configuration-with-no-script-file"></a>Herhangi bir komut dosyası ile ortak yapılandırma

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Azure CLI komutu:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name CustomScriptForLinux \
  --publisher Microsoft.OSTCExtensions \
  --settings ./script-config.json
```

#### <a name="public-and-protected-configuration-files"></a>Genel ve korumalı yapılandırma dosyaları

URI komut dosyasını belirtmek için bir ortak yapılandırma dosyası kullanın. Çalıştırılacak komutu belirtmek için korumalı yapılandırma dosyası kullanın.

Genel yapılandırma dosyası:

```json
{
  "fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"]
}
```

Korumalı yapılandırma dosyası:  

```json
{
  "commandToExecute": "./config-music.sh <param1>"
}
```

Azure CLI komutu:

```azurecli
az vm extension set 
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name CustomScriptForLinux \
  --publisher Microsoft.OSTCExtensions \
  --settings ./script-config.json \
  --protected-settings ./protected-config.json
```

## <a name="troubleshooting"></a>Sorun giderme
Özel betik uzantısının çalıştığında, komut dosyası oluşturulur veya aşağıdaki örneğe benzer bir dizine indirilir. Komut çıktısı da bu dizinine kaydedilir `stdout` ve `stderr` dosyaları. 

```bash
/var/lib/waagent/Microsoft.OSTCExtensions.CustomScriptForLinux-<version>/download/1
```

Sorun giderme, önce Linux aracı günlüğüne bakın, uzantısı çalıştı, olun denetleyin:

```bash
/var/log/waagent.log 
```

Uzantı yürütme için göz önünde bulundurmanız gerekenler, şu şekilde görünür:
```text
2018/04/26 15:29:44.835067 INFO [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Target handler state: enabled
2018/04/26 15:29:44.867625 INFO [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] [Enable] current handler state is: notinstalled
2018/04/26 15:29:44.959605 INFO Event: name=Microsoft.OSTCExtensions.CustomScriptForLinux, op=Download, message=Download succeeded, duration=59
2018/04/26 15:29:44.993269 INFO [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Initialize extension directory
2018/04/26 15:29:45.022972 INFO [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Update settings file: 0.settings
2018/04/26 15:29:45.051763 INFO [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Install extension [customscript.py -install]
2018/04/26 15:29:45 CustomScriptForLinux started to handle.
2018/04/26 15:29:45 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] cwd is /var/lib/waagent/Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2
2018/04/26 15:29:45 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Change log file to /var/log/azure/Microsoft.OSTCExtensions.CustomScriptForLinux/1.5.2.2/extension.log
2018/04/26 15:29:46.088212 INFO Event: name=Microsoft.OSTCExtensions.CustomScriptForLinux, op=Install, message=Launch command succeeded: customscript.py -install, duration=1005
2018/04/26 15:29:46.133367 INFO [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Enable extension [customscript.py -enable]
2018/04/26 15:29:46 CustomScriptForLinux started to handle.
..
2018/04/26 15:29:47.178163 INFO Event: name=Microsoft.OSTCExtensions.CustomScriptForLinux, op=Enable, message=Launch command succeeded: customscript.py -enable, duration=1012
```
Dikkat edilecek bazı noktalar:
1. Komut çalışmaya başladığında Etkinleştir ' dir.
2. İndirme CustomScript uzantısını paketi Azure'dan, indirmek üzere komut dosyaları içinde fileUris belirtilen ilişkilendirir.
3. '/Var/log/azure/Microsoft.OSTCExtensions.CustomScriptForLinux/1.5.2.2/extension.log' için yazma, hangi günlük dosyası da görebilirsiniz.

Sonraki adım bir denetim günlük dosyası gitmek için bu biçimi:
```bash
/var/log/azure/<extension-name>/<version>/extension.log file.
```

İnduvidual yürütme için göz önünde bulundurmanız gerekenler, şu şekilde görünür:
```text
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Enable,transitioning,0,Launching the script...
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] sequence number is 0
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] setting file path is/var/lib/waagent/Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2/config/0.settings
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] JSON config: {"runtimeSettings": [{"handlerSettings": {"protectedSettings": "MIIB0AYJKoZIhvcNAQcDoIIBwTCCAb0CAQAxggF+hnEXRtFKTTuKiFC8gTfHKupUSs7qI0zFYRya", "publicSettings": {"fileUris": ["https://dannytesting.blob.core.windows.net/demo/myBash.sh"]}, "protectedSettingsCertThumbprint": "4385AB21617C2452FF6998C0A37F71A0A01C8368"}}]}
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Config decoded correctly.
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Will try to download files, number of retries = 10, wait SECONDS between retrievals = 20s
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Downloading,transitioning,0,Downloading files...
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] No azure storage account and key specified in protected settings. Downloading scripts from external links...
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Converting /var/lib/waagent/Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2/download/0/myBash.sh from DOS to Unix formats: Done
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Removing BOM of /var/lib/waagent/Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2/download/0/myBash.sh: Done
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Succeeded to download files, retry count = 0
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Internal DNS is ready, retry count = 0
2018/04/26 15:29:47 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Command is finished.
2018/04/26 15:29:47 ---stdout---
2018/04/26 15:29:47 
2018/04/26 15:29:47 ---errout---
2018/04/26 15:29:47 
2018/04/26 15:29:47 
2018/04/26 15:29:47 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Daemon,success,0,Command is finished.
2018/04/26 15:29:47 ---stdout---
2018/04/26 15:29:47 
2018/04/26 15:29:47 ---errout---
2018/04/26 15:29:47 
2018/04/26 15:29:47 
```
Burada görebilirsiniz:
* Bu günlük dosyası olduğu etkinleştir komutu başlatılıyor
* Uzantı geçirilen ayarlar
* Dosya ve bu sonucu indirme uzantısı.
* Çalıştırılan komut ve sonucu.

Azure CLI kullanarak özel betik uzantısının yürütme durumunu da alabilir:

```azurecli
az vm extension list -g myResourceGroup --vm-name myVM
```

Çıktı aşağıdaki metni gibi görünür:

```azurecli
Name                  ProvisioningState    Publisher                   Version  AutoUpgradeMinorVersion
--------------------  -------------------  ------------------------  ---------  -------------------------
CustomScriptForLinux  Succeeded            Microsoft.OSTCExtensions        1.5  True
```

## <a name="next-steps"></a>Sonraki adımlar
Kodu, geçerli sorun ve sürümleri için bkz [CustomScript uzantısını depodaki](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript).

