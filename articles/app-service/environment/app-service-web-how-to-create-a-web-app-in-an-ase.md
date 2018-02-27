---
title: "Bir uygulama hizmeti ortamı v1 bir web uygulaması oluşturma"
description: "Web uygulamaları ve app service planları bir uygulama hizmeti ortamı v1 oluşturmayı öğrenin"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: 983ba055-e9e4-495a-9342-fd3708dcc9ac
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/11/2017
ms.author: ccompy
ms.openlocfilehash: 1e8540409c6174ad02bd2d9d57c53e0279f49871
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-web-app-in-an-app-service-environment-v1"></a>Bir uygulama hizmeti ortamı v1 bir web uygulaması oluşturma

> [!NOTE]
> Bu makale hakkında uygulama hizmeti ortamı v1 yazılmıştır.  Uygulama hizmeti ortamı kullanmak daha kolay ve daha güçlü altyapısı üzerinde çalışan daha yeni bir sürümü var. Yeni sürüm Başlarken hakkında daha fazla bilgi edinmek için [uygulama hizmeti ortamı giriş](intro.md).
> 

## <a name="overview"></a>Genel Bakış
Bu öğreticide web uygulamaları oluşturmak nasıl gösterilir ve App Service planlarına bir [uygulama hizmeti ortamı v1](app-service-app-service-environment-intro.md) (ana). 

> [!NOTE]
> Bir web uygulaması oluşturmayı öğrenmek istiyorsanız ancak gerekmeyen bir uygulama hizmeti ortamı'nda yapmak için bkz: [bir .NET web uygulaması oluşturma](../app-service-web-get-started-dotnet.md) veya diğer dilleri ve çerçeveleri için ilgili öğreticiler biri.
> 
> 

## <a name="prerequisites"></a>Önkoşullar
Bu öğretici, bir uygulama hizmeti ortamı oluşturduğunuz varsayar. Henüz yapmadıysanız bkz [bir uygulama hizmeti ortamı oluşturma](app-service-web-how-to-create-an-app-service-environment.md). 

