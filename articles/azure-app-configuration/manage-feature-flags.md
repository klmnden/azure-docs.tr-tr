---
title: Özellik bayraklarını yönetmek için Azure uygulama yapılandırmasını kullanma Öğreticisi | Microsoft Docs
description: Bu öğreticide, özellik bayraklarını kullanarak Azure uygulama yapılandırması uygulamanızdan ayrı olarak yönetmek hakkında bilgi edinin
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 04/19/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: d995a2e9f0d05dc3b0853036e8fb3c04ccdfab96
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65412346"
---
# <a name="tutorial-manage-feature-flags-in-azure-app-configuration"></a>Öğretici: Azure uygulama yapılandırmasında özellik bayraklarını Yönet

Azure uygulama yapılandırmasında tüm özellik bayraklarını depolamak ve bunları tek bir yerden yönetebilirsiniz. Adlı bir portal kullanıcı Arabirimi, sahip **özellik Yöneticisi**, yani özellik bayrakları için özel olarak tasarlanmıştır. Ayrıca, uygulama yapılandırmasını yerel olarak .NET Core özellik bayrağı veri şemasını destekler.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Tanımlayın ve uygulama yapılandırması özellik bayrakları yönetin.
> * Özellik bayrakları, uygulamanızdan erişin.

## <a name="create-feature-flags"></a>Özellik bayraklarını oluşturma

**Özellik Yöneticisi** Azure portalında uygulama yapılandırması oluşturmak için bir kullanıcı Arabirimi sağlar ve flags özelliği yönetme, uygulamanızda kullanabilirsiniz. Yeni bir özellik bayrağını eklemek için aşağıdaki adımları izleyin.

1. Seçin **özellik Yöneticisi** > **+ Oluştur** bir özellik bayrağını eklemek için.

    ![Özellik bayrağı listesi](./media/azure-app-configuration-feature-flags.png)

2. Özellik bayrağı için benzersiz bir anahtar adı girin. Kodunuzu bayrağı başvurmak için bu ada ihtiyacınız var.

3. İsteğe bağlı olarak daha kolay İnsan açıklamasını özellik bayrağını verin.

4. Yapının başlangıç durumunun özellik bayrağını ayarlayın. Genellikle yalnızca olan *üzerinde* veya *kapalı*.

    ![Özellik bayrağı oluşturma](./media/azure-app-configuration-feature-flag-create.png)

5. Durum olduğunda *üzerinde*, isteğe bağlı olarak birlikte nitelemek için herhangi bir ek koşul belirtin **Filtre Ekle**. Bir yerleşik veya özel filtre anahtarı girin ve parametre ilişkilendirin. Yerleşik filtreler aşağıdakileri içerir:

    | Anahtar | JSON parametreleri |
    |---|---|
    | Microsoft.Percentage | {"Value": 0-%100} |
    | Microsoft.TimeWindow | {"Başlat": UTC saati, "Bitiş": UTC saati} |

    ![Özellik bayrağı filtresi](./media/azure-app-configuration-feature-flag-filter.png)

## <a name="update-feature-flag-states"></a>Özellik bayrağı durumları güncelleştirme

Özellik bayrağı ait durum değeri değiştirmek için aşağıdaki adımları izleyin.

1. Seçin **özellik Yöneticisi**.

2. Tıklayarak **...**   >  **Düzenle** değiştirmek istediğiniz bir özellik bayrağını sağında.

3. Yeni bir durum için özellik bayrağını ayarlayın.

## <a name="access-feature-flags"></a>Erişim özellik bayrakları

Özellik bayrakları tarafından oluşturulan **özellik Yöneticisi** depolanır ve normal anahtar-değer alınır. Bir özel ad alanı öneki altında kalmasını *. appconfig.featureflag*. Kullanarak temel alınan anahtar değerleri görüntüleyebilirsiniz **yapılandırması Gezgini**. Uygulamanızı uygulama yapılandırması yapılandırma sağlayıcıları, SDK'ları, komut satırı uzantıları ve REST API'leri kullanarak bunları alabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, özellik bayrakları ve uygulama yapılandırması kullanarak durumlarını yönetme öğrendiniz. Uygulama Yapılandırması ve ASP.NET Core özellik yönetim desteği hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın.

* [Özellik bayrakları ASP.NET Core uygulaması kullanın.](./use-feature-flags-dotnet-core.md)
