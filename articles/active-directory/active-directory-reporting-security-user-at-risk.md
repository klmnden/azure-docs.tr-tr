---
title: "Azure Active Directory portalındaki risk altındaki kullanıcılar güvenlik raporu | Microsoft Docs"
description: "Azure Active Directory portalındaki risk altındaki kullanıcılar güvenlik raporu hakkında bilgi edinin"
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/01/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.translationtype: HT
ms.sourcegitcommit: 8b857b4a629618d84f66da28d46f79c2b74171df
ms.openlocfilehash: bfcaee441c54453677e7747b0bca55a8afc59391
ms.contentlocale: tr-tr
ms.lasthandoff: 08/04/2017

---
# <a name="users-at-risk-security-report-in-the-azure-active-directory-portal"></a>Azure Active Directory portalındaki risk altındaki kullanıcılar güvenlik raporu

Azure Active Directory’de (Azure AD) güvenlik raporları ile ortamınızda güvenliği tehlikeye girmiş kullanıcı hesaplarının olasılığı hakkında bilgi sahibi olabilirsiniz. 

Azure Active Directory, kullanıcı hesaplarınızla ilgili kuşkulu eylemleri algılar. Algılanan her eylem için *risk olayı* adlı bir kayıt oluşturulur. Daha ayrıntılı bilgi için bkz. [Azure Active Directory risk olayları](active-directory-identity-protection-risk-events.md). 

Algılanan risk olayları aşağıdakileri hesaplamak için kullanılır:

- **Riskli oturum açma işlemleri** - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir. Daha fazla bilgi için bkz. [Riskli oturum açma işlemleri](active-directory-identityprotection.md#risky-sign-ins). 

- **Riskli oldukları belirlenen kullanıcılar** - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir. Daha fazla bilgi için bkz. [Riskli oldukları belirlenen kullanıcılar](active-directory-identityprotection.md#users-flagged-for-risk).  

Güvenlik raporlarını, Azure portalında **Azure Active Directory** dikey penceresindeki **Güvenlik** bölümünde bulabilirsiniz.  

![Riskli Oturum Açma İşlemleri](./media/active-directory-reporting-security-user-at-risk/10.png)



## <a name="what-azure-ad-license-do-you-need-to-access-a-security-report"></a>Güvenlik raporuna erişebilmek için hangi Azure AD lisansınızın olması gerekir?  

Azure Active Directory'nin tüm sürümlerinde size riskli oldukları belirlenen kullanıcılar ve risk raporları sağlanır.  
Bununla birlikte, rapordaki ayrıntı düzeyi sürümler arasında değişiklik gösterir: 

- **Azure Active Directory Ücretsiz ve Temel sürümlerinde**, riskli olduğu belirlenen kullanıcıların listesini zaten alırsınız. 

- **Azure Active Directory Premium 1** sürümü bu modeli genişleterek her raporda algılanmış olan temel risk olaylarından bazılarını incelemenize olanak tanır. 

- **Azure Active Directory Premium 2** sürümü temel risk olayları hakkında en ayrıntılı bilgileri sağlar ve ayrıca, yapılandırılmış risk düzeylerine otomatik olarak yanıt veren güvenlik ilkeleri yapılandırmanıza da olanak tanır.



## <a name="azure-active-directory-free-and-basic-edition"></a>Azure Active Directory ücretsiz ve temel sürümleri

Azure Active Directory ücretsiz ve temel sürümlerindeki risk altındaki kullanıcılar raporu, tehlikeye girmiş olabilecek kullanıcı hesaplarının bir listesini sağlar. 


![Riskli Oturum Açma İşlemleri](./media/active-directory-reporting-security-user-at-risk/03.png)

Bir kullanıcının seçilmesi, ilgili kullanıcı verileri dikey penceresini açar.
Risk altındaki kullanıcılarla ilgili olarak kullanıcının oturum açma geçmişini gözden geçirebilir ve gerekirse parolasını sıfırlayabilirsiniz.

![Riskli Oturum Açma İşlemleri](./media/active-directory-reporting-security-user-at-risk/46.png)

## <a name="azure-active-directory-premium-editions"></a>Azure Active Directory premium sürümleri

Azure Active Directory premium sürümlerindeki risk altındaki kullanıcılar raporu aşağıdakileri içerir:

- Tehlikeye girmiş olabilecek [kullanıcı hesaplarının listesi](active-directory-identityprotection.md#users-flagged-for-risk) 

- Algılanan [risk olayı türleri](active-directory-identity-protection-risk-events.md) hakkında toplu bilgiler

- Raporu indirme seçeneği

- [Kullanıcı riskini azaltma ilkesi](active-directory-identityprotection.md#user-risk-security-policy) yapılandırma seçeneği  


![Riskli Oturum Açma İşlemleri](./media/active-directory-reporting-security-user-at-risk/71.png)

Bir kullanıcıyı seçtiğinizde bu kullanıcıya ilişkin, aşağıdakileri gerçekleştirmenize olanak tanıyan ayrıntılı bir rapor görünümü açılır:

- Tüm oturum açma işlemleri görünümünü açabilirsiniz.

- Kullanıcının parolasını sıfırlayabilirsiniz.

- Tüm olayları kapatabilirsiniz.

- Kullanıcıya ilişkin bildirilmiş risk olaylarını araştırabilirsiniz. 


![Riskli Oturum Açma İşlemleri](./media/active-directory-reporting-security-user-at-risk/324.png)


Bir risk olayını araştırmak için, söz konusu olayı listeden seçin.  
Bu risk olayına ilişkin **Ayrıntılar** dikey penceresi açılır. **Ayrıntılar** dikey penceresinde, [risk olayını elle kapatma](active-directory-identityprotection.md#closing-risk-events-manually) ve elle kapatılmış risk olayını yeniden etkinleştirme seçenekleri sunulur. 


![Riskli Oturum Açma İşlemleri](./media/active-directory-reporting-security-user-at-risk/325.png)



## <a name="next-steps"></a>Sonraki adımlar

- Azure Active Directory Kimlik Koruması hakkında daha fazla bilgi için bkz. [Azure Active Directory Kimlik Koruması](active-directory-identityprotection.md).


