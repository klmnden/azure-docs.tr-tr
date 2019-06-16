---
title: Azure'daki Linux vm'lerinde özel betik çalıştırma | Microsoft Docs
description: Özel betik uzantısı v1 kullanarak Linux VM yapılandırma görevlerini otomatikleştirme
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
ms.date: 08/14/2018
ms.author: danis
ms.openlocfilehash: fe3803b7dc75ab13831a5e42d4b1a96f5aa894e5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60800294"
---
# <a name="use-the-azure-custom-script-extension-version-1-with-linux-virtual-machines"></a>Azure özel betik uzantısı sürüm 1 ile Linux sanal makineleri kullanın.

[!INCLUDE [virtual-machines-extensions-deprecation-statement](../../../includes/virtual-machines-extensions-deprecation-statement.md)]

Özel betik uzantısı sürüm 1 indirir ve Azure sanal makinelerinde betikleri çalıştırır. Bu uzantı dağıtım sonrası yapılandırma, yazılım yükleme veya başka bir yapılandırma/yönetim görevi için kullanışlıdır. Betikler Azure depolama veya başka bir erişilebilir internet konuma indirebilir veya uzantı çalışma zamanında sağlayabilir.

Özel betik uzantısı, Azure Resource Manager şablonları ile tümleşir. Uygulamayı, Azure CLI, PowerShell, Azure portalında veya Azure sanal makineler REST API kullanarak da çalıştırabilirsiniz.

Bu makalede, Azure clı'dan özel betik uzantısı kullanma ve bir Azure Resource Manager şablonu kullanarak uzantı çalıştırma ayrıntıları. Bu makalede, Linux sistemleri için sorun giderme adımları da sağlanır.

İki Linux özel betik uzantısı vardır:

* Sürüm 1 - Microsoft.OSTCExtensions.CustomScriptForLinux

* Sürüm 2 - Microsoft.Azure.Extensions.CustomScript

Lütfen yeni sürümü kullanmak için yeni ve mevcut dağıtımları geçin ([Microsoft.Azure.Extensions.CustomScript](custom-script-linux.md)) bunun yerine. Yeni sürüm, mongodb'nin olması amaçlanmıştır. Bu nedenle, geçiş adı ve sürümü değiştirirken oldukça kolaydır, uzantı yapılandırmanızı değiştirmeniz gerekmez.

### <a name="operating-system"></a>İşletim sistemi

Desteklenen Linux dağıtımları:

* CentOS 6.5 ve üzeri
* Debian 8 ve üzeri
  * Debian 8,7 son görüntülerde hangi CustomScriptForLinux sonları ile Python2 gelmez.
* FreeBSD
* OpenSUSE 13.1 ve üzeri
* Oracle Linux 6.4 ve daha yüksek
* SUSE Linux Enterprise Server 11 SP3 ve üzeri
* Ubuntu 12.04 ve üzeri

### <a name="script-location"></a>Betik konumu

Azure Blob depolamaya erişmek için Azure Blob Depolama kimlik bilgilerini kullanmak için uzantıyı kullanabilirsiniz. Alternatif olarak, VM iç dosya sunucusu vb. gibi GitHub, o uç noktasına yönlendirebilir sürece betik konumu herhangi where, olabilir.

### <a name="internet-connectivity"></a>Internet bağlantısı

Harici olarak GitHub ya da Azure depolama gibi bir betik indirmeniz gerekiyorsa, ek güvenlik duvarı/ağ güvenlik grubu bağlantı noktalarının açılması gerekir. Örneğin betiğinizi Azure Depolama'da bulunuyorsa, size izin verebilirsiniz erişmek için Azure NSG hizmet etiketleri kullanarak [depolama](../../virtual-network/security-overview.md#service-tags).

Kodunuzu yerel bir sunucusundaysa sonra ek güvenlik duvarı/ağ güvenlik hala gerekebilir grup bağlantı noktaları açılması gerekir.

### <a name="tips-and-tricks"></a>İpuçları ve Püf Noktaları

