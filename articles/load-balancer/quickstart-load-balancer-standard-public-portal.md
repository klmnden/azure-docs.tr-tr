---
title: "Hızlı Başlangıç: standart yük dengeleyici - Azure portal'ı oluşturma"
titlesuffix: Azure Load Balancer
description: Bu hızlı başlangıçta, Azure portalını kullanarak bir Standard Load Balancer oluşturma işlemi gösterilmektedir.
services: load-balancer
documentationcenter: na
author: KumudD
manager: twooley
Customer intent: I want to create a Standard Load Balancer so that I can load balance internet traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/11/2019
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: f4f54273262f60dc01f78f4bb5828c8fdd2b97a9
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64707277"
---
# <a name="quickstart-create-a-standard-load-balancer-to-load-balance-vms-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak sanal makinelerde yük dengelemesi için Standart Yük Dengeleyici oluşturma

Yük dengeleme, gelen istekleri birden fazla sanal makineye yayarak daha yüksek bir kullanılabilirlik ve ölçek düzeyi sağlar. Sanal makinelerin (VM) yük dengelemesini yapmak amacıyla yük dengeleyici oluşturmak için Azure portalını kullanabilirsiniz. Bu hızlı başlangıçta Standart Yük Dengeleyici kullanılarak sanal makinelerde yük dengelemesi yapacağınız gösterilir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-standard-load-balancer"></a>Standart Yük Dengeleyici oluşturma

Bu bölümde, standart yük yardımcı olan sanal makinelerin yük dengelemesini dengeleyici oluşturun. Standart Yük Dengeleyici yalnızca Standart Genel IP adresini destekler. Standart Yük Dengeleyici oluşturduğunuzda, Standart Yük Dengeleyici için ön uç (varsayılan olarak *LoadBalancerFrontend* adını alır) olarak yapılandırılmış yeni bir Standart Genel IP adresi de oluşturmanız gerekir. 

1. Ekranın sol üst tarafında seçin **kaynak Oluştur** > **ağ** > **yük dengeleyici**.
2. İçinde **Temelleri** sekmesinde **yük dengeleyici Oluştur** sayfasında, girin veya aşağıdaki bilgileri seçin, geri kalan ayarlar için varsayılan değerleri kabul edin ve ardından **gözden geçir +Oluştur**:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Abonelik               | Aboneliğinizi seçin.    |    
    | Kaynak grubu         | Seçin **Yeni Oluştur** ve türü *myResourceGroupSLB* metin kutusuna.|
    | Ad                   | *myLoadBalancer*                                   |
    | Bölge         | **Doğu ABD**’yi seçin.                                        |
    | Tür          | Seçin **genel**.                                        |
    | SKU           | Seçin **standart**.                          |
    | Genel IP adresi | **Yeni oluştur**’u seçin. |
    | Ortak IP adresi adı              | Tür *Mypublicıp* metin kutusuna.   |
    |Kullanılabilirlik bölgesi| Seçin **bölgesel olarak yedekli**.    |
3. İçinde **gözden geçir + Oluştur** sekmesinde **Oluştur**.   

    ![Standart Yük Dengeleyici oluşturma](./media/quickstart-load-balancer-standard-public-portal/create-standard-load-balancer.png)

## <a name="create-load-balancer-resources"></a>Yük Dengeleyici kaynakları oluşturma

Bu bölümde, bir arka uç adres havuzu, bir durum araştırması için yük dengeleyici ayarlarını yapılandırın ve dengeleyici kuralı belirtin.

### <a name="create-a-backend-address-pool"></a>Arka uç adres havuzu oluşturma

Trafiği Vm'lere dağıtmak için bir arka uç adres havuzu içeren IP adreslerini sanal (NIC'ler), yük dengeleyiciye bağlı. Arka uç adres havuzu oluşturma *myBackendPool* sanal makineler için Yük Dengeleme internet trafiği dahil etmek için.

