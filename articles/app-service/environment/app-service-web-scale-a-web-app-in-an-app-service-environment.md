---
title: Bir uygulama hizmeti ortamı'nda bir uygulama ölçeklendirme
description: Bir uygulama hizmeti ortamı'nda bir uygulama ölçeklendirme
services: app-service
documentationcenter: ''
author: ccompy
manager: stefsch
editor: jimbe
ms.assetid: 78eb1e49-4fcd-49e7-b3c7-f1906f0f22e3
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2016
ms.author: ccompy
ms.openlocfilehash: d04a5fce920dae25507cdf2f64832574e24c51dd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23836556"
---
# <a name="scaling-apps-in-an-app-service-environment"></a>App Service Ortamında uygulamaları ölçeklendirme
Azure App Service'te normal şekilde ölçeklendirebilirsiniz üç şey vardır:

* plan fiyatlandırması
* çalışan boyutu 
* örnek sayısı.

ASE'de seçmek veya fiyatlandırma planı değiştirmek için gerek yoktur.  Özellikleri bakımından Premium yetenek düzeyi fiyatlandırma zaten var.  

Çalışan boyutları göre ana yönetim için her çalışan havuzunda kullanılacak işlem kaynak boyutu atayabilirsiniz.  P4 işlem kaynakları ile çalışan havuzu 1 olabilir ve çalışan Havuz 2 P1 ile işlem kaynakları, isterseniz anlamına gelir.  Boyutu olması gerekmez.  Belgeyi burada boyutları ve bunların fiyatlandırma ayrıntıları görmek için [Azure App Service fiyatlandırması][AppServicePricing].  Bu, bir uygulama hizmeti olarak ortamında web uygulamaları ve App Service planları için ölçeklendirme seçenekleri bırakır:

* çalışan havuzu seçimi
* örnek sayısı

Her iki öğeyi değiştirme App Service planları, ana barındırılan için gösterilen uygun kullanıcı Arabirimi aracılığıyla gerçekleştirilir.  

![][1]

ASP bulunduğu çalışan havuzunda kullanılabilir işlem kaynakları sayısı ötesinde, ASP ölçeklendirin olamaz.  İşlem kaynaklarını çalışan havuzda varsa bunları eklemek için ana yöneticinize almanız gerekir.  Burada yer alan bilgiler, ana yeniden yapılandırma geçici bilgileri okumak için: [bir uygulama hizmeti ortamını yapılandırma][HowtoConfigureASE].  Zamanlama veya ölçümleri kapasite göre eklemek için ana otomatik ölçeklendirme özelliklerden yararlanmak isteyebilirsiniz.  Yapılandırma hakkında daha fazla bilgi almak için otomatik ölçeklendirme ana ortamı için bkz: [otomatik ölçeklendirme bir uygulama hizmeti ortamı için yapılandırma][ASEAutoscale].

Birden çok uygulama farklı çalışan havuzlarından işlem kaynakları kullanılarak hizmet planları oluşturabilir veya aynı çalışan havuzu kullanabilirsiniz.  (4) kullanan çalışan havuzu 1'de (10) kullanılabilir işlem kaynakları varsa, (6) işlem kaynaklarını kullanan bir uygulama hizmeti planı oluşturmak seçebilirsiniz ve ikinci bir app service planı örneğin işlem kaynaklarını.

### <a name="scaling-the-number-of-instances"></a>Örnek sayısı ölçeklendirme
Uygulama hizmeti ortamı'nda ilk web uygulamanızı oluşturduğunuzda ile 1 bir örneğini başlatır.  Ardından çıkışı uygulamanız için ek işlem kaynaklarını sağlamaya ek örnekleri ölçekleme yapabilirsiniz.   

Ana yeterli kapasitesi varsa, daha sonra bu oldukça basittir.  Uygulama hizmeti ölçeği artırma ve ölçek seçmek istediğiniz siteleri tutan planınız için gidin.  Bu, burada el ile ölçek, ASP ayarlayabilir veya ASP için otomatik ölçeklendirme kurallarını yapılandırma kullanıcı arabirimini açar.  El ile uygulamanız yeterlidir ölçeklendirme için ayarlanmış ***göre Ölçeklendirmeniz*** için ***el ile girdiğim bir örnek sayısı***.  Buradan istenen miktar kaydırıcıyı sürükleyin ya da kaydırıcıyı yanındaki kutuya girin.  

