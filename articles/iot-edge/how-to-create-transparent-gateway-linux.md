---
title: Azure IOT kenar - Linux saydam bir ağ geçidi oluşturma | Microsoft Docs
description: Birden çok aygıt için bilgileri işleyebilir saydam bir ağ geçidi oluşturmak için Azure IOT kenar kullanın
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 6/20/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 5a78d6fb8ee52f0daba80a77cc8a5e75c2e5248d
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036518"
---
# <a name="create-a-linux-iot-edge-device-that-acts-as-a-transparent-gateway"></a>Saydam bir ağ geçidi olarak davranır Linux IOT sınır cihazı oluşturma

Bu makalede bir IOT sınır cihazı saydam bir ağ geçidi olarak kullanmaya yönelik ayrıntılı yönergeler sağlar. Bu makalenin devamında terimini *IOT sınır ağ geçidi* saydam bir ağ geçidi olarak kullanılan bir IOT sınır cihazı başvuruyor. Daha ayrıntılı bilgi için bkz: [nasıl bir IOT sınır cihazı bir ağ geçidi olarak kullanılabilir][lnk-edge-as-gateway], kavramsal genel bakış sağlar.

>[!NOTE]
>Şu anda:
> * Ağ geçidi IOT Hub'ından kesilirse, aşağı akış cihazların ağ geçidi ile kimlik doğrulaması yapamaz.
> * IOT sınır cihazları için IOT uç ağ geçitlerine bağlanamıyor.
> * Aşağı Akış aygıtları karşıya dosya yükleme kullanmayın.

Saydam bir ağ geçidi oluşturma hakkında daha fazla sabit bölümü güvenli bir şekilde aşağı akış aygıtlar için ağ geçidi bağlanmaktadır. Azure IOT kenar, bu cihazlar arasında güvenli TLS bağlantıları ayarlamak için PKI altyapısı kullanmanıza olanak sağlar. Bu durumda, biz saydam bir ağ geçidi olarak davranan bir IOT sınır cihazı bağlanmak bir aşağı akış aygıtı izin vermek.  Makul güvenliğini sağlamak için aşağı akış aygıt, ağ geçitleri ve değil kötü amaçlı bir ağ geçidi aygıtlarınızı yalnızca istediğinden sınır cihazı kimliğini onaylamalıdır.

Ağ geçidi aygıtı topolojiniz için gereken güven sağlayan herhangi bir sertifika altyapı oluşturabilirsiniz. Bu makalede, etkinleştirmek için kullanacağınız aynı sertifika Kurulumu varsayıyoruz [X.509 CA güvenlik] [ lnk-iothub-x509] IOT Hub ' hangi içerir belirli IOT hub (IOT hub sahip CA için ilişkili bir X.509 CA sertifikası ), bu CA ve bir CA için sınır cihazı imzalanmış sertifikalar, bir dizi.

![Ağ geçidi Kurulumu][1]

Ağ geçidi bağlantı başlatma sırasında kenar aygıt CA sertifikasını aşağı akış aygıta gösterir. Edge cihaz CA sertifikası sahibi CA sertifikası tarafından imzalanmış emin olmak için aşağı akış aygıt denetler. Bu işlem, ağ geçidi güvenilen bir kaynaktan geldiğinden onaylamak aşağı akış aygıt sağlar.

Aşağıdaki adımlar, sertifikaları oluşturma ve bunları doğru yerlerde yükleme işleminde size rehberlik.

## <a name="prerequisites"></a>Önkoşullar
1.  Azure IOT kenar saydam ağ geçidi olarak kullanmak istediğiniz bir Linux cihazda yükleyin.
   * [Linux x64][lnk-install-linux-x64]
   * [Linux ARM32][lnk-install-linux-arm]

2.  Aşağıdaki komutla gerekli üretim dışı sertifikalarını oluşturmak için komut dosyalarını alın. Bu komut dosyalarını saydam bir ağ geçidi kurun için gerekli sertifikaları oluşturmanıza yardımcı olur. 

   ```cmd
   git clone https://github.com/Azure/azure-iot-sdk-c.git
   ```

3. Bu komut dosyaları, gerekli sertifikaları oluşturmak için OpenSSL kullanın ve bazı kurulum OpenSSL gerektirir.
   
   1. Çalışmak istediğiniz dizine gidin. Üzerinde buradan Biz bu $WRKDIR başvurun.  Tüm dosyalar bu dizinde oluşturulur.

      CD $WRKDIR
   
   1. Yapılandırma ve komut dosyaları, çalışma dizinine kopyalayın.

      ```cmd
      cp azure-iot-sdk-c/tools/CACertificates/*.cnf .
      cp azure-iot-sdk-c/tools/CACertificates/certGen.sh .
      chmod 700 certGen.sh 
      ```

