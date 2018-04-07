---
title: Çok kiracılı SaaS öğretici - Azure SQL veritabanı | Microsoft Docs
description: Dağıtma ve Azure SQL veritabanı kullanan bir tek başına tek Kiracı SaaS uygulaması keşfedin.
keywords: sql veritabanı öğreticisi
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 04/01/2018
ms.author: genemi
ms.openlocfilehash: 86a5bc31639cbbcdac1468f3bc2e35a547068882
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="deploy-and-explore-a-standalone-single-tenant-application-that-uses-azure-sql-database"></a>Dağıtma ve Azure SQL veritabanı kullanan bir tek başına tek Kiracı uygulama keşfedin

Bu öğreticide dağıtmak ve tek başına uygulamayı veya uygulama başına-Kiracı, düzeni kullanılarak geliştirilen Wingtip biletleri SaaS örnek uygulamayı inceleyin.  Uygulama, çok kiracılı SaaS senaryoları etkinleştirme basitleştirmek Azure SQL veritabanı özelliklerini göstermek için tasarlanmıştır.

Tek başına uygulamayı veya uygulama başına Kiracı desen uygulama örneğini her bir kiracı için dağıtır.  Her uygulama için belirli bir kiracı yapılandırılabilir ve ayrı Azure kaynak grubu içinde dağıtılabilir. Uygulamasının birden çok örneği, bir çok kiracılı çözüm sağlamak için sağlanır. Bu desen Kiracı yalıtımı en önemli öncelik olduğu kiracılar küçük sayılara uygundur. Azure kiracının abonelik dağıtılması için kaynakları izin ve yönetilen ortak programlar bir hizmet sağlayıcısı tarafından kiracının adına sahip. 

Bu öğreticide, Azure aboneliğinize üç kiracılar için üç bağımsız uygulamalar dağıtır.  Keşfetmek ve tek tek uygulama bileşenleri ile çalışmak için tam erişime sahip.

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
    <a href="http://aka.ms/deploywingtipsa-contoso" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a> &nbsp; **Contoso Concert Hall**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-dogwood" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a> &nbsp; **Dogwood Dojo**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-fabrikam" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a> &nbsp; **Fabrikam Jazz kulübü**

2. Her dağıtım için gerekli parametre değerlerini girin.

    > [!IMPORTANT]
    > Bazı kimlik doğrulama ve sunucu güvenlik duvarları Tanıtım amaçlı bilerek güvenli. **Yeni bir kaynak grubu oluşturma** her uygulama dağıtımı için.  Varolan bir kaynak grubu kullanmayın. Bu uygulama ya da oluşturur, herhangi bir kaynağa üretim için kullanmayın. İlgili faturalama durdurmak için uygulamalarla tamamladığınızda, tüm kaynak grupları silin.

    Kaynak adları yalnızca küçük harf, sayı ve kısa çizgi kullanmak en iyisidir.
    * İçin **kaynak grubu**, Yeni Oluştur'u seçin ve ardından kaynak grubu için küçük bir ad sağlayın. **Wingtip-sa -\<venueName\>-\<kullanıcı\>**  önerilen deseni.  İçin \<venueName\>, boşluk salonundan adıyla değiştirin. İçin \<kullanıcı\>, kullanıcı değeri aşağıdan değiştirin.  Bu desen ile kaynak grubu adları olabilir *wingtip sa contosoconcerthall af1*, *wingtip sa dogwooddojo af1*, *wingtip sa fabrikamjazzclub af1*.
    * Seçin bir **konumu** aşağı açılan listeden.

    * İçin **kullanıcı** -adınızın baş harflerini artı bir rakam gibi bir kısa kullanıcı değeri öneririz: Örneğin, *af1*.


3. **Uygulamayı dağıtın**.

    * Hüküm ve koşulları kabul için tıklatın.
    * **Satın al**’a tıklayın.

4. Tıklayarak, tüm üç dağıtımlarının durumunu izleme **bildirimleri** (zil simgesine arama kutusunun sağındaki). Uygulamaları dağıtma yaklaşık beş dakika sürer.


## <a name="run-the-applications"></a>Uygulamaları çalıştırma

