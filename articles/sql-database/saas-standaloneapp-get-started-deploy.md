---
title: Tek kiracılı SaaS Öğreticisi - Azure SQL veritabanı | Microsoft Docs
description: Dağıtma ve Azure SQL veritabanı kullanan bir tek başına tek kiracılı SaaS uygulaması keşfedin.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: sstein
manager: craigg
ms.date: 11/07/2018
ms.openlocfilehash: 4dbf53df4d3f34e80757f9575981b4b053587d97
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61485160"
---
# <a name="deploy-and-explore-a-standalone-single-tenant-application-that-uses-azure-sql-database"></a>Azure SQL veritabanı kullanan bir tek başına tek kiracılı uygulamasını dağıtma ve keşfetme

Bu öğreticide, dağıtın ve tek başına uygulama veya uygulama Kiracı başına, deseni kullanılarak geliştirilen Wingtip bilet SaaS örnek uygulaması keşfedin.  Uygulama, çok kiracılı SaaS senaryolarını etkinleştirme kolaylaştıran Azure SQL veritabanı özelliklerini göstermek için tasarlanmıştır.

Tek başına uygulama veya Kiracı başına uygulama düzeni her Kiracı için bir uygulama örneği dağıtır.  Her uygulama için belirli bir kiracıda yapılandırılmış ve ayrı bir Azure kaynak grubunda dağıtılan. Uygulamanın birden fazla örneği, çok kiracılı bir çözüm sağlamak için sağlanır. Bu düzen nerede Kiracı yalıtımı en önemli önceliktir kiracılar için az sayıda idealdir. Azure, bir kiracının aboneliğe dağıtılacak kaynaklar sağlayan ve yönetilen iş ortağı programları bir hizmet sağlayıcısı tarafından Kiracı adına sahiptir. 

Bu öğreticide, Azure aboneliğinizde oturum üç kiracılar için üç tek başına uygulama dağıtır.  Size keşfedin ve tek tek uygulama bileşenleri ile çalışmak için tam erişime sahiptir.

Uygulama kaynak kodu ve yönetim komut dosyaları kullanılabilir [WingtipTicketsSaaS StandaloneApp](https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp) GitHub deposu. Uygulamayı Visual Studio 2015 kullanılarak oluşturulmuş ve başarıyla açın ve Visual Studio 2017'de güncelleştirmeden.


Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
> * Wingtip bilet SaaS tek başına uygulama dağıtma
> * Uygulama kaynak kodunu ve yönetim komut dosyaları elde edileceği.
> * Sunucuları ve uygulamayı oluşturan veritabanları hakkında.

Ek öğreticiler kullanıma sunulacaktır. Bunlar, bir dizi bu uygulama deseni temel alınarak yönetim senaryolarını keşfetmek izin verir.   

## <a name="deploy-the-wingtip-tickets-saas-standalone-application"></a>Wingtip bilet SaaS tek başına uygulamayı dağıtma

Uygulama için üç sağlanan Kiracı dağıtın:

1. Her mavi tıklayın **azure'a Dağıt** dağıtım şablonu açmak için düğmeyi [Azure portalında](https://portal.azure.com). Her şablon, iki parametre değerleri gerektirir; Yeni bir kaynak grubu için bir ad ve bu dağıtım uygulamanın diğer dağıtımlardan ayıran bir kullanıcı adı. Sonraki adım, bu değerleri ayarlamak için Ayrıntılar sağlar.<br><br>
    <a href="https://aka.ms/deploywingtipsa-contoso" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a> &nbsp; **Contoso Konser Salonu**
<br><br>
    <a href="https://aka.ms/deploywingtipsa-dogwood" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a> &nbsp; **Kızılcık Dojo**
<br><br>
    <a href="https://aka.ms/deploywingtipsa-fabrikam" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a> &nbsp; **Fabrikam Caz kulübü**

2. Her dağıtım için gerekli parametre değerlerini girin.

    > [!IMPORTANT]
    > Tanıtım amacıyla kasıtlı olarak güvenli olmayan bazı kimlik doğrulama ve sunucu güvenlik duvarı. **Yeni bir kaynak grubu oluşturma** her uygulama dağıtımı için.  Mevcut bir kaynak grubunu kullanmayın. Bu uygulamayı ya da oluşturduğu kaynakları üretim için kullanmayın. İlgili faturalandırmayı durdurmak için uygulama ile işiniz bittiğinde, tüm kaynak gruplarını silin.

    Yalnızca küçük harf, sayı ve kısa çizgi, kaynak adlarını kullanmak en iyisidir.
    * İçin **kaynak grubu**Yeni Oluştur'u seçin ve ardından kaynak grubu için küçük bir ad sağlayın. **Wingtip-sa -\<venueName\>-\<kullanıcı\>**  önerilen modelidir.  İçin \<venueName\>, boşluk içermeyen mekan adıyla değiştirin. İçin \<kullanıcı\>, kullanıcı değeri aşağıdan değiştirin.  Bu desen ile kaynak grubu adları olabilir *wingtip-sa-contosoconcerthall-af1*, *wingtip-sa-dogwooddojo-af1*, *wingtip-sa-fabrikamjazzclub-af1*.
    * Seçin bir **konumu** aşağı açılan listeden.

    * İçin **kullanıcı** -adınızın baş harflerini artı bir basamağı gibi bir kısa bir kullanıcı değeri öneririz: Örneğin, *af1*.


