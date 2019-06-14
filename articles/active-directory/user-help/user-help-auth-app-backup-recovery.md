---
title: Microsoft Authenticator uygulamasını - Azure Active Directory ile yedekleyip | Microsoft Docs
description: Microsoft Authenticator uygulamasını kullanarak hesap kimlik bilgilerinizi, yedekleyip öğrenin.
services: active-directory
author: eross-msft
manager: daveba
ms.subservice: user-help
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 01/24/2019
ms.author: lizross
ms.reviewer: olhaun
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9634e2578ea256d1dec71389f676ee53627e6272
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60474238"
---
# <a name="backup-and-recover-account-credentials-with-the-microsoft-authenticator-app"></a>Yedekleme ve kurtarma hesabı kimlik bilgileriyle Microsoft Authenticator uygulaması

**Şunlara uygulanır:**

- iOS cihazları

Microsoft Authenticator uygulamasını hesaplarınızı sırası gibi ilgili uygulama ayarları ve hesap kimlik bilgileri buluta yedekler. Yedeklemeden sonra uygulama bilgilerinizi yeni bir cihazda olabilecek erişimin kaybedilmesini önleme kurtarmak için de kullanabilirsiniz out veya hesaplarını yeniden oluşturmak zorunda.

> [!IMPORTANT]
> Her yedekleme depolama konumu için bir kişisel Microsoft hesabı ve bir iCloud hesabıyla gerekir. Ancak, bu depolama konumu içinde birkaç hesapları yedekleyebilirsiniz. Örneğin, bir kişisel hesap, okul hesabı ve Facebook, Google gibi bir üçüncü taraf hesabı ve benzeri.
> 
> Kullanıcı adınızı ve kimliğinizi ispatlamak için gerekli olan hesap doğrulama kodu içeren yalnızca kişisel ve 3. taraf hesap bilgilerinizi depolanır. Size e-postaları veya dosyaları dahil olmak üzere hesaplarınızla ilişkili herhangi bir bilgi depolamayın. Biz de olmayan ilişkilendirmek veya hesaplarınızı herhangi bir şekilde veya herhangi bir ürün veya hizmeti ile paylaşın. Ve son olarak, BT yöneticinizin bu hesapların ilgili herhangi bir bilgi elde etmezsiniz.

## <a name="back-up-your-account-credentials"></a>Hesap kimlik bilgilerini yedekle
Kimlik bilgilerinizi yedekleyebilmeniz için önce hem de sahip olmanız gerekir:

- Kişisel [Microsoft hesabı](https://account.microsoft.com/account) kurtarma hesabınız olarak görev yapacak.

- Bir [iCloud hesabıyla](https://www.icloud.com/) gerçek depolama konumuna için. 

Her iki hesap birlikte oturum açmanızı gerektiren yedekleme bilgileriniz için daha güçlü güvenlik sağlar.

**Bulutta yedekleme etkinleştirmek için**
-   İOS Cihazınızda seçin **ayarları**seçin **yedekleme**ve ardından açın **iCloud yedeklemesini**.

    Hesap kimlik bilgileriniz iCloud hesabınızda yedeklenir.

    ![Yedekleme ayarlarını iCloud konumunu gösteren iOS ayarları ekranı](./media/user-help-auth-app-backup-recovery/backup-and-recovery-turn-on.png)

## <a name="recover-your-account-credentials-on-your-new-device"></a>Hesap kimlik bilgilerinizi yeni Cihazınızda Kurtar
İCloud hesabınızda, hesabı kimlik bilgilerinizi bilgilerinizi yedeklendiğinde ayarladığınız Microsoft Kurtarma hesabı kullanarak kurtarabilirsiniz.

### <a name="to-recover-your-information"></a>Bilgilerinizi kurtarmak için
1.  İOS Cihazınızda Microsoft Authenticator uygulamasını açın ve seçin **başlamak kurtarma** ekranın alt.

    ![Başlangıç kurtarma yeri gösteren, Microsoft Authenticator uygulaması](./media/user-help-auth-app-backup-recovery/backup-and-recovery-begin-recovery.png)

2.  Yedekleme işlemi sırasında kullanılan aynı kişisel Microsoft hesabı kullanarak kurtarma hesabınızda oturum açın.

    Yeni cihaz için hesap kimlik bilgilerinizi kurtarılabilir.

Kurtarma işlemini tamamladıktan sonra kişisel Microsoft hesabı doğrulama kodları Microsoft Authenticator uygulamasını, eski ve yeni telefonlar arasında farklı olduğunu fark edebilirsiniz. Her bir cihaz kendi benzersiz bir kimlik bilgisi yok, ancak hem geçerli hem de iş ilişkili telefon kullanarak imzalanırken kodları farklıdır.

## <a name="recover-additional-accounts-requiring-more-verification"></a>Daha fazla doğrulama gerektiren ek hesap kurtarma
Kişisel ile anında iletme bildirimleri, iş veya Okul hesapları kullanıyorsanız, bir ekran uyarı, elde edecekleriniz bilgilerinizi kurtarmadan önce ek doğrulama sağlamalısınız söyler. Anında iletme bildirimleri belirli cihazınıza bağlı olan ve hiçbir zaman ağ üzerinden gönderilen bir kimlik bilgisi kullanarak gerektirdiğinden, kimlik bilgisi Cihazınızda oluşturulmadan önce kimliğinizi kanıtlamanız gerekir.

Kişisel Microsoft hesapları için alternatif bir e-posta veya telefon numarası ile birlikte parola girerek kimliğinizi kanıtlayabilirsiniz. İş veya Okul hesapları için size hesap sağlayıcınız tarafından verilen bir QR kodunu tarayın gerekir.

### <a name="to-provide-additional-verification-for-personal-accounts"></a>Kişisel hesapları için ek doğrulama sağlamak için
1.  İçinde **hesapları** yanındaki kurtarmak istediğiniz hesabı seçin açılır oka Microsoft Authenticator uygulamasının ekran.

    ![Microsoft Authenticator uygulaması, kullanılabilir hesaplar, ilişkili açılan oklarla gösteriliyor](./media/user-help-auth-app-backup-recovery/backup-and-recovery-arrow.png)

2.  Seçin **kurtarmak için oturum açın**parolanızı yazın ve ardından ek doğrulama olarak, e-posta adresi veya telefon numaranızı doğrulayın.

    ![Microsoft Authenticator uygulaması, oturum açma bilgilerinizi girmeniz için izin verme](./media/user-help-auth-app-backup-recovery/backup-and-recovery-sign-in.png)

### <a name="to-provide-additional-verification-for-work-or-school-accounts"></a>İş veya Okul hesapları için ek doğrulama sağlamak için
1.  İçinde **hesapları** yanındaki kurtarmak istediğiniz hesabı seçin açılır oka Microsoft Authenticator uygulamasının ekran.

    ![Microsoft Authenticator uygulaması, kullanılabilir hesaplar, ilişkili açılan oklarla gösteriliyor](./media/user-help-auth-app-backup-recovery/backup-and-recovery-additional-accts.png)

2.  Seçin **tarama QR kodunu, kurtarılır**ve ardından QR kodunu tarayın.

    ![QR kodunuz tarama olanak tanıyan, Microsoft Authenticator uygulaması](./media/user-help-auth-app-backup-recovery/backup-and-recovery-scan-qr-code.png)

    >[!NOTE]
    >QR kodu alma hakkında daha fazla bilgi için bkz. [Microsoft Authenticator uygulaması ile çalışmaya başlama](https://docs.microsoft.com/azure/active-directory/user-help/user-help-auth-app-download-install) veya [authenticator uygulamasını kullanmak için güvenlik bilgileri ' ayarlayın](https://docs.microsoft.com/azure/active-directory/user-help/security-info-setup-auth-app)bağlı olarak, yöneticiniz güvenlik bilgilerini açık olup olmadığı.

## <a name="troubleshooting-backup-and-recovery-problems"></a>Yedekleme ve kurtarma sorunlarını giderme
Yedekleme kullanılabilir olmamasının birkaç nedeni vardır:

-   **İşletim sistemlerini değiştirme.** Yedekleme, yedekleme, Android ve iOS arasında geçiş kullanılamaz olduğu anlamına gelir, telefonunuzun işletim sistemi tarafından sağlanan bulut depolama seçeneği olarak depolanır. Bu durumda, hesabınızı uygulamanın içinden el ile yeniden oluşturmanız gerekir.

-   **Ağ veya parola sorunları.** Bir ağa bağlı ve son İphone'unuzda kullanılan aynı Appleıd kullanarak iCloud hesabınızda oturum açmış emin olun.

-   **Yanlışlıkla silme.** Önceki cihazınızdan veya Bulut depolama hesabınızı yönetme sırasında yedekleme hesabınız silindi mümkündür. Bu durumda, hesabınızı uygulamanın içinden el ile yeniden oluşturmanız gerekir.

-   **Var olan Microsoft Authenticator hesaplar.** Microsoft Authenticator uygulamasını hesapları zaten ayarını etkinleştirdiyseniz, uygulama yedeklenen hesaplarınızı kurtarmanız mümkün olmayacaktır. Engelleme kurtarma, hesabınızın ayrıntıları güncel bilgileri üzerine değil olun yardımcı olur. Bu durumda, yedekleme kurtarmadan önce Authenticator uygulamanızda ayarlama var olan hesaplarından herhangi bir mevcut hesap bilgileri kaldırmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar
Yedeklenebilir ve hesap kimlik bilgilerinizi yeni Cihazınızı kurtarılan göre kimliğinizi doğrulamak için Microsoft Authenticator uygulamasını kullanmaya devam edebilirsiniz. Daha fazla bilgi için [hesaplarınız için Microsoft Authenticator uygulamasını kullanarak oturum açın](user-help-sign-in.md).

## <a name="related-topics"></a>İlgili konular

- [Microsoft Authenticator uygulaması nedir?](user-help-auth-app-overview.md)

- [Microsoft Authenticator uygulaması hakkında SSS](user-help-auth-app-faq.md)

- [Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/)
