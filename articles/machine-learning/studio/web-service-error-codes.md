---
title: Azure Machine Learning REST API'si hata kodları | Microsoft Docs
description: Bu hata kodları, bir Azure Machine Learning web hizmeti üzerinde bir işlemi tarafından döndürülebilir.
keywords: ''
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.author: hshapiro
manager: hjerez
editor: cgronlun
ms.assetid: 0923074b-3728-439d-a1b8-8a7245e39be4
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 11/16/2016
ms.openlocfilehash: 0ba44b2a93bcd542db1350def2d0554c8c44233c
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="machine-learning-rest-api-error-codes"></a>Machine Learning REST API hata kodları
 
Aşağıdaki hata kodları, bir Azure Machine Learning web hizmeti üzerinde bir işlemi tarafından döndürülebilir.
 
## <a name="badargument-http-status-code-400"></a>BadArgument (HTTP durum kodu 400)
 
Geçersiz bağımsız değişkeni sağlanmadı.
 
Bu sınıf hataların bir yerde sağlanan bağımsız değişken geçersiz anlamına gelir. Bu kimlik bilgileri veya bir web hizmetine geçirilen Azure depolama konumunu olabilir. Lütfen hata "code" alanında hangi belirli bağımsız değişkeni geçersiz tanılamak için "Ayrıntılar" bölümüne bakın.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| BadParameterValue | Sağlanan parametre değeri parametre parametre kuralında uygun değil |
| BadSubscriptionId | Abonelik Puanlama amacıyla kullanılan kimliği kaynak mevcut değil |
| BadVersionCall | Geçersiz sürüm parametresi geçirildi API çağrısı sırasında: {0}. Doğru sürümü geçirme için API Yardım sayfası denetleyin ve yeniden deneyin. |
| BatchJobInputsNotSpecified | Aşağıdaki input(s) istekle belirtilmedi gerekli: {0}. Lütfen olun tüm giriş verilerini belirtilir ve yeniden deneyin. |
| BatchJobInputsTooManySpecified | İstek hizmette tanımlanan olandan daha fazla girişler belirtildi. Kabul edilen input(s) listesi: {0}. Lütfen tüm giriş verilerini doğru belirtildiğinden emin yeniden deneyin. |
| BlobNameTooLong | Tanılama çıktıları uzun için belirtilen azure blob depolama alanı yolu: {0}. Yolun kısaltın ve yeniden deneyin. |
| BlobNotFound | Sağlanan Azure blob - {0} erişilemiyor.  Azure hata iletisi: {1}. |
| ContainerIsEmpty | Hiçbir Azure depolama kapsayıcısı adı sağlandı. Geçerli kapsayıcısı adı sağlayın ve yeniden deneyin. |
| ContainerSegmentInvalid | Geçersiz kapsayıcı adı. Geçerli kapsayıcısı adı sağlayın ve yeniden deneyin. |
| ContainerValidationFailed | BLOB kapsayıcı doğrulama Bu hata ile başarısız oldu: {0}. |
| DataTypeNotSupported | Sağlanan desteklenmeyen veri türü. Geçerli veri türleri sağlayın ve yeniden deneyin. |
| DuplicateInputInBatchCall | Toplu isteği geçersiz. Hem tek hem de birden çok giriş aynı anda belirtilemez. Bu öğelerden birini istekten kaldırın ve yeniden deneyin. |
| ExpiryTimeInThePast | Sağlanan bitiş zamanı geçmişte: {0}. Gelecekte süre sonu zamanı, UTC sağlayın ve yeniden deneyin. Süresi dolmayacak için sona erme saati NULL olarak ayarlayın. |
| IncompleteSettings | Tanılama ayarları eksik. |
| InputBlobRelativeLocationInvalid | Sağlanan Azure depolama blob adı yok. Geçerli blob adı sağlayın ve yeniden deneyin. |
| InvalidBlob | Blob geçersiz blob belirtimi: {0}. Bu bağlantı dizesini doğrulayın / göreli bir yol veya SAS belirteci belirtimi doğru olduğundan ve yeniden deneyin. |
| InvalidBlobConnectionString | Geçersiz giriş/çıkış BLOB'ları biri için belirtilen bağlantı dizesi: {0}. Lütfen bunu düzeltin ve yeniden deneyin. |
| InvalidBlobExtension | Blob başvurusu: {0} geçersiz veya eksik dosya uzantısına sahip. Bu çıktı türü için desteklenen dosya uzantıları: "{1}". |
| InvalidInputNames | Geçersiz hizmet adı istekte belirtilen giriş: {0}. Lütfen doğru hizmet girişleri giriş verilerini eşleştirin ve yeniden deneyin. |
| InvalidOutputOverrideName | Geçersiz çıkış adı geçersiz kıl: {0}. Hizmet, bu ada sahip bir çıkış düğüm yok. Lütfen doğru çıkış içinde geçersiz kılmak için düğüm adı geçirin (büyük küçük harfe duyarlılığın geçerlidir). |
| InvalidQueryParameter | Geçersiz sorgu parametresi '{0}'. {1} |
| MissingInputBlobInformation | Azure storage blobu bilgileri eksik. Geçerli bağlantı dizesi ve göreli bir yol veya URI sağlayın ve yeniden deneyin. |
| MissingJobId | Hiçbir iş sağlanan kimliği. Bir işi ilk kez gönderildiğinde bir iş kimliği döndürülür. İş kimliği doğru olduğunu doğrulayıp yeniden deneyin. |
| MissingKeys | Sağlanan anahtar yok veya bir birincil veya ikincil anahtarı sağlanmaz. |
| MissingModelPackage | Model paket kimliği veya sağlanan modeli paketi yok. Geçerli model paket kimliği sağlayın veya paket modeli ve tekrar deneyin. |
| MissingOutputOverrideSpecification | İstek çıkış geçersiz kılma {0} blob belirtimi eksik. Lütfen istekle geçerli blob konumunu belirtin veya konumu geçersiz kılma isterseniz çıkış belirtimini kaldırın. |
| MissingRequestInput | Web hizmeti, bir girdi bekliyor, ancak herhangi bir giriş sağlanmadı. Geçerli girişleri sağlanan modelinde yayımlanan giriş noktalarına dayalı emin olun ve yeniden deneyin. |
| MissingRequiredGlobalParameters | Tüm web hizmeti parametreleri sağlanan gereklidir. Modüller için beklenen parametreler doğru olduğundan emin olun ve yeniden deneyin. |
| MissingRequiredOutputOverrides | Çıktı geçirin zorunludur şifrelenmiş Hizmeti uç noktası çağrılırken hizmetin tüm çıkışlar için geçersiz kılar. Şu anda bu çıktıları için geçersiz kılmaları eksik: {0} |
| MissingWebServiceGroupId | Hiçbir web hizmeti grubu kimliği sağlanır. Geçerli web hizmeti grubu kimliği sağlayın ve yeniden deneyin. |
| MissingWebServiceId | Hiçbir web hizmeti sağlanan kimliği. Geçerli web sitesi kimliği sağlayın ve yeniden deneyin. |
| MissingWebServicePackage | Web hizmeti paketi sağlanan. Geçerli web hizmeti paketi girin ve yeniden deneyin. |
| MissingWorkspaceId | Çalışma alanı kimliği girildi. Geçerli çalışma alanı kimliği sağlayın ve yeniden deneyin. |
| ModelConfigurationInvalid | Modeli paketi yapılandırmasında geçersiz model. Çıkış uç tanımı, standart hata uç noktası, model yapılandırması içerir ve std uç nokta kapatın ve yeniden deneyin emin olun. |
| ModelPackageIdInvalid | Geçersiz model paket kimliği Model paket kimliği doğru olduğundan emin olun ve yeniden deneyin. |
| RequestBodyInvalid | Hiçbir istek gövdesinde sağlanan veya istek gövdesi seri durumdan çıkarılırken hata oluştu. |
| RequestIsEmpty | Sağlanan hiçbir isteği. Geçerli bir istek sağlayın ve yeniden deneyin. |
| UnexpectedParameter | Sağlanan beklenmeyen parametre. Doğrulama tüm parametre adları doğru yazıldığından yalnızca beklenen parametreleri geçirilir ve yeniden deneyin. |
| Başvuruları | Bilinmeyen hata. |
| UserParameterInvalid | {0} |
| WebServiceConcurrentRequestRequirementInvalid | {0} web hizmeti için eş zamanlı istekler gereksinimleri değiştiremezsiniz. |
| WebServiceIdInvalid | Sağlanan geçersiz web hizmeti kimliği. Web hizmeti kimliği geçerli bir GUID olmalıdır. |
| WebServiceTooManyConcurrentRequestRequirement | Eş zamanlı istek gereksinim birden fazla {0} ayarlanamıyor. |
| WebServiceTypeInvalid | Sağlanan geçersiz web hizmeti türü. Geçerli web hizmeti türü doğru olduğunu doğrulayıp yeniden deneyin. Geçerli web hizmeti türleri: {0}. |
 
