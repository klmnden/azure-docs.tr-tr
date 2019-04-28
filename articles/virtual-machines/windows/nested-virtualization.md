---
title: Azure sanal Makineler'de iç içe sanallaştırmayı etkinleştirmek nasıl | Microsoft Docs
description: Azure sanal Makineler'de iç içe sanallaştırmayı etkinleştirme
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
ms.author: cynthn
ms.date: 10/09/2017
ms.topic: conceptual
ms.service: virtual-machines-windows
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.openlocfilehash: f90ca51349eef92bd25095f5a2a10d7d181fdb2c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61488379"
---
# <a name="how-to-enable-nested-virtualization-in-an-azure-vm"></a>Azure VM'de iç içe sanallaştırmayı etkinleştirme

İç içe sanallaştırmayı birden fazla Azure sanal makine ailelerinde desteklenir. Bu özellik, geliştirme, test, eğitim ve tanıtım ortamları gibi senaryoları destekleyen büyük esneklik sağlar.   

Bu makalede adımları Hyper-V Azure sanal makinesinde etkinleştiriliyor ve Konuk sanal makinenin Internet bağlantısı yapılandırma.

## <a name="create-a-nesting-capable-azure-vm"></a>İç içe geçme özelliğine sahip bir Azure VM oluşturma

Yeni bir Windows Server 2016 Azure VM oluşturun.  Hızlı başvuru için iç içe sanallaştırma tüm v3 sanal makineleri destekler. Bu destek iç içe geçme tam bir listesi sanal makine boyutları için kullanıma [Azure işlem birimi makale](acu.md).

Konuk sanal makine taleplerini destekleyecek kadar büyük bir VM boyutu seçme unutmayın. Bu örnekte, bir Azure VM D3_v3 boyutu kullanıyoruz. 

