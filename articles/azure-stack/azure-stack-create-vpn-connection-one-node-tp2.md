---
title: "Farklı Azure Stack PoC Ortamlarındaki iki Sanal Ağ arasında Siteden Siteye VPN bağlantısı oluşturma | Microsoft Docs"
description: "Bir bulut yöneticisinin TP2’de tek düğümlü iki POC ortamı arasında Siteden Siteye VPN bağlantısı oluşturmasına olanak tanıyan adım adım yordam."
services: azure-stack
documentationcenter: 
author: ScottNapolitan
manager: darmour
editor: 
ms.assetid: 3f1b4e02-dbab-46a3-8e11-a777722120ec
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/08/2017
ms.author: scottnap
translationtype: Human Translation
ms.sourcegitcommit: 5104c7996de9dc0597e65be31c28a9aaa1243a90
ms.openlocfilehash: d324290caf5b5a085a2daf67e541c295dffda732


---
# <a name="create-a-site-to-site-vpn-connection-between-two-virtual-networks-in-different-azure-stack-poc-environments"></a>Farklı Azure Stack PoC Ortamlarındaki iki Sanal Ağ arasında Siteden Siteye VPN bağlantısı oluşturma
## <a name="overview"></a>Genel Bakış
Bu makalede iki ayrı Azure Stack Kavram Kanıtı (POC) ortamındaki iki ayrı sanal ağ arasında Siteden Siteye VPN bağlantısı oluşturma işlemi gösterilir. Bağlantıları yapılandırırken Azure Stack hizmetinde VPN ağ geçitlerinin nasıl çalıştığını öğreneceksiniz.

> [!NOTE]
> Bu belge özellikle Azure Stack TP2 POC için geçerlidir.
> 
> 

### <a name="connection-diagram"></a>Bağlantı diyagramı
Aşağıdaki diyagram, işiniz bittiğinde yapılandırmanın nasıl görünmesi gerektiğini göstermektedir:

![](media/azure-stack-create-vpn-connection-one-node-tp2/image1.png)

### <a name="before-you-begin"></a>Başlamadan önce
Bu yapılandırmayı tamamlamak için çalışmaya başlamadan önce aşağıdaki öğelerin bulunduğundan emin olun:

* [Azure Stack Dağıtım Önkoşulları](azure-stack-deploy.md) ile tanımlanan Azure Stack POC donanım gereksinimlerini ve aynı belgede tanımlanan diğer önkoşulları karşılayan iki sunucu.
* Azure Stack Technical Preview 2 Dağıtım Paketi.

## <a name="deploy-the-poc-environments"></a>POC ortamlarını dağıtma
Bu yapılandırmayı tamamlamak için iki Azure Stack POC ortamı dağıtacaksınız.

* Dağıttığınız her POC için [Azure Stack POC Dağıtma](azure-stack-run-powershell-script.md) makalesinde ayrıntılı olarak verilen dağıtım yönergelerini izleyebilirsiniz.
  Bu belgedeki her POC ortamı genellikle *POC1* ve *POC2* olarak adlandırılır.

## <a name="configure-quotas-for-compute-network-and-storage"></a>İşlem, Ağ ve Depolama Kotalarını Yapılandırma
İlk olarak işlem, ağ ve depolama *Kotalarını* yapılandırmanız gerekir. Bu hizmetler bir *Plan* ve sonra kiracıların abone olabileceği bir *Teklif* ile ilişkilendirilebilir.

> [!NOTE]
> Bu adımları her Azure Stack POC ortamı için uygulamanız gerekir.
> 
> 

Hizmetler Kotalar oluşturma işlemi TP1’den itibaren değişmiştir. TP2’de kota oluşturma adımları <http://aka.ms/mas-create-quotas> sayfasında bulunabilir. Bu uygulama için tüm kota ayarlarının varsayılan değerlerini kabul edebilirsiniz.

## <a name="create-a-plan-and-offer"></a>Planı ve Teklif oluşturma
[Planlar](azure-stack-key-features.md), bir veya birden fazla hizmetten oluşan gruplardır. Bir sağlayıcı olarak, kiracılarınıza sunabileceğiniz planlar oluşturabilirsiniz. Böylece kiracılarınız tekliflerinize abone olarak, bu tekliflerin içerdiği planları ve hizmetleri kullanabilir.

