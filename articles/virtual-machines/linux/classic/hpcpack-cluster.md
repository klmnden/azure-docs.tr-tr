---
title: "Linux sanal makineleri bir HPC paketi küme işlem | Microsoft Docs"
description: "Oluşturma ve Linux yüksek performanslı bilgi işlem (HPC) iş yükleri için Azure'da bir HPC Pack kümesi kullanma hakkında bilgi edinin"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: 4d080fdd-5ffe-4f54-a78d-4c818f6eb3fb
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 10/12/2016
ms.author: danlep
ms.openlocfilehash: 809d3944311badf265117d353b65642e044d900c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-linux-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>Azure’daki bir HPC Pack kümesinde Linux işlem düğümleri kullanmaya başlama
Ayarlanmış bir [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029.aspx) küme Windows Server ve birkaç çalıştıran bir baş düğüm içeren Azure işlem desteklenen Linux dağıtım çalışan düğümleri. Linux düğümleri ve kümenin Windows baş düğüm arasında verileri taşımak için seçeneklerini araştırın. Kümeye Linux HPC iş gönderme öğrenin.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

Yüksek düzeyde, aşağıdaki diyagramda HPC paketi küme oluşturun ve çalışmak gösterir.

![Linux düğümleri ile HPC Pack kümesi][scenario]

Diğer Seçenekler Linux HPC iş yüklerini Azure'da çalışması, bkz: [toplu işlem ve yüksek performanslı bilgi işlem için teknik kaynaklar](../../../batch/big-compute-resources.md).

## <a name="deploy-an-hpc-pack-cluster-with-linux-compute-nodes"></a>Linux işlem düğümlerini içeren bir HPC Pack kümeyi dağıtma
Bu makale, Azure Linux HPC Pack kümede dağıtmak için iki seçenek işlem düğümleri gösterir. Her iki yöntem Windows Server'ın bir Market görüntüsü baş düğüm oluşturmak için HPC Pack ile kullanın. 

