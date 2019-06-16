---
title: Service Fabric ve kapsayıcılar'na genel bakış | Microsoft Docs
description: Mikro hizmet uygulamalarını dağıtmak için Service Fabric ve kapsayıcılar kullanımına genel bakış. Bu makalede, kapsayıcıları nasıl kullanılabileceğini genel bir bakış ve Service fabric'te kullanılabilen özellikleri sağlar.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: c98b3fcb-c992-4dd9-b67d-2598a9bf8aab
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/8/2018
ms.author: aljo
ms.openlocfilehash: 5a45f14e5ac1da5152f320bd92b1ebb42be1d214
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60881439"
---
# <a name="service-fabric-and-containers"></a>Service Fabric ve kapsayıcılar

## <a name="introduction"></a>Giriş

Azure Service Fabric; ölçeklenebilir ve güvenilir mikro hizmetleri ve kapsayıcıları paketlemeyi, dağıtmayı ve yönetmeyi kolaylaştırmayı sağlayan bir dağıtılmış sistemler platformudur.

Service Fabric, Microsoft'un [kapsayıcı Düzenleyicisi](service-fabric-cluster-resource-manager-introduction.md) bir makine kümesindeki mikro hizmetler dağıtmak için. Service Fabric avantajları Hizmetleri çok büyük ölçekte Microsoft'ta çalışan, yıl boyunca derslerden.

Mikro hizmetler, [Service Fabric programlama modelleri](service-fabric-choose-framework.md), [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md) kullanmaktan [tercih ettiğiniz herhangi bir kodun](service-fabric-guest-executables-introduction.md) dağıtılmasına kadar birçok yolla geliştirilebilir. Veya yalnızca isterseniz [kapsayıcıları dağıtmak ve yönetmek](service-fabric-containers-overview.md), Service Fabric, ayrıca harika bir seçim.

Varsayılan olarak, Service Fabric dağıtır ve hizmetlerin işlemler olarak etkinleştirir. Bir kümedeki kaynak yoğunluklu kullanımı en yüksek ve en hızlı etkinleştirme işlemleri sağlar. Service Fabric, kapsayıcı görüntüleri Hizmetleri'nde olarak da dağıtabilirsiniz. Ayrıca, hizmetleri ve kapsayıcıları, aynı uygulama Hizmetleri'nde karıştırabilirsiniz.

Hemen başlayın ve Service Fabric kapsayıcıları denemek için hızlı başlangıç, öğretici ve örnek deneyin:  

