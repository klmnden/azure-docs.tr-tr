---
title: "Bir Azure API Management örneği oluşturma | Microsoft Docs"
description: "Yeni bir Azure API Management örneği oluşturmak için Bu öğreticide adımları izleyin."
services: api-management
documentationcenter: 
author: vladvino
manager: anneta
editor: 
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 08/17/2017
ms.author: apimpm
ms.openlocfilehash: 6433ea1f0eb6ad375402b998b4dfa80bded35c4b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-new-azure-api-management-service-instance"></a>Yeni bir Azure API Management hizmet örneği oluşturma

Bu öğretici kullanarak yeni bir API Management örneği oluşturmak için gereken adımları açıklar [Azure portal](https://portal.azure.com/).

## <a name="prerequisites"></a>Ön koşullar

+ Etkin bir Azure aboneliği.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-new-service"></a>Yeni bir hizmet oluşturun

1. İçinde [Azure portal](https://portal.azure.com/)seçin **yeni** > **Kurumsal tümleştirme** > **API management**.

    Alternatif olarak, seçin **yeni**, türü `API management` arama kutusu ve Enter tuşuna basın. **Oluştur**'a tıklayın.

2. İçinde **API Management hizmeti** penceresinde, benzersiz bir girin **adı** API Management hizmetiniz için. Bu adı daha sonra değiştirilemez.

    > [!TIP]
    > Hizmet adı biçiminde bir varsayılan etki alanı adı oluşturmak için kullanılan *{ad} .azure-api.net.* Özel etki alanı adınızı kullanmak istiyorsanız, bkz: [özel bir etki alanı yapılandırmak](configure-custom-domain.md). <br/>
    > Hizmet adı, hizmet ve karşılık gelen Azure kaynak başvurmak için kullanılır.

5. Seçin bir **abonelik** erişiminiz bulunan farklı Azure abonelikleri arasında.
6. **Kaynak Grubu**’nda yeni veya mevcut bir kaynağı seçin.  Kaynak grubu; yaşam döngüsünü, izinleri ve ilkeleri paylaşan kaynakların bir koleksiyonudur. [Burada](../azure-resource-manager/resource-group-overview.md#resource-groups) daha fazla bilgi edinin.
7. İçinde **konumu**, API Management hizmeti oluşturulduğu coğrafi bölgeyi seçin. Yalnızca API Management hizmeti kullanılabilen bölgeler açılır liste kutusunda görüntülenir. 
9. Girin bir **kuruluş adı**. Bu ad, bir basamak sayısını kullanılır. Örneğin, bildirim e-posta gönderen ve Geliştirici Portalı başlığı.
10. İçinde **yönetici e-posta**ayarlayın e-posta adresi tüm bildirimleri **API Management** gönderilir.
11. İçinde **fiyatlandırma katmanı**ayarlayın **Geliştirici** hizmet değerlendirmek için katmanı. Bu katman, üretim kullanımı için değildir. API Management katmanları ölçeklendirme hakkında daha fazla bilgi için bkz: [yükseltin ve ölçeklendirme](upgrade-and-scale.md).
12. **Oluştur**’u seçin.

    > [!TIP]
    > Genellikle, bir API Management hizmeti oluşturmak için 20 ve 30 dakika arasında alır. Seçme **panoya Sabitle** yeni oluşturulan hizmet daha kolay bulmayı kolaylaştırır.

## <a name="next-steps"></a>Sonraki adımlar

[Bir API Azure API Management ile yayımlama](#api-management-getstarted-publish-api.md)