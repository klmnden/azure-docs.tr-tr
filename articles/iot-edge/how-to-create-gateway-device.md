---
title: "Bir ağ geçidi cihazı ile Azure IOT sınır oluştur | Microsoft Docs"
description: "Birden çok aygıt için bilgileri işleyebilmesi için bir ağ geçidi aygıtı oluşturmak için Azure IOT kenar kullanın"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: e1337ddf5ed84a06a62e2faa198f3e8fb49bc3bd
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="create-an-iot-edge-gateway-device-to-process-data-from-other-iot-devices---preview"></a>Diğer IOT cihazlarından veri işlemek için bir IOT sınır ağ geçidi cihazı oluşturma - Önizleme

Ağ geçidi olarak IOT sınır cihazları kullanmanın iki yolu vardır:

* Kullanın aşağı akış cihazları bağlamak [Azure IOT cihaz SDK'sı][lnk-devicesdk];
* IOT hub'ı protokolü kullanmayan cihazları bağlayın.

Bir IOT sınır cihazı bir ağ geçidi olarak kullandığınızda, aşağıdaki avantajları alın:

* **Bağlantı çoğullama**. IOT Hub'ına IOT kenar cihazı üzerinden bağlanan tüm cihazlar aynı temel alınan bağlantı kullanır.
* **Trafik düzgünleştirme**. IOT sınır cihazı otomatik olarak IOT Hub'ın azaltma, iletileri yerel olarak kalıcı ederken kaybedildiği üstel geri alma gerçekleştireceksiniz. Bu, çözümünüzün dayanıklı trafiğinin ani için hale getirir.
* **Sınırlı Çevrimdışı Destek**. Ağ geçidi cihazı IOT Hub'ına teslim edilemeyen iletiler yerel olarak depolar.

Bu makalede diyoruz *IOT sınır ağ geçidi* bir ağ geçidi olarak kullanılan bir IOT sınır cihazı.

>[!NOTE]
>Şu anda:
> * Aşağı Akış cihazlar IOT Hub'ından ağ geçidi bağlantısı kesilmişse ağ geçidi ile kimlik doğrulaması için değildir; ve
> * IOT sınır cihazları için IOT uç ağ geçitlerine bağlanamıyor.

## <a name="use-the-azure-iot-device-sdk"></a>Azure IOT cihaz SDK'sını kullanın

Tüm IOT kenar cihazları yüklü kenar hub'ı aşağıdaki temelleri aşağı akış cihazlara sunar:

* cihaz Bulut ve bulut-cihaz iletilerini;
* doğrudan yöntemleri; ve
* cihaz çifti işlemleri.

>[!NOTE]
>Şu anda, Aşağı Akış aygıtları karşıya dosya yükleme IOT sınır ağ geçidi üzerinden bağlanırken kullanmak mümkün değildir.

Azure IOT cihaz SDK'sını kullanarak bir IOT sınır ağ geçidi aygıtları bağlandığınızda için gerekir:

* Aşağı akış aygıtı için ağ geçidi aygıtı hostname başvuran bir bağlantı dizesini ayarlamak; ve
* Aşağı Akış cihaz bağlantı kabul etmek için ağ geçidi aygıtı tarafından kullanılan sertifikanın güvendiğinden emin olun.

Denetim komut dosyası kullanarak Azure IOT kenar çalışma zamanı yükleme, öğretici yüklemek IOT edge'de sanal bir cihaz üzerinde üzerinde yaptığınız gibi kenar hub için bir sertifika oluşturulur [Windows] [ lnk-tutorial1-win] ve [Linux][lnk-tutorial1-lin]. Bu sertifika kenar hub tarafından gelen TLS bağlantıları kabul etmek için kullanılır ve aşağı akış aygıt tarafından ağ geçidi aygıtı Bağlanırken güvenilir olması gerekir.

Ağ geçidi aygıtı topolojiniz için gereken güven sağlayan herhangi bir sertifika altyapı oluşturabilirsiniz. Bu makalede, etkinleştirmek için kullanacağınız aynı sertifika Kurulumu varsayıyoruz [X.509 CA güvenlik] [ lnk-iothub-x509] IOT Hub ' hangi içerir belirli bir IOT hub'ına ilişkili bir X.509 CA sertifikasını (  *IOT hub sahibi CA*) ve IOT sınır cihazları yüklü bu CA ile imzalanmış sertifikalar, bir dizi.

>[!IMPORTANT]
>IOT kenar ve aşağı akış aygıtlar şu anda yalnızca kullanabilirsiniz [SAS belirteci] [ lnk-iothub-tokens] IOT Hub ile kimlik doğrulaması için. Sertifikaları yalnızca yaprak ve ağ geçidi aygıtı arasında TLS bağlantısını doğrulamak için kullanılır.

Bizim yapılandırmasını kullanan **IOT hub sahibi CA** olarak:
* IOT kenar çalışma zamanı, tüm IOT sınır cihazları ve olarak kurulum için bir imza sertifikası
* Aşağı Akış cihazları yüklü bir ortak anahtar sertifikası.

Bu, tüm IOT sınır cihazı bir ağ geçidi olarak kullanmak tüm cihazlar aynı IOT hub'ına bağlı olduğu sürece sağlayan bir çözümde sonuçlanır.

### <a name="create-the-certificates-for-test-scenarios"></a>Test senaryoları için sertifikaları oluşturma

Powershell örneği kullanabilirsiniz ve Bash betiklerini açıklanan [yönetme CA sertifikası örnek] [ lnk-ca-scripts] otomatik olarak imzalanan oluşturmak için **IOT hub sahibi CA** ve cihaz sertifikaları ile imzalanmış.

1. 1. adımını izleyin [yönetme CA sertifikası örnek] [ lnk-ca-scripts] komut dosyalarını yüklemek için.
2. Oluşturmak için 2. adım izleyin **IOT hub sahibi CA**, bu dosya, aşağı akış cihazlar tarafından bağlantıyı doğrulamak için kullanılacak.

Ağ geçidi cihazınız için bir sertifika oluşturmak için aşağıdaki yönergeleri kullanın.

#### <a name="bash"></a>Bash

* Çalıştırma `./certGen.sh create_edge_device_certificate myGateway` yeni bir cihaz sertifika oluşturmak için.  
  Bu ortak anahtar ve PFX ve cihazın özel anahtarı içeren.\private\new-edge-device.key.pem içeren dosyaları.\certs\new-edge-device.* oluşturur.  
* `cat new-edge-device.cert.pem azure-iot-test-only.intermediate.cert.pem azure-iot-test-only.root.ca.cert.pem > new-edge-device-full-chain.cert.pem`ortak anahtarı alınamadı.
* `./private/new-edge-device.cert.pem`cihazın özel anahtarı içerir.

#### <a name="powershell"></a>PowerShell

* Çalıştırma `New-CACertsEdgeDevice myGateway` yeni bir cihaz sertifika oluşturmak için.
  Bu dosyaları myEdgeDevice ortak anahtar, özel anahtarı ve bu sertifikanın PFX içeren * oluşturur.  İmzalama işlemi sırasında bir parola girmeniz istendiğinde, "1234" girin.


>[!IMPORTANT]
>Bu örnek yalnızca test amacıyla tasarlanmıştır. Üretim senaryoları için başvurmak [, IOT dağıtımınızın güvenliğini] [ lnk-iothub-secure-deployment] IOT çözümünüzü güvenli ve buna göre sertifikanızı sağlamak nasıl Azure IOT yönergeler.

### <a name="configure-an-iot-edge-device-as-a-gateway"></a>Bir IOT sınır cihazı bir ağ geçidi olarak yapılandırın

Bir ağ geçidi olarak IOT kenar Cihazınızı yapılandırmak için yalnızca önceki bölümde oluşturduğunuz cihaz sertifika kullanacak şekilde yapılandırmanız gerekir.

Yukarıdaki örnek komut dosyaları aşağıdaki dosya adlarından varsayıyoruz:

| Çıktı | Bash komut dosyası | PowerShell |
| ------ | ----------- | ---------- |
| Cihaz sertifikası | `certs/new-edge-device.cert.pem` | `certs/new-edge-device.cert.pem` |
| Aygıt özel anahtarı | `private/new-edge-device.cert.pem` | `private/new-edge-device.cert.pem` |
| Cihaz sertifika zinciri | `certs/new-edge-device-full-chain.cert.pem` | `certs/new-edge-device-full-chain.cert.pem` |
| IOT hub sahibi CA | `certs/azure-iot-test-only.root.ca.cert.pem` | `RootCA.pem` |

 Analogously sanal bir cihaz üzerinde dağıtmak Azure IOT Edge'de açıklanan yüklemeyi [Windows] [ lnk-tutorial1-win] veya [Linux][lnk-tutorial1-lin], gerekir IOT kenar çalışma zamanı için yukarıdaki bilgileri sağlayın. 
 
 Linux içinde:

        sudo iotedgectl setup --connection-string {device connection string}
            --edge-hostname {gateway hostname, e.g. mygateway.contoso.com}
            --device-ca-cert-file {full path}/certs/new-edge-device.cert.pem
            --device-ca-chain-cert-file {full path}/certs/new-edge-device-full-chain.cert.pem
            --device-ca-private-key-file {full path}/private/new-edge-device.key.pem
            --owner-ca-cert-file {full path}/certs/azure-iot-test-only.root.ca.cert.pem

Windows:

        iotedgectl setup --connection-string {device connection string}
            --edge-hostname {gateway hostname, e.g. mygateway.contoso.com}
            --device-ca-cert-file {full path}/certs/new-edge-device.cert.pem
            --device-ca-chain-cert-file {full path}/certs/new-edge-device-full-chain.cert.pem
            --device-ca-private-key-file {full path}/private/new-edge-device.key.pem
            --owner-ca-cert-file {full path}/RootCA.pem

Varsayılan örnek komut dosyaları aygıt özel anahtarı bir parola ayarlamayın. Bir parola ayarladıysanız, aşağıdaki parametreyi ekleyin:

        --device-ca-passphrase {passphrase}

Betik, kenar Aracı sertifikası için bir parola ayarlamak için sorar.
IOT kenar çalışma zamanı sonra bu komutu yeniden başlatmanız gerekir.

### <a name="configure-an-iot-hub-device-application-as-a-downstream-device"></a>Bir IOT Hub cihaz uygulaması bir aşağı akış aygıt olarak yapılandırma

Aşağı Akış cihaz herhangi bir uygulama olabilir kullanarak [Azure IOT cihaz SDK'sı][lnk-devicesdk], basit bir açıklandığı gibi [Cihazınızı .NET kullanarak IOT hub'ınıza bağlanmak] [ lnk-iothub-getstarted].

İlk olarak, güven bir aşağı akış cihaz uygulaması sahip **IOT hub sahibi CA** ağ geçidi aygıtlarını TLS bağlantılarını doğrulamak için sertifika. Bu adım genellikle iki yolla gerçekleştirilebilir: işletim sistemi düzeyinde veya (için belirli diller) uygulama düzeyinde.

Örneğin, .NET uygulamaları için aşağıdaki kod parçacığını yolunda saklanan PEM biçiminde bir sertifika güven ekleyebileceğiniz `certPath`.

        using System.Security.Cryptography.X509Certificates;
        
        ...

        X509Store store = new X509Store(StoreName.Root, StoreLocation.CurrentUser);
        store.Open(OpenFlags.ReadWrite);
        store.Add(new X509Certificate2(X509Certificate2.CreateFromCertFile(certPath)));
        store.Close();

Yukarıda başvurulan örnek komut dosyasında, ortak anahtarı oluşturur Not `certs/azure-iot-test-only.root.ca.cert.pem` (Bash) veya `RootCA.pem` (Powershell).

İşletim sistemi düzeyinde bu adımı gerçekleştirmenin arasında Windows ve Linux dağıtımları arasında farklılık gösterir.

İkinci adım, ağ geçidi aygıtı konak adına başvuran bir bağlantı dizesini IOT Hub cihaz SDK'sını başlatma olacak.
Bu ekleyerek yapılır `GatewayHostName` özelliği, cihaz bağlantı dizesi. Örneğin, bir örnek cihaz bağlantı dizesi eklenmiş biz bir aygıt için işte `GatewayHostName` özelliği:

        HostName=yourHub.azure-devices-int.net;DeviceId=yourDevice;SharedAccessKey=2BUaYca45uBS/O1AsawsuQslH4GX+SPkrytydWNdFxc=;GatewayHostName=mygateway.contoso.com

Bu iki adımı için ağ geçidi aygıtı bağlanmak cihaz uygulamanız olanak sağlar.

## <a name="use-other-protocols"></a>Diğer protokolleri kullanmak

Bir ağ geçidi cihazı ana işlevlerini aşağı akış aygıtların protokoller yorumlama biridir. IOT hub'ı protokolü kullanmayan cihazları bağlamak için bu çevirisi gerçekleştirir ve aşağı akış cihaz IOT Hub'ına adına bağlayan bir IOT kenar modülü geliştirebilirsiniz.

Oluşturmaya karar verebilirsiniz bir *saydam* veya *donuk* ağ geçidi:

| &nbsp; | Saydam ağ geçidi | Donuk ağ geçidi|
|--------|-------------|--------|
| IOT Hub kimlik kayıt defterinde depolanan kimlikleri | Tüm bağlı cihazların kimlikleri | Yalnızca ağ geçidi cihaz kimliği |
| Cihaz çifti | Kendi cihaz çifti bağlı her cihazın vardır | Yalnızca ağ geçidi aygıtı ve modülü çiftlerini vardır |
| Doğrudan yöntemleri ve bulut-cihaz iletilerini | Bulut bağlı her aygıt ayrı ayrı ele alabilir | Bulut yalnızca ağ geçidi aygıtı adres |
| [IOT hub'ı kısıtlamaları ve kotaları][lnk-iothub-throttles-quotas] | Her bir aygıtı Uygula | Ağ geçidi aygıtı Uygula |

Donuk ağ geçidi deseni kullanılırken, bu ağ geçidi üzerinden bağlanan tüm cihazlar en çok 50 iletileri içerebilir aynı bulut cihaz sırası paylaşır. Donuk ağ geçidi deseni yalnızca çok az aygıtları her alan ağ geçidi üzerinden bağlanan ve kendi bulut cihaz trafiğinin düşük olduğu zaman kullanılmalıdır olduğunu izler.

Genel olmayan bir ağ geçidi uygularken, protokol çevirisi modülünüzün modülü bağlantı dizesini kullanır.

Saydam bir ağ geçidi uygularken, modül aşağı akış aygıtlar için bağlantı dizeleri kullanılarak IOT Hub cihaz istemcisi birden çok örneğini oluşturur.

## <a name="next-steps"></a>Sonraki adımlar

- [IOT kenar modülleri geliştirmek için Araçlar ve gereksinimleri anlamanız][lnk-module-dev].

[lnk-devicesdk]: ../iot-hub/iot-hub-devguide-sdks.md
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
[lnk-module-dev]: module-development.md
[lnk-iothub-getstarted]: ../iot-hub/iot-hub-csharp-csharp-getstarted.md
[lnk-iothub-x509]: ../iot-hub/iot-hub-x509ca-overview.md
[lnk-iothub-secure-deployment]: ../iot-hub/iot-hub-security-deployment.md
[lnk-iothub-tokens]: ../iot-hub/iot-hub-devguide-security.md#security-tokens 
[lnk-iothub-throttles-quotas]: ../iot-hub/iot-hub-devguide-quotas-throttling.md
[lnk-iothub-devicetwins]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-iothub-c2d]: ../iot-hub/iot-hub-devguide-messages-c2d.md
[lnk-ca-scripts]: https://github.com/Azure/azure-iot-sdk-c/blob/CACertToolEdge/tools/CACertificates/CACertificateOverview.md