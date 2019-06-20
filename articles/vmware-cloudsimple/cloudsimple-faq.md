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
ms.openlocfilehash: a8cc6cf834c54ca25c12a6d66675e4290fd66136
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165808"
---
# <a name="frequently-asked-questions-about-vmware-solution-by-cloudsimple"></a>VMware çözümü CloudSimple tarafından hakkında sık sorulan sorular

Sık sorulan sorular ve yardımcı hakkındaki sorularınızın yanıtlarını Azure VMware CloudSimple çözümüyle hizmet ve nasıl kullanılacağını anlayın. Sorular ve cevaplar aşağıdaki kategoriler halinde düzenlenmiştir:

* CloudSimple hizmeti
* Bağlantı
* Ağ
* Güvenlik
* İşlem
* Depolama
* VMware
* Azure tümleştirme
 
## <a name="cloudsimple-service"></a>CloudSimple hizmeti

**Azure VMware CloudSimple çözümüyle nedir?**

Azure VMware çözümü CloudSimple tarafından dönüştürür ve dakikalar içinde VMware iş yüklerini Azure üzerinde özel, adanmış bulutlara genişletir. Çözümü sağlar, altyapıyı yönetir ve şirket içi ile Azure arasında iş yükleri düzenler. Azure'da ve tam olarak aynı şirket içi uygulamalarınızı çalıştığı için esneklik ve bulut uygulamalarınızı bütçeden karmaşıklığı olmadan hizmetlerden yararlanın. CloudSimple toplam isteğe bağlı sağlama, ödeme olarak-,-büyütme sağlayan bulut tüketimi model ve kapasite iyileştirme sahiplik maliyetinizi düşürür. Özellikler, avantajlar ve senaryolara için bkz. [CloudSimple tarafından Azure VMware çözümü nedir?](cloudsimple-vmware-solutions-overview.md).

**CloudSimple özel bulut nedir?**

Yüksek performanslı bilgi işlem, depolama ve ağ ortamı (donanım ve veri merkezi alan) Microsoft Azure altyapısında Azure konumlarında dağıtılan oluşan özel, adanmış bir bulut sağlayın. Özel bulut, yerel bir VMware platform hizmet olarak sunar. VMware bağlamında, her özel bulut, vCenter Server'ın tam bir örnek içerir. VCenter sunucusu bir veya daha fazla vSphere kümelerinizin karşılık gelen vSAN depolama yer alan birden çok ESXi düğüm yönetir. Birden çok özel bulut, Azure aboneliğinizdeki CloudSimple hizmet içerebilir. Özel bulut hakkında daha fazla bilgi için bkz: [özel buluta genel bakış](cloudsimple-private-cloud.md).

**CloudSimple hizmet nerede kullanılabilir?**

CloudSimple Doğu ABD ve Batı ABD bölgelerinde kullanılabilir.

**CloudSimple için Aboneliğimi nasıl etkinleştirebilirim?**

Microsoft hesap temsilcinize başvurun [ azurevmwaresales@microsoft.com ](mailto:azurevmwaresales@microsoft.com) CloudSimple hizmeti aboneliğinizi etkinleştirmek için. Abonelik Kimliğinizi CloudSimple hizmetinin etkinleştirilmiş istediğiniz e-posta sağlayın. 

**CloudSimple portalına nasıl erişebilirim?**

