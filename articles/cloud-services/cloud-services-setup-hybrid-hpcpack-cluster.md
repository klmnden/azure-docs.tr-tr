---
title: Azure'da karma HPC Pack kümesi ayarlama | Microsoft Docs
description: Küçük bir, karma yüksek performanslı bilgi işlem (HPC) kümesi ayarlamak için Microsoft HPC Pack ile Azure'ı kullanmayı öğrenin
services: cloud-services
documentationcenter: ''
author: dlepow
manager: timlt
editor: ''
tags: hpc-pack
ms.assetid: ''
ms.service: cloud-services
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: danlep
ms.openlocfilehash: 55dfd7e5ea93ae941d73612cc70ed82d48db725a
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51236747"
---
# <a name="set-up-a-hybrid-high-performance-computing-hpc-cluster-with-microsoft-hpc-pack-and-on-demand-azure-compute-nodes"></a>Microsoft HPC Pack ve isteğe bağlı Azure işlem düğümleriyle hibrit bir yüksek performanslı bilgi işlem (HPC) kümesi ayarlayın
Microsoft HPC Pack 2012 R2 ve Azure bilgi işlem (HPC) kümesi küçük, karma yüksek performanslı bir ayarlamak için kullanın. Bu makalede gösterilen küme bir şirket içi HPC Pack baş düğümünden oluşur ve bazı işlem düğümleri dağıttığınız isteğe bağlı bir Azure bulut hizmeti. Ardından hibrit kümede bilgi işlem işlerini çalıştırabilirsiniz.

![Karma HPC Kümesi][Overview] 

Bu öğreticide küme "buluta yığma" olarak da adlandırılan bir yaklaşım gösterilmektedir yoğun işlem gücü kullanımlı uygulamaları çalıştırmak için ölçeklenebilir, isteğe bağlı Azure kaynaklarını kullanmak için.

Bu öğretici, önceden deneyiminiz olmasa işlem kümeleri veya HPC Pack varsayar. Yalnızca tanıtım amacıyla hızla karma işlem kümesi dağıtmanıza yardımcı olmak için tasarlanmıştır. Değerlendirmeler ve adımlar bir üretim ortamında daha büyük ölçekte karma HPC Pack kümesi dağıtmak veya HPC Pack 2016 kullanmak için bkz. [ayrıntılı yönergeler](https://go.microsoft.com/fwlink/p/?LinkID=200493). HPC Pack ile diğer senaryolar için Azure sanal makineleri küme dağıtımında otomatik dahil olmak üzere, bkz: [azure'de Microsoft HPC Pack ile HPC küme seçenekleri](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="prerequisites"></a>Önkoşullar
* **Azure aboneliği** -bir Azure aboneliğiniz yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/) yalnızca birkaç dakika içinde.
* **Windows Server 2012 R2 veya Windows Server 2012 çalıştıran bir şirket içi bilgisayarınıza** -HPC küme baş düğümü olarak bu bilgisayarı kullanın. Windows Server çalıştıran değil ise, karşıdan yükleyip kurabileceğiniz bir [Değerlendirme sürümü](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2).
  
  * Bilgisayar bir Active Directory etki alanına katılması gerekir. Test amaçları için baş düğümü bilgisayarının bir etki alanı denetleyicisi olarak yapılandırabilirsiniz. Baş düğümü bilgisayarının bir etki alanı denetleyicisi olarak Yükselt ve Active Directory etki alanı Hizmetleri sunucu rolü eklemek için Windows Server belgelerine bakın.
  * HPC Pack desteklemek için işletim sistemi, bu dillerden birinde yüklenmelidir: İngilizce, Japonca veya Çince (Basitleştirilmiş).
  * Önemli ve kritik güncelleştirmelerin yüklü olduğunu doğrulayın.
