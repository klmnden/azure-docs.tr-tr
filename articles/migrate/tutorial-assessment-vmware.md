---
title: Azure’a geçiş için şirket içi VMware VM’lerini Azure Geçişi ile bulma ve değerlendirme | Microsoft Docs
description: Azure’a geçiş için şirket içi VMware VM’lerinin Azure Geçişi hizmeti kullanılarak nasıl bulunacağı ve değerlendirileceği açıklanmaktadır.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 01/31/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 9eab8a29db40118f2a15064c52419ecebcd4aecb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60749895"
---
# <a name="discover-and-assess-on-premises-vmware-vms-for-migration-to-azure"></a>Azure’a geçiş için şirket içi VMware VM’lerini bulma ve değerlendirme

[Azure Geçişi](migrate-overview.md) hizmetleri, Azure’a geçiş için şirket içi iş yüklerini değerlendirir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure Geçişi’nin şirket içi VM’leri bulmak için kullanacağı bir hesap oluşturma
> * Bir Azure Geçişi projesi oluşturun.
> * Değerlendirmek için şirket içi VMware VM’lerini toplamak üzere bir şirket içi toplayıcı sanal makinesi (VM) ayarlayın.
> * VM’leri gruplandırın ve bir değerlendirme oluşturun.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

- **VMware**: Geçirmeyi planladığınız VM'ler sürümünü çalıştıran vCenter Server tarafından 5.5, 6.0, 6.5 veya 6.7 yönetilmesi gerekir. Ayrıca, 5.5 veya Toplayıcı VM'yi dağıtmak için daha yüksek bir ESXi ana çalışan sürümü gerekir.
- **vCenter Server hesabı**: VCenter Server'a erişmek için salt okunur bir hesap gerekir. Azure Geçişi, şirket içi VM’leri bulmak için bu hesabı kullanır.
- **İzinleri**: VCenter Server'da, bir dosyada içeri aktararak VM oluşturma izni gerekir. OVA biçimi.

## <a name="create-an-account-for-vm-discovery"></a>VM bulma işlemi için hesap oluşturma

Azure Geçişi’nin, değerlendirme amacıyla VM’leri otomatik olarak bulması için VMware sunucularına erişebilmesi gerekir. Aşağıdaki özelliklere sahip bir VMware hesabı oluşturun. Bu hesabı Azure Geçişi kurulumu sırasında belirtirsiniz.

- Kullanıcı türü: En az bir salt okunur kullanıcı
- İzinler: Veri Merkezi nesnesi –> Alt Nesneye Yay, role=Read-only
- Ayrıntılar: Kullanıcı veri merkezi düzeyinde atandı ve bu veri merkezindeki tüm nesnelere erişimi var.
- Erişimi kısıtlamak için Alt nesneye yay ile Erişim yok rolünü alt nesnelere (vSphere konakları, veri depoları, VM’ler ve ağlar) atayın.


## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com) oturum açın.

## <a name="create-a-project"></a>Proje oluşturma

1. Azure portalında **Kaynak oluştur**’a tıklayın.
2. **Azure Geçişi** araması yapın ve arana sonuçlarında **Azure Geçişi** hizmetini seçin. Sonra **Oluştur**’a tıklayın.
3. Proje için bir proje adı ve Azure aboneliği belirtin.
4. Yeni bir kaynak grubu oluşturun.
5. Projeyi oluşturmak istediğiniz coğrafyayı belirtin ve ardından **Oluştur**’a tıklayın. Bu gibi durumlarda, Azure geçişi projesini yalnızca aşağıdaki coğrafyalardaki oluşturabilirsiniz. Ancak yine de herhangi bir hedef Azure konumu için geçişinizi planlayabilirsiniz. Proje için belirtilen coğrafya yalnızca şirket içi VM’lerden toplanan meta verileri depolamak için kullanılır.

**Coğrafya** | **Depolama konumu**
--- | ---
Azure Kamu | ABD Devleti Virginia
Asya | Güneydoğu Asya
Avrupa | Kuzey Avrupa veya Batı Avrupa
Durumları sahip | Doğu ABD ve Batı Orta ABD

