---
title: Azure web uygulaması güvenlik duvarı için ön kapı Azure nedir? (Önizleme)
description: Azure ön kapısı hizmeti web uygulamalarınızı kötü amaçlı saldırılara karşı korur. Azure web uygulaması Güvenlik Duvarı hakkında bilgi edinin.
services: frontdoor
documentationcenter: ''
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/02/2019
ms.author: kumud;tyao
ms.openlocfilehash: 17cf6629aca6c73bc96e4cf0c172a2e87a7aafb8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61459354"
---
# <a name="what-is-azure-web-application-firewall-for-azure-front-door-preview"></a>Azure web uygulaması güvenlik duvarı için ön kapı Azure nedir? (Önizleme)

Azure web uygulaması Güvenlik Duvarı (WAF) (Önizleme), küresel olarak Azure ön kapısı kullanılarak teslim edilir, web uygulamalarınız için merkezi koruma sağlar. Tasarlanmış ve web hizmetlerinizi açıklardan yararlanmaya ve güvenlik açıklarına karşı koruyun ve uyumluluk gereksinimlerini karşılamanıza yardımcı olacak ek olarak, kullanıcılar için yüksek oranda kullanılabilir, hizmetinize tutmak için işletilir.

> [!IMPORTANT]
> Azure web uygulaması Güvenlik Duvarı (WAF) için Azure ön kapısı, şu anda genel Önizleme aşamasındadır.
> Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanmaktadır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Web uygulamaları hizmet sel, SQL ekleme saldırıları ve siteler arası betik saldırıları engelleme gibi kötü amaçlı saldırılar giderek hedeflerdir. Bu kötü amaçlı saldırıları, hizmet kesintisi ve veri kaybına neden, web uygulama sahipleri için önemli bir tehdit teşkil.

Uygulama kodunda bu tür saldırıların önlenmesi zor olabilir ve uygulama topolojisinin birden fazla katmanında ayrıntılı bakım, düzeltme eki uygulama ve izleme işlemleri gerektirebilir. Merkezi bir web uygulaması güvenlik duvarı, güvenlik yönetimini çok daha kolay hale getirir ve yetkisiz erişim ya da izinsiz giriş tehditlerine karşı uygulama yöneticilerine daha iyi güvence verir. Ayrıca, WAF çözümü bilinen bir güvenlik açığına yerine tek tek web uygulamalarının her birinin güvenliğini sağlama merkezi bir konumda düzeltme eki uygulayarak güvenlik tehdidine daha hızlı tepki verebilir.

WAF ön kapı için genel ve merkezi bir çözümdür. Azure ağ edge konumlardaki dünya genelinde dağıtılan ve her gelen istek ön kapısı tarafından sunulan, WAF özellikli bir web uygulaması için ağ ucunda inceledi. Bu, sanal ağınıza girmeden önce saldırı kaynakları yakın kötü amaçlı saldırıları önlemek WAF sağlar ve performanstan ödün vermeden uygun ölçekte genel koruma sunar. Bir WAF ilke kolayca, aboneliğinizdeki herhangi bir ön kapısı profil bağlanabilir ve yeni kurallar tehdit desenleri değişen hızlı yanıt vermesini sağlayan dakika içinde dağıtılabilir.

![Azure web uygulaması güvenlik duvarı](./media/waf-overview/web-application-firewall-overview.png)

Azure WAF uygulama ağ geçidiyle de etkinleştirilebilir. Daha fazla bilgi için [Web uygulaması güvenlik duvarı](../application-gateway/waf-overview.md).

## <a name="waf-policy-and-rules"></a>WAF ilke ve kurallara

Bir WAF ilkesi yapılandırın ve bu ilkeyi bir veya daha fazla ön kapısı ön uçları için koruma ilişkilendirin. Bir WAF İlkesi, güvenlik kuralları iki türden oluşur:
- Müşteri tarafından yazılan özel kurallar.
- Azure tarafından yönetilen önceden yapılandırılmış bir kural kümesinin koleksiyonudur yönetilen kural kümeleri. Her ikisi de mevcut olduğunda özel kurallar, yönetilen kural kümesi içinde kuralları yürütmeden önce yürütülür. Bir kuralı bir eşleşme koşulu, öncelik ve eylem yapılır. Desteklenen eylem türleri şunlardır: İzin verme, ENGELLEME, günlük ve yeniden yönlendirme. Yönetilen ve özel kuralları birleştirerek belirli bir uygulama koruma gereksinimlerinizi karşılayan tamamen özelleştirilmiş bir ilke oluşturabilirsiniz.

