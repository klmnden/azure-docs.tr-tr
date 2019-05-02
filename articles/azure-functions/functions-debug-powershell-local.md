---
title: PowerShell Azure işlevleri yerel olarak hata ayıklama
description: PowerShell kullanarak işlevleri geliştirme hakkında bilgi edinin.
services: functions
documentationcenter: na
author: tylerleonhardt
manager: jeconnoc
ms.service: azure-functions
ms.devlang: powershell
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: tyleonha, glenga
ms.openlocfilehash: 554b7b7f401ec7cdb1ae08839550b81d797764f2
ms.sourcegitcommit: 111a7b3e19d5515ce7036287cea00a7204ca8b56
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2019
ms.locfileid: "64530339"
---
# <a name="debug-powershell-azure-functions-locally"></a>PowerShell Azure işlevleri yerel olarak hata ayıklama

Azure işlevleri, işlevlerinizi olarak PowerShell betikleri geliştirmenize olanak tanır.

[!INCLUDE [functions-powershell-preview-note](../../includes/functions-powershell-preview-note.md)]

Aşağıdaki standart geliştirme araçlarını kullanarak tüm PowerShell betikleri gibi PowerShell işlevlerinizi yerel olarak hata ayıklaması yapabilirsiniz:

* [Visual Studio Code'u](https://code.visualstudio.com/): Microsoft'un ücretsiz, hafif ve açık kaynak metin düzenleyicisi PowerShell uzantılı bir tam PowerShell geliştirme deneyimi sunar.
* Bir PowerShell konsolu: Diğer bir PowerShell işlemde hata ayıklamak için kullanacağınız aynı komutları kullanarak hata ayıklama.

[Azure işlevleri temel araçları](functions-run-local.md) yerel PowerShell işlevleri dahil olmak üzere Azure işlevleri hata ayıklamayı destekler.

## <a name="example-function-app"></a>Örnek işlev uygulaması

Bu makalede kullanılan işlev uygulaması, tek bir HTTP ile tetiklenen işlevi vardır ve aşağıdaki dosyaları vardır:

```
PSFunctionApp
 | - HttpTriggerFunction
 | | - run.ps1
 | | - function.json
 | - local.settings.json
 | - host.json
 | - profile.ps1
```

Bu işlev uygulamasını alma tamamladığınızda gibi [PowerShell Hızlı Başlangıç](functions-create-first-function-powershell.md).

İşlev kodu `run.ps1` kodun aşağıdaki gibi görünür:

```powershell
param($Request)

$name = $Request.Query.Name

if($name) {
    $status = 200
    $body = "Hello $name"
}
else {
    $status = 400
    $body = "Please pass a name on the query string or in the request body."
}

Push-OutputBinding -Name Response -Value ([HttpResponseContext]@{
    StatusCode = $status
    Body = $body
})
```

## <a name="set-the-attach-point"></a>İliştirme noktası ayarlayın

Herhangi bir PowerShell işlevi hata ayıklamak için işlev eklenmesi hata ayıklayıcının durdurulması gerekiyor. `Wait-Debugger` Cmdlet yürütmeyi durdurur ve hata ayıklayıcı için bekler.

Tek yapmak için ihtiyacınız olan bir çağrı ekleyin `Wait-Debugger` cmdlet'i yukarıdaki `if` aşağıdaki gibi bir deyimi:

```powershell
param($Request)

$name = $Request.Query.Name

# This is where we will wait for the debugger to attach
Wait-Debugger

if($name) {
    $status = 200
    $body = "Hello $name"
}
# ...
```

Hata ayıklama başladığında, `if` deyimi. 

İle `Wait-Debugger` yerine, artık Visual Studio Code veya bir PowerShell Konsolu kullanarak işlevleri ayıklayabilirsiniz.

## <a name="debug-in-visual-studio-code"></a>Visual Studio Code'da Hata Ayıkla

PowerShell işlevlerinizi Visual Studio code'da hata ayıklamak için aşağıdaki uzantılar Visual Studio Code için aşağıdakiler gereklidir:

* [PowerShell](/powershell/scripting/components/Visual Studio Code/using-Visual Studio Code)
* [Azure İşlevleri](functions-create-first-function-vs-code.md)

PowerShell ve Azure işlevleri Uzantıları yükledikten sonra var olan bir işlev uygulaması projesi yükleyin. Ayrıca [işlevler projesi oluşturma](functions-create-first-function-vs-code.md).

>[!NOTE]
> Projenize gerekli yapılandırma dosyalarını olmamalıdır, bunları eklemeniz istenir.

### <a name="start-the-function-app"></a>İşlev uygulamasını Başlat

Doğrulayın `Wait-Debugger` işlevde hata ayıklayıcıyı iliştirmek istediğiniz ayarlanır.  İle `Wait-Debugger` eklendi, işlev uygulamanızı Visual Studio Code kullanarak ayıklayabilirsiniz.

Seçin **hata ayıklama** bölmesi ve ardından **eklemek için PowerShell işlevi**.

![Hata ayıklayıcı](https://user-images.githubusercontent.com/2644648/56166073-8a7b3780-5f89-11e9-85ce-36ed38e221a2.png)

Ayrıca, hata ayıklamayı başlatmak için F5 tuşuna basabilirsiniz.

İşlem hata ayıklama başlangıç aşağıdaki görevleri gerçekleştirir:

* Çalıştırmaları `func extensions install` işlev uygulamanızın gerektirdiği herhangi bir Azure işlevleri uzantıları yüklemek için terminalde.
* Çalıştırmaları `func host start` işlevleri ana işlev uygulamasını başlatmak için terminalde.
* İşlevler çalışma zamanı içinde bir PowerShell çalışma PowerShell hata ayıklayıcı ekleyin.

İşlev uygulaması çalıştıran, HTTP ile tetiklenen işlevi çağırmak için ayrı bir PowerShell Konsolu gerekir.

Bu durumda, PowerShell konsolunu istemcisidir. `Invoke-RestMethod` İşlev tetiklemek için kullanılır.

Bir PowerShell konsolunda aşağıdaki komutu çalıştırın:

```powershell
Invoke-RestMethod "http://localhost:7071/api/HttpTrigger?Name=Functions"
```

Bir yanıt hemen döndürülmez fark edeceksiniz. Çünkü `Wait-Debugger` hata ayıklayıcı ve PowerShell eklenmiş yürütme, verebilir hemen sonra kesme moduna geçti. Bu nedenle, [BreakAll kavramı](#breakall-might-cause-your-debugger-to-break-in-an-unexpected-place), hangi daha sonra açıklanmıştır. Tuşuna bastıktan sonra `continue` düğmesi, hata ayıklayıcı artık keser satırında sonra doğru `Wait-Debugger`.

Bu noktada, hata ayıklayıcı ve tüm normal hata ayıklayıcı işlemleri yapabilirsiniz. Visual Studio Code'da hata ayıklayıcıyı kullanma ile ilgili daha fazla bilgi için bkz: [resmi belgelerine](https://code.visualstudio.com/Docs/editor/debugging#_debug-actions).

Devam etmek ve tam olarak betiğinizi çağırma sonra olduğunu fark edeceksiniz:

* Yaptığınız PowerShell konsolunu `Invoke-RestMethod` bir sonuç döndürdü
* Visual Studio Code tümleşik PowerShell konsolunda yürütülecek bir betik için bekliyor

Sonraki kez aynı işlevi, hata ayıklayıcıda uzantısı keser sonra doğru PowerShell çağırdığınızda `Wait-Debugger`.

## <a name="debugging-in-a-powershell-console"></a>Bir PowerShell konsolunda hata ayıklama

>[!NOTE]
> Bu bölümde okuduğunuz varsayılır [Azure işlevleri çekirdek araçları docs](functions-run-local.md) ve nasıl kullanılacağını `func host start` işlev uygulamanızı başlatacak komutu.

Bir konsol açın `cd` dizinine işlev uygulaması ve şu komutu çalıştırın:

```sh
func host start
```

Çalışan işlev uygulamasını ve `Wait-Debugger` yerinde işleme iliştirilebilir. Daha fazla PowerShell adlandırılan iki gerekir.

Konsollarının bir istemci olarak davranır. Bu, çağrı `Invoke-RestMethod` işlevi tetikleyebilirsiniz. Örneğin, aşağıdaki komutu çalıştırabilirsiniz:

```powershell
Invoke-RestMethod "http://localhost:7071/api/HttpTrigger?Name=Functions"
```

Bir yanıt bir sonuç döndürmez fark edeceksiniz, `Wait-Debugger`. PowerShell çalışma alanı artık eklenmiş bir hata ayıklayıcı için bekliyor. Bağlı geçelim.

Diğer PowerShell konsolunda aşağıdaki komutu çalıştırın:

```powershell
Get-PSHostProcessInfo
```

Bu cmdlet, aşağıdaki çıktıyı gibi görünen bir tablo döndürür:

```output
ProcessName ProcessId AppDomainName
----------- --------- -------------
dotnet          49988 None
pwsh            43796 None
pwsh            49970 None
pwsh             3533 None
pwsh            79544 None
pwsh            34881 None
pwsh            32071 None
pwsh            88785 None
```

Not `ProcessId` olan tablo öğesi için `ProcessName` olarak `dotnet`. İşlev uygulamanızı işlemidir.

Ardından, aşağıdaki kod parçacığını çalıştırın:

```powershell
# This enters into the the Azure Functions PowerShell process.
# Put your value of `ProcessId` here.
Enter-PSHostProcess -Id $ProcessId

# This triggers the debugger.
Debug-Runspace 1
```

Başladıktan sonra hata ayıklayıcı keser ve aşağıdaki çıktıyı gibi gösterilir:

```
Debugging Runspace: Runspace1

To end the debugging session type the 'Detach' command at the debugger prompt, or type 'Ctrl+C' otherwise.

At /Path/To/PSFunctionApp/HttpTriggerFunction/run.ps1:13 char:1
+ if($name) { ...
+ ~~~~~~~~~~~
[DBG]: [Process:49988]: [Runspace1]: PS /Path/To/PSFunctionApp>>
```

Bu noktada, içinde bir kesme noktasında durduruldu [PowerShell hata ayıklayıcı](/powershell/module/microsoft.powershell.core/about/about_debuggers). Burada, tüm normal hata ayıklama işlemlerini yapın, Atla, Adımlama, devam etmek, Çık ve diğerleri. Hata ayıklama komutları konsolda kullanılabilir kümesinin tamamını görmek için şunu çalıştırın `h` veya `?` komutları.

Bu düzeyde ile kesme noktaları ayarlayabilirsiniz `Set-PSBreakpoint` cmdlet'i.

Devam etmek ve tam olarak betiğinizi çağırma sonra olduğunu fark edeceksiniz:

* Yürütüldüğü, PowerShell konsolunu `Invoke-RestMethod` artık bir sonuç döndürdü.
* Yürütüldüğü, PowerShell konsolunu `Debug-Runspace` yürütülecek bir betik için bekliyor.

Aynı işlevi yeniden çağırın (kullanarak `Invoke-RestMethod` gibi) ve sonra sağ hata ayıklayıcı keser `Wait-Debugger` komutu.

## <a name="considerations-for-debugging"></a>Hata ayıklama için dikkat edilmesi gerekenler

Aşağıdaki sorunlar, İşlevler kodu hata ayıklamasında unutmayın.

### <a name="breakall-might-cause-your-debugger-to-break-in-an-unexpected-place"></a>`BreakAll` beklenmeyen bir yerde ayırmak, hata ayıklayıcı neden olabilir

PowerShell uzantısı kullandığı `Debug-Runspace`, hangi sırayla kullanır PowerShell'in üzerinde `BreakAll` özelliği. Bu özellik, yürütülen ilk komut durdurmak için PowerShell bildirir. Bu davranışı, hatası ayıklanmış bir çalışma içinde kesme noktaları ayarlama fırsatı sunar.

Azure işlevleri çalışma zamanı birkaç komut aslında çağırmadan önce çalışır, `run.ps1` betik hata ayıklayıcı kesme içinde yukarı sona ereceğini mümkündür `Microsoft.Azure.Functions.PowerShellWorker.psm1` veya `Microsoft.Azure.Functions.PowerShellWorker.psd1`.

Bu kesme gerçekleştirileceği, çalıştırma `continue` veya `c` Bu Kesme noktasının atlamak için komutu. Ardından beklenen bir kesme noktasında Durdur

## <a name="next-steps"></a>Sonraki adımlar

PowerShell kullanarak işlevleri geliştirme hakkında daha fazla bilgi edinmek için [Azure işlevleri PowerShell Geliştirici kılavuzunda](functions-reference-powershell.md).
