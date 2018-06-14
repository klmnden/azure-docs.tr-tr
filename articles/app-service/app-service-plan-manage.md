---
title: Azure uygulama hizmeti planında yönetme | Microsoft Docs
description: Bir uygulama hizmeti planı yönetmek için farklı görevleri nasıl gerçekleştireceğinizi öğrenin.
keywords: uygulama hizmeti, azure app service, Ölçek, uygulama hizmeti planı, değiştirme, oluşturma, yönetme, yönetimi
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
ms.openlocfilehash: 1dfe8a903e19ff524a1c4a0228e6aefcbe9ff183
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
ms.locfileid: "29117689"
---
# <a name="manage-an-app-service-plan-in-azure"></a>Azure uygulama hizmeti planında yönetme

Bir [Azure uygulama hizmeti planı](azure-web-sites-web-hosting-plans-in-depth-overview.md) çalıştırmak için bir App Service uygulaması gereken kaynakları sağlar. Bu kılavuz, bir uygulama hizmeti planı yönetme gösterir.

## <a name="create-an-app-service-plan"></a>App Service planı oluşturma

> [!TIP]
> Bir uygulama hizmeti ortamı varsa bkz [bir uygulama hizmeti ortamı'nda bir uygulama hizmeti planı oluştur](environment/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan).

Boş bir uygulama hizmeti planı oluşturabilir veya uygulaması oluşturma işleminin bir parçası olarak bir plan oluşturabilirsiniz.

1. İçinde [Azure portal](https://portal.azure.com)seçin **yeni** > **Web + mobil**ve ardından **Web uygulaması** ya da başka tür bir uygulama hizmeti uygulaması.

2. Var olan bir uygulama hizmeti planı seçin veya yeni uygulama için bir plan oluşturun.

   ![Azure Portalı'nda bir uygulama oluşturun.][createWebApp]

   Bir plan oluşturmak için:

   a. Seçin **[+] Yeni Oluştur**.

      ![Bir uygulama hizmeti planı oluşturun.][createASP] 

   b. İçin **uygulama hizmeti planı**, planın adını girin.

   c. İçin **konumu**, uygun bir konum seçin.

   d. İçin **fiyatlandırma katmanı**, hizmet için uygun bir fiyatlandırma katmanı seçin. Seçin **tüm görüntüle** daha seçenekleri gibi fiyatlandırma görünümüne **serbest** ve **paylaşılan**. Fiyatlandırma katmanı seçtikten sonra tıklatın **seçin** düğmesi.

<a name="move"></a>

## <a name="move-an-app-to-another-app-service-plan"></a>Bir uygulamayı başka bir uygulama hizmeti planına taşıma

Kaynak planı ve hedef plan sürece, bir uygulamayı başka bir uygulama hizmeti planına taşıyabilirsiniz _aynı kaynak grubunu ve coğrafi bölge_.

1. İçinde [Azure portal](https://portal.azure.com), taşımak istediğiniz uygulamayı bulun.

2. Menüsünde Ara **App Service planı** bölümü.

3. Seçin **değişiklik uygulama hizmeti planı** açmak için **uygulama hizmeti planı** Seçici.

   ![Uygulama hizmeti planı Seçici.][change] 

4. İçinde **uygulama hizmeti planı** mevcut bir plan bu uygulamaya taşımak için Seçici, seçin.   

> [!IMPORTANT]
> **Seçin uygulama hizmeti planı** sayfası aşağıdaki ölçütlere göre filtrelenen: 
> - Aynı kaynak grubunda var 
> - Aynı coğrafi bölgede var 
> - Aynı Web alanına var  
> 
> A _Web alanı_ sunucu kaynaklarını gruplandırması tanımlar uygulaması hizmetinde mantıksal bir yapıdır. Coğrafi bölge (örneğin, Batı ABD), uygulama hizmeti kullanan müşteriler ayırmak için birçok Web alanları içeriyor. Şu anda, uygulama hizmeti kaynakları Web alanları arasında taşınamaz. 
> 

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

Her plan kendi fiyatlandırma katmanı vardır. Örneğin, bir siteden taşıma bir **serbest** için katman bir **standart** katmanı özellikleri ve kaynakları kullanmak için atanmış tüm uygulamalar sağlayan **standart** katmanı. Ancak, bir uygulama daha yüksek katmanlı bir plandan alt katmanlı bir plana taşımak artık belirli özellikleri erişimi olduğu anlamına gelir. Hedef plan kullanılabilir olmayan bir özellik uygulamanızı kullanıyorsa, hangi özelliği kullanılabilir değil kullanımda olduğunu gösterir bir hata alıyorsunuz. 

Örneğin, uygulamalarınızın SSL sertifikalarını kullanıyorsa, bu hata iletisi görebilirsiniz:

`Cannot update the site with hostname '<app_name>' because its current SSL configuration 'SNI based SSL enabled' is not allowed in the target compute mode. Allowed SSL configuration is 'Disabled'.`

Hedef plan uygulama taşıyabilirsiniz önce bu durumda, ya da gerekir:
- Hedef plan fiyatlandırma katmanı ölçeklendirin **temel** ya da daha yüksek.
- Uygulamanıza tüm SSL bağlantılarını kaldırın.

## <a name="move-an-app-to-a-different-region"></a>Bir uygulama için farklı bir bölgeye taşıma

Uygulamanızın çalıştığı bölge içinde uygulama hizmeti planının bölgedir. Ancak, bir App Service planının bölgeyi değiştiremezsiniz. Uygulamanız farklı bir bölgede çalıştırmak istiyorsanız, bir uygulama alternatiftir kopyalama. Kopyalama, yeni veya var olan uygulama hizmeti planındaki tüm bölgelerdeki uygulamanızı bir kopyasını oluşturur.

Bulabileceğiniz **kopya uygulama** içinde **geliştirme araçları** menüsünün bölümünde.

> [!IMPORTANT]
> Kopyalama, bazı sınırlamalar vardır. Bunları hakkında bilgi edinebilirsiniz [Azure uygulama hizmeti kopyalama](app-service-web-app-cloning.md).

## <a name="scale-an-app-service-plan"></a>Ölçek bir uygulama hizmeti planı

Bir uygulama hizmeti ölçeklendirme planı fiyatlandırma katmanı bkz [Azure bir uygulamada ölçeklendirin](web-sites-scale.md).

Bir uygulamanın örnek sayısı ölçeklendirmek için bkz: [örnek sayısı el ile veya otomatik olarak ölçeklendirme](../monitoring-and-diagnostics/insights-how-to-scale.md).

<a name="delete"></a>

## <a name="delete-an-app-service-plan"></a>Bir uygulama hizmeti planını Sil

Bir uygulama hizmeti planında son uygulama sildiğinizde beklenmeyen ücretlerden kaçınmak için uygulama hizmeti de varsayılan olarak plan siler. Plan yerine tutmayı seçerseniz, plana değiştirmeniz gerektiğini **serbest** değil ücret ödersiniz şekilde katmanı.

> [!IMPORTANT]
> Yapılandırılmış VM örneği ayrılacak devam etmek için onlarla ilişkili hiç uygulamanız yok uygulama hizmeti planları hala ücretler uygulanır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir Azure uygulamasında ölçeklendirin](web-sites-scale.md)

[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
