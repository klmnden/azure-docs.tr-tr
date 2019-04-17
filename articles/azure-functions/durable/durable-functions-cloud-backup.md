---
title: Fan-dışarı/fan-içeren senaryolarda dayanıklı işlevler - Azure
description: Fan-dışarı-fan-gelen senaryo dayanıklı işlevler uzantısını Azure işlevleri için uygulamayı öğrenin.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 0bef5f1b64ec9f322070ba5c36cab138c7327da2
ms.sourcegitcommit: 5f348bf7d6cf8e074576c73055e17d7036982ddb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59608512"
---
# <a name="fan-outfan-in-scenario-in-durable-functions---cloud-backup-example"></a>Fan-dışarı/fan-arada senaryoda dayanıklı işlevler - bulut yedekleme örneği

*Fan-dışarı/fan-arada* birden çok işlevleri eşzamanlı olarak yürütülmesi ve ardından bazı toplama sonuçlarına gerçekleştiren deseni ifade eder. Bu makalede kullanan bir örnek açıklanmaktadır [dayanıklı işlevler](durable-functions-overview.md) bir fan-arada/fan-dışarı senaryoyu uygulamak için. Örnek tümü veya bir uygulamanın site içeriği bazıları, Azure Depolama'ya yedekler kalıcı bir işlevdir.

[!INCLUDE [durable-functions-prerequisites](../../../includes/durable-functions-prerequisites.md)]

## <a name="scenario-overview"></a>Senaryoya genel bakış

Bu örnekte, işlevleri belirtilen dizin yinelemeli olarak altındaki tüm dosyaları blob depolama alanına yükleyin. Bunlar ayrıca toplam yüklenen bayt sayısı.

Her şey üstlenir tek bir işlevi yazmak mümkündür. İçine çalıştırmak ana sorun **ölçeklenebilirlik**. Aktarım hızı, tek bir VM aktarım hızı tarafından sınırlandırılacak şekilde tek bir işlev yürütmeye yalnızca tek bir VM üzerinde çalışır. Başka bir sorun **güvenilirlik**. Yedekleme, bir hata sürecin yarısında ise veya sürecinin tamamını 5 dakikadan uzun sürerse, yalnızca kısmen tamamlanmış bir durumda başarısız olabilir. Ardından, yeniden başlatılması gerekir.

Daha güçlü bir yaklaşım iki normal işlev yazmaktır: ve dosyaları numaralandırma dosya adları için bir kuyruk ekleyin ve başka bir kuyruktan okunmak ve dosyaları blob depolama alanına yükleme. Performans ve güvenilirlik açısından daha iyi budur ancak sağlamak ve bir kuyruk yönetmenizi gerektirir. Önemli karmaşıklık açısından daha da önemlisi, sunulan **durum yönetimi** ve **koordinasyon** fazla bir şey istiyorsanız, toplam bayt sayısı gibi rapor karşıya.

Dayanıklı işlevler bir yaklaşım, tüm çok az bir Giderle bahsedilen avantajları sunar.

## <a name="the-functions"></a>İşlevleri

Bu makalede örnek uygulama aşağıdaki işlevler açıklanmaktadır:

* `E2_BackupSiteContent`
* `E2_GetFileList`
* `E2_CopyFileToBlob`

Aşağıdaki bölümlerde, kullanılan kod ve yapılandırma açıklanmaktadır. C# betik oluşturma. Visual Studio geliştirme için kod makalenin sonunda gösterilir.

## <a name="the-cloud-backup-orchestration-visual-studio-code-and-azure-portal-sample-code"></a>Bulut yedekleme düzenleme (Visual Studio Code ve Azure portalı örnek kodu)

`E2_BackupSiteContent` İşlevini kullanan standart *function.json* orchestrator işlevleri için.

[!code-json[Main](~/samples-durable-functions/samples/csx/E2_BackupSiteContent/function.json)]

Orchestrator işlevi uygulayan kod şu şekildedir:

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E2_BackupSiteContent/run.csx)]

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E2_BackupSiteContent/index.js)]

Bu orchestrator işlevi aslında şunları yapar:

1. Alan bir `rootDirectory` giriş parametresi olarak değeri.
2. Altındaki dosyaları bir özyinelemeli listesini almak için bir işlev çağırır `rootDirectory`.
3. Azure Blob depolama alanına her dosyayı karşıya yüklemek için birden çok paralel işlev çağrıları yapar.
4. Tamamlamak tüm karşıya yüklemelerin tamamlanmasını bekler.
5. Azure Blob depolama alanına yüklenen Toplamı toplam bayt döndürür.

Bildirim `await Task.WhenAll(tasks);` (C#) ve `yield context.df.Task.all(tasks);` (JavaScript) satır. Tüm Bireysel çağrılar `E2_CopyFileToBlob` işlevi olan *değil* bekleniyor. Bu, paralel olarak çalıştırmak izin vermek üzere kasıtlıdır. Bu görevler dizisi iletmek biz `Task.WhenAll` (C#) veya `context.df.Task.all` (JavaScript) aldığımız geri tamamlaması gerekmez görev *kopyalama işlemleri tamamlanana kadar*. Tanıdık ile görev paralel kitaplığı (TPL), .NET veya [ `Promise.all` ](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) JavaScript'te, ardından bu sizin için yeni değildir. Bu görevleri birden çok VM'de eşzamanlı çalışıyor olabilir ve dayanıklı işlevler uzantısını uçtan uca yürütme işlem geri dönüştürme için dayanıklı olmasını sağlar farktır.

> [!NOTE]
> Görevler için JavaScript gösterir kavramsal açıdan benzer olsa da, orchestrator işlevleri kullanmalıdır `context.df.Task.all` ve `context.df.Task.any` yerine `Promise.all` ve `Promise.race` görev paralelleştirme yönetmek için.

Bekleyen öğesinden sonra `Task.WhenAll` (veya gelen sonuçlanmıyor `context.df.Task.all`), tüm işlev çağrıları tamamladınız ve değerleri bize geri döndürülen biliyoruz. Her çağrı `E2_CopyFileToBlob` bayt sayısı karşıya Toplamı toplam bayt sayısı hesaplama sağlasa da bu tüm dönüş değerleri birbirine ekleme, bu nedenle döndürür.

## <a name="helper-activity-functions"></a>Yardımcı etkinlik işlevleri

Yardımcı etkinlik işlevleri ile diğer örnekleri olduğu gibi kullanan yalnızca normal işlevlerdir `activityTrigger` bağlama tetikleyin. Örneğin, *function.json* dosya `E2_GetFileList` aşağıdaki gibi görünür:

[!code-json[Main](~/samples-durable-functions/samples/csx/E2_GetFileList/function.json)]

Ve uygulama şu şekildedir:

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E2_GetFileList/run.csx)]

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E2_GetFileList/index.js)]

JavaScript uygulamasını `E2_GetFileList` kullanan `readdirp` yinelemeli olarak modülüne dizin yapısı okuyun.

> [!NOTE]
> Neden yalnızca bu kodu doğrudan Düzenleyici işlevi yerleştirdiğiniz uygulanamadı merak ediyor olabilirsiniz. Yapabilirsiniz, ancak bunlar hiçbir zaman dahil olmak üzere yerel dosya sistemi erişimini g/ç yapmalısınız olan orchestrator işlevlerin temel kurallarından biri bu kesme.

*Function.json* dosya `E2_CopyFileToBlob` benzer şekilde basittir:

[!code-json[Main](~/samples-durable-functions/samples/csx/E2_CopyFileToBlob/function.json)]

C# uygulaması da oldukça kolaydır. Bazı kullanmak için Azure işlevleri bağlamaları özelliklerinin Gelişmiş olur (diğer bir deyişle, kullanımını `Binder` parametresi), ancak bu gözden geçirme amacıyla bu ayrıntıları hakkında endişelenmeniz gerekmez.

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E2_CopyFileToBlob/run.csx)]

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

JavaScript uygulamasını erişimi yok `Binder` özelliği, Azure işlevleri, böylece [düğüm için Azure depolama SDK'sı](https://github.com/Azure/azure-storage-node) yerini alır.

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E2_CopyFileToBlob/index.js)]

Uygulama, dosya diskten yükler ve zaman uyumsuz olarak "yedekleme" kapsayıcı içinde aynı adlı bir blob içinde içeriği akışları. Dönüş değeri, ardından Düzenleyici işlevi tarafından toplama toplamı hesaplamak için kullanılan depolama alanına kopyalandığından bayt sayısıdır.