![Azure Geçişi](./media/tutorial-assessment-vmware/project-1.png)


## <a name="download-the-collector-appliance"></a>Toplayıcı gerecini indirin

Azure Geçişi, toplayıcı gereci olarak bilinen bir şirket içi VM oluşturur. Bu VM, şirket içi VMware VM’lerini bulur ve bunlara ilişkin meta verileri Azure Geçişi hizmetine gönderir. Toplayıcı gerecini ayarlamak için, bir .OVA dosyası indirin ve VM’yi oluşturmak için dosyayı şirket içi vCenter sunucusuna aktarın.

1. Azure Geçişi projesinde **Kullanmaya Başlama** > **Bul ve Değerlendir** > **Makineleri Keşfet**’ye tıklayın.
2. İçinde **makineleri Bul**, tıklayın **indirin** gereç indirilemedi.

    Azure geçişi Gereci vCenter Server ile iletişim kurar ve sürekli olarak şirket içi ortamda, her VM için gerçek zamanlı kullanım verilerini toplamak için profil. Bu, her bir ölçüm (CPU kullanımı, bellek kullanımı vb.) için en yüksek sayaçları toplar. Bu model, performans verilerinin toplanması için vCenter Server’ın istatistik ayarlarına bağlı değildir. Sürekli profil oluşturmayı aletten istediğiniz zaman durdurabilirsiniz.

    > [!NOTE]
    > Bu yöntem, vCenter Server'ın performans veri noktası kullanılabilirlik için istatistik ayarları yararlandı ve sanal makinelerin Azure'a geçiş için eksik boyutlandırma içinde sonuçlanan ortalama performans sayaçlarının toplanan gibi tek seferlik gereç artık kullanım dışı bırakılmıştır.

    **Hızlı değerlendirmesi:** Bulma olduğunda (alır birkaç saat VM sayısına bağlı olarak), sürekli bulma gereciyle tamamlamak değerlendirmeler hemen oluşturabilirsiniz. Performans veri toplama başlar hızlı değerlendirmeleri için arıyorsanız, Keşif başlatmadan sonra değerlendirmede boyutlandırma ölçütü seçmelisiniz *şirket içi olarak*. Performans tabanlı değerlendirmeleri için en az bir gün sonra güvenilir boyut önerileri almak için keşif başlatılmadan için beklemeniz önerilir.

    Gereç yalnızca performans verilerini sürekli olarak toplar, şirket içi ortamda (yani, VM ekleme, silme, disk ekleme vb.) herhangi bir yapılandırma değişikliği algılamaz. Şirket içi ortamda bir yapılandırma değişikliği gerçekleşirse değişikliklerin portala yansıması için aşağıdakileri yapabilirsiniz:

    - Öğelerin eklenmesi (VM, disk, çekirdek vb.): Bu değişiklikleri Azure portalına yansıtmak için bulma işlemini gereçten durdurup yeniden başlatabilirsiniz. Bu, değişikliklerin Azure Geçişi projesinde güncelleştirilmesini sağlar.

    - VM silme: Gerecin tasarlanma şekli nedeniyle bulma işlemini durdurup başlatsanız bile VM silme yansıtılmaz. Bunun nedeni takip eden keşiflerin eski keşiflerin üzerine yazılması yerine bunlara eklenmesidir. Bu durumda grubunuzdan kaldırarak ve değerlendirmeyi yeniden hesaplayarak portaldaki VM’yi yoksayabilirsiniz.


