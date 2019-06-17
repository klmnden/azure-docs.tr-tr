---
title: Azure Güvenlik Merkezi'nde, ağ kaynakları koruma | Microsoft Docs
description: Bu belge adresleri yardımcı önerilerini Azure Güvenlik Merkezi'ne Azure ağı kaynaklarınızı koruma ve güvenlik ilkelerine uygun haberdar olun.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 96c55a02-afd6-478b-9c1f-039528f3dea0
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/05/2019
ms.author: v-mohabe
ms.openlocfilehash: 6b3cef32cf79c2448d2e254e27c332e01ea83c62
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66428369"
---
# <a name="protect-your-network-resources-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde ağ kaynaklarınızı koruma
Azure Güvenlik Merkezi, ağ güvenliği için en iyi uygulamalar, Azure kaynaklarınızın güvenlik durumunu sürekli olarak analiz eder. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde sağlamlaştırmak ve kaynaklarınızı korumak için gerekli denetimlerin yapılandırılması işlemi boyunca size rehberlik öneriler oluşturur.

Bu makalede bir ağ güvenlik açısından Azure kaynaklarınıza uygulama önerileri ele alır. Önerileri merkezi sonraki generation güvenlik duvarları, ağ güvenlik grupları, JIT VM erişimi aşırı İhtiyari gelen trafik kuralları ve geçici ağ. Ağ önerileri ve düzeltme eylemlerinin bir listesi için bkz. [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md).

