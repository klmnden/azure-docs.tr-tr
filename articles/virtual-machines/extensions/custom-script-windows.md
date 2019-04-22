---
title: Windows için Azure özel betik uzantısı | Microsoft Docs
description: Özel betik uzantısı'nı kullanarak Windows VM yapılandırma görevlerini otomatikleştirme
services: virtual-machines-windows
documentationcenter: ''
author: georgewallace
manager: carmonm
editor: ''
tags: azure-resource-manager
ms.assetid: f4181fee-7a9d-4a1c-b517-52956f5b7fa1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/05/2018
ms.author: gwallace
ms.openlocfilehash: 075813feadfb81fe8f7d337dfc5f7ba01bd41e86
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59698699"
---
# <a name="custom-script-extension-for-windows"></a>Windows için özel betik uzantısı

Özel betik uzantısı indirir ve Azure sanal makinelerinde betikleri çalıştırır. Bu uzantı dağıtım sonrası yapılandırma, yazılım yükleme veya diğer yapılandırma/yönetim görevleri için kullanışlıdır. Betikler Azure depolama veya GitHub konumlarından indirilebilir ya da Azure portalına uzantı çalışma zamanında iletilebilir. Özel Betik uzantısı, Azure Resource Manager şablonları ile tümleşir ve Azure CLI, PowerShell, Azure portalı veya Azure Sanal Makine REST API'si kullanılarak da çalıştırılabilir.

Bu belge, Azure PowerShell modülü, Azure Resource Manager şablonları ve sorun giderme adımları Windows sistemlerinde ayrıntıları kullanarak özel betik uzantısı kullanma işlemi açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

> [!NOTE]  
> Özel betik uzantısı, bu yana kendisine bekler, parametre olarak aynı VM ile Update-AzVM çalıştırmak için kullanmayın.  
>   
> 

### <a name="operating-system"></a>İşletim Sistemi

Özel betik uzantısı'nı Linux daha fazla bilgi için desteklenen uzantısı uzantısı OSs çalışmayacak için bkz: Bu [makale](https://support.microsoft.com/en-us/help/4078134/azure-extension-supported-operating-systems).

### <a name="script-location"></a>Betik konumu

Azure Blob depolamaya erişmek için Azure Blob Depolama kimlik bilgilerini kullanmak için uzantıyı kullanabilirsiniz. Alternatif olarak, VM iç dosya sunucusu vb. gibi GitHub, o uç noktasına yönlendirebilir sürece herhangi bir yerde, betik konumu olabilir.


### <a name="internet-connectivity"></a>Internet bağlantısı
Harici olarak GitHub ya da Azure depolama gibi bir betik indirmeniz gerekiyorsa, ek güvenlik duvarı/ağ güvenlik grubu bağlantı noktalarının açılması gerekir. Örneğin betiğinizi Azure Depolama'da bulunuyorsa, size izin verebilirsiniz erişmek için Azure NSG hizmet etiketleri kullanarak [depolama](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags).

Kodunuzu yerel bir sunucusundaysa sonra ek güvenlik duvarı/ağ güvenlik hala gerekebilir grup bağlantı noktaları açılması gerekir.

