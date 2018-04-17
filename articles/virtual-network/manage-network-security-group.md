---
title: Oluşturma, değiştirme veya bir Azure ağ güvenlik grubu silme | Microsoft Docs
description: Oluşturma, değiştirme veya bir ağ güvenlik grubu silme öğrenin.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/05/2018
ms.author: jdial
ms.openlocfilehash: ba7c0400e59d8c747553479eb5474978a629dfdf
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="create-change-or-delete-a-network-security-group"></a>Oluşturma, değiştirme veya bir ağ güvenlik grubu silme

Ağ güvenlik grupları güvenlik kurallarında sanal ağ alt ağları ve ağ arabirimleri ve dışındaki ağ trafiği türü filtrelemek etkinleştirin. Ağ güvenlik gruplarıyla bilmiyorsanız bkz [ağ güvenlik grubu genel bakış](security-overview.md) bunları hakkında daha fazla bilgi ve tamamlamak için [filtre ağ trafiğini](tutorial-filter-network-traffic.md) bazı ağ deneyim kazanmak için Öğreticisi güvenlik grupları.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalenin herhangi bir bölümdeki adımları gerçekleştirmeden önce aşağıdaki görevleri tamamlayın:

- Zaten bir Azure hesabınız yoksa, kaydolun bir [ücretsiz deneme sürümü hesabı](https://azure.microsoft.com/free).
- Portalı kullanarak, açık https://portal.azure.comve Azure hesabınızda oturum.
- Bu makalede görevleri tamamlamak için PowerShell komutlarını kullanarak, ya da komutları çalıştırmak [Azure bulut Kabuk](https://shell.azure.com/powershell), veya bilgisayarınızdan PowerShell çalıştırarak. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bu öğreticide Azure PowerShell modülü sürümü 5.4.1 gerektirir veya sonraki bir sürümü. Yüklü sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.
- Bu makalede görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, ya da komutları çalıştırmak [Azure bulut Kabuk](https://shell.azure.com/bash), veya bilgisayarınızdan CLI çalıştırarak. Bu öğretici Azure CLI Sürüm 2.0.28 gerektirir veya sonraki bir sürümü. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). Azure CLI yerel olarak çalıştırıyorsanız, ayrıca çalıştırmanız gereken `az login` Azure ile bir bağlantı oluşturmak için.

## <a name="work-with-network-security-groups"></a>Ağ güvenlik gruplarıyla çalışma

Oluşturabileceğiniz, [tüm görüntüle](#view-all-network-security-groups), [ayrıntılarını görüntülemek](#view-details-of-a-network-security-group), [değiştirme](#change-a-network-security-group), ve [silme](#delete-a-network-security-group) bir ağ güvenlik grubu. Ayrıca [ilişkilendir ya da ilişkilendirmesini](#associate-or-dissociate-a-network-security-group-to-or-from-a-resource) bir ağ arabirimi veya alt ağ güvenlik grubu.

### <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

Kaç tane ağ güvenlik grubu Azure konumu ve abonelik oluşturmak için bir sınır yoktur. Ayrıntılar için [Azure limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesini inceleyin.

1. Portalın sol üst köşede seçin **+ kaynak oluşturma**.
2. Seçin **ağ**seçeneğini belirleyip **ağ güvenlik grubu**.
3. Girin bir **adı** için ağ güvenlik grubu seçin, **abonelik**, yeni bir **kaynak grubu**, veya var olan bir kaynak grubunu seçin, bir **Konumu**ve ardından **oluşturma**. 

**Komutları**

- Azure CLI: [az ağ nsg oluşturma](/cli/azure/network/nsg#az-network-nsg-create)
- PowerShell: [AzureRmNetworkSecurityGroup yeni](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup)

### <a name="view-all-network-security-groups"></a>Tüm ağ güvenlik grupları görüntüleyin

Portal üstündeki arama kutusuna girin *ağ güvenlik grubu*. Zaman **ağ güvenlik grubu** arama sonuçlarında görünecek, onu seçin. Aboneliğinizde var olan ağ güvenlik gruplarının listelenir.

**Komutları**

- Azure CLI: [az ağ nsg listesi](/cli/azure/network/nsg#az-network-nsg-list)
- PowerShell: [Get-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/get-azurermnetworksecuritygroup)

### <a name="view-details-of-a-network-security-group"></a>Bir ağ güvenlik grubu ayrıntılarını görüntüleme

1. Portal üstündeki arama kutusuna girin *ağ güvenlik grubu*. Zaman **ağ güvenlik grubu** arama sonuçlarında görünecek, onu seçin.
2. Ağ güvenlik grubu ayrıntılarını görüntülemek istediğiniz listeyi seçin. Altında **ayarları** görüntüleyebileceğiniz **gelen güvenlik kuralları** ve **giden güvenlik kurallarında**, **ağ arabirimleri** ve  **Alt ağlar** için ağ güvenlik grubu ilişkilidir. Ayrıca etkinleştirir veya devre dışı bırakmak **tanılama günlükleri** ve Görünüm **etkin güvenlik kuralları**. Daha fazla bilgi için bkz: [tanılama günlükleri](virtual-network-nsg-manage-log.md) ve [etkin güvenlik kuralları](virtual-network-nsg-troubleshoot-portal.md).
3. Listelenen ortak Azure ayarları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
    *   [Etkinlik Günlüğü](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs)
    *   [Erişim denetimi (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control)
    *   [Etiketler](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags)
    *   [Kilitler](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
    *   [Otomasyon komut dosyası](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group)

**Komutları**

- Azure CLI: [az ağ nsg Göster](/cli/azure/network/nsg#az-network-nsg-show)
- PowerShell: [Get-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/get-azurermnetworksecuritygroup)

### <a name="change-a-network-security-group"></a>Bir ağ güvenlik grubu değiştirme

1. Portal üstündeki arama kutusuna girin *ağ güvenlik grubu* arama kutusuna. Zaman **ağ güvenlik grubu** arama sonuçlarında görünecek, onu seçin.
2. Değiştirmek istediğiniz ağ güvenlik grubu seçin. En yaygın değişiklikler [ekleme](#create-a-security-rule) veya [kaldırma](#delete-a-security-rule) güvenlik kuralları ve [Associating veya bir ağ güvenlik grubu için veya bir alt ağ veya ağ arabirimi kaldırdıktan](#associate-or-dissociate-a-network-security-group-to-or-from-a-resource).

**Komutları**

- Azure CLI: [az ağ nsg güncelleştirme](/cli/azure/network/nsg#az-network-nsg-update)
- PowerShell: [kümesi AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/set-azurermnetworksecuritygroup)

### <a name="associate-or-dissociate-a-network-security-group-to-or-from-a-subnet-or-network-interface"></a>İlişkilendir ya da bir alt ağ veya ağ arabirimi bilgisayardan veya bir ağ güvenlik grubu ilişkilendirmesini Kaldır

Ağ güvenlik grubuna ilişkilendir ya da bir ağ arabiriminden ağ güvenlik grubu ilişkilendirmesini için bkz: [bir ağ güvenlik grubuna ilişkilendir ya da bir ağ arabiriminden ağ güvenlik grubu ilişkilendirmesini](virtual-network-network-interface.md#associate-or-dissociate-a-network-security-group). Ağ güvenlik grubuna ilişkilendir ya da bir alt ağdaki bir ağ güvenlik grubu ilişkilendirmesini için bkz: [alt ağ ayarlarını değiştir](virtual-network-manage-subnet.md#change-subnet-settings).

### <a name="delete-a-network-security-group"></a>Ağ güvenlik grubunu sil

Herhangi bir alt ağ veya ağ arabirimleri için ağ güvenlik grubu ilişkiliyse silinemiyor. [İlişkilendirmesini](#associate-or-dissociate-a-network-security-group-to-or-from-a-resource) tüm alt ağlar ve ağ arabirimlerinin silmeye çalışmadan önce bir ağ güvenlik grubu.

1. Portal üstündeki arama kutusuna girin *ağ güvenlik grubu* arama kutusuna. Zaman **ağ güvenlik grubu** arama sonuçlarında görünecek, onu seçin.
2. Listeden silmek istediğiniz ağ güvenlik grubu seçin.
3. Seçin **silmek**ve ardından **Evet**.

**Komutları**

- Azure CLI: [az ağ nsg Sil](/cli/azure/network/nsg#az-network-nsg-delete)
- PowerShell: [AzureRmNetworkSecurityGroup Kaldır](/powershell/module/azurerm.network/remove-azurermnetworksecuritygroupp) 

## <a name="work-with-security-rules"></a>Güvenlik kuralları ile çalışma

Ağ güvenlik grubu sıfır veya daha fazla güvenlik kuralları içerir. Oluşturabileceğiniz, [tüm görüntüle](#view-all-security-rules), [ayrıntılarını görüntülemek](#view-details-of-a-security-rule), [değiştirme](#change-a-security-rule), ve [silme](#delete-a-security-rule) bir güvenlik kuralı.

### <a name="create-a-security-rule"></a>Güvenlik kuralı oluştur

Ağ güvenlik grubu başına kaç tane kuralları Azure konumu ve abonelik oluşturabilirsiniz bir sınır yoktur. Ayrıntılar için [Azure limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesini inceleyin.

1. Portal üstündeki arama kutusuna girin *ağ güvenlik grubu* arama kutusuna. Zaman **ağ güvenlik grubu** arama sonuçlarında görünecek, onu seçin.
2. Ağ güvenlik grubu için bir güvenlik kuralı eklemek istediğiniz listeyi seçin.
3. Seçin **gelen güvenlik kuralları** altında **ayarları**. Çeşitli mevcut kurallar listelenmiştir. Bazı kuralları değil eklediğiniz. Bir ağ güvenlik grubu oluşturduğunuzda, birkaç varsayılan güvenlik kuralları içinde oluşturulur. Daha fazla bilgi için bkz: [güvenlik kuralları varsayılan](security-overview.md#default-security-rules).  Varsayılan güvenlik kuralları silinemez, ancak daha yüksek önceliğe sahip kuralları ile geçersiz kılınabilir.
4. <a name = "security-rule-settings"></a>Seçin **+ Ekle**.  Seçin veya aşağıdaki ayarları için değerleri ekleyin ve ardından **Tamam**:
    
    |Ayar  |Değer  |Ayrıntılar  |
    |---------|---------|---------|
    |Kaynak     | Seçin **herhangi**, **IP adreslerini**, veya **hizmet etiketi**.        | Seçerseniz **IP adreslerini**, ardından belirtmelisiniz **kaynak IP adresleri/CIDR aralıkları**. Tek değer veya birden çok değer virgülle ayrılmış bir listesini belirtebilirsiniz. Birden çok değer 10.0.0.0/16, 192.188.1.1 örnektir. Belirleyebileceğiniz değerlerinin sayısı sınırı vardır. Bkz: [Azure sınırlar](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) Ayrıntılar için. Seçerseniz **hizmet etiketi**, ardından bir hizmet etiketi seçmeniz gerekir. Bir hizmet etiketi, bir IP adresi kategorisini belirtmek için önceden tanımlanmış bir tanımlayıcıdır. Kullanılabilir hizmet etiketleri ve ne her etiketi temsil hakkında daha fazla bilgi için bkz: [hizmet etiketleri](security-overview.md#service-tags)        |
    |Kaynak bağlantı noktası aralıkları     | 1024-65535 80 gibi tek bir bağlantı noktası, 1024-65535 gibi bir bağlantı noktaları aralığı veya tek bağlantı noktaları ve/veya bağlantı noktası aralığı 80 gibi virgülle ayrılmış listesini belirtin. Herhangi bir bağlantı noktasında trafiğine izin vermek için bir yıldız işareti girin. | Aralıkları ve bağlantı noktaları hangi bağlantı noktalarının trafiğinin izin verilen veya kural tarafından reddedilen belirtin. Belirleyebileceğiniz bağlantı noktalarının sayısı sınırı vardır. Bkz: [Azure sınırlar](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) Ayrıntılar için.  |
    |Hedef     | Seçin **herhangi**, **IP adreslerini**, veya **sanal ağ**.        | Seçerseniz **IP adreslerini**, ardından belirtmelisiniz **hedef IP adresleri/CIDR aralıkları**. Benzer şekilde **kaynak** ve **kaynak IP adresleri/CIDR aralıkları**, tek bir veya birden çok adresler veya aralıklar belirtebilirsiniz ve sınırlar vardır sayıya, belirtebilirsiniz. Seçme **sanal ağ**, bir hizmet etiketi olduğu, trafiği anlamına gelir sanal ağın adres alanı içinde tüm IP adreslerine izin verilir.        |
    |Hedef bağlantı noktası aralıkları     | Bir tek değer veya değerlerini virgülle ayrılmış listesini belirtin. | Benzer şekilde **kaynak bağlantı noktası aralıkları**, tek bir veya birden çok bağlantı noktaları ve aralıklarını belirtebilirsiniz ve sınırlar vardır sayıya, belirtebilirsiniz. |
    |Protokol     | Seçin **herhangi**, **TCP**, veya **UDP**.        |         |
    |Eylem     | Seçin **izin** veya **Reddet**.        |         |
    |Öncelik     | Ağ güvenlik grubu içindeki tüm güvenlik kuralları için benzersizdir 100-4096 arasında bir değer girin. |Kurallar öncelik sırasına göre işlenir. Alt sayısı, öncelik o kadar yüksektir. 100, 200, 300 gibi kuralları oluştururken öncelik sayılar arasında boşluk bırakmak önerilir. Boşluk bırakarak gelecekte daha yüksek yapmanız gerekebilir kuralları eklemek kolay ya da varolan kurallarından daha düşük kolaylaştırır.         |
    |Ad     | Ağ güvenlik grubu içinde kuralı için benzersiz bir ad.        |  Ad en fazla 80 karakter olabilir. Başlaması gereken bir harf, sayı veya alt çizgi ile bir harf veya sayı ile bitmelidir ve yalnızca harf, rakam, alt çizgi, nokta veya kısa çizgi içerebilir.       |
    |Açıklama     | İsteğe bağlı bir açıklama.        |         |

    Belirtemezsiniz bir [uygulama güvenlik grubu](#work-with-application-security-groups) için **kaynak** veya **hedef** Portalı'nı kullanarak ayarlar. Ancak, Azure CLI veya PowerShell kullanarak yapabilirsiniz. Ayarlarını **giden güvenlik kurallarında** ayrı olarak kapsanmaz şekilde benzerdir.

**Komutları**

- Azure CLI: [az ağ nsg kuralı oluşturma](/cli/azure/network/nsg/rule#az-network-nsg-rule-create)
- PowerShell: [AzureRmNetworkSecurityRuleConfig yeni](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig)

### <a name="view-all-security-rules"></a>Tüm güvenlik kuralları görüntüleyin

Bir ağ güvenlik grubu sıfır veya birden çok kurallarını içerir. Kuralları görüntülerken listelenen bilgileri hakkında daha fazla bilgi için bkz: [ağ güvenlik grubu genel bakış](security-overview.md).

1. Portal üstündeki arama kutusuna girin *ağ güvenlik grubu*. Zaman **ağ güvenlik grubu** arama sonuçlarında görünecek, onu seçin.
2. Ağ güvenlik grubu kurallarını görüntülemek istediğiniz listeyi seçin.
3. Seçin **gelen güvenlik kuralları** veya **giden güvenlik kurallarında** altında **ayarları**.

Oluşturduğunuz kurallar ve ağ güvenlik grubu listesi içeren [güvenlik kuralları varsayılan](security-overview.md#default-security-rules).

**Komutları**

- Azure CLI: [az ağ nsg kuralı listesi](/cli/azure/network/nsg/rule#az-network-nsg-rule-list)
- PowerShell: [Get-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/get-azurermnetworksecurityruleconfig)

### <a name="view-details-of-a-security-rule"></a>Güvenlik kuralı ayrıntılarını görüntüleme

1. Portal üstündeki arama kutusuna girin *ağ güvenlik grubu*. Zaman **ağ güvenlik grubu** arama sonuçlarında görünecek, onu seçin.
2. İçin bir güvenlik kuralı ayrıntılarını görüntülemek istediğiniz ağ güvenlik grubu seçin.
3. Seçin **gelen güvenlik kuralları** veya **giden güvenlik kurallarında** altında **ayarları**.
4. Ayrıntılarını görüntülemek istediğiniz kuralı seçin. Tüm ayarlar ayrıntılı bir açıklaması için bkz: [güvenlik kuralı ayarları](#security-rule-settings).

**Komutları**

- Azure CLI: [az ağ nsg kural Göster](/cli/azure/network/nsg/rule#az-network-nsg-rule-show)
- PowerShell: [Get-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/get-azurermnetworksecurityruleconfig)

### <a name="change-a-security-rule"></a>Güvenlik kuralı değiştirme

1. Bölümündeki adımları tamamlamanız [bir güvenlik kuralı ayrıntılarını görüntülemek](#view-details-of-a-security-rule).
2. Ayarları istediğiniz gibi değiştirin ve ardından **kaydetmek**. Tüm ayarlar ayrıntılı bir açıklaması için bkz: [güvenlik kuralı ayarları](#security-rule-settings).

**Komutları**

- Azure CLI: [az ağ nsg kural güncelleştirmesi](/cli/azure/network/nsg/rule#az-network-nsg-rule-update)
- PowerShell: [kümesi AzureRmSecurityRuleConfig](/powershell/module/azurerm.network/set-azurermnetworksecurityruleconfig)

### <a name="delete-a-security-rule"></a>Bir güvenlik kuralını Sil

1. Bölümündeki adımları tamamlamanız [bir güvenlik kuralı ayrıntılarını görüntülemek](#view-details-of-a-security-rule).
2. Seçin **silmek**ve ardından **Evet**.

**Komutları**

- Azure CLI: [az ağ nsg kural silme](/cli/azure/network/nsg/rule#az-network-nsg-rule-delete)
- PowerShell: [AzureRmSecurityRuleConfig Kaldır](/powershell/module/azurerm.network/remove-azurermnetworksecurityruleconfig)


## <a name="work-with-application-security-groups"></a>Uygulama güvenlik grupları ile çalışma

Uygulama güvenlik grubu sıfır veya daha fazla ağ arabirimleri içerir. Daha fazla bilgi için bkz: [uygulama güvenlik grupları](security-overview.md#application-security-groups). Uygulama güvenlik gruplarıyla Portalı'nda çalışmaz, ancak PowerShell veya Azure CLI kullanabilirsiniz. Bir uygulama güvenlik grubundaki tüm ağ arabirimleri aynı sanal ağda bulunması gerekir. Bir uygulama güvenlik grubuna eklenen ilk ağ arabirimi tüm sonraki ağ arabirimleri olmalıdır, sanal ağ belirler. Bir ağ arabirimi bir uygulama güvenlik grubuna eklemeyi öğrenmek için bkz: [bir ağ arabirimi bir uygulama güvenlik grubuna eklemek](virtual-network-network-interface.md#add-to-or-remove-from-application-security-groups).

### <a name="create-an-application-security-group"></a>Uygulama güvenlik grubu oluşturma

- Azure CLI: [az ağ asg oluşturma](/cli/azure/network/asg#az-network-asg-create)
- PowerShell: [AzureRmApplicationSecurityGroup yeni](/powershell/module/azurerm.network/new-azurermapplicationsecuritygroup)

### <a name="view-all-application-security-groups"></a>Tüm uygulama güvenlik grupları görüntüleyin

- Azure CLI: [az ağ asg listesi](/cli/azure/network/asg#az-network-asg-list)
- PowerShell: [Get-AzureRmApplicationSecurityGroup](/powershell/module/azurerm.network/get-azurermapplicationsecuritygroup)

### <a name="view-details-of-a-specific-application-security-group"></a>Belirli bir uygulama güvenlik grubu ayrıntılarını görüntüleme

- Azure CLI: [az ağ asg Göster](/cli/azure/network/asg#az-network-asg-show)
- PowerShell: [Get-AzureRmApplicationSecurityGroup](/powershell/module/azurerm.network/get-azurermapplicationsecuritygroup)

### <a name="change-an-application-security-group"></a>Uygulama güvenlik grubu değiştirme

Etiketleri ve mevcut uygulama güvenlik grubu için izinleri gibi bazı ayarları değiştirebilirsiniz, ancak kendi ad veya konum değiştiremezsiniz.

- Azure CLI: [az ağ asg güncelleştirme](/cli/azure/network/asg#az-network-asg-update)
- PowerShell: Hiçbir PowerShell cmdlet'ini kullanın.

### <a name="delete-an-application-security-group"></a>Bir uygulama güvenlik grubunu sil

Tüm ağ arabirimlerini sahipse, uygulama güvenlik grubu silinemiyor. Ağ arabirimi ayarlarını değiştirmek veya ağ arabirimleri silme uygulama güvenlik grubundan tüm ağ arabirimleri kaldırmanız gerekir. Ayrıntılar için bkz [Ekle veya Kaldır uygulama güvenlik gruplarının bir ağ arabirimi](virtual-network-network-interface.md#add-to-or-remove-from-application-security-groups) veya [ağ arabirimini silmek](virtual-network-network-interface.md#delete-a-network-interface).

**Komutları**

- Azure CLI: [az ağ asg Sil](/cli/azure/network/asg#az-network-asg-delete)
- PowerShell: [AzureRmApplicationSecurityGroup Kaldır](/powershell/module/azurerm.network/remove-azurermapplicationsecuritygroup)

## <a name="permissions"></a>İzinler

Ağ güvenlik grupları, güvenlik kuralları ve uygulama güvenlik grupları görevleri gerçekleştirmek için hesabınızı atanmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) atanan rolü Aşağıdaki tabloda listelenen uygun izinleri:

|İşlem                                                       |   İşlem adı                               |
|--------------------------------------------------------------  |   -------------------------------------------  |
|Microsoft.Network/ruleTables/read                              |   Ağ güvenlik grubunu Al                              |
|Microsoft.Network/ruleTables/write                             |   Ağ güvenlik grubu güncelle                 |
|Microsoft.Network/ruleTables/delete                            |   Ağ güvenlik grubunu sil                           |
|Microsoft.Network/ruleTables/join/action                       |   Ağ güvenlik grubuna katılma                             |
|Microsoft.Network/ruleTables/rules/read                       |   Kuralını Al                                    |
|Microsoft.Network/ruleTables/rules/write                      |   Kuralı oluştur veya güncelleştir                       |
|Microsoft.Network/ruleTables/rules/delete                     |   Kural silme                                 |
|Microsoft.Network/networkInterfaces/effectiveruleTable/action  |   Ağ arabirimi etkin ağ güvenlik grubunu Al  | 
|Microsoft.Network/networkWatchers/nextHop/action                |   Bir sanal makineden sonraki atlama alır                  |

*Ağ güvenlik grubuna Katıl* işlemi, bir alt ağ için ağ güvenlik grubu ilişkilendirmek için gereklidir.