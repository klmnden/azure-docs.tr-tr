---
title: "Ölçeklendirme bulma ve değerlendirme Azure geçirmek kullanarak | Microsoft Docs"
description: "Azure geçiş hizmetini kullanarak şirket içi makineler çok sayıda değerlendirmek açıklar."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 01/08/2018
ms.author: raynew
ms.openlocfilehash: d588dc6037b6295594301b577fe9df31d169a9e6
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="discover-and-assess-a-large-vmware-environment"></a>Bul ve büyük bir VMware ortamı değerlendirin

Bu makalede kullanarak şirket içi sanal makineleri (VM'ler) çok sayıda değerlendirmek nasıl [Azure geçirmek](migrate-overview.md). Azure geçirme Azure geçiş için uygun olup olmadığını denetlemek için makineler değerlendirir. Hizmet Azure'da çalışan makineler için boyutlandırma ve maliyet tahminler sunar.

## <a name="prerequisites"></a>Önkoşullar

- **VMware**: geçirmeyi planladığınız sanal makineleri vCenter Server 5.5, 6.0 veya 6.5 sürümü tarafından yönetilmelidir. Ayrıca, bir ESXi ana bilgisayar çalışan sürüm 5.0 veya sonrasını toplayıcısı VM dağıtmak için gerekir.
- **vCenter hesap**: vCenter Server erişmek için salt okunur bir hesabınızın olması gerekir. Azure Geçişi, şirket içi VM’leri bulmak için bu hesabı kullanır.
- **İzinleri**: vCenter Server OVA biçimde bir dosyaya aktararak bir VM oluşturmak için izinlere ihtiyacınız vardır.
- **İstatistikleri ayarları**: dağıtıma başlamadan önce düzeyi 3 vCenter sunucusu istatistikleri ayarlarını ayarlamanız gerekir. Düzey 3'ten daha düşük ise, değerlendirme çalışır, ancak depolama ve ağ için performans verilerini toplanan olmaz. Boyut önerileri bu durumda CPU ve bellek için performans verilerini ve disk ve ağ bağdaştırıcıları için yapılandırma verilerini temel alır.

## <a name="plan-azure-migrate-projects"></a>Azure geçirmek projeleri planlama

Bulmaları ve aşağıdaki sınırlara göre değerlendirmeleri planlayın:

| **Varlık** | **Makine sınırı** |
| ---------- | ----------------- |
| Project    | 1,500              | 
| Bulma  | 1000              |
| Değerlendirme | 400               |

- Bulmak ve değerlendirmek için 400'den az makineler varsa, tek bir proje ve tek bir bulma gerekir. Gereksinimlerinize bağlı olarak, tek bir değerlendirme tüm makinelerde değerlendirmek veya makineler birden çok değerlendirmeleri bölün. 
- Keşfedilecek 400 için 1.000 makineler varsa, tek bulma ile tek bir proje gerekir. Ancak tek bir değerlendirme en fazla 400 makineleri tutabilen için bu makineleri değerlendirmek için birden çok değerlendirmeleri yapılandırmanız gerekir.
- 1.001 için 1.500 makineler varsa, tek bir proje ile iki bulmaları gerekir.
- 1500'den fazla makineleriniz varsa, birden çok proje oluşturmak ve birden fazla bulma işlemleri gerçekleştirmek, gereksinimlerinize göre gerekir. Örneğin:
    - 3000 makineler varsa, iki bulmaları iki projelerle ya da tek bir bulma üç projenin ayarlayabilirsiniz.
    - 5.000 makineler varsa, dört projeler ayarlayabilir: 1500 makinelerin bulma ve 500 makinelerin bulma ile bir ile üç. Alternatif olarak, her biri tek bir bulmayı beş projelerle ayarlayabilirsiniz. 

## <a name="plan-multiple-discoveries"></a>Birden çok bulmaları planlama

Bir veya daha fazla projeleri birden çok bulmalarına yapmak için aynı Azure geçirmek Toplayıcı kullanabilirsiniz. Bu planlama konuları göz önünde bulundurun:
 
- Azure geçirmek Toplayıcı kullanarak bir bulma yaptığınızda, vCenter Server klasörü, veri merkezi, küme veya ana bilgisayar için bulma kapsamını ayarlayabilirsiniz.
- Birden fazla bulma yapmak için vCenter klasörleri, veri merkezleri, kümeleri veya 1.000 makineler sınırlandırılmasıdır desteklemeyen konaklar, bulmak istediğiniz sanal makineleri olan sunucu doğrulayın.
- Değerlendirme amacıyla, değerlendirme ve aynı proje içinde bağımlılıklarını makinelerle tutmanızı öneririz. VCenter Server bağımlı makineler aynı klasörü, veri merkezi veya değerlendirmesi için küme olduğundan emin olun.


## <a name="create-a-project"></a>Proje oluşturma

Bir Azure geçirmek projesi, gereksinimlerinize uygun şekilde oluşturun:

1. Azure portalında seçin **kaynak oluşturma**.
2. **Azure Geçişi** için arama yapın ve arama sonuçlarında **Azure Geçişi (önizleme)** seçeneğini belirleyin. Ardından **Oluştur**’u seçin.
3. Bir proje adı ve proje için Azure aboneliği belirtin.
4. Yeni bir kaynak grubu oluşturun.
5. İstediğiniz projesi oluşturun ve ardından konum belirtin **oluşturma**. Vm'leriniz için farklı bir hedef konum hala değerlendirebilirsiniz unutmayın. Proje için belirtilen konum, şirket içi Vm'lerden toplanan meta verileri depolamak için kullanılır.

## <a name="set-up-the-collector-appliance"></a>Toplayıcı Gereci ayarlayın

Azure Geçişi, toplayıcı gereci olarak bilinen bir şirket içi VM oluşturur. Bu VM şirket içi VMware sanal makineleri bulur ve bunları hakkındaki meta verileri Azure geçiş hizmetine gönderir. Toplayıcı Gereci ayarlamak için bir OVA dosyasını indirin ve şirket içi vCenter sunucusu örneğine içeri aktarın.

### <a name="download-the-collector-appliance"></a>Toplayıcı gerecini indirin

Birden çok proje varsa, vCenter sunucusuna yalnızca bir kez Toplayıcı Gereci indirme gerekir. Karşıdan yükleme ve Gereci ayarladıktan sonra her proje için çalıştırın ve benzersiz proje kimliği ve anahtarı belirtin.

1. Azure geçirmek projesini seçin **Başlarken** > **bulma & değerlendirin** > **Bul makineler**.
2. İçinde **Bul makineler**seçin **karşıdan**, OVA dosyasını karşıdan yüklemek için.
3. İçinde **kopyalama proje kimlik**anahtarı proje için ve Kimliğini kopyalayın. Toplayıcıyı yapılandırırken bu bilgilere ihtiyaç duyarsınız.

   
### <a name="verify-the-collector-appliance"></a>Toplayıcı gereci doğrulama

Dağıtmadan önce OVA dosya güvenli olduğundan emin olun:

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. OVA’nın karmasını oluşturmak için aşağıdaki komutu çalıştırın:

   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

   Örnek kullanım: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Üretilen karma aşağıdaki ayarları eşleştiğinden emin olun.

    OVA sürüm 1.0.8.59

    **Algoritma** | **Karma değeri**
    --- | ---
    MD5 | 71139e24a532ca67669260b3062c3dad
    SHA1 | 1bdf0666b3c9c9a97a07255743d7c4a2f06d665e
    SHA256 | 6b886d23b24c543f8fc92ff8426cd782a77efb37750afac397591bda1eab8656  
 
    OVA 1.0.8.49 sürümü için

    **Algoritma** | **Karma değeri**
    --- | ---
    MD5 | cefd96394198b92870d650c975dbf3b8
    SHA1 | 4367a1801cf79104b8cd801e4d17b70596481d6f
    SHA256 | fda59f076f1d7bd3ebf53c53d1691cc140c7ed54261d0dc4ed0b14d7efef0ed9

    OVA 1.0.8.40 sürümü için:

    **Algoritma** | **Karma değeri**
    --- | ---
    MD5 |afbae5a2e7142829659c21fd8a9def3f
    SHA1 | 1751849c1d709cdaef0b02a7350834a754b0e71d
    SHA256 | d093a940aebf6afdc6f616626049e97b1f9f70742a094511277c5f59eacc41ad

## <a name="create-the-collector-vm"></a>Toplayıcı VM’yi oluşturma

İndirilen Dosya vCenter sunucusuna içeri aktarın:

1. VSphere istemci konsolunda seçin **dosya** > **OVF şablon dağıtma**.

    ![OVF dağıtma](./media/how-to-scale-assessment/vcenter-wizard.png)

2. OVF şablon Dağıtma Sihirbazı'nda > **kaynak**, OVA dosyasının konumunu belirtin.
3. **Ad** ve **Konum** bölümünde toplayıcı VM için bir kolay ad ve VM’nin barındırılacağı envanter nesnesini belirtin.
5. **Konak/Küme** bölümünde, toplayıcı VM’nin çalıştırılacağı konağı veya kümeyi belirtin.
7. Depolama’da, toplayıcı VM için depolama hedefini belirleyin.
8. **Disk Biçimi**’nde disk türünü ve boyutunu belirtin.
9. **Ağ Eşleme** bölümünde toplayıcı VM’nin bağlanacağı ağı belirtin. Ağ Azure'a meta veri göndermek için internet bağlantısı gerekir. 
10. Gözden geçirin ve ayarları onaylayın ve ardından **son**.

## <a name="identify-the-id-and-key-for-each-project"></a>Kimliğini belirlemek ve her proje için anahtar

Birden çok proje varsa, Kimliğini belirlemek ve her biri için anahtar emin olun. Sanal makineleri bulmak için toplayıcı çalıştırdığınızda bu anahtar gerekir.

1. Projedeki seçin **Başlarken** > **bulma & değerlendirin** > **Bul makineler**.
2. İçinde **kopyalama proje kimlik**anahtarı proje için ve Kimliğini kopyalayın. 
    ![Proje kimlik bilgilerini kopyalama](./media/how-to-scale-assessment/copy-project-credentials.png)

## <a name="set-the-vcenter-statistics-level"></a>VCenter istatistikleri düzeyini ayarlayın
Bulma sırasında toplanan performans sayaçları listesi aşağıdadır. VCenter Server çeşitli düzeylerinde varsayılan sayaçlar şunlardır. 

Böylece tüm sayaçları doğru toplanan istatistikleri düzeyi için en yüksek ortak düzeyi (3) ayarlamanızı öneririz. Daha düşük düzeyde ayarlamak vCenter varsa, yalnızca birkaç sayaçları tamamen 0 olarak ayarlayın rest ile toplanabilir. Değerlendirme ardından eksik verileri gösterebilir. 

Aşağıdaki tabloda, belirli bir sayaç alınamadı, etkilenecek değerlendirme sonuçlarını da listeler.

|Sayaç                                  |Düzey    |Aygıt başına düzeyi  |Değerlendirme etkisi                               |
|-----------------------------------------|---------|------------------|------------------------------------------------|
|cpu.usage.average                        | 1       |NA                |Önerilen VM boyutu ve maliyet                    |
|mem.usage.average                        | 1       |NA                |Önerilen VM boyutu ve maliyet                    |
|virtualDisk.read.average                 | 2       |2                 |Disk boyutu, depolama maliyeti ve VM boyutu         |
|virtualDisk.write.average                | 2       |2                 |Disk boyutu, depolama maliyeti ve VM boyutu         |
|virtualDisk.numberReadAveraged.average   | 1       |3                 |Disk boyutu, depolama maliyeti ve VM boyutu         |
|virtualDisk.numberWriteAveraged.average  | 1       |3                 |Disk boyutu, depolama maliyeti ve VM boyutu         |
|net.received.average                     | 2       |3                 |VM boyutu ve ağ maliyeti                        |
|NET.transmitted.average                  | 2       |3                 |VM boyutu ve ağ maliyeti                        |

> [!WARNING]
> Daha yüksek bir istatistik düzeyi yeni ayarladıysanız, bu günde bir performans sayaçlarını oluşturmak için kadar sürebilir. Bu nedenle, bir günün ardından bulma çalıştırmanızı öneririz.

## <a name="run-the-collector-to-discover-vms"></a>VM’leri bulmak için toplayıcıyı çalıştırma

Yapmanız gereken her bulma için gerekli kapsamında VM'ler bulmak için toplayıcı çalıştırın. Bir bulma art arda çalıştırın. Eşzamanlı bulmaları desteklenmez ve her bir keşfin farklı bir kapsama sahip olmalıdır.

1. vSphere Client konsolunda VM’ye sağ tıklayın ve **Konsolu Aç** seçeneğini belirleyin.
2. Dil, saat dilimi ve Gereci parola tercihlerini sağlar.
3. Masaüstünde seçin **Toplayıcı çalıştırmak** kısayol.
4. Azure geçirmek Toplayıcı açmak **önkoşulları ayarlama** ve sonra:

   a. Lisans koşullarını kabul edin ve üçüncü taraf bilgilerini okuyun.

   Toplayıcı, VM’nin İnternet erişimine sahip olup olmadığını denetler.
   
   b. VM Internet proxy üzerinden erişirse, seçin **Proxy ayarlarını**, proxy adresi ve dinleme bağlantı noktasını belirtin. Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin.

   Toplayıcı, Toplayıcı hizmetinin çalıştığını denetler. Hizmet, toplayıcı VM’ye varsayılan olarak yüklenir.

   c. VMware Powerclı yükleyip yeniden açın.

5. **vCenter Server bilgilerini belirtin** bölümünde şunları yapın:
    - Adı (FQDN) veya vCenter sunucusunun IP adresini belirtin.
    - İçinde **kullanıcı adı** ve **parola**, Toplayıcı VM'ler vCenter Server'da bulmak için kullanacağı salt okunur hesap kimlik bilgilerini belirtin.
    - İçinde **seçin kapsam**, VM keşfi için kapsamı seçin. Toplayıcı, yalnızca belirtilen kapsamın içindeki VM'ler bulabilir. Kapsam belirli bir klasör, veri merkezi veya küme olarak ayarlanabilir. 1. 000'den fazla VMs içermemelidir. 

6. İçinde **belirt geçiş proje**anahtarı proje için ve Kimliğini belirtin. Kopyaladığınız alamadık, Toplayıcı VM Azure Portalı'nı açın. Projenin üzerinde **genel bakış** sayfasında, **Bul makineler** ve değerleri kopyalayın.  
7. İçinde **koleksiyonu ilerlemeyi görüntüleme**, Keşif sürecini izleyebilir ve VM'lerin toplanan meta verilerin kapsamında olduğunu denetleyin. Toplayıcı, yaklaşık bir bulma süresi sağlar.


### <a name="verify-vms-in-the-portal"></a>VM’lerin portalda olup olmadığını doğrulama

Bulma süresi, kaç VM bulduğunuza bağlıdır. Genellikle, 100 VM'ler için bulma Toplayıcı çalışması bittikten sonra bir saat tamamlanır. 

1. Geçiş Planlayıcısı projedeki seçin **Yönet** > **makineler**.
2. Bulmak istediğiniz VM’lerin portalda görüntülenip görüntülenmediğini kontrol edin.


## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [bir grup oluşturmak](how-to-create-a-group.md) değerlendirmesi için.
- Değerlendirmelerin nasıl hesaplandığı hakkında [daha fazla bilgi](concepts-assessment-calculation.md) edinin.
