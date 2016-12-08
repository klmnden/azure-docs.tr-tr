---
title: "Azure Güvenlik Merkezi&quot;nde güvenlik durumunu izleme | Microsoft Belgeleri"
description: "Bu makale, Azure Güvenlik Merkezi&quot;nde izleme özelliklerini kullanmaya başlamanıza yardımcı olur."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 3bd5b122-1695-495f-ad9a-7c2a4cd1c808
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/22/2016
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: a31a1b7909ea7440780c9a7af4cb908c9aa6a449
ms.openlocfilehash: 5852808ad0f5cf02908df7ff969ab2106d9f5abd


---
# <a name="security-health-monitoring-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik durumunu izleme
Bu makale, ilkelerle uyumluluğu izlemek için Azure Güvenlik Merkezi'ndeki izleme özelliklerini kullanmanıza yardımcı olur.

## <a name="what-is-security-health-monitoring"></a>Güvenlik durumunu izleme nedir?
Genellikle izlemeyi, izleme ve bir olayın gerçekleşmesini bekleyip duruma tepki verme olarak ele alırız. Güvenliği izleme, kuruluş standartlarıyla veya en iyi uygulamalarla uyumlu olmayan sistemleri tanımlamak için kaynaklarınızı denetleyen öngörülü bir stratejiye sahip olma anlamına gelir.

## <a name="monitoring-security-health"></a>Güvenlik durumunu izleme
Bir aboneliğin kaynakları için [güvenlik ilkelerini](security-center-policies.md) etkinleştirmenizin ardından, Güvenlik Merkezi olası güvenlik açıklarını tanımlamak amacıyla kaynaklarınızın güvenliğini analiz eder. Ağ yapılandırmanız ile ilgili bilgiler hemen kullanımınıza sunulur. Sanal makine yapılandırması hakkındaki bilgilerin (güvenlik güncelleştirme durumu ve işletim sistemi yapılandırması gibi) kullanılabilir hale gelmesi bir saat veya daha uzun sürebilir. **Kaynak Güvenlik Durumu** dikey penceresinde kaynaklarınızın güvenlik durumunu ve sorunları görüntüleyebilirsiniz. Ayrıca, bu sorunların bir listesini **Öneriler** dikey penceresinde görüntüleyebilirsiniz.

