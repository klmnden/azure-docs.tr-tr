---
title: Azure Active Directory portalında risk güvenliği raporu için işaretlenmiş kullanıcılar | Microsoft Docs
description: Azure Active Directory portalında risk güvenliği için işaretlenmiş kullanıcılar hakkında bilgi edinin
services: active-directory
author: MarkusVi
manager: daveba
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 01/17/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 463f5c2d03cd96089342aa9b22ef85ebc05aa909
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60438144"
---
# <a name="users-flagged-for-risk-report-in-the-azure-portal"></a>Azure portalında risk için işaretlenmiş kullanıcılar

Azure Active Directory (Azure AD), kullanıcı hesaplarınızla ilgili kuşkulu eylemleri algılar. Algılanan her eylem için bir kaydı olarak adlandırılan bir [risk olayı](concept-risk-events.md) oluşturulur.

Güvenlik raporlarını erişebileceğiniz [Azure portalında](https://portal.azure.com) seçerek **Azure Active Directory** dikey penceresinde ve ardından giderek **güvenlik** bölümü. 

Algılanan risk olayları aşağıdakileri hesaplamak için kullanılır:

- **Riskli oturum açma işlemleri** - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir. 

- **Riskli oldukları belirlenen kullanıcılar** - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir. 

Bu risk olaylarını tetiklemek ilkelerinin nasıl yapılandırılacağını öğrenmek için bkz: [kullanıcı riski ilkesi yapılandırma](../identity-protection/howto-user-risk-policy.md). 

![Riskli Oturum Açma İşlemleri](./media/concept-user-at-risk/10.png)


## <a name="what-azure-ad-license-do-you-need-to-access-the-users-at-risk-report"></a>Hangi Azure AD lisansınızın risk altındaki kullanıcılar raporu erişmeye ihtiyacınız var?  

Azure Active Directory'nin tüm sürümlerinde size riskli oldukları belirlenen kullanıcılar ve risk raporları sağlanır. Bununla birlikte, rapordaki ayrıntı düzeyi sürümler arasında değişiklik gösterir: 

- İçinde **Azure Active Directory ücretsiz ve temel sürümlerinde**, riskli olduğu belirlenen kullanıcıların listesini alın. 

- Ayrıca, **Azure Active Directory Premium 1** edition, her raporda algılanmış temel risk olaylarından bazılarını incelemenize olanak sağlar. 

- **Azure Active Directory Premium 2** sürümü, tüm temel risk olayları hakkında en ayrıntılı bilgileri sağlar ve ayrıca, yapılandırılmış risk düzeylerine otomatik olarak yanıt veren güvenlik ilkeleri yapılandırmanıza da olanak tanır.


## <a name="users-at-risk-report-for-azure-ad-free-and-basic-editions"></a>Risk altındaki kullanıcılar raporu için Azure AD ücretsiz ve temel sürümleri

Ücretsiz Azure AD'de risk raporu için işaretlenmiş kullanıcılar ve basic sürümleri, tehlikeye girmiş olabilecek kullanıcı hesaplarının bir listesini sağlar. 

![Riskli Oturum Açma İşlemleri](./media/concept-user-at-risk/03.png)

Bir kullanıcının seçilmesi, oturum açma bilgilerini sağlar. Risk altındaki kullanıcılarla ilgili olarak kullanıcının oturum açma geçmişini gözden geçirebilir ve gerekirse parolasını sıfırlayabilirsiniz.

Bu iletişim kutusu size şu seçeneği sunar:

- Raporu indirme
- Kullanıcılarda arama

    ![Riskli Oturum Açma İşlemleri](./media/concept-user-at-risk/16.png)

Daha ayrıntılı bilgi için bir premium lisansı gereklidir.

## <a name="users-at-risk-report-for-azure-ad-premium-editions"></a>Risk altındaki kullanıcılar raporu için Azure AD premium sürümleri

Azure AD premium sürümlerinde risk için işaretlenmiş kullanıcılar aşağıdakileri içerir:

- Tehlikeye girmiş olabilecek kullanıcı hesaplarının listesi 

- Algılanan [risk olayı türleri](concept-risk-events.md) hakkında toplu bilgiler

- Raporu indirme seçeneği

- [Kullanıcı riskini azaltma ilkesi](../identity-protection/howto-user-risk-policy.md) yapılandırma seçeneği  

![Riskli Oturum Açma İşlemleri](./media/concept-user-at-risk/71.png)

Bir kullanıcıyı seçtiğinizde bu kullanıcıya ilişkin, aşağıdakileri gerçekleştirmenize olanak tanıyan ayrıntılı bir rapor görünümü açılır:

- Tüm oturum açma işlemleri görünümünü açabilirsiniz.

- Kullanıcının parolasını sıfırlayabilirsiniz.

- Tüm olayları kapatabilirsiniz.

- Kullanıcıya ilişkin bildirilmiş risk olaylarını araştırabilirsiniz. 

![Riskli Oturum Açma İşlemleri](./media/concept-user-at-risk/324.png)

Bir risk olayını araştırmak için listeden bir olay seçerek bu olayın **Ayrıntılar** dikey penceresini açın. **Ayrıntılar** dikey penceresinde, risk olayını elle kapatma ve elle kapatılmış risk olayını yeniden etkinleştirme seçenekleri sunulur. 

![Riskli Oturum Açma İşlemleri](./media/concept-user-at-risk/325.png)


## <a name="next-steps"></a>Sonraki adımlar

- [Kullanıcı riski ilkesi yapılandırma](../identity-protection/howto-user-risk-policy.md)
- [Riskini azaltma ilkesi yapılandırma](../identity-protection/howto-user-risk-policy.md)
- [Azure Active Directory kimlik koruması](../active-directory-identityprotection.md)

