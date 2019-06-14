---
title: Katılmış cihazlarda şirket kaynaklarına SSO, Azure AD'de nasıl çalışır? | Microsoft Docs
description: Hibrit Azure Active Directory'ye katılmış cihazları elle nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.subservice: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/20/2018
ms.author: joflore
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 45941de6a90a5824ebc1e5d31b18b68f5fd9d493
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60353202"
---
# <a name="how-sso-to-on-premises-resources-works-on-azure-ad-joined-devices"></a>Katılmış cihazlarda şirket kaynaklarına SSO, Azure AD'de nasıl çalışır?

Azure Active Directory (Azure AD) alanına katılmış cihaz, bir çoklu oturum açma (SSO) deneyimi kiracınızın bulut uygulamaları için verdiğini şaşkınlık olan olmayabilir. Ortamınızda bir şirket içi Active Directory (AD) varsa, SSO bir deneyim için bu cihazlarda genişletebilirsiniz.

Bu makalede, bu işlemin nasıl çalıştığı açıklanmaktadır.

## <a name="how-it-works"></a>Nasıl çalışır? 

Tek tek kullanıcı adınızı ve parolanızı hatırlayacağınızdan gerektiğinden, SSO kaynaklarınıza erişimi basitleştirir ve ortamınızın güvenliğini artırır. Azure AD alanına katılmış cihaz ile kullanıcılarınızın zaten ortamınızda bulut uygulamalarının bir SSO deneyimine sahip. Ortamınızda bir Azure AD varsa ve şirket içi bir AD, büyük olasılıkla şirket içi satır, iş kolu (LOB) uygulamalarınızı, dosya paylaşımları ve yazıcılar, SSO bir deneyim kapsamını genişletmek istediğiniz.  


Azure AD'ye katılmış cihazları, şirket içi hakkında hiçbir bilgiye sahip AD ortam için katılmamış olduğundan. Ancak, şirket içi hakkında ek bilgiler sağlayabilir. Azure AD Connect ile bu cihazlara AD.
Her ikisi de sahip ortam, Azure AD ve şirket içi bir AD, aynı zamanda, bilinen hibrit ortamı sahiptir. Karma bir ortamınız varsa, bulutta, şirket içi kimlik bilgilerini eşitlemek için dağıtılan Azure AD Connect henüz olasıdır. Eşitleme işleminin bir parçası olarak, Azure AD Connect, şirket içi etki alanı bilgilerini Azure AD'ye eşitler. Ne zaman bir kullanıcı için bir Azure AD oturum açtığında cihaz, karma bir ortamda, alanına:

1. Azure AD, şirket içi etki alanı kullanıcı adı bir üyesidir cihaza geri gönderir. 

2. Yerel güvenlik yetkilisi (LSA) hizmeti, cihaz üzerinden Kerberos kimlik doğrulamasını etkinleştirir.

Kullanıcının şirket içi etki alanı, cihazın bir kaynağa erişim denemesi sırasında:

1. Bir etki alanı denetleyicisi (DC) bulmak için etki alanı bilgilerini kullanır. 

2. Şirket içi bulunduğu DC'ye kimliği doğrulanmış kullanıcı almak için etki alanı bilgilerini ve kullanıcı kimlik bilgilerini gönderir.

3. Bir Kerberos alır [bilet sağlayan bilet (TGT)](https://docs.microsoft.com/windows/desktop/secauthn/ticket-granting-tickets) AD alanına katılmış kaynaklara erişmek için kullanılır.

İçin yapılandırılmış olan tüm uygulamalar **Windows tümleşik kimlik doğrulaması** kullanıcı bunları erişmeye çalıştığında sorunsuz SSO alın.  

Windows iş için Hello bir Azure AD alanına katılmış bir CİHAZDAN şirket içi SSO'yu etkinleştirmek için ek yapılandırma gerektirir. Daha fazla bilgi için [yapılandırma Azure AD alanına katılmış cihazlar için şirket içi çoklu oturum Windows Hello for Business kullanan üzerinde](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-hybrid-aadj-sso-base). 

## <a name="what-you-get"></a>Ne alacaksınız

SSO ile bir Azure AD üzerinde yapabilecekleriniz cihaz alanına: 

- Bir UNC yolu bir AD üye sunucuya erişim

- Windows tümleşik güvenliği için yapılandırılmış bir AD üye web sunucusuna erişim 



Şirket içi yönetmek istiyorsanız bir Windows cihazından AD yükleme [uzak sunucu yönetim araçları Windows 10 için](https://www.microsoft.com/en-us/download/details.aspx?id=45520).

Şunları kullanabilirsiniz:

- Active Directory Kullanıcıları ve Bilgisayarları (ADUC) tüm AD nesnelerini yönetmek için ek bileşenini. Ancak, el ile bağlanmak istediğiniz etki alanı belirtmeniz gerekir.

- DHCP bir AD alanına katılmış DHCP sunucusunu yönetmek için ek bileşenini. Ancak, DHCP sunucusu adını veya adresini belirtmeniz gerekebilir.

 
## <a name="what-you-should-know"></a>Bilmeniz gerekenler

Ayarlamanız gerekebilir, [etki alanı tabanlı filtreleme](../hybrid/how-to-connect-sync-configure-filtering.md#domain-based-filtering) gereken etki alanları hakkında veri eşitlendiğinden emin olmak için Azure AD CONNECT'te.

Bir bilgisayar nesnesi, uygulamalar ve Active Directory Azure AD'ye katılmış cihazlar için makine kimlik doğrulaması çalışmaz bağımlı kaynaklar AD'de sahip değilsiniz. 

Dosyaları, Azure AD'ye katılmış cihaz üzerindeki diğer kullanıcılarla paylaşamazsınız.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için [Azure Active Directory'de cihaz yönetimi nedir?](overview.md) 
