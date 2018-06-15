---
title: Service Fabric proje oluşturma sonraki adımlar | Microsoft Docs
description: Visual Studio'da oluşturduğunuz uygulama projesi hakkında bilgi edinin.  Öğreticiler kullanarak hizmetleri oluşturmayı öğrenin ve Hizmetleri için Service Fabric geliştirme hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: 299d1f97-1ca9-440d-9f81-d1d0dd2bf4df
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/07/2017
ms.author: rwike77
ms.openlocfilehash: a87dd6f4afa152aebafdde24defcabe841ae2e9c
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34206474"
---
# <a name="your-service-fabric-application-and-next-steps"></a>Service Fabric uygulaması ve sonraki adımlar
Azure Service Fabric uygulamanızı oluşturuldu. Bu makalede denemek için bazı öğreticileri, oluşma şekli projeniz, ilginizi çekebilir bazı ek bilgiler ve olası sonraki adımlar açıklanmaktadır.

## <a name="get-started-with-tutorials-walk-throughs-and-samples"></a>Öğreticiler, kılavuzlarına ve örnekleri kullanmaya başlama
Başlamaya hazır mısınız?  

.NET uygulama öğreticide çalışır. Bilgi nasıl [bir uygulama oluşturmak](service-fabric-tutorial-create-dotnet-app.md) bir ASP.NET Core ön uç ve bir durum bilgisi olan arka uç, [uygulamayı dağıtmak](service-fabric-tutorial-deploy-app-to-party-cluster.md) bir kümeye [CI/CD yapılandırmak](service-fabric-tutorial-deploy-app-with-cicd-vsts.md), ve [ayarlama İzleme ve tanılama](service-fabric-tutorial-monitoring-aspnet.md).

