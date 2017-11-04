---
title: "Azure'da Yük Dengeleme hizmetlerini kullanarak | Microsoft Docs"
description: "Bu öğretici Azure yük dengeleyici Portföy kullanarak bir senaryo oluşturulacağını gösterir: trafik Yöneticisi, uygulama ağ geçidi ve yük dengeleyici."
services: traffic-manager
documentationcenter: 
author: liumichelle
manager: vitinnan
editor: 
ms.assetid: f89be3be-a16f-4d47-bcae-db2ab72ade17
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/27/2016
ms.author: limichel
ms.openlocfilehash: ae9bd30b76786f94f0d836a39137da696fdb94a2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="using-load-balancing-services-in-azure"></a>Azure'da Yük Dengeleme Hizmetleri kullanma

## <a name="introduction"></a>Giriş

Microsoft Azure, birden çok ağ trafiğini nasıl dağıtıldığını yönetmek için hizmetleri ve Yük Dengeleme sağlar. Bu hizmetler ayrı ayrı kullanın veya en iyi çözümü oluşturmak için gereksinimlerinize bağlı olarak, kendi yöntemleri birleştirebilirsiniz.

Bu öğretici, biz ilk müşteri kullanım örneği tanımlayın ve Yük Dengeleme aşağıdaki Azure Portföy kullanarak nasıl, daha sağlam hale getirilebilir ve kullanıcı görebilirsiniz: trafik Yöneticisi, uygulama ağ geçidi ve yük dengeleyici. Coğrafi olarak yedekli, trafiği vm'lere dağıtır ve yardımcı olan bir dağıtım oluşturmak için adım adım yönergeler istekleri farklı türlerini yönetmek ardından sağlayın.

Kavramsal bir düzeyde, bu hizmetlerin her birini Yük Dengeleme hiyerarşideki farklı bir rol oynar.

* **Trafik Yöneticisi** Genel DNS Yük Dengeleme sağlar. Gelen DNS isteklerinin görünür ve müşteri seçilmiş yönlendirme ilkesi uygun olarak sağlıklı bir uç nokta ile yanıt verir. Yönlendirme yöntemleri için Seçenekler şunlardır:
  * İstek gecikme süresi açısından en yakın endpoint göndermek için Yönlendirme performans.
  * Diğer uç noktalar olarak yedekleme ile bir uç nokta için tüm trafiği yönlendirmek için yönlendirme önceliği.
  * Ağırlıklı yönlendirme, her uç noktasına atanan ağırlığı göre trafiği dağıtan hepsini.

  İstemci, doğrudan bu uç noktasına bağlanır. Bir uç nokta sağlıksız olduğunu ve başka bir iyi örneğine istemcileri yeniden yönlendiren azure trafik Yöneticisi algılar. Başvurmak [Azure Traffic Manager belgeleri](traffic-manager-overview.md) hizmeti hakkında daha fazla bilgi için.
* **Uygulama ağ geçidi** çeşitli katman 7 Yük Dengeleme özellikleri, uygulamanız için sunumu bir hizmet olarak uygulama teslim denetleyicisi (ADC) sağlar. Uygulama ağ geçidi için CPU-yoğun SSL sonlandırma boşaltarak web grubu verimliliği en iyi duruma getirme yapmasını sağlar. Diğer katman 7 Yönlendirme yetenekleri hepsini dağıtımını gelen trafiği, tanımlama bilgisi tabanlı oturum benzeşimi, URL yolu tabanlı Yönlendirme ve tek bir uygulama ağ geçidi arkasında birden çok Web sitesini barındırmak yeteneğini içerir. Uygulama ağ geçidi, bir Internet'e dönük ağ geçidi, bir yalnızca iç ağ geçidi veya her ikisinin birleşimini yapılandırılabilir. Tam Azure uygulama ağ geçidi yönetilen, ölçeklenebilir ve yüksek oranda kullanılabilir. Daha iyi yönetilebilirlik için zengin tanılama ve günlüğe kaydetme özellikleri sağlar.
* **Yük Dengeleyici** yüksek performans, düşük gecikme süreli katman 4 Yük Dengeleme için Hizmetleri tüm UDP ve TCP protokollerini sağlayan Azure SDN yığın ayrılmaz bir parçasıdır. Gelen ve giden bağlantılara yönetir. Genel ve iç yük dengeli uç noktalarını yapılandırma ve hizmet kullanılabilirliği yönetmek için TCP ve HTTP sistem durumu yoklama seçeneklerini kullanarak arka uç havuzu hedeflere gelen bağlantıları eşlemek için kurallar tanımlar.

