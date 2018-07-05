---
title: Azure Güvenlik Merkezi'nde güvenliği izleme | Microsoft Docs
description: Bu makale, Azure Güvenlik Merkezi'nde izleme özelliklerini kullanmaya başlamanıza yardımcı olur.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: mbaldwin
editor: ''
ms.assetid: 3bd5b122-1695-495f-ad9a-7c2a4cd1c808
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/26/2018
ms.author: terrylan
ms.openlocfilehash: 434b73d4625f86fab195dbda1fed9c841791f5b6
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37099468"
---
# <a name="security-health-monitoring-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik durumunu izleme
Bu makale, ilkelerle uyumluluğu izlemek için Azure Güvenlik Merkezi'ndeki izleme özelliklerini kullanmanıza yardımcı olur.

## <a name="what-is-security-health-monitoring"></a>Güvenlik durumunu izleme nedir?
Genellikle izlemeyi, izleme ve bir olayın gerçekleşmesini bekleyip duruma tepki verme olarak ele alırız. Güvenliği izleme, kuruluş standartlarıyla veya en iyi uygulamalarla uyumlu olmayan sistemleri tanımlamak için kaynaklarınızı denetleyen öngörülü bir stratejiye sahip olma anlamına gelir.

## <a name="monitoring-security-health"></a>Güvenlik durumunu izleme
Bir aboneliğin kaynakları için [güvenlik ilkelerini](security-center-policies.md) etkinleştirmenizin ardından, Güvenlik Merkezi olası güvenlik açıklarını tanımlamak amacıyla kaynaklarınızın güvenliğini analiz eder. Ağ yapılandırmanız ile ilgili bilgiler hemen kullanımınıza sunulur. Aracının yüklü olduğu VM'ler ve bilgisayarların sayısına bağlı olarak, güvenlik güncelleştirmesi durumu ve işletim sistemi yapılandırması gibi bilgisayar yapılandırma ayarları ve VM’ler hakkında bilgi toplamak bir saat veya daha fazla sürebilir. **Önleme** bölümünde kaynaklarınızın güvenlik durumunu ve sorunları görüntüleyebilirsiniz. Ayrıca, bu sorunların bir listesini **Öneriler** kutucuğunda görüntüleyebilirsiniz.

Önerileri uygulama hakkında daha fazla bilgi için [Azure Güvenlik Merkezi'nde güvenlik önerilerini uygulama](security-center-recommendations.md)'yı okuyun.

**Kaynak sistem durumu izleme** bölümünde kaynaklarınızın güvenlik durumunu izleyebilirsiniz. Aşağıdaki örnekte, her bir kaynağın kutucuğunda (İşlem ve Uygulamalar, Ağ, Veri güvenliği, Kimlik ve erişim) tanımlanan toplam sorun sayısının olduğunu görebilirsiniz.

![Kaynaklar güvenlik durumu kutucuğu](./media/security-center-monitoring/security-center-monitoring-fig1-newUI-2017.png)


### <a name="monitor-compute--apps"></a>İşlem ve uygulamaları izleme
Daha fazla bilgi için bkz. [Azure Güvenlik Merkezi'nde makinelerinizi ve uygulamalarınızı koruma](security-center-virtual-machine-recommendations.md).

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

Bu topoloji görünümünde, ilk düzeyde [sanal ağlar](../virtual-network/virtual-networks-overview.md), [sanal ağ geçitleri](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md) ve [sanal ağlar (klasik)](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) bulunur. İkinci düzeyde alt ağlar ve üçüncü düzeyde de bu alt ağlara ait sanal makineler bulunur. Aşağıdaki örnekte gösterildiği gibi sağ sütunda bu kaynaklara yönelik ağ güvenlik grubunun geçerli durumu bulunur:

![Ağ topolojisi bölümünde ağ güvenlik grubunun durumu](./media/security-center-monitoring/security-center-monitoring-fig12-ga.png)

Bu dikey pencerenin en altında, daha önce açıklanana benzer şekilde bu sanal makine için öneriler bulunur. Daha fazla bilgi edinmek ya da gerekli güvenlik denetimini veya yapılandırmasını uygulamak için bir öneriye tıklayabilirsiniz.

### <a name="monitor-data-security"></a>Veri güvenliğini izleme

**Önleme** bölümünde **Veri güvenliği**’ne tıkladığınızda, **Veri Kaynakları** bölümünü SQL ve Depolama’ya yönelik önerilerle birlikte açılır. Ayrıca, veritabanının genel sağlık durumu için [öneriler](security-center-sql-service-recommendations.md) içerir. Depolama şifrelemesi hakkında daha fazla bilgi için [Azure Güvenlik Merkezi'ndeki Azure depolama hesabı için şifrelemeyi etkinleştirme](security-center-enable-encryption-for-storage-account.md) bölümünü okuyun.

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

### <a name="monitor-identity--access"></a>Kimliği ve erişimi izleme

Daha fazla bilgi için bkz. [Azure Güvenlik Merkezi'nde kimliği ve erişimi izleme](security-center-identity-access.md).

## <a name="see-also"></a>Ayrıca bkz.
Bu makalede, Azure Güvenlik Merkezi'nde izleme işlevlerini nasıl kullanacağınız hakkında bilgi edindiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md): Azure Güvenlik Merkezi'nde güvenlik ayarlarını yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md): Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md): İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md): Hizmet kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik Blogu](http://blogs.msdn.com/b/azuresecurity/): Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.
