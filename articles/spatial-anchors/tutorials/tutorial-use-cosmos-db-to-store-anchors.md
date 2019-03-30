---
title: Öğretici - paylaşım Azure uzamsal Çıpasıyla oturumları ve cihazlar arasında bir Azure Cosmos DB arka ucu | Microsoft Docs
description: Bu öğreticide, Azure uzamsal bağlayıcılarını tanımlayıcıları Unity Android/iOS cihazlarını bir arka uç hizmeti ve Azure Cosmos DB ile paylaşılmasını öğrenin.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 17e9d19b755c1d3ac455d9fef8406e00de3a376d
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58628933"
---
# <a name="tutorial-share-azure-spatial-anchors-across-sessions-and-devices-with-an-azure-cosmos-db-back-end"></a>Öğretici: Paylaşım Azure uzamsal bağlayıcılarını oturumları ve cihazlar arasında bir Azure Cosmos DB ile arka uç

Bu öğreticide, nasıl kullanılacağını öğreneceksiniz [Azure uzamsal bağlayıcılarını](../overview.md) bir oturumu sırasında yer işaretleri oluşturmanız ve ardından aynı cihaz veya farklı bir başka bir oturum sırasında bulun. Örneğin, ikinci oturum başka bir gün olabilir. Bu aynı çıpalarını da aynı yerde ve aynı anda birden çok cihaz tarafından bulunamıyor.

![GIF gösteren nesne kalıcılığı](./media/persistence.gif)

[Azure uzamsal bağlayıcılarını](../overview.md) konumlarına cihazlar arasında zaman içinde kalıcı nesneler ile karma gerçeklik deneyimleri oluşturmak için kullanabileceğiniz bir platformlar arası Geliştirici hizmetidir. İşiniz bittiğinde, iki veya daha fazla cihaza dağıttığınız bir uygulamayı sahip olacaksınız. Bir örneği tarafından oluşturulan uzamsal bağlayıcılarını Azure Cosmos DB kullanarak kendi tanımlayıcıları başkalarıyla paylaşır.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Yer işaretleri, paylaşmak için kullanılan Azure Cosmos DB'de saklamadan azure'da ASP.NET Core web uygulaması dağıtın.
> * Yer işaretleri Web Uygulama Paylaşımı yararlanmak için Azure hızlı başlangıçlar Unity örneğinden AzureSpatialAnchorsLocalSharedDemo Sahne yapılandırın.
> * Bir veya daha fazla cihazlara uygulama dağıtın ve çalıştırın.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [Share Anchors Sample Prerequisites](../../../includes/spatial-anchors-share-sample-prereqs.md)]

Bu, Unity ve Azure Cosmos DB Bu öğreticide kullanacaksınız ancak uzamsal bağlayıcılarını tanımlayıcıları cihaz üzerinden paylaşmalarını ilişkin bir örnek size olduğu, hatalarının ayıklanabileceğini belirtmekte yarar. Kullanıcı, diğer diller ve arka uç teknolojilerinden aynı hedefe ulaşmak için kullanabilirsiniz. Ayrıca, bu öğreticide kullanılan ASP.NET Core web uygulaması, .NET Core 2.2 SDK'sını gerektirir. İçin Web Apps Windows düzgün çalışıyor, ancak şu anda Linux üzerinde Web Apps çalışmaz.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

[!INCLUDE [cosmos-db-create-dbaccount-table](../../../includes/cosmos-db-create-dbaccount-table.md)]

Kopyalama `Connection String` olduğundan buna ihtiyacınız olacak.

## <a name="open-the-sample-project-in-unity"></a>Örnek Proje içinde Unity açın

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

## <a name="deploy-the-sharing-anchors-service"></a>Paylaşım bağlayıcılarını hizmetini dağıtma

Projede açık ve Visual Studio ve açık `Sharing\SharingServiceSample` klasör.

### <a name="configure-the-service-to-use-your-azure-cosmos-db-database"></a>Hizmet, Azure Cosmos DB veritabanınızı kullanacak şekilde yapılandırma

İçinde **Çözüm Gezgini**açın `SharingService\Startup.cs`.

Bulun `#define INMEMORY_DEMO` hat çıkışı dosyası ve yorum üstünde. Dosyayı kaydedin.

İçinde **Çözüm Gezgini**açın `SharingService\appsettings.json`.

Bulun `StorageConnectionString` özelliği ve aynı olacak şekilde değeri ayarlayın `Connection String` içinde kopyaladığınız değeri [bir veritabanı hesabı adım oluşturma](#create-a-database-account). Dosyayı kaydedin.

[!INCLUDE [Publish Azure](../../../includes/spatial-anchors-publish-azure.md)]

[!INCLUDE [Run Share Anchors Sample](../../../includes/spatial-anchors-run-share-sample.md)]

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Cosmos DB bağlantı tanımlayıcıları cihaz üzerinden paylaşmalarını kullandınız. Azure uzamsal bağlayıcılarını Kitaplığı hakkında daha fazla bilgi için kılavuzumuza oluşturmak ve bağlantıları bulmak nasıl devam edin.

> [!div class="nextstepaction"]
> [Oluşturma ve Azure uzamsal bağlayıcılarını kullanarak yer işaretleri bulun](../create-locate-anchors-overview.md)
