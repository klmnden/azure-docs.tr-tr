---
title: SQL Server ile Azure Premium Storage kullanma | Microsoft Docs
description: Bu makale Klasik dağıtım modeli kullanılarak oluşturulmuş kaynaklarını kullanır ve Azure sanal makinelerde çalışan SQL Server ile Azure Premium Storage kullanma hakkında yönergeler sağlar.
services: virtual-machines-windows
documentationcenter: ''
author: danielsollondon
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
ms.author: jroth
ms.openlocfilehash: 3d3fdd8865a293c5e2f0df6a97910ac8e2a07d4c
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
ms.locfileid: "29400622"
---
# <a name="use-azure-premium-storage-with-sql-server-on-virtual-machines"></a>Sanal Makineler’de SQL Server ile Azure Premium Depolama kullanma
## <a name="overview"></a>Genel Bakış
[Azure Premium Storage](../premium-storage.md) düşük gecikme süresi ve yüksek verimlilik GÇ sağlayan depolama gelecek nesil. Anahtar g/ç yoğun iş yükleri, Iaas üzerinde SQL Server gibi en iyi şekilde çalışır [sanal makineleri](https://azure.microsoft.com/services/virtual-machines/).

> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

Bu makale, planlama ve Premium depolama kullanmak için SQL Server çalıştıran bir sanal makine geçirmek için yönergeler sağlar. Bu, Azure altyapı (ağ, depolama) ve Konuk Windows VM adımları içerir. Örnekte [ek](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) PowerShell ile geliştirilmiş yerel SSD depolama yararlanmak için daha büyük sanal makineleri taşımak nasıl tam kapsamlı bir uçtan uca geçişini gösterir.

Uçtan uca işlemi kullanılarak Azure Premium Storage IAAS Vm'leri üzerinde SQL Server ile anlamak önemlidir. Buna aşağıdakiler dahildir:

* Premium depolama kullanmak için Önkoşullar kimliği.
* SQL Server Iaas üzerinde yeni dağıtımlar için Premium depolama alanına dağıtma örnekleri.
* Geçirme varolan dağıtımları, hem tek başına sunucular hem de SQL Always On kullanılabilirlik grupları kullanarak dağıtımları örnekleri.
* Olası geçiş yaklaşıyor.
* Mevcut bir Always On uygulamasına yükseltme için Azure, Windows ve SQL Server adımlar gösteren baştan sona tam örnek.

Azure Virtual Machines'de SQL Server üzerinde daha fazla bilgi için bkz: [Azure Virtual Machines'de SQL Server](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

**Yazar:** Daniel Sol **Teknik İnceleme:** Haluk Carlos Vargas Herring, Sanjay Mishra, Pravin Mital, Juergen Thomas, Gonzalo Ruiz'e.

## <a name="prerequisites-for-premium-storage"></a>Premium depolama için Önkoşullar
Premium depolama kullanmak için birkaç önkoşul vardır.

### <a name="machine-size"></a>Makine boyutu
Premium depolama kullanmak için DS serisi sanal makineler (VM) kullanmanız gerekir. Bulut hizmetinizde önce DS serisi makineler kullanmadıysanız, var olan VM silme, ekli diskleri tutmak ve ardından yeni bir bulut hizmeti VM DS * rol boyutu olarak yeniden oluşturmadan önce oluşturmanız gerekir. Sanal makine boyutları hakkında daha fazla bilgi için bkz: [sanal makine ve bulut hizmeti boyutları Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="cloud-services"></a>Bulut hizmetleri
Yeni bir bulut hizmetinde oluşturduğunuzda, DS * VM'ler yalnızca Premium Storage ile kullanabilirsiniz. Azure'da SQL Server Always On kullanıyorsanız, her zaman üzerinde dinleyicisi bulut hizmetiyle ilişkili Azure iç veya dış yük dengeleyici IP adresi ifade eder. Bu makalede, bu senaryoda kullanılabilirliğini korurken geçirmek nasıl odaklanır.

> [!NOTE]
> DS * serisi yeni bulut hizmeti dağıtılmış ilk VM olması gerekir.
>
>

### <a name="regional-vnets"></a>Bölgesel sanal ağlar
DS * VM'ler için sanal ağ (Bölgesel olması için sanal makineleri barındırdığı VNET) yapılandırmanız gerekir. Bu "widens" VNET diğer kümelerde sağlanması ve aralarında iletişim sağlamak daha büyük sanal makineleri izin vermektir. Oysa "dar" VNET ilk sonuçlarını gösterir aşağıdaki ekran görüntüsünde vurgulanan konumda bölgesel sanal ağlara gösterir.

![RegionalVNET][1]

Bölgesel bir sanal ağ geçirmek için Microsoft destek bileti yükseltebilirsiniz. Microsoft, sonra bir değişiklik yapar. Bölgesel sanal ağlar için geçiş işlemini tamamlamak için ' % s'özelliğini AffinityGroup ağ yapılandırmasında değiştirin. İlk ağ yapılandırmasını PowerShell'de dışarı aktarmak ve ardından Değiştir **AffinityGroup** özelliğinde **VirtualNetworkSite** öğesi ile bir **konumu** özelliği. Belirtin `Location = XXXX` burada `XXXX` bir Azure bölgesi. Ardından yeni yapılandırmasını alın.

Örneğin, aşağıdaki VNET yapılandırmasını dikkate:

    <VirtualNetworkSite name="danAzureSQLnet" AffinityGroup="AzureSQLNetwork">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

Bunu Batı Avrupa'da bölgesel bir sanal ağ taşımak için yapılandırma aşağıdaki gibi değiştirin:

    <VirtualNetworkSite name="danAzureSQLnet" Location="West Europe">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

### <a name="storage-accounts"></a>Depolama hesapları
Premium Storage için yapılandırılan yeni bir depolama hesabı oluşturmanız gerekir. DS * serisi VM kullanırken VHD'ler Premium ve standart depolama hesaplarından ekleyebilirsiniz ancak Premium depolama kullanımını tek tek VHD'ler üzerinde depolama hesabında ayarlandığını unutmayın. Premium depolama hesabına OS VHD'yi yerleştirmek istemiyorsanız bu düşünebilirsiniz.

Aşağıdaki **yeni AzureStorageAccountPowerShell** "Premium_LRS" komutunu **türü** bir Premium depolama hesabı oluşturur:

    $newstorageaccountname = "danpremstor"
    New-AzureStorageAccount -StorageAccountName $newstorageaccountname -Location "West Europe" -Type "Premium_LRS"   

### <a name="vhds-cache-settings"></a>VHD'ler önbellek ayarları
Premium depolama hesabı parçası olan diskleri oluşturma arasındaki temel fark disk önbelleği ayardır. SQL Server veri birimi, diskler için kullanmanız önerilir '**okuma önbelleği**'. İşlem günlük birimleri için disk önbellek ayarı ayarlanmalı '**hiçbiri**'. Bu öneriler standart depolama hesapları için farklıdır.

Önbellek ayarını VHD'leri bağlandıktan sonra değiştirilemez. Ayırma ve VHD güncelleştirilmiş önbellek ayarı ile yeniden iliştirin gereksiniminiz olacaktır.

### <a name="windows-storage-spaces"></a>Windows depolama alanları
Kullanabileceğiniz [Windows depolama alanları](https://technet.microsoft.com/library/hh831739.aspx) önceki standart depolama ile yaptığınız gibi bu, zaten depolama alanları kullanan bir VM geçirmenize olanak sağlar. Örnekte [ek](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) (adım 9 ve iletme) ayıklayın ve birden çok ekli VHD'yle bir VM'yi almak amacıyla Powershell kodu gösterir.

Depolama havuzları verimliliği artırmak ve gecikme süresini azaltmak için standart Azure depolama hesabıyla kullanılmıştır. Yeni dağıtımlar için Premium depolama ile depolama havuzlarını testinde değeri bulabilirsiniz, ancak depolama kuruluma ek karmaşıklık ekleyin.

#### <a name="how-to-find-which-azure-virtual-disks-map-to-storage-pools"></a>Depolama havuzları için hangi Azure sanal diskler haritanın bulma
Ekli VHD'ler için farklı önbellek ayarı öneriler olarak VHD'leri bir Premium depolama hesabına kopyalamak karar verebilirsiniz. Ancak, bunları yeni DS serisi VM bağladığınızda, önbellek ayarlarını değiştirme gerekebilir. Premium depolama SQL veri dosyalarını ve günlük dosyaları (yerine her ikisini de içeren tek bir VHD) için ayrı VHD'leri olduğunda önerilen önbellek ayarları uygulamak daha basittir.

> [!NOTE]
> SQL Server veri ve günlük dosyaları aynı birimde varsa, önbelleğe alma seçeneği, veritabanı iş yükleri için g/ç erişimi desenleri bağlıdır. Yalnızca test hangi önbelleğe alma bu senaryo için en iyi seçenektir personeline gösterebilir.
>
>

Windows depolama alanları, birden çok VHD bakmak için ihtiyacınız hale kullanıyorsanız, ancak VHD'ler ekli tanımlamak için özgün belirli hangi havuzda böylece, daha sonra önbellek ayarları ayarlayabilirsiniz betiklerdir uygun şekilde her disk için.

Özgün komut dosyasını, depolama havuzuna VHD'ler eşleştiren göstermek kullanılabilir değilse, disk/depolama havuzu eşlemesini belirlemek için aşağıdaki adımları kullanabilirsiniz.

Her disk için aşağıdaki adımları kullanın:

1. VM diskleri listesini alın iliştirilmiş **Get-AzureVM** komutu:

    Get-AzureVM - ServiceName <servicename> -Name <vmname> | Get-AzureDataDisk
2. LUN ve Diskname unutmayın.

    ![DisknameAndLUN][2]
3. Uzak Masaüstü VM başlatın. Ardından **Bilgisayar Yönetimi** | **Aygıt Yöneticisi'ni** | **Disk sürücüleri**. Her 'Microsoft sanal disklerin' özelliklerine bakmak

    ![VirtualDiskProperties][3]
4. LUN numarası burada VHD'yi VM'e eklerken belirttiğiniz LUN numarası başvurudur.
5. 'Microsoft sanal diski' gitmek için **ayrıntıları** sekmesi, sonra **özelliği** listesi, Git **sürücü anahtarı**. İçinde **değeri**, Not **uzaklığı**, aşağıdaki ekran görüntüsü 0002 olduğu. Depolama havuzu başvuran PhysicalDisk2 0002 gösterir.

    ![VirtualDiskPropertyDetails][4]
6. Her depolama havuzu için ilişkili diskler dökün:

    Get-StoragePool - FriendlyName AMS1pooldata | Get-PhysicalDisk

    ![GetStoragePool][5]

Kullanabileceğiniz artık ilişkilendirmek için bu bilgileri VHD'ler fiziksel diskleri depolama havuzları bağlı.

Depolama ayırma ve bunları üzerinden bir Premium depolama alanına kopyalanmaya havuzlarındaki fiziksel disklere VHD'ler eşledikten sonra bunları ile doğru önbellek ayarı ekleyin. Örnekte bkz [ek](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage), 8-12 arası adımları. Bu adımlar, bir VM ekli VHD disk yapılandırmasını bir CSV dosyasına ayıklamak, VHD'leri kopyalama, disk yapılandırma önbelleği ayarlarını değiştirme ve son olarak VM eklenen tüm diskler ile VM DS serisi olarak yeniden dağıtın gösterilmektedir.

### <a name="vm-storage-bandwidth-and-vhd-storage-throughput"></a>VM depolama bant genişliği ve VHD depolama üretilen iş
Depolama performansı miktarı belirtilen DS * VM boyutu ve VHD boyutlarına bağlıdır. VM'ler farklı kesintileri eklenebilecek VHD'ler sayısı ve en yüksek bant genişliği (MB/sn) desteği vardır. Belirli bir bant numaraları için bkz: [sanal makine ve bulut hizmeti boyutları Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Daha fazla IOPS ile büyük disk boyutları elde edilir. Geçiş yolunuz hakkında düşünürken, düşünmelisiniz. Ayrıntılar için [IOPS ve Disk türleri için tabloya bakın](../premium-storage.md#scalability-and-performance-targets).

Son olarak, VM'ye bağlı tüm diskler için destek farklı en fazla disk bant genişlikleri sahip göz önünde bulundurun. Yüksek yük altında bu VM rol boyutu için kullanılabilen en fazla disk bant genişliği saturate. Örneğin bir Standard_DS14 512 MB/sn kadar destekler; Bu nedenle, üç P30 disklerinde disk bant genişliği VM saturate. Ancak bu örnekte, üretilen iş sınırı okuma ve yazma g/ç işlemine bağlı karışımı aştı.

## <a name="new-deployments"></a>Yeni dağıtımlar
SQL Server Vm'lerinin Premium depolama alanına nasıl dağıtabileceğiniz sonraki iki bölümde gösterilmektedir. Önceden belirtildiği gibi mutlaka Premium depolama OS diske yerleştirmek gerekmez. Yoğun g/ç iş yükleri üzerinde işletim sistemi VHD'yi yerleştirmek amaçlanıyorsa bu yapmayı seçebilirsiniz.

İlk örnek, var olan Azure galeri görüntüleri kullanılarak gösterir. İkinci örnek var olan bir standart depolama hesabında sahip özel bir VM görüntüsü kullanmayı gösterir.

> [!NOTE]
> Bu örnekler, bölgesel bir sanal ağ zaten oluşturduğunuzu varsayalım.
>
>

### <a name="create-a-new-vm-with-premium-storage-with-gallery-image"></a>Premium depolama galeri görüntüsü ile birlikte yeni bir VM oluşturun
Aşağıdaki örnek, işletim sistemi VHD premium depolama üzerine yerleştirin ve Premium depolama VHD'yi gösterilmektedir. Ancak, aynı zamanda bir standart depolama hesabı işletim sistemi diski yerleştirin ve ardından bir Premium depolama hesabında bulunan VHD ekleyin. Her iki senaryoyu gösterilmiştir.

    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Set up subscription
    Set-AzureSubscription -SubscriptionName $mysubscription
    Select-AzureSubscription -SubscriptionName $mysubscription -Current  

#### <a name="step-1-create-a-premium-storage-account"></a>1. adım: bir Premium depolama hesabı oluşturma
    #Create Premium Storage account, note Type
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  


#### <a name="step-2-create-a-new-cloud-service"></a>2. adım: yeni bir bulut hizmeti oluşturma
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-reserve-a-cloud-service-vip-optional"></a>3. adım: (isteğe bağlı) bir bulut hizmeti VIP ayırma
    #check exisitng reserved VIP
    Get-AzureReservedIP

    $reservedVIPName = “sqlcloudVIP”
    New-AzureReservedIP –ReservedIPName $reservedVIPName –Label $reservedVIPName –Location $location

#### <a name="step-4-create-a-vm-container"></a>4. adım: bir VM kapsayıcı oluşturma
    #Generate storage keys for later
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    ##Generate storage acc contexts
    $xioContext = New-AzureStorageContext –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary   

    #Create container
    $containerName = 'vhds'
    New-AzureStorageContainer -Name $containerName -Context $xioContext

#### <a name="step-5-placing-os-vhd-on-standard-or-premium-storage"></a>5. adım: İşletim sistemi VHD standart veya Premium Storage yerleştirme
    #NOTE: Set up subscription and default storage account which is used to place the OS VHD in

    #If you want to place the OS VHD Premium Storage Account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $newxiostorageaccountname  

    #If you wanted to place the OS VHD Standard Storage Account but attach Premium Storage VHDs then you would run this instead:
    $standardstorageaccountname = "danstdams"

    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $standardstorageaccountname

#### <a name="step-6-create-vm"></a>6. adım: VM oluşturma
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

    #create new Avaiability Set
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


### <a name="create-a-new-vm-to-use-premium-storage-with-a-custom-image"></a>Premium depolama ile özel bir görüntü kullanmak için yeni bir VM oluşturun
Bu senaryo, bir standart depolama hesabında bulunan var olan özelleştirilmiş görüntüleri sahip olduğu gösterir. Premium depolama OS VHD eklemek istediğiniz belirtildiği gibi standart depolama hesabı, var olan görüntüyü kopyalamak ve kullanılabilmesi için önce bunları Premium depolama alanına aktarmak gerekir. Bir görüntü şirket içi, yapabilecekleriniz varsa da doğrudan Premium depolama hesabına kopyalamak için bu yöntemi kullanın.

#### <a name="step-1-create-storage-account"></a>Adım 1: Depolama hesabı oluşturma
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Standard Storage account
    $origstorageaccountname = "danstdams"

#### <a name="step-2-create-cloud-service"></a>2. adım, bulut hizmeti oluşturma
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-use-existing-image"></a>3. adım: Mevcut görüntü kullanın
Varolan bir görüntü kullanabilirsiniz. Veya, [var olan bir makine görüntüsünü ele](../classic/capture-image-classic.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Görüntü, DS * makinenin olması gerekmez. makine unutmayın. Görüntü olduktan sonra aşağıdaki adımlar, Premium Storage hesabıyla kopyalamak nasıl gösterir. **başlangıç AzureStorageBlobCopy** PowerShell komutunu.

    #Get storage account keys:
    #Standard Storage account
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    #Premium Storage account
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Set up contexts for the storage accounts:
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $destContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

#### <a name="step-4-copy-blob-between-storage-accounts"></a>4. adım: Kopyalama Blob Depolama hesapları arasında
    #Get Image VHD
    $myImageVHD = "dansoldonorsql2k14-os-2015-04-15.vhd"
    $containerName = 'vhds'

    #Copy Blob between accounts
    $blob = Start-AzureStorageBlobCopy -SrcBlob $myImageVHD -SrcContainer $containerName `
    -DestContainer vhds -Destblob "prem-$myImageVHD" `
    -Context $origContext -DestContext $destContext  

#### <a name="step-5-regularly-check-copy-status"></a>5. adım: Kopyalama durumu düzenli olarak kontrol edin:
    $blob | Get-AzureStorageBlobCopyState

#### <a name="step-6-add-image-disk-to-azure-disk-repository-in-subscription"></a>6. adım: Azure disk abonelik deposunda görüntü disk ekleyin
    $imageMediaLocation = $destContext.BlobEndPoint+"/"+$myImageVHD
    $newimageName = "prem"+"dansoldonorsql2k14"

    Add-AzureVMImage -ImageName $newimageName -MediaLocation $imageMediaLocation

> [!NOTE]
> Başarı durum raporu olsa da, bir disk kira hatası hala alabilir bulabilirsiniz. Bu durumda, yaklaşık 10 dakika bekleyin.
>
>

#### <a name="step-7--build-the-vm"></a>7. adım: VM oluşturma
Burada görüntünüzü ve iki Premium depolama VHD'leri VM oluşturuyorsanız:

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

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS3"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "theM)stC0mplexP@ssw0rd!”


    #Create VM Config
    $vmConfigsl2 = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $newimageName  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-Datadisk-1.vhd"
    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "LogDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-logdisk-1.vhd"



    $vmConfigsl2 | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet

## <a name="existing-deployments-that-do-not-use-always-on-availability-groups"></a>Always On kullanılabilirlik grupları kullanmayın var olan dağıtımlar
> [!NOTE]
> Var olan dağıtımlar için önce bkz [Önkoşullar](#prerequisites-for-premium-storage) bu makalenin.
>
>

Always On kullanılabilirlik grupları hem de kullanmayın SQL Server dağıtımları için farklı noktalar vardır. Her zaman açık kullanmıyorsanız ve var olan bir tek başına SQL Server yüklüyse, yeni bir bulut hizmeti ve depolama hesabı kullanarak Premium depolama alanına yükseltebilirsiniz. Aşağıdaki seçenekleri göz önünde bulundurun:

* **Yeni bir SQL Server VM oluşturmak**. Yeni dağıtımlarda belirtildiği gibi yeni bir SQL Server bir Premium depolama hesabını kullanan VM oluşturabilirsiniz. Ardından yedekleyebilir ve SQL Server yapılandırma ve kullanıcı veritabanlarını geri yükleyin. Uygulama dahili veya harici erişiliyor, yeni SQL Server başvurmak için güncelleştirilmesi gerekir. Yan yana (SxS) SQL Server Geçiş yaptığınız gibi 'dışı db' tüm nesneleri kopyalamak gerekir. Bu, oturum açma bilgileri, sertifikalar ve bağlantılı sunucular gibi nesneleri içerir.
* **Mevcut bir SQL Server VM'yi geçirme**. Bu SQL Server VM çevrimdışı duruma almayı gerektirir ve ardından yeni bir bulut hizmeti aktarma, tüm bağlı VHD Premium Storage hesabına kopyalama içerir. VM çevrimiçi olduğunda, uygulamanın önce sunucu ana bilgisayar adı olarak başvurur. Mevcut disk boyutu performans özellikleri etkiler unutmayın. Örneğin, 400 GB disk için bir P20 yuvarlanan. DS serisi VM olarak VM oluşturun ve Premium depolama VHD'yi ihtiyaç duyduğunuz boyutu/performans belirtimi, bu disk performansı gerektirmez biliyorsanız. Ardından detach ve SQL DB dosyaları yeniden ekleyin.

> [!NOTE]
> Premium depolama diskini türüne boyutuna bağlı olarak boyutu bilincinde olmanız gereken VHD diskleri kopyalama anlamına gelir bunların içine kalan, bu disk performans belirtimi belirler. Azure yuvarlar yakın disk boyutu kadar bu nedenle 400 GB disk varsa bu yuvarlanan bir P20. Varolan GÇ gereksinimlerinize OS VHD'nin bağlı olarak, bu bir Premium depolama hesabına geçirmek gerekmeyebilir.
>
>

SQL Server'ınızdaki dışarıdan erişiliyorsa, bulut hizmet VIP'de değiştirir. Ayrıca güncelleştirme uç noktaları, ACL'ler ve DNS ayarları vardır.

## <a name="existing-deployments-that-use-always-on-availability-groups"></a>Always On kullanılabilirlik grupları kullanan var olan dağıtımlar
> [!NOTE]
> Var olan dağıtımlar için önce bkz [Önkoşullar](#prerequisites-for-premium-storage) bu makalenin.
>
>

Başlangıçta bu bölümde biz nasıl her zaman açık Azure ağ ile etkileşim sırasında arayın. Biz sonra iki senaryo için geçişleri ayırmanız: Burada miktar kapalı kalma süresi izin geçişler ve burada gerekir ulaşmanıza en düşük kapalı kalma geçişleri.

Bir dinleyici sanal bir DNS adı bir veya daha fazla SQL Server'lar arasında paylaşılan bir IP adresi ile birlikte kaydeden şirket içi SQL Server Always On kullanılabilirlik grupları kullanın. İstemciler bağlandığında birincil SQL Server dinleyici IP üzerinden yönlendirilir. Bu, o anda üzerinde her zaman IP kaynağına sahip sunucusudur.

![Üzerinde DeploymentsUseAlways][6]

Microsoft Azure VM'de bir NIC atanan tek bir IP adresi olabilir, bu nedenle aynı şirket olarak Soyutlama Katmanı elde etmek için Azure iç/dış yük dengeleyici (ILB/ELB) atanan IP adresini kullanır. Sunucular arasında paylaşılan IP kaynağı aynı IP ILB/ELB olarak ayarlanır. Bu DNS yayımlanır ve istemci trafiğini birincil SQL Server çoğaltma ILB/ELB geçirilir. ILB/ELB üzerinde her zaman IP kaynağı araştırma araştırmalar kullandığından SQL sunucusu olan birincil bilir. Önceki örnekte, ELB/ILB tarafından başvurulan bir uç nokta sahip her bir düğüm araştırmaları, hangisi yanıt birincil SQL Server.

> [!NOTE]
> ELB ve ILB için belirli bir Azure atanan her ikisi de, bu nedenle tüm bulut geçiş Azure yük dengeleyici IP değişiklikler büyük olasılıkla anlamına gelir, bulut hizmeti olan.
>
>

### <a name="migrating-always-on-deployments-that-can-allow-some-downtime"></a>Miktar kapalı kalma süresi sağlayan geçirme her zaman açık dağıtımları
Bazı kesintiler izin her zaman açık dağıtımları geçirmek için iki strateji vardır:

1. **Daha fazla ikincil çoğaltmaları mevcut her zaman üzerinde kümeye Ekle**
2. **Yeni bir her zaman üzerinde kümeye geçirme**

#### <a name="1-add-more-secondary-replicas-to-an-existing-always-on-cluster"></a>1. Daha fazla ikincil çoğaltmaları mevcut her zaman üzerinde kümeye Ekle
Her zaman üzerindeki kullanılabilirlik grubu için daha fazla ikincil kopya eklemek için bir strateji kullanılır. Bunları yeni bir bulut hizmeti eklemek ve Yeni Yük Dengeleyici IP dinleyicisi güncelleştirmek gerekir.

##### <a name="points-of-downtime"></a>Kapalı kalma süresi noktalar:
* Küme doğrulama.
* Her zaman açık yük test etme için yeni ikincil öğe.

VM içindeki Windows depolama havuzu için daha yüksek g/ç işleme kullanıyorsanız, ardından bu tam bir küme doğrulama sırasında çevrimdışı alınır. Kümeye düğüm eklemek için doğrulama testi gereklidir. Bu bu gereken süreyi, yaklaşık bir saat almak için temsili bir test ortamında test etmeniz gerekir böylece testi çalıştırmak için geçen süre farklılık gösterebilir.

Burada el ile yük devretme ve test chaos beklendiği gibi her zaman üzerinde yüksek kullanılabilirlik işlevleri sağlamak için yeni eklenen düğümlerine gerçekleştirebileceğiniz zaman hazırlamanız.

![DeploymentUseAlways On2][7]

> [!NOTE]
> Depolama havuzlarını kullanıldığı SQL Server'ın tüm örneklerini durması gerektiğini doğrulama çalıştırılmadan önce.
>
> ##### <a name="high-level-steps"></a>Üst düzey adımlar
>

1. İki yeni SQL sunucusuna bağlı Premium depolama ile yeni bulut hizmeti oluşturun.
2. TAM yedeklemeler kopyalayın ve geri yükleme ile **NORECOVERY**.
3. 'Dışında kullanıcı DB' oturumları vb. gibi bağımlı nesnelere kopyalayın.
4. Yeni bir yeni iç yük dengeleyici'nı (ILB) oluşturun veya bir dış yük dengeleyici (ELB) kullanın ve ardından her iki yeni düğümde yük dengeli uç noktaları ayarlayın.

   > [!NOTE]
   > Devam etmeden önce tüm düğümleri doğru uç nokta yapılandırması sahip denetleyin
   >
   >
5. SQL Server kullanıcı/uygulama erişimi (depolama havuzlarını kullanıyorsanız) durdurun.
6. SQL Server motoru Hizmetleri tüm düğümler üzerinde (depolama havuzlarını kullanıyorsanız) durdurun.
7. Küme ve tam doğrulama çalıştırmak için yeni düğümler ekleyin.
8. Doğrulama başarılı olduktan sonra tüm SQL Server hizmetlerini başlatın.
9. İşlem günlüklerini yedekleme ve kullanıcı veritabanlarını geri yükleyin.
10. Yeni düğümler her zaman üzerinde kullanılabilirlik grubuna ekleyin ve çoğaltma içine yerleştirin **zaman uyumlu**.
11. Çok siteli örnekte dayalı her zaman açık olan yeni bulut hizmeti ILB/ELB PowerShell aracılığıyla IP adresi kaynağı ekleyin [ek](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage). Windows Kümelemesi içinde ayarlamak **olası sahipler** , **IP adresi** eski yeni düğümler için kaynak. 'Ekleme IP adresi kaynağı aynı alt ağda bulunan' bölümüne bakın [ek](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).
12. Yeni düğümlerinden biri için yük devretme.
13. Yeni düğümler otomatik yük devretme iş ortakları ve test yük devretme olun.
14. Özgün düğüm kullanılabilirlik grubundan kaldırın.

##### <a name="advantages"></a>Avantajları
* Yeni SQL sunucuları olabilir (SQL Server ve uygulama) için her zaman açık eklenmeden önce test.
* VM boyutunu değiştirin ve depolama tam gereksinimlerinizi özelleştirebilirsiniz. Ancak, tüm SQL dosya yolları aynı tutmak yararlı olacaktır.
* İkincil çoğaltmaları DB yedeklemeleri aktarımını başlatıldığında denetleyebilirsiniz. Bu Azure kullanmaktan farklıdır **başlangıç AzureStorageBlobCopy** komutunu, zaman uyumsuz bir kopya olduğundan VHD'ler, kopyalayın.

##### <a name="disadvantages"></a>Olumsuz yönleri
* Windows depolama havuzlarını kullanırken var. küme kapalı kalma süresi yeni ek düğüm için tam küme doğrulama sırasında
* SQL Server sürümü ve ikincil çoğaltmaları varolan sayısına bağlı olarak mevcut ikinciller kaldırmadan daha fazla ikincil çoğaltmaları eklemeniz mümkün olmayabilir.
* İkincil kopya ayarı uzun SQL veri aktarımı zamandan olabilir.
* Yoktur ek maliyet geçiş sırasında paralel olarak çalışan yeni makineler varken.

#### <a name="2-migrate-to-a-new-always-on-cluster"></a>2. Yeni bir her zaman üzerinde kümeye geçirme
Başka bir yepyeni her zaman üzerinde Küme yepyeni düğümleriyle yeni bulut hizmeti oluşturmak ve kullanmak için istemcileri yeniden yönlendirmek için stratejidir.

##### <a name="points-of-downtime"></a>Kapalı kalma süresi noktaları
Uygulamaların ve kullanıcıların yeni her zaman açık dinleyici için aktardığınızda kapalı kalma süresi yok. Kapalı kalma süresi bağlıdır:

* Son işlem günlüğü yedeklemeleri yeni sunucularda veritabanlarını geri yüklemek için geçen süre.
* Her zaman açık dinleyici kullanmak için istemci uygulamaları güncelleştirmek için geçen süre.

##### <a name="advantages"></a>Avantajları
* SQL Server, gerçek bir üretim ortamına sınayabilirsiniz ve işletim sistemi derleme değişiklikleri.
* Depolama özelleştirmek ve potansiyel olarak VM boyutunu azaltmak için seçeneğiniz vardır. Bu maliyet azalmasına neden olabilir.
* Bu işlem sırasında SQL Server yapı veya sürüm güncelleştirebilirsiniz. Ayrıca, işletim sistemi yükseltme yapabilirsiniz.
* Önceki her zaman üzerinde Küme düz geri alma hedef olarak davranamaz.

##### <a name="disadvantages"></a>Olumsuz yönleri
* Eşzamanlı olarak çalıştıran her iki her zaman açık kümeler istiyorsanız dinleyicisi DNS adını değiştirmeniz gerekir. Bu, istemci uygulama dizeleri yeni dinleyici adı yansıtmalıdır gibi yönetim ek yükü geçişi sırasında ekler.
* Bunları olabildiğince geçişten önce son eşitleme gereksinimlerini en aza indirmek mümkün olduğunca yakın tutmak için iki ortam arasında eşitleme mekanizması uygulamalıdır.
* Var. çalıştıran yeni ortam varken maliyet geçiş sırasında eklenir.

### <a name="migrating-always-on-deployments-for-minimal-downtime"></a>Geçirme her zaman şirket dağıtımları için en az kapalı kalma süresi
En düşük kapalı kalma için her zaman açık geçirme dağıtımlar için iki strateji vardır:

1. **Var olan bir ikincil kullanma: tek siteli**
2. **Var olan ikincil çoğaltmalar kullanma: çok siteli**

#### <a name="1-utilize-an-existing-secondary-single-site"></a>1. Var olan bir ikincil kullanma: tek siteli
Mevcut bir buluta ikincil alın ve geçerli bulut hizmetinden kaldırmak için en az kapalı kalma süresi için bir strateji etmektir. Ardından yeni Premium depolama hesabı VHD'leri kopyalayın ve VM yeni bulut hizmeti oluşturun. Ardından Yük Devretme Kümelemesi ve dinleyicisinde güncelleştirin.

##### <a name="points-of-downtime"></a>Kapalı kalma süresi noktaları
* Yük dengeli uç nokta ile son düğümü güncelleştirdiğinizde kapalı kalma süresi yok.
* İstemci/DNS yapılandırmanıza bağlı olarak, istemci yeniden bağlanmayı gecikebilir.
* IP adreslerini değiştirilecek üzerinde her zaman küme grubu çevrimdışı bırakmayı seçerseniz ek kapalı kalma süresi yok. Bu, bir veya bağımlılık ve olası sahipleri için eklenen IP adresi kaynağı kullanarak önleyebilirsiniz. 'Ekleme IP adresi kaynağı aynı alt ağda bulunan' bölümüne bakın [ek](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).

> [!NOTE]
> İçinde bir her zaman üzerinde yük devretme ortağı olarak partake için eklenen düğümünü istediğinizde Azure noktayla dengeli yük ayarlamak için bir başvuru eklemeniz gerekir. Çalıştırdığınızda **Ekle AzureEndpoint** Bunu yapmak için komutu, kalmasına geçerli bağlantıları'nı açın, ancak yeni bağlantılar dinleyicisi yük dengeleyici güncelleştirdi kadar kurulması mümkün değildir. Bu test görüldü Son 90 120seconds için bu test edilmelidir.
>
>

##### <a name="advantages"></a>Avantajları
* Hiçbir ek maliyet geçiş sırasında ücrete.
* Bire bir geçiş.
* Azaltılan karmaşıklık.
* Premium depolama SKU'ları için daha yüksek IOPS sağlar. Diskler sanal makineden ayrılmış ve yeni bulut hizmeti kopyalanan bir 3. taraf aracı yüksek kapatma sağlar VHD boyutunu artırmak için kullanılabilir. VHD boyutları artırmak için bu bkz [forum tartışmasına](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows).

##### <a name="disadvantages"></a>Olumsuz yönleri
* Geçiş sırasında HA ve DR kaybı yoktur.
* 1:1 geçiş gibi VHD'ler, sayısı Vm'leriniz downsize mümkün olmayabilir şekilde destekleyen en az bir VM boyutu kullanmak zorunda.
* Bu senaryo Azure kullanırsınız **başlangıç AzureStorageBlobCopy** zaman uyumsuz komutunu. Kopya tamamlanma hiçbir SLA yoktur. Bu ayrıca aktarmak için veri miktarına bağlıdır sırasındaki bekleme bağlıdır sırada kopyaları süresi olarak değişir. Başka bir bölgede Premium Storage destekleyen başka bir Azure veri merkezi aktarımı yayınlanıyorsa kopyalama süresini artırır. 2 düğümleri varsa, kopyalama testinde uzun sürdüğünde durumunda olası azaltma göz önünde bulundurun. Bu Aşağıdaki fikirler içerebilir.
  * Bir geçici 3 SQL Server düğümü HA için üzerinde anlaşılan kapalı kalma süresi ile geçişten önce ekleyin.
  * Azure zamanlanmış bakım dışında geçiş çalıştırın.
  * Küme çekirdeğini doğru yapılandırılmış olun.  

##### <a name="high-level-steps"></a>Üst düzey adımlar
Bu belgede bir tam, uçtan uca bir örnek, gösterilmemiştir ancak [ek](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) bunu gerçekleştirmek için de ayrıntıları sağlar.

![MinimalDowntime][8]

* Disk yapılandırmasını toplamak ve düğüm kaldırmak (ekli VHD'ler silmeyin).
* Premium depolama hesabı oluşturma ve standart depolama hesabından VHD'ler kopyalayın
* Yeni bulut hizmeti oluşturun ve bu bulut Hizmeti'nde SQL2 VM yeniden dağıtın. Kopyalanan özgün işletim sistemi VHD kullanarak ve kopyalanan VHD'leri VM oluşturun.
* ILB yapılandırma / ELB ve uç noktalar ekleyebilirsiniz.
* Dinleyici ya da güncelleştirin:
  * Her zaman üzerindeki grubu çevrimdışına almak ve yeni ILB ile her zaman üzerinde dinleyicisi güncelleştirme / ELB IP adresi.
  * Veya IP adresi kaynağı, yeni bulut hizmeti ILB/ELB PowerShell aracılığıyla Windows Kümeleme içine ekleme. Ardından IP adresi kaynağının olası sahipler SQL2, geçirilen düğüme ve bu ağ adı veya bağımlılık olarak ayarlayın. 'Ekleme IP adresi kaynağı aynı alt ağda bulunan' bölümüne bakın [ek](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).
* DNS yapılandırması/yayma istemcilere denetleyin.
* SQL1 VM'yi geçirmek ve 2-4 arası adımlar gidin.
* Adımları 5ii kullanıyorsanız, SQL1 eklenen IP adres kaynağı için olası sahip olarak ardından ekleyin
* Test yük devretmeleri.

#### <a name="2-utilize-existing-secondary-replicas-multi-site"></a>2. Var olan ikincil çoğaltmalar kullanma: çok siteli
Birden fazla Azure veri merkezinde (DC) düğümleriniz varsa veya karma bir ortamınız varsa, daha sonra her zaman açık yapılandırma bu ortamda kapalı kalma süresini en aza indirmek için kullanabilirsiniz.

Şirket içi veya ikincil Azure DC ve ardından üzerinden SQL Server Yük devretme için zaman uyumlu için her zaman açık eşitleme değiştirmek için bir yaklaşımdır. Ardından VHD'leri Premium depolama alanına kopyalanmaya ve makineyi yeni bir bulut hizmetinde yeniden dağıtın. Dinleyici güncelleştirin ve geri dönecek.

##### <a name="points-of-downtime"></a>Kapalı kalma süresi noktaları
Kapalı kalma süresi alternatif DC ve geri yük devretme için zaman oluşur. Ayrıca, istemci/DNS yapılandırmasına bağlıdır ve istemci yeniden bağlanmayı gecikebilir.
Aşağıdaki örnekte, her zaman açık bir karma yapılandırmasının göz önünde bulundurun:

![MultiSite1][9]

##### <a name="advantages"></a>Avantajları
* Mevcut altyapısını kullanabilir.
* Azure depolama, DR Azure DC üzerinde önceden yükseltmeniz seçeneğiniz vardır.
* DR Azure DC depolaması yeniden.
* Yük devretme sınaması işlemlerini hariç geçiş sırasında en az iki yük devretme işlemlerini yoktur.
* Yedekleme ile SQL Server verilerini taşımak ve geri yüklemek gerekmez.

##### <a name="disadvantages"></a>Olumsuz yönleri
* SQL Server istemci erişimi bağlı olarak olabilir daha yüksek gecikme süresi SQL Server uygulamaya alternatif bir DC'nin çalışırken.
* VHD'ler kopyalama zaman Premium Depolama'ya uzun olabilir. Bu kullanılabilirlik grubunda düğüm tutulup tutulmayacağını üzerinde kararınızı etkileyebilir. Bu birincil düğüm çoğaltılmamış işlemleri, işlem günlüğünde tutulacağı olduğundan günlük yoğun iş yükleri geçiş sırasında çalışan gerekli olduğunda için göz önünde bulundurun. Bu nedenle bu önemli oranda artma.
* Bu senaryo Azure kullanırsınız **başlangıç AzureStorageBlobCopy** zaman uyumsuz komutunu. Tamamlanma hiçbir SLA yoktur. Bu sıradaki bekleme bağlıdır sırada kopyaları süre değişir da bağımlı olan aktarmak için veri miktarı. Bu nedenle 2 veri merkezinizde yalnızca bir düğüme sahip, kopya testinde uzun sürdüğünde durumda azaltma adımlarını gerçekleştirmesi gereken. Bu azaltma adımlarını Aşağıdaki fikirler şunlardır:
  * Geçici bir 2 SQL düğüm HA için üzerinde anlaşılan kapalı kalma süresi ile geçişten önce ekleyin.
  * Azure zamanlanmış bakım dışında geçiş çalıştırın.
  * Küme çekirdeğini doğru yapılandırılmış olun.

Bu senaryo, yüklemenizi belgelenmiş ve en yüksek disk önbellek ayarlarını değişiklik yapmak için depolama nasıl eşlenen bilmeniz varsayar.

##### <a name="high-level-steps"></a>Üst düzey adımlar
![Multisite2][10]

* Şirket içi / Azure DC SQL Server birincil alternatif ve diğer otomatik yük devretme iş ortağı (AFP) olun.
* SQL2 disk yapılandırma bilgilerini toplamak ve düğüm kaldırmak (ekli VHD'ler silmeyin).
* Premium depolama hesabı oluşturma ve standart depolama hesabından VHD'ler kopyalayın.
* Yeni bir bulut hizmeti oluşturun ve kendi prim depolama diskleri ekli SQL2 VM oluşturun.
* ILB yapılandırma / ELB ve uç noktalar ekleyebilirsiniz.
* Yeni ILB ile her zaman üzerinde dinleyicisi güncelleştirme / ELB IP adresi ve test yük devretme.
* DNS yapılandırmasını denetleyin.
* AFP SQL2 için değiştirmek ve SQL1 geçirmek ve 2 – 5 adımlarını gidin.
* Test yük devretmeleri.
* SQL1 ve SQL2 AFP geçiş

## <a name="appendix-migrating-a-multisite-always-on-cluster-to-premium-storage"></a>Ek: bir çoklu site küme her zaman açık Premium depolama alanına geçirme
Bu makalenin sonraki bölümlerinde, her zaman açık bir çok siteli küme Premium depolama alanına dönüştürme ayrıntılı bir örnek sağlar. Bu ayrıca dinleyicisi bir dış yük dengeleyici (ELB) kullanarak bir iç yük dengeleyici (ILB) dönüştürür.

### <a name="environment"></a>Ortam
* Windows 2k12 / SQL 2k12
* SP 1 DB dosyaları
* Düğüm başına depolama havuzları x 2

![Appendix1][11]

### <a name="vm"></a>VM:
Bu örnekte, biz ILB için bir ELB taşıma göstermek için adımıdır. Bu geçiş sırasında ILB için geçiş yapma gösterecek şekilde ELB ILB önce mevcut değildi.

![Appendix2][12]

### <a name="pre-steps-connect-to-subscription"></a>Öncesi adımları: Aboneliğine bağlanma
    Add-AzureAccount

    #Set up subscription
    Get-AzureSubscription

#### <a name="step-1-create-new-storage-account-and-cloud-service"></a>1. adım: Yeni depolama hesabı oluşturun ve bulut hizmeti
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

#### <a name="step-2-increase-the-permitted-failures-on-resources-optional"></a>2. adım: izin verilen hataları kaynaklar üzerinde artırın <Optional>
Her zaman üzerinde kullanılabilirlik grubuna ait bazı kaynaklar üzerinde sınırlar vardır, Küme hizmetinin kaynak grubunu yeniden başlatmak için girişimde bulunduğu bir dönemde ortaya kaç hatalarda. Bu sınıra yakın alabilirsiniz makineler kapatma tarafından el ile yük devretme ve tetikleyici yük devretme veritabanınız yoksa bu yana bu yordamı taramasını adımında, bu artırmanız önerilir.

Yük Devretme Kümesi Yöneticisi'nde bunun için hata indirimi çift akıllıca her zaman açık kaynak grubunun özelliklerini gidin:

![Appendix3][13]

En fazla hata sayısı için 6 değiştirin.

#### <a name="step-3-addition-ip-address-resource-for-cluster-group-optional"></a>3. adım: Ek IP adresi kaynağı küme grubu için <Optional>
Küme grubu ve bu hizalanması için bulut alt ağ için yalnızca bir IP adresi varsa, küme ağ adı ve küme IP kaynağı değil sonra çevrimiçi olamıyor, yanlışlıkla tüm küme düğümleri bu ağ üzerinde bulutta çevrimdışı izlerseniz, kaybolacağını unutmayın. Bu durumda, güncelleştirmelerinin diğer küme kaynaklarını engeller.

#### <a name="step-4-dns-configuration"></a>4. adım: DNS yapılandırması
Sorunsuz bir geçiş uygulama bağlıdır nasıl DNS yüklenmekte olan kullanılan ve güncelleştirildi.
Her zaman açık yüklendiğinde, bir Windows Küme kaynak grubu oluşturur yük devretme kümesi Yöneticisi'ni açın, gördüğünüz isteğe bağlı olarak en az üç kaynaklara sahip, belgenin başvurduğu iki şunlardır:

* Sanal ağ adı (VNN) – her zaman açık aracılığıyla SQL sunuculara bağlanmak isteyen olduğunda istemcilerin bağlandıkları DNS adı.
* IP adresi kaynağı – IP adresi VNN ile ilişkili birden fazla olabilir ve çok siteli bir yapılandırma, her site/alt ağda bir IP adresine sahip.

Ne zaman SQL Server, SQL Server sürücüsü dinleyicisi ile ilişkili DNS kayıtları alır ve her zaman açık bağlanmaya istemcisi bağlanma IP adresi ilişkili. Ardından, bu etkileyebilir bazı etkenler tartışın.

Dinleyici adıyla ilişkili eşzamanlı DNS kayıtlarını sayısı yalnızca ilişkili IP adresleri sayısına bağlıdır, ancak ' de yük devretme kümelemesi için her zaman açık VNN kaynak RegisterAllIpProviders'setting.

Azure'da her zaman açık dağıttığınızda IP adreslerini ve dinleyicisi oluşturmak için farklı adımlar vardır, 1 'RegisterAllIpProviders' el ile yapılandırmanız gerekir, bu burada 1 olarak ayarlayın her zaman açık bir şirket içi dağıtımına farklı.

'RegisterAllIpProviders' 0 ise, daha sonra yalnızca bir DNS kaydı DNS'de dinleyicisi ile ilişkili görürsünüz:

![Appendix4][14]

'RegisterAllIpProviders' 1 ise:

![Appendix5][15]

Aşağıdaki kodu VNN ayarları dökümleri ve sizin için ayarlar. Bu değişikliğin etkili olması için VNN çevrimdışı duruma getirin ve tekrar çevrimiçi Aç gerekir. Bu dinleyiciyi sürer çevrimdışı neden olan istemci bağlantı kesintisi.

    ##Always On Listener Name
    $ListenerName = "Mylistener"
    ##Get AlwaysOn Network Name Settings
    Get-ClusterResource $ListenerName| Get-ClusterParameter
    ##Set RegisterAllProvidersIP
    Get-ClusterResource $ListenerName| Set-ClusterParameter RegisterAllProvidersIP  1

Sonraki geçiş adımda, her zaman açık dinleyici bir yük dengeleyici başvuran güncelleştirilmiş IP adresi ile güncelleştirmeniz gerekir, bu bir IP adresi kaynak temizleme ve ayrıca içerir. IP Güncelleştirme tamamlandıktan sonra yeni IP adresini DNS bölgesinde güncelleştirildi ve istemcilerin kendi yerel DNS önbelleği güncelleştirdiğiniz emin olmanız gerekir.

Ne olacağını DNS bölge aktarma hakkında geçiş sırasında uygulama yeniden gibi zamanı dikkate almanız gerekir, istemciler farklı ağında bulunan ve farklı bir DNS sunucusu başvuru, en az bölge aktarım süresini herhangi yeni IP tarafından kısıtlı Dinleyici için adresleri. Burada zaman sınırlaması altında olup olmadığını ele almaktadır ve Windows ekipleriniz ile bir artımlı bölge aktarımı zorlama test ve ayrıca DNS ana bilgisayar kaydı bir alt yaşam süresi (TTL için) put, böylece istemciler güncelleştirin. Daha fazla bilgi için bkz: [artımlı bölge aktarımlarının](https://technet.microsoft.com/library/cc958973.aspx) ve [başlangıç DnsServerZoneTransfer](https://technet.microsoft.com/library/jj649917.aspx).

Varsayılan olarak, azure'da her zaman açık dinleyici ile ilişkili DNS kaydının TTL 1200 saniyedir. İstemcilerin emin olmak için geçiş sırasında kısıtlaması kendi DNS güncelleştirilmiş IP adresiyle için dinleyici güncelleştirdiğinizde altında olması durumunda bu azaltmak isteyebilirsiniz. Bakın ve VNN yapılandırma döküm alma yapılandırmasını değiştirin:

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"
    #Look at HostRecordTTL
    Get-ClusterResource $ListenerName| Get-ClusterParameter

    #Set HostRecordTTL Examples
    Get-ClusterResource $ListenerName| Set-ClusterParameter -Name "HostRecordTTL" 120

> [!NOTE]
> Düşük 'HostRecordTTL', daha yüksek bir DNS trafik miktarı oluşur.

##### <a name="client-application-settings"></a>İstemci uygulama ayarları
SQL istemci uygulamanız .net 4.5 destekliyorsa SQLClient sonra kullanabileceğiniz ' MULTISUBNETFAILOVER = TRUE' anahtar sözcüğü. Daha hızlı bağlantı SQL her zaman üzerindeki kullanılabilirlik grubu için yük devretme sırasında sağladığından, bu anahtar sözcük uygulanmalıdır. Her zaman açık dinleyici paralel ile ilişkili tüm IP adreslerini aracılığıyla numaralandırır ve yük devretme sırasında daha agresif bir TCP bağlantı yeniden deneme hızı gerçekleştirir.

Önceki ayarları hakkında daha fazla bilgi için bkz: [MultiSubnetFailover anahtar sözcüğü ve ilişkili özellikleri](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover). Ayrıca bkz. [SqlClient yüksek kullanılabilirlik, olağanüstü durum kurtarma desteği](https://msdn.microsoft.com/library/hh205662\(v=vs.110\).aspx).

#### <a name="step-5-cluster-quorum-settings"></a>5. adım: Küme çekirdek ayarlarını
Aynı anda en az bir SQL Server çıkışı sürüyor olacak şekilde iki düğüm ile dosya paylaşımı tanığı (FSW) kullanıyorsanız, küme çekirdek ayarı değiştirmelisiniz, düğüm çoğunluğu izin vermek ve dinamik oy kullanmak için çekirdek ayarlamanız gerekir , tek bir düğüm durumu kalmasını sağlar.

    Set-ClusterQuorum -NodeMajority  

Küme çekirdeği yapılandırma ve yönetme hakkında daha fazla bilgi için bkz: [yapılandırmak ve bir Windows Server 2012 yük devretme kümesinde çekirdeği yönetmek](https://technet.microsoft.com/library/jj612870.aspx).

#### <a name="step-6-extract-existing-endpoints-and-acls"></a>6. adım: Mevcut uç noktalar ACL ayıklayıp
    #GET Endpoint info
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureEndpoint
    #GET ACL Rules for Each EP, this example is for the Always On Endpoint
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureAclConfig -EndpointName "myAOEndPoint-LB"  

Bu metin bir dosyaya kaydedin.

#### <a name="step-7-change-failover-partners-and-replication-modes"></a>7. adım: Yük devretme iş ortakları ve çoğaltma modları değiştirme
İkiden fazla SQL sunucuları varsa, başka bir DC ya da şirket içi başka bir ikincil yük devretme 'zaman 'uyumlu değiştirme ve bir otomatik yük devretme iş ortağı (AFP) olun, değişiklik yapmadan adımında HA korumak için budur. Bunu TSQL yapabilirsiniz ancak SSMS değiştirin:

![Appendix6][16]

#### <a name="step-8-remove-secondary-vm-from-cloud-service"></a>8. adım: İkincil VM bulut hizmetinden kaldırın.
İlk bulut ikincil düğüme geçirilmesi için planlama. Bu düğümü şu anda birincil ise, el ile bir yük devretme başlatın.

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

#### <a name="step-9-change-disk-caching-settings-in-csv-file-and-save"></a>9. adım: önbelleğe alma ayarları CSV dosyasındaki disk değiştirin ve kaydedin
Veri birimleri için bu READONLY ayarlanmalıdır.

TLOG birimler için bunların hiçbiri olarak ayarlanması gerekir.

![Appendix7][17]

#### <a name="step-10-copy-vhds"></a>10. adım: Kopyalama VHD'leri
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



Premium depolama hesabı VHD'ler kopya durumunu denetleyebilirsiniz:

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

![Appendix8][18]

Bunların tümü başarılı kaydedilir kadar bekleyin.

Tek tek bloblar için daha fazla bilgi için:

    Get-AzureStorageBlobCopyState -Blob "blobname.vhd" -Container $containerName -Context $xioContext

#### <a name="step-11-register-os-disk"></a>11. adım: Kayıt işletim sistemi diski
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

#### <a name="step-12-import-secondary-into-new-cloud-service"></a>12. adım: yeni bulut hizmetinde ikincil alma
Aşağıdaki kodu aynı zamanda eklenen seçeneğini burada kullanan makine aktarıp retainable VIP kullanabilirsiniz.

    #Build VM Config
    $ipaddr = "192.168.0.5"
    #Remember to change to XIO
    $newInstanceSize = "Standard_DS13"
    $subnet = "SQL"

    #Create new Avaiability Set
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

#### <a name="step-13-create-ilb-on-new-cloud-svc-add-load-balanced-endpoints-and-acls"></a>13. adım: Yük eklemek ILB yeni bulut Svc üzerinde oluşturma dengeli uç noktaları ve ACL'leri
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

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

    ####WAIT FOR FULL AlwaysOn RESYNCRONISATION!!!!!!!!!#####

#### <a name="step-14-update-always-on"></a>14. adım: Her zaman üzerinde güncelleştir
    #Code to be executed on a Cluster Node
    $ClusterNetworkNameAmsterdam = "Cluster Network 2" # the azure cluster subnet network name
    $newCloudServiceIPAmsterdam = "192.168.0.25" # IP address of your cloud service

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"


    Add-ClusterResource "IP Address $newCloudServiceIPAmsterdam" -ResourceType "IP Address" -Group $AGName -ErrorAction Stop |  Set-ClusterParameter -Multiple @{"Address"="$newCloudServiceIPAmsterdam";"ProbePort"="59999";SubnetMask="255.255.255.255";"Network"=$ClusterNetworkNameAmsterdam;"OverrideAddressMatch"=1;"EnableDhcp"=0} -ErrorAction Stop

    #set dependancy and NETBIOS, then remove old IP address

    #set NETBIOS, then remove old IP address
    Get-ClusterGroup $AGName | Get-ClusterResource -Name "IP Address $newCloudServiceIPAmsterdam" | Set-ClusterParameter -Name EnableNetBIOS -Value 0

    #set dependency to Listener (OR Dependency) and delete previous IP Address resource that references:

    #Make sure no static records in DNS

![Appendix9][19]

Artık eski bulut hizmeti IP adresini kaldırın.

![Appendix10][20]

#### <a name="step-15-dns-update-check"></a>15. adım: DNS güncelleştirme denetimi
Şimdi DNS sunucuları, SQL Server istemci ağlarda denetimi ve kümeleme eklenen IP adresi için ek ana bilgisayar kaydı ekledi emin olun. Bu DNS sunucuları güncelleştirilmemiş, DNS bölge aktarımı zorlama göz önünde bulundurun ve istemcilerin emin var. alt hem her zaman üzerinde IP adreslerini çözümleyemedi, otomatik DNS çoğaltmanın tamamlanmasını bekleyin gerek yoktur bu.

#### <a name="step-16-reconfigure-always-on"></a>16. adım: Her zaman yeniden yapılandırın
Bu noktada, tam olarak şirket içi düğümle yeniden eşitleyin ve zaman uyumlu çoğaltma düğüme geçiş ve AFP yapmak için geçirilen bu düğüm için ikincil bekleyin.  

#### <a name="step-17-migrate-second-node"></a>17. adım: ikinci düğümü geçirme
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

#### <a name="step-18-change-disk-caching-settings-in-csv-file-and-save"></a>Adım 18: önbelleğe alma ayarları CSV dosyasındaki disk değiştirin ve kaydedin
Veri birimleri için önbellek ayarları READONLY ayarlamanız gerekir.

TLOG birimler için önbellek ayarlarını NONE olarak ayarlanmalıdır.

![Appendix11][21]

#### <a name="step-19-create-new-independent-storage-account-for-secondary-node"></a>19. adım: İkincil düğüm için yeni bağımsız depolama hesabı oluşturma
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

#### <a name="step-20-copy-vhds"></a>20. adım: Kopyalama VHD'leri
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

    #check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContext


Tüm VHD'leri için VHD kopya durumunu denetleyebilirsiniz: ForEach ($disk $diskobjects içinde) {$lun $disk =. LUN $vhdname $disk.vhdname $cacheoption = $disk =. HostCaching $disklabel $disk =. Disketiketi $diskName $disk =. DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContextnode2
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix12][22]

Bunların tümü başarılı kaydedilir kadar bekleyin.

Tek tek bloblar için daha fazla bilgi için:

    #Check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContextnode2

#### <a name="step-21-register-os-disk"></a>21. adım: Kayıt işletim sistemi diski
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

    #Join to existing Avaiability Set

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

#### <a name="step-22-add-load-balanced-endpoints-and-acls"></a>22. adım: Yük ekleme dengeli uç noktaları ve ACL'leri
    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM


    #STOP!!! CHECK in the Azure portal or Machine Endpoints through PowerShell that these Endpoints are created!

    #SET ACLs or Azure Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

#### <a name="step-23-test-failover"></a>23. adım: Yük devretme sınaması
Geçirilen her zaman açık şirket içi düğümle eşitlemeniz düğüme bekleyin. Zaman uyumlu çoğaltma moduna yerleştirmek ve onu eşitlenene kadar bekleyin. Ardından AFP olduğu bir şirket içi yük devretmeyi ilk düğümü geçirildi. Çalıştıktan sonra son geçirilen düğüm AFP değiştirin.

Test yük devretmeleri tüm düğümler arasında ve olarak yük devretme iş emin olmak için chaos testleri bekleniyordu ancak ve zamanında manor çalıştırmanız gerekir.

#### <a name="step-24-put-back-cluster-quorum-settings--dns-ttl--failover-pntrs--sync-settings"></a>24. adım: geri küme çekirdek ayarlarını Put / DNS TTL / yük devretme Pntrs / eşitleme ayarları
##### <a name="adding-ip-address-resource-on-same-subnet"></a>Aynı alt ağdaki IP adresi kaynağı ekleme
Yalnızca iki SQL sunucusu ve yeni bir bulut hizmeti geçirmek istediğiniz ancak aynı alt ağda tutmak istediğiniz özgün her zaman üzerinde IP adresini silin ve yeni IP adresi eklemek için dinleyici çevrimdışı duruma getirmeden engelleyebilirsiniz. Başka bir alt ağ için sanal makineleri geçiriyorsanız, bu alt ağ başvuruda bulunan bir ek küme ağı olduğundan Bunu yapmak gerekmez.

Geçirilen ikincil getirildi ve yeni IP adresi kaynak yük devretme varolan birincil önce yeni bulut hizmeti için eklenen sonra bu küme Yük Devretme Yöneticisi içinden adımları izlemesi gerekir:

IP adresi eklemek için bkz: [ek](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage), 14. adım.

1. Geçerli IP adresi kaynağı için 'Varolan birincil SQL Server' için olası sahip örnekte, 'dansqlams4' değiştirin:

    ![Appendix13][23]
2. Yeni IP adresi kaynağı için 'Geçirildi ikincil SQL Server', 'dansqlams5' örnekte olası sahip değiştirin:

    ![Appendix14][24]
3. Son düğümü geçirildiğinde bu düğümü olası sahip olarak eklenir böylece olası sahipler düzenlenmesi gerekir ve bu ayarlandıktan sonra Yük devretme kullanabilirsiniz:

    ![Appendix15][25]

## <a name="additional-resources"></a>Ek kaynaklar
* [Azure Premium depolama](../premium-storage.md)
* [Sanal Makineler](https://azure.microsoft.com/services/virtual-machines/)
* [Azure sanal makinelerde SQL Server](../sql/virtual-machines-windows-sql-server-iaas-overview.md)

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