3. **Uygulamayı dağıtın**.

    * Hüküm ve koşulları kabul etmek için tıklayın.
    * **Satın al**’a tıklayın.

4. Tıklayarak, tüm üç dağıtımların durumunu izlemek **bildirimleri** (arama kutusunun sağındaki zil simgesi). Uygulamaları dağıtma, yaklaşık beş dakika sürer.


## <a name="run-the-applications"></a>Uygulamaları çalıştırma

Uygulama olayları barındıran venues gösterir.  Mekanlar, uygulama kiracılarıdır. Her mekan, etkinliklerini ve bilet satmak için kişiselleştirilmiş bir web sitesi alır. Konser salonları, Caz kulüpleri ve Spor kulüpleri mekan türleri içerir. Bu örnekte, mekan web sitesinde gösterilen arka plan fotoğrafı mekan türünü belirler.   Tek başına uygulama modelinde, her mekanın kendi tek başına SQL veritabanı ile bir ayrı bir uygulama örneği vardır.

1. Her üç ayrı tarayıcı sekmeleri kiracılar için olayları sayfayı açın:

   - http://events.contosoconcerthall.&lt; Kullanıcı&gt;. trafficmanager.net
   - http://events.dogwooddojo.&lt; Kullanıcı&gt;. trafficmanager.net
   - http://events.fabrikamjazzclub.&lt; Kullanıcı&gt;. trafficmanager.net

     (Her URL'de değiştirin &lt;kullanıcı&gt; dağıtımınızın kullanıcı değerine sahip.)

   ![Olaylar](./media/saas-standaloneapp-get-started-deploy/fabrikam.png)

Gelen istekler, uygulamanın kullandığı dağıtımını denetlemek için [ *Azure Traffic Manager*](../traffic-manager/traffic-manager-overview.md). Her kiracıya özgü uygulama örneği, URL'de Kiracı adı etki alanı adının bir parçası olarak içerir. Tüm Kiracı URL'leri, özel **kullanıcı** değeri. URL aşağıdaki biçimde izleyin:
- http://events.&lt; venuename&gt;.&lt; Kullanıcı&gt;. trafficmanager.net

Her bir kiracının veritabanı **konumu** karşılık gelen dağıtılan uygulamayı uygulama ayarlarında dahildir.

Bir üretim ortamında, genellikle bir CNAME DNS kaydı için oluşturduğunuz [ *bir şirketin internet etki alanını işaret* ](../traffic-manager/traffic-manager-point-internet-domain.md) traffic manager profili URL'si.


## <a name="explore-the-servers-and-tenant-databases"></a>Sunucular ve Kiracı veritabanlarını öğrenme

Dağıtılan kaynakların bazılarına bakalım:

1. İçinde [Azure portalında](https://portal.azure.com), kaynak grupları, listeye göz atın.
2. Üç Kiracı kaynak gruplarını görmeniz gerekir.
3. Açık **wingtip-sa-fabrikam -&lt;kullanıcı&gt;**  Fabrikam Caz kulübü dağıtımı için kaynakları içeren kaynak grubu.  **Fabrikamjazzclub -&lt;kullanıcı&gt;**  sunucusunu içeren **fabrikamjazzclub** veritabanı.

Her Kiracı veritabanının, 50 DTU *tek başına* veritabanı.

## <a name="additional-resources"></a>Ek kaynaklar

<!--
* Additional [tutorials that build on the Wingtip SaaS application](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* To learn about elastic pools, see [*What is an Azure SQL elastic pool*](sql-database-elastic-pool.md)
* To learn about elastic jobs, see [*Managing scaled-out cloud databases*](sql-database-elastic-jobs-overview.md)
-->

- Çok kiracılı SaaS uygulamaları hakkında bilgi edinmek için [çok kiracılı SaaS uygulamaları için Tasarım Düzenleri](saas-tenancy-app-design-patterns.md).

 
## <a name="delete-resource-groups-to-stop-billing"></a>Faturalandırmayı durdurmak için kaynak gruplarını silin ##

Örneğini kullanarak tamamladığınızda, ilgili faturalandırmayı durdurmak için oluşturulan tüm kaynak gruplarını silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz:

> [!div class="checklist"]
> * Wingtip bilet SaaS tek başına uygulama dağıtma
> * Sunucuları ve uygulamayı oluşturan veritabanları hakkında.
> * İlgili faturalandırmayı durdurmak için örnek kaynakları silme yapma.

Ardından, deneyin [sağlama ve kataloğa kaydetme](saas-standaloneapp-provision-and-catalog.md) öğretici içinde keşif kiracılar arası senaryoları şema yönetim ve Kiracı analizi gibi çeşitli sağlayan Kiracı Kataloğu kullanımı.
 

