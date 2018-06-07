---
title: Azure cihaz çiftlerini MXChip IOT DevKit kullanıcı LED denetlemek için kullandığı | Microsoft Docs
description: Bu öğreticide, DevKit durumlarını izleyebilir ve Azure IOT Hub cihaz çiftlerini LED kullanıcıyla denetlemek öğrenin.
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/04/2018
ms.author: liydu
ms.openlocfilehash: 2845b545484f4eef1e6999c1c54aaba5de14b832
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34633273"
---
# <a name="mxchip-iot-devkit"></a>MXChip IOT DevKit

Bu örnek MXChip IOT DevKit WiFi bilgi ve algılayıcı durumlarını izlemek için ve Azure IOT Hub cihaz çiftlerini kullanarak kullanıcı LED rengi denetlemek için kullanabilirsiniz.

## <a name="what-you-learn"></a>Öğrenecekleriniz

- MXChip IOT DevKit algılayıcı izleme belirtir.
- Azure cihaz çiftlerini DevKit's RGB LED rengi denetlemek için nasıl kullanılacağını.

## <a name="what-you-need"></a>Ne gerekiyor

- İzleyerek geliştirme ortamınızı ayarlayın [Başlarken Kılavuzu](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started).
- Gitbash'i terminal pencerenizi (veya diğer Git komut satırı arabirimi), aşağıdaki komutları yazın:
    - `git clone https://github.com/DevKitExamples/DevKitState.git`
    - `cd DevKitState`
    - `code .`

## <a name="provision-azure-services"></a>Azure hizmetleri sağlama

1. Tıklatın **görevleri** açılır menü Visual Studio Code seçip **görevi çalıştır...**   -  **bulut sağlama**.
2. İlerleme durumunuzu altında görüntülenen **TERMINAL** sekmesinde **Hoş Geldiniz** paneli.
3. İletiyle istendiğinde *hangi aboneliğe seçmek ister misiniz*, bir abonelik seçin.
4. Veya bir kaynak grubu seçin. 
 
    > [!NOTE]
    > Bir ücretsiz IOT hub'ı zaten varsa, bu adım atlanır.

5. İletiyle istendiğinde *hangi IOT hub'ı seçmek ister misiniz*IOT hub'ı oluşturun veya seçin.
6. Benzer bir şey *işlev uygulaması: işlev uygulama adı: xxx*, görüntülenir. İşlev uygulama adını yazın; bir sonraki adımda kullanılır.
7. Tamamlamak Azure Resource Manager şablonu dağıtımı için bekleyin ne zaman gösterilen ileti *Resource Manager şablon dağıtımı: Bitti* görüntülenir.

## <a name="deploy-function-app"></a>İşlev uygulaması dağıtma

1. Tıklatın **görevleri** açılır menü Visual Studio Code seçip **görevi çalıştır...**   -  **bulut dağıtmak**.
2. İşlev uygulama kodu için karşıya yükleme işleminin tamamlanması için bekleyin; İleti *işlevi uygulamayı dağıtır: Bitti* görüntülenir.

## <a name="configure-iot-hub-device-connection-string-in-devkit"></a>İçinde DevKit IOT Hub cihaz bağlantı dizesini yapılandırma

1. MXChip IOT DevKit bilgisayarınıza bağlayın.
2. Tıklatın **görevleri** açılır menü Visual Studio Code seçip **görevi çalıştır...**   -  **config cihaz bağlantısı**
3. MXChip IOT DevKit, basın ve tutma düğmesi **A**, basın **sıfırlama** düğmesi ve yayın düğmesi **A** yapılandırma moduna DekKit yapmak için.
4. Tamamlanacak bağlantı dizesini yapılandırma işleminin bitmesini bekleyin.

## <a name="upload-arduino-code-to-devkit"></a>Arduino kod DevKit için karşıya yükleme

İle MXChip IOT DevKit bilgisayarınıza bağlı:
1. Tıklatın **görevleri** açılır menü Visual Studio Code seçip **derleme görevi çalıştır...** Arduino taslak derlenmiş ve DevKit yüklenir.
2. Taslak başarıyla karşıya yüklendi olduğunda bir *yapı & Karşıya Yükle taslak: başarı* iletisi görüntülenir.

## <a name="monitor-devkit-state-in-browser"></a>Tarayıcıda İzleyici DevKit durumu

1. Bir Web tarayıcısında Aç `DevKitState\web\index.html` sırasında oluşturulan dosya-- [gerekenleri](#whatyouneed) adım.
2. Aşağıdaki Web sayfasında görünür:![İşlev uygulaması adı belirtin.](media/iot-hub-arduino-iot-devkit-az3166-devkit-state/devkit-state-function-app-name.png)
1. Daha önce yazdığınız işlevi uygulama adı girin.
2. Tıklatın **Bağlan** düğmesi
3. Birkaç saniye içinde sayfa yenilenir ve DevKit'ın WiFi bağlantı durumunu ve yerleşik algılayıcılar her birinin durumunu görüntüler.

## <a name="control-the-devkits-user-led"></a>DevKit'ın kullanıcı LED denetimi

1. Web sayfası çizimi üzerinde kullanıcı LED grafiği tıklatın.
2. Birkaç saniye içinde ekran yeniler ve geçerli kullanıcının LED renk durumunu gösterir.
3. RGB kaydırıcı denetimlerini çeşitli konumlarının tıklayarak RGB LED renk değerini değiştirmeyi deneyin.

## <a name="example-operation"></a>Örnek işlemi

![Örnek test yordamı](media/iot-hub-arduino-iot-devkit-az3166-devkit-state/devkit-state.gif)

> [!NOTE]
> Cihaz çifti Azure portalında ham verileri görebilirsiniz: IOT Hub -\> IOT cihazları -\> *\<Cihazınızı\>*  - \> cihaz çifti.

## <a name="next-steps"></a>Sonraki adımlar

Öğrendiğiniz nasıl yapılır:
- Bir MXChip IOT DevKit cihazı, Azure IOT Uzaktan izleme Çözüm Hızlandırıcısı bağlayın.
- Anlam ve DevKit's RGB LED rengi denetlemek için Azure IOT cihaz çiftlerini işlevini kullanın.

Önerilen sonraki adımlar şunlardır:

* [Azure IOT Uzaktan izleme Çözüm Hızlandırıcısı genel bakış](https://docs.microsoft.com/azure/iot-suite/)
* [Azure IOT merkezi uygulamanıza bir MXChip IOT DevKit cihazı bağlayın](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit)
