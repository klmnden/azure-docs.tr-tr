---
title: PaaS dağıtımlarının güvenliğini sağlama | Microsoft Docs
description: " Paas'nin avantajlarını güvenlik ve diğer bulut hizmeti modeli ve Azure PaaS dağıtımınızın güvenliğini sağlamaya yönelik önerilen adımları öğrenin anlayın. "
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
ms.openlocfilehash: da5d59aaaea8e6186609eb5f3419fba5e67d4279
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2018
ms.locfileid: "42056554"
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

## <a name="identity-as-the-primary-security-perimeter"></a>Birincil güvenlik çevresi olarak kimlik
Bulut ağ merkezli daha az ilgili düşünmek getiren geniş ağ erişimi olan beş temel özellik biri. Amacı, kullanıcıların kaynakları konumdan bağımsız olarak erişmesine izin vermek için bulut çoğunu. Çoğu kullanıcı için konumlarına yere İnternette olacağı.

Aşağıdaki şekil, güvenlik çevresi bir kimlik çevre için bir ağ çevresi nasıl geliştirildiğini gösterir. Güvenlik savunma ağınız hakkında daha az ve daha fazlası hakkında verilerinizi savunma yanı uygulamaları ve kullanıcılarınızın güvenliğini yönetme olur. Şirketiniz için önemli olan için güvenlik yakın göndermek istediğiniz anahtar farktır.

![Yeni güvenlik çevresi olarak kimlik][4]

Başlangıçta, Azure PaaS hizmetlerine (örneğin, web rolleri ve Azure SQL) çok az kayıpla veya hiç geleneksel ağ çevre savunmaları sağladı. Internet'e (web rolü) sağlamak öğenin amaçlı olan ve yeni bir çevre (örneğin, BLOB veya Azure SQL) söz konusu kimlik doğrulamasını sağlar algılandı.

Modern güvenlik uygulamaları, saldırganın ağ çevresi aşmış varsayılır. Bu nedenle, savunma modern uygulamalar için kimlik taşıdık. Kuruluşlar, bir kimlik tabanlı güvenlik çevresi güçlü kimlik doğrulaması ve yetkilendirme sağlığı (iyi) ile oluşturmanız gerekir.

## <a name="recommendations-for-managing-the-identity-perimeter"></a>Kimlik çevre yönetme ile ilgili öneriler

İlkeleri ve modeller için ağ çevre yıllardır kullanılabilir olmuştur. Buna karşılık, sektör kimliği birincil güvenlik çevresi olarak kullanmaya ile görece küçük bir deneyimi vardır. Bu kullanıcıyla Bununla birlikte, alanında kanıtlanmış ve neredeyse tüm PaaS Hizmetleri için geçerli olan bazı genel öneriler sağlamak için yeterli deneyimi birikti.

Aşağıda, kimlik çevre yönetmek için genel bir en iyi uygulama yaklaşımı özetlenmektedir.

