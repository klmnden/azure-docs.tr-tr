---
title: "Öğretici: Yük Dengeleme internet trafiği Vm'lere - Azure portalı"
titlesuffix: Azure Load Balancer
description: Bu öğreticide, Azure portalını kullanarak Standard Load Balancer'ın nasıl oluşturulacağı ve yönetileceği gösterilir.
services: load-balancer
documentationcenter: na
author: KumudD
manager: twooley
Customer intent: I want to create and Standard Load Balancer so that I can load balance internet traffic to VMs and add and remove VMs from the load-balanced set.
ms.service: load-balancer
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/11/2019
ms.author: kumud
ms.custom: seodec18
ms.openlocfilehash: 78266e447d1ddf6daf5a9b0ad9172ab6470bf0c6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61320866"
---
# <a name="tutorial-load-balance-internet-traffic-to-vms-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak sanal makineleri internet trafiği Yük Dengelemesi

Yük dengeleme, gelen istekleri birden fazla sanal makineye yayarak daha yüksek bir kullanılabilirlik ve ölçek düzeyi sağlar. Bu öğreticide, internet trafiği vm'lere dağıtmak ve yüksek kullanılabilirlik sağlayan farklı bileşenleri Azure Standard Load Balancer hakkında bilgi edinin. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:


> [!div class="checklist"]
> * Bir Azure yük dengeleyici oluşturma
> * Yük Dengeleyici kaynakları oluşturma
> * Sanal makineler oluşturma ve IIS sunucusunu yükleme
> * Yük Dengeleyiciyi çalışırken görüntüle
> * Vm'leri ekleme ve bir yük Dengeleyiciden kaldırma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-standard-load-balancer"></a>Standart Yük Dengeleyici oluşturma

Bu bölümde, standart yük yardımcı olan sanal makinelerin yük dengelemesini dengeleyici oluşturun. Standart Yük Dengeleyici yalnızca Standart Genel IP adresini destekler. Standard Load Balancer oluşturduğunuzda, Standard Load Balancer için ön uç (varsayılan olarak *LoadBalancerFrontend* adını alır) olarak yapılandırılmış yeni bir Standart Genel IP adresi de oluşturmanız gerekir. 

1. Ekranın sol üst kısmında **Kaynak oluştur** > **Ağ** > **Yük Dengeleyici**'ye tıklayın.
2. İçinde **Temelleri** sekmesinde **yük dengeleyici Oluştur** sayfasında, girin veya aşağıdaki bilgileri seçin, geri kalan ayarlar için varsayılan değerleri kabul edin ve ardından **gözden geçir +Oluştur**:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Abonelik               | Aboneliğinizi seçin.    |    
    | Kaynak grubu         | Seçin **Yeni Oluştur** ve türü *myResourceGroupSLB* metin kutusuna.|
    | Ad                   | *myLoadBalancer*                                   |
    | Bölge         | **Batı Avrupa**'yı seçin.                                        |
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
2. **Ayarlar** bölümünde **Arka uç havuzları**’na ve sonra **Ekle**’ye tıklayın.
3. Üzerinde **bir arka uç havuzu Ekle** sayfasını, adını, türünü *myBackendPool*, arka uç havuzuna ve ardından adı olarak **Ekle**.

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

Yük dengeleyicinin uygulamanızın durumunu izlemesine izin vermek için durum araştırması kullanabilirsiniz. Durum yoklaması, dinamik olarak ekler veya sistem durumu denetimleri verdikleri yanıtlara göre yük dengeleyici rotasyonuna Vm'leri kaldırır. Sanal makinelerin durumunu izlemek için *myHealthProbe* durum araştırması oluşturun.

