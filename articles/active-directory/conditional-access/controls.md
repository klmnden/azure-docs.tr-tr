---
title: Azure Active Directory koşullu erişim erişim denetimleri nelerdir? | Microsoft Docs
description: Erişimi Azure Active Directory koşullu erişim işlerinde nasıl denetimleri hakkında bilgi edinin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 06/15/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: a5fc672898a56d8b3e1486b1d8d84cf532fa2b6d
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509397"
---
# <a name="what-are-access-controls-in-azure-active-directory-conditional-access"></a>Azure Active Directory koşullu erişim erişim denetimleri nelerdir?

İle [Azure Active Directory (Azure AD) koşullu erişim](../active-directory-conditional-access-azure-portal.md), bulut uygulamalarınızı nasıl yetkili kullanıcılara erişimi denetleyebilirsiniz. Bir koşullu erişim ilkesinde ("Bu durumda") ilkeniz tetikleme nedeni için ("bunu") yanıtı tanımlayın.

![Denetim](./media/controls/10.png)

Koşullu erişim bağlamında

- "**Bu durumda**" olarak adlandırılır **koşulları**
- "**Bunu yapmak**" olarak adlandırılır **erişim denetimleri**

Bir koşul deyimi, denetimleri ile koşullu erişim ilkesi temsil eder.

![Denetim](./media/controls/61.png)

Kişi tarafından yerine getirilmesi gereken bir gereksinim ya da sistem oturum açarken her denetimidir ve hangi kullanıcının bir kısıtlama oturum açtıktan sonra yapabilirsiniz.

Denetimlerin iki tür vardır:

- **İzin verme denetimleri** - kapı erişimi
- **Oturum denetimleri** - oturum içindeki erişimi kısıtlamak için

Bu konuda, Azure AD koşullu erişim kullanılabilen çeşitli denetimleri açıklanmaktadır. 

## <a name="grant-controls"></a>İzin verme denetimleri

Verme denetimleri sayesinde, erişimi tamamen engelleme veya istenen denetimleri seçerek ek gereksinimler ile erişim izni. Birden fazla denetim için gerektirebilir:

- Tüm seçilen yerine getirilmesi için denetimleri (*ve*)
- Seçili tek getirilmesi için denetimi (*veya*)

![Denetim](./media/controls/18.png)

### <a name="multi-factor-authentication"></a>Multi-factor authentication

Bu denetim, belirtilen bulut uygulamasına erişmek için çok faktörlü kimlik doğrulama isteyecek şekilde kullanabilirsiniz. Bu denetimi aşağıdaki multi-Factor Authentication sağlayıcılarını destekler:

- Azure Multi-Factor Authentication
- Active Directory Federasyon Hizmetleri (AD FS) ile birlikte bir şirket içi multi-Factor authentication sağlayıcısı.

Multi-Factor authentication kullanarak, geçerli kullanıcının birincil kimlik bilgilerini erişim elde etmiştir yetkisiz bir kullanıcı tarafından erişilen kaynaklar korunmasına yardımcı olur.

### <a name="compliant-device"></a>Uyumlu cihaz

Cihaz tabanlı koşullu erişim ilkelerini yapılandırabilirsiniz. Seçilen bulut uygulamalarınızdan yalnızca erişim vermek için cihaz tabanlı koşullu erişim ilkesi amacı olan [yönetilen cihazlar](require-managed-devices.md). Bir cihazın uyumlu olarak işaretlenmesini gerektiren yönetilen cihazlara erişimi sınırlamak için sahip olduğunuz bir seçenektir. Bir cihaz uyumlu olarak (herhangi bir cihaz için işletim sistemi) Intune veya üçüncü taraf MDM sisteminiz Windows 10 cihazları için işaretlenebilir. Windows 10 dışında cihaz işletim sistemi türleri için üçüncü taraf MDM sistemleri desteklenmez. 

Cihazınızın uyumlu olarak işaretlenebilir önce Azure AD'ye kayıtlı olması gerekir. Bir cihazı kaydetmek için üç seçeneğiniz vardır: 

- Azure AD kayıtlı cihazlar
- Azure AD’ye katılmış cihazlar  
- Hibrit Azure AD’ye katılmış cihazlar

Bu üç seçenek makalesinde açıklanan [bir cihaz Kimliği nedir?](../devices/overview.md)

Daha fazla bilgi için [yönetilen cihazlar için koşullu erişim ile bulut uygulaması erişimi zorunlu kılma](require-managed-devices.md).

### <a name="hybrid-azure-ad-joined-device"></a>Hibrit Azure AD'ye katılmış

Karma Azure AD'ye katılmış cihaz tabanlı koşullu erişim ilkeleri yapılandırmak için sahip olduğunuz başka bir seçenek olan gerek. Bu gereksinim, Windows Masaüstü, dizüstü bilgisayarlar ve bir şirket içi Active Directory'ye katılmış Kurumsal tabletler ifade eder. Bu seçenek belirlenirse, koşullu erişim ilkenizi, şirket içi Active Directory'niz ve Azure Active Directory'nize katılmış cihazları yapılan erişimi için erişim girişimlerini verir.  

