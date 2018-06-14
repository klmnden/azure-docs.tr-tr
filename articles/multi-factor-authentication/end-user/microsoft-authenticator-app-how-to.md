---
title: Cep telefonları için Microsoft Authenticator uygulaması | Microsoft Docs
description: Azure Authenticator en son sürümüne yükseltmeyi öğrenin.
services: multi-factor-authentication
documentationcenter: ''
author: barlanmsft
manager: mtillman
ms.assetid: 3065a1ee-f253-41f0-a68d-2bd84af5ffba
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: barlan
ms.reviewer: librown
ms.custom: H1Hack27Feb2017, end-user
ms.openlocfilehash: 1532054a9463d710685d3f865d2e26ee7ff5014f
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
ms.locfileid: "28201006"
---
# <a name="get-started-with-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulaması ile çalışmaya başlama
Microsoft Authenticator uygulamasını bir ek düzeyi iş veya Okul hesabınızda güvenlik sunar (örneğin, bsimon@contoso.com) veya Microsoft hesabınızı (örneğin, bsimon@outlook.com).

Uygulama iki yoldan biriyle çalışır:

* **Bildirim**. Uygulaması hesaplara yetkisiz erişimi engellemek ve sahte işlemleri akıllı telefon veya tablet bir bildirim göndererek durdurma yardımcı olabilir. Sadece bildirimi görüntülemeniz ve işlem meşru ise seçin **doğrula**. Aksi takdirde, seçebileceğiniz **reddetme**.
* **Doğrulama kodu**. Uygulama bir OAuth doğrulama kodu oluşturmak için yazılım belirteci olarak kullanılabilir. Kullanıcı adınızı ve parolanızı girdikten sonra oturum açma ekranına uygulama tarafından sağlanan kodu girin. Doğrulama kodunu ikinci bir form kimlik doğrulaması sağlar.

Microsoft Authenticator uygulamasını Azure Authenticator uygulamasını yerini alır. Azure Authenticator uygulamasını hala çalışıyor, ancak yeni Microsoft Authenticator uygulaması taşımaya karar verirseniz, bu makale size yardımcı olabilir.  

## <a name="opt-in-for-two-step-verification"></a>İki aşamalı doğrulama için kabul

Microsoft Authenticator uygulamasını kendi başına çalışmaz. Her hesaplarınızı, kullanıcı adı ve parolayla oturum sonra ikinci bir doğrulama yöntemi için isteyecek şekilde yapılandırın.

Bir iş veya Okul hesabı için bu özellik kendiniz seçmek genellikle elde etmezsiniz. Bunun yerine, bir güvenlik yöneticisi sizin adınıza kabul eder ve doğrulama yöntemlerini hesabınıza kaydetmenizi bildirir. Bu senaryo, sizin için geçerliyse, daha fazla bilgi edinin [ne Azure çok faktörlü kimlik doğrulaması için beni anlamına gelmez](multi-factor-authentication-end-user.md).

