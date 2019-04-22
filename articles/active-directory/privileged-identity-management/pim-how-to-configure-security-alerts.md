---
title: Güvenlik Uyarıları Azure AD rolleri için PIM - Azure Active Directory yapılandırma | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) güvenlik uyarıları Azure AD rolleri için yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: ce0d99fb283be8cbeba6f8a7954ff49161a2d511
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59496718"
---
# <a name="configure-security-alerts-for-azure-ad-roles-in-pim"></a>PIM'de Azure AD rolleri güvenlik uyarılarını yapılandırma

Azure Active Directory (Azure AD) Privileged Identity Management (PIM), ortamınızda şüpheli veya güvenli olmayan bir etkinlik olduğunda uyarılar oluşturur. Bir uyarı tetiklendiğinde, PIM panosunda görünür. Uyarıyı tetikleyen rollerin listeleyen bir rapor görmek için uyarıyı seçin.

![PIM güvenlik uyarıları - ekran görüntüsü](./media/pim-how-to-configure-security-alerts/pim-directory-alerts.png)

## <a name="security-alerts"></a>Güvenlik uyarıları

Bu bölümde, nasıl düzeltileceğini ve engelleme ile birlikte Azure AD rolleri için tüm güvenlik uyarılarını listeler. Önem derecesi şu anlama sahiptir:

* **Yüksek**: Acil eylem ilke ihlali nedeniyle gerektirir.
* **Orta**: Acil eylem gerektirmeyen ancak olası bir ilke ihlali bildirir.
* **Düşük**: Acil eylem gerektirmeyen ancak tercih ilke değişikliğini önerir.

### <a name="administrators-arent-using-their-privileged-roles"></a>Yöneticiler ayrıcalıklı rollerini kullanmadığınız

| | |
| --- | --- |
| **Önem derecesi** | Düşük |
| **Bu uyarı neden alıyorum?** | Bunlar gerekmez ayrıcalıklı rolleri atanmış kullanıcılar, bir saldırı olasılığını artırır. Ayrıca, etkin olarak kullanılmayan hesaplarında gözden kaçan kalmasına saldırganların daha kolay olur. |
| **Nasıl?** | Listedeki kullanıcıları gözden geçirin ve bunları yüklemeniz gerekmez ayrıcalıklı rollerini kaldırın. |
| **Önleme** | Ayrıcalıklı roller yalnızca bir iş gerekçesi sahip olan kullanıcılara atayın. </br>Zamanlama normal [erişim gözden geçirmeleriyle](pim-how-to-start-security-review.md) kullanıcılar yine de erişimleri gerektiğini doğrulayın. |
| **Portal risk azaltma eylemi** | Hesabı, ayrıcalıklı rolünden kaldırır. |
| **Tetikleyici** | Bir kullanıcı bir rolünü etkinleştirmeden belirli bir süre miktarını aşması durumunda tetiklenir. |
| **Gün sayısı** | Bu ayar, bir kullanıcı bir rolünü etkinleştirmeden gidebilirsiniz 100, 0 gün sayısını belirtir.|

### <a name="roles-dont-require-multi-factor-authentication-for-activation"></a>Rol etkinleştirmesi için çok faktörlü kimlik doğrulaması gerektirmeyen

| | |
| --- | --- |
| **Önem derecesi** | Düşük |
| **Bu uyarı neden alıyorum?** | Mfa'yı tehlikeye giren kullanıcıların ayrıcalıklı rolleri etkinleştirebilirsiniz. |
| **Nasıl?** | Rollerin listesini gözden geçirin ve [MFA gerektirecek](pim-how-to-change-default-settings.md) her rol için. |
| **Önleme** | [MFA gerektirme](pim-how-to-change-default-settings.md) her rol için.  |
| **Portal risk azaltma eylemi** | MFA ayrıcalıklı rol etkinleştirmesi için gerekli kılar. |

### <a name="the-tenant-doesnt-have-azure-ad-premium-p2"></a>Kiracının Azure AD Premium P2 yok

