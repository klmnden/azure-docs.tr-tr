---
title: Traffic Manager ile - Azure App Service trafiği denetleme
description: Bu makalede, Azure App Service bağlamında Azure Traffic Manager için Özet bilgiler sağlanır.
services: app-service\web
documentationcenter: ''
author: cephalin
writer: cephalin
manager: erikre
editor: mollybos
ms.assetid: dabda633-e72f-4dd4-bf1c-6e945da456fd
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/25/2016
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 79fe3bce558a8315f5fbf7dbc82a4979e8e24238
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59677451"
---
# <a name="controlling-azure-app-service-traffic-with-azure-traffic-manager"></a>Azure Traffic Manager ile Azure App Service trafiğini denetleme
> [!NOTE]
> Bu makalede, Azure App Service bağlamında Microsoft Azure Traffic Manager için Özet bilgiler sağlanır. Bu makalenin sonunda bağlantıları ziyaret ederek Azure Traffic Manager hakkında kendisini daha fazla bilgi bulunabilir.
> 
> 

## <a name="introduction"></a>Giriş
Azure App Service'teki uygulamalar için web istemcilerinden gelen istekleri nasıl dağıtıldığını denetlemek için Azure Traffic Manager'ı kullanabilirsiniz. App Service uç noktaları bir Azure Traffic Manager profiline eklendiğinde Azure Traffic Manager, App Service uygulamalarının durumunu (çalışıyor, durduruldu veya silindi) izleyerek trafik gönderilecek uç noktaları belirleyebilir.

## <a name="routing-methods"></a>Yönlendirme yöntemleri
Azure Traffic Manager, dört farklı yönlendirme yöntemlerini kullanır. Azure App Service'e ait oldukları gibi bu yöntemler aşağıdaki listede açıklanmıştır.

* **[Öncelik](../traffic-manager/traffic-manager-routing-methods.md#priority):** tüm trafiği için birincil bir uygulama kullanın ve birincil veya yedek uygulamalar kullanılamaz durumda yedeklemeler sağlar.
* **[Ağırlıklı](../traffic-manager/traffic-manager-routing-methods.md#weighted):** eşit ya da tanımladığınız ağırlıklara göre trafiği bir uygulamalar kümesi arasında dağıtmak.
* **[Performans](../traffic-manager/traffic-manager-routing-methods.md#performance):** uygulamaları farklı coğrafi konumlarda olduğunda, en düşük ağ gecikme süresi açısından "en yakın" uygulamayı kullanın.
* **[Coğrafi](../traffic-manager/traffic-manager-routing-methods.md#geographic):** belirli uygulamaları doğrudan kullanıcılara bağlı DNS sorgularını hangi coğrafi konum kaynaklanan. 

Daha fazla bilgi için [Traffic Manager yönlendirme yöntemleri](../traffic-manager/traffic-manager-routing-methods.md).

## <a name="app-service-and-traffic-manager-profiles"></a>App Service ve Azure Traffic Manager profilleri
App Service uygulama trafik yapılandırmak için bir Azure Traffic kullanır Dengeleme yöntemi daha önce açıklanan dört biri yük Manager profili oluşturun ve ardından istediğiniz trafiği denetlemek uç noktalar (Bu durumda, App Service) ekleyin profili. Uygulama durumunu (çalışıyor, durduruldu veya silindi) Azure Traffic Manager trafik buna göre yönlendirebilmesi profiline düzenli olarak iletilir.

Azure Traffic Manager ile Azure kullanırken aşağıdaki noktaları göz önünde bulundurun:

* Uygulama yalnızca dağıtımları için aynı bölge içinde App Service zaten yük devretme ve hepsini bir kez deneme işlevselliği bakılmaksızın uygulama modu sağlar.
* Başka bir Azure bulut hizmeti ile birlikte App Service kullanan dağıtımlar için aynı bölgede karma senaryoları etkinleştirmek için uç noktalar her iki türdeki birleştirebilirsiniz.
* Bu gibi durumlarda, bir App Service uç nokta bölge başına yalnızca bir profilinde belirtebilirsiniz. Bir bölge için bir uç nokta olarak bir uygulama seçtiğinizde, bu bölgedeki diğer uygulamalar bu profil için seçimi için kullanılamaz duruma gelir.
* Bir Azure Traffic Manager profilinde belirttiğiniz uygulama hizmet uç noktaları altında görünür **etki alanı adları** bölümünde Yapılandır sayfasında uygulama profilinde, ancak için yapılandırılabilir vardır.
* Bir profil için bir uygulamayı ekledikten sonra **Site URL'si** bir ayarlamış olduğunuz uygulamasının özel etki alanı URL'si uygulamanın portal sayfasının panosunda görüntülenir. Aksi takdirde, Traffic Manager profil URL'sini gösterir (örneğin, `contoso.trafficmanager.net`). Hem doğrudan etki alanı adı uygulamanın ve Traffic Manager URL'si altında uygulamanın yapılandırma sayfasında görünür **etki alanı adları** bölümü.
* Özel etki alanı adlarınızı beklendiği gibi ancak bunları uygulamalarınıza ekleyerek ek olarak iş, DNS haritanızı Traffic Manager URL'sine işaret edecek şekilde yapılandırmanız da gerekir. Bir App Service uygulaması için özel bir etki alanı ayarlama hakkında daha fazla bilgi için bkz: [mevcut bir özel DNS adını Azure App Service'e eşlemek](app-service-web-tutorial-custom-domain.md).
* Yalnızca standart veya premium modunda bir Azure Traffic Manager profili için uygulamalar ekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar
Bir kavramsal ve teknik genel bakış, Azure Traffic Manager için bkz: [Traffic Manager'a genel bakış](../traffic-manager/traffic-manager-overview.md).

Traffic Manager, App Service ile kullanma hakkında daha fazla bilgi için bkz. blog gönderilerini [kullanarak Azure Traffic Manager ile Azure Web siteleri](https://blogs.msdn.com/b/waws/archive/2014/03/18/using-windows-azure-traffic-manager-with-waws.aspx) ve [Azure Traffic Manager artık Azure Web siteleri ile tümleştirin](https://azure.microsoft.com/blog/2014/03/27/azure-traffic-manager-can-now-integrate-with-azure-web-sites/).

