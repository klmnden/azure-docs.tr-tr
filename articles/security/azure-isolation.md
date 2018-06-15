---
title: Azure genel bulutunda yalıtım | Microsoft Docs
description: Geniş işlem örnekleri dahil bulut tabanlı bilgi işlem Hizmetleri & Yukarı ve aşağı otomatik olarak uygulamanızı veya Kurumsal ihtiyaçlarını karşılamak üzere ölçeği hizmetleri hakkında bilgi edinin.
services: security
documentationcenter: na
author: UnifyCloud
manager: mbaldwin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomSh
ms.openlocfilehash: 6f01c2938462f3912928e183fcec215a52a3ee48
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "34010889"
---
# <a name="isolation-in-the-azure-public-cloud"></a>Azure genel bulutunda yalıtımı
##  <a name="introduction"></a>Giriş
### <a name="overview"></a>Genel Bakış
Geçerli ve gelecekteki Azure yardımcı olmak için müşteriler anlamak ve çeşitli güvenlikle ilgili bulunan becerilerinden ve Azure platformu çevreleyen, Microsoft teknik incelemeler, güvenlik genel bakışlar, en iyi yöntemler, bir dizi geliştirmiştir ve Denetim listeleri.
Konular avantajlarına ve derinliği bakımından aralığı ve düzenli aralıklarla güncelleştirilir. Bu belge, Özet bölümü aşağıda özetlendiği gibi serisi bir parçası değil.

### <a name="azure-platform"></a>Azure platformu
Azure işletim sistemlerinin programlama dilleri, çerçeveleri, Araçlar, veritabanları ve cihazları, en geniş seçim destekleyen bir açık ve esnek bir bulut hizmeti platformudur. Örneğin, şunları yapabilirsiniz:
- Linux kapsayıcıları Docker Tümleştirmesi ile çalıştırın;
- JavaScript, Python, .NET, PHP, Java ve Node.js ile uygulamalar oluşturma; ve
- Yapı geri-iOS, Android ve Windows cihazları sona erer.

Microsoft Azure aynı teknolojileri geliştiriciler milyonlarca destekler ve BT uzmanları zaten kullanır ve güven.

Oluşturmanıza veya BT varlıklarına geçirmek, genel bulut hizmeti sağlayıcı, uygulamalar ve hizmetler ve bulut tabanlı varlıklarınızı güvenliği yönetmek için sağladıkları denetimleri ile verileri korumak için bir kuruluşun yeteneklerini öğesine bağlı.

Azure altyapısı tesisten uygulamalara kadar milyonlarca müşteriye aynı anda hizmet verecek şekilde tasarlanmıştır ve işletmelerin güvenlik ihtiyaçlarını karşılayabilecek güvenilir bir temel sunar. Buna ek olarak, Azure’da çok çeşitli ve yapılandırılabilir güvenlik seçenekleri ile bunlar üzerinde denetim imkanı sunulmaktadır. Böylece, dağıtımlarınıza özel gereksinimleri karşılamak için güvenlik özelliklerini uyarlayabilirsiniz. Bu belge, bu gereksinimleri karşılayan yardımcı olur.

### <a name="abstract"></a>Özet

