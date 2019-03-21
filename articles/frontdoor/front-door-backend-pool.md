---
title: Azure ön kapısı hizmeti - arka uçlar ve arka uç havuzları | Microsoft Docs
description: Bu makale, arka uç ve arka uç havuzları için ön kapı yapılandırmasında ne olduğunu anlamanıza yardımcı olur.
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
ms.openlocfilehash: 228ed5c54a382db7b47d19adacf9e5db398c53ae
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58123700"
---
# <a name="backends-and-backend-pools-in-azure-front-door-service"></a>Azure ön kapısı hizmetinde arka uçlar ve arka uç havuzları
Bu makalede, uygulama dağıtımınızı ön kapısı ile nasıl eşleştiği ilgili farklı kavramlar açıklanır. Biz de etrafında bir uygulama arka kapı yapılandırma önde farklı koşullar anlamı açıklanmaktadır.

## <a name="backend-pool"></a>Arka uç havuzu
Front Door hizmetinde arka uç havuzu, uygulamaları için aynı trafik türünü alabilecek eşdeğer arka uçlardan oluşan bir kümeyi temsil eder. Başka bir deyişle aynı trafiği alabilecek ve beklenen davranışla yanıt verebilecek olan dünya genelindeki uygulama örneklerinin mantıksal olarak gruplandırılmasıdır. Bu arka uçları, genellikle aynı bölge içinde veya farklı bölgeler arasında dağıtılır. Ayrıca, bu arka uçları, tüm etkin-etkin dağıtım modunda olabilir veya aksi halde bir Aktif/Pasif yapılandırmayı olarak tanımlanabilir.

Arka uç havuzu da nasıl farklı arka uçları tüm sistem durumu araştırmaları aracılığıyla kendi sistem durumu için değerlendirilmesi ve gelenlere arka uçlar arasında yük dengelemeyi ne olacağını tanımlar.

### <a name="health-probes"></a>Sistem durumu araştırmaları
Ön kapısı düzenli HTTP/HTTPS araştırma isteklerine her yapılandırılmış uçlarınıza yakınlık ve yüklenecek her bir arka uç sistem durumu, son kullanıcı isteklerini Bakiye belirlemek için gönderir. Arka uç havuzu için sistem durumu araştırması ayarları nasıl biz, uygulama arka uçları için sistem durumu için yoklama tanımlayın. Yük Dengeleme için yapılandırma için aşağıdaki ayarlar kullanılabilir:

1. **Yol**: Burada araştırma ister URL yolu arka uç havuzundaki tüm arka uçlar için gönderilir. Örneğin, arka uçlar ise `contoso-westus.azurewebsites.net` ve yolunu ayarlamak `/probe/test.aspx`, Protokolü HTTP için ayarlanmış olduğu varsayılırsa, ön kapısı ortamları sistem durumu araştırma isteklerine gönderecektir http://contoso-westus.azurewebsites.net/probe/test.aspx. 
2. **Protokol**: HTTP veya HTTPS protokolü üzerinden, arka uçları ön kapısı gelen sistem durumu araştırması istekleri gönderilip gönderilmeyeceğini tanımlar.
3. **Aralık (saniye)**: Bu alan, arka uçları ön kapısı ortamların her birinde bir araştırma gönderir aralıkları başka bir deyişle, sistem durumu araştırmaları sıklığını tanımlar. Daha hızlı yük devretmeyi arıyorsanız Not - Bu alan düşük bir değere ayarlayın. Ancak, düşük değer daha fazla araştırma uçlarınıza alacak birim. Uçlarda ön kapısı oluşturacak araştırma hacmini fikir almak için bir örneği ele alalım. Şimdi varsayalım, aralığı 30 saniye olarak ayarlanır ve yaklaşık 90 ön kapısı ortamları veya yoksa POP genel. Her uçlarınıza yaklaşık hakkında alacak şekilde 3-5 saniye başına yoklama istek.

Okuma [sistem durumu araştırmaları](front-door-health-probes.md) Ayrıntılar için.

