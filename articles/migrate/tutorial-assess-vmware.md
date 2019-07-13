---
title: VMware Vm'lerini Azure geçişi ile Azure'a geçiş için değerlendirme
description: Şirket içi VMware Vm'lerini Azure Geçişi'ni kullanarak Azure'a geçiş için değerlendirme açıklar.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/12/2019
ms.author: hamusa
ms.openlocfilehash: 5f70037b1e6ce284b55ff5ff0ae38eb50c320122
ms.sourcegitcommit: 10251d2a134c37c00f0ec10e0da4a3dffa436fb3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2019
ms.locfileid: "67868667"
---
# <a name="assess-vmware-vms-with-azure-migrate-server-assessment"></a>Azure ile VMware sanal makineleri değerlendirme geçirme: Server değerlendirmesi

Bu makalede, Azure Geçişi'ni kullanarak şirket içi VMware Vm'lerinden, değerlendirme işlemini göstermektedir: Sunucu değerlendirme aracı.

[Azure geçişi](migrate-services-overview.md) hub'ı keşfedin, değerlendirin ve uygulamalar ve altyapı iş yükleri Microsoft Azure'a geçirmek için yardımcı araçlar sağlar. Hub, Azure geçişi araçları ve üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri içerir.



Bu öğretici, VMware sanal makinelerini Azure'a geçirme ve değerlendirme yapmayı gösteren bir serinin ikinci bölümüdür. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Bir Azure geçişi projesi ayarlayın.
> * Sanal makineleri değerlendirmek için şirket içinde çalışan bir Azure geçişi gereç ayarlayın.
> * Şirket içi sanal makinelerin sürekli bulma başlatın. Gereç, Azure'da bulunan VM'ler için yapılandırma ve performans verilerini gönderir.
> * Keşfedilen Vm'leri gruplar ve sanal makine grubu değerlendirin.
> * Değerlendirme gözden geçirin.



> [!NOTE]
> Öğreticiler hızlı bir kavram kanıtı ayarlayabilirsiniz bir senaryo için en basit dağıtım yolu gösterir. Öğreticiler, mümkün olduğunda varsayılan seçenekleri kullanın ve tüm olası ayarları ve yol gösterme. Ayrıntılı yönergeler için nasıl yapılır makaleleri inceleyin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.


## <a name="prerequisites"></a>Önkoşullar