> [!NOTE]
> Bu adımları her Azure Stack POC ortamı için uygulamanız gerekir.
> 
> 

1. İlk olarak bir Plan oluşturun. Bunu yapmak için [Plan oluşturma](azure-stack-create-plan.md) çevrimiçi makalesindeki adımları izleyebilirsiniz.
2. [Azure Stack’te Teklif oluşturma](azure-stack-create-offer.md) makalesinde açıklanan adımları izleyerek bir Teklif oluşturun.
3. Portalda kiracı yönetici olarak oturum açın ve [oluşturduğunuz Teklife abone olun](azure-stack-subscribe-plan-provision-vm.md).

## <a name="create-the-network-resources-in-poc-1"></a>POC 1’de Ağ Kaynakları oluşturma
Bu noktada yapılandırmanızı ayarlamak için gerekli olan kaynakları gerçekten oluşturacaksınız. Aşağıdaki adımlar yapacağınız işlemleri göstermektedir. Bu yönergeler, portalı kullanarak kaynak oluşturma işlemini gösterir, ancak aynı işlem PowerShell kullanılarak da gerçekleştirilebilir.

![](media/azure-stack-create-vpn-connection-one-node-tp2/image2.png)

### <a name="log-in-as-a-tenant"></a>Kiracı olarak oturum açma
Bir hizmet yöneticisi; kiracılarının kullanabileceği planları, teklifleri ve abonelikleri test etmek için kiracı olarak oturum açabilir. Kiracı hesabınız yoksa oturum açmadan önce [Kiracı hesabı oluşturun](azure-stack-add-new-user-aad.md).

### <a name="create-the-virtual-network--vm-subnet"></a>Sanal ağ ve VM alt ağı oluşturma
1. Kiracı hesabı kullanarak oturum açın.
2. Azure portalda **Yeni**’ye tıklayın.
   
    ![](media/azure-stack-create-vpn-connection-one-node-tp2/image3.png)
3. Market menüsünden **Ağ** seçeneğini belirleyin.
4. Menüdeki **Sanal ağ** öğesine tıklayın.
5. Kaynak açıklaması dikey penceresinin altındaki **Oluştur** öğesine tıklayın. Bu tabloya göre uygun alanlara aşağıdaki değerleri girin.
   
   | **Alan** | **Değer** |
   | --- | --- |
   | Adı |vnet-01 |
   | Adres alanı |10.0.10.0/23 |
   | Alt ağ adı |subnet-01 |
   | Alt ağ adres aralığı |10.0.10.0/24 |
6. Daha önce oluşturduğunuz Aboneliği, **Abonelik** alanında görmeniz gerekir.
7. Kaynak Grubu için yeni bir Kaynak Grubu oluşturabilir veya zaten bir tane varsa **Var olanı kullan**’ı seçebilirsiniz.
8. Varsayılan konumu doğrulayın.
9. **Oluştur**’a tıklayın.

### <a name="create-the-gateway-subnet"></a>Ağ Geçidi Alt Ağı oluşturma
1. Panodan yeni oluşturduğunuz Sanal Ağ kaynağını (Vnet-01) açın.
2. Ayarlar dikey penceresinde **Alt ağlar**’ı seçin.
3. Sanal ağa bir ağ geçidi alt ağı eklemek için **Ağ Geçidi Alt Ağı**’na tıklayın.
   
    ![](media/azure-stack-create-vpn-connection-one-node-tp2/image4.png)
4. Alt ağın adı varsayılan olarak **GatewaySubnet** şeklinde ayarlanır.
   Ağ geçidi alt ağları özeldir ve düzgün şekilde çalışabilmesi için bu ada sahip olmalıdır.
5. **Adres aralığı** alanına **10.0.11.0/24** yazın.
6. Ağ geçidi alt ağını oluşturmak için **Oluştur**’a tıklayın.

### <a name="create-the-virtual-network-gateway"></a>Sanal Ağ Geçidi oluşturma
1. Azure portalda **Yeni**’ye tıklayın.
   
