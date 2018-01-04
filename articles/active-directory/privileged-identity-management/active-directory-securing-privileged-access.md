---
title: "Azure AD'de ayrıcalıklı erişimi güvenli hale getirme | Microsoft Docs"
description: "Azure, Azure Active Directory ve Microsoft Online Services ayrıcalıklı erişim güvenliği yaklaşımları açıklayan bir konu."
services: active-directory
documentationcenter: 
author: barclayn
manager: mtillman
editor: mwahl
ms.assetid: 235a0ce9-1daf-4433-8f65-9c6afcd64d08
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2017
ms.author: kgremban
ms.custom: pim
ms.openlocfilehash: e1bc0f27b14beef91b4deb68dc625d75195445fb
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="securing-privileged-access-in-azure-ad"></a>Azure AD'de ayrıcalıklı erişimi güvenli hale getirme
Ayrıcalıklı erişimi güvenli hale getirme iş varlıklar modern bir kuruluşta korunmasına yardımcı olmak için bir kritik ilk adımdır. Ayrıcalıklı hesapları yönetmek ve BT sistemleri yöneten hesaplarıdır. Siber saldırganların bir kuruluşun veriler ve sistemlerle erişmek için bu hesaplar hedef. Ayrıcalıklı erişim güvenliğini sağlamak için hesapları ve kötü niyetli bir kullanıcı için maruz kalma riskini sistemlerden yalıtmak.

Bulut Hizmetleri üzerinden ayrıcalıklı erişim kazanmak daha fazla kullanıcı başlıyor. Bu, Office365, genel Yöneticiler, Azure aboneliği yöneticileri ve VM'ler veya SaaS uygulamalarını yönetimsel erişime sahip kullanıcılar içerebilir.

Microsoft önerir bu yol haritası izlediğiniz [ayrıcalıklı erişimi güvenli hale getirme](https://technet.microsoft.com/library/mt631194.aspx).

Kullanıcı hesapları yönetilir ve Azure Active Directory veya Active Directory tarafından kimliği doğrulanmış olup olmadığını Azure Active Directory, Office 365 veya diğer Microsoft Hizmetleri ve uygulamaları kullanan müşteriler için bu kurallar uygulanır. Aşağıdaki bölümler, ayrıcalıklı erişim güvenliği desteklemek için Azure AD özelliklerini daha fazla bilgi sağlar.

## <a name="azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication
Yönetici kimlik doğrulaması güvenliğini artırmak için iki aşamalı doğrulamayı ayrıcalıklarını verme önce istemeniz gerekir. İki aşamalı doğrulama, olan doğrulama yöntemi daha fazlasını bir kullanıcı adı ve parola kullanımını gerektirmesidir. İkinci bir kullanıcı oturum açmaları ve işlemleri için güvenlik katmanı sağlar.

Azure multi-Factor Authentication (MFA) hangi yardımcı koruma veri ve uygulamalarınıza erişmek için kullanıcının toplantı sırasında isteğe bağlı bir basit oturum açma işlemi için Microsoft'un iki aşamalı doğrulama, bir çözümdür. Kolay doğrulama seçenekleri de dahil olmak üzere çeşitli aracılığıyla güçlü kimlik doğrulaması sunar:

- telefon aramaları
- Metin iletileri
- Mobil uygulama bildirimleri
- Mobil uygulama doğrulama kodları
- Üçüncü taraf OATH belirteçleri

Azure multi-Factor Authentication nasıl çalıştığını genel bir bakış için aşağıdaki videoya bakın:

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]

Daha fazla bilgi için bkz: [MFA Office 365 ve Azure MFA için](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/).

## <a name="time-bound-privileges"></a>Zaman sınırlı ayrıcalıkları
Bazı kuruluşlar, çok sayıda kullanıcı yüksek ayrıcalıklı rolleri olması fark edebilirsiniz. Bir kullanıcı belirli bir etkinliği için rolüne gibi bir hizmet için kaydolmak için eklenmiş olabilir, ancak o ayrıcalıkları sık daha sonra kullanmadı.

Ayrıcalıkları Etkilenme süresini azaltmak ve bunların kullanılması, görünürlük artırmak için yalnızca "tam zamanında" ayrıcalıklarını üzerinde alan kullanıcıların sınırla (JIT) veya bu rolleri kısaltılmış bir süre için ayrıcalıkları iptal edilecek otomatik olarak güvenle atayın. Azure Active Directory, Azure kaynakları (Önizleme) ve Microsoft Online Services için kullanabileceğiniz [Azure AD Privileged Identity Management (PIM)](http://aka.ms/AzurePIM).

![PIM Panosu][2]

## <a name="attack-detection"></a>Saldırı algılama
[Azure Active Directory kimlik koruması](../active-directory-identityprotection.md) risk olaylarına ve olası güvenlik açıklarını kuruluşunuzdaki kimlikleri etkileyen birleştirilmiş bir görünüm sağlar. Risk olaylara göre risk tabanlı ilkeleri, kuruluşunuzun kimlikleri otomatik olarak korunacak yapılandırmanızı sağlayacak şekilde her bir kullanıcı için bir kullanıcı risk düzeyi kimlik koruması hesaplar. Azure Active Directory ve EMS tarafından sağlanan diğer koşullu erişim denetimlerle birlikte bu ilkeler, otomatik olarak kullanıcı engelleme veya parola sıfırlama ve çok faktörlü kimlik doğrulaması zorlama dahil öneriler sunar.

![Azure AD Kimlik Koruması][3]

## <a name="conditional-access"></a>Koşullu erişim
Koşullu erişim denetimi ile bir kullanıcı bir uygulamaya erişimine izin vermeden önce kimlik doğrulaması sırasında seçtiğiniz belirli koşullar Azure Active Directory denetler. Bu koşullar sağlandığında, kullanıcı kimlik doğrulaması ve uygulamaya erişim izni.

## <a name="related-articles"></a>İlgili makaleler
* Etkinleştirme [Azure çok faktörlü kimlik doğrulaması](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)
* Etkinleştirme [Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-configure.md)
* Etkinleştirme [Azure AD kimlik koruması](../active-directory-identityprotection.md)
* Etkinleştirme [koşullu erişim denetimleri](../active-directory-conditional-access-azure-portal.md)

Tüm güvenlik yol haritası oluşturma ile ilgili daha fazla bilgi için "Müşteri sorumlulukları ve yol haritası" bölümüne bakın [Microsoft Cloud Security Kurumsal Mimarlar için](http://aka.ms/securecustomer) belge. Aşağıdaki konulardan birini yardımcı olmak için Microsoft Hizmetleri katılımcılarını sürece dahil etme hakkında daha fazla bilgi için Microsoft temsilcinize başvurun veya ziyaret bizim [siber güvenlik çözümleri sayfa](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

<!--Image references-->
[1]: ../media/active-directory-privileged-identity-management-configure/Search_PIM.png
[2]: ../media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ../media/active-directory-identityprotection/29.png
