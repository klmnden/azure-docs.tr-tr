---
title: Microsoft Azure'da izleme | Microsoft Docs
description: "Microsoft Azure içindeki herhangi bir şey izlemek istediğiniz zaman Seçenekler. Azure monitör, Application Insights ve günlük analizi"
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1b962c74-8d36-4778-b816-a893f738f92d
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2017
ms.author: robb
ms.openlocfilehash: c34211e0c55c10defaa32f1e0a2195514ff3ae5f
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2017
---
# <a name="overview-of-monitoring-in-microsoft-azure"></a>Microsoft Azure'da izleme genel bakış
Bu makalede, araçları ve Hizmetleri bütünsel Microsoft Azure izlemede ilgili genel bakış sağlar. İçin geçerlidir:
- Azure altyapı ve uygulamaları izlemek için Azure services'ı kullanarak
- İzleyici karma ve Azure dışı altyapısı ve uygulamaları için Azure hizmetlerini kullanarak
- Azure altyapı ve uygulamaları izlemek için Azure olmayan hizmetleri kullanma

Bu makalede ele çeşitli ürünlerin ve hizmetlerin kullanılabilir ve bunlar birlikte nasıl çalışır. Hangi araçların hangi durumlarda sizin için en uygun olduğunu belirlemenize yardımcı olabilir.  

## <a name="why-use-azures-monitoring-services"></a>Azure'nın izleme hizmetler neden kullanılır?

Performans sorunlarını bulut uygulamanızda iş etkileyebilir. Birden çok birbirine bağlı bileşenleri ve sık sürümleri ile degradations herhangi bir zamanda oluşabilir. Ve kullanıcılarınızın bir uygulama geliştiriyorsanız, genellikle testinde bulamadı sorunları keşfedin. Bu hemen bilmeniz ve sorunlarını tanılamak ve araçları olması gerekir. Ayrıca, Azure ortamınıza izleme anahtarını bütüncül bir uygulama ve altyapı sahip olacak şekilde uygulamanızdaki sorunları bu uygulamaların çalıştığı, temel alınan altyapısından neden olabilir. Microsoft Azure çeşitli tanımlayan ve bu tür sorunlarını çözmek için araçlar vardır.

## <a name="how-do-i-monitor-my-azure-environment"></a>My Azure ortamı nasıl izlerim?

Çeşitli Azure ortamınızdan, uygulamanızı barındırma altyapısını ve Hizmetleri için Azure üzerinde çalışan uygulama koduna izleme için araçlar vardır. Bu araçlar, kapsamlı bulut izleme sunar ve dahil etmek için birlikte çalışır:

-   **Azure İzleyici** -tüm izleme verilerini Azure Hizmetleri için birleştirilmiş bir potansiyel olarak çalışır Azure hizmeti. Performans ölçümleri ve Azure altyapı ve kullandığınız herhangi bir Azure hizmeti çalışmasını açıklayan olaylar erişim sağlar. Azure İzleyici Azure ortamınız için izleme bir veri ardışık düzeni ve bu verileri doğrudan günlük analizi ve bunun yanı sıra, bu verileri kavramanıza ve verileri şirket içi veya Bulut kaynaklar ile birleştirerek 3. taraf araçları sunar.

-   **Application Insights** -uygulama performans izleme ve kullanıcı analizi sunar Azure hizmeti. Azure üzerinde dağıtılan uygulamalar ve, yazılan kod izler şirket içi ya da diğer bulut. Application Insights SDK'sı ile uygulamanızı işaretlenerek bağımlılıkları, özel durum izlemeleri, hata ayıklama anlık görüntüler ve yürütme profilleri yanıt sürelerini de dahil olmak üzere veri aralığını erişimi elde edebilirsiniz. Bu uygulama telemetri geliştirmek ve uygulamanızı işletim çözümlemek için güçlü araçlar sağlar. İç, giderebilmemiz kod sorunu satır sağ almanızı sağlayan için Visual Studio ile tümleşir ve uygulamalarınızı ürün yöneticileri için müşteri kullanımını analiz etmek için kullanım analizi sunar.

-   **Günlük analizi** -önceden OMS günlük analizi bilinen, Azure Hizmetleri (aracılığıyla Azure İzleyici), Azure Vm'leri ve şirket içi ya da diğer bulut altyapısı günlük ve ölçüm verileri alır ve esnek günlük arama sunan bir Azure hizmetidir ve çıkış-in- Bu veriler üzerinde analiz kutusu. Kaynakları genelinde verileri çözümlemek için zengin araçları tüm günlükleri karmaşık sorgular sağlar ve belirtilen koşullara proaktif olarak uyarabilir sağlar.  Sorgulamak ve onu görselleştirmek için özel veri merkezi depoya bile toplayabilirsiniz. Ayrıca, hemen güvenlik Öngörüler elde etmek için günlük analitik'ın yerleşik çözümleri avantajlarından ve altyapınızı işlevselliğini alabilir.

## <a name="accessing-monitoring-in-the-azure-portal"></a>Azure portalında izleme erişme
Tüm Azure izleme hizmetleri tek bir UI bölmesinde kullanıma sunulmuştur. Bu alan erişim hakkında daha fazla bilgi için bkz: [Azure İzleyicisi ile çalışmaya başlama](monitoring-get-started.md). 

İzleme işlevleri belirli Azure kaynakları için bu kaynakları vurgulama ve bunların izleme seçenekleri araştırıp de erişebilir. 

## <a name="examples-of-when-to-use-which-tool"></a>Hangi aracı kullanmak ne zaman örnekleri 

Aşağıdaki bölümlerde bazı temel senaryolar ve hangi araçların birlikte kullanılması gerektiğini gösterir. 

