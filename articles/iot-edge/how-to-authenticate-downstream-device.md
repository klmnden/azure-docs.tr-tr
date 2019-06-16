---
title: Aşağı Akış cihazları - Azure IOT Edge kimlik doğrulaması | Microsoft Docs
description: Aşağı Akış cihazları veya yaprak cihazlarıyla IOT hub'ına kimlik doğrulaması ve Azure IOT Edge ağ geçidi cihazları aracılığıyla kendi bağlantı yönlendirmek üzere nasıl.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/07/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 5785b0260474bd0eb861236a0bd78066475baacd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67082397"
---
# <a name="authenticate-a-downstream-device-to-azure-iot-hub"></a>Azure IOT hub'a bir aşağı akış cihaz kimlik doğrulaması

Saydam bir ağ geçidi senaryosunda, aşağı akış cihazları (yaprak cihazlar veya alt cihazlar olarak da adlandırılır) gibi başka bir cihaz IOT Hub kimliklerini gerekir. Bu makalede, IOT hub'ına bir aşağı akış cihaz kimliğini doğrulamak için seçenekleri açıklar ve sonra ağ geçidi bağlantısının nasıl gösterir.

Başarılı saydam bir ağ geçidi bağlantısı kurmak için üç genel adımlar vardır. Bu makalede, ikinci adım yer almaktadır:

1. Ağ geçidi cihazı, güvenli bir aşağı akış cihazlara bağlanın, aşağı akış cihazlardan iletişimleri almak ve iletileri yönlendirmek için uygun hedef olması gerekiyor. Daha fazla bilgi için [saydam bir ağ geçidi olarak görev yapacak bir IOT Edge cihazı yapılandırma](how-to-create-transparent-gateway.md).
2. **Aşağı Akış cihaz IOT Hub ile kimlik doğrulaması ve kendi ağ geçidi cihazı iletişim kurmak için bilmeniz yapabilmek için bir cihaz kimliği olmalıdır.**
3. Aşağı Akış cihaz, ağ geçidi cihazı için güvenli bir şekilde bağlanabilmesi gerekir. Daha fazla bilgi için [bir Azure IOT Edge ağ geçidi için aşağı akış cihaz Bağlayamama](how-to-connect-downstream-device.md).

Aşağı Akış cihazlar IOT Hub üç yöntemden birini kullanarak kimlik doğrulaması: simetrik anahtarlar (bazen paylaşılan erişim anahtarları olarak adlandırılır), X.509 Kendinden imzalanan sertifikalar veya X.509 sertifika yetkilisi (CA) sertifikaları imzalanmış. Kimlik doğrulama adımları, tüm IOT Hub ile IOT Edge cihaz ile ağ geçidi ilişkisi bildirmek için küçük farklılıklar ayarlamak için kullanılan adımlara benzerdir.

Bu makaledeki adımları el ile cihaz sağlama, Azure IOT Hub cihazı sağlama hizmeti ile sağlama otomatik göster değil. 

## <a name="symmetric-key-authentication"></a>Simetrik anahtar kimlik doğrulaması

Simetrik anahtar kimlik doğrulaması veya paylaşılan erişim anahtarı kimlik doğrulaması, IOT Hub ile kimlik doğrulaması için en basit yoludur. Simetrik anahtar kimlik doğrulaması ile bir base64 anahtar IOT hub, IOT cihaz kimliği ile ilişkilidir. Böylece IOT hub'a bağlandığında cihazınızın bunu sunabilir IOT uygulamalarınızı bu anahtarı içerir. 

### <a name="create-the-device-identity"></a>Cihaz kimliği oluşturma 

Visual Studio Code için Azure portal'ı, Azure CLI'ı veya IOT uzantısını kullanarak IOT hub'ınızda, yeni bir IOT cihazı ekleyin. Aşağı Akış cihazlar IOT Hub, IOT Edge cihazları normal IOT cihazı olarak tanımlanması gerektiğini unutmayın. 

Yeni cihaz kimliği oluşturma, aşağıdaki bilgileri sağlayın: 

* Cihazınız için bir kimlik oluşturun.

* Seçin **simetrik anahtar** kimlik doğrulama türü olarak. 

