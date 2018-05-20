---
title: Azure işlevleri için uygulama ayarları başvurusu
description: Azure işlevleri uygulama ayarları veya ortam değişkenleri için başvuru belgeleri.
services: functions
author: tdykstra
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
ms.author: tdykstra
ms.openlocfilehash: bd5603b8f0e15eeae9dd3799d4e10952e115680f
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="app-settings-reference-for-azure-functions"></a>Azure işlevleri için uygulama ayarları başvurusu

Bir işlev uygulamasında uygulama ayarları, bu işlevi uygulama için tüm işlevleri etkileyen genel yapılandırma seçenekleri içerir. Yerel olarak çalıştırdığınızda, bu ortam değişkenleri ayarlardır. Bu makalede işlevi uygulamalarında kullanılabilir uygulama ayarlarını listeler.

Diğer genel yapılandırma seçenekleri vardır [host.json](functions-host-json.md) dosya ve [local.settings.json](functions-run-local.md#local-settings-file) dosya.

## <a name="appinsightsinstrumentationkey"></a>APPINSIGHTS_INSTRUMENTATIONKEY

Application Insights kullanıyorsanız, Application Insights izleme anahtarı. Bkz: [izlemek Azure işlevleri](functions-monitoring.md).

|Anahtar|Örnek değer|
|---|------------|
|APPINSIGHTS_INSTRUMENTATIONKEY|5dbdd5e9-af77-484b-9032-64f83bb83bb|

## <a name="azurewebjobsdashboard"></a>AzureWebJobsDashboard

Günlüklerini depolamak ve bunları görüntülemek için isteğe bağlı bir depolama hesabı bağlantı dizesi **İzleyici** portal sekmesindedir. Depolama hesabı BLOB, kuyruklar ve tablolar destekleyen bir genel amaçlı olması gerekir. Bkz: [depolama hesabı](functions-infrastructure-as-code.md#storage-account) ve [depolama hesabı gereksinimleri](functions-create-function-app-portal.md#storage-account-requirements).

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsDashboard|DefaultEndpointsProtocol = https; AccountName = [name]; AccountKey = [anahtar]|

## <a name="azurewebjobsdisablehomepage"></a>AzureWebJobsDisableHomepage

`true` anlamına gelir Giriş bir işlev uygulaması kök URL'si için gösterilen sayfasında varsayılan devre dışı bırakın. `false` varsayılan değerdir.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsDisableHomepage|true|

Bu uygulama ayarı atlanmış veya diğer kümesine ne zaman `false`, yanıt URL'si olarak aşağıdaki örneğe benzer bir sayfa görüntülenir `<functionappname>.azurewebsites.net`.

![İşlev uygulama giriş sayfası](media/functions-app-settings/function-app-landing-page.png)

## <a name="azurewebjobsdotnetreleasecompilation"></a>AzureWebJobsDotNetReleaseCompilation

`true` .NET kodu derlerken anlamına gelir yayın modunu kullanın; `false` anlamına gelir, hata ayıklama modunu kullanın. `true` varsayılan değerdir.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsDotNetReleaseCompilation|true|

## <a name="azurewebjobsfeatureflags"></a>AzureWebJobsFeatureFlags

Beta özellikleri etkinleştirmek için virgülle ayrılmış listesi. Bu bayrakların tarafından etkin beta özellikler üretim hazır değildir, ancak kullanıma sunulmadan önce Deneysel kullanımı için etkinleştirilebilir.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsFeatureFlags|feature1, feature2|

## <a name="azurewebjobsscriptroot"></a>AzureWebJobsScriptRoot

Kök dizin yolu burada *host.json* dosya ve işlev klasörlerin bulunduğu. Bir işlev uygulaması varsayılandır `%HOME%\site\wwwroot`.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsScriptRoot|%Home%\site\wwwroot|

## <a name="azurewebjobssecretstoragetype"></a>AzureWebJobsSecretStorageType

Depo veya anahtar depolaması için kullanılacak sağlayıcıyı belirtir. Şu anda desteklenen depoları blob ("Blob") ve dosya sistemi ("disabled") ' dir. Varsayılan değer ("disabled") dosya sistemidir.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsSecretStorageType|devre dışı|

## <a name="azurewebjobsstorage"></a>AzureWebJobsStorage

Azure işlevleri çalışma zamanı bu depolama hesabı bağlantı dizesi tetiklenen HTTP işlevler hariç tüm işlevler için kullanır. Depolama hesabı BLOB, kuyruklar ve tablolar destekleyen bir genel amaçlı olması gerekir. Bkz: [depolama hesabı](functions-infrastructure-as-code.md#storage-account) ve [depolama hesabı gereksinimleri](functions-create-function-app-portal.md#storage-account-requirements).

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobsStorage|DefaultEndpointsProtocol = https; AccountName = [name]; AccountKey = [anahtar]|

## <a name="azurewebjobstypescriptpath"></a>AzureWebJobs_TypeScriptPath

TypeScript için kullanılan derleme yolu. Gerekirse varsayılan geçersiz kılmanıza olanak tanır.

|Anahtar|Örnek değer|
|---|------------|
|AzureWebJobs_TypeScriptPath|%Home%\typescript|

## <a name="functionappeditmode"></a>İŞLEV\_UYGULAMA\_DÜZENLE\_MODU

Geçerli değerler "readwrite" ve "salt okunur" dir.

|Anahtar|Örnek değer|
|---|------------|
|İŞLEV\_UYGULAMA\_DÜZENLE\_MODU|salt okunur|

## <a name="functionsextensionversion"></a>İŞLEVLER\_UZANTISI\_SÜRÜMÜ

Bu işlev uygulamayı kullanmak için Azure işlevleri çalışma zamanı sürümü. Bir tilde ana sürümü bu sürümle (örneğin, "~ 1") en son sürümünü kullanmanız anlamına gelir. Aynı ana sürüm için yeni sürümler kullanılabilir olduğunda, işlev uygulaması otomatik olarak yüklenirler. Belirli bir sürüme uygulama sabitlemek için tam sürüm numarası (örneğin, "1.0.12345") kullanın. "~ 1" varsayılandır.

|Anahtar|Örnek değer|
|---|------------|
|İŞLEVLER\_UZANTISI\_SÜRÜMÜ|~1|

## <a name="websitecontentazurefileconnectionstring"></a>WEBSITE_CONTENTAZUREFILECONNECTIONSTRING

Yalnızca tüketim planları için. Depolama hesabı yapılandırma ve işlev uygulama kodu depolandığı için bağlantı dizesi. Bkz: [bir işlev uygulaması oluşturma](functions-infrastructure-as-code.md#create-a-function-app).

|Anahtar|Örnek değer|
|---|------------|
|WEBSITE_CONTENTAZUREFILECONNECTIONSTRING|DefaultEndpointsProtocol = https; AccountName = [name]; AccountKey = [anahtar]|

## <a name="websitecontentshare"></a>WEBSITE_CONTENTSHARE

Yalnızca tüketim planları için. İşlev uygulama kodu ve yapılandırma dosyası yolu. WEBSITE_CONTENTAZUREFILECONNECTIONSTRING ile kullanılır. Varsayılan işlev uygulama adı ile başlayan benzersiz bir dizedir. Bkz: [bir işlev uygulaması oluşturma](functions-infrastructure-as-code.md#create-a-function-app).

|Anahtar|Örnek değer|
|---|------------|
|WEBSITE_CONTENTSHARE|functionapp091999e2|

## <a name="websitemaxdynamicapplicationscaleout"></a>WEB SİTESİ\_MAX\_DİNAMİK\_UYGULAMA\_ÖLÇEK\_ÇIKIŞI

İşlev uygulaması çıkışı ölçeklenebilen örneği maksimum sayısı. Varsayılan olarak sınır yoktur.

> [!NOTE]
> Bu ayar için bir önizleme özelliğidir.

|Anahtar|Örnek değer|
|---|------------|
|WEB SİTESİ\_MAX\_DİNAMİK\_UYGULAMA\_ÖLÇEK\_ÇIKIŞI|10|

## <a name="websitenodedefaultversion"></a>WEB SİTESİ\_DÜĞÜMÜ\_DEFAULT_VERSION

Varsayılan değer "6.5.0" dir.

|Anahtar|Örnek değer|
|---|------------|
|WEB SİTESİ\_DÜĞÜMÜ\_DEFAULT_VERSION|6.5.0|

## <a name="next-steps"></a>Sonraki adımlar

[Uygulama ayarlarını güncelleştirme öğrenin](functions-how-to-use-azure-function-app-settings.md#manage-app-service-settings)

[Genel ayarları host.json dosyasına bakın](functions-host-json.md)

[Diğer uygulama ayarlarını uygulama hizmeti uygulamalar için bkz:](https://github.com/projectkudu/kudu/wiki/Configurable-settings)