Size CloudSimple portalı Azure portalından erişim. CloudSimple portala erişim hakkında daha fazla bilgi için bkz: [VMware çözümü CloudSimple portalından Azure portalına erişin](https://docs.azure.cloudsimple.com/access-cloudsimple-portal).

**Özel bulut için kapasiteyi nasıl artırabilirim?**

Azure portalından düğümleri sağlamanıza ve özel bulutunuzun CloudSimple portalından genişletin. Özel bulut, var olan bir vSphere kümesine düğüm ekleme veya yeni bir vSphere kümesi oluşturarak genişletebilirsiniz. Yordam hakkında daha fazla bilgi için bkz. [CloudSimple özel bir buluta genişletin](https://docs.azure.cloudsimple.com/expand-private-cloud).

**My özel bulut için bakım sırasında ne olacak?**

Zamanlanan bakım önce düzenli bildirimler gün CloudSimple sağlar. Bakım özel bulutunuzun kullanılabilirliğini sağlamak için açmayan bir yolla yapılır. Bakım aşağıdaki türde olabilir:

- **CloudSimple altyapı**: CloudSimple altyapısı, yüksek oranda kullanılabilir olacak şekilde tasarlanmıştır. Bakım sırasında bağlantı ve özel bulutunuzun kullanılabilirliğini güvence altına herhangi bir zamanda yedek bileşenlerin bir güncelleştirerek. Özel bulut vCenter'ınıza, tüm sanal makineler, kendi özel buluttan internet bağlantısı ve şirket içi veya Azure bağlantıları erişebilirsiniz.
- **CloudSimple portalı**: Bakım sırasında bazı özellikler CloudSimple portalında erişilebilir olmayabilir veya devre dışı bırakılabilir. Bakım bildirimi portalında yapılabilir hakkında bilgi içerir.

## <a name="connectivity"></a>Bağlantı

**My CloudSimple bölge ağa bağlantı seçeneklerim nelerdir?**

CloudSimple CloudSimple bölge ağınıza bağlanmak için üç farklı bağlantı seçenekleri sağlar. Seçeneklerin üçünü birlikte kullanılabilir:

- CloudSimple bölge ağ için Azure ExpressRoute bağlantısı, şirket içi veri merkezinden: Global erişim kullanarak şirket içi ExpressRoute devreniz CloudSimple ExpressRoute devreniz ile arasında köprü görevi gören bir yüksek hızlı düşük gecikme süreli güvenli özel bağlantı. Bağlantıyı ayarlamak için bkz: [CloudSimple ExpressRoute kullanarak şirket içi bağlanma](https://docs.azure.cloudsimple.com/on-premises-connection).
- Azure sanal ağınız ExpressRoute bağlantısı CloudSimple bölge ağınıza: Sanal ağ geçitlerini kullanarak sanal ağınızla azure'da CloudSimple ExpressRoute bağlantı hattı arasında köprü görevi gören bir yüksek hızlı, düşük gecikme süreli güvenli özel bağlantı. Bağlantıyı ayarlamak için bkz: [CloudSimple özel bulut ortamınızı için Azure sanal ağı ExpressRoute kullanarak bağlanmak](https://docs.azure.cloudsimple.com/azure-expressroute-connection).
- CloudSimple bölge ağınızda siteden siteye VPN bağlantısı, şirket içi veri merkezinden: Güvenli bir sanal özel ağ şirket içi VPN cihazınızdaki CloudSimple özel bulut bölgeniz için. Bağlantıyı ayarlamak için bkz: [CloudSimple özel Bulut ve şirket içi ağ arasında bir VPN bağlantısı ayarlama](https://docs.azure.cloudsimple.com/set-up-vpn).

**Bir özel buluta nasıl bağlanabilirim?**

Özel bulutunuzun ayrıntılarını CloudSimple Portalı'nda görüntüleyebilirsiniz. Özel bulutunuzun karşılık gelen vCenter bağlanmak için siteden siteye, noktadan siteye ve ExpressRoute kullanarak bir ağ bağlantısı kuruldu emin olun. Ardından, Azure portalından CloudSimple portalını başlatın. Seçin **vSphere istemci başlatma** giriş sayfasında veya özel bulut Ayrıntıları sayfasında.

**ExpressRoute bağlantı hattının avantajı nedir?**

Bir Azure expressroute, yüksek hızlı, düşük gecikme süreli güvenli bir bağlantı sağlar. Müşteri başına bölge başına adanmış bir ExpressRoute bağlantı hattı CloudSimple sağlar. Bu bağlantı hattını kullanarak, şirket içi ve Azure aboneliğinizde güvenli bağlantı kurabilirsiniz.

**CloudSimple gelen ve bağlanmak için ağ maliyetlerini nelerdir? Azure'a CloudSimple gelen ve herhangi bir çıkış ücreti var mıdır? Bölgeler arasında herhangi bir çıkış ücreti vardır?**

Ağ çıkışı için ücret alınmaz. Standart Azure ücretleri, sanal ağınızdan ya da şirket içi ExpressRoute devresi herhangi bir çıkış trafiği için geçerlidir.

## <a name="networking"></a>Ağ

**Hangi ağ özellikleri my özel bulut için kullanılabilir mi?**

VLAN ve alt ağları ve güvenlik duvarı tabloları sağlayabilirsiniz. Genel IP adresleri atama ve özel bulutunuzda çalışan bir sanal makine eşleyin. Daha fazla bilgi için [VLAN ve alt ağları genel bakış](cloudsimple-vlans-subnets.md), [güvenlik duvarı tablolar genel bakış](cloudsimple-firewall-tables.md), ve [genel IP adresi genel bakış](cloudsimple-public-ip-address.md).

**Nasıl farklı alt ağlarda uygulamalarım için kendi özel bulutta ayarlayabilirim?**

CloudSimple portalınızdan özel bulutunuzda, VLAN'ları oluşturabilirsiniz. VLAN oluşturduktan sonra VLAN'ı kullanarak, özel bulut vCenter dağıtılmış bağlantı noktası grubu oluşturabilir ve dağıtılmış bir bağlantı noktası grubuna bağlı sanal makineleri oluşturun. Alt ağ ve VLAN için bir güvenlik duvarı tablo etkinleştirebilir ve ağ trafiğinin güvenliğini sağlamak için güvenlik duvarı kuralları tanımlayın.

**Hangi güvenlik duvarı ayarları benim için özel bulutlarda kullanılabilir mi?**

Kuzey-Güney ve Doğu-Batı trafiği kurallar yapılandırabilirsiniz. Kurallar, bir güvenlik duvarı tabloda tanımlanır. Güvenlik Duvarı tablo VLAN'ları özel bulutunuzda iliştirilebilir. Kurulum yordamı için bkz [güvenlik duvarı tabloları ve özel Bulutlar için kuralları ayarlama](https://docs.azure.cloudsimple.com/firewall).

**Ben genel IP adresleri VM'ler için özel bulut ortamımın atayabilirim miyim?**

Kolayca CloudSimple Portalı'nda yeni bir ortak IP adresi ayırın ve bunu özel bir IP adresi, sanal makine ya da gereçlerden biri ile ilişkilendirebilirsiniz. Ayrıca yeni bir güvenlik duvarı kuralları oluşturma veya trafiği belirli bağlantı noktaları ve belirli IP adreslerinin portalında ayarlar izin vermek için mevcut güvenlik duvarı kuralları uygulayın. Kurulum yordamı için bkz [genel IP adresleri ayırmak için bir özel bulut ortamı](https://docs.azure.cloudsimple.com/public-ips).

## <a name="security"></a>Güvenlik

**CloudSimple üzerinde güvenlik seçeneklerim nelerdir?**

CloudSimple özel bulut, özel bulut ortamınızın güvenliğini sağlamak için aşağıdaki güvenlik özellikleri sağlar:

- **Bekleyen şifreleme verileri:** VSAN depolama özel bulutunuzda bulunduğu bekleyen verileri şifreleyebilirsiniz. vsan'ı Azure sanal ağı veya şirket içi ortamınızda dağıtılabilir bir dış anahtar yönetimi sunucusu destekler. Daha fazla bilgi için [CloudSimple özel bulutunuz için vSAN şifrelemeyi yapılandırma](https://docs.azure.cloudsimple.com/vsan-encryption).
- **Ağ güvenliği:** Ağ trafik akışını denetleme ilk ve özel bulut, şirket içi, internet'ten ve güvenlik duvarı kurallarını kullanarak özel bulut alt ağların içinde.
- **Güvenli, özel bir bağlantı:** Şirket içi ağınız ile Azure aboneliğinizi arasında güvenli, özel bağlantı.

## <a name="compute"></a>İşlem

**Ne tür bir ana bilgisayar kullanılabilir?**

CloudSimple iki ana türlerini sunar:

* **CS28 düğümü**: CPU:2 2,2 GHz, toplam 28 çekirdek, 48 x HT. RAM: 256 GB. Depolama: 1600 GB'a NVMe önbellek, 5760 GB veri (tamamen Flash). Ağ: 2x25Gbe NIC
* **CS36 düğümü**: CPU 2 x 2.3 GHz, toplam 36 çekirdek, 72 HT. RAM: 512 GB. Depolama: 3200 GB NVMe önbellek 11,520 GB veri (tamamen Flash). Ağ: 2x25Gbe NIC

**Donanım hataları nasıl işlenir?**

Tüm CloudSimple altyapı sürekli olarak CloudSimple platform ve kendi hizmet operasyon ekibi tarafından izlenir. Bir donanım hatası algılanırsa, özel bulut için yeni bir düğüm eklenir. Başarısız olan düğümün özel bulutunuzda yüksek kullanılabilirliğini sağlamak için kaldırılır.

## <a name="storage"></a>Depolama

**Depolama hangi türde bir özel bulutta destekleniyor mu?**

CloudSimple sunar **tamamen flash VMware vSAN depolama** her bir özel bulutla. Her vSphere kendi vSAN veri deposu ile oluşturulur. Daha fazla bilgi için [özel bulut VMware bileşenleri - vSAN depolama](https://docs.azure.cloudsimple.com/vmware-components/#vsan-storage).

**Desteklenen verilerin şifrelenmesi var mı?**
Evet. Vsan'ı üzerinde depolanan verileri şifrelemek için azure'da veya şirket içinde dağıtılabilir bir anahtar Yönetimi Sunucusu (KMS) kullanmak için özel bulutunuzda vSAN depolamayı ayarlayabilirsiniz.

**Başarısız olan diskleri nasıl işlenir?**

CloudSimple izleme, özel bulutun tüm donanım bileşenleri sürekli olarak izler. Disk arızası algılandığında veya bir disk üzerinde buluşsal yöntemler tabanlı başarısız olarak tanımlandığında, yeni bir düğüm özel bulut için otomatik olarak eklenir. Sorunlu bir disk düğümle özel buluttan kaldırılır.

## <a name="vmware"></a>VMware

**Büyük ölçekli karşıya yükleme ve uygulama ve verileri şirket içinden nasıl yaparım?**

CloudSimple tamamen yerel bir VMware vSphere çözüm sağlar. Toplu veri geçişi için kullanılan herhangi bir aracı CloudSimple özel bulut ile kullanılabilir. Bazı seçenekler şunlardır:

- VMware HCX toplu veri geçişi için.
- Soğuk CloudSimple için şirket içi depolama vMotion'ı kullanarak veri geçirme.

**Herhangi bir VMware araçları yükleyebilir miyim?**

CloudSimple tamamen yerel bir VMware vSphere çözüm sağlar. Şirket içi CloudSimple üzerinde kullanılabilir bir vSphere ortamı yönetmek için kullanılan herhangi bir aracı. CloudSimple VMware araçları yüklemek için bir Getir-kendi lisansını (KLG) modelini destekler.

**Güncelleştirmeler ve yükseltmeler nasıl yönetilir?**

CloudSimple yönetir ve özel bulutunuzun tüm altyapı bileşenlerini açmayan sorunsuz bir şekilde güncelleştirir. Tarafından CloudSimple nitelenmiş hemen sonra Vmware'de ya da altyapı satıcıları tarafından yayımlanan bir güncelleştirme veya güvenlik düzeltme eki güncelleştirilmek üzere zamanlandı.

CloudSimple yükseltmeler veya güncelleştirmeler üzerine özel buluta yüklü uygulamaların gerçekleştirmez.

## <a name="azure-integration"></a>Azure tümleştirme

**Hangi Azure hizmetleri destekleniyor mu?**

CloudSimple Azure'da aboneliğinize Azure ExpressRoute bağlantısı sağlar. Aboneliğinizde çalışan tüm hizmetler için özel bulut ağ bağlantınız olduğunu doğrulayın ve özel bulutunuzun bağlanabilir. Örnekler:

- **Azure Active Directory**: Azure Active Directory için CloudSimple vCenter'ınıza bir kimlik kaynağı olarak kullanın.
- **Azure depolama**: Yedeklemeler, görüntüler ve özel bulutunuzun diğer verileri depolamak için depolama kullanın.
- **Karma uygulamalar**: Genel ve özel bulutlara yayılan uygulama mimarisi oluşturabilirsiniz. Örneğin, web sunucuları Azure'da, access uygulaması ve veritabanı sunucuları CloudSimple özel bir bulutta oluşturabilirsiniz.
- **Azure İzleyici** ve **Azure Güvenlik Merkezi**: VMware üzerinde çalıştırılan iş yükü, izleme ve Güvenlik Merkezi günlüğü, performans ölçümlerini ve güvenlik yönetimi için kullanabilirsiniz.

**Azure'a nasıl my VMware kiracıları eşleme?**

CloudSimple benzersiz Azure portalından özel bir buluta VMware sanal makinelerinizin yönetme olanağı sağlar. Bir vCenter kaynak havuzu, aboneliğinizde genel yönetici tarafından eşlenebilir istediğiniz kaynak kısıtlamaları ile yapılandırılmış. 

**Hangi lisans avantajlara Azure ile sahip olurum?**

CloudSimple ile Azure hibrit Avantajı'ndan yararlanın ve Microsoft lisanslarını yaptığınız yatırımı korur ve diğer bulutlara karşılaştırıldığında sahiplik toplam maliyetinizi düşürün için lisans yüzde 90'ye varan tasarruf edebilirsiniz. Ayrıca, Windows Server 2008 ve Microsoft SQL Server 2008 için güvenlik güncelleştirmeleri Genişletilmiş. Veeam, Zerto ve diğerleri gibi ortak uygulamalar için bulut maliyetlerinizi klg'li düşük tutun. 
