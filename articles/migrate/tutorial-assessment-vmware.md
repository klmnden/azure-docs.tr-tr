---
title: "Bulmak ve şirket içi VMware VM'ler Azure ile Azure geçirmek için geçiş için değerlendirmek | Microsoft Docs"
description: "Bulmak ve şirket içi VMware VM'ler Azure geçiş hizmetini kullanarak azure'a, geçiş için değerlendirmek açıklar."
services: migration-planner
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a2521630-730f-4d8b-b298-e459abdced46
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/22/2017
ms.author: raynew
ms.openlocfilehash: fd9513cda899a7ac2a51c4e1ab03a80ce0adf2fd
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="discover-and-assess-on-premises-vmware-vms-for-migration-to-azure"></a>Bul ve şirket içi VMware sanal makineleri geçiş için Azure değerlendirin

[Azure geçirmek](migrate-overview.md) Hizmetleri geçiş Azure için şirket içi iş yüklerini değerlendirir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir Azure geçirmek projesi oluşturun.
> * Şirket içi VMware Vm'lerini değerlendirmesi için bulmak için bir şirket içi Toplayıcı sanal makinesini (VM) ayarlayın.
> * Sanal makineleri gruplandırın ve bir değerlendirme oluşturun.


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.


## <a name="prerequisites"></a>Ön koşullar