1. Seçin **tüm hizmetleri** sol taraftaki menüde **tüm kaynakları**ve ardından **myLoadBalancer** ve kaynak listesinden.
2. **Ayarlar** bölümünde **Durum araştırmaları**’na ve sonra **Ekle**’ye tıklayın.
3. Durum araştırması oluşturmak için şu değerleri kullanın:
     
    | Ayar | Değer |
    | ------- | ----- |
    | Ad | Girin *myHealthProbe*. |
    | Protokol | Seçin **HTTP**. |
    | Bağlantı noktası | Girin *80*.|
    | Interval | Girin *15* sayısı için **aralığı** araştırma denemeleri arasındaki saniye. |
    | İyi durumda olmayan eşik | Seçin *2* sayısı için **sağlıksız durum eşiği** veya VM sistem durumu kötü olarak kabul edilmeden önce gerçekleşmesi gereken ardışık araştırma hatası.|
    | Durum yoklaması | Seçin *myHealthProbe*. |
    
4. **Tamam**’ı seçin.

### <a name="create-a-load-balancer-rule"></a>Yük Dengeleyici kuralı oluşturma

Trafiğin sanal makinelere dağıtımını tanımlamak için bir Yük Dengeleyici kuralı kullanılır. Gerekli kaynak ve hedef bağlantı noktalarının yanı sıra gelen trafik için ön uç IP yapılandırması ve trafiği almak için arka uç IP havuzu tanımlamanız gerekir. Bir yük dengeleyici kuralı oluşturun *myLoadBalancerRuleWeb* ön uç havuzunda 80 numaralı bağlantı noktasını dinlemek *Mybackendpool* ve Yük Dengelemesi yapılmış ağ trafiğini arka uç adres havuzuna göndermek*myBackEndPool* de 80 numaralı bağlantı noktasını kullanıyor.

1. Seçin **tüm hizmetleri** sol taraftaki menüde **tüm kaynakları**ve ardından **myLoadBalancer** ve kaynak listesinden.
2. **Ayarlar** bölümünde **Yük dengeleme kuralları**’na ve sonra **Ekle**’ye tıklayın.
3. Yük Dengeleme kuralını yapılandırmak için şu değerleri kullanın:

    | Ayar | Değer |
    | ------- | ----- |
    | Ad | Girin *myHTTPRule*. |
    | Protokol | Seçin **TCP**. |
    | Bağlantı noktası | Girin *80*.|
    | Arka uç bağlantı noktası | Girin *80*. |
    | Arka uç havuzu | Seçin *myBackendPool*.|
    | Durum yoklaması | Seçin *myHealthProbe*. |
    
4. Diğer varsayılan ayarları olduğu gibi bırakın ve **Tamam**’ı seçin.

## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu bölümde, bir sanal ağ oluşturma, yük dengeleyicinin arka uç havuzu için üç sanal makine oluşturur ve sonra Yük dengeleyicinin test edilmesi için sanal makinelere IIS yüklersiniz.

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

1. Ekranın sol üst tarafında seçin **kaynak Oluştur** > **ağ** > **sanal ağ**.
2. İçinde **sanal ağ oluştur**bu bilgileri seçin veya girin:

    | Ayar | Değer |
    | ------- | ----- |
    | Ad | *myVNet* yazın. |
    | Adres alanı | Girin *10.1.0.0/16*. |
    | Abonelik | Aboneliğinizi seçin.|
    | Kaynak grubu | Mevcut kaynağı - seçin *myResourceGroupSLB*. |
    | Location | **Batı Avrupa**'yı seçin.|
    | Alt ağ - adı | *myBackendSubnet* yazın. |
    | Alt Ağ - Adres aralığı | Girin *10.1.0.0/24*. |
    
3. Geri kalan seçin ve varsayılan değerleri bırakın **Oluştur**.

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Standart yük dengeleyici yalnızca standart IP adresleri arka uç havuzundaki Vm'leri destekler. Bu bölümde, üç VM oluşturur (*myVM1*, *myVM2*, ve *myVM3*) bir standart genel IP adresiyle üç farklı bölgelerde (*Bölge1*, *Bölge 2*, ve *bölge 3*) daha önce oluşturulan standart Load Balancer arka uç havuzuna eklenir.

