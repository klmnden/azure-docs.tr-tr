---
title: 'Öğretici: Bir bölge içindeki Yük Dengeleyici sanal makineleri - Azure portalı | Microsoft Docs'
description: Bu öğretici, Azure portalını kullanarak bir kullanılabilirlik alanı içindeki sanal makinelerin yükünü dengelemek üzere bölge ön ucu ile Standart bir Yük Dengeleyici oluşturma işlemini gösterir
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: As an IT administrator, I want to create a load balancer that load balances incoming internet traffic to virtual machines within a specific zone in a region.
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/17/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 52d0aeabab173caf4460827ca0d5984070688f0e
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/18/2018
ms.locfileid: "34304734"
---
# <a name="tutorialload-balance-vms-within-an-availability-zone-with-a-standard-load-balancer-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak Standart Yük Dengeleyici ile bir kullanılabilirlik alanındaki sanal makinelerde yük dengeleme

Bu öğreticide Azure portalı üzerinden bir Genel IP Standart adresi kullanarak bir bölge ön ucu ile genel bir [Standart Yük Dengeleyici](https://aka.ms/azureloadbalancerstandard) oluşturma adımları gösterilmektedir. Bu senaryoda, veri yolunuzu ve kaynaklarınızı belirli bir bölge ile hizalamak amacıyla ön uç ve arka uç örnekleriniz için belirli bir bölge belirtirsiniz. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir bölge ön ucu ile Azure Standart Yük Dengeleyicisi oluşturma
> * Gelen trafik kurallarını tanımlamak için ağ güvenlik grupları oluşturma
> * Bölge VM’leri oluşturma ve yük dengeleyiciye ekleme
> * Yük dengeleyici durum yoklaması oluşturma
> * Yük dengeleyici trafik kuralları oluşturma
> * Temel IIS sitesi oluşturma
> * Çalışan yük dengeleyiciyi görüntüleme

Standart Yük Dengeleyici ile Kullanılabilirlik alanlarını kullanma hakkında daha fazla bilgi için [Standart Yük Dengeleyici ve Kullanılabilirlik Alanları](load-balancer-standard-availability-zones.md).

Tercih ederseniz, [Azure CLI](load-balancer-standard-public-zonal-cli.md) kullanarak bu öğreticiyi tamamlayabilirsiniz.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[http://portal.azure.com](http://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-public-standard-load-balancer"></a>Genel bir Standart Yük Dengeleyici oluşturma

Standart Yük Dengeleyici yalnızca Standart Genel IP adresini destekler. Yük dengeleyiciyi oluştururken yeni bir genel IP oluşturduğunuzda, bir Standart SKU sürümü halinde otomatik olarak yapılandırılır ve ayrıca otomatik olarak bölgesel olarak yedeklidir.

1. Ekranın sol üst kısmında **Kaynak oluştur** > **Ağ** > **Yük Dengeleyici**'ye tıklayın.
2. **Yük dengeleyici oluştur** sayfasına, yük dengeleyici için şu değerleri girin:
    - Yük dengeleyicinin adı için *myLoadBalancer*.
    - Yük dengeleyicinin türü için **Public**.
     - *myPublicIPZonal* - Oluşturduğunuz yeni genel IP adresi. Bunu yapmak için **Genel IP adresi seçin**’e ve sonra **Yeni oluştur**’a tıklayın. Ad için *myPublicIP* yazın. SKU varsayılan olarak Standart’tır ve **Kullanılabilirlik alanı** olarak **Bölge 1**’i seçer.
    - *myResourceGroupZLB* -  oluşturduğunuz yeni kaynak grubunun adı.
    - Konum için **westeurope**.
3. Yük dengeleyiciyi oluşturmak için **Oluştur**’a tıklayın.
   
    ![Azure portalı ile bölgesel Standart Yük Dengeleyici oluşturma](./media/tutorial-load-balancer-standard-zonal-portal/create-load-balancer-zonal-frontend.png)


## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu bölümde bir sanal ağ oluşturacak, yük dengeleyicinizin arka uç havuzuna eklenecek bölge için aynı alanda (bölge 1) iki sanal makine oluşturacak ve sonra bölgesel olarak yedekli yük dengeleyicinin test edilmesine yardımcı olmak için sanal makinelere IIS yükleyeceksiniz. Bu nedenle, bir VM başarısız olursa aynı alandaki VM’nin sistem durumu yoklaması başarısız olur ve trafik aynı alandaki diğer VM’ler tarafından sunulmaya devam eder.

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma
1. Ekranın sol üst kısmında **Kaynak oluştur** > **Ağ** > **Sanal ağ**'e tıklayın ve sanal ağ için şu değerleri girin:
    - Sanal ağ adı için *myVnet*.
    - *myResourceGroupZLB* - Mevcut kaynak grubunun adı
    - Alt ağ adı için *myBackendSubnet*.
2. Sanal ağı oluşturmak için **Oluştur**’a tıklayın.

    ![Sanal ağ oluşturma](./media/tutorial-load-balancer-standard-zonal-portal/create-virtual-network.png)

## <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

1. Ekranın sol üst kısmında **Kaynak oluştur**’a tıklayın, arama kutusuna *Ağ Güvenlik Grubu* yazın ve ağ güvenlik grubu sayfasında **Oluştur**’a tıklayın.
2. Ağ güvenlik grubu oluşturun sayfasında şu değerleri girin:
    - *myNetworkSecurityGroup*  - Ağ güvenlik grubunun adı.
    - *myResourceGroupLBAZ* - Mevcut kaynak grubunun adı.
   
    ![Sanal ağ oluşturma](./media/tutorial-load-balancer-standard-zonal-portal/create-network-security-group.png)

### <a name="create-nsg-rules"></a>NSG kuralları oluşturma

Bu bölümde, Azure portalı ile HTTP ve RDP kullanarak gelen bağlantılara izin vermek için NSG kuralları oluşturacaksınız.

1. Azure portalında, sol menüden **Tüm kaynaklar**’a tıklayın ve **myResourceGroupSLB** kaynak grubunda bulunan **myNetworkSecurityGroup** öğesini bulup tıklayın.
2. **Ayarlar**’ın altında **Gelen güvenlik kuralları**’na ve sonra **Ekle**’ye tıklayın.
3. 80 numaralı bağlantı noktasını kullanarak gelen HTTP bağlantılarına izin vermek için *myHTTPRule* adlı gelen güvenlik kuralı için şu değerleri girin:
    - **Kaynak** için *Hizmet Etiketi*.
    - **Kaynak hizmet etiketi** için *İnternet*
    - **Hedef bağlantı noktası aralıkları** için *80*
    - **Protokol** için *TCP*
    - **Eylem** için *İzin Ver*
    - **Öncelik** için *100*
    - **Ad** için *myHTTPRule*
    - **Açıklama** için *HTTP’ye İzin Ver*
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

    ![RDP kuralı oluşturma](./media/tutorial-load-balancer-standard-zonal-portal/create-rdp-rule.png)

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

1. Ekranın sol üst kısmında **Kaynak oluştur** > **İşlem** > **Windows Server 2016 Datacenter**'a tıklayın ve sanal makine için şu değerleri girin:
    - Sanal makinenin adı için *myVM1*.        
    - Yönetici kullanıcı adı için *azureuser*.    
    - *myResourceGroupZLB* - **Kaynak grubu** için **Var olanı kullan**’ı seçin ve sonra *myResourceGroupZLB* öğesini seçin.
2. **Tamam**’a tıklayın.
3. Sanal makinenin boyutu için **DS1_V2** seçeneğini belirleyin ve **Seç**’e tıklayın.
4. Sanal makine ayarları için şu değerleri girin:
    - *bölge 1* - VM’yi yerleştirdiğiniz Kullanılabilirlik alanı.
    -  Sanal makine olarak *myVNet*'in seçildiğinden emin olun.
    - *myVM1PIP* - Oluşturduğunuz Standart Genel IP Adresi. *Yeni oluştur*’a tıklayın ve sonra ad için *myVM1PIP* yazıp Bölge için **1**’i seçin. IP adresi SKU’su varsayılan olarak Standart'tır.
    - Alt ağ olarak *myBackendSubnet*'in seçildiğinden emin olun.
    - *myNetworkSecurityGroup* - Zaten mevcut olan ağ güvenlik grubunun (güvenlik duvarı) adı.
5. Önyükleme tanılamalarını devre dışı bırakmak için **Devre Dışı** seçeneğine tıklayın.
6. **Tamam**’a tıklayın, özet sayfasındaki ayarları gözden geçirin ve sonra **Oluştur**’a tıklayın.
7. Adım 1-6’yı kullanarak, sanal ağ *myVnet*, Standart Genel IP adresi *myVM2PIP*, alt ağ *myBackendSubnet* ve ağ güvenlik grubu **myNetworkSecurityGroup* olacak şekilde Bölge 1’de *myVM2* adlı ikinci bir VM oluşturun.

    ![RDP kuralı oluşturma](./media/tutorial-load-balancer-standard-zonal-portal/create-virtual-machine.png) 

### <a name="install-iis-on-vms"></a>VM’lere IIS yükleme

1. Sol menüden **Tüm kaynaklar**’a tıklayın ve kaynak listesinden, *myResourceGroupZLB* kaynak grubunda bulunan **myVM1** öğesine tıklayın.
2. Sanal makineye yönelik RDP için **Genel Bakış** sayfasında **Bağlan**’a tıklayın.
3. Sanal makineyi oluştururken belirttiğiniz kullanıcı adı ve parola ile VM’de oturum açın (sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan**’ı seçmeniz gerekebilir), ardından **Tamam**’ı seçin. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için **Evet**’i seçin.
4. Sunucu masaüstünde **Windows Yönetimsel Araçları**>**Windows PowerShell** bölümüne gidin.
6. PowerShell Penceresinde aşağıdaki komutları çalıştırarak IIS sunucusunu yükleyin, varsayılan iisstart.htm dosyasını kaldırın ve ardından VM’nin adını gösteren yeni bir iisstart.htm dosyasını ekleyin:

   ```azurepowershell-interactive
    # install IIS server role
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    # remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    # Add a new htm file that displays server name
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from" + $env:computername)
   ```
7. *myVM1* ile RDP oturumunu kapatın
8. *myVM2*’ye IIS yüklemek için 1 ile 7 arasındaki adımları tekrarlayın.

## <a name="create-load-balancer-resources"></a>Yük dengeleyici kaynakları oluşturma

Bu bölümde, arka uç adres havuzu ve durum araştırması için yük dengeleyici ayarlarını yapılandıracak ve yük dengeleyici ve NAT kurallarını belirteceksiniz.


### <a name="create-a-backend-address-pool"></a>Arka uç adres havuzu oluşturma

Trafiği VM’lere dağıtmak için, bir arka uç adres havuzunda yük dengeleyiciye bağlı sanal NIC’lerin IP adresleri barındırılır. *VM1* ve *VM2* öğesini eklemek için *myBackendPool* adlı arka uç adres havuzunu oluşturun.

1. Sol taraftaki menüden **Tüm kaynaklar**’a tıklayın ve sonra kaynak listesinden **myLoadBalancer* seçeneğine tıklayın.
2. **Ayarlar** bölümünde **Arka uç havuzları**’na ve sonra **Ekle**’ye tıklayın.
3. **Arka uç havuzu ekle** sayfasında aşağıdakileri yapın:
    - Ad için, arka uç havuzunuzun adı olarak *myBackEndPool* yazın.
    - **Sanal ağ** için açılır menüde *myVNet* öğesine tıklayın
    - **Sanal makine** ve **IP adresi** için *myVM1*, *myVM2* ve karşılık gelen genel IP adreslerini ekleyin.
4. **Ekle**'ye tıklayın.
5. Yük dengeleyici arka uç havuzu ayarınızın hem **myVM1** hem de **myVM2** sanal makinelerini görüntülediğinden emin olun.
 
    ![Arka uç havuzu oluşturma](./media/tutorial-load-balancer-standard-zonal-portal/create-backend-pool.png) 

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
    
    ![Yük dengeleme kuralı ekleme](./media/tutorial-load-balancer-standard-zonal-portal/load-balancing-rule.png)

## <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme
1. **Genel Bakış** ekranında Yük Dengeleyici için genel IP adresini bulun. **Tüm kaynaklar**’a ve sonra **myPublicIP** seçeneğine tıklayın. 

2. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın. Web sunucusu sayfasının adını içeren varsayılan sayfa, tarayıcıda görüntülenir.

      ![IIS Web sunucusu](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)
3. Yük dengeleyiciyi uygulamada görmek için, gösterilen VM’yi durmaya zorlayın ve tarayıcıyı yenileyerek tarayıcıda diğer sunucu adının gösterildiğini görün.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, yük dengeleyiciyi ve tüm ilgili kaynakları silin. Bunu yapmak için, yük dengeleyiciyi içeren kaynak grubunu seçin ve **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Standart Yük Dengeleyici](load-balancer-standard-overview.md) hakkında daha fazla bilgi edinin.
- [Kullanılabilirlik bölgelerindeki VM’lerde yük dengeleme](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
