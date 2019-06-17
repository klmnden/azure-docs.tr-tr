---
title: Bir App Service ortamı - Azure uygulama ölçeklendirme
description: Bir App Service ortamında bir uygulamayı ölçeklendirme
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
ms.custom: seodec18
ms.openlocfilehash: 6e683eb07b690d7d5680b7a4d429d1150f22f67e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60767714"
---
# <a name="scaling-apps-in-an-app-service-environment"></a>App Service Ortamında uygulamaları ölçeklendirme
Azure App Service'te normalde ölçeklendirebileceğiniz üç şey vardır:

* Fiyatlandırma planı
* çalışan boyutu 
* örnek sayısı.

Bir ASE'de seçmek veya fiyatlandırma planı değiştirmek için gerek yoktur.  Özellikleri açısından, bir Premium fiyatlandırma özellik düzeyi zaten yer sağlar.  

Çalışan boyutlarına göre her çalışan havuzu için kullanılacak işlem kaynağı boyutunu ASE yönetici atayabilirsiniz.  P4 işlem kaynaklarıyla çalışan havuzu 1 olabilir ve çalışan havuzu 2 P1 bilgi işlem kaynakları, isterseniz anlamına gelir.  Boyutu olması gerekmez.  Belge burada ayrıntılarla boyutlarını ve fiyatlarını görmek için [Azure App Service fiyatlandırması][AppServicePricing].  Bu, olması için App Service ortamında web apps ve App Service planları için ölçeklendirme seçenekleri bırakır:

* çalışan havuzu seçimi
* örnek sayısı

Her iki öğe değiştirme ASE'niz App Service planlarında barındırılan için gösterilen uygun kullanıcı Arabirimi aracılığıyla gerçekleştirilir.  

![][1]

ASP içinde çalışan havuzu kullanılabilir bilgi işlem kaynaklarının sayısını aşan ASP yukarı ölçeklendirilemez.  Bilgi işlem kaynaklarını, çalışan havuzunda varsa bunları eklemek için ASE yöneticinize almanız gerekir.  Yeniden yapılandırma geçici olarak bilgi ASE'nizi buradaki bilgileri okuyun: [Bir App Service ortamını yapılandırma][HowtoConfigureASE].  Kapasite zamanlama veya ölçümleri temel alan eklemek için ASE otomatik ölçeklendirme özelliklerinden yararlanmak isteyebilirsiniz.  ASE ortam için otomatik ölçeklendirmeyi yapılandırma hakkında ayrıntılı bilgi almak için bkz [bir App Service ortamı için otomatik ölçeklendirme yapılandırma][ASEAutoscale].

Hizmet planları farklı çalışan havuzlarını işlem kaynakları kullanılarak birden çok uygulama oluşturabilir veya aynı çalışan havuzunu kullanabilirsiniz.  (4) kullanan çalışan havuzu 1'de kullanılabilir bilgi işlem kaynakları (10) varsa, (6) işlem kaynakları kullanılarak bir app service planı oluşturmak seçebilirsiniz ve ikinci bir app service planı örneği için işlem kaynakları.

### <a name="scaling-the-number-of-instances"></a>Örnek sayısında ölçeklendirme
İlk web uygulamanızı bir App Service ortamında oluşturduğunuzda, 1 örnek ile başlar.  Ardından uygulamanız için ek işlem kaynakları sağlamak için ek örnekler için ölçeği genişletebilirsiniz.   

ASE'nizi yeterli kapasitesi varsa, ardından bu oldukça basittir.  App Service ölçek ölçeği artırma ve istediğiniz siteleri tutan planınız için gittiğiniz.  Bu, el ile ölçek kümesi için ASP veya reddedebileceğiniz, ASP için otomatik ölçeklendirme kuralları yapılandırma kullanıcı arabirimini açar.  El ile sadece uygulamanızın ölçeğini ayarlamak ***ölçeklendirilmesine*** için ***el ile girdiğim bir örnek sayısı***.  Buradan kaydırıcıyı istediğiniz miktar değerine sürükleyin veya kaydırıcıyı yanındaki kutuya girin.  

![][2] 

Bunlar her zamanki gibi bir ASP ASE için otomatik ölçeklendirme kuralları aynı şekilde işler.  Seçebileceğiniz ***CPU yüzdesi*** altında ***ölçeklendirilmesine*** ve CPU yüzdesi veya size göre ASP kullanarak daha karmaşık kurallar oluşturabilirsiniz için otomatik ölçeklendirme kuralları oluşturma ***zamanlama ve performans kuralları*** .  Yapılandırma hakkında daha ayrıntılı bilgi için otomatik ölçeklendirmeyi kullanma Kılavuzu burada [bir uygulamasını Azure App Service'te ölçeklendirme][AppScale]. 

### <a name="worker-pool-selection"></a>Çalışan havuzu seçimi
Çalışan havuzu seçimi, daha önce belirtildiği gibi ASP Arabiriminden erişilir.  Çalışan havuzunu seçin ve istediğiniz ASP dikey penceresini açın.  Tüm App Service Ortamı'nda yapılandırdığınız çalışan havuzları görürsünüz.  Ardından, yalnızca bir çalışan havuzu varsa yalnızca listelenen bir havuz görürsünüz.  ASP bulunduğu hangi çalışan havuzunu değiştirmek için yalnızca App Service taşımak için planınızı istediğiniz çalışan havuzunu seçin.  

![][3]

ASP bir çalışan havuzundan diğerine geçmeden önce ASP için yeterli kapasiteye sahip emin olmak önemlidir.  Çalışan havuzları listesi, yalnızca çalışan havuzu adı listelenen ancak kaç çalışanları, çalışan havuzunda kullanılabilir olduğunu da görebilirsiniz.  App Service planınız içerecek şekilde örnekleri yeterli olduğundan emin olun.  Daha fazla işlem kaynaklarını taşımak istediğiniz çalışan havuzunda, sonra bunları eklemek için ASE yöneticinize alın.  

> [!NOTE]
> ASP uygulamalarında, soğuk bir çalışan havuzu öğesinden bir ASP açacak taşıma başlatır.  Bu istekleri yeni işlem kaynakları üzerinde çalışmaya uygulamanızı soğuk olduğu gibi yavaş çalışmasına neden olabilir.  Hazırlıksız başlatma kullanılarak önlenebilir [uygulama çalışmaya sıcak] [ AppWarmup] Azure App Service'te.  Yeni bilgi işlem kaynaklarını başlatılan uygulamalar soğuk hale geldiğinde başlatma işlemi ayrıca çağrıldığından makalesinde açıklanan uygulama başlatma modül kaldırmanıza için de kullanılabilir. 
> 
> 

## <a name="getting-started"></a>Başlarken
App Service ortamları ile çalışmaya başlamak için bkz: [nasıl için oluşturma bir App Service ortamı][HowtoCreateASE]

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
[Appserviceplans]: ../overview-hosting-plans.md
[AppServicePricing]: https://azure.microsoft.com/pricing/details/app-service/ 
[ASEAutoscale]: app-service-environment-auto-scale.md
[AppScale]: ../web-sites-scale.md
[AppWarmup]: https://ruslany.net/2015/09/how-to-warm-up-azure-web-app-during-deployment-slots-swap/
