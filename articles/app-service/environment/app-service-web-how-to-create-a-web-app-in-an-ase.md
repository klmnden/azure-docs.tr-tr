---
title: Bir App Service ortamı v1 - Azure web uygulaması oluşturma
description: Web apps ve app service planları bir App Service ortamı v1'de oluşturmayı öğrenin
services: app-service
documentationcenter: ''
author: ccompy
manager: stefsch
editor: ''
ms.assetid: 983ba055-e9e4-495a-9342-fd3708dcc9ac
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 3e7db670a125f3c5f308107aabfbbab9301b7561
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60765184"
---
# <a name="create-a-web-app-in-an-app-service-environment-v1"></a>Bir App Service ortamı v1'de bir web uygulaması oluşturma

> [!NOTE]
> Bu makale, App Service ortamı v1 hakkında yöneliktir.  App Service ortamı, kullanımı daha kolay ve daha güçlü bir altyapı üzerinde çalışan daha yeni bir sürümü var. Yeni sürüm başlama hakkında daha fazla bilgi edinmek için [App Service ortamı giriş](intro.md).
> 

## <a name="overview"></a>Genel Bakış
Bu öğreticide, web uygulamaları oluşturmak gösterilir ve App Service planlarında bir [App Service ortamı v1](app-service-app-service-environment-intro.md) (ASE). 

> [!NOTE]
> Bir web uygulamasının nasıl oluşturulacağını öğrenmek istiyor, ancak gerekmeyen bir App Service Ortamı'nda yapmak için bkz: [.NET web uygulaması oluşturma](../app-service-web-get-started-dotnet.md) veya diğer diller ve çerçeveler için ilgili öğreticiler.
> 
> 

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticide, bir App Service ortamı oluşturmuş olduğunuzu varsayar. Henüz yapmadıysanız bkz [bir App Service ortamı oluşturma](app-service-web-how-to-create-an-app-service-environment.md). 

