---
title: Azure genel bulutta yalıtım | Microsoft Docs
description: Çeşitli bilgi işlem örnekleri dahil bulut tabanlı bilgi işlem Hizmetleri ve yukarı ve aşağı otomatik olarak uygulama veya Kurumsal ihtiyaçlarını karşılamak üzere ölçeklenebilir hizmetler hakkında bilgi edinin.
services: security
documentationcenter: na
author: UnifyCloud
manager: barbkess
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomSh
ms.openlocfilehash: b8142551d9c20c18d83c256b3f07a0deb291577c
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66147650"
---
# <a name="isolation-in-the-azure-public-cloud"></a>Azure genel bulutta yalıtım
##  <a name="introduction"></a>Tanıtım
### <a name="overview"></a>Genel Bakış
Geçerli ve gelecekteki Azure yardımcı olmak üzere, müşteriler anlamak ve çeşitli güvenlikle ilgili kullanılabilir becerilerinden ve Azure platformu çevreleyen, Microsoft teknik incelemeler, güvenlik genel bakış, en iyi yöntemler, bir dizi geliştirdi ve Denetim listeleri.
Konular avantajlarına ve derinlik bakımından aralığı ve düzenli olarak güncelleştirilir. Bu belge, soyut bir bölümü aşağıda özetlendiği gibi serisi bir parçası değil.

### <a name="azure-platform"></a>Azure platformu
Azure işletim sistemi, dilleri, çerçeveler, Araçlar, veritabanları ve cihazlar programlama yelpazesini destekleyen açık ve esnek bir bulut hizmeti platformudur. Örneğin, şunları yapabilirsiniz:
- Linux kapsayıcılarını Docker tümleştirmesiyle çalıştırın;
- JavaScript, Python, .NET, PHP, Java ve Node.js kullanarak uygulamalar oluşturun; ve
- Arka iOS, Android ve Windows cihazları uçlar oluşturun.

Microsoft Azure, milyonlarca Geliştirici teknolojileri destekler ve BT profesyonellerinin zaten kullandığı ve güvendiği.

Oluşturmak ya da BT varlıklarınızı geçirme bir genel bulut hizmet sağlayıcısı, uygulamalar ve hizmetler ve bulut tabanlı varlıklarınızın güvenliğinin yönetmenizi sağlarlar denetimleri ile verileri korumak için bir kuruluşun yeteneklerine bağlı.

Azure altyapısı tesisten uygulamalara kadar milyonlarca müşteriye aynı anda hizmet verecek şekilde tasarlanmıştır ve işletmelerin güvenlik ihtiyaçlarını karşılayabilecek güvenilir bir temel sunar. Buna ek olarak, Azure’da çok çeşitli ve yapılandırılabilir güvenlik seçenekleri ile bunlar üzerinde denetim imkanı sunulmaktadır. Böylece, dağıtımlarınıza özel gereksinimleri karşılamak için güvenlik özelliklerini uyarlayabilirsiniz. Bu belge, bu gereksinimleri karşılamanıza yardımcı olur.

### <a name="abstract"></a>Özet

Microsoft Azure, uygulamaları ve sanal makineler (VM) paylaşılan fiziksel altyapı üzerinde çalıştırmasına olanak sağlar. Bir bulut ortamında çalışan uygulamalar için ekonomik prime yararlarından biri birden çok müşteriyi arasında paylaşılan kaynakların maliyetini dağıtma kabiliyeti olan. Bu yöntem, çok kiracılı çoğullama kaynaklar arasında düşük maliyetler, farklı müşteriler tarafından verimliliği artırır. Ne yazık ki, fiziksel sunucuların ve diğer altyapı kaynaklarını duyarlı uygulamalar ve rastgele ve olası kötü niyetli bir kullanıcıya ait Vm'leri çalıştırmak için paylaşımı riskini da tanıtılmaktadır.

Bu makalede, Microsoft Azure nasıl kötü amaçlı ve kötü amaçlı olmayan kullanıcılara karşı yalıtımı sağlar ve mimarları için çeşitli yalıtım seçenekleri sunarak bulut çözümleri oluşturma için bir kılavuz olarak hizmet veren özetlenmektedir. Bu teknik incelemede, Azure platformu ve müşterilere yönelik güvenlik denetimlerinin teknolojiye odaklanır ve fiyatlandırma modelleri ve DevOps yöntem değerlendirmeleri adresi SLA için çalışmaz.

## <a name="tenant-level-isolation"></a>Kiracı düzeyinde yalıtım
Birincil kavramı, paylaşılan, ortak bir altyapı üzerinde çeşitli müşterilerin aynı anda olan bilgi işlem, ekonomik ölçeklendirme için önde gelen bulut avantajlarından biri. Bu kavram, çok kiracılı modeli adı verilir. Microsoft, Microsoft bulut Azure'nın çok kiracılı mimari güvenlik, gizlilik, gizlilik, bütünlük ve kullanılabilirlik standartları desteklediğinden emin olmak için sürekli olarak çalışır.

Bulutun etkin olduğu çalışma alanında kiracı, bulut hizmetinin belirli bir örneğine sahip olan ve o örneği yöneten bir istemci veya kuruluş olarak tanımlanabilir. Microsoft Azure tarafından sağlanan kimlik platformunda Kiracı yalnızca bir adanmış Azure Active kuruluşunuz alan ve bir Microsoft bulut hizmeti için oturum açtığında sahibi Directory (Azure AD) örneğidir.

