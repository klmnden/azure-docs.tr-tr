---
title: MPI uygulamalarını çalıştırmak için bir Linux RDMA kümesi ayarlama | Microsoft Docs
description: Boyut H16r, H16mr, A8 veya A9 Vm'lerde to run MPI uygulamalarını çalıştırmak için Azure RDMA ağ kullanmak için bir Linux kümesi oluşturma
services: virtual-machines-linux
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: 01834bad-c8e6-48a3-b066-7f1719047dd2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/11/2018
ms.author: danlep
ms.openlocfilehash: 471fd4095fe45e76f94df8c61a07eeb9bbc1c120
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38990736"
---
# <a name="set-up-a-linux-rdma-cluster-to-run-mpi-applications"></a>MPI uygulamalarını çalıştırmak için bir Linux RDMA kümesi oluşturma

İle azure'da bir Linux RDMA kümesi ayarlama konusunda bilgi [yüksek performanslı işlem VM boyutları](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) paralel ileti geçiş arabirimi (MPI) uygulamalarını çalıştırmak için. Bu makale, Intel MPI bir kümede çalıştırmak için Linux HPC görüntüsünü hazırlamak için adımları sağlar. Hazırlama sonra bu görüntüyü ve aşağıdakilerden birini kullanarak bir VM kümesi dağıtma [RDMA özellikli Azure VM boyutlarını](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#rdma-capable-instances). Küme, doğrudan uzak bellek erişimi (RDMA) teknolojisini temel alan düşük gecikme süreli, yüksek performanslı bir ağ üzerinden verimli bir şekilde iletişim kuran MPI uygulamalarını çalıştırmak için kullanılır.

> [!IMPORTANT]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Azure Resource Manager](../../../azure-resource-manager/resource-manager-deployment-model.md) ve klasik. Bu makale klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

## <a name="cluster-deployment-options"></a>Küme dağıtım seçenekleri
Bir Linux RDMA kümesi olan veya olmayan bir iş Zamanlayıcısını oluşturmak için kullanabileceğiniz farklı yöntemler aşağıda verilmiştir.

* **Azure CLI betiklerini**: Bu makalenin sonraki bölümlerinde gösterilen şekilde kullanın [Azure komut satırı arabirimi](../../../cli-install-nodejs.md) RDMA özellikli bir VM kümesi dağıtımı komut dosyası için (CLI). CLI Hizmet Yönetimi modunda küme düğümleri seri olarak Klasik dağıtım modelinde oluşturur, böylelikle birçok işlem düğümleri dağıtımı birkaç dakika sürebilir. Klasik dağıtım modeli kullandığınız zaman RDMA ağ bağlantısını etkinleştirmek için sanal makinelerin aynı bulut hizmetinde dağıtın.
* **Azure Resource Manager şablonları**: Resource Manager dağıtım modeli için RDMA ağ bağlanan RDMA özellikli bir VM kümesi dağıtmak için de kullanabilirsiniz. Yapabilecekleriniz [kendi şablonunuzu oluşturun](../../../resource-group-authoring-templates.md), veya [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/documentation/templates/) istediğiniz çözümü dağıtmak için Microsoft veya topluluk tarafından katkıda bulunulan şablonları için. Resource Manager şablonları, bir Linux kümesi dağıtmak için hızlı ve güvenilir bir yol sağlayabilir. Resource Manager dağıtım modeli kullandığınız zaman RDMA ağ bağlantısını etkinleştirmek için aynı kullanılabilirlik kümesine veya sanal makine ölçek kümesi içindeki Vm'leri dağıtın.
* **HPC Pack**: Azure'da bir Microsoft HPC Pack kümesi oluşturma ve çalıştırma RDMA ağ erişmek için desteklenen bir Linux dağıtımı RDMA özellikli işlem düğümleri ekleyebilirsiniz. Daha fazla bilgi için [azure'da HPC Pack kümesinde Linux işlem düğümleri kullanmaya başlama](hpcpack-cluster.md).

