---
title: Kavram, elinde Azure IOT Hub cihazı sağlama hizmeti ile X.509 CA sertifikalarının nasıl | Microsoft Docs
description: Cihaz sağlama hizmeti ile X.509 CA sertifikalarını doğrulama
author: wesmc7777
ms.author: wesmc
ms.date: 02/26/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: afa4b3861e9fb7f91fd9f5d540353c5fad23efe0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "54913623"
---
# <a name="how-to-do-proof-of-possession-for-x509-ca-certificates-with-your-device-provisioning-service"></a>Kavram, elinde cihaz sağlama hizmeti ile X.509 CA sertifikalarının nasıl

Bir doğrulanmış X.509 sertifika yetkilisi (sertifikadır yüklenmiş ve sağlama, kayıtlı bir CA sertifikası CA), hizmet ve kavram, elinde Service aracılığıyla duruma geçti. 

Elinde kavram aşağıdaki adımları içerir:
1. X.509 CA sertifikanız için sağlama hizmeti tarafından oluşturulan benzersiz bir doğrulama kodu alın. Bu işlemi Azure portalından yapabilirsiniz.
2. Kendi konu olarak doğrulama kodunu içeren bir X.509 doğrulama sertifikası oluşturun ve X.509 CA sertifikanızla ilişkili özel anahtara sahip sertifika imzalayın.
3. Hizmete imzalı doğrulama sertifikası yükleyin. Hizmet, bu nedenle, CA sertifikanın özel anahtarı elinde olduğunuzu kanıtlama doğrulanması için CA sertifikasının ortak kısmını kullanarak doğrulama sertifikasını doğrular.

