---
title: Bir Azure Active Directory B2B işbirliği kullanıcısı özelliklerini | Microsoft Docs
description: Azure Active Directory B2B işbirliği kullanıcı özellikleri yapılandırılabilir
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 5f999a17cd375a3338aa936e2f405c36f6021ebc
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "45984824"
---
# <a name="properties-of-an-azure-active-directory-b2b-collaboration-user"></a>Bir Azure Active Directory B2B işbirliği kullanıcısı özellikleri

UserType bir kullanıcıyla bir Azure Active Directory (Azure AD) işletmeler arası (B2B) işbirliği kullanıcısı olan konuk =. Bu Konuk kullanıcı genellikle bir iş ortağı kuruluştan ve davet etme dizinde ayrıcalıkları varsayılan olarak sınırlıdır.

Davet eden bir kuruluşun gereksinimlerine bağlı olarak, bir Azure AD B2B işbirliği kullanıcısı hesabı şu durumlardan birinde olabilir:

- Durum 1: Azure AD dış örneğinde bağlantılı ve Konuk kullanıcı davet eden kuruluştan olarak temsil edilir. Bu durumda, B2B kullanıcısı davet edilen kiracıya ait bir Azure AD hesabı kullanarak oturum açtığında. İş ortağı kuruluş Azure AD'ye kullanmıyorsa, Konuk kullanıcının Azure AD'de yine de oluşturulur. Bunlar, davetini ve Azure AD, e-posta adresi doğrular gereksinimleridir. Bu düzenleme, just-ın-time (JIT) kiralama veya "viral" Kiracı olarak da bilinir.

- Durum 2: bağlantılı bir Microsoft hesabı ve konak kuruluşta Konuk kullanıcı olarak temsil edilir. Bu durumda, Konuk kullanıcı bir Microsoft hesabıyla oturum açar. Davet edilen kullanıcının sosyal kimlik (google.com veya benzer), bir Microsoft hesabı değil oluşturulur bir Microsoft hesabı olarak teklif kullanım sırasında.

- Durum 3: konak kuruluşun şirket içi Active Directory'de bağlantılı ve konak kuruluşun Azure ile eşitlenen AD. Bu yayın sırasında el ile bulutta gibi kullanıcı UserType değiştirmek için PowerShell kullanmanız gerekir.

