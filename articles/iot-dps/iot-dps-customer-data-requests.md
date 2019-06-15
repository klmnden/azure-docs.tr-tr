---
title: Müşteri verilerini talep özellikleri
author: dominicbetts
ms.author: dobett
manager: timlt
ms.date: 05/16/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: 5dd027c886e8102e77ddefe93817daee0e1ec29b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60626443"
---
# <a name="summary-of-customer-data-request-features"></a>Müşteri verilerini talep özelliklerin özeti

Azure IOT Hub cihazı sağlama hizmeti sorunsuz, otomatik sıfır dokunma cihazlarını Azure IOT hub'a cihazda başlar ve bulut ile biten güvenlik sağlama sağlayan kurumsal müşteriler hedeflenen bir REST API tabanlı bir bulut hizmetidir.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

Tek cihazlara bir kayıt kimliği ve cihaz Kimliğini, bir kiracı Yöneticisi tarafından atanır. Veri ve bu cihazlar hakkında bu kimliklerine göre temel alır. Microsoft hiçbir bilgi tutar ve bu cihazların bir kişiye bağıntı izin verilerine erişim yok.

Birçok cihaz sağlama hizmetinde yönetilen cihazların, örneğin bir office thermostat veya Fabrika robot kişisel cihazlarını değildir. Müşteriler, ancak bazı cihazların kişisel olmasını dikkate almanız ve kendi kararımıza kendi varlık veya envanter izleme cihazlar kişilere tie yöntemi olması. Cihaz sağlama hizmeti yönetir ve kişisel veriler gibi cihazlar ile ilişkili tüm verileri depolar.

Kiracı yöneticileri vererek bilgi istekleri için kullanabilir Azure portalından veya hizmetin REST API'leri veya bir cihaz kimliği veya kayıt kimliği ile ilişkili verileri silme

> [!NOTE]
> Azure IOT hub cihazı sağlama hizmeti aracılığıyla sağlanan cihazların Azure IOT hub'ı hizmetinde depolanan ek veri vardır. Bkz: [Azure IOT hub'ı başvuru belgeleri](../iot-hub/iot-hub-customer-data-requests.md) belirli bir cihaz için tam bir istek tamamlamak için.

## <a name="deleting-customer-data"></a>Müşteri verileri silme

Cihaz sağlama hizmeti, kayıtlar ve kayıt kayıt depolar. Kayıtları sağlanması için izin verilen cihazları ve hangi cihazlar sağlama sürecinde zaten sahiplikten kaydı kayıtlarını Göster hakkında bilgiler içerir.

Kiracı yöneticileri kayıtlarını Azure portalından kaldırabilir ve bu da tüm ilişkili kayıt kayıtları kaldırır.

Daha fazla bilgi için [cihaz kayıtlarını yönetme](how-to-manage-enrollments.md).

Kayıtlar ve REST API'leri kullanarak kayıt kayıt silme işlemleri gerçekleştirmek mümkündür:

* Tek bir cihaz kayıt bilgilerini silmek için kullanabileceğiniz [cihaz kaydı - silme](/rest/api/iot-dps/deleteindividualenrollment/deleteindividualenrollment).
* Cihaz grubu için kayıt bilgileri silmek için kullanabileceğiniz [cihaz kayıt grubu - silme](/rest/api/iot-dps/deleteenrollmentgroup/deleteenrollmentgroup).
* Sağlanan cihazlara ilişkin bilgileri silmek için kullanabileceğiniz [kayıt durumu - silme kayıt durumu](/rest/api/iot-dps/deletedeviceregistrationstate/deletedeviceregistrationstate).

## <a name="exporting-customer-data"></a>Müşteri verilerini dışarı aktarma

Cihaz sağlama hizmeti, kayıtlar ve kayıt kayıt depolar. Kayıtları sağlanması için izin verilen cihazları ve hangi cihazlar sağlama sürecinde zaten sahiplikten kaydı kayıtlarını Göster hakkında bilgiler içerir.

Kiracı yöneticileri, kayıtlar ve kayıt kayıtlarını Azure portalından görüntülemek ve bunları dışarı aktarmak kopyalama ve yapıştırmayı kullanma.

Kayıtları yönetme hakkında daha fazla bilgi için bkz. [cihaz kayıtlarını yönetme](how-to-manage-enrollments.md).

Kayıtlar ve kayıt kayıtları REST API'lerini kullanarak dışa aktarma işlemleri gerçekleştirmek mümkündür:

* Tek bir cihaz için kayıt bilgileri dışa aktarmak için kullanabileceğiniz [cihaz kaydı - Get](/rest/api/iot-dps/getindividualenrollment/getindividualenrollment).
* Bir cihaz grubu için kayıt bilgileri dışa aktarmak için kullanabileceğiniz [cihaz kayıt grubu - Get](/rest/api/iot-dps/getenrollmentgroup/getenrollmentgroup).
* Önceden hazırlanmış cihazlar hakkında bilgi vermek için kullanabileceğiniz [kayıt durumu - Get kayıt durumu](/rest/api/iot-dps/getdeviceregistrationstate/getdeviceregistrationstate).

> [!NOTE]
> Microsoft'un Kurumsal hizmet kullandığınızda, sistem tarafından oluşturulan günlükleri olarak bilinen bazı bilgiler, Microsoft oluşturur. Bazı cihaz sağlama hizmeti sistem tarafından oluşturulan günlükler, erişilebilir veya Kiracı yöneticileri tarafından verilebilir değildir. Bu günlükler, cihazlara ayrı ayrı ilgili tanılama veri ve hizmet içinde gerçekleştirilen yansıttığından eylemleri oluşturur.

## <a name="links-to-additional-documentation"></a>Ek belgelere bağlantılar

Tüm belgeler için cihaz sağlama hizmeti API'lerine adresindedir [ https://docs.microsoft.com/rest/api/iot-dps ](https://docs.microsoft.com/rest/api/iot-dps).

Azure IOT hub'ı [istek özellikleri müşteri verilerini](../iot-hub/iot-hub-customer-data-requests.md).