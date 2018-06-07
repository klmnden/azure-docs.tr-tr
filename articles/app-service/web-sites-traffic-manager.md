---
title: Azure uygulama hizmeti trafiğini Azure Traffic Manager ile denetleme
description: Azure App Service'e ilgili olarak bu makalede Azure trafik Yöneticisi için Özet bilgiler sağlanır.
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
ms.openlocfilehash: 92ab7bf64445ff772f33a18e7f7946a7e0be333a
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34824049"
---
# <a name="controlling-azure-app-service-traffic-with-azure-traffic-manager"></a>Azure uygulama hizmeti trafiğini Azure Traffic Manager ile denetleme
> [!NOTE]
> Azure App Service'e ilgili olarak bu makalede Microsoft Azure trafik Yöneticisi için Özet bilgiler sağlanır. Daha fazla bilgi hakkında Azure Traffic Manager kendisi bu makalenin sonunda bağlantıları ziyaret ederek bulunabilir.
> 
> 

## <a name="introduction"></a>Giriş
Azure uygulama hizmetinde uygulamalar için web istemcilerinden gelen istekleri nasıl dağıtıldığını denetlemek için Azure Traffic Manager kullanabilirsiniz. Uygulama Hizmeti uç noktaları için bir Azure Traffic Manager profili eklendiğinde, Azure trafik Yöneticisi (durdurulmuş veya silinen çalışan), uygulama hizmeti uygulamalarınız durumunu böylece bu uç hangisinin trafik alması gereken karar verebilirsiniz izler.

## <a name="routing-methods"></a>Yönlendirme yöntemleri
Azure Traffic Manager dört farklı yönlendirme yöntemleri kullanır. Azure App Service'e ilgilidir gibi bu yöntemler aşağıdaki listede açıklanmaktadır.

* **[Öncelik](../traffic-manager/traffic-manager-routing-methods.md#priority):** tüm trafiği için birincil bir uygulama kullanma ve birincil veya yedek uygulamaları kullanılamaz durumda yedeklemeler sağlar.
* **[Ağırlıklı](../traffic-manager/traffic-manager-routing-methods.md#weighted):** eşit ya da tanımladığınız ağırlıklara göre trafiği uygulamalar, bir dizi arasında dağıtmak.
* **[Performans](../traffic-manager/traffic-manager-routing-methods.md#performance):** uygulamaları farklı coğrafi konumlarda olduğunda en düşük ağ gecikmesini bakımından "en yakın" uygulama kullanın.
* **[Coğrafi](../traffic-manager/traffic-manager-routing-methods.md#geographic):** göre belirli uygulamalar için doğrudan kullanıcılara kendi DNS sorgusu hangi coğrafi konum kaynaklanan. 

Daha fazla bilgi için bkz: [Traffic Manager yönlendirme yöntemleri](../traffic-manager/traffic-manager-routing-methods.md).

## <a name="app-service-and-traffic-manager-profiles"></a>Uygulama hizmeti ve trafik Yöneticisi profilleri
Uygulama hizmeti uygulaması trafik yapılandırmak için Azure trafik kullanır ve üç birini daha önce açıklanan karşı yöntemleri yük Yöneticisi'nde bir profil oluşturmak ve trafiği denetlemek istediğiniz uç noktaları (Bu durumda, uygulama hizmeti) ekleyin profili. Uygulama durumunu (çalışıyor, durduruldu veya silinmiş) böylece Azure Traffic Manager trafik uygun şekilde yönlendirebilirsiniz profiline düzenli olarak bildirilir.

Azure Traffic Manager ile Azure kullanırken aşağıdaki noktaları göz önünde bulundurun:

* Uygulama yalnızca dağıtımları için aynı bölge içinde uygulama hizmeti zaten yük devretme ve hepsini işlevselliği uygulama modu dikkate almaksızın sağlar.
* Uygulama hizmeti başka bir Azure bulut hizmeti ile birlikte kullanan dağıtımlar için aynı bölgede, iki tür karma senaryoları etkinleştirmek için uç noktalar birleştirebilirsiniz.
* Bu gibi durumlarda, her bölge bir uygulama Hizmeti uç noktası yalnızca bir profil belirtebilirsiniz. Bir bölge için bir uç nokta olarak bir uygulama seçtiğinizde, bu bölgedeki diğer uygulamalar bu profil için seçilmek kullanılamaz duruma gelir.
* Bir Azure Traffic Manager profilini belirttiğiniz Uygulama Hizmeti uç noktaları altında görünür **etki alanı adları** bölümünde Yapılandır sayfasında uygulama profilinde, ancak değil için yapılandırılabilir vardır.
* Bir profil için bir uygulama ekledikten sonra **Site URL'si** bir ayarlamış olduğunuz uygulamanın portal sayfası Panoda özel etki alanı URL'sini görüntüler. Aksi durumda, trafik Yöneticisi Profil URL'sini görüntüler (örneğin, `contoso.trafficmanager.net`). Her iki doğrudan etki alanı adını uygulama ve trafik Yöneticisi URL uygulamanın yapılandırma sayfasında altında görünür **etki alanı adları** bölümü.
* Özel etki alanı beklendiği gibi ancak uygulamalarınıza eklemeden yanı sıra iş, DNS haritanızı trafik Yöneticisi URL'sine işaret edecek şekilde yapılandırmanız da gerekir. Bir uygulama hizmeti uygulaması için özel bir etki alanı ayarlama hakkında daha fazla bilgi için bkz: [Azure Web uygulamaları için var olan bir özel DNS ad eşleme](app-service-web-tutorial-custom-domain.md).
* Yalnızca standart veya premium modunda bir Azure Traffic Manager profili için uygulamalar ekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar
Bir kavramsal ve teknik genel bakış Azure trafik Yöneticisi'nin için bkz: [trafik Yöneticisi'ne genel bakış](../traffic-manager/traffic-manager-overview.md).

Trafik Yöneticisi App Service ile kullanma hakkında daha fazla bilgi için blog gönderilerine bakın [kullanarak Azure Traffic Manager ile Azure Web siteleri](http://blogs.msdn.com/b/waws/archive/2014/03/18/using-windows-azure-traffic-manager-with-waws.aspx) ve [Azure Traffic Manager artık Azure Web siteleri ile tümleştirmek](https://azure.microsoft.com/blog/2014/03/27/azure-traffic-manager-can-now-integrate-with-azure-web-sites/).

