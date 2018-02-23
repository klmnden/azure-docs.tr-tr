---
title: "SQL Server kullanılabilirlik gruplarını - Azure sanal makineleri - olağanüstü durum kurtarma | Microsoft Docs"
description: "Bu makalede, Azure sanal makinelerinde farklı bir bölgede bir çoğaltma ile bir SQL Server kullanılabilirlik grubu yapılandırma açıklanmaktadır."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: craigg
editor: monicar
tags: azure-service-management
ms.assetid: 388c464e-a16e-4c9d-a0d5-bb7cf5974689
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/02/2017
ms.author: mikeray
ms.openlocfilehash: 2a954ca10bdec3343dbd8796b50053a1c8c40ff4
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="configure-an-always-on-availability-group-on-azure-virtual-machines-in-different-regions"></a>Azure sanal makinelerinde farklı bölgelerdeki Always On kullanılabilirlik grubu yapılandırma

Bu makalede, Azure sanal makinelerinde Azure uzak bir konumdaki bir SQL Server Always On kullanılabilirlik grubu çoğaltması yapılandırma açıklanmaktadır. Olağanüstü durum kurtarma desteklemek üzere bu yapılandırmayı kullanır.

Bu makale, Resource Manager moduna Azure sanal makineleri için geçerlidir.

Aşağıdaki resim Azure sanal makinelerde ortak bir dağıtım bir kullanılabilirlik grubunun gösterir:

   ![Kullanılabilirlik grubu](./media/virtual-machines-windows-portal-sql-availability-group-dr/00-availability-group-basic.png)

Bu dağıtımda, tüm sanal makineleri bir Azure bölgesinde ' dir. Kullanılabilirlik grubu çoğaltmalarının zaman uyumlu işleme SQL-1 ve 2 SQL otomatik yük devretme ile olabilir. Bu mimari oluşturmak için bkz: [kullanılabilirlik grubu şablon veya öğretici](virtual-machines-windows-portal-sql-availability-group-overview.md).

Azure bölgesinde erişilemez hale gelirse, bu mimarisi için kapalı kalma süresi savunmasızdır. Bu güvenlik açığından üstesinden gelmek için farklı bir Azure bölgesinde bir çoğaltma ekleyin. Aşağıdaki diyagramda, yeni mimarisinin nasıl görüneceği gösterilmektedir:

   ![Kullanılabilirlik grubu DR](./media/virtual-machines-windows-portal-sql-availability-group-dr/00-availability-group-basic-dr.png)

Önceki diyagramda SQL 3 adlı yeni bir sanal makine gösterir. SQL-3 farklı bir Azure bölgesinde kullanılır. SQL 3, Windows Server Yük devretme kümesine eklenir. SQL 3 bir kullanılabilirlik grubu çoğaltması barındırabilir. Son olarak, Azure bölgesi SQL 3 için yeni bir Azure yük dengeleyici olduğuna dikkat edin.

>[!NOTE]
> Birden fazla sanal makine aynı bölgede bir Azure kullanılabilirlik kümesi gerekli olur. Bir sanal makine bölgede yalnızca, kullanılabilirlik kümesi gerekli değildir. Yalnızca bir sanal makine kullanılabilirlik kümesi oluşturma zamanında yerleştirebilirsiniz. Sanal makine zaten bir kullanılabilirlik kümesine ise, daha sonra ek bir çoğaltma için bir sanal makine ekleyebilirsiniz.

Bu mimaride, uzak bölgede çoğaltma normalde zaman uyumsuz tamamlama kullanılabilirlik modu ve el ile yük devretme modu ile yapılandırılır.

Kullanılabilirlik grubu çoğaltmalarının Azure sanal makinelerinde farklı Azure bölgelerinde olduğunda, her bölge gerektirir:

* Bir sanal ağ geçidi
* Sanal ağ geçidi bağlantısı

Aşağıdaki diyagramda, ağları veri merkezleri arasında nasıl iletişim kurduğunu gösterir.

   ![Kullanılabilirlik grubu](./media/virtual-machines-windows-portal-sql-availability-group-dr/01-vpngateway-example.png)