| | |
| --- | --- |
| **Önem derecesi** | Düşük |
| **Bu uyarı neden alıyorum?** | Azure AD Premium P2 geçerli Kiracı yok. |
| **Nasıl?** | Hakkında bilgileri gözden [Azure AD sürümleri](../fundamentals/active-directory-whatis.md). Azure AD Premium P2'ye yükseltin. |

### <a name="potential-stale-accounts-in-a-privileged-role"></a>Ayrıcalıklı bir role olası eski hesapları

| | |
| --- | --- |
| **Önem derecesi** | Orta |
| **Bu uyarı neden alıyorum?** | Son 90 gün içinde parolalarını değişmedi hesapları bir ayrıcalıklı rol. Bu hesaplar service olabilir veya işlenen değildir ve saldırganlara karşı savunmasız olan hesapları paylaşılan. |
| **Nasıl?** | Hesapları listesinde gözden geçirin. Bunlar artık erişime ihtiyacınız varsa bunları kendi ayrıcalıklı rollerini kaldırın. |
| **Önleme** | Parolayı biliyor kullanıcılar bir değişiklik olduğunda paylaşılan hesapları güçlü parolalar döndürme emin olun. </br>Hesapları kullanarak ayrıcalıklı rolleri ile düzenli olarak gözden [erişim gözden geçirmeleriyle](pim-how-to-start-security-review.md) ve artık gerekmeyen rol atamalarını kaldırın. |
| **Portal risk azaltma eylemi** | Hesabı, ayrıcalıklı rolünden kaldırır. |
| **En iyi uygulamalar** | , Hizmet, paylaşılan ve bir parola kullanarak kimlik doğrulaması ve genel yönetici veya Güvenlik Yöneticisi gibi yüksek ayrıcalıklı yönetici rollerine atanan Acil Durum erişim hesapları için aşağıdaki durumlarda Döndürülmüş parolalarını sahip olmalıdır:<ul><li>Kötüye kullanım veya yönetimsel erişim hakları'nın güvenliğinin içeren bir güvenlik olayı sonra</li><li>Bunlar artık yönetici (bir BT yöneticisi bırakır oluştu veya kuruluştan ayrılması Örneğin, bir çalışanın sonra), böylece herhangi bir kullanıcının ayrıcalıkları değiştikten</li><li>Düzenli aralıklarla (örneğin, üç aylık veya yıllık), bilinen ihlalinden ya da değişiklik olduysa bile BT personel</li></ul>Birden çok kişi bu hesapların kimlik bilgilerine erişiminiz olduğundan, kendi rolleri bıraktıysanız kişiler hesapları artık erişebildiğinden emin olmak için kimlik bilgilerini döndürülmesi gereken. [Daha fazla bilgi](https://aka.ms/breakglass) |

### <a name="roles-are-being-assigned-outside-of-pim"></a>PIM dışında rolleri atanmış

| | |
| --- | --- |
| **Önem derecesi** | Yüksek |
| **Bu uyarı neden alıyorum?** | Ayrıcalıklı rol atamalarını PIM dışında yapılan düzgün izlenmez ve etkin bir saldırı gösterebilir. |
| **Nasıl?** | Listedeki kullanıcıları gözden geçirin ve bunları PIM dışında atanmış ayrıcalıklı rolleri kaldırın. |
| **Önleme** | Kullanıcılar ayrıcalıklı rollerini PIM dışında burada atandığı araştırın ve buradan gelecekteki atamaları yasaklar. |
| **Portal risk azaltma eylemi** | Hesabı, ayrıcalıklı rolünden kaldırır. |

### <a name="there-are-too-many-global-administrators"></a>Çok fazla genel Yöneticiler vardır.

