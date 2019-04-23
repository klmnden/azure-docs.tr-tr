---
title: Azure ön kapısı hizmet arka uçlar ve arka uç havuzlarına | Microsoft Docs
description: Bu makalede, hangi arka uçlar ve arka uç havuzları kapı yapılandırma önde olan açıklanır.
services: front-door
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 543e237a4a8390a8ebf74d0eb2a1f4be41dcd911
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60000598"
---
# <a name="backends-and-backend-pools-in-azure-front-door-service"></a>Azure ön kapısı hizmetinde arka uçlar ve arka uç havuzları
Bu makalede, Azure ön kapısı hizmet uygulaması dağıtımınızı eşlemek hakkında kavramlarını açıklar. Ayrıca, geçici uygulama arka uçları ön kapısı yapılandırmasında farklı koşullar da açıklanır.

## <a name="backends"></a>Arka Uçlar
Bir arka uç bir bölgede bir uygulamanın dağıtım örneği eşittir. Ön kapısı hizmeti, hem Azure hem de Azure olmayan arka uçları, bölge yalnızca Azure bölgeler ile kısıtlı değildir şekilde destekler. Ayrıca, şirket içi veri merkezinizde veya başka bir bulut uygulama örneğinde olabilir.

Konak adı veya istemci isteklerine hizmet verebilen uygulamanızın genel IP için ön kapı hizmet arka uçları bakın. Arka uçları, veritabanı katmanı, depolama katmanı ve benzeri yanıltıcı olmamalıdır. Arka uçları uygulama arka ucunuzu genel uç noktası olarak görüntülenmesi. Bir arka uç bir ön kapısı arka uç havuzuna eklediğinizde, ayrıca aşağıdakileri eklemeniz gerekir:

- **Arka uç ana bilgisayar türü**. Eklemek istediğiniz kaynak türü. Otomatik bulma, app service, bulut hizmeti ya da depolama, uygulama arka uçları ön kapısı hizmeti destekler. Azure'da veya Azure dışı arka farklı bir kaynak istiyorsanız belirleyin **özel konak**.

    >[!IMPORTANT]
    >Arka uca ön ortamlardan erişilemez durumdaysa yapılandırması sırasında API'leri doğrulamaz. Ön ucunuzu erişebildiğinden emin olun.

- **Abonelik ve arka uç ana bilgisayar adı**. Henüz seçtiyseniz **özel konak** arka uç ana bilgisayar türü için uygun aboneliği ve karşılık gelen arka uç ana bilgisayar adı kullanıcı Arabiriminde seçerek arka ucunuzu seçin.

