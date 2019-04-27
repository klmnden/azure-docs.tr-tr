---
title: Azure ön kapısı hizmeti - trafik yönlendirme yöntemleri | Microsoft Docs
description: Bu makale ön kapısı tarafından kullanılan farklı trafik yönlendirme yöntemleri anlamanıza yardımcı olur.
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
ms.openlocfilehash: bd1278db43ba31ed78f13a826a330e16c3bc8d57
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60736233"
---
# <a name="front-door-routing-methods"></a>Ön kapısı yönlendirme yöntemleri

Azure ön kapısı hizmeti çeşitli hizmet uç noktaları, HTTP/HTTPS trafiğini yönlendirme hakkında belirlemek için çeşitli trafik yönlendirme yöntemlerini destekler. İstekleri en iyi arka uç örneğe iletilir emin olmak için her ön kapısı ulaşmasını, istemci istekleri ile yapılandırılmış bir yönlendirme yöntemini uygulanmış olur. 

Ön kapısı trafik yönlendirme için dört ana kavram vardır:

* **[Gecikme süresi](#latency):** Gecikme süresi tabanlı yönlendirme istekleri için en düşük gecikme süresi arka uçları bir duyarlılık aralığında kabul edilebilir gönderilmesini sağlar. Aslında, kullanıcı isteklerini ağ gecikmesi açısından arka uçları "en yakın" kümesini gönderilir.
* **[Öncelik](#priority):** Tüm trafiği için bir birincil hizmet arka ucu kullanın ve birincil veya yedek arka uçları kullanılamaz olması durumunda yedekleme sağlamak istediğinizde, farklı uçlarınıza öncelikleri atayabilirsiniz.
* **[Ağırlıklı](#weighted):** Eşit ya da ağırlığı katsayıları göre bir dizi arka uçları trafiği dağıtmak istediğinizde farklı uçlarınıza ağırlıkları atayabilirsiniz.
* **Oturum benzeşimi:** Oturum benzeşimi, frontend ana bilgisayar veya kullanıcı oturumu hala etkin olduğu sürece bir kullanıcıdan gelen sonraki istekleri için aynı arka uca gönderilen istediğinizde etki alanları ve arka uç örneği için yine de Sağlıklı durum yoklamalarına göre raporları yapılandırabilirsiniz. 

Tüm Front Door yapılandırmaları, arka uç durumu ve otomatik anında global yük devretme izlemesini kapsar. Daha fazla bilgi için [ön kapısı arka uç izleme](front-door-health-probes.md). Tek bir yönlendirme yöntemini devre dışı tabanlı ya da çalışma, ön kapısı yapılandırılabilir ve uygulamanızın bağlı olarak birden çok ya da tüm bu yönlendirme yöntemleri birlikte bir en uygun yönlendirme topolojisi oluşturmak için kullanabileceğiniz.

## <a name = "latency"></a>En düşük gecikme süreleriyle trafik yönlendirme tabanlı

Dünya çapında iki veya daha fazla konumu arka uçları dağıtmak, 'Son kullanıcılarınıza en yakın' konumu için trafiği yönlendirme tarafından birçok uygulama yanıt verme hızını artırabilir. Varsayılan trafik yönlendirme yöntemi için ön kapı yapılandırmanızı en yakın arka uca ön kapısı ortamından bir istek aldı son kullanıcılarınızdan gelen istekleri iletir. Anycast mimariyi Azure ön kapısı hizmetinin birleştiğinde, bu yaklaşım her kişiselleştirilmiş, son kullanıcıların get en yüksek bir performans konumlarını temel sağlar.

'En yakın' arka uç tarafından coğrafi uzaklık ölçülen mutlaka en yakın değil. Bunun yerine, ağ gecikmesi ölçerek en yakın arka uçları ön kapısı belirler. Daha fazla bilgi edinin [ön kapısı'nın yönlendirme mimarisi](front-door-routing-architecture.md). 

Genel karar akış aşağıdadır:

| Mevcut arka uçları | Öncelik | Gecikme süresi sinyal (sistem durumu araştırmaya dayalı) | Ağırlıklar |
|-------------| ----------- | ----------- | ----------- |
| İlk olarak, etkin ve iyi durumda döndürülen tüm arka uçlar seçin (200 Tamam) durum yoklaması için. Örneğin, A, B, C, D, E ve F, altı arka uçları vardır ve bunlar arasındaki C sağlıksız olduğunu ve E devre dışı bırakıldı. Mevcut arka uçları listesi A, bu nedenle, B ve D f  | Ardından, kullanılabilir olanlara arasından en yüksek önceliğiniz arka uçları seçilir. Örneğin, arka uç A, B ve D öncelik 1 varsa ve arka uç F bir öncelik 2. Seçilen arka uçlar ve böylece, B ve d| Arka uçları gecikme aralığıyla (en az gecikme süresi ve belirtilen MS gecikme duyarlılık) seçin. Söyleyin 15 MS'den A, B 30 ms olan ve D, a en yakın bir arka uç uzağa 30 ms ötesinde olduğundan burada isteği geldiğimizi, 30 ms gecikme duyarlılığı olan sonra arka ucunu A ve B, düşük gecikme süresi havuzu oluşur ön kapısı ortamı uzağa 60 ms D ise | Son olarak, ön kapısı olacak hepsini bir kez deneme trafiğini belirtilen ağırlıkları oranını arka uçları son seçilen havuzu arasında. Örneğin, bir arka uç varsa 8 ağırlığını 5 ve arka uç B ağırlığı olan ve ardından trafik 5:8 arka uçları A ve b arasında oranı olarak dağıtılmış |

>[!NOTE]
> Varsayılan olarak, gecikme süresi duyarlılık özelliği için 0 ms, yani ayarlanır, her zaman isteği hızlı kullanılabilir arka uca iletir.


## <a name = "priority"></a>Öncelik tabanlı trafik yönlendirme

Genellikle bir kuruluş, birincil hizmet arıza durumunda bir veya daha fazla yedekleme hizmetlerini dağıtarak hizmetlerinin için güvenilirlik sağlamak istiyor. Endüstri bu topoloji ayrıca için Etkin/Beklemede veya dağıtım topolojisi Aktif/Pasif olarak adlandırılır. Azure müşterileri kolayca bu yük devretme deseni uygulamak 'Öncelik' trafik yönlendirme yöntemi sağlar.

Varsayılan ön kapısı arka uçları bir önceliği eşittir listesini içerir. Yalnızca en yüksek önceliğiniz arka uçları (öncelik için en düşük değer) için varsayılan olarak, ön kapısı trafiğini gönderdiği arka uçları diğer bir deyişle, birincil kümesi. Birincil arka uçları kullanılabilir durumda değilse, ön kapısı arka uçları (öncelik için ikinci en düşük değer) ikincil kümesini trafiği yönlendirir. Birincil ve ikincil arka kullanılabilir durumda değilse, trafiği üçüncü ve benzeri gider. Arka uç kullanılabilirlik (etkin veya devre dışı) yapılandırılmış durumuna bağlıdır ve sistem tarafından belirlenen sürekli bir arka uç sistem durumu araştırmaları.

### <a name="configuring-priority-for-backends"></a>Arka uçları için öncelik yapılandırma

Her arka uçları ön kapısı yapılandırması içindeki arka uç havuzu, 1 ile 5 arasında bir sayı olabilir ' öncelik' adlı bir özelliğe sahiptir. Azure ön kapısı hizmetiyle, bu özellik için her bir arka uç kullanarak açıkça arka uç önceliği yapılandırın. Bu özellik, 1 ile 5 arasında bir değerdir. Düşük değerler daha yüksek önceliği temsil eder. Arka uçları öncelik değerleri paylaşabilirsiniz.

## <a name = "weighted"></a>Ağırlıklı trafik yönlendirme yöntemi
'Ağırlıklı' trafik yönlendirme yöntemini trafiği eşit olarak dağıtmak için veya önceden tanımlanmış ağırlığı kullanmayı sağlar.

Ağırlıklı trafik yönlendirme yöntemini her bir arka uca ön kapısı yapılandırmasında arka uç havuzunuzun ağırlık atayın. Ağırlık 1-1000 arasında bir tamsayıdır. Bu parametre '50' varsayılan ağırlığı kullanır.

Kabul edilen gecikme duyarlılık (belirtildiği gibi) içinde mevcut arka uçları listesini arasında trafiği belirtilen ağırlıkları oranını hepsini bir kez deneme mekanizması dağıtılmış. Daha sonra gecikme duyarlılık milisaniye 0 olarak ayarlanırsa, iki arka uçlar aynı ağ gecikme süresine sahip olmadığı sürece bu özellik geçerli olmaz. 

Ağırlıklı yöntemin bazı yararlı senaryolara olanak tanır:

* **Aşamalı uygulama yükseltmesi**: Yeni bir arka ucuna yönlendirmek için trafik yüzdesi ayırın ve yavaş yavaş, diğer arka uçları ile par getirmek için zaman içinde trafik artırın.
* **Uygulamayı azure'a**: Arka uç havuzu ile hem Azure hem de dış arka uçlar oluşturun. Yeni arka uçların tercih edilmesi arka uçları ağırlığını ayarlayın. Kademeli olarak bunu devre dışı, yeni arka uçların sahip olmakla başlatılıyor sonra bunları en düşük ağırlıkları yavaş çoğu trafik olduğu aldıkları düzeylere artmaya atayarak ayarlayabilirsiniz. Ardından son olarak daha az tercih edilen arka uçları devre dışı bırakma ve bunları havuzdan kaldırılıyor.  
* **Ek kapasite için bulut Patlaması**: Hızlı bir şekilde ön kapısı yerleştirerek bir şirket içi dağıtım buluta genişletin. Bulutta ek kapasiteye ihtiyaç duyduğunuzda, ekleyin veya daha fazla arka uçları etkinleştirebilir ve her arka uca trafik hangi kısmını gider belirtin.

## <a name = "affinity"></a>Oturum benzeşimi
Varsayılan olarak olmadan oturum benzeşimi, özellikle farklı arka uçları gecikmeleri değiştikçe veya Dengeleme yapılandırması yüküne göre farklı arka uçları aynı istemciden gelen istekler ön kapısı iletir farklı istekleri aynı kullanıcı farklı bir ön kapısı ortamında Toprakları. Ancak belirli diğer senaryolardaki bazı durum bilgisi olan uygulamalar, aynı kullanıcıdan gelen ardışık isteklerin ilk isteği işleyen aynı arka uca yönlendirilmesini tercih etmektedir. Tanımlama bilgilerine dayalı oturum benzeşimi özelliği, bir kullanıcı oturumunu aynı arka uçta tutmak istediğinizde kullanışlıdır. Ön kapısı yönetilen tanımlama bilgilerini kullanarak Azure ön kapısı hizmeti bir kullanıcı oturumundan sonraki trafiği arka uç sağlıklı olduğunu ve kullanıcı oturumunun süresi dolmadığından sürece işlemek için aynı arka uca yönlendirebilir. 

Oturum benzeşimi, yapılandırılan etki alanlarınızın (veya alt etki alanlarınızın) her biri için ön uç konağı seviyesinde etkinleştirilebilir. Bu özellik etkinleştirildikten sonra Front Door kullanıcı oturumuna bir tanımlama bilgisi ekler. Front Door, tanımlama bilgisi tabanlı oturum benzeşimi sayesinde aynı IP adresini kullanan farklı kullanıcıları dahi tanımlayabilir ve bu sayede trafiğin farklı arka uçlarınız arasında daha eşit bir şekilde dağıtılmasını sağlayabilir.

Front Door şu an için yalnızca oturum tanımlama bilgisini desteklediğinden tanımlama bilgisinin ömrü kullanıcı oturumunun ömrüyle aynıdır. 

> [!NOTE]
> Ortak Ara sunucuları ile oturum benzeşimi etkileyebilir. Bir oturumu yanıt tanımlama bilgileri aynı kaynak isteyen diğer istemcilerin kesintiye gibi önbelleğe kaydedilemeyen ise, hangi yapılamaz yanıt olarak bir oturum benzeşimi tanımlama bilgisini eklemek ön kapı gerektirdiğinden budur. Buna karşı korunmak için oturum benzeşimi olacak **değil** oluşturulması bu çalışırken arka uç önbelleğe alınabilir bir yanıt gönderirse. Oturum zaten kurulmuşsa, arka uç yanıtı önbelleğe ise bir önemi yoktur.
> Oturum benzeşimi aşağıdaki durumlarda kurulacak **sürece** yanıtı bir HTTP 304 durum koduna sahip:
> - Yanıt için ayarlanmış belirli değerlere sahip ```Cache-Control``` , "özel" gibi önbelleğe alma işlemlerini Önle üst bilgi veya mağaza içi ".
> - Yanıt içeren bir ```Authorization``` süresi dolmadı başlığı.
> - Yanıtı bir HTTP 302 durum koduna sahip.

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