1. Seçin **tüm hizmetleri** sol taraftaki menüde **tüm kaynakları**ve ardından **myLoadBalancer** ve kaynak listesinden.
2. Altında **ayarları**seçin **arka uç havuzları**, ardından **Ekle**.
3. Üzerinde **bir arka uç havuzu Ekle** sayfasını, adını, türünü *myBackendPool*, arka uç havuzuna ve ardından adı olarak **Ekle**.

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

Yük dengeleyicinin uygulamanızın durumunu izlemesine izin vermek için durum araştırması kullanabilirsiniz. Durum yoklaması, dinamik olarak ekler veya sistem durumu denetimleri verdikleri yanıtlara göre yük dengeleyici rotasyonuna Vm'leri kaldırır. Sanal makinelerin durumunu izlemek için *myHealthProbe* durum araştırması oluşturun.

1. Seçin **tüm hizmetleri** sol taraftaki menüde **tüm kaynakları**ve ardından **myLoadBalancer** ve kaynak listesinden.
2. Altında **ayarları**seçin **sistem durumu araştırmalarının**, ardından **Ekle**.
    
    | Ayar | Değer |
    | ------- | ----- |
    | Ad | Girin *myHealthProbe*. |
    | Protokol | Seçin **HTTP**. |
    | Bağlantı noktası | Girin *80*.|
    | Interval | Girin *15* sayısı için **aralığı** araştırma denemeleri arasındaki saniye. |
    | İyi durumda olmayan eşik | Seçin **2** sayısı için **sağlıksız durum eşiği** veya VM sistem durumu kötü olarak kabul edilmeden önce gerçekleşmesi gereken ardışık araştırma hatası.|
    | | |
4. **Tamam**’ı seçin.

### <a name="create-a-load-balancer-rule"></a>Yük Dengeleyici kuralı oluşturma
Trafiğin sanal makinelere dağıtımını tanımlamak için bir Yük Dengeleyici kuralı kullanılır. Gerekli kaynak ve hedef bağlantı noktalarının yanı sıra gelen trafik için ön uç IP yapılandırması ve trafiği almak için arka uç IP havuzu tanımlamanız gerekir. Bir yük dengeleyici kuralı oluşturun *myLoadBalancerRuleWeb* ön uç havuzunda 80 numaralı bağlantı noktasını dinlemek *Mybackendpool* ve Yük Dengelemesi yapılmış ağ trafiğini arka uç adres havuzuna göndermek*myBackEndPool* de 80 numaralı bağlantı noktasını kullanıyor. 

1. Seçin **tüm hizmetleri** sol taraftaki menüde **tüm kaynakları**ve ardından **myLoadBalancer** ve kaynak listesinden.
2. Altında **ayarları**seçin **Yük Dengeleme kuralları**, ardından **Ekle**.
3. Yük dengeleme kuralını yapılandırmak için şu değerleri kullanın:
    
    | Ayar | Değer |
    | ------- | ----- |
    | Ad | Girin *myHTTPRule*. |
    | Protokol | Seçin **TCP**. |
    | Bağlantı noktası | Girin *80*.|
    | Arka uç bağlantı noktası | Girin *80*. |
    | Arka uç havuzu | Seçin *myBackendPool*.|
    | Durum yoklaması | Seçin *myHealthProbe*. |
4. Geri kalan varsayılan değerleri bırakın ve ardından **Tamam**.


## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu bölümde, bir sanal ağ oluşturma, yük dengeleyicinin arka uç havuzu için üç sanal makine oluşturur ve sonra Yük dengeleyicinin test edilmesi için sanal makinelere IIS yüklersiniz.

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma
1. Ekranın sol üst tarafında seçin **kaynak Oluştur** > **ağ** > **sanal ağ**.

1. İçinde **sanal ağ oluştur**bu bilgileri seçin veya girin:

    | Ayar | Değer |
    | ------- | ----- |
    | Ad | *myVNet* yazın. |
    | Adres alanı | Girin *10.1.0.0/16*. |
    | Abonelik | Aboneliğinizi seçin.|
    | Kaynak grubu | Mevcut kaynağı - seçin *myResourceGroupSLB*. |
    | Location | **Batı Avrupa**'yı seçin.|
    | Alt ağ - adı | *myBackendSubnet* yazın. |
    | Alt Ağ - Adres aralığı | Girin *10.1.0.0/24*. |
