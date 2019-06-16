---
title: 'Azure AD Connect: Azure AD zaten varsa | Microsoft Docs'
description: Bu konu, mevcut bir Azure AD kiracısına sahip olduğunuzda Connect kullanmayı açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/25/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1495c14ae4c588661452aa3696019da00be47548
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64571354"
---
# <a name="azure-ad-connect-when-you-have-an-existent-tenant"></a>Azure AD Connect: Mevcut bir kiracınız varsa
Azure AD Connect kullanmayla ilgili konular çoğunu varsayar başlattığınız ile yeni bir Azure AD kiracısı ve kullanıcı yok veya diğer nesneleri yok. Ancak bir Azure AD kiracınız ile başlamış olması durumunda kullanıcıların ve diğer nesnelerle doldurulur ve artık bağlan, kullanmak istediğiniz sonra bu konuda size göre.

## <a name="the-basics"></a>Temel bilgiler
Azure AD'de bir nesne ya da bulutta (Azure AD) veya şirket içinde yönetilir. Tek bir nesne için bazı öznitelikler şirket içinde ve diğer bazı öznitelikler Azure AD'de yönetemez. Her nesne bir nesne yerine yönetildiği belirten bir bayrak vardır.

Bazı kullanıcılar şirket içi ve diğer bulutta yönetebilirsiniz. Bu yapılandırma için yaygın bir senaryo, hesap çalışanları ve Satış çalışanları karışımını içeren bir kuruluştur. Hesap çalışanlarının şirket içi AD hesabı ancak Satış çalışanları yapmak için Azure AD'de bir hesap sahiptirler. Bazı kullanıcılar şirket içi ve Azure ad'deki bazı yönettiğiniz.

Yönetilecek başlattıysanız da içinde olan kullanıcılar Azure AD'de şirket içi AD ve eğer göz önünde bulundurmanız gereken bazı ek konuları daha sonra Connect kullanmak istiyorsanız.

