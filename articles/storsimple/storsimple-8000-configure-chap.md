---
title: "StorSimple 8000 serisi aygıt için CHAP yapılandırma | Microsoft Docs"
description: "Karşılıklı Kimlik Doğrulama Protokolü (CHAP) StorSimple cihazında yapılandırılması açıklanmaktadır."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: TBD
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: 61e0877187759d76b6f7efcef0a5ed8bec8500fe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-chap-for-your-storsimple-device"></a>StorSimple cihazınız için CHAP yapılandırma

Bu öğretici, StorSimple cihazınız için CHAP yapılandırma açıklanmaktadır. Bu makalede ayrıntılı yordam StorSimple 8000 serisi cihazlar için geçerlidir.

CHAP karşılıklı kimlik doğrulama protokolü için anlamına gelir. Bu sunucuları tarafından uzak istemcilerin kimliğini doğrulamak için kullanılan bir kimlik doğrulama düzeni olur. Doğrulama bir paylaşılan parola veya gizli temel alır. CHAP (tek yönlü) bir yolu olabilir veya karşılıklı (çift yönlü). Hedef Başlatıcı doğruladığında bir CHAP yoludur. Karşılıklı veya ters CHAP Başlatıcı Hedef doğrular ve hedef Başlatıcı kimliğini doğrular. Başlatıcı kimlik doğrulaması olmadan hedef kimlik doğrulaması uygulanabilir. Ancak, yalnızca başlatıcı kimlik doğrulaması aynı zamanda uygulanırsa, hedef kimlik doğrulama uygulanabilir.

En iyi uygulama, iSCSI güvenliğini artırmak için CHAP kullanmanızı öneririz.

> [!NOTE]
> IPSec StorSimple cihazlarda şu anda desteklenmiyor aklınızda bulundurun.

StorSimple cihazında CHAP ayarları aşağıdaki yollarla yapılandırılabilir:

* Tek yönlü veya tek yönlü kimlik doğrulaması
* Çift yönlü veya karşılıklı veya ters kimlik doğrulaması

Her durumda, cihaz ve sunucu iSCSI Initiator yazılımı için portal yapılandırılması gerekiyor. Bu yapılandırma için ayrıntılı adımlar aşağıdaki öğreticide açıklanmaktadır.

## <a name="unidirectional-or-one-way-authentication"></a>Tek yönlü veya tek yönlü kimlik doğrulaması

Tek yönlü kimlik doğrulamada hedef Başlatıcı kimliğini doğrular. Bu kimlik doğrulama StorSimple cihazı ve iSCSI başlatıcısı yazılımı CHAP Başlatıcı ayarları yapılandırmanız gerekir. StorSimple cihazı ve Windows ana bilgisayar için ayrıntılı yordamlar sonraki açıklanmaktadır.

#### <a name="to-configure-your-device-for-one-way-authentication"></a>Tek yönlü kimlik doğrulama için Cihazınızı yapılandırmak için

1. Azure portalında StorSimple cihaz Yöneticisi hizmetinize gidin. Tıklatın **aygıtları** seçin ve istediğiniz CHAP yapılandırmak için bir cihaza tıklayın. Git **Aygıt Ayarları > Güvenlik**. İçinde **güvenlik ayarları** dikey penceresinde tıklatın **CHAP**.
   
    ![CHAP Başlatıcı](./media/storsimple-8000-configure-chap/configure-chap5.png)
2. İçinde **CHAP** dikey penceresinde ve **CHAP Başlatıcı** bölümü:
   
   1. CHAP başlatıcı için bir kullanıcı adı sağlayın.
   2. CHAP başlatıcı için bir parola sağlayın.
      
    > [!IMPORTANT]
    > CHAP kullanıcı adı 233'den az karakter içermelidir. CHAP parolası 12 ile 16 karakter arasında olmalıdır. Uzun bir kullanıcı adı ve parola kimlik doğrulama hatası Windows ana bilgisayardaki sonuçlanır.
   
   3. Parolayı onaylayın.

       ![CHAP Başlatıcı](./media/storsimple-8000-configure-chap/configure-chap6.png)
3. **Kaydet** düğmesine tıklayın. Bir onay iletisi görüntülenir. Değişiklikleri kaydetmek için **Tamam**’a tıklayın.

