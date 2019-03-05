---
title: Azure IOT Central Bağlayıcısı Microsoft Flow ile iş akışları oluşturun | Microsoft Docs
description: IOT Central Bağlayıcısı'nı Microsoft Flow için iş akışlarının kullanın ve oluşturma, güncelleştirme ve cihazları iş akışlarında silin.
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 02/20/2019
ms.topic: conceptual
ms.service: iot-central
manager: peterpr
ms.openlocfilehash: c2c172dc5f03aee8b167cd65562de3329c54d563
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2019
ms.locfileid: "57313020"
---
# <a name="build-workflows-with-the-iot-central-connector-in-microsoft-flow"></a>IOT Central Bağlayıcısı Microsoft Flow ile iş akışları oluşturun

*Bu konu, Oluşturucular ve Yöneticiler için geçerlidir.*

Birçok uygulama ve işletme kullanıcılarının kullandığı hizmetler arasında iş akışlarını otomatikleştirmek için Microsoft Flow kullanın. Microsoft Flow IOT Central Bağlayıcısı'nı kullanarak IOT Central içinde bir kuralı tetiklendiğinde iş akışları tetikleyebilirsiniz. IOT Central veya başka bir uygulama tarafından tetiklenen bir iş akışında bir cihaz oluşturma, bir cihazın özellikleri ve ayarları güncelleştirin veya bir cihazı silmek için Eylemler IOT Central Bağlayıcısı kullanabilirsiniz. Kullanıma [bu Microsoft Flow şablonları](https://aka.ms/iotcentralflowtemplates) mobil bildirim ve Microsoft Teams gibi diğer hizmetlere IOT Central bağlanma.

## <a name="prerequisites"></a>Önkoşullar

- Bir Kullandıkça Öde uygulama
- Microsoft, kişisel veya iş veya Okul hesabı Flow'da oturum açmayı ([Microsoft Flow planları hakkında daha fazla bilgi](https://aka.ms/microsoftflowplans))

## <a name="trigger-a-workflow"></a>Bir iş akışı tetikleyicisi

Bu bölümde, IOT Central bir kural tetiklendiğinde, Flow mobil uygulamasında mobil bildirim tetikleyip gösterilir.

1. Başlayın [IOT Central içinde bir kural oluşturma](howto-create-telemetry-rules-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json). Kural koşulları kaydettikten sonra seçin **Microsoft Flow eylem** yeni bir eylem olarak. Microsoft Flow alma, tarayıcınızda yeni bir sekme veya penceresi açmanız gerekir.

    ![Yeni bir Microsoft Flow eylem oluşturun](media/howto-add-microsoft-flow-experimental/createflowaction.png)

1. Microsoft Flow ' nda oturum açın. Bu IOT Central kullandığınız bir hesapla aynı olması gerekmez. Bir IOT Central Bağlayıcısı özel bir eylem bağlama gösteren bir genel bakış sayfasında açmayacaksınız.

1. IOT Central Connector'a açıp seçmek **devam**. Microsoft Flow tasarımcısına, iş akışınızı oluşturmak için alınır. İş akışı, uygulamanızı ve kural zaten doldurulmuş olan bir IOT Central tetikleyicisine sahiptir.

1. Seçin **+ yeni adım** ve **Eylem Ekle**. Bu noktada, akışınız için istediğiniz herhangi bir eylem ekleyebilirsiniz. Örneğin, şimdi mobil bildirim gönder. Arama **bildirim**ve **bildirimler - bana Mobil Bildirim Gönder**.

1. Uygulamada, bildirim söylemek istediğiniz metin alanını doldurun. Ekleyebileceğiniz *dinamik içerik* , IOT Central kuraldan boyunca cihaz adı ve zaman damgası gibi önemli bilgiler için bildirim geçirme.

    > [!NOTE]
    > Seçin **daha fazla bilgi bkz** kuralını tetikleyen ölçüm ve özellik değerlerini almak için dinamik içerik penceresindeki metin.

    ![Akış eylemi dinamik bölmesi açık düzenleme](./media/howto-add-microsoft-flow-experimental/flowdynamicpane.png)

