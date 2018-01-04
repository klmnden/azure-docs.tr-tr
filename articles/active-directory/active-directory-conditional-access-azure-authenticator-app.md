---
title: "Android için Azure Authenticator | Microsoft Docs"
description: "Microsoft Azure Authenticator uygulamasını oturum iş kaynaklarına erişmek için açmak için kullanılabilir. Azure Authenticator uygulamasını bekleniyor iki Faktörlü doğrulama isteklerini size mobil cihazınıza bir uyarı görüntüleyerek bildirir."
services: active-directory
documentationcenter: 
author: femila
manager: swadhwa
editor: 
ms.assetid: b7ceca0d-5c9d-45c4-942c-b3a9b6bad36c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: femila
ROBOTS: NOINDEX, NOFOLLOW
ms.openlocfilehash: 349649e015aae7198d2c40efc3c1865cad087e8a
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="azure-authenticator-for-android"></a>Android için Azure Authenticator
BT yöneticiniz oturum iş kaynaklarınıza erişmek için açmak için Microsoft Azure Authenticator kullanmanızı tavsiye. Bu uygulama bu iki oturum açma seçenekleri sağlar:

* Çok faktörlü kimlik doğrulaması, iş veya Okul hesaplarınıza iki aşamalı doğrulama ile güvenli hale getirmek sağlar. (Örneğin, parolanızı) bilmeniz ve hesabı koruyan bir şey kullanarak oturum açın (Bu uygulama bir güvenlik anahtarı) ile bir şey olması daha. Azure Authenticator uygulamasını bekleniyor iki Faktörlü doğrulama isteklerini size mobil cihazınıza bir uyarı görüntüleyerek bildirir. Uygulama ve dokunun istekte doğrulamak veya iptal yalnızca görünümüne gerekir. Alternatif olarak, uygulamada görüntülenen bir parola girmeniz istenebilir.
* İş hesabı, Android telefonunuzu veya Tabletinizi güvenilir bir cihaz haline getirebilir ve şirket uygulamaları için çoklu oturum açma (SSO) sağlamanıza olanak verir. BT yöneticiniz, şirket kaynaklarına erişmek için bir iş hesabı eklemeye gerektirebilir. SSO kez oturum açarak şirketinizin sizin kullanımınıza sunduğu tüm uygulamalar arasında açarken otomatik olarak kullanılabilir kredi olanak sağlar.

## <a name="installing-the-azure-authenticator-app"></a>Azure Authenticator uygulamasını yükleme
Azure Authenticator uygulamasını Google Play Mağazası'ndan yükleyebilirsiniz.
İş hesabı Samsung Android cihaz vs Samsung olmayan Android aygıttan eklemek için yönergeleri biraz farklıdır. Her ikisi için yönergeler aşağıda listelenmiştir.

## <a name="adding-the-work-account-from-samsung-android-device"></a>İş hesabı Samsung Android aygıttan ekleme
### <a name="adding-the-work-account-through-the-app-home-screen"></a>Uygulama giriş ekranı aracılığıyla iş hesabı ekleme
Aşağıdaki yönergeleri, Samsung GS3 ve telefonları ya da not2 üstünde ve tabletler yukarıda geçerlidir.