Her Azure AD dizini, diğer Azure AD dizinlerinden farklı ve ayrıdır. Kurumsal ofis binasının yalnızca kuruluşunuza özel güvenilir bir varlık olması gibi, Azure AD dizini de yalnızca sizin kuruluşunuz tarafından kullanılmak üzere tasarlanan güvenilir bir varlıktır. Azure AD mimarisi, müşteri verilerini ve kimlik bilgilerini ortak karıştırma alanından yalıtır. Bu, bir Azure AD dizinindeki kullanıcıların ve yöneticilerin yanlışlıkla veya kötü amaçlı olarak başka bir dizindeki verilere erişemeyeceği anlamına gelir.

### <a name="azure-tenancy"></a>Azure Kiracı
Azure Kiracı (Azure abonelik) başvuran bir "Müşteri/billing" ilişkisi ve benzersiz bir [Kiracı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant) içinde [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis). Kiracı düzeyinde yalıtım Microsoft azure'da, Azure Active Directory'yi kullanarak gerçekleştirilir ve [rol tabanlı denetimler](https://docs.microsoft.com/azure/role-based-access-control/overview) tarafından sunulur. Her Azure aboneliği bir Azure Active Directory (AD) dizini ile ilişkilidir.

Kullanıcılara, gruplara veya bu dizine uygulamalarından Azure aboneliğinde kaynakları yönetebilir. Azure portalı, Azure komut satırı araçları ve Azure Management API'leri kullanılarak bu erişim hakları atayabilirsiniz. Azure AD kiracısı, böylece hiçbir müşteri erişebilir veya kötü amaçlı olarak veya yanlışlıkla ortak kiracılar, tehlikeye güvenlik sınırları kullanılarak mantıksal olarak yalıtılır. İstenmeyen bağlantılar ve trafik burada konak düzeyinde paket filtreleme ve Windows Güvenlik Duvarı Engelleme "çıplak" sunuculara ayrı ağ kesiminde, yalıtılmış Azure AD'ye çalıştırır.

- Azure ad'deki verilerine erişim, bir güvenlik belirteci hizmeti (STS) aracılığıyla kullanıcı kimlik doğrulaması gerektirir. Kullanıcının varlık, etkinleştirilen ve rol bilgilerini yetkilendirme sistem tarafından istenen erişim hedef Kiracı için bu kullanıcı için bu oturumda yetki verilip verilmediğini belirlemek için kullanılır.

![Azure Kiracı](./media/azure-isolation/azure-isolation-fig1.png)


- Kiracılar ayrık kapsayıcılardır ve ilişkisi yoktur. Bunlar arasında.

- Kiracı Yöneticisi Federasyon veya diğer kiracılardan kullanıcı hesaplarını sağlama yoluyla verir sürece hiçbir erişim arasında Kiracı.

- Azure AD hizmeti oluşturan sunuculara fiziksel erişim ve Azure AD'nin arka uç sistemlerine doğrudan erişimi sınırlıdır.

- Azure AD kullanıcıların fiziksel varlıklar veya konumları erişimi yok ve bu nedenle aşağıdaki belirtilen mantıksal RBAC İlkesi denetimleri atlamak mümkün değildir.

Tanılama ve Bakım gereksinimlerini, just-ın-time ayrıcalık yükseltme sistemi kullanan bir çalışma modeline gerekli ve kullanılır. Azure AD Privileged Identity Management (PIM) uygun yönetici kavramını sunar. [Uygun Yöneticiler](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) ayrıcalıklı ihtiyacı olan kullanıcı erişim zorunluluğu ancak her gün olmalıdır. Bu rol, kullanıcı erişime ihtiyaç duyana kadar devre dışıdır ancak kullanıcı bir etkinleştirme işlemini tamamladıktan sonra önceden belirlenen süre boyunca etkin bir yönetici olur.

![Azure AD Privileged Identity Management](./media/azure-isolation/azure-isolation-fig2.png)

Azure Active Directory ilkeleri ve izinleri için ve yalnızca ait ve Kiracı tarafından yönetilen bir kapsayıcı içinde kendi korumalı kapsayıcıdaki her bir kiracı barındırır.

Kiracı kapsayıcıları kavramını derin portalları kalıcı depolama için tüm gelen tüm katmanlarda dizin hizmetinde bulunan þeklimize.

Birden çok Azure Active Directory kiracısından meta verileri aynı fiziksel diskte depolanan bile, bir ilişki yoktur sırayla Kiracı Yöneticisi tarafından dikte edilir dizin hizmeti tarafından tanımlanan farklı kapsayıcılar arasında.

### <a name="azure-role-based-access-control-rbac"></a>Azure rol tabanlı erişim denetimi (RBAC)
[Azure rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) çeşitli bileşenleri bir Azure aboneliğinde mevcut Azure için ayrıntılı erişim yönetimi sağlayarak paylaşmanıza yardımcı olur. Azure RBAC, kuruluşunuzdaki görevlerini ayırmak ve kullanıcıların işlerini yapmak gerekenler üzerinde tabanlı erişim sağlar. İzinleri vermek yerine herkese sınırsız Azure abonelik veya kaynak, yalnızca belirli eylemleri izin verebilirsiniz.

Azure RBAC, tüm kaynak türleri için geçerli olan üç temel rolüne sahiptir:

- **Sahibi** tüm kaynaklara temsilci erişimi başkalarına hakkı dahil olmak üzere tam erişimi vardır.

- **Katkıda bulunan** olabilir oluşturun ve tüm Azure kaynakları türlerini yönetmek ancak diğerleri için erişim izni veremiyor.

- **Okuyucu** mevcut Azure kaynaklarını görebilirsiniz.

![Azure rol tabanlı erişim denetimi](./media/azure-isolation/azure-isolation-fig3.png)

Azure RBAC rolleri kalan belirli bir Azure kaynak yönetimi sağlar. Örneğin, sanal makine Katılımcısı rolü oluşturmak ve sanal makineleri yönetmek kullanıcı sağlar. Bunları erişim için Azure sanal ağı veya sanal makineye bağlanan alt vermez.

[RBAC yerleşik rolleri](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) Azure'da kullanılabilen roller listesinde. Bu işlemler ve yerleşik her rol için kullanıcılara veren kapsamı belirtir. Daha fazla denetim için kendi rolleri tanımlamak için arıyorsanız, nasıl oluşturulduğunu görün [Azure rbac'de özel roller](https://docs.microsoft.com/azure/role-based-access-control/custom-roles).

Azure Active Directory için diğer özelliklerinden bazıları şunlardır:
- Azure AD SSO SaaS uygulamalarına burada barındırılır sağlar. Bazı uygulamalar Azure AD federasyonu kullanırken diğerleri parola SSO hizmetinden yararlanır. Federasyon uygulamaları, kullanıcı sağlamayı da destekleyebilir ve [parola kasası oluşturma](https://www.techopedia.com/definition/31415/password-vault).

- [Azure Storage](https://azure.microsoft.com/services/storage/) verilerine erişim, kimlik doğrulaması ile denetlenir. Her Depolama hesabı, birincil anahtar sahiptir ([depolama hesabı anahtarı](https://docs.microsoft.com/azure/storage/storage-create-storage-account), veya SAK) ve ikincil bir gizli anahtar (paylaşılan erişim imzası veya SAS).

- Azure AD sağlayan kimlik Federasyon üzerinden hizmet olarak kullanarak [Active Directory Federasyon Hizmetleri](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-azure-adfs), eşitleme ve çoğaltma şirket içi dizinlerle.

- [Azure multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) oturum açma işlemleri bir mobil uygulama, telefon araması veya kısa mesaj kullanarak doğrulamasını gerektiren çok faktörlü kimlik doğrulama hizmetidir. Bu Azure AD ile Azure multi-Factor Authentication sunucusu ile ve özel uygulamalar ve dizinler SDK'sını kullanarak ile güvenli şirket içi kaynaklara yardımcı olmak için kullanılabilir.

- [Azure AD etki alanı Hizmetleri](https://azure.microsoft.com/services/active-directory-ds/) , etki alanı denetleyicilerini dağıtmaya gerek kalmadan Azure sanal makineleri bir Active Directory etki alanına eklemenize olanak tanır. Bu sanal makinelerde Kurumsal Active Directory kimlik bilgilerinizle oturum açın ve tüm Azure sanal makineleri temel güvenlik özelliklerinizin zorlamak için Grup İlkesi kullanarak etki alanına katılmış sanal makineleri yönetme.

- [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) yüz milyonlarca kimliğe kadar ölçeklendirebileceğiniz müşteri uygulamaları için bir yüksek oranda kullanılabilir kimlik genel yönetim hizmeti sağlar. Bu hizmet mobil platformlar ve web platformlarıyla tümleştirilebilir. Tüketicileriniz için özelleştirilebilir bir deneyimle uygulamalarınızda ister mevcut sosyal hesaplarını kullanarak veya kimlik bilgileri oluşturarak oturum açabilir.

### <a name="isolation-from-microsoft-administrators--data-deletion"></a>Yalıtım Microsoft Administrators & veri silme
Microsoft, sağlam ölçüler verilerinizi uygunsuz erişiminden koruyun veya yetkisiz kişiler tarafından alır. Bu çalışma süreçleri ve denetimleri tarafından yedeklenen [çevrimiçi hizmet koşulları](https://aka.ms/Online-Services-Terms), verilerinize erişimi yöneten sözleşmeye dayalı taahhütleri sunar.

-   Microsoft mühendisleri varsayılan verilerinize bulutta erişiminiz yok. Bunun yerine, yalnızca gerektiğinde yönetim gözetim altında erişim izni verilir. Bu erişim dikkatli bir şekilde denetlenir ve günlüğe ve artık gerekli olmadığında iptal.

-   Microsoft, kendi adımıza sınırlı hizmetler sağlaması için diğer şirketlerle anlaşabilir. Alt yüklenicilerin, hizmetleri, kendilerini sağlamak için görevlendirdiğimiz yalnızca sunmak için müşteri verilerine erişebilir ve onu başka bir amaç için kullanmaları yasaktır. Ayrıca, bunlar sözleşmeye dayalı olarak müşterilerimizin bilgilerinin gizliliğini korumak için bağlıdır.

ISO/IEC 27001 gibi denetlenen sertifikaları ile Kurumsal hizmetlerini, Microsoft ve işletme amacıyla yalnızca bu erişim gerçekleştiğini doğrulamak için örnek denetimler gerçekleştirir, akredite edilmiş denetim firmaları tarafından düzenli olarak doğrulanır. Her zaman, dilediğiniz zaman ve herhangi bir nedenle kendi müşteri verilerinize erişebilirsiniz.

Herhangi bir veri silerseniz, Microsoft Azure önbelleğe alınmış veya yedek kopyalar içeren verileri siler. Kapsamdaki hizmetler için saklama süresi dolduktan sonra 90 gün içinde silme işlemi gerçekleşir. (Kapsamındaki hizmetler veri işleme koşulları bölümünde tanımlanmış bizim [çevrimiçi hizmet koşulları](https://aka.ms/Online-Services-Terms).)

Depolama için kullanılan bir disk sürücüsü bir donanım hatası nedeniyle düşerse, güvenli bir şekilde olduğu [silinmesi veya yok](https://microsoft.com/trustcenter/privacy/you-own-your-data) önce Microsoft değiştirme veya onarım için üreticinin döndürür. Herhangi bir yolla veriler kurtarılamaz emin olmak için sürücüdeki verilerin üzerine yazılır.

## <a name="compute-isolation"></a>İşlem yalıtım
Microsoft Azure, geniş bilgi işlem örnekleri içeren çeşitli bulut tabanlı bilgi işlem Hizmetleri ve yukarı ve aşağı otomatik olarak uygulama veya Kurumsal ihtiyaçlarını karşılamak üzere ölçeklenebilir hizmetler sağlar. Bu işlem örneği ve hizmet veri yapılandırmasında esneklik ödün vermeden, müşterilerinizin talep ettiği güvenli hale getirmek için birden fazla düzeyde yalıtım sağlar.

### <a name="isolated-virtual-machine-sizes"></a>Ayrılmış sanal makine boyutları
Azure Compute, belirli bir donanım türüyle sınırlanmış ve tek bir müşteriye ayrılmış olan sanal makine boyutları sunar.  Bu sanal makine boyutları, uyumluluk ve düzenleme gereksinimleri gibi öğeler içeren iş yükleri nedeniyle diğer müşterilerden yüksek ölçüde yalıtıma ihtiyaç duyan iş yükleri için idealdir.  Müşteriler daha fazla yalıtılmış bu sanal makinelerin kaynakları kullanarak alt bölümlere ayırmak de seçebilir [iç içe sanal makineleri için Azure desteği](https://azure.microsoft.com/blog/nested-virtualization-in-azure/).

Yalıtılmış bir boyut yararlanarak tek, belirli sunucu örneğinde çalışan sanal makinenizi olacağını garanti eder.  Geçerli sanal makine yalıtılmış teklifler şunları içerir:
* Standard_E64is_v3
* Standard_E64i_v3
* İşler için standart_m128ms
* Standard_GS5
* Standard_G5
* Standard_DS15_v2
* Standard_D15_v2
* Standard_F72s_v2

Her bir yalıtılmış boyutu kullanılabilir hakkında daha fazla bilgi [burada](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory).

### <a name="hyper-v--root-os-isolation-between-root-vm--guest-vms"></a>Kök VM ve Konuk Vm'leri arasında Hyper-V & kök işletim sistemi yalıtımı
Azure'nın bilgi işlem platformu üzerinde makine sanallaştırma tabanlı — bir Hyper-V sanal makinesinde tüm müşteri kodu yürütür anlamına gelir. Her Azure düğümlerindeki (veya ağ uç noktası), doğrudan donanımın üzerinde çalışır ve değişken sayıya Konuk sanal makineleri (VM'ler), bir düğüm ayıran bir hiper yoktur.


![Kök VM ve Konuk Vm'leri arasında Hyper-V & kök işletim sistemi yalıtımı](./media/azure-isolation/azure-isolation-fig4.jpg)


Bir özel kök konak işletim sistemi çalıştıran VM, her düğümü de var. Kritik bir sınır kök VM'yi Konuk Vm'lerden ve Konuk Vm'leri birbirinden, hiper yönetici ve kök işletim sistemi tarafından yönetilen yalıtımının ' dir. Eşleştirme hiper yönetici/kök işletim sistemi, işletim sistemi güvenlik deneyimi ve Microsoft Hyper-v'den Konuk VM'lerin güçlü yalıtım sağlamak için daha yeni öğrenme Microsoft'un yıllardır yararlanır.

Azure platformu, sanallaştırılmış bir ortam kullanır. Kullanıcı örnekleri, bir fiziksel ana bilgisayar sunucusuna erişimi olmayan tek başına sanal makineler olarak çalışır.

Azure hiper Yöneticisi mikro çekirdek gibi davranır ve tüm donanım erişim isteklerini Konuk sanal makinelerden işleme için ana VMBus adlı bir paylaşılan bellek arabirimi kullanılarak geçirir. Bu, kullanıcıların sistemde ham okuma/yazma/yürütme erişimine sahip olmasını önler ve sistem kaynaklarının paylaşımıyla ilgili riskleri azaltır.

### <a name="advanced-vm-placement-algorithm--protection-from-side-channel-attacks"></a>Gelişmiş VM yerleştirme algoritması & yan kanal saldırılarına karşı koruma
Çapraz-VM saldırı iki adımdan oluşur: victim Vm'leri biri olarak aynı ana bilgisayardaki bir saldırgan tarafından denetlenen VM yerleştirme ve ardından kurban hassas bilgileri çalan veya greed veya vandalism performansını etkileyen yalıtım sınırı ihlal. Microsoft Azure, Gelişmiş bir VM yerleştirme algoritması ve gürültülü komşu Vm'leri de dahil olmak üzere tüm bilinen yan kanal saldırılarına karşı koruma kullanarak her iki adım, koruma sağlar.

### <a name="the-azure-fabric-controller"></a>Azure yapı denetleyicisi
Azure yapı denetleyicisi altyapı kaynaklarını Kiracı iş yüklerini sorumlu olan ve tek yönlü iletişim ana bilgisayar ile sanal makineleri yönetir. VM yerleştirme algoritmasının Azure yapı denetleyicisi, son derece Gelişmiş ve fiziksel ana bilgisayar düzeyinde tahmin etmek neredeyse imkansızdır.

![Azure yapı denetleyicisi](./media/azure-isolation/azure-isolation-fig5.png)

Azure hiper Yöneticisi, sanal makineler arasında bellek ve işlem ayrımı yapılmasını zorunlu kılar ve güvenli bir şekilde konuk işletim sistemi kiracılarına için ağ trafiğini yönlendirir. Bu olasılığını ve VM düzeyinde yan kanal saldırıya ortadan kaldırır.

Azure'da VM kök özeldir: bir sağlamlaştırılmış işletim kök işletim sistemi olarak adlandırılan bir yapı aracısı (FA) barındıran sistemini çalıştırır. FAs sırayla Konuk aracıları (GA) yönetmek için müşteri Vm'leri konuk işletim sistemleri içinde kullanılır. FAs, ayrıca depolama düğümleri yönetin.

Azure hiper yönetici topluluğu kök işletim sistemi/FA ve müşteri Vm'leri/gaz bir işlem düğümünde oluşur. FAs, işlem ve depolama düğümleri (işlem ve depolama kümeleri ayrı FCs tarafından yönetilir) dışında var olan bir yapı denetleyicisi (FC) tarafından yönetilir. Bir müşteri, uygulamanın yapılandırma dosyası güncelleştiriyorsa çalışırken FC, yapılandırma değişikliği uygulamaya bildirmek ardından gaz kişiler, SK ile iletişim kurar. Donanım arızası olması durumunda, FC otomatik olarak kullanılabilir donanım bulun ve var olan VM'yi yeniden başlatın.

![Azure yapı denetleyicisi](./media/azure-isolation/azure-isolation-fig6.jpg)

Bir yapı denetleyicisi yönelik bir aracı iletişim tek yönlüdür. Aracı yalnızca denetleyicisinden isteklerine yanıt verip SSL korumalı bir hizmet uygular. Bu denetleyici veya ayrıcalıklı diğer iç düğümlerin bağlantılarını başlatamazsınız. Güvenilmeyen oldukları gibi tüm yanıtları FC değerlendirir.


![Yapı denetleyicisi](./media/azure-isolation/azure-isolation-fig7.png)

Konuk Vm'lerden ve Konuk Vm'leri birbirinden kök VM'den gelen yalıtım genişletir. İşlem düğümleri artan koruma için depolama düğümlerden de yalıtılır.


Hiper Yönetici ve ağ paketi - güvenilmeyen sanal makineler sahte trafik oluşturma olamaz veya kendilerine gönderilmeyen trafiği alma olmalarını sağlamak için filtreler işletim sistemi sağlayan konak trafiği korumalı altyapı uç noktalarına yönlendirmek veya gönderme ve alma uygun olmayan yayın trafiğini.


### <a name="additional-rules-configured-by-fabric-controller-agent-to-isolate-vm"></a>VM yalıtmak için yapı denetleme aracısı tarafından yapılandırılan ek kurallar
Bir sanal makine oluşturulduğunda ve yapı denetleme Aracısı kuralları ve yetkili trafiğe izin veren özel durumlar eklemek için paket filtresini ardından yapılandırır, varsayılan olarak, tüm trafik engellenir.

Programlanmış kuralları iki kategorisi vardır:

-   **Makine Yapılandırması veya altyapı kuralları:** Varsayılan olarak, tüm iletişim engellenir. Sanal makine DHCP ve DNS trafik gönderip alabilmesine izin veren özel durumlar vardır. Sanal makineler ayrıca "Genel" internet'e trafiği göndermek ve diğer sanal makinelerde aynı Azure sanal ağı ve işletim sistemi Etkinleştirme sunucusu trafiği gönderir. İzin verilen giden hedefleri sanal makinelerin listesini Azure yönlendirici alt ağları, Azure yönetim ve diğer Microsoft özellikleri içermez.

-   **Rol yapılandırma dosyası:** Bu, gelen erişim denetimi kiracının hizmet modelini temel alarak listeleri (ACL'ler) tanımlar.

### <a name="vlan-isolation"></a>VLAN yalıtımı
Her kümedeki üç VLAN'ları vardır:

![VLAN yalıtımı](./media/azure-isolation/azure-isolation-fig8.jpg)


-   Ana VLAN – eşitliyor güvenilmeyen müşteri düğümleri

-   Güvenilen FCs ve destekleyici sistemlere – FC VLAN içerir

-   VLAN – cihazın güvenilir ağ ve diğer altyapı cihazlarındaki içerir

İletişim FC VLAN'ı ana VLAN için izin verilir, ancak ana VLAN'ı FC VLAN başlatılamaz. İletişim, cihaza VLAN ana VLAN'ı da engellenir. Bu, bir düğümde çalışan müşteri kod güvenliği bozulsa bile, düğümler FC veya VLAN'ları cihaz üzerinde saldırı olamaz emin olmasını sağlar.

## <a name="storage-isolation"></a>Depolama ayırma
### <a name="logical-isolation-between-compute-and-storage"></a>İşlem ve depolama arasında mantıksal yalıtım
Temel tasarımını bir parçası olarak, Microsoft Azure depolama biriminden sanal makine tabanlı hesaplama ayırır. Bu ayrım, çok kiracılı ve yalıtım sağlamak kolaylaştırarak hesaplama ve depolamayı birbirinden bağımsız olarak ölçeklendirme sağlar.

Bu nedenle, Azure depolama ayrı donanımda ile bir ağ bağlantısı için Azure işlem dışında mantıksal olarak çalıştırılır. [Bu](https://msenterprise.global.ssl.fastly.net/vnext/PDFs/A01_AzureSecurityWhitepaper20160415c.pdf) bir sanal disk oluşturulduğunda, disk alanı için tüm kapasitesi atanmadı anlamına gelir. Bunun yerine, sanal diski adreslerinde alanları fiziksel disk üzerinde eşleşen bir tablo oluşturulur ve bu başlangıçta boş bir tablodur. **Bir müşteri sanal diskte, verileri yazar ilk kez fiziksel disk alanı ayrılır ve tabloda bir işaretçiye yerleştirilir.**
### <a name="isolation-using-storage-access-control"></a>Yalıtım kullanılarak depolama erişim denetimi
**Erişim denetimi Azure Depolama'da** basit erişim denetimi modeli vardır. Her Azure aboneliğinin bir veya daha fazla depolama hesabı oluşturabilirsiniz. Her Depolama hesabı, bu depolama hesabındaki tüm verilere erişimi denetlemek için kullanılan tek bir gizli anahtar içeriyor.

![Yalıtım kullanılarak depolama erişim denetimi](./media/azure-isolation/azure-isolation-fig9.png)

**Azure depolama veri (tablolar da dahil olmak üzere) erişim** aracılığıyla denetlenebilir bir [SAS (paylaşılan erişim imzası)](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) hangi verir kapsamlı erişim belirteci. SAS ile imzalanmış bir sorgu şablonu (URL) aracılığıyla oluşturulan [SAK (depolama hesabı anahtarı)](https://msdn.microsoft.com/library/azure/ee460785.aspx). [URL imzalı](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) sonra sorgu ayrıntıları girin ve istekte depolama hizmetinin (temsilcisi) başka bir işleme, verilebilir. Bir SAS depolama hesabının gizli anahtar göstermeden istemcilere zamana dayalı erişim vermenize olanak sağlar.

SAS anlamına gelir ki istemci depolama hesabımız ve belirtilen bir izin kümesi ile belirli bir süre için nesneleri, izinleri sınırlı verebilirsiniz. Hesap erişim anahtarlarınızı paylaşmak zorunda kalmadan Biz bu sınırlı izinler verebilirsiniz.

### <a name="ip-level-storage-isolation"></a>IP düzeyi depolama ayırma
Güvenlik duvarları oluşturmak ve güvenilen istemcileriniz için bir IP adresi aralığı tanımlayın. IP adresi tanımlı aralığın içinde olan istemcilerin bağlanabileceği bir IP adresi aralığı [Azure depolama](https://docs.microsoft.com/azure/storage/storage-security-guide).

IP depolama veri, yetkisiz kullanıcıların adanmış veya ayrılmış bir tünel IP depolama trafiğinin ayırmak için kullanılan ağ bir mekanizma aracılığıyla korunabilir.

### <a name="encryption"></a>Şifreleme
Azure aşağıdaki verileri korumak için şifreleme türlerini sunar:
-   Aktarım sırasında şifreleme

-   Bekleme sırasında şifreleme

#### <a name="encryption-in-transit"></a>Aktarım sırasında şifreleme
Aktarım sırasında şifreleme ağlar üzerinden iletilirken, veri koruma, bir mekanizmadır. Azure Storage ile verileri kullanarak güvenli hale getirebilirsiniz:

-   [Aktarım düzeyinde şifreleme](https://docs.microsoft.com/azure/storage/storage-security-guide#encryption-in-transit), Azure depolama içine veya dışına veri aktarımı HTTPS gibi.

-   [Şifreleme wire](../storage/common/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), Azure dosya paylaşımları için SMB 3.0 şifreleme gibi.

-   [İstemci tarafı şifreleme](https://docs.microsoft.com/azure/storage/storage-security-guide#using-client-side-encryption-to-secure-data-that-you-send-to-storage), verileri şifrelemenizi depolama alanına aktarılır ve depolama alanınız aktarıldıktan sonra verilerin şifresini çözmek için.

#### <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme
Birçok kuruluşta [bekleyen verileri şifreleme](https://docs.microsoft.com/azure/security/azure-isolation) veri gizlilik, uyumluluk ve veri egemenliği doğrultusunda zorunlu bir adımdır. "Beklemede" olan verilerin şifrelenmesini sağlayan üç Azure özellikleri vardır:

-   [Depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-security-guide#encryption-at-rest) depolama hizmeti otomatik olarak verileri Azure depolama alanına yazarken şifrelemeniz istemenizi sağlar.

-   [İstemci tarafı şifreleme](https://docs.microsoft.com/azure/storage/storage-security-guide#client-side-encryption) bekleyen şifreleme özelliği de sağlar.

-   [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) işletim sistemi diskleri ve veri diskleri bir Iaas sanal makine tarafından kullanılan şifreleme sağlar.

#### <a name="azure-disk-encryption"></a>Azure Disk Şifrelemesi
[Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) sanal makineler (VM) için Kurumsal güvenlik ve uyumluluk gereksinimlerini (önyükleme ve veri diskleri dahil) sanal makine disklerinizi şifreleyerek anahtarları ve ilkeleri denetlemek de size yardımcı olur [Azure anahtarı Kasa](https://azure.microsoft.com/services/key-vault/).

Windows için Disk şifrelemesi çözümü dayanır [Microsoft BitLocker Sürücü Şifrelemesi](https://technet.microsoft.com/library/cc732774.aspx), ve Linux çözüm dayanır [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt).

Microsoft Azure'da etkinleştirildiğinde çözüm Iaas Vm'leri için aşağıdaki senaryoları destekler:
-   Azure Key Vault ile tümleştirme

-   Standart katmanı Vm'lerini: A, D, DS, G, GS ve benzeri serisi Iaas Vm'leri

-   Windows ve Linux Iaas Vm'lerinde şifrelemesini etkinleştirme

-   Windows Iaas Vm'leri için sürücüler işletim sistemi ve veri şifrelemeyi devre dışı bırakma

-   Linux Iaas sanal makineler için veri sürücülerini şifreleme devre dışı bırakma

-   Windows istemci işletim sistemi çalıştıran bir Iaas Vm'leri şifrelemesini etkinleştirme

-   Bağlama yolu içeren birimlerde şifrelemesini etkinleştirme

-   Kullanarak şifreleme (RAID) bölümlenerek diskle yapılandırılmış Linux vm'lerinde etkinleştirme [mdadm](https://en.wikipedia.org/wiki/Mdadm)

-   Kullanarak Linux VM üzerinde şifrelemeyi etkinleştirme [LVM (mantıksal birim Yöneticisi)](https://msdn.microsoft.com/library/windows/desktop/bb540532) veri diskleri için

-   Depolama alanları kullanılarak yapılandırılan Windows Vm'leri üzerinde şifrelemeyi etkinleştirme

-   Tüm genel Azure bölgelerinde desteklenir

Çözüm aşağıdaki senaryoları, özellikleri ve teknoloji sürümde desteklemez:

-   Temel katman Iaas Vm'leri

-   Linux Iaas Vm'leri için şifreleme bir işletim sistemi sürücüsünde devre dışı bırakma

-   Iaas VM'ler, Klasik VM oluşturma yöntemini kullanarak oluşturulur

-   Şirket içi anahtar yönetimi hizmeti ile tümleştirme

-   Azure dosyaları (paylaşılan dosya sistemi), ağ dosya sistemi (NFS), dinamik birimler ve yazılım tabanlı RAID sistemler ile yapılandırılan Windows Vm'leri

## <a name="sql-azure-database-isolation"></a>SQL Azure veritabanı yalıtım
SQL Veritabanı, piyasa lideri Microsoft SQL Server altyapısını temel alan ve görev açısından kritik iş yüklerini üstlenebilen, Microsoft bulutu tabanlı bir ilişkisel veritabanı hizmetidir. SQL veritabanı, tahmin edilebilir veri yalıtımı hesap düzeyinde Coğrafya sunar / bölge ve ağ üzerinde dayalı — neredeyse sıfır yönetim gereksinimiyle.

### <a name="sql-azure-application-model"></a>SQL Azure uygulama modeli

[Microsoft SQL Azure](https://docs.microsoft.com/azure/sql-database/sql-database-get-started) veritabanı, SQL Server teknolojileri üzerine kurulmuş bir bulut tabanlı bir ilişkisel veritabanı hizmetidir. Bu, Microsoft bulut tarafından barındırılan bir yüksek oranda kullanılabilir, ölçeklenebilir, çok kiracılı veritabanı hizmeti sağlar.

Bir uygulamadan perspektif SQL Azure aşağıdaki hiyerarşi sağlar: Her düzey aşağıdaki düzeylerinin bir çok kapsama sahiptir.

![SQL Azure uygulama modeli](./media/azure-isolation/azure-isolation-fig10.png)

Hesap ve abonelik yönetimi ilişkilendirmek için Microsoft Azure platformu kavramlardır.

Mantıksal sunucuları ve veritabanlarını SQL Azure özgü kavramlar ve arabirimleri OData ve TSQL sağlanan SQL Azure kullanarak veya Azure portalında tümleşik SQL Azure Portalı aracılığıyla yönetilir.

SQL Azure sunucular fiziksel veya sanal makine örnekleri değildir, bunun yerine, veritabanları, böylece çağrılan "mantıksal ana dalda" depolanır, yönetim ve güvenlik ilkeleriyle paylaşımı koleksiyonlarıdır veritabanı.

![SQL Azure](./media/azure-isolation/azure-isolation-fig11.png)

Mantıksal asıl veritabanını içerir:

-   Sunucuya bağlanmak için kullanılan SQL oturum açma bilgileri

-   Güvenlik duvarı kuralları

Aynı mantıksal sunucu veritabanlarından SQL Azure kümesindeki aynı fiziksel örneğinde bunun yerine uygulamalar olması garanti edilmez fatura ve kullanım ile ilgili bilgi için SQL Azure, bağlanırken hedef veritabanı adı sağlamanız gerekir.

Sunucunun gerçek oluşturulmasını bölge kümelerini biriyle gerçekleşirken Müşteri açısından bakıldığında, bir mantıksal sunucu bir grafik coğrafi bölgede oluşturulur.

### <a name="isolation-through-network-topology"></a>Ağ topolojisi için yalıtımı

Bir mantıksal sunucu oluşturduğunuzda ve DNS adı kayıt noktaları sunucu yerleştirildiği özel veri merkezi şekilde çağrılan "Ağ geçidi VIP" adresine DNS adı.

(Sanal IP adresi) VIP durum bilgisi olmayan bir ağ geçidi Hizmetleri koleksiyonunu sahibiz. Koordinasyon birden çok veri kaynağı arasında (ana veritabanı, bir kullanıcı veritabanı, vb.) gerekli olduğunda, genel olarak, ağ geçitleri dahil. Ağ Geçidi Hizmetleri aşağıdakileri uygulayın:
-   **TDS bağlantı proxy.** Bu, bir kullanıcı veritabanı arka ucu kümede bulma, oturum sırasını uygulama ve ardından arka uç ve arka TDS paketlere iletme'yi içerir.

-   **Veritabanı yönetimi.** Bu uygulama koleksiyonu CREATE/ALTER/DROP veritabanı işlemleri yapmak için iş akışlarını içerir. Veritabanı işlemleri algılaması TDS paketleri veya açık OData API'leri tarafından çağrılabilir.

-   CREATE/ALTER/DROP oturum açma/kullanıcı işlemleri

-   OData API aracılığıyla mantıksal sunucu yönetimi işlemleri

![Ağ topolojisi için yalıtımı](./media/azure-isolation/azure-isolation-fig12.png)

Ağ geçidi arkasında katmanı "arka uç" adı verilir. Yüksek oranda kullanılabilir bir biçimde tüm verilerin depolandığı budur. Her veri parçası ait bir "Bölüm" kabul edilir veya "Yük devretme birimi", her biri en az üç kopyaya sahip. Çoğaltmaları depolanan ve SQL Server altyapısı tarafından çoğaltılır ve genellikle "yapı" adlandırılan bir yük devretme sistemi tarafından yönetilir.

Genellikle, arka uç sistemine giden diğer sistemlere bir güvenlik önlemi olarak iletişim kurmaz. Bu, ön uç (ağ geçidi) katmanında sistemleri için ayrılmıştır. Ağ geçidi katmanı makinelerinin savunma mekanizması olarak saldırı yüzeyini en aza indirmek için arka uç makinelerde ayrıcalıkları sınırlı.

### <a name="isolation-by-machine-function-and-access"></a>Makine işlevi ve erişim göre yalıtım
SQL Azure (farklı makine işlevler üzerinde çalışan hizmetleri oluşur. SQL Azure ortamlar ile genel trafiği yalnızca giderek kullanıma değil ve arka uç İlkesi "arka uç", bulut veritabanı ve "ön uç" (ağ geçidi/Yönetimi) halinde bölünmüştür. Ön uç ortam başka bir hizmetler dış dünyaya iletişim kurabilir ve genel olarak, yalnızca arka uç (çağırmak için gereken giriş noktalarını çağırması için yeterli) izinleri sınırlıdır.

## <a name="networking-isolation"></a>Ağ yalıtımı
Azure dağıtım, birden çok ağ yalıtımı katmanı vardır. Aşağıdaki diyagram, Azure müşterilerine yönelik ağ yalıtımı çeşitli katmanları gösterilmektedir. Bu, hem yerel, Azure platformunda hem de müşteri tarafından tanımlanan özellikler katmanlardır. Gelen Internet'ten Azure DDoS büyük ölçekli Azure saldırılara karşı yalıtım sağlar. Sonraki katmanı yalıtımı, hangi trafik bulut hizmeti aracılığıyla sanal ağa geçirebilirsiniz belirlemek için kullanılan müşteri tarafından tanımlanan genel IP adresleri (Bitiş) ' dir. Yerel Azure sanal tam bir yalıtım diğer tüm ağlardan ağ yalıtımı sağlar ve trafik yalnızca kullanıcı tarafından yapılandırılmış yolları ve yöntemleri akar. Bu yolları ve yöntemleri sonraki katmanı burada Nsg'ler, UDR ve ağ sanal Gereçleri korumalı ağ uygulama dağıtımları korumak, yalıtım sınırlarını oluşturmak için kullanılabilir olan.

![Ağ yalıtımı](./media/azure-isolation/azure-isolation-fig13.png)

**Trafik yalıtımı:** A [sanal ağ](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) Azure platformunda trafik yalıtımı sınırıdır. Her iki sanal ağ aynı müşteri tarafından oluşturulmamış olsa bile sanal makineleri (VM'ler) bir sanal ağdaki farklı bir sanal ağ içindeki VM'ler için doğrudan iletişim kuramaz. Yalıtım müşteri Vm'leri sağlar önemli bir özelliktir ve bir sanal ağ içindeki özel iletişimin kalır.

[Alt ağ](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) yalıtımı ile sanal ağ IP aralığı tabanlı bir ek katmanı sunar. Sanal ağdaki IP adresleri, bir sanal ağı organizasyon ve güvenlik için birden çok alt ağa bölebilirsiniz. Bir sanal ağ içindeki alt ağlara (aynı veya farklı) dağıtılan VM'ler ve PaaS rolü örnekleri, ek bir yapılandırma gerektirmeden birbirleriyle iletişim kurabilir. Ayrıca [ağ güvenlik grubu (Nsg)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) veya bir VM örneğine erişim denetim listesindeki NSG (ACL) yapılandırılmış kurallarına göre ağ trafiği reddetmek için. NSG'ler alt ağlarla veya bu alt ağların içindeki tekil VM örnekleriyle ilişkili olabilir. NSG bir alt ağ ile ilişkili olduğunda ACL kuralları bu alt ağdaki tüm VM örnekleri için geçerli olur.

## <a name="next-steps"></a>Sonraki Adımlar

- [Ağ yalıtımı seçenekleri Windows makineleri için Azure sanal ağları](https://azure.microsoft.com/blog/network-isolation-options-for-machines-in-windows-azure-virtual-networks/)

Bu, burada makineler bir özel arka uç ağ veya alt ağ yalnızca belirli istemciler veya diğer bilgisayarların bir beyaz liste IP adreslerinin üzerinde dayalı belirli bir uç noktaya bağlanmasını izin verebileceği Klasik ön uç ve arka uç senaryoları içerir.

- [İşlem yalıtım](https://msenterprise.global.ssl.fastly.net/vnext/PDFs/A01_AzureSecurityWhitepaper20160415c.pdf)

Microsoft Azure, geniş bilgi işlem örnekleri içeren bir çeşitli bulut tabanlı bilgi işlem Hizmetleri ve yukarı ve aşağı otomatik olarak uygulama veya Kurumsal ihtiyaçlarını karşılamak üzere ölçeklenebilir hizmetler sağlar.

- [Depolama ayırma](https://msenterprise.global.ssl.fastly.net/vnext/PDFs/A01_AzureSecurityWhitepaper20160415c.pdf)

Microsoft Azure depolama alanından müşteri sanal makine tabanlı hesaplama ayırır. Bu ayrım, çok kiracılı ve yalıtım sağlamak kolaylaştırarak hesaplama ve depolamayı birbirinden bağımsız olarak ölçeklendirme sağlar. Bu nedenle, Azure depolama ayrı donanımda ile bir ağ bağlantısı için Azure işlem dışında mantıksal olarak çalıştırılır. Tüm istekler, HTTP veya HTTPS müşterinin tercihine bağlı üzerinden çalıştırın.

