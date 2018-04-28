---
title: Kanıt in elinde X.509 CA sertifikalarının Azure IOT Hub cihaz sağlama hizmeti ile nasıl | Microsoft Docs
description: DPS hizmetinizle X.509 CA sertifikaları nasıl
services: iot-dps
keywords: ''
author: bryanla
ms.author: v-jamebr
ms.date: 02/26/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: eb37ce7e61796494be0a9282afdc620b0ca5886a
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="how-to-do-proof-of-possession-for-x509-ca-certificates-with-your-device-provisioning-service"></a>Kanıt in elinde aygıt hizmetinizle sağlama X.509 CA sertifikalarının nasıl

Bir doğrulanmış X.509 sertifika yetkilisi (karşıya ve sağlama, kayıtlı bir CA sertifikası sertifikasıdır CA), hizmet ve kanıt in elinde hizmetiyle aracılığıyla geçti. 

Elinde kanıt aşağıdaki adımları içerir:
1. X.509 CA sertifikanız için sağlama hizmeti tarafından oluşturulan benzersiz doğrulama kodunu alın. Bu Azure portalından yapabilirsiniz.
2. Kendi konu olarak doğrulama kodunu içeren bir X.509 doğrulama sertifikası oluşturun ve X.509 CA sertifikayla ilişkili özel anahtara sahip sertifika imzalayın.
3. İmzalı doğrulama sertifikasını hizmete yükleyin. Hizmet, bu nedenle CA sertifikanın özel anahtarı elinde olduğunu kanıtlayan doğrulanması için CA sertifikasının ortak kısmını kullanarak doğrulama sertifikasını doğrular.

