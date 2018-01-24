---
title: "Geliştirme basitleştirmek için Azure IOT Hub cihaz sağlama hizmeti SDK'ları içinde sağlanan araçlarını kullanma"
description: "Bu belge geliştirme için Azure IOT Hub cihaz sağlama hizmeti SDK'ları içinde sağlanan araçları incelemeleri"
services: iot-dps
keywords: 
author: yzhong94
ms.author: yizhon
ms.date: 01/18/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 76c6f64dea202f661691fafaa78a6d77b4a40f14
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="how-to-use-tools-provided-in-the-sdks-to-simplify-development-for-provisioning"></a>Geliştirme sağlama işlemini basitleştirmek için SDK'ları sağlanan araçları kullanma
IOT Hub cihaz sağlama hizmeti güvenli ve ölçeklenebilir bir şekilde zero touch, yalnızca zaman sağlamayla sağlama işlemini basitleştirir.  X.509 sertifikası veya Güvenilir Platform Modülü (TPM) biçiminde güvenlik kanıtlama gereklidir.  Microsoft ile ortaklık de [diğer güvenlik donanım iş ortaklarından](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) IOT dağıtımının güvenliğini sağlama güvenini artırmak için. Donanım güvenlik gereksinimi anlama geliştiriciler için oldukça zor olabilir. Azure IOT sağlama hizmeti SDK'ları bir dizi geliştiriciler kolaylık katman sağlama hizmete konuşun yazma istemcilerin kullanabilmesi için sağlanır. SDK'ları örnekleri yaygın senaryolar ve bunun yanı sıra güvenlik kanıtlama geliştirme basitleştirmek için araçlar da sağlar.

