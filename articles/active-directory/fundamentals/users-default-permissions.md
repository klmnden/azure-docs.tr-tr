---
title: Varsayılan kullanıcı izinleri Azure Active Directory nelerdir? | Microsoft Docs
description: Azure Active Directory'de bulunan farklı kullanıcı izinleri hakkında bilgi edinin.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: lizross
ms.reviewer: vincesm
custom: it-pro
ms.openlocfilehash: 9a9bd33dce4b2be9e15df2745dd25338cdb42702
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45730488"
---
# <a name="what-are-the-default-user-permissions-in-azure-active-directory"></a>Varsayılan kullanıcı izinleri Azure Active Directory nelerdir?

Azure Active Directory'de (Azure AD) tüm kullanıcılara varsayılan olarak belirli izinler verilir. Bir kullanıcının erişim bilgileri kullanıcı türünü, [rol üyeliklerini](https://docs.microsoft.com/azure/active-directory/active-directory-users-assign-role-azure-portal) ve bağımsız nesne sahiplik verilerini kapsar. Bu makalede bu varsayılan izinler ve açıklanmakta ve üye ile konuk varsayılan değerleri karşılaştırılmaktadır.

## <a name="member-and-guest-users"></a>Üyeler ve konuk kullanıcılar
Alınan varsayılan izin kümesi, kullanıcının kiracının yerel üyesi (üye kullanıcı) veya B2B işbirliği konuğu (konuk kullanıcı) olma durumuna göre değişir. Kullanıcılar ve B2B işbirliği hakkında daha fazla bilgi için bkz. [Azure AD B2B İşbirliği nedir?](../b2b/what-is-b2b.md) 
* Üye kullanıcılar uygulama kaydedebilir, kendi profil fotoğraflarını ve cep telefonu numaralarını yönetebilir, kendi parolalarını değiştirebilir ve B2B konuklarını davet edebilirler. Ayrıca kullanıcılar dizindeki tüm bilgileri okuyabilir (birkaç özel durum haricinde). 
* Azure AD B2B konuk kullanıcıları sınırlı dizin izinlerine sahiptir. Örneğin konuk kullanıcılar kendi profil bilgileri dışındaki kiracı bilgilerine ulaşamaz. Ancak konuk kullanıcılar Kullanıcı Asıl Adı veya nesne kimliği kullanarak diğer kullanıcılar hakkında bilgi alabilir. Konuklar, gruplar ve uygulamalar gibi diğer kiracı nesneleri hakkındaki bilgileri görüntüleyemez.

Konuklar için varsayılan izinler varsayılan olarak kısıtlanmıştır. Konuklar yönetici rollerine eklenerek rol kapsamındaki tüm yazma ve okuma izinlerine erişmeleri sağlanabilir. Konuklara uygulanabilen diğer bir kısıtlama da başka konukları davet etme özelliğidir. **Konuklar davet edebilir** ayarını **Hayır** olarak belirlerseniz konuklar başka konuk davet edemez. Nasıl yapılacağını öğrenmek için bkz. [B2B işbirliği için davetiye temsilcisi seçme](../b2b/delegate-invitations.md). Konuk kullanıcılara varsayılan olarak üye kullanıcılarla aynı izinleri vermek için **Konuk kullanıcıların izinleri sınırlıdır** ayarını **Hayır** olarak belirleyin. Bu ayar varsayılan olarak konuk kullanıcılara tüm üye kullanıcı izinlerini vermenin yanı sıra konuklara yönetici rollerinin verilmesini sağlar.

## <a name="compare-member-and-guest-default-permissions"></a>Üye ve konuk varsayılan izinlerini karşılaştırma

