---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 276ddf0a70fa450451cd3ddc78c7610c4ab1edc1
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66165824"
---
Kullanılabilirlik grubu dinleyicisi SQL Server kullanılabilirlik grubu dinlediği bir IP adresi ve ağ adı değil. Kullanılabilirlik grubu dinleyicisi oluşturmak için aşağıdakileri yapın:

1. <a name="getnet"></a>Küme ağ kaynağı adını alın.

    a. Birincil çoğaltmayı barındıran Azure sanal makine için bağlanmak için RDP kullanın. 

    b. Yük Devretme Kümesi Yöneticisi'ni açın.

    c. Seçin **ağları** düğüm ve Not küme ağ adı. Bu adı kullanan `$ClusterNetworkName` PowerShell betik değişken. Küme ağ adı aşağıdaki görüntüde olduğu **küme ağ 1**:

   ![Küme ağ adı](./media/virtual-machines-ag-listener-configure/90-clusternetworkname.png)

1. <a name="addcap"></a>İstemci erişim noktası ekleyin.  
    İstemci erişim noktası uygulamaları bir kullanılabilirlik grubuna veritabanlarına bağlanmak için kullandığınız ağ adıdır. İstemci erişim noktası, yük devretme kümesi Yöneticisi'nde oluşturun.

    a. Küme adını genişletin ve ardından **rolleri**.

    b. İçinde **rolleri** bölmesinde, kullanılabilirlik grubunun adına sağ tıklayın ve ardından **kaynak Ekle** > **istemci erişim noktası**.

   ![İstemci erişim noktası](./media/virtual-machines-ag-listener-configure/92-addclientaccesspoint.png)

    c. İçinde **adı** kutusunda, bu yeni bir dinleyici için bir ad oluşturun. 
   Yeni bir dinleyici için uygulamalar SQL Server kullanılabilirlik grubu içindeki veritabanlarına bağlanmak için kullandığınız ağ adı adıdır.

    d. Dinleyiciyi oluşturmayı tamamlamak için tıklatın **sonraki** iki kez tıkladıktan sonra **son**. Dinleyici veya kaynağı çevrimiçi bu noktada getirmek değil.

1. Kullanılabilirlik grubu küme rolünü çevrimdışı duruma getirin. İçinde **yük devretme kümesi Yöneticisi** altında **rolleri**, role sağ tıklayın ve seçin **rolü Durdur**.

1. <a name="congroup"></a>Kullanılabilirlik grubu için IP kaynağı yapılandırın.

    a. Tıklayın **kaynakları** sekmesine ve ardından oluşturduğunuz istemci erişim noktası genişletin.  
    İstemci erişim noktası çevrimdışı kalır.

   ![İstemci erişim noktası](./media/virtual-machines-ag-listener-configure/94-newclientaccesspoint.png) 

    b. IP kaynağını sağ tıklayın ve ardından Özellikler seçeneğine tıklayın. IP adresi adını not edin ve sonra içinde `$IPResourceName` PowerShell betik değişken.

    c. Altında **IP adresi**, tıklayın **statik IP adresi**. IP adresini Azure portalında yük dengeleyici adresi ayarlarken kullandığınız aynı adresi olarak ayarlayın.

   ![Kaynak IP](./media/virtual-machines-ag-listener-configure/96-ipresource.png) 

    <!-----------------------I don't see this option on server 2016
    1. Disable NetBIOS for this address and click **OK**. Repeat this step for each IP resource if your solution spans multiple Azure VNets. 
    ------------------------->

1. <a name = "dependencyGroup"></a>SQL Server kullanılabilirlik grubu kaynağının istemci erişim noktasında bağımlı hale getirin.

    a. Yük Devretme Kümesi Yöneticisi'nde **rolleri**ve ardından, kullanılabilirlik grubuna tıklayın.

    b. Üzerinde **kaynakları** sekmesindeki **diğer kaynakları**kullanılabilirlik kaynak grubuna sağ tıklayın ve ardından **özellikleri**. 

    c. Bağımlılıkları sekmesinde istemci erişim noktası (dinleyici) kaynağın adını ekleyin.

   ![Kaynak IP](./media/virtual-machines-ag-listener-configure/97-propertiesdependencies.png) 

    d. **Tamam** düğmesine tıklayın.