3. **Proje kimlik bilgilerini kopyala** bölümünde proje kimliğini ve anahtarı kopyalayın. Toplayıcıyı yapılandırırken bu bilgilere ihtiyaç duyarsınız.

    ![.ova dosyasını indir](./media/tutorial-assessment-vmware/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Toplayıcı gereci doğrulama

Dağıtmadan önce .OVA dosyasının güvenilir olup olmadığını kontrol edin.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. OVA’nın karmasını oluşturmak için aşağıdaki komutu çalıştırın:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek kullanım: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Oluşturulan karma bu ayarlara uygun olmalıdır.

#### <a name="continuous-discovery"></a>Sürekli keşif

  OVA sürüm 1.0.10.11

  **Algoritma** | **Karma değeri**
    --- | ---
    MD5 | 5f6b199d8272428ccfa23543b0b5f600
    SHA1 | daa530de6e8674a66a728885a7feb3b0a2e8ccb0
    SHA256 | 85da50a21a7a6ca684418a87ccc1dd4f8aab30152c438a17b216ec401ebb3a21

  OVA sürüm 1.0.10.9

  **Algoritma** | **Karma değeri**
  --- | ---
  MD5 | 169f6449cc1955f1514059a4c30d138b
  SHA1 | f8d0a1d40c46bbbf78cd0caa594d979f1b587c8f
  SHA256 | d68fe7d94be3127eb35dd80fc5ebc60434c8571dcd0e114b87587f24d6b4ee4d

  OVA sürüm 1.0.10.4 için

  **Algoritma** | **Karma değeri**
  --- | ---
  MD5 | 2ca5b1b93ee0675ca794dd3fd216e13d
  SHA1 | 8c46a52b18d36e91daeae62f412f5cb2a8198ee5
  SHA256 | 3b3dec0f995b3dd3c6ba218d436be003a687710abab9fcd17d4bdc90a11276be


#### <a name="one-time-discovery-deprecated-now"></a>Tek seferlik bulma (artık kullanım dışı)

Bu model kullanım dışı bırakıldı, var olan cihazları sağlanan için destek.

  OVA sürüm 1.0.9.15 için

  **Algoritma** | **Karma değeri**
  --- | ---
  MD5 | e9ef16b0c837638c506b5fc0ef75ebfa
  SHA1 | 37b4b1e92b3c6ac2782ff5258450df6686c89864
  SHA256 | 8a86fc17f69b69968eb20a5c4c288c194cdcffb4ee6568d85ae5ba96835559ba

  OVA sürüm 1.0.9.14 için

  **Algoritma** | **Karma değeri**
  --- | ---
  MD5 | 6d8446c0eeba3de3ecc9bc3713f9c8bd
  SHA1 | e9f5bdfdd1a746c11910ed917511b5d91b9f939f
  SHA256 | 7f7636d0959379502dfbda19b8e3f47f3a4744ee9453fc9ce548e6682a66f13c

  OVA sürüm 1.0.9.12 için

  **Algoritma** | **Karma değeri**
  --- | ---
  MD5 | d0363e5d1b377a8eb08843cf034ac28a
  SHA1 | df4a0ada64bfa59c37acf521d15dcabe7f3f716b
  SHA256 | f677b6c255e3d4d529315a31b5947edfe46f45e4eb4dbc8019d68d1d1b337c2e

## <a name="create-the-collector-vm"></a>Toplayıcı VM’yi oluşturma

İndirilen dosyayı vCenter Server’a aktarın.

1. vSphere Client konsolunda **Dosya** > **OVF Şablonu Dağıt**’a tıklayın.

    ![OVF dağıtma](./media/tutorial-assessment-vmware/vcenter-wizard.png)

2. OVF Şablonu Dağıtma Sihirbazı > **Kaynak** bölümünde .ova dosyasının konumunu belirtin.
3. **Ad** ve **Konum** bölümünde toplayıcı VM için bir kolay ad ve VM’nin barındırılacağı envanter nesnesini belirtin.
5. **Konak/Küme** bölümünde, toplayıcı VM’nin çalıştırılacağı konağı veya kümeyi belirtin.
7. Depolama’da, toplayıcı VM için depolama hedefini belirleyin.
8. **Disk Biçimi**’nde disk türünü ve boyutunu belirtin.
9. **Ağ Eşleme** bölümünde toplayıcı VM’nin bağlanacağı ağı belirtin. Meta verileri Azure’a göndermek için ağ, İnternet bağlantısına sahip olmalıdır.
10. Ayarları gözden geçirip onayladıktan sonra **Son**’a tıklayın.

## <a name="run-the-collector-to-discover-vms"></a>VM’leri bulmak için toplayıcıyı çalıştırma

1. vSphere Client konsolunda VM’ye sağ tıklayın ve **Konsolu Aç** seçeneğini belirleyin.
2. Gereç için dil, saat dilimi ve parola tercihlerini belirtin.
3. Masaüstünde **Toplayıcı çalıştır** kısayoluna tıklayın.
4. Toplayıcı kullanıcı arabiriminin üst çubuğundaki **Güncelleştirmeleri denetle**'ye tıklayın ve toplayıcının en son sürümde çalıştırıldığını doğrulayın. Aksi takdirde, bağlantıdan en son yükseltme paketini indirmeyi seçebilir ve toplayıcıyı güncelleştirebilirsiniz.
5. Azure Geçişi Toplayıcısı’nda **Önkoşulları ayarla** seçeneğini açın.
   - (Azure Global veya Azure kamu) geçirmeyi planladığınız Azure Bulutu seçin.
   - Lisans koşullarını kabul edin ve üçüncü taraf bilgilerini okuyun.
   - Toplayıcı, VM’nin İnternet erişimine sahip olup olmadığını denetler.
   - VM, proxy üzerinden İnternet erişimine sahipse **Proxy ayarları**’na tıklayın ve proxy adresini ve dinleme bağlantı noktasını belirtin. Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/concepts-collector#collector-prerequisites) internet bağlantı gereksinimleri hakkında ve [URL'lerin listesini](https://docs.microsoft.com/azure/migrate/concepts-collector) Toplayıcı erişen.

     > [!NOTE]
     > Proxy adresi form http girilmesi gerekir:\//ProxyIPAddress veya http:\//ProxyFQDN. Yalnızca HTTP proxy’si desteklenir. Araya giren bir proxy varsa, proxy sertifikası aktardıysanız değil, internet bağlantısı başlangıçta başarısız olabilir; [daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/concepts-collector) üzerinde nasıl, bu toplayıcı VM üzerinde güvenilen bir sertifika olarak proxy sertifikasını alarak düzeltebilirsiniz.

   - Toplayıcı, toplayıcı hizmetinin çalışıp çalışmadığını denetler. Hizmet, toplayıcı VM’ye varsayılan olarak yüklenir.
   - VMware PowerCLI’yı indirin ve yükleyin.

6. **vCenter Server bilgilerini belirtin** bölümünde şunları yapın:
    - vCenter sunucusunun adını (FQDN) veya IP adresini belirtin.
    - **Kullanıcı adı** ve **Parola** bölümünde, toplayıcının vCenter sunucusundaki VM’leri bulmak için kullanacağı salt okunur hesabın kimlik bilgilerini belirtin.
    - **Toplama kapsamı**’nda, VM bulma için bir kapsam seçin. Toplayıcı yalnızca belirtilen kapsam içindeki VM’leri bulabilir. Kapsam belirli bir klasör, veri merkezi veya küme olarak ayarlanabilir. Kapsam en fazla 1500 VM’yi içermelidir. Daha büyük bir ortamı nasıl bulabileceğiniz hakkında [daha fazla bilgi edinin](how-to-scale-assessment.md).

       > [!NOTE]
       > **Toplama kapsamı** yalnızca konakların ve kümelerin klasörleri listeler. Klasörleri sanal makinelerinin doğrudan koleksiyon kapsamı olarak seçilemez. Ancak, tek VM'ler için erişimi olan bir vCenter hesabı kullanarak bulabilir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/how-to-scale-assessment#set-up-permissions) kapsam VM'lerin bir klasöre nasıl hakkında.

7. **Geçişi projesini belirtin** bölümünde portaldan kopyaladığınız Azure Geçişi proje kimliğini ve anahtarını belirtin. Bu bilgileri kopyalamadıysanız toplayıcı VM’den Azure portalını açın. Projenin **Genel Bakış** sayfasında **Makineleri Bul**’a tıklayın ve değerleri kopyalayın.  
8. **Toplama ilerleme durumunu izle**’de, keşif durumunu izleyin. Azure Geçişi toplayıcı tarafından toplanan veriler hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/concepts-collector).