Kişisel bir hesap için kendiniz için iki aşamalı doğrulamayı ayarlamanız gerekir. Bir Microsoft hesabınız varsa, bu adımları kullanılabilir olan [iki basamaklı doğrulama hakkında](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

Microsoft olmayan hesapların ile Microsoft Authenticator de kullanabilirsiniz. Bunlar özelliği iki aşamalı doğrulama dışında bir şey çağırabilir ancak güvenlik veya oturum açma ayarları altında bulamıyor olmalıdır.

## <a name="install-the-app"></a>Uygulama yükleme
Microsoft Authenticator uygulaması [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594), ve [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071).

## <a name="add-accounts-to-the-app"></a>Hesapları için uygulama ekleme
Microsoft Authenticator uygulaması eklemek istediğiniz her hesap için aşağıdaki yordamlardan birini kullanın:

### <a name="add-a-personal-microsoft-account-to-the-app"></a>Uygulamayı kişisel bir Microsoft hesabı ekleme

Kişisel Microsoft hesabı (birini Outlook.com, Xbox, Skype, vb. için oturum açmak için kullandığınız), yapmanız gereken tek şey Microsoft Authenticator uygulamasını hesabınızda oturum açın.

### <a name="add-a-work-or-school-account-to-the-app-using-the-qr-code-scanner"></a>QR kodu tarayıcı kullanarak uygulama bir iş veya Okul hesabı Ekle
1. Güvenlik doğrulama ayarları ekranına gidin.  Bu ekran alma hakkında daha fazla bilgi için bkz: [güvenlik ayarlarınızı değiştirme](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page).
2. Yanındaki kutuyu işaretleyin **Doğrulayıcı uygulama** seçip **yapılandırma**.

    ![Güvenlik doğrulama ayarları ekranında Yapılandır düğmesi](./media/authenticator-app-how-to/azureauthe.png)

    Bu bir QR kodu içeren bir ekrana getirir.

    ![QR kodu sağlar ekranı](./media/authenticator-app-how-to/barcode2.png)
3. Microsoft Kimlik Doğrulayıcı uygulamasını açın. Üzerinde **hesapları** ekran, select  **+** ve ardından iş veya Okul hesabı eklemek istediğinizi belirtin.
4. QR kodunu tarayın ve ardından için kameranızı kullanın **Bitti** QR kodu ekranı kapatmak için.

    Kameranızı düzgün çalışmıyorsa, yapabilecekleriniz [QR kodu ve URL'yi elle girin](#add-an-account-to-the-app-manually).

5. Uygulama hesap adınızı ile bunun altındaki altı rakamlı bir kod gösterdiğinde bitirdiniz.

    ![Hesapları ekranı](./media/authenticator-app-how-to/accounts.png)

### <a name="add-an-account-to-the-app-manually"></a>Hesap uygulamaya el ile Ekle
1. Güvenlik doğrulama ayarları ekranına gidin.  Bu ekran alma hakkında daha fazla bilgi için bkz: [güvenlik ayarlarınızı değiştirme](multi-factor-authentication-end-user-manage-settings.md).
2. Seçin **yapılandırma**.

    ![Güvenlik doğrulama ayarları ekranında Yapılandır düğmesi](./media/authenticator-app-how-to/azureauthe.png)

    Bu bir QR kodu içeren bir ekrana getirir.  Kodu ve URL'yi unutmayın.

    ![QR kodu ve URL sağlayan ekran](./media/authenticator-app-how-to/barcode2.png)
3. Microsoft Kimlik Doğrulayıcı uygulamasını açın. Üzerinde **hesapları** ekran, select  **+** ve ardından iş veya Okul hesabı eklemek istediğinizi belirtin.

4. Tarayıcıda seçin **kodu el ile girmeniz**.

    ![QR kodu taraması için ekran](./media/multi-factor-authentication-end-user-first-time/scan2.png)
5. Uygun kutulara uygulama kodu ve URL'yi girin ve ardından **son**.

    ![Kod ve URL girme ekran](./media/authenticator-app-how-to/manual.png)

6. Uygulama hesap adınızı ile bunun altındaki altı rakamlı bir kod gösterdiğinde bitirdiniz.

    ![Hesapları ekranı](./media/authenticator-app-how-to/accounts.png)

### <a name="add-an-account-to-the-app-using-your-devices-fingerprint-or-facial-recognition-capabilities"></a>Uygulamasına aygıtınızın parmak izi veya yüz tanıma özellikleri kullanarak Hesap Ekle
Kuruluşunuz doğrulama testini tamamlamanız için bir PIN gerektirebilir. Microsoft Authenticator uygulamasını bir PIN yerine aygıtınızın parmak izi veya yüz tanıma özellikleri kullanabilirsiniz. Bunu, ilk doğrulamayı uygulama ayarlamak için (iOS için) Touch ID kullanın veya kimliği yerine parmak izi için bir seçenek görürsünüz. 

Microsoft Authenticator için Touch ID ayarlamak için bir PIN ile normal doğrulama testini tamamlamanız gerekir. Microsoft Authenticator otomatik olarak onu Touch ID destekleyen aygıtlar için ayarlar 

![Touch ID kurulumunun doğrulama](./media/authenticator-app-how-to/touchid1.png)

Gruptan İleri noktası, oturum açma işleminiz doğrulamak için gerekli olduğunda alınan anında iletme bildirimi seçin ve PIN'İNİZİ girmek yerine parmak izinizi tarama.

![Anında iletme bildirimi](./media/authenticator-app-how-to/touchid2.png)

## <a name="use-the-app-when-you-sign-in"></a>Uygulamanın, oturum açtığınızda kullanın

Hesabınızı uygulamaya eklendikten sonra her şeyin doğru şekilde yapılandırıldığından emin olmak için test doğrulama yapmak için istenebilir. Bundan sonra tamamladınız! Oturum zamana kadar başka bir şey yapmanız gerekmez.

Uygulamada doğrulama kodları kullanmayı seçerseniz, giriş sayfasında görebileceği başlatın. Bir gereksinim duyduğunuzda, böylece her zaman yeni bir kod sahip oldukları her 30 saniyede değiştirin. Ancak oturum açın ve bir doğrulama kodu girmeniz istenir kadar onlarla herhangi bir şey yapmanız gerekmez.  
