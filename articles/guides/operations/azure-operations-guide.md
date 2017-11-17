---
title: "Azure BT işleçler kılavuzuna Başlarken | Microsoft Docs"
description: "Başlarken Kılavuzu için Azure BT işleçleri alma"
services: 
documentationcenter: 
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.devlang: 
ms.topic: 
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 06/12/2017
ms.author: nepeters
ms.openlocfilehash: 1180001c9fe74aab6b51c5b5969b80a8c7e1302f
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="introduction-to-cloud-computing-and-microsoft-azure"></a>Bulut için giriş ve Microsoft Azure

Bu kılavuz bir Microsoft Azure altyapısının yönetim ve dağıtım için ilgili temel kavramları tanıtır. Yeni ise bilgi işlem ya da Azure bulut kendisi, bu kılavuzda hızla kavramları, dağıtım ve yönetim ayrıntıları başlamanıza yardımcı yardımcı olur. Bu kılavuzun birçok bölüm bir sanal makine dağıtma gibi bir işlem ele almaktadır ve sonra bağlantı için ayrıntılı teknik ayrıntıları sağlayın.


## <a name="cloud-computing-overview"></a>Bulut, genel bakış

Geleneksel şirket içi veri merkezine modern bir alternatif sağlayan bulut. Genel bulut satıcılar sağlar ve tüm bilgi işlem altyapısı ve temel yönetim yazılımı yönetir. Bu satıcılardan çok çeşitli bulut hizmetleri sağlar. Bir bulut hizmeti, bir sanal makine, bir web sunucusu veya bulutta barındırılan veritabanı altyapısı bu durumda olabilir. Bir bulut sağlayıcısı müşteri olarak gerektiği düzenli olarak bu bulut Hizmetleri kira. Bunu yaparken, donanım bakım sermaye masrafı bir işlemsel gider dönüştürün. Bir bulut hizmeti de şu avantajları sağlar:

-   Büyük işlem ortamlarının hızlı dağıtımı

-   Artık gerekli olmayan sistemleri hızlı ayırmayı kaldırma

-   Geleneksel olarak karmaşık sistemlerinin kolay dağıtım ister yük dengeleyici

-   Esnek sağlayabilme işlem kapasitesi veya gerektiğinde ölçek

-   Daha düşük maliyetli bilgi işlem ortamları

-   Herhangi bir web tabanlı portal veya programlama otomasyon ile erişebilir

-   Çoğu işlem ve uygulama gereksinimlerini karşılamak üzere bulut tabanlı hizmetler

Şirket içi altyapı ile dağıtılan yazılım ve donanım üzerinde tam denetime sahiptir. Tarihsel olarak, bu donanım satın alma kararları odaklanan ölçeklendirmeyi üzerinde açmıştır. Bir örnek, bir sunucu en yüksek performans gereksinimlerini karşılamak üzere daha fazla sayıda çekirdek ile satın alma. Ne yazık ki, bu altyapı talep penceresi dışında gereğinden az. Azure ile gereken altyapı dağıtmak ve bu yukarı veya aşağı herhangi bir zamanda ayarlayın. Bu bir odağı aracılığıyla ek işlem düğümleri dağıtımın ölçeğini üzerinde bir performans gereksinimini karşılamak için yol gösterir. Bulut hizmetlerini ölçeklendirme pahalı donanım ölçeklendirmeyi daha düşük maliyetli.

Microsoft, daha fazla ile planlı dünya, çok sayıda Azure veri merkezleri dağıtmış olan. Ayrıca, Microsoft Çin ve Almanya gibi bölgelerdeki sovereign Bulutlar artmaktadır. Azure kullanarak, kuruluşların müşterilerine yakın hizmetlerini dağıtmak için herhangi bir boyuttaki kolaylaştırır için yalnızca en büyük genel kuruluşların bu şekilde veri merkezlerinde dağıtabilirsiniz.

Küçük işletmeler için Azure düşük maliyetli giriş noktası için işlem artışları için isteğe bağlı olarak hızlı bir şekilde ölçeklendirme olanağı sağlar. Bu büyük bir eylemli sermaye yatırımı altyapısında engeller ve mimari ve sistemler gerektiği gibi yeniden düzenlenmesine esnekliği sağlar. İyi modeliyle ölçek hızlı ve başarısız-hızlı başlangıç İyileştirilecek sığar bulut kullanın.

