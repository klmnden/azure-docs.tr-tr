---
title: Azure BT operatörleri için Başlarken Kılavuzu | Microsoft Docs
description: Azure BT operatörleri için Başlarken Kılavuzu edinin
services: ''
documentationcenter: ''
author: themichaelbender-ms
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.devlang: ''
ms.topic: overview
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 08/24/2018
ms.author: mibender
ms.openlocfilehash: 1222395fd8efb7cf189ae6678f6c39f5a6c63157
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59051179"
---
# <a name="get-started-for-azure-it-operators"></a>Azure BT operatörleri için Başlarken

Bu kılavuz, bir Microsoft Azure altyapısının yönetim ve dağıtım için ilgili temel kavramlar tanıtılmaktadır. Yeni başladıysanız bilgi işlem veya Azure bulut hizmetine kendisi, bu kılavuz hızla kavramlarını, dağıtım ve yönetim ayrıntıları başlamanıza yardımcı olmak yardımcı olur. Bu kılavuz birçok bölümlerini bir sanal makine dağıtma gibi bir işlem tartışın ve sonra ayrıntılı teknik bilgiler için bir bağlantı sağlayın.

## <a name="cloud-computing-overview"></a>Bulut bilgi işleme genel bakış

Bulut bilgi işlem geleneksel şirket içi veri merkezine modern bir alternatif sunar. Genel bulut satıcıları, sağlar ve tüm bilgi işlem altyapısı ve temel yönetim yazılımı yönetir. Bu satıcıların, çok çeşitli bulut hizmetleri sağlar. Bir bulut hizmeti, bir sanal makine, web sunucusu veya bulutta barındırılan veritabanı altyapısı bu durumda olabilir. Bir bulut sağlayıcısı müşterisi olarak, bu bulut hizmetlerinden gerektiğinde olarak kira. Bunun yapılması, donanım bakım yatırım giderlerini bir işlem maliyetiyle dönüştürün. Bir bulut hizmeti, ayrıca şu avantajları sağlar:

- Büyük işlem ortamlarının hızlı dağıtımı

- Artık gerekli olmayan sistemleri hızlı ayırmayı kaldırma

- Geleneksel olarak karmaşık sistemlerin kolay dağıtım istediğiniz yük Dengeleyiciler

- Esnek işlem kapasitesi sağlayacak veya gerektiğinde ölçeklendirme olanağı

- Daha düşük maliyetli bilgi işlem ortamlarının

- Herhangi bir web tabanlı portal ya da programlı Otomasyon erişmek

- Çoğu işlem ve uygulama ihtiyaçlarınızı karşılamak için bulut tabanlı hizmeti

Şirket içi altyapı ile dağıtılan yazılım ve donanım üzerinde tam denetime sahiptir. Tarihsel olarak, bu donanım satın alma kararları odaklanan büyütme üzerinde açmıştır. Örnek en yüksek performans gereksinimlerini karşılamak için daha fazla çekirdeğe sahip bir sunucu satın alıyorsa. Ne yazık ki bu altyapı isteğe penceresi dışında potansiyelinden az kullanılmasına neden. Azure sayesinde yalnızca gereksinim duyduğunuz altyapı dağıtımı ve bu yukarı veya aşağı herhangi bir zamanda ayarlayın. Bu bir odağı ek işlem düğümleri dağıtımını genişletme üzerinde performans gereksinimini karşılamak için yol açar. Cloud services'ı ölçeklendirme pahalı donanımlar büyütme daha uygun maliyetli olup.

Microsoft, dünya çapındaki birçok Azure veri merkezleri ile daha fazla planlanmış dağıtmıştır. Microsoft ayrıca, Çin ve Almanya bölgelerinde bağımsız bulutlarda artmaktadır. Azure kullanan kuruluşlar, müşterilerinize yakın hizmetlerini dağıtmak için her boyuttaki kolaylaştırır, böylece yalnızca genel en büyük kuruluşların veri merkezlerinde bu şekilde dağıtabilirsiniz.

Küçük ölçekli işletmeler için işlem artışları için isteğe bağlı olarak hızlı bir şekilde ölçeklendirme olanağı ile düşük maliyetli giriş noktası için Azure sağlar. Bu büyük bir ön sermaye yatırımı altyapısında engeller ve mimari ve yeniden gerektiğinde sistemleri Mimarı esnekliği sağlar. Bulut ölçeği hızlı ve hızlı çıkışlı model başlangıç büyümesi ile uygun bilgi işlem kullanımı.

