---
title: Azure Load Balancer'a genel bakış | Microsoft Docs
description: Azure Load Balancer özellikleri, mimari ve uygulama genel bakış. Yük dengeleyicinin nasıl çalıştığını öğrenin ve bulutta yararlanın.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: load-balancer
Customer intent: As an IT administrator, I want to learn more about the Azure Load Balancer service and what I can use it for.
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/20/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 47509cd0a9208f41a52bf1a07c460bcdda2cb479
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "42056271"
---
# <a name="what-is-azure-load-balancer"></a>Azure Load Balancer nedir?

Azure Load Balancer ile uygulamalarınızı ölçeklendirmenize ve yüksek kullanılabilirlik için hizmetlerinizi oluşturun. Yük Dengeleyici, gelen ve giden senaryoları destekler, düşük gecikme süresi ve yüksek aktarım hızı sağlar ve kadar akışlar tüm TCP ve UDP uygulamaları için milyonlarca ölçeklendirir.  

Yük Dengeleyici kuralları ve sistem durumu araştırmaları göre arka uç havuzu örnekleri için load balancer'ın ön uç üzerinde geldiğinde yeni gelen akışlar dağıtır. 

Ayrıca, herkese açık yük dengeleyici genel IP adresleri için özel IP adreslerini çevirerek giden bağlantıları sanal makineler (VM) için sanal ağınızda sağlayabilir.

Azure Load Balancer iki SKU ile sunulur: temel ve standart. Ölçek, özellikleri ve fiyatlandırmayı farklılıklar vardır. Yaklaşımları biraz farklı olabilir temel yük dengeleyici ile mümkün olan her senaryoya standart Load Balancer ile de oluşturulabilir. Load Balancer hakkında bilgi edindiğiniz gibi temelleri ve SKU'ya özgü farklılıkları ile kendinizi alıştırın önemlidir.

## <a name="why-use-load-balancer"></a>Yük Dengeleyici neden kullanmalısınız? 

Azure Load Balancer için kullanabilirsiniz:

