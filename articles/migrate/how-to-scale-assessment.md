---
title: Ölçeklendirme bulma ve değerlendirme Azure geçirmek kullanarak | Microsoft Docs
description: Azure geçiş hizmetini kullanarak şirket içi makineler çok sayıda değerlendirmek açıklar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 06/04/2018
ms.author: raynew
ms.openlocfilehash: 89c9cfd4bdc1c483764983c886ba9f96cc75c69e
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34736839"
---
# <a name="discover-and-assess-a-large-vmware-environment"></a>Büyük bir VMware ortamını bulma ve değerlendirme

Azure geçirme 1500 makinelerin her proje bir sınırı vardır, bu makalede kullanarak şirket içi sanal makineleri (VM'ler) çok sayıda değerlendirmek nasıl [Azure geçirmek](migrate-overview.md).   

## <a name="prerequisites"></a>Önkoşullar

- **VMware**: geçirmeyi planladığınız sanal makineleri vCenter Server 5.5, 6.0 veya 6.5 sürümü tarafından yönetilmelidir. Ayrıca, bir ESXi ana bilgisayar çalışan sürüm 5.0 veya sonrasını toplayıcısı VM dağıtmak için gerekir.
- **vCenter hesap**: vCenter Server erişmek için salt okunur bir hesabınızın olması gerekir. Azure Geçişi, şirket içi VM’leri bulmak için bu hesabı kullanır.
- **İzinleri**: vCenter Server OVA biçimde bir dosyaya aktararak bir VM oluşturmak için izinlere ihtiyacınız vardır.
- **İstatistikleri ayarları**: dağıtıma başlamadan önce düzeyi 3 vCenter sunucusu istatistikleri ayarlarını ayarlamanız gerekir. Düzey 3'ten daha düşük ise, değerlendirme çalışır, ancak depolama ve ağ için performans verilerini toplanan olmaz. Boyut önerileri bu durumda CPU ve bellek için performans verilerini ve disk ve ağ bağdaştırıcıları için yapılandırma verilerini temel alır.

## <a name="plan-your-migration-projects-and-discoveries"></a>Geçiş projeleri ve bulmaları planlama

Tek bir Azure geçirmek Toplayıcı birden çok vcenter sunucuları bulma (birbiri ardından) ve ayrıca birden çok geçiş projelerine bulma (birbiri ardından) destekler. Toplayıcı bir yangın çalışır ve modeli unuttunuz bulma yaptıktan sonra farklı bir vCenter Server verilerini toplamak ya da farklı geçiş projeye göndermek için aynı Toplayıcı kullanabilirsiniz.

Bulmaları ve aşağıdaki sınırlara göre değerlendirmeleri planlayın:

| **Varlık** | **Makine sınırı** |
| ---------- | ----------------- |
| Project    | 1,500             |
| Bulma  | 1,500             |
| Değerlendirme | 1,500             |

Bu planlama konuları göz önünde bulundurun:

- Azure geçirmek Toplayıcı kullanarak bir bulma yaptığınızda, vCenter Server klasörü, veri merkezi, küme veya ana bilgisayar için bulma kapsamını ayarlayabilirsiniz.
- Birden fazla bulma yapmak için vCenter klasörleri, veri merkezleri, kümeleri veya 1500 makineler sınırlandırılmasıdır desteklemeyen konaklar, bulmak istediğiniz sanal makineleri olan sunucu doğrulayın.
- Değerlendirme amacıyla, değerlendirme ve aynı proje içinde bağımlılıklarını makinelerle tutmanızı öneririz. VCenter Server bağımlı makineler aynı klasörü, veri merkezi veya değerlendirmesi için küme olduğundan emin olun.

Senaryonuza bağlı olarak, aşağıda belirlenen gibi bulmaları ayırabilirsiniz:

### <a name="multiple-vcenter-servers-with-less-than-1500-vms"></a>Birden çok vCenter sunucuları değerinden 1500 VM'ler ile

Ortamınızda birden çok vCenter sunucuları olması ve sanal makinelerin toplam sayısını 1500'den az ise, tüm sanal makineler arasında tüm vCenter sunucularını bulmak için tek bir Toplayıcı ve tek geçiş proje kullanabilirsiniz. Toplayıcı bir vCenter sunucusu aynı anda bulur olduğundan, aynı Toplayıcı tüm vCenter karşı sunucuları, birbiri ardından çalıştırın ve aynı geçiş projesi Toplayıcı'ne gelin. Tüm bulma işlemleri tamamlandıktan sonra daha sonra makinelerin değerlendirmeleri oluşturabilirsiniz.

### <a name="multiple-vcenter-servers-with-more-than-1500-vms"></a>Birden çok vCenter sunucuları birden fazla 1500 VM'ler ile

VCenter sunucusu başına değerinden 1500 sanal makineler, ancak birden fazla 1500 VM'ler ile birden çok vCenter sunucuları arasında tüm vCenter görevi görür varsa (bir geçiş proje yalnızca 1500 VM'ler tutabilir) birden çok geçiş projeleri oluşturmanız gerekir. Bu vCenter sunucusu başına bir geçiş proje oluşturma ve bulmaları bölme elde edebilirsiniz. Tek bir Toplayıcı her vCenter Server (birbiri ardından) bulmak için kullanabilirsiniz. Aynı anda başlatmak için bulmaları istiyorsanız, birden çok uygulamaları dağıtabilir ve paralel olarak çalıştıracak.

