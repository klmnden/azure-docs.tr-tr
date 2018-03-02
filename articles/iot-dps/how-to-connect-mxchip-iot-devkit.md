---
title: "Azure IOT Hub cihaz hizmetine sağlama bağlanmak için MXChip IOT DevKit kullanma | Microsoft Docs"
description: "Azure IOT Hub cihaz hizmetine sağlama bağlanmak için MXChip IOT DevKit kullanma"
services: iot-dps
keywords: 
author: liydu
ms.author: liydu
ms.date: 02/20/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
<<<<<<< HEAD
ms.openlocfilehash: 697196f725f0cb8ad3054bd8336b588c17dddc3d
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
=======
ms.openlocfilehash: 77c8a6a5e384d27dca2582cc0fedc2bd23c0592e
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
>>>>>>> 3c7ee473158b1dd571d03cb27716afe08728885d
---
# <a name="connect-mxchip-iot-devkit-to-azure-iot-hub-device-provisioning-service"></a>Azure IOT Hub cihaza hizmet sağlama MXChip IOT DevKit bağlanma

Bu makalede, IOT Hub cihaz sağlama hizmeti kullanılarak otomatik olarak kaydedilecek sağlamak için DevKit yapılandırmanız açıklar. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

* Cihazda DPS genel uç noktasını yapılandırın
* X.509 sertifikası oluşturmak için benzersiz cihaz gizli anahtarı (UDS) kullanın
* Bir cihaz kaydetme
* Cihaz kayıtlı doğrulayın

