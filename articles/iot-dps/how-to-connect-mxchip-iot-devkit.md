---
title: Azure IOT Hub cihaz hizmeti sağlama otomatik sağlama MXChip IOT DevKit IOT Hub'ınızla kaydolmak için nasıl kullanılacağını | Microsoft Docs
description: Azure IOT Hub cihaz hizmeti sağlama otomatik sağlama MXChip IOT DevKit IOT Hub'ınızla kaydolmak için nasıl kullanılacağını.
services: iot-dps
keywords: ''
author: liydu
ms.author: liydu
ms.date: 04/04/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 7fe1cd003bd7e6b681989324a42a076f4fd2f7df
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="use-azure-iot-hub-device-provisioning-service-auto-provisioning-to-register-the-mxchip-iot-devkit-with-iot-hub"></a>Azure IOT Hub cihaz hizmeti sağlama otomatik sağlama MXChip IOT DevKit IOT Hub'ınızla kaydolmak için kullanın

Bu makalede, Azure IOT Hub cihaz sağlama hizmeti kullanmayı açıklar [otomatik sağlama](concepts-auto-provisioning.md)MXChip IOT DevKit Azure IOT Hub'ınızla kaydolmak için. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

* Bir cihazda hizmet sağlama cihazın genel uç noktasını yapılandırın.
* Bir X.509 sertifikası oluşturmak için bir benzersiz cihaz gizlilik (UDS) kullanın.
* Tek bir cihaza kaydedin.
* Cihaz kayıtlı olduğunu doğrulayın.

