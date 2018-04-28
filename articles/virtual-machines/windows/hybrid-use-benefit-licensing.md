---
title: Windows Server için Azure karma avantajı | Microsoft Docs
description: Azure için şirket içi lisansları getirmek için Windows Yazılım Güvencesi Avantajlarınızı en üst düzeye öğrenin
services: virtual-machines-windows
documentationcenter: ''
author: kmouss
manager: jeconnoc
editor: ''
ms.assetid: 332583b6-15a3-4efb-80c3-9082587828b0
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 11/22/2017
ms.author: kmouss
ms.openlocfilehash: bfad3ff71be07026cf4a7dd6ad75df399349f844
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="azure-hybrid-benefit-for-windows-server"></a>Windows Server için Azure Hibrit Teklifi
Yazılım Güvencesi ile müşteriler için Windows Server için Azure karma avantajı, şirket içi Windows Server lisanslarınızı kullanın ve Windows sanal makineleri Azure üzerinde sınırlı bir ücret çalıştırmak olanak sağlar. Windows Server için Azure karma avantajı herhangi yeni sanal makineleri dağıtmak için kullanabileceğiniz Azure desteklenen platform Windows Server görüntüsü veya Windows özel görüntüler. Bu makalede Windows Server için Azure karma avantajı olan yeni VM'ler dağıtma ve nasıl varolan güncelleştirebilirsiniz adımları üzerinden VM'ler çalıştıran gider. Windows Server için Azure karma Avantajı hakkında daha fazla bilgi için bkz: Lisans ve maliyet tasarrufu [Windows Server için Azure karma avantajı lisans sayfası](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

> [!IMPORTANT]
> Kurumsal Anlaşma Azure Market'te sahip müşteriler için yayımlanan eski '[HUB]' Windows Server görüntülerinin, 11/9/2017 sürümünden itibaren kullanımdan, standart Windows Server için Azure karma avantajı Portal'daki "Para Kaydet" seçeneğiyle kullanın Windows Server. Daha fazla bilgi için lütfen bu başvurun [makalesi.](https://support.microsoft.com/en-us/help/4036360/retirement-azure-hybrid-use-benefit-images-for-ea-subscriptions)
>

> [!NOTE]
> SQL Server ya da herhangi bir üçüncü taraf Market görüntülerini gibi ek yazılım için ücretlendirilirsiniz VM ile birlikte Windows Server için Azure karma avantajı kullanarak alınmakta. 409 hata gibi alırsanız: 'LicenseType değeri' izin verilmiyor; özelliğini değiştirme ardından Dönüştür veya yeni bir Windows Server, bu bölgede desteklenmiyor olabilir maliyeti, ek yazılım sahip VM dağıtmak çalışıyorsunuz. Aramak çalışırsanız aynı için portal yapılandırma seçeneği dönüştürme yapmak için bunu o VM için göremez.
>


> [!NOTE]
> Klasik VM'ler için yalnızca şirket içi özel resimler yeni VM'den dağıtma desteklenir. Bu makalede desteklenen özelliklerinden yararlanmak için Klasik sanal makineleri için Resource Manager modeli geçirmeniz gerekir.
>


## <a name="ways-to-use-azure-hybrid-benefit-for-windows-server"></a>Windows Server için Azure karma avantajı kullanmanın yolları
Azure karma avantajı ile Windows sanal makineleri kullanmak için birkaç yolu vardır:

1. Sağlanan birinden VM'ler dağıtabilirsiniz [Azure Marketi Windows Server görüntülerinde](#https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.WindowsServer?tab=Overview)
2. Yapabilecekleriniz [özel bir VM karşıya](#upload-a-windows-vhd) ve [Resource Manager şablonunu kullanarak dağıtın](#deploy-a-vm-via-resource-manager) veya [Azure PowerShell](#detailed-powershell-deployment-walkthrough)
3. Geçiş ve Azure karma avantajı ile çalışan arasında var olan VM dönüştürebilir veya Windows Server için isteğe bağlı maliyet ödeme
4. Ayrıca, Windows Server için Azure karma avantajı ile ayarlamak yeni bir sanal makine ölçek dağıtabilirsiniz

> [!NOTE]
> Windows Server için Azure karma avantajı kullanmak üzere ayarlanmış var olan bir sanal makine ölçek dönüştürme desteklenmiyor
>

## <a name="deploy-a-vm-from-a-windows-server-marketplace-image"></a>Bir Windows Server Market görüntüsünden bir VM'yi dağıtmak
Azure Marketi'nde kullanılabilir tüm Windows Server görüntülerini, Windows Server için Azure karma avantajı ile etkinleştirilir. Örneğin, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ve Windows Server 2008SP1 ve daha fazlası. Bu görüntüler, doğrudan Azure portalı, Resource Manager şablonları, Azure PowerShell veya diğer SDK sanal makineleri dağıtmak için kullanabilirsiniz.

Bu görüntüleri doğrudan Azure portalından dağıtabilirsiniz. Resource Manager şablonları ve Azure PowerShell ile kullanmak için aşağıdaki gibi görüntüleri listesini görüntüleyin:

### <a name="powershell"></a>PowerShell
```powershell
Get-AzureRmVMImagesku -Location westus -PublisherName MicrosoftWindowsServer -Offer WindowsServer
```
Adımlarını izleyin [PowerShell ile Windows sanal makine oluşturma](#https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-powershell?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json) ve LicenseType değeri geçirin = "Windows_Server". Bu seçenek, Azure üzerinde var olan Windows Server lisansınızı kullanmanıza olanak sağlar.

### <a name="portal"></a>Portal
Adımlarını izleyin [Azure portalı ile Windows sanal makine oluşturma](#https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal) ve var olan Windows Server lisansınızı kullanma seçeneğini seçin.

## <a name="convert-an-existing-vm-using-azure-hybrid-benefit-for-windows-server"></a>Windows Server için Azure karma avantajı kullanarak mevcut bir VM'yi Dönüştür
Windows Server için Azure karma avantajı yararlanmak için dönüştürmek istediğiniz mevcut bir VM'yi varsa, VM lisans türü aşağıdaki gibi güncelleştirebilirsiniz:

### <a name="convert-to-using-azure-hybrid-benefit-for-windows-server"></a>Windows Server için Azure karma avantajı kullanmaya Dönüştür
```powershell
$vm = Get-AzureRmVM -ResourceGroup "rg-name" -Name "vm-name"
$vm.LicenseType = "Windows_Server"
Update-AzureRmVM -ResourceGroupName rg-name -VM $vm
```

### <a name="convert-back-to-pay-as-you-go"></a>İşlemlere devam ederken geri ödeme Dönüştür
```powershell
$vm = Get-AzureRmVM -ResourceGroup "rg-name" -Name "vm-name"
$vm.LicenseType = "None"
Update-AzureRmVM -ResourceGroupName rg-name -VM $vm
```

### <a name="portal"></a>Portal
VM dikey portaldan Azure karma fayda "Yapılandırma" seçeneğini belirleyerek kullanmak için VM güncelleştirin ve "Azure karma fayda" seçeneğini Değiştir

> [!NOTE]
> "Yapılandırma" altında "Azure karma avantajı" Değiştir seçeneğini görmüyorsanız, dönüştürme için seçilen VM türü henüz desteklenmiyor, çünkü (örneğin bir VM özel görüntü veya SQL Server gibi ek Ücretli yazılım içeren bir görüntü yerleşik veya Azure Market üçüncü taraf yazılım).
>

## <a name="upload-a-windows-server-vhd"></a>Windows Server VHD karşıya yükle
Windows Server VM azure'da dağıtmak için önce temel Windows yapınızın içeren bir VHD oluşturmanız gerekir. Azure için karşıya yüklemeden önce bu VHD Sysprep uygun şekilde hazırlanması gerekir. Yapabilecekleriniz [Sysprep işlemi ve VHD gereksinimleri hakkında daha fazla](upload-generalized-managed.md) ve [sunucu rolleri için Sysprep desteği](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles). VM yedeklemesi, Sysprep çalıştırılmadan önce yedekleyin. 

VHD hazırlandıktan sonra VHD gibi Azure Storage hesabınıza yüklemek:

```powershell
Add-AzureRmVhd -ResourceGroupName "myResourceGroup" -LocalFilePath "C:\Path\To\myvhd.vhd" `
    -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd"
```

> [!NOTE]
> Microsoft SQL Server, SharePoint Server ve Dynamics Yazılım Güvencesi lisans de kullanabilir. Uygulama bileşenlerinizin yükleme ve lisans anahtarları uygun şekilde sağlayarak, sonra disk görüntüsü Azure'a yüklenmesini tarafından Windows Server görüntüsünü hazırlamak gereken. Sysprep uygulamanızla birlikte gibi çalıştırmak için uygun belgelerini gözden geçirin [yükleme Sysprep kullanarak SQL Server için ilgili önemli noktalar](https://msdn.microsoft.com/library/ee210754.aspx) veya [SharePoint Server 2016 başvuru yansıması (Sysprep)Oluştur](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx).
>
>

Ayrıca daha fazla hakkında bilgiyi [VHD için Azure işlem karşıya yükleme](upload-generalized-managed.md#upload-the-vhd-to-your-storage-account)

## <a name="deploy-a-vm-via-resource-manager-template"></a>Resource Manager şablonu aracılığıyla bir VM'yi dağıtmak
Resource Manager şablonlarınızı, ek bir parametre içinde `licenseType` belirtilmesi gerekir. Daha fazla bilgi edinebilirsiniz [Azure Resource Manager şablonları yazma](../../resource-group-authoring-templates.md). Azure'a karşıya, VHD olduktan sonra lisans türü işlem sağlayıcısı bir parçası olarak dahil edin ve normal olarak, şablonunuzu dağıtmak için Resource Manager şablonunu düzenleyin:

```json
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

## <a name="deploy-a-vm-via-powershell-quickstart"></a>PowerShell quickstart aracılığıyla bir VM'yi dağıtmak
Windows Server VM'nize PowerShell aracılığıyla dağıtırken, ek bir parametre olan `-LicenseType`. Azure için karşıya, VHD'yi oluşturduktan sonra kullanarak bir VM oluşturun `New-AzureRmVM` ve lisans türü aşağıdaki gibi belirtin:

Windows Server için:
```powershell
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Server"
```

Daha açıklayıcı bir kılavuzu için farklı adımlar okuyabilirsiniz [Resource Manager ve PowerShell kullanarak bir Windows VM oluşturma](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>VM lisans avantajı kullanılarak doğrulayın
PowerShell, Resource Manager şablonu veya portal aracılığıyla, VM dağıttıktan sonra lisans türü ile doğrulayabilirsiniz `Get-AzureRmVM` gibi:

```powershell
Get-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

Windows Server için aşağıdaki örneğe benzer bir çıkış:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

Bu çelişir çıktı Windows Server için Azure karma avantajı lisans olmadan dağıtılan aşağıdaki VM ile:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

## <a name="list-all-azure-hybrid-benefit-for-windows-server-vms-in-a-subscription"></a>Tüm Azure karma avantajı için Windows Server Vm'lerinin bir abonelik listesi

Windows Server için Azure karma avantajı ile dağıtılan tüm sanal makinelerin sayısı ve görmek için aboneliğinizden aşağıdaki komutu çalıştırabilirsiniz:

```powershell
$vms = Get-AzureRMVM 
foreach ($vm in $vms) {"VM Name: " + $vm.Name, "   Azure Hybrid Benefit for Windows Server: "+ $vm.LicenseType}
```

## <a name="deploy-a-virtual-machine-scale-set-with-azure-hybrid-benefit-for-windows-server"></a>Windows Server için Azure karma avantajı ile ayarlanmış bir sanal makine ölçek dağıtma
İçinde sanal makine ölçek kümesi Resource Manager şablonları, ek bir parametre `licenseType` belirtilmesi gerekir. Daha fazla bilgi edinebilirsiniz [Azure Resource Manager şablonları yazma](../../resource-group-authoring-templates.md). Ölçek kümesinin virtualMachineProfile bir parçası olarak LicenseType değeri özellik içerir ve normal olarak şablonunuzu dağıtmak-2016 Windows sunucu görüntüsü kullanarak örnek aşağıdaki bakın Kaynak Yöneticisi şablonunuzu düzenleyin:


```json
"virtualMachineProfile": {
    "storageProfile": {
        "osDisk": {
            "createOption": "FromImage"
        },
        "imageReference": {
            "publisher": "MicrosoftWindowsServer",
            "offer": "WindowsServer",
            "sku": "2016-Datacenter",
            "version": "latest"
        }
    },
    "licenseType": "Windows_Server",
    "osProfile": {
            "computerNamePrefix": "[parameters('vmssName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
    }
```
Ayrıca [oluşturma ve bir sanal makine ölçek kümesi dağıtma](#https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-create) ve LicenseType değeri özelliğini ayarlayın

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [Azure karma avantajı paradan tasarruf etme](https://azure.microsoft.com/pricing/hybrid-use-benefit/)

Daha fazla bilgi edinmek [ayrıntılı kılavuz lisans Windows Server için Azure karma avantajı](https://docs.microsoft.com/windows-server/get-started/azure-hybrid-benefit)

Daha fazla bilgi edinmek [kullanarak Resource Manager şablonları](../../azure-resource-manager/resource-group-overview.md)

Daha fazla bilgi edinmek [Windows Server için Azure karma avantajı Azure Site Recovery yapıp Azure uygulamaları geçirme daha uygun maliyetli](https://azure.microsoft.com/blog/hybrid-use-benefit-migration-with-asr/)

Daha fazla bilgi edinmek [çok kullanıcılı barındırma hakkına sahip Azure üzerinde Windows 10](https://docs.microsoft.com/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment)

Daha fazla bilgi edinin [sık sorulan sorular](#https://azure.microsoft.com/pricing/hybrid-use-benefit/faq/)
