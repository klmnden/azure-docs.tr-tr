---
title: Günlüğe kaydetme ve Tanılama'da Azure uzamsal bağlayıcılarını | Microsoft Docs
description: Oluşturma ve günlüğe kaydetme ve Tanılama'da Azure uzamsal bağlayıcılarını almak ayrıntılı açıklaması.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/22/2019
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: b66dc7d6ec9d11fe645587fe791824009231b7c2
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65964749"
---
# <a name="logging-and-diagnostics-in-azure-spatial-anchors"></a>Günlüğe kaydetme ve Tanılama'da Azure uzamsal yer işaretleri

Azure uzamsal bağlayıcılarını uygulama geliştirme için kullanışlı olan bir standart günlük mekanizma sağlar. Uzamsal Çıpalarını tanılama günlük modu, hata ayıklama için daha fazla bilgi gerektiğinde faydalıdır. Tanılama günlüğünü ortamının görüntülerini depolar.

## <a name="standard-logging"></a>Standart günlüğe kaydetme
Uzamsal bağlayıcılarını API'SİNDE günlük mekanizmasına, uygulama geliştirme ve hata ayıklama için yararlı günlüklerini almak için abone olabilirsiniz. Standart Günlük API'leri yok depolamak ortamın resimleri cihaz diskte. SDK, bu günlükleri Olay geri çağırmaları sağlar. Bu günlükleri uygulamanın günlüğü mekanizmasına tümleştirme size aittir.

### <a name="configuration-of-log-messages"></a>Günlük iletilerini yapılandırması
Kullanıcı için ilgilenilen iki geri çağırmaları vardır. Aşağıdaki örnek, oturum yapılandırma işlemi gösterilmektedir.

```csharp
    cloudSpatialAnchorSession = new CloudSpatialAnchorSession();
    . . .
    // set up the log level for the runtime session
    cloudSpatialAnchorSession.LogLevel = SessionLogLevel.Information;

    // configure the callback for the debug log
    cloudSpatialAnchorSession.OnLogDebug += CloudSpatialAnchorSession_OnLogDebug;

    // configure the callback for the error log
    cloudSpatialAnchorSession.Error += CloudSpatialAnchorSession_Error;
```

### <a name="events-and-properties"></a>Olayları ve özellikleri

Bu olay geri çağırmaları günlüklerini ve oturum hatalarını işlemek için sağlanır:

- [LogLevel](https://docs.microsoft.com/dotnet/api/microsoft.azure.spatialanchors.cloudspatialanchorsession.loglevel): Çalışma zamanını şuradan almak olaylar için ayrıntı düzeyini belirtir.
- [OnLogDebug](https://docs.microsoft.com/dotnet/api/microsoft.azure.spatialanchors.cloudspatialanchorsession.onlogdebug): Standart hata ayıklama günlüğü olayları sağlar.
- [Hata](https://docs.microsoft.com/dotnet/api/microsoft.azure.spatialanchors.cloudspatialanchorsession.error): Çalışma zamanı hataları olmasını dikkate günlük olayları sağlar.

## <a name="diagnostics-logging"></a>Tanılama günlükleri

Ek olarak Standart mod için günlüğe kaydetme işleminin kayma bağlayıcılarını tanı Modu'nu de vardır. Tanı Modu'nu ortamının görüntülerini yakalar ve bunları diske kaydeder. Bu mod, sorunların, tahmin edilebilir bir biçimde bir bağlantı bulunamadı hatası gibi belirli türdeki hata ayıklamak için kullanabilirsiniz. Yalnızca belirli bir sorunu yeniden oluşturmak için günlüğe kaydetme Tanılama'yı etkinleştirin. Ardından bunu devre dışı. Uygulamalarınızı normalde çalıştırırken tanılama etkinleştirmeyin.

Microsoft ile destek etkileşim sırasında daha fazla araştırma için bir tanılama paket göndermek için bir Microsoft temsilcisinin isteyebilir. Bu durumda, tanılamayı etkinleştirin ve tanılama paket gönderebilir. Bu nedenle, sorunu yeniden oluşturmak isteyebilirsiniz. 

Microsoft'a tanılama günlüğü bir Microsoft temsilcisinin önceki bildirim gönderirseniz gönderim yanıtlanmamış geçer.

Aşağıdaki bölümlerde, tanı Modu'nu etkinleştirme ve tanılama günlükleri Microsoft'a göndermek nasıl gösterir.

### <a name="enable-diagnostics-logging"></a>Tanılama günlüğünü etkinleştirme

Oturum açmak için tanılama günlüğünü etkinleştirme oturumdaki tüm işlemleri ilgili tanılama günlük kaydı yerel dosya sisteminde vardır. Günlük kaydı sırasında ortamının görüntülerini diske kaydedilir.

```csharp
private void ConfigureSession()
{
    cloudSpatialAnchorSession = new CloudSpatialAnchorSession();
    . . .

    // set up the log level for the runtime session
    cloudSpatialAnchorSession.LogLevel = SessionLogLevel.Information;

    // configure the callbacks for logging and errors
    cloudSpatialAnchorSession.OnLogDebug += CloudSpatialAnchorSession_OnLogDebug;
    cloudSpatialAnchorSession.Error += CloudSpatialAnchorSession_Error;

    // opt in to diagnostics logging of environment images
    // if this is enabled, the diagnostics bundle includes images of the environment captured by the session
    cloudSpatialAnchorSession.Diagnostics.ImagesEnabled = true;

    // set the level of detail to be collected in the diagnostics log by the session
    cloudSpatialAnchorSession.Diagnostics.LogLevel = SessionLogLevel.All;

    // set the max bundle size to capture the bug information
    cloudSpatialAnchorSession.Diagnostics.MaxDiskSizeInMB = 200;
    . . .
}
```

### <a name="submit-the-diagnostics-bundle"></a>Tanılama paket gönderin

Aşağıdaki kod parçacığı, bir tanılama paket Microsoft'a gönderme işlemi gösterilmektedir. Bu paket tanılama etkinleştirdikten sonra oturumu tarafından yakalanan ortamının görüntülerini içerir. 

```csharp
// method to handle the diagnostics bundle submission
private async Task CreateAndSubmitBundle()
{
    // create the diagnostics bundle manifest to collect the session information
    string path = await cloudSpatialAnchorSession
                              .Diagnostics
                              .CreateManifestAsync("Description of the issue");

    // submit the manifest and data to send feedback to Microsoft
    await cloudSpatialAnchorSession.Diagnostics.SubmitManifestAsync(path);
}
```

### <a name="parts-of-a-diagnostics-bundle"></a>Tanılama paket bölümleri
Tanılama paket, aşağıdaki bilgileri içerebilir:

- **Anahtar kare görüntülerini**: Tanılama etkinleştirilmiş durumdayken oturumu sırasında yakalanan ortam görüntüler.
- **Günlükleri**: Çalışma zamanı tarafından kaydedilen olayları günlüğe kaydedin.
- **Oturum meta verileri**: Oturumunu tanımlayan meta verileri.