2. Market menüsünden **Ağ** seçeneğini belirleyin.
3. Ağ kaynakları listesinden **Sanal ağ geçidi**’ni seçin.
4. Açıklamayı gözden geçirin ve **Oluştur**’a tıklayın.
5. **Ad** alanına **GW1** yazın.
6. Bir sanal ağ seçmek için **Sanal ağ** öğesine tıklayın.
   Listeden **Vnet-01** öğesini seçin.
7. **Genel IP adresi** menü öğesine tıklayın. **Genel IP adresi seçin** dikey penceresi açıldığında **Yeni oluştur**’a tıklayın.
8. **Ad** alanına **GW1-PiP** yazıp **Tamam**’a tıklayın.
9. **Ağ geçidi türü** için varsayılan olarak **VPN** seçili olmalıdır. Bu ayarı tutun.
10. **VPN türü** için varsayılan olarak **yol tabanlı** seçili olmalıdır.
    Bu ayarı tutun.
11. **Abonelik** ve **Konum** seçeneklerinin doğruluğunu onaylayın. İsterseniz kaynağı Panoya sabitleyebilirsiniz. **Oluştur**’a tıklayın.

### <a name="create-the-local-network-gateway"></a>Yerel Ağ Geçidi oluşturma
Bu Azure Stack değerlendirme dağıtımındaki *yerel ağ geçidi* uygulaması, gerçek bir Azure dağıtımındaki uygulamadan biraz farklıdır.

Azure’da olduğu gibi, bir yerel ağ geçidi kavramı vardır. Ancak, bir Azure dağıtımında yerel ağ geçidi, Azure’daki bir sanal ağ geçidine bağlanmak için kullandığınız şirket içi (kiracı üzerinde) fiziksel cihazı temsil eder. Ancak bu Azure Stack değerlendirme dağıtımında, bağlantının her iki ucu da sanal ağ geçididir!

Daha genel düşünmek gerekirse, Yerel Ağ Geçidi kaynağı her zaman bağlantının diğer ucundaki uzak ağ geçidini belirtmeye yöneliktir. POC’nin tasarlanma şekli nedeniyle, diğer POC’nin NAT VM’si üzerindeki dış ağ bağdaştırıcısının IP adresini, Yerel Ağ Geçidinin Genel IP Adresi olarak belirtmeniz gerekir. Bundan sonra, her iki ucun da düzgün bağlandığından emin olmak için NAT VM üzerinde NAT eşlemeleri oluşturursunuz.


### <a name="get-the-ip-address-of-the-external-adapter-of-the-nat-vm"></a>NAT VM dış bağdaştırıcısının IP adresini alma
1. POC2 için Azure Stack fiziksel makinesinde oturum açın.
2. [Windows Tuşu] + R tuşuna basarak **Çalıştır** menüsünü açın ve **mstsc** yazıp Enter tuşuna basın.
3. **Bilgisayar** alanına **MAS-BGPNAT01** adını yazıp  **Bağlan**’a tıklayın.
4. Başlat Menüsüne tıklayıp **Windows PowerShell**’e sağ tıklayın ve **Yönetici Olarak Çalıştır**’ı seçin.
5. **ipconfig /all** yazın.
6. Şirket içi ağınıza bağlı Ethernet Bağdaştırıcısını bulun ve bu bağdaştırıcıya bağlı IPv4 adresini not alın. Örnek ortamımızda bu değer **10.16.167.195**’tir, ancak sizinki farklı bir şey olacaktır.
7. Bu adresini kaydedin. Bu, POC1’de oluşturduğunuz Yerel Ağ Geçidi kaynağının Genel IP Adresi olarak kullanacağınız değerdir.

### <a name="create-the-local-network-gateway-resource"></a>Yerel Ağ Geçidi Kaynağı oluşturma
1. POC1 için Azure Stack fiziksel makinesinde oturum açın.
2. **Bilgisayar** alanına **MAS-CON01** adını yazıp **Bağlan**’a tıklayın.
3. Azure portalda **Yeni**’ye tıklayın.
   