## <a name="sample-deployment-steps-in-the-classic-model"></a>Klasik modeli örnek dağıtım adımları
Aşağıdaki adımlarda, Azure Market'ten SUSE Linux Enterprise Server (SLES) 12 HPC VM dağıtma, özelleştirebilir ve özel VM görüntüsü oluşturmak için Azure CLI kullanma gösterilmektedir. Ardından, RDMA özellikli bir VM kümesi dağıtım betiği oluşturmak için görüntü kullanabilirsiniz.

> [!TIP]
> CentOS tabanlı HPC görüntüleri Azure Market'te göre RDMA özellikli bir VM kümesi dağıtmak için benzer adımları kullanın. Bazı adımlar belirtildiği gibi biraz daha farklı.
>

### <a name="prerequisites"></a>Önkoşullar
* **İstemci bilgisayar**: Azure ile iletişim kurmak için bir Mac, Linux veya Windows istemci bilgisayara ihtiyacı vardır. Bu adımlar, Linux istemci kullandığınız varsayılır.
* **Azure aboneliği**: bir aboneliğiniz yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/) yalnızca birkaç dakika içinde. Daha büyük kümeler için bir Kullandıkça Öde aboneliğine veya diğer satın alma seçeneklerini göz önünde bulundurun.
* **VM boyutu kullanılabilirlik**: denetleyin [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/regions/services/) H serisi veya diğer RDMA özellikli bir VM boyutları Azure bölgelerinde genel kullanılabilirliği.
* **Çekirdek kota**: yoğun işlem gücü kullanımlı VM kümesi dağıtmak için çekirdek kotasını artırmanız gerekebilir. Örneğin, bu makalede gösterilen şekilde 8 A9 VM dağıtmak istiyorsanız, en az 128 çekirdek gerekir. Aboneliğiniz ayrıca H serisi dahil olmak üzere belirli sanal makine boyutu aileleri içinde dağıtabileceğiniz çekirdek sayısını sınırlayabilir. Bir kota artırım talebinde bulunmak [bir çevrimiçi müşteri destek isteği açın](../../../azure-supportability/how-to-create-azure-support-request.md) ücret olmadan.
* **Azure CLI**: [yükleme](../../../cli-install-nodejs.md) Azure CLI ve [Azure aboneliğinize bağlanma](/cli/azure/authenticate-azure-cli) istemci bilgisayardan.

