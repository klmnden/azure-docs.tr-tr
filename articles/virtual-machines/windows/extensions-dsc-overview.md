---
title: "İstenen durum yapılandırması Azure genel bakış | Microsoft Docs"
description: "PowerShell istenen durum yapılandırması için Microsoft Azure uzantısı işleyici kullanarak genel bakış. Önkoşullar, mimari, cmdlet'leri dahil olmak üzere..."
services: virtual-machines-windows
documentationcenter: 
author: mgreenegit
manager: timlt
editor: 
tags: azure-resource-manager
keywords: dsc
ms.assetid: bbacbc93-1e7b-4611-a3ec-e3320641f9ba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 02/02/2018
ms.author: migreene
ms.openlocfilehash: ed8b5bc54baa3a5abfb596b202f0af58e1b6c74f
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Azure istenen durum yapılandırması uzantısı işleyici giriş

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Azure VM aracısı ve ilişkili uzantıları Microsoft Azure altyapı hizmetleri parçasıdır.
VM uzantıları, VM işlevselliğini genişleten ve çeşitli VM yönetim işlemlerini basitleştirmek yazılım bileşenleridir.

İstenen durum yapılandırması uzantısı için bir VM bootstrap için birincil kullanım örneği [Azure Otomasyonu DSC hizmet](../../automation/automation-dsc-overview.md) sağlayan [avantajları](https://docs.microsoft.com/en-us/powershell/dsc/metaconfig.md#pull-service) VM yapılandırmasının devam eden yönetimi dahil ve Azure Monitoring gibi işletimsel diğer araçları ile tümleştirme.

Ancak, bu dağıtımı sırasında oluşan, tekil bir eylemdir ve devam eden raporlama veya yerel olarak VM içinden dışında yapılandırmasının yönetim bağımsız olarak Azure Otomasyonu DSC hizmetinden DSC uzantısı kullanmak da mümkündür.

Bu makalede senaryoları, DSC uzantısı için Azure Otomasyonu ekleme ve DSC uzantısı Azure SDK'sını kullanarak sanal makineleri için yapılandırmaları atamak için bir araç olarak kullanmak üzere nasıl ilgili bilgiler içerir.

## <a name="prerequisites"></a>Önkoşullar

- **Yerel makine** - Azure VM uzantısı ile etkileşim kurmak için Azure portalında veya Azure PowerShell SDK'sını kullanmanız gerekir.
- **Konuk Aracısı** -DSC yapılandırması tarafından yapılandırılan Azure VM Windows Management Framework (WMF) 4.0 veya üstünü destekleyen bir işletim sistemi olması gerekir. Desteklenen işletim sistemi sürümlerinin tam listesi sayfasında bulunabilir [DSC uzantısı sürüm geçmişi](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).

## <a name="terms-and-concepts"></a>Terimleri ve kavramları

Bu kılavuz aşağıdaki kavramlar bilindiğini varsayar:

- **Yapılandırma** -A DSC yapılandırma belgesi.
- **Düğüm** -DSC yapılandırması için hedef. Bu belgede "düğümü" her zaman bir Azure VM ifade eder.
- **Yapılandırma verilerini** - ortam yapılandırma verilerini içeren bir .psd1 dosyası.

## <a name="architectural-overview"></a>Mimari genel bakış

Azure DSC uzantısı Azure VM Aracısı framework teslim etmek, yürürlüğe ve DSC yapılandırmaları Azure Vm'lerinde çalıştırılan raporlamak için kullanır.
DSC uzantısı yapılandırma belge ve parametreleri kümesini kabul eder.
Hiçbir dosya sağlanırsa, bir [varsayılan yapılandırma komut dosyası](###default-configuration-script) uzantıya sahip olan yalnızca meta veri ayarlamak için kullanılan katıştırılmış [yerel Configuration Manager](https://docs.microsoft.com/en-us/powershell/dsc/metaconfig).

Uzantı ilk kez çağrıldığında, aşağıdaki mantık kullanılarak Windows Management Framework (WMF), bir sürümünü yükler:

1. Azure VM işletim sistemi Windows Server 2016 ise, hiçbir işlem yapılmadı. Windows Server 2016 yüklü PowerShell'in en son sürümünü zaten var.
2. Varsa `wmfVersion` özelliği belirtildi, VM'nin işletim sistemiyle uyumlu olmadığı sürece WMF'ın bu sürümü yüklü.
3. Öyle değilse `wmfVersion` özelliği belirtildi, WMF ' son geçerli sürümü yüklü.

WMF yeniden başlatma gerektirir.
Yeniden başlatıldıktan sonra belirtilen .zip dosya uzantısı indirmeleri `modulesUrl` sağladıysanız özelliği.
Bu konum Azure blob depolama alanına ise, bir SAS belirteci belirtilebilir `sasToken` dosyaya erişmek için özellik.
Yapılandırma işlevi .zip indirilir ve açılmış sonra tanımlanan `configurationFunction` bir MOF dosyası oluşturmak için çalıştırın.
Uzantısı sonra çalışır `Start-DscConfiguration -Force` oluşturulan MOF dosyası kullanarak.
Uzantı çıkış yakalar ve Azure durum kanala yazar.

### <a name="default-configuration-script"></a>Varsayılan yapılandırma betiği

Varsayılan yapılandırma Azure DSC uzantısı içerir zaman kullanılmak üzere komut dosyası ekleme Azure Otomasyonu DSC hizmetine bir VM.
Betik parametrelerini yapılandırılabilir özellikleriyle hizalanır [yerel Configuration Manager](https://docs.microsoft.com/en-us/powershell/dsc/metaconfig).
Komut parametreleri [belgelenen](extensions-dsc-template.md##default-configuration-script) ve tam komut dosyası kullanılabilir [GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/dsc-extension-azure-automation-pullserver/UpdateLCMforAAPull.zip?raw=true).

## <a name="dsc-extension-in-arm-templates"></a>DSC uzantı ARM şablonları

Azure Resource Manager (ARM) dağıtım şablonları, çoğu senaryo için DSC uzantısı'yla çalışmak için beklenen bir yöntemdir.
Bilgi ve ARM dağıtım şablonlarında DSC uzantısı dahil örnekleri görmek için ayrılmış belge sayfasının [istenen durum yapılandırması uzantısı Azure Resource Manager şablonları ile](extensions-dsc-template.md).

## <a name="dsc-extension-powershell-cmdlets"></a>DSC uzantı PowerShell cmdlet'leri

DSC uzantı yönetmek için PowerShell cmdlet'leri, en iyi etkileşimli sorun giderme ve bilgi toplama senaryoları için kullanılır.
Cmdlet'ler, paketi, yayımlama ve DSC uzantısı dağıtımlarını izlemek için kullanılabilir.
DSC uzantısı için cmdlet'leri henüz çalışmak için güncelleştirilmediği gerektiğini lütfen unutmayın [varsayılan yapılandırma komut dosyası](###default-configuration-script).

`Publish-AzureRMVMDscConfiguration`bir yapılandırma dosyasında alır, bağımlı DSC kaynakları için tarar ve yapılandırma ve yapılandırmasını yürürlüğe için gereken DSC kaynakları içeren bir .zip dosyası oluşturur.
Yerel olarak kullanarak paketi oluşturabilirsiniz `-ConfigurationArchivePath` parametresi.
Aksi takdirde, .zip dosyasını Azure blob depolama alanına yayımlar ve bir SAS belirteci ile güvenliğini sağlar.

Bu cmdlet ile oluşturulan .zip dosyası arşiv klasörün kökünde .ps1 yapılandırma komut dosyası vardır.
Kaynaklar arşiv klasöre yerleştirilen modülün klasörü sahiptir.

`Set-AzureRMVMDscExtension`bir VM yapılandırması nesnesine PowerShell DSC uzantısı tarafından gerekli ayarları yerleştirir.

`Get-AzureRMVMDscExtension`belirli bir VM DSC uzantı durumunu alır.

`Get-AzureRMVMDscExtensionStatus`DSC uzantı işleyici tarafından kamulaştırılmış DSC yapılandırması durumunu alır.
Bu eylem, bir tek VM veya grup Vm'lere üzerinde gerçekleştirilebilir.

`Remove-AzureRMVMDscExtension`Uzantı işleyicinin belirli bir sanal makineden kaldırır.
Bu cmdlet mu **değil** yapılandırmasını kaldırmak, WMF kaldırın veya sanal makinedeki uygulanan ayarları değiştirebilirsiniz.
Yalnızca uzantısı işleyici de kaldırır. 

AzureRM DSC uzantısı cmdlet'leri ilgili önemli bilgiler:

- Azure Resource Manager cmdlet'lerini zaman uyumlu.
- ResourceGroupName, VMName, ArchiveStorageAccountName, Version, and Location are all required parameters.
- ArchiveResourceGroupName isteğe bağlı bir parametredir. Sanal makinenin oluşturulduğu olandan farklı bir kaynak grubu için depolama hesabınıza ait olduğunda, bu parametre belirtebilirsiniz.
- Otomatik güncelleştirme anahtarı otomatik en son sürüme ve kullanılabilir olduğunda uzantısı işleyicisini güncelleştirme sağlar. WMF yeni bir sürümü yayımlandığında, bu parametre neden olma olasılığı olan Not VM yeniden başlatır.

### <a name="getting-started-with-cmdlets"></a>Cmdlet ile çalışmaya başlama

Azure DSC düğümü olacak şekilde bu Azure Otomasyonu düğüme kaydedilmiyor olsa da Azure Vm'lerde dağıtımı sırasında doğrudan yapılandırmak için DSC yapılandırma belgelerini kullanma yeteneği uzantısıdır **değil*-merkezi olarak yönetilebilir.

Bir yapılandırma basit bir örnek aşağıda verilmiştir.
Yerel olarak, "IisInstall.ps1" kaydedin:

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

Aşağıdaki adımları IisInstall.ps1 komut dosyası üzerinde belirtilen sanal Makineyi yerleştirmek, yapılandırma yürütün ve durumuna geri rapor.

```powershell
$resourceGroup = "dscVmDemo"
$location = "westus"
$vmName = "myVM"
$storageName = "demostorage"
#Publish the configuration script into user storage
Publish-AzureRmVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set the VM to run the DSC configuration
Set-AzureRmVmDscExtension -Version 2.72 -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName iisInstall.ps1.zip -AutoUpdate:$true -ConfigurationName "IISInstall"
```

## <a name="azure-portal-functionality"></a>Azure Portal işlevi

Bir VM öğesine göz atın. Ayarlar altında "Uzantıları." Genel tıklatın ->
Yeni bir bölme oluşturulur.
"Ekle"'yi tıklatın ve PowerShell DSC seçin.

Portal giriş gerekir.
**Yapılandırma modülleri veya komut dosyası**: Bu alan zorunludur (form için güncelleştirilmemiş [varsayılan yapılandırma komut dosyası](###default-configuration-script).
Bir yapılandırma betiğini içeren bir .ps1 dosyası ya da bir .ps1 yapılandırma komut dosyası kök ve .zip içinde modülü klasörlerdeki tüm bağımlı kaynaklarla olan bir .zip dosyası gerektirir.
İle oluşturulabilir `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` Azure PowerShell SDK'da bulunan cmdlet'i.
.Zip dosyasını tarafından bir SAS belirteci güvenli, kullanıcı blob depolama alanına yüklenir.

**Yapılandırma verileri PSD1 dosyası**: Bu alan isteğe bağlıdır.
Yapılandırmanızı .psd1 yapılandırma veri dosyası gerektiriyorsa, bu alanı seçin ve kullanıcı blob depolama alanına yüklemek için burada tarafından bir SAS belirteci güvenli kullanın.

**Yapılandırma, modül adı**: .ps1 dosyaları birden çok yapılandırma işlevleri sahip olabilir.
Ardından yapılandırma .ps1 komut dosyasının adını girin bir '\' ve yapılandırma işlevin adı.
Örneğin, .ps1 komut adı "configuration.ps1" sahiptir ve yapılandırma "IisInstall" ise, girersiniz:`configuration.ps1\IisInstall`

**Yapılandırma değişkenleri**: yapılandırma işlevi bağımsız değişken alıyorsa, bunları burada biçimde girin `argumentName1=value1,argumentName2=value2`.
Bu biçim PowerShell cmdlet'lerini veya Resource Manager şablonları ile yapılandırma bağımsız değişkenleri nasıl kabul daha farklı bir biçim olduğuna dikkat edin.

## <a name="logging"></a>Günlüğe kaydetme
Günlükleri yerleştirilir:

```powerShell
C:\WindowsAzure\Logs\Plugins\Microsoft.Powershell.DSC\[Version Number]
```

## <a name="next-steps"></a>Sonraki adımlar
PowerShell DSC hakkında daha fazla bilgi için [PowerShell Belge Merkezi ziyaret](https://msdn.microsoft.com/powershell/dsc/overview). 

İncelemek [DSC uzantısı için Azure Resource Manager şablonu](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

PowerShell DSC ile yönetebileceğiniz ek işlevsellik bulmak için [PowerShell Galerisi Gözat](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) daha fazla DSC kaynakları için.

Yapılandırmaları hassas parametreleri geçirme hakkında daha fazla bilgi için bkz: [yönetmek DSC uzantısı işleyici ile güvenli kimlik bilgileri](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
