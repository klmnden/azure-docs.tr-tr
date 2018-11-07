---
title: Bir HPC Pack kümesinde Linux işlem Vm'leri | Microsoft Docs
description: Oluşturma ve bir HPC Pack kümesinde Linux yüksek performanslı bilgi işlem (HPC) iş yükleri için Azure'da kullanma hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: 4d080fdd-5ffe-4f54-a78d-4c818f6eb3fb
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 10/12/2016
ms.author: danlep
ms.openlocfilehash: 4156071c36b06be586b05ee98e9eeb0a9138e4bb
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51246869"
---
# <a name="get-started-with-linux-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>Azure’daki bir HPC Pack kümesinde Linux işlem düğümleri kullanmaya başlama
Ayarlanmış bir [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029.aspx) desteklenen bir Linux dağıtımı çalışan düğümleri işlem kümesi azure'da Windows Server ve birkaç çalıştıran bir baş düğüm içerir. Linux düğümleri ve kümenin Windows baş düğüm arasında verileri taşımak için seçenekleri keşfedin. Kümeye Linux HPC iş gönderme hakkında bilgi edinin.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

Yüksek düzeyde, aşağıdaki diyagramda HPC Pack küme oluşturmak ve çalışmak gösterir.

![HPC Pack kümesinde Linux düğümleri ile][scenario]

Linux HPC iş yüklerini Azure'da çalıştırmak diğer seçenekler için bkz. [toplu işlem ve yüksek performanslı bilgi işlem için teknik kaynaklar](../../../batch/big-compute-resources.md).

## <a name="deploy-an-hpc-pack-cluster-with-linux-compute-nodes"></a>Bir HPC Pack kümesinde Linux işlem düğümleri ile dağıtma
Bu makalede, azure'da Linux ile bir HPC Pack kümesine dağıtmak için iki seçenek işlem düğümleri gösterir. Her iki yöntem de, baş düğümünü oluşturma için HPC Pack ile Windows Server'ın bir Market görüntüsü kullanın. 

