---
title: Azure ve şirket içi ortamlar için ExpressRoute bağlantıları oluşturmak için Azure sanal WAN'ı kullanma | Microsoft Docs
description: Bu öğreticide, Azure sanal WAN Azure ve şirket içi ortamlar için ExpressRoute bağlantıları oluşturmak için kullanmayı öğrenin.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 06/10/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect my corporate on-premises network(s) to my VNets using Virtual WAN and ExpressRoute.
ms.openlocfilehash: edf5e04b7cf9b5c79666c54fbeca49858cf21079
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67077535"
---
# <a name="tutorial-create-an-expressroute-association-using-azure-virtual-wan-preview"></a>Öğretici: Azure sanal WAN (Önizleme) kullanarak ExpressRoute ilişkilendirme oluşturma

Bu öğreticide Sanal WAN kullanarak Azure'daki kaynaklarınıza bir ExpressRoute bağlantı hattı ve ilişkisi kullanarak bağlanmayı öğreneceksiniz. Sanal WAN hakkında daha fazla bilgi için bkz. [Sanal WAN'a Genel Bakış](virtual-wan-about.md)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * vWAN oluşturma
> * Hub oluşturma
> * Bağlantı hattı bulun ve hub ile ilişkilendirin
> * Bağlantı hattını hub’lar ile ilişkilendirin
> * Bir sanal ağı bir hub'a bağlama
> * Sanal WAN'ınızı görüntüleme
> * Kaynak durumunu görüntüleme
> * Bir bağlantıyı izleme

> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [Before you begin](../../includes/virtual-wan-tutorial-vwan-before-include.md)]

## <a name="register"></a>Bu özelliği kaydedin

Sanal WAN yapılandırabilmeniz için önce aboneliğinizi Önizleme'ye kaydetmeniz gerekir. Aksi halde portalda Sanal WAN ile çalışamazsınız. Kaydetmek için bir e-posta Gönder **azurevirtualwan\@microsoft.com** abonelik kimliğinizi Aboneliğiniz kaydedildiğinde siz de bir e-posta alırsınız.

**Önizlemede Dikkat Edilmesi Gerekenler:**

  * ExpressRoute bağlantı hattı, bir ülke/destekleyen bölge içinde etkinleştirilmelidir [ExpressRoute Global erişim](https://docs.microsoft.com/azure/expressroute/expressroute-faqs#where-is-expressroute-global-reach-supported).
  * ExpressRoute bağlantı hattına sanal WAN hub'ına bağlanmak için bir Premium devresi olması gerekir. 

## <a name="vnet"></a>1. Sanal ağ oluşturma

[!INCLUDE [Create a virtual network](../../includes/virtual-wan-tutorial-vnet-include.md)]

## <a name="openvwan"></a>2. Sanal WAN oluşturma

Bir tarayıcıdan [Azure portala (önizleme)](https://aka.ms/azurevirtualwanpreviewfeatures) gidin ve Azure hesabınızla oturum açın.

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-vwan-include.md)]

### <a name="getting-started-page"></a>Başlarken sayfası

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-gettingstarted-include.md)]

## <a name="hub"></a>3. Hub oluşturma

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-hub-include.md)]

## <a name="hub"></a>4. Bağlantı hattı bulun ve hub ile ilişkilendirin

