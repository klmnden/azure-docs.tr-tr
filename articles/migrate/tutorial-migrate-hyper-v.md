---
title: Şirket içi Hyper-V Vm'lerini Azure ile Azure'a geçiş sunucusu geçişi geçirme | Microsoft Docs
description: Bu makalede şirket içi Hyper-V Vm'lerini Azure geçişi sunucusu geçişi ile azure'a geçirme
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/09/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 0d4ab3225501c2573b07f27e40a8f42508a4df88
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809543"
---
# <a name="migrate-hyper-v-vms-to-azure"></a>Hyper-V VM’lerini Azure’a Geçirin 

Bu makalede Azure geçişi Server Geçiş Aracı ile aracısız geçiş kullanarak şirket içi Hyper-V Vm'lerini Azure'a geçirme gösterilmektedir.

[Azure geçişi](migrate-services-overview.md) bulma, değerlendirme ve şirket içi uygulamalarınızı ve iş yükleri ve private/public bulut Vm'lerinden azure'a geçişini izlemek için merkezi bir nokta sağlar. Hub araçları Azure geçişi, değerlendirme ve geçiş, ek olarak üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri sağlar.

Değerlendirmek ve Hyper-V Azure geçişi Server değerlendirme ve geçiş kullanarak Azure'a geçirme gösterilmektedir, serideki üçüncü öğreticidir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:


> [!div class="checklist"]
> * Azure ve şirket içi Hyper-V ortamınızı hazırlama
> * Kaynak ortamı ayarlamak ve çoğaltma Gereci dağıtabilirsiniz.
> * Hedefi environmen Ayarla...
> * Çoğaltmayı etkinleştirin.
> * Her şeyin beklendiği gibi çalıştığından emin olmak için geçiş testi çalıştırma.
> * Çalıştıran bir Azure tam geçişi.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.


## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce karşılamanız gereken ön koşullar şunlardır:

