---
title: Azure Güvenlik Merkezi'nde güvenliği izleme | Microsoft Docs
description: Bu makale, Azure Güvenlik Merkezi'nde izleme özelliklerini kullanmaya başlamanıza yardımcı olur.
services: security-center
documentationcenter: na
author: terrylan
manager: mbaldwin
editor: ''
ms.assetid: 3bd5b122-1695-495f-ad9a-7c2a4cd1c808
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/06/2018
ms.author: yurid
ms.openlocfilehash: 330a12f851ef0191adc4dc46102b798f1b752589
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="security-health-monitoring-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik durumunu izleme
Bu makale, ilkelerle uyumluluğu izlemek için Azure Güvenlik Merkezi'ndeki izleme özelliklerini kullanmanıza yardımcı olur.

## <a name="what-is-security-health-monitoring"></a>Güvenlik durumunu izleme nedir?
Genellikle izlemeyi, izleme ve bir olayın gerçekleşmesini bekleyip duruma tepki verme olarak ele alırız. Güvenliği izleme, kuruluş standartlarıyla veya en iyi uygulamalarla uyumlu olmayan sistemleri tanımlamak için kaynaklarınızı denetleyen öngörülü bir stratejiye sahip olma anlamına gelir.

## <a name="monitoring-security-health"></a>Güvenlik durumunu izleme
Bir aboneliğin kaynakları için [güvenlik ilkelerini](security-center-policies.md) etkinleştirmenizin ardından, Güvenlik Merkezi olası güvenlik açıklarını tanımlamak amacıyla kaynaklarınızın güvenliğini analiz eder. Ağ yapılandırmanız ile ilgili bilgiler hemen kullanımınıza sunulur. Aracının yüklü olduğu VM'ler ve bilgisayarların sayısına bağlı olarak, güvenlik güncelleştirmesi durumu ve işletim sistemi yapılandırması gibi bilgisayar yapılandırma ayarları ve VM’ler hakkında bilgi toplamak bir saat veya daha fazla sürebilir. **Önleme** bölümünde kaynaklarınızın güvenlik durumunu ve sorunları görüntüleyebilirsiniz. Ayrıca, bu sorunların bir listesini **Öneriler** kutucuğunda görüntüleyebilirsiniz.