### <a name="more-than-1500-machines-in-a-single-vcenter-server"></a>Tek bir vCenter Server'ın birden fazla 1500 makineler

Tek bir vCenter Server'da birden fazla 1500 sanal makineler varsa, birden çok geçiş projelere bulma bölmeniz gerekir. Bulmaları bölmek için Gereci kapsam alanında yararlanır ve ana bilgisayar, küme, klasör veya bulmak istediğiniz datacenter belirtin. VCenter Server, 1000 ile de iki klasör varsa, örneğin, sanal makineleri (Klasör1) ve diğer 800 sanal makineler (klasör2) ile tek toplayıcısını kullanın ve iki bulmaları gerçekleştirin. İlk bulma kapsamı olarak Klasör1 belirtin ve noktası ilk geçiş projesine ilk bulma tamamlandıktan sonra aynı toplayıcının, klasör2 ve geçiş proje ayrıntılarını ikinci geçiş projeye için kapsamı değiştirin ve İkinci bulma yapın.

### <a name="multi-tenant-environment"></a>Çok kiracılı ortamı

Kiracılar arasında paylaşılan bir ortamda varsa ve başka bir kiracının Abonelikteki bir kiracı VM'ler bulmak istediğinizde değil, bulma kapsamı için toplayıcı Gereci kapsam alanında kullanabilirsiniz. Kiracılar konakları paylaşıyorsanız, yalnızca belirli kiracıya ait sanal makineleri salt okunur erişimi olan bir kimlik bilgisi oluşturun ve sonra Toplayıcı Gereci bu kimlik bilgilerini kullanın ve kapsam bulma yapmak için ana bilgisayar olarak belirtin. Alternatif olarak, vcenter Server'daki (tenant1 Klasör1 ve tenant2 klasör2 düşünelim), paylaşılan konak altında klasör oluşturabilirsiniz ve tenant2 Klasör1 içine tenant1 için sanal makineleri klasör2 taşıyın ve bulmaları Toplayıcı, buna göre kapsam uygun klasörü belirterek.

## <a name="discover-on-premises-environment"></a>Şirket içi ortamına Bul

Planınızla hazır olduğunuzda, şirket içi sanal makinelerin bulma başlatabilirsiniz:

### <a name="create-a-project"></a>Proje oluşturma

Bir Azure geçirmek projesi, gereksinimlerinize uygun şekilde oluşturun:

1. Azure portalında seçin **kaynak oluşturma**.
2. **Azure Geçişi** için arama yapın ve arama sonuçlarında **Azure Geçişi (önizleme)** seçeneğini belirleyin. Ardından **Oluştur**’u seçin.
3. Bir proje adı ve proje için Azure aboneliği belirtin.
4. Yeni bir kaynak grubu oluşturun.
5. İstediğiniz projesi oluşturun ve ardından konum belirtin **oluşturma**. Vm'leriniz için farklı bir hedef konum hala değerlendirebilirsiniz unutmayın. Proje için belirtilen konum, şirket içi Vm'lerden toplanan meta verileri depolamak için kullanılır.

### <a name="set-up-the-collector-appliance"></a>Toplayıcı Gereci ayarlayın