## <a name="sync-with-existing-users-in-azure-ad"></a>Mevcut kullanıcıların Azure AD'de eşitleme
Azure AD Connect'i yüklemek ve Eşitlemeyi Başlat, Azure AD eşitleme hizmetini (Azure AD'de) her yeni nesne üzerinde bir denetimi yapar ve eşleştirmek için var olan bir nesneyi bulmak deneyin. Bu işlem için kullanılan üç öznitelikleri vardır: **userPrincipalName**, **proxyAddresses**, ve **sourceAnchor**/**Immutableıd** . Bir eşleşme **userPrincipalName** ve **proxyAddresses** olarak bilinen bir **yazılım eşleşme**. Bir eşleşme **sourceAnchor** olarak da bilinen **sabit eşleşme**. İçin **proxyAddresses** yalnızca değeri ile özniteliği **SMTP:** , diğer bir deyişle birincil e-posta adresi, değerlendirme için kullanılır.

Eşleşme yalnızca Connect'ten yakında yeni nesneler için değerlendirilir. Bu öznitelikleri eşleşen şekilde var olan bir nesneyi değiştirin, ardından bir hata bunun yerine görürsünüz.

Ardından Azure AD'ye nerede öznitelik değerleri Connect'ten gelen bir nesne için aynıdır ve Azure AD'de mevcut olan bir nesneyi bulursa, nesnenin Azure ad Connect tarafından devralınırsa alınır. Daha önce bulutta Yönetilen Nesne yönetilen şirket içi işaretlenir. Tüm öznitelikler Azure AD'de bir değerle şirket içi AD yazılır ile şirket içi değer. Bir öznitelik olduğunda istisnadır bir **NULL** şirket içi değer. Bu durumda, değer Azure AD'ye kalır, ancak yine de yalnızca şirket içi şeye değiştirebilirsiniz.

> [!WARNING]
> Tüm öznitelikler Azure AD'de şirket içi değerin üzerine gidip beri iyi verilerinizi şirket içinde olduğundan emin olun. Örneğin, e-posta adresi Office 365'te yalnızca yönetilen ve tutulan değil, güncelleştirme, AD DS, AD DS'de mevcut Azure AD/Office 365'te herhangi bir değeri kaybetmek sonra şirket içi.

> [!IMPORTANT]
> Her zaman hızlı ayarları tarafından kullanılır, parola eşitleme, kullandığınız sonra ile şirket içi parolanın Azure AD'de parola üzerine AD. Kullanıcılarınız farklı parolaları yönetmek için kullanılan, bunları Connect yüklendiğinde, şirket içi parola kullanmasını bildirmeniz gerekir.

Uyarı ve önceki bölümde, planlamada dikkate alınmalıdır. Yansıtılan değil Azure AD'de birçok değişiklik yaptıysanız, AD DS yapma nesnelerinizi Azure AD Connect ile eşitleme önce AD DS güncelleştirilmiş değerlerle doldurmak için planlama yapmanız sonra şirket içi.

Nesnelerinizi soft-eşleşen, karşılarsa, ardından **sourceAnchor** sabit bir eşleşme daha sonra kullanılabilmesi için Azure AD'de nesnesine eklenir.

>[!IMPORTANT]
> Microsoft Azure Active Directory'de önceden mevcut olan yönetici hesaplarıyla şirket hesapları eşitleme karşı önerir.

### <a name="hard-match-vs-soft-match"></a>Vs Soft-match sabit eşleştir
Connect yeni yükleme için pratik soft - ve eşleşme sabit arasında fark yoktur. Bir olağanüstü durum kurtarma durumunda farktır. Azure AD Connect ile sunucunuza kaybettiyseniz, veri kaybı olmadan yeni bir örneğini yeniden yükleyebilirsiniz. Bir sourceAnchor sahip bir nesne, ilk yükleme sırasında bağlan gönderilir. Eşleşme aynı Azure AD'de yapmaktan daha çok daha hızlıdır (Azure AD Connect), istemci tarafından ardından değerlendirilebilir. Sabit bir eşleşme hem Connect ve Azure AD tarafından değerlendirilir. Geçici eşleştirme, yalnızca Azure AD tarafından kabul edilir.

### <a name="other-objects-than-users"></a>' Den fazla kullanıcının diğer nesneleri
Posta özellikli gruplar ve kişiler için geçici göre proxyAddresses eşleştirme. Match sabit sourceAnchor / (PowerShell kullanarak) Immutableıd kullanıcılar yalnızca yalnızca güncelleştirebilirsiniz geçerli değildir. Posta etkin olmayan gruplar için şu anda yazılımla eşleşen veya eşleşme sabit desteği yoktur.

### <a name="admin-role-considerations"></a>Yönetici rolü konuları
Güvenilmeyen şirket içi kullanıcıların herhangi bir yönetici rolüne sahip bir bulut kullanıcısı ile eşleşen dizinden önlemek için Azure AD Connect Yöneticisi rolüne sahip nesneleri ile şirket içi kullanıcı nesneleri eşleşmez. Bu, varsayılan olarak açıktır. Geçici çözüm için bu davranış, şunları yapın:

1.  Yalnızca bulutta yer alan kullanıcı nesneden dizin rolü kaldırılamıyor
2.  Bir eşitleme tetikleme
3.  Eşleme gerçekleştikten sonra kullanıcı nesnenin bulutta isteğe bağlı olarak dizin rollerini ekleyin.



## <a name="create-a-new-on-premises-active-directory-from-data-in-azure-ad"></a>Yeni bir şirket içi Active Directory, Azure AD'de verilerden oluşturma
Bazı müşteriler bir yalnızca bulut çözümünü Azure AD ile başlayın ve şirket içi olmayan AD. Daha sonra şirket kaynaklarının kullanılmasına ve bir şirket içi yapı istedikleri AD, Azure AD verileri temel alarak. Azure AD Connect, bu senaryo için yardımcı olamaz. Kullanıcılar şirket içi oluşturmaz ve Azure AD ile aynı şirket içi parolayı ayarlamak için hiçbir özelliği yok.

Eklemeyi planladığınız neden tek nedeni şirket içi AD belki de kullanmayı düşünmelisiniz sonra LOB'lar (satır iş kolu uygulamaları) desteklemek için ise [Azure AD etki alanı Hizmetleri](../../active-directory-domain-services/index.yml) yerine.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