4. Market menüsünden **Ağ** seçeneğini belirleyin.
5. Kaynak listesinden **yerel ağ geçidi**’ni seçin.
6. **Ad** alanına **POC2-GW** yazın.
7. Diğer ağ geçidinin IP adresini henüz bilmiyorsanız önemli değildir, daha sonra geri dönüp değiştirebilirsiniz. Şimdilik **IP adresi** alanına **10.16.167.195** yazın.
8. **Adres Alanı** alanına POC2’de oluşturacağınız sanal ağın adres alanını yazın. Bu değer **10.0.20.0/23** olacağı için bunu yazın.
9. **Abonelik**, **Kaynak Grubu** ve **konum** seçeneklerinin doğruluğunu onaylayıp **Oluştur**’a tıklayın.

### <a name="create-the-connection"></a>Bağlantı oluşturma
1. Azure portalda **Yeni**’ye tıklayın.
   
2. Market menüsünden **Ağ** seçeneğini belirleyin.
3. Kaynak listesinden **Bağlantı**’yı seçin.
4. **Temel** ayarlar dikey penceresinde **Bağlantı türü** olarak **Siteden siteye (IPSec)** seçeneğini belirleyin.
5. **Abonelik**, **Kaynak Grubu** ve **Konum**’u seçip **Tamam**’a tıklayın.
6. **Ayarlar** dikey penceresinde daha önce oluşturduğunuz **Sanal Ağ Geçidi**’ni (**GW1**) seçin.
7. Daha önce oluşturduğunuz **yerel Ağ Geçidi**’ni (**POC2-GW**) seçin.
8. **Bağlantı Adı** alanına **POC1-POC2** yazın.
9. **Paylaşılan Anahtar (PSK)** alanına **12345** yazıp **Tamam**’a tıklayın.

### <a name="create-a-vm"></a>VM oluşturma
VPN Bağlantısından geçen verileri doğrulamak için VM’lerin her POC’de veri gönderip alması gerekir. Şimdi POC1’de bir VM oluşturun ve sanal ağınızdaki VM alt ağına yerleştirin.

1. Azure portalda **Yeni**’ye tıklayın.
   
2. Market menüsünden **Sanal Makineler**’i seçin.
3. Sanal makine görüntüleri listesinde **Windows Server 2012 R2 Datacenter** görüntüsünü seçin.
4. **Temel Bilgiler** dikey penceresinde **Ad** alanına **VM01** yazın.
5. Geçerli bir kullanıcı adı ve parola yazın. Bu hesabı oluşturduktan sonra VM’de oturum açmak için kullanacaksınız.
6. **Abonelik**, **Kaynak Grubu** ve **Konum** bilgilerini sağlayıp **Tamam**’a tıklayın.
7. **Boyut** dikey penceresinde bu örnek için bir VM boyutu seçin ve ardından **Seç**’e tıklayın.
8. **Ayarlar** dikey penceresinde varsayılan değerleri kabul edebilirsiniz; yalnızca seçili Sanal ağın **VNET-01** olduğundan ve Alt ağın **10.0.10.0/24** olarak ayarlandığından emin olun. **Tamam** düğmesine tıklayın.
9. **Özet** dikey penceresindeki ayarları gözden geçirin ve **Tamam**’a tıklayın.

## <a name="create-the-network-resources-in-poc-2"></a>POC 2’de Ağ Kaynakları oluşturma
### <a name="log-in-as-a-tenant"></a>Kiracı olarak oturum açma
Bir hizmet yöneticisi; kiracılarının kullanabileceği planları, teklifleri ve abonelikleri test etmek için kiracı olarak oturum açabilir. Kiracı hesabınız yoksa oturum açmadan önce [Kiracı hesabı oluşturun](azure-stack-add-new-user-aad.md).

### <a name="create-the-virtual-network-and-vm-subnet"></a>Sanal ağ ve VM alt ağı oluşturma
1. Kiracı hesabı kullanarak oturum açın.
2. Azure portalda **Yeni**’ye tıklayın.
   
3. Market menüsünden **Ağ** seçeneğini belirleyin.
4. Menüde **Sanal ağ** öğesine tıklayın.
5. Kaynak açıklaması dikey penceresinin altındaki **Oluştur** öğesine tıklayın. Aşağıdaki tabloda listelenen uygun alanlar için aşağıdaki değerleri yazın.
   
   | **Alan** | **Değer** |
   | --- | --- |
   | Adı |vnet-02 |
   | Adres alanı |10.0.20.0/23 |
   | Alt ağ adı |subnet-02 |
   | Alt ağ adres aralığı |10.0.20.0/24 |
