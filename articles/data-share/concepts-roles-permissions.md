---
title: Rolleri ve Azure veri paylaşımı önizlemesi için gereksinimleri
description: Rolleri ve Azure veri paylaşımı önizlemesi için gereksinimleri
author: joannapea
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: a5d70b9aa611b4f939cb46b5d25655edd818cb35
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807530"
---
# <a name="roles-and-requirements-for-azure-data-share-preview"></a>Rolleri ve Azure veri paylaşımı önizlemesi için gereksinimleri

Bu makalede, Azure veri paylaşma önizlemesi kabul edin ve Azure veri paylaşma Önizlemesi'ni kullanarak verileri almak için de kullanarak veri paylaşımı için gereken rolleri açıklanır. 

## <a name="roles-and-requirements"></a>Roller ve gereksinimler

Paylaşma veya Azure veri paylaşımı kullanarak verileri almak için Azure'da oturum açarken kullandığınız kullanıcı hesabının veri paylaşma izinleri, veri paylaşımı veya için verileri alma, depolama hesabına mümkün olması gerekir. Var olan bir izin genellikle budur **sahibi** rolüne ya da atanan Microsoft.Authorization/role atamaları/yazma izni olan özel bir rol. 

Ya da bir Azure depolama hesabına veri alma veya paylaşmak için depolama hesabı sahibi olmalıdır. Depolama hesabı oluşturmuş olsa bile, bu otomatik olarak, depolama hesabı sahipliğini tanımaz. Kendiniz için Azure depolama hesabınızın sahip rolü eklemek için aşağıdaki adımları izleyin.

1. Azure portalında depolama hesabına gidin
1. Seçin **erişim denetimi (IAM)**
1. Tıklayın **Ekle**
1. Sahip olarak ekleyin

Abonelikte sahip olduğunuz izinleri görüntülemek için Azure portalına gidin, sağ üst köşeden kullanıcı adınızı ve sonra **İzinler**’i seçin. Birden çok aboneliğe erişiminiz varsa uygun aboneliği seçin. 

## <a name="resource-provider-registration"></a>Kaynak Sağlayıcısı kaydı 

Azure veri paylaşımı daveti kabul ederken Microsoft.DataShare kaynak sağlayıcısındaki aboneliğinize elle kaydetmeniz gerekir. Azure aboneliğinize Microsoft.DataShare kaynak sağlayıcısını kaydetmek için aşağıdaki adımları izleyin. 

1. Azure portalında gidin **abonelikler**
1. Azure veri paylaşımı için kullanmakta olduğunuz aboneliği seçin
1. Tıklayarak **kaynak sağlayıcıları**
1. Microsoft.DataShare arayın
1. Tıklayın **kaydetme**

## <a name="next-steps"></a>Sonraki adımlar

- Azure - roller hakkında daha fazla bilgi [rol tanımları anlama](../role-based-access-control/role-definitions.md)

