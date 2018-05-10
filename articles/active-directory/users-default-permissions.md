---
title: Varsayılan kullanıcı izinlerini Azure Active Directory'de karşılaştırma | Microsoft Docs
description: Üye, konuk, uygulama sahibi ve Grup sahibi izinleri karşılaştırma
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 01/29/2018
ms.author: curtand
ms.reviewer: vincesm
ms.openlocfilehash: a9bf9748de5f390f95b8b672e0cf77afa5c49581
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="default-user-permissions-in-azure-active-directory"></a>Azure Active Directory'de varsayılan kullanıcı izinleri

Azure Active Directory (Azure AD), tüm kullanıcıların varsayılan izinleri verilir. Bir kullanıcının erişimi kullanıcı türünü oluşur kendi [rol üyeliklerini](https://docs.microsoft.com/azure/active-directory/active-directory-users-assign-role-azure-portal)ve bunların nesnelerin mülkiyetini. Bu makalede, bu varsayılan izinleri açıklar ve üye ve Konuk kullanıcı Varsayılanları karşılaştırması içerir.

## <a name="member-and-guest-users"></a>Üye ve Konuk kullanıcılar
Alınan varsayılan izinler kümesini kullanıcı Kiracı (üye kullanıcı) yerel bir üyesi ise veya kullanıcı B2B işbirliği Konuk (Konuk kullanıcı) ise bağlıdır. B2B işbirliği hakkında daha fazla bilgi için bkz: [Azure AD B2B işbirliği nedir?](active-directory-b2b-what-is-azure-ad-b2b.md) Konuk kullanıcılar hakkında daha fazla bilgi için). 
* Üye kullanıcılar uygulamalarını kaydetmek, kendi profili fotoğraf ve cep telefonu numarası yönetmek, kullanıcıların kendi parolalarını değiştirmek ve B2B konuklar davet. Ayrıca, kullanıcılar (birkaç istisna dışında ile) tüm dizin bilgilerinin okuyabilir. 
* Azure AD B2B Konuk kullanıcılar sınırlı dizin izinlere. Örneğin, Konuk kullanıcılar kendi profil bilgilerini ötesinde Kiracı bilgilerinden öğesine göz atamıyor. Ancak, bir Konuk kullanıcı, kullanıcı asıl adı veya objectID sağlayarak başka bir kullanıcı hakkında bilgi alabilirsiniz. Konuk, gruplar ve uygulamalar gibi diğer Kiracı nesneler hakkındaki tüm bilgileri görüntüleyemezsiniz.

Konuklar için varsayılan izinler, varsayılan olarak kısıtlayıcıdır. Konuklar tam okuma ve yazma izinleri rolünde bulunan verebileceğiniz yönetici rolleri eklenebilir. Bir ek kısıtlama kullanılabilir diğer konuklar davet etmek konuklar yeteneği yoktur. Ayarı **konuklar davet edebildiğiniz** için **Hayır** diğer konuklar davet konuklar engeller. Bkz: [temsilci B2B işbirliğinin davetleri](active-directory-b2b-delegate-invitations.md) öğrenmek için nasıl. Konuk kullanıcılar varsayılan olarak üyesi kullanıcı olarak aynı izinleri vermek için ayarlanmış **konuk kullanıcılara izinler sınırlı** için **Hayır**. Bu ayarı tüm üye yönetici rollerine eklenecek konuklar izin vermek için varsayılan olarak da konuk kullanıcılar için kullanıcı izinlerini verir.

## <a name="compare-member-and-guest-default-permissions"></a>Üye ve Konuk varsayılan izinleri karşılaştırma