Microsoft Azure, uygulamalar ve sanal makineleri (VM'ler) paylaşılan fiziksel altyapı üzerinde çalıştırmasına olanak sağlar. Uygulamalar bulut ortamında çalıştırmanın ekonomik prime sözleri de birden çok müşteri arasında paylaşılan kaynakların maliyeti dağıtmak yeteneğidir. Bu yöntem, çok kiracılı çoğullama kaynaklar arasında düşük maliyetler farklı müşterilerine tarafından verimliliği artırır. Ne yazık ki, aynı zamanda fiziksel sunucuları ve diğer altyapı kaynakları duyarlı uygulamalar ve rasgele ve kötü amaçlı bir kullanıcı ait olabilir sanal makineleri çalıştırmak için paylaşımı riski sunar.

Bu makalede, Microsoft Azure kötü amaçlı ve kötü amaçlı olmayan kullanıcılara karşı yalıtımı sağlar ve bulut çözümleri mimarları için çeşitli yalıtım seçenekleri sunarak mimariden için bir kılavuz olarak hizmet veren nasıl özetlenmektedir. Bu teknik incelemede Azure platformu ve müşteri dönük güvenlik denetimleri teknolojisine odaklanır ve fiyatlandırma modelleri ve DevOps yöntem değerlendirmeleri adresi SLA için çalışmaz.

## <a name="tenant-level-isolation"></a>Kiracı düzeyinde yalıtım
Paylaşılan ortak bir altyapının çeşitli müşterilerin arasında aynı anda kavramdır bilgi işlem, ölçek ekonomileri baştaki bulut birincil avantajlarından biri. Bu kavram çoklu kiracı adı verilir. Microsoft, Microsoft bulut Azure çok kiracılı mimarisini güvenlik, gizlilik, gizlilik, bütünlük ve kullanılabilirlik standartları desteklediğinden emin olmak için sürekli çalışmaktadır.

Bulutun etkin olduğu çalışma alanında kiracı, bulut hizmetinin belirli bir örneğine sahip olan ve o örneği yöneten bir istemci veya kuruluş olarak tanımlanabilir. Microsoft Azure tarafından sağlanan kimlik platformunda Kiracı yalnızca bir ayrılmış Azure Active kuruluşunuz alır ve bir Microsoft bulut hizmeti için oturum kaydolduğunda sahibi Directory (Azure AD) örneğidir.

Her Azure AD dizini, diğer Azure AD dizinlerinden farklı ve ayrıdır. Kurumsal ofis binasının yalnızca kuruluşunuza özel güvenilir bir varlık olması gibi, Azure AD dizini de yalnızca sizin kuruluşunuz tarafından kullanılmak üzere tasarlanan güvenilir bir varlıktır. Azure AD mimarisi, müşteri verilerini ve kimlik bilgilerini ortak karıştırma alanından yalıtır. Bu, bir Azure AD dizinindeki kullanıcıların ve yöneticilerin yanlışlıkla veya kötü amaçlı olarak başka bir dizindeki verilere erişemeyeceği anlamına gelir.

### <a name="azure-tenancy"></a>Azure kiralama
Azure kiralama (Azure abonelik) başvuran bir "Müşteri/faturalama" ilişki ve benzersiz bir [Kiracı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant) içinde [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis). Kiracı düzeyinde yalıtım Microsoft Azure, Azure Active Directory'yi kullanarak gerçekleştirilir ve [rol tabanlı denetimler](https://docs.microsoft.com/azure/role-based-access-control/overview) tarafından sunulan. Her Azure aboneliği bir Azure Active Directory (AD) dizin ile ilişkilendirilir.

Kullanıcılar, gruplar ve bu dizine uygulamalardan Azure aboneliğindeki kaynaklar yönetebilirsiniz. Azure portalı, Azure komut satırı araçları ve Azure Management API'leri kullanılarak bu erişim haklarını atayabilirsiniz. Azure AD kiracısı böylece hiçbir müşterinin erişmek veya kötü amaçlı olarak veya yanlışlıkla ortak kiracılar tehlikeye güvenlik sınırları kullanarak mantıksal olarak ayrı tutulur. İstenmeyen bağlantılar ve trafik burada ana bilgisayar düzeyinde paket filtreleme ve Windows Güvenlik Duvarı Engelleme "tam" sunuculara ayrı ağ kesiminde yalıtılmış Azure AD çalışır.

- Azure AD veri erişimi bir güvenlik belirteci hizmeti (STS) aracılığıyla kullanıcı kimlik doğrulaması gerektirir. Kullanıcının varlığı, etkin durumunu ve rol bilgilerini yetkilendirme sistem tarafından hedef Kiracı istenen erişimi bu kullanıcı için bu oturumda yetki verilip verilmediğini belirlemek için kullanılır.

![Azure kiralama](./media/azure-isolation/azure-isolation-fig1.png)


- Kiracılar ayrık kapsayıcılardır ve ilişkisi yoktur. Bunlar arasında.

- Kiracı yönetici Federasyon veya diğer kiracıdan kullanıcı hesapları sağlama aracılığıyla verir sürece üzerinden erişim yok Kiracı.

- Azure AD hizmeti oluşturan sunuculara fiziksel erişim ve Azure AD arka uç sistemleri, doğrudan erişim sınırlıdır.

- Azure AD kullanıcıları fiziksel varlık veya konumları erişiminiz yok ve bu nedenle bunları aşağıdaki belirtilen mantıksal RBAC İlkesi denetimleri atlamak mümkün değildir.

Tanılama ve Bakım gereksinimlerini için yalnızca zaman ayrıcalık yükseltme sistem kullanan bir çalışma modeline gereken ve kullanılır. Azure AD Privileged Identity Management (PIM) uygun bir yönetici kavramı sunar [Uygun admins](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) ayrıcalıklı olması gereken kullanıcılar erişim şimdi yapıp ancak her gün olmalıdır. Etkinleştirme işlemini tamamlamak ve önceden belirlenen bir süre için etkin bir yönetim hale sonra kullanıcı, erişmesi kadar rolü etkin değil.

![Azure AD Privileged Identity Management](./media/azure-isolation/azure-isolation-fig2.png)

Azure Active Directory ilkeler ve izinler için ve yalnızca ait ve Kiracı tarafından yönetilen kapsayıcı içinde kendi korumalı kapsayıcıdaki her bir kiracı barındırır.

Kiracı kapsayıcıları kavramı derine tüm katmanlar, bu süreç boyunca tüm kalıcı depolama alanına portalları konumundaki dizin hizmetindeki þeklimize.

Hatta birden çok Azure Active Directory Kiracı meta verilerini aynı fiziksel diskte depolandığında ilişkisi yoktur sırayla Kiracı Yöneticisi tarafından dikte edilir dizin hizmeti tarafından tanımlanan dışında kapsayıcıları arasında.

### <a name="azure-role-based-access-control-rbac"></a>Azure rol tabanlı erişim denetimi (RBAC)
[Azure rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) Azure için ayrıntılı erişim yönetimi sağlayarak bir Azure aboneliği kullanılabilen çeşitli bileşenler paylaşmanıza yardımcı olur. Azure RBAC kuruluşunuzdaki görevlerini kurabilmeleri ve kullanıcıların işlerini yapmak için gerekenler üzerinde tabanlı erişim sağlar. Kısıtlanmamış izinlerini herkes vermek yerine Azure aboneliği veya kaynakları, yalnızca belirli eylemleri izin verebilirsiniz.

Azure RBAC tüm kaynak türleri için geçerli üç temel rol vardır:

- **Sahibi** temsilci başkalarına erişimi hakkı dahil olmak üzere tüm kaynaklara tam erişimi vardır.

- **Katkıda bulunan** olabilir oluşturun ve tüm türlerini Azure kaynaklarını yönetmek ancak başkalarına erişim izni veremiyor.

- **Okuyucu** mevcut Azure kaynaklarını görüntüleyebilirsiniz.

![Azure Rol Tabanlı Erişim Denetimi](./media/azure-isolation/azure-isolation-fig3.png)

Azure RBAC rollerin geri kalanı belirli Azure kaynaklarının yönetimini sağlar. Örneğin, sanal makine katılımcı rolü oluşturmak ve sanal makineleri yönetmek kullanıcının sağlar. Bu onları erişimi Azure sanal ağı veya sanal makine bağlandığı alt sağlamaz.

[RBAC yerleşik rolleri](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) mevcut rolleri listeler. Her yerleşik rol kullanıcılara veren kapsam ve işlemleri belirtir. Daha fazla denetim için kendi rolleri tanımlamak için arıyorsanız, nasıl oluşturacağınızı öğrenin [Azure rbac'de özel roller](https://docs.microsoft.com/azure/role-based-access-control/custom-roles).

Azure Active Directory için başka bazı özellikleri şunlardır:
- Azure AD SaaS uygulamaları burada barındırılan SSO sağlar. Bazı uygulamalar Azure AD federasyonu kullanırken diğerleri parola SSO hizmetinden yararlanır. Federasyon uygulamalarına, kullanıcı sağlamayı da destekleyebilir ve [parola kasası oluşturma](https://www.techopedia.com/definition/31415/password-vault).

- [Azure Storage](https://azure.microsoft.com/services/storage/) verilerine erişim, kimlik doğrulaması ile denetlenir. Her Depolama hesabı bir birincil anahtara sahip ([depolama hesabı anahtarı](https://docs.microsoft.com/azure/storage/storage-create-storage-account), veya SAK) ve ikincil bir gizli anahtar (paylaşılan erişim imzası veya SAS).

- Azure AD kimlik Federasyon üzerinden bir hizmet olarak kullanarak sağlar [Active Directory Federasyon Hizmetleri](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-azure-adfs), eşitleme ve çoğaltma ile şirket içi dizinleri.

- [Azure çok faktörlü kimlik doğrulaması](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) oturum açma işlemleri bir mobil uygulama, telefon araması veya kısa mesaj kullanarak doğrulamasını gerektiren çok faktörlü kimlik doğrulama hizmetidir. Bu Azure AD ile Azure multi-Factor Authentication sunucusu ile ve özel uygulamalar ve dizinler SDK'sını kullanarak güvenli şirket içi kaynaklara yardımcı olmak için kullanılabilir.

- [Azure AD etki alanı Hizmetleri](https://azure.microsoft.com/services/active-directory-ds/) , etki alanı denetleyicileri dağıtmadan Azure sanal makineleri bir Active Directory etki alanına katılmasını sağlar. Bu sanal makinelere Kurumsal Active Directory kimlik bilgilerinizle oturum açın ve sanal makinelerin etki alanına katılmış tüm Azure sanal makinelerinde güvenlik temelleri zorlamak için Grup İlkesi kullanarak yönetebilirsiniz.

- [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) yüz milyonlarca kimlikleri için ölçeklenebilir bir yüksek oranda kullanılabilir kimlik genel yönetim hizmeti tüketiciye yönelik uygulamalar için sağlar. Bu hizmet mobil platformlar ve web platformlarıyla tümleştirilebilir. Tüketicileriniz özelleştirilebilir deneyimler aracılığıyla tüm uygulamalarınıza, var olan sosyal hesaplarını kullanarak veya kimlik bilgileri oluşturma oturum açabilir.

### <a name="isolation-from-microsoft-administrators--data-deletion"></a>Yalıtım Microsoft Yöneticiler & veri silme
Microsoft, verilerinizin uygunsuz erişimden korumak veya yetkisiz kişiler tarafından kullanmak için güçlü ölçüleri alır. Bu işlem süreçlerini ve denetimleri tarafından yedeklenen [çevrimiçi Hizmet Koşulları'nı](http://aka.ms/Online-Services-Terms), verilerinize erişimi yöneten sözleşme taahhüt sunar.

-   Microsoft mühendisleri varsayılan verilerinizin bulutta erişiminiz yok. Bunun yerine, bunlar yönetim gözetim, yalnızca gerekli olduğunda altında erişimi verilir. Bu erişim olduğunu dikkatle denetlenen ve oturum açmış ve artık gerekli olmadığında iptal.

-   Microsoft, diğer şirketlerle kendi adımıza sınırlı hizmetleri sağlaması için. Müşteri verileri yalnızca sunmak için Hizmetleri, biz bunları sağlamak için işe Yükleniciler erişebilir ve başka bir amaçla kullanmaları yasaktır. Ayrıca, bunlar kapsamındaki müşterilerimizin bilgi gizliliğini korumak için bağlıdır.

ISO/IEC 27001 gibi denetlenen sertifikalar ile iş Hizmetleri, Microsoft ve yasal İşletme amaçları için yalnızca bu erişim şifreli olarak örnek denetimleri gerçekleştirmek onaylanmış denetim firmalarından tarafından düzenli olarak doğrulanır. Müşteri verilerini her zaman, dilediğiniz zaman ve herhangi bir nedenle da erişebilirsiniz.

Herhangi bir veri silerseniz, Microsoft Azure önbelleğe alınmış veya yedek kopyaları dahil olmak üzere verileri siler. Kapsamı içindeki hizmetler için bu silme saklama süresi dolduktan sonra 90 gün içinde ortaya çıkar. (Kapsam services veri işleme koşulları bölümünde tanımlanmış bizim [çevrimiçi Hizmet Koşulları'nı](http://aka.ms/Online-Services-Terms).)

Depolama için kullanılan bir disk sürücüsü bir donanım hatası varsa, güvenli bir şekilde olan [silinmesi veya tahrip](https://www.microsoft.com/trustcenter/Privacy/You-own-your-data) değiştirme veya onarım için üreticinin döndürür Microsoft önce. Herhangi bir yolla veriler kurtarılamaz emin olmak için sürücüdeki verilerin üzerine yazılır.

## <a name="compute-isolation"></a>Yalıtım işlem
Microsoft Azure, geniş işlem örnekleri içeren çeşitli bulut tabanlı bilgi işlem hizmetler & Yukarı ve aşağı otomatik olarak uygulamanızı veya Kurumsal ihtiyaçlarını karşılamak üzere ölçeği hizmetleri sağlar. Bu işlem örneğinde ve hizmet yalıtım veri yapılandırma esneklik ödün vermeden, müşterilerinizin talep güvenliğini sağlamak için birden çok düzeyleri sunar.

### <a name="isolated-virtual-machine-sizes"></a>Yalıtılmış sanal makine boyutları
Azure işlem sunar sanal makine boyutları olan Isolated belirli donanım türlerine ve tek bir müşteriye ayrılmış.  Bu sanal makine boyutlarını uyumluluk ve Mevzuat gereklilikleri gibi öğeleri içeren iş yükleri için yüksek derecede diğer müşterilerden yalıtım gerektiren iş yükleri için uygundur.  Müşterileri de seçebilirsiniz daha fazla kullanarak bu yalıtılmış sanal makinelerin kaynakları ayırabilir [iç içe geçmiş sanal makineler için Azure desteği](https://azure.microsoft.com/en-us/blog/nested-virtualization-in-azure/).

Yalıtılmış bir boyut kullanılarak sanal makineniz bu belirli sunucu örneğinde yalnızca bir çalışan olacağını garanti eder.  Geçerli yalıtılmış sanal makine önerileri şunlardır:
* Standard_E64is_v3
* Standard_E64i_v3
* Standard_M128ms
* Standard_GS5
* Standard_G5
* Standard_DS15_v2
* İçin Standard_D15_v2

Kullanılabilir her yalıtılmış boyutunu hakkında daha fazla bilgiyi [burada](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes-memory).

### <a name="hyper-v--root-os-isolation-between-root-vm--guest-vms"></a>Kök VM & Konuk VM'ler arasında Hyper-V & kök işletim sistemi yalıtımı
Azure işlem platformu makine sanallaştırmayı temel — yani bir Hyper-V sanal makinesinde tüm müşteri kodu yürütür. Her Azure düğümü (veya ağ uç noktası), doğrudan donanımın üzerinde çalışır ve değişken sayıya Konuk sanal makineleri (VM'ler), bir düğüm böler hiper yönetici yoktur.


![Kök VM & Konuk VM'ler arasında Hyper-V & kök işletim sistemi yalıtımı](./media/azure-isolation/azure-isolation-fig4.jpg)


Her düğüm, bir özel kök konak işletim sistemi çalıştıran VM da sahiptir. VM Konuk sanal makineleri ve Konuk sanal makineleri birbirinden, hiper yönetici ve kök işletim sistemi tarafından yönetilir kök yalıtım bir kritik sınırıdır. Hiper yönetici/kök işletim sistemi eşleştirme Microsoft'un on yılları işletim sistemi güvenlik deneyimi ve Microsoft Hyper-Konuk VM'ler güçlü yalıtımı sağlamak için V'ye, daha yeni öğrenme yararlanır.

Azure platformu, sanallaştırılmış bir ortam kullanır. Kullanıcı örnekleri bir fiziksel ana bilgisayar sunucusu erişimi olmayan bir tek başına sanal makineler çalışacağı ve bu yalıtım fiziksel işlemci (halka-0/halkası-3) ayrıcalık düzeylerini kullanarak zorlanır.

Halka 0 en yüksek, halka 3 ise en düşük ayrıcalıklara sahiptir. Daha düşük ayrıcalıklı halkası 1'de konuk işletim sistemini çalıştıran ve en az ayrıcalıklı Halka 3'te uygulamaları çalıştırın. Bu fiziksel kaynak sanallaştırma yöntemi sayesinde konuk işletim sistemi ve hiper yönetici arasında net bir ayrım sağlanarak bu iki bileşen arasında ek bir güvenlik ayrımı yapılmış olur.

Azure hiper yönetici mikro çekirdek gibi davranır ve tüm donanım erişim istekleri Konuk sanal makineleri ana bilgisayar işleme için VMBus adında bir paylaşılan bellek arabirimi kullanarak geçirir. Bu, kullanıcıların sistemde ham okuma/yazma/yürütme erişimine sahip olmasını önler ve sistem kaynaklarının paylaşımıyla ilgili riskleri azaltır.

### <a name="advanced-vm-placement-algorithm--protection-from-side-channel-attacks"></a>Gelişmiş VM yerleştirme algoritması & yan kanal saldırılarına karşı koruma
Çapraz VM saldırı iki adımdan oluşur: VM'ler kurban biri olarak aynı ana bilgisayardaki bir saldırganın denetimli VM yerleştirme ve yalıtım sınırı kurban hassas bilgilerinizi çalan ya da greed veya vandalism için kendi performansını etkileyen ihlal. Microsoft Azure, Gelişmiş bir VM yerleştirme algoritması ve gürültülü komşu Vm'leri de dahil olmak üzere tüm bilinen yan kanal saldırılarına karşı koruma kullanarak her iki adımın korumasının sağlar.

### <a name="the-azure-fabric-controller"></a>Azure yapı denetleyicisi
Kiracı İş yükleri için altyapı kaynaklarını ayırma için Azure yapı denetleyicisi sorumludur ve sanal makineler ana bilgisayardan tek yönlü iletişim yönetir. Azure yapı denetleyicisi VM yerleştirme algoritmasının oldukça karmaşık ve neredeyse imkansız fiziksel ana bilgisayar düzeyinde tahmin etmek ' dir.

![Azure yapı denetleyicisi](./media/azure-isolation/azure-isolation-fig5.png)

Azure hiper yönetici sanal makineler arasındaki bellek ve işlem ayrımı zorunlu kılan ve güvenli bir şekilde konuk işletim sistemi kiracılar için ağ trafiğini yönlendirir. Bu olasılığını ve yan kanal saldırı VM düzeyinde ortadan kaldırır.

Azure'da VM kök özeldir: bir sıkı işletim kök işletim sistemi olarak adlandırılan bir doku Aracısı (FA) barındıran sistem çalıştırır. SK'lar sırayla Konuk aracıları (GA) yönetmek için müşteri sanal makineleri konuk işletim sistemleri içinde kullanılır. SK'lar depolama düğümleri da yönetebilirsiniz.

Azure hiper yönetici topluluğu kök işletim sistemi/FA ve müşteri sanal makineleri/gaz bir işlem düğümünde oluşur. SK'lar (bilgi işlem ve depolama kümeleri ayrı FCs tarafından yönetilen) işlem ve depolama düğümleri dışında var olan bir yapı denetleyicisi (FC) tarafından yönetilir. Çalışırken bir müşteri, uygulamanın yapılandırma dosyasına güncelleştirmeleri olursa FC hangi yapılandırma değişikliği uygulama bildirim sonra gaz kişiler, FA ile iletişim kurar. Donanım arızası olması durumunda FC otomatik olarak kullanılabilir donanım bulmak ve var olan VM yeniden başlatın.

![Azure yapı denetleyicisi](./media/azure-isolation/azure-isolation-fig6.jpg)

Bir aracı bir yapı denetleyicisi arasında iletişimi tek yönlüdür. Aracı yalnızca denetleyicisinden isteklerine yanıt SSL korumalı bir hizmet uygular. Denetleyici veya diğer ayrıcalıklı iç düğümleri bağlantılar başlatamaz. Güvenilmeyen sanki FC tüm yanıtları değerlendirir.


![Yapı denetleyicisi](./media/azure-isolation/azure-isolation-fig7.png)

Konuk sanal makineleri ve Konuk sanal makineleri birbirinden kök VM'den yalıtım genişletir. İşlem düğümleri artan koruma için depolama düğümlerinden de yalıtılır.


Hiper Yönetici ve ağ paketi - güvenilmeyen sanal makineleri olamaz sahte trafik oluşturabilir veya bunları için değil trafiği alabilmesine olmalarını sağlamak için filtreleri OS sağlamak ana bilgisayar korumalı altyapı uç noktaları için trafiği doğrudan veya gönderme ve alma uygunsuz yayın trafiği.


### <a name="additional-rules-configured-by-fabric-controller-agent-to-isolate-vm"></a>VM yalıtmak için yapı denetleyicisi aracısı tarafından yapılandırılan ek kurallar
Varsayılan olarak, tüm trafik, bir sanal makine oluşturulduğunda ve ardından doku Denetleyicisi aracı kurallar ve yetkili trafiğine izin veren özel durumlar eklemek için paket filtresi yapılandırır engellenir.

Programlanmış kuralları iki kategorisi vardır:

-   **Makine yapılandırma veya altyapı kuralları:** varsayılan olarak, tüm iletişimin engellenir. Bir sanal makine, DHCP ve DNS trafiği gönderip almasına izin veren özel durumlar vardır. Sanal makineler ayrıca "Genel" internet'e trafiği göndermek ve diğer sanal makinelerin aynı Azure sanal ağ ve işletim sistemi Etkinleştirme sunucusu içinde trafiği göndermek. İzin verilen giden hedefler, sanal makineler listesini Azure yönlendirici alt ağlar, Azure yönetim ve diğer Microsoft özellikleri içermez.

-   **Rol yapılandırma dosyası:** bu gelen erişim denetim kiracının hizmet modelini temel alan listeleri (ACL'ler) tanımlar.

### <a name="vlan-isolation"></a>VLAN yalıtımı
Her kümedeki üç VLAN'ları vardır:

![VLAN yalıtımı](./media/azure-isolation/azure-isolation-fig8.jpg)


-   Ana VLAN – bağlantılar güvenilmeyen müşteri düğümler

-   FC VLAN – güvenilen FCs ve sistemlerini destekleyen içerir

-   VLAN – cihaz güvenilir ağ ve diğer altyapı aygıtları içerir

İletişim FC VLAN'ı ana VLAN izin verilir, ancak ana VLAN'ı FC VLAN başlatılamaz. İletişim, VLAN cihaza ana VLAN'de engellenir. Bu, bir düğümde müşteri kodu çalışan tehlikede olsa bile, onu FC ya da cihaz VLAN'lar düğümlerinde saldırı olamaz emin olmasını sağlar.

## <a name="storage-isolation"></a>Depolama ayırma
### <a name="logical-isolation-between-compute-and-storage"></a>İşlem ve depolama arasında mantıksal ayırma
Temel tasarımını bir parçası olarak, Microsoft Azure VM tabanlı hesaplama depolama biriminden ayırır. Bu ayrım, çoklu kiracı ve yalıtımı sağlamak daha kolay hesaplama ve depolama bağımsız olarak, ölçeklendirme sağlar.

Bu nedenle, Azure Storage ayrı donanımda ile ağ bağlantısı için Azure işlem dışında mantıksal olarak çalışır. [Bu](https://msenterprise.global.ssl.fastly.net/vnext/PDFs/A01_AzureSecurityWhitepaper20160415c.pdf) bir sanal disk oluşturulduğunda, disk alanı için tüm kapasitesi atanmadı anlamına gelir. Bunun yerine, bir tablo alanlara fiziksel disk üzerinde sanal diskin adreslerini eşleştiren oluşturulur ve bu başlangıçta boş bir tablodur. **Bir müşteri sanal disk üzerinde veri Yazar ilk kez fiziksel disk alanı ayrılır ve onu bir işaretçi tabloda yerleştirilir.**
### <a name="isolation-using-storage-access-control"></a>Yalıtım kullanılarak depolama erişim denetimi
**Erişim denetimi Azure storage'da** basit erişim denetimi modeli vardır. Her Azure aboneliği bir veya daha fazla depolama hesabı oluşturabilirsiniz. Her Depolama hesabının o depolama hesabındaki tüm verilere erişimi denetlemek için kullanılan tek bir gizli anahtar vardır.

![Yalıtım kullanılarak depolama erişim denetimi](./media/azure-isolation/azure-isolation-fig9.png)

**(Tablolar dahil) Azure Storage veri erişimi** aracılığıyla denetlenen bir [SAS (paylaşılan erişim imzası)](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) hangi verir kapsamlı erişim belirteci. SAS ile imzalanmış bir sorgu şablonu (URL) aracılığıyla oluşturulan [SAK (depolama hesabı anahtarı)](https://msdn.microsoft.com/library/azure/ee460785.aspx). [URL imzalı](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) sonra sorgunun ayrıntıları doldurun ve depolama hizmeti isteği yapabilirsiniz (yani temsilci) başka bir işlem için verilebilir. Bir SAS depolama hesabının gizli anahtarı göstermeden istemcilere zamana dayalı erişim vermenize olanak sağlar.

SAS anlamına gelir ki istemci sınırlı süre ve belirtilen bir izin kümesi ile belirli bir süre için bizim depolama hesabındaki nesnelere izinleri verebilirsiniz. Hesap erişim tuşlarınızı paylaşmak zorunda kalmadan Biz bu sınırlı izinleri verebilirsiniz.

### <a name="ip-level-storage-isolation"></a>IP düzey depolama yalıtımı
Güvenlik duvarları kurmak ve güvenilir istemcileriniz için bir IP adresi aralığı tanımlayabilirsiniz. Bir IP adresi tanımlı aralığın içinde olan istemcilerin bağlanabileceği bir IP adresi aralığıyla [Azure Storage](https://docs.microsoft.com/azure/storage/storage-security-guide).

IP depolama veri trafiği IP depolama için ayrılmış ya da ayrılmış bir tünel ayırmak için kullanılan bir ağ mekanizması aracılığıyla yetkisiz kullanıcılara karşı korunur.

### <a name="encryption"></a>Şifreleme
Azure verileri korumak için şifreleme aşağıdaki türlerini sağlar:
-   Aktarımdaki şifreleme

-   Bekleme sırasında şifreleme

#### <a name="encryption-in-transit"></a>Aktarımdaki şifreleme
Şifreleme Aktarımdaki ağlar üzerinden iletilirken veri koruma, bir mekanizmadır. Azure Storage ile verileri kullanarak güvenliğini sağlayabilirsiniz:

-   [Aktarım düzeyinde şifreleme](https://docs.microsoft.com/azure/storage/storage-security-guide#encryption-in-transit), içine veya dışına Azure Storage veri aktarırken HTTPS gibi.

-   [Şifreleme wire](../storage/common/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), Azure dosya paylaşımları için SMB 3.0 şifreleme gibi.

-   [İstemci tarafı şifreleme](https://docs.microsoft.com/azure/storage/storage-security-guide#using-client-side-encryption-to-secure-data-that-you-send-to-storage), depolama alanına transfer önce verileri şifrelemek için ve depolama alanı biterse aktarıldıktan sonra verilerin şifresini çözmek için.

#### <a name="encryption-at-rest"></a>Bekleyen şifreleme
Çoğu kuruluş için [bekleyen verileri şifreleme](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) veri gizliliği, uyumluluk ve veri egemenliği doğru zorunlu bir adımdır. "Bekleyen" olan verilerin şifrelenmesini sağlayan üç Azure özellikleri şunlardır:

-   [Depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-security-guide#encryption-at-rest) depolama birimi hizmeti otomatik olarak verileri Azure depolama birimine yazılırken şifrelemeniz istemenizi sağlar.

-   [İstemci tarafı şifreleme](https://docs.microsoft.com/azure/storage/storage-security-guide#client-side-encryption) bekleyen şifreleme özelliği de sağlar.

-   [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) işletim sistemi ve bir Iaas sanal makine tarafından kullanılan veri disklerle şifrelemenizi sağlar.

#### <a name="azure-disk-encryption"></a>Azure Disk Şifrelemesi
[Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) sanal makineleri (VM'ler) adresi kuruluş güvenlik ve uyumluluk gereksinimleri (önyükleme ve veri diskleri dahil), VM diskleri şifreleyerek anahtarları ve, denetim ilkeleri ile yardımcı olan [Azure anahtarı Kasa](https://azure.microsoft.com/services/key-vault/).

Windows için Disk şifrelemesi çözüm dayanır [Microsoft BitLocker Sürücü Şifrelemesi](https://technet.microsoft.com/library/cc732774.aspx), ve Linux çözüm dayanır [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt).

Microsoft Azure'da etkinleştirildiğinde çözümü Iaas VM'ler için aşağıdaki senaryoları destekler:
-   Azure anahtar kasası ile tümleştirme

-   Standart katmanı VMs: A, D, DS, G, GS ve benzeri serisi Iaas VM'ler

-   Windows ve Linux Iaas VM'ler üzerinde şifrelemeyi etkinleştirme

-   Windows Iaas VM'ler için sürücüler işletim sistemi ve veri şifreleme devre dışı bırakma

-   Veri sürücülerini şifreleme Linux Iaas VM'ler için devre dışı bırakma

-   Windows istemci işletim sistemi çalıştıran bir Iaas Vm'leri üzerinde şifrelemeyi etkinleştirme

-   Bağlama yolları birimlerle şifrelemeyi etkinleştirme

-   Kullanarak Linux (RAID) bölümlemesine diskle yapılandırılmış sanal makineleri üzerinde şifrelemeyi etkinleştirme [mdadm](https://en.wikipedia.org/wiki/Mdadm)

-   Kullanarak Linux VM'ler üzerinde şifrelemeyi etkinleştirme [LVM (mantıksal birim Yöneticisi)](https://msdn.microsoft.com/library/windows/desktop/bb540532) , veri diskleri

-   Depolama alanları kullanılarak yapılandırılan Windows VM'ler üzerinde şifrelemeyi etkinleştirme

-   Tüm Azure genel bölgeleri desteklenir

Çözüm aşağıdaki senaryolar, özellikler ve teknoloji sürümde desteklemez:

-   Temel katman Iaas VM'ler

-   Bir işletim sistemi sürücüsünde Linux Iaas VM'ler için şifrelemeyi devre dışı bırakma

-   Klasik VM oluşturma yöntemini kullanarak oluşturduğunuz Iaas VM'ler

-   Şirket içi anahtar yönetimi hizmeti ile tümleştirme

-   Azure dosyaları (paylaşılan dosya sistemi), ağ dosya sistemi (NFS), dinamik birimler ve yazılım tabanlı RAID sistemler ile yapılandırılmış Windows VM'ler

## <a name="sql-azure-database-isolation"></a>SQL Azure veritabanı yalıtımı
SQL Veritabanı, piyasa lideri Microsoft SQL Server altyapısını temel alan ve görev açısından kritik iş yüklerini üstlenebilen, Microsoft bulutu tabanlı bir ilişkisel veritabanı hizmetidir. SQL veritabanı sunar tahmin edilebilir veri yalıtımı hesap düzeyinde Coğrafya / bölge ve ağ üzerinde dayalı — yönetime neredeyse tüm ile.

### <a name="sql-azure-application-model"></a>SQL Azure uygulama modeli

[Microsoft SQL Azure](https://docs.microsoft.com/azure/sql-database/sql-database-get-started) veritabanı olan SQL Server teknolojilerinde kurulu bir bulut tabanlı ilişkisel veritabanı hizmeti. Bulutta Microsoft tarafından barındırılan bir yüksek oranda kullanılabilir, ölçeklenebilir, çok Kiracı veritabanı hizmeti sağlar.

Bir uygulama açısından SQL Azure aşağıdaki hiyerarşi sağlar: bir çok kapsama aşağıdaki düzeylerinin her düzeyine sahip.

![SQL Azure uygulama modeli](./media/azure-isolation/azure-isolation-fig10.png)

Hesap ve abonelik yönetimi ilişkilendirmek için Microsoft Azure platform kavram olmasıdır.

Mantıksal sunucular ve veritabanları SQL Azure özgü kavramlar ve OData ve TSQL arabirimleri sağlanan SQL Azure kullanarak veya Azure portalına tümleşik SQL Azure portalı üzerinden yönetilir.

SQL Azure sunucusu fiziksel veya VM örnekleri olmayan, bunun yerine, veritabanları, bu nedenle çağrılan "mantıksal yöneticisinde" depolanır, yönetim ve güvenlik ilkeleriyle paylaşımı koleksiyonlarıdır veritabanı.

![SQL Azure](./media/azure-isolation/azure-isolation-fig11.png)

Mantıksal asıl veritabanını içerir:

-   Sunucuya bağlanmak için kullanılan SQL oturum açma

-   Güvenlik duvarı kuralları

Bağlanırken aynı mantıksal sunucu veritabanlarından SQL Azure kümedeki aynı fiziksel örneğinde bunun yerine uygulamaların olması garanti edilmez fatura ve kullanımı ile ilgili bilgi için SQL Azure hedef veritabanı adı sağlamanız gerekir.

Sunucunun gerçek oluşturulmasını bir bölgede kümelerin gerçekleşirken Müşteri açısından bakıldığında, bir mantıksal sunucu bir grafik coğrafi bölgede oluşturulur.

### <a name="isolation-through-network-topology"></a>Ağ topolojisi yalıtımı

Bir mantıksal sunucu oluşturduğunuzda ve DNS adı kayıt noktaları sunucunun nerede yerleştirilen belirli veri merkezinde böylece çağrılan "Ağ geçidi VIP" adresine DNS adı.

(Sanal IP adresi) VIP durum bilgisiz ağ geçidi hizmetler koleksiyonu vardır. Genel olarak, birden çok veri kaynağı arasında (ana veritabanı, kullanıcı veritabanı, vb.) gerekli koordinasyon olduğunda ağ geçitleri dahil. Ağ Geçidi Hizmetleri aşağıdakileri uygulayın:
-   **TDS bağlantı proxy.** Bu, kullanıcı veritabanı arka uç kümesinde bulma, oturum açma sırası uygulama ve arka uç ve arka TDS paketlere iletme içerir.

-   **Veritabanı yönetimi.** Bu CREATE/ALTER/DROP veritabanı işlemleri yapmak için iş akışları koleksiyonu uygulama içerir. Veritabanı işlemleri algılaması TDS paketleri veya açık OData API tarafından çağrılabilir.

-   CREATE/ALTER/DROP oturum açma/kullanıcı işlemleri

-   OData API aracılığıyla mantıksal sunucu yönetimi işlemleri

![Ağ topolojisi yalıtımı](./media/azure-isolation/azure-isolation-fig12.png)

Ağ geçitleri arkasında katmanı "arka uç" adı verilir. Yüksek oranda kullanılabilir bir şekilde tüm verilerin depolandığı budur. Her veri parçası bir "Bölüm" ait söylenir ya da "Yük devretme birimi", her birinin en az üç çoğaltmaları sahip. Çoğaltmaları depolanan ve SQL Server altyapısı tarafından çoğaltılır ve genellikle "doku" adlandırılan bir yük devretme sistemi tarafından yönetilir.

Genellikle, arka uç sistem diğer sistemlere giden bir güvenlik önlemi olarak iletişim kurmaz. Bu, ön uç (ağ geçidi) katmanındaki sistemleri için ayrılmıştır. Ağ geçidi katman makinelerde sınırlı bir savunma mekanizması olarak saldırı yüzeyini en aza indirmek için arka uç makinelerde ayrıcalıklarına sahip.

### <a name="isolation-by-machine-function-and-access"></a>Yalıtım makine işlevi ve erişim
SQL Azure (başka bir makine işlevler üzerinde çalışan hizmetleri oluşur. SQL Azure trafiği yalnızca giderek arka uç giriş ve çıkışı değil genel ilkesini olduğu ortamlarda "arka uç" bulut veritabanı ve "ön uç" (ağ geçidi/Yönetimi) bölünür. Ön uç ortam diğer hizmetlerin dış dünya iletişim kurabilir ve genel olarak, yalnızca arka uç (yeterli çağırmak için gereken giriş noktaları çağırmak için), sınırlı izinleri.

## <a name="networking-isolation"></a>Ağ yalıtımı
Azure dağıtımı, ağ yalıtımı çok katmanlı sahiptir. Aşağıdaki diyagram, ağ yalıtımı müşteriler için Azure sağlar çeşitli katmanları gösterilmektedir. Bu, hem Azure platformu yerel hem de müşteri tarafından tanımlanan özellikler katmanlardır. Gelen Internet'ten Azure DDoS Azure karşı büyük ölçekli saldırılarına karşı yalıtım sağlar. Sonraki yalıtım, trafiği sanal ağa bulut hizmeti aracılığıyla geçirebilirsiniz belirlemek için kullanılan müşteri tanımlı genel IP adresleri (Bitiş) katmanıdır. Yerel Azure sanal ağ yalıtımı diğer ağlarla tam yalıtımı sağlar ve trafik yalnızca yapılandırılan kullanıcı yolları ve yöntemleri akar. Bu yolları ve yöntemleri sonraki katmanı nerede Nsg'ler, UDR ve ağ sanal Gereçleri korumalı ağ uygulama dağıtımları korumak için yalıtım sınırlarını oluşturmak için kullanılabilir durumdadır.

![Ağ yalıtımı](./media/azure-isolation/azure-isolation-fig13.png)

**Trafik yalıtımı:** A [sanal ağ](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) Azure platformunda trafik yalıtımı sınırıdır. Her iki sanal ağlar aynı müşteri tarafından oluşturulmamış olsa bile bir sanal ağdaki sanal makinelerden (VM'ler) farklı bir sanal ağ içindeki VM'ler için doğrudan iletişim kuramıyor. Yalıtım müşteri sanal makineleri sağlar önemli bir özelliktir ve iletişim bir sanal ağ içindeki özel kalır.

[Alt ağ](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview#subnets) IP aralığı tabanlı sanal ağ yalıtımı ile bir ek katmanı sunar. Sanal ağ IP adresleri, birden çok alt ağı organizasyon ve güvenlik için sanal bir ağa bölebilirsiniz. Bir sanal ağ içindeki alt ağlara (aynı veya farklı) dağıtılan VM'ler ve PaaS rolü örnekleri, ek bir yapılandırma gerektirmeden birbirleriyle iletişim kurabilir. Ayrıca yapılandırabilirsiniz [ağ güvenlik grubu (Nsg'ler)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview#network-security-groups-nsg) izin ver veya erişim denetim listesindeki NSG (ACL) yapılandırılmış kurallar temel alınarak bir VM örneğine ağ trafiği reddetmek için. NSG'ler alt ağlarla veya bu alt ağların içindeki tekil VM örnekleriyle ilişkili olabilir. NSG bir alt ağ ile ilişkili olduğunda ACL kuralları bu alt ağdaki tüm VM örnekleri için geçerli olur.

## <a name="next-steps"></a>Sonraki Adımlar

- [Ağ yalıtımı seçenekleri makineler Windows için Azure sanal ağlar](https://azure.microsoft.com/blog/network-isolation-options-for-machines-in-windows-azure-virtual-networks/)

Burada makineler belirli arka uç ağ veya alt ağ yalnızca belirli istemciler veya diğer bilgisayarların IP adreslerini beyaz üzerinde göre belirli bir uç nokta bağlanmak izin verebilir Klasik ön uç ve arka uç senaryo içerir.

- [Yalıtım işlem](https://msenterprise.global.ssl.fastly.net/vnext/PDFs/A01_AzureSecurityWhitepaper20160415c.pdf)

Microsoft Azure, geniş işlem örnekleri içeren bir çeşitli bulut tabanlı bilgi işlem hizmetler & Yukarı ve aşağı otomatik olarak uygulamanızı veya Kurumsal ihtiyaçlarını karşılamak üzere ölçeği hizmetleri sağlar.

- [Depolama ayırma](https://msenterprise.global.ssl.fastly.net/vnext/PDFs/A01_AzureSecurityWhitepaper20160415c.pdf)

Microsoft Azure müşteri hesaplama VM tabanlı depolama biriminden ayırır. Bu ayrım, çoklu kiracı ve yalıtımı sağlamak daha kolay hesaplama ve depolama bağımsız olarak, ölçeklendirme sağlar. Bu nedenle, Azure Storage ayrı donanımda ile ağ bağlantısı için Azure işlem dışında mantıksal olarak çalışır. HTTP ya da Müşteri'nin tercihine bağlı HTTPS üzerinden tüm istekleri çalıştırın.

