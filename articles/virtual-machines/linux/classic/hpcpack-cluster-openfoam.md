---
title: OpenFOAM HPC paketi ile Linux VM'ler üzerinde çalıştırın. | Microsoft Docs
description: Azure Microsoft HPC Pack kümede dağıtın ve bir RDMA ağ üzerinden birden çok Linux işlem düğümlerinde OpenFOAM işi çalıştırın.
services: virtual-machines-linux
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: c0bb1637-bb19-48f1-adaa-491808d3441f
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 07/22/2016
ms.author: danlep
ms.openlocfilehash: f43790d3495e1c09730e90b5077ec840731a7d83
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="run-openfoam-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Azure’daki bir Linux RDMA kümesinde Microsoft HPC Pack ile OpenFoam çalıştırma
Bu makalede, Azure sanal makinelerinde OpenFoam çalıştırmak için bir yol gösterir. Bir Microsoft HPC Pack kümesinde Linux işlem düğümleri ile Azure ve Çalıştır burada dağıttığınız bir [OpenFoam](http://openfoam.com/) Intel MPI işlemiyle. Böylece işlem düğümlerini Azure RDMA ağ üzerinden iletişim RDMA özellikli Azure Vm'lerde işlem düğümleri için kullanabilirsiniz. Azure'da OpenFoam çalıştırmak için diğer seçenekleri UberCloud'ın gibi Market kullanılabilir tam olarak yapılandırılmış ticari görüntüleri dahil [OpenFoam 2.3 CentOS 6](https://azure.microsoft.com/marketplace/partners/ubercloud/openfoam-v2dot3-centos-v6/)ve çalıştırarak [Azure Batch](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/). 

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

(İçin alan işlemi açın ve düzenleme) OpenFOAM mühendislik ve Bilim hem ticari hem de akademik kuruluşlarda yaygın olarak kullanılan bir açık kaynak hesaplama sıvı dinamiği (CFD) yazılım paketidir. Meshing, özellikle snappyHexMesh, bir parallelized mesher karmaşık CAD geometri ve öncesi ve sonrası işleme için Araçlar içerir. Neredeyse tüm işlemler en bilgisayar donanımı tam anlamıyla yararlanabilmek kullanıcıları etkinleştirme paralel olarak çalışır.  

Microsoft HPC Pack büyük ölçekli HPC ve Microsoft Azure sanal makinelerin kümelerde MPI uygulamaları da dahil olmak üzere paralel uygulamaları çalıştırmak için özellikleri sağlar. HPC Pack de uygulamaların Linux işlem düğümü VM'ler bir HPC Pack kümede dağıtılmış çalışan Linux HPC destekler. Bkz: [Linux işlem düğümlerini Azure bir HPC Pack kümesindeki kullanmaya başlama](hpcpack-cluster.md) işlem düğümleri HPC paketi ile Linux kullanmaya giriş bilgileri için.

> [!NOTE]
> Bu makalede nasıl HPC paketi ile Linux MPI iş yükü çalıştırılacağı gösterilmektedir. Bu, Linux sistem yönetimi ile Linux kümelerinde MPI iş yükleri çalıştıran bazı benzer olduğunu varsayar. MPI ve OpenFOAM bu makalede gösterilen olanları farklı sürümlerini kullanıyorsanız, bazı yükleme ve yapılandırma adımları değiştirmeniz gerekebilir. 
> 
> 

## <a name="prerequisites"></a>Önkoşullar
* **HPC Pack küme RDMA özellikli Linux işlem düğümlerini** - HPC paketi küme boyutu A8, A9, H16r, dağıtmak veya H16rm Linux işlem düğümlerini kullanarak bir [Azure Resource Manager şablonu](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) veya bir [Azure PowerShell Betiği](hpcpack-cluster-powershell-script.md). Bkz: [Linux işlem düğümlerini Azure bir HPC Pack kümesindeki kullanmaya başlama](hpcpack-cluster.md) Önkoşullar ve adımlar her iki seçenek için. PowerShell komut dosyası dağıtım seçeneği seçerseniz, bu makalenin sonunda örnek dosyalarını örnek yapılandırma dosyasına bakın. Bu yapılandırma, bir boyut A8 Windows Server 2012 R2 baş düğüm ve 2 boyutu A8 SUSE Linux Enterprise Server 12 işlem düğümleri oluşan bir HPC Pack Azure tabanlı küme dağıtmak için kullanın. Aboneliği ve hizmet adları için uygun değerleri değiştirin. 
  
  **Ek bilmeniz gerekenler**
  
  * Linux RDMA ağ ön koşulu için Azure bkz [yüksek performanslı işlem VM boyutları](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
  * Powershell komut dosyası dağıtım seçeneği kullanırsanız, RDMA ağ bağlantısı kullanmak için bir bulut hizmeti içindeki tüm Linux işlem düğümlerini dağıtın.
  * Linux düğümleri dağıttıktan sonra ek yönetim görevleri gerçekleştirmek için SSH tarafından bağlayın. SSH bağlantı ayrıntıları her bir Linux VM için Azure portalında bulun.  
* **Intel MPI** - Azure, SLES 12 HPC işlem düğümlerinde OpenFOAM çalıştırmak için Intel MPI kitaplığı 5 çalışma zamanını şuradan yüklemenize gerek [Intel.com site](https://software.intel.com/en-us/intel-mpi-library/). (Intel MPI 5 CentOS tabanlı HPC görüntülerinde önceden yüklenir.)  Bir sonraki adımda gerekiyorsa, Linux işlem düğümlerinde Intel MPI yükleyin. Intel ile kaydettikten sonra bu adım için hazırlamak için ilgili web sayfasına onay e-postadaki bağlantıyı izleyin. Ardından, Intel MPI uygun sürümünü .tgz dosyası için indirme bağlantısı kopyalayın. Bu makalede, Intel MPI sürüm 5.0.3.048 temel alır.
* **OpenFOAM kaynak paketi** -Linux OpenFOAM kaynak paketi yazılımını indirmek [OpenFOAM Foundation site](http://openfoam.org/download/2-3-1-source/). Bu makalede kaynak paketi sürümü 2.3.1, indirme için kullanılabilir OpenFOAM 2.3.1.tgz temel alır. Paketini açın ve Linux işlem düğümlerinde OpenFOAM derlemek için bu makalenin sonraki bölümlerinde'ndaki yönergeleri izleyin.
* **EnSight** (isteğe bağlı) - OpenFOAM benzetimi, indirme ve yükleme sonuçları görmek için [EnSight](https://www.ceisoftware.com/download/) Görselleştirme ve analiz program. Lisans ve yükleme bilgilerini EnSight sitede şunlardır.

## <a name="set-up-mutual-trust-between-compute-nodes"></a>İşlem düğümleri arasında karşılıklı güven ayarlama
Bir geçici düğüm işi birden çok Linux düğümler üzerinde çalışan düğümleri birbirine güvenen gerektirir (tarafından **rsh** veya **ssh**). Microsoft HPC Pack Iaas dağıtım komut dosyası ile HPC Pack kümesi oluşturduğunuzda, betik kalıcı karşılıklı güven belirttiğiniz yönetici hesabı için otomatik olarak ayarlar. Yönetici olmayan kullanıcılar, kümenin etki alanında oluşturmak için bir iş için ayrılmış düğümler arasında geçici karşılıklı güven ayarlamak sahip ve iş tamamlandıktan sonra ilişki yok. Her kullanıcı için güven için bir RSA anahtar çifti için güven ilişkisi HPC Pack kullanan kümesi sağlayın.

### <a name="generate-an-rsa-key-pair"></a>RSA anahtar çifti oluşturma
Linux çalıştırarak bir ortak anahtar ve özel anahtarı içeren bir RSA anahtar çifti oluşturmak kolaydır **ssh-keygen** komutu.

1. Bir Linux bilgisayara oturum açın.
2. Şu komutu çalıştırın:
   
   ```
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
1. Bir Uzak Masaüstü Bağlantısı, baş düğümüne HPC Pack Yönetici hesabınızla (dağıtım betiğini çalıştırdığınızda ayarladığınız yönetici hesabı) oluşturun.
2. Kümenin Active Directory etki alanında bir etki alanı kullanıcı hesabı oluşturmak için standart Windows Server yordamları kullanın. Örneğin, Active Directory Kullanıcıları ve Bilgisayarları aracını baş düğümünde kullanın. Bu makaledeki örneklerde, hpclab\hpcuser adlı bir etki alanı kullanıcısı oluşturun varsayılmaktadır.
3. C:\cred.xml adlı bir dosya oluşturun ve RSA anahtar veri dosyasını buraya kopyalayın. Bir örnek cred.xml bu makalenin sonundaki dosyasıdır.
   
   ```
   <ExtendedData>
     <PrivateKey>Copy the contents of private key here</PrivateKey>
     <PublicKey>Copy the contents of public key here</PublicKey>
   </ExtendedData>
   ```
4. Bir komut istemi açın ve hpclab\hpcuser hesabı için kimlik bilgilerini veri kümesi için aşağıdaki komutu girin. Kullandığınız **extendeddata** dosyasının adı oluşturduğunuz C:\cred.xml anahtar verilerini geçirmek için parametre.
   
   ```
   hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
   ```
   
   Bu komut çıktısı başarıyla tamamlanır. İşlerini çalıştırmak için gereken kullanıcı hesapları için kimlik bilgilerini ayarlama sonra cred.xml dosyayı güvenli bir konumda depolayın veya silin.
5. RSA anahtar çifti Linux düğümlerinden biri üzerinde oluşturursa, bunları kullanmayı bitirdikten sonra anahtarlarını silin unutmayın. HPC Pack varolan id_rsa dosyaya veya id_rsa.pub dosyası bulursa, karşılıklı güven ayarlı değil.

> [!IMPORTANT]
> Bir yönetici tarafından gönderilen bir işin Linux düğümleri kök hesapta çalıştığından Linux iş paylaşılan bir kümede bir Küme Yöneticisi olarak çalışan öneririz yok. Ancak, yönetici olmayan bir kullanıcı tarafından gönderilen bir işi işi kullanıcı aynı ada sahip bir yerel Linux kullanıcı hesabı altında çalışır. Bu durumda, HPC Pack karşılıklı güven bu Linux kullanıcı için iş için ayrılan düğümler arasında ayarlar. Linux kullanıcı el ile Linux düğümleri üzerinde iş çalıştırmadan önce ayarlayabilirsiniz veya iş gönderildiğinde HPC Pack kullanıcı otomatik olarak oluşturur. HPC Pack kullanıcı oluşturursa, HPC Pack işi tamamlandıktan sonra onu siler. Güvenlik tehditleri azaltmak için HPC Pack işi tamamlandıktan sonra anahtarlarını kaldırır.
> 
> 

## <a name="set-up-a-file-share-for-linux-nodes"></a>Linux düğümleri için bir dosya paylaşımı ayarlama
Baş düğüm üzerinde bir klasör standart bir SMB paylaşımında şimdi ayarlayın. Ortak yoluna sahip uygulama dosyalara erişmek Linux düğümleri izin vermek için Linux düğümlerinde paylaşılan klasöre bağlayın. İsterseniz, başka bir dosya paylaşımı gibi birçok senaryo - ya da bir NFS paylaşımına için önerilen bir Azure dosya paylaşımı - seçeneği kullanabilirsiniz. Dosya bilgileri ve ayrıntılı adımlar paylaşımı bkz [Linux işlem düğümlerini Azure bir HPC Pack kümesindeki kullanmaya başlama](hpcpack-cluster.md).

1. Baş düğüm üzerinde bir klasör oluşturun ve herkese okuma/yazma ayrıcalıklarına ayarlayarak paylaşın. Örneğin, baş düğüm C:\OpenFOAM paylaşmak \\ \\SUSE12RDMA HN\OpenFOAM. Burada, *SUSE12RDMA HN* baş düğüm ana bilgisayar adıdır.
2. Bir Windows PowerShell penceresi açın ve aşağıdaki komutları çalıştırın:
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
   clusrun /nodegroup:LinuxNodes mount -t cifs //SUSE12RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
   ```

İlk komut LinuxNodes grubundaki tüm düğümlerde /openfoam adlı bir klasör oluşturur. İkinci komut dir_mode ile Linux düğümlerdeki paylaşılan klasör //SUSE12RDMA-HN/OpenFOAM bağlar ve file_mode BITS 777 ayarlayın. *Kullanıcıadı* ve *parola* komutta baş düğüm bir kullanıcının kimlik bilgileri olmalıdır.

> [!NOTE]
> "\`" İkinci komut dosyasındaki simge olan bir PowerShell kaçış simgesi. "\`," "," (virgül karakteri) anlamına gelir komutu, bir parçasıdır.
> 
> 

## <a name="install-mpi-and-openfoam"></a>MPI ve OpenFOAM yükleyin
OpenFOAM RDMA ağ üzerinde bir MPI iş olarak çalıştırmak için Intel MPI kitaplıklarıyla OpenFOAM derlemek gerekir. 

Öncelikle birkaç çalıştırın **clusrun** Intel MPI kitaplıkları (henüz yüklenmemişse) yüklemek için komutları ve Linux düğümleri üzerinde OpenFOAM. Yükleme dosyaları Linux düğümleri arasında paylaşmak için önceden yapılandırılmış baş düğüm paylaşımı kullanın.

> [!IMPORTANT]
> Bu yükleme ve derleniyor adımları verilebilir. Bazı bağımlı derleyicileri ve kitaplıklarını doğru şekilde yüklendiğinden emin olmak için Linux sistem yönetim bilgisi gerekir. Belirli ortam değişkenleri veya Intel MPI ve OpenFOAM sürümü için diğer ayarları değiştirmeniz gerekebilir. Ayrıntılar için bkz [Linux Yükleme Kılavuzu için Intel MPI Kitaplığı](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) ve [OpenFOAM kaynak paketi yüklemesi](http://openfoam.org/download/2-3-1-source/) ortamınız için.
> 
> 

### <a name="install-intel-mpi"></a>Intel MPI yükleyin
Linux düğümleri bu dosyayı /openfoam erişebilmesi için indirilen yükleme paketi Intel MPI (Bu örnekte l_mpi_p_5.0.3.048.tgz) için C:\OpenFoam baş düğümünde kaydedin. Ardından çalıştırın **clusrun** Intel MPI kitaplığı tüm Linux düğümlerine yüklemek için.

1. Aşağıdaki komutları yükleme paketini kopyalayın ve her düğümde /opt/intel ayıklayın.
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /opt/intel
   
   clusrun /nodegroup:LinuxNodes cp /openfoam/l_mpi_p_5.0.3.048.tgz /opt/intel/
   
   clusrun /nodegroup:LinuxNodes tar -xzf /opt/intel/l_mpi_p_5.0.3.048.tgz -C /opt/intel/
   ```
2. Intel MPI kitaplığı sessizce yüklemek için silent.cfg dosyasını kullanın. Bu makalenin sonunda örnek dosyalarını bir örnek bulabilirsiniz. Bu dosyayı paylaşılan klasör /openfoam yerleştirin. Silent.cfg dosyası hakkında daha fazla ayrıntı için bkz: [Linux Yükleme Kılavuzu - sessiz yükleme için Intel MPI Kitaplığı](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).
   
   > [!TIP]
   > Silent.cfg dosyanız Linux sahip bir metin dosyası olarak kaydettiğinizde satır sonlarını (yalnızca LF, CR LF) emin olun. Bu adım, düzgün şekilde Linux düğümlerinde çalıştığını sağlar.
   > 
   > 
3. Intel MPI kitaplığı sessiz modda yükleyin.
   
   ```
   clusrun /nodegroup:LinuxNodes bash /opt/intel/l_mpi_p_5.0.3.048/install.sh --silent /openfoam/silent.cfg
   ```

### <a name="configure-mpi"></a>MPI yapılandırın
Test etmek için aşağıdaki satırları /etc/security/limits.conf her Linux düğümleri için eklemeniz gerekir:

    clusrun /nodegroup:LinuxNodes echo "*               hard    memlock         unlimited" `>`> /etc/security/limits.conf
    clusrun /nodegroup:LinuxNodes echo "*               soft    memlock         unlimited" `>`> /etc/security/limits.conf


Limits.conf dosya güncelleştirdikten sonra Linux düğümleri yeniden başlatın. Örneğin, aşağıdaki kullanın **clusrun** komutu:

```
clusrun /nodegroup:LinuxNodes systemctl reboot
```

Yeniden başlattıktan sonra paylaşılan klasör /openfoam bağlandığından emin olun.

### <a name="compile-and-install-openfoam"></a>Derleme ve OpenFOAM yükleyin
Linux düğümleri bu dosyayı /openfoam erişebilmesi için indirilen yükleme paketi C:\OpenFoam OpenFOAM kaynak paketine (Bu örnekte, OpenFOAM-2.3.1.tgz) için baş düğümünde kaydedin. Ardından çalıştırın **clusrun** OpenFOAM tüm Linux düğümlerine derlemek için komutları.

1. Her bir Linux düğümde bir klasör /opt/OpenFOAM oluşturma kaynak paketi bu klasöre kopyalayın ve var. ayıklayın.
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /opt/OpenFOAM
   
   clusrun /nodegroup:LinuxNodes cp /openfoam/OpenFOAM-2.3.1.tgz /opt/OpenFOAM/
   
   clusrun /nodegroup:LinuxNodes tar -xzf /opt/OpenFOAM/OpenFOAM-2.3.1.tgz -C /opt/OpenFOAM/
   ```
2. Intel MPI kitaplığıyla OpenFOAM derlemek için önce bazı ortam değişkenleri Intel MPI hem OpenFOAM ayarlayın. Settings.sh adlı bir bash komut dosyası değişkenleri ayarlamak için kullanın. Bu makalenin sonunda örnek dosyalarını bir örnek bulabilirsiniz. (Linux satır sonları ile kaydedilen) Bu dosya paylaşılan klasör /openfoam yerleştirin. Bu dosya ayrıca daha sonra bir OpenFOAM işi çalıştırmak için kullandığınız MPI ve OpenFOAM çalışma zamanları ayarlarını içerir.
3. Bağımlı paketler OpenFOAM derlemek için gereken yükleyin. Linux dağıtımınız bağlı olarak, önce bir havuz eklemeniz gerekebilir. Çalıştırma **clusrun** komutları aşağıdakine benzer:
   
    ```
    clusrun /nodegroup:LinuxNodes zypper ar http://download.opensuse.org/distribution/13.2/repo/oss/suse/ opensuse
   
    clusrun /nodegroup:LinuxNodes zypper -n --gpg-auto-import-keys install --repo opensuse --force-resolution -t pattern devel_C_C++
    ```
   
    Gerekirse, bunların düzgün çalıştığını doğrulamak için komutları çalıştırmak için SSH her Linux düğümü.
4. OpenFOAM derlemek için aşağıdaki komutu çalıştırın. Derleme işlemi tamamlanması biraz zaman alır ve büyük miktarda standart çıktı için günlük bilgilerini oluşturur, böylece kullanma **/ araya eklemeli** araya eklemeli çıktı görüntülemek için seçeneği.
   
   ```
   clusrun /nodegroup:LinuxNodes /interleaved source /openfoam/settings.sh `&`& /opt/OpenFOAM/OpenFOAM-2.3.1/Allwmake
   ```
   
   > [!NOTE]
   > "\`" Komutta simgenin olup PowerShell bir kaçış simgesi. "\`&" anlamına gelir "&" komutu, bir parçasıdır.
   > 
   > 

## <a name="prepare-to-run-an-openfoam-job"></a>Bir OpenFOAM işi çalıştırmak hazırlama
Şimdi, iki Linux düğümlerinde OpenFoam örnekleri olan sloshingTank3D adlı bir MPI işi çalıştırmak hazırlanın. 

### <a name="set-up-the-runtime-environment"></a>Çalışma zamanı ortamını ayarlama
Çalışma zamanı ortamı MPI ve OpenFOAM için Linux düğümleri üzerinde ayarlamak için baş düğüm üzerinde bir Windows PowerShell penceresinde aşağıdaki komutu çalıştırın. (Bu komut, yalnızca SUSE Linux için geçerlidir.)

```
clusrun /nodegroup:LinuxNodes cp /openfoam/settings.sh /etc/profile.d/
```

### <a name="prepare-sample-data"></a>Örnek verileri hazırlama
Daha önce (/openfoam takılı) Linux düğümleri arasında dosya paylaşmak üzere yapılandırılmış baş düğüm paylaşımı kullanın.

1. SSH bir, Linux işlem düğümlerini.
2. Bunu zaten yapmadıysanız OpenFOAM çalışma zamanı ortamı, ayarlamak için aşağıdaki komutu çalıştırın.
   
   ```
   $ source /openfoam/settings.sh
   ```
3. SloshingTank3D örneği paylaşılan klasöre kopyalayın ve kendisine gidin.
   
   ```
   $ cp -r $FOAM_TUTORIALS/multiphase/interDyMFoam/ras/sloshingTank3D /openfoam/
   
   $ cd /openfoam/sloshingTank3D
   ```
4. Bu örnek varsayılan parametrelerini kullandığınızda, daha hızlı çalışmasını sağlamak için bazı parametreler değiştirmek isteyebilirsiniz onlarca çalıştırmak için dakika sürebilir. Bir basit zaman adım değişkenleri deltaT ve sistem/controlDict dosyasında writeInterval değiştirmek için bir seçimdir. Bu dosyayı saat ve okuma ve çözüm veri yazma denetim ile ilgili tüm giriş verilerini depolar. Örneğin, 0,05 gelen deltaT 0,5 değeri ve 0,05 gelen writeInterval 0,5 değeri değiştirebilir.
   
   ![Adım değişkenleri değiştirin][step_variables]
5. Sistem/decomposeParDict dosyasında değişkenleri için istenen değerleri belirtin. Bu örnek iki Linux düğümleri her 8 çekirdeğiyle kullanır, böylece numberOfSubdomains 16 hierarchicalCoeffs için n ayarlayın (1 1 16), 16 süreçleri ile paralel OpenFOAM anlamına çalıştırın. Daha fazla bilgi için bkz: [OpenFOAM Kullanıcı Kılavuzu: paralel 3.4 çalışan uygulamaları](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).
   
   ![İşlemler parçalayın][decompose]
6. Örnek verileri hazırlamak için sloshingTank3D dizininden aşağıdaki komutları çalıştırın.
   
   ```
   $ . $WM_PROJECT_DIR/bin/tools/RunFunctions
   
   $ m4 constant/polyMesh/blockMeshDict.m4 > constant/polyMesh/blockMeshDict
   
   $ runApplication blockMesh
   
   $ cp 0/alpha.water.org 0/alpha.water
   
   $ runApplication setFields  
   ```
7. Baş düğümünde örnek veri dosyalarını C:\OpenFoam\sloshingTank3D kopyalanır görmeniz gerekir. (C:\OpenFoam paylaşılan baş düğüm klasörüdür.)
   
   ![Baş düğüm veri dosyaları][data_files]

### <a name="host-file-for-mpirun"></a>Ana bilgisayar dosyası mpirun için
Bu adımda, bir ana bilgisayar dosyası (işlem düğümleri listesi) oluşturmak, **mpirun** komutunu kullanır.

1. Linux düğümleri her birinde bu dosyayı /openfoam/hostfile tüm Linux düğümlerde üzerinde erişilebilir şekilde /openfoam altında hostfile adlı bir dosya oluşturun.
2. Linux düğüm adları bu dosyaya yazma. Bu örnekte, dosya aşağıdaki adları içerir:
   
   ```       
   SUSE12RDMA-LN1
   SUSE12RDMA-LN2
   ```
   
   > [!TIP]
   > Bu dosya C:\OpenFoam\hostfile baş düğümünde de oluşturabilirsiniz. Bu seçeneği seçerseniz, Linux bir metin dosyası olarak kaydedin (yalnızca LF, CR LF) satır sonlarını. Bu, düzgün şekilde Linux düğümlerinde çalıştığını sağlar.
   > 
   > 
   
   **Bash betik sarmalayıcı**
   
   Hangi düğümlerin işinize ayrılır tanımadığınız için birçok Linux düğümleri varsa ve işinizin yalnızca bunlardan bazıları üzerinde çalıştırmak istediğiniz, onu bir sabit ana bilgisayar dosyası kullanmak için iyi bir fikir değil. Bu durumda, bir bash betik sarmalayıcı için yazma **mpirun** ana bilgisayar dosyası otomatik olarak oluşturmak için. Bu makalenin sonunda hpcimpirun.sh olarak adlandırılan bir örnek bash betik sarmalayıcı bulmak ve /openfoam/hpcimpirun.sh kaydedin. Bu örnek komut dosyası şunları yapar:
   
   1. İçin ortam değişkenleri ayarlar **mpirun**ve RDMA ağ üzerinden MPI işi çalıştırmak için bazı ek komut parametreleri. Bu durumda, aşağıdaki değişkenleri ayarlar:
      
      * I_MPI_FABRICS=shm:dapl
      * I_MPI_DAPL_PROVIDER=ofa-v2-ib0
      * I_MPI_DYNAMIC_CONNECTION=0
   2. Ortam göre bir ana bilgisayar dosyası oluşturur değişken $ iş etkinleştirildiğinde, HPC baş düğümü tarafından ayarlanan CCP_NODES_CORES.
      
      $CCP_NODES_CORES biçimlerinin bu deseni izler:
      
      ```
      <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
      ```
      
      Burada
      
      * `<Number of nodes>` -Bu iş için ayrılmış düğüm sayısı.  
      * `<Name of node_n_...>` -Bu iş için ayrılmış her düğümün adı.
      * `<Cores of node_n_...>` -Bu iş için ayrılmış düğümünde çekirdek sayısı.
      
      Örneğin, işi çalıştırmak için iki düğüm gerekirse, $CCP_NODES_CORES benzer.
      
      ```
      2 SUSE12RDMA-LN1 8 SUSE12RDMA-LN2 8
      ```
   3. Çağrıları **mpirun** komut ve iki parametre için komut satırını ekler.
      
      * `--hostfile <hostfilepath>: <hostfilepath>` -komut dosyası oluşturur ana bilgisayar dosyasının yolu
      * `-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}` -Bu iş için ayrılan toplam çekirdek sayısı depolar HPC paketi üstbilgi düğümü tarafından ayarlanan bir ortam değişkeni. Bu durumda, işlemler için sayısını belirtir **mpirun**.

## <a name="submit-an-openfoam-job"></a>Bir OpenFOAM işi gönderin
Şimdi bir işi HPC Küme Yöneticisi'nde gönderebilirsiniz. Komut satırlarında bazı iş görevleri için komut dosyası hpcimpirun.sh geçmesi gerekir.

1. Küme baş düğümüne bağlanmak ve HPC Küme Yöneticisi'ni başlatın.
2. **Kaynak Yönetimi'nde**, Linux işlem düğümü olduğundan emin olun **çevrimiçi** durumu. Değilse, bunları seçin ve tıklatın **çevrimiçine**.
3. İçinde **iş yönetimi**, tıklatın **yeni iş**.
4. İş için bir ad girin *sloshingTank3D*.
   
   ![İş ayrıntıları][job_details]
5. İçinde **iş kaynakları**, "Düğümü" olarak kaynak türünü seçin ve en az 2'ye ayarlayın. Bu yapılandırma, bu örnekte, her biri sekiz çekirdeği olması iki Linux düğümlerde işi çalıştırır.
   
   ![İş kaynakları][job_resources]
6. Tıklatın **Düzenle görevleri** sol gezinti ve ardından **Ekle** iş için bir görev eklemek için. Aşağıdaki komut satırları ve ayarlar işlemiyle dört görevleri ekleyin.
   
   > [!NOTE]
   > Çalışan `source /openfoam/settings.sh` her aşağıdaki görevlerden önce OpenFOAM komutunu çağırır OpenFOAM ve MPI çalışma zamanı ortamları ayarlar.
   > 
   > 
   
   * **Görev 1**. Çalıştırma **decomposePar** çalıştırmak için veri dosyaları oluşturmak için **interDyMFoam** paralel.
     
     * Bir düğüm göreve atayın
     * **Komut satırı** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`
     * **Çalışma dizini** -/ openfoam/sloshingTank3D
     
     Aşağıdaki şekle bakın. Kalan görevlere benzer şekilde yapılandırın.
     
     ![Görev 1 ayrıntıları][task_details1]
   * **Görev 2**. Çalıştırma **interDyMFoam** paralel işlem örnek.
     
     * İki düğüm göreve atayın
     * **Komut satırı** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`
     * **Çalışma dizini** -/ openfoam/sloshingTank3D
   * **Görev 3**. Çalıştırma **reconstructPar** tek bir kümesine her processor_N_ dizini zaman dizinlerden kümeleri birleştirmek için.
     
     * Bir düğüm göreve atayın
     * **Komut satırı** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`
     * **Çalışma dizini** -/ openfoam/sloshingTank3D
   * **Görev 4**. Çalıştırma **foamToEnsight** OpenFOAM sonuç dönüştürmek için paralel EnSight dosyalarıyla biçimlendirmek ve büyük/küçük harfe dizinde Ensight adlı bir dizin EnSight dosyaları yerleştirir.
     
     * İki düğüm göreve atayın
     * **Komut satırı** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`
     * **Çalışma dizini** -/ openfoam/sloshingTank3D
7. Bu görevleri görev artan bağımlılıkları ekleyin.
   
   ![Görev bağımlılıkları][task_dependencies]
8. Tıklatın **gönderme** bu işi çalıştırmak için.
   
   Varsayılan olarak, geçerli oturum açmış kullanıcı hesabınız olarak işi HPC paketi gönderir. Tıklattıktan sonra **gönderme**, kullanıcı adını ve parolasını girmenizi isteyen bir iletişim kutusu görebilirsiniz.
   
   ![İş kimlik bilgileri][creds]
   
   Bazı koşullarda HPC Pack önce giriş ve bu iletişim kutusunu göstermez kullanıcı bilgilerini hatırlıyor. HPC Pack yeniden Göster yapmak için bir komut isteminde aşağıdaki komutu girin ve sonra işi göndermek.
   
   ```
   hpccred delcreds
   ```
9. İş dakika onlarca örnek için ayarladığınız parametrelere göre birkaç saat sürer. Isı Haritası Linux düğümleri üzerinde iş bakın. 
   
   ![Isı haritası][heat_map]
   
   Her düğümde sekiz işlemleri başlatıldı.
   
   ![Linux işlemleri][linux_processes]
10. İş tamamlandığında iş sonuçlarını C:\OpenFoam\sloshingTank3D ve günlük dosyalarını C:\OpenFoam altındaki klasörleri bulun.

## <a name="view-results-in-ensight"></a>EnSight görünümü sonuçları
İsteğe bağlı olarak kullanmak [EnSight](https://www.ceisoftware.com/) görselleştirmek ve OpenFOAM iş sonuçlarını analiz etmek için. Bu Görselleştirme ve EnSight animasyonda hakkında daha fazla bilgi için bkz [video Kılavuzu](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html).

1. Baş düğümünde EnSight yükledikten sonra başlatın.
2. Open C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.
   
   Tank Görüntüleyicisi'ndeki bakın.
   
   ![EnSight tank][tank]
3. Oluşturma bir **Isosurface** gelen **internalMesh**ve ardından değişkeni **alpha_water**.
   
   ![Bir isosurface oluşturma][isosurface]
4. Rengini ayarlama **Isosurface_part** önceki adımda oluşturduğunuz. Örneğin, su için mavi ayarlayın.
   
   ![İsosurface renk Düzenle][isosurface_color]
5. Oluşturma bir **ISO hacimli** gelen **duvarları** seçerek **duvarları** içinde **bölümleri** panel ve'ı tıklatın **Isosurfaces** araç çubuğu düğmesini.
6. İletişim kutusunda **türü** olarak **Isovolume** ve en küçük **Isovolume aralığı** 0,5. İsovolume oluşturmak için tıklatın **Create seçilen parçaları ile**.
7. Rengini ayarlama **Iso_volume_part** önceki adımda oluşturduğunuz. Örneğin, derin su mavi ayarlayın.
8. Rengini ayarlama **duvarları**. Örneğin, saydam beyaza ayarlayın.
9. Şimdi **Yürüt** benzetimi sonuçlarını görmek için.
   
    ![Tank sonucu][tank_result]

## <a name="sample-files"></a>Örnek dosyaları
### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>PowerShell komut dosyası tarafından küme dağıtımı için örnek XML yapılandırma dosyası
 ```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>suse12rdmavnet</VNetName>
    <SubnetName>SUSE12RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>SUSE12RDMA-HN</VMName>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>SUSE12RDMA-LN%1%</VMNamePattern>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <NodeCount>2</NodeCount>
      <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
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
### <a name="sample-silentcfg-file-to-install-mpi"></a>Örnek silent.cfg dosyası MPI yüklemek için
```
# Patterns used to check silent configuration file
#
# anythingpat - any string
# filepat     - the file location pattern (/file/location/to/license.lic)
# lspat       - the license server address pattern (0123@hostname)
# snpat       - the serial number pattern (ABCD-01234567)

# accept EULA, valid values are: {accept, decline}
ACCEPT_EULA=accept

# optional error behavior, valid values are: {yes, no}
CONTINUE_WITH_OPTIONAL_ERROR=yes

# install location, valid values are: {/opt/intel, filepat}
PSET_INSTALL_DIR=/opt/intel

# continue with overwrite of existing installation directory, valid values are: {yes, no}
CONTINUE_WITH_INSTALLDIR_OVERWRITE=yes

# list of components to install, valid values are: {ALL, DEFAULTS, anythingpat}
COMPONENTS=DEFAULTS

# installation mode, valid values are: {install, modify, repair, uninstall}
PSET_MODE=install

# directory for non-RPM database, valid values are: {filepat}
#NONRPM_DB_DIR=filepat

# Serial number, valid values are: {snpat}
#ACTIVATION_SERIAL_NUMBER=snpat

# License file or license server, valid values are: {lspat, filepat}
#ACTIVATION_LICENSE_FILE=

# Activation type, valid values are: {exist_lic, license_server, license_file, trial_lic, serial_number}
ACTIVATION_TYPE=trial_lic

# Path to the cluster description file, valid values are: {filepat}
#CLUSTER_INSTALL_MACHINES_FILE=filepat

# Intel(R) Software Improvement Program opt-in, valid values are: {yes, no}
PHONEHOME_SEND_USAGE_DATA=no

# Perform validation of digital signatures of RPM files, valid values are: {yes, no}
SIGNING_ENABLED=yes

# Select yes to enable mpi-selector integration, valid values are: {yes, no}
ENVIRONMENT_REG_MPI_ENV=no

# Select yes to update ld.so.conf, valid values are: {yes, no}
ENVIRONMENT_LD_SO_CONF=no


```

### <a name="sample-settingssh-script"></a>Örnek settings.sh komut dosyası
```
#!/bin/bash

# impi
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# openfoam
export FOAM_INST_DIR=/opt/OpenFOAM
source /opt/OpenFOAM/OpenFOAM-2.3.1/etc/bashrc
export WM_MPLIB=INTELMPI
```


### <a name="sample-hpcimpirunsh-script"></a>Örnek hpcimpirun.sh komut dosyası
```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"

# Set mpirun runtime evironment
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# mpirun command
MPIRUN=mpirun
# Argument of "--hostfile"
NODELIST_OPT="--hostfile"
# Argument of "-np"
NUMPROCESS_OPT="-np"

# Get node information from ENVs
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # CCP_NODES_CORES is not found or is empty, just run the mpirun without hostfile arg.
    ${MPIRUN} $*
else
    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$

    # Get every node name and write into the hostfile file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the mpirun with hostfile arg
    ${MPIRUN} ${NUMPROCESS_OPT} ${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}

```





<!--Image references-->
[keygen]:media/hpcpack-cluster-openfoam/keygen.png
[keys]:media/hpcpack-cluster-openfoam/keys.png
[step_variables]:media/hpcpack-cluster-openfoam/step_variables.png
[data_files]:media/hpcpack-cluster-openfoam/data_files.png
[decompose]:media/hpcpack-cluster-openfoam/decompose.png
[job_details]:media/hpcpack-cluster-openfoam/job_details.png
[job_resources]:media/hpcpack-cluster-openfoam/job_resources.png
[task_details1]:media/hpcpack-cluster-openfoam/task_details1.png
[task_dependencies]:media/hpcpack-cluster-openfoam/task_dependencies.png
[creds]:media/hpcpack-cluster-openfoam/creds.png
[heat_map]:media/hpcpack-cluster-openfoam/heat_map.png
[tank]:media/hpcpack-cluster-openfoam/tank.png
[tank_result]:media/hpcpack-cluster-openfoam/tank_result.png
[isosurface]:media/hpcpack-cluster-openfoam/isosurface.png
[isosurface_color]:media/hpcpack-cluster-openfoam/isosurface_color.png
[linux_processes]:media/hpcpack-cluster-openfoam/linux_processes.png
