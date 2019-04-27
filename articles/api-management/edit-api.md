---
title: Azure portal ile API’yi düzenleme| Microsoft Docs
description: Bu öğreticide, bir API’yi düzenlemek için API Management’ın (APIM) nasıl kullanılacağı gösterilir.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/08/2017
ms.author: apimpm
ms.openlocfilehash: 6f1a0cf6025cb3a398ab93320c6fcb69b1e62429
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60558114"
---
# <a name="edit-an-api"></a>API’yi düzenleme

Bu öğreticideki adımlar, bir API’yi düzenlemek için API Management’ın (APIM) nasıl kullanılacağını gösterir. 

+ Bu işlemi, APIM örneğindeki işlemleri ekleyerek, silerek ve yeniden adlandırarak yapabilirsiniz. 
+ API’nizin swagger’ını düzenleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

+ [Azure API Management örneği oluşturma](get-started-create-service-instance.md)
+ [İlk API’nizi içeri aktarma ve yayımlama](import-and-publish.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="edit-an-api-in-apim"></a>APIM’de bir API’yi düzenleme

![API’yi düzenleme](./media/edit-api/edit-api001.png)

1. **API'ler** sekmesine tıklayın.
2. Daha önce içeri aktardığınız API'lerden birini seçin.
3. **Tasarım** sekmesini seçin.
4. Düzenlemek istediğiniz bir işlem seçin.
5. İşlemi yeniden adlandırmak için **Ön uç** penceresinden bir **kalem** seçim.

## <a name="update-the-swagger"></a>Swagger’ı güncelleştirme

Aşağıdaki adımları izleyerek Azure portaldan arka uç API’nizi güncelleştirebilirsiniz:

1. **Tüm işlemler**’i seçin
2. **Ön uç** penceresinde kaleme tıklayın.

    ![API’yi düzenleme](./media/edit-api/edit-api002.png)

    API’nizin swagger’ı görüntülenir.

    ![API’yi düzenleme](./media/edit-api/edit-api003.png)

3. Swagger’ı güncelleştirin.
4. **Kaydet**’e basın.

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [APIM ilkesi örnekleri](policy-samples.md)
> [Yayımlanan API’yi dönüştürme ve koruma](transform-api.md)