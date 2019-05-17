---
title: Oluşturma, değiştirme veya bir Azure ağ güvenlik grubunu sil
titlesuffix: Azure Virtual Network
description: Oluşturma, değiştirme veya bir ağ güvenlik grubunu silmek öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/05/2018
ms.author: kumud
ms.openlocfilehash: f1353165954021cd949d6e46357d10514ee26b3c
ms.sourcegitcommit: 179918af242d52664d3274370c6fdaec6c783eb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/13/2019
ms.locfileid: "65560939"
---
# <a name="create-change-or-delete-a-network-security-group"></a>Oluşturma, değiştirme veya bir ağ güvenlik grubunu sil

Güvenlik kuralları olan ağ güvenlik gruplarında sanal ağ alt ağları ve ağ arabirimleri ve giden akış ağ trafiği türünü filtrelemek olanak sağlar. Ağ güvenlik grupları ile ilgili bilgi sahibi değilseniz bkz [ağ güvenlik grubu genel bakış](security-overview.md) bunlar hakkında daha fazla bilgi edinin ve tamamlamak için [ağ trafiğini filtreleme](tutorial-filter-network-traffic.md) ağ biraz deneyim kazanmak için öğretici güvenlik grupları.

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu makalenin bir bölümündeki adımları tamamlamadan önce aşağıdaki görevleri tamamlayın:

- Azure hesabınız yoksa, kaydolmaya bir [ücretsiz deneme hesabınızı](https://azure.microsoft.com/free).
- Portalı kullanarak, açık https://portal.azure.comve Azure hesabınızda oturum.
- Bu makaledeki görevleri tamamlamak için PowerShell komutlarını kullanarak, ya da komutları çalıştırmak [Azure Cloud Shell](https://shell.azure.com/powershell), veya PowerShell bilgisayarınızdan çalıştırarak. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bu öğretici Azure PowerShell modülü sürüm 1.0.0 gerektirir veya üzeri. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.
- Bu makaledeki görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, ya da komutları çalıştırmak [Azure Cloud Shell](https://shell.azure.com/bash), veya bilgisayarınızdan CLI çalıştırarak. Bu öğretici, Azure CLI 2.0.28 gerektirir veya üzeri. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli). Azure CLI'yi yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `az login` Azure ile bir bağlantı oluşturmak için.

Hesap oturum veya bağlantı ile azure'a atanmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) içinde listelenen uygun eylemleri atanan [izinleri ](#permissions).

## <a name="work-with-network-security-groups"></a>Ağ güvenlik grupları ile çalışma

Oluşturabileceğiniz, [tümünü görüntüle](#view-all-network-security-groups), [ayrıntılarını görüntülemek](#view-details-of-a-network-security-group), [değiştirme](#change-a-network-security-group), ve [Sil](#delete-a-network-security-group) bir ağ güvenlik grubu. Ayrıca [ilişkisini kaldırın veya ilişkilendirme](#associate-or-dissociate-a-network-security-group-to-or-from-a-subnet-or-network-interface) bir ağ arabirimi veya alt ağ bir ağ güvenlik grubu.

### <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

Kaç ağ güvenlik grupları Azure konumu ve abonelik oluşturmak için bir sınır yoktur. Ayrıntılar için [Azure limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesini inceleyin.

1. Portalın sol üst köşedeki seçin **+ kaynak Oluştur**.
2. Seçin **ağ**, ardından **ağ güvenlik grubu**.
3. Girin bir **adı** için ağ güvenlik grubu seçin, **abonelik**, yeni bir **kaynak grubu**, veya mevcut bir kaynak grubunu seçin, bir **Konumu**ve ardından **Oluştur**.

**Komutları**

- Azure CLI: [az ağ nsg oluşturma](/cli/azure/network/nsg#az-network-nsg-create)
- PowerShell: [New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup)

### <a name="view-all-network-security-groups"></a>Tüm ağ güvenlik grupları görüntüleme

Portalın üst kısmındaki arama kutusuna girin *ağ güvenlik grupları*. Zaman **ağ güvenlik grupları** arama sonuçlarında görünmesini, onu seçin. Aboneliğinizde var olan ağ güvenlik gruplarının listelenir.

**Komutları**

- Azure CLI: [az ağ nsg listesi](/cli/azure/network/nsg#az-network-nsg-list)
- PowerShell: [Get-AzNetworkSecurityGroup](/powershell/module/az.network/get-aznetworksecuritygroup)

### <a name="view-details-of-a-network-security-group"></a>Bir ağ güvenlik grubu ayrıntılarını görüntüle

1. Portalın üst kısmındaki arama kutusuna girin *ağ güvenlik grupları*. Zaman **ağ güvenlik grupları** arama sonuçlarında görünmesini, onu seçin.
2. Ağ güvenlik grubu ayrıntılarını görüntülemek istediğiniz listeyi seçin. Altında **ayarları** görüntüleyebileceğiniz **gelen güvenlik kuralları** ve **giden güvenlik kuralları**, **ağ arabirimleri** ve  **Alt ağlar** ilişkili ağ güvenlik grubu. Ayrıca etkinleştirir veya devre dışı bırakmak **tanılama günlükleri** ve Görünüm **geçerli güvenlik kuralları**. Daha fazla bilgi için bkz. [tanılama günlükleri](virtual-network-nsg-manage-log.md) ve [geçerli güvenlik kuralları görüntüleyebiliriz](diagnose-network-traffic-filter-problem.md).
3. Listelenen yaygın Azure ayarları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
    *   [Etkinlik Günlüğü](../azure-monitor/platform/activity-logs-overview.md)
    *   [Erişim denetimi (IAM)](../role-based-access-control/overview.md)
    *   [Etiketler](../azure-resource-manager/resource-group-using-tags.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
    *   [Kilitler](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
    *   [Otomasyon betiği](../azure-resource-manager/manage-resource-groups-portal.md#export-resource-groups-to-templates)

**Komutları**

- Azure CLI: [az ağ nsg show](/cli/azure/network/nsg#az-network-nsg-show)
- PowerShell: [Get-AzNetworkSecurityGroup](/powershell/module/az.network/get-aznetworksecuritygroup)

### <a name="change-a-network-security-group"></a>Bir ağ güvenlik grubu değiştirme

1. Portalın üst kısmındaki arama kutusuna girin *ağ güvenlik grupları* arama kutusuna. Zaman **ağ güvenlik grupları** arama sonuçlarında görünmesini, onu seçin.
2. Değiştirmek istediğiniz ağ güvenlik grubu seçin. En yaygın değişiklikler [ekleme](#create-a-security-rule) veya [kaldırma](#delete-a-security-rule) güvenlik kuralları ve [Associating veya bir ağ güvenlik grubu için veya bir alt ağ veya ağ arabirimi ile ilişkisi kaldırılıyor](#associate-or-dissociate-a-network-security-group-to-or-from-a-subnet-or-network-interface).

**Komutları**

- Azure CLI: [az ağ nsg update](/cli/azure/network/nsg#az-network-nsg-update)
- PowerShell: [Set-AzNetworkSecurityGroup](/powershell/module/az.network/set-aznetworksecuritygroup)

### <a name="associate-or-dissociate-a-network-security-group-to-or-from-a-subnet-or-network-interface"></a>İlişkilendirme veya için veya bir alt ağ veya ağ arabirimi bir ağ güvenlik grubu ilişkilendirmesini Kaldır

İlişkilendirmek için bir ağ güvenlik grubu veya bir ağ arabiriminden bir ağ güvenlik grubu ilişkilendirmesini için bkz: [ilişkilendirmek için bir ağ güvenlik grubu veya bir ağ arabiriminden bir ağ güvenlik grubu ilişkilendirmesini](virtual-network-network-interface.md#associate-or-dissociate-a-network-security-group). Bir ağ güvenlik grubu için veya bir alt ağdan ağ güvenlik grubu ilişkilendirmesini için bkz: [alt ağ ayarları değiştir](virtual-network-manage-subnet.md#change-subnet-settings).

### <a name="delete-a-network-security-group"></a>Bir ağ güvenlik grubunu sil

Herhangi bir alt ağ veya ağ arabirimleri için ağ güvenlik grubu ilişkiliyse, bu komut dosyası silinemiyor. Tüm alt ağlar ve ağ arabirimlerinin ağ güvenlik grubu silmeye çalışmadan önce ilişkisini kaldırın.

1. Portalın üst kısmındaki arama kutusuna girin *ağ güvenlik grupları* arama kutusuna. Zaman **ağ güvenlik grupları** arama sonuçlarında görünmesini, onu seçin.
2. Listeden silmek istediğiniz ağ güvenlik grubu seçin.
3. Seçin **Sil**ve ardından **Evet**.

**Komutları**

- Azure CLI: [az ağ nsg delete](/cli/azure/network/nsg#az-network-nsg-delete)
- PowerShell: [Remove-AzNetworkSecurityGroup](/powershell/module/az.network/remove-aznetworksecuritygroup)

## <a name="work-with-security-rules"></a>Güvenlik kuralları ile çalışma

Bir ağ güvenlik grubu, sıfır veya daha fazla güvenlik kuralları içerir. Oluşturabileceğiniz, [tümünü görüntüle](#view-all-security-rules), [ayrıntılarını görüntülemek](#view-details-of-a-security-rule), [değiştirme](#change-a-security-rule), ve [Sil](#delete-a-security-rule) bir güvenlik kuralı.

### <a name="create-a-security-rule"></a>Bir güvenlik kuralı oluşturun

Ağ güvenlik grubu başına kaç kuralları Azure konumu ve abonelik oluşturabileceğinize dair bir sınır yoktur. Ayrıntılar için [Azure limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesini inceleyin.

1. Portalın üst kısmındaki arama kutusuna girin *ağ güvenlik grupları* arama kutusuna. Zaman **ağ güvenlik grupları** arama sonuçlarında görünmesini, onu seçin.
2. Ağ güvenlik grubu için bir güvenlik kuralı eklemek istediğiniz listeyi seçin.
3. Seçin **gelen güvenlik kuralları** altında **ayarları**. Var olan çeşitli kurallar listelenmiştir. Bazı değil eklemiş olabileceğiniz kuralları. Bir ağ güvenlik grubu oluşturulduğunda, bazı varsayılan güvenlik kuralları içinde oluşturulur. Daha fazla bilgi için bkz. [varsayılan güvenlik kuralları](security-overview.md#default-security-rules).  Varsayılan güvenlik kuralları silinemez, ancak daha yüksek bir önceliğe sahip kurallar ile geçersiz kılabilirsiniz.
4. <a name = "security-rule-settings"></a>Seçin **+ Ekle**.  Seçin veya aşağıdaki ayarları için değerleri ekleyin ve ardından **Tamam**:
    
    |Ayar  |Değer  |Ayrıntılar  |
    |---------|---------|---------|
    |Kaynak     | Seçin **herhangi**, **uygulama güvenlik grubu**, **IP adresleri**, veya **hizmet etiketi** gelen güvenlik kuralları için. Giden güvenlik kuralı oluşturuyorsanız seçenekleri için listelenen seçenekleri aynıdır **hedef**.       | Seçerseniz **uygulama güvenlik grubu**birini seçin veya daha fazla var olan uygulama güvenlik gruplarını ağ arabirimi ile aynı bölgede mevcut. Bilgi edinmek için nasıl [uygulama güvenlik grubu oluşturma](#create-an-application-security-group). Seçerseniz **uygulama güvenlik grubu** hem **kaynak** ve **hedef**, her iki uygulama güvenlik grupları içinde ağ arabirimleri aynı olmalıdır. sanal ağ. Seçerseniz **IP adresleri**, ardından belirtin **kaynak IP adresleri/CIDR aralıkları**. Tek değer veya birden çok değer virgülle ayrılmış listesini belirtebilirsiniz. 10.0.0.0/16, 192.188.1.1 birden çok değer örneğidir. Belirtebileceğiniz değerler sayısı sınırı yoktur. Bkz: [Azure limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) Ayrıntılar için. Seçerseniz **hizmet etiketi**, ardından bir hizmet etiketi seçin. Hizmet etiketi, bir IP adresi kategorisini belirtmek için önceden tanımlanmış bir tanımlayıcıdır. Kullanılabilir hizmet etiketleri ve her bir etiketin ne temsil hakkında daha fazla bilgi için bkz: [hizmet etiketleri](security-overview.md#service-tags). Bir Azure sanal makinesi için belirttiğiniz IP adresi atanmış ise, özel IP olmayan sanal makineye atanan genel IP adresi belirttiğinizden emin olun. Güvenlik kuralları için gelen güvenlik kuralları için özel bir IP adresi genel IP adresini Azure çevirir sonra ve Azure genel IP adresine giden kuralları için özel bir IP adresi çevirir önce işlenir. Azure'da genel ve özel IP adresleri hakkında daha fazla bilgi için bkz: [IP adresi türleri](virtual-network-ip-addresses-overview-arm.md).        |
    |Kaynak bağlantı noktası aralıkları     | 1024-65535 80 gibi tek bir bağlantı noktası, bağlantı noktaları, 1024-65535 gibi bir dizi veya tek bağlantı noktası ve/veya bağlantı noktası aralığı, 80 gibi virgülle ayrılmış listesini belirtin. Herhangi bir bağlantı noktası üzerinde trafiğe izin veren bir yıldız işareti girin. | Aralıkları ve bağlantı noktalarını hangi bağlantı noktalarının trafiğine izin verilir veya trafik kuralı tarafından reddedildi belirtin. Belirtebileceğiniz bağlantı noktalarının sayısı sınırları vardır. Bkz: [Azure limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) Ayrıntılar için.  |
    |Hedef     | Seçin **herhangi**, **uygulama güvenlik grubu**, **IP adresleri**, veya **sanal ağ** giden güvenlik kuralları için. Bir gelen güvenlik kuralı oluşturuyorsanız seçenekleri için listelenen seçenekleri aynıdır **kaynak**.        | Seçerseniz **uygulama güvenlik grubu** ardından ağ arabirimi ile aynı bölgede bulunan bir veya daha fazla var olan uygulama güvenlik grupları seçmeniz gerekir. Bilgi edinmek için nasıl [uygulama güvenlik grubu oluşturma](#create-an-application-security-group). Seçerseniz **uygulama güvenlik grubu**, ardından ağ arabirimi ile aynı bölgede var olan bir var olan uygulama güvenlik grubu seçin. Seçerseniz **IP adresleri**, ardından belirtin **hedef IP adresleri/CIDR aralıkları**. Benzer şekilde **kaynak** ve **kaynak IP adresleri/CIDR aralıkları**, tek bir veya birden çok adresi veya aralığı belirtebilirsiniz ve bununla ilgili sınırlamalar sayısını belirtebilirsiniz. Seçme **sanal ağ**, hizmet etiketini olacağı, trafiği anlamına gelir sanal ağın adres alanı içindeki tüm IP adreslerine izin verilir. Bir Azure sanal makinesi için belirttiğiniz IP adresi atanmış ise, özel IP olmayan sanal makineye atanan genel IP adresi belirttiğinizden emin olun. Güvenlik kuralları için gelen güvenlik kuralları için özel bir IP adresi genel IP adresini Azure çevirir sonra ve Azure genel IP adresine giden kuralları için özel bir IP adresi çevirir önce işlenir. Azure'da genel ve özel IP adresleri hakkında daha fazla bilgi için bkz: [IP adresi türleri](virtual-network-ip-addresses-overview-arm.md).        |
    |Hedef bağlantı noktası aralıkları     | Tek değer veya değerlerden oluşan virgülle ayrılmış bir listeyi belirtin. | Benzer şekilde **kaynak bağlantı noktası aralıkları**, tek bir veya birden çok bağlantı noktaları ve aralıkları belirtebilirsiniz ve bununla ilgili sınırlamalar sayısını belirtebilirsiniz. |
    |Protocol     | Seçin **herhangi**, **TCP**, veya **UDP**.        |         |
    |Eylem     | Seçin **izin** veya **Reddet**.        |         |
    |Öncelik     | Tüm güvenlik kuralları olan ağ güvenlik grubu içinde benzersiz olan 100-4096 arasında bir değer girin. |Kurallar öncelik sırasına göre işlenir. Alt sayısı, öncelik o kadar yüksektir. 100, 200, 300 gibi kuralları oluştururken öncelik sayılar arasında boşluk bırakmanızı öneririz. Boş bırakabiliyor gelecekte daha yapmanız gerekebilir kuralları eklemek daha kolay bir şekilde veya mevcut kurallardan daha düşük kolaylaştırır.         |
    |Ad     | Ağ güvenlik grubu kuralı için benzersiz bir ad.        |  Ad en fazla 80 karakter uzunluğunda olabilir. Başlamalı bir harf veya sayı ile bir harf, sayı veya alt çizgi ile bitmelidir ve yalnızca harf, sayı, alt çizgi, nokta veya kısa çizgi içerebilir.       |
    |Açıklama     | İsteğe bağlı bir açıklama.        |         |

**Komutları**

- Azure CLI: [az ağ nsg kuralı oluşturma](/cli/azure/network/nsg/rule#az-network-nsg-rule-create)
- PowerShell: [New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig)

### <a name="view-all-security-rules"></a>Tüm güvenlik kurallarını görüntüleyin

Bir ağ güvenlik grubu, sıfır veya birden çok kural içerir. Kuralları görüntülerken listelenen bilgiler hakkında daha fazla bilgi için bkz. [ağ güvenlik grubu genel bakış](security-overview.md).

1. Portalın üst kısmındaki arama kutusuna girin *ağ güvenlik grupları*. Zaman **ağ güvenlik grupları** arama sonuçlarında görünmesini, onu seçin.
2. Ağ güvenlik grubu kurallarını görüntülemek istediğiniz listeyi seçin.
3. Seçin **gelen güvenlik kuralları** veya **giden güvenlik kuralları** altında **ayarları**.

Oluşturduğunuz tüm kuralları ve ağ güvenlik grubu listesi içeren [varsayılan güvenlik kuralları](security-overview.md#default-security-rules).

**Komutları**

- Azure CLI: [az ağ nsg kural listesi](/cli/azure/network/nsg/rule#az-network-nsg-rule-list)
- PowerShell: [Get-AzNetworkSecurityRuleConfig](/powershell/module/az.network/get-aznetworksecurityruleconfig)

### <a name="view-details-of-a-security-rule"></a>Bir güvenlik kuralının ayrıntılarını görüntüleme

1. Portalın üst kısmındaki arama kutusuna girin *ağ güvenlik grupları*. Zaman **ağ güvenlik grupları** arama sonuçlarında görünmesini, onu seçin.
2. Bir güvenlik kuralı için ayrıntılarını görüntülemek istediğiniz ağ güvenlik grubu seçin.
3. Seçin **gelen güvenlik kuralları** veya **giden güvenlik kuralları** altında **ayarları**.
4. Ayrıntılarını görüntülemek istediğiniz kuralı seçin. Tüm ayarları ayrıntılı bir açıklaması için bkz. [güvenlik kural ayarları](#security-rule-settings).

**Komutları**

- Azure CLI: [az ağ nsg kuralı Göster](/cli/azure/network/nsg/rule#az-network-nsg-rule-show)
- PowerShell: [Get-AzNetworkSecurityRuleConfig](/powershell/module/az.network/get-aznetworksecurityruleconfig)

### <a name="change-a-security-rule"></a>Güvenlik kuralı değiştirme

1. Bölümündeki adımları tamamlamanız [bir güvenlik kuralının ayrıntıları görüntüle](#view-details-of-a-security-rule).
2. Ayarları istediğiniz gibi değiştirin ve ardından **Kaydet**. Tüm ayarları ayrıntılı bir açıklaması için bkz. [güvenlik kural ayarları](#security-rule-settings).

**Komutları**

- Azure CLI: [az ağ nsg kuralı güncelleştirme](/cli/azure/network/nsg/rule#az-network-nsg-rule-update)
- PowerShell: [Set-AzNetworkSecurityRuleConfig](/powershell/module/az.network/set-aznetworksecurityruleconfig)

### <a name="delete-a-security-rule"></a>Güvenlik kuralını Sil

1. Bölümündeki adımları tamamlamanız [bir güvenlik kuralının ayrıntıları görüntüle](#view-details-of-a-security-rule).
2. Seçin **Sil**ve ardından **Evet**.

**Komutları**

- Azure CLI: [az ağ nsg kuralı Sil](/cli/azure/network/nsg/rule#az-network-nsg-rule-delete)
- PowerShell: [Remove-AzNetworkSecurityRuleConfig](/powershell/module/az.network/remove-aznetworksecurityruleconfig)

## <a name="work-with-application-security-groups"></a>Uygulama güvenlik grupları ile çalışma

Uygulama güvenlik grubu, sıfır veya daha fazla ağ arabirimlerini içerir. Daha fazla bilgi için bkz. [uygulama güvenlik grupları](security-overview.md#application-security-groups). Bir uygulama güvenlik grubundaki tüm ağ arabirimleri aynı sanal ağda bulunması gerekir. Uygulama güvenlik grubu için bir ağ arabirimi ekleme konusunda bilgi edinmek için [bir ağ arabirimi bir uygulama güvenlik grubuna eklemek](virtual-network-network-interface.md#add-to-or-remove-from-application-security-groups).

### <a name="create-an-application-security-group"></a>Bir uygulama güvenlik grubu oluşturun

1. Azure portalının sol üst köşesinde bulunan **+ Kaynak oluştur** seçeneğini belirleyin.
2. **Market içinde ara** kutusuna *Uygulama güvenlik grubu* girin. Arama sonuçlarında **Uygulama güvenlik grubu** gösterildiğinde bunu seçin, **Her şey**'in altında yeniden **Uygulama güvenlik grubu**'nu seçin ve sonra da **Oluştur**'u seçin.
3. Aşağıdaki bilgileri girin veya seçin ve sonra **Oluştur**’u seçin:

    | Ayar        | Değer                                                   |
    | ---            | ---                                                     |
    | Ad           | Ad, kaynak grubu içinde benzersiz olmalıdır.        |
    | Abonelik   | Aboneliğinizi seçin.                               |
    | Kaynak grubu | Mevcut bir kaynak grubunu seçin veya yeni bir tane oluşturun. |
    | Location       | Konum seçin                                       |

**Komutları**

- Azure CLI: [az ağ asg oluşturma](/cli/azure/network/asg#az-network-asg-create)
- PowerShell: [Yeni AzApplicationSecurityGroup](/powershell/module/az.network/new-azapplicationsecuritygroup)

### <a name="view-all-application-security-groups"></a>Tüm uygulama güvenlik grupları görüntüleme

1. Seçin **tüm hizmetleri** üst üzerinde köşe Azure portalının sol.
2. Girin *uygulama güvenlik grupları* içinde **tüm hizmetleri filtre** kutusuna ve ardından **uygulama güvenlik grupları** arama sonuçlarında görüntülendiğinde.

**Komutları**

- Azure CLI: [az ağ asg listesi](/cli/azure/network/asg#az-network-asg-list)
- PowerShell: [Get-AzApplicationSecurityGroup](/powershell/module/az.network/get-azapplicationsecuritygroup)

### <a name="view-details-of-a-specific-application-security-group"></a>Belirli bir uygulama güvenlik grubu ayrıntılarını görüntüle

1. Seçin **tüm hizmetleri** üst üzerinde köşe Azure portalının sol.
2. Girin *uygulama güvenlik grupları* içinde **tüm hizmetleri filtre** kutusuna ve ardından **uygulama güvenlik grupları** arama sonuçlarında görüntülendiğinde.
3. Ayrıntılarını görüntülemek istediğiniz uygulama güvenlik grubu seçin.

**Komutları**

- Azure CLI: [az ağ asg Göster](/cli/azure/network/asg#az-network-asg-show)
- PowerShell: [Get-AzApplicationSecurityGroup](/powershell/module/az.network/get-azapplicationsecuritygroup)

### <a name="change-an-application-security-group"></a>Uygulama güvenlik grubu değiştirme

1. Seçin **tüm hizmetleri** üst üzerinde köşe Azure portalının sol.
2. Girin *uygulama güvenlik grupları* içinde **tüm hizmetleri filtre** kutusuna ve ardından **uygulama güvenlik grupları** arama sonuçlarında görüntülendiğinde.
3. Ayarlarını değiştirmek istediğiniz uygulama güvenlik grubu seçin. Ekleyebilir veya etiketleri, kaldırmak veya atayın veya uygulama güvenlik grubu izinlerini kaldırın.

- Azure CLI: [az ağ asg güncelleştirme](/cli/azure/network/asg#az-network-asg-update)
- PowerShell: PowerShell cmdlet yok.

### <a name="delete-an-application-security-group"></a>Bir uygulama güvenlik grubunu sil

İçindeki tüm ağ arabirimleri varsa bir uygulama güvenlik grubu silinemiyor. Ağ arabirimi ayarları değiştirme veya ağ arabirimlerini silerek, tüm ağ arabirimleri uygulama güvenlik grubundan kaldırın. Ayrıntılar için bkz [Ekle veya Kaldır'ı uygulama güvenlik grupları ağ arabiriminden](virtual-network-network-interface.md#add-to-or-remove-from-application-security-groups) veya [ağ arabirimini Sil](virtual-network-network-interface.md#delete-a-network-interface).

1. Seçin **tüm hizmetleri** üst üzerinde köşe Azure portalının sol.
2. Girin *uygulama güvenlik grupları* içinde **tüm hizmetleri filtre** kutusuna ve ardından **uygulama güvenlik grupları** arama sonuçlarında görüntülendiğinde.
3. Silmek istediğiniz uygulama güvenlik grubu seçin.
4. Seçin **Sil**ve ardından **Evet** uygulama güvenlik grubu silinemedi.

**Komutları**

- Azure CLI: [az ağ asg Sil](/cli/azure/network/asg#az-network-asg-delete)
- PowerShell: [Remove-AzApplicationSecurityGroup](/powershell/module/az.network/remove-azapplicationsecuritygroup)

## <a name="permissions"></a>İzinler

Ağ güvenlik grupları, güvenlik kuralları ve uygulama güvenlik grupları görevleri gerçekleştirmek için hesabınızı atanmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) atanan Aşağıdaki tablolarda listelenenler uygun izinleri:

### <a name="network-security-group"></a>Ağ güvenlik grubu

| Eylem                                                        |   Ad                                                                |
|-------------------------------------------------------------- |   -------------------------------------------                         |
| Microsoft.Network/networkSecurityGroups/read                  |   Ağ güvenlik grubunu Al                                          |
| Microsoft.Network/networkSecurityGroups/write                 |   Ağ güvenlik grubu veya güncelleştirilemiyor                             |
| Microsoft.Network/networkSecurityGroups/delete                |   Ağ güvenlik grubunu sil                                       |
| Microsoft.Network/networkSecurityGroups/join/action           |   Bir ağ güvenlik grubu bir alt ağ veya ağ arabirimine ilişkilendirin 


### <a name="network-security-group-rule"></a>Ağ güvenlik grubu kuralı

| Eylem                                                        |   Ad                                                                |
|-------------------------------------------------------------- |   -------------------------------------------                         |
| Microsoft.Network/networkSecurityGroups/rules/read            |   Kuralı alma                                                            |
| Microsoft.Network/networkSecurityGroups/rules/write           |   Kuralı oluşturun veya güncelleştirin                                               |
| Microsoft.Network/networkSecurityGroups/rules/delete          |   Kuralı sil                                                         |

### <a name="application-security-group"></a>Uygulama güvenlik grubu

| Eylem                                                                     | Ad                                                     |
| --------------------------------------------------------------             | -------------------------------------------              |
| Microsoft.Network/applicationSecurityGroups/joinIpConfiguration/action     | Bir IP yapılandırması, bir uygulama güvenlik grubuna katılma|
| Microsoft.Network/applicationSecurityGroups/joinNetworkSecurityRule/action | Bir güvenlik kuralı bir uygulama güvenlik grubuna katılma    |
| Microsoft.Network/applicationSecurityGroups/read                           | Bir uygulama güvenlik grubunu Al                        |
| Microsoft.Network/applicationSecurityGroups/write                          | Bir uygulama güvenlik grubu veya güncelleştirilemiyor           |
| Microsoft.Network/applicationSecurityGroups/delete                         | Bir uygulama güvenlik grubunu sil                     |

## <a name="next-steps"></a>Sonraki adımlar

- Bir ağ veya güvenlik grubu kullanarak uygulama oluşturma [PowerShell](powershell-samples.md) veya [Azure CLI](cli-samples.md) örnek komut dosyaları veya Azure kullanarak [Resource Manager şablonları](template-samples.md)
- Oluşturma ve uygulama [Azure İlkesi](policy-samples.md) sanal ağlar için
