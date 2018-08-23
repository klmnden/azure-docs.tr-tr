---
title: Linux Vm'lerinde HPC Pack ile OpenFOAM çalıştırma | Microsoft Docs
description: Azure'da bir Microsoft HPC Pack kümesini dağıtmayı ve OpenFOAM işi, bir RDMA ağ üzerinden birden çok Linux işlem düğümleri üzerinde çalışmak.
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
ms.openlocfilehash: 9032a0b68c4c8789010b0304b64a63d4924521fb
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42056013"
---
# <a name="run-openfoam-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Azure’daki bir Linux RDMA kümesinde Microsoft HPC Pack ile OpenFoam çalıştırma
Bu makalede, Azure sanal makineler'de OpenFoam çalıştırma yollarından biri gösterilmektedir. Bir Microsoft HPC Pack kümesinde Linux işlem düğümleri Azure ve çalıştırın, burada dağıttığınız bir [OpenFoam](http://openfoam.com/) Intel MPI işle. İşlem düğümlerinin Azure RDMA ağ üzerinden iletişim kurmak için işlem düğümleri için RDMA özellikli bir Azure sanal makinelerini kullanabilirsiniz. Azure'da OpenFoam çalıştırma için diğer seçenekler UberCloud'ın gibi Market kullanılabilir tam olarak yapılandırılmış ticari görüntüleri dahil [OpenFoam 2.3 CentOS 6](https://azuremarketplace.microsoft.com/marketplace/apps/cfd-direct.cfd-direct-from-the-cloud)ve çalıştırarak [Azure Batch](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/). 

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

OpenFOAM (için alanı işlemi açın ve düzenleme) mühendislik ve Bilim, hem ticari hem de akademik kuruluşlardaki yaygın olarak kullanılan açık kaynaklı bilgi işlem akış dinamikleri (CFD) yazılım paketidir. Bu özellikle snappyHexMesh, karmaşık CAD geometriler ve öncesi ve sonrası işleme için paralel bir mesher meshing yönelik araçlar içerir. Kullanıcıların kendi elden çıkarma sırasında bilgisayar donanımı tam anlamıyla paralel olarak neredeyse tüm işlemler çalıştırılır.  

Microsoft HPC Pack, büyük ölçekli HPC ve Microsoft Azure sanal makinelerini kümelerinde MPI uygulamaları da dahil, paralel uygulamaları çalıştırmak için özellikler sağlar. HPC Pack ayrıca çalışan Linux HPC işlem düğümü Vm'lerinde dağıtılan bir HPC Pack kümesinde Linux uygulamaları destekler. Bkz: [azure'da HPC Pack kümesinde Linux işlem düğümleri kullanmaya başlama](hpcpack-cluster.md) işlem düğümleriyle HPC Pack ile Linux kullanarak giriş.

> [!NOTE]
> Bu makalede, HPC paketi ile bir Linux MPI iş yükü çalıştırma gösterilmektedir. Bu, bazı benzerlik çalışan Linux kümelerinde MPI iş yükleri ile Linux sistem yönetim ile sahip olduğunuz varsayılır. MPI ve OpenFOAM bu makalede gösterilen gördüğünüzden farklı sürümleri kullanıyorsanız, bazı yükleme ve yapılandırma adımları değiştirmeniz gerekebilir. 
> 
> 

