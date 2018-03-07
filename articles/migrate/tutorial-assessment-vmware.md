---
title: "Azure’a geçiş için şirket içi VMware VM’lerini Azure Geçişi ile bulma ve değerlendirme | Microsoft Docs"
description: "Azure’a geçiş için şirket içi VMware VM’lerinin Azure Geçişi hizmeti kullanılarak nasıl bulunacağı ve değerlendirileceği açıklanmaktadır."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 02/27/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 3c8d345d8846994ac1e286d977b62d9ae2b7d660
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
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


## <a name="prerequisites"></a>Ön koşullar

- **VMware**: Geçirmeyi planladığınız sanal makineler, 5.5, 6.0 veya 6.5 sürümünü çalıştıran vCenter Server tarafından yönetilmelidir. Buna ek olarak, toplayıcı VM’yi dağıtmak için 5.0 veya daha sonraki sürüme sahip bir ESXi konağı gerekir. 
 
> [!NOTE]
> Hyper-V desteği, yol haritasında yer almakta olup kısa süre sonra etkinleştirilecektir. 

- **vCenter Server hesabı**: vCenter Server’a erişmek için salt okunur bir hesabınız olması gerekir. Azure Geçişi, şirket içi VM’leri bulmak için bu hesabı kullanır.
- **İzinler**: vCenter Server’da, bir dosyayı .OVA biçiminde içeri aktararak VM oluşturma iznine sahip olmanız gerekir. 
- **İstatistik ayarları**: vCenter Server için istatistik ayarları, dağıtım başlatılmadan önce düzey 3 olarak belirlenmelidir. Ayarlar düzey 3’ün altında olursa değerlendirme gerçekleştirilir, ancak depolama ve ağ için performans verileri toplanmaz. Bu durumda boyut önerileri, CPU ve belleğe ait performans verilerine ve diskin ve ağ bağdaştırıcılarının yapılandırma verilerine bağlı olarak yapılır. 

## <a name="create-an-account-for-vm-discovery"></a>VM bulma işlemi için hesap oluşturma

Azure Geçişi’nin, değerlendirme amacıyla VM’leri otomatik olarak bulması için VMware sunucularına erişebilmesi gerekir. Aşağıdaki özelliklere sahip bir VMware hesabı oluşturun. Bu hesabı Azure Geçişi kurulumu sırasında belirtirsiniz.

- Kullanıcı türü: En azından salt okunur bir kullanıcı
- İzinler: Veri Merkezi nesnesi –> Alt Nesneye Yay, rol=Salt okunur
- Ayrıntılar: Veri merkezi düzeyinde atanmış ve veri merkezindeki tüm nesnelere erişimi olan kullanıcı.
- Erişimi kısıtlamak için Alt nesneye yay ile Erişim yok rolünü alt nesnelere (vSphere konakları, veri depoları, VM’ler ve ağlar) atayın.

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com)’da oturum açın.

## <a name="create-a-project"></a>Proje oluşturma

1. Azure portalında **Kaynak oluştur**’a tıklayın.
2. **Azure Geçişi** araması yapın ve arana sonuçlarında **Azure Geçişi** hizmetini seçin. Sonra **Oluştur**’a tıklayın.
3. Proje için bir proje adı ve Azure aboneliği belirtin.
4. Yeni bir kaynak grubu oluşturun.
5. Projenin oluşturulacağı konumu belirtin ve sonra **Oluştur**’a tıklayın. Bir Azure Geçişi projesini yalnızca Orta Batı ABD veya Doğu ABD bölgesinde oluşturabilirsiniz. Ancak yine de herhangi bir hedef Azure konumu için geçişinizi planlayabilirsiniz. Proje için belirtilen konum yalnızca şirket içi VM’lerden toplanan meta verileri depolamak için kullanılır. 

    ![Azure Geçişi](./media/tutorial-assessment-vmware/project-1.png)
    


## <a name="download-the-collector-appliance"></a>Toplayıcı gerecini indirin

Azure Geçişi, toplayıcı gereci olarak bilinen bir şirket içi VM oluşturur. Bu VM, şirket içi VMware VM’lerini bulur ve bunlara ilişkin meta verileri Azure Geçişi hizmetine gönderir. Toplayıcı gerecini ayarlamak için, bir .OVA dosyası indirin ve VM’yi oluşturmak için dosyayı şirket içi vCenter sunucusuna aktarın.

