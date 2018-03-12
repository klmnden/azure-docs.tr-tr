---
title: "MPI uygulamaları çalıştırmak için Windows RDMA küme ayarlama | Microsoft Docs"
description: "Boyutuyla H16r, H16mr, A8 veya A9 Vm'lerde MPI uygulamaları çalıştırmak için Azure RDMA ağ kullanmak üzere bir Windows HPC Pack küme oluşturmayı öğrenin."
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: jeconnoc
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 7d9f5bc8-012f-48dd-b290-db81c7592215
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 03/06/2018
ms.author: danlep
ms.openlocfilehash: 437c475735ec3823de51c5f9e996a5303fe9cfa7
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="set-up-a-windows-rdma-cluster-with-hpc-pack-to-run-mpi-applications"></a>MPI uygulamaları çalıştırmak için Windows RDMA küme HPC paketi ile ayarlama
Azure ile Windows RDMA kümedeki ayarlamak [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) ve [RDMA özellikli HPC VM boyutları](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#rdma-capable-instances) paralel ileti geçirme arabirimi (MPI) uygulamalarını çalıştırmak için. RDMA özelliğine sahip, Windows Server tabanlı bir HPC Pack kümedeki düğümlerin ayarladığınızda, MPI uygulamaları düşük gecikme, doğrudan uzak bellek erişimi (RDMA) teknolojisine dayalı azure'da yüksek verimlilik ağ üzerinden verimli bir şekilde iletişim kurar.

Linux VM'ler üzerinde Azure RDMA ağ erişim Bkz MPI iş yüklerini çalıştırmak istiyorsanız [MPI uygulamaları çalıştırmak için Linux RDMA küme ayarlama](../../linux/classic/rdma-cluster.md).

## <a name="hpc-pack-cluster-deployment-options"></a>HPC Pack küme dağıtım seçenekleri
Microsoft HPC Pack bir oluşturmak için hiçbir ek ücret ödemeden şirket içi HPC kümelerinde sağlanan veya Windows veya Linux HPC uygulamaları çalıştırmak için Azure aracıdır. HPC Pack ileti geçirme arabirimi for Windows (MS-MPI) Microsoft uygulaması için bir çalışma zamanı ortamı içerir. Desteklenen bir Windows Server işletim sistemi çalıştıran RDMA özellikli örnekleriyle birlikte kullanıldığında, HPC Pack Azure RDMA ağ erişim Windows MPI uygulamaları çalıştırmak için etkili bir seçenek sağlar. 

Bu makalede, Microsoft HPC Pack 2012 R2 ile Windows RDMA küme ayarlamak için ayrıntılı yönergeler için iki senaryo ve bağlantıları tanıtılır. 

* Senaryo 1. İşlem yoğunluklu çalışan rolü örnekleri (PaaS) dağıtma
* Senaryo 2. İşlem yoğunluklu VM'ler (Iaas) hesaplama düğümlerini dağıtmak

## <a name="scenario-1-deploy-compute-intensive-worker-role-instances-paas"></a>Senaryo 1: işlem yoğunluklu çalışan rolü örnekleri (PaaS) dağıtma
Mevcut bir HPC Pack kümeden ek işlem kaynaklarında bir bulut hizmeti (PaaS) çalışan Azure çalışan rolü örnekleri (Azure düğümleri) ekleyin. "HPC paketinden Azure'a veri bloğu" olarak da adlandırılan bu özellik, bir dizi boyutları çalışan rolü örnekleri için destekler. Azure düğümleri eklerken, RDMA özellikli boyutlarından birini belirtin.

Şunlardır konuları ve adımları RDMA özellikli için veri bloğu mevcut bir kümeden Azure örnekleri (genellikle şirket içi) küme. Bir Azure VM dağıtılan bir HPC paketi üstbilgi düğümü çalışan rolü örnekleri eklemek için benzer yordamları kullanın.

> [!NOTE]
> HPC Pack ile azure'a veri bloğu bir öğretici için bkz: [HPC paketi ile karma küme ayarlama](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Özellikle RDMA özellikli için uygulama aşağıdaki adımlarda konuları not Azure düğümleri.
> 
> 

![Azure'a veri bloğu][burst]

### <a name="steps"></a>Adımlar
1. **HPC Pack 2012 R2 baş düğüm yapılandırmak ve dağıtmak**
   
    HPC Pack yükleme paketinden karşıdan [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=49922). Gereksinimler ve bir Azure veri bloğu dağıtımına hazırlanmak için yönergeler için bkz: [Microsoft HPC paketi ile Azure çalışan örneklerine veri bloğu](https://technet.microsoft.com/library/gg481749.aspx).
2. **Azure aboneliğinde bir yönetim sertifikası yapılandırma**
   
    Baş düğüm ile Azure arasındaki bağlantının güvenli hale getirmek için bir sertifika yapılandırın. Seçenekler ve yordamlar için bkz: [HPC Pack için Azure yönetim sertifikasını yapılandırma senaryoları](http://technet.microsoft.com/library/gg481759.aspx). Test dağıtımları için HPC Pack bir varsayılan Microsoft HPC Azure yönetim Azure aboneliğinize hızlı bir şekilde yükleyebilirsiniz sertifikası yükler.
3. **Yeni bir bulut hizmeti ve bir depolama hesabı oluştur**
   
    Bir bulut hizmeti (Klasik) ve bir depolama hesabı dağıtım için (Klasik) oluşturmak için Azure Portalı'nı kullanın. Bu kaynaklar, kullanmak istediğiniz H-serisi, A8 veya A9 boyutu kullanılabilir olduğu bir bölgede oluşturun. Bkz: [bölgeye göre Azure ürünleri](https://azure.microsoft.com/regions/services/).

4. **Bir Azure düğüm şablonu oluşturma**
   
    Kullanım düğümü Şablon Sihirbazı'nı HPC Küme Yöneticisi'nde oluşturun. Adımlar için bkz: [Azure düğüm şablonu oluşturma](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Templ) "Adımları için dağıtmak Azure düğümleri ile Microsoft HPC Pack".
   
    İlk sınamalar için el ile kullanılabilirlik İlkesi şablonunda yapılandırma öneririz.
5. **Küme Düğümleri Ekle**
   
    Kullanım düğümü HPC Küme Yöneticisi'nde Ekleme Sihirbazı. Daha fazla bilgi için bkz: [Windows HPC kümesine Azure düğümleri Ekle](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Add).
   
    Düğümlerin boyutunu belirtirken, RDMA özellikli örneği boyutlarından birini seçin.
   
   > [!NOTE]
   > İşlem yoğunluklu örnekler ile Azure dağıtımı için her veri bloğu içinde HPC paketi otomatik olarak en az iki RDMA özellikli örneği (örneğin, A8) proxy düğüm olarak belirttiğiniz Azure çalışan rolü örneklerine ek olarak dağıtır. Proxy düğümleri abonelik için ayrılmış ve Azure çalışan rolü örnekleri birlikte ücretlendirme çekirdek kullanın.
   > 
   > 
6. **(Sağlayamaz) başlangıç düğümleri ve işlerini çalıştırmak için bunları çevrimiçi duruma getirin**
   
    Düğümleri seçin ve **Başlat** eylem HPC Küme Yöneticisi'nde. Sağlama tamamlandıktan sonra düğümleri seçin ve kullanmak **çevrimiçine** eylem HPC Küme Yöneticisi'nde. İşlerini çalıştırmak düğümleri hazırsınız.
7. **Kümeye iş göndermek**
   
   Küme işlerini çalıştırmak için HPC Pack iş gönderme araçlarını kullanın. Bkz: [Microsoft HPC paketi: iş yönetimi](http://technet.microsoft.com/library/jj899585.aspx).
8. **(Deprovision) durdurun düğümleri**
   
   Çalışan işleri bittiğinde düğümleri çevrimdışı duruma getirin ve kullanma **durdurmak** eylem HPC Küme Yöneticisi'nde.

## <a name="scenario-2-deploy-compute-nodes-in-compute-intensive-vms-iaas"></a>Senaryo 2: Dağıtmak işlem düğümleri işlem yoğunluklu VM'ler (Iaas) olarak
Bu senaryoda, küme işlem düğümlerinde sanal makineleri bir Azure sanal ağında ve HPC paketi üstbilgi düğümü dağıtın. HPC Pack sağlayan birkaç [dağıtım seçenekleri Azure VM'de](../../linux/hpcpack-cluster-options.md)otomatik dağıtım betikleri ve Azure hızlı başlangıç şablonlarını dahil olmak üzere. Örnek olarak, aşağıdaki konuları ve adımları kullanmak için size kılavuzluk [HPC Pack Iaas dağıtım betiği](hpcpack-cluster-powershell-script.md) Azure HPC Pack 2012 R2 kümesinde dağıtımını otomatik hale getirmek için.

![Azure sanal makineleri kümedeki][iaas]

### <a name="steps"></a>Adımlar
1. **Bir küme baş düğümüne oluşturmak ve bir istemci bilgisayarda HPC Pack Iaas dağıtım betiği çalıştırarak düğümü VM'ler işlem**
   
    HPC Pack Iaas dağıtım betiği paketinden karşıdan [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).
   
    İstemci bilgisayarı hazırlamak için yapılandırma betiği ve komut dosyası çalıştırma görür oluşturma [HPC Kümesi ile HPC Pack Iaas dağıtım komut dosyası oluşturma](hpcpack-cluster-powershell-script.md). 
   
    RDMA özellikli dağıtma hakkında dikkat edilecek noktalar için işlem düğümleri, bkz: [yüksek performanslı işlem VM boyutları](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#rdma-capable-instances) ve aşağıdakilere dikkat edin:
   
   * **Sanal ağ**: kullanmak istediğiniz H-serisi, A8 veya A9 boyutu olduğu kullanılabilir bir bölgede yeni bir sanal ağ belirtin. Bkz: [bölgeye göre Azure ürünleri](https://azure.microsoft.com/regions/services/).

   * **Windows Server işletim sisteminin**: RDMA bağlantısı desteklemek için uyumlu bir Windows Server işletim sistemi Windows Server 2012 R2 gibi işlem düğümü VM'ler için belirtin.
   * **Bulut Hizmetleri**: betik Klasik dağıtım modeli kullandığından, küme sanal makineleri Azure bulut Hizmetleri kullanılarak dağıtılan (`ServiceName` yapılandırma dosyasındaki ayarları). Bir bulut hizmeti, baş düğümü ve işlem düğümleriniz farklı bir bulut hizmeti dağıtma öneririz. 
   * **Baş düğüm boyutu**: Bu senaryo için boyutu en az göz önünde bulundurun A4 (çok büyük) baş düğüm için.
   * **HpcVmDrivers uzantısı**: bir Windows Server işletim sistemi ile boyutu A8 veya A9 işlem düğümleri dağıttığınızda dağıtım komut dosyası Azure VM Aracısı'nı ve HpcVmDrivers uzantısı otomatik olarak yükler. RDMA ağa bağlanabilmeleri HpcVmDrivers işlem düğümünde VM'ler sürücüleri yükler. RDMA özellikli H-serisi Vm'lerinde HpcVmDrivers uzantısı el ile yüklemeniz gerekir. Bkz: [yüksek performanslı işlem VM boyutları](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
   * **Küme ağ yapılandırması**: dağıtım betiğini HPC paketi küme topolojisi 5 (Kurumsal ağ üzerindeki tüm düğümler) otomatik olarak ayarlar. Bu topoloji VM'ler tüm HPC paketi küme dağıtımları için gereklidir. Küme ağ topolojisi daha sonra değişmez.
1. **İşlerini çalıştırmak için işlem düğümleri çevrimiçi duruma getirin**
   
    Düğümleri seçin ve **çevrimiçine** eylem HPC Küme Yöneticisi'nde. İşlerini çalıştırmak düğümleri hazırsınız.
3. **Kümeye iş göndermek**
   
    İşlerini göndermek için baş düğümüne bağlanmak veya bunu yapmak için bir şirket içi bilgisayarın ayarlarını yapın. Bilgi için bkz: [bir HPC iş gönderme küme Azure'da](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
4. **Düğümleri çevrimdışı duruma getirin ve durdurun (deallocate) bunları**
   
    Çalışan işleri bittiğinde düğümleri çevrimdışı HPC Küme Yöneticisi'nde alın. Ardından, onları kapatmanız için Azure yönetim araçlarını kullanın.

## <a name="run-mpi-applications-on-the-cluster"></a>Kümede MPI uygulamaları çalıştırma
### <a name="example-run-mpipingpong-on-an-hpc-pack-cluster"></a>Örnek: mpipingpong bir HPC Pack kümede çalıştırın.
RDMA özellikli örneklerinin bir HPC Pack dağıtımı doğrulamak için HPC Pack çalıştırın **mpipingpong** kümede komutu. **mpipingpong** art arda gecikme süresi ve verimlilik ölçümleri hesaplamak için eşleştirilmiş düğümler ve uygulama RDMA özellikli ağ istatistiklerini arasında veri paketleri gönderir. Bu örnek, bir MPI işini çalıştırmak için tipik bir düzen gösterir (Bu durumda, **mpipingpong**) kümesi kullanarak **mpiexec** komutu.

Bu örnekte "Azure veri bloğu" yapılandırmasında Azure düğümleri eklenen varsayılır ([Senaryo 1](#scenario-1.-deploy-compute-intensive-worker-role-instances-\(PaaS\) in this article). HPC Pack Azure Vm'leri bir kümede dağıtılmışsa, farklı bir düğüme grubunu belirtin ve RDMA ağ için ağ trafiğini yönlendirmek için ek ortam değişkenlerini ayarlama komut söz dizimi değiştirmek gerekir.

Mpipingpong küme üzerinde çalıştırmak için:

1. Baş düğüm ya da düzgün yapılandırılmış istemci bilgisayarda bir komut istemi açın.
2. Dört düğüm Azure veri bloğu dağıtımını düğümler çiftleri arasındaki gecikme süresini tahmin etmek için bir küçük paket boyutu ve fazla yineleme mpipingpong çalıştırmak için bir işi göndermek için aşağıdaki komutu yazın:
   
    ```Command
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 1:100000 -op -s nul
    ```
   
    Komut gönderildiğinde iş Kimliğini döndürür.
   
    Azure Vm'lerinde dağıtılan HPC paketi küme dağıttıysanız içeren bir düğüm grubu işlem düğümü VM'ler tek bulut hizmet'te belirtin ve değiştirme **mpiexec** gibi komut:
   
    ```Command
    job submit /nodegroup:vmcomputenodes /numnodes:4 mpiexec -c 1 -affinity -env MSMPI_DISABLE_SOCK 1 -env MSMPI_PRECONNECT all -env MPICH_NETMASK 172.16.0.0/255.255.0.0 mpipingpong -p 1:100000 -op -s nul
    ```
3. İş tamamlandığında, çıkış (Bu durumda, görev 1 iş çıkışı), görüntülemek için aşağıdaki komutu yazın
   
    ```Command
    task view <JobID>.1
    ```
   
    Burada &lt; *JobId* &gt; gönderilen işi kimliğidir.
   
    Çıktı aşağıdakine benzer gecikme süresi sonuçları içerir.
   
    ![Ping pong gecikme süresi][pingpong1]
4. Azure veri bloğu düğümleri çiftleri arasındaki verimliliği tahmin etmek için işi göndermek için aşağıdaki komutu yazın **mpipingpong** büyük paket boyutu ve birkaç yinelemeleri ile:
   
    ```Command
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 4000000:1000 -op -s nul
    ```
   
    Komut gönderildiğinde iş Kimliğini döndürür.
   
    Azure Vm'lerinde dağıtılan bir HPC Pack kümede 2. adımda not ettiğiniz şekilde komut değiştirin.
5. İş tamamlandığında, çıkış (Bu durumda, görev 1 iş çıkışı), görüntülemek için aşağıdaki komutu yazın:
   
    ```Command
    task view <JobID>.1
    ```
   
   Çıktı aşağıdakine benzer üretilen iş sonuçları içerir.
   
   ![Ping pong işleme][pingpong2]

### <a name="mpi-application-considerations"></a>MPI uygulama konuları
HPC Pack Azure MPI uygulamaları çalıştırmak için dikkat edilecek noktalar aşağıda verilmiştir. Bazı yalnızca Azure düğümleri (çalışan rolü örnekleri "Azure veri bloğu" yapılandırmasında eklenen) dağıtımları için geçerlidir.

* Bir bulut hizmetinde çalışan rolü örnekleri düzenli aralıklarla bildirilmeksizin Azure tarafından (örneğin, Sistem Bakımı veya örneği başarısız durumda) sağlama. Bir MPI iş çalışırken örneği sağlama, örnek verileri kaybeder ve, MPI işinin başarısız olmasına neden olabilir, dağıtılmayan başlandığı ilk durumuna döndürür. Tek bir MPI iş için ve uzun kullanmak daha fazla düğüm işi, büyük olasılıkla bir iş çalışırken, örneklerden birini sağlama çalışır. Dosya sunucusu olarak tek bir düğüm dağıtımdaki belirtirseniz ayrıca şunları göz önünde bulundurun.
* Azure'da MPI işlerini çalıştırmak için RDMA özellikli örnekleri kullanmak gerekmez. HPC paketi tarafından desteklenen herhangi bir örnek boyutu kullanabilirsiniz. Ancak, RDMA özellikli örnekleri gecikme süresi ve düğümlerini bağlayan ağ bant genişliği için duyarlıdır görece büyük ölçekli MPI işlerini çalıştırmak için önerilir. Gecikme süresi ve bant genişliği duyarlı MPI işlerini çalıştırmak için diğer boyutlara kullanıyorsanız, tek bir görev yalnızca birkaç düğüm üzerinde çalışır, küçük bir iş çalışmadığı öneririz.
* Azure örneklerine dağıtılan uygulamalar uygulama ile ilişkili Lisans Koşulları'nı tabidir. Herhangi bir ticari uygulama bulutta çalıştırmak için lisans ya da başka kısıtlamalar için satıcıyla denetleyin. Satıcıların tümü kullandıkça öde lisansı sunmaz.
* Azure örnekleri erişim şirket içi düğümleri, paylaşımları ve lisans sunucuları için daha fazla kurulum gerekir. Örneğin, bir şirket içi lisans sunucusuna erişmek Azure düğümleri etkinleştirmek için bir siteden siteye Azure sanal ağı yapılandırabilirsiniz.
* Azure örnekleri üzerinde MPI uygulamaları çalıştırmak için her MPI uygulama Özellikli Windows Güvenlik Duvarı örneklerinde çalıştırarak kayıt **hpcfwutil** komutu. Bu güvenlik duvarı tarafından dinamik olarak atanan bir bağlantı noktası üzerinde gerçekleşmesi MPI iletişim sağlar.
  
  > [!NOTE]
  > Azure dağıtımları için veri bloğu için bir güvenlik duvarı özel durum komutu kümenize eklenen tüm düğümlerde yeni Azure otomatik olarak çalışacak şekilde de yapılandırabilirsiniz. Çalıştırdıktan sonra **hpcfwutil** komut ve, uygulama works eklediğiniz komutu başlangıç betiği, Azure düğümleri olarak doğrulayın. Daha fazla bilgi için bkz: [Azure düğümleri için bir başlangıç komut dosyası kullanma](https://technet.microsoft.com/library/jj899632.aspx).
  > 
  > 
* HPC Pack MPI iletişimi için kabul edilebilir adres aralığını belirtmek için CCP_MPI_NETMASK küme ortam değişkenini kullanır. HPC Pack 2012 R2'de başlayarak, CCP_MPI_NETMASK küme ortam değişkeni, yalnızca etki alanına katılmış küme bilgi işlem düğümleri arasındaki MPI iletişimi etkiler (ya da şirket içi veya Azure VM'de). Değişken veri bloğu içinde Azure yapılandırmaya eklenmiş düğümleri tarafından göz ardı edilir.
* MPI işlerini farklı bulut Hizmetleri (örneğin, farklı bir düğüme şablonları ya da birden çok bulut Hizmetleri'nde dağıtılan Azure VM işlem düğümlerini Azure dağıtımlar için veri bloğu) dağıtılan Azure örnekleri üzerinde çalıştırılamaz. Farklı bir düğüme şablonlarla çalışmaya birden çok Azure düğümlü dağıtımlar varsa, MPI işi yalnızca bir Azure düğümleri kümesi üzerinde çalıştırmanız gerekir.
* Kümeniz için Azure düğümleri eklemek ve bunları çevrimiçi duruma getirmeden HPC iş Zamanlayıcı hizmeti hemen düğümlerde işleri başlatmak çalışır. Yalnızca İş yükünüzün bir kısmını Azure üzerinde çalıştırabilir, güncelleştirmek veya hangi iş türleri Azure üzerinde çalıştırabilirsiniz tanımlamak için proje şablonları oluşturma emin olun. Örneğin, bir proje şablonu ile gönderilen işler yalnızca Azure düğümleri üzerinde çalıştığından emin olmak için düğüm grupları özelliği proje şablonuna ekleyin ve gerekli değer olarak AzureNodes seçin. Azure düğümleri için özel gruplar oluşturmak için Ekle HpcGroup HPC PowerShell cmdlet'ini kullanın.

## <a name="next-steps"></a>Sonraki adımlar
* HPC Pack kullanarak alternatif olarak, yönetilen Azure işlem düğümleri havuzlarının MPI uygulamaları çalıştırmak için Azure Batch hizmetiyle geliştirin. Bkz: [Azure Batch'de ileti geçirme arabirimi (MPI) uygulamalarını çalıştırmak için çok örnekli görevleri kullanma](../../../batch/batch-mpi.md).
* Linux MPI Azure RDMA ağ erişmek için bkz: uygulamaları çalıştırmak istiyorsanız [MPI uygulamaları çalıştırmak için Linux RDMA küme ayarlama](../../linux/classic/rdma-cluster.md).

<!--Image references-->
[burst]:media/hpcpack-rdma-cluster/burst.png
[iaas]:media/hpcpack-rdma-cluster/iaas.png
[pingpong1]:media/hpcpack-rdma-cluster/pingpong1.png
[pingpong2]:media/hpcpack-rdma-cluster/pingpong2.png
