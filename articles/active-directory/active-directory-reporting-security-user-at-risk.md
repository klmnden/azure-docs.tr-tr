---
title: "Azure Active Directory portalındaki risk altındaki kullanıcılar güvenlik raporu - önizleme | Microsoft Belgeleri"
description: "Azure Active Directory portalındaki risk altındaki kullanıcılar güvenlik raporunun önizleme sürümü hakkında bilgi edinin"
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/21/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 349109e0c12a1394f96529a94ab884eeb451d242
ms.openlocfilehash: 48c504a9ed5bc4ef9f0bff889df031962c5bf6e8
ms.lasthandoff: 02/22/2017


---
# <a name="users-at-risk-security-report-in-the-azure-active-directory-portal---preview"></a>Azure Active Directory portalındaki risk altındaki kullanıcılar güvenlik raporu - önizleme

Azure Active Directory [önizleme](active-directory-preview-explainer.md) sürümünde güvenlik raporları ile ortamınızda güvenliği tehlikeye girmiş kullanıcı hesaplarının olasılığı hakkında bilgi sahibi olabilirsiniz. 

Azure Active Directory, kullanıcı hesaplarınızla ilgili kuşkulu eylemleri algılar. Algılanan her eylem için *risk olayı* adlı bir kayıt oluşturulur. Daha ayrıntılı bilgi için bkz. [Azure Active Directory risk olayları](active-directory-identity-protection-risk-events.md). 

Algılanan risk olayları aşağıdakileri hesaplamak için kullanılır:

- **Riskli oturum açma işlemleri** - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir. Daha fazla bilgi için bkz. [Riskli oturum açma işlemleri](active-directory-identityprotection.md#risky-sign-ins). 

- **Riskli oldukları belirlenen kullanıcılar** - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir. Daha fazla bilgi için bkz. [Riskli oldukları belirlenen kullanıcılar](active-directory-identityprotection.md#users-flagged-for-risk).  

Güvenlik raporlarını, Azure portalında **Azure Active Directory** dikey penceresindeki **Güvenlik** bölümünde bulabilirsiniz.  

![Riskli Oturum Açma İşlemleri](./media/active-directory-reporting-security-user-at-risk/10.png)

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


