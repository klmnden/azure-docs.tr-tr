---
title: 'Hızlı Başlangıç: Azure portalını kullanarak genel bir Temel yük dengeleyici oluşturma | Microsoft Docs'
description: Bu hızlı başlangıçta, Azure portalını kullanarak genel bir Temel yük dengeleyicinin nasıl oluşturulacağı gösterilmektedir.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to create a Basic Load balancer so that I can load balance internet traffic to VMs.
ms.assetid: aa9d26ca-3d8a-4a99-83b7-c410dd20b9d0
ms.service: load-balancer
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 49fa4cf9b24c432b0956f930a1429e1cdf827f1b
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/18/2018
ms.locfileid: "34304887"
---
# <a name="quickstart-create-a-public-basic-load-balancer-by-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak genel bir Temel yük dengeleyici oluşturma

Yük dengeleme, gelen istekleri birden fazla sanal makineye (VM) yayarak daha yüksek bir kullanılabilirlik ve ölçek düzeyi sağlar. Sanal makinelerin yük dengelemesini yapacak bir yük dengeleyici oluşturmak için Azure portalını kullanabilirsiniz. Bu hızlı başlangıçta, ağ kaynaklarının, arka uç sunucularının ve bir yük dengeleyicinin Temel fiyatlandırma katmanında nasıl oluşturulacağı gösterilmektedir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Bu hızlı başlangıçtaki tüm görevler için [Azure portalında](http://portal.azure.com) oturum açın.

## <a name="create-a-basic-load-balancer"></a>Temel yük dengeleyici oluşturma

Bu bölümde, portalı kullanarak genel bir Temel yük dengeleyici oluşturacaksınız. Genel IP adresi, portalı kullanarak genel IP’yi ve yük dengeleyici kaynağını oluştururken yük dengeleyicinin ön ucu olarak otomatik şekilde yapılandırılır. Ön uç adı **LoadBalancerFrontend** şeklindedir.

1. Portalın sol üst kısmında **Kaynak oluştur** > **Ağ** > **Yük Dengeleyici** seçeneğini belirleyin.
2. **Yük dengeleyici oluştur** bölmesine şu değerleri girin:
   - Yük dengeleyicinin adı için **myLoadBalancer**
   - Yük dengeleyicinin türü için **Public** 
   - **SKU** **Temel** ve **Atama** **Dinamik** olarak ayarlanmış şekilde oluşturmanız gereken genel IP için **myPublicIP**
   - Yeni kaynak grubunun adı için **myResourceGroupLB**
3. **Oluştur**’u seçin.
   
![Yük dengeleyici oluşturma](./media/load-balancer-get-started-internet-portal/1-load-balancer.png)


## <a name="create-back-end-servers"></a>Arka uç sunucuları oluşturma

Bu bölümde bir sanal ağ oluşturacak ve Temel yük dengeleyicinizin arka uç havuzu için iki sanal makine oluşturacaksınız. Daha sonra yük dengeleyicinin test edilmesine yardımcı olması için sanal makinelere Internet Information Services (IIS) yükleyeceksiniz.

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma
1. Portalın sol üst kısmında **Yeni** > **Ağ** > **Sanal ağ** seçeneğini belirleyin.
2. **Sanal ağ oluştur** bölmesine şu değerleri girin ve **Oluştur**’u seçin:
   - Sanal ağ adı için **myVnet**
   - Mevcut kaynak grubunun adı için **myResourceGroupLB**
   - Alt ağ adı için**myBackendSubnet**

   ![Sanal ağ oluşturma](./media/load-balancer-get-started-internet-portal/2-load-balancer-virtual-network.png)

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

1. Portalın sol üst kısmında **Yeni** > **İşlem** > **Windows Server 2016 Datacenter** seçeneğini belirleyin. 
2. Sanal makine için şu değerleri girin ve **Tamam**’ı seçin:
   - Sanal makinenin adı için **myVM1**.        
   - Yönetici kullanıcı adı için **azureuser**.    
   - Kaynak grubu için **myResourceGroupLB**. (**Kaynak grubu** bölümünde, **Var olanı kullan**’ı seçin ve sonra **myResourceGroupLB** seçeneğini belirleyin.)   
3. Sanal makinenin boyutu için **DS1_V2** seçeneğini belirleyin ve **Seç**’e tıklayın.
4. Sanal makine ayarları için şu değerleri girin:
   - Oluşturduğunuz yeni kullanılabilirlik kümesinin adı için **myAvailabilitySet**.
   - Sanal ağ adı için **myVNet**. (Seçili olduğundan emin olun.)
   - Alt ağ adı için **myBackendSubnet**. (Seçili olduğundan emin olun.)
   - Genel IP adresi için **myVM1-ip**.
   - Oluşturmanız gereken yeni ağ güvenlik grubunun (bir güvenlik duvarı türü NSG) adı için **myNetworkSecurityGroup**.
5. Önyükleme tanılamalarını devre dışı bırakmak için **Devre Dışı** seçeneğini belirleyin.
6. **Tamam**’a tıklayın, özet sayfasındaki ayarları gözden geçirin ve sonra **Oluştur**’u seçin.
7. 1 ila 6 adımlarını kullanarak, aşağıdaki ayarlarla **VM2** adlı ikinci bir sanal makine oluşturun:
   - Kullanılabilirlik kümesi olarak **myAvailabilityset**.
   - Sanal ağ olarak **myVnet**.
   - Alt ağ olarak **myBackendSubnet**.
   - Ağ güvenlik grubu olarak **myNetworkSecurityGroup**. 

### <a name="create-nsg-rules"></a>NSG kuralları oluşturma

Bu bölümde, HTTP ve RDP kullanan gelen bağlantılara izin vermek için NSG kuralları oluşturacaksınız.

1. Soldaki menüden **Tüm kaynaklar**’ı seçin. Kaynak listesinden, **myResourceGroupLB** kaynak grubunda **myNetworkSecurityGroup** seçeneğini belirleyin.
2. **Ayarlar** bölümünde **Gelen güvenlik kuralları**’nı ve sonra **Ekle**’yi seçin.
3. 80 numaralı bağlantı noktasını kullanan gelen HTTP bağlantılarına izin vermek için **myHTTPRule** adlı gelen güvenlik kuralı için şu değerleri girin: Sonra **Tamam**’ı seçin.
   - **Kaynak** için **Hizmet Etiketi**
   - **Kaynak hizmet etiketi** için **İnternet**
   - **Hedef bağlantı noktası aralıkları** için **80**
   - **Protokol** için **TCP**
   - **Eylem** için **İzin Ver**
   - **Öncelik** için **100**
   - **Ad** için **myHTTPRule**
   - **Açıklama** için **HTTP’ye İzin Ver**
 
   ![NSG kuralı oluşturma](./media/load-balancer-get-started-internet-portal/8-load-balancer-nsg-rules.png)
4. 2. ve 3. adımları yineleyerek, 3389 numaralı bağlantı noktası üzerinden gelen RDP bağlantısına izin vermek için **myRDPRule** adlı başka bir kural oluşturun. Aşağıdaki değerleri kullanın:
   - **Kaynak** için **Hizmet Etiketi**
   - **Kaynak hizmet etiketi** için **İnternet**
   - **Hedef bağlantı noktası aralıkları** için **3389**
   - **Protokol** için **TCP**
   - **Eylem** için **İzin Ver**
   - **Öncelik** için **200**
   - **Ad** için **myRDPRule**
   - **Açıklama** için **RDP’ye İzin Ver**

   

### <a name="install-iis"></a>IIS yükleme

1. Soldaki menüden **Tüm kaynaklar**’ı seçin. Kaynak listesinden, **myResourceGroupLB** kaynak grubunda **myVM1** seçeneğini belirleyin.
2. Sanal makineye yönelik RDP için **Genel Bakış** sayfasında **Bağlan**’ı seçin.
3. **azureuser** kullanıcı adı ve **Azure123456!** parolasıyla sanal makinede oturum açın.
4. Sunucu masaüstünde **Windows Yönetimsel Araçları** > **Sunucu Yöneticisi** bölümüne gidin.
5. Sunucu Yöneticisi’nde **Yönet**’i ve sonra **Rol ve Özellik Ekle**’yi seçin.
   ![Sunucu yöneticisi rolü ekleme](./media/load-balancer-get-started-internet-portal/servermanager.png)
6. Rol ve Özellik Ekleme Sihirbazı’nda aşağıdaki değerleri kullanın:
   - **Yükleme türünü seçin** sayfasında **Rol tabanlı veya özellik tabanlı yükleme**’yi seçin.
   - **Hedef sunucuyu seç** sayfasında **myVM1** seçeneğini belirleyin.
   - **Sunucu rolü seçin** sayfasında **Web Sunucusu (IIS)** seçeneğini belirleyin.
   - Sihirbazın geri kalanını tamamlamak için yönergeleri izleyin. 
7. **myVM2** sanal makinesi için 1-6 arası adımları yineleyin.

## <a name="create-resources-for-the-basic-load-balancer"></a>Temel yük dengeleyici için kaynaklar oluşturma

Bu bölümde, arka uç adres havuzu ve durum araştırması için yük dengeleyici ayarlarını yapılandıracaksınız. Ayrıca yük dengeleyici ve NAT kurallarını da belirteceksiniz.


### <a name="create-a-back-end-address-pool"></a>Arka uç adres havuzu oluşturma

Trafiği sanal makinelere dağıtmak için bir arka uç adres havuzu, yük dengeleyiciye bağlı sanal NIC’lerin IP adreslerini içerir. **VM1** ve **VM2** sanal makinesini eklemek için **myBackendPool** adlı arka uç adres havuzunu oluşturun.

1. Sol taraftaki menüden **Tüm kaynaklar**’ı seçin ve sonra kaynak listesinden **myLoadBalancer** seçeneğini belirleyin.
2. **Ayarlar** bölümünde **Arka uç havuzları**’nı ve sonra **Ekle**’yi seçin.
3. **Arka uç havuzu ekle** sayfasında aşağıdakileri yapın ve **Tamam**’ı seçin:
   - **Ad** için **myBackEndPool** girin.
   - **İlişkilendirildiği öğe** için, açılır menüden **Kullanılabilirlik kümesi**’ni seçin.
   - **Kullanılabilirlik kümesi** için **myAvailabilitySet** seçeneğini belirleyin.
   - Oluşturduğunuz her sanal makineyi (**myVM1** ve **myVM2**) arka uç havuzuna eklemek için **Hedef ağ IP yapılandırması ekle** seçeneğini belirleyin.

   ![Arka uç adres havuzuna ekleme](./media/load-balancer-get-started-internet-portal/3-load-balancer-backend-02.png)

3. Yük dengeleyicinizin arka uç havuzu ayarının hem **VM1** hem de **VM2** sanal makinelerini görüntülediğinden emin olun.

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

Temel yük dengeleyicinin uygulamanızın durumunu izlemesine izin vermek için durum araştırması kullanabilirsiniz. Durum yoklaması, durum denetimlerine verdikleri yanıtlara göre VM’leri dinamik olarak yük dengeleyici rotasyonuna ekler ve kaldırır. Sanal makinelerin durumunu izlemek için **myHealthProbe** adlı bir durum araştırması oluşturun.

1. Sol taraftaki menüden **Tüm kaynaklar**’ı seçin ve sonra kaynak listesinden **myLoadBalancer** seçeneğini belirleyin.
2. **Ayarlar** bölümünde **Durum araştırmaları**’nı ve sonra **Ekle**’yi seçin.
3. Şu değerleri kullanın ve sonra **Tamam**’ı seçin:
   - Durum araştırmasının adı için **myHealthProbe**
   - Protokol türü için **HTTP**
   - Bağlantı noktası numarası için **80**
   - Araştırma denemeleri arasındaki saniye cinsinden süreyi ifade eden **Aralık** için **15**
   - Bir sanal makinenin sağlıksız olduğu kanısına varılmadan önce gerçekleşmesi gereken ardışık araştırma hatası sayısını ifade eden **Sağlıksız eşik** sayısı için **2**

   ![Araştırma ekleme](./media/load-balancer-get-started-internet-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Trafiğin sanal makinelere nasıl dağıtılacağını tanımlamak için bir yük dengeleyici kuralı kullanırsınız. Gerekli kaynak ve hedef bağlantı noktalarının yanı sıra gelen trafik için ön uç IP yapılandırması ve trafiği almak için arka uç IP havuzu tanımlamanız gerekir. 

**LoadBalancerFrontEnd** ön ucunda 80 numaralı bağlantı noktasını dinlemek için **myLoadBalancerRuleWeb** adlı bir yük dengeleyici kuralı oluşturun. Kural, 80 numaralı bağlantı noktasını kullanarak **myBackEndPool** arka uç adres havuzuna yük dengeli ağ trafiği göndermek için de geçerlidir. 

1. Sol taraftaki menüden **Tüm kaynaklar**’ı seçin ve sonra kaynak listesinden **myLoadBalancer** seçeneğini belirleyin.
2. **Ayarlar** bölümünde **Yük dengeleme kuralları**’nı ve sonra **Ekle**’yi seçin.
3. Şu değerleri kullanın ve sonra **Tamam**’ı seçin:
   - Yük dengeleyici kuralının adı için**myHTTPRule**
   - Protokol türü için **TCP**
   - Bağlantı noktası numarası için **80**
   - Arka uç bağlantı noktası için **80**
   - Arka uç havuzunun adı için **myBackendPool**
   - Durum araştırmasının adı için **myHealthProbe**
    
   ![Yük dengeleyici kuralı ekleme](./media/load-balancer-get-started-internet-portal/5-load-balancing-rules.png)

## <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme
1. **Genel Bakış** ekranında yük dengeleyici için genel IP adresini bulun. **Tüm kaynaklar**’ı seçin ve sonra **myPublicIP** seçeneğini belirleyin.

2. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın. IIS web sunucusunun varsayılan sayfası, tarayıcıda görüntülenir.

   ![IIS web sunucusu](./media/load-balancer-get-started-internet-portal/9-load-balancer-test.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, yük dengeleyiciyi ve tüm ilgili kaynakları silebilirsiniz. Yük dengeleyiciyi içeren kaynak grubunu seçin ve **Sil** seçeneğini belirleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir kaynak grubu, ağ kaynakları ve arka uç sunucuları oluşturdunuz. Daha sonra bir Temel bir Load Balancer oluşturmak için bu kaynakları kullandınız. Azure Load Balancer hakkında daha fazla bilgi almak için Azure Load Balancer öğreticisine devam edin.

> [!div class="nextstepaction"]
> [Azure Load Balancer öğreticileri](tutorial-load-balancer-basic-internal-portal.md)
