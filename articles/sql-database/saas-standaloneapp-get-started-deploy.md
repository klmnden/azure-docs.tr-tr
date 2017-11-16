---
title: "Çok kiracılı SaaS öğretici - Azure SQL veritabanı | Microsoft Docs"
description: "Dağıtma ve Azure SQL veritabanı kullanan bir tek başına tek Kiracı SaaS uygulaması keşfedin."
keywords: "sql veritabanı öğreticisi"
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: sstein
ms.openlocfilehash: 2ca290dfcb23215ffd727500e76076ae8b14fa6f
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="deploy-and-explore-a-standalone-single-tenant-application-that-uses-azure-sql-database"></a>Dağıtma ve Azure SQL veritabanı kullanan bir tek başına tek Kiracı uygulama keşfedin

Bu öğreticide dağıtmak ve Wingtip biletleri SaaS tek başına uygulamayı inceleyin. Uygulama, etkinleştirme SaaS senaryoları basitleştirmek Azure SQL veritabanı özelliklerini göstermek için tasarlanmıştır.

Tek başına uygulama düzeni her bir kiracı için tek kiracılı uygulama ve tek Kiracı veritabanı içeren bir Azure kaynak grubu dağıtır.  Uygulamasının birden çok örneği, bir çok kiracılı çözüm sağlamak için sağlanabilir.

Bu öğreticide, kaynak grupları çeşitli kiracılar için Azure aboneliğinize dağıtacağınız olsa da, bir kiracının Azure aboneliğinize dağıtılması için kaynak gruplarında bu deseni sağlar.  Azure kiracının adına bir hizmet sağlayıcısı tarafından kiracının aboneliğe yönetici olarak yönetilmesi bu kaynak gruplarını izin ortak programlar sahiptir.

Dağıtım bölümünde Azure düğmeleri, her biri belirli bir kiracı için özelleştirilmiş uygulama farklı bir örneğine dağıtır üç Dağıt vardır. Her düğmesine basıldığında karşılık gelen uygulama beş dakika sonra tam olarak dağıtılır.  Uygulamaları Azure aboneliğinizde dağıtılır.  Keşfetmek ve tek tek uygulama bileşenleri ile çalışmak için tam erişime sahip.

Uygulama kaynak kodu ve yönetim komut dosyaları kullanılabilir [WingtipTicketsSaaS StandaloneApp](https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp) GitHub depo.


Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]

> * Wingtip biletleri SaaS tek başına uygulama dağıtma
> * Uygulama kaynak koduna ve yönetim komut dosyaları nereden
> * Sunucular ve uygulaması olun veritabanları hakkında

Ek öğreticileri yayımlanacaktır son doğal imkan tanıyan bir dizi yönetim senaryoları bu uygulama deseni temel alınarak keşfetmek.   

## <a name="deploy-the-wingtip-tickets-saas-standalone-application"></a>Wingtip biletleri SaaS tek başına uygulamayı dağıtma

Uygulamayı üç sağlanan kiracılar için dağıtma:

1. Her tıklatın **Azure'a Dağıt** düğme Azure portalında dağıtım şablonu açın. Her bir şablon iki parametre değerlerini gerektirir; Yeni bir kaynak grubu için bir ad ve bu dağıtım uygulamanın diğer dağıtımlardan ayıran bir kullanıcı adı. Sonraki adım, bu değerleri ayarlamak için Ayrıntılar sağlar.<br><br>
    <a href="http://aka.ms/deploywingtipsa-contoso" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>     &nbsp;&nbsp;**Contoso birlikte Hall**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-dogwood" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>&nbsp; &nbsp; **Kızılcık Dojo**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-fabrikam" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>    &nbsp;&nbsp;**Fabrikam Jazz kulübü**

2. Her dağıtım için gerekli parametre değerlerini girin.

    > [!IMPORTANT]
    > Gösterim amaçları doğrultusunda bazı kimlik doğrulama işlemlerinin ve sunucu güvenlik duvarlarının güvenliği kasıtlı olarak kaldırılmıştır. **Yeni bir kaynak grubu oluşturma** her uygulama dağıtımı için.  Varolan bir kaynak grubu kullanmayın. Bu uygulama ya da oluşturur, herhangi bir kaynağa üretim için kullanmayın. İlgili faturalama durdurmak için uygulamalarla tamamladığınızda, tüm kaynak grupları silin.

    Kaynak adları yalnızca küçük harf, sayı ve kısa çizgi kullanmak en iyisidir.
    * İçin **kaynak grubu** - Yeni Oluştur seçin ve ardından (büyük küçük harf duyarlı) kaynak grubu için bir ad sağlayın.
        * Kaynak grubu adı tüm harfler küçük harfli olması önerilir.
        * Bir rakam ile izlenen adınızın baş harflerini ve ardından bir tire append öneririz: Örneğin, _wingtip sa af1_.
        * Aşağı açılan listeden bir konum seçin.

    * İçin **kullanıcı** -adınızın baş harflerini artı bir rakam gibi kısa bir kullanıcı değerini seçmenizi öneririz: Örneğin, _af1_.


