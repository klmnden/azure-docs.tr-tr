---
title: Windows için Azure özel betik uzantısı | Microsoft Docs
description: Özel betik uzantısı kullanarak Windows VM yapılandırma görevleri otomatikleştirme
services: virtual-machines-windows
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: f4181fee-7a9d-4a1c-b517-52956f5b7fa1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/24/2018
ms.author: danis
ms.openlocfilehash: 80f9ecd40c5b9504a6554b95bf374046d8253933
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34809786"
---
# <a name="custom-script-extension-for-windows"></a>Windows için özel betik uzantısı

Özel betik uzantısının indirir ve Azure sanal makinelerde komut dosyaları çalıştırılır. Bu uzantı dağıtım sonrası yapılandırma, yazılım yükleme veya diğer yapılandırma/yönetim görevleri için kullanışlıdır. Betikler Azure depolama veya GitHub konumlarından indirilebilir ya da Azure portalına uzantı çalışma zamanında iletilebilir. Özel Betik uzantısı, Azure Resource Manager şablonları ile tümleşir ve Azure CLI, PowerShell, Azure portalı veya Azure Sanal Makine REST API'si kullanılarak da çalıştırılabilir.

Bu belge Azure PowerShell modülü, Azure Resource Manager şablonları ve sorun giderme adımları Windows sistemlerinde ayrıntıları kullanarak özel betik uzantısı kullanma ayrıntılarını verir.

## <a name="prerequisites"></a>Önkoşullar

> [!NOTE]  
> Özel betik uzantısının kendisini bekleyeceği olduğundan, parametre olarak aynı VM ile güncelleştirme-AzureRmVM çalıştırmak için kullanmayın.  
>   
> 

### <a name="operating-system"></a>İşletim Sistemi

