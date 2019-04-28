---
title: MXChip IOT DevKit kullanıcı LED denetlemek için Azure cihaz ikizlerini kullanma | Microsoft Docs
description: Bu öğreticide, DevKit durumlu monitör ve Azure IOT Hub cihaz ikizlerini ile LED kullanıcı denetimi hakkında bilgi edinin.
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/04/2018
ms.author: liydu
ms.openlocfilehash: e955d21132dda6caa137ad3b5de9d00ccf7ed1b4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61369864"
---
# <a name="mxchip-iot-devkit"></a>MXChip IoT DevKit

Bu örnekte MXChip IOT DevKit WiFi bilgi ve algılayıcısı durumlarını izlemek ve Azure IOT Hub cihaz ikizlerini kullanarak kullanıcı LED rengini denetlemek için kullanabilirsiniz.

## <a name="what-you-learn"></a>Öğrenecekleriniz

- MXChip IOT DevKit algılayıcıyı izleme belirtir.

- RGB LED'i DevKit'ın rengini denetlemek için Azure cihaz ikizlerini kullanma

## <a name="what-you-need"></a>Ne gerekiyor

- Takip ederek geliştirme ortamınızı ayarlama [Başlarken Kılavuzu](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started).

- GitBash terminal penceresini (veya diğer Git komut satırı arabirimi), aşağıdaki komutları yazın:

   ```bash
   git clone https://github.com/DevKitExamples/DevKitState.git
   cd DevKitState
   code .
   ```

## <a name="provision-azure-services"></a>Azure hizmetleri sağlama

1. Tıklayın **görevleri** aşağı açılan menüyü seçip Visual Studio Code **görevi çalıştır...**   -  **bulut sağlama**.

2. İlerleme durumunuzu altında görüntülenen **TERMINAL** sekmesinde **Hoş Geldiniz** paneli.

3. İletinin istendiğinde *hangi aboneliğe seçmek ister misiniz*, bir abonelik seçin.

4. Veya bir kaynak grubu seçin. 
 
   > [!NOTE]
   > Bir ücretsiz IOT hub'ı zaten varsa bu adımı atlayabilirsiniz.

5. İletinin istendiğinde *hangi IOT hub'ı seçmek ister misiniz*seçin veya bir IOT Hub oluşturun.

6. Benzer bir şey *işlev uygulaması: işlev uygulaması adı: xxx*, görüntülenir. İşlev uygulamanızın adını yazabilirsiniz. bir sonraki adımda kullanılır.

7. Tamamlamak Azure Resource Manager şablon dağıtımı için bekleme zaman belirtilen ileti *Resource Manager şablon dağıtımı: Bitti* görüntülenir.

## <a name="deploy-function-app"></a>İşlev uygulaması dağıtma

1. Tıklayın **görevleri** aşağı açılan menüyü seçip Visual Studio Code **görevi çalıştır...**   -  **Bulutu dağıtma**.

2. İşlevi uygulama kodunuz için karşıya yükleme işleminin tamamlanması için bekleyin; İleti *işlev uygulaması dağıtır: Bitti* görüntülenir.

## <a name="configure-iot-hub-device-connection-string-in-devkit"></a>DevKit IOT Hub cihaz bağlantı dizesini yapılandırma

1. MXChip IOT Devkit'inize bilgisayarınıza bağlayın.

2. Tıklayın **görevleri** aşağı açılan menüyü seçip Visual Studio Code **görevi çalıştır...**   -  **yapılandırma cihaz bağlantısı**

3. MXChip IOT DevKit, tuşuna basın ve basılı düğmesi **bir**, basın **sıfırlama** düğmesini ve ardından yayın düğmesini **A** yapılandırma moduna DekKit yapmak.

4. Tamamlanması bağlantı dizesi yapılandırma işleminin bitmesini bekleyin.

## <a name="upload-arduino-code-to-devkit"></a>Arduino kod Devkit'e karşıya yükleme

MXChip IOT Devkit'inize bilgisayarınıza bağlı:

1. Tıklayın **görevleri** aşağı açılan menüyü seçip Visual Studio Code **derleme görevi çalıştır...** Arduino taslak derlenir ve Devkit'e karşıya yüklendi.

2. Taslak başarıyla yüklendiğinde bir *derleme ve karşıya yükleme taslak: başarılı* iletisi görüntülenir.

## <a name="monitor-devkit-state-in-browser"></a>Tarayıcıda DevKit durumu İzleyicisi

1. Bir Web tarayıcısında açın `DevKitState\web\index.html` oluşturulan dosya--sırasında adım gerekenler.

2. Aşağıdaki Web sayfasında görünür:![İşlev uygulamanızın adını belirtin.](media/iot-hub-arduino-iot-devkit-az3166-devkit-state/devkit-state-function-app-name.png)

3. Daha önce yazdığınız işlev uygulamanızın adını girin.

4. Tıklayın **Connect** düğmesi

5. Birkaç saniye içinde sayfayı yeniler ve DevKit'ın Wi-Fi bağlantı durumunu ve her bir yerleşik sensör durumunu görüntüler.

## <a name="control-the-devkits-user-led"></a>LED DevKit'ın kullanıcı denetimi

1. Web sayfası çizimi üzerinde kullanıcı LED grafiğine tıklayın.

2. Birkaç saniye içinde ekranı yeniler ve kullanıcı LED geçerli renk durumunu gösterir.

3. RGB kaydırıcı denetimleri çeşitli konumlarının tıklayarak RGB LED'i renk değerini değiştirmeyi deneyin.

## <a name="example-operation"></a>Örnek işlemi

![Örnek test yordamı](media/iot-hub-arduino-iot-devkit-az3166-devkit-state/devkit-state.gif)

> [!NOTE]
> Azure portalında cihaz ikizi ham verileri görebilirsiniz: IOT hub'ı -\> IOT cihazları -\> *\<Cihazınızı\>*  - \> cihaz İkizi.

## <a name="next-steps"></a>Sonraki adımlar

Öğrendiğiniz nasıl yapılır:
- MXChip IOT DevKit cihaz, Azure IOT Uzaktan izleme çözüm hızlandırıcısına bağlamayı.
- Azure IOT cihaz ikizlerini işlevi algılayacak ve DevKit'ın RGB LED'i rengini denetlemek için kullanın.

Önerilen sonraki adımlar şunlardır:

* [Azure IOT Uzaktan izleme çözüm hızlandırıcısına genel bakış](https://docs.microsoft.com/azure/iot-suite/)
* [Azure IOT Central uygulamanıza bir MXChip IOT DevKit cihazı bağlayın](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit)
