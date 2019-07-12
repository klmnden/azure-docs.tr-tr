---
title: Azure IOT Central Bağlayıcısı Microsoft Flow ile iş akışları oluşturun | Microsoft Docs
description: IOT Central Bağlayıcısı'nı Microsoft Flow için tetikleyici iş akışları kullanın ve oluşturma, alma, güncelleştirme, cihazları silin ve akışlarında komutları çalıştırın.
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 04/25/2019
ms.topic: conceptual
ms.service: iot-central
manager: hegate
ms.openlocfilehash: e8bc8b8d4e3585ea4c0505f2e36abc6d1da7f8eb
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797715"
---
# <a name="build-workflows-with-the-iot-central-connector-in-microsoft-flow"></a>IOT Central Bağlayıcısı Microsoft Flow ile iş akışları oluşturun

*Bu konu, Oluşturucular ve Yöneticiler için geçerlidir.*

Birçok uygulama ve işletme kullanıcılarının kullandığı hizmetler arasında iş akışlarını otomatikleştirmek için Microsoft Flow kullanın. Microsoft Flow IOT Central Bağlayıcısı'nı kullanarak IOT Central içinde bir kuralı tetiklendiğinde iş akışları tetikleyebilirsiniz. IOT Central veya başka bir uygulama tarafından tetiklenen bir iş akışında eylemleri için IOT Central Bağlayıcısı kullanabilirsiniz:
- Cihaz oluşturma
- Cihaz bilgilerini alma
- Bir cihazın özelliklerini ve ayarlarını güncelleştirme
- Bir cihaza bir komut çalıştırın
- Bir cihazı silme

