---
title: PaaS dağıtımları güvenli hale getirme | Microsoft Docs
description: " PaaS güvenlik avantajlarından diğer karşı bulut hizmet modeli ve Azure PaaS dağıtımınızın güvenliğini sağlamaya yönelik önerilen yöntemleri öğrenin anlayın. "
services: security
documentationcenter: na
author: techlake
manager: MBaldwin
editor: techlake
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: f19c52629a997687692eef9bce2e13b2b7894052
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31794744"
---
# <a name="securing-paas-deployments"></a>PaaS dağıtımlarının güvenliğini sağlama

Bu makale size yardımcı olacak bilgileri sağlar:

- Uygulamalarını bulutta barındırma güvenlik avantajlarından anlama
- Diğer bulut hizmet modeli karşılaştırması (PaaS) hizmet olarak platform güvenlik avantajlarından değerlendir
- Güvenlik odağınız bir ağ merkezli bir kimlik merkezli çevre güvenlik yaklaşım değiştirme
- Genel PaaS güvenlik en iyi uygulama önerilerini uygulama

## <a name="cloud-security-advantages"></a>Bulut güvenlik avantajları
Bulutta olmanın güvenlik avantajları vardır. Kuruluşların büyük olasılıkla bir şirket içi ortamda sahip unmet sorumluluklar ve sınırlı kaynak burada saldırganlar tüm katmanlar, açıklardan yararlanmak için bir ortam oluşturur güvenlik yatırım kullanılabilir.

![Bulut dönem güvenlik avantajları][1]

Kuruluşların bir sağlayıcının bulut tabanlı güvenlik özellikleri ve bulut Intelligence kullanarak kendi tehdit algılama ve yanıt sürelerini iyileştirebilirsiniz.  Bulut sağlayıcısı sorumlulukları kaydırma tarafından kuruluşların güvenlik kaynakları ve diğer iş önceliklerini bütçeye yeniden ayırmak üzere sağlayan daha fazla güvenlik kapsamı elde edebilirsiniz.

## <a name="division-of-responsibility"></a>Sorumluluk bölme
Ve Microsoft arasında sorumluluk bölümünün anlamak önemlidir. Şirket içi, tüm yığın sahipseniz, ancak bazı sorumlulukları buluta taşımak gibi transfer Microsoft. Sorumlu olan bir SaaS, PaaS ve Iaas dağıtımında yığın alanlarının aşağıdaki sorumluluk matrisi gösterir ve Microsoft sorumludur.

![Sorumluluk bölgeleri][2]

Tüm bulut dağıtım türleri için veri ve kimlikler sahip. Verilerinizi ve kimlikleri, şirket içi kaynaklar ve bulut bileşenleri güvenliğini korumak sizin sorumluluğunuzdadır (hangi hizmet türüne göre farklılık gösterir), denetim.

Her zaman dağıtım türü ne olursa olsun, tarafından korunur sorumlulukları şunlardır:

- Veriler
- Uç Noktalar
- Hesap
- Erişim Yönetimi

## <a name="security-advantages-of-a-paas-cloud-service-model"></a>Hizmet modeli bir PaaS güvenlik avantajlarından bulut
Aynı sorumluluk matrisi kullanarak, bir Azure PaaS dağıtımı şirket içi karşı güvenlik avantajlarından bakalım.

![PaaS güvenlik avantajları][3]

Yığın, fiziksel altyapı alt kısmındaki Başlangıç Microsoft ortak riskleri ve sorumlulukları azaltır. Microsoft bulut sürekli olarak Microsoft tarafından izlenen, bunun saldırmak zor olmasıdır. Bu, saldırganın hedefi olarak Microsoft bulut işin peşine düşmek doesn't make Sense. Saldırgan para ve kaynakları, saldırganın çok sayıda olmadıkça başka bir hedefe taşımaya olasıdır.  

Yığın ortasında bir PaaS dağıtımı ve şirket içi arasında fark yoktur. Uygulama katmanı ve hesabı ve erişim yönetimi katmanı, benzer riskleri vardır. Sonraki adımlar bölümünde bu makalede, ortadan kaldırmak veya bu riskleri en aza indirmek için en iyi uygulamalar için rehberlik ediyoruz.

Yığın, veri yönetimi ve hak yönetimi üst kısmında, Anahtar Yönetimi tarafından azaltılabilir bir riskini alırsınız. (Anahtar yönetimi en iyi yöntemleri ele alınmıştır.) Anahtar Yönetimi ek sorumluluk olsa da, artık anahtar yönetimi için kaynakları kaydırabilirsiniz şekilde yönetmeniz gereken bir PaaS dağıtımı alanlarında sahip.