* Yük Dengeleme gelen internet trafiğini sanal makineleriniz için. Bu yapılandırma olarak bilinen bir [herkese açık yük dengeleyici](#publicloadbalancer).
* Bir sanal ağ içindeki VM'ler arasında Yük Dengeleme trafiği. Ayrıca, karma bir senaryoda, bir şirket içi ağdan bir yük dengeleyici ön ucuna ulaşabilirsiniz. Her iki senaryo olarak bilinen bir yapılandırma kullanmak bir [iç yük dengeleyici](#internalloadbalancer).
* Belirli bir bağlantı noktasına gelen ağ adresi çevirisi (NAT) kuralları özel Vm'leriyle bağlantı noktası iletme trafiği.
* Sağlamak [giden bağlantı](load-balancer-outbound-connections.md) herkese açık yük dengeleyici kullanarak sanal ağınızdaki VM'ler için.


>[!NOTE]
> Azure, senaryolarınız için tam olarak yönetilen yük dengeleme çözümleri sunar. Aktarım Katmanı Güvenliği (TLS) protokolü sonlandırma ("SSL yük boşaltma") veya HTTP/HTTPS isteği başına uygulama katmanı işleme özellikleri istiyorsanız [Application Gateway](../application-gateway/application-gateway-introduction.md)'i inceleyin. Arıyorsanız, Genel DNS için Yük Dengeleme, gözden [Traffic Manager](../traffic-manager/traffic-manager-overview.md). Uçtan uca senaryolarınızda bu çözümleri bir arada da kullanabilirsiniz.

## <a name="what-are-load-balancer-resources"></a>Yük Dengeleyici kaynakları nelerdir?

Bir yük dengeleyici kaynağını bir genel yük dengeleyiciye veya iç yük dengeleyici olarak bulunabilir. Yük Dengeleyici kaynak işlevleri, bir ön uç, bir kural, bir durum araştırması ve arka uç havuzu tanımı ifade edilir. Arka uç havuzu VM'den belirterek arka uç havuzuna VM yerleştirin.

Yük Dengeleyici kaynaklarının içinde Azure oluşturmak istediğiniz senaryoyu elde etmek için çok kiracılı altyapısını nasıl program ifade edebilirsiniz nesneleridir. Yük Dengeleyici kaynakları ve gerçek altyapınız arasında doğrudan bir ilişki yoktur. Bir yük dengeleyici oluşturmaya örneğini oluşturmaz ve kapasite her zaman kullanılabilir. 

## <a name="fundamental-load-balancer-features"></a>Temel yük dengeleyici özellikleri

Yük Dengeleyici için TCP ve UDP uygulamaları aşağıdaki temel özellikleri sağlar:

* **Yük Dengeleme**

    Azure Load Balancer ile ön uç arka uç havuzu örneklerine gelen trafiği dağıtmak için bir Yük Dengeleme kuralı oluşturabilirsiniz. Yük Dengeleyici, gelen akışlar dağıtılması için bir karma tabanlı algoritması kullanır ve arka uç havuzu örneklerine akış üstbilgileri uygun şekilde yeniden yazar. Bir sunucu, sağlam bir arka uç nokta durum araştırması gösteren yeni akışlar almak kullanılabilir.
    
    Varsayılan olarak, kullanılabilir olan sunucular için akışlar eşlemek için kaynak IP adresi, kaynak bağlantı noktası, hedef IP adresi, hedef bağlantı noktası ve IP protokol numarası oluşan bir 5 bölütlü karma yük dengeleyici kullanır. Belirli bir kaynak IP benzeşimi için belirli bir kural 2 veya 3 demet karma çıkarak oluşturmayı seçebilirsiniz. Yük dengeli bir ön uç arkasındaki aynı örneğinde aynı paket akışın tüm paketlerini ulaşır. Ne zaman istemci aynı kaynak IP, kaynak bağlantı noktası değişiklikleri yeni bir akış başlatır. Sonuç olarak, 5 demet farklı arka uç noktasına gitmek trafiği neden olabilir.

    Daha fazla bilgi için [yük dengeleyici dağıtım modu](load-balancer-distribution-mode.md). Aşağıdaki görüntüde, karma tabanlı dağıtım görüntüler:

    ![Karma tabanlı dağıtım](./media/load-balancer-overview/load-balancer-distribution.png)

    *Şekil: Karma tabanlı dağıtım*

* **Bağlantı noktası iletme**

    Load Balancer ile belirli bir bağlantı noktasına bir sanal ağ içindeki belirli bir arka uç örneğinin belirli ön uç IP adresinin belirli bir bağlantı noktasından bağlantı noktası iletme trafiği için bir gelen NAT kuralı oluşturabilirsiniz. Bu Yük Dengeleme olarak aynı olan karma tabanlı dağıtım tarafından gerçekleştirilir. Bu özellik için ortak senaryolar Azure sanal ağ içindeki tekil VM örnekleriyle Uzak Masaüstü Protokolü (RDP) veya güvenli Kabuk (SSH) oturumları verilmiştir. Aynı ön uç IP adresi üzerindeki çeşitli bağlantı noktaları için birden çok iç uç nokta eşleyebilirsiniz. Sanal makinelerinizin bir ek atlama kutusunu gerek kalmadan internet üzerinden uzaktan kullanabilirsiniz.

* **Uygulama dilden bağımsız ve saydam**

    Yük Dengeleyici doğrudan TCP veya UDP veya uygulama katmanı ve tüm TCP ile etkileşime girmez veya UDP uygulama senaryosuna desteklenebilir.  Yük Dengeleyici değil, sona veya akışlar kaynaklanan ve etkileşim akış yükü hiçbir uygulama katmanı ağ geçidi işlevi sağlar ve protokolü el sıkışmaları her zaman meydana doğrudan istemci ve arka uç havuzu örnek arasında.  Gelen bir akış yanıt her zaman bir sanal makineden bir yanıt olan.  Akış sanal makinede geldiğinde, özgün kaynak IP adresini de korunur.  Birkaç örnek daha fazla saydamlık göstermek için:
    - Her uç nokta yalnızca bir VM tarafından yanıt verdi.  Örneğin, bir TCP el sıkışması her zaman istemci ve seçilen arka uç VM arasında gerçekleşir.  Arka uç VM tarafından oluşturulan yanıt ön uç için bir isteğe yanıt olmadığı. Bir ön uç bağlantısı başarıyla doğruladığınızda en az bir arka uç sanal makine için uçtan uca bağlantıyı doğrulama.
    - Uygulama yükü Yük Dengeleyiciyi ve tüm UDP için saydam veya TCP uygulama desteklenebilir. HTTP istek işleme veya uygulama katmanı yüklerini işlenmesini başına gerektiren iş yükleri için (örneğin, HTTP URL'lerini ayrıştırma) gibi katman 7 yük dengeleyici kullanması gereken [Application Gateway](https://azure.microsoft.com/services/application-gateway).
    - Yük Dengeleyici için TCP yükünü belirsiz olduğundan ve TLS boşaltabilirsiniz ("SSL") sağlanmazsa, Load Balancer'ı kullanarak uçtan uca şifrelenmiş senaryoları oluşturun ve VM üzerindeki TLS bağlantısını sonlandırarak TLS uygulamalar için büyük ölçeklendirme elde edebilirsiniz.  Örneğin, TLS oturum anahtarlama kapasitenizi yalnızca arka uç havuzuna eklediğiniz sanal makinelerin sayısını ve türünü sınırlıdır.  "SSL yük boşaltma", uygulama katmanı işleme ya da Azure sertifika yönetimi temsilcisi istiyorsanız kullanmanız gerekiyorsa, Azure'nın katman 7 yük dengeleyici kullanması gereken [Application Gateway](https://azure.microsoft.com/services/application-gateway) yerine.
        

* **Otomatik yeniden yapılandırma**

    Örnekleri yukarı veya aşağı ölçeklendirme, yük dengeleyici anında kendisi yeniden yapılandırır. Yük Dengeleyici kaynağına ek işlemleri olmadan bir yük dengeleyici ekleme veya VM'ler arka uç havuzundan kaldırma yeniden yapılandırır.

* **Sistem durumu araştırmaları**

    Arka uç havuzundaki örneklerinin durumunu belirlemek için yük dengeleyici tanımladığınız sistem durumu araştırmaları kullanır. Bir araştırma yanıt veremediğinde, yük dengeleyici yeni bağlantılar için iyi durumda olmayan örnekler göndermeyi durdurur. Varolan bağlantılar etkilenmez ve VM'yi kapatın veya bir boşta kalma zaman aşımı oluşur, akış uygulama sonlanana kadar devam ederler.
     
    Yük Dengeleyici sağlar [farklı sistem durumu araştırması türleri](load-balancer-custom-probe-overview.md#types) TCP, HTTP ve HTTPS uç noktaları için.

    Ayrıca, ek türü Klasik bulut hizmetlerini kullanırken izin: [Konuk Aracısı](load-balancer-custom-probe-overview.md#guestagent).  Bu durum araştırması son çare olarak düşündüğünüz olmalıdır ve diğer seçenekleri uygulanabilir olduğunda önerilmez.
    
* **Giden bağlantılar (SNAT)**

    Sanal ağınız içindeki özel IP adreslerinden tüm giden akışlar internet üzerindeki genel IP adresleri için bir yük dengeleyici ön uç IP adresi çevrilebilir. Genel ön uç Yük Dengeleme kuralı ile bir arka uç VM bağlıdır, Azure genel ön uç IP adresi için otomatik olarak çevrilemeyen giden bağlantılar programlar.

    * Ön uç hizmetinin başka bir örneği için dinamik olarak eşlenebilir çünkü kolay yükseltme ve Hizmetleri, olağanüstü durum kurtarma sağlar.
    * Daha kolay erişim denetimi listesi (ACL) yönetimi için. Ön uç açısından ifade ACL IP'ler Hizmetleri ölçek yukarı veya aşağı değiştirmeyin veya yeniden.  Makineleri beyaz listeye ekleme yükünü azaltabilir daha giden bağlantılar için IP adreslerini daha az sayıda çevriliyor.

    Daha fazla bilgi için [giden bağlantılar](load-balancer-outbound-connections.md).

Standart Load Balancer bu temelleri ötesinde ek SKU'ya özgü özellikleri vardır. Ayrıntılar için bu makalenin geri kalanında gözden geçirin.

## <a name="skus"></a> Yük Dengeleyici SKU karşılaştırma

Load Balancer, temel ve standart her farklı senaryo ölçek, özellikler ve fiyatlandırma SKU destekler. Temel Load Balancer ile mümkün olan her senaryoya standart Load Balancer ile de oluşturulabilir. Aslında, her iki SKU'ları için API, benzer ve çağrılan bir SKU belirtimi. Yük Dengeleyici ve genel IP için SKU'ları desteklemek için API 2017-08-01 API ile başlayan kullanılabilir. Her iki SKU'ları aynı genel API ve yapısına sahip.

Ancak, seçtiğiniz SKU'ya bağlı olarak, tüm senaryonun yapılandırmasını biraz farklı olabilir. Bir makalede yalnızca belirli bir SKU için geçerli olduğu durumlarda yük Dengeleyicinizin belgelerine çağırır. Karşılaştırın ve farkları anlamak için aşağıdaki tabloya bakın. Daha fazla bilgi için [standart Load Balancer'a genel bakış](load-balancer-standard-overview.md).

>[!NOTE]
> Yeni Tasarım, Standard Load Balancer benimseyin. 

Tek başına VM'lerin kullanılabilirlik kümelerini ve sanal makine ölçek kümeleri yalnızca bir SKU için hiçbir zaman hem de bağlanabilir. Bunları genel IP adresleri ile kullandığınızda, SKU yük dengeleyici hem de genel IP adresi eşleşmelidir. Yük Dengeleyici ve genel IP SKU değiştirilebilir değildir.

_Henüz zorunlu olmadığı halde SKU'ları açıkça belirtmek için en iyi bir uygulamadır._  Şu anda gerekli değişiklikleri için en az eşit durumda tutulur. Bir SKU belirtilmezse, temel SKU 2017-08-01 API sürümünü kullanmak için bir amaç yorumlanır.

>[!IMPORTANT]
>Standart Load Balancer, yeni bir yük dengeleyici ürün ve büyük ölçüde temel yük dengeleyici kümesi ' dir. İki ürün arasındaki önemli ve bilinçli farklılıklar vardır. Temel Load Balancer ile mümkün olan herhangi bir uçtan uca senaryo standart Load Balancer ile de oluşturulabilir. Temel yük dengeleyici için zaten alışıksanız, Standard Load Balancer standart ve temel ve bunların etkilerine arasındaki davranış son değişiklikleri anlamak için öğrenmeniz. Bu bölümde dikkatle gözden geçirin.

[!INCLUDE [comparison table](../../includes/load-balancer-comparison-table.md)]

Daha fazla bilgi için [yük dengeleyici için hizmet sınırları](https://aka.ms/lblimits). Standard Load Balancer için bilgi [genel bakış](load-balancer-standard-overview.md), [fiyatlandırma](https://aka.ms/lbpricing), ve [SLA](https://aka.ms/lbsla).

## <a name="concepts"></a>Kavramlar

### <a name = "publicloadbalancer"></a>Herkese açık yük dengeleyici

Genel yük dengeleyici genel IP adresi ve bağlantı noktası numarasını, gelen trafiğin sanal makineden özel IP adresi ve bağlantı noktası numarası VM'nin ve tersi yanıt trafiği için eşler. Yük Dengeleme kuralları uygulayarak, birden çok VM veya hizmet arasında trafiği belirli türlerdeki dağıtabilirsiniz. Örneğin, web isteği trafik yükünü birden çok web sunucusu arasında yayılabilir.

Yük dengeli uç nokta genel ve TCP bağlantı noktası 80 için üç VM'ler arasında paylaşılan web trafiği için aşağıdaki şekilde gösterilmiştir. Bu üç Vm'leri bir yük dengeli küme içindedir.

![Herkese açık yük dengeleyici örneği](./media/load-balancer-overview/IC727496.png)

*Şekil: karşı web trafiği bir genel yük dengeleyici kullanarak yükleme*

İnternet istemcileri genel IP adresine TCP bağlantı noktası 80 üzerinde bir web uygulaması Web sayfası istekleri gönderirken, Azure Load Balancer istekleri yük dengeli küme üç vm'lere dağıtır. Yük Dengeleyici algoritmalar hakkında daha fazla bilgi için bkz. [yük dengeleyici özellikleri](load-balancer-overview.md##fundamental-load-balancer-features) bu makalenin.

Varsayılan olarak, Azure yük dengeleyici ağ trafiği birden çok sanal makine örnekleri arasında eşit olarak dağıtır. Oturum benzeşimini de yapılandırabilirsiniz. Daha fazla bilgi için [yük dengeleyici dağıtım modu](load-balancer-distribution-mode.md).

### <a name = "internalloadbalancer"></a> İç yük dengeleyici

İç yük dengeleyici trafiği sanal ağ içinde olmayan veya Azure altyapı erişmek için bir VPN kullanan kaynaklara yönlendirir. Bu bakımdan, iç yük dengeleyici genel yük dengeleyiciden farklıdır. Azure altyapısı, bir sanal ağın ön uç yük dengeli IP adresleri için erişimi kısıtlar. ön uç IP adresleri ve sanal ağlar, bir internet uç noktasına hiçbir zaman doğrudan kullanıma sunulur. İç satır iş kolu uygulamalarını Azure'da çalışır ve azure'da veya şirket içi kaynaklardan gelen sonuna erişilir.

İç yük dengeleyici, Yük Dengeleme aşağıdaki türleri sağlar:

* **Bir sanal ağ içindeki**: yük Vm'lerden sanal ağ, aynı sanal ağda bulunan VM'ler bir dizi Dengeleme.
* **Şirketler arası sanal ağ için**: yük, aynı sanal ağda bulunan VM'ler bir dizi şirket içi bilgisayarlardan Dengeleme. 
* **Çok katmanlı uygulamalar için**: Yük Dengeleme internet'e yönelik çok katmanlı uygulamalar için arka uç katmanlarından olmadığı internet'e yönelik. Arka uç katmanlarından trafik yükünü dengeleme gerektiren internet'e yönelik Katmanı (aşağıdaki şekilde bakın).
* **Satır iş kolu uygulamaları için**: Yük Dengeleme için ek yük dengeleyici donanım veya yazılım Azure üzerinde barındırılan iş kolu satır uygulama. Bu senaryo, şirket içi sunucular, yük dengeli trafiği olan bilgisayarların küme içindedir içerir.

![İç yük dengeleyici örneği](./media/load-balancer-overview/IC744147.png)

*Şekil: hem genel hem de iç load balancer'ları kullanarak çok katmanlı uygulamalar Dengeleme yükleme*

## <a name="pricing"></a>Fiyatlandırma
Standart Load Balancer kullanımı yapılandırılmış Yük Dengeleme kuralları ve işlenen gelen ve giden veri miktarına bağlı olarak ücretlendirilir. Fiyatlandırma bilgileri, standart yük dengeleyici için Git [Load Balancer fiyatlandırması](https://azure.microsoft.com/pricing/details/load-balancer/) sayfası.

Temel Load Balancer, ücretsiz olarak sunulur.

## <a name="sla"></a>SLA

Standart yük dengeleyici SLA'sı hakkında daha fazla bilgi için Git [yük dengeleyici SLA](https://aka.ms/lbsla) sayfası. 

## <a name="limitations"></a>Sınırlamalar

- Yük Dengeleyici, Yük Dengeleme ve bu belirli IP protokolleri için bağlantı noktası iletme için TCP veya UDP bir üründür.  Yük Dengeleme kuralları ve gelen NAT kuralları TCP ve UDP için desteklenen ve ICMP gibi diğer IP protokolleri için desteklenmiyor. Aksi takdirde bir UDP veya TCP akışı yükü ile etkileşime veya yanıt veya yük dengeleyici değil sonlandır. Bir proxy değil. Bir ön uç bağlantı başarılı doğrulama gerçekleştirmeniz gereken bant bağlantısı ile bir Yük Dengeleme veya gelen NAT kuralı (TCP veya UDP) kullanılan aynı protokol _ve_ en az bir sanal makinelerinizin gerekir oluşturmak bir yanıt için bir istemci için bir ön uç noktasından yanıt bakın.  Yük Dengeleyici ön uç bir bant dışı yanıt almadıktan hiçbir sanal makine yanıt verebilmesi gösterir.  Bir sanal makine yanıt verebilmesi olmadan bir yük dengeleyici ön uç ile etkileşim kurmak mümkün değildir.  Bu durum giden bağlantılar için de geçerlidir burada [bağlantı noktası maske SNAT](load-balancer-outbound-connections.md#snat) olduğu TCP ve UDP; yalnızca ICMP dahil olmak üzere diğer IP protokolleri de başarısız olur.  Azaltmak için bir örnek düzeyi genel IP adresi atayın.
- Sağlayan ortak yük Dengeleyiciler aksine [giden bağlantılar](load-balancer-outbound-connections.md) sanal ağ içindeki özel IP adresleri için ortak IP adresleri aşamasından geçme, iç yük dengeleyici giden çevrilmemesine kaynağı ön uç için her ikisi de olarak bir iç yük dengeleyicisinin özel IP adres alanı bağlantılardır.  Bu çeviri gerekli olduğu değil, benzersiz, iç IP adresi alanı içindeki bağlantı noktası tükenmesi SNAT olasılığını ortadan kaldırır.  Arka uç havuzundaki bir VM'den giden bir akışı hangi havuzda bulunduğu iç yük dengeleyicinin ön uç bir akışa çalışırsa, yan etkisi olan _ve_ eşlendi geri kendisine, akışın iki Bacak eşleşmiyor ve akışın başarısız olur.  Akış ön uç için akışı oluşturduğunuz arka uç havuzundaki aynı sanal makine için yeniden eşleyemiyorsanız, akışın başarılı olur.   Kendisine geri akışı eşler giden akış ön uç VM'den oluşmuş görünür ve karşılık gelen akışta VM'den kendisine oluşmuş görünür. Konuk işletim sisteminin açısından bakıldığında, sanal makinenin içinde aynı akışın gelen ve giden bölümleri eşleşmiyor. TCP yığınına, kaynak ve hedef eşleşmeyen gibi aynı akışı parçası olacak şekilde bu yarısının aynı akışı tanımaz.  Arka uç havuzundaki herhangi bir VM akışı eşlendiği akışı yarısının eşleşir ve VM akışa başarılı bir şekilde yanıt verebilir.  Flow, akışı kaynağı aynı arka uç döndürüldüğünde bu senaryo için aralıklı bağlantı zaman aşımları belirtisidir. İç yük dengeleyicinin arkasındaki proxy katmanı ya da ekleme dahil güvenilir bir şekilde (arka uç havuzundan arka uç havuzları ilgili iç yük dengeleyici ön uç akışlara kaynak) Bu senaryo elde etmek için kullanabileceğiniz birkaç ortak geçici çözümler vardır veya [DSR stili kurallarını kullanarak](load-balancer-multivip-overview.md).  Müşteriler, bir iç yük dengeleyici birleştirebilirsiniz, tüm 3 taraf proxy veya yedek dahili [Application Gateway](../application-gateway/application-gateway-introduction.md) sınırlı bir HTTP/HTTPS Ara sunucu senaryoları için. Elde edilen senaryo azaltmak için bir genel yük dengeleyici kullanabilirken potansiyeli [SNAT tükenmesi](load-balancer-outbound-connections.md#snat) ve dikkatli bir şekilde yönetilen sürece kaçınılmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure Load Balancer genel bir bakış var. Bir yük dengeleyici kullanmaya başlamak için oluşturun, Vm'leri bir özel IIS uzantısı yüklendikten ve Yük Dengeleme ile sanal makineler arasında web uygulaması oluşturun. Bilgi edinmek için bkz [temel yük dengeleyici oluşturma](quickstart-create-basic-load-balancer-portal.md) hızlı başlangıç.
