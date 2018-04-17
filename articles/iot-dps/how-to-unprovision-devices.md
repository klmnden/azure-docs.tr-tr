---
title: Azure IOT Hub cihaz sağlama hizmeti ile sağlanan aygıtlar yetkisini kaldırma nasıl | Microsoft Docs
description: Azure IOT Hub cihaz sağlama hizmeti ile sağlanan deprovision cihazları
services: iot-dps
keywords: ''
author: bryanla
ms.author: v-jamebr;bryanla
ms.date: 04/06/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 439d4ffa8eec12481f52bd15f0060800411f316e
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="how-to-deprovision-devices-that-were-previously-auto-provisioned"></a>Nasıl daha önce otomatik-sağlanan aygıtlar yetkisini kaldırma 

Daha önce otomatik cihaz sağlama hizmeti aracılığıyla sağlanan deprovision cihazlara gerekli bulabilirsiniz. Örneğin, bir cihaz satılan veya farklı bir IOT hub'ına taşınmış veya kayıp, çalınmış veya olabilir Aksi takdirde gizliliği. 

Genel olarak, bir cihaz sağlamayı iki adımdan oluşur:

1. Gelecekte otomatik sağlama önlemek için sağlama hizmetinize aygıttan disenroll. Geçici veya kalıcı olarak erişimi iptal etmek isteyip istemediğinizi bağlı olarak, devre dışı bırakın ya da bir kayıt girişi silmek isteyebilirsiniz. X.509 kanıtlama kullanan cihazlar için devre dışı bırak/mevcut kayıt gruplarınızın hiyerarşideki bir girişi silmek isteyebilirsiniz.  
 
   - Bir aygıt disenroll öğrenmek için bkz: [Azure IOT Hub cihaz hizmeti sağlama aygıttan disenroll nasıl](how-to-revoke-device-access-portal.md).
   - Program aracılığıyla sağlama hizmeti SDK'ları birini kullanarak bir aygıtı disenroll öğrenmek için bkz: [hizmet SDK'ları ile cihaz kayıtlarını yönetme](how-to-manage-enrollments-sdks.md).

2. Gelecekteki iletişim ve veri aktarımını önlemek üzere, IOT Hub CİHAZDAN kaydını silemiyor. Yeniden, geçici olarak devre dışı bırakın ya da nerede sağlandıktan IOT Hub kimlik kayıt defteri girişi cihazın kalıcı olarak silmek. Bkz: [aygıtları devre dışı bırakma](/azure/iot-hub/iot-hub-devguide-identity-registry.md#disable-devices) disablement hakkında daha fazla bilgi edinmek için. "Aygıt Yönetimi / IOT cihazları" IOT hub'ı kaynağınız için görmek [Azure portal](https://portal.azure.com).

Bir aygıt yetkisini kaldırma için uygulayacağınız tam adımlar kanıtlama mekanizması ve sağlama hizmetiniz ile ilgili kayıt girdisini bağlıdır. Aşağıdaki bölümler kayıt ve kanıtlama türüne göre işlemine genel bakış sağlar.

## <a name="individual-enrollments"></a>Tek tek kayıtları
TPM kanıtlama veya X.509 kanıtlama yaprak sertifika ile kullanmak aygıtları tek tek kayıt girişi sağlanır. 

Tek bir kayıt cihaza yetkisini kaldırma için: 

1. Cihaz sağlama hizmetinizden disenroll:

   - TPM kanıtlama kullanan cihazlar için kalıcı olarak cihazın erişimi sağlama hizmeti iptal etmek için tek tek kayıt girişi silmek veya geçici olarak kendi erişimi iptal etmek için giriş devre dışı. 
   - X.509 kanıtlama kullanan cihazlar için silebilir veya giriş devre dışı bırakın. Ancak, X.509 kullanan bir cihazı tek bir kaydını silerseniz ve aygıtın sertifika zinciri, aygıtı yeniden kaydetmek için bir etkin kayıt grubu için bir imza sertifikası var. aklınızda bulundurun. Bu tür aygıtlar için kayıt girişi devre dışı bırakmak daha güvenli olabilir. Bunu yaptığınızda bu nedenle devredilmişse, cihazın etkin kayıt Grup imzalama sertifikalarını birini var olup bakılmaksızın engeller.

2. Hazırlanmış olan IOT hub kimlik kayıt defterinde cihazı silmek veya devre dışı. 


## <a name="enrollment-groups"></a>Kayıt grupları
X.509 kanıtlama ile cihazları bir kayıt grubu aracılığıyla sağlanabilir. Kayıt gruplarını ya da bir ara bir imza sertifikası yapılandırılmış veya kök CA sertifikasını ve bu sertifikayı, sertifika zinciri ile cihazları için sağlama hizmeti erişimi denetlemek. Kayıt grupları ve sağlama hizmetiyle X.509 sertifikaları hakkında daha fazla bilgi için bkz: [X.509 sertifikalarını](concepts-security.md#x509-certificates). 

Bir kayıt grubu üzerinden sağlanan cihazların bir listesini görmek için kayıt grubun ayrıntıları görüntüleyebilirsiniz. Bu, her cihaz için sağlanan hangi IOT hub'ı anlamak için kolay bir yoludur. Cihaz listesini görüntülemek için: 

1. Azure portalında oturum açın ve tıklatın **tüm kaynakları** sol menüdeki.
2. Sağlama hizmetinizi kaynak listesine tıklayın.
3. Sağlama hizmetinizi tıklatın **kayıtlarını yönetme**seçeneğini belirleyip **kayıt grupları** sekmesi.
4. Kayıt grubu açmak için tıklatın.

   ![Kayıt Grup giriş portalında görüntüleyin](./media/how-to-unprovision-devices/view-enrollment-group.png)

Kayıt gruplarıyla dikkate alınması gereken iki senaryo vardır:

- Bir kayıt grubu üzerinden sağlanan tüm aygıtları yetkisini kaldırma için:
  1. İmzalama sertifikasını kara listeye kayıt grubu devre dışı bırakın. 
  2. Kendi IOT hub kimlik kayıt defterinden her cihazı silmek veya devre dışı bırakmak sağlanan cihazlar için bu kayıt Grup listesini kullanın. 
  3. Devre dışı bırakma veya tüm cihazlar ilgili kendi IOT hub'larından sildikten sonra isteğe bağlı olarak kayıt grubu silebilirsiniz. Ancak, kayıt grubunu silmek ve etkin kayıt grubu sertifika zincirinde daha yüksek bir imza sertifikası için bir veya daha fazla aygıtları varsa, bu aygıtların yeniden kaydetmek için unutmayın. 

- Tek bir cihaz kayıt grubundan yetkisini kaldırma için:
  1. Yaprak (cihaz) sertifikasını için devre dışı bırakılmış tek tek bir kayıt oluşturun. Bu, bu cihaz için sağlama hizmetine erişim hala izin veren kendi zincirinde kayıt grubun imzalama sertifikasına sahip diğer cihazlar için erişimi iptal eder. Cihaz devre dışı bırakılmış tek tek kaydını silmeyin. Bunun yapılması kayıt grubu üzerinden yeniden kaydetmek için cihazın izin verir. 
  2. Cihaz için hazırlanmış olan IOT hub'ı bulmak için bu kayıt grubu için sağlanan aygıtların listesini kullanın ve devre dışı veya bu hub'ın kimlik kayıt defterinden silin. 
  
  










