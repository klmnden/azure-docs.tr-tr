---
title: MXChip IOT DevKit IOT Hub'ınızla kaydolmak için Azure IOT Hub cihazı sağlama hizmeti otomatik sağlama kullanma | Microsoft Docs
description: MXChip IOT DevKit IOT Hub'ınızla kaydolmak için Azure IOT Hub cihazı sağlama hizmeti otomatik sağlama kullanma
author: liydu
ms.author: liydu
ms.date: 12/18/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: jeffya
ms.openlocfilehash: 80e4895e0b276e701a6d7f10d8fc67649db0f188
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58904500"
---
# <a name="use-azure-iot-hub-device-provisioning-service-auto-provisioning-to-register-the-mxchip-iot-devkit-with-iot-hub"></a>MXChip IOT DevKit IOT Hub'ınızla kaydolmak için Azure IOT Hub cihazı sağlama hizmeti otomatik sağlama kullanın

Bu makalede, Azure IOT Hub cihazı sağlama hizmeti kullanmayı açıklar [otomatik sağlama](concepts-auto-provisioning.md)MXChip IOT DevKit Azure IOT Hub'ınızla kaydolmak için. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

* Bir cihazda cihaz sağlama hizmeti genel uç noktasını yapılandırın.
* Benzersiz cihaz gizli dizi (UD) bir X.509 sertifikası oluşturmak için kullanın.
* Tek bir cihazı kaydetme.
* Cihaz kayıtlı olduğunu doğrulayın.

[MXChip IOT DevKit](https://aka.ms/iot-devkit) bir zengin çevre ve sensörlerden hepsi bir arada Arduino ile uyumlu panosudur. Onu kullanarak geliştirebilirsiniz [Azure IOT cihaz Workbench](https://aka.ms/iot-workbench) veya [Azure IOT Araçları](https://aka.ms/azure-iot-tools) Visual Studio Code uzantısı paketinde. DevKit büyüyen ile birlikte gelen [projeleri Kataloğu](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) Azure hizmetlerinden yararlanan prototip nesnelerin interneti (IOT) çözümlerinizi gösterecek.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğreticideki adımları tamamlamak için önce aşağıdaki görevleri yapın:

* DevKit'ın Wi-Fi yapılandırma ve adımları izleyerek geliştirme ortamınızı hazırlama [IOT DevKit AZ3166 bulutta Azure IOT hub'a bağlanma](/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started).
* En son üretici yazılımı yükseltin (1.3.0 veya üzeri) ile [güncelleştirme DevKit bellenim](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/) öğretici.
* Oluşturma ve adımları izleyerek bir IOT hub'a bir cihaz sağlama hizmeti örneği ile bağlantı [IOT Hub cihazı sağlama hizmeti Azure portalıyla ayarlama](/azure/iot-dps/quick-setup-auto-provision).

## <a name="open-sample-project"></a>Açık örnek proje

1. Kendi IOT DevKit olduğundan emin olun **bağlı** bilgisayarınıza. VS Code ilk kez başlatın ve ardından DevKit bilgisayarınıza bağlayın.

1. Tıklayın `F1` komut paletini açın için girin ve seçin **Azure IOT cihaz Workbench: Örnek Aç...** . Ardından **IOT DevKit** tablosu olarak.

1. IOT Workbench örnekler sayfasında bulma **DPS ile cihaz kaydı** tıklatıp **açık örnek**. Ardından örnek kodu indirmek için varsayılan yolu seçer.
    ![Açık örnek](media/how-to-connect-mxchip-iot-devkit/open-sample.png)

## <a name="save-a-unique-device-secret-on-device-security-storage"></a>Cihaz güvenlik depolama alanında benzersiz bir cihaz gizli Kaydet

