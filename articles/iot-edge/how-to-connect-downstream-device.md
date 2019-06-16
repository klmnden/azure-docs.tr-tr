---
title: Aşağı Akış cihazları - Azure IOT Edge bağlayın | Microsoft Docs
description: Aşağı yönde yapılandırma veya yaprak cihazların Azure IOT Edge ağ geçidi cihazlara bağlanın.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/07/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 7a66355ca1a0c9c2c144f04cd944efe22467d3ae
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67058519"
---
# <a name="connect-a-downstream-device-to-an-azure-iot-edge-gateway"></a>Bir Azure IOT Edge ağ geçidi için bir aşağı akış cihazı bağlayın

Bu makale, aşağı akış cihazları ve IOT Edge saydam ağ geçitleri arasında güvenilir bir bağlantı kurmak için yönergeler sağlar. Saydam bir ağ geçidi senaryosunda, bir veya daha fazla cihazı IOT hub'ına bağlantı tutan bir tek ağ geçidi cihazı aracılığıyla iletilerinin geçirebilirsiniz. Bir aşağı akış cihaz herhangi bir uygulama veya ile oluşturulan bir kimliğe sahiptir platform olabilir [Azure IOT hub'ı](https://docs.microsoft.com/azure/iot-hub) bulut hizmeti. Çoğu durumda, bu uygulamaları kullanma [Azure IOT cihaz SDK'sını](../iot-hub/iot-hub-devguide-sdks.md). Bir aşağı akış cihaz bile IOT Edge ağ geçidi cihazın kendisinde çalışan bir uygulama olabilir. 

Başarılı saydam bir ağ geçidi bağlantısı kurmak için üç genel adımlar vardır. Bu makale, üçüncü adımı kapsar:

1. Ağ geçidi cihazı güvenli bir şekilde aşağı akış cihazlara bağlanın, aşağı akış cihazlardan iletişimleri alabilir ve iletileri yönlendirmek için uygun hedef gerekir. Daha fazla bilgi için [saydam bir ağ geçidi olarak görev yapacak bir IOT Edge cihazı yapılandırma](how-to-create-transparent-gateway.md).
2. Aşağı Akış cihaz IOT Hub ile kimlik doğrulaması yapabilmek için bir cihaz kimliği gerekiyor ve kendi ağ geçidi cihazı iletişim kurmak için biliyorsunuz. Daha fazla bilgi için [Azure IOT hub'a bir aşağı akış cihaz kimlik doğrulaması](how-to-authenticate-downstream-device.md).
3. **Aşağı Akış cihaz, ağ geçidi cihazı için güvenli bir şekilde bağlanabilmesi gerekir.**

Bu makalede, aşağı akış cihaz bağlantıları olan yaygın sorunlar tanımlanmakta ve aşağı akış cihazları tarafından ayarlama işleminde size yol gösterir: 

* Aktarım Katmanı Güvenliği (TLS) açıklayan ve sertifika ile ilgili temel bilgiler. 
* TLS kitaplıkları farklı işletim sistemleri arasında nasıl çalıştığı ve her işletim sistemi sertifikaları ile nasıl ilgileneceğini açıklar.
* Azure IOT walking başlamanıza yardımcı olmak üzere çeşitli dillerde örnekleri başlatıldı. 

Bu makalede, koşulları *ağ geçidi* ve *IOT Edge ağ geçidi* saydam bir ağ geçidi olarak yapılandırılmış bir IOT Edge cihazı bakın. 

## <a name="prepare-a-downstream-device"></a>Bir aşağı akış cihazı hazırlama

Bir aşağı akış cihaz herhangi bir uygulama veya ile oluşturulan bir kimliğe sahiptir platform olabilir [Azure IOT hub'ı](https://docs.microsoft.com/azure/iot-hub) bulut hizmeti. Çoğu durumda, bu uygulamaları kullanma [Azure IOT cihaz SDK'sını](../iot-hub/iot-hub-devguide-sdks.md). Bir aşağı akış cihaz bile IOT Edge ağ geçidi cihazın kendisinde çalışan bir uygulama olabilir. 

Bir IOT Edge ağ geçidi için bir aşağı akış cihaza bağlanmak için iki şey gerekir:

* Bir cihaz veya uygulama ağ geçidine bağlanmak için gereken bilgileri ile eklenmiş bir IOT Hub cihaz bağlantı dizesi ile yapılandırılmış. 

    Bu adımda açıklanan [Azure IOT hub'a bir aşağı akış cihaz kimlik doğrulaması](how-to-authenticate-downstream-device.md).

* Cihaz veya uygulama ağ geçidinin güvenmeyi sahip **kök CA** ağ geçidi cihazı TLS bağlantılarını doğrulamak için sertifika. 

    Bu adım, bu makalenin geri kalanında ayrıntılı açıklanmıştır. Bu adımda gerçekleştirilen iki yöntemden biri olabilir: Azure IOT SDK'larını kullanarak uygulamalar içindeki sertifika başvurarak CA sertifikasını işletim sisteminin sertifika deposunda veya (Belirli diller) yükleyerek.

## <a name="tls-and-certificate-fundamentals"></a>TLS ve sertifika ile ilgili temel bilgiler

İnternet üzerinden oluşan yalnızca herhangi diğer güvenli istemci/sunucu iletişimini güvenli bir aşağı akış cihazları IOT Edge bağlanma zorluklarıyla gibidir. Bir istemci ve sunucu güvenli bir şekilde kullanarak internet üzerinden iletişim [Aktarım Katmanı Güvenliği (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security). TLS standardı kullanılarak oluşturulan [ortak anahtar altyapısı (PKI)](https://en.wikipedia.org/wiki/Public_key_infrastructure) sertifikaları yapılar çağırılır. TLS oldukça karmaşık bir özelliğidir ve çok çeşitli iki uç nokta güvenliğini sağlamak için ilgili konuları ele. Bu bölümde, bir IOT Edge ağ geçidi için cihazların güvenli bir şekilde bağlamak için ilgili kavramları özetlenmektedir.

Bir istemci bir sunucuya bağlandığında, sunucu adı verilen sertifikaları zincirine sunar *sunucu sertifika zinciri*. Bir sertifika zinciri, genellikle bir kök sertifika yetkilisi (CA) sertifikası, bir veya daha fazla ara CA sertifikalarının ve son olarak sunucunun sertifikanın kendisi oluşur. Bir istemci, şifreli olarak tüm sunucu sertifika zinciri doğrulayarak sunucusuyla güven oluşturur. Bu istemci doğrulama, sunucu sertifika zinciri olarak adlandırılan *sunucu zinciri doğrulaması*. İstemci, şifreli olarak adlı bir işlem içinde sunucu sertifikasının ilişkili özel anahtarı elinde kanıtlamak için hizmet sorunlarını *kavram kanıtı elinde*. Sunucu zinciri doğrulaması birleşimi ve kanıtını çağırıldı *sunucu kimlik doğrulaması*. Sunucu sertifika zinciri doğrulamak için bir istemci bir kopyasını oluşturun (veya sorun) sunucu sertifikasının kullanılan kök CA sertifikası gerekiyor. İstemci sorunsuz bir işlem bu normalde Web sitelerine bağlanmak, bir tarayıcı ile yaygın olarak kullanılan CA sertifikalarını önceden yapılandırılmış olarak gelir. 

Cihaz Azure IOT Hub'ına bağlandığında istemci cihazdır ve sunucu IOT hub'ı bulut hizmetidir. IOT hub'ı bulut hizmeti olarak adlandırılan bir kök CA sertifikası tarafından desteklenen **Baltimore CyberTrust Root**, yaygın olarak kullanılan ve genel olarak kullanılabilir. IOT hub'ı CA sertifika çoğu cihazlarda zaten yüklenmiş olduğundan, birçok TLS uygulamaları (OpenSSL, Schannel, LibreSSL) sunucu sertifika doğrulaması sırasında otomatik olarak kullanır. IOT Hub'ına başarılı bir şekilde bağlanabilir bir cihaz bir IOT Edge ağ geçidi bağlantısı kurulurken sorunlarla karşılaşabilirsiniz.

Bir IOT Edge ağ geçidi için bir cihaz bağlandığında, aşağı akış cihaz istemci ve ağ geçidi cihazı sunucu. Azure IOT Edge, uygun gördüğünüz ancak ağ geçidi sertifika zincirleri oluşturmak işleçleri (veya kullanıcılar) sağlar. İşleç Baltimore gibi bir ortak CA sertifika veya otomatik olarak imzalanan (veya şirket içi) bir kök CA sertifikasını kullanın tercih edebilirsiniz. Ortak CA sertifikaları genellikle ilişkili bir maliyeti vardır, bu nedenle genellikle üretim senaryolarında kullanılır. CA sertifikaları otomatik olarak imzalanan geliştirme ve test için tercih edilir. Giriş listelenen saydam bir ağ geçidi Kurulumu makaleleri otomatik olarak imzalanan kök CA sertifikaları kullanın. 

Bir IOT Edge ağ geçidi için bir otomatik olarak imzalanan kök CA sertifikası kullandığınızda, üzerinde yüklü olması gerekir veya ağ geçidine bağlanmaya tüm aşağı akış cihazları için sağlanan. 

![Ağ geçidi sertifikası Kurulumu](./media/how-to-create-transparent-gateway/gateway-setup.png)

Bazı üretim uygulamalarını ve IOT Edge sertifikalar hakkında daha fazla bilgi için bkz: [IOT Edge sertifika kullanım ayrıntılarını](iot-edge-certs.md).

## <a name="provide-the-root-ca-certificate"></a>Kök CA sertifikası sağlayın

Ağ geçidi cihazın sertifikaları doğrulamak için kök CA sertifikasını kendi kopyasını aşağı akış cihaz gereksinim duyar. IOT Edge git deposunda sağlanan betikleri test sertifikalarınızı oluşturmak için kullanılan sonra kök CA sertifikasını adlı **azure-IOT-test-only.root.ca.cert.pem**. Bir aşağı akış aygıt hazırlık adımları bir parçası olarak zaten yapmadıysanız, bu sertifika dosyasını aşağı akış Cihazınızda herhangi bir dizinini taşıyın. Bir hizmet gibi kullanabilirsiniz [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault) veya bir işlev [güvenli kopya Protokolü](https://www.ssh.com/ssh/scp/) sertifika dosyayı taşır.

## <a name="install-certificates-in-the-os"></a>İşletim sisteminde sertifikalarını yükleme

Kök CA sertifikasını işletim sisteminin sertifika deposunda genellikle yükleme kök CA sertifikasını kullanmak, çoğu uygulama sağlar. Bazı özel durumlar vardır, işletim sistemi sertifika kullanmayın NodeJS uygulamaları gibi depolamak yerine düğüm çalışma zamanının iç sertifika deposunu kullanır Sertifika işletim sistemi düzeyinde yükleyemezse, atlayın [Azure IOT SDK'ları ile sertifikalarınız](#use-certificates-with-azure-iot-sdks). 

### <a name="ubuntu"></a>Ubuntu

Aşağıdaki komutlar bir Ubuntu konakta bir CA sertifikası yüklemek nasıl bir örnektir. Bu örnekte, kullanmakta olduğunuz varsayılır **azure-IOT-test-only.root.ca.cert.pem** sertifikadan önkoşulları makaleleri ve aşağı akış cihazdaki bir konuma sertifika kopyaladığınızdan emin.

```bash
sudo cp <path>/azure-iot-test-only.root.ca.cert.pem /usr/local/share/ca-certificates/azure-iot-test-only.root.ca.cert.pem.crt
sudo update-ca-certificates
```

"Güncelleştirme sertifikaları /etc/ssl/certs... belirten bir ileti görürsünüz. 1, 0 kaldırıldı eklendi; bitti."

### <a name="windows"></a>Windows

Aşağıdaki adımları, bir Windows konak üzerinde bir CA sertifikası yüklemek nasıl bir örnektir. Bu örnekte, kullanmakta olduğunuz varsayılır **azure-IOT-test-only.root.ca.cert.pem** sertifikadan önkoşulları makaleleri ve aşağı akış cihazdaki bir konuma sertifika kopyaladığınızdan emin.

1. Başlat menüsünde arayın ve seçin **bilgisayar sertifikalarını yönetme**. Bir hizmet olarak adlandırılan **certlm** açılır.
2. Gidin **sertifikalar - yerel bilgisayar** > **güvenilen kök sertifika yetkilileri**.
3. Sağ **sertifikaları** seçip **tüm görevler** > **alma**. Sertifika İçeri Aktarma Sihirbazı'nı başlatma. 
4. Yönergelerine uygun olarak aşağıdaki adımları uygulayın ve sertifika dosyasını içe `<path>/azure-iot-test-only.root.ca.cert.pem`. İşlem tamamlandığında bir "Başarıyla içeri aktarıldı" iletisini görmeniz gerekir. 

Bu makalenin devamındaki .NET örneğinde gösterildiği gibi sertifikaları .NET API'lerini program aracılığıyla kullanarak da yükleyebilirsiniz. 

Genellikle TLS yığın adlı sağlanan Windows uygulamaları kullanın [Schannel](https://docs.microsoft.com/windows/desktop/com/schannel) TLS üzerinden güvenli bir şekilde bağlanmak için. Schannel *gerektirir* sertifikalarını TLS bağlantı çalışmadan önce Windows sertifika deposuna yüklenmesi.

## <a name="use-certificates-with-azure-iot-sdks"></a>Azure IOT SDK'ları ile sertifikaları kullan

Bu bölümde, Azure IOT SDK'ları basit örnek uygulamalar kullanarak bir IOT Edge cihazı nasıl bağlanacağını açıklar. Tüm örnekler cihaz istemcisi bağlanma ve telemetri iletilerini göndermek için ağ geçidi, daha sonra bağlantıyı kapatmak ve çıkış hedefidir. 

Uygulama düzeyi örnekleri kullanmadan önce hazır iki şey vardır:

* Aşağı Akış cihazınızın IOT Hub bağlantı dizesine ağ geçidi cihazı için işaret edecek şekilde değiştirilebilir ve aşağı akış cihazınızın IOT hub'a kimlik doğrulaması için sertifikalar gerekir. Daha fazla bilgi için [Azure IOT hub'a bir aşağı akış cihaz kimlik doğrulaması](how-to-authenticate-downstream-device.md).

* Kök CA sertifikası kopyalanır ve herhangi bir aşağı akış Cihazınızda kaydedilmiş tam yolu.

    Örneğin, `<path>/azure-iot-test-only.root.ca.cert.pem`. 

### <a name="nodejs"></a>NodeJS

Bu bölümde, bir IOT Edge ağ geçidi için bir Azure IOT NodeJS cihaz istemcisi bağlanmak için örnek bir uygulama sağlar. NodeJS uygulamaları için aşağıda gösterildiği gibi uygulama düzeyinde kök CA sertifikasını yüklemeniz gerekir. NodeJS uygulamaları sistem sertifika deposu kullanmayın. 

1. Örneğe ilişkin edinme **edge_downstream_device.js** gelen [Node.js için Azure IOT cihaz SDK'sı örnekleri deposu](https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples). 
2. İnceleyerek örneği çalıştırmak için tüm önkoşullara sahip olduğunuzdan emin olun **readme.md** dosya. 
3. Edge_downstream_device.js dosyasında güncelleştirme **connectionString** ve **edge_ca_cert_path** değişkenleri. 
4. Cihazınızda örneği çalıştırmak yönergeler için SDK belgelerine bakın. 

Kullanmakta olduğunuz örnek anlamak için aşağıdaki kod parçacığını nasıl istemci SDK'sı sertifika dosyasını okur ve güvenli bir TLS bağlantı kurmak için kullandığı verilmiştir: 

```javascript
// Provide the Azure IoT device client via setOptions with the X509
// Edge root CA certificate that was used to setup the Edge runtime
var options = {
    ca : fs.readFileSync(edge_ca_cert_path, 'utf-8'),
};
```

### <a name="net"></a>.NET

Bu bölüm, bir IOT Edge ağ geçidi için bir Azure IOT .NET cihaz istemcisi bağlanmak için örnek bir uygulama tanıtır. Ancak, .NET uygulamaları otomatik olarak hem Linux hem de Windows konaklarda sistemin sertifika deposunda yüklü sertifikalarını kullanmanız mümkün değildir.

1. Örneğe ilişkin edinme **EdgeDownstreamDevice** gelen [IOT Edge .NET örnekler klasörüne](https://github.com/Azure/iotedge/tree/master/samples/dotnet/EdgeDownstreamDevice). 
2. İnceleyerek örneği çalıştırmak için tüm önkoşullara sahip olduğunuzdan emin olun **readme.md** dosya. 
3. İçinde **özellikleri / launchSettings.json** dosyası, güncelleştirme **DEVICE_CONNECTION_STRING** ve **CA_CERTIFICATE_PATH** değişkenleri. Konak sisteminde güvenilir sertifika deposunda yüklü sertifika kullanmak istiyorsanız, bu değişken boş bırakın. 
4. Cihazınızda örneği çalıştırmak yönergeler için SDK belgelerine bakın. 

Program aracılığıyla bir .NET uygulaması aracılığıyla sertifika deposundaki güvenilen bir sertifika yüklemek için başvurmak **InstallCACert()** işlevi **EdgeDownstreamDevice / Program.cs** dosya. Bu işlem, başka hiçbir etkisi olmadan aynı değerlerle birden çok kez çalıştırılabilir. Bu nedenle etkili olur. 

### <a name="c"></a>C

Bu bölüm, bir IOT Edge ağ geçidi için bir Azure IOT C cihaz istemcisi bağlanmak için örnek bir uygulama tanıtır. C SDK'sı OpenSSL WolfSSL ve Schannel dahil olmak üzere birçok TLS kitaplıkları ile çalışır. Daha fazla bilgi için [Azure IOT C SDK'sı](https://github.com/Azure/azure-iot-sdk-c). 

1. Alma **iotedge_downstream_device_sample** gelen uygulama [C örnekleri için Azure IOT cihaz SDK'sını](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples). 
2. İnceleyerek örneği çalıştırmak için tüm önkoşullara sahip olduğunuzdan emin olun **readme.md** dosya. 
3. İotedge_downstream_device_sample.c dosyasında güncelleştirme **connectionString** ve **edge_ca_cert_path** değişkenleri. 
4. Cihazınızda örneği çalıştırmak yönergeler için SDK belgelerine bakın. 

C için Azure IOT cihaz SDK'sını istemcisi kurarken bir CA sertifikası kaydetmek için bir seçenek sunar. Bu işlem, herhangi bir sertifika yüklemez, ancak bunun yerine sertifikanın bir dize biçimi bellekte kullanır. Kaydedilen sertifikayı, temel alınan TLS yığına bağlantı kurarken sağlanır. 

```C
(void)IoTHubDeviceClient_SetOption(device_handle, OPTION_TRUSTED_CERT, cert_string);
```

Windows konaklarda OpenSSL veya başka bir TLS kitaplığı kullanmıyorsanız SDK'sı varsayılan Schannel kullanarak. Schannel çalışmak üzere IOT Edge kök CA sertifikasını kullanarak ayarlanmamış Windows sertifika deposunda yüklenmelidir `IoTHubDeviceClient_SetOption` işlemi. 

### <a name="java"></a>Java

Bu bölüm, bir IOT Edge ağ geçidi için bir Azure IOT Java cihaz istemcisi bağlanmak için örnek bir uygulama tanıtır. 

1. Örneğe ilişkin edinme **gönderme olayı** gelen [Java örnekleri için Azure IOT cihaz SDK'sını](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples). 
2. İnceleyerek örneği çalıştırmak için tüm önkoşullara sahip olduğunuzdan emin olun **readme.md** dosya. 
3. Cihazınızda örneği çalıştırmak yönergeler için SDK belgelerine bakın.

### <a name="python"></a>Python

Bu bölüm, bir IOT Edge ağ geçidi için bir Azure IOT Python cihaz istemcisi bağlanmak için örnek bir uygulama tanıtır. 

1. Örneğe ilişkin edinme **edge_downstream_client** gelen [Python örnekleri için Azure IOT cihaz SDK'sını](https://github.com/Azure/azure-iot-sdk-python/tree/master/device/samples). 
2. İnceleyerek örneği çalıştırmak için tüm önkoşullara sahip olduğunuzdan emin olun **readme.md** dosya. 
3. Edge_downstream_client.py dosyasında güncelleştirme **CONNECTION_STRING** ve **TRUSTED_ROOT_CA_CERTIFICATE_PATH** değişkenleri. 
4. Cihazınızda örneği çalıştırmak yönergeler için SDK belgelerine bakın. 


## <a name="test-the-gateway-connection"></a>Ağ geçidi bağlantısını test etme

Bu, her şeyin doğru şekilde ayarlandığını gösterdiğinde, testleri bir örnek bir komuttur. "Tamam doğrulandı" belirten bir ileti görürsünüz.

```cmd/sh
openssl s_client -connect mygateway.contoso.com:8883 -CAfile <CERTDIR>/certs/azure-iot-test-only.root.ca.cert.pem -showcerts
```

## <a name="troubleshoot-the-gateway-connection"></a>Ağ geçidi bağlantısıyla ilgili sorunları giderme

Yaprak cihazınız aralıklı bağlantı, ağ geçidi cihazı varsa, çözüm için aşağıdaki adımları deneyin. 

1. Ağ geçidi ana bilgisayar adını bağlantı dizesinde ağ geçidi cihazı IOT Edge config.yaml dosyasında bir ana bilgisayar adı değeri ile aynı mı?
2. Ağ geçidi ana bilgisayar adını bir IP adresi olarak çözümlenebilir mi? Aralıklı bağlantı DNS kullanarak veya yaprak cihazda ana işlem dosya girdisi ekleyerek çözülebilir.
3. İletişim bağlantı noktalarını güvenlik duvarınızdan açık misiniz? İletişim tabanlı kullanılan protokol (MQTTS:8883 / AMQPS:5671 / HTTPS:433) aşağı akış cihaz arasında saydam bir IOT Edge mümkün olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

IOT Edge nasıl genişletebileceğinizi öğreneceğiz [çevrimdışı özellikleri](offline-capabilities.md) aşağı akış cihazlara. 