1. Geri kalan seçin ve varsayılan değerleri bırakın **Oluştur**.

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma
Standart yük dengeleyici yalnızca standart IP adresleri arka uç havuzundaki Vm'leri destekler. Bu bölümde, üç VM oluşturur (*myVM1*, *myVM2* ve *myVM3*) bir standart genel IP adresiyle üç farklı bölgelerde (*Bölge1*, *Bölge 2*, ve *bölge 3*) daha önce oluşturulan standart Load Balancer arka uç havuzu için daha sonra eklenen.

1. Portalda sol tarafta seçin **kaynak Oluştur** > **işlem** > **Windows Server 2019 Datacenter**. 
   
1. İçinde **sanal makine oluşturma**yazın veya aşağıdaki değerleri seçin **Temelleri** sekmesinde:
   - **Abonelik** > **kaynak grubu**: Seçin **myResourceGroupSLB**.
   - **Örnek ayrıntıları** > **sanal makine adı**: Tür *myVM1*.
   - **Örnek ayrıntıları** > **bölge** > seçin **Batı Avrupa**.
   - **Örnek ayrıntıları** > **kullanılabilirlik seçeneklerini** > seçin **kullanılabilirlik**. 
   - **Örnek ayrıntıları** > **kullanılabilirlik alanı** > seçin **1**.
   - **Yönetici hesabı**> Enter **kullanıcıadı**, **parola** ve **parolayı onayla** bilgileri.
   - Seçin **ağ** sekmesinde veya seçin **sonraki: Diskleri**, ardından **sonraki: Ağ**.
  
1. İçinde **ağ** sekmesinde aşağıdaki seçili olduğundan emin olun:
   - **Sanal ağ**: *myVnet*
   - **Alt ağ**: *myBackendSubnet*
   - **Genel IP** > seçin **Yeni Oluştur**hem de **genel IP adresi oluşturma** penceresinde için **SKU**seçin **standart**, ve **kullanılabilirlik alanı**seçin **bölgesel olarak yedekli**ve ardından **Tamam**.
   - Yeni bir ağ güvenlik grubu (NSG), güvenlik duvarı, türü altında oluşturulacak **ağ güvenlik grubu**seçin **Gelişmiş**. 
       1. İçinde **yapılandırma ağ güvenlik grubu** alanın, Seç **Yeni Oluştur**. 
       1. Tür *Vm2*seçip **Tamam**.
   - VM yük dengeleyicinin arka uç havuzu bir parçası yapmak için aşağıdaki adımları tamamlayın:
        - İçinde **Yük Dengeleme**, için **arkasındaki bir var olan yük dengeleme çözümü bu sanal makineyi yerleştirmek?** seçin **Evet**.
        - İçinde **yük dengeleme ayarlarını**, için **Yük Dengeleme seçeneklerini**seçin **Azure yük dengeleyici**.
        - İçin **yük dengeleyicisini işaretleyin**, *myLoadBalancer*.
        - Seçin **Yönetim** sekmesinde veya seçin **sonraki** > **Yönetim**.
2. İçinde **Yönetim** sekmesindeki **izleme**ayarlayın **önyükleme tanılaması** için **kapalı**. 
1. **İncele ve oluştur**’u seçin.   
1. Ayarları gözden geçirin ve ardından **Oluştur**.
1. Aşağıdaki değerleri ve diğer ayarları ile aynı iki ek sanal makineler oluşturmak için 2-6 adımları *myVM1*:

    | Ayar | VM 2| VM 3|
    | ------- | ----- |---|
    | Ad |  *myVM1* |*myVM3*|
    | Kullanılabilirlik bölgesi | 2 |3|
    |Genel IP| **Standart** SKU|**Standart** SKU|
    | Genel IP - kullanılabilirlik alanı| Bölgesel olarak yedekli |
    | Ağ güvenlik grubu | Var olan *myNetworkSecurity grubu*| Var olan *myNetworkSecurity grubu*|

 ### <a name="create-nsg-rule"></a>NSG kuralı oluşturma