Bir ilke kuralların önceliğini kural yürütme sırasını tanımlar benzersiz bir tamsayı olduğu öncelikli bir sırada yürütülür. Daha küçük bir tamsayı değeri daha yüksek bir önceliği gösterir ve bu daha yüksek bir tamsayı değeri olan kurallardan önce değerlendirilir. Bir kural eşleştiğinde, kuralda tanımlanan karşılık gelen eylem isteği uygulanır. Böyle bir eşleşme işlendikten sonra daha düşük öncelikleri kurallarıyla daha da işlenmez.

Ön kapısı tarafından sunulan bir web uygulaması, kendisiyle ilişkili bir kerede yalnızca bir WAF ilkeniz olabilir. Ancak, kendisiyle ilişkilendirilmiş herhangi bir WAF ilkeleri olmadan bir ön kapısı yapılandırması olabilir. Bir WAF ilke varsa, tüm dünya genelinde güvenlik ilkelerindeki tutarlılık sağlamak için sunduğumuz edge konumları çoğaltılır.

## <a name="waf-modes"></a>WAF Modları

WAF İlkesi, aşağıdaki iki modda çalışacak şekilde yapılandırılabilir:

- **Algılama modu:** Algılama modunda çalıştırdığınızda, WAF izleyiciler dışındaki diğer tüm eylemler almaz ve istek ve kendi eşleşen WAF kural WAF günlüklerine kaydeder. Günlük tanılamalarının ön kapısı kapatabilirsiniz (portal kullanırken bu giderek gerçekleştirilebilir **tanılama** Azure portalında bölümü).

- **Önleme modu:** Önleme modunda çalışacak şekilde yapılandırıldığında, WAF bir istek, bir kural ile eşleşen ve bir eşleşme bulunursa, daha düşük önceliğe sahip başka hiçbir kural değerlendirilir belirtilen eylemi gerçekleştirir. Eşleşen tüm istekleri de WAF günlüklerine kaydedilir.

## <a name="waf-actions"></a>WAF eylemleri

WAF müşteriler, bir isteğin bir kuralın koşulları eşleştiğinde eylemlerden birini çalıştırmayı da seçebilirsiniz:
- **İzin ver:**  İstek, WAF geçirir ve arka uca yönlendirilir. Düşük öncelikli kurallar, bu isteği başka engelleyebilirsiniz.
- **Engelleme:** İstek engellenir ve isteği arka uç için iletme olmadan WAF istemcisine bir yanıt gönderir.
- **Günlük:**  İstek WAF günlüklerine kaydedilir ve daha düşük öncelik kuralları değerlendirilirken WAF devam eder.
- **Yeniden yönlendirme:** WAF, belirtilen URI'ye isteği yönlendirir. Belirtilen ilke düzeyi ayarı URI'dir. Yapılandırıldıktan sonra eşleşen tüm istekleri **yeniden yönlendirme** eylem bu URI'yi gönderilir.


## <a name="waf-rules"></a>WAF kurallarını

Bir WAF İlkesi iki tür güvenlik kuralları - özel kurallar, yönetilen rulesets ve müşteri tarafından yazılan oluşabilir Azure tarafından yönetilen önceden yapılandırılmış kuralları kümesi.

### <a name="custom-authored-rules"></a>Yazılan özel kurallar
Özel kurallar WAF aşağıdaki gibi yapılandırabilirsiniz:

- **IP listesi ve Engellenenler listesine izin ver:** Web uygulamalarınızı bir istemci IP adresi veya IP adresi aralıkları listesine göre erişimi denetlemek için özel kurallar yapılandırabilirsiniz. IPv4 ve IPv6 adres türleri desteklenir. Bu liste, engellemek veya kaynak IP, bir IP listesinde eşleştiği isteklere izin vermek için yapılandırılabilir.

- **Coğrafi tabanlı erişim denetimi:** Web uygulamalarınızı, istemcinin IP adresi ile ilişkili ülke kodu göre erişimi denetlemek için özel kurallar yapılandırabilirsiniz.