**Alan** | **Üye kullanıcı izinleri** | **Konuk kullanıcı izinleri**
------------ | --------- | ----------
Kullanıcıları ve kişileri | Kullanıcıları ve kişileri ortak tüm özellikleri oku<br>Konuk davet et<br>Kendi parolasını değiştirme<br>Kendi cep telefonu numarası yönetme<br>Kendi photo yönetme<br>Kendi yenileme belirteçleri geçersiz kıl | Kendi özellikleri okuma<br>Görünen ad, e-posta, oturum açma adı, fotoğraf, kullanıcı asıl adı ve kullanıcı türü özelliklerinin diğer kullanıcıları ve kişileri okuma<br>Kendi parolasını değiştirme
Gruplar   | Güvenlik grupları oluşturma<br>Office 365 grupları oluşturma<br>Grupların tüm özellikleri oku<br>Gizli olmayan grup üyeliklerini okuma<br>Birleştirilmiş grup okuma gizli Office 365 grup üyeliği<br>Özellikler, sahipliği ve sahip olduğu grupları üyeliğini yönetme<br>Konuklar ait gruplarına Ekle<br>Dinamik üyelik ayarlarını yönet<br>Ait gruplarını Sil<br>Office 365 grupları ait geri yükleme | Grupların tüm özellikleri oku<br>Gizli olmayan grup üyeliklerini okuma<br>Gizli Office 365 grup üyeliği birleştirilmiş gruplar için okuma<br>Ait gruplarını yönetme<br>Konuklar (izin veriyorsa) ait gruplarına Ekle<br>Ait gruplarını Sil<br>Office 365 grupları ait geri yükleme           
Uygulamalar | Kaydetme (Oluştur) yeni bir uygulama<br>Okuma özelliklerini kayıtlı ve kurumsal uygulamalar<br>Uygulama özellikleri, atamalarını ve sahip olduğu uygulamalar için kimlik bilgilerini yönetme<br>Oluşturma veya kullanıcının uygulama parolasını silme<br>Uygulamaları ait Sil<br>Uygulamaları ait geri yükleme | Okuma özelliklerini kayıtlı ve kurumsal uygulamalar<br>Uygulama özellikleri, atamalarını ve sahip olduğu uygulamalar için kimlik bilgilerini yönetme<br>Uygulamaları ait Sil<br>Uygulamaları ait geri yükleme
Cihazlar | Aygıtların tüm özellikleri oku<br>Ait cihazlar tüm özelliklerini yönetme<br> | İzin yok<br>Delete ait cihazlar<br>
Dizin | Tüm şirket bilgilerini okuma<br>Tüm etki alanları okuma<br>Tüm iş ortağı sözleşmelerini oku | Görünen ad okuma ve etki alanı doğrulandı
Rolleri ve kapsamlar | Tüm yönetim rolleri ve üyeliklerini okuma<br>Tüm özellikleri ve idari birim üyeliğini okuyun | İzin yok              
Abonelikler | Tüm abonelikleri okuma<br>Hizmet planı üyesini etkinleştir | İzin yok
İlkeler | İlkelerin tüm özellikleri oku<br>Ait ilkesinin tüm özelliklerini yönetme | İzin yok

## <a name="to-restrict-the-default-permissions-for-member-users"></a>Üye kullanıcıların varsayılan izinlerini kısıtlamak için


