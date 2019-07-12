---
title: Azure'da özel eylemler ve kaynakları oluşturma
description: Bu öğreticide, Azure Resource Manager'da özel eylemler ve kaynakları oluşturma ve bunları Azure Resource Manager şablonları, Azure CLI, Azure İlkesi ve etkinlik günlüğü için özel iş akışlarınızla tümleştirme nasıl üzerinden geçer.
author: jjbfour
ms.service: managed-applications
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: 4bbfcf070611e3df5c0fe47070f2ab6961111e07
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67800046"
---
# <a name="create-custom-actions-and-resources-in-azure"></a>Azure'da özel eylemler ve kaynakları oluşturma

Özel sağlayıcılar azure'da iş akışlarını özelleştirmenizi sağlar. Özel bir sağlayıcı Azure arasında bir sözleşmedir ve `endpoint`. Yeni dağıtım ve yönetim özellikleri sağlamak üzere Azure Resource Manager ile yeni özel API eklenmesini sağlar. Bu öğreticide, Azure için yeni eylemler ve kaynakları ekleme ve bunları tümleştirmek nasıl basit bir örnek geçer.

Bu öğreticide aşağıdaki adımlar ayrılır:

- Azure işlevleri için Azure özel sağlayıcılar Kurulumu
- Bir RESTful uç noktası için özel sağlayıcılar yazma
- Oluşturma ve özel bir sağlayıcı faydalanma

## <a name="setup-azure-functions-for-azure-custom-providers"></a>Azure işlevleri için Azure özel sağlayıcılar Kurulumu

Öğreticinin bu bölümünde, özel sağlayıcıları ile çalışmak için bir Azure işlevi ayarlama hakkında daha fazla ayrıntıya gider. Özel sağlayıcılar, genel bir URL ile çalışabilirsiniz.

- [Azure işlevleri için Azure özel sağlayıcılar Kurulumu](./tutorial-custom-providers-function-setup.md)

## <a name="authoring-a-restful-endpoint-for-custom-providers"></a>Bir RESTful uç noktası için özel sağlayıcılar yazma

Öğreticinin bu bölümünde, bir RESTful uç noktası için özel sağlayıcılar yazma hakkında ayrıntıya gider.

- [Bir RESTful uç noktası için özel sağlayıcılar yazma](./tutorial-custom-providers-function-authoring.md)

## <a name="creating-and-utilizing-the-custom-provider"></a>Oluşturma ve özel bir sağlayıcı faydalanma

Öğreticinin bu bölümünde, kendi özel eylemler ve kaynakları kullanın ve özel bir sağlayıcı oluşturma hakkında daha fazla ayrıntıya gider.

- [Azure özel sağlayıcısı oluşturma ve kullanma](./tutorial-custom-providers-create.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, özel sağlayıcılar ve bir yapı hakkında öğrendiniz. Öğreticinin sonraki adıma devam etmek için:

- [Öğretici: Azure işlevleri için Azure özel sağlayıcılar Kurulumu](./tutorial-custom-providers-function-setup.md)

Başvuruları veya bir hızlı başlangıç için kullanmak istiyorsanız, bazı yararlı bağlantıları aşağıda verilmiştir:

- [Hızlı Başlangıç: Azure özel kaynak sağlayıcısı oluşturursanız ve özel kaynakları dağıtma](./create-custom-provider.md)
- [Nasıl Yapılır: Azure REST API'si için özel eylemler ekleme](./custom-providers-action-endpoint-how-to.md)
- [Nasıl Yapılır: Azure REST API'si için özel kaynak ekleme](./custom-providers-resources-endpoint-how-to.md)
