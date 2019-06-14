---
title: Birden fazla IP yapılandırması - Azure portalı üzerinde Yük Dengelemesi
titlesuffix: Azure Load Balancer
description: Birincil ve ikincil IP yapılandırmaları arasında yük dengelemeyi.
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: se0dec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 0cf5aa45e1e8a28dfcdadac0ea32658e5993d06c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60591703"
---
# <a name="load-balancing-on-multiple-ip-configurations-by-using-the-azure-portal"></a>Birden fazla IP yapılandırması üzerinde Azure portalını kullanarak yük dengelemenin

> [!div class="op_single_selector"]
> * [Portal](load-balancer-multiple-ip.md)
> * [PowerShell](load-balancer-multiple-ip-powershell.md)
> * [CLI](load-balancer-multiple-ip-cli.md)


Bu makalede, bir ikincil ağ arabirimi denetleyicisi (NIC) üzerinde birden çok IP adresi ile Azure Load Balancer'ı kullanma göstermek için ekleyeceğiz. Bizim senaryomuz Aşağıdaki diyagramda gösterilmiştir:

![Yük dengeleyici senaryosu](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

Senaryomuzdaki ise aşağıdaki yapılandırmayı kullanıyoruz:

- İki Windows çalıştıran sanal makinelerin (VM'ler).
- Her VM birincil ve ikincil bir NIC'ye sahiptir.
- Her ikincil NIC iki IP yapılandırmasına sahip değil.
- İki Web sitelerini barındıran her VM: contoso.com ve fabrikam.com.
- Her Web sitesi ikincil NIC üzerinde bir IP yapılandırmasına bağlıdır
- Azure Load Balancer iki ön uç IP adresi, her Web sitesi için bir tane oluşturmak için kullanılır. Ön uç adreslerini her Web sitesi için ilgili IP yapılandırması trafiği dağıtmak için kullanılır.
- Aynı bağlantı noktasını, ön uç IP adresleri ve IP adresleri arka uç havuzu için kullanılır.

## <a name="prerequisites"></a>Önkoşullar

Senaryo örneğimiz, adlı bir kaynak grubu sahibi olduğunuzu varsayar **contosofabrikam** yapılandırılmış gibi:

- Kaynak grubu adında bir sanal ağ içeren **myVNet**.
- **MyVNet** adlı iki sanal ağı içerir **VM1** ve **VM2**.
- VM1 ve VM2 olan aynı kullanılabilirlik kümesinde adlandırılmış **myAvailset**. 
- VM1 ve VM2 sahip adlı birincil NIC **VM1NIC1** ve **VM2NIC1**sırasıyla. 
- VM1 ve VM2 sahip adlı bir ikincil NIC **VM1NIC2** ve **VM2NIC2**sırasıyla.

Birden çok NIC ile VM oluşturma hakkında daha fazla bilgi için bkz. [PowerShell kullanarak birden çok NIC ile VM oluşturma](../virtual-machines/windows/multiple-nics.md).

## <a name="perform-load-balancing-on-multiple-ip-configurations"></a>Birden çok IP yapılandırmalarında Yük Dengelemesi gerçekleştirme

Bu makalede açıklanan senaryo elde etmek için aşağıdaki adımları tamamlayın.

### <a name="step-1-configure-the-secondary-nics"></a>1\. adım: İkincil ağ arabirimlerine yapılandırın

Sanal ağınızda bulunan her VM için ikincil NIC IP yapılandırmasını ekleyin:  

1. Azure portalına gidin: https://portal.azure.com. Azure hesabınızla oturum açın.

2. Ekranın üst sol seçin **kaynak grubu** simgesi. Ardından, Vm'lerinizin yerleştirildiği kaynak grubunu seçin (örneğin, **contosofabrikam**). **Kaynak grupları** bölmesi tüm kaynakları ve NIC VM'ler için görüntüler.

3. Her VM için ikincil NIC IP yapılandırmasını ekleyin:

    1. Yapılandırmak istediğiniz ikincil NIC'ı seçin.
    
    2. Seçin **IP yapılandırmaları**. Sayfanın en üstüne, sonraki bölmesinde seçin **Ekle**.

    3. Altında **ekleme IP yapılandırmaları**, ikinci bir IP yapılandırması için NIC ekleyin: 

        1. İkincil IP yapılandırması için bir ad girin. (Örneğin, VM1 ve VM2 için IP Yapılandırması adı **VM1NIC2 ipconfig2** ve **VM2NIC2 ipconfig2**sırasıyla.)

        2. İçin **özel IP adresi**, **ayırma** ayarını seçin **statik**.

        3. **Tamam**’ı seçin.

İkincil NIC tamamlandıktan sonra ikinci IP Yapılandırması altında gösterilir **IP yapılandırmaları** verilen NIC için ayarları

### <a name="step-2-create-the-load-balancer"></a>2\. adım: Yük dengeleyiciyi oluşturma

Load balancer'ınız için yapılandırmayı oluşturun:

1. Azure portalına gidin: https://portal.azure.com. Azure hesabınızla oturum açın.

2. Ekranın üst sol seçin **kaynak Oluştur** > **ağ** > **yük dengeleyici**. Ardından, **Oluştur**.

3. Altında **yük dengeleyici Oluştur**, load balancer'ınız için bir ad yazın. Bu senaryoda, ad kullanıyoruz **mylb**.

4. Altında **genel IP adresi**, adlı yeni bir genel IP oluşturma **PublicIP1**.

5. Altında **kaynak grubu**, sanal makineleriniz için mevcut kaynak grubunu seçin (örneğin, **contosofabrikam**). Load balancer'ı dağıtın ve ardından konumunu seçin **Tamam**.

Yük Dengeleyici dağıtımı başlar. Dağıtım başarıyla tamamlanması birkaç dakika sürebilir. Dağıtım tamamlandıktan sonra Yük Dengeleyici kaynak grubunuzda bir kaynak olarak görüntülenir.

### <a name="step-3-configure-the-front-end-ip-pool"></a>3\. adım: Ön uç IP havuzu yapılandırma

Her Web sitesi için (contoso.com ve fabrikam.com), ön uç IP havuzu, yük dengeleyicide yapılandırın:

1. Portalında **diğer hizmetler**. Filtre kutusuna **genel IP adresi** seçip **genel IP adresleri**. Sayfanın en üstüne, sonraki bölmesinde seçin **Ekle**.

2. İki ortak IP adreslerini yapılandırın (**PublicIP1** ve **PublicIP2**) her iki Web siteleri (contoso.com ve fabrikam.com) için:

   1. Ön uç IP adresiniz için bir ad yazın.

   2. İçin **kaynak grubu**, sanal makineleriniz için mevcut kaynak grubunu seçin (örneğin, **contosofabrikam**).

   3. İçin **konumu**, Vm'leri olarak aynı konumu seçin.

   4. **Tamam**’ı seçin.

      Genel IP adreslerini oluşturulduktan sonra bunların altında görüntülenen **genel IP** adresleri.

3. <a name="step3-3"></a>Portalında **diğer hizmetler**. Filtre kutusuna **yük dengeleyici** seçip **Load Balancer**. 

4. Yük Dengeleyiciyi seçin (**mylb**) ön uç IP havuzu için eklemek istediğiniz.

5. Altında **ayarları**seçin **ön uç IP yapılandırması**. Sayfanın en üstüne, sonraki bölmesinde seçin **Ekle**.

6. Ön uç IP adresiniz için bir ad yazın (örneğin, **contosofe** veya **fabrikamfe**).

7. <a name="step3-7"></a>Seçin **IP adresi**. Altında **seçin genel IP adresi**, uygulamanızın ön uç IP adresi seçin (**PublicIP1** veya **PublicIP2**).

8. Tekrarlayarak ikinci ön uç IP adresi oluşturma <a href="#step3-3">3. adım</a> aracılığıyla <a href="#step3-7">7. adım</a> bu bölümdeki.

IP adreslerini ön uç havuzu yapılandırdıktan sonra load balancer'ınız altında görüntülenen **ön uç IP yapılandırması** ayarları. 
    
### <a name="step-4-configure-the-back-end-pool"></a>4\. Adım: Arka uç havuzunu yapılandırma

Her Web sitesi için (contoso.com ve fabrikam.com), yük dengeleyicinizin arka uç adres havuzu yapılandırın:
        
1. Portalında **diğer hizmetler**. Filtre kutusuna **yük dengeleyici** seçip **Load Balancer**.

2. Yük Dengeleyiciyi seçin (**mylb**) arka uç havuzuna eklemek istediğiniz.

3. Altında **ayarları**seçin **arka uç havuzları**. Arka uç havuzunuz için bir ad yazın (örneğin, **contosopool** veya **fabrikampool**). Sayfanın en üstüne, sonraki bölmesinde seçin **Ekle**. 

4. İçin **ilişkili**seçin **kullanılabilirlik kümesi**.

5. İçin **kullanılabilirlik kümesi**seçin **myAvailset**.

6. Hedef ağ IP yapılandırmaları için iki VM'nin de ekleyin: 

    ![Yük Dengeleyici için arka uç havuzlarını yapılandırın.](./media/load-balancer-multiple-ip/lb-backendpool.PNG)
    
    1. İçin **hedef sanal makine**, arka uç havuzuna eklemek istediğiniz VM'yi seçin (örneğin, **VM1** veya **VM2**).

    2. İçin **ağ IP yapılandırması**, önceki adımda seçtiğiniz VM ikincil NIC IP yapılandırmasını seçin (örneğin, **VM1NIC2 ipconfig2** veya **VM2NIC2 ipconfig2** ).

7. **Tamam**’ı seçin.

Arka uç havuzu yapılandırdıktan sonra adresleri load balancer'ınız altında görüntülenen **arka uç havuzu** ayarları.

### <a name="step-5-configure-the-health-probe"></a>5\. Adım: Sistem durumu araştırmasını yapılandırma

Load balancer'ınız için bir durum araştırması yapılandırın:

1. Portalında **diğer hizmetler**. Filtre kutusuna **yük dengeleyici** seçip **Load Balancer**.

2. Yük Dengeleyiciyi seçin (**mylb**) sistem durumu araştırması için eklemek istediğiniz.

3. Altında **ayarları**seçin **durum araştırması**. Sayfanın en üstüne, sonraki bölmesinde seçin **Ekle**. 

4. Durum araştırması için bir ad yazın (örneğin, **HTTP**). **Tamam**’ı seçin.

### <a name="step-6-configure-load-balancing-rules"></a>6\. Adım: Yük dengeleme kurallarını yapılandırma

Her Web sitesi için (contoso.com ve fabrikam.com), Yük Dengeleme kuralları yapılandırın:
    
1. <a name="step6-1"></a>Altında **ayarları**seçin **Yük Dengeleme kuralları**. Sayfanın en üstüne, sonraki bölmesinde seçin **Ekle**. 

2. İçin **adı**, Yük Dengeleme kuralı için bir ad yazın (örneğin, **HTTPc** contoso.com için veya **HTTPf** fabrikam.com için).

3. İçin **ön uç IP adresi**, daha önce oluşturduğunuz ön uç IP adresi seçin (örneğin, **contosofe** veya **fabrikamfe**).

4. İçin **bağlantı noktası** ve **arka uç bağlantı noktası**, varsayılan değer tutmak **80**.

5. İçin **kayan IP (doğrudan sunucu dönüşü)** seçin **devre dışı bırakılmış**.

6. <a name="step6-6"></a>Seçin **Tamam**.

7. Tekrarlayarak ikinci yük dengeleyici kuralı oluşturma <a href="#step6-1">1. adım</a> aracılığıyla <a href="#step6-6">6. adım</a> bu bölümdeki.

Kuralları yapılandırıldıktan sonra load balancer'ınız altında görüntülenen **Yük Dengeleme kuralları** ayarları.

### <a name="step-7-configure-dns-records"></a>7\. Adım: DNS kayıtlarını yapılandırın

Son adım olarak, DNS kaynak kayıtlarını load balancer'ınız için ilgili ön uç IP adreslerine işaret edecek şekilde yapılandırın. Azure DNS etki alanlarınızı barındırabilirsiniz. Load Balancer ile Azure DNS kullanma hakkında daha fazla bilgi için bkz. [kullanarak Azure DNS diğer Azure hizmetleriyle](../dns/dns-for-azure-services.md).

## <a name="next-steps"></a>Sonraki adımlar
- Yük Dengeleme hizmetlerini Azure kapsamında birleştirme hakkında daha fazla bilgi [Azure'da Yük Dengeleme hizmetlerini kullanarak](../traffic-manager/traffic-manager-load-balancing-azure.md).
- Günlükleri farklı türde yönetmek ve yük dengeleyicide sorunlarını gidermek için nasıl kullanacağınızı öğrenin [Azure İzleyici, Azure Load Balancer için günlükleri](../load-balancer/load-balancer-monitor-log.md).