Kullanıcıları aşağıdaki yollarla kısıtlanabilir üye izinlerini varsayılan. Daha fazla bilgi için bkz: [uygulamalar, izinler ve Azure Active Directory'de izin](active-directory-apps-permissions-consent.md).

İzin | Ayar açıklaması
---------- | ------------
Güvenlik grupları oluşturma olanağı | Bu seçenek Hayır olarak ayarlanırsa, kullanıcılar güvenlik grupları oluşturmasını engeller. Genel Yöneticiler ve kullanıcı hesap yöneticileri güvenlik grupları oluşturabilirsiniz. Bkz: [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](active-directory-accessmanagement-groups-settings-cmdlets.md) öğrenmek için nasıl.
Office 365 grupları oluşturma olanağı | Bu seçenek Hayır olarak ayarlanırsa, kullanıcıların Office 365 grupları oluşturmasını engeller. Bu seçenek bazı ayarı Office 365 grupları oluşturmak için kullanıcıları seçin kümesini sağlar. Genel Yöneticiler ve kullanıcı hesap yöneticileri Office 365 grupları oluşturabilmek için olmaya devam edecektir. Bkz: [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](active-directory-accessmanagement-groups-settings-cmdlets.md) öğrenmek için nasıl.
Uygulama onayı yeteneği | Bu seçenek Hayır olarak ayarlanırsa, kullanıcılar uygulamaları onaylıyorsunuz engeller. Genel Yöneticiler uygulamalara onay mümkün olacaktır. Daha fazla bilgi için bkz: [uygulamalar, izinler ve Azure Active Directory'de izin](active-directory-apps-permissions-consent.md).
Galeri uygulama erişim Paneli'ne ekleme yeteneği | Bu seçenek Hayır olarak ayarlanırsa, kullanıcılar kendi erişim Paneli'ne galeri uygulamaları eklemesini engeller.
Kaydedilecek özelliği (Oluştur) uygulamaları | Bu seçenek Hayır olarak ayarlanırsa, yönetici olmayanlar uygulamaları oluşturmanızı engeller. Genel Yöneticiler, uygulamaları oluşturabilmek için olmaya devam edecektir. Daha fazla bilgi için bkz: [uygulamalar, izinler ve Azure Active Directory'de izin](active-directory-apps-permissions-consent.md).
Konuklar, Yöneticiler ve kullanıcılar Konuk davet eden rolündeki davet edebilirsiniz | Bu seçenek Hayır olarak ayarlanırsa, tüm kullanıcıların davet konuklar engeller. Üye kullanıcılar için varsayılan izinler yapılandırma konusuna bakın. Daha fazla bilgi için bkz: [uygulamalar, izinler ve Azure Active Directory'de izin](active-directory-apps-permissions-consent.md).
Üyeleri konuklar davet edebilirsiniz | Bunun için hiçbir ayar davet konuklar kullanıcılardan engeller. Genel Yöneticiler, kullanıcı hesap yöneticileri ve Konuk Inviters hala konuklar davet kuramaz. Daha fazla bilgi için bkz: [uygulamalar, izinler ve Azure Active Directory'de izin](active-directory-apps-permissions-consent.md).
Azure AD yönetim portalına erişimi sınırlayın | Bu seçenek Hayır olarak ayarlanırsa, kullanıcıların Azure Active Directory portalında erişmesini engeller.
Diğer kullanıcıların okuma yeteneği | Bu ayar yalnızca PowerShell içinde kullanılabilir. Bu $false olarak ayarlama, yönetici olmayan tüm kullanıcı bilgileri dizinden okumasını önler. Bu okuma engellemez Exchange Online gibi diğer Microsoft hizmetlerinde kullanıcı bilgileri. Bu ayar, özel durumlar ve bu $false önerilmez ayarı için tasarlanmıştır.

## <a name="object-ownership"></a>Nesne sahipliği

### <a name="application-registration-owner-permissions"></a>Uygulama kayıt sahibi izinleri
Bir kullanıcı bir uygulama kaydolduğunda, bunlar otomatik olarak uygulamanın sahibi olarak eklenir. Bir sahip olarak uygulama meta verileri yönetebilir, uygulama adını ve izinleri gibi ister. Bunlar, ayrıca uygulama SSO yapılandırması ve kullanıcı atamaları gibi kiracıya özgü yapılandırmasını yönetebilirsiniz. Bir sahip da ekleyebilir veya diğer sahipleri kaldırabilirsiniz. Genel Yöneticiler sahipleri yalnızca oldukları uygulamaları yönetebilirsiniz. Bir uygulama kayıt sahibi atamak için bkz: [Azure Active Directory Uygulama kaydı](active-directory-app-registration.md).

<!-- ### Enterprise application owner permissions

When a user adds a new enterprise application, they are automatically added as an owner for the tenant-specific configuration of the application. As an owner, they can manage the tenant-specific configuration of the application, such as the SSO configuration, provisioning, and user assignments. An owner can also add or remove other owners. Unlike Global Administrators, owners can manage only the applications they own. <!--To assign an enterprise application owner, see *Assigning Owners for an Application*.-->

### <a name="group-owner-permissions"></a>Grup sahibi izinleri

Bir kullanıcı bir grup oluşturduğunda, bunlar otomatik olarak o grubun sahibi olarak eklenir. Bir sahibi olarak, bunlar grubunun adı gibi özellikleri yönetmenize yanı üyelik yönetin. Bir sahip da ekleyebilir veya diğer sahipleri kaldırabilirsiniz. Genel Yöneticiler ve kullanıcı hesap yöneticileri aksine, sahipleri yalnızca sahip olduğu grupları yönetebilirsiniz. Grup sahibi atamak için bkz: [bir grubun sahiplerini yönetme](active-directory-accessmanagement-managing-group-owners.md).

## <a name="next-steps"></a>Sonraki adımlar

* Bir Azure aboneliğine yönelik olarak yöneticileri değiştirme hakkında daha fazla bilgi için bkz. [Azure yönetici rollerini ekleme veya değiştirme](../billing-add-change-azure-subscription-administrator.md)
* Microsoft Azure'da kaynak erişiminin nasıl denetlendiği konusunda daha fazla bilgi için bkz. [Azure'da kaynak erişimini anlama](../role-based-access-control/rbac-and-directory-admin-roles.md)
* Azure Active Directory Azure aboneliğinize ilişkilendirilme şekli ile ilgili daha fazla bilgi için bkz: [Azure aboneliklerinin Azure Active Directory ile ilişkili](active-directory-how-subscriptions-associated-directory.md)
* [Kullanıcıları yönetme](active-directory-create-users.md)