Kullanılabilir Azure bölgeleri hakkında daha fazla bilgi için bkz. [Azure bölgeleri](https://azure.microsoft.com/regions/).

### <a name="cloud-computing-model"></a>Bulut bilgi işlem modeli

Azure, bir bulut müşterilerine sunulan hizmet kategorilerini dayalı modeli kullanır. Hizmet üç kategoriye olarak bir hizmet (Iaas), Platform (PaaS) hizmet olarak yazılım (SaaS) hizmet olarak altyapı içerir. Satıcılar, bazılarını veya tümünü bu kategorilerin her bilgi işlem yığını bileşenlerini sorumluluğunu paylaşır. Bulut için kategorilerin her birine bir göz atalım bilgi işlem.
![Bulut bilgi işlem yığını karşılaştırması](./media/cloud-computing-comparison.png)

#### <a name="iaas-infrastructure-as-a-service"></a>Iaas: Hizmet olarak altyapı

Bir Iaas bulut satıcı çalıştırır ve tüm fiziksel hesaplama kaynakları ve bilgisayar sanallaştırmayı etkinleştirmek için gerekli yazılımı yönetir. Bu hizmet, bir müşteri sanal makineleri bu barındırılan veri merkezleri içinde dağıtır. Sanal makineleri bir şirket dışı veri merkezinde bulunan olsa da, Iaas tüketici yapılandırma ve yönetim işletim sisteminin altyapının bulut satıcıya bırakarak üzerinde denetimi yoktur.

Azure sanal makineler, sanal makine ölçek kümeleri ve ilgili ağ altyapısı dahil olmak üzere çeşitli Iaas çözümleri içerir. Sanal makinelerin bir popüler olduğunu başlangıçta geçirmek istediğiniz hizmetleri Azure'a çünkü bir "kaldırma ve kaydırma" geçiş modeli sağlar. Şu anda, hizmetler, veri merkezinde çalışan altyapısı gibi bir VM yapılandırın ve ardından yazılım yeni VM'ye geçirmeniz. Diğer hizmetlere veya depolama URL'leri gibi yapılandırma güncelleştirmeleri yapmanız gerekebilir, ancak çoğu uygulama bu şekilde geçiş yapabilirsiniz.

Sanal makine ölçek kümeleri, Azure sanal makineler üzerinde oluşturulmuş ve kümeleri, birbirinin aynısı olan Vm'leri dağıtmak için kolay bir yol sağlar. Böylece yeni VM'ler otomatik olarak gerektiğinde dağıtılabilir sanal makine ölçek kümeleri, ayrıca otomatik ölçeklendirmeyi destekler. Bu sanal makine ölçek kümeleri için ideal bir platform konak üst düzey mikro işlem kümeleri, Azure Service Fabric ve Azure Container Service gibi yapar.

#### <a name="paas-platform-as-a-service"></a>PaaS: Hizmet olarak platform

PaaS ile bulut hizmeti satıcısı sağlayan bir ortama uygulamanızı dağıtın. Satıcı tüm veri yönetimi ve uygulama geliştirme üzerinde odaklanabilirsiniz altyapı yönetimini desteklemez.

Azure teklifleri, Azure App Service ve Azure Cloud Services (web ve çalışan rolleri), Web Apps özelliği dahil olmak üzere çeşitli PaaS işlem sağlar. Her iki durumda da geliştiriciler uygulamalarını destekleyen yazılımındaki ilgili hiçbir şeyi bilmeden dağıtmak için birçok yolu vardır. Geliştiriciler, sanal makineler (VM'ler) oluşturmak, Uzak Masaüstü Protokolü (RDP) her biri için oturum açarken kullandığınız ya da uygulamayı yüklemek zorunda kalmaz. Bunlar yalnızca bir düğmesine tıklayın (veya kapatmak için) ve Microsoft tarafından sağlanan araçları Vm'leri hazırlama dağıtın ve uygulamayı yükler.

#### <a name="saas-software-as-a-service"></a>SaaS: Hizmet olarak yazılım

SaaS merkezi olarak barındırılan ve yönetilen bir yazılımdır. Çok kiracılı bir mimaride genellikle temel: tek bir sürüm uygulamanın tüm müşteriler için kullanılır. Bunu tüm konumlarda en iyi performansı elde etmek için birden çok örneğe genişletilebilir. SaaS yazılım genellikle bir aylık veya yıllık aboneliğiniz lisanslanır. SaaS yazılım genellikle bir aylık veya yıllık aboneliğiniz lisanslanır. Yönettiğiniz tüm için sağlanan hizmetleri SaaS yazılım satıcıları için yazılım yığınının tüm bileşenleri sorumludur.

Microsoft Office 365 sunan bir SaaS iyi bir örnektir. Aboneleri aylık veya yıllık abonelik ücreti ödersiniz ve Microsoft Exchange, Microsoft OneDrive ve hizmet olarak Microsoft Office paketinin geri kalanını alın. Abonelerin her zaman en son sürümü Al ve Exchange server sizin adınıza yönetilir. Her yıl Office Yükseltme ve yükleme ile karşılaştırıldığında, bu daha düşük maliyetli ve daha az çaba gerektirir.

## <a name="azure-services"></a>Azure hizmetleri

Azure bulut bilgi işlem platformu, birçok hizmet sunar. Bu hizmetler şunları içerir.

### <a name="compute-services"></a>İşlem hizmetleri

Hizmetler, barındırma ve uygulama iş yükünü çalıştırmak için:

- Azure sanal makineler — Linux ve Windows

- Uygulama Hizmetleri (Web uygulamaları, mobil uygulamalar, Logic Apps, API Apps ve işlev uygulamaları)

- Azure Batch (için büyük ölçekli paralel ve toplu bilgi işlem işlerini)

- Azure Service Fabric

- Azure Container Service

### <a name="data-services"></a>Veri hizmetleri

Depolama ve Veri Yönetimi Hizmetleri:

- Azure depolama (Azure Blob, kuyruk, tablo ve Dosya Hizmetleri oluşur)

- Azure SQL Database

- Azure Cosmos DB

- Microsoft Azure StorSimple

- Redis için Azure Önbelleği

### <a name="application-services"></a>Uygulama hizmetleri

Uygulamaları oluşturmaya ve hizmetler:

- Azure Active Directory (Azure AD)

- Dağıtılmış sistemler bağlanmak için Azure Service Bus

- Azure HDInsight, büyük veri işleme

- Azure Zamanlayıcı

- Azure Media Services

### <a name="network-services"></a>Ağ Hizmetleri

Hem Azure içindeki hem de Azure ve şirket içi veri merkezleri arasında Ağ Hizmetleri:

- Azure Sanal Ağ

- Azure ExpressRoute

- Azure tarafından sağlanan DNS

- Azure Traffic Manager

- Azure Content Delivery Network

Azure Hizmetleri ile ilgili ayrıntılı belgeler için bkz. [Azure hizmet belgeleri](https://docs.microsoft.com/azure).

## <a name="azure-key-concepts"></a>Azure temel kavramları

### <a name="datacenters-and-regions"></a>Veri merkezleri ve bölgeleri

Azure dünyanın dört bir yanındaki birçok bölgede genel kullanıma açık olan bir genel bulut platformudur. Bir hizmet, uygulama veya azure'da VM sağlarken bir bölge seçin istenir. Seçilen bölge, uygulamanızın çalıştığı speciﬁc veri merkezini temsil eder. Daha fazla bilgi için [Azure bölgeleri](https://azure.microsoft.com/regions/).

Azure kullanımının beneﬁts dünyanın çeşitli veri merkezleri, uygulamalarınızı dağıtabilirsiniz biridir. Seçtiğiniz bölge aﬀect, uygulamanızın performansı yapabilirsiniz. En iyi ağ istek gecikme süresini azaltmak için en müşterilerinizin yakın bir bölge seçin. Ayrıca, uygulamanızın belirli ülkelerde dağıtmak için yasal gereksinimleri karşılamak için bir bölge seçebilirsiniz.

### <a name="azure-portal"></a>Azure portal

Azure portalında Azure kaynaklarını ve Hizmetleri Kaldır oluşturmak ve yönetmek için kullanılan bir web tabanlı bir uygulamadır. Azure portalında şu konumdadır [portal.azure.com](https://portal.azure.com). Bu, özelleştirilebilir bir Pano ve Azure kaynaklarını yönetmek için Araçlar içerir. Ayrıca, faturalandırma ve abonelik bilgileri sağlar. Daha fazla bilgi için [Microsoft Azure portalına genel bakış](https://azure.microsoft.com/documentation/articles/azure-portal-overview/) ve [Azure kaynaklarınızı portal üzerinden yönetme](https://docs.microsoft.com/azure/azure-portal/resource-group-portal).

### <a name="resources"></a>Kaynaklar

Azure kaynaklarını tek tek işlem, ağ, veri veya uygulama barındırma, Azure aboneliğinin dağıtılan Hizmetleri ' dir. Bazı ortak sanal makineler, depolama hesabı veya SQL veritabanları kaynaklardır. Azure Hizmetleri, genellikle birkaç ilgili Azure kaynaklarını oluşur. Örneğin, bir Azure sanal makinesi bir VM, depolama hesabı, ağ bağdaştırıcısı ve genel IP adresi içerebilir. Bu kaynaklar oluşturulmuş, yönetilen ve ayrı ayrı veya bir grup olarak silindi. Azure kaynakları, bu kılavuzun devamında daha ayrıntılı ele alınmıştır.

### <a name="resource-groups"></a>Kaynak grupları

Bir Azure kaynak grubu, bir Azure çözümü için ilgili kaynakları barındıran bir kapsayıcıdır. Kaynak grubu, çözümün tüm kaynaklarını veya yalnızca bir grup olarak yönetmek istediğiniz kaynakları içerebilir. Azure kaynak gruplarını, bu kılavuzun devamında daha ayrıntılı ele alınmıştır.

### <a name="resource-manager-templates"></a>Resource Manager şablonları

Bir Azure Resource Manager şablonu bir kaynak grubuna dağıtmak için bir veya daha fazla kaynağı tanımlayan JavaScript nesne gösterimi (JSON) dosyasıdır. Ayrıca dağıtılan kaynaklar arasındaki bağımlılıkları da tanımlar. Resource Manager şablonları, bu kılavuzun devamında daha ayrıntılı ele alınmıştır.

### <a name="automation"></a>Otomasyon

Oluşturma, yönetme ve Azure portalını kullanarak silme kaynaklara ek olarak, PowerShell veya Azure komut satırı arabirimi (CLI) kullanarak bu etkinlikleri otomatik hale getirebilirsiniz.

#### <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell, Azure'ı yönetmek için cmdlet'ler sağlayan modüller kümesidir. Cmdlet'ler, oluşturmak, yönetmek ve Azure hizmetlerini kaldırmak için kullanabilirsiniz. Cmdlet'ler yardımcı olabilecek tutarlı, tekrarlanabilir ve El değmeden dağıtımlarını elde edebilirsiniz. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/install-Az-ps).

#### <a name="azure-command-line-interface"></a>Azure komut satırı arabirimi

Azure komut satırı arabirimi oluşturmak, yönetmek ve Azure kaynaklarını komut satırından kaldırmak için kullanabileceğiniz bir araçtır. Linux, Mac OS X ve Windows için Azure CLI kullanılabilir. Daha fazla bilgi ve teknik ayrıntılar için bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli).

#### <a name="rest-apis"></a>REST API'leri

Azure REST API'leri, Azure portalı kullanıcı arabirimini destekleyen bir dizi üzerinde geliştirilmiştir. Bu REST API'lerin çoğu, program aracılığıyla sağlama ve Azure kaynaklarınızın ve uygulamalarınızın Internet özellikli herhangi bir CİHAZDAN yönetme izin vermek için de desteklenir. Daha fazla bilgi için [Azure REST SDK başvurusu](https://docs.microsoft.com/rest/api/index).

### <a name="azure-cloud-shell"></a>Azure Cloud Shell

Yöneticiler, Azure PowerShell ve Azure CLI'yı Azure Cloud Shell adlı tarayıcı erişilebilir bir deneyim erişebilir. Bu etkileşimli bir arabirim, kendi seçtiğiniz, Bash veya PowerShell komut satırı arabirimi kullanmak Linux ve Windows yöneticileri için esnek bir araç sağlar. Azure Cloud Shell, bir tek başına web arabirimi portal üzerinden erişim olabilir [shell.azure.com](https://shell.azure.com), veya diğer erişim noktaları sayısı. Daha fazla bilgi için [Azure Cloud shell'e genel bakış](https://docs.microsoft.com/azure/cloud-shell/overview).

## <a name="azure-subscriptions"></a>Azure abonelikleri

Bir Azure hesabına bağlı mantıksal bir gruplandırması olan Azure hizmetlerini bir aboneliktir. Tek bir Azure hesabı, birden fazla abonelik içerebilir. Azure Hizmetleri için faturalama, abonelik başına temelinde gerçekleştirilir. Abonelik üzerinde tam denetime sahip bir Hesap Yöneticisi ve tüm hizmetleri denetime sahip abonelikte Hizmet Yöneticisi, Azure aboneliğiniz yok. Klasik abonelik yöneticileri hakkında daha fazla bilgi için bkz: [ekleme veya değiştirme Azure aboneliği yöneticileri](../../billing/billing-add-change-azure-subscription-administrator.md). Yöneticiler ek olarak, bireysel hesaplar verilebilir ayrıntılı denetim kullanarak Azure kaynaklarınızın [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md).

### <a name="select-and-enable-an-azure-subscription"></a>Seçin ve bir Azure aboneliği etkinleştir

Azure hizmetleriyle çalışmak için önce bir aboneliğinizin olması gerekir. Bazı abonelik türleri kullanılabilir.

**Ücretsiz hesaplar**: Üzerinde ücretsiz bir hesap için kaydolmak için bağlantıyı [Azure Web sitesi](https://azure.microsoft.com/). Bu kredi Azure kaynaklarının herhangi bir birleşimini denemek için 30 gün boyunca sağlar. Kredi tutarı aşarsa, hesabınız askıya alındı. Deneme sonunda hizmetlerinizi yetkisi ve artık çalışmayacak. Herhangi bir zamanda Kullandıkça Öde aboneliğine yükseltebilirsiniz.

**MSDN Abonelikleri**: Bir MSDN aboneliğiniz varsa, belirli bir miktarı her ay Azure kredisi alın. Örneğin, bir Microsoft Visual Studio Enterprise with MSDN aboneliğinizin varsa, elde \$Azure kredisi, aylık 150.

Kredi miktarı aşarsanız, hizmetiniz devre dışı bırakıldı sonraki ayın başlatana kadar. Harcama sınırınızı kapatabilir ve ek maliyetleri için kullanılacak bir kredi kartı ekleyin. Bu maliyetler bazıları için MSDN hesapları indirim uygulanır. Örneğin, Windows Server çalıştıran VM'ler için Linux fiyatı ödeme ve Microsoft SQL Server gibi Microsoft sunucuları için ek ücret yoktur. Bu hesapları MSDN Geliştirme ve test senaryoları için ideal hale getirir.

**BizSpark hesapları**: Microsoft BizSpark programı, startup'lara yönelik birçok avantaj sunar. Bu avantajlar tüm Microsoft yazılımları geliştirme ve test ortamları için en fazla beş MSDN hesapları için erişim biridir. 150 ABD Doları değerinde Azure kredisi her beş MSDN hesaplar için sahip olursunuz ve birkaç sanal makineler gibi Azure Hizmetleri için daha düşük ücretler ödersiniz.

**Kullandıkça Öde**: Bu aboneliğe kredi kartı veya banka kartı hesabına ekleyerek kullandıklarınız için ödeme yaparsınız. Bir kuruluş ise, aynı zamanda faturalama için onaylanabilir.

**Kurumsal anlaşmalar**: Bir kurumsal anlaşma ile belirli bir dizi hizmet Azure'da Önümüzdeki Yıl kullanarak kaydedin ve bu tutar önceden ödeme yaparsınız. Yaptığınız taahhüt yıl boyunca kullanılır. Taahhüt tutarı aşarsa, fazla kullanım borçlanarak ödeme yapabilirsiniz. Taahhüt miktarına bağlı olarak, Azure hizmetlerinde indirim alabilir.

### <a name="grant-administrative-access-to-an-azure-subscription"></a>Bir Azure aboneliğine yönetim erişimi verme

RBAC izinler atamak için kullanabileceğiniz birkaç yerleşik rol yok. Bir kullanıcının bir Azure aboneliğinin bir yöneticisi olmak için bunları atayın [sahibi](../../role-based-access-control/built-in-roles.md#owner) abonelik kapsamında bir rol. Sahip rolü, diğerleri erişim hakkı dahil olmak üzere, Abonelikteki tüm kaynaklara kullanıcı tam erişim sağlar.

Daha fazla bilgi için [RBAC ve Azure portalını kullanarak erişimini yönetme](../../role-based-access-control/role-assignments-portal.md).

### <a name="view-billing-information-in-the-azure-portal"></a>Azure portalında faturalama bilgileri görüntüleyin

Fatura bilgileri görüntüleme olanağı Azure kullanmanın önemli bir bileşendir. Azure portalı, Azure faturalandırma bilgileri ayrıntılı Öngörüler sağlar.

Daha fazla bilgi için [Azure faturanızı ve günlük kullanım verilerinizi indirme](../../billing/billing-download-azure-invoice-daily-usage-date.md).

### <a name="get-billing-information-from-billing-apis"></a>API'lerini faturalama, fatura bilgilerini alma

Faturalandırma portalında görüntülemenin yanı sıra, faturalandırma bilgileri bir betik veya program aracılığıyla Azure faturalandırma REST API'leri kullanarak erişebilirsiniz:

- Azure kullanım API'si, kullanım verilerinizi almak için kullanabilirsiniz. İlgili Azure kaynaklarını etiketleyerek fatura kullanım bilgilerini hassas ayarlamalar yapabilirsiniz. Örneğin, kaynakların her biri bir departman veya proje adı ile bir kaynak grubuna etiket ve ardından özellikle bir etikete ilişkin maliyetleri izleyin.

- Tüm kullanılabilir kaynaklar meta veriler ve bu kaynakların her biri hakkında bilgi fiyatlandırması ile birlikte listelemek için Azure oranı kart API'sini kullanabilirsiniz.

Daha fazla bilgi için [Microsoft Azure kaynak tüketiminize öngörü](../../billing/billing-usage-rate-card-overview.md).

### <a name="forecast-cost-with-the-pricing-calculator"></a>Fiyatlandırma hesaplayıcı ile tahmini maliyet

Her Azure hizmeti için fiyatlandırma farklılık gösterir. Çoğu Azure hizmeti temel, standart ve Premium Katmanlar sunar. Genellikle, her katmanda, çeşitli fiyat ve performans düzeyleri vardır. Kullanarak [çevrimiçi fiyatlandırma hesaplayıcı](https://azure.microsoft.com/pricing/calculator), fiyatlandırma tahminler oluşturabilirsiniz. Hesaplayıcı, tek bir kaynak veya kaynak grubunun maliyetini tahmin etmek için esneklik içerir.

## <a name="azure-resource-manager"></a>Azure Resource Manager

Azure Resource Manager, Azure kaynakları için dağıtımı, yönetimi ve kuruluş bir mekanizmadır. Kaynak Yöneticisi'ni kullanarak birçok kaynakların kaynak grubunda birlikte koyabilirsiniz.

Kaynak Yöneticisi ayrıca özelleştirilebilir dağıtımı ve yapılandırılmasıyla ilgili kaynak için izin dağıtım özellikleri içerir. Örneğin, Kaynak Yöneticisi'ni kullanarak birden çok sanal makine, yük dengeleyici ve tek bir birim olarak SQL veritabanı içeren bir uygulama dağıtabilirsiniz. Bu dağıtımlar, Resource Manager şablonu kullanarak geliştirin.

Resource Manager çeşitli avantajlar sunar:

- Çözümünüzdeki tüm kaynakları ayrı ayrı ele almak yerine bunları grup halinde dağıtabilir, yönetebilir ve izleyebilirsiniz.

- Art arda çözümünüzü geliştirme yaşam döngüsü boyunca dağıtabilir ve kaynaklarınızın tutarlı bir durumda dağıtıldığından emin olabilirsiniz.

- Altyapınızı betikler yerine bildirim temelli şablonları kullanarak yönetebilirsiniz.

- Doğru sırayla dağıtılmalarını sağlamak için kaynaklarınız arasındaki bağımlılıkları tanımlayabilirsiniz.

- RBAC yönetim platformu ile yerel olarak tümleşik olduğundan, kaynak grubunuzdaki tüm hizmetlere erişim denetimi uygulayabilirsiniz.

- Üzerinde aboneliğinizdeki tüm kaynakları mantıksal olarak düzenlemek için kaynaklarınıza etiketler ekleyebilirsiniz.

- Aynı etiketi paylaşan bir kaynak grubunun maliyetlerini görüntüleyerek kuruluşunuzun faturalarına açıklık getirebilirsiniz.

### <a name="tips-for-creating-resource-groups"></a>Kaynak grupları oluşturmaya ilişkin ipuçları

Kaynak grupları hakkında bir karar almadan bu ipuçlarını dikkate alın:

- Bir kaynak grubundaki tüm kaynaklar aynı yaşam döngüsüne sahip olmalıdır.

- Bir kerede yalnızca bir grup için bir kaynak atayabilirsiniz.

- Ekleyebilir veya bir kaynak, herhangi bir zamanda bir kaynak grubundan kaldırın. Her kaynak bir kaynak grubuna ait olmalıdır. Bir gruptan bir kaynağı kaldırırsanız, bu nedenle onu başka eklemeniz gerekir.

- Kaynakları birçok türde herhangi bir zamanda farklı kaynak grubuna taşıyabilirsiniz.

- Bir kaynak grubundaki kaynaklar, farklı bölgelerde olabilir.

- Bir kaynak grubu içindeki kaynaklar için erişimi denetlemek için kullanabilirsiniz.

### <a name="building-resource-manager-templates"></a>Resource Manager şablonları oluşturma

Resource Manager şablonları, bir tek bir kaynak grubuna dağıtılacak kaynak yapılandırmaları ve kaynakları bildirimli olarak tanımlayın. Resource Manager şablonları karmaşık dağıtımları fazla komut dosyası çalıştırma veya el ile yapılandırma gerek kalmadan yönetmek için kullanabilirsiniz. Şablon geliştirme sonra birden çok kez dağıtabilirsiniz; her zaman aynı sonucu.

Resource Manager şablonu dört bölümden oluşur:

- **Parametreleri**: Bu dağıtım için girişleri. Parametre değerlerini bir insan veya otomatik bir işlem tarafından sağlanabilir. Bir örnek parametresi, bir yönetici kullanıcı adı ve parolayı bir Windows VM için olabilir. Parametre değerlerini belirtilmiş dağıtım kullanılır.

- **Değişkenleri**: Bunlar dağıtım kullanılan değerleri tutmak için kullanılır. Parametreleri, dağıtım sırasında bir değişken değeri sağlanmadı. Bunun yerine, kodlanmış veya dinamik olarak oluşturulan zordur.

- **Kaynaklar**: Şablonu'nun bu bölümünde, sanal makineler, depolama hesapları ve sanal ağlar gibi dağıtılması için kaynakları tanımlar.

- **Çıkış**: Bir dağıtım tamamlandıktan sonra Resource Manager dinamik olarak üretilen bağlantı dizeleri gibi veri döndürebilir.

Dağıtım Otomasyon için şu mekanizmaları kullanılabilir:

- **İşlevler**: Resource Manager şablonlarında çeşitli işlevler kullanabilirsiniz. Bunlar, bir dizeyi küçük harfe dönüştürme gibi işlemler birden çok örneğini tanımlı kaynak dağıtma ve dinamik olarak hedef kaynak grubu döndüren içerir. Resource Manager işlevleri dinamik dağıtımların yapı yardımcı olur.

- **Kaynak bağımlılıkları**: Birden çok kaynak dağıtıyorsanız, bazı kaynaklar bazılarında bir bağımlılık olacaktır. Dağıtım kolaylaştırmak için önce diğer bağımlı kaynaklar dağıtılan böylece bir bağımlılık bildirimi kullanabilirsiniz.

- **Şablon Bağlama**: Bir Resource Manager şablonu içinde başka bir şablona bağlayabilirsiniz. Bu, hedeflenen, amaca şablonları kümesine dağıtım ayrıştırma sağlar.

Resource Manager şablonları herhangi bir metin düzenleyicisinde oluşturabilirsiniz. Ancak, Visual Studio için Azure SDK'sı yardımcı olacak araçlar içerir. Visual Studio kullanarak, bir sihirbaz üzerinden şablonu kaynak eklemek sonra dağıtabilir ve şablondan doğrudan Visual Studio'dan hata ayıklama. Daha fazla bilgi için [Azure Resource Manager şablonları yazma](../../resource-group-authoring-templates.md).

Son olarak, Azure portalından yeniden kullanılabilir şablon mevcut kaynak gruplarını dönüştürebilirsiniz. Mevcut bir kaynak grubu dağıtılabilir şablonunu oluşturmak istediğiniz veya yalnızca temel alınan JSON incelemek isterseniz, bu yararlı olabilir. Bir kaynak grubunu dışarı aktarma için seçin **Otomasyon betiği** kaynak grubunun ayarlar düğmesinden.

## <a name="security-of-azure-resources-rbac"></a>(RBAC) Azure kaynaklarınızın güvenliğini

Kullanıcı hesaplarını belirli bir kapsamda işletimsel erişimi verebilir: Abonelik, kaynak grubu veya tek başına bir kaynak. Bu, kaynakları bir sanal makine gibi bir kaynak grubuna ve tüm ilgili kaynakları kümesi dağıtma ve belirli kullanıcı veya grup için izinler anlamına gelir. Bu yaklaşım hedef kaynak grubuna ait kaynaklara erişimi sınırlar. Ayrıca, bir sanal makine veya sanal ağ gibi tek bir kaynağa erişim izni verebilirsiniz.

Erişim vermek için kullanıcı veya kullanıcı grubunun bir rol atayın. Birçok önceden tanımlı roller bulunur. Ayrıca, kendi özel rollerinizi de tanımlayabilirsiniz.

İşte birkaç örnek [azure'da yerleşik roller](../../role-based-access-control/built-in-roles.md):

- **Sahibi**: Bu role sahip bir kullanıcı erişim dahil her şeyi yönetebilir.

- **Okuyucu**: Bu role sahip bir kullanıcı (gizli anahtarlar) hariç tüm türlerdeki kaynakların okuyabilir, ancak değişiklik yapamaz.

- **Sanal makine Katılımcısı**: Bu role sahip bir kullanıcı sanal makineleri yönetebilir, ancak sanal ağa yönetemez, bağlı oldukları veya VHD dosyasının bulunduğu depolama hesabı.

- **SQL DB Katılımcısı**: Bu role sahip bir kullanıcı, SQL veritabanları, ancak değil güvenlikle ilgili ilkelerini yönetebilirsiniz.

- **SQL Güvenlik Yöneticisi**: Bu role sahip bir kullanıcı SQL sunucularının ve veritabanlarının güvenlikle ilgili ilkelerini yönetebilirsiniz.

- **Depolama hesabı Katılımcısı**: Bu role sahip bir kullanıcı, depolama hesaplarını yönetebilir, ancak depolama hesaplarına erişim yönetemez.

Daha fazla bilgi için [RBAC ve Azure portalını kullanarak erişimini yönetme](../../role-based-access-control/role-assignments-portal.md).

## <a name="azure-virtual-machines"></a>Azure Sanal Makineler

Azure sanal makineleri, azure'da Orta Iaas Hizmetleri biridir. Azure sanal makineler, bir Microsoft Azure veri merkezinde Windows veya Linux sanal makine dağıtımını destekler. Azure sanal makineler ile VM yapılandırması üzerinde tam denetim sahibi olması ve tüm yazılım yükleme, yapılandırma ve bakım için sorumludur.

Bir Azure VM dağıtırken, görüntü Azure Market'ten seçebilir veya, kendi genelleştirilmiş görüntü sağlayabilirsiniz. Bu görüntü, işletim sistemi ve başlangıç yapılandırmasını uygulamak için kullanılır. Dağıtım sırasında Resource Manager bilgisayar adı, yönetici kimlik bilgilerine ve ağ yapılandırması atama gibi bazı yapılandırma ayarları işler. Daha fazla yazılım yükleme gibi yapılandırmaları, virüsten koruma yapılandırma ve izleme çözümleri otomatik hale getirmek için Azure sanal makine Uzantıları'nı kullanabilirsiniz.

Çok sayıda farklı boyutlardaki sanal makineler oluşturabilirsiniz. Kaynak ayırma işlemi, bellek ve depolama kapasitesi gibi sanal makine boyutunu belirler. Bazı durumlarda, RDMA özellikli ağ bağdaştırıcıları ve SSD diskleri gibi belirli özellikleri yalnızca belirli VM boyutları ile kullanılabilir. "Azure'da sanal makine boyutları" VM boyutlarına ve özelliklerine tam listesi için bkz [Windows](../../virtual-machines/windows/sizes.md) ve [Linux](../../virtual-machines/linux/sizes.md).

### <a name="use-cases"></a>Uygulama alanları

Azure sanal makine yapılandırması üzerinde tam denetim sunduklarından, çok çeşitli PaaS modele uymayan sunucu iş yükleri için idealdir. Sunucu iş yükleri gibi veritabanı sunucuları (SQL Server, Oracle veya MongoDB), Windows Server Active Directory, Microsoft SharePoint ve çok daha fazla Microsoft Azure platformunda çalıştırmak mümkün olur. İsterseniz, bir şirket içi veri merkezlerinden gibi iş yükleri için bir veya daha fazla Azure bölgesi ile büyük miktarda yeniden yapılandırma olmadan taşıyabilirsiniz.

### <a name="deployment-of-virtual-machines"></a>Sanal makinelerin dağıtımı

Azure portalını kullanarak, Azure PowerShell modülü ile Otomasyon kullanarak veya Otomasyon ile platformlar arası CLI'yı kullanarak Azure sanal makineleri dağıtabilirsiniz.

#### <a name="portal"></a>Portal

Azure portalını kullanarak bir sanal makine dağıtma, yalnızca bir etkin Azure aboneliği ve bir web tarayıcısına erişimi gerektirir. Birçok farklı işletim sistemi görüntüleri değişkenlik gösteren yapılandırmaları ile seçebilirsiniz. Tüm depolama ve ağ gereksinimleri dağıtım sırasında yapılandırılır. Görmek için daha fazla bilgi için "Azure portalında sanal makine oluşturma" [Windows](../../virtual-machines/windows/quick-create-portal.md) ve [Linux](../../virtual-machines/linux/quick-create-portal.md).

Azure portalından bir sanal makine dağıtımına ek olarak, portalda bir Azure Resource Manager şablonu dağıtabilirsiniz. Bu, dağıtın ve tüm kaynaklar şablonda tanımlanan şekilde yapılandırın. Daha fazla bilgi için [kaynakları Resource Manager şablonları ve Azure portalı ile dağıtma](../../azure-resource-manager/resource-group-template-deploy-portal.md).

#### <a name="powershell"></a>PowerShell

PowerShell kullanarak bir Azure sanal makine dağıtımı için ağ ve depolama dahil olmak üzere tüm ilgili sanal makine kaynaklarının tam dağıtım otomasyonunu sağlar. Daha fazla bilgi için [Resource Manager ve PowerShell kullanarak bir Windows VM oluşturma](../../virtual-machines/windows/quick-create-powershell.md).

Azure işlem kaynaklarını ayrı ayrı dağıtmanın yanı sıra Azure Resource Manager şablonu dağıtmak için Azure PowerShell modülünü kullanabilirsiniz. Daha fazla bilgi için [kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/resource-group-template-deploy.md).

#### <a name="command-line-interface-cli"></a>Komut satırı arabirimi (CLI)

PowerShell modülüyle olduğu gibi Azure komut satırı arabirimi dağıtım otomasyonunu sağlar ve Windows, OS X veya Linux sistemlerinde kullanılabilir. Azure CLI'yı kullanırken **vm hızlı oluşturma** komut, tüm ilgili sanal makine kaynakları (depolama da dahil olmak üzere ve ağ) ve sanal makine dağıtılır. Daha fazla bilgi için [CLI'yi kullanarak Azure'da bir Linux VM oluşturma](../../virtual-machines/linux/quick-create-cli.md).

Benzer şekilde, bir Azure Resource Manager şablonu dağıtmak için Azure CLI'yı kullanabilirsiniz. Daha fazla bilgi için [kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../../azure-resource-manager/resource-group-template-deploy-cli.md).

### <a name="access-and-security-for-virtual-machines"></a>Erişim ve sanal makineler için güvenlik

Bir sanal makineyi Internet'ten erişmeyi gerektirir ilişkili ağ arabirimi ya da yük dengeleyici varsa bir genel IP adresiyle yapılandırılması. Genel IP adresini sanal makineye çözmek veya yük dengeleyici DNS adı içerir. Daha fazla bilgi için [azure'da IP adresleri](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).

Bir ağ güvenlik grubu (NSG) kaynak kullanarak genel IP adresi sanal makineye erişimi yönetin. Bir NSG bir güvenlik duvarı gibi davranır ve izin verir veya ağ arabirimiyle ya da bir dizi tanımlanmış bağlantı noktalarına alt ağ arasında trafiği engeller. Örneğin, bir Azure VM ile Uzak Masaüstü oturumu oluşturmak için 3389 numaralı bağlantı noktasında gelen trafiğe izin veren NSG yapılandırmanız gerekir. Daha fazla bilgi için [Azure portalını kullanarak Azure'da bağlantı noktalarını VM'ye açma](../../virtual-machines/windows/nsg-quickstart-portal.md).

Son olarak, gibi Yönetimi herhangi bir bilgisayar sisteminin, size güvenlik bir Azure sanal makine işletim sistemi için güvenlik kimlik bilgileri ve yazılım güvenlik duvarları kullanarak sağlamanız gerekir.

## <a name="azure-storage"></a>Azure Storage

Azure depolama, dayanıklı, ölçeklenebilir ve yedekli depolama sağlayan bir Microsoft tarafından yönetilen bir hizmettir. Bir Azure depolama hesabı herhangi bir kaynak dağıtım yöntemi kullanarak herhangi bir kaynak grubuna bir kaynak olarak ekleyebilirsiniz. Azure, dört depolama türlerini içerir: BLOB Depolama, dosya depolama, tablo depolama ve kuyruk depolama. Bir depolama hesabını dağıtırken, iki hesap türleri kullanılabilir, genel amaçlı ve blob depolama alanı. Genel amaçlı depolama hesabı için dört tüm depolama türlerinde erişmenizi sağlar. BLOB Depolama hesapları, genel amaçlı hesaplar için benzerdir, ancak içeren sıcak ve soğuk erişim katmanları içeren özelleştirilmiş blobları. Blob depolama hakkında daha fazla bilgi için bkz. [Azure Blob Depolama](../../storage/blobs/storage-blob-storage-tiers.md).

Azure depolama hesapları farklı düzeylerde yedeklilik ile yapılandırılabilir:

- **Yerel olarak yedekli depolama** yazma başarılı olarak kabul önce tüm verilerin üç kopyasını zaman uyumlu olarak yapılmasını sağlayarak yüksek düzeyde kullanılabilirlik sağlar. Bu kopyalar, tek bir yerde tek bir bölgede depolanır. Çoğaltmalar ayrı hata etki alanlarında bulunan ve yükseltme etki alanları. Bu, bir depolama düğümü, veri başarısız bulunduran veya güncelleştirilmesi için çevrimdışına alındığında bile verileri kullanılabilir anlamına gelir.

- **Coğrafi olarak yedekli depolama** yüksek kullanılabilirlik için bir birincil bölgede verilerin üç zaman uyumlu kopyasını getirir ve sonra zaman uyumsuz olarak üç kopyaya olağanüstü durum kurtarma için eşleştirilmiş bir bölge yapar.

- **Okuma erişimli coğrafi olarak yedekli depolama** coğrafi olarak yedekli depolama artı ikincil bölgedeki veri okuma yeteneği. Bu özelliği kısmi olağanüstü durum kurtarma için uygun hale getirir. Birincil bölge ile ilgili bir sorun varsa, uygulamanız için eşleştirilmiş bölge salt okunur erişim sağlamak için değiştirebilirsiniz.

### <a name="use-cases"></a>Uygulama alanları

Her depolama türü farklı kullanım örneği vardır.

#### <a name="blob-storage"></a>Blob depolama

Word *blob* kısaltması olduğundan *ikili büyük nesne*. Bloblar, bilgisayarınızda depolayıp olanlar gibi yapılandırılmamış dosyalarıdır. Blob Storage belge, medya dosyası veya uygulama yükleyici gibi her tür metin veya ikili veri depolayabilir. Blob Storage aynı zamanda nesne depolama olarak adlandırılır. Azure Blob Depolama, Azure sanal makineler veri diskleri de içerir.

Azure depolama üç BLOB türünü destekler:

- **Blok blobları** tutmak için kullanılan sıradan dosyaları 195 GB cinsinden boyutu (4 MB x 50.000 blok). Blok blobları için birincil kullanım durumu, başından sonuna medya dosyalarını veya Web siteleri için görüntü dosyaları gibi okunur dosyaların depolamadır. Küçük bloklar olarak 64 MB'tan büyük dosyaları karşıya yüklenmelidir çünkü bunlar blok blobları olarak adlandırılır. Bu blokları ardından birleştirilmiş (kaydedilen son blob veya).

- **Sayfa blobları** tutmak için kullanılan rastgele erişim, boyutu 1 TB'ye kadar dosyaları. Sayfa blobları öncelikli olarak yedekleme depolama alanı olarak Azure sanal makineler için kalıcı diskler sağlayan VHD'ler için kullanılan, Iaas Azure hizmetinde işlem. 512 baytlık sayfalarına rastgele okuma/yazma erişimi sağlarlar çünkü bunlar sayfa blobları olarak adlandırılır.

- **Ekleme blobları** oluşur bloklarını beğendiniz blok blobları, ancak için iyileştirilmiş ekleme işlemleri. Bunlar aynı blob için bir veya daha fazla kaynaktan alınan günlük bilgileri için sık kullanılır. Örneğin, birden çok VM'de çalışan bir uygulama için aynı ekleme blobu için izleme günlüğü tümünün yazabilirsiniz. Tek bir ekleme blobu 195 GB olabilir.

Daha fazla bilgi için [.NET kullanarak Azure Blob depolamayı kullanmaya başlama](../../storage/blobs/storage-dotnet-how-to-use-blobs.md).

#### <a name="file-storage"></a>Dosya depolama

Azure dosya depolama standart sunucu ileti bloğu (SMB) protokolünü kullanarak bulutta dosya paylaşımları sağlayan bir hizmettir. Hizmet, SMB 2.1 ve SMB 3.0 destekler. Azure dosya depolama ile hızlı ve pahalı yeniden yazmalar olmadan Azure dosya paylaşımları kullanan uygulamaları geçirebilirsiniz. Azure sanal makineler üzerinde çalışan uygulamalar, bulut Hizmetlerinde veya şirket içi istemcileri buluta bir dosya paylaşımı bağlayabilir. Bu, bir masaüstü uygulamanın tipik bir SMB paylaşımına bağlandığı nasıl ile için benzer. Ardından herhangi sayıda uygulama bileşeni eş zamanlı olarak File Storage paylaşımını bağlayıp buna erişim sağlayabilir.

File storage paylaşımı standart SMB dosya paylaşımı olduğundan Azure'da çalışan uygulamalar dosya sistemi g/ç API'leri üzerinden paylaşımdaki verilere erişebilir. Geliştiriciler bu nedenle, mevcut uygulamalarını taşımak üzere kullandıkları kodlar ve becerileri kullanabilirsiniz. BT uzmanları, oluşturmak, bağlamak ve Azure uygulamalarının Yönetimi kapsamında dosya depolama paylaşımlarını yönetmek için PowerShell cmdlet'lerini kullanabilirsiniz.

Daha fazla bilgi için [Windows üzerinde Azure dosya depolama ile çalışmaya başlama](../../storage/files/storage-how-to-use-files-windows.md) veya [Azure dosya depolamayı Linux ile kullanma konusunda](../../storage/files/storage-how-to-use-files-linux.md).

#### <a name="table-storage"></a>Table Storage

Azure Table Storage, bulutta yapılandırılmış NoSQL verileri depolayan bir hizmettir. Tablo depolama, şemasız tasarım ile anahtar/öznitelik deposudur. Tablo depolama şemasız olduğu için ihtiyaçları, uygulama geliştikçe verilerinizi uyarlamak da kolaylaşır. Her türlü uygulama için verilere erişim hızlı ve uygun maliyetlidir. Table Storage, benzer hacimdeki veriler için geleneksel SQL’e oranla çok daha düşük maliyetlidir.

Web uygulamaları için kullanıcı verileri, adres defterleri, cihaz bilgileri ve hizmetiniz için gerekli olan tüm diğer meta veri türleri gibi esnek veri kümelerini depolamak üzere Table Storage’ı kullanabilirsiniz. Bir tablodaki herhangi bir sayıda varlık depolayabilirsiniz. Bir depolama hesabı herhangi bir sayıda depolama hesabının kapasite sınırına kadar tablo içerebilir.

Daha fazla bilgi için [Azure tablo depolama ile çalışmaya başlama](../../cosmos-db/table-storage-how-to-use-dotnet.md).

#### <a name="queue-storage"></a>Kuyruk depolama

Azure Queue depolama birimi, uygulama bileşenleri arasında bulut mesajlaşma özelliği sağlar. Böylece ölçeklendirilebilmeleri ölçek için uygulamaları tasarlarken, uygulama bileşenleri genellikle birbirinden ayrılır. Kuyruk depolama bulutta, masaüstünde, şirket içi sunucuda veya mobil bir cihazda çalışan uygulama bileşenleri arasındaki iletişim için zaman uyumsuz mesajlaşma sunar. Kuyruk depolama zaman uyumsuz görevlerin yönetilmesini ve süreç iş akışlarının oluşturulmasını da destekler.

Daha fazla bilgi için [Azure kuyruk depolama ile çalışmaya başlama](../../storage/queues/storage-dotnet-how-to-use-queues.md).

### <a name="deploying-a-storage-account"></a>Bir depolama hesabı'nı dağıtma

Bir depolama hesabı dağıtmak için birkaç seçenek vardır.

#### <a name="portal"></a>Portal

Azure portalını kullanarak bir depolama hesabı dağıtmak, yalnızca bir etkin Azure aboneliği ve bir web tarayıcısına erişimi gerekir. Yeni bir depolama hesabı, yeni veya mevcut bir kaynak grubuna dağıtabilirsiniz. Depolama hesabı oluşturduktan sonra portalı kullanarak blob kapsayıcı veya dosya paylaşımı oluşturabilirsiniz. Tablo oluşturma ve depolama varlıkları program aracılığıyla kuyruk. Daha fazla bilgi için bkz. [Depolama hesabı oluşturma](../../storage/common/storage-quickstart-create-account.md).

Azure portalında bir depolama hesabından dağıtımına ek olarak, portalda bir Azure Resource Manager şablonu dağıtabilirsiniz. Bu, dağıtmak ve tüm kaynakların herhangi bir depolama hesabı dahil olmak üzere şablonda tanımlanan şekilde yapılandırın. Daha fazla bilgi için [kaynakları Resource Manager şablonları ve Azure portalı ile dağıtma](../../azure-resource-manager/resource-group-template-deploy-portal.md).

#### <a name="powershell"></a>PowerShell

PowerShell kullanarak bir Azure depolama hesabı dağıtmak için depolama hesabının Tam dağıtım otomasyonunu sağlar. Daha fazla bilgi için [Azure PowerShell kullanarak Azure depolama ile](../../storage/common/storage-powershell-guide-full.md).

Azure kaynaklarını tek tek dağıtmanın yanı sıra Azure Resource Manager şablonu dağıtmak için Azure PowerShell modülünü kullanabilirsiniz. Daha fazla bilgi için [kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/resource-group-template-deploy.md).

#### <a name="command-line-interface-cli"></a>Komut satırı arabirimi (CLI)

PowerShell modülüyle olduğu gibi Azure komut satırı arabirimi dağıtım otomasyonunu sağlar ve Windows, OS X veya Linux sistemlerinde kullanılabilir. Azure CLI'yı kullanabilirsiniz **depolama hesabı oluşturma** bir depolama hesabı oluşturmak için komutu. Daha fazla bilgi için [Azure depolama ile Azure CLI kullanma.](../../storage/common/storage-azure-cli.md)

Benzer şekilde, bir Azure Resource Manager şablonu dağıtmak için Azure CLI'yı kullanabilirsiniz. Daha fazla bilgi için [kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../../resource-group-template-deploy-cli.md).

### <a name="access-and-security-for-azure-storage"></a>Erişim ve güvenlik için Azure depolama

Azure depolama, ancak Azure portalındaki VM oluşturma ve işlem sırasında ve depolama istemci kitaplıkları dahil olmak üzere çeşitli yollarla erişilir.

#### <a name="virtual-machine-disks"></a>Sanal makine diskleri

Bir sanal makine dağıtıyorsanız, ayrıca sanal makine işletim sistemi diski ve herhangi bir ek veri diskleri tutmak için bir depolama hesabı oluşturmanız gerekir. Mevcut bir depolama hesabını seçin veya yeni bir tane oluşturun. Bir blob en büyük boyutunu 1.024 GB olduğundan, tek bir sanal makine diskini 1,023 GB'lık boyut sınırı vardır. Büyük veri diski yapılandırmak için birden çok veri diski sanal makineye sunmak ve bunları birlikte tek bir mantıksal disk havuzu. Daha fazla bilgi için "Manage Azure diskler" için [Windows](../../virtual-machines/windows/tutorial-manage-data-disk.md) ve [Linux](../../virtual-machines/linux/tutorial-manage-disks.md).

#### <a name="storage-tools"></a>Depolama araçları

Azure depolama hesapları, Visual Studio Cloud Explorer gibi birçok farklı depolama gezginleri aracılığıyla erişilebilir. Bu araçlar veri ve depolama hesapları ile göz atmanıza olanak tanır. Daha fazla bilgi ve kullanılabilir depolama gezginleri listesi için bkz: [Azure depolama istemci araçları](../../storage/common/storage-explorers.md).

#### <a name="storage-api"></a>Depolama API'si

Depolama kaynaklarını, HTTP/HTTPS istekleri yapabilen herhangi bir dil tarafından erişilebilir. Ayrıca, Azure Storage birkaç popüler dilde programlama kitaplıkları sunar. Bu kitaplıklar Azure Storage ile çalışma işleme ayrıntıları zaman uyumlu ve zaman uyumsuz çağrılar, işlemleri, özel durum yönetimi ve otomatik yeniden denemeler toplu işleme basitleştirir. Daha fazla bilgi için [Azure depolama hizmeti REST API Başvurusu](/rest/api/storageservices/Azure-Storage-Services-REST-API-Reference).

#### <a name="storage-access-keys"></a>Depolama erişim anahtarlarını

Her Depolama hesabı, iki kimlik doğrulaması anahtarları, birincil ve ikincil sahiptir. Ya da depolama erişim işlemleri için kullanılabilir. Bu depolama anahtarları, depolama hesabını güvenli hale getirmek için kullanılır ve verilere program aracılığıyla erişmek için gereklidir. Güvenliği artırmak için anahtarların zaman geçiş izin vermek için iki anahtar vardır. Hesap adı ile birlikte kendi mülkü depolama hesabındaki tüm verilere sınırsız erişim sağladığından anahtarların güvenliğini sağlamak için önemlidir.

#### <a name="shared-access-signatures"></a>Paylaşılan erişim imzaları

Kullanıcıların depolama kaynaklarınıza erişim denetimine izin vermeniz gerekiyorsa, paylaşılan erişim imzası oluşturabilirsiniz. Paylaşılan erişim imzası bir depolama kaynağına yetkilendirilmiş erişim sağlayan URL'ye eklenebilen bir belirteçtir. Belirtece sahip olan herkes onun geçerli süre için bu izinlere sahip, belirttiğinden, işaret eden kaynaklara erişebilir. Daha fazla bilgi için [paylaşılan erişim imzaları kullanma](../../storage/common/storage-dotnet-shared-access-signature-part-1.md).

## <a name="azure-virtual-network"></a>Azure Sanal Ağ

Sanal ağlar, sanal makineler arasındaki iletişimi desteklemek gereklidir. Alt ağlar, özel IP adresi, DNS ayarlarını, güvenlik filtresini tanımlayın ve Yük Dengeleme. Azure, farklı kullanım durumlarını destekler: sadece bulutta yer alan ağları veya karma sanal ağları.

### <a name="cloud-only-virtual-networks"></a>Yalnızca bulutta yer alan sanal ağlar

Varsayılan olarak, bir Azure sanal ağı, yalnızca Azure'da depolanan kaynakları erişilebilir. Aynı sanal ağa bağlı kaynakların birbiriyle iletişim kurabilir. Yük Dengeleyiciler ile sanal makineye Internet üzerinden erişilebilir hale getirmek için genel bir IP adresi ve sanal makine ağ arabirimleri ilişkilendirin. Bir ağ güvenlik grubu kullanarak herkese kaynaklarına güvenli erişim yardımcı olabilir.

![2 katmanlı Web uygulaması için Azure sanal ağı](https://docs.microsoft.com/azure/load-balancer/media/load-balancer-internal-overview/ic744147.png)

### <a name="hybrid-virtual-networks"></a>Karma sanal ağlar

ExpressRoute veya siteden siteye VPN bağlantısı kullanarak bir Azure sanal ağı şirket içi ağa bağlanabilir. Bu yapılandırmada, Azure sanal ağı aslında bir bulut tabanlı, şirket içi ağınızın uzantısıdır.
![VPN kullanarak karma sanal ağ](https://docs.microsoft.com/azure/architecture/reference-architectures/_images/blueprints/hybrid-network-vpn.png)

Azure sanal ağı şirket içi ağınıza bağlı olduğundan, sanal ağlar benzersiz bir bölümü kuruluşunuzda kullanılan adres alanının kullanmalısınız şirketler arası. Ağınızı genişletmek gibi belirli bir IP alt ağı farklı Kurumsal konumda atanan aynı şekilde, Azure başka bir konum haline gelir.
Bir sanal ağı dağıtmak için birkaç seçenek vardır.

- [Portal](../..//virtual-network/quick-create-portal.md)

- [PowerShell](../../virtual-network/quick-create-powershell.md)

- [Komut satırı arabirimi (CLI)](../../virtual-network/quick-create-cli.md)

- Azure Resource Manager şablonları

> **Ne zaman kullanılacağı**: Azure'da sanal makineler çalışırken kullandığınız zaman, sanal ağlarla çalışır. Bu, genel kullanıma yönelik ve özel alt ağlar benzer şirket içi veri merkezleri içinde Vm'lerinizi kesimlere için sağlar.
> 
> **Başlama**: Azure portalını kullanarak bir Azure sanal ağı dağıtmak için yalnızca bir etkin Azure aboneliği ve bir web tarayıcısına erişimi gerektirir. Yeni veya mevcut bir kaynak grubuna yeni bir sanal ağa dağıtabilirsiniz. Portaldan yeni bir sanal makine oluştururken, mevcut bir sanal ağ seçin veya yeni bir tane oluşturun. Kullanmaya başlayın ve [Azure portalını kullanarak bir sanal ağ oluşturma](../../virtual-network/quick-create-portal.md).

### <a name="access-and-security-for-virtual-networks"></a>Erişim ve sanal ağlar için güvenlik

Bir ağ güvenlik grubu kullanarak güvenli bir Azure sanal ağları yardımcı olabilir. Nsg'ler izin veren veya reddeden ağ trafiğini bir sanal ağdaki sanal makine örnekleriniz için erişim denetimi listesi (ACL) kurallarının bir listesini içerir. Nsg'ler alt ağlarla veya bu alt ağ içindeki tekil VM örnekleriyle ilişkilendirebilirsiniz. Bir NSG'yi bir alt ağ ile ilişkilendirdiğinizde, ACL kuralları bu alt ağdaki tüm VM örnekleri için geçerlidir. Ayrıca, daha fazla trafik tek bir VM ile bir NSG doğrudan bu VM ile ilişkilendirilmesi yoluyla sınırlayabilirsiniz. Daha fazla bilgi için [ağ güvenlik grupları ile ağ trafiğini filtreleme](../../virtual-network/security-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Bir Windows VM oluşturma](../../virtual-machines/windows/quick-create-portal.md)
- [Linux VM oluşturma](../../virtual-machines/linux/quick-create-portal.md)
