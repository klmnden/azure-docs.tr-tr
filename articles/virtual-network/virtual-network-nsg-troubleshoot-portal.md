---
title: Ağ güvenlik grupları - Portal sorunlarını giderme | Microsoft Docs
description: Ağ güvenlik grupları Azure Portalı'nı kullanarak Azure Resource Manager dağıtım modelinde sorun giderme öğrenin.
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: ''
tags: azure-resource-manager
ms.assetid: a54feccf-0123-4e49-a743-eb8d0bdd1ebc
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
ms.openlocfilehash: 67ffe826ba13576578e8f09e36f84128f4ceb0f2
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="troubleshoot-network-security-groups-using-the-azure-portal"></a>Ağ güvenlik grupları Azure Portalı'nı kullanarak sorun giderme
> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-nsg-troubleshoot-portal.md)
> * [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)
> 
> 

Ağ güvenlik grupları (Nsg'ler), sanal makine (VM) üzerinde yapılandırılmış ve VM bağlantı sorunları yaşıyorsanız, bu makalede daha fazla gidermek Nsg'ler tanılama özelliklerine genel bakış sağlar.

Nsg'ler sanal makineleri (VM'ler) ve akan trafik türlerini denetlemenize sağlar. Nsg'ler alt ağlara bir Azure sanal ağı (VNet), ağ arabirimleri (NIC) ya da her ikisini de uygulanabilir. Bir NIC uygulanan etkili bir NIC'ye uygulanan Nsg'ler mevcut kurallar ve bağlı olduğu alt ağ bir toplama kurallardır. Bu Nsg'ler arasında kuralları bazen birbiriyle çelişen ve bir sanal makinenin ağ bağlantısını etkileyebilir.

VM Nıc'lerde uygulanan olarak, tüm etkin güvenlik kuralları Nsg'lerinizi görüntüleyebilirsiniz. Bu makalede, bu kurallar Azure Resource Manager dağıtım modelinde kullanarak VM bağlantı sorunlarını gidermek gösterilmiştir. VNet ve NSG kavramlarına alışık değilseniz bkz [sanal ağa genel bakış](virtual-networks-overview.md) ve [ağ güvenlik grubu genel bakış](security-overview.md).

## <a name="using-effective-security-rules-to-troubleshoot-vm-traffic-flow"></a>VM trafik akışı sorun giderme için etkili güvenlik kurallarını kullanma
Aşağıdaki senaryoda, ortak bir bağlantı sorunu örneğidir:

Adlı bir VM'den *VM1* adlı bir alt ağın parçası olan *Subnet1* adlı bir sanal ağ içindeki *WestUS VNet1*. 3389 numaralı TCP bağlantı noktası üzerinden RDP kullanarak VM bağlanma denemesi başarısız olur. Nsg'ler, her iki NIC üzerinde uygulanır *VM1 nıc1* ve alt ağ *Subnet1*. TCP bağlantı noktası 3389 trafiği ağ arabirimiyle ilişkilendirilmiş NSG izin verilen *VM1 nıc1*, kullanıcının VM1 ping TCP bağlantı noktası ancak 3389 başarısız olur.

Bu örnek 3389 numaralı TCP bağlantı noktasını kullanır, ancak herhangi bir bağlantı noktası gelen ve giden bağlantı hataları belirlemek için aşağıdaki adımları kullanılabilir.

### <a name="vm"></a>Bir sanal makine için etkili güvenlik kurallarını görüntüle
Nsg'leri bir VM için sorun giderme için aşağıdaki adımları tamamlayın:

VM'den bir NIC üzerinde etkili güvenlik kuralları tam listesini görüntüleyebilirsiniz. Eklemek, değiştirmek ve bu işlemleri gerçekleştirmek için izinleri varsa NIC ve alt ağ NSG kuralları etkili kuralları dikey penceresinden silin.

1. Azure portalında oturum açma https://portal.azure.com ile bir Azure hesabı. Hesabınızı atanmalıdır *Microsoft.Network/networkInterfaces/effectiveNetworkSecurityGroups/action* ağ arabirimi için işlemi. Operations hesaplara atamak üzere öğrenmek için bkz: [Azure rol tabanlı erişim denetimi için özel roller oluşturmanızı](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. ' I tıklatın **tüm hizmetleri**, ardından **sanal makineler** listesinde görünür.
3. Görüntülenen listesinden gidermek için VM seçin ve seçeneklerle bir VM dikey penceresi görünür.
4. Tıklatın **Tanıla & sorunları** ve ortak bir sorun seçin. Bu örnek için **Windows VM'ime bağlanamıyorum** seçilir. 
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image1.png)
5. Adımlar sorunu altında aşağıdaki resimde gösterildiği gibi görünür: 
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image2.png)
   
    Tıklatın *etkin güvenlik grubu kuralları* önerilen adımları listesinde.
