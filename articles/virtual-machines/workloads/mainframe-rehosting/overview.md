---
title: Azure sanal makinelerinde ana bilgisayar yeniden barındırma | Microsoft Docs
description: Ana bilgisayar iş yüklerinizi Microsoft Azure üzerinde sanal makineleri (VM'ler) kullanarak IBM Z-tabanlı sistemler gibi yeniden barındırın.
services: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
editor: edprice
tags: ''
keywords: ''
ms.openlocfilehash: 067ab7538924f4aef7c48731d10fa7e68855214a
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2019
ms.locfileid: "56739767"
---
# <a name="mainframe-rehosting-on-azure-virtual-machines"></a>Azure sanal makinelerinde ana bilgisayar yeniden barındırma

Geçirme iş yükleri ana bilgisayardan bulut ortamları, altyapınızı modernleştirin ve genellikle maliyetlerinden tasarruf sağlar. Çok sayıda iş yükü, veritabanlarının adlarını güncelleştirme gibi yalnızca küçük bir kod değişikliği yapmadan, Azure'a aktarılabilir.

Terim *anabilgisayar* genellikle büyük bilgisayar sistemine başvuruyor ancak şu anda dağıtılmış büyük bir çoğunluğu IBM sistem Z sunucuları veya MVS, DOS, VSE, işletim sistemi/390 veya z/OS çalıştıran IBM plug-compatible sistemleri.

Azure sanal makine'de (VM) yalıtmak ve tek bir örneği üzerinde belirli bir uygulama için kaynakları yönetmek için kullanılır. IBM z/OS gibi ana bilgisayarlar, bu amaç için mantıksal bölümleri (LPARS) kullanın. Bir ana bilgisayar için IBM Db2 veritabanı bir CICS bölgeyle ilişkili COBOL programları için bir LPAR ve ayrı LPAR kullanabilir. Tipik bir [azure'da n katmanlı uygulama](https://docs.microsoft.com/azure/architecture/reference-architectures/n-tier/n-tier-sql-server) her katman için alt ağlar ile bölümlenmiş bir sanal ağa Azure Vm'leri dağıtır.

Azure sanal makineleri ana bilgisayar öykünmesi ortamları ve lift-and-shift ile taşıma senaryoları destekleyen derleyiciler çalıştırabilirsiniz. Geliştirme ve test, bir Azure geliştirme ve test ortamı için bir ana bilgisayardan geçirmek için ilk iş yükleri arasında çoğunlukla olur. Taklit edebilir ortak sunucusu bileşenleri işlem çevrimiçi işlem (gerçekleştirme OLTP), toplu işlem ve veri alımı sistemleri aşağıdaki şekilde gösterildiği gibi ekleyin.

![](media/01-overview.png)

Diğer bir iş ortağı çözümü kullanarak Azure'da rehosted ancak bazı ana bilgisayar iş yüklerini Azure'a göreli kolaylıkla geçirilebilir. Bir iş ortağı çözümü seçme hakkında ayrıntılı yönergeler için [Azure ana bilgisayar geçiş Merkezi](https://azure.microsoft.com/migration/mainframe/) yardımcı olabilir.

## <a name="set-up-devtest-environment-using-a-micro-focus-rehosting-platform"></a>Micro odak rehosting platformunu kullanarak geliştirme ve test ortamını ayarlama

Micro odak kuruluş sunucusu kullanılabilir platformları yeniden barındırma büyük anabilgisayar biridir. Bunu bir daha ucuz x86 üzerinde z/OS iş yüklerinizi çalıştırmak Azure platformunda.

Başlamak için aşağıdaki makalelere bakın:

- [Micro odak Enterprise Server 4.0 ve kurumsal Geliştirici 4.0 Azure'a yükleme](./microfocus/set-up-micro-focus-on-azure.md)
- [Micro odak Kurumsal Geliştirici 4.0 azure'da Micro odak CICS BankDemo ' ayarlayın](./microfocus/demo.md)

## <a name="tmaxsoft-openframe-on-azure"></a>TmaxSoft OpenFrame on Azure

Mevcut ana bilgisayar varlıklarınızı kaldırma ve bunları Azure'a kaydırma kolaylaştırır çözümü yeniden barındırma popüler bir ana bilgisayar TmaxSoft OpenFrame olur. Azure'da OpenFrame ortam, geliştirme, tanıtımlar, test veya üretim iş yükleri için uygundur.

Başlamak için Yükle [yükleme TmaxSoft OpenFrame azure'da](https://azure.microsoft.com/resources/install-tmaxsoft-openframe-on-azure/) e-Kitap.

## <a name="set-up-a-devtest-environment-using-ibm-z-devtest-120"></a>IBM Z geliştirme/Test 12.0 kullanılarak bir geliştirme ve test ortamını ayarlama

IBM Z geliştirme ve Test Ortamı (IBM zD & T) ayarlayan bir üretim dışı ortamda azure'da geliştirme, test ve Tanıtımlar z/OS tabanlı uygulamalar için kullanabilirsiniz.

Öykünme ortamı azure'da uygulama geliştiricilerin denetlenen dağıtımları (ADCDs) aracılığıyla Z örnekleri çeşitli barındırabilirsiniz. Azure ve Azure Stack üzerinde zD & T kişisel Edition, zD & T paralel Sysplex ve zD & T Enterprise Edition çalıştırabilirsiniz.

Başlamak için aşağıdaki makalelere bakın:

-   [IBM zD & T 12.0 azure'da ayarlama](./ibm/install-ibm-z-environment.md)
-   [ZD & T ADCD ayarlayın](./ibm/demo.md)

## <a name="migrate-ibm-db2-purescale-to-azure"></a>IBM DB2 pureScale Azure'a geçirme

IBM DB2 pureScale ortamı, yüksek kullanılabilirlik ve ölçeklenebilirlik Linux işletim sistemlerinde için Azure veritabanı kümesi sağlar. Aynı orijinal ortama rağmen Linux üzerinde IBM DB2 pureScale benzer yüksek kullanılabilirlik ve ölçeklenebilirlik özellikleri IBM DB2 anabilgisayar üzerindeki bir paralel Sysplex yapılandırmasında çalışan z/OS için sunar.

Başlamak için bkz: [IBM DB2 pureScale azure'da](https://docs.microsoft.com/azure/virtual-machines/linux/ibm-db2-purescale-azure).

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Anabilgisayar iş yükleri için Azure altyapı (Iaas) hizmet olarak geçirdiğinizde, çeşitli Azure Vm'leri de dahil olmak üzere isteğe bağlı ve ölçeklenebilir işlem kaynağı türlerinden de seçebilirsiniz. Azure'un sunduğu çeşitli [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/overview) ve [Windows](https://docs.microsoft.com/azure/virtual-machines/windows/overview) VM'ler.

### <a name="high-availability-and-failover"></a>Yüksek kullanılabilirlik ve yük devretme

Azure'un sunduğu taahhüt tabanlı hizmet düzeyi sözleşmeleri (SLA'lar), burada birden çok 9s kullanılabilirlik, hizmetleri yerel ya da coğrafi tabanlı çoğaltma ile en iyi duruma getirilmiş varsayılan,. [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) şartları, Azure’un tamamının kullanılabilirlik garantisini açıklamaktadır.

Yük devretme için Azure Iaas Vm'leri gibi yük devretme kümeleme örnekleri gibi belirli sistem işlevleri kullanır ve [kullanılabilirlik kümeleri](https://docs.microsoft.com/azure/virtual-machines/windows/regions-and-availability#availability-sets). Kullandığınızda, Azure platformu hizmet (PaaS) kaynakları, gibi [Azure SQL veritabanı](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview) ve [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction), platform yük devretme işlemleri otomatik olarak işler.

### <a name="scalability"></a>Ölçeklenebilirlik

Ana bilgisayarlar genellikle, bulut ortamında ölçeğinizi çalışırken ölçeklendirin. Azure'un sunduğu çeşitli [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/sizes) ve [Windows](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) gereksinimlerinizi karşılayacak şekilde boyutları. Bulut ayrıca yukarı veya aşağı eşleştirme tam kullanıcı belirtimleri ölçeklendirir. İşlem gücü, depolama ve Hizmetleri [ölçek](https://docs.microsoft.com/azure/architecture/best-practices/auto-scaling) kullanım tabanlı faturalandırma modeli altında isteğe bağlı.

### <a name="storage"></a>Depolama

Bulutta, esnek, ölçeklenebilir depolama seçenekleriyle sahiptir ve yalnızca ihtiyacınız olan kadarını ödersiniz. [Azure depolama](https://docs.microsoft.com/azure/storage/common/storage-introduction) veri nesneleri için bir yüksek düzeyde ölçeklenebilir nesne deposu, bulut, güvenilir bir Mesajlaşma deposuna ve bir NoSQL deposu için bir dosya sistemi hizmeti sunar. VM'ler için yönetilen ve yönetilmeyen diskler kalıcı ve güvenli disk depolama alanı sağlar.

### <a name="backup-and-recovery"></a>Yedekleme ve kurtarma

Kendi olağanüstü durum kurtarma sitenizi bulundurma, pahalı bir teklifi olabilir. Azure için uygulama kolay ve ekonomik seçenekleri sahip [yedekleme](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup), [kurtarma](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), ve [yedeklilik](https://docs.microsoft.com/azure/storage/common/storage-redundancy) yerel ve bölgesel düzeylerde veya coğrafi olarak yedeklilik aracılığıyla.

## <a name="azure-government-for-mainframe-migrations"></a>Azure kamu için ana bilgisayar geçişi

Birçok Kamu sektörü varlıkları, ana bilgisayar uygulamalarını modern daha esnek bir platform için taşıma mutluluk duyarız. Microsoft Azure kamu, bulut hizmetleri genel Microsoft Azure platformunu temel alan ancak paketlenmiş federal, devlet ve yerel kamu sistemler için fiziksel olarak ayrı bir örneğidir. Bunu, birinci sınıf güvenlik, koruma ve özellikle ABD devlet kurumları ile bunların iş ortakları için Uyumluluk hizmetleri sağlar.

Bu ortam türünü gerektiren sistemleri için FedRAMP yüksek etkili için Operate (P-ATO) için bir Provisional Authority Azure kamu yararlanmaya hak kazanırsınız. 

Başlamak için Yükle [ana bilgisayar uygulamalarını Microsoft Azure kamu bulutunda](https://azure.microsoft.com/resources/microsoft-azure-government-cloud-for-mainframe-applications/en-us/).

## <a name="learn-more"></a>Daha fazla bilgi edinin

Bir ana bilgisayar geçişi, sunduğumuz kapsamlı düşünüyorsanız, [iş ortağı](partner-workloads.md) ekosistemi yardımcı olması kullanılabilir. Bir iş ortağı çözümü seçme hakkında ayrıntılı yönergeler için bkz [Platform Modernizasyonu Alliance](https://www.platformmodernization.org/pages/mainframe.aspx).

Ayrıca bkz:

-   [Ana bilgisayar geçişi](https://docs.microsoft.com/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/overview)
-   [Sorun giderme](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/)
-   [Azure'a geçiş için ana bilgisayar demystifying](https://azure.microsoft.com/resources/demystifying-mainframe-to-azure-migration/en-us/)

<!-- INTERNAL LINKS -->
[microfocus-get-started]: /microfocus/get-started.md
[microfocus-setup]: /microfocus/set-up-micro-focus-on-azure.md
[microfocus-demo]: /microfocus/demo.md
[ibm-get-started]: /ibm/get-started.md
[ibm-install-z]: /ibm/install-ibm-z-environment.md
[ibm-demo]: /ibm/demo.md