### <a name="tips-and-tricks"></a>İpuçları ve Püf Noktaları
* Bu uzantı için en yüksek hata oranı hata, komut dosyasını çalıştırır test komut dosyasında sözdizimi hataları kaynaklanır ve başarısız olduğu bulmak daha kolay hale getirmek için komut dosyası bir günlük daha koyun de.
* Eşgüçlüdür, komut dosyaları yazmak için yeniden birden çok kez yanlışlıkla çalıştırma alınamadı, da sistem değişiklikleri neden olmaz.
* Çalıştırdıklarında betikleri kullanıcı girişi gerektirmeyen emin olun.
* Çalıştırılacak betik için izin verilen 90 dakika, başarısız bir sağlama uzantının uzun herhangi bir şey neden olur.
* Yeniden başlatma komut dosyası içine koymayın, bu eylem, yüklenmekte olan diğer uzantılarla sorunlarına neden olur. Sonrası yeniden başlatma, yeniden başlatma sonrasında uzantısı devam olmaz. 
* Yeniden başlatma neden olacak bir betiğiniz varsa, uygulama yükleme ve betikler vb. çalıştırın. Windows zamanlanmış bir görev veya DSC veya Chef, Puppet uzantıları gibi araçları kullanarak yeniden başlatma zamanlayabilirsiniz.
* Uzantısı Windows zamanlanmış bir görev oluşturmak için kullanmanız gereken sonra her önyükleme, bir komut dosyası çalıştırmak istiyorsanız, uzantı yalnızca bir komut dosyası bir kez çalışır.
* Çalışacak bir betik zamanlama istiyorsanız, Windows zamanlanmış bir görev oluşturmak için uzantıyı kullanmanız gerekir. 
* Komut dosyası çalıştırılırken, yalnızca Azure portal veya CLI 'geçirmeyi' bir uzantı durumu görürsünüz. Çalışan bir betiğin daha sık aralıklı durum güncelleştirmeleri isterseniz kendi çözümünüzü oluşturmak gerekir.
* Özel betik uzantısı yerel olarak proxy sunucularını desteklemez, ancak komut dosyanızın içinde proxy sunucuları gibi destekleyen bir dosya aktarım aracı kullanabilir *Curl* 
* Varsayılan olmayan dikkat, betikler veya komutlar ihtiyaç duyabilir dizin konumları sahip bu durumu yönetmek için mantığı.


## <a name="extension-schema"></a>Uzantı şeması

Betik konumu ve çalıştırılacak komutu gibi özel betik uzantısı yapılandırmasını belirtir. Bu yapılandırma, yapılandırma dosyalarında depolayın, komut satırında belirtin veya bir Azure Resource Manager şablonu belirtin. 

Hassas veriler şifrelenir ve bunların şifresini yalnızca sanal makinenin içinde bir korumalı bir yapılandırma depolayabilirsiniz. Korumalı yapılandırma yürütme komutu bir parola eşdeğerindeki içerdiğinde yararlıdır.

Bu öğeler hassas verisi olarak kabul edilir ve uzantıları korumalı ayarı yapılandırmasında belirtilen. Azure VM uzantısının korumalı ayarı veriler şifrelenir ve yalnızca hedef sanal makinede şifresi.

```json
{
    "apiVersion": "2018-06-01",
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
        "[variables('musicstoresqlName')]"
    ],
    "tags": {
        "displayName": "config-app"
    },
    "properties": {
        "publisher": "Microsoft.Compute",
        "type": "CustomScriptExtension",
        "typeHandlerVersion": "1.9",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "fileUris": [
                "script location"
            ],
            "timestamp":123456789
        },
        "protectedSettings": {
            "commandToExecute": "myExecutionCommand",
            "storageAccountName": "myStorageAccountName",
            "storageAccountKey": "myStorageAccountKey"
        }
    }
}
```
**Not** -uzantı yalnızca bir sürümü yüklenebilir bir noktada bir VM'de zaman içinde aynı sanal makine başarısız olacağına ilişkin özel betik iki kez aynı Resource Manager şablonunda belirtme. 

### <a name="property-values"></a>Özellik değerleri

| Ad | Değer / örnek | Veri Türü |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| Yayımcı | Microsoft.Compute | string |
| type | CustomScriptExtension | string |
| typeHandlerVersion | 1.9 | int |
| fileUris (örn.) | https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1 | array |
| zaman damgası (örn.) | 123456789 | 32 bit tamsayı |
| commandToExecute (örn.) | PowerShell - ExecutionPolicy sınırsız - dosya yapılandırma-müzik-app.ps1 | string |
| storageAccountName (örn.) | examplestorageacct | string |
| storageAccountKey (örn.) | TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg== | string |

>[!NOTE]
>Bu özellik adları büyük/küçük harfe duyarlıdır. Dağıtım sorunları önlemek için burada gösterildiği gibi adları kullanın.

