---
title: Hyper-V Vm'lerini Azure geçişi ile Azure'a geçiş için değerlendirme | Microsoft Docs
description: Şirket içi Hyper-V Vm'lerini Azure Geçişi'ni kullanarak Azure'a geçiş için değerlendirme açıklar.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/11/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 83567a45980b29931f9b68bd6d60df0d427b09de
ms.sourcegitcommit: af31deded9b5836057e29b688b994b6c2890aa79
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67813012"
---
# <a name="assess-hyper-v-vms-with-azure-migrate-server-assessment"></a>Azure ile Hyper-V sanal makineleri değerlendirme Server değerlendirmesi geçirme

Bu makalede, Azure Geçişi'ni kullanarak şirket içi Hyper-V Vm'leri, değerlendirme işlemini göstermektedir: Sunucu değerlendirme aracı.

[Azure geçişi](migrate-services-overview.md) hub'ı keşfedin, değerlendirin ve uygulamalar ve altyapı iş yükleri Microsoft Azure'a geçirmek için yardımcı araçlar sağlar. Hub, Azure geçişi araçları ve üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri içerir.



Bu öğretici, değerlendirmek ve Hyper-V Vm'lerini Azure'a geçirme gösterilmektedir bir serinin ikinci bölümüdür. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir Azure geçişi projesi ayarlayın.
> * Ayarlama ve Azure geçişi gereçlerden biri kaydedin.
> * Şirket içi sanal makinelerin sürekli bulma başlatın.
> * Keşfedilen Vm'leri gruplar ve Grup değerlendirin.
> * Değerlendirme gözden geçirin.

> [!NOTE]
> Öğreticiler hızlı bir kavram kanıtı ayarlayabilirsiniz bir senaryo için en basit dağıtım yolu gösterir. Öğreticiler, mümkün olduğunda varsayılan seçenekleri kullanın ve tüm olası ayarları ve yol gösterme. Ayrıntılı yönergeler için nasıl yapılır makaleleri inceleyin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.


## <a name="prerequisites"></a>Önkoşullar

