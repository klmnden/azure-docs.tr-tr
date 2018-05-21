---
title: Müşteri verileri istek özellikleri
author: dominicbetts
ms.author: dobett
ms.date: 05/16/2018
ms.topic: conceptual
ms.service: iot-hub
ms.openlocfilehash: 3af76fe22e93b6c5d502733196994bda61b9a93e
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="summary-of-customer-data-request-features"></a>Müşteri verileri isteği özelliklerinin özeti

Azure IOT Hub, milyonlarca cihaza bölümlenmiş bir Azure hizmeti arasında güvenli, çift yönlü iletişimi sağlayan kurumsal müşteriler hedeflenen bir REST API tabanlı bulut hizmetidir.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

Tek tek cihazlar cihaz tanımlayıcısı (cihaz kimliği) bir kiracı Yöneticisi tarafından atanır. Aygıt verilerini atanan aygıtın kimliğini temel alır. Microsoft hiçbir bilgi tutar ve kullanıcı bağıntı cihaz Kimliğine izin verilere erişim yok.

Çoğu Azure IOT Hub ' yönetilen aygıtlar örneğin office thermostat veya Fabrika robot kişisel aygıtlar değildir. Müşteriler, ancak, bazı cihazların kişisel olmasını düşünebilirsiniz ve kendi istediğiniz kadar kendi varlık veya izleme kişiler cihazlara tie yöntemlerini stok korumak. Azure IOT hub'ı yönetir ve kişisel veriler gibi cihazlar ile ilişkili tüm verileri depolar.

Kiracı Yöneticiler dışarı aktarma veya bir cihaz kimliği ile ilişkili verileri silerek bilgi isteklerini karşılamak için kullanabilir Azure portalından veya hizmetin REST API'leri

Diğer hizmetler için cihaz iletilerini iletmek için Azure IOT Hub hizmetinin yönlendirme özelliğini kullanırsanız, ardından veri istekleri her yönlendirme uç noktası için Kiracı yönetici tarafından belirli bir aygıt için tam bir isteği tamamlamak için gerçekleştirilmesi gerekir. Daha ayrıntılı bilgi için her uç noktanın başvuru belgelerine bakın. Desteklenen uç noktaları hakkında daha fazla bilgi için bkz: [başvuru - IOT Hub uç noktaları](iot-hub-devguide-endpoints.md).

Azure IOT Hub hizmeti Azure olay kılavuz tümleştirme özelliğini kullanın, ardından veri istekleri bu olayların her abone için Kiracı yönetici tarafından gerçekleştirilmelidir. Daha fazla bilgi için bkz: [olay kılavuz kullanarak IOT hub'ı olaylarına tepki](iot-hub-event-grid.md).

Tanılama günlükleri oluşturmak için Azure IOT Hub hizmeti Azure İzleyici tümleştirme özelliğini kullanırsanız, veri istekleri saklı günlüklere karşı Kiracı yönetici tarafından gerçekleştirilmelidir. Daha fazla bilgi için bkz: [Azure IOT Hub sağlığını izlemek](iot-hub-monitor-resource-health.md).

## <a name="deleting-customer-data"></a>Müşteri verileri silme

Kiracı Yöneticiler IOT cihazları dikey penceresi Azure IOT Hub uzantısı'nın bu aygıtla ilişkili verileri siler bir cihazı silmek için Azure portalında kullanın.

REST API'lerini kullanarak aygıtlar için silme işlemlerini gerçekleştirmek mümkündür. Daha fazla bilgi için bkz: [aygıt API - cihaz silme](https://docs.microsoft.com/rest/api/iothub/deviceapi/deletedevice).

## <a name="exporting-customer-data"></a>Müşteri verileri dışarı aktarma

Kiracı Yöneticiler, kopya kullanan ve bir aygıtla ilişkili verileri dışarı aktarmak için Azure portalında Azure IOT Hub uzantısı IOT cihazları dikey yapıştırın.

REST API'lerini kullanarak cihazlar için verme işlemlerini gerçekleştirmek mümkündür. Daha fazla bilgi için bkz: [aygıt API - Al aygıt](https://docs.microsoft.com/rest/api/iothub/deviceapi/getdevice).

> [!NOTE]
> Microsoft'un enterprise hizmetlerini kullandığınızda, sistem tarafından oluşturulan günlükleri bilinen bazı bilgiler, Microsoft oluşturur. Sistem tarafından oluşturulan bazı Azure IOT Hub günlükleri erişilebilir veya Kiracı Yöneticiler tarafından verilebilir değil. Bu günlükler hizmet ve tek cihazlara ilgili Tanılama verileri içinde gerçekleştirilen factual Eylemler oluşturur.

## <a name="links-to-additional-documentation"></a>Ek belgelere bağlantıları

Azure IOT Hub cihaz API'leri için tam belgelerine adresindedir [ https://docs.microsoft.com/rest/api/iothub/deviceapi ](https://docs.microsoft.com/rest/api/iothub/deviceapi).