---
title: Azure yönetilen uygulamalar görünüm tanımında genel bakış | Microsoft Docs
description: Azure yönetilen uygulamalar için Görünüm tanımı oluşturma kavramını açıklar.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.author: lazinnat
author: lazinnat
ms.date: 06/12/2019
ms.openlocfilehash: 6735787f9b43f98ab611584f3c7191c9f927dbc2
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67478751"
---
# <a name="view-definition-artifact-in-azure-managed-applications"></a>Azure yönetilen uygulamalar, görünüm tanımı yapıt

Görünüm tanımı, Azure yönetilen uygulamalar, isteğe bağlı bir yapıdır. Genel Bakış sayfasını özelleştirme ve ölçümler ve özel kaynakları gibi diğer görünümler sağlar.

Bu makalede, görünüm tanımı yapıt genel bir bakış ve özelliklerini sağlar.

## <a name="view-definition-artifact"></a>Tanım yapıtını görüntüleme

Görünüm tanımı yapıt olarak adlandırılmalıdır **viewDefinition.json** ve aynı düzeyde yerleştirilmiş **createUiDefinition.json** ve **mainTemplate.json** .zip olarak Bu paketi bir yönetilen uygulama tanımı oluşturur. .Zip paketi oluşturmak ve yönetilen uygulama tanımı yayımlama hakkında bilgi edinmek için [Azure yönetilen uygulama tanımı yayımlama](publish-managed-app-definition-quickstart.md)

## <a name="view-definition-schema"></a>Görünüm tanımı şema