6. Daha önce oluşturduğunuz Aboneliği, **Abonelik** alanında görmeniz gerekir.
7. Kaynak Grubu için yeni bir Kaynak Grubu oluşturabilir veya zaten bir tane varsa **Var olanı kullan**’ı seçebilirsiniz.
8. Varsayılan **konumu** doğrulayın. İsterseniz, sanal ağı kolay erişim için Panoya sabitleyebilirsiniz.
9. **Oluştur**’a tıklayın.

### <a name="create-the-gateway-subnet"></a>Ağ Geçidi Alt Ağı oluşturma
1. Panodan oluşturduğunuz Sanal ağ kaynağını (**Vnet-02**) açın.
2. **Ayarlar** dikey penceresinde **Alt ağlar**’ı seçin.
3. Sanal ağa bir ağ geçidi alt ağı eklemek için **Ağ geçidi alt ağı**’na tıklayın.
   
4. Alt ağın adı varsayılan olarak **GatewaySubnet** şeklinde ayarlanır.
   Ağ geçidi alt ağları özeldir ve düzgün şekilde çalışabilmesi için bu ada sahip olmalıdır.
5. **Adres aralığı** alanına **10.0.20.0/24** yazın.
6. Ağ geçidi alt ağını oluşturmak için **Oluştur**’a tıklayın.

### <a name="create-the-virtual-network-gateway"></a>Sanal Ağ Geçidi oluşturma
1. Azure portalda **Yeni**’ye tıklayın.
   
2. Market menüsünden **Ağ** seçeneğini belirleyin.
3. Ağ kaynakları listesinden **Sanal ağ geçidi**’ni seçin.
4. Açıklamayı gözden geçirin ve **Oluştur**’a tıklayın.
5. **Ad** alanına **GW2** yazın.
6. **Sanal ağ**'a tıklayarak bir sanal ağ seçin.
   Listeden **Vnet-02** öğesini seçin.
7. **Genel IP adresi**’ne tıklayın. **Genel IP adresi seçin** dikey penceresi açıldığında **Yeni oluştur**’a tıklayın.
8. **Ad** alanına **GW2-PiP** yazıp **Tamam**’a tıklayın.
9. **Ağ geçidi türü** için varsayılan olarak **VPN** seçili olmalıdır. Bu ayarı tutun.
10. **VPN türü** için varsayılan olarak **yol tabanlı** seçili olmalıdır.
    Bu ayarı tutun.
11. **Abonelik** ve **Konum** seçeneklerinin doğruluğunu onaylayın. İsterseniz kaynağı Panoya sabitleyebilirsiniz. **Oluştur**’a tıklayın.

### <a name="create-the-local-network-gateway"></a>Yerel Ağ Geçidi oluşturma
#### <a name="get-the-ip-address-of-the-external-adapter-of-the-nat-vm"></a>NAT VM dış bağdaştırıcısının IP adresini alma
1. POC1 için Azure Stack fiziksel makinesinde oturum açın.
2. [Windows Tuşu] + R tuşunu basılı tutarak **Çalıştır** menüsünü açın ve **mstsc** yazıp Enter tuşuna basın.
3. **Bilgisayar** alanına **MAS-BGPNAT01** adını yazıp  **Bağlan**’a tıklayın.
4. Başlat menüsüne tıklayıp **Windows PowerShell**’e sağ tıklayın ve **Yönetici Olarak Çalıştır**’ı seçin.
5. **ipconfig /all** yazın.
6. Şirket içi ağınıza bağlı Ethernet bağdaştırıcısını bulun ve bu bağdaştırıcıya bağlı IPv4 adresini not alın. Örnek ortamda bu değer **10.16.169.131**’dir, ancak sizinki farklı bir şey olacaktır.
7. Bu adresini kaydedin. Bu, daha sonra POC1’de oluşturduğunuz Yerel Ağ Geçidi kaynağının Genel IP Adresi olarak kullanacağınız değerdir.

#### <a name="create-the-local-network-gateway-resource"></a>Yerel Ağ Geçidi Kaynağı oluşturma
1. POC2 için Azure Stack fiziksel makinesinde oturum açın.
2. **Bilgisayar** alanına **MAS-CON01** adını yazıp **Bağlan**’a tıklayın.
3. Azure portalda **Yeni**’ye tıklayın.
   
