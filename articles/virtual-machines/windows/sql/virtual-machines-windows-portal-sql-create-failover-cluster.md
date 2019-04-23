---
title: SQL Server FCI - Azure sanal makineleri | Microsoft Docs
description: Bu makalede, Azure sanal Makineler'de SQL Server Yük devretme kümesi örneğini oluşturma işlemini açıklar.
services: virtual-machines
documentationCenter: na
author: MikeRayMSFT
manager: craigg
editor: monicar
tags: azure-service-management
ms.assetid: 9fc761b1-21ad-4d79-bebc-a2f094ec214d
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/11/2018
ms.author: mikeray
ms.openlocfilehash: a758cce85645e72bfd9434a69393133d3da6b57d
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60011376"
---
# <a name="configure-sql-server-failover-cluster-instance-on-azure-virtual-machines"></a>Azure sanal makinelerinde SQL Server Yük devretme kümesi örneğini yapılandırma

Bu makalede Resource Manager modeli Azure sanal makinelerinde bir SQL Server Yük devretme kümesi örneği (FCI) oluşturmayı açıklar. Bu çözümü kullanan [depolama alanları doğrudan Windows Server 2016 Datacenter edition \(S2D\) ](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview) (veri diskleri) depolama (Azure Vm'leri) düğümler arasında eşitlenen bir yazılım tabanlı sanal SAN olarak bir Windows Küme. S2d'yi, Windows Server 2016'da yenidir.

Aşağıdaki diyagramda, Azure sanal makinelerinde tam çözümünü gösterilmektedir:

![Kullanılabilirlik Grubu](./media/virtual-machines-windows-portal-sql-create-failover-cluster/00-sql-fci-s2d-complete-solution.png)

Yukarıdaki diyagramda gösterilmektedir:

- İki Azure sanal makinelerinde Windows Yük devretme kümesi. Ayrıca bir sanal makine yük devretme kümesinde olduğunda çağrılır bir *küme düğümü*, veya *düğümleri*.
- Her sanal makinenin iki veya daha fazla veri diski vardır.
- S2D, verileri diskteki verileri eşitler ve bir depolama havuzu olarak eşitlenmiş depolama sunar.
- Depolama havuzu ve yük devretme kümesine Küme Paylaşılan birimi (CSV) gösterir.
- SQL Server FCI küme rolünü CSV veri sürücüleri için kullanır.
- SQL Server FCI için IP adresini tutacak bir Azure yük dengeleyici.
- Azure kullanılabilirlik kümesine tüm kaynakları tutar.

   >[!NOTE]
   >Tüm Azure kaynaklarını diyagramda olan aynı kaynak grubundaysa.

S2D hakkında daha fazla ayrıntı için bkz: [depolama alanları doğrudan Windows Server 2016 Datacenter edition \(S2D\)](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview).

İki tür mimarileri - yakınsanmış ve hiper yakınsama s2d'yi destekler. Bu belgedeki hiper yakınsanmış bir mimaridir. Bir hiper yakınsama altyapısı, kümelenmiş uygulamanızı barındıran sunucularda depolama yerleştirir. Bu mimaride, her bir SQL Server FCI düğümde depolamadır.

## <a name="licensing-and-pricing"></a>Lisanslama ve fiyatlandırma

Azure sanal Makineler'de Kullandıkça Öde (PAYG) kullanarak SQL Server Lisansı veya kendi lisansını getir (KLG) VM görüntüleri. Seçtiğiniz görüntü türünü nasıl ücretlendirilir etkiler.

PAYG lisansı ile Azure Virtual Machines'de SQL Server Yük devretme kümesi örneği (FCI) FCI pasif düğümler dahil olmak üzere, tüm düğümlerinin ücreti alınmaz. Daha fazla bilgi için [SQL Server Enterprise sanal makineleri fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-enterprise/). 

Kurumsal Anlaşma Yazılım Güvencesi olan müşteriler ücretsiz pasif FCI düğüm etkin her düğüm için kullanılacak doğru olması. Azure bu Avantajdan yararlanmak için KLG VM görüntülerini kullanmak ve ardından her iki etkin ve Pasif düğümde FCI'ın aynı lisans'ı kullanın. Daha fazla bilgi için [Kurumsal Anlaşma](https://www.microsoft.com/en-us/Licensing/licensing-programs/enterprise.aspx).

Azure Virtual Machines'de SQL Server için lisanslama PAYG ve KLG Karşılaştırılacak bakın [SQL Vm'lerini kullanmaya başlayın](virtual-machines-windows-sql-server-iaas-overview.md#get-started-with-sql-vms).

SQL Server lisanslama hakkında tam bilgi için bkz. [fiyatlandırma](https://www.microsoft.com/sql-server/sql-server-2017-pricing).

### <a name="example-azure-template"></a>Örnek Azure şablonu

Çözümün tamamını Azure'da şablondan oluşturabilirsiniz. Bir şablon örneği Github'da kullanılabilir [Azure hızlı başlangıç şablonları](https://github.com/MSBrett/azure-quickstart-templates/tree/master/sql-server-2016-fci-existing-vnet-and-ad). Bu örnek değil tasarlanmış veya belirli bir iş yükü için test. Bir SQL Server FCI ile S2D depolamayı etki alanınıza bağlı oluşturmak için şablon çalıştırabilirsiniz. Şablonu değerlendirmek ve amaçlarınız için değiştirebilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Bilmeniz gereken bazı noktalar ve birkaç yerde, önce devam şey vardır.

### <a name="what-to-know"></a>Bilinmesi gerekenler
Aşağıdaki teknolojileri işletimsel bir anlayışa sahip olmalıdır:

- [Windows Küme teknolojilerini](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview)
- [SQL Server Yük devretme kümesi örnekleri](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server).

Önemli bir fark, bir Azure Iaas VM Konuk yük devretme kümesinde (düğüm) sunucusu ve tek bir alt ağ başına tek bir NIC'ye öneririz, ' dir. Azure ağı Ek NIC ve alt ağları gereksiz bir Azure Iaas VM Konuk kümede olmasını sağlayan fiziksel yedeklilik sahiptir. Küme doğrulama raporunu düğümlere yalnızca tek bir ağda erişilebilir durumda bir uyarı verir ancak Azure Iaas VM Konuk yük devretme kümelerinde bu uyarıyı güvenle yoksayılabilir. 

Ayrıca, aşağıdaki teknolojileri genel bir anlayışa sahip olmalıdır:

- [Windows Server 2016'da depolama alanları doğrudan kullanan hiper yakınsama çözümü](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct)
- [Azure kaynak grupları](../../../azure-resource-manager/manage-resource-groups-portal.md)

> [!IMPORTANT]
> Şu anda [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md) azure'da SQL Server FCI için desteklenmiyor. Uzantı FCI katılan vm'lerden kaldırmanızı öneririz. Bu uzantıyı otomatik yedekleme ve düzeltme eki uygulama ve SQL için portal bazı özellikleri gibi özellikleri destekler. Aracı kaldırıldıktan sonra bu özellikler, SQL VM'ler için çalışmaz.

### <a name="what-to-have"></a>Olması gerekenler

Bu makaledeki yönergeleri izlemeden önce olmanız gerekir:

- Microsoft Azure aboneliği.
- Bir Windows etki alanına Azure sanal makinelerinde.
- Azure sanal makineler'de nesneleri oluşturma iznine sahip bir hesap.
- Bir Azure sanal ağı ve alt ağ yeterli IP adres alanı aşağıdaki bileşenler için:
   - Her iki sanal makine.
   - Yük devretme küme IP adresi.
   - Her FCI için bir IP adresi.
- DNS etki alanı denetleyicilerine işaret eden Azure ağı üzerinde yapılandırılmış.

Bu önkoşulları yerine getirilince, yük devretme kümeniz oluşturmaya devam edebilirsiniz. İlk adım, sanal makineleri oluşturmaktır.

## <a name="step-1-create-virtual-machines"></a>1. Adım: Sanal makineler oluşturma

1. Oturum [Azure portalında](https://portal.azure.com) aboneliğinizle.

1. [Azure kullanılabilirlik kümesi oluşturma](../tutorial-availability-sets.md).

   Kullanılabilirlik grupları sanal makineler hata etki alanlarına ayarlayın ve güncelleme etki alanları. Kullanılabilirlik kümesi, uygulamanızın tek hata noktaları, ağ anahtarı veya güç ünitesi sunucu rafı etkilenmez emin olur.

   Sanal makineleriniz için kaynak grubunu oluşturmadıysanız, bunu Azure kullanılabilirlik kümesine oluşturduğunuzda. Kullanılabilirlik kümesi oluşturmak için Azure portalını kullanıyorsanız, aşağıdaki adımları uygulayın:

   - Azure portalında **+** Azure Marketi'nde açın. Arama **kullanılabilirlik kümesi**.
   - Tıklayın **kullanılabilirlik kümesi**.
   - **Oluştur**’a tıklayın.
   - Üzerinde **kullanılabilirlik kümesi oluştur** dikey penceresinde aşağıdaki değerleri ayarlayın:
      - **Ad**: Kullanılabilirlik kümesi için bir ad.
      - **Abonelik**: Azure aboneliğiniz.
      - **Kaynak grubu**: Varolan bir grubu kullanmak istiyorsanız, tıklayın **var olanı kullan** ve grubun aşağı açılan listeden seçin. Bir seçim **Yeni Oluştur** ve grup için bir ad yazın.
      - **Konum**: Sanal makinelerinizi oluştururken planladığınız konumunu ayarlayın.
      - **Hata etki alanları**: Varsayılan (3) kullanın.
      - **Güncelleme etki alanları**: Varsayılan (5) kullanın.
   - Tıklayın **Oluştur** kullanılabilirlik kümesi oluşturmak için.

1. Kullanılabilirlik kümesindeki sanal makineleri oluşturun.

   Azure kullanılabilirlik kümesinde iki SQL Server sanal makineleri sağlayın. Yönergeler için [Azure Portal'da bir SQL Server sanal makinesi sağlama](virtual-machines-windows-portal-sql-server-provision.md).

   Her iki sanal makine yerleştirin:

   - Aynı Azure, kullanılabilirlik kümesi kaynak grubunun içinde olduğu.
   - Etki alanı denetleyicisine aynı ağ üzerinde.
   - Hem sanal makineler ve sonuçta bu kümede kullanabilecek tüm Fcı'lerde için yeterli IP adres alanı ile bir alt ağ üzerinde.
   - Azure kullanılabilirlik kümesinde.   

      >[!IMPORTANT]
      >Ayarlayamıyor veya bir sanal makine oluşturulduktan sonra kullanılabilirlik değiştirin.

   Azure Marketi'nden bir görüntü seçin. Kullanabileceğiniz bir Market görüntüsü ile Windows Server ve SQL Server ya da yalnızca Windows Server içerir. Ayrıntılar için bkz [SQL Server'a genel bakış Azure sanal Makineler'de](virtual-machines-windows-sql-server-iaas-overview.md)

   Yüklü bir SQL Server örneğinin yanı sıra SQL Server yükleme yazılımı ve gerekli anahtar Azure Galerisi'nde bulunan resmi SQL Server görüntüleri içerir.

   SQL Server lisansı için ödeme yapmak istediğiniz göre doğru görüntüyü seçin:

   - **Lisans kullanım başına ödeme**: Bu görüntülerin saniye başına maliyet, SQL Server Lisans içerir:
      - **Windows Server Datacenter 2016 üzerinde SQL Server 2016 Enterprise**
      - **Windows Server Datacenter 2016 üzerinde SQL Server 2016 Standard**
      - **Windows Server Datacenter 2016 üzerinde SQL Server 2016 Developer**

   - **-Kendi-lisansını getir (KLG)**

      - **{KLG} Windows Server Datacenter 2016 üzerinde SQL Server 2016 Enterprise**
      - **{KLG} Windows Server Datacenter 2016 üzerinde SQL Server 2016 Standard**

   >[!IMPORTANT]
   >Sanal makineyi oluşturduktan sonra önceden yüklenmiş bir tek başına SQL Server örneğini kaldırın. Yük devretme kümesi ve S2D yapılandırdıktan sonra SQL Server FCI oluşturmak için önceden yüklenmiş bir SQL Server medya kullanın.

   Alternatif olarak, yalnızca işletim sistemi ile Azure Market görüntülerini kullanabilirsiniz. Seçin bir **Windows Server 2016 Datacenter** görüntü ve yük devretme kümesi ve S2D yapılandırdıktan sonra SQL Server FCI yükleyin. Bu görüntü, SQL Server yükleme medyası içermiyor. Yükleme medyasını her sunucu için SQL Server yüklemesi burada çalıştırabileceğiniz bir konuma yerleştirebilirsiniz.

1. Azure sanal makinelerinizi oluşturduktan sonra her bir sanal makine ile RDP bağlanın.

   RDP ile sanal makine için ilk kez bağlandığınızda, bilgisayarın ağda bulunabilir olması bu bilgisayar izin vermek isteyip istemediğinizi sorar. **Evet**'e tıklayın.

1. SQL Server tabanlı sanal makine görüntülerinden birini kullanıyorsanız, SQL Server örneğini kaldırın.

   - İçinde **programlar ve Özellikler**, sağ **Microsoft SQL Server 2016 (64-bit)** tıklatıp **Kaldır/Değiştir**.
   - Tıklayın **Kaldır**.
   - Varsayılan örneği seçin.
   - Tüm özellikleri altında Kaldır **veritabanı altyapısı Hizmetleri**. Kaldırmayın **paylaşılan özellikleri**. Aşağıdaki resme bakın:

      ![Özellikleri Kaldır](./media/virtual-machines-windows-portal-sql-create-failover-cluster/03-remove-features.png)

   - Tıklayın **sonraki**ve ardından **Kaldır**.

1. <a name="ports"></a>Güvenlik Duvarı bağlantı noktalarını açın.

   Her sanal makinede aşağıdaki Windows Güvenlik Duvarı bağlantı noktalarını açın.

   | Amaç | TCP bağlantı noktası | Notlar
   | ------ | ------ | ------
   | SQL Server | 1433 | Varsayılan SQL Server örnekleri için normal bağlantı noktası. Galeriden bir görüntü kullandıysanız, bu bağlantı noktasını otomatik olarak açılır.
   | Durum yoklaması | 59999 | Tüm TCP bağlantı noktasını açın. Daha sonraki bir adımda yük dengeleyici yapılandırma [durum araştırması](#probe) ve bu bağlantı noktasını kullanacak şekilde kümesi.  

1. Depolama, sanal makineye ekleyin. Ayrıntılı bilgi için bkz. [depolama ekleme](../disks-types.md).

   Her iki sanal makinelerin en az iki veri diski gerekir.

   Ham diski - değil NTFS biçimli disk.
      >[!NOTE]
      >Yalnızca NTFS biçimli disk eklerseniz, hiçbir disk uygunluk denetimi ile S2D etkinleştirebilirsiniz.  

   En az iki premium SSD her VM'e ekleyin. En az öneririz P30 (1 TB) diskleri.

   Kümesi konak için önbelleğe alma **salt okunur**.

   Üretim ortamlarında kullandığınız depolama kapasitesi, iş yüküne bağlıdır. Bu makalede açıklanan tanıtım ve test için değerler.

1. [Sanal makineleri önceden var olan etki alanınıza ekleyin](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).

Sanal makine oluşturulup yapılandırıldıktan sonra Yük devretme kümesini yapılandırabilirsiniz.

## <a name="step-2-configure-the-windows-failover-cluster-with-s2d"></a>2. Adım: Windows Yük devretme kümesi ile s2d'yi yapılandırma

Sonraki adım, yük devretme kümesi ile S2D yapılandırmaktır. Bu adımda, aşağıdaki alt adımların yaparsınız:

1. Windows Yük devretme kümeleme özelliğini ekleyin
1. Kümeyi doğrula
1. Yük devretme kümesi oluşturma
1. Bulut tanığı oluşturma
1. Depolama ekleme

### <a name="add-windows-failover-clustering-feature"></a>Windows Yük devretme kümeleme özelliğini ekleyin

1. Başlamak için ilk sanal makineye RDP Yerel yöneticilerin üyesi olan ve Active Directory'deki nesneleri oluşturma izni olan bir etki alanı hesabı kullanarak bağlanın. Yapılandırmayı geri kalanı için bu hesabı kullanın.

1. [Her bir sanal makine için Yük Devretme Kümelemesi özelliği eklemek](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).

   Kullanıcı Arabiriminden yük devretme kümeleme özelliğini yüklemek için aşağıdaki adımları her iki sanal makine üzerinde yapın.
   - İçinde **Sunucu Yöneticisi'ni**, tıklayın **Yönet**ve ardından **rol ve Özellik Ekle**.
   - İçinde **Ekle roller ve Özellikler Sihirbazı**, tıklayın **sonraki** gelene kadar **Özellikleri Seç**.
   - İçinde **Özellikleri Seç**, tıklayın **Yük Devretme Kümelemesi**. Tüm gerekli özellikleri ve yönetim araçlarını içerir. Tıklayın **özellikler eklemek**.
   - Tıklayın **sonraki** ve ardından **son** özellikleri yüklemek için.

   PowerShell ile yük devretme kümeleme özelliğini yüklemek için aşağıdaki betiği bir yönetici PowerShell oturumundan sanal makinelerden birini çalıştırın.

   ```powershell
   $nodes = ("<node1>","<node2>")
   Invoke-Command  $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools}
   ```

Başvuru için sonraki adımlara adım 3 / yönergeleri uygulayın. [Windows Server 2016'da depolama alanları doğrudan kullanan hiper yakınsama çözümü](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-3-configure-storage-spaces-direct).

### <a name="validate-the-cluster"></a>Kümeyi doğrula

Altında yönergeler bu kılavuzda başvurduğu [kümesi doğrulama](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-31-run-cluster-validation).

Küme kullanıcı arabirimini veya PowerShell ile doğrulayın.

Küme kullanıcı Arabirimi ile doğrulamak için sanal makinelerin birinden aşağıdaki adımları uygulayın.

1. İçinde **Sunucu Yöneticisi'ni**, tıklayın **Araçları**, ardından **yük devretme kümesi Yöneticisi**.
1. İçinde **yük devretme kümesi Yöneticisi**, tıklayın **eylem**, ardından **yapılandırmayı doğrula...** .
1. **İleri**’ye tıklayın.
1. Üzerinde **seçin sunucuları veya kümeyi**, her iki sanal makine adını yazın.
1. Üzerinde **seçeneği belirlenerek**, seçin **yalnızca seçtiğim Testleri Çalıştır**. **İleri**’ye tıklayın.
1. Üzerinde **Test seçimi**, dışındaki tüm testleri dahil **depolama**. Aşağıdaki resme bakın:

   ![Doğrulama testleri](./media/virtual-machines-windows-portal-sql-create-failover-cluster/10-validate-cluster-test.png)

1. **İleri**’ye tıklayın.
1. Üzerinde **onay**, tıklayın **sonraki**.

**Yapılandırma Doğrulama Sihirbazı** doğrulama testleri çalıştırır.

PowerShell ile küme doğrulamak için aşağıdaki betiği bir yönetici PowerShell oturumundan sanal makinelerden birini çalıştırın.

   ```powershell
   Test-Cluster –Node ("<node1>","<node2>") –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
   ```

Küme doğruladıktan sonra Yük devretme kümesi oluşturun.

### <a name="create-the-failover-cluster"></a>Yük devretme kümesi oluşturma

Bu kılavuzda başvurduğu [yük devretme kümesi oluşturma](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-32-create-a-cluster).

Yük devretme kümesi oluşturmak için ihtiyacınız vardır:
- Küme düğümleri duruma sanal makinelerin adları.
- Yük devretme kümesi için bir ad
- Yük devretme kümesi için bir IP adresi. Aynı Azure sanal ağı ve alt ağ üzerinde Küme düğümü olarak kullanılmayan bir IP adresi kullanabilirsiniz.

Aşağıdaki PowerShell, yük devretme kümesi oluşturur. Düğümler (sanal makine adları) ve Azure sanal ağdan kullanılabilir bir IP adresi adlarıyla betiğini güncelleştirin:

```powershell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage
```   

### <a name="create-a-cloud-witness"></a>Bulut tanığı oluşturma

Bulut tanığı yeni bir Azure depolama Blobu'nda depolanan küme çekirdek tanığı türüdür. Bu, bir Tanık paylaşımı barındıran ayrı bir sanal makinenin ihtiyacını ortadan kaldırır.

1. [Yük devretme kümesi için bulut tanığı oluşturma](https://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).

1. Bir blob kapsayıcı oluşturun.

1. Erişim anahtarları ve kapsayıcı URL'si kaydedin.

1. Yük devretme kümesi çekirdek tanığı yapılandırın. Bkz, [çekirdek tanığı yapılandırma kullanıcı arabiriminde](https://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness#to-configure-cloud-witness-as-a-quorum-witness) kullanıcı arabiriminde.

### <a name="add-storage"></a>Depolama ekleme

S2D için diskler boş ve bölümler veya başka veri içermemesi gerekir. Temizlemek için diskleri izleyin [bu kılavuzdaki adımları](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-34-clean-disks).

1. [Alanları doğrudan etkinleştir Store \(S2D\)](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-35-enable-storage-spaces-direct).

   Aşağıdaki PowerShell depolama alanları doğrudan'ı etkinleştirir.  

   ```powershell
   Enable-ClusterS2D
   ```

   İçinde **yük devretme kümesi Yöneticisi**, artık depolama havuzu görebilirsiniz.

1. [Birim oluşturma](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-36-create-volumes).

   S2D özelliklerden biri, etkinleştirdiğinizde, otomatik olarak bir depolama havuzu oluşturmasıdır. Birim oluşturmak artık hazırsınız. PowerShell komutu `New-Volume` biçimlendirme, kümeye ekleme ve Küme Paylaşılan birimi (CSV) oluşturma gibi birim oluşturma işlemini otomatikleştirir. Aşağıdaki örnek, bir 800 gigabayt (GB) CSV oluşturur.

   ```powershell
   New-Volume -StoragePoolFriendlyName S2D* -FriendlyName VDisk01 -FileSystem CSVFS_REFS -Size 800GB
   ```   

   Bu komut tamamlandıktan sonra 800 GB birimin küme kaynağı olarak bağlanmıştır. Birim altındadır `C:\ClusterStorage\Volume1\`.

   Bir Küme Paylaşılan birimine S2D ile Aşağıdaki diyagramda gösterilmiştir:

   ![ClusterSharedVolume](./media/virtual-machines-windows-portal-sql-create-failover-cluster/15-cluster-shared-volume.png)

## <a name="step-3-test-failover-cluster-failover"></a>3. Adım: Test yük devretme küme yük devretmesi

Yük Devretme Kümesi Yöneticisi'nde bir küme düğümü için depolama kaynağı taşıyabilirsiniz doğrulayın. Yük devretme kümesine ile bağlanabiliyorsa **yük devretme kümesi Yöneticisi** ve depolama, bir düğümden diğerine taşımak, FCI yapılandırmaya hazır olursunuz.

## <a name="step-4-create-sql-server-fci"></a>4. Adım: SQL Server FCI oluşturun

Yük devretme kümesi ve depolama da dahil olmak üzere tüm küme bileşenleri yapılandırdıktan sonra SQL Server FCI oluşturabilirsiniz.

1. İlk sanal makineye RDP ile bağlanın.

1. İçinde **yük devretme kümesi Yöneticisi**, tüm küme çekirdek kaynakları ilk sanal makinede olduğundan emin olun. Gerekirse, tüm kaynaklar için bu sanal makineyi taşıyın.

1. Yükleme medyasını bulun. Sanal makine, Azure Market görüntülerinden birini kullanıyorsa, ortam şu konumdadır `C:\SQLServer_<version number>_Full`. Tıklayın **Kurulum**.

1. İçinde **SQL Server Yükleme Merkezi'ni**, tıklayın **yükleme**.

1. Tıklayın **yeni SQL Server Yük devretme kümesi yükleme**. SQL Server FCI yüklemek için sihirbazdaki yönergeleri izleyin.

   FCI veri dizinleri kümelenmiş depolama biriminde olması gerekir. S2d'yi ile paylaşılan bir disk, ancak bağlama noktası her sunucusundaki bir birime değil. S2D birimin iki düğüm arasında eşitler. Birim kümeye Küme Paylaşılan birimi olarak sunulur. CSV bağlama noktası veri dizinleri için kullanın.

   ![DataDirectories](./media/virtual-machines-windows-portal-sql-create-failover-cluster/20-data-dicrectories.png)

1. Sihirbazı tamamladıktan sonra Kurulum SQL Server FCI ilk düğümüne yükleyin.

1. Kurulum, ilk düğümde FCI başarıyla yüklendikten sonra ikinci düğüme RDP ile bağlanın.

1. Açık **SQL Server Yükleme Merkezi'ni**. Tıklayın **yükleme**.

1. Tıklayın **bir SQL Server Yük devretme kümesine Ekle düğüm**. SQL server yükleyin ve FCI için bu sunucuyu eklemek için sihirbazdaki yönergeleri izleyin.

   >[!NOTE]
   >SQL Server ile Azure Market Galerisi görüntüye kullandıysanız, SQL Server araçları görüntüsüne dahil. Bu görüntü kullanmadıysanız, SQL Server araçlarını ayrı olarak yükleyin. Bkz: [SQL Server Management Studio'yu (SSMS) indirme](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="step-5-create-azure-load-balancer"></a>5. Adım: Azure yük dengeleyici oluşturma

Azure sanal makinelerinde kümeleri aynı anda bir küme düğümünde olması gereken IP adresi tutmak için bir yük dengeleyici kullanın. Bu çözümde, SQL Server FCI için IP adresini yük dengeleyici tutar.

[Oluşturma ve bir Azure load balancer yapılandırma](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).

### <a name="create-the-load-balancer-in-the-azure-portal"></a>Azure portalında yük dengeleyici oluşturma

Yük Dengeleyici oluşturmak için:

1. Azure portalında, sanal makineler ile kaynak grubuna gidin.

1. **+ Ekle**'ye tıklayın. Markette Ara **yük dengeleyici**. Tıklayın **yük dengeleyici**.

1. **Oluştur**’a tıklayın.

1. Yük Dengeleyici ile yapılandırın:

   - **Ad**: Yük Dengeleyici tanımlayan ad.
   - **Tür**: Yük Dengeleyici, genel veya özel olabilir. Özel yük dengeleyici aynı sanal ağ içinde erişilebilir. Çoğu Azure uygulamaları özel yük dengeleyici kullanabilirsiniz. Uygulamanızın doğrudan Internet üzerinden SQL Server'a erişim gerekiyorsa, herkese açık yük dengeleyici kullanın.
   - **Sanal ağ**: Sanal makineler aynı ağ.
   - **Alt ağ**: Sanal makineler aynı alt ağda.
   - **Özel IP adresi**: SQL Server FCI küme ağ kaynağına atanan aynı IP adresi.
   - **Abonelik**: Azure aboneliğiniz.
   - **Kaynak grubu**: Aynı kaynak grubunu, sanal makinelerinizi kullanın.
   - **Konum**: Aynı Azure konumunda sanal makinelerinizi kullanın.
   Aşağıdaki resme bakın:

   ![CreateLoadBalancer](./media/virtual-machines-windows-portal-sql-create-failover-cluster/30-load-balancer-create.png)

### <a name="configure-the-load-balancer-backend-pool"></a>Yük Dengeleyici arka uç havuzunu yapılandırma

1. Sanal makineler ile Azure kaynak grubu geri dönün ve Yeni Yük Dengeleyiciyi bulun. Kaynak grubu görünümü yenilemeniz gerekebilir. Yük dengeleyiciye tıklayın.

1. Tıklayın **arka uç havuzları** tıklatıp **+ Ekle** arka uç havuzuna eklenecek.

1. Arka uç havuzu Vm'leri içeren kullanılabilirlik kümesi ile ilişkilendirin.

1. Altında **hedef ağ IP yapılandırmaları**, kontrol **sanal makine** ve küme düğümü olarak katılacak olan sanal makineleri seçin. FCI barındıracak tüm sanal makineler eklediğinizden emin olun. 

1. Tıklayın **Tamam** arka uç havuzu oluşturun.

### <a name="configure-a-load-balancer-health-probe"></a>Bir yük dengeleyici durum araştırması yapılandırın

1. Yük Dengeleyici dikey penceresinde tıklayın **sistem durumu araştırmalarının**.

1. **+ Ekle**'ye tıklayın.

1. Üzerinde **durum araştırması Ekle** dikey penceresinde <a name="probe"> </a>sistem durumu araştırması parametrelerini ayarla:

   - **Ad**: Durum araştırması için bir ad.
   - **Protokol**: TCP.
   - **Bağlantı noktası**: Sistem durumu araştırması için güvenlik duvarını oluşturduğunuz bağlantı noktasına ayarlayın [bu adımı](#ports). Bu makalede, örnek TCP bağlantı noktasını kullanır. `59999`.
   - **Aralığı**: 5 saniye.
   - **Sağlıksız durum eşiği**: 2 art arda hatalar.

1. Tamam'a tıklayın.

### <a name="set-load-balancing-rules"></a>Yük Dengeleme kuralları ayarlayın

1. Yük Dengeleyici dikey penceresinde tıklayın **Yük Dengeleme kuralları**.

1. **+ Ekle**'ye tıklayın.

1. Yük Dengeleme kuralları parametreleri ayarlayın:

   - **Ad**: Yük Dengeleme kuralları için bir ad.
   - **Ön uç IP adresi**: IP adresi için SQL Server FCI küme ağ kaynağı kullanın.
   - **Bağlantı noktası**: SQL Server FCI TCP bağlantı noktasını ayarlayın. Varsayılan örneği bağlantı noktası 1433'dür.
   - **Arka uç bağlantı noktası**: Aynı bağlantı noktası olarak bu değeri kullanır **bağlantı noktası** değeri, etkinleştirdiğinizde **kayan IP (doğrudan sunucu dönüşü)**.
   - **Arka uç havuzu**: Daha önce yapılandırılmış arka uç havuzu adı kullanın.
   - **Durum araştırması**: Daha önce yapılandırılmış durum araştırması kullanabilirsiniz.
   - **Oturum kalıcılığı**: Yok.
   - **Boşta kalma zaman aşımı (dakika)**: 4.
   - **Kayan IP (doğrudan sunucu dönüşü)**: Enabled

1. **Tamam** düğmesine tıklayın.

## <a name="step-6-configure-cluster-for-probe"></a>6. Adım: Araştırma için küme yapılandırma

PowerShell'de küme araştırma bağlantı noktası parametresini ayarlayın.

Küme araştırma bağlantı noktası parametresini ayarlamak için aşağıdaki betiği değişkenlerinde ortamınızdaki değerlerle güncelleştirin. Açılı ayraçlar kaldırmak `<>` komut dosyası. 

   ```powershell
   $ClusterNetworkName = "<Cluster Network Name>"
   $IPResourceName = "<SQL Server FCI IP Address Resource Name>" 
   $ILBIP = "<n.n.n.n>" 
   [int]$ProbePort = <nnnnn>

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

Önceki betikte ortamınızı ayarlayın. Aşağıdaki liste değerleri açıklar:

   - `<Cluster Network Name>`: Ağ için Windows Server Yük devretme kümesi adı. İçinde **yük devretme kümesi Yöneticisi** > **ağları**, ağ üzerinde sağ tıklatıp **özellikleri**. Doğru değeri altındadır **adı** üzerinde **genel** sekmesi. 

   - `<SQL Server FCI IP Address Resource Name>`: SQL Server FCI IP adresi kaynağın adı. İçinde **yük devretme kümesi Yöneticisi** > **rolleri**, SQL Server FCI rolü altında altında **sunucu adı**, IP adresi kaynağını sağ tıklayın ve tıklayın**Özellikleri**. Doğru değeri altındadır **adı** üzerinde **genel** sekmesi. 

   - `<ILBIP>`: ILB IP adresi. Bu adres ILB uç adresi olarak Azure portalında yapılandırılır. Bu aynı zamanda SQL Server FCI IP adresidir. İçinde bulabilirsiniz **yük devretme kümesi Yöneticisi** nerede aynı özellikleri sayfasında `<SQL Server FCI IP Address Resource Name>`.  

   - `<nnnnn>`: Yük Dengeleyici durum araştırması yapılandırılmış araştırma bağlantı noktasıdır. Kullanılmayan TCP bağlantı noktası geçerli değil. 

>[!IMPORTANT]
>Küme parametresi için alt ağ maskesi, TCP IP yayın adresi olmalıdır: `255.255.255.255`.

Küme araştırma ayarladıktan sonra PowerShell'de küme parametrelerin tümü görebilirsiniz. Şu betiği çalıştırın:

   ```powershell
   Get-ClusterResource $IPResourceName | Get-ClusterParameter 
  ```

## <a name="step-7-test-fci-failover"></a>7. Adım: FCI yük devretme testi

Yük devretme küme işlevselliğini doğrulamak için FCI testi. Aşağıdaki adımları uygulayın:

1. RDP ile SQL Server FCI küme düğümlerinden birinin bağlanın.

1. Açık **yük devretme kümesi Yöneticisi**. Tıklayın **rolleri**. SQL Server FCI rol sahibi olan düğümü dikkat edin.

1. SQL Server FCI role sağ tıklayın.

1. Tıklayın **taşıma** tıklatıp **olası en iyi düğüme**.

**Yük Devretme Kümesi Yöneticisi** rol ve kaynaklarını çevrimdışı duruma gösterir. Kaynakları taşıma ve başka bir düğümde çevrimiçi olması.

### <a name="test-connectivity"></a>Bağlantıyı test etme

Bağlantıyı test etmek için aynı sanal ağdaki başka bir sanal makinede oturum açın. Açık **SQL Server Management Studio** ve SQL Server FCI adına bağlanın.

>[!NOTE]
>Gerekirse, yapabilecekleriniz ise [SQL Server Management Studio'yu indirme](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="limitations"></a>Sınırlamalar

Azure sanal makineleri kümelenmiş paylaşılan birimleri (CSV) depolaması ile Windows Server 2019 üzerinde Microsoft Dağıtılmış İşlem Düzenleyicisi (MSDTC) desteği ve [standart load balancer](../../../load-balancer/load-balancer-standard-overview.md).

Azure sanal makinelerinde MSDTC Windows Server 2016 ve önceki sürümleri için desteklenmez:

- Kümelenmiş MSDTC'nin kaynak, paylaşılan depolama kullanmak için yapılandırılamaz. Bir MSDTC kaynağı oluşturursanız, depolama olsa bile ile Windows Server 2016, tüm paylaşılan depolama kullanmak için kullanılabilir göstermez. Windows Server 2019 ' Bu sorun düzeltilmiştir.
- Temel Azure load balancer'a RPC bağlantı noktalarını işlemez.

## <a name="see-also"></a>Ayrıca Bkz.

[Kurulum s2d'yi Uzak Masaüstü (Azure)](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-storage-spaces-direct-deployment)

[Depolama alanları doğrudan ile hiper yakınsama çözümü](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct).

[Depolama alanına doğrudan genel bakış](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview)

[S2d'yi SQL Server desteği](https://blogs.technet.microsoft.com/dataplatforminsider/2016/09/27/sql-server-2016-now-supports-windows-server-2016-storage-spaces-direct/)
