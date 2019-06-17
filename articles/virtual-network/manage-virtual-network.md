---
title: Oluşturma, değiştirme veya bir Azure sanal ağı Sil
titlesuffix: Azure Virtual Network
description: Oluşturma, değiştirme veya bir Azure sanal ağı silme öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/10/2019
ms.author: kumud
ms.openlocfilehash: 235a82c6bba4165790c370c2641ee6cd41f10840
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64700476"
---
# <a name="create-change-or-delete-a-virtual-network"></a>Oluşturma, değiştirme veya bir sanal ağı silme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Oluşturma ve bir sanal ağı silmek ve DNS sunucuları ve var olan bir sanal ağ için IP adresi alanları gibi ayarları değiştirme hakkında bilgi edinin. Sanal ağlara yeniyseniz, bunları hakkında daha fazla bilgi edinebilirsiniz [sanal ağa genel bakış](virtual-networks-overview.md) veya tamamlayarak bir [öğretici](quick-create-portal.md). Bir sanal ağ alt ağları içeriyor. Oluşturma, değiştirme ve alt ağları Sil öğrenmek için bkz. [alt ağları yönetme](virtual-network-manage-subnet.md).

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalenin bir bölümündeki adımları tamamlamadan önce aşağıdaki görevleri tamamlayın:

- Azure hesabınız yoksa, kaydolmaya bir [ücretsiz deneme hesabınızı](https://azure.microsoft.com/free).
- Portalı kullanarak, açık https://portal.azure.com ve Azure hesabınızda oturum.
- Bu makaledeki görevleri tamamlamak için PowerShell komutlarını kullanarak, ya da komutları çalıştırmak [Azure Cloud Shell](https://shell.azure.com/powershell), veya PowerShell bilgisayarınızdan çalıştırarak. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bu öğretici Azure PowerShell modülü sürüm 1.0.0 gerektirir veya üzeri. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.
- Bu makaledeki görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, ya da komutları çalıştırmak [Azure Cloud Shell](https://shell.azure.com/bash), veya bilgisayarınızdan CLI çalıştırarak. Bu öğretici, Azure CLI Sürüm 2.0.31 gerektirir veya üzeri. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli). Azure CLI'yi yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `az login` Azure ile bir bağlantı oluşturmak için.
- Oturum açın ya da Azure ile bağlandığınız hesabı atanmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) içinde listelenen uygun eylemleri atanan [izinleri ](#permissions).

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

1. Seçin **+ kaynak Oluştur** > **ağ** > **sanal ağ**.
2. Girin veya aşağıdaki ayarları için değerleri seçip **Oluştur**:
   - **Ad**: Ad benzersiz olmalıdır [kaynak grubu](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) sanal ağ oluşturulacağını seçin. Sanal ağ oluşturduktan sonra adını değiştiremezsiniz. Zaman içinde birden fazla sanal ağ oluşturabilirsiniz. Öneriler adlandırmak için bkz: [adlandırma kuralları](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions#naming-rules-and-restrictions). Bir adlandırma kuralı birden fazla sanal ağ yönetmeyi kolaylaştırmak yardımcı olabilir.
   - **Adres alanı**: Bir sanal ağ adres alanı CIDR gösteriminde belirtilen bir veya daha fazla çakışmayan adres aralıkları oluşur. Tanımladığınız adres aralığı, genel veya özel (RFC 1918) olabilir. Genel veya özel olarak adres aralığını tanımlamak olsun, adres aralığı yalnızca birbirine bağlı sanal ağlar ve sanal ağa bağlı tüm şirket içi ağlarınızı sanal ağ içindeki erişilebiliyor. Aşağıdaki adresi aralıklarını eklenemiyor:
     - 224.0.0.0/4 (çok noktaya yayın)
     - 255.255.255.255/32 (yayın)
     - 127.0.0.0/8 (geri döngü)
     - 169.254.0.0/16 (bağlantı-yerel)
     - 168.63.129.16/32 (iç DNS, DHCP ve Azure Load Balancer [durum araştırması](../load-balancer/load-balancer-custom-probe-overview.md#probesource))

     Sanal ağ oluştururken yalnızca bir adres aralığı tanımlayabilirsiniz, sanal ağ oluşturulduktan sonra daha fazla adres aralıkları adres alanına ekleyebilirsiniz. Mevcut bir sanal ağa adres aralığı ekleme konusunda bilgi için bkz [Ekle veya Kaldır'ı bir adres aralığı](#add-or-remove-an-address-range).

     >[!WARNING]
     >İki ağ, sanal ağ başka bir sanal ağ ile çakışan adres aralıkları varsa veya şirket içinde ağ bağlanamaz. Bir adres aralığı tanımlamadan önce gelecekte diğer sanal ağlara veya şirket içi ağları sanal ağa bağlanmak isteyebileceğiniz olup olmadığını göz önünde bulundurun.
     >
     >

     - **Alt ağ adı**: Alt ağ adı sanal ağ içinde benzersiz olmalıdır. Alt ağ oluşturulduktan sonra alt ağ adı değiştiremezsiniz. Portal, bir sanal ağ tüm alt ağlara sahip olması gereken da bir sanal ağ oluşturduğunuzda, bir alt ağda tanımladığınız gerektiriyor. Portalda, sanal ağ oluştururken yalnızca bir alt ağ tanımlayabilirsiniz. Sanal ağ oluşturulduktan sonra daha fazla alt ağı sanal ağa daha sonra ekleyebilirsiniz. Bir sanal ağa bir alt ağı eklemek için bkz: [alt ağları yönetme](virtual-network-manage-subnet.md). Azure CLI veya PowerShell kullanarak birden çok alt ağa sahip bir sanal ağ oluşturabilirsiniz.

       >[!TIP]
       >Bazı durumlarda, yöneticiler filtrelemek veya alt ağlar arasında trafiği yönlendirme denetlemek için farklı alt ağlar oluşturun. Alt ağlar tanımlamanız önce alt ağlar arasında trafiği yönlendirmek ve filtrelemek isteyebilirsiniz nasıl göz önünde bulundurun. Alt ağlar arası trafik filtreleme hakkında daha fazla bilgi için bkz. [ağ güvenlik grupları](security-overview.md). Azure otomatik olarak alt ağlar, ancak arasındaki trafiği yönlendirir Azure varsayılan yolları geçersiz kılabilirsiniz. Azures varsayılan alt ağ trafiğini yönlendirme hakkında daha fazla bilgi için bkz: [yönlendirmeye genel bakış](virtual-networks-udr-overview.md).
       >

     - **Alt ağ adres aralığı**: Sanal ağ için girilen adres alanı içinde olmalıdır. Belirtebileceğiniz en küçük /29, alt ağ için sekiz IP adreslerini sağlayan aralığındadır. Azure her alt ağda protokol uyumluluğu için ilk ve son adresi ayırır. Üç ek adresleri, Azure hizmet kullanımı için ayrılmıştır. Sonuç olarak, bir sanal ağ bir / 29 alt ağ adres aralığı ile yalnızca üç kullanılabilir IP adresleri bulunur. Bir sanal ağ VPN ağ geçidi bağlamayı planlıyorsanız, ağ geçidi alt ağı oluşturmanız gerekir. Daha fazla bilgi edinin [ağ geçidi alt ağları için belirli bir adres aralığı konuları](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). Belirli koşullar altında bir alt ağ oluşturulduktan sonra adres aralığını değiştirebilirsiniz. Bir alt ağ adres aralığı değiştirme konusunda bilgi edinmek için [alt ağları yönetme](virtual-network-manage-subnet.md).
     - **Abonelik**: Seçin bir [abonelik](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription). Aynı sanal ağda birden fazla Azure aboneliğinde kullanamazsınız. Ancak, bir Abonelikteki bir sanal ağı ile diğer Aboneliklerdeki sanal ağlara bağlanabilirsiniz [sanal ağ eşlemesi](virtual-network-peering-overview.md). Sanal ağa bağlanan tüm Azure kaynakları, sanal ağ ile aynı abonelikte olmalıdır.
     - **Kaynak grubu**: Mevcut bir seçin [kaynak grubu](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-groups) veya yeni bir tane oluşturun. Sanal ağ ile aynı kaynak grubunda veya farklı bir kaynak grubu sanal ağa bağlanan bir Azure kaynağı olabilir.
     - **Konum**: Azure'ı seçin [konumu](https://azure.microsoft.com/regions/), bölge olarak da bilinir. Bir sanal ağ, Azure yalnızca bir konumda olabilir. Ancak, bir sanal ağ tek bir konumda bir sanal ağ başka bir konuma bir VPN ağ geçidi'ni kullanarak bağlanabilirsiniz. Sanal ağa bağlanan tüm Azure kaynakları, sanal ağ ile aynı konumda olmalıdır.

**Komutları**

- Azure CLI: [az ağ vnet oluşturma](/cli/azure/network/vnet)
- PowerShell: [Yeni AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)

## <a name="view-virtual-networks-and-settings"></a>Sanal ağları görüntüle ve ayarları

1. Portalın üst kısmındaki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünmesini, onu seçin.
2. Sanal ağlar listesinden ilgili ayarları görüntülemek istediğiniz sanal ağı seçin.
3. Seçtiğiniz sanal ağ için aşağıdaki ayarları listelenmiştir:
   - **Genel Bakış**: Adres alanı ve DNS sunucuları da dahil olmak üzere sanal ağ hakkında bilgi sağlar. Adlı bir sanal ağ için genel ayarları aşağıdaki ekran görüntüsünde gösterilmektedir **MyVNet**:

     ![Ağ arabirimine genel bakış](./media/manage-virtual-network/vnet-overview.png)

     Bir sanal ağ seçerek farklı bir abonelik veya kaynak grubuna taşıyabilirsiniz **değişiklik** yanındaki **kaynak grubu** veya **abonelik adı**. Bir sanal ağ taşıma hakkında bilgi edinmek için [kaynakları farklı kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Makalede Önkoşullar ve Azure portalı, PowerShell ve Azure CLI kullanarak kaynakları taşıma listelenmektedir. Sanal ağa bağlı tüm kaynaklar ile sanal ağa taşımanız gerekir.
   - **Adres alanı**: Sanal ağa atanır adres alanları listelenir. Adres alanına adres aralığı ekleyip öğrenmek için adımları tamamlamanız [Ekle veya Kaldır'ı bir adres aralığı](#add-or-remove-an-address-range).
   - **Bağlı cihazlar**: Sanal ağa bağlı herhangi bir kaynağa listelenir. Önceki ekran görüntüsünde, üç ağ arabirimleri ve bir yük dengeleyici sanal ağa bağlanır. Tüm yeni kaynaklar oluşturup sanal ağa bağlama listelenir. Sanal ağa bağlı bir kaynağı silerseniz, artık listede görünür.
   - **Alt ağlar**: Sanal ağ içinde mevcut alt ağlar listesi gösterilir. Bir alt ağı ekleyip öğrenmek için bkz. [alt ağları yönetme](virtual-network-manage-subnet.md).
   - **DNS sunucuları**: Azure iç DNS sunucusu veya özel bir DNS sunucusu ad çözümlemesi için sanal ağa bağlı cihazları sağlayıp sağlamadığını belirtebilirsiniz. Azure portalını kullanarak bir sanal ağ oluşturduğunuzda, Azure'nın DNS sunucuları, sanal ağ içindeki ad çözümlemesi için varsayılan olarak kullanılır. DNS sunucuları değiştirmek için adımları tamamlamanız [değişiklik DNS sunucuları](#change-dns-servers) bu makaledeki.
   - **Eşlemeler**: Abonelikte var olan eşlemeleri varsa, burada listelenir. Var olan eşlemeleri için ayarları görüntüleyin veya oluşturma, değiştirme veya silme eşlemeleri. Eşlemeleri hakkında daha fazla bilgi için bkz: [sanal ağ eşlemesi](virtual-network-peering-overview.md).
   - **Özellikler**: Sanal ağın kaynak Kimliğini ve içinde bir Azure aboneliği de dahil olmak üzere sanal ağ ayarlarını görüntüler.
   - **Diyagram**: Diyagram, sanal ağa bağlı tüm cihazlara görsel bir gösterimini sağlar. Diyagram cihazlarla ilgili bazı temel bilgileri yok. Bu görünümde, diyagramdaki bir cihazı yönetmek için cihazı seçin.
   - **Azure ortak ayarları**: Azure ortak ayarları hakkında daha fazla bilgi için aşağıdaki bilgileri bakın:
     - [Etkinlik Günlüğü](../azure-monitor/platform/activity-logs-overview.md)
     - [Erişim denetimi (IAM)](../role-based-access-control/overview.md)
     - [Etiketler](../azure-resource-manager/resource-group-using-tags.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
     - [Kilitler](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
     - [Otomasyon betiği](../azure-resource-manager/manage-resource-groups-portal.md#export-resource-groups-to-templates)

**Komutları**

- Azure CLI: [az ağ vnet show](/cli/azure/network/vnet)
- PowerShell: [Get-AzVirtualNetwork](/powershell/module/az.network/get-azvirtualnetwork)

## <a name="add-or-remove-an-address-range"></a>Bir adres aralığı Ekle Kaldır

Ekleyebilir ve sanal ağ için adres aralıklarını kaldırın. Bir adres aralığı CIDR gösteriminde belirtilmelidir ve aynı sanal ağdaki başka adres aralıklarıyla çakışamaz. Tanımladığınız adres aralıkları, genel veya özel (RFC 1918) olabilir. Genel veya özel olarak adres aralığını tanımlamak olsun, adres aralığı yalnızca birbirine bağlı sanal ağlar ve sanal ağa bağlı tüm şirket içi ağlarınızı sanal ağ içindeki erişilebiliyor. 

İlişkili hiçbir alt ağ yoksa sanal ağ adres aralığı düşürebilir. Aksi takdirde, örneğin, adres aralığı için bir /16 /8 değiştirme yalnızca genişletebilirsiniz. Bir küçük adres aralığı ile başlayın ve ardından daha sonra genişletmek veya ek aralıkları ekleyin.

<!-- the last two sentences above are added per GitHub issue https://github.com/MicrosoftDocs/azure-docs/issues/20572 -->

Aşağıdaki adresi aralıklarını eklenemiyor:

- 224.0.0.0/4 (çok noktaya yayın)
- 255.255.255.255/32 (yayın)
- 127.0.0.0/8 (geri döngü)
- 169.254.0.0/16 (bağlantı-yerel)
- 168.63.129.16/32 (iç DNS, DHCP ve Azure Load Balancer [durum araştırması](../load-balancer/load-balancer-custom-probe-overview.md#probesource))

Eklemek veya bir adres aralığı kaldırmak için:

1. Portalın üst kısmındaki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünmesini, onu seçin.
2. Sanal ağlar listesinden eklemek veya bir adres aralığı kaldırmak istediğiniz sanal ağı seçin.
3. Seçin **adres alanı**altında **ayarları**.
4. Aşağıdaki seçeneklerden birini tamamlayın:
    - **Bir adres aralığı Ekle**: Yeni bir adres aralığı girin. Adres aralığını, sanal ağ için tanımlanan var olan bir adres aralığı ile çakışamaz.
    - **Bir adres aralığı kaldırmak**: Kaldırmak istediğiniz adres aralığını sağ tarafta seçin **...** , ardından **Kaldır**. Bir alt ağ adres aralığı varsa, adres aralığı kaldırılamıyor. Bir adres aralığı kaldırmak için önce tüm alt ağların (ve alt ağlarda herhangi bir kaynağa) silmelisiniz adres aralığı yok.
5. **Kaydet**’i seçin.

**Komutları**

- Azure CLI: [az ağ vnet update](/cli/azure/network/vnet)
- PowerShell: [Set-AzVirtualNetwork](/powershell/module/az.network/set-azvirtualnetwork)

## <a name="change-dns-servers"></a>DNS sunucularını değiştirme

Sanal ağ kaydı belirttiğiniz sanal ağ için DNS sunucuları ile bağlı tüm sanal makineler. Bunlar ayrıca ad çözümlemesi için belirtilen DNS sunucusu kullanın. Bir VM'deki her ağ arabirimi (NIC) kendi DNS sunucu ayarlarına sahip olabilir. Bir NIC kendi DNS sunucu ayarları varsa, sanal ağın DNS sunucusu ayarlarını geçersiz kılın. NIC DNS ayarları hakkında daha fazla bilgi edinmek için [ağ arabirimi görevler ve ayarlar](virtual-network-network-interface.md#change-dns-servers). VM'ler ve Azure Cloud Services'ta bir rol örnekleri için ad çözümlemesi hakkında daha fazla bilgi edinmek için bkz. [VM'ler ve rol örnekleri için ad çözümlemesi](virtual-networks-name-resolution-for-vms-and-role-instances.md). Ekleme, değiştirme veya bir DNS sunucusunu kaldırmak için:

1. Portalın üst kısmındaki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünmesini, onu seçin.
2. Sanal ağlar listesinden, DNS sunucuları için değiştirmek istediğiniz sanal ağı seçin.
3. Seçin **DNS sunucuları**altında **ayarları**.
4. Aşağıdaki seçeneklerden birini seçin:
   - **Varsayılan (Azure tarafından sağlanan)** : Tüm kaynak adları ve özel IP adresleri, Azure DNS sunucularını otomatik olarak kaydedilir. Aynı sanal ağa bağlı herhangi bir kaynağa arasında adları çözebilirsiniz. Sanal ağlar arasında adlarını çözümlemek için bu seçeneği kullanamazsınız. Sanal ağlar arasında adlarını çözümlemek için özel bir DNS sunucusu kullanmanız gerekir.
   - **Özel**: Bir sanal ağ için Azure sınıra kadar bir veya daha fazla sunucu ekleyebilirsiniz. DNS sunucusu sınırları hakkında daha fazla bilgi için bkz: [Azure sınırları](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-networking-limits-classic). Aşağıdaki seçenekleriniz vardır:
   - **Adres Ekle**: Sunucu, sanal ağın DNS sunucuları listesine ekler. Bu seçenek, DNS sunucusu ayrıca Azure ile kaydeder. Azure ile bir DNS sunucusu kaydettiğinize göre bu DNS sunucusu listesinde seçebilirsiniz.
   - **Bir adresi kaldırmak**: Kaldırmak istediğiniz sunucuyu yanındaki seçin **...** , ardından **Kaldır**. Sunucuyu silmek sunucuya yalnızca bu sanal ağ listeden kaldırır. DNS sunucusu kullanmak için Azure'da diğer sanal ağlarınız için kayıtlı kalır.
   - **DNS sunucusu adresleri yeniden sıralama**: DNS sunucularınızın doğru sırada ortamınız için liste doğrulamak önemlidir. DNS sunucu listesine belirtildikleri sırada kullanılır. Hepsini bir kez deneme ayarı olarak çalışmaz. Listedeki ilk DNS sunucusunu ulaşılabilirse, istemci DNS sunucusu düzgün çalışmasını bağımsız olarak, DNS sunucusu kullanır. Listelenen tüm DNS sunucuları kaldırın ve ardından bunları geri istediğiniz sırayla ekleyin.
   - **Adresle değiştirmek**: DNS sunucusu listesinde vurgulayın ve ardından yeni adresi girin.
5. **Kaydet**’i seçin.
6. Yeni DNS sunucusu ayarlarını atanmış oldukları için sanal ağa bağlı sanal makineleri yeniden başlatın. Vm'leri yeniden başlatılana kadar geçerli DNS ayarlarını kullanmaya devam edin.

**Komutları**

- Azure CLI: [az ağ vnet update](/cli/azure/network/vnet)
- PowerShell: [Set-AzVirtualNetwork](/powershell/module/az.network/set-azvirtualnetwork)

## <a name="delete-a-virtual-network"></a>Bir sanal ağı silme

Yalnızca ona bağlı hiçbir kaynaklar varsa, bir sanal ağ silebilirsiniz. Sanal ağ içindeki herhangi bir alt ağa bağlı kaynaklar varsa, sanal ağ içindeki tüm alt ağlara bağlı kaynakları silmeniz gerekir. Kaynak silme için uygulayacağınız adımlar, kaynak bağlı olarak farklılık gösterir. Alt ağlara bağlı kaynakları silmek öğrenmek için silmek istediğiniz her bir kaynak türü için belgeleri okuyun. Bir sanal ağı silmek için:

1. Portalın üst kısmındaki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünmesini, onu seçin.
2. Sanal ağlar listesinden silmek istediğiniz sanal ağı seçin.
3. Cihaz seçerek sanal ağa bağlı olduğundan emin olun **bağlı cihazları**altında **ayarları**. Bağlı cihazlar varsa, sanal ağı silmeden önce bunları silmeniz gerekir. Bağlı cihaz yok ise seçin **genel bakış**.
4. **Sil**’i seçin.
5. Sanal ağ silinmesini onaylamak için seçin **Evet**.

**Komutları**

- Azure CLI: [azure ağı vnet Sil](/cli/azure/network/vnet)
- PowerShell: [Remove-AzVirtualNetwork](/powershell/module/az.network/remove-azvirtualnetwork)

## <a name="permissions"></a>İzinler

Sanal ağlarda görevleri gerçekleştirmek için hesabınızı atanmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) uygun eylemleri atanan rolü aşağıdaki tabloda listelenen:

| Eylem                                  |   Ad                                |
|---------------------------------------- |   --------------------------------    |
|Microsoft.Network/virtualNetworks/read   |   Bir sanal ağ okuyun              |
|Microsoft.Network/virtualNetworks/write  |   Bir sanal ağ oluştur veya güncelleştir  |
|Microsoft.Network/virtualNetworks/delete |   Bir sanal ağı silme            |

## <a name="next-steps"></a>Sonraki adımlar

- Kullanarak bir sanal ağ oluşturma [PowerShell](powershell-samples.md) veya [Azure CLI](cli-samples.md) örnek komut dosyaları veya Azure kullanarak [Resource Manager şablonları](template-samples.md)
- Oluşturma ve uygulama [Azure İlkesi](policy-samples.md) sanal ağlar için
