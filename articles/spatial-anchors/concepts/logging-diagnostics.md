---
title: Günlüğe kaydetme ve Tanılama'da Azure uzamsal bağlayıcılarını | Microsoft Docs
description: Oluşturma ve günlüğe kaydetme ve Tanılama'da Azure uzamsal bağlayıcılarını almak ayrıntılı açıklaması.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: ramonarguelles
ms.date: 02/22/2019
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: d9377e2b5b66a7d426373a8a85e4880dafeaeee6
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2019
ms.locfileid: "56753288"
---
# <a name="logging-and-diagnostics-in-azure-spatial-anchors"></a>Günlüğe kaydetme ve Tanılama'da Azure uzamsal yer işaretleri

Azure uzamsal bağlayıcılarını uygulama geliştirme için kullanışlı bir standart günlük mekanizma sağlar. Ayrıca, yoktur tanılama günlük modunu yararlı daha fazla bilgi için hata ayıklama gerekli olduğunda. Tanılama günlüğünü ortamının görüntülerini depolamayı da içerir.

## <a name="standard-logging-in-azure-spatial-anchors"></a>Standart günlüğüne Azure uzamsal yer işaretleri
Azure uzamsal bağlayıcılarını API, uygulamalar için uygulama geliştirme için yararlı günlüklerini alma ve hata ayıklama için abone olabilirsiniz bir günlük mekanizma sağlar. Standart Günlük API'leri resimleri ortamın cihaz diske kalıcı yok. SDK, bu günlükleri Olay geri çağırmaları sağlar. Bu günlükleri uygulamanın günlüğü mekanizmasına tümleştirme size aittir.

### <a name="how-to-configure-the-log-messages"></a>Günlük iletilerini yapılandırma
Kullanıcı için ilgilenilen iki geri çağırmaları vardır. Aşağıdaki örnekte oturum yapılandırma görebilirsiniz.

```csharp
    cloudSpatialAnchorSession = new CloudSpatialAnchorSession();
    . . .
    // setup the log level for the runtime session
    cloudSpatialAnchorSession.LogLevel = SessionLogLevel.Information;

    // configure the callback for the debug log
    cloudSpatialAnchorSession.OnLogDebug += CloudSpatialAnchorSession_OnLogDebug;

    // configure the callback for the error log
    cloudSpatialAnchorSession.Error += CloudSpatialAnchorSession_Error;
```

### <a name="events--properties"></a>Olayları & Özellikleri

Olay Günlükleri ve oturumun hatalarını işlemek için sağlanan geri çağırmalar.

- [LogLevel](https://docs.microsoft.com/dotnet/api/microsoft.azure.spatialanchors.cloudspatialanchorsession.loglevel): Çalışma zamanını şuradan almak olaylar için ayrıntı düzeyini belirtir.
- [OnLogDebug](https://docs.microsoft.com/dotnet/api/microsoft.azure.spatialanchors.cloudspatialanchorsession.onlogdebug): Bu olay geri çağırma standart hata ayıklama günlüğü olayları sağlar.
- [Hata](https://docs.microsoft.com/dotnet/api/microsoft.azure.spatialanchors.cloudspatialanchorsession.error): Bu olay geri çağırma günlüğü olaylarını çalışma zamanı tarafından hata olarak kabul sağlar.

## <a name="diagnostics-logging-in-azure-spatial-anchors"></a>Azure uzamsal tutturucular günlüğe kaydetme tanılama

Ek olarak Standart mod için yukarıda açıklanan günlüğe kaydetme işleminin Azure uzamsal yer işaretleri, geliştiricilerin katılmayı seçebileceği bir tanı Modu'nu de vardır. Tanılama ortamının görüntülerini yakalar ve bunları diske kaydeder. Bu mod, tahmin edilebilir bir biçimde bir bağlantı bulabilir değilken sorunları gibi belirli türdeki hata ayıklama için kullanışlıdır. Yalnızca belirli bir sorunu yeniden oluşturun ve ardından devre dışı bırakmak için günlüğe kaydetme Tanılama'yı etkinleştirin. Uygulamalarınızı, normalde tanılamayı ile çalışmıyor.

Microsoft ile destek etkileşim sırasında daha fazla araştırma için bir tanılama paket Microsoft'a göndermek için bir Microsoft temsilcisinin isteyebilir. Bu durumda, tanılamayı etkinleştirme, sorunu yeniden oluşturun ve daha fazla bilgi için Microsoft tanılama paket göndermek isteyebilirsiniz. Bir Microsoft temsilcisi tarafından önceki bildirim Microsoft'a gönderilen tanılama günlükleri yanıtlanmamış geçer.

Aşağıdaki kod parçacıkları tanı Modu'nu nasıl etkinleştirileceği ve ayrıca nasıl Tanılama günlükleri Microsoft'a gönderebilirsiniz gösterir.

### <a name="enabling-diagnostics-logging"></a>Tanılama günlüğünü etkinleştirme

Bir oturum için tanılama günlüğünü etkinken oturumu üzerindeki tüm işlemler yerel dosya sisteminde oturum ilgili tanılama sahip olur. Günlük kaydı ortamının görüntülerini diske kaydedilirken içerir.

```csharp
private void ConfigureSession()
{
    cloudSpatialAnchorSession = new CloudSpatialAnchorSession();
    . . .

    // setup the log level for the runtime session
    cloudSpatialAnchorSession.LogLevel = SessionLogLevel.Information;

    // configure the callbacks for logging and errors
    cloudSpatialAnchorSession.OnLogDebug += CloudSpatialAnchorSession_OnLogDebug;
    cloudSpatialAnchorSession.Error += CloudSpatialAnchorSession_Error;

    // Opt-in to diagnostics logging of environment images.
    // If this is enabled, the diagnostics bundle will include images of the environment captured by the session
    cloudSpatialAnchorSession.Diagnostics.ImagesEnabled = true;

    // set the level of detail to be collected in the diagnostics log by the session
    cloudSpatialAnchorSession.Diagnostics.LogLevel = SessionLogLevel.All;

    // set the max bundle size to capture the bug information
    cloudSpatialAnchorSession.Diagnostics.MaxDiskSizeInMB = 200;
    . . .
}
```

### <a name="submitting-the-diagnostic-bundle"></a>Tanılama paket gönderme

Aşağıdaki kod parçacığı, bir tanılama paket Microsoft'a gönderme işlemi gösterilmektedir. Unutmayın, bu tanılama etkinleştirdikten sonra oturumu tarafından yakalanan ortamının görüntülerini içerir. Ayrıca, bir Microsoft temsilcisi tarafından önceki bildirim Microsoft'a gönderilen Tanılama paketleri yanıtlanmamış geçer.

```csharp
// method to handle the diagnostics bundle submission
private async Task CreateAndSubmitBundle()
{
    // create the diagnostics bundle manifest  to collect the session information
    string path = await cloudSpatialAnchorSession
                              .Diagnostics
                              .CreateManifestAsync("Description of the issue");

    // submit the manifest and data to send feedback to Microsoft
    await cloudSpatialAnchorSession.Diagnostics.SubmitManifestAsync(path);
}
```

### <a name="anatomy-of-the-diagnostics-bundle"></a>Tanılama paket anatomisi
Aşağıdaki bilgiler bir tanılama paket halinde mevcut olabilir:

- Anahtar kare görüntülerini - tanılama etkinleştirilmiş durumdayken oturumu sırasında yakalanmış ortamının görüntülerini.
- Günlükleri - çalışma zamanı tarafından kaydedilmiş günlük olayları.
- Oturum meta veriler - oturumunu tanımlayan meta verileri.
