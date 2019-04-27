---
title: 'Hızlı Başlangıç: Azure portalını kullanarak genel bir temel yük dengeleyici oluşturma'
titlesuffix: Azure Load Balancer
description: Bu hızlı başlangıçta, Azure portalını kullanarak genel bir Temel yük dengeleyicinin nasıl oluşturulacağı gösterilmektedir.
services: load-balancer
documentationcenter: na
author: KumudD
manager: twooley
Customer intent: I want to create a Basic Load balancer so that I can load balance internet traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/26/2019
ms.author: kumud
ms.custom: seodec18
ms.openlocfilehash: fe095b8f5a0080c0f28ec570303c9dc23962dfc8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60507968"
---
# <a name="quickstart-create-a-basic-load-balancer-by-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak bir temel yük dengeleyici oluşturma

Yük Dengeleme, sanal makineye (VM) gelen istekleri yayarak daha yüksek düzeyde kullanılabilirlik ve ölçek sağlar. Yük Dengeleyici oluşturma ve VM'ler arasında trafiği dengelemek için Azure portalını kullanabilirsiniz. Bu hızlı başlangıçta oluşturma ve bir yük dengeleyici, arka uç sunucularının ve ağ kaynakları temel fiyatlandırma katmanında yapılandırma gösterilmektedir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

