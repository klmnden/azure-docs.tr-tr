---
title: "Azure karma HPC Pack kümedeki ayarlamak | Microsoft Docs"
description: "Küçük bir, karma yüksek performanslı bilgi işlem (HPC) küme ayarlamak için Microsoft HPC Pack ve Azure kullanmayı öğrenin"
services: cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: hpc-pack
ms.assetid: 
ms.service: cloud-services
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: danlep
ms.openlocfilehash: f6dc9657e64160be1e68a7356863b53131e9b3c3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="set-up-a-hybrid-high-performance-computing-hpc-cluster-with-microsoft-hpc-pack-and-on-demand-azure-compute-nodes"></a>Bir karma yüksek performanslı bilgi işlem (HPC) küme Microsoft HPC Pack ve isteğe bağlı Azure işlem düğümleri ile ayarlama
Microsoft HPC Pack 2012 R2 ve Azure (HPC) küme bir küçük, karma yüksek performanslı bilgi işlem ayarlamak için kullanın. Bu makalede gösterilen küme bir şirket içi HPC Pack baş düğümüne oluşur ve bazı düğümler isteğe bağlı bir azure'da dağıtmak bulut hizmeti işlem. Karma küme üzerinde işlem işleri sonra çalıştırabilirsiniz.

![Karma HPC Kümesi][Overview] 

Bu öğretici bir yaklaşım, küme "bulut için veri bloğu" olarak da adlandırılan gösterir yoğun uygulamaları çalıştırmak için ölçeklenebilir, isteğe bağlı Azure kaynaklarını kullanmak için.

Bu öğretici hesaplama kümeleri veya HPC paketi ile konusunda deneyim varsayar. Yalnızca tanıtım amacıyla hızla karma işlem kümesi dağıtmanıza yardımcı olmak için tasarlanmıştır. Değerlendirmeler ve adımlar bir karma HPC Pack devretme büyük ölçekli bir üretim ortamında dağıtmak veya HPC Pack 2016 kullanmak için bkz: [ayrıntılı yönergeler](http://go.microsoft.com/fwlink/p/?LinkID=200493). HPC paketi ile diğer senaryolar için Azure sanal makineleri küme dağıtımında otomatik dahil olmak üzere, bkz: [azure'da Microsoft HPC paketi ile HPC küme seçenekleri](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="prerequisites"></a>Ön koşullar
* **Azure aboneliği** -bir Azure aboneliğiniz yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/) yalnızca birkaç dakika içinde.
* **Windows Server 2012 R2 veya Windows Server 2012 çalıştıran bir şirket içi bilgisayar** -HPC küme baş düğümü olarak bu bilgisayarı kullanın. Windows Server zaten çalıştırmıyor değilse, yükleme indirin ve bir [Değerlendirme sürümü](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2).
  
  * Bilgisayar bir Active Directory etki alanına katılması gerekir. Test amaçları için baş düğümü bilgisayarının bir etki alanı denetleyicisi olarak yapılandırabilirsiniz. Baş düğüm bilgisayarı bir etki alanı denetleyicisi olarak Yükselt ve Active Directory etki alanı Hizmetleri sunucu rolü eklemek için Windows Server belgelerine bakın.
  * HPC Pack desteklemek için işletim sisteminin bu dillerden birinde yüklenmelidir: İngilizce, Japonca veya Çince (Basitleştirilmiş).
  * Önemli ve kritik güncelleştirmelerin yüklü olduğunu doğrulayın.