#### <a name="property-value-details"></a>Özellik değeri ayrıntıları
* `commandToExecute`: (**gerekli**, string) yürütmek için giriş noktası betiği. Bu alan, bunun yerine komutunuz parolalar gibi gizli dizileri içeren ya da kendi fileUris büyük/küçük harfe duyarlıdır kullanın.
  * `fileUris`: (isteğe bağlı, dize dizisi) dosyaların indirilmesi için URL.
  * `timestamp` Bu alan yalnızca bu alanın değerini değiştirerek bir yeniden betiğin tetiklemek için (isteğe bağlı, 32 bit tamsayı) kullanın.  Kabul edilebilir tamsayı değerdeki; yalnızca önceki değerinden farklı olmalıdır.
  * `storageAccountName`: (isteğe bağlı, dize) depolama hesabı adı. Depolama kimlik bilgileri, belirtirseniz, tüm `fileUris` URL'leri, Azure BLOB'ları için olmalıdır.
  * `storageAccountKey`: (isteğe bağlı, dize) depolama hesabı erişim anahtarı

Ortak veya korumalı ayarlarında aşağıdaki değerleri ayarlayabilirsiniz Bu, uzantı hem genel hem de korumalı ayarlarında aşağıdaki değerleri ayarlandığı herhangi bir yapılandırma reddeder.
* `commandToExecute`

Hata ayıklama, ancak için genel ayarları yararlı olabilir kullanarak, korumalı ayarları kullanmanız önerilir.

Genel ayarlar, betik yürütüldüğü VM düz metin olarak gönderilir.  Korumalı ayarları, yalnızca Azure ve VM bildiği bir anahtar kullanılarak şifrelenir. Gönderildiği gibi VM ayarları kaydedildi, ayarları şifrelenmiş diğer bir deyişle, bunlar şifrelenmiş VM üzerinde kaydedilir. Şifrelenmiş değerler şifresini çözmek için kullanılan sertifika, VM üzerinde depolanan ve çalışma zamanında ayarlarını (gerekiyorsa) şifresini çözmek için kullanılan.

## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları Azure Resource Manager şablonları ile dağıtılabilir. Önceki bölümde ayrıntılı JSON şema, bir Azure Resource Manager şablonunda bir Azure Resource Manager şablon dağıtımı sırasında özel betik uzantısı'nı çalıştırmak için kullanılabilir. Aşağıdaki örnekler özel betik uzantısı kullanma işlemini gösterir:

* [Öğretici: Sanal makine uzantıları Azure Resource Manager şablonları ile dağıtma](../../azure-resource-manager/resource-manager-tutorial-deploy-vm-extensions.md)
* [Windows ve Azure SQL DB iki katmanlı uygulama dağıtma](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows)

## <a name="powershell-deployment"></a>PowerShell dağıtım

`Set-AzVMCustomScriptExtension` Komutu, mevcut bir sanal makine için özel betik uzantısı eklemek için kullanılabilir. Daha fazla bilgi için [kümesi AzVMCustomScriptExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmcustomscriptextension).

```powershell
Set-AzVMCustomScriptExtension -ResourceGroupName myResourceGroup `
    -VMName myVM `
    -Location myLocation `
    -FileUri myURL `
    -Run 'myScript.ps1' `
    -Name DemoScriptExtension
```
## <a name="further-examples"></a>Daha fazla örnek

### <a name="using-multiple-script"></a>Birden çok komut dosyası kullanma
Bu örnekte, ilk komut, ardından sahip 'commandToExecute' çağrıları sunucunuzun adlı nasıl diğer seçenekleri oluşturmak için kullanılan üç betik vardır, örneğin, şu hata ile yürütme denetimleri ana bir betik olabilir işleme, günlüğe kaydetme ve durum yönetimi.

```powershell

$fileUri = @("https://xxxxxxx.blob.core.windows.net/buildServer1/1_Add_Tools.ps1",
"https://xxxxxxx.blob.core.windows.net/buildServer1/2_Add_Features.ps1",
"https://xxxxxxx.blob.core.windows.net/buildServer1/3_CompleteInstall.ps1")

$Settings = @{"fileUris" = $fileUri};

$storageaccname = "xxxxxxx"
$storagekey = "1234ABCD"
$ProtectedSettings = @{"storageAccountName" = $storageaccname; "storageAccountKey" = $storagekey; "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File 1_Add_Tools.ps1"};

