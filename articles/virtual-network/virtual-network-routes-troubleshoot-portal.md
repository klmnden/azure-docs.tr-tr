---
title: "Yollar - Portal sorunlarını giderme | Microsoft Docs"
description: "Azure Portalı'nı kullanarak Azure Resource Manager dağıtım modelinde yollar giderileceğini öğrenin."
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: bdd8b6dc-02fb-4057-bb23-8289caa9de89
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
ms.openlocfilehash: e0b835f4cbad9855bfb7ddccf2d9bf5b4bf88231
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2018
---
# <a name="troubleshoot-routes-using-the-azure-portal"></a>Azure Portalı'nı kullanarak yolları sorun giderme
> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-routes-troubleshoot-portal.md)
> * [PowerShell](virtual-network-routes-troubleshoot-powershell.md)
>
>

Ağ bağlantısı sorunları için veya, Azure sanal makine'nden (VM) karşılaşıyorsanız, yolları VM trafik akışına etkiliyor olabilir. Bu makalede daha fazla gidermek rotalar için tanılama özelliklerine genel bakış sağlar.

Yönlendirme tabloları alt ağ ile ilişkili ve tüm ağ arabirimlerindeki (NIC) bu alt ağdaki etkili olur. Aşağıdaki türde yollar her bir ağ arabirimine uygulanabilir:

