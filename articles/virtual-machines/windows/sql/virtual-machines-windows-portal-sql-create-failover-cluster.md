---
title: SQL Server FCI - Azure sanal makineleri | Microsoft Docs
description: "Bu makalede Azure sanal makinelerde SQL Server Yük devretme kümesi örneği oluşturma açıklanmaktadır."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
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
ms.date: 09/26/2017
ms.author: mikeray
ms.openlocfilehash: 8c957b1f2b4466ba68d81885fb014ad4026a47d2
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="configure-sql-server-failover-cluster-instance-on-azure-virtual-machines"></a>SQL Server Yük devretme kümesi örneği üzerinde Azure sanal makineleri yapılandırma

Bu makalede, Resource Manager modelinde Azure sanal makinelerde bir SQL Server Yük devretme kümesi örneği (FCI) oluşturma açıklanmaktadır. Bu çözümü kullanan [depolama alanları doğrudan Windows Server 2016 Datacenter edition \(S2D\) ](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview) Windows kümesi (Azure VM) düğümleri arasında (veri diskleri) depolama eşitleyen bir yazılım tabanlı sanal SAN olarak. S2D, Windows Server 2016'da yeni bir özelliktir.

Aşağıdaki diyagramda, Azure sanal makinelerde eksiksiz çözüm gösterilmektedir:

![Kullanılabilirlik grubu](./media/virtual-machines-windows-portal-sql-create-failover-cluster/00-sql-fci-s2d-complete-solution.png)

Önceki diyagramda gösterilmektedir:

- İki Azure sanal makinelerinde Windows Yük devretme kümesi. Bir sanal makine bir yük devretme kümesinde olduğunda da adlandırılır bir *küme düğümü*, veya *düğümleri*.
- Her sanal makinenin iki veya daha fazla veri diski var.
- S2D verileri diskteki verileri eşitler ve bir depolama havuzu olarak eşitlenmiş depolama sunar.
- Depolama havuzu ve yük devretme kümesine Küme Paylaşılan birimi (CSV) gösterir.
- SQL Server FCI küme rolünü CSV veri sürücüleri için kullanır.
- IP adresi için SQL Server FCI tutmak için bir Azure yük dengeleyici.
- Bir Azure kullanılabilirlik kümesi tüm kaynakları tutar.

   >[!NOTE]
   >Tüm Azure diyagramda kaynaklardır aynı kaynak grubunda yer alan.

S2D hakkında daha fazla ayrıntı için bkz: [depolama alanları doğrudan Windows Server 2016 Datacenter edition \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview).

S2D mimarileri - yakınsanmış ve hiper yakınsanmış iki türlerini destekler. Bu belgedeki hiper yakınsanmış bir mimaridir. Hiper yakınsanmış bir altyapı depolama kümelenmiş uygulama ana bilgisayar aynı sunuculara yerleştirir. Bu mimaride, depolama, üzerinde her SQL Server FCI düğümdür.

### <a name="example-azure-template"></a>Örnek Azure şablonu

