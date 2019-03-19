---
title: Öğretici - oturumları ve Azure uzamsal yer işaretleri ve bir Azure Cosmos DB uç cihazları arasında paylaşma | Microsoft Docs
description: Bu öğreticide, Azure Cosmos DB ile bir arka uç hizmetiyle Unity Android/iOS cihazları arasındaki Azure uzamsal bağlantı tanımlayıcıları paylaşma hakkında bilgi edinin.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: b585b13f40be447a5c5a4b348efc28bf5171e210
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "57863194"
---
# <a name="tutorial-sharing-across-sessions-and-devices-with-azure-spatial-anchors-and-an-azure-cosmos-db-back-end"></a>Öğretici: Oturumlarda ve cihazlarda Azure uzamsal yer işaretleri ve bir Azure Cosmos DB uç arasında paylaşma

Bu öğreticide nasıl kullanılacağını gösterilecek [Azure uzamsal bağlayıcılarını](../overview.md) için:

1. Tek bir oturumda yer işaretleri oluşturmanız ve ardından aynı veya farklı cihazdaki başka bir oturumda bulun. Örneğin, başka bir gün.
2. Aynı anda aynı yerde birden çok cihaz bulunabilir bağlantıları oluşturun.

![Kalıcılığı](./media/persistence.gif)

[Azure uzamsal bağlayıcılarını](../overview.md) olan karma gerçeklik oluşturmanıza olanak sağlayan bir platformlar arası Geliştirici hizmeti deneyimleri konumlarına cihazlar arasında zaman içinde kalıcı nesneler kullanma. İşiniz bittiğinde, iki veya daha fazla cihaza dağıttığınız bir uygulamayı sahip olacaksınız. Azure uzamsal bir örneği tarafından oluşturulan bağlantıları başkalarına Cosmos DB kullanarak kendi tanımlayıcıları paylaşır.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Azure'da Cosmos DB'de saklamadan yer işaretleri, paylaşmak için kullanılan bir ASP.NET Core Web uygulaması dağıtın.
> * Yer işaretleri Web Uygulama Paylaşımı yararlanmak için dört Hızlı başlangıçtan Unity örneğinden içinde AzureSpatialAnchorsLocalSharedDemo Sahne yapılandırın.
> * Dağıtın ve bir veya daha fazla cihaza çalıştırın.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [Share Anchors Sample Prerequisites](../../../includes/spatial-anchors-share-sample-prereqs.md)]

Bu, Unity ve Azure Cosmos DB Bu öğreticide kullanacaksınız ancak bunu yalnızca Azure uzamsal bağlantı tanımlayıcıları diğer cihazlar arasında paylaşmak nasıl bir örnek gösterecek şekilde olduğunu fark değer olur. Diğer diller ve arka uç teknolojilerinden aynı hedefe ulaşmak için kullanabilirsiniz. Ayrıca, bu öğreticide kullanılan ASP.NET Core Web uygulaması üzerinde .NET Core 2.2 SDK bağımlılığı vardır. Normal Azure Web Apps üzerinde (Windows için) düzgün çalışıyor, ancak şu anda Linux için Azure Web Apps üzerinde çalışmaz.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

[!INCLUDE [cosmos-db-create-dbaccount-table](../../../includes/cosmos-db-create-dbaccount-table.md)]

Not `Connection String` daha sonra kullanılır.

## <a name="open-the-sample-project-in-unity"></a>Örnek Proje içinde Unity açın

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

## <a name="deploy-your-sharing-anchors-service"></a>Paylaşım, yer işaretleri hizmetini dağıtma

Visual Studio'yu açın ve proje açmak `Sharing\SharingServiceSample` klasör.

### <a name="configure-the-service-so-that-it-uses-your-cosmos-db"></a>Böylece, Cosmos DB kullanır hizmetini yapılandırma

İçinde **Çözüm Gezgini**açın `SharingService\Startup.cs`.

Bulun `#define INMEMORY_DEMO` dosyasının en üstüne satır ve yorum çıkarın. Dosyayı kaydedin.

İçinde **Çözüm Gezgini**açın `SharingService\appsettings.json`.

Bulun `StorageConnectionString` özellik ve değere kümesi `Connection String` Not içinde gerçekleştirdiğiniz [bir veritabanı hesabı adım oluşturma](#create-a-database-account). Dosyayı kaydedin.

[!INCLUDE [Publish Azure](../../../includes/spatial-anchors-publish-azure.md)]

[!INCLUDE [Run Share Anchors Sample](../../../includes/spatial-anchors-run-share-sample.md)]

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Cosmos DB bağlantı tanımlayıcıları cihaz üzerinden paylaşmalarını kullandınız. Azure uzamsal bağlayıcılarını Kitaplığı hakkında daha fazla bilgi için kılavuzumuza oluşturmak ve bağlantıları bulmak nasıl devam edin.

> [!div class="nextstepaction"]
> [Oluşturma ve Azure uzamsal bağlayıcılarını kullanarak yer işaretleri bulun](../create-locate-anchors-overview.md)
