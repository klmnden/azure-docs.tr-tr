---
title: Azure AD ile Konuk erişimi yönetme erişimi değerlendirmeleri | Microsoft Docs
description: Konuk kullanıcılar bir gruba üye olarak yönetmek veya Azure Active Directory erişimi incelemeler uygulamayla atanan
services: active-directory
documentationcenter: ''
author: markwahl-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: billmath
ms.openlocfilehash: 564f4f4a3f7532a7419e15b91fdbae9ee12088fd
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="manage-guest-access-with-azure-ad-access-reviews"></a>Azure AD ile Konuk erişimi yönetme erişim gözden geçirme


Azure Active Directory ile (Azure AD), kolayca işbirliği kuruluş sınırlarında kullanarak etkinleştirebilirsiniz [Azure AD B2B özelliği](active-directory-b2b-what-is-azure-ad-b2b.md). Konuk kullanıcılar diğer kiracıdan olabilir [yöneticiler tarafından davet](active-directory-b2b-admin-add-users.md) veya [diğer kullanıcıların](active-directory-b2b-how-it-works.md). Bu özellik, Microsoft hesapları gibi sosyal kimlikler için de geçerlidir.

Ayrıca, konuk kullanıcıların uygun erişime sahip olmasını kolayca sağlayabilirsiniz. Konuklar kendileri veya bir erişim gözden geçirme ve yeniden Onayla (veya şifreli) konuklar erişimi katılmaya karar veren sorabilirsiniz. Gözden geçirenler, Azure AD önerilerine dayanarak her kullanıcının erişiminin devam edip etmemesine yönelik girişler ekleyebilir. Bir erişim gözden geçirme tamamlandığında, sonra değişiklik ve artık ihtiyaç duyan konuklar için erişim kaldırın.

> [!NOTE]
> Bu belge, konuk kullanıcıların erişimini İnceleme odaklanır. Tüm kullanıcıların erişimini, yalnızca Konuklar, gözden geçirmek isterseniz bkz [erişim incelemeleri ile kullanıcı erişimini yönetme](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md). Genel yönetici gibi yönetim rolleri kullanıcıların üyelik gözden geçirmek isterseniz bkz [Azure AD Privileged Identity Management bir erişim incelemesi başlatma](active-directory-privileged-identity-management-how-to-start-security-review.md). 
>
>

## <a name="prerequisites"></a>Önkoşullar 

Erişim gözden geçirmeleri, Azure AD’nin Microsoft Enterprise Mobility + Security, E5’e dahil olan Premium P2 sürümü ile kullanılabilir. Daha fazla bilgi için bkz. [Azure Active Directory sürümleri](active-directory-editions.md). Bir gözden geçirme oluşturmak, gözden geçirmeye erişmek veya gözden geçirme uygulamak üzere bu özellikle etkileşimde bulunan her kullanıcının bir lisansı olması gerekir.

Konuk kullanıcılar kendi access gözden geçirmek için isteyin planlıyorsanız, Konuk kullanıcı lisansı hakkında okuyun. Daha fazla bilgi için bkz: [Azure AD B2B işbirliği lisans](active-directory-b2b-licensing.md).

## <a name="create-and-perform-an-access-review-for-guests"></a>Oluşturma ve konuklar için bir erişim incelemesi gerçekleştir

İlk olarak, gözden geçirenin erişim panoları görünmesi erişim incelemeler etkinleştirin. Bir genel yönetici olarak, [erişim gözden geçirmeleri sayfasına](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/) gidin. 

Azure AD Konuk kullanıcılar gözden geçirme için birçok senaryolara olanak sağlar.

Aşağıdakilerden birini seçin:

 - Bir veya daha fazla konuklar üye olarak sahip Azure AD'de bir grup.
 - Bir uygulama bir veya daha fazla Konuk kullanıcısı kendisine atanmış olan Azure AD bağlı. 

Ardından her Konuk kendi access gözden geçirmek ya da her konuğun erişim gözden geçirmek için bir veya daha fazla kullanıcı sormak için sormak karar verebilirsiniz.

 Bu senaryolar aşağıdaki bölümlerde ele alınmıştır.

### <a name="ask-guests-to-review-their-own-membership-in-a-group"></a>Bir grup içinde kendi üyelik gözden geçirmek için konuklar isteyin

Erişim incelemeleri, davet ettiğiniz ve bir gruba eklenen kullanıcılar erişmeniz devam etmesini sağlamak için kullanabilirsiniz. Kolayca konuklar o gruptaki kendi üyeliğini gözden geçirmek isteyebilirsiniz.

1. Grup için bir erişim gözden geçirme başlatmak için Konuk kullanıcı yalnızca üyeleri ve üyeleri kendilerini gözden dahil olmak üzere gözden seçin. Daha fazla bilgi için [Erişim gözden geçirmesi oluşturma](active-directory-azure-ad-controls-create-access-review.md) konusunu inceleyin.