Çözümün tamamında Azure'da bir şablondan oluşturabilirsiniz. Şablon örneği Github'da kullanılabilir [Azure hızlı başlangıç şablonlarını](https://github.com/MSBrett/azure-quickstart-templates/tree/master/sql-server-2016-fci-existing-vnet-and-ad). Bu örnekte değil tasarlanmış veya belirli bir iş yükü için test. Etki alanınıza bağlı S2D depolama ile bir SQL Server FCI oluşturmak için şablon çalıştırabilirsiniz. Şablon değerlendirin ve amaçlarınız için değiştirebilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Bilmeniz gereken birkaç şey ve, önce yerinde devam şunları vardır.

### <a name="what-to-know"></a>Bilinmesi gerekenler
Aşağıdaki teknolojileri işletimsel bir anlamış olmanız gerekir:

- [Windows Küme teknolojileri](http://technet.microsoft.com/library/hh831579.aspx)
-  [SQL Server Yük devretme kümesi örnekleri](http://msdn.microsoft.com/library/ms189134.aspx).

Ayrıca, aşağıdaki teknolojileri genel bir anlamış olmanız gerekir:

- [Depolama alanları doğrudan Windows Server 2016 kullanarak Hyper-yakınsanmış çözüm](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct)
- [Azure kaynak grupları](../../../azure-resource-manager/resource-group-portal.md)

### <a name="what-to-have"></a>Ne sağlamak için

Bu makaledeki yönergeleri izlemeden önce olmanız gerekir:

- Microsoft Azure aboneliği.
- Bir Windows etki alanı Azure sanal makinelerde.
- Azure sanal makinede nesneleri oluşturma izni olan bir hesap.
- Bir Azure sanal ağ ve alt yeterli IP adres alanı aşağıdaki bileşenler için:
   - Her iki sanal makineler.
   - Yük devretme küme IP adresi.
   - Her FCI için bir IP adresi.
- DNS etki alanı denetleyicilerine işaret eden Azure ağ üzerinde yapılandırılmış.

Bu önkoşulları yerine getirilince, yük devretme kümesi oluşturmaya devam edebilirsiniz. Sanal makineler oluşturmak için ilk adımdır bakın.

## <a name="step-1-create-virtual-machines"></a>1. adım: sanal makineler oluşturma

1. Oturum [Azure portal](http://portal.azure.com) aboneliğinizle.

1. [Azure kullanılabilirlik kümesi oluştur](../tutorial-availability-sets.md).

   Kullanılabilirlik grupları sanal makineleri hata etki alanlarında ayarlayın ve güncelleme etki alanları. Kullanılabilirlik kümesi, uygulamanızın tek hata noktaları, ağ anahtarı veya güç ünitesi bir sunucu rafı gibi etkilenmez emin olur.

   Sanal makineleriniz için kaynak grubu oluşturmadıysanız, bunu bir Azure kullanılabilirlik kümesi oluşturduğunuzda. Kullanılabilirlik kümesi oluşturmak için Azure portalını kullanıyorsanız, aşağıdaki adımları uygulayın:

   - Azure portalında tıklatın  **+**  Azure Marketi açın. Arama **kullanılabilirlik kümesi**.
   - Tıklatın **kullanılabilirlik kümesi**.
   - **Oluştur**’a tıklayın.
   - Üzerinde **kullanılabilirlik kümesi oluştur** dikey penceresinde, aşağıdaki değerleri ayarlayın:
      - **Ad**: kullanılabilirlik kümesi için bir ad.
      - **Abonelik**: bilgisayarınızı Azure aboneliği.
      - **Kaynak grubu**: varolan bir grubu kullanmak istiyorsanız, tıklatın **var olanı kullan** ve Grup aşağı açılan listeden seçin. Seçmediğiniz **Yeni Oluştur** ve grup için bir ad yazın.
      - **Konum**: sanal makinelerinizi oluştururken planladığınız konumunu ayarlayın.
      - **Hata etki alanları**: varsayılan (3) kullanın.
      - **Güncelleme etki alanları**: varsayılan (5) kullanın.
   - Tıklatın **oluşturma** kullanılabilirlik oluşturmak için ayarlayın.

1. Sanal makineler kullanılabilirlik kümesi oluşturun.

   Azure kullanılabilirlik kümesinde iki SQL Server sanal makine sağlayın. Yönergeler için bkz: [Azure portalında bir SQL Server sanal makine sağlama](virtual-machines-windows-portal-sql-server-provision.md).

   Her iki sanal makine koyun:

   - Aynı Azure, kullanılabilirlik kümesi kaynak grubu değil.
   - Etki alanı denetleyicisi olarak aynı ağ üzerinde.
   - Sanal makineler hem bu kümede sonunda kullanabilir tüm Fcı'lerde için yeterli IP adres alanı ile bir alt ağ üzerinde.
   - Azure kullanılabilirlik kümesinde.   

      >[!IMPORTANT]
      >Ayarlayın veya bir sanal makine oluşturulduktan sonra ayarlanmış kullanılabilirlik değiştirin.

   Azure Marketi'nde bir görüntü seçin. Bir Market kullanabilirsiniz, görüntü, Windows Server ve SQL Server ya da yalnızca Windows Server içerir. Ayrıntılar için bkz [genel bakış SQL Server'ın Azure sanal makineler üzerinde](virtual-machines-windows-sql-server-iaas-overview.md)

   Resmi SQL Server görüntülerini Azure galerisinde yüklü SQL Server örneğini ve SQL Server yükleme yazılım ve gereken anahtar içerir.

   SQL Server Lisans için ödeme yapmak istediğiniz nasıl göre uygun görüntüyü seçin:

   - **Kullanım lisansı başına ödeme**: SQL Server Lisansı bu görüntülerin dakika başına maliyet içerir:
      - **Windows Server Datacenter 2016 üzerinde SQL Server 2016 Enterprise**
      - **Windows Server Datacenter 2016 SQL Server 2016 standardı**
      - **Windows Server Datacenter 2016 SQL Server 2016 Geliştirici**

   - **Getir bilgisayarınızı-kendi-lisans (KLG)**

      - **{KLG} Windows Server Datacenter 2016 üzerinde SQL Server 2016 Enterprise**
      - **{KLG} Windows Server Datacenter 2016 SQL Server 2016 standardı**

   >[!IMPORTANT]
   >Sanal makineyi oluşturduktan sonra önceden yüklenmiş tek başına SQL Server örneğini kaldırın. Yük devretme kümesi ve S2D yapılandırdıktan sonra SQL Server FCI oluşturmak için önceden yüklenmiş SQL Server medya kullanır.

   Alternatif olarak, yalnızca işletim sistemi ile Azure Market görüntülerini kullanabilirsiniz. Seçin bir **Windows Server 2016 Datacenter** görüntü ve yük devretme kümesi ve S2D yapılandırdıktan sonra SQL Server FCI yükleyin. Bu görüntü, SQL Server yükleme medyasını içermiyor. Yükleme medyasındaki her sunucu için SQL Server yüklemesi çalıştırdığı bir konuma yerleştirin.

1. Azure sanal makinelerinizi oluşturduktan sonra her sanal makinenin RDP ile bağlanır.

   İlk RDP ile bir sanal makineye bağlandığınızda, bilgisayar bu Bilgisayara ağ üzerinde bulunabilir izin vermek isteyip istemediğinizi sorar. **Evet**'e tıklayın.

1. SQL Server tabanlı sanal makine görüntülerden birini kullanıyorsanız, SQL Server örneğini kaldırın.

   - İçinde **programlar ve Özellikler**, sağ **Microsoft SQL Server 2016 (64 bit)** tıklatıp **Kaldır/Değiştir**.
   - Tıklatın **kaldırmak**.
   - Varsayılan örneği seçin.
   - Tüm Özellikleri'nin altında kaldırmak **veritabanı altyapısı Hizmetleri**. Kaldırmayın **Paylaşılan Özellikler**. Aşağıdaki resme bakın:

      ![Özellikleri Kaldır](./media/virtual-machines-windows-portal-sql-create-failover-cluster/03-remove-features.png)

   - Tıklatın **sonraki**ve ardından **kaldırmak**.

1. <a name="ports"></a>Güvenlik Duvarı bağlantı noktalarını açın.

   Her bir sanal makine Windows Güvenlik Duvarı'nda aşağıdaki bağlantı noktalarını açın.

   | Amaç | TCP bağlantı noktası | Notlar
   | ------ | ------ | ------
   | SQL Server | 1433 | SQL Server'ın varsayılan örnekleri için normal bağlantı noktası. Galeriden bir görüntü kullandıysanız, bu bağlantı noktası otomatik olarak açılır.
   | Sistem durumu araştırması | 59999 | Herhangi bir TCP bağlantı noktasını açın. Bir sonraki adımda, yük dengeleyici yapılandırma [durumu araştırması](#probe) ve bu bağlantı noktasını kullanmak üzere küme.  

1. Depolama sanal makineye ekleyin. Ayrıntılı bilgi için bkz: [depolama ekleme](../premium-storage.md).

   Her iki sanal makinelerin en az iki veri diskleri gerekir.

   Ham diskleri - bağlamanız değil NTFS biçimli disk.
      >[!NOTE]
      >NTFS biçimli disk eklerseniz, yalnızca hiçbir disk uygunluk denetimi ile S2D etkinleştirebilirsiniz.  

   En az iki Premium Storage (SSD diskleri) her VM'e ekleyin. En az öneririz P30 (1 TB) diskler.

   Kümesi konak için önbelleğe almayı **salt okunur**.

   Üretim ortamlarında kullanmak depolama kapasitesi, iş yüküne bağlıdır. Bu makalede açıklanan tanıtım ve test için değerlerdir.

1. [Sanal makineleri önceden var olan etki alanınıza ekleyin](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).

Sanal makineler oluşturup yapılandırılmış sonra Yük devretme kümesini yapılandırabilirsiniz.

## <a name="step-2-configure-the-windows-failover-cluster-with-s2d"></a>2. adım: S2D ile Windows Yük devretme kümesi yapılandırma

Sonraki adım, yük devretme kümesi ile S2D yapılandırmaktır. Bu adımda, aşağıdaki alt adımlar yapar:

1. Windows Yük Devretme Kümelemesi özelliğini ekleyin
1. Küme doğrulama
1. Yük devretme kümesi oluşturma
1. Bulut tanığı oluşturma
1. Depolama ekleme

### <a name="add-windows-failover-clustering-feature"></a>Windows Yük Devretme Kümelemesi özelliğini ekleyin

1. Başlamak için Yerel yöneticilerin üyesi olan ve Active Directory'deki nesneleri oluşturma izni olan bir etki alanı hesabı kullanarak RDP ile ilk sanal makineye bağlanın. Bu hesap yapılandırması geri kalanı için kullanın.

1. [Her bir sanal makine için Yük Devretme Kümelemesi özelliği eklemek](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).

   Kullanıcı Arabiriminden Yük Devretme Kümelemesi özelliğini yüklemek için her iki sanal makinede aşağıdaki adımları uygulayın.
   - İçinde **Sunucu Yöneticisi'ni**, tıklatın **Yönet**ve ardından **rol ve Özellik Ekle**.
   - İçinde **Ekle roller ve Özellikler Sihirbazı**, tıklatın **sonraki** için elde edene kadar **Özellikleri Seç**.
   - İçinde **Özellikleri Seç**, tıklatın **Yük Devretme Kümelemesi**. Tüm gerekli özellikleri ve yönetim araçlarını içerir. Tıklatın **özelliklere**.
   - Tıklatın **sonraki** ve ardından **son** özellikleri yüklemek için.

   PowerShell ile Yük Devretme Kümelemesi özelliğini yüklemek için aşağıdaki betiği bir yönetici PowerShell oturumundan sanal makinelerden birini çalıştırın.

   ```PowerShell
   $nodes = ("<node1>","<node2>")
   Invoke-Command  $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools}
   ```

Başvuru için sonraki adımlar adım 3'ün altında yönergeleri [depolama alanları doğrudan Windows Server 2016 kullanarak Hyper-yakınsanmış çözüm](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-3-configure-storage-spaces-direct).

### <a name="validate-the-cluster"></a>Küme doğrulama

Bu kılavuzda altındaki yönergeleri başvuruyor [küme doğrulama](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-31-run-cluster-validation).

Küme kullanıcı arabirimini veya PowerShell ile doğrulayın.

Kullanıcı Arabirimi ile küme doğrulamak için aşağıdaki adımları sanal makinelerin birini yapın.

1. İçinde **Sunucu Yöneticisi'ni**, tıklatın **Araçları**, ardından **yük devretme kümesi Yöneticisi**.
1. İçinde **yük devretme kümesi Yöneticisi**, tıklatın **eylem**, ardından **yapılandırmasını doğrula...** .
1. **İleri**’ye tıklayın.
1. Üzerinde **sunucuları Seç veya küme**, her iki sanal makine adını yazın.
1. Üzerinde **testi seçenekleri**, seçin **yalnızca seçtiğim Testleri Çalıştır**. **İleri**’ye tıklayın.
1. Üzerinde **Test seçimi**, dışındaki tüm testleri dahil **depolama**. Aşağıdaki resme bakın:

   ![Testleri doğrula](./media/virtual-machines-windows-portal-sql-create-failover-cluster/10-validate-cluster-test.png)

1. **İleri**’ye tıklayın.
1. Üzerinde **onay**, tıklatın **sonraki**.

**Yapılandırma Doğrulama Sihirbazı** doğrulama testleri çalıştırır.

PowerShell ile küme doğrulamak için aşağıdaki komut dosyasını bir yönetici PowerShell oturumundan sanal makinelerden birini çalıştırın.

   ```PowerShell
   Test-Cluster –Node ("<node1>","<node2>") –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
   ```

Kümeyi doğrulamaya sonra Yük devretme kümesi oluşturun.

### <a name="create-the-failover-cluster"></a>Yük devretme kümesi oluşturma

Bu kılavuz başvurduğu [yük devretme kümesi oluşturma](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-32-create-a-cluster).

Yük devretme kümesi oluşturmak için gerekir:
- Küme düğümleri duruma sanal makineleri adları.
- Yük devretme kümesi için bir ad
- Yük devretme kümesi için bir IP adresi. Aynı Azure sanal ağ ve alt küme düğümleri olarak kullanılmayan bir IP adresi kullanabilirsiniz.

Aşağıdaki PowerShell yük devretme kümesi oluşturur. Düğümler (sanal makine adları) ve Azure VNET kullanılabilir bir IP adresinden adlarıyla betik güncelleştirin:

```PowerShell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage
```   

### <a name="create-a-cloud-witness"></a>Bir bulut tanığı oluşturma

Bulut tanığı, küme çekirdek tanığı bir Azure depolama Blob depolanan yeni türüdür. Bu, bir Tanık paylaşımı barındırma ayrı bir VM gereksinimini ortadan kaldırır.

1. [Yük devretme kümesi için bir bulut tanığı oluşturma](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).

1. Bir blob kapsayıcı oluşturun.

1. Erişim tuşları ve kapsayıcı URL'si kaydedin.

1. Yük devretme kümesi küme çekirdek tanığı yapılandırın. [Kullanıcı arabiriminde çekirdek tanığı yapılandırma], bakın. (http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness#to-configure-cloud-witness-as-a-quorum-witness) kullanıcı arabiriminde.

### <a name="add-storage"></a>Depolama ekleme

S2D diskler boş ve bölümleri veya diğer veri olması gerekir. Temizlemek için diskleri izleyin [bu kılavuzdaki adımları](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-34-clean-disks).

1. [Etkinleştirme depolama alanları doğrudan \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-35-enable-storage-spaces-direct).

   Aşağıdaki PowerShell doğrudan depolama alanları sağlar.  

   ```PowerShell
   Enable-ClusterS2D
   ```

   İçinde **yük devretme kümesi Yöneticisi**, depolama havuzu şimdi görebilirsiniz.

1. [Bir birim oluşturmak](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-36-create-volumes).

   S2D özelliklerden biri, etkinleştirdiğinizde, otomatik olarak bir depolama havuzu oluşturmasıdır. Şimdi bir birim oluşturmak hazır olursunuz. PowerShell komutunu `New-Volume` biçimlendirme, kümeye ekleme ve Küme Paylaşılan birimi (CSV) oluşturma gibi birim oluşturma işlemini otomatikleştirir. Aşağıdaki örnek, bir 800 gigabayt (GB) CSV oluşturur.

   ```PowerShell
   New-Volume -StoragePoolFriendlyName S2D* -FriendlyName VDisk01 -FileSystem CSVFS_REFS -Size 800GB
   ```   

   Bu komut tamamlandıktan sonra bir 800 GB birimin küme kaynağı olarak bağlı. Birim altındadır `C:\ClusterStorage\Volume1\`.

   Aşağıdaki diyagramda, Küme Paylaşılan birimi S2D ile gösterilmektedir:

   ![ClusterSharedVolume](./media/virtual-machines-windows-portal-sql-create-failover-cluster/15-cluster-shared-volume.png)

## <a name="step-3-test-failover-cluster-failover"></a>3. adım: Test yük devretme küme yük devretmesi

Yük Devretme Kümesi Yöneticisi'nde, diğer küme düğümüne depolama kaynağı taşıyabildiğinizi doğrulayın. İle yük devretme kümesine bağlanabiliyorsa **yük devretme kümesi Yöneticisi** ve bir düğümden diğerine, depolama birimini taşımak, FCI yapılandırmaya hazırsınız demektir.

## <a name="step-4-create-sql-server-fci"></a>4. adım: SQL Server FCI oluşturma

Yük devretme kümesi ve depolama da dahil olmak üzere tüm küme bileşenleri yapılandırdıktan sonra SQL Server FCI oluşturabilirsiniz.

1. RDP ile ilk sanal makineye bağlanın.

1. İçinde **yük devretme kümesi Yöneticisi**, tüm küme çekirdek kaynakları ilk sanal makinede olduğundan emin olun. Gerekirse, tüm kaynaklar bu sanal makineyi taşıyın.

1. Yükleme medyasındaki bulun. Sanal makine Azure Marketi görüntülerden birini kullanıyorsa, medya konumundadır `C:\SQLServer_<version number>_Full`. Tıklatın **Kurulum**.

1. İçinde **SQL Server Yükleme Merkezi'ni**, tıklatın **yükleme**.

1. Tıklatın **yeni SQL Server Yük devretme kümesi yükleme**. SQL Server FCI yüklemek için sihirbazdaki yönergeleri izleyin.

   FCI veri dizinlerini kümelenmiş depolama biriminde olması gerekir. S2D ile paylaşılan bir disk, ancak her sunucu üzerindeki bir birime bir bağlama noktası olmadığı. S2D birimin her iki düğüm arasında eşitler. Birim kümeye Küme Paylaşılan birimi olarak sunulur. CSV bağlama noktası veri dizinler için kullanın.

   ![DataDirectories](./media/virtual-machines-windows-portal-sql-create-failover-cluster/20-data-dicrectories.png)

1. Kurulum Sihirbazı'nı tamamladıktan sonra bir SQL Server FCI ilk düğümde yükleyecek.

1. Kurulum, ilk düğümde FCI başarıyla yüklendikten sonra ikinci düğüme RDP ile bağlanın.

1. Açık **SQL Server Yükleme Merkezi'ni**. Tıklatın **yükleme**.

1. Tıklatın **bir SQL Server Yük devretme kümesine Ekle düğümü**. SQL server yükleyin ve bu sunucu için FCI eklemek için sihirbazdaki yönergeleri izleyin.

   >[!NOTE]
   >SQL Server ile bir Azure Market galeri görüntüsü kullandıysanız, SQL Server araçları görüntüsüne dahil edilir. Bu görüntü kullanmadıysanız, SQL Server araçları ayrı olarak yükleyin. Bkz: [karşıdan SQL Server Management Studio'yu (SSMS)](http://msdn.microsoft.com/library/mt238290.aspx).

## <a name="step-5-create-azure-load-balancer"></a>5. adım: Azure yük dengeleyici oluşturma

Azure sanal makinelerde kümeleri aynı anda bir küme düğümünde olması gereken bir IP adresi tutmak için bir yük dengeleyici kullanın. Bu çözümde, SQL Server FCI için IP adresini yük dengeleyici tutar.

[Oluşturma ve Azure yük dengeleyici yapılandırma](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).

### <a name="create-the-load-balancer-in-the-azure-portal"></a>Azure portalında yük dengeleyici oluşturma

Yük Dengeleyici oluşturmak için:

1. Azure portalında sanal makinelerle kaynak grubuna gidin.

1. Tıklatın **+ Ekle**. Market arama **yük dengeleyici**. Tıklatın **yük dengeleyici**.

1. **Oluştur**’a tıklayın.

1. Yük Dengeleyici ile yapılandırın:

   - **Ad**: yük dengeleyici tanımlayan bir ad.
   - **Tür**: yük dengeleyici, genel veya özel olabilir. Özel yük dengeleyici aynı sanal ağ içinde erişilebilir. Çoğu Azure uygulamaları özel yük dengeleyici kullanabilirsiniz. Uygulamanızın doğrudan Internet üzerinden SQL Server erişmesi gerekirse bir genel yük dengeleyiciye kullanın.
   - **Sanal ağ**: sanal makineleri aynı ağ.
   - **Alt ağ**: sanal makineler aynı alt ağda.
   - **Özel IP adresi**: SQL Server FCI küme ağ kaynağına atanan aynı IP adresi.
   - **Abonelik**: bilgisayarınızı Azure aboneliği.
   - **Kaynak grubu**: sanal makinelerinizi aynı kaynak grubunu kullanın.
   - **Konum**: aynı Azure konumunda sanal makinelerinizi kullanın.
   Aşağıdaki resme bakın:

   ![CreateLoadBalancer](./media/virtual-machines-windows-portal-sql-create-failover-cluster/30-load-balancer-create.png)

### <a name="configure-the-load-balancer-backend-pool"></a>Yük Dengeleyici arka uç havuzunu yapılandırma

1. Sanal makineler ile Azure kaynak grubu geri dönün ve Yeni Yük Dengeleyici bulun. Kaynak grubu görünümü yenilemeniz gerekebilir. Yük Dengeleyici'ı tıklatın.

1. Yük Dengeleyici dikey penceresinde **arka uç havuzları**.

1. Tıklatın **+ Ekle** bir arka uç havuzu eklemek için.

1. Arka uç havuzu için bir ad yazın.

1. Tıklatın **bir sanal makine Ekle**.

1. Üzerinde **sanal makineleri seçin** dikey penceresinde tıklatın **bir kullanılabilirlik kümesi seçin**.

1. SQL Server sanal makinelere yerleştirilen kullanılabilirlik kümesi'ni seçin.

1. Üzerinde **sanal makineleri seçin** dikey penceresinde tıklatın **sanal makineleri seçin**.

   Azure portalı, aşağıdaki resimde gibi görünmelidir:

   ![CreateLoadBalancerBackEnd](./media/virtual-machines-windows-portal-sql-create-failover-cluster/33-load-balancer-back-end.png)

1. Tıklatın **seçin** üzerinde **sanal makineleri seçin** dikey.

1. Tıklatın **Tamam** iki kez.

### <a name="configure-a-load-balancer-health-probe"></a>Bir yük dengeleyici durum araştırması yapılandırın

1. Yük Dengeleyici dikey penceresinde **sistem durumu araştırmalarının**.

1. Tıklatın **+ Ekle**.

1. Üzerinde **Ekle durumu araştırması** dikey penceresinde <a name="probe"> </a>sistem araştırma parametreleri ayarlayın:

   - **Ad**: durumu araştırması için bir ad.
   - **Protokol**: TCP.
   - **Bağlantı noktası**: bir kullanılabilir TCP bağlantı noktasına ayarlayın. Bu bağlantı noktası açık güvenlik duvarı bağlantı noktası gerektirir. Kullanım [aynı bağlantı noktasını](#ports) sistem durumu araştırması için güvenlik duvarında ayarlayın.
   - **Aralığı**: 5 saniye.
   - **Sağlıksız eşik**: 2 art arda hatalar.

1. Tamam'a tıklayın.

### <a name="set-load-balancing-rules"></a>Yük Dengeleme kuralları ayarlama

1. Yük Dengeleyici dikey penceresinde **Yük Dengeleme kuralları**.

1. Tıklatın **+ Ekle**.

1. Yük Dengeleme kuralları parametreleri ayarlayın:

   - **Ad**: Yük Dengeleme kuralları için bir ad.
   - **Ön uç IP adresi**: SQL Server FCI küme ağ kaynağı için IP adresini kullanın.
   - **Bağlantı noktası**: SQL Server FCI TCP bağlantı noktası için ayarlanmış. Varsayılan örneği bağlantı noktası 1433'tür.
   - **Arka uç bağlantı noktası**: aynı bağlantı noktası olarak bu değeri kullanır **bağlantı noktası** değeri, etkinleştirdiğinizde **kayan IP (doğrudan sunucu dönüşü)**.
   - **Arka uç havuzu**: daha önce yapılandırılmış arka uç havuzu adı kullanın.
   - **Sistem durumu araştırma**: daha önce yapılandırılmış durumu araştırması kullanın.
   - **Oturum kalıcılığı**: yok.
   - **Boşta kalma zaman aşımı (dakika)**: 4.
   - **Kayan IP (doğrudan sunucu dönüşü)**: etkin

1. **Tamam**’a tıklayın.

## <a name="step-6-configure-cluster-for-probe"></a>Adım 6: küme araştırma için yapılandırın

Küme araştırma port parametresi PowerShell'de ayarlayın.

Küme araştırma bağlantı noktası parametresini ayarlamak için aşağıdaki komut dosyası değişkenleri ortamınız değerlerle güncelleştirin. Köşeli kaldırmak `<>` komut. 

   ```PowerShell
   $ClusterNetworkName = "<Cluster Network Name>"
   $IPResourceName = "<SQL Server FCI IP Address Resource Name>" 
   $ILBIP = "<n.n.n.n>" 
   [int]$ProbePort = <nnnnn>

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

Yukarıdaki komut dosyasında, ortamınız için değerleri ayarlayın. Aşağıdaki liste değerleri açıklar:

   - `<Cluster Network Name>`: Ağ için Windows Server Yük devretme kümesi adı. İçinde **yük devretme kümesi Yöneticisi** > **ağlar**, ağ üzerinde sağ tıklatın ve **özellikleri**. Doğru değeri altındadır **adı** üzerinde **genel** sekmesi. 

   - `<SQL Server FCI IP Address Resource Name>`: SQL Server FCI IP adresi kaynak adı. İçinde **yük devretme kümesi Yöneticisi** > **rolleri**, SQL Server FCI rolü altında altında **sunucu adı**, IP adresi kaynağı sağ tıklayın ve tıklatın**Özellikleri**. Doğru değeri altındadır **adı** üzerinde **genel** sekmesi. 

   - `<ILBIP>`: ILB'nin IP adresi. Bu adres ILB uç adresi olarak Azure portalında yapılandırılır. Bu aynı zamanda SQL Server FCI IP adresidir. İçinde bulabileceğiniz **yük devretme kümesi Yöneticisi** nerede aynı özellikleri sayfasında `<SQL Server FCI IP Address Resource Name>`.  

   - `<nnnnn>`: Yük dengeleyici durum araştırması yapılandırılmış yoklama bağlantı noktasıdır. Kullanılmayan tüm TCP bağlantı noktası geçerli değil. 

>[!IMPORTANT]
>Küme parametresi için alt ağ maskesi TCP IP yayın adresi olması gerekir: `255.255.255.255`.

Küme araştırma ayarladıktan sonra tüm küme parametreleri PowerShell'de görebilirsiniz. Şu betiği çalıştırın:

   ```PowerShell
   Get-ClusterResource $IPResourceName | Get-ClusterParameter 
  ```

## <a name="step-7-test-fci-failover"></a>7. adım: Test FCI yük devretme

Yük devretme küme işlevselliğini doğrulamak için FCI. Aşağıdaki adımları uygulayın:

1. RDP ile SQL Server FCI küme düğümlerinden birinin bağlayın.

1. Açık **yük devretme kümesi Yöneticisi**. Tıklatın **rolleri**. SQL Server FCI rol sahibi olan düğümü dikkat edin.

1. SQL Server FCI role sağ tıklayın.

1. Tıklatın **taşıma** tıklatıp **olası iyi düğüme**.

**Yük Devretme Kümesi Yöneticisi** rolü ve kaynaklarını çevrimdışı duruma gösterir. Kaynakları taşıma ve başka bir düğümde çevrimiçi olması.

### <a name="test-connectivity"></a>Bağlantıyı test etme

Bağlantıyı sınamak için aynı sanal ağda başka bir sanal makinede oturum açın. Açık **SQL Server Management Studio** ve SQL Server FCI adına bağlanın.

>[!NOTE]
>Gerekirse, aşağıdakileri yapabilirsiniz, [SQL Server Management Studio'yu indirme](http://msdn.microsoft.com/library/mt238290.aspx).

## <a name="limitations"></a>Sınırlamalar
Azure sanal makinelerde RPC bağlantı noktası, yük dengeleyici tarafından desteklenmediği için Microsoft Dağıtılmış İşlem Düzenleyicisi (DTC) Fcı'lerde üzerinde desteklenmiyor.

## <a name="see-also"></a>Ayrıca Bkz.

[Uzak Masaüstü'nü (Azure) S2D Kurulumu](http://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-storage-spaces-direct-deployment)

[Depolama alanları doğrudan ile çözüm Hyper yakınsanmış](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct).

[Depolama alanına doğrudan genel bakış](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview)

[S2D için SQL Server desteği](https://blogs.technet.microsoft.com/dataplatforminsider/2016/09/27/sql-server-2016-now-supports-windows-server-2016-storage-spaces-direct/)