4. Market menüsünden **Ağ** seçeneğini belirleyin.
5. Kaynak listesinden **yerel ağ geçidi**’ni seçin.
6. **Ad** alanına **POC1-GW** yazın.
7. Bu noktada, POC1’de Sanal ağ geçidi için kaydettiğiniz Genel IP Adresi gereklidir. **IP adresi** alanına **10.16.169.131** yazın.
8. **Adres Alanı** alanına POC1 - **10.0.0.0/16**’daki **Vnet-01** adres alanını yazın.
9. **Abonelik**, **Kaynak Grubu** ve **konum** seçeneklerinin doğruluğunu onaylayıp **Oluştur**’a tıklayın.

## <a name="create-the-connection"></a>Bağlantı oluşturma
1. Azure portalda **Yeni**’ye tıklayın.
   
2. Market menüsünden **Ağ** seçeneğini belirleyin.
3. Kaynak listesinden **Bağlantı**’yı seçin.
4. **Temel** ayarlar dikey penceresinde **Bağlantı türü** olarak **Siteden siteye (IPSec)** seçeneğini belirleyin.
5. **Abonelik**, **Kaynak Grubu** ve **Konum**’u seçip **Tamam**’a tıklayın.
6. **Ayarlar** dikey penceresinde daha önce oluşturduğunuz **Sanal Ağ Geçidi**’ni (**GW1**) seçin.
7. Daha önce oluşturduğunuz **Yerel Ağ Geçidi**’ni (**POC1-GW**) seçin.
8. **Bağlantı Adı** alanına **POC2-POC1** yazın.
9. **Paylaşılan Anahtar (PSK)** alanına **12345** yazın. Farklı bir değer seçerseniz, POC1 üzerinde oluşturduğunuz Paylaşılan Anahtar değeriyle eşleşmesi ZORUNLUDUR. **Tamam** düğmesine tıklayın.

## <a name="create-a-vm"></a>VM oluşturma
Şimdi POC1’de bir VM oluşturun ve sanal ağınızdaki VM alt ağına yerleştirin.

1. Azure portalda **Yeni**’ye tıklayın.
   
2. Market menüsünden **Sanal Makineler**’i seçin.
3. Sanal makine görüntüleri listesinde **Windows Server 2012 R2 Datacenter** görüntüsünü seçin.
4. **Temel Bilgiler** dikey penceresinde **Ad** alanına **VM02** yazın.
5. Geçerli bir kullanıcı adı ve parola yazın. Bu hesabı oluşturduktan sonra VM’de oturum açmak için kullanacaksınız.
6. **Abonelik**, **Kaynak Grubu** ve **Konum** bilgilerini sağlayıp **Tamam**’a tıklayın.
7. **Boyut** dikey penceresinde bu örnek için bir VM boyutu seçin ve ardından **Seç**’e tıklayın.
8. Ayarlar dikey penceresinde varsayılan değerleri kabul edebilirsiniz; yalnızca seçili sanal ağın **VNET-02** olduğundan ve alt ağın **20.0.0.0/24** olarak ayarlandığından emin olun. **Tamam** düğmesine tıklayın.
9. **Özet** dikey penceresindeki ayarları gözden geçirin ve **Tamam**’a tıklayın.

## <a name="configure-the-nat-vm-in-each-poc-for-gateway-traversal"></a>Ağ geçidi geçişi için her POC’de NAT VM yapılandırma
POC bağımsız ve fiziksel konağın dağıtıldığı ağdan yalıtılmış olarak tasarlandığı için, ağ geçitlerinin bağlı olduğu “Dış” VIP ağı gerçekten dış değildir, aslında Ağ Adresi Çevirisi (NAT) yapan bir yönlendiricinin arkasında gizlenmiştir. Yönlendirici gerçekte POC altyapısındaki Uzak Erişim Hizmetleri (RRAS) rolünü çalıştıran bir Windows Server sanal makinesidir (**MAS-BGPNAT01**). Siteden Siteye VPN Bağlantısının her iki uca bağlanması için MAS-BGPNAT01 VM üzerinde NAT’yi yapılandırmanız gerekir. Bunu yapmak için, bir VPN bağlantısında gerekli olan bağlantı noktaları için BGPNAT VM üzerindeki dış arabirimi Edge Ağ Geçidi Havuzunun VIP’sine eşleyen bir Statik NAT eşlemesi oluşturmanız gerekir.

