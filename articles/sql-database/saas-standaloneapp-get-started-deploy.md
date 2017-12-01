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
ms.date: 11/29/2017
ms.author: genemi
ms.openlocfilehash: 164d98220f69a8c25d853baf1e07f9a11c200f16
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="deploy-and-explore-a-standalone-single-tenant-application-that-uses-azure-sql-database"></a>Dağıtma ve Azure SQL veritabanı kullanan bir tek başına tek Kiracı uygulama keşfedin

Bu öğreticide dağıtmak ve Wingtip biletleri SaaS tek başına uygulamayı inceleyin. Uygulama, etkinleştirme SaaS senaryoları basitleştirmek Azure SQL veritabanı özelliklerini göstermek için tasarlanmıştır.

Tek başına uygulama düzeni her bir kiracı için tek kiracılı uygulama ve tek Kiracı veritabanı içeren bir Azure kaynak grubu dağıtır.  Uygulamasının birden çok örneği, bir çok kiracılı çözüm sağlamak için sağlanabilir.

Bu öğreticide, kaynak grupları, Azure aboneliğinizin çeşitli kiracılar için dağıtın.  Bu desen kaynak gruplarının bir kiracının Azure aboneliğinize dağıtılması olanak tanır. Azure kiracının adına bir hizmet sağlayıcısı tarafından yönetilmek üzere bu kaynak gruplarını izin ortak programlar sahiptir. Hizmet sağlayıcısı, kiracının Abonelikteki bir yöneticidir.

Sonraki dağıtım bölümünde vardır üç mavi **Azure'a Dağıt** düğmeler. Her düğme başka bir örnek uygulamanın dağıtır. Her örneği için belirli bir kiracı özelleştirilir. Her düğmesine basıldığında karşılık gelen uygulama beş dakika içinde tam olarak dağıtılır.  Uygulamaları Azure aboneliğinizde dağıtılır.  Keşfetmek ve tek tek uygulama bileşenleri ile çalışmak için tam erişime sahip.

Uygulama kaynak kodu ve yönetim komut dosyaları kullanılabilir [WingtipTicketsSaaS StandaloneApp](https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp) GitHub depo.


Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
> * Wingtip biletleri SaaS tek başına uygulamayı dağıtmak nasıl.
> * Uygulama kaynak koduna ve yönetim komut dosyaları nereden.
> * Sunucular ve veritabanları hakkında uygulaması olun.

Ek öğreticileri yayınlanacaktır. Bunlar, bu uygulama deseni temel alınarak yönetim senaryoları çeşitli keşfetmek izin verir.   

## <a name="deploy-the-wingtip-tickets-saas-standalone-application"></a>Wingtip biletleri SaaS tek başına uygulamayı dağıtma

Uygulamayı üç sağlanan kiracılar için dağıtma:

1. Her mavi tıklatın **Azure'a Dağıt** dağıtım şablonu açmak için düğmeye [Azure portal](https://portal.azure.com). Her bir şablon iki parametre değerlerini gerektirir; Yeni bir kaynak grubu için bir ad ve bu dağıtım uygulamanın diğer dağıtımlardan ayıran bir kullanıcı adı. Sonraki adım, bu değerleri ayarlamak için Ayrıntılar sağlar.<br><br>
    <a href="http://aka.ms/deploywingtipsa-contoso" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>&nbsp; **Contoso birlikte Hall**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-dogwood" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>&nbsp; **Kızılcık Dojo**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-fabrikam" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>&nbsp; **Fabrikam Jazz kulübü**

2. Her dağıtım için gerekli parametre değerlerini girin.

    > [!IMPORTANT]
    > Bazı kimlik doğrulama ve sunucu güvenlik duvarları Tanıtım amaçlı bilerek güvenli. **Yeni bir kaynak grubu oluşturma** her uygulama dağıtımı için.  Varolan bir kaynak grubu kullanmayın. Bu uygulama ya da oluşturur, herhangi bir kaynağa üretim için kullanmayın. İlgili faturalama durdurmak için uygulamalarla tamamladığınızda, tüm kaynak grupları silin.

    Kaynak adları yalnızca küçük harf, sayı ve kısa çizgi kullanmak en iyisidir.
    * İçin **kaynak grubu** - seçin **Yeni Oluştur**, küçük harf girin **adı** kaynak grubu için.
        * Bir rakam ile izlenen adınızın baş harflerini ve ardından bir tire append öneririz: Örneğin, *wingtip sa af1*.
        * Seçin bir **konumu** aşağı açılan listeden.

    * İçin **kullanıcı** -adınızın baş harflerini artı bir rakam gibi kısa kullanıcı değeri seçmenizi öneririz: Örneğin, *af1*.


