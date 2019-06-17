---
title: Azure'da Yük Dengeleme hizmetlerini kullanarak | Microsoft Docs
description: 'Bu öğreticide, Azure yük dengeleyici Portföy kullanarak bir senaryo oluşturulacağını gösterir: Traffic Manager, uygulama ağ geçidi ve yük dengeleyici.'
services: traffic-manager
documentationcenter: ''
author: liumichelle
manager: dkays
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/27/2016
ms.author: limichel
ms.openlocfilehash: 906e1840f35ab14997c727551b893a0219eb78d8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60330573"
---
# <a name="using-load-balancing-services-in-azure"></a>Azure'da Yük Dengeleme hizmetlerini kullanma

## <a name="introduction"></a>Giriş

Microsoft Azure, birden çok ağ trafiğin dağıtımını yönetmek için hizmet ve Yük Dengelemesi sağlar. Bu hizmetlerin ayrı ayrı kullanın veya ihtiyaçlarınıza en uygun çözümü derlemek için kendi yöntemlerini birleştirin.

Bu öğreticide, biz öncelikle müşteri kullanım örneğini tanımlamak ve nasıl, daha sağlam hale getirilebilir ve yüksek performanslı aşağıdaki Azure yük dengeleyici Portföy kullanarak görebilirsiniz: Traffic Manager, uygulama ağ geçidi ve yük dengeleyici. Ardından, coğrafi olarak yedekli, trafiği Vm'lere dağıtır ve yardımcı olan bir dağıtım oluşturmak için adım adım yönergeler farklı türde istekler yönetme sunuyoruz.

Kavramsal bir düzeyde, bu hizmetlerin her biri Yük Dengeleme hiyerarşideki farklı bir rol oynar.

* **Traffic Manager** Genel DNS Yük Dengeleme sağlar. Bu, gelen DNS istekleri arar ve müşterinin seçtiği yönlendirme ilkesine uygun olarak sağlıklı bir uç nokta ile yanıt verir. Yönlendirme yöntemleri için Seçenekler şunlardır:
  * Performans, gecikme süresi açısından en yakın uç noktaya bir istek sahibi göndermek için yönlendirme.
  * Tüm trafiği bir uç nokta yedekleme diğer uç noktalar ile yönlendirme önceliği.
  * Ağırlıklı yönlendirme, trafiği her uç noktasına atanan ağırlığı göre dağıtan hepsini.
  * Coğrafi konum tabanlı uygulama uç noktalarınıza kullanıcı coğrafi konumunu temel alarak trafiği dağıtmak için yönlendirme.
  * Alt ağ tabanlı alt ağa kullanıcının (IP adresi aralığı) dayalı bir uygulama uç noktalarınıza trafiği dağıtmak için yönlendirme.
  * Birden çok değer, yönlendirme, birden fazla uygulama uç noktası IP adresleri, tek bir DNS yanıtına göndermek etkinleştirin.

  İstemci, doğrudan Traffic Manager tarafından döndürülen uç noktasına bağlanır. Azure Traffic Manager uç nokta sağlıksız olduğunu ve ardından istemcilerin sağlıklı başka bir örneğine yeniden yönlendirir algılar. Başvurmak [Azure Traffic Manager belgeleri](traffic-manager-overview.md) hizmeti hakkında daha fazla bilgi için.
* **Uygulama ağ geçidi** çeşitli 7. Katman Yük Dengeleme Özellikleri uygulamanız için sunan hizmet olarak uygulama teslim denetleyicisi (ADC) sunar. Bu, müşterilere CPU yoğunluklu SSL sonlandırması yükünü application gateway'e boşaltarak web grubu üretkenliğini iyileştirme olanağı tanır. Hepsini bir kez deneme dağıtımını gelen trafiği, tanımlama bilgilerine dayalı oturum benzeşimi, URL yolu tabanlı Yönlendirme ve tek bir uygulama ağ geçidinin arkasında birden fazla Web sitesi barındırma olanağı diğer 7. Katman yönlendirme özelliklerini içerir. Uygulama ağ geçidi, bir Internet'e yönelik ağ geçidi, yalnızca dahili ağ geçidi veya her ikisinin bir birleşimi yapılandırılabilir. Application Gateway tamamen Azure yönetilen, ölçeklenebilir ve yüksek oranda kullanılabilir. Daha iyi yönetilebilirlik için zengin tanılama ve günlüğe kaydetme özellikleri sağlar.
* **Yük Dengeleyici** yüksek performanslı, düşük gecikme süreli katman 4 Yük Dengeleme Hizmetleri için tüm UDP ve TCP protokollerini sağlayan Azure SDN yığınını önemli bir parçasıdır. Bu, gelen ve giden bağlantıları yönetir. Genel ve iç yük dengeli uç noktalarını yapılandırma ve hizmet kullanılabilirliği yönetmek için TCP ve HTTP sistem durumu yoklaması seçeneklerini kullanarak arka uç havuzu hedeflere gelen bağlantıları eşlemek için kurallar tanımlar.

