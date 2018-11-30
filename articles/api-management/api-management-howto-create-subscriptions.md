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
ms.openlocfilehash: 3f4e113125ec9644aac974e47996afe290e57cee
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52621852"
---
# <a name="how-to-create-subscriptions-in-azure-api-management"></a>Azure API Yönetimi'nde abonelikleri oluşturma

API'ler aracılığıyla Azure API Management (APIM) yayımlarken bu API'lerine erişimi güvenli hale getirmek için kolay ve en yaygın yolu abonelik anahtarlarını kullanmaktır. Diğer bir deyişle, yayımlanan API'leri kullanmak için gereken istemci uygulamalar geçerli bir abonelik anahtarı HTTP isteklerini bu API'lere çağrı yaparken içermelidir. API erişimi için bir abonelik anahtarı almak için bir abonelik gereklidir. Abonelikler hakkında daha fazla bilgi için bkz. [abonelikleri Azure API Management](api-management-subscriptions.md)

Bu makalede Azure portalında abonelikleri oluşturma adımlarını açıklar.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlayabilmeniz için gerekir:

+ [APIM örneği oluşturma](get-started-create-service-instance.md)
+ Anlamak [APIM abonelikleri](api-management-subscriptions.md)

## <a name="create-a-new-subscription"></a>Yeni Abonelik Oluştur

1. Tıklayarak **abonelikleri** sol taraftaki menüden
2. Tıklayın **Abonelik Ekle**
3. Abonelik adını sağlayın ve kapsam seçin
4. **Kaydet**’e tıklayın

![Esnek abonelikler](./media/api-management-subscriptions/flexible-subscription.png)

Abonelik oluşturulduktan sonra API anahtarları (birincil ve ikincil) bir çift sağlanan API'leri erişmek için.

## <a name="next-steps"></a>Sonraki adımlar
API Management hakkında daha fazla bilgi için:

+ Diğer bilgi [kavramları](api-management-terminology.md) API Management
+ İzleyin bizim [öğreticiler](import-and-publish.md) API yönetimi hakkında daha fazla bilgi edinmek için
+ Denetleme bizim [SSS sayfasını](api-management-faq.md) sık sorulan sorular için