- **Anahtarları veya kimlik bilgilerini kaybetmeyin** anahtarlarını ve kimlik bilgilerini güvenli hale getirme PaaS dağıtımlarının güvenliğini sağlamak için önemli. Anahtarlar ve kimlik bilgileri kaybetme sık karşılaşılan bir sorundur. İyi bir çözümdür, anahtarları ve gizli dizileri donanım güvenlik modülleri (HSM) burada depolanabilir merkezi bir çözüm kullanmaktır. Azure ile bulutta HSM sağlar [Azure anahtar kasası](../key-vault/key-vault-whatis.md).
- **Kimlik bilgileri ve diğer gizli dizileri kaynak kodu veya GitHub koymayın** gereken tek şey anahtarlarını ve kimlik bilgilerini kaybetme yetkisiz bir tarafın sahip daha da kötüsü onlara yönelik erişimi elde edin. Saldırganlar teknolojileri anahtarları ve GitHub gibi kod depoları içinde depolanan gizli bulmak için robot yararlanmak olanağına sahip olursunuz. Bu ortak kaynak kodu depoları anahtar ve gizli anahtarları koymayın.
- **Karma PaaS ve Iaas Hizmetleri, sanal makine yönetim arabirimlerindeki korumak** Iaas ve PaaS Hizmetleri, sanal makineler (VM) çalıştırın. Hizmet türüne bağlı olarak birkaç yönetim arabirimleri kullanılabilir uzak yönettiğiniz bu VM'lerin doğrudan bu etkinleştir. Uzaktan Yönetim protokolleri gibi [Secure Shell Protokolü (SSH)](https://en.wikipedia.org/wiki/Secure_Shell), [Uzak Masaüstü Protokolü (RDP)](https://support.microsoft.com/kb/186607), ve [uzak PowerShell](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/enable-psremoting) kullanılabilir. Genel olarak, doğrudan uzak erişim VM'ler için Internet'ten etkinleştirmemeniz önerilir. Varsa, Azure sanal ağına sanal özel ağ kullanarak gibi alternatif bir yaklaşım kullanmanız gerekir. Alternatif yaklaşımlar kullanılabilir değil ve ardından karmaşık parolaları kullandığınızdan emin olun ve kullanılabilir olduğunda, iki öğeli kimlik doğrulama (gibi [Azure multi-Factor Authentication](../active-directory/authentication/multi-factor-authentication.md)).
- **Güçlü kimlik doğrulaması ve yetkilendirme platformları kullanın**

  - Özel kullanıcı depoları yerine Azure AD'de Federasyon kimlikleri kullanın. Federasyon kimlikleri kullandığınızda, platform temelli bir yaklaşım yararlanın ve yetkili kimlik yönetimini, iş ortakları için temsilci. Federe kimlik yaklaşım çalışanlar sonlandırılır ve birden çok kimlik doğrulaması ve yetkilendirme sistemler arasında yansıtılması bilgi gereken senaryolarda özellikle önemlidir.
  - Platform tarafından sağlanan kimlik doğrulama ve yetkilendirme mekanizmaları yerine özel kod kullanın. Özel kimlik doğrulama kodu geliştirme hatalar oluşabilir nedenidir. Geliştiricilerinizin çoğu güvenlik konusunda uzman olmayan ve ıot'nin ve kimlik doğrulama ve yetkilendirme son gelişmeleri dikkat edilmesi gereken düşüktür. Ticari koddan (örneğin Microsoft), genellikle kapsamlı bir şekilde gözden güvenlik sağlıyor.
  - Çok faktörlü kimlik doğrulaması kullanın. Kullanıcı adı ve parola kimlik doğrulaması türlerinde devralınan güvenlik zayıf önlediği için çok faktörlü kimlik doğrulaması kimlik doğrulaması ve yetkilendirme geçerli standardıdır. Azure Yönetim (portal/uzak PowerShell) arabirimler ve Müşteri Hizmetleri'e yönelik erişim tasarlanmış verilecek ve kullanmak üzere yapılandırılmış [Azure multi-Factor Authentication (MFA)](../active-directory/authentication/multi-factor-authentication.md).
  - OAuth2 ve Kerberos gibi standart kimlik doğrulama protokolleri kullanır. Bu protokollerin kapsamlı bir şekilde gözden eş atanmış olması ve büyük olasılıkla kimlik doğrulaması ve yetkilendirme platformu Kitaplıklarınızı bir parçası olarak uygulanır.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, sizi Azure PaaS dağıtımı güvenlik avantajlarından odaklanır. Ardından, PaaS web ve mobil çözümler güvenliğini sağlamaya yönelik önerilen yöntemler öğrenin. Azure App Service, Azure SQL veritabanı ve Azure SQL veri ambarı ile başlayacağız. Diğer Azure Hizmetleri için önerilen uygulamaların makalelerde kullanılabilir oldukça aşağıdaki listede bağlantıları sağlanır:

- [Azure App Service](security-paas-applications-using-app-services.md)
- [Azure SQL veritabanı ve Azure SQL veri ambarı](security-paas-applications-using-sql.md)
- [Azure Depolama](security-paas-applications-using-storage.md)
- Azure REDIS önbelleği
- Azure Service Bus
- Web uygulaması güvenlik duvarları

<!--Image references-->
[1]: ./media/security-paas-deployments/advantages-of-cloud.png
[2]: ./media/security-paas-deployments/responsibility-zones.png
[3]: ./media/security-paas-deployments/advantages-of-paas.png
[4]: ./media/security-paas-deployments/identity-perimeter.png