* Bu uzantı için en çok karşılaşılan hatalar, betikteki söz dizimi hatalarıdır. Betiğin hatasız bir şekilde çalıştığından emin olun ve ayrıca hata noktalarını daha kolay bir şekilde belirlemek için betiğe ek günlük kaydı işlevleri ekleyin.
* Bir kez etkili betikler yazın. Bu sayede yanlışlıkla birden fazla çalıştırılan betikler, sistemde değişiklik gerçekleştirilmesine neden olmaz.
* Betikleri çalıştırdıklarında kullanıcı girişi gerektirmeyen emin olun.
* Çalıştırılacak betik için izin verilen 90 dakika, başarısız bir sağlama uzantının uzun herhangi bir şey neden olur.
* Yeniden başlatma komut dosyası içine koymayın bu yüklenmekte olan diğer uzantılarla sorunlarına neden olur ve sonrası yeniden başlatma, uzantıyı yeniden başlatma sonrasında devam etmez. 
* Yeniden başlatmaya neden olacak bir betiğiniz varsa uygulamaları yükleyip betikleri sonra çalıştırma yöntemini izleyin. Bir sıralanmış işin veya DSC veya Chef, Puppet uzantıları gibi araçları kullanarak yeniden zamanlamanız gerekir.
* Kullanabileceğiniz sonra her önyükleme üzerinde bir komut dosyası çalıştırmak istiyorsanız, uzantı yalnızca bir komut dosyası bir kez çalışır [cloud-init görüntü](../linux/using-cloud-init.md) ve bir [betikleri başına önyükleme](https://cloudinit.readthedocs.io/en/latest/topics/modules.html#scripts-per-boot) modülü. Alternatif olarak, bir Systemd hizmeti birim oluşturmak için komut dosyasını kullanabilirsiniz.
* Çalışacak bir betik zamanlama istiyorsanız, bir sıralanmış iş oluşturmak için uzantıyı kullanmanız gerekir.
* Betik çalışırken Azure portalı veya CLI üzerinden uzantı durumunu yalnızca "geçiş durumunda" şeklinde görürsünüz. Çalışan bir betiğin daha sık aralıklı durum güncelleştirmeleri isterseniz, kendi çözümünüzü oluşturmak gerekir.
* Özel betik uzantısı yerel olarak proxy sunucularını desteklemez, ancak komut dosyanızın içinde proxy sunucuları gibi destekleyen bir dosya aktarım aracı kullanabilir *Curl*.
* Betikler veya komutlar System.Environment.UserInteractive varsayılan olmayan dizin konumlarını unutmayın, bu durumu çözmek için mantığı vardır.

## <a name="extension-schema"></a>Uzantı şeması

Betik konumu ve çalıştırılacak komutu gibi özel betik uzantısı yapılandırmasını belirtir. Bu yapılandırma, yapılandırma dosyalarında depolayın, komut satırında belirtin veya bir Azure Resource Manager şablonu belirtin. 

Hassas veriler şifrelenir ve bunların şifresini yalnızca sanal makinenin içinde bir korumalı bir yapılandırma depolayabilirsiniz. Korumalı yapılandırma yürütme komutu bir parola eşdeğerindeki içerdiğinde yararlıdır.

Bu öğeler hassas verisi olarak kabul edilir ve uzantıları korumalı ayarı yapılandırmasında belirtilen. Azure VM uzantısının korumalı ayarı veriler şifrelenir ve yalnızca hedef sanal makinede şifresi.

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
| apiVersion | 2015-06-15 | date |
| publisher | Microsoft.OSTCExtensions | string |
| türü | CustomScriptForLinux | string |
| typeHandlerVersion | 1,5 | int |
| fileUris (örn.) | https://github.com/MyProject/Archive/MyPythonScript.py | array |
| commandToExecute (örn.) | Python MyPythonScript.py \<param1 my\> | string |
| enableInternalDNSCheck | true | boole |
| storageAccountName (örn.) | examplestorageacct | string |
| storageAccountKey (örn.) | TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg== | string |

### <a name="property-value-details"></a>Özellik değeri ayrıntıları

* `fileUris`: (isteğe bağlı, dize dizisi) betikleri URI listesi
* `enableInternalDNSCheck`: (isteğe bağlı, Boole) varsayılan değer True, DNS denetimi devre dışı bırakmak için False olarak ayarlayın.
* `commandToExecute`: (isteğe bağlı, dize) yürütmek için giriş noktası betiği
* `storageAccountName`: (isteğe bağlı, dize) depolama hesabı adı
* `storageAccountKey`: (isteğe bağlı, dize) depolama hesabı erişim anahtarı

Ortak veya korumalı ayarlarında aşağıdaki değerleri ayarlayabilirsiniz Bu, hem genel hem de korumalı ayarlarında bu değerleri kümesi olmamalıdır.

* `commandToExecute`

Hata ayıklama, ancak için genel ayarları yararlı olabilir kullanarak korumalı ayarları kullanmanız önerilir.

Genel ayarlar, betik yürütüldüğü VM düz metin olarak gönderilir.  Korumalı ayarları, yalnızca Azure ve VM bildiği bir anahtar kullanılarak şifrelenir. Gönderildiği gibi VM ayarları kaydedildi, ayarları şifrelenmiş yani bunlar şifrelenmiş VM üzerinde kaydedilir. Şifrelenmiş değerler şifresini çözmek için kullanılan sertifika, VM üzerinde depolanan ve çalışma zamanında ayarlarını (gerekiyorsa) şifresini çözmek için kullanılan.

## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları Azure Resource Manager şablonları ile dağıtılabilir. Önceki bölümde açıklanan JSON şeması bir Azure Resource Manager şablonunda bir Azure Resource Manager şablon dağıtımı sırasında özel betik uzantısı'nı çalıştırmak için kullanılabilir.

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
>Bu özellik adları büyük/küçük harfe duyarlıdır. Dağıtım sorunları önlemek için burada gösterildiği gibi adları kullanın.

## <a name="azure-cli"></a>Azure CLI

Özel betik uzantısı'nı çalıştırmak için Azure CLI'yı kullanırken, bir yapılandırma dosyası veya dosya oluşturun. En azından 'commandToExecute' olmalıdır.

```azurecli
az vm extension set -n VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.5 \
  --vm-name MyVm --resource-group MyResourceGroup \
  --protected-settings '{"commandToExecute": "echo hello"}'
```

İsteğe bağlı olarak, JSON biçimli dize olarak komutta ayarları belirtebilirsiniz. Bu, yürütme sırasında ve ayrı bir yapılandırma dosyası olmadan belirtilmesi için yapılandırmayı sağlar.

```azurecli
az vm extension set \
  --resource-group exttest \
  --vm-name exttest \
  --name CustomScriptForLinux \
  --publisher Microsoft.OSTCExtensions \
  --settings '{"fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"],"commandToExecute": "./config-music.sh"}'
```

### <a name="azure-cli-examples"></a>Azure CLI örnekleri

#### <a name="public-configuration-with-no-script-file"></a>Genel yapılandırma ile betik dosyası yok

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Azure CLI komutunu:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name CustomScriptForLinux \
  --publisher Microsoft.OSTCExtensions \
  --settings ./script-config.json
```

#### <a name="public-and-protected-configuration-files"></a>Ortak ve korunan yapılandırma dosyaları

Genel yapılandırma dosyası, URI komut dosyasını belirtmek için kullanın. Çalıştırılacak komutu belirtmek için korumalı yapılandırma dosyası kullanın.

Genel yapılandırma dosyası:

```json
{
  "fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"]
}
```

Korumalı bir yapılandırma dosyası:  

```json
{
  "commandToExecute": "./config-music.sh <param1>"
}
```

Azure CLI komutunu:

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

Özel betik uzantısı'nı çalıştırdığında, komut dosyası oluşturulduğunda veya aşağıdaki örneğe benzer bir dizine indirilir. Komut çıktısı Ayrıca bu dizinde kaydedilir `stdout` ve `stderr` dosyaları.

```bash
/var/lib/waagent/Microsoft.OSTCExtensions.CustomScriptForLinux-<version>/download/1
```

Sorun giderin, ilk Linux Aracısı günlüğünü kontrol edin, uzantıyı çalıştırdığınız, emin olmak için kontrol edin:

```bash
/var/log/waagent.log
```

Uzantı yürütme için göz önünde bulundurmanız gerekenler, bunun aşağıdaki gibi görünür:

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
1. İndirme CustomScript uzantısı paketi yükleme Azure'nın sunduğu değildir komut dosyalarını fileUris içinde belirtilen ilişkilendirir.
1. Hangi günlük dosyası, için yazma göz atabilirsiniz `/var/log/azure/Microsoft.OSTCExtensions.CustomScriptForLinux/1.5.2.2/extension.log`

Sonraki adım bir onay günlük dosyasına gitmek için bu biçimi şu şekildedir:

```bash
/var/log/azure/<extension-name>/<version>/extension.log file.
```

Tek yürütme için göz önünde bulundurmanız gerekenler, bunun aşağıdaki gibi görünür:

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

* Bu günlük olan etkinleştirme başlangıç komutu
* Uzantı geçirilen ayarları
* Dosya ve sonucu, indirme uzantısı.
* Çalıştırılan komut ve sonucu.

Ayrıca, Azure CLI kullanarak özel betik uzantısı yürütme durumunu alabilirsiniz:

```azurecli
az vm extension list -g myResourceGroup --vm-name myVM
```

Çıkışı şu metin gibi görünür:

```azurecli
Name                  ProvisioningState    Publisher                   Version  AutoUpgradeMinorVersion
--------------------  -------------------  ------------------------  ---------  -------------------------
CustomScriptForLinux  Succeeded            Microsoft.OSTCExtensions        1.5  True
```

## <a name="next-steps"></a>Sonraki adımlar

Kod, geçerli sorun ve sürümleri için bkz [CustomScript uzantısı depo](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript).
