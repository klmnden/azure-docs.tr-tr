---
title: Azure ve şirket içi ortamlara ExpressRoute bağlantıları oluşturmak için Azure Sanal WAN kullanma | Microsoft Docs
description: Bu öğreticide Azure ve şirket içi ortamlar için ExpressRoute bağlantıları oluşturma amacıyla Azure Sanal WAN’ı nasıl kullanacağınızı öğrenirsiniz.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 10/5/2018
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect my corporoate on-premises network(s) to my VNets using Virtual WAN and ExpressRoute.
ms.openlocfilehash: c02020ba8d49b123cf8914214d52ac40896a3c20
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51248189"
---
# <a name="tutorial-create-an-expressroute-association-using-azure-virtual-wan-preview"></a>Öğretici: Azure Sanal WAN kullanarak ExpressRoute ilişkisi oluşturma (önizleme)

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

[!INCLUDE [Before you begin](../../includes/virtual-wan-tutorial-vwan-before-include.md)]

## <a name="register"></a>Bu özelliği kaydedin

Sanal WAN yapılandırabilmeniz için önce aboneliğinizi Önizleme'ye kaydetmeniz gerekir. Aksi halde portalda Sanal WAN ile çalışamazsınız. Kaydolmak için **azurevirtualwan@microsoft.com** adresine abonelik kimliğinizin bulunduğu bir e-posta gönderin. Aboneliğiniz kaydedildiğinde siz de bir e-posta alırsınız.

**Önizlemede Dikkat Edilmesi Gerekenler:**

* Bölge Kullanılabilirliği: Orta Batı ABD
* ExpressRoute bağlantı hattı [ExpressRoute Global Reach](https://docs.microsoft.com/azure/expressroute/expressroute-faqs#where-is-expressroute-global-reach-supported) özelliğini destekleyen bir ülkede etkinleştirilmelidir

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

1. vWAN’ınızı seçin ve **Sanal WAN Mimarisi** altında **ExpressRoute Bağlantı Hatları**’nı seçin
2. ExpressRoute bağlantı hattı, vWAN’ınız ile aynı abonelikte bulunuyorsa aboneliklerinizden **ExpressRoute bağlantı hattı seç** seçeneğine tıklayın 
3. Aşağı açılan menüyü kullanarak hub ile ilişkilendirmek istediğiniz ExpressRoute hizmetini seçin.
4. ExpressRoute bağlantı hattı aynı abonelikte değilse veya size bir [yetkilendirme anahtarı ve eş kimliği](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md) sağlandıysa **yetkilendirme anahtarı kullanan bir bağlantı hattı bul** seçeneğini belirleyin
5. Şu ayrıntıları girin:
* **Yetkilendirme anahtarı** - Yukarıda açıklandığı gibi bağlantı hattı sahibinden tarafından oluşturulur
* **Eş bağlantı hattı URI’si** - Bağlantı hattının sahibi tarafından oluşturulan ve hattın benzersiz tanımlayıcısı olan bağlantı hattı URI’si
* **Yönlendirme ağırlığı** - [Yönlendirme ağırlığı](../expressroute/expressroute-optimize-routing.md), farklı eşleme konumlarındaki birden fazla bağlantı hattı aynı hub’a bağlı olduğunda belirli yolları tercih etmenize olanak sağlar
6. **Bağlantı hattı bul**’a tıklayın ve bulmanız halinde bağlantı hattını seçin
7. Açılır listeden 1 veya daha fazla hub seçerek **Kaydet**’e tıklayın

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

Bu kaynaklar artık gerekli değilse, [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanarak kaynak grubunu ve içerdiği tüm kaynakları kaldırabilirsiniz. "myResourceGroup" yerine kaynak grubunuzun adını yazın ve aşağıdaki PowerShell komutunu çalıştırın:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
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
