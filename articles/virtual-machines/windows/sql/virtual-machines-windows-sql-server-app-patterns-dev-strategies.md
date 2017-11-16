---
title: "SQL Server uygulama düzenleri vm'lerde | Microsoft Docs"
description: "Bu makalede Azure Vm'lerde SQL Server için uygulama düzenleri yer almaktadır. Bu çözüm mimarları ve geliştiricileri bir temel iyi uygulama mimarisi ve tasarım sağlar."
services: virtual-machines-windows
documentationcenter: na
author: ninarn
manager: jhubbard
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 41863c8d-f3a3-4584-ad86-b95094365e05
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/31/2017
ms.author: ninarn
ms.openlocfilehash: ed2ff59ad33408bef70f332f8a93eb8679bf328f
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="application-patterns-and-development-strategies-for-sql-server-in-azure-virtual-machines"></a>Azure Sanal Makineler'de SQL Server için Uygulama Desenleri ve Geliştirme Stratejileri
[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="summary"></a>Özeti:
Hangi uygulama düzeni veya kullanılacak desenlerini belirlemek için SQL Server tabanlı uygulamalarınızı Azure ortamı önemli tasarım kararı ve SQL Server ve Azure her altyapı bileşeni birlikte nasıl çalışır, tam olarak anlaşılması gerekir. Azure altyapı Hizmetleri'nde SQL Server ile kolayca geçirme, korumanıza ve yerleşik Windows Server Azure sanal makinelerine açma var olan SQL Server uygulamalarınızı izleyin.

Bu makalede amacı çözüm mimarları ve geliştiricileri bir temel iyi uygulama mimarisi ve mevcut uygulamalar Azure yanı sıra Azure içinde yeni uygulamalar geliştirmek için geçirilirken takip edebilirsiniz tasarım sağlamaktır.

Her uygulama düzeni için bir şirket içi senaryo, bulut etkin ilgili çözümünün ve ilgili teknik öneri bulabilirsiniz. Ayrıca, böylece uygulamalarınızı doğru şekilde tasarlayabilirsiniz makaleyi Azure özgü geliştirme stratejilerini açıklar. Birçok olası uygulama düzenleri nedeniyle mimarları ve geliştiricileri kendi uygulamalar ve kullanıcılar için en uygun deseni seçmelisiniz önerilir.

**Teknik katkıda:** Haluk Carlos Vargas Herring Madhan Arumugam Ramakrishnan

**Teknik İnceleme:** Corey Sanders, Drew McDaniel, Narayan Annamalai, Nir Mashkowski, Sanjay Mishra, Silvano Coriani, Stefan Schackow Tim Hickey, Tim Wieman, Xin Jin

## <a name="introduction"></a>Giriş
Farklı uygulama katmanları ayrı bileşenler olduğu gibi de farklı makinelerde bileşenlerinin ayırarak türlerde n katmanlı uygulamalar geliştirebilir. Örneğin, istemci uygulaması yerleştirebilirsiniz ve bir makine, ön uç web katmanı ve veri erişim katmanı bileşenlerinin başka bir makinede ve başka bir makine arka uç veritabanı katmanında bileşenlerinde iş kuralları. Bu tür yapılandırma, her katman birbirinden yalıtmak yardımcı olur. Veri nereden geldiğini değiştirirseniz, istemci veya web uygulamasını ancak yalnızca veri erişim katmanı bileşenleri değiştirmeniz gerekmez.

Tipik bir *n katmanlı* sunu katmanı, iş katmanı ve veri katmanı uygulama içerir:

| Katman | Açıklama |
| --- | --- |
| **Sunu** |*Sunu katmanı* (web katmanı, ön uç katmanı) olan kullanıcıların bir uygulama ile etkileşim katmanı. |
| **İş** |*İş katmanı* (orta katman) sunu katmanı katmanı ve veri katmanı birbirleri ile iletişim kurmak için kullanır ve sistem çekirdek işlevselliğini içerir. |
| **Veriler** |*Veri katmanı* temelde (örneğin, SQL Server çalıştıran bir sunucu) uygulama verilerini depolayan sunucusudur. |

Uygulama katmanları mantıksal gruplandırmalarını uygulamada bileşenleri ve işlevleri açıklanır; Katmanları ayrı fiziksel sunucular, bilgisayarlar, ağlar veya uzak konumlardaki bileşenleri ve işlevleri fiziksel dağıtımını açıklayan ise. Bir uygulama katmanları aynı fiziksel bilgisayarda (aynı katman) bulunabilir ya da ayrı bilgisayarlar (n katmanlı) dağıtılabilir ve diğer katmanları iyi tanımlanmış arabirimleri aracılığıyla bileşenlerinde her katman bileşenlerinde iletişim. İki katmanlı ve üç katmanlı n katmanlı gibi fiziksel dağıtım modelleri başvurma olarak terim katmanının düşünebilirsiniz. A **2 katmanlı uygulama düzeni** iki uygulama katmanı içerir: uygulama sunucusu ve veritabanı sunucusu. Uygulama sunucusu ve veritabanı sunucusu arasında doğrudan iletişim gerçekleşir. Uygulama sunucusu, web katmanı ve iş katmanı bileşenleri içerir. İçinde **3 katmanlı uygulama deseni**, üç uygulama katmanı vardır: web sunucusu, iş mantığı katmanından ve/veya iş katmanı veri erişimi bileşenleri içerir, uygulama sunucusu ve veritabanı sunucusu. Web sunucusu ve veritabanı sunucusu arasındaki iletişimi uygulama sunucusu gerçekleşir. Uygulama katmanları ve katmanları hakkında ayrıntılı bilgi için bkz: [Microsoft Uygulama Mimarisi Kılavuzu](https://msdn.microsoft.com/library/ff650706.aspx).

Bu makaleyi okuduktan başlamadan önce SQL Server ve Azure temel kavramlar hakkında bilgi olmalıdır. Bilgi için bkz: [SQL Server Books Online](https://msdn.microsoft.com/library/bb545450.aspx), [Azure Virtual Machines'de SQL Server](virtual-machines-windows-sql-server-iaas-overview.md) ve [Azure.com](https://azure.microsoft.com/).

Bu makalede oldukça karmaşık kurumsal uygulamalar yanı sıra, basit uygulamalar için uygun olabilir birkaç uygulama desenleri açıklar. Her desen ayrıntılı önce Azure, kullanılabilir veri depolama Hizmetleri'nde gibi öğrenmeniz, öneririz [Azure Storage](../../../storage/common/storage-introduction.md), [Azure SQL veritabanı](../../../sql-database/sql-database-technical-overview.md), ve [SQL Sunucu bir Azure sanal makinesinde](virtual-machines-windows-sql-server-iaas-overview.md). Uygulamalarınız için en iyi tasarım kararları için ne zaman hangi veri depolama hizmeti açıkça kullanılacağını anlayın.

### <a name="choose-sql-server-in-an-azure-virtual-machine-when"></a>Bir Azure sanal makine içinde SQL Server'ı seçin:
* SQL Server ve Windows üzerinde denetim gerekir. Örneğin, bu içerebilir SQL Server sürümü, özel düzeltmeleri, performans yapılandırması, vs.
* Şirket içi SQL Server ile tam uyumluluk gerekir ve mevcut uygulamalar Azure taşımak istiyorsanız-değil.
* Azure ortamı özelliklerden yararlanacak şekilde istiyor ancak Azure SQL veritabanı uygulamanızın gerektirdiği tüm özellikleri desteklemiyor. Bu, aşağıdaki alanları içerebilir:
  
  * **Veritabanı boyutu**: Bu makalede güncelleştirilen aynı anda en fazla 1 TB'lık veriyi bir veritabanı SQL veritabanı destekler. Uygulamanız birden fazla 1 TB'lık veriyi gerektirir ve özel parçalama çözümlerin uygulanmasını istemiyorsanız, bir Azure sanal makinede SQL Server kullanmanız önerilir. En son bilgiler için bkz: [Azure SQL veritabanlarını kullanıma ölçeklendirme](https://msdn.microsoft.com/library/azure/dn495641.aspx) ve [Azure SQL Database hizmet katmanları ve performans düzeyleri](../../../sql-database/sql-database-service-tiers.md).
  * **HIPAA Uyumluluk**: sağlık müşteriler ve bağımsız yazılım satıcıları (ISV) seçin [Azure Virtual Machines'de SQL Server](virtual-machines-windows-sql-server-iaas-overview.md) yerine [Azure SQL veritabanı](../../../sql-database/sql-database-technical-overview.md) olduğundan SQL Sunucu bir Azure sanal makine HIPAA iş ilişkilendirmek anlaşması (BAA) tarafından ele alınmıştır. Uyumluluk hakkında daha fazla bilgi için bkz: [Microsoft Azure Trust Center: Uyumluluk](https://azure.microsoft.com/support/trust-center/compliance/).
  * **Örnek düzeyinde Özellikler**: şu anda SQL veritabanı dışında veritabanı Canlı özellikleri desteklemez (bağlantılı sunucular gibi Aracısı işleri, FILESTREAM, hizmet Aracısı vb..). Daha fazla bilgi için bkz: [Azure SQL veritabanı yönergeleri ve sınırlamaları](https://msdn.microsoft.com/library/azure/ff394102.aspx).

## <a name="1-tier-simple-single-virtual-machine"></a>1 katmanlı (Basit): tek bir sanal makine
Bu uygulama modelinde, SQL Server uygulama ve veritabanı için azure'da bir tek başına sanal makine dağıtın. İstemci/web uygulamanız, iş bileşenleri, veri erişim katmanı ve veritabanı sunucusu aynı sanal makine içeriyor. Sunu, iş ve veri erişim kodu mantıksal olarak ayrılır ancak fiziksel bir tek sunucu makinede bulunan. Bu uygulama düzeni müşterilerin çoğu başlatın ve sonra bunlar daha fazla web rolleri ya da sanal makineleri sistemlerine ekleyerek ölçeğini.

Bu uygulama düzeni ne zaman yararlı olur:

* Platform veya uygulamanızın gereksinimleri yanıtlar olup olmadığını değerlendirmek için Azure platformu için basit bir geçiş yapmak istiyorsunuz.
* Katmanları arasındaki gecikme süresini azaltmak için aynı Azure veri merkezinde aynı sanal makinede barındırılan uygulama katmanları tutmak istiyor.
* Hızlı geliştirme sağlamak ve test ortamları kısa süreyle istiyor.
* Stres testi için değişen iş yükü düzeyleri gerçekleştirmek istediğiniz, ancak aynı anda sahip ve her zaman birçok fiziksel makineleri korumak istediğiniz değil.

Aşağıdaki diyagramda, basit bir senaryo ve tek bir sanal makine azure'da etkin bulut çözümünün nasıl dağıtabileceğiniz içi gösterir.

![1 katmanlı uygulama düzeni](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728008.png)

Ölçeklenebilirlik veya güvenlik sorunları nedeniyle ayrı bir katman kullanmadığınız sürece aynı fiziksel katman sunu katmanı olarak (iş mantığı ve verileri bileşenlerine erişmek) iş katmanı dağıtma uygulama performansını en üst düzeye çıkarabilirsiniz.

Bu çok yaygın bir desen ile başlamak olduğundan, aşağıdaki makalede geçiş verilerinizi SQL Server VM'nize taşımak için yararlı olabilir: [SQL Server'a Azure VM'de bir veritabanını geçirme](virtual-machines-windows-migrate-sql.md).

## <a name="3-tier-simple-multiple-virtual-machines"></a>3 katmanlı (Basit): birden çok sanal makineler
Bu uygulama modelinde farklı bir sanal makinede her uygulama katmanı koyarak 3 katmanlı uygulama azure'da dağıtın. Bu, kolay bir ölçek büyütme ve ölçek genişletme senaryoları için esnek bir ortam sağlar. Bir sanal makine istemci/web uygulamanızı içerdiğinde, diğeri, iş bileşenlerini barındıran ve diğeri veritabanı sunucusunu barındırır.

Bu uygulama düzeni ne zaman yararlı olur:

* Karmaşık veritabanı uygulamaları için Azure sanal makineleri geçirmek istediğiniz.
* Farklı bölgelerde barındırılması için farklı uygulama katmanları istiyor. Örneğin, raporlama amacıyla birden çok bölgeye dağıtılan veritabanları paylaşılan.
* Kuruluş uygulamaları, Azure sanal makineleri için şirket içi sanallaştırılmış platformlarından taşımak istersiniz. Kurumsal uygulamalar hakkında ayrıntılı bilgi için bkz: [Kurumsal uygulama nedir](https://msdn.microsoft.com/library/aa267045.aspx).
* Hızlı geliştirme sağlamak ve test ortamları kısa süreyle istiyor.
* Stres testi için değişen iş yükü düzeyleri gerçekleştirmek istediğiniz, ancak aynı anda sahip ve her zaman birçok fiziksel makineleri korumak istediğiniz değil.

Aşağıdaki diyagramda, Azure'da farklı bir sanal makinede her uygulama katmanı koyarak Basit 3 katmanlı uygulama nasıl yerleştirebilirsiniz gösterir.

![3 katmanlı uygulama düzeni](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728009.png)

Bu uygulama modelinde, her katman içinde yalnızca bir sanal makine (VM) yoktur. Azure içinde birden çok VM varsa, sanal bir ağ ayarlamanızı öneririz. [Azure sanal ağı](../../../virtual-network/virtual-networks-overview.md) güvenilir güvenlik sınırı oluşturur ve kendilerini arasında özel bir IP adresi iletişim kurmak sanal makineleri de sağlar. Ayrıca, her zaman tüm Internet bağlantıları için sunu katmanı yalnızca Git emin olun. Bu uygulama düzeni izleyerek, erişimi denetlemek için ağ güvenlik grubu kuralları yönetin. Daha fazla bilgi için bkz: [VM'nizi Azure portalını kullanarak dış erişmesine izin vermek](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Diyagramda Internet protokolleri TCP, UDP, HTTP veya HTTPS olabilir.

> [!NOTE]
> Azure sanal ağında ayarlama ücretsizdir. Ancak, şirket içi bağlayan VPN ağ geçidi için sizden ücret kesilir. Bu ücretsiz olarak bağlantı sağlandı ve kullanılabilir durumda süre miktarını temel alır.
> 
> 

## <a name="2-tier-and-3-tier-with-presentation-tier-scale-out"></a>Katman 2 ve 3 katmanlı sunu katmanı ölçek genişletme ile
Bu uygulama modelinde Katman 2 veya 3 katmanlı veritabanı uygulaması için Azure sanal makineleri farklı bir sanal makinede her uygulama katmanı koyarak dağıtın. Ayrıca, gelen istemci isteklerini artan hacmi nedeniyle sunu katmanı ölçeği genişletme.

Bu uygulama düzeni ne zaman yararlı olur:

* Kuruluş uygulamaları, Azure sanal makineleri için şirket içi sanallaştırılmış platformlarından taşımak istersiniz.
* Gelen istemci isteklerini artan hacmi nedeniyle sunu katmanı genişletmek istiyor.
* Hızlı geliştirme sağlamak ve test ortamları kısa süreyle istiyor.
* Stres testi için değişen iş yükü düzeyleri gerçekleştirmek istediğiniz, ancak aynı anda sahip ve her zaman birçok fiziksel makineleri korumak istediğiniz değil.
* İsteğe bağlı olarak yukarı ve aşağı ölçeklenebilen bir altyapısı ortamına sahip olmak istiyorsanız.

Aşağıdaki diyagramda, nasıl, uygulama katmanları birden çok sanal makine azure'da gelen istemci isteklerini artan hacmi nedeniyle sunu katmanı ölçeğini yerleştirebilirsiniz gösterir. Diyagramda görüldüğü gibi Azure yük dengeleyici trafiği birden çok sanal makineler arasında dağıtma ve ayrıca bağlanmak için hangi web sunucusu belirleme sorumludur. Web sunucuları yük dengeleyici arkasında birden çok örneğini sahip sunu katmanı yüksek kullanılabilirlik sağlar.

![Uygulama düzeni - sunu katmanı ölçek genişletme](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728010.png)

### <a name="best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier"></a>Birden çok VM'ye sahip bir katman 2 katmanlı, 3 katmanlı veya n katmanlı modelleri için en iyi uygulamalar
Kullanılabilirlik aynı bulut hizmetindeki ve aynı aynı katmana ait sanal makineler yerleştirmeniz önerilir ayarlayın. Örneğin, web sunucularının bir kümesi yerleştirin **CloudService1** ve **AvailabilitySet1** ve veritabanı sunucuları kümesi **CloudService2** ve  **AvailabilitySet2**. Kullanılabilirlik kümesi Azure'da yüksek kullanılabilirlik düğümleri ayrı hata etki alanları yerleştirin ve etki alanlarını yükseltme sağlar.

Bir katman birden çok VM örneği kullanabilmeniz için uygulama katmanları arasında Azure yük dengeleyici yapılandırmanız gerekir. Yük dengeleyici her katman yapılandırmak için her katmanın sanal makinelerin yük dengeli bir uç nokta ayrı olarak oluşturun. Belirli bir katman için sanal makineleri aynı bulut hizmetinde önce oluşturun. Bu, sahip oldukları aynı genel sanal IP adresi sağlar. Ardından, o katman sanal makinelerde birinde bir uç nokta oluşturun. Ardından, aynı uç nokta Yük Dengeleme için bu katmanda diğer sanal makineler atayın. Yük dengeli kümesi oluşturarak, trafiği birden çok sanal makinelerde dağıtmak ve ayrıca bir arka uç VM düğüm başarısız olduğunda bağlanmak için hangi düğümünü belirlemek yük dengeleyici izin. Örneğin, web sunucuları yük dengeleyici arkasında birden çok örneğini sahip sunu katmanı yüksek kullanılabilirlik sağlar.

En iyi uygulama, her zaman tüm internet bağlantıları için sunu katmanı ilk Git emin olun. İş katmanı sunu katmanı erişir ve ardından iş katmanı veri katmanı erişir. Sunu katmanı erişmesine izin vermek nasıl daha fazla bilgi için bkz: [VM'nizi Azure portalını kullanarak dış erişmesine izin vermek](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Azure yük dengeleyici benzer bir şirket içi ortama yük çalıştığını unutmayın. Daha fazla bilgi için bkz: [Yük Dengeleme için Azure altyapı hizmetleri](../tutorial-load-balancer.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Ayrıca, Azure sanal ağı kullanarak, sanal makineleriniz için özel bir ağ ayarlamanızı öneririz. Bu kendilerini arasında özel bir IP adresi iletişim kurmalarına izin verir. Daha fazla bilgi için bkz: [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md).

## <a name="2-tier-and-3-tier-with-business-tier-scale-out"></a>Katman 2 ve 3 katmanlı iş katmanı ölçek genişletme ile
Bu uygulama modelinde Katman 2 veya 3 katmanlı veritabanı uygulaması için Azure sanal makineleri farklı bir sanal makinede her uygulama katmanı koyarak dağıtın. Ayrıca, birden çok sanal makine uygulamanın karmaşıklığı nedeniyle uygulama sunucu bileşenlerinin dağıtmak isteyebilirsiniz.

Bu uygulama düzeni ne zaman yararlı olur:

* Kuruluş uygulamaları, Azure sanal makineleri için şirket içi sanallaştırılmış platformlarından taşımak istersiniz.
* Birden çok sanal makine uygulamanın karmaşıklığı nedeniyle uygulama sunucu bileşenlerinin dağıtmak istediğiniz.
* İş mantığı ağır içi LOB (iş,) uygulamaları için Azure sanal makineleri taşımak istersiniz. İş KOLU uygulamaları çalıştıran hesap, İnsan Kaynakları (HR), bordro, tedarik zinciri yönetimi ve kaynak planlama uygulamaları gibi kuruluş için önemli olan kritik bilgisayar uygulamalar kümesidir.
* Hızlı geliştirme sağlamak ve test ortamları kısa süreyle istiyor.
* Stres testi için değişen iş yükü düzeyleri gerçekleştirmek istediğiniz, ancak aynı anda sahip ve her zaman birçok fiziksel makineleri korumak istediğiniz değil.
* İsteğe bağlı olarak yukarı ve aşağı ölçeklenebilen bir altyapısı ortamına sahip olmak istiyorsanız.

Aşağıdaki diyagramda, şirket içi senaryo ve etkin bulut çözümünün gösterir. Bu senaryoda, iş mantığı katmanı ve veri erişim bileşenlerini içeren iş katmanı ölçeklendirme tarafından birden çok sanal makine azure'da uygulama katmanında yerleştirin. Diyagramda görüldüğü gibi Azure yük dengeleyici trafiği birden çok sanal makineler arasında dağıtma ve ayrıca bağlanmak için hangi web sunucusu belirleme sorumludur. Uygulama sunucuları yük dengeleyici arkasında birden çok örneğini sahip iş katmanı yüksek kullanılabilirlik sağlar. Daha fazla bilgi için bkz: [bir katmanında birden çok sanal makineye sahip Katman 2, 3 katmanlı veya n katmanlı uygulama düzenleri için en iyi uygulamaları](#best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier).

![İş katmanı ölçek genişletmeli uygulama düzeni](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728011.png)

## <a name="2-tier-and-3-tier-with-presentation-and-business-tiers-scale-out-and-hadr"></a>Katman 2 ve 3 katmanlı sunu ve iş katmanları genişleme ve HADR ile
Bu uygulama modelinde sunu katmanı (web sunucusu) ve birden çok sanal makine için iş Katmanı (uygulama sunucusu) bileşenleri dağıtarak Katman 2 veya 3 katmanlı veritabanı uygulaması için Azure sanal makinelerini dağıtın. Ayrıca, veritabanlarınızı Azure sanal makineler için yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümleri uygulayın.

Bu uygulama düzeni ne zaman yararlı olur:

* SQL Server yüksek kullanılabilirlik ve olağanüstü durum kurtarma özellikleri uygulayarak kurumsal uygulamalar sanallaştırılmış platformları şirket içinden Azure'a taşımak istersiniz.
* Sunu katmanı ve daha yüksek miktarda gelen istemci isteklerini ve uygulamanın karmaşıklığı nedeniyle iş katmanı genişletmek istiyor.
* Hızlı geliştirme sağlamak ve test ortamları kısa süreyle istiyor.
* Stres testi için değişen iş yükü düzeyleri gerçekleştirmek istediğiniz, ancak aynı anda sahip ve her zaman birçok fiziksel makineleri korumak istediğiniz değil.
* İsteğe bağlı olarak yukarı ve aşağı ölçeklenebilen bir altyapısı ortamına sahip olmak istiyorsanız.

Aşağıdaki diyagramda, şirket içi senaryo ve etkin bulut çözümünün gösterir. Bu senaryoda, sunu katmanı ve birden çok sanal makine azure'da iş katmanı bileşenlerinde ölçeğini. Ayrıca, Azure'da SQL Server veritabanları için yüksek kullanılabilirlik ve olağanüstü durum kurtarma (HADR) teknikleri uygulayın.

Bir uygulama birden çok kopyasını farklı çalıştıran VM'ler istekleri arasında gezinmek dengelemesini olduğundan emin olun. Birden çok sanal makine varsa, tüm Vm'leriniz zamanında erişilebilir bir noktada olduğundan ve çalıştığından emin olmanız gerekir. Yük Dengeleme yapılandırırsanız, Azure yük dengeleyici sanal makineleri durumunu izler ve iyi çalışan VM düğümleri gelen çağrıları düzgün şekilde yönlendirir. Sanal makinelerin Yük Dengeleme yukarı ayarlama konusunda daha fazla bilgi için bkz: [Yük Dengeleme için Azure altyapı hizmetleri](../tutorial-load-balancer.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Web ve uygulama sunucularını bir yük dengeleyici arkasında birden çok örneğini sahip sunu ve iş katmanı yüksek kullanılabilirlik sağlar.

![Genişletme ve yüksek kullanılabilirlik](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728012.png)

### <a name="best-practices-for-application-patterns-requiring-sql-hadr"></a>SQL HADR gerektiren uygulama düzenleri için en iyi yöntemler
SQL Server yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümleri Azure Virtual Machines'de ayarladığınızda, bir sanal ağ kullanarak, sanal makineleriniz ayarlama [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) zorunludur.  Bir sanal ağ içindeki sanal makineleri kararlı ve özel bir IP adresi sonra bile bir hizmet kesintisi olacaktır, böylece DNS ad çözümlemesi için gereken güncelleştirme zamanı önleyebilirsiniz. Ayrıca, sanal ağı şirket içi ağınızı Azure'a genişletmenizi sağlar ve güvenilir güvenlik sınırı oluşturur. Örneğin, uygulamanız (örneğin, Windows kimlik doğrulaması, Active Directory), şirket etki alanı kısıtlamaları sahipse ayarlama [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) gereklidir.

Üretim kodu Azure üzerinde çalıştırıyorsanız, müşterilerin çoğu, birincil ve ikincil çoğaltmaları Azure'da önlüyor.

Kapsamlı bilgi ve yüksek kullanılabilirlik ve olağanüstü durum kurtarma teknikleri öğreticiler için bkz: [yüksek kullanılabilirlik ve olağanüstü durum kurtarma Azure Virtual Machines'de SQL Server için](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="2-tier-and-3-tier-using-azure-vms-and-cloud-services"></a>Katman 2 ve 3 katmanlı Azure Vm'leri ve bulut Hizmetleri kullanma
Bu uygulama modelinde Katman 2 veya 3 katmanlı uygulama Azure her ikisini de kullanarak dağıttığınız [Azure Cloud Services](../../../cloud-services/cloud-services-choose-me.md#tellmecs) (web ve çalışan rolleri - (PaaS) hizmet olarak Platform) ve [Azure sanal makineleri](../overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) () Hizmet olarak altyapı (Iaas)). Kullanarak [Azure Cloud Services](https://azure.microsoft.com/documentation/services/cloud-services/) sunu katmanı/iş katmanı ve SQL Server için [Azure sanal makineleri](../overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) için veri katmanı Azure üzerinde çalışan çoğu uygulamalar için yararlıdır. Bulut hizmetleri üzerinde çalışan bir işlem örneğinde sahip bir kolay yönetimi, dağıtım, izleme ve genişleme sağladığı nedenidir.

Bulut Hizmetleri ile Azure altyapı sizin için tutar, bakım gerçekleştirir, işletim sistemleri düzeltme ekleri ve hizmet ve donanım hatalarından kurtarmaya çalışır. Uygulamanızı genişleme gerektiğinde otomatik ve el ile genişletme seçenekleri artırarak veya azaltarak örneği veya uygulamanız tarafından kullanılan sanal makine sayısı tarafından bulut hizmeti projenizi için kullanılabilir. Ayrıca, bir bulut hizmeti projesini Azure uygulamanızı dağıtmak için şirket içi Visual Studio'yu kullanabilirsiniz.

Özet olarak, kapsamlı sahip olmasını istemiyorsanız, yönetim görevlerini sunu/iş katmanı ve uygulamanız yok yazılım veya işletim sistemi herhangi bir karmaşık yapılandırma gerektirir, Azure bulut Hizmetleri kullanın. Azure SQL veritabanı aradığınız tüm özelliklerini desteklemeyen, SQL Server veri katmanı için bir Azure sanal makinesine kullanın. Azure Cloud Services üzerinde bir uygulama çalıştıran ve Azure sanal makinelerinde veri depolama her iki hizmet avantajlarını birleştirir. Ayrıntılı bir karşılaştırması için bu konudaki bölümüne bakarak [azure'da geliştirme stratejileri karşılaştırma](#comparing-web-development-strategies-in-azure).

Bu uygulama modelinde bir bulut hizmetleri bileşeni Azure yürütme ortamında çalışan ve IIS ve ASP.NET tarafından desteklenen gibi web uygulama programlama için özelleştirilmiş bir web rolü sunu katmanı içerir. İş veya arka uç katmanı bulut hizmetleri bileşen Azure yürütme ortamında çalıştığını ve genelleştirilmiş geliştirme için faydalı olur ve bir web rolü için arka plan işlemleri gerçekleştirebilir çalışan rolü içerir. Azure'da bir SQL Server sanal makine veritabanı katmanı bulunur. Sunu katmanı ve veritabanı katmanı arasındaki iletişimi, doğrudan ya da iş katmanı – çalışan rolü bileşenleri üzerinden gerçekleşir.

Bu uygulama düzeni ne zaman yararlı olur:

* SQL Server yüksek kullanılabilirlik ve olağanüstü durum kurtarma özellikleri uygulayarak kurumsal uygulamalar sanallaştırılmış platformları şirket içinden Azure'a taşımak istersiniz.
* İsteğe bağlı olarak yukarı ve aşağı ölçeklenebilen bir altyapısı ortamına sahip olmak istiyorsanız.
* Azure SQL veritabanı uygulamanızı veya veritabanı gereken tüm özellikleri desteklemiyor.
* Stres testi için değişen iş yükü düzeyleri gerçekleştirmek istediğiniz, ancak aynı anda sahip ve her zaman birçok fiziksel makineleri korumak istediğiniz değil.

Aşağıdaki diyagramda, şirket içi senaryo ve etkin bulut çözümünün gösterir. Bu senaryoda, sunu katmanı web rollerinde, iş katmanı çalışan rollerinde, ancak veri katmanı azure'daki sanal makinelerde yerleştirin. Yük Dengeleme isteklerini arasında gezinmek için farklı bir web rollerinde sunu katmanı birden çok kopyasını çalıştıran sağlar. Azure bulut Hizmetleri ile Azure sanal makineleri birleştirdiğinizde ayarlamanız önerilir [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) de. İle [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md), sizin sahip kararlı ve kalıcı özel IP adresleri içinde aynı bulut hizmetine bulutta. Bir sanal ağ, sanal makineleriniz için tanımlama ve bulut Hizmetleri zaman kendilerini arasında özel bir IP adresi iletişim kurmasını başlayabilirsiniz. Sanal makineler ve Azure web/çalışan rolleri aynı ek olarak, sahip olma [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) düşük gecikme süresi ve daha güvenli bir bağlantı sağlar. Daha fazla bilgi için bkz: [bir bulut hizmeti nedir](../../../cloud-services/cloud-services-choose-me.md).

Diyagramda görüldüğü gibi Azure yük dengeleyici trafiği birden çok sanal makine genelinde dağıtır ve hangi web sunucusu veya uygulama sunucusuna bağlanmak için de belirler. Web ve uygulama sunucularını bir yük dengeleyici arkasında birden çok örneğini sahip sunu katmanı ve iş katmanı yüksek kullanılabilirlik sağlar. Daha fazla bilgi için bkz: [en iyi yöntemler uygulaması gerektiren SQL HADR desenler için](#best-practices-for-application-patterns-requiring-sql-hadr).

![Bulut Hizmetleri ile uygulama düzenleri](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728013.png)

Bu uygulama düzeni uygulamak için başka bir yaklaşım, sunu katmanı ve aşağıdaki çizimde gösterildiği gibi iş katmanı bileşenlerini içeren birleştirilmiş web rolü kullanmaktır. Bu uygulama düzeni durum bilgisi olan tasarım gerektiren uygulamalar için kullanışlıdır. Azure web ve çalışan rolleri durum bilgisiz işlem düğümleri sağlar olduğundan, aşağıdaki teknolojileri birini kullanarak oturum durumunu depolamak için bir mantığı uygulamanız önerilir: [Azure önbelleğe alma](https://azure.microsoft.com/documentation/services/redis-cache/), [Azure Table Storage](../../../cosmos-db/table-storage-how-to-use-dotnet.md) veya [Azure SQL veritabanı](../../../sql-database/sql-database-technical-overview.md).

![Bulut Hizmetleri ile uygulama düzenleri](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728014.png)

## <a name="pattern-with-azure-vms-azure-sql-database-and-azure-app-service-web-apps"></a>Azure VM'ler, Azure SQL Database ve Azure uygulama hizmeti (Web uygulamaları) deseni
Bu uygulama düzeni birincil amacı, Azure altyapı (ıaas) bileşenleri bileşenlerle Azure hizmet olarak platform (PaaS), çözümünüzdeki olarak birleştirmek nasıl göstermektir. Bu desen ilişkisel veri depolama için Azure SQL veritabanı üzerinde odaklanmıştır. Bu SQL Server bir Azure sanal makinesinin, hizmet teklifi olarak Azure altyapısının bir parçası olduğu içermez.

Bu uygulama modelinde sunu ve iş katmanları aynı sanal makine yerleştirme ve Azure SQL veritabanı (SQL veritabanı) sunucuları bir veritabanına erişim Azure bir veritabanı uygulamayı dağıtın. Sunu katmanı geleneksel IIS tabanlı web çözümleri kullanarak uygulayabilirsiniz. Veya, bir birleşik sunu ve iş katmanı kullanarak uygulayabileceğiniz [Azure Web Apps](https://azure.microsoft.com/documentation/services/app-service/web/).

Bu uygulama düzeni ne zaman yararlı olur:

* Azure üzerinde yapılandırılmış varolan bir SQL veritabanı sunucusuna zaten var ve hızlı bir şekilde uygulamanızı test etmek istediğiniz.
* Azure ortamı özelliklerini test etmek istediğiniz.
* Hızlı geliştirme sağlamak ve test ortamları kısa süreyle istiyor.
* İş mantığı ve verileri erişim bileşenleri bir web uygulamasının içinden müstakil olabilir.

Aşağıdaki diyagramda, şirket içi senaryo ve etkin bulut çözümünün gösterir. Bu senaryoda, Azure SQL veritabanı Azure ve erişim verilerini tek bir sanal makine uygulama katmanında yerleştirin.

![Karma uygulama düzeni](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728015.png)

Azure Web Apps kullanarak bir birleşik web ve uygulama katmanı uygulama seçerseniz, bir web uygulaması bağlamında dinamik bağlantı kitaplıklarını (DLL'ler) olarak orta katmanda veya uygulama katmanı tutmanızı öneririz.

Ayrıca, verilen önerileri gözden [Azure web geliştirme stratejilerine karşılaştırma](#comparing-web-development-strategies-in-azure) teknikleri programlama hakkında daha fazla bilgi edinmek için bu makalenin sonunda bölüm.

## <a name="n-tier-hybrid-application-pattern"></a>N katmanlı karma uygulama düzeni
N katmanlı karma uygulama düzeni şirket içi ve Azure arasında dağıtılmış birden çok katman uygulamanızda uygulayın. Bu nedenle, esnek ve yeniden kullanılabilir karma sistem oluşturmak, değiştirmek veya diğer katmanları değiştirmeden belirli bir katman ekleyin. Şirket ağınıza buluta genişletmek için kullandığınız [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) hizmet.

Bu karma uygulama düzeni ne zaman yararlı olur:

* Kısmen bulutta çalıştırmak ve kısmen şirket içi uygulamalar oluşturmak istiyorsunuz.
* Bulut için bir şirket içi uygulamasının bazı veya tüm öğeleri geçirmek istediğiniz.
* Kuruluş uygulamaları şirket içi sanallaştırılmış platformlarından Azure'a taşımak istersiniz.
* İsteğe bağlı olarak yukarı ve aşağı ölçeklenebilen bir altyapısı ortamına sahip olmak istiyorsanız.
* Hızlı geliştirme sağlamak ve test ortamları kısa süreyle istiyor.
* Kurumsal veritabanı uygulamaları için yedeklemeleri almak için uygun maliyetli bir yöntem istiyor.

Aşağıdaki diyagramda, şirket içi ve Azure üzerinden yayılan bir n katmanlı karma uygulama düzeni gösterir. Aşağıdaki çizimde gösterildiği gibi altyapı içeren şirket içi [Active Directory etki alanı Hizmetleri](https://technet.microsoft.com/library/hh831484.aspx) kullanıcı kimlik doğrulaması ve yetkilendirme desteklemek için etki alanı denetleyicisi. Diyagram Azure'da veri katmanı bazı bölümleri Canlı ancak veri katmanı bazı bölümleri bir şirket içi veri merkezinde burada canlı bir senaryo gösterilmektedir unutmayın. Uygulamanızın gereksinimlerine bağlı olarak birkaç diğer karma senaryolar uygulayabilirsiniz. Örneğin, bir şirket içi ortamda ancak azure'da veri katmanı sunu katmanı ve iş katmanı tutabilirsiniz.

![N katmanlı uygulama düzeni](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728016.png)

Azure'da, Active Directory tek başına bulut dizini olarak kuruluşunuz için kullanabileceğiniz ya da mevcut şirket içi Active Directory ile de tümleştirebilir [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Diyagramda görüldüğü gibi iş katmanı bileşenleri birden çok veri kaynağına gibi için erişebileceği [Azure SQL Server'da](virtual-machines-windows-sql-server-iaas-overview.md) özel bir iç IP adresi için SQL Server aracılığıyla şirket içi [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md), veya [SQL veritabanı](../../../sql-database/sql-database-technical-overview.md) .NET Framework veri sağlayıcısı teknolojilerini kullanarak. Bu diyagramda, Azure SQL veritabanı bir isteğe bağlı veri depolama hizmetidir.

N katmanlı karma uygulama düzeni aşağıdaki iş akışı belirtilen sırada uygulayabilirsiniz:

1. Kullanarak buluta taşınması gereken Kurumsal veritabanı uygulamaları belirlemek [Microsoft Assessment ve planlama (MAP) Araç Seti](http://microsoft.com/map). MAP Araç Kiti sanallaştırma için dikkate bilgisayarlardan Envanter ve performans verilerini toplar ve kapasite ve değerlendirme planlama önerileri sağlar.
2. Yapılandırma depolama hesapları ve sanal makineler gibi Azure platformu gerekmez ve kaynak planlayın.
3. Şirket ağı şirket içi arasında ağ bağlantısı ayarlama ve [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md). Şirket ağı şirket içi ve azure'da bir sanal makine arasındaki bağlantıyı ayarlamak için aşağıdaki iki yöntemden birini kullanın:
   
   1. Azure'da bir sanal makinede ortak uç noktaları yoluyla şirket içi ve Azure arasında bir bağlantı oluşturun. Bu yöntem, kolay kurulum sağlar ve sanal makinenizde SQL Server kimlik doğrulama kullanmanıza olanak tanır. Ayrıca, VM'ye ortak trafiği denetlemek için ağ güvenlik grubu kurallarınızı ayarlayın. Daha fazla bilgi için bkz: [VM'nizi Azure portalını kullanarak dış erişmesine izin vermek](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
   2. Azure sanal özel ağ (VPN) tüneli yoluyla şirket içi ve Azure arasında bağlantı kurun. Bu yöntem azure'da bir sanal makine için etki alanı ilkelerini genişletmenizi sağlar. Ayrıca, güvenlik duvarı kuralları ayarlamak ve sanal makinenizde Windows kimlik doğrulaması kullanın. Şu anda, Azure güvenli siteden siteye VPN ve noktadan siteye VPN bağlantılarını destekler:
      
      * Güvenli siteden siteye bağlantı ile ağ bağlantısı şirket içi ağınız ile sanal ağınız arasında Azure'da kurabilirsiniz. Şirket içi veri merkezi ortamınızı Azure'a bağlanmak için önerilir.
      * Güvenli noktadan siteye bağlantı ile Azure sanal ağınızda ve herhangi bir yere çalıştıran bağımsız bilgisayar arasında ağ bağlantısı kurabilirsiniz. Geliştirme ve test amaçları için çoğunlukla önerilir.
      
      Azure SQL sunucusuna bağlanma hakkında daha fazla bilgi için bkz: [bir SQL Server sanal makinesine bağlanma Azure üzerinde](virtual-machines-windows-sql-connect.md).
4. Zamanlanan işler ve şirket içi verileri azure'da bir sanal makine disk yedekleme Uyarıları ayarlayın. Daha fazla bilgi için bkz: [SQL Server Yedekleme ve geri yükleme ile Azure Blob Depolama hizmetinin](https://msdn.microsoft.com/library/jj919148.aspx) ve [yedekleme ve Azure Virtual Machines'de SQL Server için geri yükleme](virtual-machines-windows-sql-backup-recovery.md).
5. Uygulamanızın gereksinimlerine bağlı olarak, aşağıdaki üç sık karşılaşılan senaryolardan biri, uygulayabilirsiniz:
   
   1. Hassas verileri şirket içi tutmak ancak azure'da veritabanı sunucusundaki web sunucusu, uygulama sunucusu ve duyarlı verileri tutabilirsiniz.
   2. Web sunucunuzun tutabilir ve uygulama sunucusu şirket içi ise bir sanal makinede Azure veritabanı sunucusu.
   3. Tutabilirsiniz veritabanı sunucusu, web sunucusu ve uygulama sunucusu azure'daki sanal makinelerde veritabanı çoğaltmalarını tutmak ancak şirket içi. Bu ayar şirket içi web sunucuları veya azure'da veritabanı çoğaltmaları erişmek için raporlama uygulamaları sağlar. Bu nedenle, bir şirket içi veritabanındaki iş yükünü azaltmak için elde edebilirsiniz. Bu senaryo için ağır uygulamak öneririz iş yükleri ve gelişim amacıyla okuyun. AlwaysOn Kullanılabilirlik grupları adresindeki Azure'da veritabanı çoğaltmaları oluşturma hakkında daha fazla bilgi için bkz [yüksek kullanılabilirlik ve olağanüstü durum kurtarma Azure Virtual Machines'de SQL Server için](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="comparing-web-development-strategies-in-azure"></a>Azure Web geliştirme stratejilerine karşılaştırma
Uygulama ve çok katmanlı bir SQL Server tabanlı bir uygulama azure'da dağıtmak için aşağıdaki iki programlama yöntemlerden birini kullanabilirsiniz:

* Azure Virtual Machines'de SQL Server'ın Azure ve erişim veritabanlarındaki bir geleneksel web sunucusu (IIS - Internet Information Services) ayarlayın.
* Uygulama ve bir bulut hizmeti Azure'a dağıtın. Ardından, bu bulut hizmeti SQL Server'da veritabanlarını Azure sanal makinelerinde erişebildiğinden emin olun. Bir bulut hizmeti birden çok web ve çalışan rolleri ekleyebilirsiniz.

Aşağıdaki tabloda geleneksel web geliştirme Azure Cloud Services ve Azure Virtual Machines'de SQL Server göre Azure Web uygulamaları ile karşılaştırması sağlar. Tablo, SQL Server Azure VM'de bir veri kaynağı olarak Azure Web uygulamaları için ortak sanal IP adresi veya DNS adı kullanmak mümkün olduğu gibi Azure Web uygulamaları içerir.

|  | Azure Virtual Machines'de geleneksel web geliştirme | Azure bulut Hizmetleri | Web barındırma ile Azure Web uygulamaları |
| --- | --- | --- | --- |
| **Şirket içi uygulama geçiş** |Var olan uygulamalar olarak-değil. |Uygulamaları, web ve çalışan rolleri gerekir. |Var olan uygulamalar olarak-kendi içindeki web uygulamaları ve hızlı ölçeklenebilirlik gerektiren web hizmetleri için ancak uygundur. |
| **Geliştirme ve dağıtım** |Visual Studio, WebMatrix, Visual Web Developer, WebDeploy, FTP, TFS, IIS Yöneticisi, PowerShell. |Visual Studio, Azure SDK, TFS, PowerShell. Her bir bulut hizmeti için dağıtabileceğiniz hizmet paketi ve yapılandırma için iki ortam vardır: hazırlama ve üretim. Bir bulut hizmeti üretime Yükselt önce test etmek için hazırlık ortamı dağıtabilirsiniz. |Visual Studio, WebMatrix, Visual Web Developer, FTP, GIT, BitBucket, CodePlex, DropBox, GitHub, Mercurial, TFS, Web PowerShell dağıtın. |
| **Yönetim ve Kur** |Uygulama, verileri, güvenlik duvarı kuralları, sanal ağ ve işletim sistemi yönetim görevlerini siz sorumlusunuz. |Uygulama, verileri, güvenlik duvarı kuralları ve sanal ağ üzerindeki yönetim görevleri için siz sorumlusunuz. |Yalnızca veri ve uygulama yönetim görevlerini siz sorumlusunuz. |
| **Yüksek kullanılabilirlik ve olağanüstü durum kurtarma (HADR)** |Sanal makineler aynı kullanılabilirlik kümesinde ve aynı bulut hizmetine yerleştirin öneririz. Vm'leriniz aynı kalmasını yüksek kullanılabilirlik düğümleri ayrı hata etki alanları yerleştirin ve etki alanlarını yükseltme Azure kullanılabilirlik kümesi sağlar. Benzer şekilde, aynı bulut hizmetinde Vm'leriniz tutma Yük Dengeleme sağlar ve VM'lerin doğrudan birbiriyle bir Azure veri merkezi içinde yerel ağ üzerinden iletişim kurabilir.<br/><br/>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümü için SQL Server herhangi kesinti süresini önlemek için Azure sanal makinelerinde uygulamak için sorumlu. Desteklenen HADR teknolojileri için bkz: [yüksek kullanılabilirlik ve olağanüstü durum kurtarma Azure Virtual Machines'de SQL Server için](virtual-machines-windows-sql-high-availability-dr.md).<br/><br/>Kendi veri ve uygulama yedekleme için sorumlu.<br/><br/>Ana makine veri merkezindeki donanım sorunları nedeniyle başarısız olursa, azure sanal makinelerinizi taşıyabilirsiniz. Ayrıca, olabilir planlanan kapalı kalma süresi, VM'nin konak makine güvenlik veya yazılım güncelleştirildiğinde güncelleştirmeler. Bu nedenle, sürekli kullanılabilirliğini sağlamak için en az iki VM her uygulama katmanında korumak öneririz. Azure SLA için tek bir sanal makine sağlamaz. Daha fazla bilgi için bkz: [Azure dayanıklılık teknik kılavuz](../../../resiliency/resiliency-technical-guidance.md). |Azure temel alınan donanım veya işletim sistemi yazılımlardan kaynaklanan hataları yönetir. Uygulamanızın yüksek kullanılabilirlik sağlamak için bir web veya çalışan rolü birden çok örneğini uygulamak öneririz. Bilgi için bkz: [bulut Hizmetleri, sanal makineler ve sanal ağ hizmet düzeyi sözleşmesi](http://www.microsoft.com/download/details.aspx?id=38427) ve [Azure uygulamaları için yüksek kullanılabilirlik ve olağanüstü durum kurtarma](../../../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)<br/><br/>Kendi veri ve uygulama yedekleme için sorumlu.<br/><br/>Azure VM'de SQL Server veritabanında bulunan veritabanları için siz herhangi kesinti süresini önlemek için yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümü uygulamak için sorumlu olursunuz. Desteklenen HDAR teknolojileri için Azure Virtual Machines'de SQL Server için yüksek kullanılabilirlik ve olağanüstü durum kurtarma bakın.<br/><br/>**SQL Server veritabanı yansıtma**: Azure bulut hizmetlerine (web/çalışan rolleri) kullanın. SQL Server VM'ler ve bir bulut hizmeti projesini aynı Azure sanal ağında olabilir. SQL Server VM aynı sanal ağ içinde değilse, SQL Server örneğine iletişimi yönlendirmek için bir SQL Server diğer oluşturmanız gerekir. Ayrıca, diğer adı SQL Server adı eşleşmelidir. |Yüksek kullanılabilirlik Azure çalışan rolleri, Azure blob depolama ve Azure SQL veritabanı devralınır. Örneğin, Azure Storage tüm blob, tablo ve kuyruk veri üç çoğaltmalarını korur. Herhangi bir zamanda Azure SQL Database çalıştıran verilerin üç çoğaltmalarının tutar — bir birincil çoğaltma ve iki ikincil çoğaltma. Daha fazla bilgi için bkz: [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) ve [Azure SQL veritabanı](../../../sql-database/sql-database-technical-overview.md).<br/><br/>SQL Server Azure Web uygulamaları için bir veri kaynağı olarak Azure VM'de kullanırken, Azure Web uygulamaları Azure sanal ağı desteklemiyor göz önünde bulundurun. Diğer bir deyişle, Azure Web Apps gelen tüm bağlantıları Azure SQL Server vm'lerinin sanal makinelerin genel Uç noktalara gitmeniz gerekir. Bu, yüksek kullanılabilirlik ve olağanüstü durum kurtarma senaryoları için bazı sınırlamalar neden olabilir. Örneğin, istemci uygulamasını Azure Web Apps veritabanı yansıtma ile SQL Server VM'ye bağlanma veritabanı yansıtma Azure sanal ağı Azure SQL Server ana VM'ler arasında ayarlama gerektirdiği şekilde yeni birincil sunucuya mümkün olmayacaktır. Bu nedenle, kullanarak **SQL Server veritabanı yansıtma** Azure Web Apps ile şu anda desteklenmiyor.<br/><br/>**SQL Server AlwaysOn Kullanılabilirlik grupları**: Azure Web Apps ile Azure SQL Server Vm'lerinin kullanırken AlwaysOn Kullanılabilirlik grupları ayarlayabilirsiniz. Ancak birincil çoğaltmaya yük dengeli bir ortak uç noktaları aracılığıyla iletişime yönlendirmek için AlwaysOn Kullanılabilirlik grubu dinleyicisini yapılandırmak gerekir. |
| **Şirket içi bağlantılar** |Şirket içi bağlanmak için Azure sanal ağı kullanabilirsiniz. |Şirket içi bağlanmak için Azure sanal ağı kullanabilirsiniz. |Azure sanal ağı desteklenir. Daha fazla bilgi için bkz: [Web uygulamaları sanal ağ tümleştirmesinin](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/). |
| **Ölçeklenebilirlik** |Sanal makine boyutlarını artırmayı veya daha fazla disk ekleme büyütme kullanılabilir. Sanal makine boyutları hakkında daha fazla bilgi için bkz: [Azure için sanal makine boyutlarını](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).<br/><br/>**Veritabanı sunucusu için**: genişleme veritabanı bölümleme teknikleri ve SQL Server AlwaysOn Kullanılabilirlik grupları aracılığıyla kullanılabilir.<br/><br/>Yoğun okuma iş yükleri için kullandığınız [AlwaysOn Kullanılabilirlik grupları](https://msdn.microsoft.com/library/hh510230.aspx) SQL Server çoğaltması yanı sıra birden fazla ikincil düğümü.<br/><br/>Ağır yazma iş yükleri için uygulama genişleme sağlamak için birden fazla fiziksel sunucu üzerinde yatay bölümleme veri uygulayabilirsiniz.<br/><br/>Kullanarak bir genişletme ayrıca uygulayabilirsiniz [SQL Server veri bağımlı yönlendirme ile](https://technet.microsoft.com/library/cc966448.aspx). Veri bağımlı yönlendirme (DDR ile), birden çok SQL Server düğümü için veritabanı isteği yönlendirmek için istemci uygulamasında, genellikle iş katmanı katmanı bölümleme mekanizması uygulama gerekir. İş katmanı nasıl verileri bölümlenen ve hangi düğümün verileri içeren eşlemeler içerir.<br/><br/>Sanal makineler çalışmakta olan uygulamalar ölçeklendirebilirsiniz. Daha fazla bilgi için bkz: [bir uygulama ölçeklendirme](../../../cloud-services/cloud-services-how-to-scale-portal.md).<br/><br/>**Önemli Not**: **otomatik ölçeklendirme** azure'daki özelliği otomatik olarak artırma veya uygulamanız tarafından kullanılan sanal makineleri azaltma olanak tanır. Bu özellik yoğun dönemlerde son kullanıcı deneyimi olumsuz etkilenmez ve isteğe bağlı düşük olduğunda VM'ler kapatıldığından güvence altına alır. SQL Server Vm'lerinin içeriyorsa, otomatik ölçeklendirme seçeneği'ı bulut hizmetinizin ayarlamayın önerilir. Otomatik ölçeklendirme özelliği, Azure CPU kullanımı bu VM'de bazı eşik değerinden daha yüksek olduğunda bir sanal makineyi açma ve CPU kullanımı, düşük olduğunda bir sanal makineyi kapatın olanak tanır nedenidir. Otomatik ölçeklendirme özelliği, tüm VM iş yükü olmadan önceki bir duruma yönelik tüm başvuruları yönetebileceği web sunucuları gibi durum bilgisiz uygulamaları için kullanışlıdır. Ancak, otomatik ölçeklendirme özelliği SQL Server, burada yalnızca bir örneği veritabanına yazma sağlar gibi durum bilgisi olan uygulamalar için yararlı değildir. |Büyütme birden çok web ve çalışan rolleri ile kullanılabilir. Web rolleri ve çalışan rolleri için sanal makine boyutları hakkında daha fazla bilgi için bkz: [Cloud Services boyutları yapılandırma](../../../cloud-services/cloud-services-sizes-specs.md).<br/><br/>Kullanırken **bulut Hizmetleri**, işleme dağıtabilir ve ayrıca uygulamanızı esnek ölçeklemeyi elde etmek için birden çok rol tanımlayabilirsiniz. Her bir bulut hizmeti bir veya daha fazla web rolleri ve/veya çalışan rolleri, her biri kendi uygulama dosyalarını ve yapılandırma içerir. Bir bulut hizmeti bir rol için dağıtılan (sanal makineler) rol örneklerinin sayısını artırarak büyütme ve ölçek bir bulut Hizmeti rol örneklerinin sayısını azaltarak açılır. Ayrıntılı bilgi için bkz: [Azure yürütme modelleri](../../../cloud-services/cloud-services-choose-me.md).<br/><br/>Azure yerleşik yüksek kullanılabilirlik desteğini aracılığıyla genişleme kullanılabilir [bulut Hizmetleri, sanal makineler ve sanal ağ hizmet düzeyi sözleşmesi](http://www.microsoft.com/download/details.aspx?id=38427) ve yük dengeleyici.<br/><br/>Çok katmanlı bir uygulama için veritabanı sunucusu sanal makineleri Azure sanal ağı aracılığıyla web/çalışan rolleri uygulamaya bağlanmak öneririz. Ayrıca, kullanıcı isteklerini arasında gezinmek yayılmak Azure VM'ler için aynı bulut hizmetinde yük dengelemesini sağlar. Bu şekilde bağlı sanal makinelere, bir Azure veri merkezi içinde yerel ağ üzerinden doğrudan birbiriyle iletişim kurabilir.<br/><br/>Ayarlayabilirsiniz **otomatik ölçeklendirme** zamanlar yanı sıra Azure portalı. Daha fazla bilgi için bkz: [bir bulut hizmeti Portalı'nda ölçeklendirme otomatik yapılandırma](../../../cloud-services/cloud-services-how-to-scale-portal.md). |**Yukarı ve aşağı doğru ölçeklendirme**:, artırma/web siteniz için ayrılmış örneğinin (VM) boyutunu azaltma.<br/><br/>Ölçeği genişletme: web siteniz için daha ayrılmış örnekler (VM'ler) ekleyebilirsiniz.<br/><br/>Ayarlayabilirsiniz **otomatik ölçeklendirme** portal ve bunun yanı sıra zamanlar. Daha fazla bilgi için bkz: [ölçek Web uygulamaları için nasıl](../../../app-service/web-sites-scale.md). |

Bu programlama yöntemleri arasında seçme hakkında daha fazla bilgi için bkz: [Azure Web uygulamaları, bulut Hizmetleri ve sanal makineleri: hangi kullanmak ne zaman](../../../app-service/choose-web-site-cloud-service-vm.md).

## <a name="next-steps"></a>Sonraki Adımlar
SQL Server Azure sanal makinelerde çalıştırma hakkında daha fazla bilgi için bkz: [Azure sanal makineleri genel bakış SQL Server'da](virtual-machines-windows-sql-server-iaas-overview.md).

