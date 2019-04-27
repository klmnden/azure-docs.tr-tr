---
title: Azure Application Insights izleme dağıtılmış | Microsoft Docs
description: Yerel ileticisi ve OpenCensus projedeki iş ortaklığı dağıtılmış izleme için Microsoft'un desteği hakkında bilgi sağlar
services: application-insights
keywords: ''
author: nikmd23
ms.author: nimolnar
ms.reviewer: mbullwin
ms.date: 09/17/2018
ms.service: application-insights
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 7bc04748f2a5b8caa8f589140dd46f0650b7b390
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60898857"
---
# <a name="what-is-distributed-tracing"></a>Dağıtılmış izleme nedir?

Modern bulut gelişinden ve [mikro Hizmetler](https://azure.com/microservices) mimarileri verilen artış basit, bağımsız bir şekilde dağıtılabilen hizmetlerine kullanılabilirlik ve aktarım hızı artırırken maliyetleri azaltmaya yardımcı olabilir. Ancak bu hareketleri tek tek Hizmetleri bir bütün olarak anlamak daha da kolaylaştırdık, ancak bunlar genel sistemleri hakkında neden ve hatalarını ayıklamak daha zor yaptık.

Tek parça mimarilerde çağrı yığınları ile hata ayıklama için kullandığımız değil. Çağrı yığınlarını olan (çağrılan yöntem C yöntemi adlı bir yöntemi B) akışını göstermek için parlak araçları, ayrıntıları ve parametreleri her çağrılara hakkında yanı sıra. Bu hamleye olanak veya tek bir işlem üzerinde çalışan hizmetler için mükemmel olmakla birlikte nasıl hata ayıklama ne zaman bir başvuru değil yalnızca yerel yığın üzerinde bir işlem sınırı arasında çağrıdır? 

Bu, dağıtılmış izleme burada devreye girer.  

Modern Bulut ve mikro hizmet mimarileri için çağrı yığınlarını oluşturulan bir alıyormuş performans profili Oluşturucu eklenmesi ile dağıtılmış izleme eşdeğerdir. Azure İzleyici'de iki deneyimleri dağıtılmış izleme verileri tüketim için sunuyoruz. İlk bizim [işlem tanılamaları](https://docs.microsoft.com/azure/application-insights/app-insights-transaction-diagnostics) eklenen zaman boyutu ile çağrı yığını gibi olan bir görünümü. İşlem tanılama görünümü tek bir işlem/istek görünürlük sağlar ve güvenilirlik sorunlarını ve istek başına üzerinde performans sorunlarının temel nedenini bulmak için yararlıdır.

Azure İzleyici ayrıca sunan bir [Uygulama Haritası](https://docs.microsoft.com/azure/application-insights/app-insights-app-map) sistemleri nasıl etkileşim ve ortalama performans ve hata oranları nelerdir topolojik bir görünümünü göstermek için birçok işlemleri toplayan görünümü. 

## <a name="how-to-enable-distributed-tracing"></a>Dağıtılmış izlemeyi etkinleştirme

Uygulama hizmetlerinde dağıtılmış izlemeyi etkinleştirme, uygun SDK veya kitaplığı her hizmet için bağlı hizmet içinde uygulanmış dil eklemek kadar kolaydır.

## <a name="enabling-via-application-insights-sdks"></a>Application Insights SDK'ları etkinleştirme

Application Insights SDK'ları için .NET, .NET Core, Java, Node.js ve JavaScript tüm dağıtılmış izleme yerel olarak destekler. Yükleyip her Application Insights SDK'sını yapılandırmaya yönelik yönergeler aşağıda sunulmaktadır:

* [.NET](https://docs.microsoft.com/azure/application-insights/quick-monitor-portal)
* [.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-dotnetcore-quick-start)
* [Java](https://docs.microsoft.com/azure/application-insights/app-insights-java-get-started)
* [Node.js](https://docs.microsoft.com/azure/application-insights/app-insights-nodejs-quick-start)
* [JavaScript](https://docs.microsoft.com/azure/application-insights/app-insights-javascript)

Uygun Application Insights yüklenmiş ve yapılandırılmış SDK ile izleme bilgileri otomatik olarak popüler çerçeveleri, kitaplıkları ve teknolojileri için SDK'sı bağımlılık otomatik-toplayıcıları tarafından toplanır. Desteklenen teknolojiler tam listesi kullanılabilir [bağımlılık toplama otomatik belgeleri](https://docs.microsoft.com/azure/application-insights/auto-collect-dependencies).

 Ayrıca, herhangi bir teknoloji el ile bir çağrıyla izlenebilir [TrackDependency](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics) üzerinde [TelemetryClient](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics).

## <a name="enable-via-opencensus"></a>OpenCensus etkinleştirme

Application Insights SDK'ları yanı sıra Application Insights ile dağıtılmış izleme de destekler [OpenCensus](https://opencensus.io/). Bir açık kaynak, bağımsız satıcı, kitaplıkların tek dağıtım ölçümleri toplama ve dağıtılmış izleme hizmetleri sunmak için OpenCensus olur. Ayrıca, Redis gibi Memcached veya MongoDB popüler teknolojileri ile dağıtılmış izlemeyi etkinleştirmek açık kaynak topluluğundan da sağlar. [Microsoft ortak çalışmalarla OpenCensus üzerinde çeşitli diğer izleme ve bulut iş ortakları ile](https://open.microsoft.com/2018/06/13/microsoft-joins-the-opencensus-project/).

İlk dağıtılmış izleme özellikleri OpenCensus, bir uygulamaya eklemek için [yükleme ve Application Insights yerel ileticisi yapılandırma](./../../azure-monitor/app/opencensus-local-forwarder.md). Buradan, yerel ileticisi dağıtılmış İzleme verilerine yönlendirmek için OpenCensus yapılandırın. Her ikisi de [Python](./../../azure-monitor/app/opencensus-python.md) ve [Git](./../../azure-monitor/app/opencensus-go.md) desteklenir.

OpenCensus Web sitesi için API başvuru belgeleri tutar [Python](https://opencensus.io/api/python/trace/usage.html) ve [Git](https://godoc.org/go.opencensus.io), çeşitli farklı kılavuzları OpenCensus kullanmak için ek olarak. 

## <a name="next-steps"></a>Sonraki adımlar

* [OpenCensus Python Kullanım Kılavuzu](https://opencensus.io/api/python/trace/usage.html)
* [Uygulama Haritası](./../../azure-monitor/app/app-map.md)
* [Uçtan uca performans izleme](./../../azure-monitor/learn/tutorial-performance.md)
