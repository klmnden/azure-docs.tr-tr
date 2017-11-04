---
title: "StorSimple 8000 serisi aygıt için CHAP yapılandırma | Microsoft Docs"
description: "Karşılıklı Kimlik Doğrulama Protokolü (CHAP) StorSimple cihazında yapılandırılması açıklanmaktadır."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 467044d7-7885-4382-90bd-3148dbbd341f
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: TBD
ms.date: 11/02/2017
ms.author: alkohli
ms.openlocfilehash: 83ad256522ca19a19b3fe46fcc48e9cb37cbe246
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="configure-chap-for-your-storsimple-device"></a>StorSimple cihazınız için CHAP yapılandırma
> [!NOTE]
> StorSimple için Klasik portalı kullanım dışıdır. StorSimple cihaz yöneticileri yeni Azure portalına kullanımdan zamanlamaya göre otomatik olarak taşır. Bir e-posta ve bu taşıma için portal bir bildirim alırsınız. Bu belgede ayrıca yakında kullanımdan kaldırılacaktır. Bu makalede yeni Azure portalına için sürümünü görüntülemek için şu adrese gidin [StorSimple cihazınız için CHAP yapılandırma](storsimple-8000-configure-chap.md). Taşıma hakkında herhangi bir sorunuz için bkz: [SSS: Azure portalına taşıma](storsimple-8000-move-azure-portal-faq.md).

Bu öğretici, StorSimple cihazınız için CHAP yapılandırma açıklanmaktadır. Bu makalede ayrıntılı yordam StorSimple 1200 aygıtların yanı sıra StorSimple 8000 serisi için geçerlidir.

CHAP karşılıklı kimlik doğrulama protokolü için anlamına gelir. Bu sunucuları tarafından uzak istemcilerin kimliğini doğrulamak için kullanılan bir kimlik doğrulama düzeni olur. Doğrulama bir paylaşılan parola veya gizli temel alır. CHAP tek yönlü olabilir (tek yönlü) veya karşılıklı (çift yönlü). Hedef Başlatıcı doğruladığında tek yönlü CHAP türüdür. Karşılıklı veya ters CHAP, diğer yandan, hedef, başlatıcı kimlik doğrulaması yapmak ve Başlatıcı Hedef kimlik doğrulaması yapmak gerektirir. Başlatıcı kimlik doğrulaması olmadan hedef kimlik doğrulaması uygulanabilir. Ancak, yalnızca başlatıcı kimlik doğrulaması aynı zamanda uygulanırsa, hedef kimlik doğrulama uygulanabilir. 

En iyi uygulama, iSCSI güvenliğini artırmak için CHAP kullanmanızı öneririz.

> [!NOTE]
> IPSec StorSimple cihazlarda şu anda desteklenmiyor aklınızda bulundurun.
> 
> 

StorSimple cihazında CHAP ayarları aşağıdaki yollarla yapılandırılabilir:

* Tek yönlü veya tek yönlü kimlik doğrulaması
* Çift yönlü veya karşılıklı veya ters kimlik doğrulaması

Her durumda, cihaz ve sunucu iSCSI Initiator yazılımı için portal yapılandırılması gerekiyor. Bu yapılandırma için ayrıntılı adımlar aşağıdaki öğreticide açıklanmaktadır.

## <a name="unidirectional-or-one-way-authentication"></a>Tek yönlü veya tek yönlü kimlik doğrulaması
Tek yönlü kimlik doğrulamada hedef Başlatıcı kimliğini doğrular. Bu kimlik doğrulama StorSimple cihazı ve iSCSI başlatıcısı yazılımı CHAP Başlatıcı ayarları yapılandırmanız gerekir. StorSimple cihazı ve Windows ana bilgisayar için ayrıntılı yordamlar sonraki açıklanmaktadır.

#### <a name="to-configure-your-device-for-one-way-authentication"></a>Tek yönlü kimlik doğrulama için Cihazınızı yapılandırmak için
1. Azure Klasik portalında üzerinde **aygıtları** sayfasında, **yapılandırma** sekmesi.
   
    ![CHAP Başlatıcı](./media/storsimple-configure-chap/IC740943.png)
