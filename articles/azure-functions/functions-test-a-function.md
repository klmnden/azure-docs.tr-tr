---
title: Azure İşlevlerini test etme
description: Otomatik testler için oluşturma bir C# işlevi Visual Studio ve VS code'da JavaScript işlevi
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
keywords: Azure işlevleri, İşlevler, olay işleme, Web kancaları, dinamik işlem, sunucusuz mimari, test etme
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 030/25/2019
ms.author: cshoe
ms.openlocfilehash: 4b3cba7e7656ea13a6e7b36be4cb2fef99893867
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58439337"
---
# <a name="strategies-for-testing-your-code-in-azure-functions"></a>Kodunuzu Azure işlevleri'nde test stratejileri

Bu makalede, Azure işlevleri için otomatik testler oluşturma işlemini gösterir. 

Tüm kod sınama önerilir, ancak bir işlevin logic kaydırma ve işlev dışındaki testler oluşturma en iyi sonuçlar elde edebilirsiniz. Mantıksal hemen özetleyen bir işlevin satır kod sınırlar ve işlevi diğer sınıfları veya modülleri çağırmak için sorumlu sağlar. Bu makalede, ancak otomatik testler karşı bir HTTP ve Zamanlayıcı ile tetiklenen işlev oluşturma işlemini gösterir.

Aşağıdaki içeriği hedef farklı dilleri ve ortamları yönelik iki farklı bölümlere ayrılır. Testleri oluşturmayı öğrenebilirsiniz:

- [C#xUnit ile Visual Studio](#c-in-visual-studio)
- [VS code'da Jest ile JavaScript](#javascript-in-vs-code)

Örnek depoyu kullanılabilir [GitHub](https://github.com/Azure-Samples/azure-functions-tests).

## <a name="c-in-visual-studio"></a>C#Visual Studio'da
Aşağıdaki örnek nasıl oluşturulacağını açıklar bir C# işlev uygulamanız Visual Studio'da çalıştırma ve test [xUnit](https://xunit.github.io).

![İle Azure işlevlerini test etme C# Visual Studio'da](./media/functions-test-a-function/azure-functions-test-visual-studio-xunit.png)

### <a name="setup"></a>Kurulum

Ortamınızı ayarlamak için bir işlev oluşturun ve uygulamayı test etme. Aşağıdaki adımlar, uygulama ve testleri desteklemek için gereken işlevleri oluşturmanıza yardımcı:

1. [Yeni bir işlev uygulaması oluşturma](./functions-create-first-azure-function.md) ve adlandırın *işlevleri*
2. [Bir HTTP işlev şablonu oluşturma](./functions-create-first-azure-function.md) ve adlandırın *HttpTrigger*.
3. [Şablondan Zamanlayıcı bir işlev oluşturma](./functions-create-scheduled-function.md) ve adlandırın *TimerTrigger*.
4. [XUnit Test uygulama oluşturma](https://xunit.github.io/docs/getting-started-dotnet-core) tıklayarak Visual Studio'da **Dosya > Yeni > Proje > Visual C# > .NET Core > xUnit Test projesi** ve adlandırın *Functions.Test*. 
5. Test uygulamadan bir başvuru eklemek için Nuget kullanma [Microsoft.AspNetCore.Mvc](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc/)
6. [Başvuru *işlevleri* uygulama](https://docs.microsoft.com/visualstudio/ide/managing-references-in-a-project?view=vs-2017) gelen *Functions.Test* uygulama.

### <a name="create-test-classes"></a>Test sınıfları oluşturma

Uygulamaları oluşturulan, otomatik testleri çalıştırmak için kullanılan sınıfları oluşturabilirsiniz.

Her işlev bir örneğini alır [ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.ilogger) ileti günlüğe kaydetmeyi işlemek için. Bazı testler olmayan iletileri günlüğe ya da günlük nasıl uygulandığını için herhangi bir sorun vardır. Bir test geçmediği belirlenemiyor oturum iletileri değerlendirmek diğer testlerin gerekir.

`ListLogger` Sınıfı uygulamak için tasarlanmıştır `ILogger` arabirim ve iç değerlendirmesi sırasında bir test için ileti listesi de tutun.

**Sağ** üzerinde *Functions.Test* seçin ve uygulama **Ekle > sınıfı**, adlandırın **NullScope.cs** ve aşağıdaki kodu girin:

```csharp
using System;

namespace Functions.Tests
{
    public class NullScope : IDisposable
    {
        public static NullScope Instance { get; } = new NullScope();

        private NullScope() { }

        public void Dispose() { }
    }
}
```

Ardından, **sağ** üzerinde *Functions.Test* seçin ve uygulama **Ekle > sınıfı**, adlandırın **ListLogger.cs** girin Aşağıdaki kodu:

```csharp
using Microsoft.Extensions.Logging;
using System;
using System.Collections.Generic;
using System.Text;

namespace Functions.Tests
{
    public class ListLogger : ILogger
    {
        public IList<string> Logs;

        public IDisposable BeginScope<TState>(TState state) => NullScope.Instance;

        public bool IsEnabled(LogLevel logLevel) => false;

        public ListLogger()
        {
            this.Logs = new List<string>();
        }

        public void Log<TState>(LogLevel logLevel, 
                                EventId eventId,
                                TState state,
                                Exception exception,
                                Func<TState, Exception, string> formatter)
        {
            string message = formatter(state, exception);
            this.Logs.Add(message);
        }
    }
}
```

`ListLogger` Sınıfın uyguladığı aşağıdaki üyeleri tarafından sözleşmeleri yapılır gibi `ILogger` arabirimi:

- **BeginScope**: Kapsamları, günlüğe kaydetme için bağlam ekleyin. Bu durumda, test yalnızca statik örneğine noktaları `NullScope` test işlevine izin veren sınıfı.

- **IsEnabled**: Varsayılan değer olarak `false` sağlanır.

- **Günlük**: Bu yöntem sağlanan kullanan `formatter` iletiyi biçimlendirmek için işlev ve ortaya çıkan metni ekler `Logs` koleksiyonu.

`Logs` Koleksiyon öğesinin bir örneğiyse `List<string>` ve oluşturucuda başlatılır.

Ardından, **sağ** üzerinde *Functions.Test* seçin ve uygulama **Ekle > sınıfı**, adlandırın **LoggerTypes.cs** girin Aşağıdaki kodu:

```csharp
namespace Functions.Tests
{
    public enum LoggerTypes
    {
        Null,
        List
    }
}
```
Bu numaralandırma testleri tarafından kullanılan Günlükçü türünü belirtir. 

Ardından, **sağ** üzerinde *Functions.Test* seçin ve uygulama **Ekle > sınıfı**, adlandırın **TestFactory.cs** girin Aşağıdaki kodu:

```csharp
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Http.Internal;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Logging.Abstractions;
using Microsoft.Extensions.Primitives;
using System.Collections.Generic;

namespace Functions.Tests
{
    public class TestFactory
    {
        public static IEnumerable<object[]> Data()
        {
            return new List<object[]>
            {
                new object[] { "name", "Bill" },
                new object[] { "name", "Paul" },
                new object[] { "name", "Steve" }

            };
        }

        private static Dictionary<string, StringValues> CreateDictionary(string key, string value)
        {
            var qs = new Dictionary<string, StringValues>
            {
                { key, value }
            };
            return qs;
        }

        public static DefaultHttpRequest CreateHttpRequest(string queryStringKey, string queryStringValue)
        {
            var request = new DefaultHttpRequest(new DefaultHttpContext())
            {
                Query = new QueryCollection(CreateDictionary(queryStringKey, queryStringValue))
            };
            return request;
        }

        public static ILogger CreateLogger(LoggerTypes type = LoggerTypes.Null)
        {
            ILogger logger;

            if (type == LoggerTypes.List)
            {
                logger = new ListLogger();
            }
            else
            {
                logger = NullLoggerFactory.Instance.CreateLogger("Null Logger");
            }

            return logger;
        }
    }
}
```
`TestFactory` Sınıfı aşağıdaki üyelere uygular:

- **Veri**: Bu özellik döndürür bir [IEnumerable](https://docs.microsoft.com/dotnet/api/system.collections.ienumerable) örnek verileri koleksiyonu. Anahtar-değer çiftlerinin bir sorgu dizesinde geçirilen değerleri temsil eder.

- **CreateDictionary**: Bu yöntem, anahtar/değer çifti bağımsız değişken olarak kabul eder ve yeni bir `Dictionary` oluşturmak için kullanılan `QueryCollection` sorgu dizesi değerlerini temsil etmek için.

- **CreateHttpRequest**: Bu yöntem ile belirtilen sorgu dizesi parametreleri başlatılan bir HTTP isteği oluşturur.

- **CreateLogger**: Günlükçü türüne göre bu yöntem test etmek için kullanılan bir Günlükçü sınıf döndürür. `ListLogger` Günlüğe kaydedilen iletilere değerlendirmesi testlerinde kullanılabilecek izler.

Ardından, **sağ** üzerinde *Functions.Test* seçin ve uygulama **Ekle > sınıfı**, adlandırın **FunctionsTests.cs** girin Aşağıdaki kodu:

```csharp
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Logging;
using Xunit;

namespace Functions.Tests
{
    public class FunctionsTests
    {
        private readonly ILogger logger = TestFactory.CreateLogger();

        [Fact]
        public async void Http_trigger_should_return_known_string()
        {
            var request = TestFactory.CreateHttpRequest("name", "Bill");
            var response = (OkObjectResult)await HttpTrigger.Run(request, logger);
            Assert.Equal("Hello, Bill", response.Value);
        }

        [Theory]
        [MemberData(nameof(TestFactory.Data), MemberType = typeof(TestFactory))]
        public async void Http_trigger_should_return_known_string_from_member_data(string queryStringKey, string queryStringValue)
        {
            var request = TestFactory.CreateHttpRequest(queryStringKey, queryStringValue);
            var response = (OkObjectResult)await HttpTrigger.Run(request, logger);
            Assert.Equal($"Hello, {queryStringValue}", response.Value);
        }

        [Fact]
        public void Timer_should_log_message()
        {
            var logger = (ListLogger)TestFactory.CreateLogger(LoggerTypes.List);
            TimerTrigger.Run(null, logger);
            var msg = logger.Logs[0];
            Assert.Contains("C# Timer trigger function executed at", msg);
        }
    }
}
```
Bu sınıfta uygulanır üyeleri şunlardır:

- **Http_trigger_should_return_known_string**: Bu test bir istek sorgu dizesi değerlerini oluşturur `name=Bill` HTTP işlevi ve beklenen yanıt verilir denetimleri.

- **Http_trigger_should_return_string_from_member_data**: Bu test xUnit öznitelikleri HTTP işlevi örnek verilerini sağlamak için kullanır.

- **Timer_should_log_message**: Bu test örneği oluşturur `ListLogger` ve Zamanlayıcı işleve geçirir. İşlevi çalıştırıldığında, günlük, beklenen bir ileti mevcut olduğundan emin olmak için denetlenir.

### <a name="run-tests"></a>Testleri çalıştırma

Testleri çalıştırmak için gidin **Test Gezgini** tıklatıp **çalıştırması**.

![İle Azure işlevlerini test etme C# Visual Studio'da](./media/functions-test-a-function/azure-functions-test-visual-studio-xunit.png)

### <a name="debug-tests"></a>Testlerde Hata Ayıkla

Testlerde hata ayıklamak için bir test üzerinde bir kesme noktası ayarlayın, gidin **Test Gezgini** tıklatıp **çalıştırın > hata ayıklama son çalıştırma**.

## <a name="javascript-in-vs-code"></a>VS code'da JavaScript

Aşağıdaki örnekte, VS Code'da JavaScript işlev uygulaması oluşturma ve çalıştırma açıklar ve test [Jest](https://jestjs.io). Bu yordamı kullanır [VS Code işlevleri uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) Azure işlevleri.

![VS Code'da JavaScript ile Azure işlevlerini test etme](./media/functions-test-a-function/azure-functions-test-vs-code-jest.png)

### <a name="setup"></a>Kurulum

Ortamınızı ayarlamak için yeni bir Node.js uygulaması boş bir klasör içinde çalıştırarak başlatmak `npm init`.

```bash
npm init -y
```
Ardından, Jest aşağıdaki komutu çalıştırarak yükleyin:

```bash
npm i jest
```
Şimdi Güncelleştir _package.json_ aşağıdaki komutla var olan test komutu değiştirmek için:

```bash
"scripts": {
    "test": "jest"
}
```

### <a name="create-test-modules"></a>Test modülleri oluşturma
Başlatılan proje ile otomatik testleri çalıştırmak için kullanılan modüller oluşturabilirsiniz. Adlı yeni bir klasör oluşturarak başlayın *test* destek modülleri tutacak.

İçinde *test* klasörüne yeni dosya ekleme, adlandırın **defaultContext.js**ve aşağıdaki kodu ekleyin:

```javascript
module.exports = {
    log: jest.fn()
};
```
Bu modül mocks *günlük* varsayılan yürütme bağlamı temsil etmek için işlevi.

Ardından, yeni bir dosya ekleyin, adlandırın **defaultTimer.js**ve aşağıdaki kodu ekleyin:

```javascript
module.exports = {
    IsPastDue: false
};
```
Bu modül uygular `IsPastDue` öne çıkarmak için özelliği olan sahte Zamanlayıcı örneği olarak.

Ardından, VS Code işlevleri uzantısı kullanın [yeni bir JavaScript HTTP işlev oluşturma](https://code.visualstudio.com/tutorials/functions-extension/getting-started) ve adlandırın *HttpTrigger*. İşlev oluşturulduktan sonra yeni bir dosya adlı klasörde eklemek **index.test.js**ve aşağıdaki kodu ekleyin:

```javascript
const httpFunction = require('./index');
const context = require('../testing/defaultContext')

test('Http trigger should return known text', async () => {

    const request = {
        query: { name: 'Bill' }
    };

    await httpFunction(context, request);

    expect(context.log.mock.calls.length).toBe(1);
    expect(context.res.body).toEqual('Hello Bill');
});
```
Şablondan HTTP işlevi sorgu dizesinde sağlanan ad ile birleştirilmiş "Hello" dizesi döndürür. Bu test, istek sahte bir örneğini oluşturur ve HTTP işleve geçirir. Test denetleyen *günlük* yöntemi bir kez çağrılır ve döndürülen metin "Hello Bill" değerine eşittir.

Ardından, yeni bir JavaScript Zamanlayıcı işlevi oluşturun ve adlandırın için VS Code işlevleri uzantısının kullanılması *TimerTrigger*. İşlev oluşturulduktan sonra yeni bir dosya adlı klasörde eklemek **index.test.js**ve aşağıdaki kodu ekleyin:

```javascript
const timerFunction = require('./index');
const context = require('../testing/defaultContext');
const timer = require('../testing/defaultTimer');

test('Timer trigger should log message', () => {
    timerFunction(context, timer);
    expect(context.log.mock.calls.length).toBe(1);
});
```
Şablondan Zamanlayıcı işlevi, işlev gövdesinin sonuna bir iletiyi günlüğe kaydeder. Bu test sağlar *günlük* işlevi bir kez çağrılır.

### <a name="run-tests"></a>Testleri çalıştırma
Testleri çalıştırmak için basın **CTRL + ~** komut penceresi açın ve çalıştırmak için `npm test`:

```bash
npm test
```

![VS Code'da JavaScript ile Azure işlevlerini test etme](./media/functions-test-a-function/azure-functions-test-vs-code-jest.png)

### <a name="debug-tests"></a>Testlerde Hata Ayıkla

Testlerinizi hata ayıklamak için aşağıdaki yapılandırmaya ekleyin, *launch.json* dosyası:

```json
{
  "type": "node",
  "request": "launch",
  "name": "Jest Tests",
  "program": "${workspaceRoot}\\node_modules\\jest\\bin\\jest.js",
  "args": [
      "-i"
  ],
  "internalConsoleOptions": "openOnSessionStart"
}
```

Ardından, test ve basın bir kesme noktası ayarlamak **F5**.

## <a name="next-steps"></a>Sonraki adımlar

İşlevleriniz için otomatik testler yazmak öğrendiniz, bu kaynakları ile devam edin:
- [El ile olmayan HTTP ile tetiklenen bir işlev çalıştırın](./functions-manually-run-non-http.md)
- [Azure işlevleri hata işleme](./functions-bindings-error-pages.md)
- [Azure işlevi olay Kılavuzu tetikleyicisi yerel hata ayıklama](./functions-debug-event-grid-trigger-local.md)
