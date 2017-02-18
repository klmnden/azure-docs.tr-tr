---
title: "Azure Active Directory Ücretsiz ve Temel sürümünde güvenlik raporlama - önizleme | Microsoft Docs"
description: "Azure Active Directory önizlemesinde kullanılabilen çeşitli raporları listeler"
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: 6141a333-38db-478a-927e-526f1e7614f4
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/19/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: fa5a6dab1e76d62956cbfdd9b8b0f64c014696db
ms.openlocfilehash: c1a3953c79cfac9d6f55b38971ee2b79ff4244eb


---
# <a name="security-reporting-in-the-azure-active-directory-free-and-basic-edition---preview"></a>Azure Active Directory Ücretsiz ve Temel sürümünde güvenlik raporlama - önizleme

Azure Active Directory [önizleme](active-directory-preview-explainer.md) sürümünde güvenlik raporları ile ortamınızda güvenliği tehlikeye girmiş kullanıcı hesaplarının olasılığı hakkında bilgi sahibi olabilirsiniz. 

Azure Active Directory, kullanıcı hesaplarınızla ilgili kuşkulu eylemleri algılar. Algılanan her eylem için *risk olayı* adlı bir kayıt oluşturulur. Daha ayrıntılı bilgi için bkz. [Azure Active Directory risk olayları](active-directory-identity-protection-risk-events.md). 

Algılanan risk olayları aşağıdakileri hesaplamak için kullanılır:

- **Riskli oturum açma işlemleri** - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir. Daha fazla bilgi için bkz. [Riskli oturum açma işlemleri](active-directory-identityprotection.md#risky-sign-ins). 

- **Riskli oldukları belirlenen kullanıcılar** - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir. Daha fazla bilgi için bkz. [Riskli oldukları belirlenen kullanıcılar](active-directory-identityprotection.md#users-flagged-for-risk).  


## <a name="risky-sign-ins-report"></a>Riskli oturum açma işlemleri raporu

Azure Active Directory ücretsiz ve temel sürümleri, kullanıcılarınız için algılanıp bildirilen riskli oturum açma işlemlerinin listesini sunar. Risk olayları raporu aşağıdakileri sağlar:

- **Kullanıcı** - Oturum açma işlemi sırasında kullanılan kullanıcının adı
- **IP** - Azure Active Directory'ye bağlanmak için kullanılan cihazın IP adresi
- **Konum** - Azure Active Directory'ye bağlanmak için kullanılan konum
- **Oturum açma saati** - Oturum açma işleminin gerçekleştirildiği saat
- **Durum** - Oturum açma durumu

Bu rapor size rapor verilerini indirmek için bir seçenek sağlar.

![Raporlama](./media/active-directory-reporting-security-azure-portal-free-basic/01.png)

Riskli oturum açma işlemi araştırmanıza göre, Azure Active Directory'ye aşağıdaki eylemlerle geri bildirim sağlayabilirsiniz:

- Çözümleme
- Yanlış pozitif olarak işaretleme
- Yoksayma
- Yeniden etkinleştirme

![Raporlama](./media/active-directory-reporting-security-azure-portal-free-basic/21.png)

Daha fazla ayrıntı için bkz. [Risk olaylarını elle kapatma](active-directory-identityprotection.md#closing-risk-events-manually).


## <a name="users-at-risk-report"></a>Risk altındaki kullanıcılar raporu

Azure Active Directory ücretsiz sürümü, tehlikeye girmiş olabilecek kullanıcı hesaplarının bir listesini sağlar. 


![Raporlama](./media/active-directory-reporting-security-azure-portal-free-basic/03.png)

Listede bir kullanıcıya tıklanması ilgili kullanıcı verileri dikey penceresini açar.
Risk altındaki kullanıcılar için kullanıcının oturum açma geçmişini gözden geçirin ve gerekirse parolayı sıfırlayın.

![Raporlama](./media/active-directory-reporting-security-azure-portal-free-basic/46.png)



## <a name="next-steps"></a>Sonraki adımlar

- Azure Active Directory raporlama hakkında daha fazla ayrıntı için bkz. [Azure Active Directory Raporlama Kılavuzu](active-directory-reporting-guide.md).
- Azure Active Directory Kimlik Koruması hakkında daha fazla bilgi için bkz. [Azure Active Directory Kimlik Koruması](active-directory-identityprotection.md).




<!--HONumber=Jan17_HO3-->


