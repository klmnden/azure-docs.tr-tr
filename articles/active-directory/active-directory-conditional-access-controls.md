---
title: "Azure Active Directory koşullu erişim denetimleri | Microsoft Docs"
description: "Azure Active Directory koşullu erişim denetimleri nasıl çalıştığını öğrenin."
services: active-directory
keywords: "uygulamaları, Azure AD ile koşullu erişim, koşullu erişim ilkeleri, şirket kaynaklarına güvenli erişim için koşullu erişim"
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/08/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 24db2d29684f7ce5822c77c71f944327476b7196
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="controls-in-azure-active-directory-conditional-access"></a>Azure Active Directory koşullu erişim denetimleri 

İle [Azure Active Directory (Azure AD) koşullu erişim](active-directory-conditional-access-azure-portal.md), bulut uygulamalarınızı nasıl yetkili kullanıcılara erişimi denetleyebilirsiniz. Bir koşullu erişim ilkesi ("bunu") yanıt ("Bu durumda") ilkeniz tetikleme nedenini tanımlayın. 

![Denetim](./media/active-directory-conditional-access-controls/10.png)


Koşullu erişim bağlamında 

- "**Bu durumda**" olarak adlandırılır **koşulları**

- "**Sonra bunu**" olarak adlandırılır **erişim denetimleri**


Bir koşul deyimi, denetimleri ile birlikte bir koşullu erişim ilkesi temsil eder.

![Denetim](./media/active-directory-conditional-access-controls/61.png)

Her denetimidir kişi tarafından yerine getirilmesi gereken bir gereksinim veya sistem oturum açma veya bir kısıtlama hangi kullanıcı oturum açtıktan sonra yapabilirsiniz. 

İki tür denetimi vardır: 

- **Verme denetimlerini** - kapı erişimi

- **Oturum denetimleri** - oturum içinde erişimi kısıtlamak için

Bu konu, Azure AD koşullu erişim kullanılabilen çeşitli denetimleri açıklar. 

## <a name="grant-controls"></a>İzin verme denetimleri

GRANT denetimleriyle için tamamen engelleyin veya istenen denetimleri seçerek ek gereksinimler erişimle izin verebilirsiniz. Birden çok denetim için gerektirebilir:

- Tüm seçilen yerine getirilmesi için denetimleri (*ve*) 
- Bir seçili yerine getirilmesi için denetimi (*veya*)

![Denetim](./media/active-directory-conditional-access-controls/17.png)



### <a name="multi-factor-authentication"></a>Multi-factor authentication

Belirtilen bulut uygulamasında erişmek için çok faktörlü kimlik doğrulaması gerektirmek için bu denetim kullanabilirsiniz. Bu denetim aşağıdaki çok faktörlü sağlayıcılarını destekler: 

- Azure Multi-Factor Authentication 

- Active Directory Federasyon Hizmetleri (AD FS) ile birlikte bir şirket içi çok faktörlü kimlik doğrulama sağlayıcısı.
 
Çok faktörlü kimlik doğrulaması kullanarak geçerli bir kullanıcı birincil kimlik bilgilerini erişim elde etmiştir yetkisiz bir kullanıcı tarafından erişilen kaynaklar korunmasına yardımcı olur.



### <a name="compliant-device"></a>Uyumlu cihaz

Cihaz temelli koşullu erişim ilkeleri yapılandırabilirsiniz. Amacı bir cihaz temelli koşullu erişim ilkesi, yalnızca güvenilen cihazlardan yapılandırılmış kaynaklarına erişim sağlamaktır. Uyumlu bir cihaz gerektirmektir sahip tanımlamak için bir seçenek güvenilir cihazlar olduğunu. Daha fazla bilgi için bkz: [Azure Active Directory cihaz temelli koşullu erişim ilkeleri Ayarla](active-directory-conditional-access-policy-connected-applications.md).

### <a name="domain-joined-device"></a>Cihaz etki alanına katılmış