* **Azure Resource Manager şablonu** -Resource Manager dağıtım modelinde kümenin oluşturmayı otomatikleştirmek için bir şablonu Azure Marketi'nden veya topluluktan hızlı başlatma şablonunu kullanın. Örneğin, [Linux iş yükleri için HPC paketi küme](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) Azure Marketi şablonunda Linux HPC için eksiksiz bir HPC paketi küme altyapı iş yükleri oluşturur.
* **PowerShell Betiği** -kullanım [Microsoft HPC Pack Iaas dağıtım betiği](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (**yeni HpcIaaSCluster.ps1**) Klasik dağıtım modelinde bir tam küme dağıtımı otomatik hale getirmek için. Bu Azure PowerShell Betiği hızlı dağıtımı için Azure Marketi'nde bir HPC Pack VM görüntüsü kullanır ve Linux işlem düğümlerini dağıtmak için yapılandırma parametrelerini kapsamlı bir kümesini sağlar.

Azure içindeki HPC paketi küme dağıtım seçenekleri hakkında daha fazla bilgi için bkz: [küme oluşturmak ve yüksek performanslı hesaplama (HPC) yönetmek için seçenekleri Microsoft HPC Pack ile azure'da](../hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="prerequisites"></a>Ön koşullar
* **Azure aboneliği** -Azure genel veya Azure Çin hizmetinde bir aboneliği kullanabilirsiniz. Bir hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.
* **Çekirdek kota** -özellikle çok çekirdekli VM boyutları birkaç küme düğümleri dağıtmak isterseniz, çekirdek, Kotayı artırmak gerekebilir. Bir Kotayı artırmak için ücretsiz bir çevrimiçi müşteri destek isteği açın.
* **Linux dağıtımları** -şu anda HPC Pack aşağıdaki Linux dağıtımları için işlem düğümlerine destekler. Bu dağıtımları Market sürümleri kullanılabiliyorsa kullanın veya kendi sağlayın.
  
  * **CentOS tabanlı**: 6.5, 6.6, 6.7, 7.0, 7.1, 7.2, 6.5 HPC, 7.1 HPC
  * **Red Hat Enterprise Linux**: 6.7, 6,8, 7.2
  * **SUSE Linux Enterprise Server**: SLES 12, SLES 12 (Premium), SLES 12 SP1, SLES 12 SP1 (Premium), SLES 12 HPC, SLES 12 HPC (Premium) için için
  * **Ubuntu Server**: 14.04 LTS, 16.04 LTS
    
    > [!TIP]
    > Azure RDMA ağ RDMA özelliğine sahip VM boyutları biri ile kullanmak için Azure Marketi'nden bir SUSE Linux Enterprise Server 12 HPC veya CentOS tabanlı HPC görüntüsü belirtin. Daha fazla bilgi için bkz: [yüksek performanslı işlem VM boyutları](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
    > 
    > 

HPC Pack Iaas dağıtım komut dosyası kullanarak küme dağıtmak için ek önkoşulları:

* **İstemci bilgisayar** -küme dağıtım betiğini çalıştırmak için bir Windows tabanlı bir istemci bilgisayar gerekir.
* **Azure PowerShell** - [yükleyin ve Azure PowerShell yapılandırma](/powershell/azure/overview) (sürüm 0.8.10 veya üzeri) istemci bilgisayarınızda.
* **HPC Pack Iaas dağıtım betiği** - karşıdan yükleme ve komut dosyasını en son sürümünü paket [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Komut dosyasının sürümünü çalıştırarak denetleyebilirsiniz `.\New-HPCIaaSCluster.ps1 –Version`. Bu makalede, sürüm 4.4.1 veya daha sonra komut dosyasını temel alır.

### <a name="deployment-option-1-use-a-resource-manager-template"></a>Dağıtım seçeneği 1. Resource Manager şablonu kullanın
1. Git [Linux iş yükleri için HPC paketi küme](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) Azure Marketi ve tıklatın şablonunda **dağıtma**.
2. Azure Portalı'ndaki bilgileri gözden geçirin ve ardından **oluşturma**.
   
    ![Portal oluşturma][portal]
3. Üzerinde **Temelleri** dikey penceresinde de üstbilgi düğüm VM'ine adları küme için bir ad girin. Varolan bir kaynak grubu seçin veya bir grup dağıtımı için kullanabileceğiniz bir konumda oluşturun. Konumun belirli VM boyutları ve diğer Azure hizmetleriyle kullanılabilirliğini etkiler (bkz [bölgeye göre ürünleri](https://azure.microsoft.com/regions/services/)).
4. Üzerinde **düğümü ayarları Head** dikey penceresinde, ilk dağıtım için genellikle varsayılan ayarları kabul edebilirsiniz. 
   
   > [!NOTE]
   > **Sonrası yapılandırma betiği URL'si** çalışmaya başladıktan sonra baş düğümünde VM çalıştırmak istediğiniz genel kullanıma açık bir Windows PowerShell komut dosyasını belirtmek için isteğe bağlı bir ayardır. 
   > 
   > 
5. Üzerinde **düğümü ayarları işlem** bir adlandırma deseni dikey penceresinde, select düğümleri, sayısını ve boyutunu düğümleri ve Linux dağıtım dağıtmak.
6. Üzerinde **Altyapı ayarlarını** dikey penceresinde, sanal ağ ve Active Directory adlarını girin etki alanı, etki alanı ve VM yönetici kimlik bilgileri ve depolama hesapları için bir adlandırma deseni.
   
   > [!NOTE]
   > HPC Pack Active Directory etki alanı küme kullanıcıların kimliklerini doğrulamak için kullanır. 
   > 
   > 
7. Doğrulama testlerini çalıştırmak ve kullanım koşullarını gözden geçirin sonra tıklayın **satın alma**.

### <a name="deployment-option-2-use-the-iaas-deployment-script"></a>Dağıtım seçeneği 2. Iaas dağıtım komut dosyası kullan
HPC Pack Iaas dağıtım komut dosyası kullanarak küme dağıtmak için ek ön koşullar aşağıda verilmiştir:

* **İstemci bilgisayar** -küme dağıtım betiğini çalıştırmak için bir Windows tabanlı bir istemci bilgisayar gerekir.
* **Azure PowerShell** - [yükleyin ve Azure PowerShell yapılandırma](/powershell/azure/overview) (sürüm 0.8.10 veya üzeri) istemci bilgisayarınızda.
* **HPC Pack Iaas dağıtım betiği** - karşıdan yükleme ve komut dosyasını en son sürümünü paket [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Komut dosyasının sürümünü çalıştırarak denetleyebilirsiniz `.\New-HPCIaaSCluster.ps1 –Version`. Bu makalede, sürüm 4.4.1 veya daha sonra komut dosyasını temel alır.

**XML yapılandırma dosyası**

HPC Pack Iaas dağıtım betiği HPC küme açıklamak için giriş olarak bir XML yapılandırma dosyasını kullanır. Aşağıdaki örnek yapılandırma dosyası bir HPC paketi üstbilgi düğümü ve iki boyutu A7 CentOS 7.0 Linux işlem düğümleri oluşan küçük bir küme belirtir. 

Dosya ortamınız ve istenen küme yapılandırması için gereken şekilde değiştirin ve HPCDemoConfig.xml gibi bir adla kaydedin. Örneğin, abonelik adı ve benzersiz depolama hesabı adı girin ve hizmet adı bulut gerekir. Ayrıca, işlem düğümleri için farklı bir desteklenen Linux görüntüsü seçin isteyebilirsiniz. Yapılandırma dosyasındaki öğeler hakkında daha fazla bilgi için komut dosyası klasöründeki Manual.rtf dosyasına bakın ve [HPC Kümesi ile HPC Pack Iaas dağıtım komut dosyası oluşturma](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>centos7rdmavnetje</VNetName>
    <SubnetName>CentOS7RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS7RDMA-HN</VMName>
    <ServiceName>centos7rdma-je</ServiceName>
  <VMSize>ExtraLarge</VMSize>
  <EnableRESTAPI />
  <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS7RDMA-LN%1%</VMNamePattern>
    <ServiceName>centos7rdma-je</ServiceName>
    <VMSize>A7</VMSize>
    <NodeCount>2</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```

**HPC Pack Iaas dağıtım betiğini çalıştırmak için**

1. Windows PowerShell istemci bilgisayarda yönetici olarak açın.
2. Değişiklik komut dosyasının olduğu klasöre directory (Bu örnekte E:\IaaSClusterScript) yüklü.
   
    ```powershell
    cd E:\IaaSClusterScript
    ```
3. HPC Pack küme dağıtmak için aşağıdaki komutu çalıştırın. Bu örnekte, yapılandırma dosyası E:\HPCDemoConfig.xml içinde bulunduğu varsayılmaktadır.
   
    ```powershell
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```
   
    a. Çünkü **Admınpassword** önceki komutta, sorulur kullanıcı için parolayı girmek için belirtilmemiş *MyAdminName*.
   
    b. Yapılandırma dosyasını doğrulamak komut dosyası sonra başlar. Bu ağ bağlantısı bağlı olarak birkaç dakika sürebilir.
   
    ![Doğrulama][validate]
   
    c. Doğrulama geçirmek sonra komut dosyasını oluşturmak için küme kaynaklarını listeler. Girin *Y* devam etmek için.
   
    ![Kaynaklar][resources]
   
    d. Komut dosyası HPC paketi küme dağıtmak başlar ve daha fazla el ile adımlar olmadan yapılandırmasını tamamlar. Komut dosyası için birkaç dakika çalışabilir.
   
    ![Dağıtma][deploy]
   
   > [!NOTE]
   > Otomatik olarak beri bir günlük dosyası bu örnekte, komut dosyasını oluşturur **- günlük dosyası** parametresinde değil. Günlükler gerçek zamanlı olarak yazılmış değil, ancak doğrulama ve dağıtımını sonunda toplanır. Komut dosyası çalışırken PowerShell işlem durursa, bazı günlükleri kaybolur.
   > 
   > 

## <a name="connect-to-the-head-node"></a>Baş düğümüne bağlanmak
Azure, HPC Pack kümede dağıttıktan sonra [tarafından Uzak Masaüstü Bağlantısı](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) kümeyi dağıttığınızda sağlanan üstbilgi düğüm VM'ine etki alanını kullanarak, kimlik bilgileri (örneğin, *hpc\\clusteradmin*). Küme baş düğümünden yönetin.

Baş düğümünde HPC paketi küme durumunu denetlemek için HPC Küme Yöneticisi'ni başlatın. Linux işlem düğümlerini Windows ile çalışacak şekilde İzleyici işlem düğümlerini ve, yönetebilirsiniz. Örneğin, listelenen Linux düğümleri bkz **kaynak yönetimi** (Bu düğümler dağıtılan **LinuxNode** şablonu).

![Düğüm Yönetimi][management]

Ayrıca Linux düğümleri bkz **ısı Haritası** görünümü.

![Isı Haritası][heatmap]

## <a name="how-to-move-data-in-a-cluster-with-linux-nodes"></a>Linux düğümleri ile bir kümede veri taşıma
Linux düğümleri ve kümenin Windows baş düğüm arasında verileri taşımak için birkaç seçeneğiniz vardır. Aşağıdaki bölümlerde daha ayrıntılı açıklanan üç yaygın yöntemleri şunlardır:

* **Azure dosya** -Azure depolama alanında veri dosyalarını depolamak için yönetilen bir SMB dosya paylaşımı kullanıma sunar. Farklı sanal ağlarda dağıtmış olsa bile Windows düğümleri ve Linux düğümleri bir Azure dosya paylaşımı bir sürücü veya klasöre aynı anda bağlayabilir.
* **Baş düğüm SMB paylaşımı** -Linux düğümleri üzerinde Windows standart bir paylaşılan klasör baş düğümü bağlar.
* **Baş düğüm NFS sunucusu** -karma Windows ve Linux ortamı için bir dosya paylaşımı çözümü sağlar.

### <a name="azure-file-storage"></a>Azure dosya depolama
[Azure dosya](https://azure.microsoft.com/services/storage/files/) hizmetini standart SMB 2.1 protokolünü kullanan dosya paylaşımları sunar. Azure VM'ler ve cloud services bağlı paylaşımlar üzerinden uygulama bileşenleri arasında dosya verileri paylaşabilir ve şirket içi uygulamalara File storage API'si dosya verilerine erişebilir. 

Bir Azure dosya paylaşımı oluşturmak ve baş düğümünde bağlamak ayrıntılı adımlar için bkz: [Windows Azure File storage ile çalışmaya başlama](../../../storage/files/storage-how-to-use-files-windows.md). Azure dosya paylaşımını Linux düğümlerinde bağlama için bkz: [Azure File storage'ı Linux ile kullanma konusunda](../../../storage/files/storage-how-to-use-files-linux.md). Kalıcı bağlantıları kurmak için bkz: [Persisting bağlantıları Microsoft Azure dosyaları](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx).

Aşağıdaki örnekte, bir Azure dosya paylaşımı bir depolama hesabı oluşturun. Baş düğümünde ve paylaşımı bağlamak için bir komut istemi açın ve aşağıdaki komutları girin:

```command
cmdkey /add:allvhdsje.file.core.windows.net /user:allvhdsje /pass:<storageaccountkey>

net use Z: \\allvhdje.file.core.windows.net\rdma /persistent:yes
```

Bu örnekte, depolama hesabınızın adını allvhdsje olduğundan, depolama alanı hesabı anahtarınız storageaccountkey ise ve rdma Azure dosya paylaşım adıdır. Azure dosya paylaşımı Z: baş düğümünde bağlı.

Azure dosya paylaşımını Linux düğümleri üzerinde bağlama için çalıştırın bir **clusrun** baş düğüm komutu. **[Clusrun](https://technet.microsoft.com/library/cc947685.aspx)**  birden çok düğüm üzerinde yönetim görevlerini gerçekleştirmek için yararlı bir HPC Pack araçtır. (Ayrıca bkz. [Clusrun Linux düğümleri için](#Clusrun-for-Linux-nodes) bu makalede.)

Bir Windows PowerShell penceresi açın ve aşağıdaki komutları girin:

```powershell
clusrun /nodegroup:LinuxNodes mkdir -p /rdma

clusrun /nodegroup:LinuxNodes mount -t cifs //allvhdsje.file.core.windows.net/rdma /rdma -o vers=2.1`,username=allvhdsje`,password=<storageaccountkey>'`,dir_mode=0777`,file_mode=0777
```

İlk komut LinuxNodes grubundaki tüm düğümlerde /rdma adlı bir klasör oluşturur. İkinci komut dir /rdma klasörüyle üzerine Azure dosya paylaşımı allvhdsjw.file.core.windows.net/rdma bağlar ve dosya modu BITS 777 ayarlayın. İkinci komut içinde allvhdsje depolama hesabınızın adını ve storageaccountkey depolama hesabı anahtarınızı.

> [!NOTE]
> "\`" İkinci komut dosyasındaki simge olan bir PowerShell kaçış simgesi. "\`," "," (virgül karakteri) komutun bir parçası olduğu anlamına gelir.
> 
> 

### <a name="head-node-share"></a>Baş düğüm paylaşımı
Alternatif olarak, paylaşılan bir klasöre baş düğümü Linux düğümleri üzerinde bağlayın. Bir paylaşımı, dosyaları paylaşmak için en basit yolu sağlar, ancak baş düğüm ve tüm Linux düğümler aynı sanal ağda dağıtılmalıdır. Adımlar şunlardır.

1. Baş düğüm üzerinde bir klasör oluşturun ve herkese okuma/yazma izinleri ile paylaşın. Örneğin, baş düğüm D:\OpenFOAM paylaşmak \\CentOS7RDMA HN\OpenFOAM. Burada CentOS7RDMA HN baş düğüm barındırıcı adıdır.
   
    ![Dosya paylaşım izinleri][fileshareperms]
   
    ![Dosya Paylaşımı][filesharing]
2. Bir Windows PowerShell penceresi açın ve aşağıdaki komutları çalıştırın:
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS7RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

İlk komut LinuxNodes grubundaki tüm düğümlerde /openfoam adlı bir klasör oluşturur. İkinci komut dir klasörüyle üzerine paylaşılan klasör //CentOS7RDMA-HN/OpenFOAM bağlar ve dosya modu BITS 777 ayarlayın. Kullanıcı adı ve parola komut kullanıcı adı ve parola bir küme kullanıcının baş düğüm olmalıdır. (Bkz [ekleme veya kaldırma küme kullanıcılar](https://technet.microsoft.com/library/ff919330.aspx).)

> [!NOTE]
> "\`" İkinci komut dosyasındaki simge olan bir PowerShell kaçış simgesi. "\`," "," (virgül karakteri) komutun bir parçası olduğu anlamına gelir.
> 
> 

### <a name="nfs-server"></a>NFS sunucusu
NFS hizmeti paylaşma ve dosyaları SMB protokolünü kullanan Windows Server 2012 işletim sistemi çalıştıran bilgisayarlarda ve NFS protokolünü kullanarak Linux tabanlı bilgisayarlar arasında geçirmek etkinleştirir. NFS sunucusu ve diğer tüm düğümlere aynı sanal ağda dağıtılmış gerekir. Linux düğümleriyle SMB paylaşımı ile karşılaştırıldığında daha iyi uyumluluk sağlar. Örneğin, dosya bağlantıları destekler.

1. Yüklemek ve bir NFS sunucusu kurmak için adımları [sunucusu için ağ dosya sistemi ilk paylaşımını uçtan uca](http://blogs.technet.com/b/filecab/archive/2012/10/08/server-for-network-file-system-first-share-end-to-end.aspx).
   
    Örneğin, aşağıdaki özelliklere sahip nfs adlı bir NFS paylaşımına oluşturun:
   
    ![NFS yetkilendirme][nfsauth]
   
    ![NFS paylaşım izinlerini][nfsshare]
   
    ![NFS NTFS izinleri][nfsperm]
   
    ![NFS yönetim özellikleri][nfsmanage]
2. Bir Windows PowerShell penceresi açın ve aşağıdaki komutları çalıştırın:
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /nfsshare
   
    clusrun /nodegroup:LinuxNodes mount CentOS7RDMA-HN:/nfs /nfsshared
    ```
   
   İlk komut LinuxNodes grubundaki tüm düğümlerde /nfsshared adlı bir klasör oluşturur. İkinci komut bir NFS paylaşımına CentOS7RDMA HN bağlar: / klasör üzerine nfs. Burada CentOS7RDMA HN: / nfs NFS paylaşımınızın uzak yoludur.

## <a name="how-to-submit-jobs"></a>İşlerini gönderme
HPC Pack kümeye iş göndermek için birkaç yolu vardır:

* HPC Küme Yöneticisi'ni veya HPC İş Yöneticisi GUI
* HPC web portalı
* REST API

HPC Pack GUI araçları ve HPC web Portalı aracılığıyla Azure kümeye iş gönderme olan Windows aynı işlem düğümleri. Bkz: [HPC Pack İş Yöneticisi](https://technet.microsoft.com/library/ff919691.aspx) ve [bir şirket içi istemci bilgisayarından işlerini göndermek nasıl](../../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

REST API aracılığıyla iş göndermek için bkz [oluşturma ve gönderme Microsoft HPC Pack REST API kullanarak işleri](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx). Bir Linux istemciden işlerini göndermek için de Python örnekte bkz [HPC Pack SDK](https://www.microsoft.com/download/details.aspx?id=47756).

## <a name="clusrun-for-linux-nodes"></a>Linux düğümleri için Clusrun
HPC Pack [clusrun](https://technet.microsoft.com/library/cc947685.aspx) aracı, Linux düğümleri ya da bir komut istemini veya HPC Küme Yöneticisi aracılığıyla komut yürütmek için kullanılabilir. Temel bazı örnekler verilmiştir.

* Kümedeki tüm düğümlerde geçerli kullanıcı adlarını gösterir.
  
    ```command
    clusrun whoami
    ```
* Yükleme **gdb** hata ayıklayıcısı aracı ile **yum** tüm linuxnodes düğümler grup ve ardından düğümler 10 dakika sonra yeniden başlatın.
  
    ```command
    clusrun /nodegroup:linuxnodes yum install gdb –y; shutdown –r 10
    ```
* Kümedeki her numarası 1 ile 10 her Linux düğümde bir saniye için görüntüleme bir kabuk betiği oluşturma çalıştırın ve çıkış düğümlerden hemen göster.
  
    ```command
    clusrun /interleaved /nodegroup:linuxnodes echo \"for i in {1..10}; do echo \\\"\$i\\\"; sleep 1; done\" ^> script.sh; chmod +x script.sh; ./script.sh
    ```

> [!NOTE]
> Belirli kaçış karakterleri gerekebilir **clusrun** komutları. Bu örnekte gösterildiği gibi kullanın ^ kaçınmak için bir komut isteminde ">" simgesi.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* Fazla sayıda düğüm kümeye ölçeklendirme deneyin veya küme üzerinde bir Linux iş yükü çalıştırmayı deneyin. Bir örnek için bkz: [çalıştırmak NAMD Microsoft HPC Pack Linux işlem düğümlerini Azure](hpcpack-cluster-namd.md).
* Bir küme ile deneyin [RDMA özellikli, işlem yoğunluklu VM'ler](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) MPI iş yüklerini çalıştırmak için. Bir örnek için bkz: [çalıştırmak OpenFOAM Linux RDMA üzerinde Microsoft HPC Pack ile küme Azure'da](hpcpack-cluster-openfoam.md).
* İçinde bir şirket içi HPC Pack kümesindeki Linux düğümleri ile çalışma ilgileniyorsanız bkz [TechNet Kılavuzu](https://technet.microsoft.com/library/mt595803.aspx).

<!--Image references-->
[scenario]:media/hpcpack-cluster/scenario.png
[portal]:media/hpcpack-cluster/portal.png
[validate]:media/hpcpack-cluster/validate.png
[resources]:media/hpcpack-cluster/resources.png
[deploy]:media/hpcpack-cluster/deploy.png
[management]:media/hpcpack-cluster/management.png
[heatmap]:media/hpcpack-cluster/heatmap.png
[fileshareperms]:media/hpcpack-cluster/fileshare1.png
[filesharing]:media/hpcpack-cluster/fileshare2.png
[nfsauth]:media/hpcpack-cluster/nfsauth.png
[nfsshare]:media/hpcpack-cluster/nfsshare.png
[nfsperm]:media/hpcpack-cluster/nfsperm.png
[nfsmanage]:media/hpcpack-cluster/nfsmanage.png
