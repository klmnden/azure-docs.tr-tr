---
title: include dosyası
description: include dosyası
services: app-service
author: ggailey777
ms.service: app-service
ms.topic: include
ms.date: 02/19/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 97387e24d5b55c1438a69da1a1fd0a9bc1720e47
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66136158"
---
1. **Çözüm Gezgini**'nde projeye sağ tıklayın ve **Yayımla**'yı seçin.

1. İçinde **Yayımla** iletişim kutusunda **Microsoft Azure App Service**, seçin **Yeni Oluştur**ve ardından **Yayımla**.

   ![Yayımlama hedefi seçme](./media/webjobs-publish-netcore/pick-publish-target.png)

1. İçinde **App Service Oluştur** iletişim kutusunda, görüntünün altındaki tabloda belirtilen barındırma ayarlarını kullanın:

    ![App Service iletişim kutusu oluşturma](./media/webjobs-publish-netcore/app-service-dialog.png)

    | Ayar      | Önerilen değer  | Açıklama                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Uygulama Adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı benzersiz şekilde tanımlayan ad. |
    | **Abonelik** | Aboneliğinizi seçin | Kullanılacak Azure aboneliği. |
    | **[Kaynak Grubu](../articles/azure-resource-manager/resource-group-overview.md)** | myResourceGroup |  İşlev uygulamanızın oluşturulacağı kaynak grubunun adı. Yeni kaynak grubu oluşturmak **Yeni**'yi seçin.|
    | **[Barındırma planı](../articles/app-service/overview-hosting-plans.md)** | App Service planı | [App Service planı](../articles/app-service/overview-hosting-plans.md), uygulamanızı barındıran web sunucusu grubunun konumunu, boyutunu ve özelliklerini belirtir. Web uygulamalarında ortak bir App Service planının kullanılacağı şekilde yapılandırma gerçekleştirerek birden fazla uygulama barındırdığınızda maliyet tasarrufu elde edebilirsiniz. App Service planları, bölgeye, örnek boyutu, Ölçek sayısı ve SKU (ücretsiz, paylaşılan, temel, standart veya Premium) tanımlayın. Seçin **yeni** yeni bir App Service planı oluşturmak için. |

1. Tıklayın **Oluştur** WebJob ve ilgili kaynakları şu ayarlarla oluşturacak ve proje kodunu dağıtın.