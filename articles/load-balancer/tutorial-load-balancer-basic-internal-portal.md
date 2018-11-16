---
title: 'Öğretici: Genel Temel Yük Dengeleyici oluşturma - Azure portal | Microsoft Docs'
description: Bu öğreticide Azure portalını kullanarak bir iç Temel Yük Dengeleyici oluşturma işlemi gösterilmektedir.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: As an IT administrator, I want to create a load balancer that load balances incoming internet traffic to virtual machines within a specific zone in a region.
ms.assetid: aa9d26ca-3d8a-4a99-83b7-c410dd20b9d0
ms.service: load-balancer
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/28/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: a5b6ae833fcd340a639c068156940e6b9ad469ca
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51711999"
---
# <a name="tutorial-load-balance-internal-traffic-with-basic-load-balancer-to-vms-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak Temel Yük Dengeleyici ile VM’lere iç trafiğin yükünü dengeleme

Yük dengeleme, gelen istekleri birden fazla sanal makineye yayarak daha yüksek bir kullanılabilirlik ve ölçek düzeyi sağlar. Bir Temel Yük Dengeleyici ile sanal makinelere iç trafiğin yük dengelemesini yapmak için Azure portalını kullanabilirsiniz. Bu öğreticide, ağ kaynaklarının, arka uç sunucularının ve bir iç Temel Yük Dengeleyicinin nasıl oluşturulacağı gösterilmektedir.

Tercih ederseniz, [Azure CLI](load-balancer-get-started-ilb-arm-cli.md) veya [Azure PowerShell](load-balancer-get-started-ilb-arm-ps.md) kullanarak bu öğreticiyi tamamlayabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma
1. Ekranın sol üst kısmında **Yeni** > **Ağ** > **Sanal ağ**'e tıklayın ve sanal ağ için şu değerleri girin:
    - Sanal ağ adı için *myVnet*.
    - *myResourceGroupILB* - Mevcut kaynak grubunun adı
    - Alt ağ adı için *myBackendSubnet*.
2. Sanal ağı oluşturmak için **Oluştur**’a tıklayın.

![Yük dengeleyici oluşturma](./media/tutorial-load-balancer-basic-internal-portal/1-load-balancer.png)

## <a name="create-a-basic-load-balancer"></a>Temel Yük Dengeleyici oluşturma
Portalı kullanarak bir iç Temel Yük Dengeleyici oluşturun.

1. Ekranın sol üst kısmında **Kaynak oluştur** > **Ağ** > **Yük Dengeleyici**'ye tıklayın.
2. **Yük dengeleyici oluştur** sayfasına, yük dengeleyici için şu değerleri girin:
    - Yük dengeleyicinin adı için *myLoadBalancer*.
    - **İç** - Yük dengeleyicinin türü.
    - **Temel** - SKU sürümü.
    - **10.1.0.7** - Statik özel IP adresi.
    - *myVNet* - Mevcut ağların listesinden seçtiğiniz sanal ağ.
    - *mySubnet* - Mevcut alt ağların listesinden seçtiğiniz alt ağ.
    - *myResourceGroupILB* - oluşturduğunuz yeni kaynak grubunun adı.
3. Yük dengeleyiciyi oluşturmak için **Oluştur**’a tıklayın.
   
    ## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu bölümde, Temel Yük Dengeleyicinizin arka uç havuzu için iki sanal makine oluşturacak ve sonra yük dengeleyicinin test edilmesi için sanal makinelere IIS yükleyeceksiniz.

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

1. Ekranın sol üst kısmında **Kaynak oluştur** > **İşlem** > **Windows Server 2016 Datacenter**'a tıklayın ve sanal makine için şu değerleri girin:
    - Sanal makinenin adı için *myVM1*.        
    - Yönetici kullanıcı adı için *azureuser*.   
    - *myResourceGroupILB* - **Kaynak grubu** için **Var olanı kullan**’ı seçin ve sonra *myResourceGroupILB* öğesini seçin.
2. **Tamam** düğmesine tıklayın.
3. Sanal makinenin boyutu için **DS1_V2** seçeneğini belirleyin ve **Seç**’e tıklayın.
4. Sanal makine ayarları için şu değerleri girin:
    - Oluşturduğunuz yeni Kullanılabilirlik kümesinin adı için *myAvailabilitySet*.
    -  Sanal makine olarak *myVNet*'in seçildiğinden emin olun.
    - Alt ağ olarak *myBackendSubnet*'in seçildiğinden emin olun.