- **Arka uç ana bilgisayar üstbilgisi**. Her istek için bir arka uca gönderilen ana bilgisayar üstbilgisi değeri. Daha fazla bilgi için [arka uç ana bilgisayar üstbilgisi](#hostheader).

- **Öncelik**. Tüm trafiği için bir birincil hizmet arka ucu kullanmak istediğinizde farklı uçlarınıza öncelikler atayın. Ayrıca, birincil veya yedek arka uçları kullanılamıyorsa yedeklemeler sağlar. Daha fazla bilgi için [öncelik](front-door-routing-methods.md#priority).

- **Ağırlık**. Eşit ya da ağırlığı katsayıları göre bir dizi arka uçları trafiği dağıtmak için farklı uçlarınıza ağırlıkları atayın. Daha fazla bilgi için [ağırlıkları](front-door-routing-methods.md#weighted).

### <a name = "hostheader"></a>Arka uç ana bilgisayar üstbilgisi

Arka uca ön kapısı tarafından iletilen istekler hedef kaynak almak için arka uç kullanan bir konak üstbilgisi alanı içerir. Bu alan için değer, genellikle arka uç URI gelir ve konak ve bağlantı noktası vardır.

Örneğin, www için yapılan bir istek\.contoso.com, ana bilgisayar üst bilgisi www olacaktır\.contoso.com. Arka ucunuzu yapılandırmak için Azure portalını kullanıyorsanız, bu alan için varsayılan değer arka uç ana bilgisayar adıdır. Azure portalında contoso-westus.azurewebsites.net arka uç ise arka uç ana bilgisayar üstbilgisi girdiğinizde değeri contoso westus.azurewebsites.net olacaktır. Açıkça bu alanı ayarlamadan, Azure Resource Manager şablonları ya da başka bir yöntem kullanmanız durumunda, ancak ön kapı hizmet gelen konak adı değeri olarak için barındırma üstbilgisini gönderir. İstek için www yapıldıysa\.contoso.com ve arka uç ise bir boş üstbilgi alanı olan contoso westus.azurewebsites.net, ön kapısı hizmet www ana bilgisayar üst bilgisini ayarlar\.contoso.com.

Çoğu uygulama arka uçları (Azure Web Apps, Blob Depolama ve Cloud Services) arka uç etki alanını eşleştirmek için ana bilgisayar üst bilgisi gerektirir. Ancak arka ucunuza yönlendiren frontend ana bilgisayar farklı bir ana bilgisayar adı www gibi kullanırsınız\.contoso.azurefd.net.

Arka uç arka uç ana bilgisayar adı ile eşleşmesi için ana bilgisayar üstbilgisi gerektiriyorsa, arka uç ana bilgisayar üstbilgisi'nın ana bilgisayar adı arka uç içerdiğinden emin olun.

#### <a name="configuring-the-backend-host-header-for-the-backend"></a>Arka uç ana bilgisayar üstbilgisi arka uç için yapılandırma

Yapılandırmak için **arka uç ana bilgisayar üstbilgisi** alan için bir arka uç arka uç havuzu bölümünde:

1. Ön kapısı kaynağınızı açın ve yapılandırmak için arka ucu ile arka uç havuzunu seçin.

2. Bu işlemi yapmadıysanız ya da mevcut bir düzen, bir arka uç ekleyin.

3. Arka uç ana üstbilgi alanı özel bir değere ayarlamak veya boş bırakın. Gelen istek için ana bilgisayar adı, ana bilgisayar üstbilgisi değeri kullanılır.

## <a name="backend-pools"></a>Arka uç havuzları
Arka uç havuzu önde kapı hizmet uygulamalarına yönelik benzer trafiğinin yönlendirildiği arka uçları kümesini ifade eder. Diğer bir deyişle, aynı trafiği almak ve beklenen davranışlar ile yanıt veren dünya genelinde uygulama örneklerinizi mantıksal bir gruplandırması olan. Bu arka uçları farklı bölgeler arasında ya da aynı bölgede dağıtılır. Tüm arka uçlar, dağıtım modu aktif/aktif veya Aktif/Pasif yapılandırmayı olarak tanımlanmış olabilir.

Arka uç havuzu farklı arka uçları sistem durumu araştırmaları nasıl değerlendirilmesi gerektiğini tanımlar. Ayrıca, nasıl Yük Dengeleme aralarında gerçekleştiğinden tanımlar.

### <a name="health-probes"></a>Sistem durumu araştırmaları
Ön kapısı hizmeti düzenli aralıklarla HTTP/HTTPS araştırma isteklerine her yapılandırılmış uçlarınıza gönderir. Araştırma isteklerine yakınlık ve yüklenecek her bir arka uç sistem durumu, son kullanıcı isteklerini Bakiye belirleyin. Arka uç havuzu için sistem durumu araştırması ayarları nasıl biz uygulama arka uçları sistem durumunu yoklamak tanımlayın. Aşağıdaki ayarlar kullanılabilir yük dengeleyici yapılandırması:

- **Yol**. Araştırma istekleri arka uç havuzundaki tüm arka uçlar için kullanılan URL. Örneğin, contoso westus.azurewebsites.net uçlarınıza biridir ve yolu için /probe/test.aspx ayarlanır, ardından Protokolü HTTP için ayarlanmış olduğu varsayılırsa, ön kapısı Service ortamları sistem durumu araştırma isteklerine http gönderir\:/ / contoso-westus.azurewebsites.net/probe/test.aspx.

- **Protokol**. Sistem durumu araştırma isteklerine uçlarınıza HTTP veya HTTPS protokolü için ön kapı hizmetinden gönderilip gönderilmeyeceğini tanımlar.

- **Aralık (saniye)**. Sistem durumu araştırmaları sıklığını uçlarınıza veya ön kapısı ortamların her birinde bir araştırma gönderir aralıklarını tanımlar.

    >[!NOTE]
    >Daha hızlı yük devretme işlemleri için aralığı daha düşük bir değere ayarlayın. Düşük değeri, yüksek sistem durumu araştırması birim uçlarınıza alırsınız. Aralığı 30 saniye 90 ön kapısı ortamları veya POP'ları ile bir genel olarak ayarlanırsa, örneğin, her arka uç hakkında alırsınız saniyede 3-5 yoklama istek.

Daha fazla bilgi için [sistem durumu araştırmalarının](front-door-health-probes.md).

### <a name="load-balancing-settings"></a>Yük Dengeleme ayarları
Yük Dengeleyici arka uç havuzu ayarlarını nasıl biz sistem durumu araştırmaları değerlendirmek tanımlayın. Bu ayarlar, arka uç sağlam olup olmadığını belirler. Ayrıca onlar iade arka uç havuzundaki farklı arka uçlar arasında trafik yükünü dengele öğreneceksiniz. Aşağıdaki ayarlar kullanılabilir yük dengeleyici yapılandırması:

- **Örnek boyutu**. Arka uç sistem durumu değerlendirme için göz önünde bulundurmanız gereken sistem durumu araştırmaları kaç örnekleri biz tanımlar.

- **Başarılı örnek boyutu**. Daha önce belirtildiği gibi örnek boyutu arka uç sağlıklı çağırmanın başarılı örneklerin sayısını tanımlar. Örneğin, bir ön kapısı sistem durumu araştırma aralığı 30 saniyedir, örnek boyutu 5'tir ve başarılı örnek boyutu 3 varsayalım. Arka ucunuz için biz durumunu değerlendirme her zaman araştırmaları, 150 saniye (5 x 30) son beş samples umuyoruz. En az üç başarılı araştırmaları, arka uç sağlıklı olarak bildirmek için gereklidir.

- **Gecikme süresi duyarlılık (ek gecikme)**. Arka uçları gecikme ölçüm Duyarlılık aralık içinde istek göndermek veya yakın arka uç isteği iletmek için ön kapı isteyip istemediğinizi tanımlar.

Daha fazla bilgi için [en az gecikme göre yönlendirme yöntemini](front-door-routing-methods.md#latency).

## <a name="next-steps"></a>Sonraki adımlar

- [Bir ön kapısı profili oluşturma](quickstart-create-front-door.md)
- [Ön kapısı işleyişi](front-door-routing-architecture.md)