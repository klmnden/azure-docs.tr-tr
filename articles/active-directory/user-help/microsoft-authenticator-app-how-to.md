---
title: Cep telefonu - Azure AD için Microsoft Authenticator uygulaması | Microsoft Docs
description: Azure Authenticator'ın en son sürümüne yükseltmeyi öğrenin.
services: multi-factor-authentication
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.assetid: 3065a1ee-f253-41f0-a68d-2bd84af5ffba
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/17/2017
ms.author: lizross
ms.reviewer: librown
ms.custom: H1Hack27Feb2017, end-user
ms.openlocfilehash: 8241dcaaf5623a22f4fc485f021766276472fb51
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39059826"
---
# <a name="get-started-with-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulaması ile çalışmaya başlama
Microsoft Authenticator uygulaması ek bir iş veya Okul hesabınızda güvenlik düzeyi sağlar (örneğin, bsimon@contoso.com) veya Microsoft hesabınızı (örneğin, bsimon@outlook.com).

Uygulama, iki yoldan biriyle çalışır:

* **Bildirim**. Uygulama akıllı telefonundaki veya tabletindeki bildirim göndererek sahte işlemleri durdurun ve hesaplara yetkisiz erişimi önlemeye yardımcı olabilir. Yalnızca bildirimi görüntülemeniz ve işlem meşru ise seçin **doğrulama**. Aksi takdirde, seçebileceğiniz **Reddet**.
* **Doğrulama kodu**. Uygulamayı bir OAuth doğrulama kodu oluşturmak için yazılım belirteci olarak kullanılabilir. Kullanıcı kimliğiniz ve parolanızı girdikten sonra oturum açma ekranına uygulama tarafından sağlanan kodu girin. Doğrulama kodu, ikinci bir form kimlik doğrulaması sağlar.

Microsoft Authenticator uygulamasını Azure Authenticator uygulaması değiştirir. Azure Authenticator uygulaması hala çalışıyor, ancak yeni Microsoft Authenticator uygulamasına geçmeye karar verirseniz, bu makalede size yardımcı olabilir.  

## <a name="opt-in-for-two-step-verification"></a>İki aşamalı doğrulama için kabul et

Microsoft Authenticator uygulamasını kendi başına çalışmaz. Her hesaplarınız, kullanıcı adı ve parolanızla oturum sonra ikinci bir doğrulama yöntemi için isteyecek şekilde yapılandırın.

Bir iş veya Okul hesabı için bu özelliği kendiniz seçmek genellikle elde etmezsiniz. Bunun yerine, bir güvenlik yöneticisi, sizin adınıza kabul eder ve doğrulama yöntemlerini hesabınıza kaydetmenizi bildirir. Bu senaryo sizin için geçerliyse, daha fazla bilgi [Azure multi-Factor Authentication benim için ne demektir](multi-factor-authentication-end-user.md).

