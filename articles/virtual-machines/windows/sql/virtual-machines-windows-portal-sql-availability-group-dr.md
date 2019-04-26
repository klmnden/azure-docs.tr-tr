---
title: SQL Server kullanılabilirlik gruplarını - Azure sanal makineler - olağanüstü durum kurtarma | Microsoft Docs
description: Bu makalede, Azure sanal makinelerinde farklı bir bölgede bir çoğaltması ile bir SQL Server kullanılabilirlik grubu yapılandırma açıklanmaktadır.
services: virtual-machines
documentationCenter: na
author: MikeRayMSFT
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
ms.openlocfilehash: 8f5b470cb3f75f434033a245f4aaa185aeb665c0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60325987"
---
# <a name="configure-an-always-on-availability-group-on-azure-virtual-machines-in-different-regions"></a>Farklı bölgelerdeki Azure sanal makinelerinde Always On kullanılabilirlik grubu yapılandırma

Bu makalede, Azure sanal makineleri uzak bir Azure konumunda bir SQL Server Always On kullanılabilirlik grubu çoğaltması yapılandırma açıklanmaktadır. Bu yapılandırma, olağanüstü durum kurtarmayı desteklemek için kullanın.

Bu makale, Azure sanal makinelerini Resource Manager modunda geçerlidir.

Aşağıdaki resimde, Azure sanal makinelerinde bir kullanılabilirlik grubunda ortak bir dağıtım gösterilmektedir:

   ![Kullanılabilirlik Grubu](./media/virtual-machines-windows-portal-sql-availability-group-dr/00-availability-group-basic.png)

Bu Dağıtımdaki tüm sanal makineler bir Azure bölgesinde olur. SQL-1 ve 2 SQL otomatik yük devretme ile zaman uyumlu yürütme kullanılabilirlik grubu çoğaltmalarının olabilir. Bu mimari oluşturmak için bkz: [kullanılabilirlik grubu şablonu veya öğretici](virtual-machines-windows-portal-sql-availability-group-overview.md).

Azure bölgesi erişilemez duruma gelirse, bu mimari, kapalı kalma savunmasızdır. Bu güvenlik açığını gidermek için farklı bir Azure bölgesinde bir çoğaltma ekleyin. Aşağıdaki diyagram, yeni mimarisinin nasıl görüneceğini gösterir:

   ![Kullanılabilirlik grubu DR](./media/virtual-machines-windows-portal-sql-availability-group-dr/00-availability-group-basic-dr.png)

SQL-3 olarak adlandırılan yeni bir sanal makine önceki şemada gösterilmektedir. SQL-3 farklı bir Azure bölgesinde kullanılır. SQL-3 Windows sunucu yük devretme kümesine eklenir. SQL-3, bir kullanılabilirlik grubu çoğaltması barındırabilirsiniz. Son olarak, SQL 3 Azure bölgesinde yeni bir Azure yük dengeleyici olduğuna dikkat edin.

>[!NOTE]
> Bir Azure kullanılabilirlik kümesinde birden fazla sanal makine aynı bölgede olduğunda gereklidir. Yalnızca bir sanal makine bölgededir, kullanılabilirlik kümesinin gerekli değildir. Yalnızca bir sanal makine bir kullanılabilirlik kümesi oluşturma zamanında yerleştirebilirsiniz. Sanal makineyi bir kullanılabilirlik kümesine ise, bir sanal makine için daha sonra ek bir yineleme ekleyebilirsiniz.

Bu mimaride, uzak bir bölgeye çoğaltma genellikle zaman uyumsuz tamamlama kullanılabilirlik modu ve el ile yük devretme modu ile yapılandırılır.

Kullanılabilirlik grubu çoğaltmalarının farklı Azure bölgelerindeki Azure sanal makinelerinde olduğunda, her bölge gerektirir:

* Bir sanal ağ geçidi
* Sanal ağ geçidi bağlantısı

Aşağıdaki diyagramda, ağları veri merkezleri arasında iletişim kurma biçimini gösterir.

   ![Kullanılabilirlik Grubu](./media/virtual-machines-windows-portal-sql-availability-group-dr/01-vpngateway-example.png)

