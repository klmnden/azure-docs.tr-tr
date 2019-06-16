---
title: Azure dijital İkizlerini ortak sorgu kalıpları | Microsoft Docs
description: Azure dijital İkizlerini yönetim API'leri sorgulama ortak desenler öğrenin.
author: dsk-2015
manager: philmea
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 1/7/2019
ms.author: dkshir
ms.openlocfilehash: ff8638042fa10c939ff9c5fa7af99a660fcdc753
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60924745"
---
# <a name="how-to-query-azure-digital-twins-apis-for-common-tasks"></a>Azure dijital İkizlerini API'leri için ortak görevler sorgulama

Bu makalede, yaygın senaryoları Azure dijital İkizlerini örneğinizin yürütmesine yardımcı olacak sorgu desenleri gösterir. Bu dijital İkizlerini örneğinizin çalışmakta olduğunu varsayar. Postman gibi herhangi bir REST istemcisi kullanabilirsiniz. 

[!INCLUDE [digital-twins-management-api](../../includes/digital-twins-management-api.md)]


## <a name="queries-for-spaces-and-types"></a>Sorgu alanları ve türler

Bu bölümde, sağlanan alanları hakkında daha fazla bilgi almak için örnek sorgular gösterilmiştir. Yer tutucuları kurulumunuzu değerlerle değiştirerek örnek sorguları kimliği doğrulanmış GET HTTP istekleriyle olun. 

- Kök düğümleri olan alanları alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/spaces?$filter=ParentSpaceId eq null
    ```

- Ada göre bir alan elde etme ve cihazlar, algılayıcılar, hesaplanan değerler ve algılayıcı değerlerini içerir. 

    ```plaintext
    YOUR_MANAGEMENT_API_URL/spaces?name=Focus Room A1&includes=fullpath,devices,sensors,values,sensorsvalues
    ```

- Alanları ve ana verilen alanı kimliği ve iki ila beş düzey olan cihaz/sensör bilgilerini yeniden [belirli alan göreli](how-to-navigate-apis.md#api-navigation). 

    ```plaintext
    YOUR_MANAGEMENT_API_URL/spaces?spaceId=YOUR_SPACE_ID&includes=fullpath,devices,sensors,values,sensorsvalues&traverse=Down&minLevel=1&minRelative=true&maxLevel=5&maxRelative=true
    ```

- Belirtilen Kimliğe sahip bir alan elde etme ve dahil hesaplanan ve algılayıcı değerlerini.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/spaces?ids=YOUR_SPACE_ID&includes=Values,sensors,SensorsValues
    ```

- Belirli bir alan özelliği anahtarları alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/propertykeys?spaceId=YOUR_SPACE_ID
    ```

- Adlı özelliği anahtarla alanları alma *AreaInSqMeters* ve değerini 30'dur. Ayrıca operations dize, örneğin, get içeren özellik anahtarı ile alanları `name = X contains Y`.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/spaces?propertyKey=AreaInSqMeters&propertyValue=30
    ```

- Ada sahip tüm adları almak *sıcaklık* ve ilişkili bağımlılıkları ve ontolojileri.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/types?names=Temperature&includes=space,ontologies,description,fullpath
    ```


## <a name="queries-for-roles-and-role-assignments"></a>Rolleri ve rol atamaları için sorgular

Bu bölümde, roller ve onların atamaları hakkında daha fazla bilgi almak için bazı sorgular gösterilmektedir. 

- Azure dijital İkizlerini tarafından desteklenen tüm rolleri alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/system/roles
    ```

- Tüm rol atamalarını dijital İkizlerini Örneğinizde alın. 

    ```plaintext
    YOUR_MANAGEMENT_API_URL/roleassignments?path=/&traverse=down
    ```

- Rol ataması, belirli bir yolda alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/roleassignments?path=/A_SPATIAL_PATH
    ```

## <a name="queries-for-devices"></a>Cihazlar için sorgular

Bu bölümde, cihazlarınızı hakkında ayrıntılı bilgi almak için yönetim API'leri nasıl kullanabileceğiniz bazı örnekler gösterilmektedir. Tüm API çağrıları, kimliği doğrulanmış GET HTTP isteklerini gerekecektir.

- Tüm cihazlar alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices
    ```

- Tüm cihaz durumları bulun.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/system/devices/statuses
    ```

- Belirli bir aygıt alma.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices/YOUR_DEVICE_ID
    ```