Önerileri uygulama hakkında daha fazla bilgi için [Azure Güvenlik Merkezi'nde güvenlik önerilerini uygulama](security-center-recommendations.md)'yı okuyun.

**Kaynak güvenlik durumu** kutucuğunda kaynaklarınızın güvenlik durumunu izleyebilirsiniz. Aşağıdaki örnekte, yüksek ve orta önem derecesine sahip olan ve dikkat gerektiren birkaç sorun görebilirsiniz. Etkinleştirilmiş güvenlik ilkeleri, izlenen denetim türlerini etkiler.

![Kaynaklar güvenlik durumu kutucuğu](./media/security-center-monitoring/security-center-monitoring-fig1-new4.png)

Güvenlik Merkezi, ilgilenilmesi gereken bir güvenlik açığı (eksik güvenlik güncelleştirmelerine sahip bir sanal makine veya [ağ güvenlik grubu](../virtual-network/virtual-networks-nsg.md) olmayan bir alt ağ gibi) tanımlarsa bu güvenlik açığı burada listelenir.

### <a name="monitor-virtual-machines"></a>Sanal makineleri izleme
**Kaynakların güvenlik durumu** kutucuğunda **Sanal makineler**’e tıkladığınızda, açılan **Sanal makineler** dikey penceresi, aşağıdaki ekran görüntüsünde gösterildiği gibi ekleme ve önleme adımları hakkında daha fazla ayrıntı ve Güvenlik Merkezi’nin izlediği tüm sanal makinelerin bir listesini sağlar.

![Sanal makine tarafından eksik sistem güncelleştirmesi](./media/security-center-monitoring/security-center-monitoring-fig2-ga.png)

* Ekleme adımları
* Sanal makine önerileri
* Sanal makineler

Her bölümde, sorunu çözmek üzere önerilen adımlarla ilgili daha fazla ayrıntı görmek için ayrı ayrı seçenekler belirleyebilirsiniz. Aşağıdaki bölümlerde, bu alanlar daha ayrıntılı açıklanmaktadır.

#### <a name="monitoring-recommendations"></a>İzleme önerileri
Bu bölüm, veri toplama ve bunların geçerli durumu için başlatılan sanal makinelerin toplam sayısını gösterir. Tüm sanal makinelerde veri toplama başlatıldıktan sonra, sanal makineler Güvenlik Merkezi güvenlik ilkelerini almaya hazır olurlar. Bu girdiye tıkladığınızda, **Veri toplama yükleme durumu** dikey penceresi açılır ve aşağıdaki ekran görüntüsünde gösterildiği gibi **YÜKLEME DURUMU** sütununda sanal makinelerin adlarını ve veri toplamanın geçerli durumunu görebilirsiniz.

![Sanal makinelerin başlatma durumu](./media/security-center-monitoring/security-center-monitoring-fig3-ga.png)

#### <a name="virtual-machine-recommendations"></a>Sanal makine önerileri
Bu bölümde, Azure Güvenlik Merkezi’nin izlediği [her bir sanal makine için bir öneri](security-center-virtual-machine-recommendations.md) kümesi bulunur. İlk sütunda öneriler listelenmiştir. İkinci sütunda, bu önerinin etkilediği sanal makinelerin toplam sayısı gösterilmiştir. Üçüncü sütunda, aşağıdaki ekran görüntüsünde gösterildiği gibi sorunun önem derecesi belirtilmiştir.

![Sanal makine önerileri](./media/security-center-monitoring/security-center-monitoring-fig4-ga.png)

> [!NOTE]
> **Ağ topolojisi listesinin** **Ağ Durumu** dikey penceresinde yalnızca en az bir genel uç nokta içeren sanal makineler gösterilir.
> 
> 

Her öneride, tıkladıktan sonra gerçekleştirebileceğiniz bir eylemler kümesi bulunur. Örneğin, **Eksik sistem güncelleştirmeleri**’ne tıklarsanız **Eksik sistem güncelleştirmeleri** dikey penceresi açılır. Yamaları eksik olan sanal makineleri ve aşağıdaki ekran görüntüsünde gösterildiği gibi eksik güncelleştirmenin önem derecesini listeler.

![Sanal makineler için eksik sistem güncelleştirmeleri](./media/security-center-monitoring/security-center-monitoring-fig5-ga.png)

**Eksik sistem güncelleştirmeleri** dikey penceresi, şu bilgileri içeren bir tablo gösterir:

* **SANAL MAKİNE**: Güncelleştirmeleri eksik olan sanal makinenin adı.
* **SİSTEM GÜNCELLEŞTİRMELERİ**: Eksik sistem güncelleştirmelerinin sayısı.
* **SON TARAMA ZAMANI**: Güvenlik Merkezi'nin güncelleştirmeler için sanal makineyi son tarama zamanı.
* **DURUM**: Önerinin geçerli durumu:
  * **Açık**: Öneri henüz ele alınmadı.
  * **Devam Ediyor**: Öneri şu anda bu kaynaklara uygulanıyor; sizin bir eylem yapmanıza gerek yok.
  * **Çözüldü**: Öneri daha önce tamamlandı. (Sorun çözüldüğünde girdi soluklaşır).
* **ÖNEM DERECESİ**: Belirli bir önerinin önem derecesini açıklar:
  * **Yüksek**: Anlamlı bir kaynakta (uygulama, sanal makine veya ağ güvenlik grubu) güvenlik açığı var ve ilgilenilmesi gerekiyor.
  * **Orta**: Bir işlemi tamamlamak veya bir güvenlik açığını ortadan kaldırmak için kritik olmayan adımlar veya ek adımlar gerekiyor.
  * **Düşük**: Bir güvenlik açığının ele alınması gerekiyor ancak hemen ilgilenilmesi gerekmiyor. (Varsayılan olarak, düşük öneriler sunulmaz ancak bunları görüntülemek istiyorsanız düşük öneriler filtresini kullanabilirsiniz.)

Öneri ayrıntılarını görüntülemek için sanal makinenin adına tıklayın. Aşağıdaki ekran görüntüsünde gösterildiği gibi güncelleştirmelerin listesiyle birlikte bu sanal makine için yeni bir dikey pencere açılır.

![Belirli bir sanal makine için eksik sistem güncelleştirmeleri](./media/security-center-monitoring/security-center-monitoring-fig6-ga.png)

> [!NOTE]
> Buradaki güvenlik önerileri, **Öneriler** dikey penceresindekilerle aynıdır. Önerileri çözümleme hakkında daha fazla bilgi için [Azure Güvenlik Merkezi'nde güvenlik önerilerini uygulama](security-center-recommendations.md)'yı okuyun. Bu yalnızca sanal makineler için değil, **Kaynak Durumu** kutucuğunda kullanılabilen tüm kaynaklar için geçerlidir.
> 
> 

#### <a name="virtual-machines-section"></a>Sanal makineler bölümü
Sanal makineler bölümü, tüm sanal makineler ve öneriler için bir genel bakış sağlar. Her sütun, aşağıdaki ekran görüntüsünde gösterildiği gibi bir dizi öneriyi temsil eder:

![Tüm sanal makinelere ve önerilere genel bakış](./media/security-center-monitoring/security-center-monitoring-fig7-ga.png)

Her önerinin altında görüntülenen simge, ilgilenilmesi gereken sanal makineleri ve önerinin türünü hızla tanımlamanıza yardımcı olur.

Önceki örnekte, bir sanal makinenin uç nokta koruması ile ilgili kritik bir önerisi vardır. Sanal makine hakkında daha fazla bilgi edinmek için tıklayın. Aşağıdaki ekran görüntüsünde gösterildiği gibi açılan yeni bir dikey pencere bu sanal makineyi temsil eder.

![Sanal makine güvenlik ayrıntıları](./media/security-center-monitoring/security-center-monitoring-fig8-ga.png)

Bu dikey pencere, sanal makine için güvenlik ayrıntılarını içerir. Bu dikey pencerenin en altında, önerilen eylemi ve sorunların önem derecesini görebilirsiniz.

#### <a name="cloud-services-preview-section"></a>Bulut hizmetleri (Önizleme) bölümü
Bulut hizmetlerinin sistem durumu, sanal makinenin **Güvenlik Durumu** kutucuğuna dahil edilir. İşletim sistemi sürümü güncel olmadığında aşağıdaki ekran görüntüsünde gösterildiği gibi bir öneri oluşturulur:

![Bulut hizmetlerinin sistem durumu](./media/security-center-monitoring/security-center-monitoring-fig8-new2.png)

İşletim sistemi sürümünü güncelleştirmek için önerideki adımları uygulamanız gerekir. Örneğin, WebRole1 (web uygulamanızı IIS’e otomatik olarak dağıtarak Windows Server çalıştırır) veya WorkerRole1 (web uygulamanızı IIS’e otomatik olarak dağıtarak Windows Server çalıştırır) satırlarında kırmızı uyarıya tıklarsanız aşağıdaki ekran görüntüsünde gösterildiği gibi bu öneriye ilişkin daha fazla ayrıntı içeren yeni bir dikey pencere açılır:

![Bulut hizmeti ayrıntıları](./media/security-center-monitoring/security-center-monitoring-fig8-new3.png)

Bu öneriyle ilgili daha kesin bir açıklama görmek için **AÇIKLAMA** sütunu altındaki **İşletim sistemi sürümünü güncelleştir**’e tıklayın. **İşletim sistemi sürümünü güncelleştir (Önizleme)** dikey penceresi daha fazla ayrıntı ile açılır.

![Bulut hizmeti önerileri](./media/security-center-monitoring/security-center-monitoring-fig8-new4.png)  

### <a name="monitor-virtual-networks"></a>Sanal ağları izleme
**Kaynak güvenlik durumu** kutucuğunda **Ağ**'a tıkladığınızda, **Ağ** dikey penceresi aşağıdaki ekran görüntüsünde gösterildiği gibi daha fazla ayrıntıyla açılır:

![Ağ dikey penceresi](./media/security-center-monitoring/security-center-monitoring-fig9-new3.png)

#### <a name="networking-recommendations"></a>Ağ önerileri
Bu dikey pencere, sanal makinelerin kaynak durumu bilgilerine benzer şekilde dikey pencerenin en üstünde sorunların bir özet listesini ve en altında izlenen ağların bir listesini sağlar.

Ağ durumu döküm bölümü, olası güvenlik sorunlarını listeler ve [öneriler](security-center-network-recommendations.md) sunar. Olası sorunlar şunları içerebilir:

* Yeni Nesil Güvenlik Duvarı (NGFW) yüklü değil
* Alt ağlardaki ağ güvenlik grupları etkin değil
* Sanal makinelerdeki ağ güvenlik grupları etkin değil
* Genel dış uç nokta aracılığıyla harici erişimi kısıtlama
* İnternet’e yönelik sorunsuz çalışan uç noktalar

Bir öneriye tıkladığınızda, aşağıdaki örnekte gösterildiği gibi öneri hakkında daha fazla ayrıntı içeren yeni bir dikey pencere açılır.

![Ağ dikey penceresinde önerinin ayrıntıları](./media/security-center-monitoring/security-center-monitoring-fig9-ga.png)

Bu örnekte, **Alt Ağlar için Eksik Ağ Güvenlik Gruplarını Yapılandırma** dikey penceresinde alt ağların ve ağ güvenlik grubu koruması eksik olan sanal makinelerin bir listesi bulunur. Ağ güvenlik grubunu uygulamak istediğiniz alt ağa tıklarsanız başka bir dikey pencere açılır.

**Ağ güvenlik grubunu seçme** dikey penceresinde alt ağ için en uygun ağ güvenlik grubunu seçebilir veya yeni bir ağ güvenlik grubu oluşturabilirsiniz.

#### <a name="internet-facing-endpoints-section"></a>İnternet'e yönelik uç noktalar bölümü
**İnternet'e yönelik uç noktalar** bölümünde, hâlihazırda İnternet'e yönelik uç noktayla yapılandırılmış sanal makineleri ve bunların geçerli durumlarını görebilirsiniz.

![İnternet'e yönelik uç nokta ve durumu ile yapılandırılan sanal makineler](./media/security-center-monitoring/security-center-monitoring-fig10-ga.png)

Bu tabloda sanal makineyi temsil eden uç noktanın adı, İnternet'e yönelik IP adresi, ağ güvenlik grubunun ve NGFW'nun geçerli önem derecesi durumu bulunur. Tablo, önem derecesine göre sıralanır:

* Kırmızı (en üstte): Yüksek önceliklidir ve hemen ilgilenilmesi gerekir
* Turuncu: Orta önceliklidir ve olabildiğince yakın zamanda ilgilenilmesi gerekir
* Yeşil (sonuncu): Sorunsuz çalışma durumu

#### <a name="networking-topology-section"></a>Ağ topolojisi bölümü
**Ağ topolojisi** bölümünde, aşağıdaki ekran görüntüsünde gösterildiği gibi kaynakların hiyerarşik bir görünümü bulunur:

![Ağ topolojisi bölümünde kaynakların hiyerarşik görünümü](./media/security-center-monitoring/security-center-monitoring-fig121-new4.png)

Bu tablo, önem derecesine göre sıralanır (sanal makineler ve alt ağlar):

* Kırmızı (en üstte): Yüksek önceliklidir ve hemen ilgilenilmesi gerekir
* Turuncu: Orta önceliklidir ve olabildiğince yakın zamanda ilgilenilmesi gerekir
* Yeşil (sonuncu): Sorunsuz çalışma durumu

Bu topoloji görünümünde, ilk düzeyde [sanal ağlar](../virtual-network/virtual-networks-overview.md), [sanal ağ geçitleri](../vpn-gateway/vpn-gateway-site-to-site-create.md) ve [sanal ağlar (klasik)](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) bulunur. İkinci düzeyde alt ağlar ve üçüncü düzeyde de bu alt ağlara ait sanal makineler bulunur. Aşağıdaki örnekte gösterildiği gibi sağ sütunda bu kaynaklara yönelik ağ güvenlik grubunun geçerli durumu bulunur:

![Ağ topolojisi bölümünde ağ güvenlik grubunun durumu](./media/security-center-monitoring/security-center-monitoring-fig12-ga.png)

Bu dikey pencerenin en altında, daha önce açıklanana benzer şekilde bu sanal makine için öneriler bulunur. Daha fazla bilgi edinmek ya da gerekli güvenlik denetimini veya yapılandırmasını uygulamak için bir öneriye tıklayabilirsiniz.

### <a name="monitor-data"></a>Veri izleme
**Kaynak güvenlik durumu** kutucuğunda **Veriler**'e tıkladığınızda **SQL** dikey penceresi, denetim ve saydam veri şifrelemesinin etkinleştirilmemiş olması gibi sorunlara yönelik önerilerle birlikte açılır. Ayrıca, veritabanının genel sağlık durumu için [öneriler](security-center-sql-service-recommendations.md) içerir.

![SQL kaynak durumu](./media/security-center-monitoring/security-center-monitoring-fig13-ga.png)

Herhangi bir öneriye tıklayabilir ve sorunu çözmek için yapılacak diğer eylemlerle ilgili daha fazla ayrıntı görebilirsiniz. Aşağıdaki örnekte, **Veritabanı Denetimi etkinleştirilmemiş** önerisinin genişletilmiş hali gösterilmiştir.

![SQL önerisi hakkındaki ayrıntılar](./media/security-center-monitoring/security-center-monitoring-fig14-ga.png)

**SQL veritabanlarında Denetimi etkinleştirme** dikey penceresinde aşağıdaki bilgiler bulunur:

* SQL veritabanlarının bir listesi
* Bulundukları sunucu
* Bu ayarın sunucudan devralınmış olduğuna veya bu veritabanında benzersiz olduğuna dair bilgiler
* Geçerli durum
* Sorunun önem derecesi

Bu öneriyle ilgilenmek için veritabanına tıkladığınızda, **Denetim ve Tehdit algılama** dikey penceresi aşağıdaki ekran görüntüsünde gösterildiği gibi açılır.

![Denetim ve Tehdit algılama dikey penceresi](./media/security-center-monitoring/security-center-monitoring-fig15-ga.png)

Denetimi etkinleştirmek için **Denetim** seçeneğinin altında **AÇIK**'ı seçin.

### <a name="monitor-applications"></a>Uygulamaları izleme
Azure iş yükünüzün, sunulan web bağlantı noktaları (TCP bağlantı noktaları 80 ve 443) ile [sanal makinelerde](../azure-resource-manager/resource-manager-deployment-model.md) (Azure Resource Manager ile oluşturulmuştur) bulunan uygulamaları varsa Güvenlik Merkezi, olası güvenlik sorunlarını tanımlamak ve düzeltme adımları önermek için bunları izleyebilir. **Uygulamalar** kutucuğuna tıkladığınızda, **Önleme adımları** bölümünde bir dizi öneriyle birlikte **Uygulamalar** dikey penceresi açılır. Ayrıca, aşağıdaki ekran görüntüsünde gösterildiği gibi ana bilgisayar/sanal IP başına uygulama dökümünü gösterir.

![Uygulamaların güvenlik durumu](./media/security-center-monitoring/security-center-monitoring-fig16-ga.png)

Diğer önerilerde yaptığınız gibi, sorun ve sorunun nasıl düzeltileceği hakkında daha fazla ayrıntı görmek için bir öneriye tıklayabilirsiniz. Aşağıdaki şekilde gösterilen örnekte, güvenli olmayan bir web uygulaması olarak tanımlanan bir uygulama bulunur. Güvenli kabul edilmeyen uygulamayı seçtiğinizde, aşağıdaki seçenek kullanılabilir durumda olacak şekilde başka bir dikey pencere açılır:

![Güvenli olmayan bir uygulama hakkındaki ayrıntılar](./media/security-center-monitoring/security-center-monitoring-fig17-ga.png)

Bu dikey pencerede bu uygulamaya ilişkin tüm önerilerin bir listesi bulunur. **Web uygulaması güvenlik duvarı ekleyin** önerisine tıkladığınızda aşağıdaki ekran görüntüsünde gösterildiği gibi bir web uygulaması güvenlik duvarını (WAF) yüklemenize yönelik seçenekleri içeren **Web Uygulaması Güvenlik Duvarı Ekleme** dikey penceresi açılır.

![Web Uygulaması Güvenlik Duvarı Ekleme iletişim kutusu](./media/security-center-monitoring/security-center-monitoring-fig18-ga.png)

## <a name="see-also"></a>Ayrıca bkz.
Bu makalede, Azure Güvenlik Merkezi'nde izleme işlevlerini nasıl kullanacağınız hakkında bilgi edindiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md): Azure Güvenlik Merkezi'nde güvenlik ayarlarını yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md): Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md): İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md): Hizmet kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik Blogu](http://blogs.msdn.com/b/azuresecurity/): Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.




<!--HONumber=Nov16_HO4-->