> [!NOTE]
> Bu yapılandırma yalnızca POC ortamları için gereklidir.
> 
> 

### <a name="configure-nat"></a>NAT yapılandırma
HER İKİ POC ortamında aşağıdaki adımları izlemeniz gerekir.

1. POC1 için Azure Stack fiziksel makinesinde oturum açın.
2. [Windows Tuşu] + R tuşunu basılı tutarak **Çalıştır** menüsünü açın ve **mstsc** yazıp **Enter** tuşuna basın.
3. **Bilgisayar** alanına **MAS-BGPNAT01** adını yazıp **Bağlan**’a tıklayın.
4. Başlat menüsüne tıklayıp **Windows PowerShell**’e sağ tıklayın ve **Yönetici Olarak Çalıştır**’ı seçin.
5. **ipconfig /all** yazın.
6. Şirket içi ağınıza bağlı Ethernet Bağdaştırıcısını bulun ve bu bağdaştırıcıya bağlı IPv4 adresini not alın. Örnek ortamda bu değer **10.16.169.131**’dir (aşağıda kırmızı daire içine alınmış), ancak sizinki farklı bir şey olacaktır.
   
    ![](media/azure-stack-create-vpn-connection-one-node-tp2/image16.png)
7. IKE kimlik doğrulamasına yönelik bağlantı noktaları için dış NAT adresini belirtmek üzere aşağıdaki PowerShell komutunu yazın. IP adresini ortamınızla eşleşecek şekilde değiştirmeyi unutmayın.
   
       Add-NetNatExternalAddress -NatName BGPNAT -IPAddress 10.16.169.131 -PortStart 499 -PortEnd 501
8. Ardından, Ağ Geçidi Genel IP Adresini IPSEC tünelinin 1. AŞAMASI için ISAKMP 500 bağlantı noktasına eşlemek üzere statik bir NAT eşlemesi oluşturun.
   
        Add-NetNatStaticMapping -NatName BGPNAT -Protocol UDP -ExternalIPAddress 10.16.169.131 -InternalIPAddress 192.168.102.1 -ExternalPort 500 -InternalPort 500
> [!NOTE] 
> Buradaki `-InternalAddress` parametresi daha önce oluşturduğunuz Sanal Ağ Geçidinin Genel IP Adresidir.  Bu IP adresini bulmak için Sanal Ağ Geçidi dikey penceresinin özelliklerine bakın ve Genel IP Adresinin değerini bulun.       

9. Son olarak, NAT cihazları üzerinde tam IPEC tünelini başarıyla oluşturmak için 4500 bağlantı noktasını kullanan bir NAT geçişi yapılandırmanız gerekir.
   
        Add-NetNatStaticMapping -NatName BGPNAT -Protocol UDP -ExternalIPAddress 10.16.169.131 -InternalIPAddress 192.168.102.1 -ExternalPort 4500 -InternalPort 4500
> [!NOTE] 
> Buradaki `-InternalAddress` parametresi daha önce oluşturduğunuz Sanal Ağ Geçidinin Genel IP Adresidir.  Bu IP adresini bulmak için Sanal Ağ Geçidi dikey penceresinin özelliklerine bakın ve Genel IP Adresinin değerini bulun.       

10. POC2’de 1-9 arasındaki adımları yineleyin.

## <a name="test-the-connection"></a>Bağlantıyı sınama
Siteden Siteye bağlantı kurulduğuna göre içinden geçen trafiği alabildiğinizi doğrulamanız gerekir. Bu basit görev yalnızca POC ortamlarından birinde daha önce oluşturduğunuz bir sanal makinede oturum açıp, diğer ortamda oluşturduğunuz sanal makineye ping göndermeyi içerir. Trafiği Siteden Siteye bağlantı üzerinden geçirdiğinizden emin olmak için VIP üzerindeki değil, uzak ağdaki VM’nin Doğrudan IP (DIP) adresine ping gönderdiğinizden emin olmanız gerekir. Bunu yapmak için bağlantının diğer ucundaki adresi bulup not etmeniz gerekir.

