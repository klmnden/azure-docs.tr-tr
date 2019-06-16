---
title: Azure IOT Hub cihazı sağlama hizmeti ile sağlanan cihazları sağlamasını kaldırmak nasıl | Microsoft Docs
description: Azure IOT Hub cihazı sağlama hizmeti ile sağlanan sağlamayı kaldırma cihazları
author: wesmc7777
ms.author: wesmc
ms.date: 05/11/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: ba62d8cff646ce5ef4f4b8a36fdad78ddc354227
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60626527"
---
# <a name="how-to-deprovision-devices-that-were-previously-auto-provisioned"></a>Nasıl daha önce otomatik olarak sağlanan cihazları sağlamasını kaldırmak için 

Daha önce otomatik olarak cihaz sağlama hizmeti sağlanan sağlamayı kaldırma cihazlara gerekli bulabilirsiniz. Örneğin, bir cihaz satılan veya farklı bir IOT hub'ına taşınmış veya kayıp, çalınmış veya olabilir Aksi takdirde gizliliği. 

Genel olarak, bir cihaz sağlamayı kaldırma iki adımdan oluşur:

1. Gelecekte otomatik sağlama önlemek için sağlama hizmetinize CİHAZDAN disenroll. Erişim geçici veya kalıcı olarak iptal etmek istediğinize bağlı olarak, devre dışı bırakın ya da kayıt girişi silmek isteyebilirsiniz. X.509 kanıtlama kullanan cihazlar için bir giriş var olan kayıt grupları hiyerarşisi içinde devre dışı bırakma/silme isteyebilirsiniz.  
 
   - Bir cihaz disenroll öğrenmek için bkz. [nasıl bir CİHAZDAN bir Azure IOT Hub cihazı sağlama hizmeti disenroll](how-to-revoke-device-access-portal.md).
   - Bir cihaz sağlama hizmeti Sdk'lardan birini kullanarak program aracılığıyla disenroll öğrenmek için bkz. [hizmet Sdk'leri ile cihaz kayıtlarını yönetme](how-to-manage-enrollments-sdks.md).

2. Gelecekteki iletişim ve veri aktarımını önlemek için IOT Hub CİHAZDAN kaydını silemiyor. Yeniden geçici olarak devre dışı veya cihazın giriş kimlik kayıt defterinde burada sağlanan IOT Hub için kalıcı olarak sil. Bkz: [cihazları devre dışı bırak](/azure/iot-hub/iot-hub-devguide-identity-registry#disable-devices) disablement hakkında daha fazla bilgi edinmek için. "Cihaz Yönetimi / IOT cihazları" IOT hub'ı kaynağınızın görmek [Azure portalında](https://portal.azure.com).

Cihaz sağlamasını kaldırmak için uygulayacağınız tam adımlar, kanıtlama mekanizması ve sağlama hizmetinizle ilgili bir kayıt girdisini bağlıdır. Aşağıdaki bölümler, kayıt ve kanıtlama türüne göre işlemi, genel bir bakış sağlar.

## <a name="individual-enrollments"></a>Bireysel kayıtlar
TPM kanıtlama veya X.509 kanıtlama ile bir yaprak sertifikayı kullanan cihazları bireysel kayıt girişi sağlanır. 

Bireysel kayıt olan bir cihazda sağlamasını kaldırmak için: 

1. Cihaz sağlama hizmetinizde disenroll:

   - TPM kanıtlama kullanan cihazlar için cihazın erişim sağlama hizmetine kalıcı olarak iptal etmek için bireysel kayıt girişi silebilir veya geçici olarak erişimini iptal etmek için giriş devre dışı. 
   - X.509 kanıtlama kullanan cihazlar için silmek veya giriş devre dışı bırakın. Ancak, cihazın sertifika zinciri, cihaz yeniden kaydetmek için bir imza sertifikası için kullandığı X.509 cihazı için bireysel bir kayıt silme ve etkin kayıt grubu varsa, dikkat edin. Bu tür cihazlar için kayıt girişi devre dışı bırakmak daha güvenli olabilir. Bunu yapmak, bu nedenle gerektireceğini, cihazın etkin kayıt grubu imzalama sertifikalarını birini var olup bakılmaksızın engeller.

2. Devre dışı bırakın veya cihaz için sağlanan IOT hub kimlik kayıt defterinde silin. 


## <a name="enrollment-groups"></a>Kayıt grupları
X.509 kanıtlama ile cihazları bir kayıt grubu sağlanabilir. Kayıt grupları ya da bir ara bir imza sertifikası yapılandırılır veya kök CA sertifikasını ve bu sertifika, sertifika zincirlerinde ile cihazları için sağlama hizmetine erişimi denetler. Kayıt grupları ve X.509 sertifikalarıyla sağlama hizmeti hakkında daha fazla bilgi için bkz: [X.509 sertifikaları](concepts-security.md#x509-certificates). 

Kayıt grubu sağlanan cihazların listesini görmek için kayıt grubunun ayrıntılarını görüntüleyebilirsiniz. Bu, her cihaz için sağlanan hangi IOT hub'a anlaşılması kolay bir yoludur. Cihaz listesini görüntülemek için: 

1. Azure portalında oturum açın ve tıklayın **tüm kaynakları** sol menüdeki.
2. Sağlama hizmetinize kaynak listesine tıklayın.
3. Sağlama hizmetinize tıklayın **kayıtları Yönet**, ardından **kayıt grupları** sekmesi.
4. Kayıt grubu açmak için tıklayın.

   ![Portalda kayıt grubu girdisini görüntüleme](./media/how-to-unprovision-devices/view-enrollment-group.png)

Kayıt gruplarıyla dikkate alınması gereken iki senaryo vardır:

- Kayıt grubu sağlanan tüm aygıtları sağlamasını kaldırmak için:
  1. İmzalama sertifikasını kara listeye kayıt grubu devre dışı bırakın. 
  2. Kendi IOT hub kimlik kayıt defterinden her cihazı silmek veya devre dışı bırakmak, kayıt grubu için sağlanan cihaz listesini kullanın. 
  3. Devre dışı bırakma veya kendi ilgili IOT hub'ları tüm cihazları sildikten sonra isteğe bağlı olarak kayıt grubu silebilirsiniz. Ancak, kayıt grubunu silin ve sertifika zincirine daha yüksek bir imza sertifikası için etkin kayıt grubu bir veya daha fazla cihaz varsa, bu cihazları yeniden kaydedebilirsiniz, unutmayın. 

- Kayıt grubu tek bir CİHAZDAN sağlamasını kaldırmak için:
  1. Yaprak (cihaz) sertifikasını devre dışı bırakılmış bir bireysel kayıt oluşturun. Bu, bu cihaz için sağlama hizmetine erişiminizi kayıt Grup imzalama sertifikasını kendi zincirine sahip diğer cihazlar için erişimi hala kullanılmasına da izin veren iptal eder. Cihaz devre dışı bireysel kayıt silmeyin. Bunun yapılması, cihaz kayıt grubu yeniden kaydetmeye izin verir. 
  2. Cihaz için sağlanan IOT hub'ı bulmak için bu kayıt grubu için sağlanan cihaz listesini kullanın ve devre dışı bırakmak veya bu hub'ının kimlik kayıt defterinden silebilirsiniz. 
  
  