[MXChip IOT DevKit](https://aka.ms/iot-devkit) bir zengin çevre birimleri ve algılayıcılar hepsi bir arada Arduino uyumlu panosudur. Bunun için kullanarak geliştirmek [Arduino için Visual Studio Code uzantısı](https://aka.ms/arduino). Büyüyen ile DevKit gelen [projeleri katalog](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) Azure Hizmetleri yararlanmak prototip nesnelerin interneti (IOT) çözümlerinizi kılavuzluk edecek.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğreticide adımları tamamlamak için önce aşağıdaki görevleri yapın:

* İçindeki adımları izleyerek, DevKit hazırlama [IOT DevKit AZ3166 bulutta Azure IOT Hub'ına bağlanmak](/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started).
* En son üretici yazılımına yükseltin (1.3.0 veya sonrası) ile [güncelleştirme DevKit bellenim](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/) Öğreticisi.
* Oluşturun ve IOT hub'ı içindeki adımları izleyerek hizmet örneği sağlama aygıtıyla bağlantı [IOT Hub cihaz sağlama hizmeti Azure portal ile ayarlama](/azure/iot-dps/quick-setup-auto-provision).

## <a name="build-and-deploy-auto-provisioning-registration-software-to-the-device"></a>Derleme ve aygıta otomatik sağlama kayıt yazılım dağıtma

Oluşturduğunuz hizmet örneği sağlama cihaza DevKit bağlanmak için:

1. Azure portalında seçin **genel bakış** aşağı sağlama hizmeti ve Not Cihazınızı bölmesinde **genel cihaz endpoint** ve **kimliği kapsam** değerleri.
  ![DPS genel uç noktası ve kimliği kapsamı](./media/how-to-connect-mxchip-iot-devkit/dps-global-endpoint.png)

2. Olduğundan emin olun `git` makinenize yüklü ve ortam değişkenleri komut penceresine erişilebilir eklenen. Bkz: [yazılım özgürlük Conservancy'nın Git istemci araçları](https://git-scm.com/download/) en son sürümü yüklü olması.

3. Bir komut istemi açın. Cihaz hizmeti örnek kod sağlama için GitHub depoyu kopyalama:
  ```bash
  git clone https://github.com/DevKitExamples/DevKitDPS.git
  ```

4. Visual Studio Code açın, DevKit bilgisayarınıza bağlayın ve sonra kopyaladığınız kodu içeren klasörü açın.

5. Açık **DevKitDPS.ino**. Bulma ve değiştirme `[Global Device Endpoint]` ve `[ID Scope]` sadece ettiğiniz aşağı değerlere sahip.
  ![DPS Endpoint](./media/how-to-connect-mxchip-iot-devkit/endpoint.png) bırakabilirsiniz **RegistrationId** boş. Uygulama, MAC adresi ve bellenim sürümlerine göre için bir tane oluşturur. Kayıt Kimliği özelleştirmek istiyorsanız, yalnızca alfasayısal, küçük harf kullanın ve birleşimleri en çok 128 karakterden kısa çizgi gerekir. Daha fazla bilgi için bkz: [Azure portal ile cihaz kayıtlarını yönetme](https://docs.microsoft.com/azure/iot-dps/how-to-manage-enrollments).

6. VS code'da hızlı açık kullanın (Windows: `Ctrl+P`, macOS: `Cmd+P`) ve türü *görev aygıt karşıya yükleme* yapı ve kod DevKit karşıya yüklemek için.

7. Çıktı penceresi görev başarılı olup olmadığını gösterir.

## <a name="save-a-unique-device-secret-on-an-stsafe-security-chip"></a>Benzersiz cihaz gizli bir STSAFE güvenlik yonga Kaydet

Cihazın üzerinde dayalı bir cihazda otomatik sağlama yapılandırılabilir [kanıtlama mekanizması](concepts-security.md#attestation-mechanism). MXChip IOT DevKit kullanan [aygıt kimlik bileşim motoru](https://trustedcomputinggroup.org/wp-content/uploads/Foundational-Trust-for-IOT-and-Resource-Constrained-Devices.pdf) gelen [Trusted Computing Group](https://trustedcomputinggroup.org). A *benzersiz cihaz gizli* (UDS) kaydedilen bir STSAFE güvenlik yongası DevKit üzerinde üretmek için kullanılan aygıt benzersiz [X.509 sertifikası](concepts-security.md#x509-certificates). Sertifika daha sonra cihaz hizmet sağlama ve çalışma zamanında kayıt sırasında kayıt işlemi için kullanılır.

Aşağıdaki örnekte görüldüğü gibi tipik benzersiz cihaz gizli 64 karakterli bir dize olmadığından:

```
19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eae
```

Her iki karakter güvenlik hesaplama onaltılık değer olarak kullanılır. UDS Çözüldü önceki örnek: `0x19`, `0xe2`, `0x5a`, `0x25`, `0x9d`, `0x0c`, `0x2b`, `0xe0`, `0x3a`, `0x02`, `0xd4`, `0x16` , `0xc0`, `0x5c`, `0x48`, `0xcc`, `0xd0`, `0xcc`, `0x7d`, `0x17`, `0x43`, `0x45`, `0x8a`, `0xae`, `0x1c`, `0xb4`, `0x88`, `0xb0`, `0x74`, `0x99`, `0x3e`, `0xae`.

Benzersiz cihaz gizli anahtarı üzerinde DevKit kaydetmek için:

1. Putty gibi bir araç kullanarak seri İzleyicisi'ni açın. Bkz: [yapılandırma modu kullan](https://microsoft.github.io/azure-iot-developer-kit/docs/use-configuration-mode/) Ayrıntılar için.

2. Bilgisayarınıza bağlı DevKit ile basılı **A** button, tuşuna basın ve yayın **sıfırlama** yapılandırma moduna düğmesi. Ekran DevKit kimliği ve yapılandırma gösterilmektedir.

3. UDS dize ve bir veya daha fazla karakter arasında diğer değerlerle değiştirin örnek ele `0` ve `f` kendi UDS için.

4. Seri İzleyicisi penceresinde yazın *set_dps_uds [your_own_uds_value]* Enter seçin.
  > [!NOTE]
  > Örneğin, son iki karakter değiştirerek kendi UDS ayarlarsanız `f`, bu komutu şu şekilde girmeniz gerekir: `set_dps_uds 19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eff`.

5. Seri İzleyici penceresini kapatmadan basın **sıfırlama** DevKit düğmesinde.

6. Aşağı Not **DevKit MAC adresi** ve **DevKit bellenim sürümü** değerleri.
  ![Bellenim sürümü](./media/how-to-connect-mxchip-iot-devkit/firmware-version.png)

## <a name="generate-an-x509-certificate"></a>Bir X.509 sertifikası oluştur

### <a name="windows"></a>Windows

1. Dosya Gezgini'ni açın ve daha önce kopyaladığınız hizmeti örnek kod sağlama aygıtı içeren klasöre gidin. İçinde **.build** klasörü bulun ve kopyalama **DPS.ino.bin** ve **DPS.ino.map** klasörüne kodunu içerir.
  ![Oluşturulan dosyalar](./media/how-to-connect-mxchip-iot-devkit/generated-files.png)
  > [!NOTE]
  > Değiştirdiyseniz `built.path` Arduino yapılandırma başka bir klasöre, yapılandırdığınız klasördeki dosyaları bulmak vermeniz gerekir.

2. Bu iki dosyaya yapıştırın **Araçları** klasör ile aynı düzeyde **.build** klasör.

3. Çalıştırma **dps_cert_gen.exe**. Girmek için istemleri izleyin, **UDS**, **MAC adresi** DevKit için ve **bellenim sürümü** X.509 sertifikası oluşturmak için.
  ![DPS cert gen.exe çalıştırın](./media/how-to-connect-mxchip-iot-devkit/dps-cert-gen.png)

4. X.509 sertifikası oluşturulduktan sonra bir **.pem** sertifika aynı klasöre kaydedilir.

## <a name="create-a-device-enrollment-entry-in-the-device-provisioning-service"></a>Hizmet sağlama cihazı bir cihaz kayıt girişi oluşturun

1. Azure portalında aygıtı sağlama hizmeti örneğine gidin. Seçin **kayıtlarını yönetme**ve ardından **tek tek kayıtları** sekmesi. ![Tek tek kayıtları](./media/how-to-connect-mxchip-iot-devkit/individual-enrollments.png)

2. **Add (Ekle)** seçeneğini belirleyin.

3. "Ekle kaydı" panelde:
   - seçin **X.509** altında **mekanizması**
   - "dosya" altında Seç **birincil sertifika .pem veya .cer dosyası**
   - Dosya Aç iletişim kutusunda gidin ve karşıya **.pem** yeni oluşturulan sertifika
   - geri kalan varsayılan olarak bırakın ve tıklayın **Kaydet**

   ![Sertifikayı karşıya yükleme](./media/how-to-connect-mxchip-iot-devkit/upload-cert.png)

## <a name="start-the-devkit"></a>DevKit Başlat

1. VS Code ve seri İzleyicisi'ni açın.

2. Tuşuna **sıfırlama** , DevKit düğmesinde.

Cihaz sağlama hizmeti ile kayıt DevKit başlangıç bakın.

![VS kod çıkışı](./media/how-to-connect-mxchip-iot-devkit/vscode-output.png)

## <a name="verify-that-the-devkit-is-registered-with-azure-iot-hub"></a>DevKit Azure IOT Hub ile kaydedildiğini doğrulayın

Aygıt başlatıldıktan sonra aşağıdaki eylemler gerçekleşir:

1. Cihaz hizmet sağlama Cihazınızı bir kayıt isteği gönderir.
2. Cihaz hizmet sağlama Cihazınızı yanıt vereceği bir kayıt sınama geri gönderir.
3. Başarılı kaydı üzerinde hizmet sağlama cihaz IOT Hub URI, cihaz kimliği ve şifrelenmiş anahtar geri cihaza gönderir.
4. Cihaz IOT Hub istemci uygulamasında hub'ınıza bağlanır.
5. Hub'ına başarılı bağlantı üzerine IOT Hub cihaz Explorer'da görünen aygıt bakın.
  ![Kayıtlı cihaz](./media/how-to-connect-mxchip-iot-devkit/device-registered.png)

## <a name="change-the-device-id"></a>Cihaz Kimliğini değiştirin

Azure IOT Hub ile kayıtlı varsayılan cihaz kimliği *AZ3166*. Kimliği değiştirmek istiyorsanız,'ndaki yönergeleri izleyin [cihaz kimliği özelleştirme](https://microsoft.github.io/azure-iot-developer-kit/docs/customize-device-id/).

## <a name="problems-and-feedback"></a>Sorunları ve geri bildirim

Sorunlarla karşılaşırsanız, IOT DevKit başvuran [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/), ya da aşağıdaki kanallar desteği ulaşmak:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir cihazın güvenli bir şekilde böylece cihaz Azure IOT Hub ile otomatik olarak kaydedebilirsiniz hizmeti cihaz kimliği oluşturma altyapısını kullanarak sağlama aygıtına öğrendiniz. 

Özet olarak, öğrenilen nasıl yapılır:

> [!div class="checklist"]
> * Bir cihazda hizmet sağlama cihazın genel uç noktasını yapılandırın.
> * Benzersiz cihaz gizli bir X.509 sertifikası oluşturmak için kullanın.
> * Tek bir cihaza kaydedin.
> * Cihaz kayıtlı olduğunu doğrulayın.

Bilgi edinmek için nasıl [oluşturma ve sağlama bir sanal cihaz](./quick-create-simulated-device.md).

