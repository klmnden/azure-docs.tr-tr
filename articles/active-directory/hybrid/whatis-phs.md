---
title: Azure AD'ye parola karması eşitleme nedir? | Microsoft Docs
description: Parola Karması eşitlemeyi açıklar.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 12/05/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: f588115e38ded07b79568c91b8d166949b34a886
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60294969"
---
# <a name="what-is-password-hash-synchronization-with-azure-ad"></a>Azure AD'ye parola karması eşitleme nedir?
Parola Karması eşitleme karma kimlik gerçekleştirmek için kullanılan oturum açma yöntemleri biridir. Karma bir karma Azure AD Connect eşitlendiğinde, şirket içi Active Directory örneğinden kullanıcılar parola bulut tabanlı bir Azure ad örneği.

Parola Karması eşitleme uygulanan Azure AD Connect eşitlemesi ile dizin eşitleme özelliğini uzantısıdır. Bu özellik, Office 365 gibi Azure AD hizmetlerinde oturum açmak için kullanabilirsiniz. Hizmete şirket içi Active Directory Örneğinizde oturum açmak için kullandığınız aynı parolayı kullanarak oturum açın.

![Azure AD Connect nedir?](./media/how-to-connect-password-hash-synchronization/arch1.png)

Parola Karması eşitleme için yalnızca bir tane korumak için kullanıcılarının ihtiyaç duyduğu olan parola sayısını azaltarak yardımcı olur. Parola Karması eşitleme yapabilirsiniz:

* Kullanıcılarınızın verimliliğini artırın.
* Yardım Masası maliyetlerini azaltın.  

Kullanmaya karar verirseniz, isteğe bağlı olarak, parola karması eşitleme yedek olarak ayarlayabilirsiniz [Active Directory Federasyon Hizmetleri (AD FS) ile Federasyon](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect) , oturum açma yöntemi olarak.

Parola Karması eşitleme, ortamınızda kullanmak için yapmanız:

* Azure AD Connect yükleme.  
* Şirket içi Active Directory örneğinizi ve Azure Active Directory örneğinizle arasında dizin eşitlemesi yapılandırın.
* Parola karma eşitlemesini etkinleştirme.



Daha fazla bilgi için [karma kimlik nedir?](whatis-hybrid-identity.md).




## <a name="next-steps"></a>Sonraki Adımlar

- [Hibrit kimlik nedir?](whatis-phs.md)
- [Azure AD Connect ve Connect Health nedir?](whatis-azure-ad-connect.md)
- [Geçişli kimlik doğrulaması (PTA) nedir?](how-to-connect-pta.md)
- [Federasyon nedir?](whatis-fed.md)
- [Üzerinde çoklu oturum nedir?](how-to-connect-sso.md)
- [Parola Karması eşitleme nasıl çalışır?](how-to-connect-password-hash-synchronization.md)
