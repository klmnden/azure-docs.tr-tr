---
title: "İstenen durum yapılandırması Azure genel bakış | Microsoft Docs"
description: "PowerShell istenen durum yapılandırması için Microsoft Azure uzantısı işleyici kullanarak genel bakış. Önkoşullar, mimari, cmdlet'leri dahil olmak üzere..."
services: virtual-machines-windows
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: bbacbc93-1e7b-4611-a3ec-e3320641f9ba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 01/09/2017
ms.author: zachal
ms.openlocfilehash: deb360e36b68f7ddb13b00946c700d0c83890ca6
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Azure istenen durum yapılandırması uzantısı işleyici giriş
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Azure VM aracısı ve ilişkili uzantıları Microsoft Azure altyapı hizmetleri parçasıdır. VM uzantıları, VM işlevselliğini genişleten ve çeşitli VM yönetim işlemlerini basitleştirmek yazılım bileşenleridir. Örneğin, bir yönetici parolasını sıfırlamak için VMAccess uzantısını kullanılabilir veya özel betik uzantısı VM'de bir betik yürütmek için kullanılabilir.

Bu makalede, Azure PowerShell SDK'ın bir parçası olarak Azure VM'ler için PowerShell istenen durum yapılandırması (DSC) uzantısı tanıtılır. Karşıya yükleme ve PowerShell DSC yapılandırması PowerShell DSC uzantısı ile etkin bir Azure VM uygulamak için yeni cmdlet'lerini kullanabilirsiniz. Alınan DSC yapılandırma VM'de yürürlüğe için PowerShell DSC uzantısı çağrılarının içine PowerShell DSC. Bu işlevsellik, ayrıca Azure portalı üzerinden kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar
**Yerel makine** Azure VM uzantısı ile etkileşim kurmak için Azure portalında veya Azure PowerShell SDK'sını kullanmanız gerekir. 

**Konuk Aracısı** DSC yapılandırması tarafından yapılandırılan Azure VM Windows Management Framework (WMF) 4.0 veya 5.0 destekleyen bir işletim sistemi olması gerekir. Desteklenen işletim sistemi sürümleri tam listesini bulabilirsiniz [DSC uzantısı sürüm geçmişi](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).

## <a name="terms-and-concepts"></a>Terimleri ve kavramları
Bu kılavuz aşağıdaki kavramlar bilindiğini varsayar:

* **Yapılandırma** -A DSC yapılandırma belgesi. 
* **Düğüm** -DSC yapılandırması için hedef. Bu belgede "düğümü" her zaman bir Azure VM ifade eder.
* **Yapılandırma verilerini** - ortam yapılandırma verilerini içeren bir .psd1 dosyası

## <a name="architectural-overview"></a>Mimari genel bakış
Azure DSC uzantısı Azure VM Aracısı framework teslim etmek, yürürlüğe ve DSC yapılandırmaları Azure Vm'lerinde çalıştırılan raporlamak için kullanır. DSC uzantı en az bir yapılandırma belge ve Azure PowerShell SDK veya Azure Portalı aracılığıyla sağlanan parametreleri kümesini içeren bir .zip dosyası bekler.

Uzantı ilk kez çağrıldığında bir yükleme işlemi çalıştırır. Bu işlem aşağıdaki mantık kullanılarak Windows Management Framework (WMF), bir sürümünü yükler:

1. Azure VM işletim sistemi Windows Server 2016 ise, hiçbir işlem yapılmadı. Windows Server 2016 yüklü PowerShell'in en son sürümünü zaten var.
2. Varsa `wmfVersion` özelliği belirtildi, VM'nin işletim sistemiyle uyumlu olmadığı sürece WMF'ın bu sürümü yüklü.
3. Öyle değilse `wmfVersion` özelliği belirtildi, WMF ' son geçerli sürümü yüklü.

WMF yeniden başlatma gerektirir. Yeniden başlatıldıktan sonra belirtilen .zip dosya uzantısı indirmeleri `modulesUrl` özelliği. Bu konum Azure blob depolama alanına ise, bir SAS belirteci belirtilebilir `sasToken` dosyaya erişmek için özellik. Yapılandırma işlevi .zip indirilir ve açılmış sonra tanımlanan `configurationFunction` MOF dosyası oluşturmak için çalıştırın. Uzantısı sonra çalışır `Start-DscConfiguration -Force` oluşturulan MOF dosyası üzerinde. Uzantı, çıkış ve yazma işlemleri geri Azure durum kanala yakalar. Bu noktadan itibaren DSC LCM'yi izleme ve düzeltme normal olarak işler. 