Azure platformu Ayrıca, güçlü DDoS koruması çeşitli ağ tabanlı teknolojileri kullanılarak sağlar. Ancak, tüm ağ tabanlı DDoS koruması yöntemleri sınırlarının bir bağlantı başına ve veri merkezi başına temelinde vardır. Büyük DDoS saldırıların etkisini önlemek için hızla ve otomatik olarak DDoS saldırılarına karşı korumak için genişletilecek sağlayarak, Azure'nın çekirdek bulut yeteneğinden sürebilir. Nasıl önerilen uygulamalar makalelerinde bunu yapabilirsiniz üzerinde size daha fazla ayrıntıya gidersiniz.

## <a name="modernizing-the-defenders-mindset"></a>Defender'ın bakış modernizing
İle PaaS dağıtımları için güvenlik genel yaklaşım, SHIFT gelir. Her şeyi kendinizi Microsoft ile sorumluluk paylaşımını denetlemek gerek shift.

Başka bir önemli PaaS geleneksel şirket içi dağıtımlar arasındaki farktır ne birincil güvenlik çevre tanımlar, yeni bir görünüm. Tarihsel olarak, birincil şirket içi güvenlik çevre ağınızda oldu ve çoğu şirket içi güvenlik tasarımı, kendi birincil güvenlik Özet olarak ağ kullanır. PaaS dağıtımları için birincil güvenlik çevre olmasını kimlik dikkate alarak daha iyi sunulur.

## <a name="identity-as-the-primary-security-perimeter"></a>Birincil güvenlik çevre olarak kimliği
Bulut ağ merkezli daha az ilgili düşünüyorum yapar geniş ağ erişimi olan bilgi işlem beş temel özellik biri. Amacı kullanıcıların konum bağımsız olarak kaynaklarına erişmesine izin vermek için bulut bilgi işlemin çoğunu. Çoğu kullanıcı için konumlarını yere Internet'te olacak.

Güvenlik çevre bir kimlik çevre ağ çevre nasıl gelişmiştir aşağıdaki şekilde gösterilmiştir. Güvenlik ağınızın savunma hakkında daha az ve daha fazla hakkında verilerinizi savunma, yanı sıra güvenlik uygulamaları ve kullanıcıları yönetme olur. Yakın güvenlik nedir şirketiniz için önemli itme istediğiniz anahtar farktır.

![Yeni güvenlik çevre olarak kimliği][4]

Başlangıçta, Azure PaaS Hizmetleri (örneğin, web rolleri ve Azure SQL) çok az kayıpla veya hiç geleneksel ağ çevre savunma sağlanan. (Web rolü) Internet'e açığa çıkarılması öğenin amacı olan ve yeni bir çevre (örneğin, BLOB veya Azure SQL), kimlik doğrulamasının sağlar algılandı.

Modern güvenlik uygulamaları etkilemeyi ağ çevre İhlale yol varsayalım. Bu nedenle, modern savunma yöntemler kimliğine taşınmış olması. Kuruluşlar, güçlü kimlik doğrulama ve yetkilendirme sağlığı (iyi) ile kimlik tabanlı güvenlik çevre oluşturmanız gerekir.

## <a name="recommendations-for-managing-the-identity-perimeter"></a>Kimlik çevre yönetme ile ilgili öneriler

İlkeleri ve ağ çevre modeller için on yılları kullanılabilir olmuştur. Buna karşılık, endüstri kimlik birincil güvenlik çevre kullanarak görece daha az deneyimi olur. İle denirse, biz alanında kanıtlanmış ve neredeyse tüm PaaS hizmet için geçerli olan bazı genel öneriler sağlamak için yeterli deneyimi toplanan.

Kimlik çevre yönetmek için genel bir en iyi uygulama yaklaşımı özetler.