<<<<<<< HEAD
=======
[MXChip IOT DevKit](https://aka.ms/iot-devkit) bir hepsi bir arada Arduino uyumlu zengin çevre birimleri ve algılayıcılar panosudur. Bunun için kullanarak geliştirebilirsiniz [Arduino için Visual Studio Code uzantısı](https://aka.ms/arduino). Ve büyüyen ile gelir [projeleri katalog](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) Microsoft Azure hizmetlerini yararlanmak prototip nesnelerin interneti (IOT) çözümleri size yol gösterecek.

>>>>>>> 3c7ee473158b1dd571d03cb27716afe08728885d
## <a name="before-you-begin"></a>Başlamadan önce

Bu öğreticide adımları tamamlamak için aşağıdakiler gerekir:

* İle DevKit hazırlama [Getting Started Guide](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started).
* En son bellenim yükseltin (> = 1.3.0) ile [bellenimi yükseltmek](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/) Öğreticisi.
* Oluşturun ve IOT Hub cihaz sağlama hizmet örneği ile bağlantı [otomatik sağlama yukarı ayarlamak](https://docs.microsoft.com/en-us/azure/iot-dps/quick-setup-auto-provision).

## <a name="set-up-the-device-provisioning-service-configuration-on-the-device"></a>Aygıttaki aygıt sağlama hizmet yapılandırmasını ayarlama

Oluşturduğunuz cihaz sağlama hizmeti örneğine bağlanmak DevKit etkinleştirmek için:

1. Azure portalında seçin **genel bakış** aygıt hizmeti sağlama ve Not **genel cihaz endpoint** ve **kimliği kapsam** değeri.
  ![DPS genel uç noktası ve kimliği kapsamı](./media/how-to-connect-mxchip-iot-devkit/dps-global-endpoint.png)

2. `git` uygulamasının makinenizde yüklü olduğundan ve komut penceresinden erişilebilir ortam değişkenlerine eklendiğinden emin olun. Bkz: [yazılım özgürlük Conservancy'nın Git istemci araçları](https://git-scm.com/download/) en son sürümü yüklü olması.

3. Bir komut istemi açın. DPS örnek kod için GitHub depoyu kopyalama:
  ```bash
  git clone https://github.com/DevKitExamples/DevKitDPS.git
  ```

4. VS Code'u başlatın ve DevKit bilgisayara bağlanın, kopyaladığınız kodu içeren klasörü açın.

5. Açık **DevKitDPS.ino**Bul ve Değiştir `[Global Device Endpoint]` ve `[ID Scope]` yalnızca not aşağı değerlere sahip.
  ![DPS Endpoint](./media/how-to-connect-mxchip-iot-devkit/endpoint.png) bırakabilirsiniz **RegistrationId** MAC adresi ve bellenim sürümlerine göre için boş uygulamanın aşağıdakilerden oluşturur. Özelleştirmek istiyorsanız, kayıt kimliği alfasayısal, küçük harf kullanın ve en fazla 128 karakter yalnızca birleşimlerine uzun kısa çizgi bulunur. Daha fazla bilgi için bkz: [Azure portal ile cihaz kayıtlarını yönetme](https://docs.microsoft.com/en-us/azure/iot-dps/how-to-manage-enrollments).

6. Kullanım **hızlı açık** VS code'da (Windows: `Ctrl+P`, macOS: `Cmd+P`) ve türü **görev aygıt karşıya yükleme** yapı ve kod DevKit karşıya yüklemek için.

7. Görev başarılı çıktı penceresinde inceleyin.

## <a name="save-unique-device-secret-on-stsafe-security-chip"></a>Benzersiz cihaz gizli STSAFE güvenlik yonga üzerinde Kaydet

Temel aygıttaki aygıt hizmeti sağlama yapılandırılabilir kendi [donanım güvenlik modülü (HSM)](https://azure.microsoft.com/en-us/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/). DevKit kullanan [cihaz kimliği oluşturma altyapısı (BÖLMEK)](https://trustedcomputinggroup.org/wp-content/uploads/Foundational-Trust-for-IOT-and-Resource-Constrained-Devices.pdf) gelen [Trusted Computing Group (TCG)](https://trustedcomputinggroup.org). A **benzersiz cihaz gizli anahtarı (UDS)** kaydedilen STSAFE güvenlik yongası DevKit üzerinde benzersiz cihaz oluşturmak için kullanılan [X.509](https://docs.microsoft.com/en-us/azure/iot-dps/tutorial-set-up-device#select-a-hardware-security-module) sertifika. Sertifika daha sonra cihaz sağlama hizmet kayıt işlemi için kullanılabilir.

Tipik bir **benzersiz cihaz gizli anahtarı (UDS)** 64 karakterden uzun bir dize değil. UDS olan farklı bir örnek aşağıda:

```
19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eae
```

Her iki karakter güvenlik hesaplama onaltılık değer olarak kullanılır. Bu nedenle yukarıdaki örnekte UDS çözümlenir: "`0x19`, `0xe2`, `0x5a`, `0x25`, `0x9d`, `0x0c`, `0x2b`, `0xe0`, `0x3a`, `0x02`, `0xd4`, `0x16` , `0xc0`, `0x5c`, `0x48`, `0xcc`, `0xd0`, `0xcc`, `0x7d`, `0x17`, `0x43`, `0x45`, `0x8a`, `0xae`, `0x1c`, `0xb4`, `0x88`, `0xb0`, `0x74`, `0x99`, `0x3e`, `0xae`".

Benzersiz cihaz gizli üzerinde DevKit kaydetmek için:

1. Putty gibi aracını kullanarak seri İzleyicisi'ni açmak için bkz [yapılandırma modu kullan](https://microsoft.github.io/azure-iot-developer-kit/docs/use-configuration-mode/) Ayrıntılar için.

2. Bilgisayara bağlı DevKit ile A düğmesini basılı tutun sonra push ve yapılandırma moduna girmek için Sıfırla düğmesini bırakın. Ekran DevKit kimliği göstermesi gerekir ve **'Configuration'**.

3. Yukarıdaki uzun örnek UDS dize almak ve bir veya daha çok karakter arasında diğer değerlerle değiştirin `0` ve `f`. Bu, kendi UDS kullanılır.

4. Seri İzleyicisi penceresinde yazın **set_dps_uds [your_own_uds_value]** ve kaydetmek için Enter tuşuna basın.
  > [!NOTE]
  > Örneğin, son iki karakter değiştirerek kendi UDS ayarlarsanız `f`, komutu gibi girmenize gerek **set_dps_uds 19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eff**.

5. Seri İzleyici penceresini kapatmadan DevKit düğmesine basın sıfırlayın.

6. Aşağı Not **DevKit MAC adresi** ve **DevKit bellenim sürümü** değeri.
  ![Bellenim sürümü](./media/how-to-connect-mxchip-iot-devkit/firmware-version.png)

## <a name="generate-x509-certificate"></a>X.509 sertifikası oluştur

### <a name="windows"></a>Windows

1. Açık dosya Gezgini ve klasöre gidin içeren DSP kopyaladığınız örnek kod, bir **.build** klasörü bulun ve kopyalama **DPS.ino.bin** ve **DPS.ino.map** da.
  ![Oluşturulan dosyalar](./media/how-to-connect-mxchip-iot-devkit/generated-files.png)
  > [!NOTE]
  > Değişip değişmediğini `built.path` Arduino yapılandırması için başka bir klasör. Yapılandırdığınız klasördeki dosyaları bulmak gerekir.

2. Bu iki dosyaya yapıştırın **Araçları** klasör ile aynı düzeyde **.build** klasör.

3. Çalıştırma **dps_cert_gen.exe**, girmek için istemleri izleyin, **UDS**, **MAC adresi** DevKit için ve **bellenim sürümü** oluşturmak için X.509 sertifikası.
  ![Run dps-cert-gen.exe](./media/how-to-connect-mxchip-iot-devkit/dps-cert-gen.png)

4. Nesil başarı gözlemlemek bir **.pem** sertifika aynı klasöre kaydedilir.

## <a name="create-a-device-enrollment-entry-in-the-device-provisioning-service"></a>Cihaz sağlama hizmetinde bir cihaz kayıt girişi oluşturma

1. Azure portalında sağlama hizmetinize gidin. **Kayıtları yönetme**'ye tıklayıp **Bireysel Kayıtlar** sekmesini seçin. ![Tek tek kayıtları](./media/how-to-connect-mxchip-iot-devkit/individual-enrollments.png)

2. **Ekle**'ye tıklayın.

3. İçinde **mekanizması**, seçin **X.509**.
  ![Sertifika karşıya yükle](./media/how-to-connect-mxchip-iot-devkit/upload-cert.png)

4. İçinde **sertifika .pem veya .cer dosyasını**, karşıya yükleme **.pem** yalnızca sertifika.

5. Geri kalan varsayılan olarak bırakın ve tıklayın **kaydetmek**.

## <a name="start-the-devkit"></a>DevKit Başlat

1. VS Code'u başlatın ve seri İzleyicisi'ni açın.

2. Tuşuna **sıfırlama** , DevKit düğmesinde.

Cihaz sağlama hizmetiniz ile kayıt Başlat DevKit görmeniz gerekir.

![VS kod çıkışı](./media/how-to-connect-mxchip-iot-devkit/vscode-output.png)

## <a name="verify-the-devkit-is-registered-on-iot-hub"></a>DevKit IOT hub'da kayıtlı doğrulayın

Bir kez, cihaz önyüklemesinde, aşağıdaki eylemler gerçekleşir:

1. Aygıtın aygıt sağlama hizmetinize bir kayıt isteği gönderir.
2. Aygıt hizmeti sağlama Cihazınızı yanıt vereceği bir kayıt sınama geri gönderir.
3. Başarılı kaydı üzerinde aygıt hizmeti sağlama IOT hub'ı URI, cihaz kimliği ve şifrelenmiş anahtar cihaza geri gönderir.
4. IOT Hub istemci uygulaması cihazda ardından hub'ınıza bağlanır.
5. Hub'ına başarılı bağlantı üzerine IOT hub'ın aygıt Explorer'da görünen aygıt görmeniz gerekir.
  ![Kayıtlı cihaz](./media/how-to-connect-mxchip-iot-devkit/device-registered.png)

## <a name="change-device-id"></a>Değişiklik cihaz kimliği

Azure IOT hub'da kayıtlı varsayılan cihaz kimliği **AZ3166**. Bunu değiştirmek istiyorsanız, izleyin [yönergeleri burada](https://microsoft.github.io/azure-iot-developer-kit/docs/customize-device-id/).

## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

Sorunlarla karşılaşırsanız, başvurmak [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) veya aşağıdaki kanaldan bize ulaşın:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki adımlar

Göre otomatik olarak sıfır dokunarak IOT Hub'ına kaydedebilirsiniz güvenli bir şekilde BÖLMEK, kullanarak dağıtım noktaları için bir aygıt kaydetmeye DevKit hazırlamak öğrendiniz. Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Cihazda DPS genel uç noktasını yapılandırın
> * X.509 sertifikası oluşturmak için benzersiz cihaz gizli anahtarı (UDS) kullanın
> * Bir cihaz kaydetme
> * Cihaz kayıtlı doğrulayın

Bilgi edinmek için diğer öğreticileri için ilerleyin:

> [!div class="nextstepaction"]
> [Oluşturma ve bir sanal cihaz sağlama](./quick-create-simulated-device.md)

