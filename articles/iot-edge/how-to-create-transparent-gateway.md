---
title: Saydam bir ağ geçidi cihazı - Azure IOT Edge oluştur | Microsoft Docs
description: Azure IOT Edge cihazı, aşağı akış cihazlardan bilgi işleyebilen saydam bir ağ geçidi olarak kullanma
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/07/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 5881adb7e2fc0d52cc2037d3d4a9e986b3e29d74
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67058364"
---
# <a name="configure-an-iot-edge-device-to-act-as-a-transparent-gateway"></a>Saydam bir ağ geçidi olarak görev yapacak bir IOT Edge cihazı yapılandırma

Bu makalede, diğer cihazlar IOT Hub ile iletişim kurmak için saydam bir ağ geçidi olarak çalışması için bir IOT Edge cihazı yapılandırmaya ayrıntılı yönergeleri sağlar. Bu makalede, terim *IOT Edge ağ geçidi* saydam bir ağ geçidi olarak kullanılan bir IOT Edge cihazı gösterir. Daha fazla bilgi için [nasıl bir IOT Edge cihazı ağ geçidi olarak kullanılabilir](./iot-edge-as-gateway.md).

>[!NOTE]
>Şu anda:
> * Edge özellikli cihazlar IOT Edge ağ geçidi için bağlantı kurulamıyor. 
> * Aşağı Akış cihazları karşıya dosya yükleme kullanamazsınız.

Başarılı saydam bir ağ geçidi bağlantısı kurmak için üç genel adımlar vardır. Bu makalede, ilk adımı kapsar:

1. **Ağ geçidi cihazı, güvenli bir aşağı akış cihazlara bağlanın, aşağı akış cihazlardan iletişimleri almak ve iletileri yönlendirmek için uygun hedef olması gerekiyor.**
2. Aşağı Akış cihaz IOT Hub ile kimlik doğrulaması ve kendi ağ geçidi cihazı iletişim kurmak için bilmeniz yapabilmek için bir cihaz kimliği olmalıdır. Daha fazla bilgi için [Azure IOT hub'a bir aşağı akış cihaz kimlik doğrulaması](how-to-authenticate-downstream-device.md).
3. Aşağı Akış cihaz, ağ geçidi cihazı için güvenli bir şekilde bağlanabilmesi gerekir. Daha fazla bilgi için [bir Azure IOT Edge ağ geçidi için aşağı akış cihaz Bağlayamama](how-to-connect-downstream-device.md).


Bir cihaz bir ağ geçidi olarak çalışması, aşağı akış, cihazlara güvenli bir şekilde bağlanabilir olması gerekir. Azure IOT Edge cihazları arasında güvenli bağlantılar kurmak için bir ortak anahtar altyapısı (PKI) kullanmanıza olanak tanır. Bu durumda, biz saydam bir ağ geçidi olarak görev yapan bir IOT Edge cihazına bağlamak için bir aşağı akış cihazı vermiş olursunuz. Makul güvenliğini sağlamak için aşağı akış cihaz kimliğini ağ geçidi cihazı onaylamalıdır. Bu kimlik denetimi, cihazlarınız için kötü amaçlı ağ geçitleri bağlanmasını engeller.

Bir aşağı akış cihaz herhangi bir uygulama veya ile oluşturulan bir kimliğe sahiptir platform olabilir [Azure IOT hub'ı](https://docs.microsoft.com/azure/iot-hub) bulut hizmeti. Çoğu durumda, bu uygulamaları kullanma [Azure IOT cihaz SDK'sını](../iot-hub/iot-hub-devguide-sdks.md). Tüm pratik amacıyla bir aşağı akış cihazın IOT Edge ağ geçidi cihazın kendisinde çalışan bir uygulama bile olabilir. 

Ağ geçidi cihazı topolojiniz için gerekli güven sağlayan herhangi bir sertifika altyapısı oluşturabilirsiniz. Bu makalede, etkinleştirmek için kullanacağınız aynı sertifika Kurulumu varsayıyoruz [X.509 CA güvenlik](../iot-hub/iot-hub-x509ca-overview.md) IOT Hub'ında bir belirli IOT hub'a (IOT hub'ı kök CA), bir dizi imzalanmış sertifikalar ilişkili bir X.509 CA sertifikası kapsamaktadır Bu CA ve IOT Edge cihazı için bir CA ile.

![Ağ geçidi sertifikası Kurulumu](./media/how-to-create-transparent-gateway/gateway-setup.png)

>[!NOTE]
>"Kök Bu makale boyunca kullanılan CA" terimi, üstteki yetkilisi ortak sertifika PKI sertifika zinciri ve mutlaka dağıtılmış sertifika yetkilisinin sertifikayı kök ifade eder. Çoğu durumda, aslında bir ara CA ortak sertifikasını olduğu. 

Ağ geçidi, IOT Edge cihaz CA sertifikası aşağı akış cihaza bağlantının başlatma sırasında sunar. IOT Edge cihaz CA sertifikası kök CA sertifikası tarafından imzalanmış emin olmak için aşağı akış cihaz denetler. Bu işlem, ağ geçidi güvenilir bir kaynaktan geldiğini doğrulamak aşağı akış cihaz sağlar.

Aşağıdaki adımlarda sertifikaları oluşturma ve bunları doğru yerlerde ağ geçidi yükleme işleminde size yol. Herhangi bir makine sertifikalarını oluşturmak için kullanın ve ardından bunları IOT Edge cihazınıza kopyalayabilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar

Bir ağ geçidi olarak yapılandırmak için Azure IOT Edge cihazı. IOT Edge yükleme adımlarını aşağıdaki işletim sistemlerinden birini kullanın:
  * [Windows](./how-to-install-iot-edge-windows.md)
  * [Linux x64](./how-to-install-iot-edge-linux.md)
  * [Linux ARM32](./how-to-install-iot-edge-linux-arm.md)

Bu makalede başvurduğu *ağ geçidi ana bilgisayar adı* çeşitli noktalarda. Ağ geçidi ana bilgisayar adını, bildirilen **hostname** IOT Edge ağ geçidi cihazı config.yaml dosyada parametresi. Bu makalede sertifikaları oluşturmak için kullanılır ve aşağı akış cihazların bağlantı dizesinde adlandırılır. Ağ geçidi ana bilgisayar IP DNS veya ana işlem dosya girdisi kullanarak bir adresine çözülebilir olması gerekir.

## <a name="generate-certificates-with-windows"></a>Windows ile sertifikalar oluşturma

Windows üzerinde test sertifikalarınızı oluşturmak için bu bölümdeki adımları kullanın. Windows Makine sertifikalarını oluşturmak için kullanın ve ardından bunları üzerinden tüm desteklenen işletim sistemi üzerinde çalışan herhangi bir IOT Edge cihazı kopyalayın. 

Bu bölümde oluşturulan sertifikaları yalnızca test amaçlarına yöneliktir. 

### <a name="install-openssl"></a>OpenSSL yükleyin

OpenSSL için Windows sertifikalarını oluşturmak için kullanmakta olduğunuz makineye yükleyin. Windows Cihazınızda yüklü OpenSSL zaten varsa bu adımı atlayın, ancak bu openssl.exe uygulamanız yol ortam değişkeninde kullanılabilir olduğundan emin. 

OpenSSL yükleyebileceğiniz birkaç yolu vardır:

* **Daha kolay:** İndirin ve yükleyin [üçüncü taraf OpenSSL ikili](https://wiki.openssl.org/index.php/Binaries), örneğin, gelen [SourceForge üzerinde OpenSSL](https://sourceforge.net/projects/openssl/). Tam yolunu openssl.exe, PATH ortam değişkenine ekleyin. 
   
* **Önerilen:** OpenSSL kaynak kodunu indirebilir ve ikili dosyaları makinenizde sizin tarafınızdan veya aracılığıyla derleme [vcpkg](https://github.com/Microsoft/vcpkg). Aşağıda listelenen yönergeleri vcpkg kaynak kodu, derleme ve yükleme OpenSSL kolay adımda Windows makinenize indirmek için kullanın.

   1. Vcpkg yüklemek istediğiniz bir dizine gidin. Bu dizine başvuracağınız  *\<VCPKGDIR >* . İndirme ve yükleme yönergelerini [vcpkg](https://github.com/Microsoft/vcpkg).
   
   2. Bir kez x64 Windows için OpenSSL paketi yüklemek için bir powershell isteminden aşağıdaki komutu çalıştırarak vcpkg yüklenir. Yükleme, genellikle tamamlanması yaklaşık 5 dakika sürer.

      ```powershell
      .\vcpkg install openssl:x64-windows
      ```
   3. Ekleme `<VCPKGDIR>\installed\x64-windows\tools\openssl` , PATH ortam değişkenine böylece openssl.exe dosya çağırma için kullanılabilir.

### <a name="prepare-creation-scripts"></a>Oluşturma betikleri hazırlama

Azure IOT Edge git deposu test sertifikalarınızı oluşturmak için kullanabileceğiniz betikleri içerir. Bu bölümde, IOT Edge depoyu kopyalayalım ve betikleri yürütme. 

1. Yönetici modunda bir PowerShell penceresi açın. 

2. Üretim dışı sertifikalarını oluşturmak için komut dosyalarını içeren bir git deposunu kopyalayın. Bu betikler, saydam bir ağ geçidini ayarlamak için gerekli sertifikaları oluşturmanıza yardımcı olur. Kullanım `git clone` komutu veya [ZIP indir](https://github.com/Azure/iotedge/archive/master.zip). 

   ```powershell
   git clone https://github.com/Azure/iotedge.git
   ```

3. Çalışmak istediğiniz dizine gidin. Bu makale boyunca bu dizin sizi ararız  *\<WRKDIR >* . Tüm sertifikaları ve anahtarları bu çalışma dizininde oluşturulur.

4. Yapılandırma ve komut dosyaları kopyalanan depodan çalışma dizininize kopyalayın. 

   ```powershell
   copy <path>\iotedge\tools\CACertificates\*.cnf .
   copy <path>\iotedge\tools\CACertificates\ca-certs.ps1 .
   ```

   Depo bir ZIP olarak indirdiğiniz sonra klasör adı `iotedge-master` ve yolun geri kalanı aynıdır. 
<!--
5. Set environment variable OPENSSL_CONF to use the openssl_root_ca.cnf configuration file.

    ```powershell
    $env:OPENSSL_CONF = "$PWD\openssl_root_ca.cnf"
    ```
-->
5. PowerShell komut dosyalarını çalıştırmak etkinleştirin.

   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser
   ```

7. PowerShell'in genel ad alanına betikler tarafından kullanılan işlevler getirin.
   
   ```powershell
   . .\ca-certs.ps1
   ```

   PowerShell penceresinde bu betiği tarafından oluşturulan sertifikaları yalnızca test amaçlı olan ve üretim senaryolarında kullanılmaması gereken bir uyarı görüntüler.

8. OpenSSL doğru şekilde yüklendiğini doğrulayın ve var olan sertifikalar ile ad çakışmalarını olmayacak emin olun. İlgili sorun varsa betiği nasıl düzeltileceğini sisteminizde açıklamalıdır.

   ```powershell
   Test-CACertsPrerequisites
   ```

### <a name="create-certificates"></a>Sertifika oluşturma

Bu bölümde, üç sertifikaları oluşturma ve bunları bir zincirinde bağlayın. Sertifika zinciri dosyasında yerleştirme, bunları, IOT Edge ağ geçidi cihazı ve herhangi bir aşağı akış cihazlarda kolayca yüklemenize olanak tanır.  

1. Kök CA sertifikasını oluşturabilir ve bunu bir ara sertifika sağlayabilirsiniz. Sertifikaları tümünü çalışma dizininize yerleştirilir.

   ```powershell
   New-CACertsCertChain rsa
   ```

   Bu komut birkaç sertifika ve anahtar dosyalarını oluşturur, ancak özellikle bu makalenin devamındaki başvurmak için oluşturacağız:
   * `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

2. IOT Edge cihaz CA sertifikasını ve özel anahtarı aşağıdaki komutla oluşturun. Ağ geçidi cihazı iotedge\config.yaml dosyada bulunan ağ geçidi konak adı belirtin. Ağ geçidi ana bilgisayar adı, dosya adı için kullanılır ve sertifika oluşturma sırasında. 

   ```powershell
   New-CACertsEdgeDevice "<gateway hostname>"
   ```

   Bu komut birkaç sertifika ve bu makalenin ilerleyen bölümlerinde bkz oluşturacağız iki dahil olmak üzere anahtar dosyalarını oluşturur:
   * `<WRKDIR>\certs\iot-edge-device-<gateway hostname>-full-chain.cert.pem`
   * `<WRKDIR>\private\iot-edge-device-<gateway hostname>.key.pem`

Sertifikaları olduğuna göre atlayın [sertifikaları ağ geçidi yükleme](#install-certificates-on-the-gateway)

## <a name="generate-certificates-with-linux"></a>Linux ile sertifikalar oluşturma

Linux üzerinde test sertifikalarınızı oluşturmak için bu bölümdeki adımları kullanın. Bir Linux Makine sertifikalarını oluşturmak için kullanın ve ardından bunları üzerinden tüm desteklenen işletim sistemi üzerinde çalışan herhangi bir IOT Edge cihazı kopyalayın. 

Bu bölümde oluşturulan sertifikaları yalnızca test amaçlarına yöneliktir. 

### <a name="prepare-creation-scripts"></a>Oluşturma betikleri hazırlama

Azure IOT Edge git deposu test sertifikalarınızı oluşturmak için kullanabileceğiniz betikleri içerir. Bu bölümde, IOT Edge depoyu kopyalayalım ve betikleri yürütme. 

1. Üretim dışı sertifikalarını oluşturmak için komut dosyalarını içeren bir git deposunu kopyalayın. Bu betikler, saydam bir ağ geçidini ayarlamak için gerekli sertifikaları oluşturmanıza yardımcı olur. 

   ```bash
   git clone https://github.com/Azure/iotedge.git
   ```

2. Çalışmak istediğiniz dizine gidin. Bu dizine makale boyunca başvuracağınız  *\<WRKDIR >* . Tüm sertifika ve anahtar dosyalarını bu dizinde oluşturulur.
  
3. Yapılandırma ve komut dosyaları kopyalanan IOT Edge depodan çalışma dizininize kopyalayın.

   ```bash
   cp <path>/iotedge/tools/CACertificates/*.cnf .
   cp <path>/iotedge/tools/CACertificates/certGen.sh .
   ```

<!--
4. Configure OpenSSL to generate certificates using the provided script. 

   ```bash
   chmod 700 certGen.sh 
   ```
-->

### <a name="create-certificates"></a>Sertifika oluşturma

Bu bölümde, üç sertifikaları oluşturma ve bunları bir zincirinde bağlayın. IOT Edge ağ geçidi cihazınıza ve herhangi bir aşağı akış cihazlarda kolayca yüklemek için sertifika zinciri dosyasında yerleştirme sağlar.  

1. Kök CA sertifikasını ve bir ara sertifika oluşturun. Bu sertifikalar yerleştirilir  *\<WRKDIR >* .

   ```bash
   ./certGen.sh create_root_and_intermediate
   ```

   Betik, bazı sertifikaları ile anahtarlarını oluşturur. Bir sonraki bölümde diyoruz, not edin:
   * `<WRKDIR>/certs/azure-iot-test-only.root.ca.cert.pem`

2. IOT Edge cihaz CA sertifikasını ve özel anahtarı aşağıdaki komutla oluşturun. Ağ geçidi cihazı iotedge/config.yaml dosyada bulunan ağ geçidi konak adı belirtin. Ağ geçidi ana bilgisayar adı, dosya adı için kullanılır ve sertifika oluşturma sırasında. 

   ```bash
   ./certGen.sh create_edge_device_certificate "<gateway hostname>"
   ```

   Betik, bazı sertifikaları ile anahtarlarını oluşturur. Sonraki bölümde diyoruz, iki, not edin: 
   * `<WRKDIR>/certs/iot-edge-device-<gateway hostname>-full-chain.cert.pem`
   * `<WRKDIR>/private/iot-edge-device-<gateway hostname>.key.pem`

## <a name="install-certificates-on-the-gateway"></a>Sertifika ağ geçidi yükleme

Bir sertifika zinciri yaptığınız, IOT Edge ağ geçidi cihazında yükleyin ve yeni sertifikalar başvurmak için IOT Edge çalışma zamanı yapılandırma gerekir. 

1. Aşağıdaki dosyaları kopyalayın  *\<WRKDIR >* . Bunları herhangi bir IOT Edge cihazı kaydedin. IOT Edge cihazınız olarak hedef dizine başvuracağınız  *\<CERTDIR >* . 

   * Cihaz CA sertifikası-  `<WRKDIR>\certs\iot-edge-device-<gateway hostname>-full-chain.cert.pem`
   * Cihaz CA özel anahtarı- `<WRKDIR>\private\iot-edge-device-<gateway hostname>.key.pem`
   * Kök CA- `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

   Bir hizmet gibi kullanabileceğiniz [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault) veya bir işlev [güvenli kopya Protokolü](https://www.ssh.com/ssh/scp/) sertifika dosyalarını taşımak için.  IOT Edge cihazı sertifikaları oluşturursa, bu adımı atlayın ve çalışma dizininin yolunu kullanın.

2. IOT Edge güvenlik daemon yapılandırma dosyasını açın. 

   * Windows: `C:\ProgramData\iotedge\config.yaml`
   * Linux: `/etc/iotedge/config.yaml`

3. Ayarlama **sertifika** IOT Edge cihazında sertifika ve anahtar dosyalarının tam yolunu config.yaml dosyasındaki özellikleri. Kaldırma `#` sertifika özellikleri ve dört satırın açıklamasını kaldırın için önce karakter. Yaml içinde girintileri iki boşluk olduğunu unutmayın.

   * Windows:

      ```yaml
      certificates:
        device_ca_cert: "<CERTDIR>\\certs\\iot-edge-device-<gateway hostname>-full-chain.cert.pem"
        device_ca_pk: "<CERTDIR>\\private\\iot-edge-device-<gateway hostname>.key.pem"
        trusted_ca_certs: "<CERTDIR>\\certs\\azure-iot-test-only.root.ca.cert.pem"
      ```
   
   * Linux: 
      ```yaml
      certificates:
        device_ca_cert: "<CERTDIR>/certs/iot-edge-device-<gateway hostname>-full-chain.cert.pem"
        device_ca_pk: "<CERTDIR>/private/iot-edge-device-<gateway hostname>.key.pem"
        trusted_ca_certs: "<CERTDIR>/certs/azure-iot-test-only.root.ca.cert.pem"
      ```

4. Emin Linux cihazlarda kullanıcı **iotedge** sertifikaları bulunduran dizini için okuma izni. 

## <a name="deploy-edgehub-to-the-gateway"></a>Ağ geçidine EdgeHub dağıtma

IOT Edge üzerinde bir cihaz ilk kez yüklediğinizde, yalnızca bir sistem modülü otomatik olarak başlar: IOT Edge Aracısı. Cihazınızı bir ağ geçidi olarak çalışmak her iki sistem modüllerini gerekir. Ağ geçidi cihazınıza önce modüllerin dağıtmadıysanız, ikinci bir sistem modülü, IOT Edge hub'ı başlatmak cihazınızın ilk dağıtımı oluşturun. Dağıtım Sihirbazı'nda modüllerin ekleme yapmazsanız, ancak her iki sistem modüllerini çalışır durumda olduğundan emin olun çünkü boş görünür. 

Komutu ile bir cihaz üzerinde çalışan modüllerine denetleyebilirsiniz `iotedge list`. Modül listesi yalnızca döndürürse **edgeAgent** olmadan **edgeHub**, aşağıdaki adımları kullanın:

1. Azure portalında IoT Hub'ınıza gidin.

2. Git **IOT Edge** ve ağ geçidi olarak kullanacağınız IOT Edge Cihazınızı seçin.

3. **Modülleri Ayarlama**'yı seçin.

4. **İleri**’yi seçin.

5. İçinde **yolları belirtin** sayfasında, tüm iletileri tüm modüllerden IOT Hub'ına gönderir ve varsayılan bir yol olmalıdır. Yoksa aşağıdaki kodu ekleyip **İleri**'yi seçin.

   ```JSON
   {
       "routes": {
           "route": "FROM /* INTO $upstream"
       }
   }
   ```

6. İçinde **gözden geçirme şablonu** sayfasında **Gönder**.

## <a name="open-ports-on-gateway-device"></a>Ağ geçidi cihazı bağlantı noktalarını Aç

IOT Hub ile tüm iletişimi üzerinden giden bağlantılar yapıldığı için standart IOT Edge cihazları işleve, herhangi bir gelen bağlantı gerekmez. Ağ geçidi cihazları, aşağı akış cihazlarından iletilerini gerektiği için farklıdır. Aşağı Akış cihazları ve ağ geçidi cihazı arasında bir güvenlik duvarı olup, iletişim, güvenlik duvarından mümkün olması gerekir.

Bir ağ geçidi senaryo çalışmak en az bir IOT Edge hub'ın desteklenen protokoller aşağı akış cihazlardan gelen trafik için açık olmalıdır. MQTT, AMQP ve HTTPS Desteklenen protokoller şunlardır. 

| Port | Protocol |
| ---- | -------- |
| 8883 | MQTT |
| 5671 | AMQP |
| 443 | HTTPS <br> MQTT+WS <br> AMQP + WS | 

## <a name="route-messages-from-downstream-devices"></a>Aşağı Akış cihazlardan yönlendirme iletileri
IOT Edge çalışma zamanı yalnızca modülleri tarafından gönderilen iletiler gibi aşağı akış cihazlardan gönderilen iletiler yönlendirebilirsiniz. Bu özellik tüm verileri buluta göndermeden önce ağ geçidi üzerinde çalışan bir modüldeki analiz gerçekleştirmenize olanak tanır. 

Şu anda, aşağı akış cihazlar tarafından gönderilen iletileri yönlendiren bunları modülleri tarafından gönderilen iletilerden farklılaştırılması tarafından yoludur. Tüm modüller tarafından gönderilen iletiler olarak adlandırılan bir sistem özelliği içeren **connectionModuleId** ancak aşağı akış cihazlar tarafından gönderilen iletileri yapın. Bu sistem özelliği içeren herhangi bir iletiyi hariç tutmak için rota WHERE yan tümcesini kullanabilirsiniz. 

Adlı bir modül için herhangi bir aşağı akış CİHAZDAN iletileri gönderir gibi bir örnek yoldur `ai_insights`ve sonra `ai_insights` IOT hub'ına.

```json
{
    "routes":{
        "sensorToAIInsightsInput1":"FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/ai_insights/inputs/input1\")", 
        "AIInsightsToIoTHub":"FROM /messages/modules/ai_insights/outputs/output1 INTO $upstream" 
    } 
}
```

İleti yönlendirme hakkında daha fazla bilgi için bkz. [modülleri dağıtma ve yollar'kurmak](./module-composition.md#declare-routes).


## <a name="enable-extended-offline-operation"></a>Genişletilmiş çevrimdışı işlemi etkinleştir

İle başlayarak [v1.0.4 yayın](https://github.com/Azure/azure-iotedge/releases/tag/1.0.4) IOT Edge çalışma zamanını, kendisine bağlanmak için aşağı akış cihazlarının ve ağ geçidi cihazı için genişletilmiş çevrimdışı işlemi yapılandırılabilir. 

Bu özellik sayesinde yerel modülleri veya aşağı akış cihazları IOT Edge cihazı ile gerektiği gibi kimliğinizi yeniden doğrulayın ve birbirleriyle iletiler ve IOT hub'ından değilken bile yöntemlerini kullanarak iletişim kurar. Daha fazla bilgi için [anlayın cihazları, modülleri ve alt cihazlar bu çevrimdışı özellikleri IOT Edge için Genişletilmiş](offline-capabilities.md).

Genişletilmiş çevrimdışı özellikleri etkinleştirmek için bir IOT Edge ağ geçidi cihazı için bağlanacak aşağı akış cihazlar arasında bir üst-alt ilişkisi oluşturun. Bu adımların daha ayrıntılı olarak açıklanmıştır [Azure IOT hub'a bir aşağı akış cihaz kimlik doğrulaması](how-to-authenticate-downstream-device.md).

## <a name="next-steps"></a>Sonraki adımlar

Saydam bir ağ geçidi olarak çalışan bir IOT Edge cihazına sahip olduğunuza göre ağ geçidi güven ve iletileri göndermek için aşağı akış cihazlarınızı yapılandırmak gerekir. Daha fazla bilgi için [bir Azure IOT Edge ağ geçidi için aşağı akış cihaz Bağlayamama](how-to-connect-downstream-device.md) ve [Azure IOT hub'a bir aşağı akış cihaz kimlik doğrulaması](how-to-authenticate-downstream-device.md).
