---
title: "Azure Traffic Manager ile Azure web uygulaması trafiğini denetleme"
description: "Azure web uygulamaları için ilgili olarak bu makalede Azure trafik Yöneticisi için Özet bilgiler sağlanır."
services: app-service\web
documentationcenter: 
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
ms.openlocfilehash: 5bf687afa8f42292a3b21b19a572c76926fef1cd
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="controlling-azure-web-app-traffic-with-azure-traffic-manager"></a>Azure Traffic Manager ile Azure web uygulaması trafiğini denetleme
> [!NOTE]
> Azure Web uygulamaları için ilgili olarak bu makalede Microsoft Azure trafik Yöneticisi için Özet bilgiler sağlanır. Daha fazla bilgi hakkında Azure Traffic Manager kendisi bu makalenin sonunda bağlantıları ziyaret ederek bulunabilir.
> 
> 

## <a name="introduction"></a>Giriş
Azure App Service'te web uygulamalarını web istemcilerinden gelen istekleri nasıl dağıtıldığını denetlemek için Azure Traffic Manager kullanabilirsiniz. Web uygulama uç noktaları için bir Azure Traffic Manager profili eklendiğinde, Azure trafik Yöneticisi (durdurulmuş veya silinen çalışan), web uygulamalarınızı durumunu böylece bu uç hangisinin trafik alması gereken karar verebilirsiniz izler.

## <a name="routing-methods"></a>yönlendirme yöntemleri
Azure Traffic Manager üç farklı yönlendirme yöntemleri kullanır. Azure web uygulamaları için ilgilidir gibi bu yöntemler aşağıdaki listede açıklanmaktadır.

* **[Öncelik](#priority):** tüm trafiği için bir birincil web uygulamasını kullanmaya ve birincil veya yedek web uygulamaları kullanılamaz durumda yedeklemeler sağlar.
* **[Ağırlıklı](#weighted):** eşit ya da tanımladığınız ağırlıklara göre trafiği web uygulamaları, bir dizi arasında dağıtın.
* **[Performans](#performance):** web uygulamaları farklı coğrafi konumlarda olduğunda en düşük ağ gecikmesini bakımından "en yakın" web uygulaması kullanın.
* **[Coğrafi](#geographic):** göre belirli web uygulamaları için doğrudan kullanıcılara kendi DNS sorgusu hangi coğrafi konum kaynaklanan. 

Daha fazla bilgi için bkz: [Traffic Manager yönlendirme yöntemleri](../traffic-manager/traffic-manager-routing-methods.md).

## <a name="web-apps-and-traffic-manager-profiles"></a>Web uygulamaları ve trafik Yöneticisi profilleri
Web uygulaması trafik yapılandırmak için kullanır ve üç birini daha önce açıklanan karşı yöntemleri yük Azure trafik Yöneticisi'nde bir profil oluşturmak ve trafik profili için denetlemek istediğiniz uç noktaları (Bu durumda, web uygulamaları) ekleyin. Web uygulama durumunu (çalışıyor, durduruldu veya silinmiş) böylece Azure Traffic Manager trafik uygun şekilde yönlendirebilirsiniz profiline düzenli olarak bildirilir.

Azure Traffic Manager ile Azure kullanırken aşağıdaki noktaları göz önünde bulundurun:

* Web uygulaması yalnızca dağıtımları için aynı bölge içinde Web uygulamaları zaten yük devretme ve hepsini işlevselliği web uygulama modu dikkate almaksızın sağlar.
* Web uygulamaları başka bir Azure bulut hizmeti ile birlikte kullanan dağıtımlar için aynı bölgede, iki tür karma senaryoları etkinleştirmek için uç noktalar birleştirebilirsiniz.
* Bu gibi durumlarda, her bölge bir web uygulaması uç noktası yalnızca bir profil belirtebilirsiniz. Bir bölge için bir uç nokta olarak bir web uygulaması seçtiğinizde, bu bölgedeki diğer web uygulamaları bu profil için seçilmek kullanılamaz duruma gelir.
* Bir Azure Traffic Manager profilini belirtin web uygulama uç noktaları altında görünür **etki alanı adları** bölümünde Yapılandır sayfasında profil web uygulamasında ancak değil için yapılandırılabilir vardır.
* Bir profil için bir web uygulaması ekledikten sonra **Site URL'si** bir ayarlamış olduğunuz web Panoda uygulamanızın portal sayfası web uygulamasının özel etki alanı URL'sini görüntüler. Aksi durumda, trafik Yöneticisi Profil URL'sini görüntüler (örneğin, `contoso.trafficmanager.net`). Her iki doğrudan etki alanı adını web uygulamasını ve trafik Yöneticisi URL web uygulamanızın yapılandırma sayfasında altında görünür **etki alanı adları** bölümü.
* Özel etki alanı beklendiği gibi ancak bunları web uygulamalarınızdan ekleme yanı sıra iş, DNS haritanızı trafik Yöneticisi URL'sine işaret edecek şekilde yapılandırmanız da gerekir. Azure web uygulaması için özel bir etki alanı ayarlama hakkında daha fazla bilgi için bkz: [bir Azure web sitesi için bir özel etki alanı adı yapılandırma](app-service-web-tutorial-custom-domain.md).
* Yalnızca standart veya premium modunda bir Azure Traffic Manager profilini web uygulamaları da ekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar
Bir kavramsal ve teknik genel bakış Azure trafik Yöneticisi'nin için bkz: [trafik Yöneticisi'ne genel bakış](../traffic-manager/traffic-manager-overview.md).

Trafik Yöneticisi Web uygulamaları ile kullanma hakkında daha fazla bilgi için bkz. blog gönderileri [kullanarak Azure Traffic Manager ile Azure Web siteleri](http://blogs.msdn.com/b/waws/archive/2014/03/18/using-windows-azure-traffic-manager-with-waws.aspx) ve [Azure Traffic Manager artık Azure Web siteleri ile tümleştirmek](https://azure.microsoft.com/blog/2014/03/27/azure-traffic-manager-can-now-integrate-with-azure-web-sites/).