> [!NOTE]
> Toplayıcı, işletim sistemi dili ve toplayıcı arabirimi dili olarak yalnızca "İngilizce (ABD)"yi destekler.
> Değerlendirmek istediğiniz bir makinenin ayarlarını değiştirirseniz, değerlendirmeyi çalıştırmadan önce yeniden keşfetmeyi tetikleyin. Toplayıcıda, bunu yapmak için **Koleksiyonu yeniden başlat** seçeneğini kullanın. Koleksiyon tamamlandıktan sonra, güncelleştirilmiş değerlendirme sonuçlarını almak için portalda değerlendirmeye yönelik **Yeniden hesapla** seçeneğini belirleyin.


### <a name="verify-vms-in-the-portal"></a>VM’lerin portalda olup olmadığını doğrulama

Toplayıcı gerecini sürekli olarak şirket içi ortamda profil ve performans verilerini bir saatlik zaman aralığı içinde göndermeye devam. Keşif başlatılmadan bir saat sonra makineleri portalında görüntüleyebilirsiniz.

1. Geçiş projesinde **Yönet** > **Makineler**’e tıklayın.
2. Bulmak istediğiniz VM’lerin portalda görüntülenip görüntülenmediğini kontrol edin.


## <a name="create-and-view-an-assessment"></a>Değerlendirme oluşturma ve görüntüleme

