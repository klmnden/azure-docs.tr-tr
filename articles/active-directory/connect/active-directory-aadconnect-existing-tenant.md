---
title: 'Azure AD Connect: Zaten varsa Azure AD | Microsoft Docs'
description: Bu konu, mevcut Azure AD kiracısı olduğunda Bağlan kullanmayı açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 726d8998d24a630808186eea417f236fdbfb565e
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34725216"
---
# <a name="azure-ad-connect-when-you-have-an-existent-tenant"></a>Azure AD Connect: Mevcut bir kiracı olduğunda
Azure AD Connect kullanmayla ilgili konular çoğunu varsayar ile yeni bir Azure başlattığınız AD Kiracı ve hiçbir kullanıcı ya da diğer nesneleri vardır. Ancak bir Azure AD kiracısı ile başlamış olması durumunda kullanıcılar ve diğer nesneleri ile doldurulur ve şimdi bağlan, kullanmak istediğiniz sonra bu konuda size göre.

## <a name="the-basics"></a>Temel bilgiler
Azure AD'de bir nesne ya da bulutta (Azure AD) veya şirket içinde yönetilir. Tek bir nesne için Azure AD içinde bazı öznitelikler şirket içi ve bazı diğer öznitelikleri yönetemez. Her nesnenin nesne yönetildiği belirten bir bayrak vardır.

Bazı kullanıcılar şirket içi ve diğer bulutta yönetebilirsiniz. Bu yapılandırma için yaygın bir senaryo bir kuruluş hesap çalışanları ve Satış çalışanları karışımını içeren ' dir. Şirket içi hesap çalışanlarının AD hesabının ancak Satış çalışanları yapmak için bunlar Azure AD'de bir hesabınız yok. Bazı kullanıcılar şirket içi ve bazı Azure AD'de yönetmek.

Yönetmek başladıysanız, ayrıca, kullanıcılar Azure AD'de şirket içi AD ve göz önünde bulundurmanız gereken bazı ek sorunları sonra daha sonra Connect kullanmak istiyorsanız.

## <a name="sync-with-existing-users-in-azure-ad"></a>Mevcut kullanıcıların Azure AD'de eşitleme
Azure AD Connect'i yüklemek ve eşitleme başlatın, Azure AD eşitleme hizmeti (Azure AD'de) her yeni nesne üzerinde bir denetimi yapar ve eşleştirmek için varolan bir nesneyi bulmak deneyin. Bu işlem için kullanılan üç öznitelikleri vardır: **userPrincipalName**, **proxyAddresses**, ve **sourceAnchor**/**İmmutableıd**. Bir eşleşme **userPrincipalName** ve **proxyAddresses** olarak bilinen bir **yumuşak eşleşme**. Bir eşleşme **sourceAnchor** olarak bilinen **sabit eşleşme**. İçin **proxyAddresses** yalnızca değerle özniteliği **SMTP:**, diğer bir deyişle birincil e-posta adresi değerlendirmesi için kullanılır.

Eşleşmeyi yalnızca Bağlan gelen yeni nesneler için değerlendirilir. Varolan bir nesne, bu özelliklerden herhangi birini eşleşen şekilde değiştirirseniz, daha sonra bir hata yerine bakın.

Azure AD, öznitelik değerleri Bağlan gelen bir nesne için aynıdır ve Azure AD'de mevcut olan bir nesneyi bulursa, Azure AD nesnesinde Connect tarafından ele alınır. Daha önce bulut Yönetilen Nesne yönetilen şirket içi işaretlenir. Tüm öznitelikleri şirket içi bir değere sahip Azure AD'de AD yazılır ile şirket içi değer. Özel bir öznitelik olduğunda bir **NULL** şirket içi değer. Bu durumda, Azure AD kalır, ancak değer hala yalnızca şirket içi başka bir şey için değiştirebilirsiniz.

> [!WARNING]
> Azure AD'de tüm özniteliklerin şirket içi değer tarafından üzerine yazılmasını kalacaklarını beri iyi verileri şirket içinde olduğundan emin olun. Örneğin, e-posta adresi Office 365'te yalnızca yönetilen ve değil tutulan içinde güncelleştirilmiş AD DS, AD DS'de mevcut Azure AD/Office 365'te herhangi bir değeri kaybetmek sonra şirket içi.

> [!IMPORTANT]
> Her zaman hızlı ayarları tarafından kullanılır, Parola Eşitleme'yi kullanmaya sonra Azure AD'de parola şirket parolayla üzerine AD. Kullanıcılarınız farklı parolaları yönetmek için kullanılıyorsa, bunları Bağlan yüklendiğinde bunlar şirket içi parola kullanmalısınız bildirmeniz gerekir.

Planlamanızda, uyarı ve önceki bölümde ele alınması gerekir. Yansıtılan değil Azure AD'de birçok değişiklik yaptıysanız, AD DS nesnelerinizi Azure AD Connect ile eşitleme önce AD DS güncelleştirilmiş değerlerle doldurmak nasıl planlama yapmanız sonra şirket içi.

Nesnelerinizi soft-eşleştirme ile eşleşmesi durumunda sonra **sourceAnchor** sabit bir eşleşme daha sonra kullanılabilmesi için Azure AD'de nesnesine eklenir.

### <a name="hard-match-vs-soft-match"></a>Vs Soft-match sabit eşleşmiyor
Connect yeni yükleme için arasındaki soft - eşleşme sabit pratik fark yoktur. Bir olağanüstü durum kurtarma durumda farktır. Azure AD Connect sunucunuzla kaybettiyseniz, hiçbir veri kaybı olmadan yeni bir örneğini yeniden yükleyebilirsiniz. Bir sourceAnchor sahip bir nesne Bağlan ilk yükleme sırasında gönderilir. Eşleşme sonra aynı Azure AD içinde yapmaktan daha çok daha hızlıdır (Azure AD Connect), istemci tarafından değerlendirilebilir. Sabit bir eşleşme hem Bağlan ve Azure AD tarafından değerlendirilir. Geçici bir eşleşme yalnızca Azure AD tarafından değerlendirilir.

### <a name="other-objects-than-users"></a>Kullanıcılar daha başka nesneler
Posta özellikli gruplar ve kişiler için yazılım göre proxyAddresses eşleştirme. Eşleşme sabit yalnızca sourceAnchor / (PowerShell kullanarak) İmmutableıd kullanıcıları yalnızca güncelleştirme geçerli değildir. Posta etkin olmayan grupları için şu anda soft-match veya eşleşme sabit desteği yoktur.

## <a name="create-a-new-on-premises-active-directory-from-data-in-azure-ad"></a>Yeni bir şirket içi Active Directory verilerini Azure AD oluşturma
Azure AD ile yalnızca bulut çözümü olan bazı müşteriler başlatmak ve şirket içi olmayan AD. Daha sonra şirket kaynaklarını tüketebilir ve bir şirket içi oluşturmak istediğiniz istedikleri AD tabanlı Azure AD verileri. Azure AD Connect bu senaryo için yardımcı olamaz. Kullanıcıların şirket içi oluşturmaz ve Azure AD ile aynı parola şirket içi koymak yeteneği yok.

Eklemek için planlama neden tek nedeni şirket içi AD belki de kullanmayı düşünmelisiniz sonra LOB'lar (satır iş kolu uygulamaları) desteklemek için ise [Azure AD etki alanı Hizmetleri](../../active-directory-domain-services/index.yml) yerine.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.