1. [Gözden geçirme](migrate-architecture.md) Hyper-V geçiş mimarisi.
2. [İlk öğreticiyi Tamamla](tutorial-prepare-hyper-v.md) geçiş için Azure ve Hyper-V ayarlamak için bu bölümdeki. İlk öğreticide:
    - [Azure'u hazırlama](tutorial-prepare-hyper-v.md#prepare-azure) geçiş.
    - [Şirket içi ortamı hazırlama](tutorial-prepare-hyper-v.md#prepare-for-hyper-v-migration) geçiş.
3. Hyper-V Vm'leri, Azure geçişi Server değerlendirmesi, geçiş yapmadan önce kullanarak değerlendirme deneyin öneririz bunları azure'a. Bunu yapmak için [ikinci öğreticiye](tutorial-assess-hyper-v.md) bu seri. Bir değerlendirmeyi deneyin öneririz, ancak sanal makineleri geçirmeden önce bir değerlendirme çalıştırın gerekmez.
4. İzinlerine sahip olduğunu Azure hesabınızı kullanarak sanal makine Katılımcısı rolü atanmış olduğundan emin olun:

    - Seçilen kaynak grubunda sanal makine oluşturma.
    - Seçilen sanal ağda sanal makine oluşturma.
    - Yazma için bir Azure yönetilen disk.   
5. [Bir Azure ağı ayarlama](../virtual-network/manage-virtual-network.md#create-a-virtual-network). Azure'a geçiş yaptığınızda, oluşturulan Azure Vm'lerinin geçişi belirlediğiniz bir Azure ağına katılır.


## <a name="add-the-azure-migrate-server-migration-tool"></a>Azure geçişi Server Geçiş Aracı ekleme

Hyper-V sanal makineleri değerlendirme için ikinci bir öğreticiyi izleyin yaramadı için gerekirse [bu yönergeleri izleyin](how-to-add-tool-first-time.md) Azure geçişi projesini ayarlarsınız ve Azure geçişi Server Geçiş Aracı projeye ekleyin.

İkinci öğreticide izleyen ve bir Azure geçişi projesi Kurulum zaten varsa, Azure geçişi Server Geçiş Aracı aşağıdaki şekilde ekleyin:

1. Azure geçişi projesinde tıklayın **genel bakış**. 
2. İçinde **keşfedin, değerlendirin ve geçiş sunucuları**, tıklayın **değerlendirin ve sunucularını geçirme**.
3. İçinde **Geçiş Araçları**seçin **geçirmeye hazır olduğunuzda, geçiş aracı eklemek için burayı tıklatın**.

    ![Araç seçin](./media/tutorial-migrate-hyper-v/select-migration-tool.png)

4. Araçlar listesinden seçin **Azure geçişi: Sunucu geçiş** > **Ekle aracı**

    ![Sunucu geçiş aracı](./media/tutorial-migrate-hyper-v/server-migration-tool.png)


## <a name="set-up-the-azure-migrate-appliance"></a>Azure geçişi gerecini ayarlamak

Azure geçişi sunucusu geçişi, basit bir Hyper-V VM Gereci çalıştırır.

- Gereç, VM bulma işlemini gerçekleştirir ve Azure geçişi sunucusu geçişi için VM meta verileri ve performans verilerini gönderir.
- Aynı gereç, Azure geçişi Server değerlendirmesi aracı tarafından da kullanılır.

Gerecini ayarlamak için:
- Hyper-V sanal makineleri değerlendirme için ikinci öğreticide izlediyseniz, Bu öğretici sırasında zaten gereç ayarlayın.
- Bu öğreticiyi izleyin alamadık, gerecini ayarlamak artık gerekir. Bunu yapmak için: 

    - Sıkıştırılmış bir Hyper-V VHD Azure portalından indirin.
    - Aleti oluşturmak ve Azure geçişi Server değerlendirmesi için bağlanıp bağlanamadığını denetleyin. 
    - İlk kez Gereci yapılandırın ve Azure geçişi projesi ile kaydedin.

    Bölümündeki yönergeleri [bu makalede](how-to-set-up-appliance-hyper-v.md) gerecini ayarlamak için.

## <a name="prepare-hyper-v-hosts"></a>Hyper-V konaklarını hazırlama

1. Azure geçişi projesinde > **sunucuları**, **Azure geçişi: Sunucu geçiş**, tıklayın **bulma**.
2. İçinde **makineleri keşfet** > **makineleriniz sanallaştırıldı mı?** seçin **Evet, Hyper-V ile**.
3. İçinde **hedef bölge**, makineleri geçirmek istediğiniz Azure bölgesini seçin.
6. Seçin **geçiş için hedef bölgede bölge adı olduğundan emin olun**.
7. Tıklayın **kaynakları oluşturma**. Bu, arka planda bir Azure Site Recovery kasası oluşturur.
    - Azure geçişi sunucusu geçişi ile geçiş'kurmak zaten ayarını etkinleştirdiyseniz, kaynakları önceden ayarlanmış olduğundan bu seçenek görünmez.
    - Bu düğmesine tıklandıktan sonra bu proje için hedef bölgeyi değiştiremezsiniz.
    - Sonraki tüm geçişler bu bölgeye ' dir.
    
8. İçinde **hazırlama Hyper-V konak sunucuları**, Hyper-V çoğaltma sağlayıcısı ve kayıt anahtarı dosyasını indirin.
    - Kayıt anahtarı, Azure geçişi sunucu geçiş ile Hyper-V konağı kaydetmek için gereklidir.
    - Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

    ![Sağlayıcı ve anahtarla indirin](./media/tutorial-migrate-hyper-v/download-provider-hyper-v.png)

4. Sağlayıcı kurulum dosyasını ve kayıt defteri anahtar dosyası, her Hyper-V Konağı (veya küme düğümü) çoğaltmak istediğiniz Vm'leri çalıştıran kopyalayın.
5. Sağlayıcı kurulum dosyasını her bir ana bilgisayarda bir sonraki yordamda açıklandığı gibi çalıştırın.
6. İçinde konaklarda sağlayıcı yükledikten sonra **makineleri keşfet**, tıklayın **kayıt Sonlandır**.

    ![Kayıt Sonlandır](./media/tutorial-migrate-hyper-v/finalize-registration.png)

Bu, Azure geçişi sunucusu geçişi içinde bulunan VM'ler görünene kadar kayıt sonlandırılıyor sonra 15 dakika sürebilir. VM'ler bulunduktan gibi **bulunan sunucuları** sayısı yükseldiğinde.

![Bulunan sunucuları](./media/tutorial-migrate-hyper-v/discovered-servers.png)

### <a name="register-hyper-v-hosts"></a>Hyper-V konakları kaydetme

Her ilgili Hyper-V konağında indirilen kurulum dosyasını (AzureSiteRecoveryProvider.exe) yükleyin.

1. Sağlayıcı kurulum dosyasını her bir konak veya küme düğümünde çalıştırın.
2. Sağlayıcı Kurulum sihirbazındaki > **Microsoft Update**, sağlayıcı güncelleştirmelerini denetlemek için Microsoft Update'i kullanmayı kabul et.
3. İçinde **yükleme**, sağlayıcı ve aracı için varsayılan yükleme konumunu kabul edin ve seçin **yükleme**.
4. Yüklemeden sonra Kayıt Sihirbazı'ndaki > **kasa ayarları**seçin **Gözat**hem de **anahtar dosyası**, indirdiğiniz kasa anahtarı dosyasını seçin.
5. İçinde **Proxy ayarlarını**, ana bilgisayarda çalışan Sağlayıcı'nın internet'e nasıl bağlandığını belirtin.
    - Gereç bir proxy sunucusu arkasında bulunuyorsa, Ara sunucu ayarlarını belirtmeniz gerekir.
    - Ara sunucu adı olarak belirtmeniz **http://ip-address** , veya **http://FQDN** . HTTPS Ara sunucular desteklenmez.
   

6. Sağlayıcı erişebildiğinden emin olun [gerekli URL'ler](migrate-support-matrix-hyper-v.md#migration-hyper-v-host-url-access).
7. İçinde **kayıt**, ana bilgisayar kaydedildikten sonra tıklayın **son**.

## <a name="replicate-hyper-v-vms"></a>Hyper-V Vm'lerini çoğaltma

Keşfin tamamlanma ile Hyper-V Vm'lerini azure'a çoğaltma başlayabilirsiniz.

1. Azure geçişi projesinde > **sunucuları**, **Azure geçişi: Sunucu geçiş**, tıklayın **çoğaltmak**.
2. İçinde **çoğaltmak**, > **kaynağı ayarları** > **makineleriniz sanallaştırıldı mı?** seçin **Evet, Hyper-V ile**. Ardından **sonraki: Sanal makineler**.
3. İçinde **sanal makineler**, çoğaltmak istediğiniz makineleri seçin.
    - VM'ler için bir değerlendirme çalıştırın, VM boyutu ve disk türü (premium veya standart) önerileri ve değerlendirme sonuçlarda uygulayabilirsiniz. Bunu yapmak için **Azure geçişi değerlendirmesi geçiş ayarlarını içeri aktarma?** seçin **Evet** seçeneği.
    - Değerlendirme çalışmadığı ya da seçin, değerlendirme ayarlarını kullanmak istemiyorsanız **Hayır** seçenekleri.
    - Değerlendirme kullanmayı seçtiyseniz, VM grubu ve değerlendirme adını seçin.

        ![Değerlendirme seçin](./media/tutorial-migrate-hyper-v/select-assessment.png)

4. İçinde **sanal makineler**gerektiğinde VM'ler için arama yapın ve geçirmek istediğiniz her sanal makine kontrol edin. ' A tıklayarak **sonraki: Ayarları hedef**.

    ![Sanal makineleri seçin](./media/tutorial-migrate-hyper-v/select-vms.png)

5. İçinde **hedef ayarları**, geçiş, aboneliği ve Azure Vm'lerinin geçişten sonra bulunacağı kaynak grubu hedef bölgeyi seçin.
7. İçinde **çoğaltma depolama hesabı**, Azure'da seçin çoğaltılan veriler Azure depolama hesabında depolanır.
8. **Sanal ağ**, Azure Vm'lerini katıldığı geçişten sonra Azure VNet/alt seçin.
9. İçinde **Azure hibrit avantajı**:

    - Seçin **Hayır** Azure hibrit avantajı uygulamak istemiyorsanız. Ardından **İleri**'ye tıklayın.
    - Seçin **Evet** ile etkin Yazılım Güvencesi'ni veya Windows Server abonelikleri kapsamındaki Windows Server makineleri sahip ve geçiş yaptığınız makinelere avantajı uygulamak istediğiniz. Ardından **İleri**'ye tıklayın.

    ![Hedef ayarları](./media/tutorial-migrate-hyper-v/target-settings.png)

10. İçinde **işlem**, VM adı, boyutu, işletim sistemi disk türü ve kullanılabilirlik kümesi gözden geçirin. Vm'leri uygun olmalıdır [Azure gereksinimleri](migrate-support-matrix-vmware.md#agentless-migration-vmware-vm-requirements).

    - **VM boyutu**: Değerlendirmesi önerileri kullanıyorsanız, VM boyutu açılan önerilen boyut içerir. Aksi takdirde Azure geçişi, Azure aboneliğinde en yakın eşleşme dayalı bir boyutu seçer. Alternatif olarak, el ile boyutundaki çekme **Azure VM boyutu**. 
    - **İşletim sistemi diski**: VM için işletim sistemi (önyükleme) diskini belirtin. İşletim sistemi diski işletim sistemi önyükleme yükleyicisi ve yükleyici sahip bir disktir. 
    - **Kullanılabilirlik kümesi**: Sanal makine geçişten sonra bir Azure kullanılabilirlik gerekiyorsa, kümesi belirtin. Küme, geçiş için belirttiğiniz hedef kaynak grubunda olmalıdır.

    ![VM işlem ayarları](./media/tutorial-migrate-hyper-v/compute-settings.png)

11. İçinde **diskleri**, VM diskleri Azure'a çoğaltılacak ve disk türünü (SSD/HDD için standart veya premium yönetilen diskler) Azure'da seçin olup olmadığını belirtin. Ardından **İleri**'ye tıklayın.
    - Diskleri çoğaltmadan hariç tutabilirsiniz.
    - Diskleri hariç tutarsanız, geçişten sonra Azure sanal makinesinde mevcut olmaz. 

    ![Diskler](./media/tutorial-migrate-hyper-v/disks.png)

10. İçinde **gözden geçirin ve çoğaltma başlangıç**, ayarları gözden geçirin ve tıklayın **çoğaltmak** sunucular için ilk çoğaltma başlatılamadı.

> [!NOTE]
> Çoğaltma ayarları çoğaltma başlar, buna önce dilediğiniz zaman güncelleştirebilirsiniz **Yönet** > **makineleri çoğaltma**. Çoğaltma başladıktan sonra ayarları değiştirilemez.

### <a name="provisioning-for-the-first-time"></a>İlk kez sağlama

Azure geçişi projesinde çoğaltma yapıyorsanız ilk VM varsa, Azure geçişi sunucu geçiş projesi olarak aynı kaynak grubunda bu kaynakları otomatik olarak sağlar.

- **Service bus**: Azure geçişi sunucusu geçişi, çoğaltma gerecine düzenleme iletileri göndermek için hizmet veri yolu kullanır.
- **Ağ geçidi depolama hesabı**: Sunucu geçiş ağ geçidi depolama hesabına çoğaltılan VM'ler hakkındaki durum bilgilerini depolamak için kullanır.
- **Kayıt depolama hesabı**: Azure geçişi Gereci çoğaltma günlükleri VM'ler için bir günlük depolama hesabına yükler. Azure geçişi, yönetilen çoğaltma diskleri çoğaltma bilgiler geçerlidir.
- **Anahtar kasası**: Azure geçişi gereç, hizmet veri yolu için bağlantı dizelerini ve Çoğaltmada kullanılan depolama hesapları için erişim anahtarlarını yönetmek için anahtar kasasını kullanır. Anahtar kasası, hazırlandığınızda depolama hesabına erişmek için gereken izinleri ayarlamanız gerekir. [Bu izinleri gözden](tutorial-prepare-vmware.md#assign-role-assignment-permissions).   


## <a name="track-and-monitor"></a>İzleme ve izleme


- Tıkladığınızda **çoğaltmak** Başlat çoğaltma işi başlar. 
- Başlangıç çoğaltma işlemi başarıyla tamamlandığında, makineleri azure'a, ilk çoğaltma başlar.
- İlk çoğaltma sonlandırıldıktan sonra değişim çoğaltması başlar. Şirket içi diskler artımlı değişiklikleri düzenli olarak Azure'a çoğaltılır.

İş durumunu portal bildirimlerinden izleyebilirsiniz.

Tıklayarak, çoğaltma durumunu izleyebilirsiniz **sunucuları çoğaltılması** içinde **Azure geçişi: Sunucu geçiş**.
![İzleyici çoğaltma](./media/tutorial-migrate-hyper-v/replicating-servers.png)



## <a name="run-a-test-migration"></a>Geçiş testi çalıştırma


Değişim çoğaltması başladığında Azure tam geçişi çalıştırmadan önce VM'ler için bir geçiş testi çalıştırabilirsiniz. Bu geçiş yapmadan önce en az bir kez her makine için bunu yapmanızı öneririz.

- Test geçişi, geçiş, şirket içi makineleri etkilemeden beklendiği gibi çalıştığından denetimleri çalıştırılıyor kalması ve çoğaltmaya devam edin. 
- Test geçişi, çoğaltılan veriler (genellikle, Azure aboneliğinizde bir üretim dışı sanal ağa geçirme) kullanarak Azure VM oluşturarak geçiş benzetimini yapar.
- Azure VM çoğaltılmış test kullanma geçişi doğrulamak için uygulama testi gerçekleştirin ve tam geçişten önce tüm sorunları giderin.

Test geçişi şu şekilde yapabilirsiniz:


1. İçinde **geçiş hedefleri** > **sunucuları** > **Azure geçişi: Sunucu geçiş**, tıklayın **Test geçişi sunucuları**.

     ![Geçirilen sunucularını test etme](./media/tutorial-migrate-hyper-v/test-migrated-servers.png)

2. Test ve VM'ye sağ **Test geçirme**.

    ![Test geçişi](./media/tutorial-migrate-hyper-v/test-migrate.png)

3. İçinde **geçiş testi**, hangi Azure VM konumlandırılacağı geçişten sonra Azure sanal ağı seçin. Üretim dışı VNet kullanmanızı öneririz.
4. **Test geçiş** işi başlatır. Portal bildirimleri işi izleyin.
5. Geçiş tamamlandıktan sonra geçirilen Azure sanal Makinesinde görüntüleyin **sanal makineler** Azure portalında. Bir sonek makineyi adına sahip **-Test**.
6. Test tamamlandıktan sonra Azure VM'nin sağ **makineleri çoğaltma**, tıklatıp **test geçişi temiz**.

    ![Geçişi Temizle](./media/tutorial-migrate-hyper-v/clean-up.png)


## <a name="migrate-vms"></a>VM’leri geçirme

Test geçiş beklendiği gibi çalıştığını doğruladıktan sonra şirket içi makineleri geçirebilirsiniz.

1. Azure geçişi projesinde > **sunucuları** > **Azure geçişi: Sunucu geçiş**, tıklayın **sunucuları çoğaltılması**.

    ![Çoğaltma sunucuları](./media/tutorial-migrate-hyper-v/replicate-servers.png)

2. İçinde **makineleri çoğaltma**, VM'ye sağ tıklayın > **geçirme**.
3. İçinde **geçirme** > **sanal makineleri kapatmak ve veri kaybı olmadan planlanan bir geçiş gerçekleştirmek**seçin **Evet** > **Tamam** .
    - Varsayılan olarak Azure geçişi, şirket içi VM'yi kapatır ve son çoğaltma gerçekleştikten oluşan VM değişiklikleri eşitlemek için bir isteğe bağlı çoğaltma çalışır. Bu, veri kaybı olmadan sağlar.
    - Sanal makineyi istemiyorsanız seçin **yok**
4. VM için bir geçiş işi başlatır. Azure bildirimleri işi izleyin.
5. İş tamamlandıktan sonra görüntüleyebilir ve VM'den yönetme **sanal makineler** sayfası.

## <a name="complete-the-migration"></a>Geçişi tamamlama

1. Geçiş yaptıktan sonra VM'ye sağ tıklayın > **Durdur geçiş**. Bu şirket içi makine için çoğaltma durdurulur ve sanal makine için çoğaltma durumu bilgilerini temizler.
2. Azure VM yükleme [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) veya [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) geçirilen makinelerdeki aracı.
3. Veritabanı bağlantısı dizelerini ve web sunucusu yapılandırmalarını güncelleştirme gibi herhangi bir geçiş sonrası uygulama ayarı gerçekleştirin.
4. Geçirilen uygulamada son uygulama ve geçiş kabul testi gerçekleştirme işlemi şimdi Azure’da çalıştırılmaktadır.
5. Geçirilen Azure sanal makine örneği üzerinden trafiğe keser.
6. Yerel sanal makine envanterinizden şirket içi sanal makineleri kaldırın.
7. Yerel yedeklemelerden şirket içi sanal makineleri kaldırın.
8. Azure sanal makinelerinin yeni konumunu ve IP adresini göstermek için herhangi bir iç belgeyi güncelleştirin. 

## <a name="post-migration-best-practices"></a>Geçiş sonrası en iyi uygulamalar

- Daha fazla esneklik için:
    - Azure Backup hizmetini kullanarak Azure sanal makinelerini yedekleyip verileri güvende tutun. [Daha fazla bilgi edinin](../backup/quick-backup-vm-portal.md).
    - Site Recovery ile Azure sanal makinelerini ikincil bölgeye çoğaltarak iş yüklerinin çalışmaya devam etmesini ve sürekli kullanılabilir olmasını sağlayın. [Daha fazla bilgi edinin](../site-recovery/azure-to-azure-tutorial-enable-replication.md).
- Daha fazla güvenlik için:
    - Kilitleme ve gelen trafik ile erişimini [Azure Güvenlik Merkezi - tam zamanında Yönetim](https://docs.microsoft.com/azure/security-center/security-center-just-in-time).
    - [Ağ Güvenlik Grupları](https://docs.microsoft.com/azure/virtual-network/security-overview) ile ağ trafiğini yönetim uç noktaları ile kısıtlayın.
    - [Azure Disk Şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview)’ni dağıtarak disklerin güvenliğinin sağlanmasına yardımcı olun ve verileri hırsızlık ve yetkisiz erişime karşı koruyun.
    - [IaaS kaynaklarının güvenliğini sağlama](https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/) hakkında daha fazla bilgi edinin ve [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)’ni ziyaret edin.
- İzleme ve yönetim için:
-  [Azure Maliyet Yönetimi](https://docs.microsoft.com/azure/cost-management/overview)’ni dağıtarak kaynak kullanımını ve harcamayı izleyin.


## <a name="next-steps"></a>Sonraki adımlar

Araştırma [bulut geçişi Yolculuğunuzun](https://docs.microsoft.com/azure/architecture/cloud-adoption/getting-started/migrate) Azure bulut benimseme çerçevesi içinde.
