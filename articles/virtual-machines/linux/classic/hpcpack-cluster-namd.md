---
title: Linux sanal makineleri üzerinde Microsoft HPC paketi ile NAMD | Microsoft Docs
description: Azure Microsoft HPC Pack kümede dağıtmak ve NAMD benzetimi charmrun ile birden çok Linux işlem düğümlerinde çalışacak.
services: virtual-machines-linux
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: 76072c6b-ac35-4729-ba67-0d16f9443bd7
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 10/13/2016
ms.author: danlep
ms.openlocfilehash: 61dd49d4bd3183b6b9a78036d6d7d01798e4dc89
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="run-namd-with-microsoft-hpc-pack-on-linux-compute-nodes-in-azure"></a>Azure’daki Linux işlem düğümlerinde Microsoft HPC Pack ile NAMD çalıştırma
Bu makalede, Linux yüksek performanslı bilgi işlem (HPC) iş yükü Azure sanal makineler üzerinde çalıştırılacak bir yolu gösterir. Burada, ayarladığınız bir [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) Azure üzerinde Linux işlem düğümü ile küme ve Çalıştır bir [NAMD](http://www.ks.uiuc.edu/Research/namd/) hesaplamak ve büyük biomolecular sistem yapısını görselleştirmek için benzetimi.  

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

* **NAMD** (Nanoscale Molecular Dynamics için) programıdır kadar Atom milyonlarca içeren büyük biomolecular sistemleri yüksek performanslı benzetimi için tasarlanmış bir paralel molecular dynamics paketi. Bu sistemlere örnek olarak, virüsler, hücre yapıları ve büyük proteins içerir. NAMD tipik benzetimleri çekirdeği yüzlerce ve en büyük benzetimleri için 500. 000'den fazla çekirdek ölçeklendirir.
* **Microsoft HPC Pack** büyük ölçekli HPC ve paralel uygulamalar şirket içi bilgisayarları veya Azure sanal makineleri kümelerde çalıştırmak için özellikleri sağlar. İlk olarak Windows HPC iş yükleri, HPC paketi için bir çözüm olarak geliştirilen şimdi Linux HPC uygulamaları Linux üzerinde çalışan destekleyen bir HPC Pack kümede dağıtılan düğümü VM'ler işlem. Bkz: [Linux işlem düğümlerini Azure bir HPC Pack kümesindeki kullanmaya başlama](hpcpack-cluster.md) giriş.

## <a name="prerequisites"></a>Önkoşullar
* **HPC Pack küme Linux işlem düğümlerini** -ya da kullanarak Azure Linux işlem düğümlerini içeren bir HPC Pack kümeyi dağıtma bir [Azure Resource Manager şablonu](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) veya bir [Azure PowerShell Betiği](hpcpack-cluster-powershell-script.md). Bkz: [Linux işlem düğümlerini Azure bir HPC Pack kümesindeki kullanmaya başlama](hpcpack-cluster.md) Önkoşullar ve adımlar her iki seçenek için. PowerShell komut dosyası dağıtım seçeneği seçerseniz, bu makalenin sonunda örnek dosyalarını örnek yapılandırma dosyasına bakın. Bu dosya bir Windows Server 2012 R2 baş düğüm ve dört boyutu büyük CentOS 6.6 işlem düğümleri oluşan bir HPC Pack Azure tabanlı küme yapılandırır. Bu dosya, ortamınız için gerektiği gibi özelleştirin.
* **NAMD yazılım ve öğretici dosyaları** -Linux indirme NAMD yazılımı [NAMD](http://www.ks.uiuc.edu/Research/namd/) site (kayıt gerekli). Bu makalede sürüm 2.10 NAMD üzerinde temel alır ve kullanır [Linux-x86_64 (64-bit Intel/AMD Ethernet ile)](http://www.ks.uiuc.edu/Development/Download/download.cgi?UserID=&AccessCode=&ArchiveID=1310) arşiv. Ayrıca karşıdan [NAMD öğreticileri](http://www.ks.uiuc.edu/Training/Tutorials/#namd). Karşıdan yüklemeler .tar dosyaları ve küme baş düğümüne dosyalarını ayıklamak için bir Windows Aracı gerekir. Dosyaları ayıklamak için daha sonra bu makaledeki yönergeleri izleyin. 
* **VMD** (isteğe bağlı) - NAMD işinizin sonuçları görmek karşıdan yükleyip molecular görselleştirme program [VMD](http://www.ks.uiuc.edu/Research/vmd/) tercih ettiğiniz bir bilgisayarda. 1.9.2 geçerli sürümdür. Başlamak için site karşıdan VMD bakın.  

## <a name="set-up-mutual-trust-between-compute-nodes"></a>İşlem düğümleri arasında karşılıklı güven ayarlama
Bir geçici düğüm işi birden çok Linux düğümler üzerinde çalışan düğümleri birbirine güvenen gerektirir (tarafından **rsh** veya **ssh**). Microsoft HPC Pack Iaas dağıtım komut dosyası ile HPC Pack kümesi oluşturduğunuzda, betik kalıcı karşılıklı güven belirttiğiniz yönetici hesabı için otomatik olarak ayarlar. Kümenin etki alanında oluşturduğunuz yönetici olmayan kullanıcılar için düğümler arasında geçici karşılıklı güveni bir iş için ayrılmış ayarlamanız gerekir. Ardından, işi tamamlandıktan sonra ilişki yok. Her kullanıcı için bunun için bir RSA anahtar çifti HPC Pack güven ilişkisi kurmak için kullandığı kümeye sağlayın. Yönergeleri izleyin.

### <a name="generate-an-rsa-key-pair"></a>RSA anahtar çifti oluşturma
Linux çalıştırarak bir ortak anahtar ve özel anahtarı içeren bir RSA anahtar çifti oluşturmak kolaydır **ssh-keygen** komutu.

1. Bir Linux bilgisayara oturum açın.
2. Şu komutu çalıştırın:
   
   ```bash
   ssh-keygen -t rsa
   ```
   
   > [!NOTE]
   > Tuşuna **Enter** komut tamamlanıncaya kadar varsayılan ayarları kullanmak için. Burada bir parola girmeyin; için bir parola istendiğinde, yalnızca basın **Enter**.
   > 
   > 
   
   ![RSA anahtar çifti oluşturma][keygen]
3. Dizin ~/.ssh dizine geçin. Özel anahtar id_rsa ve id_rsa.pub ortak anahtarında depolanır.
   
   ![Özel ve genel anahtarlar][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>Anahtar çiftini HPC Pack kümeye ekleme
1. [Tarafından Uzak Masaüstü Bağlantısı](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (örneğin, hpc\clusteradmin) küme dağıttığınızda sağlanan üstbilgi düğüm VM'ine etki alanını kullanarak, kimlik bilgileri. Küme baş düğümünden yönetin.
2. Kümenin Active Directory etki alanında bir etki alanı kullanıcı hesabı oluşturmak için standart Windows Server yordamları kullanın. Örneğin, Active Directory Kullanıcıları ve Bilgisayarları aracını baş düğümünde kullanın. Bu makaledeki örneklerde hpcuser hpclab etki alanında (hpclab\hpcuser) adlı bir etki alanı kullanıcısı oluşturun varsayalım.
3. Etki alanı kullanıcısı bir küme kullanıcı HPC Pack kümeye ekleyin. Yönergeler için bkz: [ekleme veya kaldırma küme kullanıcılar](https://technet.microsoft.com/library/ff919330.aspx).
4. C:\cred.xml adlı bir dosya oluşturun ve RSA anahtar veri dosyasını buraya kopyalayın. Bu makalenin sonunda örnek dosyalarını bir örnek bulabilirsiniz.
   
   ```
   <ExtendedData>
     <PrivateKey>Copy the contents of private key here</PrivateKey>
     <PublicKey>Copy the contents of public key here</PublicKey>
   </ExtendedData>
   ```
5. Bir komut istemi açın ve hpclab\hpcuser hesabı için kimlik bilgilerini veri kümesi için aşağıdaki komutu girin. Kullandığınız **extendeddata** dosyasının adı oluşturduğunuz C:\cred.xml anahtar verilerine yönelik iletilecek parametre.
   
   ```command
   hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
   ```
   
   Bu komut çıktısı başarıyla tamamlanır. İşlerini çalıştırmak için gereken kullanıcı hesapları için kimlik bilgilerini ayarlama sonra cred.xml dosyayı güvenli bir konumda depolayın veya silin.
6. RSA anahtar çifti Linux düğümlerinden biri üzerinde oluşturursa, bunları kullanmayı bitirdikten sonra anahtarlarını silin unutmayın. Varolan bir id_rsa dosya veya id_rsa.pub dosyası bulursa, HPC Pack karşılıklı güven ayarlı değil.

> [!IMPORTANT]
> Bir yönetici tarafından gönderilen bir işin Linux düğümleri kök hesapta çalıştığından Linux iş paylaşılan bir kümede bir Küme Yöneticisi olarak çalışan öneririz yok. Yönetici olmayan bir kullanıcı tarafından gönderilen bir işi işi kullanıcı aynı ada sahip bir yerel Linux kullanıcı hesabı altında çalışır. Bu durumda, HPC Pack karşılıklı güven bu Linux kullanıcı için iş için ayrılan tüm düğümlere ayarlar. Linux kullanıcı el ile Linux düğümleri üzerinde iş çalıştırmadan önce ayarlayabilirsiniz veya iş gönderildiğinde HPC Pack kullanıcı otomatik olarak oluşturur. HPC Pack kullanıcı oluşturursa, HPC Pack işi tamamlandıktan sonra onu siler. Bir güvenlik tehdidi azaltmak için düğümler üzerinde iş tamamlandıktan sonra anahtarları kaldırılır.
> 
> 

## <a name="set-up-a-file-share-for-linux-nodes"></a>Linux düğümleri için bir dosya paylaşımı ayarlama
Şimdi bir SMB dosya paylaşımı ayarlama ve ortak bir yol ile NAMD dosyalara erişmek Linux düğümleri izin vermek için tüm Linux düğümlerdeki paylaşılan klasöre bağlayın. Baş düğüm üzerinde paylaşılan bir klasöre bağlamak için adımlar aşağıda belirtilmiştir. Bir paylaşım için CentOS 6.6 gibi Azure dosya hizmeti şu anda desteklenmiyor dağıtımları önerilir. Linux düğümleri bir Azure dosya paylaşımı desteği olup [Azure File storage'ı Linux ile kullanma konusunda](../../../storage/files/storage-how-to-use-files-linux.md). Ek dosya paylaşımı ile HPC Pack seçenekleri için bkz: [Linux işlem düğümlerini Azure bir HPC Pack kümesindeki kullanmaya başlama](hpcpack-cluster.md).

1. Baş düğüm üzerinde bir klasör oluşturun ve herkese okuma/yazma ayrıcalıklarına ayarlayarak paylaşın. Bu örnekte, \\ \\CentOS66HN\Namd CentOS66HN baş düğümü ana bilgisayar adı olduğu klasörü, adıdır.
2. Paylaşılan klasörde namd2 adlı bir alt klasör oluşturun. Namd2 içinde namdsample adlı başka bir alt klasör oluşturun.
3. Bir Windows sürümünü kullanarak klasördeki NAMD dosyaları ayıklayın **tar** veya .tar arşivler üzerinde çalıştığı başka bir Windows yardımcı programı. 
   
   * NAMD tar arşive ayıklamak \\ \\CentOS66HN\Namd\namd2.
   * Altında öğretici dosyaları ayıklayın \\ \\CentOS66HN\Namd\namd2\namdsample.
4. Bir Windows PowerShell penceresi açın ve paylaşılan klasör Linux düğümlerinde bağlamak için aşağıdaki komutları çalıştırın.
   
    ```command
    clusrun /nodegroup:LinuxNodes mkdir -p /namd2
   
    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS66HN/Namd/namd2 /namd2 -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

İlk komut LinuxNodes grubundaki tüm düğümlerde /namd2 adlı bir klasör oluşturur. İkinci komut dir_mode klasörüyle üzerine paylaşılan klasör //CentOS66HN/Namd/namd2 bağlar ve file_mode BITS 777 ayarlayın. *Kullanıcıadı* ve *parola* komutta baş düğüm bir kullanıcının kimlik bilgileri olmalıdır.

> [!NOTE]
> "\`" İkinci komut dosyasındaki simge olan bir PowerShell kaçış simgesi. "\`," "," (virgül karakteri) anlamına gelir komutu, bir parçasıdır.
> 
> 

## <a name="create-a-bash-script-to-run-a-namd-job"></a>Bir NAMD işini çalıştırmak için bir Bash komut dosyası oluşturma
NAMD işinizi gereken bir *listesi* dosya **charmrun** NAMD işlemlerini başlatma sırasında kullanmak için düğüm sayısını belirlemek için. Listesi dosyası oluşturur ve çalışan bir Bash komut dosyası kullanarak **charmrun** bu listesi dosyayla. Ardından NAMD işi HPC Kümesi bu komut dosyasını çağıran Yöneticisi'nde gönderebilirsiniz.

Tercih ettiğiniz bir metin düzenleyicisi kullanarak NAMD program dosyalarını içeren /namd2 klasöründe bir Bash komut dosyası oluşturabilir ve hpccharmrun.sh adlandırın. Hızlı bir kavram kanıtı için bu makalenin sonunda sağlanan örnek hpccharmrun.sh komut dosyasını kopyalayın ve Git [NAMD işi göndermek](#submit-a-namd-job).

> [!TIP]
> Satır sonlarını (yalnızca LF, CR LF) kodunuzu Linux bir metin dosyası olarak kaydedin. Bu, düzgün şekilde Linux düğümlerinde çalıştığını sağlar.
> 
> 

Bu bash betiğin yaptığı hakkında ayrıntılar verilmiştir. 

1. Bazı değişkenleri tanımlayın.
   
   ```bash
   #!/bin/bash
   
   # The path of this script
   SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
   # Charmrun command
   CHARMRUN=${SCRIPT_PATH}/charmrun
   # Argument of ++nodelist
   NODELIST_OPT="++nodelist"
   # Argument of ++p
   NUMPROCESS="+p"
   ```
2. Ortam değişkenlerinin düğümü bilgi alın. $NODESCORES $CCP_NODES_CORES bölünmüş sözcükleri listesini depolar. $COUNT $NODESCORES boyutudur.
   
   ```
   # Get node information from the environment variables
   NODESCORES=(${CCP_NODES_CORES})
   COUNT=${#NODESCORES[@]}
   ```    
   
   $CCP_NODES_CORES değişkeni biçimi aşağıdaki gibidir:
   
   ```
   <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>…
   ```
   
   Bu değişken düğümler, düğüm adı ve her düğümde projeye ayrılan çekirdek sayısı toplam sayısını listeler. İşi çalıştırmak için 10 çekirdekler gerekirse, örneğin, $CCP_NODES_CORES değerini benzer:
   
   ```
   3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 2
   ```
3. $CCP_NODES_CORES değişken ayarlanmamışsa, başlangıç **charmrun** doğrudan. (Bu komut dosyasını doğrudan Linux düğümlerinde çalıştırdığınızda bu yalnızca olmalıdır.)
   
   ```
   if [ ${COUNT} -eq 0 ]
   then
     # CCP_NODES is_CORES is not found or is empty, so just run charmrun without nodelist arg.
     #echo ${CHARMRUN} $*
     ${CHARMRUN} $*
   ```
4. Veya bir listesi dosyası oluşturma **charmrun**.
   
   ```
   else
     # Create the nodelist file
     NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$
   
     # Write the head line
     echo "group main" > ${NODELIST_PATH}
   
     # Get every node name and number of cores and write into the nodelist file
     I=1
     while [ ${I} -lt ${COUNT} ]
     do
         echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
         let "I=${I}+2"
     done
   ```
5. Çalıştırma **charmrun** listesi dosyayla dönüş durumunu almak ve sonunda listesi dosyasını kaldırın.
   
   ${CCP_NUMCPUS} HPC paketi üstbilgi düğümü tarafından ayarlanmış başka bir ortam değişkenidir. Bu iş için ayrılan toplam çekirdek sayısı depolar. Bu işlemler için charmrun sayısını belirtmek için kullanıyoruz.
   
   ```
   # Run charmrun with nodelist arg
   #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
   ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
   
   RTNSTS=$?
   rm -f ${NODELIST_PATH}
   fi
   
   ```
6. Çıkış **charmrun** dönüş durumu.
   
   ```
   exit ${RTNSTS}
   ```

Komut dosyası oluşturur listesi dosyasındaki bilgiler aşağıdadır:

```
group main
host <Name of node1> ++cpus <Cores of node1>
host <Name of node2> ++cpus <Cores of node2>
…
```

Örneğin:

```
group main
host CENTOS66LN-00 ++cpus 4
host CENTOS66LN-01 ++cpus 4
host CENTOS66LN-03 ++cpus 2
```

## <a name="submit-a-namd-job"></a>NAMD işi gönderin
Şimdi NAMD işi HPC Küme Yöneticisi'nde göndermek hazırsınız.

1. Küme baş düğümüne bağlanmak ve HPC Küme Yöneticisi'ni başlatın.
2. İçinde **kaynak yönetimi**, Linux işlem düğümü olduğundan emin olun **çevrimiçi** durumu. Değilse, bunları seçin ve tıklatın **çevrimiçine**.
3. İçinde **iş yönetimi**, tıklatın **yeni iş**.
4. İş için bir ad girin *hpccharmrun*.
   
   ![New HPC job][namd_job]
5. Üzerinde **iş ayrıntılarını** sayfasında **iş kaynakları**, kaynak olarak türünü seçin **düğümü** ve ayarlayın **Minimum** 3. , şu üç Linux düğümlerinde işini çalıştırın ve her düğümün dört çekirdeğe sahip.
   
   ![İş kaynakları][job_resources]
6. Tıklatın **Düzenle görevleri** sol gezinti ve ardından **Ekle** iş için bir görev eklemek için.    
7. Üzerinde **görev ayrıntılarını ve g/ç yönlendirmesi** sayfasında, aşağıdaki değerleri ayarlayın:
   
   * **Komut satırı** -
     `/namd2/hpccharmrun.sh ++remote-shell ssh /namd2/namd2 /namd2/namdsample/1-2-sphere/ubq_ws_eq.conf > /namd2/namd2_hpccharmrun.log`
     
     > [!TIP]
     > Yukarıdaki komut satırının satır sonu olmayan tek bir komuttur. Birkaç satırdaki altında görünmesi sarmalar **komut satırı**.
     > 
     > 
   * **Çalışma dizini** -/namd2
   * **Minimum** - 3
     
     ![Görev ayrıntıları][task_details]
     
     > [!NOTE]
     > İçin çalışma dizini Burada ayarlanan **charmrun** her düğümde aynı çalışma dizinine gidin dener. Çalışma dizini ayarlanmamışsa HPC Pack komut Linux düğümlerinden biri üzerinde oluşturduğunuz rastgele adlandırılmış bir klasördeki başlatır. Bu, diğer düğümlerde aşağıdaki hata neden olur: `/bin/bash: line 37: cd: /tmp/nodemanager_task_94_0.mFlQSN: No such file or directory.` bu sorunu önlemek için tüm düğümler tarafından çalışma dizini olarak erişilebilen bir klasör yolu belirtin.
     > 
     > 
8. Tıklatın **Tamam** ve ardından **gönderme** bu işi çalıştırmak için.
   
   Varsayılan olarak, geçerli oturum açmış kullanıcı hesabınız olarak işi HPC paketi gönderir. Bir iletişim kutusu tıklattıktan sonra kullanıcı adı ve parola girmenizi isteyebilir **gönderme**.
   
   ![İş kimlik bilgileri][creds]
   
   Bazı koşullarda HPC Pack önce giriş ve bu iletişim kutusunu göstermez kullanıcı bilgilerini hatırlıyor. HPC Pack yeniden Göster yapmak için bir komut isteminde aşağıdaki komutu girin ve sonra işi göndermek.
   
   ```command
   hpccred delcreds
   ```
9. İşin tamamlanması birkaç dakika sürer.
10. İş günlüğü Bul \\ <headnodeName>\Namd\namd2\namd2_hpccharmrun.log ve çıktı dosyalarında \\ <headnodeName>\Namd\namd2\namdsample\1-2-sphere\.
11. İsteğe bağlı olarak, iş sonuçları görüntülemek için VMD başlatın. Bu makalenin kapsamı dışındadır dosyalarında (Bu durumda, bir ubiquitin protein molecule su küre içinde) olan NAMD görselleştirme adımlarını çıktı. Bkz: [NAMD öğretici](http://www.life.illinois.edu/emad/biop590c/namd-tutorial-unix-590C.pdf) Ayrıntılar için.
    
    ![İş sonuçları][vmd_view]

## <a name="sample-files"></a>Örnek dosyaları
### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>PowerShell komut dosyası tarafından küme dağıtımı için örnek XML yapılandırma dosyası
```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
      <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS66HN</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS66LN-%00%</VMNamePattern>
    <ServiceName>MyLnxCNService</ServiceName>
     <VMSize>Large</VMSize>
     <NodeCount>4</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-66-20150325</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>    
```

### <a name="sample-credxml-file"></a>Örnek cred.xml dosyası
```
<ExtendedData>
  <PrivateKey>-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAxJKBABhnOsE9eneGHvsjdoXKooHUxpTHI1JVunAJkVmFy8JC
qFt1pV98QCtKEHTC6kQ7tj1UT2N6nx1EY9BBHpZacnXmknpKdX4Nu0cNlSphLpru
lscKPR3XVzkTwEF00OMiNJVknq8qXJF1T3lYx3rW5EnItn6C3nQm3gQPXP0ckYCF
Jdtu/6SSgzV9kaapctLGPNp1Vjf9KeDQMrJXsQNHxnQcfiICp21NiUCiXosDqJrR
AfzePdl0XwsNngouy8t0fPlNSngZvsx+kPGh/AKakKIYS0cO9W3FmdYNW8Xehzkc
VzrtJhU8x21hXGfSC7V0ZeD7dMeTL3tQCVxCmwIDAQABAoIBAQCve8Jh3Wc6koxZ
qh43xicwhdwSGyliZisoozYZDC/ebDb/Ydq0BYIPMiDwADVMX5AqJuPPmwyLGtm6
9hu5p46aycrQ5+QA299g6DlF+PZtNbowKuvX+rRvPxagrTmupkCswjglDUEYUHPW
05wQaNoSqtzwS9Y85M/b24FfLeyxK0n8zjKFErJaHdhVxI6cxw7RdVlSmM9UHmah
wTkW8HkblbOArilAHi6SlRTNZG4gTGeDzPb7fYZo3hzJyLbcaNfJscUuqnAJ+6pT
iY6NNp1E8PQgjvHe21yv3DRoVRM4egqQvNZgUbYAMUgr30T1UoxnUXwk2vqJMfg2
Nzw0ESGRAoGBAPkfXjjGfc4HryqPkdx0kjXs0bXC3js2g4IXItK9YUFeZzf+476y
OTMQg/8DUbqd5rLv7PITIAqpGs39pkfnyohPjOe2zZzeoyaXurYIPV98hhH880uH
ZUhOxJYnlqHGxGT7p2PmmnAlmY4TSJrp12VnuiQVVVsXWOGPqHx4S4f9AoGBAMn/
vuea7hsCgwIE25MJJ55FYCJodLkioQy6aGP4NgB89Azzg527WsQ6H5xhgVMKHWyu
Q1snp+q8LyzD0i1veEvWb8EYifsMyTIPXOUTwZgzaTTCeJNHdc4gw1U22vd7OBYy
nZCU7Tn8Pe6eIMNztnVduiv+2QHuiNPgN7M73/x3AoGBAOL0IcmFgy0EsR8MBq0Z
ge4gnniBXCYDptEINNBaeVStJUnNKzwab6PGwwm6w2VI3thbXbi3lbRAlMve7fKK
B2ghWNPsJOtppKbPCek2Hnt0HUwb7qX7Zlj2cX/99uvRAjChVsDbYA0VJAxcIwQG
TxXx5pFi4g0HexCa6LrkeKMdAoGAcvRIACX7OwPC6nM5QgQDt95jRzGKu5EpdcTf
g4TNtplliblLPYhRrzokoyoaHteyxxak3ktDFCLj9eW6xoCZRQ9Tqd/9JhGwrfxw
MS19DtCzHoNNewM/135tqyD8m7pTwM4tPQqDtmwGErWKj7BaNZARUlhFxwOoemsv
R6DbZyECgYEAhjL2N3Pc+WW+8x2bbIBN3rJcMjBBIivB62AwgYZnA2D5wk5o0DKD
eesGSKS5l22ZMXJNShgzPKmv3HpH22CSVpO0sNZ6R+iG8a3oq4QkU61MT1CfGoMI
a8lxTKnZCsRXU1HexqZs+DSc+30tz50bNqLdido/l5B4EJnQP03ciO0=
-----END RSA PRIVATE KEY-----</PrivateKey>
  <PublicKey>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDEkoEAGGc6wT16d4Ye+yN2hcqigdTGlMcjUlW6cAmRWYXLwkKoW3WlX3xAK0oQdMLqRDu2PVRPY3qfHURj0EEellpydeaSekp1fg27Rw2VKmEumu6Wxwo9HddXORPAQXTQ4yI0lWSerypckXVPeVjHetbkSci2foLedCbeBA9c/RyRgIUl227/pJKDNX2Rpqly0sY82nVWN/0p4NAyslexA0fGdBx+IgKnbU2JQKJeiwOomtEB/N492XRfCw2eCi7Ly3R8+U1KeBm+zH6Q8aH8ApqQohhLRw71bcWZ1g1bxd6HORxXOu0mFTzHbWFcZ9ILtXRl4Pt0x5Mve1AJXEKb username@servername;</PublicKey>
</ExtendedData>
```

### <a name="sample-hpccharmrunsh-script"></a>Örnek hpccharmrun.sh komut dosyası
```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
# Charmrun command
CHARMRUN=${SCRIPT_PATH}/charmrun
# Argument of ++nodelist
NODELIST_OPT="++nodelist"
# Argument of ++p
NUMPROCESS="+p"

# Get node information from ENVs
# CCP_NODES_CORES=3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 4
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # If CCP_NODES_CORES is not found or is empty, just run the charmrun without nodelist arg.
    #echo ${CHARMRUN} $*
    ${CHARMRUN} $*
else
    # Create the nodelist file
    NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

    # Write the head line
    echo "group main" > ${NODELIST_PATH}

    # Get every node name & cores and write into the nodelist file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}
```

 

<!--Image references-->
[keygen]:media/hpcpack-cluster-namd/keygen.png
[keys]:media/hpcpack-cluster-namd/keys.png
[namd_job]:media/hpcpack-cluster-namd/namd_job.png
[job_resources]:media/hpcpack-cluster-namd/job_resources.png
[creds]:media/hpcpack-cluster-namd/creds.png
[task_details]:media/hpcpack-cluster-namd/task_details.png
[vmd_view]:media/hpcpack-cluster-namd/vmd_view.png