Cihazın bir cihazda otomatik sağlama yapılandırılabilir [kanıtlama mekanizması](concepts-security.md#attestation-mechanism). MXChip IOT DevKit kullanan [cihaz kimliği bileşim motoru](https://trustedcomputinggroup.org/wp-content/uploads/Foundational-Trust-for-IOT-and-Resource-Constrained-Devices.pdf) gelen [Trusted Computing Group](https://trustedcomputinggroup.org). A **benzersiz cihaz gizli** (UD) kaydedilmiş bir STSAFE güvenlik yongası ([STSAFE A100](https://microsoft.github.io/azure-iot-developer-kit/docs/understand-security-chip/)) üzerinde DevKit oluşturmak için kullanılan cihazın benzersiz [X.509 sertifikası](concepts-security.md#x509-certificates). Sertifika, daha sonra cihaz sağlama hizmeti ve çalışma zamanında kayıt sırasında kayıt işlemi için kullanılır.

Aşağıdaki örnekte görüldüğü gibi tipik bir UD 64 karakterli bir dizedir verilmiştir:

```
19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eae
```

Bir UD üzerinde DevKit kaydetmek için:

1. VS Code'da COM bağlantı noktası için DevKit seçmek için durum çubuğuna tıklayın.
  ![Select COM bağlantı noktası](media/how-to-connect-mxchip-iot-devkit/select-com.png)

1. DevKit üzerinde basılı **bir düğme**, gönderme ve yayın **sıfırlama** düğmesini ve ardından sürüm **bir düğme**. Yapılandırma modu, DevKit girer.

1. Tıklayın `F1` komut paletini açın için girin ve seçin **Azure IOT cihaz Workbench: Cihaz ayarlarını yapılandırma > yapılandırma benzersiz cihaz dizesi (UD)**.
  ![UD yapılandırın](media/how-to-connect-mxchip-iot-devkit/config-uds.png)

1. Oluşturulan UD dize unutmayın. X.509 sertifikası oluşturmak için ihtiyacınız. Tuşuna basarak `Enter`.
  ![UD kopyalayın](media/how-to-connect-mxchip-iot-devkit/copy-uds.png)

1. Bildirimden UD STSAFE üzerinde başarılı bir şekilde yapılandırıldığını onaylayın.
  ![Yapılandırma UD başarılı](media/how-to-connect-mxchip-iot-devkit/config-uds-success.png)

> [!NOTE]
> Alternatif olarak, UD Putty gibi yardımcı programlar kullanarak seri bağlantı noktası yapılandırabilirsiniz. İzleyin [yapılandırma modunu kullan](https://microsoft.github.io/azure-iot-developer-kit/docs/use-configuration-mode/) Bunu yapmak için.

## <a name="update-the-global-device-endpoint-and-id-scope"></a>Genel cihaz uç noktası ve kimlik kapsamı güncelleştirme

Cihaz kodda belirtmeniz gerekir. [cihaz uç noktası sağlama](/azure/iot-dps/concepts-service#device-provisioning-endpoint) ve Kiracı yalıtımı sağlamak üzere kimlik kapsamı.

1. Azure portalında **genel bakış** not alın ve cihaz sağlama hizmeti bölmesinde **genel cihaz uç noktası** ve **kimlik kapsamı** değerleri.
  ![Cihaz sağlama hizmeti genel uç noktası ve kimlik kapsamı](media/how-to-connect-mxchip-iot-devkit/dps-global-endpoint.png)

1. Open **DeKitDPS.ino**. Bul ve Değiştir `[Global Device Endpoint]` ve `[ID Scope]` yalnızca aşağı değerlerle.
  ![Cihaz sağlama Hizmeti uç noktası](media/how-to-connect-mxchip-iot-devkit/endpoint.png)

1. Dolgu `registrationId` kodda değişken. Yalnızca alfasayısal, küçük harf, ve en çok 128 karakterden kısa çizgi birlikte izin verilir. Ayrıca değeri not.
  ![Kayıt Kimliği](media/how-to-connect-mxchip-iot-devkit/registration-id.png)

1. Tıklayın `F1`yazın ve seçin **Azure IOT cihaz Workbench: Cihaz kodu karşıya**. Derleme ve kod Devkit'e karşıya yükleme başlar.
  ![Cihaz yükleme](media/how-to-connect-mxchip-iot-devkit/device-upload.png)

## <a name="generate-x509-certificate"></a>X.509 sertifikası oluşturma

[Kanıtlama mekanizması](/azure/iot-dps/concepts-device#attestation-mechanism) Bu örnek tarafından kullanılan X.509 sertifikasıdır. Sertifikayı oluşturmak için bir yardımcı programını kullanmanız gerekir.

> [!NOTE]
> X.509 Sertifika Oluşturucu yalnızca artık Windows destekler.

1. VS Code'da tıklayın `F1`yazın ve seçin **yeni Terminal açın** terminal penceresini açın.

1. Çalıştırma `dps_cert_gen.exe` içinde `tool` klasör.

1. Derlenmiş ikili dosya konumu olarak belirtin `..\.build\DevKitDPS`. Yapıştırın **UD** ve **RegistrationId** yalnızca aşağı Not. 
  ![X.509 oluştur](media/how-to-connect-mxchip-iot-devkit/gen-x509.png)

1. A `.pem` aynı klasörde X.509 sertifikası oluşturur.
  ![X.509 dosyası](media/how-to-connect-mxchip-iot-devkit/pem-file.png)

## <a name="create-a-device-enrollment-entry"></a>Cihaz kaydı girişi oluşturma

1. Azure portalında cihaz sağlama hizmetinizi açın, kayıtları bölüm yönetmek için gidin ve tıklayın **Ekle bireysel kayıt**.
  ![Tek kayıt ekleme](media/how-to-connect-mxchip-iot-devkit/add-enrollment.png)

1. Dosya simgesine tıklayın **birincil sertifika .pem veya .cer dosyası** yüklenecek `.pem` oluşturulan dosya.
  ![.Pem karşıya yükleme](media/how-to-connect-mxchip-iot-devkit/upload-pem.png)

## <a name="verify-the-devkit-is-registered-with-azure-iot-hub"></a>Azure IOT Hub ile DevKit kayıtlı olduğunu doğrulayın

Tuşuna **sıfırlama** , DevKit düğmesi. Görmelisiniz **DPS bağlı!** DevKit ekranda. Cihaz yeniden başlatıldıktan sonra aşağıdaki eylemler gerçekleşir:

1. Cihaz, Cihaz Sağlama hizmetinize bir kayıt isteği gönderir.
1. Cihaz sağlama hizmetine geri Cihazınızı yanıt vereceği bir kayıt sınaması gönderir.
1. Kayıt başarılı olduğunda, cihaz sağlama hizmeti, IOT Hub URI'sini, cihaz Kimliğini ve şifrelenmiş anahtarı cihaza geri gönderir.
1. Cihazın IOT Hub istemci uygulaması, hub'ınıza bağlanır.
1. Hub'a başarılı bağlantı üzerinde görünmesi IOT hub'ı Device Explorer bakın.
  ![Cihaz kaydedildi](./media/how-to-connect-mxchip-iot-devkit/device-registered.png)

## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

Sorunlarla karşılaşırsanız, IOT Devkit'e bakın [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/), veya aşağıdaki kanalları desteği ulaşın:

* [Gitter.im](https://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure IOT Hub ile cihaz otomatik olarak kaydedebilir, böylece cihaz kimliği bileşim motoru kullanarak güvenli bir şekilde cihaz sağlama Hizmeti'ne cihazı kaydetme işlemini öğrendiniz. 

Öğrendiğiniz Özet olarak, nasıl yapılır:

> [!div class="checklist"]
> * Bir cihazda cihaz sağlama hizmeti genel uç noktasını yapılandırın.
> * Benzersiz cihaz gizli bir X.509 sertifikası oluşturmak için kullanın.
> * Tek bir cihazı kaydetme.
> * Cihaz kayıtlı olduğunu doğrulayın.

Bilgi edinmek için nasıl [sanal cihaz oluşturma ve sağlama](./quick-create-simulated-device.md).