- **HTTP parametre tabanlı erişim denetimi:** Sorgu dizeleri, POST args, istek URI'si, istek üst bilgisi ve istek gövdesi gibi HTTP/HTTPS isteği parametreleriyle eşleşen dize temel özel kuralları yapılandırabilirsiniz.

- **Metot tabanlı erişim denetimi iste:** GET, PUT veya baş gibi isteğin HTTP istek yöntemine göre özel kurallar yapılandırabilirsiniz.

- **Boyutu kısıtlaması:** Bir isteğin sorgu dizesi, URI gibi belirli kısımlarını uzunluklarının temel özel kuralları yapılandırabilirsiniz ya da istek gövdesi.

- **Kuralları sınırlama oranı:** Oranı denetimi kuralı, tüm istemci IP olağandışı yüksek trafik çalıştırmanız gerekir. Bir istemci IP, bir dakikalık süre izin verilen web isteklerinin sayısı bir eşiği yapılandırabilirsiniz. Bu, tüm veya tüm istek bir istemci IP blokları sağlar ya da bir IP listesi tabanlı izin verme veya engelleme özel kural'den farklıdır. Oran sınırlandırma için ayrıntılı fiyat denetimi eşleşen HTTP (S) parametreleri gibi ek eşleştirme koşulları ile birleştirilebilir.


### <a name="azure-managed-rule-sets"></a>Azure tarafından yönetilen kural kümeleri

Azure tarafından yönetilen kural kümeleri ortak bir dizi güvenlik tehditlerine karşı koruma dağıtmak için kolay bir yol sağlar. Bu tür rulesets Azure tarafından yönetilen bu yana kuralları yeni saldırı imzalarını karşı korumak için gerektiği şekilde güncelleştirilir. Genel Önizleme aşamasında Azure tarafından yönetilen varsayılan kural kümesi kurallara göre aşağıdaki tehdit kategorileri içerir:

- Siteler arası betik
- Java saldırıları
- Yerel dosya ekleme
- PHP ekleme saldırılarına karşı
- Uzaktan komut yürütme
- Uzak dosya ekleme
- Oturum sabitleme
- SQL ekleme koruması

Kural kümesi için yeni saldırı imzalarını eklendiğinde varsayılan kural kümesi sürüm numarasını artırır.
Varsayılan kural kümesi, ilkelerinizi WAF algılama modunda varsayılan olarak etkindir. Varsayılan kural, uygulama gereksinimlerinizi karşılamak için kümesi içinde bireysel kuralları etkinleştirmek veya devre dışı. Kural başına belirli eylemleri (İzin Ver/Engelle/yeniden yönlendirme/günlüğü) da ayarlayabilirsiniz. Varsayılan eylemin taşıdır. Ayrıca, varsayılan kural kümesi'ndeki önceden yapılandırılmış kurallardan herhangi birine geçmek istiyorsanız özel kurallar aynı WAF ilkesinde yapılandırılabilir.
Özel kurallar, varsayılan kural kümesi kurallarında değerlendirilmeden önce her zaman uygulanır. Bir isteği bir özel kural eşleşirse, karşılık gelen kural eylemi uygulanır ve istek engellendi veya herhangi bir başka özel kuralın veya kural varsayılan kural kümesi'ndeki olmadan arka uca geçirilen aracılığıyla. Ayrıca, WAF ilkelerinizi varsayılan kural kümesi kaldırma seçeneği vardır.


## <a name="configuration"></a>Yapılandırma
Genel Önizleme sırasında:
- REST API'leri, Azure Resource Manager şablonları ve Azure PowerShell kullanarak yapılandırma ve dağıtma tüm WAF kural türlerini tam olarak desteklenir.
- Azure portalını kullanarak, yalnızca Azure tarafından yönetilen varsayılan kural kümesi görüntülemek veya yapılandırabilirsiniz.

## <a name="monitoring"></a>İzleme

Waf ön Kapıda izleme uyarıları izleyin ve trafiği eğilimleri daha kolay izlemek için Azure İzleyici ile tümleşiktir.

## <a name="pricing"></a>Fiyatlandırma

Ön kapı için WAF ile ilişkili tüm kullanımlar, genel Önizleme boyunca ücretsizdir ve Ücret alınmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.

