---
title: "Yıldız Çalıştır-CCM + Linux VM'ler üzerinde HPC Pack ile | Microsoft Docs"
description: "Azure üzerinde Microsoft HPC Pack küme dağıtma ve bir yıldız çalıştırma-CCM + işi birden çok Linux işlem düğümlerini bir RDMA ağ üzerinden."
services: virtual-machines-linux
documentationcenter: 
author: xpillons
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: 75523406-d268-4623-ac3e-811c7b74de4b
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 09/13/2016
ms.author: xpillons
ms.openlocfilehash: b45fcfb981287035da02fda62eaf5f9436ec2379
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="run-star-ccm-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Yıldız Çalıştır-CCM + Linux RDMA üzerinde Microsoft HPC Pack ile Azure'da küme
Bu makalede, Azure ve Çalıştır Microsoft HPC Pack kümede dağıtma gösterilmektedir bir [CD adapco yıldız-CCM +](http://www.cd-adapco.com/products/star-ccm%C2%AE) InfiniBand ile bağlandığına birden çok Linux işlem düğümlerinde iş.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

Microsoft HPC Pack büyük ölçekli HPC ve paralel uygulamalar, Microsoft Azure sanal makinelerin kümelerde MPI uygulamaları da dahil olmak üzere çeşitli çalıştırmak için özellikleri sağlar. HPC Pack çalışan Linux HPC uygulamaları bir HPC Pack kümede dağıtılan Linux işlem düğümü vm'lerde de destekler. Linux işlem düğümlerini HPC Pack ile kullanma giriş bilgileri için bkz: [Linux işlem düğümlerini Azure bir HPC Pack kümesindeki kullanmaya başlama](hpcpack-cluster.md).

## <a name="set-up-an-hpc-pack-cluster"></a>Bir HPC Pack kümesi
HPC Pack Iaas dağıtım betiklerinden karşıdan [Yükleme Merkezi'nden](https://www.microsoft.com/en-us/download/details.aspx?id=44949) ve yerel olarak ayıklayın.

Azure PowerShell önkoşuldur. PowerShell yerel makinenizde yapılandırılmamışsa, makaleyi okuyun [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).

Bu yazma zaman, SLES 12, CentOS 6.5 ve CentOS 7.1 (içeren InfiniBand sürücüleri için Azure) Azure Marketi Linux görüntülerden içindir. Bu makalede, SLES 12 kullanımı hakkında temel alır. Market HPC destekleyen tüm Linux görüntüleri adını almak için aşağıdaki PowerShell komutunu çalıştırabilirsiniz:

```
    get-azurevmimage | ?{$_.ImageName.Contains("hpc") -and $_.OS -eq "Linux" }
```

Çıkış, bu görüntüleri kullanılabilir konumu ve görüntü adı listeler (**görüntü adı**) daha sonra dağıtım şablonunda kullanılacak.

Küme dağıtmadan önce bir HPC paketi dağıtım şablon dosyası derleme sahip. Küçük bir küme hedefleme olduğundan, baş düğüm etki alanı denetleyicisi olması ve yerel bir SQL veritabanı ana bilgisayar.

Aşağıdaki şablonu bu tür bir baş düğüm dağıtmak, adlı bir XML dosyası oluşturma **MyCluster.xml**ve değerlerini değiştirme **Subscriptionıd**, **StorageAccount**,  **Konum**, **VMName**, ve **ServiceName** sizin ile.

    <?xml version="1.0" encoding="utf-8" ?>
    <IaaSClusterConfig>
      <Subscription>
        <SubscriptionId>99999999-9999-9999-9999-999999999999</SubscriptionId>
        <StorageAccount>mystorageaccount</StorageAccount>
      </Subscription>
      <Location>North Europe</Location>
      <VNet>
        <VNetName>hpcvnetne</VNetName>
        <SubnetName>subnet-hpc</SubnetName>
      </VNet>
      <Domain>
        <DCOption>HeadNodeAsDC</DCOption>
        <DomainFQDN>hpc.local</DomainFQDN>
      </Domain>
      <Database>
        <DBOption>LocalDB</DBOption>
      </Database>
      <HeadNode>
        <VMName>myhpchn</VMName>
        <ServiceName>myhpchn</ServiceName>
        <VMSize>Standard_D4</VMSize>
      </HeadNode>
      <LinuxComputeNodes>
        <VMNamePattern>lnxcn-%0001%</VMNamePattern>
        <ServiceNamePattern>mylnxcn%01%</ServiceNamePattern>
        <MaxNodeCountPerService>20</MaxNodeCountPerService>
        <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
        <VMSize>A9</VMSize>
        <NodeCount>0</NodeCount>
        <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
      </LinuxComputeNodes>
    </IaaSClusterConfig>

Baş düğüm oluşturma, yükseltilmiş bir komut istemi PowerShell komutunu çalıştırarak başlatın:

```
    .\New-HPCIaaSCluster.ps1 -ConfigFile MyCluster.xml
```

20-30 dakika sonra baş düğüm hazır olmanız gerekir. Tıklatarak için Azure portalından bağlanabilirsiniz **Bağlan** sanal makinenin simgesi.

Sonunda, DNS ileticisi düzeltme yapmanız gerekebilir. Bunu yapmak için DNS Yöneticisi'ni başlatın.

1. DNS Yöneticisi ' nde seçin sunucu adına sağ tıklayın **özellikleri**ve ardından **İleticiler** sekmesi.
2. Tıklatın **Düzenle** herhangi ileticiler kaldırmak için düğmesine tıklayın ve ardından **Tamam**.
3. Olduğundan emin olun **hiçbir ileticiler mevcutsa, kök ipuçlarını kullanacak** onay kutusu seçilidir ve ardından **Tamam**.

## <a name="set-up-linux-compute-nodes"></a>Linux işlem düğümlerini ayarlayın
Baş düğüm oluşturmak için kullanılan aynı dağıtım şablonu kullanarak Linux işlem düğümlerini dağıtın.

Dosya Kopyala **MyCluster.xml** baş düğüm ve güncelleştirme yerel makinenizden **NodeCount** dağıtmak istediğiniz düğümlerin sayısını etiketi (< = 20). Her A9 örneği aboneliğinizde 16 çekirdeğe tüketir olduğundan Azure kota, yeterli kullanılabilir çekirdeğe sahip dikkatli olun. Daha fazla sanal makineleri aynı bütçeye kullanmak istiyorsanız, A9 yerine A8 örnekleri (8 çekirdek) kullanabilirsiniz.

Baş düğümünde HPC Pack Iaas dağıtım betikleri kopyalayın.

Yükseltilmiş bir komut isteminde aşağıdaki Azure PowerShell komutlarını çalıştırın:

1. Çalıştırma **Add-AzureAccount** Azure aboneliğinize bağlanmak için.
2. Birden çok aboneliğiniz varsa çalıştırmak **Get-AzureSubscription** onları listelemek için.
3. Varsayılan bir abonelik çalıştırarak ayarlayın **Select-AzureSubscription - varlığıyla SubscriptionName xxxx-varsayılan** komutu.
4. Çalıştırma **.\New-HPCIaaSCluster.ps1 - ConfigFile MyCluster.xml** Linux işlem düğümlerini dağıtmaya başlamak için.
   
   ![Baş düğüm dağıtım eylem][hndeploy]

HPC Pack Küme Yöneticisi aracını açın. Birkaç dakika sonra Linux işlem düğümlerini düzenli olarak küme işlem düğümleri listede görünür. Klasik dağıtım modeliyle Iaas VM'ler sıralı olarak oluşturulur. Düğüm sayısını önemliyse, böylece tüm dağıtılan alma önemli miktarda zaman alabilir.

![Linux düğümleri HPC Pack Küme Yöneticisi'nde][clustermanager]

Kümedeki tüm düğümler hazır ve çalışır olduğundan, ek altyapı ayarları vardır.

## <a name="set-up-an-azure-file-share-for-windows-and-linux-nodes"></a>Windows ve Linux düğümleri için bir Azure dosya paylaşımı ayarlama
Azure dosya hizmeti, komut dosyaları, uygulama paketleri ve veri dosyalarını depolamak için kullanabilirsiniz. Azure dosya kalıcı deposu olarak Azure Blob Depolama CIFS işlevleri sağlar. Bu en ölçeklenebilir bir çözüm değildir, ancak basit bir olduğundan ve özel VM'ler gerektirmeyen unutmayın.

Makalesindeki yönergeleri izleyerek bir Azure dosya paylaşımı oluşturmak [Windows Azure File storage ile çalışmaya başlama](../../../storage/files/storage-dotnet-how-to-use-files.md).

Depolama hesabınız olarak adını tutmak **saname**, dosya paylaşımı adı olarak **sharename**ve depolama hesabı anahtarı olarak **sakey**.

### <a name="mount-the-azure-file-share-on-the-head-node"></a>Baş düğümünde Azure dosya paylaşımını bağlama
Yükseltilmiş bir komut istemi açın ve yerel makine kasaya kimlik bilgilerini depolamak için aşağıdaki komutu çalıştırın:

```
    cmdkey /add:<saname>.file.core.windows.net /user:<saname> /pass:<sakey>
```

Ardından, Azure dosya paylaşımını bağlama için çalıştırın:

```
    net use Z: \\<saname>.file.core.windows.net\<sharename> /persistent:yes
```

### <a name="mount-the-azure-file-share-on-linux-compute-nodes"></a>Linux işlem düğümlerini Azure dosya paylaşımını bağlama
HPC paketi ile birlikte gelen bir yararlı aracı clusrun araçtır. İşlem düğümleri kümesi üzerinde aynı anda aynı komutu çalıştırmak için bu komut satırı aracını kullanabilirsiniz. Örneğimizde, bunu Azure dosya paylaşımını bağlama ve onu yeniden başlatmalar varlığını sürdürmesi için kalıcı olması için kullanılır.
Baş düğüm yükseltilmiş bir komut istemi içinde aşağıdaki komutları çalıştırın.

Bağlama dizin oluşturmak için:

```
    clusrun /nodegroup:LinuxNodes mkdir -p /hpcdata
```

Azure dosya paylaşımını bağlama için:

```
    clusrun /nodegroup:LinuxNodes mount -t cifs //<saname>.file.core.windows.net/<sharename> /hpcdata -o vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777
```

Bağlama paylaşım devam ettirmek için:

```
    clusrun /nodegroup:LinuxNodes "echo //<saname>.file.core.windows.net/<sharename> /hpcdata cifs vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777 >> /etc/fstab"
```

## <a name="install-star-ccm"></a>Yıldız yükle-CCM +
Azure VM örnekleri A8 ve A9 InfiniBand destek ve RDMA yetenekleri sağlar. Bu özellikleri etkinleştirecek çekirdek sürücüleri, Windows Server 2012 R2, SUSE 12, CentOS 6.5 ve Azure Marketi CentOS 7.1 görüntüleri için kullanılabilir. Microsoft MPI ve Intel MPI (5.x sürümü), bu sürücüleri Azure'da destekleyen iki MPI kitaplıkları var.

CD adapco yıldız-CCM + 11.x bırakın ve daha sonra Intel MPI sürümüyle birlikte Azure InfiniBand desteği dahil olacak şekilde 5.x.

Linux64 alma yıldız-CCM + paketinden [CD adapco portal](https://steve.cd-adapco.com). Örneğimizde, karma duyarlık 11.02.010 sürümünde kullandık.

Baş düğüm içinde **/hpcdata** Azure dosya paylaşımı, adlandırılmış bir kabuk betiği oluşturma **setupstarccm.sh** aşağıdaki içeriğe sahip. Bu komut dosyası yıldız ayarlamak için her işlem düğümünde çalıştırılacak-CCM + yerel olarak.

#### <a name="sample-setupstarcmsh-script"></a>Örnek setupstarcm.sh komut dosyası
```
    #!/bin/bash
    # setupstarcm.sh to set up STAR-CCM+ locally

    # Create the CD-adapco main directory
    mkdir -p /opt/CD-adapco

    # Copy the STAR-CCM package from the file share to the local directory
    cp /hpcdata/StarCCM/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz /opt/CD-adapco/

    # Extract the package
    tar -xzf /opt/CD-adapco/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz -C /opt/CD-adapco/

    # Start a silent installation of STAR-CCM without the FLEXlm component
    /opt/CD-adapco/starccm+_11.02.010/STAR-CCM+11.02.010_01_linux-x86_64-2.5_gnu4.8.bin -i silent -DCOMPUTE_NODE=true -DNODOC=true -DINSTALLFLEX=false

    # Update memory limits
    echo "*               hard    memlock         unlimited" >> /etc/security/limits.conf
    echo "*               soft    memlock         unlimited" >> /etc/security/limits.conf
```
Yıldız ayarlamak için şimdi-CCM + tüm Linux işlem düğümleri, yükseltilmiş bir komut istemi açın ve aşağıdaki komutu çalıştırın:

```
    clusrun /nodegroup:LinuxNodes bash /hpcdata/setupstarccm.sh
```

Komut çalışırken, Küme Yöneticisi'nin ısı Haritası kullanarak CPU kullanımını izleyebilirsiniz. Birkaç dakika sonra tüm düğümleri doğru şekilde ayarlanmış olması.

## <a name="run-star-ccm-jobs"></a>Yıldız Çalıştır-CCM + işleri
HPC Pack kullanılan iş Zamanlayıcı yeteneklerini için yıldız çalıştırmak için-CCM + işler. Bunu yapmak için destek iş başlatmak ve yıldız çalıştırmak için kullanılan birkaç komut dosyalarının gerekiyor-CCM +. Giriş verilerini Azure dosya paylaşımında kolaylık olması için ilk olarak tutulur.

Aşağıdaki PowerShell betiğini bir yıldız sıraya almak için kullanılan-CCM + işi. Üç bağımsız değişkeni alır:

* Model adı
* Kullanılacak düğüm sayısı
* Kullanılacak her bir düğümüne çekirdek sayısı

Çünkü yıldız-CCM + bellek bant genişliği doldurabilirsiniz işlem düğümleri başına daha az çekirdek kullanın ve yeni düğümler eklemek genellikle daha iyi olur. Tam düğümü başına çekirdek sayısı, işlemci ailesi ve bağlantı hızı değişir.

Düğümleri yalnızca iş için ayrılmış ve diğer işlemlerle paylaşılamaz. İş MPI iş olarak doğrudan başlatılmadı. **Runstarccm.sh** Kabuk betiği MPI Başlatıcısı başlayacak.

Giriş modeli ve **runstarccm.sh** betik depolanır **/hpcdata** daha önce oluşturulmuş paylaşımı.

Günlük dosyaları ile iş kimliği adlı ve depolanan **/hpcdata paylaşımı**, yıldız birlikte-CCM + çıkış dosyaları.

#### <a name="sample-submitstarccmjobps1-script"></a>Örnek SubmitStarccmJob.ps1 komut dosyası
```
    Add-PSSnapin Microsoft.HPC -ErrorAction silentlycontinue
    $scheduler="headnodename"
    $modelName=$args[0]
    $nbCoresPerNode=$args[2]
    $nbNodes=$args[1]

    #---------------------------------------------------------------------------------------------------------
    # Create a new job; this will give us the job ID that's used to identify the name of the uploaded package in Azure
    #
    $job = New-HpcJob -Name "$modelName $nbNodes $nbCoresPerNode" -Scheduler $scheduler -NumNodes $nbNodes -NodeGroups "LinuxNodes" -FailOnTaskFailure $true -Exclusive $true
    $jobId = [String]$job.Id

    #---------------------------------------------------------------------------------------------------------
    # Submit the job     
    $workdir =  "/hpcdata"
    $execName = "$nbCoresPerNode runner.java $modelName.sim"

    $job | Add-HpcTask -Scheduler $scheduler -Name "Compute" -stdout "$jobId.log" -stderr "$jobId.err" -Rerunnable $false -NumNodes $nbNodes -Command "runstarccm.sh $execName" -WorkDir "$workdir"


    Submit-HpcJob -Job $job -Scheduler $scheduler
```
Değiştir **runner.java** , tercih edilen yıldız ile-CCM + Java modeli Başlatıcısı ve günlük kodu.

#### <a name="sample-runstarccmsh-script"></a>Örnek runstarccm.sh komut dosyası
```
    #!/bin/bash
    echo "start"
    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    echo ${SCRIPT_PATH}
    # Set the mpirun runtime environment
    export CDLMD_LICENSE_FILE=1999@flex.cd-adapco.com

    # mpirun command
    STARCCM=/opt/CD-adapco/STAR-CCM+11.02.010/star/bin/starccm+

    # Get node information from ENVs
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    NBCORESPERNODE=$1

    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$
    echo ${NODELIST_PATH}

    # Get every node name and write into the hostfile file
    I=1
    NBNODES=0
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
        let "NBNODES=${NBNODES}+1"
    done
    let "NBCORES=${NBNODES}*${NBCORESPERNODE}"

    # Run STAR-CCM with the hostfile argument
    #  
    ${STARCCM} -np ${NBCORES} -machinefile ${NODELIST_PATH} \
        -power -podkey "<yourkey>" -rsh ssh \
        -mpi intel -fabric UDAPL -cpubind bandwidth,v \
        -mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0" \
        -batch $2 $3
    RTNSTS=$?
    rm -f ${NODELIST_PATH}

    exit ${RTNSTS}
```

Testimizde, bir isteğe bağlı güç lisans belirteci kullandık. Bu bir belirteç ayarlamanız gerekir. **$CDLMD_LICENSE_FILE** ortam değişkenine  **1999@flex.cd-adapco.com**  ve anahtar **- podkey** komut satırı seçeneği.

Bazı başlatma sonra komut dosyasını ayıklar--gelen **$CCP_NODES_CORES** ortam değişkenlerini ayarlama, HPC Pack--MPI Başlatıcısı'nı kullanan bir hostfile oluşturmak için düğüm listesi. Bu hostfile işi, satır başına bir ad için kullanılan işlem düğümü adlarının listesini içerir.

Biçimi **$CCP_NODES_CORES** bu deseni izler:

```
<Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
```

Konumlar:

* `<Number of nodes>`Bu iş için ayrılan düğümler sayısıdır.
* `<Name of node_n_...>`Bu iş için ayrılmış her düğümün adıdır.
* `<Cores of node_n_...>`Bu iş için ayrılmış düğümünde çekirdek sayısıdır.

Çekirdek sayısı (**$NBCORES**) düğüm sayısını temel alınarak hesaplanır (**$NBNODES**) ve düğümü başına çekirdek sayısı (parametre olarak sağlanan **$NBCORESPERNODE**).

MPI seçenekleri için Azure üzerinde Intel MPI ile kullanılan olanlardır:

* `-mpi intel`Intel MPI belirtmek için.
* `-fabric UDAPL`Azure InfiniBand fiiller kullanmak için.
* `-cpubind bandwidth,v`YILDIZ ile MPI için bant genişliğini iyileştirmek için-CCM +.
* `-mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0"`Intel ile Azure InfiniBand iş MPI yapmak için ve gerekli düğümü başına çekirdek sayısını ayarlama.
* `-batch`Yıldız başlatmak için-CCM + toplu iş modunda kullanıcı Arabirimi ile.

Son olarak, bir işlemi başlatmak için düğümlerinizin ve çalışıyor olduğundan ve Küme Yöneticisi'nde çevrimiçi olduğundan emin olun. Ardından bir PowerShell komut isteminde bu çalıştırın:

```
    .\ SubmitStarccmJob.ps1 <model> <nbNodes> <nbCoresPerNode>
```

## <a name="stop-nodes"></a>Düğümler durdurun
Daha sonra testlerinizi ile bitirdikten sonra durdurmak ve düğümler başlatmak için aşağıdaki HPC Pack PowerShell komutlarını kullanabilirsiniz:

```
    Stop-HPCIaaSNode.ps1 -Name <prefix>-00*
    Start-HPCIaaSNode.ps1 -Name <prefix>-00*
```

## <a name="next-steps"></a>Sonraki adımlar
Diğer Linux iş yükleri çalıştırmayı deneyin. Örneğin, bakın:

* [Linux işlem düğümlerinde Azure ile birlikte Microsoft HPC Pack NAMD çalıştırın](hpcpack-cluster-namd.md)
* [OpenFOAM, azure'da bir Linux RDMA kümede Microsoft HPC Pack ile çalıştırın.](hpcpack-cluster-openfoam.md)

<!--Image references-->
[hndeploy]:media/hpcpack-cluster-starccm/hndeploy.png
[clustermanager]:media/hpcpack-cluster-starccm/ClusterManager.png
