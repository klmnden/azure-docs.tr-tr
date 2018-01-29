---
title: "Azure Site Recovery kullanan bir web uygulamasına alınarak çoğaltılır çok katmanlı IIS | Microsoft Docs"
description: "Bu makalede, Azure Site Recovery kullanarak IIS web grubu sanal makineleri çoğaltmak açıklar."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: nisoneji
ms.openlocfilehash: 00d5c1fa8c0c16daef5d928147e169553672e1f6
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="replicate-a-multi-tier-iis-based-web-application-using-azure-site-recovery"></a>Azure Site Recovery kullanan çok katmanlı tabanlı IIS web uygulamasına Çoğalt

## <a name="overview"></a>Genel Bakış


Uygulama, bir kuruluştaki iş üretkenliğini altyapısı yazılımdır. Çeşitli web uygulamaları bir kuruluşta farklı amaçla kullanılabilir. Bordro işleme, finansal uygulamalar ve müşteri kullanıma yönelik Web siteleri gibi bazılarını utmost olabilir bir kuruluş için kritik. Bunları sahip olmak kuruluş için önemlidir ve hiç çalıştıran ve daha da önemlisi üretkenlik kaybını önlemek için kez herhangi bir zarar kuruluş marka görüntüsü engelleyebilirsiniz.

Kritik web uygulamalarını web, veritabanı ve uygulama farklı katmanlardaki ile çok katmanlı uygulamaları olarak tipik olarak ayarlanır. Çeşitli katmanları arasında yayılmasını dışında uygulamaların da birden çok sunucu her katmanında trafiği dengelemek için kullanıyor olabilir. Ayrıca, web sunucusunda ve çeşitli katmanları arasındaki eşlemeleri statik IP adreslerini temel olabilir. Web sunucusunda yapılandırılmış birden çok Web sitesi varsa, yük devretme üzerinde bu eşlemelerin bazıları, özellikle, güncelleştirilmesi gerekir. Web uygulamaları SSL kullanıyorsanız, sertifika bağlamaları güncelleştirilmesi gerekir.

Çeşitli yapılandırma dosyaları, kayıt defteri ayarlarını, bağlamaları, özel bileşenleri (COM veya .NET), içerik ve ayrıca sertifikaları yedekleme ve kurtarma adımları el ile bir dizi dosyalarıyla geleneksel çoğaltma olmayan tabanlı kurtarma yöntemleri içerir. Bu teknikler açıkça sıkıcı hata eğilimindedir ve ölçeklenebilir olmaması. Örneğin, sertifikalarını yedekleme unutursanız ve hiçbir seçeneği ile bırakılması sağlar ancak yük devretme sonrasında sunucu için yeni sertifikalar satın almanız için kolayca mümkün olduğu.

İyi olağanüstü durum kurtarma çözümü izin vermelidir kurtarma planları modelleme karmaşık uygulama mimarilerindeki. Ayrıca çeşitli katmanları arasındaki uygulama eşlemeleri işlemek için özelleştirilmiş adımları ekleme yeteneği sahip olmalıdır. Bir olağanüstü durum varsa, bu düşük RTO için önde gelen bir tek tıklamalı emin görüntüsü çözümü sağlar.


Bu makalede bir temel IIS web uygulamasını kullanarak koruma konusunda [Azure Site Recovery](site-recovery-overview.md). Bu makalede, uygulamayı Azure'a üç katmanı tabanlı IIS web uygulamasını Azure, nasıl bir olağanüstü durum kurtarma detaylandırma yapabilirsiniz ve yük devretme işlemine çoğaltmak için en iyi yöntemler yer almaktadır.


## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdaki gereksinimleri anladığınızdan emin olun:

1. [Bir sanal makine Azure'a çoğaltma](site-recovery-vmware-to-azure.md)
1. Nasıl yapılır [kurtarma ağını tasarlama](site-recovery-network-design.md)
1. [Azure'a yük devretme sınamasını yapmak](./site-recovery-test-failover-to-azure.md)
1. [Azure için bir yük devretme işleminden](site-recovery-failover.md)
1. Nasıl yapılır [bir etki alanı denetleyicisi çoğaltma](site-recovery-active-directory.md)
1. Nasıl yapılır [SQL Server çoğaltma](site-recovery-sql.md)

## <a name="deployment-patterns"></a>Dağıtım modelleri
Bir temel IIS web uygulamasını genellikle aşağıdaki dağıtım desenleri birini aşağıdaki gibidir:

**Dağıtım modeli 1** bir IIS web grubu uygulama isteği Routing(ARR), IIS sunucusu ve Microsoft SQL Server ile bağlı.

![Dağıtım modeli](./media/site-recovery-iis/deployment-pattern1.png)

**Dağıtım modeli 2** bir IIS web grubu uygulama isteği Routing(ARR), IIS sunucusunu, uygulama sunucusu ve Microsoft SQL Server ile bağlı.


![Dağıtım modeli](./media/site-recovery-iis/deployment-pattern2.png)

## <a name="site-recovery-support"></a>Site kurtarma desteği

Bu makale oluşturmak amacıyla VMware sanal makinelerinin IIS sunucusunun Windows Server 2012 R2 Enterprise 7.5 sürümünde kullanılır. Site recovery çoğaltma uygulama belirsiz olduğundan, burada sağlanan öneriler de ve farklı IIS sürümü için aşağıdaki senaryoları tutun beklenir.

### <a name="source-and-target"></a>Kaynak ve hedef

**Senaryo** | **İkincil bir siteye** | **To Azure**
--- | --- | ---
**Hyper-V** | Evet | Evet
**VMware** | Evet | Evet
**Fiziksel sunucu** | Hayır | Evet
**Azure**|NA|Evet

## <a name="replicate-virtual-machines"></a>Çoğaltma sanal makineler

İzleyin [bu kılavuz](site-recovery-vmware-to-azure.md) tüm IIS web grubu sanal makinelerin Azure'a çoğaltılması başlatmak için.

Bir statik IP kullanıyorsanız, sanal makine yapılacağını istediğiniz IP belirtin [ **hedef IP** ](./site-recovery-replicate-vmware-to-azure.md#view-and-manage-vm-properties) işlem ve ağ ayarlarını.

![Hedef IP](./media/site-recovery-active-directory/dns-target-ip.png)


## <a name="creating-a-recovery-plan"></a>Bir kurtarma planı oluşturma

Bir kurtarma planı çeşitli katmanlarda çok katmanlı bir uygulama, bu nedenle, uygulama tutarlılığını sağlamak için yük devretme sıralama sağlar. Çok katmanlı web uygulaması için bir kurtarma planı oluşturmak için aşağıdaki adımları verilmiştir.  [Bir kurtarma planı oluşturma hakkında daha fazla bilgi](./site-recovery-create-recovery-plans.md).

### <a name="adding-virtual-machines-to-failover-groups"></a>Sanal makineler yük devretme gruplara ekleme
Tipik bir çok katmanlı IIS web uygulamasını SQL sanal makineler, bir IIS sunucusu tarafından constituted web katmanı ve bir uygulama katmanı ile veritabanı katmanından oluşur. Bu sanal makineleri ayarlayamadı adımlarda haliyle katmanı göre farklı grubuna ekleyin. [Kurtarma planı customising hakkında daha fazla bilgi](site-recovery-runbook-automation.md#customize-the-recovery-plan).

1. Bir kurtarma planı oluşturun. Grup kapandığından son ve ilk duruma emin olmak için 1 altında veritabanı katmanı sanal makineleri ekleyin.

1. Veritabanı katmanı duruma sonra bunlar getirildiği, Grup 2 altında uygulama katman sanal makinelerde ekleyin.

1. Uygulama katmanı duruma sonra bunlar getirildiği gibi web katman sanal makinelerde grubu 3'te ekleyin.

1. Yük Dengeleme sanal makineleri Grup 4'te web katmanı duruma sonra bunlar getirildiği gibi ekleyin.


### <a name="adding-scripts-to-the-recovery-plan"></a>Betikleri kurtarma planına ekleniyor
IIS web grubu işlevi doğru yapmak için Azure sanal makineleri post yük devretme ve Test yük devretme üzerindeki bazı işlemler yapmanız gerekebilir. Site bağlaması değiştirme DNS girişi, güncelleştirme gibi post yük devretme işlemini otomatikleştirmek, aşağıdaki şekilde kurtarma planında betikleri ekleyerek bağlantı dizesini değiştirin. [Komut dosyası kurtarma planı ekleme hakkında daha fazla bilgi edinin](./site-recovery-how-to-add-vmmscript.md).

#### <a name="dns-update"></a>DNS güncelleştirme
DNS genellikle dinamik DNS güncelleştirmeleri ve ardından sanal makineleri için yapılandırılmışsa, DNS başladıktan sonra yeni IP ile güncelleştirin. Sanal makineleri yeni IP ile DNS güncelleştirme için açık bir adım eklemek istiyorsanız, bunu eklemek [IP DNS'de güncelleştirmek için komut dosyası](https://aka.ms/asr-dns-update) kurtarma planı grupları sonrası eylemi olarak.  

#### <a name="connection-string-in-an-applications-webconfig"></a>Bir uygulamanın web.config dosyasında bağlantı dizesi
Web sitesi iletişim kuran veritabanı bağlantı dizesi belirtir.

Bağlantı dizesi veritabanı sanal makinenin adı taşıyan başka bir adım gerekli post yük devretme demektir. Uygulama DB otomatik olarak iletişim kurabilir. Veritabanı sanal makine için IP adresi korunup korunmadığını Ayrıca, bu bağlantı dizesini güncelleştirmek için gerekli. Bağlantı dizesi veritabanına bir IP adresi kullanarak sanal makine için başvuruyorsa, güncelleştirilmiş post yük devretme olması gerekiyor. Örneğin, IP 127.0.1.2 DB bağlantı dizesi olarak şunu gösteriyor

        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
        <connectionStrings>
        <add name="ConnStringDb1" connectionString="Data Source= 127.0.1.2\SqlExpress; Initial Catalog=TestDB1;Integrated Security=False;" />
        </connectionStrings>
        </configuration>

Ekleyerek web katmanı bağlantı dizesinde güncelleştirebilirsiniz [IIS bağlantı güncelleştirme betiğini](https://aka.ms/asr-update-webtier-script-classic) sonra kurtarma planı Grup 3.

#### <a name="site-bindings-for-the-application"></a>Uygulama için site bağlamaları
Her site, bağlama, hangi IIS sunucu isteklerini site, bağlantı noktası numarasını ve site için ana bilgisayar adları için dinleyen IP adresi türünü içeren bilgiler bağlama oluşur. Yük devretme sırasında bu bağlamaların kendileriyle ilişkilendirilmiş IP adresini bir değişiklik olursa güncelleştirilmesi gerekebilir.

> [!NOTE]
>
> 'Tüm atanmamış' aşağıdaki örnekte olduğu gibi site bağlaması için işaretlenmiş durumunda bu bağlama post yük devretme güncelleştirme gerekmez. Ayrıca, bir site ile ilişkili IP adresi yük devretme sonrası değiştirilmezse site bağlaması güncelleştirilmesi (bekletme IP adresinin ağ mimarisine bağlıdır ve alt birincil ve kurtarma sitelere atanmış ve bu nedenle olabilir veya kuruluşunuz için uygun olmayabilir.)

![SSL Bağlaması](./media/site-recovery-iis/sslbinding.png)

Bir site ile IP adresi ilişkilendirdiyseniz, tüm site bağlamaları yeni IP adresiyle güncelleştirmeniz gerekir. Ekleyebileceğiniz [IIS Web Katmanı güncelleştirme betiğini](https://aka.ms/asr-web-tier-update-runbook-classic) Grup 3 site bağlamaları değiştirmek için kurtarma planında sonra.


#### <a name="update-load-balancer-ip-address"></a>Yük Dengeleyici IP adresi güncelleştir
Uygulama isteği yönlendirme sanal makine varsa eklemeniz [IIS ARR yük devretme betiği](https://aka.ms/asr-iis-arrtier-failover-script-classic) IP adresini güncelleştirmek için Grup 4 sonra.

#### <a name="the-ssl-cert-binding-for-an-https-connection"></a>Bir https bağlantısı için SSL sertifikası bağlama
Web siteleri, Web sunucusu ve kullanıcının tarayıcısı arasında güvenli iletişim sağlamak için yardımcı olan ilişkili bir SSL sertifikası olabilir. Web sitesi https bağlantısı ve bir ilişkili site olan https bağlaması IIS sunucusunun IP adresi için bir SSL sertifikası bağlaması varsa, yeni bir site bağlaması için IIS sanal makine post yük devretme IP sertifikayla eklenmesi gerekir.

SSL sertifikası karşı verilebilir-

a) Web sitesinin tam etki alanı adı<br>
b) sunucu adı<br>
c) etki alanı adı için bir joker karakter sertifikası<br>
d) SSL sertifikası IIS sunucusunun IP karşı verilirse, bir IP adresi – başka bir SSL sertifikası Azure sitesinde IIS sunucusunun IP adresini karşı verilmesi gerekiyor ve bu sertifika için ek SSL bağlaması oluşturulması gerekir. Bu nedenle, IP karşı verilen bir SSL sertifikası kullanmamanız önerilir. Bu, daha az yaygın olarak kullanılan ve yeni CA/tarayıcı Forumu değişiklikleri göredir yakında kullanım dışı kalacaktır seçenektir.

#### <a name="update-the-dependency-between-the-web-and-the-application-tier"></a>Web ve uygulama katmanı arasındaki bağımlılığı güncelleştir
Sanal makinelerin IP adresine göre bir uygulamaya özgü bağımlılık varsa, bu bağımlılık post yük devretme güncelleştirmeniz gerekir.

## <a name="doing-a-test-failover"></a>Yük devretme sınamasını yapmak
İzleyin [bu kılavuz](site-recovery-test-failover-to-azure.md) yük devretme sınamasını yapmak için.

1.  Azure Portalı'na gidin ve kurtarma hizmeti kasanızı seçin.
1.  IIS web grubu için oluşturulan kurtarma planı tıklayın.
1.  'Test yük devretme üzerinde' tıklayın.
1.  Kurtarma noktası ve test yük devretme işlemini başlatmak için Azure sanal ağı seçin.
1.  İkincil ortamı kurma olduğunda, doğrulama gerçekleştirebilirsiniz.
1.  Doğrulama tamamlandıktan sonra 'Doğrulamaları tamamlamak' seçin ve yük devretme sınama ortamı temizlendi.

## <a name="doing-a-failover"></a>Bir yük devretme işleminden
İzleyin [bu kılavuz](site-recovery-failover.md) , yaparken bir yük devretme.

1.  Azure Portalı'na gidin ve kurtarma hizmeti kasanızı seçin.
1.  IIS web grubu için oluşturulan kurtarma planı tıklayın.
1.  'Failover' üzerinde'ı tıklatın.
1.  Yük devretme işlemini başlatmak için kurtarma noktası seçin.

## <a name="next-steps"></a>Sonraki adımlar
Hakkında daha fazla bilgiyi [diğer uygulamaları çoğaltmak](site-recovery-workload.md) Site RECOVERY'yi kullanarak.