## <a name="scenario"></a>Senaryo

Bu örnek senaryoda, içeriği iki tür hizmet veren basit bir Web sitesi kullanırız: görüntüleri ve dinamik olarak oluşturulan Web sayfaları. Web sitesi coğrafi olarak yedekli olmalıdır ve en yakın (en düşük gecikme süresi), kullanıcılardan kullanılmalıdır bunları konuma. Uygulama geliştiricisi, karar vermiştir düzeni/görüntüleri/eşleşen herhangi bir URL * web grubu geri kalanından farklı olan VM'ler ayrılmış havuzundan sunulur.

Ayrıca, dinamik içerik sunan varsayılan VM havuzu yüksek oranda kullanılabilirlik kümesi üzerinde barındırılan bir arka uç veritabanı konuşun gerekiyor. Tüm dağıtım Azure Resource Manager aracılığıyla ayarlanır.

Trafik Yöneticisi, uygulama ağ geçidi ve yük dengeleyici kullanılarak bu tasarım hedeflerinize ulaşmak bu Web sitesi sağlar:

* **Birden çok coğrafi artıklık**: tek bir bölge devre dışı kalırsa, Traffic Manager trafik sorunsuz bir şekilde en yakın bölgeyi müdahalesi olmadan uygulama sahibinden yönlendirir.
* **Gecikme süresi sınırlı**: olmadığından trafik Yöneticisi otomatik olarak en yakın bölgeyi müşteriye yönlendirir, Web sayfası içeriği isterken daha düşük gecikme süresi müşteri deneyimleri.
* **Bağımsız ölçeklenebilirlik**: web uygulaması iş yükü içerik türüne göre ayrılmış olduğundan, uygulama sahibi isteği iş yükleri birbirinden bağımsız ölçeklendirebilirsiniz. Uygulama ağ geçidi trafik, belirtilen kuralları ve uygulama sağlığını göre doğru havuzlarına yönlendirilir sağlar.
* **İç Yük Dengeleme**: çünkü yük dengeleyici önünde yüksek oranda kullanılabilirlik kümesi, yalnızca etkin ve Sağlıklı uç nokta için bir veritabanı uygulamanın kullanımına sunulur. Ayrıca, bir veritabanı yöneticisi, ön uç uygulamasını bağımsız küme üzerinde etkin ve etkin olmayan çoğaltmaları dağıtarak iş yükünü iyileştirebilirsiniz. Yük Dengeleyici bağlantıları için yüksek oranda kullanılabilirlik kümesi sunar ve yalnızca iyi veritabanları bağlantı isteklerini almak sağlar.

Aşağıdaki diyagramda, bu senaryo mimarisi gösterilmektedir:

![Yük Dengeleme diyagramı mimarisi](./media/traffic-manager-load-balancing-azure/scenario-diagram.png)

> [!NOTE]
> Bu örnek yalnızca Azure sunar Yük Dengeleme Hizmetleri birçok olası yapılandırmaları biridir. Trafik Yöneticisi, uygulama ağ geçidi ve yük dengeleyici karışabilir ve Yük Dengeleme gereksinimlerinizi en iyi karşılayacak eşleşti. Örneğin, SSL boşaltma veya katman 7 işlem gerekli değil, uygulama ağ geçidi yerine yük dengeleyici kullanılabilir.

## <a name="setting-up-the-load-balancing-stack"></a>Yük Dengeleme yığını ayarlama

### <a name="step-1-create-a-traffic-manager-profile"></a>1. adım: bir Traffic Manager profilini oluşturma

1. Azure portalında tıklatın **yeni**ve ardından "Trafik Yöneticisi profili." Market arayın
2. Üzerinde **oluşturma trafik Yöneticisi profili** dikey penceresinde, aşağıdaki temel bilgileri girin:

  * **Ad**: Traffic Manager profilinize bir DNS ön ek adı verin.
  * **Yönlendirme yöntemi**: trafik yönlendirme yöntemini ilkesini seçin. Yöntemleri hakkında daha fazla bilgi için bkz: [hakkında Traffic Manager trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md).
  * **Abonelik**: Profil içeriyorsa aboneliği seçin.
  * **Kaynak grubu**: profili içeren kaynak grubunu seçin. Yeni veya var olan kaynak grubu olabilir.
  * **Kaynak grubu konumu**: trafik Yöneticisi hizmeti, genel ve bir konuma bağlı değil. Ancak, trafik Yöneticisi profili ile ilişkilendirilen meta verilerin bulunduğu bir bölge grubu belirtmeniz gerekir. Bu konum profil çalışma zamanı kullanılabilirliğini bir etkisi yoktur.