## <a name="baduserargument-http-status-code-400"></a>BadUserArgument (HTTP durum kodu 400)
 
Geçersiz kullanıcı bağımsız değişkeni sağlanmadı.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| InputMismatchError | Giriş verisi giriş bağlantı noktası şeması eşleşmiyor. |
| InputParseError | Giriş vektörü başarısız ayrıştırma.  Sütunları ve türleri doğru sayıda giriş vektör olduğunu doğrulayın.  Ek ayrıntıları: {0}. |
| MissingRequiredGlobalParameters | Web hizmeti tarafından beklenen parametre eksik. Web hizmeti tarafından beklenen tüm gerekli parametreleri doğru olduğundan emin olun ve yeniden deneyin. |
| UnexpectedParameter | Doğrulayın yalnızca web hizmeti tarafından beklenen gerekli parametreleri geçirilir ve yeniden deneyin. |
| UserParameterInvalid | {0} |
 
## <a name="invalidoperation-http-status-code-400"></a>InvalidOperation (HTTP durum kodu 400)
 
İstek Geçerli bağlamda geçersiz.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| CannotStartJob | {0} durumda olduğundan, işi başlatılamıyor. |
| IncompatibleModel | Model isteği sürümüyle uyumlu değil. İstek sürümü yalnızca tek datatable çıkış modellerini destekler. |
| MultipleInputsNotAllowed | Model birden çok girişi izin vermiyor. |
 
