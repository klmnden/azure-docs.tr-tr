---
title: "Hızlandırılmış ağ ile bir Azure sanal makine oluşturma | Microsoft Docs"
description: "Hızlandırılmış ağ ile bir sanal makine oluşturmayı öğrenin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/10/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2e887230a102f5c6289ca2eec0e4700a0e1fdfde
ms.sourcegitcommit: 54fd091c82a71fbc663b2220b27bc0b691a39b5b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2017
---
# <a name="create-a-virtual-machine-with-accelerated-networking"></a>Hızlandırılmış ağ ile bir sanal makine oluşturun

Bu öğreticide, bir Azure sanal makine (VM) ağ hızlandırılmış oluşturmayı öğrenin. Windows için ve belirli Linux dağıtımları için genel Önizleme GA hızlandırılmış ağdır. Hızlandırılmış ağ önemli ölçüde ağ performansını iyileştirme, bir VM tek köklü g/ç Sanallaştırması (SR-IOV) sağlar. Bu yüksek performanslı yolu gecikme, değişim ve desteklenen VM türlerinde zorlu ağ iş yükleri ile kullanmak için CPU kullanımını azaltma datapath konaktan atlar. Aşağıdaki resimde, ile ve hızlandırılmış ağ olmadan iki sanal makine (VM) arasındaki iletişimi gösterir:

![Karşılaştırma](./media/virtual-network-create-vm-accelerated-networking/image1.png)

