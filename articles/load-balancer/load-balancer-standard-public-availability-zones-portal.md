---
title: Yük Dengeleyici sanal makineleri kullanılabilirlik dilimlerinde - Azure portalı | Microsoft Docs
description: Standart bir yük dengeleyici sanal makineleri Azure portalını kullanarak kullanılabilirlik bölgeler arasında dengelemek için bölge olarak yedekli ön uç ile oluşturma
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: ''
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/18
ms.author: kumud
ms.openlocfilehash: ad476922342844a908961960407eb344711932f5
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="load-balance-vms-across-availability-zones-with-a-standard-load-balancer-using-the-azure-portal"></a>Azure portalını kullanarak standart bir yük dengeleyici ile kullanılabilirlik bölgeler arasında Yük Dengeleme VM'ler

Bir genel yük dengeleyiciye standart elde etmek için bir bölge olarak yedekli ön uç ile oluşturma aracılığıyla bu makale adımları bölge artıklık birden çok DNS kaydı bağımlılığını olmadan elde edin. Standart bir yük dengeleyici içinde tek bir ön uç IP adresi otomatik olarak bölge olarak yedekli ' dir. Tek bir IP adresi ile yük dengeleyici için bir bölge olarak yedekli ön kullanarak tüm kullanılabilirlik bölgeler arasında olan bir bölge içindeki bir sanal ağdaki tüm VM artık ulaşabilir. Uygulamalarınızı beklenmeyen hatalardan veya tüm veri merkezinin kaybedilmesinden korumak için kullanılabilirlik alanlarından yararlanın. Bölge-artıklık ile bir veya daha fazla kullanılabilirlik bölgeleri başarısız olabilir ve veri yolu bölge kalır sağlıklı bir bölgede sürece devam eder. 