## <a name="libraryexecutionerror-http-status-code-400"></a>LibraryExecutionError (HTTP durum kodu 400)
 
Modül yürütme iç kitaplığı hatayla karşılaştı.
 
 
## <a name="moduleexecutionerror-http-status-code-400"></a>ModuleExecutionError (HTTP durum kodu 400)
 
Modül yürütme bir hatayla karşılaştı.
 
 
## <a name="webservicepackageerror-http-status-code-400"></a>WebServicePackageError (HTTP durum kodu 400)
 
Geçersiz web hizmeti paketi. Sağlanan web hizmeti paketi doğru olduğunu doğrulayıp yeniden deneyin.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| FormatError | Web hizmeti paketi bozuk. Ayrıntılar: {0} |
| RuntimesError | Web hizmeti paketi grafik geçersiz. Ayrıntılar: {0} |
| ValidationError | Web hizmeti paketi grafik geçersiz. Ayrıntılar: {0} |
 
## <a name="unauthorized-http-status-code-401"></a>Yetkisiz (HTTP durum kodu 401)
 
İstek için erişim kaynak yetkilendirilmemiş.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| AdminRequestUnauthorized | Yetkilendirilmemiş |
| ManagementRequestUnauthorized | Yetkilendirilmemiş |
| ScoreRequestUnauthorized | Geçersiz kimlik bilgileri sağlandı. |
 
## <a name="notfound-http-status-code-404"></a>NotFound (HTTP durum kodu 404)
 
Kaynak bulunamadı.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| ModelPackageNotFound | Model paketi bulunamadı. Model paket kimliği doğru olduğunu doğrulayıp yeniden deneyin. |
| WebServiceIdNotFoundInWorkspace | Web hizmeti bulunamadı Bu çalışma alanı altında. WebServiceId Workspaceıd arasında bir uyuşmazlık var. Sağlanan web hizmeti çalışma alanının parçası olduğunu doğrulayıp yeniden deneyin. |
| WebServiceNotFound | Web hizmeti bulunamadı. Web hizmeti kimliği doğru olduğunu doğrulayıp yeniden deneyin. |
| WorkspaceNotFound | Çalışma alanı bulunamadı. Çalışma alanı kimliği doğru olduğunu doğrulayıp yeniden deneyin. |
 
## <a name="requesttimeout-http-status-code-408"></a>RequestTimeout (HTTP durum kodu 408)
 
İşlem izin verilen sürede tamamlanamadı.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| RequestCanceled | İstek, istemci tarafından iptal edildi. |
| ScoreRequestTimeout | Yürütme isteği zaman aşımına uğradı. |
 
## <a name="conflict-http-status-code-409"></a>Çakışma (HTTP durum kodu 409)
 
Kaynak zaten var.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| ModelOutputMetadataMismatch | Geçersiz çıkış parametre adı. Sütunları yeniden adlandırın ve yeniden denemek için meta veriler Düzenleyicisi modülü kullanmayı deneyin. |
 
## <a name="memoryquotaviolation-http-status-code-413"></a>MemoryQuotaViolation (HTTP status code 413)
 
Model, kendisine atanmış bellek kotasını aştı.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| OutOfMemoryLimit | Model için tahsis olandan daha fazla bellek tüketilen. Modeli için izin verilen maksimum bellekten {0} MB ' dir. Modelinizi sorunları gözden geçirin. |
 