* **Azure Resource Manager şablonu** -Resource Manager dağıtım modelinde kümenin oluşturmayı otomatikleştirmek için Azure Market'teki bir şablonu veya topluluğundan Hızlı Başlangıç şablonu kullanın. Örneğin, [HPC Pack kümesinde Linux iş yükleri için](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) şablonu Azure marketi'ndeki Linux HPC için eksiksiz bir HPC Pack küme altyapı iş yükleri oluşturur.
* **PowerShell Betiği** -kullanım [Microsoft HPC Pack Iaas dağıtım betiği](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (**yeni HpcIaaSCluster.ps1**) Klasik dağıtım modelinde bir tam küme dağıtımını otomatikleştirmek için. Bu Azure PowerShell Betiği, bir HPC Pack VM görüntüsü, hızlı dağıtım için Azure Marketi'nde kullanır ve kapsamlı Linux işlem düğümlerini dağıtmak için yapılandırma parametrelerini sağlar.

Azure'da HPC Pack küme dağıtım seçenekleri hakkında daha fazla bilgi için bkz. [kümesi oluşturmak ve yüksek performanslı hesaplama (HPC) yönetmek için seçenekleri Microsoft HPC Pack ile azure'da](../hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="prerequisites"></a>Önkoşullar
* **Azure aboneliği** -Azure Global veya Azure China hizmetinde bir aboneliği kullanabilirsiniz. Bir hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.
* **Çekirdek kota** -özellikle birden fazla çekirdekli VM boyutlarının birden çok küme düğümüyle dağıtmayı seçerseniz, çekirdek kotasını artırmanız gerekebilir. Kotayı artırmak için ücretsiz bir çevrimiçi müşteri destek isteği açın.
* **Linux dağıtımları** -şu anda HPC Pack, işlem düğümleri için aşağıdaki Linux dağıtımları destekler. Uygun olduğu durumlarda bu dağıtımları Market sürümlerini kullanın veya kendi sağlayın.
  
  * **CentOS tabanlı**: 6.5, 6.6, 6.7, 7.0, 7.1, 7.2, 6.5 HPC, 7.1 HPC
  * **Red Hat Enterprise Linux**: 6.7 6,8, 7.2
  * **SUSE Linux Enterprise Server**: SLES 12, SLES 12 (Premium), SLES 12 SP1, SLES 12 SP1 (Premium) için HPC, SLES 12 HPC (Premium) için SLES 12
  * **Ubuntu Server**: 14.04 LTS, 16.04 LTS
    
    > [!TIP]
    > RDMA özellikli bir VM boyutlarını biriyle Azure RDMA ağ kullanmak için Azure Market'ten bir SUSE Linux Enterprise Server 12 HPC veya CentOS tabanlı HPC görüntüsü belirtin. Daha fazla bilgi için [yüksek performanslı işlem VM boyutları](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
    > 
    > 

HPC Pack Iaas dağıtım betiği kullanarak kümeyi dağıtmak için ek Önkoşullar:

* **İstemci bilgisayar** -küme dağıtım betiğini çalıştırmak için bir Windows tabanlı bir istemci bilgisayara ihtiyacı vardır.
* **Azure PowerShell** - [yüklemek ve Azure PowerShell yapılandırma](/powershell/azure/overview) (0.8.10 sürümü veya üzeri) istemci bilgisayarınızda.
* **HPC Pack Iaas dağıtım betiği** : indirin ve komut dosyasından en son sürümünü paketten [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Komut dosyasının sürümünü çalıştırarak denetleyebilirsiniz `.\New-HPCIaaSCluster.ps1 –Version`. Bu makalede, sürüm 4.4.1 veya daha sonra komut dosyasını temel alır.

### <a name="deployment-option-1-use-a-resource-manager-template"></a>Dağıtım seçeneği 1. Resource Manager şablonu kullanma
1. Git [HPC Pack kümesinde Linux iş yükleri için](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) şablonu Azure Market'ten seçeneğine tıklayıp **Dağıt**.
1. Azure Portalı'ndaki bilgileri gözden geçirin ve ardından **Oluştur**.
   
    ![Portal oluşturma][portal]
1. Üzerinde **Temelleri** dikey penceresinde, üstbilgi düğüm VM'ine da adlar kümesi için bir ad girin. Mevcut bir kaynak grubu seçin veya bir konumda kullanılabilir dağıtım için bir grup oluşturun. Konumu belirli VM boyutları ve diğer Azure Hizmetleri kullanılabilirliğini etkileyen (bkz [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/regions/services/)).
1. Üzerinde **baş düğüm ayarları** dikey penceresinde, ilk dağıtım için genellikle varsayılan ayarları kabul edebilirsiniz. 
   
   > [!NOTE]
   > **Yapılandırma sonrası betik URL'si** üstbilgi düğüm VM'ine çalışmaya başladıktan sonra çalıştırmak istediğiniz genel kullanıma açık bir Windows PowerShell komut dosyasını belirtmek için isteğe bağlı bir ayardır. 
   > 
   > 
1. Üzerinde **düğüm ayarları işlem** bir adlandırma deseni dikey penceresinde düğümleri, sayısını ve boyutunu düğümleri ve Linux dağıtım dağıtılacak.
1. Üzerinde **Altyapı ayarlarını** dikey penceresinde, sanal ağ ve Active Directory adlarını girin etki alanı, etki alanı ve Sanal Makine Yöneticisi kimlik bilgileri ve depolama hesapları için bir adlandırma deseni.
   
   > [!NOTE]
   > HPC Pack, küme kullanıcıların kimliğini doğrulamak için Active Directory etki alanı kullanır. 
   > 
   > 
1. Doğrulama testlerini çalıştırmak ve kullanım koşullarını gözden sonra tıklayın **satın alma**.

### <a name="deployment-option-2-use-the-iaas-deployment-script"></a>Dağıtım seçeneği 2. Iaas dağıtım betiği kullanın
HPC Pack Iaas dağıtım betiği kullanarak kümeyi dağıtmak için ek Önkoşullar şunlardır:

* **İstemci bilgisayar** -küme dağıtım betiğini çalıştırmak için bir Windows tabanlı bir istemci bilgisayara ihtiyacı vardır.
* **Azure PowerShell** - [yüklemek ve Azure PowerShell yapılandırma](/powershell/azure/overview) (0.8.10 sürümü veya üzeri) istemci bilgisayarınızda.
* **HPC Pack Iaas dağıtım betiği** : indirin ve komut dosyasından en son sürümünü paketten [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Komut dosyasının sürümünü çalıştırarak denetleyebilirsiniz `.\New-HPCIaaSCluster.ps1 –Version`. Bu makalede, sürüm 4.4.1 veya daha sonra komut dosyasını temel alır.

**XML yapılandırma dosyası**

HPC Pack Iaas dağıtım betiği, HPC kümesi tanımlamak için giriş olarak bir XML yapılandırma dosyasını kullanır. Aşağıdaki örnek yapılandırma dosyası bir HPC paketi üstbilgi düğümü ve iki boyutu A7 CentOS 7.0 Linux işlem düğümleri oluşan küçük bir küme belirtir. 

Ortamı ve istenen küme yapılandırması için gerektiği şekilde dosyayı değiştirebilir ve HPCDemoConfig.xml gibi bir adla kaydedin. Örneğin, bulut hizmeti adı ve abonelik adınız ve benzersiz depolama hesabı adı sağlamanız gerekir. Buna ek olarak, işlem düğümleri için farklı bir desteklenen Linux görüntüsünü seçin isteyebilirsiniz. Betik klasöründeki Manual.rtf dosya yapılandırma dosyasında öğeler hakkında daha fazla bilgi için bkz ve [HPC Pack Iaas dağıtım betiği ile bir HPC kümesi oluşturma](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

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

**HPC Pack Iaas dağıtım betiği çalıştırmak için**

1. Windows PowerShell, istemci bilgisayarda yönetici olarak açın.
1. Değişiklik dizini betiğin bulunduğu klasöre yüklü (Bu örnekte E:\IaaSClusterScript).
   
    ```powershell
    cd E:\IaaSClusterScript
    ```
1. HPC Pack kümesine dağıtmak için aşağıdaki komutu çalıştırın. Bu örnek yapılandırma dosyası içinde E:\HPCDemoConfig.xml bulunduğunu varsayar.
   
    ```powershell
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```
   
    a. Çünkü **AdminPassword** önceki komutta, istenen kullanıcının parolayı girmek için belirtilmemiş *MyAdminName*.
   
    b. Betik daha sonra yapılandırma dosyasını doğrulamak başlatır. Bu ağ bağlantısı bağlı olarak birkaç dakika sürebilir.
   
    ![Doğrulama][validate]
   
    c. Doğrulama başarılı sonra komut dosyası oluşturmak için küme kaynaklarını listeler. Girin *Y* devam etmek için.
   
    ![Kaynaklar][resources]
   
    d. Betik HPC Pack kümesini dağıtmayı başlar ve daha fazla el ile adımlar olmadan yapılandırmasını tamamlar. Komut dosyası için birkaç dakika çalıştırabilirsiniz.
   
    ![Dağıtma][deploy]
   
   > [!NOTE]
   > Otomatik olarak beri bir günlük dosyası bu örnekte, komut dosyasının oluşturduğu **- günlük dosyası** parametresi belirtilmediyse. Günlükler gerçek zamanlı olarak yazılmış değildir, ancak doğrulama ve dağıtımı sonunda toplanır. Betiğin çalıştığı sırada PowerShell işlem durursa, bazı günlükleri kaybolur.
   > 
   > 

## <a name="connect-to-the-head-node"></a>Baş düğümüne bağlanma
Azure'da HPC Pack kümesine dağıttıktan sonra [Uzak Masaüstü ile bağlanın](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) kümeyi dağıttığınızda sağlanan üstbilgi düğüm VM'ine etki alanı, kimlik bilgilerini kullanma (örneğin, *hpc\\clusteradmin*). Küme baş düğümünden yönettiğiniz.

Baş düğümde HPC Pack küme durumunu denetlemek için HPC Küme Yöneticisi'ni başlatın. Yönetebileceğiniz ve İzleyici Linux işlem düğümleri, Windows ile çalışmanıza aynı şekilde işlem düğümleri. Örneğin, listelenen Linux düğümleri görürsünüz **kaynak yönetimi** (Bu düğümler ile dağıtılan **LinuxNode** şablonu).

![Düğüm Yönetimi][management]

Ayrıca Linux düğümleri gördüğünüz **ısı Haritası** görünümü.

![Isı haritası][heatmap]

## <a name="how-to-move-data-in-a-cluster-with-linux-nodes"></a>Bir kümedeki Linux düğümleri ile verileri taşıma
Linux düğümleri ve kümenin Windows baş düğüm arasında verileri taşımak için birkaç seçeneğiniz vardır. Aşağıdaki bölümlerde daha ayrıntılı olarak açıklanan üç yaygın yöntemleri şunlardır:

* **Azure dosya** -Azure depolama alanında veri dosyalarını depolamak için yönetilen bir SMB dosya paylaşımı kullanıma sunar. Farklı sanal ağlarda bulunan dağıtıldıkları bile Windows düğümleri ve Linux düğümleri Azure dosya paylaşımını bir sürücü veya klasörü aynı anda bağlayabilir.
* **Baş düğüm SMB paylaşımı** -Linux düğümlerinde Windows standart bir paylaşılan klasör baş düğümün bağlar.
* **Baş düğüm NFS sunucusu** -karma Windows ve Linux ortamı için bir dosya paylaşım çözümü sağlar.

### <a name="azure-file-storage"></a>Azure dosya depolama
[Azure dosya](https://azure.microsoft.com/services/storage/files/) hizmeti, standart SMB 2.1 protokolünü kullanan dosya paylaşımları sunar. Azure Vm'leri ve cloud services bağlı paylaşımlar üzerinden uygulama bileşenleri arasında dosya verilerini paylaşabilir ve şirket içi uygulamalar dosya depolama API'si paylaşımdaki dosya verilerine erişebilir. 

Azure dosya paylaşımı oluşturmak ve baş düğüme bağlamak ayrıntılı adımlar için bkz. [Windows üzerinde Azure dosya depolama ile çalışmaya başlama](../../../storage/files/storage-how-to-use-files-windows.md). Linux düğümlerinde Azure dosya paylaşımını bağlayabilmeniz için bkz: [Azure dosya depolamayı Linux ile kullanma konusunda](../../../storage/files/storage-how-to-use-files-linux.md). Kalıcı bağlantıları ayarlamak için bkz: [Microsoft Azure dosyaları Persisting bağlantılar](https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx).

Aşağıdaki örnekte, Azure dosya paylaşımını bir depolama hesabı oluşturun. Baş düğüme ve paylaşımı bağlamak için bir komut istemi açın ve aşağıdaki komutları girin:

```command
cmdkey /add:allvhdsje.file.core.windows.net /user:allvhdsje /pass:<storageaccountkey>

net use Z: \\allvhdje.file.core.windows.net\rdma /persistent:yes
```

Bu örnekte, allvhdsje, depolama hesabının adıdır, depolama hesabı anahtarınızı storageaccountkey olan ve rdma Azure dosya paylaşımı adıdır. Azure dosya paylaşımı Z: baş düğüme bağlı.

Linux düğümlerinde Azure dosya paylaşımını bağlayabilmeniz için çalıştırma bir **clusrun** baş düğümde komutu. **[Clusrun](https://technet.microsoft.com/library/cc947685.aspx)**  birden çok düğümde yönetim görevlerini gerçekleştirmek için kullanışlı bir HPC Pack araçtır. (Ayrıca bkz: [Linux düğümleri için Clusrun](#Clusrun-for-Linux-nodes) bu makaledeki.)

Bir Windows PowerShell penceresi açın ve aşağıdaki komutları girin:

```powershell
clusrun /nodegroup:LinuxNodes mkdir -p /rdma

clusrun /nodegroup:LinuxNodes mount -t cifs //allvhdsje.file.core.windows.net/rdma /rdma -o vers=2.1`,username=allvhdsje`,password=<storageaccountkey>'`,dir_mode=0777`,file_mode=0777
```

İlk komut /rdma LinuxNodes grubundaki tüm düğümlerde adlı bir klasör oluşturur. İkinci komut, Azure dosya paylaşımı allvhdsjw.file.core.windows.net/rdma dir /rdma klasörü üzerine bağlar ve dosya modunu BITS 777 ayarlayın. İkinci komut, allvhdsje depolama hesabınızın adını ve storageaccountkey, depolama hesabı anahtarı.

> [!NOTE]
> "\`" Semboldür ikinci komut bir PowerShell kaçış simgesi. "\`," "," (virgül) komutunun bir parçası olduğu anlamına gelir.
> 
> 

### <a name="head-node-share"></a>Baş düğüm paylaşımı
Alternatif olarak, Linux düğümlerinde baş düğümün paylaşılan bir klasöre bağlayın. Dosyaları paylaşmak için en kolay yolu bir paylaşım sağlar, ancak baş düğümü ve tüm Linux düğümleri aynı sanal ağda dağıtılmalıdır. Adımlar aşağıda verilmiştir.

1. Baş düğüm üzerinde bir klasör oluşturun ve herkese okuma/yazma izinlerine sahip paylaşabilirsiniz. Örneğin, baş düğümü olarak D:\OpenFOAM paylaşım \\CentOS7RDMA HN\OpenFOAM. Burada CentOS7RDMA HN baş düğümün adıdır.
   
    ![Dosya paylaşım izinleri][fileshareperms]
   
    ![Dosya paylaşımı][filesharing]
1. Bir Windows PowerShell penceresi açın ve aşağıdaki komutları çalıştırın:
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS7RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

İlk komut /openfoam LinuxNodes grubundaki tüm düğümlerde adlı bir klasör oluşturur. İkinci komut, paylaşılan klasör //CentOS7RDMA-HN/OpenFOAM dir klasörü üzerine bağlar ve dosya modunu BITS 777 ayarlayın. Kullanıcı adı ve parola komutu, kullanıcı adını ve parolasını bir baş düğüm Küme kullanıcı olması gerekir. (Bkz [Ekle veya Kaldır küme kullanıcı](https://technet.microsoft.com/library/ff919330.aspx).)

> [!NOTE]
> "\`" Semboldür ikinci komut bir PowerShell kaçış simgesi. "\`," "," (virgül) komutunun bir parçası olduğu anlamına gelir.
> 
> 

### <a name="nfs-server"></a>NFS sunucusu
NFS hizmeti, paylaşma ve dosyaları SMB protokolünü kullanan Windows Server 2012 işletim sistemi çalıştıran bilgisayarlar ve NFS protokolünü kullanarak Linux tabanlı bilgisayarlar arasında geçirmek sağlar. NFS sunucusu ve diğer tüm düğümler aynı sanal ağda dağıtılmış gerekir. Bu, Linux düğümleri SMB paylaşımı ile karşılaştırıldığında daha iyi uyum sağlar. Örneğin, dosya bağlantılarını desteklemiyor.

1. Yükleme ve bir NFS sunucusu ayarlamak için adımları [sunucusu için ağ dosya sistemi ilk paylaşımı uçtan uca](http://blogs.technet.com/b/filecab/archive/2012/10/08/server-for-network-file-system-first-share-end-to-end.aspx).
   
    Örneğin, aşağıdaki özelliklere sahip nfs adlı bir NFS paylaşımına oluşturun:
   
    ![NFS yetkilendirme][nfsauth]
   
    ![NFS paylaşım izinlerini][nfsshare]
   
    ![NFS NTFS izinleri][nfsperm]
   
    ![NFS yönetim özellikleri][nfsmanage]
1. Bir Windows PowerShell penceresi açın ve aşağıdaki komutları çalıştırın:
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /nfsshare
   
    clusrun /nodegroup:LinuxNodes mount CentOS7RDMA-HN:/nfs /nfsshared
    ```
   
   İlk komut /nfsshared LinuxNodes grubundaki tüm düğümlerde adlı bir klasör oluşturur. İkinci komut, bir NFS paylaşımına CentOS7RDMA HN bağlar: / klasörün üzerine nfs. Burada CentOS7RDMA HN: / nfs NFS paylaşımınızın uzak yoludur.

## <a name="how-to-submit-jobs"></a>İşleri göndermek nasıl
HPC Pack kümesine göndermek için birkaç yol vardır:

* HPC Küme Yöneticisi'ni veya HPC İş Yöneticisi GUI
* HPC web portalı
* REST API

HPC Pack GUI araçları ve HPC web Portalı aracılığıyla azure'daki bir kümeye iş gönderme olan aynı Windows işlem düğümleri. Bkz: [HPC Pack İş Yöneticisi](https://technet.microsoft.com/library/ff919691.aspx) ve [nasıl bir şirket içi istemci bilgisayarından'ndan iş gönderme](../../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

REST API aracılığıyla iş göndermek için başvurmak [oluşturma ve Microsoft HPC Pack REST API'yi kullanarak işleri gönderme](https://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx). Bir Linux İstemcisi'ndan iş gönderme için da Python örnek bakın [HPC Pack SDK](https://www.microsoft.com/download/details.aspx?id=47756).

## <a name="clusrun-for-linux-nodes"></a>Linux düğümleri için Clusrun
HPC Pack [clusrun](https://technet.microsoft.com/library/cc947685.aspx) aracı, bir komut istemini veya HPC Küme Yöneticisi aracılığıyla Linux düğümlerinde komutları yürütmek için kullanılabilir. Bazı temel örnekler aşağıda verilmiştir.

* Geçerli kullanıcı adları kümedeki tüm düğümleri gösterir.
  
    ```command
    clusrun whoami
    ```
* Yükleme **gdb** hata ayıklayıcısı aracı ile **yum** tüm linuxnodes düğümler grup ve 10 dakika sonra düğümleri yeniden başlatın.
  
    ```command
    clusrun /nodegroup:linuxnodes yum install gdb –y; shutdown –r 10
    ```
* Kümedeki her bir sayının 1'den 10 her Linux düğümde bir saniye için görüntüleyen bir kabuk betiği oluşturun, çalıştırın ve çıkış düğümlerden hemen göster.
  
    ```command
    clusrun /interleaved /nodegroup:linuxnodes echo \"for i in {1..10}; do echo \\\"\$i\\\"; sleep 1; done\" ^> script.sh; chmod +x script.sh; ./script.sh
    ```

> [!NOTE]
> Belirli bir çıkış karakterlerinin tanınmasından kullanmanız gerekebilir **clusrun** komutları. Bu örnekte gösterildiği gibi kullanın ^ atlamak için bir komut isteminde ">" simgesi.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* Çok sayıda düğüm kümeye'kurmak ölçeklendirme veya Linux iş yükü küme üzerinde çalışan deneyin. Bir örnek için bkz. [Linux'ta Microsoft HPC Pack ile NAMD çalıştırma azure'deki işlem düğümlerinde](hpcpack-cluster-namd.md).
* Bir küme ile deneyin [RDMA özellikli, yoğun işlem gücü kullanımlı VM'ler](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) MPI iş yüklerini çalıştırmak için. Bir örnek için bkz. [Azure'da bir Linux RDMA üzerinde Microsoft HPC Pack ile OpenFOAM çalıştırma küme](hpcpack-cluster-openfoam.md).
* Şirket içi HPC Pack kümesinde Linux düğümleri ile çalışmayı düşünüyorsanız, bkz. [TechNet Kılavuzu](https://technet.microsoft.com/library/mt595803.aspx).

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
