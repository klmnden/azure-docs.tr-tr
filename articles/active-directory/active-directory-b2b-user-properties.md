---
title: Bir Azure Active Directory B2B işbirliği kullanıcının özelliklerini | Microsoft Docs
description: Azure Active Directory B2B işbirliği kullanıcı özellikleri yapılandırılabilir
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/25/2017
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 401c001f897a926de1b7d68403b6945164f3333b
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="properties-of-an-azure-active-directory-b2b-collaboration-user"></a>Bir Azure Active Directory B2B işbirliği kullanıcının özelliklerini

Bir Azure Active Directory (Azure AD) işletmeden işletmeye (B2B) işbirliği UserType sahip bir kullanıcı kullanıcıdır konuk =. Bu Konuk kullanıcı genellikle bir iş ortağı kuruluştan ve davet etme dizininde ayrıcalıkları varsayılan olarak sınırlıdır.

Davet kuruluşunuzun gereksinimlerine bağlı olarak, bir Azure AD B2B işbirliği kullanıcı aşağıdaki hesap durumlardan birinde olabilir:

- Durum 1: Azure AD dış örneğinde bağlantılı ve davet kuruluşunuzdaki Konuk kullanıcı olarak gösterilir. Bu durumda, B2B davet edilen kiracısına ait bir Azure AD hesabı kullanarak oturum açtığında. İş ortağı kuruluş Azure AD kullanmıyorsa, Azure AD'de Konuk kullanıcı yine de oluşturulur. Kullanıcıların davet almak ve Azure AD, e-posta adreslerini doğrular gereksinimleri verilmiştir. Bu düzenlemenin, tam zamanında (JIT) Kiracı veya "viral" Kiracı olarak da bilinir.

- Durum 2: bir Microsoft hesabı bağlantılı ve konak kuruluşunuzdaki Konuk kullanıcı olarak gösterilir. Bu durumda, Konuk kullanıcı bir Microsoft hesabıyla oturum açar. Davet edilen kullanıcının sosyal kimliğini (google.com veya benzer), bir Microsoft hesabı olmayan oluşturulur bir Microsoft hesabı olarak teklif kullanım sırasında.

- Durum 3: konak kuruluşun şirket içi Active Directory'de bağlantılı ve ana bilgisayar kuruluşunuzun Azure ile eşitlenen AD. Bu yayın sırasında el ile bulutta tür kullanıcılar UserType değiştirmek için PowerShell kullanmanız gerekir.