## <a name="create-a-web-app"></a>Web uygulaması oluşturma
1. İçinde [Azure Portal](https://portal.azure.com/), tıklatın **kaynak oluşturma > Web + mobil > Web uygulaması**. 
   
    ![][1]
2. Aboneliğinizi seçin.  
   
    Birden çok aboneliğiniz varsa, uygulama hizmeti ortamı'nda bir uygulama oluşturmak için ortam oluşturulurken kullanılan aynı aboneliği kullanmanız gerektiğini unutmayın. 
3. Kaynak grubunu seçin veya oluşturun.
   
    *Kaynak grupları* bir birim olarak ilgili Azure kaynaklarını yönetmenizi sağlayan ve kurulurken faydalıdır *rol tabanlı erişim denetimi* (RBAC) kuralları, uygulamalarınız için. Daha fazla bilgi için bkz: [Azure Resource Manager'a genel bakış][ResourceGroups]. 
4. Bir App Service planı seçin ya da oluşturun.
   
    *Uygulama hizmeti planları* web uygulamaları yönetilen kümeleridir.  Normalde fiyatlandırma seçtiğinizde, ücret fiyat uygulama hizmeti planına yerine tek tek uygulamalara uygulanır. Ana için ayrılan işlem örnekleri için ödeme ASE'de ASP ile listelenen yerine.  App Service örneği ölçeklendirme bir web uygulaması örnek sayısı ölçeklendirmek için plan ve etkiler tüm web uygulamalarının bu planında.  Site yuvaları veya VNET tümleştirme gibi bazı özellikler de planı içinde miktar kısıtlamaları vardır.  Daha fazla bilgi için bkz: [Azure App Service planlarına genel bakış](../azure-web-sites-web-hosting-plans-in-depth-overview.md)
   
    Plan adı altında belirtilen konumda bakarak, uygulama hizmeti planları, ana tanımlayabilirsiniz.  
   
    ![][5]
   
    Uygulama hizmeti ortamı'nda zaten bir uygulama hizmeti planı kullanmak istiyorsanız, o planı seçin. Yeni bir uygulama hizmeti planı oluşturmak istiyorsanız, bu öğreticinin aşağıdaki bölüme bakın [bir uygulama hizmeti ortamı'nda bir uygulama hizmeti planı oluştur](#createplan).
5. Web uygulamanız için bir ad girin ve ardından **oluşturma**. 
   
    Bir ana uygulamada URL'sini ise, ana dış bir VIP kullanır: [*sitename*]. [ *uygulama hizmeti ortamınızı adını*]. yerine p.azurewebsites.net [*sitename*]. azurewebsites.net
   
    Ana olması, ana iç VIP bir uygulamanın URL'sini kullanırsa: [*sitename*]. [ *ana oluşturma sırasında belirtilen alt etki alanı*]   
    Ana oluşturma sırasında ASP seçtikten sonra aşağıda güncelleştirme alt etki alanı görürsünüz **adı**

## <a name="createplan"></a> Bir uygulama hizmeti planı oluştur
Uygulama hizmeti ortamı'nda bir uygulama hizmeti planı oluşturduğunuzda, çalışan seçimlerinizi ASE'de paylaşılan çalışanlar olduğundan farklıdır.  Kullanmak zorunda çalışanları için ana yönetici tarafından ayrılan olanlardır  Bu yeni bir plan oluşturmak için tüm bu çalışan havuzunda zaten planlarınıza örneklerin toplam sayısından ana çalışan havuzuna ayrılmış daha fazla çalışanlarının gerektiği anlamına gelir.  Planınızı oluşturmak için ana çalışan havuzunda yeterli çalışanları yoksa, bunları eklenen almak için ana yöneticinize çalışması gerekir.

Bir uygulama hizmeti ortamı tarafından barındırılan uygulama hizmeti planına sahip başka bir fark seçimi fiyatlandırma yetersizliğidir.  Bir uygulama hizmeti ortamı olduğunda sistem tarafından kullanılan işlem kaynakları için ödeme ve planlar için ek ücret, ortamda sahip değil.  Normalde, bir uygulama hizmeti planı oluşturduğunuzda, faturalama belirleyen bir fiyatlandırma planı seçin.  Bir uygulama hizmeti ortamı temelde içeriği oluşturabileceğiniz özel bir konum değil.  İçeriğinizi barındırmak değil ve ortam için ücret ödersiniz.

Aşağıdaki yönergeler öğreticinin önceki bölümde açıklandığı gibi bir web uygulaması oluştururken bir uygulama hizmeti planı oluşturmayı gösterir.

1. Tıklatın **Yeni Oluştur** planı seçimi Arabirimi içinde ve dışında bir ana normalde yaptığınız gibi planınız için bir ad sağlayın.
2. Konum seçiciden kullanmak istediğiniz ana seçin.
   
    Bir uygulama hizmeti ortamı temelde özel dağıtım konumu olduğundan, konum altında gösterilir. 
   
    ![][2]
   
    Konum Seçici ASE'de seçimi yaptıktan sonra uygulama hizmeti plan oluşturma UI güncelleştirir.  Konumun şimdi olarak ve fiyatlandırma planı seçici bir çalışan havuzu Seçici ile değiştirilir ana sistem ve bölge adını gösterir.  
   
    ![][3]

### <a name="selecting-a-worker-pool"></a>Bir çalışan havuzu seçme
Normalde Azure App Service'te ve bir uygulama hizmeti ortamı dışında bir adanmış fiyat planı seçimle kullanılabilir 3 işlem boyutları vardır.  Benzer bir şekilde için bir ana çalışanı 3 adede kadar havuzları tanımlayabilir ve bu çalışan havuzu için kullanılan işlem boyutu belirtin.  Ne anlamına ana kiracılar işlem boyutu uygulama hizmeti planınız için bir fiyatlandırma planı seçme için yerine, ne adlı seçtiğiniz bir *çalışan havuzunda*.  

Çalışan havuzu seçimi UI adının altında bu çalışan havuzu için kullanılan işlem boyutunu gösterir.  Kullanılabilir miktarı örnekleri Havuzda kullanılabilir kaç tane işlem için ifade eder.  Toplam havuzu gerçekte bu sayıdan fazla örneğe sahip olabilir ancak bu değer yalnızca kaç kullanımda olmayan başvuruyor.  Daha fazla işlem kaynakları eklemek için uygulama hizmeti ortamınızı ayarlamanız gerekiyorsa bkz [uygulama hizmeti ortamınızı yapılandırma](app-service-web-configure-an-app-service-environment.md).

![][4]

Bu örnekte, yalnızca iki çalışan havuzlarında kullanılabilir görürsünüz. ANA yönetici bu iki alt havuzları halinde yalnızca ana ayrılan olmasıdır.  İçine ayrılan VM'ler olduğunda üçüncü göstermeniz.  

## <a name="after-web-app-creation"></a>Sonra Web uygulaması oluşturma
Web uygulamaları çalıştıran ve dikkate alınması gereken bir ana uygulama hizmeti planları yönetmek için bazı noktalar vardır.  

Daha önce belirtildiği gibi ana sistemin boyutu için sorumlu sahibi ve sonuç olarak, aynı zamanda istenen uygulama hizmeti planları barındırmak için yeterli kapasitesi olduğunu sağlamaktan sorumludur. Kullanılabilir hiçbir çalışanları varsa, uygulama hizmeti planınızı oluşturmak mümkün olmaz.  Bu, ayrıca web uygulamanızı ölçeklendirme true olur.  Daha fazla örnekleri ihtiyacınız varsa daha fazla çalışanları eklemek için uygulama hizmeti ortamı yöneticiniz almak gerekir.

Web uygulaması ve uygulama hizmeti planı oluşturduktan sonra ölçeği iyi bir fikirdir.  ASE'de her zaman en az 2 örnekleri, uygulamalarınız için hataya dayanıklılık sağlamak için uygulama hizmeti planınızın olması gerekir.  Bir uygulama hizmeti planı ASE'de ölçeklendirme uygulama hizmeti planı kullanıcı Arabirimi üzerinden normal olarak aynıdır.  Ölçeklendirme hakkında daha fazla bilgi için [bir uygulama hizmeti ortamı'nda bir web uygulaması ölçeklendirme](app-service-web-scale-a-web-app-in-an-app-service-environment.md)

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspnewwebapp.png
[2]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createasplocation.png
[3]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspselected.png
[4]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspworkerpool.png
[5]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/selectaspinase.png

<!--Links-->
[WhatisASE]: app-service-app-service-environment-intro.md
[Appserviceplans]: ../azure-web-sites-web-hosting-plans-in-depth-overview.md
[HowtoCreateASE]: app-service-web-how-to-create-an-app-service-environment.md
[HowtoScale]: app-service-web-scale-a-web-app-in-an-app-service-environment.md
[HowtoConfigureASE]: app-service-web-configure-an-app-service-environment.md
[ResourceGroups]: ../../azure-resource-manager/resource-group-overview.md
[AzurePowershell]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