1. Uygulamanın giriş ekranında, son kullanıcı lisans sözleşmesi (EULA) kabul edin.
2. Hesap etkinleştirme ekranında, bağlam menüsünü seçin ve sağ tıklatın **iş hesabı**.
3. Hesap Ekle ekranda, select ** iş hesabı **.
4. Etkinleştirme Aygıt Yöneticisi ekranında tıklatın **etkinleştirme**.
5. Gizlilik İlkesi ekranında, onay kutusunu işaretleyin ve **Onayla**.
6. Çalışma alanına katılma ekranda tıklatın ve kuruluş tarafından sağlanan kullanıcı kimliği girin **katılma**.
7. Azure Authenticator uygulamasını oturum açmak için Kurumsal hesap ve parola girin ve **oturum**.
8. Çok faktörlü kimlik doğrulaması (MFA) hakkında bilgi görüntüler sonraki ekranda için güvenlik eklenir ve isteğe bağlıdır. İşiniz veya okulunuz iş hesabı oluşturmak için ikinci faktör kimlik doğrulaması gerektiriyorsa, bu ekranı görürsünüz. Daha fazla hesabınızı doğrulamak için yönergeler sağlar.
9. İleti çalışma alanına katılma ekranını görüntüler "**, çalışma alanına katılma**". Azure authenticator uygulamasını Cihazınızı çalışma alanına katılmaya çalışıyor.
10. Sonraki ekranda çalışma alanına katılmış iletiyi görmeniz gerekir.

> [!NOTE]
> Cihazınızda bir tek iş hesabı izin verilir.
> 
> 

### <a name="adding-the-work-account-from-the-settings-menu"></a>İş hesabı ayarları menüsünden ekleme
Azure Authenticator uygulamasını yükledikten sonra bir iş hesabı Android Hesap Yöneticisi'nden de oluşturabilirsiniz.

1. Ayarlar menüsünden gidin **hesapları** tıklatıp **hesabı Ekle**.
2. Bir iş hesabı eklemek için uygulama giriş ekranı, iş hesabıyla ekleme yordamı, 3-10 adımları izleyin.

## <a name="adding-the-work-account-from-a-non-samsung-android-device"></a>Bir Samsung Android aygıttan iş hesabı ekleme
### <a name="adding-the-work-account-through-the-app-home-screen"></a>Uygulama giriş ekranı aracılığıyla iş hesabı ekleme
1. Uygulamanın giriş ekranında, son kullanıcı lisans sözleşmesi (EULA) kabul edin.
2. Hesap etkinleştirme ekranında, bağlam menüsünü seçin ve sağ tıklatın **iş hesabı**.
3. Hesapları ekranında tıklatın **hesabı Ekle**.
4. Hesapları ekran görürseniz tıklatın **hesabı eklemek**. Bir iş hesabı zaten daha önce oluşturulan iş hesabı var olan gösteren bir eşitleme ekran görürsünüz. İş hesabı giriş ekranına geri oku dokunarak tutabilirsiniz. Alternatif olarak, kaldırmak ve üzerinde çalışma alanına katılma yeni bir iş hesabı ekran yeniden, kuruluşunuz tarafından sağlanan kullanıcı kimliği girin ve katılma hesabı seçebilirsiniz.
5. Azure Authenticator uygulamasını oturum açmak için Kurumsal hesap ve parola girin ve **oturum**.
6. Çok faktörlü kimlik doğrulaması (MFA) hakkında bilgi görüntüler sonraki ekranda için güvenlik eklenir ve isteğe bağlıdır. İşiniz veya okulunuz iş hesabı oluşturmak için ikinci faktör kimlik doğrulaması gerektiriyorsa, bu ekranı görürsünüz. Daha fazla hesabınızı doğrulamak için yönergeler sağlar.
7. Tıklatın **Tamam** sonraki ekranda. Sertifika adı değiştirmeyin.
   ", çalışma alanına katılma" iletisi. Azure authenticator uygulamasını Cihazınızı çalışma alanına katılmaya çalışıyor.
   Sonraki ekranda çalışma alanına katılmış iletiyi görmeniz gerekir.

> [!NOTE]
> Cihazınızda bir tek iş hesabı izin verilir.
> 
> 

Azure Authenticator uygulamasını yükledikten sonra bir iş hesabı Android Hesap Yöneticisi'nden de oluşturabilirsiniz.

1. Gelen **ayarları** menüsünde hesaplarına gidin ve tıklayın **hesabı Ekle**.
2. Bir iş hesabı eklemek için iş hesabı üzerinden uygulama giriş ekranı **, ekleme yordamı, 2-7 adımları izleyin.

