---
title: Azure Active Directory portalındaki riskli oturum açma işlemleri raporu | Microsoft Docs
description: Azure Active Directory portalındaki riskli oturum açma işlemleri raporu hakkında bilgi edinin
services: active-directory
author: MarkusVi
manager: daveba
ms.assetid: 7728fcd7-3dd5-4b99-a0e4-949c69788c0f
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 40e125f8e1e7909c5866a03c0571f49ec42d690a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60287532"
---
# <a name="risky-sign-ins-report-in-the-azure-active-directory-portal"></a>Azure Active Directory portalındaki riskli oturum açma işlemleri raporu

Azure Active Directory (Azure AD), kullanıcı hesaplarınızla ilgili kuşkulu eylemleri algılar. Algılanan her eylem için bir kaydı olarak adlandırılan bir **risk olayı** oluşturulur. Daha fazla ayrıntı için [Azure AD'ye risk olayları](concept-risk-events.md). 

Güvenlik raporlarını erişebileceğiniz [Azure portalında](https://portal.azure.com) seçerek **Azure Active Directory** dikey penceresinde ve ardından giderek **güvenlik** bölümü. 

Risk olaylarına göre hesaplanan iki farklı güvenlik raporu vardır:

- **Riskli oturum açma işlemleri** - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir.

- **Riskli oldukları belirlenen kullanıcılar** - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir. 

![Riskli Oturum Açma İşlemleri](./media/concept-risky-sign-ins/10.png)

Bu risk olaylarını tetiklemek ilkelerinin nasıl yapılandırılacağını öğrenmek için bkz: [kullanıcı riski ilkesi yapılandırma](../identity-protection/howto-user-risk-policy.md).  

## <a name="who-can-access-the-risky-sign-ins-report"></a>Riskli oturum açma işlemleri raporu erişebilecek mi?

Riskli oturum açma işlemleri raporları, kullanıcıların aşağıdaki rollerin kullanılabilir:

- Güvenlik Yöneticisi
- Genel Yönetici
- Güvenlik Okuyucusu

Azure Active Directory'de bir kullanıcıya yönetici rollerini atama hakkında bilgi edinmek için [Azure Active Directory'de yönetici rolleri görüntüleyin ve Ata](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-manage-roles-portal).

## <a name="what-azure-ad-license-do-you-need-to-access-a-security-report"></a>Güvenlik raporuna erişebilmek için hangi Azure AD lisansınızın olması gerekir?  

Azure AD'in tüm sürümlerinde riskli oturum açma işlemleri raporları sağlanır. Bununla birlikte, rapordaki ayrıntı düzeyi sürümler arasında değişiklik gösterir: 

- İçinde **Azure Active Directory ücretsiz ve temel sürümlerinde**, riskli oturum açmaların bir listesini alın. 

- Ayrıca, **Azure Active Directory Premium 1** edition, her raporda algılanmış temel risk olaylarından bazılarını incelemenize olanak sağlar. 

- **Azure Active Directory Premium 2** sürümü, tüm temel risk olayları hakkında en ayrıntılı bilgileri sağlar ve ayrıca, yapılandırılmış risk düzeylerine otomatik olarak yanıt veren güvenlik ilkeleri yapılandırmanıza da olanak tanır.

## <a name="risky-sign-ins-report-for-azure-ad-free-and-basic-edition"></a>Riskli oturum açma işlemleri raporu için Azure AD ücretsiz ve temel sürümünde

Azure AD ücretsiz ve temel sürümleri, kullanıcılarınız için riskli oturum açma, algılanmış olan işlemleri bir listesini sağlar. Her kayıt, aşağıdaki öznitelikler içerir:

- **Kullanıcı** -oturum açma işlemi sırasında kullanılan kullanıcının adı.
- **IP** -Azure Active Directory'ye bağlanmak için kullanılan cihazın IP adresi.
- **Konum** -Azure Active Directory'ye bağlanmak için kullanılan konum. Bu izlemeler, kayıt defteri verisi, geriye doğru arama ve diğer bilgileri göre bir en iyi çaba değeridir.
- **Oturum açma saati** - Oturum açma işleminin gerçekleştirildiği saat
- **Durum** - Oturum açma durumu

![Riskli Oturum Açma İşlemleri](./media/concept-risky-sign-ins/01.png)

Araştırmanızı riskli oturum açma bağlı olarak, aşağıdaki eylemleri gerçekleştirerek Azure AD'ye geri bildirim sağlayabilirsiniz:

- Çözümleme
- Yanlış pozitif olarak işaretleme
- Yoksayma
- Yeniden etkinleştirme

![Riskli Oturum Açma İşlemleri](./media/concept-risky-sign-ins/21.png)

Bu rapor ayrıca size şu seçeneği sağlar:

- Kaynak arama
- Rapor verilerini indir

![Riskli Oturum Açma İşlemleri](./media/concept-risky-sign-ins/93.png)

## <a name="risky-sign-ins-report-for-azure-ad-premium-editions"></a>Azure AD premium sürümleri için riskli oturum açma işlemleri raporu

Azure AD premium sürümlerindeki riskli oturum açma işlemleri raporu aşağıdakileri sağlar:

- Hakkında toplu bilgiler [risk olayı türleri](concept-risk-events.md) algılanan. İle **Azure AD Premium P1 edition**, lisansınızı tarafından kapsanmaz algılamalar risk olayı görünür **ek risk algılandı ile oturum açma**. İle **Azure AD Premium P2 sürümünü**, temel alınan tüm algılamalar hakkında en ayrıntılı bilgileri alın.

- Raporu indirme seçeneği

![Riskli Oturum Açma İşlemleri](./media/concept-risky-sign-ins/456.png)

Bir risk olayını seçtiğinizde bu risk olayına ilişkin, aşağıdakileri gerçekleştirmenize olanak tanıyan ayrıntılı bir rapor görünümü açılır:

- Bir [kullanıcı riskini azaltma ilkesi](../identity-protection/howto-user-risk-policy.md) yapılandırabilirsiniz.  

- Risk olayının algılanma zaman çizelgesini gözden geçirebilirsiniz.  

- Bu risk olayının hangi kullanıcılarla ilgili olarak algılandığının listesini gözden geçirebilirsiniz.

- Risk olayını elle kapatabilir. 

![Riskli Oturum Açma İşlemleri](./media/concept-risky-sign-ins/457.png)

> [!IMPORTANT]
> Bazı durumlarda, karşılık gelen oturum açma bir giriş olmadan bir risk olayını bulabilirsiniz [oturum açma işlemleri raporu](concept-sign-ins.md). Kimlik koruması her ikisi için risk değerlendirdiğinden budur **etkileşimli** ve **etkileşimli olmayan** oturum açma işlemleri, oysa yalnızca etkileşimli oturum açma oturum açma işlemleri raporu gösterir.

Bir kullanıcıyı seçtiğinizde bu kullanıcıya ilişkin, aşağıdakileri gerçekleştirmenize olanak tanıyan ayrıntılı bir rapor görünümü açılır:

- Tüm oturum açma işlemleri görünümünü açabilirsiniz.

- Kullanıcının parolasını sıfırlayabilirsiniz.

- Tüm olayları kapatabilirsiniz.

- Kullanıcıya ilişkin bildirilmiş risk olaylarını araştırabilirsiniz. 

![Riskli Oturum Açma İşlemleri](./media/concept-risky-sign-ins/324.png)

Bir risk olayını araştırmak için, söz konusu olayı listeden seçin.  
Bu risk olayına ilişkin **Ayrıntılar** dikey penceresi açılır. **Ayrıntılar** dikey penceresinde, risk olayını elle kapatma ve elle kapatılmış risk olayını yeniden etkinleştirme seçenekleri sunulur. 

![Riskli Oturum Açma İşlemleri](./media/concept-risky-sign-ins/325.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Kullanıcı riski ilkesi yapılandırma](../identity-protection/howto-user-risk-policy.md)
- [Riskini azaltma ilkesi yapılandırma](../identity-protection/howto-user-risk-policy.md)
- [Risk olayı türleri](concept-risk-events.md)