Bu hızlı başlangıç görevleri yapmak için oturum açın [Azure portalında](https://portal.azure.com).

## <a name="create-a-basic-load-balancer"></a>Temel Yük Dengeleyici oluşturma

Portalı kullanarak ilk olarak genel bir temel yük dengeleyici oluşturun. Oluşturduğunuz genel IP adresi ve adını, load balancer'ın ön ucu olarak otomatik olarak yapılandırılır.

1. Ekranın sol üst kısmında **Kaynak oluştur** > **Ağ** > **Yük Dengeleyici**'ye tıklayın.
2. İçinde **Temelleri** sekmesinde **yük dengeleyici Oluştur** sayfasında, girin veya aşağıdaki bilgileri seçin, geri kalan ayarlar için varsayılan değerleri kabul edin ve ardından **gözden geçir +Oluştur**:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Abonelik               | Aboneliğinizi seçin.    |    
    | Kaynak grubu         | Seçin **Yeni Oluştur** ve türü *MyResourceGroupLB* metin kutusuna.|
    | Ad                   | *myLoadBalancer*                                   |
    | Bölge         | **Batı Avrupa**'yı seçin.                                        |
    | Tür          | Seçin **genel**.                                        |
    | SKU           | **Temel**'i seçin.                          |
    | Genel IP adresi | **Yeni oluştur**’u seçin. |
    | Ortak IP adresi adı              | *Mypublicıp*   |
    | Atama| Statik|

3. İçinde **gözden geçir + Oluştur** sekmesinde **Oluştur**.   


## <a name="create-back-end-servers"></a>Arka uç sunucuları oluşturma

Ardından, bir sanal ağ ve temel yük dengeleyicinizin arka uç havuzu için iki sanal makine oluşturun. 

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

1. Portalda sol tarafta seçin **kaynak Oluştur** > **ağ** > **sanal ağ**.
   
1. İçinde **sanal ağ oluştur** bölmesinde yazın veya bu değerleri seçin:
   
   - **Ad**: Tür *MyVnet*.
   - **ResourceGroup**: Açılan menü **var olanı Seç** seçip **MyResourceGroupLB**. 
   - **Alt ağ** > **adı**: Tür *MyBackendSubnet*.
   
1. **Oluştur**’u seçin.

   ![Sanal ağ oluşturma](./media/load-balancer-get-started-internet-portal/2-load-balancer-virtual-network.png)

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

1. Portalda sol tarafta seçin **kaynak Oluştur** > **işlem** > **Windows Server 2016 Datacenter**. 
   
1. İçinde **sanal makine oluşturma**yazın veya aşağıdaki değerleri seçin **Temelleri** sekmesinde:
   - **Abonelik** > **kaynak grubu**: Açılır listesine tıklayıp **MyResourceGroupLB**.
   - **Örnek ayrıntıları** > **sanal makine adı**: Tür *MyVM1*.
   - **Örnek ayrıntıları** > **kullanılabilirlik seçeneklerini**: 
     1. Açılır listesine tıklayıp **kullanılabilirlik kümesi**. 
     2. Seçin **Yeni Oluştur**, türü *MyAvailabilitySet*seçip **Tamam**.
  
1. Seçin **ağ** sekmesinde veya seçin **sonraki: Diskleri**, ardından **sonraki: Ağ**. 
   
   Aşağıdaki seçili olduğundan emin olun:
   - **Sanal ağ**: **myVnet**
   - **Alt ağ**: **MyBackendSubnet**
   - **Genel IP**: **MyVM1-ip**
   
   Yeni bir ağ güvenlik grubu (NSG), güvenlik duvarı, türü altında oluşturulacak **ağ güvenlik grubu**seçin **Gelişmiş**. 
   1. İçinde **yapılandırma ağ güvenlik grubu** alanın, Seç **Yeni Oluştur**. 
   1. Tür *Vm2*seçip **Tamam**. 
   
1. Seçin **Yönetim** sekmesinde veya seçin **sonraki** > **Yönetim**. Altında **izleme**ayarlayın **önyükleme tanılaması** için **kapalı**.
   
1. **İncele ve oluştur**’u seçin.
   
1. Ayarları gözden geçirin ve ardından **Oluştur**. 

1. Adlı ikinci bir VM oluşturmak için adımları *MyVM2*, ile bir **genel IP** adresini *MyVM2 IP*ve diğer ayarları MyVM1 ile aynı. 

### <a name="create-nsg-rules-for-the-vms"></a>VM'ler için NSG kuralları oluşturma

Bu bölümde, ağ güvenlik grubu (NSG) kuralları için gelen İnternet'e (HTTP) ve Uzak Masaüstü (RDP) bağlantılarına izin vermek için sanal makinelerin oluşturun.

1. Soldaki menüden **Tüm kaynaklar**’ı seçin. Kaynak listesinden **Vm2** içinde **MyResourceGroupLB** kaynak grubu.
   
1. **Ayarlar** bölümünde **Gelen güvenlik kuralları**’nı ve sonra **Ekle**’yi seçin.
   
1. İçinde **gelen Güvenlik Kuralı Ekle** iletişim kutusunda, HTTP kural, tür veya aşağıdakileri seçin:
   
   - **Kaynak**: Seçin **hizmet etiketi**.  
   - **Kaynak hizmet etiketi**: Seçin **Internet**. 
   - **Hedef bağlantı noktası aralıkları**: Tür *80*.
   - **Protokol**: Seçin **TCP**. 
   - **Eylem**: Seçin **izin**.  
   - **Öncelik**: Tür *100*. 
   - **Ad**: Tür *MyHTTPRule*. 
   - **Açıklama**: Tür *HTTP'ye izin ver*. 
   
1. **Add (Ekle)** seçeneğini belirleyin. 
   
   ![NSG kuralı oluşturma](./media/load-balancer-get-started-internet-portal/8-load-balancer-nsg-rules.png)
   
1. Aşağıdaki farklı değerlerle gelen RDP kuralı için adımları yineleyin:
   - **Hedef bağlantı noktası aralıkları**: Tür *3389*.
   - **Öncelik**: Tür *200*. 
   - **Ad**: Tür *MyRDPRule*. 
   - **Açıklama**: Tür *RDP'ye izin ver*. 

## <a name="create-resources-for-the-load-balancer"></a>Yük Dengeleyici kaynakları oluşturma

Bu bölümde, bir arka uç adres havuzu, bir durum araştırması ve yük dengeleyici kuralı için yük dengeleyici ayarlarını yapılandırın.

### <a name="create-a-backend-address-pool"></a>Arka uç adres havuzu oluşturma

Trafiği Vm'lere dağıtmak için yük dengeleyici arka uç adres havuzu kullanır. Arka uç adres havuzundaki IP adreslerini yük dengeleyiciye bağlı sanal ağ arabirimlerini (NIC'ler) içerir. 

**VM1 ve VM2 içeren bir arka uç adres havuzu oluşturmak için:**

1. Seçin **tüm kaynakları** sol menüsünü ve ardından **MyLoadBalancer** kaynak listesinde.
   
