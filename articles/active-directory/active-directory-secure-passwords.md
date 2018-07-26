---
title: Azure AD katmanlı parola güvenliği | Microsoft Docs
description: Azure AD’nin güçlü parolaları zorunlu tutarak kullanıcıların parolalarını siber suçlulardan nasıl koruduğu açıklanır.
services: active-directory
documentationcenter: ''
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 08/28/2017
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: rogoya
ms.openlocfilehash: 277e66e954d0fdff026d473ba3f6ad07a8b87da7
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39173418"
---
# <a name="a-multi-tiered-approach-to-azure-ad-password-security"></a>Azure AD parola güvenliğine çok katmanlı bir yaklaşım

Bu makalede, Azure Active Directory (Azure AD) veya Microsoft Hesabınızı korumak için kullanıcı veya yönetici olarak izleyebileceğiniz en iyi yöntemlerden bazıları ele alınmıştır.

 > [!NOTE]
 > **Oturum açmada sorun yaşadığınız için mi buradasınız?** Sorun yaşıyorsanız bkz. [kendi parolanızı değiştirme ve sıfırlama](user-help/active-directory-passwords-update-your-own-password.md).
 >
 > Azure AD yöneticileri [Azure Active Directory'de kullanıcının parolasını sıfırlama](fundamentals/active-directory-users-reset-password-azure-portal.md) makalesindeki yönergeleri kullanarak kullanıcı parolalarını sıfırlayabilir.
 >

## <a name="password-requirements"></a>Parola gereksinimleri

Azure AD, parolaların güvenliğini sağlama konusunda aşağıdaki genel yaklaşımları benimser:

* Parola uzunluğu gereksinimleri
* Parola karmaşıklığı gereksinimleri
* Normal ve düzenli parola sona erme süresi

Azure Active Directory’de parola sıfırlama hakkında bilgi için [BT uzmanları için Azure AD kendi kendine parola sıfırlama](user-help/active-directory-passwords-update-your-own-password.md) konusuna bakın.

## <a name="azure-ad-password-protections"></a>Azure AD parola korumaları

Azure AD ve Microsoft Hesap Sistemi, kullanıcı ve yönetici parolalarının güvenliğini sağlamak için sektörde kanıtlanmış yaklaşımları kullanır. Bu yaklaşımlar şunları içerir:

* Dinamik olarak yasaklanmış parolalar
* Akıllı Parola Kilitleme

Güncel bir araştırmaya göre parola yönetimi hakkında bilgi almak için [Parola Yönergeleri](https://aka.ms/passwordguidance) teknik incelemesine bakın.

### <a name="dynamically-banned-passwords"></a>Dinamik olarak yasaklanmış parolalar

Azure AD ve Microsoft Hesapları, yaygın olarak kullanılan parolaları yasaklayarak, parola korumasını gerçekleştirir. Azure AD Kimlik Koruması ekibi, yasaklı parola listelerini düzenli olarak analiz eder ve kullanıcıların yaygın olarak kullanılan parolaları seçmesini engeller. Bu hizmet, Azure AD ve Microsoft Hesap Hizmeti müşterileri tarafından kullanılabilir.

Parola oluşturulurken, yöneticilerin kullanıcıları harf, sayı, karakter ve sözcüklerin benzersiz bir bileşiminden oluşan parola tümcecikleri seçmeye teşvik etmesi gerekir. Bu yaklaşım kullanıcı parolalarının gizliliğinin tehlikeye girmesini neredeyse imkansız hale getirirken kullanıcıların anımsamalarını da kolaylaştırır.

#### <a name="password-breaches"></a>Parola ihlalleri

Microsoft, siber suçluların her zaman bir adım önünde olmak için çalışmaktadır.

Azure AD Kimlik Koruması ekibi, yaygın olarak kullanılan parolaları sürekli olarak analiz eder. Siber suçlular da saldırılarını geliştirmek üzere parola karmalarını çözmek için [şifre tablosu](https://en.wikipedia.org/wiki/Rainbow_table) oluşturmak gibi benzer stratejiler kullanır.

Microsoft dinamik olarak güncellenen bir yasaklı parola listesi tutmak üzere [veri ihlallerini](https://www.privacyrights.org/data-breaches) sürekli olarak analiz eder; bu liste, güvenlik açığı olan parolaların Azure AD müşterileri için gerçek bir tehdit haline gelmesinden önce engellenmesini sağlar. Geçerli güvenlik çalışmalarımız hakkında daha fazla bilgi için bkz. [Microsoft Güvenlik Bilgileri Raporu](https://www.microsoft.com/security/sir/default.aspx).

### <a name="smart-password-lockout"></a>Akıllı Parola Kilitleme

Azure AD bir kullanıcı parolasına saldırmaya çalışan olası bir siber suçluyu algıladığında, kullanıcı hesabı Akıllı Parola Kilitleme özelliğiyle kilitlenir. Azure AD, belirli oturumlarla ilişkili riski belirlemek için tasarlanmıştır. En güncel güvenlik verilerini kullanırken, siber tehditleri durdurmak için karşı kilitleme semantiği uyguluyoruz.

Bir kullanıcı Azure AD’nin dışında kalırsa, ekranı aşağıdaki gibi görünür:

  ![Azure AD dışında kalmış](./media/active-directory-secure-passwords/locked-out-azuread.png)

Diğer Microsoft hesapları için ekran aşağıdaki gibi görünür:

  ![Microsoft hesabı dışında kalmış](./media/active-directory-secure-passwords/locked-out-ms-accounts.png)

Azure Active Directory’de parola sıfırlama hakkında bilgi için [BT uzmanları için Azure AD kendi kendine parola sıfırlama](user-help/active-directory-passwords-update-your-own-password.md) konusuna bakın.

  >[!NOTE]
  >Bir Azure AD yöneticisiyseniz, kullanıcıların geleneksel parolalar oluşturmasını önlemek için [Windows Hello](https://www.microsoft.com/windows/windows-hello) kullanabilirsiniz.
  >

## <a name="next-steps"></a>Sonraki adımlar

* [Kendi parolanızı güncelleştirme](user-help/active-directory-passwords-update-your-own-password.md)
* [Azure kimlik yönetimi ile ilgili temel bilgiler](fundamentals-identity.md)
* [Parola sıfırlama etkinliğini bildirme](authentication/howto-sspr-reporting.md)
