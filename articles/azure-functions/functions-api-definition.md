---
title: Azure işlevleri'nde Openapı meta verileri | Microsoft Docs
description: Azure işlevleri'nde Openapı desteğine genel bakış
services: functions
author: alexkarcher-msft
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alkarche
ms.openlocfilehash: 6d11961f06a75341e633c7a8963e6b83ed37cf13
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61341734"
---
# <a name="openapi-20-metadata-support-in-azure-functions-preview"></a>Openapı 2.0 (Önizleme) Azure işlevleri'nde meta veri desteği
Openapı 2.0 (eski adıyla Swagger) meta verileri desteği Azure işlevleri bir Openapı 2.0 tanımı bir işlev uygulaması içinde yazmak için kullanabileceğiniz bir önizleme özelliği değil. Ardından, işlev uygulamasını kullanarak bu dosyayı barındırabilirsiniz.

> [!IMPORTANT]
> OpenAPI önizleme özelliği şu anda yalnızca 1.x çalışma zamanında kullanılabilir. 1.x işlev uygulaması oluşturma hakkında bilgi [burada bulunabilir](./functions-versions.md#creating-1x-apps).

[Openapı meta verileri](https://swagger.io/) çok çeşitli diğer yazılımlar tarafından kullanılması için bir REST API'sini barındıran bir işlev verir. Bu yazılım PowerApps gibi Microsoft teklifleriyle içerir ve [Azure App Service API Apps özelliklerinden](../app-service/overview.md), gibi üçüncü taraf geliştirici araçları [Postman](https://www.getpostman.com/docs/importing_swagger), ve [diğer birçok paket](https://swagger.io/tools/).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

>[!TIP]
>İle başlamanızı öneririz [çalışmaya başlama Öğreticisi](./functions-api-definition-getting-started.md) ve belirli özellikler hakkında daha fazla bilgi edinmek için bu belgede daha sonra dönme.

## <a name="enable"></a>Openapı tanımı desteğini etkinleştir
Tüm Openapı ayarları yapılandırabileceğiniz **API tanımı** işlev uygulamanızın sayfasında **Platform özellikleri**.

> [!NOTE]
> İşlev API'si tanımı özelliği şu anda beta çalışma zamanı için desteklenmiyor.

Barındırılan bir Openapı tanımı ve bir hızlı başlangıç tanımı oluşturmayı etkinleştirmek için **API tanımı kaynağı** için **işlevi (Önizleme)**. **Dış URL** başka bir yerde barındırılan bir Openapı tanımı kullanmak, işlevinizi sağlar.

## <a name="generate-definition"></a>İşlevinizin meta verilerini Swagger skeleton oluştur
Bir şablon ilk Openapı tanımınızı yazmaya yardımcı olabilir. Tanım şablonu özelliğini tüm meta verilerin function.json dosyasında her, HTTP tetikleyicisi işlevlerini kullanarak seyrek bir Openapı tanımı oluşturur. API'niz hakkında daha fazla bilgi doldurmanız gerekir [Openapı belirtimi](https://swagger.io/specification/), istek ve yanıt şablonlar gibi.

Adım adım yönergeler için bkz: [çalışmaya başlama Öğreticisi](./functions-api-definition-getting-started.md).

### <a name="templates"></a>Kullanılabilir şablonlar

|Ad| Açıklama |
|:-----|:-----|
|Oluşturulan tanım|En fazla işlevin var olan meta verileri çıkarılan bilgi ile bir Openapı tanımı.|

### <a name="quickstart-details"></a>Oluşturulan tanımında bulunan meta verileri

Oluşturulan bir Swagger çatı eşleştirildiğinden aşağıdaki tabloda Azure portal ayarları ve function.json karşılık gelen verileri temsil eder.

|Swagger.json|Portal kullanıcı Arabirimi|Function.JSON|
|:----|:-----|:-----|
|[Ana Bilgisayar](https://swagger.io/specification/#fixed-fields-15)|**İşlev uygulaması ayarları** > **App Service ayarlarını** > **genel bakış** > **URL'si**|*Mevcut değil*
|[Yolları](https://swagger.io/specification/#paths-object-29)|**Tümleştirme** > **seçili HTTP metotları**|Bindings: Yol
|[Yol öğesi](https://swagger.io/specification/#path-item-object-32)|**Tümleştirme** > **rota şablonu**|Bindings: Yöntemler
|[Güvenlik](https://swagger.io/specification/#security-scheme-object-112)|**anahtarları**|*Mevcut değil*|
|Operationıd *|**Rota + izin verilen fiiller**|Rota + izin verilen fiiller|

\*İşlem kimliği, yalnızca PowerApps ve Flow ile tümleştirmek için gereklidir.
> [!NOTE]
> X-ms-summary uzantı bir görünen ad'ın Logic Apps, PowerApps ve Flow sağlar.
>
> Daha fazla bilgi için bkz. [PowerApps için Swagger tanımınızı özelleştirme](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/).

## <a name="CICD"></a>Bir API tanımı için CI/CD kullanın

 Kaynak denetiminden API tanımınıza değiştirmek kaynak denetimi etkinleştirmeden önce portalda barındırma API tanımı etkinleştirmeniz gerekir. Bu yönergeleri izleyin:

1. Gözat **API tanımı (Önizleme)** , işlev uygulaması ayarları içinde.
   1. Ayarlama **API tanımı kaynağı** için **işlevi**.
   1. Tıklayın **API tanımı şablonu oluşturun** ardından **Kaydet** daha sonra değiştirmek için bir şablon tanımı oluşturmak için.
   1. API tanımı URL'si ve anahtarı not edin.
1. [Sürekli Tümleştirme/sürekli dağıtım (CI/CD) ayarlayın](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment#continuous-deployment-requirements).
2. Sürümünde kaynak denetiminde \site\wwwroot swagger.JSON değiştirme\.azurefunctions\swagger\swagger.json.

Şimdi, swagger.json deponuzda yapılan API işlev uygulamanız tarafından barındırılan tanımı URL'si ve de not ettiğiniz anahtar adım 1.c.

## <a name="next-steps"></a>Sonraki adımlar
* [Çalışmaya başlama Öğreticisi](functions-api-definition-getting-started.md). Bir Openapı tanımı iş başında görmek için izlenecek deneyin.
* [Azure işlevleri GitHub deposu](https://github.com/Azure/Azure-Functions/). API tanımı önizlemesi hakkında geri bildirimde bulunun için işlevleri depoya göz atın. Bir GitHub sorunu güncelleştirilmiş görmek istiyorsanız her şey için yapın.
* [Azure işlevleri Geliştirici Başvurusu](functions-reference.md). İşlevlerin kodlanması ve tetikleyicilerin ve bağlamaların tanımlanması hakkında bilgi edinin.
