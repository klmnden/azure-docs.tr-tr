---
title: "VPN ağ geçidi Klasik'ten Kaynak Yöneticisi geçirme | Microsoft Docs"
description: "Bu sayfa, VPN ağ geçidi Klasik Kaynak Yöneticisi'ni geçişine genel bakış sağlar."
documentationcenter: na
services: vpn-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: caa8eb19-825a-4031-8b49-18fbf3ebc04e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/02/2017
ms.author: amsriva
ms.openlocfilehash: 1164fc24355657af22b6befaad74685ebbc2b5cb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="vpn-gateway-classic-to-resource-manager-migration"></a>VPN ağ geçidi Klasik'ten Kaynak Yöneticisi geçirme
VPN ağ geçitleri şimdi Klasik Resource Manager dağıtım modeline geçirilebilir. Daha fazla bilgiyi Azure Kaynak Yöneticisi hakkında [özellikler ve sunduğu yararlar](../azure-resource-manager/resource-group-overview.md). Bu makalede, biz Klasik dağıtımlarından daha yeni Resource Manager temelli modeline geçirmek nasıl ayrıntılı olarak açıklanmaktadır. 

VPN ağ geçitleri, Klasik VNet geçiş için Kaynak Yöneticisi'ni bir parçası olarak geçirilir. Bu geçiş bir kerede bir VNet yapılır. Araçlar ya da geçiş için Önkoşullar açısından ek gereksinimi yoktur. Geçiş adımları mevcut VNet geçiş özdeş ve adresinde belgelenen [Iaas kaynaklar geçiş sayfası](../virtual-machines/windows/migration-classic-resource-manager-ps.md). Geçiş sırasında hiçbir veri yolu kapalı kalma süresi olduğunu ve bu nedenle var olan iş yükleri şirket içi bağlantı kaybı geçiş sırasında çalışmaya devam eder. VPN ağ geçidi ile ilişkili ortak IP adresi geçiş işlemi sırasında değiştirmez. Bu, geçiş tamamlandıktan sonra şirket içi yönlendiriciyi yeniden yapılandırarak gerekmez anlamına gelir.  

Kaynak Yöneticisi'nde modeli Klasik modelinden farklıdır ve sanal ağ geçitleri, yerel ağ geçitleri ve bağlantı kaynaklarını oluşur. Bunlar, VPN ağ geçidinin kendisi, şirket içi adres alanı ve ikisi arasında bağlantı sırasıyla temsil eden yerel site temsil eder. Geçiş tamamlandıktan sonra ağ geçitleri Klasik modelde kullanılabilir olmaz ve Resource Manager modelini kullanarak sanal ağ geçitleri, yerel ağ geçitleri ve bağlantı nesneleri tüm yönetim işlemlerinin gerçekleştirilmesi gerekir.

## <a name="supported-scenarios"></a>Desteklenen senaryolar
En yaygın VPN bağlantısı senaryoları Klasik'ten Kaynak Yöneticisi geçirme tarafından ele alınmıştır. Desteklenen senaryoları Ekle-

* Site bağlantı noktası
* Siteden siteye bağlantı VPN ağ geçidi ile şirket içi konumuna bağlı
* VNet için VPN ağ geçitleri kullanarak iki Vnet arasında VNet bağlantısı
* Birden çok sanal ağlar aynı şirket içi konumuna bağlı
* Çok siteli bağlantı
* Zorlamalı tünel sanal ağlar etkin

Desteklenmeyen senaryolar da dahil-  

* Sanal ağ ExpressRoute ağ geçidi ve VPN ağ geçidi ile şu anda desteklenmiyor.
* VM uzantıları için şirket içi sunucular, bağlı geçiş senaryoları. Transit VPN bağlantısı sınırlamalar aşağıda açıklanmıştır.

> [!NOTE]
> Resource Manager modelinde CIDR doğrulama olandan Klasik modelde daha katı. Geçirmeden önce Klasik adres aralıklarını verilen geçiş işlemine başlamadan önce geçerli CIDR biçimine uygun emin olun. CIDR tüm ortak CIDR doğrulayıcıları kullanılarak doğrulanabilir. VNet veya geçişi olduğunda geçersiz CIDR aralıkları ile yerel siteleri başarısız durumda neden olur.
> 
> 