![][2] 

Bunlar her zamanki gibi bir ana bir ASP için otomatik ölçeklendirme kurallarını aynı çalışır.  Seçebileceğiniz ***CPU yüzdesi*** altında ***göre Ölçeklendirmeniz*** ve CPU yüzdesi veya, göre ASP kullanarak daha karmaşık kurallar oluşturabilirsiniz için otomatik ölçeklendirme kuralları oluşturmak ***zamanlama ve performans kuralları*** .  Yapılandırma hakkında daha kapsamlı ayrıntıları görmek için otomatik ölçeklendirme kullanmak Kılavuzu burada [bir uygulama Azure App Service'te ölçeklendirme][AppScale]. 

### <a name="worker-pool-selection"></a>Çalışan havuzu seçimi
Daha önce belirtildiği gibi çalışan havuzu seçimi ASP Arabiriminden erişilir.  Ölçek ve çalışan havuzunda seçmek için istediğiniz ASP için dikey penceresini açın.  Tüm uygulama hizmeti ortamı'nda yapılandırılan çalışan havuzlarında görürsünüz.  Yalnızca bir çalışan havuzu varsa listelenen bir havuz yalnızca görürsünüz.  ASP bulunduğu hangi çalışan havuzunu değiştirmek için yalnızca uygulama hizmeti taşımak için planınız istediğiniz çalışan havuzu seçin.  

![][3]

ASP bir çalışan havuzundan diğerine geçmeden önce ASP için yeterli kapasiteye sahip emin olmak önemlidir.  Çalışan havuzlarında listesinde yalnızca çalışan havuzu adı listelenen ancak, kaç tane çalışanları, çalışan havuzunda kullanılabilir olduğunu görebilirsiniz.  Yeterli örnekleri içeren uygulama hizmeti planınız kullanılabilir olduğundan emin olun.  Daha fazla işlem kaynaklarını taşımak istediğiniz çalışan havuzunda varsa, bunları eklemek için ana yöneticinize alın.  

> [!NOTE]
> Bu ASP uygulamalarının bir çalışan havuzundan bir ASP soğuk neden olacak taşıma başlatır.  Bu istekleri uygulamanızı yeni işlem kaynakları üzerinde çalışmaya soğuk olarak yavaş çalışmasına neden olabilir.  Soğuk başlangıç kullanılarak önlenebilir [uygulama yeteneğini sıcak] [ AppWarmup] Azure App Service'te.  Yeni işlem kaynakları üzerinde çalışmaya uygulamaları soğuk olduğunda başlatma işlemi de çağrılan çünkü makalede açıklanan uygulama başlatma modül soğuk başlıyor de çalışır. 
> 
> 

## <a name="getting-started"></a>Başlarken
Uygulama hizmeti ortamları ile çalışmaya başlamak için bkz: [nasıl için oluşturma bir uygulama hizmeti ortamı][HowtoCreateASE]

<!--Image references-->
[1]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-aspblade.png
[2]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-manualscale.png
[3]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-sizescale.png

<!--Links-->
[WhatisASE]: app-service-app-service-environment-intro.md
[ScaleWebapp]: ../web-sites-scale.md
[HowtoCreateASE]: app-service-web-how-to-create-an-app-service-environment.md
[HowtoConfigureASE]: app-service-web-configure-an-app-service-environment.md
[CreateWebappinASE]: app-service-web-how-to-create-a-web-app-in-an-ase.md
[Appserviceplans]: ../azure-web-sites-web-hosting-plans-in-depth-overview.md
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[ASEAutoscale]: app-service-environment-auto-scale.md
[AppScale]: ../web-sites-scale.md
[AppWarmup]: http://ruslany.net/2015/09/how-to-warm-up-azure-web-app-during-deployment-slots-swap/