* İsteğe bağlı olarak, **üst cihaz** ve aşağı akış bu cihaz üzerinden bağlanacak IOT Edge ağ geçidi cihazını seçin. Bu adım simetrik anahtar kimlik doğrulaması için isteğe bağlıdır, ancak üst cihaz ayarı sağlar çünkü önerilir [çevrimdışı özellikleri](offline-capabilities.md) aşağı akış cihazınız için. Cihaz ayrıntılarını eklemek veya üst daha sonra değiştirmek için her zaman güncelleştirebilirsiniz. 

   ![Simetrik anahtar kimlik doğrulama Portalı'nda ile cihaz kimliği oluşturma](./media/how-to-authenticate-downstream-device/symmetric-key-portal.png)

Kullanabileceğiniz [Azure CLI için IOT uzantısı](https://github.com/Azure/azure-iot-cli-extension) aynı işlem tamamlanamadı. Aşağıdaki örnek, yeni bir IOT cihazı ile simetrik anahtar kimlik doğrulaması oluşturur ve bir üst cihaz atar: 

```cli
az iot hub device-identity create -n {iothub name} -d {device ID} --pd {gateway device ID}
```

Cihaz oluşturma ve üst/alt yönetimi için Azure CLI komutları hakkında daha fazla bilgi için başvurusu içeriğine bakın [az IOT hub cihaz kimliği](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest) komutları.

### <a name="connect-to-iot-hub-through-a-gateway"></a>Bir ağ geçidi üzerinden IOT hub'a bağlama

Aynı işlem normal bir IOT doğrulamak için kullanılan simetrik anahtarlarla cihazlar IOT hub'ına akış cihazlar için de geçerlidir. Tek fark, bir işaretçi bağlantı yönlendirmek için ağ geçidi cihazı için veya IOT Hub adına kimlik doğrulama işlemini çevrimdışı senaryolarda eklemeniz gerektiğini ' dir. 

Simetrik anahtar kimlik doğrulaması için IOT Hub ile Cihazınızda, kimlik doğrulaması için atmanız gereken ek adımlar yoktur. Aşağı Akış Cihazınızı kendi ağ geçidi cihazı için bağlanabilmesi yerinde sertifikaları açıklandığı yine [bir Azure IOT Edge ağ geçidi için aşağı akış cihaz Bağlayamama](how-to-connect-downstream-device.md).

Portalda bir IOT cihazıyla oluşturduktan sonra birincil veya ikincil anahtarlarını alabilirsiniz. Bu anahtarlardan birini iletişim kurduğu herhangi bir uygulamanın IOT Hub ile içeren bağlantı dizesini dahil edilmesi gerekir. Simetrik anahtar kimlik doğrulaması için IOT Hub cihaz ayrıntılarını tam olarak biçimlendirilmiş bağlantı dizesinde size kolaylık olması için sağlar. Ağ geçidi cihazı hakkında ek bilgi için bağlantı dizesi eklemek gerekir. 

Simetrik anahtarı bağlantı dizelerini aşağı akış cihazları için aşağıdaki bileşenler gerekmektedir: 

* Cihazın bağlandığı IOT hub: `Hostname={iothub name}.azure-devices.net`
* Hub'da kayıtlı cihaz kimliği: `DeviceID={device ID}`
* Her iki birincil veya ikincil anahtarı: `SharedAccessKey={key}`
* Ağ geçidi cihazını cihaz üzerinden bağlanır. Sağlamak **hostname** IOT Edge ağ geçidi cihazın config.yaml dosyasından değer: `GatewayHostName={gateway hostname}`

Tümünü bir araya tam bağlantı dizesi şu şekilde görünür:

``` 
HostName=myiothub.azure-devices.net;DeviceId=myDownstreamDevice;SharedAccessKey=xxxyyyzzz;GatewayHostName=myGatewayDevice
```

Bir üst/alt ilişkisi aşağı akış bu cihaz için oluşturulan, ağ geçidi bağlantısı konak olarak doğrudan çağrı yaparak bağlantı dizesini basitleştirebilir. Örneğin: 

```
HostName=myGatewayDevice;DeviceId=myDownstreamDevice;SharedAccessKey=xxxyyyzzz
```

## <a name="x509-authentication"></a>X.509 kimlik doğrulaması 

X.509 sertifikaları kullanarak bir IOT cihaz kimliğini doğrulamak için iki yolu vardır. Hangi şekilde, kimlik doğrulaması için Cihazınızı IOT Hub'ına bağlanma adımları aynıdır seçin. Kimlik doğrulaması için otomatik olarak imzalanan veya CA imzalı sertifikaları seçin ve ardından IOT Hub'ına bağlanma hakkında bilgi almak devam edin. 

IOT hub'ı X.509 kimlik doğrulaması nasıl kullandığı hakkında daha fazla bilgi için aşağıdaki makalelere bakın: 
* [X.509 CA sertifikalarını kullanarak cihaz kimlik doğrulaması](../iot-hub/iot-hub-x509ca-overview.md)
* [X.509 CA sertifikalarını IOT sektördeki kavramsal anlayış](../iot-hub/iot-hub-x509ca-concept.md)

### <a name="create-the-device-identity-with-x509-self-signed-certificates"></a>Cihaz kimliği X.509 ile otomatik olarak imzalanan sertifikalar oluşturma

Bazen parmak izi kimlik doğrulaması olarak adlandırılır X.509 kendinden imzalı kimlik doğrulaması için IOT Cihazınızda yerleştirmek için yeni sertifikalar oluşturmanız gerekir. Bu sertifikaların parmak izi kimlik doğrulaması için IOT Hub ile paylaşan bunlara sahip. 

Bu senaryoyu test etmek için en kolay yolu, sertifikaları oluşturmak için kullandığınız aynı makineye kullanmaktır [saydam bir ağ geçidi olarak görev yapacak bir IOT Edge cihazı yapılandırma](how-to-create-transparent-gateway.md). Bu makine zaten doğru aracı, kök CA sertifikası ve ara CA sertifikası ile IOT cihaz sertifikaları oluşturmak için ayarlanmalıdır. Son sertifikaları ve özel anahtarlarına üzerinden aşağı akış cihazınıza sonradan kopyalayabilirsiniz. Ağ geçidi makalesindeki adımları makinenizde openssl ' ayarlayın sonra erişim sertifikası oluşturma betikleri için IOT Edge deponun bir kopyasını. Ardından diyoruz çalışan bir dizine yaptığınız  **\<WRKDIR >** sertifikaları tutacak. Varsayılan Sertifika, geliştirme ve test, bu nedenle yalnızca son 30 gün için yöneliktir. Bir kök CA sertifikası ve bir ara sertifika oluşturmuş olması gerekir. 

1. Çalışma dizininizde bir bash veya PowerShell penceresine gidin. 

2. Aşağı Akış aygıt için iki sertifika (birincil ve ikincil) oluşturun. Cihazınızın adında ve birincil veya ikincil etiketi belirtin. Bu bilgiler, dosyaları böylece birden çok cihaza yönelik sertifikaları takip adlandırmak için kullanılır. 

   ```PowerShell
   New-CACertsDevice "<device name>-primary"
   New-CACertsDevice "<device name>-secondary"
   ```

   ```bash
   ./certGen.sh create_device_certificate "<device name>-primary"
   ./certGen.sh create_device_certificate "<device name>-secondary"
   ```

3. (Parmak izi IOT hub'ı arabiriminde denir) SHA1 parmak izi 40 onaltılık karakter dizesi her sertifikadan alın. Sertifikayı görüntülemek ve parmak izini bulmak için aşağıdaki openssl komutu kullanın:

   ```PowerShell/bash
   openssl x509 -in <WORKDIR>/certs/iot-device-<device name>-primary.cert.pem -text -fingerprint | sed 's/[:]//g'
   ```

4. Azure portalındaki IOT hub'ınıza gidin ve aşağıdaki değerlerle yeni bir IOT cihaz kimliği oluşturma: 

   * Seçin **X.509 kendinden imzalı** kimlik doğrulama türü olarak.
   * Cihazınızın birincil ve ikincil sertifikaları kopyaladığınız onaltılık dizeler yapıştırın.
   * Seçin **üst cihaz** ve IOT Edge ağ geçidi cihazı, bu akış bir cihaz üzerinden bağlanır. Üst cihaz bir aşağı akış cihaz X.509 kimlik doğrulaması için gereklidir. 

   ![İle otomatik olarak imzalanan X.509 kimlik doğrulama Portalı'nda cihaz kimliği oluşturma](./media/how-to-authenticate-downstream-device/x509-self-signed-portal.png)

5. Aşağıdaki dosyaları, aşağı akış Cihazınızda herhangi bir dizine kopyalayın:

   * `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device name>*.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device id>*.cert.pfx`
   * `<WRKDIR>\certs\iot-device-<device name>*-full-chain.cert.pem`
   * `<WRKDIR>\private\iot-device-<device name>*.key.pem`

   Bu dosyalar, IOT Hub'ına bağlanan yaprak cihaz uygulamaları başvuru. Bir hizmet gibi kullanabileceğiniz [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault) veya bir işlev [güvenli kopya protoco](https://www.ssh.com/ssh/scp/) sertifika dosyalarını taşımak için.

Kullanabileceğiniz [Azure CLI için IOT uzantısı](https://github.com/Azure/azure-iot-cli-extension) aynı cihaz oluşturma işlemini tamamlamak için. Aşağıdaki örnek, yeni bir IOT cihaz otomatik olarak imzalanan X.509 kimlik doğrulaması ile oluşturur ve bir üst cihaz atar: 

```cli
az iot hub device-identity create -n {iothub name} -d {device ID} --pd {gateway device ID} --am x509_thumbprint --ptp {primary thumbprint} --stp {secondary thumbprint}
```

Cihaz oluşturma, sertifika oluşturma ve üst ve alt yönetimi için Azure CLI komutları hakkında daha fazla bilgi için başvurusu içeriğine bakın [az IOT hub cihaz kimliği](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest) komutları.

### <a name="create-the-device-identity-with-x509-ca-signed-certificates"></a>Cihaz oluşturma kimliği X.509 CA imzalı sertifikaları

X.509 sertifika yetkilisi (CA) imzalı kimlik doğrulaması için IOT Hub, IOT cihazınızın sertifikaları imzalamak için kullandığınız kayıtlı bir kök CA sertifikası gerekir. Sorunları olan bir sertifikayı kök CA sertifikasını veya Ara sertifikalarını birini kullanarak herhangi bir CİHAZDAN kimlik doğrulaması için izin verilir. 

Bu bölümde, IOT hub'ı makalesinde ayrıntılı yönergeleri dayalı [X.509 güvenliği Azure IOT hub'ınızdaki](../iot-hub/iot-hub-security-x509-get-started.md). Hangi değerlerin bir ağ geçidi üzerinden bağlanan bir aşağı akış cihazı kurmak için kullanılacağını öğrenmek için bu bölümdeki adımları izleyin. 

Bu senaryoyu test etmek için en kolay yolu, sertifikaları oluşturmak için kullandığınız aynı makineye kullanmaktır [saydam bir ağ geçidi olarak görev yapacak bir IOT Edge cihazı yapılandırma](how-to-create-transparent-gateway.md). Bu makine zaten doğru aracı, kök CA sertifikası ve ara CA sertifikası ile IOT cihaz sertifikaları oluşturmak için ayarlanmalıdır. Son sertifikaları ve özel anahtarlarına üzerinden aşağı akış cihazınıza sonradan kopyalayabilirsiniz. Ağ geçidi makalesindeki adımları makinenizde openssl ' ayarlayın sonra erişim sertifikası oluşturma betikleri için IOT Edge deponun bir kopyasını. Ardından diyoruz çalışan bir dizine yaptığınız  **\<WRKDIR >** sertifikaları tutacak. Varsayılan Sertifika, geliştirme ve test, bu nedenle yalnızca son 30 gün için yöneliktir. Bir kök CA sertifikası ve bir ara sertifika oluşturmuş olması gerekir. 

1. Bölümündeki yönergeleri [IOT hub'ınıza kaydetmek X.509 CA sertifikalarını](../iot-hub/iot-hub-security-x509-get-started.md#register-x509-ca-certificates-to-your-iot-hub) bölümünü *X.509 güvenliği Azure IOT hub'ınızdaki*. Bu bölümde, aşağıdaki adımları gerçekleştirin: 

   1. Bir kök CA sertifikası yükleyin. Saydam bir ağ geçidi makalede oluşturulan sertifikalar kullanıyorsanız, karşıya yükleme  **\<WRKDIR > /certs/azure-iot-test-only.root.ca.cert.pem** kök sertifika dosyası olarak. 
   2. Bu kök CA sertifikasını sahibi olduğunu doğrulayın. Sertifika araçları sahip olma onayı doğrulayabilirsiniz \<WRKDIR >. 

      ```powershell
      New-CACertsVerificationCert "<verification code from Azure portal>"
      ```

      ```bash
      ./certGen.sh create_verification_certificate <verification code from Azure portal>"
      ```

2. Bölümündeki yönergeleri [IOT hub'ınız için bir X.509 cihazı oluşturma](../iot-hub/iot-hub-security-x509-get-started.md#create-an-x509-device-for-your-iot-hub) bölümünü *X.509 güvenliği Azure IOT hub'ınızdaki*. Bu bölümde, aşağıdaki adımları gerçekleştirin: 

   1. Yeni bir cihaz ekleyin. Küçük bir ad verin **cihaz kimliği**, kimlik doğrulaması türünü seçin **X.509 CA imzalı**. 
   2. Üst cihaz ayarlayın. Aşağı Akış cihazlar için seçin **üst cihaz** ve IOT Edge, IOT Hub'ına bağlantı sağlayan ağ geçidi cihazını seçin. 

3. Aşağı Akış cihazınız için bir sertifika zinciri oluşturun. Bu zincir yapmak için IOT Hub'ına yüklediğiniz aynı kök CA sertifikasını kullanın. Cihaz kimliğinize Portalı'nda verdiğiniz aynı küçük bir cihaz kimliği kullanın.

   ```powershell
   New-CACertsDevice "<device id>"
   ```

   ```bash
   ./certGen.sh create_device_certificate "<device id>"
   ```

4. Aşağıdaki dosyaları, aşağı akış Cihazınızda herhangi bir dizine kopyalayın: 

   * `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device id>*.cert.pem`
   * `<WRKDIR>\certs\iot-device-<device id>*.cert.pfx`
   * `<WRKDIR>\certs\iot-device-<device id>*-full-chain.cert.pem`
   * `<WRKDIR>\private\iot-device-<device id>*.key.pem`

   Bu dosyalar, IOT Hub'ına bağlanan yaprak cihaz uygulamaları başvuru. Bir hizmet gibi kullanabileceğiniz [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault) veya bir işlev [güvenli kopya protoco](https://www.ssh.com/ssh/scp/) sertifika dosyalarını taşımak için.

Kullanabileceğiniz [Azure CLI için IOT uzantısı](https://github.com/Azure/azure-iot-cli-extension) aynı cihaz oluşturma işlemini tamamlamak için. Aşağıdaki örnek, yeni bir IOT cihazı ile X.509 CA imzalı kimlik doğrulaması oluşturur ve bir üst cihaz atar: 

```cli
az iot hub device-identity create -n {iothub name} -d {device ID} --pd {gateway device ID} --am x509_ca
```

Cihaz oluşturma ve üst/alt yönetimi için Azure CLI komutları hakkında daha fazla bilgi için başvurusu içeriğine bakın [az IOT hub cihaz kimliği](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest) komutları.


### <a name="connect-to-iot-hub-through-a-gateway"></a>Bir ağ geçidi üzerinden IOT hub'a bağlama

Her Azure IOT SDK'sı, X.509 kimlik doğrulaması biraz farklı işler. Ancak, aynı işlem normal bir IOT kimliğini doğrulamak için kullanılır X.509 sertifikalarıyla cihazlar IOT hub'ına akış cihazlar için de geçerlidir. Tek fark, bir işaretçi bağlantı yönlendirmek için ağ geçidi cihazı için veya IOT Hub adına kimlik doğrulama işlemini çevrimdışı senaryolarda eklemeniz gerektiğini ' dir. Genel olarak, tüm IOT Hub cihazları aynı X.509 kimlik doğrulama adımlarını izleyin, sonra yalnızca değiştirin **Hostname** bağlantı dizesinde ana bilgisayar adını, ağ geçidi cihazı olabilir. 

Aşağıdaki bölümlerde farklı SDK diller için bazı örnekler gösterilmektedir. 

>[!IMPORTANT]
>Aşağıdaki örnekleri sertifikaları IOT Hub SDK'ları cihazların kimliklerini doğrulamak için nasıl kullanıldığını göstermektedir. Bir üretim dağıtımında, özel veya SAS anahtarları donanım güvenli modülünde (HSM) gibi tüm gizli dizilerin depolamanız gerekir. 

#### <a name="net"></a>.NET

Bir örneği bir C# program IOT Hub'ına X.509 sertifikalarıyla kimlik doğrulaması için bkz: [X.509 güvenliği Azure IOT hub'ınızdaki](../iot-hub/iot-hub-security-x509-get-started.md#authenticate-your-x509-device-with-the-x509-certificates). Bu örnek önemli satırlarını bazıları burada kimlik doğrulama işlemini göstermek için dahil edilir.

DeviceClient örneğinizin ana bilgisayar bildirirken, IOT Edge ağ geçidi cihazın ana bilgisayar adı kullanın. Ana bilgisayar adı, ağ geçidi cihazın config.yaml dosyasında bulunabilir. 

IOT Edge git deposu tarafından sağlanan test sertifikaları kullanıyorsanız, sertifikalara anahtarıdır **1234**.

```csharp
try
{
    var cert = new X509Certificate2(@"<absolute-path-to-your-device-pfx-file>", "1234");
    var auth = new DeviceAuthenticationWithX509Certificate("<device-id>", cert);
    var deviceClient = DeviceClient.Create("<gateway hostname>", auth, TransportType.Amqp_Tcp_Only);

    if (deviceClient == null)
    {
        Console.WriteLine("Failed to create DeviceClient!");
    }
    else
    {
        Console.WriteLine("Successfully created DeviceClient!");
        SendEvent(deviceClient).Wait();
    }

    Console.WriteLine("Exiting...\n");
}
catch (Exception ex)
{
    Console.WriteLine("Error in sample: {0}", ex.Message);
}
```

#### <a name="c"></a>C

Bir C programının bir örnek için C IOT SDK'ın bkz IOT Hub'ına X.509 sertifikalarıyla kimlik doğrulaması [iotedge_downstream_device_sample](https://github.com/Azure/azure-iot-sdk-c/tree/x509_edge_bugbash/iothub_client/samples/iotedge_downstream_device_sample) örnek. Bu örnek önemli satırlarını bazıları burada kimlik doğrulama işlemini göstermek için dahil edilir.

IOT Edge ağ geçidi cihazın konak adı için aşağı akış cihazınız için bağlantı dizesini tanımlarken kullanmak **HostName** parametresi. Ana bilgisayar adı, ağ geçidi cihazın config.yaml dosyasında bulunabilir. 

```C
// If your downstream device uses X.509 authentication (self signed or X.509 CA) then
// resulting connection string should look like the following:
// "HostName=<gateway device hostname>;DeviceId=<device_id>;x509=true"
static const char* connectionString = "[Downstream device IoT Edge connection string]";

// Path to the Edge "owner" root CA certificate
static const char* edge_ca_cert_path = "[Path to root CA certificate]";

// When the downstream device uses X.509 authentication, a certificate and key 
// in PRM format must be provided.
static const char * x509_device_cert_path = "[Path to primary or secondary device cert]";
static const char * x509_device_key_path = "[Path to primary or secondary device key]";

int main(void)
{
    // Create the iothub handle here
    device_handle = IoTHubDeviceClient_CreateFromConnectionString(connectionString, protocol);

    // Provide the Azure IoT device client with the same root
    // X509 CA certificate that was used to set up the IoT Edge gateway runtime
    if (edge_ca_cert_path != NULL)
    {
        cert_string = obtain_edge_ca_certificate();
        (void)IoTHubDeviceClient_SetOption(device_handle, OPTION_TRUSTED_CERT, cert_string);
    }

    if ((x509_device_cert_path != NULL) && (x509_device_key_path != NULL))
    {
        const char *x509certificate = obtain_file_contents(x509_device_cert_path);
        const char *x509privatekey = obtain_file_contents(x509_device_key_path);
        if ((IoTHubDeviceClient_SetOption(device_handle, OPTION_X509_CERT, x509certificate) != IOTHUB_CLIENT_OK) ||
            (IoTHubDeviceClient_SetOption(device_handle, OPTION_X509_PRIVATE_KEY, x509privatekey) != IOTHUB_CLIENT_OK)
            )
        {
            printf("failure to set options for x509, aborting\r\n");
            exit(1);
        }
    }
}
```

#### <a name="nodejs"></a>Node.js

Node.js IOT SDK'ın bir Node.js program örneği için bkz IOT Hub'ına X.509 sertifikalarıyla kimlik doğrulaması [simple_sample_device_x509.js](https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/simple_sample_device_x509.js) örnek. Bu örnek önemli satırlarını bazıları burada kimlik doğrulama işlemini göstermek için dahil edilir.

IOT Edge ağ geçidi cihazın konak adı için aşağı akış cihazınız için bağlantı dizesini tanımlarken kullanmak **HostName** parametresi. Ana bilgisayar adı, ağ geçidi cihazın config.yaml dosyasında bulunabilir. 

IOT Edge git deposu tarafından sağlanan test sertifikaları kullanıyorsanız, sertifikalara anahtarıdır **1234**.

```node
// String containing Hostname and Device Id in the following format:
//  "HostName=<gateway device hostname>;DeviceId=<device_id>;x509=true"
var connectionString = '<DEVICE CONNECTION STRING WITH x509=true>';
var certFile = '<PATH-TO-CERTIFICATE-FILE>';
var keyFile = '<PATH-TO-KEY-FILE>';
var passphrase = '<KEY PASSPHRASE IF ANY>';

// fromConnectionString must specify a transport constructor, coming from any transport package.
var client = Client.fromConnectionString(connectionString, Protocol);

var options = {
   cert : fs.readFileSync(certFile, 'utf-8').toString(),
   key : fs.readFileSync(keyFile, 'utf-8').toString(),
   passphrase: passphrase
 };

// Calling setOptions with the x509 certificate and key (and optionally, passphrase) will configure the client transport to use x509 when connecting to IoT Hub
client.setOptions(options);
```

#### <a name="python"></a>Python

Python programını örneği için Java IOT SDK'ın bkz IOT Hub'ına X.509 sertifikalarıyla kimlik doğrulaması [iothub_client_sample_x509.py](https://github.com/Azure/azure-iot-sdk-python/blob/master/device/samples/iothub_client_sample_x509.py) örnek. Bu örnek önemli satırlarını bazıları burada kimlik doğrulama işlemini göstermek için dahil edilir.

IOT Edge ağ geçidi cihazın konak adı için aşağı akış cihazınız için bağlantı dizesini tanımlarken kullanmak **HostName** parametresi. Ana bilgisayar adı, ağ geçidi cihazın config.yaml dosyasında bulunabilir. 

```python
# String containing Hostname, Device Id in the format:
# "HostName=<gateway device hostname>;DeviceId=<device_id>;x509=true"
CONNECTION_STRING = "[Device Connection String]"

X509_CERTIFICATE = (
    "-----BEGIN CERTIFICATE-----""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX""\n"
    "...""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX""\n"
    "XXXXXXXXXXXX""\n"
    "-----END CERTIFICATE-----"
)

X509_PRIVATEKEY = (
    "-----BEGIN RSA PRIVATE KEY-----""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX""\n"
    "...""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX""\n"
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    "-----END RSA PRIVATE KEY-----"
)

def iothub_client_init():
    # prepare iothub client
    client = IoTHubClient(CONNECTION_STRING, PROTOCOL)

    # this brings in x509 privateKey and certificate
    client.set_option("x509certificate", X509_CERTIFICATE)
    client.set_option("x509privatekey", X509_PRIVATEKEY)

    return client
```

#### <a name="java"></a>Java

Bir Java programının bir örnek için Java IOT SDK'ın bkz IOT Hub'ına X.509 sertifikalarıyla kimlik doğrulaması [SendEventX509.java](https://github.com/Azure/azure-iot-sdk-python/blob/master/device/samples/iothub_client_sample_x509.py) örnek. Bu örnek önemli satırlarını bazıları burada kimlik doğrulama işlemini göstermek için dahil edilir.

IOT Edge ağ geçidi cihazın konak adı için aşağı akış cihazınız için bağlantı dizesini tanımlarken kullanmak **HostName** parametresi. Ana bilgisayar adı, ağ geçidi cihazın config.yaml dosyasında bulunabilir. 

```java
//PEM encoded representation of the public key certificate
private static String publicKeyCertificateString =
    "-----BEGIN CERTIFICATE-----\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "-----END CERTIFICATE-----\n";

//PEM encoded representation of the private key
private static String privateKeyString =
    "-----BEGIN EC PRIVATE KEY-----\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
    "-----END EC PRIVATE KEY-----\n";

DeviceClient client = new DeviceClient(connectionString, protocol, publicKeyCertificateString, false, privateKeyString, false);
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede tamamlayarak saydam bir ağ geçidi olarak çalışan bir IOT Edge cihazı olması gerekir ve bir aşağı akış cihaz bir IOT hub'da kayıtlı. Ardından, ağ geçidi cihazı güven ve iletileri göndermek için aşağı akış cihazlarınızı yapılandırmak gerekir. Daha fazla bilgi için [bir Azure IOT Edge ağ geçidi için aşağı akış cihaz Bağlayamama](how-to-connect-downstream-device.md).
