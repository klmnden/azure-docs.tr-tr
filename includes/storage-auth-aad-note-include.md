---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/06/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 3a3ee0cda3643ac73581f868e47905841d9f1563
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44095600"
---
> [!IMPORTANT]
> - Azure AD kimlik doğrulamasını BLOB'lar ve Kuyruklar önizlemesi yalnızca üretim dışı kullanması için tasarlanmıştır. Üretim hizmet düzeyi sözleşmeleri (SLA'lar) şu anda kullanılamıyor. Azure AD kimlik doğrulaması senaryonuz için henüz desteklenmiyor, uygulamalarınızda paylaşılan anahtar yetkilendirme veya SAS belirteçlerini kullanmaya devam.
>
> - Önizleme sırasında RBAC rolü atamalarını yayılması için beş dakika sürebilir.
>
> - Azure AD kimlik doğrulaması için HTTPS kullanmalıdır blob ve kuyruk işlemlerini çağırırken.
>
> - Önizleme sürümünde, Azure portalında Azure AD kimlik blob ve kuyruk veri okuma ve yazma için kullanmaz. Bunun yerine, portal, hesabı erişim anahtarını temel işleminden yararlanmaya devam eder. Görüntülemek veya Portalı'nda blob veya kuyruk verileri güncelleştirmek için kullanıcının kapsayan bir RBAC rolü atanmış [Microsoft.Storage/storageAccounts/listkeys/action](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-account-key-operator-service-role), çağırmak için izinler veren [- depolama hesapları Anahtarları Listele](https://docs.microsoft.com/rest/api/storagerp/storageaccounts/listkeys). BLOB'lar ve Kuyruklar için katkıda bulunan ve okuyucu rolleri şu anda içermeyen **listkeys'i** eylem önizlemesinin bir parçası olarak bırakın ve bu nedenle Azure Portalı aracılığıyla veri erişimi sağlamaz. Kimlik tabanlı erişim Önizleme sürümü sırasında blob ve kuyruk veri için keşfetmek için PowerShell veya Azure CLI'yı kullanın.
