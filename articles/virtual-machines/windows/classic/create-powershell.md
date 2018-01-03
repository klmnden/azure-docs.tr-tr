---
title: "PowerShell ile bir Windows VM oluşturma | Microsoft Docs"
description: "Azure PowerShell ve klasik dağıtım modeli kullanarak Windows sanal makine oluşturun."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 42c0d4be-573c-45d1-b9b0-9327537702f7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: af672a873b33cbd0b6151fd564e84c96f0b6e1e3
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="create-a-windows-virtual-machine-with-powershell-and-the-classic-deployment-model"></a>Klasik dağıtım modeli ve PowerShell ile bir Windows sanal makine oluşturma
> [!div class="op_single_selector"]
> * [Azure portal - Windows](tutorial.md)
> * [PowerShell - Windows](create-powershell.md)
> 
> 

<br>

> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. [Bu adımları Resource Manager modeli kullanarak gerçekleştirmeyi](../../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) öğrenin.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Bu adımlar oluşturmak ve bir yapı bloğu yaklaşımını kullanarak Windows tabanlı bir Azure sanal makine önceden Azure PowerShell komut kümesini özelleştirmek nasıl gösterir. Bu işlem, hızlı bir şekilde yeni bir Windows tabanlı sanal makine için bir komut kümesi oluşturmak ve var olan bir dağıtıma genişletin veya hızlı bir şekilde bir özel geliştirme ve test veya BT Uzmanı ortamı oluşturmak birden çok komut kümesi oluşturmak için kullanabilirsiniz.

Bir doldurma-arada boşlukları yaklaşım Azure PowerShell komut kümelerini oluşturmak için aşağıdaki adımları izleyin. Bu yaklaşım, PowerShell için yeni olan veya yalnızca başarılı yapılandırmayı belirtmek için hangi değerlerin bilmek istiyorsanız yararlı olabilir. Gelişmiş PowerShell kullanıcılar, komutları almak ve ("$" ile başlayan satırlar) değişkenleri için kendi değerlerinizi yerleştirin.

Henüz yapmadıysanız, konusundaki yönergeleri kullanın [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview) yerel bilgisayarınıza Azure PowerShell'i yüklemek için. Sonra bir Windows PowerShell komut istemi açın.

## <a name="step-1-add-your-account"></a>1. adım: hesabınızı ekleme
1. PowerShell komut isteminde yazın **Add-AzureAccount** tıklatıp **Enter**. 
2. Azure aboneliğinizle ilişkili e-posta adresini yazın ve tıklatın **devam**. 
3. Hesabınızın parolasını yazın. 
4. Tıklatın **oturum**.

## <a name="step-2-set-your-subscription-and-storage-account"></a>2. adım: aboneliğiniz ve depolama hesabı ayarlama
Azure aboneliği ve depolama hesabı Windows PowerShell komut isteminde şu komutları çalıştırarak ayarlayın. Dahil olmak üzere tırnak işaretleri içindeki her şeyi değiştirin < ve > karakterleri doğru adlara sahip.

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

Doğru abonelik adını çıktısını varlığıyla SubscriptionName özelliğinden alabilirsiniz **Get-AzureSubscription** komutu. Doğru depolama hesabı adı çıktısını etiket özelliğinden alabilirsiniz **Get-AzureStorageAccount** çalıştırdıktan sonra komut **Select-AzureSubscription** komutu.

## <a name="step-3-determine-the-imagefamily"></a>3. adım: ImageFamily belirleme
Ardından, oluşturmak istediğiniz Azure sanal makinesi için karşılık gelen belirli görüntü ImageFamily veya etiket değeri belirlemeniz gerekir. Bu komutla kullanılabilir ImageFamily değerler listesini elde edebilirsiniz.

    Get-AzureVMImage | select ImageFamily -Unique

Windows tabanlı bilgisayarlar için ImageFamily değeri bazı örnekleri şunlardır:

* Windows Server 2012 R2 Datacenter
* Windows Server 2008 R2 SP1
* Windows Server 2016 Technical Preview 4
* Windows Server 2012'de SQL Server 2012 SP1 Enterprise

Aradığınız görüntü bulursanız, metin düzenleyici tercih ettiğiniz veya PowerShell Tümleşik komut dosyası ortamı (ISE) yeni bir örneğini açın. Aşağıdaki yeni metin dosyası veya ImageFamily değerini değiştirerek PowerShell ISE kopyalayın.

    $family="<ImageFamily value>"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

Bazı durumlarda, etiket özelliği ImageFamily değeri yerine görüntü adı kullanılıyor. ImageFamily özelliği kullanılarak için aradığınız görüntü bulamadıysanız, bu komutla kendi etiket özelliği tarafından görüntüleri listeleyin.

    Get-AzureVMImage | select Label -Unique

