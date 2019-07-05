---
title: Bir Azure AD nedir, cihazı alanına?
description: Cihaz Kimlik Yönetimi ortamınızdaki kaynakların erişen cihazların yönetmenize nasıl yardımcı olabileceğini öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4af3aea7218ea8792bb66188e8df7baf9f460b0b
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67462817"
---
# <a name="azure-ad-joined-devices"></a>Azure AD’ye katılmış cihazlar

Azure AD join, bulut öncelikli veya salt bulut isteyen kuruluşlar için tasarlanmıştır. Her kuruluş, sektör ve boyutu ne olursa olsun Azure AD'ye katılmış cihazlara dağıtabilirsiniz. Hem bulut hem de şirket içi uygulamaları ve kaynaklarına erişimi etkinleştirme bile karma bir ortamda, Azure AD'ye katılım'ı çalışır.

|   | Azure AD'ye katılım |
| --- | --- |
| **Tanım** | Cihaza oturum açmak için kuruluş hesabı gerekmeden yalnızca Azure AD'ye katılmış |
| **Birincil hedef kitlesi** | Yalnızca bulutta yer alan her ikisi için uygun ve hibrit kuruluşlar. |
|   | Bir kuruluştaki tüm kullanıcılar için geçerlidir |
| **Cihaz sahipliği** | Kuruluş |
| **İşletim sistemleri** | Tüm Windows 10 cihazları |
| **Sağlama** | Self Servis: Windows OOBE veya ayarları |
|   | Toplu kayıt |
|   | Windows Autopilot |
| **Cihaz oturum açma seçenekleri** | Kuruluş hesapları kullanarak: |
|   | Parola |
|   | İş İçin Windows Hello |
|   | FIDO2.0 güvenlik anahtarları (Önizleme) |
| **Cihaz yönetimi** | Mobil cihaz Yönetimi (örnek: Microsoft Intune) |
|   | Microsoft Intune ve System Center Configuration Manager ile ortak yönetim |
| **Anahtar özellikleri** | Hem bulut hem de şirket içi kaynaklara SSO |
|   | Koşullu erişim aracılığıyla MDM kaydı ve MDM uyumluluk değerlendirmesi |
|   | Kilit ekranında Self Servis parola sıfırlama ve Windows Hello PIN sıfırlama |
|   | Kurumsal durumda Dolaşım cihazlar arasında |

Azure AD'ye katılmış cihazları oturum açmaya bir kuruluş için kullanılacak Azure AD hesabı. Kuruluşunda bulunan kaynaklara erişimi daha fazla olabilir sınırlı dayalı bu Azure AD hesap ve [koşullu erişim ilkeleri](../conditional-access/overview.md) cihaz kimliğine uygulanır.

Daha fazla denetim Azure AD'ye katılmış cihazların Microsoft Intune gibi veya ortak yönetim senaryolarında System Center Configuration Manager kullanarak mobil cihaz Yönetimi (MDM) araçlarını kullanarak ve yöneticiler güvenli. Bu araçlar, şifrelenmiş depolama, parola karmaşıklığı, yazılım yüklemeleri ve yazılım güncelleştirmeleri gerektirme gibi kuruluş gerekli yapılandırmaları zorlamak için bir yol sağlar. Yöneticiler yapabilir kuruluş uygulamaları kullanarak Azure AD'ye katılmış cihazlar için kullanılabilir [System Center Configuration Manager ve iş için Microsoft Store](https://docs.microsoft.com/sccm/apps/deploy-use/manage-apps-from-the-windows-store-for-business).

Azure AD'ye katılım seçenekleri gibi ilk çalıştırma deneyimi (OOBE), toplu kayıt, Self Servis kullanarak gerçekleştirilebilir veya [Windows Autopilot](https://docs.microsoft.com/intune/enrollment-autopilot).

Kuruluşun ağ üzerinde olmaları durumunda azure AD'ye katılmış cihazları yine de şirket içi kaynaklara çoklu oturum açma erişimi koruyabilirsiniz. Azure AD'ye katılmış cihazlar yine de dosya, yazdırma ve diğer uygulamalar gibi şirket içi sunucular için kimlik doğrulaması yapabilir.

## <a name="scenarios"></a>Senaryolar

Azure AD’ye katılma özelliği temel olarak bir şirket içi Windows Server Active Directory altyapısı bulunmayan kuruluşlar için tasarlanmıştır ancak aşağıdaki senaryolarda siz de bu özellikten yararlanabilirsiniz:

- Azure AD ve MDM benzeri Intune kullanarak bulut tabanlı altyapıya geçiş yapmak istediğinizde.
- Şirket içi etki alanına katılma özelliğini kullanamadığınız durumlarda; örneğin, tabletler ve telefonlar gibi mobil cihazlar üzerinde denetim sağlamanız gerektiğinde.
- Kullanıcılarınızın temel olarak Office 365 veya Azure AD ile tümleşik diğer SaaS uygulamalarına erişmesi gerektiğinde.
- Active Directory yerine Azure AD’de bir kullanıcı grubunu yönetmek istediğinizde. Bu senaryo, örneğin, Dönemsel çalışanların, yüklenicilerin veya Öğrenciler uygulayabilirsiniz.
- Sınırlı şirket içi altyapısı olan uzak şube ofislerindeki çalışanlara katılma özellikleri sağlamak istediğinizde.

Windows 10 cihazları için Azure AD’ye katılmış cihazları yapılandırabilirsiniz.

Azure AD'ye katılmış cihazların hedefi şu işlemlerde basitleştirme sağlamaktır:

- İşe ait cihazların Windows dağıtımları
- Herhangi bir Windows cihazından kuruluş uygulamalarına ve kaynaklarına erişim
- İşe ait cihazların bulut tabanlı yönetimi
- Kullanıcılar cihazlarını kendi Azure AD ile oturum açabilir veya Eşitlenen Active Directory iş veya Okul hesapları.

![Azure AD’ye katılmış cihazlar](./media/concept-azure-ad-join/azure-ad-joined-device.png)

Aşağıdaki yöntemlerden birini kullanarak Azure AD'ye Katılım dağıtımı yapılabilir:

- [Windows Autopilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot)
- [Toplu dağıtım](https://docs.microsoft.com/intune/windows-bulk-enroll)
- [Self servis deneyimi](azuread-joined-devices-frx.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD join uygulamanızı planlama](azureadjoin-plan.md)
- [Azure AD'de yerel Yöneticiler grubuna yönetme alanına katılmış cihazları](assign-local-admin.md)
- [Azure portalını kullanarak cihaz kimliklerini yönetme](device-management-azure-portal.md)
- [Azure AD'de eski cihazları yönetme](manage-stale-devices.md)