Uygulama olayları konak görebildikleri gösterir.  Görebildikleri uygulama kiracılarıdır. Bunların olaylarını listelemek ve bilet satabilir için kişiselleştirilmiş bir web sitesi her yerini alır. Birlikte salonları, jazz Sinek ve Spor Sinek salonundan türleri içerir. Aşağıdaki örnekte salonundan 's web sitesinde gösterilen arka plan fotoğrafı salonundan türünü belirler.   Tek başına uygulama modeli her salonundan kendi tekil SQL veritabanı ile ayrı uygulama örneği vardır.

1. Her üç ayrı tarayıcı sekmeleri kiracılar Etkinlikler sayfasını açın:

    - http://events.contosoconcerthall.&lt;user&gt;.trafficmanager.net
    - http://events.dogwooddojo.&lt;user&gt;.trafficmanager.net
    - http://events.fabrikamjazzclub.&lt;user&gt;.trafficmanager.net

    (Her URL ile değiştirin &lt;kullanıcı&gt; dağıtımınızın kullanıcı değerine sahip.)

   ![Olaylar](./media/saas-standaloneapp-get-started-deploy/fabrikam.png)

Gelen istekleri, uygulamanın kullandığı dağıtımını denetlemek için [ *Azure Traffic Manager*](../traffic-manager/traffic-manager-overview.md). Her bir kiracı özel uygulama örneği URL'de Kiracı adı etki alanı adının bir parçası olarak içerir. Tüm Kiracı URL'leri özel **kullanıcı** değeri. URL aşağıdaki biçimde izleyin:
- http://events.&lt;venuename&gt;.&lt;user&gt;.trafficmanager.net

Her bir kiracının veritabanı **konumu** karşılık gelen dağıtılan uygulama uygulaması ayarları içinde bulunur.

Bir üretim ortamında genellikle bir CNAME DNS kaydı oluşturmanız [ *bir şirketin internet etki alanını işaret* ](../traffic-manager/traffic-manager-point-internet-domain.md) trafik Yöneticisi profili URL.


## <a name="explore-the-servers-and-tenant-databases"></a>Sunucuları araştırın ve Kiracı veritabanları

Dağıtılan kaynakların bazıları bakalım:

1. İçinde [Azure portal](http://portal.azure.com), kaynak grupları listesine göz atın.
2. Üç Kiracı kaynak grupları görmeniz gerekir.
3. Açık **wingtip-sa-fabrikam -&lt;kullanıcı&gt;**  Fabrikam Jazz kulübü dağıtımı için kaynakları içeren kaynak grubu.  **Fabrikamjazzclub -&lt;kullanıcı&gt;**  sunucusunu içeren **fabrikamjazzclub** veritabanı.

50 DTU her Kiracı veritabanıdır *tek başına* veritabanı.

## <a name="additional-resources"></a>Ek kaynaklar

<!--
* Additional [tutorials that build on the Wingtip SaaS application](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* To learn about elastic pools, see [*What is an Azure SQL elastic pool*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)
* To learn about elastic jobs, see [*Managing scaled-out cloud databases*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview)
-->

- Çok kiracılı SaaS uygulamaları hakkında bilgi edinmek için [tasarım desenleri çok kiracılı SaaS uygulamaları için](saas-tenancy-app-design-patterns.md).

 
## <a name="delete-resource-groups-to-stop-billing"></a>Faturalama durdurmak için kaynak gruplarını Sil ##

Örnek ile işiniz bittiğinde, ilişkili faturalama durdurmak için oluşturulan tüm kaynak grupları silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz:

> [!div class="checklist"]
> * Wingtip biletleri SaaS tek başına uygulamayı dağıtmak nasıl.
> * Sunucular ve veritabanları hakkında uygulaması olun.
> * Nasıl ilgili faturalama durdurmak için örnek kaynaklar silinir.

Ardından, deneyin [sağlamak ve Katalog](saas-standaloneapp-provision-and-catalog.md) içinde araştırmanız arası Kiracı senaryolarının şema yönetim ve Kiracı çözümlemeleri gibi çeşitli sağlayan kiracılar kataloğunu kullanma Öğreticisi.
 

