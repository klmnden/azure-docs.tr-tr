---
title: Müşteri verileri istek özellikleri
author: dominicbetts
ms.author: dobett
ms.date: 05/16/2018
ms.topic: conceptual
ms.service: iot-dps
ms.openlocfilehash: 28cadac33c4e73e6158f878b3c79ff09b4765fff
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="summary-of-customer-data-request-features"></a>Müşteri verileri isteği özelliklerinin özeti

Azure IOT Hub cihaz sağlama hizmeti sorunsuz, otomatik zero touch Azure IOT Hub'ına aygıtların konumunda aygıttan başlar ve Bulutu ile biten güvenlik sağlama sağlayan kurumsal müşteriler hedeflenen bir REST API tabanlı bulut hizmetidir.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

Bireysel aygıtlar bir kayıt kimliği ve cihaz kimliği bir kiracı Yöneticisi tarafından atanır. Gelen ve bu aygıtlar hakkında veri üzerinde bu kimlikleri temel alır. Microsoft hiçbir bilgi tutar ve bu aygıtların bir kişiye bağıntı izin verilere erişim yok.

Birçok cihaz sağlama hizmetinde yönetilen aygıtlar örneğin office thermostat veya Fabrika robot kişisel aygıtlar değildir. Müşteriler, ancak, bazı cihazların kişisel olmasını düşünebilirsiniz ve kendi istediğiniz kadar kendi varlık veya izleme kişiler cihazlara tie yöntemlerini stok korumak. Cihaz sağlama hizmeti yönetir ve kişisel veriler gibi cihazlar ile ilişkili tüm verileri depolar.

Kiracı Yöneticiler vererek bilgi isteklerini karşılamak için kullanabilir Azure portalından veya hizmetin REST API'leri veya verileri silme bir cihaz kimliği veya kayıt kimliği ile ilişkili

> [!NOTE]
> Azure IOT hub cihaz sağlama hizmeti aracılığıyla sağlanan cihazları Azure IOT Hub hizmetinde depolanan ek veriler var. Bkz: [Azure IOT Hub başvuru belgeleri](../iot-hub/iot-hub-customer-data-requests.md) belirli bir aygıt için tam bir isteği tamamlamak için.

## <a name="deleting-customer-data"></a>Müşteri verileri silme

Cihaz sağlama hizmeti kayıtlarını ve kayıt kayıtları depolar. Kayıtları sağlanacak izin cihazlar ve aygıtları üzerinden sağlama işlemine zaten ilerlemiş kayıt kayıtlarını Göster hakkındaki bilgileri içerir.

Kiracı Yöneticiler kayıtları Azure portalından kaldırabilir ve bu ilişkili kayıt kayıtları de kaldırır.

Daha fazla bilgi için bkz: [cihaz kayıtlarını yönetme](how-to-manage-enrollments.md).

Kayıtları ve REST API'lerini kullanarak kayıt kayıtları için silme işlemlerini gerçekleştirmek mümkündür:

* Tek bir cihaz kayıt bilgilerini silmek için kullanabileceğiniz [cihaz kaydı - Delete](https://docs.microsoft.com/rest/api/iot-dps/deviceenrollment/delete).
* Aygıt grubu kayıt bilgilerini silmek için kullanabileceğiniz [cihaz kayıt grubu - Delete](https://docs.microsoft.com/rest/api/iot-dps/deviceenrollmentgroup/delete).
* Sağlanan aygıtlar hakkındaki bilgileri silmek için kullanabileceğiniz [kayıt durumu - Delete kayıt durumu](https://docs.microsoft.com/rest/api/iot-dps/registrationstate/deleteregistrationstate).

## <a name="exporting-customer-data"></a>Müşteri verileri dışarı aktarma

Cihaz sağlama hizmeti kayıtlarını ve kayıt kayıtları depolar. Kayıtları sağlanacak izin cihazlar ve aygıtları üzerinden sağlama işlemine zaten ilerlemiş kayıt kayıtlarını Göster hakkındaki bilgileri içerir.

Kiracı Yöneticiler kayıtları ve Azure portalı üzerinden kayıt kayıtları görüntülemek ve bunları dışarı aktarmak kopyalama ve yapıştırmayı kullanma.

Kayıtları yönetme hakkında daha fazla bilgi için bkz: [cihaz kayıtlarını yönetme](how-to-manage-enrollments.md).

Kayıtları ve REST API'lerini kullanarak kayıt kayıtları için verme işlemlerini gerçekleştirmek mümkündür:

* Tek bir cihaz için kayıt bilgilerini dışarı aktarmak için kullanabileceğiniz [cihaz kaydı - Get](https://docs.microsoft.com/rest/api/iot-dps/deviceenrollment/get).
* Aygıtları grubu için kayıt bilgilerini dışarı aktarmak için kullanabileceğiniz [cihaz kayıt grubu - Get](https://docs.microsoft.com/rest/api/iot-dps/deviceenrollmentgroup/get).
* Önceden hazırlanmış cihazlar hakkında bilgi vermek için kullanabileceğiniz [kayıt durumu - Get kayıt durumu](https://docs.microsoft.com/rest/api/iot-dps/registrationstate/getregistrationstate).

> [!NOTE]
> Microsoft'un enterprise hizmetlerini kullandığınızda, sistem tarafından oluşturulan günlükleri bilinen bazı bilgiler, Microsoft oluşturur. Sistem tarafından oluşturulan bazı aygıt sağlama hizmeti günlükleri erişilebilir veya Kiracı Yöneticiler tarafından verilebilir değil. Bu günlükler hizmet ve tek cihazlara ilgili Tanılama verileri içinde gerçekleştirilen factual Eylemler oluşturur.

## <a name="links-to-additional-documentation"></a>Ek belgelere bağlantıları

Cihaz sağlama hizmet API'leri için tam belgelerine adresindedir [ https://docs.microsoft.com/rest/api/iot-dps ](https://docs.microsoft.com/rest/api/iot-dps).

Azure IOT Hub [istek özellikleri müşteri verilerini](../iot-hub/iot-hub-customer-data-requests.md).