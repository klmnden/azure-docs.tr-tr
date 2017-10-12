---
title: "İnternet’e yönelik yük dengeleyicisi oluşturma - Azure portal | Microsoft Docs"
description: "Azure portalını kullanarak Resource Manager’da İnternet’e yönelik yük dengeleyici oluşturmayı öğrenin"
services: load-balancer
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: aa9d26ca-3d8a-4a99-83b7-c410dd20b9d0
ms.service: load-balancer
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: annahar
ms.openlocfilehash: 922c33f712e160835256ad9ad040e523dfbf76db
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="creating-an-internet-facing-load-balancer-using-the-azure-portal"></a>Azure portalını kullanarak İnternet'e yönelik Yük Dengeleyici oluşturma

> [!div class="op_single_selector"]
> * [Portal](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [Şablon](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Bu makalede Resource Manager dağıtım modeli anlatılmaktadır.

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

Bu sayfada yük dengeleyici oluşturmak için yapılması gereken işlemlerin sırası verilmekte ve hedefe ulaşmak için yapılması gerekenler ayrıntılı bir şekilde anlatılmaktadır.

## <a name="what-is-required-to-create-an-internet-facing-load-balancer"></a>İnternet’e yönelik yük dengeleyici oluşturmak için neler gerekir?

Yük dengeleyici dağıtmak için aşağıdaki nesneleri oluşturmanız ve yapılandırmanız gerekir.

* Ön uç IP yapılandırması: Gelen ağ trafiği için genel IP adreslerini içerir.
* Arka uç adres havuzu: Sanal makinelerin yük dengeleyiciden ağ trafiği alması için ağ arabirimlerini (NIC’ler) içerir.
* Yük dengeleme kuralları: Yük dengeleyici üzerindeki bir genel bağlantı noktasını arka uç adres havuzundaki bağlantı noktasına eşleme kurallarını içerir.
* Gelen NAT kuralları: Yük dengeleyici üzerindeki bir genel bağlantı noktasını arka uç adres havuzundaki belirli bir sanal makineye ait bağlantı noktasına eşleme kurallarını içerir.
* Araştırmalar: Arka uç adres havuzundaki sanal makine örneklerinin kullanılabilirliğini kontrol etmek için kullanılan durum araştırmalarını içerir.

Azure Resource Manager içindeki yük dengeleyici bileşenleri hakkında daha fazla bilgi için bkz. [Azure Resource Manager Yük Dengeleyici desteği](load-balancer-arm.md).

## <a name="set-up-a-load-balancer-in-azure-portal"></a>Azure portalında yük dengeleyici kurma

> [!IMPORTANT]
> Bu örnekte **myVNet** adında bir sanal ağınız olduğu varsayılmaktadır. Bu ağı oluşturmak için [sanal ağ oluşturma](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) bölümüne bakın. Örnekte ayrıca **myVNet** içinde **LB-Subnet-BE** ile **web1** ve **web2** adında ve sırasıyla **myAvailSet** ve **myVNet** adlı kullanılabilirlik kümesinde iki VM olduğu varsayılmaktadır. VM oluşturmak için [bu bağlantıya](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) başvurun.

1. Tarayıcıdan Azure portalına ([http://portal.azure.com](http://portal.azure.com)) gidin ve Azure hesabınızla oturum açın.
2. Ekranın sol üst tarafından **Yeni** > **Ağ** > **Yük Dengeleyici**’yi seçin.
3. **Yük dengeleyici oluştur** dikey penceresinde yük dengeleyiciniz için bir ad girin. Burada **myLoadBalancer** kullanılmıştır.
4. **Tür** bölümünde **Genel**’i seçin.
5. **Genel IP adresi** bölümünde **myPublicIP** adında yeni bir genel IP oluşturun.
6. Kaynak Grubu altında **myRG**’yi seçin. Ardından uygun bir **Konum** seçin ve **Tamam**’a tıklayın. Yük dengeleyici dağıtımı başlar ve birkaç dakika içinde başarıyla tamamlanır.

    ![Yük dengeleyicinin kaynak grubunu güncelleştirme](./media/load-balancer-get-started-internet-portal/1-load-balancer.png)

## <a name="create-a-back-end-address-pool"></a>Arka uç adres havuzu oluşturma

1. Başarıyla dağıtılan yük dengeleyicinizi kaynaklarınızdan seçin. Ayarlar’ın altında Arka Uç Havuzları’nı seçin. Arka uç havuzunuz için bir ad girin. Ardından açılan dikey pencerenin üst tarafındaki **Ekle** düğmesine tıklayın.
2. **Arka uç havuzu ekle** dikey penceresinde **Sanal makine ekle**’ye tıklayın.  **Kullanılabilirlik kümesi** bölümünde **Kullanılabilirlik kümesi seç**’e tıklayıp **myAvailSet**’i seçin. Sonraki adımda dikey pencerenin Sanal Makineler bölümünden **Sanal makineleri seç**’e ve ardından yük dengeleme için oluşturulmuş olan **web1** ve **web2** VM’lerine tıklayın. İkisinin de yanında aşağıdaki resimde gösterilen şekilde mavi onay işareti olduğundan emin olun. Ardından aynı dikey pencerede **Seç**’e, **Sanal makineleri seç** dikey penceresinde Tamam’a ve ardından **Arka uç havuzu ekle** dikey penceresinde **Tamam**’a tıklayın.

    ![Arka uç adres havuzuna ekleme - ](./media/load-balancer-get-started-internet-portal/3-load-balancer-backend-02.png)

3. Bildirimler açılır listenizi kontrol ederek yük dengeleyici arka uç havuzunun kaydedilmesi ve hem **web1** hem de **web2** VM’leri için ağ arabirimlerinin güncelleştirilmesi bildirimlerinin göründüğünden emin olun.

## <a name="create-a-probe-lb-rule-and-nat-rules"></a>Araştırma, LB kuralı ve NAT kuralları oluşturma

1. Durum araştırması oluşturun.

    Yük dengeleyicinizin Ayarlar sayfasında Araştırmalar’ı seçin. Ardından dikey pencerenin en üstünde yer alan **Ekle**’ye tıklayın.

    Araştırmaları iki şekilde yapılandırabilirsiniz: HTTP veya TCP. Bu örnekte HTTP gösterilmektedir ancak TCP de benzer şekilde yapılandırılabilir.
    Gerekli bilgileri güncelleştirin. Daha önceden belirtildiği üzere **myLoadBalancer**, 80 numaralı bağlantı noktasına gelen trafik için yük dengeleme gerçekleştirir. Seçilen yol HealthProbe.aspx, Aralık 15 saniye, Sağlıksız durum eşiği ise 2 olarak belirlenmiştir. Ayarları tamamladıktan sonra araştırmayı oluşturmak için **Tamam**’a tıklayın.

    Bu yapılandırmalar ve onları ihtiyaçlarınızı karşılayacak şekilde nasıl değiştirebileceğiniz hakkında bilgi almak için işaretçiyi "i" simgesinin üzerine getirin.

    ![Araştırma ekleme](./media/load-balancer-get-started-internet-portal/4-load-balancer-probes.png)

2. Yük dengeleyici kuralı oluşturun.

    Yük dengeleyicinizin Ayarlar bölümünde Yük dengeleme kuralları’na tıklayın. Yeni dikey pencerede **Ekle**’ye tıklayın. Kuralınıza bir ad verin. Burada HTTP kullanılmıştır. Ön uç ve arka uç bağlantı noktalarını seçin. Burada ikisi için de 80 seçilmiştir. Arka uç havuzu olarak **LB-backend**, Araştırma olarak da önceki adımlarda oluşturduğunuz **HealthProbe**’u seçin. Diğer yapılandırmaları ihtiyaçlarınıza göre belirleyebilirsiniz. Ardından Tamam’a tıklayarak yük dengeleme kuralını kaydedebilirsiniz.

    ![Yük dengeleme kuralı ekleme](./media/load-balancer-get-started-internet-portal/5-load-balancing-rules.png)

3. Gelen NAT kuralları oluşturma

    Yük dengeleyicinizin ayarlar bölümünde Gelen NAT kuralları’na tıklayın. Açılan yeni dikey pencerede **Ekle**’ye tıklayın. Ardından gelen NAT kuralınıza bir ad verin. Burada **inboundNATrule1** kullanılmıştır. Hedefin daha önce oluşturulan Genel IP olması gerekir. Hizmet bölümünde Özel’i seçip kullanmak istediğiniz protokolü belirleyin. Burada TCP seçilmiştir. Bağlantı noktası olarak 3441, Hedef bağlantı noktası olarak ise 3389 yazın. Ardından Tamam’a tıklayarak bu kuralı kaydedin.

    İlk kuralı oluşturduktan sonra bu adımları tekrarlayarak inboundNATrule2 adında ve 3442 numaralı bağlantı noktasından 3389 numaralı Hedef bağlantı noktasına giden ikinci gelen NAT kuralını oluşturun.

    ![Gelen NAT kuralı ekleme](./media/load-balancer-get-started-internet-portal/6-load-balancer-inbound-nat-rules.png)

## <a name="remove-a-load-balancer"></a>Yük Dengeleyici kaldırma

Bir yük dengeleyici silmek için kaldırmak istediğiniz yük dengeleyiciyi seçin. *Yük Dengeleyici* dikey penceresinin en üstünde yer alan **Sil**’e tıklayın. Ardından açılan istemde **Evet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Bir iç yük dengeleyici yapılandırmaya başlayın](load-balancer-get-started-ilb-arm-cli.md)

[Yük dengeleyici dağıtım modu yapılandırma](load-balancer-distribution-mode.md)

[Yük dengeleyiciniz için boşta TCP zaman aşımı ayarlarını yapılandırma](load-balancer-tcp-idle-timeout.md)