>[!IMPORTANT]
>Bu mimarinin Azure bölgeler arasında çoğaltılan veriler için giden veri ücret doğurur. Bkz: [bant genişliği fiyatlandırma](http://azure.microsoft.com/pricing/details/bandwidth/).  

## <a name="create-remote-replica"></a>Uzak bir çoğaltma oluşturma

Bir uzak veri merkezinde bir çoğaltma oluşturmak için aşağıdaki adımları uygulayın:

1. [Yeni bölgede bir sanal ağ oluşturma](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

1. [Azure portalını kullanarak VNet-VNet bağlantı yapılandırma](../../../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).

   >[!NOTE]
   >Bazı durumlarda, VNet-VNet bağlantısı oluşturmak için PowerShell kullanmak zorunda kalabilirsiniz. Örneğin, farklı Azure hesapları kullanıyorsanız Portalı'nda bağlantı yapılandıramazsınız. Bu durumda bakın, [Azure portalını kullanarak VNet-VNet bağlantı yapılandırma](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).

1. [Yeni Bölge bir etki alanı denetleyicisi oluşturmak](../../../active-directory/active-directory-new-forest-virtual-machine.md).

   Birincil sitedeki etki alanı denetleyicisinin kullanılabilir değilse, bu etki alanı denetleyicisi kimlik doğrulaması sağlar.

1. [Yeni bölgede bir SQL Server sanal makine oluşturmak](virtual-machines-windows-portal-sql-server-provision.md).

1. [Yeni bölge ağındaki bir Azure yük dengeleyici oluşturma](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).

   Bu yük dengeleyici gerekir:

   - Aynı ağ ve alt ağ yeni bir sanal makine olabilir.
   - Kullanılabilirlik grubu dinleyicisi için statik bir IP adresi var.
   - Yalnızca sanal makineler yük dengeleyici ile aynı bölgede oluşan bir arka uç havuzu ekleyin.
   - IP adresi için belirli bir TCP bağlantı noktası araştırma kullanın.
   - Bir Yük Dengeleme kuralını belirli SQL Server'a aynı bölgede olması.  

1. [Yeni SQL Server Yük Devretme Kümelemesi özelliği eklemek](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).

1. [Yeni SQL Server etki alanına](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).

1. [Bir etki alanı hesabı kullanmak için yeni SQL Server hizmet hesabını ayarlamak](virtual-machines-windows-portal-sql-availability-group-prereq.md#setServiceAccount).

1. [Yeni SQL Server için Windows Server Yük devretme eklemek](virtual-machines-windows-portal-sql-availability-group-tutorial.md#addNode).

1. Küme üzerinde bir IP adresi kaynağı oluşturun.

   Yük Devretme Kümesi Yöneticisi'nde IP adresi kaynağı oluşturabilirsiniz. Kullanılabilirlik grubu role sağ tıklayın, **kaynak ekleme**, **daha kaynakları**, tıklatıp **IP adresi**.

   ![IP adresi oluşturun](./media/virtual-machines-windows-portal-sql-availability-group-dr/20-add-ip-resource.png)

   Bu IP adresi gibi yapılandırın:

   - Ağ uzak veri merkezinden kullanın.
   - IP adresi yeni Azure yük dengeleyiciden atayın. 

1. Yeni SQL Server'da SQL Server Yapılandırma Yöneticisi'nde, [Always On kullanılabilirlik grupları etkinleştirmek](http://msdn.microsoft.com/library/ff878259.aspx).

1. [Yeni SQL Server'da Güvenlik Duvarı bağlantı noktalarını açmak](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).

   Açmak için gereken bağlantı noktası numaralarını ortamınıza bağlıdır. Açık bağlantı noktası yansıtma uç nokta ve Azure dengeleyici durum araştırması yükleyin.

1. [Yeni SQL Server üzerinde kullanılabilirlik grubu bir çoğaltma eklemek](http://msdn.microsoft.com/library/hh213239.aspx).

   Uzak bir Azure bölgesindeki bir çoğaltma için el ile yük devretme ile zaman uyumsuz çoğaltma için ayarlayın.  

1. IP adresi kaynağı dinleyicisi istemci erişim noktası (ağ adı) küme için bağımlılık olarak ekleyin.

   Aşağıdaki ekran görüntüsü, düzgün şekilde yapılandırılmış bir IP adresi küme kaynağı gösterir:

   ![Kullanılabilirlik grubu](./media/virtual-machines-windows-portal-sql-availability-group-dr/50-configure-dependency-multiple-ip.png)

   >[!IMPORTANT]
   >Küme kaynak grubu iki IP adresi içerir. Bağımlılıklar dinleyicisi istemci erişim noktası için her iki IP adresleridir. Kullanım **veya** küme bağımlılık yapılandırmasında işleci.

1. [PowerShell'de küme parametrelerinin](virtual-machines-windows-portal-sql-availability-group-tutorial.md#setparam).

Küme ağ adı, IP adresini ve yeni bölgede yük dengeleyici üzerinde yapılandırılmış yoklama bağlantı noktasını PowerShell komut dosyasını çalıştırın.

   ```PowerShell
   $ClusterNetworkName = "<MyClusterNetworkName>" # The cluster name for the network in the new region (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name).
   $IPResourceName = "<IPResourceName>" # The cluster name for the new IP Address resource.
   $ILBIP = “<n.n.n.n>” # The IP Address of the Internal Load Balancer (ILB) in the new region. This is the static IP address for the load balancer you configured in the Azure portal.
   [int]$ProbePort = <nnnnn> # The probe port you set on the ILB.

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

## <a name="set-connection-for-multiple-subnets"></a>Birden çok alt ağlar için kümesi bağlantı

Uzak Veri merkezinde çoğaltmayı kullanılabilirlik grubunun parçası olan ancak farklı bir alt ağda değil. Bu çoğaltma birincil çoğaltmaya olursa, uygulama bağlantı zaman aşımları ortaya çıkabilir. Bu davranış bir şirket içi kullanılabilirlik grubunda birden çok alt ağ dağıtımı ile aynıdır. Uygulamaları istemciden gelen bağlantılara izin vermek için istemci bağlantısı güncelleştirmek veya küme ağ adı kaynağına bağlı önbelleğe alma ad çözümlemesi yapılandırın.

Tercihen ayarlamak için istemci bağlantı dizelerini güncelleştirmek `MultiSubnetFailover=Yes`. Bkz: [MultiSubnetFailover ile bağlanma](http://msdn.microsoft.com/library/gg471494#Anchor_0).

Bağlantı dizeleri değiştirilemiyor, ad çözümlemesi önbelleğe alma yapılandırabilirsiniz. Bkz: [birden çok alt ağ kullanılabilirlik grubundaki zaman aşımları](http://blogs.msdn.microsoft.com/alwaysonpro/2014/06/03/connection-timeouts-in-multi-subnet-availability-group/).

## <a name="fail-over-to-remote-region"></a>Uzak bir bölgeye yük devri

Dinleyici bağlantı uzak bölge sınamak için Uzak bölge çoğaltma üzerinden başarısız olabilir. Çoğaltma zaman uyumsuz olsa da, yük devretme olası veri kaybına karşı savunmasızdır. Veri kaybı olmadan yük devri için kullanılabilirlik modu için zaman uyumlu değiştirmek ve yük devretme modu otomatik olarak ayarlayın. Aşağıdaki adımları kullanın:

1. İçinde **Nesne Gezgini**, birincil çoğaltmayı barındıran SQL Server örneğine bağlanın.
1. Altında **AlwaysOn Kullanılabilirlik grupları**, **kullanılabilirlik grupları**, kullanılabilirlik grubunuzun sağ tıklatın ve **özellikleri**.
1. Üzerinde **genel** sayfasında **kullanılabilirlik çoğaltmalarının**, kullanılacak DR sitesi ikincil kopya kümesi **zaman uyumlu yürütme** kullanılabilirlik modu ve **otomatik** yük devretme modu.
1. Yüksek kullanılabilirlik için birincil, çoğaltma olarak aynı sitedeki bir ikincil çoğaltma varsa, bu çoğaltma kümesine **zaman uyumsuz tamamlama** ve **el ile**.
1. Tamam'a tıklayın.
1. İçinde **Object Explorer**, kullanılabilirlik grubunu sağ tıklatın ve **Göster Pano**.
1. Panoda, DR sitesi üzerinde çoğaltma eşitlendiğini doğrulayın.
1. İçinde **Object Explorer**, kullanılabilirlik grubunu sağ tıklatın ve **yük devretme...** . SQL Server Management stüdyoları SQL Server vermesine bir sihirbaz açar.  
1. Tıklatın **sonraki**, DR sitedeki SQL Server örneğini seçin. Tıklatın **sonraki** yeniden.
1. DR sitedeki SQL Server örneğine bağlanın ve tıklatın **sonraki**.
1. Üzerinde **Özet** sayfasında, ayarları doğrulayın ve tıklayın **son**.

Bağlantı test ediliyor sonra birincil çoğaltma birincil veri Merkezinize geri dönün ve yeniden normal işletim ayarlarına kullanılabilirlik modunu ayarlayın. Aşağıdaki tabloda bu belgede açıklanan mimarisi için normal işletimsel ayarları gösterilmektedir:

| Konum | Sunucu örneği | Rol | Kullanılabilirlik modu | Yük devretme modu
| ----- | ----- | ----- | ----- | -----
| Birincil veri merkezi | SQL-1 | Birincil | Zaman uyumlu | Automatic
| Birincil veri merkezi | SQL-2 | İkincil | Zaman uyumlu | Automatic
| İkincil ya da uzak veri merkezi | SQL-3 | İkincil | Zaman uyumsuz | El ile


### <a name="more-information-about-planned-and-forced-manual-failover"></a>Planlanan ve zorunlu el ile yük devretme hakkında daha fazla bilgi

Daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:

- [Bir kullanılabilirlik grubu (SQL Server) planlanmış bir el ile yük gerçekleştirin](http://msdn.microsoft.com/library/hh231018.aspx)
- [Bir kullanılabilirlik grubu (SQL Server) zorla el ile yük devretme gerçekleştirme](http://msdn.microsoft.com/library/ff877957.aspx)

## <a name="additional-links"></a>Ek Bağlantılar

* [Always On kullanılabilirlik grupları](http://msdn.microsoft.com/library/hh510230.aspx)
* [Azure Sanal Makineler](http://docs.microsoft.com/azure/virtual-machines/windows/)
* [Azure yük dengeleyicileri](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer)
* [Azure kullanılabilirlik kümeleri](../manage-availability.md)
