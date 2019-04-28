---
title: PaaS dağıtımlarının güvenliğini sağlama | Microsoft Docs
description: " Paas'nin avantajlarını güvenlik ve diğer bulut hizmeti modeli ve Azure PaaS dağıtımınızın güvenliğini sağlamaya yönelik önerilen adımları öğrenin anlayın. "
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: techlake
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2019
ms.author: terrylan
ms.openlocfilehash: e833317fa16576fa0006a774226d12974fd93ed8
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62107455"
---
# <a name="securing-paas-deployments"></a>PaaS dağıtımlarının güvenliğini sağlama

Bu makalede yardımcı olacak bilgileri sağlar:

- Uygulamaları bulutta barındırma güvenlik avantajlarını öğrenin
- Platform güvenlik avantajlarından hizmet (PaaS) ve diğer bulut hizmeti modeli olarak değerlendir
- Güvenlik odağınızı bir ağ merkezli bir kimlik odaklı çevre güvenlik yaklaşımı değiştirme
- Genel PaaS güvenlik en iyi yöntem önerilerini uygulama

## <a name="cloud-security-advantages"></a>Bulut güvenlik avantajları
Bulutta olan güvenlik avantajları vardır. Kuruluşların büyük olasılıkla bir şirket içi ortamda sahip karşılaşılmamış sorumluluklar ve burada saldırganlar tarafından tüm katmanlarda güvenlik açıklarından bir ortam oluşturur güvenlik, yatırım için kullanılabilen sınırlı kaynaklar.

![Bulut çağının güvenlik avantajları][1]

Kuruluşlar, bir sağlayıcıya ait bulut tabanlı güvenlik özellikleri ve bulut zekasını kullanarak kendi tehdit algılama ve yanıt sürelerini iyileştirebilirsiniz.  Bulut sağlayıcısı sorumlulukları ilerletmeniz, kuruluşlar güvenlik kaynakları ve diğer iş önceliklerini bütçeye yeniden ayırmak üzere etkinleştirir daha fazla güvenlik kapsamı elde edebilirsiniz.

## <a name="division-of-responsibility"></a>Sorumluluk bölme
Siz ve Microsoft arasında Sorumluluklar anlamak önemlidir. Şirket içinde tam bir yığın sahipseniz, ancak bazı sorumlulukları buluta taşıdıkça transfer Microsoft. Aşağıdaki sorumluluk matris alanları yığınının sorumlu olan bir SaaS, PaaS ve Iaas dağıtım gösterir ve Microsoft sorumludur.

![Sorumluluk bölgeleri][2]

Tüm bulut dağıtım türleri için veri ve kimlikler size aittir. Verilerinizi ve kimlikleri, şirket içi kaynakları ve bulut bileşenlerinin güvenliğini korumak için sorumludur (Bu hizmet türüne göre değişiklik gösterir) siz denetlersiniz.

Dağıtım türü ne olursa olsun size her zaman korunur sorumlulukları şunlardır:

- Veriler
- Uç Noktalar
- Hesap
- Erişim Yönetimi

## <a name="security-advantages-of-a-paas-cloud-service-model"></a>Bir PaaS güvenlik avantajlarından bulut hizmeti modeli
Aynı sorumluluk matris kullanarak, şirket içi Azure PaaS dağıtımının güvenlik avantajları göz atalım.

