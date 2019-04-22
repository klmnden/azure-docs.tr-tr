---
title: Cihaz bağlantısı, Azure IOT Central | Microsoft Docs
description: Bu makalede Azure IOT Central, cihaz bağlantısı ile ilgili temel kavramlar tanıtılmaktadır.
author: dominicbetts
ms.author: dobett
ms.date: 04/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: 9e1e85d1ab1c5e7ce0cbd96c64137309c2e2916a
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59425976"
---
# <a name="device-connectivity-in-azure-iot-central"></a>Azure IOT Central, cihaz bağlantısı

Bu makalede, Microsoft Azure IOT Central, cihaz bağlantısı ile ilgili temel kavramlar tanıtılmaktadır.

Azure IOT Central'ı kullanan [Azure IOT Hub cihazı sağlama hizmeti (DPS)](https://docs.microsoft.com/azure/iot-dps/about-iot-dps) tüm cihaz kaydı ve bağlantı yönetmek için.

DPS kullanarak sağlar:

- IOT Central ekleme ve uygun ölçekte bağlanan cihazların desteklemek için.
- Cihaz oluşturmanız için kimlik bilgilerini ve IOT Central UI aracılığıyla cihaz kayıt olmadan cihazları çevrimdışı yapılandırma.
- Paylaşılan erişim imzaları (SAS) kullanarak bağlanmak için cihazlar.
- Endüstri standardı X.509 sertifikaları kullanarak bağlanmak için cihazlar.
- Kendi cihazını kimlikleri IOT Central cihazlarını kaydetmek için kullanılacak. Kendi cihaz kimlikleri kullanarak mevcut arka ofis sistemleriyle tümleştirmeyi basitleştirir.
- Cihazları IOT Central bağlamak için tek, tutarlı şekilde.

Bu makalede aşağıdaki dört kullanım örnekleri:

1. [SAS kullanarak tek bir cihazı hızlı bir şekilde bağlanın](#connect-a-single-device)
1. [Cihazları uygun ölçekte SAS kullanarak bağlanma](#connect-devices-at-scale-using-sas)
1. [Cihazları uygun ölçekte X.509 sertifikaları kullanarak bağlanma](#connect-devices-using-x509-certificates) bu üretim ortamları için önerilen yaklaşımdır.
1. [İlk kayıt cihazları bağlayın](#connect-without-registering-devices)

## <a name="connect-a-single-device"></a>Tek bir cihazı bağlayın

Bu yaklaşım, IOT Central ile denemeler ya da cihazları test yararlıdır. Bir cihaz için bağlantı dizesi oluşturmak için IOT Central uygulamanızdan cihaz bağlantı bilgisini kullanın. Ayrıntılı adımlar için bkz. [bir Azure IOT Central uygulamasına bağlanmak için bir cihaz bağlantı dizesi oluşturmak nasıl](howto-generate-connection-string.md).

## <a name="connect-devices-at-scale-using-sas"></a>Cihazları uygun ölçekte SAS kullanarak bağlanma

SAS kullanarak uygun ölçekte IOT Central aygıtlarını bağlamak için kaydetme ve ardından cihazları ayarlamak gerekir:

### <a name="register-devices-in-bulk"></a>Cihazları toplu kaydetme

Çok sayıda cihaz ile IOT Central uygulamanızı kaydetmek için bir CSV dosyasına kullanın [cihaz kimlikleri ve cihaz adları içeri aktarma](howto-manage-devices.md#import-devices).

İçeri aktarılan cihazlar için bağlantı bilgilerini almak için [IOT Central uygulamanızdan bir CSV dosyasını dışarı](howto-manage-devices.md#export-devices).

> [!NOTE]
> İlk bunları IOT Central kayıt olmadan cihazları nasıl bağlayabileceğini bilgi edinmek için [ilk kayıt cihazları olmadan Bağlan](#connect-without-registering-devices).

### <a name="set-up-your-devices"></a>Cihazları ayarlama

Bağlanmak ve verileri için IOT, IOT Central uygulamasına göndermek için cihazlarınızı etkinleştirmek için cihaz kodunuzda dışarı aktarma dosyasından bağlantı bilgilerini kullanın. Cihazlar bağlama hakkında daha fazla bilgi için bkz. [sonraki adımlar](#next-steps).

## <a name="connect-devices-using-x509-certificates"></a>X.509 sertifikaları kullanarak cihazları bağlayın

Bir üretim ortamında, X.509 sertifikaları IOT Central için önerilen cihaz kimlik doğrulama mekanizması kullanmaktır. Daha fazla bilgi için bkz. [cihaz X.509 CA sertifikalarını kullanarak kimlik doğrulaması](../iot-hub/iot-hub-x509ca-overview.md).

Aşağıdaki adımlar, X.509 sertifikaları kullanarak IOT Central için cihazların bağlanacağı açıklanır:

1. IOT Central uygulamanızdaki _ekleyebilir ve Ara doğrulayın ya da X.509 sertifikası kök_ cihaz sertifikalarını oluşturmak için kullanıyorsanız:

    - Gidin **Yönetim > cihaz bağlantısı > Sertifikalar (X.509)** ve X.509 kök veya Ara sertifikayı yaprak cihaz sertifikalarını oluşturmak için kullandığınız ekleyin.

      ![Bağlantı ayarları](media/concepts-connectivity/connection-settings.png)

      Bir güvenlik ihlali varsa veya birincil sertifikanızın süresi dolacak şekilde ayarlanan kapalı kalma süresini azaltmak için ikincil sertifika'yı kullanın. Birincil sertifika güncelleştirilirken ikincil sertifika kullanarak cihaz sağlamayı devam edebilirsiniz.

    - Sertifika sahipliğini doğrulayarak sertifikanın uploader sertifikanın özel anahtarı sahip olmasını sağlar. Sertifikayı doğrulamak için:
        - Düğmeyi seçin **doğrulama kodu** bir kod oluşturmak için.
        - Önceki adımda oluşturulan doğrulama kodunu içeren bir X.509 doğrulama sertifikası oluşturun. Sertifikayı bir .cer dosyası olarak kaydedin.
        - İmzalı doğrulama sertifikasını karşıya yükleyin ve seçin **doğrulama**.

          ![Bağlantı ayarları](media/concepts-connectivity/verify-cert.png)

1. İçin bir CSV dosyası kullanma _almak ve cihazları kaydetme_ IOT Central uygulamanızdaki.

1. _Cihazlarınızı ayarlayın._ Karşıya yüklenen bir kök sertifikayı kullanarak yaprak sertifikalar oluşturur. Kullanım **cihaz kimliği** yaprak sertifikalar CNAME değeri. Cihaz kimliği, tamamen küçük olmalıdır. Ardından hizmet bilgisi sağlama ile cihazlarınızı program. Bir cihaz ilk açıldığında DPS IOT Central uygulamanız için bağlantı bilgilerini alır.

### <a name="further-reference"></a>Daha fazla başvuru

- Örnek uygulama için [RaspberryPi.](https://aka.ms/iotcentral-docs-Raspi-releases)

- [C'de, örnek cihaz istemcisi](https://github.com/Azure/azure-iot-sdk-c/blob/dps_symm_key/provisioning_client/devdoc/using_provisioning_client.md)

### <a name="for-testing-purposes-only"></a>Yalnızca test amaçlı

Yalnızca test için bu yardımcı programlar CA sertifikaları ve cihaz sertifikaları oluşturmak için kullanabilirsiniz.

- Bir DevKit cihaz kullanıyorsanız, bu [komut satırı aracı](https://aka.ms/iotcentral-docs-dicetool) sertifikaları doğrulamak için IOT Central uygulamanıza ekleyebileceğiniz bir CA sertifikası oluşturur.

- Bunu kullanın [komut satırı aracı](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md ) için:
  - Bir sertifika zinciri oluşturun. 2. adım GitHub makaleyi izleyin.
  - Sertifikalar, IOT Central uygulamanızı karşıya yüklenecek .cer dosyası olarak kaydedin.
  - Doğrulama sertifikası oluşturmak için IOT Central uygulamasına ilişkin doğrulama kodunu kullanın. 3. adım GitHub makaleyi izleyin.
  - Aracı için parametre olarak, cihaz kimliklerini kullanarak cihazlarınızı yaprak sertifikaları oluşturun. 4. adım GitHub makaleyi izleyin.

## <a name="connect-without-registering-devices"></a>Cihazları kaydetme olmadan bağlayın

IOT Central sağlayan bir anahtar OEM'ler ilk olmadan bir IOT Central uygulamasına cihazlarını toplu üretmek için senaryodur kaydediliyor. Bir üretici uygun kimlik bilgilerini oluşturmak ve cihazları Fabrika yapılandırmanız gerekir. Bir cihaz ilk kez açtığında bir IOT Central uygulamasına otomatik olarak bağlanır. Önce stat veri gönderimi IOT Central operatörün cihaz onaylamanız gerekir.

Aşağıdaki diyagramda, bu akış özetlenmektedir:

![Bağlantı ayarları](media/concepts-connectivity/device-connection-flow.png)

Aşağıdaki adımlarda, bu işlem daha ayrıntılı açıklanmıştır. Adımları olup olmadığını, SAS veya X.509 sertifikaları için cihaz kimlik doğrulamasını kullanıyorsanız bağlı olarak biraz farklılık gösterir:

1. Bağlantı ayarlarını yapılandırın:

    - **X.509 sertifikaları:** [Ekleme ve doğrulama kök/Ara sertifikayı](#connect-devices-using-x509-certificates) ve aşağıdaki adımda cihaz sertifikaları oluşturmak için kullanın.
    - **SAS:** Birincil anahtarı kopyalayın. IOT Central uygulaması için Grup SAS anahtarını anahtardır. Aşağıdaki adımda cihaz SAS anahtarları oluşturmak için anahtarı kullanın.
    ![SAS bağlantı ayarları](media/concepts-connectivity/connection-settings-sas.png)

1. Cihaz kimlik bilgileri oluştur
    - **X.509 sertifikaları:** Kök veya Ara sertifikayı IOT Central uygulamanıza eklediğiniz kullanarak cihazlarınız için yaprak sertifikalar oluşturur. Küçük harf kullandığınızdan emin olun **cihaz kimliği** CNAME yaprak sertifikalar olarak. Sınama amacıyla yalnızca, bu kullanım için [komut satırı aracı](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md ) cihaz sertifikaları oluşturulacak.
    - **SAS:** Bunu kullanın [komut satırı aracını](https://www.npmjs.com/package/dps-keygen) cihaz SAS anahtarları oluşturmak için. Gruplandırma **birincil anahtar** önceki adımdan. Cihaz kimliği küçük harf olması gerekir.

      Yüklenecek [tuşu Oluşturucu yardımcı programı](https://github.com/Azure/dps-keygen), aşağıdaki komutu çalıştırın:

      ```cmd/sh
      npm i -g dps-keygen
      ```

      Bir cihaz anahtarı grubu SAS birincil anahtarını oluşturmak için aşağıdaki komutu çalıştırın:

      ```cmd/sh
      dps-keygen -mk:<Primary_Key(GroupSAS)> -di:<device_id>
      ```

1. Cihazları ayarlamak için her bir cihaza flash **kapsam kimliği**, **cihaz kimliği**, ve **X.509 cihazı sertifikası** veya **SAS anahtarı**.

1. Ardından, IOT Central uygulamasına bağlanmak için cihazı açın. Bir cihaza geçtiğinizde, ilk IOT Central kayıt bilgileri almak için DPS bağlanır.

1. Bağlı cihaz ilk olarak görünür bir **ilişkilendirilmemiş cihaz** üzerinde **Device Explorer** sayfası. Sağlama durumu cihaz **kayıtlı**. **İlişkilendirme** uygun cihaz şablonu cihaza ve cihazı IOT Central uygulamanızı bağlamak için onaylayın. Cihaz IOT Hub'ından bir bağlantı dizesi alma ve veri göndermeye başlayın. Cihaz sağlama, artık tamamlandı ve sağlama durumunu şimdi **sağlanan**.

## <a name="provisioning-status"></a>Sağlama durumu

Ne zaman gerçek bir cihaz sağlama durumu değişiklikleri, IOT Central uygulaması'na aşağıdaki gibi bağlanır:

1. Cihaz durumu sağlama ilk **kayıtlı**. Cihaz IOT Central içinde oluşturulur ve bir cihaz kimliği vardır. Bu durum anlamına gelir Bir cihazın kayıtlı olduğunda:
    - Yeni bir gerçek cihaz üzerinde eklenir **Device Explorer** sayfası.
    - Bir cihaz kümesini kullanarak eklendiğinden **alma** üzerinde **Device Explorer** sayfası.
    - Bir cihaz üzerinde el ile kayıtlı olmayan **Device Explorer** sayfasında, ancak geçerli kimlik bilgileriyle bağlı ve olarak görünür bir **Unassociated** cihazda **Device Explorer**sayfası.

1. Cihaz sağlama durumu değişikliklerini **sağlanan** sağlama adım tamamlandığında, geçerli kimlik bilgileri ile IOT Central uygulamasına bağlı cihaz. Bu adımda, cihaz IOT Hub'ından bir bağlantı dizesi alır. Cihaz artık IOT Hub'ına bağlanmak ve veri göndermeye başlayın.

1. Bir işleç bir cihaz engelleyebilirsiniz. Bir cihaz engellendiğinde, IOT Central uygulamanıza veri gönderemezsiniz. Engellenen cihazlar sağlama durumuna sahip **bloke**. Veri göndermeye devam etmeden önce operatörün cihaz sıfırlamanız gerekir. Bir cihaz sağlama durumunu döndürür önceki değerine, operatörün engellemesinin kaldırıldığı zaman **kayıtlı** veya **sağlanan**.

## <a name="sdk-support"></a>SDK desteği

Sizin için en kolay yolu Azure cihaz SDK'ları teklif cihazınızın kodunu uygulayın. Aşağıdaki cihaz SDK'ları kullanılabilir:

- [C için Azure IOT SDK](https://github.com/azure/azure-iot-sdk-c)
- [Python için Azure IOT SDK](https://github.com/azure/azure-iot-sdk-python)
- [Azure IOT SDK'sı için Node.js](https://github.com/azure/azure-iot-sdk-node)
- [Java için Azure IOT SDK](https://github.com/azure/azure-iot-sdk-java)
- [.NET için Azure IOT SDK](https://github.com/azure/azure-iot-sdk-csharp)

Her cihaz, cihaz tanımlayan benzersiz bir bağlantı dizesi kullanarak bağlanır. Bir cihaz, yalnızca, kayıtlı olduğu IOT hub'ına bağlanabilir. Azure IOT Central uygulamanızda gerçek bir cihaz oluşturduğunuzda, bağlantı dizesini kullanarak oluşturmak ihtiyacınız olan bilgileri uygulamanın oluşturduğu `dps-keygen`.

### <a name="sdk-features-and-iot-hub-connectivity"></a>SDK özelliklerinin ve IOT Hub bağlantı

Tüm cihaz iletişimi IOT Hub ile IOT Hub bağlantı şunlardan kullanır:

- [CİHAZDAN buluta ileti gönderme](../iot-hub/iot-hub-devguide-messages-d2c.md)
- [Cihaz ikizlerini](../iot-hub/iot-hub-devguide-device-twins.md)

Azure IOT Central cihaz özellikleri açın IOT hub'ı özelliklerinden nasıl eşleştiği aşağıdaki tabloda özetlenmiştir:

| Azure IoT Central | Azure IoT Hub |
| ----------- | ------- |
| Ölçüm: Telemetri | CİHAZDAN buluta ileti gönderme |
| Cihaz özellikleri | Özellikler cihaz çiftinin bildirilen |
| Ayarlar | Cihaz ikizi istenen ve bildirilen özellikler |

Cihaz SDK'ları kullanma hakkında daha fazla bilgi edinmek için aşağıdaki makalelerden birine örnek kod için bkz:

- [Genel bir Node.js istemcisini Azure IoT Central uygulamanıza bağlama](howto-connect-nodejs.md)
- [Azure IOT Central uygulamanıza bir Raspberry Pi cihazı bağlayın](howto-connect-raspberry-pi-python.md)
- [Azure IOT Central uygulamanıza Devdiv'e Seti cihaz bağlayamama](howto-connect-devkit.md).

### <a name="protocols"></a>Protokoller

Cihaz SDK'ları, bir IOT hub'ına bağlamak için aşağıdaki ağ protokollerini destekler:

- MQTT
- AMQP
- HTTPS

Bir seçme bu fark protokolleri ve yönergeleri hakkında bilgi için [iletişim protokolü seçme](../iot-hub/iot-hub-devguide-protocols.md).

Cihazınızı desteklenen protokollerinin hiçbirini kullanamıyorsanız, Azure IOT Edge, dönüştürme protokolü için de kullanabilirsiniz. IOT Edge, işleme, Azure IOT Central uygulamadan uca boşaltmak için diğer zeka-üzerinde--edge senaryolarını destekler.

## <a name="security"></a>Güvenlik

Cihazlar ile Azure IOT Central arasında alınıp verilen tüm veriler şifrelenir. IOT Hub cihaz bakan IOT Hub uç noktaları birine bağlayan bir CİHAZDAN her isteğin kimliğini doğrular. Kimlik bilgileri kablo üzerinden değişimi önlemek için bir cihaz kimlik doğrulaması için imzalanmış belirteçleri kullanır. Daha fazla bilgi için bkz: [IOT hub'a erişimi denetleme](../iot-hub/iot-hub-devguide-security.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Central, cihaz bağlantısı hakkında öğrendiniz, önerilen sonraki adımlar şunlardır:

- [Hazırlama ve DevKit cihazı bağlayın](howto-connect-devkit.md)
- [Raspberry Pi'yi hazırlama ve bağlama](howto-connect-raspberry-pi-python.md)
- [Genel bir Node.js istemcisini Azure IoT Central uygulamanıza bağlama](howto-connect-nodejs.md)
- [C SDK'SI: Cihaz istemci SDK'sı sağlama](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_provisioning_client.md)