Bu komutla sağ görüntü bulursanız, tercih ettiğiniz veya PowerShell ISE Metin Düzenleyicisi'ni yeni bir örneğini açın. Aşağıdaki yeni metin dosyası veya etiket değeri değiştirerek PowerShell ISE kopyalayın.

    $label="<Label value>"
    $image = Get-AzureVMImage | where { $_.Label -eq $label } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

## <a name="step-4-build-your-command-set"></a>4. adım: komut kümesi oluşturma
Yeni metin dosyanızı veya işe blokları uygun kümesini kopyalama ve daha sonra değişken değerleri doldurma veya kaldırarak ayarlamak komutunuzu kalan yapı < ve > karakterleri. İki bkz [örnekler](#examples) son sonucunun hakkında bir fikir için bu makalenin sonundaki.

(Gerekli) bu iki komut blokları birini seçerek, komutunu başlatın.

Seçenek 1: bir sanal makine adı ve bir boyut belirtin.

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

Seçenek 2: bir ad, boyutu ve kullanılabilirlik kümesi adı belirtin.

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $availset="<set name>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image -AvailabilitySetName $availset

D, DS veya G-serisi sanal makineler için InstanceSize değerleri için bkz: [sanal makine ve bulut hizmeti boyutları Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).

> [!NOTE]
> Yazılım Güvencesi ile bir Kurumsal Anlaşma varsa ve Windows Server yararlanmak düşündüğünüz [karma kullanma avantajı](https://azure.microsoft.com/pricing/hybrid-use-benefit/), ekleme **- LicenseType değeri** parametresi **yeni AzureVMConfig** değere geçirme cmdlet'ini **Windows_Server** için tipik kullanım örneği.  Görüntüyü karşıya yüklediğiniz kullandığınızdan emin olun; Standart bir görüntü ile karma kullanma avantajını Galeriden kullanamazsınız.
> 
> 

İsteğe bağlı olarak, bir tek başına Windows bilgisayar için yerel yönetici hesabı ve parolayı belirtin.

    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.Username -Password $cred.GetNetworkCredential().Password

Güçlü bir parola seçin. Kendi gücünü denetlemek için bkz: [parola denetleyicisi: güçlü parolalar kullanarak](https://www.microsoft.com/security/pc-security/password-checker.aspx).

İsteğe bağlı olarak, Windows bilgisayarı mevcut bir Active Directory etki alanına eklemek için yerel yönetici hesabı parolası, etki alanı ve adı ve etki alanı hesabının parolasını belirtin.

    $cred1=Get-Credential –Message "Type the name and password of the local administrator account."
    $cred2=Get-Credential –Message "Now type the name (not including the domain) and password of an account that has permission to add the machine to the domain."
    $domaindns="<FQDN of the domain that the machine is joining>"
    $domacctdomain="<domain of the account that has permission to add the machine to the domain>"
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $cred2.Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domaindns

Windows tabanlı sanal makinelere ilişkin ek öncesi yapılandırma seçenekleri için sözdizimi için bkz: **Windows** ve **WindowsDomain** parametre kümelerine [Ekle AzureProvisioningConfig](/powershell/module/azure/add-azureprovisioningconfig).

İsteğe bağlı olarak, sanal makine statik DIP bilinen belirli bir IP adresi atayın.

    $vm1 | Set-AzureStaticVNetIP -IPAddress <IP address>

Belirli bir IP adresi ile kullanılabilir olduğunu doğrulayabilirsiniz:

    Test-AzureStaticVNetIP –VNetName <VNet name> –IPAddress <IP address>

İsteğe bağlı olarak, sanal makine bir Azure sanal ağında belirli bir alt atayın.

    $vm1 | Set-AzureSubnet -SubnetNames "<name of the subnet>"

İsteğe bağlı olarak, bir tek veri diski sanal makineye ekleyin.

    $disksize=<size of the disk in GB>
    $disklabel="<the label on the disk>"
    $lun=<Logical Unit Number (LUN) of the disk>
    $hcaching="<Specify one: ReadOnly, ReadWrite, None>"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

Bir Active Directory etki alanı denetleyicisi için "None" $hcaching ayarlayın.

İsteğe bağlı olarak, sanal makineyi dış trafiği için mevcut bir yük dengeli küme ekleyin.

    $protocol="<Specify one: tcp, udp>"
    $localport=<port number of the internal port>
    $pubport=<port number of the external port>
    $endpointname="<name of the endpoint>"
    $lbsetname="<name of the existing load-balanced set>"
    $probeprotocol="<Specify one: tcp, http>"
    $probeport=<TCP or HTTP port number of probe traffic>
    $probepath="<URL path for probe traffic>"
    $vm1 | Add-AzureEndpoint -Name $endpointname -Protocol $protocol -LocalPort $localport -PublicPort $pubport -LBSetName $lbsetname -ProbeProtocol $probeprotocol -ProbePort $probeport -ProbePath $probepath

Son olarak, sanal makine oluşturmak için bu gerekli komutu blokları birini seçin.

Seçenek 1: var olan bir bulut hizmetinde sanal makine oluşturun.

    New-AzureVM –ServiceName "<short name of the cloud service>" -VMs $vm1

Bulut hizmetinin kısa ad listesinde bulut Hizmetleri Azure portalında veya Azure portalındaki kaynak gruplarının listesinde görüntülenen addır.

Seçenek 2: sanal makineyi bir bulut hizmetini ve sanal ağ oluşturun.

    $svcname="<short name of the cloud service>"
    $vnetname="<name of the virtual network>"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

## <a name="step-5-run-your-command-set"></a>5. adım: komut kümesini çalıştırın
Metin Düzenleyicisi veya birden çok adım 4 komutlarından bloklarını oluşan PowerShell ISE oluşturulan Azure PowerShell komut kümesini gözden geçirin. Gerekli tüm değişkenleri belirttiniz ve doğru değerlerin olduğundan emin olun. Ayrıca tüm kaldırdınız emin olun < ve > karakterleri.

Bir metin düzenleyicisi kullanarak, komut kümesini panoya kopyalayın ve sonra Windows PowerShell komut istemini açın sağ tıklatın. Bu komut kümesi PowerShell komutlarını bir dizi olarak vermek ve Azure sanal makine oluşturun. Alternatif olarak, PowerShell ISE set komutunu çalıştırın.

Bu sanal makineyi yeniden veya benzer bir oluşturacaksanız, şunları yapabilirsiniz:

* Bu komut bir PowerShell komut dosyası (*.ps1) ayarlanmış kaydedin.
* Bu komutu bir Azure Otomasyonu runbook'u yap kaydetme **Automation hesapları** Azure portalının bölümü.

## <a id="examples"></a>Örnekler
Aşağıda, Windows tabanlı Azure sanal makineler oluşturma Azure PowerShell komut kümelerini oluşturmak için yukarıdaki adımları kullanarak, iki örnek verilmiştir.

### <a name="example-1"></a>Örnek 1
Bir PowerShell gereksinim Ayarla komutu bir Active Directory etki alanı denetleyicisi için ilk sanal makine oluşturun:

* Windows Server 2012 R2 Datacenter görüntüsü kullanır.
* AZDC1 adına sahip.
* Bir tek başına bilgisayardır.
* Bir ek veri diski 20 GB sahiptir.
* Statik IP adresi 192.168.244.4 vardır.
* AZDatacenter sanal ağ arka uç alt ağda değil.
* Azure TailspinToys bulut hizmetidir.

Okunabilirlik için her bloğu arasında boş satırlar ile bu sanal makine oluşturmak için karşılık gelen Azure PowerShell komutunu aşağıda verilmiştir.

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="AZDC1"
    $vmsize="Medium"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.Username -Password $cred.GetNetworkCredential().Password

    $vm1 | Set-AzureSubnet -SubnetNames "BackEnd"

    $vm1 | Set-AzureStaticVNetIP -IPAddress 192.168.244.4

    $disksize=20
    $disklabel="DCData"
    $lun=0
    $hcaching="None"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

### <a name="example-2"></a>Örnek 2
Bir PowerShell gereksinim Ayarla komutu iş sunucu için bir sanal makine oluşturun:

* Windows Server 2012 R2 Datacenter görüntüsü kullanır.
* LOB1 adına sahip.
* Corp.contoso.com etki alanının bir üyesidir.
* Bir ek veri diski 200 GB sahiptir.
* AZDatacenter sanal ağ ön uç alt ağda değil.
* Azure TailspinToys bulut hizmetidir.

Bu sanal makine oluşturmak için karşılık gelen Azure PowerShell komutunu aşağıda verilmiştir.

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="LOB1"
    $vmsize="Large"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred1=Get-Credential –Message "Type the name and password of the local administrator account."
    $cred2=Get-Credential –Message "Now type the name (not including the domain) and password of an account that has permission to add the machine to the domain."
    $domaindns="corp.contoso.com"
    $domacctdomain="CORP"
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $cred2.Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "FrontEnd"

    $disksize=200
    $disklabel="LOBData"
    $lun=0
    $hcaching="ReadWrite"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname


## <a name="next-steps"></a>Sonraki adımlar
127 GB'den büyük bir işletim sistemi diski gerekiyorsa, yapabilecekleriniz [OS sürücüyü genişletin](../../virtual-machines-windows-expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

