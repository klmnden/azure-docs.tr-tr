---
title: "Azure IOT Hub X.509 CA güvenliğine genel bakış | Microsoft Docs"
description: "Genel Bakış - cihazlar IOT hub'ına X.509 sertifika yetkililerini kullanarak kimlik doğrulaması yapmayı."
services: iot-hub
documentationcenter: .net
author: eustacea
manager: arjmands
editor: 
ms.assetid: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2017
ms.author: eustacea
ms.openlocfilehash: 080c83fd0c34bdcb8978edf0ba4f783402a88b1f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="device-authentication-using-x509-ca-certificates"></a>X.509 CA sertifikaları kullanarak cihaz kimlik doğrulaması

Bu makalede X.509 sertifika yetkilisi (CA) sertifikalarının IOT hub'a bağlanan cihazların kimliğini doğrulamak için nasıl kullanılacağını açıklar.  Bu makalede şunları öğreneceksiniz:

* Bir X.509 CA sertifikası alma
* IOT Hub'ına X.509 CA sertifikası kaydetme
* X.509 CA sertifikaları kullanarak cihazları imzalama
* X.509 CA ile imzalanmış aygıtları nasıl doğrulanır

## <a name="overview"></a>Genel Bakış

X.509 CA özellik IOT hub'ına bir sertifika yetkilisi (CA) kullanan cihaz kimlik doğrulamasını sağlar. Büyük ölçüde ilk cihaz kayıt işlemi ve tedarik zinciri Lojistik aygıt üretim sırasında kolaylaştırır. [X.509 CA sertifikaları kullanarak, bu makaledeki senaryo değeri hakkında daha fazla bilgi edinin](iot-hub-x509ca-concept.md) cihaz kimlik doğrulaması için.  Neden adımları mevcut açıklandığı gibi bu senaryo makale devam etmeden önce okumanızı öneririz.

## <a name="prerequisite"></a>Önkoşul

X.509 CA özelliğini kullanarak bir IOT hub'ı hesabına sahip olması gerekir.  [IOT Hub örneği oluşturmayı öğrenin](iot-hub-csharp-csharp-getstarted.md) zaten yoksa.

## <a name="how-to-get-an-x509-ca-certificate"></a>Bir X.509 CA sertifikası alma

Aygıtlarınızı her biri için bir sertifika zinciri üstündeki X.509 CA sertifikasıdır.  Satın alabilir veya nasıl kullanmak istediğinize bağlı olarak oluşturun.

Üretim ortamı için bir ortak kök sertifika yetkilisinden bir X.509 CA sertifikası satın öneririz. Kök CA'ın avantajı sahip bir CA sertifikası satın aygıtlarınızı meşru olup olmadığı için vouch için güvenilir bir üçüncü taraf olarak çalışıyor. Üçüncü taraf ürünleri veya Hizmetleri ile etkileşim kurmak için burada beklenen açık bir IOT ağ parçası olarak aygıtlarınızı istiyorsanız bu seçeneği göz önünde bulundurun.

Ayrıca, otomatik olarak imzalanan bir X.509 CA kullanın veya deneme için kapalı IOT ağlar oluşturabilirsiniz.

Nasıl X.509 CA sertifikası alın, karşılık gelen özel anahtar parolası sakladığınızdan emin olun ve korumalı bakılmaksızın her zaman.  Bu, güven X.509 CA kimlik doğrulama güven ilişkisi oluşturmak için gereklidir. 