## <a name="scenario"></a>Senaryo

Bu örnek senaryoda, içeriği iki tür veren basit bir Web sitesi kullanıyoruz: görüntüleri ve dinamik olarak oluşturulan Web sayfaları. Web sitesi coğrafi olarak yedekli olmalıdır ve en yakın (en düşük gecikme süresi), kullanıcıların kullanılmalıdır onlara konumu. Uygulama geliştiricisi, verdi deseni/resimler /'ile eşleşen herhangi bir URL * web grubu geri kalanından farklı bir VM adanmış bir havuzdan sunulur.

Ayrıca, dinamik içerik sunan varsayılan sanal makine havuzu barındırılan bir arka uç veritabanı için yüksek kullanılabilirlik kümesinde konuşmak gerekir. Tüm dağıtım, Azure Resource Manager aracılığıyla ayarlanır.

Traffic Manager, uygulama ağ geçidi ve Load Balancer'ı kullanarak, bu tasarım hedeflere ulaşmak bu Web sitesi sağlar:

* **Çoklu coğrafi yedeklilik**: Tek bir bölge kalırsa, Traffic Manager trafiği sorunsuz bir şekilde en yakın bölgeyi müdahalesi olmadan uygulama sahibinden yönlendirir.
* **Düşük gecikme süresi**: Traffic Manager otomatik olarak en yakın bölgeyi müşteriye yönlendirir olduğundan, müşterinin Web sayfası içeriği isteğinde bulunurken daha düşük gecikme süresi karşılaşır.
* **Bağımsız ölçeklenebilirlik**: Web uygulaması iş yükü, içerik türü tarafından ayrılmış olduğundan, uygulama sahibi istek iş yüklerini birbirinden bağımsız olarak ölçeklendirebilirsiniz. Uygulama ağ geçidi, belirtilen kuralları ve uygulama durumunu göre doğru havuzlarına trafik yönlendirilir sağlar.
* **İç Yük Dengeleme**: Yüksek oranda kullanılabilirlik kümesi önündeki yük dengeleyici olduğundan, yalnızca etkin ve iyi durumda uç nokta için bir veritabanı uygulamanın kullanımına sunulur. Ayrıca, bir veritabanı yöneticisi, ön uç uygulamasının bağımsız küme arasında etkin ve Pasif çoğaltmaları dağıtarak iş yükü iyileştirebilirsiniz. Yük Dengeleyici, yüksek oranda kullanılabilirlik kümesi için bağlantılar sağlar ve yalnızca iyi durumdaki veritabanları bağlantı isteklerini almanızı sağlar.

Aşağıdaki diyagramda bu senaryonun mimarisi gösterilmektedir:

![Yük Dengeleme diyagramı mimarisi](./media/traffic-manager-load-balancing-azure/scenario-diagram.png)

> [!NOTE]
> Bu örnek yalnızca, Azure'un sunduğu Yük Dengeleme Hizmetleri birçok olası yapılandırmalar biridir. Traffic Manager, uygulama ağ geçidi ve Load Balancer karıştırılabilir ve Yük Dengeleme ihtiyaçlarınızı en iyi karşılayacak şekilde eşleşti. Örneğin, SSL yük boşaltma ya da 7. Katman işlem gerekli değildir, Application Gateway yerine yük dengeleyici kullanılabilir.

## <a name="setting-up-the-load-balancing-stack"></a>Yük Dengeleme yığını kurma

### <a name="step-1-create-a-traffic-manager-profile"></a>1\. adım: Traffic Manager profili oluşturma

