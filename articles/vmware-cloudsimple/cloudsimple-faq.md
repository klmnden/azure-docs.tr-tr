---
title: SSS - CloudSimple VMware çözümüyle
description: Azure VMware CloudSimple çözümüyle için sık sorulan sorular
author: sharaths-cs
ms.author: b-shsury
ms.date: 05/24/19
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: e727398e1b7bfa406166574ab40320c68dac5709
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66358536"
---
# <a name="frequently-asked-questions-about-vmware-solution-by-cloudsimple"></a>VMware çözümü CloudSimple tarafından hakkında sık sorulan sorular

Sık sorulan sorular ve yardımcı hakkındaki sorularınızın yanıtlarını CloudSimple VMware çözümüyle hizmet ve nasıl kullanılacağını anlayın.  Sorular ve cevaplar aşağıdaki kategoriler halinde düzenlenir.

* CloudSimple hizmeti
* Bağlantı
* Ağ
* Güvenlik
* İşlem
* Depolama
* VMware
* Azure tümleştirme
  
## <a name="cloudsimple-service"></a>CloudSimple hizmeti

**VMware çözümü CloudSimple tarafından nedir?**

**VMware CloudSimple çözümüyle** dönüştüren ve dakikalar içinde VMware iş yüklerini Azure üzerinde özel, adanmış bulutlara genişletir. Biz sağlama, yönetme ve şirket içi ile Azure arasında iş yükleri işlemlerini ilgileniriz. Azure'da ve tam olarak aynı şirket içi uygulamalarınızı çalıştığı için esneklik ve bulut uygulamalarınızı bütçeden karmaşıklığı olmadan hizmetlerden yararlanın. CloudSimple toplam isteğe bağlı sağlama, ödeme olarak-,-büyütme sağlayan bulut tüketimi model ve kapasite iyileştirme sahiplik maliyetinizi düşürür.  Bkz: [CloudSimple tarafından azure'da VMware çözümü nedir](cloudsimple-vmware-solutions-overview.md) özellikleri, avantajları ve senaryolar için.

**CloudSimple özel bulut nedir?**

Yüksek performanslı işlem, depolama, oluşan ve Azure konumlarında (donanım ve veri merkezi alan) Microsoft Azure altyapısında dağıtılan ortamı ağ özel, adanmış bir bulut sağlayın.  Özel bulut, yerel bir VMware platform 'bir hizmet olarak' sağlar. VMware bağlamında, her özel bulut, vCenter Server'ın tam bir örnek içerir. VCenter sunucusu bir veya daha fazla vSphere kümelerinizin karşılık gelen sanal SAN (vSAN) depolama yer alan birden çok ESXi düğüm yönetir. Birden çok özel bulut, Azure aboneliğinizdeki CloudSimple hizmet içerebilir.  Özel bulut hakkında daha fazla bilgi için bkz: [özel buluta genel bakış](cloudsimple-private-cloud.md).

**CloudSimple hizmet nerede kullanılabilir?**

CloudSimple Doğu ABD ve Batı ABD bölgelerinde kullanılabilir.

**CloudSimple için Aboneliğimi nasıl etkinleştirebilirim?**

Microsoft hesap temsilcinize başvurabilirsiniz [ azurevmwaresales@microsoft.com ](mailto:azurevmwaresales@microsoft.com) CloudSimple hizmeti aboneliğinizi etkinleştirmek için. Abonelik Kimliğinizi CloudSimple hizmetinin etkinleştirilmiş istediğiniz e-posta sağlayın.  

**CloudSimple portalına nasıl erişebilirim?**

