---
title: Azure sanal makinelerinde ana bilgisayar yeniden barındırma
description: Ana bilgisayar iş yüklerinizi Microsoft Azure üzerinde sanal makineleri (VM'ler) kullanarak IBM Z-tabanlı sistemler gibi yeniden barındırın.
services: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
editor: edprice
ms.author: larryme
ms.date: 04/02/2019
ms.topic: article
ms.service: multiple
ms.openlocfilehash: 8b7c2a088dc917c319acf6cad251ad53367a14b6
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58895046"
---
# <a name="mainframe-rehosting-on-azure-virtual-machines"></a>Azure sanal makinelerinde ana bilgisayar yeniden barındırma

Geçirme iş yükleri ana bilgisayardan bulut ortamları, altyapınızı modernleştirin ve genellikle maliyetlerinden tasarruf sağlar. Çok sayıda iş yükü, veritabanlarının adlarını güncelleştirme gibi yalnızca küçük bir kod değişikliği yapmadan, Azure'a aktarılabilir.

Genel anlamda, terim *anabilgisayar* büyük bir bilgisayar anlamına gelir. Özellikle büyük bir çoğunluğu şu anda kullanımda IBM sistem Z sunucuları veya MVS, DOS, VSE, işletim sistemi/390 veya z/OS çalıştıran IBM plug-compatible sistemleri olan.

Azure sanal makine'de (VM) yalıtmak ve tek bir örneği üzerinde belirli bir uygulama için kaynakları yönetmek için kullanılır. IBM z/OS gibi ana bilgisayarlar, bu amaç için mantıksal bölümleri (LPARS) kullanın. Bir ana bilgisayar için IBM Db2 veritabanı bir CICS bölgeyle ilişkili COBOL programları için bir LPAR ve ayrı LPAR kullanabilir. Tipik bir [azure'da n katmanlı uygulama](/azure/architecture/reference-architectures/n-tier/n-tier-sql-server) her katman için alt ağlar ile bölümlenmiş bir sanal ağa Azure Vm'leri dağıtır.

Azure sanal makineleri ana bilgisayar öykünmesi ortamları ve lift-and-shift ile taşıma senaryoları destekleyen derleyiciler çalıştırabilirsiniz. Geliştirme ve test, bir Azure geliştirme ve test ortamı için bir ana bilgisayardan geçirmek için ilk iş yükleri arasında çoğunlukla olur. Taklit edebilir ortak sunucusu bileşenleri işlem çevrimiçi işlem (gerçekleştirme OLTP), toplu işlem ve veri alımı sistemleri aşağıdaki şekilde gösterildiği gibi ekleyin.