3. Tıklatın **oluşturma** trafik Yöneticisi profili oluşturmak için.

  !["Trafik Yöneticisi oluşturma" dikey penceresi](./media/traffic-manager-load-balancing-azure/s1-create-tm-blade.png)

### <a name="step-2-create-the-application-gateways"></a>2. adım: uygulama ağ geçidi oluşturma

1. Sol bölmede, Azure portal'ı tıklatın **yeni** > **ağ** > **uygulama ağ geçidi**.
2. Uygulama ağ geçidi hakkında aşağıdaki temel bilgileri girin:

  * **Ad**: uygulama ağ geçidi adı.
  * **SKU boyutunu**: uygulama ağ geçidinin boyutunu, küçük, Orta veya büyük kullanılabilir.
  * **Örnek sayısını**: örnek sayısı, 2 ile 10 arasında bir değer.
  * **Kaynak grubu**: uygulama ağ geçidi tutan kaynak grubu. Varolan bir kaynak grubu veya yeni bir tane olabilir.
  * **Konum**: kaynak grubu ile aynı konumda olan uygulama ağ geçidi için bölge. Sanal ağ ve genel IP ağ geçidi ile aynı konumda olması gerektiğinden önemli bir konumdur.
3. **Tamam** düğmesine tıklayın.
4. Sanal ağ, alt ağ, ön uç IP ve uygulama ağ geçidi için dinleyici yapılandırmalarını tanımlayın. Bu senaryoda, ön uç IP adresidir **ortak**, onu bir uç nokta trafik Yöneticisi profiline daha sonra eklenmesine izin veren.
5. Dinleyici aşağıdaki seçeneklerden birini yapılandırın:
    * HTTP kullanırsanız yapılandırmak için hiçbir şey yoktur. **Tamam** düğmesine tıklayın.
    * Daha fazla HTTPS kullanırsanız, bu yapılandırma gereklidir. Başvurmak [bir uygulama ağ geçidi oluşturma](../application-gateway/application-gateway-create-gateway-portal.md)başlayarak adım 9. Yapılandırmayı tamamladıktan sonra tıklatın **Tamam**.

#### <a name="configure-url-routing-for-application-gateways"></a>Uygulama ağ geçitleri için URL yönlendirmeyi yapılandırma

Bir arka uç havuzu seçtiğinizde, yol tabanlı bir kuralla yapılandırılmış bir uygulama ağ geçidi istek URL'si hepsini dağıtım yanı sıra bir yolu desenini alır. Bu senaryoda, herhangi bir URL'ye yönlendirmek için bir yol tabanlı kural ekliyoruz "/images/\*" görüntü sunucu havuzuna. URL yapılandırma hakkında daha fazla bilgi için yol tabanlı bir uygulama ağ geçidi için yönlendirme başvurmak için [bir uygulama ağ geçidi için bir yol tabanlı kural oluşturma](../application-gateway/application-gateway-create-url-route-portal.md).

![Uygulama ağ geçidi web katmanı diyagramı](./media/traffic-manager-load-balancing-azure/web-tier-diagram.png)

1. Önceki bölümde oluşturduğunuz uygulama ağ geçidi örneği, kaynak grubundan gidin.
2. Altında **ayarları**seçin **arka uç havuzları**ve ardından **Ekle** web katmanı arka uç havuzları ile ilişkilendirmek istediğiniz sanal makineleri eklemek için.
3. Üzerinde **arka uç havuzu ekleme** dikey penceresinde, arka uç havuzunun adı ve Havuzda bulunan makinelerin tüm IP adreslerini girin. Bu senaryoda, biz sanal makinelerin iki arka uç sunucu havuzu bağlanırsınız.

  ![Uygulama ağ geçidi "arka uç havuzu Ekle" dikey](./media/traffic-manager-load-balancing-azure/s2-appgw-add-bepool.png)

4. Altında **ayarları** uygulama ağ geçidi için seçin **kuralları**ve ardından **yol temelli** bir kural eklemek için düğmesi.

  ![Uygulama ağ geçidi kuralları "Yoluna bağlı" düğmesi](./media/traffic-manager-load-balancing-azure/s2-appgw-add-pathrule.png)

