---
title: "İç içe geçmiş sanallaştırma Azure Virtual Machines'de etkinleştirme | Microsoft Docs"
description: "İç içe geçmiş sanallaştırma Azure Virtual Machines'de etkinleştirme"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: philmea
manager: timlt
ms.author: philmea
ms.date: 10/09/2017
ms.topic: howto
ms.service: virtual-machines-windows
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.openlocfilehash: 180b87e18d98bb1e7ddefdcce09fc45d2fc26d0f
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2018
---
# <a name="how-to-enable-nested-virtualization-in-an-azure-vm"></a>İç içe geçmiş sanallaştırma Azure VM'deki etkinleştirme

İç içe geçmiş sanallaştırma Azure sanal makineleri Dv3 ve Ev3 serinin desteklenir. Bu özelliği geliştirme, test, eğitim ve tanıtım ortamları gibi senaryolarını büyük esneklik sağlar. 

Azure VM'de iç içe geçmiş sanallaştırma etkinleştirme ve yapılandırma, Konuk sanal makine için Internet bağlantısı üzerinden bu makalede adımlar.

## <a name="create-a-dv3-or-ev3-series-azure-vm"></a>Bir Dv3 veya Ev3 serisi Azure VM oluşturma

Yeni bir Windows Server 2016 Azure VM oluşturun ve Dv3 veya Ev3 serisinden bir boyutu seçin. Bir konuk sanal makine taleplerini desteklemek için büyük bir boyutu seçin emin olun. Bu örnekte, bir D3_v3 boyutu Azure VM kullanıyoruz. 

