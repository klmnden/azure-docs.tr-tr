---
title: Tetikleyiciler ve bağlamalar Azure işlevleri'nde
description: Azure işlevinizi çevrimiçi etkinliklerimizden ve bulut tabanlı hizmetlere bağlanmak için Tetikleyicileri ve bağlamaları kullanmayı öğrenin.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 02/18/2019
ms.author: cshoe
ms.openlocfilehash: 3865f748a9ca2fe09660d6454542d64f73a8e3c1
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "56736966"
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Azure işlevleri Tetikleyicileri ve bağlamaları kavramları

Bu makalede işlevleri Tetikleyicileri ve bağlamaları çevreleyen üst düzey kavramlarını öğrenin.

Tetikleyiciler, çalıştırılacak bir işlev nedeni ne ' dir. Bir tetikleyici bir işlevin nasıl çağrılır tanımlar ve bir işlev tam olarak bir tetikleyici olmalıdır. Tetikleyiciler genellikle işlev yükü olarak sağlanan veri ilişkilendirdiniz. 

Bir işlev bağlaması başka bir kaynak için işlev bildirimli olarak bağlanan bir yoludur; bağlamaları bağlanabilir olarak *giriş*, *çıktı bağlaması*, veya her ikisini de. Veri bağlamaları işlevi için parametre olarak sağlanır.

Karışık ve ihtiyaçlarınıza uyacak şekilde farklı bağlamaları eşleşmesi. Bağlamaları isteğe bağlıdır ve bir işlevi ve/veya bir veya birden çok girişi olan çıktı bağlaması.

Tetikleyiciler ve bağlamalar runbook'a kod diğer hizmetlere erişimi engellemenize olanak tanır. İşlevinizi verileri (örneğin, bir kuyruk iletisinin içeriği) işlevi parametreleri alır. Verileri (örneğin, bir kuyruk iletisi oluşturmak için), işlev dönüş değeri kullanarak gönderin. 

Farklı işlevleri nasıl uygulayabileceğine ilişkin aşağıdaki örneklerde göz önünde bulundurun.

| Örnek senaryo | Tetikleyici | Giriş bağlama | Çıktı bağlaması |
|-------------|---------|---------------|----------------|
| Yeni bir kuyruk iletisi, başka bir kuyruğa yazmak için bir işlev çalıştığı ulaşır. | Kuyruk<sup>*</sup> | *None* | Kuyruk<sup>*</sup> |
|Zamanlanmış bir iş, Blob Depolama içerikleri okur ve yeni bir Cosmos DB belgesini oluşturur. | Zamanlayıcı | Blob Depolama | Cosmos DB |
|Event Grid, Blob Depolama ve Cosmos DB belge e-posta göndermek için bir görüntü okumak için kullanılır. | Event Grid | BLOB Depolama ve Cosmos DB | SendGrid |
| Bir Excel sayfası güncelleştirmek için Microsoft Graph'ı kullanan bir Web kancası. | HTTP | *None* | Microsoft Graph |

<sup>\*</sup> Farklı kuyruklara temsil eder

Bu örneklerin ayrıntılı olması beklenmez, ancak nasıl Tetikleyicileri ve bağlamaları birlikte kullanabileceğiniz göstermek için sağlanır.

###  <a name="trigger-and-binding-definitions"></a>Tetikleyici ve bağlama tanımları

Tetikleyiciler ve bağlamalar geliştirme yaklaşımını bağlı olarak farklı şekilde tanımlanır.

| Platform | Tetikleyiciler ve bağlamalar tarafından yapılandırılan... |
|-------------|--------------------------------------------|
| C#sınıf kitaplığı | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;yöntemleri ve parametrelerle dekorasyon C# öznitelikleri |
| Diğerlerinin tümü (Azure portalı dahil) | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Güncelleştirme [function.json](./functions-reference.md) ([şema](http://json.schemastore.org/function)) |

Portal, bu yapılandırma için bir kullanıcı Arabirimi sağlar, ancak doğrudan açarak dosyayı düzenleyebilirsiniz **Gelişmiş Düzenleyici** aracılığıyla kullanılabilen **tümleştir** işlevinizin sekmesi.

. NET'te, parametre türü, giriş verileri için veri türünü tanımlar. Örneğin, kullanın `string` ikili ve bir nesneyi seri durumdan çıkarılamıyor için özel bir tür olarak okunacak bayt dizisi olarak bir kuyruk tetikleyicisi metnini bağlanacak.

JavaScript gibi dinamik olarak yazılan diller için kullanmak `dataType` özelliğinde *function.json* dosya. Örneğin, ikili biçimde bir HTTP isteği içeriğini okumak için ayarlanmış `dataType` için `binary`:

```json
{
    "dataType": "binary",
    "type": "httpTrigger",
    "name": "req",
    "direction": "in"
}
```

Diğer seçenekler için `dataType` olan `stream` ve `string`.

## <a name="binding-direction"></a>Bağlama yönü

Tüm tetikleyiciler ve bağlamalar bir `direction` özelliğinde [function.json](./functions-reference.md) dosyası:

- Tetikleyiciler için her zaman yönü olur `in`
- Giriş ve çıkış bağlamaları kullanın `in` ve `out`
- Bazı bağlamalar destekleyen özel bir yön `inout`. Kullanırsanız `inout`, yalnızca **Gelişmiş Düzenleyici** aracılığıyla kullanılabilir **tümleştir** portalında sekmesi.

Kullanırken [öznitelikleri bir sınıf kitaplığı'nda](functions-dotnet-class-library.md) Tetikleyicileri ve bağlamaları yapılandırmak için yön bir öznitelik oluşturucuda sağlanan veya parametre türünden çıkarsanan.

## <a name="supported-bindings"></a>Desteklenen bağlamaları

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

Bağlamaları önizlemededir veya üretim kullanımı için onaylanmış olan hakkında bilgi için bkz: [desteklenen diller](supported-languages.md).

## <a name="resources"></a>Kaynaklar
- [Bağlama ifadeleri ve desenleri](./functions-bindings-expressions-patterns.md)
- [Azure işlev dönüş değeri kullanma](./functions-bindings-return-value.md)
- [Bağlama ifadesi kaydetme](./functions-bindings-register.md)
- Test:
  - [Kodunuzu Azure işlevleri'nde test stratejileri](functions-test-a-function.md)
  - [HTTP ile tetiklenmeyen bir işlevi el ile çalıştırma](functions-manually-run-non-http.md)
- [Bağlama hataları işleme](./functions-bindings-errors.md)

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Azure işlevleri bağlama uzantılarını kaydetme](./functions-bindings-register.md)