Portalda VM'ler bulunduktan sonra bunları gruplandırın ve değerlendirme oluşturun. Değerlendirmeler Portalı'nda VM'ler bulunduktan sonra şirket olarak hemen oluşturabilirsiniz. Güvenilir boyut önerileri almak için tüm performans temel alan değerlendirmeleri oluşturmadan önce en az bir gün beklemeniz önerilir.

1. Projenin **Genel Bakış** sayfasında **+Değerlendirme oluştur**’a tıklayın.
2. Değerlendirme özelliklerini gözden geçirmek için **Tümünü görüntüle**’ye tıklayın.
3. Grubu oluşturun ve bir grup adı belirtin.
4. Gruba eklemek istediğiniz makineleri seçin.
5. Grubu ve değerlendirmeyi oluşturmak için **Değerlendirme Oluştur**’a tıklayın.
6. Değerlendirme oluşturulduktan sonra **Genel Bakış** > **Pano** bölümünde görüntüleyebilirsiniz.
7. Excel dosyası olarak indirmek için **Değerlendirmeyi dışarı aktar**’a tıklayın.

> [!NOTE]
> Bulma, değerlendirme oluşturmadan önce başlangıç sonrasında en az bir gün için beklenecek önemle tavsiye edilir. Var olan bir değerlendirmeyi en son performans verileriyle güncelleştirmek isterseniz, değerlendirmeyi güncelleştirmek için **Yeniden Hesapla** komutunu kullanabilirsiniz.

### <a name="assessment-details"></a>Değerlendirme ayrıntıları

Değerlendirme, şirket içi sanal makinelerin Azure için uyumlu olup olmadığı, Azure’da sanal makineyi çalıştırmak için doğru sanal makine boyutunun ne olacağı ve tahmini aylık Azure maliyetleri hakkında bilgileri içerir.

![Değerlendirme raporu](./media/tutorial-assessment-vmware/assessment-report.png)

#### <a name="azure-readiness"></a>Azure için hazır olma

Değerlendirmedeki Azure için hazır olma görünümü, her bir sanal makinenin hazır olma durumunu gösterir. Sanal makinenin özelliklerine bağlı olarak her bir sanal makine şöyle işaretlenebilir:
- Azure için hazır
- Azure için koşullu olarak hazır
- Azure için hazır değil
- Hazır olma durumu bilinmiyor