### <a name="how-to-find-out-which-version-is-installed"></a>Hangi sürümünün yüklü bulma
1. Azure Authenticator uygulamasını hangi sürümü bulabilirsiniz ve ilişkili hizmet sürümleri aygıtınızda yüklenir.
2. Menü açılan, tıklatın **hakkında**.
3. Hakkında ekranı aygıtınızda yüklü sürümleri ile uygulama ve hizmetleri görüntüler.

### <a name="sending-log-files-to-report-issues"></a>Sorunları bildirmek için gönderen günlük dosyaları
1. Microsoft Azure Authenticator uygulaması ile bir olay rapor, bir olay numarası edinmek ve atanan olay numarası karşı günlük dosyaları gibi göndermek için Support yönergeleri izleyin:
2. Menü açılan, tıklatın **günlüğü**.
3. Microsoft Support açık bir olay varsa, (Bu bir sonraki adım için gerekir) olay numarasını not edin. Zaten bir destek olayını oluşturmadıysanız ve Yardım ister misiniz yerindeki yönergeye izleyin [Microsoft Support](https://support.microsoft.com/en-us/contactus) yeni bir olayı açmak için.
4. Günlük kaydı ekranında tıklatın **Şimdi Gönder**.
5. Kullanmak istediğiniz e-posta sağlayıcısı seçin.
6. Açık bir olay Microsoft Support zaten varsa, sorununuzu için günlük verileri göndermek nasıl öğrenmek için atanan destek mühendisine başvurun ve olayınız ile ilişkili sahip. Destek mühendisine e-posta adresi ve konu satırı bilgileri sağlayacaktır. Lütfen bir destek olayını zaten yoksa, yeni bir olayı açmak için Microsoft Support adresindeki yönergeleri izleyin.
7. Düzen **için** satır ve **konu** Microsoft Support almış bilgilerle satır.
8. Azure Authenticator uygulamasını gönderdiğiniz e-posta için günlük dosyasına ekler. Yaşadığınız sorunu açıklayan, (isteğe bağlı) alıcı listesini güncelleştirin ve e-posta gönderin.

### <a name="deleting-the-work-account-and-leaving-your-workplace"></a>İş hesabı siler ve çalışma
Herhangi bir zamanda gibi oluşturulan iş hesabı kaldırabilirsiniz:

**İş hesabı ayarları menüsünden silmek için**

1. Hesapları Yöneticisi'nden seçin **iş hesabı**.
2. İş hesabı ekranında içinde **genel ayarları**seçin **hesap ayarlarını – bırakın iş yeri ağınıza**.
3. Seçin **bırakın** üzerinde **çalışma alanına katılma** ekran.
4. Tıklatın **Tamam** zaman "istediğinizden emin misiniz çalışma alanından ayrılmak olan" iletisi görüntülenir.
5. Bu çalışma alanından silinmiş iş hesabınızı sağlar.

> [!NOTE]
> Bu seçenek bazı eski sürümleri Android çalışmayabilir bir iş hesabı silmek için hesap kaldırma seçeneği kullanmamanız önerilir.
> 
> 

## <a name="uninstalling-the-app"></a>Uygulamayı kaldırma
Bir Samsung Android cihazda aygıt yönetici ayrıcalıkları gibi kaldırmadan önce kaldırılmalıdır 

1. Gelen **ayarları**altında **sistem**seçin **güvenlik**.
2. D içinde**evice Yönetim**, tıklatın **cihaz yöneticileri**. Onay kutusunun yanına emin **Azure Authenticator** temizlenir.

## <a name="troubleshooting"></a>Sorun giderme
Görürseniz **Keystore hata**, kilit yok olmasından kaynaklanabilir ekran kümesi oluşturan bir PIN ile. Bu sorunu çözmek için Azure Authenticator uygulamasını kaldırın, kilit ekranınızda PIN yapılandırmak ve uygulamayı yeniden yükleyin.