5. Üzerinde **yol tabanlı Kuralı Ekle** dikey penceresinde, aşağıdaki bilgileri sağlayarak kuralını yapılandırın.

   Temel ayarlar:

   + **Ad**: Portalı'nda erişilebilir olan kural kolay adı.
   + **Dinleyici**: kural için kullanılan dinleyicisi.
   + **Varsayılan arka uç havuzu**: varsayılan kuralı ile kullanılacak arka uç havuzu.
   + **Varsayılan HTTP ayarları**: varsayılan kuralı ile kullanılacak HTTP ayarları.

   Yol tabanlı kurallar:

   + **Ad**: yol tabanlı kural kolay adı.
   + **Yollar**: trafiği iletmek için kullanılan yol kuralı.
   + **Arka uç havuzu**: Bu kural ile kullanılacak arka uç havuzu.
   + **HTTP ayarı**: Bu kural ile kullanılacak HTTP ayarları.

   > [!IMPORTANT]
   > Yollar: Geçerli yolları başlamalıdır "/". Joker karakter "\*" yalnızca sonunda izin verilir. Geçerli örnekler /xyz, /xyz\*, veya /xyz/\*.

   ![Uygulama ağ geçidi "Yol tabanlı Kuralı Ekle" dikey penceresi](./media/traffic-manager-load-balancing-azure/s2-appgw-pathrule-blade.png)

### <a name="step-3-add-application-gateways-to-the-traffic-manager-endpoints"></a>3. adım: uygulama ağ geçitleri trafik Yöneticisi uç noktaları ekleme

Bu senaryoda, farklı bölgelerde bulunur (önceki adımlarda yapılandırılmış gibi) uygulama ağ geçitleri trafik Yöneticisi bağlı. Uygulama ağ geçitleri yapılandırılır, sonraki adım bunları Traffic Manager profilinize bağlamaktır.

1. Traffic Manager profilinizin açın. Bunu yapmak için kaynak grubu veya trafik Yöneticisi profilinin adını arayın Ara **tüm kaynakları**.
2. Sol bölmede seçin **uç noktaları**ve ardından **Ekle** bir uç noktası eklemek için.

  ![Trafik Yöneticisi uç noktaları "Ekle" düğmesi](./media/traffic-manager-load-balancing-azure/s3-tm-add-endpoint.png)

3. Üzerinde **uç nokta ekleme** dikey penceresinde, aşağıdaki bilgileri girerek bir uç nokta oluşturun:

  * **Tür**: Yük Dengeleme uç türünü seçin. Bu senaryoda seçin **Azure uç noktası** biz önceden yapılandırılmış uygulama ağ geçidi örnekleri bağlandığınız için.
  * **Ad**: uç nokta adını girin.
  * **Hedef kaynak türünün**: seçin **genel IP adresi** ve ardından, **hedef kaynak**, daha önce yapılandırılan uygulama ağ geçidinin genel IP adresi seçin.

   ![Trafik Yöneticisi "uç nokta Ekle" dikey](./media/traffic-manager-load-balancing-azure/s3-tm-add-endpoint-blade.png)

4. Traffic Manager profilinizin DNS ile erişerek kurulumunuzu sınayabilirsiniz artık (Bu örnekte: TrafficManagerScenario.trafficmanager.net). İsteği yeniden gönderin, ortaya çıkarmak veya VM'ler ve farklı bölgelerde oluşturulan web sunucuları aşağı Getir ve kurulumunuzu test etmek için trafik Yöneticisi profili ayarlarını değiştirin.

### <a name="step-4-create-a-load-balancer"></a>4. adım: bir yük dengeleyici oluşturma

Bu senaryoda, yük dengeleyici web katmanı bağlantılarından veritabanlarına yüksek oranda kullanılabilirlik kümesi içinde dağıtır.

Yüksek kullanılabilirlik veritabanı Kümenizin SQL Server AlwaysOn kullanıyorsanız başvurmak [bir veya daha fazla her zaman üzerinde kullanılabilirlik grubu dinleyicileri yapılandırma](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) adım adım yönergeler için.

Bir iç yük dengeleyici yapılandırma hakkında daha fazla bilgi için bkz: [Azure portalında bir iç yük dengeleyici oluşturma](../load-balancer/load-balancer-get-started-ilb-arm-portal.md).

1. Sol bölmede, Azure portal'ı tıklatın **yeni** > **ağ** > **yük dengeleyici**.
2. Üzerinde **oluşturma yük dengeleyici** dikey penceresinde, yük dengeleyici için bir ad seçin.
3. Ayarlama **türü** için **iç**, uygun sanal ağ ve alt ağ yük dengeleyici bulunması için seçin.
4. Altında **IP adresi ataması**, şunlardan birini seçin **dinamik** veya **statik**.
5. Altında **kaynak grubu**, yük dengeleyici için kaynak grubunu seçin.
6. Altında **konumu**, yük dengeleyici için uygun bir bölge seçin.
7. Tıklatın **oluşturma** yük dengeleyici oluşturmak için.

