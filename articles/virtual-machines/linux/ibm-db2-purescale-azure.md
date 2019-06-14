---
title: Azure üzerinde IBM DB2 pureScale
description: Bu makalede, Azure'da bir IBM DB2 pureScale ortamında çalıştırmaya yönelik bir mimari göstereceğiz.
services: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
editor: edprice
tags: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/09/2018
ms.author: njray
ms.openlocfilehash: 1622de0cccdbc8fee0681e209e756b30da292d3c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60543020"
---
# <a name="ibm-db2-purescale-on-azure"></a>Azure üzerinde IBM DB2 pureScale

IBM DB2 pureScale ortamı, yüksek kullanılabilirlik ve ölçeklenebilirlik Linux işletim sistemlerinde için Azure veritabanı kümesi sağlar. Bu makalede, Azure'da DB2 pureScale çalıştırmaya yönelik bir mimari gösterilmektedir.

## <a name="overview"></a>Genel Bakış

Kuruluşlar, kendi çevrimiçi işlem işleme (OLTP) ihtiyaçları için ilişkisel veritabanı yönetim sistemi (RDBMS) platformları uzun kullandınız. Bugünlerde çoğu veritabanı ana bilgisayar tabanlı ortamlarını Azure'a bir şekilde kapasiteyi genişletmeniz, maliyetleri azaltmak ve sürekli işletim maliyeti yapısı korumak için geçiriliyor.

Geçiş çoğunlukla daha eski bir platform modernleştirme, ilk adımıdır. Örneğin, bir kurumsal müşteri kısa bir süre önce azure'da IBM DB2 pureScale z/OS üzerinde çalışan, IBM DB2 ortam rehosted. Aynı orijinal ortama rağmen Linux üzerinde IBM DB2 pureScale benzer yüksek kullanılabilirlik ve ölçeklenebilirlik özellikleri IBM DB2 anabilgisayar üzerindeki bir paralel Sysplex yapılandırmasında çalışan z/OS için sunar.

Bu makalede, bu Azure geçişi için kullanılan mimarisini açıklar. Müşteri, Red Hat Linux 7.4 yapılandırmasını test etmek için kullanılır. Bu sürüm, Azure Marketi'nde kullanılabilir. Bir Linux dağıtımı seçmeden önce şu anda desteklenen sürümleri doğrulamak emin olun. Ayrıntılar için belgelere bakın [IBM DB2 pureScale](https://www.ibm.com/support/knowledgecenter/SSEPGG) ve [GlusterFS](https://docs.gluster.org/en/latest/).

Bu makalede, DB2 uygulama planınız için bir başlangıç noktasıdır. İş gereksinimlerinizi farklılık gösterir, ancak aynı temel düzeni uygular. Bu mimari deseni, azure'da çevrimiçi analitik işlem (OLAP) uygulamaları için de kullanabilirsiniz.

Bu makalede farklar ve IBM DB2 pureScale Linux üzerinde çalışan bir IBM DB2 veritabanı z/OS için taşımak için olası geçiş görevleri ele alınmamıştır. Ve DB2 z/OS için DB2 pureScale taşımak için boyutlandırma tahminleri ve iş yükü analizleri sağlamaz. 

Ortamınız için en iyi DB2 pureScale mimarisi hakkında karar vermenize yardımcı olmak için tam olarak boyutlandırma tahmin edin ve bir varsayım olun öneririz. Kaynak sistemde DB2 z/OS paralel dikkate aldığınızdan emin olun Sysplex mimarisi verilerin paylaşımı, tesis eşlenmesiyle yapılandırma ve Dağıtılmış veri özelliği (DDF) kullanım istatistikleri.

> [!NOTE]
> Bu makalede bir yaklaşım DB2 geçiş, ancak diğerleri vardır. Örneğin, DB2 pureScale sanallaştırılmış şirket içi ortamlarda da çalıştırabilirsiniz. IBM DB2 Microsoft Hyper-V üzerinde çeşitli yapılandırmalarda destekler. Daha fazla bilgi için [DB2 pureScale sanallaştırma mimarisi](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/r0061462.html) IBM bilgi Merkezi'nde.