* **HPC Pack 2012 R2** - [indirme](https://go.microsoft.com/fwlink/p/?linkid=328024) yükleme paketini en son sürümü için ücretsiz ve baş düğüm bilgisayara kopyalayın. Windows Server yüklemesini olarak aynı dilde yükleme dosyalarını seçin.

    >[!NOTE]
    > HPC Pack 2012 R2 yerine HPC Pack 2016 kullanmak istiyorsanız, ek yapılandırma gerekmez. Bkz: [ayrıntılı yönergeler](https://go.microsoft.com/fwlink/p/?LinkID=200493).
    > 
* **Etki alanı hesabı** -bu hesap HPC paketi yüklemek için yerel yönetici izinlerine sahip baş düğümde yapılandırılması gerekir.
* **TCP bağlantısı 443 numaralı bağlantı noktasında** azure'a baş düğümünden.

## <a name="install-hpc-pack-on-the-head-node"></a>HPC Pack baş düğümüne yükleme
Microsoft HPC Pack ilk Windows Server çalıştıran şirket içi bilgisayarınıza yükleyin. Bu bilgisayar kümesinin baş düğümünü olur.

1. Baş düğümünde yerel yönetici izinlerine sahip bir etki alanı hesabı kullanarak oturum açın.

2. HPC Pack yükleme dosyalarından Setup.exe çalıştırarak HPC Pack Yükleme Sihirbazı'nı başlatın.

3. Üzerinde **HPC Pack 2012 R2 Kurulum** ekranında **yeni yükleme veya varolan yüklemeye yeni özellikler eklemek**.

    ![HPC Pack 2012 Kurulumu][install_hpc1]

4. Üzerinde **Microsoft yazılım kullanıcı Sözleşmesi sayfasında**, tıklayın **sonraki**.

5. Üzerinde **yükleme türünü seç** sayfasında **bir baş düğüm oluşturarak yeni bir HPC kümesi oluşturma**ve ardından **sonraki**.

6. Sihirbaz, birkaç yükleme öncesi testleri çalıştırır. Tıklayın **sonraki** üzerinde **yükleme kuralları** tüm testler başarıyla geçirilirse sayfa. Aksi takdirde, sağlanan bilgileri gözden geçirin ve ortamınızda gerekli değişiklikleri yapın. Ardından testleri yeniden çalıştırın veya gerekiyorsa Yükleme Sihirbazı'nı yeniden başlatın.
7. Üzerinde **HPC DB yapılandırma** sayfasında, emin olun **baş düğüm** tüm HPC veritabanlarını seçilir ve ardından **sonraki**. 

    ![DB yapılandırma][install_hpc4]

8. Sihirbazın kalan sayfalarında varsayılan seçimleri kabul edin. Üzerinde **gerekli bileşenleri yükleme** sayfasında **yükleme**.
   
    ![Yükleme][install_hpc6]

9. Yükleme tamamlandıktan sonra'seçeneğinin işaretini kaldırın **HPC Küme Yöneticisi'ni başlatın** ve ardından **son**. (Daha sonraki bir adımda HPC Küme Yöneticisi'ni başlatın.)
   
    ![Son][install_hpc7]

## <a name="prepare-the-azure-subscription"></a>Azure aboneliği hazırlama
Aşağıdaki adımlarda gerçekleştirmek [Azure portalında](https://portal.azure.com) Azure aboneliğiniz. Bu adımları tamamladıktan sonra şirket içi baş düğümünden Azure düğümleri dağıtabilirsiniz. 
  
  > [!NOTE]
  > Ayrıca, daha sonra ihtiyacınız Azure abonelik Kimliğinizi not edin. Kimliği Bul **abonelikleri** portalında.
  > 

### <a name="upload-the-default-management-certificate"></a>Varsayılan yönetim sertifikasını karşıya yükleyin
HPC Pack otomatik olarak imzalanan bir sertifika adı bir Azure yönetim sertifikası karşıya yüklediğiniz varsayılan Microsoft HPC Azure yönetim sertifikası, baş düğüm yükler. Bu sertifika için sağlanan baş düğüm ve Azure arasındaki bağlantıyı güvenli hale getirmek için test ve kavram kanıtı dağıtımları.

1. Baş düğüm bilgisayarında oturum açın [Azure portalında](https://portal.azure.com).

2. Tıklayın **abonelikleri** > *your_subscription_name*.

3. Tıklayın **yönetim sertifikaları** > **karşıya**.

4. Baş düğüm için dosya C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer göz atın. ' A tıklayarak **karşıya**.

   
**Varsayılan HPC Azure Management** sertifika yönetim sertifikaları listesinde görünür.

### <a name="create-an-azure-cloud-service"></a>Bir Azure bulut hizmeti oluşturma
> [!NOTE]
> Bulut hizmeti ve depolama hesabı (bir sonraki adımda), en iyi performans için aynı coğrafi bölgede oluşturun.
> 
> 

1. Portalında **bulut Hizmetleri (Klasik)** > **+ Ekle**.

2.  Hizmeti için bir DNS adını yazın, bir kaynak grubunu ve konumu seçin ve ardından **Oluştur**.


### <a name="create-an-azure-storage-account"></a>Azure Storage hesabı oluşturma
1. Portalında **depolama hesapları (Klasik)** > **+ Ekle**.

2. Hesap için bir ad girin ve seçin **Klasik** dağıtım modeli.

3. Bir kaynak grubunu ve konumu seçin ve diğer ayarları varsayılan değerlerinde bırakın. Sonra **Oluştur**’a tıklayın.

## <a name="configure-the-head-node"></a>Baş düğüm yapılandırma
Azure düğümleri dağıtmak ve iş göndermek için HPC Küme Yöneticisi'ni kullanmak için öncelikle bazı gerekli küme yapılandırma adımlarını gerçekleştirin.

1. Baş düğümde HPC Küme Yöneticisi'ni başlatın. Varsa **baş düğüm Seç** iletişim kutusu görüntülendikten sonra **yerel bilgisayar**. **Dağıtım Yapılacaklar listesi** görünür.

2. Altında **gerekli dağıtım görevlerini**, tıklayın **ağınızı yapılandırmak**.
   
    ![Ağ yapılandırma][config_hpc2]

3. Ağ Yapılandırma Sihirbazı'nda seçin **yalnızca bir kurumsal ağ üzerindeki tüm düğümleri** (topoloji 5). Basit tanıtım amacıyla bu ağ yapılandırmadır.
   
    ![Topoloji 5][config_hpc3]
   
4. Tıklayın **sonraki** sihirbazın kalan sayfalarında varsayılan değerleri kabul etmek için. Ardından **gözden geçirme** sekmesinde **yapılandırma** ağ yapılandırmasını tamamlamak için.

5. İçinde **dağıtım Yapılacaklar listesi**, tıklayın **yükleme kimlik bilgileri sağlamak**.

6. İçinde **yükleme kimlik bilgileri** iletişim kutusunda, HPC paketi yüklemek için kullanılan etki alanı hesabının kimlik bilgilerini yazın. Daha sonra, **Tamam**'a tıklayın. 
   
    ![Yükleme kimlik bilgileri][config_hpc6]
   
7. İçinde **dağıtım Yapılacaklar listesi**, tıklayın **adlandırma yeni düğümleri yapılandırmak**.

8. İçinde **belirtin düğüm adlandırma serisi** iletişim kutusunda, adlandırma serisi ve'a tıklayın varsayılanı kabul **Tamam**. Bu öğreticide eklediğiniz Azure düğümleri otomatik olarak adlandırılmış olsa da bu adımı tamamlayın.
   
    ![Düğüm adlandırma][config_hpc8]
   
9. İçinde **dağıtım Yapılacaklar listesi**, tıklayın **düğüm şablonu oluşturma**. Öğreticide daha sonra düğüm şablonu kümesine Azure düğümleri eklemek için kullanın.

10. Oluşturma düğüm Şablonu Sihirbazı'nda aşağıdakileri yapın:
    
    a. Üzerinde **düğüm şablon türü seçin** sayfasında **Windows Azure düğüm şablonuna görev**ve ardından **sonraki**.
    
    ![Düğüm şablonu][config_hpc10]
    
    b. Tıklayın **sonraki** varsayılan şablon adı kabul etmek için.
    
    c. Üzerinde **sağladığınız abonelik bilgileri** sayfasında, Azure abonelik Kimliğinizi (Azure hesap bilgilerinizi kullanılabilir) girin. Ardından **yönetim sertifikası**, tarayabileceği **varsayılan Microsoft HPC Azure yönetimi.** Ardından **İleri**'ye tıklayın.
    
    ![Düğüm şablonu][config_hpc12]
    
    d. Üzerinde **hizmet bilgileri sağlayan** sayfasında, bulut hizmeti ve bir önceki adımda oluşturduğunuz depolama hesabını seçin. Ardından **İleri**'ye tıklayın.
    
    ![Düğüm şablonu][config_hpc13]
    
    e. Tıklayın **sonraki** sihirbazın kalan sayfalarında varsayılan değerleri kabul etmek için. Ardından **gözden geçirme** sekmesinde **Oluştur** düğüm şablonu oluşturmak için.
    
    > [!NOTE]
    > Varsayılan olarak, Azure düğümü şablon düğümler el ile durdurun ve (sağlayamaz) başlatmak için HPC Küme Yöneticisi'ni kullanarak ayarları içerir. İsteğe bağlı olarak, otomatik olarak Azure düğümleri durdurmak ve başlatmak için bir zamanlama da yapılandırabilirsiniz.
    > 
    > 

## <a name="add-azure-nodes-to-the-cluster"></a>Azure düğümleri kümeye ekleme
Artık kümesine Azure düğümleri eklemek için düğüm şablonuna görev kullanın. Kümeye düğüm ekleme depolar yapılandırma bilgilerini (sağlayamaz) başlayabilmesi bulut hizmetindeki herhangi bir zamanda bunları. Bulut hizmeti örneği çalıştırıldıktan sonra aboneliğiniz yalnızca Azure düğümleri için ücret.

İki küçük düğümleri eklemek için aşağıdaki adımları izleyin.

1. HPC Küme Yöneticisi'nde **düğüm Yönetimi** (adlı **kaynak yönetimi** HPC Pack'ın geçerli sürümlerinde) > **düğümü Ekle**.
   
    ![Düğüm Ekle][add_node1]

2. Ekleme düğüm Sihirbazı üzerinde **dağıtım yöntemini seçin** sayfasında **Ekle Windows Azure düğümleri**ve ardından **sonraki**.
   
    ![Azure düğümü ekleme][add_node1_1]

3. Üzerinde **belirtin yeni düğümler** sayfasında, daha önce oluşturduğunuz Azure düğüm şablonu seçin (varsayılan olarak adlandırılan **varsayılan AzureNode şablonu**). Ardından belirtin **2** boyutu düğümlerinin **küçük**ve ardından **sonraki**.
   
    ![Düğüm belirtin][add_node2]
   
4. Üzerinde **Düğüm Ekleme Sihirbazı'nı tamamlama** sayfasında **son**.
    
     Adlı iki Azure düğümleri **AzureCN 0001** ve **AzureCN 0002**, artık HPC Kümesi Yöneticisi'nde görüntülenir. Hem bulunan **değil dağıtılan** durumu.
   
    ![Ek düğümler][add_node3]

## <a name="start-the-azure-nodes"></a>Azure düğümleri Başlat
Azure'da küme kaynaklarını kullanmak istediğinizde, HPC Küme Yöneticisi'ni (sağlayamaz) Azure düğümleri başlatmak ve çevrimiçi duruma getirmek için kullanın.

1. HPC Küme Yöneticisi'nde **düğüm Yönetimi** (adlı **kaynak yönetimi** HPC Pack'ın geçerli sürümlerinde) ve Azure düğümleri seçin.

2. Tıklayın **Başlat**ve ardından **Tamam**.
   
   ![Başlangıç düğümleri][add_node4]
   
    Düğümler için geçiş **sağlama** durumu. Sağlama ilerleme durumunu izlemek için sağlama günlüğünü görüntüleyin.
   
    ![Sağlama düğümleri][add_node6]

3. Birkaç dakika sonra Azure düğümleri son sağlama ve bulundukları **çevrimdışı** durumu. Bu durumda, rol örneklerini çalışıyor, ancak küme işleri henüz kabul edilemiyor.

4. Rol örnekleri, Azure portalında çalıştığını onaylamak için tıklatın **bulut Hizmetleri (Klasik)** > *your_cloud_service_name*.
   
   İki görmelisiniz **HpcWorkerRole** Service'te çalışan örnekleri (düğümler). HPC Pack ayrıca otomatik olarak dağıtan iki **HpcProxy** baş düğüm ve Azure arasındaki iletişimi işlemek üzere (boyut Orta) örnek.

   ![Örnekleri çalıştırma][view_instances1]

5. Küme işlerini çalıştırmak için Azure düğümleri çevrimiçi duruma getirmek için düğümler, sağ tıklayın ve ardından seçin **çevrimiçine**.
   
    ![Çevrimdışı düğümleri][add_node7]
   
    HPC Küme Yöneticisi'ni gösteren düğümleri olduğunu **çevrimiçi** durumu.

## <a name="run-a-command-across-the-cluster"></a>Küme üzerinde bir komut çalıştırın
Yükleme denetlemek için HPC Pack kullanın **clusrun** bir komut veya uygulama bir veya daha fazla küme düğümlerinde çalıştırılacak komut. Basit bir örnek olarak kullanın **clusrun** Azure düğümleri IP yapılandırması alınamıyor.

1. Baş düğümde bir yönetici olarak bir komut istemi açın.

2. Aşağıdaki komutu yazın:
   
    `clusrun /nodes:azurecn* ipconfig`

3. İstenirse, Küme Yöneticisi parolanızı girin. Komut çıktısı aşağıdakine benzer görmeniz gerekir.
   
    ![Clusrun][clusrun1]

## <a name="run-a-test-job"></a>Test işi çalıştırma
Şimdi hibrit kümede çalışan bir test işi gönderin. Bu örnek, bir basit parametreli Süpürme işi (doğası gereği Paralel hesaplama türü) içindir. Bu örnek, tamsayı kendisine kullanarak ekleyin. alt görevlere çalıştırılır **/a ayarlamak** komutu. Kümedeki tüm düğümler için 1'den 100'e tamamlama görevleri tamsayılar için katkıda bulunur.

1. HPC Küme Yöneticisi'nde **iş yönetimi** > **yeni parametrik tarama işi**.
   
    ![Yeni Proje][test_job1]

2. İçinde **yeni parametrik tarama işi** iletişim kutusundaki **komut satırı**, türü `set /a *+*` (varsayılan komut satırında görüntülenen üzerine). Kalan ayarlar için varsayılan değerleri bırakın ve ardından **Gönder** işi göndermek için.
   
    ![Parametreli Süpürme][param_sweep1]

3. İş tamamlandığında, çift **My tarama görev** işi.

4. Tıklayın **görünümü görevleri**ve o alt görevin hesaplanan çıkışı görüntülemek için bir alt görevin'ye tıklayın.
   
    ![Görev sonuçları][view_job361]

5. Hangi düğümün hesaplama için bu alt görevin gerçekleştirilen görmek için tıklayın **ayrılan düğümler**. (Kümenizi farklı düğüm adını gösterebilir.)
   
    ![Görev sonuçları][view_job362]

## <a name="stop-the-azure-nodes"></a>Azure düğümleri Durdur
Küme çalıştıktan sonra hesabınıza gereksiz ücretlerden kaçınmak için Azure düğümleri durdurun. Bu adım, bulut hizmeti durdurulur ve Azure rol örneklerini kaldırır.

1. HPC Küme Yöneticisi ' nde, **düğüm Yönetimi** (adlı **kaynak yönetimi** HPC Pack önceki sürümlerinde), hem Azure düğümleri seçin. ' A tıklayarak **Durdur**.
   
    ![Düğümleri Durdur][stop_node1]

2. İçinde **durdurma Windows Azure düğümleri** iletişim kutusu, tıklayın **Durdur**. 
   
3. Düğümler için geçiş **durdurma** durumu. Birkaç dakika sonra HPC Küme Yöneticisi'ni düğümleri gösterir **değil dağıtılan**.
   
    ![Dağıtılmadı düğümleri][stop_node4]

4. Azure portalında, Azure'da çalışan rolü örnekleri artık olduğunu onaylamak için tıklayın **bulut Hizmetleri (Klasik)** > *your_cloud_service_name*. Hiçbir örneği üretim ortamında dağıtılır. 
   
    Bu öğreticiyi tamamlar.

## <a name="next-steps"></a>Sonraki adımlar
* Belgeler için keşfetmek [HPC Pack](https://technet.microsoft.com/library/cc514029).
* HPC Pack Küme dağıtımı daha büyük ölçekte karma ayarlamak için bkz: [Microsoft HPC Pack ile Azure çalışan rolü örneklerine veri bloğu](https://go.microsoft.com/fwlink/p/?LinkID=200493).
* Azure Resource Manager şablonları kullanarak Azure'da bir HPC Pack kümesi oluşturmak diğer yolları için de dahil olmak üzere bkz [azure'de Microsoft HPC Pack ile HPC küme seçenekleri](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


[Overview]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/hybrid_cluster_overview.png
[install_hpc1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc1.png
[install_hpc4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc4.png
[install_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc6.png
[install_hpc7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc7.png
[install_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc10.png
[config_hpc2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc2.png
[config_hpc3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc3.png
[config_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc6.png
[config_hpc8]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc8.png
[config_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc10.png
[config_hpc12]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc12.png
[config_hpc13]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc13.png
[add_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1.png
[add_node1_1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1_1.png
[add_node2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node2.png
[add_node3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node3.png
[add_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node4.png
[add_node6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node6.png
[add_node7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node7.png
[view_instances1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances1.png
[clusrun1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/clusrun1.png
[test_job1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/test_job1.png
[param_sweep1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/param_sweep1.png
[view_job361]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job361.png
[view_job362]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job362.png
[stop_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node1.png
[stop_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node4.png
[view_instances2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances2.png