1. Azure portalında **kaynak Oluştur** > **ağ** > **Traffic Manager profili**  >   **Oluşturma**.
2. Aşağıdaki temel bilgileri girin:

   * **Ad**: Traffic Manager profilinizin vermek bir DNS ön ek adı.
   * **Yönlendirme yöntemi**: Trafik yönlendirme yöntemini ilkeyi seçin. Yöntemleri hakkında daha fazla bilgi için bkz. [hakkında Traffic Manager trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md).
   * **Abonelik**: Profili içeren aboneliği seçin.
   * **Kaynak grubu**: Profili içeren kaynak grubunu seçin. Bu, yeni veya mevcut bir kaynak grubu olabilir.
   * **Kaynak grubu konumu**: Traffic Manager hizmeti, genel ve bir konuma bağlı değildir. Ancak Traffic Manager profili ile ilişkili meta verilerin bulunduğu grubu için bir bölge belirtmeniz gerekir. Bu konum profili çalışma zamanı kullanılabilirliğini etkilemez.

3. Tıklayın **Oluştur** Traffic Manager profili oluşturmak için.

   !["Traffic Manager'ı Oluştur" dikey penceresi](./media/traffic-manager-load-balancing-azure/s1-create-tm-blade.png)

### <a name="step-2-create-the-application-gateways"></a>2\. adım: Uygulama ağ geçitleri oluşturma

1. Azure portalında, sol bölmede, tıklayın **kaynak Oluştur** > **ağ** > **Application Gateway**.
2. Application gateway hakkında aşağıdaki temel bilgileri girin:

   * **Ad**: Uygulama ağ geçidi adı.
   * **SKU boyutunu**: Küçük, Orta veya büyük kullanılabilir application gateway boyutu.
   * **Örnek sayısı**: Örnek sayısı, 2 ile 10 arasında bir değer.
   * **Kaynak grubu**: Uygulama ağ geçidinin bulunduğu kaynak grubu. Mevcut bir kaynak grubu veya yeni bir tane olabilir.
   * **Konum**: Kaynak grubu ile aynı konumda olan uygulama ağ geçidi için bölge. Sanal ağ ve genel IP ağ geçidi ile aynı konumda olması gerektiğinden önemli bir konumdur.
3. **Tamam**'ı tıklatın.
4. Sanal ağ, alt ağ, ön uç IP ve application gateway için dinleyici yapılandırmaları tanımlar. Bu senaryoda, ön uç IP adresidir **genel**, Traffic Manager profiline bir uç nokta daha sonra eklenmesi için sağlar.
5. Dinleyici aşağıdaki seçeneklerden birini yapılandırın:
    * HTTP kullanırsanız yapılandırmak için hiçbir şey yoktur. **Tamam**'ı tıklatın.
    * Daha fazla HTTPS kullanırsanız, bu yapılandırma gereklidir. Başvurmak [bir uygulama ağ geçidi oluşturma](../application-gateway/application-gateway-create-gateway-portal.md)başlayan 9. adım. Yapılandırmasını tamamladıktan sonra tıklayın **Tamam**.

#### <a name="configure-url-routing-for-application-gateways"></a>Uygulama ağ geçitleri için URL yönlendirmeyi yapılandırma

Arka uç havuzu seçtiğinizde, bir yol tabanlı kural ile yapılandırılmış bir uygulama ağ geçidi bir yol deseni hepsini bir kez deneme dağıtım ek olarak istek URL'si alır. Bu senaryoda, herhangi bir URL'ye yönlendirmek için bir yol tabanlı kural ekliyoruz "/images/\*" görüntü sunucu havuzuna. URL yapılandırma hakkında daha fazla bilgi için bir uygulama ağ geçidi için yol tabanlı yönlendirme başvurduğu [bir uygulama ağ geçidi için bir yol tabanlı kural oluşturma](../application-gateway/application-gateway-create-url-route-portal.md).

![Uygulama ağ geçidi web katman diyagramı](./media/traffic-manager-load-balancing-azure/web-tier-diagram.png)

1. Önceki bölümde oluşturduğunuz uygulama ağ geçidi örneğini, kaynak grubunuzdan gidin.
2. Altında **ayarları**seçin **arka uç havuzları**ve ardından **Ekle** web katmanı arka uç havuzları ile ilişkilendirmek istediğiniz Vm'leri eklemek için.
3. Arka uç havuzunun adı ve Havuzda bulunan makineler tüm IP adreslerini girin. Bu senaryoda, şu iki arka uç sunucu havuzuna sanal makinelerin bağlanırsınız.

   ![Uygulama ağ geçidi "Arka uç havuzu Ekle"](./media/traffic-manager-load-balancing-azure/s2-appgw-add-bepool.png)

4. Altında **ayarları** uygulama ağ geçidi için seçin **kuralları**ve ardından **yol tabanlı** düğmesini bir kural ekleyin.

   ![Uygulama ağ geçidi kuralları "Yol tabanlı" düğmesi](./media/traffic-manager-load-balancing-azure/s2-appgw-add-pathrule.png)