## <a name="powershell-cmdlets"></a>PowerShell cmdlet'leri
PowerShell cmdlet'leri, Azure Resource Manager veya Klasik dağıtım modeli ile paketini, yayımlama ve DSC uzantısı dağıtımlarını izlemek için kullanılabilir. Listelenen aşağıdaki cmdlet'leri için Klasik dağıtım modülleri olsa da, "Azure", "Azure Resource Manager modelini kullanmak için AzureRm" ile değiştirilebilir. Örneğin, `Publish-AzureVMDscConfiguration` Klasik dağıtım modelini kullanan nerede `Publish-AzureRmVMDscConfiguration` Azure Kaynak Yöneticisi'ni kullanır. 

`Publish-AzureVMDscConfiguration`bir yapılandırma dosyasında alır, bağımlı DSC kaynakları için tarar ve yapılandırma ve yapılandırmasını yürürlüğe için gereken DSC kaynakları içeren bir .zip dosyası oluşturur. Yerel olarak kullanarak paketi oluşturabilirsiniz `-ConfigurationArchivePath` parametresi. Aksi takdirde, .zip dosyasını Azure blob depolama alanına yayımlar ve bir SAS belirteci ile güvenliğini sağlar.

Bu cmdlet ile oluşturulan .zip dosyası arşiv klasörün kökünde .ps1 yapılandırma komut dosyası vardır. Kaynaklar arşiv klasöre yerleştirilen modülün klasörü sahiptir. 

`Set-AzureVMDscExtension`bir VM yapılandırması nesnesine PowerShell DSC uzantısı tarafından gerekli ayarları yerleştirir. Klasik dağıtım modelinde bir Azure VM ile VM değişikliklerin uygulanması gereken `Update-AzureVM`. 

`Get-AzureVMDscExtension`belirli bir VM DSC uzantı durumunu alır. 

`Get-AzureVMDscExtensionStatus`DSC uzantı işleyici tarafından kamulaştırılmış DSC yapılandırması durumunu alır. Bu eylem, bir tek VM veya grup Vm'lere üzerinde gerçekleştirilebilir.

`Remove-AzureVMDscExtension`Uzantı işleyicinin belirli bir sanal makineden kaldırır. Bu cmdlet mu **değil** yapılandırmasını kaldırmak, WMF kaldırın veya sanal makinedeki uygulanan ayarları değiştirebilirsiniz. Yalnızca uzantısı işleyici de kaldırır. 

**ASM ve Azure Resource Manager cmdlet'lerini temel farklılıkları**

* Azure Resource Manager cmdlet'lerini zaman uyumlu. ASM cmdlet'leri zaman uyumsuzdur.
* ResourceGroupName, VMName, ArchiveStorageAccountName, sürüm ve konum tüm gerekli Azure Kaynak Yöneticisi'nde parametreleridir.
* ArchiveResourceGroupName yeni bir isteğe bağlı parametre için Azure Resource Manager ' dir. Sanal makinenin oluşturulduğu olandan farklı bir kaynak grubu için depolama hesabınıza ait olduğunda, bu parametre belirtebilirsiniz.
* Azure Kaynak Yöneticisi'nde ArchiveBlobName ConfigurationArchive çağrılır
* Kapsayıcı adı Azure Kaynak Yöneticisi'nde ArchiveContainerName çağrılır
* Azure Kaynak Yöneticisi'nde ArchiveStorageEndpointSuffix StorageEndpointSuffix çağrılır
* Otomatik güncelleştirme anahtarı otomatik en son sürüme ve kullanılabilir olduğunda uzantısı işleyicisini güncelleştirme etkinleştirmek için Azure Resource Manager için eklendi. WMF yeni bir sürümü yayımlandığında, bu parametre neden olma olasılığı olan Not VM yeniden başlatır. 

## <a name="azure-portal-functionality"></a>Azure portal işlevi
Bir VM öğesine göz atın. Ayarlar altında "Uzantıları." Genel tıklatın -> Yeni bir bölme oluşturulur. "Ekle"'yi tıklatın ve PowerShell DSC seçin.

