---
title: "Ölçeklendirme bulma ve değerlendirme Azure geçirmek kullanarak | Microsoft Docs"
description: "Azure geçiş hizmetini kullanarak şirket içi makineler çok sayıda değerlendirmek açıklar."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 12/19/2017
ms.author: raynew
ms.openlocfilehash: 9b457252fdb7a1ad62b7e6038b341451df2e1590
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="discover-and-assess-a-large-vmware-environment"></a>Bul ve büyük bir VMware ortamı değerlendirin

Bu makalede kullanarak şirket içi sanal makineleri (VM'ler) çok sayıda değerlendirmek nasıl [Azure geçirmek](migrate-overview.md). Azure geçirme Azure geçiş için uygun olup olmadığını denetlemek için makineler değerlendirir. Hizmet Azure'da çalışan makineler için boyutlandırma ve maliyet tahminler sunar.

## <a name="prerequisites"></a>Önkoşullar

- **VMware**: geçirmeyi planladığınız sanal makineleri vCenter Server 5.5, 6.0 veya 6.5 sürümü tarafından yönetilmelidir. Ayrıca, bir ESXi ana bilgisayar çalışan sürüm 5.0 veya sonrasını toplayıcısı VM dağıtmak için gerekir.
- **vCenter hesap**: vCenter Server erişmek için salt okunur bir hesabınızın olması gerekir. Azure geçirme, şirket içi sanal makineleri bulmak için bu hesabı kullanır.
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
    - 5.000 makineler varsa, dört projeler ayarlayabilir: iki 1500 makinelerin bulma ve 500 makinelerin bulma ile bir. Alternatif olarak, her biri tek bir bulmayı beş projelerle ayarlayabilirsiniz. 

## <a name="plan-multiple-discoveries"></a>Birden çok bulmaları planlama

Bir veya daha fazla projeleri birden çok bulmalarına yapmak için aynı Azure geçirmek Toplayıcı kullanabilirsiniz. Bu planlama konuları göz önünde bulundurun:
 
- Azure geçirmek Toplayıcı kullanarak bir bulma yaptığınızda, vCenter Server klasörü, veri merkezi, küme veya ana bilgisayar için bulma kapsamını ayarlayabilirsiniz.
- Birden fazla bulma yapmak için vCenter klasörleri, veri merkezleri, kümeleri veya 1.000 makineler sınırlandırılmasıdır desteklemeyen konaklar, bulmak istediğiniz sanal makineleri olan sunucu doğrulayın.
- Değerlendirme amacıyla, değerlendirme ve aynı proje içinde bağımlılıklarını makinelerle tutmanızı öneririz. VCenter Server bağımlı makineler aynı klasörü, veri merkezi veya değerlendirmesi için küme olduğundan emin olun.


## <a name="create-a-project"></a>Proje oluştur

Bir Azure geçirmek projesi, gereksinimlerinize uygun şekilde oluşturun:

1. Azure portalında seçin **kaynak oluşturma**.
2. Arama **Azure geçirmek**ve hizmeti seçin **Azure geçirme (Önizleme)** arama sonuçlarında. Ardından **Oluştur**’u seçin.
3. Bir proje adı ve proje için Azure aboneliği belirtin.
4. Yeni bir kaynak grubu oluşturun.
5. İstediğiniz projesi oluşturun ve ardından konum belirtin **oluşturma**. Vm'leriniz için farklı bir hedef konum hala değerlendirebilirsiniz unutmayın. Proje için belirtilen konum, şirket içi Vm'lerden toplanan meta verileri depolamak için kullanılır.

## <a name="set-up-the-collector-appliance"></a>Toplayıcı Gereci ayarlayın

Azure geçirme toplayıcı uygulaması olarak bilinen bir şirket içi VM oluşturur. Bu VM şirket içi VMware sanal makineleri bulur ve bunları hakkındaki meta verileri Azure geçiş hizmetine gönderir. Toplayıcı Gereci ayarlamak için bir OVA dosyasını indirin ve şirket içi vCenter sunucusu örneğine içeri aktarın.

### <a name="download-the-collector-appliance"></a>Toplayıcı Gereci indirin

Birden çok proje varsa, vCenter sunucusuna yalnızca bir kez Toplayıcı Gereci indirme gerekir. Karşıdan yükleme ve Gereci ayarladıktan sonra her proje için çalıştırın ve benzersiz proje kimliği ve anahtarı belirtin.

1. Azure geçirmek projesini seçin **Başlarken** > **bulma & değerlendirin** > **Bul makineler**.
2. İçinde **Bul makineler**seçin **karşıdan**, OVA dosyasını karşıdan yüklemek için.
3. İçinde **kopyalama proje kimlik**anahtarı proje için ve Kimliğini kopyalayın. Toplayıcı yapılandırırken bu gerekir.

   
### <a name="verify-the-collector-appliance"></a>Toplayıcı Gereci doğrulayın

Dağıtmadan önce OVA dosya güvenli olduğundan emin olun:

1. Dosya karşıdan makinede bir yönetici komut penceresi açın.
2. Karma için OVA oluşturmak için aşağıdaki komutu çalıştırın:

   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

   Örnek Kullanım:```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Üretilen karma aşağıdaki ayarları eşleştiğinden emin olun.
 
    OVA sürümü 1.0.8.38:
    **Algoritması** | **Karma değeri**
    --- | ---
    MD5 | dd27dd6ace28f9195a2b5d52a4003067 
    SHA1 | d2349e06a5d4693fc2a1c0619591b9e45c36d695
    SHA256 | 1492a0c6d6ef76e79269d5cd6f6a22f336341e1accbc9e3dfa5dad3049be6798

    OVA sürümü 1.0.8.40:
    **Algoritması** | **Karma değeri**
    --- | ---
    MD5 | afbae5a2e7142829659c21fd8a9def3f
    SHA1 | 1751849c1d709cdaef0b02a7350834a754b0e71d
    SHA256 | d093a940aebf6afdc6f616626049e97b1f9f70742a094511277c5f59eacc41ad



## <a name="create-the-collector-vm"></a>Toplayıcı VM oluşturma

İndirilen Dosya vCenter sunucusuna içeri aktarın:

1. VSphere istemci konsolunda seçin **dosya** > **OVF şablon dağıtma**.

    ![OVF dağıtma](./media/how-to-scale-assessment/vcenter-wizard.png)

2. OVF şablon Dağıtma Sihirbazı'nda > **kaynak**, OVA dosyasının konumunu belirtin.
3. İçinde **adı** ve **konumu**toplayıcısı VM için bir kolay ad belirtin ve stok nesne VM hangi barındırılacak içinde.
5. İçinde **konak/küme**, konak belirtin veya küme VM Toplayıcı çalıştırdığınız.
7. Depolama biriminde, VM Toplayıcı depolama hedefini belirtin.
8. İçinde **Disk biçimi**, disk türünü ve boyutunu belirtin.
9. İçinde **ağ eşlemesi**, VM Toplayıcı bağlanacağı ağı belirtin. Ağ Azure'a meta veri göndermek için internet bağlantısı gerekir. 
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
|CPU.Usage.average                        | 1       |NA                |Önerilen VM boyutu ve maliyet                    |
|mem.Usage.average                        | 1       |NA                |Önerilen VM boyutu ve maliyet                    |
|virtualDisk.read.average                 | 2       |2                 |Disk boyutu, depolama maliyeti ve VM boyutu         |
|virtualDisk.write.average                | 2       |2                 |Disk boyutu, depolama maliyeti ve VM boyutu         |
|virtualDisk.numberReadAveraged.average   | 1       |3                 |Disk boyutu, depolama maliyeti ve VM boyutu         |
|virtualDisk.numberWriteAveraged.average  | 1       |3                 |Disk boyutu, depolama maliyeti ve VM boyutu         |
|NET.Received.average                     | 2       |3                 |VM boyutu ve ağ maliyeti                        |
|NET.transmitted.average                  | 2       |3                 |VM boyutu ve ağ maliyeti                        |

> [!WARNING]
> Daha yüksek bir istatistik düzeyi yeni ayarladıysanız, bu günde bir performans sayaçlarını oluşturmak için kadar sürebilir. Bu nedenle, bir günün ardından bulma çalıştırmanızı öneririz.

## <a name="run-the-collector-to-discover-vms"></a>Sanal makineleri bulmak için toplayıcı çalıştırın

Yapmanız gereken her bulma için gerekli kapsamında VM'ler bulmak için toplayıcı çalıştırın. Bir bulma art arda çalıştırın. Eşzamanlı bulmaları desteklenmez ve her bir keşfin farklı bir kapsama sahip olmalıdır.

1. VSphere istemci konsolunda VM'ye sağ tıklayın > **açık Konsolu**.
2. Dil, saat dilimi ve Gereci parola tercihlerini sağlar.
3. Masaüstünde seçin **Toplayıcı çalıştırmak** kısayol.
4. Azure geçirmek Toplayıcı açmak **önkoşulları ayarlama** ve sonra:

   a. Lisans koşullarını kabul edin ve üçüncü taraf bilgileri okuyun.

   Toplayıcı VM Internet erişimi olduğunu denetler.
   
   b. VM Internet proxy üzerinden erişirse, seçin **Proxy ayarlarını**, proxy adresi ve dinleme bağlantı noktasını belirtin. Proxy kimlik doğrulaması gerektiriyorsa kimlik bilgilerini belirtin.

   Toplayıcı, Toplayıcı hizmetinin çalıştığını denetler. Hizmeti toplayıcısı VM üzerinde varsayılan olarak yüklenir.

   c. VMware Powerclı yükleyip yeniden açın.

5. İçinde **vCenter sunucusu ayrıntılarını belirtin**, aşağıdakileri yapın:
    - Adı (FQDN) veya vCenter sunucusunun IP adresini belirtin.
    - İçinde **kullanıcı adı** ve **parola**, Toplayıcı VM'ler vCenter Server'da bulmak için kullanacağı salt okunur hesap kimlik bilgilerini belirtin.
    - İçinde **seçin kapsam**, VM keşfi için kapsamı seçin. Toplayıcı, yalnızca belirtilen kapsamın içindeki VM'ler bulabilir. Kapsamı belirli klasör, veri merkezi veya küme ayarlayabilirsiniz. 1. 000'den fazla VMs içermemelidir. 
    - İçinde **vCenter etiketi kategori gruplandırma için**seçin **hiçbiri**.

    ![Kapsam seçin](./media/how-to-scale-assessment/select-scope.png)

6. İçinde **belirt geçiş proje**anahtarı proje için ve Kimliğini belirtin. Kopyaladığınız alamadık, Toplayıcı VM Azure Portalı'nı açın. Projenin üzerinde **genel bakış** sayfasında, **Bul makineler** ve değerleri kopyalayın.  
7. İçinde **koleksiyonu ilerlemeyi görüntüleme**, Keşif sürecini izleyebilir ve VM'lerin toplanan meta verilerin kapsamında olduğunu denetleyin. Toplayıcı bir yaklaşık bulma süresi sağlar.


### <a name="verify-vms-in-the-portal"></a>Portalda VM'ler doğrulayın

Kaç tane sanal makinelerin süre bağlıdır bulma, bulma. Genellikle, 100 VM'ler için bulma Toplayıcı çalışması bittikten sonra bir saat tamamlanır. 

1. Geçiş Planlayıcısı projedeki seçin **Yönet** > **makineler**.
2. Bulmak istediğiniz sanal makineleri portalda görünür denetleyin.


## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [bir grup oluşturmak](how-to-create-a-group.md) değerlendirmesi için.
- [Daha fazla bilgi edinin](concepts-assessment-calculation.md) değerlendirmelerinin nasıl hesaplandığını hakkında.
