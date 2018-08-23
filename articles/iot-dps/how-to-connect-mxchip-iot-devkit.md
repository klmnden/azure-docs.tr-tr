---
title: MXChip IOT DevKit IOT Hub'ınızla kaydolmak için Azure IOT Hub cihazı sağlama hizmeti otomatik sağlama kullanma | Microsoft Docs
description: MXChip IOT DevKit IOT Hub'ınızla kaydolmak için Azure IOT Hub cihazı sağlama hizmeti otomatik sağlama kullanma
author: liydu
ms.author: liydu
ms.date: 04/04/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: jeffya
ms.openlocfilehash: d8912a5da8c4df2069d8bc53454748b5fb3d5c39
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "42059678"
---
# <a name="use-azure-iot-hub-device-provisioning-service-auto-provisioning-to-register-the-mxchip-iot-devkit-with-iot-hub"></a>MXChip IOT DevKit IOT Hub'ınızla kaydolmak için Azure IOT Hub cihazı sağlama hizmeti otomatik sağlama kullanın

Bu makalede, Azure IOT Hub cihazı sağlama hizmeti kullanmayı açıklar [otomatik sağlama](concepts-auto-provisioning.md)MXChip IOT DevKit Azure IOT Hub'ınızla kaydolmak için. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

* Bir cihazda cihaz sağlama hizmeti genel uç noktasını yapılandırın.
* Benzersiz cihaz gizli dizi (UD) bir X.509 sertifikası oluşturmak için kullanın.
* Tek bir cihazı kaydetme.
* Cihaz kayıtlı olduğunu doğrulayın.

