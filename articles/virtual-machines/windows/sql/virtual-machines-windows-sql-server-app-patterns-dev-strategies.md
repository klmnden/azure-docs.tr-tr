---
title: Vm'lerde SQL Server uygulama desenleri | Microsoft Docs
description: Bu makale, Azure vm'lerinde SQL Server için uygulama desenleri kapsar. Bu çözüm mimarları ve geliştiricileri bir temel iyi uygulama mimarisi ve tasarımı için sağlar.
services: virtual-machines-windows
documentationcenter: na
author: ninarn
manager: craigg
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: 41863c8d-f3a3-4584-ad86-b95094365e05
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/31/2017
ms.author: ninarn
ms.openlocfilehash: 51d572ac324d0bc875e7ed81879f2456eeea4fbb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65506609"
---
# <a name="application-patterns-and-development-strategies-for-sql-server-in-azure-virtual-machines"></a>Azure Sanal Makineler'de SQL Server için Uygulama Desenleri ve Geliştirme Stratejileri
[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="summary"></a>Özet:
Hangi uygulama düzeni veya kullanmak için desenler belirleme için SQL Server tabanlı uygulamalarınızı azure'da ortam bir önemli tasarım kararı ve SQL Server ve her bir Azure altyapı bileşeni birlikte nasıl çalıştığını, düz bir anlayış gerektirir. Azure altyapı hizmetlerinde SQL Server ile kolayca geçirme, tutabilir ve edebilirsiniz yerleşik Windows Server için Azure sanal makineler'de açma mevcut SQL Server uygulamalarınızı izleyin.

Bu makalenin hedefi çözüm mimarları ve geliştiricileri bir temel için iyi uygulama mimarisi ve tasarımı, mevcut uygulamaları Azure yanı sıra azure'da yeni uygulamalar geliştirmek için geçiş sırasında izleyebilirsiniz sağlamaktır.

Her uygulama düzeni için bir şirket içi senaryo, ilgili bulut etkin çözüm ve ilgili teknik öneriler bulabilirsiniz. Ayrıca, böylece uygulamalarınız doğru tasarım makalede Azure'a özel geliştirme stratejileri açıklanır. Çok sayıda olası uygulama desenleri nedeniyle, mimarların ve geliştiricilerin uygulamalarını ve kullanıcılar için en uygun Düzen seçmelisiniz önerilir.

**Teknik katkıda:** Luıs Carlos Vargas Herring, Madhan Arumugam Ramakrishnan

**Teknik İnceleme:** Corey Sanders, Drew McDaniel, Narayan Annamalai, Ben Nir Mashkowski, Sanjay Mishra, Silvano Coriani, Stefan Schackow Tim Hickey, Tim Wieman Xin Jin

## <a name="introduction"></a>Giriş
Farklı uygulama katmanları ayrı bileşenler olduğu gibi farklı makinelerde de bileşenlerinin ayrılarak türlerde n katmanlı uygulamalar geliştirebilirsiniz. Örneğin, istemci uygulaması yerleştirebilirsiniz ve bileşenleri bir makineye, ön uç web katmanı ve veri erişim katmanı bileşenleri başka bir makinede bulunan ve başka bir makinede bir arka uç veritabanı katmanı iş kuralları. Bu tür yapılandırma her bir katman birbirinden yalıtmaya yardımcı olur. Verilerin nereden geldiğini değiştirirseniz, istemci veya web uygulamasına ancak yalnızca veri erişim katmanı bileşenlerini değiştirmek gerekmez.

Tipik bir *n katmanlı* sunu katmanı, iş katmanı ve veri katmanı uygulaması içerir:

| Katman | Açıklama |
| --- | --- |
| **Sunu** |*Sunu katmanı* (web katmanı, ön uç katmanına) olan kullanıcıların uygulamayla etkileşim katmanı. |
| **İş** |*İş katmanı* (orta katman) sunu katmanı katmanı ve veri katmanı birbirleri ile iletişim kurmak için kullanır ve sistemde temel işlevlerini içerir. |
| **Veri** |*Veri katmanı* temelde (örneğin, SQL Server çalıştıran bir sunucu) bir uygulamanın verilerini depolayan sunucusudur. |

Uygulama katmanları, uygulama bileşenlerini ve işlevi mantıksal gruplandırmalarını açıklamak; Katmanları ayrı fiziksel sunucular, bilgisayarlar, ağ veya uzak konumlardaki bileşenleri ve işlevleri fiziksel dağıtımını açıklayan ise. Uygulama katmanları aynı fiziksel bilgisayarda (aynı katman) bulunabilir ya da ayrı bilgisayarlar (n-katmanlı) dağıtılmış ve her katman bileşenlerde iyi tanımlanmış arabirimler üzerinden diğer katmanlarında bileşenleriyle iletişim kurmak. Terim katmanının iki katmanlı ve üç katmanlı n katmanlı gibi fiziksel dağılım desenleri başvuran olarak düşünebilirsiniz. A **2 katmanlı uygulama düzeni** iki uygulama katmanları içerir: uygulama sunucusu ve veritabanı sunucusu. Uygulama sunucusu ve veritabanı sunucusu arasında doğrudan iletişim gerçekleşir. Uygulama sunucusu, hem web katmanı ve iş katmanı bileşenlerini içerir. İçinde **3 katmanlı uygulama düzeni**, üç uygulama katmanı vardır: web sunucusu, iş mantığı katmanı ve/veya iş katmanı veri erişim bileşenleri içeren, uygulama sunucusu ve veritabanı sunucusu. Web sunucusu ve veritabanı sunucusu arasındaki iletişim, uygulama sunucusu gerçekleşir. Uygulama katmanları ve katmanları hakkında ayrıntılı bilgi için bkz. [Microsoft Uygulama Mimarisi Kılavuzu](https://msdn.microsoft.com/library/ff650706.aspx).

Bu makaleyi okuduktan başlamadan önce SQL Server ve Azure'nın temel kavramlar hakkında bilgilere sahip olmalıdır. Bilgi için [SQL Server Books Online](https://msdn.microsoft.com/library/bb545450.aspx), [Azure sanal Makineler'de SQL Server](virtual-machines-windows-sql-server-iaas-overview.md) ve [Azure.com](https://azure.microsoft.com/).

Bu makale, son derece karmaşık kurumsal uygulamalar yanı sıra, basit uygulamalar için uygun birkaç uygulama desenleri açıklar. Her desen gerçekleşen önce azure'da kullanılabilen veri depolama hizmetleriyle gibi planladığınızdan öneririz [Azure depolama](../../../storage/common/storage-introduction.md), [Azure SQL veritabanı](../../../sql-database/sql-database-technical-overview.md), ve [SQL Bir Azure sanal makinesinde sunucu](virtual-machines-windows-sql-server-iaas-overview.md). Uygulamalarınız için en iyi tasarım kararları için ne zaman açıkça hangi veri depolama hizmeti kullanılacağını anlayın.

### <a name="choose-sql-server-in-an-azure-virtual-machine-when"></a>Bir Azure sanal Makineler'de SQL Server'ı seçin:
* SQL Server ve Windows üzerinde denetim ihtiyacınız vardır. Örneğin, bu içerebilir SQL Server sürümü, özel düzeltmeleri, performans yapılandırması, vs.
* Şirket içi SQL Server ile tam bir uyumluluk gerekir ve mevcut uygulamaları azure'a taşımak istediğiniz-olduğu.
* Azure ortamının özelliklerinden yararlanmak istiyorsanız ancak Azure SQL veritabanı, uygulamanızın gerektirdiği tüm özelliklerini desteklemez. Bu, aşağıdaki alanları içerebilir:
  
  * **Veritabanı boyutu**: Bu makalede güncelleştirildiği sırada, SQL veritabanı en fazla 1 TB veri veritabanını destekler. 1 TB'den fazla veri uygulamanızın gerektirdiği ve özel parçalama çözümlerin uygulanmasını istemiyorsanız, bir Azure sanal Makineler'de SQL Server kullanmanız önerilir. En son bilgiler için bkz. [Azure SQL veritabanı kullanıma ölçeklendirme](https://msdn.microsoft.com/library/azure/dn495641.aspx), [DTU-Based satın alma modeli](../../../sql-database/sql-database-service-tiers-dtu.md), ve [sanal çekirdek tabanlı satın alma modeli](../../../sql-database/sql-database-service-tiers-vcore.md)(Önizleme).
  * **HIPAA Uyumluluk**: Sağlık Hizmeti müşteriler ve bağımsız yazılım satıcılarına (ISV) seçin [Azure sanal Makineler'de SQL Server](virtual-machines-windows-sql-server-iaas-overview.md) yerine [Azure SQL veritabanı](../../../sql-database/sql-database-technical-overview.md) bir Azure sanal Makine'de SQL Server kapatıldığından HIPAA iş ortağı sözleşmesi tarafından (BAA). Uyumluluk hakkında daha fazla bilgi için bkz: [Microsoft Azure Trust Center: Uyumluluk](https://azure.microsoft.com/support/trust-center/compliance/).
  * **Örnek düzeyinde özelliklerin**: Şu anda SQL veritabanı, Canlı veritabanı haricinde özellikler desteklemiyor (bağlı sunucular gibi aracı işleri, FILESTREAM, hizmet Aracısı, vs.). Daha fazla bilgi için [Azure SQL veritabanı kılavuzları ve sınırlamaları](https://msdn.microsoft.com/library/azure/ff394102.aspx).

## <a name="1-tier-simple-single-virtual-machine"></a>1 katmanlı (Basit): tek bir sanal makine
Bu uygulama modelinde, uygulamanızı SQL Server ve veritabanı bir tek başına sanal makine azure'da dağıtın. Aynı sanal makineye, istemci/web uygulaması, iş bileşenleri, veri erişim katmanı ve veritabanı sunucusu içerir. Sunu, iş ve veri erişim kodu mantıksal olarak ayrılır, ancak fiziksel bir tek sunuculu makinede bulunan. Müşterilerin çoğu bu uygulama düzeni ile başlatın ve sonra bunlar daha fazla web rolleri veya sanal makineler sistemlerine ekleyerek ölçeği.

Bu uygulama Düzen şu durumlarda yararlı olur:

* Azure platformu, platform veya uygulamanızın gereksinimlerini yanıtlar olup olmadığını değerlendirmek için basit bir geçiş gerçekleştirmek istediğiniz.
* Katmanlar arasındaki gecikme süresini azaltmak için aynı Azure veri merkezinde aynı sanal makinede barındırılan uygulama katmanları tutmak istediğiniz.
* Geliştirme sağlayın ve kısa süreler için test ortamlarını hızlıca istiyorsunuz.
* Değişen iş yükü düzeyleri için stres gerçekleştirmek istediğiniz, ancak aynı anda sahip çok sayıda fiziksel makineyi her zaman korumak istediğiniz değil.

Aşağıdaki diyagramda, basit bir senaryo ve azure'da tek bir sanal makinede etkin bulut çözümünün nasıl dağıtabileceğiniz şirket içinde gösterir.

![1 katmanlı uygulama düzeni](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728008.png)

Ölçeklenebilirlik ve güvenlik kaygıları nedeniyle ayrı bir katman kullanmalısınız sürece sunu katmanı olarak aynı fiziksel katmanda (iş mantığı ve verileri bileşenlerine erişmek) iş katmanı dağıtma uygulama performansını en üst düzeye çıkarabilirsiniz.

Bu çok yaygın bir düzen ile başlaması olduğundan, makaleyi geçiş verilerinizi SQL Server VM'nize taşımak için kullanışlı olabilir: [Bir veritabanını Azure VM'deki SQL Server'a geçirme](virtual-machines-windows-migrate-sql.md).

## <a name="3-tier-simple-multiple-virtual-machines"></a>3 katmanlı (Basit): birden çok sanal makine
Bu uygulama modelinde, her uygulama katmanı farklı bir sanal makine yerleştirme tarafından bir 3 katmanlı uygulama azure'da dağıtın. Bu, bir kolayca büyütme ve ölçek genişletme senaryoları için esnek bir ortam sağlar. İstemci/web uygulamanızı bir sanal makine içeriyorsa, diğeri iş bileşenlerinizi barındıran ve diğeri veritabanı sunucusu barındırır.

Bu uygulama Düzen şu durumlarda yararlı olur:

* Karmaşık veritabanı uygulamaları için Azure sanal makineleri geçirmek istediğiniz.
* Farklı bölgelerde barındırılması için farklı uygulama katmanı kullanmanız gerekir. Örneğin, raporlama amacıyla birden fazla bölgede dağıtılan veritabanları paylaşılan.
* Kurumsal uygulamalar, şirket içinde sanallaştırılmış platformlarından Azure sanal makineler için taşımak istediğiniz. Kurumsal uygulamalar üzerinde ayrıntılı bir açıklaması için [Kurumsal uygulama nedir](https://msdn.microsoft.com/library/aa267045.aspx).
* Geliştirme sağlayın ve kısa süreler için test ortamlarını hızlıca istiyorsunuz.
* Değişen iş yükü düzeyleri için stres gerçekleştirmek istediğiniz, ancak aynı anda sahip çok sayıda fiziksel makineyi her zaman korumak istediğiniz değil.

Aşağıdaki diyagramda, Azure'da her uygulama katmanı farklı bir sanal makine yerleştirerek basit bir 3 katmanlı uygulama nasıl yerleştirebilirsiniz gösterir.

![3 katmanlı uygulama düzeni](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728009.png)

Bu uygulama modelinde, her katmanında yalnızca bir sanal makine (VM) yoktur. Azure'da birden fazla VM varsa, sanal bir ağ ayarlamanızı öneririz. [Azure sanal ağı](../../../virtual-network/virtual-networks-overview.md) güvenilen güvenlik sınırı oluşturur ve ayrıca aralarında özel IP adresi iletişim kurmak VM'lerin sağlar. Ayrıca, her zaman tüm Internet bağlantıları için sunu katmanı yalnızca uyguladığınızdan emin olun. Bu uygulama düzenini izleyerek, erişimi denetlemek için ağ güvenlik grubu kurallarını yönetin. Daha fazla bilgi için [Azure portalını kullanarak sanal makinelerinize dış erişime izin vermek](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Diyagramda, Internet protokolleri TCP, UDP, HTTP veya HTTPS olabilir.

> [!NOTE]
> Azure'da bir sanal ağ ayarlama ücretsizdir. Ancak, şirket içine bağlanan VPN ağ geçidi için ücretlendirilirsiniz. Bu ücretlendirme, bağlantının sağlandığı ve kullanılabilir olduğu süre miktarını üzerinde temel alır.
> 
> 

## <a name="2-tier-and-3-tier-with-presentation-tier-scale-out"></a>Katman 2 ve 3 katmanlı ile sunu katmanı ölçek genişletme
Bu uygulama modelinde, her uygulama katmanı farklı bir sanal makine yerleştirerek veritabanı, Katman 2 veya 3 katmanlı uygulama için Azure sanal makineler dağıtın. Ayrıca, sunu katmanı gelen istemci isteklerini artan hacmi nedeniyle ölçeği genişletme.

Bu uygulama Düzen şu durumlarda yararlı olur:

* Kurumsal uygulamalar, şirket içinde sanallaştırılmış platformlarından Azure sanal makineler için taşımak istediğiniz.
* Gelen istemci isteklerini artan hacmi nedeniyle sunu katmanı ölçeğini istiyorsunuz.
* Geliştirme sağlayın ve kısa süreler için test ortamlarını hızlıca istiyorsunuz.
* Değişen iş yükü düzeyleri için stres gerçekleştirmek istediğiniz, ancak aynı anda sahip çok sayıda fiziksel makineyi her zaman korumak istediğiniz değil.
* İsteğe bağlı olarak yukarı ve aşağı ölçeklendirilebilir bir altyapı ortamı kendi istiyorsunuz.

Aşağıdaki diyagramda, nasıl, uygulama katmanları birden çok sanal makine azure'da gelen istemci isteklerini artan hacmi nedeniyle sunu katmanı ölçek genişletilerek yerleştirebilirsiniz gösterir. Diyagramda görüldüğü gibi Azure Load Balancer için birden çok sanal makine arasında trafiği dağıtma ve ayrıca bağlanmak için hangi web sunucu belirleme sorumludur. Web sunucusu bir yük dengeleyicinin arkasına birden çok örnek içeren sunu katmanı yüksek kullanılabilirliğini sağlar.

![Uygulama düzeni - sunu katmanı ölçek genişletme](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728010.png)

### <a name="best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier"></a>Bir katmanda birden çok Vm'si vardır Katman 2, 3 katmanlı veya n-katmanlı desenleri için en iyi uygulamalar
Aynı katman aynı bulut hizmetindeki ve aynı kullanılabilirlik ait sanal makineleri yerleştireceğiniz önerilir ayarlayın. Örneğin, bir dizi web sunucularından birinde yerleştirin **CloudService1** ve **AvailabilitySet1** ve veritabanı sunucuları kümesi **CloudService2** ve  **AvailabilitySet2**. Bir kullanılabilirlik kümesi Azure'da yüksek kullanılabilirlik düğümleri ayrı hata etki alanlarına yerleştirin ve yükseltme etki alanlarında sağlar.

Bir katmanı birden çok VM örneğini yararlanmak için uygulama katmanları arasında Azure Load Balancer'ı yapılandırmanız gerekir. Her katmanda Load Balancer'ı yapılandırmak için her katmanın Vm'lerde yük dengeli uç nokta ayrı olarak oluşturun. Belirli bir katman için öncelikle aynı bulut hizmetinde VM'ler oluşturun. Bu, aynı genel sanal IP adresi olmasını sağlar. Ardından, söz konusu katman sanal makinelerde birinde bir uç nokta oluşturun. Ardından, Yük Dengeleme için bu katmanda diğer sanal makineler aynı uç noktasına atayın. Yük dengeli bir küme oluşturarak, birden çok sanal makine arasında trafiği dağıtma ve ayrıca bir arka uç VM düğüm başarısız olduğunda bağlanmak için hangi düğümün belirlemek için yük dengeleyici izin. Örneğin, web sunucusu bir yük dengeleyicinin arkasına birden çok örnek içeren sunu katmanı yüksek kullanılabilirliğini sağlar.

En iyi uygulama, her zaman tüm internet bağlantıları için sunu katmanı'ı ilk uyguladığınızdan emin olun. İş katmanı sunu katmanını erişir ve ardından iş katmanı, veri katmanı erişir. Sunu katmanı erişmesine izin vermek nasıl hakkında daha fazla bilgi için bkz. [Azure portalını kullanarak sanal makinelerinize dış erişime izin vermek](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Azure Load Balancer benzer yük Dengeleyiciler bir şirket içi ortamda çalıştığını unutmayın. Daha fazla bilgi için [Yük Dengeleme için Azure altyapı hizmetleri](../tutorial-load-balancer.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Ayrıca, Azure sanal ağ'ı kullanarak, sanal makineleriniz için özel bir ağ ayarlamanızı öneririz. Bu özel IP adresi aralarında iletişim kurmak sağlar. Daha fazla bilgi için [Azure sanal ağı](../../../virtual-network/virtual-networks-overview.md).

## <a name="2-tier-and-3-tier-with-business-tier-scale-out"></a>Katman 2 ve 3 katmanlı iş katmanı ölçek genişletme ile
Bu uygulama modelinde, her uygulama katmanı farklı bir sanal makine yerleştirerek veritabanı, Katman 2 veya 3 katmanlı uygulama için Azure sanal makineler dağıtın. Ayrıca, birden fazla sanal makineye uygulamanın karmaşıklığı nedeniyle uygulama sunucusu bileşenlerini dağıtmak isteyebilirsiniz.

Bu uygulama Düzen şu durumlarda yararlı olur:

* Kurumsal uygulamalar, şirket içinde sanallaştırılmış platformlarından Azure sanal makineler için taşımak istediğiniz.
* Uygulama sunucusu bileşenleri uygulamanın karmaşıklığı nedeniyle birden fazla sanal makineye dağıtmak istediğiniz.
* İş mantığı ağır şirket LOB (satır iş kolu) uygulamalar için Azure sanal makineleri taşımak istediğiniz. LOB uygulamaları, muhasebe, İnsan Kaynakları (ik), bordro, tedarik zinciri yönetimi ve kaynak planlama uygulamaları gibi kuruluş çalışması için hayati öneme kritik bilgisayar uygulamaları kümesidir.
* Geliştirme sağlayın ve kısa süreler için test ortamlarını hızlıca istiyorsunuz.
* Değişen iş yükü düzeyleri için stres gerçekleştirmek istediğiniz, ancak aynı anda sahip çok sayıda fiziksel makineyi her zaman korumak istediğiniz değil.
* İsteğe bağlı olarak yukarı ve aşağı ölçeklendirilebilir bir altyapı ortamı kendi istiyorsunuz.

Aşağıdaki diyagramda, şirket içi senaryo ve etkin bulut çözümünün gösterir. Bu senaryoda, iş mantığı katmanı ve veri erişim bileşenlerini içeren iş katmanın ölçeğini genişletmenizi tarafından birden çok sanal makine azure'da uygulama katmanda yerleştirin. Diyagramda görüldüğü gibi Azure Load Balancer için birden çok sanal makine arasında trafiği dağıtma ve ayrıca bağlanmak için hangi web sunucu belirleme sorumludur. Uygulama sunucuları bir yük dengeleyicinin arkasına birden çok örnek içeren iş katmanı, yüksek kullanılabilirlik sağlar. Daha fazla bilgi için [bir katmanda birden çok sanal makineye sahip bir katman 2, 3 katmanlı veya n katmanlı uygulama desenleri için en iyi yöntemler](#best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier).

![İş katmanı ölçek genişletmeli uygulama düzeni](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728011.png)

## <a name="2-tier-and-3-tier-with-presentation-and-business-tiers-scale-out-and-hadr"></a>Katman 2 ve 3 katmanlı sunu, iş katmanı ölçek genişletme ve HADR
Bu uygulama modelinde, sunu katmanı (web sunucusu) ve iş Katmanı (uygulama sunucusu) bileşenleri birden fazla sanal makineye dağıtarak veritabanı, Katman 2 veya 3 katmanlı uygulama için Azure sanal makineler dağıtın. Ayrıca, Azure sanal makineler'de veritabanlarınız için yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümleri uygulayın.

Bu uygulama Düzen şu durumlarda yararlı olur:

* Kurumsal uygulamalar SQL Server yüksek kullanılabilirlik ve olağanüstü durum kurtarma olanaklarını uygulayarak sanallaştırılmış platformlara şirket içinden Azure'a taşımak istiyorsanız.
* Sunu katmanı ve gelen istemci isteklerini artan hacmi ve uygulamanın karmaşıklığı nedeniyle iş katmanı ölçeğini istiyorsunuz.
* Geliştirme sağlayın ve kısa süreler için test ortamlarını hızlıca istiyorsunuz.
* Değişen iş yükü düzeyleri için stres gerçekleştirmek istediğiniz, ancak aynı anda sahip çok sayıda fiziksel makineyi her zaman korumak istediğiniz değil.
* İsteğe bağlı olarak yukarı ve aşağı ölçeklendirilebilir bir altyapı ortamı kendi istiyorsunuz.

Aşağıdaki diyagramda, şirket içi senaryo ve etkin bulut çözümünün gösterir. Bu senaryoda, sunu katmanı ve iş katmanı bileşenleri azure'da birden çok sanal makinelerde ölçeği. Ayrıca, Azure'da SQL Server veritabanları için yüksek kullanılabilirlik ve olağanüstü durum kurtarma (HADR) tekniklerini uygulayın.

Bir uygulama birden çok kopyasını farklı çalıştıran VM'ler istekleri arasında yük dengelemeyi olduğundan emin olun. Birden çok sanal makine varsa, tüm Vm'leriniz erişilebilir ve tek bir noktada süre çalıştığını emin olmanız gerekir. Yük Dengeleme yapılandırırsanız, Azure Load Balancer VM'lerin durumunu izler ve iyi durumda çalıştığını VM düğümleri Gelen Çağrılara düzgün bir şekilde yönlendirir. Sanal makinelerin yük dengelemeyi ayarlamadan hakkında daha fazla bilgi için bkz: [Yük Dengeleme için Azure altyapı hizmetlerinde](../tutorial-load-balancer.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Web ve uygulama sunucularını bir yük dengeleyicinin arkasına birden çok örneğini sahip, sunu ve iş katmanları yüksek kullanılabilirliğini sağlar.

![Ölçek genişletme ve yüksek kullanılabilirlik](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728012.png)

### <a name="best-practices-for-application-patterns-requiring-sql-hadr"></a>SQL HADR gerektiren uygulama desenleri için en iyi uygulamalar
Bir sanal ağ kullanan sanal makineleriniz için SQL Server yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümleri Azure sanal Makineler'de ayarladığınızda, ayarlama [Azure sanal ağı](../../../virtual-network/virtual-networks-overview.md) zorunludur.  Bir sanal ağdaki sanal makineler bile bir kapalı kalma sonra kararlı bir özel IP adresi gerekir, bu nedenle, DNS ad çözümlemesi için gereken güncelleştirme zamanı önleyebilirsiniz. Ayrıca, sanal ağ, şirket içi ağınızı Azure'a genişletmek sağlar ve güvenilen güvenlik sınırı oluşturur. Örneğin, uygulamanızın şirket etki alanı kısıtlamaları (örneğin, Windows kimlik doğrulaması, Active Directory), varsa ayarlama [Azure sanal ağı](../../../virtual-network/virtual-networks-overview.md) gereklidir.

Çoğu üretim kodu, Azure üzerinde çalıştırıyorsanız, müşteriler, Azure'da hem birincil hem de ikincil çoğaltmaları önlüyor.

Kapsamlı bilgi ve yüksek kullanılabilirlik ve olağanüstü durum kurtarma teknikleri öğreticiler için bkz. [yüksek kullanılabilirlik ve olağanüstü durum kurtarma için Azure sanal Makineler'de SQL Server](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="2-tier-and-3-tier-using-azure-vms-and-cloud-services"></a>Katman 2 ve 3 katmanlı Azure Vm'leri ve Cloud Services'ı kullanma
Bu uygulama modelinde, Katman 2 veya 3 katmanlı uygulamayı azure'a her ikisini de kullanarak dağıttığınız [Azure Cloud Services](../../../cloud-services/cloud-services-choose-me.md) (web ve çalışan rolleri - (PaaS) hizmet olarak Platform) ve [Azure sanal makineler](../overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) () Hizmet olarak altyapı (Iaas)). Kullanarak [Azure Cloud Services](https://azure.microsoft.com/documentation/services/cloud-services/) sunu katmanı/iş katmanı ve SQL Server'da [Azure sanal makineler](../overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) için veri katmanı uygulamalarının Azure üzerinde çalışan çoğu için yararlıdır. Cloud Services üzerinde çalışan bir işlem örneğine sahip bir kolay yönetim, dağıtım, izleme ve ölçeklendirme sağladığı nedenidir.

Bulut hizmetleriyle Azure altyapı sizin yerinize tutar, düzenli bakım gerçekleştirir, işletim sistemi düzeltme ekleri ve hizmet ve donanım hatalardan kurtarmak çalışır. Uygulamanızın ihtiyaç duyduğu ölçek genişletme, otomatik ve el ile ölçek genişletme seçenekleri artan veya azalan örnekleri veya uygulamanız tarafından kullanılan sanal makineleri tarafından bulut hizmeti projenizi için kullanılabilir. Ayrıca, uygulamanızı bir Azure bulut hizmeti projesindeki dağıtmak için şirket içi Visual Studio kullanabilirsiniz.

Özet olarak, kapsamlı sahip olmasını istemiyorsanız, yönetim görevleri için sunu/iş katmanı ve uygulamanızı değil karmaşık yapılandırmaların yazılım veya işletim sistemi gerektirir, Azure Cloud Services'ı kullanın. Azure SQL veritabanı, aradığınız tüm özellikleri desteklemiyorsa, SQL Server veri katmanı için bir Azure sanal makinesinde kullanın. Azure Cloud Services üzerinde bir uygulama çalıştıran ve Azure sanal Makineler'de veri depolama her iki hizmet avantajlarını bir araya getirir. Ayrıntılı bir karşılaştırması için bu konudaki bakın [azure'da geliştirme stratejileri karşılaştırma](#comparing-web-development-strategies-in-azure).

Bu uygulama modelinde bulut hizmetleri bileşen Azure yürütme ortamında çalıştığından ve IIS ve ASP.NET tarafından desteklenen web uygulaması programlama için özelleştirilmiş bir web rolü, sunu katmanı içerir. İş veya arka uç katmanı, Azure yürütme ortamında bir Cloud Services bileşeni çalıştıran genelleştirilmiş geliştirme için kullanışlıdır ve bir web rolü için arka plan işlemlerini gerçekleştirebilir ve çalışan rolü içerir. Azure'da bir SQL Server sanal makinesi veritabanı katmanı bulunur. Sunu katmanı ve veritabanı katmanı arasındaki iletişim, doğrudan ya da iş katmanı – çalışan rolü bileşenleri üzerinden gerçekleşir.

Bu uygulama Düzen şu durumlarda yararlı olur:

* Kurumsal uygulamalar SQL Server yüksek kullanılabilirlik ve olağanüstü durum kurtarma olanaklarını uygulayarak sanallaştırılmış platformlara şirket içinden Azure'a taşımak istiyorsanız.
* İsteğe bağlı olarak yukarı ve aşağı ölçeklendirilebilir bir altyapı ortamı kendi istiyorsunuz.
* Azure SQL veritabanı, uygulamanızın veya veritabanı gereken tüm özellikleri desteklemez.
* Değişen iş yükü düzeyleri için stres gerçekleştirmek istediğiniz, ancak aynı anda sahip çok sayıda fiziksel makineyi her zaman korumak istediğiniz değil.

Aşağıdaki diyagramda, şirket içi senaryo ve etkin bulut çözümünün gösterir. Bu senaryoda, web rolleri, çalışan rolleri iş katmanından ancak Azure sanal makineler'de veri katmanında, sunu katmanı yerleştirin. Sunu katmanı, birden çok kopyasının farklı web rollerinde çalışan arasında Yük Dengeleme isteklerini için sağlar. Azure bulut Hizmetleri ile Azure sanal makineler birleştirdiğinizde ayarlamanız önerilir [Azure sanal ağı](../../../virtual-network/virtual-networks-overview.md) de. İle [Azure sanal ağı](../../../virtual-network/virtual-networks-overview.md), sizin olan kararlı ve kalıcı özel IP adresleri aynı bulut hizmeti bulutta. Bir sanal ağ, sanal makineleriniz için tanımlayın ve bulut Hizmetleri sonra aralarında özel IP adresi iletişim kuran başlayabilirsiniz. Aynı Ayrıca, sanal makineler ve Azure web/çalışan rollerini sahip [Azure sanal ağı](../../../virtual-network/virtual-networks-overview.md) düşük gecikme süresi ve daha güvenli bir bağlantı sağlar. Daha fazla bilgi için [bir bulut hizmeti nedir](../../../cloud-services/cloud-services-choose-me.md).

Diyagramda görüldüğü gibi Azure Load Balancer, birden çok sanal makine arasında trafiği dağıtır ve hangi web sunucusu veya uygulama sunucusuna bağlanmak için de belirler. Web ve uygulama sunucuları bir yük dengeleyicinin arkasına birden çok örnek içeren sunu katmanı ve iş katmanı, yüksek kullanılabilirlik sağlar. Daha fazla bilgi için [gerektiren SQL HADR uygulama desenleri için en iyi yöntemler](#best-practices-for-application-patterns-requiring-sql-hadr).

![Cloud Services ile uygulama düzenleri](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728013.png)

Bu uygulama deseni uygulamak için başka bir yaklaşım, sunu katmanı hem de aşağıdaki diyagramda gösterildiği gibi iş katmanı bileşenlerini içeren bir birleştirilmiş web rolü kullanmaktır. Bu uygulama düzeni, durum bilgisi olan tasarım gerektiren uygulamalar için yararlıdır. Azure, web ve çalışan rollerinde durum bilgisi olmayan bir işlem düğümleri sağlar. bu yana, aşağıdaki teknolojileri kullanarak oturum durumunu depolamak için bir mantıksal uygulamadan öneririz: [Azure önbelleği](https://azure.microsoft.com/documentation/services/azure-cache-for-redis/), [Azure tablo depolaması](../../../cosmos-db/table-storage-how-to-use-dotnet.md) veya [Azure SQL veritabanı](../../../sql-database/sql-database-technical-overview.md).

![Cloud Services ile uygulama düzenleri](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728014.png)

## <a name="pattern-with-azure-vms-azure-sql-database-and-azure-app-service-web-apps"></a>Desen ile Azure Vm'leri, Azure SQL veritabanı ve Azure App Service (Web uygulamaları)
Bu uygulama düzeni birincil amacı, Azure altyapısının bir hizmet (Iaas) bileşenleri ile Azure hizmet olarak platform (PaaS) bileşenlerde çözümünüz olarak bir araya getirilebileceğini öğrenin göstermektir. Bu düzen, ilişkisel veri depolama için Azure SQL veritabanı'nda odaklanmıştır. Bu SQL Server bir Azure sanal makine, hizmet teklifi olarak Azure altyapısının bir parçası olduğu içermez.

Bu uygulama modelinde, sunu ve iş katmanları aynı sanal makine yerleştirmeyi ve bir Azure SQL veritabanı (SQL veritabanı) sunucuları veritabanına erişim veritabanı uygulamayı azure'a dağıtın. Sunu katmanı geleneksel IIS tabanlı web çözümleri kullanarak uygulayabilirsiniz. Veya bir birleşik sunu ve iş katmanı kullanarak uygulayabilirsiniz [Azure App Service](https://azure.microsoft.com/documentation/services/app-service/web/).

Bu uygulama Düzen şu durumlarda yararlı olur:

* Azure'da yapılandırılmış mevcut bir SQL veritabanı sunucusu zaten yüklü ve uygulamanızı hızlı bir şekilde test etmek istediğiniz.
* Azure ortamı yeteneklerini test etmek istediğiniz.
* Geliştirme sağlayın ve kısa süreler için test ortamlarını hızlıca istiyorsunuz.
* İş mantığı ve verileri erişim bileşenleri web uygulamasının içinden kendi içinde olabilir.

Aşağıdaki diyagramda, şirket içi senaryo ve etkin bulut çözümünün gösterir. Bu senaryoda, Azure ve erişim verilerini Azure SQL veritabanı'nda tek bir sanal makine içinde uygulama katmanları yerleştirin.

![Karma uygulama düzeni](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728015.png)

Azure Web Apps kullanarak bir birleştirilmiş web ve uygulama katmanı uygulamak seçerseniz, Orta katmanda veya uygulama katmanı web uygulaması bağlamında dinamik bağlantı kitaplıklarını (DLL'ler) olarak tutmanızı öneririz.

Ayrıca, verilen önerileri gözden [azure'da web geliştirme stratejileri karşılaştırma](#comparing-web-development-strategies-in-azure) programlama hakkında daha fazla bilgi edinmek için bu makalenin sonunda bölüm.

## <a name="n-tier-hybrid-application-pattern"></a>N katmanlı karma uygulama düzeni
N katmanlı karma uygulama düzeni şirket içi ile Azure arasında dağıtılmış birden fazla katman uygulamanızda uygulayın. Bu nedenle, esnek ve yeniden kullanılabilir hibrit bir sistemin oluşturmak, değiştirmek veya diğer Katmanlar değiştirmeden belirli bir katman ekleyin. Kurumsal ağınızı buluta genişletmek için kullandığınız [Azure sanal ağı](../../../virtual-network/virtual-networks-overview.md) hizmeti.

Bu karma uygulama düzeni durumlarda yararlı olur:

* Kısmen bulutta çalıştırın ve kısmen şirket içi uygulamalar oluşturmak istiyorsunuz.
* Bazı veya tüm öğelerin bir mevcut şirket içi uygulamanızı buluta geçirmek istediğiniz.
* Kurumsal uygulamaları, şirket içinde sanallaştırılmış platformlarından Azure'a taşımak istediğiniz.
* İsteğe bağlı olarak yukarı ve aşağı ölçeklendirilebilir bir altyapı ortamı kendi istiyorsunuz.
* Geliştirme sağlayın ve kısa süreler için test ortamlarını hızlıca istiyorsunuz.
* İstediğiniz kuruluş veritabanı uygulamaları için yedeklemeler almak için uygun maliyetli bir yöntemdir.

Aşağıdaki diyagramda, şirket içi ve Azure yayılan bir n katmanlı karma uygulama desenini gösterir. Diyagramda gösterildiği gibi altyapı içeren şirket [Active Directory Domain Services](https://technet.microsoft.com/library/hh831484.aspx) kullanıcı kimlik doğrulaması ve yetkilendirme desteklemek için etki alanı denetleyicisi. Not diyagram burada veri katmanı bazı bölümlerini bir şirket içi veri merkezinde canlı veri katmanı bazı bölümleri Azure'da dinamik ise, bir senaryo gösterir. Uygulamanızın ihtiyaçlarına bağlı olarak diğer birkaç karma senaryolar uygulayabilirsiniz. Örneğin, bir şirket içi ortamda ancak Azure veri katmanında sunu katmanı ve iş katmanı tutabilirsiniz.

![N katmanlı uygulama düzeni](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728016.png)

Azure'da Active Directory tek başına bulut dizini olarak kuruluşunuz için kullanabilirsiniz veya var olan şirket içi Active Directory ile de tümleştirebilirsiniz [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Diyagramda görüldüğü gibi iş katmanı bileşenlerini birden çok veri kaynağına gibi için erişebileceği [azure'da SQL Server](virtual-machines-windows-sql-server-iaas-overview.md) özel bir iç IP adresi için SQL Server aracılığıyla şirket [Azure sanal ağı](../../../virtual-network/virtual-networks-overview.md), veya [SQL veritabanı](../../../sql-database/sql-database-technical-overview.md) .NET Framework veri sağlayıcısı teknolojilerini kullanarak. Bu diyagramda, Azure SQL veritabanı bir isteğe bağlı veri depolama hizmetidir.

N katmanlı karma uygulama düzeni aşağıdaki iş akışında belirtilen sırada uygulayabilirsiniz:

1. Kullanarak buluta taşınması gereken Kurumsal veritabanı uygulamaları belirlemek [Microsoft Assessment ve planlama (eşleme) Araç Seti](https://microsoft.com/map). MAP Araç Kiti, sanallaştırma için değerlendiriyorsanız bilgisayarlardan Envanter ve performans verilerini toplar ve kapasitesini ve değerlendirmesini planlama önerileri sağlar.
2. Kaynaklar ve mimariler gerekli depolama hesapları ve sanal makineler gibi Azure platformunda planlayın.
3. Şirket ağı şirket içi arasında ağ bağlantısı ayarlayın ve [Azure sanal ağı](../../../virtual-network/virtual-networks-overview.md). Kurumsal ağa şirket içinde ve Azure'da bir sanal makine arasındaki bağlantıyı ayarlamak için aşağıdaki iki yöntemden birini kullanın:
   
   1. Azure'da bir sanal makinede genel uç noktaları aracılığıyla şirket içi ve Azure arasında bağlantı kurun. Bu yöntem bir kolayca Kurulum sağlar ve sanal makinenizde SQL Server kimlik doğrulama kullanmanıza olanak tanır. Ayrıca, VM genel trafiği denetlemek için ağ güvenlik grubu kurallarınızı ayarlayın. Daha fazla bilgi için [Azure portalını kullanarak sanal makinelerinize dış erişime izin vermek](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
   2. Azure sanal özel ağ (VPN) tüneli yoluyla şirket içi ile Azure arasında bağlantı kurun. Bu yöntem etki alanı ilkeleri için Azure sanal makineler'de genişletmenizi sağlar. Ayrıca, güvenlik duvarı kurallarını ayarlayın ve sanal makinenizi Windows kimlik doğrulaması kullanın. Şu anda Azure, güvenli siteden siteye VPN ve noktadan siteye VPN bağlantıları destekler:
      
      * Güvenli siteden siteye bağlantı ile ağ bağlantısı, sanal ağınızın şirket içi ağınız arasında Azure'da kurabilirsiniz. Şirket içi veri merkezi ortamınızı Azure'a bağlanmak için önerilir.
      * Güvenli noktadan siteye bağlantı ile sanal ağınızda Azure ile her yerde çalışan, tek tek bilgisayarlar arasında ağ bağlantısı kurabilirsiniz. Ayrıca, geliştirme ve test amaçları için çoğunlukla önerilir.
      
      Azure SQL Server'a bağlanma hakkında daha fazla bilgi için bkz: [azure'da SQL Server sanal makinesine bağlanma](virtual-machines-windows-sql-connect.md).
4. Zamanlanmış işler ve şirket içi verileri azure'da bir sanal makine disk yedekleme Uyarıları ayarlayın. Daha fazla bilgi için [SQL Server Yedekleme ve geri yükleme ile Azure Blob Depolama hizmetinin](https://msdn.microsoft.com/library/jj919148.aspx) ve [yedekleme ve Azure sanal Makineler'de SQL Server için geri yükleme](virtual-machines-windows-sql-backup-recovery.md).
5. Uygulamanızın ihtiyaçlarına bağlı olarak, aşağıdaki üç sık karşılaşılan senaryolardan biri uygulayabilirsiniz:
   
   1. Hassas verileri şirket içi tutmak ise, azure'da bir veritabanı sunucusu, web sunucusu, uygulama sunucusu ve duyarlı veri tutabilirsiniz.
   2. Web sunucunuzun tutabilir ve uygulama sunucusu şirket içi ise bir sanal makinede Azure veritabanı sunucusu.
   3. Tutabilir, veritabanı sunucusu, web sunucusu ve uygulama sunucusu veritabanı çoğaltmalarını Azure sanal makineler'de saklamak ise şirket. Bu ayar, azure'da veritabanı çoğaltmalarını erişmek için raporlama uygulamaları ve şirket içi web sunucuları sağlar. Bu nedenle, bir şirket içi veritabanı iş yükünü azaltmak için elde edebilirsiniz. Bu senaryo için ağır uygulamak öneririz iş yükleri ve gelişim amacıyla okuyun. Veritabanı çoğaltmalarını Azure'da oluşturma hakkında daha fazla bilgi için bkz: AlwaysOn Kullanılabilirlik Grupları'na [yüksek kullanılabilirlik ve olağanüstü durum kurtarma için Azure sanal Makineler'de SQL Server](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="comparing-web-development-strategies-in-azure"></a>Azure'da Web geliştirme stratejileri karşılaştırma
Uygulama ve çok katmanlı bir SQL Server tabanlı bir uygulama azure'da dağıtmak için aşağıdaki iki programlama yöntemlerden birini kullanabilirsiniz:

* Azure ve erişim Azure sanal Makineler'de SQL Server veritabanlarında geleneksel web sunucusu (IIS - Internet Information Services) ayarlayın.
* Hazırlayın ve Azure'a bir bulut hizmeti dağıtın. Daha sonra bu bulut hizmetini Azure sanal makineler'de SQL Server veritabanlarını erişebildiğinden emin olun. Bir bulut hizmeti, birden çok web ve çalışan rolleri dahil edebilirsiniz.

Aşağıdaki tabloda, Azure Cloud Services ve Azure sanal Makineler'de SQL Server ile ilgili Azure Web Apps ile geleneksel web geliştirme karşılaştırması verilmiştir. Tablo, SQL Server Azure sanal Makinesinde veri kaynağı olarak Azure Web Apps için genel sanal IP adresi veya DNS adı aracılığıyla kullanmak mümkün olsa da, Azure Web Apps içerir.

|  | Azure sanal Makineler'de geleneksel web geliştirme | Azure'da cloud Services | Web sitesi ile Azure Web Apps barındırma |
| --- | --- | --- | --- |
| **Şirket içi uygulama geçişi** |Mevcut uygulamaları olarak-olduğu. |Uygulamaları, web ve çalışan rolleri gerekir. |Mevcut uygulamaları olarak-kendi içinde web uygulamaları ve hızlı ölçeklenebilirlik gerektiren web hizmetleri için ancak uygundur. |
| **Geliştirme ve dağıtma** |Visual Studio, WebMatrix, Visual Web Developer, WebDeploy, FTP, TFS, IIS Yöneticisi'ni PowerShell. |Visual Studio, Azure SDK, TFS, PowerShell. Her bir bulut hizmeti dağıttığınız hizmet paketini ve yapılandırma için iki ortam vardır: hazırlama ve üretim. Üretim hazırlığına önce test etmek için hazırlık ortamı için bir bulut hizmetinde dağıtabilirsiniz. |Visual Studio, WebMatrix, Visual Web Developer, FTP, GIT, BitBucket, CodePlex, DropBox, GitHub, Mercurial, TFS ve Web PowerShell dağıtın. |
| **Yönetim ve Kurulum** |Uygulama, veri, güvenlik duvarı kuralları, sanal ağ ve işletim sistemi yönetim görevleri için sorumlu olursunuz. |Uygulama, veri, güvenlik duvarı kuralları ve sanal ağ üzerinde yönetim görevleri için sorumlu olursunuz. |Sadece veri ve uygulama yönetim görevlerini sizin sorumluluğunuzdadır. |
| **Yüksek kullanılabilirlik ve olağanüstü durum kurtarma (HADR)** |Sanal makineler aynı kullanılabilirlik kümesinde ve aynı bulut hizmetindeki yerleştirmenizi öneririz. Kullanılabilirlik kümesi, Vm'lerinizin aynı kalmasını Azure'un yüksek kullanılabilirlik düğümleri ayrı hata etki alanlarına yerleştirin ve yükseltme etki alanlarında sağlar. Benzer şekilde, sanal makineleriniz aynı bulut hizmetinde tutma Yük Dengeleme sağlar ve VM'lerin doğrudan bir Azure veri merkezinde yerel ağ üzerinden birbirleriyle.<br/><br/>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümü SQL Server için kapalı kalma süresi önlemek için Azure sanal Makineler'de uygulamak için sorumlu olursunuz. Desteklenen HADR teknolojileri için bkz: [yüksek kullanılabilirlik ve olağanüstü durum kurtarma için Azure sanal Makineler'de SQL Server](virtual-machines-windows-sql-high-availability-dr.md).<br/><br/>Kendi veri ve uygulama yedekleme için sorumlu olursunuz.<br/><br/>Konak makine veri merkezindeki donanım sorunları nedeniyle başarısız olursa, azure sanal makinelerinizi taşıyabilirsiniz. Ayrıca, olabilir planlanan kapalı kalma süresi sanal makinenizin güvenlik veya yazılım için ana makinenin güncelleştirildiğinde güncelleştirmeleri. Bu nedenle, sürekli kullanılabilirlik sağlamak için her uygulama katmanında en az iki VM bakımını öneririz. Azure, tek bir sanal makine için SLA sağlanmaz. |Azure, temel alınan donanım veya işletim sistemi yazılımlardan kaynaklanan hataları yönetir. Uygulamanızın yüksek kullanılabilirliğini sağlamak için bir web veya çalışan rolünün birden fazla uygulama öneririz. Bilgi için [bulut Hizmetleri, sanal makineler ve sanal ağ hizmet düzeyi sözleşmesi](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/).<br/><br/>Kendi veri ve uygulama yedekleme için sorumlu olursunuz.<br/><br/>Bir SQL Server veritabanını Azure VM'deki bulunan veritabanları için kapalı kalma süresi önlemek için bir yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümü uygulamak için sorumlu olursunuz. Desteklenen HDAR teknolojiler, Azure sanal Makineler'de SQL Server için yüksek kullanılabilirlik ve olağanüstü durum kurtarma bakın.<br/><br/>**SQL Server veritabanı yansıtma**: Azure Cloud Services (web/çalışan rolleri) kullanın. SQL Server Vm'leri ve bir bulut hizmeti projesini aynı Azure sanal ağında olabilir. SQL Server VM aynı sanal ağ içinde değilse, bir SQL Server örneğine iletişim yönlendirmek için SQL Server diğer adı oluşturmak gerekir. Ayrıca, diğer ad, SQL Server adı eşleşmelidir. |Yüksek kullanılabilirlik, Azure çalışan rolleri, Azure blob depolama ve Azure SQL veritabanı ' devralınıyor. Örneğin, Azure depolama, tüm blob, tablo ve kuyruk verilerin üç kopyasını tutar. Herhangi bir anda, Azure SQL veritabanı çalıştıran verilerin üç kopyasını tutar — bir birincil çoğaltma ve iki ikincil çoğaltma. Daha fazla bilgi için [Azure depolama](https://azure.microsoft.com/documentation/services/storage/) ve [Azure SQL veritabanı](../../../sql-database/sql-database-technical-overview.md).<br/><br/>SQL Server, Azure sanal Makinesinde veri kaynağı olarak Azure Web Apps için kullanıldığında, Azure Web Apps, Azure sanal ağ desteklemediğini aklınızda bulundurun. Diğer bir deyişle, tüm Azure Web Apps bağlantılarından azure'da SQL Server Vm'leri için sanal makinelerin ortak Uç noktalara gitmeniz gerekir. Bu, yüksek kullanılabilirlik ve olağanüstü durum kurtarma senaryoları için bazı sınırlamalara neden olabilir. Örneğin, Azure Web apps'te veritabanı yansıtma ile SQL Server VM'ye bağlanma istemci uygulaması veritabanı yansıtma Azure sanal ağ arasındaki Azure Vm'leri barındıracak SQL Server ayarlama gerektirdiği yeni birincil sunucuya bağlanmanız mümkün olmaz. Bu nedenle, kullanarak **SQL Server veritabanı yansıtma** Azure Web Apps ile şu anda desteklenmiyor.<br/><br/>**SQL Server AlwaysOn Kullanılabilirlik grupları**: AlwaysOn Kullanılabilirlik gruplarını ayarlama, Azure Web Apps ile Azure SQL Server Vm'leri kullanırken ayarlayabilirsiniz. Ancak birincil Çoğaltmaya genel yük dengeli uç noktaları aracılığıyla iletişime yönlendirmek için AlwaysOn Kullanılabilirlik grubu dinleyicisini yapılandırmak gerekmez. |
| **Şirket içi bağlantılar** |Azure sanal ağ, şirket içine bağlanmak için kullanabilirsiniz. |Azure sanal ağ, şirket içine bağlanmak için kullanabilirsiniz. |Azure sanal ağı desteklenir. Daha fazla bilgi için [Web uygulamaları sanal ağ tümleştirmesi](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/). |
| **Ölçeklenebilirlik** |Daha fazla disk ekleyerek veya sanal makine boyutları artan ölçek büyütme kullanılabilir. Sanal makine boyutları hakkında daha fazla bilgi için bkz. [Azure için sanal makine boyutlarını](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).<br/><br/>**Veritabanı sunucusu için**: Ölçeği Genişletilmiş veritabanı bölümleme teknikleri ve SQL Server AlwaysOn Kullanılabilirlik grupları kullanılabilir.<br/><br/>Yoğun okuma iş yükleri için kullanabileceğiniz [AlwaysOn Kullanılabilirlik grupları](https://msdn.microsoft.com/library/hh510230.aspx) SQL Server çoğaltma yanı sıra birden çok ikincil düğüm.<br/><br/>Ağır yazma iş yükleri için uygulama ölçeklendirme sağlamak için birden çok fiziksel sunucuya yatay bölümleme veri uygulayabilirsiniz.<br/><br/>Ayrıca, bir genişleme kullanarak uygulayabileceğiniz [verilere bağımlı yönlendirme ile SQL Server](https://technet.microsoft.com/library/cc966448.aspx). Veri bağımlı yönlendirme (DDR ile), birden çok SQL Server düğümü için veritabanı isteği yönlendirmek için istemci uygulamasında, genellikle iş katmanı katmanı içinde bölümleme mekanizması uygulamanız gerekir. İş katmanı, verilerin nasıl bölümlendiğini ve hangi düğümün verileri içeren eşlemeleri içerir.<br/><br/>Sanal makineler çalıştıran uygulamaları ölçekleyebilirsiniz. Daha fazla bilgi için [bir uygulama nasıl ölçeklendirilir](../../../cloud-services/cloud-services-how-to-scale-portal.md).<br/><br/>**Önemli Not**: **Otomatik ölçeklendirme** azure'da özelliği otomatik olarak artırın veya azaltın, uygulamanız tarafından kullanılan sanal makineleri olanak tanır. Bu özellik, son kullanıcı deneyimini olumsuz yoğun dönemlerde etkilenmez ve talep azaldığında VM'ler kapatılır garanti eder. SQL Server Vm'leri içeriyorsa, otomatik ölçeklendirme seçeneği'ı bulut hizmetinizin ayarlamayın önerilir. Bunun nedeni otomatik ölçeklendirme Özelliği Azure CPU kullanımı, VM ile bazı eşik değerinden yüksek olduğunda bir sanal makineyi Aç ve CPU kullanımı, düşük olduğunda bir sanal makineyi kapatmayı sağlamasıdır. Otomatik ölçeklendirme özelliği, herhangi bir VM iş yükü olmadan önceki bir duruma yapılan tüm başvuruları yönetebileceğiniz web sunucuları gibi durum bilgisiz uygulamalar için yararlıdır. Ancak, otomatik ölçeklendirme özelliği, burada yalnızca bir örneği veritabanına yazma sağlar, SQL Server gibi durum bilgisi olan uygulamalar için yararlı değildir. |Ölçek büyütme, birden çok web ve çalışan rolleri kullanılarak kullanılabilir. Web rolleri ve çalışan rolleri için sanal makine boyutları hakkında daha fazla bilgi için bkz. [Cloud Services boyutları yapılandırma](../../../cloud-services/cloud-services-sizes-specs.md).<br/><br/>Kullanırken **Cloud Services**, işleme dağıtabilir ve ayrıca uygulamanızı esnek ölçeklendirme ulaşmak için birden çok rol tanımlayabilirsiniz. Her bir bulut hizmeti, bir veya daha fazla web rollerinin ve/veya çalışan rolleri, her biri kendi uygulama dosyaları ve yapılandırması içerir. Bir bulut Hizmeti rol örnekleri (sanal makineler) sayısını artırarak bir rol için dağıtılan büyütme ve ölçek bir bulut Hizmeti rol örnekleri sayısını azaltarak aşağı. Ayrıntılı bilgi için bkz. [Azure yürütme modelleri](../../../cloud-services/cloud-services-choose-me.md).<br/><br/>Ölçeği genişletme aracılığıyla Azure yerleşik yüksek kullanılabilirlik desteği aracılığıyla kullanılabilir [bulut Hizmetleri, sanal makineler ve sanal ağ hizmet düzeyi sözleşmesi](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/) ve yük dengeleyici.<br/><br/>Çok katmanlı bir uygulama için veritabanı sunucusu Vm'leri Azure sanal ağ aracılığıyla web/çalışan rolleri uygulamaya bağlanmak öneririz. Ayrıca, Azure VM'ler için aynı bulut hizmetinde yük dengelemeyi kullanıcı istekleri arasında yayılmasını sağlar. Bu şekilde bağlı sanal makineleri, bir Azure veri merkezinde yerel ağ üzerinden doğrudan birbiriyle iletişim kurabilir.<br/><br/>Ayarlayabileceğiniz **otomatik ölçeklendirme** zamanlar yanı sıra Azure portalı. Daha fazla bilgi için [otomatik ölçeklendirme portalda bir bulut hizmeti için yapılandırma](../../../cloud-services/cloud-services-how-to-scale-portal.md). |**Ölçeği artırma ve azaltma**: Artış/web siteniz için ayrılmış örnek (VM) boyutu azaltabilir.<br/><br/>Ölçeği genişletme: Web siteniz için daha ayrılmış örnekleri (VM'ler) ekleyebilirsiniz.<br/><br/>Ayarlayabileceğiniz **otomatik ölçeklendirme** portalı ve bunun yanı sıra zamanlar. Daha fazla bilgi için [ölçek Web Apps'e nasıl](../../../app-service/web-sites-scale.md). |

Programlama bu yöntemler arasında seçme hakkında daha fazla bilgi için bkz. [Azure Web Apps, Cloud Services ve Vm'leri: Hangisi ne zaman kullanılmalıdır](/azure/architecture/guide/technology-choices/compute-decision-tree).

## <a name="next-steps"></a>Sonraki Adımlar
Azure sanal makineler'de SQL Server çalıştırma hakkında daha fazla bilgi için bkz. [SQL Server Azure sanal makinelerine genel bakış](virtual-machines-windows-sql-server-iaas-overview.md).

