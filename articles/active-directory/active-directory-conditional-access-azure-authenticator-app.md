---
title: Android için Azure Authenticator | Microsoft Docs
description: Microsoft Azure Authenticator uygulaması, iş kaynaklarına erişmek için oturum için kullanılabilir. Azure Authenticator uygulaması mobil cihazınıza bir uyarı görüntüleyerek bekleyen iki aşamalı doğrulama isteklerini size bildirir.
services: active-directory
documentationcenter: ''
author: femila
manager: swadhwa
editor: ''
ms.assetid: b7ceca0d-5c9d-45c4-942c-b3a9b6bad36c
ms.service: active-directory
ms.component: protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: femila
ROBOTS: NOINDEX, NOFOLLOW
ms.openlocfilehash: fbd728ec8cd2c8c4cd7ca74ecd84fd4d0d41cbd0
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42056360"
---
# <a name="azure-authenticator-for-android"></a>Android için Azure Authenticator
BT yöneticiniz, iş kaynaklarına erişmek için oturum için Microsoft Azure Authenticator'ı kullanmanızı önermiş olabilir. Bu uygulama, bu iki oturum açma seçenekleri sağlar:

* Çok faktörlü kimlik doğrulaması, iş veya Okul hesaplarınıza iki aşamalı doğrulama ile güvenli hale getirmenize olanak sağlar. Bir şey (örneğin, parolanızı) bildiğiniz ve hesabı koruyan kullanarak oturum açın (Bu uygulamadan bir güvenlik anahtarı) ile bir sorun olması daha. Azure Authenticator uygulaması mobil cihazınıza bir uyarı görüntüleyerek bekleyen iki aşamalı doğrulama isteklerini size bildirir. Dokunun ve uygulamaya istekte doğrulayın veya iptal yalnızca görünümüne ihtiyacınız vardır. Alternatif olarak, uygulamada görüntülenen bir parola girmeniz istenebilir.
* İş hesabı Android telefonunuzu veya Tabletinizi güvenilir bir cihazda oturum açın ve şirket uygulamaları için çoklu oturum açma (SSO) sağlamanıza olanak sağlar. BT yöneticiniz, şirket kaynaklarına erişmek için bir iş hesabı eklemeye gerektirebilir. SSO, bir kez oturum açın ve şirketiniz için kullanılabilir hale getirdiği tüm uygulamalar arasında oturum açma otomatik olarak yararlanabilmek sağlar.

## <a name="installing-the-azure-authenticator-app"></a>Azure Authenticator uygulamasını yükleme
Azure Authenticator uygulamasını Google Play Store ' yükleyebilirsiniz.
Samsung Android cihaz vs Samsung olmayan Android cihazından iş hesabını eklemek için yönergeleri biraz farklıdır. Her ikisi için yönergeler aşağıda listelenmiştir.

## <a name="adding-the-work-account-from-samsung-android-device"></a>Samsung Android cihazından iş hesabı ekleme
### <a name="adding-the-work-account-through-the-app-home-screen"></a>Uygulamayı giriş ekranına aracılığıyla iş hesabı ekleme
Aşağıdaki yönergeleri, Samsung GS3 ve telefonlar veya not2 yukarıda ve tabletler yukarıda geçerlidir.

1. Uygulamanın giriş ekranında, son kullanıcı lisans sözleşmesi (EULA) kabul edin.
2. Hesap Etkinleştir ekranında, bağlam menüsünü seçin ve sağ tıklatın **iş hesabı**.
3. Hesap Ekle ekranında seçin ** iş hesabı **.
4. Etkinleştirme cihaz Yöneticisi ekranında tıklayın **etkinleştirme**.
5. Gizlilik İlkesi ekranında, onay kutusunu işaretleyin ve **Onayla**.
6. Çalışma alanına katılma ekrana tıklayın ve kuruluş tarafından sağlanan kullanıcı kimliği girin **katılın**.
7. Azure Authenticator uygulaması için oturum açmak için kuruluş hesabınızı ve parolanızı girin ve **oturum**.
8. Multi-Factor authentication (MFA) hakkında bilgi görüntüler sonraki ekranda güvenlik için eklenir ve isteğe bağlıdır. İş hesabı oluşturmak için İş yeriniz veya okulunuz ikinci faktör kimlik doğrulaması gerektiriyorsa, bu ekran görürsünüz. Bunu daha da hesabınızı doğrulamak için yönergeler sağlar.
9. Çalışma alanına katılma ekran iletisini görüntüler "**, çalışma alanına katılma**". Azure authenticator uygulaması, Cihazınızı, çalışma alanına katılma çalışıyor.
10. Sonraki ekranda çalışma alanına katılmış iletisini görmeniz gerekir.