**ViewDefinition.json** dosya sahip yalnızca bir üst düzey `views` görünümleri dizisidir özelliği. Her görünüm, ayrı bir menü öğesi olarak içindekiler tablosunda yönetilen uygulama kullanıcı arabiriminde gösterilir. Her görünüm sahip bir `kind` görünüm türünü ayarlar özelliği. Aşağıdaki değerlerden birine ayarlanmalıdır: [Genel Bakış](#overview), [ölçümleri](#metrics), [CustomResources](#custom-resources). Daha fazla bilgi için bkz: geçerli [viewDefinition.json için JSON şeması](https://schema.management.azure.com/schemas/viewdefinition/0.0.1-preview/ViewDefinition.json#).

Görünüm tanımı için JSON örneği:

```json
{
    "views": [
        {
            "kind": "Overview",
            "properties": {
                "header": "Welcome to your Azure Managed Application",
                "description": "This managed application is for demo purposes only.",
                "commands": [
                    {
                        "displayName": "Test Action",
                        "path": "testAction"
                    }
                ]
            }
        },
        {
            "kind": "Metrics",
            "properties": {
                "displayName": "This is my metrics view",
                "version": "1.0.0",
                "charts": [
                    {
                        "displayName": "Sample chart",
                        "chartType": "Bar",
                        "metrics": [
                            {
                                "name": "Availability",
                                "aggregationType": "avg",
                                "resourceTagFilter": [ "tag1" ],
                                "resourceType": "Microsoft.Storage/storageAccounts",
                                "namespace": "Microsoft.Storage/storageAccounts"
                            }
                        ]
                    }
                ]
            }
        },
        {
            "kind": "CustomResources",
            "properties": {
                "displayName": "Test custom resource type",
                "version": "1.0.0",
                "resourceType": "testCustomResource",
                "createUIDefinition": { },
                "commands": [
                    {
                        "displayName": "Custom Test Action",
                        "path": "testAction"
                    },
                    {
                        "displayName": "Custom Context Action",
                        "path": "testCustomResource/testContextAction",
                        "icon": "Stop",
                        "createUIDefinition": { },
                    }
                ],
                "columns": [
                    {"key": "name", "displayName": "Name"},
                    {"key": "properties.myProperty1", "displayName": "Property 1"},
                    {"key": "properties.myProperty2", "displayName": "Property 2", "optional": true}
                ]
            }
        }
    ]
}

```

## <a name="overview"></a>Genel Bakış

`"kind": "Overview"`

Bu görünümde, sağladığınız **viewDefinition.json**, yönetilen uygulamanız varsayılan genel bakış sayfasında onu geçersiz kılar.

```json
{
    "kind": "Overview",
    "properties": {
        "header": "Welcome to your Azure Managed Application",
        "description": "This managed application is for demo purposes only.",
        "commands": [
            {
                "displayName": "Test Action",
                "path": "testAction"
            }
        ]
    }
}
```

|Özellik|Gerekli|Açıklama|
|---------|---------|---------|
|üst bilgi|Hayır|Genel Bakış sayfasının başlığı.|
|description|Hayır|Yönetilen uygulama tanımı.|
|Komutları|Hayır|Bkz: genel bakış sayfasının ek düğmeler dizisi [komutları](#commands).|

## <a name="metrics"></a>Ölçümler

`"kind": "Metrics"`

Ölçümler görünümü, toplamak ve yönetilen uygulama kaynaklarınızdan gelen verileri birleştiren sağlar [Azure İzleyici ölçümleri](../azure-monitor/platform/data-platform-metrics.md).

```json
{
    "kind": "Metrics",
    "properties": {
        "displayName": "This is my metrics view",
        "version": "1.0.0",
        "charts": [
            {
                "displayName": "Sample chart",
                "chartType": "Bar",
                "metrics": [
                    {
                        "name": "Availability",
                        "aggregationType": "avg",
                        "resourceTagFilter": [ "tag1" ],
                        "resourceType": "Microsoft.Storage/storageAccounts",
                        "namespace": "Microsoft.Storage/storageAccounts"
                    }
                ]
            }
        ]
    }
}
```

|Özellik|Gerekli|Açıklama|
|---------|---------|---------|
|displayName|Hayır|Görünüm görüntülenen başlığı.|
|version|Hayır|Görünümü işlemek için kullanılan platform sürümü.|
|Grafikler|Evet|Grafik ölçümlerini sayfanın dizisi.|

### <a name="chart"></a>Grafik

|Özellik|Gerekli|Açıklama|
|---------|---------|---------|
|displayName|Evet|Grafikte görüntülenen başlığı.|
|chartType|Hayır|Bu grafik için kullanılacak görselleştirmeyi. Varsayılan olarak, bir çizgi grafik kullanır. Grafik türleri desteklenir: `Bar, Line, Area, Scatter`.|
|metrics|Evet|Bu grafiği çizmek için ölçümleri dizisi. Azure Portalı'nda desteklenen ölçümler hakkında daha fazla bilgi edinmek için [Azure İzleyici ile desteklenen ölçümler](../azure-monitor/platform/metrics-supported.md)|

### <a name="metric"></a>Ölçüm

|Özellik|Gerekli|Açıklama|
|---------|---------|---------|
|name|Evet|Ölçüm adı.|
|aggregationType|Evet|Bu ölçüm için kullanılacak toplama türü. Toplama türleri desteklenir: `none, sum, min, max, avg, unique, percentile, count`|
|ad alanı|Hayır|Doğru ölçümleri sağlayıcısı belirlenirken kullanılacak ek bilgiler.|
|resourceTagFilter|Hayır|Kaynak etiketler dizisi (ile ayrılmış `or` word) için hangi ölçümler görüntülenir. Kaynak Türü Filtresi üzerine uygulanır.|
|Kaynak türü|Evet|Ölçümleri görüntülenmekteydi kaynak türü.|

## <a name="custom-resources"></a>Özel kaynaklar

`"kind": "CustomResources"`

Bu türün birden çok görünüm tanımlayabilirsiniz. Her görünüm temsil eden bir **benzersiz** özel kaynak türü özel sağlayıcısından tanımladığınız **mainTemplate.json**. Özel sağlayıcılar için bir giriş için bkz [Azure özel sağlayıcıları Önizlemesi'ne genel bakış](custom-providers-overview.md).

Bu görünümde gerçekleştirebileceğiniz Al koy, Sil ve işlemleri için özel kaynak türünüzü yayınlayın. Gönderme işlemleri genel özel veya özel kaynak türden bir bağlamda özel eylemler olabilir.

```json
{
    "kind": "CustomResources",
    "properties": {
        "displayName": "Test custom resource type",
        "version": "1.0.0",
        "resourceType": "testCustomResource",
        "createUIDefinition": { },
        "commands": [
            {
                "displayName": "Custom Test Action",
                "path": "testAction"
            },
            {
                "displayName": "Custom Context Action",
                "path": "testCustomResource/testContextAction",
                "icon": "Stop",
                "createUIDefinition": { },
            }
        ],
        "columns": [
            {"key": "name", "displayName": "Name"},
            {"key": "properties.myProperty1", "displayName": "Property 1"},
            {"key": "properties.myProperty2", "displayName": "Property 2", "optional": true}
        ]
    }
}
```

|Özellik|Gerekli|Açıklama|
|---------|---------|---------|
|displayName|Evet|Görünüm görüntülenen başlığı. Başlığı olmalıdır **benzersiz** her CustomResources görünümünde, **viewDefinition.json**.|
|version|Hayır|Görünümü işlemek için kullanılan platform sürümü.|
|Kaynak türü|Evet|Özel kaynak türü. Olmalıdır bir **benzersiz** özel sağlayıcınızın özel kaynak türü.|
|createUIDefinition|Hayır|UI tanımı oluşturmak için şema özel kaynak komutu oluşturun. UI tanımları oluşturma, bir giriş için bkz. [createuidefinition dosyasını kullanmaya başlama](create-uidefinition-overview.md)|
|Komutları|Hayır|Ek araç çubuğu düğmeleri CustomResources görünümün dizisi bkz [komutları](#commands).|
|Sütunları|Hayır|Özel kaynak sütun dizisi. Tanımlı değilse `name` sütun varsayılan olarak gösterilir. Sütun olmalıdır `"key"` ve `"displayName"`. Anahtar için bir görünümde görüntülenecek özelliğin anahtarını belirtin. İç içe geçmiş, nokta, sınırlayıcı olarak örneğin kullanın `"key": "name"` veya `"key": "properties.property1"`. Görüntü adı için bir görünümde görüntülenecek özelliği görünen adını sağlayın. De sağlayabilirsiniz bir `"optional"` özelliği. Ne zaman true olarak sütun görünümü'nde varsayılan olarak gizlidir.|

## <a name="commands"></a>Komutlar

Bir dizi sayfasında görüntülenen ek araç çubuğu düğmeleri komuttur. Azure özel tanımlı sağlayıcınız POST eylemden her komutu temsil eder **mainTemplate.json**. Özel sağlayıcılar için bir giriş için bkz [Azure özel sağlayıcılara genel bakış](custom-providers-overview.md).

```json
{
    "commands": [
        {
            "displayName": "Start Test Action",
            "path": "testAction",
            "icon": "Start",
            "createUIDefinition": { }
        },
    ]
}
```

|Özellik|Gerekli|Açıklama|
|---------|---------|---------|
|displayName|Evet|Komut düğmesi görüntülenen adı.|
|yol|Evet|Özel sağlayıcı eylem adı. Eylem tanımlanmalıdır **mainTemplate.json**.|
|Simgesi|Hayır|Komut düğmesi simge. Desteklenen simge listesi tanımlanmış [JSON şeması](https://schema.management.azure.com/schemas/viewdefinition/0.0.1-preview/ViewDefinition.json#).|
|createUIDefinition|Hayır|Komut için kullanıcı Arabirimi Tanım Şeması oluşturun. UI tanımları oluşturma, bir giriş için bkz. [createuidefinition dosyasını kullanmaya başlama](create-uidefinition-overview.md).|

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen uygulamalara giriş için [Azure Yönetilen Uygulamalara genel bakış](overview.md) konusunu inceleyin.
- Özel sağlayıcılar için bir giriş için bkz [Azure özel sağlayıcılara genel bakış](custom-providers-overview.md).