Öğrenin nasıl [otomatik olarak imzalanan bir sertifika oluşturmak](iot-hub-security-x509-create-certificates.md#createcerts), bu özellik açıklaması boyunca deneme için kullanabilirsiniz.

## <a name="sign-devices-into-the-certificate-chain-of-trust"></a>Sertifika güven zinciri oturum cihazlarının

Bir X.509 Sertifika sahibinin başka bir ara CA sırayla imzalayabilirsiniz bir ara CA şifreli olarak oturum açabilir ve vb. kadar son ara CA'ın bu işlemi bir aygıtı imzalayarak sonlandırır. Art arda bir sertifika güven zinciri bilinen bir sertifika zinciri sonucudur. Gerçek Hayatta bu aygıtlar imzalama doğrultusunda güven temsilci olarak oynar. Bu temsilci önemlidir, çünkü şifreleme açısından değişken bir gözetim zinciri oluşturur ve anahtarları imzalama paylaşımı önler.

![img-Generic-CERT-chain-of-Trust](./media/generic-cert-chain-of-trust.png)

Burada nasıl [sertifikası zinciri oluşturmak](iot-hub-security-x509-create-certificates.md#createcertchain) aygıtları imzalarken yapıldığı şekilde.

## <a name="how-to-register-the-x509-ca-certificate-to-iot-hub"></a>IOT Hub'ına X.509 CA sertifikası kaydetme

IOT hub'ına burada bunu aygıtlarınızı kaydı ve bağlantı sırasında kimlik doğrulaması için kullanılacak X.509 CA sertifikanızı kaydedin.  X.509 CA sertifikasını kaydetme, sertifika dosyası karşıya yükleme ve kanıtını oluşur iki adımlı bir işlemdir.

Karşıya yükleme işlemi sertifikanızı içeren bir dosyayı karşıya yüklemeyi kapsar.  Bu dosya hiçbir zaman herhangi bir özel anahtarı olmalıdır.

Bir şifreleme sınama ve yanıtlama süreci IOT hub'ı arasındaki elinde adım kanıtı içerir.  Genel ve bu nedenle açıktır. gizli dinleme dijital sertifika içeriği o, IOT hub'ı CA sertifikasını gerçekten sahibi olduğunu onaylaması ister misiniz?  Bu CA sertifikanın karşılık gelen özel anahtarı ile oturum açmanız gerekir rastgele bir sınama oluşturarak bunu.  Özel anahtar gizli ve korumalı yukarıda bıraktıysanız, sonra da bu adımı tamamlamak için bilgi yalnızca sahip, önerilir. Gizliliği özel anahtarların, bu yöntem güvenin kaynağıdır.  Sınama oturum sonra sonuçlarını içeren bir dosyayı karşıya yükleyerek bu adımı tamamlayın.

Burada nasıl [CA sertifikanızı kaydedin](iot-hub-security-x509-get-started.md#registercerts).

## <a name="how-to-create-a-device-on-iot-hub"></a>Bir cihaz IOT hub'ına oluşturma

Cihaz kimliğe bürünme engellemek için IOT Hub, hangi aygıtların beklediğiniz bilmeniz izin gerektirir.  Bunun için IOT Hub'ın cihaz kayıt defterinde bir cihaz girişi oluşturarak.  Bu işlem, IOT hub'ı kullanırken otomatik [aygıt hizmeti sağlama](https://azure.microsoft.com/en-us/blog/azure-iot-hub-device-provisioning-service-preview-automates-device-connection-configuration/) (DPS). 

Burada nasıl [el ile bir cihaz IOT Hub oluşturma](iot-hub-security-x509-get-started.md#createdevice).

## <a name="authenticating-devices-signed-with-x509-ca-certificates"></a>X.509 CA sertifikaları ile imzalanmış aygıtları kimlik doğrulaması

Cihaz, hatta ilk kez bağlandığında, kayıtlı X.509 CA sertifikası ve imzalanmış bir sertifika güven zinciri cihazlarla, nelerin cihaz kimlik doğrulaması şeklindedir.  Bir X.509 CA imzalandığında aygıt bağlanır, kendi sertifika zinciri doğrulaması için yükler. Tüm ara CA ve aygıt sertifika zinciri içerir.  Bu bilgi ile iki adımlı bir işlem cihazı IOT hub'ı kimliğini doğrular.  IOT hub'ı şifreleme açısından iç tutarlılık için sertifika zinciri doğrular ve ardından cihaza elinde kanıt sınama verir.  IOT Hub cihaz gerçek aygıttan başarılı bir elinde kanıt yanıt üzerinde bildirir.  Bu bildirim, yalnızca cihaz, bu sınama başarılı bir şekilde yanıt verebilir ve cihazın özel anahtarı korunmaktadır varsayar.  Özel anahtarları korumak için aygıtları güvenli yongaları gibi donanım güvenli modülleri (HSM) kullanılmasını öneririz.

IOT Hub'ına başarılı cihaz bağlantısı kimlik doğrulama işlemi tamamlandıktan ve ayrıca uygun kurulumunun göstergesi.

Burada nasıl [bu cihaz bağlantısı adımı tamamlamak](iot-hub-security-x509-get-started.md#authenticatedevice).

## <a name="next-steps"></a>Sonraki Adımlar

Hakkında bilgi edinin [X.509 CA'ın kimlik doğrulaması değeri](iot-hub-x509ca-concept.md) IOT içinde.

IOT Hub ile çalışmaya başlama [aygıt hizmeti sağlama](https://docs.microsoft.com/en-us/azure/iot-dps/).