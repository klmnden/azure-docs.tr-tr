---
title: Genel Temel Yük Dengeleyici - Azure portalı | Microsoft Docs
description: Azure portalını kullanarak genel bir Temel Yük Dengeleyici nasıl oluşturabileceğinizi öğrenin.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: aa9d26ca-3d8a-4a99-83b7-c410dd20b9d0
ms.service: load-balancer
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2018
ms.author: kumud
ms.openlocfilehash: 1b7901542a699e74f65527bf734133f73acb0bea
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="create-a-public-basic-load-balancer-to-load-balance-vms-using-the-azure-portal"></a>Azure portalını kullanarak sanal makinelerin yük dengelemesi için genel bir Temel Yük Dengeleyici oluşturma

Yük dengeleme, gelen istekleri birden fazla sanal makineye yayarak daha yüksek bir kullanılabilirlik ve ölçek düzeyi sağlar. Sanal makinelerin yük dengelemesini yapmak amacıyla yük dengeleyici oluşturmak için Azure portalını kullanabilirsiniz. Bu hızlı başlangıçta, ağ kaynaklarının, arka uç sunucularının ve genel bir Temel Yük Dengeleyicinin nasıl oluşturulacağı gösterilmektedir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[http://portal.azure.com](http://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-basic-load-balancer"></a>Temel Yük Dengeleyici oluşturma

Bu bölümde, portalı kullanarak genel bir Temel Yük Dengeleyici oluşturacaksınız. Genel IP adresi, portalı kullanarak yük dengeleyici kaynağını oluştururken genel IP’yi oluşturduğunuzda Yük Dengeleyicinin *LoadBalancerFrontend* adlı ön ucu olarak otomatik şekilde yapılandırılır.

1. Ekranın sol üst kısmında **Kaynak oluştur** > **Ağ** > **Yük Dengeleyici** seçeneklerine tıklayın.
2. **Yük dengeleyici oluştur** sayfasına, yük dengeleyici için şu değerleri girin:
    - *myLoadBalancer* - yük dengeleyicinin adı için.
    - **Public** - yük dengeleyicinin fonrt türü için 
     - *myPublicIP* - SKU **Temel** ve **Atama** **Dinamik** olarak ayarlanmış şekilde oluşturmanız gereken Genel IP için.
    - *myResourceGroupLB* - oluşturduğunuz yeni kaynak grubunun adı için.
3. Yük dengeleyiciyi oluşturmak için **Oluştur**’a tıklayın.
   
    ![Yük dengeleyici oluşturma](./media/load-balancer-get-started-internet-portal/1-load-balancer.png)


## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu bölümde, bir sanal ağ oluşturur, Temel Yük Dengeleyicinizin arka uç havuzu için iki sanal makine oluşturur ve sonra yük dengeleyicinin test edilmesi için sanal makinelere IIS yüklersiniz.

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma
1. Ekranın sol üst kısmında **Yeni** > **Ağ** > **Sanal ağ** seçeneklerine tıklayın ve sanal ağ için şu değerleri girin:
    - Sanal ağın adı için *myVnet*.
    - Mevcut kaynak grubunun adı için *myResourceGroupLB*
    - Alt ağ adı için *myBackendSubnet*.
2. Sanal ağı oluşturmak için **Oluştur**’a tıklayın.

    ![Sanal ağ oluşturma](./media/load-balancer-get-started-internet-portal/2-load-balancer-virtual-network.png)

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

1. Ekranın sol üst kısmında **Yeni** > **İşlem** > **Windows Server 2016 Datacenter** seçeneklerine tıklayın ve sanal makine için şu değerleri girin:
    - Sanal makinenin adı için *myVM1*.        
    - Yönetici kullanıcı adı için *azureuser*. -    
    - *myResourceGroupLB* - **Kaynak grubu** için **Var olanı kullan**’ı seçin ve sonra *myResourceGroupLB* seçeneğini belirleyin.
2. **Tamam**’a tıklayın.
3. Sanal makinenin boyutu için **DS1_V2** seçeneğini belirleyin ve **Seç**’e tıklayın.
4. Sanal makine ayarları için şu değerleri silin:
    - Oluşturduğunuz yeni Kullanılabilirlik kümesinin adı için *myAvailabilitySet*.
    -  Sanal ağ olarak *myVNet* öğesinin seçildiğinden emin olun.
    - Alt ağ olarak *myBackendSubnet* öğesinin seçildiğinden emin olun.
    - Genel IP adresi için *myVM1-ip*.
    - Oluşturmanız gereken yeni ağ güvenliği grubunun (güvenlik duvarı) adı için *myNetworkSecurityGroup*.
5. Önyükleme tanılamalarını devre dışı bırakmak için **Devre Dışı** seçeneğine tıklayın.
6. **Tamam**’a tıklayın, özet sayfasındaki ayarları gözden geçirin ve sonra **Oluştur**’a tıklayın.
7. 1-6 arası adımları kullanarak, Kullanılabilirlik kümesi *myAvailabilityset*, sanal ağ *myVnet*, alt ağ *myBackendSubnet* ve Ağ Güvenlik Grubu *myNetworkSecurityGroup* olarak ayarlanmış şekilde *VM2* adlı ikinci bir sanal makine oluşturun. 

### <a name="create-nsg-rules"></a>NSG kuralları oluşturma

Bu bölümde, HTTP ve RDP kullanarak gelen bağlantılara izin vermek için NSG kuralları oluşturacaksınız.

1. Sol menüden **Tüm kaynaklar**’a tıklayın ve kaynak listesinden, **myResourceGroupLB** kaynak grubunda bulunan **myNetworkSecurityGroup** seçeneğine tıklayın.
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
4. **Tamam**’a tıklayın.
 
 ![Sanal ağ oluşturma](./media/load-balancer-get-started-internet-portal/8-load-balancer-nsg-rules.png)
5. 2 - 4 arası adımları yineleyerek, aşağıdaki değerlerle 3389 numaralı bağlantı noktasını kullanarak gelen RDP bağlantısına izin vermek için *myRDPRule* adlı başka bir kural oluşturun:
    - **Kaynak** için *Hizmet Etiketi*.
    - **Kaynak hizmet etiketi** için *İnternet*
    - **Hedef bağlantı noktası aralıkları** için *3389*
    - **Protokol** için *TCP*
    - **Eylem** için *İzin Ver*
    - **Öncelik** için *200*
    - Ad için *myRDPRule*
    - Açıklama için *RDP’ye İzin Ver*

   

### <a name="install-iis"></a>IIS yükleme

1. Sol menüden **Tüm kaynaklar**’a tıklayın ve kaynak listesinden, *myResourceGroupLB* kaynak grubunda bulunan **myVM1** seçeneğine tıklayın.
2. Sanal makineye yönelik RDP için **Genel Bakış** sayfasında **Bağlan**’a tıklatın.
3. *azureuser* kullanıcı adı ve *Azure123456!* parolasıyla sanal makinede oturum açın
4. Sunucu masaüstünde **Windows Yönetimsel Araçları**>**Sunucu Yöneticisi** bölümüne gidin.
5. Sunucu Yöneticisi’nde Yönet’e ve sonra **Rol ve Özellik Ekle**’ye tıklayın.
 ![Sunucu yöneticisi rolü ekleme](./media/load-balancer-get-started-internet-portal/servermanager.png)
6. **Rol ve Özellik Ekleme Sihirbazı** bölümünde aşağıdaki değerleri kullanın:
    - **Yükleme türünü seçin** sayfasında **Rol tabanlı veya özellik tabanlı yükleme** seçeneğine tıklayın.
    - **Hedef sunucuyu seç** sayfasında **myVM1** seçeneğine tıklayın
    - **Sunucu rolü seçin** sayfasında **Web Sunucusu (IIS)** seçeneğine tıklayın
    - Sihirbazın geri kalanını tamamlamak için yönergeleri izleyin 
7. *myVM2* sanal makinesi için 1-6 arası adımları yineleyin.

## <a name="create-basic-load-balancer-resources"></a>Temel Yük Dengeleyici kaynakları oluşturma

Bu bölümde, arka uç adres havuzu ve durum araştırması için yük dengeleyici ayarlarını yapılandıracak ve yük dengeleyici ve NAT kurallarını belirteceksiniz.


### <a name="create-a-backend-address-pool"></a>Arka uç adres havuzu oluşturma

Trafiği VM’lere dağıtmak için, bir arka uç adres havuzunda yük dengeleyiciye bağlı sanal NIC’lerin IP adresleri barındırılır. *VM1* ve *VM2* öğesini eklemek için *myBackendPool* adlı arka uç adres havuzunu oluşturun.

1. Sol menüden **Tüm kaynaklar**’a tıklayın ve sonra kaynak listesinden **myLoadBalancer** seçeneğine tıklayın.
2. **Ayarlar** bölümünde **Arka uç havuzları**’na ve sonra **Ekle**’ye tıklayın.
3. **Arka uç havuzu ekle** sayfasında aşağıdakileri yapın:
    - Ad için, arka uç havuzunuzun adı olarak *myBackEndPool yazın.
    - **İlişkilendirildiği öğe** için, açılır menüden **Kullanılabilirlik kümesi**’ne tıklayın
    - **Kullanılabilirlik kümesi** için **myAvailabilitySet** seçeneğine tıklayın.
    - Arka uç havuzunda oluşturduğunuz her bir sanal makineyi(*myVM1* & *myVM2*) eklemek için **Hedef ağ IP yapılandırması ekle** seçeneğine tıklayın.
    - **Tamam**’a tıklayın.

    ![Arka uç adres havuzuna ekleme - ](./media/load-balancer-get-started-internet-portal/3-load-balancer-backend-02.png)

3. Yük dengeleyici arka uç havuzu ayarınızın hem **VM1** hem de **VM2** sanal makinelerini görüntülediğinden emin olun.

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

Temel Yük Dengeleyicinin uygulamanızın durumunu izlemesine izin vermek için durum araştırması kullanabilirsiniz. Durum araştırması, durum denetimlerine verdikleri yanıtlara göre VM’leri dinamik bir şekilde yük dengeleyiciye dönüşümlü olarak ekler ve kaldırır. Sanal makinelerin durumunu izlemek için *myHealthProbe* durum araştırması oluşturun.

1. Sol menüden **Tüm kaynaklar**’a tıklayın ve sonra kaynak listesinden **myLoadBalancer** seçeneğine tıklayın.
2. **Ayarlar** bölümünde **Durum araştırmaları**’na ve sonra **Ekle**’ye tıklayın.
3. Durum araştırması oluşturmak için şu değerleri kullanın:
    - Durum araştırmasının adı için *myHealthProbe*.
    - Protokol türü için **HTTP**.
    - Bağlantı noktası numarası için *80*.
    - Araştırma denemeleri arasındaki saniye cinsinden **Aralık** için *15*.
    - Bir sanal makinenin sağlıksız olduğu kanısına varılmadan önce gerçekleşmesi gereken ardışık araştırma hatası veya **Sağlıksız eşik** sayısı için *2*.
4. **Tamam**’a tıklayın.

   ![Araştırma ekleme](./media/load-balancer-get-started-internet-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Yük Dengeleyici kuralı oluşturma

Trafiğin sanal makinelere dağıtımını tanımlamak için bir Yük Dengeleyici kuralı kullanılır. Gerekli kaynak ve hedef bağlantı noktalarının yanı sıra gelen trafik için ön uç IP yapılandırması ve trafiği almak için arka uç IP havuzu tanımlamanız gerekir. *LoadBalancerFrontEnd* ön uç havuzunda 80 numaralı bağlantı noktasını dinlemek ve yine 80 numaralı bağlantı noktasını kullanarak *myBackEndPool* arka uç adres havuzuna yük dengelemesi yapılmış ağ trafiğini göndermek için *myLoadBalancerRuleWeb* yük dengeleyici kuralını oluşturun. 

1. Sol menüden **Tüm kaynaklar**’a tıklayın ve sonra kaynak listesinden **myLoadBalancer** seçeneğine tıklayın.
2. **Ayarlar** bölümünde **Yük dengeleme kuralları**’na ve sonra **Ekle**’ye tıklayın.
3. Yük dengeleme kuralını yapılandırmak için şu değerleri kullanın:
    - Yük dengeleme kuralının adı için *myHTTPRule*.
    - Protokol türü için **TCP**.
    - Bağlantı noktası numarası için *80*.
    - Arka uç bağlantı noktası için *80*.
    - Arka uç havuzunun adı için *myBackendPool*.
    - Durum araştırmasının adı için *myHealthProbe*.
4. **Tamam**’a tıklayın.
    
    ![Yük dengeleme kuralı ekleme](./media/load-balancer-get-started-internet-portal/5-load-balancing-rules.png)

## <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme
1. **Genel Bakış** ekranında Yük Dengeleyici için genel IP adresini bulun. **Tüm kaynaklar**’a ve sonra **myPublicIP** seçeneğine tıklayın.

2. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın. IIS Web sunucusunun varsayılan sayfası, tarayıcıda görüntülenir.

  ![IIS Web sunucusu](./media/load-balancer-get-started-internet-portal/9-load-balancer-test.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, yük dengeleyiciyi ve tüm ilgili kaynakları silin. Bunu yapmak için, yük dengeleyiciyi içeren kaynak grubunu seçin ve **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir kaynak grubu, ağ kaynakları ve arka uç sunucuları oluşturdunuz. Daha sonra bir yük dengeleyici oluşturmak için bu kaynakları kullandınız. Yük dengeleyiciler ve bunların ilişkili kaynakları hakkında daha fazla bilgi edinmek için öğretici makaleleriyle devam edin.
