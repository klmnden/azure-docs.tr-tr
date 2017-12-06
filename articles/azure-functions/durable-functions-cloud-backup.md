---
title: "Fan-dışarı/fan-arada senaryolarda dayanıklı işlevleri - Azure"
description: "Fan-dışarı fan-bileşenini senaryo için Azure işlevleri dayanıklı işlevleri uzantısı'nda uygulama hakkında bilgi edinin."
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: a5d539172f03246e3c658f2485d29d3ae389ae52
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="fan-outfan-in-scenario-in-durable-functions---cloud-backup-example"></a>Fan-dışarı/fan-arada senaryoda dayanıklı işlevleri - bulut yedekleme örneği

*Fan-dışarı/fan-arada* birden çok işlevleri eşzamanlı olarak yürütülen ve ardından bazı toplama sonuçlarına gerçekleştirme desenini başvuruyor. Bu makalede kullanan bir örneğin açıklanmaktadır [dayanıklı işlevleri](durable-functions-overview.md) fan-arada/fan kapatma senaryo uygulamak için. Örnek tümü veya bir uygulamanın site içeriğini bazıları Azure depolama alanına yedekler dayanıklı bir işlevdir.

## <a name="prerequisites"></a>Ön koşullar

* ' Ndaki yönergeleri izleyin [yükleme dayanıklı işlevleri](durable-functions-install.md) örneğini kurmak için.
* Bu makalede, zaten gitti varsayar [Hello dizisi](durable-functions-sequence.md) örnek gözden geçirme.

## <a name="scenario-overview"></a>Senaryoya genel bakış

Bu örnekte, İşlevler, blob depolama alanına belirtilen dizin yinelemeli olarak altındaki tüm dosyaları karşıya yükleyin. Bunlar ayrıca toplam yüklenen bayt sayısı.

Her şey mvc'deki tek bir işlevi yazma mümkündür. İçine çalışıyor ana sorun **ölçeklenebilirlik**. Bu tek VM üretilen işi tarafından üretilen işi sınırlandırılacak şekilde bir tek işlevi yürütme yalnızca tek bir VM üzerinde çalışabilir. Başka bir sorun **güvenilirlik**. Hata yarıda aracılığıyla ise veya tüm işlem 5 dakikadan daha uzun sürerse, yedekleme kısmen tamamlanmış bir durumunda başarısız olabilir. Ardından, yeniden başlatılması gerekir.

İki normal işlevleri yazmak için daha sağlam bir yaklaşım olacaktır: biri dosyaları numaralandırma dosya adları için bir kuyruk ekleyin ve başka bir kuyruktan okunmak ve blob depolama alanına dosyaları karşıya. Bu verimlilik ve güvenilirlik bakımından daha iyi olur, ancak sağlamak ve bir sıraya yönetmek gerektirir. Önemli karmaşıklık cinsinden daha da önemlisi, sunulan **durum yönetimi** ve **düzenleme** başka bir işlem yapmanız istiyorsanız, raporu gibi karşıya yüklenen toplam bayt sayısı.

Dayanıklı işlevleri bir yaklaşım, tüm çok düşük ek yükleri ile sözü edilen avantajları sunar.

## <a name="the-functions"></a>İşlevler

Bu makalede örnek uygulamasında aşağıdaki işlevleri açıklanmaktadır:

* `E2_BackupSiteContent`
* `E2_GetFileList`
* `E2_CopyFileToBlob`

Aşağıdaki bölümlerde, Azure portal geliştirme için kullanılan kod ve yapılandırma açıklanmaktadır. Visual Studio geliştirme için kod makalenin sonunda gösterilir.

## <a name="the-cloud-backup-orchestration-visual-studio-code-and-azure-portal-sample-code"></a>Bulut yedekleme düzenleme (Visual Studio Code ve Azure portal örnek kodu)

`E2_BackupSiteContent` İşlevini kullanan standart *function.json* orchestrator işlevler için.

[!code-json[Main](~/samples-durable-functions/samples/csx/E2_BackupSiteContent/function.json)]

Orchestrator işlevi uygulayan kod aşağıdaki gibidir:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E2_BackupSiteContent/run.csx)]

Bu orchestrator işlevi temelde şunları yapar:

1. Alan bir `rootDirectory` giriş parametresi olarak değeri.
2. Dosyaları altında özyinelemeli listesini almak için bir işlev çağrılarını `rootDirectory`.
3. Her dosyayı Azure Blob depolama alanına yüklemek için birden çok paralel işlevi çağrısı yapmaz.
4. Tamamlamak tüm yüklemeler için bekler.
5. Azure Blob Depolama birimine yüklenen Toplamı toplam bayt döndürür.

Bildirim `await Task.WhenAll(tasks);` satır. Tüm çağrıların `E2_CopyFileToBlob` işlevi olan *değil* beklemenin. Bu paralel olarak çalıştırılmasına izin vermeniz için bilinen bir durumdur. Bu görevler dizisi iletmek biz `Task.WhenAll`, Geri Al biz tamamlamak olmaz bir görevi *tüm kopyalama işlemleri tamamlanana kadar*. İle görev paralel kitaplığı (TPL) .NET içinde bilgi sahibi değilseniz, ardından bu size yeni değildir. Bu görevleri birden çok sanal makine aynı anda çalışması olasıdır ve dayanıklı işlevleri uzantısı uçtan uca yürütme işlem geri dönüştürme için dayanıklı olmasını sağlar farktır.

