---
title: "Azure Active Directory portalındaki riskli oturum açma işlemleri raporu - önizleme | Microsoft Belgeleri"
description: "Azure Active Directory portalındaki riskli oturum açma işlemleri raporunun önizleme sürümü hakkında bilgi edinin"
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: 7728fcd7-3dd5-4b99-a0e4-949c69788c0f
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/21/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 349109e0c12a1394f96529a94ab884eeb451d242
ms.openlocfilehash: 69b2166dcbc3e4abd99084b47907c90e157791de
ms.lasthandoff: 02/22/2017


---
# <a name="risky-sign-ins-report-in-the-azure-active-directory-portal---preview"></a>Azure Active Directory portalındaki riskli oturum açma işlemleri raporu - önizleme

Azure Active Directory [önizleme](active-directory-preview-explainer.md) sürümünde güvenlik raporları ile ortamınızda güvenliği tehlikeye girmiş kullanıcı hesaplarının olasılığı hakkında bilgi sahibi olabilirsiniz. 

Azure Active Directory, kullanıcı hesaplarınızla ilgili kuşkulu eylemleri algılar. Algılanan her eylem için *risk olayı* adlı bir kayıt oluşturulur. Daha ayrıntılı bilgi için bkz. [Azure Active Directory risk olayları](active-directory-identity-protection-risk-events.md). 

Algılanan risk olayları aşağıdakileri hesaplamak için kullanılır:

- **Riskli oturum açma işlemleri** - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir. Daha fazla bilgi için bkz. [Riskli oturum açma işlemleri](active-directory-identityprotection.md#risky-sign-ins). 

- **Riskli oldukları belirlenen kullanıcılar** - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir. Daha fazla bilgi için bkz. [Riskli oldukları belirlenen kullanıcılar](active-directory-identityprotection.md#users-flagged-for-risk).  

Güvenlik raporlarını, Azure portalında **Azure Active Directory** dikey penceresindeki **Güvenlik** bölümünde bulabilirsiniz. 

![Riskli Oturum Açma İşlemleri](./media/active-directory-reporting-security-risky-sign-ins/10.png)


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


