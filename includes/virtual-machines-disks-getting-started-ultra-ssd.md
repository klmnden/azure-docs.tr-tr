---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 05/10/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 326382339e2b4aeaa488d3d7f76b7ff35f9bc620
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66147775"
---
# <a name="enable-and-deploy-azure-ultra-ssds-preview"></a>Etkinleştirme ve Azure ultra SSD (Önizleme) dağıtma

Azure Iaas sanal makinelerini (VM) için tutarlı düşük gecikme süreli disk depolama (SSD) (Önizleme) teklif yüksek aktarım hızı ve yüksek IOPS Azure ultra katı hal sürücüleri. Bu yeni teklif, var olan diskleri tekliflerimizi aynı kullanılabilirlik düzeylerinde satırı performansının üst sağlar. Ultra yüksek SSD önemli avantajlarından biri, Vm'leri yeniden başlatma gerekmeden iş yüklerinizi birlikte SSD performansını dinamik olarak değiştirmenize olanak yeteneğidir. Ultra yüksek SSD'ler, SAP HANA, en çok katmanı veritabanları ve işlem yoğunluklu iş yükleri gibi veri kullanımı yoğun iş yükleri için uygundur.

Şu anda, ultra Ssd'leri olan Önizleme aşamasındadır ve gerekir [kaydetme](https://aka.ms/UltraSSDPreviewSignUp) erişebilmeleri için önizlemede.

## <a name="determine-your-availability-zone"></a>Kullanılabilirlik alanı belirleme

Onaylandıktan sonra ultra SSD kullanmak için içinde bulunduğunuz hangi kullanılabilirlik alanı'nı belirlemeniz gerekir. Hangi Doğu ultra diskinize dağıtmak için ABD 2 bölgesinde belirlemek için aşağıdaki komutlardan birini çalıştırın:

PowerShell: `Get-AzComputeResourceSku | where {$_.ResourceType -eq "disks" -and $_.Name -eq "UltraSSD_LRS" }`

CLI: `az vm list-skus --resource-type disks --query "[?name=='UltraSSD_LRS'].locationInfo"`

Yanıt X olduğu bölge Doğu ABD 2 bölgesinde kullanmak için aşağıdaki formu, benzer olacaktır. X 1, 2 veya 3 olabilir.

Koruma **bölgeleri** değeri, kullanılabilirlik alanı temsil ettiği ve Ultra yüksek bir SSD dağıtmak için gerekir.

|KaynakTürü  |Ad  |Location  |Bölgeler  |Kısıtlama  |Özellik  |Değer  |
|---------|---------|---------|---------|---------|---------|---------|
|diskler     |UltraSSD_LRS         |eastus2         |X         |         |         |         |

Komutundan yanıt yoktu sonra hala geçerli özelliğine kaydınızı Beklemede veya henüz onaylanmadı.

Dağıtmak için hangi bölgeyi artık bildiğinize göre bu makalede, ilk Vm'lerinizi ultra SSD ile dağıtılan almak için dağıtım adımları izleyin.

## <a name="deploy-an-ultra-ssd-using-azure-resource-manager"></a>Azure Resource Manager kullanarak bir ultra SSD dağıtma

İlk olarak dağıtmak için VM boyutunu belirler. Bu önizleme kapsamında, yalnızca DsV3 ve EsV3 VM aileleri desteklenir. Bu ikinci tablo başvurmak [blog](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/) bu VM boyutları hakkında ek ayrıntılar için.

Birden çok ultra SSD'ler ile bir VM oluşturmak istiyorsanız, örneğe bakın [birden çok ultra SSD ile VM oluşturma](https://aka.ms/UltraSSDTemplate).

Kendi şablonunuzu kullanmak istiyorsanız, emin **apiVersion** için `Microsoft.Compute/virtualMachines` ve `Microsoft.Compute/Disks` olarak ayarlandığından `2018-06-01` (veya üzeri).

Disk sku kümesine **UltraSSD_LRS**, disk kapasitesi, IOPS, kullanılabilirlik alanı ve aktarım hızı Ultra yüksek bir disk oluşturmak için MB/sn olarak ayarlayın.

VM oluşturulduktan sonra bölüm ve veri diskleri Biçimlendir ve bunları iş yükleriniz için yapılandırabilirsiniz.

## <a name="deploy-an-ultra-ssd-using-cli"></a>CLI kullanarak bir ultra SSD dağıtma

İlk olarak dağıtmak için VM boyutunu belirler. Bu önizleme kapsamında, yalnızca DsV3 ve EsV3 VM aileleri desteklenir. Bu ikinci tablo başvurmak [blog](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/) bu VM boyutları hakkında ek ayrıntılar için.

Ultra yüksek SSD'ler için Ultra yüksek SSD kullanma yeteneği olan bir VM oluşturmanız gerekir.

