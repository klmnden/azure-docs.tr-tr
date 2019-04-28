---
title: Aşağı Akış cihazları - Azure IOT Edge bağlayın | Microsoft Docs
description: Aşağı yönde yapılandırma veya yaprak cihazların Azure IOT Edge ağ geçidi cihazları aracılığıyla bağlanın.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/01/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 5a05b8f0f9484ea49fbfb0bbe8818aa9cd0d66ee
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62126435"
---
# <a name="connect-a-downstream-device-to-an-azure-iot-edge-gateway"></a>Bir Azure IOT Edge ağ geçidi için bir aşağı akış cihazı bağlayın

Azure IOT Edge, bir veya daha fazla cihaz kendi iletileri IOT hub'ına bağlantı tutan bir tek ağ geçidi cihazı aracılığıyla geçirebilirsiniz saydam bir ağ geçidi senaryolarını etkinleştirir. Yapılandırılmış ağ geçidi cihazı aldıktan sonra aşağı akış cihazları güvenli bir şekilde bağlanma bilmeniz gerekir. 

Bu makalede, aşağı akış cihaz bağlantıları olan yaygın sorunlar tanımlanmakta ve aşağı akış cihazları tarafından ayarlama işleminde size yol gösterir: 

* Aktarım Katmanı Güvenliği (TLS) açıklayan ve sertifika ile ilgili temel bilgiler. 
* TLS kitaplıkları farklı işletim sistemleri arasında nasıl çalıştığı ve her işletim sistemi sertifikaları ile nasıl ilgileneceğini açıklar.
* Azure IOT walking başlamanıza yardımcı olmak üzere çeşitli dillerde örnekleri başlatıldı. 

Bu makalede, koşulları *ağ geçidi* ve *IOT Edge ağ geçidi* saydam bir ağ geçidi olarak yapılandırılmış bir IOT Edge cihazı bakın. 

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları izlemeden önce iki cihazınız kullanıma hazır olmanız gerekir:

1. IOT Edge cihazı saydam bir ağ geçidi olarak ayarlama. 
    [Saydam bir ağ geçidi olarak görev yapacak bir IOT Edge cihazı yapılandırma](how-to-create-transparent-gateway.md)

    Yapılandırılmış, ağ geçidi cihazı aldıktan sonra kopyalama **azure-IOT-test-only.root.ca.cert.pem** ağ geçidini, sertifika ve varsa kullanılabilir herhangi bir aşağı akış Cihazınızda. 