## <a name="internalerror-http-status-code-500"></a>InternalError (HTTP durum kodu 500)
 
Yürütme bir iç hatayla karşılaştı.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| AdminAuthenticationFailed |  |
| BackendArgumentError |  |
| BackendBadRequest |  |
| ClusterConfigBlobMisconfigured |  |
| ContainerProcessTerminatedWithSystemError | Sistem hatası ile kapsayıcı işlemi kilitlendi |
| ContainerProcessTerminatedWithUnknownError | Bilinmeyen hata arda kapsayıcı işlemi |
| ContainerValidationFailed | BLOB kapsayıcı doğrulama Bu hata ile başarısız oldu: {0}. |
| DeleteWebServiceResourceFailed |  |
| ExceptionDeserializationError |  |
| FailedGettingApiDocument |  |
| FailedStoringWebService |  |
| InvalidMemoryConfiguration | InvalidMemoryConfiguration, ConfigValue: {0} |
| InvalidResourceCacheConfiguration |  |
| InvalidResourceDownloadConfiguration |  |
| InvalidWebServiceResources |  |
| MissingTaskInstance | Sağlanan bağımsız değişkenler. Doğrulayın. geçerli bağımsız değişkenler geçirilir ve yeniden deneyin. |
| ModelPackageInvalid |  |
| ModuleExecutionFailed |  |
| ModuleLoadFailed |  |
| ModuleObjectCloneFailed |  |
| OutputConversionFailed |  |
| PortDataTypeNotSupported | Bağlantı noktası kimliği = {0} desteklenmeyen bir veri türüne sahip: {1}. |
| ResourceDownload |  |
| ResourceLoadFailed |  |
| ServiceUrisNotFound |  |
| SwaggerGeneration | Swagger oluşturma başarısız oldu, Ayrıntılar: {0} |
| UnexpectedScoreStatus |  |
| UnknownBackendErrorResponse |  |
| Başvuruları |  |
| UnknownJobStatusCode | Bilinmeyen İş durum kodu: {0}. |
| UnknownModuleError |  |
| UpdateWebServiceResourceFailed |  |
| WebServiceGroupNotFound |  |
| WebServicePackageInvalid | InvalidWebServicePackage, Ayrıntılar: {0} |
| WorkerAuthorizationFailed |  |
| WorkerUnreachable |  |
 
## <a name="internalerrorsystemlowonmemory-http-status-code-500"></a>InternalErrorSystemLowOnMemory (HTTP durum kodu 500)
 
Yürütme bir iç hatayla karşılaştı. Sistem bellek yetersiz. Lütfen yeniden deneyin.
 
 
## <a name="modelpackageformaterror-http-status-code-500"></a>ModelPackageFormatError (HTTP status code 500)
 
Geçersiz model paketi. Sağlanan modeli paketi doğru olduğunu doğrulayıp yeniden deneyin.
 
 
## <a name="webservicepackageinternalerror-http-status-code-500"></a>WebServicePackageInternalError (HTTP durum kodu 500)
 
Geçersiz web hizmeti paketi. Sağlanan web paketinin doğru olduğundan emin olun ve yeniden deneyin.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| ModuleError | Web hizmeti paketi grafik geçersiz. Ayrıntılar: {0} |
 
## <a name="initializingcontainers-http-status-code-503"></a>InitializingContainers (HTTP durum kodu 503)
 
Kapsayıcılar başlatıldı olarak istek yürütülemiyor.
 
 
## <a name="serviceunavailable-http-status-code-503"></a>ServiceUnavailable (HTTP durum kodu 503)
 
Hizmet geçici olarak kullanılamıyor.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| NoMoreResources | İstek için kullanılabilir kaynaklar yoktur. |
| RequestThrottled | İstek {0} uç noktası için kısıtlanan. Uç nokta için en fazla eşzamanlı {1} ' dir. |
| TooManyConcurrentRequests | Çok sayıda eşzamanlı istek gönderildi. |
| TooManyHostsBeingInitialized | Çok fazla konak aynı anda başlatılmış. Azaltma / yeniden deneniyor göz önünde bulundurun. |
| TooManyHostsBeingInitializedPerModel | Çok fazla konak aynı anda başlatılmış. Azaltma / yeniden deneniyor göz önünde bulundurun. |
 
## <a name="gatewaytimeout-http-status-code-504"></a>GatewayTimeout (HTTP durum kodu 504)
 
İşlem izin verilen sürede tamamlanamadı.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| BackendInitializationTimeout | Web hizmeti başlatma izin verilen sürede tamamlanamadı. |
| BackendScoreTimeout | Web hizmeti isteği yürütülmesine izin verilen sürede tamamlanamadı. |
 