#### <a name="connect-a-back-end-database-tier-to-the-load-balancer"></a>Yük Dengeleyici arka uç veritabanı katmanı Bağlan

1. Kaynak grubundan önceki adımlarda oluşturulan yük dengeleyici bulunamıyor.
2. Altında **ayarları**, tıklatın **arka uç havuzları**ve ardından **Ekle** bir arka uç havuzu ekleme.

  ![Yük Dengeleyici "arka uç havuzu Ekle" dikey](./media/traffic-manager-load-balancing-azure/s4-ilb-add-bepool.png)

3. Üzerinde **arka uç havuzu ekleme** dikey penceresinde, arka uç havuzunun adını girin.
4. Makineleri tek tek veya kullanılabilirlik kümesi arka uç havuzuna ekleyin.

#### <a name="configure-a-probe"></a>Bir araştırma yapılandırmanız

1. İçinde yük dengeleyici altındaki **ayarları**seçin **yoklamaları**ve ardından **Ekle** bir araştırma eklemek için.

 ![Yük Dengeleyici "Ekle araştırma" dikey penceresi](./media/traffic-manager-load-balancing-azure/s4-ilb-add-probe.png)

2. Üzerinde **Ekle araştırma** dikey penceresinde, araştırma için bir ad girin.
3. Seçin **Protokolü** araştırma için. Bir veritabanı için bir HTTP araştırmasını yerine bir TCP araştırması isteyebilirsiniz. Yük Dengeleyici araştırmalar hakkında daha fazla bilgi için bkz [anlayın yük dengeleyici araştırmalar](../load-balancer/load-balancer-custom-probe-overview.md).
4. Girin **bağlantı noktası** veritabanınızın araştırma erişmek için kullanılacak.
5. Altında **aralığı**, sık sık uygulama araştırma nasıl belirtin.
6. Altında **sağlıksız durum eşiği**, ortaya çıkması gereken sürekli araştırma hataları sağlıksız olarak kabul edilmesi arka uç VM sayısını belirtin.
7. Tıklatın **Tamam** araştırma oluşturulamadı.

#### <a name="configure-the-load-balancing-rules"></a>Yük Dengeleme kurallarını yapılandırma

1. Altında **ayarları** , yük dengeleyici seçin **Yük Dengeleme kuralları**ve ardından **Ekle** bir kural oluşturmak için.
2. Üzerinde **Ekle Yük Dengeleme kuralını** dikey penceresinde girin **adı** Yük Dengeleme kuralı için.
3. Seçin **ön uç IP adresi** yük dengeleyicinin **Protokolü**, ve **bağlantı noktası**.
4. Altında **arka uç bağlantı noktası**, arka uç havuzunda kullanılacak bağlantı noktasını belirtin.
5. Seçin **arka uç havuzu** ve **araştırma** kural uygulamak için önceki adımlarda oluşturulmuş.
6. Altında **oturum kalıcılığını**, nasıl kalıcı hale getirmek için oturumları istediğinizi seçin.
7. Altında **boş durma zaman aşımlarını**, boşta kalma zaman aşımından önce dakika sayısını belirtin.
8. Altında **kayan IP**, şunlardan birini seçin **devre dışı** veya **etkin**.
9. Kuralı oluşturmak için **Tamam**'a tıklayın.

### <a name="step-5-connect-web-tier-vms-to-the-load-balancer"></a>5. adım: web katmanı VM'ler yük dengeleyiciye bağlanın.

Şimdi biz web katmanı Vm'leriniz için herhangi bir veritabanı bağlantısı üzerinde çalışan uygulamalar, IP adresi ve yük dengeleyici ön uç bağlantı noktasını yapılandırın. Bu yapılandırma, bu Vm'lerde çalışan uygulamaları özgüdür. Hedef IP adresi ve bağlantı noktası yapılandırmak için uygulama belgelerine başvurun. Azure portalında ön uç IP adresini bulmak için ön uç IP havuzuna geçin **yük dengeleyici ayarları** dikey.

![Yük Dengeleyici "Ön uç IP havuzu" Gezinti Bölmesi](./media/traffic-manager-load-balancing-azure/s5-ilb-frontend-ippool.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Trafik Yöneticisi'nin genel bakış](traffic-manager-overview.md)
* [Uygulama ağ geçidi'ne genel bakış](../application-gateway/application-gateway-introduction.md)
* [Azure Load Balancer’a genel bakış](../load-balancer/load-balancer-overview.md)
