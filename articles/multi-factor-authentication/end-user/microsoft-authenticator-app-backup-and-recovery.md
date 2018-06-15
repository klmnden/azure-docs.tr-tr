---
title: Yedekleme ve Kurtarma Microsoft Authenticator - Azure bilgiyle | Microsoft Docs
description: Yedekleme ve Microsoft Authenticator uygulaması kullanılarak hesap kimlik bilgilerinizi kurtarmak öğrenin.
services: multi-factor-authentication
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2018
ms.author: lizross
ms.reviewer: olhaun
ms.custom: end-user
ms.openlocfilehash: e25ccdad5285bfaa96f538aca415746942523d85
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
ms.locfileid: "33896274"
---
# <a name="backup-and-recover-account-credentials-with-the-microsoft-authenticator-app"></a>Yedekleme ve kurtarma hesabı kimlik bilgileriyle Microsoft Authenticator uygulaması
**Uygulandığı öğe:**

- iOS cihazları

Microsoft Authenticator uygulaması, hesapların sırasını gibi ilgili uygulama ayarları ve hesap kimlik bilgileri buluta yedekler. Yedeklemeden sonra uygulama kilitli olasılığı önleme yeni bir cihaz bilgilerinizi kurtarmak için de kullanabilirsiniz ya da hesapları yeniden oluşturmak zorunda.

>[!IMPORTANT]
> Her yedekleme depolama konumu için bir kişisel Microsoft hesabı ve bir iCloud hesabıyla gerekir. Ancak bu depolama konumu içinde birkaç hesapları yedekleyebilirsiniz. Örneğin, bir kişisel hesap, okul hesabı ve Facebook, Google, gibi bir üçüncü taraf hesabı varsa ve benzeri.<br><br>Kullanıcı adınızı ve, kimliğini kanıtlamak için gerekli olan hesabı doğrulama kodu içeren yalnızca kişisel ve 3. taraf hesap bilgileriniz depolanır. E-postalar veya dosyaları dahil olmak üzere, hesaplarıyla ilişkili herhangi bir bilgi depolarız yok. Biz de yok ilişkilendirmek veya hesaplarınızı herhangi bir şekilde veya herhangi bir ürün veya hizmet ile paylaşın. Ve son olarak, BT yöneticiniz bu hesaplardan birini hakkında hiçbir bilgi alamazsınız.

## <a name="back-up-your-account-credentials"></a>Hesap kimlik bilgilerini yedekle
Kimlik bilgilerinizi yedeklenmeden önce sahip olmalıdır:

- Kişisel [Microsoft hesabı](https://account.microsoft.com/account) kurtarma hesabı olarak davranacak.

- Bir [iCloud hesabıyla](https://www.icloud.com/) için gerçek depolama konumuna. 

Her iki hesap birlikte oturum açmanızı gerektiren yedekleme bilgilerinizi için daha güçlü güvenlik sağlar.

**Bulut üzerinde yedekleme etkinleştirmek için**
-   İOS Cihazınızda seçin **ayarları**seçin **yedekleme**ve ardından Aç **otomatik yedekleme**.

    Hesap kimlik bilgilerinizi iCloud hesabınıza yedeklenir.

    ![Yedekleme ayarlarını otomatik konumunu gösteren iOS ayarları ekranı](./media/authenticator-app-backup-and-recovery/backup-and-recovery-turn-on.png)

## <a name="recover-your-account-credentials-on-your-new-device"></a>Yeni aygıtınızda hesabı kimlik bilgilerinizi Kurtar
İCloud hesabınızdan hesabı kimlik bilgilerinizi bilgilerinizi yedeklenen sırada ayarladığınız aynı Microsoft Kurtarma hesabı kullanarak kurtarabilirsiniz.

**Bilgilerinizi kurtarmak için**
1.  İOS Cihazınızda Microsoft Authenticator uygulamasını açıp seçin **başlamak kurtarma** ekranın en altındaki.

    ![Begin kurtarma yeri gösteren Microsoft Authenticator uygulaması](./media/authenticator-app-backup-and-recovery/backup-and-recovery-begin-recovery.png)

2.  Yedekleme işlemi sırasında kullanılan aynı kişisel Microsoft hesabı kullanarak kurtarma hesabınızda oturum açın.

    Hesap kimlik bilgilerinizi yeni cihaz kurtarılır.

Kurtarma işlemini tamamladıktan sonra kişisel Microsoft hesabı doğrulama kodları için Microsoft Authenticator uygulama eski ve yeni telefonları arasında farklı olduğunu fark edebilirsiniz. Kodları, çünkü her aygıt, kendi benzersiz bir kimlik bilgisi yok, ancak her ikisi de geçerli ve iş ilişkili telefon kullanarak oturum açma sırasında farklıdır.

## <a name="recover-additional-accounts-requiring-more-verification"></a>Daha fazla doğrulama gerektiren ek hesapları Kurtar
Kişisel ile anında iletme bildirimleri, iş veya Okul hesapları kullanıyorsanız, bir ekran uyarı, elde edersiniz bilgilerinizi kurtarabilmek ek doğrulama sağlamalıdır söyler. Anında iletme bildirimleri, belirli cihaza bağlanır ve hiçbir zaman ağ üzerinden gönderilen kimlik bilgilerini kullanarak gerektirdiğinden, kimlik bilgisi aygıtınızda oluşturulmadan önce kimliğinizi kanıtlamanız gerekir.

Kişisel Microsoft hesapları için alternatif bir e-posta veya telefon numarası ile birlikte parolanızı girerek kimliğinizi kanıtlayabilirsiniz. İş veya Okul hesapları için hesap sağlayıcınız tarafından verilen bir QR kodu tarama gerekir.

**Kişisel hesapları için ek doğrulama sağlamak için**
1.  İçinde **hesapları** Microsoft Authenticator uygulamasının kurtarmak istediğiniz hesap yanındaki Seç açılan oka ekranı.

    ![Bunların ilişkili açılan oklarla kullanılabilir hesaplar gösteren Microsoft Authenticator uygulaması](./media/authenticator-app-backup-and-recovery/backup-and-recovery-arrow.png)

2.  Seçin **kurtarmak oturum**parolanızı yazın ve ardından e-posta adresi veya telefon numaranızı ek doğrulama olarak onaylayın.

    ![Microsoft Authenticator uygulaması, oturum açma bilgilerinizi girmeniz için izin verme](./media/authenticator-app-backup-and-recovery/backup-and-recovery-sign-in.png)

**İş veya Okul hesapları için ek doğrulama sağlamak için**
1.  İçinde **hesapları** Microsoft Authenticator uygulamasının kurtarmak istediğiniz hesap yanındaki Seç açılan oka ekranı.

    ![Bunların ilişkili açılan oklarla kullanılabilir hesaplar gösteren Microsoft Authenticator uygulaması](./media/authenticator-app-backup-and-recovery/backup-and-recovery-additonal-accts.png)

2.  Seçin **kurtarmak için tarama QR kodu**, yöneticiniz tarafından sağlanan QR kodunu tarayın

    ![QR kodu taramak sağlayarak Microsoft Authenticator uygulaması](./media/authenticator-app-backup-and-recovery/backup-and-recovery-scan-qr-code.png)

## <a name="troubleshooting-backup-and-recovery-problems"></a>Yedekleme ve kurtarma sorunlarını giderme
Neden yedekleme kullanılamayabilir birkaç nedeni vardır:

-   **İşletim sistemlerini değiştirme.** Yedekleme, yedekleme Android ve iOS arasında geçiş yaparsanız kullanılamayacağı anlamına gelir, telefonunuzun işletim sistemi tarafından sağlanan bulut depolama seçeneği olarak depolanır. Bu durumda, uygulama içinde hesabınızı el ile yeniden oluşturmanız gerekir.

-   **Ağ veya parola sorunları.** Bir ağa bağlı ve son iPhone kullanılan aynı AppleID kullanarak iCloud hesabınızda oturum emin olun.

-   **Yanlışlıkla silme.** Önceki cihazınızdan veya Bulut depolama hesabınızı yönetme sırasında yedekleme hesabınızı silinmiş olabilir. Bu durumda, uygulama içinde hesabınızı el ile yeniden oluşturmanız gerekir.

-   **Var olan Microsoft Authenticator hesapları.** Microsoft Authenticator uygulama hesaplarını ayarlamış olduğunuz, uygulama yedeklenen hesaplarınızı kurtarmanız mümkün olmayacaktır. Engelleme kurtarma hesap bilgilerinizi güncel bilgilerle üzerine değildir sağlamaya yardımcı olur. Bu durumda, tüm mevcut hesap bilgileri yedekleme kurtarabilmek Doğrulayıcı uygulamanız ayarlamak varolan hesaplarından kaldırmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar
Yedeklenebilir ve yeni Cihazınızı hesabı kimlik bilgilerinizi kurtarılan göre kimliğinizi doğrulamak için Microsoft Authenticator uygulamasını kullanmaya devam edebilirsiniz.

## <a name="related-topics"></a>İlgili konular
- [Microsoft Authenticator uygulaması ile çalışmaya başlama](microsoft-authenticator-app-how-to.md)  

- [Telefonunuzla oturum açma](microsoft-authenticator-app-phone-signin-faq.md)

- [Microsoft Authenticator uygulaması hakkında SSS](microsoft-authenticator-app-faq.md)

- [Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/)