1. **Uygulamayı dağıtın**.

    * Hüküm ve koşulları kabul için tıklatın.
    * **Satın al**’a tıklayın.

1. İzleme, tüm üç dağıtımlarının dağıtım durumu tıklayarak **bildirimleri** (zil simgesine arama kutusunun sağında). Uygulama dağıtımı yaklaşık beş dakika sürer.


## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama birlikte salonları, jazz Sinek ve olayları barındıran Spor Sinek gibi görebildikleri gösterir. Görebildikleri olayları listelemek ve bilet satabilir kolay bir yol için müşterinin (veya) Wingtip biletlerinin kaydedin. Her salonundan yönetmek ve bunların olaylarını listelemek ve bağımsız ve diğer kiracıdan yalıtılmış bilet satabilir için kişiselleştirilmiş bir web sitesini alır. Perde arkasında her Kiracı ayrı uygulama örneği ve tekil SQL veritabanı alır.

1. Her üç ayrı tarayıcı sekmeleri kiracılar Etkinlikler sayfasını açın:

    http://Events.contosoconcerthall. &lt;Kullanıcı&gt;. trafficmanager.net <br>
    http://Events.dogwooddojo. &lt;Kullanıcı&gt;. trafficmanager.net<br>
    http://Events.fabrikamjazzclub. &lt;Kullanıcı&gt;. trafficmanager.net

    (Değiştir &lt;kullanıcı&gt; dağıtımınızın kullanıcı değeri ile).

   ![Olaylar](./media/saas-standaloneapp-get-started-deploy/fabrikam.png)


Gelen istekleri, uygulamanın kullandığı dağıtımını denetlemek için [ *Azure Traffic Manager*](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview). Her Kiracı özgü uygulama URL'de Kiracı adı etki alanı adının bir parçası olarak içerir. Tüm Kiracı URL'leri özel *kullanıcı* değeri ve bu biçim izleyin: http://events.&lt; venuename&gt;.&lt; Kullanıcı&gt;. trafficmanager.net. Her bir kiracının veritabanı konumu karşılık gelen dağıtılan uygulama uygulama ayarlarında dahil edilir.

Bir üretim ortamında genellikle bir CNAME DNS kaydı oluşturacak [ *bir şirketin internet etki alanını işaret* ](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-point-internet-domain) trafik Yöneticisi profili URL.


## <a name="explore-the-servers-and-tenant-databases"></a>Sunucuları araştırın ve Kiracı veritabanları

Dağıtılan kaynakların bazıları bakalım:

1. İçinde [Azure portal](http://portal.azure.com), kaynak grupları listesine göz atın.  Görmeniz gerekir **wingtip-sa-katalog -&lt;kullanıcı&gt;**  kaynak grubunu **katalog-sa -&lt;kullanıcı&gt;**  sunucusu ile dağıtılan**tenantcatalog** veritabanı. Ayrıca üç Kiracı kaynak grupları görmeniz gerekir.

1. Açık **wingtip-sa-fabrikam -&lt;kullanıcı&gt;**  Fabrikam Jazz kulübü dağıtımı için kaynakları içeren kaynak grubu.  **Fabrikamjazzclub -&lt;kullanıcı&gt;**  sunucusunu içeren **fabrikamjazzclub** veritabanı.


50 DTU her Kiracı veritabanıdır _tek başına_ veritabanı.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz:

> [!div class="checklist"]

> * Wingtip biletleri SaaS tek başına uygulama dağıtma
> * Sunucular ve uygulaması olun veritabanları hakkında
> * İlgili faturalandırmayı durdurmak için örnek kaynakları silme


## <a name="additional-resources"></a>Ek kaynaklar

<!--* Additional [tutorials that build on the Wingtip SaaS application](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* To learn about elastic pools, see [*What is an Azure SQL elastic pool*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)
* To learn about elastic jobs, see [*Managing scaled-out cloud databases*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview) -->
* Çok kiracılı SaaS uygulamaları hakkında bilgi edinmek için bkz. [*Çok kiracılı SaaS uygulamaları için tasarım düzenleri*](saas-tenancy-app-design-patterns.md)
