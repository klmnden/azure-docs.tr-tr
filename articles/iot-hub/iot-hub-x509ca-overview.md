---
title: Azure IOT hub'ı X.509 CA güvenliğine genel bakış | Microsoft Docs
description: Genel Bakış - X.509 sertifika yetkililerini kullanarak IOT hub'a cihazların kimliklerini doğrulamak nasıl.
author: eustacea
manager: arjmands
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 09/18/2017
ms.author: eustacea
ms.openlocfilehash: b7464e5cc052ecade4a10102de947d37a63c962a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60615030"
---
# <a name="device-authentication-using-x509-ca-certificates"></a>X.509 CA sertifikalarını kullanarak cihaz kimlik doğrulaması

Bu makalede, IOT hub'a bağlanan cihazların kimliklerini doğrulamak için X.509 sertifika yetkilisi (CA) sertifikaları kullanmayı açıklar.  Bu makalede şunları öğreneceksiniz:

* X.509 CA sertifikası alma
* X.509 CA sertifikası IOT hub'ına kaydetme
* X.509 CA sertifikalarını kullanarak cihazları oturum açma
* X.509 CA imzalı cihazları nasıl doğrulanır

## <a name="overview"></a>Genel Bakış

X.509 CA özelliği, bir sertifika yetkilisi (CA) kullanarak IOT hub'a cihaz kimlik doğrulaması sağlar. Büyük ölçüde ilk cihaz kayıt işlemi ve tedarik zinciri Lojistik cihaz üretim sırasında kolaylaştırır. [X.509 CA sertifikalarını kullanarak, bu makaledeki senaryoda değeri hakkında daha fazla bilgi edinin](iot-hub-x509ca-concept.md) cihaz kimlik doğrulaması için.  Aşağıdaki adımları neden mevcut açıklandığı gibi devam etmeden önce bu senaryo makaleyi okuyun etmenizi öneririz.

## <a name="prerequisite"></a>Önkoşul

X.509 CA özelliğini kullanarak bir IOT hub'ı hesabına sahip olmasını gerektirir.  [Bir IOT hub'ı örneği oluşturmayı](quickstart-send-telemetry-dotnet.md) zaten yoksa.

## <a name="how-to-get-an-x509-ca-certificate"></a>X.509 CA sertifikası alma

X.509 CA sertifika, cihazlarınızın her biri için sertifikaları zincirine üst kısmındaki içindir.  Satın alabilir veya nasıl kullanmak istediğinize bağlı olarak bir tane oluşturun.

Üretim ortamı için ortak kök sertifika yetkilisinden bir X.509 CA sertifikası satın almanız önerilir. Kök CA'ın avantajı sahip bir CA sertifikası satın alma için cihazlarınızı yasallığı vouch için güvenilir bir üçüncü taraf olarak davranan. Üçüncü taraf ürünler veya hizmetler ile etkileşim kurmak için burada beklenen açık bir IOT ağının parçası olması için cihazlarınızı istiyorsanız bu seçeneği göz önünde bulundurun.

Ayrıca, deneme veya kullanmak için otomatik olarak imzalanan X.509 CA kapalı IOT ağlar oluşturabilirsiniz.

X.509 CA sertifikanızı almak, onun karşılık gelen özel anahtar parolası tuttuğunuzdan emin olun ve korumalı bakılmaksızın her zaman.  Bu, güven X.509 CA kimlik doğrulama güven ilişkisi oluşturmak için gereklidir.

Bilgi nasıl [otomatik olarak imzalanan bir sertifika oluşturmak](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md), deneme boyunca bu özellik açıklaması için kullanabilirsiniz.

## <a name="sign-devices-into-the-certificate-chain-of-trust"></a>Oturum cihazlara sertifika güven zinciri

X.509 CA sertifikası sahibi olan başka bir ara CA sırayla imzalayabilirsiniz ara CA şifreli olarak oturum açabilir ve bu şekilde kadar son ara CA bu işlem bir cihaz açarak sonlandırır. Art arda bir güven sertifikası zinciri bilinen bir sertifikalar zincirinden oluşur. Gerçek Hayatta bu cihazları imzalama doğru güven temsilci olarak yürütülür. Bu temsilci önemlidir, çünkü şifreli olarak değişken bir gözetim zinciri oluşturur ve imzalama anahtarları paylaşımı önler.

![img-Generic-CERT-chain-of-Trust](./media/generic-cert-chain-of-trust.png)

(Bir yaprak sertifikası olarak da bilinir) cihaz sertifikası olmalıdır *konu adı* kümesine **cihaz kimliği** Azure IOT Hub'ında IOT cihaz kaydı sırasında kullanıldı. Bu ayar, kimlik doğrulaması için gereklidir.

