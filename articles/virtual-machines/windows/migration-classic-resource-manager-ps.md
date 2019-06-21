---
title: PowerShell ile Resource Manager'a geçirme | Microsoft Docs
description: Bu makale platform destekli geçiş sanal makineleri (VM'ler), sanal ağlar (Vnet'ler) ve depolama hesapları gibi Iaas kaynaklarının Klasik modelden Azure Resource Manager (ARM) için Azure PowerShell komutlarını kullanarak size
services: virtual-machines-windows
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 2b3dff9b-2e99-4556-acc5-d75ef234af9c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: 7cc8970e739d2e762fb08e563ef0498948ac8251
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "64692884"
---
# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-powershell"></a>Azure PowerShell'i kullanarak Iaas kaynaklarını Klasik modelden Azure Resource Manager'a geçiş
Bu adımlar altyapı olarak hizmet (Iaas) kaynaklarını Klasik dağıtım modelinden Azure Resource Manager dağıtım modeline geçirmek için Azure PowerShell komutlarını kullanmayı gösterir.

Dilerseniz de kaynakları kullanarak geçirebileceğiniz [Azure komut satırı arabirimi (Azure CLI)](../linux/migration-classic-resource-manager-cli.md).

* Desteklenen geçiş senaryoları hakkında arka plan bilgileri için bkz. [Iaas kaynaklarının Klasik modelden Azure Resource Manager'a Platform destekli geçişe](migration-classic-resource-manager-overview.md).
* Ayrıntılı yönergeler ve bir geçiş kılavuzu için bkz. [teknik platform destekli geçişe derinlemesine Klasik modelden Azure Resource Manager'a](migration-classic-resource-manager-deep-dive.md).
* [En sık karşılaşılan geçiş hatalarını gözden geçirme](migration-classic-resource-manager-errors.md)

<br>
İşte adımları bir geçiş işlemi sırasında yürütülmek üzere gereken sırayı tanımlamak için bir akış çizelgesi

![Geçiş adımlarını gösteren ekran görüntüsü](media/migration-classic-resource-manager/migration-flow.png)

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="step-1-plan-for-migration"></a>1\. adım: Geçiş planlaması
Resource Manager'a geçirme Iaas kaynaklarını Klasik portaldan değerlendirirken öneririz birkaç en iyi uygulamalar şunlardır:

* Okumak [desteklenen ve desteklenmeyen özellikleri ve yapılandırmalar](migration-classic-resource-manager-overview.md). Desteklenmeyen yapılandırmalar veya özellikleri kullanan sanal makineler varsa, Duyurulacak yapılandırma/özellik desteği için beklemenizi öneririz. Alternatif olarak, gereksinimlerinize uyan, bu özelliği kaldırın veya geçişi etkinleştirmek için bu yapılandırma dışında taşıyın.
* Altyapınızı ve uygulamalarınızı dağıtmak, bugün komut otomatik değilse, geçiş için bu komut dosyalarını kullanarak benzer bir test ayarı oluşturmayı deneyin. Alternatif olarak, Azure portalını kullanarak örnek ortamlarını ayarlayabilirsiniz.

> [!IMPORTANT]
> Uygulama ağ geçitleri, Resource Manager'a geçiş Klasik modelden için şu anda desteklenmemektedir. Bir uygulama ağ geçidi ile bir Klasik sanal ağ'ı geçirmek için ağ taşımak için bir hazırlama işlemi çalıştırmadan önce ağ geçidini kaldırın. Azure Resource Manager'daki ağ geçidi geçişi tamamlandıktan sonra yeniden bağlanın.
>
>ExpressRoute ağ geçitleri başka bir Abonelikteki ExpressRoute devrelerine bağlama otomatik olarak geçirilemez. Böyle durumlarda, ExpressRoute ağ geçidini kaldırmak, sanal ağı geçirme ve ağ geçidini yeniden oluşturun. Lütfen [geçirme ExpressRoute devrelerini ve ilişkili sanal ağları Klasikten Resource Manager dağıtım modeline](../../expressroute/expressroute-migration-classic-resource-manager.md) daha fazla bilgi için.

## <a name="step-2-install-the-latest-version-of-azure-powershell"></a>2\. adım: Azure PowerShell'in en son sürümünü yükleyin
Azure PowerShell'i yüklemek için iki ana seçeneğiniz vardır: [PowerShell Galerisi](https://www.powershellgallery.com/profiles/azure-sdk/) veya [Web Platformu Yükleyicisi (Webpı)](https://aka.ms/webpi-azps). Webpı aylık güncelleştirmeler alır. PowerShell Galerisi güncelleştirmeleri sürekli olarak alır. Bu makalede, Azure PowerShell sürümü 2.1.0 üzerinde temel alır.

Yükleme yönergeleri için bkz. [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview).

<br>

## <a name="step-3-ensure-that-you-are-an-administrator-for-the-subscription-in-azure-portal"></a>3\. adım: Azure portalında Abonelik Yöneticisi olduğundan emin olun
Bu geçiş gerçekleştirmek için abonelik için ortak yönetici olarak eklenmelidir [Azure portalında](https://portal.azure.com).

1. [Azure portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde **abonelik**. Görmüyorsanız, seçin **tüm hizmetleri**.
3. Uygun aboneliği girişini bulun ve ardından bakmak **SİTEM ROLÜ** alan. Bir ortak yönetici için değer olmalıdır _Hesap Yöneticisi_.

Ortak yönetici eklemek mümkün değilse, bir Hizmet Yöneticisi veya aboneliğin ortak yönetici eklenen kendiniz almak için iletişime geçin.   

## <a name="step-4-set-your-subscription-and-sign-up-for-migration"></a>4\. Adım: Geçiş için kaydolun ve aboneliğinizi ayarlama
İlk olarak bir PowerShell komut istemini başlatın. Geçiş için Klasik ortamınızı kurmanız gerekir ve Resource Manager.

Resource Manager modeli için hesabınızda oturum açın.

```powershell
    Connect-AzAccount
```

Kullanılabilir abonelikler, aşağıdaki komutu kullanarak alın:

```powershell
    Get-AzSubscription | Sort Name | Select Name
```

Azure aboneliğiniz için geçerli oturumu ayarlayın. Bu örnekte varsayılan abonelik adını ayarlar **Azure Aboneliğim**. Örnek abonelik adı kendi değerlerinizle değiştirin.

```powershell
    Select-AzSubscription –SubscriptionName "My Azure Subscription"
```

> [!NOTE]
> Kayıt tek seferlik bir adım, ancak geçişi denemeden önce bir kez yapmanız gerekir. Kayıt olmadan, aşağıdaki hata iletisini görürsünüz:
>
> *BadRequest: Abonelik geçiş için kayıtlı değil.*

Geçiş kaynak sağlayıcısı ile aşağıdaki komutu kullanarak kaydedin:

```powershell
    Register-AzResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Lütfen kaydı tamamlamak beş dakika bekleyin. Aşağıdaki komutu kullanarak onay durumunu kontrol edebilirsiniz:

```powershell
    Get-AzResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

RegistrationState olduğundan emin olun `Registered` devam etmeden önce.

Artık Klasik model için hesabınızda oturum açın.

```powershell
    Add-AzureAccount
```

Kullanılabilir abonelikler, aşağıdaki komutu kullanarak alın:

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

Azure aboneliğiniz için geçerli oturumu ayarlayın. Bu örnekte varsayılan aboneliği ayarlar **Azure Aboneliğim**. Örnek abonelik adı kendi değerlerinizle değiştirin.

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```

<br>

## <a name="step-5-make-sure-you-have-enough-azure-resource-manager-virtual-machine-vcpus-in-the-azure-region-of-your-current-deployment-or-vnet"></a>5\. Adım: Azure Resource Manager sanal makinesine yeterli Vcpu geçerli dağıtım veya VNET Azure bölgesinde olduğundan emin olun
Azure Resource Manager'da sahip Vcpu geçerli sayısını denetlemek için aşağıdaki PowerShell komutunu kullanabilirsiniz. VCPU kotaları hakkında daha fazla bilgi için bkz: [sınırlarını ve Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-azure-resource-manager).

Bu örnekte kullanılabilirlik iade **Batı ABD** bölge. Örneğin bölge adı kendi değerlerinizle değiştirin.

```powershell
Get-AzVMUsage -Location "West US"
```

## <a name="step-6-run-commands-to-migrate-your-iaas-resources"></a>6\. Adım: Iaas kaynakları geçirmek için komutlar çalıştırın
* [Bir bulut hizmetinde (değil, sanal ağ) Vm'leri geçirme](#step-61-option-1---migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network)
* [Bir sanal ağda sanal makineleri geçirme](#step-61-option-2---migrate-virtual-machines-in-a-virtual-network)
* [Depolama hesabını geçirin](#step-62-migrate-a-storage-account)

> [!NOTE]
> Burada açıklanan tüm işlemler bir kere etkili olur. Desteklenmeyen bir özellik veya bir yapılandırma hatası dışında bir sorun varsa, hazırlama, yeniden deneme öneririz durdurma veya işlemeyi işlemi. Platform sonra eylemi yeniden dener.


### <a name="step-61-option-1---migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a>6\.1. adım: 1. seçenek - (değil, sanal ağ için) bir bulut hizmetindeki sanal makinelerin geçişini gerçekleştirin
Aşağıdaki komutu kullanarak bulut Hizmetleri'nın listesini alın ve ardından geçirmek istediğiniz bulut hizmeti seçin. Bulut hizmetindeki sanal makinelerin sanal ağ içinde yoksa veya bunlar web veya çalışan rolü varsa, komutu bir hata iletisi döndürür.

```powershell
    Get-AzureService | ft Servicename
```

Bulut hizmeti için dağıtım adını alın. Bu örnekte, hizmet adıdır **Hizmetim**. Örnek hizmet adı, kendi hizmet adıyla değiştirin.

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

Geçiş için bulut hizmetindeki sanal makine hazırlayın. Aralarından seçim yapabileceğiniz iki seçeneğiniz vardır.

* **1. seçenek. Platform tarafından oluşturulan bir sanal ağa sanal makineleri geçirme**

    Aşağıdaki komutlar kullanılarak bulut hizmetine geçirebilirsiniz, ilk olarak, doğrulama:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```

    Aşağıdaki komut, tüm uyarıları ve geçiş engelleyecek hataları görüntüler. Doğrulama başarılı olur ve ardından oturum taşıyabilirsiniz, **hazırlama** . adım:

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```
* **2. seçenek. Resource Manager dağıtım modelinde var olan bir sanal ağa geçirme**

    Bu örnekte kaynak grubu adını ayarlar **myResourceGroup**, sanal ağ adına **myVirtualNetwork** ve alt ağ adı için **mySubNet**. Örnek adları kendi kaynaklarınızın adlarıyla değiştirin.

    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```

    İlk olarak, aşağıdaki komutu kullanarak sanal ağa geçirirseniz doğrulama:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```

    Aşağıdaki komut, tüm uyarıları ve geçiş engelleyecek hataları görüntüler. Doğrulama başarılı olursa, aşağıdaki hazırlama adımı ile devam edebilirsiniz:

    ```powershell
        Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```

Yukarıdaki seçeneklerden birini kullanarak Hazırlama işlemi başarılı olduktan sonra sanal makinelerin geçiş durumu sorgulayın. İçinde bulundukları olun `Prepared` durumu.

Bu örnekte VM adını ayarlar **myVM**. Örnek adı, kendi VM adıyla değiştirin.

```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
```

PowerShell veya Azure portalını kullanarak hazırlanmış kaynaklar için yapılandırmasını denetleyin. Geçiş için hazır değildir ve eski durumuna geri dönmek istiyorsanız aşağıdaki komutu kullanın:

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

Hazırlanan yapılandırma iyi görünüyorsa, ileriye taşıyın ve aşağıdaki komutu kullanarak kaynakları işleyin:

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

### <a name="step-61-option-2---migrate-virtual-machines-in-a-virtual-network"></a>6\.1. adım: Seçenek 2 - bir sanal ağdaki sanal makinelerin geçişini gerçekleştirin

Bir sanal ağdaki sanal makineleri geçirmek için sanal ağını geçirin. Sanal makineler, sanal ağ ile otomatik olarak geçirin. Geçirmek istediğiniz sanal ağı seçin.
> [!NOTE]
> [Tek Klasik sanal makine geçirme](migrate-single-classic-to-resource-manager.md) diskler yönetilen sanal makine VHD (işletim sistemi ve veri) dosyalarını kullanarak yeni bir Resource Manager sanal makine oluşturarak.
<br>

> [!NOTE]
> Sanal ağ adı, yeni portalda gösterilen öğesinden farklı olabilir. Yeni Azure portalı adı olarak görüntüler `[vnet-name]` ancak gerçek sanal ağ adı türü `Group [resource-group-name] [vnet-name]`. Geçişini, arama komutunu kullanarak gerçek sanal ağ adı önce `Get-AzureVnetSite | Select -Property Name` veya eski Azure Portalı'nda görüntüleyebilirsiniz. 

Bu örnekte sanal ağ adı ayarlar **myVnet**. Örnek sanal ağ adı kendi değerlerinizle değiştirin.

```powershell
    $vnetName = "myVnet"
```

> [!NOTE]
> Sanal ağ ile desteklenmeyen yapılandırmalar web veya çalışan rolleri veya sanal makineleri içeriyorsa, bir doğrulama hata iletisi alırsınız.

Aşağıdaki komutu kullanarak sanal ağın geçişini yapabilirsiniz, ilk olarak, doğrulama:

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

Aşağıdaki komut, tüm uyarıları ve geçiş engelleyecek hataları görüntüler. Doğrulama başarılı olursa, aşağıdaki hazırlama adımı ile devam edebilirsiniz:

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

Azure PowerShell veya Azure portalını kullanarak hazırlanmış sanal makineler için yapılandırmayı denetleyin. Geçiş için hazır değildir ve eski durumuna geri dönmek istiyorsanız aşağıdaki komutu kullanın:

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

Hazırlanan yapılandırma iyi görünüyorsa, ileriye taşıyın ve aşağıdaki komutu kullanarak kaynakları işleyin:

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

### <a name="step-62-migrate-a-storage-account"></a>Bir depolama hesabı adım 6.2 geçirme
İşiniz bittiğinde sanal makineleri geçiriyorsanız, aşağıdaki önkoşul denetimlerini depolama hesaplarını geçirmeden önce gerçekleştirmenizi öneririz.

> [!NOTE]
> Depolama hesabınızı hiçbir ilişkili diskler veya sanal makine verilerini varsa, doğrudan atlayabilirsiniz **depolama hesabı doğrulamak ve başlangıç geçiş** bölümü.

* **Herhangi bir VM geçişi veya Disk kaynaklarını depolama hesabınızın sahip önkoşul denetimleri**
    * **Klasik sanal makineleri olan diskler depolama hesabında depolanır.**

        Aşağıdaki komutu kullanarak depolama hesabı Klasik VM disklerini RoleName ve DiskName özelliklerini döndürür. RoleName bir disk bağlı olduğu sanal makinenin adıdır. Bu komut döndürürse diskleri ardından bu diskleri bağlı sanal makineleri depolama hesabı geçiş yapmadan önce geçirildiğinden emin olun.
        ```powershell
         $storageAccountName = 'yourStorageAccountName'
          Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Select-Object -ExpandProperty AttachedTo -Property `
          DiskName | Format-List -Property RoleName, DiskName

        ```
    * **Depolama hesabında depolanan eklenmemiş Klasik VM disk Sil**

        Eklenmemiş Klasik VM disk depodaki aşağıdaki komutu kullanarak hesap bulun:

        ```powershell
            $storageAccountName = 'yourStorageAccountName'
            Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Where-Object -Property AttachedTo -EQ $null | Format-List -Property DiskName  

        ```
        Komut döndürür diskleri ardından aşağıdaki komutu kullanarak bu diskleri silmeniz halinde:

        ```powershell
           Remove-AzureDisk -DiskName 'yourDiskName'
        ```
    * **Depolama hesabında depolanan delete VM görüntüleri**

        Aşağıdaki komutu, işletim sistemi diski depolama hesabında depolanan tüm VM görüntülerini döndürür.
         ```powershell
            Get-AzureVmImage | Where-Object { $_.OSDiskConfiguration.MediaLink -ne $null -and $_.OSDiskConfiguration.MediaLink.Host.Contains($storageAccountName)`
                                    } | Select-Object -Property ImageName, ImageLabel
         ```
         Aşağıdaki komut, depolama hesabında depolanan veri diskleri ile tüm VM görüntülerini döndürür.
         ```powershell

            Get-AzureVmImage | Where-Object {$_.DataDiskConfigurations -ne $null `
                                             -and ($_.DataDiskConfigurations | Where-Object {$_.MediaLink -ne $null -and $_.MediaLink.Host.Contains($storageAccountName)}).Count -gt 0 `
                                            } | Select-Object -Property ImageName, ImageLabel
         ```
        Bu komutu kullanarak komutları tarafından döndürülen tüm VM görüntülerini silin:
        ```powershell
        Remove-AzureVMImage -ImageName 'yourImageName'
        ```
* **Doğrulama depolama hesabı ve geçiş Başlat**

    Her Depolama hesabı geçiş için aşağıdaki komutu kullanarak doğrulayın. Bu örnekte, depolama hesabının adıdır **myStorageAccount**. Örnek adı, depolama hesabınızın adıyla değiştirin.

    ```powershell
        $storageAccountName = "myStorageAccount"
        Move-AzureStorageAccount -Validate -StorageAccountName $storageAccountName
    ```

    Depolama hesabı geçiş için hazırlamak üzere sonraki adımdır

    ```powershell
        $storageAccountName = "myStorageAccount"
        Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
    ```

    Azure PowerShell veya Azure portalını kullanarak hazırlanmış bir depolama hesabı için yapılandırmasını denetleyin. Geçiş için hazır değildir ve eski durumuna geri dönmek istiyorsanız aşağıdaki komutu kullanın:

    ```powershell
        Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
    ```

    Hazırlanan yapılandırma iyi görünüyorsa, ileriye taşıyın ve aşağıdaki komutu kullanarak kaynakları işleyin:

    ```powershell
        Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
    ```

## <a name="next-steps"></a>Sonraki adımlar
* [Iaas kaynaklarının Klasik modelden Azure Resource Manager'a platform destekli geçişe genel bakış](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Klasik modelden Azure Resource Manager’a platform destekli geçişe ayrıntılı teknik bakış](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarının Klasik’ten Azure Resource Manager’a geçişini planlama](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Iaas kaynaklarının Klasik'ten Azure Resource Manager'a geçiş için CLI kullanma](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Iaas kaynaklarını Klasik modelden Azure Resource Manager'a geçişini ile Yardım için topluluk araçları](migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [En sık karşılaşılan geçiş hatalarını gözden geçirme](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Gözden geçirme Iaas kaynaklarını Klasik modelden Azure Resource Manager'a hakkında sık sorulan sorular](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