## <a name="create-a-web-app"></a>Web uygulaması oluşturma
1. İçinde [Azure portalı](https://portal.azure.com/), tıklayın **kaynak Oluştur > Web + mobil > Web uygulaması**. 
   
    ![][1]
2. Aboneliğinizi seçin.  
   
    Birden fazla aboneliğiniz varsa App Service Ortamı'nda bir uygulama oluşturmak için ortamı oluştururken kullandığınız aynı aboneliği kullanmanız gerektiğini unutmayın. 
3. Kaynak grubunu seçin veya oluşturun.
   
    *Kaynak grupları* bir birim olarak ilgili Azure kaynaklarını yönetmenizi sağlayan ve kurulurken faydalıdır *rol tabanlı erişim denetimi* uygulamalarınız için (RBAC) kuralları. Daha fazla bilgi için [Azure Resource Manager'a genel bakış][ResourceGroups]. 
4. Bir App Service planı seçin ya da oluşturun.
   
    *App Service planları* web Apps yönetilen kümeleri.  Normal fiyatlandırma seçtiğinizde fiyatın App Service planı yerine tek tek uygulamalar için uygulanır. ASE için ayrılan işlem örnekleri için ödeme bir ASE'de, ASP ile listelenen değil.  Bir web uygulamasının App service'inizin örneği ölçeklendirme örneklerinin artırabileceğinizi planı ve etkiler tüm web uygulamaları ve bu planı.  Site yuvaları veya VNET tümleştirmesi gibi bazı özellikleri, ayrıca plan içindeki miktar kısıtlamaları vardır.  Daha fazla bilgi için [Azure App Service planlarına genel bakış](../overview-hosting-plans.md)
   
    Plan adı altında belirtilen konuma bakarak ASE'NİZDE App Service planları tanımlayabilirsiniz.  
   
    ![][5]
   
    App Service Ortamı'nda zaten bir App Service planı kullanmak istiyorsanız, bu planı seçin. Yeni bir App Service planı oluşturmak istiyorsanız, bu öğreticinin aşağıdaki bölüme bakın [bir App Service ortamında bir App Service planı oluşturma](#createplan).
5. Web uygulamanız için bir ad girin ve ardından **Oluştur**. 
   
    Dış VIP ASE'nizi kullanıyorsa, bir ase'de uygulamanın URL'sidir: [*sitename*]. [ *App Service ortamınızın adını*]. yerine p.azurewebsites.net [*sitename*]. azurewebsites.net
   
    ASE'nizi bir iç VIP sonra bir uygulamanın URL'sini kullanıyorsa ASE olması: [*sitename*]. [ *ASE oluşturma sırasında belirtilen bir alt etki alanı*]   
    ASE oluşturma sırasında ASP seçtikten sonra aşağıdaki güncelleştirme alt etki alanı görürsünüz **adı**

## <a name="createplan"></a> Bir App Service planı oluşturma
Bir App Service ortamında bir App Service planı oluşturduğunuzda, bir ASE'de paylaşılan çalışanlar olarak çalışan seçimlerinizi farklıdır.  Kullanmak zorunda çalışanları ASE için yönetici tarafından ayrılmış olan olaylardır  Bu, yeni bir plan oluşturmak için çalışan havuzunda zaten planlarınızı tüm örneklerin toplam sayısından ASE çalışan havuzuna ayrılan çalışan daha ihtiyacınız olduğunu anlamına gelir.  Planınızı oluşturmak için ASE çalışan havuzunda yeterli çalışanları yoksa, eklenen alacağınız ASE yöneticiniz ile çalışmak gerekir.

Başka bir App Service planlarında barındırılan bir App Service ortamı tarafından fiyatlandırma seçimi olmaması farktır.  App Service ortamı varsa, sistem tarafından kullanılan işlem kaynakları için ödeme yaparsınız ve eklenen planlarının ücretini, o ortamda izniniz yok.  Normalde, bir App Service planı oluşturduğunuzda, fatura belirleyen bir fiyatlandırma planı seçin.  App Service ortamı temelde içerik oluşturabileceğiniz özel bir konuma ' dir.  Ortamı ve içeriğinizi barındıran değil için ödeme yaparsınız.

Aşağıdaki yönergeler, öğreticinin önceki bölümünde açıklandığı gibi bir web uygulaması oluştururken bir App Service planı oluşturma işlemi gösterilmektedir.

1. Tıklayın **Yeni Oluştur** planı seçimi Arabirimi içinde ve dışında bir ASE normalde olduğu gibi planınız için bir ad sağlayın.
2. Konum seçicinizde kullanmak istediğiniz ASE seçin.
   
    App Service ortamı, aslında bir özel dağıtım konumu olduğundan, altında konumu gösterir. 
   
    ![][2]
   
    Bir ASE'de konumu Seçici seçimi yaptıktan sonra App Service planı oluşturma, UI güncelleştirir.  Konum, artık durumda ve fiyatlandırma planı seçici bir çalışan havuzu Seçici ile değiştirilir ASE sistem ve bölge adını gösterir.  
   
    ![][3]

### <a name="selecting-a-worker-pool"></a>Bir çalışan havuzu seçme
Normalde Azure App Service'te ve App Service ortamı dışında kullanılabilir bir adanmış fiyat planı seçimle 3 işlem boyutu vardır.  Benzer şekilde, ASE çalışan en fazla 3 havuzları tanımlayabilir ve bu çalışan havuzu için kullanılan işlem boyutu belirtin.  Anlamı kiracılar ase'nin işlem boyutu App Service planınız için bir fiyatlandırma planı seçme için bunun yerine, hangi çağrılır seçin bir *çalışan havuzu*.  

Çalışan havuzu seçimi kullanıcı Arabirimi, adının altında çalışan havuzu için kullanılan işlem boyutu gösterir.  Kullanılabilir miktarı örnekleri Havuzda kullanılabilir kaç hesaplamak için ifade eder.  Toplam havuzu aslında bu sayıdan daha fazla örneği olabilir, ancak yalnızca kaç olmadığı için bu değeri başvuruyor.  Daha fazla bilgi işlem kaynaklarını eklemek için App Service ortamınızı ayarlamanız gerekip gerekmediğine bakın [App Service ortamınızı yapılandırma](app-service-web-configure-an-app-service-environment.md).

![][4]

Bu örnekte, yalnızca iki çalışan havuzları görürsünüz. ASE yönetici, yalnızca ana bu iki çalışan havuzlarına ayrılan olmasıdır.  İçine ayrılmış Vm'leri olduğunda üçüncü gösterebilir.  

## <a name="after-web-app-creation"></a>Sonra Web uygulaması oluşturma
Web uygulamalarını çalıştırmak ve dikkate alınması gereken bir ASE içindeki App Service planlarının yönetmek için birkaç nokta vardır.  

Daha önce belirtildiği gibi ASE sahibi sistem boyutu için sorumlu olduğunu ve bunun sonucunda bunlar da istenen App Service planları barındırmak için yeterli kapasite olduğundan emin olmak sizin sorumluluğunuzdadır. Kullanılabilir çalışanlar varsa, App Service planınızı oluşturmak mümkün olmayacaktır.  Web uygulamanızın ölçeğini genişletme true budur.  Daha fazla örnek gerekiyorsa çalışan daha eklemek için App Service ortamı yönetim almak gerekir.

Web uygulaması ve App Service planı oluşturduktan sonra ölçeği iyi bir fikirdir.  Bir ASE her zaman en az 2 örnekleri, uygulamalarınız için hataya dayanıklılık sağlamak için App Service planınızın olması gerekir.  Bir ASE'de App Service planı ölçeklendirme normal App Service planı kullanıcı Arabirimi aracılığıyla aynıdır.  Ölçeklendirme hakkında daha fazla bilgi için [bir App Service ortamında bir web uygulamasını ölçeklendirme](app-service-web-scale-a-web-app-in-an-app-service-environment.md)

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspnewwebapp.png
[2]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createasplocation.png
[3]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspselected.png
[4]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspworkerpool.png
[5]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/selectaspinase.png

<!--Links-->
[WhatisASE]: app-service-app-service-environment-intro.md
[Appserviceplans]: ../overview-hosting-plans.md
[HowtoCreateASE]: app-service-web-how-to-create-an-app-service-environment.md
[HowtoScale]: app-service-web-scale-a-web-app-in-an-app-service-environment.md
[HowtoConfigureASE]: app-service-web-configure-an-app-service-environment.md
[ResourceGroups]: ../../azure-resource-manager/resource-group-overview.md
[AzurePowershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
