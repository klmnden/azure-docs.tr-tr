---
title: Özel komut dosyaları Linux VM'ler için Azure'da çalışan | Microsoft Docs
description: Özel betik uzantısının v2 kullanarak Linux VM yapılandırma görevleri otomatikleştirme
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: cf17ab2b-8d7e-4078-b6df-955c6d5071c2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/25/2018
ms.author: danis
ms.openlocfilehash: 3977aa16878ea1e2d808376165f551ce5fb8d00f
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="use-the-azure-custom-script-extension-version-2-with-linux-virtual-machines"></a>Linux sanal makineleri ile kullanmak üzere Azure özel betik uzantısı sürüm 2
Özel betik uzantısı sürüm 2 indirir ve Azure sanal makinelerde betikleri çalıştırır. Bu uzantı, dağıtım sonrası yapılandırma, yazılım yükleme veya başka bir yapılandırma/yönetim görevi için yararlıdır. Azure Storage veya başka bir erişilebilir Internet konum komut dosyaları indirebilir veya uzantısı çalışma zamanına sağlayabilir. 

Özel betik uzantısının Azure Resource Manager şablonları ile tümleşir. Ayrıca Azure CLI, PowerShell, Azure portalında veya Azure sanal makineleri REST API'sini kullanarak çalıştırabilirsiniz.

Bu makalede Azure clı'dan özel betik uzantısının kullanmayı ve Azure Resource Manager şablonu kullanarak uzantısı çalıştırmaya nasıl ayrıntıları. Bu makalede, ayrıca Linux sistemleri için sorun giderme adımları sağlar.


İki Linux özel betik uzantısı vardır:
* Sürüm 1 - Microsoft.OSTCExtensions.CustomScriptForLinux
* Sürüm 2 - Microsoft.Azure.Extensions.CustomScript

Lütfen yeni sürüm 2 kullanmayı yeni ve varolan dağıtımları geçin. Yeni sürüm içeri kayma değiştirme olması amaçlanmıştır. Bu nedenle, geçiş adı ve sürümü değiştirirken kadar kolaydır, uzantısı yapılandırmanızı değiştirmeniz gerekmez.


### <a name="operating-system"></a>İşletim Sistemi

Özel betik uzantısının Linux işletim sistemlerine, daha fazla bilgi için desteklenen uzantısı uzantısı çalışmayacak için bkz: Bu [makale](https://support.microsoft.com/en-us/help/4078134/azure-extension-supported-operating-systems).

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
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "skipDos2Unix":false,
      "timestamp":"123456789",          
    },
    "protectedSettings": {
       "commandToExecute": "<command-to-execute>",
       "script": "<base64-script-to-execute>",
       "storageAccountName": "<storage-account-name>",
       "storageAccountKey": "<storage-account-key>",
       "fileUris": ["https://.."]  
    }
  }
}
```

### <a name="property-values"></a>Özellik değerleri

| Ad | Değer / örnek | Veri Türü | 
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | tarih |
| Yayımcı | Microsoft.Compute.Extensions | dize |
| type | CustomScript | dize |
| typeHandlerVersion | 2.0 | Int |
| fileUris (örneğin) | https://github.com/MyProject/Archive/MyPythonScript.py | array |
| commandToExecute (örneğin) | Python MyPythonScript.py < param1 my > | dize |
| Komut dosyası | IyEvYmluL3NoCmVjaG8gIlVwZGF0aW5nIHBhY2thZ2VzIC4uLiIKYXB0IHVwZGF0ZQphcHQgdXBncmFkZSAteQo = | dize |
| skipDos2Unix (örneğin) | false | boole |
| zaman damgası (örneğin) | 123456789 | 32 bit tamsayı |
| storageAccountName (örneğin) | examplestorageacct | dize |
| storageAccountKey (örneğin) | TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg== | dize |

### <a name="property-value-details"></a>Özellik değeri ayrıntıları
* `skipDos2Unix`: (isteğe bağlı, boolean) dos2unix dönüştürülmesi betik tabanlı bir dosya URL'leri veya komut dosyası atlayın.
* `timestamp` Bu alan yalnızca komut dosyasını yeniden çalıştırmak bu alanın değerini değiştirerek tetiklemek için (isteğe bağlı, 32 bit tamsayı) kullanın.  Kabul edilebilir herhangi bir tamsayı değerdir; yalnızca önceki değerinden farklı olmalıdır.
 * `commandToExecute`: (**gerekli** komut dosyası değil ayarlarsanız, dize) yürütmek için giriş noktası komut dosyası. Parolalar gibi gizli komutunuzu içeriyorsa, bu alan kullanın.
* `script`: (**gerekli** commandToExecute ayarlı değil, dize) bir base64 ile kodlanmış (ve isteğe bağlı olarak gzip'ed) yürütülen/bin/Paylaş komut dosyası.
* `fileUris`: (isteğe bağlı, dize dizisi) yüklenmek üzere dosyaları için URL'leri.
* `storageAccountName`: (isteğe bağlı, dize) depolama hesabının adı. Depolama kimlik belirtirseniz, tüm `fileUris` URL'ler için Azure BLOB'ları olmalıdır.
* `storageAccountKey`: (isteğe bağlı, dize) depolama hesabının erişim anahtarı


Aşağıdaki değerleri ortak ya da korumalı ayarlarında ayarlanabilir, uzantı hem genel hem de korumalı ayarlarında aşağıdaki değerleri belirlendiği herhangi bir yapılandırma reddeder.
* `commandToExecute`
* `script`
* `fileUris`

Hata ayıklama, ancak için genel ayarları belki de yararlı kullanarak korumalı ayarları kullanmanız önerilir.

Genel ayarları burada betik yürütülecek VM düz metin olarak gönderilir.  Korumalı ayarları yalnızca Azure ve VM bildiği bir anahtar kullanılarak şifrelenmiş. Gönderildikleri olarak ayarlar VM kaydedilir, ayarları şifrelediyseniz yani şifrelenmiş VM kaydedilirler. Şifrelenmiş değerler şifresini çözmek için kullanılan sertifika, VM üzerinde depolanır ve çalışma zamanında ayarlarını (gerekiyorsa) şifresini çözmek için kullanılan.

#### <a name="property-skipdos2unix"></a>Özellik: skipDos2Unix

Dos2unix dönüştürme başka bir deyişle, varsayılan değer false **olan** yürütülür.

CustomScript, Microsoft.OSTCExtensions.CustomScriptForLinux, önceki sürümü otomatik olarak DOS dosyaları UNIX dosyalara çevirerek dönüştürecektir `\r\n` için `\n`. Bu çeviri hala var olduğundan ve varsayılan olarak açıktır. Bu dönüştürme fileUris veya herhangi aşağıdaki ölçütlere göre komut dosyası ayarı indirilen tüm dosyaları uygulanır.

* Uzantı ise `.sh`, `.txt`, `.py`, veya `.pl` dönüştürülüp dönüştürülmeyeceğini. /Bin/sh ile çalıştırılan bir komut dosyası olarak kabul edilir ve VM script.sh kaydedilmiş olduğundan komut dosyası ayarı her zaman bu ölçütlere uyan.
* Dosya ile başlarsa `#!`.