* **HPC Pack 2012 R2** - [karşıdan](http://go.microsoft.com/fwlink/p/?linkid=328024) yükleme paketini en son sürüm için ücretsiz ve dosyaları baş düğüm bilgisayara kopyalayın. Windows Server yüklemenizin aynı dilde yükleme dosyalarını seçin.

    >[!NOTE]
    > HPC Pack 2016 HPC Pack 2012 R2 yerine kullanmak istediğiniz ek yapılandırma gerekli değildir. Bkz: [ayrıntılı yönergeler](http://go.microsoft.com/fwlink/p/?LinkID=200493).
    > 
* **Etki alanı hesabı** -bu hesap HPC paketini yüklemek için yerel yönetici izinleri ile baş düğüm yapılandırılması gerekir.
* **TCP bağlantı noktası 443 üzerinden** Azure baş düğümünden.

## <a name="install-hpc-pack-on-the-head-node"></a>HPC Pack baş düğümüne yükleyin
Microsoft HPC Pack ilk Windows Server çalıştıran şirket içi bilgisayarınıza yükleyin. Bu bilgisayar kümenin baş düğümüne haline gelir.

1. Baş düğümünde yerel yönetici izinlerine sahip bir etki alanı hesabı kullanarak oturum açın.

2. HPC Pack yükleme dosyalarından Setup.exe çalıştırarak HPC Pack Yükleme Sihirbazı'nı başlatın.

3. Üzerinde **HPC Pack 2012 R2 Kurulum** ekranında **yeni yükleme veya mevcut bir yüklemeye yeni özelliklere**.

    ![HPC Pack 2012 Kurulumu][install_hpc1]

4. Üzerinde **Microsoft yazılım kullanıcı Sözleşmesi sayfası**, tıklatın **sonraki**.

5. Üzerinde **yükleme türünü seç** sayfasında, **bir baş düğüm oluşturarak yeni bir HPC kümesi oluşturma**ve ardından **sonraki**.

6. Sihirbazı çeşitli ön yükleme testleri çalıştırır. Tıklatın **sonraki** üzerinde **yükleme kuralları** tüm testleri geçerse sayfa. Aksi takdirde, sağlanan bilgileri gözden geçirin ve ortamınızda gerekli değişiklikleri yapın. Testleri yeniden çalıştırın veya gerekiyorsa Yükleme Sihirbazı'nı yeniden başlatın.
7. Üzerinde **HPC DB yapılandırma** sayfasında, emin olun **baş düğüm** tüm HPC veritabanları için seçilir ve ardından **sonraki**. 

    ![DB yapılandırma][install_hpc4]

8. Sihirbazın sonraki sayfalarındaki varsayılan seçimleri kabul edin. Üzerinde **gerekli bileşenleri yüklemek** sayfasında, **yükleme**.
   
    ![Yükleme][install_hpc6]

9. Yükleme tamamlandıktan sonra işaretini **HPC Küme Yöneticisi'ni başlatın** ve ardından **son**. (Bir sonraki adımda HPC Küme Yöneticisi'ni başlatın.)
   
    ![Son][install_hpc7]

## <a name="prepare-the-azure-subscription"></a>Azure aboneliği hazırlama
Aşağıdaki adımlarda gerçekleştirmek [Azure portal](https://portal.azure.com) Azure aboneliğinizle. Bu adımları tamamladıktan sonra şirket içi baş düğümünden Azure düğümleri dağıtabilirsiniz. 
  
  > [!NOTE]
  > Ayrıca, daha sonra ihtiyacınız Azure abonelik Kimliğinizi not edin. Kimliğinde Bul **abonelikleri** Portalı'nda.
  > 

### <a name="upload-the-default-management-certificate"></a>Varsayılan yönetim sertifikasını karşıya yükle
HPC Pack kendinden imzalı bir sertifika bir Azure yönetim sertifikası olarak yükleyebileceğiniz varsayılan Microsoft HPC Azure yönetim sertifikası adlı baş düğümüne yükler. Bu sertifika için sağlanan baş düğüm ile Azure arasındaki bağlantının güvenli hale getirmek için test etme ve kavram kanıtı dağıtımları.

1. Baş düğüm bilgisayardan oturum [Azure portal](https://portal.azure.com).

2. Tıklatın **abonelikleri** > *your_subscription_name*.

3. Tıklatın **yönetim sertifikaları** > **karşıya**.4. Baş düğüm dosyası C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer için göz atın. Ardından **karşıya**.

   
**HPC Azure Management'ı varsayılan** sertifika yönetim sertifikaları listesinde görünür.

### <a name="create-an-azure-cloud-service"></a>Bir Azure bulut hizmeti oluşturma
> [!NOTE]
> En iyi performans için aynı coğrafi bölgede bulut hizmetini ve (sonraki adımda) depolama hesabı oluşturun.
> 
> 

1. Portalı'nda tıklatın **bulut hizmetlerini (Klasik)** > **+ Ekle**.

2.  Hizmet için bir DNS adı yazın, bir kaynak grubu ve bir konum seçin ve ardından **oluşturma**.


### <a name="create-an-azure-storage-account"></a>Azure Storage hesabı oluşturma
1. Portalı'nda tıklatın **depolama hesapları (Klasik)** > **+ Ekle**.

2. Hesap için bir ad yazın ve seçin **Klasik** dağıtım modeli.

3. Bir kaynak grubu ve bir konum seçin ve diğer ayarları varsayılan değerlerinde bırakın. Sonra **Oluştur**’a tıklayın.

## <a name="configure-the-head-node"></a>Baş düğüm yapılandırın
Azure düğümleri dağıtmak ve iş göndermek için HPC Küme Yöneticisi'ni kullanmak için önce bazı gerekli küme yapılandırma adımlarını uygulayın.

1. Baş düğümünde HPC Küme Yöneticisi'ni başlatın. Varsa **baş düğüm Seç** iletişim kutusu görüntülenirse, tıklatın **yerel bilgisayar**. **Dağıtım Yapılacaklar listesi** görüntülenir.

2. Altında **gerekli dağıtım görevlerini**, tıklatın **ağınızı yapılandırmak**.
   
    ![Ağ yapılandırma][config_hpc2]

3. Ağ Yapılandırma Sihirbazı'nda seçin **yalnızca bir kurumsal ağ üzerindeki tüm düğümleri** (topoloji 5). Bu ağ yapılandırması tanıtım amacıyla en kolayıdır.
   
    ![Topoloji 5][config_hpc3]
   
4. Tıklatın **sonraki** Sihirbazı'nın kalan sayfalarındaki varsayılan değerleri kabul etmek için. Ardından **gözden geçirme** sekmesini tıklatın, **yapılandırma** ağ yapılandırmasını tamamlamak için.

5. İçinde **dağıtım Yapılacaklar listesi**, tıklatın **yükleme kimlik bilgileri sağlamak**.

6. İçinde **yükleme kimlik bilgileri** iletişim kutusunda, HPC paketi yüklemek için kullanılan etki alanı hesabının kimlik bilgilerini yazın. Daha sonra, **Tamam**'a tıklayın. 
   
    ![Yükleme kimlik bilgileri][config_hpc6]
   
7. İçinde **dağıtım Yapılacaklar listesi**, tıklatın **adlandırma yeni düğümleri yapılandırmak**.

8. İçinde **belirtin düğümü adlandırma serisi** iletişim kutusunda, adlandırma serisi ve'ı tıklatın varsayılanı kabul **Tamam**. Bu öğreticide eklemek Azure düğümleri otomatik olarak adlandırılmış olsa da bu adımı tamamlayın.
   
    ![Düğüm adlandırma][config_hpc8]
   
9. İçinde **dağıtım Yapılacaklar listesi**, tıklatın **düğüm şablonu oluşturma**. Öğreticide daha sonra düğüm şablonu kümesine Azure düğümleri eklemek için kullanın.

10. Oluşturma düğüm Şablonu Sihirbazı'nda, aşağıdakileri yapın:
    
    a. Üzerinde **düğümü şablon türünü seç** sayfasında, **Windows Azure düğüm şablonu**ve ardından **sonraki**.
    
    ![Düğüm şablonu][config_hpc10]
    
    b. Tıklatın **sonraki** varsayılan şablonu adını kabul etmek için.
    
    c. Üzerinde **abonelik bilgilerini gir** sayfasında, Azure abonelik Kimliğinizi (Azure hesap bilgilerinizi kullanılabilir) girin. Ardından **yönetim sertifikası**, göz **varsayılan Microsoft HPC Azure yönetimi.** Ardından **İleri**'ye tıklayın.
    
    ![Düğüm şablonu][config_hpc12]
    
    d. Üzerinde **hizmet bilgileri sağlayan** sayfasında, bulut hizmeti ve bir önceki adımda oluşturduğunuz depolama hesabı seçin. Ardından **İleri**'ye tıklayın.
    
    ![Düğüm şablonu][config_hpc13]
    
    e. Tıklatın **sonraki** Sihirbazı'nın kalan sayfalarındaki varsayılan değerleri kabul etmek için. Ardından **gözden geçirme** sekmesini tıklatın, **oluşturma** düğüm şablonu oluşturmak için.
    
    > [!NOTE]
    > Varsayılan olarak, Azure düğüm şablonu HPC Küme Yöneticisi'ni kullanarak (sağlayamaz) başlatmanızı ve düğümler el ile durdurun ayarlarını içerir. İsteğe bağlı olarak Azure düğümleri otomatik olarak durdurmak ve başlatmak için bir zamanlama yapılandırabilir.
    > 
    > 

## <a name="add-azure-nodes-to-the-cluster"></a>Azure düğümleri kümeye ekleme
Şimdi düğüm şablonu kümesine Azure düğümleri eklemek için kullanın. Kümenin düğümleri eklemek depolar, yapılandırma bilgilerini (sağlayamaz) başlayabilmeniz için bulut hizmetindeki herhangi bir zamanda bunları. Örnekler bulut hizmetinde çalışan sonra aboneliğinizi yalnızca Azure düğümleri için sizden ücret.

İki küçük düğümleri eklemek için aşağıdaki adımları izleyin.

1. HPC Küme Yöneticisi'nde **düğüm Yönetimi** (adlı **kaynak yönetimi** HPC Pack geçerli sürümlerinde) > **düğüm Ekle**.
   
    ![Düğüm Ekle][add_node1]

2. Düğüm Ekleme Sihirbazı içinde üzerinde **dağıtım yöntemini seçin** sayfasında, **Ekle Windows Azure düğümleri**ve ardından **sonraki**.
   
    ![Azure düğüm Ekle][add_node1_1]

3. Üzerinde **belirtin yeni düğümler** sayfasında, daha önce oluşturduğunuz Azure düğüm şablonu seçin (varsayılan olarak adlandırılan **varsayılan AzureNode şablonu**). Ardından belirtin **2** boyutu düğümlerinin **küçük**ve ardından **sonraki**.
   
    ![Düğüm belirtin][add_node2]
   
4. Üzerinde **Düğüm Ekleme Sihirbazı'nı tamamlama** sayfasında, **son**.
    
     Adlı iki Azure düğümleri **AzureCN 0001** ve **AzureCN 0002**, artık HPC Küme Yöneticisi'nde görüntülenir. Her ikisi de bulunan **değil dağıtılan** durumu.
   
    ![Eklenen düğümlerine][add_node3]

## <a name="start-the-azure-nodes"></a>Azure düğümleri Başlat
Azure'da küme kaynaklarını kullanmak istediğinizde (sağlayamaz) Azure düğümleri başlatmak ve çevrimiçi duruma getirmek için HPC Küme Yöneticisi'ni kullanın.

1. HPC Küme Yöneticisi'nde **düğüm Yönetimi** (adlı **kaynak yönetimi** HPC Pack geçerli sürümlerinde) ve Azure düğümleri seçin.

2. Tıklatın **Başlat**ve ardından **Tamam**.
   
   ![Düğümleri Başlat][add_node4]
   
    Düğümler için geçiş **sağlama** durumu. Sağlama ilerleme durumunu izlemek için sağlama günlüğünü görüntüleyin.
   
    ![Sağlama düğümler][add_node6]

3. Birkaç dakika sonra Azure düğümleri sağlama tamamladığınızda ve içinde **çevrimdışı** durumu. Bu durumda, rolü örneklerini çalıştıran ancak henüz küme işlerini kabul edemez.

4. Rol örnekleri, Azure portalında çalıştığını doğrulamak için tıklatın **bulut Hizmetleri (Klasik)** > *your_cloud_service_name*.
   
   İki görmelisiniz **HpcWorkerRole** Hizmeti'nde çalışan örnekleri (düğümler). HPC Pack ayrıca otomatik olarak dağıtan iki **HpcProxy** baş düğüm ile Azure arasındaki iletişimi işlemek için örnekleri (boyutu Orta).

   ![Çalışan örnekleri][view_instances1]

5. Küme işlerini çalıştırmak için Azure düğümleri çevrimiçi duruma getirmek için düğüm, sağ tıklatın ve ardından seçin **çevrimiçine**.
   
    ![Çevrimdışı düğümler][add_node7]
   
    HPC Küme Yöneticisi'ni gösterir düğümleri olduğundan **çevrimiçi** durumu.

## <a name="run-a-command-across-the-cluster"></a>Küme üzerinde bir komutu çalıştırın
Yükleme denetimi için HPC Pack kullanın **clusrun** bir komutu veya uygulama bir veya daha fazla küme düğümleri üzerinde çalıştırmak için komutu. Basit bir örnek olarak kullanın **clusrun** Azure düğümleri IP yapılandırması alınamıyor.

1. Baş düğümünde yönetici olarak bir komut istemi açın.

2. Aşağıdaki komutu yazın:
   
    `clusrun /nodes:azurecn* ipconfig`

3. İstenirse, Küme Yönetici parolanızı girin. Komut çıktısı şuna benzer görmeniz gerekir.
   
    ![Clusrun][clusrun1]

## <a name="run-a-test-job"></a>Bir test işini çalıştır
Şimdi karma kümede çalışan bir test işi gönderin. Bu, bir basit parametrik tarama işi (doğası gereği Paralel hesaplama türü) örneğidir. Bu örnek bir tamsayı kendisine kullanarak eklemeyi görevleri çalıştırır **/a ayarlamak** komutu. Kümedeki tüm düğümler için 1'den 100'e tamamlama tamsayılar görevleri katkıda.

1. HPC Küme Yöneticisi'nde **iş yönetimi** > **yeni parametrik Sweep işi**.
   
    ![Yeni Proje][test_job1]

2. İçinde **yeni parametrik Sweep işi** iletişim kutusunda **komut satırı**, türü `set /a *+*` (görünür varsayılan komut satırı üzerine). Diğer ayarlar için varsayılan değerleri bırakın ve ardından **gönderme** işi göndermek için.
   
    ![Parametrik tarama][param_sweep1]

3. İş tamamlandığında, çift **My Sweep görev** iş.

4. Tıklatın **görünümü görevleri**ve o alt hesaplanan çıktısını görüntülemek için bir alt'ı tıklatın.
   
    ![Görev sonuçları][view_job361]

5. Hangi düğümün hesaplama için bu alt gerçekleştirilen görmek için tıklatın **ayrılan düğümler**. (Kümeniz farklı bir düğüme adını gösterebilir.)
   
    ![Görev sonuçları][view_job362]

## <a name="stop-the-azure-nodes"></a>Azure düğümleri Durdur
Küme çalıştıktan sonra hesabınıza gereksiz ücret yazılmaması için Azure düğümleri durdurun. Bu adım bulut hizmetini durdurur ve Azure rol örneklerini kaldırır.

1. HPC Küme Yöneticisi ' nde, **düğüm Yönetimi** (adlı **kaynak yönetimi** HPC Pack önceki sürümlerinde), hem Azure düğümleri seçin. Ardından **durdurmak**.
   
    ![Düğümler durdurun][stop_node1]

2. İçinde **durdurmak Windows Azure düğümleri** iletişim kutusu, tıklatın **durdurmak**. 
   
3. Düğümler için geçiş **durdurma** durumu. Birkaç dakika sonra HPC Küme Yöneticisi'ni düğümlerin olduğunu gösterir **değil dağıtılan**.
   
    ![Dağıtılmadı düğümler][stop_node4]

4. Azure portalında, Azure'da çalışan rolü örnekleri artık olduğunu doğrulamak için tıklatın **bulut hizmetlerini (Klasik)** > *your_cloud_service_name*. Hiçbir örneği üretim ortamında dağıtılır. 
   
    Bu öğreticiyi tamamlar.

## <a name="next-steps"></a>Sonraki adımlar
* Belgelerine keşfedin [HPC Pack](https://technet.microsoft.com/library/cc514029).
* Karma bir HPC paketi Küme dağıtımı büyük ölçekli ayarlamak için bkz: [Microsoft HPC paketi ile Azure çalışan rolü örneklerine veri bloğu](http://go.microsoft.com/fwlink/p/?LinkID=200493).
* Azure Resource Manager şablonları kullanarak Azure'da bir HPC paketi küme oluşturmak diğer yolları için de dahil olmak üzere bkz [azure'da Microsoft HPC paketi ile HPC küme seçenekleri](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


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
