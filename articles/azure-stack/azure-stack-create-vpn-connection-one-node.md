---
title: "Farklı Azure yığın Geliştirme Seti ortamlarda iki sanal ağ arasında bir siteden siteye VPN bağlantısı oluşturun | Microsoft Docs"
description: "Tek düğümlü Azure yığın Geliştirme Seti için iki ortam arasında bir siteden siteye VPN bağlantısı oluşturmak için bir bulut yöneticisinin kullandığı adım adım yordam."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 3f1b4e02-dbab-46a3-8e11-a777722120ec
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 7/10/2017
ms.author: brenduns
ms.reviewer: scottnap
ms.openlocfilehash: 886d56169c5500c9175b7ddc43edfc29c5142fbb
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-site-to-site-vpn-connection-between-two-virtual-networks-in-different-azure-stack-development-kit-environments"></a>Farklı Azure yığın Geliştirme Seti ortamlarda iki sanal ağ arasında bir siteden siteye VPN bağlantısı oluşturun
## <a name="overview"></a>Genel Bakış
Bu makalede iki ayrı Azure yığın Geliştirme Seti ortamlarda iki sanal ağ arasında bir siteden siteye VPN bağlantısı oluşturulacağını gösterir. Bağlantı yapılandırması sırasında VPN ağ geçitleri Azure yığınında nasıl çalıştığını öğrenin.

### <a name="connection-diagram"></a>Bağlantı diyagramı
Aşağıdaki diyagramda, işiniz bittiğinde bağlantı yapılandırması gibi görünmelidir gösterir.

![Siteden siteye VPN bağlantısı yapılandırma](media/azure-stack-create-vpn-connection-one-node-tp2/OneNodeS2SVPN.png)

### <a name="before-you-begin"></a>Başlamadan önce
Bağlantı yapılandırmasını tamamlamak için başlamadan önce aşağıdaki öğelerin bulunduğundan emin olun:

* Tarafından tanımlanan Azure yığın Geliştirme Seti donanım gereksinimlerini karşılayan iki sunucu [Azure yığın dağıtımının önkoşulları](azure-stack-deploy.md). Görünen diğer önkoşulları emin [makale](azure-stack-deploy.md) çok karşılandıktan.
* [Azure yığın Geliştirme Seti](https://azure.microsoft.com/en-us/overview/azure-stack/try/) dağıtım paketi.

## <a name="deploy-the-azure-stack-development-kit-environments"></a>Azure yığın Geliştirme Seti ortamları dağıtma
Bağlantı yapılandırmasını tamamlamak için iki Azure yığın Geliştirme Seti ortamını dağıtmanız gerekir.
> [!NOTE] 
> Her Azure yığın geliştirme dağıttığınız Seti, izleyin [dağıtım yönergeleri](azure-stack-run-powershell-script.md). Bu makalede, Azure yığın Geliştirme Seti ortamları denir *POC1* ve *POC2*.


## <a name="prepare-an-offer-on-poc1-and-poc2"></a>POC1 ve POC2 bir teklif hazırlama
Bir kullanıcı için teklif abone olmak ve sanal makineleri dağıtmak için hem POC1 hem de POC2 bir teklif hazırlayın. Bir teklif oluşturma hakkında daha fazla bilgi için bkz: [sanal makineler kullanılabilir duruma Azure yığın kullanıcılarınıza](azure-stack-tutorial-tenant-vm.md).

## <a name="review-and-complete-the-network-configuration-table"></a>Gözden geçirin ve ağ yapılandırması tablosu tamamlayın
Aşağıdaki tabloda her iki Azure yığın Geliştirme Seti ortamlar için ağ yapılandırması özetler. Ağınız için belirli dış BGPNAT adresi eklemek için tablodan sonra görünür yordamı kullanın.

**Ağ yapılandırma tablosu**
|   |POC1|POC2|
|---------|---------|---------|
|Sanal ağ adı     |VNET-01|VNET-02 |
|Sanal ağ adres alanı |10.0.10.0/23|10.0.20.0/23|
|Alt ağ adı     |Alt ağ-01|Subnet-02|
|Alt ağ adres aralığı|10.0.10.0/24 |10.0.20.0/24 |
|Ağ geçidi alt ağı     |10.0.11.0/24|10.0.21.0/24|
|Dış BGPNAT adresi     |         |         |

> [!NOTE]
> Örnek ortamında dış BGPNAT IP adresleri POC1 10.16.167.195 ve POC2 10.16.169.131 ' dir. Azure yığın Geliştirme Seti konakları için dış BGPNAT IP adreslerini belirlemek için aşağıdaki yordamı kullanın ve sonra önceki ağ yapılandırması tabloya ekleyin.


### <a name="get-the-ip-address-of-the-external-adapter-of-the-nat-vm"></a>NAT VM dış bağdaştırıcısının IP adresini alma
1. Azure yığın fiziksel makine POC1 için oturum açın.
2. Yönetici parolanızı değiştirmek için aşağıdaki Powershell kodu düzenleyin ve ardından kod POC ana bilgisayarda çalıştırın:

   ```powershell
   cd \AzureStack-Tools-master\connect
   Import-Module .\AzureStack.Connect.psm1
   $Password = ConvertTo-SecureString "<your administrator password>" `
    -AsPlainText `
    -Force
   Get-AzureStackNatServerAddress `
    -HostComputer "AzS-bgpnat01" `
    -Password $Password
   ```
3. Önceki bölümde görünür ağ yapılandırması tablosu IP adresini ekleyin.

4. POC2 bu yordamı yineleyin.

## <a name="create-the-network-resources-in-poc1"></a>Ağ kaynakları POC1 içinde oluşturma
Şimdi, ağ geçitleri ayarlamanız gerekir POC1 ağ kaynaklarını oluşturun. Aşağıdaki yönergeler, Kullanıcı Portalı'nı kullanarak kaynakların oluşturulacağını gösterir. PowerShell kodu, kaynak oluşturmak için de kullanabilirsiniz.

![Kaynak oluşturmak için kullanılan iş akışı](media/azure-stack-create-vpn-connection-one-node-tp2/image2.png)

### <a name="sign-in-as-a-tenant"></a>Bir kiracı olarak oturum açın
Hizmet Yöneticisi bir kiracı planları, teklifleri ve kiracıları kullanabilir abonelikleri test etmek için oturum açabilirsiniz. Zaten yoksa, [bir kiracı hesap oluşturmak](azure-stack-add-new-user-aad.md) oturum açmadan önce.

### <a name="create-the-virtual-network-and-vm-subnet"></a>Sanal ağ ve VM alt ağı oluşturma
1. Kullanıcı portalında oturum açmak için bir kiracı hesabını kullanın.
2. Kullanıcı Portalı'nda seçin **yeni**.

    ![Yeni sanal ağ oluştur](media/azure-stack-create-vpn-connection-one-node-tp2/image3.png)

3. Git **Market**ve ardından **ağ**.
4. Seçin **sanal ağ**.
5. İçin **adı**, **adres alanı**, **alt ağ adı**, ve **alt ağ adres aralığı**, ağda görünen değerlerini kullanın Yapılandırma tablo.
6. İçinde **abonelik**, daha önce oluşturduğunuz aboneliği görünüyor.
7. İçin **kaynak grubu**, bir kaynak grubu oluşturabilir veya zaten bir varsa seçin **var olanı kullan**.
8. Varsayılan konumu doğrulayın.
9. **Panoya sabitle**’yi seçin.
10. **Oluştur**’u seçin.

### <a name="create-the-gateway-subnet"></a>Ağ geçidi alt ağını oluşturma
1. Panoda daha önce oluşturduğunuz VNET-01 sanal ağ kaynağı açın.
2. **Ayarlar** dikey penceresinde **Alt ağlar**’ı seçin.
3. Sanal ağ için ağ geçidi alt ağı eklemek için seçin **ağ geçidi alt ağı**.
   
    ![Ağ geçidi alt ağı Ekle](media/azure-stack-create-vpn-connection-one-node-tp2/image4.png)

4. Varsayılan olarak, alt ağ adı ayarlamak **GatewaySubnet**.
   Ağ geçidi alt ağları özel. Düzgün çalışması için bunlar kullanmalısınız *GatewaySubnet* adı.
5. İçinde **adres aralığı**, adresi olduğunu doğrulayın **10.0.11.0/24**.
6. Seçin **Tamam** ağ geçidi alt ağı oluşturmak için.

### <a name="create-the-virtual-network-gateway"></a>Sanal ağ geçidini oluşturma
1. Azure portalında seçin **yeni**. 
2. Git **Market**ve ardından **ağ**.
3. Ağ kaynakları listesinden seçin **sanal ağ geçidi**.
4. İçinde **adı**, girin **GW1**.
5. Seçin **sanal ağ** öğesi bir sanal ağ seçin.
   Seçin **VNET-01** listeden.
6. Seçin **genel IP adresi** menü öğesi. Zaman **genel IP adresi seçin** dikey penceresi açılır ve select **Yeni Oluştur**.
7. İçinde **adı**, girin **GW1 PIP**ve ardından **Tamam**.
8.  Varsayılan olarak, için **VPN türü**, **rota tabanlı** seçilir.
    Tutmak **rota tabanlı** VPN türü.
9. **Abonelik** ve **Konum** seçeneklerinin doğruluğunu onaylayın. Kaynak panoya sabitleyebilirsiniz. **Oluştur**’u seçin.

### <a name="create-the-local-network-gateway"></a>Yerel ağ geçidini oluşturma
Bu Azure Stack değerlendirme dağıtımındaki *yerel ağ geçidi* uygulaması, gerçek bir Azure dağıtımındaki uygulamadan biraz farklıdır.

Bir Azure dağıtımında azure'da bir sanal ağ geçidi bağlanmak için kullandığı bir şirket içi (en Kiracı) fiziksel aygıt, bir yerel ağ geçidi temsil eder. Bu Azure yığın değerlendirme dağıtımda bağlantının her iki ucuna sanal ağ geçitlerini olan!

Bu konuda daha genel düşünmek için bir yerel ağ geçidi kaynağı her zaman bağlantısının diğer ucundaki uzak ağ geçidi belirten yoludur. Azure yığın Geliştirme Seti tasarlandığı şekilde nedeniyle, ağ adresi çevirisi (NAT) diğer Azure yığın geliştirme Seti'nin genel IP adresi VM üzerinde dış ağ bağdaştırıcısının IP adresi verin yerel ağ geçidi gerekir. Ardından her iki ucuna düzgün şekilde bağlandığından emin olmak için NAT VM'de NAT eşlemeleri oluşturma.


### <a name="create-the-local-network-gateway-resource"></a>Yerel ağ geçidi kaynağı oluşturma
1. Azure yığın fiziksel makine POC1 için oturum açın.
2. Kullanıcı Portalı'nda seçin **yeni**.
3. Git **Market**ve ardından **ağ**.
4. Kaynakların listesinden **yerel ağ geçidi**.
5. İçinde **adı**, girin **POC2-GW**.
6. İçinde **IP adresi**, POC2 için dış BGPNAT adresini girin. Bu adres, daha önce ağ yapılandırması tablosunda görünür.
7. İçinde **adres alanı**, daha sonra oluşturmayı POC2 VNET adres alanı için girin **10.0.20.0/23**.
8. Doğrulayın, **abonelik**, **kaynak grubu**, ve **konumu** doğru olduğunu onaylayın ve ardından **oluşturma**.

### <a name="create-the-connection"></a>Bağlantı oluşturma
1. Kullanıcı Portalı'nda seçin **yeni**.
2. Git **Market**ve ardından **ağ**.
3. Kaynakların listesinden **bağlantı**.
4. Üzerinde **Temelleri** dikey penceresinde, için **bağlantı türü**seçin **siteden siteye (IPSec)**.
5. Seçin **abonelik**, **kaynak grubu**, ve **konumu**ve ardından **Tamam**.
6. Üzerinde **ayarları** dikey penceresinde, select **sanal ağ geçidi**ve ardından **GW1**.
7. Seçin **yerel ağ geçidi**ve ardından **POC2-GW**.
8. İçinde **bağlantı adı**, girin **POC1 POC2**.
9. İçinde **paylaşılan anahtar (PSK)**, girin **12345**ve ardından **Tamam**.
10. Üzerinde **Özet** dikey penceresinde, select **Tamam**.

### <a name="create-a-vm"></a>VM oluşturma
VPN bağlantısı üzerinden geçen verileri doğrulamak için her Azure yığın Development Kit'te veri alıp göndermek için sanal makinelerin gerekir. Bir sanal makine içinde POC1 şimdi oluşturmak ve ardından VM alt ağında alın, sanal ağınızda bulunan.

1. Azure portalında seçin **yeni**.
2. Git **Market**ve ardından **işlem**.
3. Sanal makine görüntülerini listesinde seçin **Windows Server 2016 Datacenter Eval** görüntü.
4. Üzerinde **Temelleri** dikey penceresindeki **adı**, girin **VM01**.
5. Geçerli kullanıcı adı ve parola girin. Oluşturulduktan sonra VM oturum açmak için bu hesabı kullanın.
6. Sağlayan bir **abonelik**, **kaynak grubu**, ve **konumu**ve ardından **Tamam**.
7. Üzerinde **boyutu** dikey penceresinde, bu örnek için bir sanal makine boyutu seçin ve ardından **seçin**.
8. Üzerinde **ayarları** dikey penceresinde Varsayılanları kabul edin. Emin **VNET-01** sanal ağın seçili. Alt ağ değerine ayarlandığını doğrulayın **10.0.10.0/24**. Ardından **Tamam**.
9. Üzerinde **Özet** dikey penceresinde, ayarları gözden geçirin ve ardından **Tamam**.



## <a name="create-the-network-resources-in-poc2"></a>Ağ kaynakları POC2 içinde oluşturma

Sonraki adım, ağ kaynakları için POC2 oluşturmaktır. Aşağıdaki yönergeler Kullanıcı Portalı'nı kullanarak kaynakların oluşturulacağını gösterir.

### <a name="sign-in-as-a-tenant"></a>Bir kiracı olarak oturum açın
Hizmet Yöneticisi bir kiracı planları, teklifleri ve kiracıları kullanabilir abonelikleri test etmek için oturum açabilirsiniz. Zaten yoksa, [bir kiracı hesap oluşturmak](azure-stack-add-new-user-aad.md) oturum açmadan önce.

### <a name="create-the-virtual-network-and-vm-subnet"></a>Sanal ağ ve VM alt ağı oluşturma

1. Bir kiracı hesabını kullanarak oturum açın.
2. Kullanıcı Portalı'nda seçin **yeni**.
3. Git **Market**ve ardından **ağ**.
4. Seçin **sanal ağ**.
5. POC2 için değerleri tanımlamak için daha önce ağ yapılandırması tabloda görünen bilgileri kullanın **adı**, **adres alanı**, **alt ağ adı**ve **Alt ağ adres aralığı**.
6. İçinde **abonelik**, daha önce oluşturduğunuz aboneliği görünüyor.
7. İçin **kaynak grubu**, yeni bir kaynak grubu oluşturun veya zaten varsa, seçin **var olanı kullan**.
8. Varsayılan doğrulama **konumu**.
9. **Panoya sabitle**’yi seçin.
10. **Oluştur**’u seçin.

### <a name="create-the-gateway-subnet"></a>Ağ Geçidi Alt Ağı oluşturma
1. Oluşturduğunuz sanal ağ kaynağı açın (**VNET-02**) panodan.
2. **Ayarlar** dikey penceresinde **Alt ağlar**’ı seçin.
3. Seçin **ağ geçidi alt ağı** sanal ağa bir ağ geçidi alt ağı eklemek için.
4. Alt ağın adı varsayılan olarak **GatewaySubnet** şeklinde ayarlanır.
   Ağ geçidi alt ağları özeldir ve düzgün şekilde çalışabilmesi için bu ada sahip olmalıdır.
5. İçinde **adres aralığı** alan, adresi doğrulayın **10.0.21.0/24**.
6. Seçin **Tamam** ağ geçidi alt ağı oluşturmak için.

### <a name="create-the-virtual-network-gateway"></a>Sanal ağ geçidini oluşturma
1. Azure portalında seçin **yeni**.  
2. Git **Market**ve ardından **ağ**.
3. Ağ kaynakları listesinden seçin **sanal ağ geçidi**.
4. İçinde **adı**, girin **GW2**.
5. Bir sanal ağ seçmek için Seç **sanal ağ**. Ardından **VNET-02** listeden.
6. Seçin **genel IP adresi**. Zaman **genel IP adresi seçin** dikey penceresi açılır ve select **Yeni Oluştur**.
7. İçinde **adı**, girin **GW2 PIP**ve ardından **Tamam**.
8. Varsayılan olarak, için **VPN türü**, **rota tabanlı** seçilir.
    Tutmak **rota tabanlı** VPN türü.
9. **Abonelik** ve **Konum** seçeneklerinin doğruluğunu onaylayın. Kaynak panoya sabitleyebilirsiniz. **Oluştur**’u seçin.

### <a name="create-the-local-network-gateway-resource"></a>Yerel ağ geçidi kaynağı oluşturma

1. POC2 Kullanıcı Portalı'nda seçin **yeni**. 
4. Git **Market**ve ardından **ağ**.
5. Kaynakların listesinden **yerel ağ geçidi**.
6. İçinde **adı**, girin **POC1-GW**.
7. İçinde **IP adresi**, daha önce ağ yapılandırması tabloda listelenen POC1 dış BGPNAT adresini girin.
8. İçinde **adres alanı**, POC1 girin **10.0.10.0/23** adres alanı **VNET-01**.
9. Doğrulayın, **abonelik**, **kaynak grubu**, ve **konumu** doğru olduğunu onaylayın ve ardından **oluşturma**.

## <a name="create-the-connection"></a>Bağlantı oluşturma
1. Kullanıcı Portalı'nda seçin **yeni**. 
2. Git **Market**ve ardından **ağ**.
3. Kaynakların listesinden **bağlantı**.
4. Üzerinde **temel** dikey penceresinde, için **bağlantı türü**, seçin **siteden siteye (IPSec)**.
5. Seçin **abonelik**, **kaynak grubu**, ve **konumu**ve ardından **Tamam**.
6. Üzerinde **ayarları** dikey penceresinde, select **sanal ağ geçidi**ve ardından **GW2**.
7. Seçin **yerel ağ geçidi**ve ardından **POC1-GW**.
8. İçinde **bağlantı adı**, girin **POC2 POC1**.
9. İçinde **paylaşılan anahtar (PSK)**, girin **12345**. Farklı bir değer seçerseniz, bu BT unutmayın *gerekir* POC1 üzerinde oluşturulan paylaşılan anahtarı için değer ile eşleşmelidir. **Tamam**’ı seçin.
10. Gözden geçirme **Özet** dikey ve ardından **Tamam**.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma
Bir sanal makine içinde POC2 şimdi oluşturmak ve sanal ağınızda VM alt ağınız üzerinde yerleştirin.

1. Azure portalında seçin **yeni**.
2. Git **Market**ve ardından **işlem**.
3. Sanal makine görüntülerini listesinde seçin **Windows Server 2016 Datacenter Eval** görüntü.
4. Üzerinde **Temelleri** dikey penceresinde için **adı**, girin **VM02**.
5. Geçerli kullanıcı adı ve parola girin. Oluşturulduktan sonra sanal makinede oturum açmak için bu hesabı kullanın.
6. Sağlayan bir **abonelik**, **kaynak grubu**, ve **konumu**ve ardından **Tamam**.
7. Üzerinde **boyutu** bir sanal makine boyutu bu örneği için ve ardından dikey penceresinde, select **seçin**.
8. Üzerinde **ayarları** dikey penceresinde Varsayılanları kabul edebilir. Emin **VNET-02** sanal ağ seçilir ve alt ağ değerine ayarlandığını doğrulayın **10.0.20.0/24**. **Tamam**’ı seçin.
9. Ayarları gözden **Özet** dikey ve ardından **Tamam**.

## <a name="configure-the-nat-virtual-machine-on-each-azure-stack-development-kit-for-gateway-traversal"></a>Her Azure yığın Geliştirme Seti ağ geçidi geçişi için NAT sanal makine yapılandırma
Azure yığın Geliştirme Seti bağımsızdır ve fiziksel ana bilgisayar dağıtıldığı, ağdan yalıtılmış olduğundan *dış* ağ geçitleri bağlı VIP ağ aslında dış değil. Bunun yerine, VIP ağ ağ adresi çevirisi gerçekleştirir bir yönlendirici gizlenir. 

Adlı bir Windows Server sanal makine yönlendiricisidir *AzS bgpnat01*, Yönlendirme ve Uzaktan Erişim Hizmetleri (RRAS) rol Azure yığın Geliştirme Seti altyapısında çalışır. NAT AzS bgpnat01 sanal makinedeki her iki ucunun bağlanmak siteden siteye VPN bağlantısı izin verecek şekilde yapılandırmanız gerekir. 

VPN bağlantısı yapılandırmak için sınır ağ geçidi havuzu VIP için dış arabirimi BGPNAT sanal makinede eşlemeleri statik bir NAT eşleme yol oluşturmanız gerekir. Her bağlantı noktası bir VPN bağlantısı için statik bir NAT eşleme yolu gereklidir.

> [!NOTE]
> Bu yapılandırma, yalnızca Azure yığın Geliştirme Seti ortamları için gereklidir.
> 
> 

### <a name="configure-the-nat"></a>NAT yapılandırın
> [!IMPORTANT]
> Bu yordam için tamamlamanız gereken *her ikisi de* Azure yığın Geliştirme Seti ortamları.

1. Belirlemek **iç IP adresi** aşağıdaki PowerShell Betiği kullanmak için. Sanal ağ geçidi (GW1 ve GW2) açın ve ardından **genel bakış** değeri kaydetme dikey **genel IP adresi** daha sonra kullanmak için.
![İç IP adresi](media/azure-stack-create-vpn-connection-one-node-tp2/InternalIP.PNG)
2. Azure yığın fiziksel makine POC1 için oturum açın.
3. Kopyalayın ve aşağıdaki PowerShell betiğini düzenleyin. Her Azure yığın Geliştirme Seti NAT yapılandırmak için yükseltilmiş bir Windows PowerShell ISE komut dosyasını çalıştırın. Komut dosyasında değerler ekleyin *dış BGPNAT adresi* ve *iç IP adresi* yer tutucuları:

   ```powershell
   # Designate the external NAT address for the ports that use the IKE authentication.
   Invoke-Command `
    -ComputerName AzS-bgpnat01 `
     {Add-NetNatExternalAddress `
      -NatName BGPNAT `
      -IPAddress <External BGPNAT address> `
      -PortStart 499 `
      -PortEnd 501}
   Invoke-Command `
    -ComputerName AzS-bgpnat01 `
     {Add-NetNatExternalAddress `
      -NatName BGPNAT `
      -IPAddress <External BGPNAT address> `
      -PortStart 4499 `
      -PortEnd 4501}
   # create a static NAT mapping to map the external address to the Gateway
   # Public IP Address to map the ISAKMP port 500 for PHASE 1 of the IPSEC tunnel
   Invoke-Command `
    -ComputerName AzS-bgpnat01 `
     {Add-NetNatStaticMapping `
      -NatName BGPNAT `
      -Protocol UDP `
      -ExternalIPAddress <External BGPNAT address> `
      -InternalIPAddress <Internal IP address> `
      -ExternalPort 500 `
      -InternalPort 500}
   # Finally, configure NAT traversal which uses port 4500 to
   # successfully establish the complete IPSEC tunnel over NAT devices
   Invoke-Command `
    -ComputerName AzS-bgpnat01 `
     {Add-NetNatStaticMapping `
      -NatName BGPNAT `
      -Protocol UDP `
      -ExternalIPAddress <External BGPNAT address> `
      -InternalIPAddress <Internal IP address> `
      -ExternalPort 4500 `
      -InternalPort 4500}
   ```

4. POC2 bu yordamı yineleyin.

## <a name="test-the-connection"></a>Bağlantıyı sınama
Siteden siteye bağlantı kuran, üzerinden akan trafik alabilir doğrulamalıdır. Doğrulamak için bir ya da Azure yığın Geliştirme Seti ortamında oluşturulan sanal makinelerin oturum açın. Ardından, diğer ortamında oluşturduğunuz sanal makine ping işlemi uygulayın. 

Siteden siteye bağlantı üzerinden trafiği gönderme emin olmak için uzak alt VIP sanal makineye doğrudan IP'yi (DIP) adresini ping emin olun. Bunu yapmak için diğer uçtaki bağlantının DIP adresi bulun. Daha sonra kullanmak için adresi kaydedin.

### <a name="sign-in-to-the-tenant-vm-in-poc1"></a>Kiracı VM POC1 ile oturum açın
1. Azure yığın fiziksel makine POC1 için oturum açın ve ardından kullanıcı portalında oturum açmak için bir kiracı hesabını kullanın.
2. Sol gezinti çubuğunda seçin **işlem**.
3. Sanal makineleri listesinde bulun **VM01** daha önce oluşturduğunuz ve ardından seçin.
4. Sanal makine için dikey penceresinde **Bağlan**ve VM01.rdp dosyasını açın.
   
     ![Düğme Bağlan](media/azure-stack-create-vpn-connection-one-node-tp2/image17.png)
5. Sanal makine oluşturduğunuzda, yapılandırdığınız hesabıyla oturum açın.
6. Yükseltilmiş bir açık **Windows PowerShell** penceresi.
7. Girin **ipconfig/all**.
8. Çıktıda Bul **IPv4 adresi**ve daha sonra kullanmak için adresi kaydedin. Bu, POC2 ping gönderecek adresidir. Örnek ortamda adres **10.0.10.4** şeklindedir, ancak sizin ortamınızda farklı olabilir. İçinde girecektir **10.0.10.0/24** daha önce oluşturduğunuz alt ağ.
9. Sanal makinenin ping komutuna yanıt veren bir güvenlik duvarı kuralı oluşturmak için aşağıdaki PowerShell komutunu çalıştırın:

   ```powershell
   New-NetFirewallRule `
    –DisplayName “Allow ICMPv4-In” `
    –Protocol ICMPv4
   ```

### <a name="sign-in-to-the-tenant-vm-in-poc2"></a>Kiracı VM POC2 ile oturum açın
1. Azure yığın fiziksel makine POC2 için oturum açın ve ardından kullanıcı portalında oturum açmak için bir kiracı hesabını kullanın.
2. Sol gezinti çubuğunda **işlem**.
3. Sanal makineler listesinden Bul **VM02** daha önce oluşturduğunuz ve ardından seçin.
4. Sanal makine için dikey pencere üzerinde **Bağlan**’a tıklayın.
5. Sanal makine oluşturduğunuzda, yapılandırdığınız hesabıyla oturum açın.
6. Yükseltilmiş bir açık **Windows PowerShell** penceresi.
7. Girin **ipconfig/all**.
8. İçine düşerse bir IPv4 adresi görmeniz gerekir **10.0.20.0/24**. Örnek ortamında adresidir **10.0.20.4**, ancak adresinizi farklı olabilir.
9. Sanal makinenin ping komutuna yanıt veren bir güvenlik duvarı kuralı oluşturmak için aşağıdaki PowerShell komutunu çalıştırın:

   ```powershell
   New-NetFirewallRule `
    –DisplayName “Allow ICMPv4-In” `
    –Protocol ICMPv4
   ```

10. POC2 sanal makineden POC1, sanal makine tüneli üzerinden ping işlemi yapın. Bunu yapmak için VM01 kaydettiğiniz DIP ping atın.
   Örnek ortamında budur **10.0.10.4**, ancak laboratuvarınızda ettiğiniz adresi ping emin olun. Aşağıdakine benzer bir sonuç görmeniz gerekir:
   
    ![PING başarılı](media/azure-stack-create-vpn-connection-one-node-tp2/image19b.png)
11. Uzak sanal makineden bir yanıt başarılı bir test gösteriyor! Sanal makine penceresinin kapatabilirsiniz. Bağlantınızı test veri aktarımları bir dosya kopyalama gibi diğer tür deneyebilirsiniz.

### <a name="viewing-data-transfer-statistics-through-the-gateway-connection"></a>Ağ geçidi bağlantısı üzerinden veri aktarımı istatistiklerini görüntüleme
Siteden siteye bağlantınızı ne kadar veri geçirmeden bilmek istiyorsanız, bu bilgi edinilebilir **bağlantı** dikey. Bu test ayrıca yalnızca gönderilen ping gerçekte VPN bağlantısı üzerinden geçmeden doğrulamak için başka bir yoludur.

1. POC2 Kiracı sanal makineye oturum açtınız yaparken, kullanıcı portalında oturum açmak için Kiracı hesabınızı kullanın.
2. Git **tüm kaynakları**ve ardından **POC2 POC1** bağlantı. **Bağlantıları** görüntülenir.
4. Üzerinde **bağlantı** dikey penceresinde, istatistikleri **verileri** ve **verileri** görünür. Aşağıdaki ekran görüntüsünde, çok sayıda ek dosya aktarımı öznitelikli. Bazı sıfır olmayan değerler var. görmeniz gerekir.
   
    ![Giren ve çıkan veriler](media/azure-stack-create-vpn-connection-one-node-tp2/image20.png)