## <a name="trusted-platform-module-tpm-simulator"></a>Güvenilir Platform Modülü (TPM) simulator
[TPM](https://docs.microsoft.com/azure/iot-dps/concepts-security#trusted-platform-module-tpm) platform kimliğini doğrulamak için anahtarlarını güvenli bir şekilde depolamak için bir standart başvurabilir veya standart uygulama modülleri ile etkileşim kurmak için kullanılan g/ç arabirimi başvurabilirsiniz. TPM'ler ayrık donanım, tümleşik donanım, bellenim veya yazılım tabanlı bulunabilir.  Üretimde TPM cihazda ayrık donanım, tümleşik donanım olarak da bulunur ve bellenim tabanlı. Sınama aşamasında, bir yazılım tabanlı TPM simulator geliştiricileri için sağlanır.  Bu simulator, yalnızca Windows platformunda şimdilik geliştirmek için kullanılabilir.

TPM simulator kullanma adımları şunlardır:
1. [Geliştirme ortamı hazırlamalısınız](https://docs.microsoft.com/azure/iot-dps/quick-enroll-device-x509-java#prepare-the-development-environment) ve GitHub deposunu kopyalayın:
```
git clone https://github.com/Azure/azure-iot-sdk-java.git
```
2. TPM simulator klasörü altında gidin ```azure-iot-sdk-java/provisioning/provisioning-tool/tpm-simulator/```.
3. Simulator.exe sağlama cihaz için herhangi bir istemci uygulama çalıştırılmadan önce çalıştırın.
4. Kayıt kimliği ve onay anahtarını elde etmek için sağlama işlemi boyunca arka planda simulator olanak tanır.  Her iki değer yalnızca çalışma bir örneği için geçerlidir.

## <a name="x509-certificate-generator"></a>X.509 sertifikası Oluşturucusu
[X.509 sertifikaları](https://docs.microsoft.com/azure/iot-dps/concepts-security#x509-certificates) kanıtlama mekanizması olarak üretim ölçeği ve cihaz sağlamayı kolaylaştırmak için kullanılabilir.  Vardır [çeşitli yollardan](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview#how-to-get-an-x509-ca-certificate) bir X.509 sertifikası almak için:
* Üretim ortamı için bir ortak kök sertifika yetkilisinden bir X.509 CA sertifikası satın öneririz.
* Sınama ortamı için bir X.509 kök sertifikası veya X.509 sertifika zincirini kullanarak oluşturabilirsiniz:
    * OpenSSL: Bu [kılavuzluk etmesi nasıl](https://docs.microsoft.com/azure/iot-hub/iot-hub-security-x509-create-certificates) kullanan örnek PowerShell komut dosyalarıyla anlatılmaktadır [OpenSSL](https://www.openssl.org/) oluşturmak ve X.509 sertifikaları imzalamak için.  Ayrıca, betik diğer dillerde sertifika oluşturma için kullanabilirsiniz:
        * [Node.js](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/tools)
        * [PowerShell](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md)
        
    * Cihaz kimliği oluşturma altyapısı (BÖLMEK) öykünücüsü: BÖLMEK için şifreleme cihaz kimliği kullanılabilir ve istemci sertifikalarını kanıtlama TLS protokolünü ve X.509 göre.  [Bilgi](https://www.microsoft.com/research/publication/device-identity-dice-riot-keys-certificates/) BÖLMEK cihaz kimliği hakkında daha fazla bilgi.

### <a name="using-x509-certificate-generator-with-dice-emulator"></a>X.509 sertifikası Oluşturucu BÖLMEK öykünücü ile kullanma
SDK'ları bir X.509 sertifikası Oluşturucu bulunan BÖLMEK öykünücü ile sağlayın [Java SDK'sı](https://github.com/Azure/azure-iot-sdk-java/tree/master/provisioning/provisioning-tools/provisioning-x509-cert-generator).  Bu oluşturucunun platformlar arası çalışır.  Oluşturulan sertifika diğer dillerde geliştirme için kullanılabilir.

Şu anda çalışırken bir kök sertifikası, bir ara sertifika, bir yaprak sertifikası ve ilişkili özel anahtarı BÖLMEK öykünücüsü çıkarır.  Ancak, kök sertifikasını veya ara sertifika ayrı yaprak sertifikasını imzalamak için kullanılamaz.  Birden çok cihaz yaprak sertifikaları imzalamak için bir imzalama sertifikası kullanıldığı Grup kayıt senaryoyu test etmek istiyorsanız, bir sertifika zinciri oluşturmak için OpenSSL kullanabilirsiniz.

Bu oluşturucu kullanılarak X.509 sertifikası oluşturmak için:
1. [Geliştirme ortamı hazırlamalısınız](https://docs.microsoft.com/azure/iot-dps/quick-enroll-device-x509-java#prepare-the-development-environment) ve GitHub deposunu kopyalayın:
```
git clone https://github.com/Azure/azure-iot-sdk-java.git
```
2. Kök azure-IOT-sdk-java ile değiştirin.
3. Çalıştırma ```mvn install -DskipTests=true``` gereken tüm paketler indirip SDK derleme
4. X.509 sertifikası üreteci için kök dizinine gidin ```azure-iot-sdk-java/provisioning/provisioning-tools/provisioning-x509-cert-generator```.
5. İle derleme```mvn clean install```
6. Aşağıdaki komutları kullanarak aracı çalıştırın:
```
cd target
java -jar ./provisioning-x509-cert-generator-{version}-with-deps.jar
```
7. İstendiğinde sertifikalarınız için _Common Name_ (Ortak Ad) girebilirsiniz.
8. Aracı yerel olarak oluşturan bir **istemci sertifikası**, **istemci sertifika özel anahtar**, **ara sertifika**ve **kök sertifika**.

**İstemci sertifikası** cihazda yaprak sertifikasıdır.  **İstemci sertifikası** ve ilişkili **istemci sertifika özel anahtar** aygıt istemcisi gereklidir. Seçtiğiniz dil ne bağlı olarak, bu istemci uygulamasında put mekanizması farklı olabilir.  Daha fazla bilgi için bkz: [Quickstarts](https://docs.microsoft.com/azure/iot-dps/quick-create-simulated-device-x509) üzerinde daha fazla bilgi için X.509 kullanarak sanal bir cihaz oluşturma.

Bir kayıt grubunu veya tek tek kayıt oluşturmak için kök sertifikasını veya Ara kullanılabilir [program aracılığıyla](https://docs.microsoft.com/azure/iot-dps/how-to-manage-enrollments-sdks) veya kullanarak [portal](https://docs.microsoft.com/azure/iot-dps/how-to-manage-enrollments).

## <a name="next-steps"></a>Sonraki adımlar
* Kullanarak geliştirme [Azure IOT SDK'sı]( https://github.com/Azure/azure-iot-sdks) Azure IOT Hub ve Azure IOT Hub cihaz hizmeti sağlama