1. İşiniz bittiğinde, eyleminiz düzenleme seçin **Kaydet**. İş akışının genel bakış sayfasına yönlendirilirsiniz. Çalıştırma geçmişini burada görebilirsiniz ve diğer iş arkadaşlarınızla paylaşabilirsiniz.

    > [!NOTE]
    > Diğer kullanıcıların bu kuralı düzenlemek için IOT Central uygulamanızda istiyorsanız, onlarla Microsoft Flow paylaşmanız gerekir. Microsoft Flow hesaplarında akışınızın sahipler ekleyin.

1. IOT Central uygulamanıza geri dönün, bu kural Eylemler alanının altında bir Microsoft Flow eylemi içeriyor görürsünüz.

Her zaman, IOT Central Bağlayıcısı'nı kullanarak Microsoft Flow iş akışı oluşturmaya başlayabilirsiniz. Sonra hangi IOT Central ve bağlanmak için hangi kural de seçebilirsiniz.

## <a name="create-a-device-in-a-workflow"></a>Bir iş akışında bir cihaz oluşturma

Bu bölümde, IOT Central içinde bir düğme anında iletme Microsoft Flow mobil uygulamasını kullanarak mobil bir cihazda yeni bir cihaz oluşturmak nasıl gösterir. Bu eylem, başka bir cihaz eklendiğinde, yeni bir cihaz oluşturarak IOT Central ile gibi Dynamics ERP sistemleriyle tümleştirmek için Flow'da kullanabilirsiniz.

1. Microsoft Flow boş bir iş akışı oluşturarak başlayın.

1. Arama **mobil için akış düğmesi** bir tetikleyici olarak.

1. Bu tetikleyiciyi adlı bir metin girişi ekleme **cihaz adı**. Açıklama metni değiştirme **yeni Cihazınızı cihaz adını**.

1. Yeni bir eylem ekleyin. Arama **Azure IOT Central - bir cihaz oluşturma** eylem.

1. Uygulamanızı seçin ve bir CİHAZDAN açılan menülerde oluşturmak üzere cihaz şablonu seçin. Tüm cihaz ayarlarını ve özelliklerini göstermek için genişletin eylem görürsünüz.

1. Cihaz ad alanını seçin. Dinamik içerik bölmesinden seçin **cihaz adı**. Bu değer, kullanıcı mobil uygulama üzerinden girer ve IOT Central'nde yeni Cihazınızı adıdır girdisinden geçirilir. Bu örnekte, tek gerekli alan kırmızı yıldız işaretiyle belirtilen cihaz adıdır. Başka bir cihaz şablonu yeni bir cihaz oluşturmak için doldurulması gereken birden çok gerekli alanları olabilir.

    ![Dinamik eylem bölmesinde, cihaz akış oluşturma](./media/howto-add-microsoft-flow-experimental/flowcreatedevice.png)

1. (İsteğe bağlı) Oluşturma yeni cihazlarınız için dilediğiniz şekilde diğer alanları doldurun.

1. Son olarak, iş akışınızı kaydedin.

1. İş akışınızı Microsoft Flow mobil uygulamasını deneyin. Git **düğmeleri** uygulama sekmesinde. -> Yeni bir cihaz iş akışı oluşturma düğmenizin görmeniz gerekir. Yeni Cihazınızı adını girin ve IOT Central ' gösterilmesi izleyin!

    ![Flow mobil cihaz ekran oluşturma](./media/howto-add-microsoft-flow-experimental/flowmobilescreenshot.png)

## <a name="update-a-device-in-a-workflow"></a>Bir cihaz bir iş akışında güncelleştir

Bu bölümde, cihaz ayarlarını ve özelliklerini IOT Central içinde bir düğme anında iletme Microsoft Flow mobil uygulamasını kullanarak mobil bir cihazda güncelleştirme işlemini göstermektedir.