- Durum 4: ana bilgisayar kuruluşunuzun Azure ana bilgisayara bağlı AD UserType ile = Konuk ve ana bilgisayar kuruluş yönetir kimlik bilgileri.

  ![Davet eden'ın baş harflerini görüntüleme](media/active-directory-b2b-user-properties/redemption-diagram.png)


Şimdi bir Azure AD B2B işbirliği kullanıcı durumu 1'de nasıl Azure AD'de göründüğünü görelim.

### <a name="before-invitation-redemption"></a>Davet kullanım önce

![Teklif kullanım önce](media/active-directory-b2b-user-properties/before-redemption.png)

### <a name="after-invitation-redemption"></a>Sonra davet kullanım

![Teklif kullanım sonra](media/active-directory-b2b-user-properties/after-redemption.png)

## <a name="key-properties-of-the-azure-ad-b2b-collaboration-user"></a>Azure AD B2B işbirliği kullanıcı temel özellikleri
### <a name="usertype"></a>UserType
Bu özellik, konak kiralama kullanıcıya arasındaki ilişkiyi gösterir. Bu özellik, iki değerlere sahip olabilir:
- Üye: Bu değer ana kuruluş ve kuruluşun Bordro bir kullanıcı bir çalışan gösterir. Örneğin, bu kullanıcı yalnızca iç sitelere erişimi olmasını bekler. Bu kullanıcı dış bir ortak çalışanı kabul edilir değil.

- Konuk: Bu değer bir dış ortak çalışanı, iş ortağı, müşteri veya benzer kullanıcı gibi şirket için dahili olarak kabul olmayan bir kullanıcıyı gösterir. Böyle bir kullanıcı bir CEO'nun iç not almak veya şirket, örneğin avantajların yanı sıra beklenen olmayacaktır.

  > [!NOTE]
  > UserType nasıl kullanıcı oturum açtığında, kullanıcı vb. dizin rolü ilişki yok. Bu özellik yalnızca kullanıcının ilişki konak kuruluşa gösterir ve kuruluşunuzun bu özellikte bağımlı ilkeleri zorunlu tutmanıza olanak sağlar.

### <a name="source"></a>Kaynak
Bu özellik, kullanıcı nasıl oturum açtığında gösterir.

- Kullanıcı Davet: Bu kullanıcı davet edilen, ancak henüz bir davet kullanılan değil.

- Dış Active Directory: Bu kullanıcı bir dış kuruluşta bağlantılı ve başka bir kuruluşa ait bir Azure AD hesabı kullanarak kimliğini doğrular. Bu tür bir oturum açma durumu 1'e karşılık gelir.

- Microsoft hesabı: Bu kullanıcı bir Microsoft hesabı bağlantılı ve bir Microsoft hesabı kullanarak kimliğini doğrular. Bu tür bir oturum açma durumu 2'ye karşılık gelir.

- Windows Server Active Directory: Bu kullanıcı bu kuruluşa ait şirket içi Active Directory'den oturum. Bu tür bir oturum açma durumu 3'e karşılık gelir.

- Azure Active Directory: Bu bir kuruluşa ait bir Azure AD hesabı kullanarak bu kullanıcının kimliğini doğrular. Bu tür bir oturum açma durumu 4'e karşılık gelir.
  > [!NOTE]
  > Kaynak ve UserType bağımsız özelliklerdir. Kaynak değerini belirli bir değeri için UserType göstermez.

## <a name="can-azure-ad-b2b-users-be-added-as-members-instead-of-guests"></a>Azure AD B2B kullanıcılar konuklar yerine üye olarak eklenebilir mi?
Genellikle, bir Azure AD B2B kullanıcı ve Konuk kullanıcı eşanlamlıdır. Bu nedenle, bir Azure AD B2B işbirliği kullanıcı UserType olan bir kullanıcı olarak eklenen varsayılan olarak konuk =. Ancak, bazı durumlarda, ortağı kuruluştaki bir ana bilgisayar kuruluş aynı zamanda ait olduğu daha büyük bir kuruluşta üyesidir. Bu durumda, ana bilgisayar kuruluş konuklar yerine üye olarak ortağı kuruluştaki kullanıcılar kabul isteyebilirsiniz. Eklemek veya iş ortağı kuruluştan bir kullanıcı bir üye olarak ana bilgisayar kuruluşa davet için Azure AD B2B davet Manager API'leri kullanın.

## <a name="filter-for-guest-users-in-the-directory"></a>Konuk kullanıcılar dizininde Filtrele

![Filtre Konuk kullanıcılar](media/active-directory-b2b-user-properties/filter-guest-users.png)

## <a name="convert-usertype"></a>UserType Dönüştür
Şu anda UserType üyeden Konuk ve tam tersini PowerShell kullanarak dönüştürmek kullanıcıların mümkündür. Ancak, kuruluşa kullanıcının ilişkiyi göstermek için UserType özelliği gerekiyor. Yalnızca kuruluş kullanıcıya ilişkisi değişirse, bu nedenle, bu özelliğin değerini değiştirmeniz gerekir. Kullanıcı ilişkisi değişirse, kullanıcı asıl adı (UPN) değiştirmeniz gerekir gibi sorunlar, ilgilenilmesi gerekir? Kullanıcı aynı kaynaklara erişimi olacak şekilde devam etmelidir? Bir posta kutusu atanması gerekir? Bu nedenle, atomik aktivite olarak PowerShell kullanarak UserType değiştirilmesi önerilmez. PowerShell kullanarak bu özellik sabit hale gelmesi durumu, ayrıca, bu değeri bir bağımlılık alma önermiyoruz.

## <a name="remove-guest-user-limitations"></a>Konuk kullanıcı kısıtlama Kaldır
Konuk kullanıcılar yüksek ayrıcalıkları vermek istediğiniz durumlar olabilir. Konuk kullanıcı hiçbir role ekleyin ve hatta dizininde bir kullanıcı üyeleri aynı ayrıcalıkları vermek için varsayılan Konuk kullanıcı kısıtlamaları kaldırın.

Şirket dizinde Konuk kullanıcı aynı izinlere sahip bir üye kullanıcı verilen için varsayılan Konuk kullanıcı kısıtlamaları devre dışı bırakmak mümkündür.

![Konuk kullanıcı kısıtlama Kaldır](media/active-directory-b2b-user-properties/remove-guest-limitations.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD B2B işbirliği nedir?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [B2B işbirliği kullanıcı belirteçleri](active-directory-b2b-user-token.md)
* [B2B işbirliği kullanıcı taleplerini eşleme](active-directory-b2b-claims-mapping.md)