## <a name="architecture"></a>Mimari

Azure'da yüksek kullanılabilirlik ve ölçeklenebilirlik desteklemek için ölçeği genişletme, paylaşılan veri mimarisi için DB2 pureScale kullanabilirsiniz. Aşağıdaki örnek mimarisi müşteri geçiş kullanılır.

![Azure sanal makinelerinde gösteren depolama ve ağ DB2 pureScale](media/db2-purescale-on-azure/pureScaleArchitecture.png "DB2 pureScale Azure sanal makinelerinde gösteren depolama ve ağ")


Diyagramda bir DB2 pureScale küme için gereken mantıksal katmanları gösterilmektedir. Bunlar, bir istemci, Yönetim için önbelleğe alma için veritabanı altyapısı için ve paylaşılan depolama için sanal makineleri içerir. 

Veritabanı altyapısı düğümlerin yanı sıra diyagram özellikleri (CFs) önbelleğe alma küme için kullanılan iki düğüm içerir. En az iki düğüm veritabanı altyapısı kendisi için kullanılır. Bir pureScale kümeye ait bir DB2 sunucu bir üye adı verilir. 

Küme, depolama ölçek genişletme ve yüksek kullanılabilirlik sağlamak için üç düğümlü GlusterFS paylaşılan depolama kümesi için iSCSI aracılığıyla bağlanır. DB2 pureScale Linux çalıştıran Azure sanal makinelere yüklenir.

Bu yaklaşım boyut ve ölçek kuruluşunuzun için değiştirebileceğiniz bir şablonudur. Aşağıdakilere bağlıdır:

-   İki veya daha fazla veritabanı üyeleri, en az iki CF düğüm ile birleştirilir. Düğümlerin paylaşılan bellek ve paylaşılan erişim ve kilit çakışması etkin üyelerinden denetlemek için genel kilit Yöneticisi'ni (GLM) Hizmetleri için genel arabellek havuzu (GBP) yönetin. Birincil ve ikincil, yük devretme CF düğümü olarak diğer bir CF düğümünü görür. Tek bir ortamda bir hata noktası önlemek için en az dört düğüm bir DB2 pureScale kümesi gerektirir.

-   Yüksek performanslı paylaşılan depolama (P30 boyutu diyagramda gösterilmiştir). Gluster FS düğümlerinin her biri, bu depolama kullanır.

-   Paylaşılan depolama ve veri üyeleri için yüksek performanslı ağ.

### <a name="compute-considerations"></a>İşlem değerlendirmeleri