2. Bu sayfada ve buna aşağı kaydırın **CHAP Başlatıcı** bölümü:
   
   1. CHAP başlatıcı için bir kullanıcı adı sağlayın.
   2. CHAP başlatıcı için bir parola sağlayın.
      
    > [!IMPORTANT]
    > CHAP kullanıcı adı 233'den az karakter içermelidir. CHAP parolası 12 ile 16 karakter arasında olmalıdır. Uzun bir kullanıcı adı ve parola kimlik doğrulama hatası Windows ana bilgisayardaki sonuçlanır.
   
   3. Parolayı onaylayın.
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
5. Üzerinde **hedefleri** sekmesinde **iSCSI başlatıcısı özellikleri** penceresinde, cihaz durumu olarak görünmelidir **bağlı**. Bir StorSimple 1200 cihaz kullanıyorsanız, ardından her birim bir iSCSI hedefi olarak aşağıda gösterildiği gibi bağlanır. Bu nedenle, 3-4 arası adımlar, her birim için yinelenmesi gerekir.
   
    ![Ayrı hedefleri olarak takılı birimleri](./media/storsimple-configure-chap/chap4.png)
   
   > [!IMPORTANT]
   > İSCSI adını değiştirirseniz, yeni ad yeni iSCSI oturumları için kullanılır. Yeni ayarları oturumunuzu kapattığınızda kadar oturumlar var ve oturum açma için tekrar kullanılmaz.
   > 
   > 

Windows ana bilgisayar sunucusunda CHAP yapılandırma hakkında daha fazla bilgi için Git [ek hususlar](#additional-considerations).

## <a name="bidirectional-or-mutual-authentication"></a>Çift yönlü veya karşılıklı kimlik doğrulaması
Çift yönlü kimlik doğrulama, hedef Başlatıcı kimliğini doğrular ve hedef Başlatıcı kimliğini doğrular. Bu cihaz ve iSCSI Initiator yazılımı konaktaki CHAP Başlatıcı Ayarları yanı sıra ters CHAP ayarlarını yapılandırmak kullanıcının gerektirir. Aşağıdaki yordamlar, Windows ana bilgisayar ve aygıt üzerinde karşılıklı kimlik doğrulaması yapılandırmak için gereken adımları açıklar.

#### <a name="to-configure-your-device-for-mutual-authentication"></a>Karşılıklı kimlik doğrulama için Cihazınızı yapılandırmak için
1. Azure Klasik portalında üzerinde **aygıtları** sayfasında, **yapılandırma** sekmesi.
   
    ![CHAP hedefi](./media/storsimple-configure-chap/IC740948.png)
2. Bu sayfada ve buna aşağı kaydırın **CHAP hedefi** bölümü:
   
   1. Sağlayan bir **ters CHAP kullanıcı adı** cihazınız için.
   2. Tedarik bir **ters CHAP parolası** cihazınız için.
   3. Parolayı onaylayın.
3. İçinde **CHAP Başlatıcı** bölümü:
   
   1. Sağlayan bir **kullanıcı adı** cihazınız için.
   2. Sağlayan bir **parola** cihazınız için.
   3. Parolayı onaylayın.
4. **Kaydet** düğmesine tıklayın. Bir onay iletisi görüntülenir. Değişiklikleri kaydetmek için **Tamam**’a tıklayın.

#### <a name="to-configure-bidirectional-authentication-on-the-windows-host-server"></a>Windows ana bilgisayar sunucusunda çift yönlü kimlik doğrulamasını yapılandırmak için
1. Windows ana bilgisayarı sunucusunda, iSCSI başlatıcısı başlatın.
2. İçinde **iSCSI başlatıcısı özellikleri** penceresinde tıklatın **yapılandırma** sekmesi.
3. Tıklatın **CHAP**.
4. İçinde **iSCSI Başlatıcı Karşılıklı CHAP parolası** iletişim kutusunda:
   
   1. Tür **ters CHAP parolası** Azure Klasik Portalı'nda yapılandırılmış.
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

İçinde **hedef Bağlan** sunulur, iletişim kutusu seç **Bu bağlantı sık kullanılan hedefler listesine eklemek** onay kutusu. Bu bilgisayar yeniden başlatıldığında her girişiminde bağlantı iSCSI sık kullanılan hedefler geri yüklemek için yapılan sağlar.

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
* Daha fazla bilgi edinmek [StorSimple güvenlik](storsimple-security.md).
* Daha fazla bilgi edinmek [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).