## <a name="vnet-to-vnet-connectivity-migration"></a>VNet bağlantısı geçiş Vnet'e
VNet Bağlantısı'nda Klasik bir Vnet'e bağlı sanal ağ yerel sitesi gösterimini oluşturarak elde. Müşteriler, birbirine bağlı olması gereken iki Vnet temsil iki yerel siteleri oluşturmak için gerekli olmuştur. Bunlar daha sonra iki sanal ağlar arasında bağlantı kurmak için IPSec tüneli kullanarak karşılık gelen sanal ağlara bağlı. Bir sanal ağ adres aralığı değişiklikler, karşılık gelen yerel site gösterimini korunmalıdır beri bu modeli yönetilebilirlik sorunları vardır. Resource Manager modelinde, bu geçici çözüm artık gerekli değildir. İki sanal ağlar arasındaki bağlantıyı doğrudan bağlantı kaynağında 'Vnet2Vnet' bağlantı türü kullanılarak gerçekleştirilebilir. 

![Ekran görüntüsü, VNet geçiş Vnet'e.](./media/vpn-gateway-migration/migration1.png)

VNet geçiş sırasında geçerli sanal ağınızın VPN ağ geçidine bağlı varlık başka bir VNet olduğunu ve her iki Vnet'in geçiş tamamlandıktan sonra diğer Vnet'in temsil eden iki yerel siteleri artık görür olun algıla. İki VPN ağ geçitleri ve Vnet2Vnet türünde iki bağlantılarıyla Resource Manager modeli için iki VPN ağ geçitleri, iki yerel site ve aralarında iki bağlantı Klasik modeli dönüştürüldüğünde.

## <a name="transit-vpn-connectivity"></a>Transit VPN bağlantısı
VPN ağ geçitleri bir VNet için şirket içi bağlantı şirket içi doğrudan bağlı başka bir sanal ağa bağlanarak gerçekleştirilir gibi bir topolojisinde yapılandırabilirsiniz. Bu, geçiş VPN bağlantısı, ilk VNet durumlarda şirket içi doğrudan bağlı bağlı sanal ağ VPN ağ geçidi geçiş aracılığıyla şirket kaynaklarına bağlı olur. Bu yapılandırma Klasik dağıtım modelinde elde etmek için iki bağlı VNet temsil eden önekleri toplanmış bir yerel site oluşturmanız gerekir ve şirket içi adres alanı. Bu temsili yerel site sonra geçiş bağlantısı elde etmek için Vnet'e bağlandı. Şirket içi adres aralığındaki herhangi bir değişiklik, sanal ağ ve şirket içi bir toplamasını temsil eden yerel site korunmalıdır beri bu Klasik modeli benzer yönetilebilirlik güçlükler de vardır. Resource Manager'ın desteklenen ağ geçidi BGP desteği giriş bağlı ağ geçitleri şirket içi önekler için el ile değişiklik yapmadan yolları öğrenebilirsiniz yönetilebilirlik kolaylaştırır.

![Transit yönlendirme senaryosu ekran görüntüsü.](./media/vpn-gateway-migration/migration2.png)

Geçiş senaryosu şirket içi bağlantı biz VNet yerel siteleri gerek kalmadan VNet bağlantısı dönüştürme olduğundan, şirket içi dolaylı olarak bağlı VNet için kaybeder. Geçiş tamamlandıktan sonra bağlantı kaybı aşağıdaki iki yolla azaltılabilir- 

* Bağlı olduğu birlikte ve şirket içi VPN ağ geçitlerinde BGP etkinleştirin. Yollar hakkında bilgi edindiniz ve sanal ağ geçitleri arasında tanıtılan beri BGP etkinleştirme bağlantısı herhangi bir yapılandırma değişikliği olmadan geri yükler. BGP seçeneği yalnızca standart ve daha yüksek SKU'larında kullanılabilir olduğunu unutmayın.
* Şirket içi konumu temsil eden yerel ağ geçidi etkilenen VNet arasında açık bir bağlantı oluşturun. Bu ayrıca oluşturmak ve IPSec tüneli yapılandırmak için şirket içi yönlendirici yapılandırmasını değiştirme gerektirir.

## <a name="next-steps"></a>Sonraki adımlar
VPN ağ geçidi geçiş desteği hakkında daha fazla bilgi sonra Git [platform desteklenen geçiş kaynakların Iaas Klasik'ten Kaynak Yöneticisi](../virtual-machines/windows/migration-classic-resource-manager-ps.md) başlamak için.