Doğrulanmış sertifikalar, kayıt grupları kullanılırken önemli bir rol oynar. Sertifika sahipliğini doğrulayarak, sertifikanın uploader sertifikanın özel anahtarı elinde sağlayarak bir ek güvenlik katmanı sağlar. Doğrulama cihazlarınızı etkili bir şekilde ele geçirme bir ara sertifika ayıklanırken ve kendi sağlama hizmetinde bir kayıt grubu oluşturmak için bu sertifikayı kullanarak trafiğinizi algılaması kötü amaçlı bir aktör engeller. Kök veya Ara Sertifika bir Sertifika zincirindeki sahipliğini doğrulayan tarafından yaprak sertifikalar, kayıt grubunun bir parçası olarak kayıt cihazları için oluşturma iznine sahip kanıtlama. Bu nedenle, kök veya Ara Sertifika bir kayıt grubunda yapılandırılmış ya da doğrulanmış bir sertifika olmalıdır veya gerekir hizmetiyle doğruladığında bir cihaz Sertifika zincirindeki doğrulanmış bir sertifika hangilerine sunar. Kayıt grupları hakkında daha fazla bilgi için bkz: [X.509 sertifikaları](concepts-security.md#x509-certificates) ve [X.509 sertifikalarıyla sağlama hizmetine cihaz erişimini denetleme](concepts-security.md#controlling-device-access-to-the-provisioning-service-with-x509-certificates).

## <a name="register-the-public-part-of-an-x509-certificate-and-get-a-verification-code"></a>X.509 sertifikasının ortak bölümünü kaydolun ve bir doğrulama kodu alın

Bir CA sertifikası, sağlama hizmetinizle kaydetmek ve kavram, elinde sırasında kullanabileceğiniz doğrulama kodunu almak için aşağıdaki adımları izleyin. 

1. Azure portalında sağlama hizmetinize gidin ve açmak **sertifikaları** sol taraftaki menüden. 
2. Tıklayın **Ekle** yeni bir sertifika eklemek için.
3. Sertifikanız için bir kolay görünen ad girin. X.509 sertifikasının ortak bölümünü temsil eden .cer veya .pem dosyasına göz atın. **Karşıya Yükle**'ye tıklayın.
4. Sertifikanızı başarıyla karşıya yüklendiğini bir bildirim alırsınız bitince **Kaydet**.

    ![Sertifikayı karşıya yükleme](./media/how-to-verify-certificates/add-new-cert.png)  

   Sertifikanızı kategoride görüneceğini **sertifika Gezgini** listesi. Unutmayın **durumu** Bu sertifika *doğrulanmamış*.

5. Önceki adımda eklediğiniz sertifika tıklayın.

6. İçinde **sertifika ayrıntıları**, tıklayın **doğrulama kodu oluştur**.

7. Sağlama hizmeti oluşturur bir **doğrulama kodu** sertifika sahipliği doğrulamak için kullanabilirsiniz. Kodunu panonuza kopyalayın. 

   ![Sertifika doğrulama](./media/how-to-verify-certificates/verify-cert.png)  

## <a name="digitally-sign-the-verification-code-to-create-a-verification-certificate"></a>Doğrulama sertifikası oluşturmak için doğrulama kodu imzala

Şimdi, oturum açmanız gerekir *doğrulama kodu* X.509 CA sertifikanızla ilişkili özel anahtara sahip bir imza oluşturduğu. Bu olarak bilinir [kanıtını](https://tools.ietf.org/html/rfc5280#section-3.1) ve imzalı doğrulama sertifikasını sonuçlanır.

Microsoft araçları sağlar ve yardımcı olabilecek örneklerini imzalı doğrulama sertifikası oluşturun: 

- **Azure IOT Hub C SDK'sı** geliştirme için CA ve yaprak sertifikaları oluşturmanıza yardımcı olacak ve elinde kavram doğrulama kodunu kullanarak gerçekleştirmek için (Windows) PowerShell ve Bash (Linux) komut dosyaları sağlar. İndirebilirsiniz [dosyaları](https://github.com/Azure/azure-iot-sdk-c/tree/master/tools/CACertificates) ilgili bir çalışma klasörü sisteminize ve yönergeleri izleyin [yönetme CA sertifikaları Benioku](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) üzerinde bir CA sertifikası elinde kavram gerçekleştirilecek. 
- **Azure IOT Hub C# SDK'sı** içeren [Grup sertifikası doğrulama örneği](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/master/provisioning/Samples/service/GroupCertificateVerificationSample), hangi elinde kavram yapmak için kullanabilirsiniz.
 
> [!IMPORTANT]
> Elinde kavram gerçekleştirmenin yanı sıra daha önce bahsedilen ayrıca PowerShell ve Bash betikleri kök sertifikaları ve ara sertifika kimlik doğrulaması ve cihazları sağlamak için kullanılan bir yaprak sertifikalar oluşturmanıza imkan tanır. Bu sertifikalar yalnızca geliştirme için kullanılması gerekir. Bir üretim ortamında hiçbir zaman kullanılmamalıdır. 

Belgeler ve SDK'ları sağlanan PowerShell ve Bash betiklerini kullanan [OpenSSL](https://www.openssl.org/). Elinde kavram bunu yapmanıza yardımcı olmak için OpenSSL veya diğer üçüncü taraf araçları da kullanabilirsiniz. SDK'ları ile sağlanan araçları hakkında daha fazla bilgi için bkz: [SDK sağlanan araçları nasıl kullanacağınızı](how-to-use-sdk-tools.md). 


## <a name="upload-the-signed-verification-certificate"></a>İmzalı doğrulama sertifikasını karşıya yükle

1. Sonuçta elde edilen imza portalında sağlama hizmetinize doğrulama sertifikası olarak karşıya yükleyin. İçinde **sertifika ayrıntıları** Azure portalını _dosya Gezgini_ yanındaki simge **doğrulama sertifikası .pem veya .cer dosyası** alan imzalı karşıya yüklemek için sisteminizi sertifikadan doğrulama.

2. Sertifika başarıyla karşıya yüklendikten sonra tıklayın **doğrulama**. **Durumu** sertifika değişikliklerinizin **_doğrulandı_** içinde **sertifika Gezgini** listesi. Tıklayın **Yenile** varsa otomatik olarak güncelleştirilmez.

   ![Karşıya sertifika doğrulama](./media/how-to-verify-certificates/upload-cert-verification.png)  

## <a name="next-steps"></a>Sonraki adımlar

- Kayıt grubu oluşturmak için portalı kullanma hakkında bilgi edinmek için [Azure portalı ile cihaz kayıtlarını yönetme](how-to-manage-enrollments.md).
- Kayıt grubu oluşturmak için hizmet SDK'ları kullanma hakkında bilgi edinmek için [hizmet Sdk'leri ile cihaz kayıtlarını yönetme](how-to-manage-enrollments-sdks.md).