Kullanılabilir Azure bölgeleri hakkında daha fazla bilgi için bkz: [Azure bölgeleri](https://azure.microsoft.com/regions/).

### <a name="cloud-computing-is-classified-into-three-categories-saas-paas-and-iaas"></a>Sınıflandırılmış üç kategoride bulut: SaaS, PaaS ve Iaas.

#### <a name="saas-software-as-a-service"></a>SaaS: Hizmet olarak yazılım

SaaS merkezi olarak barındırılan ve yönetilen bir yazılımdır. Genellikle çok müşterili mimarisi temel — tek bir uygulama sürümünü tüm müşteriler için kullanılır. Bunu tüm konumlarda en iyi performansı elde etmek için birden çok örneği için genişletilebilir. SaaS yazılım genellikle bir aylık veya yıllık abonelikle lisanslanmıştır.

Microsoft Office 365 sunan bir SaaS iyi bir örnektir. Aboneleri aylık veya yıllık abonelik ücret ödemeniz ve Microsoft Exchange, Microsoft OneDrive ve bir hizmet olarak Microsoft Office suite kalan alın. Abonelerin her zaman en son sürümünü almak ve Exchange server sizin için yönetilir. Office'i yükleme ve her yıl yükseltme için ile karşılaştırıldığında, bu daha az pahalıdır ve daha az çaba gerektirir.

#### <a name="paas-platform-as-a-service"></a>PaaS: Hizmet olarak Platform 

PaaS ile uygulamanızı bulut hizmeti satıcının sağladığı bir ortama dağıtın. Uygulama geliştirmeye odaklanabilirsiniz satıcı tüm altyapı yönetimini desteklemez.

Azure teklifleri, Azure bulut Hizmetleri (web ve çalışan rolleri) ve Azure App Service Web Apps özelliği de dahil olmak üzere birkaç PaaS işlem sağlar. Her iki durumda da, geliştiricilerin destekleyen yazılımındaki hakkında hiçbir şey bilmeden uygulamalarını dağıtmak için birden çok yolu vardır. Geliştiriciler, sanal makineleri (VM'ler) oluşturmak, her biri için oturum açmak için Uzak Masaüstü Protokolü (RDP) kullandığınız veya uygulamayı yüklemek zorunda değilsiniz. Bunlar yalnızca bir düğmesine basın (veya ona Kapat) ve Microsoft tarafından sağlanan araçları VM'ler sağlamak dağıtın ve uygulamayı yükler.

#### <a name="iaas-infrastructure-as-a-service"></a>Iaas: Hizmet olarak altyapı

Bir Iaas bulut satıcısı çalışır ve tüm fiziksel hesaplama kaynakları ve bilgisayar sanallaştırma etkinleştirmek için gerekli yazılım yönetir. Bu hizmet bir müşteri sanal makineleri bu barındırılan veri merkezlerinde dağıtır. Sanal makineleri bir site dışı veri merkezinde bulunan rağmen Iaas tüketici yapılandırma ve bunları yönetimini denetime sahiptir.

Azure sanal makineler, sanal makine ölçek kümeleri ve ilgili ağ altyapısı dahil olmak üzere birkaç Iaas çözümleri içerir. Sanal makineler olan bir popüler başlangıçta geçirmek için seçim Hizmetleri için Azure çünkü "kaldırın ve shift" geçiş modeli sağlar. Bir VM hizmetlerinizi, veri merkezinizde çalışmakta altyapı gibi yapılandırın ve ardından yeni VM yazılımınızı geçirin. Diğer hizmetler ya da depolama URL'lere gibi yapılandırma güncelleştirmeleri yapmanız gerekebilir, ancak bu şekilde birçok uygulama geçirebilirsiniz.

Sanal makine ölçek kümeleri Azure sanal makineler üzerinde oluşturulur ve aynı VM'lerin kümeler dağıtmak için kolay bir yol sağlar. Yeni sanal makineleri otomatik olarak gerektiğinde dağıtılabilir böylece sanal makine ölçek kümeleri de otomatik ölçeklendirmeyi destekler. Bu sanal makine ölçek kümeleri ideal bir platform ana bilgisayar üst düzey mikro hizmet hesaplama kümeleri, Azure Service Fabric ve Azure kapsayıcı hizmeti gibi yapar.

## <a name="azure-services"></a>Azure hizmetleri

Azure, bulut bilgi işlem Platform birçok hizmetleri sunar. Bu hizmetler aşağıda verilmiştir.

### <a name="compute-services"></a>İşlem hizmetleri

Barındırma ve uygulama iş yükünü çalıştırmak için Hizmetler:

-   Azure sanal makineleri — Linux ve Windows

-   Uygulama Hizmetleri (Web uygulamaları, mobil uygulamalar, Logic Apps, API uygulamaları ve işlev uygulamalarının)

-   Azure toplu işlem (için büyük ölçekli paralel ve toplu işlem)

-   Azure Service Fabric

-   Azure Container Service

### <a name="data-services"></a>Veri hizmetleri

Depolama ve verileri yönetmek için Hizmetler:

-   Azure depolama (Azure Blob, kuyruk, tablo ve dosya hizmetleri içerir)

-   Azure SQL Database

-   Azure DocumentDB

-   Microsoft Azure StorSimple

-   Azure Redis Cache

### <a name="application-services"></a>Uygulama hizmetleri

Oluşturma ve uygulamaları işletim için Hizmetler:

-   Azure Active Directory (Azure AD)

-   Bağlanmak için Azure Service Bus dağıtılmış sistemleri

-   Büyük veri işleme için azure Hdınsight

-   Azure Zamanlayıcı

-   Azure Media Services

### <a name="network-services"></a>Ağ Hizmetleri

Hizmetleri hem Azure içinde hem de Azure ve şirket içi veri merkezleri arasında ağ bağlantısı için:

-   Azure Sanal Ağ

-   Azure ExpressRoute

-   Azure tarafından sağlanan DNS

-   Azure Traffic Manager

-   Azure içerik teslim ağı

Azure hizmetleri hakkında ayrıntılı belgeler için bkz: [Azure hizmet belgeleri](https://docs.microsoft.com/azure).

## <a name="azure-key-concepts"></a>Azure temel kavramları

### <a name="datacenters-and-regions"></a>Veri merkezleri ve bölgeler


Azure dünyadaki birçok bölgelerdeki genel olarak kullanılabilir olan bir genel bulut platformudur. Bir hizmet, uygulama veya azure'da VM sağladığınızda, bir bölge seçmeniz istenir. Seçilen bölge, uygulamanızın çalıştığı speciﬁc datacenter temsil eder. Daha fazla bilgi için bkz: [Azure bölgeleri](https://azure.microsoft.com/regions/).

Azure kullanmanın beneﬁts dünyanın çeşitli veri merkezleri uygulamalarınıza dağıtabilirsiniz biridir. Seçtiğiniz bölge aﬀect uygulamanızın performansını görüntüleyebilirsiniz. Ağ istek gecikme süresini azaltmak için en müşterilerinize, yakın olan bir bölge seçmek için idealdir. Ayrıca, bazı ülkelerde uygulamanızı dağıtmak için yasal gereksinimlerini karşılamak için bir bölge seçin.

### <a name="azure-portal"></a>Azure portalına


Azure portalında Azure kaynaklarını ve Hizmetleri Kaldır oluşturmak ve yönetmek için kullanılan bir web tabanlı bir uygulamadır. Azure portalı https://portal.azure.com bulunur. Özelleştirilebilir bir Pano ve Azure kaynaklarınızı yönetmek için araçları içerir. Ayrıca, faturalama ve abonelik bilgileri sağlar. Daha fazla bilgi için bkz: [Microsoft Azure portalına genel bakış](https://azure.microsoft.com/documentation/articles/azure-portal-overview/) ve [yönetmek Azure kaynakları portal üzerinden](https://docs.microsoft.com/azure/azure-portal/resource-group-portal).

### <a name="resources"></a>Kaynaklar

Azure kaynaklarını tek tek işlem, ağ, veri veya bir Azure aboneliğinize dağıtılan hizmetlerini barındıran uygulama verilebilir. Bazı ortak sanal makine, depolama hesapları ya da SQL veritabanları kaynaklardır. Azure Hizmetleri genellikle birkaç ilgili Azure kaynaklarını oluşur. Örneğin, bir Azure sanal makinesi bir VM, depolama hesabı, ağ bağdaştırıcısı ve genel IP adresi içerebilir. Bu yok oluşturulur, yönetilen ve ayrı ayrı veya bir grup olarak silinir. Azure kaynaklarını bu kılavuzun ilerleyen bölümlerinde daha ayrıntılı ele alınmaktadır.

### <a name="resource-groups"></a>Kaynak grupları

Bir Azure kaynak grubu Azure çözümünü ilgili kaynaklara tutan bir kapsayıcıdır. Kaynak grubu, çözüm için tüm kaynakları veya yalnızca bir grup olarak yönetmek istediğiniz kaynakları içerebilir. Azure kaynak gruplarını, bu kılavuzun ilerleyen bölümlerinde daha ayrıntılı olarak ele alınmıştır.

### <a name="resource-manager-templates"></a>Resource Manager şablonları

Bir Azure Resource Manager şablonu bir kaynak grubuna dağıtmak için bir veya daha fazla kaynakları tanımlayan bir JavaScript nesne gösterimi (JSON) dosyasıdır. Ayrıca, dağıtılan kaynaklarınız arasındaki bağımlılıkları da tanımlar. Resource Manager şablonları bu kılavuzun ilerleyen bölümlerinde daha ayrıntılı ele alınmaktadır.

### <a name="automation"></a>Otomasyon


Oluşturma, yönetme ve Azure portalını kullanarak silme kaynaklarına ek olarak, PowerShell veya Azure komut satırı arabirimi (CLI) kullanarak bu etkinlikleri otomatik hale getirebilirsiniz.

**Azure PowerShell**

Azure PowerShell cmdlet'leri, Azure yönetmek için sağlar modülleri kümesidir. Cmdlet öğelerini oluşturmak, yönetmek ve Azure hizmetlerini kaldırmak için kullanabilirsiniz. Cmdlet'leri yardımcı olabilecek tutarlı, yinelenebilir ve El değmeden dağıtımları elde edebilirsiniz. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/install-azurerm-ps).

**Azure komut satırı arabirimi**

Azure komut satırı arabirimi oluşturmak, yönetmek ve Azure kaynakları komut satırından kaldırmak için kullanabileceğiniz bir araçtır. Azure CLI, Linux, Mac OS X ve Windows için kullanılabilir. Daha fazla bilgi ve teknik ayrıntılar için bkz: [Azure CLI yükleme](/cli/azure/install-azure-cli.md).

**REST API** Azure, Azure portalı kullanıcı arabirimini destekler API'lerini kümesine göre oluşturulur. Bu REST API'leri çoğu program aracılığıyla sağlamak ve Azure kaynaklarınızın ve uygulamalarınızın Internet etkin herhangi bir aygıttan yönetmenize izin vermek için de desteklenir. Daha fazla bilgi için bkz: [Azure REST SDK başvurusu](https://docs.microsoft.com/rest/api/index).


## <a name="azure-subscriptions"></a>Azure abonelikleri


Bir aboneliği bir Azure hesabı için bağlı olan bir mantıksal Azure Hizmetleri gruplandırmasıdır. Bir Azure hesabı singe birden çok abonelik içerebilir. Faturalama Azure Hizmetleri için bir abonelik başına temelinde gerçekleştirilir. Aboneliği üzerinde tam denetime sahip bir hesabı yönetici ve Abonelikteki tüm Hizmetleri üzerinden denetimine sahip bir Hizmet Yöneticisi Azure aboneliğinizin olması. Ek olarak Yöneticiler, bireysel hesaplar verilebilir ayrıntılı denetim RBAC aracılığıyla Azure kaynaklarının.

### <a name="select-and-enable-an-azure-subscription"></a>Seçin ve bir Azure aboneliği etkinleştir

Azure Hizmetleri ile çalışabilmeniz için önce bir abonelik gerekiyor. Birden fazla abonelik türleri kullanılabilir.

**Ücretsiz hesaplar**: ücretsiz bir hesap üzerinde için kaydolmak için bağlantıyı [Azure Web sitesi](https://azure.microsoft.com/). Bu, bir kredi 30 gün içinde herhangi bir bileşimini Azure kaynaklarında deneyin seyri üzerinden sağlar. Kredi miktarını aşarsa, hesabınız askıya alındı. Deneme sonunda hizmetlerinizi yetkisi ve artık çalışmaz. Herhangi bir zamanda Kullandıkça Öde aboneliğine yükseltebilirsiniz.

**MSDN Abonelikleri**: bir MSDN aboneliğiniz varsa, belirli bir miktar Azure kredisi her ay alırsınız. Örneğin, bir Microsoft Visual Studio Enterprise MSDN aboneliğiniz varsa, elde \$Azure kredisi aylık 150.

Kredi miktarını aşarsa, hizmet devre dışı bırakılır sonraki ay başlatana kadar. Harcama sınırını devre dışı bırakma ve ek ücrete için kullanılacak bir kredi kartı ekleyin. Bu maliyetleri bazıları için MSDN hesapları indirildiğini. Örneğin, Windows Server çalıştıran VM'ler için Linux fiyat ödeme ve Microsoft SQL Server gibi Microsoft sunucuları için ek ücret yoktur. Bu hesapları MSDN Geliştirme ve test senaryoları için ideal hale getirir.

**BizSpark hesapları**: Microsoft BizSpark program startup'lar için birçok fayda sağlar. Bu avantajlar geliştirme ve test ortamları en fazla beş MSDN hesapları için tüm Microsoft yazılımları için erişim biridir. 150 ABD dolarını Azure kredisi her beş MSDN hesaplar için almanız ve Azure Hizmetleri, sanal makineleri gibi çeşitli azaltılmış ücretleri ödeme.

**Kullandıkça Öde**: Bu abonelikle, ne, kredi kartı veya ATM kartı hesabına ekleyerek kullanmak için ücret ödersiniz. Bir kuruluş varsa, siz de faturalama için onaylanabilir.

**Kurumsal Anlaşma**: bir kurumsal anlaşma ile belirli bir sayıda Hizmetleri Azure'da Önümüzdeki Yıl kullanarak kaydetmek ve önceden bu tutar ödeme. Yaptığınız taahhüt yıl kullanılır. Taahhüt tutarı aşarsa, içinde arrears fazla kullanım ödeme. Taahhüt miktarına bağlı olarak, bir indirim Azure hizmetlerinde öğrenin.



### <a name="grant-administrative-access-to-an-azure-subscription"></a>Bir Azure aboneliği için yönetim erişim izni ver


Birden fazla hesap yöneticisi rolleri yüklenebilir ve herhangi bir zamanda değiştirilebilir. İki anahtar rolü şunlardır:

-   **Hizmet Yöneticisi**: Bu rolün Azure Hizmetleri yönetmeye yetkilidir. Varsayılan olarak, verilen hesap yöneticisi olarak aynı hesabına erişim izni.

-   **Ortak yönetici**: Bu rolün Hizmet Yöneticisi olarak aynı erişime sahiptir. Ancak, bu rolü Azure dizinler için bir abonelik ilişkisini değiştiremezsiniz.

Daha fazla bilgi için bkz: [eklemek veya Azure yönetici rollerini değiştirmek nasıl](../../billing/billing-add-change-azure-subscription-administrator.md).

### <a name="view-billing-information-in-the-azure-portal"></a>Azure portalında faturalama bilgileri görüntüleyin


Fatura bilgilerini görüntüleme olanağı Azure kullanmanın önemli bir bileşendir. Azure portal, Azure faturalama bilgileri hakkında ayrıntılı bilgi sağlar.

Daha fazla bilgi için bkz: [faturayı ve günlük kullanım verileri faturalama Azure indirmek nasıl](../../billing/billing-download-azure-invoice-daily-usage-date.md).

### <a name="get-billing-information-from-billing-apis"></a>API faturalama fatura bilgilerini alma


Faturalama portalda görüntülemeye ek olarak, fatura bilgilerini bir betik veya program aracılığıyla Azure fatura REST API'lerini kullanarak erişebilirsiniz:

-   Azure kullanım API, kullanım verilerini almak için kullanabilirsiniz. Fatura kullanım bilgilerini ilgili Azure kaynaklarını etiketleme tarafından ince ayar yapabilirsiniz. Örneğin, her bir kaynağın bir bölüm adı ya da proje adına sahip bir kaynak grubunda etiketi ve özellikle, tek bir etiket için maliyetleri izlemek.

-   Tüm kullanılabilir kaynakları, meta veriler ve bu kaynakların her biri hakkında bilgi fiyatlandırma birlikte listelemek için Azure oranı kartı API'sini kullanabilirsiniz.

Daha fazla bilgi için bkz: [Microsoft Azure kaynak tüketimini Öngörüler elde](../../billing/billing-usage-rate-card-overview.md).

### <a name="forecast-cost-with-the-pricing-calculator"></a>Tahmini maliyet ile fiyatlandırma hesaplayıcısı

Azure'da her hizmet için fiyatlandırma farklıdır. Çoğu Azure hizmeti temel, standart ve Premium katman sağlar. Genellikle, her katman birkaç fiyat ve performans düzeyleri vardır. Kullanarak [çevrimiçi fiyatlandırma hesaplayıcısı](http://azure.microsoft.com/pricing/calculator), fiyatlandırma tahminler oluşturabilirsiniz. Hesaplayıcı tek bir kaynak veya kaynak grubunun maliyetini tahmin etmek için esneklik içerir.

### <a name="set-up-billing-alerts"></a>Fatura uyarılarını ayarlama

Uygulamanızı veya Azure ile ilgili çözüm dağıttıktan sonra harcama limitlerini yaklaşım uyarıda tanımlandığında, e-posta Gönder uyarılar oluşturabilirsiniz. Daha fazla bilgi için bkz: [uyarılar, Microsoft Azure abonelikleri için ödeme yukarı ayarlamak](../../billing/billing-set-up-alerts.md).

## <a name="azure-resource-manager"></a>Azure Resource Manager

Azure Resource Manager Azure kaynakları için dağıtım, yönetim ve kuruluşunuzun bir mekanizmadır. Kaynak Yöneticisi'ni kullanarak birçok kaynakların kaynak grubunda birlikte koyabilirsiniz.

Kaynak Yöneticisi'ni de özelleştirilebilir dağıtımı ve yapılandırılmasıyla ilgili kaynaklar için izin dağıtım özellikleri içerir. Örneğin, Kaynak Yöneticisi'ni kullanarak birden çok sanal makine, yük dengeleyici ve tek bir birim olarak bir SQL veritabanı oluşan bir uygulama dağıtabilirsiniz. Bir Resource Manager şablonu kullanarak bu dağıtımlar geliştirin.

Resource Manager çeşitli avantajlar sunar:

-   Çözümünüzdeki tüm kaynakları ayrı ayrı ele almak yerine bunları grup halinde dağıtabilir, yönetebilir ve izleyebilirsiniz.

-   Art arda çözümünüzü geliştirme yaşam döngüsü boyunca dağıtmak ve kaynaklarınızın tutarlı bir durumda dağıtılması da size güven verir.

-   Altyapınızı betikler yerine bildirim temelli şablonları kullanarak yönetebilirsiniz.

-   Doğru sırayla dağıtılmalarını sağlamak için kaynaklarınız arasındaki bağımlılıkları tanımlayabilirsiniz.

-   RBAC yerel yönetim platformuyla doğrudan tümleşik olduğundan, kaynak grubunuzdaki tüm hizmetlere erişim denetimi uygulayabilirsiniz.

-   Aboneliğinizdeki tüm kaynakları mantıksal olarak düzenlemek için kaynaklarda etiket uygulayabilirsiniz.

-   Aynı etiketi paylaşan kaynak grubu için maliyetleri görüntüleyerek kuruluşunuzun fatura açıklık getirebilir.

### <a name="tips-for-creating-resource-groups"></a>Kaynak grupları oluşturmak için ipuçları


Kaynak grupları hakkında kararlar yapıyorsanız, bu ipuçlarını göz önünde bulundurun:

-   Bir kaynak grubundaki tüm kaynakların aynı yaşam döngüsüne sahip olmalıdır.

-   Aynı anda yalnızca bir gruba bir kaynak atayabilirsiniz.

-   Ekleyebilir veya bir kaynak herhangi bir anda bir kaynak grubundan kaldırın. Her kaynak bir kaynak grubuna ait olmalıdır. Bir gruptan bir kaynağı kaldırırsanız, bu nedenle onu diğerine eklemeniz gerekir.

-   Birçok türdeki kaynağı farklı bir kaynak grubu için herhangi bir zamanda taşıyabilirsiniz.

-   Bir kaynak grubunda kaynaklar farklı bölgelerde olabilir.

-   Kaynaklar için erişimi denetlemek için bir kaynak grubu kullanabilirsiniz.

### <a name="building-resource-manager-templates"></a>Yapı Resource Manager şablonları

Resource Manager şablonları bildirimli olarak tek bir kaynak grubu içinde dağıtılan kaynak yapılandırmaları ve kaynakları tanımlayın. Resource Manager şablonları fazla komut dosyası çalıştırma veya el ile yapılandırma gerektirmeden karmaşık dağıtımlar düzenlemek için kullanabilirsiniz. Bir şablon geliştirmek sonra birden çok kez dağıtabilirsiniz — her zaman aynı sonucu.

Resource Manager şablonu dört bölümden oluşur:

-   **Parametreleri**: dağıtım girişleri bunlar. Parametre değerleri, bir insan veya otomatik bir işlem tarafından sağlanabilir. Bir örnek parametresi bir yönetici kullanıcı adı ve parola, bir Windows VM için olabilir. Parametre değerleri belirtilen dağıtım kullanılır.

-   **Değişkenleri**: Bunlar dağıtımı kullanılan değerleri tutmak için kullanılır. Parametreleri farklı olarak, dağıtım sırasında bir değişken değeri sağlanmadı. Bunun yerine, bu kodlanmış veya dinamik olarak üretilen zordur.

-   **Kaynakları**: Şablonu'nun bu bölümünde, sanal makineler, depolama hesapları ve sanal ağlar gibi dağıtılması için gereken kaynakları tanımlar.

-   **Çıktı**: bir dağıtım tamamlandıktan sonra Resource Manager dinamik olarak üretilen bağlantı dizeleri gibi verileri geri dönebilirsiniz.

Aşağıdaki mekanizmalardan dağıtım Otomasyon için kullanılabilir:

-   **İşlevler**: Resource Manager şablonları çeşitli işlevleri kullanabilirsiniz. Tanımlanan bir kaynağa birden çok örneğini dağıtma ve dinamik olarak hedef kaynak grubu döndürme bunlar küçük bir dize dönüştürme gibi işlemleri içerir. Resource Manager işlevleri dinamik dağıtımların oluşturmanıza yardımcı.

-   **Kaynak bağımlılıkları**: zaman dağıtmakta birden fazla kaynak, bazı kaynaklar bağımlılık bazılarında sahip olur. Dağıtım kolaylaştırmak için böylece bağımlı kaynaklar önce diğer dağıtılan bir bağımlılık bildirimi kullanabilirsiniz.

-   **Şablon Bağlama**: gelen bir Resource Manager şablonu içinde farklı bir şablona bağlayabilirsiniz. Bu dağıtım ayrıştırma hedeflenen, amaca şablonları kümesine izin verir.

Resource Manager şablonları herhangi bir metin düzenleyicisinde oluşturabilirsiniz. Ancak, Visual Studio için Azure SDK'sı yardımcı olacak araçlar içerir. Visual Studio kullanarak, bir sihirbaz üzerinden şablonu kaynakları eklemek sonra dağıtabilir ve doğrudan içinden şablonu Visual Studio hata ayıklama. Daha fazla bilgi için bkz: [Azure Resource Manager şablonları yazma](../../resource-group-authoring-templates.md).

Son olarak, Azure portalından yeniden kullanılabilir bir şablonu varolan kaynak gruplarını dönüştürebilirsiniz. Mevcut bir kaynak grubunun dağıtılabilir bir şablon oluşturmak istediğiniz ya da yalnızca temel alınan JSON incelemek istediğiniz, bu yararlı olabilir. Bir kaynak grubu vermek için seçin **Otomasyon betiğini** kaynak grubunun ayarlar düğmesinden.

## <a name="security-of-azure-resources-rbac"></a>Güvenlik, Azure kaynaklarının (RBAC)

Belirtilen bir kapsamda kullanıcı hesapları işletimsel erişim izni verebilir: Abonelik, kaynak grubu veya tek başına bir kaynak. Bu, bir kaynak kümesi bir sanal makine gibi bir kaynak grubuna ve tüm ilgili kaynaklar dağıtabilir ve belirli bir kullanıcı veya grup için izinler anlamına gelir. Bu yaklaşım hedef kaynak grubuna ait kaynaklara erişimi sınırlar. Ayrıca, bir sanal makine veya sanal ağ gibi tek bir kaynağa erişim izni verebilir.

Erişim vermek için kullanıcı veya kullanıcı grubu için bir rol atayın. Çok sayıda önceden tanımlanmış roller yoktur. Özel rollerinizi de tanımlayabilirsiniz.

Azure'da yerleşik birkaç örnek rolleri şunlardır:

-   **Sahibi**: Bu rolüne sahip bir kullanıcı erişim dahil her şeyi yönetebilir.

-   **Okuyucu**: Bu rolüne sahip bir kullanıcı (gizli anahtarlar dışında) tüm türlerinin kaynakları okuyabilir, ancak değişiklik yapamaz.

-   **Sanal makine Katılımcısı**: Bu rolüne sahip bir kullanıcı sanal makineleri yönetebilirsiniz ancak sanal ağa yönetemez bağlı veya VHD dosyasının bulunduğu depolama hesabı.

-   **SQL DB Katılımcısı**: Bu rolüne sahip bir kullanıcı SQL veritabanları ancak değil güvenlikle ilgili ilkelerini yönetebilirsiniz.

-   **SQL Güvenlik Yöneticisi**: Bu rolüne sahip bir kullanıcı SQL sunucularının ve veritabanlarının güvenlikle ilgili ilkelerini yönetebilirsiniz.

-   **Depolama hesabı katkıda bulunan**: Bu rolüne sahip bir kullanıcı depolama hesapları yönetebilir ancak depolama hesaplarına erişim yönetemez.

Daha fazla bilgi için bkz: [Azure aboneliği kaynaklarınıza erişimi yönetmek için rol atamalarını kullanın](../../active-directory/role-based-access-control-configure.md).

## <a name="azure-virtual-machines"></a>Azure Sanal Makineler

Azure sanal makinelerin azure'da merkezi Iaas hizmetlerden biri şeklindedir. Azure sanal makineleri bir Microsoft Azure veri merkezinde Windows veya Linux sanal makineleri dağıtımını destekler. VM yapılandırması üzerinde toplam denetimi yoktur ve tüm yazılım yükleme, yapılandırma ve bakım için sorumlu Azure Virtual Machines ile.

Bir Azure VM dağıtıyorsanız, Azure Market'teki bir görüntü seçin veya, kendi genelleştirilmiş yansıma sağlayabilirsiniz. Bu görüntü işletim sistemi ve ilk yapılandırmasını uygulamak için kullanılır. Dağıtım sırasında Resource Manager bilgisayar adı, yönetici kimlik bilgilerini ve ağ yapılandırması atama gibi bazı yapılandırma ayarları işleyecek. Azure sanal makine uzantıları, ek yazılım yüklemesi gibi yapılandırmaları, virüsten koruma yapılandırma ve çözümlerini izleme otomatikleştirmek için kullanabilirsiniz.

Sanal makineler birçok farklı boyutlarda oluşturabilirsiniz. Kaynak ayırma işlemi, bellek ve depolama kapasitesi gibi sanal makine boyutunu belirler. Bazı durumlarda, RDMA özellikli ağ bağdaştırıcıları ve SSD diskleri gibi belirli özellikleri yalnızca belirli VM boyutları ile kullanılabilir. "Azure sanal makineler için Boyutlar" VM boyutları ve yetenekleri tam listesi için bkz [Windows](../../virtual-machines/windows/sizes.md) ve [Linux](../../virtual-machines/linux/sizes.md).

### <a name="use-cases"></a>Uygulama alanları

Azure sanal makineleri yapılandırma üzerinde tam denetim sunduklarından, bunlar çok çeşitli PaaS modele uymayan sunucu iş yükleri için idealdir. Sunucu iş yükleri gibi veritabanı sunucuları (SQL Server, Oracle veya MongoDB), Windows Server Active Directory, Microsoft SharePoint ve birçok daha fazla Microsoft Azure platformu üzerinde çalıştırmak mümkün olur. İsterseniz, bir veya daha fazla Azure bölgeleri, çok miktarda yeniden yapılandırma olmadan için bir şirket içi veri merkezinden tür iş yüklerinin taşıyabilirsiniz.

### <a name="deployment-of-virtual-machines"></a>Sanal makineler dağıtımı

Azure portalını kullanarak otomasyon ile Azure PowerShell modülünü kullanarak veya Otomasyon ile platformlar arası CLI kullanarak Azure sanal makineler dağıtabilirsiniz.

**Portal**

Azure portalını kullanarak bir sanal makine dağıtma, yalnızca bir etkin Azure aboneliği ve bir web tarayıcısı erişimi gerektirir. Birçok farklı işletim sistemi görüntüleri değişen yapılandırmalarla seçebilirsiniz. Tüm depolama ve ağ gereksinimleri dağıtım sırasında yapılandırılır. Daha fazla bilgi için "Azure portalında bir sanal makine oluşturma" bkz [Windows](../../virtual-machines/windows/quick-create-portal.md) ve [Linux](../../virtual-machines/linux/quick-create-portal.md).

Azure portalından bir sanal makine dağıtımına ek olarak, bir Azure Resource Manager şablonu portalından dağıtabilirsiniz. Bu, dağıtın ve tüm kaynakları şablonunda tanımlandığı şekilde yapılandırın. Daha fazla bilgi için bkz: [kaynakları Resource Manager şablonları ve Azure portalı ile dağıtma](../../azure-resource-manager/resource-group-template-deploy-portal.md).


**PowerShell**

PowerShell kullanarak bir Azure sanal makine dağıtımı tamamlandı dağıtım ağ ve depolama dahil olmak üzere tüm ilgili sanal makine kaynakları için sağlar. Daha fazla bilgi için bkz: [Resource Manager ve PowerShell kullanarak bir Windows VM oluşturma](../../virtual-machines/windows/quick-create-powershell.md).

Azure işlem kaynakları ayrı ayrı dağıtımına ek olarak, bir Azure Resource Manager şablonu dağıtmak için Azure PowerShell modülü kullanabilirsiniz. Daha fazla bilgi için bkz: [Resource Manager şablonları ve Azure PowerShell ile kaynakları dağıtmak](../../azure-resource-manager/resource-group-template-deploy.md).

**Komut satırı arabirimi (CLI)**

PowerShell modülü gibi Azure komut satırı arabirimi dağıtım Otomasyon sağlar ve Windows, OS X veya Linux sistemlerinde kullanılabilir. Azure CLI kullanırken **vm hızlı Oluştur** komutu, ilişkili tüm sanal makine kaynakları (depolama alanıyla birlikte ve ağ iletişimi) ve sanal makine dağıtılır. Daha fazla bilgi için bkz: [CLI kullanarak Azure'da bir Linux VM oluşturma](../../virtual-machines/linux/quick-create-cli.md).

Benzer şekilde, bir Azure Resource Manager şablonu dağıtmak için Azure CLI kullanabilirsiniz. Daha fazla bilgi için bkz: [Resource Manager şablonları ve Azure CLI kaynaklarla dağıtmak](../../azure-resource-manager/resource-group-template-deploy-cli.md).

### <a name="access-and-security-for-virtual-machines"></a>Erişim ve sanal makineleri için güvenlik

Bir sanal makine Internet'ten erişmeyi gerektirir ilişkili ağ arabirim ya da yük dengeleyici (varsa), bir ortak IP adresi ile yapılandırılmalıdır. Genel IP adresi sanal makineyi gidermek veya yük dengeleyici bir DNS adı içerir. Daha fazla bilgi için bkz: [azure'daki IP adresleri](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).

Bir ağ güvenlik grubu (NSG) kaynağı kullanarak genel IP adresi sanal makineye erişimi yönetin. Bir NSG'yi bir güvenlik duvarı gibi davranır ve izin verir veya ağ arabirimi ya da tanımlanmış bağlantı noktalarını bir dizi alt ağ arasında trafiği engeller. Örneğin, bir Azure VM ile Uzak Masaüstü oturumu oluşturmak için NSG 3389 numaralı bağlantı noktasına gelen trafiğe izin verecek şekilde yapılandırmanız gerekir. Daha fazla bilgi için bkz: [Azure portal kullanarak Azure'da bir VM için bağlantı noktaları açma](../../virtual-machines/windows/nsg-quickstart-portal.md).

Son olarak, yönetimi herhangi bir bilgisayar sisteminin gibi ile güvenlik bir Azure sanal makine işletim sistemi için güvenlik kimlik bilgileri ve güvenlik duvarı yazılımları kullanarak sağlamalıdır.

## <a name="azure-storage"></a>Azure Storage

Azure Storage sağlam, ölçeklenebilir ve yedekli depolama sağlayan bir Microsoft tarafından yönetilen bir hizmettir. Bir Azure depolama hesabı herhangi bir kaynak dağıtım yöntemini kullanarak herhangi bir kaynak grubuna bir kaynak olarak ekleyebilirsiniz. Azure dört depolama türlerini içerir: Blob Depolama, File Storage, Table storage ve kuyruk depolama. Bir depolama hesabını dağıtırken, iki hesap türleri kullanılabilir, genel amaçlı ve blob depolama. Genel amaçlı depolama hesabı için tüm dört depolama türlerini erişmenizi sağlar. BLOB storage hesapları, genel amaçlı hesaplarınıza benzer ancak sıcak ve soğuk erişim katmanları dahil özelleştirilmiş BLOB'ları içerir. Blob depolama hakkında daha fazla bilgi için bkz: [Azure Blob Depolama](../../storage/blobs/storage-blob-storage-tiers.md).

Azure depolama hesapları farklı düzeylerde artıklık yapılandırılabilir:

-   **Yerel olarak yedekli depolama** yazma başarılı olarak kabul önce tüm verileri üç kopyasını eş zamanlı olarak yapılır sağlayarak yüksek düzeyde kullanılabilirlik sağlar. Bu kopyaları, tek bir bölgedeki tek bir tesis içinde depolanır. Çoğaltmalar ayrı hata etki alanlarında bulunan ve etki alanlarını yükseltme. Bu, veri depolama düğümü, veri başarısız bulunduran veya güncelleştirilmesi için çevrimdışı duruma olsa bile kullanılabilir olduğu anlamına gelir.

-   **Coğrafi olarak yedekli depolama** yüksek kullanılabilirlik için birincil bölgede verileri üç zaman uyumlu kopyası yapar ve sonra zaman uyumsuz olarak üç çoğaltmaları olağanüstü durum kurtarma için eşlenmiş bir bölgede yapar.

-   **Coğrafi olarak yedekli depolamaya okuma erişimi** coğrafi olarak yedekli depolama artı ikincil bölge verileri okuma özelliği. Bu özelliği kısmi olağanüstü durum kurtarma için uygun hale getirir. Birincil bölge ile ilgili bir sorun varsa, eşleştirilmiş bir bölgeye salt okunur erişim sağlamak için uygulamanızın değiştirebilirsiniz.

### <a name="use-cases"></a>Uygulama alanları 

Her depolama türü farklı kullanım örneği vardır.

**Blob depolama** 

Word *blob* kısaltması olan *ikili büyük nesne*. BLOB'ları, bilgisayarınızda depolamak olanlar gibi yapılandırılmamış dosyalarıdır. Blob Storage belge, medya dosyası veya uygulama yükleyici gibi her tür metin veya ikili veri depolayabilir. Blob Storage aynı zamanda nesne depolama olarak adlandırılır. Azure Blob storage ayrıca Azure sanal makine veri diski tutar.

Azure Storage üç tür BLOB'ları destekler:

-   **Blok blobları** tutmak için kullanılan normal dosyaları 195 GB'a kadar boyutu (4 MB x 50.000 blok). Blok blobları birincil kullanım nedeni, medya dosyalarını veya Web siteleri için görüntü dosyaları gibi baştan okuma dosyalarının depolanması ' dir. 64 MB'tan büyük dosyalar küçük taşı olarak karşıya gerekir çünkü bunlar blok blobları olarak adlandırılır. Bu blokları ardından birleştirilmiş (kaydedilen son blob'a veya).

-   **Sayfa blobları** tutmak için kullanılan rastgele erişim boyutu 1 TB'ye kadar dosyaları. Sayfa bloblarını dayanıklı diskler için Azure sanal makineler sağlamak VHD için öncelikle yedekleme depolama alanı olarak kullanılır, Iaas Azure hizmetinde işlem. 512 baytlık sayfalarına rasgele okuma/yazma erişimi sağlayan, sayfa bloblarını adlandırılır.

-   **Ekleme blobları** oluşur blok blok blobları benzer, ama için iyileştirilmiş ekleme işlemleri. Bunlar sıklıkla aynı blob için bir veya daha fazla kaynaklardan günlük bilgileri için kullanılır. Örneğin, birden çok Vm'lerinde çalışan bir uygulama için aynı ek blobu için tüm izleme günlüğü yazabilirsiniz. Tek ek blob 195 GB'a kadar olabilir.

Daha fazla bilgi için bkz: [.NET kullanarak Azure Blob storage'ı kullanmaya başlama](../../storage/blobs/storage-dotnet-how-to-use-blobs.md).

**Dosya depolama**

Azure File storage standart sunucu ileti bloğu (SMB) protokolünü kullanarak dosya paylaşımlarını bulutta sunan bir hizmettir. Hizmeti SMB 2.1 ve SMB 3.0 destekler. Azure File storage ile maliyetli yeniden yazdırmaya gerek duymadan ve hızla Azure dosya paylaşımları kullanan uygulamalar geçirebilirsiniz. Azure sanal makinelerde çalışan uygulamaların, bulut Hizmetleri veya şirket içi istemcileri buluta bir dosya paylaşımı bağlayabilir. Bu, bir masaüstü uygulamasının tipik bir SMB paylaşımına nasıl bağlar için benzer. Ardından herhangi sayıda uygulama bileşeni eş zamanlı olarak File Storage paylaşımını bağlayıp buna erişim sağlayabilir.

File storage paylaşımı standart SMB dosya paylaşımı olduğu için Azure'da çalışan uygulamalar dosya sistemi g/ç API'leri üzerinden paylaşımdaki veriye erişebilir. Geliştiriciler mevcut uygulamalarını taşımak üzere kullandıkları kodlar ve yetenekleri bu nedenle kullanabilirsiniz. BT uzmanları, oluşturmak, bunları bağlamak ve Azure uygulamalarını yönetiminin bir parçası File storage paylaşımlarını yönetmek için PowerShell cmdlet'lerini kullanabilirsiniz.

Daha fazla bilgi için bkz: [Windows Azure File storage ile çalışmaya başlama](../../storage/files/storage-how-to-use-files-windows.md) veya [Azure File storage'ı Linux ile kullanma konusunda](../../storage/files/storage-how-to-use-files-linux.md).

**Tablo depolama**

Azure Table Storage, bulutta yapılandırılmış NoSQL verileri depolayan bir hizmettir. Table storage, şema daha az tasarım ile bir anahtar/öznitelik deposudur. Table storage şema daha az olduğundan, uygulamanızın ihtiyaçları geliştikçe verilerinizi uyarlamak da kolaylaşır. Her türlü uygulama için verilere erişim hızlı ve uygun maliyetlidir. Table Storage, benzer hacimdeki veriler için geleneksel SQL’e oranla çok daha düşük maliyetlidir.

Web uygulamaları için kullanıcı verileri, adres defterleri, cihaz bilgileri ve hizmetiniz için gerekli olan tüm diğer meta veri türleri gibi esnek veri kümelerini depolamak üzere Table Storage’ı kullanabilirsiniz. Bir tablodaki tüm varlıkların sayısı depolayabilirsiniz. Bir depolama hesabı herhangi bir sayı depolama hesabının kapasite sınırını kadar tablo içerebilir.

Daha fazla bilgi için bkz: [Azure Table storage'ı kullanmaya başlama](../../cosmos-db/table-storage-how-to-use-dotnet.md).

**Kuyruk depolama**

Azure Queue depolama birimi, uygulama bileşenleri arasında bulut mesajlaşma özelliği sağlar. Böylece bağımsız olarak ölçeklendirebilirsiniz uygulamalar için ölçek tasarlarken, uygulama bileşenleri genellikle birbirinden ayrılır. Kuyruk depolama bulutta, masaüstünde, şirket içi sunucuda veya mobil bir cihazda çalışan uygulama bileşenleri arasındaki iletişim için zaman uyumsuz mesajlaşma sunar. Kuyruk depolama zaman uyumsuz görevlerin yönetilmesini ve süreç iş akışlarının oluşturulmasını da destekler.

Daha fazla bilgi için bkz: [Azure kuyruk depolamaya başlayın](../../storage/queues/storage-dotnet-how-to-use-queues.md).

### <a name="deploying-a-storage-account"></a>Bir depolama hesabı dağıtma

Bir depolama hesabı dağıtmak için birkaç seçeneğiniz vardır.

**Portal**

Azure portalı kullanarak bir depolama hesabı dağıtılması yalnızca bir etkin Azure aboneliği ve bir web tarayıcısı erişimi gerekir. Yeni veya var olan kaynak grubuna yeni bir depolama hesabı dağıtabilirsiniz. Depolama hesabınızı oluşturduktan sonra portal kullanarak bir blob kapsayıcı veya dosya paylaşımı oluşturabilirsiniz. Tablo oluşturun ve depolama varlıklar program aracılığıyla sıra. Daha fazla bilgi için bkz: [depolama hesabı oluşturma](../../storage/common/storage-create-storage-account.md#create-a-storage-account).

Bir depolama hesabı Azure portalından dağıtımına ek olarak, bir Azure Resource Manager şablonu portalından dağıtabilirsiniz. Bu, dağıtın ve tüm kaynakları herhangi bir depolama hesabı dahil olmak üzere şablonunda tanımlandığı şekilde yapılandırın. Daha fazla bilgi için bkz: [kaynakları Resource Manager şablonları ve Azure portalı ile dağıtma](../../azure-resource-manager/resource-group-template-deploy-portal.md).

**PowerShell**

PowerShell kullanarak Azure storage hesabı dağıtma tam dağıtım depolama hesabı için sağlar. Daha fazla bilgi için bkz: [Azure Storage ile Azure PowerShell'i kullanma](../../storage/common/storage-powershell-guide-full.md).

Azure kaynaklarını tek tek dağıtımına ek olarak, bir Azure Resource Manager şablonu dağıtmak için Azure PowerShell modülü kullanabilirsiniz. Daha fazla bilgi için bkz: [Resource Manager şablonları ve Azure PowerShell ile kaynakları dağıtmak](../../azure-resource-manager/resource-group-template-deploy.md).

**Komut satırı arabirimi (CLI)**

PowerShell modülü gibi Azure komut satırı arabirimi dağıtım Otomasyon sağlar ve Windows, OS X veya Linux sistemlerinde kullanılabilir. Azure CLI kullanabilirsiniz **depolama hesabı oluşturma** bir depolama hesabı oluşturmak için komutu. Daha fazla bilgi için bkz: [Azure Storage ile Azure CLI kullanma.](../../storage/common/storage-azure-cli.md)

Benzer şekilde, bir Azure Resource Manager şablonu dağıtmak için Azure CLI kullanabilirsiniz. Daha fazla bilgi için bkz: [Resource Manager şablonları ve Azure CLI kaynaklarla dağıtmak](../../resource-group-template-deploy-cli.md).

### <a name="access-and-security-for-azure-storage"></a>Erişim ve güvenliği Azure depolama

Azure depolama, ancak Azure portalı, VM oluşturma işlemi sırasında ve depolama istemci kitaplıklarından dahil olmak üzere çeşitli yollarla erişilir. 

**Sanal makine disklerini**

Bir sanal makine dağıtıyorsanız, aynı zamanda sanal makine işletim sistemi diski ve ek diskleri tutmak için bir depolama hesabı oluşturmanız gerekir. Mevcut bir depolama hesabını seçin veya yeni bir tane oluşturun. Bir blob en büyük boyutu 1024 GB olduğundan, tek bir VM disk 1,023 GB boyut sınırı yoktur. Daha büyük bir veri diski yapılandırmak için sanal makinenin birden fazla veri diski sunacak ve bunları birlikte tek bir mantıksal disk havuzu. Daha fazla bilgi için "Yönet Azure disklerine" için [Windows](../../virtual-machines/windows/tutorial-manage-data-disk.md) ve [Linux](../../virtual-machines/linux/tutorial-manage-disks.md).

**Depolama araçları**

Azure depolama hesapları, Visual Studio Cloud Explorer gibi birçok farklı depolama gezginleri aracılığıyla erişilebilir. Bu araçlar, depolama hesapları ve verileri Gözat olanak tanır. Daha fazla bilgi ve kullanılabilir depolama gezginleri listesi için bkz: [Azure Storage istemci araçları](../../storage/common/storage-explorers.md).

**Depolama API**

Depolama kaynaklarını HTTP/HTTPS istekleri yapabilen herhangi bir dil tarafından erişilebilir. Ayrıca, Azure Storage birkaç popüler dilde programlama kitaplıkları sunar. Bu kitaplıklar Azure Storage ile çalışma zaman uyumlu ve zaman uyumsuz çağırma gibi ayrıntıları işleme, işlem, özel durum yönetimi ve otomatik yeniden denemeler toplu işleme basitleştirir. Daha fazla bilgi için bkz: [Azure depolama hizmeti REST API Başvurusu](/rest/api/storageservices/Azure-Storage-Services-REST-API-Reference).

**Depolama erişim tuşu**

Her Depolama hesabı, iki kimlik doğrulaması anahtarları, birincil ve ikincil sahiptir. Ya da depolama erişim işlemleri için kullanılabilir. Bu depolama anahtarları ve verileri programlı olarak erişmek için gerekli olan bir depolama hesabı güvenliğinin sağlanmasına yardımcı olmak için kullanılır. Güvenliği artırmak için anahtarların zaman rollover izin vermek için iki anahtar vardır. Hesap adı yanı sıra kendi mülkü depolama hesabında herhangi bir veri sınırsız erişimi verdiğinden anahtarların güvenliğini sağlamak için önemlidir.

**Paylaşılan erişim imzaları**

Kullanıcıların depolama kaynaklarınıza erişmek için denetlenen izin vermeniz gerekiyorsa, paylaşılan erişim imzası oluşturabilirsiniz. Paylaşılan erişim imzası bir depolama kaynağına yetkilendirilmiş erişim sağlayan URL'ye eklenebilen bir belirteçtir. Süre olan geçerli için izinlere sahip belirtir, işaret kaynak belirtece sahip olan herkes erişebilir. Daha fazla bilgi için bkz: [kullanarak paylaşılan erişim imzaları](../../storage/common/storage-dotnet-shared-access-signature-part-1.md).

## <a name="azure-virtual-network"></a>Azure Sanal Ağ


Sanal ağlar, sanal makineler arasındaki iletişim desteklemek gereklidir. Yük Dengeleme ve alt ağlar, özel IP adresi, DNS ayarlarını, güvenlik filtresini tanımlayın. Bir VPN ağ geçidi veya expressroute bağlantı hattı kullanarak, Azure sanal ağı şirket içi ağlarınız bağlanabilir.

### <a name="use-cases"></a>Uygulama alanları

Azure ağı farklı kullanım durumlar vardır.

**Yalnızca bulut sanal ağlar**

Varsayılan olarak, bir Azure sanal ağı, yalnızca Azure'da depolanan kaynakları erişilebilir. Aynı sanal ağa bağlı Kaynaklar birbirleri ile iletişim kurabilir. Sanal makine ağ arabirimleri ilişkilendirin ve yük Dengeleyiciler sanal makine Internet üzerinden erişilebilir olması için ortak IP adresine sahip. Bir ağ güvenlik grubu kullanarak genel olarak sunulan kaynaklarına güvenli erişim yardımcı olabilir.

**Şirket içi sanal ağlar**

Bir Azure sanal ağı için bir şirket içi ağ ExpressRoute veya siteden siteye VPN bağlantısı kullanarak bağlanabilir. Bu yapılandırmada, Azure sanal ağı aslında bir bulut tabanlı ve Şirket ağınızın uzantısıdır.

Azure sanal ağı şirket içi ağınıza bağlı olduğundan, sanal ağlar kuruluşunuzun kullandığı adres alanı benzersiz bir kısmı kullanmalısınız şirketler arası. Ağınızı genişletirsiniz farklı Kurumsal konumların belirli bir IP alt ağı atanan aynı şekilde, Azure başka bir konum haline gelir.

###<a name="deploying-a-virtual-network"></a>Bir sanal ağ dağıtma

Bir sanal ağı dağıtmak için birkaç seçeneğiniz vardır.

**Portal**

Azure portalını kullanarak bir Azure sanal ağı dağıtma, yalnızca bir etkin Azure aboneliği ve bir web tarayıcısı erişimi gerektirir. Yeni veya var olan kaynak grubuna yeni bir sanal ağ dağıtabilirsiniz. Portaldan yeni bir sanal makine oluştururken, varolan bir sanal ağı seçin veya yeni bir tane oluşturun. Daha fazla bilgi için bkz: [Azure portalını kullanarak bir sanal ağ oluşturma](../../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

Bir Azure sanal ağı Azure portalından dağıtımına ek olarak, bir Azure Resource Manager şablonu portalından dağıtabilirsiniz. Bu, dağıtın ve tüm kaynakları tüm sanal ağ kaynakları dahil etme şablonunda tanımlandığı şekilde yapılandırın. Daha fazla bilgi için bkz: [kaynakları Resource Manager şablonları ve Azure portalı ile dağıtma](../../azure-resource-manager/resource-group-template-deploy-portal.md).

**PowerShell**

PowerShell kullanarak bir Azure sanal ağı dağıtmak için depolama hesabının tamamını dağıtım Otomasyon sağlar. Daha fazla bilgi için bkz: [PowerShell kullanarak bir sanal ağ oluşturma](../../virtual-network/virtual-networks-create-vnet-arm-ps.md).

Azure kaynaklarını tek tek dağıtımına ek olarak, bir Azure Resource Manager şablonu dağıtmak için Azure PowerShell modülü kullanabilirsiniz. Daha fazla bilgi için bkz: [Resource Manager şablonları ve Azure PowerShell ile kaynakları dağıtmak](../../azure-resource-manager/resource-group-template-deploy.md).

**Komut satırı arabirimi (CLI)**

PowerShell modülü gibi Azure komut satırı arabirimi dağıtım Otomasyon sağlar ve Windows, OS X veya Linux sistemlerinde kullanılabilir. Azure CLI kullanabilirsiniz **ağ vnet oluşturma** bir sanal ağ oluşturmak için komutu. Daha fazla bilgi için bkz: [Azure CLI kullanarak bir sanal ağ oluşturma](../../virtual-network/virtual-networks-create-vnet-arm-cli.md).

Benzer şekilde, bir Azure Resource Manager şablonu dağıtmak için Azure CLI kullanabilirsiniz. Daha fazla bilgi için bkz: [Resource Manager şablonları ve Azure CLI kaynaklarla dağıtmak](../../azure-resource-manager/resource-group-template-deploy-cli.md).

### <a name="access-and-security-for-virtual-networks"></a>Erişim ve sanal ağlar için güvenlik

Bir ağ güvenlik grubu kullanarak güvenli Azure sanal ağlar yardımcı olabilir. Nsg'ler izin veren veya reddeden bir sanal ağ üzerindeki VM örneklerinize ağ trafiğinin erişim denetimi listesi (ACL) kurallarının bir listesini içerir. Nsg'ler alt ağlarla veya bu alt ağ içindeki tekil VM örnekleriyle ilişkilendirebilirsiniz. Bir NSG'yi bir alt ağ ile ilişkilendirdiğinizde, ACL kuralları bu alt ağdaki tüm VM örnekleri için geçerlidir. Ayrıca, daha fazla trafik tekil bir VM için bir NSG doğrudan bu VM ile ilişkilendirerek kısıtlayabilirsiniz. Daha fazla bilgi için bkz: [filtre ağ güvenlik grupları ile ağ trafiği](../../virtual-network/virtual-networks-nsg.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Windows VM oluşturma](/virtual-machines/windows/quick-create-portal.md)
- [Bir Linux VM oluşturma](../../virtual-machines/linux/quick-create-portal.md)