2. Kendi üyelik gözden geçirmek için her Konuk isteyin. Varsayılan olarak, daveti kabul her konuk bir e-posta bir bağlantı ile Azure AD erişim gözden geçirme için alır. Azure AD nasıl konuklar için yönergeler açmıştır [erişimleri gözden](active-directory-azure-ad-controls-perform-access-review.md).

3. Gözden geçirenler bilgileri girdikten sonra erişim gözden geçirmesini durdurun ve değişiklikleri uygulayın. Daha fazla bilgi için [Erişim gözden geçirmesini tamamlama](active-directory-azure-ad-controls-complete-access-review.md) konusunu inceleyin.

4. Kendi gereksinimini sürekli erişim reddedildi kullanıcılar ek olarak, yanıt vermedi kullanıcılar da kaldırabilirsiniz. Olmayan yanıt kullanıcıları büyük olasılıkla artık e-posta alır.

5. Grup erişim yönetimi için kullanılmaz, çünkü bunların daveti kabul kaydetmedi incelemeye katılmak için seçili doğru kullanıcılar da kaldırabilirsiniz. Kabul davet edilen kullanıcının e-posta adresi bir yazım hatası olduğunu gösteriyor olabilir. Bir grup bir dağıtım listesi kullanılırsa, belki de bazı Konuk Seçili kullanıcıları silmeye kişi nesneleri oldukları için katılmak için doğru.

### <a name="ask-a-sponsor-to-review-a-guests-membership-in-a-group"></a>Bir gruptaki bir konuğun üyeliği gözden geçirmek için bir sponsoru isteyin

Devam eden bir gruba üyelik bir konuğun gereksinimini gözden geçirmek için bir grup sahibi gibi bir sponsoru sorabilirsiniz.

1. Grup için bir erişim gözden geçirme başlatmak için Konuk kullanıcı yalnızca üyeleri dahil etmek için gözden geçirme seçin. Ardından bir veya daha fazla gözden geçirenler belirtin. Daha fazla bilgi için [Erişim gözden geçirmesi oluşturma](active-directory-azure-ad-controls-create-access-review.md) konusunu inceleyin.

2. Gözden geçirenlerden bilgileri girmelerini isteyin. Varsayılan olarak, tüm gözden geçirenler Azure AD’den [erişim gözden geçirmelerini gerçekleştirecekleri](active-directory-azure-ad-controls-perform-access-review.md) erişim paneline yönlendiren bir bağlantı içeren bir e-posta alır.

3. Gözden geçirenler bilgileri girdikten sonra erişim gözden geçirmesini durdurun ve değişiklikleri uygulayın. Daha fazla bilgi için [Erişim gözden geçirmesini tamamlama](active-directory-azure-ad-controls-complete-access-review.md) konusunu inceleyin.

### <a name="ask-guests-to-review-their-own-access-to-an-application"></a>Kendi bir uygulamaya erişmeye gözden geçirmek için konuklar isteyin

Belirli bir uygulama için davet kullanıcılar erişmeniz devam etmesini sağlamak için erişim incelemeler kullanabilirsiniz. Kendi gözden geçirmek için konuklar kendilerini erişmeniz için kolaylıkla isteyebilir.

1. Uygulama için bir erişim gözden geçirme başlatmak için yalnızca konuklar ve kullanıcılar kendi access gözden dahil olmak üzere gözden seçin. Daha fazla bilgi için [Erişim gözden geçirmesi oluşturma](active-directory-azure-ad-controls-create-access-review.md) konusunu inceleyin.

2. Uygulama kendi erişimi gözden geçirmek için her Konuk isteyin. Varsayılan olarak, daveti kabul her konuk, bir bağlantı ile Azure AD'den kuruluşunuzun erişim panelinde erişim gözden geçirme için bir e-posta alır. Azure AD nasıl konuklar için yönergeler açmıştır [erişimleri gözden](active-directory-azure-ad-controls-perform-access-review.md).

3. Gözden geçirenler bilgileri girdikten sonra erişim gözden geçirmesini durdurun ve değişiklikleri uygulayın. Daha fazla bilgi için [Erişim gözden geçirmesini tamamlama](active-directory-azure-ad-controls-complete-access-review.md) konusunu inceleyin.

4. Kendi reddedildi kullanıcıların yanı sıra, sürekli erişim için gerekir, ayrıca yanıt vermedi Konuk kullanıcılar kaldırabilirsiniz. Olmayan yanıt kullanıcıları büyük olasılıkla artık e-posta alır. Özellikle, son davet doğru değilse katılmak için seçilen doğru Konuk kullanıcılar da kaldırabilirsiniz. Bu kullanıcılar kendi daveti ve bu nedenle uygulamaya erişimi olmadığına kabul alamadık. 