Azure Geçişi, hazır olan VM’ler için Azure’da bir VM boyutu önerir. Azure Geçişi’nin yaptığı boyut önerisi, değerlendirme özelliklerinde belirtilen boyutlandırma ölçütüne bağlıdır. Boyutlandırma ölçütü performansa dayalı boyutlandırma ise, sanal makinelerin performans geçmişi (CPU ve bellek) ve diskleri (IOPS ve aktarım hızı) dikkate alınarak boyut önerisinde bulunulur. Boyutlandırma ölçütünün 'şirket içinde olduğu gibi' olması durumunda, Azure Geçişi VM'ler ve diskler için performans verilerini dikkate almaz. Azure'da VM boyutu önerisi, şirket içi VM'nin boyutuna bakılarak yapılır ve disk boyutlandırması da değerlendirme özelliklerinde belirtilen Depolama türü (varsayılan değer premium diskler) temelinde yapılır. Azure Geçişi’nde boyutlandırmanın nasıl yapıldığı hakkında [daha fazla bilgi edinin](concepts-assessment-calculation.md).

Azure için hazır olmayan veya koşullu olarak hazır olan sanal makineler için Azure Geçişi, hazır olma durumu sorunlarını açıklar ve düzeltme adımları sağlar.

Azure Geçişi’nin Azure için hazır olma durumunu belirleyemediği (verilerin kullanılabilir olmaması nedeniyle) sanal makineler, hazır olma durumu bilinmiyor olarak işaretlenir.

