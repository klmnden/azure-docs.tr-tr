---
title: Ölçeklendirme Keşif ve değerlendirme Azure Geçişi'ni kullanarak | Microsoft Docs
description: Azure geçişi hizmetini kullanarak şirket içi makinelerin çok sayıda değerlendirmek açıklar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 04/04/2019
ms.author: raynew
ms.openlocfilehash: ae84313cd750e3d6c7eb9443ec59095dec9c632e
ms.sourcegitcommit: b4ad15a9ffcfd07351836ffedf9692a3b5d0ac86
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59057481"
---
# <a name="discover-and-assess-a-large-vmware-environment"></a>Büyük bir VMware ortamını bulma ve değerlendirme

Azure geçişi, proje başına 1500 makineyi sınırı vardır, bu makalede kullanarak çok sayıda şirket içi sanal makineleri (VM'ler) değerlendirmek nasıl [Azure geçişi](migrate-overview.md).

> [!NOTE]
> Tek bir gereç kullanılarak tek bir projede en fazla 10.000 VMware Vm'lerinin bulunmasını sağlayan bir önizleme sürümünü kullanıma sunuyoruz, deniyorsanız düşünüyorsanız, lütfen kaydolun [burada.](https://aka.ms/migratefuture)

## <a name="prerequisites"></a>Önkoşullar

- **VMware**: Geçirmeyi planladığınız sanal makineleri, vCenter Server sürüm 5.5, 6.0, 6.5 veya 6.7 tarafından yönetilmelidir. Ayrıca, 5.5 veya sonraki Toplayıcı VM'yi dağıtmak için bir ESXi ana çalışan sürümü gerekir.
- **vCenter hesabı**: VCenter Server'a erişmek için salt okunur bir hesap gerekir. Azure Geçişi, şirket içi VM’leri bulmak için bu hesabı kullanır.
- **İzinleri**: VCenter Server'da, bir dosyayı OVA biçiminde içeri aktararak VM oluşturma izni gerekir.
- **İstatistik ayarları**: Bu gereksinim yalnızca geçerlidir [tek seferlik modeli](https://docs.microsoft.com/azure/migrate/concepts-collector) kullanım dışı şimdi. Dağıtımı başlatmadan önce tek seferlik modeli için vCenter Server için istatistik ayarları düzeyini 3 ayarlanması gerekir. İstatistik düzeyini 3 olarak her gün, haftanın günü ve ay toplama aralıkları için ayarlamanız sağlamaktır. Düzeyi üç toplama aralıkları için 3'ten daha düşük olan, değerlendirme çalışır, ancak depolama ve ağ için performans verileri toplanmaz. Boyut önerileri, CPU ve bellek için performans verilerini ve disk ve ağ bağdaştırıcıları için yapılandırma verilerini ardından hesaplanır.

> [!NOTE]
> Bu yöntem, vCenter Server'ın performans veri noktası kullanılabilirlik için istatistik ayarları yararlandı ve sanal makinelerin Azure'a geçiş için eksik boyutlandırma içinde sonuçlanan ortalama performans sayaçlarının toplanan gibi tek seferlik gereç artık kullanım dışı bırakılmıştır.

### <a name="set-up-permissions"></a>İzinleri ayarlama

Azure Geçişi’nin, değerlendirme amacıyla VM’leri otomatik olarak bulması için VMware sunucularına erişebilmesi gerekir. VMware hesap için şu izinler gereklidir:

- Kullanıcı türü: En az bir salt okunur kullanıcı
- İzinler: Veri Merkezi nesnesi –> Alt Nesneye Yay, role=Read-only
- Ayrıntılar: Kullanıcı veri merkezi düzeyinde atandı ve bu veri merkezindeki tüm nesnelere erişimi var.
- Erişimi kısıtlamak için Alt nesneye yay ile Erişim yok rolünü alt nesnelere (vSphere konakları, veri depoları, VM’ler ve ağlar) atayın.

Bir kiracı ortamda dağıtıyorsanız, bunu ayarlamak için yöntemlerinden biri aşağıda verilmiştir:

1. Her Kiracı ve kullanarak bir kullanıcı oluşturmak [RBAC](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal), belirli bir kiracıya ait tüm sanal makineler için salt okunur izinler atayın. Ardından, bu kimlik bilgilerinin bulma için kullanın. RBAC, karşılık gelen bir vCenter kullanıcı yalnızca kiracıya özgü Vm'leri erişimi olmasını sağlar.
2. Aşağıdaki örnekte, kullanıcı #1 ve 2 numaralı kullanıcı için açıklandığı gibi farklı Kiracı kullanıcılar için RBAC ayarlayın:

    - İçinde **kullanıcı adı** ve **parola**, Toplayıcının içinde Vm'leri bulmak için kullanacağı salt okunur hesabın kimlik bilgilerini belirtin
    - Datacenter1 - kullanıcı #1 ve 2 numaralı Kullanıcı salt okunur izinleri verin. Tüm alt nesneleri için bu izinleri, tek VM'ler üzerinde izinler çünkü yay yok.

      - VM1 (Kiracı #1) (salt okunur izni kullanıcı # 1)
      - VM2 (Kiracı #1) (salt okunur izni kullanıcı # 1)
      - VM3 (Kiracı #2) (salt okunur izni kullanıcı # 2)
      - VM4 (Kiracı #2) (salt okunur izni kullanıcı # 2)

   - 1 kullanıcı kimlik bilgilerini kullanarak bulma gerçekleştirirseniz, yalnızca VM1 ve VM2 bulunacaktır.

## <a name="plan-your-migration-projects-and-discoveries"></a>Geçiş projeleri ve bulmaları planlama

Vm'leri bulmak için planlama sayısına bağlı olarak, birden çok proje oluşturur ve ortamınızda birden çok gereçlerini dağıtma. (Bulma durdurup parçalarından sürece) tek bir vCenter Server ve tek bir proje için bir gereç bağlanabilir.

(Artık kullanım dışı), tek seferlik bir bulma bulma yangın içinde çalışır ve bir bulma tamamlandıktan sonra modeli unutmanız durumunda, farklı bir vCenter sunucusu veri toplamak veya farklı bir geçiş projesine göndermek için aynı Toplayıcısı'nı kullanabilirsiniz.

> [!NOTE]
> Bu yöntem, vCenter Server'ın performans veri noktası kullanılabilirlik için istatistik ayarları yararlandı ve sanal makinelerin Azure'a geçiş için eksik boyutlandırma içinde sonuçlanan ortalama performans sayaçlarının toplanan gibi tek seferlik gereç artık kullanım dışı bırakılmıştır. Tek seferlik gerecine taşımanız önerilir.

Bulma ve değerlendirme şu sınırlara göre planlayın:

| **Varlık** | **Makine sınırı** |
| ---------- | ----------------- |
| Project    | 1.500             |
| Bulma  | 1.500             |
| Değerlendirme | 1.500             |

Bu planlama konuları göz önünde bulundurun:

- Azure geçişi Toplayıcısı'nı kullanarak bir bulma işlemi gerçekleştirdiğinizde, bulma kapsamı bir vCenter Server klasörü, veri merkezi, küme veya konağı ayarlayabilirsiniz.
- Vcenter Server'daki, bulmak istediğiniz VM'lerin klasörleri, veri merkezleri, kümeler veya 1.500 makineler SORUMLULUĞUN destekleyen konakların aynı vCenter Server'dan birden fazla keşif yapmak için doğrulayın.
- Değerlendirme amacıyla, değerlendirme ve aynı proje içinde bağımlılıklarını makinelerle tutmanızı öneririz. VCenter Server'da, bağımlı makinelere/klasör, veri merkezi veya küme değerlendirmesi için olduğundan emin olun.

Senaryonuza bağlı olarak, aşağıda belirtilen şekilde, bulmalar ayırabilirsiniz:

### <a name="multiple-vcenter-servers-with-less-than-1500-vms"></a>Birden fazla vCenter sunucuları ile 1500'den az VM'ler
Ortamınızda birden fazla vCenter sunucuları varsa ve sanal makinelerin toplam sayısını 1500'den az ise, senaryo temel alınarak aşağıdaki yaklaşımı kullanabilirsiniz:

**Sürekli bulma:** Sürekli bulma olması durumunda, yalnızca tek bir projeye bir gereç bağlanabilir. Bu nedenle her biri, vCenter sunucuları için bir gereç dağıtın ve ardından uygun şekilde her Gereci ve bulmalar tetikleyici için bir proje oluşturun gerekir.

**Tek seferlik bulma (artık kullanım dışı):** Tüm sanal makineler arasında tüm vCenter sunucularını bulmak için tek bir Toplayıcı ve tek bir geçiş projesi kullanabilirsiniz. Tek seferlik Toplayıcı, bir kerede bir vCenter Server bulur olduğundan, aynı Toplayıcı tüm vCenter karşı sunucuları, birbiri ardına çalıştırın ve Toplayıcı aynı geçiş projeye işaret edecek. Bulma tamamlandıktan sonra makineler değerlendirmeler oluşturabilirsiniz.


### <a name="multiple-vcenter-servers-with-more-than-1500-vms"></a>Birden fazla vCenter sunucuları ile 1500'den fazla VM

Tüm vCenter sunucuları arasında birden fazla vCenter sunucuları ile 1500'den küçük sanal makine vCenter sunucusu sayısı, ancak 1500'den fazla VM varsa, (bir geçiş projesi yalnızca 1500 Vm'leri barındırmak) birden çok geçiş projeleri oluşturmanız gerekir. Bu vCenter Server her bir geçiş projesi oluşturma ve bulmaları bölerek elde edebilirsiniz.

**Sürekli bulma:** Birden fazla Toplayıcı cihazları (her bir vCenter sunucusu için bir tane) oluşturun ve her Gereci bir proje ve tetikleyici bulma için uygun şekilde bağlantı gerekir.

**Tek seferlik bulma (artık kullanım dışı):** Tek bir Toplayıcı, her bir vCenter Server (birbiri ardına) bulmak için kullanabilirsiniz. Aynı anda başlatmak için bulmaları istiyorsanız, birden çok gereçlerini dağıtma ve bulmaları paralel olarak çalıştırmak.

### <a name="more-than-1500-machines-in-a-single-vcenter-server"></a>Tek bir vcenter Server 1500'den fazla makineler

Tek bir vCenter Server'da 1500'den fazla sanal makineleriniz varsa, birden çok geçiş projelere bulma bölmek gerekir. Bulmaları bölmek için gereç kapsam alanı yararlanın ve konağa, küme, klasör veya bulmak istediğiniz veri merkezinde belirtin. VCenter sunucusu ile 1000 ile de iki klasör varsa, örneğin, VM'ler (Klasör1) ve diğer 800 VM (klasör2), bu klasörleri arasındaki bulmaları bölmek için kapsam alanı kullanabilirsiniz.

**Sürekli bulma:** Bu durumda, ilk toplayıcı için iki Toplayıcı Gereçleri, oluşturma, kapsam Klasör1 belirtin ve için ilk geçiş projenizi bağlamak gerekir. Paralel olarak yapabilecekleriniz ikinci Toplayıcı gerecini kullanarak klasör2 bulmayı Başlat ve ikinci geçiş projeye bağlanın.

**Tek seferlik bulma (artık kullanım dışı):** Her iki bulmaları tetiklemek için aynı Toplayıcısı'nı kullanabilirsiniz. İlk bulma kapsamı olarak Klasör1 belirtin ve noktası ilk geçiş projenizi ilk keşif tamamlandıktan sonra aynı Toplayıcı, klasör2 ve geçiş proje ayrıntılarını ikinci geçiş projesi için kapsamı değiştirin ve İkinci keşif yapın.

### <a name="multi-tenant-environment"></a>Çok kiracılı ortam

Kiracılar genelinde paylaşılan bir ortamda varsa ve bir kiracının başka bir kiracının Abonelikteki sanal makinelerin keşfetmek istiyor musunuz, bulma kapsamı için toplayıcı gerecini kapsam alanı kullanabilirsiniz. Kiracıların konaklar paylaşıyorsanız, yalnızca belirli kiracıya ait sanal makinelerin salt okunur erişimi olan bir kimlik bilgisi oluşturmak ve ardından bu kimlik bilgisi Toplayıcı Gereci kullanabilir ve keşif yapmak için ana bilgisayarı olarak kapsamını belirtin.

## <a name="discover-on-premises-environment"></a>Şirket içi ortamı bulma

Planınızla hazır olduktan sonra şirket içi sanal makineleri bulma başlatabilirsiniz:

### <a name="create-a-project"></a>Proje oluşturma

Gereksinimlerinize uygun olarak bir Azure geçişi projesi oluşturun:

1. Azure portalda **Kaynak oluştur**’u seçin.
2. **Azure Geçişi** araması yapın ve arana sonuçlarında **Azure Geçişi** hizmetini seçin. Ardından **Oluştur**’u seçin.
3. Bir proje adı ve proje için Azure aboneliği belirtin.
4. Yeni bir kaynak grubu oluşturun.
5. Projeyi oluşturmak ve ardından istediğiniz konumu belirtin **Oluştur**. Sanal makineleriniz için farklı bir konuma hala değerlendirebilirsiniz unutmayın. Proje için belirtilen konum, şirket içi Vm'lerden toplanan meta verileri depolamak için kullanılır.

### <a name="set-up-the-collector-appliance"></a>Toplayıcı gerecini ayarlamak

Azure Geçişi, toplayıcı gereci olarak bilinen bir şirket içi VM oluşturur. Bu VM, şirket içi VMware Vm'lerini bulur ve bunlar hakkındaki meta veriler Azure geçişi hizmetine gönderir. Toplayıcı gerecini ayarlamak için bir OVA dosyasını indirmek ve şirket içi vCenter sunucusu örneğine içe aktarın.

#### <a name="download-the-collector-appliance"></a>Toplayıcı gerecini indirin

Birden çok proje varsa, vCenter Server'a yalnızca bir kez Toplayıcı gerecini indirin gerekir. İndirin ve gerecini ayarlamak sonra her proje için çalıştırın ve benzersiz proje Kimliğini ve anahtarını belirtin.

1. Azure Geçişi projesinde **Kullanmaya Başlama** > **Bul ve Değerlendir** > **Makineleri Keşfet**’ye tıklayın.
2. İçinde **makineleri Bul**, tıklayın **indirin** gereç indirilemedi.

    Azure geçişi Gereci vCenter Server ile iletişim kurar ve sürekli olarak şirket içi ortamda, her VM için gerçek zamanlı kullanım verilerini toplamak için profil. Bu, her bir ölçüm (CPU kullanımı, bellek kullanımı vb.) için en yüksek sayaçları toplar. Bu model, performans verilerinin toplanması için vCenter Server’ın istatistik ayarlarına bağlı değildir. Sürekli profil oluşturmayı aletten istediğiniz zaman durdurabilirsiniz.

    > [!NOTE]
    > Bu yöntem, vCenter Server'ın performans veri noktası kullanılabilirlik için istatistik ayarları yararlandı ve sanal makinelerin Azure'a geçiş için eksik boyutlandırma içinde sonuçlanan ortalama performans sayaçlarının toplanan gibi tek seferlik gereç artık kullanım dışı bırakılmıştır.

    **Anında keyif:** Bulma olduğunda (alır birkaç saat VM sayısına bağlı olarak), sürekli bulma gereciyle tamamlamak değerlendirmeler hemen oluşturabilirsiniz. Anında sonuç elde etmek için kullanmak istiyorsanız, Keşif yaslanıp performans veri toplama başlar bu yana değerlendirmede boyutlandırma ölçütü seçmelisiniz *şirket içi olarak*. Performans tabanlı değerlendirmeleri için en az bir gün sonra güvenilir boyut önerileri almak için keşif başlatılmadan için beklemeniz önerilir.

    Gerecin performans verilerini yalnızca sürekli olarak topladığını unutmayın, şirket içi ortamdaki (ör. VM eklemesi, silmesi, disk eklemesi vb.) hiçbir yapılandırma değişikliğini algılamaz. Şirket içi ortamda bir yapılandırma değişikliği gerçekleşirse değişikliklerin portala yansıması için aşağıdakileri yapabilirsiniz:

    - Ayrıca öğeleri (VM'ler, diskler ve çekirdek vb.): Azure portalında bu değişiklikleri yansıtacak şekilde gereç keşiften durdurun ve yeniden başlatın. Bu, değişikliklerin Azure Geçişi projesinde güncelleştirilmesini sağlar.

    - VM silme: Bulma durdurup bile gereç tasarlandığı şekilde nedeniyle, VM'ler silinmesini yansıtılmaz. Bunun nedeni takip eden keşiflerin eski keşiflerin üzerine yazılması yerine bunlara eklenmesidir. Bu durumda grubunuzdan kaldırarak ve değerlendirmeyi yeniden hesaplayarak portaldaki VM’yi yoksayabilirsiniz.

3. İçinde **proje kimlik bilgilerini kopyalama**, kopya kimliği ve anahtarı için proje. Toplayıcıyı yapılandırırken bu bilgilere ihtiyaç duyarsınız.


#### <a name="verify-the-collector-appliance"></a>Toplayıcı gereci doğrulama

OVA dosyasını dağıtmadan önce güvenli olup olmadığını denetleyin:

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.

2. OVA’nın karmasını oluşturmak için aşağıdaki komutu çalıştırın:

   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

   Örnek Kullanım: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```

3. Oluşturulan karma aşağıdaki ayarları eşleştiğinden emin olun.

#### <a name="continuous-discovery"></a>Sürekli keşif

OVA sürüm 1.0.10.4 için

**Algoritma** | **Karma değeri**
--- | ---
MD5 | 2ca5b1b93ee0675ca794dd3fd216e13d
SHA1 | 8c46a52b18d36e91daeae62f412f5cb2a8198ee5
SHA256 | 3b3dec0f995b3dd3c6ba218d436be003a687710abab9fcd17d4bdc90a11276be

#### <a name="one-time-discovery-deprecated-now"></a>Tek seferlik bulma (artık kullanım dışı)

OVA sürüm 1.0.9.15 (23/10/2018 tarihinde serbest bırakılmış)

**Algoritma** | **Karma değeri**
--- | ---
MD5 | e9ef16b0c837638c506b5fc0ef75ebfa
SHA1 | 37b4b1e92b3c6ac2782ff5258450df6686c89864
SHA256 | 8a86fc17f69b69968eb20a5c4c288c194cdcffb4ee6568d85ae5ba96835559ba

OVA sürüm 1.0.9.14 (24/8/2018 tarihinde serbest bırakılmış)

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

OVA sürüm 1.0.9.8 için

**Algoritma** | **Karma değeri**
--- | ---
MD5 | b5d9f0caf15ca357ac0563468c2e6251
SHA1 | d6179b5bfe84e123fabd37f8a1e4930839eeb0e5
SHA256 | 09c68b168719cb93bd439ea6a5fe21a3b01beec0e15b84204857061ca5b116ff

OVA sürüm 1.0.9.7 için

**Algoritma** | **Karma değeri**
--- | ---
MD5 | d5b6a03701203ff556fa78694d6d7c35
SHA1 | f039feaa10dccd811c3d22d9a59fb83d0b01151e
SHA256 | e5e997c003e29036f62bf3fdce96acd4a271799211a84b34b35dfd290e9bea9c

### <a name="create-the-collector-vm"></a>Toplayıcı VM’yi oluşturma

İndirilen dosyayı vCenter Server'a aktarın:

1. VSphere Client konsolunda seçin **dosya** > **OVF şablonu Dağıt**.

    ![OVF dağıtma](./media/how-to-scale-assessment/vcenter-wizard.png)

2. OVF şablonu Dağıtma Sihirbazı > **kaynak**, OVA dosyasının konumunu belirtin.
3. **Ad** ve **Konum** bölümünde toplayıcı VM için bir kolay ad ve VM’nin barındırılacağı envanter nesnesini belirtin.
4. **Konak/Küme** bölümünde, toplayıcı VM’nin çalıştırılacağı konağı veya kümeyi belirtin.
5. Depolama’da, toplayıcı VM için depolama hedefini belirleyin.
6. **Disk Biçimi**’nde disk türünü ve boyutunu belirtin.
7. **Ağ Eşleme** bölümünde toplayıcı VM’nin bağlanacağı ağı belirtin. Ağ meta verileri Azure'a göndermek için internet bağlantısı gerekir.
8. Gözden geçirin ve ayarları onaylayın ve ardından **son**.

### <a name="identify-the-id-and-key-for-each-project"></a>Kimliği tanımlayın ve her proje için anahtar

Birden çok proje varsa, Kimliğini belirlemek ve her biri için anahtar emin olun. Vm'leri bulmak için toplayıcıyı çalıştırdığınızda anahtarı gerekir.

1. Projede **Başlarken** > **Keşif ve değerlendirme** > **makineleri Bul**.
2. İçinde **proje kimlik bilgilerini kopyalama**, kopya kimliği ve anahtarı için proje.
    ![Proje kimlik bilgilerini kopyalayın](./media/how-to-scale-assessment/copy-project-credentials.png)

### <a name="run-the-collector-to-discover-vms"></a>VM’leri bulmak için toplayıcıyı çalıştırma

Gerçekleştirmeniz gereken her bulma için gerekli kapsam içindeki Vm'leri bulmak için toplayıcıyı çalıştırın. Bir bulma art arda çalıştırın. Eş zamanlı bulmalar desteklenmez ve her bir keşfin farklı bir kapsama sahip olmalıdır.

1.  vSphere Client konsolunda VM’ye sağ tıklayın ve **Konsolu Aç** seçeneğini belirleyin.
2.  Gereç için dil, saat dilimi ve parola tercihlerini belirtin.
3.  Masaüstünde seçin **Toplayıcı Çalıştır** kısayol.
4.  Azure geçişi toplayıcısı açın **önkoşulları ayarlama** ve ardından:

    a. Lisans koşullarını kabul edin ve üçüncü taraf bilgilerini okuyun.

    Toplayıcı, VM’nin İnternet erişimine sahip olup olmadığını denetler.

    b. VM Internet proxy üzerinden erişirse, seçin **Proxy ayarlarını**, Ara sunucu adresi ve dinleme bağlantı noktasını belirtin. Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin.

    Toplayıcı, toplayıcı hizmetinin çalışıp çalışmadığını denetler. Hizmet, toplayıcı VM’ye varsayılan olarak yüklenir.

    c. VMware PowerCLI’yı indirin ve yükleyin.

5.  **vCenter Server bilgilerini belirtin** bölümünde şunları yapın:
    - Adı (FQDN) veya vCenter Server'ın IP adresi belirtin.
    - İçinde **kullanıcı adı** ve **parola**, Toplayıcının vCenter Server'da Vm'leri bulmak için kullanacağı salt okunur hesabın kimlik bilgilerini belirtin.
    - **Kapsam seçin** bölümünde, sanal makine bulma için bir kapsam seçin. Toplayıcı yalnızca belirtilen kapsam içindeki Vm'leri bulabilir. Kapsam belirli bir klasör, veri merkezi veya küme olarak ayarlanabilir. Bu 1. 000'den fazla VM içermelidir.

6.  İçinde **geçişi projesini belirtin**anahtarı için proje ve Kimliğini belirtin. Kopyaladığınız alamadık, Toplayıcı VM'den Azure portalını açın. Projenin **genel bakış** sayfasında **makineleri Bul** ve değerleri kopyalayın.  
7.  İçinde **toplama durumunu görüntüle**, Keşif sürecini izleyebilir ve Vm'lerden toplanan meta verilerin kapsam içinde olup olmadığını denetleyin. Toplayıcı, yaklaşık bir bulma süresi sağlar.

#### <a name="verify-vms-in-the-portal"></a>VM’lerin portalda olup olmadığını doğrulama

Toplayıcı sürekli olarak şirket içi ortamda profil ve performans verilerini bir saatlik zaman aralığı içinde göndermeye devam. Keşfin başlamasından bir saat sonra makineleri portalda inceleyebilirsiniz. VM’ler için performansa dayalı değerlendirmeler oluşturmadan önce en az bir gün beklemeniz önerilir.

1. Geçiş projesinde **Yönet** > **Makineler**’e tıklayın.
2. Bulmak istediğiniz VM’lerin portalda görüntülenip görüntülenmediğini kontrol edin.

### <a name="data-collected-from-on-premises-environment"></a>Şirket içi ortamdan toplanan veriler

Toplayıcı gerecini seçili sanal makineler hakkında aşağıdaki yapılandırma verilerini bulur.

1. VM görünen adı (temel, vCenter)
2. Sanal makinenin envanteri yolu (konak/klasör vcenter)
3. IP adresi
4. MAC adresi
5. İşletim sistemi
5. Çekirdek, disk, NIC sayısı
6. Bellek boyutu, Disk boyutları
7. Ve VM, disk ve aşağıdaki tabloda listelendiği gibi ağ performans sayaçları.

Toplayıcı gerecini 20 saniyelik bir aralıkta ESXi konağından her VM için aşağıdaki performans sayaçlarını toplar. Bu sayaçlardan vCenter sayaçları ve terminolojiyi ortalama diyor olsa da, 20 saniye örnekleri gerçek zamanlı sayaçları. Gereç ardından pay en yüksek değeri 20 saniye örnekleri seçerek boyunca 15 dakikada bir tek veri noktası oluşturmak için 20 saniye örnekleri yukarı ve Azure'a gönderir. VM'ler için performans verilerini iki saat sonra keşif devreye girdi portalda kullanılabilir hale gelmeden başlatır. İçin en az doğru doğru boyutlandırma önerilerini almak için Değerlendirmeler performans tabanlı oluşturmadan önce bir gün beklemeniz önerilir. Anında sonuç elde etmek için arıyorsanız, boyutlandırma ölçütü ile değerlendirmeler oluşturabilirsiniz *şirket içi olarak* hangi değil dikkate alınır doğru boyutlandırma için performans verileri.

**Sayaç** |  **Etki değerlendirmesi**
--- | ---
cpu.usage.average | Önerilen VM boyutu ve maliyet  
mem.Usage.average | Önerilen VM boyutu ve maliyet  
virtualDisk.read.average | Disk boyutu, depolama maliyeti, VM boyutunu hesaplar
virtualDisk.write.average | Disk boyutu, depolama maliyeti, VM boyutunu hesaplar
virtualDisk.numberReadAveraged.average | Disk boyutu, depolama maliyeti, VM boyutunu hesaplar
virtualDisk.numberWriteAveraged.average | Disk boyutu, depolama maliyeti, VM boyutunu hesaplar
NET.Received.average | VM boyutunu hesaplar                          
NET.transmitted.average | VM boyutunu hesaplar     

> [!WARNING]
> VCenter Server'ın performans verilerini toplama için istatistik ayarları yararlandı tek seferlik bir bulma yöntemi artık kullanılmıyor.

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [bir grup oluşturmak](how-to-create-a-group.md) değerlendirmesi için.
- Değerlendirmelerin nasıl hesaplandığı hakkında [daha fazla bilgi](concepts-assessment-calculation.md) edinin.