1. Azure Geçişi projesinde **Kullanmaya Başlama** > **Bul ve Değerlendir** > **Makineleri Keşfet**’ye tıklayın.
2. **Makineleri keşfet** bölümünde .OVA dosyasını indirmek için **İndir**’e tıklayın.
3. **Proje kimlik bilgilerini kopyala** bölümünde proje kimliğini ve anahtarı kopyalayın. Toplayıcıyı yapılandırırken bu bilgilere ihtiyaç duyarsınız.

    ![.ova dosyasını indir](./media/tutorial-assessment-vmware/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Toplayıcı gereci doğrulama

Dağıtmadan önce .OVA dosyasının güvenilir olup olmadığını kontrol edin.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. OVA’nın karmasını oluşturmak için aşağıdaki komutu çalıştırın:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek kullanım: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Oluşturulan karma bu ayarlara uygun olmalıdır.
    
    OVA 1.0.9.2 sürümü için

    **Algoritma** | **Karma değeri**
    --- | ---
    MD5 | 7326020e3b83f225b794920b7cb421fc
    SHA1 | a2d8d496fdca4bd36bfa11ddf460602fa90e30be
    SHA256 | f3d9809dd977c689dda1e482324ecd3da0a6a9a74116c1b22710acc19bea7bb2  
    
    OVA 1.0.8.59 sürümü için

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
    MD5 | afbae5a2e7142829659c21fd8a9def3f
    SHA1 | 1751849c1d709cdaef0b02a7350834a754b0e71d
    SHA256 | d093a940aebf6afdc6f616626049e97b1f9f70742a094511277c5f59eacc41ad

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
4. Azure Geçişi Toplayıcısı’nda **Önkoşulları ayarla** seçeneğini açın.
    - Lisans koşullarını kabul edin ve üçüncü taraf bilgilerini okuyun.
    - Toplayıcı, VM’nin İnternet erişimine sahip olup olmadığını denetler.
    - VM, proxy üzerinden İnternet erişimine sahipse **Proxy ayarları**’na tıklayın ve proxy adresini ve dinleme bağlantı noktasını belirtin. Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin.

    > [!NOTE]
    > Proxy adresi şu biçimde girilmelidir http://ProxyIPAdresi veya http://ProxyFQDN. Yalnızca HTTP proxy’si desteklenir.

    - Toplayıcı, toplayıcı hizmetinin çalışıp çalışmadığını denetler. Hizmet, toplayıcı VM’ye varsayılan olarak yüklenir.
    - VMware PowerCLI’yı indirin ve yükleyin.

5. **vCenter Server bilgilerini belirtin** bölümünde şunları yapın:
    - vCenter sunucusunun adını (FQDN) veya IP adresini belirtin.
    - **Kullanıcı adı** ve **Parola** bölümünde, toplayıcının vCenter sunucusundaki VM’leri bulmak için kullanacağı salt okunur hesabın kimlik bilgilerini belirtin.
    - **Toplama kapsamı**’nda, VM bulma için bir kapsam seçin. Toplayıcı yalnızca belirtilen kapsam içindeki VM’leri bulabilir. Kapsam belirli bir klasör, veri merkezi veya küme olarak ayarlanabilir. Kapsam 1.000 VM’den fazlasını içermemelidir. 

6. **Geçişi projesini belirtin** bölümünde portaldan kopyaladığınız Azure Geçişi proje kimliğini ve anahtarını belirtin. Bu bilgileri kopyalamadıysanız toplayıcı VM’den Azure portalını açın. Projenin **Genel Bakış** sayfasında **Makineleri Bul**’a tıklayın ve değerleri kopyalayın.  
7. **Toplama durumunu görüntüle** bölümünde bulma işlemini izleyin ve VM’lerden toplanan meta verilerin kapsam içinde olup olmadığını denetleyin. Toplayıcı, yaklaşık bir bulma süresi sağlar.

> [!NOTE]
> Toplayıcı, işletim sistemi dili ve toplayıcı arabirimi dili olarak yalnızca "İngilizce (ABD)"yi destekler. Yakında daha fazla dil desteği kullanıma sunulacaktır.


### <a name="verify-vms-in-the-portal"></a>VM’lerin portalda olup olmadığını doğrulama

Bulma süresi, kaç VM bulduğunuza bağlıdır. 100 VM için, toplayıcı çalışmayı durdurduktan sonra bulma işleminin tamamlanması genellikle yaklaşık bir saat sürer. 

1. Migration Planner projesinde **Yönet** > **Makineler**’e tıklayın.
2. Bulmak istediğiniz VM’lerin portalda görüntülenip görüntülenmediğini kontrol edin.


## <a name="create-and-view-an-assessment"></a>Değerlendirme oluşturma ve görüntüleme

VM’ler bulunduktan sonra bunları gruplandırın ve bir değerlendirme oluşturun. 

1. Projenin **Genel Bakış** sayfasında **+Değerlendirme oluştur**’a tıklayın.
2. Değerlendirme özelliklerini gözden geçirmek için **Tümünü görüntüle**’ye tıklayın.
3. Grubu oluşturun ve bir grup adı belirtin.
4. Gruba eklemek istediğiniz makineleri seçin.
5. Grubu ve değerlendirmeyi oluşturmak için **Değerlendirme Oluştur**’a tıklayın.
6. Değerlendirme oluşturulduktan sonra **Genel Bakış** > **Pano** bölümünde görüntüleyebilirsiniz.
7. Excel dosyası olarak indirmek için **Değerlendirmeyi dışarı aktar**’a tıklayın.

### <a name="assessment-details"></a>Değerlendirme ayrıntıları

Değerlendirme, şirket içi sanal makinelerin Azure için uyumlu olup olmadığı, Azure’da sanal makineyi çalıştırmak için doğru sanal makine boyutunun ne olacağı ve tahmini aylık Azure maliyetleri hakkında bilgileri içerir. 

![Değerlendirme raporu](./media/tutorial-assessment-vmware/assessment-report.png)

#### <a name="azure-readiness"></a>Azure için hazır olma

Değerlendirmedeki Azure için hazır olma görünümü, her bir sanal makinenin hazır olma durumunu gösterir. Sanal makinenin özelliklerine bağlı olarak her bir sanal makine şöyle işaretlenebilir:
- Azure için hazır
- Azure için koşullu olarak hazır
- Azure için hazır değil
- Hazır olma durumu bilinmiyor 

Azure Geçişi, hazır olan VM’ler için Azure’da bir VM boyutu önerir. Azure Geçişi’nin yaptığı boyut önerisi, değerlendirme özelliklerinde belirtilen boyutlandırma ölçütüne bağlıdır. Boyutlandırma ölçütü performansa dayalı boyutlandırma ise, sanal makinelerin performans geçmişi dikkate alınarak boyut önerisinde bulunulur. Boyutlandırma ölçütü 'şirket içi olarak' ise, şirket içi sanal makinenin boyutuna bakılarak öneride bulunulur (olduğu gibi boyutlandırma). Bu durumda kullanım verileri dikkate alınmaz. Azure Geçişi’nde boyutlandırmanın nasıl yapıldığı hakkında [daha fazla bilgi edinin](concepts-assessment-calculation.md). 

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

Azure Geçişi’ndeki her değerlendirme 1 yıldız ile 5 yıldız (1 yıldız en düşük, 5 yıldız en yüksektir) arasında değişen bir güvenilirlik derecesiyle ilişkilendirilir. Güvenilirlik derecelendirmesi, değerlendirmeyi hesaplamak için gereken veri noktalarının kullanılabilirliği temelinde bir değerlendirmeye atanır. Bir değerlendirmenin güvenilirlik derecesi, Azure Geçişi tarafından sağlanan boyut önerilerinin güvenilirliğini tahmin etmenize yardımcı olur. 

Azure Geçişi, kullanım tabanlı boyutlandırma gerçekleştirmek için yeterli miktarda veri noktasına sahip olmayabileceğinden güvenilirlik derecesi, *performans tabanlı boyutlandırmada* size fayda sağlar. Azure Geçişi, VM’yi boyutlandırmak için ihtiyaç duyduğu tüm veri noktalarına sahip olduğundan, *şirket içi boyutlandırma* için güvenilirlik derecesi her zaman 5 yıldızdır. 

VM’nin performans tabanlı boyutlandırması için Azure Geçişi, CPU ve bellek için kullanım verileri gerektirir. Ayrıca VM’ye takılı her disk için okuma/yazma IOPS ve aktarım hızı gereklidir. Sanal makineye eklenmiş her bir ağ bağdaştırıcısı için benzer şekilde Azure Geçişi’nin performansa dayalı boyutlandırma gerçekleştirmek amacıyla ağ giriş/çıkışına ihtiyacı vardır. Yukarıdaki kullanım rakamlarından herhangi biri vCenter Server’da mevcut değilse Azure Geçişi’nin yaptığı boyut önerisi güvenilir olmayabilir. Kullanılabilir veri noktalarının yüzdesine bağlı olarak değerlendirme için güvenilirlik derecelendirmesi sağlanır:

   **Veri noktalarının kullanılabilirliği** | **Güvenilirlik derecelendirmesi**
   --- | ---
   %0-%20 | 1 Yıldız
   %21-%40 | 2 Yıldız
   %41-%60 | 3 Yıldız
   %61-%80 | 4 Yıldız
   %81-%100 | 5 Yıldız

Aşağıdaki nedenlerle bir değerlendirme için tüm veri noktaları kullanılabilir olmayabilir:
- vCenter Server’daki istatistik ayarı 3 düzeyine ayarlanmamıştır ve değerlendirme için boyutlandırma ölçütü performansa dayalı boyutlandırma şeklindedir. vCenter Server’daki istatistik ayarı 3 düzeyinden küçükse vCenter Server’dan disk ve ağ için performans verileri toplanmaz. Bu durumda, Azure Geçişi tarafından disk ve ağ için sağlanan öneri, kullanım tabanlı olmaz. Azure Geçişi, diskin IOPS/iş hacmi göz önünde bulundurulmadan diskin Azure’da premium bir disk gerektirip gerektirmediğini belirleyemeyeceğinden depolama için standart diskleri önerir.
- vCenter Server’daki istatistik ayarı, keşif başlatılmadan önce daha kısa süre bir için 3. düzey olarak ayarlanmıştır. Örneğin, bugün istatistik ayarı düzeyini 3 olarak değiştirdiğiniz, yarın ise (24 saat sonra) toplayıcı gerecini kullanarak keşfi başlattığınız bir senaryoyu ele alalım. Bir gün için değerlendirme oluşturuyorsanız tüm veri noktalarına sahip olursunuz ve değerlendirmenin güvenilirlik derecesi 5 yıldız olur. Ancak değerlendirme özelliklerinde performans süresini bir ay olarak değiştiriyorsanız, son bir ay için disk ve ağ performansı verileri kullanılabilir halde olmayacağından güvenilirlik derecesi düşer. Son bir ayın performans verilerini dikkate almak istiyorsanız, keşif başlatmadan önce bir ay boyunca vCenter Server istatistik ayarını 3 düzeyinde tutmanız önerilir. 
- Değerlendirmenin hesaplandığı dönem boyunca birkaç sanal makine kapatılmıştır. Herhangi bir sanal makine belirli bir süre boyunca kapatıldıysa vCenter Server o süreye ait performans verilerine sahip olmaz. 
- Değerlendirmenin hesaplandığı dönem boyunca birkaç sanal makine oluşturulmuştur. Örneğin, son bir ayın performans geçmişi için değerlendirme oluşturuyorsanız, ancak yalnızca bir hafta önce ortamda birkaç sanal makine oluşturulduysa. Bu tür durumlarda yeni sanal makinelerin performans geçmişi, sürenin tamamı boyunca mevcut olmaz.

> [!NOTE]
> Herhangi bir değerlendirmenin güvenilirlik derecesi 4 Yıldız’ın altında ise vCenter Server istatistik ayarları düzeyini 3 olarak değiştirmeniz, değerlendirme için göz önünde bulundurmak istediğiniz süre (1 gün/1 hafta/1 ay) boyunca bekleyip daha sonra keşif ve değerlendirme gerçekleştirmeniz önerilir. Bu yapılamazsa, performansa dayalı boyutlandırma güvenilir olmayabilir ve değerlendirme özellikleri değiştirilerek *şirket içi olarak boyutlandırmaya* geçiş yapılması önerilir.
 
## <a name="next-steps"></a>Sonraki adımlar

- Geniş bir VMware ortamında bulma ve değerlendirme işlemlerinin nasıl yapılacağını [öğrenin](how-to-scale-assessment.md).
- [Makine bağımlılık eşlemesi](how-to-create-group-machine-dependencies.md) kullanarak yüksek güvenilirlikli değerlendirme grubu oluşturmayı öğrenin
- Değerlendirmelerin nasıl hesaplandığı hakkında [daha fazla bilgi](concepts-assessment-calculation.md) edinin.
