---
title: "Özel bağlayıcıların - Azure Logic Apps OpenAPI uzantıları | Microsoft Docs"
description: "Gelişmiş işlevselliği ile özel bağlayıcı OpenAPI genişletme"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: e8565019bd4631043485224c73c2fe60e633951c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="extend-your-custom-connectors-openapi-with-advanced-functionality"></a>Gelişmiş işlevselliği ile özel bağlayıcı OpenAPI genişletme

Azure mantıksal uygulamaları, Microsoft Flow ya da Microsoft PowerApps için özel Bağlayıcıyı oluşturmak için bir API'nin işlemlerini ve parametrelerini açıklayan bir dilden bağımsız makine tarafından okunabilir belge bir OpenAPI tanım dosyası sağlamanız gerekir. OpenAPI'ın Giden kutusu işlevselliği ile birlikte özel bağlayıcılar mantıksal uygulamalar ve akış oluşturduğunuzda bu OpenAPI uzantıları de içerebilir:

* [`summary`](#summary)
* [`x-ms-summary`](#x-ms-summary)
* [`description`](#description)
* [`x-ms-visibility`](#x-ms-visibility)
* [`x-ms-dynamic-values`](#x-ms-dynamic-values)
* [`x-ms-dynamic-schema`](#x-ms-dynamic-schema)

Bu uzantılar hakkında daha fazla ayrıntı aşağıdadır:

<a name="summary"></a>

## <a name="summary"></a>Özet

(İşlem) eylemi için başlığı belirtir. </br>
Uygulandığı öğe: işlemleri </br>
Önerilen: Kullanın *tümce* için `summary`. </br>
Örnek: "bir olay eklendiğinde takvime" veya "e-posta Gönder"

![Her işlem için "Özet"](./media/custom-connector-openapi-extensions/summary.png)

``` json
"actions" {
  "Send_an_email": {
    /// Other action properties here...
    "summary": "Send an email",
    /// Other action properties here...
  }
},
```

<a name="x-ms-summary"></a>

## <a name="x-ms-summary"></a>x-ms-Özet

Bir varlık için başlık belirtir. </br>
Uygulandığı öğe: parametreler, yanıt şeması </br>
Önerilen: Kullanın *başlık durumda* için `x-ms-summary`. </br>
Örnek: "Takvim kimliği", "Konu", "Olay açıklaması" vb.

!["x-ms-Özeti" her varlık](./media/custom-connector-openapi-extensions/x-ms-summary.png)

``` json
"actions" {
  "Send_an_email": {
    /// Other action properties here...
    "parameters": [ 
      {
        /// Other parameters here...
        "x-ms-summary": "Subject",
        /// Other parameters here...
      }
    ]
  }
},
```

<a name="description"></a>

## <a name="description"></a>açıklama

İşlemin işlevselliği veya bir varlığın biçimi ve işlevi hakkında ayrıntılı bir açıklama belirtir. </br>
Uygulandığı öğe: işlem, parametreleri, yanıt şeması </br>
Önerilen: Kullanın *tümce* için `description`. </br>
Örnek: "yeni bir olay takvime eklendiğinde bu işlemi tetikleyen", "postanın konusunu belirtin", vb.

![Her işlem ya da varlık için "Açıklama"](./media/custom-connector-openapi-extensions/description.png)

``` json
"actions" {
  "Send_an_email": {
     "description": "Specify the subject of the mail",
     /// Other action properties here...
  }
},
```

<a name="visibility"></a>

## <a name="x-ms-visibility"></a>x-ms-görünürlüğü

Bir varlık için kullanıcı dönük görünürlüğünü belirtir. </br>
Olası değerler: `important`, `advanced`, ve`internal` </br>
Uygulandığı öğe: işlem, parametreleri, şemaları

* `important`her zaman işlemlerini ve parametrelerini kullanıcıya ilk gösterilir.
* `advanced`işlemler ve parametreleri bir ek menüsünün altında gizlenir.
* `internal`işlemler ve parametreleri kullanıcıdan gizlenir.

> [!NOTE] 
> Parametre için `internal` ve `required`, size **gerekir** Bu parametreler için varsayılan değerleri sağlayın.

Örnek: **daha fazla** menü ve **Gelişmiş Seçenekleri Göster** menü gizleme `advanced` işlemleri ve parametreleri.

!["x-ms-görünürlük" gösteren veya işlemleri ve parametreleri gizleme](./media/custom-connector-openapi-extensions/x-ms-visibility.png)

``` json
"actions" {
  "Send_an_email": {
     /// Other action properties here...
     "parameters": [
         {
           "name": "Subject",
           "type": "string",
           "description": "Specify the subject of the mail",
           "x-ms-summary": "Subject",
           "x-ms-visibility": "important",
           /// Other parameter properties here...
         }
     ]
     /// Other action properties here...
  }
},
```

<a name="x-ms-dynamic-values"></a>

## <a name="x-ms-dynamic-values"></a>x-ms-dinamik değerleri

Giriş parametreleri için bir işlem seçebilmeniz için kullanıcı doldurulan bir listesini gösterir. </br>
Uygulandığı öğe: parametreleri </br>
Nasıl kullanılacağını: eklemek `x-ms-dynamic-values` parametrenin tanımı nesnesi. Örneğin, bu bkz [OpenAPI örnek](https://procsi.blob.core.windows.net/blog-images/sampleDynamicSwagger.json).

!["x-ms-dinamik listeleri göstermek için değerleri"](./media/custom-connector-openapi-extensions/x-ms-dynamic-values.png)

### <a name="properties-for-x-ms-dynamic-values"></a>X-ms-dinamik değerleri için özellikleri

| Ad | Gerekli veya isteğe bağlı | Açıklama | 
| ---- | -------------------- | ----------- | 
| **Operationıd** | Gerekli | Listeyi doldurmak için aranacak işlemi | 
| **değer yolu** | Gerekli | Yol dizesi içinde nesnesindeki `value-collection` parametre değerine başvuruyor. </br>Varsa `value-collection` değil belirtilen yanıt bir dizi olarak değerlendirildi. | 
| **değer başlığı** | İsteğe bağlı | Yol dizesi içinde nesnesindeki `value-collection` değerinin açıklamasını gösterir. </br>Varsa `value-collection` değil belirtilen yanıt bir dizi olarak değerlendirildi. | 
| **değer koleksiyonu** | İsteğe bağlı | Yanıt yükte nesnelerinin bir dizisi değerlendiren bir yol dizesi | 
| **parametreleri** | İsteğe bağlı | Nesnenin özelliklerini dinamik değerleri işlemini çağırmak için gerekli giriş parametreleri belirtin | 
|||| 

İşte özellikleri gösteren bir örnek `x-ms-dynamic-values`:

``` json
"x-ms-dynamic-values": {
  "operationId": "PopulateDropdown",
  "value-path": "name",
  "value-title": "properties/displayName",
  "value-collection": "value",
  "parameters": {
     "staticParameter": "<value>",
     "dynamicParameter": {
        "parameter": "<value-to-pass-to-dynamicParameter>"
     }
  }
}
```

## <a name="example-all-the-openapi-extensions-up-to-this-point"></a>Örnek: Tüm OpenAPI uzantıları bu noktaya kadar

``` json
"/api/lists/{listID-dynamic}": {
    "get": {
        "description": "Get items from a single list - uses dynamic values and outputs dynamic schema",
        "summary": "Gets items from the selected list",
        "operationID": "GetListItems",
        "parameters": [
           {
             "name": "listID-dynamic",
             "type": "string",
             "in": "path",
             "description": "Select the list from where you want outputs",
             "required": true,
             "x-ms-summary": "Select List",
             "x-ms-dynamic-values": {
                "operationID": "GetLists",
                "value-path": "id",
                "value-title": "name"
             }
           }
        ]
    }
}
```

<a name="x-ms-dynamic schema"></a>

## <a name="x-ms-dynamic-schema"></a>x-ms-dinamik-şeması

Geçerli parametre veya yanıt için şemayı dinamik olduğunu gösterir. Bu nesne bu alanın değeri tarafından tanımlanan bir işlem çağırma, dinamik olarak şema bulmak ve kullanıcı girişleri toplamak için uygun UI görüntüleyebilir veya kullanılabilir alanlar göster. 

Uygulandığı öğe: parametreler, yanıt

Nasıl kullanılacağını: eklemek `x-ms-dynamic-schema` bir istek parametresi veya yanıt gövdesi tanımı nesnesi. Bu örnek için bkz [OpenAPI örnek](https://procsi.blob.core.windows.net/blog-images/sampleDynamicSwagger.json).

Bu örnek nasıl giriş formu değişiklikler, kullanıcının açılan listeden seçer öğesi göre gösterir:

!["x-ms-dinamik-schema" dinamik parametreleri](./media/custom-connector-openapi-extensions/x-ms-dynamic-schema.png)

Ve bu örnek nasıl çıkışları değiştirmek, kullanıcının açılan listeden seçer öğesi göre gösterir. Bu sürümde "Araba" kullanıcı seçer:

!["x-ms-dinamik şema-yanıt" Seçili öğe "Araba" için](./media/custom-connector-openapi-extensions/x-ms-dynamic-schema-output1.png)

Bu sürümde "Yemek" kullanıcı seçer:

!["x-ms-dinamik şema-yanıt" Seçili öğe "Yemek" için](./media/custom-connector-openapi-extensions/x-ms-dynamic-schema-output2.png)

### <a name="properties-for-x-ms-dynamic-schema"></a>X-ms-dinamik-schema özellikleri

| Ad | Gerekli veya isteğe bağlı | Açıklama | 
| ---- | -------------------- | ----------- | 
| **Operationıd** | Gerekli | Şema getirme çağırmak için işlemi | 
| **parametreleri** | Gerekli | Nesnenin özelliklerini dinamik şema işlemini çağırmak için gerekli giriş parametreleri belirtin | 
| **değer yolu** |İsteğe bağlı | Şema olan özelliğe başvuruyor yol dizesi. </br>Belirtilmezse, şema kök nesnesinin özellikleri içerecek şekilde yanıt varsayılır. | 
|||| 

Dinamik bir parametre için örnek aşağıda verilmiştir:

``` json
{
  "name": "dynamicListSchema",
  "in": "body",
  "description": "Dynamic schema for items in the selected list",
  "schema": {
    "type": "object",
    "x-ms-dynamic-schema": {
        "operationID": "GetListSchema",
        "parameters": {
          "listID": {
            "parameter": "listID-dynamic"
          }
        },
        "value-path": "items"
    }
  }
}
```

Dinamik yanıt örnek aşağıda verilmiştir:

``` json
"DynamicResponseGetListSchema": {
   "type": "object",
   "x-ms-dynamic-schema": {
      "operationID": "GetListSchema",
      "parameters": {
         "listID": {
            "parameter": "listID-dynamic"
         }
      },
      "value-path": "items"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [Özel API'leri ve bağlayıcılar açıklanmaktadır](../logic-apps/custom-connector-api-postman-collection.md)
* [Logic Apps: Bağlayıcınızı kaydetme](../logic-apps/logic-apps-custom-connector-register.md)
* [Akış: Bağlayıcınızı kaydetme](https://ms.flow.microsoft.com/documentation/register-custom-api/#register-your-custom-connector)