[MXChip IOT DevKit](https://aka.ms/iot-devkit) bir zengin çevre ve sensörlerden hepsi bir arada Arduino ile uyumlu panosudur. Bunun için kullanarak geliştirme [Arduino için Visual Studio Code uzantısı](https://aka.ms/arduino). DevKit büyüyen ile birlikte gelen [projeleri Kataloğu](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) Azure hizmetlerinden yararlanan prototip nesnelerin interneti (IOT) çözümlerinizi gösterecek.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğreticideki adımları tamamlamak için önce aşağıdaki görevleri yapın:

* İçindeki adımları izleyerek, DevKit hazırlama [IOT DevKit AZ3166 bulutta Azure IOT hub'a bağlanma](/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started).
* En son üretici yazılımı yükseltin (1.3.0 veya üzeri) ile [güncelleştirme DevKit bellenim](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/) öğretici.
* Oluşturma ve adımları izleyerek bir IOT hub'a bir cihaz sağlama hizmeti örneği ile bağlantı [IOT Hub cihazı sağlama hizmeti Azure portalıyla ayarlama](/azure/iot-dps/quick-setup-auto-provision).

## <a name="build-and-deploy-auto-provisioning-registration-software-to-the-device"></a>Oluşturun ve otomatik sağlama kayıt yazılımı cihaza dağıtın

DevKit oluşturduğunuz cihaz sağlama hizmeti örneğine bağlanmak için:

1. Azure portalında **genel bakış** not alın ve cihaz sağlama hizmeti bölmesinde **genel cihaz uç noktası** ve **kimlik kapsamı** değerleri.
  ![Cihaz sağlama hizmeti genel uç noktası ve kimlik kapsamı](./media/how-to-connect-mxchip-iot-devkit/dps-global-endpoint.png)

2. Olduğundan emin olun `git` makinenizde yüklü ve komut penceresinden erişilebilir ortam değişkenlerine eklenir. Bkz: [Software Freedom conservancy'nin Git istemci araçlarını](https://git-scm.com/download/) yüklü en son sürümü için.

3. Bir komut istemi açın. Cihaz sağlama hizmeti örnek kod için GitHub deposunu kopyalayın:
  ```bash
  git clone https://github.com/DevKitExamples/DevKitDPS.git
  ```

4. Visual Studio Code'u açın, DevKit bilgisayarınıza bağlayın ve ardından kopyaladığınız kod içeren klasörü açın.

5. Açık **DevKitDPS.ino**. Bul ve Değiştir `[Global Device Endpoint]` ve `[ID Scope]` yalnızca aşağı değerlerle.
  ![Cihaz sağlama Hizmeti uç noktası](./media/how-to-connect-mxchip-iot-devkit/endpoint.png) bırakabilirsiniz **RegistrationId** boş. Uygulama, MAC adresi ve üretici yazılımı sürüme göre sizin için oluşturur. Kayıt Kimliği özelleştirmek istiyorsanız, yalnızca alfasayısal, küçük harf kullanın ve en fazla 128 karakter birleşimlerine kısa çizgi. Daha fazla bilgi için [Azure portalı ile cihaz kayıtlarını yönetme](https://docs.microsoft.com/azure/iot-dps/how-to-manage-enrollments).

6. VS code'da Quick Open'ı kullanın (Windows: `Ctrl+P`, macOS: `Cmd+P`) ve türü *cihaz karşıya yükleme görevi* oluşturup Devkit'e kodu yükleyin.

7. Çıkış penceresi, görev başarılı olup olmadığını gösterir.

## <a name="save-a-unique-device-secret-on-an-stsafe-security-chip"></a>Benzersiz cihaz gizli bir STSAFE güvenlik yonga üzerinde Kaydet

Cihazın bir cihazda otomatik sağlama yapılandırılabilir [kanıtlama mekanizması](concepts-security.md#attestation-mechanism). MXChip IOT DevKit kullanan [cihaz kimliği bileşim motoru](https://trustedcomputinggroup.org/wp-content/uploads/Foundational-Trust-for-IOT-and-Resource-Constrained-Devices.pdf) gelen [Trusted Computing Group](https://trustedcomputinggroup.org). A *benzersiz cihaz gizli* (UD) kaydedilmiş bir STSAFE güvenlik yonga üzerinde DevKit üretmek için kullanılan cihazın benzersiz [X.509 sertifikası](concepts-security.md#x509-certificates). Sertifika, daha sonra cihaz sağlama hizmeti ve çalışma zamanında kayıt sırasında kayıt işlemi için kullanılır.

Aşağıdaki örnekte görüldüğü gibi bir genel benzersiz cihaz parolası 64 karakterli bir dizedir:

```
19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eae
```

Dize Güvenlik hesaplanmasında kullanılan karakter çiftlerine ayrılmıştır. Önceki örnekte UD çözüldü: `0x19`, `0xe2`, `0x5a`, `0x25`, `0x9d`, `0x0c`, `0x2b`, `0xe0`, `0x3a`, `0x02`, `0xd4`, `0x16` , `0xc0`, `0x5c`, `0x48`, `0xcc`, `0xd0`, `0xcc`, `0x7d`, `0x17`, `0x43`, `0x45`, `0x8a`, `0xae`, `0x1c`, `0xb4`, `0x88`, `0xb0`, `0x74`, `0x99`, `0x3e`, `0xae`.

Benzersiz cihaz gizli dizi üzerinde DevKit kaydetmek için:

1. Putty gibi bir araç kullanarak seri İzleyicisi'ni açın. Bkz: [yapılandırma modunu kullan](https://microsoft.github.io/azure-iot-developer-kit/docs/use-configuration-mode/) Ayrıntılar için.

2. Bilgisayarınıza bağlı ile DevKit basılı **A** düğmesini tuşuna basın ve yayın **sıfırlama** düğmesini yapılandırma modunu girin. Ekran DevKit kimliği ve yapılandırma gösterilmektedir.

3. Örnek UD dize ve diğer değerleri arasında bir veya daha fazla karakter geçin ele `0` ve `f` kendi UD için.

4. Seri İzle penceresine *set_dps_uds [your_own_uds_value]* ve Enter tuşuna basın.
  > [!NOTE]
  > Örneğin, son iki karakter değiştirerek kendi UD ayarlarsanız `f`, şunun gibi komut girmeniz gerekir: `set_dps_uds 19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eff`.

5. Seri İzleyici pencereyi kapatmadan basın **sıfırlama** DevKit düğmesi.

6. Not **DevKit MAC adresi** ve **DevKit üretici yazılımı sürümü** değerleri.
  ![Üretici yazılımı sürümü](./media/how-to-connect-mxchip-iot-devkit/firmware-version.png)

## <a name="generate-an-x509-certificate"></a>Bir X.509 sertifikası oluşturma

Şimdi bir X.609 sertifika oluşturmanız gerekir. 

### <a name="windows"></a>Windows

1. Dosya Gezgini'ni açın ve daha önce kopyaladığınız cihaz sağlama hizmeti örnek kodu içeren klasöre gidin. İçinde **.yapı** klasörü bulun ve kopyalama **DPS.ino.bin** ve **DPS.ino.map**.
  ![Oluşturulan dosyalar](./media/how-to-connect-mxchip-iot-devkit/generated-files.png)
  > [!NOTE]
  > Değiştirdiyseniz `built.path` Arduino yapılandırma başka bir klasöre, yapılandırdığınız bir klasördeki dosyaları bulmak için ihtiyacınız.

2. Bu iki dosyalarına yapıştırın **Araçları** klasör ile aynı düzeyde **.yapı** klasör.

3. Çalıştırma **dps_cert_gen.exe**. Girmek için istemleri takip edin, **UD**, **MAC adresi** DevKit için ve **üretici yazılımı sürümü** X.509 sertifikası oluşturmak için.
  ![DPS cert gen.exe çalıştırın](./media/how-to-connect-mxchip-iot-devkit/dps-cert-gen.png)

4. X.509 Sertifika oluşturulduktan sonra bir **.pem** sertifika aynı klasöre kaydedilir.

## <a name="create-a-device-enrollment-entry-in-the-device-provisioning-service"></a>Cihaz sağlama hizmetinde bir cihaz kayıt girişi oluşturma

1. Azure portalında cihaz sağlama hizmeti örneğine gidin. Seçin **kayıtları Yönet**ve ardından **bireysel kayıtlar** sekmesi. ![Bireysel kayıtlar](./media/how-to-connect-mxchip-iot-devkit/individual-enrollments.png)

2. **Add (Ekle)** seçeneğini belirleyin.

3. "Kayıt Ekle" Panoda:

   - Seçin **X.509** altında **mekanizması**.
   - "Dosya" altında Seç **birincil sertifika .pem veya .cer dosyası**.
   - Dosya Aç iletişim gidin ve karşıya yükleme **.pem** ürettiğiniz sertifikayı.
   - Geri kalan varsayılan olarak bırakın ve tıklayın **Kaydet**.

   ![Sertifikayı karşıya yükleme](./media/how-to-connect-mxchip-iot-devkit/upload-cert.png)

  > [!NOTE]
  > Bu iletiyi bir hata varsa:
  >
  > `{"message":"BadRequest:{\r\n \"errorCode\": 400004,\r\n \"trackingId\": \"1b82d826-ccb4-4e54-91d3-0b25daee8974\",\r\n \"message\": \"The certificate is not a valid base64 string value\",\r\n \"timestampUtc\": \"2018-05-09T13:52:42.7122256Z\"\r\n}"}`
  >
  > Sertifika dosyasını Aç **.pem** metin (Not Defteri veya herhangi bir metin düzenleyicisi ile açık) olarak ve satırları silin:
  >
  > `"-----BEGIN CERTIFICATE-----"` ve `"-----END CERTIFICATE-----"`.
  >

## <a name="start-the-devkit"></a>DevKit Başlat

1. VS Code ve seri İzleyicisi'ni açın.

2. Tuşuna **sıfırlama** , DevKit düğmesi.

Cihaz sağlama hizmetinize kaydı DevKit başlangıç görürsünüz.

![VS kod çıkışı](./media/how-to-connect-mxchip-iot-devkit/vscode-output.png)

## <a name="verify-that-the-devkit-is-registered-with-azure-iot-hub"></a>Azure IOT Hub ile DevKit kayıtlı olduğunu doğrulayın

Cihaz başlatıldıktan sonra aşağıdaki işlemler yapılır:

1. Cihaz, Cihaz Sağlama hizmetinize bir kayıt isteği gönderir.
2. Cihaz sağlama hizmetine geri Cihazınızı yanıt vereceği bir kayıt sınaması gönderir.
3. Kayıt başarılı olduğunda, cihaz sağlama hizmeti, IOT Hub URI'sini, cihaz Kimliğini ve şifrelenmiş anahtarı cihaza geri gönderir.
4. Cihazın IOT Hub istemci uygulaması, hub'ınıza bağlanır.
5. Hub'a başarılı bağlantı üzerinde görünmesi IOT hub'ı Device Explorer bakın.
  ![Cihaz kaydedildi](./media/how-to-connect-mxchip-iot-devkit/device-registered.png)

## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

Sorunlarla karşılaşırsanız, IOT Devkit'e bakın [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/), veya aşağıdaki kanalları desteği ulaşın:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure IOT Hub ile cihaz otomatik olarak kaydedebilir, böylece cihaz kimliği bileşim motoru kullanarak güvenli bir şekilde cihaz sağlama Hizmeti'ne cihazı kaydetme işlemini öğrendiniz. 

Öğrendiğiniz Özet olarak, nasıl yapılır:

> [!div class="checklist"]
> * Bir cihazda cihaz sağlama hizmeti genel uç noktasını yapılandırın.
> * Benzersiz cihaz gizli bir X.509 sertifikası oluşturmak için kullanın.
> * Tek bir cihazı kaydetme.
> * Cihaz kayıtlı olduğunu doğrulayın.

Bilgi edinmek için nasıl [sanal cihaz oluşturma ve sağlama](./quick-create-simulated-device.md).

