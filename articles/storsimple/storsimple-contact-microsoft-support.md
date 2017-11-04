---
title: "StorSimple 8000 serisi için destek bileti oturum | Microsoft Docs"
description: "Bir destek isteği oluşturun ve StorSimple Cihazınızda destek oturum başlatma hakkında bilgi edinin."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 2ebc20fe-f490-4749-8e43-c9fac86f1676
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: alkohli;anbacker
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8d25fb4902d37faca5358e5a9010e9e146e344e1
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="contact-microsoft-support-for-your-storsimple"></a>StorSimple için Microsoft Destek'e başvurun
> [!NOTE]
> StorSimple için Klasik portalı kullanım dışıdır. StorSimple cihaz yöneticileri yeni Azure portalına kullanımdan zamanlamaya göre otomatik olarak taşır. Bir e-posta ve bu taşıma için portal bir bildirim alırsınız. Bu belgede ayrıca yakında kullanımdan kaldırılacaktır. Bu makalede yeni Azure portalına için sürümünü görüntülemek için şu adrese gidin [, StorSimple için Microsoft Destek'e başvurun](storsimple-8000-contact-microsoft-support.md). Taşıma hakkında herhangi bir sorunuz için bkz: [SSS: Azure portalına taşıma](storsimple-8000-move-azure-portal-faq.md).

Microsoft Azure StorSimple çözümünüzle birlikte herhangi bir sorunla karşılaşırsanız, teknik destek için bir hizmet isteği oluşturabilirsiniz. Destek mühendisinize ile çevrimiçi oturumda StorSimple Cihazınızda bir destek oturumu başlatmak gerekebilir. Bu makalede, izlenecek yol:

* Bir destek isteği oluşturma
* StorSimple Cihazınızı Windows PowerShell arabiriminde bir destek oturumu başlatmak nasıl.

Gözden geçirme [StorSimple 8000 serisi destek SLA'ları ve bilgileri](https://msdn.microsoft.com/library/mt433077.aspx) önce bir destek isteği oluşturun.

## <a name="create-a-support-request"></a>Destek isteği oluşturun
Bir destek isteği oluşturmak için aşağıdaki adımları gerçekleştirin:

#### <a name="to-create-a-support-request"></a>Bir destek isteği oluşturmak için
1. İçinde [Klasik Azure portalı](https://manage.windowsazure.com/), sağ üst köşedeki hesap adınızı tıklatın ve ardından **Microsoft Destek birimine başvurun**.
   
    ![ManagementPortal üzerinden kişi MS desteği](./media/storsimple-contact-microsoft-support/Ibiza1.png)
2. Yeni Azure portalına (portal.azure.com) yönlendirilir. Tıklatın **yeni destek isteği** döşeme.
   
    ![Yeni portal aracılığıyla kişinin MS desteği](./media/storsimple-contact-microsoft-support/Ibiza2.png)
   
    Ekranın sağ taraftaki **yeni destek isteği** bölmesinde görünür. 
   
    ![Yeni destek isteği bölmesi](./media/storsimple-contact-microsoft-support/Ibiza3a.png)
3. İçinde **Temelleri** iletişim kutusunda, aşağıdaki adımları tamamlayın:                                
   
   1. Gelen **sorun türü** aşağı açılan listesinden, **teknik**.
   2. Seçin bir **abonelik** aşağı açılan listeden.
   3. Gelen **hizmet** aşağı açılan listesinden, **StorSimple**. 
   4. Seçin bir **destek planı** aşağı açılan listeden. Teknik Destek etkinleştirmek için ücretli bir destek planı gerekiyor.
4. **İleri**’ye tıklayın. **Sorun** iletişim kutusu görüntülenir.
   
    ![Yeni destek isteği bölmesi](./media/storsimple-contact-microsoft-support/Ibiza5a.png) 
5. İçinde **sorun** iletişim kutusunda, aşağıdaki adımları tamamlayın:
   
   1. Seçin bir **önem** aşağı açılan listeden düzeyi.
   2. Seçin bir **sorun türü** aşağı açılan listeden.
   3. Seçin bir **kategori** aşağı açılan listeden. 
   4. İçinde **ayrıntıları** kutusunda, sorununuzu kısaca anlatın.
   5. İçinde **zaman çerçevesi** kutusunda, tarih, saat ve sorunu en son görüldüğü için karşılık gelen saat dilimini belirtin.
   6. Altında **karşıya dosya yükleme**, destek paketinizi göz atmak için klasör simgesine tıklayın.
   7. Seçin **tanılama bilgileri paylaşabilir** onay kutusu.
6. **İleri**’ye tıklayın. **Kişi bilgileri** iletişim kutusu görüntülenir.
   
    ![Yeni destek isteği bölmesi](./media/storsimple-contact-microsoft-support/Ibiza6a.png) 
7. İletişim bilgilerinizi girin ve bir kişi bir yöntem (telefon veya e-posta) seçin. 
8. Seçin **Kaydet kişi değişikliklerini gelecekteki destek istekleri** onay kutusu.
9. **Oluştur**'a tıklayın.

İsteğinizi gönderdikten sonra destek mühendisi, mümkün olan en kısa sürede isteğinize devam etmek için sizinle iletişim kuracaktır.

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
> 
> 

