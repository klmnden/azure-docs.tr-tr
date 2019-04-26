---
title: Bir B2B özellikleri Konuk kullanıcı - Azure Active Directory | Microsoft Docs
description: Azure Active Directory B2B Konuk kullanıcı özelliklerini ve durumları önce ve sonra Davetiyesi kullanımı
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.custom: it-pro, seo-update-azuread-jan, seoapril2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4d4466e4ac7a4e818da6332254e3094eccbaf2b4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60355582"
---
# <a name="properties-of-an-azure-active-directory-b2b-collaboration-user"></a>Bir Azure Active Directory B2B işbirliği kullanıcısı özellikleri

Bu makalede, önce ve sonra Davetiyesi kullanımı özellikleri ve Azure Active Directory'de (Azure AD) B2B Konuk kullanıcı nesnenin durumlarını açıklar. UserType bir kullanıcıyla bir Azure AD işletmeler arası (B2B) işbirliği kullanıcısı olan konuk =. Bu Konuk kullanıcı genellikle bir iş ortağı kuruluştan ve davet etme dizinde ayrıcalıkları varsayılan olarak sınırlıdır.

Davet eden bir kuruluşun gereksinimlerine bağlı olarak, bir Azure AD B2B işbirliği kullanıcısı hesabı şu durumlardan birinde olabilir:

- Durum 1: Azure AD dış örneğinde bağlantılı ve Konuk kullanıcı davet eden kuruluştan olarak temsil edilir. Bu durumda, B2B kullanıcısı davet edilen kiracıya ait bir Azure AD hesabı kullanarak oturum açtığında. İş ortağı kuruluş Azure AD'ye kullanmıyorsa, Konuk kullanıcının Azure AD'de yine de oluşturulur. Bunlar, davetini ve Azure AD, e-posta adresi doğrular gereksinimleridir. Bu düzenleme, just-ın-time (JIT) kiralama veya "viral" Kiracı olarak da bilinir.

- Durum 2: Bir Microsoft veya başka bir hesap bağlantılı ve konak kuruluşta Konuk kullanıcı olarak temsil edilir. Bu durumda, Konuk kullanıcı bir Microsoft hesabı veya bir sosyal hesap bilgilerinizle oturum açtığı (google.com veya benzer). Davet edilen kullanıcının kimliği, teklif alma sırasında bir Microsoft hesabını davet eden kuruluş dizininizle oluşturulur.

- 3. durum: Konak kuruluşun şirket içi Active Directory'de bağlantılı ve konak kuruluşun Azure ile eşitlenen AD. İş ortağı hesapları eşitleme için Azure AD Connect kullanabilirsiniz UserType ile Azure AD B2B kullanıcıları olarak buluta konuk =. Bkz: [bulut kaynaklarına erişime yerel olarak yönetilen bir iş ortağı hesapları](hybrid-on-premises-to-cloud.md).

- Durum 4: Konak kuruluşun Azure'da bağlantılı AD ile UserType = Konuk ve konak kuruluş tarafından yönetilen kimlik bilgileri.

  ![Dört kullanıcı durumlarını gösteren diyagram](media/user-properties/redemption-diagram.png)


Artık, bir Azure AD B2B işbirliği kullanıcısı Azure AD'de nasıl göründüğüne bakalım.

### <a name="before-invitation-redemption"></a>Önce Davetiyesi kullanımı

Durum 1 ve 2 durumu hesapları konuk kullanıcıların kendi kimlik bilgilerini kullanarak işbirliği yapmak üzere konuk kullanıcıları davet sonucu var. Davet başlangıçta Konuk kullanıcıya gönderildiğinde, dizininizde bir hesap oluşturulur. Bu hesap, Konuk kullanıcının kimlik sağlayıcısı tarafından gerçekleştirilen kimlik doğrulama için kendisiyle ilişkili herhangi bir kimlik bilgisi yok. **Kaynak** özelliği, dizinde Konuk kullanıcı hesabı için **Invited kullanıcı**. 

![Teklif kullanım önce kullanıcı özelliklerini gösteren ekran görüntüsü](media/user-properties/before-redemption.png)

### <a name="after-invitation-redemption"></a>Sonra Davetiyesi kullanımı

Konuk kullanıcı davet kabul ettikten sonra **kaynak** özelliği, Konuk kullanıcının kimlik sağlayıcısına göre güncelleştirilir.

Durum 1 Konuk kullanıcılar için **kaynak** olduğu **dış Azure Active Directory**.

![Teklif süreyle Konuk kullanıcı durumu 1](media/user-properties/after-redemption-state1.png)

Durum 2 Konuk kullanıcılar için **kaynak** olduğu **Microsoft Account**.

![Teklif süreyle Konuk kullanıcı durumu 2](media/user-properties/after-redemption-state2.png)

Durum 3 ve 4 durumu, konuk kullanıcıların **kaynak** özelliği **Azure Active Directory** veya **Windows Server Active Directory**sonraki bölümde açıklandığı gibi.

## <a name="key-properties-of-the-azure-ad-b2b-collaboration-user"></a>Azure AD B2B işbirliği kullanıcısı anahtar özellikleri
### <a name="usertype"></a>UserType
Bu özellik için ana Kiracı Kullanıcı arasındaki ilişkiyi gösterir. Bu özellik, iki değerlere sahip olabilir:
- Üye: Bu değer, bir çalışan konak kuruluş ve bir kullanıcı kuruluşun Bordro gösterir. Örneğin, bu kullanıcı yalnızca iç sitelere erişimine sahip olmasını bekliyor. Bu kullanıcı bir dış ortak çalışan olarak kabul edilmez.

