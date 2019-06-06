---
title: Erişim gözden geçirmeleri - Azure Active Directory ile konuk erişimini yönetme | Microsoft Docs
description: Azure Active Directory erişim gözden geçirmeleri ile bir uygulamaya atanan veya bir grubun üyesi olarak Konuk kullanıcıları yönetme
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
ms.date: 12/13/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7c896356b67de185c55396e98aa6bb2e61dd004d
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66472905"
---
# <a name="manage-guest-access-with-azure-ad-access-reviews"></a>Azure AD ile konuk erişimini yönetme erişim gözden geçirmeleri


Azure Active Directory'ye (Azure AD), kolayca işbirliği kuruluş sınırlarında kullanarak etkinleştirebilirsiniz [Azure AD B2B özellik](../b2b/what-is-b2b.md). Konuk kullanıcıları diğer kiracılardan olabilir [yöneticiler tarafından davet](../b2b/add-users-administrator.md) ya da [diğer kullanıcıların](../b2b/what-is-b2b.md). Bu özellik, Microsoft hesapları gibi sosyal kimlikler için de geçerlidir.

Ayrıca, konuk kullanıcıların uygun erişime sahip kolayca sağlayabilirsiniz. Konuklar, kendileri veya konuklar erişim için bir erişim gözden geçirmesi ve yeniden onaylamasını (veya işleyişinin doğrular) katılmak için bir karar verenden sorabilirsiniz. Gözden geçirenler, Azure AD önerilerine dayanarak her kullanıcının erişiminin devam edip etmemesine yönelik girişler ekleyebilir. Erişim gözden geçirmesi tamamlandığında, sonra değişiklikleri yapın ve artık ihtiyaç duymayan konukların erişimini kaldırmak.