6. **Alma etkin güvenlik kuralları** dikey penceresi görünür, aşağıdaki resimde gösterildiği gibi:
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image3.png)
   
    Aşağıdaki bölümlerde resmin dikkat edin:
   
   * **Kapsam:** kümesine *VM1*, 3. adımda seçtiğiniz VM.
   * **Ağ arabirimi:** *VM1 nıc1* seçilir. Bir VM birden çok ağ arabirimine (NIC) sahip. Her NIC'nin benzersiz etkin güvenlik kuralları olabilir. Sorunlarını giderirken, her bir NIC için etkili güvenlik kuralları görüntülemek gerekebilir
   * **İlişkili Nsg'ler:** Nsg'ler, NIC ve NIC bağlı alt ağ için uygulanabilir. Aşağıdaki resimde, NIC ve bağlı olduğu alt ağ için bir NSG uygulanmıştır. Doğrudan Nsg'ler kurallarında değiştirmek için NSG adları tıklatabilirsiniz.
   * **VM1 nsg sekmesi:** NSG, NIC'ye uygulanır için resim görüntülenen kuralların listesidir Her bir NSG oluşturulduğunda birkaç varsayılan kuralları Azure tarafından oluşturulur. Varsayılan kuralları kaldırılamıyor, ancak daha yüksek öncelik kuralları ile geçersiz kılınabilir. [Varsayılan güvenlik kuralları](security-overview.md#default-security-rules) hakkında daha fazla bilgi edinin.
   * **HEDEF sütun:** başkalarının adres öneklerini bulunurken metin sütununda bazı kurallar vardır. Güvenlik kuralı oluşturulduğunda uygulanan varsayılan etiketleri adını metindir. Etiketler birden çok önekleri temsil eden sistem tarafından sağlanan tanımlayıcılardır. Bir etiketi olan bir kural gibi seçerek *AllowInternetOutBound*, öneklerini listeler **adres önekleri** dikey.
   * **İndirin:** kurallar listesinin uzun olabilir. Kuralları çevrimdışı analiz için bir .csv dosyası tıklayarak indirebileceğiniz **karşıdan** ve dosyayı kaydetme.
   * **AllowRDP** gelen kuralı: Bu kural, VM RDP bağlantılara izin verir.
7. Tıklatın **Subnet1 NSG** NSG etkili kurallardan görüntülemek için aşağıdaki resimde gösterildiği gibi alt ağa uygulanan: 
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image4.png)
   
    Bildirim *denyRDP* **gelen** kuralı. Gelen kuralları alt ağa uygulanan ağ arabiriminin uygulanan kurallardan önce değerlendirilir. Reddetme kuralı alt uygulanan olduğundan, NIC izin ver kuralı hiçbir zaman değerlendirilen çünkü TCP 3389 bağlanma isteği başarısız olur. 
   
    *DenyRDP* kuralı neden RDP bağlantısı başarısız neden olur. Kaldırarak sorunu çözecektir.
   
   > [!NOTE]
   > Nsg'ler NIC veya alt ağa uygulanmamış veya NIC ile ilişkili VM çalışır durumda değil, hiçbir kural gösterilir.
   > 
   > 
8. NSG kuralları düzenlemek için tıklatın *Subnet1 NSG* içinde **ilişkili Nsg'ler** bölümü.
   Bu açılır **Subnet1 NSG** dikey. Tıklayarak kuralları doğrudan düzenleyebilirsiniz **gelen güvenlik kuralları**.
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image7.png)
9. Kaldırdıktan sonra *denyRDP* gelen kuralı **Subnet1 NSG** ve ekleyerek bir *allowRDP* kural etkili kuralları aşağıdaki resimde listesi görülüyor:
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image8.png)
   
    TCP bağlantı noktası 3389 VM için RDP bağlantısı açarak veya PsPing aracını kullanarak açık olduğundan emin olun. Okuyarak PsPing hakkında daha fazla bilgiyi [PsPing indirme sayfası](https://technet.microsoft.com/sysinternals/psping.aspx).

### <a name="nic"></a>Bir ağ arabirimi için etkili güvenlik kurallarını görüntüle
VM trafik akışı için belirli bir NIC etkilenir, aşağıdaki adımları tamamlayarak ağ arabirimleri bağlamından NIC için tam etkili kuralları listesini görüntüleyebilirsiniz:

1. Azure portalında oturum açma https://portal.azure.com.
2. Tıklatın **tüm hizmetleri**, ardından **ağ arabirimleri** listesinde görünür.
3. Bir ağ arabirimi seçin. Aşağıdaki resimde, bir NIC adlı *VM1 nıc1* seçilir.
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image5.png)
   
    Dikkat **kapsam** seçili ağ arabirimi için ayarlanır. Gösterilen ek bilgiler hakkında daha fazla bilgi için 6. adımını okuma **sorun giderme Nsg'leri bir VM için** bu makalenin.
   
   > [!NOTE]
   > Bir NSG'yi bir ağ arabiriminden kaldırılırsa, alt ağ NSG verilen NIC üzerinde hala etkin Bu durumda, çıktı yalnızca alt NSG kuralları gösterir. Bir VM'ye NIC bağlıysa kurallarını yalnızca görünür.
   > 
   > 
