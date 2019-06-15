---
title: Azure için ayırmaları yazılım maliyetleri | Microsoft Docs
description: Azure ayrılmış VM örneği maliyetlerini hangi yazılım ölçümleri bulunmayan öğrenin.
services: billing
documentationcenter: ''
author: manish-shukla01
manager: manshuk
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2019
ms.author: banders
ms.openlocfilehash: 340cba65a1faac247678cd187f106157ba566f3e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60371181"
---
# <a name="software-costs-not-included-with-azure-reserved-vm-instances"></a>Azure ayrılmış VM örnekleri ile bulunmayan yazılım maliyetleri

Bir Azure hibrit avantajı, ayrılmış sanal makine örneklerine sahip değilseniz, aşağıdaki bölümde listelenen yazılım ölçümleri için ücretlendirilir.

## <a name="windows-software-meters-not-included-in-reservation-cost"></a>Windows yazılım ölçümleri rezervasyon maliyeti dahil değildir

| MeterId | Kullanım dosyasındaki MeterName | Sanal makine tarafından kullanılan |
| ------- | ------------------------| --- |
| e7e152ac-f29c-4cce-ad6e-026192c01ef2 | Ayırma-Windows Svr seri Aktarım (1 çekirdek) | B serisi |
| cac255a2-9f0f-4c62-8bd6-f0fa449c5f76 | Ayırma-Windows Svr seri Aktarım (2 Çekirdek) | B serisi |
| 09756b58-3fb5-4390-976d-9ddd14f9ed18 | Ayırma-Windows Svr seri Aktarım (4 çekirdek) | B serisi |
| e828cb37-5920-4dc7-b30f-664e4dbcb6c7 | Ayırma-Windows Svr seri Aktarım (8 çekirdek) | B serisi |
| f65a06cf-c9c3-47a2-8104-f17a8542215a | Ayırma-Windows Svr (1 çekirdek) | B serisi tüm |
| b99d40ae-41fe-4d1d-842b-56d72f3d15ee | Ayırma-Windows Svr (2 Çekirdek) | B serisi tüm |
| 1cb88381-0905-4843-9ba2-7914066aabe5 | Ayırma-Windows Svr (4 çekirdek) | B serisi tüm |
| 07d9e10d-3e3e-4672-ac30-87f58ec4b00a | Ayırma-Windows Svr (6 çekirdek) | B serisi tüm |
| 603f58d1-1e96-460b-a933-ce3775ac7e2e | Ayırma-Windows Svr (8 çekirdek) | B serisi tüm |
| 36aaadda-da86-484a-b465-c8b5ab292d71 | Ayırma-Windows Svr (12 çekirdek) | B serisi tüm |
| 02968a6b-1654-4495-ada6-13f378ba7172 | Ayırma-Windows Svr (16 çekirdek) | B serisi tüm |
| 175434d8-75f9-474b-9906-5d151b6bed84 | Ayırma-Windows Svr (20 çekirdek) | B serisi tüm |
| 77eb6dd0-88f5-4a16-ab39-05d1742efb25 | Ayırma-Windows Svr (24 çekirdek) | B serisi tüm |
| 0d5bdf46-b719-4b1f-a780-b9bdfffd0591 | Ayırma-Windows Svr (32 çekirdek) | B serisi tüm |
| f1214b5c-cc16-445f-be6c-a3bb75f8395a | Ayırma-Windows Svr (40 çekirdek) | B serisi tüm |
| 637b7c77-65ad-4486-9cc7-dc7b3e9a8731 | Ayırma-Windows Svr (64 çekirdek) | B serisi tüm |
| da612742-e7cc-4ca3-9334-0fb7234059cd | Ayırma-Windows Svr (72 çekirdek) | B serisi tüm |
| a485cb8c-069b-4cf3-9a8e-ddd84b323da2 | Ayırma-Windows Svr (128 çekirdek) | B serisi tüm |
| 904c5c71-1eb7-43a6-961c-d305a9681624 | Ayırma-Windows Svr (256 çekirdek) | B serisi tüm |
| 6fdab81b-4284-4df9-8939-c237cc7462fe | Ayırma-Windows Svr (96 çekirdek) | B serisi tüm |

## <a name="cloud-services-software-meters-not-included-in-reservation-cost"></a>Bulut Hizmetleri yazılım ölçümleri rezervasyon maliyeti dahil değildir

| MeterId | Kullanım dosyasındaki MeterName |
| ------- | ------------------------|
|ac9d47ff-ff68-4afc-a145-0c321cf8d0d5|Cloud Services 1 vCPU lisans|
|e0434559-19ee-4132-9c46-05ad4044f3f7|Cloud Services 2 vCPU lisans|
|6ecc834e-39b3-48b3-8d10-cc5626bacb66|Cloud Services 4 vCPU lisans|
|13103090-ca72-4825-ab12-7f16c4931d95|Cloud Services 8 vCPU lisans|
|ecd2bb6e-45a5-49aa-a58b-3947ba21c364|Bulut Hizmetleri 16 vCPU lisans|
|de2c7f1d-06dc-4b16-bc8b-c2ec5f4c8aee|Bulut Hizmetleri 20 vCPU lisans|
|ca1af837-4B35-47f5-8d14-b1988149c4ca|Bulut Hizmetleri 32 vCPU lisans|
|dc72ee45-2ab7-4698-b435-e2cf10d1f9f6|Cloud Services 64 vCPU lisans|
|7a803026-244c-4659-834c-11e6b2d6b76f|Bulut Hizmetleri 80 vCPU lisans|

## <a name="rates-for-azure-meters"></a>Azure ölçümleri ücretleri

Bu ölçümlerin her birinin maliyeti Azure RateCard API aracılığıyla elde edebilirsiniz. Bir azure ölçüm için ücretler alma hakkında daha fazla bilgi için bkz [almak için kullanılan bir Azure aboneliğinde kaynakları fiyat ve meta veri bilgileri](/previous-versions/azure/reference/mt219004(v=azure.100)).

## <a name="next-steps"></a>Sonraki adımlar
Azure için ayırma hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure için ayırmaları nelerdir?](billing-save-compute-costs-reservations.md)
- [Azure Ayrılmış VM Örnekleri ile Sanal Makinelere ön ödeme yapma](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure için ayırmaları yönetme](billing-manage-reserved-vm-instance.md)
- [Ayırma indirimi nasıl uygulanacağını anlama](billing-understand-vm-reservation-charges.md)
- [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](billing-understand-reserved-instance-usage.md)
- [Kurumsal kayıt için ayırma kullanımını anlama](billing-understand-reserved-instance-usage-ea.md)

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).