* **Sistem yolları:** varsayılan olarak, yerel VNet trafiği, VPN ağ geçitleri üzerinden şirket içi trafik ve Internet trafiğini izin sistem yönlendirme tabloları bir Azure sanal ağı (VNet) içinde oluşturulan her alt ağ vardır. Sistem yolları da eşlenmiş Vnet'ler için mevcut.
* **BGP yolları:** ExpressRoute veya siteden siteye VPN bağlantıları üzerinden ağ arabirimlerine yayılır. Okuyarak BGP yönlendirme hakkında daha fazla bilgi [VPN gateways ile BGP'ye](../vpn-gateway/vpn-gateway-bgp-overview.md) ve [ExpressRoute genel bakış](../expressroute/expressroute-introduction.md) makaleleri.
* **Kullanıcı tanımlı yolları (UDR):** ağ sanal Gereçleri veya olan kullanıyorsanız zorlamalı tünel trafiği için bir şirket içi ağ bir siteden siteye VPN aracılığıyla alt yol tablosu ile ilişkili kullanıcı tanımlı yollar (Udr'ler) olabilir. İle Udr'ler bilmiyorsanız okuma [kullanıcı tanımlı yollar](virtual-networks-udr-overview.md#user-defined) makalesi.

Bir ağ arabirimi için uygulanabilecek çeşitli yollar, hangi toplama yolların etkili belirlemek zor olabilir. VM ağ bağlantısı gidermenize yardımcı olması için Azure Resource Manager dağıtım modelinde bir ağ arabirimi için tüm etkin yollar görüntüleyebilirsiniz.

## <a name="using-effective-routes-to-troubleshoot-vm-traffic-flow"></a>VM trafik akışı gidermek için etkili yolları kullanma
Bu makalede, bir ağ arabirimi için etkili rotaları ile ilgili sorunları giderme göstermek için örnek olarak aşağıdaki senaryoyu kullanır:

Bir VM'yi (*VM1*) Vnet'e bağlı (*VNet1*, öneki: 10.9.0.0/16) VM(VM3) yeni vnet'teki bağlanamıyor (*VNet3*, 10.10.0.0/16 öneki). VM'ye bağlı VM1 nıc1 ağ arabirimine uygulanan Udr'ler ya da BGP yolları, yalnızca sistem yolları uygulanır vardır.

Bu makalede, Azure kaynak yönetimi dağıtım modelinde etkili yollar özelliği kullanarak bağlantı hatanın nedenini belirlemek açıklanmaktadır.
Bu örnek yalnızca sistem yolları kullanır, ancak aynı adımlar herhangi bir rota türü gelen ve giden bağlantı hataları belirlemek için kullanılabilir.

> [!NOTE]
> VM bağlı birden çok NIC varsa, her bir VM gelen ve giden ağ bağlantısı sorunları tanılamak için NIC etkili yolları denetleyin.
>
>

### <a name="view-effective-routes-for-a-virtual-machine"></a>Bir sanal makine için Görünüm etkili yolları
Bir VM'ye uygulanan toplama rotaları görmek için aşağıdaki adımları tamamlayın:

1. Azure portalında oturum açma https://portal.azure.com. Hesabınızı atanmalıdır *Microsoft.Network/networkInterfaces/effectiveRouteTable/action* ağ arabirimi için işlemi. Operations hesaplara atamak üzere öğrenmek için bkz: [Azure rol tabanlı erişim denetimi için özel roller oluşturmanızı](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#actions).
2. ' I tıklatın **tüm hizmetleri**, ardından **sanal makineler** listesinde görünür.
3. Görüntülenen listesinden gidermek için VM seçin ve seçeneklerle bir VM dikey penceresi görünür.
4. Tıklatın **Tanıla & sorunları** ve ortak bir sorun seçin. Bu örnek için **Windows VM'ime bağlanamıyorum** seçilir.

    ![](./media/virtual-network-routes-troubleshoot-portal/image1.png)
5. Adımlar sorunu altında aşağıdaki resimde gösterildiği gibi görünür:

    ![](./media/virtual-network-routes-troubleshoot-portal/image2.png)

    Tıklatın *etkili yolları* önerilen adımları listesinde.
6. **Etkili yolları** dikey penceresi görünür, aşağıdaki resimde gösterildiği gibi:

    ![](./media/virtual-network-routes-troubleshoot-portal/image3.png)

    Tek bir NIC VM varsa, varsayılan olarak seçilidir. Birden çok NIC varsa, etkili yollarını görüntülemek istediğiniz NIC seçin.

   > [!NOTE]
   > NIC ile ilişkili VM çalışır durumda değilse, etkili yolları gösterilmez. Yalnızca ilk 200 etkili yolların portalda gösterilir. Tam liste için tıklatın **karşıdan**. Daha fazla indirilen .csv dosyasından sonuçları filtreleyebilirsiniz.
   >
   >

    Çıktı aşağıdakilere dikkat edin:

   * **Kaynak**: yol türünü belirtir. Sistem yolları olarak gösterilir *varsayılan*, Udr'ler olarak gösterilir *kullanıcı* ve ağ geçidi yollarını (statik ya da BGP) olarak gösterilen *VPNGateway*.
   * **Durum**: etkin yönlendirme durumunu gösterir. Olası değerler şunlardır: *etkin* veya *geçersiz*.
   * **AddressPrefixes**: CIDR gösteriminde etkin yönlendirme adresi öneki belirtir.
   * **nextHopType**: belirli bir rota için sonraki atlama gösterir. Olası değerler şunlardır: *değerinin VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*, veya *Null*. Değerini *Null* için **nextHopType** bir UDR geçersiz bir yol gösteriyor olabilir. Örneğin, varsa **nextHopType** olan *değerinin VirtualAppliance* ve ağ sanal gereç VM sağlanan çalışıyor durumda değil. Varsa **nextHopType** olan *VPNGateway* ve sağlanan/çalışan verilen VNet içinde yol geçersiz hale ağ geçidi yok.
7. Listelenen yol yok *WestUS VNET3* (önek 10.10.0.0/16) ağdan *WestUS VNet1* (10.9.0.0/16 önek) önceki adımda resim. Aşağıdaki resimde, eşleme bağlantısı bulunduğu *bağlantı kesildi* durumu:

    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)

    Çift yönlü bağlantı eşlemeyi bozuk için açıklayan neden VM1 içinde VM3 bağlanılamadı *WestUS VNet3* VNet.
8. Aşağıdaki resimde, çift yönlü eşleme bağlantı kurulduktan sonra yolları gösterir:

    ![](./media/virtual-network-routes-troubleshoot-portal/image5.png)

Zorlamalı tünel ve rota değerlendirme için daha fazla sorun giderme senaryoları için okuma [konuları](virtual-network-routes-troubleshoot-portal.md#considerations) bu makalenin.

### <a name="view-effective-routes-for-a-network-interface"></a>Bir ağ arabirimi için Görünüm etkili yolları
Ağ trafiği akışını belirli bir ağ arabirimi (NIC) için etkilenen varsa etkili yolların tam bir listesi NIC üzerinde doğrudan görüntüleyebilirsiniz. Bir NIC'ye uygulanır toplama rotaları görmek için aşağıdaki adımları tamamlayın:

1. Azure portalında oturum açma https://portal.azure.com.
2. Tıklatın **tüm hizmetleri**, ardından **ağ arabirimleri**
3. Bir NIC adı için listede arama veya görüntülenen listesinden seçin. Bu örnekte, **VM1 nıc1** seçilir.
4. Seçin **etkili yolları** içinde **ağ arabirimi** dikey penceresinde, aşağıdaki resimde gösterildiği gibi:

       ![](./media/virtual-network-routes-troubleshoot-portal/image6.png)

    **Kapsam** varsayılan olarak seçilen ağ arabirimi.

      ![](./media/virtual-network-routes-troubleshoot-portal/image7.png)

### <a name="view-effective-routes-for-a-route-table"></a>Görünüm etkili yollar için bir yol tablosu
Kullanıcı tanımlı yollar (Udr'ler) rota tablosunda değiştirirken, belirli bir VM'de eklenmekte olan yollar etkisini gözden geçirmek isteyebilirsiniz. Bir yol tablosu alt ağlar herhangi bir sayı ile ilişkili olabilir. Verilen yol tablosu uygulandığı tüm NIC'ler için tüm etkin yollar artık görüntüleyebilirsiniz belirli bir rota tablosu dikey penceresinden bağlamı değiştirmek zorunda kalmadan.

Bu örneğin, bir UDR (*UDRoute*) rota tablosunda belirtilen (*UDRouteTable*). Bu rotanın gelen tüm Internet trafiğini gönderir *Subnet1* içinde *WestUS VNet1* ağ sanal gereç (NVA) aracılığıyla VNet içinde *Altağ2* aynı vnet'in. Yol aşağıdaki resimde gösterilir:

![](./media/virtual-network-routes-troubleshoot-portal/image8.png)

Bir yol tablosu için birleşik rota görmek için aşağıdaki adımları tamamlayın:

1. Azure portalında oturum açma https://portal.azure.com.
2. Tıklatın **tüm hizmetleri**, ardından **yol tablosu**
3. Birleşik yollar için bakın ve seçmek istediğiniz yol tablosu için listesi arayın. Bu örnekte, **UDRouteTable** seçilir. Dikey penceresinde seçili rota tablosu için aşağıdaki resimde gösterildiği gibi görünür:

    ![](./media/virtual-network-routes-troubleshoot-portal/image9.png)
4. Seçin **etkili yolları** içinde **yol tablosu** dikey. **Kapsam** seçtiğiniz yol tablosu ayarlanır.
5. Bir yol tablosu birden çok alt ağlara uygulanabilir. Seçin **alt** listeden gözden geçirmek istediğiniz. Bu örnekte, **Subnet1** seçilir.
6. Seçin bir **ağ arabirimi**. Seçilen alt ağa bağlı olan tüm NIC'ler listelenir. Bu örnekte, **VM1 nıc1** seçilir.

    ![](./media/virtual-network-routes-troubleshoot-portal/image10.png)

   > [!NOTE]
   > NIC çalışan bir VM ile ilişkili değilse, etkili bir yol gösterilir.
   >
   >

## <a name="considerations"></a>Dikkat edilmesi gerekenler
Yolların listesini gözden geçirirken göz önünde bulundurmanız gereken birkaç şey döndürdü:

* Yönlendirme üzerinde en uzun ön ek eşleşmesi (LPM) Udr'ler arasında temel BGP ve sistem yolları. Aynı LPM eşleşmesine sahip birden fazla yol varsa, bir yol aşağıdaki sırayla ve kaynağına göre seçilir:

  * Kullanıcı tanımlı yol
  * BGP yolu
  * Sistem (varsayılan) yolu

    Etkin yollar ile tüm kullanılabilir yollara dayalı LPM eşleşmesine olan etkili yollar yalnızca görebilirsiniz. Nasıl yollar için belirli bir NIC gerçekte değerlendirildiği göstererek Bu, çok bağlantı denetleyicisinden VM etkileyen belirli yollar gidermek kolaylaştırır.
* Udr'ler varsa ve trafiği bir ağ sanal gereç (NVA) ile göndermeye *değerinin VirtualAppliance* olarak **nextHopType**, IP iletimini trafiğini alma NVA üzerinde etkinleştirildiğinden emin olun veya paket bırakılır.
* Zorlamalı tünel etkinse, tüm giden Internet trafiği şirket içi yönlendirilir. Şirket içi bu trafiği nasıl işler bağlı olarak bu ayar, VM için RDP/SSH Internet'ten çalışmayabilir.
  Zorlamalı tünel etkinleştirilebilir:
  * Siteden siteye VPN, VPN ağ geçidi olarak nextHopType ile bir kullanıcı tanımlı yönlendirme (UDR) ayarlayarak kullanıyorsanız
  * Varsayılan rota BGP Tanıtımı
* Bir sistem rotası doğru çalışması VNet eşleme trafiği için **nextHopType** *VNetPeering* eşlenmiş sanal ağınızın önek aralığının mevcut olması gerekir. Böyle bir yol yok ve VNet eşleme bağlantısı sorunsuz görünüyor varsa:
  * Birkaç saniye bekleyin ve yeni oluşturulmuş bir eşleme bağlantısı ise, yeniden deneyin. Bazen, bir alt ağdaki tüm ağ arabirimleri yolları yaymak için daha uzun sürer.
  * Ağ güvenlik grubu (NSG) kuralları, trafik akışlarının etkiliyor olabilir. Daha fazla bilgi için bkz: [sorun giderme ağ güvenlik grupları](virtual-network-nsg-troubleshoot-portal.md) makalesi.