>[!IMPORTANT]
>Bu mimari, Azure bölgeleri arasında çoğaltılan veriler için giden veri ücreti alınmaz. Bkz: [bant genişliği fiyatlandırma](https://azure.microsoft.com/pricing/details/bandwidth/).  

## <a name="create-remote-replica"></a>Uzak bir çoğaltma oluşturma

Bir uzak veri merkezinde bir çoğaltma oluşturmak için aşağıdaki adımları uygulayın:

1. [Yeni bölgede bir sanal ağ oluşturma](../../../virtual-network/manage-virtual-network.md#create-a-virtual-network).

1. [Azure portalını kullanarak VNet-VNet bağlantı yapılandırma](../../../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).

   >[!NOTE]
   >Bazı durumlarda, VNet-VNet bağlantısı oluşturmak için PowerShell kullanmanız gerekebilir. Örneğin, farklı Azure hesapları kullanıyorsanız, portalda bağlantı yapılandıramazsınız. Bu durumda bkz [Azure portalını kullanarak VNet-VNet bağlantı yapılandırma](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).

1. [Bir etki alanı denetleyicisi nda yeni bölge Oluştur](../../../active-directory/active-directory-new-forest-virtual-machine.md).

   Birincil sitedeki etki alanı denetleyicisinin kullanılabilir değilse, bu etki alanı denetleyicisi kimlik doğrulaması sağlar.

1. [Yeni bölgede bir SQL Server sanal makinesi oluşturma](virtual-machines-windows-portal-sql-server-provision.md).

1. [Yeni bölge hakkında ağdaki bir Azure yük dengeleyici oluşturma](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).

   Bu yük dengeleyici gerekir:

   - Aynı ağ ve alt ağ yeni bir sanal makine olabilir.
   - Kullanılabilirlik grubu dinleyicisi için statik bir IP adresi var.
   - Sanal makineler yalnızca yük dengeleyici ile aynı bölgede oluşan bir arka uç havuzu ekleyin.
   - IP adresi için belirli bir TCP bağlantı noktası araştırması kullanabilirsiniz.
   - Yük Dengeleme kuralı belirli SQL Server'a aynı bölgede olması.  
   - Standart yük dengeleyici arka uç havuzundaki sanal makinelerin tek bir kullanılabilirlik kümesi ya da sanal makine ölçek kümesi parçası değilse olabilir. Ek bilgileri İnceleme için [Azure Load Balancer Standard genel bakış](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview).

1. [Yeni SQL Server için Yük Devretme Kümelemesi özelliği eklemek](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).

1. [Yeni SQL Server etki alanına](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).

1. [Yeni SQL Server hizmet hesabı bir etki alanı hesabı kullanacak şekilde ayarlama](virtual-machines-windows-portal-sql-availability-group-prereq.md#setServiceAccount).

1. [Yeni SQL Server için Windows Server Yük devretme kümesi ekleme](virtual-machines-windows-portal-sql-availability-group-tutorial.md#addNode).

1. Küme üzerinde bir IP adresi kaynağı oluşturun.

   Yük Devretme Kümesi Yöneticisi'nde IP adresi kaynağı oluşturabilirsiniz. Kullanılabilirlik grubu role sağ tıklayın, **kaynak Ekle**, **daha kaynakları**, tıklatıp **IP adresi**.

   ![IP adresi oluşturma](./media/virtual-machines-windows-portal-sql-availability-group-dr/20-add-ip-resource.png)

   Bu IP adresi gibi yapılandırın:

   - Uzak veri merkezinden ağ kullanın.
   - Yeni Azure yük dengeleyiciden IP adresi atayın. 

1. SQL Server Yapılandırma Yöneticisi'nde, yeni SQL Server'da [Always On kullanılabilirlik grupları'nı etkinleştirme](https://msdn.microsoft.com/library/ff878259.aspx).

1. [Yeni SQL Server'da Güvenlik Duvarı bağlantı noktalarını açmak](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).

   Açmak için gereken bağlantı noktası numaraları, ortamınıza bağlıdır. Yansıtma uç noktası ve Azure için açık bağlantı noktalarını dengeleyici durum araştırması yükleyin.

1. [Yeni SQL Server kullanılabilirlik grubu için bir çoğaltma ekleme](https://msdn.microsoft.com/library/hh213239.aspx).

   Uzak bir Azure bölgesinde bir çoğaltma için el ile yük devretme ile zaman uyumsuz çoğaltma için bunu ayarlayın.  

1. IP adresi kaynağı dinleyicisi istemci erişim noktası (ağ adı) küme için bir bağımlılık olarak ekleyin.

   Aşağıdaki ekran görüntüsünde, düzgün bir şekilde yapılandırılmış bir IP adresi küme kaynağı gösterir:

   ![Kullanılabilirlik Grubu](./media/virtual-machines-windows-portal-sql-availability-group-dr/50-configure-dependency-multiple-ip.png)

   >[!IMPORTANT]
   >Küme kaynak grubunu, her iki IP adreslerini içerir. Bağımlılıklar dinleyicisi istemci erişim noktası için her iki IP adresleridir. Kullanım **veya** küme bağımlılık yapılandırmasında işleci.

1. [PowerShell'de küme parametrelerinin](virtual-machines-windows-portal-sql-availability-group-tutorial.md#setparam).

Küme ağ adı, IP adresi ve yeni bölgede yük dengeleyicideki yapılandırılmış yoklama bağlantı noktası ile PowerShell Betiği çalıştırın.

   ```powershell
   $ClusterNetworkName = "<MyClusterNetworkName>" # The cluster name for the network in the new region (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name).
   $IPResourceName = "<IPResourceName>" # The cluster name for the new IP Address resource.
   $ILBIP = “<n.n.n.n>” # The IP Address of the Internal Load Balancer (ILB) in the new region. This is the static IP address for the load balancer you configured in the Azure portal.
   [int]$ProbePort = <nnnnn> # The probe port you set on the ILB.

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

## <a name="set-connection-for-multiple-subnets"></a>Birden çok alt ağlar için bağlantıyı Ayarla

Uzak Veri merkezinde çoğaltmayı kullanılabilirlik Grubu'nun parçası ancak farklı bir alt ağ içinde. Bu çoğaltma birincil çoğaltmaya hale gelirse, uygulama bağlantı zaman aşımları oluşabilir. Bu davranış, bir şirket içi kullanılabilirlik grubunda birden çok alt ağ dağıtımı ile aynıdır. İstemci bağlantılarını uygulamaları izin vermek için istemci bağlantısını güncelleştirin veya küme ağ adı kaynağına bağlı önbelleğe alma ad çözümlemesi yapılandırabilirsiniz.

Tercihen, ayarlanacak istemci bağlantı dizelerini güncelleştirmek `MultiSubnetFailover=Yes`. Bkz: [MultiSubnetFailover ile bağlanma](https://msdn.microsoft.com/library/gg471494#Anchor_0).

Bağlantı dizeleri değiştiremeyeceğiniz, ad çözümlemesi önbelleğe alma yapılandırabilirsiniz. Bkz: [birden çok alt ağ kullanılabilirlik grubu bağlantı zaman aşımları](https://blogs.msdn.microsoft.com/alwaysonpro/2014/06/03/connection-timeouts-in-multi-subnet-availability-group/).

## <a name="fail-over-to-remote-region"></a>Uzak bir bölgeye yük devretme

Uzak bir bölgeye dinleyici bağlantısını test etmek için uzak bir bölgeye çoğaltma üzerinde başarısız olabilir. Çoğaltma zaman uyumsuz olsa da, yük devretme olası veri kaybına karşı savunmasızdır. Veri kaybı olmadan yük devretmek için kullanılabilirlik modunu zaman uyumlu şekilde değiştirin ve yük devretme modunu otomatik olarak ayarlayın. Aşağıdaki adımları kullanın:

1. İçinde **Nesne Gezgini**, birincil çoğaltmayı barındıran SQL Server örneğine bağlanın.
1. Altında **AlwaysOn Kullanılabilirlik grupları**, **kullanılabilirlik grupları**, kullanılabilirlik grubunuzun sağ tıklatıp **özellikleri**.
1. Üzerinde **genel** sayfasındaki **kullanılabilirlik çoğaltmalarının**, ikincil çoğaltma kullanmak için DR sitesi ayarlayın **zaman uyumlu işleme** kullanılabilirlik modu ve  **Otomatik** yük devretme modu.
1. Yüksek kullanılabilirlik için birincil çoğaltma olarak aynı sitedeki bir ikincil çoğaltma varsa, bu çoğaltma kümesi **zaman uyumsuz tamamlama** ve **el ile**.
1. Tamam'a tıklayın.
1. İçinde **Nesne Gezgini**, kullanılabilirlik grubunu sağ tıklatın ve **Göster Pano**.
1. Panoda, DR sitesi üzerinde çoğaltma eşitlendiğini doğrulayın.
1. İçinde **Nesne Gezgini**, kullanılabilirlik grubunu sağ tıklatın ve **yük devretme...** . SQL Server Management Studios, SQL sunucusu üzerinde başarısız bir sihirbaz açar.  
1. Tıklayın **sonraki**, DR sitedeki SQL Server örneği seçin. Tıklayın **sonraki** yeniden.
1. DR sitedeki SQL Server örneğine bağlanın ve tıklayın **sonraki**.
1. Üzerinde **özeti** sayfasında, ayarları doğrulayın ve tıklayın **son**.

Bağlantı test ediliyor sonra birincil çoğaltmayı birincil veri merkeziniz dönün ve kullanılabilirlik modu geri normal çalışma ayarlarına ayarlayın. Aşağıdaki tabloda bu belgede açıklanan ve mimarinin normal çalıştırma ayarlarını gösterilmektedir:

| Location | Sunucu örneği | Rol | Kullanılabilirlik modu | Yük devretme modu
| ----- | ----- | ----- | ----- | -----
| Birincil veri merkezi | SQL-1 | Birincil | Zaman uyumlu | Automatic
| Birincil veri merkezi | SQL-2 | İkincil | Zaman uyumlu | Automatic
| İkincil veya uzak veri merkezi | SQL-3 | İkincil | Zaman uyumsuz | Manual


### <a name="more-information-about-planned-and-forced-manual-failover"></a>Planlanmış ve zorla el ile yük devretme hakkında daha fazla bilgi

Daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:

- [Bir kullanılabilirlik grubuna (SQL Server) planlanan el ile Yük Devretmesini gerçekleştirin.](https://msdn.microsoft.com/library/hh231018.aspx)
- [Bir kullanılabilirlik grubuna (SQL Server) zorla el ile Yük Devretmesini gerçekleştirin.](https://msdn.microsoft.com/library/ff877957.aspx)

## <a name="additional-links"></a>Ek Bağlantılar

* [Always On kullanılabilirlik grupları](https://msdn.microsoft.com/library/hh510230.aspx)
* [Azure Sanal Makineler](https://docs.microsoft.com/azure/virtual-machines/windows/)
* [Azure Load balancer'ları](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer)
* [Azure kullanılabilirlik kümeleri](../manage-availability.md)
