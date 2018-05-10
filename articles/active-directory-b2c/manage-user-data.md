---
title: Azure AD B2C kullanıcı verileri yönetmek | Microsoft Docs
description: Silme veya Azure AD B2C kullanıcı verilerini dışarı aktarma öğrenin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 05/06/2018
ms.author: davidmu
ms.openlocfilehash: 414221c3e4942801b5792835d520ec936c8c4bbb
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="manage-user-data-in-azure-ad-b2c"></a>Azure AD B2C kullanıcı verilerini yönetme

 Bu makalede tarafından sağlanan işlemleri kullanarak Azure Active Directory (AD) B2C kullanarak kullanıcı verilerini yönetmek hakkında bilgiler sağlanmaktadır [Azure Active Directory grafik API'si](https://msdn.microsoft.com/en-us/library/azure/ad/graph/api/api-catalog). Kullanıcı verileri yönetme verileri silmek veya denetim günlüklerinden verileri dışarı aktarma özelliğini içerir.

[!INCLUDE [gdpr-intro-sentence.md](../../includes/gdpr-intro-sentence.md)]

## <a name="delete-user-data"></a>Kullanıcı verilerini sil

Kullanıcı verileri Azure AD B2C dizini ve denetim günlüklerini depolanır. Tüm kullanıcı denetim verilerini Azure AD B2C'de 30 gün içinde veri saklama için tutulur. Bu 30 gün içinde kullanıcı verilerini silmek istiyorsanız kullanabileceğiniz [kullanıcı silme](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#DeleteUser) işlemi. Bir silme işlemi veri bulunduğu her Azure AD B2C kiracılar için gereklidir. 

Azure AD B2C'de her kullanıcı bir nesne kimliği atanır Nesne kimliği, Azure AD B2C kullanıcı verilerini silmek için kullanmanız gereken benzersiz bir tanımlayıcı sağlar.  Mimarinizi bağlı olarak, finansal, pazarlama gibi başka hizmetleri arasında yararlı bağıntı tanımlayıcısı nesne kimliği olabilir ve müşteri ilişkileri yönetim veritabanları.  

Bir kullanıcı için nesne kimliği almak için en doğru şekilde Azure AD B2C ile bir kimlik doğrulama gezisine parçası olarak elde edilir.  Veriler için geçerli bir istek diğer yöntemleri, çevrimdışı bir işlemdir kullanan bir kullanıcıdan alınan bir müşteri hizmetleri desteği göre arama gibi Aracısı, olabilir kullanıcıyı bulun ve ilişkili bir nesne kimliğini not edin gerekli 

Aşağıdaki örnek, olası veri silme akışı gösterir:

1. Kullanıcı oturum açtığında ve seçer **verilerimi silme**.
2. Uygulama Yönetim bölümünde uygulamanın içinde verileri silmek için bir seçenek sunar.
3. Uygulamayı bir kimlik doğrulaması Azure AD B2C zorlar. Azure AD B2C, kullanıcının uygulamayı nesne Kimliğine sahip bir belirteç sağlar. 
4. Belirteç, uygulama ve Azure AD Graph API çağrısı aracılığıyla kullanıcı verilerini silmek için kullanılan kimliği nesnesi tarafından alınır. Azure AD grafik API'si, kullanıcı verilerini siler ve bir durum kodu 200 Tamam döndürür.
5. Nesne kimliği veya diğer tanımlayıcıları kullanarak gerektiği gibi uygulama diğer kuruluş sistemlerinde kullanıcı verilerinin silinmesini yönetir.
6. Uygulama verileri silinmesini onaylar ve kullanıcı için sonraki adımları sağlar.

## <a name="export-customer-data"></a>Müşteri verileri dışarı aktarma

Müşteri verileri Azure AD B2C ' dışarı aktarma işlemi silme işlemine benzer.

Azure AD B2C kullanıcı verilerini sınırlıdır:

- **Azure Active Directory'de depolanan verileri** -veri nesne kimliği veya herhangi bir oturum açma adı e-posta veya kullanıcı adı gibi kullanarak bir Azure AD B2C kimlik doğrulama kullanıcı gezisine içinde alınabilir.  
- **Kullanıcıya özgü denetim olayları rapor** -veri dizine nesne kimliği kullanarak

Dışarı aktarma veri akışını aşağıdaki örnekte uygulama tarafından gerçekleştirilen olarak açıklanan adımları ayrıca bir arka uç işlemi veya dizinde bir yönetici rolüne sahip bir kullanıcı tarafından gerçekleştirilebilir:

1. Kullanıcı uygulamayı açar. Azure AD B2C, gerekirse çok faktörlü yetkilendirme ile kimlik doğrulaması zorlar.
2. Uygulamanın, kullanıcı öznitelikleri alma Azure AD Graph API işlemi çağırmak için kullanıcı kimlik bilgilerini kullanır. Azure AD Graph API JSON biçiminde öznitelik verilerini sağlar. Şemasına göre bir kullanıcı hakkında tüm kişisel verileri içerecek şekilde kimliği belirteç içerikleri ayarlanmış olabilir.
3. Uygulama, son kullanıcı Denetim etkinlik alır. Azure AD Graph API uygulamasına olay verileri sağlar.
4. Uygulama verileri toplar ve kullanıcı için kullanılabilir hale getirir.

## <a name="next-steps"></a>Sonraki adımlar

- Kullanıcıların uygulamanızda nasıl erişebileceğiniz yönetmeyi öğrenin [kullanıcı erişimini yönetme](manage-user-access.md)




