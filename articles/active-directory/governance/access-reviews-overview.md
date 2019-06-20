---
title: Erişim gözden geçirmeleri nedir? -Azure Active Directory | Microsoft Docs
description: Azure Active Directory erişim gözden geçirmelerini kullanarak, yönetim, risk yönetimi ve kuruluşunuzdaki uyumluluk girişimlerini karşılamak için Grup üyeliği ve uygulama erişimini denetleyebilirsiniz.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 06/05/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7fcc804db66430598e72e9ebf31a8837eda1cca6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67204601"
---
# <a name="what-are-azure-ad-access-reviews"></a>Nelerdir Azure AD erişim gözden geçirmeleri?

Azure Active Directory (Azure AD) erişim gözden geçirmeleri, kuruluşların grup üyeliğini yönetme, erişim kurumsal uygulamalar ve rol atamalarını verimli bir şekilde olanak sağlar. Kullanıcının erişimini yalnızca doğru kişilere erişim devam emin olmak için düzenli olarak gözden geçirilebilir.

Erişim gözden geçirmeleri hızlı bir genel bakış sağlayan bir video şu şekildedir:

>[!VIDEO https://www.youtube.com/embed/kDRjQQ22Wkk]

## <a name="why-are-access-reviews-important"></a>Erişimi neden önemli incelemeleri?

Azure AD, dahili olarak, kuruluşunuz içinde ve iş ortakları gibi dış kuruluşlardan kullanıcılarla işbirliği sağlar. Kullanıcıları gruplara katılmak, konuklar davet, bulut uygulamalarına bağlanın ve kendi iş veya kişisel cihazlardan uzaktan çalışın. Self Servis gücünden yararlanan kolaylık daha iyi erişim yönetimi özellikleri gereksinimini açmıştır.

- Nasıl yeni çalışanlar katılma gibi üretken olmak için doğru erişime sahip oldukları emin olabilirim?
- Nasıl kişiler, takımlar taşıyın veya şirketten gibi özellikle Konukları gerektirdiğinde, eski erişim kaldırıldı emin olabilirim?
- Aşırı erişim hakları geldikleri erişim üzerinde denetim olmaması gibi denetim bulgularını ve güvenlik ihlalleri neden olabilir.
- Kaynak sahiplerine kaynaklarına kimlerin erişebileceğini, düzenli olarak gözden emin olmak için proaktif olarak etkileşim kurmak zorunda.

## <a name="when-to-use-access-reviews"></a>Ne zaman kullanılır erişim gözden geçirmeleri?

- **Çok sayıda kullanıcı ayrıcalıklı roller:** Kaç kullanıcının yönetici erişimi denetlemek için iyi bir fikirdir, kaç bunları genel Yöneticiler, konuklar veya bir yönetim görevi gerçekleştirmek için atandıktan sonra kaldırılmamış iş ortakları ve olmadığını davet. Rol ataması kullanıcılar onaylayabilirsiniz [Azure AD rolleri](../privileged-identity-management/pim-how-to-perform-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) gibi genel Yöneticiler veya [Azure kaynak rolleri](../privileged-identity-management/pim-resource-roles-perform-access-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) gibi kullanıcı erişimi Yöneticisi'nde [Azure AD Privileged Identity Management (PIM)](../privileged-identity-management/pim-configure.md) karşılaşırsınız.
- **Ne zaman Otomasyon olanaksız olacaktır:** Güvenlik grupları veya Office 365 gruplarında dinamik üyelik ancak ne ik veri Azure AD'de değil veya kullanıcılar yine de erişim grubu kendi değiştirme eğitmek için bırakarak sonra ihtiyacınız varsa için kurallar oluşturabilir miyim? Yine de erişmesi gereken kişiler erişimi devam etmesi gerekip emin olmak için bu Grup İnceleme oluşturabilirsiniz.
- **Bir gruba yeni bir amaç için kullanıldığında:** Azure AD ile eşitlenmesi için gittiği bir grubunuz varsa ya da satış ekibi gruptaki herkes için Salesforce uygulama etkinleştirmeyi planlıyorsanız, önce bir farklı risk ortak kullanılan grubun grup üyeliğini gözden geçirmek için Grup sahibi istemek yararlı olacaktır içeriği.
- **İş kritik veri erişimi:** belirli kaynaklar için onu dışında istemem için gerekli olabilecek düzenli olarak oturumunu kapatmak ve neden erişim denetim amacıyla için ihtiyaç duydukları üzerinde bir gerekçe vermek mümkün KILAR.
- **Bir ilkenin özel durum listesi tutmak için:** İdeal bir dünyada, tüm kullanıcılar, kuruluşunuzun kaynaklarına erişimi güvenli hale getirmek için erişim ilkeleri izlemeniz gerekir. Ancak, bazı durumlarda özel durumları yapmanızı gerektiren iş durumlar vardır. BT yöneticisi olarak, bu görevi yönetmek, İlkesi özel durumları sözleşmeli önlemek ve bu özel durumlar düzenli olarak gözden kavram ile denetçiler sağlayın.
- **Grup sahipleri kendi gruplarındaki Konukları çağrılmasına halen ihtiyaçları onaylamanız istenir:** Çalışan erişimi, şirket içi IAM, konuklar davet edilen değil ancak bazı otomatik. Bir grubu Konukları iş hassas içerik ve ardından onun Konukları onaylamak için Grup sahibinin sorumluluk çözümlenmedi erişim için bir işletme ihtiyaçlarına erişmenizi durumunda.
- **Düzenli aralıklarla yinelenen incelemeleri vardır:** Kümesi frekansları kullanıcıların erişim gözden geçirmeleri yinelenen gibi haftalık, aylık, üç aylık veya yıllık olarak ayarlayabilirsiniz ve gözden geçirenler her İnceleme başlangıcında bildirilir. Gözden geçirenler onaylayabilir veya Yardımı akıllı öneriler ve kullanıcı dostu bir arabirim ile erişimi engelle.

## <a name="where-do-you-create-reviews"></a>Gözden geçirmeler oluşturduğunuz?

Gözden geçirme istediğinize bağlı olarak, erişim gözden geçirme oluşturacak Azure AD'de incelemeleri, Azure AD Kurumsal uygulamaları (önizlemede) veya Azure AD PIM erişimi.

| Kullanıcı erişim hakları | Gözden geçirenler olabilir | Oluşturulan gözden geçirme | Gözden Geçiren deneyimi |
| --- | --- | --- | --- |
| Güvenlik grubu üyeleri</br>Office grubu üyeleri | Belirtilen gözden geçirenler</br>Grup sahipleri</br>Kendi kendini gözden geçirin | Azure AD erişim gözden geçirmeleri</br>Azure AD grupları | Erişim paneli |
| Bağlı bir uygulamaya atanan | Belirtilen gözden geçirenler</br>Kendi kendini gözden geçirin | Azure AD erişim gözden geçirmeleri</br>Azure AD Kurumsal uygulamaları (önizlemede) | Erişim paneli |
| Azure AD rolü | Belirtilen gözden geçirenler</br>Kendi kendini gözden geçirin | [Azure AD PIM](../privileged-identity-management/pim-how-to-start-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) | Azure portal |
| Azure Kaynak rolü | Belirtilen gözden geçirenler</br>Kendi kendini gözden geçirin | [Azure AD PIM](../privileged-identity-management/pim-resource-roles-start-access-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) | Azure portal |

## <a name="which-users-must-have-licenses"></a>Hangi kullanıcıların lisansına sahip olması gerekir?

Erişim gözden geçirmeleri ile etkileşime giren her kullanıcının Ücretli bir Azure AD Premium P2 Lisansı olmalıdır. Örneklere şunlar dahildir:

- Erişim gözden geçirmesi Oluştur yöneticileri
- Bir erişim gerçekleştiren Grup sahiplerinin gözden geçirin
- Gözden geçirenler olarak atanan kullanıcılar
- Kendi kendini gözden gerçekleştiren kullanıcı

Ayrıca kendi erişimini gözden geçirmek için konuk kullanıcıları sorabilir. Kendi kuruluşunuzun kullanıcılardan birinin atadığınız her Ücretli Azure AD Premium P2 lisansı için dış kullanıcı indirimi altında en fazla beş Konuk kullanıcıları davet etmek için Azure AD--işletmeler arası (B2B) kullanabilirsiniz. Bu konuk kullanıcılara Azure AD Premium P2 özellikleri de kullanabilirsiniz. Daha fazla bilgi için [Azure AD B2B işbirliği lisanslama Kılavuzu](../b2b/licensing-guidance.md).

Olmalıdır lisans sayısını belirlemenize yardımcı olması için bazı örnek senaryolar aşağıda verilmiştir.

| Senaryo | Hesaplama | Gerekli lisans sayısı |
| --- | --- | --- |
| Bir yönetici ile 500 kullanıcıya bir erişim gözden geçirmesi gruba oluşturur.<br/>3 Grup sahiplerinin gözden geçirenler olarak atar. | 1 yönetici + 3 Grup sahipleri | 4 |
| Bir yönetici ile 500 kullanıcıya bir erişim gözden geçirmesi gruba oluşturur.<br/>Kendi kendini gözden kolaylaştırır. | 1 yönetici + 500 kullanıcıya kendini Gözden Geçiren olarak | 501 |
| Yönetici 5 kullanıcı ve 25 konuk kullanıcıların erişim gözden geçirmesi gruba oluşturur.<br/>Kendi kendini gözden kolaylaştırır. | 1 yönetici + kendini Gözden Geçiren olarak 5 kullanıcı<br/>(konuk kullanıcılar gerekli 1:5 oranı ele alınmıştır) | 6 |
| Yönetici, 5 kullanıcı ve 28 konuk kullanıcıların erişim gözden geçirmesi gruba oluşturur.<br/>Kendi kendini gözden kolaylaştırır. | 1 yönetici + 5 kullanıcı kendini Gözden Geçiren olarak + 1 kullanıcı gerekli 1:5 oranı Konuk kullanıcıları kapsayacak şekilde | 7 |

İçin kullanılan lisansları atama hakkında daha fazla bilgi için bkz: [atamayı veya kaldırmayı lisanslarını kullanarak Azure Active Directory portalında](../fundamentals/license-users-groups.md).

## <a name="learn-about-access-reviews"></a>Erişim gözden geçirmeleri hakkında bilgi edinin

Oluşturma ve erişim gözden geçirmesi gerçekleştirme hakkında daha fazla bilgi için bu kısa bir tanıtım izleyin:

>[!VIDEO https://www.youtube.com/embed/6KB3TZ8Wi40]

Erişim gözden geçirmeleri, kuruluşunuzda dağıtmak hazırsanız, videonun ekleme, yöneticilerinize eğitmek için aşağıdaki adımları izleyin ve ilk erişim gözden geçirmesi oluşturma!

>[!VIDEO https://www.youtube.com/embed/X1SL2uubx9M]

## <a name="license-requirements"></a>Lisans gereksinimleri

[!INCLUDE [Azure AD Premium P2 license](../../../includes/active-directory-p2-license.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Grupları ve uygulamaları, erişim gözden geçirmesi oluştur](create-access-review.md)
- [Bir Azure AD yönetici rolündeki kullanıcılar için erişim gözden geçirmesi oluşturma](../privileged-identity-management/pim-how-to-start-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json)
- [Gruplar veya uygulamalar için erişim gözden geçirin](perform-access-review.md)
- [Grupları ve uygulamaları, erişim değerlendirmesi tamamlama](complete-access-review.md)
