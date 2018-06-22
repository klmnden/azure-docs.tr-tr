---
title: 'Öğretici: Load Balancer’da bağlantı noktası iletmeyi yapılandırma - Azure portalı | Microsoft Docs'
description: Bu öğreticide bir Azure sanal ağındaki VM’lere bağlantı oluşturmak için Azure Load Balancer kullanarak bağlantı noktası iletmeyi yapılandırma gösterilmektedir.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: As an IT administrator, I want to configure port forwarding in Azure Load Balancer to remotely connect to VMs in an Azure virtual network.
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/31/18
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: eac0636af1b46912897a4c93f86477c46299bc0f
ms.sourcegitcommit: 4f9fa86166b50e86cf089f31d85e16155b60559f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34757763"
---
# <a name="tutorial-configure-port-fowarding-in-load-balancer-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak Load Balancer’da bağlantı noktası iletmeyi yapılandırma

Azure Load Balancer kullanarak bağlantı noktası iletme, bağlantı noktası numarası üzerinden Load Balancer’ın ortak adresini kullanarak Azure sanal ağındaki VM’lere uzaktan bağlanmanızı sağlar. Bu öğreticide, Azure Load Balancer’da bağlantı noktası iletmeyi yapılandırmayı ve aşağıdakileri yapmayı öğreneceksiniz:


> [!div class="checklist"]
> * Azure yük dengeleyici oluşturma
> * Yük dengeleyici durum yoklaması oluşturma
> * Yük dengeleyici trafik kuralları oluşturma
> * Sanal makineler oluşturma ve IIS sunucusunu yükleme
> * Sanal makineleri yük dengeleyiciye ekleme
> * Yük dengeleyici gelen NAT kurallarını oluşturma
> * Bağlantı noktası iletmeyi çalışırken görün


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[http://portal.azure.com](http://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-standard-load-balancer"></a>Standart Yük Dengeleyici oluşturma

Bu bölümde, sanal makinelerde yük dengelemesine yardımcı olan bir genel yük dengeleyici oluşturursunuz. Standart Yük Dengeleyici yalnızca Standart Genel IP adresini destekler. Standard Load Balancer oluşturduğunuzda, Standard Load Balancer için ön uç (varsayılan olarak *LoadBalancerFrontend* adını alır) olarak yapılandırılmış yeni bir Standart Genel IP adresi de oluşturmanız gerekir. 

1. Ekranın sol üst kısmında **Kaynak oluştur** > **Ağ** > **Yük Dengeleyici**'ye tıklayın.
2. **Yük dengeleyici oluştur** sayfasına, yük dengeleyici için şu değerleri girin:
    - Yük dengeleyicinin adı için *myLoadBalancer*.
    - Yük dengeleyicinin SKU sürümü için **Standard**.
    - Yük dengeleyicinin türü için **Public**.
    - Oluşturduğunuz **Yeni** Genel IP için *myPublicIP*.
    - Oluşturmak için seçtiğiniz **Yeni** kaynak grubunun adı için *myResourceGroupSLB*.
    - Konum için **westeurope**.
3. Yük dengeleyiciyi oluşturmak için **Oluştur**’a tıklayın.

![Yük dengeleyici oluşturma](./media/load-balancer-standard-public-portal/1a-load-balancer.png)
   
## <a name="create-load-balancer-resources"></a>Yük dengeleyici kaynakları oluşturma

Bu bölümde, arka uç adres havuzu ve durum araştırması için yük dengeleyici ayarlarını yapılandıracak ve yük dengeleyici kurallarını belirteceksiniz.

### <a name="create-a-backend-address-pool"></a>Arka uç adres havuzu oluşturma

Trafiği VM’lere dağıtmak için, bir arka uç adres havuzunda yük dengeleyiciye bağlı sanal NIC’lerin IP adresleri barındırılır. *VM1* ve *VM2*'yi eklemek için *myBackendPool* adlı arka uç adres havuzunu oluşturun.

1. Sol taraftaki menüden **Tüm kaynaklar**’a tıklayın ve sonra kaynak listesinden **myLoadBalancer** seçeneğine tıklayın.
2. **Ayarlar** bölümünde **Arka uç havuzları**’na ve sonra **Ekle**’ye tıklayın.
3. **Arka uç havuzu ekle** sayfasında, arka uç havuzunuzun adı olarak *myBackEndPool* yazın ve ardından **Tamam**'a tıklayın.

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

   ![Araştırma ekleme](./media/load-balancer-standard-public-portal/4-load-balancer-probes.png)

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
    
## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu bölümde, bir sanal ağ oluşturur, yük dengeleyicinizin arka uç havuzu için iki sanal makine oluşturur ve sonra yük dengeleyiciyi kullanarak bağlantı noktası iletmeyi test etmek için sanal makinelere IIS yüklersiniz.

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma
1. Ekranın sol üst kısmında **Yeni** > **Ağ** > **Sanal ağ**'e tıklayın ve sanal ağ için şu değerleri girin:
    - Sanal ağ adı için *myVnet*.
    - Mevcut kaynak grubunun adı için *myResourceGroupSLB*
    - Alt ağ adı için *myBackendSubnet*.
2. Sanal ağı oluşturmak için **Oluştur**’a tıklayın.

    ![Sanal ağ oluşturma](./media/load-balancer-standard-public-portal/2-load-balancer-virtual-network.png)

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

1. Ekranın sol üst kısmında **Yeni** > **İşlem** > **Windows Server 2016 Datacenter**'a tıklayın ve sanal makine için şu değerleri girin:
    - Sanal makinenin adı için *myVM1*.        
    - Yönetici kullanıcı adı için *azureuser*.    
    - *myResourceGroupSLB* - **Kaynak grubu** için **Var olanı kullan**’ı seçin ve sonra *myResourceGroupSLB* öğesini seçin.
2. **Tamam**’a tıklayın.
3. Sanal makinenin boyutu için **DS1_V2** seçeneğini belirleyin ve **Seç**’e tıklayın.
4. Sanal makine ayarları için şu değerleri girin:
    -  Sanal makine olarak *myVNet*'in seçildiğinden emin olun.
    - Alt ağ olarak *myBackendSubnet*'in seçildiğinden emin olun.
    - Oluşturmanız gereken yeni ağ güvenlik grubunun (güvenlik duvarı) adı için *myNetworkSecurityGroup*.
5. Önyükleme tanılamalarını devre dışı bırakmak için **Devre Dışı** seçeneğine tıklayın.
6. **Tamam**’a tıklayın, özet sayfasındaki ayarları gözden geçirin ve sonra **Oluştur**’a tıklayın.
7. 1-6 arası adımları kullanarak, sanal ağı *myVnet*, alt ağı *myBackendSubnet* ve ağ güvenlik grubu **myNetworkSecurityGroup* olarak ayarlanmış şekilde *VM2* adlı bir sanal makine daha oluşturun. 

### <a name="create-nsg-rules"></a>NSG kuralları oluşturma

Bu bölümde, HTTP ve RDP kullanarak gelen bağlantılara izin vermek için NSG kuralları oluşturacaksınız.

1. Sol menüden **Tüm kaynaklar**’a tıklayın ve kaynak listesinden, **myResourceGroupSLB** kaynak grubunda bulunan **myNetworkSecurityGroup** öğesine tıklayın.
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
 
 ![Sanal ağ oluşturma](./media/load-balancer-standard-public-portal/8-load-balancer-nsg-rules.png)
5. 2 - 4 arası adımları yineleyerek, aşağıdaki değerlerle 3389 numaralı bağlantı noktasını kullanarak gelen RDP bağlantısına izin vermek için *myRDPRule* adlı başka bir kural oluşturun:
    - **Kaynak** için *Hizmet Etiketi*.
    - **Kaynak hizmet etiketi** için *İnternet*
    - **Hedef bağlantı noktası aralıkları** için *3389*
    - **Protokol** için *TCP*
    - **Eylem** için *İzin Ver*
    - **Öncelik** için *200*
    - Ad için *myRDPRule*
    - Açıklama için *RDP’ye İzin Ver*

### <a name="install-iis-on-vms"></a>VM’lere IIS yükleme

1. Sol menüden **Tüm kaynaklar**’a tıklayın ve kaynak listesinden, *myResourceGroupSLB* kaynak grubunda bulunan **myVM1** öğesine tıklayın.
2. Sanal makineye yönelik RDP için **Genel Bakış** sayfasında **Bağlan**’a tıklayın.
3. *azureuser* kullanıcı adıyla sanal makinede oturum açın.
4. Sunucu masaüstünde **Windows Yönetimsel Araçları**>**Windows PowerShell** bölümüne gidin.
5. PowerShell Penceresinde aşağıdaki komutları çalıştırarak IIS sunucusunu yükleyin, varsayılan iisstart.htm dosyasını kaldırın ve ardından VM’nin adını gösteren yeni bir iisstart.htm dosyasını ekleyin:

   ```azurepowershell-interactive
    
    # install IIS server role
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    # Add a new htm file that displays server name
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from" + $env:computername)
   ```
6. *myVM1* ile RDP oturumunu kapatın.
7. IIS’yi ve *myVM2*’deki güncelleştirilmiş iisstart.htm dosyasını yüklemek için 1 ile 6 arasındaki adımları tekrarlayın.

## <a name="add-vms-to-the-backend-address-pool"></a>Arka uç adres havuzuna VM ekleme

VM'lere trafik dağıtımı yapmak için, *VM1* ve *VM2* sanal makinelerini önceden oluşturulmuş olan *myBackendPool* arka uç adres havuzuna ekleyin. Arka uç havuzunda yük dengeleyiciye bağlı sanal NIC’lerin IP adresleri barındırılır.

1. Sol taraftaki menüden **Tüm kaynaklar**’a tıklayın ve sonra kaynak listesinden **myLoadBalancer** seçeneğine tıklayın.
2. **Ayarlar**'ın altında **Arka uç havuzları**'na tıklayın, ardından arka uç havuzunun listesinde **myBackendPool**'a tıklayın.
3. **myBackendPool** sayfasında aşağıdakileri yapın:
    - Arka uç havuzunda oluşturduğunuz tüm sanal makineleri (*myVM1* ve *myVM2*) eklemek için **Hedef ağ IP yapılandırması ekle** seçeneğine tıklayın.
    - **Tamam**’a tıklayın.
4. Yük dengeleyici arka uç havuzu ayarınızın tüm VM’leri (**VM1** ve **VM2**) görüntülediğinden emin olun.

## <a name="create-inbound-nat-rules"></a>Gelen NAT kuralları oluşturma
Load Balancer ile, IP adresinin ön ucundaki belirli bir bağlantı noktasından trafiği sanal ağ içindeki bir arka uçtaki belirli bir bağlantı noktasına iletmek için bir gelen NAT kuralı oluşturabilirsiniz.

Yük dengeleyicinin ön uç bağlantı noktalarından arka uç VM’leri için 3389 bağlantı noktasına trafiği iletmek için gelen NAT kuralı oluşturun.

1. Sol taraftaki menüden **Tüm kaynaklar**’a tıklayın ve sonra kaynak listesinden **myLoadBalancer** seçeneğine tıklayın.
2. **Ayarlar**'ın altında **Gelen NAT kuralları**'na tıklayın, ardından arka uç havuzunun listesinde **myBackendPool**'a tıklayın.
3. **Gelen NAT kuralı ekle** sayfasında, aşağıdaki değerleri girin:
    - NAT kuralının adı olarak *myNATRuleRDPVM1* yazın,
    - Bağlantı noktası için *4221* yazın.
    - **Hedef sanal makine** için, açılan menüden *myVM1*'i seçin.
    - **Bağlantı noktası eşleme** için, Özel’e tıklayın ve **Hedef bağlantı noktası** için **3389** yazın.
    - **Tamam**’a tıklayın.
4. *myVM2* sanal makineleri için *4222* ön uç bağlantı noktasını kullanarak *myNATRuleRDPVM2* adlı gelen NAT kurallarını oluşturmak için 2. ve 3. adımı tekrarlayın.

## <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme
1. **Genel Bakış** ekranında Yük Dengeleyici için genel IP adresini bulun. **Tüm kaynaklar**’a ve sonra **myPublicIP** seçeneğine tıklayın.

2. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın. IIS Web sunucusunun varsayılan sayfası, tarayıcıda görüntülenir.

      ![IIS Web sunucusu](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

Yük dengeleyicinin trafiği, uygulamanızı çalıştıran üç VM’ye dağıtmasını görmek için web tarayıcınızı yenilemeye zorlayabilirsiniz.

## <a name="test-port-forwarding"></a>Bağlantı noktası iletmeyi test etme
Bağlantı noktası iletme ile, yük dengeleyicinin IP adresini ve önceki adımda tanımlanan ön uç bağlantı noktası değerini kullanarak arka uç adres havuzundaki VM’ler ile uzak masaüstü bağlantısı kurabilirsiniz.

1. **Genel Bakış** ekranında Yük Dengeleyici için genel IP adresini bulun. **Tüm kaynaklar**’a ve sonra **myPublicIP** seçeneğine tıklayın.
2. Yerel bilgisayarınızdan *myVM2* VM’si ile bir uzak masaüstü oturumu oluşturmak için aşağıdaki komutu kullanın. `<publicIpAddress>` değerini önceki adımda döndürülen IP adresi ile değiştirin.

    ```
    mstsc /v:<publicIpAddress>:4222
    ```
  
3. İndirilen RDP dosyasını açın. İstendiğinde **Bağlan**’ı seçin.

4. Sanal makineyi oluştururken belirttiğiniz kullanıcı adını ve parolayı girin (sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan**’ı seçmeniz gerekebilir), ardından **Tamam**’ı seçin. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için **Evet**’i seçin.
 
   *myNATRuleRDPVM2* gelen NAT kuralına göre RDP bağlantısı başarılı olur, yük dengeleyicinin Ön uç bağlantı noktasından (**4222**) trafik *myVM2* sanal makinesinin 3389 bağlantı noktasına yönlendirilmek için yapılandırılır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, yük dengeleyiciyi ve tüm ilgili kaynakları silin. Bunu yapmak için, yük dengeleyiciyi içeren kaynak grubunu seçin ve **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir Standart Yük Dengeleyici oluşturdunuz, buna sanal makineler eklediniz, yük dengeleyici trafik kuralını ve durum araştırmasını yapılandırdınız ve yük dengeleyiciyi test ettiniz. Ayrıca, yük dengeli kümeden bir VM kaldırdınız ve VM'yi arka uç adres havuzuna geri eklediniz. Azure Load Balancer hakkında daha fazla bilgi almak için Azure Load Balancer öğreticisine devam edin.

> [!div class="nextstepaction"]
> [Azure Load Balancer öğreticileri](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