Dos2unix dönüştürme skipDos2Unix true olarak ayarlayarak atlanabilir.

```json
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
  "skipDos2Unix": true
}
```

####  <a name="property-script"></a>Özellik: komut dosyası

CustomScript kullanıcı tanımlı betik yürütme işlemi destekler. Tek bir ayar içine commandToExecute ve fileUris birleştirmek için komut dosyası ayarları. Bir dosyayı Azure depolama veya GitHub gist indirmek için Kurulum gerek kalmadan yerine, komut dosyası ayarı olarak yalnızca şifreleyebilirsiniz. Komut dosyası, değiştirilen commandToExecute ve fileUris için kullanılabilir.

Komut dosyası **gerekir** Base64 ile kodlanmış olmalıdır.  Komut dosyası için **isteğe bağlı olarak** gzip'ed olabilir. Komut dosyası ayarı genel ya da korumalı ayarlarında kullanılabilir. Komut dosyası parametrenin veri en büyük boyutu 256 KB'tır. Komut dosyası bu boyutu aşarsa çalıştırılmayacak.

Dosya /script.sh/ kaydedilmiş aşağıdaki betiği verilen örneğin.

```sh
#!/bin/sh
echo "Updating packages ..."
apt update
apt upgrade -y
```

Aşağıdaki komut çıktısı gerçekleştirerek doğru CustomScript komut dosyası ayarı oluşturulması.

```sh
cat script.sh | base64 -w0
```

```json
{
  "script": "IyEvYmluL3NoCmVjaG8gIlVwZGF0aW5nIHBhY2thZ2VzIC4uLiIKYXB0IHVwZGF0ZQphcHQgdXBncmFkZSAteQo="
}
```

Komut dosyası, isteğe bağlı olarak daha da (çoğu durumda) boyutunu azaltmak için gzip'ed olabilir. (CustomScript otomatik-kullanımını gzip sıkıştırmasını algılar.)

```sh
cat script | gzip -9 | base64 -w 0
```