- [Tam](tutorial-prepare-vmware.md) bu serideki ilk öğreticide. Aksi takdirde, bu öğreticideki yönergeler işe yaramaz.
- İşte ne ilk öğreticide yaptığınız:
    - [Azure izinleri ayarlama](tutorial-prepare-vmware.md#prepare-azure) Azure geçişi için.
    - [Vmware'i hazırlama](tutorial-prepare-vmware.md#prepare-for-vmware-vm-assessment) değerlendirmesi için. VMware ayarları yeniden doğrulanmalıdır ve OVA şablonuyla bir VMware VM oluşturmak için izinlere sahip olmalıdır. Ayrıca, VM bulma için ayarlanmış bir hesabınız olmalıdır. Gerekli bağlantı noktaları kullanılabilir olması gerektiğini ve Azure erişimi için gereken URL'leri haberdar olmanız gerekir.


## <a name="set-up-an-azure-migrate-project"></a>Azure geçişi projesi kurun

Yeni bir Azure geçişi projesi şu şekilde ayarlayın.

1. Azure portalında > **tüm hizmetleri**, arama **Azure geçişi**.
2. Altında **Hizmetleri**seçin **Azure geçişi**.
3. İçinde **genel bakış**altında **keşfedin, değerlendirin ve sunucularını geçirme**, tıklayın **değerlendirin ve sunucularını geçirme**.

    ![Bulma ve değerlendirme sunucuları](./media/tutorial-assess-vmware/assess-migrate.png)

4. İçinde **Başlarken**, tıklayın **ekleme Araçları**.
5. İçinde **geçiş projesi**, Azure aboneliğinizi seçin ve bir yoksa, bir kaynak grubu oluşturun.     
6. İçinde **Project Details**, proje adı ve projeyi oluşturmak istediğiniz coğrafi konum belirtin. Asya, Avrupa, Birleşik Krallık ve Amerika Birleşik Devletleri desteklenir.

    - Proje Coğrafya, yalnızca şirket içi Vm'lerden toplanan meta verileri depolamak için kullanılır.
    - Bir geçiş çalıştırdığınızda, herhangi bir hedef bölgeyi seçebilirsiniz.

    ![Bir Azure geçişi projesi oluşturun](./media/tutorial-assess-vmware/migrate-project.png)


7.           **İleri**'ye tıklayın.
8. İçinde **Select Değerlendirme Aracı**seçin **Azure geçişi: Server değerlendirmesi** > **sonraki**.

    ![Bir Azure geçişi projesi oluşturun](./media/tutorial-assess-vmware/assessment-tool.png)

9. İçinde **Select geçiş aracı**seçin **şimdilik geçiş aracı eklemeyi atlamak mı** > **sonraki**.
10. İçinde **gözden + araçları Ekle**, ayarları gözden geçirin ve tıklayın **ekleme Araçları**.
11. Azure geçişi projesi dağıtmak için birkaç dakika bekleyin. Proje sayfasına gidersiniz. Proje görmüyorsanız, buradan erişebilirsiniz **sunucuları** Azure geçişi panosunda.


## <a name="set-up-the-appliance-vm"></a>VM gerecini ayarlamak

Azure geçişi: Basit bir VMware VM Gereci Server değerlendirmesi çalıştırır.

- Bu gereç, VM bulma işlemini gerçekleştirir ve Azure geçişi Server değerlendirmesi için VM meta verileri ve performans verilerini gönderir.
- Ayarladığınız gerecini ayarlamak için:
    - OVA bir şablon dosyasını indirin ve vCenter Server'a aktarın.
    - Aleti oluşturmak ve Azure geçişi Server değerlendirmesi için bağlanıp bağlanamadığını denetleyin.
    - İlk kez Gereci yapılandırın ve Azure geçişi projesi ile kaydedin.
- Birden çok cihazları tek bir Azure geçişi projesi için ayarlayabilirsiniz. Tüm cihazları arasında en fazla 35.000 VM'lerin bulma desteklenir. Gereç en fazla 10.000 sunucuları bulunabilir.

### <a name="download-the-ova-template"></a>OVA şablonunu indirme

1. İçinde **geçiş hedefleri** > **sunucuları** > **Azure geçişi: Server değerlendirmesi**, tıklayın **bulma**.
2. İçinde **makineleri keşfet** > **makineleriniz sanallaştırıldı mı?** , tıklayın **Evet, VMWare vSphere hiper Yöneticisi ile**.
3. Tıklayın **indirme** indirilemedi. OVA şablon dosyası.

    ![.ova dosyasını indir](./media/tutorial-assess-vmware/download-ova.png)


### <a name="verify-security"></a>Güvenlik doğrulayın

OVA dosyasını dağıtmadan önce güvenli olup olmadığını denetleyin.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. OVA’nın karmasını oluşturmak için aşağıdaki komutu çalıştırın:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek kullanım: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. 1\.19.06.27 sürümü için oluşturulan karma bu değerlerin eşleşmesi. 

  **Algoritma** | **Karma değeri**
  --- | ---
  MD5 | 605d208ac5f4173383f616913441144e
  SHA256 | 447d16bd55f20f945164a1189381ef6e98475b573d6d1c694f3e5c172cfc30d4


### <a name="create-the-appliance-vm"></a>' % S'Gereci VM oluşturma

İndirilen dosyayı içeri aktarın ve bir VM oluşturun.

1. vSphere Client konsolunda **Dosya** > **OVF Şablonu Dağıt**’a tıklayın.

    ![OVF dağıtma](./media/tutorial-assess-vmware/deploy-ovf.png)

2. OVF şablonu Dağıtma Sihirbazı > **kaynak**, OVA dosyasının konumunu belirtin.
3. İçinde **adı** ve **konumu**, VM için bir kolay ad belirtin. Sanal Makinenin barındırılacağı Envanter nesnesini seçin.
5. İçinde **konak/küme**, konağı veya kümeyi belirtin, sanal Makinenin çalıştığı.
6. İçinde **depolama**, VM için depolama hedefini belirtin.
7. **Disk Biçimi**’nde disk türünü ve boyutunu belirtin.
8. İçinde **ağ eşlemesi**, sanal Makinenin bağlanacağı ağı belirtin. Ağ Azure geçişi Server değerlendirmesi için meta verileri göndermek için internet bağlantısına sahip olmalıdır.
9. Ayarları gözden geçirip onayladıktan sonra **Son**’a tıklayın.


### <a name="verify-appliance-access-to-azure"></a>Azure'a Gereci erişimi doğrulayın

Gereç için VM bağlanabildiğinden emin olun [Azure URL'lerini](migrate-support-matrix-vmware.md#assessment-url-access-requirements).


### <a name="configure-the-appliance"></a>Gereci yapılandırın

Aşağıdaki adımları kullanarak gereç ayarlayın.

1. vSphere Client konsolunda VM’ye sağ tıklayın ve **Konsolu Aç** seçeneğini belirleyin.
2. Gereç için dil, saat dilimi ve parola sağlayın.
3. VM'ye bağlanın ve gereç web uygulamasının URL'sini açın herhangi bir makinede bir tarayıcı açın: **https://*gereç adı veya IP adresi*: 44368**.

   Alternatif olarak, uygulamayı uygulama kısayoluna tıklayarak Gereci Desktop'tan açabilirsiniz.
4. Web uygulamasında > **önkoşulları ayarlama**, aşağıdakileri yapın:
    - **Lisans**: Lisans koşullarını kabul edin ve üçüncü taraf bilgilerini okuyun.
    - **Bağlantı**: Uygulama, VM internet erişimi olup olmadığını denetler. VM bir ara sunucu kullanıyorsa:
        - Tıklayın **Proxy ayarlarını**ve dinleme bağlantı noktasını ve proxy adresi biçiminde belirtin http://ProxyIPAddress veya http://ProxyFQDN.
        - Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin.
        - Yalnızca HTTP proxy’si desteklenir.
    - **Zaman eşitleme**: gerecin zamanında düzgün çalışması bulma için internet saati ile eşitlenmiş olması gerekir.
    - **Güncelleştirmeleri yüklemek**: Gereç, en son güncelleştirmelerin yüklü olduğunu sağlar.
    - **VDDK yükleme**: Gereç Sanal Disk Geliştirme Seti (VDDK) yüklü VMWare vSphere denetler.
        - Azure geçişi: Sunucu geçişi, azure'a geçiş sırasında makineleri çoğaltmak için VDDK kullanır.
        - Vmware'den VDDK 6.7 yükleyin ve indirilen ZIP içeriği gereçte belirtilen konuma ayıklayın.

### <a name="register-the-appliance-with-azure-migrate"></a>Gerecin Azure geçişi ile kaydetme

1. Tıklayın **oturum**. Görünmüyorsa, tarayıcıda açılır pencere engelleyicisini devre dışı bıraktık emin olun.
2. Yeni sekmede Azure kimlik bilgilerinizi kullanarak oturum açın.
    - Kullanıcı adı ve parola ile oturum açın.
    - Bir PIN ile oturum desteklenmiyor.
3. Başarıyla oturum açtıktan sonra web uygulamasına geri dönün.
2. Azure geçişi projesi oluşturulduğu aboneliği seçin, sonra projeyi seçin.
3. Gereç için bir ad belirtin. Ad ile 14 karakterler alfasayısal veya daha az olmalıdır.
4. Tıklayın **kaydetme**.


## <a name="start-continuous-discovery"></a>Sürekli bulmayı Başlat

Şimdi, Gereci vCenter Server'a bağlanın ve VM bulmayı Başlat.

1. İçinde **vCenter Server ayrıntılarını belirtin**, adını (FQDN) veya vCenter Server'ın IP adresi belirtin. Varsayılan bağlantı noktasını değiştirmeyin veya vCenter Server'ınıza dinlediği bir özel bağlantı noktası belirtin.
2. İçinde **kullanıcı adı** ve **parola**, Gereci vCenter sunucusundaki Vm'leri bulmak için kullanacağı salt okunur hesabın kimlik bilgilerini belirtin. Hesabı olduğundan emin olun [bulma için gerekli izinler](migrate-support-matrix-vmware.md#assessment-vcenter-server-permissions). Buna göre vCenter hesabına erişimi kısıtlayarak bulma kapsamını belirleyebilirsiniz; bulma kapsamı hakkında daha fazla bilgi [burada](tutorial-assess-vmware.md#scoping-discovery).
3. Tıklayın **bağlantısını doğrulama** için Gereci vCenter Server'a bağlanabildiğinden emin olun.
4. Bağlantı kurulduktan sonra tıklayın **kaydedin ve bulmayı Başlat**.

Bu bulma başlatır. Portalda görünmesi bulunan VM'ler meta verilerini yaklaşık 15 dakika sürer.

### <a name="scoping-discovery"></a>Bulma kapsamı

Bulma, bulma için kullanılan vCenter hesabının erişimini kısıtlayarak sınırlayabilirsiniz. Kapsam vCenter sunucusu veri merkezleri, kümeleri, kümeleri, konaklar, konak veya tek tek sanal makineleri klasöründe klasörü ayarlayabilirsiniz. 

> [!NOTE]
> Bugün, Server değerlendirmesi vCenter hesabı vCenter VM klasör düzeyinde verilen erişimi varsa Vm'leri bulmak mümkün değil. VM klasörlere göre bulma kapsamı için arıyorsanız, vCenter sağlayarak hesabı VM düzeyinde atanmış salt okunur erişime sahiptir, bunu yapabilirsiniz.  Bunu yapmak nasıl yönergeler aşağıda verilmiştir:
>
> 1. Bulma kapsamı istediğiniz VM klasörlerdeki tüm sanal makineler salt okunur izinler atayın. 
> 2. Vm'leri barındırıldığı tüm üst nesneleri yalnızca okuma erişimi verin. Tüm üst - host, konaklar, küme, klasör kümelerinin klasörü - veri merkezi kadar hiyerarşideki dahil edilecek nesneleridir. Tüm alt nesneleri için izinleri yayılması gerekmez.
> 3. Veri merkezi olarak seçerek bulma için kimlik bilgilerini kullan *koleksiyon kapsamı*. Ayarlanan RBAC, karşılık gelen bir vCenter kullanıcı yalnızca kiracıya özgü Vm'leri erişimi olmasını sağlar.
>
> Konak bu klasöre dikkat edin ve kümeleri desteklenir.

### <a name="verify-vms-in-the-portal"></a>VM’lerin portalda olup olmadığını doğrulama

Bulunduktan sonra sanal makinelerin Azure portalda görüntülenip görüntülenmediğini doğrulayın.

1. Azure geçişi panoyu açın.
2. İçinde **Azure geçişi - sunucuları** > **Azure geçişi: Server değerlendirmesi** sayfasında, sayısını görüntüler simgesini **bulunan sunucuları**.

## <a name="set-up-an-assessment"></a>Bir değerlendirmenin ayarlayın

Değerlendirmeler Azure Geçişi'ni kullanarak oluşturabileceğiniz iki tür vardır: Server değerlendirmesi.

**Değerlendirme** | **Ayrıntılar** | **Veri**
--- | --- | ---
**Performans tabanlı** | Toplanan performans verileri temel alan değerlendirmeleri | **VM boyutu önerilen**: CPU ve bellek kullanım verilerini temel alarak.<br/><br/> **Disk türü (standart veya premium yönetilen disk) önerilen**: IOPS ve aktarım hızını şirket içi disk bağlı.
**Şirket içi olarak** | Boyutlandırma şirket içinde temel alan değerlendirmeleri. | **VM boyutu önerilen**: Şirket içi VM boyutuna göre<br/><br> **Disk türü önerilen**: Değerlendirme için seçtiğiniz depolama türüne ayarına göre.


### <a name="run-an-assessment"></a>Değerlendirme çalıştırma

Değerlendirme gibi çalıştırın:

1. Gözden geçirme [en iyi uygulamalar](best-practices-assessment.md) değerlendirme oluşturmak için.
2. İçinde **sunucuları** sekmesinde **Azure geçişi: Server değerlendirmesi** 'a tıklayın **değerlendirme**.

    ![Değerlendirme](./media/tutorial-assess-vmware/assess.png)

2. İçinde **sunucuları değerlendirmek**, değerlendirme için bir ad belirtin.
3. Değerlendirme özelliklerini gözden geçirmek için **Tümünü görüntüle**’ye tıklayın.

    ![Değerlendirme özellikleri](./media/tutorial-assess-vmware/view-all.png)

3. İçinde **bir grubu seçin veya oluşturun**seçin **Yeni Oluştur**, bir grup adı belirtin. Bir grubu bir veya daha fazla sanal makineleri değerlendirme için birlikte toplar.
4. İçinde **makineleri gruba ekleyin**, gruba eklemek için sanal makineleri seçin.
5. Tıklayın **Değerlendirme Oluştur** grubu oluşturmak ve değerlendirmeyi çalıştırın.

    ![Değerlendirme oluşturma](./media/tutorial-assess-vmware/assessment-create.png)

6. Değerlendirme oluşturulduktan sonra içinde görüntüleyebilir **sunucuları** > **Azure geçişi: Server değerlendirmesi** > **değerlendirmeleri**.
7. Excel dosyası olarak indirmek için **Değerlendirmeyi dışarı aktar**’a tıklayın.



## <a name="review-an-assessment"></a>Değerlendirme gözden geçirin

Değerlendirme açıklanmaktadır:

- **Azure için hazır olma**: Vm'leri Azure'a geçiş için uygun olup olmadığı.
- **Aylık maliyet tahmini**: Azure'da sanal makineleri çalıştırmak için tahmini aylık işlem ve depolama maliyetlerini.
- **Aylık depolama maliyeti tahmini**: Geçişten sonra disk depolama için tahmini maliyetler.

### <a name="view-an-assessment"></a>Bir değerlendirmeyi görüntüle

1. İçinde **geçiş hedefleri** >  **sunucuları**, tıklayın **değerlendirmeleri** içinde **Azure geçişi: Server değerlendirmesi**.
2. İçinde **değerlendirmeleri**, açmak için bir değerlendirme tıklayın.

    ![Değerlendirme özeti](./media/tutorial-assess-vmware/assessment-summary.png)

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

![Güvenilirlik derecelendirmesi](./media/tutorial-assess-vmware/confidence-rating.png)

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

VMware Vm'lerini Azure geçişi sunucusu geçişi ile Azure'a geçiş öğrenmek için serinin üçüncü öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [VMware Vm'lerini geçirme](./tutorial-migrate-vmware.md)