#### <a name="to-configure-one-way-authentication-on-the-windows-host-server"></a>Windows ana bilgisayar sunucusunda tek yönlü kimlik doğrulamasını yapılandırmak için
1. Windows ana bilgisayarı sunucusunda, iSCSI başlatıcısı başlatın.
2. İçinde **iSCSI başlatıcısı özellikleri** penceresinde, aşağıdaki adımları gerçekleştirin:
   
   1. Tıklatın **bulma** sekmesi.
      
       ![iSCSI başlatıcısı özellikleri](./media/storsimple-configure-chap/IC740944.png)
   2. Tıklatın **Portal Bul**.
3. İçinde **Hedef Portal Bul** iletişim kutusunda:
   
   1. Cihazınızı IP adresini belirtin.
   2. Tıklatın **Gelişmiş**.
      
       ![Hedef portalı Bul](./media/storsimple-configure-chap/IC740945.png)
4. İçinde **Gelişmiş ayarları** iletişim kutusunda:
   
   1. Seçin **CHAP'yi etkinleştir oturum** onay kutusu.
   2. İçinde **adı** alanında, Klasik portalda CHAP başlatıcı için belirtilen kullanıcı adı girin.
   3. İçinde **hedef gizli** alan, Klasik portalda CHAP başlatıcı için belirtilen parola sağlayın.
   4. **Tamam** düğmesine tıklayın.
      
       ![Gelişmiş ayarlar genel](./media/storsimple-configure-chap/IC740946.png)
5. Üzerinde **hedefleri** sekmesinde **iSCSI başlatıcısı özellikleri** penceresinde, cihaz durumu olarak görünmelidir **bağlı**. Bir StorSimple 1200 cihaz kullanıyorsanız, her birimin bir iSCSI hedefi olarak bağlı. Bu nedenle, 3-4 arası adımlar, her birim için yinelenmesi gerekir.
   
    ![Ayrı hedefleri olarak takılı birimleri](./media/storsimple-configure-chap/chap4.png)
   
   > [!IMPORTANT]
   > İSCSI adını değiştirirseniz, yeni ad yeni iSCSI oturumları için kullanılır. Yeni ayarları oturumunuzu kapattığınızda kadar oturumlar var ve oturum açma için tekrar kullanılmaz.