Azure Geçişi, Azure için hazır olma ve boyutlandırmaya ek olarak sanal makineyi geçirmek için kullanabileceğiniz araçları da önerir. Bunun için şirket içi ortamın daha kapsamlı bir keşfi gereklidir. Şirket içi makinelere aracılar yükleyerek nasıl daha kapsamlı bir keşif gerçekleştirebileceğiniz hakkında [daha fazla bilgi edinin](how-to-get-migration-tool.md). Şirket içi makinelere aracılar yüklenmemişse [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) kullanılarak lift and shift ile geçiş önerilir. Şirket içi makineye aracılar yüklenmişse Azure Geçişi, makinenin içinde çalışan işlemlere bakar ve makinenin bir veritabanı makinesi olup olmadığını belirler. Makine bir veritabanı makinesiyse geçiş aracı olarak [Azure Veritabanı Geçiş Hizmeti](https://docs.microsoft.com/azure/dms/dms-overview); makine bir veritabanı makinesi değilse geçiş aracı olarak Azure Site Recovery önerilir.

  ![Hazır olma durumu değerlendirmesi](./media/tutorial-assessment-vmware/assessment-suitability.png)  

#### <a name="monthly-cost-estimate"></a>Aylık maliyet tahmini

Bu görünümde, Azure’da çalışan VM’lerin toplam işlem ve depolama maliyetinin yanı sıra her makineye ilişkin ayrıntılar da görüntülenir. Maliyet tahminleri, Azure Geçişi tarafından bir makine, bu makinenin diskleri ve değerlendirme özellikleri için gerçekleştirilen boyut önerileri göz önünde bulundurularak hesaplanır.

> [!NOTE]
> Azure Geçişi tarafından sağlanan maliyet tahmini, hizmet olarak Azure Altyapısı (IaaS) VM’leri biçiminde çalıştırılan şirket içi VM’lerine yöneliktir. Azure Geçişi, Hizmet olarak platform (PaaS) veya Hizmet olarak yazılım (SaaS) maliyetlerini hesaba katmaz.

İşlem ve depolama için tahmini aylık maliyetler gruptaki tüm VM’ler için birleştirilir.

![VM maliyeti değerlendirmesi](./media/tutorial-assessment-vmware/assessment-vm-cost.png)

#### <a name="confidence-rating"></a>Güvenilirlik derecelendirmesi

Azure Geçişi’ndeki her performans tabanlı değerlendirme 1 yıldız ile 5 yıldız (1 yıldız en düşük, 5 yıldız en yüksektir) arasında değişen bir güvenilirlik derecesiyle ilişkilendirilir. Güvenilirlik derecelendirmesi, değerlendirmeyi hesaplamak için gereken veri noktalarının kullanılabilirliği temelinde bir değerlendirmeye atanır. Bir değerlendirmenin güvenilirlik derecesi, Azure Geçişi tarafından sağlanan boyut önerilerinin güvenilirliğini tahmin etmenize yardımcı olur. Güvenilirlik derecelendirmesi için uygulanabilir değil "olarak-olan" Şirket değerlendirmeleri.

Performansa dayalı boyutlandırma için Azure Geçişi, CPU ile VM'nin belleği için kullanım verilerine ihtiyaç duyar. Bunlara ek olarak, VM'ye eklenen her disk için disk IOPS ve aktarım hızı değerleri de gerekir. Sanal makineye eklenmiş her bir ağ bağdaştırıcısı için benzer şekilde Azure Geçişi’nin performansa dayalı boyutlandırma gerçekleştirmek amacıyla ağ giriş/çıkışına ihtiyacı vardır. Yukarıdaki kullanım rakamlarından herhangi biri vCenter Server’da mevcut değilse Azure Geçişi’nin yaptığı boyut önerisi güvenilir olmayabilir. Kullanılabilir veri noktalarının yüzdesine bağlı olarak değerlendirme için aşağıdaki gibi güvenilirlik derecelendirmesi sağlanır:

   **Veri noktalarının kullanılabilirliği** | **Güvenilirlik derecelendirmesi**
   --- | ---
   %0-%20 | 1 Yıldız
   %21-%40 | 2 Yıldız
   %41-%60 | 3 Yıldız
   %61-%80 | 4 Yıldız
   %81-%100 | 5 Yıldız

Aşağıdaki nedenlerle bir değerlendirme için tüm veri noktaları kullanılabilir olmayabilir:

- Değerlendirmeyi oluşturduğunuz süre boyunca ortamınızın profilini oluşturmadınız. Örneğin, değerlendirmeyi, 1 gün olarak ayarlanmış performans süresiyle oluşturuyorsanız, toplanacak tüm veri noktalarının keşfini başlattıktan sonra en az bir gün beklemeniz gerekir.

- Değerlendirmenin hesaplandığı dönem boyunca birkaç sanal makine kapatılmıştır. Herhangi bir VM, belirli bir süre boyunca kapatıldıysa o süreye ait performans verilerine sahip olamayız.

- Değerlendirmenin hesaplandığı dönem boyunca birkaç sanal makine oluşturulmuştur. Örneğin, son bir ayın performans geçmişi için değerlendirme oluşturuyorsanız, ancak yalnızca bir hafta önce ortamda birkaç sanal makine oluşturulduysa. Bu tür durumlarda yeni sanal makinelerin performans geçmişi, sürenin tamamı boyunca mevcut olmaz.

> [!NOTE]
> Herhangi bir değerlendirmenin güvenilirlik derecesi 5 yıldızdan düşükse, gereç ortamı profilini oluşturmak için en az bir gün bekleyin ve ardından *yeniden hesapla* değerlendirme. Bu yapılamazsa, performansa dayalı boyutlandırma güvenilir olmayabilir ve değerlendirme özellikleri değiştirilerek *şirket içi olarak boyutlandırmaya* geçiş yapılması önerilir.

## <a name="next-steps"></a>Sonraki adımlar

- Bir değerlendirmeyi gereksinimleriniz temelinde nasıl özelleştirebileceğinizi [öğrenin](how-to-modify-assessment.md).
- [Makine bağımlılık eşlemesi](how-to-create-group-machine-dependencies.md) kullanarak yüksek güvenilirlikli değerlendirme grubu oluşturmayı öğrenin
- Değerlendirmelerin nasıl hesaplandığı hakkında [daha fazla bilgi](concepts-assessment-calculation.md) edinin.
- Geniş bir VMware ortamında bulma ve değerlendirme işlemlerinin nasıl yapılacağını [öğrenin](how-to-scale-assessment.md).
- Azure Geçişi’ne ilişkin SSS hakkında [daha fazla bilgi edinin](resources-faq.md)