Size, Azure portalından CloudSimple portalına erişim.  Bkz: [VMware çözümüyle erişme CloudSimple portalı Azure portalından](https://docs.azure.cloudsimple.com/access-cloudsimple-portal) makale CloudSimple portalına erişim ile ilgili ayrıntılar için. 

**Bir özel bulut için kapasiteyi nasıl artırabilirim?**

Azure portalından düğümleri satın alıp CloudSimple portalından özel Bulutunuzu genişletin.  Özel bulut, var olan bir vSphere kümesine ek düğümler eklemek veya yeni vSphere kümesi oluşturarak genişletebilirsiniz.  Bkz: [CloudSimple özel Bulutu genişletme](https://docs.azure.cloudsimple.com/expand-private-cloud) yordam için makale.

**Bakım sırasında özel Bulutum neler?**

Günlük zamanlanan bakım önce düzenli bildirimler CloudSimple sağlar.  Bakım, özel bulut kullanılabilirliğini sağlamak için kesintiye neden olmayan bir yolla yapılır.  Bakım aşağıdaki türde olabilir:

1. **CloudSimple altyapısı:**  CloudSimple altyapısı, yüksek oranda kullanılabilir olacak şekilde tasarlanmıştır.  Bakım sırasında bağlantı ve özel Bulutunuzu kullanılabilirliğini güvence altına herhangi bir zamanda yedek bileşenlerin bir güncelleştirerek.  Özel bulut vCenter'ınıza, tüm sanal makineler, kendi özel buluttan internet bağlantısı ve şirket içi veya Azure bağlantıları erişiminiz.
2. **CloudSimple portalı:** Bakım sırasında bazı özellikler CloudSimple portalında erişilebilir ya da devre dışı olmayabilir.  Bakım bildirimi portalında yapılabilir ayrıntılarını içerir.

## <a name="connectivity"></a>Bağlantı

**CloudSimple bölge ağa bağlantı seçeneklerim nelerdir?**

CloudSimple CloudSimple bölge ağınıza bağlanmak için üç farklı bağlantı seçenekleri sağlar.  Üç birlikte kullanılabilir.

1. ExpressRoute bağlantısı, şirket içi veri merkezinden CloudSimple bölge ağ - hızlı düşük gecikme süresi güvenli özel bağlantı kullanarak Global erişim CloudSimple ExpressRoute devreniz ile şirket içi ExpressRoute devreniz köprüleme. Bkz: [CloudSimple ExpressRoute aracılığıyla şirket içi bağlanma](https://docs.azure.cloudsimple.com/on-premises-connection) bağlantısı kurmak için makale.
2. Azure sanal ağınız ExpressRoute bağlantısı CloudSimple bölge ağ - hızlı düşük gecikme süresi güvenli özel bağlantı kullanarak sanal ağ geçitleri CloudSimple ExpressRoute devreniz ile Azure sanal ağınızda köprüleme.  Bkz: [CloudSimple özel bulut ortamınızı ExpressRoute kullanarak Azure sanal ağına bağlama](https://docs.azure.cloudsimple.com/azure-expressroute-connection) bağlantısı kurmak için makale.
3. CloudSimple bölge ağınıza - siteden siteye VPN bağlantısı, şirket içi veri merkezinden şirket içi VPN cihazınızdaki CloudSimple özel bulut bölgeniz için sanal özel ağ güvenliğini sağlayın.  VPN bağlantısı kurmak için bkz. [CloudSimple özel Bulut ve şirket içi ağ arasında VPN bağlantısı kurma] makalesi.

**Bir özel buluta nasıl bağlanabilirim?**

CloudSimple Portalı'nda özel bulut ayrıntılarını görüntüleyebilirsiniz. Özel bulut için karşılık gelen vCenter bağlanmak için belirlenen kullanarak siteden siteye, noktadan siteye veya ExpressRoute ağ bağlantısı olduğundan emin olun. Ardından, CloudSimple portalı ve Azure portalını başlatma *vSphere istemci başlatma* giriş sayfasında veya özel bulut ayrıntıları sayfasındaki.

**ExpressRoute bağlantı hattı avantajı nedir?**

Azure ExpressRoute bağlantı hattı, yüksek hızda çok düşük gecikme süresi güvenli bir bağlantı sağlar.  Müşteri başına bölge başına adanmış bir ExpressRoute bağlantı hattı CloudSimple sağlar.  Bu bağlantı hattını kullanarak, şirket içi ve/veya Azure aboneliğinize güvenli bağlantı kurabilirsiniz.

**Hangi CloudSimple buradan bağlanmak için ağ maliyetleri aşağıda sunulmuştur. Öğesine/öğesinden CloudSimple azure'a tüm çıkış ücretlerini? Bölgeler arasında?**

Tüm ağ çıkışı için ücret alınmaz.  Standart Azure ücretleri, sanal ağınızdan ya da şirket içi ExpressRoute bağlantı hattı herhangi bir çıkış trafiği için geçerlidir.

## <a name="networking"></a>Ağ

**Hangi ağ özellikleri my özel bulut için kullanılabilir mi?**

VLAN'ları (ve alt ağlarını) sağlama, tablolar, güvenlik duvarı ve genel IP adresleri atayabilirim ve özel bulutta çalışan bir sanal makine eşleyin.  Daha fazla bilgi için [VLAN'lar/alt ağlar genel bakış](cloudsimple-vlans-subnets.md), [güvenlik duvarı tablolar genel bakış](cloudsimple-firewall-tables.md), ve [genel IP adresi genel bakış](cloudsimple-public-ip-address.md) makaleler.

**Nasıl farklı alt ağlarda uygulamalarım için özel Bulutum ayarlayabilirim?**

CloudSimple portalınızdan özel Bulutunuzda, VLAN'ları oluşturabilirsiniz.  VLAN oluşturduktan sonra VLAN'ı kullanarak, özel bulut vCenter dağıtılmış bağlantı noktası grubu oluşturabilir ve dağıtılmış bir bağlantı noktası grubuna bağlı sanal makineleri oluşturun.  VLAN/alt ağ için güvenlik duvarı tablo etkinleştirebilir ve ağ trafiğinin güvenliğini sağlamak için güvenlik duvarı kuralları tanımlayın.

**Hangi güvenlik duvarı ayarları benim için özel bulutlarda kullanılabilir mi?**

Kuzey-Güney ve Doğu-Batı trafiği kurallar yapılandırabilirsiniz.  Kurallar, bir güvenlik duvarı tabloda tanımlanır.  Güvenlik Duvarı tablo özel bulutunuzda VLAN(s) iliştirilebilir.  Bkz: [güvenlik duvarı tablolar ve kuralları için özel Bulutları ayarlama](https://docs.azure.cloudsimple.com/firewall) makale yordamı ayarlamak için.

**Ben genel IP adresleri VM'ler için özel bulut ortamımın atayabilirim miyim?**

Kolayca CloudSimple Portalı'nda yeni bir genel IP adresi ayırın ve bunu özel bir IP adresi, sanal makine ya da gereçlerden biri ile ilişkilendirebilirsiniz.  Ayrıca, yeni güvenlik duvarı kuralları oluşturma veya trafiği belirli bağlantı noktalarını ve/veya belirli IP adreslerinin portalında kümesi izin vermek için mevcut güvenlik duvarı kuralları uygulayın. Bkz: [özel bulut ortamı için genel IP adresleri ayırmak](https://docs.azure.cloudsimple.com/public-ips) yordamı ayarlamak için.

## <a name="security"></a>Güvenlik

**CloudSimple üzerinde güvenlik seçeneklerim nelerdir?**

CloudSimple özel bulut, özel bulutta ortamınızın güvenliğini sağlamak için aşağıdaki güvenlik özellikleri sağlar:

1. **Bekleyen şifreleme veri**: Özel bulut vSAN depolama alanında bulunan bekleyen verileri şifreleyebilirsiniz. vsan'ı Azure sanal ağı veya şirket içi ortamınızda dağıtılabilir dış anahtar yönetimi sunucusu, destekler.  Bkz: [CloudSimple özel bulutunuzun vSAN şifrelemeyi yapılandırma](https://docs.azure.cloudsimple.com/vsan-encryption) daha fazla ayrıntı için.
2. **Ağ güvenliği**: Ağ trafik akışını denetleme'ndan / şirket içi Internet'ten ve güvenlik duvarı kurallarının kullanılmasını, özel bulut alt ağların içindeki özel bulut.
3. **Güvenli, özel bağlantı**: Şirket içi ağınız ile Azure aboneliğinizi arasında güvenli özel bağlantı.

## <a name="compute"></a>İşlem

**Ne tür bir ana bilgisayar kullanılabilir?**

CloudSimple iki ana türlerini sunar:

* **CS28 düğüm:** CPU:2 2,2 GHz, toplam 28 çekirdek, 48 x HT.  RAM: 256 GB.  Depolama: 1600 GB'a NVMe önbellek, 5760 GB veri (tamamen Flash). Ağ: 2x25Gbe NIC
* **CS36 düğüm:** CPU 2 x 2.3 GHz, toplam 36 çekirdek, 72 HT.  RAM: 512 GB.  Depolama: 3200 GB NVMe önbellek 11,520 GB veri (tamamen Flash).  Ağ: 2x25Gbe NIC

**Donanım hatalarının nasıl işlenir?**

Tüm CloudSimple altyapı sürekli olarak CloudSimple platform ve hizmet işlemleri ekiplerimiz tarafından izlenir.  Bir donanım hatası algılanırsa, özel bulut için yeni bir düğüm eklenir ve başarısız düğüm özel Bulutunuzdaki yüksek kullanılabilirliğini sağlama kaldırılır.

## <a name="storage"></a>Depolama

**Depolama hangi türde bir özel bulutta destekleniyor mu?**

CloudSimple sunar **tamamen flash VMware vSAN depolama** her özel bulut ile.  Her vSphere kendi vSAN veri deposu ile oluşturulur.  Bkz: [özel bulut VMware bileşenleri - vSAN depolama](https://docs.azure.cloudsimple.com/vmware-components/#vsan-storage) makale daha fazla ayrıntı için.

**Desteklenen verilerin şifrelenmesi var mı?**
Evet.  Vsan'ı üzerinde depolanan verileri şifrelemek için azure'da veya şirket içinde dağıtılabilir bir anahtar Yönetimi Sunucusu (KMS) kullanmak için özel bulutunuzda vSAN depolamayı ayarlayabilirsiniz

**Başarısız olan diskleri nasıl işlenir?**

CloudSimple izleme, özel bulutun tüm donanım bileşenleri sürekli olarak izler.  Herhangi bir disk arıza tespit veya herhangi bir disk (buluşsal yöntemler üzerinde göre) başarısız olarak tanımlandığında, yeni bir düğüm özel bulut için otomatik olarak eklenir.  Sorunlu disk düğümle özel Buluttan kaldırılır.

## <a name="vmware"></a>VMware

**Büyük ölçekli karşıya yükleme/migrate uygulamaları ve verileri şirket içinden nasıl yaparım?**

CloudSimple yerel VMware vSphere çözümü sağlar.  Toplu veri geçişi için kullanılan herhangi bir aracı CloudSimple özel bulut ile kullanılabilir.  Bazı seçenekler şunlardır:

1. VMware HCX toplu veri geçişi için.
2. Soğuk CloudSimple için şirket içi depolama vMotion'ı kullanarak verileri geçirme.

**Herhangi bir VMware araçları yükleyebilir miyim?**

CloudSimple yerel VMware vSphere çözümü sağlar.  Şirket ortamına vSphere yönetmek için kullanılan herhangi bir aracı üzerinde CloudSimple kullanılabilir.  VMware araçları yüklemek için CloudSimple Getir-kendi lisansını (KLG) modelini destekler.

**Güncelleştirmeler ve yükseltmeler nasıl yönetilir?**

CloudSimple yönetir ve özel bulut altyapısı bileşenlerinin tümünü açmayan sorunsuz bir şekilde güncelleştirir.  Tarafından CloudSimple nitelenmiş hemen sonra Vmware'de ya da altyapı satıcıları tarafından yayımlanan bir güncelleştirme veya güvenlik düzeltme eki güncelleştirmesi zamanlanacak.

Yükseltmeler veya güncelleştirmeler özel bulutta yüklü uygulamaların CloudSimple gerçekleştirmez.

## <a name="azure-integration"></a>Azure tümleştirme

**Hangi Azure hizmetleri destekleniyor mu?**

CloudSimple Azure'da aboneliğinize Azure ExpressRoute bağlantısı sağlar.  Aboneliğinizde çalıştırılan diğer hizmetler için özel bulut ağ bağlantısı olduğundan ve özel Bulutunuzu bağlanabilir.  Örnekler:

1. **Azure Active Directory** CloudSimple vCenter'ınıza bir kimlik kaynağı olarak
2. **Azure depolama** yedeklemeler, resimler ve diğer verileri, özel bulut depolama
3. **Karma uygulamalar** -genel ve özel bulutlara yayılan uygulama mimarisi oluşturabilirsiniz.  Örneğin, azure'da uygulama ve veritabanı sunucuları CloudSimple özel bulut üzerinde erişim webservers oluşturabilirsiniz.
4. **Azure İzleyici** ve **Azure Güvenlik Merkezi** -VMware üzerinde çalışan iş yüklerini bu günlüğü, performans ölçümlerini ve güvenlik yönetimi için kullanabilir.

**Azure'a nasıl my VMware kiracıları eşleme?**

CloudSimple benzersiz özel bulut, VMware Vm'lerinde Azure portalından yönetme olanağı sağlar.  Bir vCenter kaynak havuzu (istenen kaynak kısıtlamaları ile yapılandırılmış) aboneliğinizde genel yönetici tarafından eşlenebilir.  

**Hangi lisans avantajlara Azure ile sahip olurum?**

CloudSimple ile Azure hibrit kullanım Teklifi'nden yararlanın ve en fazla %90 düşürmek için diğer bulutlarda karşılaştırıldığında TCO'nuzu Microsoft Licenses yaptığınız yatırımı korur lisansları kaydedin. Ayrıca, güvenlik güncelleştirmeleri Windows Server 2008 ve Microsoft SQL Server 2008 için genişletilmiş.  Veeam, Zerto ve diğerleri gibi ortak uygulamaları için bulut maliyetlerinizi ile kendi Getir lisansları (KLG) düşük tutun.  