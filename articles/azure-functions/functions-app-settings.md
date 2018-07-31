---
title: Azure işlevleri için uygulama ayarları başvurusu
description: Azure işlevleri uygulama ayarları veya ortam değişkenleri için başvuru belgeleri.
services: functions
author: ggailey777
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/26/2017
ms.author: glenga
ms.openlocfilehash: b5f4ce786371608b276e41f6881dcb1e0a91e303
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39345064"
---
# <a name="app-settings-reference-for-azure-functions"></a>Azure işlevleri için uygulama ayarları başvurusu

Uygulama ayarlarında, bir işlev uygulaması, işlev uygulaması için tüm işlevleri etkiler genel yapılandırma seçenekleri içerir. Yerel olarak çalıştırdığınızda, bu ortam değişkenleri ayarlardır. Bu makalede, işlev uygulamalarında kullanılabilir uygulama ayarlarını listeler.

Diğer genel yapılandırma seçeneği yoktur [host.json](functions-host-json.md) dosya ve [local.settings.json](functions-run-local.md#local-settings-file) dosya.

## <a name="appinsightsinstrumentationkey"></a>APPINSIGHTS_INSTRUMENTATIONKEY

Application Insights kullanıyorsanız, Application Insights izleme anahtarı. Bkz: [Azure işlevlerini izleme](functions-monitoring.md).

|Anahtar|Örnek değer|
|---|------------|
|APPINSIGHTS_INSTRUMENTATIONKEY|5dbdd5e9-af77-484b-9032-64f83bb83bb|

## <a name="azurewebjobsdashboard"></a>AzureWebJobsDashboard

Günlükleri depolamak ve bunları görüntülemek için isteğe bağlı bir depolama hesabı bağlantı dizesi **İzleyici** portalında sekmesi. Bloblar, kuyruklar ve tablolar destekleyen genel amaçlı bir depolama hesabı olmalıdır. Bkz: [depolama hesabı](functions-infrastructure-as-code.md#storage-account) ve [depolama hesabı gereksinimleri](functions-create-function-app-portal.md#storage-account-requirements).

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsDashboard|DefaultEndpointsProtocol = https; AccountName = [name]; AccountKey = [anahtar]|

## <a name="azurewebjobsdisablehomepage"></a>AzureWebJobsDisableHomepage

`true` yol Giriş bir işlev uygulaması için kök URL'si gösterilen sayfası varsayılan devre dışı bırakın. `false` varsayılan değerdir.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsDisableHomepage|true|

Bu uygulama ayarı yok sayıldıysa veya kümesine ne zaman `false`, aşağıdaki örneğe benzer bir sayfa yanıt URL'si olarak görüntülenen `<functionappname>.azurewebsites.net`.

![İşlevi uygulama giriş sayfası](media/functions-app-settings/function-app-landing-page.png)

## <a name="azurewebjobsdotnetreleasecompilation"></a>AzureWebJobsDotNetReleaseCompilation

`true` anlamına gelir, .NET kodunu derlerken sürüm modu kullanın. `false` anlamına gelir, hata ayıklama modunu kullanın. `true` varsayılan değerdir.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsDotNetReleaseCompilation|true|

## <a name="azurewebjobsfeatureflags"></a>AzureWebJobsFeatureFlags

Beta özellikleri etkinleştirmek için virgülle ayrılmış listesi. Bu bayraklar tarafından beta özellikleriyle, üretime hazır değildir, ancak bunlar kullanıma sunulmadan önce Deneysel kullanımı için etkinleştirilebilir.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsFeatureFlags|özellik1, Özellik2|

## <a name="azurewebjobsscriptroot"></a>AzureWebJobsScriptRoot

Kök dizin yolu burada *host.json* dosya ve işlev klasörleri yer. Bir işlev uygulaması ile varsayılandır `%HOME%\site\wwwroot`.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsScriptRoot|%Home%\site\wwwroot|

## <a name="azurewebjobssecretstoragetype"></a>AzureWebJobsSecretStorageType

Depo veya için anahtar depolama sağlayıcı belirtir. Şu anda desteklenen depoları blob ("Blob") ve dosya sistemi ("disabled") ' dir. Dosya sistemi ("disabled") varsayılandır.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsSecretStorageType|devre dışı|

## <a name="azurewebjobsstorage"></a>AzureWebJobsStorage

Azure işlevleri çalışma zamanı HTTP tetiklemeli işlevleri hariç tüm işlevler için bu depolama hesabı bağlantı dizesi kullanır. Bloblar, kuyruklar ve tablolar destekleyen genel amaçlı bir depolama hesabı olmalıdır. Bkz: [depolama hesabı](functions-infrastructure-as-code.md#storage-account) ve [depolama hesabı gereksinimleri](functions-create-function-app-portal.md#storage-account-requirements).

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsStorage|DefaultEndpointsProtocol = https; AccountName = [name]; AccountKey = [anahtar]|

## <a name="azurewebjobstypescriptpath"></a>AzureWebJobs_TypeScriptPath

TypeScript için kullanılan derleme yolu. Gerekirse varsayılan geçersiz kılmanıza da olanak sağlar.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobs_TypeScriptPath|%Home%\typescript|

## <a name="functionappeditmode"></a>İŞLEV\_UYGULAMA\_DÜZENLE\_MODU

Geçerli değerler şunlardır: "readwrite" ve "salt okunur".

|Anahtar|Örnek değer|
|---|------------|
|İŞLEV\_UYGULAMA\_DÜZENLE\_MODU|salt okunur|

## <a name="functionsextensionversion"></a>İŞLEVLERİ\_UZANTISI\_SÜRÜMÜ

Bu işlev uygulamasında kullanmak için Azure işlevleri çalışma zamanı sürümü. Bir tilde ana sürümle (örneğin, "~ 1") bu ana sürüm en son sürümünü kullanmanız anlamına gelir. Aynı ana sürüm için yeni sürümler kullanılabilir olduğunda işlev uygulamasına otomatik olarak yüklenirler. Belirli bir sürüme uygulamayı sabitlemek için tam sürüm numarası (örneğin, "1.0.12345") kullanın. "~ 1" varsayılandır.

|Anahtar|Örnek değer|
|---|------------|
|İŞLEVLERİ\_UZANTISI\_SÜRÜMÜ|~1|

## <a name="websitecontentazurefileconnectionstring"></a>WEBSITE_CONTENTAZUREFILECONNECTIONSTRING

Yalnızca tüketim planları için. İşlevi uygulama kodu ve yapılandırması depolandığı depolama hesabı için bağlantı dizesi. Bkz: [bir işlev uygulaması oluşturma](functions-infrastructure-as-code.md#create-a-function-app).

|Anahtar|Örnek değer|
|---|------------|
|WEBSITE_CONTENTAZUREFILECONNECTIONSTRING|DefaultEndpointsProtocol = https; AccountName = [name]; AccountKey = [anahtar]|

## <a name="websitecontentshare"></a>WEBSITE_CONTENTSHARE

Yalnızca tüketim planları için. İşlev uygulaması kod ve yapılandırma dosyası yolu. WEBSITE_CONTENTAZUREFILECONNECTIONSTRING ile kullanılır. Varsayılan işlev uygulamasının adı ile başlayan benzersiz bir dizedir. Bkz: [bir işlev uygulaması oluşturma](functions-infrastructure-as-code.md#create-a-function-app).

|Anahtar|Örnek değer|
|---|------------|
|WEBSITE_CONTENTSHARE|functionapp091999e2|

## <a name="websitemaxdynamicapplicationscaleout"></a>WEB SİTESİ\_MAX\_DİNAMİK\_UYGULAMA\_ÖLÇEK\_ÇIKIŞ

İşlev uygulaması için ölçeğini genişletebilirsiniz örneklerinin sayısı. Varsayılan olarak sınır yoktur.

> [!NOTE]
> Bu ayar için bir önizleme özelliğidir.

|Anahtar|Örnek değer|
|---|------------|
|WEB SİTESİ\_MAX\_DİNAMİK\_UYGULAMA\_ÖLÇEK\_ÇIKIŞ|10|

## <a name="websitenodedefaultversion"></a>WEB SİTESİ\_DÜĞÜM\_DEFAULT_VERSION

"6.5.0" varsayılandır.

|Anahtar|Örnek değer|
|---|------------|
|WEB SİTESİ\_DÜĞÜM\_DEFAULT_VERSION|6.5.0|

## <a name="next-steps"></a>Sonraki adımlar

[Uygulama ayarlarını güncelleştirme hakkında bilgi edinin](functions-how-to-use-azure-function-app-settings.md#manage-app-service-settings)

[Genel ayarlar host.json dosyasına bakın](functions-host-json.md)

[App Service uygulamalarını diğer uygulama ayarlarını bakın](https://github.com/projectkudu/kudu/wiki/Configurable-settings)