Daha fazla bilgi için [Azure Active Directory cihaz tabanlı koşullu erişim ilkeleri ayarlama](require-managed-devices.md).

### <a name="approved-client-app"></a>Onaylı istemci uygulaması

Çalışanlarınız hem kişisel mobil cihazları ve çalışma görevlerini kullandığından, durumda bile nerede bunlar sizin tarafınızdan yönetilmeyen cihazları kullanarak erişilen şirket verilerini koruma özelliğine sahip olmak isteyebilirsiniz.
Kullanabileceğiniz [Intune uygulama koruma ilkeleri](https://docs.microsoft.com/intune/app-protection-policy) tüm mobil cihaz Yönetimi (MDM) çözümünden bağımsız Şirketinizin verilerini korumaya yardımcı olmak için.

Onaylı istemci uygulamaları ile bulut uygulamalarınızı destekleyecek şekilde erişmeyi denediği bir istemci uygulaması gerektirebilir [Intune uygulama koruma ilkeleri](https://docs.microsoft.com/intune/app-protection-policy). Örneğin, erişimi Exchange Online için Outlook uygulamasında kısıtlayabilirsiniz. Onaylı istemci uygulamalarını gerektiren bir koşullu erişim ilkesi de denir [uygulama tabanlı koşullu erişim ilkesi](app-based-conditional-access.md). Desteklenen onaylı istemci uygulamalarının listesi için bkz. [onaylı istemci uygulaması gereksinimi](technical-reference.md#approved-client-app-requirement).

### <a name="app-protection-policy-preview"></a>Uygulama koruma İlkesi (Önizleme)

Çalışanlarınız hem kişisel mobil cihazları ve çalışma görevlerini kullandığından, durumda bile nerede bunlar sizin tarafınızdan yönetilmeyen cihazları kullanarak erişilen şirket verilerini koruma özelliğine sahip olmak isteyebilirsiniz.
Kullanabileceğiniz [Intune uygulama koruma ilkeleri](https://docs.microsoft.com/intune/app-protection-policy) tüm mobil cihaz Yönetimi (MDM) çözümünden bağımsız Şirketinizin verilerini korumaya yardımcı olmak için.

Uygulama koruma İlkesi ile Azure'a bildirilen istemci uygulamaları için erişimi sınırlayabilirsiniz AD sahip alınan [Intune uygulama koruma ilkeleri](https://docs.microsoft.com/intune/app-protection-policy). Örneğin, erişimi Exchange Online için Intune uygulama koruma ilkesi olan Outlook uygulamasında kısıtlayabilirsiniz. Uygulama koruma İlkesi gerektiren bir koşullu erişim ilkesi de denir [uygulama koruma tabanlı koşullu erişim ilkesi](app-protection-based-conditional-access.md). 

Cihazınızı bir uygulama ilkesi korumalı işaretlenebilir önce Azure AD'ye kayıtlı olması gerekir.

Korumalı istemci uygulamaları, desteklenen ilke listesi için bkz. [uygulama koruma İlkesi gereksinimi](technical-reference.md#app-protection-policy-requirement).

### <a name="terms-of-use"></a>Kullanım koşulları

Bir kullanıcı bir kaynağa erişim izni verilmeden önce kullanım koşullarını kabul kiracınızdaki gerektirebilir. Yönetici olarak, yapılandırma ve kullanım koşullarını PDF'yi karşıya yükleyerek özelleştirin. Bir kullanıcı kalırsa bu denetimi bir uygulamaya erişim kapsamını yalnızca kullanım koşullarını kabul verilir.

## <a name="custom-controls-preview"></a>Özel denetimler (Önizleme)

Özel denetimler, Azure Active Directory Premium P1 edition özellikleridir. Özel denetimleri kullanarak, kullanıcılarınızın daha fazla Azure Active Directory dışında gereksinimlerini karşılamak için uyumlu bir hizmete yönlendirilir. Bu denetimi gerçekleştirmek için bir kullanıcının tarayıcı dış hizmete yönlendirilir, gerekli herhangi bir kimlik doğrulaması veya doğrulama etkinliklerini gerçekleştirir ve ardından Azure Active Directory'ye yönlendirilir. Azure Active Directory yanıt doğrular ve kullanıcı başarıyla kimlik doğrulaması veya doğrulanan kullanıcı koşullu erişim akışı devam eder.

Bu denetimler belirli dış veya özel hizmetler koşullu erişim denetimleri olarak kullanılmasına izin verin ve genellikle koşullu erişim özelliklerini genişleten.

Şu anda uyumlu bir hizmet teklifi sağlayıcıları içerir:

- [Duo güvenlik](https://duo.com/docs/azure-ca)
- [Entrust Datacard](https://www.entrustdatacard.com/products/authentication/intellitrust)
- [GSMA](https://mobileconnect.io/azure/)
- [Ping Identity](https://documentation.pingidentity.com/pingid/pingidAdminGuide/index.shtml#pid_c_AzureADIntegration.html)
- RSA
- [SecureAuth](https://docs.secureauth.com/pages/viewpage.action?pageId=47238992#)
- [Silverfort](https://www.silverfort.io/company/using-silverfort-mfa-with-azure-active-directory/)
- [Symantec VIP](https://help.symantec.com/home/VIP_Integrate_with_Azure_AD)
- [Thales (Gemalto)](https://resources.eu.safenetid.com/help/AzureMFA/Azure_Help/Index.htm)
- [Trusona](https://www.trusona.com/docs/azure-ad-integration-guide)

Bu hizmetler hakkında daha fazla bilgi için sağlayıcı doğrudan başvurun.

### <a name="creating-custom-controls"></a>Özel denetimler oluşturma

Özel bir denetim oluşturmak için kullanmak istediğiniz sağlayıcıyı başvurmalısınız. Her bir Microsoft dışı sağlayıcısı, kendi işlem ve kaydolun, abone olmanızı veya aksi halde hizmetin bir parçası haline gelir ve koşullu erişim ile tümleştirmek istediğiniz belirtmek için gereksinimleri vardır. Bu noktada, sağlayıcı, JSON biçiminde bir veri bloğunu sağlayacak. Bu veri sağlayıcısı ve koşullu erişim, kiracınız için birlikte çalışmasına olanak sağlar, yeni denetimi oluşturur ve koşullu erişimi nasıl anlayabilirim tanımlar, kullanıcılarınızın doğrulama sağlayıcısı ile başarıyla gerçekleştirdiyseniz.

Özel denetimler, çok faktörlü kimlik doğrulaması gerektiren kimlik Koruması'nın Otomasyonu ile ya da rolleri Privileged Identity Manager (PIM) olarak yükseltmek için kullanılamaz.

JSON verilerini kopyalayın ve ardından ilgili metin kutusuna yapıştırın. Açıkça hale getirildiği değişiklik anlamadan JSON herhangi bir değişiklik yapmayın. Herhangi bir değişiklik yapmadan, sağlayıcısı ile Microsoft arasındaki bağlantıyı kesin ve potansiyel olarak siz ve kullanıcılarınız hesaplarınızı dışında kilitleyin.

Özel bir denetim oluşturmak için seçeneği **Yönet** bölümünü **koşullu erişim** sayfası.

![Denetim](./media/controls/82.png)

Tıklayarak **yeni özel denetim**, bir metin kutusu denetiminizin JSON verilerini içeren bir dikey pencere açılır.  

![Denetim](./media/controls/81.png)

### <a name="deleting-custom-controls"></a>Özel denetim siliniyor

Özel denetim silmek için önce tüm koşullu erişim ilkesinde kullanılmadığından emin olmalısınız. Bir kez tamamlayın:

1. Özel denetimleri listesine Git
1. Tıklayın...  
1. **Sil**’i seçin.

### <a name="editing-custom-controls"></a>Özel denetimleri düzenleme

Özel denetim düzenlemek için geçerli denetim silin ve güncelleştirilmiş bilgileri ile yeni bir denetim oluşturmak gerekir.

## <a name="session-controls"></a>Oturum denetimleri

Oturum denetimleri, bulut uygulaması içinde sınırlı deneyim sağlar. Oturum denetimleri, bulut uygulamaları tarafından zorunlu tutulmaz ve oturumla ilgili uygulamayı Azure AD'ye tarafından sağlanan ek bilgiler dayanır.

![Denetim](./media/controls/31.png)

### <a name="use-app-enforced-restrictions"></a>Uygulama tarafından zorlanan kısıtlamaları kullan

Bu denetim, seçili bulut uygulamaları için cihaz bilgilerini geçirmek Azure AD zorunlu tutmak için kullanabilirsiniz. Cihaz bilgilerini bir bağlantı uyumlu veya etki alanına katılmış bir CİHAZDAN başlatılır bilmeniz bulut uygulamaları sağlar. Bu denetim yalnızca SharePoint Online ve Exchange Online seçilen bulut uygulamaları destekler. Bulut uygulaması seçili olduğunda, aygıt bilgisi sınırlı veya tam bir deneyim ile cihaz durumuna bağlı olarak kullanıcılara sağlamanız kullanır.

Daha fazla bilgi için bkz:

- [SharePoint Online ile sınırlı erişimini etkinleştirme](https://aka.ms/spolimitedaccessdocs)
- [Exchange Online ile sınırlı erişimini etkinleştirme](https://aka.ms/owalimitedaccess)

## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim ilkesi yapılandırmak için bkz. nasıl bilmek istiyorsanız [gerektiren MFA belirli uygulamalar için Azure Active Directory koşullu erişim ile](app-based-mfa.md).
- Ortamınız için koşullu erişim ilkelerini yapılandırmaya hazırsanız bkz [Azure Active Directory'de koşullu erişim için en iyi uygulamalar](best-practices.md).