- Kök alanınıza bağlı tüm aygıt alma.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?maxLevel=1
    ```

- 2 ile 4 arasındaki düzeylerinde alanlarına bağlı tüm aygıt alma.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?minLevel=2&maxLevel=4
    ```

- Tüm cihazlar için bir özel alan kimliği doğrudan bağlı Al

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID
    ```

- Belirli bir alanı ve alt öğelerini bağlı olan tüm cihazları alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID&traverse=Down
    ```

- Bir alanı alt öğeleri bu alanı hariç bağlı tüm aygıt alma.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID&traverse=Down&minLevel=1&minRelative=true
    ```

- Bir alanı doğrudan alt öğelere bağlı tüm aygıt alma.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID&traverse=Down&minLevel=1&minRelative=true&maxLevel=1&maxRelative=true
    ```

- Bir alanı öğelerinden biri için eklenen tüm cihazları alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID&traverse=Up&maxLevel=-1&maxRelative=true
    ```

- Düzeyi değerinden küçük veya 5'e eşit olan alt öğeleri bir alanı eklenmiş olan tüm cihazları alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID&traverse=Down&maxLevel=5
    ```

- Alanı kimliği ile aynı düzeyde olan alanlarına bağlı tüm aygıt alma *YOUR_SPACE_ID*.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID&traverse=Span&minLevel=0&minRelative=true&maxLevel=0&maxRelative=true
    ```

- Cihazınız için IOT Hub cihaz bağlantı dizesini alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices/YOUR_DEVICE_ID?includes=ConnectionString
    ```

- Ekli algılayıcılar dahil belirli donanım Kimliğine sahip cihazı alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?hardwareIds=YOUR_DEVICE_HARDWARE_ID&includes=sensors
    ```

- Algılayıcılar belirli veri türleri için bu durumda alma *hareket* ve *sıcaklık*.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/sensors?dataTypes=Motion,Temperature
    ```

## <a name="queries-for-matchers-and-user-defined-functions"></a>Sorgular için matchers ve kullanıcı tanımlı işlevler 

- Tüm sağlanan matchers ve kimliklerini alın.

   ```plaintext
    YOUR_MANAGEMENT_API_URL/matchers
    ```

- Alanları ve onunla ilişkili kullanıcı tanımlı işlev de dahil olmak üzere, belirli bir Eşleştiricisi hakkındaki ayrıntıları alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/matchers/YOUR_MATCHER_ID?includes=description, conditions, fullpath, userdefinedfunctions, space
    ```

- Bir algılayıcı karşı bir Eşleştiricisi değerlendirin ve hata ayıklama amacıyla günlük kaydını etkinleştir. Bu HTTP GET iletinin dönüş Eşleştiricisi ve algılayıcı veri türüne ait olup olmadığını söyler. 

   ```plaintext
    YOUR_MANAGEMENT_API_URL/matchers/YOUR_MATCHER_ID/evaluate/YOUR_SENSOR_ID?enableLogging=true
    ```

- Kullanıcı tanımlı işlevleri Kimliğini alın. 

   ```plaintext
    YOUR_MANAGEMENT_API_URL/userdefinedfunctions
    ```

- Belirli bir kullanıcı tanımlı işlev içeriğini Al 

   ```plaintext
    YOUR_MANAGEMENT_API_URL/userdefinedfunctions/YOUR_USER_DEFINED_FUNCTION_ID/contents
    ```


## <a name="queries-for-users"></a>Kullanıcılar için sorgular

Bu bölümde Azure dijital İkizlerini kullanıcıları yönetmek için bazı örnek API sorgular gösterilmektedir. Yer tutucuları kurulumunuzu değerlerle değiştirerek bir HTTP GET isteği oluşturun. 

- Tüm kullanıcılar alır. 

    ```plaintext
    YOUR_MANAGEMENT_API_URL/users
    ```

- Belirli bir kullanıcı alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/users/ANY_USER_ID
    ```

## <a name="next-steps"></a>Sonraki adımlar

Yönetim API'si ile kimlik doğrulaması yapmayı öğrenmek için [API'leri ile kimlik doğrulaması](./security-authenticating-apis.md).

Tüm API uç noktalarını görmek için okuma [dijital Swagger İkizlerini kullanma](./how-to-use-swagger.md).
