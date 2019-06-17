---
title: StorSimple 8000 serisi cihaz için CHAP yapılandırma | Microsoft Docs
description: Karşılıklı Kimlik Doğrulama Protokolü (CHAP) bir StorSimple cihazında yapılandırılması açıklanmaktadır.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: TBD
ms.date: 05/09/2018
ms.author: alkohli
ms.openlocfilehash: efc116c278bfe72419800603a3b365f461fe0a28
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60362858"
---
# <a name="configure-chap-for-your-storsimple-device"></a>StorSimple cihazınız için CHAP yapılandırma

Bu öğreticide, StorSimple cihazınız için CHAP yapılandırma açıklanmaktadır. Bu makalede ayrıntılı bir yordam, StorSimple 8000 serisi cihazlar için geçerlidir.

CHAP için karşılıklı kimlik doğrulama protokolü anlamına gelir. Bu, uzak istemcilerin kimliğini doğrulamak için sunucuları tarafından kullanılan bir kimlik doğrulama düzeni olur. Doğrulama bir paylaşılan parola veya gizli dizi temel alır. CHAP (tek yönlü) bir yolu olabilir veya karşılıklı (çift yönlü). Hedef bir başlatıcı doğruladığında bir CHAP yoludur. Karşılıklı veya çift ters CHAP Başlatıcı Hedef doğrular ve hedef Başlatıcı'kimliğini doğrular. Başlatıcı kimlik doğrulaması olmadan hedef kimlik doğrulaması uygulanabilir. Ancak, hedef kimlik doğrulaması, yalnızca başlatıcı kimlik doğrulaması da uygulanmışsa uygulanabilir.

En iyi uygulama, iSCSI güvenliğini artırmak için CHAP kullanmanızı öneririz.

> [!NOTE]
> IPSec StorSimple cihazları üzerinde şu anda desteklenmediğini göz önünde bulundurun.

StorSimple cihazında CHAP ayarları, aşağıdaki yollarla yapılandırılabilir:

* Tek yönlü veya tek yönlü kimlik doğrulaması
* Çift yönlü veya karşılıklı veya çift ters kimlik doğrulaması

Her durumda, cihaz ve sunucu iSCSI Initiator yazılımı için portalı yapılandırılması gerekiyor. Aşağıdaki Öğreticide bu yapılandırma için ayrıntılı adımlar açıklanmaktadır.

## <a name="unidirectional-or-one-way-authentication"></a>Tek yönlü veya tek yönlü kimlik doğrulaması

Tek yönlü kimlik doğrulamasında başlatıcının hedef kimliğini doğrular. Bu kimlik doğrulaması, StorSimple cihaz ve iSCSI Initiator yazılımı konaktaki CHAP Başlatıcı ayarları yapılandırmanız gerekir. StorSimple cihaz ve Windows konak için ayrıntılı yordamlar sonraki açıklanmaktadır.

#### <a name="to-configure-your-device-for-one-way-authentication"></a>Tek yönlü kimlik doğrulaması için Cihazınızı yapılandırmak için

1. Azure portalında StorSimple cihaz Yöneticisi hizmetinize gidin. Tıklayın **cihazları** seçin ve CHAP yapılandırmak istediğiniz bir cihaza tıklayın. Git **cihaz Ayarları > Güvenlik**. İçinde **güvenlik ayarları** dikey penceresinde tıklayın **CHAP**.
   
    ![CHAP Başlatıcı](./media/storsimple-8000-configure-chap/configure-chap5.png)
2. İçinde **CHAP** dikey penceresinde ve **CHAP Başlatıcısı** bölümü:
   
   1. CHAP Başlatıcı kullanıcı adı sağlayın.
   2. CHAP Başlatıcısı için bir parola sağlayın.
      
      > [!IMPORTANT]
      > CHAP kullanıcı adı, 233'den az karakter içermelidir. CHAP parolasının uzunluğu 12 ile 16 karakter arasında olmalıdır. Uzun bir kullanıcı adı ve parola kimlik doğrulama hatası Windows konağında sonuçlanır.
   
   3. Parolayı onaylayın.

       ![CHAP Başlatıcı](./media/storsimple-8000-configure-chap/configure-chap6.png)