1. Portalda sol tarafta seçin **kaynak Oluştur** > **işlem** > **Windows Server 2016 Datacenter**. 
   
1. İçinde **sanal makine oluşturma**yazın veya aşağıdaki değerleri seçin **Temelleri** sekmesinde:
   - **Abonelik** > **kaynak grubu**: Seçin **myResourceGroupSLB**.
   - **Örnek ayrıntıları** > **sanal makine adı**: Tür *myVM1*.
   - **Örnek ayrıntıları** > **bölge** > seçin **Batı Avrupa**.
   - **Örnek ayrıntıları** > **kullanılabilirlik seçeneklerini** > seçin **kullanılabilirlik**. 
   - **Örnek ayrıntıları** > **kullanılabilirlik alanı** > seçin **1**.
  
1. Seçin **ağ** sekmesinde veya seçin **sonraki: Diskleri**, ardından **sonraki: Ağ**. 
   
   - Aşağıdaki seçili olduğundan emin olun:
       - **Sanal ağ**: **myVnet**
       - **Alt ağ**: **myBackendSubnet**
       - **Genel IP** > seçin **Yeni Oluştur**hem de **genel IP adresi oluşturma** penceresinde için **SKU**seçin **standart**, ve **kullanılabilirlik alanı**seçin **bölgesel olarak yedekli**
      
   - Yeni bir ağ güvenlik grubu (NSG), güvenlik duvarı, türü altında oluşturulacak **ağ güvenlik grubu**seçin **Gelişmiş**. 
       1. İçinde **yapılandırma ağ güvenlik grubu** alanın, Seç **Yeni Oluştur**. 
       1. Tür *Vm2*seçip **Tamam**.

   - VM yük dengeleyicinin arka uç havuzu bir parçası yapmak için aşağıdaki adımları tamamlayın:
        - İçinde **Yük Dengeleme**, için **arkasındaki bir var olan yük dengeleme çözümü bu sanal makineyi yerleştirmek?** seçin **Evet**.
        - İçinde **yük dengeleme ayarlarını**, için **Yük Dengeleme seçeneklerini**seçin **Azure yük dengeleyici**.
        - İçin **yük dengeleyicisini işaretleyin**, *myLoadBalancer*. 
1. Seçin **Yönetim** sekmesinde veya seçin **sonraki** > **Yönetim**. Altında **izleme**ayarlayın **önyükleme tanılaması** için **kapalı**. 
1. **İncele ve oluştur**’u seçin.   
1. Ayarları gözden geçirin ve ardından **Oluştur**.
1. İki ek VM - oluşturma adımları izleyerek *myVM2* ve *myVM3*, bir standart SKU genel IP adresi ile **kullanılabilirlik alanı** **2** ve **3** sırasıyla ve diğer ayarları aynı *myVM1*.  

### <a name="create-network-security-group-rule"></a>Ağ güvenlik grubu kuralı oluşturma

Bu bölümde, HTTP kullanarak gelen bağlantılara izin vermek için bir ağ güvenlik grubu kuralı oluşturun.

1. Seçin **tüm hizmetleri** sol taraftaki menüde **tüm kaynakları**ve ardından'a tıklayın ve kaynak listesinden **Vm2** içindebulunan**myResourceGroupSLB** kaynak grubu.
2. **Ayarlar**’ın altında **Gelen güvenlik kuralları**’na ve sonra **Ekle**’ye tıklayın.
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

### <a name="install-iis-on-vms"></a>VM’lere IIS yükleme

