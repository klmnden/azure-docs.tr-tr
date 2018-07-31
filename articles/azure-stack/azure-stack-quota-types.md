---
title: Azure stack'teki kota türleri | Microsoft Docs
description: Kullanılabilir hizmetleri ve Azure Stack'te kaynakların farklı kota türlerini gözden geçirin.
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
ms.date: 07/30/2018
ms.author: brenduns
ms.reviewer: xiaofmao
ms.openlocfilehash: 2e884164347239838d08fbbc1616ed54ffc4ff24
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39358744"
---
# <a name="quota-types-in-azure-stack"></a>Azure stack'teki kota türleri

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

[Kotalar](azure-stack-plan-offer-quota-overview.md#plans) kaynaklara kullanıcı aboneliği sağlama veya tüketen sınırlarını tanımlayın. Örneğin, bir kota en fazla beş VM oluşturmak bir kullanıcı izin verebilir. Her kaynak kendi tip kotalar olabilir.

## <a name="compute-quota-types"></a>Kota türleri işlem 
| **Tür** | **Varsayılan değer** | **Açıklama** |
| --- | --- | --- |
| En fazla sanal makine sayısı | 20 | Bu konumda bir abonelik oluşturan sanal makineler en fazla sayısı. |
| En fazla sanal makine çekirdeklerinin sayısı | 50 | Bu konumda abonelik oluşturup oluşturamayacağını çekirdek sayısı (örneğin, bir A3 VM dört çekirdek vardır). |
| Maks. kullanılabilirlik kümesi sayısı | 10 | Bu konumda oluşturulabilir kullanılabilirlik kümelerinin maksimum sayısı. |
| Sanal makine ölçek maksimum sayısını ayarlar | 20 | Bu konumda oluşturulan sanal makine ölçek kümeleri en fazla sayısı. |

## <a name="storage-quota-types"></a>Depolama kota türleri 
| **Öğesi** | **Varsayılan değer** | **Açıklama** |
| --- | --- | --- |
| Maksimum Kapasite (GB) |500 |Bu konumda bulunan bir abonelik tarafından tüketilen toplam depolama kapasitesi. |
| Toplam depolama hesabı sayısı |20 |Bu konumda bir abonelik oluşturduğunuz depolama hesabı sayısı. |

> [!NOTE]  
> Uygulamanın, iki depolama kotası uygulanmadan önce saate kadar sürebilir.


## <a name="network-quota-types"></a>Ağ kota türleri
| **Öğesi** | **Varsayılan değer** | **Açıklama** |
| --- | --- | --- |
| En büyük ortak IP'ler |50 |Bu konumda abonelik oluşturup oluşturamayacağını genel IP'ler maksimum sayısı. |
| En fazla sanal ağlar |50 |Bu konumda bir abonelik oluşturan sanal ağlar maksimum sayısı. |
| En fazla sanal ağ geçitleri |1 |Bu konumda bir abonelik oluşturan, sanal ağ geçitleri (VPN ağ geçitleri) sayısı. |
| En fazla ağ bağlantıları |2 |Bu konumdaki tüm sanal ağ geçitleri arasında abonelik oluşturup oluşturamayacağını olan ağ bağlantıları (Noktadan noktaya veya siteden siteye) sayısı. |
| En fazla yük Dengeleyiciler |50 |Bu konumda abonelik oluşturup oluşturamayacağını yük Dengeleyiciler maksimum sayısı. |
| En fazla NIC |100 |Bu konumda bir abonelik oluşturan ağ arabirimleri sayısı. |
| En fazla ağ güvenlik grupları |50 |Bu konumda bir abonelik oluşturan ağ güvenlik grupları maksimum sayısı. |

## <a name="view-an-existing-quota"></a>Var olan bir kota görüntüleyin
1. Yönetim Portalı'nın varsayılan Panoda bulmak **kaynak sağlayıcıları** Döşe.
2. Gibi görüntülemek istediğiniz kota hizmetiyle seçin **işlem** veya **depolama**.
3. Seçin **kotalar**ve ardından görüntülemek istediğiniz kota seçin.


## <a name="edit-a-quota"></a>Kotayı Düzenle  
Bir kota yerine yapılandırmasını düzenlemek seçebileceğiniz [eklenti planı'nı kullanarak](create-add-on-plan.md). Kota düzenlediğinizde, yeni yapılandırmayı otomatik olarak genel olarak, kota kullanan tüm planlar ve bu planları kullanın ve var olan tüm abonelikler için geçerlidir. Kotayı düzenleme, bir eklenti planı abone olmak için bir kullanıcının seçtiği değiştirilmiş bir kota sağlamak için kullandığınızda farklıdır. 

### <a name="to-edit-a-quota"></a>Kota düzenlemek için  
1. Yönetim Portalı'nın varsayılan Panoda bulmak **kaynak sağlayıcıları** Döşe.
2. Gibi değiştirmek istediğiniz kota hizmetiyle seçin **işlem**, **ağ**, veya **depolama**.
3. Ardından, **kotalar**ve ardından değiştirmek istediğiniz kota seçin.
4. Üzerinde **kotalar belirleyebilirsiniz** bölmesinde değerlerini düzenleyin ve ardından **Kaydet**. 

Kota için yeni değerler genel olarak değiştirilen kota kullanan tüm planlar ve bu planları kullanın ve var olan tüm abonelikler için geçerlidir. 



## <a name="next-steps"></a>Sonraki adımlar

- [Daha fazla ilgili planlar, teklifler ve kotalar öğrenin.](azure-stack-plan-offer-quota-overview.md)
- [Kotalar bir plan oluştururken oluşturun.](azure-stack-create-plan.md)
