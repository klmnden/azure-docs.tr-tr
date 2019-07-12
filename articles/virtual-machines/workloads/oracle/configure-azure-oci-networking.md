---
title: Azure ExpressRoute Oracle bulut altyapısına bağlamak | Microsoft Docs
description: Azure ExpressRoute, Bulutlar arası Oracle uygulama çözümleri etkinleştirmek için Oracle bulut altyapısı (OCI) FastConnect ile bağlanma
documentationcenter: virtual-machines
author: romitgirdhar
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/24/2019
ms.author: rogirdh
ms.openlocfilehash: 671d7c8eb9f10e346b49056e1cc117c9882bb6e8
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67707644"
---
# <a name="set-up-a-direct-interconnection-between-azure-and-oracle-cloud-infrastructure"></a>Azure ile Oracle bulut altyapısı arasında doğrudan bir bağlantısı ayarlama  

Oluşturmak için bir [tümleşik deneyim birden çok bulut](oracle-oci-overview.md) (Önizleme), Microsoft ve Oracle, Oracle bulut altyapısı (OCI) ile Azure arasındaki doğrudan bağlantısı sunar [ExpressRoute](../../../expressroute/expressroute-introduction.md) ve [ FastConnect](https://docs.cloud.oracle.com/iaas/Content/Network/Concepts/fastconnectoverview.htm). ExpressRoute ve FastConnect bağlantısı müşteriler düşük gecikme süreli, yüksek aktarım hızı, özel, iki bulut arasında doğrudan bağlantı oluşabilir.

> [!IMPORTANT]
> Microsoft Azure ile OCI arasındaki bağlantı, Önizleme aşamasında olduğundan. Azure ile OCI arasında düşük gecikme süreli bağlantı etkinleştirmek için Azure aboneliği ve OCI kiralama önce bu özellik için izin verilenler listesinde olması gerekir.

Aşağıdaki resimde, bağlantısı üst düzey bir genel bakış gösterilmektedir:

![Bulutlar arası ağ bağlantısı](media/configure-azure-oci-networking/azure-oci-connect.png)

## <a name="prerequisites"></a>Önkoşullar

* Azure ile OCI arasında bağlantı kurmak için bir etkin Azure aboneliği ve etkin bir OCI kiralama olmalıdır.

* Bağlantı yalnızca bir Azure ExpressRoute eşleme konumuna yakınlığını veya aynı konumda eşleme OCI FastConnect olduğu mümkündür. Bkz: [Önizleme sınırlamaları](oracle-oci-overview.md#preview-limitations).

* Azure aboneliğinizi bu önizleme özelliği için izin verilenler listesinde olmalıdır. Aboneliğinizde bu özelliği etkinleştirmek için Microsoft temsilcinize başvurun.

* OCI kiracınızdaki Bu önizleme özelliği için izin verilenler listesinde olmalıdır. Ayrıntılar için Oracle temsilcinize başvurun.

## <a name="configure-direct-connectivity-between-expressroute-and-fastconnect"></a>ExpressRoute ile FastConnect arasında doğrudan bağlantı yapılandırma

1. Azure aboneliğinizde bir kaynak grubu altında standart bir ExpressRoute bağlantı hattı oluşturun. 
    * ExpressRoute oluşturulurken seçin **Oracle bulut FastConnect** hizmet sağlayıcısı olarak. Bir ExpressRoute bağlantı hattı oluşturmak için bkz [adım adım Kılavuzu](../../../expressroute/expressroute-howto-circuit-portal-resource-manager.md).
    * Azure ExpressRoute devresi, 1, 2, 5 veya 10 GB/sn FastConnect destekler ancak ayrıntılı bant genişliği seçenekleri sağlar. Bu nedenle, ExpressRoute altında eşleşen bir bant genişliği seçeneklerden birini seçmeniz önerilir.

    ![ExpressRoute bağlantı hattı oluşturma](media/configure-azure-oci-networking/exr-create-new.png)
1. Not alın, ExpressRoute **hizmet anahtarı**. FastConnect devreniz yapılandırılırken anahtarınızı sağlamanız gerekir.

    ![ExpressRoute hizmet anahtarı](media/configure-azure-oci-networking/exr-service-key.png)

    > [!IMPORTANT]
    > ExpressRoute bağlantı hattının sağlandığından hemen sonra ExpressRoute ücretlerini için faturalandırılır (bile **sağlayıcısı durumu** olduğu **sağlanmadı**).

1. İki özel IP adresi alanları / her, Azure sanal ağınızla veya OCI bulut sanal ağ IP adres alanı ile çakışmaması 30 ayırması. İlk IP adresi alanını birincil adres alanı olarak ve ikinci IP adresi alanı ikincil adres alanı olarak anılacaktır. FastConnect devreniz yapılandırırken ihtiyacınız adresleri unutmayın.
1. Dinamik yönlendirme ağ geçidi (DRG) oluşturun. Bu, FastConnect devreniz oluştururken gerekir. Daha fazla bilgi için [dinamik yönlendirme ağ geçidi](https://docs.cloud.oracle.com/iaas/Content/Network/Tasks/managingDRGs.htm) belgeleri.
1. Oracle kiracınız altında FastConnect bağlantı hattı oluşturun. Daha fazla bilgi için [Oracle belgeleri](https://docs.cloud.oracle.com/iaas/Content/Network/Concepts/azure.htm).
  
    * FastConnect yapılandırmada seçin **Microsoft Azure: ExpressRoute** sağlayıcısı.
    * Önceki adımda dinamik yönlendirme, sağlanan ağ geçidi seçin.
    * Sağlanacak bant genişliği'ni seçin. En iyi performans için bant genişliği, ExpressRoute bağlantı hattını oluştururken, seçilen bant genişliği eşleşmesi gerekir.
    * İçinde **sağlayıcısı hizmet anahtarı**, ExpressRoute hizmet anahtarını yapıştırın.
    * Kullanmak için önceki bir adımda özel IP adres alanı gerekmez birinci/30 **birincil BGP IP adresi** ve ikinci/30 için özel IP adresi alanını **ikincil BGP IP'sini** adresi.
        * İki aralığın ilk kullanım adresini Oracle BGP IP adresi (birincil ve ikincil) ve ikinci adresini (FastConnect açısından bakıldığında) müşterinin BGP IP adresi atayın. İlk kullanılabilir IP adresi olan ikinci IP adresini/30 adres alanı (Microsoft tarafından ilk IP adresi ayrılmış).
    *           **Oluştur**'a tıklayın.
1. Bulut sanal ağ dinamik yönlendirme rota tablosunu kullanarak ağ geçidi üzerinden, Oracle kiracınız altında FastConnect bağlanma tamamlayın.
1. Azure'a gidin ve emin **sağlayıcısı durumu** için ExpressRoute bağlantı hattı için değişti **sağlanan** ve türünde bir eşlemeye **Azure özel** kaldırıldı sağlanan. Aşağıdaki adımlar için önkoşul budur.

    ![ExpressRoute sağlayıcısı durumu](media/configure-azure-oci-networking/exr-provider-status.png)
1. Tıklayarak **Azure özel** eşlemesi. Eşleme ayrıntıları FastConnect devreniz ayarlarken girilen bilgilere göre otomatik olarak yapılandırıldı görürsünüz.

    ![Özel eşleme ayarları](media/configure-azure-oci-networking/exr-private-peering.png)

## <a name="connect-virtual-network-to-expressroute"></a>ExpressRoute için sanal ağınıza bağlama

1. Henüz yapmadıysanız, bir sanal ağ ve sanal ağ geçidi oluşturun. Ayrıntılar için bkz [adım adım Kılavuzu](../../../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md).
1. Yürüterek sanal ağ geçidi ve ExpressRoute bağlantı hattı arasında bağlantı kurma [Terraform betik](https://github.com/microsoft/azure-oracle/tree/master/InterConnect-2) veya için PowerShell komutunu yürüterek [yapılandırın, ExpressRoute FastPath](../../../expressroute/expressroute-howto-linkvnet-arm.md#configure-expressroute-fastpath).

Ağ yapılandırmasını tamamladıktan sonra tıklayarak yapılandırmanızı geçerliliğini doğrulayabilirsiniz **ARP kayıtlarını alma** ve **Get yol tablosu** ExpressRoute özel eşlemesi dikey pencerede altında Azure portalı.

## <a name="automation"></a>Otomasyon

Microsoft, ağ bağlantı, otomatik dağıtımı etkinleştirmek için Terraform betikleri oluşturdu. Bunlar Azure aboneliği üzerinde yeterli izinleri gerektirdiğinden Terraform betikleri Azure ile çalışmaya başlamadan önce kimlik doğrulamanız gerekir. Kimlik doğrulama kullanılarak yapılabilir bir [Azure Active Directory Hizmet sorumlusu](../../../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) veya Azure CLI kullanarak. Daha fazla bilgi için [Terraform belgeleri](https://www.terraform.io/docs/providers/azurerm/auth/azure_cli.html).

Terraform betikleri ve ilgili belgeler inter-connect dağıtmak için bu bulunabilir [GitHub deposu](https://aka.ms/azureociinterconnecttf).

## <a name="monitoring"></a>İzleme

Her iki bulut üzerinde aracıları yükleme, Azure yararlanabilir [Ağ Performansı İzleyicisi'ni (NPM)](../../../expressroute/how-to-npm.md) uçtan uca ağ performansını izlemek için. NPM ağ sorunları kolayca belirlemenize yardımcı olabilir ve bunları ortadan kaldırılmasına yardımcı olur.

## <a name="delete-the-interconnect-link"></a>Bağlantı bağlantıyı silme

Bağlantı silmek için aşağıdaki adımları, verilen belirli sırada gelmelidir. Bunun yapılmaması, bir "başarısız durumu" ExpressRoute bağlantı hattı neden olur.

1. ExpressRoute bağlantısını silin. Bağlantıyı tıklatarak silmek **Sil** sayfasında, bağlantınız için bir simge. Daha fazla bilgi için [ExpressRoute belgeleri](../../../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md#delete-a-connection-to-unlink-a-vnet).
1. Oracle FastConnect Oracle bulut konsolundan silin.
1. Oracle FastConnect devre silindikten sonra Azure ExpressRoute bağlantı hattı silebilirsiniz.

Bu noktada, silme ve sağlamayı kaldırma işlemi tamamlanmıştır.

## <a name="next-steps"></a>Sonraki adımlar

* OCI ve Azure arasında Bulutlar arası bağlantı hakkında daha fazla bilgi için bkz: [Oracle belgeleri](https://docs.cloud.oracle.com/iaas/Content/Network/Concepts/azure.htm).
* Kullanım [Terraform betikleri](https://aka.ms/azureociinterconnecttf) hedeflenen Oracle uygulamalar için altyapının Azure üzerinde dağıtın ve ağ bağlantısı yapılandırmak için. 