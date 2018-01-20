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
ms.openlocfilehash: 83ed72c0f2eb342c372ef97e5443d60eab1e2174
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2018
---
# <a name="how-to-unprovision-devices-enrolled-by-your-provisioning-service"></a>Sağlama hizmetiniz tarafından kaydedilen cihazlar sağlamasını yapma

Cihaz sağlama hizmeti aracılığıyla sağlanan sıfırladığından cihazlara gerekli bulabilirsiniz. Örneğin, bir cihaz satılan veya farklı bir IOT hub'ına taşınmış veya kayıp, çalınmış veya olabilir Aksi takdirde gizliliği. 

Genel olarak, bir cihaz sağlamanın kaldırılması iki adımdan oluşur:

1. Cihaz sağlama hizmetiniz için erişimi iptal. Geçici veya kalıcı olarak erişimi iptal etmek istediğinize bağlı olarak, veya, söz konusu olduğunda X.509 kanıtlama mekanizması mevcut kayıt gruplarınızı hiyerarşisini devre dışı bırakın ya da bir kayıt girişi silmek isteyebilirsiniz. 
 
   - Portalı kullanarak cihaz erişimi iptal etme öğrenmek için bkz: [cihaz erişimi iptal](how-to-revoke-device-access-portal.md).
   - Program aracılığıyla sağlama hizmeti SDK'ları birini kullanarak cihaz erişimi iptal öğrenmek için bkz: [hizmet SDK'ları ile cihaz kayıtlarını yönetme](how-to-manage-enrollments-sdks.md).

2. Burada sağlandıktan IOT Hub kimlik kayıt defterinde cihazın girişi silmek veya devre dışı. Daha fazla bilgi için bkz: [aygıt kimlikleri yönetmek](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-identity-registry#disable-devices) Azure IOT Hub belgelerinde. 

Bir aygıt sağlamasını olacak adımların kullandığı kanıtlama mekanizması bağlıdır.

TPM kanıtlama veya X.509 kanıtlama bir otomatik olarak imzalanan Yaprak sertifikası (sertifika zinciri) kullanan cihazları tek tek kayıt girişi sağlanır. Bu cihazlar için kalıcı olarak cihazın erişimi sağlama hizmeti iptal etmek veya geçici olarak kendi erişim ve ardından izleme silerek veya cihaz IOT ile kimlik kayıt defterinde devre dışı bırakma iptal etmek için giriş devre dışı bırakmak giriş silebilirsiniz. hazırlanmış olan hub.

X.509 kanıtlama ile cihazları bir kayıt grubu aracılığıyla sağlanabilir. Kayıt gruplarını ya da bir ara bir imza sertifikası yapılandırılmış veya kök CA sertifikasını ve bu sertifikayı, sertifika zinciri ile cihazları için sağlama hizmeti erişimi denetlemek. Kayıt grupları ve sağlama hizmetiyle X.509 sertifikaları hakkında daha fazla bilgi için bkz: [X.509 sertifikalarını](concepts-security.md#x509-certificates). 

Bir kayıt grubu üzerinden sağlanan cihazların bir listesini görmek için kayıt grubun ayrıntıları görüntüleyebilirsiniz. Bu, her cihaz için sağlanan hangi IOT hub'ı anlamak için kolay bir yoludur. Cihaz listesini görüntülemek için: 

1. Azure portalında oturum açın ve tıklatın **tüm kaynakları** sol menüdeki.
2. Sağlama hizmetinizi kaynak listesine tıklayın.
3. Sağlama hizmetinizi tıklatın **kayıtlarını yönetme**seçeneğini belirleyip **kayıt grupları** sekmesi.
4. Kayıt grubu açmak için tıklatın.

   ![Kayıt Grup giriş portalında görüntüleyin](./media/how-to-unprovision-devices/view-enrollment-group.png)

Kayıt gruplarıyla dikkate alınması gereken iki senaryo vardır:

- Bir kayıt grubu üzerinden sağlanan tüm aygıtları sağlamayı kaldırmak için ilk imzalama sertifikasını kara listeye kayıt grubu devre dışı bırakmanız gerekir. Ardından ilgili kendi IOT hub kimlik kayıt defterinden her cihazı silmek veya devre dışı bırakmak, kayıt grubu için sağlanan aygıtların listesini kullanabilirsiniz. Devre dışı bırakma veya tüm cihazlar ilgili kendi IOT hub'larından sildikten sonra isteğe bağlı olarak kayıt grubu silebilirsiniz. Ancak, kayıt grubunu silmek ve etkin kayıt grubu sertifika zincirinde daha yüksek bir imza sertifikası için bir veya daha fazla aygıtları varsa, bu aygıtların yeniden kaydetmek için unutmayın. 
- Tek bir cihaz kayıt grubundan sağlamayı kaldırmak için önce yaprak (cihaz) sertifikasını için devre dışı bırakılmış bir bireysel kaydı oluşturmanız gerekir. Bu, bu cihaz için sağlama hizmetine erişim hala izin veren kendi zincirinde kayıt grubun imzalama sertifikasına sahip diğer cihazlar için erişimi iptal eder. Ardından cihaz için hazırlanmış olan IOT hub'ı bulun ve devre dışı bırakın veya bu hub'ın kimlik kayıt defterinden silmek için kayıt Grup ayrıntılarında sağlanan aygıtların listesi kullanabilirsiniz. Cihaz devre dışı bırakılmış tek tek kaydını silmeyin. Bunun yapılması kayıt grubu üzerinden yeniden kaydetmek için cihazın izin verir. 