5. Aşağıdaki bilgileri sağlayarak kuralını yapılandırın.

   Temel ayarları:

   + **Ad**: Portalda erişilebilir bir kural kolay adı.
   + **Dinleyici**: Kural için kullanılan dinleyicisi.
   + **Varsayılan arka uç havuzu**: Varsayılan kuralı ile kullanılmak üzere arka uç havuzu.
   + **Varsayılan HTTP ayarları**: Varsayılan kuralı ile kullanılacak HTTP ayarları.

   Yola dayalı kural:

   + **Ad**: Yola dayalı kural kolay adı.
   + **Yolları**: Trafiği iletmek için kullanılan yol kuralı.
   + **Arka uç havuzu**: Bu kural ile kullanılmak üzere arka uç havuzu.
   + **HTTP ayarı**: Bu kural ile kullanılacak HTTP ayarları.

   > [!IMPORTANT]
   > Yolları: Geçerli yollar ile başlamalıdır "/". Joker karakter "\*" yalnızca sonunda izin verilir. Geçerli örnekler /xyz, /xyz\*, veya /xyz/\*.

   ![Uygulama ağ geçidi "Yol tabanlı Kural Ekle" dikey penceresi](./media/traffic-manager-load-balancing-azure/s2-appgw-pathrule-blade.png)

### <a name="step-3-add-application-gateways-to-the-traffic-manager-endpoints"></a>3\. adım: Traffic Manager uç noktaları için uygulama ağ geçitleri ekleyin

Bu senaryoda, Traffic Manager, farklı bölgelerde bulunan (önceki adımlarda yapılandırılmış gibi) uygulama ağ geçitleri bağlıdır. Uygulama ağ geçitleri yapılandırılır, sonraki Traffic Manager profilinize bağlanmak adımdır.

1. Traffic Manager profilinizin açın. Bunu yapmak için kaynak grubu veya Traffic Manager profili adını arayın Ara **tüm kaynakları**.
2. Sol bölmede seçin **uç noktaları**ve ardından **Ekle** uç nokta ekleme.

   ![Traffic Manager uç noktaları "Ekle" düğmesi](./media/traffic-manager-load-balancing-azure/s3-tm-add-endpoint.png)

3. Aşağıdaki bilgileri girerek bir uç nokta oluşturun:

   * **Tür**: Yük Dengeleme uç noktasına türünü seçin. Bu senaryoda seçin **Azure uç noktası** biz, önceden yapılandırılmış uygulama ağ geçidi örnekleri bağlandığınız.
   * **Ad**: Uç nokta adı girin.
   * **Hedef kaynak türü**: Seçin **genel IP adresi** ve sonra **hedef kaynak**, önceden yapılandırılmış uygulama ağ geçidinin genel IP adresini seçin.

   ![Traffic Manager "Uç Ekle"](./media/traffic-manager-load-balancing-azure/s3-tm-add-endpoint-blade.png)

4. Traffic Manager profilinizin DNS ile erişerek kurulumunuzu sınayabilirsiniz artık (Bu örnekte: TrafficManagerScenario.trafficmanager.net). İstekleri yeniden ortaya çıkarmak veya Vm'leri ve farklı bölgelerde oluşturulmuş web sunucuları aşağı taşıyın ve kurulumunuzu test etmek için Traffic Manager profili ayarları değiştirin.

### <a name="step-4-create-a-load-balancer"></a>4\. Adım: Yük dengeleyici oluşturma

Bu senaryoda, yük dengeleyici web katmanından gelen bağlantıları veritabanlarına yüksek oranda kullanılabilirlik kümesi içinde dağıtır.

SQL Server AlwaysOn, yüksek kullanılabilirlik veritabanı kümesi kullanıyorsanız, başvurmak [bir veya daha fazla her zaman üzerinde kullanılabilirlik grubu dinleyicisi yapılandırma](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) adım adım yönergeler için.

İç yük dengeleyici yapılandırma hakkında daha fazla bilgi için bkz. [Azure portalını kullanarak iç yük dengeleyici oluşturma](../load-balancer/load-balancer-get-started-ilb-arm-portal.md).

1. Azure portalında, sol bölmede, tıklayın **kaynak Oluştur** > **ağ** > **yük dengeleyici**.
2. Load balancer'ınız için bir ad seçin.
3. Ayarlama **türü** için **dahili**, uygun bir sanal ağ ve bulunması için yük dengeleyici için alt ağ seçin.
4. Altında **IP adresi ataması**, şunlardan birini seçin **dinamik** veya **statik**.
5. Altında **kaynak grubu**, yük dengeleyici için kaynak grubunu seçin.
6. Altında **konumu**, yük dengeleyici için uygun bölgeyi seçin.
7. Tıklayın **Oluştur** yük dengeleyici oluşturmak için.