## <a name="certificate-creation"></a>Sertifika oluşturma
1.  Sahibi CA sertifikasını ve ara sertifika oluşturun. Bunlar tüm yerleştirilir `$WRKDIR`.

   ```cmd
   ./certGen.sh create_root_and_intermediate
   ```

   Betik yürütme çıkışları aşağıdaki sertifikaları ve anahtarları şunlardır:
   * Sertifikalar
      * `$WRKDIR/certs/azure-iot-test-only.root.ca.cert.pem`
      * `$WRKDIR/certs/azure-iot-test-only.intermediate.cert.pem`
   * Anahtarlar
      * `$WRKDIR/private/azure-iot-test-only.root.ca.key.pem`
      * `$WRKDIR/private/azure-iot-test-only.intermediate.key.pem`

2.  Sınır cihazı CA sertifikasını ve özel anahtarı komutuyla oluşturun.

   >[!NOTE]
   > **SAĞLAMADIĞI** ağ geçidi'nin DNS ana bilgisayar adı ile aynı adı kullanın. Bunun yapılması, istemci sertifika vermesine bu sertifikalara karşı neden olur.

      ```cmd
      ./certGen.sh create_edge_device_certificate "<gateway device name>"
      ```

   Betik yürütme çıktısı aşağıdaki sertifika ve anahtar şunlardır:
   * `$WRKDIR/certs/new-edge-device.*`
   * `$WRKDIR/private/new-edge-device.key.pem`

## <a name="certificate-chain-creation"></a>Sertifika zinciri oluşturma
Sahibi CA sertifikasını, ara sertifika ve kenar aygıt CA sertifikasını aşağıdaki komutla bir sertifika zinciri oluşturun. Bir zincir dosyasında yerleştirme, kolayca saydam bir ağ geçidi olarak çalıştığından, kenar Cihazınızda yüklemenize olanak tanır.

   ```cmd
   cat ./certs/new-edge-device.cert.pem ./certs/azure-iot-test-only.intermediate.cert.pem ./certs/azure-iot-test-only.root.ca.cert.pem > ./certs/new-edge-device-full-chain.cert.pem
   ```

## <a name="installation-on-the-gateway"></a>Ağ geçidi yükleme
1.  Aşağıdaki dosyalar $WRKDIR kenar cihazınız üzerinde herhangi bir yere kopyalayın, biz için $CERTDIR bakın. Edge aygıtınızda sertifikaları oluşturulan bu adımı atlayın.

   * Cihaz CA sertifikası-  `$WRKDIR/certs/new-edge-device-full-chain.cert.pem`
   * Cihaz CA özel anahtarını- `$WRKDIR/private/new-edge-device.key.pem`
   * Sahibi CA- `$WRKDIR/certs/azure-iot-test-only.root.ca.cert.pem`

2.  Ayarlama `certificate` özelliklerinde sertifika ve anahtar dosyalarını yerleştirdiğiniz güvenlik arka plan programı config yaml dosya yolu.

```yaml
certificates:
  device_ca_cert: "$CERTDIR/certs/new-edge-device-full-chain.cert.pem"
  device_ca_pk: "$CERTDIR/private/new-edge-device.key.pem"
  trusted_ca_certs: "$CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem"
```

## <a name="deploy-edgehub-to-the-gateway"></a>Ağ geçidi EdgeHub dağıtma
Azure IoT Edge'in önemli özelliklerinden biri buluttan IoT Edge cihazlarınıza modüller dağıtabilmektir. Bu bölümde, görünen boş bir dağıtım oluşturmak sahiptir; ancak kenar Hub diğer modülünüz mevcut olsa bile tüm dağıtımlar için eklenen automatcially olur. Edge hub'ı saydam bir ağ geçidi olarak boş bir dağıtımı oluşturma yeterli olacak şekilde hareket için bir sınır aygıtında gereken yalnızca modülüdür. 
1. Azure portalında IoT Hub'ınıza gidin.
2. Git **IOT kenar** ve bir ağ geçidi olarak kullanmak istediğiniz IOT kenar Cihazınızı seçin.
3. **Modülleri Ayarlama**'yı seçin.
4. **İleri**’yi seçin.
5. İçinde **belirtin yollar** adım, tüm iletileri tüm modüllerden IOT Hub'ına gönderir ve varsayılan bir yol olmalıdır. Aksi durumda, aşağıdaki kodu ekleyin ve sonra seçin **sonraki**.
   ```JSON
   {
       "routes": {
           "route": "FROM /* INTO $upstream"
       }
   }
   ```
6. Gözden geçirme şablon adımda seçin **gönderme**.

## <a name="installation-on-the-downstream-device"></a>Aşağı Akış aygıtta yükleme
Aşağı Akış cihaz herhangi bir uygulama olabilir kullanarak [Azure IOT cihaz SDK'sı][lnk-devicesdk], basit bir açıklandığı gibi [Cihazınızı .NET kullanarak IOT hub'ınıza bağlanmak] [ lnk-iothub-getstarted]. Güven bir aşağı akış cihaz uygulaması sahip **sahibi CA** ağ geçidi aygıtlarını TLS bağlantılarını doğrulamak için sertifika. Bu adım genellikle iki yolla gerçekleştirilebilir: işletim sistemi düzeyinde veya (için belirli diller) uygulama düzeyinde.

