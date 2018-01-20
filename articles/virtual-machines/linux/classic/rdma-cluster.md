---
title: "MPI uygulamaları çalıştırmak için Linux RDMA küme ayarlama | Microsoft Docs"
description: "Linux küme boyutu H16r, H16mr, A8 veya A9 Vm'lerde Azure RDMA ağ MPI uygulamaları çalıştırmak için oluşturma"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 01834bad-c8e6-48a3-b066-7f1719047dd2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/14/2017
ms.author: danlep
ms.openlocfilehash: 428dc1e8ba03ba17e348a33a33b5cf5e6118a43c
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="set-up-a-linux-rdma-cluster-to-run-mpi-applications"></a>MPI uygulamalarını çalıştırmak için bir Linux RDMA kümesi oluşturma
Azure ile Linux RDMA kümedeki ayarlamak öğrenin [yüksek performanslı işlem VM boyutları](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) paralel ileti geçirme arabirimi (MPI) uygulamalarını çalıştırmak için. Bu makalede bir kümede Intel MPI çalıştırmak için Linux HPC görüntüsünü hazırlamak için adımları sağlar. Hazırlık sonra bu görüntü ve RDMA özellikli Azure VM boyutlarını (şu anda H16r, H16mr, A8 veya A9) birini kullanarak sanal makineleri bir küme dağıtın. Küme, doğrudan uzak bellek erişimi (RDMA) teknolojisine dayalı düşük gecikmeli, yüksek verimlilik bir ağ üzerinden verimli bir şekilde iletişim kuran MPI uygulamaları çalıştırmak için kullanın.

> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Azure Resource Manager](../../../resource-manager-deployment-model.md) ve klasik. Bu makale klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

## <a name="cluster-deployment-options"></a>Küme dağıtım seçenekleri
İle veya iş Zamanlayıcı olmadan Linux RDMA küme oluşturmak için kullanabileceğiniz yöntemleri aşağıda verilmiştir.

* **Azure CLI betikleri**: Bu makalenin sonraki bölümlerinde gösterildiği gibi kullanın [Azure komut satırı arabirimi](../../../cli-install-nodejs.md) RDMA özellikli VM'lerin bir küme dağıtımı komut dosyası için (CLI). Hizmet Yönetimi modunda CLI küme düğümleri seri olarak Klasik dağıtım modelinde oluşturur, bu nedenle birçok işlem düğümleri dağıtma birkaç dakika sürebilir. Klasik dağıtım modeli kullandığınızda RDMA ağ bağlantısı etkinleştirmek için sanal makineleri aynı bulut hizmetindeki dağıtın.
* **Azure Resource Manager şablonları**: RDMA ağa bağlanır RDMA özellikli VM'ler oluşan bir küme dağıtmak için Resource Manager dağıtım modeli de kullanabilirsiniz. Yapabilecekleriniz [kendi şablonunuzu oluşturun](../../../resource-group-authoring-templates.md), ya da kontrol [Azure hızlı başlangıç şablonlarını](https://azure.microsoft.com/documentation/templates/) istediğiniz çözümü dağıtmak için Microsoft veya topluluk tarafından katkıda bulunan şablonları için. Resource Manager şablonları Linux kümesi dağıtmak için hızlı ve güvenilir bir yol sağlar. Resource Manager dağıtım modeli kullandığınızda RDMA ağ bağlantısı etkinleştirmek için aynı kullanılabilirlik kümesindeki sanal makineleri dağıtın.
* **HPC Pack**: Microsoft HPC Pack küme oluşturmayı ve RDMA ağ erişmek için desteklenen bir Linux dağıtım çalıştırmak RDMA özellikli işlem düğümleri ekleyin. Daha fazla bilgi için bkz: [Linux işlem düğümlerini Azure bir HPC Pack kümesindeki kullanmaya başlama](hpcpack-cluster.md).

## <a name="sample-deployment-steps-in-the-classic-model"></a>Klasik modeli örnek dağıtım adımları
Aşağıdaki adımlar, Azure CLI Azure Marketi'nden SUSE Linux Enterprise Server (SLES) 12 SP1 HPC VM dağıtmak, özelleştirebilir ve özel bir VM görüntüsü oluşturmak için nasıl kullanılacağını gösterir. Ardından RDMA özellikli VM'lerin bir küme dağıtımı komut dosyası için görüntüyü kullanabilirsiniz.

