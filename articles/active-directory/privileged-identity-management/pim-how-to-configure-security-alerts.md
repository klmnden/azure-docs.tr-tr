---
title: Güvenlik Uyarıları Azure AD Dizin rolleri için PIM içinde yapılandırma | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) güvenlik uyarıları Azure AD Dizin rolleri için yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 11/01/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: e7204c223681b9a33c439b0d9fc653167422384a
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51011706"
---
# <a name="configure-security-alerts-for-azure-ad-directory-roles-in-pim"></a>Azure AD Dizin rolleri için güvenlik uyarıları PIM'de yapılandırın

Azure AD Privileged Identity Management (PIM), ortamınızda şüpheli veya güvenli olmayan bir etkinlik olduğunda uyarılar oluşturur. Bir uyarı tetiklendiğinde, PIM panosunda görünür. Uyarıyı tetikleyen rollerin listeleyen bir rapor görmek için uyarıyı seçin.

![PIM güvenlik uyarıları - ekran görüntüsü](./media/pim-how-to-configure-security-alerts/pim-directory-alerts.png)

## <a name="security-alerts"></a>Güvenlik uyarıları

Bu bölüm, nasıl düzeltileceğini ve nasıl engelleyeceğinizi yanı sıra Dizin rolleri için tüm güvenlik uyarılarını listeler. Önem derecesi şu anlama sahiptir:

* **Yüksek**: ilke ihlali nedeniyle Acil eylem gerektirir.
* **Orta**: Acil eylem gerektirmeyen ancak olası bir ilke ihlali bildirir.
* **Düşük**: Acil eylem gerektirmeyen ancak tercih ilke değişikliğini önerir.

### <a name="roles-are-being-assigned-outside-of-pim"></a>PIM dışında rolleri atanmış

| | |
| --- | --- |
| **Önem derecesi** | Yüksek |
| **Bu uyarı neden alıyorum?** | Ayrıcalıklı rol atamalarını PIM dışında yapılan düzgün izlenmez ve etkin bir saldırı gösterebilir. |
| **Nasıl?** | Listedeki kullanıcıları gözden geçirin ve bunları PIM dışında atanmış ayrıcalıklı rolleri kaldırın. |
| **Önleme** | Kullanıcılar ayrıcalıklı rollerini PIM dışında burada atandığı araştırın ve buradan gelecekteki atamaları yasaklar. |
| **Portal risk azaltma eylemi** | Hesabı, ayrıcalıklı rolünden kaldırır. |

### <a name="potential-stale-accounts-in-a-privileged-role"></a>Ayrıcalıklı bir role olası eski hesapları

| | |
| --- | --- |
| **Önem derecesi** | Orta |
| **Bu uyarı neden alıyorum?** | Parolalarını değişmedi hesapları, en son hizmet olabilir veya işlenen olmayan hesapları paylaşılan. Ayrıcalıklı roller bu hesaplarda saldırganlara karşı savunmasızdır. |
| **Nasıl?** | Hesapları listesinde gözden geçirin. Bunlar artık erişime ihtiyacınız varsa bunları kendi ayrıcalıklı rollerini kaldırın. |
| **Önleme** | Parolayı biliyor kullanıcılar bir değişiklik olduğunda paylaşılan hesapları güçlü parolalar döndürme emin olun. </br>Düzenli olarak hesapları erişim gözden geçirmelerini kullanarak ayrıcalıklı rolleri ile gözden geçirin ve artık gerekmeyen rol atamalarını kaldırın. |
| **Portal risk azaltma eylemi** | Hesabı, ayrıcalıklı rolünden kaldırır. |

### <a name="users-arent-using-their-privileged-roles"></a>Kullanıcılar ayrıcalıklı rollerini kullanmadığınız

| | |
| --- | --- |
| **Önem derecesi** | Düşük |
| **Bu uyarı neden alıyorum?** | Bunlar gerekmez ayrıcalıklı rolleri atanmış kullanıcılar, bir saldırı olasılığını artırır. Ayrıca, etkin olarak kullanılmayan hesaplarında gözden kaçan kalmasına saldırganların daha kolay olur. |
| **Nasıl?** | Listedeki kullanıcıları gözden geçirin ve bunları yüklemeniz gerekmez ayrıcalıklı rollerini kaldırın. |
| **Önleme** | Ayrıcalıklı roller yalnızca bir iş gerekçesi sahip olan kullanıcılara atayın. </br>Kullanıcılar hala doğrulamak için zamanlamayı normal erişim gözden geçirmeleri, bunların erişim gerekir. |
| **Portal risk azaltma eylemi** | Hesabı, ayrıcalıklı rolünden kaldırır. |
| **Tetikleyici** | Bir kullanıcı bir rolünü etkinleştirmeden belirli bir süre miktarını aşması durumunda tetiklenir. |
| **Gün sayısı** | Bu ayar, bir kullanıcı bir rolünü etkinleştirmeden gidebilirsiniz 100, 0 gün sayısını belirtir.|

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

### <a name="roles-dont-require-mfa-for-activation"></a>Rol etkinleştirmesi için MFA gerektirmeyen

| | |
| --- | --- |
| **Önem derecesi** | Düşük |
| **Bu uyarı neden alıyorum?** | Mfa'yı tehlikeye giren kullanıcıların ayrıcalıklı rolleri etkinleştirebilirsiniz. |
| **Nasıl?** | Rollerin listesini gözden geçirin ve [MFA gerektirecek](pim-how-to-change-default-settings.md) her rol için. |
| **Önleme** | [MFA gerektirme](pim-how-to-change-default-settings.md) her rol için.  |
| **Portal risk azaltma eylemi** | MFA ayrıcalıklı rol etkinleştirmesi için gerekli kılar. |

## <a name="configure-security-alert-settings"></a>Güvenlik Uyarısı Ayarları yapılandırma

Güvenlik Uyarıları ortamınız ve Güvenlik amaçları ile çalışmak için PIM bazıları özelleştirebilirsiniz. Güvenlik Uyarısı Ayarları'nı açmak için şu adımları izleyin:

1. Açık **Azure AD Privileged Identity Management**.

1. Tıklayın **Azure AD rolleri**.

1. Tıklayın **ayarları** ardından **uyarılar**.

    ![Güvenlik Uyarıları ayarlarına gidin](./media/pim-how-to-configure-security-alerts/settings-alerts.png)

1. Bu uyarının ayarı yapılandırmak için bir uyarı adına tıklayın.

    ![Güvenlik uyarısı ayarları](./media/pim-how-to-configure-security-alerts/security-alert-settings.png)

## <a name="next-steps"></a>Sonraki adımlar

- [PIM'de Azure AD dizini rol ayarlarını yapılandırma](pim-how-to-change-default-settings.md)
