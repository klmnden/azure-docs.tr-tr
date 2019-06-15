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
ms.openlocfilehash: bc791fea1dfd184749e84cb7b7a912972c6a9f12
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60657621"
---
# <a name="create-subscriptions-in-azure-api-management"></a>Azure API Yönetimi'nde abonelikleri oluşturma

Azure API Management aracılığıyla API'leri yayımladığınızda, bu Abonelik anahtarları kolay ve bu API'lere güvenli erişim için ortak kullanmaktır. Bu API'lere giden çağrıların zaman yayımlanan API'leri kullanmak için gereken istemci uygulamaları HTTP isteklerini bir geçerli abonelik anahtarı eklemeniz gerekir. API erişimi için bir abonelik anahtarı almak için bir abonelik gereklidir. Abonelikler hakkında daha fazla bilgi için bkz. [abonelikleri Azure API Management'ta](api-management-subscriptions.md).

Bu makalede Azure portalında abonelikleri oluşturma adımlarını açıklar.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımlar için Önkoşullar aşağıdaki gibidir:

+ [Bir API Management örneği oluşturma](get-started-create-service-instance.md).
+ Anlamak [API Management abonelikleri](api-management-subscriptions.md).

## <a name="create-a-new-subscription"></a>Yeni Abonelik Oluştur

1. Seçin **abonelikleri** sol taraftaki menüden.
2. Seçin **Abonelik Ekle**.
3. Abonelik adını sağlayın ve kapsamı seçin.
4. İsteğe bağlı olarak, abonelik bir kullanıcıyla ilişkili kaldırılması gerekip gerekmediğini seçin.
5. **Kaydet**’i seçin.

![Esnek abonelikler](./media/api-management-subscriptions/flexible-subscription.png)

Abonelik oluşturduktan sonra iki API anahtarları API'lere sağlanır. Bir anahtarı birincil ve ikincil biridir. 

## <a name="next-steps"></a>Sonraki adımlar
API yönetimi hakkında daha fazla bilgi alın:

+ Diğer bilgi [kavramları](api-management-terminology.md) API Management.
+ İzleyin bizim [öğreticiler](import-and-publish.md) API yönetimi hakkında daha fazla bilgi edinmek için.
+ Denetleme bizim [SSS sayfasını](api-management-faq.md) sık sorulan sorular için.