3. **Kaydet**’e tıklayın. Bir onay iletisi görüntülenir. Değişiklikleri kaydetmek için **Tamam** 'a tıklayın.

#### <a name="to-configure-one-way-authentication-on-the-windows-host-server"></a>Tek yönlü kimlik doğrulaması Windows ana bilgisayar sunucusunda yapılandırmak için
1. Windows ana bilgisayarı sunucusunda, iSCSI Başlatıcısı'nı başlatın.
2. İçinde **iSCSI başlatıcısı özellikleri** penceresinde aşağıdaki adımları gerçekleştirin:
   
   1. Tıklayın **bulma** sekmesi.
      
       ![iSCSI başlatıcısı özellikleri](./media/storsimple-configure-chap/IC740944.png)
   2. Tıklayın **Portal Bul**.
3. İçinde **Hedef Portal Bul** iletişim kutusunda:
   
   1. Cihazınızın IP adresini belirtin.
   2. **Gelişmiş**'e tıklayın.
      
       ![Hedef portalı bulun](./media/storsimple-configure-chap/IC740945.png)
4. İçinde **Gelişmiş ayarlar** iletişim kutusunda:
   
   1. Seçin **etkinleştirme CHAP oturum** onay kutusu.
   2. İçinde **adı** alan, Azure portalında CHAP Başlatıcısı için belirtilen kullanıcı adı sağlayın.
   3. İçinde **hedef gizli** alan, Azure portalında CHAP Başlatıcısı için belirttiğiniz parolayı girin.
   4. **Tamam**'ı tıklatın.
      
       ![Gelişmiş ayarlar genel](./media/storsimple-configure-chap/IC740946.png)
5. Üzerinde **hedefleri** sekmesinde **iSCSI başlatıcısı özellikleri** penceresinde cihazın durumu olarak görünmelidir **bağlı**. Bir StorSimple 1200 cihaz kullanıyorsanız, her birim bir iSCSI hedefi olarak bağlanır. Bu nedenle, 3-4 arası adımları her birim için yinelenen gerekir.
   
    ![Ayrı hedefler olarak bağlanmış birimleri](./media/storsimple-configure-chap/chap4.png)
   
   > [!IMPORTANT]
   > İSCSI adını değiştirirseniz, yeni ad yeni iSCSI oturumları için kullanılır. Yeni ayarları oturumunuzu kadar oturumlar var ve oturum açma için tekrar kullanılmaz.