> [!NOTE]
> Cihazınızda tek bir iş hesabı verilir.
> 
> 

### <a name="adding-the-work-account-from-the-settings-menu"></a>Ayarlar menüsünden iş hesabı ekleme
Azure Authenticator uygulamasını yükledikten sonra Android Hesap Yöneticisi'nden bir iş hesabı da oluşturabilirsiniz.

1. Ayarlar menüsünden gidin **hesapları** tıklatıp **hesabı Ekle**.
2. Bir iş hesabı eklemeye uygulama ana ekran aracılığıyla iş hesabı ekleme, yordam 3-10 adımları izleyin.

## <a name="adding-the-work-account-from-a-non-samsung-android-device"></a>Bir Samsung Android CİHAZDAN iş hesabı ekleme
### <a name="adding-the-work-account-through-the-app-home-screen"></a>Uygulamayı giriş ekranına aracılığıyla iş hesabı ekleme
1. Uygulamanın giriş ekranında, son kullanıcı lisans sözleşmesi (EULA) kabul edin.
2. Hesap Etkinleştir ekranında, bağlam menüsünü seçin ve sağ tıklatın **iş hesabı**.
3. Hesapları ekrana tıklayın **hesabı Ekle**.
4. Hesaplar ekranı görürseniz, tıklayın **Hesap Ekle**. Bir iş hesabı zaten daha önce oluşturulduysa, iş hesabı var olan gösteren bir eşitleme ekran görürsünüz. İş hesabı yalnızca giriş ekranına geri okuna dokunarak tutabilirsiniz. Alternatif olarak, kaldırın ve yeniden üzerinde Workplace Join yeni bir iş hesabı ekran, kuruluşunuz tarafından sağlanan kullanıcı kimliği girin ve Katıl'ı tıklatın, hesabı seçebilirsiniz.
5. Azure Authenticator uygulaması için oturum açmak için kuruluş hesabınızı ve parolanızı girin ve **oturum**.
6. Multi-Factor authentication (MFA) hakkında bilgi görüntüler sonraki ekranda güvenlik için eklenir ve isteğe bağlıdır. İş hesabı oluşturmak için İş yeriniz veya okulunuz ikinci faktör kimlik doğrulaması gerektiriyorsa, bu ekran görürsünüz. Bunu daha da hesabınızı doğrulamak için yönergeler sağlar.
7. Tıklayın **Tamam** sonraki ekranda. Sertifika adını değiştirmeyin.
   ", çalışma alanına katılma" iletisi. Azure authenticator uygulaması, Cihazınızı, çalışma alanına katılma çalışıyor.
   Sonraki ekranda çalışma alanına katılmış iletisini görmeniz gerekir.

> [!NOTE]
> Cihazınızda tek bir iş hesabı verilir.
> 
> 

Azure Authenticator uygulamasını yükledikten sonra Android Hesap Yöneticisi'nden bir iş hesabı da oluşturabilirsiniz.

1. Gelen **ayarları** menüsünden hesaplarına gidin ve tıklayın **hesabı Ekle**.
2. Bir iş hesabı eklemeye aracılığıyla uygulama giriş ekranı **, iş hesabı ekleme yordamı, 2-7 adımları izleyin.