Kullanıma [bu Microsoft Flow şablonları](https://aka.ms/iotcentralflowtemplates) mobil bildirim ve Microsoft Teams gibi diğer hizmetlere IOT Central bağlanma.

## <a name="prerequisites"></a>Önkoşullar

- Bir Kullandıkça Öde uygulama
- Microsoft, kişisel veya iş veya Okul hesabı Microsoft Flow kullanmak için ([Microsoft Flow planları hakkında daha fazla bilgi](https://aka.ms/microsoftflowplans))
- Azure IOT Central bağlayıcıyı kullanmak üzere bir iş veya Okul hesabı

## <a name="trigger-a-workflow"></a>Bir iş akışı tetikleyicisi

Bu bölümde, IOT Central bir kural tetiklendiğinde, Flow mobil uygulamasında mobil bildirim tetikleyip gösterilir. Katıştırılmış Microsoft Flow Tasarımcısı'nı kullanarak IOT Central uygulamanızda bu tüm iş akışı oluşturabilirsiniz.

1. Başlayın [IOT Central içinde bir kural oluşturma](howto-create-telemetry-rules.md). Kural koşulları kaydettikten sonra seçin **Microsoft Flow eylem** yeni bir eylem olarak. Bir iletişim kutusu penceresinin, iş akışınızı yapılandırmak üzere açılır. IOT Central, oturum açmış kullanıcı hesabı, Microsoft Flow ile imzalamak için kullanılır.

    ![Yeni bir Microsoft Flow eylem oluşturun](media/howto-add-microsoft-flow/createflowaction.png)

1. Erişimi ve bu IOT Central kuralına bağlı iş akışlarının listesini görürsünüz. Tıklayın **şablonları keşfedin** veya **yeni > Şablondan Oluştur** ve kullanılabilir şablonlardan birini seçebilirsiniz. 

    ![Kullanılabilir Microsoft Flow şablonları](media/howto-add-microsoft-flow/flowtemplates1.png)

1. Seçtiğiniz şablon bağlayıcılarında oturum açmanız istenir. 

    > [!NOTE]
    > Azure IOT Central Bağlayıcısı'nı kullanmak için bir Azure Active Directory hesabı (iş veya Okul hesabı) kullanarak oturum açmanız gerekir. Gibi bir kişisel hesap abc@outlook.com veya abc@live.com Azure IOT Central Bağlayıcısı tarafından desteklenmez.

    Bağlayıcılar açtıktan sonra iş akışınızı oluşturmak için Tasarımcısı'nda ulaşırsınız. İş akışı, uygulamanızı ve kural zaten doldurulmuş olan bir IOT Central tetikleyicisine sahiptir.

1. Ekleme yeni eylemler ve eylem için geçirilen bilgileri özelleştirerek, iş akışını özelleştirebilirsiniz. Bu örnekte, eylem ise **bildirimler - bana Mobil Bildirim Gönder**. Ekleyebileceğiniz *dinamik içerik* , IOT Central kuraldan boyunca cihaz adı ve zaman damgası gibi önemli bilgiler için bildirim geçirme.

    > [!NOTE]
    > Seçin **daha fazla bilgi bkz** kuralını tetikleyen ölçüm ve özellik değerlerini almak için dinamik içerik penceresindeki metin.

    ![Akış eylemi dinamik bölmesi açık düzenleme](./media/howto-add-microsoft-flow/flowdynamicpane1.png)

1. İşiniz bittiğinde, eyleminiz düzenleme seçin **Kaydet**. İş akışının genel bakış sayfasına yönlendirilirsiniz. Çalıştırma geçmişini burada görebilirsiniz ve diğer iş arkadaşlarınızla paylaşabilirsiniz.

    > [!NOTE]
    > Diğer kullanıcıların bu kuralı düzenlemek için IOT Central uygulamanızda istiyorsanız, onlarla Microsoft Flow paylaşmanız gerekir. Microsoft Flow hesaplarında akışınızın sahipler ekleyin.

1. IOT Central uygulamanıza geri dönün, bu kural Eylemler alanında bir Microsoft Flow eylemi içeriyor görürsünüz.

Ayrıca, Microsoft Flow doğrudan IOT Central Bağlayıcısı'nı kullanarak iş akışları da oluşturabilirsiniz. Daha sonra bağlanmak için hangi IOT Central uygulaması seçebilirsiniz.

## <a name="create-a-device-in-a-workflow"></a>Bir iş akışında bir cihaz oluşturma

Bu bölümde, IOT Central içinde bir düğme anında iletme Microsoft Flow mobil uygulamasını kullanarak mobil bir cihazda yeni bir cihaz oluşturmak nasıl gösterir. Bu eylem, başka bir cihaz eklendiğinde, yeni bir cihaz oluşturarak IOT Central ile gibi Dynamics ERP sistemleriyle tümleştirmek için Flow'da kullanabilirsiniz.

1. Microsoft Flow boş bir iş akışı oluşturarak başlayın.

1. Arama **mobil için akış düğmesi** bir tetikleyici olarak.

1. Bu tetikleyiciyi adlı bir metin girişi ekleme **cihaz adı**. Açıklama metni değiştirme **yeni Cihazınızı cihaz adını**.

1. Yeni bir eylem ekleyin. Arama **Azure IOT Central - bir cihaz oluşturma** eylem.

1. Uygulamanızı seçin ve bir CİHAZDAN açılan menülerde oluşturmak üzere cihaz şablonu seçin. Tüm cihaz ayarlarını ve özelliklerini göstermek için genişletin eylem görürsünüz.

1. Cihaz ad alanını seçin. Dinamik içerik bölmesinden seçin **cihaz adı**. Bu değer, kullanıcı mobil uygulama üzerinden girer ve IOT Central'nde yeni Cihazınızı adıdır girdisinden geçirilir. Bu örnekte, tek gerekli alan kırmızı yıldız işaretiyle belirtilen cihaz adıdır. Başka bir cihaz şablonu yeni bir cihaz oluşturmak için doldurulması gereken birden çok gerekli alanları olabilir.

    ![Dinamik eylem bölmesinde, cihaz akış oluşturma](./media/howto-add-microsoft-flow/flowcreatedevice1.png)

1. (İsteğe bağlı) Oluşturma yeni cihazlarınız için dilediğiniz şekilde diğer alanları doldurun.

1. Son olarak, iş akışınızı kaydedin.

1. İş akışınızı Microsoft Flow mobil uygulamasını deneyin. Git **düğmeleri** uygulama sekmesinde. -> Yeni bir cihaz iş akışı oluşturma düğmenizin görmeniz gerekir. Yeni Cihazınızı adını girin ve IOT Central ' gösterilmesi izleyin!

    ![Flow mobil cihaz ekran oluşturma](./media/howto-add-microsoft-flow/flowmobilescreenshot.png)

## <a name="update-a-device-in-a-workflow"></a>Bir cihaz bir iş akışında güncelleştir

Bu bölümde, cihaz ayarlarını ve özelliklerini IOT Central içinde bir düğme anında iletme Microsoft Flow mobil uygulamasını kullanarak mobil bir cihazda güncelleştirme işlemini göstermektedir.

1. Microsoft Flow boş bir iş akışı oluşturarak başlayın.

1. Arama **mobil için akış düğmesi** bir tetikleyici olarak.

1. Bu tetikleyici, giriş gibi ekleme bir **konumu** karşılık gelen metin girişi değiştirmek istediğiniz bir ayar veya özelliği. Açıklama metnini değiştirin.

1. Yeni bir eylem ekleyin. Arama **Azure IOT Central - bir cihaz güncelleştirmesi** eylem.

1. Açılır listeden uygulamanızı seçin. Şimdi, güncelleştirmek istediğiniz var olan cihazın kimliği gerekir. 

    > [!NOTE] 
    > **URL'de bulunan kimliği kullanmalıdır** güncelleştirmek istediğiniz cihazın cihaz Ayrıntıları sayfasında. Cihaz explorer'ın cihaz listesinde bulunan cihaz kimliği Microsoft Flow kullanmak için doğru olanı değil.

    ![URL'den IOT Central kimliği](./media/howto-add-microsoft-flow/iotcdeviceidurl.png)

1. Cihaz adını güncelleştirebilirsiniz. Cihazın özellikleri ve ayarları güncelleştirmek için cihaz şablonu güncelleştirmek istediğiniz cihazı seçin **cihaz şablonu** açılır. Tüm özellikleri ve ayarları güncelleştirebilirsiniz göstermek için eylem kutucuk genişletir.

    ![Akış güncelleştirme cihaz iş akışı](./media/howto-add-microsoft-flow/flowupdatedevice1.png)

1. Her özellik ve güncelleştirmek istediğiniz ayarları seçin. Dinamik içerik bölmesinden tetikleyiciden karşılık gelen bir giriş seçin. Bu örnekte konum değeri aşağı cihazın konum özelliği güncelleştirilecek yayılır.

1. Son olarak, iş akışınızı kaydedin.

1. İş akışınızı Microsoft Flow mobil uygulamasını deneyin. Git **düğmeleri** uygulama sekmesinde. Bir cihaz iş akışını güncelleştirme -> düğmenizin görmeniz gerekir. Girişleri girin ve IOT Central ' güncelleştirilmesi cihaz bakın!

## <a name="get-device-information-in-a-workflow"></a>Bir iş akışında cihaz bilgilerini alma

Kimliği kullanarak cihaz bilgilerini alabileceğiniz **Azure IOT Central - bir aygıt alma** eylem. 
> [!NOTE] 
> **URL'de bulunan kimliği kullanmalıdır** güncelleştirmek istediğiniz cihazın cihaz Ayrıntıları sayfasında. Cihaz explorer'ın cihaz listesinde bulunan cihaz kimliği Microsoft Flow kullanmak için doğru olanı değil.

Cihaz adı, cihaz şablonu adı, özellik değerlerini ve iş akışınızı sonraki eylemlerde geçirilecek ayarları değerlerini gibi daha fazla bilgi edinebilirsiniz. Müşteri adı özellik değeri bir CİHAZDAN için Microsoft Teams geçirir. bir örnek iş akışı şu şekildedir.

   ![Akış get cihaz iş akışı](./media/howto-add-microsoft-flow/flowgetdevice1.png)


## <a name="run-a-command-on-a-device-in-a-workflow"></a>Bir komut bir iş akışında bir cihazda çalıştırma
Kendi kimliği kullanılarak belirtilen bir cihaz üzerinde komut çalıştırabilirsiniz **Azure IOT bir komut Merkezi -** eylem. 

> [!NOTE] 
> **URL'de bulunan kimliği kullanmalıdır** güncelleştirmek istediğiniz cihazın cihaz Ayrıntıları sayfasında. Cihaz explorer'ın cihaz listesinde bulunan cihaz kimliği Microsoft Flow kullanmak için doğru olanı değil.
    
Komutu çalıştırın ve komut parametrelerinde bu eylem geçirmek için seçebilirsiniz. Microsoft Flow mobil uygulamasında bir düğmeyle cihaz yeniden başlatma komutu çalışan bir örnek iş akışı şu şekildedir.

   ![Akış get cihaz iş akışı](./media/howto-add-microsoft-flow/flowrunacommand1.png)

## <a name="delete-a-device-in-a-workflow"></a>Bir iş akışında bir cihazı silme

Bir cihaz kimliği kullanarak silebilirsiniz **Azure IOT Central - bir cihazı silme** eylem. 
> [!NOTE] 
> **URL'de bulunan kimliği kullanmalıdır** güncelleştirmek istediğiniz cihazın cihaz Ayrıntıları sayfasında. Cihaz explorer'ın cihaz listesinde bulunan cihaz kimliği Microsoft Flow kullanmak için doğru olanı değil.

Microsoft Flow mobil uygulamasındaki bir düğmeye bir cihazda silen bir örnek iş akışı şu şekildedir.

   ![Akışı Sil cihaz iş akışı](./media/howto-add-microsoft-flow/flowdeletedevice.png)

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

İş akışlarını oluşturmak için Microsoft Flow kullanmayı öğrendiniz, önerilen sonraki adım olarak [cihazları yönetme](howto-manage-devices.md).

