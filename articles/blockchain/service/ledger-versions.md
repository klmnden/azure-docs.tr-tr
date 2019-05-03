---
title: Azure Blockchain hizmet desteklenen muhasebe sürümleri, düzeltme eki uygulama ve yükseltme
description: Düzeltme eki uygulama ve sistem tarafından yönetilen sistemler ve kullanıcı tarafından yönetilen yükseltmeleri ile ilgili ilkeler de dahil olmak üzere Azure blok zinciri Service'te desteklenen defterler sürümlerinin genel bakış.
services: azure-blockchain
keywords: Blok zinciri
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: janders
manager: femila
ms.openlocfilehash: 63c9a8b9e266dacbb0fb6faba50fb44ac9a4b46e
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027893"
---
# <a name="supported-azure-blockchain-service-ledger-versions"></a>Desteklenen Azure blok zinciri hizmet muhasebe sürümleri

Azure Blockchain hizmetinin kullandığı Ethereum tabanlı [çekirdek](https://github.com/jpmorganchase/quorum/wiki) bilinen katılımcılar, Azure Blockchain hizmetinde Konsorsiyumu olarak tanımlanan bir grup içinde özel hareketler işlenmesi için tasarlanmış muhasebe.

Şu anda Azure blok zinciri hizmet destekler [çekirdek sürümü 2.2.1](https://github.com/jpmorganchase/quorum/releases/tag/v2.2.1) ve [Tessera hareket yöneticisi](https://github.com/jpmorganchase/tessera).

## <a name="managing-updates-and-upgrades"></a>Yönetme güncelleştirmeler ve yükseltmeler

Çekirdek sürüm oluşturma bir ana sürüm, önemli nokta yayın ve küçük noktası yayın gerçekleştirilir. Örneğin, bir ana sürümüne ait çekirdek 2.0.1, sürüm 2 ise 0 önemli nokta sürümüdür ve küçük noktası sürüm 1. Hizmet, muhasebe küçük nokta sürümleri otomatik olarak düzeltme ekleri. Şu anda yükseltme birincil ve önemli nokta sürümleri desteklenmez.

## <a name="availability-of-new-ledger-versions"></a>Yeni kayıt defteri sürümlerinde kullanılabilirliği

Azure Blockchain hizmeti muhasebe üreticisinden olma 60 gün içinde muhasebe en son sürümlerini sağlar. Yeni üye ve consortium sağlanırken aralarından seçim yapabileceğiniz consortia için en fazla dört önemli nokta sürümleri sağlanır.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Blockchain hizmet sınırları](limits.md)