**Alan** | **Üye kullanıcı izinleri** | **Konuk kullanıcı izinleri**
------------ | --------- | ----------
Kullanıcılar ve kişiler | Kullanıcıların ve kişilerin tüm genel özelliklerini okuma<br>Konuk davet etme<br>Kendi parolasını değiştirme<br>Kendi cep telefonu numarasını yönetme<br>Kendi fotoğrafını yönetme<br>Kendi yenileme belirteçlerini geçersiz kılma | Kendi özelliklerini okuma<br>Diğer kullanıcıların ve kişilerin görünen ad, e-posta, oturum açma adı, fotoğraf, kullanıcı asıl adı ve kullanıcı türü özelliklerini okuma<br>Kendi parolasını değiştirme
Gruplar | Güvenlik grubu oluşturma<br>Office 365 grubu oluşturma<br>Grupların tüm özelliklerini okuma<br>Gizli olmayan grup üyeliklerini okuma<br>Birleştirilen grup için gizli Office 365 grup üyeliklerini okuma<br>Sahip olunan grupların özelliklerini, sahipliğini ve üyeliğini yönetme<br>Sahip olunan gruplara konuk ekleme<br>Dinamik üyelik ayarlarını yönetme<br>Sahip olunan grupları silme<br>Sahip olunan Office 365 gruplarını geri yükleme | Grupların tüm özelliklerini okuma<br>Gizli olmayan grup üyeliklerini okuma<br>Birleştirilen gruplar için gizli Office 365 grup üyeliklerini okuma<br>Sahip olunan grupları yönetme<br>Sahip olunan gruplara konuk ekleme (izin veriliyorsa)<br>Sahip olunan grupları silme<br>Sahip olunan Office 365 gruplarını geri yükleme 
Uygulamalar | Yeni uygulama kaydetme (oluşturma)<br>Kayıtlı ve kurumsal uygulamaların özelliklerini okuma<br>Sahip olunan uygulamaların uygulama özelliklerini, atamalarını ve kimlik bilgilerini yönetme<br>Kullanıcı için uygulama parolasını oluşturma veya silme<br>Sahip olunan uygulamaları silme<br>Sahip olunan uygulamaları geri yükleme | Kayıtlı ve kurumsal uygulamaların özelliklerini okuma<br>Sahip olunan uygulamaların uygulama özelliklerini, atamalarını ve kimlik bilgilerini yönetme<br>Sahip olunan uygulamaları silme<br>Sahip olunan uygulamaları geri yükleme
Cihazlar | Cihazların tüm özelliklerini okuma<br>Sahip olunan cihazların tüm özelliklerini yönetme<br> | İzin yok<br>Sahip olunan cihazları silme<br>
Dizin | Tüm şirket bilgilerini okuma<br>Tüm etki alanlarını okuma<br>Tüm iş ortağı sözleşmelerini okuma | Görünen ad ve onaylı etki alanlarını okuma
Roller ve Kapsamlar | Tüm yönetim rollerini ve üyelikleri okuma<br>Tüm yönetim birimlerinin özelliklerini ve üyeliklerini okuma | İzin yok 
Abonelikler | Tüm abonelikleri okuma<br>Hizmet Planı Üyesini etkinleştirme | İzin yok
İlkeler | İlkelerin tüm özelliklerini okuma<br>Sahip olunan ilkelerin tüm özelliklerini yönetme | İzin yok

## <a name="to-restrict-the-default-permissions-for-member-users"></a>Üye kullanıcıların varsayılan izinlerini kısıtlama

Üye kullanıcıların varsayılan izinleri aşağıdaki yöntemlerle kısıtlanabilir.