Windows ana bilgisayar sunucusunda CHAP yapılandırma hakkında daha fazla bilgi için Git [ek hususlar](#additional-considerations).

## <a name="bidirectional-or-mutual-authentication"></a>Çift yönlü veya karşılıklı kimlik doğrulaması

Çift yönlü kimlik doğrulama, hedef Başlatıcı kimliğini doğrular ve hedef Başlatıcı kimliğini doğrular. Bu yordam kullanıcıya CHAP Başlatıcı ayarlarını yapılandırabilir, cihaz ve iSCSI başlatıcısı yazılımı ters CHAP ayarları gerektirir. Aşağıdaki yordamlar, Windows ana bilgisayar ve aygıt üzerinde karşılıklı kimlik doğrulaması yapılandırmak için gereken adımları açıklar.

#### <a name="to-configure-your-device-for-mutual-authentication"></a>Karşılıklı kimlik doğrulama için Cihazınızı yapılandırmak için

1. Azure portalında StorSimple cihaz Yöneticisi hizmetinize gidin. Tıklatın **aygıtları** seçin ve istediğiniz CHAP yapılandırmak için bir cihaza tıklayın. Git **Aygıt Ayarları > Güvenlik**. İçinde **güvenlik ayarları** dikey penceresinde tıklatın **CHAP**.
   
    ![CHAP hedefi](./media/storsimple-8000-configure-chap/configure-chap5.png)
2. Bu sayfada ve buna aşağı kaydırın **CHAP hedefi** bölümü:
   
   1. Sağlayan bir **ters CHAP kullanıcı adı** cihazınız için.
   2. Tedarik bir **ters CHAP parolası** cihazınız için.
   3. Parolayı onaylayın.
3. İçinde **CHAP Başlatıcı** bölümü:
   
   1. Sağlayan bir **kullanıcı adı** cihazınız için.
   2. Sağlayan bir **parola** cihazınız için.
   3. Parolayı onaylayın.

       ![CHAP Başlatıcı](./media/storsimple-8000-configure-chap/configure-chap11.png)
4. **Kaydet** düğmesine tıklayın. Bir onay iletisi görüntülenir. Değişiklikleri kaydetmek için **Tamam**’a tıklayın.

#### <a name="to-configure-bidirectional-authentication-on-the-windows-host-server"></a>Windows ana bilgisayar sunucusunda çift yönlü kimlik doğrulamasını yapılandırmak için

1. Windows ana bilgisayarı sunucusunda, iSCSI başlatıcısı başlatın.
2. İçinde **iSCSI başlatıcısı özellikleri** penceresinde tıklatın **yapılandırma** sekmesi.
3. Tıklatın **CHAP**.
4. İçinde **iSCSI Başlatıcı Karşılıklı CHAP parolası** iletişim kutusunda:
   
   1. Tür **ters CHAP parolası** Azure Portalı'nda yapılandırılmış.
   2. **Tamam** düğmesine tıklayın.
      
       ![iSCSI Başlatıcı Karşılıklı CHAP parolası](./media/storsimple-configure-chap/IC740949.png)
5. Tıklatın **hedefleri** sekmesi.
6. Tıklatın **Bağlan** düğmesi. 
7. İçinde **bağlanmak için hedef** iletişim kutusu, tıklatın **Gelişmiş**.
8. İçinde **Gelişmiş Özellikler** iletişim kutusunda:
   
   1. Seçin **CHAP'yi etkinleştir oturum** onay kutusu.
   2. İçinde **adı** alanında, Klasik portalda CHAP başlatıcı için belirtilen kullanıcı adı girin.
   3. İçinde **hedef gizli** alan, Klasik portalda CHAP başlatıcı için belirtilen parola sağlayın.
   4. Seçin **gerçekleştirme karşılıklı kimlik doğrulaması** onay kutusu.
      
       ![Gelişmiş ayarlar karşılıklı kimlik doğrulaması](./media/storsimple-configure-chap/IC740950.png)
   5. Tıklatın **Tamam** CHAP yapılandırmasını tamamlamak için

Windows ana bilgisayar sunucusunda CHAP yapılandırma hakkında daha fazla bilgi için Git [ek hususlar](#additional-considerations).

## <a name="additional-considerations"></a>Diğer konular

**Hızlı Bağlan** özelliği etkinleştirilmiş CHAP sahip bağlantıları desteklemiyor. CHAP etkinleştirildiğinde kullandığınızdan emin olun **Bağlan** kullanılabilir düğmesi **hedefleri** sekmesinde bir hedef bağlanmak için.

![Hedefe Bağlan](./media/storsimple-configure-chap/IC740947.png)

İçinde **hedef Bağlan** sunulur, iletişim kutusu seç **Bu bağlantı sık kullanılan hedefler listesine eklemek** onay kutusu. Bu seçim, bilgisayar yeniden başlatıldığında her girişiminde bağlantı iSCSI sık kullanılan hedefler geri yüklemek için yapılan sağlar.

## <a name="errors-during-configuration"></a>Yapılandırma sırasında hatalar

CHAP yapılandırması doğru değil sonra görmek büyük olasılıkla bir **kimlik doğrulama hatası** hata iletisi.

## <a name="verification-of-chap-configuration"></a>Doğrulama CHAP yapılandırma

Aşağıdaki adımları tamamlayarak CHAP kullanılmakta olduğunu doğrulayabilirsiniz.

#### <a name="to-verify-your-chap-configuration"></a>CHAP yapılandırmasını doğrulamak için
1. Tıklatın **sık kullanılan hedefler**.
2. Kimlik doğrulamasının etkin hedef seçin.
3. Tıklatın **ayrıntıları**.
   
    ![iSCSI Başlatıcı özellikleri sık kullanılan hedefler](./media/storsimple-configure-chap/IC740951.png)
4. İçinde **sık kullanılan hedef ayrıntıları** iletişim kutusunda, giriş Not **kimlik doğrulaması** alan. Yapılandırma başarılı olup olmadığını, yazması gerekir **CHAP**.
   
    ![Sık kullanılan hedef ayrıntıları](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinmek [StorSimple güvenlik](storsimple-8000-security.md).
* Daha fazla bilgi edinmek [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanarak](storsimple-8000-manager-service-administration.md).