Windows ana bilgisayar sunucusunda CHAP yapılandırma hakkında daha fazla bilgi için Git [ek hususlar](#additional-considerations).

## <a name="bidirectional-or-mutual-authentication"></a>Çift yönlü veya karşılıklı kimlik doğrulaması

Çift yönlü kimlik hedef Başlatıcı doğrular ve ardından hedef Başlatıcı kimliğini doğrular. Bu yordam, CHAP ayarları cihaz ve iSCSI Initiator yazılımı konaktaki ters CHAP Başlatıcısı ayarlarını yapılandırmak kullanıcının gerektirir. Aşağıdaki yordamlar, cihazda ve Windows konağında karşılıklı kimlik doğrulamasını yapılandırmak için gereken adımları açıklar.

#### <a name="to-configure-your-device-for-mutual-authentication"></a>Karşılıklı kimlik doğrulama için Cihazınızı yapılandırmak için

1. Azure portalında StorSimple cihaz Yöneticisi hizmetinize gidin. Tıklayın **cihazları** seçin ve CHAP yapılandırmak istediğiniz bir cihaza tıklayın. Git **cihaz Ayarları > Güvenlik**. İçinde **güvenlik ayarları** dikey penceresinde tıklayın **CHAP**.
   
    ![CHAP hedefi](./media/storsimple-8000-configure-chap/configure-chap5.png)
2. Bu sayfada ve buna aşağı kaydırın **CHAP hedefini** bölümü:
   
   1. Sağlayan bir **ters CHAP kullanıcı adı** cihazınız için.
   2. Tedarik bir **ters CHAP parolası** cihazınız için.
   3. Parolayı onaylayın.
3. İçinde **CHAP Başlatıcısı** bölümü:
   
   1. Sağlayan bir **kullanıcı adı** cihazınız için.
   2. Sağlayan bir **parola** cihazınız için.
   3. Parolayı onaylayın.

       ![CHAP Başlatıcı](./media/storsimple-8000-configure-chap/configure-chap11.png)
4. **Kaydet**’e tıklayın. Bir onay iletisi görüntülenir. Değişiklikleri kaydetmek için **Tamam** 'a tıklayın.

#### <a name="to-configure-bidirectional-authentication-on-the-windows-host-server"></a>Windows ana bilgisayar sunucusunda çift yönlü kimlik doğrulaması yapılandırmak için

1. Windows ana bilgisayarı sunucusunda, iSCSI Başlatıcısı'nı başlatın.
2. İçinde **iSCSI başlatıcısı özellikleri** penceresinde tıklayın **yapılandırma** sekmesi.
3. Tıklayın **CHAP**.
4. İçinde **iSCSI Başlatıcı Karşılıklı CHAP parolası** iletişim kutusunda:
   
   1. Tür **ters CHAP parolası** Azure portalında yapılandırılmış.
   2. **Tamam** düğmesine tıklayın.
      
       ![iSCSI Başlatıcı Karşılıklı CHAP parolası](./media/storsimple-configure-chap/IC740949.png)
5. Tıklayın **hedefleri** sekmesi.
6. Tıklayın **Connect** düğmesi. 
7. İçinde **bağlanmak için hedef** iletişim kutusu, tıklayın **Gelişmiş**.
8. İçinde **Gelişmiş Özellikler** iletişim kutusunda:
   
   1. Seçin **etkinleştirme CHAP oturum** onay kutusu.
   2. İçinde **adı** alan, Azure portalında CHAP Başlatıcısı için belirtilen kullanıcı adı sağlayın.
   3. İçinde **hedef gizli** alan, Azure portalında CHAP Başlatıcısı için belirttiğiniz parolayı girin.
   4. Seçin **karşılıklı kimlik doğrulaması gerçekleştirme** onay kutusu.
      
       ![Gelişmiş ayarlar ile karşılıklı kimlik doğrulaması](./media/storsimple-configure-chap/IC740950.png)
   5. Tıklayın **Tamam** CHAP yapılandırmasını tamamlamak için

Windows ana bilgisayar sunucusunda CHAP yapılandırma hakkında daha fazla bilgi için Git [ek hususlar](#additional-considerations).

## <a name="additional-considerations"></a>Diğer konular

**Hızlı Bağlan** özellik CHAP etkin olan bağlantılarını desteklemiyor. CHAP etkin olduğunda, kullandığınızdan emin olun **Connect** kullanılabilir düğme **hedefleri** bir hedef bağlanmak için sekmesinde.

![Hedefe bağlanın](./media/storsimple-configure-chap/IC740947.png)

İçinde **hedef Bağlan** sunulur, iletişim kutusu seç **Bu bağlantı sık kullanılan hedefler listesine Ekle** onay kutusu. Bu seçim, her zaman bilgisayarı yeniden başlatır, girişiminde bağlantı geri yüklemek için iSCSI sık kullanılan hedefler için yapılmasını sağlar.

## <a name="errors-during-configuration"></a>Yapılandırma sırasında hatalar

CHAP yapılandırma yanlış sonra görmek muhtemelen bir **kimlik doğrulama hatası** hata iletisi.

## <a name="verification-of-chap-configuration"></a>CHAP yapılandırma doğrulama

Aşağıdaki adımları tamamlayarak CHAP kullanılmakta olduğunu doğrulayabilirsiniz.

#### <a name="to-verify-your-chap-configuration"></a>CHAP yapılandırmanızı doğrulamak için
1. Tıklayın **sık kullanılan hedefler**.
2. Kimlik doğrulaması etkin hedef seçin.
3. Tıklayın **ayrıntıları**.
   
    ![iSCSI Başlatıcı özellikleri sık kullanılan hedefler](./media/storsimple-configure-chap/IC740951.png)
4. İçinde **sık kullanılan hedef ayrıntıları** iletişim kutusunda, Not girişi **kimlik doğrulaması** alan. Yapılandırma başarılı olup olmadığını, yazması gerekir **CHAP**.
   
    ![Sık kullanılan hedef ayrıntıları](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [StorSimple güvenlik](storsimple-8000-security.md).
* Daha fazla bilgi edinin [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanarak](storsimple-8000-manager-service-administration.md).

