---
title: Hızlı Başlangıç - özel Bulutu bir VMware VM oluşturma
description: Nasıl oluşturulacağını ve bir VMware VM CloudSimple özel bulut üzerinde açıklar.
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/03/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 33354ce09ad6ba1a9a7c08a8cd3b945f3788011a
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67595694"
---
# <a name="create-vmware-virtual-machines-on-your-private-cloud"></a>VMware sanal makinelerini özel Bulutunuzda oluşturma

Özel bulut üzerinde sanal makineler oluşturmak için Azure portalından CloudSimple portalı erişerek başlayın.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="access-the-cloudsimple-portal"></a>CloudSimple portalına erişim

1. **Tüm Hizmetler**’i seçin.
2. Arama **CloudSimple Hizmetleri**.
3. Özel bulut oluşturmak istediğiniz CloudSimple hizmeti seçin.
4. Gelen **genel bakış** sayfasında **CloudSimple Portal'a** CloudSimple portalı için yeni bir tarayıcı sekmesinde açmak için.  İstenirse, oturum, Azure'da oturum kimlik oturum açın.  

    ![CloudSimple portalını başlatma](media/launch-cloudsimple-portal.png)

## <a name="launch-vcenter-web-ui"></a>VCenter web kullanıcı arabirimini Başlat

Sanal makineler ve ilkeleri ayarlamak için vCenter artık başlatabilirsiniz.

