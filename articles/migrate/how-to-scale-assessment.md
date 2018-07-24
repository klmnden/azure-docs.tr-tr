---
title: Ölçeklendirme Keşif ve değerlendirme Azure Geçişi'ni kullanarak | Microsoft Docs
description: Azure geçişi hizmetini kullanarak şirket içi makinelerin çok sayıda değerlendirmek açıklar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: raynew
ms.openlocfilehash: 4bcb6734c33d70e4045860a2c0f0acfedfa06eff
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39215188"
---
# <a name="discover-and-assess-a-large-vmware-environment"></a>Büyük bir VMware ortamını bulma ve değerlendirme

Azure geçişi, proje başına 1500 makineyi sınırı vardır, bu makalede kullanarak çok sayıda şirket içi sanal makineleri (VM'ler) değerlendirmek nasıl [Azure geçişi](migrate-overview.md).   

## <a name="prerequisites"></a>Önkoşullar

- **VMware**: geçirmeyi planladığınız VM'ler, vCenter Server sürüm 5.5, 6.0 veya 6.5 ile yönetilmelidir. Ayrıca, 5.0 veya üzeri Toplayıcı VM'yi dağıtmak için bir ESXi ana çalışan sürümü gerekir.
- **vCenter hesabı**: vCenter Server'a erişmek için salt okunur bir hesabınız olması gerekir. Azure Geçişi, şirket içi VM’leri bulmak için bu hesabı kullanır.
- **İzinleri**: vCenter Server'da, bir dosyayı OVA biçiminde içeri aktararak VM oluşturma için izinlerinizin olması gerekir.
- **İstatistik ayarları**: dağıtımı başlatmadan önce vCenter Server için istatistik ayarları düzeyini 3 ayarlanması gerekir. Düzey 3'ten daha düşükse, değerlendirme çalışır, ancak depolama ve ağ için performans verileri toplanmaz. Boyut önerileri, CPU ve bellek için performans verilerini ve disk ve ağ bağdaştırıcıları için yapılandırma verilerini bu durumda hesaplanır.


### <a name="set-up-permissions"></a>İzinleri ayarlama

Azure Geçişi’nin, değerlendirme amacıyla VM’leri otomatik olarak bulması için VMware sunucularına erişebilmesi gerekir. VMware hesap için şu izinler gereklidir:

- Kullanıcı türü: En azından salt okunur bir kullanıcı
- İzinler: Veri Merkezi nesnesi –> Alt Nesneye Yay, rol=Salt okunur
- Ayrıntılar: Veri merkezi düzeyinde atanmış ve veri merkezindeki tüm nesnelere erişimi olan kullanıcı.
- Erişimi kısıtlamak için Alt nesneye yay ile Erişim yok rolünü alt nesnelere (vSphere konakları, veri depoları, VM’ler ve ağlar) atayın.

Bir kiracı ortamda dağıtıyorsanız, bunu ayarlamak için yöntemlerinden biri aşağıda verilmiştir:

1.  Her Kiracı ve kullanarak bir kullanıcı oluşturmak [RBAC](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal), belirli bir kiracıya ait sanal makinenin salt okunur izinler atayın. Ardından, bu kimlik bilgilerinin bulma için kullanın. RBAC, karşılık gelen bir vCenter kullanıcı erişimi yalnızca belirli sanal makinenin Kiracı olmasını sağlar.
2. Aşağıdaki örnekte, kullanıcı #1 ve 2 numaralı kullanıcı için açıklandığı gibi farklı Kiracı kullanıcılar için RBAC ayarlayın:

    - İçinde **kullanıcı adı** ve **parola**, Toplayıcının içinde Vm'leri bulmak için kullanacağı salt okunur hesabın kimlik bilgilerini belirtin
    - Datacenter1 - kullanıcı #1 ve 2 numaralı Kullanıcı salt okunur izinleri verin. Tek VM izinleri çünkü tüm alt nesneleri için bu izinleri yay yok.

      - VM1 (Kiracı #1) (#1 kullanıcı okuma yalnızca izni)
      - VM2 (Kiracı #1) (#1 kullanıcı okuma yalnızca izni)
      - VM3 (Kiracı #2) (2 kullanıcı okuma yalnızca izni)
      - VM4 (Kiracı #2) (2 kullanıcı okuma yalnızca izni)

   - 1 kullanıcı kimlik bilgilerini kullanarak bulma gerçekleştirirseniz, yalnızca VM1 ve VM2 bulunacaktır.

## <a name="plan-your-migration-projects-and-discoveries"></a>Geçiş projeleri ve bulmaları planlama

Tek bir Azure geçişi toplayıcısı birden fazla vCenter sunucuları keşiften (birbiri ardına) destekler ve aynı zamanda birden fazla geçiş projeleri için bulma (birbiri ardına) destekler. Toplayıcı yangın içinde çalışır ve unut modeli, bir bulma yaptıktan sonra farklı bir vCenter sunucusu veri toplamak veya farklı bir geçiş projesine göndermek için aynı Toplayıcı kullanabilirsiniz.

Bulma ve değerlendirme şu sınırlara göre planlayın:

| **Varlık** | **Makine sınırı** |
| ---------- | ----------------- |
| Project    | 1,500             |
| Bulma  | 1,500             |
| Değerlendirme | 1,500             |

Bu planlama konuları göz önünde bulundurun:

- Azure geçişi Toplayıcısı'nı kullanarak bir bulma işlemi gerçekleştirdiğinizde, bulma kapsamı bir vCenter Server klasörü, veri merkezi, küme veya konağı ayarlayabilirsiniz.
- Vcenter Server'daki klasörleri, veri merkezleri, kümeler veya 1.500 makineler SORUMLULUĞUN destekleyen konakların bulmak istediğiniz VM'lerin olan birden fazla keşif yapmak için doğrulayın.
- Değerlendirme amacıyla, değerlendirme ve aynı proje içinde bağımlılıklarını makinelerle tutmanızı öneririz. VCenter Server'da, bağımlı makinelere/klasör, veri merkezi veya küme değerlendirmesi için olduğundan emin olun.

Senaryonuza bağlı olarak, aşağıda belirtilen şekilde, bulmalar ayırabilirsiniz:

### <a name="multiple-vcenter-servers-with-less-than-1500-vms"></a>Birden fazla vCenter sunucuları ile 1500'den az VM'ler

Ortamınızda birden fazla vCenter sunucuları varsa ve sanal makinelerin toplam sayısını 1500'den az ise tüm sanal makineler arasında tüm vCenter sunucularını keşfetmek için tek bir Toplayıcı ve tek bir geçiş projesi kullanabilirsiniz. Toplayıcı, bir kerede bir vCenter Server bulur olduğundan, aynı Toplayıcı tüm vCenter karşı sunucuları, birbiri ardına çalıştırın ve Toplayıcı aynı geçiş projeye işaret edecek. Bulma tamamlandıktan sonra makineler değerlendirmeler oluşturabilirsiniz.

### <a name="multiple-vcenter-servers-with-more-than-1500-vms"></a>Birden fazla vCenter sunucuları ile 1500'den fazla VM

Tüm vCenter hizmet etmesi arasında birden fazla vCenter sunucuları ile 1500'den küçük sanal makine vCenter sunucusu sayısı, ancak 1500'den fazla VM varsa, (bir geçiş projesi yalnızca 1500 Vm'leri barındırmak) birden çok geçiş projeleri oluşturmanız gerekir. Bu vCenter Server her bir geçiş projesi oluşturma ve bulmaları bölerek elde edebilirsiniz. Tek bir Toplayıcı, her bir vCenter Server (birbiri ardına) bulmak için kullanabilirsiniz. Aynı anda başlatmak için bulmaları istiyorsanız, birden çok gereçlerini dağıtma ve bulmaları paralel olarak çalıştırmak.

### <a name="more-than-1500-machines-in-a-single-vcenter-server"></a>Tek bir vcenter Server 1500'den fazla makineler

Tek bir vCenter Server'da 1500'den fazla sanal makineleriniz varsa, birden çok geçiş projelere bulma bölmek gerekir. Bulmaları bölmek için gereç kapsam alanı yararlanın ve konağa, küme, klasör veya bulmak istediğiniz veri merkezinde belirtin. VCenter sunucusu ile 1000 ile de iki klasör varsa, örneğin, Vm'leri (Klasör1) ve diğer 800 VM (klasör2) ile tek bir Toplayıcı ve iki bulmalar gerçekleştirmek kullanabilirsiniz. İlk bulma kapsamı olarak Klasör1 belirtin ve noktası ilk geçiş projenizi ilk keşif tamamlandıktan sonra aynı Toplayıcı, klasör2 ve geçiş proje ayrıntılarını ikinci geçiş projesi için kapsamı değiştirin ve İkinci keşif yapın.

### <a name="multi-tenant-environment"></a>Çok kiracılı ortam

Kiracılar genelinde paylaşılan bir ortamda varsa ve bir kiracının başka bir kiracının Abonelikteki sanal makinelerin keşfetmek istiyor musunuz, bulma kapsamı için toplayıcı gerecini kapsam alanı kullanabilirsiniz. Kiracıların konaklar paylaşıyorsanız, yalnızca belirli kiracıya ait sanal makinelerin salt okunur erişimi olan bir kimlik bilgisi oluşturmak ve ardından bu kimlik bilgisi Toplayıcı Gereci kullanabilir ve keşif yapmak için ana bilgisayarı olarak kapsamını belirtin. Alternatif olarak, vcenter Server (Klasör1 tenant1 için ve klasör2 tenant2 için diyelim), paylaşılan konak altında klasör oluşturabilirsiniz tenant2 ve Klasör1 içine tenant1 için Vm'leri klasör2 taşıyın ve bulmaları, Toplayıcı'ı uygun şekilde kapsam uygun bir klasör belirterek.

## <a name="discover-on-premises-environment"></a>Şirket içi ortamı bulma

Planınızla hazır olduktan sonra şirket içi sanal makineleri bulma başlatabilirsiniz:

### <a name="create-a-project"></a>Proje oluşturma

Gereksinimlerinize uygun olarak bir Azure geçişi projesi oluşturun:

1. Azure portalında **kaynak Oluştur**.
2. **Azure Geçişi** için arama yapın ve arama sonuçlarında **Azure Geçişi (önizleme)** seçeneğini belirleyin. Ardından **Oluştur**’u seçin.
3. Bir proje adı ve proje için Azure aboneliği belirtin.
4. Yeni bir kaynak grubu oluşturun.
5. Projeyi oluşturmak ve ardından istediğiniz konumu belirtin **Oluştur**. Sanal makineleriniz için farklı bir konuma hala değerlendirebilirsiniz unutmayın. Proje için belirtilen konum, şirket içi Vm'lerden toplanan meta verileri depolamak için kullanılır.

### <a name="set-up-the-collector-appliance"></a>Toplayıcı gerecini ayarlamak

Azure Geçişi, toplayıcı gereci olarak bilinen bir şirket içi VM oluşturur. Bu VM, şirket içi VMware Vm'lerini bulur ve bunlar hakkındaki meta veriler Azure geçişi hizmetine gönderir. Toplayıcı gerecini ayarlamak için bir OVA dosyasını indirmek ve şirket içi vCenter sunucusu örneğine içe aktarın.

#### <a name="download-the-collector-appliance"></a>Toplayıcı gerecini indirin

Birden çok proje varsa, vCenter Server'a yalnızca bir kez Toplayıcı gerecini indirin gerekir. İndirin ve gerecini ayarlamak sonra her proje için çalıştırın ve benzersiz proje Kimliğini ve anahtarını belirtin.

1. Azure geçişi projesinde seçin **Başlarken** > **Keşif ve değerlendirme** > **makineleri Bul**.
2. İçinde **makineleri keşfet**seçin **indirme**OVA dosyasını indirmek için.
3. İçinde **proje kimlik bilgilerini kopyalama**, kopya kimliği ve anahtarı için proje. Toplayıcıyı yapılandırırken bu bilgilere ihtiyaç duyarsınız.


#### <a name="verify-the-collector-appliance"></a>Toplayıcı gereci doğrulama

OVA dosyasını dağıtmadan önce güvenli olup olmadığını denetleyin:

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.

2. OVA’nın karmasını oluşturmak için aşağıdaki komutu çalıştırın:

   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

   Örnek kullanım: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```

3. Oluşturulan karma aşağıdaki ayarları eşleştiğinden emin olun.

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

    OVA sürüm 1.0.9.5 için

    **Algoritma** | **Karma değeri**
    --- | ---
    MD5 | fb11ca234ed1f779a61fbb8439d82969
    SHA1 | 5bee071a6334b6a46226ec417f0d2c494709a42e
    SHA256 | b92ad637e7f522c1d7385b009e7d20904b7b9c28d6f1592e8a14d88fbdd3241c  

    OVA 1.0.9.2 sürümü için

    **Algoritma** | **Karma değeri**
    --- | ---
    MD5 | 7326020e3b83f225b794920b7cb421fc
    SHA1 | a2d8d496fdca4bd36bfa11ddf460602fa90e30be
    SHA256 | f3d9809dd977c689dda1e482324ecd3da0a6a9a74116c1b22710acc19bea7bb2  

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

### <a name="set-the-vcenter-statistics-level"></a>VCenter istatistikleri düzeyi
Bulma sırasında toplanan performans sayaçları listesi aşağıdadır. Vcenter Server'daki çeşitli düzeylerde varsayılan sayaçlar şunlardır.

Böylece tüm sayaçları düzgün toplanan istatistikleri düzeyi için en yüksek genel düzeyde (3) ayarlamanızı öneririz. Daha düşük bir düzeyde ayarlamak vCenter varsa, yalnızca birkaç sayaçları 0 olarak ayarlamak geri kalanı ile tamamen toplanabilir. Değerlendirmeyi, ardından eksik veri gösterebilir.

Aşağıdaki tabloda, belirli bir sayaç alınamadı, etkilenecek değerlendirme sonuçlarını da listeler.

| Sayaç                                 | Düzey | Cihaz başına düzeyi | Etki değerlendirmesi                    |
| --------------------------------------- | ----- | ---------------- | ------------------------------------ |
| CPU.Usage.average                       | 1     | NA               | Önerilen VM boyutu ve maliyet         |
| mem.Usage.average                       | 1     | NA               | Önerilen VM boyutu ve maliyet         |
| virtualDisk.read.average                | 2     | 2                | Disk boyutu, depolama maliyetini ve VM boyutu |
| virtualDisk.write.average               | 2     | 2                | Disk boyutu, depolama maliyetini ve VM boyutu |
| virtualDisk.numberReadAveraged.average  | 1     | 3                | Disk boyutu, depolama maliyetini ve VM boyutu |
| virtualDisk.numberWriteAveraged.average | 1     | 3                | Disk boyutu, depolama maliyetini ve VM boyutu |
| NET.Received.average                    | 2     | 3                | VM boyutu ve ağ maliyeti             |
| NET.transmitted.average                 | 2     | 3                | VM boyutu ve ağ maliyeti             |

> [!WARNING]
> Daha yüksek bir istatistik düzeyini yeni ayarladıysanız, bir gün için performans sayaçları oluşturma sürer. Bu nedenle, bir gün sonra bulma çalıştırmanızı öneririz.

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

Bulma süresi, kaç VM bulduğunuza bağlıdır. Genellikle, 100 VM için toplayıcı çalışmayı tamamladıktan sonra bir saat geçici bir çözüm bulma tamamlanır.

1. Migration Planner projesinde seçin **Yönet** > **makineler**.
2. Bulmak istediğiniz VM’lerin portalda görüntülenip görüntülenmediğini kontrol edin.


## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [bir grup oluşturmak](how-to-create-a-group.md) değerlendirmesi için.
- Değerlendirmelerin nasıl hesaplandığı hakkında [daha fazla bilgi](concepts-assessment-calculation.md) edinin.