### <a name="load-balancing-settings"></a>Yük Dengeleme ayarları
Arka uç havuzu için yük dengeleme ayarlarını nasıl biz sağlam olması için arka uç karar için sistem durumu araştırmaları değerlendirmek ve ayrıca nasıl trafiği arka uç havuzundaki farklı arka uçlar arasında Yük Dengelemesi ihtiyacımız tanımlayın. Yük Dengeleme için yapılandırma için aşağıdaki ayarlar kullanılabilir:

1. **Örnek boyutu**: Bu özellik, arka uç sistem durumu değerlendirme için göz önünde bulundurmanız gereken sistem durumu araştırmaları kaç örnekleri biz tanımlar.
2. **Başarılı örnek boyutu**: Bu özellik, 'örnek boyutu', yukarıda açıklandığı gibi kaç tane örnek arka uç sağlıklı olarak çağırmak başarılı olup olmadığını denetlemek ihtiyacımız olduğunu tanımlar. 
</br>Örneğin, uygulamanızın ön kapısı durum yoklaması ayarladığınız varsayalım *aralığı* 30 saniye, *örnek boyutu* '5'e ayarlanır ve *başarılı örnek boyutu* '3'e ayarlanır. Sonra ne bu yapılandırma her seferinde sizi arka uç için sistem durumu araştırmaları değerlendirmek, biz son 150 saniye kapsayan son beş örnekler görüneceğini anlamına gelir (5 * 30 = s) ve olmadığı sürece bu araştırmaların 3 veya başarılı geri bildirimini sağlıksız bitiş olayı. Diyelim ki yalnızca iki başarılı araştırmaları vardı ve bu nedenle biz arka uç sistem durumu kötü olarak işaretler. Biz 3 son beş araştırmaları, başarılı bulursanız, biz değerlendirme bir sonraki çalıştırmanızda ardından biz sağlıklı olarak arka uç yeniden işaretleyin.
3. **Gecikme süresi duyarlılık (ek gecikme)**: İstek gecikme süresi ölçüm ve isteği arka uca en yakın açısından Duyarlılık aralık içinde olan arka uçları göndermek için ön kapı isteyip istemediğinizi gecikme duyarlılık alanı tanımlar. Okuma [en az gecikme göre yönlendirme yöntemini](front-door-routing-methods.md#latency) ön kapısı, daha fazla bilgi edinmek için.

## <a name="backend"></a>Arka uç
Bir arka uç bir bölgede bir uygulamanın dağıtım örnek eşdeğerdir. Hem Azure hem de Azure olmayan arka uçları ön kapısı destekler ve bu nedenle burada bölge yalnızca Azure bölgeler ile kısıtlı değildir ancak şirket içi veri merkezinizde veya diğer bir bulutta bir uygulama örneği de olabilir.

Arka uçları, konak adı veya istemci isteklerine hizmet verebilir, uygulamanızın genel IP için ön kapı bağlamında ifade eder. Bu nedenle, arka uçları, veritabanı katmanı veya depolama katmanınızı vb. ile karıştırılmamalıdır ancak bunun yerine uygulama arka ucunuzu genel uç noktası olarak görülmelidir.

Bir arka uç, ön kapısı, bir arka uç havuzuna eklediğinizde, aşağıdaki ayrıntıları doldurun gerekir:

1. **Arka uç ana bilgisayar türü**: Eklemek istediğiniz kaynak türü. Ön kapısı, app service, bulut hizmeti ya da depolama, otomatik olarak bulunması, uygulama arka uçları, destekler. Azure'da veya Azure dışı arka farklı bir kaynak istiyorsanız 'Özel konak' seçin. -Yapılandırma sırasında API'leri doğrulamaz, bunun yerine, arka uç tarafından ön kapısı erişilebildiğini emin olmak gereken arka uç ön ortamlardan erişilebilir olup olmadığını unutmayın. 
2. **Abonelik ve arka uç ana bilgisayar adı**: Değil seçtiyseniz 'Özel konak' arka uç ana yazın ve ardından uygun aboneliği ve karşılık gelen arka uç ana bilgisayar adı kullanıcı arabiriminden seçerek arka ucunuzu seçin ve aşağı kapsam gerekir.
3. **Arka uç ana bilgisayar üstbilgisi**: Her istek için bir arka uca gönderilen ana bilgisayar üstbilgisi değeri. Okuma [arka uç ana bilgisayar üstbilgisi](#hostheader) Ayrıntılar için.
4. **Öncelik**: Tüm trafiği için bir birincil hizmet arka ucu kullanın ve birincil veya yedek arka uçları kullanılamaz olması durumunda yedekleme sağlamak istediğinizde, farklı uçlarınıza öncelikleri atayabilirsiniz. Daha fazla bilgi edinin [öncelik](front-door-routing-methods.md#priority).
5. **Ağırlık**: Eşit ya da ağırlığı katsayıları göre bir dizi arka uçları trafiği dağıtmak istediğinizde farklı uçlarınıza ağırlıkları atayabilirsiniz. Daha fazla bilgi edinin [ağırlıkları](front-door-routing-methods.md#weighted).


### <a name = "hostheader"></a>Arka uç ana bilgisayar üstbilgisi

Arka uca ön kapısı tarafından iletilen istekler hedef kaynak almak için arka uç kullanan bir konak üstbilgisi alanı var. Bu alan için değer, genellikle arka uç URI gelir ve konak ve bağlantı noktası vardır. Örneğin, bir istek için yapılan `www.contoso.com` barındırma üst bilgisi gerekir `www.contoso.com`. Azure portalını kullanarak arka uç yapılandırıyorsanız, bu alan için doldurulur varsayılan değer arka uç ana bilgisayar adı olan. Örneğin, arka uç ise `contoso-westus.azurewebsites.net`, sonra Azure portalında arka uç ana bilgisayar üstbilgisi otomatik olarak doldurulan değeri `contoso-westus.azurewebsites.net`. 
</br>Ancak, Resource Manager şablonları kullanıyorsanız veya başka bir mekanizma ve bu alan açıkça ayarlıyorsanız değil, ardından ön kapısı gelen ana bilgisayar adı olarak ana bilgisayar üstbilgisi değeri gönderir. Örneğin, istek yapıldıysa `www.contoso.com`, ve arka uç `contoso-westus.azurewebsites.net` boş olarak arka uç ana üstbilgi alanı içeren ön kapısı barındırma üst bilgisi olarak ayarlayacak `www.contoso.com`.

Çoğu uygulama arka uçları (örneğin, Web Apps, Blob Depolama ve Cloud Services) arka uç etki alanını eşleştirmek için ana bilgisayar üst bilgisi gerektirir. Ancak arka ucunuza yönlendiren frontend ana bilgisayar www gibi farklı bir ana bilgisayar adı olacaktır\.contoso.azurefd.net. Ayarladığınız arka uç, arka uç ana bilgisayar adını eşleştirmek için ana bilgisayar üstbilgisi gerektiriyorsa, 'arka uç ana bilgisayar üstbilgisi' da arka uç ana bilgisayar adı olduğundan emin olmalısınız.

#### <a name="configuring-the-backend-host-header-for-the-backend"></a>Arka uç ana bilgisayar üstbilgisi arka uç için yapılandırma
'Arka uç ana bilgisayar üstbilgisi' alanı, bir arka uç, arka uç havuzu için yapılandırılabilir.

1. Ön kapısı kaynağınızı açın ve yapılandırılması için arka ucu olan arka uç havuzuna tıklayın.

2. Eklenen tüm değil ya da mevcut bir düzen, bir arka uç ekleyin. 'Arka uç ana bilgisayar üstbilgisi' alanı özel bir değere ayarlamak veya boş bırakılmış konağı gelen istek için ana üst bilgi değeri kullanılacak anlamına gelir.



## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.