1. Seçin **tüm hizmetleri** sol taraftaki menüde **tüm kaynakları**ve ardından'a tıklayın ve kaynak listesinden **myVM1** bulunan  *myResourceGroupSLB* kaynak grubu.
2. Sanal makineye yönelik RDP için **Genel Bakış** sayfasında **Bağlan**’a tıklayın.
3. **Sanal makineye bağlanın** açılır penceresinde **RDP Dosyasını İndir**'i seçin ve indirilen RDP dosyasını açın.
4. **Uzak Masaüstü Bağlantısı** penceresinde **Bağlan**'a tıklayın.
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

Yük dengeleyicinin trafiği, uygulamanızı çalıştıran üç VM'ye dağıtmasını görmek için web tarayıcınızı yenilemeye zorlayabilirsiniz.

## <a name="remove-or-add-vms-from-the-backend-pool"></a>Arka uç havuzunda VM'leri kaldırma veya ekleme
Uygulamanızı çalıştıran VM’lerde işletim sistemi güncelleştirmelerini yükleme gibi bakım işlemleri gerçekleştirmeniz gerekebilir. Uygulamanıza gelen trafiği fazla trafiği yönetmek için ek VM’lere ihtiyaç duyabilirsiniz. Bu bölümde bir VM eklemek veya kaldırmak gösterilmektedir (*myVM1*) yük dengeleyiciden.

### <a name="remove-vm-from-a-backend-pool"></a>VM bir arka uç havuzundan Kaldır
Kaldırılacak *myVM1* arka uç havuzundan aşağıdaki adımları tamamlayın:

1. Seçin **tüm hizmetleri** sol taraftaki menüde **tüm kaynakları**ve ardından **myLoadBalancer** ve kaynak listesinden.
2. **Ayarlar**'ın altında **Arka uç havuzları**'na tıklayın, ardından arka uç havuzunun listesinde **myBackendPool**'a tıklayın.
3. Üzerinde **myBackendPool** sayfasında kaldırmak için *VM1* görüntüler satırın sonunda Sil simgesini *myVM1*ve ardından **Kaydet**.

Artık *myVM1* arka uç adres havuzunda yer almadığından, *myVM1* üzerinde yazılım güncelleştirmelerini yükleme gibi tüm bakım görevlerini gerçekleştirebilirsiniz. Mevcut olmadığında *VM1*, yükü artık dengelenir *myVM2* ve *myVM3*. 

### <a name="add-vm-to-a-backend-pool"></a>Bir arka uç havuzuna VM ekleme
Eklenecek *myVM1* geri arka uç havuzu için aşağıdaki adımları tamamlayın:

1. Seçin **tüm hizmetleri** sol taraftaki menüde **tüm kaynakları**ve ardından **myVM1** ve kaynak listesinden.
2. İçinde **VM1** sayfasındaki **ayarları**seçin **ağ**.
3. İçinde **ağ** sayfasında **Yük Dengeleme** sekmesine tıklayın ve ardından **Yük Dengeleme eklemek**.
4. İçinde **Yük Dengeleme eklemek** sayfasında, aşağıdakileri yapın:
   1. İçin **Yük Dengeleme seçeneklerini**seçin **Azure yük dengeleyici**.
   2. İçin **yük dengeleyicisini işaretleyin**seçin *myLoadBalancer*.
   3. İçin **bir arka uç havuzunu seçin**seçin *myBackendPool*. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, Yük Dengeleyiciyi ve tüm ilgili kaynakları silin. Bunu yapmak için *myResouceGroupSLB* yük dengeleyici içeren ve ardından kaynak grubu **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir standart yük dengeleyici oluşturdunuz, Vm'lere bağlı, yük dengeleyici trafik kuralını ve durum yoklaması, yapılandırılmış ve Load Balancer'ı test. Ayrıca, yük dengeli kümeden bir VM kaldırdınız ve VM'yi arka uç adres havuzuna geri eklediniz. Azure Load Balancer hakkında daha fazla bilgi almak için Azure Load Balancer öğreticisine devam edin.

> [!div class="nextstepaction"]
> [Azure Load Balancer öğreticileri](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
