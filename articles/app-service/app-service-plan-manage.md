---
title: App Service planı - Azure'ı yönetme | Microsoft Docs
description: Bir App Service planı yönetmek için farklı görevleri nasıl gerçekleştireceğinizi öğrenin.
keywords: App service, azure app service, Ölçek, app service planı, Değiştir, oluşturma, yönetme, Yönetim
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.assetid: 4859d0d5-3e3c-40cc-96eb-f318b2c51a3d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 936abe80a66c1dbe99e7d8a255fe8995a2df0803
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60852302"
---
# <a name="manage-an-app-service-plan-in-azure"></a>Azure App Service planı yönetme

Bir [Azure App Service planı](overview-hosting-plans.md) bir App Service uygulaması çalıştırmak için gereken kaynakları sağlar. Bu kılavuz, bir App Service planı yönetme işlemi gösterilmektedir.

## <a name="create-an-app-service-plan"></a>App Service planı oluşturma

> [!TIP]
> App Service ortamı varsa, bkz: [bir App Service ortamında bir App Service planı oluşturma](environment/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan).

Boş bir App Service planı oluşturabilir veya uygulama oluşturmanın bir parçası bir plan oluşturabilirsiniz.

1. İçinde [Azure portalında](https://portal.azure.com)seçin **yeni** > **Web + mobil**ve ardından **Web uygulaması** veya başka tür bir App Service uygulaması.

2. Var olan bir App Service planı seçin veya yeni bir uygulama için bir plan oluşturun.

   ![Azure portalında bir uygulama oluşturun.][createWebApp]

   Bir plan oluşturmak için:

   a. Seçin **[+] Yeni Oluştur**.

      ![Bir App Service planı oluşturun.][createASP] 

   b. İçin **App Service planı**, planın adını girin.

   c. İçin **konumu**, uygun bir konum seçin.

   d. İçin **fiyatlandırma katmanı**, hizmet için uygun bir fiyatlandırma katmanı seçin. Seçin **tümünü görüntüle** seçenekleri gibi daha fazla fiyatlandırma görünümüne **ücretsiz** ve **paylaşılan**. Fiyatlandırma katmanını seçtikten sonra tıklayın **seçin** düğmesi.

<a name="move"></a>

## <a name="move-an-app-to-another-app-service-plan"></a>Bir uygulamayı başka bir App Service planına Taşı

Kaynak planı ve hedef plan içinde olduğu sürece, uygulama başka bir App Service planına taşıyabilirsiniz _aynı kaynak grubunda ve coğrafi bölgede_.

> [!NOTE]
> Azure, her yeni App Service planı dahili bir Web alanı adlı bir dağıtım birimi dağıtır. Her bölgede birçok için Web alanlarının olabilir ancak uygulamanız yalnızca aynı Web alanına oluşturulan planlar arasında taşıyabilirsiniz. App Service ortamı yalıtılmış bir Web alanı olduğundan uygulamaları planlarında farklı App Service ortamları arasında değil ancak aynı App Service Ortamı'nda planları arasında taşınabilir.
>
> Bir plan oluştururken istediğiniz Web alanı belirtemezsiniz, ancak bir plan var olan bir planı olarak aynı aboneliğindeki oluşturulduğunu sağlamak mümkündür. Kısaca, tüm planlar ile aynı kaynak grubu oluşturulur ve bölge birleşimi, aynı Web alanı dağıtılır. Örneğin, bir kaynak grubu ve bölgesinde B bir plan oluşturduysanız, daha sonra bir kaynak grubu ve bölgesinde B oluşturma herhangi bir plan ile aynı aboneliğindeki dağıtılır. Bunlar oluşturduktan sonra "aynı Web alanı" bir plana taşınamıyor şekilde planları için Web alanlarının taşıyamazsınız, başka bir plan olarak başka bir kaynak grubuna taşıyarak unutmayın.
> 

1. İçinde [Azure portalında](https://portal.azure.com), taşımak istediğiniz uygulamaya göz atın.

1. Menüde Ara **App Service planı** bölümü.

1. Seçin **değişiklik App Service planı** açmak için **App Service planı** Seçici.

   ![App Service planı Seçici.][change] 

1. İçinde **App Service planı** bu uygulamaya taşımak için mevcut bir planı Seçici, seçin.   

**Seçin App Service planı** sayfası aynı kaynak grubunu ve geçerli uygulamanın App Service planı coğrafi bölgede olan planları gösterir.

Her bir planı kendi fiyatlandırma katmanı vardır. Örneğin, bir siteden taşıma bir **ücretsiz** için katmanı bir **standart** katmanı sağlayan özellikler ve kaynakları kullanmak için kendisine atanan tüm uygulamalar **standart** katmanı. Ancak, uygulama daha yüksek katmanlı bir plandan daha düşük katmanlı bir plana taşımak artık belirli özelliklere erişim olduğu anlamına gelir. Uygulamanızı hedef plana kullanılabilir olmayan bir özellik kullanır, hangi özelliği kullanılamıyor kullanımda olduğunu gösteren bir hata alırsınız. 

Örneğin, uygulamalarınızı birini SSL sertifikalarını kullanıyorsa, bu hata iletisini görebilirsiniz:

`Cannot update the site with hostname '<app_name>' because its current SSL configuration 'SNI based SSL enabled' is not allowed in the target compute mode. Allowed SSL configuration is 'Disabled'.`

Hedef plan için uygulamayı taşımadan önce bu durumda, ya da ihtiyacınız vardır:
- Fiyatlandırma katmanı için hedef planın ölçeğini **temel** veya üzeri.
- Uygulamanıza tüm SSL bağlantıları kaldırın.

## <a name="move-an-app-to-a-different-region"></a>Bir uygulama farklı bir bölgeye Taşı

Uygulamanızın çalıştığı bölge içinde App Service planının bölgedir. Ancak, bir App Service planının bölgeyi değiştiremezsiniz. Farklı bir bölgede uygulamanızı çalıştırmak istiyorsanız, bir uygulama alternatiftir kopyalama. Kopyalama, yeni veya mevcut bir App Service planında herhangi bir bölgedeki uygulamanızı bir kopyasını oluşturur.

Bulabilirsiniz **uygulama kopyalama** içinde **geliştirme araçları** menüsünün bölümünde.

> [!IMPORTANT]
> Kopyalama işlemi, bazı sınırlamalar vardır. Bunlar hakkında bilgi edinebilirsiniz [Azure App Service uygulaması kopyalama](app-service-web-app-cloning.md).

## <a name="scale-an-app-service-plan"></a>Bir App Service planı ölçeklendirin

Bir App Service ' ölçeklendirme planı fiyatlandırma katmanı bkz [azure'da uygulamanın ölçeğini](web-sites-scale.md).

Bir uygulamanın örnek sayısını ölçeklendirmek için bkz: [örnek sayısını elle veya otomatik olarak ölçeklendirme](../monitoring-and-diagnostics/insights-how-to-scale.md).

<a name="delete"></a>

## <a name="delete-an-app-service-plan"></a>Bir App Service planını Sil

Bir App Service planındaki son uygulama sildiğinizde beklenmeyen ücretlerden kaçınmak için App Service Ayrıca varsayılan plan siler. Plan yerine tutmayı seçerseniz, plana değiştirmelisiniz **ücretsiz** değil ücretlendirilirsiniz şekilde katmanı.

> [!IMPORTANT]
> Yapılandırılmış sanal makine örneklerine ayrılacak devam etmek için ilişkili uygulama yok olan app Service planları yine de ücret.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure'da uygulamanın ölçeğini](web-sites-scale.md)

[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