5. **Ağ Güvenlik Grubu** bölümünde **Gelişmiş**'i seçin. Ardından **Ağ güvenlik grubu (güvenlik duvarı)** için **Yok**'u seçin.
5. Önyükleme tanılamalarını devre dışı bırakmak için **Devre Dışı** seçeneğine tıklayın.
6. **Tamam**’a tıklayın, özet sayfasındaki ayarları gözden geçirin ve sonra **Oluştur**’a tıklayın.
7. 1-6 arası adımları kullanarak, Kullanılabilirlik kümesi *myAvailabilityset*, sanal ağ *myVnet*, alt ağ *myBackendSubnet* ve **Ağ güvenlik grubu (güvenlik grubu)** için **Yok**'u seçerek *VM2* adlı ikinci bir sanal makine oluşturun. 

### <a name="install-iis-and-customize-the-default-web-page"></a>IIS yükleme ve varsayılan web sayfasını özelleştirme

1. Sol menüden **Tüm kaynaklar**’a tıklayın ve kaynak listesinden, *myResourceGroupILB* kaynak grubunda bulunan **myVM1** öğesine tıklayın.
2. Sanal makineye yönelik RDP için **Genel Bakış** sayfasında **Bağlan**’a tıklayın.
3. VM'de oturum açın.
4. Sunucu masaüstünde **Windows Yönetimsel Araçları**>**Sunucu Yöneticisi** bölümüne gidin.
5. VM1’de Windows PowerShell’i başlatın ve IIS sunucusunu yükleyip varsayılan htm dosyasını güncelleştirmek için aşağıdaki komutları kullanın.
    ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
    ```
5. *myVM1* ile RDP bağlantısını kapatın.
6. IIS yüklemek ve varsayılan web sayfasını özelleştirmek için *myVM2* ile adım 1-5’i yineleyin.

## <a name="create-basic-load-balancer-resources"></a>Temel Yük Dengeleyici kaynakları oluşturma

Bu bölümde, arka uç adres havuzu ve durum araştırması için yük dengeleyici ayarlarını yapılandıracak ve yük dengeleyici kurallarını belirteceksiniz.


### <a name="create-a-backend-address-pool"></a>Arka uç adres havuzu oluşturma

Trafiği VM’lere dağıtmak için, bir arka uç adres havuzunda yük dengeleyiciye bağlı sanal NIC’lerin IP adresleri barındırılır. *VM1* ve *VM2* öğesini eklemek için *myBackendPool* adlı arka uç adres havuzunu oluşturun.

1. Sol menüden **Tüm kaynaklar**’a tıklayın ve sonra kaynak listesinden **myLoadBalancer** seçeneğine tıklayın.
2. **Ayarlar** bölümünde **Arka uç havuzları**’na ve sonra **Ekle**’ye tıklayın.
3. **Arka uç havuzu ekle** sayfasında aşağıdakileri yapın:
    - Ad için, arka uç havuzunuzun adı olarak *myBackEndPool* yazın.
    - **İlişkilendirildiği öğe** için, açılır menüden **Kullanılabilirlik kümesi**’ne tıklayın
    - **Kullanılabilirlik kümesi** için **myAvailabilitySet** seçeneğine tıklayın.
    - Arka uç havuzunda oluşturduğunuz tüm sanal makineleri (*myVM1* & *myVM2*) eklemek için **Hedef ağ IP yapılandırması ekle** seçeneğine tıklayın.
    - **Tamam** düğmesine tıklayın.

        ![Arka uç adres havuzuna ekleme - ](./media/tutorial-load-balancer-basic-internal-portal/3-load-balancer-backend-02.png)

3. Yük dengeleyici arka uç havuzu ayarınızın hem **VM1** hem de **VM2** sanal makinelerini görüntülediğinden emin olun.

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

Temel Yük Dengeleyicinin uygulamanızın durumunu izlemesine izin vermek için durum araştırması kullanabilirsiniz. Durum yoklaması, durum denetimlerine verdikleri yanıtlara göre VM’leri dinamik olarak yük dengeleyici rotasyonuna ekler ve kaldırır. Sanal makinelerin durumunu izlemek için *myHealthProbe* durum araştırması oluşturun.

1. Sol taraftaki menüden **Tüm kaynaklar**’a tıklayın ve sonra kaynak listesinden **myLoadBalancer** seçeneğine tıklayın.
2. **Ayarlar** bölümünde **Durum araştırmaları**’na ve sonra **Ekle**’ye tıklayın.
3. Durum araştırması oluşturmak için şu değerleri kullanın:
    - Durum araştırmasının adı için *myHealthProbe*.
    - Protokol türü için **HTTP**.
    - Bağlantı noktası numarası için *80*.
    - Araştırma denemeleri arasındaki saniye cinsinden **Aralık** için *15*.
    - Bir sanal makinenin sağlıksız olduğu kanısına varılmadan önce gerçekleşmesi gereken ardışık araştırma hatası veya **Sağlıksız eşik** sayısı için *2*.
4. **Tamam** düğmesine tıklayın.

   ![Araştırma ekleme](./media/tutorial-load-balancer-basic-internal-portal/4-load-balancer-probes.png)

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
4. **Tamam** düğmesine tıklayın.
    
    ![Yük dengeleme kuralı ekleme](./media/tutorial-load-balancer-basic-internal-portal/5-load-balancing-rules.png)

## <a name="create-a-virtual-machine-to-test-the-load-balancer"></a>Yük dengeleyiciyi test etmek için sanal makine oluşturma
İç yük dengeleyiciyi test etmek için, arka uç sunucusunun sanal makineleri ile aynı sanal ağda bulunan bir sanal makine oluşturmanız gerekir.
1. Ekranın sol üst kısmında **Kaynak oluştur** > **İşlem** > **Windows Server 2016 Datacenter**'a tıklayın ve sanal makine için şu değerleri girin:
    - *myVMTest* - Sanal makinenin adı.        
    - *myResourceGroupILB* - **Kaynak grubu** için **Var olanı kullan**’ı seçin ve sonra *myResourceGroupILB* öğesini seçin.
2. **Tamam** düğmesine tıklayın.
3. Sanal makinenin boyutu için **DS1_V2** seçeneğini belirleyin ve **Seç**’e tıklayın.
4. Sanal makine ayarları için şu değerleri girin:
    -  Sanal makine olarak *myVNet*'in seçildiğinden emin olun.
    - Alt ağ olarak *myBackendSubnet*'in seçildiğinden emin olun.
5. Önyükleme tanılamalarını devre dışı bırakmak için **Devre Dışı** seçeneğine tıklayın.
6. **Tamam**’a tıklayın, özet sayfasındaki ayarları gözden geçirin ve sonra **Oluştur**’a tıklayın.

## <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme
1. Azure portalında, **Genel Bakış** ekranından Yük Dengeleyici için Özel IP adresini alın. Bunu yapmak için: a. Sol taraftaki menüden **Tüm kaynaklar**’a tıklayın ve sonra kaynak listesinden **myLoadBalancer** seçeneğine tıklayın.
    b. **Genel Bakış** ayrıntılar sayfasında Özel IP adresini kopyalayın (bu örnekte, 10.1.0.7).

2. *myVMTest* ile aşağıdaki gibi bir uzak bağlantı oluşturun: a. Sol menüden **Tüm kaynaklar**’a tıklayın ve kaynak listesinden, *myResourceGroupILB* kaynak grubunda bulunan **myVMTest** öğesine tıklayın.
2. **Genel Bakış** sayfasında **Bağlan**’a tıklayarak VM ile bir uzak oturum başlatın.
3. *myVMTest* oturumunu açın.
3. Özel IP adresini *myVMTest* içindeki tarayıcının adres çubuğuna yapıştırın. IIS Web sunucusunun varsayılan sayfası, tarayıcıda görüntülenir.

      ![IIS Web sunucusu](./media/tutorial-load-balancer-basic-internal-portal/9-load-balancer-test.png)

Yük dengeleyicinin trafiği, uygulamanızı çalıştıran her iki VM’ye de dağıtmasını görmek için web tarayıcınızı yenilemeye zorlayabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, yük dengeleyiciyi ve tüm ilgili kaynakları silin. Bunu yapmak için, yük dengeleyiciyi içeren kaynak grubunu seçin ve **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir kaynak grubu, ağ kaynakları ve arka uç sunucuları oluşturdunuz. Daha sonra bu kaynakları kullanarak bir iç yük dengeleyici oluşturdunuz ve VM’lere iç trafiğin yük dengelemesini yaptınız. Bundan sonra, [Farklı alanlarındaki VM’lerin yükünü dengeleme](tutorial-load-balancer-standard-public-zone-redundant-portal.md) hakkında bilgi edinin
