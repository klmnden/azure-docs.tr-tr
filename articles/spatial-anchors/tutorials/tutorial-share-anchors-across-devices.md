---
title: Öğretici - paylaşım Azure uzamsal bağlayıcılarını oturumları ve cihazlar arasında | Microsoft Docs
description: Bu öğreticide, Azure uzamsal bağlantı tanımlayıcılar bir arka uç hizmetiyle Unity Android/iOS cihazlar arasında paylaşma hakkında bilgi edinin.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: ff9868dd7347812eb6ef566288ec364bc89b6955
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58629314"
---
# <a name="tutorial-share-azure-spatial-anchors-across-sessions-and-devices"></a>Öğretici: Azure uzamsal bağlayıcılarını oturumlarda ve cihazlarda paylaşma

Bu öğreticide, nasıl kullanılacağını öğreneceksiniz [Azure uzamsal bağlayıcılarını](../overview.md) bir oturumu sırasında yer işaretleri oluşturmanız ve bunları aynı cihaz veya farklı bir bulun. Bu aynı çıpalarını da aynı yerde ve aynı anda birden çok cihaz tarafından bulunamıyor.

![Kalıcılık](./media/persistence.gif)

Azure uzamsal bağlayıcılarını konumlarına cihazlar arasında zaman içinde kalıcı nesneler kullanarak karma gerçeklik deneyimleri oluşturmanıza olanak tanıyan platformlar arası Geliştirici hizmetidir. İşiniz bittiğinde, iki veya daha fazla cihaza dağıttığınız bir uygulamayı sahip olacaksınız. Azure uzamsal bir örneği tarafından oluşturulan bağlantıları başkalarına paylaşılabilir.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Yer işaretleri, paylaşmak için kullanılan bir süre için bunları bellekte depolanıyor azure'da ASP.NET Core Web uygulaması dağıtın.
> * Yer işaretleri Web Uygulama Paylaşımı yararlanmak için dört Hızlı başlangıçtan Unity örneğinden içinde AzureSpatialAnchorsLocalSharedDemo Sahne yapılandırın.
> * Dağıtın ve bir veya daha fazla cihaza çalıştırın.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [Share Anchors Sample Prerequisites](../../../includes/spatial-anchors-share-sample-prereqs.md)]

Bu, Unity ve ASP.NET Core Web uygulaması Bu öğreticide kullanacaksınız ancak bunu yalnızca Azure uzamsal bağlantı tanımlayıcıları diğer cihazlar arasında paylaşmak nasıl bir örnek gösterecek şekilde olduğunu fark değer olur. Diğer diller ve arka uç teknolojilerinden aynı hedefe ulaşmak için kullanabilirsiniz. Ayrıca, bu öğreticide kullanılan ASP.NET Core Web uygulaması üzerinde .NET Core 2.2 SDK bağımlılığı vardır. Normal Azure Web Apps üzerinde (Windows için) düzgün çalışıyor, ancak şu anda Linux için Azure Web Apps üzerinde çalışmaz.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project-in-unity"></a>Örnek Proje içinde Unity açın

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

## <a name="deploy-your-sharing-anchors-service"></a>Paylaşım, yer işaretleri hizmetini dağıtma

Visual Studio'yu açın ve proje açmak `Sharing\SharingServiceSample` klasör.

[!INCLUDE [Publish Azure](../../../includes/spatial-anchors-publish-azure.md)]

[!INCLUDE [Run Share Anchors Sample](../../../includes/spatial-anchors-run-share-sample.md)]

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir ASP.NET Core Web uygulaması, azure'da dağıtılan ve ardından yapılandırılmış ve bir Unity uygulamasını dağıtmışsınızdır. Uygulamayla uzamsal yer işaretleri oluşturulur ve ASP.NET Core Web uygulamanızı kullanarak diğer cihazlarla paylaşılan.

Böylece, paylaşılan uzamsal bağlantı tanımlayıcılarını depolamak için Azure Cosmos DB kullanır, ASP.NET Core Web uygulamanızı geliştirme konusunda daha fazla bilgi edinmek için sonraki öğreticiye devam edin. Azure Cosmos DB, ASP.NET Core Web uygulamanızı Kalıcılık sağlayacaktır. Bunun yapılması, bu nedenle uygulamanızın hemen bir bağlantı oluşturun ve web uygulamanızda depolanan bağlantı tanımlayıcısını kullanarak yeniden bulamaz için gün sonra geri dönün izin verir.

> [!div class="nextstepaction"]
> [Öğretici: Azure Cosmos DB Store bağlayıcılarını kullanın](./tutorial-use-cosmos-db-to-store-anchors.md)
