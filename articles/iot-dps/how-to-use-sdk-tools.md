---
title: Geliştirmeyi basitleştirmek için Azure IOT Hub cihaz sağlama hizmeti SDK'ları sağlanan araçları kullanın
description: Bu belge, geliştirme için Azure IOT Hub cihaz sağlama hizmeti SDK'ları sağlanan araçları gözden geçirmeleri
author: yzhong94
ms.author: yizhon
ms.date: 04/09/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: arjmands
ms.openlocfilehash: dc8c29b1c7d4e5056cb6aeee6335e32687fd547f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60627330"
---
# <a name="how-to-use-tools-provided-in-the-sdks-to-simplify-development-for-provisioning"></a>Geliştirme için sağlama işlemini basitleştirmek için SDK sağlanan araçları kullanma
IOT Hub cihazı sağlama hizmeti sıfır dokunma ile sağlama işlemini kolaylaştıran just-ın-time [otomatik sağlama](concepts-auto-provisioning.md) güvenli ve ölçeklenebilir bir şekilde.  X.509 sertifikası veya Güvenilir Platform Modülü (TPM) biçiminde güvenlik kanıtlama gereklidir.  Microsoft ile işbirliği ayrıca [diğer güvenlik donanım iş ortaklarından](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) IOT dağıtım güvenliğindeki emin olmak için. Donanım güvenlik gereksinimi anlama, geliştiriciler için oldukça zor olabilir. Azure IOT sağlama hizmeti SDK'ları kümesi geliştiriciler kolaylık katman sağlama hizmetinizle iletişim kurmasına yazma istemcilerin kullanabilmesi için sağlanır. SDK'ları örnekleri yaygın senaryolar ve bunun yanı sıra güvenlik kanıtlama geliştirme kolaylaştıran araçlar kümesi de sağlar.