[Hızlı Başlangıç: Bir Service Fabric Linux kapsayıcı uygulaması dağıtma](service-fabric-quickstart-containers-linux.md)  
[Hızlı Başlangıç: Service Fabric için bir Windows kapsayıcı uygulaması dağıtma](service-fabric-quickstart-containers.md)  
[Mevcut bir .NET uygulamasını kapsayıcılı hale getirme](service-fabric-host-app-in-a-container.md)  
[Service Fabric Kapsayıcı Örnekleri](https://azure.microsoft.com/resources/samples/service-fabric-containers/)  

## <a name="what-are-containers"></a>Kapsayıcıları nelerdir

Kapsayıcılar, güvenilir bir şekilde uygulamaları çalıştırmak bir uygulama için sabit bir ortam sağlayarak farklı bilgi işlem ortamlarında çalışan sorununu çözün. Kapsayıcılar, 'yazılım kapsayıcı içinde çalıştırmak için gereken her şeyi içeren, kendi yalıtılmış kutusuna' bir uygulama ve tüm bağımlılıklarını kitaplıkları ve yapılandırma dosyaları gibi kaydırın. Kapsayıcıyı çalıştıran her yerde, her zaman uygulama içindeki doğru sürümlerini, bağımlı kitaplıkları, tüm yapılandırma dosyalarını ve çalışması için gereken başka bir şey gibi çalışması için gereken her şeyi sahiptir.

Kapsayıcılar, doğrudan çekirdek üzerinde çalıştırma ve dosya sistemi ve diğer kaynakları yalıtılmış bir görünüm. Bir kapsayıcıdaki bir uygulama, diğer tüm uygulamalar ve süreçler, kapsayıcısı dışındaki olanağıyla sahiptir. Her uygulama ve çalışma zamanı, bağımlılıklar ve sistem kitaplıklarını bir kapsayıcı içinde tam ve özel erişim ile işletim sisteminin yalıtılmış kapsayıcının kendi görünümüne çalıştırın. Hale getirmenin yanı sıra tüm uygulama bağımlılıklarınızın farklı bilgi işlem ortamında çalıştırmak için ihtiyaç duyduğu sağlamak güvenlik ve kaynak yalıtımı ile Service Fabric'i--aksi çalıştığı Hizmetleri, kapsayıcılar kullanmanın önemli avantajları kolaydır bir işlem.

Sanal makinelere kıyasla, kapsayıcılar, şu avantajlara sahip olursunuz:

* **Küçük**: Kapsayıcılar, verimliliği artırmak için tek bir depolama alanı ve katman sürümler ve Güncelleştirmeler'ni kullanın.
* **Hızlı**: Kapsayıcılar, bunlar çok daha hızlı--genellikle saniye içinde analize başlamanızı bütün bir işletim sistemi önyükleme gerekmez.
* **Taşınabilirlik**: Kapsayıcılı uygulama görüntüsü, bulutta, şirket içi, sanal makineler içinde veya fiziksel makinelere doğrudan çalıştırmak için unity'nin.
* **Kaynak İdaresi**: Bir kapsayıcı, konakta tüketebileceği fiziksel kaynakları sınırlayabilirsiniz.

### <a name="container-types-and-supported-environments"></a>Kapsayıcı türü ve desteklenen ortamlar

Service Fabric, hem Linux hem de Windows kapsayıcılarını destekleyen ve Windows üzerinde Hyper-V yalıtım modunu destekler.

#### <a name="docker-containers-on-linux"></a>Linux'ta docker kapsayıcıları

Docker, oluşturmak ve Linux çekirdek kapsayıcıları üzerine kapsayıcıları yönetmek için API'ler sağlar. Docker Hub depolamak ve kapsayıcı görüntüleri almak için merkezi bir depo sunar.
Linux tabanlı bir öğretici için bkz. [Linux'ta ilk Service Fabric kapsayıcı uygulamanızı oluşturma](service-fabric-get-started-containers-linux.md).

#### <a name="windows-server-containers"></a>Windows Server kapsayıcıları

Windows Server 2016, iki farklı yalıtım düzeyine göre farklı kapsayıcılar sağlar. Windows Server kapsayıcıları ve Docker kapsayıcıları hem de çekirdek çalıştırıldıkları konak ile paylaşım sırasında ad alanı ve dosya sistemi yalıtım olduğundan benzerdir. Linux üzerinde bu yalıtım, geleneksel olarak cgroups ve ad alanları tarafından sağlanmıştır ve Windows Server kapsayıcıları benzer şekilde davranır.

Hiçbir kapsayıcı başka bir kapsayıcı veya ana bilgisayar ile işletim sistemi çekirdeği paylaştığından Windows kapsayıcılarını Hyper-V desteği ile daha fazla yalıtım ve güvenlik sağlar. Bu daha yüksek düzeyde güvenlik yalıtımı ile Hyper-V etkin kapsayıcıları potansiyel olarak tehlikeli, çok kiracılı senaryolarda hedefler.
Windows tabanlı bir öğretici için bkz. [Windows üzerinde ilk Service Fabric kapsayıcı uygulamanızı oluşturma](service-fabric-get-started-containers.md).

Aşağıdaki şekilde sanallaştırmasının ve yalıtım düzeyleri farklı türleri gösterilmektedir.
![Service Fabric platformu][Image1]

## <a name="scenarios-for-using-containers"></a>Kapsayıcıları kullanmaya yönelik senaryolar

Burada bir kapsayıcı iyi bir seçimdir tipik örnekler şunlardır:

* **IIS lift- and -shift**: Mevcut bir koyabilirsiniz [ASP.NET MVC](https://www.asp.net/mvc) geçirmek yerine, bir kapsayıcıdaki uygulama ASP.NET Core için. Bu ASP.NET MVC uygulamaları, Internet Information Services (IIS) bağlıdır. Bu kapsayıcı görüntülerini uygulamalarla precreated IIS görüntüden paketini ve Service Fabric ile dağıtın. Bkz: [kapsayıcı görüntülerini Windows Server'da](https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-server) Windows kapsayıcıları hakkında daha fazla bilgi için.

* **Kapsayıcıları ve Service Fabric mikro hizmetler karıştırmak**: Var olan bir kapsayıcı görüntüsü, uygulamanızın bir parçası için kullanın. Örneğin, kullanabileceğinize [NGINX kapsayıcısını](https://hub.docker.com/_/nginx/) web ön uç uygulama ve daha yoğun bir arka uç hesaplama için durum bilgisi olan hizmetler için.

* **"Gürültülü komşu" Hizmetleri etkisini azaltmak**: Kaynak İdaresi özelliği kapsayıcıların bir konakta bir hizmetin kullandığı kaynakları kısıtlamak için kullanabilirsiniz. Hizmetleri pek çok kaynak tüketen ve başkalarının (örneğin, uzun süre çalışan, sorgu benzeri işlemi) performansını etkiler, bu hizmetler kaynak İdaresi kapsayıcılarına yerleştirme göz önünde bulundurun.

## <a name="service-fabric-support-for-containers"></a>Kapsayıcıları Service Fabric desteği

Service Fabric Linux üzerinde Docker kapsayıcıları ve Windows Server kapsayıcıları dağıtımını desteğinin yanı sıra Hyper-V yalıtım modunda Windows Server 2016'da destekler. 

Service Fabric sağlar bir [uygulama modeli](service-fabric-application-model.md) hangi birden çok çoğaltmaları hizmete bir uygulama konağı bir kapsayıcıyı temsil eden içinde. Service Fabric ayrıca destekleyen bir [Konuk yürütülebilir senaryo](service-fabric-guest-executables-introduction.md) , yerleşik Service Fabric programlama modellerini kullanmayın, ancak bunun yerine içinde herhangi bir dil veya framework kullanılarak yazılmış mevcut bir uygulama paketi içindeki bir kapsayıcı. Kapsayıcılar için yaygın kullanım örneği senaryodur.

Ayrıca çalıştırabileceğiniz [bir kapsayıcı içinde Service Fabric Hizmetleri](service-fabric-services-inside-containers.md). Kapsayıcıların içinde çalışan Service Fabric Hizmetleri için destek sınırlıdır.

Service Fabric, kapsayıcılı mikro hizmetler gibi hizmetlerden oluşturulmuş uygulamalara yardımcı olacak birkaç kapsayıcı özellikleri oluşturmanızı sağlar:

* Kapsayıcı görüntü dağıtımının ve etkinleştirmesinin.
* Kaynak değerler varsayılan olarak Azure kümelerinde ayarlanması dahil olmak üzere kaynak İdaresi.
* Depo kimlik doğrulaması.
* Kapsayıcı bağlantı noktasından konak bağlantı noktası eşlemesi.
* İletişim ve kapsayıcı kapsayıcı bulma.
* Yapılandırma ve ortam değişkenlerini ayarlama yeteneği.
* Kapsayıcıdaki güvenlik kimlik bilgilerini ayarlama olanağı.
* Kapsayıcılar için farklı ağ modları seçimi.

Kapsayıcı kapsamlı bir bakış için destek, Azure'da Azure Kubernetes hizmeti ile nasıl bir Kubernetes kümesi oluşturmak nasıl gibi Azure Container Registry ve daha fazla özel bir Docker kayıt defteri oluşturmak için bkz [kapsayıcılariçinAzure](https://docs.microsoft.com/azure/containers/).

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Service Fabric kapsayıcıları çalıştırmak için sağladığı desteği hakkında bilgi edindiniz. Ardından, bunların nasıl kullanılacağını göstermek için özelliklerin her biri örnekleri ele alacağız.

[Linux'ta ilk Service Fabric kapsayıcı uygulamanızı oluşturma](service-fabric-get-started-containers-linux.md)  
[Windows üzerinde ilk Service Fabric kapsayıcı uygulamanızı oluşturma](service-fabric-get-started-containers.md)  
[Windows kapsayıcıları hakkında daha fazla bilgi edinin](https://docs.microsoft.com/virtualization/windowscontainers/about/)

[Image1]: media/service-fabric-containers/Service-Fabric-Types-of-Isolation.png