3. **Uygulamayı dağıtın**.

    * Hüküm ve koşulları kabul için tıklatın.
    * **Satın al**’a tıklayın.

4. İzleme, tüm üç dağıtımlarının dağıtım durumu tıklayarak **bildirimleri** (zil simgesine arama kutusunun sağındaki). Uygulama dağıtımı beş dakika sürer.


## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama olayları konak görebildikleri gösterir. Birlikte salonları, jazz Sinek ve Spor Sinek salonundan türleri içerir. Görebildikleri Wingtip biletleri uygulama müşterilerdir. Wingtip anahtarların görebildikleri olarak kayıtlı *kiracılar*. Bir kiracı olmak bir salonundan kolay bir yol listesi olayları ve müşterilerine bilet satabilir olanak verir. Bunların olaylarını listeler ve bilet satabilir için kişiselleştirilmiş bir web sitesine her yerini alır. Her bir kiracı diğer kiracılardan yalıtılmış ve bunları bağımsızdır. Perde arkasında her bir kiracı kendi tekil SQL veritabanı ile ayrı uygulama örneğini alır.

1. Her üç ayrı tarayıcı sekmeleri kiracılar Etkinlikler sayfasını açın:

    - http://Events.contosoconcerthall. &lt;Kullanıcı&gt;. trafficmanager.net
    - http://Events.dogwooddojo. &lt;Kullanıcı&gt;. trafficmanager.net
    - http://Events.fabrikamjazzclub. &lt;Kullanıcı&gt;. trafficmanager.net

    (Her URL ile değiştirin &lt;kullanıcı&gt; dağıtımınızın kullanıcı değerine sahip.)

   ![Olaylar](./media/saas-standaloneapp-get-started-deploy/fabrikam.png)

Gelen istekleri, uygulamanın kullandığı dağıtımını denetlemek için [ *Azure Traffic Manager*](../traffic-manager/traffic-manager-overview.md). Her bir kiracı özel uygulama örneği URL'de Kiracı adı etki alanı adının bir parçası olarak içerir. Tüm Kiracı URL'leri özel **kullanıcı** değeri. URL aşağıdaki biçimde izleyin:
- http://Events. &lt;venuename&gt;.&lt; Kullanıcı&gt;. trafficmanager.net

Her bir kiracının veritabanı **konumu** karşılık gelen dağıtılan uygulama uygulaması ayarları içinde bulunur.

Bir üretim ortamında genellikle bir CNAME DNS kaydı oluşturmanız [ *bir şirketin internet etki alanını işaret* ](../traffic-manager/traffic-manager-point-internet-domain.md) trafik Yöneticisi profili URL.


## <a name="explore-the-servers-and-tenant-databases"></a>Sunucuları araştırın ve Kiracı veritabanları

Dağıtılan kaynakların bazıları bakalım:

1. İçinde [Azure portal](http://portal.azure.com), kaynak grupları listesine göz atın.
2. Bkz: **wingtip-sa-katalog -&lt;kullanıcı&gt;**  kaynak grubu.
    - Bu kaynak grubundaki **katalog-sa -&lt;kullanıcı&gt;**  sunucusu dağıtılır. Sunucunun bulunduğu **tenantcatalog** veritabanı.
    - Ayrıca üç Kiracı kaynak grupları görmeniz gerekir.
3. Açık **wingtip-sa-fabrikam -&lt;kullanıcı&gt;**  Fabrikam Jazz kulübü dağıtımı için kaynakları içeren kaynak grubu.  **Fabrikamjazzclub -&lt;kullanıcı&gt;**  sunucusunu içeren **fabrikamjazzclub** veritabanı.

50 DTU her Kiracı veritabanıdır *tek başına* veritabanı.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz:

> [!div class="checklist"]
> * Wingtip biletleri SaaS tek başına uygulamayı dağıtmak nasıl.
> * Sunucular ve veritabanları hakkında uygulaması olun.
> * Nasıl ilgili faturalama durdurmak için örnek kaynaklar silinir.


## <a name="additional-resources"></a>Ek kaynaklar

<!--* Additional [tutorials that build on the Wingtip SaaS application](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* To learn about elastic pools, see [*What is an Azure SQL elastic pool*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)
* To learn about elastic jobs, see [*Managing scaled-out cloud databases*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview)
-->

- Çok kiracılı SaaS uygulamaları hakkında bilgi edinmek için [tasarım desenleri çok kiracılı SaaS uygulamaları için](saas-tenancy-app-design-patterns.md).
