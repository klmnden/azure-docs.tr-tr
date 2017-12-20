---
title: "Linux içindeki Azure mikro hata ayıklama | Microsoft Docs"
description: "İzleme ve bir yerel geliştirme makinede Microsoft Azure Service Fabric kullanılarak yazılmış hizmetlerinizi tanılama öğrenin."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 4eebe937-ab42-4429-93db-f35c26424321
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 4bc73f581f4855ebc724df19dd56fab8bf103854
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>İzleme ve yerel makine geliştirme Kurulum Hizmetleri'nde tanılama


> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
> * [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
>
>

İzleme, algılama, tanılama ve sorun giderme Hizmetleri kullanıcı deneyimini en az kesintiyi devam etmek izin verir. İzleme ve tanılama gerçek dağıtılmış üretim ortamında önemlidir. Benzer bir model Hizmetleri geliştirme sırasında benimsenmesi üretim ortamına taşıdığınızda tanılama ardışık düzen çalışmasını sağlar. Service Fabric tek makineli yerel geliştirme kurulumları ve gerçek üretim küme kurulumları arasında sorunsuz bir şekilde çalışabilirsiniz tanılama uygulamak hizmet geliştiricileri için kolaylaştırır.


## <a name="debugging-service-fabric-java-applications"></a>Service Fabric Java uygulamalarında hata ayıklama

Java uygulamaları için [birden çok günlük altyapıları](http://en.wikipedia.org/wiki/Java_logging_framework) kullanılabilir. Bu yana `java.util.logging` varsayılan seçenektir JRE ile de için kullanıldığı [kod örnekleri github'da](http://github.com/Azure-Samples/service-fabric-java-getting-started).  Aşağıdaki tartışma nasıl yapılandırılacağını açıklar `java.util.logging` framework.

Java.Util.Logging kullanarak uygulama günlüklerinizin bellek, çıkış akışları, konsol dosyaları veya yuva yönlendirebilirsiniz. Bu seçeneklerin her biri için varsayılan işleyicisi zaten framework sağlanan vardır. Oluşturabileceğiniz bir `app.properties` dosya işleyicisi tüm günlükleri yerel bir dosyaya yeniden yönlendirmek, uygulamanız için yapılandırmak için bir dosya.

Aşağıdaki kod parçacığını bir örnek yapılandırma içerir:

```java
handlers = java.util.logging.FileHandler

java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.limit = 1024000
java.util.logging.FileHandler.count = 10
java.util.logging.FileHandler.pattern = /tmp/servicefabric/logs/mysfapp%u.%g.log             
```

Tarafından klasörü işaret için `app.properties` dosya bulunmalıdır. Sonra `app.properties` dosyası oluşturuldu, ayrıca, giriş noktası komut değiştirmenize gerek `entrypoint.sh` içinde `<applicationfolder>/<servicePkg>/Code/` özelliğini ayarlamak için klasör `java.util.logging.config.file` için `app.propertes` dosya. Giriş aşağıdaki kod parçacığını gibi görünmelidir:

```sh
java -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=<path to app.properties> -jar <service name>.jar
```


Bu yapılandırma sırasında dönen bir şekilde toplanmakta olan günlükleri sonuçlanır `/tmp/servicefabric/logs/`. Günlük dosyası bu durumda mysfapp%u.%g.log adlı burada:
* **%u** eşzamanlı Java işlemleri arasındaki çakışmaları çözümlemek için benzersiz bir sayıdır.
* **%g** günlükleri döndürme arasında ayrım yapmak için nesil sayıdır.

Hiçbir işleyici açıkça yapılandırdıysanız, varsayılan olarak konsol işleyicisi kaydedildi. Bir syslog /var/log/syslog altında günlükleri görüntüleyebilirsiniz.

Daha fazla bilgi için bkz: [kod örnekleri github'da](http://github.com/Azure-Samples/service-fabric-java-getting-started).  


## <a name="debugging-service-fabric-c-applications"></a>Service Fabric C# uygulamalarının hatalarını ayıklama


Birden çok çerçeveyi CoreCLR uygulamaları Linux izleme için kullanılabilir. Daha fazla bilgi için bkz: [GitHub: günlüğü](http:/github.com/aspnet/logging).  EventSource C# geliştiricileri için tanıdık olduğundan ' CoreCLR örnekleri Linux izleme için bu makalede EventSource kullanır.

İlk adım bellek, çıkış akışları ya da konsol dosyalara günlüklerinizi yazabilirsiniz böylece System.Diagnostics.Tracing eklemektir.  EventSource kullanarak günlüğe kaydetme için aşağıdaki proje project.json dosyasına ekleyin:

```
    "System.Diagnostics.StackTrace": "4.0.1"
```

Hizmet olayı dinleme ve ardından bunları izleme dosyaları için uygun şekilde yönlendirmek için özel bir EventListener kullanabilirsiniz. Aşağıdaki kod parçacığını bir örnek uygulama EventSource ve özel EventListener kullanarak oturum gösterir:


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
         Out.Write(" {0} ",  Write(eventData.Task.ToString(), eventData.EventName, eventData.EventId.ToString(), eventData.Level,""));
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


Yukarıdaki kod parçacığında bulunan bir dosyaya günlükleri çıkarır `/tmp/MyServiceLog.txt`. Bu dosya adı uygun şekilde güncelleştirilmesi gerekir. Konsol günlükleri yönlendirmek istemeniz durumunda, aşağıdaki kod parçacığında özelleştirilmiş EventListener sınıfınızda kullanın:

```csharp
public static TextWriter Out = Console.Out;
```

Konumundaki örnekleri [C# örnekleri](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started) bir dosyaya günlük olaylarıyla EventSource ve özel EventListener kullanın.



## <a name="next-steps"></a>Sonraki adımlar
Uygulamanıza eklediğiniz aynı izleme kodu uygulamanızın Azure bir kümede Tanılama ile de çalışır. Araçlar için farklı seçenekleri ele ve bunları nasıl ayarlanacağını açıklayan Bu makaleler göz atın.
* [Azure Tanılama ile günlükleri toplamak nasıl](service-fabric-diagnostics-how-to-setup-lad.md)
