---
title: "PowerShell ile kaynak yöneticisi geçirme | Microsoft Docs"
description: "Bu makalede sanal makineleri (VM'ler), sanal ağlar (Vnet'ler) ve depolama hesapları gibi Iaas kaynakların platform desteklenen geçiş Klasikten Azure Resource Manager (ARM) için Azure PowerShell komutlarını kullanarak anlatılmaktadır"
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 2b3dff9b-2e99-4556-acc5-d75ef234af9c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: 01bccb2f8d103faf77b39825a1f9ff663329ed7a
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-powershell"></a>Iaas kaynaklarına Klasikten Azure Resource Manager Azure PowerShell kullanarak geçirme
Bu adımlar Azure PowerShell komutlarının altyapı Klasik dağıtım modeli hizmet (Iaas) kaynaklardan Azure Resource Manager dağıtım modeline olarak geçirmek için nasıl kullanılacağını gösterir.

İsterseniz, ayrıca kaynakları kullanarak geçirebileceğiniz [Azure komut satırı arabirimi (Azure CLI)](../linux/migration-classic-resource-manager-cli.md).

* Desteklenen geçiş senaryoları hakkında daha fazla bilgiler için bkz: [Iaas Klasik kaynaklardan Azure Resource Manager'a Platform desteklenen geçişini](migration-classic-resource-manager-overview.md).
* Ayrıntılı yönergeler ve bir geçiş kılavuz için bkz: [teknik ya ilişkin ayrıntılar platform desteklenen geçiş Klasik'ten için Azure Resource Manager](migration-classic-resource-manager-deep-dive.md).
* [En sık karşılaşılan geçiş hatalarını gözden geçirme](migration-classic-resource-manager-errors.md)

<br>
İşte adımları geçiş işlemi sırasında yürütülmesi gerekir siparişi tanımlamak için bir akış çizelgesi

![Geçiş adımlarını gösteren ekran görüntüsü](media/migration-classic-resource-manager/migration-flow.png)

## <a name="step-1-plan-for-migration"></a>1. adım: geçişini planlama
Geçirme Iaas kaynaklardan Klasik Kaynak Yöneticisi değerlendirirken öneririz birkaç en iyi uygulamalar şunlardır:

* Okuyun [desteklenen ve desteklenmeyen özellikler ve yapılandırmalar](migration-classic-resource-manager-overview.md). Desteklenmeyen yapılandırmaları veya özellikleri kullanan sanal makineler varsa, duyurdu yapılandırma/özellik desteği beklemenizi öneririz. Alternatif olarak, gereksinimlerinize uygun değilse, bu özelliği kaldırmak veya geçiş etkinleştirmek için bu yapılandırma dışında taşıyın.
* Bugün, altyapı ve uygulamalarınızı dağıtma komut otomatik değilse, geçiş için bu komut dosyalarını kullanarak benzer bir test Kurulum oluşturmayı deneyin. Alternatif olarak, Azure portalını kullanarak örnek ortamları ayarlayabilirsiniz.

> [!IMPORTANT]
> Uygulama ağ geçitleri, Klasik geçiş için kaynak yöneticisi için şu anda desteklenmemektedir. Bir uygulama ağ geçidi Klasik sanal ağla geçirmek için ağ taşımak için bir hazırlama işlemi çalıştırmadan önce ağ geçidi kaldırın. Geçişi tamamladıktan sonra Azure Kaynak Yöneticisi'nde ağ geçidi yeniden bağlanın.
>
>ExpressRoute ağ geçidi başka bir Abonelikteki ExpressRoute bağlantı hatları bağlanma otomatik olarak geçirilemez. Böyle durumlarda, ExpressRoute ağ geçidi kaldırın, sanal ağ geçirmek ve ağ geçidi oluşturun. Lütfen bakın [geçirmek ExpressRoute bağlantı hattına ve Resource Manager dağıtım modeli için Klasik sanal ağlar ilişkili](../../expressroute/expressroute-migration-classic-resource-manager.md) daha fazla bilgi için.
>
>

## <a name="step-2-install-the-latest-version-of-azure-powershell"></a>2. adım: Azure PowerShell'in en son sürümünü yükleme
Azure PowerShell'i yüklemek için iki ana seçeneğiniz vardır: [PowerShell Galerisi](https://www.powershellgallery.com/profiles/azure-sdk/) veya [Web Platformu Yükleyicisi (Webpı)](http://aka.ms/webpi-azps). Webpı aylık güncelleştirmeleri alır. PowerShell Galerisi sürekli güncelleştirmeleri alır. Bu makalede, Azure PowerShell sürüm 2.1.0 temel alır.

Yükleme yönergeleri için bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).

<br>

## <a name="step-3-ensure-that-you-are-an-administrator-for-the-subscription-in-azure-portal"></a>3. adım: Azure portalında abonelik için bir yönetici olduğundan emin olun
Bu geçiş işlemini gerçekleştirmek için abonelik için ortak yönetici olarak eklenmelidir [Azure portal](https://portal.azure.com).

1. [Azure portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde seçin **abonelik**. Göremiyorsanız, seçin **daha fazla hizmet**.
3. Uygun abonelik girişini bulun, sonra bakmak **MY ROL** alan. Bir ortak yönetici için değer olmalıdır _Hesap Yöneticisi_.

Bir ortak yönetici eklemek mümkün değilse, bir Hizmet Yöneticisi veya aboneliğin ortak Yöneticisi kendiniz eklenen almak için başvurun.   

## <a name="step-4-set-your-subscription-and-sign-up-for-migration"></a>4. adım: aboneliğinizi ayarlamak ve geçiş için kaydolun
İlk olarak, bir PowerShell istemi başlatın. Geçiş için her iki Klasik ortamınızı ayarlamanız gerekir ve Resource Manager.

Hesabınız Resource Manager modeli için oturum açın.

```powershell
    Login-AzureRmAccount
```

Kullanılabilir abonelikler, aşağıdaki komutu kullanarak alın:

```powershell
    Get-AzureRMSubscription | Sort Name | Select Name
```

Azure aboneliğiniz geçerli oturum için ayarlayın. Bu örnek varsayılan abonelik adını ayarlar **My Azure aboneliği**. Örneğin, abonelik adı kendi ile değiştirin.

```powershell
    Select-AzureRmSubscription –SubscriptionName "My Azure Subscription"
```

> [!NOTE]
> Kayıt tek seferlik bir adımdır, ancak geçişi denemeden önce bir kez yapmanız gerekir. Kayıt olmadan, aşağıdaki hata iletisi görüntülenir:
>
> *BadRequest: Abonelik geçiş için kayıtlı değil.*
>
>

Geçiş kaynak sağlayıcısı ile aşağıdaki komutu kullanarak kaydedin:

```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Kaydın son beş dakika bekleyin. Aşağıdaki komutu kullanarak onay durumunu kontrol edebilirsiniz:

```powershell
    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

RegistrationState olduğundan emin olun `Registered` devam etmeden önce.

Şimdi Klasik model için hesabınızla oturum açın.

```powershell
    Add-AzureAccount
```

Kullanılabilir abonelikler, aşağıdaki komutu kullanarak alın:

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

Azure aboneliğiniz geçerli oturum için ayarlayın. Bu örnek varsayılan abonelik ayarlar **My Azure aboneliği**. Örneğin, abonelik adı kendi ile değiştirin.

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```

<br>

## <a name="step-5-make-sure-you-have-enough-azure-resource-manager-virtual-machine-vcpus-in-the-azure-region-of-your-current-deployment-or-vnet"></a>5. adım: yeterli Azure Resource Manager sanal makine Vcpu'lar geçerli dağıtım veya VNET Azure bölgesinde olduğundan emin olun
Azure Kaynak Yöneticisi'nde sahip Vcpu'lar geçerli sayısını denetlemek için aşağıdaki PowerShell komutunu kullanabilirsiniz. VCPU kotaları hakkında daha fazla bilgi için bkz: [sınırları ve Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager).

Bu örnek kullanılabilirliğini denetler **Batı ABD** bölge. Örneğin, bölge adı kendi ile değiştirin.

```powershell
Get-AzureRmVMUsage -Location "West US"
```

## <a name="step-6-run-commands-to-migrate-your-iaas-resources"></a>6. adım: Iaas kaynaklarınızı geçirmek için komutları çalıştırın
> [!NOTE]
> Burada açıklanan tüm ıdempotent işlemleridir. Desteklenmeyen bir özellik veya bir yapılandırma hatası başka bir sorun varsa, hazırlama yeniden deneme öneririz, veya tamamlanmaya işlemi. Platform sonra eylemi yeniden dener.
>
>

## <a name="step-61-option-1---migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a>6.1. adım: 1. seçenek - sanal makineler bir bulut hizmetinde (değil, sanal ağ için) geçirme
Aşağıdaki komutu kullanarak bulut Hizmetleri listesini almak ve geçirmek istediğiniz bulut hizmeti seçin. Bulut hizmetindeki sanal makineleri bir sanal ağ veya web veya çalışan rolleri varsa, komutu bir hata iletisi döndürür.

```powershell
    Get-AzureService | ft Servicename
```

Bulut hizmeti için dağıtım adı alın. Bu örnekte, hizmet adı; **Hizmetim**. Örneğin, hizmet adı, kendi hizmet adıyla değiştirin.

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

Sanal makine bulut hizmeti geçiş için hazırlayın. Aralarından seçim yapabileceğiniz iki seçeneğiniz vardır.

* **Seçenek 1. Platform tarafından oluşturulan bir sanal ağ sanal makineleri geçirme**

    İlk olarak, aşağıdaki komutları kullanarak bulut hizmeti geçirirseniz doğrulama:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```

    Yukarıdaki komut, tüm uyarıları ve geçiş engelleme hataları görüntüler. Doğrulama başarılı olduğunda sonra taşımak **Prepare** . adım:

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```
* **Seçenek 2. Resource Manager dağıtım modelinde varolan sanal ağını geçirme**

    Bu örnekte kaynak grubu adı ayarlar **myResourceGroup**, sanal ağ adına **myVirtualNetwork** ve alt ağ adı **mySubNet**. Örnek adları kendi kaynakların adları ile değiştirin.

    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```

    İlk olarak, aşağıdaki komutu kullanarak sanal ağ geçirirseniz doğrulama:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```

    Yukarıdaki komut, tüm uyarıları ve geçiş engelleme hataları görüntüler. Doğrulama başarılı olursa, aşağıdaki hazırlama adımıyla devam edebilirsiniz:

    ```powershell
        Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```

Yukarıdaki seçeneklerden birini kullanarak Hazırlama işlemi başarılı olduktan sonra sanal makineleri geçiş durumunu sorgular. İçinde olduklarından emin olmak `Prepared` durumu.

Bu örnek VM adını ayarlar **myVM**. Örnek adı kendi VM adıyla değiştirin.

```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
```

PowerShell veya Azure portalını kullanarak hazırlanmış kaynakların yapılandırmasını denetleyin. Geçiş için hazır olmayan ve eski durumuna geri dönmek istiyorsanız aşağıdaki komutu kullanın:

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

Hazırlanan yapılandırma iyi görünüyorsa, ilerlemek ve aşağıdaki komutu kullanarak kaynakları Yürüt:

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

## <a name="step-61-option-2---migrate-virtual-machines-in-a-virtual-network"></a>6.1. adım: 2. seçenek - bir sanal ağdaki sanal makineleri geçirme

Bir sanal ağdaki sanal makineleri geçirmek için sanal ağ geçirin. Sanal makineler, sanal ağ ile otomatik olarak geçirin. Geçirmek istediğiniz sanal ağ seçin.
> [!NOTE]
> [Tek Klasik sanal makineyi geçirmek](migrate-single-classic-to-resource-manager.md) diskleri yönetilen sanal makine VHD (işletim sistemi ve veri) dosyalarını kullanarak yeni bir kaynak yöneticisi sanal makine oluşturarak.
<br>

> [!NOTE]
> Sanal ağ adı yeni Portalı'nda gösterilen alanından farklı olabilir. Yeni Azure Portal adı olarak görüntüler `[vnet-name]` ancak gerçek sanal ağ adı türünde `Group [resource-group-name] [vnet-name]`. Geçişi olan komutunu kullanarak gerçek sanal ağ adı arama yapmadan önce `Get-AzureVnetSite | Select -Property Name` veya eski Azure Portalı'nda görüntüleyebilirsiniz. 

Bu örnekte sanal ağ adını ayarlar **myVnet**. Örneğin, sanal ağ adı kendinizinkilerle değiştirin.

```powershell
    $vnetName = "myVnet"
```

> [!NOTE]
> Sanal ağ ile desteklenmeyen yapılandırmalar web veya çalışan rolleri ya da sanal makineleri içeriyorsa, bir doğrulama hata iletisi alırsınız.
>
>

Aşağıdaki komutu kullanarak sanal ağ geçirebilirsiniz, ilk olarak, doğrulama:

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

Yukarıdaki komut, tüm uyarıları ve geçiş engelleme hataları görüntüler. Doğrulama başarılı olursa, aşağıdaki hazırlama adımıyla devam edebilirsiniz:

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

Azure PowerShell veya Azure portalını kullanarak hazırlanmış sanal makineler için yapılandırmayı denetleyin. Geçiş için hazır olmayan ve eski durumuna geri dönmek istiyorsanız aşağıdaki komutu kullanın:

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

Hazırlanan yapılandırma iyi görünüyorsa, ilerlemek ve aşağıdaki komutu kullanarak kaynakları Yürüt:

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

## <a name="step-62-migrate-a-storage-account"></a>Adım 6.2 geçirme bir depolama hesabı
İşiniz bittiğinde sanal makineleri geçiriyorsanız, depolama hesapları geçirme öneririz.

Depolama hesabı geçirmeden önce önkoşul denetimleri önceki gerçekleştirin:

* **Klasik sanal makineleri, diskler ve depolama hesabında depolanır**

    Önceki komutu tüm Klasik VM diskleri RoleName ve DiskName özelliklerini depolama hesabında döndürür. RoleName bir disk bağlı olduğu sanal makine adıdır. Komut önceki döndürürse diskleri sonra bu diskler bağlı sanal makineleri depolama hesabı geçirmeden önce geçirildiğinden emin olun.
    ```powershell
     $storageAccountName = 'yourStorageAccountName'
      Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Select-Object -ExpandProperty AttachedTo -Property `
      DiskName | Format-List -Property RoleName, DiskName

    ```
* **Depolama hesabında depolanan eklenmemiş Klasik VM diskleri silme**

    Komutu kullanarak eklenmemiş Klasik VM diskleri depolama hesabı bulun:

    ```powershell
        $storageAccountName = 'yourStorageAccountName'
        Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Where-Object -Property AttachedTo -EQ $null | Format-List -Property DiskName  

    ```
    Komut döndürür diskleri ardından komutu kullanarak bu diskleri silerseniz:

    ```powershell
       Remove-AzureDisk -DiskName 'yourDiskName'
    ```
* **Depolama hesabında depolanan Sil VM görüntüleri**

    Tüm VM görüntüleri, komut önceki depolama hesabında depolanan işletim sistemi diski ile döndürür.
     ```powershell
        Get-AzureVmImage | Where-Object { $_.OSDiskConfiguration.MediaLink -ne $null -and $_.OSDiskConfiguration.MediaLink.Host.Contains($storageAccountName)`
                                } | Select-Object -Property ImageName, ImageLabel
     ```
     Komut önceki depolama hesabında depolanan veri diskleri içeren tüm VM görüntüleri döndürür.
     ```powershell

        Get-AzureVmImage | Where-Object {$_.DataDiskConfigurations -ne $null `
                                         -and ($_.DataDiskConfigurations | Where-Object {$_.MediaLink -ne $null -and $_.MediaLink.Host.Contains($storageAccountName)}).Count -gt 0 `
                                        } | Select-Object -Property ImageName, ImageLabel
     ```
    Önceki komutu kullandıktan komutları tarafından döndürülen tüm VM görüntüleri silin:
    ```powershell
    Remove-AzureVMImage -ImageName 'yourImageName'
    ```

Her Depolama hesabı geçiş için aşağıdaki komutu kullanarak doğrulayın. Bu örnekte, depolama hesabı adı olan **myStorageAccount**. Örnek adı depolama hesabınızın adıyla değiştirin.

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Validate -StorageAccountName $storageAccountName
```

Depolama hesabı geçiş için hazırlamak için sonraki adımdır

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
```

Azure PowerShell veya Azure portalını kullanarak hazırlanmış depolama hesabı için yapılandırmasını denetleyin. Geçiş için hazır olmayan ve eski durumuna geri dönmek istiyorsanız aşağıdaki komutu kullanın:

```powershell
    Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
```

Hazırlanan yapılandırma iyi görünüyorsa, ilerlemek ve aşağıdaki komutu kullanarak kaynakları Yürüt:

```powershell
    Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
```

## <a name="next-steps"></a>Sonraki adımlar
* [Platform desteklenen geçişi Iaas Klasik kaynaklardan Azure Resource Manager'a genel bakış](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Teknik ya ilişkin ayrıntılar platform desteklenen geçiş Klasik'ten Azure Kaynak Yöneticisi](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarının Klasik’ten Azure Resource Manager’a geçişini planlama](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Iaas kaynaklarına Klasikten Azure Resource Manager geçirmek için CLI kullanın](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Iaas Klasik kaynaklardan Azure Resource Manager için geçiş ile Yardım için topluluk araçları](migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [En sık karşılaşılan geçiş hatalarını gözden geçirme](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Gözden geçirme Iaas kaynaklardan Klasik Azure Kaynak Yöneticisi hakkında en sık sorulan sorular](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