1. Microsoft Flow boş bir iş akışı oluşturarak başlayın.

1. Arama **mobil için akış düğmesi** bir tetikleyici olarak.

1. Bu tetikleyici, giriş gibi ekleme bir **konumu** karşılık gelen metin girişi değiştirmek istediğiniz bir ayar veya özelliği. Açıklama metnini değiştirin.

1. Yeni bir eylem ekleyin. Arama **Azure IOT Central - bir cihaz güncelleştirmesi** eylem.

1. Açılır listeden uygulamanızı seçin. Şimdi, güncelleştirmek istediğiniz var olan cihazın kimliği gerekir. IOT Central CİHAZDAN Kimliğini alabilirsiniz **Device Explorer**.

    ![IOT Central cihaz Gezgini cihaz kimliği](./media/howto-add-microsoft-flow-experimental/iotcdeviceid.png)

1. Cihaz adını güncelleştirebilirsiniz. Cihazın özellikleri ve ayarları güncelleştirmek için cihaz şablonu güncelleştirmek istediğiniz cihazı seçin **cihaz şablonu** açılır. Tüm özellikleri ve ayarları güncelleştirebilirsiniz göstermek için eylem kutucuk genişletir.

    ![Akış güncelleştirme cihaz iş akışı](./media/howto-add-microsoft-flow-experimental/flowupdatedevice.png)

1. Her özellik ve güncelleştirmek istediğiniz ayarları seçin. Dinamik içerik bölmesinden tetikleyiciden karşılık gelen bir giriş seçin. Bu örnekte konum değeri aşağı cihazın konum özelliği güncelleştirilecek yayılır.

1. Son olarak, iş akışınızı kaydedin.

1. İş akışınızı Microsoft Flow mobil uygulamasını deneyin. Git **düğmeleri** uygulama sekmesinde. Bir cihaz iş akışını güncelleştirme -> düğmenizin görmeniz gerekir. Girişleri girin ve IOT Central ' güncelleştirilmesi cihaz bakın!

## <a name="delete-a-device-in-a-workflow"></a>Bir iş akışında bir cihazı silme

Bir cihaz, cihaz kimliği kullanarak silebilirsiniz **Azure IOT Central - bir cihazı silme** eylem. Microsoft Flow mobil uygulamasındaki bir düğmeye bir cihazda silen bir örnek iş akışı şu şekildedir.

   ![Akışı Sil cihaz iş akışı](./media/howto-add-microsoft-flow-experimental/flowdeletedevice.png)

## <a name="troubleshooting"></a>Sorun giderme

Azure IOT Central Bağlayıcısı bağlantı oluşturma ile ilgili sorunlar yaşıyorsanız, size yardımcı olabilecek bazı ipuçları şunlardır.

1. Kişisel Microsoft hesapları (gibi @hotmail.com, @live.com, @outlook.com etki alanları) şu anda desteklenmiyor. Bir Azure Active Directory (AD) iş veya Okul hesabı gerekir.

2. Microsoft Flow IOT Central Bağlayıcısı'nı kullanmak için en az bir kez IOT Central uygulamasına oturum gerekir. Aksi takdirde uygulama uygulama açılan menülerde gösterilmez.

3. Bir Azure AD hesabı kullanırken bir hata alıyorsanız, Windows PowerShell açmayı deneyin ve bir yönetici olarak aşağıdaki komutları çalıştırın.

    ``` PowerShell
    Install-Module AzureAD
    Connect-AzureAD
    New-AzureADServicePrincipal -AppId 9edfcdd9-0bc5-4bd4-b287-c3afc716aac7 -DisplayName "Azure IoT Central"
    ```

## <a name="next-steps"></a>Sonraki adımlar

İş akışlarını oluşturmak için Microsoft Flow kullanmayı öğrendiniz, önerilen sonraki adım olarak [cihazları yönetme](howto-manage-devices-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).