Veya, aşağıdaki kılavuzlarına birini deneyin ve ilk oluştur...
- [C# Reliable Services Windows hizmeti](service-fabric-reliable-services-quick-start.md) 
- [C# Reliable Actors hizmeti Windows](service-fabric-reliable-actors-get-started.md) 
- [Windows Konuk yürütülebilir hizmeti](quickstart-guest-app.md) 
- [Windows kapsayıcı uygulaması](service-fabric-get-started-containers.md) 

Ayrıca çalışırken ilginizi çekebilir bizim [örnek uygulamaları](http://aka.ms/servicefabricsamples).

## <a name="have-questions-or-feedback--need-to-report-an-issue"></a>Sorularınız veya Geri bildiriminiz var?  Bir sorunu bildirmek üzere gerekiyor?
Okuyun [sık sorulan sorular](service-fabric-common-questions.md) ve Service Fabric neler yapabileceğinizi ve nasıl kullanılacağını yanıtlarını bulun.

[Destek Seçenekleri](service-fabric-support.md) sorunları raporlaması, destek almak ve ürün geri bildirim göndermek için seçenekleri yanı sıra sorular soran için StackOverflow ve MSDN Forumları listeler.

## <a name="the-application-project"></a>Uygulama projesi
Her yeni uygulama, bir uygulama projesi içerir. Bir veya iki ek projeleri, seçilen hizmet türüne bağlı olarak olabilir.

Uygulama projesi oluşur:

* Başvurular, uygulamayı oluşturan hizmetlerine kümesi.
* Üç--varsayılan olarak bir küme uç noktası ve yükseltme dağıtımları yapılıp yapılmayacağını ilgili tercihleri gibi farklı ortamları ile çalışmaya yönelik tercihlerini korumak için kullanabileceğiniz profilleri (1-düğümü yerel, 5-Node yerel ve bulut) yayımlayın.
* Üç uygulama parametre (bir hizmet oluşturmak için bölüm sayısı gibi ortama özgü uygulama yapılandırmaları korumak için kullanabileceğiniz aynı yukarıda) dosyaları. Bilgi edinmek için nasıl [uygulamanız birden çok ortamı için yapılandırma](service-fabric-manage-multiple-environment-app-configuration.md).
* Komut satırından veya bir otomatik sürekli tümleştirme ve dağıtım ardışık parçası olarak, uygulamanızı dağıtmak için kullanabileceğiniz bir dağıtım komut dosyası. Daha fazla bilgi edinmek [PowerShell kullanarak uygulamaları dağıtma](service-fabric-deploy-remove-applications.md).
* Uygulama bildirimi uygulamanın açıklar. Bildirim ApplicationPackageRoot klasörü altında bulabilirsiniz. Daha fazla bilgi edinmek [uygulama ve hizmet bildirimlerini](service-fabric-application-model.md).



## <a name="learn-more-about-the-programming-models"></a>Programlama modelleri hakkında daha fazla bilgi edinin
Service Fabric yazma ve hizmetlerinizi yönetmek için birden çok yol sunar.  İşte genel bakış ve kavramsal bilgileri [durum bilgisiz ve durum bilgisi olan güvenilir hizmetler](service-fabric-reliable-services-introduction.md), [Reliable Actors](service-fabric-reliable-actors-introduction.md), [kapsayıcıları](service-fabric-containers-overview.md), [Konuk yürütülebilir dosyalar ](service-fabric-guest-executables-introduction.md), ve [durum bilgisiz ve durum bilgisi olan ASP.NET Core services](service-fabric-reliable-services-communication-aspnetcore.md).

## <a name="learn-about-service-communication"></a>Hizmet iletişimi hakkında bilgi edinin
Service Fabric uygulaması burada her hizmet özelleştirilmiş bir görev gerçekleştiren farklı hizmetlerinden oluşur. Bu hizmetlerin birbirleriyle iletişim kurabilir ve bağlanmak ve Hizmetleri ile iletişim kuran istemci uygulamaları küme dışında olabilir. Bilgi nasıl [ile ve hizmetlerinizi arasındaki iletişimi ayarlama](service-fabric-connect-and-communicate-with-services.md) Service Fabric içinde. 

## <a name="learn-about-configuring-application-security"></a>Uygulama güvenliği yapılandırma hakkında bilgi edinin
Kümedeki farklı kullanıcı hesabı altında çalışan uygulamaları güvenliğini sağlayabilirsiniz. Service Fabric uygulamaları tarafından kullanıcı hesapları altında--dağıtım zamanında örneğin kullanılan kaynaklar, dosyaları, dizinleri ve sertifikaları güvenli yardımcı olur. Bu çalışan uygulamaları bile paylaşılan bir barındırılan ortamda, diğerinden daha güvenli hale getirir.  Bilgi edinmek için nasıl [uygulamanız için güvenlik ilkelerini yapılandırmak](service-fabric-application-runas-security.md).

Uygulamanızı depolama bağlantı dizeleri, parolalar veya düz metin olarak işleneceğini olmayan diğer değerleri gibi hassas bilgiler içerebilir. Bilgi edinmek için nasıl [uygulamanızda parolaları yönetme](service-fabric-application-secret-management.md).

## <a name="learn-about-the-application-lifecycle"></a>Uygulama yaşam döngüsü hakkında bilgi edinin
Diğer platformlar ile bir Service Fabric uygulaması genellikle aşağıdaki aşamaları geçtikçe: tasarım, geliştirme, test, dağıtım, yükseltme, Bakım ve kaldırma. [Bu makalede](service-fabric-application-lifecycle.md) API'ler ve Service Fabric uygulama yaşam döngüsü aşamaları boyunca farklı rolleri tarafından nasıl kullanıldıkları hakkında genel bir bakış sağlar.

## <a name="next-steps"></a>Sonraki adımlar
- [Windows Küme oluşturmayı](service-fabric-tutorial-create-vnet-and-windows-cluster.md).
- Dağıtılan uygulamalar ve fiziksel düzenini dahil olmak üzere kümenizi görselleştirme [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
- [Sürümü ve hizmetlerinizi yükseltme](service-fabric-application-upgrade-tutorial.md)