- **Anahtarları veya kimlik bilgilerini kaybetmeyin** anahtarlarını ve kimlik bilgilerini güvenli hale getirme PaaS dağıtımları güvenliğini sağlamak için önemlidir. Anahtarları ve kimlik bilgileri kaybetme yaygın görülen bir sorundur. İyi çözümlerden biri, anahtarları ve gizli anahtarları donanım güvenlik modülleri (HSM) depolanabileceği bir merkezi çözümü kullanmaktır. Azure ile bulutta HSM sağlar [Azure anahtar kasası](../key-vault/key-vault-whatis.md).
- **Kimlik bilgileri ve diğer parolaları kaynak kodu veya GitHub koymayın** tek şey anahtarlarını ve kimlik bilgilerini kaybetme yetkisiz bir tarafın sahip daha da kötüsü erişmesini sağlamak. Saldırganlar, anahtarları ve gizli anahtarları kod depoları GitHub gibi depolanan bulma teknolojileri bot yararlanamazsınız. Bu ortak kaynak kodu depolarına anahtarı ve gizli anahtarları koymayın.
- **Karma PaaS ve Iaas Hizmetleri, VM yönetim arabirimlerindeki korumak** Iaas ve PaaS Hizmetleri sanal makinelerde (VM) çalışır. Hizmet türüne bağlı olarak birkaç yönetim arabirimleri kullanılabilir uzaktan yönettiğiniz bu Vm'lere doğrudan o etkinleştir. Uzaktan Yönetim protokolleri gibi [Secure Shell Protokolü (SSH)](https://en.wikipedia.org/wiki/Secure_Shell), [Uzak Masaüstü Protokolü (RDP)](https://support.microsoft.com/kb/186607), ve [uzak PowerShell](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/enable-psremoting) kullanılabilir. Genel olarak, doğrudan uzak erişim VM'ler için Internet'ten etkinleştirmemeniz önerilir. Mevcut ise, Azure sanal ağda sanal özel ağ kullanarak gibi alternatif yaklaşımlar kullanmanız gerekir. Alternatif yaklaşımlar kullanılabilir değil ve ardından karmaşık parolaları kullandığınızdan emin olun ve varsa, iki öğeli kimlik doğrulama (gibi [Azure çok faktörlü kimlik doğrulaması](../active-directory/authentication/multi-factor-authentication.md)).
- **Güçlü kimlik doğrulama ve yetkilendirme platformları kullanın**

  - Federasyon kimlikleri özel kullanıcı depoları yerine Azure AD kullanın. Federasyon kimlikleri kullandığınızda, platform tabanlı bir yaklaşım yararlanmak ve iş ortaklarınıza yetkili kimliklerinin yönetilmesini atayabilirsiniz. Bir Federasyon kimlik yaklaşım çalışanlar sona erdirilir ve birden çok kimlik ve yetkilendirme sistemler arasında yansıtılması gerek bilgi senaryolarında özellikle önemlidir.
  - Sağlanan platform kimlik doğrulama ve yetkilendirme mekanizmaları yerine özel kod kullanın. Özel kimlik doğrulama kodu geliştirme hataya olabilir nedenidir. Çoğu, geliştiricilerin güvenlik uzmanları değildir ve subtleties ve kimlik doğrulama ve yetkilendirme son gelişmeleri farkında olması olası değildir. Ticari (örneğin Microsoft'tan) genellikle kapsamlı bir şekilde gözden güvenlik kodudur.
  - Çok faktörlü kimlik doğrulaması kullanın. Çok faktörlü kimlik doğrulaması kimlik doğrulama ve yetkilendirme için geçerli standart çünkü kullanıcı adı ve parola kimlik doğrulaması türlerinde devralınan güvenlik zayıf önler. Azure Yönetim (portal/uzak PowerShell) arabirimler ve müşteri hizmetleri Karşılıklı Erişim tasarlanmış ve gerekir kullanmak üzere yapılandırılmış [Azure çok faktörlü kimlik doğrulama (MFA)](../active-directory/authentication/multi-factor-authentication.md).
  - OAuth2 ve Kerberos gibi standart kimlik doğrulama protokolleri kullanır. Bu protokollerin gözden eş yaygın olmuştur ve büyük olasılıkla platform Kitaplıklarınızı kimlik doğrulaması ve yetkilendirme bir parçası olarak uygulanır.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, bir Azure PaaS dağıtımı güvenlik avantajlarından odaklanır. Ardından, PaaS web ve mobil çözümlerinin güvenliğini sağlamaya yönelik önerilen yöntemleri öğrenin. Azure App Service, Azure SQL Database ve Azure SQL Data Warehouse ile başlayacağız. Diğer Azure Hizmetleri için önerilen uygulamaları makalelerini kullanılabilir duruma geldiğinde, bağlantılar aşağıdaki listede sağlanacak:

- [Azure App Service](security-paas-applications-using-app-services.md)
- [Azure SQL Database ve Azure SQL veri ambarı](security-paas-applications-using-sql.md)
- Azure Storage
- Azure REDIS önbelleği
- Azure Service Bus
- Web uygulaması güvenlik duvarı

<!--Image references-->
[1]: ./media/security-paas-deployments/advantages-of-cloud.png
[2]: ./media/security-paas-deployments/responsibility-zones.png
[3]: ./media/security-paas-deployments/advantages-of-paas.png
[4]: ./media/security-paas-deployments/identity-perimeter.png