Kişisel bir hesap için iki aşamalı doğrulama için kendiniz ayarlamanız gerekir. Bir Microsoft hesabınız varsa, bu adımları kullanılabilir [iki basamaklı doğrulama hakkında](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

Microsoft Authenticator ile Microsoft olmayan hesapların de kullanabilirsiniz. Bunlar özellikle iki aşamalı doğrulamanın dışında bir şey çağırmaz, ancak güvenlik veya oturum açma ayarları altında bulabilirsiniz olmalıdır.

## <a name="install-the-app"></a>Uygulama yükleme
Microsoft Authenticator uygulaması [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594) ve [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071)'da kullanılabilir.

## <a name="add-accounts-to-the-app"></a>Uygulamaya hesapları ekleme
Microsoft Authenticator uygulamasına eklemek istediğiniz her hesap için aşağıdaki prosedürlerden birini kullanın:

### <a name="add-a-personal-microsoft-account-to-the-app"></a>Kişisel bir Microsoft hesabı uygulamasına ekleme

Kişisel Microsoft hesabı için (bir Outlook.com, Xbox, Skype, vb. için oturum açmak için kullandığınız), tüm yapmanız gereken olan Microsoft Authenticator uygulaması hesabınızda oturum açın.

### <a name="add-a-work-or-school-account-to-the-app-using-the-qr-code-scanner"></a>QR kodu tarayıcısını kullanarak uygulamaya iş veya Okul hesabı ekleme
1. Güvenlik doğrulama ayarları ekranına gidin.  Bu ekrana alma hakkında daha fazla bilgi için bkz: [güvenlik ayarlarınızı değiştirme](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page).
2. Yanındaki kutuyu işaretleyin **Authenticator uygulamasını** seçip **yapılandırma**.

    ![Güvenlik doğrulama ayarları ekranında Yapılandır düğmesi](./media/microsoft-authenticator-app-how-to/azureauthe.png)

    Bu bir QR kodu içeren bir ekran getirir.

    ![QR kodunu sağlayan ekranı](./media/microsoft-authenticator-app-how-to/barcode2.png)
3. Microsoft Authenticator uygulamasını açın. Üzerinde **hesapları** ekranındayken **+** ve ardından iş veya Okul hesabı eklemek istediğinizi belirtin.
4. QR kodunu tarayın ve ardından kameranızı kullanmasına **Bitti** QR kodu ekranı kapatmak için.

    Kameranız düzgün şekilde çalışmıyor varsa [QR kodu ve URL'yi el ile girmek](#add-an-account-to-the-app-manually).

5. Hesap adınızı altındaki altı rakamlı bir kod ile uygulamayı ortaya çıktığında hazırsınız.

    ![Hesaplar ekranı](./media/microsoft-authenticator-app-how-to/accounts.png)

### <a name="add-an-account-to-the-app-manually"></a>Hesap için uygulamayı el ile Ekle
1. Güvenlik doğrulama ayarları ekranına gidin.  Bu ekrana alma hakkında daha fazla bilgi için bkz: [güvenlik ayarlarınızı değiştirme](multi-factor-authentication-end-user-manage-settings.md).
2. Seçin **yapılandırma**.

    ![Güvenlik doğrulama ayarları ekranında Yapılandır düğmesi](./media/microsoft-authenticator-app-how-to/azureauthe.png)

    Bu bir QR kodu içeren bir ekran getirir.  Kodu ve URL'yi not edin.

    ![QR kodu ve URL sağlayan ekranı](./media/microsoft-authenticator-app-how-to/barcode2.png)
3. Microsoft Authenticator uygulamasını açın. Üzerinde **hesapları** ekranındayken **+** ve ardından iş veya Okul hesabı eklemek istediğinizi belirtin.

4. Tarayıcıda seçin **kodu el ile girmeniz**.

    ![Ekran için QR kodu tarama](./media/microsoft-authenticator-app-how-to/scan2.png)
5. Uygun kutulara uygulama kodu ve URL'yi girin ve ardından **son**.

    ![Kodu ve URL'yi girme ekran](./media/microsoft-authenticator-app-how-to/manual.png)

6. Hesap adınızı altındaki altı rakamlı bir kod ile uygulamayı ortaya çıktığında hazırsınız.

    ![Hesaplar ekranı](./media/microsoft-authenticator-app-how-to/accounts.png)

### <a name="add-an-account-to-the-app-using-your-devices-fingerprint-or-facial-recognition-capabilities"></a>Cihazınızın parmak izi veya yüz tanıma özelliklerini kullanarak uygulamasında Hesap Ekle
Kuruluşunuz doğrulama testini tamamlamanız için bir PIN isteyebilir. Microsoft Authenticator uygulamasını bir PIN yerine cihazınızın parmak izi veya yüz tanıma özelliklerini kullanabilirsiniz. Uygulamasında, ilk doğrulamayı bunu ayarlamak için Touch ID kullanın (iOS için) veya kimliği yerine parmak izi için bir seçenek görürsünüz. 

Touch ID ' için Microsoft Authenticator ayarlamak için bir PIN ile bir normal doğrulama testini tamamlamanız gerekir. Microsoft Authenticator otomatik olarak bu Touch ID destekleyen cihazlar için ayarlar 

![Touch ID Kurulum doğrulama](./media/microsoft-authenticator-app-how-to/touchid1.png)

Bundan İleri noktası, oturum açma işleminizi doğrulamak için gerekmesinden, alınan anında iletme bildirimi seçin ve PIN'İNİZİ girmek yerine parmak izinizi tarama.

![Anında iletme bildirimi](./media/microsoft-authenticator-app-how-to/touchid2.png)

## <a name="use-the-app-when-you-sign-in"></a>Oturum açtığınızda, uygulama kullanma

Hesabınız uygulamaya eklendikten sonra her şeyin doğru yapılandırıldığından emin olmak için bir test doğrulaması yapmanız istenebilir. Bundan sonra hazırsınız! Bir sonraki oturum açışınızda kadar başka bir şey yapmanız gerekmez.

Uygulama doğrulama kodları kullanmayı seçerseniz, bunları giriş sayfasında bakın başlatın. Böylece, ihtiyacınız olduğunda yeni bir kod her zaman sahip oldukları her 30 saniyede değiştirin. Ancak oturum açın ve bir doğrulama kodu girmeniz istenir kadar bunları ile herhangi bir şey yapmanız gerekmez.  
