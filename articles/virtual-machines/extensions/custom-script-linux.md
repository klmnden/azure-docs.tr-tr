---
title: Azure'daki Linux vm'lerinde özel betik çalıştırma | Microsoft Docs
description: Özel betik uzantısı v2'yi kullanarak Linux VM yapılandırma görevlerini otomatikleştirme
services: virtual-machines-linux
documentationcenter: ''
author: roiyz-msft
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
ms.author: roiyz
ms.openlocfilehash: b9bc3ef0cf5dd54802d32058afb904800c364c19
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64725239"
---
# <a name="use-the-azure-custom-script-extension-version-2-with-linux-virtual-machines"></a>Azure özel betik uzantısı sürüm 2 ile Linux sanal makineleri kullanın.
Özel betik uzantısı sürüm 2 indirir ve Azure sanal makinelerinde betikleri çalıştırır. Bu uzantı dağıtım sonrası yapılandırma, yazılım yükleme veya başka bir yapılandırma/yönetim görevi için kullanışlıdır. Betikler Azure depolama veya başka bir erişilebilir internet konuma indirebilir veya uzantı çalışma zamanında sağlayabilir. 

Özel betik uzantısı, Azure Resource Manager şablonları ile tümleşir. Uygulamayı, Azure CLI, PowerShell veya Azure sanal makineler REST API kullanarak da çalıştırabilirsiniz.

Bu makalede, Azure clı'dan özel betik uzantısı kullanma ve bir Azure Resource Manager şablonu kullanarak uzantı çalıştırma ayrıntıları. Bu makalede, Linux sistemleri için sorun giderme adımları da sağlanır.


İki Linux özel betik uzantısı vardır:
* Sürüm 1 - Microsoft.OSTCExtensions.CustomScriptForLinux
* Sürüm 2 - Microsoft.Azure.Extensions.CustomScript

Lütfen bunun yerine yeni sürüm 2 kullanmak için yeni ve mevcut dağıtımları geçin. Yeni sürüm, mongodb'nin olması amaçlanmıştır. Bu nedenle, geçiş adı ve sürümü değiştirirken oldukça kolaydır, uzantı yapılandırmanızı değiştirmeniz gerekmez.


### <a name="operating-system"></a>İşletim sistemi

