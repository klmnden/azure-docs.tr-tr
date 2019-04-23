---
title: Azure Active Directory B2C kullanıcı verilerini yönetme | Microsoft Docs
description: Silme veya Azure AD B2C kullanıcı verilerini dışarı aktarma konusunda bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 05/06/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 62846fe744e7295f58902481400ce91770c916da
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59798121"
---
# <a name="manage-user-data-in-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanıcı verilerini yönetme

 Bu makalede ele alınmaktadır tarafından sağlanan işlemler kullanarak Azure Active Directory (Azure AD) B2C kullanıcı verilerini nasıl yönetebileceğinizi [Azure Active Directory Graph API'si](/previous-versions/azure/ad/graph/api/api-catalog). Kullanıcı verileri yönetmek, silme veya denetim günlüklerinden verileri dışarı aktarma içerir.

[!INCLUDE [gdpr-intro-sentence.md](../../includes/gdpr-intro-sentence.md)]

## <a name="delete-user-data"></a>Kullanıcı verilerini sil

Kullanıcı verileri, Denetim günlüklerinde ve Azure AD B2C dizininde depolanır. Tüm kullanıcı denetim verileri 30 gün içinde Azure AD B2C için korunur. Bu 30 günlük süre içinde kullanıcı verilerini silmek istiyorsanız, kullanabileceğiniz [kullanıcı silme](/previous-versions/azure/ad/graph/api/users-operations#DeleteUser) işlemi. Bir silme işlemi, veri bulunduğu her Azure AD B2C kiracıları için gerekli değildir. 

Her Azure AD B2C kullanıcı nesne kimliği atanır. Nesne Kimliğini Azure AD B2C kullanıcı verilerini silmek için kullanabilmeniz için benzersiz bir tanımlayıcı sağlar. Mimarinizi bağlı olarak, Finans, pazarlama gibi diğer hizmetlerde, kullanışlı bağıntı tanımlayıcısı nesne kimliği olabilir ve müşteri ilişkileri yönetim veritabanları. 

Bir kullanıcı için nesne Kimliğini almak için en doğru şekilde, bir Azure AD B2C ile kimlik doğrulaması yolculuğunun bir parçası olarak elde edilir. Çevrimdışı bir süreç başka yöntemler kullanarak veriler için geçerli bir isteği bir kullanıcıdan alırsanız bir müşteri hizmetleri desteği göre arama gibi aracı, olabilir kullanıcıyı bulun ve ilişkili nesne kimliğini not alın. 

Aşağıdaki örnekte, olası veri silme akış gösterilmektedir:

1. Kullanıcı oturum açtığında ve seçer **verilerimi Sil**.
2. Uygulama içinde bir yönetim bölümündeki uygulama verilerini silmek için bir seçenek sunar.
3. Uygulamayı bir Azure AD B2C kimlik doğrulaması zorlar. Azure AD B2C, kullanıcının uygulamaya nesne Kimliğine sahip bir belirteç sağlar. 
4. Belirteç, uygulama ve kullanıcı verilerini Azure AD Graph API'sine yapılan bir çağrı ile silmek için kullanılan kimliği nesne tarafından alınır. Azure AD Graph API, kullanıcı verilerini siler ve bir durum kodu 200 Tamam döndürür.
5. Uygulama kullanıcı verilerini silmeyi nesne kimliği veya diğer tanımlayıcıları kullanarak gerektiği gibi diğer Kurumsal sistemlere düzenler.
6. Uygulama verileri silinmesini onaylıyor ve kullanıcıya sıradaki adımları sunmaktadır.

## <a name="export-customer-data"></a>Müşteri verilerini dışarı aktarma

Müşteri verileri, Azure AD B2C'den dışarı aktarma işleminin silme işlemine benzer.

Azure AD B2C kullanıcı verilerini sınırlıdır:

- **Azure Active Directory'de depolanan verileri**: Nesne kimliği veya tüm oturum açma adı, e-posta adresi veya kullanıcı adı gibi kullanarak bir Azure AD B2C kimlik doğrulaması kullanıcı yolculuğu veri alabilir. 
- **Kullanıcıya özgü denetim olayları raporu**: Nesne Kimliği'ni kullanarak verileri dizinleyebilirsiniz.

Bir dışarı aktarma veri akışı aşağıdaki örnekte açıklanan adımları uygulama tarafından gerçekleştirilen olarak da bir arka uç işleme veya dizindeki Yönetici rolüne sahip bir kullanıcı tarafından gerçekleştirilebilir:

1. Kullanıcı uygulamayı açar. Azure AD B2C, Azure multi-Factor Authentication ile kimlik doğrulaması gerekirse zorlar.
2. Uygulama, kullanıcı öznitelikleri almak için Azure AD Graph API işlemini çağırmak için kullanıcı kimlik bilgilerini kullanır. Azure AD Graph API, öznitelik verileri JSON biçiminde sağlar. Şemasına bağlı olarak, bir kullanıcı ile ilgili tüm kişisel verileri içerecek şekilde kimliği belirteç içeriği ayarlayabilirsiniz.
3. Uygulama kullanıcı Denetim etkinliği alır. Azure AD Graph API uygulamasına olay verilerini sağlar.
4. Uygulama verileri bir araya toplar ve kullanıcı tarafından kullanılabilir hale getirir.

## <a name="next-steps"></a>Sonraki adımlar

- Kullanıcıların uygulamanızı nasıl eriştiğini yönetme konusunda bilgi almak için bkz: [kullanıcı erişimini yönetme](manage-user-access.md).