1. <a name="listname"></a>İstemci erişim noktası kaynak IP adresine bağımlı olun.

    a. Yük Devretme Kümesi Yöneticisi'nde **rolleri**ve ardından, kullanılabilirlik grubuna tıklayın. 

    b. Üzerinde **kaynakları** sekmesinde, istemci erişim noktası kaynağa altında sağ **sunucu adı**ve ardından **özellikleri**. 

   ![Kaynak IP](./media/virtual-machines-ag-listener-configure/98-dependencies.png) 

    c. Tıklayın **bağımlılıkları** sekmesi. IP adresi bir bağımlılığı olduğundan emin olun. Yüklü değilse, IP adresi üzerinde bir bağımlılık ayarlayın. Listelenen birden çok kaynaklar varsa, IP adresleri veya değil olduğunu doğrulayın ve bağımlılıkları. **Tamam**'ı tıklatın. 

   ![Kaynak IP](./media/virtual-machines-ag-listener-configure/98-propertiesdependencies.png) 

    >[!TIP]
    >Bağımlılıkları düzgün şekilde yapılandırıldığını doğrulayabilirsiniz. Kullanılabilirlik grubu, rollere sağ tıklayın, yük devretme kümesi Yöneticisi'nde Git **diğer eylemler**ve ardından **bağımlılık raporunu göster**. Bağımlılıkları doğru şekilde yapılandırıldığında, ağ adı üzerinde kullanılabilirlik grubu bağlıdır ve ağ adı IP adresi üzerinde bağlıdır. 


1. <a name="setparam"></a>PowerShell'de küme parametrelerini ayarlayın.

   a. SQL Server örneklerinden birinde aşağıdaki PowerShell betiğini kopyalayın. Benzeri değişkenleri ortamınız için güncelleştirin.

   - `$ListenerILBIP` Azure yük dengeleyicide kullanılabilirlik grubu dinleyicisi için oluşturduğunuz IP adresidir.
    
   - `$ListenerProbePort` Azure yük dengeleyicide kullanılabilirlik grubu dinleyicisi için yapılandırılan bağlantı noktasıdır.

   ```powershell
   $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
   $IPResourceName = "<IPResourceName>" # the IP Address resource name
   $ListenerILBIP = "<n.n.n.n>" # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
   [int]$ListenerProbePort = <nnnnn>
  
   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ListenerILBIP";"ProbePort"=$ListenerProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

   b. Küme düğümlerinden biri üzerinde PowerShell betiğini çalıştırarak küme parametrelerini ayarlayın.  

   > [!NOTE]
   > SQL Server örneklerinizin farklı bölgelerde bulunuyorsa, iki kez PowerShell betiğini çalıştırmanız gerekir. İlk kez kullanmak `$ListenerILBIP` ve `$ListenerProbePort` ilk bölgeden. İkinci kez kullanın `$ListenerILBIP` ve `$ListenerProbePort` ikinci bölgeden. Küme ağ adı ve küme IP kaynak adı her bölge için farklıdır.

1. Kullanılabilirlik grubu küme rolünü çevrimiçi duruma getirin. İçinde **yük devretme kümesi Yöneticisi** altında **rolleri**, role sağ tıklayın ve seçin **rol Başlat**.

Gerekirse, WSFC küme IP adresi kümesi parametrelerini ayarlamak için yukarıdaki adımları yineleyin.

1. IP adresi adı WSFC küme IP adresini alın. İçinde **yük devretme kümesi Yöneticisi** altında **küme çekirdek kaynakları**, bulun **sunucu adı**.

1. Sağ **IP adresi**seçip **özellikleri**.

1. Kopyalama **adı** IP adresi. Olabilir `Cluster IP Address`. 

1. <a name="setwsfcparam"></a>PowerShell'de küme parametrelerini ayarlayın.
  
   a. SQL Server örneklerinden birinde aşağıdaki PowerShell betiğini kopyalayın. Benzeri değişkenleri ortamınız için güncelleştirin.

   - `$ClusterCoreIP` Azure load balancer WSFC çekirdek küme kaynağı için oluşturduğunuz IP adresidir. Kullanılabilirlik grubu dinleyicisi için IP adresi farklıdır.

   - `$ClusterProbePort` WSFC durum araştırması için Azure yük dengeleyici üzerinde yapılandırılan bağlantı noktasıdır. Kullanılabilirlik grubu dinleyicisinin araştırma farklıdır.

   ```powershell
   $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
   $IPResourceName = "<ClusterIPResourceName>" # the IP Address resource name
   $ClusterCoreIP = "<n.n.n.n>" # the IP Address of the Cluster IP resource. This is the static IP address for the load balancer you configured in the Azure portal.
   [int]$ClusterProbePort = <nnnnn> # The probe port from the WSFCEndPointprobe in the Azure portal. This port must be different from the probe port for the availability group listener probe port.
  
   Import-Module FailoverClusters
  
   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ClusterCoreIP";"ProbePort"=$ClusterProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

   b. Küme düğümlerinden biri üzerinde PowerShell betiğini çalıştırarak küme parametrelerini ayarlayın.  

>[!WARNING]
>Kullanılabilirlik grubu dinleyicisi sistem durumu araştırma bağlantı noktası, küme çekirdeği IP adresi sistem durumu araştırma bağlantı noktasından farklı olmak zorundadır. Bu örneklerde dinleyicisi bağlantı noktası 59999 ve küme çekirdek IP adresi 58888 şeklindedir. Her iki bağlantı noktası izin verme gelen güvenlik duvarı kuralı gerektirir.