Hızlandırılmış ağ desteği olmadan, VM ve dışındaki tüm ağ trafiğini konak ve sanal anahtar çapraz geçiş gerekir. Sanal anahtar ağ güvenlik grupları, erişim denetim listeleri, yalıtım ve diğer ağ trafiği sanallaştırılmış Ağ Hizmetleri gibi tüm ilke zorunluluğu sağlar. Sanal anahtarları hakkında daha fazla bilgi için okuma [Hyper-V ağ sanallaştırma ve sanal anahtar](https://technet.microsoft.com/library/jj945275.aspx) makalesi.

Hızlandırılmış ağ ile ağ trafiğini VM ağ arabiriminin (NIC) ulaştığında ve ardından VM iletilir. Sanal anahtar olmadan hızlandırılmış ağ uygulanan tüm ağ ilkeleri Boşaltılan ve donanım uygulanır. Donanım ilkesinde uygulama konak ve sanal anahtarın konak uygulanan tüm ilke korurken atlayarak doğrudan VM'ye iletme ağ trafiğini NIC'ye sağlar.

Hızlandırılmış ağ yararları yalnızca üzerinde etkin VM için de geçerlidir. En iyi sonuçlar için en az iki VM aynı Azure sanal ağa (VNet) bağlı bu özelliği etkinleştirmek idealdir. Sanal ağlar veya içi bağlantı üzerinden iletişim kurarken, bu özellik genel gecikme en az bir etkisi yoktur.

> [!WARNING]
> Genel kullanılabilirlik özellikleri serbest olarak bu Linux genel Önizleme aynı düzeyde kullanılabilirlik ve güvenilirlik olmayabilir. Özellik desteklenmiyor, yetenekleri kısıtlı ve tüm Azure konumlarda kullanılamayabilir. Bu özellik durumunu ve kullanılabilirliğini'daki en güncel bildirimleri için Azure sanal ağ güncelleştirmeleri sayfasında denetleyin.

## <a name="benefits"></a>Avantajlar
* **Düşük gecikme / saniye başına daha yüksek paket (pps):** datapath sanal anahtarı kaldırmak ana bilgisayar ilke işleme için paketler harcadığı zamanı kaldırır ve VM içinde işlenebilecek paketlerin sayısını artırır.
* **Değişim azaltılmış:** sanal anahtar işleme uygulanması gereken ilke miktarını ve işlem yaptığını CPU iş yüküne bağlıdır. İlke zorlama donanım yük boşaltma, VM iletişim ve tüm yazılım kesmelerini ve İçerik Geçişi konağa kaldırma doğrudan VM paketleri sunarak bu değişkenlik kaldırır.
* **Düşük CPU kullanımı:** konakta sanal anahtar atlayarak müşteri adayları ağ trafiğini işlemek için daha az CPU kullanımı için.

## <a name="Limitations"></a>Sınırlamaları
Bu özelliği kullanırken aşağıdaki sınırlamalar bulunmaktadır:

* **Ağ arabirimi oluşturma:** hızlandırılmış ağ yalnızca yeni bir NIC için etkinleştirilmesi Varolan bir NIC için etkinleştirilemez
* **VM oluşturma:** etkin hızlandırılmış ağ ile bir NIC yalnızca eklenebilir bir VM VM oluşturulduğunda. NIC için mevcut bir VM'yi eklenemiyor.
* **Bölgeler:** Windows VM hızlandırılmış ağ iletişimi ile birlikte, çoğu Azure bölgelerde sunulur. Linux VM'ler hızlandırılmış ağ ile birden çok bölgede sunulur. Bu özellik kullanılabilir bölgeleri genişletme. Aşağıda Azure sanal ağı güncelleştirmeleri blog en son bilgiler için bkz.   
* **Desteklenen işletim sistemleri:** Windows: Microsoft Windows Server 2012 R2 Datacenter ve Windows Server 2016. Linux: Ubuntu Server 16.04 LTS çekirdek 4.4.0-77 veya sonrası, SLES 12 SP2, RHEL 7.3 ve CentOS 7.3 (yayımlanmış "Standart dışı Wave yazılım").
* **VM boyutu:** genel amaçlı ve en az sekiz çekirdeği ile işlem iyileştirilmiş örnek boyutları. Daha fazla bilgi için bkz: [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM boyutları makaleleri. Desteklenen VM örneği boyutlarının kümesi gelecekte genişletilir.
* **Dağıtım Azure Resource Manager (ARM) aracılığıyla yalnızca:** hızlandırılmış ağ ASM/RDFE aracılığıyla dağıtımı için kullanılabilir değil.

Bu sınırlamalara değişiklikler duyurdu aracılığıyla [Azure sanal ağı güncelleştirir](https://azure.microsoft.com/updates/accelerated-networking-in-expanded-preview/) sayfası.

## <a name="create-a-windows-vm"></a>Windows VM oluşturma
Azure portalında veya Azure kullanabilirsiniz [PowerShell](#windows-powershell) VM'yi oluşturmak için.

### <a name="windows-portal"></a>Portal

1. Bir Internet tarayıcısından Azure açın [portal](https://portal.azure.com) ve Azure ile oturum [hesap](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Zaten bir hesabınız yoksa, için kaydolabilirsiniz bir [ücretsiz deneme sürümü](https://azure.microsoft.com/offers/ms-azr-0044p).
2. Portalı'nda tıklatın **+ yeni** > **işlem** > **Windows Server 2016 Datacenter**. 
3. İçinde **Windows Server 2016 Datacenter** görünür, dikey bırakın *Resource Manager* altında seçilen **dağıtım modeli seçin**, tıklatıp **oluşturma**.
4. İçinde **Temelleri** görünür, dikey aşağıdaki değerleri girin, geri kalan varsayılan seçenekleri bırakın veya seçin ya da kendi değerlerinizi girin ve tıklatın **Tamam** düğmesi:

    |Ayar|Değer|
    |---|---|
    |Ad|MyVm|
    |Kaynak grubu|Bırakın **Yeni Oluştur** seçili ve girin *MyResourceGroup*|
    |Konum|Batı ABD 2|

    Hakkında daha fazla bilgi için Azure yeniyseniz, [kaynak grupları](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [abonelikleri](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), ve [konumları](https://azure.microsoft.com/regions) (hangi de bilinir bölgeleri olarak).
5. İçinde **bir boyutu seçin** görünür, dikey girin *8* içinde **Minimum çekirdek** kutusuna ve ardından **tüm görüntüle**.
6. Tıklatın **DS4_V2 standart**, veya desteklenen herhangi bir VM, ardından **seçin** düğmesi.
7. İçinde **ayarları** görünür, dikey olarak tüm ayarları bırakın-, tıklatın dışında olduğu **etkin** altında **ağ hızlandırılmış**, ardından **Tamam** düğmesi. **Not:** önceki adımlarda, VM boyutu, işletim sistemi veya içinde listelenmeyen konum değerleri seçtiyseniz, [sınırlamalar](#Limitations) bu makalenin bölümüne **ağ hızlandırılmış** görünür değil.
8. İçinde **Özet** görünür, dikey tıklayın **Tamam** düğmesi. Azure VM oluşturma başlatır. VM oluşturmak birkaç dakika sürer.
9. Windows için hızlandırılmış ağ sürücüsü yüklemek için adımları tamamlamanız [Windows'u yapılandırma](#configure-windows) bu makalenin.

### <a name="windows-powershell"></a>PowerShell
1. Azure PowerShell'in en son sürümünü yüklemek [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modülü. Azure PowerShell yeniyseniz, okuma [Azure PowerShell genel bakış](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) makalesi.
2. Windows Başlat düğmesine tıklayarak bir PowerShell oturumu Başlat yazarak **powershell**, ardından **PowerShell** Arama sonuçlarından.
3. PowerShell pencerenize girin `login-azurermaccount` oturum Azure imzalamak için komutu [hesap](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Zaten bir hesabınız yoksa, için kaydolabilirsiniz bir [ücretsiz deneme sürümü](https://azure.microsoft.com/offers/ms-azr-0044p).
4. Tarayıcınızda, aşağıdaki komutu kopyalayın:
    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
      -Name $RgName `
      -Location $Location
    
    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
      -Name MySubnet `
      -AddressPrefix 10.0.0.0/24
    
    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName $RgName `
      -Location $Location `
      -Name MyVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet

    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
      -Name MyPublicIp `
      -ResourceGroupName $RgName `
      -Location $Location `
      -AllocationMethod Static

    # Create a virtual network interface and associate the public IP address to it
    $Nic = New-AzureRmNetworkInterface `
      -Name MyNic `
      -ResourceGroupName $RgName `
      -Location $Location `
      -SubnetId $Vnet.Subnets[0].Id `
      -PublicIpAddressId $Pip.Id `
      -EnableAcceleratedNetworking
     
    # Define a credential object for the VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential

    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
      -VMName MyVM -VMSize Standard_DS4_v2 | `
      Set-AzureRmVMOperatingSystem `
      -Windows `
      -ComputerName myVM `
      -Credential $Cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName MicrosoftWindowsServer `
      -Offer WindowsServer `
      -Skus 2016-Datacenter `
      -Version latest | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id 

    # Create the virtual machine.    
    New-AzureRmVM `
      -ResourceGroupName $RgName `
      -Location $Location `
      -VM $VmConfig
    #
    ```
5. PowerShell pencerenize betiğini yapıştırın ve bunu çalıştırmaya başlamak için sağ tıklatın. Bir kullanıcı adı ve parolası istenir. VM için sonraki adımda bağlanırken oturum açmak için bu kimlik bilgileri kullanın. Komut başarısız olursa ve değerleri komut yürütülmeden önce değiştirilen VM boyutu, işletim sistemi için kullanılan değerleri onaylayın ve konum içinde listelenen [sınırlamalar](#Limitations) bu makalenin.
6. Windows için hızlandırılmış ağ sürücüsü yüklemek için adımları tamamlamanız [Windows'u yapılandırma](#configure-windows) bu makalenin.

### <a name="configure-windows"></a>Windows yapılandırma
Azure'da VM oluşturduktan sonra Windows için hızlandırılmış ağ sürücüsü yüklemeniz gerekir. Aşağıdaki adımları gerçekleştirmeden önce bir Windows VM ile hızlandırılmış ağ kullanarak oluşturduğunuz gerekir [portal](#windows-portal) veya [PowerShell](#windows-powershell) bu makaledeki adımları. 

1. Bir Internet tarayıcısından Azure açın [portal](https://portal.azure.com) ve Azure ile oturum [hesap](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Zaten bir hesabınız yoksa, için kaydolabilirsiniz bir [ücretsiz deneme sürümü](https://azure.microsoft.com/offers/ms-azr-0044p).
2. Metni içeren kutusunda *arama kaynakları* Azure portalının en üstünde yazın *MyVm*. Zaman **MyVm** görünür arama sonuçlarında tıklatın.
3. İçinde **MyVm** görünür, dikey tıklayın **Bağlan** dikey pencerenin sol üst köşesinde düğmesini. **Not:** varsa **oluşturma** altında görülebilir **Bağlan** düğmesi, Azure henüz tamamlanmadı VM oluşturma. Tıklatın **Bağlan** yalnızca artık gördükten sonra **oluşturma** altında **Bağlan** düğmesi.
4. Karşıdan yüklemek tarayıcınızın izin **MyVm.rdp** dosya.  Yüklendikten sonra dosyayı açmak için tıklatın. 
5. Tıklatın **Bağlan** düğmesini **Uzak Masaüstü Bağlantısı** görünür, uzak bağlantı yayımcısı tanımlanamıyor bildiren kutusu.
6. İçinde **Windows Güvenlik** görünen kutusu **daha fazla seçenek**, ardından **farklı bir hesap kullanın**. Adım 4'te girdiğiniz parola ve kullanıcı adı girin ve ardından **Tamam** düğmesi.
7. Tıklatın **Evet** görünür, uzak bilgisayarın kimliğinin doğrulanıp doğrulanamadığını bildiren Uzak Masaüstü Bağlantısı kutusundaki düğmesi.
8. Windows Başlat düğmesine sağ tıklatın ve **Aygıt Yöneticisi'ni**. Genişletme **ağ bağdaştırıcıları** düğümü. Onaylayın **Mellanox ConnectX-3 sanal işlev Ethernet bağdaştırıcısı** , aşağıdaki resimde gösterildiği gibi görünür:
   
    ![Cihaz Yöneticisi](./media/virtual-network-create-vm-accelerated-networking/image2.png)

9. Hızlandırılmış ağ, VM için etkinleştirilmiştir.

## <a name="create-a-linux-vm"></a>Linux VM oluşturma
Azure portalında veya Azure kullanabilirsiniz [PowerShell](#linux-powershell) bir Ubuntu veya SLES VM oluşturmak için. RHEL ve CentOS VM'ler için farklı bir iş akışı yok.  Lütfen aşağıdaki yönergelere bakın.

### <a name="linux-portal"></a>Portal
1. 1-5 tamamlayarak Linux Önizleme adımları için hızlandırılmış ağlar için kayıt [PowerShell bir Linux VM - oluşturma](#linux-powershell) bu makalenin.  Önizleme portalında kaydedilemiyor.
2. Adım 1-8'de tamamlamak [bir Windows VM - oluşturma portal](#windows-portal) bu makalenin. 2. adımda tıklatın **Ubuntu Server 16.04 LTS** yerine **Windows Server 2016 Datacenter**. Üretim dağıtımları için ya da kullanabilirsiniz ancak bu öğreticide, bir SSH anahtarı yerine bir parola kullanmak üzere seçin. Varsa **ağ hızlandırılmış** 7. adımını tamamladığınızda görünmez [bir Windows VM - oluşturma portal](#windows-portal) bölümünde bu makalede, aşağıdaki nedenlerden birinden dolayı olasıdır:
    - Henüz Önizleme için kayıtlı değil. Kayıt durumu olduğunu onaylayın **kayıtlı**4 adımda açıklandığı gibi [Powershell bir Linux VM - oluşturma](#linux-powershell) bu makalenin. **Not:** (bunu artık sanal makineleri için Windows ağ hızlandırılmış kullanmak üzere kaydetmek gerekli) Windows VM'ler Önizleme hızlandırılmış ağ katıldıysanız Linux VM'ler önizlemek için otomatik olarak hızlandırılmış ağ iletişimi için kayıtlı olmayan. İçinde katılmak için Linux VM'ler Önizleme hızlandırılmış ağ kaydetmeniz gerekir.
    - İşletim sistemi, bir VM boyutu seçili olmadığında veya konum listelenen [sınırlamalar](#limitations) bu makalenin.
3. Linux için hızlandırılmış ağ sürücüsü yüklemek için adımları tamamlamanız [yapılandırma Linux](#configure-linux) bu makalenin.

### <a name="linux-powershell"></a>PowerShell

>[!WARNING]
>Bir abonelikte hızlandırılmış ağ ile Linux VM'ler oluşturur ve aynı abonelikte hızlandırılmış ağ ile bir Windows VM oluşturma girişimi, Windows VM oluşturma işlemi başarısız olabilir. Bu önizleme sırasında ayrı Aboneliklerde hızlandırılmış ağ ile Linux ve Windows VM'ler test önerilir.
>

1. Azure PowerShell'in en son sürümünü yüklemek [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modülü. Azure PowerShell yeniyseniz, okuma [Azure PowerShell genel bakış](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) makalesi.
2. Windows Başlat düğmesine tıklayarak bir PowerShell oturumu Başlat yazarak **powershell**, ardından **PowerShell** Arama sonuçlarından.
3. PowerShell pencerenize girin `login-azurermaccount` oturum Azure imzalamak için komutu [hesap](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Zaten bir hesabınız yoksa, için kaydolabilirsiniz bir [ücretsiz deneme sürümü](https://azure.microsoft.com/offers/ms-azr-0044p).
4. Kasa Azure hızlandırılmış ağ iletişimi için aşağıdaki adımları tamamlayarak önizleme:
    - E-posta Gönder [ axnpreview@microsoft.com ](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) Azure abonelik kimliği ve kullanım. Aboneliğiniz etkinleştirilecek hakkında Microsoft'tan bir e-posta onayı için lütfen bekleyin.
    - Önizleme için kayıtlı onaylamak için aşağıdaki komutu girin:
    
        ```powershell
        Get-AzureRmProviderFeature -FeatureName AllowAcceleratedNetworkingForLinux -ProviderNamespace Microsoft.Network
        ```

        Adım 5 kadar ile devam etmeyin **kayıtlı** önceki komutu girdikten sonra çıktısında görüntülenir. Çıktı, devam etmeden önce aşağıdaki çıkış gibi benzemelidir:
    
        ```powershell
        FeatureName                        ProviderName      RegistrationState
        -----------                        ------------      -----------------
        AllowAcceleratedNetworkingForLinux Microsoft.Network Registered
        ```
        
      >[!NOTE]
      >Windows sanal makineleri Önizleme (artık Windows VM'ler için ağ hızlandırılmış kullanmak üzere kaydetmek gerekli değildir) hızlandırılmış ağ katıldıysanız Linux VM'ler önizlemek için otomatik olarak hızlandırılmış ağ iletişimi için kayıtlı değil. İçinde katılmak için Linux VM'ler Önizleme hızlandırılmış ağ kaydetmeniz gerekir.
      >
5. Tarayıcınızda, Ubuntu veya SLES istendiği gibi değiştirerek aşağıdaki komutu kopyalayın.  Yeniden Redhat ve CentOS aşağıda açıklanan farklı bir iş akışı vardır:

    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
      -Name $RgName `
      -Location $Location
    
    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
      -Name MySubnet `
      -AddressPrefix 10.0.0.0/24
    
    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName $RgName `
      -Location $Location `
      -Name MyVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet

    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
      -Name MyPublicIp `
      -ResourceGroupName $RgName `
      -Location $Location `
      -AllocationMethod Static

    # Create a virtual network interface and associate the public IP address to it
    $Nic = New-AzureRmNetworkInterface `
      -Name MyNic `
      -ResourceGroupName $RgName `
      -Location $Location `
      -SubnetId $Vnet.Subnets[0].Id `
      -PublicIpAddressId $Pip.Id `
      -EnableAcceleratedNetworking
     
    # Create a new Storage account and define the new VM’s OSDisk name and its URI
    # Must end with ".vhd" extension
    $OSDiskName = "MyOsDiskName.vhd"
    # Storage account name must be between 3 and 24 characters in length and use numbers and lower-case letters only.
    $OSDiskSAName = "thestorageaccountname"  
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $RgName -Name $OSDiskSAName -Type "Standard_GRS" -Location $Location
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName
 
    # Define a credential object for the VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential

    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
      -VMName MyVM -VMSize Standard_DS4_v2 | `
      Set-AzureRmVMOperatingSystem `
      -Linux `
      -ComputerName myVM `
      -Credential $Cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName Canonical `
      -Offer UbuntuServer `
      -Skus 16.04-LTS `
      -Version latest | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id | `
    Set-AzureRmVMOSDisk -Name $OSDiskName `
      -VhdUri $OSDiskUri `
      -CreateOption FromImage 

    # Create the virtual machine.    
    New-AzureRmVM `
      -ResourceGroupName $RgName `
      -Location $Location `
      -VM $VmConfig
    ```
    
6. PowerShell pencerenize betiğini yapıştırın ve bunu çalıştırmaya başlamak için sağ tıklatın. Bir kullanıcı adı ve parolası istenir. VM için sonraki adımda bağlanırken oturum açmak için bu kimlik bilgileri kullanın. Komut başarısız olursa, onaylayın:
    - 4. adımda açıklandığı gibi Önizleme için kayıtlı
    - VM boyutu, işletim sistemi türü veya komut dosyası konumu değerleri yürütmeden önce değiştirdiğiniz varsa, değerleri listelenen [sınırlamalar](#Limitations) bu makalenin.
7. Linux için hızlandırılmış ağ sürücüsü yüklemek için adımları tamamlamanız [yapılandırma Linux](#configure-linux) bu makalenin.

### <a name="configure-linux"></a>Linux yapılandırın

Azure'da VM oluşturduktan sonra Linux için hızlandırılmış ağ sürücüsü yüklemeniz gerekir. Aşağıdaki adımları gerçekleştirmeden önce bir Linux VM ile hızlandırılmış ağ kullanarak oluşturduğunuz gerekir [portal](#linux-portal) veya [PowerShell](#linux-powershell) bu makaledeki adımları. 

1. Bir Internet tarayıcısından Azure açın [portal](https://portal.azure.com) ve Azure ile oturum [hesap](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Zaten bir hesabınız yoksa, için kaydolabilirsiniz bir [ücretsiz deneme sürümü](https://azure.microsoft.com/offers/ms-azr-0044p).
2. En sağındaki portalı üstündeki *arama kaynakları* çubuğu, tıklatın **> _** Bash bulut Kabuğu (Önizleme) başlatmak için simge. Portal ve birkaç saniye sonra altındaki Bash bulut Kabuk bölmesinde görünür sunar bir  **username@Azure:~ $** istemi. SSH VM bulut Kabuk yerine bilgisayarınızı olabilir ancak bu öğreticideki yönergeler bulut Kabuk kullanmakta olduğunuz varsayılır.
3. Portal en üstünde metni içeren kutusuna *arama kaynakları*, türü *MyVm*. Zaman **MyVm** görünür arama sonuçlarında tıklatın.
4. İçinde **MyVm** görünür, dikey tıklayın **Bağlan** dikey pencerenin sol üst köşesinde düğmesini. **Not:** varsa **oluşturma** altında görülebilir **Bağlan** düğmesi, Azure henüz tamamlanmadı VM oluşturma. Tıklatın **Bağlan** yalnızca artık gördükten sonra **oluşturma** altında **Bağlan** düğmesi.
5. Azure açar girmenizi bildiren bir kutusu `ssh adminuser@<ipaddress>`. ENTER bulut Kabuğu (veya onu görünen kutusundan adım 4 kopyalayın ve bulut Kabuk yapıştırın) Bu komutta ENTER tuşuna basın.
6. ENTER **Evet** , bağlanmak, devam etmek istiyorsanız ENTER tuşuna basın isteyen soruya.
7. VM oluştururken girdiğiniz parola girin. Bir kez başarıyla VM için bkz: oturum açmış bir adminuser@MyVm:~ $ istemi. Artık VM bulut kabuk oturumu aracılığıyla oturum açmış. **Not:** 10 dakika işlem yapılmadıktan sonra bulut Kabuk oturumları zaman aşımına uğrar.

Bu noktada, yönergeleri, kullanmakta olduğunuz dağıtım göre farklılık gösterir. 

#### <a name="ubuntusles"></a>Ubuntu/SLES

1. İsteminde girin `uname -r` ve sürümü onaylayın:

    * Ubuntu olan "4.4.0-77-generic," veya daha büyük
    * SLES "4.4.59-92.20-default" olan veya daha büyük

2. Standart ağ Vnıc hızlandırılmış ağ Vnıc arasındaki bono izleyin komutlarını çalıştırarak oluşturun. Bononun ağ trafiği üzerinde bazı yapılandırma değişikliklerinin kesilmemesini sağlar ancak daha yüksek performanslı hızlandırılmış ağ Vnıc, ağ trafiğini kullanır.
          
     ```bash
     wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/configure_hv_sriov.sh
     chmod +x ./configure_hv_sriov.sh
     sudo ./configure_hv_sriov.sh
     ```
3. Komut dosyasını çalıştırdıktan sonra 60 saniye duraklatmak sonra VM yeniden başlatılır.
4. VM yeniden başlatıldıktan sonra adım 5-7 yeniden tamamlayarak yeniden bağlanın.
5. Çalıştırma `ifconfig` komut ve bond0 gündeme ve arabirim yukarı gösteren onaylayın. 
 
 >[!NOTE]
      >Hızlandırılmış ağ kullanan uygulamalar üzerinden iletişim kurması gerekir *bond0* arabirim, *eth0*.  Genel kullanılabilirlik hızlandırılmış ağ erişmeden önce arabirim adı değişebilir.

#### <a name="rhelcentos"></a>RHEL/CentOS

Red Hat Enterprise Linux veya CentOS 7.3 VM oluşturma SR-IOV ve sanal işlev (VF) sürücüsünün ağ kartı için gereken en son sürücüleri yüklemek için bazı ek adımlar gerektirir. Birinci aşama yönergeleri bir veya daha fazla sanal önceden yüklenen sürücülerin makinelerde yapmak için kullanılan bir görüntü hazırlar.

##### <a name="phase-one-prepare-a-red-hat-enterprise-linux-or-centos-73-base-image"></a>Aşama bir: Red Hat Enterprise Linux veya CentOS 7.3 temel görüntü hazırlayın. 

1.  Bir olmayan - SRLOV CentOS 7.3 VM Azure sağlama

2.  LIS 4.2.2 yükleyin:
    
    ```bash
    wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
    tar -xvf lis-rpms-4.2.2-2.tar.gz
    cd LISISO && sudo ./install.sh
    ```

3.  Yapılandırma dosyaları indirme
    
    ```bash
    cd /etc/udev/rules.d/  
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/60-hyperv-vf-name.rules 
    cd /usr/sbin/
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/hv_vf_name 
    sudo chmod +x hv_vf_name
    cd /etc/sysconfig/network-scripts/
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/ifcfg-vf1   
    ```

4.  Bu VM yetkisini kaldırma

    ```bash
    sudo waagent -deprovision+user 
    ```

5.  Azure portalından, bu VM'yi durdurun; ve sanal makinenin "disklere" gidin, OSDisk ait VHD URİ'si yakalama. Bu URI temel görüntünün VHD adını ve kendi depolama hesabını içerir. 
 
##### <a name="phase-two-provision-new-vms-on-azure"></a>İki aşama: sağlama yeni sanal makineleri Azure üzerinde

1.  Sağlama yeni VM'ler aşamasında, vNIC üzerinde etkin AcceleratedNetworking ile yakalanmış VHD temel görüntü kullanarak yeni AzureRMVMConfig ile göre:

    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
     -Name $RgName `
     -Location $Location

    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
     -Name MySubnet `
     -AddressPrefix 10.0.0.0/24

    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
     -ResourceGroupName $RgName `
     -Location $Location `
     -Name MyVnet `
     -AddressPrefix 10.0.0.0/16 `
     -Subnet $Subnet
    
    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
     -Name MyPublicIp `
     -ResourceGroupName $RgName `
     -Location $Location `
     -AllocationMethod Static
    
    # Create a virtual network interface and associate the public IP address to it
    $Nic = New-AzureRmNetworkInterface `
     -Name MyNic `
     -ResourceGroupName $RgName `
     -Location $Location `
     -SubnetId $Vnet.Subnets[0].Id `
     -PublicIpAddressId $Pip.Id `
     -EnableAcceleratedNetworking
    
    # Specify the base image's VHD URI (from phase one step 5). 
    # Note: The storage account of this base image vhd should have "Storage service encryption" disabled
    # See more from here: https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption
    # This is just an example URI, you will need to replace this when running this script
    $sourceUri="https://myexamplesa.blob.core.windows.net/vhds/CentOS73-Base-Test120170629111341.vhd" 

    # Specify a URI for the location from which the new image binary large object (BLOB) is copied to start the virtual machine. 
    # Must end with ".vhd" extension
    $OsDiskName = "MyOsDiskName.vhd" 
    $destOsDiskUri = "https://myexamplesa.blob.core.windows.net/vhds/" + $OsDiskName
    
    # Define a credential object for the VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential
    
    # Create a custom virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
     -VMName MyVM -VMSize Standard_DS4_v2 | `
    Set-AzureRmVMOperatingSystem `
     -Linux `
     -ComputerName myVM `
     -Credential $Cred | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id | `
    Set-AzureRmVMOSDisk `
     -Name $OsDiskName `
     -SourceImageUri $sourceUri `
     -VhdUri $destOsDiskUri `
     -CreateOption FromImage `
     -Linux
    
    # Create the virtual machine.    
    New-AzureRmVM `
     -ResourceGroupName $RgName `
     -Location $Location `
     -VM $VmConfig
    ```

2.  Yukarı VM'ler önyükleme sonra "lspci" tarafından VF aygıt denetleyin ve Mellanox girdisine bakın. Örneğin, biz bu öğede lspci çıktı görmeniz gerekir:
    
    ```
    0001:00:02.0 Ethernet controller: Mellanox Technologies MT27500/MT27520 Family [ConnectX-3/ConnectX-3 Pro Virtual Function]
    ```
    
3.  Tarafından bağlama komut dosyasını çalıştırın:

    ```bash
    sudo bondvf.sh
    ```

4.  Yeni sanal makineleri yeniden başlatın:

    ```bash
    sudo reboot
    ```

VM yapılandırılmış bond0 ve etkin hızlandırılmış ağ yolu ile başlamanız gerekir.  Çalıştırma `ifconfig` onaylamak için.
