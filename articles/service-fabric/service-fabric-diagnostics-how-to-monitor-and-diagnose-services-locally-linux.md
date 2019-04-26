---
title: Hata ayıklama Azure Service Fabric Linux uygulamalarında | Microsoft Docs
description: Yerel Linux geliştirme makinesinde, Service Fabric Hizmetleri izleme ve Tanılama hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: chackdan
editor: ''
ms.assetid: 4eebe937-ab42-4429-93db-f35c26424321
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: f0b850038a29dd0949def97b359b2b7a5ce920bc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60392866"
---
# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>İçinde bir yerel makine dağıtım kurulumunda Hizmetleri izleme ve tanılama


> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
> * [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
>
>

İzleme, algılama, tanılama ve sorun giderme kullanıcı deneyimi için çok az kesinti devam etmek hizmetler sağlar. İzleme ve Tanılama, gerçek dağıtılan üretim ortamında önemlidir. Benzer bir model Hizmetleri geliştirilmesi sırasında benimsenmesi, bir üretim ortamına taşıdığınızda tanılama işlem hattının çalışır sağlar. Service Fabric, tek makinede yerel geliştirme ayarları ve gerçek üretim kümesi ayarlar arasında sorunsuz bir şekilde çalışabilir tanılama uygulamak hizmet geliştiricileri kolaylaştırır.


## <a name="debugging-service-fabric-java-applications"></a>Service Fabric Java uygulamalarında hata ayıklama

Java uygulamaları için [birden çok günlük altyapılarına](https://en.wikipedia.org/wiki/Java_logging_framework) kullanılabilir. Bu yana `java.util.logging` varsayılan seçenek JRE ile de için kullanıldığı [kodu github'da örnekleri](https://github.com/Azure-Samples/service-fabric-java-getting-started). Aşağıdaki tartışma nasıl yapılandırılacağını açıklar `java.util.logging` framework.

Java.Util.Logging kullanarak uygulama günlüklerinizi bellek, çıkış akışları, konsol dosyaları ya da yuva yönlendirebilirsiniz. Bu seçeneklerin her biri için zaten çerçevesinde sağlanan varsayılan işleyicileri vardır. Oluşturabileceğiniz bir `app.properties` dosya işleyicisi için tüm günlükleri yerel bir dosyaya yeniden yönlendirmek uygulamanızın yapılandırma dosyası.

Aşağıdaki kod parçacığını bir örnek yapılandırma içerir:

```java
handlers = java.util.logging.FileHandler

java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.limit = 1024000
java.util.logging.FileHandler.count = 10
java.util.logging.FileHandler.pattern = /tmp/servicefabric/logs/mysfapp%u.%g.log
```

Klasör tarafından işaret edilen `app.properties` dosyası bulunmalıdır. Sonra `app.properties` dosyası oluşturulur, aynı zamanda, giriş noktası betiğini değiştirmeniz gerekir `entrypoint.sh` içinde `<applicationfolder>/<servicePkg>/Code/` özelliğini ayarlamak için klasör `java.util.logging.config.file` için `app.properties` dosya. Giriş, aşağıdaki kod parçacığı gibi görünmelidir:

```sh
java -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=<path to app.properties> -jar <service name>.jar
```


Bu yapılandırma günlüklerinde yenilenen bir biçimde toplanan sonuçları `/tmp/servicefabric/logs/`. Günlük dosyası bu durumda mysfapp%u.%g.log adlı burada:
* **%u** eşzamanlı Java işlemleri arasındaki çakışmaları gidermek için benzersiz bir sayıdır.
* **%g** günlükleri döndürme arasında ayrım yapmak için nesil sayıdır.

Herhangi işleyici açıkça yapılandırdıysanız, varsayılan olarak konsol işleyici kayıtlı. Bir syslog /var/log/syslog altında günlükleri görüntüleyebilirsiniz.

Daha fazla bilgi için [kodu github'da örnekleri](https://github.com/Azure-Samples/service-fabric-java-getting-started).


## <a name="debugging-service-fabric-c-applications"></a>Service Fabric C# uygulamalarında hata ayıklama


Birden çok çerçeveler, CoreCLR uygulamaları Linux izleme için kullanılabilir. Daha fazla bilgi için [GitHub: günlük](http:/github.com/aspnet/logging).  EventSource C# geliştiricileri için ilgili bilgi sahibi olduğundan ' CoreCLR örnekleri Linux izleme için bu makalede EventSource kullanır.

İlk adım, bellek, çıkış akışları veya Konsolu dosyaları günlüklerinizi yazabilmesi amacıyla System.Diagnostics.Tracing dahil etmektir.  EventSource kullanarak günlüğe kaydetme için aşağıdaki proje, Project.json'yi ekleyin:

```json
    "System.Diagnostics.StackTrace": "4.0.1"
```

Hizmet olayı için dinler ve ardından bunları izleme dosyaları için uygun şekilde yönlendirmek için bir özel EventListener kullanabilirsiniz. Aşağıdaki kod parçacığını bir örnek uygulama EventSource ve özel EventListener kullanarak oturum gösterir:


```csharp

public class ServiceEventSource : EventSource
{
        public static ServiceEventSource Current = new ServiceEventSource();

        [NonEvent]
        public void Message(string message, params object[] args)
        {
            if (this.IsEnabled())
            {
                var finalMessage = string.Format(message, args);
                this.Message(finalMessage);
            }
        }

        // TBD: Need to add method for sample event.

}

```


```csharp
internal class ServiceEventListener : EventListener
{

        protected override void OnEventSourceCreated(EventSource eventSource)
        {
            EnableEvents(eventSource, EventLevel.LogAlways, EventKeywords.All);
        }
        protected override void OnEventWritten(EventWrittenEventArgs eventData)
        {
                using (StreamWriter Out = new StreamWriter( new FileStream("/tmp/MyServiceLog.txt", FileMode.Append)))
                {
                        // report all event information
                        Out.Write(" {0} ", Write(eventData.Task.ToString(), eventData.EventName, eventData.EventId.ToString(), eventData.Level,""));
                        if (eventData.Message != null)
                                Out.WriteLine(eventData.Message, eventData.Payload.ToArray());
                        else
                        {
                                string[] sargs = eventData.Payload != null ? eventData.Payload.Select(o => o.ToString()).ToArray() : null; 
                                Out.WriteLine("({0}).", sargs != null ? string.Join(", ", sargs) : "");
                        }
                }
        }
}
```


Yukarıdaki kod parçacığında günlükleri bir dosyaya çıkarır `/tmp/MyServiceLog.txt`. Bu dosya adı, uygun şekilde güncelleştirilmesi gerekiyor. Konsol günlükleri yeniden yönlendirmek istemeniz durumunda, özelleştirilmiş EventListener sınıfınızda aşağıdaki kod parçacığını kullanın:

```csharp
public static TextWriter Out = Console.Out;
```

Örnekler'i de [C# örnekleri](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started) EventSource ve özel EventListener bir dosyaya olaylarını günlüğe kaydedecek şekilde kullanın.



## <a name="next-steps"></a>Sonraki adımlar
Uygulamanıza eklediğiniz aynı izleme kodu bir Azure kümesine uygulama Tanılama ile de çalışır. Araçlar için farklı seçenekler hakkında konuşmak ve bunları ayarlamak nasıl açıklayan şu makalelere göz atın.
* [Azure Tanılama ile günlük toplama](service-fabric-diagnostics-how-to-setup-lad.md)