> [!NOTE]
> **Ağ** sayfa, Azure kaynak durumu hakkında ayrıntılı bir inceleme ağ açısından sağlar. Uyarlamalı ağ denetimleri ve ağ eşlemesi Azure Güvenlik Merkezi standart katmanı için yalnızca kullanılabilir. [Ücretsiz katmanı kullanırsanız, düğmeyi tıklatabilirsiniz **eski ağ iletişimini görüntüle** ve ağ kaynağı önerileri almak](#legacy-networking).
>

**Ağ** sayfası için ayrıntılı bölümlere genel bir bakış sağlar derinlerine, ağ kaynaklarınızın sağlığı hakkında daha fazla bilgi edinin:

- Ağ eşlemesi (yalnızca Azure Güvenlik Merkezi standart katmanı)
- Uyarlamalı Ağ Sağlamlaştırma
- Ağ güvenlik önerileri.
- Eski **ağ** dikey (önceki ağ dikey) 
 
![Ağ bölmesi](./media/security-center-network-recommendations/networking-pane.png)

## <a name="network-map"></a>Ağ eşlemesi
Etkileşimli ağ eşlemesi, ağ kaynaklarını sağlamlaştırmak için öneriler ve öngörüleri veren güvenlik katmanları ile bir grafik görünümü sağlar. Haritayı kullanarak Azure iş yüklerinizi, sanal makinelerinizi ve alt ağları ve haritadaki belirli kaynaklar ve bu kaynaklar için önerileri incelemek için özellik arasında bağlantılar, ağ topolojisini görebilirsiniz.

Ağ eşlemesi'ni açmak için:

1. Güvenlik Merkezi, kaynak güvenlik sağlığı seçin **ağ**.
2. Altında **ağ eşlemesi** tıklayın **bkz topolojisi**.
 
Bir topoloji Haritası varsayılan görünümünü görüntüler:
- Azure'da seçili abonelikler. Eşleme, birden çok abonelik destekler.
- VM'ler, alt ağlar ve sanal ağları Resource Manager kaynak türü (Klasik Azure kaynakları desteklenmez)
- Eşlenmiş sanal ağlar
- Sahip kaynakları [ağ önerileri](security-center-recommendations.md) Orta veya yüksek önem derecesi  
- İnternet'e yönelik kaynaklar
- Harita, Azure'da seçili abonelikler için optimize edilmiştir. Seçiminizi değiştirirseniz, haritayı yeniden hesaplanması yeniden en iyi duruma getirilmiş ve yeni kendi ayarlarınızı temel alan.  

![Ağ topolojisi haritası](./media/security-center-network-recommendations/network-map-info.png)

## <a name="understanding-the-network-map"></a>Ağ eşlemesini anlama

Ağ eşlemesi, Azure kaynaklarınızı gösterebilirsiniz bir **topolojisi** görünümü ve bir **trafiği** görünümü.

### <a name="the-topology-view"></a>Topoloji görünümü

İçinde **topolojisi** görünümü ağ haritasını, ağ kaynakları hakkında aşağıdaki bilgileri görüntüleyebilirsiniz:
- İç daire içinde seçili aboneliklerinizi tüm Vnet'lerin görebilirsiniz, sonraki daire tüm alt ağları, dış daire tüm sanal makineler.
- Eşlem içindeki kaynaklarına bağlanma satırları hangi kaynakların birbiriyle ilişkilendirilir ve Azure ağınız nasıl yapılandırıldığını bildiren olanak tanır. 
- Önem derecesi göstergeleri ve kaynakların açık öneriler Güvenlik Merkezi'nden sahip bir genel bakış için hızlıca kullanın.
- Bunları detaya gitme ve bu kaynağın ayrıntılarını görüntülemek için kaynaklardan herhangi birini tıklatın ve öneriler doğrudan ve ağ bağlamında eşleyin.  
- En kritik durumdaki ve en yüksek önem derecesi önerileri kaynakları vurgulama kaynaklarınız varsa haritada görüntülenmesini kaynakları, Azure Güvenlik Merkezi akıllı için kendi özel algoritmasını kullanır. çok fazla küme. 

Haritanın etkileşimli ve dinamik olduğundan, her düğüm tıklanabilir ve görünümü değiştirebilirsiniz filtrelere göre:

1. Ağ haritanın en üstünde filtreleri kullanarak gördüğünüz değiştirebilirsiniz. Harita üzerinde odaklanın:
   -  **Güvenlik durumu**: Harita önem derecesine, Azure kaynaklarınızın (yüksek, Orta, düşük) göre filtreleyebilirsiniz.
   - **Öneriler**: Hangi önerileri üzerinde bu kaynakları etkin olmasına göre hangi kaynakların görüntülenen seçebilirsiniz. Örneğin, yalnızca ağ güvenlik gruplarını etkinleştirmek için Güvenlik Merkezi önerir kaynakları görüntüleyebilirsiniz.
   - **Ağ alanları**: Varsayılan olarak, iç VM'ler de seçebilir, harita yalnızca Internet'e yönelik kaynakları görüntüler.
 
2. Tıklayabilirsiniz **sıfırlama** sol üst köşedeki eşleme varsayılan durumuna geri döndürmek için dilediğiniz zaman içinde.

Bir kaynağa detaya gitmek için:
1. Harita üzerinde belirli bir kaynak seçin, sağ bölmede açılır ve kaynak varsa, bağlı güvenlik çözümleri hakkında genel bilgiler verir ve kaynağa ilgili öneriler. Bu davranış her seçtiğiniz kaynak türü için aynı türüdür. 
2. Eşlem içindeki bir düğüm üzerine geldiğinizde, abonelik, kaynak türü ve kaynak grubu da dahil olmak üzere kaynak hakkında genel bilgileri görüntüleyebilirsiniz.
3. Araç İpucu yakınlaştırma ve haritayı belirli bir düğümde yeniden odaklanılmasına bağlantıyı kullanın. 
4. Haritayı belirli bir düğümün uzağa yeniden odaklanılmasına için uzaklaştırabilirsiniz.

### <a name="the-traffic-view"></a>Trafik görünümü

**Trafiği** görünümü ile kaynaklarınızı arasındaki olası tüm trafik haritasını, sağlar. Bu görsel haritasını, yapılandırılmış olan tüm kuralların sizinle birlikte hangi kaynakların iletişim kurabilir tanımlamak sağlar. Bu yanı sıra var olan ağ güvenlik gruplarının yapılandırmasını görmek mümkün riskli yapılandırmaları içinde iş yüklerinizi hızlıca tanımlamanıza olanak sağlar.

### <a name="uncover-unwanted-connections"></a>İstenmeyen bağlantılar ortaya çıkarın

Bu görünüm gücünü, kullanabilmeniz için izin verilen bu bağlantıların birlikte var, güvenlik açıklarını göstermek için bu Kesiti veri kaynaklarınız üzerinde gerekli sağlamlaştırılmasını olmasıdır. 

Örneğin, uyumlu olmayan iki makine iletişim kurulamadı, alt ağlar ve iş yükleri için daha ayrıntılı etkinleştirme algılamanıza.

### <a name="investigate-resources"></a>Kaynakları araştırın

Bir kaynağa detaya gitmek için:
1. Harita üzerinde belirli bir kaynak seçin, sağ bölmede açılır ve kaynak varsa, bağlı güvenlik çözümleri hakkında genel bilgiler verir ve kaynağa ilgili öneriler. Bu davranış her seçtiğiniz kaynak türü için aynı türüdür. 
2. Tıklayın **trafiği** kaynakta - olası giden ve gelen trafik listesini görmek için bu kapsamlı kimin kaynak ile iletişim kurabilir ve ile ve hangi protokoller ve bağlantı noktaları üzerinden iletişim kurabilir listesidir. Örneğin, seçtiğinizde bir VM, ile iletişim kurabilir tüm sanal makineler gösterilir ve bir alt ağ seçtiğinizde ile iletişim kurabilir, tüm alt ağlar gösterilir.

**Bu veriler ağ güvenlik grupları yanı sıra gelişmiş makine öğrenimi, kendi crossovers ve etkileşimleri anlamak için birden çok kural çözümleyen ve algoritma Analizine temel alır.** 

![Ağ trafiği eşleme](./media/security-center-network-recommendations/network-map-traffic.png)

## Eski ağ <a name ="legacy-networking"></a>

Bu bölümde, Güvenlik Merkezi standart katmanı yoksa, ücretsiz ağ önerileri görüntüleme açıklanmaktadır.

Ağ dikey penceresinde bu bilgilere erişmek için tıklayın **eski ağ iletişimini görüntüle**. 

![Eski ağ](./media/security-center-network-recommendations/legacy-networking.png)

### <a name="internet-facing-endpoints-section"></a>İnternet'e yönelik uç noktalar bölümü
İçinde **Internet'e yönelik uç noktalar** bölümünde, uç nokta ve durumu Internet'e yönelik ile yapılandırılmış sanal makineleri görebilirsiniz.

Bu tabloda, uç nokta adı, Internet'e yönelik IP adresi ve ağ güvenlik grubunun ve NGFW'nun önerileri geçerli önem derecesi durumu bulunur. Tablo, önem derecesine göre sıralanır.

### <a name="networking-topology-section"></a>Ağ topolojisi bölümü
**Ağ topolojisi** bölümünde kaynakların hiyerarşik bir görünümü bulunur.

Bu tablo (sanal makineler ve alt ağlar) önem derecesine göre sıralanır.

Bu topoloji görünümünde ilk düzeyde sanal ağlar görüntüler. İkinci görüntülediğine alt ağlar ve bu alt ağlara ait sanal makineler üçüncü düzeyini görüntüler. Sağ sütunda bu kaynaklar için ağ güvenlik grubu önerileri geçerli durumunu gösterir.

Üçüncü düzey daha önce açıklanana için benzer sanal makineler, görüntülenir. Daha fazla bilgi edinmek veya gerekli güvenlik denetimini veya yapılandırmasını uygulamak için herhangi bir kaynağa tıklayabilirsiniz.

## <a name="network-recommendations"></a>Ağ önerileri

|Kaynak türü|Güvenlik puanı|Öneri|Açıklama|
|----|----|----|----|
|Machine|40|Sanal makineler için ağ güvenlik grupları etkinleştirilmelidir.|Ağ güvenlik grupları, sanal makinelerinizin ağ erişimini denetlemenize imkan sağlar.|
|Alt ağ|35|Ağ güvenlik grupları alt düzeydeki etkinleştirilmelidir.|Ağ güvenlik grupları, alt ağlara dağıtılan kaynakların ağ erişimini denetlemenize imkan sağlar.|
|Machine|30|Sanal makinelerde Just-In-Time ağ erişim denetimi uygulanmalıdır|Seçili bağlantı noktalarına erişim kalıcı olarak kilitlemek için yalnızca zaman VM erişimi denetimi uygulayın ve aynı mekanizması aracılığıyla ve sınırlı bir süre için açmak yetkili kullanıcıların etkinleştirin.|
|Machine|20|Internet'e yönelik uç noktalarla erişimi sınırlama|Vm'leri varolan erişimi kısıtlayarak, Internet'e yönelik ağ güvenlik gruplarını izin sağlamlaştırma kuralları.|
|Machine|10|Yeni nesil güvenlik duvarı ekleme|VM'ler, Internet'e daha iyi korunması için bir sonraki Generation Firewall (NGFW) çözüme ekleyin.|
|Machine|5|Ağ geçidi sadece güvenlik duvarı aracılığıyla trafiği yönlendirme|Sonraki nesil güvenlik duvarı çözümünüzün dağıtımını tamamlamak için korumalı internet'e yönelik Vm'leri trafiği yalnızca ileri nesil güvenlik duvarı çözüm yönlendirileceğini.|
|Sanal ağ|5|DDoS koruma standardını etkinleştir|Bu sanal ağlardaki genel IP'ler uygulamalarla, DDOS koruması hizmeti standart korunmaz. Ağ volumetric Protokolü saldırıları ve azaltma olanağı sağlamak için önerilir.|

## <a name="see-also"></a>Ayrıca bkz.
Diğer Azure kaynak türü için geçerli öneriler hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Azure Güvenlik Merkezi'nde sanal makinelerinizi koruma](security-center-virtual-machine-recommendations.md)
* [Azure Güvenlik Merkezi'nde uygulamalarınızı koruma](security-center-application-recommendations.md)
* [Azure Güvenlik Merkezi'nde Azure SQL hizmetinizi koruma](security-center-sql-service-recommendations.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
