---
title: MPI uygulamalarını çalıştırmak için bir Windows RDMA kümesi ayarlama | Microsoft Docs
description: Boyutuyla H16r, H16mr, A8 veya A9 Vm'lerde to run MPI uygulamalarını çalıştırmak için Azure RDMA ağ kullanmak için bir Windows HPC Pack kümesi oluşturmayı öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-service-management,hpc-pack
ms.assetid: 7d9f5bc8-012f-48dd-b290-db81c7592215
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 03/06/2018
ms.author: danlep
ms.openlocfilehash: 52338cc21e46b544c2abb79cd7094615c837a2e8
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51233788"
---
# <a name="set-up-a-windows-rdma-cluster-with-hpc-pack-to-run-mpi-applications"></a>MPI uygulamalarını çalıştırmak için HPC Pack ile Windows RDMA kümesi ayarlama
Azure ile Windows RDMA kümesi ayarlama [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) ve [RDMA özellikli HPC VM boyutları](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#rdma-capable-instances) paralel ileti geçiş arabirimi (MPI) uygulamalarını çalıştırmak için. RDMA özellikli, Windows Server tabanlı bir HPC Pack kümesinde düğümler ayarladığınızda, MPI uygulamaları düşük gecikme süreli, yüksek aktarım hızı ağ doğrudan uzak bellek erişimi (RDMA) teknolojisini temel alan Azure üzerinden verimli bir şekilde iletişim kurar.

## <a name="hpc-pack-cluster-deployment-options"></a>HPC Pack küme dağıtım seçenekleri
Microsoft HPC Pack bir oluşturmak için ek ücret ödemeden, şirket içi HPC kümelerinde sağlanan veya Windows veya Linux HPC uygulamaları çalıştırmak için Azure'da aracıdır. HPC Pack, ileti geçirme arabirimi için Windows (MS-MPI) Microsoft uygulaması için bir çalışma zamanı ortamı içerir. Desteklenen bir Windows Server işletim sistemi çalıştıran RDMA özellikli örnekler ile kullanıldığında, HPC Pack'i Azure RDMA ağ erişim Windows MPI uygulamalarını çalıştırmak için verimli bir seçenek sunar. 

Bu makalede, Microsoft HPC Pack 2012 R2 ile Windows RDMA kümesi ayarlama ile ilgili ayrıntılı kılavuz için iki senaryoları ve bağlantıları tanıtılmaktadır. 

* Senaryo 1. Yoğun işlem gücü kullanımlı çalışan rolü örnekleri (PaaS) dağıtma
* Senaryo 2. Yoğun işlem gücü kullanımlı VM'ler (Iaas) işlem düğümlerine dağıtmak

## <a name="scenario-1-deploy-compute-intensive-worker-role-instances-paas"></a>Senaryo 1: yoğun işlem gücü kullanımlı çalışan rolü örnekleri (PaaS) dağıtma
Mevcut bir HPC Pack kümeden ek bilgi işlem kaynaklarının (PaaS) bulut hizmetinde çalışan Azure çalışan rolü örnekleri (Azure düğümleri) ekleyin. "Azure'da HPC paketinden bloğu" olarak da bilinir, bu özellik, çalışan rolü örnekleri için bir dizi boyutları destekler. Azure düğümleri eklerken, RDMA özellikli boyutlarından birini belirtin.

Şunlardır değerlendirmeler ve adımlar RDMA özellikli için veri bloğu için mevcut bir Azure örnekleri (genellikle şirket içi) kümesi. Çalışan rolü örnekleri, Azure VM'deki dağıtılmış bir HPC paketi üstbilgi düğümü eklemek için benzer yordamları kullanın.

> [!NOTE]
> HPC Pack ile azure'a veri bloğu bir öğretici için bkz [HPC Pack ile karma kümesi ayarlama](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Özellikle RDMA özellikli için geçerli olan aşağıdaki adımlarda dikkat edilecek noktalara dikkat edin. Azure düğümleri.
> 
> 

![Azure'a veri bloğu][burst]

### <a name="steps"></a>Adımlar
1. **Dağıtma ve bir HPC Pack 2012 R2 baş düğüm yapılandırma**
   
    HPC Pack yükleme paketinden indirme [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=49922). Gereksinimler ve bir Azure veri bloğu dağıtımına hazırlanmak için yönergeler için bkz. [Microsoft HPC Pack ile Azure çalışan örneklerine patlama](https://technet.microsoft.com/library/gg481749.aspx).
2. **Azure aboneliğinde bir yönetim sertifikası yapılandırma**
   
    Baş düğüm ve Azure arasındaki bağlantıyı güvenli hale getirmek için bir sertifika yapılandırın. Seçenekleri ve yordamlar için bkz. [Azure yönetim sertifikası için HPC Pack yapılandırma senaryolarını](https://technet.microsoft.com/library/gg481759.aspx). Test dağıtımları için HPC Pack bir varsayılan Microsoft HPC Azure yönetimi, Azure aboneliğinize hızlı bir şekilde karşıya yükleyebilirsiniz sertifikası yükler.
3. **Yeni bir bulut hizmeti ve depolama hesabı oluşturma**
   
    Azure portalını (Klasik) bir bulut hizmeti ve depolama hesabı (Klasik) dağıtım için oluşturmak için kullanın. Bu kaynaklar, kullanmak istediğiniz H serisi, A8 veya A9 boyutu kullanılabilir olduğu bir bölgede oluşturun. Bkz: [bölgelere göre Azure ürünleri](https://azure.microsoft.com/regions/services/).

4. **Bir Azure düğümüne şablonu oluşturma**
   
    Kullanım HPC Kümesi Yöneticisi'nde düğümü Şablon Sihirbazı oluşturma. Adımlar için bkz: [Azure düğüm şablonu oluşturma](https://technet.microsoft.com/library/gg481758.aspx#BKMK_Templ) "Adımları için dağıtma Azure düğümleri ile Microsoft HPC Pack".
   
    İlk testler için el ile kullanılabilirlik İlkesi şablonunda yapılandırma öneririz.
5. **Küme Düğümleri Ekle**
   
    Kullanım düğümü Sihirbazı HPC Kümesi Yöneticisi'nde ekleyin. Daha fazla bilgi için [Windows HPC kümesine Azure düğümleri Ekle](https://technet.microsoft.com/library/gg481758.aspx#BKMK_Add).
   
    Düğümlerin boyutunu belirtirken, RDMA özellikli örneği boyutlarından birini seçin.
   
   > [!NOTE]
   > Yoğun işlem gücü kullanımlı örnekler ile Azure dağıtımı için her veri bloğu içinde HPC Pack en az iki RDMA özellikli örneği (örneğin, A8), belirttiğiniz Azure çalışan rolü örneklerinin yanı sıra proxy düğümleri olarak otomatik olarak dağıtır. Proxy düğümleri çekirdek aboneliğe ayrılır ve ücretleri Azure çalışan rolü örnekleri ile birlikte kullanın.
   > 
   > 
6. **(Sağlayamaz) başlangıç düğümleri ve işlerini çalıştırmak için bunları çevrimiçi duruma getirin**
   
    Düğümleri seçme ve kullanma **Başlat** HPC Kümesi Yöneticisi'nde eylem. Sağlama tamamlandığında, düğümleri seçin ve kullanmak **çevrimiçine** HPC Kümesi Yöneticisi'nde eylem. Düğümleri, işler çalıştırmaya hazırsınız.
7. **Kümeye işlerini gönderme**
   
   Küme işlerini çalıştırmak için HPC Pack iş gönderme araçlarını kullanın. Bkz: [Microsoft HPC paketi: iş yönetimi](https://technet.microsoft.com/library/jj899585.aspx).
8. **Durdur (sağlamayı kaldırma) düğümler**
   
   Çalışan işleri tamamladığınızda, düğüm çevrimdışı duruma getirin ve kullanma **Durdur** HPC Kümesi Yöneticisi'nde eylem.

## <a name="scenario-2-deploy-compute-nodes-in-compute-intensive-vms-iaas"></a>Senaryo 2: Dağıtma işlem düğümleri yoğun işlem gücü kullanımlı sanal makinelerinde (Iaas)
Bu senaryoda, HPC paketi üstbilgi düğümü ve bir Azure sanal ağı, küme işlem düğümleri üzerinde Vm'leri dağıtın. HPC Pack sağlayan çeşitli [dağıtım seçenekleri Azure sanal makinelerinde](../../windows/hpcpack-cluster-options.md)otomatik dağıtım betikleri ve Azure hızlı başlangıç şablonları dahil olmak üzere. Örneğin, aşağıdaki konuları ve adımları kullanmak için size yol [HPC Pack Iaas dağıtım betiği](hpcpack-cluster-powershell-script.md) azure'daki bir HPC Pack 2012 R2 kümesine dağıtımını otomatik hale getirmek için.

![Azure sanal makineleri kümedeki][iaas]

### <a name="steps"></a>Adımlar
1. **Küme baş düğümünü oluşturma ve bir istemci bilgisayara HPC Pack Iaas dağıtım betiği çalıştırarak düğüm Vm'leri işlem**
   
    HPC Pack Iaas dağıtım betiği paketinden indirme [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).
   
    İstemci bilgisayarı hazırlamak için betik yapılandırma dosyası ve betik çalıştırma bakın oluşturma [HPC Pack Iaas dağıtım betiği ile bir HPC kümesi oluşturma](hpcpack-cluster-powershell-script.md). 
   
    RDMA özellikli dağıtma konuları için işlem düğümleri, bkz: [yüksek performanslı işlem VM boyutları](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#rdma-capable-instances) ve aşağıdakilere dikkat edin:
   
   * **Sanal ağ**: kullanmak istediğiniz H serisi, A8 veya A9 boyutu kullanılabildiği bir bölgede yeni bir sanal ağ belirtin. Bkz: [bölgelere göre Azure ürünleri](https://azure.microsoft.com/regions/services/).

   * **Windows Server işletim sistemi**: RDMA bağlantısı desteklemek için uyumlu Windows Server işletim sistemi Windows Server 2012 R2 gibi bilgi işlem düğümü sanal makineleri için belirtin.
   * **Bulut Hizmetleri**: betik Klasik dağıtım modeli kullandığından, kümesi Vm'lerini Azure bulut Hizmetleri kullanılarak dağıtılan (`ServiceName` yapılandırma dosyasındaki ayarları). Bir bulut hizmetinde, baş düğüm ve farklı bir bulut hizmeti, işlem düğümlerine dağıtmak öneririz. 
   * **Baş düğüm boyutu**: Bu senaryo için en az bir boyutunu dikkate A4 (çok büyük) ve baş düğüm için.
   * **HpcVmDrivers uzantısı**: bir Windows Server işletim sistemi ile boyutu A8 veya A9 işlem düğümleri dağıttığınızda dağıtım komut dosyası Azure VM Aracısı'nı ve HpcVmDrivers uzantısı otomatik olarak yükler. RDMA ağ bağlanabilmelerini HpcVmDrivers Vm'leri işlem düğümünde sürücüleri yükler. RDMA özellikli H serisi Vm'lerde HpcVmDrivers uzantıyı el ile yüklemeniz gerekir. Bkz: [yüksek performanslı işlem VM boyutları](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
   * **Küme ağ yapılandırması**: dağıtım betiği HPC Pack kümesinde topolojisi 5 (Kurumsal ağ üzerindeki tüm düğümleri) otomatik olarak ayarlar. Bu topoloji, HPC Pack küme içindeki tüm dağıtımlarda yer VM'ler için gereklidir. Daha sonra küme ağ topolojisini değiştirmeyin.
1. **İşleri çalıştırmak için işlem düğümlerine Getir**
   
    Düğümleri seçme ve kullanma **çevrimiçine** HPC Kümesi Yöneticisi'nde eylem. Düğümleri, işler çalıştırmaya hazırsınız.
3. **Kümeye işlerini gönderme**
   
    İşleri göndermek için baş düğümüne bağlanmak veya bunu yapmak için bir şirket içi bilgisayarını ayarlayın. Bilgi için [Azure'da bir HPC işleri gönderme küme](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
4. **Düğüm çevrimdışı duruma getirin ve durdurun (serbest bırakın) bunları**
   
    Çalışan işleri tamamladığınızda, HPC Kümesi Yöneticisi'nde çevrimdışı düğümleri yararlanın. Ardından, bunları kapatmak için Azure yönetim araçlarını kullanın.

## <a name="run-mpi-applications-on-the-cluster"></a>MPI uygulamalarını küme üzerinde çalıştırılır.
### <a name="example-run-mpipingpong-on-an-hpc-pack-cluster"></a>Örnek: bir HPC Pack kümesinde mpipingpong çalıştırın
RDMA özellikli örnekler dağıtımının bir HPC Pack doğrulamak için HPC Pack çalıştırma **mpipingpong** kümede komutu. **mpipingpong** art arda ölçümlerin gecikme süresi ve aktarım hızını hesaplamak için eşleştirilmiş düğümleri ve istatistikleri uygulama RDMA özellikli ağ arasındaki veri paketleri gönderir. Bu örnek, bir MPI işi çalıştırmak için tipik bir düzen gösterir. (Bu durumda, **mpipingpong**) kümesi kullanarak **mpiexec** komutu.

Bu örnekte, bir "azure'a veri bloğu" yapılandırmasında, eklediğiniz Azure düğümleri varsayılır ([Senaryo 1](#scenario-1.-deploy-compute-intensive-worker-role-instances-\(PaaS\) in this article). Azure Vm'leri kümede HPC Pack dağıttıysanız, farklı bir düğüme grubunu belirtin ve RDMA ağ ağ trafiğini yönlendirmek için ek ortam değişkenlerini ayarlamak için komut sözdizimini değiştirmek üzere gerekir.

Mpipingpong kümede çalıştırmak için:

1. Baş düğüm veya düzgün bir şekilde yapılandırılmış istemci bilgisayarda, bir komut istemi açın.
2. Dört düğüm dağıtımının bir Azure veri bloğu düğümler çiftleri arasındaki gecikme süresini tahmin etmek için bir küçük bir paket boyutu ve fazla yineleme ile mpipingpong çalıştırılacak bir iş göndermek için aşağıdaki komutu yazın:
   
    ```Command
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 1:100000 -op -s nul
    ```
   
    Komut gönderilen işin kimliği döndürür.
   
    Azure Vm'lerinde dağıtılan HPC Pack kümesi dağıttıysanız, işlem düğümü Vm'leri tek bulut hizmet'te içeren bir düğüm grubunu belirtin ve değiştirme **mpiexec** komutuyla şu şekilde:
   
    ```Command
    job submit /nodegroup:vmcomputenodes /numnodes:4 mpiexec -c 1 -affinity -env MSMPI_DISABLE_SOCK 1 -env MSMPI_PRECONNECT all -env MPICH_NETMASK 172.16.0.0/255.255.0.0 mpipingpong -p 1:100000 -op -s nul
    ```
3. İş tamamlandığında (Bu durumda, 1. Görev iş çıktısı), çıkışı görüntülemek için aşağıdaki komutu yazın
   
    ```Command
    task view <JobID>.1
    ```
   
    Burada &lt; *JobId* &gt; gönderilen işin kimliği.
   
    Çıktı aşağıdakine benzer gecikme süresi sonuçları içerir.
   
    ![Ping pong gecikme süresi][pingpong1]
4. Azure veri bloğu düğümleri çiftleri arasındaki aktarım hızını tahmin etmek için çalıştırılacak bir iş göndermek için aşağıdaki komutu yazın **mpipingpong** büyük paket boyutu ve birkaç yineleme ile:
   
    ```Command
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 4000000:1000 -op -s nul
    ```
   
    Komut gönderilen işin kimliği döndürür.
   
    Azure Vm'lerine dağıtılmış bir HPC Pack kümesinde, adım 2'de belirtildiği gibi komutu değiştirin.
5. İş tamamlandığında (Bu durumda, 1. Görev iş çıktısı), çıkışı görüntülemek için aşağıdaki komutu yazın:
   
    ```Command
    task view <JobID>.1
    ```
   
   Çıktı aşağıdakine benzer bir aktarım hızı sonuçlarını içerir.
   
   ![Ping pong aktarım hızı][pingpong2]

### <a name="mpi-application-considerations"></a>MPI uygulama konusunda dikkat edilecekler
Azure'da HPC Pack ile MPI uygulamalarını çalıştırmak için dikkat edilecek noktalar aşağıda verilmiştir. Bazı yalnızca Azure düğümleri (çalışan rolü örnekleri eklenen bir "azure'a veri bloğu" yapılandırma) dağıtımlar için geçerlidir.

* Bir bulut hizmetinde çalışan rolü örnekleri düzenli aralıklarla bir bildirim olmadan Azure tarafından (örneğin, Sistem Bakımı veya bir örneği başarısız durumda) daralıp. MPI işi devam ederken örneği daralıp, örnek verilerini kaybeder ve bunu öncelikle, hangi MPI işinin başarısız olmasına neden olabilir dağıtıldığında durumuna döndürür. Tek bir MPI işin ve uzun süre kullanan daha fazla düğüm işi, büyük olasılıkla bir iş çalışırken, örneklerden birini daralıp çalıştırır. Ayrıca dosya sunucusu olarak tek düğüm dağıtımında belirtirseniz bu düşünün.
* Azure'da MPI işlerini çalıştırmak için RDMA özellikli örnekler kullanmak zorunda değilsiniz. HPC Pack tarafından desteklenen herhangi bir örnek boyutu kullanabilirsiniz. Ancak, RDMA özellikli örnekler gecikme süresi ve düğümlerini bağlayan ağ bant genişliği için duyarlıdır görece büyük ölçekli MPI işlerini çalıştırmak için önerilir. Gecikme süresi ve bant genişliği duyarlı MPI işlerini çalıştırma için diğer boyutlarını kullanıyorsanız, tek bir görev yalnızca birkaç düğüm üzerinde çalıştığı küçük işleri çalıştırmanızı öneririz.
* Azure örnekleri için dağıtılan uygulamaları uygulamayla ilişkili lisans koşullarına tabidir. Birlikte ticari uygulamaları bulutta çalıştırmak için lisans ya da başka kısıtlamalar için satıcıyla denetleyin. Satıcıların tümü kullandıkça öde lisansı sunmaz.
* Azure örnekleri erişimi şirket içi düğümlerinizi, paylaşımları ve lisans sunucuları için ek kurulum gerekir. Örneğin, bir şirket içi lisans sunucusuna erişmek Azure düğümleri etkinleştirmek için bir siteden siteye Azure sanal ağı yapılandırabilirsiniz.
* Azure örnekleri üzerinde MPI uygulamalarını çalıştırmak için her MPI uygulama Özellikli Windows Güvenlik Duvarı örneklerinde çalıştırarak kayıt **hpcfwutil** komutu. Bu, güvenlik duvarı tarafından dinamik olarak atanan bir bağlantı noktası üzerinde gerçekleşmesi MPI iletişim sağlar.
  
  > [!NOTE]
  > Azure dağıtımları için veri bloğu için otomatik olarak kümenize eklenen tüm yeni Azure düğümleri üzerinde çalıştırmak için bir güvenlik duvarı özel durum komutu yapılandırabilirsiniz. Çalıştırdıktan sonra **hpcfwutil** komut ve uygulamanın çalıştığı eklediğiniz komutu Azure düğümlerinizi için başlatma betiği için doğrulayın. Daha fazla bilgi için [Azure düğümleri için bir başlangıç betiği kullanma](https://technet.microsoft.com/library/jj899632.aspx).
  > 
  > 
* HPC Pack MPI iletişimi için kabul edilebilir adresleri aralığını belirtmek için CCP_MPI_NETMASK küme ortam değişkenini kullanır. HPC Pack 2012 R2'de başlayarak, CCP_MPI_NETMASK küme ortam değişkeni, yalnızca etki alanına katılmış bir küme işlem düğümleri arasında MPI iletişimi etkiler (şirket içinde veya Azure sanal makinelerinde). Değişken bir veri bloğu içinde Azure yapılandırmaya eklenmiş düğümler tarafından göz ardı edilir.
* MPI işleri, farklı bulut hizmetlerinde (örneğin, farklı bir düğüme şablonları veya Azure VM işlem düğümü birden çok bulut hizmetlerinde dağıtılan Azure dağıtımları için aşırı yük) dağıtılan Azure örnekleri üzerinde çalıştırılamaz. MPI işi, farklı bir düğüme şablonlarla çalışmaya birden çok Azure düğümlü dağıtımlar varsa, yalnızca bir dizi Azure düğümleri üzerinde çalıştırmanız gerekir.
* Kümenize Azure düğümleri eklemek ve bunları çevrimiçi duruma getirmeden, düğümlerde işleri başlatmak HPC İş Zamanlayıcısı hizmeti hemen çalışır. Yalnızca kendi İş yükünüzün bir kısmını Azure'da çalıştırabileceğiniz, güncelleştirme veya hangi iş türleri Azure'da çalıştırabilirsiniz tanımlamak için proje şablonları oluşturma emin olun. Örneğin, bir proje şablonu ile gönderilen bir iş yalnızca Azure düğümleri üzerinde çalıştığından emin olmak için düğüm grupları özelliği proje şablonuna ekleyin ve AzureNodes gerekli değeri olarak seçin. Azure düğümlerinizi için özel gruplar oluşturmak için Add-HpcGroup HPC PowerShell cmdlet'ini kullanın.

## <a name="next-steps"></a>Sonraki adımlar
* HPC Pack kullanmaya alternatif, yönetilen Azure bilgi işlem düğümü havuzlarını MPI uygulamalarını çalıştırmak için Azure Batch hizmeti ile geliştirin. Bkz: [Azure Batch'de ileti geçirme arabirimi (MPI) uygulamalarını çalıştırmak için çok örnekli görevleri kullanma](../../../batch/batch-mpi.md).

<!--Image references-->
[burst]:media/hpcpack-rdma-cluster/burst.png
[iaas]:media/hpcpack-rdma-cluster/iaas.png
[pingpong1]:media/hpcpack-rdma-cluster/pingpong1.png
[pingpong2]:media/hpcpack-rdma-cluster/pingpong2.png
