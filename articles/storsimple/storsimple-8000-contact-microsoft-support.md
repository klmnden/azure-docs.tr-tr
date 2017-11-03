---
title: "Destek bileti veya StorSimple 8000 serisi için servis talebi oluşturun | Microsoft Docs"
description: "Destek isteği oturum ve StorSimple 8000 serisi aygıtınızda destek oturum başlatma hakkında bilgi edinin."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: alkohli;
ms.openlocfilehash: 4b5a14237ce79100f980b2186b2c3c887abaa296
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="contact-microsoft-support"></a>Microsoft Destek'e Başvur

StorSimple Aygıt Yöneticisi'ni yeteneği sağlar **yeni bir destek isteği oturum** hizmet Özet dikey. StorSimple çözümünüzle birlikte herhangi bir sorunla karşılaşırsanız, teknik destek için bir hizmet isteği oluşturabilirsiniz. Destek mühendisinize ile çevrimiçi oturumda StorSimple Cihazınızda bir destek oturumu başlatmak gerekebilir. Bu makalede, izlenecek yol:

* Bir destek isteği oluşturma
* Bir destek isteği döngüsü portalındaki yönetmek nasıl.
* StorSimple Cihazınızı Windows PowerShell arabiriminde bir destek oturumu başlatmak nasıl.