1. **Ayarlar** bölümünde **Arka uç havuzları**’nı ve sonra **Ekle**’yi seçin.
   
1. Üzerinde **bir arka uç havuzu Ekle** sayfasında yazın veya aşağıdaki değerleri seçin:
   
   - **Ad**: Tür *MyBackEndPool*.
   - **İlişkili**: Açılır listesine tıklayıp **kullanılabilirlik kümesi**.
   - **Kullanılabilirlik kümesi**: Seçin **MyAvailabilitySet**.
   
1. Seçin **hedef ağ IP Yapılandırması Ekle**. 
   1. Her sanal makine ekleyin (**MyVM1** ve **MyVM2**) arka uç havuzu için oluşturulan.
   2. Her makine ekledikten sonra açılır listesine tıklayıp, **ağ IP yapılandırması**. 
   
1. **Tamam**’ı seçin.
   
   ![Arka uç adres havuzu ekleme](./media/load-balancer-get-started-internet-portal/3-load-balancer-backend-02.png)
   
1. Üzerinde **arka uç havuzları** sayfasında **MyBackendPool** ve her ikisi de emin **VM1** ve **VM2** listelenir.

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

VM durumunu izlemek için yük dengeleyicisine izin vermek için durum araştırması kullanabilirsiniz. Durum yoklaması, durum denetimlerine verdikleri yanıtlara göre VM’leri dinamik olarak yük dengeleyici rotasyonuna ekler ve kaldırır. 

**Sanal makinelerin durumunu izlemek için bir durum araştırması oluşturmak için:**

1. Seçin **tüm kaynakları** sol menüsünü ve ardından **MyLoadBalancer** kaynak listesinde.
   
1. **Ayarlar** bölümünde **Durum araştırmaları**’nı ve sonra **Ekle**’yi seçin.
   
