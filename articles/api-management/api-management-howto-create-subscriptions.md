---
title: Azure API Yönetimi'nde abonelikleri oluşturma | Microsoft Docs
description: Azure API Yönetimi'nde abonelikleri oluşturmayı öğrenin.
services: api-management
documentationcenter: ''
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2018
ms.author: apimpm
ms.openlocfilehash: 1393e548c46c23f6b50c1b18a274febb74914ae8
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54054521"
---
# <a name="create-subscriptions-in-azure-api-management"></a>Azure API Yönetimi'nde abonelikleri oluşturma

Azure API Management aracılığıyla API'leri yayımladığınızda, bu Abonelik anahtarları kolay ve bu API'lere güvenli erişim için ortak kullanmaktır. Bu API'lere giden çağrıların zaman yayımlanan API'leri kullanmak için gereken istemci uygulamaları HTTP isteklerini bir geçerli abonelik anahtarı eklemeniz gerekir. API erişimi için bir abonelik anahtarı almak için bir abonelik gereklidir. Abonelikler hakkında daha fazla bilgi için bkz. [abonelikleri Azure API Management'ta](api-management-subscriptions.md).

Bu makalede Azure portalında abonelikleri oluşturma adımlarını açıklar.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımlar için Önkoşullar aşağıdaki gibidir:

+ [Bir API Management örneği oluşturma](get-started-create-service-instance.md).
+ Anlamak [API Management abonelikleri](api-management-subscriptions.md).

## <a name="create-a-new-subscription"></a>Yeni abonelik oluştur

1. Seçin **abonelikleri** sol taraftaki menüden.
2. Seçin **Abonelik Ekle**.
3. Abonelik adını sağlayın ve kapsamı seçin.
4. **Kaydet**’i seçin.

![Esnek abonelikler](./media/api-management-subscriptions/flexible-subscription.png)

Abonelik oluşturduktan sonra iki API anahtarları API'lere sağlanır. Bir anahtarı birincil ve ikincil biridir. 

## <a name="next-steps"></a>Sonraki adımlar
API yönetimi hakkında daha fazla bilgi alın:

+ Diğer bilgi [kavramları](api-management-terminology.md) API Management.
+ İzleyin bizim [öğreticiler](import-and-publish.md) API yönetimi hakkında daha fazla bilgi edinmek için.
+ Denetleme bizim [SSS sayfasını](api-management-faq.md) sık sorulan sorular için.