Bölgesel Dv3 veya Ev3 serisi sanal makinelerin kullanılabilirliğini görüntüleyebilirsiniz [burada](https://azure.microsoft.com/regions/services/).

>[!NOTE]
>
>Yeni bir sanal makine oluşturma hakkında ayrıntılı yönergeler için bkz: [oluşturma ve Azure PowerShell modülü ile Windows sanal makineleri yönetme](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-manage-vm)
    
## <a name="connect-to-your-azure-vm"></a>Azure VM'nize bağlanmak

Sanal makine ile bir uzak masaüstü bağlantısı oluşturun.

1. Sanal makine özelliklerinde **Bağlan** düğmesine tıklayın. Uzak Masaüstü Protokolü dosyasını (.rdp dosyası) oluşturulup indirilir.

2. VM'nize bağlanmak için indirilen RDP dosyasını açın. İstenirse, **Bağlan**’a tıklayın. Mac bilgisayarlarda, Mac App Store’dan bu [Uzak Masaüstü İstemcisi](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) gibi bir RDP istemcisi indirmeniz gerekir.

3. Sanal makine oluştururken belirttiğiniz kullanıcı adı ile parolayı girin ve **Tamam**’a tıklayın.

4. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için **Evet** veya **Devam**’a tıklayın.

## <a name="enable-the-hyper-v-feature-on-the-azure-vm"></a>Azure VM'de Hyper-V özelliğini etkinleştir
Bu ayarları yapılandırabilirsiniz el ile veya yapılandırma otomatikleştirmek için bir PowerShell komut dosyası sağladık.

### <a name="option-1-use-a-powershell-script-to-configure-nested-virtualization"></a>Seçenek 1: iç içe geçmiş sanallaştırma yapılandırmak için bir PowerShell betiğini kullanın.
İç içe geçmiş bir Windows Server 2016 konak sanallaştırmayı etkinleştirmek için bir PowerShell Betiği kullanılabilir [GitHub](https://github.com/charlieding/Virtualization-Documentation/tree/live/hyperv-tools/Nested). Komut dosyası, önkoşulları denetler ve iç içe geçmiş sanallaştırma Azure VM'de yapılandırır. Azure VM yeniden yapılandırmayı tamamlamak gereklidir. Bu komut diğer ortamlarda çalışabilir ancak garanti edilmez. İç içe geçmiş sanallaştırma Azure üzerinde çalışan bir canlı video Tanıtımı ile Azure blog gönderisine göz atın! https://aka.ms/AzureNVblog.

### <a name="option-2-configure-nested-virtualization-manually"></a>Seçenek 2: iç içe geçmiş sanallaştırma el ile yapılandırma

1. Azure VM'de, bir yönetici olarak PowerShell'i açın. 

2. Yönetim Araçları ve Hyper-V özelliğini etkinleştirin.

    ```powershell
    Install-WindowsFeature -Name Hyper-V -IncludeManagementTools -Restart
    ```

    >[!WARNING] 
    >
    >Bu komut Azure VM'yi yeniden başlatır. RDP bağlantınızı yeniden başlatma işlemi sırasında kaybedersiniz.
    
3. Azure VM yeniden başlatıldıktan sonra RDP kullanarak VM için yeniden bağlanın.

## <a name="set-up-internet-connectivity-for-the-guest-virtual-machine"></a>Konuk sanal makine için internet bağlantısı ayarlama
Konuk sanal makine için yeni bir sanal ağ bağdaştırıcısı oluşturun ve Internet bağlantısını etkinleştirmek için NAT ağ geçidi yapılandırın.

### <a name="create-a-nat-virtual-network-switch"></a>Bir NAT sanal ağ anahtarı oluşturma

1. Azure VM'de, bir yönetici olarak PowerShell'i açın.
   
2. Bir iç anahtar oluşturun.

    ```powershell
    New-VMSwitch -Name "InternalNATSwitch" -SwitchType Internal
    ```

3. Anahtar özelliklerini görüntülemek ve yeni bağdaştırıcısı İfındex dikkat edin.

    ```powershell
    Get-NetAdapter
    ```

    ![NetAdapter](./media/virtual-machines-nested-virtualization/get-netadapter.png)

    >[!NOTE] 
    >
    >Yeni oluşturduğunuz sanal anahtarı için "İfındex" not edin.
    
4. Bir IP adresi için NAT ağ geçidi oluşturun.
    
Ağ geçidini yapılandırmak için ağınız hakkında bazı bilgiler gereklidir:    
  * IPADDRESS - NAT ağ geçidi IP sanal ağ alt ağı için varsayılan ağ geçidi adresi olarak kullanılacak IPv4 veya IPv6 adresini belirtir. Genel form a.b.c.1 (örneğin, "192.168.0.1") olur. Son Konum, genellikle (önek uzunluğuna göre).1 olmak zorunda değildir ancak. Genellikle bir RFC 1918 özel ağ adres alanı kullanmanız gerekir. 
  * PrefixLength - alt ağ önek uzunluğu (alt ağ maskesi) yerel alt ağ boyutunu tanımlar. Alt ağ önek uzunluğu 0 ile 32 arasında bir tamsayı değeri olacaktır. 0 tüm internet eşleyen, 32 yalnızca bir eşlenen IP olanak tanır. 24 ortak değerleri arasında kaç IP'leri bağlı olarak 12 NAT bağlı olması gerekir Ortak PrefixLength 24 olur - bu, 255.255.255.0 alt ağ maskesidir.
  * InterfaceIndex - **İfındex** önceki adımda oluşturduğunuz sanal anahtarın arabirim dizinidir. 

    ```powershell
    New-NetIPAddress -IPAddress 192.168.0.1 -PrefixLength 24 -InterfaceIndex 13
    ```

### <a name="create-the-nat-network"></a>NAT ağ oluşturma

Ağ geçidini yapılandırmak için ağ ve NAT ağ geçidi hakkında bilgi sağlamanız gerekir:
  * Ad - NAT ağın adı budur. 
  * InternalIPInterfaceAddressPrefix - NAT alt ağ öneki hem üstten NAT ağ geçidi IP öneki, hem de NAT alt ağ önek uzunluğu üstten açıklar. Genel form a.b.c.0/NAT alt ağ önek uzunluğu olacaktır. 

PowerShell'de, yeni bir NAT ağ oluşturun.
```powershell
New-NetNat -Name "InternalNat" -InternalIPInterfaceAddressPrefix 192.168.0.0/24
```


## <a name="create-the-guest-virtual-machine"></a>Konuk sanal makine oluşturma

1. Hyper-V Yöneticisi'ni açın ve yeni bir sanal makine oluşturun. Sanal makineyi oluşturduğunuz yeni iç ağda kullanmak için yapılandırın.
    
    ![NetworkConfig](./media/virtual-machines-nested-virtualization/configure-networking.png)
    
2. Bir işletim sistemi Konuk sanal makineye yükleyin.
    
    >[!NOTE] 
    >
    >VM yüklemek bir işletim sistemi yükleme medyası gerekir. Bu durumda Windows 10 Enterprise kullanıyoruz.

## <a name="assign-an-ip-address-to-the-guest-virtual-machine"></a>Konuk sanal makineye bir IP adresi atayın

Konuk sanal makineye el ile statik bir IP adresi Konuk sanal makinede ayarlama veya Azure VM IP adresini dinamik olarak atamak için DHCP yapılandırma, bir IP adresi atayabilirsiniz.

###  <a name="option-1-configure-dhcp-to-dynamically-assign-an-ip-address-to-the-guest-virtual-machine"></a>Seçenek 1: dinamik olarak Konuk sanal makineye bir IP adresi atamak için DHCP yapılandırma
DHCP ana bilgisayar sanal makine dinamik adres ataması için yapılandırmak için aşağıdaki adımları izleyin.

#### <a name="install-dchp-server-on-the-azure-vm"></a>DHCP sunucusu Azure VM olanağına yükle

1. Sunucu Yöneticisi'ni açın. Panoda tıklatın **rol ve Özellik Ekle**. Ekle roller ve Özellikler Sihirbazı görünür.
  
2. Sihirbazı'nda tıklatın **sonraki** sunucu rollerini sayfasında kadar.
  
3. Seçmek için tıklatın **DHCP sunucusu** onay kutusunu tıklatın **Özellik Ekle**ve ardından **sonraki** Sihirbazı tamamlanana kadar.
  
4. **Yükle**'ye tıklayın.

#### <a name="configure-a-new-dhcp-scope"></a>Yeni bir DHCP kapsamı yapılandırılması

1. DHCP Yöneticisi'ni açın.
  
2. Gezinti bölmesinde, sunucu adını genişletin, sağ tıklatın **IPv4**, tıklatıp **yeni kapsam**. Yeni Kapsam Sihirbazı'nı görüntülendikten sonra **sonraki**.
  
3. Kapsam için bir ad ve açıklama girin ve tıklayın **sonraki**.
  
4. Bir IP aralığı DCHP sunucunuz için (örneğin, 192.168.0.100 için 192.168.0.200) tanımlayın.
  
5. Tıklatın **sonraki** varsayılan ağ geçidi sayfa kadar. Varsayılan ağ geçidi olarak daha önce (örneğin, 192.168.0.1) oluşturduğunuz IP adresi girin.
  
6. Tıklatın **sonraki** sihirbaz tamamlanana kadar tüm varsayılan değerler, bırakarak ardından **son**.
    
### <a name="option-2-manually-set-a-static-ip-address-on-the-guest-virtual-machine"></a>Seçenek 2: Statik bir IP adresi Konuk sanal makineye el ile ayarlayın.
Dinamik olarak Konuk sanal makineye bir IP adresi atamak için DHCP yapılandırmadıysanız, statik IP adresi ayarlamak için aşağıdaki adımları izleyin.

1. Azure VM'de, bir yönetici olarak PowerShell'i açın.

2. Konuk sanal makineye sağ tıklayın ve Bağlan'ı tıklatın.

3. Konuk sanal makinede oturum açın.

4. Konuk sanal makinede ağ ve Paylaşım Merkezi'ni açın.

5. Ağ bağdaştırıcısı önceki bölümde oluşturduğunuz NAT ağ aralıkta bir adres için yapılandırın.

Bu örnekte 192.168.0.0/24 aralıkta bir adres kullanır.

## <a name="test-connectivity-in-guest-virtual-machine"></a>Konuk sanal makinesinde bağlantı testi

Konuk sanal makinede tarayıcınızı açın ve bir web sayfasına gidin.
    ![GuestVM](./media/virtual-machines-nested-virtualization/guest-virtual-machine.png)