Portal giriş gerekir.
**Yapılandırma modülleri veya komut dosyası**: Bu alan zorunludur. Bir yapılandırma betiğini içeren bir .ps1 dosyası ya da bir .ps1 yapılandırma komut dosyası kök ve .zip içinde modülü klasörlerdeki tüm bağımlı kaynaklarla olan bir .zip dosyası gerektirir. İle oluşturulabilir `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` Azure PowerShell SDK'da bulunan cmdlet'i. .Zip dosyasını tarafından bir SAS belirteci güvenli, kullanıcı blob depolama alanına yüklenir. 

**Yapılandırma verileri PSD1 dosyası**: Bu alan isteğe bağlıdır. Yapılandırmanızı .psd1 yapılandırma veri dosyası gerektiriyorsa, bu alanı seçin ve kullanıcı blob depolama alanına yüklemek için burada tarafından bir SAS belirteci güvenli kullanın. 

**Yapılandırma, modül adı**: .ps1 dosyaları birden çok yapılandırma işlevleri sahip olabilir. Ardından yapılandırma .ps1 komut dosyasının adını girin bir '\' ve yapılandırma işlevin adı. Örneğin, .ps1 komut adı "configuration.ps1" sahiptir ve yapılandırma "IisInstall" ise, girersiniz:`configuration.ps1\IisInstall`

**Yapılandırma değişkenleri**: yapılandırma işlevi bağımsız değişken alıyorsa, bunları burada biçimde girin `argumentName1=value1,argumentName2=value2`. Bu biçim PowerShell cmdlet'lerini veya Resource Manager şablonları ile yapılandırma bağımsız değişkenleri nasıl kabul daha farklı bir biçim olduğuna dikkat edin. 

## <a name="getting-started"></a>Başlarken
Azure DSC uzantısı DSC yapılandırma belgelerde alır ve Azure Vm'lerinde enacts. Bir yapılandırma basit bir örnek aşağıda verilmiştir. Yerel olarak, "IisInstall.ps1" kaydedin:

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
###<a name="classic-model"></a>Klasik modeli
```powershell
#Azure PowerShell cmdlets are required
Import-Module Azure

#Use an existing Azure Virtual Machine, 'DscDemo1'
$demoVM = Get-AzureVM DscDemo1

#Publish the configuration script into user storage.
Publish-AzureVMDscConfiguration -ConfigurationPath ".\IisInstall.ps1" -StorageContext $storageContext -Verbose -Force

#Set the VM to run the DSC configuration
Set-AzureVMDscExtension -VM $demoVM -ConfigurationArchive "IisInstall.ps1.zip" -StorageContext $storageContext -ConfigurationName "IisInstall" -Verbose

#Update the configuration of an Azure Virtual Machine
$demoVM | Update-AzureVM -Verbose

#check on status
Get-AzureVMDscExtensionStatus -VM $demovm -Verbose
```
###<a name="azure-resource-manager-model"></a>Azure Resource Manager modeli

```powershell
$resourceGroup = "dscVmDemo"
$location = "westus"
$vmName = "myVM"
$storageName = "demostorage"
#Publish the configuration script into user storage
Publish-AzureRmVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set the VM to run the DSC configuration
Set-AzureRmVmDscExtension -Version 2.21 -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName iisInstall.ps1.zip -AutoUpdate:$true -ConfigurationName "IISInstall"

```

## <a name="logging"></a>Günlüğe kaydetme
Günlükleri yerleştirilir:

```
C:\WindowsAzure\Logs\Plugins\Microsoft.Powershell.DSC\[Version Number]
```

## <a name="next-steps"></a>Sonraki adımlar
PowerShell DSC hakkında daha fazla bilgi için [PowerShell Belge Merkezi ziyaret](https://msdn.microsoft.com/powershell/dsc/overview). 

İncelemek [DSC uzantısı için Azure Resource Manager şablonu](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

PowerShell DSC ile yönetebileceğiniz ek işlevsellik bulmak için [PowerShell Galerisi Gözat](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) daha fazla DSC kaynakları için.

Yapılandırmaları hassas parametreleri geçirme hakkında daha fazla bilgi için bkz: [yönetmek DSC uzantısı işleyici ile güvenli kimlik bilgileri](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