### <a name="scenario-1--fix-errors-in-an-azure-application-under-development"></a>Senaryo 1 – Azure uygulaması geliştirme altında düzeltme hataları   

**Application Insights, Azure İzleyici ve Visual Studio birlikte kullanmak için en iyi seçenektir**

Azure, Visual Studio hata ayıklayıcısı bulutta gücünü şimdi sağlar. Azure Application Insights telemetri göndermeyi İzleyici yapılandırın. Visual Studio Application Insights SDK'sı, uygulamanızda içerecek şekilde etkinleştirin. Bir kez Application Insights'ta görsel olarak çalışan uygulamanızın hangi parçalarını veya sağlıklı olduğunu öğrenmek için uygulama eşlemesi kullanabilirsiniz. Bölümlerinde her zaman iyi durumda olmayan hataları ve özel durumları zaten araştırması için kullanılabilir. Application Insights'ta çeşitli analytics derinlemesine için kullanabilirsiniz. Hata hakkında emin değilseniz, başka bir sorun kodu ve PIN noktasına izlemek için Visual Studio hata ayıklayıcısı kullanabilirsiniz. 

Daha fazla bilgi için bkz: [Web uygulamaları izleme](../application-insights/app-insights-azure-web-apps.md) ve uygulamaları ve dilleri çeşitli türleri hakkında yönergeler için soldaki içindekiler tablosu bakın.  

### <a name="scenario-2--debug-an-azure-net-web-application-for-errors-that-only-show-in-production"></a>Senaryo 2 – bir Azure .NET web uygulaması yalnızca üretimde Göster hataları için hata ayıklama 

> [!NOTE]
> Bu özellikler önizlemede. 

**Application Insights ve olası Visual tam hata ayıklama için Studio karşılaşırsanız, en iyi seçenek kullanmaktır.**

Uygulamanızın hatalarını ayıklama için uygulama Öngörüler anlık görüntü hata ayıklayıcı kullanın. Belirli bir hata eşiği üretim bileşenleriyle oluştuğunda sistem Windows "anlık görüntüler." olarak adlandırılan süreyi telemetri otomatik olarak yakalar. Yakalanan performans etkilememesi için küçük olduğundan güvenli bir üretim bulut ancak izleme izin vermek yeterince önemli miktarıdır.  Sistem, birden çok anlık görüntü yakalayabilirsiniz. Azure portalında zaman içinde bir noktada arayın veya tam deneyimi için Visual Studio'yu kullanın. Gerçek zamanlı olarak debugging gibi Visual Studio ile geliştiriciler bu anlık görüntü yol. Yerel değişkenler, parametreleri, bellek ve çerçeveleri tüm kullanılabilir. Geliştiriciler bu üretim verilere erişim verilmelidir bir [RBAC rolü](../active-directory/role-based-access-built-in-roles.md).  

Daha fazla bilgi için bkz: [anlık görüntü hata ayıklama](../application-insights/app-insights-snapshot-debugger.md). 

### <a name="scenario-3--debug-an-azure-application-that-uses-containers-or-microservices"></a>Senaryo 3 – kapsayıcıları veya mikro kullanan bir Azure uygulama hata ayıklama 

**Senaryo 1 ile aynıdır. Application Insights, Azure İzleyici ve Visual Studio birlikte kullanın**

Application Insights da toplama telemetri kapsayıcılarda çalışan işlemleri ve mikro (Kubernetes, Docker, Azure Service Fabric) destekler. Daha fazla bilgi için [kapsayıcıları ve mikro hata ayıklamayı bu video bkz](https://go.microsoft.com/fwlink/?linkid=848184). 


### <a name="scenario-4--fix-performance-issues-in-your-azure-application"></a>Senaryo 4 – Azure uygulamanızda düzeltme performans sorunları

[Application Insights profil oluşturucu](../application-insights/app-insights-profiler.md) bu tür sorunları gidermenize yardımcı olması için tasarlanmıştır. Tanımlamak ve işlem kaynaklarını sanal makineleri, sanal makine ölçek kümeleri (VMSS) bulut Hizmetleri gibi uygulama Hizmetleri (Web uygulamaları, Logic Apps, Mobile Apps, API uygulamaları, işlevi uygulamalar) ve diğer çalışan uygulamalar için performans sorunlarını ve Service Fabric. 

> [!NOTE]
> Profili sanal makineler, sanal makine ölçek kümeleri (VMSS) bulut Hizmetleri ve Hizmetleri doku yeteneği önizlemede değil.   

Ayrıca, önceden belirli türde bir yavaş sayfa yükleme süreleri gibi hatalar hakkında e-postayla akıllı algılama aracı tarafından bildirilir.  Bu araç hakkında herhangi bir yapılandırma gerekmez. Daha fazla bilgi için bkz: [akıllı algılama - performans Anormalliklerini](../application-insights/app-insights-proactive-performance-diagnostics.md).



## <a name="next-steps"></a>Sonraki adımlar
Şu konular hakkında daha fazla bilgi edinin:

* [Azure İzleyicisi'nde Ignite 2016'den video](https://myignite.microsoft.com/videos/4977)
* [Azure İzleyicisi ile çalışmaya başlama](monitoring-get-started.md)
* [Azure tanılama](../azure-diagnostics.md) çalışıyorsanız, bulut hizmeti, sanal makine sorunlarını tanılamak sanal makine ölçeklendirme ayarlayın veya Service Fabric uygulaması.
* [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) App Service Web uygulamanızda tanılama sorunları çalışıyorsanız.
* [Günlük analizi](https://azure.microsoft.com/documentation/services/log-analytics/) ve [Operations Management Suite](https://www.microsoft.com/oms/) üretim izleme çözümü