VCenter erişmeye CloudSimple portaldan başlatın. Giriş sayfasında altında **ortak görevleri**, tıklayın **vSphere istemci başlatma**.  Özel Bulutu seçin ve ardından **vSphere istemci başlatma** özel bulutta.

   ![VSphere İstemcisi'ni başlatma](media/launch-vcenter-from-cloudsimple-portal.png)

## <a name="upload-an-iso-or-vsphere-template"></a>Bir ISO veya vSphere şablonunu karşıya yükleyin

> [!WARNING]
> ISO yüklemeler için vSphere HTML5 istemcisini kullanın.  Flash istemcisini kullanarak bir hataya neden olabilir.

1. VCenter sanal makine oluşturma ve karşıya yüklemek için kullanılabilir yerel sisteminizde istediğiniz ISO veya vSphere şablon edinin.

2. Vcenter'da, tıklayın **Disk** simgesini seçip alt **vsanDatastore**. Tıklayın **dosyaları** ve ardından **yeni klasör**.

    ![vCenter ISO](media/vcenter-create-folder.png)

3. ISO dosyaları depolamak için bir klasör oluşturun.

4. Oluşturduğunuz yeni klasöre gidin ve tıklayın **dosyaları karşıya yükle**. İzleyin ekrandaki ISO dosyasını karşıya yüklemek için yönergeler.

## <a name="create-a-virtual-machine-in-vcenter"></a>VCenter sanal makine oluşturma

1. Vcenter'da, tıklayın **konaklar ve kümeler** simgesi.

2. Sağ **iş yükü** seçip **yeni sanal makine**.
    
    ![Sanal makine oluşturma](media/create-vcenter-virtual-machine-01.png)

3. Seçin **yeni sanal makine oluşturma** tıklatıp **sonraki**.

    ![Yeni sanal makine Sihirbazı](media/create-vcenter-virtual-machine-02.png)

4. Adı seçin makinenin **iş yükü sanal makinenin** klasör seçeneğine tıklayıp **sonraki**.

    ![Ad ve klasör seçin](media/create-vcenter-virtual-machine-03.png)

5. Seçin **iş yükü** tıklayın ve işlem kaynağı **sonraki**.

    ![İşlem kaynağını seçin](media/create-vcenter-virtual-machine-04.png)

6. Seçin **vsanDatastore** tıklatıp **sonraki**.

    ![Depolama’yı seçme](media/create-vcenter-virtual-machine-05.png)

7. ESXi 6.5 uyumluluk varsayılan seçimini tutun ve tıklayın **sonraki**.

    ![Uyumluluk seçin](media/create-vcenter-virtual-machine-06.png)

8. ISO sanal makine için konuk işletim sistemi seçin ve tıklayın **sonraki**.

    ![Konuk işletim sistemi özelleştirme](media/create-vcenter-virtual-machine-07.png)

9. Sabit disk ve ağ seçeneklerini seçin. Yeni bir CD/DVD sürücüsü için seçin **veri deposu ISO dosyasını**.  Bu sanal makinenin genel IP adresinden gelen trafiğe izin vermek istiyorsanız, ağda şu şekilde seçin **vm-1**.

    ![Donanım özelleştirme seçin](media/create-vcenter-virtual-machine-08.png)

10. Bir seçim penceresi açılır. Daha önce yüklediğiniz Iso'lar ve şablonları klasöre dosya seçip tıklayın **Tamam**.

    ![ISO değerini seçin](media/create-vcenter-virtual-machine-10.png)

11. Ayarları gözden geçirin ve tıklayın **Tamam** sanal makine oluşturmak için.

    ![Seçenekleri gözden geçir](media/create-vcenter-virtual-machine-11.png)

Sanal makine iş yükü işlem kaynakları için artık eklenir ve kullanıma hazırdır. 

![Yeni bir sanal makine vcenter](media/create-vcenter-virtual-machine-12.png)

Temel kurulum tamamlanmıştır. Özel bulut, şirket içi sanal makine altyapısı nasıl kullanacağınız için benzer kullanmaya başlayabilirsiniz.

Sonraki bölümlerde, DNS ve DHCP sunucuları için özel bulut iş yükleri ayarlanıyor. ve varsayılan ağ yapılandırmasını değiştirme hakkında isteğe bağlı bilgiler içerir.

## <a name="add-users-and-identity-sources-to-vcenter-optional"></a>Kullanıcı ve kimlik kaynaklar (isteğe bağlı) vCenter Ekle

Kullanıcı adı varsayılan vCenter kullanıcı hesabıyla CloudSimple atar **cloudowner@cloudsimple.local** . Hiçbir ek hesap kurulumu kullanmaya başlamak gerekli değildir.  CloudSimple Yöneticiler, normal işlemleri gerçekleştirmek için ihtiyaç duydukları ayrıcalıkları normalde atar.  Şirket içi active directory veya Azure AD olarak ayarlamak bir [ek kimlik kaynağı](https://docs.azure.cloudsimple.com/set-vcenter-identity/) özel bulutunuzda.

## <a name="create-a-dns-and-dhcp-server-optional"></a>Bir DNS ve DHCP sunucusu oluşturma (isteğe bağlı)

Uygulamalar ve özel bulut ortamında çalışan iş yükleri, ad çözümlemesi ve DHCP hizmetlerinin arama ve IP adresi ataması gerektirir. Uygun bir DHCP ve DNS altyapısı, bu hizmetleri sağlamak için gereklidir. Bir sanal makine, özel bulut ortamınızda bu hizmetleri sağlamak için vcenter yapılandırabilirsiniz.

### <a name="prerequisites"></a>Önkoşullar

* Bir VLAN yapılandırılmış dağıtılmış bağlantı noktası grubuyla

* Şirket içi veya Internet tabanlı DNS sunucuları için ayarlanmış bir rota

* Sanal makine şablonunu veya sanal makine oluşturmak için ISO

Aşağıdaki bağlantılar, Linux ve Windows üzerinde DHCP ve DNS sunucularını ayarlama hakkında yönergeler sağlar.

### <a name="linux-based-dns-server-setup"></a>Linux tabanlı bir DNS Sunucusu Kurulumu

Linux, DNS sunucularını ayarlamak için çeşitli paketler sunar.  Bağlantı bir açık kaynak bağlama DNS sunucusunu ayarlama yönergeleri aşağıda verilmiştir.

[Örnek kurulumu](https://www.digitalocean.com/community/tutorials/how-to-configure-bind-as-a-private-network-dns-server-on-centos-7)

### <a name="windows-based-setup"></a>Windows tabanlı Kurulumu

Bu Microsoft makaleler, bir Windows sunucusuna bir DNS sunucusu ve DHCP sunucusu olarak ayarlama açıklanmaktadır.
<br>
[DNS sunucusu olarak Windows Server](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

[DHCP sunucusu olarak Windows Server](https://docs.microsoft.com/windows-server/networking/technologies/dhcp/dhcp-top)

## <a name="customize-networking-configuration-optional"></a>(İsteğe bağlı) ağ yapılandırmasını özelleştirme

Ağ sayfaları CloudSimple Portalı'nda güvenlik duvarı tablolar için yapılandırmayı ve sanal makineler için genel IP adresleri belirtmenizi sağlar.

### <a name="allocate-public-ips"></a>Genel IP'ler Ayır

1. Gidin **Ağ > genel IP** CloudSimple portalında.
2. Tıklayın **genel IP ayırma**.
3. IP adresi girişi tanımlamak için bir ad girin.
4. Özel bulut konumunu seçin.
5. Boşta kalma zaman aşımı isterseniz değiştirme için kaydırıcıyı kullanın.
6. Genel bir IP adresi atamak istediğiniz yerel IP adresi girin.
7. İstenirse, ilişkili bir DNS adını girin.
8. **Bitti**’ye tıklayın.

    ![Genel IP](media/quick-create-pc-public-ip.png)

Genel IP adresi ayırmayı görevini başlatır. Görev durumunu kontrol edebilirsiniz **etkinlik > görevleri** sayfası. Ayırma işlemi tamamlandığında, yeni giriş genel IP'ler sayfasında gösterilir.

Bu IP adresi eşlenmesi gereken sanal makine, yukarıda belirtilen yerel adresi ile yapılandırılması gerekir. Bir IP adresini yapılandırmak için sanal makine işletim sistemine özgü bir yordamdır. Sanal makine işletim sisteminiz için doğru yordamı için belgelere bakın.

#### <a name="example"></a>Örnek

Örneğin, Ubuntu 16.04 için ilişkin ayrıntılar aşağıdadır.

INet Adres ailesi yapılandırma dosyasında statik bir yöntem ekleyin ```/etc/network/interfaces```. Ağ geçidi adresi ve ağ maskesi değerleri değiştirin. Bu örnekte, eth0 arabirimi, iç IP adresi 192.168.24.10, ağ geçidi adresi 192.168.24.1 ve ağ maskesi 255.255.255.0 kullanıyoruz. 

Düzen ```interfaces``` dosya.

```
sudo vi /etc/network/interfaces
```

Aşağıdaki bölümde güncelleştirme ```interfaces``` dosya.

```
auto eth0
iface eth0 inet static
address 192.168.24.10
netmask 255.255.255.0
gateway 192.168.24.1
dns-nameservers 8.8.8.8
dns-domain acme.com
dns-search acme.com
```

Arabirim devre dışı bırakın.

```
sudo ifdown eth0
```

Arabirim yeniden etkinleştirin.

```
sudo ifup eth0
```

Varsayılan olarak, Internet'ten gelen tüm trafiği olan **reddedildi**. Başka bir bağlantı açmak istiyorsanız, oluşturun bir [güvenlik duvarı tablo](https://docs.azure.cloudsimple.com/firewall/).

Bir iç IP adresi statik IP adresi olarak yapılandırdıktan sonra Internet'ten sanal makine içinde erişebildiğini doğrulayın.

```
ping 8.8.8.8
```

Sanal makineyi Internet'ten genel IP adresini kullanarak erişebildiğini doğrulayın.

Sanal makinede herhangi bir güvenlik duvarı (iptable) kuralları 80 numaralı bağlantı noktasını engellemediğinden emin olun gelen.

```
netstat -an | grep 80
```

80 numaralı bağlantı noktasında dinleyen bir http sunucusu başlatın.
       
```
python2.7 -m SimpleHTTPServer 80
```

veya

```
python3 -m http.server 80
```

Sizin masaüstünüzde tarayıcıyı başlatın ve sanal makinenizde dosyalara göz atmak genel IP adresi için 80 numaralı bağlantı noktası. 

### <a name="default-cloudsimple-firewall-rules-for-public-ip"></a>Genel IP için varsayılan CloudSimple güvenlik duvarı kuralları

* VPN trafiği: (/ İçin) VPN ve tüm iş yükü ağlara ve yönetim ağı arasındaki tüm trafiğe izin verilir.
* Özel bulut iç trafiği: (/ İçin) iş yükü ağ ve (yukarıda gösterilmiştir) yönetim ağ arasındaki tüm Doğu-Batı trafiği izin verilir.
* Internet trafiğini:
    * Internet'ten gelen tüm trafiği, iş yükü ağları ve yönetim ağı için reddedildi.
    * İnternet'e iş yükü ağları veya yönetim ağına tüm giden trafiğe izin verilir.

Güvenlik duvarı kuralları özelliğini kullanarak trafiğinizi güvenli şekilde de değiştirebilirsiniz. Daha fazla bilgi için [güvenlik duvarı tablolar ve kuralları ayarlama](https://docs.azure.cloudsimple.com/firewall/).

## <a name="install-solutions-optional"></a>(İsteğe bağlı) çözümlerini yükleyin
VCenter ortam özel Bulutunuzdaki tüm avantajlarından yararlanmak için CloudSimple özel bulut üzerinde çözümler yükleyebilirsiniz. Sanal makinelerinizi korumak için yedekleme, olağanüstü durum kurtarma, çoğaltma ve diğer işlevleri ayarlayabilirsiniz. VMware Site kurtarma Yöneticisi'ni (VMware SRM) ve Veeam yedekleme ve çoğaltma örneklerindendir.

Çözümü yüklemek için sınırlı bir süre için ek ayrıcalıkları istemesi gerekir. Bkz: [ayrıcalıkları yükseltilmeye](https://docs.azure.cloudsimple.com/escalate-private-cloud-privileges).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure'da VMware sanal makinelerini kullanma](quickstart-create-vmware-virtual-machine.md)
* [Azure ExpressRoute kullanarak şirket içi ağa bağlanma](https://docs.azure.cloudsimple.com/on-premises-connection)
* [VPN ağ geçitleri CloudSimple ağdaki ayarlama](https://docs.azure.cloudsimple.com/vpn-gateway)
