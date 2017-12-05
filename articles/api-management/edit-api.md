---
title: "Azure portal ile bir API Düzenle | Microsoft Docs"
description: "Bu öğreticide, API Management (APIM) bir API düzenlemek için nasıl kullanılacağını gösterir."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/08/2017
ms.author: apimpm
ms.openlocfilehash: 362c36181da706e3fe0a27cc5ab262271c2a1e57
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="edit-an-api"></a>Bir API Düzenle

Bu öğreticideki adımlar, API Management (APIM) bir API düzenlemek için nasıl kullanılacağını gösterir. 

+ Bunu yapabilirsiniz ekleme, silme, APIM örneği işlemlerinde yeniden adlandırma. 
+ API swagger düzenleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

+ [Bir Azure API Management örneği oluşturma](get-started-create-service-instance.md)
+ [İçeri aktarma ve ilk API'nizi yayımlama](import-and-publish.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="edit-an-api-in-apim"></a>Bir API APIM Düzenle

![Bir API Düzenle](./media/edit-api/edit-api001.png)

1. Tıklatın **API'leri** sekmesi.
2. Daha önce aldığınız API'leri birini seçin.
3. Seçin **tasarım** sekmesi.
4. Düzenlemek istediğiniz bir işlem seçin.
5. İşlemi yeniden adlandırmak için seçin bir **kalem** içinde **ön uç** penceresi.

## <a name="update-the-swagger"></a>Swagger güncelleştir

Aşağıdaki adımları izleyerek Azure portalından, backbend API güncelleştirebilirsiniz:

1. Seçin **tüm işlemleri**
2. Kalem içinde tıklatın **ön uç** penceresi.

    ![Bir API Düzenle](./media/edit-api/edit-api002.png)

    API swagger görünür.

    ![Bir API Düzenle](./media/edit-api/edit-api003.png)

3. Swagger güncelleştirin.
4. Tuşuna **kaydetmek**.

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [APIM ilkesi örnekleri](policy-samples.md)
> [dönüştürme ve yayımlanan bir API koruyun](transform-api.md)