### <a name="os-level"></a>İşletim sistemi düzeyi
Bu sertifika işletim sistemi sertifika deposunda yükleme sahibi kullanmak için tüm uygulamaları CA sertifikası güvenilen bir sertifika izin verir.

* Ubuntu -, Ubuntu ana bilgisayarda bir CA sertifikası yüklemek nasıl bir örneği burada verilmiştir.

   ```cmd
   sudo cp $CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem  /usr/local/share/ca-certificates/azure-iot-test-only.root.ca.cert.pem.crt
   sudo update-ca-certificates
   ```
 
    "Güncelleştirme sertifikaları... /etc/ssl/certs bildiren bir ileti görürsünüz 0 kaldırılan 1 eklenen; tamamlandı."

* Windows - [bu](https://msdn.microsoft.com/en-us/library/cc750534.aspx) makale ayrıntıları Sertifika Alma Sihirbazı kullanarak bir Windows aygıtında bunu yapma. 

### <a name="application-level"></a>Uygulama düzeyi
.NET uygulamaları için aşağıdaki kod parçacığını PEM biçiminde bir sertifika güven ekleyebilirsiniz. Değişkeni başlatmak `certPath` ile `$CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem`.

   ```
   using System.Security.Cryptography.X509Certificates;

   ...

   X509Store store = new X509Store(StoreName.Root, StoreLocation.CurrentUser);
   store.Open(OpenFlags.ReadWrite);
   store.Add(new X509Certificate2(X509Certificate2.CreateFromCertFile(certPath)));
   store.Close();
   ```

## <a name="connect-the-downstream-device-to-the-gateway"></a>Aşağı Akış aygıt ağ geçidine bağlanmak
Ağ geçidi aygıtı konak adına başvuran bir bağlantı dizesini IOT Hub cihaz SDK'sı başlatması gerekir. Bu ekleyerek yapılır `GatewayHostName` özelliği, cihaz bağlantı dizesi. Örneğin, bir örnek cihaz bağlantı dizesi eklenmiş biz bir aygıt için işte `GatewayHostName` özelliği:

   ```
   HostName=yourHub.azure-devices.net;DeviceId=yourDevice;SharedAccessKey=XXXYYYZZZ=;GatewayHostName=mygateway.contoso.com
   ```

   >[!NOTE]
   >Bu her şeyi bırakıldı hangi testlerin ayarlanan bir örnek doğru komuttur. Sohuld "Tamam doğrulandı" iletisini.
   >
   >openssl s_client-mygateway.contoso.com:8883 - CAfile $CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem - showcerts Bağlan

## <a name="routing-messages-from-downstream-devices"></a>Aşağı Akış aygıtlardan ileti yönlendirme
IOT kenar çalışma zamanı yalnızca modülleri tarafından gönderilen iletileri gibi aşağı akış aygıtlardan gönderilen iletileri yönlendirebilir. Bu ağ geçidi bulut için veri göndermeden önce çalışan bir modüldeki analiz gerçekleştirmenize olanak sağlar. Rota adlı bir aşağı akış aygıttan iletileri göndermek için kullanılacak `sensor` modül adı için `ai_insights`.

   ```json
   { "routes":{ "sensorToAIInsightsInput1":"FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/ai_insights/inputs/input1\")", "AIInsightsToIoTHub":"FROM /messages/modules/ai_insights/outputs/output1 INTO $upstream" } }
   ```

[Modülü birleşim makalesine] başvurun [lnk--için modül birleşimi] ileti yönlendirme hakkında daha ayrıntılı bilgi.

## <a name="next-steps"></a>Sonraki adımlar
[IOT kenar modülleri geliştirmek için Araçlar ve gereksinimleri anlamanız][lnk-module-dev].

<!-- Images -->
[1]: ./media/how-to-create-transparent-gateway/gateway-setup.png

<!-- Links -->
[lnk-install-linux-x64]: ./how-to-install-iot-edge-linux.md
[lnk-install-linux-arm]: ./how-to-install-iot-edge-linux-arm.md
[lnk-devicesdk]: ../iot-hub/iot-hub-devguide-sdks.md
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
[lnk-edge-as-gateway]: ./iot-edge-as-gateway.md
[lnk-module-dev]: module-development.md
[lnk-iothub-getstarted]: ../iot-hub/iot-hub-csharp-csharp-getstarted.md
[lnk-iothub-x509]: ../iot-hub/iot-hub-x509ca-overview.md
[lnk-iothub-secure-deployment]: ../iot-hub/iot-hub-security-deployment.md
[lnk-iothub-tokens]: ../iot-hub/iot-hub-devguide-security.md#security-tokens
[lnk-iothub-throttles-quotas]: ../iot-hub/iot-hub-devguide-quotas-throttling.md
[lnk-iothub-devicetwins]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-iothub-c2d]: ../iot-hub/iot-hub-devguide-messages-c2d.md
[lnk-ca-scripts]: https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md
[lnk-modbus-module]: https://github.com/Azure/iot-edge-modbus