1. Üzerinde **bir durum araştırması Ekle** sayfasında yazın veya aşağıdaki değerleri seçin:
   
   - **Ad**: Tür *MyHealthProbe*.
   - **Protokol**: Açılır listesine tıklayıp **HTTP**. 
   - **Bağlantı noktası**: Tür *80*. 
   - **Yol**: Kabul */* için varsayılan bir URI. Diğer bir URI ile bu değeri değiştirebilirsiniz. 
   - **Aralığı**: Tür *15*. Araştırma denemeleri arasındaki saniye sayısını aralığıdır.
   - **Sağlıksız durum eşiği**: Tür *2*. Bu değer bir VM kötü olarak kabul edilmeden önce gerçekleşmesi ardışık araştırma hatası sayısıdır.
   
1. **Tamam**’ı seçin.
   
   ![Bir araştırma eklemek](./media/load-balancer-get-started-internet-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Yük dengeleyici kuralı, trafiğin sanal makinelere nasıl dağıtıldığını belirler. Ön uç IP yapılandırmasını gelen trafiğe, trafik ve gerekli kaynak ve hedef bağlantı noktalarını almak için arka uç IP havuzu için kuralı tanımlar. 

Adlı yük dengeleyici kuralı **MyLoadBalancerRule** ön 80 numaralı bağlantı noktasını dinler **LoadBalancerFrontEnd**. Kural ağ trafiği arka uç adres havuzuna gönderen **MyBackEndPool**, bağlantı noktası 80'de. 

**Yük Dengeleyici kuralı oluşturmak için:**


1. Seçin **tüm kaynakları** sol menüsünü ve ardından **MyLoadBalancer** kaynak listesinde.
   
1. **Ayarlar** bölümünde **Yük dengeleme kuralları**’nı ve sonra **Ekle**’yi seçin.
   
1. Üzerinde **Yük Dengeleme Kuralı Ekle** sayfasında yazın veya aşağıdaki değerleri seçin:
   
   - **Ad**: Tür *MyLoadBalancerRule*.
   - **Ön uç IP adresi:** Tür *LoadBalancerFrontend*.
   - **Protokol**: Seçin **TCP**.
   - **Bağlantı noktası**: Tür *80*.
   - **Arka uç bağlantı noktası**: Tür *80*.
   - **Arka uç havuzu**: Seçin **MyBackendPool**.
   - **Durum araştırması**: Seçin **MyHealthProbe**. 
   
1. **Tamam**’ı seçin.
   
   ![Yük Dengeleyici Kuralı Ekle](./media/load-balancer-get-started-internet-portal/5-load-balancing-rules.png)

## <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme

Vm'lerde Yük Dengeleyiciyi sınamak için genel IP adresini kullanacaksınız. 

Portalında, üzerinde **genel bakış** sayfasındaki **MyLoadBalancer**, altında genel IP adresini bulun **genel IP adresi**. Seçin ve adresi üzerine geldiğinizde **kopyalama** simgesini kopyalayın. 

### <a name="install-iis-on-the-vms"></a>Sanal makinelere IIS yükleme

Internet Information Services (IIS), yük dengeleyicinin test edilmesi için sanal makinelere yükleyin.

**VM ile Uzak Masaüstü (RDP):**

1. Portalında **tüm kaynakları** sol menüsünde. Kaynak listesinden **MyVM1** içinde **MyResourceGroupLB** kaynak grubu.
   
1. Üzerinde **genel bakış** sayfasında **Connect**ve ardından **indirme RDP dosyası**. 
   
1. RDP dosyası indirilir ve seçin açık **Connect**.
   
1. Windows Güvenlik ekranında seçin **daha fazla seçenek** ardından **farklı bir hesap kullan**. 
   
   Kullanıcı adı ve parola girin ve seçin **Tamam**.
   
1. Yanıt **Evet** herhangi bir sertifika istemi. 
   
   VM masaüstüne yeni bir pencerede açılır. 
   
**Bir VM'de IIS yüklemek için:**

1. Varsa **Sunucu Yöneticisi'ni** zaten sunucu masaüstünde açın, Gözat değil **Windows Yönetim Araçları** > **Sunucu Yöneticisi**.
   
1. İçinde **Sunucu Yöneticisi'ni**seçin **rol ve Özellik Ekle**.
   
   ![Sunucu Yöneticisi rolü ekleme](./media/load-balancer-get-started-internet-portal/servermanager.png)
   
1. İçinde **rol ve Özellik Ekle Sihirbazı**:
   1. **Yükleme türünü seçin** sayfasında **Rol tabanlı veya özellik tabanlı yükleme**’yi seçin.
   1. Üzerinde **hedef sunucuyu seçin** sayfasında **MyVM1**.
   1. **Sunucu rolü seçin** sayfasında **Web Sunucusu (IIS)** seçeneğini belirleyin. 
   1. Gerekli Araçları'nı yüklemek için komut isteminde, seçin **Özellik Ekle**. 
   1. Varsayılanları kabul edin ve seçin **yükleme**. 
   1. Özellikleri bittiğinde yüklemeden, seçin **Kapat**. 
   
1. Sanal makine için adımları yineleyin **MyVM2**, dışındaki hedef sunucuda ayarlanmış **MyVM2**.

### <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme

Bir tarayıcı açın ve yük dengeleyicinin genel IP adresi tarayıcının adres çubuğuna yapıştırın. IIS web sunucusunun varsayılan sayfası, tarayıcıda görüntülenmesi gerekir.

![IIS web sunucusu](./media/load-balancer-get-started-internet-portal/9-load-balancer-test.png)

Yük dengeleyicinin trafiği, uygulamanızı çalıştıran üç VM’ye dağıtmasını görmek için web tarayıcınızı yenilemeye zorlayabilirsiniz.
## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız kalmadığında Yük Dengeleyiciyi ve tüm ilgili kaynakları silmek için açık **MyResourceGroupLB** kaynak grubu ve select **kaynak grubunu Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir temel katman yük dengeleyici oluşturdunuz. Oluşturduğunuz ve bir kaynak grubu, ağ kaynaklarına, arka uç sunucuları, bir durum araştırması ve kuralları yük dengeleyiciyle kullanmak için yapılandırılmış. Sanal makinelere IIS yüklemesini ve Yük Dengeleyiciyi test etmek için kullanılır. 

Azure Load Balancer hakkında daha fazla bilgi edinmek için öğreticilere geçin.

> [!div class="nextstepaction"]
> [Azure Load Balancer öğreticileri](tutorial-load-balancer-basic-internal-portal.md)