#run command
Set-AzVMExtension -ResourceGroupName myRG `
    -Location myLocation ` 
    -VMName myVM ` 
    -Name "buildserver1" ` 
    -Publisher "Microsoft.Compute" ` 
    -ExtensionType "CustomScriptExtension" ` 
    -TypeHandlerVersion "1.9" ` 
    -Settings $Settings ` 
    -ProtectedSettings $ProtectedSettings `
```

### <a name="running-scripts-from-a-local-share"></a>Betikleri yerel bir paylaşımından çalıştırma
Bu örnekte, komut dosyası konumunuz için yerel bir SMB sunucusu kullanmak isteyebileceğiniz Not, başka bir programda dışındaki diğer ayarları geçirmek gerekmez *commandToExecute*.

```powershell
$ProtectedSettings = @{"commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File \\filesvr\build\serverUpdate1.ps1"};
 
Set-AzVMExtension -ResourceGroupName myRG 
    -Location myLocation ` 
    -VMName myVM ` 
    -Name "serverUpdate" 
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" ` 
    -TypeHandlerVersion "1.9" `
    -ProtectedSettings $ProtectedSettings

```

### <a name="how-to-run-custom-script-more-than-once-with-cli"></a>Özel betik CLI ile birden çok kez çalıştırmayı öğrenin
Özel betik uzantısı birden çok kez çalıştırmak istiyorsanız, bu koşullar altında bu eylem yalnızca yapabilirsiniz:
1. Uzantı 'Name' parametresi önceki dağıtım uzantısı ile aynıdır.
2. Komut yeniden yürütülmesi olmaz yapılandırma aksi güncelleştirmeniz gerekir. Dinamik bir özellik gibi bir zaman damgası komutu içine ekleyebilirsiniz.

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorun giderme

Uzantı dağıtım durumuyla ilgili veriler, Azure portalından ve Azure PowerShell modülü kullanılarak alınabilir. Belirli bir VM'nin için uzantıları dağıtım durumunu görmek için aşağıdaki komutu çalıştırın:

```powershell
Get-AzVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Uzantı yürütme çıktısı, hedef sanal makinede aşağıdaki dizini altında bulunan dosyaları için günlüğe kaydedilir.
```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

Belirtilen dosyalar, hedef sanal makinedeki aşağıdaki dizine yüklenir.
```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads\<n>
```
Burada `<n>` uzantının çalıştırmaları arasında değişebilir bir ondalık tamsayı.  `1.*` Gerçek, geçerli değerle `typeHandlerVersion` uzantısı değeri.  Örneğin, gerçek dizin olabilir `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.  

Yürütülürken `commandToExecute` komut uzantısı bu dizin ayarlar (örneğin, `...\Downloads\2`) geçerli çalışma dizini. Bu işlem üzerinden indirilen dosyaları bulmak için göreli yolların kullanılması tanır `fileURIs` özelliği. Örnekler için aşağıdaki tabloya bakın.

Zaman içinde mutlak indirme yolunu değişebildiğinden, göreli komut dosyası yolları için iyileştirilmiş en iyisidir `commandToExecute` dize, mümkün olduğunda. Örneğin:
```json
    "commandToExecute": "powershell.exe . . . -File \"./scripts/myscript.ps1\""
```

İlk URI segmenti üzerinden indirilen dosyaları sağlamak için tutulur sonra yol bilgisi `fileUris` özellik listesi.  Aşağıdaki tabloda gösterildiği gibi indirilen dosyaları indirme alt yapısını yansıtmak için eşlenen `fileUris` değerleri.  

#### <a name="examples-of-downloaded-files"></a>İndirilen Dosya örnekleri

| FileUris URI | İndirilen göreli konum | Mutlak konumu indirilen * |
| ---- | ------- |:--- |
| `https://someAcct.blob.core.windows.net/aContainer/scripts/myscript.ps1` | `./scripts/myscript.ps1` |`C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\scripts\myscript.ps1`  |
| `https://someAcct.blob.core.windows.net/aContainer/topLevel.ps1` | `./topLevel.ps1` | `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\topLevel.ps1` |

\* Olarak yukarıdaki mutlak dizin yolları CustomScript uzantısı, tek bir yürütme içinde değil ancak, VM'nin ömrü boyunca değiştirin.

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Destek Al'ı seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği SSS](https://azure.microsoft.com/support/faq/).