Azure Geçişi, toplayıcı gereci olarak bilinen bir şirket içi VM oluşturur. Bu VM şirket içi VMware sanal makineleri bulur ve bunları hakkındaki meta verileri Azure geçiş hizmetine gönderir. Toplayıcı Gereci ayarlamak için bir OVA dosyasını indirin ve şirket içi vCenter sunucusu örneğine içeri aktarın.

#### <a name="download-the-collector-appliance"></a>Toplayıcı gerecini indirin

Birden çok proje varsa, vCenter sunucusuna yalnızca bir kez Toplayıcı Gereci indirme gerekir. Karşıdan yükleme ve Gereci ayarladıktan sonra her proje için çalıştırın ve benzersiz proje kimliği ve anahtarı belirtin.

1. Azure geçirmek projesini seçin **Başlarken** > **bulma & değerlendirin** > **Bul makineler**.
2. İçinde **Bul makineler**seçin **karşıdan**, OVA dosyasını karşıdan yüklemek için.
3. İçinde **kopyalama proje kimlik**anahtarı proje için ve Kimliğini kopyalayın. Toplayıcıyı yapılandırırken bu bilgilere ihtiyaç duyarsınız.


#### <a name="verify-the-collector-appliance"></a>Toplayıcı gereci doğrulama

Dağıtmadan önce OVA dosya güvenli olduğundan emin olun:

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.