- **VMware**: en az bir VMware VM bulunan bir ESXi konağı veya sürüm 5.0 veya sonraki sürümlerini çalıştıran küme gerekir. Ana bilgisayar veya küme 5.5, 6.0 veya 6.5 sürümünü çalıştıran bir vCenter sunucusu tarafından yönetiliyor olması gerekir.
- **vCenter hesap**: vCenter sunucusu için yönetici kimlik bilgilerine sahip bir salt okunur hesabı gerekir. Azure geçirme VM'ler bulmak için bu hesabı kullanır.
- **İzinleri**: vCenter sunucusunda, bir dosyada içeri aktararak bir VM oluşturma izinlerine sahip olmanız gerekir. OVA biçimi. 
- **İstatistikleri ayarları**: dağıtıma başlamadan önce düzeyi 3 vCenter sunucusu istatistikleri ayarlarını ayarlamanız gerekir. Düzey 3 değerlendirme belirtilenden düşük çalışır, ancak depolama ve ağ için performans verilerini toplanan değil

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma
[Azure Portal](https://portal.azure.com)’da oturum açın.

## <a name="create-a-project"></a>Proje oluştur

1. Azure portalında tıklatın **kaynak oluşturma**.
2. Arama **Azure geçirmek**ve hizmeti seçin (**Azure geçirme (Önizleme)** arama sonuçlarında. Sonra **Oluştur**’a tıklayın.
3. Bir proje adı ve proje için Azure aboneliği belirtin.
4. Yeni bir kaynak grubu oluşturun.
5. Projeyi oluşturmak ve ardından edileceği bölge belirtin **oluşturma**. Şirket içi Vm'lerden toplanan meta verileri bu bölgede depolanır. Bu önizleme için Batı Orta ABD bölgede bir Azure geçirmek proje yalnızca oluşturabilirsiniz. Ancak, sanal makineleri için farklı bir konum değerlendirebilirsiniz.

    ![Azure Geçişi](./media/tutorial-assessment-vmware/project-1.png)
    


## <a name="download-the-collector-appliance"></a>Toplayıcı Gereci indirin

Azure geçirme toplayıcı uygulaması olarak bilinen bir şirket içi VM oluşturur. Bu VM, şirket içi VMware sanal makineleri bulur ve bunlarla ilgili meta verileri Azure geçiş hizmetine gönderir. Toplayıcı Gereci ayarlamak için indirme bir. OVA dosya ve VM oluşturmak için şirket içi vCenter sunucusuna aktarın.

1. Azure geçirmek projeye tıklayarak **Başlarken** > **bulma & değerlendirin** > **Bul makineler**.
2. İçinde **Bul makineler**, tıklatın **karşıdan**, karşıdan yüklemek için. OVA dosyası.
3. İçinde **kopyalama proje kimlik**anahtarı ve proje Kimliğini kopyalayın. Toplayıcı yapılandırırken bu gerekir.

    ![.OVA dosyasını indirin](./media/tutorial-assessment-vmware/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Toplayıcı Gereci doğrulayın

Denetleyin. Dağıtmadan önce OVA dosyası güvenlidir.

1. Dosya karşıdan makinede bir yönetici komut penceresi açın.
2. Karma için OVA oluşturmak için aşağıdaki komutu çalıştırın:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek Kullanım:```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Üretilen karma bu ayarları eşleşmelidir.

    **Algoritması** | **Karma değeri**
    --- | ---
    MD5 | dd27dd6ace28f9195a2b5d52a4003067 
    SHA1 | d2349e06a5d4693fc2a1c0619591b9e45c36d695
    SHA256 | 1492a0c6d6ef76e79269d5cd6f6a22f336341e1accbc9e3dfa5dad3049be6798

## <a name="create-the-collector-vm"></a>Toplayıcı VM oluşturma

İndirilen Dosya vCenter Server'a alın.

1. VSphere istemci konsolunda tıklatın **dosya** > **OVF şablon dağıtma**.

    ![OVF dağıtma](./media/tutorial-assessment-vmware/vcenter-wizard.png)

2. OVF şablon Dağıtma Sihirbazı'nda > **kaynak**, .ova dosyasının konumunu belirtin.
3. İçinde **adı** ve **konumu**toplayıcısı VM için bir kolay ad belirtin ve stok nesne VM hangi barındırılacak içinde.
5. İçinde **konak/küme**, konak belirtin veya küme VM Toplayıcı çalıştırdığınız.
7. Depolama biriminde, VM Toplayıcı depolama hedefini belirtin.
8. İçinde **Disk biçimi**, disk türünü ve boyutunu belirtin.
9. İçinde **ağ eşlemesi**, VM Toplayıcı bağlanacağı ağı belirtin. Ağ Azure'a meta veri göndermek için internet bağlantısı gerekir. 
10. Gözden geçirin ve ayarları onaylayın ve ardından **son**.


## <a name="run-the-collector-to-discover-vms"></a>Sanal makineleri bulmak için toplayıcı çalıştırın

1. VSphere istemci konsolunda VM'ye sağ tıklayın > **açık Konsolu**.
2. Dil ve saat dilimi Gereci parola tercihlerini sağlar.
3. Masaüstünde **Toplayıcı çalıştırmak** kısayol.
4. Azure geçirmek Toplayıcı ' açık **önkoşulları ayarlamak**.
    - Lisans koşullarını kabul edin ve üçüncü taraf bilgileri okuyun.
    - Toplayıcı VM Internet erişimi olduğunu denetler.
    - VM Internet proxy üzerinden erişirse, tıklatın **Proxy ayarlarını**, proxy adresi ve dinleme bağlantı noktasını belirtin. Proxy kimlik doğrulaması gerektiriyorsa kimlik bilgilerini belirtin.
    - Toplayıcı, Windows profil oluşturucu hizmetinin çalıştığını denetler. Hizmeti toplayıcısı VM üzerinde varsayılan olarak yüklenir.
    - VMware Powerclı yükleyip yeniden açın.
. İçinde **Bul makineler**, aşağıdakileri yapın:
    - Adı (FQDN) veya vCenter sunucusunun IP adresini belirtin.
    - İçinde **kullanıcı adı** ve **parola**, Toplayıcı vCenter sunucusu üzerinde sanal makineleri bulmak için kullanacağı salt okunur hesap kimlik bilgilerini belirtin.
    - İçinde **koleksiyonu kapsam**, VM keşfi için kapsamı seçin. Toplayıcı, yalnızca sanal makineleri için belirtilen kapsamda bulabilir. Kapsamı belirli klasör, veri merkezi veya küme ayarlayabilirsiniz. 1000'den fazla VMs içermemelidir. 
    - İçinde **gruplandırma için etiket kategorisi**seçin **hiçbiri**.
1. İçinde **seçin proje**portalından kopyalandığından anahtarı ve Azure geçirmek proje kimliği belirtin. Siz bunları kopyalayın, Toplayıcı VM Azure Portalı'nı açın. Projedeki **genel bakış** sayfasında, **Bul makineler**ve değerlerini kopyalayın.  
2. İçinde **tam bulma**izlemek bulma ve VM'lerin toplanan meta verilerin kapsamında olduğunu denetleyin. Toplayıcı bir yaklaşık bulma süresi sağlar.


### <a name="verify-vms-in-the-portal"></a>Portalda VM'ler doğrulayın

Kaç tane sanal makinelerin süre bağlıdır bulma, bulma. Genellikle, çalışan Toplayıcı bittikten sonra 100 VM'ler için bir saat tamamlanması için geçici alır. 

1. Geçiş Planlayıcısı projesinde tıklayın **Yönet** > **makineler**.
2. Bulmak istediğiniz sanal makineleri portalda görünür denetleyin.


## <a name="create-and-view-an-assessment"></a>Oluşturun ve bir değerlendirme görüntüleyin

VM'ler bulunduktan sonra bunları gruplamak ve bir değerlendirme oluşturun. 

1. Projedeki **genel bakış** sayfasında, **+ değerlendirmesi oluşturma**.
2. Tıklatın **tüm görüntüle** değerlendirme ayarlarını gözden geçirmek için.
3. Grubu oluşturmak ve bir grup adı belirtin.
4. Gruba eklemek istediğiniz makineleri seçin.
5. Tıklatın **oluşturma değerlendirme**, Grup ve değerlendirme oluşturmak için.
6. Değerlendirme oluşturulduktan sonra bunu görüntülemek **genel bakış** > **Pano**.
7. Tıklatın **verme değerlendirme**, bir Excel dosyası olarak karşıdan yüklemek için.

### <a name="sample-assessment"></a>Örnek değerlendirme

Burada, örnek değerlendirme raporu verilmiştir. Sanal makineleri Azure ve aylık tahmini maliyetleri uyumlu olup olmadığı hakkında bilgiler içerir. 

![Değerlendirme raporu](./media/tutorial-assessment-vmware/assessment-report.png)

#### <a name="azure-readiness"></a>Azure Hazırlık

Bu görünüm, her makine için hazırlık durumu gösterir.

- Hazır olan VM'ler için Azure geçirmek Azure VM boyutu önerir.
- Hazır olmayan sanal makineleri için Azure geçirmek, nedenini açıklar ve düzeltme adımları sağlar.
- Azure geçirme geçiş için kullanabileceğiniz araçlar önerir. Makine yükseltme ve shift geçiş için uygun değilse, Azure Site Recovery hizmeti önerilir. Veritabanı makine ise, Azure veritabanı geçiş hizmeti önerilir.

  ![Değerlendirme Hazırlık](./media/tutorial-assessment-vmware/assessment-suitability.png)  

#### <a name="monthly-cost-estimate"></a>Aylık maliyeti tahmin

Bu görünüm, işlem ve her makine için depolama maliyeti gösterir. Maliyet tahminleri bir makine ve kendi diskleri ve değerlendirme özellikleri için performans tabanlı boyutu önerileri kullanarak hesaplanır.

Tahmini aylık maliyetlerini hesaplama ve depolama grubundaki tüm VM'ler için toplanır. Ayrıntılar için detaya gitmek için her makinede tıklatabilirsiniz. 

![VM maliyet değerlendirme](./media/tutorial-assessment-vmware/assessment-vm-cost.png) 

Belirli bir makine için bkz: maliyetlerini azaltmak inebilir.

![VM maliyet değerlendirme](./media/tutorial-assessment-vmware/assessment-vm-drill.png) 

## <a name="next-steps"></a>Sonraki adımlar

- [Bilgi](how-to-scale-assessment.md) çok sayıda şirket içi makineler için bir değerlendirme kurulur.
- Kullanarak daha ayrıntılı değerlendirme grupları oluşturma hakkında bilgi edinin [makine bağımlılık eşleme](how-to-create-group-machine-dependencies.md)
- [Daha fazla bilgi edinin](concepts-assessment-calculation.md) değerlendirmelerinin nasıl hesaplandığını hakkında.
