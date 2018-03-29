---
title: Azure içinde birden fazla IP yapılandırması dengelemesini | Microsoft Docs
description: Birincil ve ikincil IP yapılandırmaları yükdengeleme
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: na
ms.assetid: 244907cd-b275-4494-aaf7-dcfc4d93edfe
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: fcd9ff8b726b5dc3e0d447bc384dbcc7cc1a4e88
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="load-balancing-on-multiple-ip-configurations-by-using-the-azure-portal"></a>Birden çok IP yapılandırmalarını Azure portalını kullanarak yük dengelemenin

> [!div class="op_single_selector"]
> * [Portal](load-balancer-multiple-ip.md)
> * [PowerShell](load-balancer-multiple-ip-powershell.md)
> * [CLI](load-balancer-multiple-ip-cli.md)


Bu makalede, bir ikincil ağ arabirimi denetleyicisinde (NIC) birden çok IP adresleriyle Azure yük dengeleyici kullanmayı göstermek için yapacağız. Aşağıdaki diyagramda Senaryomuzda gösterilmektedir:

![Yük dengeleyici senaryosu](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

Senaryomuzda biz aşağıdaki yapılandırma kullanıyorsanız:

- İki sanal Windows çalışan sanal makineleri (VM'ler).
- Her VM birincil ve ikincil bir NIC'e sahip.
- Her ikincil NIC iki IP yapılandırmaları vardır.
- Her VM iki Web sitesi barındırır: contoso.com ve fabrikam.com.
- Her Web sitesi ikincil NIC üzerinde bir IP yapılandırmasına bağlıdır
- Azure yük dengeleyici iki ön uç IP adresi, her Web sitesi için bir tane göstermek için kullanılır. Ön uç adreslerini her Web sitesi için ilgili IP yapılandırması için trafiği dağıtmak için kullanılır.
- Aynı bağlantı noktası, ön uç IP adresleri ve arka uç havuzu IP adresleri için kullanılır.

## <a name="prerequisites"></a>Önkoşullar

Senaryo Örneğimizde, bir kaynak grubu sahibi olduğunuzu varsayar **contosofabrikam** , aşağıdaki gibi yapılandırılmış:

- Adlı bir sanal ağ kaynak grubu içeren **myVNet**.
- **MyVNet** ağ içerir adlı iki VM **VM1** ve **VM2**.
- VM1 ve VM2 olan adlandırılmış kümesi kullanılabilirlik **myAvailset**. 
- VM1 ve VM2 sahip adlı birincil NIC **VM1NIC1** ve **VM2NIC1**sırasıyla. 
- VM1 ve VM2 sahip adlı ikincil bir NIC **VM1NIC2** ve **VM2NIC2**sırasıyla.

Birden çok NIC VM'ler oluşturma hakkında daha fazla bilgi için bkz: [PowerShell kullanarak birden çok NIC ile VM oluşturma](../virtual-machines/windows/multiple-nics.md).

## <a name="perform-load-balancing-on-multiple-ip-configurations"></a>Yük Dengeleme birden çok IP yapılandırmalarını gerçekleştirme

Bu makalede açıklanan senaryo elde etmek için aşağıdaki adımları tamamlayın.

### <a name="step-1-configure-the-secondary-nics"></a>1. adım: ikincil NIC'ler yapılandırma

Sanal ağınızdaki her VM için ikincil NIC IP yapılandırmasını ekleyin:  

1. Azure portalına göz atın: http://portal.azure.com. Azure hesabınızla oturum açın.

2. Ekranın üst sol seçin **kaynak grubu** simgesi. Ardından, VM'lerin bulunduğu kaynak grubunu seçin (örneğin, **contosofabrikam**). **Kaynak grupları** bölmesinde tüm kaynakları ve NIC VM'ler için görüntüler.

3. Her VM ikincil NIC IP yapılandırmasını ekleyin:

    1. Yapılandırmak istediğiniz ikincil NIC seçin.
    
    2. Seçin **IP yapılandırmaları**. Üstüne yakın sonraki bölmesinde seçin **Ekle**.

    3. Altında **eklemek IP yapılandırmaları**, ikinci bir IP yapılandırması NIC'ye ekleyin: 

        1. İkincil IP yapılandırması için bir ad girin. (Örneğin, IP yapılandırması VM1 ve VM2 için ad **VM1NIC2 ipconfig2** ve **VM2NIC2 ipconfig2**sırasıyla.)

        2. İçin **özel IP adresi**, **ayırma** ayarını seçin **statik**.

        3. **Tamam**’ı seçin.

İkincil NIC tamamlandıktan sonra ikinci IP yapılandırması onu altında görüntülenen **IP yapılandırmaları** verilen NIC için ayarları

### <a name="step-2-create-the-load-balancer"></a>2. Adım: Yük dengeleyiciyi oluşturun

Yük Dengeleyici yapılandırması için oluşturun:

1. Azure portalına göz atın: http://portal.azure.com. Azure hesabınızla oturum açın.

2. Ekranın üst sol seçin **kaynak oluşturma** > **ağ** > **yük dengeleyici**. Ardından, **oluşturma**.

3. Altında **oluşturma yük dengeleyici**, yük dengeleyici için bir ad yazın. Bu senaryoda, biz adı kullandığınızdan **mylb**.

4. Altında **genel IP adresi**, adlı yeni bir genel IP oluşturun **PublicIP1**.

5. Altında **kaynak grubu**, Vm'leriniz için varolan bir kaynak grubu seçin (örneğin, **contosofabrikam**). Yük Dengeleyici ile dağıtın ve ardından konumu seçin **Tamam**.

Yük Dengeleyici dağıtmayı başlatır. Dağıtım başarılı bir şekilde tamamlanması birkaç dakika sürebilir. Dağıtım tamamlandıktan sonra Yük Dengeleyici bir kaynak, kaynak grubu olarak görüntülenir.

### <a name="step-3-configure-the-front-end-ip-pool"></a>3. adım: ön uç IP havuzu yapılandırma

Her Web sitesi için (contoso.com ve fabrikam.com), ön uç IP havuzu yük dengeleyicide yapılandırın:

1. Portalı'nda seçin **daha fazla hizmet**. Filtre kutusuna **genel IP adresi** ve ardından **ortak IP adresleri**. Üstüne yakın sonraki bölmesinde seçin **Ekle**.

2. İki ortak IP adreslerini yapılandırın (**PublicIP1** ve **PublicIP2**) (contoso.com ve fabrikam.com) her iki Web siteleri için:

    1. Ön uç IP adresi için bir ad yazın.

    2. İçin **kaynak grubu**, Vm'leriniz için varolan bir kaynak grubu seçin (örneğin, **contosofabrikam**).

    3. İçin **konumu**, VM'ler aynı konumu seçin.

    4. **Tamam**’ı seçin.

    Genel IP adresleri oluşturulduktan sonra altında görüntülendikleri **genel IP** adresleri.

3. <a name="step3-3"></a>Portalı'nda seçin **daha fazla hizmet**. Filtre kutusuna **yük dengeleyici** ve ardından **yük dengeleyici**. 

4. Yük Dengeleyici seçin (**mylb**) ön uç IP havuzuna eklemek istediğiniz.

5. Altında **ayarları**seçin **ön uç havuzları**. Üstüne yakın sonraki bölmesinde seçin **Ekle**.

6. Ön uç IP adresi için bir ad yazın (örneğin, **contosofe** veya **fabrikamfe**).

7. <a name="step3-7"></a>Seçin **IP adresi**. Altında **seçin genel IP adresi**, ön uç IP adresi seçin (**PublicIP1** veya **PublicIP2**).

8. İkinci ön uç IP adresi tekrarlayarak oluşturma <a href="#step3-3">3. adım</a> aracılığıyla <a href="#step3-7">adım 7</a> bu bölümdeki.

Ön uç havuzu yapılandırıldıktan sonra IP adresleri, yük dengeleyici altında görüntülenen **ön uç IP havuzu** ayarlar. 
    
### <a name="step-4-configure-the-back-end-pool"></a>Adım 4: arka uç havuzunu yapılandırma

Her Web sitesi için (contoso.com ve fabrikam.com), arka uç adres havuzu, yük dengeleyicide yapılandırın:
        
1. Portalı'nda seçin **daha fazla hizmet**. Filtre kutusuna **yük dengeleyici** ve ardından **yük dengeleyici**.

2. Yük Dengeleyici seçin (**mylb**) arka uç havuzuna eklemek istediğiniz.

3. Altında **ayarları**seçin **arka uç havuzları**. Arka uç havuzu için bir ad yazın (örneğin, **contosopool** veya **fabrikampool**). Üstüne yakın sonraki bölmesinde seçin **Ekle**. 

4. İçin **ilişkili**seçin **kullanılabilirlik kümesi**.

5. İçin **kullanılabilirlik kümesi**seçin **myAvailset**.

6. Her iki VM için hedef ağ IP yapılandırmaları ekleyin: 

    ![Yük Dengeleyici için arka uç havuzlarını yapılandırın.](./media/load-balancer-multiple-ip/lb-backendpool.PNG)
    
    1. İçin **hedef sanal makine**, arka uç havuzuna eklemek istediğiniz VM seçin (örneğin, **VM1** veya **VM2**).

    2. İçin **ağ IP yapılandırması**, önceki adımda seçtiğiniz VM için ikincil NIC IP yapılandırmasını seçin (örneğin, **VM1NIC2 ipconfig2** veya **VM2NIC2 ipconfig2** ).

7. **Tamam**’ı seçin.

Arka uç havuzu yapılandırıldıktan sonra adresler, yük dengeleyici altında görüntülenen **arka uç havuzu** ayarlar.

### <a name="step-5-configure-the-health-probe"></a>5. adım: sistem durumu araştırmasını yapılandırma

Yük Dengeleyici için bir sistem durumu araştırma yapılandırın:

1. Portalı'nda seçin **daha fazla hizmet**. Filtre kutusuna **yük dengeleyici** ve ardından **yük dengeleyici**.

2. Yük Dengeleyici seçin (**mylb**) sistem durumu araştırma eklemek istediğiniz.

3. Altında **ayarları**seçin **durumu araştırması**. Üstüne yakın sonraki bölmesinde seçin **Ekle**. 

4. Sistem durumu araştırması için bir ad yazın (örneğin, **HTTP**). **Tamam**’ı seçin.

### <a name="step-6-configure-load-balancing-rules"></a>6. adım: Yük Dengeleme kuralları yapılandırma

Her Web sitesi için (contoso.com ve fabrikam.com), Yük Dengeleme kuralları yapılandırın:
    
1. <a name="step6-1"></a>Altında **ayarları**seçin **durumu araştırması**. Üstüne yakın sonraki bölmesinde seçin **Ekle**. 

2. İçin **adı**, Yük Dengeleme kuralı için bir ad yazın (örneğin, **HTTPc** contoso.com, veya **HTTPf** fabrikam.com için).

3. İçin **ön uç IP adresi**, daha önce oluşturduğunuz ön uç IP adresi seçin (örneğin, **contosofe** veya **fabrikamfe**).

4. İçin **bağlantı noktası** ve **arka uç bağlantı noktası**, varsayılan değer tutmak **80**.

5. İçin **kayan IP (doğrudan sunucu dönüşü)**seçin **etkin**.

6. <a name="step6-6"></a>Seçin **Tamam**.

7. İkinci yük dengeleyici kuralı tekrarlayarak oluşturmak <a href="#step6-1">1. adım</a> aracılığıyla <a href="#step6-6">adım 6</a> bu bölümdeki.

Kuralları yapılandırıldıktan sonra Yük Dengeleyici altında görüntülenen **Yük Dengeleme kuralları** ayarlar.

### <a name="step-7-configure-dns-records"></a>7. adım: DNS kayıtlarını Yapılandır

Son adım olarak, DNS kaynak kayıtları, yük dengeleyici için ilgili ön uç IP adreslerine işaret edecek şekilde yapılandırın. Azure DNS'de etki alanlarınızı barındırabilir. Azure DNS yük dengeleyici ile kullanma hakkında daha fazla bilgi için bkz: [kullanarak Azure DNS diğer Azure hizmetleriyle](../dns/dns-for-azure-services.md).

## <a name="next-steps"></a>Sonraki adımlar
- Yük Dengeleme Azure hizmetlerinde birleştirme hakkında daha fazla bilgi edinin [Azure'da Yük Dengeleme hizmetlerini kullanarak](../traffic-manager/traffic-manager-load-balancing-azure.md).
- Farklı türde günlüklerini yönetmek ve yük dengeleyici sorunlarını gidermek için nasıl kullanabileceğinizi öğrenin [analytics Azure yük dengeleyici için oturum](../load-balancer/load-balancer-monitor-log.md).
