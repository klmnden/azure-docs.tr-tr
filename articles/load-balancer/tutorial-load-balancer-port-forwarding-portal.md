---
title: "Öğretici: Azure portalını kullanarak Azure yük Dengeleyicide bağlantı noktası iletme'yi yapılandırma"
titlesuffix: Azure Load Balancer
description: Bu öğreticide, bir Azure sanal ağındaki Vm'leri bağlantılar oluşturmak için Azure Load Balancer'ı kullanarak bağlantı noktası iletme yapılandırma işlemi gösterilmektedir.
services: load-balancer
documentationcenter: na
author: KumudD
manager: twooley
Customer intent: As an IT administrator, I want to configure port forwarding in Azure Load Balancer to remotely connect to VMs in an Azure virtual network.
ms.service: load-balancer
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/26/2019
ms.author: kumud
ms.custom: seodec18
ms.openlocfilehash: db94f1d241223a9c54a6e3d516840dd17fd0c576
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60007517"
---
# <a name="tutorial-configure-port-forwarding-in-azure-load-balancer-using-the-portal"></a>Öğretici: Portalı kullanarak Azure yük Dengeleyicide bağlantı noktası iletme'yi yapılandırma

Bağlantı noktası iletme, bir Azure yük dengeleyici genel IP adresi ve bağlantı noktası numarası kullanarak sanal makineleri (VM'ler) bağlanmanızı bir Azure sanal ağında sağlar. 

Bu öğreticide, bir Azure yük dengeleyicideki bağlantı noktası iletmeyi ayarlayın. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Ağ trafiği dengelemek için genel bir Standard load balancer, yükü devredilen Vm'leri oluşturmak. 
> * Bir sanal ağ ve VM ile bir ağ güvenlik grubu (NSG) kuralı oluşturun. 
> * VM'ler yük dengeleyici arka uç adres havuzuna ekleyin.
> * Bir yük dengeleyici durum araştırması ve trafik kuralları oluşturun.
> * Yük Dengeleyici oluşturma gelen NAT bağlantı noktası iletme kuralları.
> * Yükleyip IIS görünümü Yük Dengeleme ve bağlantı noktası iletme eyleminin Vm'lere yapılandırın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

Bu öğreticideki tüm adımları için Azure portalında oturum açın [ https://portal.azure.com ](https://portal.azure.com).

## <a name="create-a-standard-load-balancer"></a>Standard load balancer oluşturma

İlk olarak, VM'lerin yükünü trafik Yük Dengelemesi genel bir Standard load balancer oluşturun. Standart yük dengeleyici yalnızca standart genel IP adresi destekler. Standard load balancer'ı oluşturduğunuzda, yük dengeleyici ön ucuna yapılandırılır ve adlı yeni bir standart genel IP adresi, ayrıca oluşturma **LoadBalancerFrontEnd** varsayılan olarak. 

1. Ekranın sol üst kısmında **Kaynak oluştur** > **Ağ** > **Yük Dengeleyici**'ye tıklayın.
2. İçinde **Temelleri** sekmesinde **yük dengeleyici Oluştur** sayfasında, girin veya aşağıdaki bilgileri seçin, geri kalan ayarlar için varsayılan değerleri kabul edin ve ardından **gözden geçir +Oluştur**:

    | Ayar                 | Value                                              |
    | ---                     | ---                                                |
    | Abonelik               | Aboneliğinizi seçin.    |    
    | Kaynak grubu         | Seçin **Yeni Oluştur** ve türü *MyResourceGroupLB* metin kutusuna.|
    | Name                   | *myLoadBalancer*                                   |
    | Bölge         | **Batı Avrupa**'yı seçin.                                        |
    | Type          | Seçin **genel**.                                        |
    | SKU           | Seçin **standart**.                          |
    | Genel IP adresi | **Yeni oluştur**’u seçin. |
    | Ortak IP adresi adı              | Tür *Mypublicıp* metin kutusuna.   |
    |Kullanılabilirlik bölgesi| Seçin **bölgesel olarak yedekli**.    |
     
    >[!NOTE]
     >Load Balancer'ınız ve tüm kaynaklar için kullanılabilirlik alanlarını destekleyen bir konumda oluşturduğunuzdan emin olun. Daha fazla bilgi için [kullanılabilirlik alanlarını destekleyen bölgeler](../availability-zones/az-overview.md#services-support-by-region). 

3. İçinde **gözden geçir + Oluştur** sekmesinde **Oluştur**.  
  
## <a name="create-and-configure-back-end-servers"></a>Oluşturma ve arka uç sunucularını yapılandırma

Sanal ağ ile iki sanal makine oluşturma ve Vm'leri yük dengeleyicinizin arka uç havuzuna ekleyin. 

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

1. Portalda sol tarafta seçin **kaynak Oluştur** > **ağ** > **sanal ağ**.
   
1. İçinde **sanal ağ oluştur** bölmesinde yazın veya bu değerleri seçin:
   
   - **Ad**: Tür *MyVNet*.
   - **ResourceGroup**: Açılan menü **var olanı Seç** seçip **MyResourceGroupLB**. 
   - **Alt ağ** > **adı**: Tür *MyBackendSubnet*.
   
1. **Oluştur**’u seçin.

   ![Sanal ağ oluşturma](./media/tutorial-load-balancer-port-forwarding-portal/2-load-balancer-virtual-network.png)

### <a name="create-vms-and-add-them-to-the-load-balancer-back-end-pool"></a>Sanal makineleri oluşturun ve bunları yük dengeleyici arka uç havuzuna ekleyin

1. Portalda sol tarafta seçin **kaynak Oluştur** > **işlem** > **Windows Server 2016 Datacenter**. 
   
1. İçinde **sanal makine oluşturma**yazın veya aşağıdaki değerleri seçin **Temelleri** sekmesinde:
   - **Abonelik** > **kaynak grubu**: Açılır listesine tıklayıp **MyResourceGroupLB**.
   - **Sanal makine adı**: Tür *MyVM1*.
   - **Bölge**: **Batı Avrupa**'yı seçin. 
   - **Kullanıcı adı**: Tür *azureuser*.
   - **Parola**: Tür *Azure1234567*. 
     Parolayı yeniden yazın **parolayı onayla** alan.
   
1. Seçin **ağ** sekmesinde veya seçin **sonraki: Diskleri**, ardından **sonraki: Ağ**. 
   
   Aşağıdaki seçili olduğundan emin olun:
   - **Sanal ağ**: **MyVNet**
   - **Alt ağ**: **MyBackendSubnet**
   
1. Altında **genel IP**seçin **Yeni Oluştur**seçin **standart** üzerinde **genel IP adresi oluşturma** sayfasında ve ardından **Tamam**. 
   
1. Altında **ağ güvenlik grubu**seçin **Gelişmiş** yeni bir ağ güvenlik grubu (NSG), bir tür bir güvenlik duvarı oluşturmak için. 
   1. İçinde **yapılandırma ağ güvenlik grubu** alanın, Seç **Yeni Oluştur**. 
   1. Tür *Vm2*seçip **Tamam**. 
   
   >[!NOTE]
   >Varsayılan olarak, NSG zaten Uzak Masaüstü (RDP) bağlantı noktası olan 3389 numaralı bağlantı noktasını açmak için bir gelen kuralı olduğuna dikkat edin.
   
1. VM, oluşturduğunuz bir yük dengeleyici arka uç havuzuna ekleyin:
   
   1. Altında **yük DENGELEME** > **arkasındaki bir var olan yük dengeleme çözümü bu sanal makineyi yerleştirmek?** seçin **Evet**. 
   1. İçin **Yük Dengeleme seçeneklerini**, açılır listesine tıklayıp **Azure yük dengeleyici**. 
   1. İçin **yük dengeleyicisini işaretleyin**, açılır listesine tıklayıp **MyLoadBalancer**. 
   1. Altında **bir arka uç havuzunu seçin**seçin **Yeni Oluştur**, Anahtar'a *MyBackendPool*seçip **Oluştur**. 
   
   ![Sanal ağ oluşturma](./media/tutorial-load-balancer-port-forwarding-portal/create-vm-networking.png)
   
1. Seçin **Yönetim** sekmesinde veya seçin **sonraki** > **Yönetim**. Altında **izleme**ayarlayın **önyükleme tanılaması** için **kapalı**.
   
1. **İncele ve oluştur**’u seçin.
   
1. Ayarları gözden geçirin ve doğrulama başarılı olduğunda seçin **Oluştur**. 

1. Adlı ikinci bir VM oluşturmak için adımları *MyVM2*, diğer tüm ayarlarla MyVM1 ile aynı. 
   
   İçin **ağ güvenlik grubu**, seçtikten sonra **Gelişmiş**, açılır listesine tıklayıp **Vm2** önceden oluşturulmuş. 
   
   Altında **bir arka uç havuzunu seçin**, emin **MyBackendPool** seçilir. 

### <a name="create-an-nsg-rule-for-the-vms"></a>VM'ler için bir NSG kuralı oluşturma

Sanal makinelerin internet'e (HTTP) gelen bağlantılara izin vermek bir ağ güvenlik grubu (NSG) kuralı oluşturun.

>[!NOTE]
>NSG, varsayılan olarak, Uzak Masaüstü (RDP) bağlantı noktası olan 3389 numaralı bağlantı noktasını açan bir kural zaten sahip.

1. Soldaki menüden **Tüm kaynaklar**’ı seçin. Kaynak listesinden **Vm2** içinde **MyResourceGroupLB** kaynak grubu.
   
1. **Ayarlar** bölümünde **Gelen güvenlik kuralları**’nı ve sonra **Ekle**’yi seçin.
   
1. İçinde **gelen Güvenlik Kuralı Ekle** iletişim, tür veya aşağıdakileri seçin:
   
   - **Kaynak**: Seçin **hizmet etiketi**.  
   - **Kaynak hizmet etiketi**: Seçin **Internet**. 
   - **Hedef bağlantı noktası aralıkları**: Tür *80*.
   - **Protokol**: Seçin **TCP**. 
   - **Eylem**: Seçin **izin**.  
   - **Öncelik**: Tür *100*. 
   - **Ad**: Tür *MyHTTPRule*. 
   - **Açıklama**: Tür *HTTP'ye izin ver*. 
   
1. **Add (Ekle)** seçeneğini belirleyin. 
   
   ![NSG kuralı oluşturma](./media/tutorial-load-balancer-port-forwarding-portal/8-load-balancer-nsg-rules.png)
   
## <a name="create-load-balancer-resources"></a>Yük dengeleyici kaynakları oluşturma

Bu bölümde, yük dengeleyici arka uç havuzuna inceleyin ve bir yük dengeleyici durum araştırması ve trafik kuralları yapılandırın.

### <a name="view-the-back-end-address-pool"></a>Arka uç adres havuzu görüntüleyin

Trafiği Vm'lere dağıtmak için yük dengeleyiciye bağlı sanal ağ arabirimlerini (NIC'ler) IP adreslerini içeren bir arka uç adres havuzu, yük dengeleyici kullanır. 

Yük Dengeleyici arka uç havuzu oluşturduğunuz ve Vm'leri oluştururken VM'ler için eklendi. Ayrıca arka uç havuzları oluşturma ve ekleyebilir veya Vm'leri yük dengeleyiciden kaldırma **arka uç havuzları** sayfası. 

1. Seçin **tüm kaynakları** sol menüsünü ve ardından **MyLoadBalancer** kaynak listesinde.
   
1. **Ayarlar**’ın altında **Arka Uç Havuzları**’nı seçin.
   
1. Üzerinde **arka uç havuzları** sayfasında **MyBackendPool** ve her ikisi de emin **VM1** ve **VM2** listelenir.

1. Seçin **MyBackendPool**. 
   
   Üzerinde **MyBackendPool** sayfasındaki **sanal makine** ve **IP adresi**, kaldırın veya kullanılabilir VM'ler havuzuna ekleyin.

Seçerek yeni bir arka uç havuzları oluşturabilirsiniz **Ekle** üzerinde **arka uç havuzları** sayfası.

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

VM durumunu izlemek için yük dengeleyicisine izin vermek için durum araştırması kullanabilirsiniz. Durum yoklaması, durum denetimlerine verdikleri yanıtlara göre VM’leri dinamik olarak yük dengeleyici rotasyonuna ekler ve kaldırır. 

1. Seçin **tüm kaynakları** sol menüsünü ve ardından **MyLoadBalancer** kaynak listesinde.
   
1. **Ayarlar** bölümünde **Durum araştırmaları**’nı ve sonra **Ekle**’yi seçin.
   
1. Üzerinde **durum araştırması Ekle** sayfasında yazın veya aşağıdaki değerleri seçin:
   
   - **Ad**: Tür *MyHealthProbe*.
   - **Protokol**: Açılır listesine tıklayıp **HTTP**. 
   - **Bağlantı noktası**: Tür *80*. 
   - **Yol**: Kabul */* için varsayılan bir URI. Diğer bir URI ile bu değeri değiştirebilirsiniz. 
   - **Aralığı**: Tür *15*. Araştırma denemeleri arasındaki saniye sayısını aralığıdır.
   - **Sağlıksız durum eşiği**: Tür *2*. Bu değer bir VM kötü olarak kabul edilmeden önce gerçekleşmesi ardışık araştırma hatası sayısıdır.
   
1. **Tamam**’ı seçin.
   
   ![Bir araştırma eklemek](./media/tutorial-load-balancer-port-forwarding-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Yük dengeleyici kuralı, trafiğin sanal makinelere nasıl dağıtıldığını belirler. Ön uç IP yapılandırmasını gelen trafiğe, trafik ve gerekli kaynak ve hedef bağlantı noktalarını almak için arka uç IP havuzu için kuralı tanımlar. 

Adlı yük dengeleyici kuralı **MyLoadBalancerRule** ön 80 numaralı bağlantı noktasını dinler **LoadBalancerFrontEnd**. Kural ağ trafiği arka uç adres havuzuna gönderen **MyBackendPool**, bağlantı noktası 80'de. 

1. Seçin **tüm kaynakları** sol menüsünü ve ardından **MyLoadBalancer** kaynak listesinde.
   
1. **Ayarlar** bölümünde **Yük dengeleme kuralları**’nı ve sonra **Ekle**’yi seçin.
   
1. Üzerinde **Yük Dengeleme Kuralı Ekle** sayfasında yazın veya aşağıdaki değerleri seçin:
   
   - **Ad**: Tür *MyLoadBalancerRule*.
   - **Protokol**: Seçin **TCP**.
   - **Bağlantı noktası**: Tür *80*.
   - **Arka uç bağlantı noktası**: Tür *80*.
   - **Arka uç havuzu**: Seçin **MyBackendPool**.
   - **Durum araştırması**: Seçin **MyHealthProbe**. 
   
1. **Tamam**’ı seçin.
   
   ![Yük Dengeleyici Kuralı Ekle](./media/tutorial-load-balancer-port-forwarding-portal/5-load-balancing-rules.png)

## <a name="create-an-inbound-nat-port-forwarding-rule"></a>Gelen NAT bağlantı noktası iletme kuralı oluşturma

Trafiği bir arka uç sanal makinenin belirli bir bağlantı noktasıyla ön uç IP adresi belirli bir bağlantı noktasından iletmek için bir yük dengeleyici gelen ağ adresi çevirisi (NAT) kuralı oluşturun.

1. Seçin **tüm kaynakları** seçin ve soldaki menüden **MyLoadBalancer** kaynak listesinde.
   
1. Altında **ayarları**seçin **gelen NAT kuralları**ve ardından **Ekle**. 
   
1. Üzerinde **gelen NAT kuralı ekleyin** sayfasında yazın veya aşağıdaki değerleri seçin:
   
   - **Ad**: Tür *MyNATRuleVM1*.
   - **Bağlantı noktası**: Tür *4221*.
   - **Hedef sanal makine**: Seçin **MyVM1** açılır listeden.
   - **Bağlantı noktası eşlemesi**: Seçin **özel**.
   - **Hedef bağlantı noktası**: Tür *3389*.
   
1. **Tamam**’ı seçin.
   
1. Bir gelen NAT ekleme adımlarını kural adlı yineleme *MyNATRuleVM2*kullanarak **bağlantı noktası**: *4222* ve **hedef sanal makine**: **MyVM2**.

## <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme

Bu bölümde, Internet Information Services (IIS) arka uç sunucularına yükleyin ve makine adını göstermek için varsayılan web sayfasını özelleştirme. Ardından, Yük Dengeleyiciyi test etmek için yük dengeleyicinin genel IP adresini kullanacaksınız. 

Her arka uç VM, yük dengeleyicinin istekler iki VM arasında dağıtmasını görebilmeniz için varsayılan IIS web sayfasına, farklı bir sürümü işlevi görür.

### <a name="connect-to-the-vms-with-rdp"></a>Vm'lere RDP ile bağlanma

Her VM ile Uzak Masaüstü (RDP) bağlanın. 

1. Portalında **tüm kaynakları** sol menüsünde. Kaynak listesinden her bir sanal Makineye **MyResourceGroupLB** kaynak grubu.
   
1. Üzerinde **genel bakış** sayfasında **Connect**ve ardından **indirme RDP dosyası**. 
   
1. RDP dosyası indirilir ve seçin açık **Connect**.
   
1. Windows Güvenlik ekranında seçin **daha fazla seçenek** ardından **farklı bir hesap kullan**. 
   
   Kullanıcı adı girin *azureuser* ve parola *Azure1234567*seçip **Tamam**.
   
1. Yanıt **Evet** herhangi bir sertifika istemi. 
   
   VM masaüstüne yeni bir pencerede açılır. 

### <a name="install-iis-and-replace-the-default-iis-web-page"></a>IIS yüklemek ve varsayılan IIS web sayfasını değiştirin 

IIS yüklemek ve varsayılan IIS web sayfasına ve VM adını görüntüleyen bir sayfa ile değiştirmek için PowerShell kullanın.

1. MyVM1 ve MyVM2 başlatma **Windows PowerShell** gelen **Başlat** menüsü. 

2. IIS yüklemek ve varsayılan IIS web sayfasına değiştirmek için aşağıdaki komutları çalıştırın:
   
   ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file that displays server name
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
    
   ```
   
1. RDP bağlantıları MyVM1 ve MyVM2 seçerek kapatmak **Bağlantıyı Kes**. Vm'lerini kapatmanız yok.

### <a name="test-load-balancing"></a>Test Yük Dengeleme

1. Portalında, üzerinde **genel bakış** sayfasındaki **MyLoadBalancer**, altında genel IP adresini kopyalayın **genel IP adresi**. Seçin ve adresi üzerine geldiğinizde **kopyalama** simgesini kopyalayın. Bu örnekte olduğu **40.67.218.235**. 
   
1. Yük dengeleyicinin genel IP adresini yazın veya yapıştırın (*40.67.218.235*) internet tarayıcınızın adres çubuğuna. 
   
   Özelleştirilmiş bir IIS web sunucusunun varsayılan sayfası, tarayıcıda görüntülenir. Ya da iletiyi okur **Hello World MyVM1 gelen**, veya **Hello World MyVM2 gelen**.
   
   ![Yeni IIS varsayılan sayfası](./media/tutorial-load-balancer-port-forwarding-portal/9-load-balancer-test.png) 
   
1. Yük dengeleyicinin trafiği VM'ye dağıtmasını görmek için tarayıcıyı yenileyin. Bazen **MyVM1** sayfası görüntülenirse ve diğer zamanlarda **MyVM2** sayfası görüntülenirse, gibi yük dengeleyici yapılan istekler her arka uç VM'si dağıtır.
   
   >[!NOTE]
   >Denemeler arasındaki yeni bir tarayıcı penceresi açın veya tarayıcı önbelleğini temizlemeniz gerekebilir.

## <a name="test-port-forwarding"></a>Bağlantı noktası iletmeyi test etme

Bağlantı noktası iletme ile bir arka uç sanal makinesine Uzak Masaüstü IP adresini yük dengeleyici ve NAT kuralda tanımlanan ön uç bağlantı noktası değeri kullanarak yapabilirsiniz. 

1. Portalında, üzerinde **genel bakış** sayfasındaki **MyLoadBalancer**, genel IP adresini kopyalayın. Seçin ve adresi üzerine geldiğinizde **kopyalama** simgesini kopyalayın. Bu örnekte olduğu **40.67.218.235**. 
   
1. Bir komut istemi açın ve yük dengeleyicinin genel IP adresi ve sanal makinenin NAT kuralında tanımladığınız ön uç bağlantı noktasını kullanarak bir Uzak Masaüstü oturumu MyVM2 ile oluşturmak için aşağıdaki komutu kullanın. 
   
   ```
   mstsc /v:40.67.218.235:4222
   ```
  
1. İndirilen RDP dosyasını açın ve seçin **Connect**.
   
1. Windows Güvenlik ekranında seçin **daha fazla seçenek** ardından **farklı bir hesap kullan**. 
   
   Kullanıcı adı girin *azureuser* ve parola *Azure1234567*seçip **Tamam**.
   
1. Yanıt **Evet** herhangi bir sertifika istemi. 
   
   MyVM2 Masaüstü, yeni bir pencerede açılır. 

RDP bağlantı başarılı olduğundan gelen NAT kuralı **MyNATRuleVM2** MyVM2'ın bağlantı noktası 3389 (RDP bağlantı noktası) yük dengeleyicinin ön uç bağlantı noktası 4222 giden trafiği yönlendirir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız kalmadığında Yük Dengeleyiciyi ve tüm ilgili kaynakları silmek için açık **MyResourceGroupLB** kaynak grubu ve select **kaynak grubunu Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, standart bir genel yük dengeleyici oluşturdunuz. Oluşturduğunuz ve ağ kaynaklarına, arka uç sunucuları, bir durum araştırması ve yük dengeleyici kuralları yapılandırılmış. Arka uç sanal makinelere IIS yüklemesini ve Yük Dengeleyiciyi test etmek için yük dengeleyicinin genel IP adresi kullanılır. Ayarlar ve bir bağlantı noktasını arka uç VM'si için yük dengeleyicide belirli bir bağlantı noktasından bağlantı noktası iletme test. 

Azure Load Balancer hakkında daha fazla bilgi edinmek için daha dengeleyici öğreticiler yüklemek devam edin.

> [!div class="nextstepaction"]
> [Azure Load Balancer öğreticileri](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