> [!NOTE]
> Bu belge, konuk kullanıcıların erişimini gözden geçirme üzerinde odaklanır. Tüm kullanıcıların erişim, yalnızca Konukları gözden geçirmek istiyorsanız bkz [erişim gözden geçirmeleriyle kullanıcı erişimini yönetme](manage-user-access-with-access-reviews.md). Genel yönetici gibi yönetim rollerindeki kullanıcı üyeliklerini gözden geçirmek istiyorsanız bkz [Azure AD Privileged Identity Management'ta erişim gözden geçirmesi Başlat](../privileged-identity-management/pim-how-to-start-security-review.md).

## <a name="prerequisites"></a>Önkoşullar

- Azure AD Premium P2

Daha fazla bilgi için [hangi kullanıcıların lisansına sahip olması gerekir?](access-reviews-overview.md#which-users-must-have-licenses).

## <a name="create-and-perform-an-access-review-for-guests"></a>Oluşturma ve konuklar için erişim gözden geçirmesi gerçekleştirme

İlk olarak genel yönetici veya Kullanıcı Yöneticisi Git [Kimlik Yönetimi sayfası](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/) erişim gözden geçirmeleri, kuruluşunuz için hazır olduğundan emin olun.

Azure AD, Konuk kullanıcıları gözden geçirme için çeşitli senaryolara olanak sağlar.

Ya da gözden geçirebilirsiniz:

 - Azure AD'de bir veya daha fazla Konuk üyeleri olarak sahip bir grup.
 - Azure AD'de kendisine atanmış bir veya daha fazla Konuk kullanıcılar sahip bir uygulama bağlanmış. 

Ardından her Konuk kendi erişimini gözden geçirmek için veya her konuğun erişimi gözden geçirmek için bir veya daha fazla talep etmek isteyebilir karar verebilirsiniz.

 Bu senaryolar aşağıdaki bölümlerde ele alınmıştır.

### <a name="ask-guests-to-review-their-own-membership-in-a-group"></a>Bir grubu kendi üyelik gözden geçirmek için Konukları isteyin

Erişim gözden geçirmeleri erişmeniz davet ve bir gruba eklendiğinden kullanıcılar devam etmesini sağlamak için kullanabilirsiniz. Kolayca Konukları o gruptaki kendi üyeliğini gözden geçirmek isteyebilirsiniz.

1. Grup için bir erişim gözden geçirmesi oluşturma için yalnızca Konuk kullanıcı üyelerini ve üyeleri kendilerini gözden içerecek şekilde gözden geçirme'ı seçin. Daha fazla bilgi için [grupları ve uygulamaları, erişim gözden geçirmesi Oluştur](create-access-review.md).

2. Kendi üyeliğini gözden geçirme için her Konuk isteyin. Varsayılan olarak, daveti kabul her konuk bir e-posta bağlantısını içeren bir Azure ad erişim gözden geçirmesi için alır. Azure AD'ye nasıl yapılır yönergeleri konuklar için sahip [gruplar veya uygulamalar için erişim gözden geçirme](perform-access-review.md).

3. Gözden geçirenler bilgileri girdikten sonra erişim gözden geçirmesini durdurun ve değişiklikleri uygulayın. Daha fazla bilgi için [grupları ve uygulamaları, erişim değerlendirmesi tamamlama](complete-access-review.md).

4. Ek olarak kendi reddedildi kullanıcılar için sürekli erişmeniz, yanıt vermedi kullanıcılar da kaldırabilirsiniz. Olmayan yanıt kullanıcılar, potansiyel olarak bundan böyle e-posta alır.

5. Gruba erişim yönetimi için kullanılan değil, ayrıca, davetini kabul etmedi çünkü gözden geçirmesine seçili olmayan kullanıcılar kaldırabilirsiniz. Kabul davet edilen kullanıcının e-posta adresi bir yazım yanlışı olduğunu gösteriyor olabilir. Belki de bir grup bir dağıtım listesi kullanılırsa, bazı Konuk kullanıcılar, iletişim nesneleri olduğundan katılmak için seçili olmayan.

### <a name="ask-a-sponsor-to-review-a-guests-membership-in-a-group"></a>Bir konuğun üyelik bir gruba, gözden geçirmek için bir sponsor isteyin

Bir konuğun gereksinimini sürekli bir gruba üyelik gözden geçirmek için bir grup sahibi gibi bir sponsor sorabilirsiniz.

1. Grup için bir erişim gözden geçirmesi oluşturma için yalnızca Konuk kullanıcı üyelerini eklemeyi gözden geçirme'ı seçin. Ardından bir veya daha fazla Gözden Geçiren belirtin. Daha fazla bilgi için [grupları ve uygulamaları, erişim gözden geçirmesi Oluştur](create-access-review.md).

2. Gözden geçirenlerden bilgileri girmelerini isteyin. Varsayılan olarak, her bir e-posta bağlantısını içeren bir Azure ad erişim paneli için aldıkları nerede bunlar [gruplar veya uygulamalar için erişim gözden geçirme](perform-access-review.md).

3. Gözden geçirenler bilgileri girdikten sonra erişim gözden geçirmesini durdurun ve değişiklikleri uygulayın. Daha fazla bilgi için [grupları ve uygulamaları, erişim değerlendirmesi tamamlama](complete-access-review.md).

### <a name="ask-guests-to-review-their-own-access-to-an-application"></a>Kendi uygulama erişimini gözden geçirmek için Konukları isteyin

Erişim gözden geçirmeleri, belirli bir uygulama için davet kullanıcılar erişmesi gereken çalışmaya devam etmesini sağlamak için kullanabilirsiniz. Kolayca erişmesi için kendi gözden geçirmek için Konukları kendilerini kaldırmasını isteyebilirsiniz.

1. Uygulama için erişim gözden geçirmesi oluşturma için yalnızca Konukları ve kullanıcılar kendi erişim gözden içerecek şekilde gözden geçirme'ı seçin. Daha fazla bilgi için [grupları ve uygulamaları, erişim gözden geçirmesi Oluştur](create-access-review.md).

2. Kendi uygulama erişimi gözden geçirmek için her Konuk isteyin. Varsayılan olarak, Azure AD'den bir e-posta daveti kabul her Konuk alır. Bu e-posta, kuruluşunuzun erişim panelinde erişim gözden geçirmesi için bir bağlantı vardır. Azure AD'ye nasıl yapılır yönergeleri konuklar için sahip [gruplar veya uygulamalar için erişim gözden geçirme](perform-access-review.md).

3. Gözden geçirenler bilgileri girdikten sonra erişim gözden geçirmesini durdurun ve değişiklikleri uygulayın. Daha fazla bilgi için [grupları ve uygulamaları, erişim değerlendirmesi tamamlama](complete-access-review.md).

4. Ek olarak kendi reddedildi kullanıcılar için sürekli erişmeniz gerekir, ayrıca yanıt vermedi Konuk kullanıcılar kaldırabilirsiniz. Olmayan yanıt kullanıcılar, potansiyel olarak bundan böyle e-posta alır. Özellikle, yakın zamanda davet edilmemiş olursa katılmak için seçili olmayan Konuk kullanıcılar ayrıca kaldırabilirsiniz. Bu kullanıcılar, uygulama erişimi ve bu nedenle, davetini gerekmedi kabul etmedi. 

### <a name="ask-a-sponsor-to-review-a-guests-access-to-an-application"></a>Bir konuğun uygulama erişimini gözden geçirmek için bir sponsor isteyin

Konuğun uygulamaya sürekli erişim gereksinimini gözden geçirmek için bir uygulamanın sahibi gibi bir sponsor sorabilirsiniz.

1. Uygulama için erişim gözden geçirmesi oluşturma için yalnızca Konukları içerecek şekilde gözden geçirme'ı seçin. Ardından bir veya daha fazla kullanıcı, gözden geçirenler olarak belirtin. Daha fazla bilgi için [grupları ve uygulamaları, erişim gözden geçirmesi Oluştur](create-access-review.md).

2. Gözden geçirenlerden bilgileri girmelerini isteyin. Varsayılan olarak, her bir e-posta bağlantısını içeren bir Azure ad erişim paneli için aldıkları nerede bunlar [gruplar veya uygulamalar için erişim gözden geçirme](perform-access-review.md).

3. Gözden geçirenler bilgileri girdikten sonra erişim gözden geçirmesini durdurun ve değişiklikleri uygulayın. Daha fazla bilgi için [grupları ve uygulamaları, erişim değerlendirmesi tamamlama](complete-access-review.md).

### <a name="ask-guests-to-review-their-need-for-access-in-general"></a>Genel erişim için kendi ihtiyacına gözden geçirmek için Konukları isteyin

Bazı Kurumlarda, konuklar grup üyeliklerini haberdar olmayabilir.

> [!NOTE]
> Azure portal'ın önceki sürümlerinde, UserType Konuk olan kullanıcılar tarafından yönetici erişimine izin vermedi. Bazı durumlarda, yöneticinin directory'niz içinde bir konuğun UserType değer üyesi için PowerShell kullanarak değişmiş olabilir. Bu değişiklik, daha önce dizininizde oluştu, daha önce yönetici erişim haklarına sahip olan tüm Konuk kullanıcılar önceki sorguyu içermeyebilir. Bu durumda, konuğun UserType değiştirin veya konuk grubu üyeliği el ile eklemek gerekir.

1. Uygun bir grup zaten mevcut değilse Konuk üyeleri olarak ile Azure AD'de bir güvenlik grubu oluşturun. Örneğin, bir Konukları tutulan el ile üyelik ile bir grup oluşturabilirsiniz. Veya, Konuk UserType özniteliği değeri olan kullanıcılar Contoso kiracısındaki için "Contoso Konukları" gibi bir ad ile dinamik bir grup oluşturabilirsiniz.  Verimlilik için çoğunlukla Konukları grubu - üye kullanıcıları gözden geçirilmesi gerekmez gibi üye kullanıcılar, bir grup seçmeyin emin olun.  Ayrıca, grubun diğer üyeleri grubunun bir üyesi olan bir konuk kullanıcıyı görebildiğini unutmayın.

2. Bu grup için bir erişim gözden geçirmesi oluşturma için gözden geçirenler üyesi olacak şekilde seçin. Daha fazla bilgi için [grupları ve uygulamaları, erişim gözden geçirmesi Oluştur](create-access-review.md).

3. Kendi üyeliğini gözden geçirme için her Konuk isteyin. Varsayılan olarak, daveti kabul her konuk, bağlantısını içeren bir Azure ad erişim gözden geçirmesi kuruluşunuzun erişim Paneli'nde için bir e-posta alır. Azure AD'ye nasıl yapılır yönergeleri konuklar için sahip [gruplar veya uygulamalar için erişim gözden geçirme](perform-access-review.md).  Kendi daveti kabul etmedi bu Konukları gözden geçirme sonuçları, "değil bildirim" görüntülenir.

4. Gözden geçirenler giriş verdikten sonra erişim gözden geçirmesi durdurun. Daha fazla bilgi için [grupları ve uygulamaları, erişim değerlendirmesi tamamlama](complete-access-review.md).

5. Konuk erişimi reddedildi, gözden geçirme tamamlanmadı veya önceden davetini kabul etmedi konuklar için kaldırın. Konuklar bazılarını gözden geçirmesine üzere seçilmiş olan kişiler veya bunlar daha önce bir daveti kabul etmedi kullanıyorsanız, Azure portal veya PowerShell kullanarak, hesaplarını devre dışı bırakabilirsiniz. Konuk artık erişmesi ve bir kişi yoksa, Konuk kullanıcı nesnesini silmek için Azure portal veya PowerShell kullanarak kendi kullanıcı nesnesi dizininizden kaldırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Grupları ve uygulamaları, erişim gözden geçirmesi oluştur](create-access-review.md)