Önerileri uygulama hakkında daha fazla bilgi için [Azure Güvenlik Merkezi'nde güvenlik önerilerini uygulama](security-center-recommendations.md)'yı okuyun.

**Önleme** bölümünde kaynaklarınızın güvenlik durumunu izleyebilirsiniz. Aşağıdaki örnekte, her bir kaynağın kutucuğunda (İşlem, Ağ, Depolama ve veriler ile Uygulama) tanımlanan toplam sorun sayısının olduğunu görebilirsiniz.

![Kaynaklar güvenlik durumu kutucuğu](./media/security-center-monitoring/security-center-monitoring-fig1-newUI-2017.png)


### <a name="monitor-compute"></a>İzleyici işlemi
**Bilgisayar** kutucuğuna tıkladığınızda, üç sekme görünür:

- **Genel Bakış**: izleme ve öneriler.
- **VM’ler ve Bilgisayarlar**: Tüm sanal makineleri ve geçerli güvenlik durumlarını listeler.
- **Cloud Services**: Güvenlik Merkezi tarafından izlenen tüm web ve çalışan rollerinin listesi.

![Sanal makine tarafından eksik sistem güncelleştirmesi](./media/security-center-monitoring/security-center-monitoring-fig1-sep2017.png)

Her sekmede birden fazla bölüm olabilir ve her bölümde, sorunu çözmek üzere önerilen adımlarla ilgili daha fazla ayrıntı görmek için ayrı ayrı seçenekler belirleyebilirsiniz.

#### <a name="monitoring-recommendations"></a>İzleme önerileri
Bu bölüm, otomatik hazırlaması başlatılmış sanal makinelerin ve bilgisayarların toplam sayısıyla bunların geçerli durumlarını gösterir. Bu örnekte şu öneri mevcuttur: **Aracı sistem durumu sorunlarını izleme**.  Bu öneriyi seçin.

![Aracı sistem durumu sorunlarını izleme](./media/security-center-monitoring/security-center-monitoring-fig1-new003-2017.png)

**Aracı sistem durumu sorunlarını izleme** açılır. Güvenlik Merkezi'nin başarılı bir şekilde izleyemediği VM'ler ve bilgisayarlar listelenir. Ayrıntılı bilgi için bir VM'yi veya bilgisayarı seçin. **İZLEME DURUMU**, Güvenlik Merkezi’nin izlemeyi neden yapamadığını belirtir. **İZLEME DURUMU** değerleri, açıklamaları ve çözüm adımlarının listesi için bkz. [Güvenlik Merkezi sorun giderme kılavuzu](security-center-troubleshooting-guide.md#monitoring-agent-health-issues).

#### <a name="recommendations"></a>Öneriler
Bu bölümde, Azure Güvenlik Merkezi’nin izlediği [her bir sanal makine ve bilgisayar için bir öneri](security-center-virtual-machine-recommendations.md) kümesi bulunur. İlk sütunda öneriler listelenmiştir. İkinci sütunda, bu önerinin etkilediği sanal makinelerin ve bilgisayarların toplam sayısı gösterilmiştir. Üçüncü sütunda, aşağıdaki ekran görüntüsünde gösterildiği gibi sorunun önem derecesi belirtilmiştir.

![Sanal makine önerileri](./media/security-center-monitoring/security-center-monitoring-fig2-sep2017.png)

> [!NOTE]
> **Ağ topolojisi listesinin** **Ağ Durumu** penceresinde yalnızca en az bir genel uç nokta içeren sanal makineler gösterilir.
>

Her öneride, tıkladıktan sonra gerçekleştirebileceğiniz bir eylemler kümesi bulunur. Örneğin, **Eksik sistem güncelleştirmeleri**’ne tıklarsanız, eksik düzeltme eklerine sahip sanal makineler ve bilgisayarlar ile eksik güncelleştirmelerin önem derecesi, aşağıdaki ekran görüntüsündeki gibi bir liste halinde görüntülenir:

![Sanal makineler için eksik sistem güncelleştirmeleri](./media/security-center-monitoring/security-center-monitoring-fig9-sep2017.png)

**Eksik sistem güncelleştirmeleri**, kritik güncelleştirmelerin biri Windows ve diğeri de Linux için olmak üzere iki grafik biçiminde özetini sunar. İkinci bölümde, aşağıdaki bilgileri içeren bir tablo vardır:

* **AD**: Eksik güncelleştirmenin adıdır.
* **VM’LER VE BİLGİSAYARLARIN SAYISI**: Bu güncelleştirmenin eksik olduğu VM'ler ve bilgisayarların toplam sayısı.
* **DURUM**: Önerinin geçerli durumu:
  * **Açık**: Öneri henüz ele alınmadı.
  * **Devam Ediyor**: Öneri şu anda bu kaynaklara uygulanıyor; sizin bir eylem yapmanıza gerek yok.
  * **Çözüldü**: Öneri daha önce tamamlandı. (Sorun çözüldüğünde girdi soluklaşır).
* **ÖNEM DERECESİ**: Belirli bir önerinin önem derecesini açıklar:
  * **Yüksek**: Anlamlı bir kaynakta (uygulama, sanal makine veya ağ güvenlik grubu) güvenlik açığı var ve ilgilenilmesi gerekiyor.
  * **Orta**: Bir işlemi tamamlamak veya bir güvenlik açığını ortadan kaldırmak için kritik olmayan adımlar veya ek adımlar gerekiyor.
  * **Düşük**: Bir güvenlik açığının ele alınması gerekiyor ancak hemen ilgilenilmesi gerekmiyor. (Varsayılan olarak, düşük öneriler sunulmaz ancak bunları görüntülemek istiyorsanız düşük öneriler filtresini kullanabilirsiniz.)

Öneri ayrıntılarını görüntülemek için listeden eksik güncelleştirmenin adına tıklayın.

![Belirli bir sanal makine için eksik sistem güncelleştirmeleri](./media/security-center-monitoring/security-center-monitoring-fig4-sep2017.png)

> [!NOTE]
> Buradaki güvenlik önerileri, **Öneriler** seçeneğindekilerle aynıdır. Önerileri çözümleme hakkında daha fazla bilgi için [Azure Güvenlik Merkezi'nde güvenlik önerilerini uygulama](security-center-recommendations.md)'yı okuyun. Bu yalnızca sanal makineler ve bilgisayarlar için değil, **Kaynak Durumu** kutucuğunda kullanılabilen tüm kaynaklar için geçerlidir.
>

#### <a name="unmonitored-vms"></a>İzlenmeyen VM’ler
VM, Microsoft Monitoring Agent uzantısını çalıştırmıyorsa, Güvenlik Merkezi tarafından izlenmez. VM’nin, OMS doğrudan aracısı veya SCOM aracısı gibi zaten yüklü bir yerel aracısı olabilir. Güvenlik Merkezi’nde tam olarak desteklenmediğinden, bu aracılara sahip VM’ler izlenmeyen olarak tanımlanır. Güvenlik Merkezi’nin tüm özelliklerinden tam olarak faydalanmak için, Microsoft Monitoring Agent uzantısı gereklidir.

Uzantıyı, zaten yüklü olan yerel aracıya ek olarak izlenmeyen VM’de yükleyebilirsiniz. İki aracıyı da aynı çalışma alanına bağlayarak aynı şekilde yapılandırın. Bu, Güvenlik Merkezi’nin Microsoft Monitoring Agent uzantısıyla etkileşim kurup veri toplamasını sağlar.  Microsoft Monitoring Agent uzantısını nasıl yükleyeceğiniz hakkında yönergeler için bkz. [VM uzantısını etkinleştir](../log-analytics/log-analytics-quick-collect-azurevm.md#enable-the-log-analytics-vm-extension).

Güvenlik Merkezi’nin otomatik hazırlama için başlatılan VM’leri ve bilgisayarları başarılı bir şekilde izleyememe nedenleri hakkında daha fazla bilgi edinmek için bkz. [Aracı durumu sorunlarını izleme](security-center-troubleshooting-guide.md#monitoring-agent-health-issues).

#### <a name="vms--computers-section"></a>VM’ler ve bilgisayarlar bölümü
Sanal makineler ve bilgisayarlar bölümü, tüm sanal makineler ve bilgisayarlarla ilgili önerilere bir genel bakış sağlar. Her sütun, aşağıdaki ekran görüntüsünde gösterildiği gibi bir dizi öneriyi temsil eder:

![Tüm sanal makinelere ve önerilere genel bakış](./media/security-center-monitoring/security-center-monitoring-fig5-sep2017.png)

Bu listede açıklandığı gibi bu listede gösterilen dört tür simge vardır:

![simge1](./media/security-center-monitoring/security-center-monitoring-icon1.png) Azure olmayan bilgisayar.

![simge2](./media/security-center-monitoring/security-center-monitoring-icon2.png) Azure Resource Manager sanal makinesi.

![simge3](./media/security-center-monitoring/security-center-monitoring-icon3.png) Azure Klasik sanal makinesi.

![simge4](./media/security-center-monitoring/security-center-monitoring-icon4.png) VM'ler yalnızca görüntülenen abonelik parçası olan çalışma alanından tanımlanır. Buna diğer aboneliklere ait ancak bu çalışma alanına bildirimde bulunan sanal makineler, SCOM doğrudan aracısıyla yüklenen ve kaynak kimliği olmayan VM’ler dahildir.

Her önerinin altında görüntülenen simge, ilgilenilmesi gereken sanal makineler ve bilgisayarların hızla tanımlamanıza yardımcı olur. Bu ekranda hangi seçenekleri göreceğinizi seçmek için **Filtre** seçeneğini de kullanabilirsiniz.

![Filtre](./media/security-center-monitoring/security-center-monitoring-fig6-sep2017.png)

Önceki örnekte, bir sanal makinenin uç nokta koruması ile ilgili kritik bir önerisi vardır. Sanal makine hakkında daha fazla bilgi edinmek için sanal makineye tıklayın:

![Sanal makine güvenlik ayrıntıları](./media/security-center-monitoring/security-center-monitoring-fig7-sep2017.png)

Burada sanal makine veya bilgisayar için güvenlik ayrıntılarını görürsünüz. En altta, sorunlar için önerilen eylemleri ve sorunların önem derecesini görebilirsiniz.

#### <a name="cloud-services-section"></a>Bulut hizmetleri bölümü
Bulut hizmetleri için işletim sistemi sürümü güncel olmadığında aşağıdaki ekran görüntüsünde gösterildiği gibi bir öneri oluşturulur:

![Bulut hizmetlerinin sistem durumu](./media/security-center-monitoring/security-center-monitoring-fig1-new006-2017.png)

Öneri gördüğünüz bir senaryoda (önceki örnek için geçerli bir durum değildir), işletim sistemi sürümünü güncelleştirmek için önerideki adımları izlemeniz gerekir. Bir güncelleştirme mevcut olduğunda uyarı alırsınız (sorunun önem derecesine bağlı olarak kırmızı veya turuncu). WebRole1 (web uygulamanızı IIS’e otomatik olarak dağıtarak Windows Server çalıştırır) veya WorkerRole1 (web uygulamanızı IIS’e otomatik olarak dağıtarak Windows Server çalıştırır) satırlarında bu uyarıya tıkladığınızda aşağıdaki ekran görüntüsünde gösterildiği gibi bu öneriye ilişkin daha fazla ayrıntı görürsünüz:

![Bulut hizmeti ayrıntıları](./media/security-center-monitoring/security-center-monitoring-fig8-new3.png)

Bu öneriyle ilgili daha kesin bir açıklama görmek için **AÇIKLAMA** sütunu altındaki **İşletim sistemi sürümünü güncelleştir**’e tıklayın.

![Bulut hizmeti önerileri](./media/security-center-monitoring/security-center-monitoring-fig8-new4.png)  

### <a name="monitor-virtual-networks"></a>Sanal ağları izleme
**Ağ** kutucuğuna tıkladığınızda, **Ağ** dikey penceresi aşağıdaki ekran görüntüsünde gösterildiği gibi daha fazla ayrıntıyla açılır:

![Ağ dikey penceresi](./media/security-center-monitoring/security-center-monitoring-fig9-new3.png)

#### <a name="networking-recommendations"></a>Ağ önerileri
Burada, sanal makinelerin kaynak durumu bilgilerine benzer şekilde en üstte sorunların bir özet listesini ve en altında izlenen ağların bir listesini sağlar.

Ağ durumu döküm bölümü, olası güvenlik sorunlarını listeler ve [öneriler](security-center-network-recommendations.md) sunar. Olası sorunlar şunları içerebilir:

* Yeni Nesil Güvenlik Duvarı (NGFW) yüklü değil
* Alt ağlardaki ağ güvenlik grupları etkin değil
* Sanal makinelerdeki ağ güvenlik grupları etkin değil
* Genel dış uç nokta aracılığıyla harici erişimi kısıtlama
* İnternet’e yönelik sorunsuz çalışan uç noktalar

Bir öneriye tıkladığınızda, aşağıdaki örnekte gösterildiği gibi öneri hakkında daha fazla ayrıntı görürsünüz.

![Ağ bölümünde önerinin ayrıntıları](./media/security-center-monitoring/security-center-monitoring-fig9-ga.png)

Bu örnekte, **Alt Ağlar için Eksik Ağ Güvenlik Gruplarını Yapılandırma** bölümünde alt ağların ve ağ güvenlik grubu koruması eksik olan sanal makinelerin bir listesi bulunur. Ağ güvenlik grubunu uygulamak istediğiniz alt ağa tıklarsanız **Ağ güvenlik grubunu seçme** bölümünü görürsünüz. Burada alt ağ için en uygun ağ güvenlik grubunu seçebilir veya yeni bir ağ güvenlik grubu oluşturabilirsiniz.

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

Bu topoloji görünümünde, ilk düzeyde [sanal ağlar](../virtual-network/virtual-networks-overview.md), [sanal ağ geçitleri](/vpn-gateway/vpn-gateway-site-to-site-create.md) ve [sanal ağlar (klasik)](/virtual-network/virtual-networks-create-vnet-classic-pportal.md) bulunur. İkinci düzeyde alt ağlar ve üçüncü düzeyde de bu alt ağlara ait sanal makineler bulunur. Aşağıdaki örnekte gösterildiği gibi sağ sütunda bu kaynaklara yönelik ağ güvenlik grubunun geçerli durumu bulunur:

![Ağ topolojisi bölümünde ağ güvenlik grubunun durumu](./media/security-center-monitoring/security-center-monitoring-fig12-ga.png)

Bu dikey pencerenin en altında, daha önce açıklanana benzer şekilde bu sanal makine için öneriler bulunur. Daha fazla bilgi edinmek ya da gerekli güvenlik denetimini veya yapılandırmasını uygulamak için bir öneriye tıklayabilirsiniz.

### <a name="monitor-storage--data"></a>Depolama ve verileri izleme

**Önleme** bölümünde **Depolama ve veriler**’e tıkladığınızda, **Veri Kaynakları** bölümünü SQL ve Depolama’ya yönelik önerilerle birlikte açılır. Ayrıca, veritabanının genel sağlık durumu için [öneriler](security-center-sql-service-recommendations.md) içerir. Depolama şifrelemesi hakkında daha fazla bilgi için [Azure Güvenlik Merkezi'ndeki Azure depolama hesabı için şifrelemeyi etkinleştirme](security-center-enable-encryption-for-storage-account.md) bölümünü okuyun.

![Veri Kaynakları](./media/security-center-monitoring/security-center-monitoring-fig13-newUI-2017.png)

**SQL Önerileri** altında herhangi bir öneriye tıklayabilir ve sorunu çözmek için yapılacak diğer eylemlerle ilgili daha fazla ayrıntı görebilirsiniz. Aşağıdaki örnekte, **SQL veritabanlarında Veritabanı Denetimi ve Tehdit algılama** önerisinin genişletilmiş hali gösterilmiştir.

![SQL önerisi hakkındaki ayrıntılar](./media/security-center-monitoring/security-center-monitoring-fig14-ga-new.png)

**SQL veritabanlarında Denetimi ve Tehdit algılamayı etkinleştirme** bölümünde aşağıdaki bilgiler bulunur:

* SQL veritabanlarının bir listesi
* Bulundukları sunucu
* Bu ayarın sunucudan devralınmış olduğuna veya bu veritabanında benzersiz olduğuna dair bilgiler
* Geçerli durum
* Sorunun önem derecesi

Bu öneriyle ilgilenmek için veritabanına tıkladığınızda, **Denetim ve Tehdit algılama** bölümü aşağıdaki ekran görüntüsünde gösterildiği gibi açılır.

![Denetim ve Tehdit algılama](./media/security-center-monitoring/security-center-monitoring-fig15-ga.png)

Denetimi etkinleştirmek için **Denetim** seçeneğinin altında **AÇIK**'ı seçin.

### <a name="monitor-applications"></a>Uygulamaları izleme

Azure iş yükünüzün, sunulan web bağlantı noktaları (TCP bağlantı noktaları 80 ve 443) ile [sanal makinelerde](../azure-resource-manager/resource-manager-deployment-model.md) (Azure Resource Manager ile oluşturulmuştur) bulunan uygulamaları varsa Güvenlik Merkezi, olası güvenlik sorunlarını tanımlamak ve düzeltme adımları önermek için bunları izleyebilir. **Uygulamalar** kutucuğuna tıkladığınızda, **Uygulama önerileri** bölümünde bir dizi öneriyle birlikte **Uygulamalar** bölümü açılır. Ayrıca konak ve IP/etki alanı başına uygulama dökümüyle WAF çözümünün yüklenmiş olup olmadığını gösterir:

![Uygulamaların güvenlik durumu](./media/security-center-monitoring/security-center-monitoring-fig8-sep2017.png)

Diğer önerilerde yaptığınız gibi, sorun ve sorunun nasıl düzeltileceği hakkında daha fazla ayrıntı görmek için öneriye tıklayabilirsiniz. Aşağıdaki şekilde gösterilen örnekte, güvenli olmayan bir web uygulaması olarak tanımlanan bir uygulama bulunur. Güvenli kabul edilmeyen uygulamayı seçtiğinizde, aşağıdaki seçenek kullanılabilir:

![Ayrıntılar](./media/security-center-monitoring/security-center-monitoring-fig17-ga.png)

Burada bu uygulamaya ilişkin tüm önerilerin bir listesi bulunur. **Web uygulaması güvenlik duvarı ekleyin** önerisine tıkladığınızda aşağıdaki ekran görüntüsünde gösterildiği gibi bir web uygulaması güvenlik duvarını (WAF) yüklemenize yönelik seçenekleri içeren **Web Uygulaması Güvenlik Duvarı Ekleme** bölümü açılır.

![Web Uygulaması Güvenlik Duvarı Ekleme iletişim kutusu](./media/security-center-monitoring/security-center-monitoring-fig18-ga.png)

## <a name="see-also"></a>Ayrıca bkz.
Bu makalede, Azure Güvenlik Merkezi'nde izleme işlevlerini nasıl kullanacağınız hakkında bilgi edindiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md): Azure Güvenlik Merkezi'nde güvenlik ayarlarını yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md): Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md): İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md): Hizmet kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik Blogu](http://blogs.msdn.com/b/azuresecurity/): Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.
