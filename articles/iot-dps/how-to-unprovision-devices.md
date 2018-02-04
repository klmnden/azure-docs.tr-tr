---
title: "Aygıtları sağlamasını nasıl kayıtlı Azure IOT Hub cihaz hizmeti sağlama | Microsoft Docs"
description: "Azure Portalı'nda DPS hizmetiniz tarafından kaydedilen cihazlar sağlamasını yapma"
services: iot-dps
keywords: 
author: JimacoMS
ms.author: v-jamebr
ms.date: 01/08/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 1d057a4df43cf25e6817672d198207d9a50e462e
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="how-to-unprovision-devices-enrolled-by-your-provisioning-service"></a>Sağlama hizmetiniz tarafından kaydedilen cihazlar sağlamasını yapma

Cihaz sağlama hizmeti aracılığıyla sağlanan sıfırladığından cihazlara gerekli bulabilirsiniz. Örneğin, bir cihaz satılan veya farklı bir IOT hub'ına taşınmış veya kayıp, çalınmış veya olabilir Aksi takdirde gizliliği. 

Genel olarak, bir cihaz sağlamanın kaldırılması iki adımdan oluşur:

1. Cihaz sağlama hizmetiniz için erişimi iptal. Geçici veya kalıcı olarak erişimi iptal etmek istediğinize bağlı olarak, veya, söz konusu olduğunda X.509 kanıtlama mekanizması mevcut kayıt gruplarınızı hiyerarşisini devre dışı bırakın ya da bir kayıt girişi silmek isteyebilirsiniz. 
 
   - Portalı kullanarak cihaz erişimi iptal etme öğrenmek için bkz: [cihaz erişimi iptal](how-to-revoke-device-access-portal.md).
   - Program aracılığıyla sağlama hizmeti SDK'ları birini kullanarak cihaz erişimi iptal öğrenmek için bkz: [hizmet SDK'ları ile cihaz kayıtlarını yönetme](how-to-manage-enrollments-sdks.md).

2. Burada sağlandıktan IOT Hub kimlik kayıt defterinde cihazın girişi silmek veya devre dışı. Daha fazla bilgi için bkz: [aygıt kimlikleri yönetmek](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-identity-registry#disable-devices) Azure IOT Hub belgelerinde. 

Bir aygıt sağlamasını olacak adımların kanıtlama mekanizması ve sağlama hizmetiniz ile ilgili kayıt girdisini bağlıdır.

## <a name="individual-enrollments"></a>Tek tek kayıtları
TPM kanıtlama veya X.509 kanıtlama yaprak sertifika ile kullanmak aygıtları tek tek kayıt girişi sağlanır. 

Tek bir kayıt cihaza sağlamayı kaldırmak için: 
1. TPM kanıtlama kullanan cihazlar için kalıcı olarak cihazın erişimi sağlama hizmeti iptal etmek veya geçici olarak kendi erişimi iptal etmek için giriş devre dışı bırakmak tek tek kayıt girişi silin. X.509 kanıtlama kullanan cihazlar için silebilir veya giriş devre dışı bırakın. Ancak, bir cihazın tek tek bir kaydını silerseniz X.509 kanıtlama kullanan ve aygıtın sertifika zinciri, aygıtı yeniden kaydetmek için bir etkin kayıt grubu için bir imza sertifikası var olduğunu aklınızda bulundurun. Bu tür aygıtlar için kayıt girişi devre dışı bırakmak daha güvenli olabilir. Bunu yaptığınızda bu nedenle devredilmişse, cihazın etkin kayıt Grup imzalama sertifikalarını birini var olup bakılmaksızın engeller.
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

- Bir kayıt grubu üzerinden sağlanan tüm aygıtları sağlamayı kaldırmak için:
  1. İmzalama sertifikasını kara listeye kayıt grubu devre dışı bırakın. 
  2. Kendi IOT hub kimlik kayıt defterinden her cihazı silmek veya devre dışı bırakmak sağlanan cihazlar için bu kayıt Grup listesini kullanın. 
  3. Devre dışı bırakma veya tüm cihazlar ilgili kendi IOT hub'larından sildikten sonra isteğe bağlı olarak kayıt grubu silebilirsiniz. Ancak, kayıt grubunu silmek ve etkin kayıt grubu sertifika zincirinde daha yüksek bir imza sertifikası için bir veya daha fazla aygıtları varsa, bu aygıtların yeniden kaydetmek için unutmayın. 
- Tek bir cihaz kayıt grubundan sağlamayı kaldırmak için:
  1. Yaprak (cihaz) sertifikasını için devre dışı bırakılmış tek tek bir kayıt oluşturun. Bu, bu cihaz için sağlama hizmetine erişim hala izin veren kendi zincirinde kayıt grubun imzalama sertifikasına sahip diğer cihazlar için erişimi iptal eder. Cihaz devre dışı bırakılmış tek tek kaydını silmeyin. Bunun yapılması kayıt grubu üzerinden yeniden kaydetmek için cihazın izin verir. 
  2. Cihaz için hazırlanmış olan IOT hub'ı bulmak için bu kayıt grubu için sağlanan aygıtların listesini kullanın ve devre dışı veya bu hub'ın kimlik kayıt defterinden silin. 
  
  