Doğrulanmış sertifikaları kayıt grupları kullanırken önemli bir rol oynar. Sertifika sahipliğini doğrulama sertifikanın özel anahtarı elinde sertifikanın karşıya yükleyen olan sağlayarak bir ek güvenlik katmanı sağlar. Doğrulama cihazlarınızı etkili bir şekilde ele geçirme ara sertifika ayıklanması ve kendi sağlama hizmetinde bir kayıt grubu oluşturmak için bu sertifikayı kullanarak trafiğinizi algılaması kötü amaçlı bir aktör engeller. Kök veya Ara Sertifika bir Sertifika zincirindeki sahipliğini kanıtlayan tarafından bu kayıt grubunun bir parçası kaydetme aygıtlara yaprak sertifikalarını oluşturmak için izne sahip kanıtlayan. Bu nedenle, kök veya ara sertifika kayıt grubunda yapılandırılmış ya da doğrulanmış bir sertifikası olmalıdır veya gerekir hizmetiyle doğruladığında bir aygıt Sertifika zincirindeki doğrulanmış bir sertifika hangilerine gösterir. Kayıt grupları hakkında daha fazla bilgi edinmek için [X.509 sertifikalarını](concepts-security.md#x509-certificates) ve [X.509 sertifikalarını sağlama hizmetiyle cihaz erişimini denetleme](concepts-security.md#controlling-device-access-to-the-provisioning-service-with-x509-certificates).

## <a name="register-the-public-part-of-an-x509-certificate-and-get-a-verification-code"></a>Bir X.509 sertifikası ortak parçası kaydetmek ve bir doğrulama kodu alın

Bir CA sertifikası, sağlama Hizmeti'ne kaydolmak ve kanıt in elinde sırasında kullanabileceğiniz bir doğrulama kodu almak için aşağıdaki adımları izleyin. 

1. Azure portalında sağlama hizmetinize gidin ve açmak **sertifikaları** sol taraftaki menüden. 
2. Tıklatın **Ekle** yeni bir sertifika eklemek için.
3. Sertifikanız için kolay bir görünen ad girin. X.509 sertifikası ortak parçasını temsil eden .cer veya .pem dosyasına gözatın. **Karşıya Yükle**'ye tıklayın.
4. Sertifikanızı başarıyla karşıya yüklendiğini bildirimi aldıktan sonra tıklatın **kaydetmek**.

    ![Sertifikayı karşıya yükleme](./media/how-to-verify-certificates/add-new-cert.png)  

   Sertifikanızı kategoride görüneceğini **sertifika Explorer** listesi. Unutmayın **durum** Bu sertifika *Unverified*.

5. Önceki adımda eklediğiniz sertifika'yı tıklatın.

6. İçinde **sertifika ayrıntıları**, tıklatın **doğrulama kodu oluştur**.

7. Sağlama hizmeti oluşturur bir **doğrulama kodu** sertifika sahipliği doğrulamak için kullanabilirsiniz. Kodunu panonuza kopyalayın. 

   ![Sertifika doğrulayın](./media/how-to-verify-certificates/verify-cert.png)  

## <a name="digitally-sign-the-verification-code-to-create-a-verification-certificate"></a>Doğrulama sertifikası oluşturmak için doğrulama kodu dijital olarak imzala

Şimdi, oturum açmanız gerekir *doğrulama kodu* X.509 CA sertifikayla ilişkili özel anahtarı ile hangi oluşturur imza. Bu olarak bilinir [kanıtını](https://tools.ietf.org/html/rfc5280#section-3.1) ve imzalı doğrulama sertifikası sonuçlanır.

Microsoft araçları sağlar ve yardımcı olabilecek örnek imzalı doğrulama sertifikası oluşturun: 

- **Azure IOT Hub C SDK'sı** geliştirme için CA ve yaprak sertifikaları oluşturmanıza yardımcı olması için ve elinde kanıt doğrulama kodunu kullanarak gerçekleştirmek için PowerShell'i (Windows) ve Bash (Linux) komut dosyaları sağlar. İndirebilirsiniz [dosyaları](https://github.com/Azure/azure-iot-sdk-c/tree/master/tools/CACertificates) sisteminize çalışma klasörü için ilgili ve yönergeleri izleyin [yönetme CA sertifikaları Benioku](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) üzerindeki bir CA sertifikasını elinde kanıt gerçekleştirmek için. 
- **Azure IOT hub'ı C# SDK'sı** içeren [Grup sertifika doğrulama örnek](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/provisioning/service/samples/GroupCertificateVerificationSample), hangi mülkü kanıt yapmak için kullanabilirsiniz.
- İçindeki adımları takip edebilirsiniz [CA tarafından imzalanmış X.509 sertifikalarını yönetmek için PowerShell betiklerini](https://docs.microsoft.com/azure/iot-hub/iot-hub-security-x509-create-certificates) özellikle başlıklı bölümde belirtilen komut dosyası IOT Hub belgeleri makalesinde [elinde kanıtı, X.509 CA sertifikasını](https://docs.microsoft.com/azure/iot-hub/iot-hub-security-x509-create-certificates#signverificationcode).
 
> [!IMPORTANT]
> Elinde kanıt gerçekleştirmenin yanı sıra, daha önce de bildirdi PowerShell ve Bash betiklerini kök sertifikaları, Ara sertifikalar ve kimlik doğrulaması ve cihazlara sağlamak için kullanılan yaprak sertifikaları oluşturmanızı sağlar. Bu sertifikalar yalnızca geliştirme için kullanılmalıdır. Bunlar, hiçbir zaman bir üretim ortamında kullanılmamalıdır. 

Belgeleri ve SDK'ları sağlanan PowerShell ve Bash betiklerini kullanır [OpenSSL](https://www.openssl.org/). Elinde kanıt yapmanıza yardımcı olmak için OpenSSL veya diğer üçüncü taraf araçlarını kullanabilir. SDK'ları ile sağlanan araçları hakkında daha fazla bilgi için bkz: [SDK sağlanan araçları nasıl kullanacağınızı](how-to-use-sdk-tools.md). 


## <a name="upload-the-signed-verification-certificate"></a>İmzalı doğrulama sertifikasını karşıya yükle

1. Sonuçta elde edilen imza doğrulama sertifikası portalında sağlama hizmetinize olarak karşıya yükleyin. İçinde **sertifika ayrıntıları** Azure Portal'da kullanmak _dosya Gezgini_ yanındaki simge **doğrulama sertifikası .pem veya .cer dosyasını** alan imzalı karşıya yüklemek için doğrulama sertifikası sisteminizden.

2. Sertifika başarıyla yüklendikten sonra tıklatın **doğrula**. **Durum** sertifika değişikliklerinizi **_doğrulandı_** içinde **sertifika Explorer** listesi. Tıklatın **yenileme** varsa otomatik olarak güncelleştirmez.

   ![Sertifika doğrulama karşıya yükle](./media/how-to-verify-certificates/upload-cert-verification.png)  

## <a name="next-steps"></a>Sonraki adımlar

- Portal bir kayıt grubu oluşturmak için nasıl kullanılacağı hakkında bilgi edinmek için [Azure portal ile cihaz kayıtlarını yönetme](how-to-manage-enrollments.md).
- Bir kayıt grubu oluşturmak için hizmet SDK'ları kullanma hakkında bilgi edinmek için [hizmet SDK'ları ile cihaz kayıtlarını yönetme](how-to-manage-enrollments-sdks.md).