Özel betik uzantısının Linux işletim sistemlerine, daha fazla bilgi için desteklenen uzantısı uzantısı çalışmayacak için bkz: Bu [makale](https://support.microsoft.com/en-us/help/4078134/azure-extension-supported-operating-systems).

### <a name="script-location"></a>Komut dosyası konumu

Azure Blob depolama alanına erişmek için Azure Blob Depolama kimlik bilgilerinizi kullanılacak uzantı kullanabilirsiniz. Alternatif olarak, VM iç dosya sunucusu vb. gibi GitHub, o uç noktasına yönlendirebilir sürece komut dosyası konumu herhangi where, olabilir.


### <a name="internet-connectivity"></a>Internet bağlantısı
GitHub ya da Azure depolama gibi harici olarak bir komut dosyası karşıdan yüklemeniz gerekiyorsa, ek güvenlik duvarı/ağ güvenlik grubu bağlantı noktalarının açılması gerekir. Örneğin, komut dosyası Azure depolama alanında bulunuyorsa, size izin verebilir Azure NSG hizmet etiketleri kullanarak erişim [depolama](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags).

Yerel bir sunucuda komut dosyanızı olduğu sonra ek güvenlik duvarı/ağ güvenlik hala gerekebilir grup bağlantı noktalarının açılması gerekir.

### <a name="tips-and-tricks"></a>İpuçları ve Püf Noktaları
* Bu uzantı için en yüksek hata oranı test hata, komut dosyasını çalıştırır komut dosyasında sözdizimi hataları nedeni ve ayrıca başarısız olduğu bulmayı kolaylaştırmak için komut dosyasına bir günlük daha yerleştirecek.
* Idempotent, olan komut dosyaları yazmak için bunlar yeniden birden fazla kez yanlışlıkla çalıştırırsanız, bu sistem değişiklikleri neden olmaz.
* Çalıştırdıklarında betikleri kullanıcı girişi gerektirmeyen emin olun.
* Betik çalıştırmak izin verilen 90 dakika, başarısız bir uzantı sağlama içinde daha uzun bir şey neden olur.
* Betik içinde yeniden başlatmalar koymayın, bu sorunları yüklenmekte olan diğer uzantılarıyla neden olur ve sonrası yeniden başlatma, uzantısı yeniden başlatma sonrasında devam etmez. 
* Yeniden başlatma neden olacak bir komut dosyası varsa, uygulamaları yüklemek ve betikler vb. çalıştırın. Windows zamanlanmış bir görev veya DSC veya Chef, Puppet uzantıları gibi araçları kullanarak yeniden zamanlamanız gerekir.
* Windows zamanlanmış bir görev oluşturmak için bu uzantıyı kullanan gerek sonra bir komut dosyası her önyükleme üzerinde çalıştırmak istiyorsanız, uzantısı yalnızca bir komut dosyası bir kez çalıştırılır.
* Bir komut dosyası çalışacağı zamanlamak isterseniz, Windows zamanlanmış bir görev oluşturmak için uzantı kullanmanız gerekir. 
* Komut dosyası çalıştırılırken yalnızca Azure portal veya CLI 'geçirme' uzantı durumunu görürsünüz. Çalışan bir komut dosyasının daha sık durum güncelleştirmeleri istiyorsanız, kendi çözüm oluşturmanız gerekir.
* Özel betik uzantısı yerel olarak desteklemez proxy sunucuları, ancak komut dosyanızın içinde proxy sunucuları gibi destekleyen bir dosya aktarımı aracı kullanabilir *Curl* 
* Betikleri veya komutları dayanabileceği varsayılan olmayan directory konumlarını unutmayın, bu durumu çözmek için mantığı vardır.


## <a name="extension-schema"></a>Uzantı şeması

Özel betik uzantısı yapılandırma komut dosyası konumunu ve çalıştırılacak komut gibi belirtir. Bu yapılandırma yapılandırma dosyalarını depolamak, komut satırında belirtin veya bir Azure Resource Manager şablonu belirtin. 

Duyarlı veri şifrelenir ve yalnızca sanal makine içinde şifresi bir korumalı bir yapılandırma depolayabilirsiniz. Korumalı yapılandırma, bir parola gibi gizli yürütme komutu içerir yararlıdır.

Bu öğeler hassas verisi olarak kabul edilir ve uzantıları korumalı ayarı yapılandırmasında belirtilen. Azure VM uzantısının korumalı ayarı veri şifrelenir ve yalnızca hedef sanal makineye şifresi.

```json
{
    "apiVersion": "2015-06-15",
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
            ]
        },
        "protectedSettings": {
            "commandToExecute": "myExecutionCommand",
            "storageAccountName": "myStorageAccountName",
            "storageAccountKey": "myStorageAccountKey"
        }
    }
}
```
**Not** -uzantı yalnızca bir sürümü yüklenebilir bir noktada bir VM'de, zaman içindeki aynı VM başarısız olacağına ilişkin özel komut dosyası iki kere aynı Resource Manager şablonunda belirtme. 

### <a name="property-values"></a>Özellik değerleri

| Ad | Değer / örnek | Veri Türü |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | tarih |
| Yayımcı | Microsoft.Compute | dize |
| type | CustomScriptExtension | dize |
| typeHandlerVersion | 1.9 | Int |
| fileUris (örneğin) | https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1 | array |
| commandToExecute (örneğin) | PowerShell - ExecutionPolicy Unrestricted - dosya yapılandırma-müzik-app.ps1 | dize |
| storageAccountName (örneğin) | examplestorageacct | dize |
| storageAccountKey (örneğin) | TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg== | dize |

>[!NOTE]
>Bu özellik adları büyük/küçük harfe duyarlıdır. Dağıtım sorunlarını önlemek için aşağıda gösterildiği gibi adları kullanın.

#### <a name="property-value-details"></a>Özellik değeri ayrıntıları
 * `commandToExecute`: (**gerekli**, dize) yürütmek için giriş noktası komut dosyası. Bu alan parolalar gibi gizli komutunuzu içeriyorsa veya, fileUris büyük/küçük harfe duyarlıdır kullanın.
* `fileUris`: (isteğe bağlı, dize dizisi) yüklenmek üzere dosyaları için URL'leri.
* `storageAccountName`: (isteğe bağlı, dize) depolama hesabının adı. Depolama kimlik belirtirseniz, tüm `fileUris` URL'ler için Azure BLOB'ları olmalıdır.
* `storageAccountKey`: (isteğe bağlı, dize) depolama hesabının erişim anahtarı

Aşağıdaki değerleri ortak ya da korumalı ayarlarında ayarlanabilir, uzantı hem genel hem de korumalı ayarlarında aşağıdaki değerleri belirlendiği herhangi bir yapılandırma reddeder.
* `commandToExecute`

Hata ayıklama, ancak için genel ayarları belki de yararlı kullanarak korumalı ayarları kullanmanız önerilir.

Genel ayarları burada betik yürütülecek VM düz metin olarak gönderilir.  Korumalı ayarları yalnızca Azure ve VM bildiği bir anahtar kullanılarak şifrelenmiş. Gönderildikleri olarak ayarlar VM kaydedilir, ayarları şifrelediyseniz yani şifrelenmiş VM kaydedilirler. Şifrelenmiş değerler şifresini çözmek için kullanılan sertifika, VM üzerinde depolanır ve çalışma zamanında ayarlarını (gerekiyorsa) şifresini çözmek için kullanılan.

## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları, Azure Resource Manager şablonları ile dağıtılabilir. Önceki bölümde ayrıntılı JSON şeması bir Azure Resource Manager şablonunda bir Azure Resource Manager şablon dağıtımı sırasında özel betik uzantısının çalıştırmak için kullanılabilir. Özel betik uzantısı içeren bir örnek şablonu burada bulunabilir [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="powershell-deployment"></a>PowerShell dağıtım

`Set-AzureRmVMCustomScriptExtension` Komutu, varolan bir sanal makineye özel betik uzantısı eklemek için kullanılabilir. Daha fazla bilgi için bkz: [kümesi AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).
```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName myResourceGroup `
    -VMName myVM `
    -Location myLocation `
    -FileUri myURL `
    -Run 'myScript.ps1' `
    -Name DemoScriptExtension
```
## <a name="further-examples"></a>Daha fazla örnekleri

### <a name="using-multiple-script"></a>Birden çok komut dosyası kullanma
Bu örnekte, ilk komut dosyası sahip 'commandToExecute' çağrıları sunucunuzun adlı nasıl diğer on seçenekleri oluşturmak için kullanılan üç komut dosyalarınız varsa, örneğin, şu hata ile yürütme denetimleri ana bir komut dosyası sahip olabilir işleme, günlük kaydı ve durum yönetimi.

```powershell

$fileUri = @("https://xxxxxxx.blob.core.windows.net/buildServer1/1_Add_Tools.ps1",
"https://xxxxxxx.blob.core.windows.net/buildServer1/2_Add_Features.ps1",
"https://xxxxxxx.blob.core.windows.net/buildServer1/3_CompleteInstall.ps1")

$Settings = @{"fileUris" = $fileUri};

$storageaccname = "xxxxxxx"
$storagekey = "1234ABCD"
$ProtectedSettings = @{"storageAccountName" = $storageaccname; "storageAccountKey" = $storagekey; "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File 1_Add_Tools.ps1"};

#run command
Set-AzureRmVMExtension -ResourceGroupName myRG `
    -Location myLocation ` 
    -VMName myVM ` 
    -Name "buildserver1" ` 
    -Publisher "Microsoft.Compute" ` 
    -ExtensionType "CustomScriptExtension" ` 
    -TypeHandlerVersion "1.9" ` 
    -Settings $Settings ` 
    -ProtectedSettings $ProtectedSettings `
```

### <a name="running-scripts-from-a-local-share"></a>Yerel bir paylaşımdan komut dosyalarını çalıştırma
Bu örnekte, komut dosyası konumu için yerel bir SMB sunucusunu kullanmak üzere isteyebilir Not gerektirmeyen başka bir programda dışındaki diğer ayarları geçişi *commandToExecute*.

```powershell
$ProtectedSettings = @{"commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File \\filesvr\build\serverUpdate1.ps1"};
 
Set-AzureRmVMExtension -ResourceGroupName myRG 
    -Location myLocation ` 
    -VMName myVM ` 
    -Name "serverUpdate" 
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" ` 
    -TypeHandlerVersion "1.9" `
    -ProtectedSettings $ProtectedSettings

```

### <a name="how-to-run-custom-script-more-than-once-with-cli"></a>Özel bir komut dosyası CLI ile birden çok kez çalıştırma
Özel betik uzantısı birden çok kez çalıştırmak istiyorsanız, bunu yalnızca bu koşullar altında yapabilirsiniz:
1. Uzantı 'Name' parametresiyle uzantısı'nın önceki dağıtım ile aynıdır.
2. Gereken güncelleştirilmiş komutu yürütülmedi yeniden olur yapılandırma Aksi takdirde, örneğin, dinamik bir özellik olarak için bir zaman damgası gibi komut ekleyebilirsiniz. 

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorun giderme

Veri uzantısı dağıtımları durumuyla ilgili Azure portalından ve Azure PowerShell modülü kullanılarak alınabilir. İçin belirli bir VM uzantıları dağıtım durumunu görmek için aşağıdaki komutu çalıştırın:

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Uzantı yürütme çıktısını hedef sanal makinede aşağıdaki dizini altında bulunan dosyaları için günlüğe kaydedilir.
```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

Belirtilen dosyalar hedef sanal makinedeki şu dizine yüklenir.
```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads\<n>
```
Burada `<n>` hangi uzantısı yürütmeleri arasında değişebilir ondalık bir tamsayıdır.  `1.*` Değerle gerçek, geçerli `typeHandlerVersion` uzantının değerini.  Örneğin, gerçek dizin olabilir `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.  

Yürütülürken `commandToExecute` komutunu uzantısı ayarlar bu dizin (örneğin, `...\Downloads\2`) geçerli çalışma dizini olarak. Bu aracılığıyla indirilen dosyaları bulmak için göreli yolların kullanımını etkinleştirir `fileURIs` özelliği. Örnekler için aşağıdaki tabloya bakın.

Mutlak indirme yolunu zaman içinde değişebildiğinden göreli komut dosyası yolları için kabul etmek daha iyi `commandToExecute` , mümkün olduğunda dize. Örneğin:
```json
    "commandToExecute": "powershell.exe . . . -File \"./scripts/myscript.ps1\""
```

İlk URI segmenti aracılığıyla karşıdan yüklenen dosyalar için tutulmaktadır sonra yol bilgisi `fileUris` özellik listesi.  Aşağıdaki tabloda gösterildiği gibi karşıdan yüklenen dosyalar indirme alt yapısını yansıtacak şekilde eşlenir `fileUris` değerleri.  

#### <a name="examples-of-downloaded-files"></a>Karşıdan yüklenen dosyalar örnekleri

| FileUris URI | Göreli indirilen konumu | Mutlak konum indirilen * |
| ---- | ------- |:--- |
| `https://someAcct.blob.core.windows.net/aContainer/scripts/myscript.ps1` | `./scripts/myscript.ps1` |`C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\scripts\myscript.ps1`  |
| `https://someAcct.blob.core.windows.net/aContainer/topLevel.ps1` | `./topLevel.ps1` | `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\topLevel.ps1` |

\* Olarak yukarıdaki mutlak dizin yolları VM ancak CustomScript uzantısını tek yürütülmesi içinde değil dağıtımınızın ömrü boyunca değiştirin.

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Get destek seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği ile ilgili SSS](https://azure.microsoft.com/support/faq/).
