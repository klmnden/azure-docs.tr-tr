---
title: "Azure Portalı - Uygulama ağ geçidi oluşturma | Microsoft Docs"
description: "Portalı kullanarak bir uygulama ağ geçidi oluşturmayı öğrenin"
services: application-gateway
documentationcenter: na
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 54dffe95-d802-4f86-9e2e-293f49bd1e06
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: davidmu
ms.openlocfilehash: 17d09ce98c40717d1db0927f791a7c97ea7835e0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-application-gateway-with-the-portal"></a>Portal ile bir uygulama ağ geçidi oluşturma

[Uygulama ağ geçidi](application-gateway-introduction.md) olan çeşitli katman 7 Yük Dengeleme, uygulamanız için sunumu bir hizmet olarak uygulama teslim Denetleyicisi'ni (ADC) sağlayan ayrılmış bir sanal uygulama. Bu makalede bir uygulamayı Azure Portalı'nı kullanarak ve arka uç üye olarak var olan bir sunucu ekleme ağ geçidi oluşturmak için adım alır.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Oturum açtığınızda Azure portalında [http://portal.azure.com](http://portal.azure.com)

## <a name="create-application-gateway"></a>Uygulama ağ geçidi oluşturma

Bir uygulama ağ geçidi oluşturmak için izlediği adımları tamamlayın. Uygulama ağ geçidi, kendi alt gerektirir. Bir sanal ağ oluştururken, birden çok alt ağa sahip için yeterli adres alanı bıraktığınızdan emin olun. Uygulama ağ geçidi alt ağına dağıtıldıktan sonra yalnızca başka uygulama ağ geçitleri eklenebilir.

1. Sık Kullanılanlar portalı bölmesinde **yeni**
1. **Yeni** dikey penceresinde, **Ağ** öğesine tıklayın. İçinde **ağ** dikey penceresinde tıklatın **uygulama ağ geçidi**aşağıdaki görüntüde gösterildiği gibi:

    ![Uygulama ağ geçidi oluşturma][1]

1. İçinde **Temelleri** görünür, dikey penceresinde aşağıdaki değerleri girin ve ardından **Tamam**:

   | **Ayar** | **Değer** | **Ayrıntılar**|
   |---|---|---|
   |**Ad**|AdatumAppGateway|Uygulama ağ geçidi adı|
   |**Katmanı**|Standart|Standart ve WAF değerleri kullanılabilir. Ziyaret [web uygulaması güvenlik duvarı](application-gateway-web-application-firewall-overview.md) WAF hakkında daha fazla bilgi edinmek için.|
   |**SKU boyutu**|Orta|Standart katmanı seçildiğinde küçük, Orta ve büyük seçimlerdir. WAF katmanı seçerken, Orta ve büyük yalnızca seçeneklerdir.|
   |**Örnek sayısı**|2|Uygulama ağ geçidi yüksek kullanılabilirlik için örnek sayısı. Örnek sayısı 1 yalnızca sınama amacıyla kullanılmalıdır.|
   |**Abonelik**|[Aboneliğiniz]|Uygulama ağ geçidinin oluşturulacağı bir abonelik seçin.|
   |**Kaynak grubu**|**Yeni Oluştur:** AdatumAppGatewayRG|Bir kaynak grubu oluşturun. Kaynak grubu adı, seçili abonelik içinde benzersiz olmalıdır. Kaynak grupları hakkında daha fazla bilgi için, [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#resource-groups)’a genel bakış makalesini okuyun.|
   |**Konum**|Batı ABD||

1. İçinde **ayarları** altında görüntülenen dikey **sanal ağ**, tıklatın **sanal ağ seçin**. **Seç sanal ağ** dikey pencere açılır.  Tıklatın **Yeni Oluştur** açmak için **sanal ağ oluştur** dikey.

   ![sanal ağ seçin][2]

1. Üzerinde **oluşturma sanal ağ dikey** aşağıdaki değerleri girin ve ardından **Tamam**. **Sanal ağ oluştur** ve **Seç sanal ağ** dikey pencereleri kapatın. Bu adım doldurur **alt** alanını **ayarları** dikey penceresinde seçilen alt ağ ile.

   | **Ayar** | **Değer** | **Ayrıntılar**|
   |---|---|---|
   |**Ad**|AdatumAppGatewayVNET|Uygulama ağ geçidi adı|
   |**Adres alanı**|10.0.0.0/16|Bu sanal ağın adres alanı olduğu|
   |**Alt ağ adı**|AppGatewaySubnet|Uygulama ağ geçidi için alt ağ adı|
   |**Alt ağ adres aralığı**|10.0.0.0/28|Bu alt ağ daha fazla ek alt ağlar sanal ağında arka uç havuzu üyeleri için izin verir|

1. Üzerinde **ayarları** altında dikey **ön uç IP yapılandırmasını**, seçin **ortak** olarak **IP adresi türü**

1. Üzerinde **ayarları** altında dikey **genel IP adresi** tıklatın **genel bir IP adresi seçin**, **genel IP adresi seçin** dikey penceresi açıldığında, tıklatın **Yeni Oluştur**.

   ![genel IP seçin][3]

1. Üzerinde **ortak IP adresi oluştur** dikey penceresinde, varsayılan değeri kabul edin ve tıklatın **Tamam**. Dikey penceresi kapanır ve dolduran **genel IP adresi** seçilen genel IP adresine sahip.

1. Üzerinde **ayarları** altında dikey **dinleyici Yapılandırması**, tıklatın **HTTP** altında **Protokolü**. Kullanılacak bağlantı noktasını girin **bağlantı noktası** alan.

2. Tıklatın **Tamam** üzerinde **ayarları** devam etmek için dikey.

1. Ayarları gözden **Özet** tıklayın ve dikey **Tamam** uygulama ağ geçidi oluşturmayı başlatmak için. Bir uygulama ağ geçidi oluşturma, uzun süre çalışan bir görevdir ve tamamlanması zaman alır.

## <a name="add-servers-to-backend-pools"></a>Arka uç havuzları sunucuları ekleme

Uygulamanızın barındıran sistemleri uygulama ağ geçidi oluşturduktan sonra Yük uygulama ağ geçidi için eklenmesi gerek hala dengeli. Arka uç adres havuzu için IP adresleri, FQDN veya bu sunucuların NIC eklenir.

### <a name="ip-address-or-fqdn"></a>IP adresi veya FQDN

1. Oluşturulan, Azure portalında uygulama ağ geçidi ile **Sık Kullanılanlar** bölmesinde tıklatın **tüm kaynakları**. Tıklatın **AdatumAppGateway** tüm kaynaklar dikey penceresinde Uygulama ağ geçidi. Zaten seçili abonelik çeşitli kaynaklar varsa, girebilirsiniz **AdatumAppGateway** içinde **ada göre Filtrele...** girebilirsiniz.

1. Oluşturduğunuz uygulama ağ geçidi görüntülenir. Tıklatın **arka uç havuzları**, geçerli arka uç havuzunu tıklatıp **appGatewayBackendPool**, **appGatewayBackendPool** dikey pencere açılır.

   ![Uygulama ağ geçidi arka uç havuzları][4]

1. Tıklatın **hedef Ekle** FQDN değerlerini IP adreslerini eklemek için. Seçin **IP adresi veya FQDN** olarak **türü** ve IP adresi veya FQDN girin. Ek arka uç havuzu üyeleri için bu adımı yineleyin. Tıklatın tamamlanınca **kaydetmek**.

### <a name="virtual-machine-and-nic"></a>Sanal makine ve NIC

Bu gibi durumlarda, sanal makine NIC de arka uç havuzu üyeleri ekleyebilirsiniz. Yalnızca uygulama ağ geçidiyle aynı sanal ağ içindeki sanal makineleri açılır kullanılabilir.

1. Oluşturulan, Azure portalında uygulama ağ geçidi ile **Sık Kullanılanlar** bölmesinde tıklatın **tüm kaynakları**. Tıklatın **AdatumAppGateway** tüm kaynaklar dikey penceresinde Uygulama ağ geçidi. Zaten seçili abonelik çeşitli kaynaklar varsa, girebilirsiniz **AdatumAppGateway** içinde **ada göre Filtrele...** girebilirsiniz.

1. Oluşturduğunuz uygulama ağ geçidi görüntülenir. Tıklatın **arka uç havuzları**, geçerli arka uç havuzunu tıklatıp **appGatewayBackendPool**, **appGatewayBackendPool** dikey pencere açılır.

   ![Uygulama ağ geçidi arka uç havuzları][5]

1. Tıklatın **hedef Ekle** FQDN değerlerini IP adreslerini eklemek için. Seçin **sanal makine** olarak **türü** ve kullanmak için NIC ve sanal makine seçin. Tıklatın tamamlanınca **Kaydet**

   > [!NOTE]
   > Yalnızca uygulama ağ geçidiyle aynı sanal ağdaki sanal makinelerden açılan kutuda mevcuttur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, kaynak grubu, uygulama ağ geçidi ve tüm ilişkili kaynakları silin. Bunu yapmak için uygulama ağ geçidi dikey penceresinden kaynak grubunu seçin ve **silmek**.

## <a name="next-steps"></a>Sonraki adımlar

Bu senaryoda, bir uygulama ağ geçidi dağıtılmış ve bir sunucu için arka uç eklenir. Uygulama ağ geçidi ayarlarını değiştirme ve ayarlama kuralları tarafından ağ geçidi yapılandırmak için sonraki adımlar şunlardır. Bu adımları, aşağıdaki makaleler ziyaret ederek bulunabilir:

Özel sistem durumu araştırmalarının ziyaret ederek oluşturmayı öğrenin [bir özel durum araştırması oluştur](application-gateway-create-probe-portal.md)

SSL boşaltma yapılandırmak ve web sunucularınızın kapalı maliyetli SSL şifre çözme ziyaret ederek ele öğrenin [SSL boşaltma yapılandırın](application-gateway-ssl-portal.md)

Uygulamalarınızı korumaya öğrenin [Web uygulaması güvenlik duvarı](application-gateway-webapplicationfirewall-overview.md) uygulama ağ geçidi özelliğidir.

<!--Image references-->
[1]: ./media/application-gateway-create-gateway-portal/figure1.png
[2]: ./media/application-gateway-create-gateway-portal/figure2.png
[3]: ./media/application-gateway-create-gateway-portal/figure3.png
[4]: ./media/application-gateway-create-gateway-portal/figure4.png
[5]: ./media/application-gateway-create-gateway-portal/figure5.png
[scenario]: ./media/application-gateway-create-gateway-portal/scenario.png
