---
title: Güvenlik uyarılarının nasıl yapılandırılacağı | Microsoft Docs
description: Güvenlik Uyarıları Azure Privileged Identity Management uzantısı için yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 06/06/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 8df9bc7c332a83e9761ea71dddfbfbfaa3ae5154
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622162"
---
# <a name="how-to-configure-security-alerts-in-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management güvenlik uyarılarını yapılandırma
## <a name="security-alerts"></a>Güvenlik uyarıları
Azure Privileged Identity Management (PIM), ortamınızda şüpheli veya güvenli olmayan bir etkinlik olduğunda uyarılar oluşturur. Bir uyarı tetiklendiğinde, PIM panosunda görünür. Uyarıyı tetikleyen rollerin listeleyen bir rapor görmek için uyarıyı seçin.

![PIM Pano güvenlik uyarıları - ekran görüntüsü](./media/pim-how-to-configure-security-alerts/PIM_security_dash.png)

| Uyarı | Severity | Tetikleyici | Öneri |
| --- | --- | --- | --- |
| **PIM dışında rolleri atanmış** |Yüksek |Bir kullanıcının PIM arabirimi dışında bir ayrıcalıklı rol kalıcı olarak atandı. |Kullanıcılar listesi ve bunları PIM dışında atanmış olan rolleri ayrıcalıklı powerbı.com'u gözden geçirin. |
| **Rolleri çok sık etkinleştiriliyor** |Orta |Ayarlar izin verilen süre içinde çok fazla yeniden etkinleştirmeleri aynı rol vardı. |Bunlar çok fazla kez rolü neden etkinleştirmiş görmek için kullanıcıyla iletişime geçin. Belki de için çok fazla görevlerini tamamlamak için bunları veya belki de komut dosyaları otomatik olarak bir rolü etkinleştirmek için kullanmakta olduğunuz zaman sınırı yoktur. Kendi rol etkinleştirme süresi görevlerini gerçekleştirmek için yeterince uzun ayarlanmış olduğundan emin olun. |
| **Rol etkinleştirmesi için çok faktörlü kimlik doğrulaması gerektirmeyen** |Orta |MFA ayarları içinde etkin olmayan roller bulunur. |Biz en üst düzeyde ayrıcalıklı roller için mfa'yı gerekli, ancak önemle tüm rollerin etkinleştirme için mfa'yı etkinleştirin. |
| **Kullanıcılar ayrıcalıklı rollerini kullanmadığınız** |Düşük |Kendi rolleri son etkinleştirmediyseniz uygun Yöneticiler vardır. |Erişim artık ihtiyacınız yoksa kullanıcıları belirlemek için bir erişim gözden geçirmesini başlatın. |
| **Çok fazla genel Yöneticiler vardır.** |Düşük |Önerilen daha fazla küresel Yöneticiler vardır. |Genel Yöneticiler yüksek sayıda varsa, kullanıcıların ihtiyaç duydukları daha fazla izin alıyorsanız olasıdır. Kullanıcıları için daha az ayrıcalıklı rolleri taşımak veya bunlardan bazıları kalıcı olarak atanan yerine rol için uygun yapın. |

### <a name="severity"></a>Severity
* **Yüksek**: ilke ihlali nedeniyle Acil eylem gerektirir. 
* **Orta**: Acil eylem gerektirmeyen ancak olası bir ilke ihlali bildirir.
* **Düşük**: Acil eylem gerektirmeyen ancak preferrable ilke değişikliğini önerir.

## <a name="configure-security-alert-settings"></a>Güvenlik Uyarısı Ayarları yapılandırma
Güvenlik Uyarıları ortamınız ve Güvenlik amaçları ile çalışmak için PIM bazıları özelleştirebilirsiniz. Ayarlar dikey penceresine erişmek için şu adımları izleyin:

1. Oturum [Azure portalında](https://portal.azure.com/) seçip **Azure AD Privileged Identity Management** kutucuğu panodan.
2. Seçin **yönetilen ayrıcalıklı rolleri** > **ayarları** > **uyarıları ayarları**.
   
    ![Güvenlik Uyarıları ayarlarına gidin](./media/pim-how-to-configure-security-alerts/PIM_security_settings.png)

### <a name="roles-are-being-activated-too-frequently-alert"></a>"Rolleri çok sık etkinleştirilmekte" Uyarısı
Bir kullanıcı birden çok kez belirli bir dönem içinde aynı ayrıcalıklı rol etkinleştirir, bu bir uyarı tetikler. Süre hem etkinleştirme sayısını yapılandırabilirsiniz.

* **Etkinleştirme yenileme zaman çerçevesi**: belirtin gün, saat, dakika ve saniye şüpheli yenilemeler izlemek için kullanmak istediğiniz zaman aralığını.
* **Etkinleştirme yenileme sayısı**: 2 seçtiğiniz zaman çerçevesi içinde bir uyarının bu durum göz önünde bulundurmanız ve 100'den bir etkinleştirme sayısını belirtin. Bu, kaydırıcıyı hareket ya da metin kutusuna bir sayı yazmak ayarı değiştirebilirsiniz.

### <a name="there-are-too-many-global-administrators-alert"></a>"Çok fazla genel Yöneticiler bulunur" Uyarısı
PIM iki farklı ölçütleri karşılandığında ve bunların her ikisi de yapılandırabilirsiniz, bu bir uyarı tetikler. İlk olarak, genel yöneticiler belirli bir eşiğe ulaşması gerekir. İkinci olarak, belirli bir yüzdesini toplam rol atamalarınızı genel yönetici olması gerekir. Yalnızca Bu ölçümler birini karşılıyorsanız, uyarı görüntülenmez.  

* **Minimum genel yönetici sayısı**: 2 ila 100 güvenli olmayan bir miktar göz önünde bulundurun, genel Yöneticiler sayısını belirtin.
* **Genel yönetici oranı**: % %100 0, yani ortamınızda güvenli olmayan, genel yönetici olan yöneticilere yüzdesini belirtin.

### <a name="administrators-arent-using-their-privileged-roles-alert"></a>"Yöneticiler ayrıcalıklı rollerini kullanarak değil" Uyarısı
Bir kullanıcı bir rolünü etkinleştirmeden belirli bir miktar olursa bu uyarıyı tetikler.

* **Gün sayısı**: bir kullanıcı bir rolünü etkinleştirmeden gidebilirsiniz 100, 0 gün sayısını belirtin.

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [active-directory-privileged-identity-management-toc](../../../includes/active-directory-privileged-identity-management-toc.md)]