- Durum 4: konak kuruluşun Azure'da bağlantılı AD ile UserType = Konuk ve konak kuruluş tarafından yönetilen kimlik bilgileri.

  ![Davet eden'ın baş görüntüleme](media/user-properties/redemption-diagram.png)


Artık, bir Azure AD B2B işbirliği kullanıcısı durum 1'deki Azure AD'de nasıl göründüğüne bakalım.

### <a name="before-invitation-redemption"></a>Önce Davetiyesi kullanımı

![Teklif kullanım önce](media/user-properties/before-redemption.png)

### <a name="after-invitation-redemption"></a>Sonra Davetiyesi kullanımı

![Teklif kullanım sonra](media/user-properties/after-redemption.png)

## <a name="key-properties-of-the-azure-ad-b2b-collaboration-user"></a>Azure AD B2B işbirliği kullanıcısı anahtar özellikleri
### <a name="usertype"></a>UserType
Bu özellik için ana Kiracı Kullanıcı arasındaki ilişkiyi gösterir. Bu özellik, iki değerlere sahip olabilir:
- Üye: Bu değer, bir çalışan konak kuruluş ve bir kullanıcı kuruluşun Bordro gösterir. Örneğin, bu kullanıcı yalnızca iç sitelere erişimine sahip olmasını bekliyor. Bu kullanıcı, bir dış ortak çalışan değerlendirilmeyecektir.

- Konuk: Gibi bir dış ortak çalışan, iş ortağı, müşteri veya benzer bir kullanıcı şirket için dahili olarak kabul olmayan bir kullanıcı bu değeri gösterir. Böyle bir kullanıcı bir CEO'nun iç not alın veya şirket, örneğin avantajların beklenen mıydı.

  > [!NOTE]
  > UserType nasıl kullanıcı oturum açtığında, kullanıcı vb. dizin rolünü ilgisi yoktur. Bu özellik yalnızca kullanıcının ilişki konak kuruluşa gösterir ve kuruluş tuto vlastnost nelze upravovat bağımlı ilkeleri zorunlu tutmanıza olanak tanır.

### <a name="source"></a>Kaynak
Bu özellik, nasıl kullanıcı oturum açtığında gösterir.

- Kullanıcı Davet: Bu kullanıcı davet edildi, ancak henüz bir davet kuponları değil.

- Dış Active Directory: Bu kullanıcı, dış bir kuruluşta bağlantılı ve başka bir kuruluşa ait bir Azure AD hesabını kullanarak kimliğini doğrular. Bu tür bir oturum açma durumu 1'e karşılık gelir.

- Microsoft hesabı: Bu kullanıcı bir Microsoft hesabı bağlantılı ve bir Microsoft hesabını kullanarak kimliğini doğrular. Bu tür bir oturum açma durumu 2'ye karşılık gelir.

- Windows Server Active Directory: Bu kullanıcı bu kuruluşa ait şirket içi Active Directory'den açmıştır. Bu tür bir oturum açma durumu 3'e karşılık gelir.

- Azure Active Directory: Bu kuruluşa ait bir Azure AD hesabını kullanarak bu kullanıcının kimliğini doğrular. Bu tür bir oturum açma durumu 4'e karşılık gelir.
  > [!NOTE]
  > Kaynak ve UserType bağımsız özellikleridir. Kaynak değeri belirli bir değeri için UserType göstermez.

## <a name="can-azure-ad-b2b-users-be-added-as-members-instead-of-guests"></a>Azure AD B2B kullanıcıları Konukları yerine üye olarak eklenebilir?
Genellikle, bir Azure AD B2B kullanıcısı ve Konuk kullanıcı eşanlamlıdır. Bu nedenle, bir Azure AD B2B işbirliği kullanıcısı UserType sahip bir kullanıcı olarak eklendiğinde varsayılan olarak konuk =. Ancak, bazı durumlarda, iş ortağı kuruluşun bir konak kuruluş ayrıca ait olduğu daha büyük bir kuruluş üyesidir. Bu durumda, konak kuruluş iş ortağı kuruluşta Konukları yerine üyeleri olarak kullanıcılar değerlendirilecek isteyebilirsiniz. Ekleme veya iş ortağı kuruluştan bir kullanıcı bir üye olarak konak kuruluşa davet etmek için Azure AD B2B davet Manager API'lerini kullanın.

## <a name="filter-for-guest-users-in-the-directory"></a>Dizinde Konuk kullanıcılar için filtre

![Konuk kullanıcıları Filtrele](media/user-properties/filter-guest-users.png)

## <a name="convert-usertype"></a>UserType Dönüştür
Şu anda kullanıcı UserType üyeden Konuk tersi PowerShell kullanarak dönüştürmek mümkündür. Ancak, kullanıcının ilişki için kuruluşu temsil etmek için UserType özelliği gerekiyor. Kuruluşun kullanıcı arasındaki ilişki değişirse, bu nedenle, bu özelliğin değerini değiştirmeniz gerekir. Kullanıcı asıl adı (UPN) değiştirmeniz gerekir gibi sorunlar için kullanıcı arasındaki ilişki değişirse, ilgilenilmesi gerekir? Kullanıcı aynı kaynaklara erişmeye devam etmelidir? Bir posta kutusu atansın? Bu nedenle, atomik aktivite olarak PowerShell kullanarak UserType değiştirilmesi önerilmez. PowerShell kullanarak bu özellik sabit olur durumunda, ek olarak, bu değer üzerinde bir bağımlılık alma önermiyoruz.

## <a name="remove-guest-user-limitations"></a>Konuk kullanıcı kısıtlamaları Kaldır
Daha yüksek ayrıcalıklar Konuk kullanıcılarınıza sunmak için istediğiniz durumlar olabilir. Herhangi bir role Konuk kullanıcı ekleme ve bile dizinde kullanıcı üyeleri aynı ayrıcalıkları vermek için varsayılan Konuk kullanıcı kısıtlamaları kaldırın.

Şirket dizinde Konuk kullanıcı üyesi kullanıcı olarak aynı izinleri verilir için varsayılan Konuk kullanıcı kısıtlamaları devre dışı bırakmak mümkündür.

![Konuk kullanıcı kısıtlamaları Kaldır](media/user-properties/remove-guest-limitations.png)

## <a name="can-i-make-guest-users-visible-in-the-exchange-global-address-list"></a>Konuk kullanıcılar Exchange Genel adres listesinde görünür yapmak?
Evet. Varsayılan olarak, Konuk nesneleri kuruluşunuzun genel adres listesinde görünür değildir, ancak onları görünür yapmak için Azure Active Directory PowerShell kullanabilirsiniz. Ayrıntılar için bkz **Konuk nesneler genel adres listesinde görünür yapabilirsiniz?** içinde [Konuk erişimi Office 365 gruplarında](https://support.office.com/article/guest-access-in-office-365-groups-bfc7a840-868f-4fd6-a390-f347bf51aff6#PickTab=FAQ). 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
* [B2B işbirliği kullanıcı belirteçleri](user-token.md)
* [B2B işbirliği kullanıcı taleplerini eşleme](claims-mapping.md)