İçin ilgili dağıtım konuları da gözden [RDMA özellikli Azure VM boyutlarını](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#rdma-capable-instances).

### <a name="provision-an-sles-12-hpc-vm"></a>Bir 12 SLES HPC VM sağlama
Azure CLI ile azure'da oturum açtıktan sonra çalıştırma `azure config list` için çıkış Hizmet Yönetimi modunda gösterildiğinden emin olun. Kullanmıyorsa, bu komutu çalıştırarak modunu ayarlayın:

    azure config mode asm


Kullanma yetkisi verilen tüm abonelikleri listeleme için şunu yazın:

    azure account list

Geçerli etkin aboneliği ile tanımlanan `Current` kümesine `true`. Bu abonelik kümeyi oluşturmak için kullanmak istediğiniz bir tane yoksa, uygun bir abonelik kimliği etkin bir aboneliğiniz ayarlayın:

    azure account set <subscription-Id>

Azure'da genel kullanıma açık SLES 12 HPC görüntüleri görmek için aşağıdakine benzer bir komut çalıştırın, kabuk ortamını desteklediği varsayılarak **grep**:

    azure vm image list | grep "suse.*hpc"

RDMA özellikli bir VM, SLES 12 SP3 HPC görüntü aşağıdakine benzer bir komut çalıştırarak sağlayın:

    azure vm create -g <username> -p <password> -c <cloud-service-name> -l <location> -z A9 -n <vm-name> -e 22 b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp3-hpc-v20170913

Konumlar:

* Boyutu (Bu örnekte A9), RDMA özellikli bir VM boyutlarını biridir.
* Dış SSH bağlantı noktası numarası (Bu örnekte, SSH varsayılan değer 22), tüm geçerli bağlantı noktası numarasıdır. İç SSH bağlantı noktası 22 olarak ayarlanır.
* Konuma göre belirtilen bir Azure bölgesinde yeni bir bulut hizmeti oluşturulur. Seçtiğiniz VM boyutuna "Batı ABD gibi" kullanılabilir olduğu bir konum belirtin.
* (Bu, ek ücret alınmaz) SUSE öncelik desteği için SLES 12 görüntü adı şu anda sahip seçeneklerden birini olabilir `priority` adı gibi: 

 `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp3-hpc-priority-v20170913`

### <a name="customize-the-vm"></a>VM özelleştirme
VM tamamlandıktan sağlama, sanal makinenin dış IP adresi (veya DNS adı) kullanarak sanal Makineye SSH uygulama ve dış bağlantı noktası numarası yapılandırılmış ve daha sonra özelleştirmek sonra. Bağlantı ayrıntıları için bkz. [Linux çalıştıran bir sanal makine için oturum açma](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Bir adımı tamamlamak için gerekli kök erişimi olmadıkça sanal makinede yapılandırılan kullanıcı olarak komutları gerçekleştirin.

> [!IMPORTANT]
> Microsoft Azure Linux Vm'leri için kök erişim sağlamaz. VM için bir kullanıcı olarak bağlıyken yönetimsel erişim kazanmak için komutları çalıştırmak `sudo`.
>
>

* **Güncelleştirmeleri**: zypper kullanarak güncelleştirmeleri yükleyin. NFS utilities yüklemek isteyebilirsiniz.

  > [!IMPORTANT]
  > SLES 12 HPC VM sürücüler ile Linux RDMA sorunlarına yol açabilir çekirdek güncelleştirmeleri uygulanmaz öneririz.
  >
  >
* **Intel MPI**: aşağıdaki komutu çalıştırarak sanal makine Intel MPI yüklenmesini tamamlamak:

    ```bash
    sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
    ```

* **Bellek kilitleme**: RDMA için kullanılabilir bellek kilitleme için MPI kodları ekleyin veya /etc/security/limits.conf dosyasında aşağıdaki ayarları değiştirebilirsiniz. Bu dosyayı düzenlemek için kök erişmeniz gerekir.

    ```bash
    <User or group name> hard    memlock <memory required for your application in KB>

    <User or group name> soft    memlock <memory required for your application in KB>
    ```

  > [!NOTE]
  > Test amacıyla da memlock sınırsız olarak ayarlayabilirsiniz. Örneğin: `* hard memlock unlimited`. Daha fazla bilgi için [bilinen en iyi yöntemler ayarın kilitli bellek boyutu](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).
  >
  >
* **SLES VM'ler için SSH anahtarları**: küme MPI işlerini çalıştırma, SLES kullanıcı hesabınızın işlem düğümleri arasında güven oluşturma SSH anahtarları. CentOS tabanlı bir HPC VM dağıttıysanız, bu adımı izlemeyin. Görüntü yakalamak ve küme'ı dağıttıktan sonra küme düğümler arasında parolasız SSH güven ayarlamak için bu makaledeki yönergeleri bakın.

  SSH anahtarları oluşturmak için çalıştırılması `ssh-keygen` komutu. Giriş için sorulduğunda **Enter** anahtarları parola ayarlamadan varsayılan konumunda oluşturulacak.

  Bilinen ortak anahtarları için authorized_keys dosya ortak anahtarı ekleyin.

  ```bash
  cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
  ```

  ~/.Ssh dizinde, düzenleme veya oluşturma `config` dosya. Azure'da (Bu örnekte 10.32.0.0/16) kullanmayı planlıyorsanız özel ağda IP adres aralığı belirtin:

  ```bash
  host 10.32.0.*
  StrictHostKeyChecking no
  ```

  Alternatif olarak, kümenizdeki her sanal makinenin özel ağ IP adresini aşağıdaki gibi listeleyin:

  ```bash
  host 10.32.0.1
  StrictHostKeyChecking no
  host 10.32.0.2
  StrictHostKeyChecking no
  host 10.32.0.3
  StrictHostKeyChecking no
  ...
  ```

  > [!NOTE]
  > Yapılandırma `StrictHostKeyChecking no` belirli bir IP adresi veya aralığı belirtilmediğinde olası bir güvenlik riski oluşturabilir.
  >
  >
* **Uygulamaları**: görüntüyü yakalamadan önce diğer özelleştirmeleri gerçekleştirin veya gerekir tüm uygulamaları yükleyin.

### <a name="capture-the-image"></a>Yansıma Yakalama
Görüntü yakalamak için önce Linux VM üzerinde aşağıdaki komutu çalıştırın. Bu komut, VM'nin deprovisions ancak kullanıcı hesapları ve ayarladığınız SSH anahtarları tutar.

```bash
sudo waagent -deprovision
```

İstemci bilgisayarınızdan görüntü yakalamak için aşağıdaki Azure CLI komutlarını çalıştırabilirsiniz. Daha fazla bilgi için [klasik bir Linux sanal makinesini görüntü olarak yakalama](capture-image-classic.md).  

```
azure vm shutdown <vm-name>

azure vm capture -t <vm-name> <image-name>

```

Bu komutları çalıştırdıktan sonra kullanımınız için VM görüntüsü yakalanır ve sanal makine silinir. Artık özel görüntünüzü bir küme dağıtmak hazır olması.

### <a name="deploy-a-cluster-with-the-image"></a>Görüntü ile bir küme dağıtma
Aşağıdaki Bash betiği, ortamınız için uygun değerlerle değiştirin ve istemci bilgisayarınızdan çalıştırın. Azure Klasik dağıtım modelinde seri olarak Vm'leri dağıtır, çünkü bu betikte önerilen sekiz A9 Vm'leri dağıtmak için birkaç dakika sürer.

```bash
#!/bin/bash -x
# Script to create a compute cluster without a scheduler in a classic VNet in Azure
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
        azure vm create -g <username> -p <password> -c <cloud-service-name> -z A9 -n $vmname$i -e $portnumber$i -w <network-name> -b Subnet-1 <image-name>;
done

# Save this script with a name like makecluster.sh and run it in your shell environment to provision your cluster
```

## <a name="considerations-for-a-centos-hpc-cluster"></a>CentOS HPC Kümesi için dikkat edilmesi gerekenler
Bir Azure Marketi'nde SLES 12 yerine HPC CentOS tabanlı görüntüler için HPC tabanlı bir küme ayarlamak istiyorsanız, önceki bölümde genel adımları izleyin. Sağlayın ve VM'yi yapılandırmak aşağıdaki farklılıklara dikkat edin:

- Intel MPI bir CentOS tabanlı HPC görüntüsünden bir VM'de zaten yüklü.
- Kilit bellek ayarlarını, sanal makinenin /etc/security/limits.conf dosyasında zaten eklenmiş.
- Yakalama için SSH anahtarları, sağlamanız VM'deki oluşturmaz. Küme dağıtıldıktan sonra bunun yerine, kullanıcı tabanlı kimlik doğrulaması ayarlarken öneririz. Daha fazla bilgi için aşağıdaki bölüme bakın.  

### <a name="set-up-passwordless-ssh-trust-on-the-cluster"></a>Küme üzerinde parolasız SSH güven ayarlama
CentOS tabanlı HPC kümesinde, işlem düğümleri arasında güven oluşturma için iki yöntem vardır: ana bilgisayar tabanlı kimlik doğrulaması ve kullanıcı tabanlı kimlik doğrulaması. Ana bilgisayar tabanlı kimlik doğrulaması bu makalenin kapsamı dışında olan ve genel olarak dağıtımı sırasında bir uzantı komut dosyası yapılmalıdır. Kullanıcı tabanlı kimlik doğrulaması, dağıtımdan sonra güven oluşturma için kullanışlıdır ve oluşturma ve kümeye SSH anahtarları işlem düğümleri arasında paylaşılmasını gerektirir. Bu yöntem genellikle parolasız SSH oturumu açma olarak da bilinir ve MPI işlerini çalıştırırken gereklidir.

Örnek bir betik `user_authentication.sh` CentOS tabanlı bir HPC kullanıcı tabanlı kimlik doğrulamasını etkinleştirmek için küme izler:

```bash
#!/bin/bash
# For CentOS user must first install epel-release, sshpass, and nmap (sshpass and nmap are available from epel-release for CentOS)

# usage ./user_authentication.sh [username] [password] [internalIP prefix]
# ./user_authentication.sh azureuser Azure@123 10.32.0
USER=$1
PASS=$2
IPPRE=$3
HEADNODE=`hostname`

mkdir -p .ssh
echo -e  'y\n' | ssh-keygen -f .ssh/id_rsa -t rsa -N ''

echo 'Host *' >> .ssh/config
echo 'StrictHostKeyChecking no' >> .ssh/config
chmod 400 .ssh/config
chown azureuser:azureuser /home/azureuser/.ssh/config

nmap -sn $IPPRE.* | grep $IPPRE. | awk '{print $5}' > nodeips.txt
for NAME in `cat nodeips.txt`; do sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'hostname' >> nodenames.txt;done

NAMES=`cat nodenames.txt` #names from names.txt file
for NAME in $NAMES; do
    sshpass -p $PASS scp -o "StrictHostKeyChecking no" -o ConnectTimeout=2 /home/$USER/nodenames.txt $USER@$NAME:/home/$USER/
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME "mkdir .ssh && chmod 700 .ssh"
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME "echo -e  'y\n' | ssh-keygen -f .ssh/id_rsa -t rsa -N ''"
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'touch /home/'$USER'/.ssh/config'
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'echo "Host *" >  /home/'$USER'/.ssh/config'
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'echo StrictHostKeyChecking no >> /home/'$USER'/.ssh/config'
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'chmod 400 /home/'$USER'/.ssh/config'
    cat .ssh/id_rsa.pub | sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'cat >> .ssh/authorized_keys'
    sshpass -p $PASS scp -o "StrictHostKeyChecking no" -o ConnectTimeout=2 $USER@$NAME:/home/$USER/.ssh/id_rsa.pub .ssh/sub_node.pub

    for SUBNODE in `cat nodeips.txt`; do
         sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$SUBNODE 'mkdir -p .ssh'
         cat .ssh/sub_node.pub | sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$SUBNODE 'cat >> .ssh/authorized_keys'
    done
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'chmod 700 .ssh/'
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'chmod 640 .ssh/authorized_keys'
done
```

Aşağıdaki adımları kullanarak bu betiği çalıştırın. Ayrıca, bu komut dosyasını değiştirdiğinizde veya küme işlem düğümleri arasında parolasız SSH kimlik doğrulaması'kurmak için başka bir yöntemi kullanabilirsiniz.

Betiği çalıştırmak için alt ağ IP adresi için önek bilmeniz gerekir. Ön ek, küme düğümlerinden biri üzerinde aşağıdaki komutu çalıştırarak alın. Çıkış 10.1.3.5 aşağıdakine benzer olmalıdır ve 10.1.3 önekidir bölümü.

```bash
ifconfig eth0 | grep -w inet | awk '{print $2}'
```

Artık üç parametre kullanarak betiği çalıştırın: işlem düğümlerinde ortak kullanıcı adı, işlem düğümleri ve önceki komuttan döndürülen alt ağ ön eki söz konusu kullanıcı için ortak parola.

```bash
./user_authentication.sh <myusername> <mypassword> 10.1.3
```

Bu betik şunları yapar:

* Parolasız oturum açma için gerekli olduğu .ssh, adlı konak düğüm üzerinde bir dizin oluşturur.
* Kümedeki herhangi bir düğümden oturum açma izni parolasız oturum açma bildirir .ssh dizine bir yapılandırma dosyası oluşturur.
* Düğüm adları ve düğüm kümedeki tüm düğümler için IP adreslerini içeren bir dosya oluşturur. Bu dosyalar daha sonra başvurmak için komut dosyası çalıştırıldıktan sonra bırakılır.
* Her küme düğümü (ana bilgisayar düğümü dahil) için bir genel ve özel anahtar çifti oluşturur ve girişleri authorized_keys dosyası oluşturur.

> [!WARNING]
> Bu betiğin çalıştırılması, olası bir güvenlik riski oluşturabilir. Ortak anahtar bilgilerini ~/.ssh içinde olmayan dağıtılmış emin olun.
>

## <a name="run-mpi-on-a-basic-two-node-cluster"></a>MPI temel iki düğümlü bir kümede çalıştırın
Zaten yapmadıysanız, aynı zamanda Intel MPI için önce ortamı ayarlayın.

```bash
# For a SLES 12 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh
```

### <a name="run-an-mpi-command"></a>MPI komutu çalıştırın
MPI komut MPI düzgün yüklendiğinden ve arasında en az iki işlem düğümleriyle iletişim kurabilmesi göstermek için işlem düğümlerinden biri üzerinde çalıştırın. Aşağıdaki **mpırun** komutu çalıştırmaları **ana bilgisayar adı** iki düğüm üzerinde komutu. İçin `-hosts` parametre, iki düğüm IP adresleri Azure Vnet'te (örneğin, 10.32.0.4,10.32.0.5) geçirin.

```bash
mpirun -ppn 1 -n 2 -hosts <host1>,<host2> -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname
```
Çıkış için giriş olarak geçirilen tüm düğümleri adları listelemelidir `-hosts`. Örneğin, bir **mpırun** iki düğümle komut aşağıdaki gibi bir çıktı döndürür:

```
cluster11
cluster12
```

### <a name="run-an-mpi-benchmark"></a>MPI Kıyaslama çalıştırın
RDMA ağ bağlantısı ve küme yapılandırmayı doğrulamak için pingpong Kıyaslama aşağıdaki Intel MPI komutu çalıştırır.

```bash
mpirun -hosts <host1>,<host2> -ppn 1 -n 2 -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong
```

İki düğümle bir çalışma kümesinde aşağıdaki gibi bir çıktı görmeniz gerekir. Azure RDMA ağ üzerinde veya iletinin 3 mikro saniyenin altında gecikme süresi en fazla 512 bayt boyutları bekler.

```
#------------------------------------------------------------
#    Intel (R) MPI Benchmarks 4.0 Update 1, MPI-1 part
#------------------------------------------------------------
# Date                  : Fri Jul 6 17:16:46 2018
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
### <a name="sample-mpi-run-script"></a>Örnek MPI betiği çalıştırın
İşte bir örnek batch betiği MPI uygulamanın çalıştırılması için gereken değişkenleri yapılandırın. Yola değiştirmek `mpivars.sh` Intel MPI yüklemeniz için gerektiği şekilde.

```bash
#!/bin/bash -x

# For a SLES 12 HPC cluster

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

# Command line to run the MPI job. Substitute with values appropriate for your application.

mpirun -n <number-of-cores> -ppn <core-per-node> -hostfile <hostfilename>  /path <path to the application exe> <arguments specific to the application>

#end
```

Konak dosyasının biçimi aşağıdaki gibidir. Kümedeki her düğüm için bir satır ekleyin. Sanal ağ özel IP adreslerini DNS adları daha önce tanımlanan belirtin. Örneğin, IP adresleriyle 10.32.0.4 ve 10.32.0.4 iki ana bilgisayarı için dosyası şunları içerir:

```bash
10.32.0.4:16
10.32.0.5:16
```


## <a name="next-steps"></a>Sonraki adımlar
* Dağıtın ve kendi Linux MPI uygulamaları Linux kümenizde çalıştırın.
* Bkz: [Intel MPI kitaplığı belgeleri](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) Intel MPI hakkında yönergeler için.
* Deneyin bir [Hızlı Başlangıç şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) HPC CentOS tabanlı bir görüntü kullanarak bir Intel Lustre kümesi oluşturmak için. 