> [!NOTE]
> Bu bir g/ç işlemi taşıma mükemmel örnektir bir `activityTrigger` işlevi. Yalnızca iş birçok farklı sanal makinelerde dağıtılabilir, ancak ilerleme ayrıca denetim noktası avantajlarından yararlanın. Ana bilgisayar işlemi için herhangi bir nedenle sonlandırıldıysa, hangi karşıya yükleme zaten tamamlandı bildirin.

## <a name="run-the-sample"></a>Örneği çalıştırma

Aşağıdaki HTTP POST isteği göndererek düzenleme başlayabilirsiniz.

```
POST http://{host}/orchestrators/E2_BackupSiteContent
Content-Type: application/json
Content-Length: 20

"D:\\home\\LogFiles"
```

> [!NOTE]
> `HttpStart` Çağırdığınız işlev, yalnızca JSON biçimli içerik ile çalışır. Bu nedenle, `Content-Type: application/json` üst bilgisi gereklidir ve dizin yolu bir JSON dizesi kodlanır. Ayrıca, HTTP kod parçacığı bir giriş olduğunu varsayar `host.json` varsayılan kaldıran dosya `api/` tüm HTTP tetikleyicisi işlevlerini URL'lerden öneki. Bu yapılandırma için işaretleme bulabilirsiniz `host.json` örnekleri dosyasında.

Bu HTTP isteği Tetikleyicileri `E2_BackupSiteContent` orchestrator ve dize `D:\home\LogFiles` bir parametre olarak. Yanıt, yedekleme işlemi durumunu almak için bir bağlantı sağlar:

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

(...trimmed...)
```

İşlev uygulamanızda sahip olduğunuz kaç günlük dosyalarının bağlı olarak, bu işlemin tamamlanması birkaç dakika sürebilir. En son durumu URL'de sorgulayarak alabileceğiniz `Location` önceki HTTP 202 yanıt üstbilgisi.

```
GET http://{host}/admin/extensions/DurableTaskExtension/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```

```
HTTP/1.1 202 Accepted
Content-Length: 148
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

{"runtimeStatus":"Running","input":"D:\\home\\LogFiles","output":null,"createdTime":"2017-06-29T18:50:55Z","lastUpdatedTime":"2017-06-29T18:51:16Z"}
```

Bu durumda, işlev hala çalışıyor. Orchestrator durumunu ve son güncelleştirme zamanı kaydedildi girişi görebilirsiniz. Kullanmaya devam edebilirsiniz `Location` tamamlanmasını yoklamak için üstbilgi değerleri. Durumu tamamlandı""olduğunda, aşağıdakine benzer bir HTTP yanıt değeri bakın:

```
HTTP/1.1 200 OK
Content-Length: 152
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":"D:\\home\\LogFiles","output":452071,"createdTime":"2017-06-29T18:50:55Z","lastUpdatedTime":"2017-06-29T18:51:26Z"}
```

Artık düzenleme tamamlandıktan ve tamamlanması yaklaşık olarak ne kadar zaman geçen görebilirsiniz. İçin bir değer de gördüğünüz `output` alanı gösterir, yaklaşık 450 KB günlükleri karşıya yüklendi.

## <a name="visual-studio-sample-code"></a>Visual Studio örnek kod

Tek bir C# dosyası olarak Visual Studio projesi içinde düzenleme şu şekildedir:

> [!NOTE]
> Yüklemek ihtiyacınız olacak `Microsoft.Azure.WebJobs.Extensions.Storage` aşağıdaki örnek kodu çalıştırmak için Nuget paketi.

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/BackupSiteContent.cs)]

## <a name="next-steps"></a>Sonraki adımlar

Bu örnek fan-dışarı/fan-arada düzeni nasıl uygulayacağınıza karar göstermiştir. Sonraki örnek, İzleyicisi'ni kullanarak desen uygulamak gösterilmektedir [dayanıklı zamanlayıcılar](durable-functions-timers.md).

> [!div class="nextstepaction"]
> [İzleyici örneği çalıştırma](durable-functions-monitor.md)