- Konuk: Bu değer, bir dış ortak çalışan, iş ortağı veya müşterinin gibi şirket için dahili olarak kabul olmayan bir kullanıcıyı gösterir. Böyle bir kullanıcı bir CEO'nun iç not alın veya şirket, örneğin avantajların beklenen değil.

  > [!NOTE]
  > UserType nasıl kullanıcı oturum açtığında, kullanıcı vb. dizin rolünü ilgisi yoktur. Bu özellik yalnızca kullanıcının ilişki konak kuruluşa gösterir ve kuruluş tuto vlastnost nelze upravovat bağımlı ilkeleri zorunlu tutmanıza olanak tanır.

### <a name="source"></a>Kaynak
Bu özellik, nasıl kullanıcı oturum açtığında gösterir.

- Davet edilen kullanıcı: Bu kullanıcı, davet edilen ancak henüz bir davet kuponları değil.

- Dış Active Directory: Bu kullanıcı, dış bir kuruluşta bağlantılı ve başka bir kuruluşa ait bir Azure AD hesabını kullanarak kimliğini doğrular. Bu tür bir oturum açma durumu 1'e karşılık gelir.

- Microsoft hesabı: Bu kullanıcı bir Microsoft hesabı bağlantılı ve bir Microsoft hesabını kullanarak kimliğini doğrular. Bu tür bir oturum açma durumu 2'ye karşılık gelir.

- Windows Server Active Directory: Bu kullanıcı bu kuruluşa ait şirket içi Active Directory'den açmıştır. Bu tür bir oturum açma durumu 3'e karşılık gelir.

- Azure Active Directory: Bu kullanıcı, bu kuruluşa ait bir Azure AD hesabı kullanarak kimliğini doğrular. Bu tür bir oturum açma durumu 4'e karşılık gelir.
  > [!NOTE]
  > Kaynak ve UserType bağımsız özellikleridir. Kaynak değeri belirli bir değeri için UserType göstermez.

## <a name="can-azure-ad-b2b-users-be-added-as-members-instead-of-guests"></a>Azure AD B2B kullanıcıları Konukları yerine üye olarak eklenebilir?
Genellikle, bir Azure AD B2B kullanıcısı ve Konuk kullanıcı eşanlamlıdır. Bu nedenle, bir Azure AD B2B işbirliği kullanıcısı UserType sahip bir kullanıcı olarak eklendiğinde varsayılan olarak konuk =. Ancak, bazı durumlarda, iş ortağı kuruluşun bir konak kuruluş ayrıca ait olduğu daha büyük bir kuruluş üyesidir. Bu durumda, konak kuruluş iş ortağı kuruluşta Konukları yerine üyeleri olarak kullanıcılar değerlendirilecek isteyebilirsiniz. Ekleme veya iş ortağı kuruluştan bir kullanıcı bir üye olarak konak kuruluşa davet etmek için Azure AD B2B davet Manager API'lerini kullanın.

## <a name="filter-for-guest-users-in-the-directory"></a>Dizinde Konuk kullanıcılar için filtre

![Konuk kullanıcılar için filtre gösteren ekran görüntüsü](media/user-properties/filter-guest-users.png)

## <a name="convert-usertype"></a>UserType Dönüştür
UserType üyeden Konuk tersi PowerShell kullanarak dönüştürmek mümkündür. Ancak, UserType özelliği, kuruluş kullanıcının ilişkiyi temsil eder. Bu nedenle, bu özellik yalnızca, kullanıcı arasındaki ilişkiyi kuruluş değişiklikler değiştirmelisiniz. Kullanıcı asıl adı (UPN), kullanıcı arasındaki ilişki değişirse değişsin? Kullanıcı aynı kaynaklara erişmeye devam etmelidir? Bir posta kutusu atansın? Atomik bir etkinlik PowerShell kullanarak UserType değiştirme önerilmemektedir. PowerShell kullanarak bu özellik sabit olur durumunda, ayrıca, bu değer üzerinde bir bağımlılık alma önerilmemektedir.

## <a name="remove-guest-user-limitations"></a>Konuk kullanıcı kısıtlamaları Kaldır
Daha yüksek ayrıcalıklar Konuk kullanıcılarınıza sunmak için istediğiniz durumlar olabilir. Herhangi bir role Konuk kullanıcı ekleme ve bile dizinde kullanıcı üyeleri aynı ayrıcalıkları vermek için varsayılan Konuk kullanıcı kısıtlamaları kaldırın.

Şirket dizinde Konuk kullanıcı üyesi kullanıcı olarak aynı izinlere sahip olacak şekilde varsayılan kısıtlamaları devre dışı açmak mümkündür.

![Dış kullanıcılar, kullanıcı ayarları seçeneğini gösteren ekran görüntüsü](media/user-properties/remove-guest-limitations.png)

## <a name="can-i-make-guest-users-visible-in-the-exchange-global-address-list"></a>Konuk kullanıcılar Exchange Genel adres listesinde görünür yapmak?
Evet. Varsayılan olarak, Konuk nesneleri kuruluşunuzun genel adres listesinde görünmez, ancak onları görünür yapmak için Azure Active Directory PowerShell kullanabilirsiniz. Ayrıntılar için bkz **Konuk nesneler genel adres listesinde görünür yapabilirsiniz?** içinde [Office 365 gruplarında konuk erişimini yönetme](https://docs.microsoft.com/office365/admin/create-groups/manage-guest-access-in-groups?redirectSourcePath=%252fen-us%252farticle%252fmanage-guest-access-in-office-365-groups-9de497a9-2f5c-43d6-ae18-767f2e6fe6e0&view=o365-worldwide#faq). 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
* [B2B işbirliği kullanıcı belirteçleri](user-token.md)
* [B2B işbirliği kullanıcı taleplerini eşleme](claims-mapping.md)
