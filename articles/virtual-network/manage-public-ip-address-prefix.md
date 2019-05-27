---
title: Oluşturma, değiştirme veya bir Azure genel IP adresi öneki Sil
titlesuffix: Azure Virtual Network
description: Oluşturma, değiştirme veya genel bir IP adresi ön eki Sil öğrenin.
services: virtual-network
documentationcenter: na
author: anavinahar
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/13/2019
ms.author: anavin
ms.openlocfilehash: 82ee9d04785fc0f6ac534428bf411ca0fe3204ad
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65601501"
---
# <a name="create-change-or-delete-a-public-ip-address-prefix"></a>Oluşturma, değiştirme veya genel bir IP adresi ön eki Sil

Genel bir IP adresi ön eki ve oluşturmak, değiştirmek ve silmek hakkında bilgi edinin. Genel bir IP adresi ön eki belirttiğiniz genel IP adresleri sayısına göre adreslerinin bitişik bir aralıktır. Adresleri aboneliğinize atanır. Bir genel IP adresi kaynağı oluşturduğunuzda, bir statik genel IP adresi ön ekini atayabilir ve sanal makineler, yük Dengeleyiciler veya internet bağlantısını etkinleştirmek için diğer kaynaklar adresini ilişkilendirmek. Genel IP adresi ön ekleri bilmiyorsanız bkz [genel IP adresi ön eki genel bakış](public-ip-address-prefix.md)

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu makalenin bir bölümündeki adımları tamamlamadan önce aşağıdaki görevleri tamamlayın:

- Azure hesabınız yoksa, kaydolmaya bir [ücretsiz deneme hesabınızı](https://azure.microsoft.com/free).
- Portalı kullanarak, açık https://portal.azure.comve Azure hesabınızda oturum.
- Bu makaledeki görevleri tamamlamak için PowerShell komutlarını kullanarak, ya da komutları çalıştırmak [Azure Cloud Shell](https://shell.azure.com/powershell), veya PowerShell bilgisayarınızdan çalıştırarak. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bu öğretici Azure PowerShell modülü sürüm 1.0.0 gerektirir veya üzeri. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.
- Bu makaledeki görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, ya da komutları çalıştırmak [Azure Cloud Shell](https://shell.azure.com/bash), veya bilgisayarınızdan CLI çalıştırarak. Bu öğretici, Azure CLI Sürüm 2.0.41 gerektirir veya üzeri. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). Azure CLI'yi yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `az login` Azure ile bir bağlantı oluşturmak için.

Oturum açın ya da Azure ile bağlandığınız hesabı atanmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) içinde listelenen uygun eylemleri atanan [izinleri ](#permissions).

Genel IP adresi ön eklerini bir ücreti vardır. Ayrıntılar için bkz [fiyatlandırma](https://azure.microsoft.com/pricing/details/ip-addresses).

## <a name="create-a-public-ip-address-prefix"></a>Genel bir IP adresi ön eki oluşturma

1. Portalın üst, sol köşede seçin **+ kaynak Oluştur**.
2. Girin *genel IP adresi ön eki* içinde *markette Ara* kutusu. Zaman **genel IP adresi ön eki** arama sonuçlarında görünür.
3. Altında **genel IP adresi ön eki**seçin **Oluştur**.
4. Altında aşağıdaki ayarları için değerleri seçin veya girin **Oluştur genel IP adresi ön eki**, ardından **Oluştur**:

   |Ayar|Gerekli mi?|Ayrıntılar|
   |---|---|---|
   |Abonelik|Evet|Aynı bulunmalıdır [abonelik](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) için genel IP adresini ilişkilendirmek istediğiniz kaynağı olarak.|
   |Kaynak grubu|Evet|Aynı veya farklı bir arada var [kaynak grubu](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) için genel IP adresini ilişkilendirmek istediğiniz kaynağı olarak.|
   |Ad|Evet|Adı kaynak grubu içinde benzersiz olmalıdır.|
   |Bölge|Evet|Aynı bulunmalıdır [bölge](https://azure.microsoft.com/regions)genel IP adresleri olarak adresleri aralığından atayacaksınız. Ön eki şu anda Batı Orta ABD, Batı ABD, Batı ABD 2, Orta ABD, Kuzey Avrupa, Batı Avrupa ve Güneydoğu Asya Önizleme aşamasındadır.|
   |Ön ek boyutu|Evet| Gereksinim duyduğunuz ön ek boyutu. A/28 ya da 16 IP adresleri, varsayılan değerdir.

**Komutları**

|Tool|Komut|
|---|---|
|CLI|[az ağ public-ip ön eki oluşturma](/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-create)|
|PowerShell|[New-AzPublicIpPrefix](/powershell/module/az.network/new-azpublicipprefix)|

## <a name="create-a-static-public-ip-address-from-a-prefix"></a>Bir önekten statik genel IP adresi oluşturma
Bir önek oluşturduktan sonra statik IP adresi ön ekini oluşturmanız gerekir. Bunu yapmak için aşağıdaki adımları izleyin.

1. Metni içeren kutuya *kaynak Ara* Azure portalının üst kısmında, yazın *genel IP adresi ön eki*. Zaman **genel IP adresi ön eklerini** arama sonuçlarında görünmesini, onu seçin.
2. Ortak Ip'lerden oluşturmak istediğiniz ön eki seçin.
3. Arama sonuçlarında görüntülendiğinde seçin ve tıklayın **+ IP adresi Ekle** genel bakış bölümünde.
4. Girin veya seçin, aşağıdaki ayarları için değerleri **genel IP adresi oluşturma**. Bir önek, standart SKU, IPv4 ve statik olduğundan, yalnızca aşağıdaki bilgileri vermeniz gerekir:

   |Ayar|Gerekli mi?|Ayrıntılar|
   |---|---|---|
   |Ad|Evet|Genel IP adresinin adı kaynak grubu içinde benzersiz olmalıdır.|
   |Boşta kalma zaman aşımı (dakika)|Hayır|Etkin tutma iletileri göndermek için istemcileri kullanmadan bir TCP veya HTTP bağlantısını açık tutmak için kaç dakika. |
   |DNS ad etiketi|Hayır|(Tüm abonelikler ve müşteriler arasında) ad oluşturma Azure bölgesi içinde benzersiz olmalıdır. Ada sahip bir kaynak için bağlanabilmesi için azure otomatik olarak adı ve IP adresi, DNS kaydeder. Azure varsayılan alt ağ gibi ekler *location.cloudapp.azure.com* (konum seçtiğiniz konum olduğu) adına, DNS adı tam olarak nitelenmiş oluşturulacağını sağladığınız. Daha fazla bilgi için [Azure genel bir IP adresi ile Azure DNS kullanma](../dns/dns-custom-domain.md?toc=%2fazure%2fvirtual-network%2ftoc.json#public-ip-address).|

## <a name="view-or-delete-a-prefix"></a>Görüntülemek veya bir ön eki Sil

1. Metni içeren kutuya *kaynak Ara* Azure portalının üst kısmında, yazın *genel IP adresi ön eki*. Zaman **genel IP adresi ön eklerini** arama sonuçlarında görünmesini, onu seçin.
2. Görüntülemek istediğiniz ayarları değiştirin ya da listeden silmek genel IP adresi ön adını seçin.
3. Görüntüleyin, silin veya genel IP adresi ön eki değiştirmek istediğinize bağlı olarak aşağıdaki seçeneklerden birini tamamlayın.
   - **Görünüm**: **Genel bakış** öneki gibi genel IP adresi ön anahtar ayarları bölümünde gösterilir.
   - **Silme**: Genel IP adresi ön eki silmek için seçin **Sil** içinde **genel bakış** bölümü. Adres ön eki içinde genel IP adresi kaynaklarına ilişkiliyse, genel IP adresi kaynakları silmeniz gerekir. Bkz: [bir genel IP adresini silmek](virtual-network-public-ip-address.md#view-change-settings-for-or-delete-a-public-ip-address).

**Komutları**

|Tool|Komut|
|---|---|
|CLI|[az ağ public-ip ön ek listesini](/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-list) listesi genel IP adreslerine [az ağ public-ip önek show](/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-show) ayarları; göstermek için [az ağ public-ip önek güncelleştirme](/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-update) güncelleştirmek için; [az ağ public-ip ön eki Sil](/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-delete) silmek için|
|PowerShell|[Get-AzPublicIpPrefix](/powershell/module/az.network/get-azpublicipprefix) genel bir IP adresi nesnesi almak ve ilişkili ayarları görüntülemek için [kümesi AzPublicIpPrefix](/powershell/module/az.network/set-azpublicipprefix) ; ayarlarını güncelleştirmek için [Remove-AzPublicIpPrefix](/powershell/module/az.network/remove-azpublicipprefix) silmek için|

## <a name="permissions"></a>İzinler

Genel IP adresi ön eklerini görevleri gerçekleştirmek için hesabınızı atanmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) uygun eylemleri atanan rolü aşağıdaki tabloda listelenen:

| Eylem                                                            | Ad                                                           |
| ---------                                                         | -------------                                                  |
| Microsoft.Network/publicIPPrefixes/read                           | Genel bir IP adresi ön eki okuma                                |
| Microsoft.Network/publicIPPrefixes/write                          | Genel bir IP adresi ön eki güncelle                    |
| Microsoft.Network/publicIPPrefixes/delete                         | Genel bir IP adresi ön eki Sil                              |
|Microsoft.Network/publicIPPrefixes/join/action                     | Bir önekten genel bir IP adresi oluşturma |

## <a name="next-steps"></a>Sonraki adımlar

- Senaryolar ve kullanmanın avantajları hakkında bilgi edinin bir [genel IP öneki](public-ip-address-prefix.md)
