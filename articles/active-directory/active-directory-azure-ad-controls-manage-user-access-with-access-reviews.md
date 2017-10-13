---
title: "Azure AD erişim gözden geçirmeleriyle kullanıcı erişimini yönetme| Microsoft Docs"
description: "Azure Active Directory erişim gözden geçirmeleri ile bir grup üyeliği veya bir uygulamaya atama aracılığıyla kullanıcı erişimini yönetmeyi öğrenin"
services: active-directory
documentationcenter: 
author: markwahl-msft
manager: femila
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/19/2017
ms.author: billmath
ms.openlocfilehash: 0459eb5cc71939202c8491f6b2714e28bd8e202d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-user-access-with-azure-ad-access-reviews"></a>Azure AD erişim gözden geçirmeleriyle kullanıcı erişimini yönetme

Azure Active Directory sayesinde, kullanıcıların kendilerinden veya karar verme yetkisi olan bir kişiden bir erişim gözden geçirmesine katılarak kullanıcı erişimini yeniden onaylamalarını (veya “kanıtlamalarını”) isteyebilir ve kullanıcıların uygun erişime sahip olduklarını kolayca doğrulayabilirsiniz.  Gözden geçirenler, Azure AD önerilerine dayanarak her kullanıcının erişiminin devam edip etmemesine yönelik girişler ekleyebilir. Bir erişim gözden geçirmesi tamamlandığında, değişiklikler yapabilir ve artık erişime ihtiyaç duymayan kullanıcıların erişimini kaldırabilirsiniz.

> [!NOTE]
> Tüm kullanıcı türlerinin erişimini yerine yalnızca konuk kullanıcıların erişimini gözden geçirmek istiyorsanız bkz. [Erişim gözden geçirmeleriyle konuk kullanıcı erişimini yönetme](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md).  Genel Yönetici gibi yönetim rollerindeki kullanıcı erişimini gözden geçirmek istiyorsanız bkz. [Azure AD PIM’de erişim gözden geçirmesi başlatma](active-directory-privileged-identity-management-how-to-start-security-review.md). 
>
>

## <a name="prerequisites"></a>Ön koşullar 

Erişim gözden geçirmeleri, EMS E5’e dahil edilen Azure Active Directory Premium P2 sürümü ile kullanılabilir. Daha fazla bilgi için bkz. [Azure Active Directory sürümleri](active-directory-editions.md).  Gözden geçirme oluşturmak, erişimi gözden geçirmek veya bir gözden geçirmeyi uygulamak için, bu özellikle etkileşime giren her kullanıcının bir lisansı olması gerekir.


## <a name="creating-and-performing-an-access-review"></a>Erişim gözden geçirmesi oluşturma ve gerçekleştirme

Bir erişim gözden geçirmesinde bir veya daha çok kullanıcı gözden geçiren olabilir.  

1. Azure Active Directory’de bir veya daha fazla üyesi olan bir grubu ya da bir veya daha fazla kullanıcı atanmış olan Azure Active Directory’ye bağlı bir uygulamayı seçin. 
2. Her kullanıcının kendi erişimini mi gözden geçireceğine yoksa bir veya daha fazla kullanıcının tüm kullanıcıların erişimini mi gözden geçireceğine karar verin.
3. Erişim gözden geçirmelerini gözden geçiren kişinin erişim panellerinde görünmesi için etkinleştirin.  Bir genel yönetici olarak, [erişim gözden geçirmeleri sayfasına](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/) gidin. 
4. Erişim gözden geçirmesini başlatın. [Erişim gözden geçirme oluşturma](active-directory-azure-ad-controls-create-access-review.md) konusunda daha fazla bilgi edinin.
5. Gözden geçirenlerden bilgileri girmelerini isteyin. Varsayılan olarak, tüm gözden geçirenler Azure AD’den [erişim gözden geçirmelerini gerçekleştirecekleri](active-directory-azure-ad-controls-perform-access-review.md) erişim paneline yönlendiren bir bağlantı içeren bir e-posta alır.
6. Gözden geçirenler bilgi girmediyse, Azure AD’nin onlara bir anımsatıcı göndermesini isteyebilirsiniz.  Varsayılan olarak, bitiş tarihine kadar olan sürenin yarısına ulaşıldığında, Azure AD henüz yanıt vermemiş olan gözden geçirenlere bir anımsatıcı gönderir.
7. Gözden geçirenler bilgileri girdikten sonra erişim gözden geçirmesini durdurun ve değişiklikleri uygulayın. [Erişim gözden geçirmeyi tamamlama](active-directory-azure-ad-controls-complete-access-review.md) konusunda daha fazla bilgi edinin.


## <a name="next-steps"></a>Sonraki adımlar

- [Bir grubun üyeleri veya bir uygulamaya erişim için erişim gözden geçirmesi oluşturma](active-directory-azure-ad-controls-create-access-review.md)




