---
title: "Azure Active Directory portalındaki riskli oturum açma işlemleri raporu | Microsoft Docs"
description: "Azure Active Directory portalındaki riskli oturum açma işlemleri raporu hakkında bilgi edinin"
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: 7728fcd7-3dd5-4b99-a0e4-949c69788c0f
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
ms.openlocfilehash: f66c4c0b7c391b3fc5ee042cc7a6643e46566d3f
ms.contentlocale: tr-tr
ms.lasthandoff: 08/04/2017

---
# <a name="risky-sign-ins-report-in-the-azure-active-directory-portal"></a>Azure Active Directory portalındaki riskli oturum açma işlemleri raporu

Azure Active Directory’de (Azure AD) güvenlik raporları ile ortamınızda güvenliği tehlikeye girmiş kullanıcı hesaplarının olasılığı hakkında bilgi sahibi olabilirsiniz. 

Azure AD, kullanıcı hesaplarınızla ilgili kuşkulu eylemleri algılar. Algılanan her eylem için *risk olayı* adlı bir kayıt oluşturulur. Daha ayrıntılı bilgi için bkz. [Azure Active Directory risk olayları](active-directory-identity-protection-risk-events.md). 

Algılanan risk olayları aşağıdakileri hesaplamak için kullanılır:

- **Riskli oturum açma işlemleri** - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir. Daha fazla bilgi için bkz. [Riskli oturum açma işlemleri](active-directory-identityprotection.md#risky-sign-ins). 

- **Riskli oldukları belirlenen kullanıcılar** - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir. Daha fazla bilgi için bkz. [Riskli oldukları belirlenen kullanıcılar](active-directory-identityprotection.md#users-flagged-for-risk).  

Güvenlik raporlarını, [Azure portalının](https://portal.azure.com) **Azure Active Directory** dikey penceresindeki **Güvenlik** bölümünde bulabilirsiniz. 

![Riskli Oturum Açma İşlemleri](./media/active-directory-reporting-security-risky-sign-ins/10.png)


## <a name="what-azure-ad-license-do-you-need-to-access-a-security-report"></a>Güvenlik raporuna erişebilmek için hangi Azure AD lisansınızın olması gerekir?  

Azure Active Directory'nin tüm sürümlerinde size riskli oturum açma işlemleri raporları sağlanır.  
Bununla birlikte, rapordaki ayrıntı düzeyi sürümler arasında değişiklik gösterir: 

- **Azure Active Directory Ücretsiz ve Temel sürümlerinde**, riskli oturum açmaların bir listesini zaten alırsınız. 

- **Azure Active Directory Premium 1** sürümü bu modeli genişleterek her raporda algılanmış olan temel risk olaylarından bazılarını incelemenize olanak tanır. 

- **Azure Active Directory Premium 2** sürümü temel risk olayları hakkında en ayrıntılı bilgileri sağlar ve ayrıca, yapılandırılmış risk düzeylerine otomatik olarak yanıt veren güvenlik ilkeleri yapılandırmanıza da olanak tanır.



## <a name="azure-active-directory-free-and-basic-edition"></a>Azure Active Directory ücretsiz ve temel sürümleri

Azure Active Directory ücretsiz ve temel sürümleri, kullanıcılarınızla ilgili algılanan riskli oturum açma işlemlerinin listesini sunar. Risk olayları raporu aşağıdakileri sağlar:

- **Kullanıcı** - Oturum açma işlemi sırasında kullanılan kullanıcının adı
- **IP** - Azure Active Directory'ye bağlanmak için kullanılan cihazın IP adresi
- **Konum** - Azure Active Directory'ye bağlanmak için kullanılan konum
- **Oturum açma saati** - Oturum açma işleminin gerçekleştirildiği saat
- **Durum** - Oturum açma durumu

Bu rapor size rapor verilerini indirmek için bir seçenek sağlar.

![Riskli Oturum Açma İşlemleri](./media/active-directory-reporting-security-risky-sign-ins/01.png)

Riskli oturum açma işlemi araştırmanıza göre, Azure Active Directory'ye aşağıdaki eylemlerle geri bildirim sağlayabilirsiniz:

- Çözümleme
- Yanlış pozitif olarak işaretleme
- Yoksayma
- Yeniden etkinleştirme

![Riskli Oturum Açma İşlemleri](./media/active-directory-reporting-security-risky-sign-ins/21.png)

Daha fazla ayrıntı için bkz. [Risk olaylarını elle kapatma](active-directory-identityprotection.md#closing-risk-events-manually).

## <a name="azure-active-directory-premium-editions"></a>Azure Active Directory premium sürümleri

Azure Active Directory premium sürümlerindeki riskli oturum açma işlemleri raporu aşağıdakileri içerir:

- Algılanan [risk olayı türleri](active-directory-identity-protection-risk-events.md) hakkında toplu bilgiler

- Raporu indirme seçeneği


![Riskli Oturum Açma İşlemleri](./media/active-directory-reporting-security-risky-sign-ins/456.png)


Bir risk olayını seçtiğinizde bu risk olayına ilişkin, aşağıdakileri gerçekleştirmenize olanak tanıyan ayrıntılı bir rapor görünümü açılır:

- Bir [kullanıcı riskini azaltma ilkesi](active-directory-identityprotection.md#user-risk-security-policy) yapılandırabilirsiniz.  

- Risk olayının algılanma zaman çizelgesini gözden geçirebilirsiniz.  

- Bu risk olayının hangi kullanıcılarla ilgili olarak algılandığının listesini gözden geçirebilirsiniz.

- [Risk olayını elle kapatabilir](active-directory-identityprotection.md#closing-risk-events-manually) ya da elle kapatılmış bir risk olayını yeniden etkinleştirebilirsiniz. 


![Riskli Oturum Açma İşlemleri](./media/active-directory-reporting-security-risky-sign-ins/457.png)

Bir kullanıcıyı seçtiğinizde bu kullanıcıya ilişkin, aşağıdakileri gerçekleştirmenize olanak tanıyan ayrıntılı bir rapor görünümü açılır:

- Tüm oturum açma işlemleri görünümünü açabilirsiniz.

- Kullanıcının parolasını sıfırlayabilirsiniz.

- Tüm olayları kapatabilirsiniz.

- Kullanıcıya ilişkin bildirilmiş risk olaylarını araştırabilirsiniz. 


![Riskli Oturum Açma İşlemleri](./media/active-directory-reporting-security-risky-sign-ins/324.png)


Bir risk olayını araştırmak için, söz konusu olayı listeden seçin.  
Bu risk olayına ilişkin **Ayrıntılar** dikey penceresi açılır. **Ayrıntılar** dikey penceresinde, [risk olayını elle kapatma](active-directory-identityprotection.md#closing-risk-events-manually) ve elle kapatılmış risk olayını yeniden etkinleştirme seçenekleri sunulur. 


![Riskli Oturum Açma İşlemleri](./media/active-directory-reporting-security-risky-sign-ins/325.png)





## <a name="next-steps"></a>Sonraki adımlar

- Azure Active Directory Kimlik Koruması hakkında daha fazla bilgi için bkz. [Azure Active Directory Kimlik Koruması](active-directory-identityprotection.md).


