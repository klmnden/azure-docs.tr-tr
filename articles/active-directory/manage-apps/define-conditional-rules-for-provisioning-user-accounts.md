---
title: Kapsam filtreleri ile uygulamaları sağlama | Microsoft Docs
description: Bir nesne iş gereksinimlerinizi karşılamaya yetmezse sağlanan otomatik kullanıcı sağlamayı destekleyen uygulamalar nesneleri önlemek için kapsam belirleme filtrelerini kullanma konusunda bilgi edinin.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: mimart
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2c831fc7ab1a646d41c0dc08d0e1a66380fe1232
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65824730"
---
# <a name="attribute-based-application-provisioning-with-scoping-filters"></a>Kapsam filtreleri ile öznitelik tabanlı uygulama sağlama
Bu makalenin amacı, bir uygulamaya hangi kullanıcılar sağlanır belirlemek öznitelik tabanlı kurallar tanımlamak için kapsam belirleme filtrelerini kullanma açıklanmaktadır sağlamaktır.

## <a name="scoping-filter-use-cases"></a>Kapsam belirleme filtresi kullanım örnekleri

Kapsam belirleme filtresi, Azure Active sağlama hizmeti dahil edin veya belirli bir değerle eşleşen bir özniteliği olan tüm kullanıcıları dışlayın Directory (Azure AD) sağlar. Örneğin, satış ekibi tarafından kullanılan bir SaaS uygulamasını Azure AD'den kullanıcılara sağlanırken, "Sales", "Departman" özniteliğine sahip yalnızca kullanıcıların sağlama kapsamında olması gerektiğini belirtebilirsiniz.

Kapsam filtreleri sağlama bağlayıcı türüne bağlı olarak farklı şekilde kullanılabilir:

* **Giden Azure AD'den SaaS uygulamaları için sağlama**. Azure AD, kaynak sistemi olduğunda [kullanıcı ve Grup atamaları](assign-user-or-group-access-portal.md) hangi kullanıcıların sağlama kapsamında olduğunu belirlemek için en yaygın bir yöntemdir. Ayrıca bu atamaları çoklu oturum açmayı etkinleştirmek için kullanılır ve erişim ve sağlama yönetmek için tek bir yöntem sağlar. Kapsam filtreleri isteğe bağlı olarak, atamalar yanı sıra veya bunların yerine kullanıcıları öznitelik değerlerine göre filtrelemek için kullanılabilir.

    >[!TIP]
    > Kurumsal uygulama atamalarını ayarlarını değiştirerek göre sağlama devre dışı bırakabilirsiniz [kapsam](user-provisioning.md#how-do-i-set-up-automatic-provisioning-to-an-application) sağlama ayarları menüsünde **tüm kullanıcıları ve grupları eşitleme**. Bu seçenek yanı sıra kapsamı belirleme filtreleri öznitelik tabanlı kullanarak grup tabanlı atamaları kullanarak daha hızlı performans sağlar.  

* **Azure AD'ye HCM uygulamalardan sağlama gelen ve Active Directory**. Olduğunda bir [HCM uygulama Workday gibi](../saas-apps/workday-tutorial.md) kaynak sistem kapsamı belirleme filtreleri olan Active Directory veya Azure AD HCM uygulamasından hangi kullanıcıların sağlanan belirlemek için birincil yöntemi.

Varsayılan olarak, Azure AD sağlama bağlayıcıları yapılandırılmış öznitelik tabanlı kapsam belirleme filtre yok. 

## <a name="scoping-filter-construction"></a>Kapsam belirleme filtresi oluşturma

Bir veya daha fazla kapsam belirleme filtresi oluşur *yan tümceleri*. Yan tümceleri, her kullanıcının öznitelikleri değerlendirerek kapsam belirleme filtresi geçmesine izin verilen kullanıcıları belirleyin. Örneğin, yalnızca New York kullanıcılar uygulamaya sağlanır, böylece bir kullanıcının "State" özniteliği "New York", eşittir gerektirir bir yan tümce olabilir. 

Tek bir yan tümce tek öznitelik değeri için tek bir koşulu tanımlar. Birden çok yan tümcesi içinde tek bir kapsam belirleme filtresi oluşturduysanız, birlikte olarak bunlar "Ve" mantığı kullanılarak değerlendirilir. Başka bir deyişle, tüm yan tümceleri sağlanacak bir kullanıcı için sırayla "true" olarak değerlendirilmelidir.

Son olarak, tek bir uygulama için birden çok kapsam belirleme filtrelerini oluşturulabilir. Birden çok kapsam belirleme Fitresi varsa, birlikte olarak bunlar "Veya" mantığı kullanılarak değerlendirilir. Bu kullanıcı herhangi birinde yapılandırılan kapsamı belirleme filtreleri tüm sözleşme maddeleri "true" olarak değerlendiriliyorsa sağlandığından emin anlamına gelir.

Her bir kullanıcı veya grup Azure AD sağlama hizmeti tarafından işlenen her zaman tek tek her kapsam belirleme filtresi karşı değerlendirilir.

Örneğin, aşağıdaki kapsam belirleme filtresi göz önünde bulundurun:

![Kapsam belirleme filtresi](./media/define-conditional-rules-for-provisioning-user-accounts/scoping-filter.PNG) 

Bu kapsam belirleme filtresi göre kullanıcıların sağlanması için aşağıdaki ölçütleri karşılaması gerekir:

* New York'daki olmaları gerekir.
* Bunlar mühendislik departmanında çalışmalıdır.
* Kendi şirket çalışan kimliği 2.000.000 1.000.000 arasında olmalıdır.
* Kendi iş unvanı, null veya boş olmamalıdır.

## <a name="create-scoping-filters"></a>Kapsam belirleme filtre oluşturma
Kapsam belirleme filtrelerini bağlayıcı sağlama her bir Azure AD kullanıcı için öznitelik eşlemelerini bir parçası olarak yapılandırılır. Aşağıdaki yordamı, otomatik için sağlamayı ayarlama zaten ayarlanmış olduğunu varsayar [desteklenen uygulamaların birini](../saas-apps/tutorial-list.md) ve kapsam belirleme filtresi için ekliyoruz.

### <a name="create-a-scoping-filter"></a>Kapsam belirleme filtresi oluşturma
1. İçinde [Azure portalında](https://portal.azure.com)Git **Azure Active Directory** > **kurumsal uygulamalar** > **tümuygulamalar** bölümü.

2. Kendisi için yapılandırdığınız otomatik sağlama uygulamayı seçin: Örneğin, "ServiceNow".

3. Seçin **sağlama** sekmesi.

4. İçinde **eşlemeleri** bölümünde, bir kapsam belirleme filtresi için yapılandırmak istediğiniz eşlemeyi Seç: Örneğin, "eşitleme Azure Active Directory Kullanıcıları için ServiceNow".

5. Seçin **kaynak nesne kapsamı** menüsü.

6. Seçin **kapsam belirleme Filtresi Ekle**.

7. Bir kaynak seçerek bir yan tümce tanımlamak **öznitelik adı**e **işleci**ve bir **öznitelik değeri** eşleştirilecek. Aşağıdaki işleçleri destekler:

   a. **EŞİTTİR**. Yan tümcesi döndürür "true" değerlendirilen öznitelik giriş dizesi değerini tam olarak eşleşmesi durumunda (büyük/küçük harfe duyarlı).

   b. **EŞİT DEĞİLDİR**. Yan tümcesi "değerlendirilen öznitelik (büyük/küçük harfe duyarlı) giriş dizesi değerini eşleşmiyorsa true" değerini döndürür.

   c. **TRUE**. Yan tümcesi "değerlendirilen özniteliği Boolean true değerini içeriyorsa true" değerini döndürür.

   d. **YANLIŞ**. Yan tümcesi "değerlendirilen özniteliği false Boolean değerini içeriyorsa true" değerini döndürür.

   e. **NULL**. Yan tümcesi "değerlendirilen öznitelik boşsa true" değerini döndürür.

   f. **NULL DEĞİL**. Yan tümcesi "değerlendirilen öznitelik boş değilse true" değerini döndürür.

   g. **DÜZENLİ İFADESİ EŞLEŞTİRMESİ**. Yan tümcesi "değerlendirilen öznitelik, bir normal ifade deseni eşleşiyorsa true" değerini döndürür. Örneğin: ([1-9][0-9]) eşleşen 10 ile 99 arasında bir sayı.

   h. **DÜZENLİ İFADESİ EŞLEŞTİRMESİ**. Yan tümcesi "değerlendirilen özniteliği bir normal ifade deseniyle eşleşmeyen ise true" değerini döndürür.

8. Seçin **Ekle Yeni kapsam belirleme yan tümcesi**.

9. İsteğe bağlı olarak, adımları 7-8, daha fazla kapsam belirleme yan tümceler eklemek için yineleyin.

10. İçinde **kapsam belirleme filtresi başlığı**, kapsam belirleme filtresi için bir ad ekleyin.

11. **Tamam**’ı seçin.

12. Seçin **Tamam** tekrar **kapsamı belirleme filtreleri** ekran. İsteğe bağlı olarak, adım 6-11, başka bir kapsam belirleme filtresi eklemek için yineleyin.

13. Seçin **Kaydet** üzerinde **eşleme özniteliği** ekran. 

>[!IMPORTANT] 
> Yeni bir kapsam belirleme filtresi Tetikleyicileri burada kaynak sistemindeki tüm kullanıcılar yeni bir kapsam belirleme filtresi karşı tekrar değerlendirilir uygulama için yeni bir tam eşitleme kaydediliyor. Uygulamada bir kullanıcı daha önce sağlama, ancak kapsam dışında yer alan için kapsamdaki ise hesabını devre dışı veya uygulamada sağlaması kaldırıldı.


## <a name="related-articles"></a>İlgili makaleler
* [Sağlama ve sağlamayı kaldırma SaaS uygulamalarına kullanıcı otomatikleştirin](user-provisioning.md)
* [Kullanıcı sağlama için öznitelik eşlemelerini özelleştirme](customize-application-attributes.md)
* [Öznitelik eşlemeleri için ifadeler yazma](functions-for-customizing-application-data.md)
* [Hesap sağlama bildirimleri](user-provisioning.md)
* [Kullanıcılar ve grupların Azure Active Directory'den uygulamalara otomatik olarak hazırlanmasını etkinleştirmek için SCIM'yi kullanma](use-scim-to-provision-users-and-groups.md)
* [SaaS uygulamalarını tümleştirme hakkında öğreticiler listesi](../saas-apps/tutorial-list.md)

