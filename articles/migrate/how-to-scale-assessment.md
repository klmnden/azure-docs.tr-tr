---
title: "Bulma ve değerlendirmesi Azure geçirme ile ölçeklendirin | Microsoft Docs"
description: "Çok sayıda Azure geçiş hizmeti ile şirket içi makineler değerlendirmek açıklar."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: dde0d07f-94b7-4b6a-a158-a89aa9324a35
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/22/2017
ms.author: raynew
ms.openlocfilehash: 930ec182cf329e7dda072dc49bd7f70abb413f2d
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="discover-and-assess-a-large-vmware-environment"></a>Bul ve büyük bir VMware ortamı değerlendirin

Bu makale, şirket içi makineler ile çok sayıda değerlendirmek açıklamaktadır [Azure geçirmek](migrate-overview.md). Azure geçirme bunlar Azure geçiş için uygun ve makine Azure'da çalışan için boyutlandırma ve maliyet tahminler sunar olup olmadığını denetlemek için makineler değerlendirir.

## <a name="prerequisites"></a>Ön koşullar

- **VMware**: en az bir VMware VM bulunan bir ESXi konağı veya sürüm 5.0 veya sonraki sürümlerini çalıştıran küme gerekir. Ana bilgisayar veya küme 5.5 veya 6.0 sürümünü çalıştıran bir vCenter sunucusu tarafından yönetiliyor olması gerekir.
- **vCenter hesap**: vCenter sunucusu için yönetici kimlik bilgilerine sahip bir salt okunur hesabı gerekir. Azure geçirme VM'ler bulmak için bu hesabı kullanır.
- **İzinleri**: vCenter sunucusunda, bir dosyada içeri aktararak bir VM oluşturma izinlerine sahip olmanız gerekir. OVA biçimi.
- **İstatistikleri ayarları**: dağıtıma başlamadan önce vCenter sunucusu istatistikleri ayarlarını düzeyine 2 veya sonrası, ayarlanmalıdır.

## <a name="plan-azure-migrate-projects"></a>Azure geçirmek projeleri planlama

Azure geçirmek projesinde 1500 makinelere değerlendirebilirsiniz. Tek bir bulma projesinde, en fazla 1000 makineler bulabilir.

- Bulmak için 1000'den az makineler varsa, tek bulma ile tek bir proje gerekir.
- 1000 ve 1500 makineler arasında varsa, tek bir proje ile iki bulmaları gerekir.
- Birden fazla 1500 makineleriniz varsa, birden çok proje oluşturmak ve birden fazla bulma işlemleri gerçekleştirmek, gereksinimlerinize göre gerekir. Örneğin:
    - 3000 makineler varsa, iki bulmaları iki projelerle ya da tek bir bulma üç projenin ayarlayabilirsiniz.
    - 5000 makineler varsa, dört projelerini ayarlayabilirsiniz. İki 1500 makinelerin bulma ve 500 makinelerin bulma ile bir. Alternatif olarak, her biri tek bir bulmayı beş projelerle ayarlayabilirsiniz. 
- Azure geçirmek bulmada yaptığınızda, VMware klasörü, veri merkezi veya küme için bulma kapsamını ayarlayabilirsiniz.
- Birden fazla bulma yapmak için vCenter, bulmak istediğiniz sanal makineleri klasörleri, veri merkezleri veya 1000 makineler sınırlandırılmasıdır destekleyen kümeleri doğrulayın.
- Değerlendirme amacıyla, makineler arası değerlendirmesi ve aynı proje bağımlılıkları ile tutmanızı öneririz. Bu nedenle, vCenter bağımlı makineler değerlendirme amaçları için aynı klasörü, veri merkezi veya küme içinde olduğundan emin olun.


## <a name="create-a-project"></a>Proje oluştur

Bir Azure geçirmek projesi, gereksinimlerinize uygun olarak oluşturun.