2. OVA’nın karmasını oluşturmak için aşağıdaki komutu çalıştırın:

   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

   Örnek kullanım: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```

3. Üretilen karma aşağıdaki ayarları eşleştiğinden emin olun.

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

İndirilen Dosya vCenter sunucusuna içeri aktarın:

1. VSphere istemci konsolunda seçin **dosya** > **OVF şablon dağıtma**.

    ![OVF dağıtma](./media/how-to-scale-assessment/vcenter-wizard.png)

2. OVF şablon Dağıtma Sihirbazı'nda > **kaynak**, OVA dosyasının konumunu belirtin.
3. **Ad** ve **Konum** bölümünde toplayıcı VM için bir kolay ad ve VM’nin barındırılacağı envanter nesnesini belirtin.
4. **Konak/Küme** bölümünde, toplayıcı VM’nin çalıştırılacağı konağı veya kümeyi belirtin.
5. Depolama’da, toplayıcı VM için depolama hedefini belirleyin.
6. **Disk Biçimi**’nde disk türünü ve boyutunu belirtin.
7. **Ağ Eşleme** bölümünde toplayıcı VM’nin bağlanacağı ağı belirtin. Ağ Azure'a meta veri göndermek için internet bağlantısı gerekir.
8. Gözden geçirin ve ayarları onaylayın ve ardından **son**.

### <a name="identify-the-id-and-key-for-each-project"></a>Kimliğini belirlemek ve her proje için anahtar

Birden çok proje varsa, Kimliğini belirlemek ve her biri için anahtar emin olun. Sanal makineleri bulmak için toplayıcı çalıştırdığınızda bu anahtar gerekir.

1. Projedeki seçin **Başlarken** > **bulma & değerlendirin** > **Bul makineler**.
2. İçinde **kopyalama proje kimlik**anahtarı proje için ve Kimliğini kopyalayın.
    ![Proje kimlik bilgilerini kopyalama](./media/how-to-scale-assessment/copy-project-credentials.png)

### <a name="set-the-vcenter-statistics-level"></a>VCenter istatistikleri düzeyini ayarlayın
Bulma sırasında toplanan performans sayaçları listesi aşağıdadır. VCenter Server çeşitli düzeylerinde varsayılan sayaçlar şunlardır.

Böylece tüm sayaçları doğru toplanan istatistikleri düzeyi için en yüksek ortak düzeyi (3) ayarlamanızı öneririz. Daha düşük düzeyde ayarlamak vCenter varsa, yalnızca birkaç sayaçları tamamen 0 olarak ayarlayın rest ile toplanabilir. Değerlendirme ardından eksik verileri gösterebilir.

Aşağıdaki tabloda, belirli bir sayaç alınamadı, etkilenecek değerlendirme sonuçlarını da listeler.

| Sayaç                                 | Düzey | Aygıt başına düzeyi | Değerlendirme etkisi                    |
| --------------------------------------- | ----- | ---------------- | ------------------------------------ |
| CPU.Usage.average                       | 1     | NA               | Önerilen VM boyutu ve maliyet         |
| mem.Usage.average                       | 1     | NA               | Önerilen VM boyutu ve maliyet         |
| virtualDisk.read.average                | 2     | 2                | Disk boyutu, depolama maliyeti ve VM boyutu |
| virtualDisk.write.average               | 2     | 2                | Disk boyutu, depolama maliyeti ve VM boyutu |
| virtualDisk.numberReadAveraged.average  | 1     | 3                | Disk boyutu, depolama maliyeti ve VM boyutu |
| virtualDisk.numberWriteAveraged.average | 1     | 3                | Disk boyutu, depolama maliyeti ve VM boyutu |
| NET.Received.average                    | 2     | 3                | VM boyutu ve ağ maliyeti             |
| NET.transmitted.average                 | 2     | 3                | VM boyutu ve ağ maliyeti             |

> [!WARNING]
> Daha yüksek bir istatistik düzeyi yeni ayarladıysanız, bu günde bir performans sayaçlarını oluşturmak için kadar sürebilir. Bu nedenle, bir günün ardından bulma çalıştırmanızı öneririz.

### <a name="run-the-collector-to-discover-vms"></a>VM’leri bulmak için toplayıcıyı çalıştırma

Yapmanız gereken her bulma için gerekli kapsamında VM'ler bulmak için toplayıcı çalıştırın. Bir bulma art arda çalıştırın. Eşzamanlı bulmaları desteklenmez ve her bir keşfin farklı bir kapsama sahip olmalıdır.

1.  vSphere Client konsolunda VM’ye sağ tıklayın ve **Konsolu Aç** seçeneğini belirleyin.
2.  Gereç için dil, saat dilimi ve parola tercihlerini belirtin.
3.  Masaüstünde seçin **Toplayıcı çalıştırmak** kısayol.
4.  Azure geçirmek Toplayıcı açmak **önkoşulları ayarlama** ve sonra:

    a. Lisans koşullarını kabul edin ve üçüncü taraf bilgilerini okuyun.

    Toplayıcı, VM’nin İnternet erişimine sahip olup olmadığını denetler.

    b. VM Internet proxy üzerinden erişirse, seçin **Proxy ayarlarını**, proxy adresi ve dinleme bağlantı noktasını belirtin. Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin.

    Toplayıcı, toplayıcı hizmetinin çalışıp çalışmadığını denetler. Hizmet, toplayıcı VM’ye varsayılan olarak yüklenir.

    c. VMware PowerCLI’yı indirin ve yükleyin.

5.  **vCenter Server bilgilerini belirtin** bölümünde şunları yapın:
    - Adı (FQDN) veya vCenter sunucusunun IP adresini belirtin.
    - İçinde **kullanıcı adı** ve **parola**, Toplayıcı VM'ler vCenter Server'da bulmak için kullanacağı salt okunur hesap kimlik bilgilerini belirtin.
    - **Kapsam seçin** bölümünde, sanal makine bulma için bir kapsam seçin. Toplayıcı, yalnızca belirtilen kapsamın içindeki VM'ler bulabilir. Kapsam belirli bir klasör, veri merkezi veya küme olarak ayarlanabilir. 1. 000'den fazla VMs içermemelidir.

6.  İçinde **belirt geçiş proje**anahtarı proje için ve Kimliğini belirtin. Kopyaladığınız alamadık, Toplayıcı VM Azure Portalı'nı açın. Projenin üzerinde **genel bakış** sayfasında, **Bul makineler** ve değerleri kopyalayın.  
7.  İçinde **koleksiyonu ilerlemeyi görüntüleme**, Keşif sürecini izleyebilir ve VM'lerin toplanan meta verilerin kapsamında olduğunu denetleyin. Toplayıcı, yaklaşık bir bulma süresi sağlar.


#### <a name="verify-vms-in-the-portal"></a>VM’lerin portalda olup olmadığını doğrulama

Bulma süresi, kaç VM bulduğunuza bağlıdır. Genellikle, 100 VM'ler için bulma Toplayıcı çalışması bittikten sonra bir saat tamamlanır.

1. Geçiş Planlayıcısı projedeki seçin **Yönet** > **makineler**.
2. Bulmak istediğiniz VM’lerin portalda görüntülenip görüntülenmediğini kontrol edin.


## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [bir grup oluşturmak](how-to-create-a-group.md) değerlendirmesi için.
- Değerlendirmelerin nasıl hesaplandığı hakkında [daha fazla bilgi](concepts-assessment-calculation.md) edinin.
