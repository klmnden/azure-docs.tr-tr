---
title: "Azure işlevleri OpenAPI meta verilerde | Microsoft Docs"
description: "Azure işlevleri OpenAPI desteği'ne genel bakış"
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: cfowler
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 03/23/2017
ms.author: alkarche
ms.openlocfilehash: b6aacc536e589a2036aba5a0784a4ba71641a59e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="openapi-20-metadata-support-in-azure-functions-preview"></a>OpenAPI 2.0 meta verileri desteği Azure işlevlerinde (Önizleme)
OpenAPI 2.0 (önceki adıyla Swagger) meta verileri desteği Azure işlevleri OpenAPI 2.0 tanımının bir işlev uygulaması içinde yazmak için kullanabileceğiniz bir önizleme özelliği değil. Bu dosyayı işlev uygulaması kullanarak ardından barındırabilir.

[OpenAPI meta veri](http://swagger.io/) çok çeşitli diğer yazılımlar tarafından kullanılması için bir REST API barındıran bir işlev sağlar. Bu yazılım Microsoft teklifleri PowerApps gibi içerir ve [Azure App Service API Apps özelliğini](../app-service/app-service-web-overview.md), üçüncü taraf geliştirici araçları ister [Postman](https://www.getpostman.com/docs/importing_swagger), ve [pek çok daha fazla paket](http://swagger.io/tools/).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

>[!TIP]
>İle başlayan öneririz [başlama Öğreticisi](./functions-api-definition-getting-started.md) ve belirli özellikler hakkında daha fazla bilgi edinmek için bu belgeye döndürüyor.

## <a name="enable"></a>OpenAPI tanımı desteğini etkinleştir
Tüm OpenAPI ayarları yapılandırabilirsiniz **API tanımı** işlevi uygulamanızın sayfa **Platform özellikleri**.

Barındırılan bir OpenAPI tanımı ve hızlı başlangıç tanım oluşturulmasını etkinleştirmek için ayarlanmış **API tanımı kaynak** için **işlevi (Önizleme)**. **Dış URL** başka bir yerde barındırılan bir OpenAPI tanımı kullanmak, işlevi sağlar.

## <a name="generate-definition"></a>İşlevin meta verilerini Swagger çatıyı oluştur
Bir şablonu ilk OpenAPI tanımınızı yazma başlamanıza yardımcı olabilir. Tanımı şablon özelliği bir seyrek OpenAPI tanımı tüm meta verilerin function.json dosyasında her bir HTTP tetikleyicisi işlevlerinizi için kullanarak oluşturur. API öğesinden hakkında daha fazla bilgi doldurmanız gerekir [OpenAPI belirtimi](http://swagger.io/specification/), istek ve yanıt şablonlar gibi.

Adım adım yönergeler için bkz: [başlama Öğreticisi](./functions-api-definition-getting-started.md).

### <a name="templates"></a>Kullanılabilir şablonlar

|Ad| Açıklama |
|:-----|:-----|
|Oluşturulan tanımı|Bir işlevin varolan meta verilerini çıkarsanabileceği bilgi maksimum OpenAPI tanımıyla.|

### <a name="quickstart-details"></a>Oluşturulan tanımı'nda bulunan meta verileri

Oluşturulan Swagger çatıyı eşlenmiş olarak aşağıdaki tabloda Azure portal ayarları ve function.json karşılık gelen verileri temsil eder.

|Swagger.JSON|Portal UI|Function.JSON|
|:----|:-----|:-----|
|[Ana bilgisayar](http://swagger.io/specification/#fixed-fields-15)|**İşlev uygulaması ayarları** > **App Service ayarlarını** > **genel bakış** > **URL'si**|*Mevcut değil*
|[Yolları](http://swagger.io/specification/#paths-object-29)|**Tümleştirme** > **seçili HTTP yöntemleri**|Bağlamaları: yol
|[Yol öğesi](http://swagger.io/specification/#path-item-object-32)|**Tümleştirme** > **rota şablonu**|Bağlamaları: yöntemler
|[Güvenlik](http://swagger.io/specification/#security-scheme-object-112)|**Anahtarları**|*Mevcut değil*|
|Operationıd *|**Rota + izin verilen fiiller**|Rota + izin verilen fiiller|

\*İşlem kimliği, yalnızca PowerApps ve akış ile tümleştirmek için gereklidir.
> [!NOTE]
> X-ms-Özet uzantısı bir görünen ad mantıksal uygulamalar, PowerApps ve akış sağlar.
>
> Daha fazla bilgi için bkz: [PowerApps için Swagger tanımı özelleştirme](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/).

## <a name="CICD"></a>CI/CD bir API tanımı ayarlamak için kullanın

 API tanımı, API tanımı kaynak denetiminden değiştirmek kaynak denetimi etkinleştirmeden önce portalda barındırma etkinleştirmeniz gerekir. Bu yönergeleri izleyin:

1. Gözat **API tanımı (Önizleme)** işlevi uygulama ayarları'nda.
  1. Ayarlama **API tanımı kaynak** için **işlevi**.
  1. Tıklatın **oluşturmak API tanımı şablonu** ve ardından **kaydetmek** daha sonra değiştirmek için bir şablon tanımı oluşturmak için.
  1. API tanımı URL'si ve anahtar unutmayın.
1. [Sürekli Tümleştirme/sürekli dağıtım (CI/CD) ayarlayın](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment#continuous-deployment-requirements).
2. Swagger.JSON \site\wwwroot adresindeki kaynak denetiminde değiştirme\.azurefunctions\swagger\swagger.json.

Şimdi, deponuzun swagger.json değişiklikleri işlevi uygulamanızı API tarafından barındırılan tanımı URL'si ve de not ettiğiniz anahtarı adım 1.c.

## <a name="next-steps"></a>Sonraki adımlar
* [Başlangıç Öğreticisi](functions-api-definition-getting-started.md). Bir eylem OpenAPI tanımında görmek için izlenecek deneyin.
* [Azure işlevleri GitHub deposunu](https://github.com/Azure/Azure-Functions/). API tanımı destek Önizleme'bize geri bildirim sağlamak için işlevleri depo göz atın. GitHub sorunu güncelleştirilmiş görmek istediğiniz her şey için yapın.
* [Azure işlevleri Geliştirici Başvurusu](functions-reference.md). İşlevlerin kodlanması ve tetikleyicilerin ve bağlamaların tanımlanması hakkında bilgi edinin.