### <a name="how-to-find-out-which-version-is-installed"></a>Nasıl hangi sürümünün yüklü olduğunu öğrenin
1. Azure Authenticator uygulamanın hangi sürümünün bulabilirsiniz ve Cihazınızda ilişkili hizmet sürümleri yüklenir.
2. Pop menüsünü tıklatın **hakkında**.
3. Hakkında ekranı cihazınıza yüklenen sürümler ile uygulama ve hizmetleri görüntüler.

### <a name="sending-log-files-to-report-issues"></a>Sorunları bildirmek için gönderen günlük dosyaları
1. Microsoft Azure Authenticator uygulaması ile olay bildirmek, bir olay numarasını almak ve atanmış olay sayısını karşı günlük dosyaları gibi Gönder Support yönergeleri izleyin:
2. Pop menüsünü tıklatın **günlüğü**.
3. Microsoft Support açık bir olayı varsa (buna bir sonraki adım için ihtiyacınız olacak) olay numarasını not edin. Zaten bir destek olayı oluşturmadıysanız ve Yardım isterseniz, adresindeki yönergeleri [Microsoft Support](https://support.microsoft.com/contactus) yeni bir olayı açmak için.
4. Günlük ekrana tıklayın **Şimdi Gönder**.
5. Kullanmak istediğiniz e-posta sağlayıcısı seçin.
6. Microsoft Support açık bir olayı zaten varsa, sorununuzu için günlük verileri göndermek nasıl öğrenmek için atanan destek mühendisine başvurun ve olayınız ile ilişkilendirdiniz. Destek Mühendisi e-posta adresi ve konu satırı için daha fazla bilgi sağlayacaktır. Lütfen bir destek olayı zaten yoksa, yeni bir olayı açmak için Microsoft Support adresindeki yönergeleri izleyin.
7. Düzen **için** satır ve **konu** Microsoft Support almış bilgilerle satır.
8. Azure Authenticator uygulaması gönderdiğiniz e-posta için günlük dosyasına ekler. Yaşadığınız sorunu açıklayın (isteğe bağlı) alıcı listesi güncelleştirin ve e-posta gönderin.

### <a name="deleting-the-work-account-and-leaving-your-workplace"></a>Çalışma alanınızı bırakarak ve iş hesabı siliniyor
Şu şekilde oluşturduğunuz herhangi bir zamanda iş hesabını kaldırabilirsiniz:

**Ayarlar menüsünden iş hesabını silmek için**

1. Hesapları Yöneticisi'nden seçin **iş hesabı**.
2. İş hesabı ekranında, içinde **genel ayarlar**seçin **hesap ayarları – iş yeri ağına bırakın**.
3. Seçin **bırakın** üzerinde **çalışma alanına katılma** ekran.
4. Tıklayın **Tamam** ne zaman "emin çalışma alanına çıkmak istediğinizden olan" iletisi görüntülenir.
5. Bu, iş hesabınızı işyerinizden sildiniz sağlar.

> [!NOTE]
> Bu seçenek, Android, önceki bazı sürümlerinde çalışmayabilir bir iş hesabı silmek için hesap kaldırma seçeneğini kullanmayın önerilir.
> 
> 

## <a name="uninstalling-the-app"></a>Uygulamayı kaldırma
Bir Samsung Android cihazda, cihaz yönetici ayrıcalıkları gibi kaldırmadan önce kaldırılmalıdır 

1. Gelen **ayarları**altında **sistem**seçin **güvenlik**.
2. D içinde**aygıt Yönetim**, tıklayın **cihaz yöneticileri**. Onay kutusunun yanındaki emin **Azure Authenticator** temizlenir.

## <a name="troubleshooting"></a>Sorun giderme
Görürseniz **Keystore hata**, kilit sahip olmadığınızdan bu olabilir ekran kümesi oluşturan bir PIN ile. Bu sorunu çözmek için Azure Authenticator uygulamasını kaldırmak, şirketiniz kilit ekranının PIN yapılandırmak ve uygulamayı yeniden yükleyin.

