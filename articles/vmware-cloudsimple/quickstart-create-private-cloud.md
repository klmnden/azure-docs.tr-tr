---
title: CloudSimple hızlı başlangıç - Azure VMware çözümüyle özel bulut oluşturma
description: Oluşturma ve Azure VMware CloudSimple çözümüyle ile özel bir bulut yapılandırma hakkında bilgi edinin
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 8a67cd2f82eb069555bda68e4cb04a6634e3b31d
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67164348"
---
# <a name="quickstart---configure-a-private-cloud-environment"></a>Hızlı Başlangıç - özel bir bulut ortamı yapılandırma

Bu makalede, CloudSimple özel bir bulut oluşturmak ve özel bulut ortamınızı ayarlama hakkında bilgi edinin.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-private-cloud"></a>Özel bulut oluşturma

1. **Tüm Hizmetler**’i seçin.
2. Arama **CloudSimple Hizmetleri**.
3. Özel bulut oluşturmak istediğiniz CloudSimple hizmeti seçin.
4. Genel Bakış'tan tıklayın **özel bulut oluşturma** CloudSimple portalı için yeni bir tarayıcı sekmesinde açmak için.  İstenirse, oturum, Azure'da oturum kimlik oturum açın.  

    ![Azure'dan özel bulut oluşturma](media/create-private-cloud-from-azure.png)

5. CloudSimple Portalı'nda özel bulut için bir ad sağlayın
6. Seçin **konumu** , özel bulutun
7. Seçin **düğüm türü** Azure'da sağlanan.  Seçebileceğiniz [CS28 veya CS36 seçeneği](cloudsimple-node.md#vmware-solution-by-cloudsimple-nodes-sku). İkinci seçeneği, en fazla işlem ve bellek kapasitesini içerir.
8. Belirtin **düğüm sayısı**.  Bir özel bulut oluşturmak için gereken en az üç düğüm

    ![Özel bulut - temel bilgilerini oluşturma](media/create-private-cloud-basic-info.png)

9. Tıklayın **sonraki: Gelişmiş Seçenekleri**.
10. VSphere/vSAN alt ağ için CIDR aralığı girin. CIDR aralığı herhangi bir şirket içi veya diğer Azure alt ağlar ile çakışmayacak emin olun.

    ![Gelişmiş Seçenekleri - özel bulut oluşturma](media/create-private-cloud-advanced-options.png)

11. Seçin **sonraki: Gözden geçir ve Oluştur**.
12. Ayarları gözden geçirin. Herhangi bir ayarı değiştirmeniz gerekiyorsa, tıklayın **önceki**.
13. **Oluştur**’a tıklayın.

Özel bulut sağlama işlemi başlatıldı.  Bu, en fazla sağlanacak özel bulut için iki saat sürebilir.

## <a name="launch-cloudsimple-portal"></a>CloudSimple portalını başlatma

Azure Portalı'ndan CloudSimple portalına erişebilmek için.  CloudSimple portalı ile Azure başlatılır kimlik çoklu oturum açma (SSO) kullanarak oturum açın.  CloudSimple portalına erişim gerektirir yetkilendirmek **CloudSimple hizmet yetkilendirme** uygulama.  İzinler verme daha fazla bilgi için bkz: [CloudSimple hizmet yetkilendirme uygulamaya onay](https://docs.azure.cloudsimple.com/access-cloudsimple-portal/#consent-to-cloudsimple-service-authorization-application)

1. **Tüm Hizmetler**’i seçin.
2. Arama **CloudSimple Hizmetleri**.
3. Özel bulut oluşturmak istediğiniz CloudSimple hizmeti seçin.
4. Genel Bakış'tan tıklayın **CloudSimple Portal'a** CloudSimple portalı için yeni bir tarayıcı sekmesinde açmak için.  İstenirse, oturum, Azure'da oturum kimlik oturum açın.  

    ![CloudSimple portalını başlatma](media/launch-cloudsimple-portal.png)

## <a name="create-point-to-site-vpn"></a>Noktadan siteye VPN oluşturma

Noktadan siteye VPN bağlantısı, özel bulut için bilgisayarınızdan bağlanmak için en basit yoludur. Özel bulutta uzaktan bağlanıyorsanız, noktadan siteye VPN bağlantısı kullanın.  Özel bulut, hızlı erişim için aşağıdaki adımları izleyin.  Şirket içi ağınızdan CloudSimple bölgeye erişim yapılabilir kullanarak [siteden siteye VPN](https://docs.azure.cloudsimple.com/vpn-gateway/) veya [Azure ExpressRoute](https://docs.azure.cloudsimple.com/on-premises-connection/).

### <a name="create-gateway"></a>Ağ geçidi oluşturma

1. Portal ve select CloudSimple başlatma **ağ**.
2. Seçin **VPN ağ geçidi**.
3. Tıklayın **yeni VPN ağ geçidi**.

    ![VPN ağ geçidi oluşturma](media/create-vpn-gateway.png)

4. İçin **ağ geçidi Yapılandırması**, aşağıdaki ayarları belirtin ve tıklayın **sonraki**.

    * Seçin **noktadan siteye VPN** ağ geçidi türü.
    * Ağ geçidi tanımlamak için bir ad girin.
    * CloudSimple hizmetinizin dağıtıldığı Azure konumu seçin.
    * Noktadan siteye ağ geçidi için istemci ile ilgili alt ağ belirtin.  Bağlandığınızda bu alt ağdan DHCP adresi verilir.

5. İçin **kullanıcı bağlantı**, aşağıdaki ayarları belirtin ve tıklayın **sonraki**.

    * Otomatik olarak özel buluta bu noktadan siteye ağ geçidi üzerinden erişmek tüm mevcut ve gelecekteki kullanıcılara izin vermek için seçin **tüm kullanıcılarını otomatik olarak Ekle**. Bu seçeneği belirlediğinizde, kullanıcı listesindeki tüm kullanıcıları otomatik olarak seçilir. Bireysel kullanıcılar listesinden kaldırarak otomatik seçeneğini geçersiz kılabilirsiniz.
    * Yalnızca bireysel kullanıcılar için kullanıcı listesini onay kutularına tıklayın.

6. VLAN'lar/alt ağlar bölümü, yönetim ve kullanıcı VLAN'lar/alt ağlar için bağlantıları ve ağ geçidi belirtmenize olanak sağlar.

    * **Otomatik olarak Ekle** bu ağ geçidi için genel ilke seçeneklerini ayarlayın. Ayarlar, geçerli ağ geçidi için geçerlidir. Ayarları kılınabilir **seçin** alan.
    * Seçin **ekleme VLAN'lar/alt ağlar özel bulut Yönetimi**. 
    * Kullanıcı tanımlı VLAN'lar/alt ağlar eklemek için tıklatın **kullanıcı tanımlı VLAN'lar/alt ağlar eklemek**. 
    * **Seçin** ayarlarını geçersiz kılmak, genel ayarlar altında **otomatik olarak Ekle**. 

7. Tıklayın **sonraki** ayarları gözden geçirin. Değişiklik yapmak için Düzenle simgeleri tıklatın.
8. Tıklayın **Oluştur** VPN ağ geçidi oluşturmak için.

### <a name="connect-to-cloudsimple-using-point-to-site-vpn"></a>İçin CloudSimple noktadan siteye VPN kullanarak bağlanma

VPN istemcisi için CloudSimple bilgisayarınızdan bağlanmak için gereklidir.  İndirme [OpenVPN istemci](https://openvpn.net/community-downloads/) Windows için veya [ilişkili Akışkanlık](https://www.sparklabs.com/viscosity/download/) macOS ve OS X için.

1. Portal ve select CloudSimple başlatma **ağ**.
2. Seçin **VPN ağ geçidi**.
3. VPN ağ geçitleri listesinden noktadan siteye VPN ağ geçidi'ni tıklatın.
4. Seçin **kullanıcılar**.
5. Tıklayarak **my VPN yapılandırmasını indir**

    ![VPN yapılandırmasını indirme](media/download-p2s-vpn-configuration.png)

6. İçeri aktarma üzerinde VPN istemcinizi yapılandırma

    * Yönergeler için [Windows istemci yapılandırmasını alma](https://openvpn.net/vpn-server-resources/connecting-to-access-server-with-windows/#openvpn-open-source-openvpn-gui-program)
    * Yönergeler için [macOS ya da OS x'te yapılandırmasını alma](https://www.sparklabs.com/support/kb/article/getting-started-with-viscosity-mac/#creating-your-first-connection)

7. İçin CloudSimple bağlanma

## <a name="create-a-vlan-for-your-workload-vms"></a>İş yükünüz olan VM'ler için bir VLAN'ı oluşturma

Özel bulut oluşturduktan sonra iş yükü/uygulama sanal makineleri dağıtacağınız bir VLAN oluşturun.

1. CloudSimple portalında **ağ**.
2. Tıklayın **VLAN/alt ağlar**.
3. Tıklayın **VLAN/alt ağ oluşturma**

    ![VLAN/alt ağ oluşturma](media/create-new-vlan-subnet.png)

4. Seçin **özel bulut** yeni VLAN/alt ağ için.
5. Bir VLAN kimliği listeden seçin.  
6. Alt ağ tanımlamak için bir alt ağ adı girin.
7. Maske ve alt ağ CIDR aralığı belirtin.  Bu aralık mevcut hiçbir alt ağ ile çakışmaması gerekir.
8. **Gönder**'e tıklayın.

    ![VLAN/alt ağ ayrıntıları oluşturma](media/create-new-vlan-subnet-details.png)

VLAN/alt ağ oluşturulur.  Artık bu VLAN kimliği üzerinde özel bulut vCenter'ınıza bir dağıtılmış bağlantı noktası grubu oluşturmak için de kullanabilirsiniz. 

## <a name="connect-your-environment-to-an-azure-virtual-network"></a>Ortamınızı bir Azure sanal ağına bağlama

CloudSimple ile bir ExpressRoute bağlantı hattı için özel bulutunuzun sağlar. ExpressRoute bağlantı hattı için Azure sanal ağınızda bağlanabilirsiniz. Bağlantı kurma hakkında tam Ayrıntılar için adımları [Azure sanal ağı ExpressRoute kullanarak bağlantı](https://docs.azure.cloudsimple.com/cloudsimple-azure-network-connection/)

## <a name="sign-in-to-vcenter"></a>VCenter'ı açın

Artık vCenter sanal makineler ve ilkeleri ayarlamak oturum açabilir.

1. VCenter erişmeye CloudSimple portaldan başlatın. Giriş sayfasında altında **ortak görevleri**, tıklayın **vSphere istemci başlatma**.  Özel Bulutu seçin ve ardından **vSphere istemci başlatma** özel bulutta.

    ![VSphere İstemcisi'ni başlatma](media/launch-vcenter-from-cloudsimple-portal.png)

2. VCenter erişip bunları, kullanıcı adı ve parolanızla oturum için tercih edilen vSphere istemcinizi seçin.  Varsayılan değerler şunlardır:
    * Kullanıcı adı: **CloudOwner@cloudsimple.local**
    * Parola: **CloudSimple123!**  

VSphere (HTML5) istemciden sonraki yordamlarda vCenter ekranlardır.

## <a name="change-your-vcenter-password"></a>VCenter parolanızı değiştirme

CloudSimple ilk kez vCenter oturum yöneticiniz parolanızı değiştirmenizi önerir.  
Belirlediğiniz parola aşağıdaki gereksinimleri karşılaması gerekir:

* Maksimum ömrü: Parola 365 günde bir değiştirilmesi
* Yeniden kısıtla: Kullanıcılar herhangi bir önceki beş parolaları yeniden kullanılamaz.
* Uzunluk: 8 - 20 karakter
* Özel karakter: En az bir özel karakter
* Alfabetik karakter sayısı: En az bir büyük harf karakter, A-Z ve en az bir küçük harf karakter, a-z
* Sayılar: En az bir sayısal karakteri, 0-9
* En fazla özdeş bitişik karakter: Üç

    Örnek: Bilgi veya Kutulardaki parola bir parçası olarak kabul edilebilir olmakla birlikte CCCC değil.

Bir parola ayarlarsanız, gereksinimleri karşılamıyor:

* vSphere istemci Flash kullanırsanız, bir hata bildirir.
* HTML5 istemci kullanırsanız, bir hata bildirmez. İstemci değişikliği kabul etmez ve eski parolayı çalışmaya devam eder.

## <a name="change-nsx-administrator-password"></a>NSX yönetici parolasını değiştirme

Varsayılan parola NSX manager dağıtılır.  Özel bulut oluşturduktan sonra parolayı değiştirmesi önerilir.

   * Kullanıcı adı: **yönetici**
   * Parola: **CloudSimple123!**

Tam etki alanı adı (FQDN) ve IP adresi NSX Manager CloudSimple portalında bulabilirsiniz.

1. Portal ve select CloudSimple başlatma **kaynakları**.
2. Kullanmak istediğiniz özel buluta üzerinde tıklayın.
3. Seçin **vSphere yönetim ağı**
4. FQDN veya IP adresini kullanın **NSX Manager** ve bir web tarayıcısı kullanarak bağlanın. 

    ![NSX Manager FQDN Bul](media/private-cloud-nsx-manager-fqdn.png)

Parolayı değiştirmek için yönergeleri izleyin. [NSX Manager yüklemesi](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/2.2/com.vmware.nsxt.install.doc/GUID-A65FE3DD-C4F1-47EC-B952-DEDF1A3DD0CF.html).

## <a name="create-a-port-group"></a>Bir bağlantı noktası grubu oluştur

VSphere dağıtılmış bağlantı noktası grubu oluşturmak için:

1. "Bir dağıtılmış bağlantı noktası Grup Ekle"'ndaki yönergeleri takip edin [vSphere Ağ Kılavuzu](https://docs.vmware.com/en/VMware-vSphere/6.5/vsphere-esxi-vcenter-server-65-networking-guide.pdf).
2. Dağıtılmış bir bağlantı noktası grubu ayarlama oluşturduğunuz VLAN kimliği belirtin [iş yükü Vm'leriniz için bir VLAN oluşturma](#create-a-vlan-for-your-workload-vms).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure'da VMware sanal makinelerini kullanma](https://docs.azure.cloudsimple.com/quickstart-create-vmware-virtual-machine)
* [Azure'da VMware sanal makinelerini kullanma](quickstart-create-vmware-virtual-machine.md)
* [Azure ExpressRoute kullanarak şirket içi ağa bağlanma](https://docs.azure.cloudsimple.com/on-premises-connection/)
* [Şirket içi siteden siteye VPN Kurulumu](https://docs.azure.cloudsimple.com/vpn-gateway/)
