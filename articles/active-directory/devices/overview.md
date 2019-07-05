---
title: Azure Active Directory'de cihaz Kimliği nedir?
description: Cihaz Kimlik Yönetimi, ortamınızdaki kaynakların erişen cihazları yönetmek için nasıl yardımcı olabileceğini öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: overview
ms.date: 06/27/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5d38ca8bdf93ff201b3f5842f4cb0e8409dcd0c3
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67481671"
---
# <a name="what-is-a-device-identity"></a>Cihaz kimliği nedir?

Tüm şekil ve boyutları ve kendi cihazını getir (KCG) kavramı cihazları yaygınlaşmasının ile BT uzmanları, iki biraz kpı'lere hedefleriyle kalmaktadır:

- Son kullanıcıların ne zaman ve yerde üretken izin ver
- Kuruluşun varlıklarını koruma

Bu varlıklar korumak için BT personelinin ilk cihaz kimliklerini yönetme gerekir. BT personeli, güvenlik ve uyumluluk standartlarını karşılandığından emin olmak için Microsoft Intune gibi araçlarla cihaz kimliği oluşturabilirsiniz. Azure Active Directory (Azure AD), cihazları ve uygulamaları için çoklu oturum açma sağlar ve hizmetlere herhangi bir yerden bu cihazlar.

- Kullanıcılarınızın, ihtiyaç duydukları kuruluşunuzun varlıklarına erişin. 
- BT personelinizin, kuruluşunuzun güvenliğini sağlamak için ihtiyaç duydukları denetimleri alın.

Cihaz kimlik yönetimi için temel, [cihaz tabanlı koşullu erişim](../conditional-access/require-managed-devices.md). Cihaz tabanlı koşullu erişim ilkeleriyle ortamınızdaki kaynaklarına erişimi yalnızca yönetilen cihazlarla mümkün olduğundan emin olun.

## <a name="getting-devices-in-azure-ad"></a>Azure AD'de aygıt alma

Azure AD'de bir cihaz almak için birçok seçeneğiniz vardır:

- **Azure AD kayıtlı**
   - Azure AD kayıtlı cihazlar genellikle kişisel olarak sahip olduğu veya mobil cihazlara olan ve uygulamasında kişisel bir Microsoft hesabı veya başka bir yerel hesap ile oturum.
      - Windows 10
      - iOS
      - Android
      - macOS
- **Azure AD'ye katıldı**
   - Azure AD'ye katılmış cihazlar bir kuruluşa ait ve bu kuruluşa ait bir Azure AD hesabı ile oturum açmış. Bunlar, yalnızca bulutta mevcut.
      - Windows 10 
- **Hibrit Azure AD'ye katılmış**
   - Hibrit Azure AD'ye katılmış cihazlar bir kuruluşa ait ve bu kuruluşa ait bir Azure AD hesabı ile oturum açmış. Bunlar bulutta mevcut ve 
      - Windows 7, 8.1 veya 10
      - Windows Server 2008 veya üzeri

![Azure AD cihazları için dikey pencerede gösterilen cihazlar](./media/overview/azure-ad-devices-all-devices-overview.png)

## <a name="device-management"></a>Cihaz yönetimi

Azure AD'de cihazları Intune, System Center Configuration Manager, Grup İlkesi (hibrit Azure AD katılımı), mobil uygulama yönetimi (MAM) araçları veya diğer üçüncü taraf araçları gibi mobil cihaz Yönetimi (MDM) araçlarını kullanarak yönetilebilir.

## <a name="resource-access"></a>Kaynak erişimi

Kaydetme ve katılma özelliği bu kaynaklara koşullu erişim ilkelerini uygulamak için bulut kaynakları ve Yöneticiler için sorunsuz oturum açma (SSO) kullanıcılarınızın verin. 

SSO özelliğinden yararlanır, kuruluşunuzun şirket içi kaynakların yanı sıra bulut kaynakları için Azure AD'ye katılmış cihazlar veya hibrit Azure AD'ye katıldı. Daha fazla bilgi makalesinde bulunabilir [nasıl SSO için şirket içi kaynakları çalışır Azure AD alanına katılmış cihazlar](azuread-join-sso.md).

## <a name="device-security"></a>Cihaz güvenliği

- **Azure AD'ye kayıtlı cihazlar** son kullanıcı tarafından yönetilen bir hesap kullanın, bu hesap bir Microsoft hesabı ya da bir veya daha fazlasını korunan başka bir yerel olarak yönetilen kimlik bilgileri.
   - Parola
   - PIN
   - Desen
   - Windows Hello
- **Azure AD'ye katılmış veya hibrit Azure AD'ye katılmış cihazları** birini veya birkaçını ile güvenliği sağlanan Azure AD'de bir kuruluş hesabı kullanın.
   - Parola
   - İş İçin Windows Hello

## <a name="provisioning"></a>Sağlama

Cihazları Azure AD'ye alınırken bir Self Servis şekilde ya da denetimli bir sağlama sürecindeki yöneticileri tarafından gerçekleştirilebilir.

## <a name="summary"></a>Özet

Azure AD'de cihaz Kimlik Yönetimi ile şunları yapabilirsiniz:

- Getirme ve Azure AD'de cihazları yönetme işlemini basitleştirin
- Kullanıcılarınıza, kuruluşunuzun bulut tabanlı kaynaklarına kolay erişim olanağı sağlama

## <a name="license-requirements"></a>Lisans gereksinimleri

[!INCLUDE [Active Directory P1 license](../../../includes/active-directory-p1-license.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [Azure AD'ye kayıtlı cihazlar](concept-azure-ad-register.md)
- Daha fazla bilgi edinin [Azure AD'ye katılmış cihazlar](concept-azure-ad-join.md)
- Daha fazla bilgi edinin [hibrit Azure AD'ye katılmış cihazlar](concept-azure-ad-join-hybrid.md)
- Azure portalında cihaz kimliklerini nasıl yöneteceğiniz hakkında genel bakış edinmek için [Azure portalını kullanarak cihaz kimliklerini yönetme](device-management-azure-portal.md).
- Cihaz tabanlı koşullu erişim hakkında daha fazla bilgi için bkz: [cihaz tabanlı koşullu erişim ilkeleri Azure Active Directory'yi Yapılandır](../conditional-access/require-managed-devices.md).