Gözden geçirme [StorSimple 8000 serisi destek SLA'ları ve bilgileri](https://msdn.microsoft.com/library/mt433077.aspx) önce bir destek isteği oluşturun.

## <a name="create-a-support-request"></a>Destek isteği oluşturun

Bağlı olarak, [destek planı](https://azure.microsoft.com/support/plans/), doğrudan StorSimple cihaz Yöneticisi hizmeti Özet dikey penceresinden, StorSimple Cihazınızda bir sorun destek bileti oluşturabilirsiniz. Bir destek isteği oluşturmak için aşağıdaki adımları gerçekleştirin:

#### <a name="to-create-a-support-request"></a>Bir destek isteği oluşturmak için

1. StorSimple Cihaz Yöneticisi hizmetinize gidin. Hizmet özeti dikey ayarları Git **destek + sorun giderme** bölümünde ve ardından **yeni destek isteği**.
     
    ![Yeni portal aracılığıyla kişinin MS desteği](./media/storsimple-8000-contact-microsoft-support/contactsupport1.png)
   
2. İçinde **yeni destek isteği** dikey penceresinde, select **Temelleri**. İçinde **Temelleri** dikey penceresinde, aşağıdaki adımları uygulayın:
   1. Gelen **sorun türü** aşağı açılan listesinden, **teknik**.
   2. Geçerli **abonelik**, **hizmet** türü ve **kaynak** (StorSimple cihaz Yöneticisi hizmeti) otomatik olarak seçilir. 
   3. Seçin bir **destek planı** aboneliğinizle ilişkili birden çok planları varsa aşağı açılan listeden. Teknik Destek etkinleştirmek için ücretli bir destek planı gerekiyor.
   4. **İleri**’ye tıklayın.

       ![Yeni portal aracılığıyla kişinin MS desteği](./media/storsimple-8000-contact-microsoft-support/contactsupport2.png)

3. İçinde **yeni destek isteği** dikey penceresinde, select **adım 2 sorun**. İçinde **sorun** dikey penceresinde, aşağıdaki adımları uygulayın:
    
    1. Seçin **önem**.
    2. Sorunu gereç ya da StorSimple cihaz Yöneticisi hizmeti ilişkili olup olmadığını belirtin.
    3. Seçin bir **kategori** bu vermek ve daha fazlasını sağlamak **ayrıntıları** sorun hakkında.
    4. Başlangıç tarihi ve saati sorun için sağlar.
    5. İçinde **karşıya dosya yükleme**, destek paketinizi göz atmak için klasör simgesine tıklayın.
    6. Denetleme **tanılama bilgileri paylaşabilir**.
    7. **İleri**’ye tıklayın.

       ![Yeni portal aracılığıyla kişinin MS desteği](./media/storsimple-8000-contact-microsoft-support/contactsupport3.png) 

4. İçinde **yeni destek isteği** dikey penceresinde tıklatın **adım 3 irtibat bilgileri**. İçinde **kişi bilgileri** dikey penceresinde, aşağıdaki adımları uygulayın:

    1. İçinde **başvurun seçenekleri**, tercih edilen iletişim yönteminiz (telefon veya e-posta) ve dili girin. Yanıt süresi, abonelik plana göre otomatik olarak seçilir.
    2. Kişi bilgileri adı, e-posta, isteğe bağlı kişi, ülke sağlamak. Seçin **Kaydet kişi değişikliklerini gelecekteki destek istekleri** onay kutusu.
    3. **Oluştur**'a tıklayın.
   
        ![Yeni portal aracılığıyla kişinin MS desteği](./media/storsimple-8000-contact-microsoft-support/contactsupport5.png)   

    Microsoft Support size daha fazla bilgi, tanılama ve çözümleme için ulaşmak için bu bilgileri kullanır.
İsteğinizi gönderdikten sonra destek mühendisi, mümkün olan en kısa sürede isteğinize devam etmek için sizinle iletişim kuracaktır.

## <a name="manage-a-support-request"></a>Bir destek isteği yönetme

Bir destek bileti oluşturduktan sonra portal üzerinden bu biletin yaşam döngüsünü yönetebilirsiniz.

#### <a name="to-manage-your-support-requests"></a>Destek isteklerinizi yönetmek için

1. Yardım ve Destek sayfasına ulaşmak için gidin **Gözat > Yardım + Destek**.

    ![Destek isteklerini yönet](./media/storsimple-8000-contact-microsoft-support/managesupport1.png)

2. Tüm destek istekleri tablolu bir listesi görüntülenir **Yardım + Destek** dikey.

    ![Destek isteklerini yönet](./media/storsimple-8000-contact-microsoft-support/managesupport2.png)

3. Seçin ve bir destek isteği'ni tıklatın. Durum ve bu isteği ayrıntılarını görüntüleyebilirsiniz. Tıklatın **+ yeni ileti** bu istekte takip etmek istiyorsanız.

    ![Destek isteklerini yönet](./media/storsimple-8000-contact-microsoft-support/managesupport3.png)

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell içinde bir destek oturumu Başlat

StorSimple cihazı karşılaşabileceğiniz sorunları gidermek için Microsoft Support ekibinin sağlayacağı gerekecektir. Microsoft Support aygıtınıza oturum açmak için bir destek oturumu kullanmanız gerekebilir.

Bir destek oturumu başlatmak için aşağıdaki adımları gerçekleştirin:

#### <a name="to-start-a-support-session"></a>Bir destek oturumu başlatmak için

1. Cihaz seri Konsolu kullanarak doğrudan veya uzak bir bilgisayardan telnet oturumu aracılığıyla erişin. Bunu yapmak için adımları izleyin [kullan cihaz seri konsoluna bağlanmak için PuTTY](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. Açılan oturumda basın **Enter** bir komut istemi almak için anahtar.
3. Seri konsol menüsünde seçeneğini 1, **oturum oturum tam erişim**.
4. İsteminde aşağıdaki parolasını yazın:
   
    `Password1`
5. İsteminde aşağıdaki komutu yazın:
   
    `Enable-HcsSupportAccess`
6. Şifreli bir dize size sunulur. Bu dize, Not Defteri gibi bir metin düzenleyicisine kopyalayın.
7. Bu dizeyi kaydedin ve Microsoft Support bir e-posta iletisi gönderin.

> [!IMPORTANT]
> Çalıştırarak destek erişim devre dışı bırakabilirsiniz `Disable-HcsSupportAccess`. StorSimple cihazı da 8 saat oturumu başlatıldı sonra destek erişimini devre dışı bırakma dener. Bir destek oturum başlatıldıktan sonra StorSimple cihaz kimlik bilgilerinizi değiştirmek için en iyi bir uygulamadır.


## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [tanılamak ve StorSimple 8000 serisi Cihazınızı ilgili sorunları](storsimple-troubleshoot-deployment.md)
