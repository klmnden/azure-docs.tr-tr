---
title: Olağanüstü durum kurtarma ayarlama fo çok katmanlı bir Azure Site RECOVERY'yi kullanarak IIS tabanlı web uygulamasının | Microsoft Docs
description: Azure Site RECOVERY'yi kullanarak IIS web grubu sanal makinelerini çoğaltma öğrenin.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: mayg
ms.openlocfilehash: 66b9342f1a67c4c9d35fda447a297cc64d048c1e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66480302"
---
# <a name="set-up-disaster-recovery-for-a-multi-tier-iis-based-web-application"></a>Çok katmanlı bir IIS tabanlı web uygulamasının olağanüstü durum kurtarmayı ayarlayın

Uygulama yazılımı, bir kuruluştaki iş üretkenliğini altyapısıdır. Çeşitli web uygulamaları bir kuruluştaki farklı amaçla kullanılabilir. Bordro işleme, finansal uygulamalar ve müşterilere yönelik Web siteleri, uygulamalar gibi bazı uygulamalar, bir kuruluş için önemli olabilir. Üretkenlik kaybını önlemek için bu uygulamalara sürekli olarak çalışır ve çalışıyor olması kuruluş için önemli. Daha da önemlisi, bu uygulamaları sürekli kullanılabilir olması marka veya görüntü kuruluşun zarar önlemeye yardımcı olabilir.

Kritik web uygulamaları genellikle ayarlandığı çok katmanlı uygulamalar: web, veritabanı ve uygulama farklı üzerinde katmanlarıdır. Çeşitli katmanlarda yayılmasını yanı sıra, uygulamaların da birden çok sunucu her katmanında trafiği Yük Dengelemesi için kullanabilirsiniz. Ayrıca, statik IP adresi eşlemeleri çeşitli katmanları arasında ve web sunucusundaki dayalı olabilir. Özellikle birden çok Web siteleri web sunucusunda yapılandırılmış olması halinde yük devretmede, bu eşlemelerin bazıları güncelleştirilmesi gerekir. Web uygulamalarında SSL kullanıyorsanız, sertifika bağlamaları güncelleştirmeniz gerekir.

Çeşitli yapılandırma dosyaları, kayıt defteri ayarlarını, bağlamaları, özel bileşenlerin (COM veya .NET), içerik ve sertifikaları yedekleme tabanlı olmayan bir çoğaltma üzerinde geleneksel kurtarma yöntemleri içerir. Dosyaları el ile yapılacak adımlar bir dizi aracılığıyla kurtarılabilir. Yedekleme ve dosyaları el ile kurtarma geleneksel kurtarma yöntemlerini hantal, hata yapmaya açık ve ölçeklenebilir değildir. Örneğin, kolayca sertifikalarını yedeklemek unutursanız. Yük devretmeden sonra seçeneği yoktur ancak sunucu için yeni sertifikalar satın almak için sol.

İyi bir olağanüstü durum kurtarma çözümü destekleyen modelleme kurtarma planları karmaşık uygulama mimarileri için. Ayrıca Katmanlar arasındaki uygulama eşlemeleri işlemek için kurtarma planına özelleştirilmiş adımlar eklemeniz mümkün olması gerekir. Olağanüstü bir durum varsa, uygulama eşlemeleri için daha düşük bir RTO liderlikte yardımcı olan tek tıklamayla, görüntüsü emin bir çözüm sağlar.

Bu makalede, Internet Information Services (IIS) kullanarak temel alan bir web uygulaması korunacak açıklanır [Azure Site Recovery](site-recovery-overview.md). Makale, bir üç katmanlı, IIS tabanlı web uygulaması Azure, olağanüstü durum kurtarma tatbikatı yapmak nasıl ve uygulamayı azure'a yük devretme işlemini çoğaltmak için en iyi uygulamaları kapsar.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdaki görevleri gerçekleştirmek nasıl bildiğinizden emin olun:

* [Bir sanal makineyi Azure'a çoğaltın](vmware-azure-tutorial.md)
* [Bir kurtarma ağı tasarlama](site-recovery-network-design.md)
* [Azure'a yük devretme testi yapın](site-recovery-test-failover-to-azure.md)
* [Azure'a yük devretme yapın](site-recovery-failover.md)
* [Bir etki alanı denetleyicisi çoğaltma](site-recovery-active-directory.md)
* [SQL Server çoğaltma](site-recovery-sql.md)

## <a name="deployment-patterns"></a>Dağıtım modelleri
Bir IIS tabanlı web uygulamasının genellikle aşağıdaki dağıtım desenlerden birini izler:

**Dağıtım modeli 1**

Uygulama isteği yönlendirme (ARR), bir IIS sunucusu ve SQL Server ile bir IIS tabanlı web grubu.

![Üç katmanı olan bir IIS tabanlı web grubu diyagramı](./media/site-recovery-iis/deployment-pattern1.png)

**Dağıtım modeli 2**

ARR, IIS sunucusu, bir uygulama sunucusu ve SQL Server ile bir IIS tabanlı web grubu.

![Dört katman olan bir IIS tabanlı web grubu diyagramı](./media/site-recovery-iis/deployment-pattern2.png)

## <a name="site-recovery-support"></a>Site Recovery desteği

Bu makaledeki örnekler için IIS 7.5 üzerinde Windows Server 2012 R2 Enterprise ile VMware sanal makinelerini kullanın. Site Recovery çoğaltma uygulamaya özgü olmadığından, bu makaledeki önerileri IIS farklı sürümlerini yanı sıra, aşağıdaki tabloda listelenen senaryolarda uygulama beklenir.

### <a name="source-and-target"></a>Kaynak ve hedef

Senaryo | İkincil siteye | Azure’a
--- | --- | ---
Hyper-V | Yes | Yes
VMware | Yes | Yes
Fiziksel sunucu | Hayır | Evet
Azure|NA|Evet

## <a name="replicate-virtual-machines"></a>Çoğaltma sanal makineler

Tüm IIS web grubu sanal makineleri Azure'a çoğaltmaya başlamak için sunulan yönergeleri [Azure Site recovery'de yük devretme testi](site-recovery-test-failover-to-azure.md).

Statik bir IP adresi kullanıyorsanız, yararlanmak için sanal makine istediğiniz IP adresini belirtebilirsiniz. IP adresi ayarlamak için şuraya gidin: **işlem ve ağ ayarları** > **hedef IP**.

![Hedef IP Site Recovery işlem ve ağ bölmesinde ayarlanacağı gösteren ekran görüntüsü](./media/site-recovery-active-directory/dns-target-ip.png)

## <a name="create-a-recovery-plan"></a>Kurtarma planı oluşturma
Bir kurtarma planı yük devretme sırasında çok katmanlı bir uygulama çeşitli katmanları sıralama destekler. Sıralama, özel olarak uygulama tutarlılığı sürdürmeye yardımcı olur. Çok katmanlı web uygulaması için bir kurtarma planı oluşturduğunuzda, tam adımlar açıklanan [Site Recovery kullanarak bir kurtarma planı oluşturma](site-recovery-create-recovery-plans.md).

### <a name="add-virtual-machines-to-failover-groups"></a>Sanal makine için yük devretme grupları ekleyin
Tipik bir çok katmanlı IIS web uygulamasını aşağıdaki bileşenlerden oluşur:
* SQL sanal makinelerin bulunduğu bir veritabanı katmanı.
* Bir IIS sunucusu oluşan web katmanı ve bir uygulama katmanı. 

Sanal makineleri katmanını temel alan farklı gruplara ekleyin:

1. Bir kurtarma planı oluşturun. Grup 1 altında veritabanı katmanı sanal makine ekleyin. Bu, veritabanı katmanı sanal makinelerinin Kapat son ve ilk duruma sağlar.
1. 2\. Grup altında uygulama katmanı sanal makine ekleyin. Bu, veritabanı katmanı olana sonra uygulama katmanı sanal makineler getirilir, sağlar.
1. Web katmanı sanal makinelerinin grubu 3'te ekleyin. Bu uygulama katmanı olana sonra web katmanı sanal makineler getirilir, sağlar.
1. Yük Dengeleme sanal makine grubu 4'te ekleyin. Bu, web katmanı olana sonra sanal makinelerin yük dengelemesini yük getirilir olmasını sağlar.

Daha fazla bilgi için [kurtarma planını özelleştirin](site-recovery-runbook-automation.md#customize-the-recovery-plan).


### <a name="add-a-script-to-the-recovery-plan"></a>Kurtarma planı için bir komut dosyası Ekle
IIS web grubu düzgün çalışması Azure sanal makineleri yük devretme sonrası veya test yük devretmesi sırasında bazı işlemler yapmanız gerekebilir. Bazı yük devretme sonrası işlemleri otomatik hale getirebilirsiniz. Örneğin, DNS girişini güncelleştirmek, bir site bağlaması değiştirme veya kurtarma planına betikleri ekleyerek bir bağlantı dizesini değiştirin. [VMM komut bir kurtarma planına ekleyin](site-recovery-how-to-add-vmmscript.md) bir betik kullanarak otomatik görevler ayarlama işlemi açıklanmaktadır.

#### <a name="dns-update"></a>DNS güncelleştirme
DNS, DNS dinamik güncelleştirmesi için yapılandırıldıysa, bunlar başlattığınızda sanal makinelerin DNS genellikle yeni IP adresiyle güncelleştirin. Sanal makineler yeni IP adresleri ile DNS güncelleme, eklemek için açık bir adım eklemek istiyorsanız bir [DNS IP güncelleştirmek için betik](https://aka.ms/asr-dns-update) kurtarma planı grupları üzerinde bir yük devretme sonrası eylem olarak.  

#### <a name="connection-string-in-an-applications-webconfig"></a>Bir uygulamanın web.config dosyasında bağlantı dizesi
Web sitesi iletişim kuran bir veritabanı bağlantı dizesini belirtir. Bağlantı dizesi veritabanı sanal makinenin adı taşıyan başka bir adım gerekli yük devretme sonrası vardır. Uygulama, otomatik olarak veritabanı ile iletişim kurabilir. Ayrıca, veritabanı sanal makine için IP adresi tutulursa, bağlantı dizesini güncelleştirmeniz gerekiyor olması değil. 

Bağlantı dizesi bir IP adresi kullanarak veritabanı sanal makine başvurursa, güncelleştirilmiş yük devretme sonrası olması gerekiyor. Örneğin, aşağıdaki bağlantı dizesi noktalarının IP ile veritabanına 127.0.1.2 adres:

        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
        <connectionStrings>
        <add name="ConnStringDb1" connectionString="Data Source= 127.0.1.2\SqlExpress; Initial Catalog=TestDB1;Integrated Security=False;" />
        </connectionStrings>
        </configuration>

Web katmanı bağlantı dizesinde güncelleştirmek için ekleme bir [IIS bağlantı güncelleştirme betiğini](https://gallery.technet.microsoft.com/Update-IIS-connection-2579aadc) 3. Grup kurtarma planında sonra.

#### <a name="site-bindings-for-the-application"></a>Uygulama için site bağlamaları
Her site, bağlama bilgileri içerir. Bağlama bilgileri bağlama, hangi IIS sunucu isteklerini site, bağlantı noktası numarası ve site için ana bilgisayar adları için dinleyen IP adresi türünü içerir. Yük devretme sırasında bunlarla ilişkili IP adresini bir değişiklik olursa bu bağlamaları güncelleştirmeniz gerekebilir.

> [!NOTE]
>
> Site bağlaması yaparsanız **Tümü Atanmamış**, bu bağlama yük devretme sonrası güncelleştirmeniz gerekmez. Ayrıca, bir site ile ilişkili IP adresi değiştirilen yük devretme sonrası değilse, site bağlaması güncelleştirmek gerekmez. (IP adresi elde tutulmasını birincil ve kurtarma sitelerine atanmış alt ağların ve ağ mimarisini bağlıdır. Güncelleştiren kuruluşunuz için uygun olmayabilir.)

![SSL bağlaması ayarını gösteren ekran görüntüsü](./media/site-recovery-iis/sslbinding.png)

IP adresi bir site ile ilişkili tüm site bağlamaları, yeni IP adresiyle güncelleştirin. Site bağlamaları değiştirmek için ekleme bir [IIS web katmanı güncelleştirme betiğini](https://aka.ms/asr-web-tier-update-runbook-classic) 3. Grup kurtarma planında sonra.

#### <a name="update-the-load-balancer-ip-address"></a>Yük Dengeleyici IP adresi güncelleştir
Bir ARR sanal IP adresini güncelleştirmek için makine varsa ekleyin bir [IIS ARR yük devretme betiği](https://aka.ms/asr-iis-arrtier-failover-script-classic) grubu 4 '.

#### <a name="ssl-certificate-binding-for-an-https-connection"></a>Bir HTTPS bağlantısı için SSL sertifikası bağlama
Bir Web sitesi web sunucusu ve kullanıcının tarayıcı arasında güvenli bir bağlantı sağlamaya yardımcı olur ilişkili bir SSL sertifikası olabilir. Web sitesi bir HTTPS bağlantısı varsa ve aynı zamanda bir ilişkili site olan HTTPS bağlaması IIS sunucusu IP adresi için bir SSL sertifikası bağlaması, IIS sanal makine yük devretme sonrası IP adresine sahip sertifika için yeni bir site bağlaması eklemeniz gerekir.

SSL sertifikası, bu bileşenlerin karşı verilebilir:

* Web sitesinin tam etki alanı adı.
* Sunucu adı.
* Etki alanı adı için joker karakter sertifikası.  
* Bir IP adresi. IIS sunucusu IP adresini karşı SSL sertifikası verilirse, başka bir SSL sertifikası Azure sitesindeki IIS sunucusu IP adresini karşı verilmesi gerekir. Bu sertifika için ek SSL bağlaması oluşturulması gerekir. Bu nedenle, IP adresi karşı bir SSL sertifikası kullanmamanızı öneririz. Bu seçenek, daha az yaygın olarak kullanılır ve uygun olarak yeni sertifika yetkilisi/tarayıcı Forumu değişiklikler yakında kullanımdan kaldırılacak.

#### <a name="update-the-dependency-between-the-web-tier-and-the-application-tier"></a>Web katmanı ve uygulama katmanı arasında bağımlılık güncelleştir
Sanal makinelerin IP adresine göre bir uygulamaya özgü bağımlılığı varsa, bu bağımlılık yük devretme sonrası güncelleştirmeniz gerekir.

## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

1. Azure portalında kurtarma Hizmetleri kasanızı seçin.
2. IIS web grubu için oluşturulan kurtarma planı seçin.
3. **Yük Devretme Testi**'ni seçin.
4. Test yük devretme işlemini başlatmak için kurtarma noktası ve Azure sanal ağı'nı seçin.
5. İkincil bir ortamı yukarı olduğunda doğrulamaları gerçekleştirebilirsiniz.
6. Yük devretme test ortamı temizlemek için doğrulamaları tamamlandığı zaman seçin **doğrulamaların tamamlanması**.

Daha fazla bilgi için [Azure Site recovery'de yük devretme testi](site-recovery-test-failover-to-azure.md).

## <a name="run-a-failover"></a>Yük devretme çalıştırma

1. Azure portalında kurtarma Hizmetleri kasanızı seçin.
1. IIS web grubu için oluşturulan kurtarma planı seçin.
1. **Yük devretme**'yi seçin.
1. Yük devretme işlemini başlatmak için kurtarma noktasını seçin.

Daha fazla bilgi için [Site recovery'de yük devretme](site-recovery-failover.md).

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinin [diğer uygulamaların çoğaltılması](site-recovery-workload.md) Site Recovery kullanarak.
