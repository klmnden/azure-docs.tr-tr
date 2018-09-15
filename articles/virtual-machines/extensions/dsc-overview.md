---
title: Desired State Configuration Azure genel bakış
description: Microsoft Azure uzantısı işleyicisi PowerShell Desired State Configuration (DSC için) kullanmayı öğrenin. Makalede Önkoşullar, mimari ve cmdlet'leri içerir.
services: virtual-machines-windows
documentationcenter: ''
author: bobbytreed
manager: carmonm
editor: ''
tags: azure-resource-manager
keywords: DSC
ms.assetid: bbacbc93-1e7b-4611-a3ec-e3320641f9ba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 05/02/2018
ms.author: robreed
ms.openlocfilehash: 18d6478763fd6551cc8baac6ea54e8d91f1a28e6
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2018
ms.locfileid: "45629977"
---
# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Azure Desired State Configuration uzantısı işleyicisi giriş

Microsoft Azure altyapı hizmetleri, Azure VM aracısı ve ilişkili uzantıları parçasıdır. VM uzantıları VM işlevselliğini genişleten ve çeşitli VM yönetimi işlemleri basitleştirmek yazılım bileşenleridir.

Azure Desired State Configuration (DSC) uzantısı için bir VM önyükleme için birincil kullanım örneği [Azure Automation DSC hizmet](../../automation/automation-dsc-overview.md). Bir VM önyükleme sağlar [avantajları](/powershell/dsc/metaconfig#pull-service) VM yapılandırması ve Azure izleme gibi işletimsel diğer araçlarla tümleştirme devam eden Yönetimi içerir.

DSC uzantısı Automation DSC hizmet bağımsız olarak kullanabilirsiniz. Ancak, bu dağıtımı sırasında oluşan bir tekil eylemi içerir. Devam eden raporlama veya yapılandırma yönetimi VM yerel olarak dışında kullanılabilir.

Bu makalede her iki senaryoyu hakkında bilgi sağlar: DSC uzantısı için Otomasyon ekleme ve Azure SDK'sını kullanarak sanal makineleri için yapılandırmaları atamak için bir araç olarak DSC uzantısı kullanarak.

## <a name="prerequisites"></a>Önkoşullar

- **Yerel makine**: Azure VM uzantısı ile etkileşimde bulunmak üzere Azure portalında veya Azure PowerShell SDK'sını kullanmanız gerekir.
- **Konuk Aracısı**: DSC yapılandırması tarafından yapılandırılmış olan Azure VM, Windows Management Framework (WMF) 4.0 veya üzeri destekleyen bir işletim sistemi olmalıdır. Desteklenen işletim sistemi sürümleri tam listesi için bkz. [DSC uzantısı sürüm geçmişi](/powershell/dsc/azuredscexthistory).

## <a name="terms-and-concepts"></a>Terimleri ve kavramları

Bu kılavuz, aşağıdaki kavramları bilindiğini varsayar:

- **Yapılandırma**: bir DSC yapılandırma belgesi.
- **Düğüm**: bir DSC yapılandırması için bir hedef. Bu belgedeki *düğüm* her zaman bir Azure VM'ye ifade eder.
- **Yapılandırma verilerini**: Ortam veri yapılandırması için bir .psd1 dosya.

## <a name="architecture"></a>Mimari

Azure DSC uzantı Azure VM Aracısı framework sunun, yürürlüğe koymasına ve Azure Vm'lerinde çalışan DSC yapılandırmaları raporlamak için kullanır. DSC uzantısı yapılandırma belgesi ve bir dizi parametre kabul eder. Hiçbir dosya sağlanırsa, bir [varsayılan yapılandırma betiğini](#default-configuration-script) uzantısıyla katıştırılır. Yalnızca meta veri kümesi için kullanılan varsayılan yapılandırma betiğini [yerel Configuration Manager](/powershell/dsc/metaconfig).

Uzantının ilk kez çağrıldığında, aşağıdaki mantık kullanarak WMF sürümünü yükler:

- Azure VM işletim sistemi Windows Server 2016 ise, hiçbir işlem yapılmaz. Windows Server 2016 yüklü PowerShell'in en son sürümünü zaten var.
- Varsa **wmfVersion** özelliği belirtilmişse, bu sürüm sanal makinenin işletim sistemi ile uyumlu olmadığı sürece WMF sürümünün yüklü.
- Hayır ise **wmfVersion** özelliği belirtildi, wmf'nin ayrıca ilgili en son sürümü yüklü.

WMF yükleme yeniden başlatılması gerekiyor. Yeniden başlattıktan sonra uzantıyı belirtilen .zip dosyasını indirir **modulesUrl** sağlanırsa, özellik. Bu konum, Azure Blob Depolama alanında ise, bir SAS belirtecini belirtebilirsiniz **sasToken** dosyaya erişmek için özelliği. .zip indirilir ve açılmış sonra yapılandırma işlevi içinde tanımlanan **configurationFunction** .mof dosyası oluşturmak için çalışır. Uzantı ardından çalıştırır `Start-DscConfiguration -Force` oluşturulan .mof dosyasını kullanarak. Uzantı çıktısını yakalar ve Azure durumu kanala yazar.

### <a name="default-configuration-script"></a>Varsayılan yapılandırma betiği

Azure DSC uzantısı olması amaçlanan bir varsayılan yapılandırma betiği içeren kullanılabilir eklemenize Azure Automation DSC hizmetine bir VM. Betik parametreleri yapılandırılabilir özellikleriyle hizalanır [yerel Configuration Manager](/powershell/dsc/metaconfig). Betik parametreleri için bkz. [varsayılan yapılandırma betiğini](dsc-template.md#default-configuration-script) içinde [Desired State Configuration uzantısı Azure Resource Manager şablonları ile](dsc-template.md). Tam betik için bkz [github'da Azure Hızlı Başlangıç şablonu](https://github.com/Azure/azure-quickstart-templates/blob/master/dsc-extension-azure-automation-pullserver/UpdateLCMforAAPull.zip?raw=true).

## <a name="dsc-extension-in-resource-manager-templates"></a>Resource Manager şablonlarında DSC uzantısı

Çoğu senaryoda, Resource Manager dağıtım şablonlarını DSC uzantısı ile çalışmak için beklenen yoludur. Daha fazla bilgi ve Resource Manager dağıtım şablonlarında DSC uzantısı ekleme örnekleri için bkz: [Desired State Configuration uzantısı Azure Resource Manager şablonları ile](dsc-template.md).

## <a name="dsc-extension-powershell-cmdlets"></a>DSC uzantısı PowerShell cmdlet'leri

DSC uzantısı'nı yönetmek için kullanılan PowerShell cmdlet'lerini, etkileşimli sorun giderme ve bilgi toplama senaryolar en iyi şekilde kullanılır. Cmdlet'ler, paketleme, yayımlama ve DSC uzantı dağıtımlarını izlemek için kullanabilirsiniz. DSC uzantısı için cmdlet'leri olmayan henüz çalışmak için güncelleştirilmiş [varsayılan yapılandırma betiğini](#default-configuration-script).

**Yayımla AzureRmVMDscConfiguration** cmdlet'i bir yapılandırma dosyasında alır, bağımlı DSC kaynakları tarar ve ardından bir .zip dosyası oluşturur. .Zip dosyasını, yapılandırma ve yapılandırmayı uygulamak için gereken DSC kaynakları içerir. Cmdlet ayrıca paketin yerel olarak kullanarak oluşturabilirsiniz *- OutputArchivePath* parametresi. Aksi takdirde, cmdlet .zip dosyasını BLOB depolamaya yayımlar ve bir SAS belirteci ile güvenliğini sağlar.

Cmdlet oluşturan .ps1 yapılandırma Arşiv klasörünün kökünde .zip dosyasındaki betiğidir. Modül klasörü kaynakları arşivi klasöre yerleştirilir.

**Kümesi AzureRmVMDscExtension** cmdlet'i bir VM yapılandırma nesnesine PowerShell DSC uzantısı için gerekli ayarları ekler.

**Get-AzureRmVMDscExtension** cmdlet'i belirli bir VM DSC uzantı durumunu alır.

**Get-AzureRmVMDscExtensionStatus** cmdlet'i DSC uzantı işleyici tarafından yapılandırmalara uygun koşullarda çalışıldığından DSC yapılandırma durumunu alır. Bu eylem, bir grup VM'yi veya tek bir VM üzerinde gerçekleştirilebilir.

**Remove-AzureRmVMDscExtension** cmdlet'i uzantısı işleyicinin belirli bir sanal makineden kaldırır. Bu cmdlet mu *değil* yapılandırmasını kaldırmak, WMF kaldırın veya VM üzerinde uygulanan ayarları değiştirin. Uzantı işleyici yalnızca kaldırır. 

Resource Manager DSC uzantı cmdlet'leri hakkında önemli bilgiler:

- Azure Resource Manager cmdlet'leri, zaman uyumlu.
- *ResourceGroupName*, *VMName*, *ArchiveStorageAccountName*, *sürüm*, ve *konumu*gerekli tüm parametreleri.
- *ArchiveResourceGroupName* isteğe bağlı bir parametredir. Sanal Makinenin oluşturulduğu olandan farklı bir kaynak grubu için depolama hesabınıza ait olduğunda, bu parametre belirtebilirsiniz.
- Kullanım **AutoUpdate** uzantısı işleyicisi uygun olduğunda en son sürüme otomatik olarak güncelleştirmek için anahtar. Bu parametre WMF yeni bir sürümü yayımlandığında sanal makinede yeniden neden olma olasılığı vardır.

### <a name="get-started-with-cmdlets"></a>Cmdlet'lerini kullanmaya başlama

Azure DSC uzantı DSC yapılandırma belgelerini doğrudan dağıtım sırasında Azure Vm'leri yapılandırmak için kullanabilirsiniz. Bu adım, otomasyon için düğümü kaydettirin değil. Düğüm *değil* merkezi olarak yönetilen.

Aşağıdaki örnek bir yapılandırma basit bir örneği gösterilmektedir. Yapılandırmayı Iısınstall.ps1 yerel olarak kaydedin.

```powershell
configuration IISInstall
{
    node "localhost"
    {
        WindowsFeature IIS
        {
            Ensure = "Present"
            Name = "Web-Server"
        }
    }
}
```

Aşağıdaki komutları Iısınstall.ps1 betiğin belirtilen VM yerleştirin. Komutlar da yapılandırmasını yürütün ve durumuna geri rapor.

```powershell
$resourceGroup = 'dscVmDemo'
$location = 'westus'
$vmName = 'myVM'
$storageName = 'demostorage'
#Publish the configuration script to user storage
Publish-AzureRmVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set the VM to run the DSC configuration
Set-AzureRmVMDscExtension -Version '2.76' -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName 'iisInstall.ps1.zip' -AutoUpdate $true -ConfigurationName 'IISInstall'
```

## <a name="azure-portal-functionality"></a>Azure portal işlevi

Portalda DSC ayarlamak için:

1. Bir sanal makineye gidin.
2. **Ayarlar** bölümünde **Uzantılar**’ı seçin.
3. Oluşturulan yeni sayfasında seçin **+ Ekle**ve ardından **PowerShell Desired State Configuration**.
4. Tıklayın **Oluştur** uzantı bilgileri sayfanın alt kısmındaki.

Portal şu giriş toplar:

- **Yapılandırma modülleri veya betik**: Bu alan zorunludur (formu için güncelleştirilmemiş [varsayılan yapılandırma betiğini](#default-configuration-script)). Yapılandırma modülleri ve komut dosyalarını bir yapılandırma betiği içeren bir .ps1 dosyası veya kökünde bir .ps1 yapılandırma betiği içeren bir .zip dosyası gerektirir. Bir .zip dosyası kullanıyorsanız, tüm bağımlı kaynaklarla .zip modülü klasörlerdeki eklenmesi gerekir. .Zip dosyasını kullanarak oluşturabileceğiniz **Yayımla AzureVMDscConfiguration - OutputArchivePath** Azure PowerShell SDK'sına dahil cmdlet'i. .Zip dosyası, kullanıcı blob depolama alanına yüklenir ve bir SAS belirteci tarafından güvenli hale getirilmiş.

- **Yapılandırmasını modülü ile nitelenen adı**: birden çok yapılandırma işlevi bir .ps1 dosyasına ekleyebilirsiniz. Ardından yapılandırma .ps1 komut dosyasının adını girin \\ ve yapılandırma işlevin adı. Örneğin, .ps1 komut dosyanızın adı configuration.ps1 ve yapılandırmayı ise **IisInstall**, girin **configuration.ps1\IisInstall**.

- **Yapılandırma değişkenleri**: yapılandırma işlev bağımsız değişkeni alır, bunları burada biçiminde girin **argumentName1 value1, argumentName2 = Value2**. Bu biçim, PowerShell cmdlet'leri veya Resource Manager şablonlarında yapılandırma bağımsız değişkenleri kabul edilir, farklı bir biçimidir.

- **Yapılandırma verileri PSD1 dosyası**: Bu alan isteğe bağlıdır. Yapılandırmanızı .psd1 yapılandırma veri dosyası gerektiriyorsa, veri alanını seçebilir ve kullanıcı blob depolamaya yüklemek için bu alanı kullanın. Yapılandırma verileri dosyası, blob depolama alanındaki bir SAS belirteci tarafından sağlanır.

- **WMF sürümünü**: sanal makinenizde yüklü Windows Management Framework (WMF) sürümünü belirtir. Bu özellik için en son yükler WMF'nin en son sürümünü ayarlama. Şu anda bu özellik için yalnızca olası değerler şunlardır 4.0, 5.0, 5.1 ve son. Bu olası değerler şunlardır: güncelleştirmeleri tabidir. Varsayılan değer **son**.

- **Veri toplama**: uzantı bir telemetri toplarız belirler. Daha fazla bilgi için [Azure DSC uzantı veri koleksiyonu](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/).

- **Sürüm**: yüklemek için DSC uzantı sürümünü belirtir. Sürümleri hakkında daha fazla bilgi için bkz. [DSC uzantısı sürüm geçmişi](/powershell/dsc/azuredscexthistory).

- **Alt sürüm yükseltme otomatik**: Bu alan eşlendiği **AutoUpdate** geçiş cmdlet'leri ve otomatik olarak yükleme sırasında en son sürüme güncelleştirmek uzantı sağlar. **Evet** kullanılabilir en son sürüme kullanılacak uzantısı işleyicisi yenilemelerini ister ve **Hayır** zorlayacak **sürüm** belirtilen yüklenecek. Hiçbirini seçme **Evet** ya da **Hayır** seçerek aynı **Hayır**.

## <a name="logs"></a>Günlükler

Uzantı için günlükleri şu konumda depolanır: `C:\WindowsAzure\Logs\Plugins\Microsoft.Powershell.DSC\<version number>`

## <a name="next-steps"></a>Sonraki adımlar

- PowerShell DSC hakkında daha fazla bilgi için Git [PowerShell Belge Merkezi](/powershell/dsc/overview).
- İnceleme [DSC uzantısı için Resource Manager şablonu](dsc-template.md).
- PowerShell DSC kullanarak yönetebilirsiniz. daha fazla işlevsellik ve daha fazla DSC kaynakları için Gözat [PowerShell Galerisi](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0).
- Yapılandırmalarına önemli parametreleri geçirme hakkında daha fazla ayrıntı için bkz [yönetin, DSC uzantısı işleyicisine ile güvenli kimlik bilgileri](dsc-credentials.md).