4. Kuralları, bir NIC ve bir alt ağ ile ilişkili Nsg'ler için doğrudan düzenleyebilirsiniz. Bilgi edinmek için nasıl adım 8 / okuma **bir sanal makine için etkili güvenlik kuralları** bu makalenin.

## <a name="nsg"></a>Bir ağ güvenlik grubu (NSG) için etkili güvenlik kuralları görüntülemek
NSG kuralları değiştirirken, belirli bir VM'de eklenen kurallar etkisini gözden geçirmek isteyebilirsiniz. Belirli bir NSG uygulandığı tüm NIC'ler için etkili güvenlik kuralları tam bir listesini görüntüleyebilirsiniz verilen NSG dikey penceresinden bağlamı değiştirmek zorunda kalmadan. Bir NSG içinde etkili kuralları gidermek için aşağıdaki adımları tamamlayın:

1. Azure portalında oturum açma https://portal.azure.com.
2. Tıklatın **tüm hizmetleri**, ardından **ağ güvenlik grupları** listesinde görünür.
3. Bir NSG'yi seçin. Aşağıdaki resimde, VM1 nsg adlı bir NSG seçilmedi.
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image6.png)
   
    Önceki resimde aşağıdaki bölümlerini dikkat edin:
   
   * **Kapsam:** seçili NSG ayarlayın.
   * **Sanal makine:** olduğunda bir NSG bir alt ağa uygulanan, alt ağına bağlı tüm sanal makineleri bağlı tüm ağ arabirimleri için uygulanır. Bu liste, bu NSG uygulandığı tüm sanal makineleri gösterir. Tüm VM listeden seçebilirsiniz.
     
     > [!NOTE]
     > Yalnızca boş bir alt ağa bir NSG uyguladıysanız, VM'ler listelenmez. Bir NSG'yi bir VM ile ilişkili olmayan bir NIC uygulanırsa, bu NIC'ler da listelenmez. 
     > 
     > 
   * **Ağ arabirimi:** VM birden çok ağ arabirimine sahip. Seçili VM'ye bağlı bir ağ arabirimi seçebilirsiniz.
   * **AssociatedNSGs:** herhangi bir zamanda bir NIC bir NIC ve diğer alt ağa uygulanan en fazla iki etkili Nsg'ler sahip olabilir. NIC etkin bir alt ağ NSG varsa kapsamı VM1-nsg seçilen rağmen çıktı hem Nsg'ler gösterir.
4. Kuralları, bir NIC veya alt ağ ile ilişkili Nsg'ler için doğrudan düzenleyebilirsiniz. Bilgi edinmek için nasıl adım 8 / okuma **bir sanal makine için etkili güvenlik kuralları** bu makalenin.

Gösterilen ek bilgiler hakkında daha fazla bilgi için 6. adımını okuma **bir sanal makine için etkili güvenlik kuralları** bu makalenin.

> [!NOTE]
> Bir alt ağ ve NIC her yalnızca bir NSG uygulanmış kurabiliyor olsa bile, bir NSG birden çok NIC ve birden çok alt ağları ile ilişkili olabilir.
> 
> 

## <a name="considerations"></a>Dikkat edilmesi gerekenler
Bağlantı sorunlarını giderme yaparken aşağıdaki noktaları dikkate alın:

* Varsayılan NSG kuralları internet'ten gelen erişimi engellemek ve yalnızca VNet izin gelen trafiği. Internet'ten gelen erişim gerektiğinde izin vermek için açıkça kuralları eklenmesi gerekir.
* Bir sanal makinenin ağ bağlantısı başarısız olmasına neden olan hiçbir NSG güvenlik kuralları varsa, sorunu nedeniyle olabilir:
  * Sanal makinenin işletim sistemi içinde çalışan güvenlik duvarı yazılımı
  * Sanal gereçler veya şirket içi trafiği için yapılandırılmış yollar. Internet trafiği, şirket içi zorlamalı tünel aracılığıyla yönlendirilebilir. VM Internet'ten bir RDP/SSH bağlantısı nasıl şirket içi ağ donanımı bu trafiği işler bağlı olarak bu ayar ile çalışmayabilir. Okuma [sorun giderme yolları](virtual-network-routes-troubleshoot-powershell.md) makale içeri ve dışarı VM trafik akışını engelleyen rota sorunları tanılamak öğrenin. 
* Varsayılan olarak sanal ağlar, eşlenen, VIRTUAL_NETWORK etiketine önekler için eşlenmiş Vnet'lerde dahil etmek için otomatik olarak genişletilir. Bu önekleri görüntüleyebilirsiniz **ExpandedAddressPrefix** VNet eşleme bağlantısı ilgili sorunları gidermek için liste. 
* Etkin güvenlik kuralları yalnızca VM NIC ve veya alt ağ ilişkilendirilmiş bir NSG olup olmadığını gösterilir. 
* Alt ağ ve NIC ile ilişkili hiçbir Nsg'ler vardır veya VM'nize atanan genel bir IP adresi varsa, tüm bağlantı noktalarına gelen ve giden erişimi açık olacaktır. VM bir ortak IP adresi varsa, NIC veya alt ağ Nsg'ler uygulanması önerilir.

