---
title: "Azure’a geçiş için şirket içi VMware VM’lerini Azure Geçişi ile bulma ve değerlendirme | Microsoft Docs"
description: "Azure’a geçiş için şirket içi VMware VM’lerinin Azure Geçişi hizmeti kullanılarak nasıl bulunacağı ve değerlendirileceği açıklanmaktadır."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 01/08/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: bbcfe95f5427681f8d55d647b102d35fc37f15ee
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2018
---
# <a name="discover-and-assess-on-premises-vmware-vms-for-migration-to-azure"></a>Azure’a geçiş için şirket içi VMware VM’lerini bulma ve değerlendirme

[Azure Geçişi](migrate-overview.md) hizmetleri, Azure’a geçiş için şirket içi iş yüklerini değerlendirir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir Azure Geçişi projesi oluşturun.
> * Değerlendirmek için şirket içi VMware VM’lerini toplamak üzere bir şirket içi toplayıcı sanal makinesi (VM) ayarlayın.
> * VM’leri gruplandırın ve bir değerlendirme oluşturun.


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.


## <a name="prerequisites"></a>Ön koşullar

- **VMware**: Geçirmeyi planladığınız VM’ler 5.5, 6.0 veya 6.5 sürümüne sahip bir vCenter Server tarafından yönetilmelidir. Buna ek olarak, toplayıcı VM’yi dağıtmak için 5.0 veya daha sonraki sürüme sahip bir ESXi konağı gerekir. 
 
> [!NOTE]
> Yol haritamızda bulunan Hyper-V desteği kısa süre içinde etkinleştirilecektir. 

- **vCenter Server hesabı**: vCenter Server’a erişmek için salt okunur bir hesabınız olması gerekir. Azure Geçişi, şirket içi VM’leri bulmak için bu hesabı kullanır.
- **İzinler**: vCenter Server’da, bir dosyayı .OVA biçiminde içeri aktararak VM oluşturma iznine sahip olmanız gerekir. 
- **İstatistik ayarları**: vCenter Server için istatistik ayarları, dağıtım başlatılmadan önce düzey 3 olarak belirlenmelidir. Ayarlar düzey 3’ün altında olursa değerlendirme gerçekleştirilir, ancak depolama ve ağ için performans verileri toplanmaz. Bu durumda boyut önerileri, CPU ve belleğe ait performans verilerine ve diskin ve ağ bağdaştırıcılarının yapılandırma verilerine bağlı olarak yapılır. 

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma
[Azure Portal](https://portal.azure.com)’da oturum açın.

## <a name="create-a-project"></a>Proje oluşturma

1. Azure portalında **Kaynak oluştur**’a tıklayın.
2. **Azure Geçişi** için arama yapın ve arama sonuçlarında **Azure Geçişi (önizleme)** seçeneğini belirleyin. Sonra **Oluştur**’a tıklayın.
3. Proje için bir proje adı ve Azure aboneliği belirtin.
4. Yeni bir kaynak grubu oluşturun.
5. Projenin oluşturulacağı konumu belirtin ve sonra **Oluştur**’a tıklayın. Bu önizleme için Azure Geçişi projesini yalnızca Orta Batı ABD bölgesinde oluşturabilirsiniz. Ancak yine de herhangi bir hedef Azure konumu için geçişinizi planlayabilirsiniz. Proje için belirtilen konum yalnızca şirket içi VM’lerden toplanan meta verileri depolamak için kullanılır. 

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
2. Gereç için dil, saat dilimi ve parola tercihlerini girin.
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

### <a name="sample-assessment"></a>Örnek değerlendirme

Burada örnek bir değerlendirme raporu bulabilirsiniz. Bu rapor, VM’lerin Azure için uyumlu olup olmadığı ve tahmini aylık maliyetleri ile ilgili bilgiler içerir. 

![Değerlendirme raporu](./media/tutorial-assessment-vmware/assessment-report.png)

#### <a name="azure-readiness"></a>Azure için hazır olma

Bu görünümde her bir makinenin hazır olma durumu görüntülenir.

- Azure Geçişi, hazır olan VM’ler için Azure’da bir VM boyutu önerir.
- Azure Geçişi, hazır olmayan VM’ler için bu durumun nedenini açıklar ve düzeltme adımları sunar.
- Azure Geçişi, geçiş için kullanabileceğiniz araçları önerir. Makine lift-and-shift geçişine uygunsa Azure Site Recovery hizmeti önerilir. Bir veritabanı makinesiyse Azure Veritabanı Geçiş Hizmeti önerilir.

  ![Hazır olma durumu değerlendirmesi](./media/tutorial-assessment-vmware/assessment-suitability.png)  

#### <a name="monthly-cost-estimate"></a>Aylık maliyet tahmini

Bu görünümde, Azure’da çalışan VM’lerin toplam işlem ve depolama maliyetinin yanı sıra her makineye ilişkin ayrıntılar da görüntülenir. Maliyet tahminleri, bir makine ve disklerine yönelik performans tabanlı boyut önerileri ve değerlendirme özellikleri kullanılarak hesaplanır. 

> [!NOTE]
> Azure Geçişi tarafından sağlanan maliyet tahmini, hizmet olarak Azure Altyapısı (IaaS) VM’leri biçiminde çalıştırılan şirket içi VM’lerine yöneliktir. Azure Geçişi, Hizmet olarak platform (PaaS) veya Hizmet olarak yazılım (SaaS) maliyetlerini hesaba katmaz. 

İşlem ve depolama için tahmini aylık maliyetler gruptaki tüm VM’ler için birleştirilir. 

![VM maliyeti değerlendirmesi](./media/tutorial-assessment-vmware/assessment-vm-cost.png) 

Belirli bir makinenin bilgilerini görüntülemek için detaya gidebilirsiniz.

![VM maliyeti değerlendirmesi](./media/tutorial-assessment-vmware/assessment-vm-drill.png) 

## <a name="next-steps"></a>Sonraki adımlar

- Geniş bir VMware ortamında bulma ve değerlendirme işlemlerinin nasıl yapılacağını [öğrenin](how-to-scale-assessment.md).
- [Makine bağımlılık eşlemesi](how-to-create-group-machine-dependencies.md) kullanarak yüksek güvenilirlikli değerlendirme grubu oluşturmayı öğrenin
- Değerlendirmelerin nasıl hesaplandığı hakkında [daha fazla bilgi](concepts-assessment-calculation.md) edinin.