| | |
| --- | --- |
| **Önem derecesi** | Düşük |
| **Bu uyarı neden alıyorum?** | En yüksek ayrıcalıklı rol genel yöneticidir. Genel yönetici ele geçirilirse saldırgan tüm sisteminizi riske koyar izinlerini tüm erişim kazanır. |
| **Nasıl?** | Listedeki kullanıcıları gözden geçirin ve genel Yönetici rolüne kesinlikle gerekmeyen herhangi kaldırın. </br>Bu kullanıcıların daha düşük ayrıcalıklı rolleri atayın. |
| **Önleme** | Kullanıcıların ihtiyaç duydukları az ayrıcalıklı rol atayın. |
| **Portal risk azaltma eylemi** | Hesabı, ayrıcalıklı rolünden kaldırır. |
| **Tetikleyici** | Tetiklenecek iki farklı ölçütleri karşılandığında ve bunların her ikisi de yapılandırabilirsiniz. İlk olarak, genel yöneticiler belirli bir eşiğe ulaşması gerekir. İkinci olarak, belirli bir yüzdesini toplam rol atamalarınızı genel yönetici olması gerekir. Yalnızca Bu ölçümler birini karşılıyorsanız, uyarı görüntülenmez. |
| **Minimum genel yönetici sayısı** | Bu ayar, 2 ila 100 güvenli olmayan bir miktar göz önünde bulundurun, genel Yöneticiler sayısını belirtir. |
| **Genel yönetici oranı** | Bu ayar, %0 ile % 100 ortamınızda güvenli değil %, genel yönetici olan yöneticilere minimum yüzdesini belirtir. |

### <a name="roles-are-being-activated-too-frequently"></a>Rolleri çok sık etkinleştiriliyor

| | |
| --- | --- |
| **Önem derecesi** | Düşük |
| **Bu uyarı neden alıyorum?** | Aynı kullanıcı tarafından aynı ayrıcalıklı rol için birden çok etkinleştirmeye bir saldırının işaretini olur. |
| **Nasıl?** | Listedeki kullanıcıları gözden geçirin ve emin [etkinleştirme süresi](pim-how-to-change-default-settings.md) ayrıcalıklı rolünü yeteri kadar bunlar için görevlerini gerçekleştirmek ayarlanır. |
| **Önleme** | Emin [etkinleştirme süresi](pim-how-to-change-default-settings.md) ayrıcalıklı roller için kullanıcıların görevlerini gerçekleştirmek için yeterince uzun ayarlanır.</br>[MFA gerektirme](pim-how-to-change-default-settings.md) birden çok yöneticileri tarafından paylaşılan hesaplarına sahip ayrıcalıklı roller için. |
| **Portal risk azaltma eylemi** | Yok |
| **Tetikleyici** | Bir kullanıcı birden çok kez belirli bir dönem içinde aynı ayrıcalıklı rol etkinleştirir tetiklenecek. Süre hem etkinleştirme sayısını yapılandırabilirsiniz. |
| **Etkinleştirme yenileme zaman çerçevesi** | Bu ayar, gün, saat, dakika belirtir ve ikinci şüpheli yenilemeler izlemek için kullanmak istediğiniz zaman aralığı. |
| **Etkinleştirme yenileme sayısı** | Bu ayar, 2, seçtiğiniz zaman çerçevesi içinde bir uyarının bu durum göz önünde bulundurmanız ve 100'den etkinleştirme sayısını belirtir. Bu, kaydırıcıyı hareket ya da metin kutusuna bir sayı yazmak ayarı değiştirebilirsiniz. |

## <a name="configure-security-alert-settings"></a>Güvenlik Uyarısı Ayarları yapılandırma

Güvenlik Uyarıları ortamınız ve Güvenlik amaçları ile çalışmak için PIM bazıları özelleştirebilirsiniz. Güvenlik Uyarısı Ayarları'nı açmak için şu adımları izleyin:

1. Açık **Azure AD Privileged Identity Management**.

1. Tıklayın **Azure AD rolleri**.

1. Tıklayın **ayarları** ardından **uyarılar**.

    ![Güvenlik Uyarıları ayarlarına gidin](./media/pim-how-to-configure-security-alerts/settings-alerts.png)

1. Bu uyarının ayarı yapılandırmak için bir uyarı adına tıklayın.

    ![Güvenlik uyarısı ayarları](./media/pim-how-to-configure-security-alerts/security-alert-settings.png)

## <a name="next-steps"></a>Sonraki adımlar

- [PIM'de Azure AD rol ayarlarını yapılandırma](pim-how-to-change-default-settings.md)
