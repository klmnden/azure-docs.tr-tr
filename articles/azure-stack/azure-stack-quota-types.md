---
title: Kota türleri Azure yığınında | Microsoft Docs
description: Kullanılabilir hizmet ve kaynakları Azure yığınındaki farklı kota türlerini gözden geçirin.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/20/2018
ms.author: brenduns
ms.reviewer: xiaofmao
ms.openlocfilehash: b68a963dae4b3621bfd9ecdcbc20146d7b20c457
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="quota-types-in-azure-stack"></a>Azure yığınında kota türleri

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

[Kotalar](azure-stack-plan-offer-quota-overview.md#plans) , bir kullanıcı abonelik sağlamak veya tüketmek kaynakları sınırlarını tanımlayın. Örneğin, kota en fazla beş VM'ler oluşturmak bir kullanıcı izin verebilir. Her kaynak kendi tip kotalar olabilir.

## <a name="compute-quota-types"></a>Kota türleri işlem
| **Tür** | **Varsayılan değer** | **Açıklama** |
| --- | --- | --- |
| Sanal makineler sayısı üst sınırı | 20 | Bu konumda bir abonelik oluşturan sanal makineleri maksimum sayısı. |
| Sanal makine çekirdek sayısı üst sınırı | 50 | En yüksek abonelik bu konumda oluşturabilirsiniz çekirdek sayısı (örneğin, bir A3 VM dört çekirdek yoktur). |
| Kullanılabilirlik kümesi sayısı maks. | 10 | Bu konumda oluşturulan kullanılabilirlik kümesi maksimum sayısı. |
| Sanal makine ölçek maksimum sayısını ayarlar | 20 | Bu konumda oluşturulan sanal makine ölçek kümeleri maksimum sayısı. |



## <a name="storage-quota-types"></a>Depolama kotası türleri
| **Öğesi** | **Varsayılan değer** | **Açıklama** |
| --- | --- | --- |
| Maksimum Kapasite (GB) |500 |Bu konumda bulunan abonelik tarafından kullanılan toplam depolama kapasitesi. |
| Depolama hesaplarının toplam sayısına |20 |Bu konumda bir abonelik oluşturduğunuz depolama hesapları maksimum sayısı. |

> [!NOTE]  
> İki yeni bir depolama kotası uygulanmadan önce saate kadar sürebilir. 
> 
> 

## <a name="network-quota-types"></a>Ağ kota türleri
| **Öğesi** | **Varsayılan değer** | **Açıklama** |
| --- | --- | --- |
| Max genel IP'ler |50 |Bu konumda bir abonelik oluşturduğunuz genel IP'ler maksimum sayısı. |
| En büyük sanal ağlar |50 |Bu konumda bir abonelik oluşturduğunuz sanal ağlar maksimum sayısı. |
| En fazla sanal ağ geçitleri |1 |Bu konumda bir abonelik oluşturduğunuz sanal ağ geçitleri (VPN ağ geçitleri) maksimum sayısı. |
| En fazla ağ bağlantıları |2 |(Noktadan noktaya veya siteden siteye) bir aboneliğiniz bu konumdaki tüm sanal ağ geçitleri arasında oluşturabilirsiniz ağ bağlantılarının maksimum sayısı. |
| En fazla yük dengeleyici |50 |Bu konumda bir abonelik oluşturduğunuz yük Dengeleyiciler maksimum sayısı. |
| En fazla NIC |100 |Bu konumda bir abonelik oluşturduğunuz ağ arabirimleri sayısı. |
| En fazla ağ güvenlik grupları |50 |Bu konumda bir abonelik oluşturduğunuz ağ güvenlik grupları maksimum sayısı. |

## <a name="view-an-existing-quota"></a>Var olan bir kota görüntüleyin
1. Tıklatın **daha fazla hizmet** > **kaynak sağlayıcıları**.
2. Görüntülemek istediğiniz kota hizmetiyle seçin.
3. Tıklatın **kotaları**, görüntülemek istediğiniz kota seçin.

## <a name="next-steps"></a>Sonraki adımlar
[Daha fazla hakkında planları, teklifleri ve kotalar öğrenin.](azure-stack-plan-offer-quota-overview.md)

[Kotalar bir plan oluştururken oluşturun.](azure-stack-create-plan.md)