Bu bölümde, HTTP kullanarak gelen bağlantılara izin vermek için bir ağ güvenlik grubu kuralı oluşturun.

1. Seçin **tüm hizmetleri** sol taraftaki menüde **tüm kaynakları**ve ardından select listesinde kaynaklardan **Vm2** içindebulunan**myResourceGroupSLB** kaynak grubu.
2. **Ayarlar** bölümünde **Gelen güvenlik kuralları**’nı ve sonra **Ekle**’yi seçin.
3. 80 numaralı bağlantı noktasını kullanarak gelen HTTP bağlantılarına izin vermek için *myHTTPRule* adlı gelen güvenlik kuralı için şu değerleri girin:
    - **Kaynak** için *Hizmet Etiketi*.
    - **Kaynak hizmet etiketi** için *İnternet*
    - **Hedef bağlantı noktası aralıkları** için *80*
    - **Protokol** için *TCP*
    - **Eylem** için *İzin Ver*
    - **Öncelik** için *100*
    - Ad için *myHTTPRule*
    - Açıklama için *HTTP’ye İzin Ver*
4. **Add (Ekle)** seçeneğini belirleyin.
 
### <a name="install-iis"></a>IIS yükleme

1. Seçin **tüm hizmetleri** sol taraftaki menüde **tüm kaynakları**ve ardından kaynak listesinden **myVM1** bulunan  *myResourceGroupSLB* kaynak grubu.
2. Sanal makineye yönelik RDP için **Genel Bakış** sayfasında **Bağlan**’ı seçin.
5. VM oluşturma işlemleri sırasında belirlediğiniz kimlik bilgilerini kullanarak VM'de oturum açın. *myVM1* adlı sanal makinede uzak masaüstü oturumu başlatılır.
6. Sunucu masaüstünde **Windows Yönetimsel Araçları**>**Windows PowerShell** bölümüne gidin.
7. PowerShell Penceresinde aşağıdaki komutları çalıştırarak IIS sunucusunu yükleyin, varsayılan iisstart.htm dosyasını kaldırın ve ardından VM’nin adını gösteren yeni bir iisstart.htm dosyasını ekleyin:

   ```azurepowershell-interactive
    
    # install IIS server role
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    # Add a new htm file that displays server name
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
   ```
6. *myVM1* ile RDP oturumunu kapatın.
7. IIS’yi ve *myVM2* ve *myVM3*’teki güncelleştirilmiş iisstart.htm dosyasını yüklemek için 1 ile 6 arasındaki adımları tekrarlayın.

## <a name="test-the-load-balancer"></a>Yük Dengeleyiciyi test etme
1. **Genel Bakış** ekranında Yük Dengeleyici için genel IP adresini bulun. Seçin **tüm hizmetleri** sol taraftaki menüde **tüm kaynakları**ve ardından **Mypublicıp**.

2. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın. IIS Web sunucusunun varsayılan sayfası, tarayıcıda görüntülenir.

   ![IIS Web sunucusu](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

Yük dengeleyicinin trafiği, tüm üç VM'ye dağıtmasını görmek için her bir sanal makinenin IIS Web sunucusunun varsayılan sayfası özelleştirebilir ve daha sonra istemci makineden web tarayıcınızı yenilemeye zorlayabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, Yük Dengeleyiciyi ve tüm ilgili kaynakları silin. Bunu yapmak için kaynak grubunu seçin (*myResourceGroupSLB*) içeren yük dengeleyici ve ardından **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir standart yük dengeleyici oluşturdunuz, Vm'lere bağlı, yük dengeleyici trafik kuralını ve durum yoklaması, yapılandırılmış ve sonra da Yük Dengeleyiciyi test ettiniz. Azure Load Balancer hakkında daha fazla bilgi almak için Azure Load Balancer öğreticisine devam edin.

> [!div class="nextstepaction"]
> [Azure Load Balancer öğreticileri](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
