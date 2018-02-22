---
title: "Çok katmanlı bir Azure Site RECOVERY'yi kullanarak IIS tabanlı web uygulamasının çoğaltmak | Microsoft Docs"
description: "Azure Site Recovery kullanarak IIS web grubu sanal makineleri çoğaltmak öğrenin."
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
ms.openlocfilehash: 7ed7df2451a44075a79f514cf67efbf479a2ebb1
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="replicate-a-multi-tier-iis-based-web-application-by-using-site-recovery"></a>Site RECOVERY'yi kullanarak çok katmanlı, IIS tabanlı web uygulaması Çoğalt

Uygulama, bir kuruluştaki iş üretkenliğini altyapısı yazılımdır. Çeşitli web uygulamaları bir kuruluşta farklı amaçla kullanılabilir. Bordro işleme, finansal uygulamalar ve müşteri kullanıma yönelik Web siteleri için kullanılan uygulamalar gibi bazı uygulamalar, kuruluş için kritik olabilir. Üretkenlik kaybını önlemek için bu uygulamalar sürekli açık ve çalışıyor olmasını kuruluş için önemlidir. Daha da önemlisi, bu uygulamalara sürekli olarak kullanılabilir sahip marka veya görüntü kuruluşun zarar önlenmesine yardımcı olabilir.

Kritik web uygulamaları genellikle ayarlandığı çok katmanlı uygulamalar: web, veritabanı ve uygulama farklı katmanda bulunan verilebilir. Çeşitli katmanları arasında yayılmasını yanı sıra, uygulamaların da birden çok sunucuları her katmanında trafiği dengelemek için kullanabilir. Ayrıca, web sunucusunda ve çeşitli katmanları arasındaki eşlemeleri statik IP adreslerini dayalı olabilir. Özellikle birden çok Web sitesini web sunucusunda yapılandırılmışsa yük devretme üzerinde bu eşlemelerin bazıları güncelleştirilmesi gerekir. Web uygulamaları SSL kullanıyorsanız, sertifika bağlamaları güncelleştirmeniz gerekir.

Çeşitli yapılandırma dosyaları, kayıt defteri ayarlarını, bağlamaları, özel bileşenleri (COM veya .NET), içerik ve sertifika yedeklemeye çoğaltma esasında olmayan geleneksel kurtarma yöntemleri içerir. Dosyaları el ile yapılacak adımlar bir dizi ile kurtarılır. Yedekleme ve dosyaları el ile kurtarma geleneksel kurtarma yöntemlerinin sıkıcı, hatasız ve ölçeklenebilir olmaması. Örneğin, kolayca sertifikalarını yedeklemek unuttunuz. Yük devretme sonrasında hiç seçimi ancak sunucu için yeni sertifikalar satın almak için sol.

Modelleme iyi olağanüstü durum kurtarma çözümü destekler karmaşık uygulama mimari planları. Ayrıca uygulama katmanları arasındaki eşlemeleri işlemek için kurtarma planını özelleştirilmiş adımlarını eklemek mümkün olması gerekir. Bir olağanüstü durum ise, uygulama eşlemeleri için daha düşük RTO liderlikte yardımcı olan bir tek tıklamalı, görüntüsü emin çözümü sağlar.

Bu makale, Internet Information Services (IIS) kullanarak temel bir web uygulaması korunacak açıklamaktadır [Azure Site Recovery](site-recovery-overview.md). Makalede, bir üç katmanlı, IIS tabanlı bir web uygulamasını Azure, olağanüstü durum kurtarma ayrıntıya nasıl ve uygulamayı Azure'a yük devretme nasıl çoğaltmak için en iyi yöntemler yer almaktadır.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdaki görevlerin nasıl yapılacağını bildiğinizden emin olun:

* [Bir sanal makine azure'a](site-recovery-vmware-to-azure.md)
* [Kurtarma ağını tasarlama](site-recovery-network-design.md)
* [Azure'a yük devretme testi yapın](site-recovery-test-failover-to-azure.md)
* [Bir yük devretme gerçekleştirmek](site-recovery-failover.md)
* [Bir etki alanı denetleyicisi çoğaltma](site-recovery-active-directory.md)
* [SQL Server çoğaltma](site-recovery-sql.md)

## <a name="deployment-patterns"></a>Dağıtım modelleri
Bir IIS tabanlı web uygulama aşağıdaki dağıtım desenleri birini genellikle aşağıdaki gibidir:

**Dağıtım modeli 1**

Uygulama isteği yönlendirme (ARR), bir IIS sunucusu ve SQL Server ile bir IIS tabanlı web grubu.

![Üç katmanları olan bir IIS tabanlı web grubu diyagramı](./media/site-recovery-iis/deployment-pattern1.png)

**Dağıtım modeli 2**

ARR, bir IIS sunucusunu, bir uygulama sunucusu ve SQL Server ile bir IIS tabanlı web grubu.

![Dört katmanları olan bir IIS tabanlı web grubu diyagramı](./media/site-recovery-iis/deployment-pattern2.png)

## <a name="site-recovery-support"></a>Site kurtarma desteği

Bu makaledeki örnekler için Windows Server 2012 R2 Enterprise üzerinde IIS 7.5 ile VMware sanal makineleri kullanın. Site Recovery çoğaltma uygulamaya özgü olmadığından, bu makaledeki önerileri aşağıdaki tabloda ve IIS farklı sürümleri için listelenen senaryolarda uygulamak beklenir.

### <a name="source-and-target"></a>Kaynak ve hedef

Senaryo | İkincil bir siteye | Azure'a
--- | --- | ---
Hyper-V | Evet | Evet
VMware | Evet | Evet
Fiziksel sunucu | Hayır | Evet
Azure|NA|Evet

## <a name="replicate-virtual-machines"></a>Çoğaltma sanal makineler

Tüm IIS web grubu sanal makinelerin Azure'a çoğaltılması başlatmak için yönergeleri [Azure Site kurtarma için yük devretme testi](site-recovery-test-failover-to-azure.md).

Statik bir IP adresi kullanıyorsanız, sanal makinenin almak için istediğiniz IP adresini belirtebilirsiniz. IP adresi ayarlamak için Git **işlem ve ağ ayarlarını** > [**hedef IP**](./site-recovery-replicate-vmware-to-azure.md#view-and-manage-vm-properties).

![Hedef IP Site kurtarma işlem ve ağ bölmesinde ayarlanacağı gösteren ekran görüntüsü](./media/site-recovery-active-directory/dns-target-ip.png)

## <a name="create-a-recovery-plan"></a>Kurtarma planı oluşturma
Bir kurtarma planı yük devretme sırasında çok katmanlı bir uygulama çeşitli katmanları sıralama destekler. Sıralama uygulama tutarlılığı korumaya yardımcı olur. Çok katmanlı web uygulaması için bir kurtarma planı oluşturduğunuzda, tam adımlar açıklanan [Site Recovery kullanarak bir kurtarma planı oluşturmak](site-recovery-create-recovery-plans.md).

### <a name="add-virtual-machines-to-failover-groups"></a>Sanal makineler yük devretme gruplarına Ekle
Tipik bir çok katmanlı IIS web uygulama aşağıdaki bileşenlerden oluşur:
* SQL sanal makinelerin bulunduğu bir veritabanı katmanı.
* Bir IIS sunucusu oluşur web katmanı ve bir uygulama katmanı. 

Sanal makineler katmanını temel alan farklı gruplara ekleyin:

1. Bir kurtarma planı oluşturun. 1. Grup altında veritabanı katmanı sanal makineleri ekleyin. Bu veritabanı katman sanal makinelerde son kapatın ve ilk duruma sağlar.
1. Grup 2 altında uygulama katmanı sanal makineleri ekleyin. Bu, veritabanı katmanı duruma sonra uygulama katman sanal makinelerde getirildiği olmasını sağlar.
1. Web katmanı sanal makineleri grubu 3'te ekleyin. Bu, uygulama katmanı duruma sonra web katman sanal makinelerde getirildiği olmasını sağlar.
1. Yük Dengeleme sanal makine Grup 4'te ekleyin. Bu, web katmanı duruma sonra Yük Dengeleme sanal makineleri getirildiği olmasını sağlar.

Daha fazla bilgi için bkz: [kurtarma planını özelleştirin](site-recovery-runbook-automation.md#customize-the-recovery-plan).


### <a name="add-a-script-to-the-recovery-plan"></a>Kurtarma planı için bir komut dosyası ekleme
IIS web grubu düzgün çalışması Azure sanal makineleri yük devretme sonrasında veya yük devretme testi sırasında bazı işlemler yapmanız gerekebilir. Bazı yük devretme sonrası işlemleri otomatik hale getirebilirsiniz. Örneğin, DNS girişini güncelleştirmek, bir site bağlaması değiştirebilir veya kurtarma planına betikleri ekleyerek bir bağlantı dizesini değiştirin. [Bir VMM komut dosyası için bir kurtarma planı eklemek](site-recovery-how-to-add-vmmscript.md) bir komut dosyası kullanarak otomatikleştirilmiş görevler ayarlamayı açıklar.

#### <a name="dns-update"></a>DNS güncelleştirme
DNS dinamik DNS güncelleştirme için yapılandırılmışsa, bunlar başlattığınızda sanal makinelerin DNS genellikle yeni IP adresiyle güncelleştirin. Sanal makineleri yeni IP adresleri ile DNS güncelleştirme, ekleme için açık bir adım eklemek istiyorsanız bir [IP DNS'de güncelleştirmek için komut dosyası](https://aka.ms/asr-dns-update) kurtarma planı grupları bir yük devretme sonrasında eylem olarak.  

#### <a name="connection-string-in-an-applications-webconfig"></a>Bir uygulamanın web.config dosyasında bağlantı dizesi
Web sitesinin iletişim kurduğu veritabanı bağlantı dizesini belirtir. Bağlantı dizesi veritabanı sanal makinenin adı taşıyan başka bir adım gerekli yük devretme sonrasında demektir. Uygulama otomatik olarak veritabanı ile iletişim kurabilir. Veritabanı sanal makine için IP adresi korunur Ayrıca, bağlantı dizesini güncellemeniz olması değil. 

Bağlantı dizesi, bir IP adresi kullanarak veritabanı sanal makineye başvuruyorsa, güncelleştirilmiş yük devretme sonrasında olması gerekiyor. Örneğin, aşağıdaki bağlantı dizesi noktaları IP ile veritabanında 127.0.1.2 adresi:

        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
        <connectionStrings>
        <add name="ConnStringDb1" connectionString="Data Source= 127.0.1.2\SqlExpress; Initial Catalog=TestDB1;Integrated Security=False;" />
        </connectionStrings>
        </configuration>

Web katmanı bağlantı dizesinde güncelleştirmek için ekleyin bir [IIS bağlantı güncelleştirme betiğini](https://aka.ms/asr-update-webtier-script-classic) sonra kurtarma planı Grup 3.

#### <a name="site-bindings-for-the-application"></a>Uygulama için site bağlamaları
Her site, bağlama bilgileri oluşur. Bağlama bilgileri tür bağlama, hangi IIS sunucu isteklerini site, bağlantı noktası numarasını ve site için ana bilgisayar adları için dinleyen IP adresini içerir. Yük devretme sırasında bunlarla ilişkili IP adresi bir değişiklik olursa bu bağlamaların güncelleştirmeniz gerekebilir.

> [!NOTE]
>
> Site bağlaması ayarlarsanız **Tümü Atanmamış**, bu bağlama yük devretme sonrasında güncelleştirmeniz gerekmez. Ayrıca, bir site ile ilişkili IP adresi değiştirilen yük devretme sonrasında değilse, site bağlaması güncelleştirme gerekmez. (IP adresi bekletme birincil ve kurtarma sitelere atanan alt ağları ve ağ mimarisini bağlıdır. Güncelleştiren kuruluşunuz için uygun olmayabilir.)

![SSL bağlaması ayarı gösteren ekran görüntüsü](./media/site-recovery-iis/sslbinding.png)

IP adresi bir site ile ilişkili ise, tüm site bağlamaları yeni IP adresiyle güncelleştirin. Site bağlamaları değiştirmek için ekleyin bir [IIS web katmanı güncelleştirme betiğini](https://aka.ms/asr-web-tier-update-runbook-classic) sonra kurtarma planı Grup 3.

#### <a name="update-the-load-balancer-ip-address"></a>Yük Dengeleyici IP adresi güncelleştir
Bir ARR sanal IP adresini güncelleştirmek için makine varsa eklemeniz bir [IIS ARR yük devretme betiği](https://aka.ms/asr-iis-arrtier-failover-script-classic) Grup 4 sonra.

#### <a name="ssl-certificate-binding-for-an-https-connection"></a>Bir HTTPS bağlantısı için SSL sertifikası bağlama
Bir Web sitesi web sunucusu ve kullanıcının tarayıcısı arasında güvenli bir bağlantı sağlamaya yardımcı olur ilişkili bir SSL sertifikası olabilir. Web sitesi bir HTTPS bağlantısı olan ve ayrıca bir ilişkili site olan HTTPS bağlaması IIS sunucusunun IP adresi için bir SSL sertifikası bağlaması varsa, IIS sanal makine yük devretme sonrasında IP adresine sahip sertifika için yeni bir site bağlaması eklemeniz gerekir.

SSL sertifikası bu bileşenlerin karşı verilebilir:

* Web sitesinin tam etki alanı adı.
* Sunucu adı.
* Etki alanı adı için bir joker karakter sertifikası.  
* Bir IP adresi. IIS sunucusunun IP adresini karşı SSL sertifika, başka bir SSL sertifikası Azure sitesinde IIS sunucusunun IP adresini karşı verilmesi gerekir. Bu sertifika için ek SSL bağlaması oluşturulması gerekir. Bu nedenle, IP adresi karşı bir SSL sertifikası kullanmayan öneririz. Bu seçenek daha az yaygın olarak kullanılan ve yeni sertifika yetkilisi/tarayıcı Forumu değişiklikleri uygun olarak hemen kullanım dışı kalacaktır.

#### <a name="update-the-dependency-between-the-web-tier-and-the-application-tier"></a>Web katmanı ve uygulama katmanı arasındaki bağımlılığı güncelleştir
Sanal makinelerin IP adresini temel alan bir uygulamaya özgü bağımlılık varsa, bu bağımlılık yük devretme sonrasında güncelleştirmeniz gerekir.

## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

1. Azure portalında, Kurtarma Hizmetleri kasası seçin.
2. IIS web grubu için oluşturulan kurtarma planı seçin.
3. Seçin **yük devretme testi**.
4. Test yük devretme işlemini başlatmak için kurtarma noktası ve Azure sanal ağı seçin.
5. İkincil ortam yukarı olduğunda doğrulama gerçekleştirebilirsiniz.
6. Doğrulama tamamlandığında, yük devretme sınama ortamı temizlenecekse seçin **doğrulamaları tamamlamak**.

Daha fazla bilgi için bkz: [Azure Site kurtarma için yük devretme testi](site-recovery-test-failover-to-azure.md).

## <a name="run-a-failover"></a>Yük devretme çalıştırma

1. Azure portalında, Kurtarma Hizmetleri kasası seçin.
1. IIS web grubu için oluşturulan kurtarma planı seçin.
1. Seçin **yük devretme**.
1. Yük devretme işlemini başlatmak için kurtarma noktası seçin.

Daha fazla bilgi için bkz: [Site Recovery yük](site-recovery-failover.md).

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [diğer uygulamaları çoğaltma](site-recovery-workload.md) Site Recovery kullanarak.