Özel betik uzantısı için Linux işletim sisteminin, daha fazla bilgi için desteklenen uzantısı uzantısı çalışmayacak görmeniz [makale](https://support.microsoft.com/en-us/help/4078134/azure-extension-supported-operating-systems).

### <a name="script-location"></a>Betik konumu

Azure Blob depolamaya erişmek için Azure Blob Depolama kimlik bilgilerini kullanmak için uzantıyı kullanabilirsiniz. Alternatif olarak, VM iç dosya sunucusu vb. gibi GitHub, o uç noktasına yönlendirebilir sürece betik konumu herhangi where, olabilir.

### <a name="internet-connectivity"></a>Internet bağlantısı
Harici olarak GitHub ya da Azure depolama gibi bir betik indirmeniz gerekiyorsa, ek güvenlik duvarı/ağ güvenlik grubu bağlantı noktalarının açılması gerekir. Örneğin betiğinizi Azure Depolama'da bulunuyorsa, size izin verebilirsiniz erişmek için Azure NSG hizmet etiketleri kullanarak [depolama](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags).

Kodunuzu yerel bir sunucusundaysa sonra ek güvenlik duvarı/ağ güvenlik hala gerekebilir grup bağlantı noktaları açılması gerekir.

### <a name="tips-and-tricks"></a>İpuçları ve Püf Noktaları
* Bu uzantı için en çok karşılaşılan hatalar, betikteki söz dizimi hatalarıdır. Betiğin hatasız bir şekilde çalıştığından emin olun ve ayrıca hata noktalarını daha kolay bir şekilde belirlemek için betiğe ek günlük kaydı işlevleri ekleyin.
* Bir kez etkili betikler yazın. Bu sayede yanlışlıkla birden fazla çalıştırılan betikler, sistemde değişiklik gerçekleştirilmesine neden olmaz.
* Betikleri çalıştırdıklarında kullanıcı girişi gerektirmeyen emin olun.
* Çalıştırılacak betik için izin verilen 90 dakika, başarısız bir sağlama uzantının uzun herhangi bir şey neden olur.
* Yeniden başlatma komut dosyası içine koymayın bu yüklenmekte olan diğer uzantılarla sorunlarına neden olur ve sonrası yeniden başlatma, uzantıyı yeniden başlatma sonrasında devam etmez. 
* Yeniden başlatmaya neden olacak bir betiğiniz varsa uygulamaları yükleyip betikleri sonra çalıştırma yöntemini izleyin. Bir sıralanmış işin veya DSC veya Chef, Puppet uzantıları gibi araçları kullanarak yeniden zamanlamanız gerekir.
* Kullanabileceğiniz sonra her önyükleme üzerinde bir komut dosyası çalıştırmak istiyorsanız, uzantı yalnızca bir komut dosyası bir kez çalışır [cloud-init görüntü](https://docs.microsoft.com/azure/virtual-machines/linux/using-cloud-init) ve bir [betikleri başına önyükleme](https://cloudinit.readthedocs.io/en/latest/topics/modules.html#scripts-per-boot) modülü. Alternatif olarak, bir Systemd hizmeti birim oluşturmak için komut dosyasını kullanabilirsiniz.
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
  "type": "Microsoft.Compute/virtualMachines/extensions",
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
      "timestamp":123456789          
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
| apiVersion | 2015-06-15 | date |
| publisher | Microsoft.Compute.Extensions | string |
| type | CustomScript | string |
| typeHandlerVersion | 2.0 | int |
| fileUris (örn.) | https://github.com/MyProject/Archive/MyPythonScript.py | array |
| commandToExecute (örn.) | Python MyPythonScript.py \<param1 my > | string |
| script | IyEvYmluL3NoCmVjaG8gIlVwZGF0aW5nIHBhY2thZ2VzIC4uLiIKYXB0IHVwZGF0ZQphcHQgdXBncmFkZSAteQo= | string |
| skipDos2Unix (örn.) | false | boolean |
| timestamp (örn.) | 123456789 | 32 bit tamsayı |
| storageAccountName (örn.) | examplestorageacct | string |
| storageAccountKey (örn.) | TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg== | string |

### <a name="property-value-details"></a>Özellik değeri ayrıntıları
* `skipDos2Unix`: (isteğe bağlı, boolean) dos2unix dönüştürme betiği tabanlı dosya URL'lerini veya betiği atlayın.
* `timestamp` Bu alan yalnızca betiğini yeniden çalıştırmak bu alanın değerini değiştirerek tetiklemek için (isteğe bağlı, 32 bit tamsayı) kullanın.  Kabul edilebilir tamsayı değerdeki; yalnızca önceki değerinden farklı olmalıdır.
  * `commandToExecute`: (**gerekli** betik ayarlanmamış olması halinde, string) yürütmek için giriş noktası betiği. Bu alan, bunun yerine komutunuz parolalar gibi gizli dizileri içeriyorsa kullanın.
* `script`: (**gerekli** commandToExecute ayarlanmamış olması halinde, string) bir base64 kodlu (ve isteğe bağlı olarak gzip'ed) / bin/sh yürütülen kod.
* `fileUris`: (isteğe bağlı, dize dizisi) dosyaların indirilmesi için URL.
* `storageAccountName`: (isteğe bağlı, dize) depolama hesabı adı. Depolama kimlik bilgileri, belirtirseniz, tüm `fileUris` URL'leri, Azure BLOB'ları için olmalıdır.
* `storageAccountKey`: (isteğe bağlı, dize) depolama hesabı erişim anahtarı


Ortak veya korumalı ayarlarında aşağıdaki değerleri ayarlayabilirsiniz Bu, uzantı hem genel hem de korumalı ayarlarında aşağıdaki değerleri ayarlandığı herhangi bir yapılandırma reddeder.
* `commandToExecute`
* `script`
* `fileUris`

Hata ayıklama, ancak için genel ayarları yararlı olabilir kullanarak korumalı ayarları kullanmanız önerilir.

Genel ayarlar, betik yürütüldüğü VM düz metin olarak gönderilir.  Korumalı ayarları, yalnızca Azure ve VM bildiği bir anahtar kullanılarak şifrelenir. Gönderildiği gibi VM ayarları kaydedildi, ayarları şifrelenmiş yani bunlar şifrelenmiş VM üzerinde kaydedilir. Şifrelenmiş değerler şifresini çözmek için kullanılan sertifika, VM üzerinde depolanan ve çalışma zamanında ayarlarını (gerekiyorsa) şifresini çözmek için kullanılan.

#### <a name="property-skipdos2unix"></a>Özellik: skipDos2Unix

Varsayılan değer false dos2unix dönüştürme anlamına gelir: **olduğu** yürütülür.

CustomScript, Microsoft.OSTCExtensions.CustomScriptForLinux, önceki sürümünü otomatik olarak DOS dosyaları UNIX dosyaları çevirerek dönüştürecektir `\r\n` için `\n`. Bu çeviri hala var ve varsayılan olarak açıktır. Bu dönüştürme fileUris veya aşağıdaki ölçütleri birini ayarlayarak betiği indirilen tüm dosyaları uygulanır.

* Uzantı ise `.sh`, `.txt`, `.py`, veya `.pl` dönüştürülmesi. Çünkü /bin/sh ile çalıştırılan bir betik olarak kabul edilir ve VM'de script.sh kaydedilen ayarlayarak betiği her zaman bu ölçütlere uyan.
* Dosya ile başlarsa `#!`.

Dos2unix dönüştürme skipDos2Unix true olarak ayarlayarak atlanabilir.

```json
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>",
  "skipDos2Unix": true
}
```

####  <a name="property-script"></a>Özellik: betik

CustomScript bir kullanıcı tanımlı betik yürütme işlemi destekler. İçinde tek bir ayar commandToExecute ve fileUris birleştirmek için betik ayarları. Kurulum bir dosyası Azure depolama veya GitHub gist indirmek zorunda kalmak yerine bir ayarı olarak betik yalnızca şifreleyebilirsiniz. Betik, değiştirilen commandToExecute ve fileUris için kullanılabilir.

Betik **gerekir** Base64 ile kodlanmış olmalıdır.  Komut dosyası için **isteğe bağlı olarak** gzip'ed olabilir. Ortak veya korumalı ayarlarında komut dosyası ayarı kullanılabilir. Betik parametrenin veri en büyük boyutu 256 KB'dir. Komut dosyası bu boyutu aşarsa yürütülmez.

Aşağıdaki komut dosyası için dosya /script.sh/ kaydedilmiş verilen örneğin.

```sh
#!/bin/sh
echo "Updating packages ..."
apt update
apt upgrade -y
```

Aşağıdaki komut çıktısı yararlanarak doğru CustomScript komut dosyası ayarı oluşturulması.

```sh
cat script.sh | base64 -w0
```

```json
{
  "script": "IyEvYmluL3NoCmVjaG8gIlVwZGF0aW5nIHBhY2thZ2VzIC4uLiIKYXB0IHVwZGF0ZQphcHQgdXBncmFkZSAteQo="
}
```

Betik, isteğe bağlı olarak daha da (çoğu durumda) boyutunu azaltmak için gzip'ed olabilir. (CustomScript otomatik olarak gzip sıkıştırma kullanımını algılar.)

```sh
cat script | gzip -9 | base64 -w 0
```

CustomScript bir betik yürütmek için aşağıdaki algoritması kullanır.

 1. betiğin değerin uzunluğu 256 KB aşmadığından onay.
 1. Base64 kod çözme betiğin değeri
 1. _Deneme_ gunzip için base64 değeri çözülmüş
 1. kodu çözülen (ve isteğe bağlı olarak sıkıştırması açılmış) değeri diske yazma (/ var/lib/waagent/custom-script/#/script.sh)
 1. _ / bin/sh - c /var/lib/waagent/custom-script/#/script.sh kullanarak betiği yürütün.


## <a name="template-deployment"></a>Şablon dağıtımı
Azure VM uzantıları Azure Resource Manager şablonları ile dağıtılabilir. Önceki bölümde açıklanan JSON şeması bir Azure Resource Manager şablonunda bir Azure Resource Manager şablon dağıtımı sırasında özel betik uzantısı'nı çalıştırmak için kullanılabilir. Özel betik uzantısı'nı içeren bir örnek şablonu burada bulunabilir [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).


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
      "fileUris": ["https://github.com/MyProject/Archive/hello.sh"
      ]  
    }
  }
}
```

>[!NOTE]
>Bu özellik adları büyük/küçük harfe duyarlıdır. Dağıtım sorunları önlemek için burada gösterildiği gibi adları kullanın.

## <a name="azure-cli"></a>Azure CLI
Özel betik uzantısı'nı çalıştırmak için Azure CLI'yı kullanırken, bir yapılandırma dosyası veya dosya oluşturun. En azından 'commandToExecute' olmalıdır.

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --protected-settings ./script-config.json
```

İsteğe bağlı olarak, JSON biçimli dize olarak komutta ayarları belirtebilirsiniz. Bu, yürütme sırasında ve ayrı bir yapılandırma dosyası olmadan belirtilmesi için yapılandırmayı sağlar.

```azurecli
az vm extension set \
  --resource-group exttest \
  --vm-name exttest \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --protected-settings '{"fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"],"commandToExecute": "./config-music.sh"}'
```

### <a name="azure-cli-examples"></a>Azure CLI örnekleri

#### <a name="public-configuration-with-script-file"></a>Komut dosyası ile genel yapılandırma

```json
{
  "fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"],
  "commandToExecute": "./config-music.sh"
}
```

Azure CLI komutunu:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --settings ./script-config.json
```

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
  --vm-name myVM --name customScript \
  --publisher Microsoft.Azure.Extensions \
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
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \ 
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --settings ./script-config.json \
  --protected-settings ./protected-config.json
```

## <a name="troubleshooting"></a>Sorun giderme
Özel betik uzantısı'nı çalıştırdığında, komut dosyası oluşturulduğunda veya aşağıdaki örneğe benzer bir dizine indirilir. Komut çıktısı Ayrıca bu dizinde kaydedilir `stdout` ve `stderr` dosyaları.

```bash
/var/lib/waagent/custom-script/download/0/
```

Sorun giderin, ilk Linux Aracısı günlüğünü kontrol edin, uzantıyı çalıştırdığınız, emin olmak için kontrol edin:

```bash
/var/log/waagent.log 
```

Uzantı yürütme için göz önünde bulundurmanız gerekenler, bunun aşağıdaki gibi görünür:
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
2. İndirme CustomScript uzantısı paketi yükleme Azure'nın sunduğu değildir komut dosyalarını fileUris içinde belirtilen ilişkilendirir.


Azure betik uzantısı, burada bulduğunuz bir günlük üretir:

```bash
/var/log/azure/custom-script/handler.log
```

Tek yürütme için göz önünde bulundurmanız gerekenler, bunun aşağıdaki gibi görünür:
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
info:    Executing command vm extension get
+ Looking up the VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a>Sonraki adımlar
Kod, geçerli sorun ve sürümleri için bkz [özel betik uzantısı linux depo](https://github.com/Azure/custom-script-extension-linux).

