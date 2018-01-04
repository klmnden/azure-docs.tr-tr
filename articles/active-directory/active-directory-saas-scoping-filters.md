---
title: "Kapsam belirleme filtreleri ile uygulamalar sağlama | Microsoft Docs"
description: "Nesneleri nesneyi iş gereksinimlerinizi karşılamadığı, sağlanan otomatik kullanıcı sağlamayı destekleyen uygulamalarda önlemek için kapsam filtreleri kullanmayı öğrenin."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: bcfbda74-e4d4-4859-83bc-06b104df3918
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2017
ms.author: markvi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e7a2322239945a529a544054c2273e37a3d65abf
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="attribute-based-application-provisioning-with-scoping-filters"></a>Kapsam belirleme filtreleri ile öznitelik tabanlı uygulama sağlama
Bu makalenin amacı, kapsam filtreleri hangi kullanıcıların bir uygulamaya sağlanan belirlemek öznitelik tabanlı kurallar tanımlamak için nasıl kullanılacağını açıklamaktadır sağlamaktır.

## <a name="scoping-filter-use-cases"></a>Kapsam filtresi kullanım örnekleri

Bir kapsam filtresi Azure hizmet sağlama dahil etmek veya hariç belirli bir değerle eşleşen bir özniteliği olan tüm kullanıcılar için Active Directory'yi (Azure AD) sağlar. Örneğin, satış ekibi tarafından kullanılan bir SaaS uygulaması Azure AD'den kullanıcılara sağlanırken, yalnızca bir "Departman" özniteliği "Satış" sahip kullanıcılar sağlama kapsamında olması gerektiğini belirtebilirsiniz.

Kapsam belirleme filtreleri sağlama bağlayıcı türüne bağlı olarak farklı şekillerde kullanılabilir:

* **Giden Azure AD SaaS uygulamaları için sağlama**. Azure AD kaynak sistemi olduğunda [kullanıcı ve Grup atamaları](active-directory-coreapps-assign-user-azure-portal.md) sağlama kapsamında hangi kullanıcıların olan belirlemek için en yaygın bir yöntemdir. Ayrıca bu atamaları çoklu oturum açmayı etkinleştirmek için kullanılır ve erişim ve sağlama yönetmek için tek bir yöntem sağlar. Kapsam belirleme filtreleri isteğe bağlı olarak, atamaları ek olarak veya bunların yerine öznitelik değerlerine göre kullanıcıları filtrelemek için kullanılabilir.

    >[!TIP]
    > Ayarlarını değiştirerek Kurumsal uygulama atamaları göre sağlama devre dışı bırakabilirsiniz [kapsam](active-directory-saas-app-provisioning.md#how-do-i-set-up-automatic-provisioning-to-an-application) menü sağlama ayarlar altında **tüm kullanıcılar ve gruplar eşitleme**. Bu seçenek ayrıca öznitelik tabanlı kapsam filtreleri kullanarak grup tabanlı atamaları kullanarak daha hızlı performans sağlar.  

* **Azure ad HCM uygulamalardan sağlama gelen ve Active Directory**. Zaman bir [HCM uygulama Workday gibi](active-directory-saas-workday-tutorial.md) kaynak sistemi kapsam filtreleri hangi kullanıcıların Active Directory veya Azure AD HCM uygulamasından sağlanması belirlemek için birincil yöntem şunlardır.

Varsayılan olarak, Azure AD sağlama bağlayıcılar yapılandırılmış öznitelik tabanlı kapsam filtreleri sahip değilsiniz. 

## <a name="scoping-filter-construction"></a>Kapsam filtresi oluşturma

Bir veya daha fazla kapsam filtresi oluşur *yan tümceleri*. Yan tümceleri, her kullanıcının öznitelikleri değerlendirerek kapsam filtresinden geçmesine izin verilen kullanıcıları belirleyin. Örneğin, yalnızca New York kullanıcıların uygulamaya sağlanan şekilde, bir kullanıcının "Durum" özniteliği "New York" eşittir gerektiren bir yan tümce olabilir. 

Tek bir yan tümce tek öznitelik değeri için tek bir koşul tanımlar. Birden çok yan tümcesi tek bir kapsam filtrede oluşturduysanız birlikte olarak bunlar "Ve" mantığı kullanarak hesaplanan. Başka bir deyişle, tüm yan tümceleri sağlanacak bir kullanıcı için sırayla "true" değerlendirmeniz gerekir.

Son olarak, birden çok kapsam filtreleri tek bir uygulama için oluşturulabilir. Birden çok kapsam filtre varsa, birlikte olarak bunlar "Veya" mantığı kullanarak hesaplanan. Bu, herhangi bir yapılandırılmış kapsam filtreleri yan tümceleri değerlendir "true" ise, kullanıcı sağlanan anlamına gelir.

Her bir kullanıcı veya grup Azure AD sağlama hizmeti tarafından işlenen her zaman ayrı ayrı her bir ölçüm filtre karşı değerlendirilir.

Örnek olarak, aşağıdaki ölçüm filtre göz önünde bulundurun:

![Kapsamı filtresi](./media/active-directory-saas-scoping-filters/scoping-filter.PNG) 

Bu kapsam filtresi göre kullanıcılar sağlanması için aşağıdaki ölçütleri karşılamalıdır:

* New York'taki olmaları gerekir.
* Bunlar mühendislik departmanında çalışmanız gerekir.
* Kullanıcıların şirket çalışan kimliği 1.000.000 ile 2,000,000 arasında olmalıdır.
* Kendi iş unvanı, null veya boş olmamalıdır.

## <a name="create-scoping-filters"></a>Kapsam filtreleri oluşturma
Kapsam filtreleri bağlayıcı sağlama her Azure AD kullanıcı için öznitelik eşlemelerini bir parçası olarak yapılandırılır. Aşağıdaki yordam, otomatik sağlamayı yukarı zaten ayarlanmış varsayar [desteklenen uygulamaların birini](active-directory-saas-tutorial-list.md) ve kapsanan bir filtre eklemeden.

### <a name="create-a-scoping-filter"></a>Bir kapsam filtresi oluşturma
1. İçinde [Azure portal](https://portal.azure.com)gidin **Azure Active Directory** > **kurumsal uygulamalar** > **tüm uygulamaları** bölümü.

2. Kendisi için yapılandırdığınız otomatik sağlamayı uygulamayı seçin: Örneğin, "ServiceNow".

3. Seçin **sağlama** sekmesi.

4. İçinde **eşlemeleri** bölümünde, bir kapsam filtresi için yapılandırmak istediğiniz eşlemeyi seçin: Örneğin, "eşitlemek Azure Active Directory Kullanıcıları için ServiceNow".

5. Seçin **kaynak nesne kapsamı** menüsü.

6. Seçin **kapsam Filtresi Ekle**.

7. Bir kaynak seçerek bir yan tümcesi tanımlamak **öznitelik adı**, bir **işleci**ve bir **öznitelik değeri** eşleştirilecek. Aşağıdaki işleçleri desteklenir:

   a. **EŞİTTİR**. Yan tümcesi döndürürse "true" değerlendirilen özniteliği giriş dizesi değerini tam olarak eşleşir (büyük küçük harf duyarlı).

   b. **EŞİT DEĞİLDİR**. Yan tümcesi "giriş dizesi değeri (büyük küçük harf duyarlı) değerlendirilen özniteliği eşleşmiyorsa true" değerini döndürür.

   c. **TRUE İSE**. Yan tümcesi "değerlendirilen özniteliği bir Boole değeri true içeriyorsa true" değerini döndürür.

   d. **YANLIŞ**. Yan tümcesi "değerlendirilen özniteliği false Boole değeri içeriyorsa true" değerini döndürür.

   e. **NULL**. Yan tümcesi "değerlendirilen öznitelik boş ise true" değerini döndürür.

   f. **NULL OLMAYAN**. Yan tümcesi "değerlendirilen öznitelik boş değilse true" değerini döndürür.

   g. **REGEX EŞLEŞME**. Yan tümcesi "değerlendirilen özniteliği bir normal ifade deseni eşleşiyorsa true" değerini döndürür. Örneğin: ([1-9][0-9]) eşleşen 10 ile 99 arasında bir sayı.

   h. **REGEX EŞLEŞMEDİĞİNDEN**. Yan tümcesi "değerlendirilen özniteliği bir normal ifade deseni eşleşmiyorsa true" değerini döndürür.

8. Seçin **Ekle Yeni kapsam yan tümcesi**.

9. İsteğe bağlı olarak, adım 7-8, daha fazla kapsam yan tümceleri eklemek için yineleyin.

10. İçinde **kapsamı filtresi başlık**, kapsam filtreniz için bir ad ekleyin.

11. **Tamam**’ı seçin.

12. Seçin **Tamam** yeniden **kapsam belirleme filtreleri** ekran. İsteğe bağlı olarak, adım 6-11, başka bir kapsam filtre eklemek için yineleyin.

13. Seçin **kaydetmek** üzerinde **eşleme özniteliği** ekran. 

>[!IMPORTANT] 
> Yeni bir kapsam filtresi Tetikleyicileri burada kaynak sistemdeki tüm kullanıcıların yeni kapsam filtresi karşı tekrar değerlendirilir bu uygulama için yeni bir tam eşitleme kaydediliyor. Uygulamada bir kullanıcı daha önce sağlama, ancak kapsam dışında düştüğünde kapsamına varsa, kendi hesabı devre dışı veya uygulamada sağlaması kaldırılıyor. sağlaması.


## <a name="related-articles"></a>İlgili makaleler
* [Azure Active Directory'de uygulama yönetimi için makale dizini](active-directory-apps-index.md)
* [Sağlama ve SaaS uygulamaları etkinleştirmektir kullanıcı otomatikleştirme](active-directory-saas-app-provisioning.md)
* [Kullanıcı sağlama öznitelik eşlemelerini özelleştirme](active-directory-saas-customizing-attribute-mappings.md)
* [Özellik eşlemeleri için ifade yazma](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Hesap sağlama bildirimleri](active-directory-saas-account-provisioning-notifications.md)
* [Kullanıcıların ve grupların Azure Active Directory'den uygulamalara otomatik sağlamayı etkinleştirmek için SCIM'yi kullanma](active-directory-scim-provisioning.md)
* [SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)

