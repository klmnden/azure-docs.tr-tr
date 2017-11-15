---
title: "Azure uygulama hizmeti planında yönetme | Microsoft Docs"
description: "Nasıl uygulama hizmeti planları bir uygulama hizmeti planı yönetmek için farklı görevleri öğrenin."
keywords: "uygulama hizmeti, azure app service, Ölçek, uygulama hizmeti planı, değiştirme, oluşturma, yönetme, yönetimi"
services: app-service
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 4859d0d5-3e3c-40cc-96eb-f318b2c51a3d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: cephalin
ms.openlocfilehash: c1b832895476e2f64bbae638db76f89890e5c804
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="manage-an-app-service-plan-in-azure"></a>Azure uygulama hizmeti planında yönetme

Bir [uygulama hizmeti planı](azure-web-sites-web-hosting-plans-in-depth-overview.md) bir uygulama hizmeti uygulamanın ihtiyacı çalıştırmak için kaynaklar sağlar. Nasıl yapılır bu kılavuz, bir uygulama hizmeti planı yönetme gösterir. 

## <a name="create-an-app-service-plan"></a>App Service planı oluşturma

> [!TIP]
> Bir uygulama hizmeti ortamı varsa bkz [bir uygulama hizmeti ortamı'nda bir uygulama hizmeti planı oluştur](environment/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan).

Boş bir uygulama hizmeti planı oluşturabilirsiniz veya uygulama oluşturmanın bir parçası olarak.

İçinde [Azure portal](https://portal.azure.com), tıklatın **yeni** > **Web + mobil**ve ardından **Web uygulaması** veya diğer uygulama hizmeti uygulaması türü.

![Azure Portalı'nda bir uygulama oluşturun.][createWebApp]

Ardından, var olan bir uygulama hizmeti planı seçin veya yeni uygulama için bir plan oluşturun.

 ![Bir uygulama hizmeti planı oluşturun.][createASP]

Bir uygulama hizmeti planı oluşturmak için tıklatın **[+] oluştur yeni**, türü **uygulama hizmeti planı** adlandırın ve ardından uygun bir seçin **konumu**. Tıklatın **fiyatlandırma katmanı**ve ardından hizmet için uygun bir fiyatlandırma katmanı seçin. Seçin **tüm görüntüle** daha seçenekleri gibi fiyatlandırma görünümüne **serbest** ve **paylaşılan**. 

Fiyatlandırma katmanı seçtikten sonra tıklatın **seçin** düğmesi.

<a name="move"></a>

## <a name="move-an-app-to-another-app-service-plan"></a>Bir uygulamayı başka bir uygulama hizmeti planına taşıma

Kaynak planı ve hedef plan sürece, bir uygulamayı başka bir uygulama hizmeti planına taşıyabilirsiniz _aynı kaynak grubunu ve coğrafi bölge_.

Bir uygulamayı başka bir plana taşımak için taşımak istediğiniz uygulama gidin [Azure portal](https://portal.azure.com).

İçinde **menü**, Ara **App Service planı** bölümü.

Seçin **değişiklik uygulama hizmeti planı** işlemini başlatmak için.

**Uygulama hizmeti planını Değiştir** açılır **uygulama hizmeti planı** Seçici. Bu uygulamaya taşımak için var olan bir planı seçin. 

> [!IMPORTANT] 
> **Seçin uygulama hizmeti planı** sayfası aşağıdaki ölçütlere göre filtrelenen: 
> - Aynı kaynak grubunda var 
> - Aynı coğrafi bölgede var 
> - Aynı Web alanına var  
> 
> A _Web alanı_ sunucu kaynaklarını gruplandırması tanımlar uygulaması hizmetinde mantıksal bir yapıdır. Coğrafi bölge (örneğin, Batı ABD), uygulama hizmeti kullanan müşteriler ayırmak için birçok Web alanları içeriyor. Şu anda, uygulama hizmeti kaynaklar arasında izlemiyor taşınmasını mümkün değil. 
> 

![Uygulama hizmeti planı Seçici.][change]

Her plan kendi fiyatlandırma katmanı vardır. Örneğin, bir siteden taşıma bir **serbest** için katman bir **standart** katmanı, özellikleri ve kaynakları kullanmak için atanmış tüm uygulamalar sağlayan **standart** katmanı. Ancak, bir uygulama daha yüksek katmanlı bir plandan daha düşük bir katmanlı plana taşıma artık belirli özellikleri erişimi olduğu anlamına gelir. Hedef plan kullanılabilir olmayan bir özellik uygulamanızı kullanıyorsa, hangi özelliği kullanılabilir değil kullanımda olduğunu gösterir bir hata alıyorsunuz. Örneğin, uygulamalarınızın SSL sertifikalarını kullanıyorsa, hata iletisi görebilirsiniz: `Cannot update the site with hostname '<app_name>' because its current SSL configuration 'SNI based SSL enabled' is not allowed in the target compute mode. Allowed SSL configuration is 'Disabled'.`fiyatlandırma katmanı için hedef planın ölçeklendirin gerek bu durumda, **temel** veya daha yüksek veya tüm SSL bağlantıları için kaldırmanız gerekir Hedef plan uygulama taşımadan önce uygulamanızın.

## <a name="move-an-app-to-a-different-region"></a>Bir uygulama için farklı bir bölgeye taşıma

Uygulamanızın çalıştığı bölge içinde uygulama hizmeti planının bölgedir. Ancak, bir App Service planının bölgeyi değiştiremezsiniz. Uygulamanız farklı bir bölgede çalıştırmak istiyorsanız, bir uygulama alternatiftir kopyalama. Kopyalama, yeni veya var olan uygulama hizmeti planındaki tüm bölgelerdeki uygulamanızı bir kopyasını oluşturur.

Bulabileceğiniz **kopya uygulama** içinde **geliştirme araçları** menüsünün bölümünde.

> [!IMPORTANT]
> Kopyalama sırasında hakkında bilgi edinebilirsiniz bazı sınırlamalara sahiptir [Azure uygulama hizmeti kopyalama](app-service-web-app-cloning.md).

## <a name="scale-an-app-service-plan"></a>Ölçek bir uygulama hizmeti planı

AH ölçeği uygulama hizmeti planı katmanı fiyatlandırma için bkz: [Azure bir uygulamada ölçeklendirin](web-sites-scale.md).

Bir uygulamanın örnek sayısı ölçeklendirmek için bkz: [örnek sayısı el ile veya otomatik olarak ölçeklendirme](../monitoring-and-diagnostics/insights-how-to-scale.md).

<a name="delete"></a>

## <a name="delete-an-app-service-plan"></a>Bir uygulama hizmeti planını Sil

Bir uygulama hizmeti planında son uygulama sildiğinizde beklenmeyen ücretlerden kaçınmak için uygulama hizmeti de varsayılan olarak plan siler. Varsa planı yerine tutmak için seçin, plana değiştirmelisiniz **serbest** ücret yok böylece katmanı.

> [!IMPORTANT]
> **Uygulama hizmeti planları** yapılandırılmış VM örneği yedek devam beri hala ücretlendirme onlara ilişkili hiçbir uygulamalar vardır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir Azure uygulamasında ölçeklendirin](web-sites-scale.md)

[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