Buradan edinin nasıl [sertifikası zinciri oluşturmak](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) cihazları açarken bitti olarak.

## <a name="how-to-register-the-x509-ca-certificate-to-iot-hub"></a>X.509 CA sertifikası IOT hub'ına kaydetme

IOT hub'ına Burada, kayıt ve bağlantı sırasında aygıtlarınızın kimliğini doğrulamak için kullanılacak, X.509 CA sertifika kaydettirir.  X.509 CA sertifikası kaydetme sertifika dosyası karşıya yükleme ve kanıtını oluşturan iki adımlı bir işlemdir.

Karşıya yükleme işlemi, sertifikanızı içeren bir dosyayı karşıya yüklemeyi kapsar.  Bu dosya, herhangi bir özel anahtara asla içermelidir.

Kavram kanıtı elinde adım şifreleme sınama ve IOT hub'ı arasındaki yanıt işlemi içerir.  Dijital sertifika içeriği genel ve bu nedenle, gizlice maruz kalabilir düşünüldüğünde, IOT hub'ı CA sertifikasını gerçekten sahip olmak ister misiniz?  Bu CA sertifikasının karşılık gelen özel anahtar ile oturum açmanız gerekir rastgele bir sınama oluşturarak bunu.  Özel anahtarı gizli ve korumalı yukarıda bıraktıysanız sonra yalnızca bu adımı tamamlamak için Bilgi Bankası sahip, önerilir. Özel anahtarların gizliliği, bu yöntem güvende kaynağıdır.  Sınama imzaladıktan sonra sonuçları içeren bir dosyayı karşıya yükleyerek bu adımı tamamlayın.

Buradan edinin nasıl [CA sertifikanız kaydetme](iot-hub-security-x509-get-started.md#register-x509-ca-certificates-to-your-iot-hub)

## <a name="how-to-create-a-device-on-iot-hub"></a>IOT Hub'ınızda bir cihaz oluşturma

Cihaz kimliğe bürünme kullanımını için bekleyebileceğiniz hangi cihazlar hakkında bilgi sağlamak, IOT hub'ı gerektirir.  Bu IOT Hub'ınızın cihaz kayıt defterinde bir cihaz girişi oluşturmanız gerekir.  Bu işlem, IOT hub'ı kullanırken otomatik [cihaz sağlama hizmeti](https://azure.microsoft.com/blog/azure-iot-hub-device-provisioning-service-preview-automates-device-connection-configuration/). 

Buradan edinin nasıl [el ile bir cihaz IOT Hub oluşturma](iot-hub-security-x509-get-started.md#create-an-x509-device-for-your-iot-hub).

IOT hub'ınız için bir X.509 cihazı oluşturma

## <a name="authenticating-devices-signed-with-x509-ca-certificates"></a>X.509 CA sertifikalarını ile imzalanmış cihazların kimliğini doğrulama

Cihaz, hatta ilk kez bağlandığında, kayıtlı X.509 CA sertifikası ve imzalanmış bir sertifika güven zinciri cihazlar ile nelerin cihaz kimlik doğrulaması şeklindedir.  X.509 CA imzalı cihaz bağlanır, kendi sertifika zinciri doğrulama için yükler. Tüm ara CA ve cihaz sertifika zinciri içerir.  Bu bilgileri kullanarak IOT Hub cihazı iki adımlı bir işlem kimliğini doğrular.  IOT hub'ı iç tutarlılık için sertifika zinciri şifreli olarak doğrular ve ardından cihaza elinde kavram zor sorunları.  IOT Hub cihaz gerçek bir CİHAZDAN başarılı bir elinde kavram yanıtını bildirir.  Bu bildirim yalnızca cihaz, bu sınaması başarılı bir şekilde yanıt verebilir ve cihazın özel anahtarı korunduğunu varsayar.  Özel anahtarlarınızı korumak için cihazları güvenli yongalarda gibi donanım güvenli modülleri (HSM) kullanılmasını öneririz.

IOT Hub başarılı cihaz bağlantı kimlik doğrulama işlemi tamamlandıktan ve ayrıca bir doğru kurulumu gösterir.

Buradan edinin nasıl [bu cihaz bağlantı adımın](iot-hub-security-x509-get-started.md#authenticate-your-x509-device-with-the-x509-certificates).

## <a name="next-steps"></a>Sonraki Adımlar

Hakkında bilgi edinin [X.509 CA kimlik değerini](iot-hub-x509ca-concept.md) IOT.

IOT Hub ile çalışmaya başlama [cihaz sağlama hizmeti](https://docs.microsoft.com/azure/iot-dps/).
