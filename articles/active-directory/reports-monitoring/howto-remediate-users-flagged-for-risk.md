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
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7209f468f493e226fae22ccd260e8ceb2e570494
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60286666"
---
# <a name="remediate-users-flagged-for-risk-in-the-azure-active-directory-portal"></a>Azure Active Directory portalında riskli olarak işaretlenmiş kullanıcıları düzeltme

Azure Active Directory (Azure AD) güvenlik raporları ile ortamınızda güvenliği aşılan kullanıcı hesaplarının olasılığı ölçer. Riskli olduğu belirlenen bir kullanıcı gizliliği bozulmuş olabilecek bir kullanıcı hesabının göstergesidir.

Microsoft ortamınızın güvenliğini korumak için çalışmaktadır. Bu çalışma kapsamında, Microsoft alışılmadık veya bilinen saldırı düzenleriyle tutarlı etkinlikleri sürekli izler. 

Bazı kullanıcılarınızın hesaplarına yetkisiz erişimi gösteriyor olabilir alışılmadık etkinlikler algılanırsa, eyleme sağlayacak bildirimler alırsınız. Bu, Microsoft'un kendi sistemlerinde aşıldığı anlamına gelmez.

## <a name="access-the-users-flagged-for-risk-report"></a>Riskli olarak işaretlenen kullanıcılar raporuna erişme

Risk için işaretlenmiş kullanıcıları gözden geçirebilirsiniz [risk altındaki kullanıcılar raporu](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RiskyUsers) Azure portalında. Azure AD yoksa, ücretsiz, kaydolabilirsiniz [ https://aka.ms/AccessAAD ](https://aka.ms/AccessAAD). 

Risk raporu için işaretlenmiş kullanıcılar, her kullanıcı için aşağıdaki eylemleri gerçekleştirebilirsiniz:

- Geçici parola oluşturma
- Kullanıcının bir sonraki defa oturum açtığında parolasını güvenli şekilde sıfırlamasını zorunlu tutma
- Herhangi bir düzeltme eylemi gerçekleştirmeden kullanıcı riskini kapatma.

Daha fazla bilgi için [işaretlenmiş kullanıcılar güvenlik raporundan](concept-user-at-risk.md).

### <a name="azure-ad-subscription-for-office-365-customers"></a>Office 365 müşterileri için Azure AD aboneliği

Erişim için Office 365 kimlik bilgilerinizi de kullanabilirsiniz **Azure Yönetim Merkezi'ne**. Azure AD’ye erişiminizi etkinleştirdikten sonra, Azure AD portalına yeniden yönlendirilirsiniz. Temel abonelik düzeyinde, raporlarda sağlanan ayrıntı miktarı sınırlıdır. Ek veriler ve analizler, Azure Premium abonelerine sağlanır.

Erişim için **risk için işaretlenen kullanıcılar** Microsoft 365 Yönetim merkezinde raporları:

1.  Sol taraftaki gezinti menüsünden seçin **Yönetim Merkezleri**. 
2.  Seçin **Azure AD'ye**.
3.  **Azure Active Directory yönetim merkezi**'nde oturum açın.
4.  Bildiren sayfanın en üstünde bir başlık görüntüleniyorsa **yeni portala göz atın**, bağlantıyı seçin.
4.  Sol taraftaki gezinti menüsünde seçin **Azure Active Directory**. 
5.  Gezinti bölmesinde seçin **risk için işaretlenen kullanıcılar** gelen **güvenlik** bölümü.

## <a name="remediation-actions"></a>Düzeltme eylemleri

Etkilenen hesapları düzeltmeye ve ortamınızın güvenliğini sağlamaya yardımcı olmak için aşağıdaki eylemleri gerçekleştirin:

1.  [Doğru bilgileri doğrulamak](https://aka.ms/MFAValid) için multi-Factor authentication ve Self Servis parola sıfırlama. 
2.  [Çok faktörlü kimlik doğrulamasını etkinleştirme](https://aka.ms/MFAuth) tüm kullanıcılar için. 
3.  Bunu kullanın [düzeltme komut dosyası](https://aka.ms/remediate) otomatik olarak aşağıdaki adımları gerçekleştirmek için etkilenen her hesap için: 

    a. Hesabın güvenliğini sağlamak ve etkin oturumları sonlandırmak için parolayı sıfırlayın.

    b. Posta kutusu temsilcilerini kaldırın.

    c. Dış etki alanlarına posta iletme kurallarını devre dışı bırakın.

    d. Posta kutusunda genel posta iletme özelliğini kaldırın.

    e. Kullanıcının hesabında MFA'yı etkinleştirin.

    f. Hesapta parola karmaşıklığını yüksek düzeyde olacak şekilde ayarlayın.

    g. Posta kutusu denetimini etkinleştirin.

    h. Bir denetim günlüğünü gözden geçirmek yöneticiye üretir.

4. Office 365 kiracınızı ve diğer BT altyapısını inceleyin; bunun için tüm kiracı ayarlarını, kullanıcı hesaplarını ve olası değişiklikler için kullanıcı başına yapılandırma ayarlarını gözden geçirin. Kalıcılık yöntemi göstergelerini, ayrıca yetkisiz erişim sağlayan birinin VPN kimlik bilgilerini almak veya diğer kurumsal kaynaklara erişmek için ilk tutunma noktasına işaret edebilecek göstergeleri arayın. 

5.  Araştırmanızı bir parçası olarak, Emniyet teşkilatı dahil olmak üzere resmi yetkililere bildirmelisiniz olup olmadığını göz önünde bulundurun.

Ayrıca şunları yapmalısınız:

- Okuma ve bunu uygulamak [alışılmadık etkinlikleri belirlemek üzere Kılavuzu](https://aka.ms/fixaccount). 
- [Denetim işlem hattını etkinleştirin](https://aka.ms/improvesecurity) kiracınızdaki etkinliği analiz etmenize yardımcı olmak için. İşlem tamamlandıktan sonra Denetim deponuza etkinlik günlükleri ile doldurma başlatır. Bu noktada, ayrıca yararlanabileceğiniz [güvenlik ve uyumluluk Merkezi'nin arama ve araştırma kaynak](https://aka.ms/sccsearch). 
- Bunu kullanın [posta kutusu denetimini etkinleştirmek için betik](https://aka.ms/mailboxaudit1) tüm hesaplarınız için. 
- Tüm posta kutularınız için temsilci izinlerini ve posta iletme kurallarını gözden geçirin. Bu görevi gerçekleştirmek için bu [PowerShell betiğini](https://aka.ms/delegateforwardrules) kullanabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory kimlik koruması](../active-directory-identityprotection.md)
* [Riskli oldukları belirlenen kullanıcılar](concept-user-at-risk.md)