Gelen bekleyen sonra `Task.WhenAll`, tüm işlev çağrılarını tamamladınız ve değerleri bize geri döndürülen biliyoruz. Her çağrı `E2_CopyFileToBlob` bayt sayısı karşıya, sum toplam bayt sayısı hesaplama sağlasa da, bunlar tüm dönüş değerleri birlikte ekleme olacak şekilde döndürür.

## <a name="helper-activity-functions"></a>Yardımcı etkinlik işlevleri

Yardımcı etkinlik işlevleri başka bir örnek olduğu gibi kullandığınız yalnızca normal işlevlerdir `activityTrigger` tetiklemek bağlama. Örneğin, *function.json* dosya `E2_GetFileList` aşağıdaki gibi görünür:

[!code-json[Main](~/samples-durable-functions/samples/csx/E2_GetFileList/function.json)]

Ve uygulama şöyledir:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E2_GetFileList/run.csx)]

> [!NOTE]
> Neden henüz bu kodu doğrudan orchestrator işlevi koyduğunuz uygulanamadı merak ediyor olabilirsiniz. Olabilir, ancak bu bir orchestrator İşlevler, bunlar hiç dahil olmak üzere yerel dosya sistemi erişimi g/ç yapmalısınız olan temel kurallarının bölün.

*Function.json* dosya `E2_CopyFileToBlob` benzer şekilde basittir:

[!code-json[Main](~/samples-durable-functions/samples/csx/E2_CopyFileToBlob/function.json)]

Uygulama aynı zamanda oldukça basittir. Bazı kullanmak için Azure işlevleri bağlamaları özelliklerini Gelişmiş olur (diğer bir deyişle, kullanımını `Binder` parametresi), ancak bu kılavuzda amacıyla bu ayrıntıları hakkında endişelenmeniz gerekmez.

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E2_CopyFileToBlob/run.csx)]

Uygulama diskten dosyayı yükler ve zaman uyumsuz olarak "yedekleme" kapsayıcısında aynı ada sahip bir blob içine içeriği akışlarını. Dönüş değeri birleşik toplam hesaplamak için orchestrator işlevi tarafından sonra kullanılan depolama birimine kopyalanan bayt sayısıdır.

> [!NOTE]
> Bu bir g/ç işlemleri içine taşıma kusursuz örnektir bir `activityTrigger` işlevi. Yalnızca iş birçok farklı VM dağıtılabilir, ancak ayrıca denetim noktası oluşturma yararları ilerleme elde edersiniz. Ana bilgisayar işlemi için herhangi bir nedenle sonlandırıldı, hangi yüklemeleri daha önce tamamlamış bildirin.

## <a name="run-the-sample"></a>Örnek çalıştırın

Aşağıdaki HTTP POST isteği göndererek orchestration başlatabilirsiniz.

```
POST http://{host}/orchestrators/E2_BackupSiteContent
Content-Type: application/json
Content-Length: 20

"D:\\home\\LogFiles"
```

> [!NOTE]
> `HttpStart` Çağırdığınız işlevi yalnızca JSON biçimli içerik ile çalışır. Bu nedenle, `Content-Type: application/json` üstbilgi gereklidir ve dizin yolu bir JSON dizesi olarak kodlanır.

Bu HTTP isteği Tetikleyicileri `E2_BackupSiteContent` orchestrator ve dize geçirir `D:\home\LogFiles` bir parametre olarak. Yanıt, yedekleme işlemi durumunu almak için bir bağlantı sağlar:

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

(...trimmed...)
```

İşlevi uygulamanızda sahip kaç günlük dosyaları bağlı olarak, bu işlemin tamamlanması birkaç dakika sürebilir. En son durumunu URL'de sorgulayarak alabileceğiniz `Location` önceki HTTP 202 yanıtı üstbilgisi.

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

Bu durumda, işlevi hala çalışıyor. Orchestrator durumunu ve son güncellenme zamanı kaydedildi giriş görüyor. Kullanmaya devam edebilirsiniz `Location` tamamlanmasını yoklamak için üstbilgi değerleri. Durumu "tamamlandı"olduğunda, aşağıdakine benzer bir HTTP yanıt değeri bakın:

```
HTTP/1.1 200 OK
Content-Length: 152
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":"D:\\home\\LogFiles","output":452071,"createdTime":"2017-06-29T18:50:55Z","lastUpdatedTime":"2017-06-29T18:51:26Z"}
```

Şimdi orchestration tamamlandıktan ve yaklaşık ne kadar zaman tamamlamak için harcanan görebilirsiniz. İçin bir değer de gördüğünüz `output` yaklaşık 450 KB günlükleri karşıya gösterir alan.

## <a name="visual-studio-sample-code"></a>Visual Studio örnek kod

Tek bir C# dosyasında bir Visual Studio projesi olarak orchestration şöyledir:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/BackupSiteContent.cs)]

## <a name="next-steps"></a>Sonraki adımlar

Bu örnek, fan-dışarı/fan-arada desen uygulamak nasıl göstermiştir. Sonraki örnek nasıl uygulandığını gösterir [durum bilgisi olan tek](durable-functions-singletons.md) içinde desen bir [eternal orchestration](durable-functions-eternal-orchestrations.md).

> [!div class="nextstepaction"]
> [Durum bilgisi olan tek örnek çalıştırın](durable-functions-counter.md)