Etki alanına katılmış bir cihaz başka bir seçenektir gerektiren cihaz temelli koşullu erişim ilkelerini yapılandırmanız gerekir. Bu gereksinim Windows Masaüstü ve dizüstü bilgisayarlar ile bir şirket içi Active Directory'e katılmayan Kurumsal tabletlerde ifade eder. Daha fazla bilgi için bkz: [Azure Active Directory cihaz temelli koşullu erişim ilkeleri Ayarla](active-directory-conditional-access-policy-connected-applications.md).





### <a name="approved-client-app"></a>Onaylanmış istemci uygulaması

Çalışanlarınız hem kişisel mobil cihazlarda ve çalışma görevlerini kullandığından, hatta nerede bunlar sizin tarafınızdan yönetilmeyen durumda cihazları kullanarak erişilen şirket verilerini koruma özelliği sahip olmak isteyebilirsiniz.
Kullanabileceğiniz [Intune uygulama koruma ilkeleri](https://docs.microsoft.com/intune/app-protection-policy) tüm mobil cihaz Yönetimi (MDM) çözümünden bağımsız Şirketinizin verilerini korumaya yardımcı olmak için.


Onaylanmış istemci uygulamaları ile desteklemek için bulut uygulamalarınızı erişmeye çalıştığı bir istemci uygulaması gerektirebilir [Intune uygulama koruma ilkeleri](https://docs.microsoft.com/intune/app-protection-policy). Örneğin, erişimi Exchange Online için Outlook uygulaması kısıtlayabilirsiniz. Onaylanmış istemci uygulamaları gerektiren bir koşullu erişim ilkesi de denir [uygulama bağlı olarak koşullu erişim ilkesi](active-directory-conditional-access-mam.md). Desteklenen onaylanmış istemci uygulamaları listesi için bkz: [istemci uygulama gereksinimi onaylanan](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement).


### <a name="terms-of-use"></a>Kullanım Koşulları

Bir kullanıcı bir kaynağa erişim izni verilmeden önce kullanım koşullarını kabul kiracınızda gerektirebilir. Yönetici olarak, yapılandırma ve kullanım koşullarını PDF belgesini yükleyerek özelleştirin. Bir kullanıcı kalırsa uygulamanın bu denetim erişim kapsamını yalnızca kullanım koşullarını kabul verilir. 


### <a name="custom-controls"></a>Özel denetimler 

Özel denetimler, kullanıcılarınızın daha fazla Azure Active Directory dışında gereksinimlerini karşılamak için uyumlu bir hizmete yönlendirmek koşullu erişim oluşturabilirsiniz. Bu koşullu erişim kurallarını zorunlu tutmak için ya da kendi özel hizmet oluşturmak için belirli dış çok faktörlü kimlik doğrulama ve doğrulama sağlayıcıları kullanmanıza olanak sağlar. Bir kullanıcının tarayıcısının dış hizmete yönlendirilir bu denetimi gerçekleştirmek için gerekli tüm kimlik doğrulama veya doğrulama etkinlikler gerçekleştirir ve sonra Azure Active Directory'ye yönlendirilir. Kullanıcı başarıyla kimlik doğrulaması veya doğrulanan kullanıcı koşullu erişim akışı devam eder. 

## <a name="custom-controls"></a>Özel denetimler

Azure Active Directory Premium P2 edition yeteneğini özel denetimleridir. Özel denetimler kullanırken, kullanıcılarınızın daha fazla Azure Active Directory dışında gereksinimlerini karşılamak için uyumlu bir hizmete yönlendirilir. Bir kullanıcının tarayıcısının dış hizmete yönlendirilir bu denetimi gerçekleştirmek için gerekli tüm kimlik doğrulama veya doğrulama etkinlikler gerçekleştirir ve sonra Azure Active Directory'ye yönlendirilir. Azure Active Directory yanıt doğrular ve kullanıcı başarıyla kimlik doğrulaması veya doğrulanan kullanıcı koşullu erişim akışı devam eder.

Bu denetimler belirli dış veya özel Hizmetleri koşullu erişim denetimleri olarak kullanılmasına izin verin ve genellikle koşullu erişim yeteneklerini.

Şu anda uyumlu bir hizmet sunumu sağlayıcıları içerir:

- [Duo güvenlik](https://duo.com/docs/azure-ca)

- RSA

- [Trusona](https://www.trusona.com/docs/azure-ad-integration-guide)

Bu hizmetler hakkında daha fazla bilgi için sağlayıcı doğrudan başvurun.

### <a name="creating-custom-controls"></a>Özel denetimler oluşturma

Özel bir denetim oluşturmak için önce kullanmak istediğiniz sağlayıcı başvurmanız gerekir. Her Microsoft dışı sağlayıcı kendi işlem ve kaydolma, abone veya aksi halde hizmetin bir parçası haline gelir ve koşullu erişim ile tümleştirmek istediğiniz belirtmek için gereksinimleri vardır. Bu noktada, sağlayıcı bir JSON biçiminde veri bloğu ile sağlayacaktır. Bu veri sağlayıcı ve koşullu erişim kiracınız için birlikte çalışan izin verir, yeni denetim oluşturur ve koşullu erişim tanımlayan kullanıcılarınızın doğrulama sağlayıcısı ile başarıyla gerçekleştirmiş olmadığını söyleyebilir.

JSON verilerini kopyalayın ve ardından ilgili metin kutusuna yapıştırın. Değişiklik yapıyorsunuz değişiklik açıkça anlamadan JSON herhangi bir değişiklik yapmayın. Herhangi bir değişiklik yapmadan, sağlayıcı ve Microsoft arasında bağlantıyı kesmek ve potansiyel olarak siz ve kullanıcılarınız hesaplarınızı dışında kilitleyin.

Özel bir denetim oluşturmak için seçeneği olarak **Yönet** bölümünü **koşullu erişim** sayfası.

![Denetim](./media/active-directory-conditional-access-controls/82.png)

Tıklatarak **yeni bir özel denetim**, denetiminizin JSON verilerini bir metin kutusu ile bir dikey pencere açılır.  


![Denetim](./media/active-directory-conditional-access-controls/81.png)


### <a name="deleting-custom-controls"></a>Özel denetimler silme

Özel bir denetim silmek için önce herhangi bir koşullu erişim ilkesinin kullanılmadığından emin olmalısınız. Tamamlandıktan sonra:

1. Özel denetimler listesine Git

2. Tıklatın...  

3. **Sil**’i seçin.

### <a name="editing-custom-controls"></a>Özel denetimleri düzenleme

Özel bir denetim düzenlemek için geçerli denetim silin ve güncelleştirilmiş bilgileri ile yeni bir denetim oluşturun.




## <a name="session-controls"></a>Oturum denetimleri

Oturum denetimleri, bulut uygulaması içinde sınırlı deneyim sağlar. Oturum denetimleri bulut uygulamaları tarafından zorunlu tutulmaz ve oturumla ilgili uygulama için Azure AD tarafından sağlanan ek bilgileri kullanır.

![Denetim](./media/active-directory-conditional-access-controls/31.png)

### <a name="use-app-enforced-restrictions"></a>Uygulama tarafından zorlanan kısıtlamaları kullan

Bu denetim, aygıt bilgisi bulut uygulamasına geçirmek Azure AD gerektirecek şekilde kullanabilirsiniz. Bu kullanıcı bir uyumlu aygıt veya etki alanına katılmış CİHAZDAN geliyorsa bilmeniz bulut uygulaması yardımcı olur. Bu denetimidir şu anda yalnızca bulut uygulaması olarak SharePoint ile desteklenir. SharePoint aygıt bilgileri kullanıcılara cihaz durumuna bağlı olarak bir tam veya sınırlı deneyimi sağlamak için kullanır.
SharePoint ile sınırlı erişim gerektiren hakkında daha fazla bilgi için bkz: [kontrol yönetilmeyen cihazların erişimini](https://aka.ms/spolimitedaccessdocs).



## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim ilkesini yapılandırma hakkında bilmek istiyorsanız [Azure Active Directory'de koşullu erişimi kullanmaya başlama](active-directory-conditional-access-azure-portal-get-started.md).

- Ortamınız için koşullu erişim ilkelerini yapılandırma için hazır olup olmadığını görmek [Azure Active Directory'de koşullu erişim için en iyi uygulamaları](active-directory-conditional-access-best-practices.md). 