### <a name="log-in-to-the-tenant-vm-in-poc1"></a>POC1’deki kiracı VM’de oturum açma
1. POC1 için Azure Stack fiziksel makinesinde oturum açın ve bir kiracı hesabı kullanarak Portal’da oturum açın.
2. Sol gezinti çubuğunda **Sanal Makineler**’e tıklayın.
3. VM listesinde daha önce oluşturduğunuz **VM01**’i bulup tıklayın.
4. Sanal makine için dikey pencere üzerinde **Bağlan**’a tıklayın.
   
     ![](media/azure-stack-create-vpn-connection-one-node-tp2/image17.png)
5. VM içinden bir Komut istemi açın ve **ipconfig/all** yazın.
6. Çıktıda **IPv4 Adresini** bulup not edin. POC2’den ping gönderilecek adres budur. Örnek ortamda adres **10.0.10.4** şeklindedir, ancak sizin ortamınızda farklı olabilir. Bununla birlikte, adres daha önce oluşturulan **10.0.10.0/24** alt ağı dahilinde olmalıdır.

### <a name="log-in-to-the-tenant-vm-in-poc2"></a>POC2’deki kiracı VM’de oturum açma
1. POC2 için Azure Stack fiziksel makinesinde oturum açın ve bir kiracı hesabı kullanarak portalda oturum açın.
2. Sol gezinti çubuğunda **Sanal Makineler**’e tıklayın.
3. VM listesinde daha önce oluşturduğunuz **VM02**’yi bulup tıklayın.
4. Sanal makine için dikey pencere üzerinde **Bağlan**’a tıklayın.
   
5. VM içinden bir Komut istemi açın ve **ipconfig/all** yazın.
6. 10.0.20.0/24 aralığına denk gelen bir IPv4 adresi görmeniz gerekir. Örnek ortamda adres 10.0.20.4 şeklindedir, ancak sizinki farklı olabilir.
7. Şimdi POC2’deki VM’den POC1’deki VM’ye tünel aracılığıyla ping göndermeniz gerekir. Bunu yapmak için VM01’den kaydettiğiniz DIP’ye ping gönderin.
   Örnek ortamda bu adres 10.0.10.4 şeklindedir, ancak laboratuvarınızda not aldığınız adrese ping gönderdiğinizden emin olun. Şuna benzer bir sonuç görmeniz gerekir:
   
    ![](media/azure-stack-create-vpn-connection-one-node-tp2/image19b.png)
8. Uzak VM’den alınan yanıt, testin başarılı olduğunu gösteriyor! VM Bağlantısı penceresini kapatabilirsiniz. Ya da bağlantınızı sınamak için dosya kopyası gibi diğer veri aktarımlarını deneyebilirsiniz.

### <a name="viewing-data-transfer-statistics-through-the-gateway-connection"></a>Ağ geçidi bağlantısı üzerinden veri aktarımı istatistiklerini görüntüleme
Siteden Siteye bağlantınızdan ne kadar veri geçtiğini bilmek istiyorsanız, bu bilgiye Bağlantı dikey penceresinden ulaşabilirsiniz. Bu test ayrıca yeni gönderdiğiniz ping’in VPN bağlantısından gerçekte geçip geçmediğini doğrulamanın iyi bir yoludur.

1. POC2’deki kiracı VM’de oturumunuz hala açıkken, kiracı hesabınızı kullanarak **Microsoft Azure Stack POC Portalı**’nda oturum açın.
2. **Gözat** menü öğesine tıklayıp **Bağlantılar**’ı seçin.
3. Listedeki **POC2 POC1** bağlantısına tıklayın.
4. Bağlantı dikey penceresinde **Veri girişi** ve **Veri çıkışı** istatistiklerini görebilirsiniz. Aşağıdaki ekran görüntüsünde yalnızca bir ping’in oluşturacağından daha büyük sayılar görünmektedir. Bunun nedeni bazı ak dosya aktarımlarının da yapılmasıdır. Burada sıfır olmayan bazı değerler görürsünüz.
   
    ![](media/azure-stack-create-vpn-connection-one-node-tp2/image20.png)




<!--HONumber=Feb17_HO2-->


