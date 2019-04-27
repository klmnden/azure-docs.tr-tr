---
title: Azure Load Balancer nedir?
titlesuffix: Azure Load Balancer
description: Azure Load Balancer özelliklerine, mimarisine ve uygulamasına genel bakış. Load Balancer'ın nasıl çalıştığını ve bulutta nasıl faydalanabileceğinizi öğrenin.
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
Customer intent: As an IT administrator, I want to learn more about the Azure Load Balancer service and what I can use it for.
ms.devlang: na
ms.topic: overview
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/11/2019
ms.author: kumud
ms.openlocfilehash: c2f6a614524f0dfb242db11618fda94ce57e6e6a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60734899"
---
# <a name="what-is-azure-load-balancer"></a>Azure Load Balancer nedir?

Azure Load Balancer ile uygulamalarınızı ölçeklendirebilir ve hizmetleriniz için yüksek kullanılabilirlik sunabilirsiniz. Load Balancer gelen senaryoların yanı sıra giden senaryoları da destekler, hem düşük gecikme süresi hem de yüksek aktarım hızı sağlar, tüm TCP ve UDP uygulamaları için milyonlarca akışa kadar ölçekleme yapar.  

Load Balancer, ön ucuna ulaşan yeni gelen akışları kurallara ve sistem durumu yoklamalarına göre arka uç havuzu örneklerine dağıtır. 

Genel bir Load Balancer ayrıca sanal ağınızdaki sanal makinelerin (VM) özel IP'lerini genel IP adreslerine dönüştürerek giden bağlantı desteği sunabilir.

Azure Load Balancer iki Sku'da kullanılabilir: Temel ve Standart. Bu iki SKU arasında ölçek, özellik ve fiyatlandırma açısından farklar vardır. Temel Load Balancer ile gerçekleştirilebilen tüm senaryolar Standart Load Balancer ile de gerçekleştirilebilir ancak yaklaşımlar biraz farklı olabilir. Load Balancer hakkında bilgi edindikçe temel bilgilere ve SKU'ya özgü farklara hakim olmak önemlidir.

## <a name="why-use-load-balancer"></a>Neden Azure Load Balancer'ı kullanmalısınız? 

Azure Load Balancer’ı aşağıdaki görevler için kullanabilirsiniz:

* Gelen İnternet trafiğini VM’leriniz arasında dengeleme. Bu yapılandırma [Genel Yük Dengeleyici](#publicloadbalancer) olarak bilinir.
* Bir sanal ağ içindeki VM'ler arasındaki trafiği dengeleme. Karma senaryolarda yük dengeleyici ön ucuna şirket içi ağdan da erişebilirsiniz. Bu iki senaryo da [İç Yük Dengeleyici](#internalloadbalancer) olarak bilinir.
* Gelen ağ adresi çevirisi (NAT) kurallarıyla trafiği belirli VM'ler üzerindeki belirli bir bağlantı noktasına yönlendirme.
* Genel Yük Dengeleyici kullanarak sanal ağınızın içindeki VM'ler için [giden bağlantı](load-balancer-outbound-connections.md) sağlama.


>[!NOTE]
> Azure, senaryolarınız için tam olarak yönetilen yük dengeleme çözümleri sunar. Aktarım Katmanı Güvenliği (TLS) protokolü sonlandırma ("SSL yük boşaltma") veya HTTP/HTTPS isteği başına uygulama katmanı işleme özellikleri istiyorsanız [Application Gateway](../application-gateway/application-gateway-introduction.md)'i inceleyin. Genel DNS yük dengeleme özelliği arıyorsanız [Traffic Manager](../traffic-manager/traffic-manager-overview.md)'ı inceleyin. Uçtan uca senaryolarınızda bu çözümleri bir arada da kullanabilirsiniz.

## <a name="what-are-load-balancer-resources"></a>Load Balancer kaynakları nelerdir?

Bir Load Balancer kaynağı genel Load Balancer veya iç Load Balancer olabilir. Load Balancer kaynağının işlevleri ön uç, kural, sistem durumu yoklaması ve arka uç havuzu tanımı olarak ifade edilir. VM'den bir arka uç havuzu belirterek VM'lerinizi arka uç havuzuna eklemiş olursunuz.

Load Balancer kaynakları, Azure'ın çok kiracılı altyapısını oluşturmak istediğiniz senaryoya göre programlamasını belirtmek için kullanabileceğiniz nesnelerdir. Load Balancer kaynakları ile gerçek altyapı arasında doğrudan bir ilişki yoktur. Load Balancer oluşturmak bir örnek oluşturmaz ve kapasite her zaman kullanılabilir durumdadır. 

## <a name="fundamental-load-balancer-features"></a>Temel Load Balancer özellikleri

Load Balancer, TCP ve UDP uygulamaları için aşağıdaki temel özellikleri sunar:

* **Yük dengeleme**

    Azure Load Balancer ile ön uca ulaşan trafiğin arka uç havuzu örneklerine dağıtılması için bir yük dengeleme kuralı oluşturabilirsiniz. Load Balancer, gelen akışların dağıtılması için karma tabanlı bir algoritma kullanır ve akışların üst bilgilerini yeniden yazarak ilgili arka uç havuzu örneklerine yönlendirme yapar. Sistem durumu yoklaması, arka uç noktasının iyi durumda olduğunu belirttiğinde ilgili sunucu yeni akış almak için hazır durumda demektir.
    
    Load Balancer varsayılan olarak akışları kullanılabilir sunucularla eşlemek için kaynak IP adresi, kaynak bağlantı noktası, hedef IP adresi, hedef bağlantı noktası ve IP protokol numarasından oluşan 5 tanımlama grubu karması kullanır. Belirli bir kural için 2 veya 3 tanımlama grubu karmasını tercih ederek belirli bir IP adresi için benzeşim oluşturabilirsiniz. Aynı paket akışının tüm paketleri, yük dengeleme uygulanan ön ucun arkasındaki aynı örneğe ulaşır. İstemci aynı kaynak IP adresinden yeni bir akış başlattığında kaynak bağlantı noktası değiştirilir. Sonuç olarak 5 tanımlama grubu uygulaması, trafiğin farklı bir arka uç noktasına gitmesine neden olabilir.

    Daha fazla bilgi için bkz. [Load Balancer dağıtım modu](load-balancer-distribution-mode.md). Aşağıdaki görüntüde karma tabanlı dağıtım gösterilmiştir:

    ![Karma tabanlı dağıtım](./media/load-balancer-overview/load-balancer-distribution.png)

    *Şekil: Karma tabanlı dağıtım*

* **Bağlantı noktası iletme**

    Load Balancer ile, IP adresinin belirli bir ön ucundaki belirli bir bağlantı noktasından trafiği sanal ağ içindeki belirli bir arka uçtaki belirli bir bağlantı noktasına iletmek için bir gelen NAT kuralı oluşturabilirsiniz. Bu işlem de yük dengeleme ile aynı karma tabanlı dağıtım kullanılarak gerçekleştirilir. Bu özellikle ilgili yaygın senaryolar Azure Sanal Ağı içindeki VM örnekleriyle kurulan Uzak Masaüstü Protokolü (RDP) veya Secure Shell (SSH) oturumlarıdır. Birden fazla iç uç noktayı aynı ön uç IP adresi üzerindeki farklı bağlantı noktalarına eşleyebilirsiniz. Ön uç IP adreslerini sanal makinelerinize ek atlama kutusunu gerek kalmadan internet üzerinden uzaktan kullanabilirsiniz.

* **Uygulamadan bağımsız ve saydam**

    Load Balancer, TCP veya UDP ya da uygulama katmanı ile doğrudan etkileşim kurmaz ve tüm TCP veya UDP uygulama senaryoları desteklenebilir.  Load Balancer akışları sonlandırmaz veya başlatmaz, akışın yüküyle etkileşim kurmaz, uygulama katmanı ağ geçidi işlevi sunmaz ve protokol el sıkışmaları her zaman istemci ile arka uç havuzu örneği arasında gerçekleşir.  Gelen akışa verilen yanıt her zaman sanal makineden gelen yanıttır.  Akış sanal makineye ulaştığında özgün kaynak IP adresi de korunur.  Aşağıdaki örnekler saydamlık konusunda daha fazla bilgi sunmaktadır:
    - Her uç noktaya yalnızca bir VM tarafından yanıt verilir.  Örneğin bir TCP el sıkışması her zaman istemci ile seçilen arka uç VM'si arasında gerçekleşir.  Bir ön uç isteğine verilen yanıt, arka uç VM'si tarafından oluşturulan yanıttır. Ön uç bağlantısını başarıyla doğruladığınızda en az bir arka uç sanal makinesi ile uçtan uca bağlantıyı doğrulamış olursunuz.
    - Uygulama yükleri Load Balancer için saydamdır ve herhangi bir UDP veya TCP uygulamasına destek sunulabilir. HTTP isteği başına işleme veya uygulama katmanı yüklerinin değiştirilmesini (örneğin HTTP URL'lerinin ayrıştırılması) gerektiren iş yükleri için [Application Gateway](https://azure.microsoft.com/services/application-gateway) gibi bir 7. katman yük dengeleyici kullanmanız gerekir.
    - Load Balancer, TCP yükünden bağımsız olduğundan ve TLS yükü boşaltma ("SSL") özelliği sağlanmadığından Load Balancer'ı kullanarak uçtan uca şifrelenmiş senaryolar oluşturabilir ve VM üzerinde TLS bağlantısını sonlandırarak TLS uygulamalarının ölçeğini genişletebilirsiniz.  Örneğin TLS oturum anahtarı oluşturma kapasiteniz, arka uç havuzuna eklediğiniz VM'lerin türü ve sayısıyla sınırlıdır.  "SSL yükü boşaltma", uygulama katmanı işleme gereksiniminiz varsa veya sertifika yönetimini Azure'a devretmek istiyorsanız, Azure'ın 7. katman yük dengeleyici çözümü olan [Application Gateway](https://azure.microsoft.com/services/application-gateway)'i kullanmanız gerekir.
        

* **Otomatik yeniden yapılandırma**

    Örneklerin ölçeğini artırdığınızda veya azalttığınızda Load Balancer kendisini anında yeniden yapılandırır. Arka uç havuzunda VM ekleme veya kaldırma işlemi gerçekleştirdiğinizde Load Balancer kaynağında ek işlem yapmanıza gerek kalmadan Load Balancer yeniden yapılandırılır.

* **Sistem durumu yoklamaları**

    Load Balancer, arka uç havuzundaki örneklerin durumunu belirlemek için sizin tanımladığınız sistem durumu yoklamalarını kullanır. Bir yoklama yanıt veremediğinde, Load Balancer iyi durumda olmayan örneklere yeni bağlantı göndermeyi durdurur. Var olan bağlantılar durumdan etkilenmez ve uygulama akışı sonlandırana, boşta zaman aşımı oluşana veya VM kapatılana kadar devam eder.
     
    Load Balancer TCP, HTTP ve HTTPS uç noktaları için [farklı sistem durumu yoklama türleri](load-balancer-custom-probe-overview.md#types) sunar.

    Ayrıca, Klasik bulut Hizmetleri'ni kullanarak, ek bir tür izin verilir:  [Konuk Aracısı](load-balancer-custom-probe-overview.md#guestagent).  Bu seçenek sistem durumu yoklamaları için yalnızca son çare olarak kullanılmalıdır ve diğerleri kullanılabilir durumda olduğunda kullanılması önerilmez.
    
* **Giden bağlantılar (SNAT)**

    Sanal ağınızın içindeki özel IP adreslerinden internet üzerindeki genel bir IP adresine giden tüm akışlar, Load Balancer'ın ön uç IP adresine çevrilebilir. Genel bir ön uç yük dengeleme kuralı ile bir arka uç VM'ye bağlandığında Azure, giden bağlantıları otomatik olarak genel ön uç IP adresine çevrilecek şekilde programlar.

  * Ön uç dinamik olarak hizmetin başka bir örneğiyle eşlenebileceği için hizmetlerin yükseltme ve olağanüstü durum kurtarma süreçleri kolaylaştırılmış olur.
  * Erişim denetim listesi (ACL) yönetimi daha kolay hale gelir. Hizmetin ölçeği artıp azaldığında veya yeniden dağıtım yapıldığında ön uç IP adresi olarak belirtilen ACL'ler değişmez.  Giden bağlantıları makinelerden daha az sayıda IP adresine çevirmek, izin verilenler listesine ekleme yükünü azaltabilir.

    Daha fazla bilgi için bkz. [giden bağlantılar](load-balancer-outbound-connections.md).

Standart Load Balancer, bu temel bileşenlerin ötesinde SKU'ya özgü ek özelliklere sahiptir. Ayrıntılar için bu makalenin devamını inceleyin.

## <a name="skus"></a> Load Balancer SKU karşılaştırması

Load Balancer senaryo ölçeği, özellikler ve fiyatlandırma açısından farklı özelliklere sahip olan Temel ve Standart SKU'ları destekler. Temel Load Balancer ile gerçekleştirilebilen tüm senaryolar Standart Load Balancer ile oluşturulabilir. Aslında iki SKU'nun da API'leri benzerdir ve SKU belirtilerek çağrılır. Load Balancer ve genel IP SKU'ları için API desteği 2017-08-01 tarihli API ile kullanıma sunulmuştur. İki SKU da aynı genel API'ye ve yapıya sahiptir.

Ancak seçtiğiniz SKU'ya bağlı olarak senaryo yapılandırması farklı olabilir. Load Balancer belgelerinde yalnızca belirli bir SKU için geçerli olan makaleler belirtilmiştir. Farkları karşılaştırmak ve anlamak için aşağıdaki tabloya bakın. Daha fazla bilgi için bkz. [Standart Load Balancer'a genel bakış](load-balancer-standard-overview.md).

>[!NOTE]
> Yeni tasarımlarda Standart Load Balancer kullanılmalıdır. 

Tek başına VM'ler, kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri yalnızca tek bir SKU'ya bağlanabilir, ikisine birden bağlanamaz. Bunları genel IP adresleriyle kullandığınızda hem Load Balancer hem de genel IP adresi SKU'su eşleşmelidir. Load Balancer ve genel IP SKU'ları değiştirilebilir değildir.

_Henüz zorunlu olmasa da en iyi yöntem, SKU'ları açıkça belirtmektir._  Şu an için gerekli değişiklikler minimum düzeyde tutulmaktadır. SKU belirtilmediğinde Temel SKU'nın 2017-08-01 API sürümünün kullanılmak istendiği şeklinde yorumlanır.

>[!IMPORTANT]
>Standart Load Balancer, yeni bir Load Balancer ürünüdür ve Temel Load Balancer'da bulunan özelliklerin daha fazlasına sahiptir. İki ürün arasında önemli ve belirli farklar vardır. Temel Load Balancer ile gerçekleştirilebilen tüm uçtan uca senaryolar Standart Load Balancer ile de oluşturulabilir. Temel Load Balancer'ı kullanıyorsanız Standart ile Temel sürümler arasındaki davranış farklarını ve etkilerini anlamak için Standart Load Balancer'ı incelemeniz önerilir. Bu bölümü dikkatli bir şekilde inceleyin.

[!INCLUDE [comparison table](../../includes/load-balancer-comparison-table.md)]

Daha fazla bilgi için bkz. [Load Balancer hizmet sınırları](https://aka.ms/lblimits). Standart Load Balancer hakkında ayrıntılı bilgi için bkz. [genel bakış](load-balancer-standard-overview.md), [fiyatlandırma](https://aka.ms/lbpricing) ve [SLA](https://aka.ms/lbsla).

## <a name="concepts"></a>Kavramlar

### <a name = "publicloadbalancer"></a>Genel Load Balancer

Genel Load Balancer, gelen trafiğin genel IP adresi ile bağlantı noktası numarasını VM'nin özel IP adresi ve bağlantı noktası numarası ile eşler ve VM'den giden yanıt için de bunun tersini yapar. Yük dengeleme kuralları uygulayarak birden fazla VM veya hizmet için farklı trafik türlerini dağıtabilirsiniz. Örneğin web isteği trafiğinin yükünü birden fazla web sunucusuna dağıtabilirsiniz.

Aşağıdaki şekilde genel ve 80 numaralı TCP bağlantı noktası için üç VM arasında paylaşılmış olan web trafiğine yönelik yük dengeleme uygulanmış bir uç nokta gösterilmiştir. Bu üç VM, bir yük dengeleme kümesinde bulunur.

![Genel Load Balancer örneği](./media/load-balancer-overview/IC727496.png)

*Şekil: Genel Load Balancer'ı kullanarak Web trafiğini Dengeleme yükleme*

İnternet istemcileri 80 numaralı TCP bağlantı noktası üzerinden bir web uygulamasının genel IP adresine istek gönderdiğinde Azure Load Balancer bu isteği yük dengeleme kümesindeki üç VM arasında dağıtır. Load Balancer algoritmaları hakkında daha fazla bilgi için bu makalenin [Load Balancer özellikleri](load-balancer-overview.md##fundamental-load-balancer-features) bölümüne bakın.

Azure Load Balancer ağ trafiğini varsayılan olarak birden fazla VM örneği arasında eşit olarak dağıtır. Oturum benzeşimini de yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Load Balancer dağıtım modu](load-balancer-distribution-mode.md).

### <a name = "internalloadbalancer"></a> İç Load Balancer

Bir iç Load Balancer, trafiği yalnızca bir sanal ağın içindeki veya Azure altyapısına bağlanmak için VPN kullanan kaynaklara yönlendirir. İç Load Balancer ile genel Load Balancer arasındaki fark budur. Azure altyapısı erişimi bir sanal ağın yük dengeli ön uç IP adresleriyle sınırlar. ön uç IP adresleri ve sanal ağlar, bir internet uç noktasına hiçbir zaman doğrudan kullanıma sunulur. İç iş kolu uygulamaları Azure'da çalışır ve Azure'dan veya şirket içi kaynaklardan erişim sağlanır.

İç Load Balancer, aşağıdaki yük dengeleme türlerini destekler:

* **Bir sanal ağ içindeki**: Vm'lerden sanal ağında bir dizi aynı sanal ağda bulunan VM'ler için Yük Dengeleme.
* **Şirketler arası sanal ağ için**: Şirket içi bilgisayarlardan bir dizi aynı sanal ağda bulunan VM'ler için yük dengelemeyi. 
* **Çok katmanlı uygulamalar için**: Yük Dengeleme internet'e yönelik çok katmanlı uygulamalar için arka uç katmanlarından internet'e yönelik olmadığı. Arka uç katmanları için internete yönelik katmandan trafik yük dengelemesi gerekir (aşağıdaki şekilde gösterilmiştir).
* **Satır iş kolu uygulamaları için**: Yük Dengeleme için ek yük dengeleyici donanım veya yazılım Azure üzerinde barındırılan iş kolu satır uygulama. Bu senaryo, trafiğine yük dengeleme uygulanmış olan bilgisayar kümesinde bulunan şirket içi sunucuları da içerir.

![İç Load Balancer örneği](./media/load-balancer-overview/IC744147.png)

*Şekil: Çok katmanlı uygulamalar hem genel hem de iç Yük Dengeleyiciyi kullanarak yük dengelemenin*

## <a name="pricing"></a>Fiyatlandırma

Standart Load Balancer kullanım ücretlendirilir.

- Yapılandırılmış Yük Dengeleme veya giden kuralları sayısı (gelen NAT kuralları yok sayma toplam kural sayısı karşı)
- Veri miktarı, gelen ve giden bakılmadan, kural işlendi. 

Standart Load Balancer fiyatlandırma bilgileri için [Load Balancer fiyatlandırması](https://azure.microsoft.com/pricing/details/load-balancer/) sayfasını inceleyin.

Temel Load Balancer ücretsiz olarak sunulur.

## <a name="sla"></a>SLA

Standart Load Balancer SLA koşulları hakkında bilgi edinmek için [Load Balancer SLA](https://aka.ms/lbsla) sayfasına gidin. 

## <a name="limitations"></a>Sınırlamalar

- Load Balancer, TCP veya UDP IP protokolleri için yük dengeleme ve bağlantı noktası iletme özellikleri sunan bir üründür.  Yük dengeleme kuralları ve gelen NAT kuralları, TCP ve UDP için desteklenir ancak ICMP dahil olmak üzere diğer IP protokolleri için desteklenmez. Load Balancer, UDP veya TCP akışlarının yüklerini sonlandırmaz, yanıtlamaz veya başka bir şekilde etkileşim kurmaz. Bir ara sunucu değildir. Bir ön uç bağlantısının başarıyla doğrulanması için yük dengeleme veya gelen NAT kuralı için kullanılan aynı protokolde (TCP veya UDP) gerçekleşmesi _ve_ sanal makinelerinizden en az birinin istemci için ön uçtan bir yanıt olarak görülecek bir yanıt oluşturması gerekir.  Load Balancer ön ucundan aynı bantta yanıt alınmaması, sanal makinelerin hiçbirinin yanıt veremediğini gösterir.  Bir sanal makine yanıt vermediği sürece Load Balancer ile etkileşim kurulması mümkün değildir.  Bu durum [port masquerade SNAT](load-balancer-outbound-connections.md#snat) öğesinin yalnızca TCP ve UDP için desteklendiği giden bağlantılar için de geçerlidir. ICMP de dahil olmak üzere diğer bağlantılar başarısız olacaktır.  Azaltmak için bir örnek düzeyinde Genel IP adresi atayın.
- Sanal ağ içindeki özel IP adreslerden genel IP adreslerine geçiş yapan [giden bağlantılar](load-balancer-outbound-connections.md) sunan genel Load Balancer örneklerinden farklı olarak iç Load Balancer örnekleri ikisi de özel IP adres alanında bulunduğundan giden bağlantıları iç Load Balancer örneğinin ön ucuna çevirmez.  Bu durum çevirinin gerekli olmadığı durumlarda iç IP adres alanı içindeki SNAT bağlantı noktası tükenme potansiyelini ortadan kaldırır.  Bunun yan etkisi, arka uç havuzundaki bir VM'den giden akışın havuzun bulunduğu iç Load Balancer örneğinin ön ucuna doğru bir akış başlatma girişiminde bulunması _ve_ kendine eşlenmesi durumunda akışın iki ayağının eşleşmemesine ve akışın başarısız olmasına neden olmasıdır.  Akışın ön uç akışını oluşturan arka uçtaki aynı VM ile eşleşmemesi durumunda akış başarılı olacaktır.   Akış kendine eşlendiğinde giden akış VM'den ön uca doğru, ilgili gelen akış da VM'den kendine doğru görünür. Konuk işletim sisteminin açısından bakıldığında aynı akışın gelen ve giden bölümleri sanal makine ile eşleşmez. Kaynak ve hedef eşleşmediğinden TCP yığını aynı akışın iki yarısını aynı akışa ait gibi görmez.  Akış, arka uç havuzundaki başka bir VM ile eşleştiğinde akışın iki yarısı eşleşir ve VM akışa başarıyla yanıt verebilir.  Bu senaryonun belirtileri akışın, akışı başlatan aynı arka uca döndüğünde aralıklı olarak bağlantı zaman aşımına uğramasıdır. Bu senaryoyu (arka uç havuzundan başlatılan akışları ilgili Load Balancer ön ucuna göre arka uç havuzlarına iletme) elde etmek için kullanılabilecek birçok yayın geçici çözüm vardır ve bunlar iç Load Balancer arkasına ara sunucu katmanı eklemeyi veya [DSR stil kurallarını kullanmayı](load-balancer-multivip-overview.md) içerir.  Müşteriler iç Load Balancer örneklerini 3. taraf ara sunucularla birlikte kullanabilir veya HTTP/HTTPS ile sınırlı ara sunucu senaryoları için iç [Application Gateway](../application-gateway/application-gateway-introduction.md) çözümünden faydalanabilir. Karmaşıklığı azaltmak için genel Load Balancer kullanabilirsiniz ancak elde edeceğiniz senaryo [SNAT tükenmesi](load-balancer-outbound-connections.md#snat) olasılığına sahip olur ve dikkatli bir şekilde yönetilmediği sürece kaçınılmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure Load Balancer’ın genel bakış bilgilerine sahipsiniz. Load Balancer'ı kullanmaya başlamak için bir yük dengeleyici oluşturun, özel IIS uzantısı yüklü VM'ler oluşturun ve VM'ler arasında web uygulaması yük dengelemesi gerçekleştirin. Nasıl yapacağınızı öğrenmek için [Temel Yük Dengeleyici Oluşturma](quickstart-create-basic-load-balancer-portal.md) hızlı başlangıcına bakın.
