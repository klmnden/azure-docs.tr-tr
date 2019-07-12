---
title: Microsoft Azure üzerinde güvenli uygulamalar tasarlama
description: Bu makalede, web uygulaması projenize gereksinimi ve tasarım aşamalarını sırasında dikkate alınması gereken en iyi uygulamalar açıklanmaktadır.
author: TerryLanfear
manager: barbkess
ms.author: terrylan
ms.date: 06/11/2019
ms.topic: article
ms.service: security
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: 12b9793cabb261368c437bd2ae2dbb39cf078bef
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67653291"
---
# <a name="design-secure-applications-on-azure"></a>Azure'da güvenli uygulamalar tasarlama
Bu makalede güvenlik etkinliklerini ve bulut için uygulamalar tasarlarken dikkate alınması gereken denetimler sunar. Kaynakların yanı sıra güvenlik sorularını ve sırasında gereksinimlerini dikkate alın ve Microsoft aşamaları tasarım kavramları eğitim [Security Development Lifecycle (SDL)](https://msdn.microsoft.com/library/windows/desktop/84aed186-1d75-4366-8e61-8d258746bopq.aspx) ele alınmaktadır. Etkinlikleri ve daha güvenli bir uygulama tasarlamak için kullanabileceğiniz Azure Hizmetleri tanımlamanıza yardımcı olmaktır.

Aşağıdaki SDL aşamaları, bu makalede ele alınmaktadır:

- Eğitim
- Gereksinimler
- Tasarım

## <a name="training"></a>Eğitim
Bulut uygulamanızı geliştirmeye başlamadan önce Azure üzerinde güvenlik ve gizlilik anlamak için zaman ayırın. Bu adımı gerçekleştirerek, uygulamanızda açıklardan güvenlik açıklarının sayısını ve azaltabilir. Sizin için durmaksızın değişen tehdit ortamını uygun şekilde tepki vermek daha hazırlıklı olacaktır.

Azure ile ilgili en iyi güvenlik uygulamaları ve geliştiricilere sunulan Azure Hizmetleri ile tanımak için eğitim aşamasında aşağıdaki kaynakları kullanın:

  - [Azure Geliştirici Kılavuzu](https://azure.microsoft.com/campaigns/developer-guide/) Azure ile çalışmaya başlama işlemini gösterir. Kılavuz, uygulamalarınızı çalıştırmak, verilerinizi depolamak, zeka, IOT uygulamaları oluşturun ve çözümlerinizi daha verimli ve güvenli bir şekilde dağıtmak için kullanabileceğiniz hangi hizmetlerin gösterir.

  - [Azure geliştiricileri için Başlarken Kılavuzu](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide) kendi geliştirme gereksinimleri için Azure platformunu kullanan kullanmaya başlamak isteyen geliştiriciler için gerekli bilgileri sağlar.

  - [SDK'lar ve Araçlar](https://docs.microsoft.com/azure/index#pivot=sdkstools) Azure'da kullanılabilen araçları açıklar.

  - [Azure DevOps hizmetleriyle](https://docs.microsoft.com/azure/devops/) geliştirme işbirliği araçları sağlar. Yüksek performanslı işlem hatları, ücretsiz Git depoları, sağlanan yapılandırılabilir Kanban panoları ve kapsamlı otomatik ve bulut tabanlı yük testi araçları içerir.
    [DevOps Kaynak Merkezi](https://docs.microsoft.com/azure/devops/learn/) için DevOps uygulamaları, Git sürüm denetimi, çevik yöntemler, nasıl Microsoft'ta DevOps ile çalışıyoruz ve nasıl kendi DevOps ilerleme değerlendirebilirsiniz öğrenme kaynaklarımızın birleştirir.

  - [İlk 5 güvenlik öğe üretime göndermeden önce dikkate alınması gereken](https://docs.microsoft.com/learn/modules/top-5-security-items-to-consider/index?WT.mc_id=Learn-Blog-tajanca) azure'da web uygulamalarınızı güvenli ve uygulamalarınızı en yaygın ve tehlikeli web uygulama saldırılarına karşı korumaya yardımcı olmak nasıl gösterir.

  - [Azure DevOps Seti'ni güvenli](https://azsk.azurewebsites.net/index.html) , kapsamlı Azure abonelik ve kaynak güvenlik ihtiyaçlarını kapsamlı otomasyon kullanan DevOps ekipleri için oluşturabilmesine olanak sağlar, betikleri, Araçlar, uzantılar ve otomasyonları koleksiyonudur. Azure için güvenli DevOps Seti'ni güvenlik içinde yerel bir DevOps iş akışlarınızla sorunsuzca tümleştirin nasıl gösterebilirsiniz. Geliştiriciler yardımcı olabilecek güvenlik doğrulama testleri (SVTs), güvenli kod yazma ve bulut uygulamalarına güvenli yapılandırmasını kodlama ve erken geliştirme aşamasında test gibi Seti Araçları yöneliktir.

  - [Azure çözümleri için önerilen güvenlik uygulamaları](https://azure.microsoft.com/resources/security-best-practices-for-azure-solutions) tasarlama, dağıtma ve Azure'ı kullanarak bulut çözümlerinizi yönetmek olarak kullanmak için en iyi güvenlik sağlar.

## <a name="requirements"></a>Gereksinimler
Gereksinimleri tanımı aşaması, uygulamanızın nedir ve yayınlanmasının bunu ne yapacağını tanımlama çok önemli bir adımdır. Gereksinimleri aşaması, ayrıca uygulamanıza oluşturacak olan güvenlik denetimleri hakkında düşünmek için bir zamandır. Bu aşamada, sürüm ve güvenli bir uygulama dağıtma emin olmak için SDL sürer adımları da başlar.

### <a name="consider-security-and-privacy-issues"></a>Güvenlik ve gizlilik konuları göz önünde bulundurun
Bu aşama, temel güvenlik ve gizlilik konuları dikkate en iyi bir zamandır. Kabul edilebilir düzeyde güvenlik ve gizlilik projenin başlangıcında tanımlayan bir takıma yardımcı olur:

- İlişkili güvenlik sorunlarıyla riskleri anlayın.
- Belirleyin ve geliştirme sırasında güvenlik hataları düzeltin.
- Kurulan düzeyde güvenlik ve gizlilik tüm proje boyunca geçerlidir.

Uygulamanız için gereksinimleri yazdığınızda, uygulamanızı ve verilerinizi güvenli kalmasına yardımcı olmak güvenlik denetimleri düşünün emin olun.

### <a name="ask-security-questions"></a>Güvenlik sorularınızı
Güvenlik gibi sorular:

  - Uygulamamın hassas verileri içeriyor mu?

  - Uygulamam toplamak veya bana endüstri standartları ve uyumluluk programları gibi izliyor gerektiren veri deposu [Federal Finans kurumu İnceleme Council (FFIEC)](https://docs.microsoft.com/azure/security/blueprints/ffiec-analytics-overview) veya [ödeme kartı Sektörüyle Veri güvenliği standartları (PCI DSS)](https://docs.microsoft.com/azure/security/blueprints/pcidss-analytics-overview)?

  - Uygulamamın toplamak veya kullanılabilir hassas kişisel veya müşteri verileri içeren, kendi başına veya diğer tanımlamak amacıyla, bilgi, iletişim kurun veya tek bir kişi bulun?

  - Uygulamamın toplamak veya bir kişinin TIP, eğitim, Finans erişmek için kullanılan veri veya çalışma bilgilerini içerir? Verilerinizin duyarlılığına gereksinimleri aşamasında tanımlayan verilerinizi sınıflandırmak ve uygulamanız için kullanacağınız veri koruma yöntemini belirlemenize yardımcı olur.

  - Verilerim nerede ve nasıl depolanır? Beklenmeyen değişiklikleri (örneğin, daha yavaş yanıt süresi), uygulamanızın kullandığı depolama hizmetleri nasıl izleyeceğiniz göz önünde bulundurun. Ayrıntılı verileri toplamak ve derinlik olası bir sorunu çözümlemek için günlüğe kaydetmeyi etkilemek olacak mı?

  - Uygulamamın genel (internet üzerinde) kullanılabilir veya dahili olarak yalnızca olacak? Ortak uygulamanız varsa, yanlış şekilde kullanılan toplanabilir verilerinizi nasıl koruyabilirsiniz? Uygulamanız dahili olarak yalnızca kullanılabilir haldeyse, kuruluşunuzda kimlerin uygulama erişimi olması gereken ve ne kadar süreyle bunlara erişmesi gereken göz önünde bulundurun.

  - Uygulamanızı tasarlama başlamadan önce kimlik modelini anlama? Nasıl, belirler kullanıcılar söyledikleri kim olduğunu ve hangi kullanıcı yapmak için yetkili?

  - Uygulamamın (para aktarma, kapılar kilidini açma veya ilaç sunma gibi) hassas veya önemli görevleri gerçekleştirir?
    Hassas bir görevi gerçekleştiren kullanıcı görevi gerçekleştirmek için yetkili nasıl doğrulanır ve kişiye kimin söyledikleri olduğunu kimliğini nasıl doğrulayacağınızı göz önünde bulundurun. Yetkilendirme (AuthZ) bir şey yapmak için bir kimliği doğrulanmış güvenlik sorumlusu izni verme işlemidir. Kimlik doğrulaması (kimlik doğrulama) bir taraf meşru kimlik bilgileri için zor işlemidir.

  - Uygulamamın karşıya yükleme veya dosyaları veya diğer verileri indirme izin vererek gibi tüm riskli yazılım etkinlikler gerçekleştirir? Uygulamanızı riskli etkinlikler gerçekleştirirseniz, bu kötü amaçlı dosya veya veri işleme gelen uygulamanızın kullanıcıları nasıl koruyacak göz önünde bulundurun.

### <a name="review-owasp-top-10"></a>Gözden geçirme OWASP ilk 10
Gözden geçirme göz önünde bulundurun [ <span class="underline">OWASP Top 10 uygulama güvenlik risklerini</span>](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project).
OWASP Top 10 web uygulamalarına kritik güvenlik risklerini ele alır.
Bu güvenlik riskleri bilincini uygulamanızda bu riskleri en aza gereksinimi ve tasarım kararları vermenize yardımcı olabilir.

İhlalleri önlemek için güvenlik denetimleri hakkında düşünmeye büyük/küçük harf önemlidir.
Ancak, aynı zamanda istediğiniz [bir ihlal varsayımı](https://docs.microsoft.com/azure/devops/learn/devops-at-microsoft/security-in-devops) meydana gelir. Acil durumda yanıtlanması gereken zorunluluğunu ihlal varsayılarak bazı önemli güvenlik hakkında önceden sorularını yardımcı olur:

  - Saldırı algılama nasıl?

  - Bir saldırı veya bir ihlal varsa ne yapmam?

  - Veri sızıntısı veya üzerinde oynanmaya gibi saldırı kurtarılır nasıl Visual Studio'da bir?

## <a name="design"></a>Tasarım

Tasarım aşamasında, tasarım ve işlevsel belirtimleri için en iyi uygulamalar oluşturmak için önemlidir. Bir proje boyunca güvenlik ve gizlilik konuları azaltmaktadır risk analizi gerçekleştirmek için önemlidir.

Güvenlik gereksinimleri sağladığınızdan ve güvenli bir tasarım kavramları kaçının veya bir güvenlik açığı için fırsatlar en aza indirmek. Bir gözetim tasarımında uygulamanızı kullanıma sunulduktan sonra kötü amaçlı ya da beklenmeyen eylemleri gerçekleştirmek bir kullanıcı izin verebilir uygulamanın bir güvenlik kusurdur.

Tasarım aşamasında, ayrıca güvenlik katmanları nasıl uygulayabileceğiniz hakkında düşünün; bir düzey savunma hattı mutlaka yeterli değildir. Bir saldırgan, web uygulaması güvenlik duvarınız (WAF) alırsa ne olur? İstediğiniz yerde, saldırı karşı korumaya için başka bir güvenlik denetimi.

Bunu aklınızda aşağıdaki güvenli tasarım kavramları ve güvenli uygulamalar tasarlarken ilgilenmeniz gereken güvenlik denetimleri ele alır:

- Güvenli kodlama kitaplık ve bir yazılım çerçevesi kullanın.
- Güvenlik açığı bulunan bileşenler için tarayın.
- Uygulama tasarımı sırasında modelleme tehdit kullanın.
- Saldırı yüzeyinizi azaltmak.
- Bir ilke kimliğinin birincil güvenlik çevresi olarak benimseyin.
- Önemli işlemler için yeniden kimlik doğrulaması gerektirir.
- Anahtarlar, kimlik bilgilerini ve diğer gizli dizileri güvenli hale getirmek için bir anahtar yönetimi çözümü kullanın.
- Hassas verileri koruyun.
- Emniyet önlemleri uygulayın.
- Hata ve özel durum işleme yararlanın.
- Günlüğe kaydetme ve uyarı kullanın.

### <a name="use-a-secure-coding-library-and-a-software-framework"></a>Güvenli kodlama kitaplık ve bir yazılım çerçevesi kullanın

Geliştirme için güvenli kodlama kitaplık ve güvenlik gömülü içeren bir yazılım çerçevesi kullanın. Geliştiriciler, mevcut güvenlik denetimleri sıfırdan geliştirmek yerine özellikleri (şifreleme, giriş İnsan sağlığına uygunluk alanları, çıktı kodlaması, anahtarlar veya bağlantı dizelerini ve diğer her şey bir güvenlik denetimi olarak kabul edilir) kanıtlanmış kullanabilir. Bu, güvenlikle ilgili tasarım ve uygulama açıkları karşı korumanıza yardımcı olur.

Framework'ünüzün Framework'te bulunan tüm güvenlik özellikleri ve en son sürümü kullandığınızdan emin olun. Microsoft'un sunduğu kapsamlı bir [dizi geliştirme aracı](https://azure.microsoft.com/product-categories/developer-tools/) bulut uygulamaları sunmasına olanak herhangi bir platform veya dili üzerinde çalışan tüm geliştiriciler için. Tercih ettiğiniz dili ile çeşitli seçerek kodu [SDK'ları](https://azure.microsoft.com/downloads/).
Tam özellikli tümleşik geliştirme ortamlarından (IDE'ler) yararlanabilir ve Gelişmiş hata ayıklama özellikleri ve yerleşik Azure desteğine düzenleyiciler.

Microsoft'un sunduğu çeşitli [dilleri, çerçeveler ve Araçlar](https://docs.microsoft.com/azure/index#pivot=sdkstools&panel=sdkstools-all) azure'da uygulama geliştirmek için kullanabilirsiniz. Bir örnek [.NET ve .NET Core geliştiricileri için Azure](https://docs.microsoft.com/dotnet/azure/). Her dil ve sunuyoruz framework için hızlı başlangıç kılavuzlarımız, öğreticilerimiz ve hızlı bir başlangıç yapmanıza yardımcı olması için API başvuruları bulabilirsiniz.

Azure Web siteleri ve web uygulamalarını barındırmak için kullandığınız hizmetlere çeşitli sunar. .NET, .NET Core, Java, Ruby, Node.js, PHP veya Python olup olmadığını, en sevdiğiniz dilde geliştirin bu hizmetleri sağlar.
[Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service/app-service-web-overview) (Web uygulamaları) bu hizmetleri biridir.

Web Apps, Microsoft Azure'un gücünü uygulamanıza ekler. Bu, güvenlik, Yük Dengeleme, otomatik ölçeklendirme ve otomatik yönetim içerir. Ayrıca DevOps özelliklerinin Web Apps'te gibi paket yönetimi, ortamları, özel etki alanlarını, SSL/TLS sertifikalarıyla ve Azure DevOps, GitHub, Docker Hub ve diğer kaynaklardan sürekli dağıtım hazırlama yararlanabilirsiniz.

Azure Web siteleri ve web uygulamalarını barındırmak için kullanabileceğiniz diğer hizmetleri sunar. Çoğu senaryo için Web Apps en iyi seçenektir. Bir mikro hizmet mimarisi için göz önünde bulundurun [Azure Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric).
Kodlarınızın çalıştığı sanal makineler üzerinde daha fazla denetime sahip olmanız gerekiyorsa [Azure Sanal Makineler](https://azure.microsoft.com/documentation/services/virtual-machines/)’i düşünün.
Bu Azure hizmetleri arasında seçim yapma hakkında daha fazla bilgi için bkz. bir [Azure App Service, sanal makineler, Service Fabric ve Cloud Services karşılaştırması](https://docs.microsoft.com/azure/app-service/choose-web-site-cloud-service-vm).

### <a name="apply-updates-to-components"></a>Bileşenler için güncelleştirmeleri uygulama

Güvenlik açıklarını önlemek için sürekli olarak hem (örneğin, çerçeveler ve kitaplıklar), istemci tarafı ve sunucu tarafı bileşenleri ve bağımlılıkları güncelleştirmeleri için stok. Yeni güvenlik açıkları ve güncelleştirilmiş yazılım sürümleri sürekli olarak kullanıma sunulur. İzleme, Önceliklendirme ve güncelleştirmeleri uygulamak için devam eden bir planı veya kullandığınız bileşenleri ve kitaplıklar için yapılandırma değişiklikleri olduğundan emin olun.

Bkz [açık Web uygulaması güvenlik Project (OWASP)](https://www.owasp.org/index.php/Main_Page) sayfasında [bileşenleri ile bilinen güvenlik açıklarının kullanarak](https://www.owasp.org/index.php/Top_10-2017_A9-Using_Components_with_Known_Vulnerabilities) aracı öneriler. Kullandığınız bileşenleri için ilişkili güvenlik açıkları için e-posta uyarıları için abone olabilirsiniz.

### <a name="use-threat-modeling-during-application-design"></a>Uygulama tasarımı sırasında modelleme tehdit kullanın

Tehdit modelleme işletme ve uygulama olası güvenlik tehditlerini tanımlamak ve ardından uygun bir risk azaltma işlemleri yerinde olmasını sağlama işlemidir. SDL, takımlar olası sorunları çözümleme görece kolay ve uygun maliyetli olduğunda tasarım aşamasında modelleme tehdit ilgisini belirtir. Tehdit modelleme tasarım aşamasında geliştirme toplam maliyetini önemli ölçüde azaltabilir kullanma.

Tehdit modelleme işlemi kolaylaştırmaya yardımcı olması için tasarladığımız [SDL tehdit modelleme aracı](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool) güvenlikle ilgili olmayan uzmanlarla unutmayın. Bu araç tehdit modelleme tüm geliştiriciler için oluşturma ve tehdit modelleri analiz etme hakkında açık yönergeler sağlayarak kolaylaştırır.

Uygulama tasarımını modelleme ve numaralandırma [STRIDE](https://docs.google.com/viewer?a=v&pid=sites&srcid=ZGVmYXVsdGRvbWFpbnxzZWN1cmVwcm9ncmFtbWluZ3xneDo0MTY1MmM0ZDI0ZjQ4ZDMy) tehditleri — sahtekarlık, kurcalama, ret, bilgi İfşası, hizmet reddi ve ayrıcalıkların yükseltilmesine — tüm güven sınırları kanıtlanmış verimli bir yöntem Tasarım hataları erkenden yakalamak için. Aşağıdaki tabloda STRIDE tehditler listesi ve Azure tarafından sağlanan özellikleri kullanmak bazı örnek risk azaltma işlemleri sağlar. Bu risk azaltma işlemleri her durumda işe yaramaz.

| Tehdit | Güvenlik özelliği | Olası Azure platformu azaltma |
| ---------------------- | --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Kimlik sahtekarlığı               | Authentication        | [HTTPS bağlantılarına ihtiyacınız](https://docs.microsoft.com/aspnet/core/security/enforcing-ssl?view=aspnetcore-2.1&tabs=visual-studio). |
| Kurcalama              | Bütünlük             | SSL/TLS sertifikalarıyla doğrulayın. SSL/TLS kullanan uygulamaları, tam olarak bağlandıkları varlıkları X.509 sertifikaları doğrulamanız gerekir. Azure Key Vault sertifikalarınız [, x509 yönetme sertifikaları](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-vault-certificates). |
| Red            | İnkar       | Azure'ı etkinleştirme [izleme ve tanılama](https://docs.microsoft.com/azure/architecture/best-practices/monitoring).|
| Bilgilerin Açığa Çıkması | Gizliliği       | Hassas verileri şifrelemek [bekleyen](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest) ve [Aktarımdaki](https://docs.microsoft.com/azure/security/azure-security-data-encryption-best-practices#protect-data-in-transit). |
| Hizmet Reddi      | Kullanılabilirlik          | Hizmet Koşulları olası engelleme için performans ölçümlerini izleyin. Bağlantı filtreleri uygulayın. [Azure DDoS koruması](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview#next-steps), uygulama tasarım en iyi yöntemleri ile birlikte, DDoS saldırılarına karşı koruma sağlar.|
| Ayrıcalık Yükseltme | Authorization         | Azure Active Directory kullanan <span class="underline"> </span> [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure).|

### <a name="reduce-your-attack-surface"></a>Saldırı yüzeyinizi azaltmak

Saldırı yüzeyini toplamıdır sum, burada olası güvenlik açıklarını ortaya çıkabilir. Bu belgede size bir uygulamanın saldırı yüzeyi üzerinde odaklanın.
Bir uygulama saldırıdan korumaya biridir. Saldırı yüzeyini en aza indirmek için basit ve hızlı bir şekilde kullanılmayan kaynaklar ve kod uygulamanızdan kaldırmaktır. Küçük, uygulama, daha küçük saldırı yüzeyinizi. Örneğin, kaldırın:

- Özellikler yayımlanmayacak henüz yapmadıysanız kod.
- Hata ayıklama desteği kodu.
- Ağ arabirimleri ve protokolleri, kullanılmayan ya da kullanım dışı bırakıldı.
- Sanal makineler ve kullanmadığınız diğer kaynaklar.

Kaynaklarınızın Normal temizleme yapmak ve kullanılmayan kod kaldırmak sağlama kesintilerinden saldırmak kötü amaçlı aktörler için olmasını sağlamak için harika yolları açıklanmıştır.

Saldırı yüzeyinizi azaltmak için daha ayrıntılı ve ayrıntılı bir şekilde bir saldırı yüzeyi analizi tamamlamak sağlamaktır. Saldırı yüzeyi analizi gözden geçirilebilir ve güvenlik açıklarını test için gereken bir sistemin bölümleri eşlemenize yardımcı olur.

Amacı bir saldırı yüzeyi analizi, geliştiricilere ve güvenlik uzmanlarıyla uygulamanın hangi bölümlerinin saldırılara açık uyumlu olacak şekilde bir uygulama risk alanlarda öğrenmektir. Ardından, ne zaman ve nasıl saldırı yüzeyini değiştirir ve bir risk açısından ne yani olası, bu izlemeyi en aza indirmek için yol bulabilirsiniz.

Saldırı yüzeyi çözümlemesi belirlemenize yardımcı olur:

- İşlevler ve sistem bölümleri gözden geçirin ve güvenlik açıklarını test etmek gerekir.
- Savunma derinlemesine koruma (savunmak için gereken sistemin parçaları) gerektiren yüksek riskli alanları kod.
- Ne zaman saldırı yüzeyini alter ve bir tehdit değerlendirmesi yenilemeniz gerekir.

Saldırganların bir olası zayıf noktayı veya güvenlik açığından yararlanma olanaklarını azaltma, uygulamanızın genel saldırı yüzeyini kapsamlı olarak çözümlemek gerektirir. Ayrıca en az ayrıcalık ilkesinin uygulanması sistem hizmetleri için erişimi kısıtlamak veya devre dışı bırakma içerir ve kullanan savunmaları mümkünse katmanlı.

Ele [bir saldırı yüzeyi incelemesi yürütmeyi](secure-develop.md#conduct-attack-surface-review) SDL, doğrulama aşamasında.

> [!NOTE]
> **Tehdit modelleme ve saldırı yüzeyi çözümlemesi arasındaki fark nedir?**
Tehdit modelleme uygulamanıza olası güvenlik tehditlerini tanımlamak ve tehditlere karşı uygun azaltmaları yerinde olduğundan emin olmak işlemidir. Saldırı yüzeyi çözümlemesi kod saldırılara açık olmayan yüksek riskli alanlarını tanımlar. Bu, uygulamanızın ve gözden geçirme ve uygulamayı dağıtmadan önce bu alanların kod test yüksek riskli alanları korumaya yönelik yöntemler içerir.

### <a name="adopt-a-policy-of-identity-as-the-primary-security-perimeter"></a>Bir ilke kimliğinin birincil güvenlik çevresi olarak benimseyin

Bulut uygulamaları tasarlarken, ağ merkezli bir yaklaşım kimlik merkezli bir yaklaşım için güvenlik çevre odağı genişletmek önemlidir. Tarihsel olarak, birincil şirket içi güvenlik çevresi bir kuruluşun ağına oluştu. Çoğu şirket içi güvenlik tasarımları ağı birincil güvenlik pivot kullanın. Bulut uygulamaları için kimlik birincil güvenlik çevresi olarak dikkate alarak daha iyi sunulur.

Web uygulamaları geliştirmek için kimlik merkezli bir yaklaşım geliştirmek için yapabilecekleriniz:

- Kullanıcılar için çok faktörlü kimlik doğrulamasını zorunlu tutun.
- Güçlü kimlik doğrulaması ve yetkilendirme platformları kullanın.
- En düşük öncelik ilkesini uygulayın.
- Tam zamanında erişim uygulamaktır.

#### <a name="enforce-multi-factor-authentication-for-users"></a>Kullanıcılar için multi-Factor authentication yürürlüğe

İki öğeli kimlik doğrulaması kullanın. Kullanıcı adı ve parola kimlik doğrulaması türlerinde belirlidir güvenlik zayıf önlediği için iki öğeli kimlik doğrulama kimlik doğrulaması ve yetkilendirme geçerli standardıdır. Azure yönetim arabirimleri (Azure portal/uzak PowerShell) ve müşterilere yönelik hizmetler için tasarlanmış verilecek ve kullanmak üzere yapılandırılmış [Azure multi-Factor Authentication](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication).

#### <a name="use-strong-authentication-and-authorization-platforms"></a>Güçlü kimlik doğrulaması ve yetkilendirme platformları kullanın

Platform tarafından sağlanan kimlik doğrulama ve yetkilendirme mekanizmaları yerine özel kod kullanın. Bu durum, özel kimlik doğrulama kodu geliştirme hataya olabilir çünkü. Ticari kod (örneğin, Microsoft'tan) genellikle güvenlik için yaygın olarak incelenir. [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) Azure kimlik ve erişim yönetimi çözümüdür. Bu Azure AD araçları ve Hizmetleri ile güvenli geliştirme yardımcı olur:

- [Azure AD kimlik Platformu (geliştiriciler için Azure AD)](https://docs.microsoft.com/azure/active-directory/develop/about-microsoft-identity-platform) geliştiricilerin güvenli bir şekilde kullanıcılarının oturumunu uygulamalar oluşturmak için kullandığınız bir bulut kimlik hizmetidir. Azure AD, tek kiracılı, satır iş kolu (LOB) uygulamaları oluşturan geliştiriciler ve çok kiracılı uygulamalar geliştirmek isteyen geliştiricileri destekler. Temel oturum açma ek olarak, Azure AD kullanılarak oluşturulan uygulamalar Microsoft APIs ve Azure AD platformunda derlenen özel API'leri çağırabilirsiniz. Azure AD kimlik platformu, OAuth 2.0 ve Openıd Connect gibi endüstri standardı protokolleri destekler.

- [Azure Active Directory B2C (Azure AD B2C)](https://docs.microsoft.com/azure/active-directory-b2c/) özelleştirme ve müşterilerin kaydolması denetim, oturum açın ve uygulamalarınızı kullandıklarında profillerini yönetmek için kullanabileceğiniz bir kimlik yönetimi hizmetidir. Bu, diğerlerinin yanında iOS, Android ve .NET için geliştirilen uygulamaları içerir. Azure AD B2C, müşteri kimliklerini korurken bu eylemleri sağlar.

#### <a name="apply-the-principle-of-least-privilege"></a>En düşük öncelik ilkesini uygulama

Kavramını [en az ayrıcalık](https://en.wikipedia.org/wiki/Principle_of_least_privilege) kullanıcılara işlerini ve hiçbir şey daha fazla yapmak için ihtiyaç duydukları erişim ve denetim kesin düzeyini dair anlamına gelir.

Yazılım geliştiricisi, etki alanı yönetici hakları gerekiyor mu? Yönetici Yardımcısı kişisel bilgisayarlarını yönetici denetimlerine erişim gerekir? Yazılım erişimi değerlendirmek farklı değildir. Kullanırsanız [rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) uygulamanızda kullanıcıların farklı özellikler ve yetki vermek için herkesin vermediği erişmek için her şey. Her rol için gerekli olan erişimi sınırlayarak, bir güvenlik sorunu oluşma riski sınırlayın.

Uygulamanızı zorlar emin olun [en az ayrıcalık](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models#in-applications) kendi erişim desenlerini boyunca.

> [!NOTE]
>  Yazılım ve yazılım oluşturma kişiler uygulamak en az ayrıcalık kuralları gerekir. Yazılım geliştiricileri, çok fazla erişim verilirse büyük bir BT güvenlik riski olabilir. Sonuçları bir geliştirici kötü amaçlı kullanıcılardan veya çok fazla erişim verilen önemli olabilir. Geliştiriciler geliştirme yaşam döngüsü boyunca en az ayrıcalık kuralları uygulanması önerilir.

#### <a name="implement-just-in-time-access"></a>Tam zamanında erişim uygulayın

Uygulama *just-ın-time* ayrıcalıkların tehditlere maruz kalabileceği süreyi daha da azaltmak için (JIT) erişim. Kullanım [Azure AD Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-admin-roles-secure#stage-3-build-visibility-and-take-full-control-of-admin-activity) için:

- Kullanıcılara yalnızca JIT ihtiyaç duydukları izinleri verin.
- Roller için kısaltılmış bir süre ayrıcalıkları otomatik olarak iptal edilir güvenle atayın.

### <a name="require-re-authentication-for-important-transactions"></a>Önemli işlemler için yeniden kimlik doğrulaması gerektir

[Siteler arası istek sahteciliğini](https://docs.microsoft.com/aspnet/core/security/anti-request-forgery?view=aspnetcore-2.1) (diğer adıyla *XSRF* veya *CSRF*) barındırılan web uygulamaları kötü amaçlı web uygulaması istemci tarayıcısı ve bir web arasındaki etkileşimi etkiler karşı bir saldırı olma Bu tarayıcı güvenen uygulama. Siteler arası istek sahteciliği saldırılarına mümkün olduğu Web tarayıcıları, bir Web sitesine kimlik doğrulama belirteçlerinizi bazı türleri her istek ile otomatik olarak gönder.
Bu formu kötüye kullanılma de denir bir *tek tıklamayla saldırı* veya *arabası oturumu* saldırı yararlanır çünkü kullanıcı daha önce oturum kimliği doğrulanmış.

Bu tür bir saldırıya karşı korumak için en iyi yolu, yalnızca kullanıcı önce bir satın alma, hesabı devre dışı bırakma veya bir parola değişikliği gibi önemli sefer sağlayabilen bir şey için kullanıcıya sor sağlamaktır. Parolalarını yeniden girin, captcha tam kullanıcıdan veya yalnızca kullanıcı olması gereken gizli bir belirteç gönderme. Gizli belirteç için en yaygın yaklaşımdır.

### <a name="use-a-key-management-solution-to-secure-keys-credentials-and-other-secrets"></a>Anahtarlar, kimlik bilgilerini ve diğer gizli dizileri güvenli hale getirmek için bir anahtar yönetimi çözümü kullanın

Anahtarlar ve kimlik bilgileri kaybetme sık karşılaşılan bir sorundur. Gereken tek şey anahtarlarını ve kimlik bilgilerini kaybetme yetkisiz bir tarafın sahip daha da kötüsü onlara yönelik erişimi elde edin. Saldırganlar, anahtarları ve GitHub gibi kod depoları depolanan gizli dizileri bulmak için otomatik ve el ile teknikleri avantajlarından yararlanabilirsiniz. Bu ortak kod depoları veya diğer herhangi bir sunucuda, anahtarları ve gizli anahtarları yerleştirmeyin.

Her zaman anahtarlar, sertifikalar, gizli anahtarları ve bağlantı dizeleri bir anahtar yönetimi çözümünde yerleştirin. Anahtarları ve gizli dizileri donanım güvenlik modülleri (HSM'ler) içinde depolanan merkezi bir çözüm kullanabilirsiniz. Azure ile bulutta bir HSM ile size sunar [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault/key-vault-whatis).

Key Vault bir *gizli dizi deposu*: Bu uygulama parolalarını depolamak için bir merkezi bir bulut hizmetidir. Key Vault uygulama gizli dizilerini tek, merkezi bir konumda tutmak ve güvenli erişim izinleri denetim ve erişim günlüğü sağlayarak gizli verilerinizi güvende tutar.

Gizli dizileri, ayrı ayrı depolanır *kasaları*. Her kasa, kendi yapılandırma ve erişimi denetlemek için güvenlik ilkeleri vardır. Çoğu programlama dili için kullanılabilen SDK'sını bir istemci veya bir REST API aracılığıyla verilerinize alın.

> [!IMPORTANT]
> Azure Key Vault, sunucu uygulamaları için yapılandırma gizli dizileri depolamak için tasarlanmıştır. Uygulama kullanıcılarına ait verileri depolamak için tasarlanmamıştır. Bu, kendi performans özellikleri, API ve maliyet modeli yansıtılır.
>
> Kullanıcı verilerini başka bir yerde saydam veri şifrelemesi (TDE) olan bir Azure SQL veritabanı örneğine veya Azure depolama hizmeti şifrelemesi kullanan bir depolama hesabında depolanmalıdır. Bu veri depoları erişmek için uygulamanız tarafından kullanılan gizli dizileri Azure Key Vault'ta tutulabilir.

### <a name="protect-sensitive-data"></a>Hassas verileri koruma

Veri koruma, güvenlik stratejinizin önemli bir parçasıdır.
Güvenlikten ödün veri uygulamanızı tasarlamanıza yardımcı olur, verilerinizi sınıflandırmak ve veri korumanızı tanımlayan gerekir. Duyarlılık tarafından depolanan veri sınıflandırma (kategorilendirme) ve iş etkisi, geliştiriciler verilerle ilişkili riskleri belirlemek yardımcı olur.

Veri biçimleri tasarlarken, tüm geçerli veri hassas olarak etiketleyin. Uygulamanın geçerli veri hassas olarak davranır emin olun. Bu uygulamalar, hassas verilerinizi korumanıza yardımcı olabilir:

- Şifreleme kullanın.
- Sabit kodlama gizli anahtarları ve parolalar gibi kaçının.
- Erişim denetimlerini ve denetim yerinde olduğundan emin olun.

#### <a name="use-encryption"></a>Şifreleme kullanma

Veri koruma, güvenlik stratejinizin önemli bir parçası olmalıdır.
Verilerinizi bir veritabanında depolanıyorsa veya konumlar arasında ileri ve geri taşınırsa, şifreleme kullanın [bekleyen verileri](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest) (while veritabanında) ve şifreleme [Aktarımdaki verileri](https://docs.microsoft.com/azure/security/azure-security-data-encryption-best-practices#protect-data-in-transit) (kullanıcı, gelen ve giden yolu üzerindeki Veritabanı, API ya da hizmet uç noktası). Her zaman SSL/TLS protokolleri veri değişimi için kullanmanızı öneririz. Şifreleme için TLS en son sürümünü kullandığınızdan emin olun (şu anda sürüm 1.2 budur).

#### <a name="avoid-hard-coding"></a>Sabit kodlama kaçının

Bazı şeyler hiçbir zaman yazılımınızı sabit kodlanmış olmalıdır. Ana bilgisayar adlarını veya IP adresleri, URL'leri, e-posta adresleri, kullanıcı adları, parolalar, depolama hesabı anahtarlarını ve diğer şifreleme anahtarlarını örnek verilebilir. Ne yapabilir veya kodunuzu açıklama bölümlerini dahil olmak üzere, kodunuzda sabit kodlanmış etrafında gereksinimleri uygulamayı düşünün.

Kodunuzda açıklamalar koyduğunuzda, herhangi bir önemli bilgi kaydetme emin olun. E-posta adresi, parolalar, bağlantı dizeleri, bir saldırganın bir avantajı, uygulama veya kuruluşunuz saldırmak içinde verebilir yalnızca biri tarafından kuruluşunuz ve başka bir şey bilinir, uygulamayla ilgili bilgileri bu içerir .

Temelde, dağıtıldığında, geliştirme projenizdeki her şeyi genel bilgi olacağını varsayalım. Projede hiçbir hassas verileri eklemekten kaçınır.

Daha önce ele almıştık [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault/key-vault-whatis). Key Vault, anahtarları ve bunları kodlamak yerine parolalar gibi gizli dizileri depolamak için kullanabilirsiniz. Anahtar kasası yönetilen kimlikleri ile birlikte Azure kaynakları için kullandığınızda, Azure web uygulamanızın gizli yapılandırma değerleri kolayca ve güvenli bir şekilde, kaynak denetimini veya yapılandırmasını tüm gizli dizilerin depolanmasında olmadan erişebilir. Daha fazla bilgi için bkz. [gizli dizileri Azure Key Vault ile sunucu uygulamalarınızda yönetme](https://docs.microsoft.com/learn/modules/manage-secrets-with-azure-key-vault/).

### <a name="implement-fail-safe-measures"></a>Emniyet önlemlerini

Uygulamanızı işleyebilen [hataları](https://docs.microsoft.com/dotnet/standard/exceptions/) tutarlı bir şekilde yürütme sırasında oluşur. Uygulama tümünü yakala hataları ve ya da başarısız güvenli veya kapalı.

Ayrıca, hatalar, şüpheli veya kötü amaçlı etkinlikleri belirlemek için yeterli kullanıcı bağlamı ile kaydedilir emin olmalısınız. Günlükleri adli Gecikmeli analize izin vermek yeterli bir zaman tutulmalıdır. Günlükleri, günlük yönetim çözümü tarafından kolayca kullanılabilecek biçiminde olmalıdır. Güvenlikle ilgili hatalar için uyarılar tetiklenir emin olun. Daha fazla sistemleri saldırı ve kalıcılığı sağlamak saldırganlar, yetersiz günlüğe kaydetme ve izleme sağlar.

### <a name="take-advantage-of-error-and-exception-handling"></a>Hata ve özel durum işleme yararlanın

Uygulama doğru hata ve [özel durum işleme](https://docs.microsoft.com/dotnet/standard/exceptions/best-practices-for-exceptions) savunma kodlama, önemli bir parçasıdır. Hata ve özel durum işleme bir sistem güvenilir ve güvenli hale getirmek için önemlidir. Hata işleme hatalar güvenlik açıklarını, saldırganlar platform ve tasarımı hakkında daha fazla bilgi edinin yardımcı olur ve saldırganlar için bilgi sızıntısı gibi farklı türde neden olabilir.

Emin olun:

- Önlemek için merkezi bir şekilde özel durumları işlemek yinelenen [try/catch blokları](https://docs.microsoft.com/dotnet/standard/exceptions/how-to-use-the-try-catch-block-to-catch-exceptions) kod.

- Tüm beklenmeyen davranışlar içindeki uygulama işlenir.

- Kullanıcılara görüntülenen iletileri kritik veri sızıntısı yoktur ancak sorunu açıklamak için yeterli bilgi sağlayın.

- Özel durumlar günlüğe kaydedilir ve adli veya olay yanıt ekiplerinin, araştırmak için yeterli bilgi sağlarlar.

[Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview) için birinci sınıf bir deneyim sağlar [hataları ve özel durumları işleme](https://docs.microsoft.com/azure/logic-apps/logic-apps-exception-handling) bağımlı sistemleri tarafından neden oldu. Logic Apps, görevleri ve uygulamaları, verileri, sistemleri ve Hizmetleri kuruluşlar arasında tümleştirme işlemleri otomatikleştirmek için iş akışları oluşturmak için kullanabilirsiniz.

### <a name="use-logging-and-alerting"></a>Kullanımı günlüğe kaydetme ve uyarı

[Günlük](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1) güvenlik verileri araştırmak için güvenlik sorunları ve Uyarıları tetikleyin kişilerin emin olmak için sorunları hakkında zamanında sorunları bildirin. Denetim ve tüm bileşenlere günlük etkinleştirin. Denetim günlükleri, kullanıcı bağlamı yakalamak ve tüm önemli olayları belirleyin.

Bir kullanıcının gönderdiğini herhangi bir duyarlı veri sitenizde oturum yok denetleyin. Hassas verilerin örnekleri şunlardır:

- Kullanıcı kimlik bilgileri
- Sosyal Güvenlik numaraları veya diğer tanımlama bilgileri
- Kredi kartı numaraları veya diğer finansal bilgi
- Sistem durumu bilgileri
- Özel anahtarlar veya şifrelenmiş bilgilerin şifresini çözme için kullanılan diğer veriler
- Uygulama daha etkili bir şekilde saldırmak için kullanılacak sistem veya uygulama bilgileri

Uygulama başarılı ve başarısız kullanıcı oturumu açma, parola sıfırlama, parola değişiklikleri, hesap kilitleme ve kullanıcı kaydı gibi kullanıcı yönetimi olayları izler emin olun. Bu olayları günlüğe kaydetme, algılamak ve tepki şüpheli olabilecek davranışları için yardımcı olur. Ayrıca, uygulamayı kimin erişmekte olduğunu gibi işlemleri verileri toplamak sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelerde, güvenlik denetimleri öneririz ve yardımcı olabilecek etkinlikleri geliştirip güvenli uygulamalar dağıtın.

- [Güvenli uygulamalar geliştirin](secure-develop.md)
- [Güvenli uygulamalar dağıtma](secure-deploy.md)