![Azure'da öykünmesi ortamlarını z/OS tabanlı sistemleri çalıştırmanıza olanak sağlar.](media/01-overview.png)

Diğer bir iş ortağı çözümü kullanarak Azure'da rehosted ancak bazı ana bilgisayar iş yüklerini Azure'a göreli kolaylıkla geçirilebilir. Bir iş ortağı çözümü seçme hakkında ayrıntılı yönergeler için [Azure ana bilgisayar geçiş Merkezi](https://azure.microsoft.com/migration/mainframe/) yardımcı olabilir.

## <a name="mainframe-migration"></a>Ana bilgisayar geçişi

Yeniden barındırma, yeniden oluşturmak, değiştirmek veya devre dışı bırakma? Iaas veya PaaS? Ana bilgisayar uygulamanız için doğru geçiş stratejisini belirlemek için bkz: [ana bilgisayar geçişi](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/overview) Kılavuzu'nda Azure Mimari Merkezi.

## <a name="micro-focus-rehosting-platform"></a>Micro odak yeniden barındırma platformu

Micro odak kuruluş sunucusu kullanılabilir platformları yeniden barındırma büyük anabilgisayar biridir. Bunu bir daha ucuz x86 üzerinde z/OS iş yüklerinizi çalıştırmak Azure platformunda.

Kullanmaya başlamak için:

- [Kuruluş Sunucusu ve kurumsal Geliştirici Azure'a yükleme](./microfocus/set-up-micro-focus-azure.md)
- [Azure üzerinde Enterprise Developer CICS BankDemo ' ayarlayın](./microfocus/demo.md)
- [Kuruluş sunucusu, Azure'da bir Docker kapsayıcısında çalıştırma](./microfocus/run-enterprise-server-container.md)


## <a name="tmaxsoft-openframe-on-azure"></a>TmaxSoft OpenFrame on Azure

TmaxSoft OpenFrame lift-and-shift ile taşıma senaryoları kullanılan bir popüler anabilgisayar rehosting çözümüdür. Azure'da OpenFrame ortam, geliştirme, tanıtımlar, test veya üretim iş yükleri için uygundur.

Kullanmaya başlamak için:

- [TmaxSoft OpenFrame ile çalışmaya başlama](./tmaxsoft/get-started.md)
- [E-kitabı indir](https://azure.microsoft.com/resources/install-tmaxsoft-openframe-azure/)

## <a name="ibm-zdt-120"></a>IBM zD & T 12.0

IBM Z geliştirme ve Test Ortamı (IBM zD & T) ayarlayan bir üretim dışı ortamda azure'da geliştirme, test ve Tanıtımlar z/OS tabanlı uygulamalar için kullanabilirsiniz.

Öykünme ortamı azure'da uygulama geliştiricilerin denetlenen dağıtımları (ADCDs) aracılığıyla Z örnekleri farklı türde barındırabilirsiniz. Azure ve Azure Stack üzerinde zD & T kişisel Edition, zD & T paralel Sysplex ve zD & T Enterprise Edition çalıştırabilirsiniz.

Kullanmaya başlamak için:

- [IBM zD & T 12.0 azure'da ayarlama](./ibm/install-ibm-z-environment.md)
- [ZD & T ADCD ayarlayın](./ibm/demo.md)

## <a name="ibm-db2-purescale-on-azure"></a>Azure üzerinde IBM DB2 pureScale

IBM DB2 pureScale ortamı için Azure veritabanı kümesi sağlar. Orijinal ortama özdeş değil, ancak bunu benzer kullanılabilirlik ve ölçek IBM DB2 paralel Sysplex kurulumunda çalışan z/OS için teslim eder.

Başlamak için bkz: [IBM DB2 pureScale azure'da](/azure/virtual-machines/linux/ibm-db2-purescale-azure).

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Anabilgisayar iş yükleri için Azure altyapı (Iaas) hizmet olarak geçirdiğinizde, çeşitli Azure Vm'leri de dahil olmak üzere isteğe bağlı ve ölçeklenebilir işlem kaynağı türlerinden de seçebilirsiniz. Azure'un sunduğu çeşitli [Linux](/azure/virtual-machines/linux/overview) ve [Windows](/azure/virtual-machines/windows/overview) VM'ler.

### <a name="compute"></a>İşlem

Azure bilgi işlem gücü perakendecilerden anabilgisayar'ın kapasiteye karşılaştırır. Bir ana bilgisayar iş yükü Azure'a geçmeyi düşünüyorsanız, sanal CPU'lara (MIPS) saniyede bir milyon yönergeleri anabilgisayar ölçüsü karşılaştırın. 

Bilgi edinmek için nasıl [anabilgisayar işlem Azure'a taşıyın](./concepts/mainframe-compute-azure.md).

### <a name="high-availability-and-failover"></a>Yüksek kullanılabilirlik ve yük devretme

Azure, taahhüt tabanlı hizmet düzeyi sözleşmelerine (SLA) sunar. Birden çok dokuzlu kullanılabilirlik varsayılandır ve SLA'lar Hizmetleri yerel ya da coğrafi tabanlı çoğaltma ile iyileştirilebilir. [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) şartları, Azure’un tamamının kullanılabilirlik garantisini açıklamaktadır.

İle Azure Iaas VM gibi belirli sistem işlevlerini yük devretme desteği — Örneğin, yük devretme kümesi örnekleri ve [kullanılabilirlik kümeleri](/azure/virtual-machines/windows/regions-and-availability#availability-sets). Platform, Azure platformu hizmet (PaaS) kaynak olarak kullandığınızda, yük devretme otomatik olarak işler. Örnekler [Azure SQL veritabanı](/azure/sql-database/sql-database-technical-overview) ve [Azure Cosmos DB](/azure/cosmos-db/introduction).

### <a name="scalability"></a>Ölçeklenebilirlik

Ana bilgisayarlar genellikle, bulut ortamları ölçek genişletme sırasında ölçeklendirin. Azure'un sunduğu çeşitli [Linux](/azure/virtual-machines/linux/sizes) ve [Windows](/azure/virtual-machines/windows/sizes) gereksinimlerinizi karşılayacak şekilde boyutları. Bulut ayrıca yukarı veya aşağı eşleştirme tam kullanıcı belirtimleri ölçeklendirir. İşlem gücü, depolama ve Hizmetleri [ölçek](/azure/architecture/best-practices/auto-scaling) kullanım tabanlı faturalandırma modeli altında isteğe bağlı.

### <a name="storage"></a>Depolama

Bulutta, esnek, ölçeklenebilir depolama seçenekleriyle sahiptir ve yalnızca ihtiyacınız olan kadarını ödersiniz. [Azure depolama](/azure/storage/common/storage-introduction) veri nesneleri için bir yüksek düzeyde ölçeklenebilir nesne deposu, bulut, güvenilir bir Mesajlaşma deposuna ve bir NoSQL deposu için bir dosya sistemi hizmeti sunar. VM'ler için yönetilen ve yönetilmeyen diskler kalıcı ve güvenli disk depolama alanı sağlar.

Bilgi edinmek için nasıl [anabilgisayar depolama Azure'a taşımak](./concepts/mainframe-storage-azure.md).

### <a name="backup-and-recovery"></a>Yedekleme ve kurtarma

Kendi olağanüstü durum kurtarma sitenizi bulundurma, pahalı bir teklifi olabilir. Azure için uygulama kolay ve ekonomik seçenekleri sahip [yedekleme](/azure/backup/backup-introduction-to-azure-backup), [kurtarma](/azure/site-recovery/site-recovery-overview), ve [yedeklilik](/azure/storage/common/storage-redundancy) yerel ve bölgesel düzeylerde veya coğrafi olarak yedeklilik aracılığıyla.

## <a name="azure-government-for-mainframe-migrations"></a>Azure kamu için ana bilgisayar geçişi

Birçok Kamu sektörü varlıkları, ana bilgisayar uygulamalarını modern daha esnek bir platform için taşıma mutluluk duyarız. Microsoft Azure kamu, genel Microsoft Azure platformu fiziksel olarak ayrılmış bir örneğini — federal, devlet ve yerel kamu sistemler için paketlenir. Bunu, birinci sınıf güvenlik, koruma ve özellikle ABD devlet kurumları ile bunların iş ortakları için Uyumluluk hizmetleri sağlar.

Bu ortam türünü gerektiren sistemleri için FedRAMP yüksek etkili için Operate (P-ATO) için bir Provisional Authority Azure kamu yararlanmaya hak kazanırsınız.

Başlamak için Yükle [ana bilgisayar uygulamalarını Microsoft Azure kamu bulutunda](https://azure.microsoft.com/resources/microsoft-azure-government-cloud-for-mainframe-applications/en-us/).

## <a name="next-steps"></a>Sonraki adımlar

Sorun bizim [iş ortakları](partner-workloads.md) geçirme veya ana bilgisayar uygulamalarınızı yeniden barındırma yardımcı olacak. Bir iş ortağı çözümü seçme hakkında ayrıntılı yönergeler için bkz. [Platform Modernizasyonu Alliance](https://www.platformmodernization.org/pages/mainframe.aspx) Web sitesi.

Ayrıca bkz:

- [Ana bilgisayar konuları hakkında teknik incelemeler](mainframe-white-papers.md)
- [Ana bilgisayar geçişi](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/overview)
- [Sorun giderme](/azure/virtual-machines/troubleshooting/)
- [Azure'a geçiş için ana bilgisayar demystifying](https://azure.microsoft.com/resources/demystifying-mainframe-to-azure-migration/)

<!-- INTERNAL LINKS -->
[microfocus-get-started]: /microfocus/get-started.md
[microfocus-setup]: /microfocus/set-up-micro-focus-azure.md
[microfocus-demo]: /microfocus/demo.md
[ibm-get-started]: /ibm/get-started.md
[ibm-install-z]: /ibm/install-ibm-z-environment.md
[ibm-demo]: /ibm/demo.md