1. VWAN seçin ve altındaki **sanal WAN mimarisi**seçin **ExpressRoute devreleri**.
1. ExpressRoute bağlantı hattı, vWAN ile aynı abonelikte tıklayarak **seçin ExpressRoute bağlantı hattı** Abonelikleriniz öğesinden. 
1. Aşağı açılır kullanarak hub'a ilişkilendirmek istiyorsanız, ExpressRoute'ı seçin.
1. ExpressRoute bağlantı hattı, aynı abonelikte değilse veya size sağlanan [yetkilendirme anahtar ve Eş Kimliği](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md)seçin **yetkilendirme anahtarını kullanırken bir devreyi Bul**
1. Şu ayrıntıları girin:
1. **Yetkilendirme anahtarı** - Yukarıda açıklandığı gibi bağlantı hattı sahibinden tarafından oluşturulur
1. **Eş bağlantı hattı URI’si** - Bağlantı hattının sahibi tarafından oluşturulan ve hattın benzersiz tanımlayıcısı olan bağlantı hattı URI’si
1. **Yönlendirme ağırlığı** - [yönlendirme ağırlığı](../expressroute/expressroute-optimize-routing.md) eşleme farklı konumlardaki birden çok bağlantı hattına aynı hub'ına bağlandığında, belirli yollarını tercih etmesini sağlar
1. Tıklayın **bulma devre** ve bağlantı hattı seçin bulunamadı.
1. Aşağı açılan listeden 1 veya daha fazla hub'ı seçin ve tıklayın **Kaydet**.

## <a name="vnet"></a>5. Sanal ağınızı bir hub'a bağlama

Bu adımda hub'ınızla bir sanal ağ arasında eşleme bağlantısı oluşturacaksınız. Bu adımları bağlanmak istediğiniz tüm sanal ağlar için tekrarlayın.

1. Sanal WAN'ınızın sayfasında **Sanal ağ bağlantısı**'na tıklayın.
2. Sanal ağ bağlantısı sayfasında **+Bağlantı ekle**'ye tıklayın.
3. **Bağlantı ekle** sayfasında aşağıdaki alanları doldurun:

    * **Bağlantı adı**: Bağlantınıza bir ad verin.
    * **Hub'lar**: Bu bağlantıyla ilişkilendirmek istediğiniz hub'ı seçin.
    * **Abonelik**: Aboneliği doğrulayın.
    * **Sanal ağ**: Bu hub'a bağlamak istediğiniz sanal ağı seçin. Sanal ağda önceden var olan bir sanal ağ geçidi bulunamaz.


## <a name="viewwan"></a>6. Sanal WAN'ınızı görüntüleme

1. Sanal WAN'a gidin.
2. Genel bakış sayfasında haritadaki her bir nokta bir hub'ı temsil eder. Hub sistem durumu özetini görüntülemek için noktalardan birinin üzerine gidin.
3. Hub'lar ve bağlantılar bölümünde herhangi bir hub'ın durumunu, sitesini, bölgesini, VPN bağlantısı durumunu ve gelen/giden baytları görüntüleyebilirsiniz.

## <a name="viewhealth"></a>7. Kaynak durumunu görüntüleme

1. WAN'ınıza gidin.
2. WAN sayfanızın **Destek ve sorun giderme** bölümünde **Sistem durumu**'na tıklayın ve kaynağınızı görüntüleyin.

## <a name="connectmon"></a>8. Bir bağlantıyı izleme

Bir Azure sanal makinesi ile uzak site arasındaki iletişimi izlemek için bir bağlantı oluşturun. Bağlantı izleyici oluşturma hakkında bilgi almak için bkz. [Ağ iletişimini izleme](~/articles/network-watcher/connection-monitor.md). Kaynak alan Azure'daki sanal makinenin IP adresi, hedef IP ise Sitenin IP adresidir.

## <a name="cleanup"></a>9. Kaynakları temizleme

Bu kaynaklara artık ihtiyacınız olmadığında, kullanabileceğiniz [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için. "myResourceGroup" yerine kaynak grubunuzun adını yazın ve aşağıdaki PowerShell komutunu çalıştırın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * vWAN oluşturma
> * Hub oluşturma
> * Bağlantı hattı bulun ve hub ile ilişkilendirin
> * Bağlantı hattını hub’lar ile ilişkilendirin
> * Bir sanal ağı bir hub'a bağlama
> * Sanal WAN'ınızı görüntüleme
> * Kaynak durumunu görüntüleme
> * Bir bağlantıyı izleme

Sanal WAN hakkında daha fazla bilgi için [Sanal WAN'a Genel Bakış](virtual-wan-about.md) sayfasına bakın.