## <a name="trusted-platform-module-tpm-simulator"></a>Güvenilir Platform Modülü (TPM) simülatör
[TPM](https://docs.microsoft.com/azure/iot-dps/concepts-security) platform kimliğini doğrulamak için anahtarları güvenli bir şekilde depolamak için standart bir başvurabilir veya standart uygulama modülleri ile etkileşim kurmak için kullanılan g/ç arabirimi başvurabilir. TPM'ler ayrık donanım, tümleşik donanım, üretici yazılımı veya yazılım tabanlı bulunabilir.  Üretim ortamında TPM cihaz üzerinde ayrı donanım, tümleşik donanım olarak ya da bulunur ve üretici yazılımı tabanlı. Sınama aşamasında, bir yazılım tabanlı TPM simülatörünü geliştiricileri için sağlanır.  Bu simülatörü, yalnızca Windows platformunda şimdilik geliştirmek için kullanılabilir.

TPM simülatörünü kullanma adımları şunlardır:
1. [Geliştirme ortamınızı hazırlama](https://docs.microsoft.com/azure/iot-dps/quick-enroll-device-x509-java) ve GitHub deposunu kopyalayın:
   ```
   git clone https://github.com/Azure/azure-iot-sdk-java.git
   ```
2. Altındaki TPM simülatörü klasörüne gidin ```azure-iot-sdk-java/provisioning/provisioning-tool/tpm-simulator/```.
3. Cihaz sağlama için herhangi bir istemci uygulama çalıştırılmadan önce Simulator.exe çalıştırın.
4. Kayıt kimliği ve onay anahtarını elde etmek için sağlama işlemi boyunca arka planda simülatör olanak tanır.  Her iki değerin yalnızca bir örneğini çalıştırma için geçerlidir.

## <a name="x509-certificate-generator"></a>X.509 Sertifika Oluşturucu
[X.509 sertifikaları](https://docs.microsoft.com/azure/iot-dps/concepts-security#x509-certificates) bir kanıtlama mekanizması olarak üretim ölçeği ve cihaz sağlamayı kolaylaştırmak için kullanılabilir.  Vardır [birkaç şekilde](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview#how-to-get-an-x509-ca-certificate) bir X.509 sertifikası almak için:
* Üretim ortamı için ortak kök sertifika yetkilisinden bir X.509 CA sertifikası satın almayı öneririz.
* Sınama ortamı için bir X.509 kök sertifika veya X.509 sertifika zincirini kullanarak oluşturabilirsiniz:
    * OpenSSL: Sertifika oluşturma için betikler kullanabilirsiniz:
        * [Node.js](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/tools)
        * [PowerShell veya Bash](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md)
        
    * Cihaz kimliği bileşim motoru (DICE) öykünücüsü: İstemci sertifikaları TLS protokolünün ve X.509 kanıtlama göre ve DICE şifreleme cihaz kimliği için kullanılabilir.  [Bilgi](https://www.microsoft.com/research/publication/device-identity-dice-riot-keys-certificates/) DICE cihaz kimliği hakkında daha fazla bilgi.

### <a name="using-x509-certificate-generator-with-dice-emulator"></a>X.509 Sertifika Oluşturucu DICE öykünücü ile kullanma
SDK'ları bir X.509 sertifikası Oluşturucu bulunan DICE öykünücüyle sağlamak [Java SDK'sı](https://github.com/Azure/azure-iot-sdk-java/tree/master/provisioning/provisioning-tools/provisioning-x509-cert-generator).  Bu oluşturucu, platformlar arası çalışır.  Oluşturulan sertifika, diğer dillerde geliştirme için kullanılabilir.

Şu anda çalışırken bir kök sertifikası, bir ara sertifika, bir yaprak sertifikayı ve ilişkili özel anahtarı DICE öykünücü çıkarır.  Ancak, kök sertifika veya ara sertifika ayrı yaprak sertifikayı imzalamak için kullanılamaz.  Çok cihazlı yaprak sertifikaları imzalamak için bir imzalama sertifikası kullanıldığı grubu kayıt senaryoyu test etmek istiyorsanız, bir sertifikalar zincirinden üretmek için OpenSSL kullanabilirsiniz.

Bu oluşturucu kullanılarak X.509 sertifikası oluşturmak için:
1. [Geliştirme ortamınızı hazırlama](https://docs.microsoft.com/azure/iot-dps/quick-enroll-device-x509-java) ve GitHub deposunu kopyalayın:
   ```
   git clone https://github.com/Azure/azure-iot-sdk-java.git
   ```
2. Kök azure-IOT-sdk-java ile değiştirin.
3. Çalıştırma ```mvn install -DskipTests=true``` gerekli tüm paketleri indirmek ve SDK'yı derlemek için
4. X.509 Certificate Generator içinde için kök dizinine gidin ```azure-iot-sdk-java/provisioning/provisioning-tools/provisioning-x509-cert-generator```.
5. İle oluşturma ```mvn clean install```
6. Aşağıdaki komutları kullanarak aracı çalıştırın:
   ```
   cd target
   java -jar ./provisioning-x509-cert-generator-{version}-with-deps.jar
   ```
7. İstendiğinde sertifikalarınız için _Common Name_ (Ortak Ad) girebilirsiniz.
8. Yerel olarak aracının oluşturduğu bir **Client Cert**, **Client Cert Private Key**, **ara sertifika**ve **Root Cert**.

**İstemci sertifikası** yaprak sertifikanın.  **İstemci sertifikası** ve ilişkili **Client Cert Private Key** cihaz istemcisi gereklidir. Seçtiğiniz hangi dile bağlı olarak, bu istemci uygulamasında koymak için bir mekanizma farklı olabilir.  Daha fazla bilgi için [hızlı Başlangıçlar](https://docs.microsoft.com/azure/iot-dps/quick-create-simulated-device-x509) üzerinde daha fazla bilgi için X.509 kullanarak sanal cihaz oluşturma.

Bir kayıt grubu veya bireysel kayıt oluşturmak için kök sertifika veya Ara kullanılabilir [program aracılığıyla](https://docs.microsoft.com/azure/iot-dps/how-to-manage-enrollments-sdks) veya bu adı kullanıyor [portalı](https://docs.microsoft.com/azure/iot-dps/how-to-manage-enrollments).

## <a name="next-steps"></a>Sonraki adımlar
* Kullanarak geliştirme [Azure IOT SDK'sı]( https://github.com/Azure/azure-iot-sdks) Azure IOT Hub ve Azure IOT Hub cihazı sağlama hizmeti için