### <a name="ask-a-sponsor-to-review-a-guests-access-to-an-application"></a>Bir uygulamaya bir konuğun erişim gözden geçirmek için bir sponsoru isteyin

Uygulama sürekli erişim konuğun gereksinimini gözden geçirmek için bir uygulama sahibi gibi bir sponsoru sorabilirsiniz.

1. Uygulama için bir erişim gözden geçirme başlatmak için yalnızca konuklar içerecek şekilde gözden seçin. Ardından bir veya daha fazla kullanıcı gözden geçirenler belirtin. Daha fazla bilgi için [Erişim gözden geçirmesi oluşturma](active-directory-azure-ad-controls-create-access-review.md) konusunu inceleyin.

2. Gözden geçirenlerden bilgileri girmelerini isteyin. Varsayılan olarak, tüm gözden geçirenler Azure AD’den [erişim gözden geçirmelerini gerçekleştirecekleri](active-directory-azure-ad-controls-perform-access-review.md) erişim paneline yönlendiren bir bağlantı içeren bir e-posta alır.

3. Gözden geçirenler bilgileri girdikten sonra erişim gözden geçirmesini durdurun ve değişiklikleri uygulayın. Daha fazla bilgi için [Erişim gözden geçirmesini tamamlama](active-directory-azure-ad-controls-complete-access-review.md) konusunu inceleyin.

### <a name="ask-guests-to-review-their-need-for-access-in-general"></a>Erişim, kendi gereksinimini genel gözden geçirmek için konuklar isteyin

Bazı kuruluşlarda, konuklar grup üyeliklerini uyumlu olmayabilir.

> [!NOTE]
> Azure portal'ın önceki sürümlerinde, UserType Konuk kullanıcılar tarafından yönetim erişimine alamadık. Bazı durumlarda, yönetici dizininizde bir konuğun UserType değeri üyesine PowerShell kullanarak değişmiş olabilir. Bu değişiklik önceden dizininizde oluştuysa, önceki sorgu geçmişte yönetici erişim haklarına sahip olan tüm Konuk kullanıcılar içermeyebilir. Bu durumda, konuğun UserType değiştirin veya konuk grup üyeliği içinde el ile eklemek gerekir.

1. Uygun bir grup zaten mevcut değilse Konuk üyeleri olarak ile Azure AD'de bir güvenlik grubu oluşturun. Örneğin, el ile tutulan konuklar üyelikle bir grup oluşturabilirsiniz. Veya, Konuk UserType özniteliği değeri sahip kullanıcılar Contoso kiracısındaki "Contoso konuklar" gibi bir adla dinamik bir grup oluşturabilirsiniz.  Verimlilik için Grup çoğunlukla konuklar - gözden geçirilmesi gerekmez kullanıcıları olan bir grubu seçmeyin emin olun.

2. Bu grup için bir erişim gözden geçirme başlatmak için gözden geçirenler üyesi olacak şekilde seçin. Daha fazla bilgi için [Erişim gözden geçirmesi oluşturma](active-directory-azure-ad-controls-create-access-review.md) konusunu inceleyin.

3. Kendi üyelik gözden geçirmek için her Konuk isteyin. Varsayılan olarak, daveti kabul her konuk, bir bağlantı ile Azure AD'den kuruluşunuzun erişim panelinde erişim gözden geçirme için bir e-posta alır. Azure AD nasıl konuklar için yönergeler açmıştır [erişimleri gözden](active-directory-azure-ad-controls-perform-access-review.md).  Kendi daveti kabul etmediğiniz bu konuklar İnceleme sonuçları "Bilgilendirilmez" görünür.

4. Gözden geçirenler giriş verdikten sonra erişim gözden geçirme durdurun. Daha fazla bilgi için [Erişim gözden geçirmesini tamamlama](active-directory-azure-ad-controls-complete-access-review.md) konusunu inceleyin.

5. Konuk erişimi engellendi, gözden geçirme tamamlanmadı ya da daha önce kendi daveti kabul kaydetmedi konuklar için kaldırın. Konuklar bazıları incelemeye katılmak üzere seçilmiş olan kişiler veya bir davet önceden kabul etmediğiniz kullanıyorsanız, Azure portal veya PowerShell kullanarak, hesaplarını devre dışı bırakabilirsiniz. Konuk artık erişmesi ve bir kişi yoksa, Konuk kullanıcı nesnesini silmek için Azure portal veya PowerShell kullanarak kendi kullanıcı nesnesi dizininizden kaldırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Bir grubun üyeleri veya bir uygulamaya erişim için erişim gözden geçirmesi oluşturma](active-directory-azure-ad-controls-create-access-review.md)