#### <a name="connect-a-back-end-database-tier-to-the-load-balancer"></a>Arka uç veritabanı katmanı yük dengeleyiciye bağlanır.

1. Önceki adımlarda oluşturulan yük dengeleyici, kaynak grubunuzdan bulun.
2. Altında **ayarları**, tıklayın **arka uç havuzları**ve ardından **Ekle** bir arka uç havuzuna eklenecek.

   ![Yük Dengeleyici "Arka uç havuzu Ekle"](./media/traffic-manager-load-balancing-azure/s4-ilb-add-bepool.png)

3. Arka uç havuzunun adı girin.
4. Makineleri tek tek veya bir kullanılabilirlik kümesi arka uç havuzuna ekleyin.

#### <a name="configure-a-probe"></a>Bir araştırma yapılandırmanız

1. Load balancer'ınız içinde altında **ayarları**seçin **araştırmaları**ve ardından **Ekle** bir araştırma eklemek için.

   ![Yük Dengeleyici "Araştırma Ekle"](./media/traffic-manager-load-balancing-azure/s4-ilb-add-probe.png)

2. Araştırma için bir ad girin.
3. Seçin **Protokolü** yoklaması için. Bir veritabanı için bir HTTP araştırması yerine bir TCP araştırması isteyebilirsiniz. Yük Dengeleyici araştırmalarını hakkında daha fazla bilgi için bkz [yük dengeleyici araştırmalarını anlama](../load-balancer/load-balancer-custom-probe-overview.md).
4. Girin **bağlantı noktası** veritabanınızın araştırma erişmek için kullanılacak.
5. Altında **aralığı**, uygulama araştırma sıklığını belirtin.
6. Altında **sağlıksız durum eşiği**, sağlıksız olarak değerlendirilmesi arka uç VM'si için oluşması gereken sürekli araştırma hatası sayısını belirtin.
7. Tıklayın **Tamam** araştırmayı oluşturmak için.

#### <a name="configure-the-load-balancing-rules"></a>Yük Dengeleme kurallarını yapılandırma

1. Altında **ayarları** load balancer'ın, seçin **Yük Dengeleme kuralları**ve ardından **Ekle** bir kural oluşturmak için.
2. Girin **adı** Yük Dengeleme kuralı için.
3. Seçin **ön uç IP adresi** yük dengeleyicinin **Protokolü**, ve **bağlantı noktası**.
4. Altında **arka uç bağlantı noktası**, arka uç havuzunda kullanılacak bağlantı noktasını belirtin.
5. Seçin **arka uç havuzu** ve **araştırma** kuralı uygulamak için önceki adımlarda oluşturulmuş.
6. Altında **oturum kalıcılığını**, oturumları kalıcı hale getirmek için nasıl istediğinizi seçin.
7. Altında **boşta kalma zaman aşımı**, önce bir boşta kalma zaman aşımını dakika sayısını belirtin.
8. Altında **kayan IP**, şunlardan birini seçin **devre dışı bırakılmış** veya **etkin**.
9. Kuralı oluşturmak için **Tamam**'a tıklayın.

### <a name="step-5-connect-web-tier-vms-to-the-load-balancer"></a>5\. Adım: Web Katmanı Vm'leri yük dengeleyiciye bağlanır.

Artık IP adresi ve yük dengeleyici ön uç bağlantı herhangi bir veritabanı bağlantı için web katmanı VM'ler üzerinde çalışan uygulamalarda yapılandırıyoruz. Bu yapılandırma, bu sanal makineler üzerinde çalışan uygulamaları özgüdür. Hedef IP adresi ve bağlantı noktası yapılandırmak için uygulamanızın belgelerine bakın. Azure Portalı'nda ön uç IP adresini bulmak için ön uç IP havuzu için go **yük dengeleyici ayarları**.

![Yük Dengeleyici "Ön uç IP havuzu" Gezinti Bölmesi](./media/traffic-manager-load-balancing-azure/s5-ilb-frontend-ippool.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Traffic Manager'a Genel Bakış](traffic-manager-overview.md)
* [Application Gateway'e genel bakış](../application-gateway/application-gateway-introduction.md)
* [Azure Load Balancer’a genel bakış](../load-balancer/load-balancer-overview.md)
