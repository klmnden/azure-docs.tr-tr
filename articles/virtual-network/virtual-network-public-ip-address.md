---
title: Oluşturma, değiştirme veya bir Azure genel IP adresini silmek | Microsoft Docs
description: Oluşturma, değiştirme veya bir genel IP adresini silmek öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: bb71abaf-b2d9-4147-b607-38067a10caf6
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: e1e82d7f7b6b8bf9bfef56b569db2db097b914ab
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64728740"
---
# <a name="create-change-or-delete-a-public-ip-address"></a>Oluşturma, değiştirme veya bir genel IP adresini Sil

Genel bir IP adresi ve oluşturmak, değiştirmek ve silmek hakkında bilgi edinin. Bir genel IP adresini, kendi yapılandırılabilir ayarlarına sahip bir kaynaktır. Genel IP adresleri destekleyen bir Azure kaynağı bir genel IP adresi atama sağlar:
- Azure sanal makineler (VM), Azure uygulama ağ geçitleri, Azure Load balancer'ları, Azure VPN ağ geçitleri ve diğerleri gibi kaynak Internet'ten gelen iletişimi. Yine de Internet'ten sanal makineleri gibi bazı kaynaklar ile bir VM, sanal makine bir yük dengeleyici arka uç havuzu bir parçasıdır ve yük dengeleyicinin genel IP adresi atanır sürece atanmış bir genel IP adresi yoksa iletişim kurabilir. Belirli bir Azure hizmeti için bir kaynak bir genel IP adresi atanmış veya olup, ile farklı bir Azure kaynağının genel IP adresi bildirilebilmesi belirlemek için hizmet için belgelere bakın.
- Tahmin edilebilir bir IP adresi kullanarak İnternet'e giden bağlantı. Örneğin, bir sanal makinenin genel IP adresi olmadan İnternet'e giden kendisine atanmış, ancak Azure tarafından varsayılan olarak bir öngörülemeyen genel adresine çevrilen ağ adresi kendi adresidir iletişim kurabilir. Bir kaynağa genel IP adresi atamayı hangi IP adresi giden bağlantı için kullanılan bilmenizi sağlar. Tahmin edilebilir olsa, seçilen atama yöntemine bağlı olarak adresi değişebilir. Daha fazla bilgi için [genel IP adresi oluşturma](#create-a-public-ip-address). Azure kaynaklarını giden bağlantılar hakkında daha fazla bilgi için bkz: [giden bağlantıları anlama](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu makalenin bir bölümündeki adımları tamamlamadan önce aşağıdaki görevleri tamamlayın:

- Azure hesabınız yoksa, kaydolmaya bir [ücretsiz deneme hesabınızı](https://azure.microsoft.com/free).
- Portalı kullanarak, açık https://portal.azure.comve Azure hesabınızda oturum.
- Bu makaledeki görevleri tamamlamak için PowerShell komutlarını kullanarak, ya da komutları çalıştırmak [Azure Cloud Shell](https://shell.azure.com/powershell), veya PowerShell bilgisayarınızdan çalıştırarak. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bu öğretici Azure PowerShell modülü sürüm 1.0.0 gerektirir veya üzeri. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.
- Bu makaledeki görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, ya da komutları çalıştırmak [Azure Cloud Shell](https://shell.azure.com/bash), veya bilgisayarınızdan CLI çalıştırarak. Bu öğretici, Azure CLI Sürüm 2.0.31 gerektirir veya üzeri. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli). Azure CLI'yi yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `az login` Azure ile bir bağlantı oluşturmak için.

Oturum açın ya da Azure ile bağlandığınız hesabı atanmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) içinde listelenen uygun eylemleri atanan [izinleri ](#permissions).

Genel IP adreslerinin nominal bir ücreti vardır. Fiyatlandırmayı görüntüleyin okuyun [IP adresi fiyatlandırması](https://azure.microsoft.com/pricing/details/ip-addresses) sayfası.

## <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

1. Portalın üst, sol köşede seçin **+ kaynak Oluştur**.
2. Girin *genel IP adresi* içinde *markette Ara* kutusu. Zaman **genel IP adresi** arama sonuçlarında görünür.
3. Altında **genel IP adresi**seçin **Oluştur**.
4. Altında aşağıdaki ayarları için değerleri seçin veya girin **genel IP adresi oluşturma**, ardından **Oluştur**:

   |Ayar|Gerekli mi?|Ayrıntılar|
   |---|---|---|
   |Ad|Evet|Adı kaynak grubu içinde benzersiz olmalıdır.|
   |SKU|Evet|SKU'ları sunulmasından önce oluşturulan tüm genel IP adresleri **temel** SKU genel IP adresleri. Genel IP adresi oluşturulduktan sonra SKU değiştirilemiyor. Tek başına sanal makine, sanal makineler bir kullanılabilirlik kümesinde veya sanal makine ölçek kümeleri, temel veya standart SKU'ları kullanabilirsiniz. Kullanılabilirlik kümeleri veya ölçek kümeleri içinde sanal makineler arasında SKU'ları karıştırılmasına izin verilmiyor. **Temel** SKU: Kullanılabilirlik alanları, desteklediği bir bölgede genel bir IP adresi oluşturuyorsanız **kullanılabilirlik alanı** ayarı *hiçbiri* varsayılan olarak. Genel IP adresi için belirli bir bölgenin güvence altına almak için bir kullanılabilirlik bölgesi seçin seçebilirsiniz. **Standart** SKU: Standart SKU genel IP, bir sanal makine ya da bir yük dengeleyici ön ucuna ilişkilendirilebilir. Kullanılabilirlik alanları, desteklediği bir bölgede genel bir IP adresi oluşturuyorsanız **kullanılabilirlik alanı** ayarı *bölgesel olarak yedekli* varsayılan olarak. Kullanılabilirlik alanları hakkında daha fazla bilgi için bkz. **kullanılabilirlik alanı** ayarı. Standart yük dengeleyici adresine ilişkilendirirseniz, standart SKU gereklidir. Standart load balancer'ları hakkında daha fazla bilgi için bkz: [Azure yük dengeleyici standart SKU'su](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Standart bir SKU genel IP adresini bir sanal makinenin ağ arabirimine atadığınızda amaçlanan trafiğe bir [ağ güvenlik grubuyla](security-overview.md#network-security-groups) açıkça izin vermeniz gerekir. Bir ağ güvenlik grubu oluşturup ilişkilendirene ve istenen trafiğe açıkça izin verene kadar kaynakla erişim kurma girişimleri başarısız olur.|
   |IP Sürümü|Evet| IPv4 veya IPv6'yı seçin. Genel IPv4 adresleri Azure kaynaklarına atanabilir, ancak bir IPv6 genel IP adresi yalnızca Internet'e yönelik yük dengeleyiciye atanabilir. Yük Dengeleyici Bakiye IPv6 trafiği Azure sanal makinelerine yükleyebilir. Daha fazla bilgi edinin [IPv6 trafiğini sanal makinelere yük dengelemeyi](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Seçtiyseniz **standart SKU**, tercih yapma seçeneğine sahip değilsiniz *IPv6*. Kullanırken yalnızca bir IPv4 adresi oluşturabilirsiniz **standart SKU**.|
   |IP adresi ataması|Evet|**Dinamik:** Dinamik adresler yalnızca genel IP adresi için bir Azure kaynağı ilişkilendirilir ve kaynağı ilk kez başlatıldığında sonra atanır. Bir sanal makine gibi bir kaynağa atanmış oldukları ve sanal makine durduruldu (serbest bırakıldı) dinamik adresler değiştirebilirsiniz ve yeniden başlatılacak. Bir sanal makine yeniden başlatıldı veya durduruldu (ancak serbest) adresi aynı kalır. Bir genel IP adresi kaynağı için ilişkili bir kaynaktan ilişkisi, dinamik adresleri serbest bırakılır. **Statik:** Genel bir IP adresi oluşturulduğunda statik adresleri atanır. Bir genel IP adresi kaynağı silinene kadar statik adresleri serbest bırakılmaz. Adresi bir kaynağa ilişkili değilse adresi oluşturulduktan sonra atama yöntemini değiştirebilirsiniz. Adresi bir kaynağa ilişkiliyse, atama yöntemini değiştirmek mümkün olmayabilir. Seçerseniz *IPv6* için **IP sürümü**, atama yöntemi *dinamik*. Seçerseniz *standart* için **SKU**, atama yöntemi *statik*.|
   |Boşta kalma zaman aşımı (dakika)|Hayır|Etkin tutma iletileri göndermek için istemcileri kullanmadan bir TCP veya HTTP bağlantısını açık tutmak için kaç dakika. IPv6 için seçerseniz **IP sürümü**, bu değer değiştirilemez. |
   |DNS ad etiketi|Hayır|Azure konum adı (tüm abonelikler ve müşteriler arasında) oluşturmak içinde benzersiz olmalıdır. Ada sahip bir kaynak için bağlanabilmesi için azure otomatik olarak adı ve IP adresi, DNS kaydeder. Azure varsayılan alt ağ gibi ekler *location.cloudapp.azure.com* (konum seçtiğiniz konum olduğu) adına, DNS adı tam olarak nitelenmiş oluşturulacağını sağladığınız. Her iki adres sürümü oluşturmayı seçerseniz, aynı DNS adı IPv4 ve IPv6 adreslerine atanır. Azure'nın varsayılan DNS hem bir IPv4 ve IPv6 AAAA ad kayıtları içerir ve DNS adı arandığında iki kaydı ile yanıt verir. İstemci ile iletişim kurmak için adresi (IPv4 veya IPv6) seçer. Varsayılan son ek ile DNS ad etiketini kullanmak yerine veya buna ek olarak, genel IP adresine çözümlenen bir özel son ek ile DNS adını yapılandırmak için Azure DNS hizmetini kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure genel IP adresiyle Azure DNS kullanma](../dns/dns-custom-domain.md?toc=%2fazure%2fvirtual-network%2ftoc.json#public-ip-address).|
   |Bir IPv6 (ya da IPv4) adresi oluşturma|Hayır| IPv6 veya IPv4 görüntülenip görüntülenmeyeceğini ne için seçtiğiniz üzerinde bağımlı **IP sürümü**. Örneğin, **IPv4** için **IP sürümü**, **IPv6** burada görüntülenir. Seçerseniz *standart* için **SKU**, bir IPv6 adresi oluşturma seçeneğiniz yoktur.
   |Ad (işaretlediyseniz görünür **bir IPv6 (ya da IPv4) adresi oluşturma** onay kutusu)|Evet, seçerseniz **bir IPv6 oluşturma** (veya IPv4) onay kutusu.|Ad için girdiğiniz adından farklı olmalıdır. ilk **adı** bu listede. Portal, hem bir IPv4 ve IPv6 adresi oluşturmayı seçerseniz, iki ayrı genel IP adresi kaynağı, kendisine atanmış bir IP adresi sürümü her biriyle oluşturur.|
   |IP adresi ataması (işaretlediyseniz görünür **bir IPv6 (ya da IPv4) adresi oluşturma** onay kutusu)|Evet, seçerseniz **bir IPv6 oluşturma** (veya IPv4) onay kutusu.|Onay kutusunu diyorsa **IPv4 adresi oluştur**, bir atama yöntemini seçebilirsiniz. Onay kutusunu diyorsa **IPv6 adresi oluştur**, olmalıdır, bir atama yöntemi seçilemiyor **dinamik**.|
   |Abonelik|Evet|Aynı bulunmalıdır [abonelik](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) için genel IP adresini ilişkilendirmek istediğiniz kaynağı olarak.|
   |Kaynak grubu|Evet|Aynı veya farklı bir arada var [kaynak grubu](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) için genel IP adresini ilişkilendirmek istediğiniz kaynağı olarak.|
   |Location|Evet|Aynı bulunmalıdır [konumu](https://azure.microsoft.com/regions), bölge genel IP'nin ilişkilendirmek istediğiniz kaynağı olarak adresi de denir.|
   |Kullanılabilirlik bölgesi| Hayır | Bu ayar, yalnızca desteklenen bir konum seçtiğinizde görünür. Desteklenen konumların bir listesi için bkz. [kullanılabilirlik alanlarına genel bakış](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Seçtiyseniz **temel** SKU, *hiçbiri* sizin için otomatik olarak seçilir. Belirli bir bölgenin garanti tercih ederseniz, belirli bir bölgenin seçebilirsiniz. Her iki seçenek bölgesel olarak yedekli değildir. Seçtiyseniz **standart** SKU: Bölgesel olarak yedekli sizin için otomatik olarak seçilir ve veri yolunuz bölge hatasına dayanıklı hale getirir. Bölge hatalarına karşı dayanıklı değil, belirli bir bölgenin sağlamak isterseniz belirli bir bölgenin seçebilirsiniz.

**Komutları**

Portal iki genel IP adresi kaynağı (bir IPv4 ve bir IPv6) oluşturma seçeneğini sağlıyor olsa da aşağıdaki CLI ve PowerShell komutları için bir IP sürümü veya başka bir adres ile bir kaynak oluşturun. İki ortak IP adresi kaynaklarına, her IP sürümü için bir tane istiyorsanız farklı adlar ve genel IP adresi kaynakları sürümlerinin belirten iki kez komutunu çalıştırmanız gerekir.

|Tool|Komut|
|---|---|
|CLI|[az network public-ip create](/cli/azure/network/public-ip#az-network-public-ip-create)|
|PowerShell|[New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress)|

## <a name="view-change-settings-for-or-delete-a-public-ip-address"></a>Görüntülemek, ayarlarını değiştirmek veya bir genel IP adresini Sil

1. Metni içeren kutuya *kaynak Ara* Azure portalının üst kısmında, yazın *genel IP adresi*. Zaman **genel IP adresleri** arama sonuçlarında görünmesini, onu seçin.
2. Genel IP adresini görüntülemek istediğiniz ayarlarını değiştirme veya listeden silmek adını seçin.
3. Görüntüleyin, silin veya genel IP adresini değiştirmek istediğinize bağlı olarak aşağıdaki seçeneklerden birini tamamlayın.
   - **Görünüm**: **Genel bakış** bölüm ilişkili için (adresini bir ağ arabirimi ile ilişkili ise) ağ arabirimi gibi genel IP adresi için anahtar ayarlarını gösterir. Portal sürümü (IPv4 veya IPv6) adresi, görüntülemez. Sürüm bilgilerini görüntülemek için genel IP adresini görüntülemek için PowerShell veya CLI komutunu kullanın. IPv6 IP adresi sürümü ise portal, PowerShell veya CLI tarafından atanan adresi görüntülenmez.
   - **Silme**: Genel IP adresini silmek için seçin **Sil** içinde **genel bakış** bölümü. Adresi şu anda bir IP yapılandırmasıyla ilişkili ise, bu komut dosyası silinemiyor. Adresi şu anda bir yapılandırma ile ilişkili ise, seçin **Bulutlarından** IP yapılandırması adresinden ilişkilendirmesini kaldırmak.
   - **Değişiklik**: seçin **yapılandırma**. 4. adımda verilen bilgileri kullanarak Ayarları Değiştir [genel IP adresi oluşturma](#create-a-public-ip-address). Bir IPv4 adresi atamanın statik olan dinamik olarak değiştirmek için önce ilişkili olduğu IP yapılandırmasından genel IPv4 adresi ilişkilendirmesini kaldırmanız gerekir. Dinamik ve seçin sonra atama yöntemini değiştirebilirsiniz **ilişkilendirmek** IP ilişkilendirmek için aynı IP yapılandırması, farklı bir yapılandırma veya adresine, ilkenin ilişkisi bırakılabilir. İçinde bir genel IP adresi ilişkilendirmesini kaldırmak **genel bakış** bölümünden **Bulutlarından**.

   >[!WARNING]
   >Atama yöntemi statik olan dinamik olarak değiştirdiğinizde, genel IP adresi için atanan IP adresi kaybedersiniz. Azure ortak DNS sunucularını (bir tanımladıysanız) tüm DNS ad etiketi ile statik veya dinamik adresler arasında bir eşleme bakım yaparken, sanal makine durdurulmuş (serbest bırakıldığında) duruma kaldıktan sonra başlatıldığında dinamik IP adresi değiştirebilirsiniz. Adresinin değişmesini önlemek için statik bir IP adresi atayın.

**Komutları**

|Tool|Komut|
|---|---|
|CLI|[az ağ genel IP listesi](/cli/azure/network/public-ip#az-network-public-ip-list) listesi genel IP adreslerine [az ağ public-ip show](/cli/azure/network/public-ip#az-network-public-ip-show) ayarları; göstermek için [az ağ public-ip update](/cli/azure/network/public-ip#az-network-public-ip-update) güncelleştirmek için; [az ağ public-ip delete](/cli/azure/network/public-ip#az-network-public-ip-delete) silmek için|
|PowerShell|[Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) genel bir IP adresi nesnesi almak ve ilişkili ayarları görüntülemek için [kümesi AzPublicIpAddress](/powershell/module/az.network/set-azpublicipaddress) ; ayarlarını güncelleştirmek için [Remove-AzPublicIpAddress](/powershell/module/az.network/remove-azpublicipaddress) silmek için|

## <a name="assign-a-public-ip-address"></a>Genel bir IP adresi atayın

Aşağıdaki kaynaklar için bir genel IP adresi atama işlemleri gerçekleştirmeyi öğreneceksiniz:

- A [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (oluştururken) VM veya bir [var olan VM](virtual-network-network-interface-addresses.md#add-ip-addresses)
- [Internet'e yönelik Yük Dengeleyici](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure uygulama ağ geçidi](../application-gateway/application-gateway-create-gateway-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Bir Azure VPN ağ geçidi kullanarak siteden siteye bağlantı](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure sanal makine ölçek kümesi](../virtual-machine-scale-sets/virtual-machine-scale-sets-portal-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

## <a name="permissions"></a>İzinler

Genel IP adreslerinde görevleri gerçekleştirmek için hesabınızı atanmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) uygun eylemleri atanan rolü aşağıdaki tabloda listelenen:

| Eylem                                                             | Ad                                                           |
| ---------                                                          | -------------                                                  |
| Microsoft.Network/publicIPAddresses/read                           | Genel bir IP adresi okuyun                                          |
| Microsoft.Network/publicIPAddresses/write                          | Bir genel IP adresini güncelle                           |
| Microsoft.Network/publicIPAddresses/delete                         | Bir genel IP adresini Sil                                     |
| Microsoft.Network/publicIPAddresses/join/action                    | Bir kaynağa genel IP adresi ilişkilendirme                    |

## <a name="next-steps"></a>Sonraki adımlar

- Genel bir IP adresini kullanarak oluşturmak [PowerShell](powershell-samples.md) veya [Azure CLI](cli-samples.md) örnek komut dosyaları veya Azure kullanarak [Resource Manager şablonları](template-samples.md)
- Oluşturma ve uygulama [Azure İlkesi](policy-samples.md) için genel IP adresleri