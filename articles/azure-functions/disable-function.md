---
title: Azure işlevleri'nde işlevler devre dışı bırakma
description: Devre dışı bırakın ve Azure işlevleri'nde işlevleri etkinleştirme hakkında bilgi edinin 1.x ve 2.x'i.
services: functions
documentationcenter: ''
author: tdykstra
manager: cfowler
editor: ''
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
origin.date: 07/24/2018
ms.date: 08/31/2018
ms.author: v-junlch
ms.openlocfilehash: a32b4815a2716428ceeec034ddc5589e3aa062e8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60710576"
---
# <a name="how-to-disable-functions-in-azure-functions"></a>Azure işlevleri'nde işlevler devre dışı bırakma

Bu makalede, Azure işlevleri'nde bir işlev devre dışı bırakmak açıklanmaktadır. İçin *devre dışı* işlevi için tanımlı otomatik tetikleyici yoksay çalışma zamanı yapmak bir işlev anlamına gelir. Bunu yolu çalışma zamanı sürümü ve programlama diline bağlıdır:

- İşlevler 1.x
  - Komut dosyası dilleri
  - C# sınıf kitaplıkları
- İşlevler 2.x
  - Tüm diller için bir yolu
  - C# sınıf kitaplıkları için isteğe bağlı bir yol

## <a name="functions-1x---scripting-languages"></a>Komut dosyası dilleri 1.x - işlevleri

C# betiği ve JavaScript gibi komut dosyası dilleri için kullandığınız `disabled` özelliği *function.json* dosya olmayan bir işlev tetiklemek için çalışma zamanı söylemek için. Bu özellik ayarlanabilir `true` veya bir uygulama ayarının adı:

```json
{
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ],
    "disabled": true
}
```
or 

```json
    "bindings": [
        ...
    ],
    "disabled": "IS_DISABLED"
```

IS_DISABLED adlı ve ayarlamak bir uygulama ayarı olduğunda ikinci örnekte işlevi devre dışı bırakıldı `true` veya 1.

Azure portalı veya dosyayı düzenleyebilirsiniz **işlevi durumu** işlevin geçiş **Yönet** sekmesi. Portal anahtar çalışır değiştirerek *function.json* dosya.

![Durum geçiş işlevi](./media/disable-function/function-state-switch.png)

## <a name="functions-1x---c-class-libraries"></a>1.x - C# sınıf kitaplıkları olarak işlevleri

Kullandığınız işlevleri 1.x sınıf kitaplığında bir `Disable` tetiklenen bir işlev önlemek için özniteliği. Öznitelik oluşturucu parametresi olmadan, aşağıdaki örnekte gösterildiği gibi kullanabilirsiniz:

```csharp
public static class QueueFunctions
{
    [Disable]
    [FunctionName("QueueTrigger")]
    public static void QueueTrigger(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}
```

Öznitelik oluşturucu parametresi olmadan yeniden derleyin ve işlevin devre dışı durumunu değiştirmek için projenin yeniden gerektirir. Özniteliğini kullanmak için daha esnek bir şekilde, aşağıdaki örnekte gösterildiği gibi bir mantıksal uygulama ayarına başvuran bir oluşturucu parametresi dahil etmektir:

```csharp
public static class QueueFunctions
{
    [Disable("MY_TIMER_DISABLED")]
    [FunctionName("QueueTrigger")]
    public static void QueueTrigger(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}
```

Bu yöntem, etkinleştirin ve yeniden derlenmesi veya yeniden dağıtmaya gerek kalmadan uygulama ayarı değiştirerek işlevi devre dışı sağlar. Devre dışı bir durum değişikliği hemen tanınan biçimde bir uygulama ayarı değiştirmek, yeniden başlatmak, işlev uygulaması neden olur.

> [!IMPORTANT]
> `Disabled` Bir sınıf kitaplığı işlevi devre dışı bırakmak için tek yolu bir özniteliktir. Oluşturulan *function.json* doğrudan düzenlenecek bir sınıf kitaplığı işlevi değildir için dosya. Bu dosyayı düzenlerseniz, ne yapmanız için `disabled` özelliği herhangi bir etkisi olacaktır.
>
> Aynı gider **işlev durumu** açın **Yönet** sekmesinde değiştirerek çalıştığını beri *function.json* dosya.
>
> Ayrıca, portalın öyle işlevi devre dışı bırakılır gösterebilir unutmayın.



## <a name="functions-2x---all-languages"></a>2.x - tüm diller işlevleri

İşlevlerde devre dışı bir işlev uygulaması ayarı kullanarak 2.x. Örneğin, bir işlev devre dışı bırakmak için adlı `QueueTrigger`, adlı bir uygulama ayarı oluşturmak `AzureWebJobs.QueueTrigger.Disabled`ve `true`. Bu işlevi etkinleştirmek için uygulama ayarının `false`. Ayrıca **işlevi durumu** işlevin geçiş **Yönet** sekmesi. Anahtar oluşturma ve silme çalışır `AzureWebJobs.<functionname>.Disabled` uygulama ayarı.

![Durum geçiş işlevi](./media/disable-function/function-state-switch.png)

## <a name="functions-2x---c-class-libraries"></a>2.x - C# sınıf kitaplıkları olarak işlevleri

İşlevleri 2.x sınıf kitaplığında tüm diller için yöntemi kullanmanızı öneririz. Ancak isterseniz, [kullanımı devre dışı bırakma öznitelik olarak işlevleri 1.x](#functions-1x---c-class-libraries).

## <a name="next-steps"></a>Sonraki adımlar

Bu makale, otomatik tetikleyiciler devre dışı bırakma hakkında yöneliktir. Tetikleyiciler hakkında daha fazla bilgi için bkz. [Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md).