![Paas'nin avantajlarını güvenlik][3]

Yığın, fiziksel altyapı alt kısmında başlangıç Microsoft ortak riskleri ve sorumlulukları azaltır. Microsoft bulut sürekli olarak Microsoft tarafından izlenen, saldırmak zor olmasıdır. Bu Microsoft bulut hedefi olarak ele geçiren bir saldırgan için anlam ifade etmez. Saldırgan para ve kaynaklar, saldırgan çok sayıda sahip olmadığı sürece başka bir hedefe taşımak olasıdır.  

Yığın ortasında bir PaaS dağıtımı ve şirket içi arasında fark yoktur. Uygulama katmanı ve hesabı ve erişim yönetimi katmanı, benzer riskler vardır. Sonraki adımlar bölümünde bu makalede, biz size ortadan kaldırmayı veya bu riskleri en aza indirmek için en iyi uygulamaları için yol gösterir.

Yığın, veri yönetimi ve rights management'ın üstünde, Anahtar Yönetimi tarafından azaltılabilir bir risk yap. (Anahtar yönetimi en iyi yöntemleri ele alınmıştır.) Anahtar Yönetimi ek bir sorumluluktur olsa da, artık anahtar yönetimi için kaynakları kaydırabilirsiniz şekilde yönetmeniz gereken bir PaaS dağıtımı alanlarda sahip.

Azure platformu da, güçlü DDoS koruması ağ tabanlı teknolojiler kullanarak sağlar. Ancak, DDoS koruması yöntemlerinin ağ tabanlı tüm türleri, bağlantı başına ve veri merkezi başına temelinde kendi sınırlara sahiptir. Büyük DDoS saldırılarının etkisini önlemek için hızla ve otomatik olarak DDoS saldırılarına karşı korumaya için ölçeği genişletme sağlayarak, Azure'un çekirdek bulut özelliğinden yararlanabilirsiniz. Daha fazla ayrıntıya nasıl önerilen uygulamaların makaleler, bunu yapabilirsiniz üzerinde gideceğiz.

## <a name="modernizing-the-defenders-mindset"></a>Defender'ın anlayış modernleştirme
PaaS ile dağıtımları, bütün olarak yaklaşımı bir kayma güvenlik gelir. Her şeyi kendiniz sorumluluk Microsoft'la paylaşmayı denetlemek gerek tıklattığınızda.

PaaS ve geleneksel şirket içi dağıtımlar arasında başka bir önemli fark, birincil güvenlik çevresi tanımlar için yeni bir görünüm olduğundan. Tarihsel olarak, birincil şirket içi güvenlik çevre ağınızda edildi ve çoğu şirket içi güvenlik tasarımı, birincil güvenlik Özet ağ kullanın. PaaS dağıtımları için kimlik birincil güvenlik çevresi olarak dikkate alarak daha iyi sunulur.

## <a name="adopt-a-policy-of-identity-as-the-primary-security-perimeter"></a>Bir ilke kimliğinin birincil güvenlik çevresi olarak benimseyin
Bulut ağ merkezli daha az ilgili düşünmek getiren geniş ağ erişimi olan beş temel özellik biri. Amacı, kullanıcıların kaynakları konumdan bağımsız olarak erişmesine izin vermek için bulut çoğunu. Çoğu kullanıcı için konumlarına yere İnternette olacağı.

Aşağıdaki şekil, güvenlik çevresi bir kimlik çevre için bir ağ çevresi nasıl geliştirildiğini gösterir. Güvenlik savunma ağınız hakkında daha az ve daha fazlası hakkında verilerinizi savunma yanı uygulamaları ve kullanıcılarınızın güvenliğini yönetme olur. Şirketiniz için önemli olan için güvenlik yakın göndermek istediğiniz anahtar farktır.

![Yeni güvenlik çevresi olarak kimlik][4]

Başlangıçta, Azure PaaS hizmetlerine (örneğin, web rolleri ve Azure SQL) çok az kayıpla veya hiç geleneksel ağ çevre savunmaları sağladı. Internet'e (web rolü) sağlamak öğenin amaçlı olan ve yeni bir çevre (örneğin, BLOB veya Azure SQL) söz konusu kimlik doğrulamasını sağlar algılandı.

Modern güvenlik uygulamaları, saldırganın ağ çevresi aşmış varsayılır. Bu nedenle, savunma modern uygulamalar için kimlik taşıdık. Kuruluşlar, bir kimlik tabanlı güvenlik çevresi güçlü kimlik doğrulaması ve yetkilendirme sağlığı (iyi) ile oluşturmanız gerekir.

İlkeleri ve modeller için ağ çevre yıllardır kullanılabilir olmuştur. Buna karşılık, sektör kimliği birincil güvenlik çevresi olarak kullanmaya ile görece küçük bir deneyimi vardır. Bu kullanıcıyla Bununla birlikte, alanında kanıtlanmış ve neredeyse tüm PaaS Hizmetleri için geçerli olan bazı genel öneriler sağlamak için yeterli deneyimi birikti.

Kimlik çevre yönetmek için en iyi uygulamalar aşağıda verilmiştir.

**En iyi yöntem**: Anahtarları ve PaaS dağıtımınızın güvenliğini sağlamak için kimlik bilgilerini güvenli hale getirin.   
**Ayrıntı**: Anahtarlar ve kimlik bilgileri kaybetme sık karşılaşılan bir sorundur. Anahtarları ve gizli dizileri donanım güvenlik modülleri (HSM'ler) burada depolanabilir, merkezi bir çözüm kullanabilirsiniz. [Azure Key Vault](../key-vault/key-vault-whatis.md) kimlik doğrulaması anahtarları, depolama hesabı anahtarları, veri şifreleme anahtarları, .pfx dosyaları ve Hsm'leri tarafından korunan anahtarları kullanarak parolalar şifreleyerek anahtarlarınızı ve gizli bilgilerinizi korur.

**En iyi yöntem**: Kaynak kodu veya GitHub kimlik bilgilerini ve diğer gizli dizileri yerleştirmeyin.   
**Ayrıntı**: Gereken tek şey anahtarlarını ve kimlik bilgilerini kaybetme yetkisiz bir tarafın sahip daha da kötüsü onlara yönelik erişimi elde edin. Saldırganlar bot anahtarları ve GitHub gibi kod depoları içinde depolanan gizli bulmak için teknolojileri yararlanabilirsiniz. Anahtar ve gizli anahtarları bu ortak kod depolarında koymayın.

**En iyi yöntem**: Karma PaaS, sanal makine yönetim arabirimlerindeki korumak ve Iaas Hizmetleri, Uzak sağlayan bir yönetim arabirimini kullanarak bu VM'lerin doğrudan yönetin.   
**Ayrıntı**: Uzaktan Yönetim protokolleri gibi [SSH](https://en.wikipedia.org/wiki/Secure_Shell), [RDP](https://support.microsoft.com/kb/186607), ve [PowerShell uzaktan iletişimini](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/enable-psremoting) kullanılabilir. Genel olarak, doğrudan uzak erişim VM'ler için internet'ten etkinleştirmemeniz önerilir.

Mümkünse, bir Azure sanal ağında sanal özel ağlar kullanma gibi alternatif yaklaşımlar kullanın. Alternatif yaklaşımlar kullanılabilir durumda değilse, karmaşık parolaları ve iki öğeli kimlik doğrulama kullandığınızdan emin olun (gibi [Azure multi-Factor Authentication](../active-directory/authentication/multi-factor-authentication.md)).

**En iyi yöntem**: Güçlü kimlik doğrulaması ve yetkilendirme platformları kullanın.   
**Ayrıntı**: Özel kullanıcı depoları yerine Azure AD'de Federasyon kimlikleri kullanın. Federasyon kimlikleri kullandığınızda, platform temelli bir yaklaşım yararlanın ve yetkili kimlik yönetimini, iş ortakları için temsilci. Federe kimlik yaklaşım çalışanlar sonlandırılır ve birden çok kimlik doğrulaması ve yetkilendirme sistemler arasında yansıtılması bilgi gerekiyor, özellikle önemlidir.

Platform tarafından sağlanan kimlik doğrulama ve yetkilendirme mekanizmaları yerine özel kod kullanın. Özel kimlik doğrulama kodu geliştirme hatalar oluşabilir nedenidir. Geliştiricilerinizin çoğu güvenlik konusunda uzman olmayan ve ıot'nin ve kimlik doğrulama ve yetkilendirme son gelişmeleri dikkat edilmesi gereken düşüktür. Ticari koddan (örneğin, Microsoft), genellikle kapsamlı bir şekilde gözden güvenlik sağlıyor.

İki öğeli kimlik doğrulaması kullanın. Kullanıcı adı ve parola kimlik doğrulaması türlerinde devralınan güvenlik zayıf önlediği için iki öğeli kimlik doğrulama kimlik doğrulaması ve yetkilendirme geçerli standardıdır. Erişim Azure Yönetim (portal/uzak PowerShell) arabirimleri ve müşterilere yönelik hizmetler için tasarlanmış verilecek ve kullanmak üzere yapılandırılmış [Azure multi-Factor Authentication](../active-directory/authentication/multi-factor-authentication.md).

OAuth2 ve Kerberos gibi standart kimlik doğrulama protokolleri kullanır. Bu protokollerin kapsamlı bir şekilde gözden eş atanmış olması ve büyük olasılıkla kimlik doğrulaması ve yetkilendirme platformu Kitaplıklarınızı bir parçası olarak uygulanır.

## <a name="use-threat-modeling-during-application-design"></a>Uygulama tasarımı sırasında modelleme tehdit kullanın
Microsoft [güvenlik geliştirme yaşam döngüsü](https://www.microsoft.com/en-us/sdl) takımlar tehdit modelleme tasarım aşamasında adlı bir işlem ilgisini belirtir. Microsoft bu işlemi kolaylaştırmaya yardımcı olması için oluşturduğu [SDL tehdit modelleme aracı](../security/azure-security-threat-modeling-tool.md). Uygulama tasarımını modelleme ve numaralandırma [STRIDE](https://docs.google.com/viewer?a=v&pid=sites&srcid=ZGVmYXVsdGRvbWFpbnxzZWN1cmVwcm9ncmFtbWluZ3xneDo0MTY1MmM0ZDI0ZjQ4ZDMy) genelinde tüm güven sınırları yakalayabiliyorsa tehditleri tasarım hataları erkenden.

Aşağıdaki tabloda STRIDE tehditler listesi ve Azure özelliklerini kullanmak bazı örnek risk azaltma işlemleri sağlar. Bu risk azaltma işlemleri her durumda işe yaramaz.

| Tehdit | Güvenlik özelliği | Azure platformu hafifletmelerle |
| --- | --- | --- |
| Kimlik sahtekarlığı | Kimlik Doğrulaması | HTTPS bağlantıları gerektirir. |
| Kurcalama | Bütünlük | SSL sertifikaları doğrulayın. |
| Red | İnkar edilemez | Azure'ı etkinleştirme [izleme ve tanılama](https://docs.microsoft.com/azure/architecture/best-practices/monitoring). |
| Bilgilerin açığa çıkması | Gizliliği | Bekleyen hassas verileri kullanarak şifrelenip şifrelenmeyeceğinizi [hizmet sertifikaları](https://docs.microsoft.com/rest/api/appservice/certificates). |
| Hizmet reddi | Kullanılabilirlik | Olası hizmet reddi koşulları için performans ölçümlerini izleyin. Bağlantı filtreleri uygulayın. |
| Ayrıcalık yükseltme | Yetkilendirme | Kullanım [Privileged Identity Management](../active-directory/privileged-identity-management/subscription-requirements.md). |

## <a name="develop-on-azure-app-service"></a>Azure App Service üzerinde geliştirme
[Azure App Service](../app-service/overview.md) olduğu web ve herhangi bir platform veya cihaz için mobil uygulamalar oluşturmak ve bulutta veya şirket içinde her yerden, veri bağlama bir PaaS teklifidir olanak tanır. App Service, web ve daha önce ayrı olarak Azure Web siteleri ve Azure mobil hizmetler teslim edilen mobil özelliklerini içerir. Ayrıca iş süreçlerini otomatikleştirmek ve bulut API'leri barındırmak için yeni özellikler içerir. Tek bir tümleşik hizmet olarak App Service özellikleri, Web, mobil ve tümleştirme senaryolarına ilişin zengin.

App Service kullanmaya yönelik en iyi uygulamalar aşağıda verilmiştir.

**En iyi yöntem**: [Azure Active Directory aracılığıyla kimlik doğrulaması](../app-service/overview-authentication-authorization.md).   
**Ayrıntı**: App Service kimlik sağlayıcınız için bir OAuth 2.0 hizmeti sunar. OAuth 2.0 istemci Geliştirici Basitlik, web uygulamaları, Masaüstü uygulamaları ve mobil telefonlar için özel yetkilendirme akışları sağlarken odaklanır. Azure AD OAuth 2.0 web uygulamaları ve mobil erişim yetkisi vermek sağlamak için kullanır.

**En iyi yöntem**: Bilinmesi gerekenler ve en az ayrıcalık güvenlik ilkelerine dayalı olarak erişimi kısıtlayın.   
**Ayrıntı**: Erişim sınırlama, veri erişimi için güvenlik ilkelerini zorlamak istediğinizde kuruluşlar için zorunludur. RBAC, kullanıcılara, gruplara ve uygulamalara belirli bir kapsamda izinleri atamak için kullanabilirsiniz. Kullanıcılar uygulamalara erişim verme hakkında daha fazla bilgi için bkz: [erişim yönetimini kullanmaya başlama](../role-based-access-control/overview.md).

**En iyi yöntem**: Anahtarlarınızı koruyun.   
**Ayrıntı**: Azure Key Vault, şifreleme anahtarlarını korumak yardımcı olur ve bulut uygulamaları ve Hizmetleri gizli dizileri kullanabilirsiniz. Anahtar kasası ile anahtarları ve gizli anahtarları (örneğin, kimlik doğrulaması anahtarları, depolama hesabı anahtarları, veri şifreleme anahtarları şifrelemenize PFX dosyaları ve parolalar), donanım güvenlik modülleri (HSM'ler) tarafından korunan anahtarları kullanarak. Ek güvenlik için HSM'lerde anahtarları içeri aktarabilir veya oluşturabilirsiniz. Bkz: [Azure anahtar kasası](../key-vault/key-vault-whatis.md) daha fazla bilgi için. Key Vault, otomatik olarak yenilenmesi, TLS sertifikalarını yönetmek için de kullanabilirsiniz.

**En iyi yöntem**: Gelen kaynak IP adresleri kısıtlayın.   
**Ayrıntı**: [App Service ortamı](../app-service/environment/intro.md) gelen kaynak IP adresleri aracılığıyla ağ güvenlik grupları kısıtlamanıza yardımcı olan bir sanal ağ tümleştirme özelliğine sahiptir. Sanal ağları Azure kaynaklarına erişimi denetleyebilirsiniz internet olmayan, yönlendirilebilir bir ağda yerleştirmenize olanak sağlar. Daha fazla bilgi için bkz. [uygulamanızı bir Azure sanal ağı ile tümleştirme](../app-service/web-sites-integrate-with-vnet.md).

**En iyi yöntem**: App Service ortamınızı güvenlik durumunu izleyin.   
**Ayrıntı**: Azure güvenlik App Service ortamınızı izlemek için Merkezi'ni kullanın. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde, oluşturduğu [önerileri](../security-center/security-center-virtual-machine-recommendations.md) , konusunda size gerekli denetimlerin yapılandırılması işlemi boyunca.

> [!NOTE]
> App Service'ı izleme olan Önizleme aşamasındadır ve yalnızca [standart katman](../security-center/security-center-pricing.md) Güvenlik Merkezi.
>
>

## <a name="install-a-web-application-firewall"></a>Bir web uygulaması Güvenlik Duvarı'nı yükleme
Web uygulamaları, bilinen yaygın güvenlik açıklarından yararlanan kötü amaçlı saldırıların giderek daha fazla hedefi olmaktadır. Bu açıklardan yararlanma örnekleri arasında SQL ekleme saldırıları, siteler arası komut dosyası saldırıları yaygındır. Uygulama kodunda bu tür saldırıların önlenmesi zor olabilir ve uygulama topolojisinin birçok katmanında ayrıntılı bakım, düzeltme eki uygulama ve izleme işlemleri gerektirebilir. Merkezi bir web uygulaması güvenlik duvarı, güvenlik yönetimini çok daha kolay hale getirir ve yetkisiz erişim ya da izinsiz giriş tehditlerine karşı uygulama yöneticilerine daha iyi güvence verir. Bir WAF çözümü, bilinen bir güvenlik açığına merkezi bir konumda düzeltme eki uygulayarak güvenlik tehdidine karşı, web uygulamalarının her birinin güvenliğini sağlamaya göre daha hızlı tepki verebilir. Var olan uygulama ağ geçitleri, web uygulaması güvenlik duvarı bulunan bir uygulama ağ geçidine kolaylıkla dönüştürülebilir.

[Web uygulaması Güvenlik Duvarı (WAF)](../application-gateway/waf-overview.md) web uygulamalarınızda açıklardan yararlanmaya ve güvenlik açıkları merkezi koruma sağlayan bir Application Gateway özelliğidir. WAF kurallarını temel [açık Web uygulaması güvenlik Project (OWASP) çekirdek kural kümeleri](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3.0 veya 2.2.9'daki.

## <a name="monitor-the-performance-of-your-applications"></a>Uygulamalarınızın performansını izleme
İzleme, performans, sistem durumu ve uygulamanızın kullanılabilirliğini belirlemeye yönelik verileri toplayıp analiz eden işlemidir. Etkili bir izleme stratejisi, uygulamanız için bileşenlerin ayrıntılı işlemini anlamanıza yardımcı olur. Bu sorun haline gelmeden önce onları giderebilmeniz kritik sorunları size bildirerek çalışma artırmanıza yardımcı olur. Ayrıca, güvenlikle ilgili olabilecek anormallikleri yardımcı olur.

Kullanım [Azure Application Insights](https://azure.microsoft.com/documentation/services/application-insights) bulutta veya şirket içinde barındırılan, kullanılabilirlik, performans ve, uygulamanızın kullanımını izlemek için. Application Insights'ı kullanarak, hızlı bir şekilde tanımlamak ve bunları rapor bir kullanıcının bildirmesini beklemeden uygulamanızdaki hataları tanılayın. Topladığınız bilgilerle, uygulamanızın bakımı ve iyileştirmeleri ile ilgili bilinçli kararlar alabilirsiniz.

Application Insights, topladığı verilerle etkileşim kurmak için kapsamlı araçlara sahiptir. Application Insights, verilerini genel bir depoda saklar. Bu uyarılar, panolar ve Kusto sorgu dili ile ayrıntılı analiz gibi paylaşılan işlevselliği yararlanabilirsiniz.



## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, bir Azure PaaS dağıtımı ve bulut uygulamalarına yönelik en iyi güvenlik uygulamaları güvenlik avantajlarından odaklandık. Ardından, PaaS web ve mobil çözümler kullanarak belirli Azure hizmetlerinin güvenliğini sağlamak için önerilen yöntemleri öğrenin. Azure App Service, Azure SQL veritabanı ve Azure SQL veri ambarı ve Azure depolama ile başlayacağız. Diğer Azure Hizmetleri için önerilen uygulamaların makalelerde kullanılabilir oldukça aşağıdaki listede bağlantıları sağlanır:

- [Azure App Service](security-paas-applications-using-app-services.md)
- [Azure SQL veritabanı ve Azure SQL veri ambarı](security-paas-applications-using-sql.md)
- [Azure Depolama](security-paas-applications-using-storage.md)
- Redis için Azure Önbelleği
- Azure Service Bus
- Web uygulaması güvenlik duvarları

Bkz: [Azure güvenlik en iyi uygulamaları ve desenleri](security-best-practices-and-patterns.md) kullanmak üzere daha fazla güvenlik için en iyi yöntemler, tasarlama, dağıtma ve Azure'ı kullanarak bulut çözümlerinizi yönetme.

Aşağıdaki kaynaklar, Azure güvenliği ve ilgili Microsoft Hizmetleri hakkında daha fazla genel bilgi sağlamak kullanılabilir:
* [Azure güvenlik ekibi blogu](https://blogs.msdn.microsoft.com/azuresecurity/) - Azure güvenliği en güncel bilgi için
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx) - Azure ile ilgili sorunlar dahil Microsoft güvenlik açıklarının bildirilebileceği veya e-posta aracılığıyla secure@microsoft.com

<!--Image references-->
[1]: ./media/security-paas-deployments/advantages-of-cloud.png
[2]: ./media/security-paas-deployments/responsibility-zones.png
[3]: ./media/security-paas-deployments/advantages-of-paas.png
[4]: ./media/security-paas-deployments/identity-perimeter.png
