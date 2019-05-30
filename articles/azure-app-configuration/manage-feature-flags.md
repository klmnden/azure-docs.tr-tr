---
title: Özellik bayraklarını yönetmek için Azure uygulama yapılandırmasını kullanma Öğreticisi | Microsoft Docs
description: Bu öğreticide, özellik bayrakları ayrı olarak uygulamanızdan Azure uygulama yapılandırması kullanarak yönetmeyi öğrenin.
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
ms.openlocfilehash: b7fbf9add67a45c0db89fc11cee5c10bc537ab63
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393565"
---
# <a name="tutorial-manage-feature-flags-in-azure-app-configuration"></a>Öğretici: Azure uygulama yapılandırmasında özellik bayraklarını Yönet

Azure uygulama yapılandırmasında tüm özellik bayraklarını depolamak ve bunları tek bir yerden yönetebilirsiniz. Uygulama Yapılandırması kullanıcı Arabirimi adlı bir portal olan **özellik Yöneticisi** özellik bayrakları için özel olarak tasarlanmıştır. Uygulama yapılandırması, .NET Core özellik bayrağını veri şemasını da yerel olarak destekler.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Tanımlayın ve uygulama yapılandırması özellik bayrakları yönetin.
> * Özellik bayrakları, uygulamanızdan erişin.

## <a name="create-feature-flags"></a>Özellik bayraklarını oluşturma

Azure portalında uygulama yapılandırması için özellik Yöneticisi, oluşturmak ve uygulamalarınızda kullandığınız özellik bayraklarını yönetmek için bir kullanıcı Arabirimi sağlar.

Yeni bir özellik bayrağını eklemek için:

1. Seçin **özellik Yöneticisi** >  **+ Ekle** bir özellik bayrağını eklemek için.

    ![Özellik bayrağı listesi](./media/azure-app-configuration-feature-flags.png)

1. Özellik bayrağı için benzersiz bir anahtar adı girin. Kodunuzu bayrağı başvurmak için bu ada ihtiyacınız var.

1. Özellik bayrağı, isterseniz bir açıklama girin.

1. Yapının başlangıç durumunun özellik bayrağını ayarlayın. Bu durumda genellikle *kapalı* veya *üzerinde*. *Üzerinde* durumu değişiklikleri *koşullu* özellik bayrağı şu şekilde bir filtre eklerseniz.

    ![Özellik bayrağı oluşturma](./media/azure-app-configuration-feature-flag-create.png)

1. Durum olduğunda *üzerinde*seçin **+ Filtre Ekle** durumu nitelemek için tüm ek koşulları belirtmek için. Bir yerleşik veya özel filtre anahtarı girin ve ardından **+ parametre Ekle** bir veya daha fazla parametre filtre ile ilişkilendirilecek. Yerleşik filtreler aşağıdakileri içerir:

    | Anahtar | JSON parametreleri |
    |---|---|
    | Microsoft.Percentage | {"Value": 0-%100} |
    | Microsoft.TimeWindow | {"Başlat": UTC saati, "Bitiş": UTC saati} |

    ![Özellik bayrağı filtresi](./media/azure-app-configuration-feature-flag-filter.png)

## <a name="update-feature-flag-states"></a>Özellik bayrağı durumları güncelleştirme

Özellik bayrağı ait durum değeri değiştirmek için:

1. Seçin **özellik Yöneticisi**.

1. Değiştirmek istediğiniz bir özellik bayrağını sağında, üç noktayı seçin ( **...** ) ve ardından **Düzenle**.

1. Yeni bir durum için özellik bayrağını ayarlayın.

## <a name="access-feature-flags"></a>Erişim özellik bayrakları

Özellik Yöneticisi tarafından oluşturulan özellik bayraklarını depolanır ve normal anahtar değerler olarak alınır. Bir özel ad alanı öneki altında kalmasını `.appconfig.featureflag`. Temel anahtar değerlerine görüntülemek için yapılandırma Gezgini'ni kullanın. Uygulamanızı, uygulama yapılandırmasını yapılandırma sağlayıcıları, SDK'ları, komut satırı uzantıları ve REST API'leri kullanarak bu değerleri alabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, uygulama yapılandırmasını kullanarak özellik bayrakları ve durumlarını yönetme öğrendiniz. Uygulama Yapılandırması ve ASP.NET Core management özellik desteği hakkında daha fazla bilgi için şu makaleye bakın:

* [Özellik bayrakları ASP.NET Core uygulaması kullanın.](./use-feature-flags-dotnet-core.md)
