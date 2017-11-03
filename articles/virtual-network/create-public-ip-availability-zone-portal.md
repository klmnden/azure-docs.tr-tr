---
title: "Azure Portal ile zoned bir ortak IP adresi oluşturma | Microsoft Docs"
description: "Azure portal ile bir kullanılabilirlik bölgesindeki bir ortak IP adresi oluşturun."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 09/25/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 2fcbed2f83d66a0b4336cd1c464bb02eff3ef229
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-public-ip-address-in-an-availability-zone-with-the-azure-portal"></a>Azure portal ile bir kullanılabilirlik bölgesindeki bir ortak IP adresi oluştur

Bir ortak IP adresi Azure kullanılabilirlik bölgesinde (Önizleme) dağıtabilirsiniz. Bir Azure bölgesi fiziksel olarak ayrı bir bölgede bir kullanılabilirlik bölgedir. Şunları nasıl yapacağınızı öğrenin:

> * Bir kullanılabilirlik bölgesinde bir ortak IP adresi oluşturma
> * Kullanılabilirlik bölgede oluşturulan ilgili kaynakları belirleyin

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!NOTE]
> Kullanılabilirlik bölgeleri önizlemede ve geliştirme için hazır olduğunu ve test senaryoları. Destek select Azure kaynaklarını ve bölgeler ve VM boyutu aileleri için kullanılabilir. Başlamak hakkında daha fazla bilgi ve hangi Azure kaynaklarını, bölgeler ve kullanılabilirlik bölgeleri deneyebilirsiniz VM boyutu aileleri için bkz: [kullanılabilirlik bölgeleri genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview). Destek için [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) üzerinden bize ulaşabilir veya [bir Azure destek bileti açabilirsiniz](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Https://portal.azure.com Azure portalında oturum açın. 

## <a name="create-a-zonal-public-ip-address"></a>Zonal bir ortak IP adresi oluşturun

1. Azure portalının sol üst köşesinde bulunan **Yeni** düğmesine tıklayın.
2. Seçin **ağ**ve ardından **genel IP adresi**.
3. Girin veya aşağıdaki ayarları için değerleri, aboneliğinizi seçin, diğer ayarlar için Varsayılanları kabul seçip'ı tıklatın **oluşturma**:

    |Ayar|Değer|
    |---|---|
    |SKU| **Temel**: statik veya dinamik ayırma yöntemiyle atanmış. Ağ arabirimleri, VPN ağ geçitleri, uygulama ağ geçitleri ve Internet'e yönelik yük dengeleyici gibi bir ortak IP adresi atanmış herhangi bir Azure kaynak atanabilir. Belirli bir bölgenin genel IP adresi atayabilirsiniz **kullanılabilirlik bölge** ayarı. Bölge olarak yedekli değildir. Kullanılabilirlik alanları hakkında daha fazla bilgi için bkz. [Kullanılabilirlik alanlarına genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview). **Standart**: yalnızca statik ayırma yöntemiyle atanmış. Ağ arabirimlerine veya standart Internet'e yönelik Yük Dengeleyici atanabilir. Bir standart Internet'e yönelik Yük Dengeleyici genel IP adresi atarsanız, standart seçmeniz gerekir. Azure yük dengeleyici SKU'ları hakkında daha fazla bilgi edinmek için bkz. [Azure yük dengeleyici standart SKU'su](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview). Bölge-varsayılan olarak, gereksizdir. Belirli bir alanda oluşturulabilir ve belirli bir kullanılabilirlik alanında oluşturulma garantisi vardır. Standart SKU önizleme sürümündedir. Standart SKU genel bir IP adresi oluşturmadan önce ilk önizleme için kaydetmeniz gerekir. Önizlemeye kaydolmak için bkz. [Standart SKU önizlemesine kaydolma](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview#preview-sign-up). Standart SKU yalnızca desteklenen bir konumda oluşturulabilir.  Desteklenen konumların (bölgelerin) listesi için [Bölge kullanılabilirliği](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview#region-availability) sayfasını, ek bölgesel destek için de [Azure Sanal Ağı güncelleştirmeleri](https://azure.microsoft.com/updates/?product=virtual-network) sayfasını inceleyin.|   
    |Ad|Adı kaynak grubun içinde benzersiz olmalıdır.|
    |Kaynak grubu|Yeni Oluştur'u tıklatın ve myResourceGroup girin|
    |Konum|Batı Avrupa|
    |Kullanılabilirlik bölge|Seçtiyseniz **standart** seçebileceğiniz SKU, *bölge olarak yedekli* IP adresi dilimlerinde dayanıklı olmasını istiyorsanız. Seçerseniz **temel** SKU, IP adresi değil esnek dilimlerinde. Seçtiğiniz SKU bağımsız olarak seçerseniz belirli bir bölgeye adresi atayabilirsiniz. |

    Ayarları Portalı'nda aşağıdaki resimde gösterildiği gibi görünür:

    ![Zonal ortak IP adresi oluştur](./media/create-public-ip-availability-zone-portal/public-ip-address.png) 

> [!NOTE]
> Standart bir SKU genel IP adresini bir sanal makinenin ağ arabirimine atadığınızda amaçlanan trafiğe bir [ağ güvenlik grubuyla](security-overview.md#network-security-groups) açıkça izin vermeniz gerekir. Bir ağ güvenlik grubu oluşturup ilişkilendirene ve istenen trafiğe açıkça izin verene kadar kaynakla erişim kurma girişimleri başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinmek [kullanılabilirlik bölgeleri](https://docs.microsoft.com/azure/availability-zones/az-overview)
- Daha fazla bilgi edinmek [genel IP adresleri](virtual-network-public-ip-address.md?toc=%2fazure%2fvirtual-network%2ftoc.json)