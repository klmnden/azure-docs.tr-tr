---
title: Oturumlarda ve cihazlarda Azure uzamsal yer işaretleri ile arasında paylaşımı Öğreticisi - | Microsoft Docs
description: Bu öğreticide, Azure uzamsal bağlantı tanımlayıcılar bir arka uç hizmetiyle Unity Android/iOS cihazlar arasında paylaşma hakkında bilgi edinin.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 7d9fe58b7db60513eed81aae628ebd7ca754a53a
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "57901313"
---
# <a name="tutorial-sharing-across-sessions-and-devices-with-azure-spatial-anchors"></a>Öğretici: Oturumlarda ve cihazlarda Azure uzamsal yer işaretleri ile arasında paylaşma

Bu öğreticide nasıl kullanılacağını gösterilecek [Azure uzamsal bağlayıcılarını](../overview.md) için:

1. Tek bir oturumda yer işaretleri oluşturmanız ve ardından aynı veya farklı cihazdaki başka bir oturumda bulun. Örneğin, başka bir gün.
2. Aynı anda aynı yerde birden çok cihaz bulunabilir bağlantıları oluşturun.

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

## <a name="open-the-sample-project-in-unity"></a>Örnek Proje içinde Unity açın

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Run Share Anchors Sample](../../../includes/spatial-anchors-run-share-sample.md)]

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir ASP.NET Core Web uygulaması, azure'da dağıtılan ve ardından yapılandırılmış ve bir Unity uygulamasını dağıtmışsınızdır. Uygulamayla uzamsal yer işaretleri oluşturulur ve ASP.NET Core Web uygulamanızı kullanarak diğer cihazlarla paylaşılan.

Böylece, paylaşılan uzamsal bağlayıcılarını depolamak için Azure Cosmos DB kullanır, ASP.NET Core Web uygulamanızı geliştirme konusunda daha fazla bilgi edinmek için sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Öğretici: Azure Cosmos DB Store bağlayıcılarını kullanın](./tutorial-use-cosmos-db-to-store-anchors.md)
