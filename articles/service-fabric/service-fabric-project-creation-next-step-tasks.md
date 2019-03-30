---
title: Service Fabric projesi oluşturma işleminde sonraki adımlar | Microsoft Docs
description: Visual Studio'da oluşturduğunuz uygulama projesi hakkında bilgi edinin.  Öğreticiler kullanan hizmetler oluşturmayı öğrenin ve Hizmetleri için Service Fabric geliştirme hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: ''
ms.assetid: 299d1f97-1ca9-440d-9f81-d1d0dd2bf4df
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/07/2017
ms.author: atsenthi
ms.openlocfilehash: e5371cd3ea9de1993f0f824325f6cbf1e25343d4
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58667936"
---
# <a name="your-service-fabric-application-and-next-steps"></a>Service Fabric uygulaması ve sonraki adımlar
Azure Service Fabric uygulamanızı oluşturuldu. Bu makalede, bazı öğreticiler denemek için projenizi, bazı ek bilgiler de ilginizi çekebilir ve olası sonraki adımlar düzenini açıklar.

## <a name="get-started-with-tutorials-walk-throughs-and-samples"></a>Öğreticiler, Kılavuzlar ve örnekler ile çalışmaya başlama
Başlamaya hazır mısınız?  

.NET uygulaması Öğreticisi ile çalışır. Bilgi nasıl [uygulama oluşturma](service-fabric-tutorial-create-dotnet-app.md) ASP.NET Core ön ucuyla ve bir durum bilgisi olan arka uç, [uygulamayı dağıtma](service-fabric-tutorial-deploy-app-to-party-cluster.md) bir kümeye [CI/CD'yi yapılandırma](service-fabric-tutorial-deploy-app-with-cicd-vsts.md), ve [ayarlayın İzleme ve tanılama](service-fabric-tutorial-monitoring-aspnet.md).

Veya, bir aşağıdaki incelemelerde deneyin ve ilk kez oluşturma...
- [C#Windows üzerinde Reliable Services hizmeti](service-fabric-reliable-services-quick-start.md) 
- [C#Windows üzerinde Reliable Actors hizmeti](service-fabric-reliable-actors-get-started.md) 
- [Windows üzerinde Konuk yürütülebilir hizmet](quickstart-guest-app.md) 
- [Windows kapsayıcı uygulaması](service-fabric-get-started-containers.md) 

Ayrıca aşımına ilginizi çekebilir bizim [örnek uygulamalar](https://aka.ms/servicefabricsamples).

## <a name="have-questions-or-feedback--need-to-report-an-issue"></a>Sorularınız veya Geri bildiriminiz var mı?  Bir sorun bildirmek mı ihtiyacınız var?
Okumak [sık sorulan sorular](service-fabric-common-questions.md) ve Service Fabric neler yapabileceğinizi ve nasıl kullanılmalıdır sorulara yanıtlar bulun.

[Destek Seçenekleri](service-fabric-support.md) raporlama sorunları, destek almak ve ürün geri bildirim gönderiliyor seçenekleri yanı sıra soru sormaya yönelik StackOverflow ve MSDN Forumlarında listeler.

## <a name="the-application-project"></a>Uygulama projesi
Her yeni uygulama, bir uygulama projesi içerir. Bir veya iki ek projeleri, seçilen hizmet türüne bağlı olarak olabilir.

Uygulama projesini oluşur:

* Uygulamanızı oluşturan Hizmetleri başvurular kümesi.
* Üç yayımlama--varsayılan olarak bir küme uç noktası ve yükseltme dağıtımlarını gerçekleştirilip ilgili tercihleri gibi farklı ortamlarda çalışan tercihlerini korumak için kullanabileceğiniz profilleri (1 düğümlü yerel 5 düğümlü yerel ve bulut).
* (Bir hizmet için oluşturulacak bölüm sayısı gibi bir ortama özgü uygulama yapılandırmaları sağlamak için kullanabileceğiniz aynı yukarıda) üç uygulama parametresi dosyaları. Bilgi edinmek için nasıl [birden çok ortam için uygulamanızı yapılandırma](service-fabric-manage-multiple-environment-app-configuration.md).
* Komut satırından veya otomatikleştirilmiş bir sürekli tümleştirme ve dağıtım ardışık bir parçası olarak uygulamanızı dağıtmak için kullanabileceğiniz bir dağıtım betiği. Daha fazla bilgi edinin [PowerShell kullanarak dağıtmaya](service-fabric-deploy-remove-applications.md).
* Uygulama bildirimi uygulamayı açıklar. Bildirim ApplicationPackageRoot klasörü altında bulabilirsiniz. Daha fazla bilgi edinin [uygulaması ve hizmet bildirimleri](service-fabric-application-model.md).



## <a name="learn-more-about-the-programming-models"></a>Programlama modelleri hakkında daha fazla bilgi edinin
Service Fabric, yazma ve hizmetlerinizi yönetmek için birden çok yol sunar.  İşte genel bakış ve kavramsal bilgileri [durum bilgisiz ve durum bilgisi olan Reliable Services](service-fabric-reliable-services-introduction.md), [Reliable Actors](service-fabric-reliable-actors-introduction.md), [kapsayıcıları](service-fabric-containers-overview.md), [konuk tarafından yürütülebilir uygulama ](service-fabric-guest-executables-introduction.md), ve [durum bilgisiz ve durum bilgisi olan ASP.NET Core Hizmetleri](service-fabric-reliable-services-communication-aspnetcore.md).

## <a name="learn-about-service-communication"></a>Hizmet iletişim hakkında bilgi edinin
Service Fabric uygulaması farklı Hizmetleri, burada her hizmeti özel bir görev gerçekleştirir oluşur. Bu hizmetlerin birbirleriyle iletişim kurabilir ve bağlanın ve hizmetlerle iletişim kuran istemci uygulamaları küme dışında olabilir. Bilgi nasıl [ile ve hizmetleriniz arasında iletişim ayarlama](service-fabric-connect-and-communicate-with-services.md) Service fabric'te. 

## <a name="learn-about-configuring-application-security"></a>Uygulama güvenliği yapılandırma hakkında bilgi edinin
Kümedeki farklı bir kullanıcı hesabı altında çalışan uygulamaların güvenliğini sağlayabilirsiniz. Service Fabric uygulamaları tarafından kullanıcı hesaplarını altında--dağıtım zamanında örneğin kullanılan kaynaklar, dosyaları, dizinleri ve sertifikaları güvenli yardımcı olur. Bu çalışan uygulamalar da paylaşılan bir barındırılan ortamda, diğerinden daha güvenli hale getirir.  Bilgi edinmek için nasıl [uygulamanıza yönelik güvenlik ilkeleri yapılandırma](service-fabric-application-runas-security.md).

Uygulamanızı depolama bağlantı dizeleri, parolalar veya düz metin olarak işleneceğini değil diğer değerleri gibi hassas bilgiler içerebilir. Bilgi edinmek için nasıl [uygulamanızdaki gizli dizileri Yönet](service-fabric-application-secret-management.md).

## <a name="learn-about-the-application-lifecycle"></a>Uygulama yaşam döngüsü hakkında bilgi edinin
Diğer platformlar ile Service Fabric uygulaması genellikle aşağıdaki aşamaları geçtikçe: tasarım, geliştirme, test, dağıtım, yükseltme, Bakım ve kaldırma. [Bu makalede](service-fabric-application-lifecycle.md) API'leri ve Service Fabric uygulama yaşam döngüsünün aşamaları boyunca farklı rolleri tarafından nasıl kullanıldıkları hakkında genel bir bakış sağlar.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure'da bir Windows kümesi oluşturma](service-fabric-tutorial-create-vnet-and-windows-cluster.md).
- Dağıtılmış uygulamalar ve fiziksel düzenini dahil olmak üzere kümenizi görselleştirme [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
- [Sürüm ve hizmetlerinizi yükseltme](service-fabric-application-upgrade-tutorial.md)


