---
title: Azure Premium depolama, SQL Server ile kullanma | Microsoft Docs
description: Bu makale Klasik dağıtım modeliyle oluşturulan kaynaklar kullanılmaktadır ve Azure sanal makineler üzerinde çalışan SQL Server ile Azure Premium depolama kullanma yönergeleri sağlar.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
editor: monicar
tags: azure-service-management
ms.assetid: 7ccf99d7-7cce-4e3d-bbab-21b751ab0e88
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/01/2017
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 3b3bb206286629a68c14b6444f3f88ffa0af50dd
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58540882"
---
# <a name="use-azure-premium-storage-with-sql-server-on-virtual-machines"></a>Sanal Makineler’de SQL Server ile Azure Premium Depolama kullanma

## <a name="overview"></a>Genel Bakış

[Azure premium SSD](../disks-types.md) yeni nesil düşük gecikme süresi ve yüksek aktarım hızı GÇ sağlayan depolama. Anahtar g/ç kullanımlı iş yükleri, SQL Server ıaas gibi en iyi şekilde çalışır [sanal makineler](https://azure.microsoft.com/services/virtual-machines/).

> [!IMPORTANT]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

Bu makalede, planlama ve Premium depolama kullanmak için SQL Server çalıştıran bir sanal makine geçişine ilişkin yönergeler sağlar. Bu, Azure altyapısı (ağ, depolama) ve Konuk VM Windows adımlar içerir. Örnekte [ek](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) PowerShell ile geliştirilmiş yerel SSD depolama yararlanmak için daha büyük sanal makineleri taşıma tam kapsamlı bir uçtan uca geçişini gösterir.

IAAS sanal makinelerinde SQL Server ile Azure Premium depolama kullanarak uçtan uca süreci anlamak önemlidir. Buna aşağıdakiler dahildir:

* Premium depolama kullanmak için Önkoşullar kimliği.
* SQL Server ıaas yeni dağıtımlar için Premium depolamaya dağıtma örnekleri.
* Var olan dağıtımlarını, hem tek başına sunucular hem de SQL Always On kullanılabilirlik grupları'nı kullanarak dağıtımları örnekleri.
* Olası geçiş yaklaşıyor.
* Azure, Windows ve SQL Server adımları mevcut bir Always On uygulamasına geçirmeyi gösteren uçtan uca tam örnek.

Azure sanal Makineler'de SQL Server üzerinde daha fazla bilgi için bkz. [Azure sanal Makineler'de SQL Server](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

**Yazar:** Daniel Sol **Technical Reviewers:** Luıs Carlos Vargas Herring, Sanjay Mishra, Pravin Mital, Juergen Thomas, Gonzalo Ruiz'e.

## <a name="prerequisites-for-premium-storage"></a>Premium depolama için Önkoşullar

Premium depolama kullanmak için birkaç önkoşul vardır.

### <a name="machine-size"></a>Makine boyutu

Premium depolama kullanmak için DS serisi sanal makineler (VM) kullanmanız gerekir. Bulut hizmetinizde önce DS serisi makineler kullanmadıysanız, var olan VM'yi silin, bağlı diskleri tutmak ve ardından yeni bir bulut hizmeti VM rol boyutu DS * olarak yeniden oluşturmadan önce oluşturmanız gerekir. Sanal makine boyutları hakkında daha fazla bilgi için bkz. [sanal makine ve bulut hizmeti boyutları için Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="cloud-services"></a>Bulut hizmetleri

Yeni bir bulut hizmeti oluştururken, DS * VM'ler yalnızca Premium depolama ile kullanabilirsiniz. Azure'da SQL Server Always On kullanıyorsanız her zaman üzerinde dinleyici bir bulut hizmeti ile ilişkili Azure iç veya dış yük dengeleyici IP adresi ifade eder. Bu makalede, bu senaryoda, kullanılabilirliği koruyarak geçirme odaklanır.

> [!NOTE]
> * DS serisi, yeni bir bulut hizmeti için dağıtılan ilk VM olması gerekir.
>
>

### <a name="regional-vnets"></a>Bölgesel sanal ağlar

DS * VM'ler için sanal ağ (sanal makinelerinizin bölgesel olması barındırma VNET) yapılandırmanız gerekir. Bu "widens" VNET diğer kümelerinde sağlanması ve aralarında iletişim sağlamak daha büyük sanal makinelerin izin vermektir. "Dar" VNET ilk sonucu gösterir ancak aşağıdaki ekran görüntüsünde, bölgesel sanal ağlar, vurgulanan konumu gösterir.

![RegionalVNET][1]

Bölgesel bir VNET'e geçirmek için bir Microsoft destek bileti gönderebilirsiniz. Microsoft, ardından bir değişiklik yapar. Bölgesel sanal ağlar için Geçişi tamamlamak için ' % s'özelliği AffinityGroup ağ yapılandırmasında değiştirin. İlk ağ yapılandırmasını PowerShell'de dışarı aktarın ve sonra değiştirmek **AffinityGroup** özelliğinde **VirtualNetworkSite** öğesi ile bir **konumu** özelliği. Belirtin `Location = XXXX` burada `XXXX` bir Azure bölgesi. Ardından, yeni yapılandırmayı içeri aktarın.

Örneğin, aşağıdaki VNET yapılandırmasını de göz önünde bulundurur:

```xml
<VirtualNetworkSite name="danAzureSQLnet" AffinityGroup="AzureSQLNetwork">
<AddressSpace>
  <AddressPrefix>10.0.0.0/8</AddressPrefix>
  <AddressPrefix>172.16.0.0/12</AddressPrefix>
</AddressSpace>
<Subnets>
...
</VirtualNetworkSite>
```

Bunu Batı Avrupa'daki bölgesel bir sanal ağa taşımak için yapılandırmayı aşağıdaki gibi değiştirin:
```xml
<VirtualNetworkSite name="danAzureSQLnet" Location="West Europe">
<AddressSpace>
  <AddressPrefix>10.0.0.0/8</AddressPrefix>
  <AddressPrefix>172.16.0.0/12</AddressPrefix>
</AddressSpace>
<Subnets>
...
</VirtualNetworkSite>
```

### <a name="storage-accounts"></a>Depolama hesapları

Premium depolama için yapılandırılan yeni bir depolama hesabı oluşturmanız gerekir. * DS serisi VM kullanırken, Premium ve standart depolama hesaplarından VHD'LERİN ekleyebilirsiniz ancak Premium depolama kullanımını değil, tek tek VHD, depolama hesabında ayarlandığına dikkat edin. İşletim sistemi VHD'si Premium depolama hesabına yerleştirileceği istemiyorsanız bunu göz önüne alabilirsiniz.

Aşağıdaki **yeni AzureStorageAccountPowerShell** "Premium_LRS" komutunu **türü** bir Premium depolama hesabı oluşturur:

```powershell
$newstorageaccountname = "danpremstor"
New-AzureStorageAccount -StorageAccountName $newstorageaccountname -Location "West Europe" -Type "Premium_LRS"   
```

### <a name="vhds-cache-settings"></a>VHD'ler önbellek ayarları

Bir Premium depolama hesabı'nın parçası olan diskler oluşturarak arasındaki temel fark disk önbellek ayardır. SQL Server veri hacmi bu diskleri için kullanmanız önerilir '**okuma önbelleği**'. İşlem günlük birimleri için disk önbellek ayarı ayarlanmalıdır '**hiçbiri**'. Bu standart depolama hesapları için öneriler farklıdır.

Önbellek ayarını VHD'lerin ekledikten sonra değiştirilemez. Ayırma ve VHD'yi bir güncelleştirilmiş önbellek ayarı ile yeniden gerekecektir.

### <a name="windows-storage-spaces"></a>Windows depolama alanları

Kullanabileceğiniz [Windows depolama alanları](https://technet.microsoft.com/library/hh831739.aspx) önceki standart depolama ile yaptığınız gibi bu, zaten depolama alanları kullanan bir sanal Makineyi geçirmek sağlar. Örnekte [ek](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) (adım 9 ve sonrasındaki) ayıklayın ve birden çok ekli VHD'ler ile bir VM almak amacıyla Powershell kodu gösterir.

Depolama havuzları, standart Azure depolama hesabı ile aktarım hızı ve gecikme süresini azaltmak için kullanıldı. Yeni dağıtımlar için Premium depolama ile depolama havuzlarını testinde değeri bulabilirsiniz, ancak depolama kuruluma ek karmaşıklık ekleyin.

#### <a name="how-to-find-which-azure-virtual-disks-map-to-storage-pools"></a>Depolama havuzları için hangi Azure sanal diskler harita bulma

Ekli VHD'ler için farklı bir önbellek ayarı önerileri gibi bir Premium depolama hesabına VHD kopyalamak karar verebilirsiniz. Ancak, bunları yeni DS serisi VM bağladığınızda, önbellek ayarlarını değiştirme gerekebilir. SQL veri dosyalarını ve günlük dosyaları (yerine her ikisini de içeren tek bir VHD) için ayrı VHD'ler olduğunda önbellek ayarları önerilen Premium depolama uygulamak daha kolaydır.

> [!NOTE]
> Önbelleğe alma seçeneği, SQL Server veri ve günlük dosyalarını aynı birimde varsa, veritabanı iş yüklerinizi GÇ erişim desenlerini bağlıdır. Yalnızca test hangi önbelleğe alma bu senaryo için en iyi seçenektir gösterebilirsiniz.
>
>

Windows depolama alanları, birden çok VHD bakmak için sunulan kullanıyorsanız ancak VHD'ler ekli tanımlamak için özgün belirli hangi havuzda böylece, daha sonra önbellek ayarlarını ayarlayabilirsiniz betikleridir uygun şekilde her disk için.

Özgün komut dosyasının VHD'leri depolama havuzuna eşleyen göstermek kullanılabilir değilse, disk/depolama havuzu eşleştirme belirlemek için aşağıdaki adımları kullanabilirsiniz.

Her disk için aşağıdaki adımları kullanın:

1. VM ekli disklerin listesini alma **Get-AzureVM** komutu:

```powershell
Get-AzureVM -ServiceName <servicename> -Name <vmname> | Get-AzureDataDisk
```

1. LUN ve DiskName unutmayın.

    ![DisknameAndLUN][2]
1. Uzak Masaüstü VM'yi başlatın. Ardından **Bilgisayar Yönetimi** | **cihaz Yöneticisi** | **Disk sürücüleri**. Her 'Microsoft sanal disklerin' özelliklerine bakmak

    ![VirtualDiskProperties][3]
1. Burada LUN numarasını VHD'yi sanal Makineye eklerken belirttiğiniz LUN numarasını bir başvurudur.
1. 'Microsoft sanal diski' için şu adrese gidin **ayrıntıları** sekmesini, sonra **özelliği** listesinde, Git **sürücü anahtarı**. İçinde **değer**, Not **uzaklığı**, aşağıdaki ekran görüntüsünde 0002 olduğu. Depolama havuzu başvuran PhysicalDisk2 0002 gösterir.

    ![VirtualDiskPropertyDetails][4]
1. Her depolama havuzu için ilişkili diskler výpisu:

```powershell
Get-StoragePool -FriendlyName AMS1pooldata | Get-PhysicalDisk
```

![GetStoragePool][5]

Artık ilişkilendirmek için bu bilgileri fiziksel diskleri depolama havuzları VHD'ler iliştirilmiş.

Fiziksel disklere ayırmak ve bunları bir Premium depolama hesabına kopyalayabilirsiniz depolama havuzlarında VHD'ler eşledikten sonra bunları ile doğru önbellek ayarı ekleyin. Örnekte bakın [ek](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage), 8-12 arası adımları. Bu adımlarda, bir VM'ye bağlı VHD disk yapılandırması için bir CSV dosyasını ayıklayın, VHD'leri kopyalayın, disk yapılandırma önbellek ayarlarını değiştirme ve DS serisi VM ile eklenen tüm diskler olarak, son olarak VM'yi yeniden dağıtma gösterilmektedir.

### <a name="vm-storage-bandwidth-and-vhd-storage-throughput"></a>VM depolama bant genişliği ve VHD'nin depolama aktarım hızı

Belirtilen DS * VM boyutu ve VHD boyutları depolama performans miktarına bağlıdır. VM'ler, farklı kullanım sınırlarını eklenebilecek VHD'ler sayısı ve en yüksek bant genişliği (MB/sn) destekledikleri sahiptir. Belirli bir bant numaraları için bkz: [sanal makine ve bulut hizmeti boyutları için Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Daha yüksek IOPS büyük disk boyutları ile elde edilir. Geçiş yolunuz düşünürken düşünmelisiniz. Ayrıntılar için [IOPS ve Disk türleri için tabloya bakın](../disks-types.md#premium-ssd).

Son olarak, Vm'lere bağlı tüm diskler için destekledikleri farklı maksimum disk bant genişlikleri olduğunu göz önünde bulundurun. Yüksek yük altında bu VM rol boyutu için kullanılabilen maksimum disk bant genişliği saturate. Örneğin bir Standard_DS14 512 MB/sn'ye kadar destekler. Bu nedenle, üç P30 disk ile VM disk bant saturate. Ancak bu örnekte, aktarım hızı sınırı bağlı olarak okuma ve yazma IOs karışımını aşılmış.

## <a name="new-deployments"></a>Yeni dağıtımlar

Sonraki iki bölümde, SQL Server Vm'leri için Premium depolama nasıl dağıtabileceğiniz gösterilir. Daha önce belirtildiği gibi mutlaka işletim sistemi diski Premium depolama üzerine yerleştirmeniz gerekmez. Yoğun g/ç iş yükleri üzerinde işletim sistemi VHD'si yerleştirmek amaçlanıyorsa, bunu yapmak isteyebilirsiniz.

İlk örnekte, mevcut Azure galeri görüntüleri kullanan gösterir. İkinci örnek, var olan bir standart depolama hesabına sahip olduğunuz özel bir VM görüntüsü kullanma işlemini gösterir.

> [!NOTE]
> Bu örnekler, bölgesel bir sanal ağ zaten oluşturduğunuzu varsayalım.

### <a name="create-a-new-vm-with-premium-storage-with-gallery-image"></a>Galeri görüntüsü ile Premium depolama ile yeni bir VM oluşturun.

Aşağıdaki örnekte, işletim sistemi VHD'si premium depolama üzerine yerleştirin ve Premium depolama VHD'yi gösterilmektedir. Ancak, ayrıca bir standart depolama hesabı işletim sistemi diskini yerleştirin ve ardından bir Premium depolama hesabında yer alan VHD'lerin ekleyin. Her iki senaryo gösterilmektedir.

```powershell
$mysubscription = "DansSubscription"
$location = "West Europe"

#Set up subscription
Set-AzureSubscription -SubscriptionName $mysubscription
Select-AzureSubscription -SubscriptionName $mysubscription -Current  
```

#### <a name="step-1-create-a-premium-storage-account"></a>1. Adım: Premium depolama hesabı oluşturma

```powershell
#Create Premium Storage account, note Type
$newxiostorageaccountname = "danspremsams"
New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  
```

#### <a name="step-2-create-a-new-cloud-service"></a>2. Adım: Yeni bir bulut hizmeti oluşturma

```powershell
$destcloudsvc = "danNewSvcAms"
New-AzureService $destcloudsvc -Location $location
```

#### <a name="step-3-reserve-a-cloud-service-vip-optional"></a>3. Adım: (İsteğe bağlı) bir bulut hizmeti VIP'si ayırın

```powershell
#check exisitng reserved VIP
Get-AzureReservedIP

$reservedVIPName = “sqlcloudVIP”
New-AzureReservedIP –ReservedIPName $reservedVIPName –Label $reservedVIPName –Location $location
```

#### <a name="step-4-create-a-vm-container"></a>4. Adım: Bir sanal makine kapsayıcısı oluşturma

```powershell
#Generate storage keys for later
$xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

##Generate storage acc contexts
$xioContext = New-AzureStorageContext –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary   

#Create container
$containerName = 'vhds'
New-AzureStorageContainer -Name $containerName -Context $xioContext
```

#### <a name="step-5-placing-os-vhd-on-standard-or-premium-storage"></a>5. Adım: İşletim sistemi VHD'si, standart veya Premium depolama yerleştirme

```powershell
#NOTE: Set up subscription and default storage account which is used to place the OS VHD in

#If you want to place the OS VHD Premium Storage Account
Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $newxiostorageaccountname  

#If you wanted to place the OS VHD Standard Storage Account but attach Premium Storage VHDs then you would run this instead:
$standardstorageaccountname = "danstdams"

Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $standardstorageaccountname
```

#### <a name="step-6-create-vm"></a>6. Adım: VM oluşturma

```powershell
#Get list of available SQL Server Images from the Azure Image Gallery.
$galleryImage = Get-AzureVMImage | where-object {$_.ImageName -like "*SQL*2014*Enterprise*"}
$image = $galleryImage.ImageName

#Set up Machine Specific Information
$vmName = "dsDan1"
$vnet = "dansvnetwesteur"
$subnet = "SQL"
$ipaddr = "192.168.0.8"

#Remember to change to DS series VM
$newInstanceSize = "Standard_DS1"

#create new Availability Set
$availabilitySet = "cloudmigAVAMS"

#Machine User Credentials
$userName = "myadmin"
$pass = "mycomplexpwd4*"

#Create VM Config
$vmConfigsl = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $image  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

#Add Data and Log Disks to VM Config
#Note the size specified ‘-DiskSizeInGB 1023’, this attaches 2 x P30 Premium Storage Disk Type
#Utilising the Premium Storage enabled Storage account

$vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-data1.vhd"
$vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "logDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-log1.vhd"

#Create VM
$vmConfigsl  | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)  

#Add RDP Endpoint
$EndpointNameRDPInt = "3389"
Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Add-AzureEndpoint -Name "EndpointNameRDP" -Protocol "TCP" -PublicPort "53385" -LocalPort $EndpointNameRDPInt  | Update-AzureVM

#Check VHD storage account, these should be in $newxiostorageaccountname
Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Get-AzureDataDisk
Get-AzureVM -ServiceName $destcloudsvc -Name $vmName |Get-AzureOSDisk
```

### <a name="create-a-new-vm-to-use-premium-storage-with-a-custom-image"></a>Özel bir görüntü ile Premium depolama kullanmak için yeni bir VM oluşturma

Bu senaryo bir standart depolama hesabında yer alan mevcut özelleştirilmiş görüntülerinizi sahip olduğu gösterilmektedir. Premium depolama alanında işletim sistemi VHD'si yerleştirmek istiyorsanız belirtildiği gibi standart bir depolama hesabında var olan görüntüyü Kopyala ve kullanılabilmesi için önce bunları bir Premium depolama alanına aktarmak gerekir. Görüntü şirket içi, yapabilecekleriniz varsa aynı zamanda, doğrudan Premium depolama hesabına kopyalamak için bu yöntemi kullanın.

#### <a name="step-1-create-storage-account"></a>1. Adım: Depolama Hesabı Oluştur

```powershell
$mysubscription = "DansSubscription"
$location = "West Europe"

#Create Premium Storage account
$newxiostorageaccountname = "danspremsams"
New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

#Standard Storage account
$origstorageaccountname = "danstdams"
```

#### <a name="step-2-create-cloud-service"></a>2. adım, bulut hizmeti oluşturma

```powershell
$destcloudsvc = "danNewSvcAms"
New-AzureService $destcloudsvc -Location $location
```

#### <a name="step-3-use-existing-image"></a>3. Adım: Varolan bir görüntü kullanma

Mevcut bir görüntüyü kullanabilirsiniz. Veya [var olan bir makine görüntüsü alın](../classic/capture-image-classic.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Görüntü, DS * makine olması gerekmez. makinede unutmayın. Görüntüyü oluşturduktan sonra aşağıdaki adımları ile Premium depolama hesabına kopyalamak nasıl Göster **başlangıç AzureStorageBlobCopy** PowerShell komutu.

```powershell
#Get storage account keys:
#Standard Storage account
$originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
#Premium Storage account
$xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

#Set up contexts for the storage accounts:
$origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
$destContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  
```

#### <a name="step-4-copy-blob-between-storage-accounts"></a>4. Adım: BLOB Depolama hesapları arasında kopyalama

```powershell
#Get Image VHD
$myImageVHD = "dansoldonorsql2k14-os-2015-04-15.vhd"
$containerName = 'vhds'

#Copy Blob between accounts
$blob = Start-AzureStorageBlobCopy -SrcBlob $myImageVHD -SrcContainer $containerName `
-DestContainer vhds -Destblob "prem-$myImageVHD" `
-Context $origContext -DestContext $destContext  
```

#### <a name="step-5-regularly-check-copy-status"></a>5. Adım: Düzenli olarak denetim kopyalama durumu:

```powershell
$blob | Get-AzureStorageBlobCopyState
```

#### <a name="step-6-add-image-disk-to-azure-disk-repository-in-subscription"></a>6. Adım: Azure disk abonelik depoda görüntü disk ekleyin

```powershell
$imageMediaLocation = $destContext.BlobEndPoint+"/"+$myImageVHD
$newimageName = "prem"+"dansoldonorsql2k14"

Add-AzureVMImage -ImageName $newimageName -MediaLocation $imageMediaLocation
```

> [!NOTE]
> Durumu başarılı bildiriyor olsa da, bir disk kira hatası yine de alabilir bulabilirsiniz. Bu durumda, yaklaşık 10 dakika bekleyin.

#### <a name="step-7--build-the-vm"></a>7. Adım:  VM oluşturma

Burada, görüntü ve VHD'leri iki Premium depolama VM oluşturuyorsunuz:

```powershell
$newimageName = "prem"+"dansoldonorsql2k14"
#Set up Machine Specific Information
$vmName = "dansolchild"
$vnet = "westeur"
$subnet = "Clients"
$ipaddr = "192.168.0.41"

#This needs to be a new cloud service
$destcloudsvc = "danregsvcamsxio2"

#Use to DS Series VM
$newInstanceSize = "Standard_DS1"

#create new Availability Set
$availabilitySet = "cloudmigAVAMS3"

#Machine User Credentials
$userName = "myadmin"
$pass = "theM)stC0mplexP@ssw0rd!”


#Create VM Config
$vmConfigsl2 = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $newimageName  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

$vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-Datadisk-1.vhd"
$vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "LogDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-logdisk-1.vhd"



$vmConfigsl2 | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet
```

## <a name="existing-deployments-that-do-not-use-always-on-availability-groups"></a>Always On kullanılabilirlik grupları kullanılmayan mevcut dağıtımları

> [!NOTE]
> Varolan dağıtımları için bkz. ilk [önkoşulları](#prerequisites-for-premium-storage) bu makalenin.

Always On kullanılabilirlik grupları hem de kullanmayın SQL Server dağıtımları için farklı ilgili önemli noktalar vardır. Always On kullanmıyorsanız ve var olan bir tek başına SQL Server varsa yeni bir bulut hizmeti ve depolama hesabı kullanarak Premium depolamaya yükseltebilirsiniz. Aşağıdaki seçenekleri göz önünde bulundurun:

* **Yeni bir SQL Server sanal makinesi oluşturma**. Yeni dağıtımda belgelendiği gibi bir Premium depolama hesabını kullanan yeni bir SQL Server sanal makinesi oluşturabilirsiniz. Ardından yedekleme ve SQL Server yapılandırma ve kullanıcı veritabanlarını geri yükleyin. Uygulama dahili veya harici erişiliyor, yeni SQL Server'a başvurmak için güncelleştirilmesi gerekir. 'Db dışında'tüm nesneleri yan yana (SxS) SQL Server Geçiş yaptığınız işe gibi kopyalamanız gerekir. Bu, oturum açma bilgileri, sertifikalar ve bağlı sunucular gibi nesneleri içerir.
* **Mevcut bir SQL Server VM'yi geçirme**. Bu SQL Server VM çevrimdışı duruma gerektirir ve ardından bunu yeni bir bulut hizmetine aktarma, tüm ekli VHD'ler, Premium depolama hesabına kopyalama içerir. Sanal makine çevrimiçi olduğunda, uygulamanın önce sunucu ana bilgisayar adı olarak başvurur. Mevcut disk boyutu performans özellikleri etkiler dikkat edin. Örneğin, 400 GB disk P20 için yuvarlanan. VM, DS serisi VM olarak yeniden oluşturun ve ihtiyaç duyduğunuz boyuta/performans belirtiminin Premium depolama VHD'yi, bu disk performansı gerektirmez biliyorsanız. Ardından ayırmak ve SQL DB dosyaları yeniden bağlayın.

> [!NOTE]
> Premium depolama Disk türüne boyutuna bağlı olarak bir boyut bilincinde olmanız gereken VHD diskleri kopyalama anlamına gelir, yer alacağı, bu disk performans belirtimi belirler. Azure yuvarlar kadar yakın disk boyutu, bu nedenle bir 400 GB disk varsa bu yuvarlanır P20 için. Mevcut g/ç gereksinimlerinizi işletim sistemi VHD'si, bağlı olarak, bu bir Premium depolama hesabına geçirmek gerekmeyebilir.

Sonra SQL Server harici olarak erişilebilen, bulut hizmeti VIP'si değişir. Güncelleştirme uç noktaları, ACL'ler ve DNS ayarları'iniz de.

## <a name="existing-deployments-that-use-always-on-availability-groups"></a>Always On kullanılabilirlik grupları kullanan var olan dağıtımlar

> [!NOTE]
> Varolan dağıtımları için bkz. ilk [önkoşulları](#prerequisites-for-premium-storage) bu makalenin.

Başlangıçta bu bölümde nasıl Always On Azure ağı ile etkileşim sırasında bakacağız. Biz ardından iki senaryo için geçişlerde ayırmanız: Burada miktar kapalı kalma süresi kaydırmadan kaçınma şansınız geçiş ve burada gerekir elde en düşük kapalı kalma süresi geçişi.

Şirket içi SQL Server Always On kullanılabilirlik grupları bir dinleyici kaydeden sanal bir DNS adı bir veya daha fazla SQL Server'lar arasında paylaşılan bir IP adresi yanı sıra şirket içinde kullanın. İstemciler bağlandığında birincil SQL Server dinleyici IP üzerinden yönlendirilir. Bu, o anda her zaman şirket IP kaynağın sahibi sunucusudur.

![Üzerinde DeploymentsUseAlways][6]

Microsoft Azure VM'de bir NIC'e atanmış yalnızca bir IP adresine sahip olabilir, bu nedenle şirket içi olarak soyutlama aynı katman elde etmek için Azure iç/dış yük Dengeleyiciler (ILB/ELB) atandığı IP adresi kullanır. Sunucular arasında paylaşılan IP kaynağı, aynı IP ILB/ELB olarak ayarlanır. Bu DNS yayımlanır ve istemci trafiğini birincil SQL Server çoğaltmasına ILB/ELB geçirilir. ILB/ELB araştırmalar üzerinde her zaman IP kaynağını araştırma için kullandığı SQL sunucusu olan birincil bilir. Önceki örnekte, ELB/ILB tarafından başvurulan bir uç noktaya sahip her düğüm araştırmaları, hangisi yanıt birincil SQL Server.

> [!NOTE]
> ELB ve ILB için belirli bir Azure atanan her ikisi de, bu nedenle Azure yük dengeleyici IP değiştirdiğinde büyük olasılıkla anlamına gelir, bir buluta geçiş bulut hizmeti olan.
>
>

### <a name="migrating-always-on-deployments-that-can-allow-some-downtime"></a>Bazı kapalı kalma süresi sağlayan her zaman açık dağıtımlarını

Bazı kapalı kalma süresi için izin Always On dağıtımlarını geçirmeyi iki strateji vardır:

1. **Mevcut her zaman üzerindeki kümeye daha fazla ikincil çoğaltma ekleme**
2. **Yeni bir her zaman üzerindeki kümeye geçirme**

#### <a name="1-add-more-secondary-replicas-to-an-existing-always-on-cluster"></a>1. Mevcut her zaman üzerindeki kümeye daha fazla ikincil çoğaltma ekleme

Bir strateji, Always On kullanılabilirlik grubu için daha fazla ikincil veritabanı eklemektir. Bu yeni bir bulut hizmeti eklemek ve Yeni Yük Dengeleyici IP dinleyici güncelleştirme gerekir.

##### <a name="points-of-downtime"></a>Kapalı kalma süresi noktaları:

* Küme doğrulama.
* Yeni ikinciller için her zaman açık yük devretme testi.

Windows VM içindeki depolama havuzu için daha yüksek g/ç aktarım hızı kullanıyorsanız, ardından bu tam bir küme doğrulaması sırasında çevrimdışı alınır. Küme düğümleri eklediğinizde, doğrulama testi gereklidir. Testi çalıştırmak için gereken süreyi değişebilir, bu şekilde, bu ne kadar süreyle Bu alan, yaklaşık bir saat almak için temsili bir test ortamında test etmeniz gerekir.

El ile yük devretme ve kaos test beklendiği gibi Always On yüksek kullanılabilirlik işlevleri sağlamak için yeni eklenen düğümlerine yapabileceğiniz zaman sağlamanız gerekir.

![DeploymentUseAlways On2][7]

> [!NOTE]
> Depolama havuzları kullanıldığı SQL Server'ın tüm örnekleri durması gerektiğini doğrulama çalışmadan önce.
>
> ##### <a name="high-level-steps"></a>Üst düzey adımları
>

1. İki yeni SQL Server bağlı Premium depolama ile yeni bir bulut hizmeti oluşturun.
2. TAM yedeklemeler kopyalayın ve geri yükleme olanağıyla **NORECOVERY**.
3. 'Dışında kullanıcı DB' oturum açma bilgileri vb. gibi bağımlı nesneler kopyalayın.
4. Yeni bir yeni iç yük dengeleyici'nı (ILB) oluşturun veya bir dış yük dengeleyici (ELB) kullanın ve ardından her iki yeni düğümde yük dengeli uç noktaları ayarlama ayarlayın.

   > [!NOTE]
   > Devam etmeden önce tüm düğümleri doğru uç nokta yapılandırması sahip denetleyin
   >
   >
5. SQL Server için kullanıcı/uygulama erişimi, (depolama havuzları kullanıyorsanız) durdurun.
6. SQL Server Altyapısı Hizmetleri, tüm düğümler üzerinde (depolama havuzları kullanıyorsanız) durdurun.
7. Küme ve tam doğrulama çalıştırmak için yeni düğümler ekleyin.
8. Doğrulama başarılı olduktan sonra tüm SQL Server hizmetlerini başlatın.
9. İşlem günlüklerini yedeklemek ve kullanıcı veritabanlarını geri yükleyin.
10. Always On kullanılabilirlik grubu yeni bir düğüm ekleme ve çoğaltmanın yerleştirin **zaman uyumlu**.
11. Her zaman açık çok siteli örnekte tabanlı IP adresi kaynağını, yeni bulut hizmeti ILB/ELB PowerShell aracılığıyla ekleme [ek](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage). Windows Kümelemesi ayarlamak **olası sahipler** , **IP adresi** eski yeni düğümler için kaynak. 'Ekleme IP adresi kaynağı aynı alt ağda' bölümüne bakın [ek](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).
12. Yeni düğümlerinden biri için yük devretme.
13. Yeni düğümler otomatik yük devretme iş ortakları ve test yük devretme işlemleri yapın.
14. Özgün düğümleri kullanılabilirlik grubundan kaldırın.

##### <a name="advantages"></a>Yararları

* Yeni SQL sunucuları olabilir (SQL Server ve uygulama) her zaman açık eklenmeden önce test.
* VM boyutunu değiştirin ve depolama tam gereksinimlerinizi özelleştirin. Ancak, tüm SQL dosyası yolları aynı tutmak yararlı olacaktır.
* İkincil çoğaltmalar DB yedeklemeleri aktarımını başlatıldığında denetleyebilirsiniz. Bu Azure kullanmaktan farklıdır **başlangıç AzureStorageBlobCopy** VHD'ler, zaman uyumsuz bir kopya olduğundan kopyalama komutu.

##### <a name="disadvantages"></a>Dezavantajları

* Windows depolama havuzlarını kullanırken, küme kapalı kalma süresi yoktur yeni ek düğümler için tam küme doğrulama sırasında.
* SQL Server sürümü ve ikincil çoğaltmalar mevcut sayısına bağlı olarak, var olan ikincil veritabanı kaldırmadan daha fazla ikincil çoğaltma eklemek mümkün olmayabilir.
* İkincil veritabanı ayarlanırken SQL veri aktarımı uzun olabilir.
* Ek bir maliyet yoktur geçiş sırasında paralel olarak çalışan yeni makineler çalışırken.

#### <a name="2-migrate-to-a-new-always-on-cluster"></a>2. Yeni bir her zaman üzerindeki kümeye geçirme

Başka bir yeni her zaman üzerinde Küme yepyeni düğümlerle yeni bir bulut hizmeti oluşturma ve ardından bunu kullanmak için istemcileri yeniden yönlendirme stratejisidir.

##### <a name="points-of-downtime"></a>Kapalı kalma süresi noktaları

Uygulamalar ve kullanıcılar için yeni her zaman açık dinleyici aktardığınızda kapalı kalma süresi yoktur. Kapalı kalma süresi bağlıdır:

* Son işlem günlüğü yedeklemeleri yeni sunucular veritabanlarını geri yüklemek için geçen süre.
* Her zaman açık dinleyici'ı kullanmak için istemci uygulamaları güncelleştirmek için geçen süre.

##### <a name="advantages"></a>Yararları

* SQL Server, gerçek bir üretim ortamında test edebilirsiniz ve işletim sistemi derleme değişiklikleri.
* Depolama özelleştirmek ve potansiyel olarak VM'nin boyutunu azaltmak için seçeneğiniz vardır. Bu maliyet azalmasına neden olabilir.
* Bu işlem sırasında SQL Server derleme veya sürüm güncelleştirebilirsiniz. Ayrıca, işletim sistemini yükseltebilirsiniz.
* Önceki her zaman küme düz geri alma hedef olarak görev yapabilir.

##### <a name="disadvantages"></a>Dezavantajları

* Aynı anda çalışan iki Always On kümeleri istiyorsanız dinleyicinin DNS adını değiştirmeniz gerekir. İstemci uygulama dizeleri yeni dinleyici adı içermesi gibi bu yönetim ek yükü geçiş sırasında ekler.
* Bunları olabildiğince geçişten önce son eşitleme gereksinimlerini en aza indirmek mümkün olduğunca yakın tutmak için iki ortam arasında eşitleme mekanizması uygulamanız gerekir.
* Çalıştıran yeni ortamı varken maliyet geçiş işlemi sırasında eklenmiş olduğundan.

### <a name="migrating-always-on-deployments-for-minimal-downtime"></a>Geçirme her zaman şirket dağıtımları için en düşük kapalı kalma süresi

En düşük kapalı kalma süresi için Always On geçirme dağıtımlar için iki strateji vardır:

1. **Mevcut bir ikincil kullanır: Tek siteli**
2. **Var olan ikincil çoğaltmalar kullanır: Çok siteli**

#### <a name="1-utilize-an-existing-secondary-single-site"></a>1. Mevcut bir ikincil kullanır: Tek siteli

En düşük kapalı kalma süresi için bir strateji, var olan bir buluta ikincil alabilir ve geçerli bulut hizmetinden kaldırın sağlamaktır. Ardından yeni Premium depolama hesabına VHD kopyalayın ve yeni bulut hizmeti VM oluşturma. Ardından, Yük Devretme Kümelemesi ve dinleyici güncelleştirin.

##### <a name="points-of-downtime"></a>Kapalı kalma süresi noktaları

* Yük dengeli uç nokta ile son düğümü güncelleştirdiğinizde, kapalı kalma süresi yoktur.
* İstemci/DNS yapılandırmanıza bağlı olarak, istemci yeniden bağlanmayı gecikebilir.
* IP adreslerini takas etmek için her zaman üzerinde Küme grubu çevrimdışı duruma getirmeyi seçerseniz ek kapalı kalma süresi yoktur. Bu, bir bağımlılık veya ve olası sahipleri için eklenen IP adresi kaynağı kullanarak önleyebilirsiniz. 'Ekleme IP adresi kaynağı aynı alt ağda' bölümüne bakın [ek](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).

> [!NOTE]
> İçinde bir her zaman üzerinde yük devretme iş ortağı olarak partake eklenen düğümünü istediğiniz zaman Azure noktayla dengeli yük ayarlamak için bir başvuru eklemeniz gerekir. Çalıştırdığınızda **Add-AzureEndpoint** Bunu yapmak için komut kalmasına geçerli bağlantıları, ancak yeni bağlantılar dinleyicisi yük dengeleyici güncelleştirene kurulması mümkün değildir. Bu sınamada görüldü Son 90 120seconds için bu test edilmelidir.

##### <a name="advantages"></a>Yararları

* Hiçbir ek maliyet geçiş sırasında tahakkuk.
* Bire bir geçiş.
* Azaltılmış karmaşıklık.
* Premium depolama SKU'ları için daha yüksek IOPS sağlar. Diskleri VM'den ayrılmış ve yeni bulut hizmeti için kopyalanan bir 3 taraf aracı daha yüksek aktarım hızı sunan VHD boyutunu artırmak için kullanılabilir. VHD boyutları artırmak için bkz. Bu [forum tartışmasına](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows).

##### <a name="disadvantages"></a>Dezavantajları

* Geçiş sırasında HA ve DR geçici kaybı yoktur.
* 1:1 geçiş gibi VHD'ler, sayısı Vm'lerinizi downsize mümkün olmayabilir şekilde destekleyen en az bir VM boyutu kullanmak zorunda.
* Bu senaryo Azure kullanacağınız **başlangıç AzureStorageBlobCopy** zaman uyumsuz komutu. Kopya tamamlanma SLA yoktur. Bu ayrıca aktarmak için veri miktarına bağlıdır sırasındaki bekleme bağlıdır ancak zaman kopyaları değişir. Premium depolama, başka bir bölgede destekleyen başka bir Azure veri merkezi aktarımı yayınlanıyorsa kopyalama süresi de artar. 2 düğüm varsa, olası bir risk azaltma kopyalama testinde daha uzun sürer durumunda göz önünde bulundurun. Bu, aşağıdaki fikirler dahil olabilir.
  * Geçici bir 3 SQL Server düğümü için HA üzerinde anlaşılan kapalı kalma süresiyle geçiş işleminden önce ekleyin.
  * Azure zamanlanmış bakım dışında bir geçiş çalıştırın.
  * Küme çekirdeği doğru yapılandırdığınızdan emin olun.  

##### <a name="high-level-steps"></a>Üst düzey adımları

Bu belge, bir tam, uçtan uca bir örnek, göstermemiz gerekmez ancak [ek](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) bu işlemi gerçekleştirmeye yönelik yararlanılabilir ayrıntıları sağlar.

![MinimalDowntime][8]

* Disk yapılandırmasını toplamak ve düğümü kaldırın (ekli VHD'lerde silmeyin).
* Premium depolama hesabı oluşturma ve standart depolama hesabından VHD'ler kopyalayın
* Yeni bulut hizmeti oluşturun ve bu bulut hizmetinde SQL2 VM dağıtın. Kopyalanan özgün işletim sistemi VHD'si kullanılarak ve kopyalanan VHD'leri VM oluşturun.
* ILB yapılandırma / ELB ve uç noktalar ekleyin.
* Dinleyici tarafından ya da güncelleştirin:
  * Always On grubu çevrimdışı duruma getirmenin ve yeni bir ILB ile Always On dinleyicisi güncelleştirme / ELB IP adresi.
  * Veya IP adresi kaynağı, yeni bulut hizmeti ILB/ELB PowerShell aracılığıyla Windows Kümeleme içine ekleniyor. Daha sonra IP adresi kaynağı olası sahipleri SQL2, geçirilen düğüme ve bu ağ adında veya bağımlılık olarak ayarlayın. 'Ekleme IP adresi kaynağı aynı alt ağda' bölümüne bakın [ek](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).
* DNS yapılandırma/yayma istemcilere denetleyin.
* SQL1 VM'yi geçirme ve 2-4 arası adımlarında gidin.
* Adımları 5ii kullanıyorsanız, SQL1 eklenen IP adresi kaynağı için olası bir sahibi ardından ekleyin
* Yük devretme testi.

#### <a name="2-utilize-existing-secondary-replicas-multi-site"></a>2. Var olan ikincil çoğaltmalar kullanır: Çok Siteli

Birden fazla Azure veri merkezinde (DC) düğümünüz varsa veya karma bir ortamınız varsa, ardından bir Always On yapılandırma bu ortamda kapalı kalma süresini en aza indirmek için kullanabilirsiniz.

Şirket içi veya ikincil bir Azure DC ve ardından yerine bu SQL Server için zaman uyumlu için her zaman açık eşitleme değiştirmek için bir yaklaşımdır. Ardından bir Premium depolama hesabına VHD kopyalayın ve yeni bir bulut hizmeti makineyi yeniden dağıtın. Dinleyici güncelleştirin ve sonra ilk duruma döndürme.

##### <a name="points-of-downtime"></a>Kapalı kalma süresi noktaları

Kapalı kalma süresi alternatif DC ve geri yük devretme zaman oluşur. Ayrıca, istemci/DNS yapılandırmasına bağlıdır ve, istemci yeniden bağlanmayı gecikebilir.
Always On karma yapılandırma aşağıdaki örneği göz önünde bulundurun:

![MultiSite1][9]

##### <a name="advantages"></a>Yararları

* Mevcut altyapısını kullanabilir.
* Azure depolama, DR Azure DC üzerinde önceden ilk yükseltme seçeneği var.
* DR Azure DC depolama yapılandırılması.
* Yük devretme testleri hariç olmak üzere geçiş işlemi sırasında en az iki yük devretmeleri yoktur.
* SQL Server verilerini taşımak ve geri yükleme gerekmez.

##### <a name="disadvantages"></a>Dezavantajları

* SQL Server istemci erişimi bağlı olarak olabilir daha yüksek gecikme süresiyle SQL Server için uygulama alternatif bir DC çalışırken.
* Premium depolama VHD kopyalama süresi uzun olabilir. Bu düğüm kullanılabilirlik grubunda tutmak, kararı etkileyebilir. Bu, birincil düğüm çoğaltılmamış işlemleri, işlem günlüğünde tutulacağı sahip olduğundan günlük kullanımı yoğun iş yükleri geçiş sırasında çalışan gerekli olduğunda için göz önünde bulundurun. Bu nedenle bu önemli ölçüde büyüyebilir.
* Bu senaryo Azure kullanacağınız **başlangıç AzureStorageBlobCopy** zaman uyumsuz komutu. Tamamlanma SLA yoktur. Bu sıradaki bekleme bağlıdır ancak zaman kopyaları değişir aktarmak için veri miktarı ayrıca hizmetlerin. Bu nedenle yalnızca 2 veri Merkezinize bir düğüme sahip, kopya testinde daha uzun sürer durumunda, risk azaltma adımlarını uygulamanız gereken. Bu risk azaltma adımlarını Aşağıdaki fikirler şunlardır:
  * Geçici bir 2 SQL düğümü için HA üzerinde anlaşılan kapalı kalma süresiyle geçiş işleminden önce ekleyin.
  * Azure zamanlanmış bakım dışında bir geçiş çalıştırın.
  * Küme çekirdeği doğru yapılandırdığınızdan emin olun.

Bu senaryo, yüklemenizi belgelenmiş ve depolama için en iyi disk önbellek ayarları değişiklikleri yapmak için nasıl eşlendiğini bildiğiniz varsayılır.

##### <a name="high-level-steps"></a>Üst düzey adımları

![Multisite2][10]

* Şirket içi / Azure DC SQL Server birincil diğer ve diğer otomatik yük devretme iş ortağı (AFP) olun.
* SQL2 disk yapılandırma bilgilerini toplayın ve düğümü kaldırın (ekli VHD'lerde silmeyin).
* Premium depolama hesabı oluşturma ve standart depolama hesabından VHD'ler kopyalayın.
* Yeni bir bulut hizmeti oluşturma ve, primler depolama disklerini bağlı SQL2 VM oluşturun.
* ILB yapılandırma / ELB ve uç noktalar ekleyin.
* Yeni bir ILB ile Always On dinleyicisi güncelleştir / ELB IP adresi ve test yük devretmesi.
* DNS yapılandırmasını denetleyin.
* AFP SQL2 için değiştirmek ve SQL1 geçirme ve 2 – 5 adımlarını gidin.
* Yük devretme testi.
* Sql1'i ve SQL2 AFP geçiş

## <a name="appendix-migrating-a-multisite-always-on-cluster-to-premium-storage"></a>Ek: Bir çoklu site kümesinde her zaman Premium depolamaya geçiş

Bu makalenin geri kalanında her zaman açık, çok siteli küme Premium depolama alanına dönüştürmek için ayrıntılı bir örnek sağlar. Bu ayrıca dinleyicisi bir dış yük dengeleyici (ELB) kullanarak bir iç yük dengeleyici (ILB) dönüştürür.

### <a name="environment"></a>Ortam

* Windows 2k 12 / SQL 2k 12
* SP 1 DB dosyaları
* 2 x düğüm başına depolama havuzları

![Appendix1][11]

### <a name="vm"></a>VM:

Bu örnekte, ILB için bir ELB taşıma göstermek için kullanacağız. Bu ILB için geçiş sırasında geçiş yapma gösterecek şekilde ELB ILB önce kullanılabilir.

![Appendix2][12]

### <a name="pre-steps-connect-to-subscription"></a>Öncesi adımları: Abonelik'e bağlanma

```powershell
Add-AzureAccount

#Set up subscription
Get-AzureSubscription
```

#### <a name="step-1-create-new-storage-account-and-cloud-service"></a>1. Adım: Yeni depolama hesabı oluşturun ve bulut hizmeti

```powershell
$mysubscription = "DansSubscription"
$location = "West Europe"

#Storage accounts
#current storage account where the vm to migrate resides
$origstorageaccountname = "danstdams"

#Create Premium Storage account
$newxiostorageaccountname = "danspremsams"
New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

#Generate storage keys for later
$originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
$xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

#Generate storage acc contexts
$origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
$xioContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

#Set up subscription and default storage account
Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $origstorageaccountname
Select-AzureSubscription -SubscriptionName $mysubscription -Current

#CREATE NEW CLOUD SVC
$vnet = "dansvnetwesteur"

##Existing cloud service
$sourceSvc="dansolSrcAms"

##Create new cloud service
$destcloudsvc = "danNewSvcAms"
New-AzureService $destcloudsvc -Location $location
```

#### <a name="step-2-increase-the-permitted-failures-on-resources-optional"></a>2. Adım: İzin verilen kaynaklar hatalarında artırmak \<isteğe bağlı >

Always On kullanılabilirlik grubu için ait bazı kaynaklar sınırı yoktur kaç hatalarda burada kaynak grubunu yeniden başlatmak için Küme hizmeti çalışır bir süre içinde ortaya çıkabilir. Bu sınıra yakın alabilirsiniz makineleri kapatarak el ile yük devretme ve tetikleyici yük devretme yoksa bu yana Bu yordam, walking yaparken bu artırmak önerilir.

Yük Devretme Kümesi Yöneticisi'nde bunun için hata indirimi çift akıllıca her zaman açık kaynak grubunun özelliklerini gidin:

![Appendix3][13]

En fazla hata sayısı 6'ya değiştirin.

#### <a name="step-3-addition-ip-address-resource-for-cluster-group-optional"></a>3. Adım: Küme grubu için ek IP adresi kaynağı \<isteğe bağlı >

Küme grubunu ve bu hizalanması için bulut alt ağ ile tek bir IP adresi varsa, küme ağ adı ve küme IP kaynak değil sonra çevrimiçi olması için yanlışlıkla tüm küme düğümleri o ağ üzerinde bulutta çevrimdışı izlerseniz, dikkatli olun. Bu durumda, diğer küme kaynakları için güncelleştirmeleri engeller.

#### <a name="step-4-dns-configuration"></a>4. Adım: DNS yapılandırması

Sorunsuz bir geçiş uygulama bağlı DNS nasıl yapılıyor kullanılan ve güncelleştirildi.
Always On yüklü olduğunda Windows Küme kaynak grubu oluşturur. yük devretme kümesi Yöneticisi'ni açın, gördüğünüz isteğe bağlı olarak en az üç kaynaklara sahip, belgenin başvurduğu iki şunlardır:

* Sanal ağ adı (VNN) – bağlanan istemciler SQL Server Always On üzerinden bağlanmak isteyen olduğunda DNS adı.
* IP adresi kaynağı – IP adresi VNN ile ilişkili birden fazla olabilir ve bir çoklu site yapılandırmasında bir IP adresi başına site/alt ağ sahip.

SQL Server, SQL Server sürücüsü dinleyicisi ile ilişkili DNS kayıtları alır ve her zaman açık bağlanmaya Client bağlanma zaman IP adresi ilişkili olabilir. Ardından, bu etkileyen bazı faktörleri ele alır.

Dinleyici adıyla ilişkili eş zamanlı DNS kayıtları yalnızca ilişkili IP adresleri sayısına bağlıdır ancak ' de yük devretme kümelemesi için her zaman açık VNN kaynak RegisterAllIpProviders'setting.

Dağıttığınızda Azure her zaman açık dinleyici ve IP adresleri oluşturmak için farklı adımlar vardır, 1 'RegisterAllIpProviders' el ile yapılandırmanız gerekmez, bu nerede 1 olarak ayarlanmış bir şirket içi dağıtım için her zaman açık farklıdır.

'RegisterAllIpProviders' 0 ise, ardından yalnızca bir dinleyicisi ile DNS'de DNS kaydı ilişkili görürsünüz:

![Appendix4][14]

1 'RegisterAllIpProviders' ise:

![Appendix5][15]

Aşağıdaki kod, VNN ayarları dökümleri ve sizin için ayarlar. Bu değişikliğin etkili olması için VNN çevrimdışına alın ve tekrar çevrimiçi etkinleştirmeniz gerekir. Bu dinleyici sürer çevrimdışı neden olan istemci bağlantı kesintisi.

```powershell
##Always On Listener Name
$ListenerName = "Mylistener"
##Get AlwaysOn Network Name Settings
Get-ClusterResource $ListenerName| Get-ClusterParameter
##Set RegisterAllProvidersIP
Get-ClusterResource $ListenerName| Set-ClusterParameter RegisterAllProvidersIP  1
```

Sonraki bir adımda geçiş, her zaman açık dinleyici başvuran bir yük dengeleyici güncelleştirilmiş IP adresiyle güncelleştirmeniz gerekir, bu da bir IP adresi kaynak temizleme ve toplama içerir. IP Güncelleştirme tamamlandıktan sonra yeni IP adresini DNS bölgesinde güncelleştirildi ve istemcilerin kendi yerel DNS önbelleği güncelleştirdiğiniz emin olmalısınız.

Müşterilerinizin farklı ağ Segmentte bulunan ve farklı bir DNS sunucusu başvuru, ne DNS bölge aktarım hakkında geçiş sırasında uygulama yeniden gibi süresi düşünmeniz gereken en az bölge aktarım süresini herhangi yeni bir IP ile kısıtlanıyor Dinleyici için adresleri. Burada zaman kısıtlamasından varsa, tartışın ve Windows takımlarınızı bir artımlı bölge aktarımı zorlama test ve ayrıca DNS ana bilgisayar kaydının bir alt yaşam süresi (TTL için) put, böylece istemciler güncelleştirin. Daha fazla bilgi için [artımlı bölge aktarımlarını](https://technet.microsoft.com/library/cc958973.aspx) ve [başlangıç DnsServerZoneTransfer](https://docs.microsoft.com/powershell/module/dnsserver/start-dnsserverzonetransfer).

Varsayılan TTL, azure'da her zaman açık dinleyici'ile ilişkili bir DNS kaydı için 1200 saniyedir. İstemcilerin emin olmak için geçişiniz sırasında kısıtlaması dinleyici için güncelleştirilmiş bir IP adresi ile kendi DNS güncelleştirme altında olduğunda bu azaltmak isteyebilirsiniz. Görebilir ve VNN yapılandırmasını dökme tarafından yapılandırmasını değiştirme:

```powershell
$AGName = "myProductionAG"
$ListenerName = "Mylistener"
#Look at HostRecordTTL
Get-ClusterResource $ListenerName| Get-ClusterParameter

#Set HostRecordTTL Examples
Get-ClusterResource $ListenerName| Set-ClusterParameter -Name "HostRecordTTL" 120
```

> [!NOTE]
> Düşük 'HostRecordTTL', daha yüksek bir DNS trafik miktarını gerçekleşir.

##### <a name="client-application-settings"></a>İstemci uygulama ayarları

SQL istemci uygulamanızın desteklediği .NET 4.5 SQLClient sonra kullanabileceğiniz ' MULTISUBNETFAILOVER = TRUE' anahtar sözcüğü. SQL Always On kullanılabilirlik grubu yük devretme sırasında daha hızlı bağlantı için izin verdiğinden, bu anahtar sözcük uygulanmalıdır. Paralel her zaman açık dinleyici'ile ilişkili tüm IP adresleri üzerinden numaralandırır ve yük devretme sırasında daha agresif bir TCP bağlantı deneme hızı gerçekleştirir.

Önceki ayarları hakkında daha fazla bilgi için bkz. [MultiSubnetFailover anahtar sözcüğü ve ilişkili özellikleri](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover). Ayrıca bkz: [yüksek kullanılabilirlik, olağanüstü durum kurtarma için SqlClient desteği](https://msdn.microsoft.com/library/hh205662\(v=vs.110\).aspx).

#### <a name="step-5-cluster-quorum-settings"></a>5. Adım: Küme çekirdeği ayarlarını

Aynı anda en az bir SQL Server kullanıma sürüyor kısımlarında iki düğüm ile dosya paylaşımı tanığı (FSW) kullanıyorsanız, küme çekirdek ayarını değiştirmeniz gerekir, düğüm çoğunluğu izin vermek için dinamik oy kullanan çekirdek ayarlamanız gerekir , tek bir düğüm ayakta kalmasını sağlar.

```powershell
Set-ClusterQuorum -NodeMajority  
```

Küme çekirdeği yapılandırma ve yönetme hakkında daha fazla bilgi için bkz. [yapılandırıp yönetebileceğiniz bir Windows Server 2012 yük devretme kümesinde çekirdeği](https://technet.microsoft.com/library/jj612870.aspx).

#### <a name="step-6-extract-existing-endpoints-and-acls"></a>6. Adım: Var olan uç noktaları ve ACL'ler ayıklayın

```powershell
#GET Endpoint info
Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureEndpoint
#GET ACL Rules for Each EP, this example is for the Always On Endpoint
Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureAclConfig -EndpointName "myAOEndPoint-LB"  
```

Bu metin, bir dosyaya kaydedin.

#### <a name="step-7-change-failover-partners-and-replication-modes"></a>7. Adım: Yük devretme iş ortakları ve çoğaltma modu değiştirme

İkiden fazla SQL sunucuları varsa, başka bir ikincil başka bir DC ya da şirket içi yük devretme 'İçin zaman uyumlu' değiştirmek ve bir otomatik yük devretme iş ortağı (AFP) olun, değişiklik yaparken HA korumak bu olduğundan. Bunu TSQL yapabilirsiniz ancak SSMS değiştirin:

![Appendix6][16]

#### <a name="step-8-remove-secondary-vm-from-cloud-service"></a>8. adım: İkincil VM'yi bulut hizmetinden kaldırın.

Bir bulut ikincil düğüm önce geçirilmesi için planlama. Bu düğümü şu anda birincil ise, el ile bir yük devretme başlatmalıdır.

```powershell
$vmNameToMigrate="dansqlams2"

#Check machine status
Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#Shutdown secondary VM
Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM


#Extract disk configuration

##Building Existing Data Disk Configuration
$file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
$datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
foreach ($disk in $datadisks)
{
    $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
    $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
}

#Get OS Disk
$osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
$osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
$osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
$addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
$addosdisk | add-content -path $file

#Import disk config
$diskobjects  = Import-CSV $file

#Check disk config, make sure below returns the disks associated with the VM
$diskobjects

#Identify OS Disk
$osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
$osdiskforbuild = $osdiskimport.diskName

#Check machibe is off
Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

#Drop machine and rebuild to new cls
Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate
```

#### <a name="step-9-change-disk-caching-settings-in-csv-file-and-save"></a>9. adım: Diski önbelleğe alma CSV dosyasındaki ayarları değiştirin ve kaydedin

Veri birimleri için bu salt okunur olarak ayarlanmalıdır.

TLOG birimler için bunların hiçbiri olarak ayarlanması gerekir.

![Appendix7][17]

#### <a name="step-10-copy-vhds"></a>10. adım: VHD'ler kopyalayın

```powershell
#Ensure you have created the container for these:
$containerName = 'vhds'

#Create container
New-AzureStorageContainer -Name $containerName -Context $xioContext

####DISK COPYING####
#Get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
ForEach ($disk in $diskobjects)
{
   $lun = $disk.Lun
   $vhdname = $disk.vhdname
   $cacheoption = $disk.HostCaching
   $disklabel = $disk.DiskLabel
   $diskName = $disk.DiskName
   Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

   #Start async copy
   Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname.blob.core.windows.net/vhds/$vhdname" `
    -SrcContext $origContext `
    -DestContainer $containerName `
    -DestBlob $vhdname `
    -DestContext $xioContext
}
```


Premium depolama hesabına VHD kopyalama durumunu denetleyebilirsiniz:

```powershell
ForEach ($disk in $diskobjects)
{
   $lun = $disk.Lun
   $vhdname = $disk.vhdname
   $cacheoption = $disk.HostCaching
   $disklabel = $disk.DiskLabel
   $diskName = $disk.DiskName

   $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContext
   Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
}
```

![Appendix8][18]

Tüm bu başarılı kaydedilen kadar bekleyin.

Tek tek bloblar için daha fazla bilgi için:

```powershell
Get-AzureStorageBlobCopyState -Blob "blobname.vhd" -Container $containerName -Context $xioContext
```

#### <a name="step-11-register-os-disk"></a>11. adım: İşletim sistemi diski kaydetme

```powershell
#Change storage account
Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountname
Select-AzureSubscription -SubscriptionName $mysubscription -Current

#Register OS disk
$osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
$osvhd = $osdiskimport.vhdname
$osdiskforbuild = $osdiskimport.diskName

#Registering OS disk, but as XIO disk
$xioDiskName = $osdiskforbuild + "xio"
Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"
```

#### <a name="step-12-import-secondary-into-new-cloud-service"></a>12. adım: Yeni bir bulut hizmeti ikincil içeri aktarma

Aşağıdaki kod eklendi seçeneğini burada da kullanan makine içeri aktarabilir ve retainable VIP'i kullan.

```powershell
#Build VM Config
$ipaddr = "192.168.0.5"
#Remember to change to XIO
$newInstanceSize = "Standard_DS13"
$subnet = "SQL"

#Create new Availability Set
$availabilitySet = "cloudmigAVAMS"

#build machine config into object
$vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

#Reload disk config
$diskobjects  = Import-CSV $file
$datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

ForEach ( $attachdatadisk in $datadiskimport)
{
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###Attaching disks to a VM during a deploy to a new cloud service and new storage account is different from just attaching VHDs to just a redeploy in a new cloud service
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

}

#Create VM
$vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)
```

#### <a name="step-13-create-ilb-on-new-cloud-svc-add-load-balanced-endpoints-and-acls"></a>13. adım: ILB yeni bulut Svc üzerinde oluşturabilir yük ekleyebilir dengeli uç noktaları ve ACL'ler

```powershell
#Check for existing ILB
GET-AzureInternalLoadBalancer -ServiceName $destcloudsvc

$ilb="sqlIntIlbDest"
$subnet = "SQL"
$IP="192.168.0.25"
Add-AzureInternalLoadBalancer -ServiceName $destcloudsvc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP

#Endpoints
$epname="sqlIntEP"
$prot="tcp"
$locport=1433
$pubport=1433
Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM

#SET Azure ACLs or Network Security Groups & Windows FWs

#https://msdn.microsoft.com/library/azure/dn495192.aspx

####WAIT FOR FULL AlwaysOn RESYNCRONISATION!!!!!!!!!#####
```

#### <a name="step-14-update-always-on"></a>14. adım: Her zaman açık güncelleştirme

```powershell
#Code to be executed on a Cluster Node
$ClusterNetworkNameAmsterdam = "Cluster Network 2" # the azure cluster subnet network name
$newCloudServiceIPAmsterdam = "192.168.0.25" # IP address of your cloud service

$AGName = "myProductionAG"
$ListenerName = "Mylistener"


Add-ClusterResource "IP Address $newCloudServiceIPAmsterdam" -ResourceType "IP Address" -Group $AGName -ErrorAction Stop |  Set-ClusterParameter -Multiple @{"Address"="$newCloudServiceIPAmsterdam";"ProbePort"="59999";SubnetMask="255.255.255.255";"Network"=$ClusterNetworkNameAmsterdam;"OverrideAddressMatch"=1;"EnableDhcp"=0} -ErrorAction Stop

#set dependency and NETBIOS, then remove old IP address

#set NETBIOS, then remove old IP address
Get-ClusterGroup $AGName | Get-ClusterResource -Name "IP Address $newCloudServiceIPAmsterdam" | Set-ClusterParameter -Name EnableNetBIOS -Value 0

#set dependency to Listener (OR Dependency) and delete previous IP Address resource that references:

#Make sure no static records in DNS
```

![Appendix9][19]

Artık eski bulut hizmetinin IP adresini kaldırın.

![Appendix10][20]

#### <a name="step-15-dns-update-check"></a>15. adım: DNS güncelleştirme denetimi

Artık SQL Server istemci ağlarınızda DNS sunucularını denetleyerek ve kümeleme eklenen IP adresi için ek ana bilgisayar kaydı eklemiştir emin olun. Bu DNS sunucuları güncelleştirilmemiş, DNS bölge aktarımı zorlama göz önünde bulundurun ve istemcileri emin var. alt ağ hem de her zaman şirket IP adreslerine çözümleyebilir, otomatik DNS çoğaltma için beklemeniz gerekmez bu olduğundan.

#### <a name="step-16-reconfigure-always-on"></a>16. adım: Her zaman açık yeniden yapılandırma

Bu noktada, tam olarak şirket içi düğümle yeniden eşitleyin ve zaman uyumlu çoğaltma düğüme geçiş ve afp'yi de kolaylaştırmak için geçirilen bu düğüm için ikincil bekleyin.  

#### <a name="step-17-migrate-second-node"></a>17. adım: İkinci düğümü geçirme

```powershell
$vmNameToMigrate="dansqlams1"

Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#Get endpoint information
$endpoint = Get-AzureVM -ServiceName $sourceSvc  -Name $vmNameToMigrate | Get-AzureEndpoint

#Shutdown VM
Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM

#Get disk config

#Building Existing Data Disk Configuration
$file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
$datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
foreach ($disk in $datadisks)
{
    $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
    $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
}

#Get OS Disk
$osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
$osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
$osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
$addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
$addosdisk | add-content -path $file

#Import disk config
$diskobjects  = Import-CSV $file

#Check disk configuration
$diskobjects

#Identify OS Disk
$osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
$osdiskforbuild = $osdiskimport.diskName

#Check machine is off
Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

#Drop machine and rebuild to new cls
Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate
```

#### <a name="step-18-change-disk-caching-settings-in-csv-file-and-save"></a>Adım 18: Diski önbelleğe alma CSV dosyasındaki ayarları değiştirin ve kaydedin

Veri birimleri için önbellek ayarları için salt okunur olarak ayarlanmalıdır.

TLOG birimleri için önbellek ayarları hiçbiri olarak ayarlanmalıdır.

![Appendix11][21]

#### <a name="step-19-create-new-independent-storage-account-for-secondary-node"></a>19. adım: İkincil düğüm için yeni bağımsız bir depolama hesabı oluşturma

```powershell
$newxiostorageaccountnamenode2 = "danspremsams2"
New-AzureStorageAccount -StorageAccountName $newxiostorageaccountnamenode2 -Location $location -Type "Premium_LRS"  

#Reset the storage account src if node 1 in a different storage account
$origstorageaccountname2nd = "danstdams2"

#Generate storage keys for later
$xiostoragenode2 = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountnamenode2

#Generate storage acc contexts
$xioContextnode2 = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountnamenode2 -StorageAccountKey $xiostoragenode2.Primary  

#Set up subscription and default storage account
Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
Select-AzureSubscription -SubscriptionName $mysubscription -Current
```

#### <a name="step-20-copy-vhds"></a>20. adım: VHD'ler kopyalayın

```powershell
#Ensure you have created the container for these:
$containerName = 'vhds'

#Create container
New-AzureStorageContainer -Name $containerName -Context $xioContextnode2  

####DISK COPYING####
##get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
ForEach ($disk in $diskobjects)
{
   $lun = $disk.Lun
   $vhdname = $disk.vhdname
   $cacheoption = $disk.HostCaching
   $disklabel = $disk.DiskLabel
   $diskName = $disk.DiskName
   Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

   #Start async copy
   Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname2nd.blob.core.windows.net/vhds/$vhdname" `
    -SrcContext $origContext `
    -DestContainer $containerName `
    -DestBlob $vhdname `
    -DestContext $xioContextnode2
}

#Check for copy progress

#check individual blob status
Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContext
```

Tüm VHD'ler için VHD kopyalama durumu kontrol edebilirsiniz:

```powershell
ForEach ($disk in $diskobjects)
{
    $lun = $disk.Lun
    $vhdname = $disk.vhdname
    $cacheoption = $disk.HostCaching
    $disklabel = $disk.DiskLabel
    $diskName = $disk.DiskName

    $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContextnode2
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
}
```

![Appendix12][22]

Tüm bu başarılı kaydedilen kadar bekleyin.

Tek tek bloblar için daha fazla bilgi için:

```powershell
#Check individual blob status
Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContextnode2
```

#### <a name="step-21-register-os-disk"></a>21. adım: İşletim sistemi diski kaydetme

```powershell
#change storage account to the new XIO storage account
Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
Select-AzureSubscription -SubscriptionName $mysubscription -Current

#Register OS disk
$osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
$osvhd = $osdiskimport.vhdname
$osdiskforbuild = $osdiskimport.diskName

#Registering OS disk, but as XIO disk
$xioDiskName = $osdiskforbuild + "xio"
Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

#Build VM Config
$ipaddr = "192.168.0.4"
$newInstanceSize = "Standard_DS13"

#Join to existing Availability Set

#Build machine config into object
$vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

#Reload disk config
$diskobjects  = Import-CSV $file
$datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

ForEach ( $attachdatadisk in $datadiskimport)
{
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###This is different to just a straight cloud service change
    #note if you do not have a disk label the command below will fail, populate as required.
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

}

#Create VM
$vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet -Verbose
```

#### <a name="step-22-add-load-balanced-endpoints-and-acls"></a>22. adım: Ekleme yük dengeli uç noktaları ve ACL'ler

```powershell
#Endpoints
$epname="sqlIntEP"
$prot="tcp"
$locport=1433
$pubport=1433
Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM


#STOP!!! CHECK in the Azure portal or Machine Endpoints through PowerShell that these Endpoints are created!

#SET ACLs or Azure Network Security Groups & Windows FWs

#https://msdn.microsoft.com/library/azure/dn495192.aspx
```

#### <a name="step-23-test-failover"></a>23. adım: Yük devretme testi

Always On şirket içi düğümle eşitlemek geçirilen düğümü için bekleyin. Zaman uyumlu çoğaltma moduna ve onu eşitlenene kadar bekleyin. Ardından, AFP olduğu bir şirket içi yük ilk düğümü geçişi. Çalıştıktan sonra son taşınan düğümü için AFP değiştirin.

Tüm düğümler arasında yük devretme testi ve yük devretme iş olarak emin olmak için chaos testleri beklenen olsa ve zamanında manor çalıştırmak gerekir.

#### <a name="step-24-put-back-cluster-quorum-settings--dns-ttl--failover-pntrs--sync-settings"></a>24. adım: Küme çekirdeği ayarlarını geri put / DNS TTL'yi / yük devretme Pntrs / eşitleme ayarları

##### <a name="adding-ip-address-resource-on-same-subnet"></a>Aynı alt ağda IP adresi kaynağı ekleme

Yalnızca iki SQL sunucusuna sahip ve bunları yeni bir bulut hizmetine geçiş yapmak istiyorsanız ancak bunları aynı alt ağda tutmak istiyorsanız, özgün her zaman şirket IP adresini silin ve yeni IP adresi eklemek için dinleyici çevrimdışı alma önleyebilirsiniz. Başka bir alt ağ ile sanal makineleri geçiriyorsanız, bu alt ağa başvuran bir ek küme ağı olduğundan, bunu yapmak gerekmez.

Geçirilen ikincil getirildi ve var olan birincil yük devretmeden önce yeni bir bulut hizmeti için yeni IP adresi kaynağı eklenen sonra Bu önlem içinde küme Yük Devretme Yöneticisi almanız gerekir:

IP adresi eklemek için ek, adım 14 bakın.

1. Geçerli IP adresi kaynağı için 'Var olan birincil SQL Server', olası sahip örnekte, 'dansqlams4' değiştirin:

    ![Appendix13][23]
2. Yeni IP adresi kaynağı için 'Geçirildi ikincil SQL Server', 'dansqlams5' örnekte olası sahip değiştirin:

    ![Appendix14][24]
3. Son düğümü geçirildiğinde bu düğüm, olası bir sahibi eklendiğinde için olası sahipler düzenlenmesi gerekir ve bu ayarlandıktan sonra Yük devretme kullanabilirsiniz:

    ![Appendix15][25]

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Premium depolama](../disks-types.md)
* [Sanal Makineler](https://azure.microsoft.com/services/virtual-machines/)
* [Azure sanal makineler'de SQL Server](../sql/virtual-machines-windows-sql-server-iaas-overview.md)

<!-- IMAGES -->
[1]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/1_VNET_Portal.png
[2]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/2_Diskname_Lun.png
[3]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/3_Virtual_Disk_Properties.png
[4]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/4_Virtual_Disk_Properties_Details.png
[5]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/5_Get_Storage_Pool.png
[6]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/6_Deployments_Always_On.png
[7]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/7_Add_More_Secondaries.png
[8]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/8_Use_Existing_Secondary_Single_Site.png
[9]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site.png
[10]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site_b.png
[11]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_01.png
[12]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_02.png
[13]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_03.png
[14]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_04.png
[15]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_05.png
[16]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_06.png
[17]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_07.png
[18]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_08.png
[19]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_09.png
[20]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_10.png
[21]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_11.png
[22]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_12.png
[23]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_13.png
[24]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_14.png
[25]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_15.png