1. Azure portalında tıklatın **kaynak oluşturma**.
2. Arama **Azure geçirmek**ve hizmeti seçin (**Azure geçirme (Önizleme)** arama sonuçlarında. Sonra **Oluştur**’a tıklayın.
3. Bir proje adı ve proje için Azure aboneliği belirtin.
4. Yeni bir kaynak grubu oluşturun.
5. Projeyi oluşturmak ve ardından edileceği bölge belirtin **oluşturma**. Şirket içi Vm'lerden toplanan meta verileri bu bölgede depolanır.

## <a name="set-up-the-collector-appliance"></a>Toplayıcı Gereci ayarlayın

Azure geçirme toplayıcı uygulaması olarak bilinen bir şirket içi VM oluşturur. Bu VM, şirket içi VMware sanal makineleri bulur ve bunlarla ilgili meta verileri Azure geçiş hizmetine gönderir. Toplayıcı Gereci ayarlamak için indirme bir. OVA dosya ve VM oluşturmak için şirket içi vCenter sunucusuna aktarın.

### <a name="download-the-collector-appliance"></a>Toplayıcı Gereci indirin

Birden çok proje varsa, yalnızca Toplayıcı Gereci vCenter sunucusu için bir kez karşıdan yüklemek gerekir. Karşıdan yükleme ve Gereci ayarını etkinleştirirseniz sonra her proje için çalıştırın ve benzersiz proje kimliği ve anahtarı belirtin.

1. Azure geçirmek projeye tıklayarak **Başlarken** > **bulma & değerlendirin** > **Bul makineler**.
2. İçinde **Bul makineler**, tıklatın **karşıdan**, karşıdan yüklemek için. OVA dosyası.
3. İçinde **kopyalama proje kimlik**anahtarı proje için ve Kimliğini kopyalayın. Toplayıcı yapılandırırken bu gerekir.

   
### <a name="verify-the-collector-appliance"></a>Toplayıcı Gereci doğrulayın

Denetleyin. Dağıtmadan önce OVA dosyası güvenlidir.

1. Dosya karşıdan makinede bir yönetici komut penceresi açın.
2. Karma için OVA oluşturmak için aşağıdaki komutu çalıştırın:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek Kullanım:```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Üretilen karma bu ayarları eşleşmelidir.

    **Algoritması** | **Karma değeri**
    --- | ---
    MD5 | c283f00f46484bf673dc8383b01d0741 
    SHA1 | 8f49b47f53d051af1780bbc725659ce64e717ed4
    SHA256 | 7aecdbdb2aea712efe99d0c1444503f52d16de5768e783465e226fbbca71501d

## <a name="create-the-collector-vm"></a>Toplayıcı VM oluşturma

İndirilen Dosya vCenter Server'a alın.

1. VSphere istemci konsolunda tıklatın **dosya** > **OVF şablon dağıtma**.

    ![OVF dağıtma](./media/how-to-scale-assessment/vcenter-wizard.png)

2. OVF şablon Dağıtma Sihirbazı'nda > **kaynak**, .ova dosyasının konumunu belirtin.
3. İçinde **adı** ve **konumu**toplayıcısı VM için bir kolay ad belirtin ve stok nesne VM hangi barındırılacak içinde.
5. İçinde **konak/küme**, konak belirtin veya küme VM Toplayıcı çalıştırdığınız.
7. Depolama biriminde, VM Toplayıcı depolama hedefini belirtin.
8. İçinde **Disk biçimi**, disk türünü ve boyutunu belirtin.
9. İçinde **ağ eşlemesi**, VM Toplayıcı bağlanacağı ağı belirtin. Ağ Azure'a meta veri göndermek için internet bağlantısı gerekir. 
10. Gözden geçirin ve ayarları onaylayın ve ardından **son**.

## <a name="identify-the-key-and-id-for-each-project"></a>Her proje için kimliği ve anahtarı tanımlayın

Birden çok proje varsa, Kimliğini belirlemek ve her biri için anahtar emin olun. Sanal makineleri bulmak için toplayıcı çalıştırdığınızda bu anahtar gerekir.

1. Projesinde tıklayın **Başlarken** > **bulma & değerlendirin** > **Bul makineler**.
2. İçinde **kopyalama proje kimlik**anahtarı proje için ve Kimliğini kopyalayın. 
    ![Proje kimliği](./media/how-to-scale-assessment/project-id.png)

## <a name="vcenter-statistics-level-to-collect-the-performance-counters"></a>vCenter istatistikleri düzey performans sayaçlarını Topla
Bulma sırasında toplanan sayaçları listesi aşağıdadır. VCenter Server'daki çeşitli düzeyinde varsayılan sayaçlar şunlardır. Böylece tüm sayaçları doğru toplanan istatistikleri düzeyi için en yüksek ortak düzeyi (Düzey 3) ayarlamanızı öneririz. Daha düşük düzeyde ayarlamak vCenter varsa, yalnızca birkaç sayaçları tamamen 0 olarak ayarlayın rest ile toplanan. Bu nedenle değerlendirme eksik veri gösterebilir. Tabloda ayrıca belirli bir sayaç alınamadı, etkilenecek değerlendirme sonuçlarını listeler.

|Sayaç                                  |Düzey    |Cihaz düzeyinde  |Değerlendirme etkisi                               |
|-----------------------------------------|---------|------------------|------------------------------------------------|
|CPU.Usage.average                        | 1       |NA                |Önerilen VM boyutu ve maliyet                    |
|mem.Usage.average                        | 1       |NA                |Önerilen VM boyutu ve maliyet                    |
|virtualDisk.read.average                 | 2       |2                 |Disk boyutu, depolama maliyeti ve VM boyutu         |
|virtualDisk.write.average                | 2       |2                 |Disk boyutu, depolama maliyeti ve VM boyutu         |
|virtualDisk.numberReadAveraged.average   | 1       |3                 |Disk boyutu, depolama maliyeti ve VM boyutu         |
|virtualDisk.numberWriteAveraged.average  | 1       |3                 |Disk boyutu, depolama maliyeti ve VM boyutu         |
|NET.Received.average                     | 2       |3                 |VM boyutu ve ağ maliyet                        |
|NET.transmitted.average                  | 2       |3                 |VM boyutu ve ağ maliyet                        |

> [!WARNING]
> Daha yüksek bir istatistik düzeyi yeni ayarladıysanız, performans sayaçlarını oluşturmak için günde değerine kadar sürer. Bu nedenle, bir günün ardından bulma çalıştırırken önerilir.

## <a name="run-the-collector-to-discover-vms"></a>Sanal makineleri bulmak için toplayıcı çalıştırın

Gerçekleştirmek için gereken her bulma bulma gerekli kapsamında VM'ler için toplayıcı çalıştırın. Bir bulma art arda çalıştırın. Eşzamanlı bulmaları desteklenmez ve her bir keşfin farklı bir kapsama sahip olmalıdır.

1. VSphere istemci konsolunda VM'ye sağ tıklayın > **açık Konsolu**.
2. Dil, saat dilimi ve Gereci parola tercihlerini sağlar.
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
    - n **Gruplandırma için etiket kategorisi**seçin **hiçbiri**.

        ![Kapsam seçin](./media/how-to-scale-assessment/select-scope.png)

1. İçinde **seçin proje**anahtarı proje için ve Kimliğini belirtin. Siz bunları kopyalayın, Toplayıcı VM Azure Portalı'nı açın. Projedeki **genel bakış** sayfasında, **Bul makineler**ve değerlerini kopyalayın.  
İçinde **tam bulma**, Keşif sürecini izleyebilir ve VM'lerin toplanan meta verilerin kapsamında olduğunu denetleyin. Toplayıcı bir yaklaşık bulma süresi sağlar.


### <a name="verify-vms-in-the-portal"></a>Portalda VM'ler doğrulayın

Kaç tane sanal makinelerin süre bağlıdır bulma, bulma. Genellikle, çalışan Toplayıcı bittikten sonra 100 VM'ler için bir saat tamamlanması için geçici alır. 

1. Geçiş Planlayıcısı projesinde tıklayın **Yönet** > **makineler**.
2. Bulmak istediğiniz sanal makineleri portalda görünür denetleyin.


## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [bir grup oluşturmak](how-to-create-a-group.md) değerlendirmesi için.
- [Daha fazla bilgi edinin](concepts-assessment-calculation.md) değerlendirmelerinin nasıl hesaplandığını hakkında.