İzin | Ayar açıklaması
---------- | ------------
Güvenlik grubu oluşturma olanağı | Bu seçenek Hayır olarak ayarlanırsa kullanıcılar güvenlik grubu oluşturamaz. Genel Yöneticiler ve Kullanıcı Hesabı Yöneticileri, güvenlik grubu oluşturmaya devam edebilir. Nasıl yapılacağını öğrenmek için bkz. [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](../users-groups-roles/groups-settings-cmdlets.md).
Office 365 grubu oluşturma olanağı | Bu seçenek Hayır olarak ayarlanırsa kullanıcılar Office 365 grubu oluşturamaz. Bu seçeneğin Bazı olarak ayarlanması belirli bir kullanıcı kümesinin Office 365 grubu oluşturmasını sağlar. Genel Yöneticiler ve Kullanıcı Hesabı Yöneticileri, Office 365 grubu oluşturma iznine sahip olmaya devam eder. Nasıl yapılacağını öğrenmek için bkz. [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](../users-groups-roles/groups-settings-cmdlets.md).
Azure AD yönetim portalına erişimi sınırlayın | Bu seçenek Hayır olarak ayarlanırsa, kullanıcılar Azure Active Directory erişmesini engeller.
Diğer kullanıcıları okuma olanağı | Bu ayar yalnızca PowerShell ile kullanılabilir. Bu seçenek $false olarak ayarlanırsa yönetici olmayan kullanıcıların dizindeki kullanıcı bilgilerini okuması engellenir. Bu ayar Exchange Online gibi diğer Microsoft hizmetlerindeki kullanıcı bilgilerinin okunmasını önlemez. Bu ayar özel durumlar için tasarlanmıştır ve $false olarak ayarlanması önerilmez.

## <a name="object-ownership"></a>Nesne sahipliği

### <a name="application-registration-owner-permissions"></a>Uygulama kaydı sahibi izinleri
Uygulamayı kaydeden kullanıcılar otomatik olarak uygulamanın sahibi olarak eklenir. Bu uygulama sahipleri ad ve uygulama isteği izinleri gibi uygulama meta verilerini yönetebilir. Bu kullanıcılar ayrıca SSO yapılandırması ve kullanıcı atamaları gibi kiracıya özgü uygulama yapılandırmalarını da yönetebilir. Sahipler başka kullanıcıları sahip olarak ekleyebilir veya kaldırabilir. Genel Yöneticilerden farklı olarak sahipler yalnızca kendilerine ait uygulamaları yönetebilir.

<!-- ### Enterprise application owner permissions

When a user adds a new enterprise application, they are automatically added as an owner for the tenant-specific configuration of the application. As an owner, they can manage the tenant-specific configuration of the application, such as the SSO configuration, provisioning, and user assignments. An owner can also add or remove other owners. Unlike Global Administrators, owners can manage only the applications they own. <!--To assign an enterprise application owner, see *Assigning Owners for an Application*.-->

### <a name="group-owner-permissions"></a>Grup sahibi izinleri

Grup oluşturan kullanıcılar otomatik olarak o grubun sahibi olur. Grup sahipleri grup adı gibi özellikleri ve üyelikleri yönetebilir. Sahipler başka kullanıcıları sahip olarak ekleyebilir veya kaldırabilir. Genel Yöneticiler ve Kullanıcı Hesabı Yöneticilerinden farklı olarak sahipler yalnızca kendilerine ait grupları yönetebilir. Grup sahibi atamak için bkz. [Grup sahiplerini yönetme](active-directory-accessmanagement-managing-group-owners.md).

## <a name="next-steps"></a>Sonraki adımlar

* Azure AD yönetici rollerini atama hakkında daha fazla bilgi için bkz: [bir kullanıcı Azure Active Directory'de yönetici rolleri atama](active-directory-users-assign-role-azure-portal.md)
* Microsoft Azure'da kaynak erişiminin nasıl denetlendiği konusunda daha fazla bilgi için bkz. [Azure'da kaynak erişimini anlama](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Azure Active Directory ile Azure aboneliğinizin arasındaki ilişki hakkında bilgi için bkz. [Azure aboneliklerinin Azure Active Directory ile ilişkisi](active-directory-how-subscriptions-associated-directory.md)
* [Kullanıcıları yönetme](add-users-azure-active-directory.md)