Bu mimari uygulama, depolama ve veri katmanları Azure sanal makinelerde çalıştırır. [Dağıtım Kurulum betikleri](https://aka.ms/db2onazure) aşağıdaki oluşturun:

-   Bir DB2 pureScale kümesi. Kurulumunuzu temel Azure'da ihtiyacınız işlem kaynaklarının türüne bağlıdır. Genel olarak, iki yaklaşım da kullanabilirsiniz:

    -   Bir çok düğümlü, yüksek performanslı hesaplama (HPC) kullanın-style ağ paylaşılan depolama eriştiği küçük ve orta ölçekli örnekleri. Bu HPC tür yapılandırma için Azure bellek için iyileştirilmiş E serisi veya depolama için iyileştirilmiş L serisi [sanal makineler](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) gerekli işlem gücünü sağlar.

    -   Daha az büyük sanal makine örnekleri için veri altyapıları kullanın. Büyük örnekler için en büyük bellek için iyileştirilmiş [M serisi](https://azure.microsoft.com/pricing/details/virtual-machines/series/) sanal makineler ağır bellek içi iş yükleri için uygundur. DB2 çalıştırmak için kullanılan mantıksal bölüm (LPAR) boyutuna bağlı olarak ayrılmış bir örneği gerekebilir.

-   DB2 CF bellek için iyileştirilmiş sanal makinelerin E serisi veya L serisi gibi kullanır.

-   GlusterFS depolama kullanan standart\_DS4\_Linux çalıştıran v2 sanal makineler.

-   GlusterFS Sıçrama kutusu standardıdır\_DS2\_v2 Linux çalıştıran sanal makine.

-   Standart bir istemcidir\_DS3\_(test etmek için kullanılır) Windows çalıştıran v2 sanal makinesi.

-   Bir Tanık standardıdır\_DS3\_(DB2 pureScale için kullanılır) Linux çalıştıran v2 sanal makine.

> [!NOTE]
> Bir DB2 pureScale küme en az iki DB2 örneği gerektirir. Ayrıca, bir önbellek örneği ve bir kilit yöneticisi örneği gerektirir.

### <a name="storage-considerations"></a>Depolama hakkında dikkat edilmesi gerekenler

Oracle RAC gibi bir yüksek performanslı blok g/ç genişleme veritabanı DB2 pureScale olur. En büyük kullanmanızı öneririz [Azure premium SSD](disks-types.md) gereksinimlerinize uyan seçeneği. Üretim ortamları genellikle daha fazla depolama kapasitesi gerekirken depolama seçenekleri daha küçük geliştirme ve test ortamları için uygun olabilir. Örnek mimarisi kullanır [P30](https://azure.microsoft.com/pricing/details/managed-disks/) IOPS kendi oranını boyutuna ve fiyat nedeniyle. Boyutundan bağımsız olarak, en iyi performans için Premium depolama kullanın.

DB2 pureScale paylaşılan kullanır-her şeyi mimarisi, tüm verilerin bulunduğu tüm küme düğümlerinden erişilebilir. Premium depolama örneklerinde, isteğe bağlı olup olmadığını veya ayrılmış örnek paylaşılabilir olmalıdır.

200 terabayta (TB) büyük bir DB2 pureScale küme gerektirebilir veya daha fazla premium depolama, 100.000 IOPS ile paylaşılan. DB2 pureScale Azure'da kullanabileceğiniz bir iSCSI blok arabirimi destekler. İSCSI arabirimi, GlusterFS, S2D veya başka bir aracı ile uygulayabileceğiniz bir paylaşılan depolama kümesi gerektirir. Bu tür bir çözüm, Azure'da bir sanal depolama alanı ağı (vSAN) cihaz oluşturur. DB2 pureScale vSAN sanal makineler arasında veri paylaşımı için kullanılan kümelenmiş dosya sistemini yüklemek için kullanır.

Örnek mimarisi, GlusterFS, bulut depolama için iyileştirilmiş ücretsiz, ölçeklenebilir, açık kaynaklı bir dağıtılmış dosya sistemi kullanır.

### <a name="networking-considerations"></a>Ağ konusunda dikkat edilmesi gerekenler

IBM DB2 pureScale kümedeki tüm üyeleri için InfiniBand ağı önerir. DB2 pureScale de doğrudan uzak bellek erişimi (RDMA), mümkün olan durumlarda CFs için kullanır.

Kurulumu sırasında oluşturduğunuz Azure [kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) içeren tüm sanal makineleri için. Genel olarak, kaynakları kendi ömürlerine ve kim yönetecek göre gruplandırın. Sanal makineler bu mimaride gerekli [hızlandırılmış](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/). Bir sanal makine için tek köklü g/ç Sanallaştırması (SR-IOV) ile tutarlı, son derece düşük ağ gecikme süresi sağlayan bir Azure özelliğidir.

Her Azure sanal makine alt ağı varsa bir sanal ağa dağıtılır: ana, Gluster FS ön uç (gfsfe), Gluster FS arka uç (bfsbe), DB2 pureScale (db2be) ve DB2 pureScale front end (db2fe). Yükleme betiğini de birincil oluşturur [NIC](https://docs.microsoft.com/azure/virtual-machines/linux/multiple-nics) ana alt ağdaki sanal makinelere.

Kullanım [ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) sanal ağda ağ trafiğini kısıtlamak ve alt ağları yalıtmak için.

Azure üzerinde TCP/IP ağ bağlantısı depolamak için kullanılacak DB2 pureScale gerekir.

## <a name="next-steps"></a>Sonraki adımlar

-   [Bu mimariyi azure'a dağıtmak](deploy-ibm-db2-purescale-azure.md)