2. IOT Hub'ından bir cihaz kimliği olan bir aşağı akış cihazda. 
    IOT Edge cihazı aşağı akış cihazı olarak kullanamazsınız. Bunun yerine, normal bir IOT cihaz IOT hub'da kayıtlı bir cihaz kullanın. Portalda yeni bir cihaz kaydedebilirsiniz **IOT cihazları** bölümü. Veya Azure CLI'yı kullanabilirsiniz [cihaz kaydetme](../iot-hub/quickstart-send-telemetry-c.md#register-a-device). Bağlantı dizesini kopyalayın ve bu sonraki bölümlerde kullanılabilir. 

    Şu anda yalnızca simetrik anahtarlı kimlik doğrulamayı aşağı akış cihazlarıyla IOT Edge ağ geçidi bağlanabilirsiniz. X.509 sertifika yetkililerini ve otomatik olarak imzalanan X.509 sertifikaları şu anda desteklenmiyor.
    
> [!NOTE]
> "Bu makalede kullanılan ağ geçidi adı" aynı olması gerekir, IOT Edge config.yaml dosyanızdaki ana bilgisayar adı olarak kullanılan ad. Ağ geçidi adı, IP DNS veya ana işlem dosya girdisi kullanarak bir adresine çözülebilir olması gerekir. İletişim tabanlı kullanılan protokol (MQTTS:8883 / AMQPS:5671 / HTTPS:433) aşağı akış cihaz ile IOT Edge transparant arasında mümkün olması gerekir. Bir güvenlik duvarı arasında ise, ilgili bağlantı noktasının açık olması gerekir.

## <a name="prepare-a-downstream-device"></a>Bir aşağı akış cihazı hazırlama

Bir aşağı akış cihaz herhangi bir uygulama veya ile oluşturulan bir kimliğe sahiptir platform olabilir [Azure IOT hub'ı](https://docs.microsoft.com/azure/iot-hub) bulut hizmeti. Çoğu durumda, bu uygulamaları kullanma [Azure IOT cihaz SDK'sını](../iot-hub/iot-hub-devguide-sdks.md). Tüm pratik amacıyla bir aşağı akış cihazın IOT Edge ağ geçidi cihazın kendisinde çalışan bir uygulama bile olabilir. 

Bir IOT Edge ağ geçidi için bir aşağı akış cihaza bağlanmak için iki şey gerekir:

1. Bir cihaz veya uygulama ağ geçidine bağlanmak için gereken bilgileri ile eklenmiş bir IOT Hub cihaz bağlantı dizesi ile yapılandırılmış. 

    Bağlantı dizesi gibi biçimlendirilir: `HostName=yourHub.azure-devices.net;DeviceId=yourDevice;SharedAccessKey=XXXYYYZZZ=;`. Append **GatewayHostName** özelliği ile ağ geçidi cihazı bağlantı dizesinin sonuna konak adı. Değerini **GatewayHostName** değerinin eşleşmesi **hostname** ağ geçidi cihazın config.yaml dosyasında. 

    Son dize şuna benzer: `HostName=yourHub.azure-devices.net;DeviceId=yourDevice;SharedAccessKey=XXXYYYZZZ=;GatewayHostName=mygateway.contoso.com`.

2. Cihaz veya uygulama ağ geçidinin güvenmeyi sahip **kök CA** veya **sahibi CA** ağ geçidi cihazı TLS bağlantılarını doğrulamak için sertifika. 

    Daha karmaşık Bu adım, bu makalenin geri kalanında ayrıntılı açıklanmıştır. Bu adımda gerçekleştirilen iki yöntemden biri olabilir: Azure IOT SDK'larını kullanarak uygulamalar içindeki sertifika başvuran bir CA sertifikası işletim sisteminin sertifika deposunda veya (Belirli diller) yükleyerek.

## <a name="tls-and-certificate-fundamentals"></a>TLS ve sertifika ile ilgili temel bilgiler

İnternet üzerinden oluşan yalnızca herhangi diğer güvenli istemci/sunucu iletişimini güvenli bir aşağı akış cihazları IOT Edge bağlanma zorluklarıyla gibidir. Bir istemci ve sunucu güvenli bir şekilde kullanarak internet üzerinden iletişim [Aktarım Katmanı Güvenliği (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security). TLS standardı kullanılarak oluşturulan [ortak anahtar altyapısı (PKI)](https://en.wikipedia.org/wiki/Public_key_infrastructure) sertifikaları yapılar çağırılır. TLS oldukça karmaşık bir özelliğidir ve çok çeşitli iki uç nokta güvenliğini sağlamak için ilgili konuları ele ancak cihazların bir IOT Edge ağ geçidi için güvenli bir şekilde bağlanmak için gereken aşağıdaki bölümde kısaca açıklanmıştır.

Bir istemci bir sunucuya bağlandığında, sunucu adı verilen sertifikaları zincirine sunar *sunucu sertifika zinciri*. Bir sertifika zinciri, genellikle bir kök sertifika yetkilisi (CA) sertifikası, bir veya daha fazla ara CA sertifikalarının ve son olarak sunucunun sertifikanın kendisi oluşur. Bir istemci, şifreli olarak tüm sunucu sertifika zinciri doğrulayarak sunucusuyla güven oluşturur. Bu istemci doğrulama, sunucu sertifika zinciri olarak adlandırılan *sunucu kimlik doğrulaması*. Sunucu sertifika zinciri doğrulamak için bir istemci bir kopyasını oluşturun (veya sorun) sunucu sertifikasının kullanılan kök CA sertifikası gerekiyor. İstemci sorunsuz bir işlem bu normalde Web sitelerine bağlanmak, bir tarayıcı ile yaygın olarak kullanılan CA sertifikalarını önceden yapılandırılmış olarak gelir. 

Cihaz Azure IOT Hub'ına bağlandığında istemci cihazdır ve sunucu IOT hub'ı bulut hizmetidir. IOT hub'ı bulut hizmeti olarak adlandırılan bir kök CA sertifikası tarafından desteklenen **Baltimore CyberTrust Root**, yaygın olarak kullanılan ve genel olarak kullanılabilir. IOT hub'ı CA sertifika çoğu cihazlarda zaten yüklenmiş olduğundan, birçok TLS uygulamaları (OpenSSL, Schannel, LibreSSL) sunucu sertifika doğrulaması sırasında otomatik olarak kullanır. IOT Hub'ına başarılı bir şekilde bağlanabilir bir cihaz bir IOT Edge ağ geçidi bağlantısı kurulurken sorunlarla karşılaşabilirsiniz.

Bir IOT Edge ağ geçidi için bir cihaz bağlandığında, aşağı akış cihaz istemci ve ağ geçidi cihazı sunucu. Azure IOT Edge, uygun gördüğünüz ancak ağ geçidi sertifika zincirleri oluşturmak işleçleri (veya kullanıcılar) sağlar. İşleç Baltimore gibi bir ortak CA sertifika veya otomatik olarak imzalanan (veya şirket içi) bir kök CA sertifikasını kullanın tercih edebilirsiniz. Ortak CA sertifikaları genellikle ilişkili bir maliyeti vardır, bu nedenle genellikle üretim senaryolarında kullanılır. CA sertifikaları otomatik olarak imzalanan geliştirme ve test için tercih edilir. Önkoşullar bölümünde listelenen saydam bir ağ geçidi Kurulumu makaleleri otomatik olarak imzalanan kök CA sertifikaları kullanın. 

Bir IOT Edge ağ geçidi için bir otomatik olarak imzalanan kök CA sertifikası kullandığınızda, üzerinde yüklü olması gerekir veya ağ geçidine bağlanmaya tüm aşağı akış cihazları için sağlanan. 

Bazı üretim uygulamalarını ve IOT Edge sertifikalar hakkında daha fazla bilgi için bkz: [IOT Edge sertifika kullanım ayrıntılarını](iot-edge-certs.md).

## <a name="install-certificates-using-the-os"></a>İşletim sistemini kullanarak sertifikalarını yükleme

Bu makalede *sahibi CA* , önkoşul ağ geçidi makalesindeki betikler tarafından kullanılan terim olduğundan kök CA sertifika için başvurmak için. 

Sahibi CA sertifikası işletim sisteminin sertifika deposunda genellikle yükleme sahibi kullanmak için çoğu uygulama CA sertifikası sağlar. İşletim sistemi sertifika deposunu kullanmak yok, ancak bunun yerine düğüm çalışma zamanının iç sertifika deposunu kullanmak NodeJS özellikli gibi bazı özel durumlar vardır. Sertifika işletim sistemi düzeyinde yükleyemezse, uygulamalarında Azure IOT SDK'sı ile sertifikayı kullanmak için bu makalenin sonraki bölümlerinde dile özgü örneklerde bakın. 

### <a name="ubuntu"></a>Ubuntu

Aşağıdaki komutlar bir Ubuntu konakta bir CA sertifikası yüklemek nasıl bir örnektir. Bu örnek, kullandığınız varsayılır **azure-IOT-test-only.root.ca.cert.pem** sertifikadan önkoşulları makaleleri ve aşağı akış cihazdaki bir konuma sertifika kopyaladığınızdan emin.  

```bash
sudo cp <path>/azure-iot-test-only.root.ca.cert.pem /usr/local/share/ca-certificates/azure-iot-test-only.root.ca.cert.pem.crt
sudo update-ca-certificates
```

"Güncelleştirme sertifikaları /etc/ssl/certs... belirten bir ileti görürsünüz. 1, 0 kaldırıldı eklendi; bitti."

### <a name="windows"></a>Windows

Aşağıdaki adımları, bir Windows konak üzerinde bir CA sertifikası yüklemek nasıl bir örnektir. Bu örnek, kullandığınız varsayılır **azure-IOT-test-only.root.ca.cert.pem** sertifikadan önkoşulları makaleleri ve aşağı akış cihazdaki bir konuma sertifika kopyaladığınızdan emin.  

1. Başlat menüsünde arayın ve seçin **bilgisayar sertifikalarını yönetme**. Bir hizmet olarak adlandırılan **certlm** açılır.
2. Gidin **sertifikalar - yerel bilgisayar** > **güvenilen kök sertifika yetkilileri**.
3. Sağ **sertifikaları** seçip **tüm görevler** > **alma**. Sertifika İçeri Aktarma Sihirbazı'nı başlatma. 
4. Yönergelerine uygun olarak aşağıdaki adımları uygulayın ve sertifika dosyasını içe `<path>/azure-iot-test-only.root.ca.cert.pem`. İşlem tamamlandığında bir "Başarıyla içeri aktarıldı" iletisini görmeniz gerekir. 

Bu makalenin devamındaki .NET örneğinde gösterildiği gibi sertifikaları .NET API'lerini program aracılığıyla kullanarak da yükleyebilirsiniz. 

Genellikle TLS yığın adlı sağlanan Windows uygulamaları kullanın [Schannel](https://docs.microsoft.com/windows/desktop/com/schannel) TLS üzerinden güvenli bir şekilde bağlanmak için. Schannel *gerektirir* sertifikalarını TLS bağlantı çalışmadan önce Windows sertifika deposuna yüklenmesi.

## <a name="use-certificates-with-azure-iot-sdks"></a>Azure IOT SDK'ları ile sertifikaları kullan

Bu makalede kök CA sertifikası olarak başvurur *sahibi CA* , Önkoşullar makaleler, otomatik olarak imzalanan sertifika oluşturma komut dosyaları tarafından kullanılan terim olduğundan. 

Bu bölümde, Azure IOT SDK'ları basit örnek uygulamalar kullanarak bir IOT Edge cihazı nasıl bağlanacağını açıklar. Tüm örnekler cihaz istemcisi bağlanma ve telemetri iletilerini göndermek için ağ geçidi, daha sonra bağlantıyı kapatmak ve çıkış hedefidir. 

### <a name="common-concepts-across-all-azure-iot-sdks"></a>Tüm Azure IOT SDK'ları arasında ortak kavramlar

Uygulama düzeyi örnekleri kullanmadan önce hazır iki şey vardır:

1. Aşağı Akış cihazınızın IOT Hub bağlantı dizesine ağ geçidi cihazı için işaret edecek şekilde değiştirildi.

    Bağlantı dizesi gibi biçimlendirilir: `HostName=yourHub.azure-devices.net;DeviceId=yourDevice;SharedAccessKey=XXXYYYZZZ=;`. Append **GatewayHostName** özelliği ile ağ geçidi cihazı bağlantı dizesinin sonuna konak adı. Değerini **GatewayHostName** değerinin eşleşmesi **hostname** ağ geçidi cihazın config.yaml dosyasında. 

    Son dize şuna benzer: `HostName=yourHub.azure-devices.net;DeviceId=yourDevice;SharedAccessKey=XXXYYYZZZ=;GatewayHostName=mygateway.contoso.com`.

2. Kök CA sertifikası kopyalanır ve herhangi bir aşağı akış Cihazınızda kaydedilmiş tam yolu.

    Örneğin, `<path>/azure-iot-test-only.root.ca.cert.pem`. 

### <a name="nodejs"></a>NodeJS

Bu bölümde, bir IOT Edge ağ geçidi için bir Azure IOT NodeJS cihaz istemcisi bağlanmak için örnek bir uygulama sağlar. NodeJS uygulamaları sistem sertifika deposu kullanma gibi konakları, Linux ve Windows için uygulama düzeyinde kök CA sertifikasını burada gösterildiği gibi yüklemeniz gerekir. 

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

Her şeyi olmuştur hangi testlerin ayarlanmış bir örnek komut doğru budur. "Tamam doğrulandı" belirten bir ileti görürsünüz.

```cmd/sh
openssl s_client -connect mygateway.contoso.com:8883 -CAfile <CERTDIR>/certs/azure-iot-test-only.root.ca.cert.pem -showcerts
```

## <a name="troubleshoot-the-gateway-connection"></a>Ağ geçidi bağlantısıyla ilgili sorunları giderme

Yaprak cihazınız aralıklı bağlantı, ağ geçidi cihazı varsa, çözüm için aşağıdaki adımları deneyin. 

1. Eklenen bağlantı ağ geçidi adı dize ağ geçidi cihazı IOT Edge config.yaml dosyasında bir ana bilgisayar adı ile aynı mı?
2. Ağ geçidi adı, bir IP adresi olarak çözümlenebilir nedir? DNS kullanarak veya yaprak cihazda ana işlem dosya girdisi ekleyerek intenmittent bağlantıları çözümleyebilir.
3. İletişim bağlantı noktalarını güvenlik duvarınızdan açık misiniz? İletişim tabanlı kullanılan protokol (MQTTS:8883 / AMQPS:5671 / HTTPS:433) aşağı akış cihaz ile IOT Edge transparant arasında mümkün olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

IOT Edge nasıl genişletebileceğinizi öğreneceğiz [çevrimdışı özellikleri](offline-capabilities.md) aşağı akış cihazlara. 