Bölgesel kullanılabilirlik Dv3 ya da Ev3 serisi sanal makinelerin görüntüleyebilirsiniz [burada](https://azure.microsoft.com/regions/services/).

>[!NOTE]
>
>Yeni bir sanal makine oluşturma konusunda ayrıntılı yönergeler için bkz: [oluşturma ve Azure PowerShell modülü ile Windows sanal makineleri yönetme](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-manage-vm)
    
## <a name="connect-to-your-azure-vm"></a>Azure sanal Makinenize bağlanın

Sanal makine ile bir uzak masaüstü bağlantısı oluşturun.

1. Sanal makine özelliklerinde **Bağlan** düğmesine tıklayın. Uzak Masaüstü Protokolü dosyasını (.rdp dosyası) oluşturulup indirilir.

2. VM'nize bağlanmak için indirilen RDP dosyasını açın. İstenirse, **Bağlan**’a tıklayın. Mac bilgisayarlarda, Mac App Store’dan bu [Uzak Masaüstü İstemcisi](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) gibi bir RDP istemcisi indirmeniz gerekir.

3. Sanal makine oluştururken belirttiğiniz kullanıcı adı ile parolayı girin ve **Tamam**’a tıklayın.

4. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için **Evet** veya **Devam**’a tıklayın.

## <a name="enable-the-hyper-v-feature-on-the-azure-vm"></a>Azure VM üzerindeki Hyper-V özelliğini etkinleştir
Bu ayarları yapılandırabileceğiniz el ile veya yapılandırma otomatikleştirmek için bir PowerShell komut dosyası sağladık.

### <a name="option-1-use-a-powershell-script-to-configure-nested-virtualization"></a>1. seçenek: İç içe sanallaştırma yapılandırmak için bir PowerShell betiğini kullanın
Bir Windows Server 2016 konağında iç içe sanallaştırmayı etkinleştirmek için bir PowerShell Betiği edinilebilir [GitHub](https://github.com/charlieding/Virtualization-Documentation/tree/live/hyperv-tools/Nested). Betik, önkoşulları denetler ve sonra Azure sanal makinesinde iç içe sanallaştırma yapılandırır. Yapılandırmayı tamamlamak Azure VM yeniden başlatma gereklidir. Bu betik, diğer ortamlarda çalışabilir ancak garanti edilmez. Azure üzerinde çalışan iç içe sanallaştırma üzerinde canlı video gösterimi ile Azure blog gönderisine göz atın! https://aka.ms/AzureNVblog.

### <a name="option-2-configure-nested-virtualization-manually"></a>2. seçenek: İç içe sanallaştırma el ile yapılandırma

1. Azure sanal makinesinde PowerShell'i yönetici olarak açın. 

2. Hyper-V özelliğini ve Yönetim Araçları'nı etkinleştirin.

    ```powershell
    Install-WindowsFeature -Name Hyper-V -IncludeManagementTools -Restart
    ```

    >[!WARNING] 
    >
    >Bu komut, Azure VM'yi yeniden başlatır. RDP bağlantınız yeniden başlatma işlemi sırasında kaybedersiniz.
    
3. Azure VM yeniden başlatıldıktan sonra RDP kullanarak VM'nize bağlanın.

## <a name="set-up-internet-connectivity-for-the-guest-virtual-machine"></a>Konuk sanal makinesi için internet bağlantısı ayarlama
Konuk sanal makine için yeni bir sanal ağ bağdaştırıcısı oluşturmak ve Internet bağlantısını etkinleştirmek için NAT ağ geçidi yapılandırın.

### <a name="create-a-nat-virtual-network-switch"></a>NAT sanal ağ anahtarı oluşturma

1. Azure sanal makinesinde PowerShell'i yönetici olarak açın.
   
2. Bir iç anahtar oluşturun.

    ```powershell
    New-VMSwitch -Name "InternalNATSwitch" -SwitchType Internal
    ```

3. Anahtar özelliklerini görüntülemek ve yeni bağdaştırıcı ifIndex dikkat edin.

    ```powershell
    Get-NetAdapter
    ```

    ![NetAdapter](./media/virtual-machines-nested-virtualization/get-netadapter.png)

    >[!NOTE] 
    >
    >Yeni oluşturduğunuz sanal anahtar "ifIndex" not alın.
    
4. Ağ geçidi için NAT IP adresi oluşturun.
    
Ağ geçidini yapılandırmak için ağınız hakkında bazı bilgiler gereklidir:    
  * IP adresi - sanal ağ alt ağ için varsayılan ağ geçidi adresi olarak kullanılacak IPv4 veya IPv6 adresi NAT ağ geçidi IP belirtir. Genel form a.b.c.1 (örneğin, "192.168.0.1") ' dir. Son konum da genellikle (ön eki uzunluğa göre).1 olması gerekmez ancak. Genellikle, RFC 1918 özel ağ adres alanı kullanmanız gerekir. 
  * PrefixLength - alt ağ önek uzunluğunun yerel alt ağ boyutu (alt ağ maskesi) tanımlar. Alt ağ önek uzunluğu 0 ile 32 arasında bir tamsayı değeri olur. 0 tüm internet eşlemek, 32 yalnızca eşlenen bir IP çalıştırmasına olanak tanır. 24 ortak değerleri arasında 12 kaç IP'ler bağlı olarak gereken NAT için eklenecek Ortak PrefixLength 24--bir alt ağ maskesi 255.255.255.0 budur.
  * InterfaceIndex - **İfındex** arabirim önceki adımda oluşturduğunuz sanal anahtarın dizinidir. 

    ```powershell
    New-NetIPAddress -IPAddress 192.168.0.1 -PrefixLength 24 -InterfaceIndex 13
    ```

### <a name="create-the-nat-network"></a>NAT ağını oluşturma

Ağ geçidini yapılandırmak için ağ ve NAT ağ geçidi hakkında bilgi sağlamanız gerekir:
  * Ad - Bu, NAT ağını adı. 
  * InternalIPInterfaceAddressPrefix - hem yukarıdaki NAT ağ geçidi IP öneki, hem de yukarıdaki NAT alt ağ önek uzunluğunun NAT alt ağ ön eki açıklar. Genel form, alt ağ önek uzunluğu a.b.c.0/NAT olacaktır. 

PowerShell'de, yeni bir NAT ağ oluşturun.
```powershell
New-NetNat -Name "InternalNat" -InternalIPInterfaceAddressPrefix 192.168.0.0/24
```


## <a name="create-the-guest-virtual-machine"></a>Konuk sanal makinesi oluşturma

1. Hyper-V Yöneticisi'ni açın ve yeni bir sanal makine oluşturun. Oluşturduğunuz yeni iç ağa kullanılacak sanal makineyi yapılandırın.
    
    ![NetworkConfig](./media/virtual-machines-nested-virtualization/configure-networking.png)
    
2. Bir işletim sistemi Konuk sanal makineye yükleyin.
    
    >[!NOTE] 
    >
    >VM'ye yüklemek bir işletim sistemi için yükleme medyası gerekir. Bu örnekte Windows 10 Enterprise kullanıyoruz.

## <a name="assign-an-ip-address-to-the-guest-virtual-machine"></a>Konuk sanal makineye bir IP adresi atama

Konuk sanal makineye el ile Konuk sanal makinede statik bir IP adresi ayarlama veya IP adresi dinamik olarak atamak için Azure sanal makinesinde DHCP yapılandırma, bir IP adresi atayabilirsiniz.

###  <a name="option-1-configure-dhcp-to-dynamically-assign-an-ip-address-to-the-guest-virtual-machine"></a>1. seçenek: Dinamik olarak Konuk sanal makineye bir IP adresi atamak için DHCP yapılandırma
DHCP ana bilgisayar sanal makine dinamik adres ataması için yapılandırmak için aşağıdaki adımları izleyin.

#### <a name="install-dchp-server-on-the-azure-vm"></a>Azure sanal makinesine DHCP sunucusu yükleme

1. Sunucu Yöneticisi'ni açın. Panosunda **rol ve Özellik Ekle**. Rol ve Özellik Ekleme Sihirbazı görünür.
  
2. Sihirbazı ' **sonraki** sunucu rolleri sayfası kadar.
  
3. Seçmek için tıklatın **DHCP sunucusu** onay kutusuna tıklayın **Özellik Ekle**ve ardından **sonraki** sihirbaz tamamlanana kadar.
  
4. **Yükle**'ye tıklayın.

#### <a name="configure-a-new-dhcp-scope"></a>Yeni bir DHCP kapsamı yapılandırın

1. DHCP Yöneticisi'ni açın.
  
2. Gezinti bölmesinde, sunucu adını genişletin, sağ **IPv4**, tıklatıp **yeni kapsam**. Yeni Kapsam Sihirbazı'nı görüntülendikten sonra **sonraki**.
  
3. Kapsam için bir ad ve açıklama girin ve tıklayın **sonraki**.
  
4. Bir IP aralığı DHCP sunucunuz için (örneğin, 192.168.0.100 için 192.168.0.200) tanımlayın.
  
5. Tıklayın **sonraki** varsayılan ağ geçidi sayfasının kadar. Varsayılan ağ geçidi olarak daha önce (örneğin, 192.168.0.1) oluşturduğunuz IP adresini girin, ardından tıklatın **Ekle**.
  
6. Tıklayın **sonraki** sihirbaz tamamlanana kadar tüm varsayılan değerleri bırakın ardından **son**.
    
### <a name="option-2-manually-set-a-static-ip-address-on-the-guest-virtual-machine"></a>2. seçenek: El ile Konuk sanal makinede statik bir IP adresi ayarlama
Dinamik olarak Konuk sanal makineye bir IP adresi atamak için DHCP yapılandırmadıysanız, statik bir IP adresi ayarlamak için aşağıdaki adımları izleyin.

1. Azure sanal makinesinde PowerShell'i yönetici olarak açın.

2. Konuk sanal makineye sağ tıklayın ve Bağlan'a tıklayın.

3. Konuk sanal makinesinde oturum açın.

4. Konuk sanal makinede ağ ve Paylaşım Merkezi'ni açın.

5. Ağ bağdaştırıcısı için önceki bölümde oluşturduğunuz NAT ağ aralıkta bir adres yapılandırın.

Bu örnekte 192.168.0.0/24 aralıkta bir adres kullanır.

## <a name="test-connectivity-in-guest-virtual-machine"></a>Konuk sanal makinesinde bağlantı testi

Konuk sanal makinede, tarayıcınızı açın ve bir web sayfasına gidin.
    ![GuestVM](./media/virtual-machines-nested-virtualization/guest-virtual-machine.png)

## <a name="set-up-intranet-connectivity-for-the-guest-virtual-machine"></a>Konuk sanal makine için intranet bağlantısı ayarlama

Konuk Vm'lerden ve Azure Vm'leri arasında saydam bağlantı etkinleştirme hakkında yönergeler için lütfen başvuru [bu belgeyi](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization-azure-virtual-network).