Kullanılabilirlik bölgeleri standart yük dengeleyici ile kullanma hakkında daha fazla bilgi için bkz: [standart yük dengeleyici ve kullanılabilirlik bölgeleri](load-balancer-standard-availability-zones.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[http://portal.azure.com](http://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-public-standard-load-balancer"></a>Ortak bir standart yük dengeleyici oluşturma

Standart Yük Dengeleyici yalnızca Standart Genel IP adresini destekler. Yeni bir ortak IP yük dengeleyici oluşturulurken oluşturduğunuzda, standart SKU sürüm otomatik olarak yapılandırılır ve ayrıca otomatik olarak bölge olarak yedekli.

1. Ekranın sol üst kısmında **Kaynak oluştur** > **Ağ** > **Yük Dengeleyici**'ye tıklayın.
2. **Yük dengeleyici oluştur** sayfasına, yük dengeleyici için şu değerleri girin:
    - Yük dengeleyicinin adı için *myLoadBalancer*.
    - Yük dengeleyicinin türü için **Public**.
     - *myPublicIP* - oluşturduğunuz yeni ortak IP adresi. Bunu yapmak için tıklatın **genel bir IP adresi seçin**ve ardından **Yeni Oluştur**. Ad türü için *myPublicIP*, SKU özelliği standart varsayılan ve select tarafından **bölge olarak yedekli** için **kullanılabilirlik bölge**.
    - *myResourceGroupLBAZ* - oluşturduğunuz yeni kaynak grubunun adı.
    - Konum için **westeurope**.
3. Yük dengeleyiciyi oluşturmak için **Oluştur**’a tıklayın.
   
    ![Yük dengeleyici oluşturma](./media/load-balancer-standard-public-availability-zones-portal/1a-load-balancer.png)


## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu bölümde, bir sanal ağ oluşturma, sanal makineler eklemek bölge için farklı bölgelerde (bölge 1, 2 bölge ve bölge 3) oluşturma, yük dengeleyici ve IIS yükleyin bölge olarak yedekli yük mi test yardımcı olmak için sanal makinelerde arka uç havuzu ncer. Bu nedenle, bir bölge başarısız olursa, sistem durumu araştırması aynı bölgedeki VM için başarısız olur ve trafik diğer bölgelerdeki VM'ler tarafından sunulan devam eder.

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma
1. Ekranın üst sol taraftaki tıklatın **kaynak oluşturma** > **ağ** > **sanal ağ** ve bu değerleri girin sanal ağ:
    - Sanal ağ adı için *myVnet*.
    - *myResourceGroupLBAZ* - var olan kaynak grubunun adı
    - Alt ağ adı için *myBackendSubnet*.
2. Sanal ağı oluşturmak için **Oluştur**’a tıklayın.

    ![Sanal ağ oluşturma](./media/load-balancer-standard-public-availability-zones-portal/2-load-balancer-virtual-network.png)

## <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

1. Ekranın sol üst tarafında, tıklatın **kaynak oluşturma**, arama kutusuna *ağ güvenlik grubu*ve ağ güvenlik grubu sayfasını tıklatın **oluşturma**.
2. Create ağ güvenlik grubu sayfası bu değerleri girin:
    - *myNetworkSecurityGroup* - ağ güvenlik grubunun adı.
    - *myResourceGroupLBAZ* - var olan kaynak grubunun adı.
   
![Sanal ağ oluşturma](./media/load-balancer-standard-public-availability-zones-portal/create-nsg.png)

### <a name="create-nsg-rules"></a>NSG kuralları oluşturma

Bu bölümde, HTTP ve Azure portalını kullanarak RDP kullanarak gelen bağlantılara izin vermek için NSG kuralları oluşturun.

1. Azure portalında tıklatın **tüm kaynakları** sol taraftaki menüyü arama ve tıklatın **myNetworkSecurityGroup** içinde bulunan **myResourceGroupLBAZ** kaynak grubu.
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
 
 ![Sanal ağ oluşturma](./media/load-balancer-standard-public-availability-zones-portal/8-load-balancer-nsg-rules.png)
5. 2 - 4 arası adımları yineleyerek, aşağıdaki değerlerle 3389 numaralı bağlantı noktasını kullanarak gelen RDP bağlantısına izin vermek için *myRDPRule* adlı başka bir kural oluşturun:
    - **Kaynak** için *Hizmet Etiketi*.
    - **Kaynak hizmet etiketi** için *İnternet*
    - **Hedef bağlantı noktası aralıkları** için *3389*
    - **Protokol** için *TCP*
    - **Eylem** için *İzin Ver*
    - **Öncelik** için *200*
    - Ad için *myRDPRule*
    - Açıklama için *RDP’ye İzin Ver*


### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

1. Ekranın sol üst tarafında tıklatın **kaynak oluşturma** > **işlem** > **Windows Server 2016 Datacenter** ve bunlar girin sanal makine için değerler:
    - Sanal makinenin adı için *myVM1*.        
    - Yönetici kullanıcı adı için *azureuser*.    
    - *myResourceGroupLBAZ* - **kaynak grubu**seçin **var olanı kullan**ve ardından *myResourceGroupLBAZ*.
2. **Tamam**’a tıklayın.
3. Sanal makinenin boyutu için **DS1_V2** seçeneğini belirleyin ve **Seç**’e tıklayın.
4. Sanal makine ayarları için şu değerleri girin:
    - *Bölge 1* - VM nereye bölgesi için.
    -  Sanal makine olarak *myVNet*'in seçildiğinden emin olun.
    - Alt ağ olarak *myBackendSubnet*'in seçildiğinden emin olun.
    - *myNetworkSecurityGroup* - ağ güvenlik grubu (Güvenlik Duvarı) adı.
5. Önyükleme tanılamalarını devre dışı bırakmak için **Devre Dışı** seçeneğine tıklayın.
6. **Tamam**’a tıklayın, özet sayfasındaki ayarları gözden geçirin ve sonra **Oluştur**’a tıklayın.
  
 ![Sanal makine oluşturma](./media/load-balancer-standard-public-availability-zones-portal/create-vm-standard-ip.png)

7. Adlı ikinci bir VM oluşturma *VM2* bölge 2 ve üçüncü VM bölge 3'te ve ile *myVnet* sanal ağ *myBackendSubnet* alt ağ olarak ve * *myNetworkSecurityGroup* ağ güvenlik grubunu kullanarak 1-6 adımlarını gibi.

### <a name="install-iis-on-vms"></a>Sanal makineler IIS yükleme

1. Tıklatın **tüm kaynakları** sol menüde ve kaynakları listesinden tıklatın **myVM1** içinde bulunan *myResourceGroupLBAZ* kaynak grubu.
2. Sanal makineye yönelik RDP için **Genel Bakış** sayfasında **Bağlan**’a tıklayın.
3. *azureuser* kullanıcı adıyla sanal makinede oturum açın.
4. Sunucu masaüstünde **Windows Yönetimsel Araçları**>**Sunucu Yöneticisi** bölümüne gidin.
5. Sunucu Yöneticisi'ni hızlı başlangıç sayfasında tıklatın **rol ve Özellik Ekle**.

   ![Arka uç adres havuzuna ekleme - ](./media/load-balancer-standard-public-availability-zones-portal/servermanager.png)    

1. **Rol ve Özellik Ekleme Sihirbazı** bölümünde aşağıdaki değerleri kullanın:
    - **Yükleme türünü seçin** sayfasında **Rol tabanlı veya özellik tabanlı yükleme** seçeneğine tıklayın.
    - İçinde **Select hedef sunucu** sayfasında, **myVM1**.
    - İçinde **sunucu rolü** sayfasında, **Web sunucusu (IIS)**.
    - Sihirbazın geri kalanını tamamlamak için yönergeleri izleyin.
2. Sanal makineyle - RDP oturumu kapatmanız *myVM1*.
3. Sanal makineler IIS yüklemek için 1 için 7 arasındaki adımları yineleyin *myVM2* ve *myVM3*.

## <a name="create-load-balancer-resources"></a>Yük dengeleyici kaynakları oluşturma

Bu bölümde, arka uç adres havuzu ve durum araştırması için yük dengeleyici ayarlarını yapılandıracak ve yük dengeleyici ve NAT kurallarını belirteceksiniz.


### <a name="create-a-backend-address-pool"></a>Arka uç adres havuzu oluşturma

Trafiği VM’lere dağıtmak için, bir arka uç adres havuzunda yük dengeleyiciye bağlı sanal NIC’lerin IP adresleri barındırılır. *VM1* ve *VM2* öğesini eklemek için *myBackendPool* adlı arka uç adres havuzunu oluşturun.

1. Sol menüden **Tüm kaynaklar**’a tıklayın ve sonra kaynak listesinden **myLoadBalancer** seçeneğine tıklayın.
2. **Ayarlar** bölümünde **Arka uç havuzları**’na ve sonra **Ekle**’ye tıklayın.
3. **Arka uç havuzu ekle** sayfasında aşağıdakileri yapın:
    - Ad için, arka uç havuzunuzun adı olarak *myBackEndPool yazın.
    - İçin **sanal ağ**, aşağı açılan menüye tıklayın **myVNet**
    - İçin **sanal makine**, aşağı açılan menüsünde tıklatın, **myVM1**.
    - İçin **IP adresi**, myVM1 IP adresini açılan menüye tıklayın.
4. Tıklatın **yeni arka uç kaynak ekleyin** her bir sanal makine eklemek için (*myVM2* ve *myVM3*) yük dengeleyici arka uç havuzuna eklemek için.
5. **Ekle**'ye tıklayın.

    ![Arka uç adres havuzuna ekleme - ](./media/load-balancer-standard-public-availability-zones-portal/add-backend-pool.png)

3. Yük Dengeleyici arka uç havuzu ayarınız tüm üç sanal makineleri - görüntülediğinden emin olmak için kontrol edin **myVM1**, **myVM2** ve **myVM3**.

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

Yük dengeleyicinin uygulamanızın durumunu izlemesine izin vermek için durum araştırması kullanabilirsiniz. Durum yoklaması, durum denetimlerine verdikleri yanıtlara göre VM’leri dinamik olarak yük dengeleyici rotasyonuna ekler ve kaldırır. Sanal makinelerin durumunu izlemek için *myHealthProbe* durum araştırması oluşturun.

1. Sol taraftaki menüden **Tüm kaynaklar**’a tıklayın ve sonra kaynak listesinden **myLoadBalancer** seçeneğine tıklayın.
2. **Ayarlar** bölümünde **Durum araştırmaları**’na ve sonra **Ekle**’ye tıklayın.
3. Durum araştırması oluşturmak için şu değerleri kullanın:
    - Durum araştırmasının adı için *myHealthProbe*.
    - Protokol türü için **HTTP**.
    - Bağlantı noktası numarası için *80*.
    - Araştırma denemeleri arasındaki saniye cinsinden **Aralık** için *15*.
    - Bir sanal makinenin sağlıksız olduğu kanısına varılmadan önce gerçekleşmesi gereken ardışık araştırma hatası veya **Sağlıksız eşik** sayısı için *2*.
4. **Tamam**’a tıklayın.

   ![Araştırma ekleme](./media/load-balancer-standard-public-availability-zones-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Trafiğin VM’lere dağıtımını tanımlamak için bir yük dengeleyici kuralı kullanılır. Gerekli kaynak ve hedef bağlantı noktalarının yanı sıra gelen trafik için ön uç IP yapılandırması ve trafiği almak için arka uç IP havuzu tanımlamanız gerekir. *FrontendLoadBalancer* ön uç havuzunda 80 numaralı bağlantı noktasını dinlemek ve yine 80 numaralı bağlantı noktasını kullanarak *myBackEndPool* arka uç adres havuzuna yük dengelemesi yapılmış ağ trafiğini göndermek için *myLoadBalancerRuleWeb* yük dengeleyici kuralını oluşturun. 

1. Sol taraftaki menüden **Tüm kaynaklar**’a tıklayın ve sonra kaynak listesinden **myLoadBalancer** seçeneğine tıklayın.
2. **Ayarlar** bölümünde **Yük dengeleme kuralları**’na ve sonra **Ekle**’ye tıklayın.
3. Yük dengeleme kuralını yapılandırmak için şu değerleri kullanın:
    - Yük dengeleme kuralının adı için *myHTTPRule*.
    - Protokol türü için **TCP**.
    - Bağlantı noktası numarası için *80*.
    - Arka uç bağlantı noktası için *80*.
    - Arka uç havuzunun adı için *myBackendPool*.
    - Durum araştırmasının adı için *myHealthProbe*.
4. **Tamam**’a tıklayın.
    
    ![Yük dengeleme kuralı ekleme](./media/load-balancer-standard-public-availability-zones-portal/load-balancing-rule.png)

## <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme
1. **Genel Bakış** ekranında Yük Dengeleyici için genel IP adresini bulun. **Tüm kaynaklar**’a ve sonra **myPublicIP** seçeneğine tıklayın.

2. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın. IIS Web sunucusunun varsayılan sayfası, tarayıcıda görüntülenir.

      ![IIS Web sunucusu](./media/load-balancer-standard-public-availability-zones-portal/9-load-balancer-test.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, yük dengeleyiciyi ve tüm ilgili kaynakları silin. Bunu yapmak için, yük dengeleyiciyi içeren kaynak grubunu seçin ve **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

[Standart Yük Dengeleyici](load-balancer-standard-overview.md) hakkında daha fazla bilgi edinin.