- [Tam](tutorial-prepare-hyper-v.md) bu serideki ilk öğreticide. Aksi takdirde, bu öğreticideki yönergeler işe yaramaz.
- İşte ne ilk öğreticide yaptığınız:
    - [Azure izinleri ayarlama](tutorial-prepare-hyper-v.md#prepare-azure) Azure geçişi için.
    - [Hyper-V'leri hazırlama](tutorial-prepare-hyper-v.md#prepare-for-hyper-v-assessment) kümeler, konakları ve sanal makineleri değerlendirme için.

## <a name="set-up-an-azure-migrate-project"></a>Azure geçişi projesi kurun

1. Azure portalında > **tüm hizmetleri**, arama **Azure geçişi**.
2. Arama sonuçlarında seçin **Azure geçişi**.
3. İçinde **genel bakış**altında **keşfedin, değerlendirin ve sunucularını geçirme**, tıklayın **değerlendirin ve sunucularını geçirme**.

    ![Bulma ve değerlendirme sunucuları](./media/tutorial-assess-hyper-v/assess-migrate.png)

4. İçinde **Başlarken**, tıklayın **ekleme Araçları**.
5. İçinde **geçiş projesi** sekmesinde, Azure aboneliğinizi seçin ve bir yoksa, bir kaynak grubu oluşturun.
6. İçinde **Project Details**, proje adı ve projeyi oluşturmak istediğiniz bölgeyi belirtin.


    ![Bir Azure geçişi projesi oluşturun](./media/tutorial-assess-hyper-v/migrate-project.png)

    Azure geçişi projesinde şu bölgelerde oluşturabilirsiniz.

    **Coğrafya** | **Bölge**
    --- | ---
    Asya  | Güneydoğu Asya
    Avrupa | Kuzey Avrupa veya Batı Avrupa
    Birleşik Krallık |  UK Güney veya UK Batı
    Amerika Birleşik Devletleri | Doğu ABD, Batı ABD 2 ve Batı Orta ABD

    - Proje bölge, yalnızca şirket içi Vm'lerden toplanan meta verileri depolamak için kullanılır.
    - Sanal makinelerin geçiş yaptığınızda, farklı Azure hedef bölge seçebilirsiniz. Geçiş hedef için tüm Azure bölgelerinde desteklenir.

7.           **İleri**'ye tıklayın.
8. İçinde **Select Değerlendirme Aracı**seçin **Azure geçişi: Server değerlendirmesi** > **sonraki**.

    ![Bir Azure geçişi projesi oluşturun](./media/tutorial-assess-hyper-v/assessment-tool.png)

9. İçinde **Select geçiş aracı**seçin **şimdilik geçiş aracı eklemeyi atlamak mı** > **sonraki**.
10. İçinde **gözden + araçları Ekle**, ayarları gözden geçirin ve tıklayın **ekleme Araçları**.
11. Azure geçişi projesi dağıtmak için birkaç dakika bekleyin. Proje sayfasına gidersiniz. Proje görmüyorsanız, buradan erişebilirsiniz **sunucuları** Azure geçişi panosunda.




## <a name="set-up-the-appliance-vm"></a>VM gerecini ayarlamak

Azure geçişi Server değerlendirmesi basit bir Hyper-V VM Gereci çalıştırır.

- Bu gereç, VM bulma işlemini gerçekleştirir ve Azure geçişi için VM meta verileri ve performans verilerini gönderir: Server değerlendirmesi.
- Ayarladığınız gerecini ayarlamak için:
    - Sıkıştırılmış bir Hyper-V VHD Azure portalından indirin.
    - Aleti oluşturmak ve Azure geçişi Server değerlendirmesi için bağlanıp bağlanamadığını denetleyin.
    - İlk kez Gereci yapılandırın ve Azure geçişi projesi ile kaydedin.

### <a name="download-the-vhd"></a>VHD'yi indirin

Gereç için daraltılmış VHD şablonunu indirin.

1. İçinde **geçiş hedefleri** > **sunucuları** > **Azure geçişi: Server değerlendirmesi**, tıklayın **bulma**.
2. İçinde **makineleri keşfet** > **makineleriniz sanallaştırıldı mı?** , tıklayın **Evet, Hyper-V ile**.
3. Tıklayın **indirme** VHD dosyasını indirmek için.

    ![VM indirme](./media/tutorial-assess-hyper-v/download-appliance-hyperv.png)


### <a name="verify-security"></a>Güvenlik doğrulayın

Daraltılmış dosyayı dağıtmadan önce güvenli olup olmadığını denetleyin.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. VHD için karma oluşturmak için aşağıdaki komutu çalıştırın
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek kullanım: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3.  Gereç sürüm 1.19.06.27 oluşturulan karma bu ayarlara uygun olmalıdır.

  **Algoritma** | **Karma değeri**
  --- | ---
  MD5 | 3681f745fa2b0a0a6910707d85161ec5
  SHA256 | e6ca109afab9657bdcfb291c343b3e3abced9a273d25273059171f9954d25832



### <a name="create-the-appliance-vm"></a>' % S'Gereci VM oluşturma

İndirilen dosyayı içeri aktarın ve VM oluşturun.

1. Hyper-V konağında VM Gereci barındıracak sıkıştırılmış VHD dosyasını bir klasöre ayıklayın. Üç klasör ayıklanır.
2. Hyper-V Yöneticisi'ni açın. İçinde **eylemleri**, tıklayın **sanal makineyi içeri aktar**.

    ![VHD dağıtma](./media/tutorial-assess-hyper-v/deploy-vhd.png)

2. Sanal makine İçeri Aktarma Sihirbazı'nda > **başlamadan önce**, tıklayın **sonraki**.
3. İçinde **klasörü Bul**, ayıklanan VHD içeren klasörü belirtin. Ardından **İleri**'ye tıklayın.
1. İçinde **sanal makineyi Seç**, tıklayın **sonraki**.
2. İçinde **içe aktarma tipini seç**, tıklayın **sanal makineyi kopyala (yeni bir benzersiz kimlik Oluştur)** . Ardından **İleri**'ye tıklayın.
3. İçinde **seçin hedef**, varsayılan ayarı değiştirmeyin.           **İleri**'ye tıklayın.
4. İçinde **depolama klasörleri**, varsayılan ayarı değiştirmeyin.           **İleri**'ye tıklayın.
5. İçinde **ağ seçin**, VM'nin kullanacağı sanal anahtar belirtin. Anahtar verileri Azure'a göndermek için internet bağlantısı gerekir.
6. İçinde **özeti**, ayarları gözden geçirin. Ardından **son**.
7. Hyper-V Yöneticisi > **sanal makineler**, VM'yi başlatın.


### <a name="verify-appliance-access-to-azure"></a>Azure'a Gereci erişimi doğrulayın

Gereç için VM bağlanabildiğinden emin olun [Azure URL'lerini](migrate-support-matrix-hyper-v.md#assessment-appliance-url-access).

### <a name="configure-the-appliance"></a>Gereci yapılandırın

Gereç ilk kez ayarlayın.

1. Hyper-V Yöneticisi > **sanal makineler**, VM'ye sağ tıklayın > **Connect**.
2. Gereç için dil, saat dilimi ve parola sağlayın.
3. VM'ye bağlanın ve gereç web uygulamasının URL'sini açın herhangi bir makinede bir tarayıcı açın: **https://*gereç adı veya IP adresi*: 44368**.

   Alternatif olarak, uygulamayı uygulama kısayoluna tıklayarak Gereci Desktop'tan açabilirsiniz.
1. Web uygulamasında > **önkoşulları ayarlama**, aşağıdakileri yapın:
    - **Lisans**: Lisans koşullarını kabul edin ve üçüncü taraf bilgilerini okuyun.
    - **Bağlantı**: Uygulama, VM internet erişimi olup olmadığını denetler. VM bir ara sunucu kullanıyorsa:
        - Tıklayın **Proxy ayarlarını**ve dinleme bağlantı noktasını ve proxy adresi biçiminde belirtin http://ProxyIPAddress veya http://ProxyFQDN.
        - Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin.
        - Yalnızca HTTP proxy’si desteklenir.
    - **Zaman eşitleme**: Zaman doğrulanır. Gereç zamanında düzgün çalışması, VM bulma için internet saati ile eşitlenmiş olması gerekir.
    - **Güncelleştirmeleri yüklemek**: Azure geçişi Server değerlendirmesi, gereç en son güncelleştirmelerin yüklü olduğunu denetler.

### <a name="register-the-appliance-with-azure-migrate"></a>Gerecin Azure geçişi ile kaydetme

1. Tıklayın **oturum**. Görünmüyorsa, tarayıcıda açılır pencere engelleyicisini devre dışı bıraktık emin olun.
2. Yeni sekmede Azure kimlik bilgilerinizi kullanarak oturum açın.
    - Kullanıcı adı ve parola ile oturum açın.
    - Bir PIN ile oturum açma desteklenmiyor.
3. Başarıyla oturum açtıktan sonra web uygulamasına geri dönün.
4. Azure geçişi projesi oluşturulduğu abonelik seçin. Ardından, projeyi seçin.
5. Gereç için bir ad belirtin. Ad ile 14 karakterler alfasayısal veya daha az olmalıdır.
6. Tıklayın **kaydetme**.


### <a name="delegate-credentials-for-smb-vhds"></a>SMB VHD'ler için kimlik bilgileri temsilcisi

VHD'ler SMB'ler üzerinde çalıştırıyorsanız, Hyper-V konaklarına gereç kimlik temsili etkinleştirmeniz gerekir. Her bir ana bilgisayarda bunu yapmadıysanız, [önceki öğreticide](tutorial-prepare-hyper-v.md#enable-credssp-on-hosts), bu gereç bugünden yapın:

1. VM gerecinde şu komutu çalıştırın. Örnek konak adları HyperVHost1/HyperVHost2 var.

    ```
    Enable-WSManCredSSP -Role Client -DelegateComputer HyperVHost1.contoso.com HyperVHost2.contoso.com -Force
    ```

2. Alternatif olarak, yerel Grup İlkesi Düzenleyicisi'nde gereçte yapın:
    - İçinde **yerel bilgisayar ilkesi** > **Bilgisayar Yapılandırması**, tıklayın **Yönetim Şablonları** > **sistem**  >  **Kimlik bilgileri temsilcisi**.
    - Çift **yeni kimlik bilgileri yetki aktarımına izin ver**seçip **etkin**.
    - İçinde **seçenekleri**, tıklayın **Göster**, ve ile listesi için keşfetmek istediğiniz her Hyper-V konağına eklemek **wsman /** öneki olarak.
    - İçinde **kimlik bilgileri devretme**, çift **yalnızca NTLM kimlik doğrulaması ile yeni kimlik bilgileri yetki aktarımına izin ver**. İle bir listesi için keşfetmek istediğiniz her Hyper-V konağı yeniden ekleyin **wsman /** öneki olarak.

## <a name="start-continuous-discovery"></a>Sürekli bulmayı Başlat

Hyper-V konakları veya kümeleri Gereci bağlanmak ve VM bulmayı Başlat.

1. İçinde **kullanıcı adı** ve **parola**, gereç Vm'leri bulmak için kullanacağı hesap kimlik bilgilerini belirtin. Kimlik bilgileri için kolay bir ad belirtin ve tıklayın **ayrıntıları Kaydet**.
2. Tıklayın **konak Ekle**, Hyper-V konak/küme ayrıntıları belirtin.
3. Tıklayın **doğrulama**. Doğrulama sonrasında her konak/küme üzerinde bulunan VM sayısı gösterilir.
    - Bir konak için doğrulama başarısız olursa, simgenin üzerine gelerek hatayı gözden geçirme **durumu** sütun. Sorunları giderin ve yeniden doğrulayın.
    - Konaklara veya kümelere kaldırmak için seçin > **Sil**.
    - Belirli bir ana bilgisayara bir kümeden kaldırılamıyor. Tüm küme yalnızca kaldırabilirsiniz.
    - Küme içindeki belirli ana bilgisayarlar ile ilgili sorunlar olsa bile, bir küme ekleyebilirsiniz.
4. Doğrulama **kaydedin ve bulmayı Başlat** bulma işlemini başlatmak için.

Bu bulma başlatır. Azure Portalı'nda görünmesi için meta verileri bulunan VM'ler için yaklaşık 15 dakika sürer.

### <a name="verify-vms-in-the-portal"></a>VM’lerin portalda olup olmadığını doğrulama

Bulma tamamlandıktan sonra sanal makinelerin portalda görüntülenip görüntülenmediğini doğrulayabilirsiniz.

1. Azure geçişi panoyu açın.
2. İçinde **Azure geçişi - sunucuları** > **Azure geçişi: Server değerlendirmesi** sayfasında, sayısını görüntüler simgesini **bulunan sunucuları**.

## <a name="set-up-an-assessment"></a>Bir değerlendirmenin ayarlayın

Azure geçişi Server değerlendirmesi'ni kullanarak çalıştırabilirsiniz değerlendirmeleri iki tür vardır.

**Değerlendirme** | **Ayrıntılar** | **Veri**
--- | --- | ---
**Performans tabanlı** | Toplanan performans verileri temel alan değerlendirmeleri | **VM boyutu önerilen**: CPU ve bellek kullanım verilerini temel alarak.<br/><br/> **Disk türü (standart veya premium yönetilen disk) önerilen**: IOPS ve aktarım hızını şirket içi disk bağlı.
**Şirket içi olarak** | Boyutlandırma şirket içinde temel alan değerlendirmeleri. | **VM boyutu önerilen**: Şirket içi VM boyutuna göre<br/><br> **Disk türü önerilen**: Değerlendirme için seçtiğiniz depolama türüne ayarına göre.



### <a name="run-an-assessment"></a>Değerlendirme çalıştırma

Değerlendirme gibi çalıştırın:

1. Gözden geçirme [en iyi uygulamalar](best-practices-assessment.md) değerlendirme oluşturmak için.
2. İçinde **sunucuları** > **Azure geçişi: Server değerlendirmesi**, tıklayın **değerlendirme**.

    ![Değerlendirme](./media/tutorial-assess-hyper-v/assess.png)

3. İçinde **değerlendirmek sunucuları**, değerlendirme için bir ad belirtin.
4. Değerlendirme özelliklerini gözden geçirmek için **Tümünü görüntüle**’ye tıklayın.

    ![Değerlendirme özellikleri](./media/tutorial-assess-hyper-v/assessment-properties.png)

3. İçinde **bir grubu seçin veya oluşturun**seçin **Yeni Oluştur** ve bir grup adı belirtin. Bir grubu bir veya daha fazla sanal makineleri değerlendirme için birlikte toplar.
4. İçinde **makineleri gruba ekleyin**, gruba eklemek için sanal makineleri seçin.
5. Tıklayın **Değerlendirme Oluştur** grubu oluşturmak ve değerlendirmeyi çalıştırın.

    ![Değerlendirme oluşturma](./media/tutorial-assess-hyper-v/assessment-create.png)

6. Değerlendirme oluşturulduktan sonra içinde görüntüleyebilir **sunucuları** > **Azure geçişi: Server değerlendirmesi**.
7. Excel dosyası olarak indirmek için **Değerlendirmeyi dışarı aktar**’a tıklayın.


## <a name="review-an-assessment"></a>Değerlendirme gözden geçirin

Değerlendirme açıklanmaktadır:

- **Azure için hazır olma**: Vm'leri Azure'a geçiş için uygun olup olmadığı.
- **Aylık maliyet tahmini**: Azure'da sanal makineleri çalıştırmak için tahmini aylık işlem ve depolama maliyetlerini.
- **Aylık depolama maliyeti tahmini**: Geçişten sonra disk depolama için tahmini maliyetler.


### <a name="view-an-assessment"></a>Bir değerlendirmeyi görüntüle

1. İçinde **geçiş hedefleri** >  **sunucuları** > **Azure geçişi: Server değerlendirmesi**, tıklayın **değerlendirmeleri**.
2. İçinde **değerlendirmeleri**, açmak için bir değerlendirme tıklayın.

    ![Değerlendirme özeti](./media/tutorial-assess-hyper-v/assessment-summary.png)


### <a name="review-azure-readiness"></a>Azure için hazır olma gözden geçirin

1. İçinde **Azure için hazır olma**, Vm'leri Azure'a geçiş için hazır olup olmadığını doğrulayın.
2. VM durumunu gözden geçirin:
    - **Azure için hazır**: Azure geçişi, VM boyutu ve maliyet tahminleri VM'ler için değerlendirme önerir.
    - **Koşullarla hazır**: Sorunlar ve önerilen düzeltme gösterir.
    - **Azure için hazır değil**: Sorunlar ve önerilen düzeltme gösterir.
    - **Hazır olma durumu bilinmiyor**: Azure geçişi, hazır olma durumu, veri kullanılabilirlik sorunları nedeniyle değerlendiremediğinde olduğunda kullanılır.

2. Tıklayarak bir **Azure için hazır olma** durumu. VM hazır olma durumu ayrıntılarını görüntülemek ve işlem, depolama ve ağ ayarları dahil olmak üzere bkz. VM, ayrıntıya.

### <a name="review-cost-details"></a>Maliyet gözden geçirmesi ayrıntıları

Bu görünüm, Azure'da çalışan VM'lerin tahmini işlem ve depolama maliyetini gösterir.

1. Aylık işlem ve depolama maliyetleri gözden geçirin. Maliyetlerinin değerlendirilen grubundaki tüm sanal makineler için toplanır.

    - Maliyet tahminleri, bir makine ve diskler ve özellikler için boyut önerileri temel alır.
    - İşlem ve depolama için tahmini aylık maliyetleri gösterilir.
    - Maliyet tahmini, Iaas Vm'leri olarak şirket içi Vm'leri çalıştırmaya yöneliktir. Azure geçişi Server değerlendirmesi PaaS veya SaaS maliyetleri dikkate almaz.

2. Aylık depolama maliyet tahminlerini gözden geçirebilirsiniz. Bu görünüm değerlendirilen grubu için toplam depolama maliyetlerini gösterir depolama disklerinin farklı türleri üzerinden bölün.
3. Belirli sanal makineler için bkz: ayrıntıları inceleyebilirsiniz.


### <a name="review-confidence-rating"></a>Güvenilirlik derecelendirmesini gözden geçirme

Performans tabanlı değerlendirmeleri çalıştırdığınızda, değerlendirme için güvenilirlik derecelendirmesi atanır.

![Güvenilirlik derecelendirmesi](./media/tutorial-assess-hyper-v/confidence-rating.png)

- Bir 1 yıldızdan (en düşük) 5 yıldızlı (yüksek) derecelendirmesi ödüllendirildi.
- Güvenilirlik derecelendirmesi, değerlendirmeyi tarafından sağlanan boyut önerilerinin güvenilirliğini tahmin etmenize yardımcı olur.
- Güvenilirlik derecelendirmesi değerlendirmeyi hesaplamak için gereken veri noktalarının kullanılabilirliğine bağlıdır.

Değerlendirme için güvenilirlik derecelendirmelerini aşağıdaki gibidir.

**Veri noktası kullanılabilirliği** | **Güvenilirlik derecelendirmesi**
--- | ---
%0-%20 | 1 Yıldız
%21-%40 | 2 Yıldız
%41-%60 | 3 Yıldız
%61-%80 | 4 Yıldız
%81-%100 | 5 Yıldız

[Daha fazla bilgi edinin](best-practices-assessment.md#best-practices-for-confidence-ratings) güvenle derecelendirmeleri için en iyi uygulamalar hakkında.





## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Azure geçişi gerecini ayarlamak
> * Oluşturulan ve bir değerlendirme gözden geçirilmedi

Hyper-V Vm'lerini Azure geçişi sunucusu geçişi ile Azure'a geçiş öğrenmek için serinin üçüncü öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Hyper-V sanal makineleri geçirme](./tutorial-migrate-hyper-v.md)
