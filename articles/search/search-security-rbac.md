---
title: Portalda Azure Search yönetim erişimi için RBAC rolleri ayarlama | Microsoft Docs
description: Rol tabanlı yönetim denetimini Azure portalında.
services: search
documentationcenter: ''
author: HeidiSteen
manager: cgronlun
editor: ''
tags: azure-portal
ms.assetid: ''
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 03/20/2018
ms.author: heidist
ms.openlocfilehash: a5f6395a0160402b3b0dfe95dc12b866854e70d9
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="set-rbac-roles-for-administrative-access"></a>Yönetici erişimi için RBAC rolleri Ayarla

Azure sağlayan bir [genel rol tabanlı yetkilendirme modelini](../active-directory/role-based-access-control-configure.md) tüm hizmetler için yönetilen portalı veya Resource Manager API'leri. Sahibi, katkıda bulunan ve okuyucu rollerini düzeyini belirleme *Hizmet Yönetim* Active Directory Kullanıcıları, grupları ve her role atanmış güvenlik sorumluları için. 

> [!Note]
> Dizin bölümlerini ya da bir alt belgelerin güvenliğini sağlamak için rol tabanlı erişim denetim yoktur. Kimlik tabanlı arama sonuçları üzerinden erişim için istek sahibinin erişim olmamalıdır Belgeler kaldırılıyor sonuçları kimliği tarafından kırpma için güvenlik filtreler oluşturabilirsiniz. Daha fazla bilgi için bkz: [güvenlik filtreleri](search-security-trimming-for-azure-search.md) ve [güvenli Active Directory ile](search-security-trimming-for-azure-search-with-aad.md).

## <a name="management-tasks-by-role"></a>Rol yönetim görevleri

Azure arama için rolleri aşağıdaki yönetim görevlerini desteklemek izin düzeyleri ile ilişkilidir:

| Rol | Görev |
| --- | --- |
| Sahip |Oluşturun veya hizmet veya hizmetinde, API anahtarları, dizinler, dizin oluşturucular, Dizin Oluşturucu veri kaynakları ve dizin oluşturucu zamanlamaları gibi herhangi bir nesnede silin.<p>Sayıları ve depolama boyutu da dahil olmak üzere hizmetin durumunu görüntüleyin.<p>Ekleyin veya rol üyeliğini (yalnızca bir sahibi rol üyeliğini yönetebilir) silin.<p>Abonelik yöneticileri ve hizmet sahiplerini otomatik üyelik sahipleri rolüne sahiptir. |
| Katılımcı |Aynı düzeyde erişim sahibi, RBAC rol yönetimi eksi. Örneğin, katılımcı oluşturmak veya nesneler, silmek veya görüntülemek ve yeniden [api anahtarlarından](search-security-api-keys.md), rol üyeliklerini değiştiremez ancak. |
| [Arama hizmeti katkıda bulunan yerleşik rolü](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles#search-service-contributor) | Katkıda bulunan rolü eşdeğerdir. |
| Okuyucu |Görünüm hizmeti essentials ve ölçümleri. Bu rolün üyeleri dizini, dizin oluşturucu, veri kaynağı veya anahtar bilgileri görüntüleyemezsiniz.  |

Rol Hizmeti uç noktası erişim hakkı vermeyin. Arama hizmeti işlemleri, dizin yönetimi, dizin oluşturma ve sorgular gibi arama verileri api anahtarlarından, değil rolleri denetlenir. Daha fazla bilgi için bkz: [API anahtarları Yönet](search-security-api-keys.md).

## <a name="see-also"></a>Ayrıca bkz.

+ [PowerShell’i kullanarak yönetme](search-manage-powershell.md) 
+ [Performans ve Azure Search'te iyileştirme](search-performance-optimization.md)
+ [Azure portalında rol tabanlı erişim denetimi ile çalışmaya başlama](../active-directory/role-based-access-control-what-is.md).