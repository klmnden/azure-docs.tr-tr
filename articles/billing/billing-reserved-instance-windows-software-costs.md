---
title: Azure ayırma örnekleri Windows yazılım maliyetleri - Azure faturalama | Microsoft Docs
description: Hangi Windows yazılım ölçümler Azure ayrılmış VM örnek maliyetleri de dahil edilmeyen öğrenin.
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
ms.date: 05/09/2018
ms.author: manshuk
ms.openlocfilehash: b526ca578a72d7d35fb4198affeb02db4d308b20
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="windows-software-costs-not-included-with-azure-reserved-instances"></a>Windows yazılım maliyetleri ile Azure ayrılmış örnekler dahil değil

Azure karma kullanımı avantajı, ayrılmış örnek sanal makinelere sahip değilseniz, aşağıdaki bölümde listelenen Windows yazılım ölçümler için ücretlendirilirsiniz.

## <a name="windows-software-meters-not-included-in-reserved-instance-cost"></a>Windows yazılım ölçümler ayrılmış örnek maliyeti dahil değildir

| Ölçüm kimliği | Kullanım dosyasındaki MeterName | VM tarafından kullanılan |
| ------- | ------------------------| --- |
| e7e152ac-f29c-4cce-ad6e-026192c01ef2 | Ayırma Windows Svr veri bloğu (1 çekirdek) | B serisi |
| cac255a2-9f0f-4c62-8bd6-f0fa449c5f76 | Ayırma Windows Svr veri bloğu, (2 Çekirdek) | B serisi |
| 09756b58-3fb5-4390-976d-9ddd14f9ed18 | Ayırma Windows Svr veri bloğu (4 çekirdek) | B serisi |
| e828cb37-5920-4dc7-b30f-664e4dbcb6c7 | Ayırma Windows Svr veri bloğu (8 çekirdek) | B serisi |
| f65a06cf-c9c3-47a2-8104-f17a8542215a | Ayırma Windows Svr (1 çekirdek) | Tüm B serisi dışında |
| b99d40ae-41fe-4d1d-842b-56d72f3d15ee | Ayırma Windows Svr (2 Çekirdek) | Tüm B serisi dışında |
| 1cb88381-0905-4843-9ba2-7914066aabe5 | Ayırma Windows Svr (4 çekirdek) | Tüm B serisi dışında |
| 07d9e10d-3e3e-4672-ac30-87f58ec4b00a | Ayırma Windows Svr (6 çekirdek) | Tüm B serisi dışında |
| 603f58d1-1e96-460b-a933-ce3775ac7e2e | Ayırma Windows Svr (8 çekirdek) | Tüm B serisi dışında |
| 36aaadda-da86-484a-b465-c8b5ab292d71 | Ayırma Windows Svr (12 çekirdek) | Tüm B serisi dışında |
| 02968a6b-1654-4495-ada6-13f378ba7172 | Ayırma Windows Svr (16 çekirdek) | Tüm B serisi dışında |
| 175434d8-75f9-474b-9906-5d151b6bed84 | Ayırma Windows Svr (20 çekirdek) | Tüm B serisi dışında |
| 77eb6dd0-88f5-4a16-ab39-05d1742efb25 | Ayırma Windows Svr (24 çekirdek) | Tüm B serisi dışında |
| 0d5bdf46-b719-4b1f-a780-b9bdfffd0591 | Ayırma Windows Svr (32 çekirdek) | Tüm B serisi dışında |
| f1214b5c-cc16-445f-be6c-a3bb75f8395a | Ayırma Windows Svr (40 çekirdek) | Tüm B serisi dışında |
| 637b7c77-65ad-4486-9cc7-dc7b3e9a8731 | Ayırma Windows Svr (64 çekirdek) | Tüm B serisi dışında |
| da612742-e7cc-4CA3-9334-0fb7234059cd | Ayırma Windows Svr (72 çekirdek) | Tüm B serisi dışında |
| a485cb8c-069b-4cf3-9a8e-ddd84b323da2 | Ayırma Windows Svr (128 çekirdek) | Tüm B serisi dışında |
| 904c5c71-1eb7-43a6-961c-d305a9681624 | Ayırma Windows Svr (256 çekirdek) | Tüm B serisi dışında |
| 6fdab81b-4284-4df9-8939-c237cc7462fe | Ayırma Windows Svr (96 çekirdek) | Tüm B serisi dışında |

Bu ölçümler her maliyeti Azure RateCard API aracılığıyla elde edebilirsiniz. Bir azure ölçer oranları alma hakkında daha fazla bilgi için bkz: [bir Azure aboneliği kullanılan kaynaklar için fiyat ve meta veri bilgi alma](https://msdn.microsoft.com/library/azure/mt219004).

## <a name="next-steps"></a>Sonraki adımlar
Azure ayrılmış örnekler hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure ayrılmış örnekler sahip sanal makinelerde paradan tasarruf](billing-save-compute-costs-reservations.md)
- [Ayrılmış örnekler ile sanal makineler için ön ödeme](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Ayrılmış örnekler yönetme](billing-manage-reserved-vm-instance.md)
- [Ayrılmış örnek indirim nasıl uygulandığını anlama](billing-understand-vm-reservation-charges.md)
- [Kullandıkça Öde aboneliğiniz için ayrılmış örnek kullanımını anlamak](billing-understand-reserved-instance-usage.md)
- [İşletme kaydı için ayrılmış örnek kullanım anlama](billing-understand-reserved-instance-usage-ea.md)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala daha fazla, sorularınız varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.