Değiştirin veya ayarla **$vmname**, **$rgname**, **$diskname**, **$location**, **$password**, **$user** değişkenlerini kendi değerlerinizle. Ayarlama **$zone** adresinden aldığınız, kullanılabilirlik alanı değerine [bu makalenin Başlat](#determine-your-availability-zone). Ardından ultra etkin sanal makine oluşturmak için aşağıdaki CLI komutunu çalıştırın:

```azurecli-interactive
az vm create --subscription $subscription -n $vmname -g $rgname --image Win2016Datacenter --ultra-ssd-enabled --zone $zone --authentication-type password --admin-password $password --admin-username $user --attach-data-disks $diskname --size Standard_D4s_v3 --location $location
```

### <a name="create-an-ultra-ssd-using-cli"></a>CLI kullanarak bir ultra SSD oluşturma

Ultra yüksek SSD kullanma yeteneği olan bir VM'ye sahip olduğunuza göre oluşturabilir ve bir ultra SSD ekleyebilir.

```azurecli-interactive
location="eastus2"
subscription="xxx"
rgname="ultraRG"
diskname="ssd1"
vmname="ultravm1"
zone=123

#create an Ultra SSD disk
az disk create `
--subscription $subscription `
-n $diskname `
-g $rgname `
--size-gb 4 `
--location $location `
--zone $zone `
--sku UltraSSD_LRS `
--disk-iops-read-write 1000 `
--disk-mbps-read-write 50
```

### <a name="adjust-the-performance-of-an-ultra-ssd-using-cli"></a>CLI kullanarak bir ultra SSD performansını ayarlama

Ultra yüksek SSD performanslarını ayarlamanıza olanak sağlayan benzersiz bir özelliği sunar, aşağıdaki komutu bu özelliğin nasıl kullanılacağı gösterilmektedir:

```azurecli-interactive
az disk update `
--subscription $subscription `
--resource-group $rgname `
--name $diskName `
--set diskIopsReadWrite=80000 `
--set diskMbpsReadWrite=800
```

## <a name="deploy-an-ultra-ssd-using-powershell"></a>PowerShell kullanarak bir ultra SSD dağıtma

İlk olarak dağıtmak için VM boyutunu belirler. Bu önizleme kapsamında, yalnızca DsV3 ve EsV3 VM aileleri desteklenir. Bu ikinci tablo başvurmak [blog](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/) bu VM boyutları hakkında ek ayrıntılar için.

Ultra yüksek SSD'ler için Ultra yüksek SSD kullanma yeteneği olan bir VM oluşturmanız gerekir. Değiştirin veya ayarla **$resourcegroup** ve **$vmName** değişkenlerini kendi değerlerinizle. Ayarlama **$zone** adresinden aldığınız, kullanılabilirlik alanı değerine [bu makalenin Başlat](#determine-your-availability-zone). Ardından aşağıdakini çalıştırın [New-AzVm](/powershell/module/az.compute/new-azvm) komutu bir ultra oluşturmak için VM etkin:

```powershell
New-AzVm `
    -ResourceGroupName $resourcegroup `
    -Name $vmName `
    -Location "eastus2" `
    -Image "Win2016Datacenter" `
    -EnableUltraSSD `
    -size "Standard_D4s_v3" `
    -zone $zone
```

### <a name="create-an-ultra-ssd-using-powershell"></a>PowerShell kullanarak bir ultra SSD oluşturma

Ultra yüksek SSD kullanma yeteneği olan bir VM'ye sahip olduğunuza göre oluşturabilir ve bir ultra SSD ekler:

```powershell
$diskconfig = New-AzDiskConfig `
-Location 'EastUS2' `
-DiskSizeGB 8 `
-DiskIOPSReadWrite 1000 `
-DiskMBpsReadWrite 100 `
-AccountType UltraSSD_LRS `
-CreateOption Empty `
-zone $zone;

New-AzDisk `
-ResourceGroupName $resourceGroup `
-DiskName 'Disk02' `
-Disk $diskconfig;
```

### <a name="adjust-the-performance-of-an-ultra-ssd-using-powershell"></a>PowerShell kullanarak bir ultra SSD performansını ayarlama

Ultra yüksek SSD performanslarını ayarlamanıza olanak sağlayan benzersiz bir özellik varsa, aşağıdaki komutu diskini ayırmak zorunda kalmadan performans ayarlayan bir örnektir:

```powershell
$diskupdateconfig = New-AzDiskUpdateConfig -DiskMBpsReadWrite 2000
Update-AzDisk -ResourceGroupName $resourceGroup -DiskName $diskName -DiskUpdate $diskupdateconfig
```

## <a name="next-steps"></a>Sonraki adımlar

Yeni disk türünü deneyin istiyorsanız [bu anketi Önizleme için erişim isteği](https://aka.ms/UltraSSDPreviewSignUp).