> [!TIP]
> Azure Market CentOS tabanlı HPC görüntülerinde göre RDMA özellikli VM'ler oluşan bir küme dağıtmak için benzer adımları kullanın. Bazı adımlar belirtildiği gibi biraz daha farklı. 
>
>

### <a name="prerequisites"></a>Önkoşullar
* **İstemci bilgisayar**: Azure ile iletişim kurmak için bir Mac, Linux veya Windows istemci bilgisayar gerekir. Bu adımlarda Linux istemci kullandığınız varsayılır.
* **Azure aboneliği**: bir aboneliğiniz yoksa oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/) yalnızca birkaç dakika içinde. Daha büyük kümeleri için Kullandıkça Öde aboneliğine veya diğer satın alma seçenekleri göz önünde bulundurun.
* **VM boyutu kullanılabilirlik**: RDMA özellikli aşağıdaki örneği boyutlarıdır: H16r, H16mr, A8 ve A9. Denetleme [bölgeye göre ürünleri](https://azure.microsoft.com/regions/services/) Azure bölgelerindeki kullanılabilirlik.
* **Çekirdek kota**: işlem yoğunluklu VM'ler oluşan bir küme dağıtmak için çekirdek kota artırmanız gerekebilir. Örneğin, bu makaledeki gösterildiği gibi 8 A9 VM dağıtmak istiyorsanız, en az 128 çekirdek gerekir. Aboneliğinizi H-serisi dahil olmak üzere belirli VM boyutu aileleri, dağıtabilirsiniz çekirdek sayısı da sınırlayabilir. Bir kota artışı isteği göndermek üzere [bir çevrimiçi müşteri destek isteği açma](../../../azure-supportability/how-to-create-azure-support-request.md) herhangi bir ücret alınmaz.
* **Azure CLI**: [yükleme](../../../cli-install-nodejs.md) Azure CLI ve [Azure aboneliğinize bağlanmak](/cli/azure/authenticate-azure-cli) istemci bilgisayardan.

### <a name="provision-an-sles-12-sp1-hpc-vm"></a>SLES 12 SP1 HPC VM sağlama
Azure CLI ile azure'da oturum açtıktan sonra çalıştırmak `azure config list` çıkış hizmet yönetimi modu gösterir onaylamak için. Yoksa, şu komutu çalıştırarak modunu ayarlayın:

    azure config mode asm


Kullanmak için yetkilendirilmesini tüm abonelikleri listelemek için aşağıdaki komutu yazın:

    azure account list

Geçerli etkin bir aboneliğiniz ile tanımlanan `Current` kümesine `true`. Bu abonelik küme oluşturmak için kullanmak istediğiniz bir değilse, uygun abonelik kimliği etkin bir aboneliğiniz ayarlayın:

    azure account set <subscription-Id>

Azure genel kullanıma açık SLES 12 SP1 HPC görüntüleri görmek için aşağıdaki gibi bir komut çalıştırmak, kabuk ortamı destekler, varsayılarak **grep**:

    azure vm image list | grep "suse.*hpc"

RDMA özellikli bir VM SLES 12 SP1 HPC görüntüsü ile aşağıdaki gibi bir komutu çalıştırarak sağlayın:

    azure vm create -g <username> -p <password> -c <cloud-service-name> -l <location> -z A9 -n <vm-name> -e 22 b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824

Konumlar:

* Boyutu (Bu örnekte A9) RDMA özelliğine sahip VM boyutları biridir.
* Dış SSH bağlantı noktası numarası (Bu örnekte, SSH varsayılandır 22) tüm geçerli bağlantı noktası numarasıdır. İç SSH bağlantı noktası numarası 22 olarak ayarlanır.
* Yeni bir bulut hizmeti konum tarafından belirtilen Azure bölgesinde oluşturulur. Seçtiğiniz VM boyutu kullanılabilir bir konum belirtin.
* (Bu, ek ücret doğurur) SUSE öncelik desteği için SLES 12 SP1 görüntü adı şu anda bu iki seçenek biri olabilir: 

 `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824`

  `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-priority-v20160824`


### <a name="customize-the-vm"></a>VM özelleştirme
VM sağlama, VM'nin dış IP adresine (veya DNS adı) kullanarak VM SSH bitirip dış bağlantı noktası numarası yapılandırılmış ve ardından özelleştirin sonra. Bağlantı ayrıntıları için bkz: [Linux çalıştıran bir sanal makine için oturum açma](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Kök erişimi bir adımı tamamlamak için gerekli olmadıkça komutları VM üzerinde yapılandırılan kullanıcı olarak gerçekleştirin.

> [!IMPORTANT]
> Microsoft Azure Linux VM'ler için kök erişimi sağlamaz. VM için bir kullanıcı olarak bağlanıldığında yönetimsel erişim kazanmak için komutları kullanarak çalıştırmak `sudo`.
>
>

* **Güncelleştirmeleri**: zypper kullanarak güncelleştirmeleri yükleyin. NFS utilities yüklemek isteyebilirsiniz.

  > [!IMPORTANT]
  > SLES 12 SP1 HPC VM ile sorunları sürücüleri ile Linux RDMA neden çekirdek güncelleştirmeleri uygulanmaz öneririz.
  >
  >
* **Intel MPI**: Intel MPI SLES 12 SP1 HPC VM üzerinde aşağıdaki komutu çalıştırarak yüklemesini:

        sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
* **Bellek kilitleme**: RDMA için kullanılabilir bellek kilitlenemedi için MPI kodları ekleyebilir veya /etc/security/limits.conf dosyasında aşağıdaki ayarları değiştirebilirsiniz. Bu dosyayı düzenlemek için kök erişimi gerekir.

    ```
    <User or group name> hard    memlock <memory required for your application in KB>

    <User or group name> soft    memlock <memory required for your application in KB>
    ```

  > [!NOTE]
  > Test amacıyla, ayrıca memlock sınırsız olarak ayarlayabilirsiniz. Örneğin: `<User or group name>    hard    memlock unlimited`. Daha fazla bilgi için bkz: [ayarı için bilinen en iyi yöntemleri kilitli bellek boyutu](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).
  >
  >
* **SLES VM'ler için SSH anahtarları**: SLES kullanıcı hesabınızın işlem düğümleri arasında güven oluşturmak SSH anahtarları küme MPI işlerini çalıştırırken. CentOS tabanlı HPC VM dağıttıysanız, bu adımı izlemeyin. Küme düğümleri arasında passwordless SSH güven görüntüsünü yakalamak ve küme dağıttıktan sonra ayarlama yönergeleri bu makalenin sonraki bölümlerinde bkz.

    SSH anahtarları oluşturmak için aşağıdaki komutu çalıştırın. Giriş için istendiğinde seçin **Enter** parolayı belirtmeden varsayılan konumda anahtarları oluşturmak için.

        ssh-keygen

    Authorized_keys dosya bilinen ortak anahtarları için ortak anahtarı ekleyin.

        cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

    ~/.Ssh dizininde düzenleyin veya yapılandırma dosyası oluşturun. Azure (Bu örnekte 10.32.0.0/16) kullanmayı planlıyorsanız özel ağ IP adresi aralığı girin:

        host 10.32.0.*
        StrictHostKeyChecking no

    Alternatif olarak, her bir VM kümenizdeki özel ağ IP adresi gibi listesi:

    ```
    host 10.32.0.1
     StrictHostKeyChecking no
    host 10.32.0.2
     StrictHostKeyChecking no
    host 10.32.0.3
     StrictHostKeyChecking no
    ```

  > [!NOTE]
  > Yapılandırma `StrictHostKeyChecking no` belirli IP adreslerini veya aralığını belirtilmediyse, bir güvenlik riski oluşturabilir.
  >
  >
* **Uygulamaları**: gerekir ya da görüntüyü yakalamadan önce diğer özelleştirmeleri gerçekleştirin tüm uygulamaları yükleyin.

### <a name="capture-the-image"></a>Yansıma Yakalama
Görüntü yakalamak için önce Linux VM'de aşağıdaki komutu çalıştırın. Bu komut, VM deprovisions ancak kullanıcı hesapları ve ayarladığınız SSH anahtarları korur.

```
sudo waagent -deprovision
```

İstemci bilgisayarınızdan görüntüsünü yakalamak için aşağıdaki Azure CLI komutları çalıştırın. Daha fazla bilgi için bkz: [klasik bir Linux sanal makinesini görüntü olarak yakalama](capture-image-classic.md).  

```
azure vm shutdown <vm-name>

azure vm capture -t <vm-name> <image-name>

```

Bu komutları çalıştırdıktan sonra VM görüntüsü kullanımınız için yakalanır ve VM silinir. Şimdi bir küme dağıtmak için hazır özel görüntünüzü sahip.

### <a name="deploy-a-cluster-with-the-image"></a>Görüntüsü olan bir kümeyi dağıtma
Aşağıdaki Bash komut dosyası, ortamınız için uygun değerlerle değiştirin ve istemci bilgisayardan çalıştırın. Azure sanal makineleri Klasik dağıtım modelinde seri olarak dağıtır olduğundan, bu komut dosyasını önerilen sekiz A9 VM dağıtmak için birkaç dakika sürer.

```
#!/bin/bash -x
# Script to create a compute cluster without a scheduler in a VNet in Azure
# Create a custom private network in Azure
# Replace 10.32.0.0 with your virtual network address space
# Replace <network-name> with your network identifier
# Replace "West US" with an Azure region where the VM size is available
# See Azure Pricing pages for prices and availability of compute-intensive VMs

azure network vnet create -l "West US" -e 10.32.0.0 -i 16 <network-name>

# Create a cloud service. All the compute-intensive instances need to be in the same cloud service for Linux RDMA to work across InfiniBand.
# Note: The current maximum number of VMs in a cloud service is 50. If you need to provision more than 50 VMs in the same cloud service in your cluster, contact Azure Support.

azure service create <cloud-service-name> --location "West US" –s <subscription-ID>

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Define a prefix for external port numbers. If you want to turn off external ports and use only internal ports to communicate between compute nodes via port 22, don’t use this option. Since port numbers up to 10000 are reserved, use numbers after 10000. Leave external port on for rank 0 and head node.

portnumber=101

# In this cluster there will be 8 size A9 nodes, named cluster11 to cluster18. Specify your captured image in <image-name>. Specify the username and password you used when creating the SSH keys.

for (( i=11; i<19; i++ )); do
        azure vm create -g <username> -p <password> -c <cloud-service-name> -z A9 -n $vmname$i -e $portnumber$i -w <network-name> -b Subnet-1 <image-name>
done

# Save this script with a name like makecluster.sh and run it in your shell environment to provision your cluster
```

## <a name="considerations-for-a-centos-hpc-cluster"></a>CentOS HPC Kümesi için ilgili önemli noktalar
SLES 12 yerine Azure Marketi CentOS tabanlı HPC görüntülerden birini HPC için temel bir küme ayarlamak istiyorsanız, önceki bölümde genel adımları izleyin. Sağlama ve VM yapılandırdığınızda aşağıdaki farkları dikkat edin:

- Intel MPI CentOS tabanlı HPC görüntüden sağlanan bir VM üzerinde zaten yüklü.
- Kilit bellek ayarları VM /etc/security/limits.conf dosyasında zaten eklendi.
- Yakalama için SSH anahtarları sağlamanız VM'de oluşturmaz. Küme dağıttıktan sonra bunun yerine, kullanıcı tabanlı kimlik doğrulaması kurma öneririz. Daha fazla bilgi için aşağıdaki bölümüne bakın.  

### <a name="set-up-passwordless-ssh-trust-on-the-cluster"></a>Kümede passwordless SSH güven ayarlama
CentOS tabanlı HPC küme üzerinde işlem düğümleri arasında güven kurulması için iki yöntem vardır: ana bilgisayar tabanlı kimlik doğrulama ve kullanıcı tabanlı kimlik doğrulaması. Ana bilgisayar tabanlı kimlik doğrulaması, bu makalenin kapsamı dışında ve genellikle bir uzantı komut dosyası dağıtımı sırasında yapılması gerekir. Kullanıcı tabanlı kimlik doğrulaması dağıtımdan sonra güven kurulması için uygundur ve oluşturma ve bilgi işlem düğümleri arasında SSH anahtarları kümede paylaşılmasını gerektirir. Bu yöntem genellikle passwordless SSH oturumu açma olarak bilinen ve MPI işlerini çalıştırma durumlarda gereklidir.

Topluluktan katkıda bulunan bir örnek komut dosyası edinilebilir [GitHub](https://github.com/tanewill/utils/blob/master/user_authentication.sh) CentOS tabanlı HPC küme kolay kullanıcı kimlik doğrulamasını etkinleştirmek için. Karşıdan yükleyin ve aşağıdaki adımları kullanarak bu betiği kullanın. Ayrıca, bu komut dosyasını değiştirin veya küme bilgi işlem düğümleri arasında passwordless SSH kimlik doğrulaması kurmak için başka bir yöntemi kullanabilirsiniz.

    wget https://raw.githubusercontent.com/tanewill/utils/master/ user_authentication.sh

Komut dosyasını çalıştırmak için alt ağ IP adresi için önek bilmeniz gerekir. Küme düğümlerinden biri üzerinde aşağıdaki komutu çalıştırarak öneki alın. Çıktı gibi 10.1.3.5 görünmelidir ve ön eki 10.1.3 bölümü.

    ifconfig eth0 | grep -w inet | awk '{print $2}'

Artık üç parametrelerini kullanarak betiği çalıştırın: işlem düğümlerinde ortak kullanıcı adı, işlem düğümlerini ve önceki komutu döndürüldü alt ağ öneki sunucusundaki bu kullanıcı için sık kullanılan parola.

    ./user_authentication.sh <myusername> <mypassword> 10.1.3

Bu betik şunları yapar:

* .Ssh, passwordless oturum açma için gerekli olduğu adlı konak düğümde bir dizin oluşturur.
* Kümedeki herhangi bir düğümden oturum açma izni passwordless oturum açma bildirir .ssh dizininde bir yapılandırma dosyası oluşturur.
* Düğüm adı ve düğüm IP adresleri kümedeki tüm düğümler için içeren dosyaları oluşturur. Daha sonra başvurmak için komut dosyasını çalıştırdıktan sonra bu dosyaları bırakılır.
* Her küme düğümü (ana bilgisayar düğümü dahil) için özel ve ortak anahtar çifti oluşturur ve girişleri authorized_keys dosyasında oluşturur.

> [!WARNING]
> Bu komut dosyasını çalıştıran bir güvenlik riski oluşturabilir. Ortak anahtar bilgileri ~/.ssh içinde değil dağıtılmış emin olun.
>
>

## <a name="configure-intel-mpi"></a>Intel MPI yapılandırın
Azure Linux RDMA üzerinde MPI uygulamaları çalıştırmak için belirli bir ortam değişkenleri için Intel MPI belirli yapılandırmanız gerekir. Bir uygulamayı çalıştırmak için gereken değişkenleri yapılandırmak için bir Bash komut dosyası aşağıda verilmiştir. Yol için mpivars.sh Intel MPI yüklemeniz için gerektiği gibi değiştirin.

```
#!/bin/bash -x

# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh

export I_MPI_FABRICS=shm:dapl

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB
# Setting the variable to shm:dapl gives best performance for some applications
# If your application doesn’t take advantage of shared memory and MPI together, then set only dapl

export I_MPI_DAPL_PROVIDER=ofa-v2-ib0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

export I_MPI_DYNAMIC_CONNECTION=0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

# Command line to run the job

mpirun -n <number-of-cores> -ppn <core-per-node> -hostfile <hostfilename>  /path <path to the application exe> <arguments specific to the application>

#end
```

Ana bilgisayar dosyası biçimi aşağıdaki gibidir. Kümedeki her düğüm için bir satır ekleyin. Sanal ağdan özel IP adresleri daha önce tanımlanan DNS adlarını belirtin. Örneğin, IP adresleriyle 10.32.0.1 ve 10.32.0.2 için iki ana dosyası şunları içerir:

```
10.32.0.1:16
10.32.0.2:16
```

## <a name="run-mpi-on-a-basic-two-node-cluster"></a>Temel iki düğümlü bir küme üzerinde MPI çalıştırın
İlk zaten yapmadıysanız, Intel MPI ortamını ayarlama.

```
# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh
```

### <a name="run-an-mpi-command"></a>MPI komutunu çalıştırın
MPI komutu MPI düzgün bir şekilde yüklendiğini ve arasındaki en az iki işlem düğümleri iletişim kurabilir göstermek için işlem düğümlerinden biri üzerinde çalıştırın. Aşağıdaki **mpirun** komutu çalıştırır **ana bilgisayar adı** iki düğüm üzerinde komutu.

```
mpirun -ppn 1 -n 2 -hosts <host1>,<host2> -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname
```
Çıktı için giriş olarak geçirilen tüm düğümlerinin adlarını listelenmelidir `-hosts`. Örneğin, bir **mpirun** iki düğümü komut aşağıdaki gibi bir çıktı döndürür:

```
cluster11
cluster12
```

### <a name="run-an-mpi-benchmark"></a>MPI Kıyaslama çalıştırması
Küme yapılandırmasını ve RDMA ağ bağlantısını doğrulamak için pingpong Kıyaslama aşağıdaki Intel MPI komutu çalıştırır.

```
mpirun -hosts <host1>,<host2> -ppn 1 -n 2 -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong
```

İki düğümle bir çalışma kümesinde aşağıdaki gibi bir çıktı görmeniz gerekir. Azure RDMA ağ üzerinde gecikme düzeyinde veya altında 3 mikrosaniye ileti için en çok 512 bayt boyutları bekler.

```
#------------------------------------------------------------
#    Intel (R) MPI Benchmarks 4.0 Update 1, MPI-1 part
#------------------------------------------------------------
# Date                  : Fri Jul 17 23:16:46 2015
# Machine               : x86_64
# System                : Linux
# Release               : 3.12.39-44-default
# Version               : #5 SMP Thu Jun 25 22:45:24 UTC 2015
# MPI Version           : 3.0
# MPI Thread Environment:
# New default behavior from Version 3.2 on:
# the number of iterations per message size is cut down
# dynamically when a certain run time (per message size sample)
# is expected to be exceeded. Time limit is defined by variable
# "SECS_PER_SAMPLE" (=> IMB_settings.h)
# or through the flag => -time

# Calling sequence was:
# /opt/intel/impi_latest/bin64/IMB-MPI1 pingpong
# Minimum message length in bytes:   0
# Maximum message length in bytes:   4194304
#
# MPI_Datatype                   :   MPI_BYTE
# MPI_Datatype for reductions    :   MPI_FLOAT
# MPI_Op                         :   MPI_SUM
#
#
# List of Benchmarks to run:
# PingPong
#---------------------------------------------------
# Benchmarking PingPong
# #processes = 2
#---------------------------------------------------
       #bytes #repetitions      t[usec]   Mbytes/sec
            0         1000         2.23         0.00
            1         1000         2.26         0.42
            2         1000         2.26         0.85
            4         1000         2.26         1.69
            8         1000         2.26         3.38
           16         1000         2.36         6.45
           32         1000         2.57        11.89
           64         1000         2.36        25.81
          128         1000         2.64        46.19
          256         1000         2.73        89.30
          512         1000         3.09       157.99
         1024         1000         3.60       271.53
         2048         1000         4.46       437.57
         4096         1000         6.11       639.23
         8192         1000         7.49      1043.47
        16384         1000         9.76      1600.76
        32768         1000        14.98      2085.77
        65536          640        25.99      2405.08
       131072          320        50.68      2466.64
       262144          160        80.62      3101.01
       524288           80       145.86      3427.91
      1048576           40       279.06      3583.42
      2097152           20       543.37      3680.71
      4194304           10      1082.94      3693.63

# All processes entering MPI_Finalize

```



## <a name="next-steps"></a>Sonraki adımlar
* Dağıtın ve Linux MPI uygulamaları Linux kümenizde çalıştırın.
* Bkz: [Intel MPI kitaplığı belgeleri](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) Intel MPI hakkında yönergeler için.
* Deneyin bir [hızlı başlatma şablonunu](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) CentOS tabanlı HPC görüntüsünü kullanarak bir Intel Lustre küme oluşturmak için. Ayrıntılar için bkz [dağıtma Intel bulut Edition için Microsoft Azure üzerinde Lustre](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).
