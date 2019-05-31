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
ms.openlocfilehash: 53f65ec91a1e0f1e5a6322f0125bf83cd3e400b2
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399098"
---
# <a name="supported-azure-blockchain-service-ledger-versions"></a>Desteklenen Azure blok zinciri hizmet muhasebe sürümleri

Azure Blockchain hizmetinin kullandığı Ethereum tabanlı [çekirdek](https://www.goquorum.com/developers) bilinen katılımcılar, Azure Blockchain hizmetinde Konsorsiyumu olarak tanımlanan bir grup içinde özel hareketler işlenmesi için tasarlanmış muhasebe.

Şu anda Azure blok zinciri hizmet destekler [çekirdek sürümü 2.2.1](https://github.com/jpmorganchase/quorum/releases/tag/v2.2.1) ve [Tessera hareket yöneticisi](https://github.com/jpmorganchase/tessera).

## <a name="managing-updates-and-upgrades"></a>Yönetme güncelleştirmeler ve yükseltmeler

Bir ana, alt sürüm oluşturma çekirdek gerçekleştirilir ve düzeltme eki serbest bırakır. Çekirdek sürümü 2.0.1 ise, örneğin, sürüm türü gibi kategorilere:

|Büyük | Küçük  | Düzeltme Eki  |
| :--- | :----- | :----- |
| 2 | 0 | 1 | 

Azure Blockchain hizmeti çalıştıran var olan üyelerine düzeltme eki çekirdek sürümleri çekirdek kullanıma sunulacaktır, 30 gün içinde otomatik olarak güncelleştirir.

## <a name="availability-of-new-ledger-versions"></a>Yeni kayıt defteri sürümlerinde kullanılabilirliği

Azure Blockchain hizmeti çekirdek üreticisinden olma 60 gün içinde çekirdek muhasebe en büyük ve küçük sürümlere sağlar. En fazla dört ikincil sürüm yeni üye ve consortium sağlanırken aralarından seçim yapabileceğiniz consortia için sağlanır. Bir ana için yükseltme veya ikincil sürüm şu anda desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Blockchain hizmet sınırları](limits.md)