CustomScript bir betik yürütmek için aşağıdaki algoritmayı kullanır.

 1. komut dosyanızın değerinin uzunluğu 256 KB aşmayan assert.
 1. Base64 betiğin değeri kod çözme
 1. _denemek_ gunzip için base64 değeri kodunu çözdü.
 1. kodu çözülmüş (ve isteğe bağlı olarak açılmış) değeri diske yazma (/ var/lib/waagent/custom-script/#/script.sh)
 1. _ / bin/Paylaş - c /var/lib/waagent/custom-script/#/script.sh kullanarak betiğini yürütün.


## <a name="template-deployment"></a>Şablon dağıtımı
Azure VM uzantıları, Azure Resource Manager şablonları ile dağıtılabilir. Önceki bölümde ayrıntılı JSON şeması bir Azure Resource Manager şablonunda bir Azure Resource Manager şablon dağıtımı sırasında özel betik uzantısının çalıştırmak için kullanılabilir. Özel betik uzantısı içeren bir örnek şablonu burada bulunabilir [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).


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
      },
    "protectedSettings": {
      "commandToExecute": "sh hello.sh <param2>",
      "fileUris": ["https://github.com/MyProject/Archive/MyPythonScript.py"
      ]  
    }
  }
}
```

>[!NOTE]
>Bu özellik adları büyük/küçük harfe duyarlıdır. Dağıtım sorunlarını önlemek için aşağıda gösterildiği gibi adları kullanın.

## <a name="azure-cli"></a>Azure CLI
Özel betik uzantısının çalıştırmak için Azure CLI kullanırken bir yapılandırma dosyası veya dosya oluşturun. En azından 'commandToExecute' olması gerekir.

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --protected-settings ./script-config.json
```

İsteğe bağlı olarak, JSON biçimli dize olarak komutta ayarları belirtebilirsiniz. Yürütme sırasında ve ayrı yapılandırma dosyası olmadan belirtilmesi için yapılandırmasını sağlar.

```azurecli
az vm extension set \
  --resource-group exttest \
  --vm-name exttest \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --protected-settings '{"fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"],"commandToExecute": "./config-music.sh"}'
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
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --settings ./script-config.json
```

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
  --vm-name myVM --name customScript \
  --publisher Microsoft.Azure.Extensions \
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
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \ 
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --settings ./script-config.json \
  --protected-settings ./protected-config.json
```

## <a name="troubleshooting"></a>Sorun giderme
Özel betik uzantısının çalıştığında, komut dosyası oluşturulur veya aşağıdaki örneğe benzer bir dizine indirilir. Komut çıktısı da bu dizinine kaydedilir `stdout` ve `stderr` dosyaları.

```bash
/var/lib/waagent/custom-script/download/0/
```

Sorun giderme, önce Linux aracı günlüğüne bakın, uzantısı çalıştı, olun denetleyin:

```bash
/var/log/waagent.log 
```

Uzantı yürütme için göz önünde bulundurmanız gerekenler, şu şekilde görünür:
```text
2018/04/26 17:47:22.110231 INFO [Microsoft.Azure.Extensions.customScript-2.0.6] [Enable] current handler state is: notinstalled
2018/04/26 17:47:22.306407 INFO Event: name=Microsoft.Azure.Extensions.customScript, op=Download, message=Download succeeded, duration=167
2018/04/26 17:47:22.339958 INFO [Microsoft.Azure.Extensions.customScript-2.0.6] Initialize extension directory
2018/04/26 17:47:22.368293 INFO [Microsoft.Azure.Extensions.customScript-2.0.6] Update settings file: 0.settings
2018/04/26 17:47:22.394482 INFO [Microsoft.Azure.Extensions.customScript-2.0.6] Install extension [bin/custom-script-shim install]
2018/04/26 17:47:23.432774 INFO Event: name=Microsoft.Azure.Extensions.customScript, op=Install, message=Launch command succeeded: bin/custom-script-shim install, duration=1007
2018/04/26 17:47:23.476151 INFO [Microsoft.Azure.Extensions.customScript-2.0.6] Enable extension [bin/custom-script-shim enable]
2018/04/26 17:47:24.516444 INFO Event: name=Microsoft.Azure.Extensions.customScript, op=Enable, message=Launch command succeeded: bin/custom-sc
```
Dikkat edilecek bazı noktalar:
1. Komut çalışmaya başladığında Etkinleştir ' dir.
2. İndirme CustomScript uzantısını paketi Azure'dan, indirmek üzere komut dosyaları içinde fileUris belirtilen ilişkilendirir.


Azure betik uzantısı burada bulabilirsiniz bir günlük üretir:

```bash
/var/log/azure/custom-script/handler.log
```

İnduvidual yürütme için göz önünde bulundurmanız gerekenler, şu şekilde görünür:
```text
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event=start
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event=pre-check
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="comparing seqnum" path=mrseq
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="seqnum saved" path=mrseq
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="reading configuration"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="read configuration"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="validating json schema"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="json schema valid"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="parsing configuration json"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="parsed configuration json"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="validating configuration logically"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="validated configuration"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="creating output directory" path=/var/lib/waagent/custom-script/download/0
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="created output directory"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 files=1
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 file=0 event="download start"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 file=0 event="download complete" output=/var/lib/waagent/custom-script/download/0
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="executing command" output=/var/lib/waagent/custom-script/download/0
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="executing protected commandToExecute" output=/var/lib/waagent/custom-script/download/0
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="executed command" output=/var/lib/waagent/custom-script/download/0
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event=enabled
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event=end
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
info:    Executing command vm extension get
+ Looking up the VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a>Sonraki adımlar
Kodu, geçerli sorun ve sürümleri için bkz [özel betik uzantısı linux depodaki](https://github.com/Azure/custom-script-extension-linux).