## <a name="prerequisites"></a>Önkoşullar
* **HPC Pack kümesine RDMA özellikli Linux işlem düğümlerini** - boyutu A8, A9, H16r, ile bir HPC Pack kümesini dağıtmayı veya H16rm Linux işlem düğümlerini kullanarak bir [Azure Resource Manager şablonu](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) veya [Azure PowerShell Betiği](hpcpack-cluster-powershell-script.md). Bkz: [azure'da HPC Pack kümesinde Linux işlem düğümleri kullanmaya başlama](hpcpack-cluster.md) önkoşulları ve her iki seçenek için. PowerShell komut dosyası dağıtım seçeneği tercih ederseniz bu makalenin sonunda örnek dosyalarını örnek yapılandırma dosyasına bakın. Bu yapılandırma, bir boyut A8 Windows Server 2012 R2 baş düğüm ve 2 boyutu A8 SUSE Linux Enterprise Server 12 işlem düğümleri oluşan Azure tabanlı bir HPC Pack kümesine dağıtmak için kullanın. Abonelik ve hizmet adları için uygun değerleri değiştirin. 
  
  **Bilmeniz gereken ek noktalar**
  
  * Linux RDMA ağ ön koşulu için azure'da bkz [yüksek performanslı işlem VM boyutları](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
  * Powershell komut dosyası dağıtım seçeneğini kullanırsanız, RDMA ağ bağlantısı kullanmak için bir bulut hizmetindeki tüm Linux işlem düğümleri dağıtın.
  * Linux düğümleri dağıttıktan sonra ek yönetim görevleri gerçekleştirmek için SSH ile bağlanın. SSH bağlantı ayrıntılarını, her bir Linux VM için Azure portalında bulun.  
* **Intel MPI** - azure'da SLES 12 HPC işlem düğümleri üzerinde OpenFOAM çalıştırma için Intel MPI kitaplığı 5 çalışma zamanını şuradan yüklemeniz gerekir [Intel.com site](https://software.intel.com/en-us/intel-mpi-library/). (Intel MPI 5 CentOS tabanlı HPC görüntülerinde önceden yüklenir.)  Daha sonraki bir adımda gerekirse, Intel MPI kullanarak Linux işlem düğümlerinde yükleyin. Bu adım için Intel ile kaydettikten sonra hazırlamak için ilgili web sayfasında onayı e-postadaki bağlantıya izleyin. Ardından, Intel MPI'ün uygun sürümüne .tgz dosyasının indirme bağlantısı kopyalayın. Bu makalede, Intel MPI sürüm 5.0.3.048 üzerinde temel alır.
* **OpenFOAM kaynak paketi** -Linux'taki OpenFOAM kaynak paketi yazılım indir [OpenFOAM Foundation site](http://openfoam.org/download/2-3-1-source/). Bu makalede OpenFOAM 2.3.1.tgz kaynak paketi sürümü indirilebilir 2.3.1 dayanır. Paketten ve Linux işlem düğümlerinde OpenFOAM derlemek için bu makalenin sonraki bölümlerinde'ndaki yönergeleri izleyin.
* **EnSight** (isteğe bağlı) - OpenFOAM benzetimi, indirme ve yükleme sonuçları görmek için [EnSight](https://ensighttransfe.wpengine.com/direct-access-downloads/) Görselleştirme ve analiz programı. Lisans ve yükleme bilgileri EnSight sitesinde var.

## <a name="set-up-mutual-trust-between-compute-nodes"></a>İşlem düğümleri arasında karşılıklı güven ayarlama
Çapraz düğüm iş birden çok Linux düğümlerinde çalışan düğümlerinin birbirine güvenen gerektirir (tarafından **rsh** veya **ssh**). HPC Pack kümesinde Microsoft HPC Pack Iaas dağıtım betiği içeren oluşturduğunuzda, kodun kalıcı karşılıklı güven belirttiğiniz yönetici hesabı için otomatik olarak ayarlar. Yönetici olmayan kullanıcılar, kümenin etki alanında oluşturmak için bir iş için ayrılmış düğümler arasında geçici karşılıklı güven ayarlamanız gerekir ve iş tamamlandıktan sonra ilişkisi yok. Her kullanıcı için güven oluşturmak için bir güven ilişkisi için HPC Pack kullanan kümeye RSA anahtar çifti belirtin.

### <a name="generate-an-rsa-key-pair"></a>RSA anahtar çifti oluşturma
Linux çalıştırarak bir ortak anahtar ve özel anahtar içeren bir RSA anahtar çifti oluşturmak kolaydır **ssh-keygen** komutu.

1. Bir Linux bilgisayarda oturum açın.
2. Şu komutu çalıştırın:
   
   ```
   ssh-keygen -t rsa
   ```
   
   > [!NOTE]
   > Tuşuna **Enter** komut tamamlanıncaya kadar varsayılan ayarları kullanmak için. Burada bir parola girmeyin; bir parola istendiğinde tuşuna basarak **Enter**.
   > 
   > 
   
   ![RSA anahtar çifti oluşturma][keygen]
3. Dizin ~/.ssh dizine geçin. Özel anahtarı id_rsa ve ortak anahtar id_rsa.pub depolanır.
   
   ![Özel ve genel anahtarlar][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>HPC Pack kümesine anahtar çifti Ekle
1. HPC Pack Yönetici hesabınızla (dağıtım betiği çalıştırdığınızda ayarladığınız yönetici hesabı), baş düğüm için bir Uzak Masaüstü bağlantısı oluşturun.
2. Kümenin Active Directory etki alanında etki alanı kullanıcı hesabı oluşturmak için standart Windows Server yordamları kullanın. Örneğin, baş düğüm üzerinde Active Directory Kullanıcıları ve Bilgisayarları aracını kullanın. Bu makaledeki örneklerde hpclab\hpcuser adlı etki alanı kullanıcısı oluşturduğunuz varsayılır.
3. C:\cred.xml adlı bir dosya oluşturun ve RSA anahtar verilerini buraya kopyalayın. Bir örnek cred.xml bu makalenin sonunda dosyasıdır.
   
   ```
   <ExtendedData>
     <PrivateKey>Copy the contents of private key here</PrivateKey>
     <PublicKey>Copy the contents of public key here</PublicKey>
   </ExtendedData>
   ```
4. Bir komut istemi açın ve hpclab\hpcuser hesabı için kimlik bilgilerini veri kümesi için aşağıdaki komutu girin. Kullandığınız **extendeddata** anahtar verileri için oluşturduğunuz C:\cred.xml dosyasının adını geçirilecek parametre.
   
   ```
   hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
   ```
   
   Bu komut çıktı olmadan başarıyla tamamlar. İşleri çalıştırmak için gereken kullanıcı hesapları için kimlik bilgilerini ayarlama sonra cred.xml dosyasını güvenli bir konumda depolayın veya silin.
5. RSA anahtar çifti, Linux düğümlerinden biri üzerinde oluşturulan, bunları kullanarak tamamladığınızda anahtarların silmeyi unutmayın. HPC Pack id_rsa varolan bir dosyanın veya id_rsa.pub dosyası bulursa, karşılıklı güven ayarlı değil.

> [!IMPORTANT]
> Bir yönetici tarafından gönderilen bir iş Linux düğümlerinde kök hesabı altında çalıştığından Linux iş, paylaşılan bir kümede bir Küme Yöneticisi çalışan önerilmemektedir. Ancak yönetici olmayan kullanıcı tarafından gönderilen bir iş, işi kullanıcı olarak aynı ada sahip bir yerel Linux kullanıcı hesabı altında çalışır. Bu durumda, HPC Pack karşılıklı güven bu Linux kullanıcı için proje için ayrılan düğümler arasında ayarlar. Linux kullanıcı el ile Linux düğümleri işi çalıştırmadan önce ayarlayabilirsiniz veya iş gönderildiğinde HPC Pack kullanıcı otomatik olarak oluşturur. HPC Pack kullanıcı oluşturursa, HPC Pack iş tamamlandıktan sonra onu siler. Güvenlik tehditleri azaltmak için HPC Pack anahtarları işi tamamlandıktan sonra kaldırır.
> 
> 

## <a name="set-up-a-file-share-for-linux-nodes"></a>Linux düğümleri için dosya paylaşımı ayarlama
Baş düğüm üzerindeki bir klasöre standart bir SMB paylaşımında şimdi ayarlayın. Yaygın bir yolu olan uygulama dosyaları erişmek Linux düğümleri izin vermek için Linux düğümlerinde paylaşılan klasöre bağlayın. İsterseniz, başka bir dosya paylaşımı gibi birçok senaryo - veya bir NFS paylaşım için önerilen Azure dosyaları paylaşımına - bir seçeneği kullanabilirsiniz. Dosya bilgileri ve ayrıntılı adımlar paylaşımı bkz [bir HPC Pack kümesinde Linux işlem düğümleri kullanmaya başlama](hpcpack-cluster.md).

1. Baş düğüm üzerinde bir klasör oluşturun ve herkese okuma/yazma ayrıcalıkları ayarlayarak paylaşabilirsiniz. Örneğin, baş düğümü olarak C:\OpenFOAM paylaşım \\ \\SUSE12RDMA HN\OpenFOAM. Burada, *SUSE12RDMA HN* baş düğüm ana bilgisayar adıdır.
2. Bir Windows PowerShell penceresi açın ve aşağıdaki komutları çalıştırın:
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
   clusrun /nodegroup:LinuxNodes mount -t cifs //SUSE12RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
   ```

İlk komut /openfoam LinuxNodes grubundaki tüm düğümlerde adlı bir klasör oluşturur. İkinci komut, dir_mode = ile Linux düğümlerinde paylaşılan klasör //SUSE12RDMA-HN/OpenFOAM bağlar ve 777 file_mode BITS ayarlayın. *Kullanıcıadı* ve *parola* komutta baş düğüm üzerinde bir kullanıcının kimlik bilgileri olmalıdır.

> [!NOTE]
> "\`" Semboldür ikinci komut bir PowerShell kaçış simgesi. "\`," "," (virgül) anlamına gelir. komutun bir parçasıdır.
> 
> 

## <a name="install-mpi-and-openfoam"></a>MPI ve OpenFOAM yükleyin
OpenFOAM RDMA ağ üzerinde bir MPI iş olarak çalıştırmak için Intel MPI kitaplıkları ile OpenFOAM derlemek gerekir. 

Öncelikle birkaç çalıştırın **clusrun** Intel MPI kitaplıkları (henüz yüklenmemişse) yüklemek için komutları ve kendi Linux düğümlerinde OpenFOAM. Yükleme dosyaları Linux düğümler arasında paylaşmak için önceden yapılandırılmış bir baş düğüm paylaşımını kullan.

> [!IMPORTANT]
> Bu yükleme ve derleme adımları örnektir. Linux sistem yönetim bağımlı derleyicileri ve kitaplıkları düzgün yüklendiğinden emin olmak için bazı bilgisine ihtiyacınız vardır. Belirli ortam değişkenlerini veya Intel MPI ve OpenFOAM sürümü diğer ayarlarını değiştirmeniz gerekebilir. Ayrıntılar için bkz [Linux Yükleme Kılavuzu için Intel MPI Kitaplığı](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) ve [OpenFOAM kaynak paketi yükleme](http://openfoam.org/download/2-3-1-source/) ortamınız için.
> 
> 

### <a name="install-intel-mpi"></a>Intel MPI yükleyin
Linux düğümleri bu dosyayı /openfoam erişebilmesi için indirilen yükleme paketi Intel MPI (Bu örnekte l_mpi_p_5.0.3.048.tgz) için C:\OpenFoam içinde baş düğüme kaydedin. Ardından çalıştırın **clusrun** tüm Linux düğümlerinde Intel MPI kitaplığını yüklemek için.

1. Aşağıdaki komutlar, yükleme paketini kopyalayın ve her düğümde /opt/intel ayıklayın.
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /opt/intel
   
   clusrun /nodegroup:LinuxNodes cp /openfoam/l_mpi_p_5.0.3.048.tgz /opt/intel/
   
   clusrun /nodegroup:LinuxNodes tar -xzf /opt/intel/l_mpi_p_5.0.3.048.tgz -C /opt/intel/
   ```
2. Intel MPI kitaplığı sessizce yüklemek için bir silent.cfg dosyasını kullanın. Bu makalenin sonunda örnek dosyaları bir örnek bulabilirsiniz. Bu dosya paylaşılan klasör /openfoam yerleştirin. Silent.cfg dosya hakkında daha fazla ayrıntı için bkz: [Linux Yükleme Kılavuzu - sessiz yükleme için Intel MPI Kitaplığı](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).
   
   > [!TIP]
   > Linux ile bir metin dosyası olarak silent.cfg dosyanızı kaydettiğinizde satır sonlarını (yalnızca LF, CR LF) emin olun. Bu adım, düzgün bir şekilde Linux düğümleri üzerinde çalışan sağlar.
   > 
   > 
3. Sessiz modda Intel MPI Kitaplığı yükleyin.
   
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

Yeniden başlattıktan sonra paylaşılan klasör /openfoam bağlı olduğundan emin olun.

### <a name="compile-and-install-openfoam"></a>Derleme ve OpenFOAM yükleyin
Linux düğümleri bu dosyayı /openfoam erişebilmesi için indirilen yükleme paketi C:\OpenFoam OpenFOAM kaynak paketi (Bu örnekte OpenFOAM 2.3.1.tgz) baş düğüme kaydedin. Ardından çalıştırın **clusrun** tüm Linux düğümlerinde OpenFOAM derlemek için komutları.

1. Her Linux düğümünde bir klasör /opt/OpenFOAM oluşturun, kaynak paketi bu klasöre kopyalayın ve orada ayıklayın.
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /opt/OpenFOAM
   
   clusrun /nodegroup:LinuxNodes cp /openfoam/OpenFOAM-2.3.1.tgz /opt/OpenFOAM/
   
   clusrun /nodegroup:LinuxNodes tar -xzf /opt/OpenFOAM/OpenFOAM-2.3.1.tgz -C /opt/OpenFOAM/
   ```
2. Intel MPI kitaplığı ile OpenFOAM derlemek için önce bazı ortam değişkenlerini için Intel MPI hem OpenFOAM ayarlayın. Değişkenleri ayarlamak için Settings.sh adlı bir bash betiğini kullanın. Bu makalenin sonunda örnek dosyaları bir örnek bulabilirsiniz. (Linux satır sonları ile kaydedilen) Bu dosya paylaşılan klasör /openfoam yerleştirin. Bu dosya ayrıca daha sonra bir OpenFOAM işi çalıştırmak için kullandığınız MPI ve OpenFOAM çalışma zamanları ayarlarını içerir.
3. OpenFOAM derlemek için gereken bağlı paketleri yükleyin. Linux dağıtımınıza bağlı olarak, öncelikle bir depoya eklemeniz gerekebilir. Çalıştırma **clusrun** komutları aşağıdakine benzer:
   
    ```
    clusrun /nodegroup:LinuxNodes zypper ar http://download.opensuse.org/distribution/13.2/repo/oss/suse/ opensuse
   
    clusrun /nodegroup:LinuxNodes zypper -n --gpg-auto-import-keys install --repo opensuse --force-resolution -t pattern devel_C_C++
    ```
   
    Gerekirse, bunlar düzgün çalıştığını doğrulamak için komutları çalıştırmak için her Linux düğümüne SSH.
4. OpenFOAM derlemek üzere aşağıdaki komutu çalıştırın. Derleme işleminin tamamlanması biraz zaman alır ve büyük miktarda standart çıktı günlüğü bilgilerini oluşturur, kullanın **/ aralıklı** aralıklı çıkışını görüntülemek için seçeneği.
   
   ```
   clusrun /nodegroup:LinuxNodes /interleaved source /openfoam/settings.sh `&`& /opt/OpenFOAM/OpenFOAM-2.3.1/Allwmake
   ```
   
   > [!NOTE]
   > "\`" Semboldür komutta PowerShell bir kaçış simgesi. "\`&" anlamına gelir "ve" komutunun bir parçasıdır.
   > 
   > 

## <a name="prepare-to-run-an-openfoam-job"></a>OpenFOAM işi çalıştırmak hazırlama
Şimdi iki Linux düğümlerinde OpenFoam örnekleri olan sloshingTank3D adlı bir MPI işi çalıştırmak hazır olun. 

### <a name="set-up-the-runtime-environment"></a>Çalışma zamanı ortamı ayarlama
Linux düğümlerinde MPI ve OpenFOAM için çalışma zamanı ortamı ayarlamak için baş düğüm üzerinde bir Windows PowerShell penceresinde aşağıdaki komutu çalıştırın. (Bu komut, yalnızca SUSE Linux için geçerlidir.)

```
clusrun /nodegroup:LinuxNodes cp /openfoam/settings.sh /etc/profile.d/
```

### <a name="prepare-sample-data"></a>Örnek verileri hazırlama
Baş düğüm paylaşım dosyaları (bağlı /openfoam) Linux düğümleri arasında paylaşmak için daha önce yapılandırdığınız kullanın.

1. SSH, Linux birine işlem düğümleri.
2. Bu, işlemi yapmadıysanız OpenFOAM çalışma zamanı ortamı ayarlamak için aşağıdaki komutu çalıştırın.
   
   ```
   $ source /openfoam/settings.sh
   ```
3. SloshingTank3D örnek paylaşılan klasöre kopyalayın ve bu sayfaya gidin.
   
   ```
   $ cp -r $FOAM_TUTORIALS/multiphase/interDyMFoam/ras/sloshingTank3D /openfoam/
   
   $ cd /openfoam/sloshingTank3D
   ```
4. Bu örnek varsayılan parametrelerini kullandığınızda, daha hızlı çalışmasını sağlamak için bazı parametreleri değiştirmek isteyebilirsiniz onlarca çalıştırmak için dakika sürebilir. Basit bir seçim zaman adımı değişkenleri deltaT ve sistem/controlDict dosyasındaki writeInterval değiştirmektir. Bu dosya, zaman ve okuma ve yazma çözüm veri denetimi ile ilgili tüm giriş verilerini depolar. Örneğin, gelen 0,05 deltaT 0,5 değeri ve 0,05 gelen writeInterval 0,5 değeri değiştirebilir.
   
   ![Adım değişkenleri değiştirin][step_variables]
5. Sistem/decomposeParDict dosyasında istenen değişkenlerin değerlerini belirtin. Bu örnekte 8 çekirdek iki Linux düğümleri her için 16 ve n hierarchicalCoeffs için / numberOfSubdomains şekilde ayarlayın (1 1 16), yani paralel 16 süreçleri ile OpenFOAM çalıştırma. Daha fazla bilgi için [OpenFOAM Kullanıcı Kılavuzu: 3.4 çalışan uygulamalarda paralel](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).
   
   ![İşlemler parçalara ayırın][decompose]
6. SloshingTank3D dizinden örnek verileri hazırlamak için aşağıdaki komutları çalıştırın.
   
   ```
   $ . $WM_PROJECT_DIR/bin/tools/RunFunctions
   
   $ m4 constant/polyMesh/blockMeshDict.m4 > constant/polyMesh/blockMeshDict
   
   $ runApplication blockMesh
   
   $ cp 0/alpha.water.org 0/alpha.water
   
   $ runApplication setFields  
   ```
7. Baş düğümde, örnek veri dosyalarını C:\OpenFoam\sloshingTank3D kopyalanır görmeniz gerekir. (C:\OpenFoam baş düğüm üzerindeki paylaşılan bir klasörü içindir.)
   
   ![Baş düğüm veri dosyaları][data_files]

### <a name="host-file-for-mpirun"></a>Mpırun için ana bilgisayar dosyası
Bu adımda, bir ana bilgisayar dosyası (işlem düğümleri listesini) oluşturmak, **mpırun** komutunu kullanır.

1. Linux düğümlerinden biri üzerinde bu dosya, tüm Linux düğümlerinde /openfoam/hostfile adresinden ulaşılabilir şekilde hostfile /openfoam altında adlı bir dosya oluşturun.
2. Kendi Linux düğüm adları bu dosyaya yazın. Bu örnekte, dosya şu adları içeriyor:
   
   ```       
   SUSE12RDMA-LN1
   SUSE12RDMA-LN2
   ```
   
   > [!TIP]
   > Baş düğümde C:\OpenFoam\hostfile sırasında bu dosyası da oluşturabilirsiniz. Bu seçeneği belirlerseniz, Linux ile bir metin dosyası olarak kaydedin (yalnızca LF, CR LF) satır sonlarını. Bu, düzgün bir şekilde Linux düğümleri üzerinde çalışan sağlar.
   > 
   > 
   
   **Bash betiği sarmalayıcı**
   
   Hangi düğümleri, işiniz için ayrılacak bilmiyor çok sayıda Linux düğümleri olan ve yalnızca bazıları üzerinde çalıştırılacak iş'i istiyorsanız, bir sabit konak dosyasını kullanmak için iyi bir fikir değil. Bu durumda, bir bash betiği sarmalayıcının yazma **mpırun** ana bilgisayar dosyası otomatik olarak oluşturmak için. Bu makalenin sonunda hpcimpirun.sh adlı bir örnek bash betiği sarmalayıcı bulun ve /openfoam/hpcimpirun.sh kaydedin. Bu örnek betik şunları yapar:
   
   1. Ortam değişkenleri ayarlar **mpırun**ve RDMA ağ üzerinden MPI işi çalıştırmak için bazı ek komut parametreleri. Bu durumda, aşağıdaki değişkenleri ayarlar:
      
      * I_MPI_FABRICS=shm:dapl
      * I_MPI_DAPL_PROVIDER=ofa-v2-ib0
      * I_MPI_DYNAMIC_CONNECTION = 0
   2. Ortama göre bir konak dosyası oluşturur, değişken $CCP_NODES_CORES iş etkinleştirildiğinde, HPC baş düğümü tarafından ayarlanır.
      
      $CCP_NODES_CORES biçimi, bu düzen aşağıdaki gibidir:
      
      ```
      <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
      ```
      
      Burada
      
      * `<Number of nodes>` -Bu işlem için ayrılan düğüm sayısı.  
      * `<Name of node_n_...>` -Bu işlem için ayrılan her bir düğümün adı.
      * `<Cores of node_n_...>` -Bu işlem için ayrılan düğümünde çekirdek sayısı.
      
      İşi çalıştırmak için iki düğüm gerekiyor, örneğin $CCP_NODES_CORES benzer
      
      ```
      2 SUSE12RDMA-LN1 8 SUSE12RDMA-LN2 8
      ```
   3. Çağrıları **mpırun** komut ve komut satırı için iki parametre ekler.
      
      * `--hostfile <hostfilepath>: <hostfilepath>` -betik konak dosyasının yolu
      * `-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}` -Bu işlem için ayrılan toplam çekirdek sayısını depolar HPC paketi üstbilgi düğümü tarafından ayarlanan bir ortam değişkeni. Bu durumda, bu işlemlerin sayısını belirtir. **mpırun**.

## <a name="submit-an-openfoam-job"></a>OpenFOAM işi Gönder
Artık bir iş HPC Kümesi Yöneticisi'nde gönderebilirsiniz. Komut satırlarında bazı iş görevleri için betik hpcimpirun.sh geçmesi gerekir.

1. Küme baş düğümüne bağlanın ve HPC Küme Yöneticisi'ni başlatın.
2. **Kaynak Yönetimi'nde**, Linux işlem düğümleri olduğundan emin olun **çevrimiçi** durumu. Değilse, bunları seçin ve tıklayın **çevrimiçine**.
3. İçinde **iş yönetimi**, tıklayın **yeni iş**.
4. İş için bir ad girmeniz *sloshingTank3D*.
   
   ![İş ayrıntıları][job_details]
5. İçinde **iş kaynakları**"Düğüm" olarak kaynak türünü seçin ve en az 2 olarak ayarlayın. Bu yapılandırma, bu örnekte, her biri sekiz çekirdeğe sahip iki Linux düğümlerinde işi çalıştırır.
   
   ![İş kaynakları][job_resources]
6. Tıklayın **Düzenle görevleri** 'a tıklayın ve sol gezinti **Ekle** işine bir görev eklemek için. Aşağıdaki komut satırlarını ve ayarları işlemiyle dört görev ekleyin.
   
   > [!NOTE]
   > Çalışan `source /openfoam/settings.sh` OpenFOAM ve MPI çalışma zamanı ortamlarını ayarlar, aşağıdaki görevlerin her biri önce OpenFOAM komutunu çağırır.
   > 
   > 
   
   * **Görev 1**. Çalıştırma **decomposePar** veri dosyalarını çalıştırmak için oluşturulacak **interDyMFoam** paralel.
     
     * Bir düğümü için görev atama
     * **Komut satırı** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`
     * **Çalışma dizini** -/ openfoam/sloshingTank3D
     
     Aşağıdaki şekle bakın. Kalan görevlerin benzer şekilde yapılandırın.
     
     ![Görev 1 ayrıntıları][task_details1]
   * **Görev 2**. Çalıştırma **interDyMFoam** paralel işlem örneği.
     
     * İki düğümü için görev atama
     * **Komut satırı** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`
     * **Çalışma dizini** -/ openfoam/sloshingTank3D
   * **Görev 3**. Çalıştırma **reconstructPar** processor_N_ dizinden her zaman dizinler kümesi tek bir küme içine birleştirmek için.
     
     * Bir düğümü için görev atama
     * **Komut satırı** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`
     * **Çalışma dizini** -/ openfoam/sloshingTank3D
   * **Görev 4**. Çalıştırma **foamToEnsight** OpenFOAM sonucu dönüştürmek için paralel EnSight dosyalarına biçimlendirmek ve EnSight dosyaları Ensight büyük/küçük harf dizinde dizinine yerleştirin.
     
     * İki düğümü için görev atama
     * **Komut satırı** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`
     * **Çalışma dizini** -/ openfoam/sloshingTank3D
7. Bu görevler, görev sırası artan bağımlılıkları ekleyin.
   
   ![Görev bağımlılıkları][task_dependencies]
8. Tıklayın **Gönder** bu işi çalıştırmak için.
   
   Varsayılan olarak, iş, geçerli oturum açan kullanıcı hesabı olarak HPC Pack gönderir. Tıkladıktan sonra **Gönder**, kullanıcı adı ve parola girmenizi isteyen bir iletişim kutusu görebilirsiniz.
   
   ![İş kimlik bilgileri][creds]
   
   Bazı koşullar altında kullanıcı bilgilerini önce giriş ve bu iletişim kutusunu göstermez HPC Pack hatırlar. HPC Pack yeniden yapmak için bir komut isteminde aşağıdaki komutu girin ve ardından işi Gönder.
   
   ```
   hpccred delcreds
   ```
9. İş on dakika, örnek için ayarladığınız parametrelere göre birkaç saat sürer. Isı Haritası da Linux düğümlerinde çalışan iş bakın. 
   
   ![Isı haritası][heat_map]
   
   Her düğümde sekiz işlemleri başlatıldı.
   
   ![Linux işlemleri][linux_processes]
10. İş tamamlandığında iş sonuçlarını klasörleri C:\OpenFoam\sloshingTank3D ve günlük dosyalarının C:\OpenFoam en altında bulabilirsiniz.

## <a name="view-results-in-ensight"></a>EnSight içinde sonuçlarını görüntüle
İsteğe bağlı olarak [EnSight](http://www.ensight.com/) görselleştirip OpenFOAM işin sonuçları analiz edin. Görselleştirme ve EnSight animasyonda hakkında daha fazla bilgi için bkz. Bu [video Kılavuzu](http://www.ensight.com/ensight.com/envideo/).

1. Baş düğümde EnSight yükledikten sonra başlatın.
2. Open C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.
   
   Görüntüleyicide bir tank görürsünüz.
   
   ![Tank EnSight içinde][tank]
3. Oluşturma bir **Isosurface** gelen **internalMesh**ve ardından değişkeni **alpha_water**.
   
   ![Bir isosurface oluşturma][isosurface]
4. Renginin ayarlanacağı **Isosurface_part** önceki adımda oluşturduğunuz. Örneğin, su için mavi ayarlayın.
   
   ![İsosurface rengi Düzenle][isosurface_color]
5. Oluşturma bir **ISO-volume** gelen **duvarlarının** seçerek **duvarlarının** içinde **bölümleri** paneli ve tıklayın **Isosurfaces** araç çubuğu düğmesi.
6. İletişim kutusunda **türü** olarak **Isovolume** ve en küçük **Isovolume aralığı** 0,5. İsovolume oluşturmak için tıklayın **ile seçilen parçaları oluşturma**.
7. Renginin ayarlanacağı **Iso_volume_part** önceki adımda oluşturduğunuz. Örneğin, derin su için mavi ayarlayın.
8. Renginin ayarlanacağı **duvarlarının**. Örneğin, saydam beyaz olarak ayarlayın.
9. Şimdi tıklayarak **Play** benzetim sonuçlarını görmek için.
   
    ![Tank sonucu][tank_result]

## <a name="sample-files"></a>Örnek dosya
### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>PowerShell betiği tarafından küme dağıtımı için örnek XML yapılandırma dosyası
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
### <a name="sample-silentcfg-file-to-install-mpi"></a>MPI yüklemek için örnek silent.cfg dosyası
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

### <a name="sample-settingssh-script"></a>Örnek settings.sh betiği
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


### <a name="sample-hpcimpirunsh-script"></a>Örnek hpcimpirun.sh betiği
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
