---
title: Azure Active Directory portalında risk güvenliği raporu için işaretlenmiş kullanıcılar | Microsoft Docs
description: Azure Active Directory portalında risk güvenliği için işaretlenmiş kullanıcılar hakkında bilgi edinin
services: active-directory
author: priyamohanram
manager: mtillman
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 05/23/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 385106065322cae9b8e29f66bfd6920d4ebb52f8
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "42057600"
---
# <a name="remediate-users-flagged-for-risk-in-the-azure-active-directory-portal"></a>Azure Active Directory portalında riskli olarak işaretlenmiş kullanıcıları düzeltme

Azure Active Directory’de (Azure AD) güvenlik raporları ile ortamınızda güvenliği tehlikeye girmiş kullanıcı hesaplarının olasılığı hakkında bilgi sahibi olabilirsiniz. [Riskli olduğu için işaretlenmiş kullanıcılar](../identity-protection/overview.md#users-flagged-for-risk), güvenliği tehlikeye girmiş olabilecek kullanıcı hesaplarının göstergesidir.

Microsoft ortamınızın güvenliğini korumak için çalışmaktadır. Bu çalışma kapsamında, Microsoft alışılmadık veya bilinen saldırı düzenleriyle tutarlı etkinlikleri sürekli izler. 


Kullanıcılarınızdan bazılarının hesaplarına yetkisiz erişimi gösteriyor olabilecek alışılmadık etkinlikler algılanırsa, eyleme geçmenizi sağlayacak bildirimler alırsınız. Size bildirimler sağlanması Microsoft'un kendi sistemlerinde herhangi bir şekilde güvenliğin aşıldığı anlamına gelmez.
 

## <a name="access-the-users-flagged-for-risk-report"></a>Riskli olarak işaretlenen kullanıcılar raporuna erişme

Azure Active Directory’deki (AD) ilgili [rapor](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/UsersAtRisk) aracılığıyla riskli olduğu için işaretlenmiş kullanıcıları gözden geçirebilirsiniz. Azure AD’ye abone değilseniz, [https://aka.ms/AccessAAD](https://aka.ms/AccessAAD) adresinde hiç ücret ödemeden bir defalık abonelik işlemi yapabilirsiniz. Bu raporda aşağıda örnekleri verilen çeşitli eylemleri gerçekleştirebilirsiniz:

- Geçici parola oluşturma
- Kullanıcının bir sonraki defa oturum açtığında parolasını güvenli şekilde sıfırlamasını zorunlu tutma
- Herhangi bir düzeltme eylemi gerçekleştirmeden kullanıcı riskini kapatma.

Daha fazla bilgi için bkz. [Azure Active Directory portalında riskli olarak işaretlenmiş kullanıcılar güvenlik raporu](concept-user-at-risk.md).

### <a name="azure-ad-subscription-for-office-365-customers"></a>Office 365 müşterileri için Azure AD aboneliği

İşlem tamamlandığında, Office 365 kimlik bilgilerinizi kullanarak Azure Yönetim Merkezi'ne erişebilirsiniz. Azure AD’ye erişiminizi etkinleştirdikten sonra, Azure AD portalına yeniden yönlendirilirsiniz. Temel abonelik düzeyinde, raporlarda sağlanan ayrıntı miktarı sınırlıdır. Ek veriler ve analizler, Azure Premium abonelerine sağlanır.


**Office 365 yönetim merkezinde Riskli olarak işaretlenmiş kullanıcılar raporlarına erişmek için:**

1.  Sol taraftaki gezinti menüsünde **Yönetim merkezleri**'ne tıklayın. 
2.  **Azure AD**'ye tıklayın.
3.  **Azure Active Directory yönetim merkezi**'nde oturum açın.
4.  Sayfanın en üstünde **Yeni portalı gözden geçirin** ifadesini içeren bir başlık görüntüleniyorsa, bağlantıya tıklayın.
4.  Sol taraftaki gezinti menüsünde **Azure Active Directory**'ye tıklayın. 
5.  Gezinti bölmesinde, **Güvenlik**'in altında **Riskli oldukları belirlenen kullanıcılar**'a tıklayın.

Burada görüntülenen bilgileri gözden geçirin. Listelenen tüm hesapların parolasını sıfırlamalısınız. 

## <a name="remediation-actions"></a>Düzeltme eylemleri

Etkilenen hesapları düzeltmeye ve ortamınızın güvenliğini sağlamaya yardımcı olmak için aşağıdaki eylemleri gerçekleştirin:

1.  Çok faktörlü kimlik doğrulaması ve self servis parola sıfırlama için bilgilerin doğruluğunu [onaylayın](http://aka.ms/MFAValid). 
2.  Tüm kullanıcılarda çok faktörlü kimlik doğrulamasını [etkinleştirin](http://aka.ms/MFAuth). 
3.  Etkilenen her hesap için, bu [düzeltme betiğini](http://aka.ms/remediate) kullanıp aşağıdaki adımları otomatik olarak gerçekleştirebilirsiniz: 

    a. Hesabın güvenliğini sağlamak ve etkin oturumları sonlandırmak için parolayı sıfırlayın.

    b. Posta kutusu temsilcilerini kaldırın.

    c. Dış etki alanlarına posta iletme kurallarını devre dışı bırakın.

    d. Posta kutusunda genel posta iletme özelliğini kaldırın.

    e. Kullanıcının hesabında MFA'yı etkinleştirin.

    f. Hesapta parola karmaşıklığını yüksek düzeyde olacak şekilde ayarlayın.

    g. Posta kutusu denetimini etkinleştirin.

    h. Yöneticinin gözden geçirmesi için Denetim Günlüğü oluşturun.

4. Office 365 kiracınızı ve diğer BT altyapısını inceleyin; bunun için tüm kiracı ayarlarını, kullanıcı hesaplarını ve olası değişiklikler için kullanıcı başına yapılandırma ayarlarını gözden geçirin. Kalıcılık yöntemi göstergelerini, ayrıca yetkisiz erişim sağlayan birinin VPN kimlik bilgilerini almak veya diğer kurumsal kaynaklara erişmek için ilk tutunma noktasına işaret edebilecek göstergeleri arayın. 

5.  Araştırmanız kapsamında, polis de dahil olmak üzere resmi yetkililere durumu bildirmenizin gerekip gerekmediğini de düşünmelisiniz.

Ayrıca şunları yapmalısınız:

- Alışılmadık etkinlikleri belirlemek üzere bu [kılavuzu](http://aka.ms/fixaccount) okuyun ve yönergelerini uygulayın. 
- Kiracınızdaki etkinliği analiz etmenize yardımcı olması için [denetim işlem hattını etkinleştirin](http://aka.ms/improvesecurity). Bu tamamlandıktan sonra, tüm etkinlik günlükleri denetim deponuza doldurulmaya başlar. Bu noktada, [Güvenlik ve Uyumluluk Merkezi'nin Arama ve Araştırma](http://aka.ms/sccsearch) özelliğinden de yararlanabilirsiniz. 
- Tüm hesaplarınızda posta kutusu denetimini etkinleştirmek için bu [betiği](http://aka.ms/mailboxaudit1) kullanın. 
- Tüm posta kutularınız için temsilci izinlerini ve posta iletme kurallarını gözden geçirin. Bu görevi gerçekleştirmek için bu [PowerShell betiğini](http://aka.ms/delegateforwardrules) kullanabilirsiniz. 



## <a name="next-steps"></a>Sonraki adımlar

- Azure Active Directory Kimlik Koruması hakkında daha fazla bilgi için bkz. [Azure Active Directory Kimlik Koruması](../active-directory-identityprotection.md).

