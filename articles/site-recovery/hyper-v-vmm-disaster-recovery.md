---
title: Azure Site Recovery ile şirket içi siteler arasında Hyper-V Vm'leri için olağanüstü durum kurtarma ayarlama | Microsoft Docs
description: Azure Site Recovery ile şirket içi siteleriniz arasında Hyper-V Vm'leri için olağanüstü durum kurtarmayı ayarlayın öğrenin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: raynew
ms.openlocfilehash: 4cc4da130d9253bf40e5d02806088a95b2195e7c
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37915917"
---
# <a name="set-up-disaster-recovery-for-hyper-v-vms-to-a-secondary-on-premises-site"></a>Olağanüstü durum kurtarma, Hyper-V Vm'leri için ikincil şirket içi siteye ayarlayın.

[Azure Site Recovery](site-recovery-overview.md) hizmeti, şirket içi makinelerin ve Azure sanal makinelerinin (VM) çoğaltma, yük devretme ve yeniden çalışma işlemlerini yöneterek ve düzenleyerek olağanüstü durum kurtarma stratejinize katkı sağlar.

Şirket içi Hyper-V sanal makinelerini System Center Virtual Machine Manager (VMM) bulutlarında yönetilen için bu makalede, ikincil bir siteye olağanüstü durum kurtarma ayarlama işlemini göstermektedir. Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Şirket içi VMM sunucuları ve Hyper-V konaklarını hazırlama
> * Site Recovery için bir kurtarma Hizmetleri kasası oluşturma 
> * Kaynağını Ayarla ve çoğaltma ortamları hedefleyin. 
> * Ağ eşlemesi ayarlayın 
> * Çoğaltma ilkesi oluşturma
> * VM için çoğaltmayı etkinleştirme

## <a name="prerequisites"></a>Önkoşullar

Bu senaryoyu tamamlamak için:

- Gözden geçirme [senaryo mimarisini ve bileşenlerini](hyper-v-vmm-architecture.md).
- VMM sunucuları ve Hyper-V konakları ile uyumlu olduğundan emin [destek gereksinimlerini](hyper-v-vmm-secondary-support-matrix.md).
- Çoğaltmak istediğiniz VM'lerin uyumlu onay [makine desteği çoğaltılan](hyper-v-vmm-secondary-support-matrix.md#replicated-vm-support).
- VMM sunucuları ağ eşlemesi için hazırlanın.

### <a name="prepare-for-network-mapping"></a>Ağ eşlemesi için hazırlanma

[Ağ eşlemesini](hyper-v-vmm-network-mapping.md) arasındaki eşlemeleri şirket içi kaynak ile hedef Bulutlar, VMM VM ağları. Eşleme şunları yapar:

- Vm'leri yük devretme sonrasında uygun hedef VM ağlarına bağlar. 
- En uygun şekilde hedef Hyper-V ana bilgisayar sunucuları üzerinde çoğaltma Vm'leri yerleştirir. 
- Ağ eşlemesini yapılandırmazsanız, çoğalma VM'ler için bir VM ağı yük devretmeden sonra bağlanması gerekmez.

VMM aşağıdaki gibi hazırlayın:

1. Olduğundan emin olun [VMM mantıksal ağlarına](https://docs.microsoft.com/system-center/vmm/network-logical) kaynak ve hedef VMM sunucularında.
    - Mantıksal ağ kaynak sunucudaki Hyper-V konaklarının bulunduğu kaynak bulutla ilişkili olmalıdır.
    - Hedef sunucudaki mantıksal ağı hedef bulutla ilişkili olmalıdır.
1. Olduğundan emin olun [VM ağları](https://docs.microsoft.com/system-center/vmm/network-virtual) kaynak ve hedef VMM sunucularında. VM ağları her konumdaki mantıksal ağ bağlantılı olması gerekir.
2. Kaynak VM ağına kaynak Hyper-V konaklarında VM'ler bağlanın. 


## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="choose-a-protection-goal"></a>Koruma hedefi seçin

Neleri çoğaltmak istediğinizi ve bunları nereye çoğaltacağınızı seçin.

1. Tıklayın **Site Recovery** > **1. adım: altyapıyı hazırlama** > **koruma hedefi**.
2. Seçin **kurtarma sitesine**seçip **Evet, Hyper-V ile**.
3. Seçin **Evet** Hyper-V konakları yönetmek için VMM kullandığınızı belirtmek için.
4. Seçin **Evet** ikincil bir VMM sunucusu varsa. Tek bir VMM sunucusundaki Bulutlar arasında çoğaltmayı dağıtıyorsanız tıklayın **Hayır**. Daha sonra, **Tamam**'a tıklayın.


## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Azure Site kurtarma sağlayıcısı VMM sunucularında yükleme keşfedin ve sunucuları kasaya kaydedin.

1. **Altyapıyı Hazırlama** > **Kaynak** seçeneklerine tıklayın.
2. **Kaynağı ayarla** kısmında, bir VMM sunucusu eklemek için **+ VMM**'ye tıklayın.
3. İçinde **Sunucusu Ekle**, kontrol **System Center VMM sunucusunun** görünür **sunucu türü**.
4. Azure Site Recovery Sağlayıcısı yükleme dosyasını indirin.
5. Kayıt anahtarını indirin. Sağlayıcı yüklediğinizde buna ihtiyacınız olur. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

    ![Kaynağı ayarlama](./media/hyper-v-vmm-disaster-recovery/source-settings.png)

6. Her VMM sunucusunda sağlayıcıyı yükleyin. Açıkça herhangi bir Hyper-V konaklarında yükleme gerekmez.

### <a name="install-the-azure-site-recovery-provider"></a>Azure Site Recovery Sağlayıcısı'nı yükleme

1. Sağlayıcı kurulum dosyasını her VMM sunucusunda çalıştırın. VMM bir kümede dağıtılıyorsa, ilk kez aşağıda gösterildiği gibi yükleyin:
    -  Etkin bir düğümde sağlayıcıyı yükleyin ve VMM sunucusunu kasaya kaydetmek için yüklemeyi sonlandırın.
    - Ardından, sağlayıcı diğer düğümlere yükleyin. Küme düğümleri aynı sağlayıcı sürümünü çalıştırması.
2. Kurulum, bazı ön koşul denetimlerini çalıştırır ve VMM hizmetini durdurmak için izin ister. Kurulum bittiğinde VMM hizmeti otomatik olarak yeniden başlatılır. Bir VMM kümesinde yüklerseniz, küme rolünü durdurmanız istenir.
3. İçinde **Microsoft Update**, sağlayıcı güncelleştirmeleri Microsoft Update ilkenize uygun şekilde yüklenir belirtmek için de seçebilirsiniz.
4. İçinde **yükleme**kabul edin veya varsayılan yükleme konumunu değiştirmek ve tıklayın **yükleme**.
5. Yükleme tamamlandıktan sonra tıklayın **kaydetme** sunucuyu kasaya kaydetmek için.

    ![Yükleme konumu](./media/hyper-v-vmm-disaster-recovery/provider-register.png)
6. **Kasa adı** alanında, sunucunun kayıtlı olduğu kasanın adını doğrulayın. **İleri**’ye tıklayın.
7. İçinde **Ara sunucu bağlantısı**, VMM sunucusunda çalışan Sağlayıcı'nın Azure'a nasıl bağlandığını belirtin.
   - Sağlayıcı doğrudan İnternet'e veya bir ara sunucu aracılığıyla bağlanmanız gerekir belirtebilirsiniz. Proxy ayarlarını gereken şekilde belirtin.
   - Bir ara sunucu kullanırsanız belirtilen ara sunucu kimlik bilgilerini kullanarak bir VMM RunAs hesabı (DRAProxyAccount) otomatik olarak oluşturulur. Bu hesabın kimlik doğrulamasını başarıyla gerçekleştirebilmesi için ara sunucuyu yapılandırın. RunAs hesabı ayarları VMM konsolundan değiştirilebilir > **ayarları** > **güvenlik** > **farklı çalıştır hesapları**.
   - Değişiklikleri güncelleştirmek için VMM hizmetini yeniden başlatın.
8. İçinde **kayıt anahtarı**, indirdiğiniz ve VMM sunucusuna kopyaladığınız öğeyi anahtarı seçin.
9. Şifreleme ayarı bu senaryoyla ilgili değildir. 
10. **Sunucu adı** alanında, kasadaki VMM sunucusunu tanımlamak için bir kolay ad belirtin. Bir kümede bulunan bir VMM kümesi rolü adını belirtin.
11. İçinde **bulut meta verilerini eşitle**VMM sunucusundaki tüm bulutların meta verilerini eşitlemek isteyip istemediğinizi seçin. Bu eylemin her sunucuda yalnızca bir kez gerçekleştirilmesi gerekir. Tüm Bulutları eşitlemek istemiyorsanız bu ayarı işaretlemeyin. Her Bulutu VMM konsolundaki bulut özellikleri ayrı ayrı eşitleyebilirsiniz.
12. İşlemi tamamlamak için **İleri**'ye tıklayın. Kayıttan sonra Site Recovery VMM sunucusundan meta verilerini alır. Sunucu görüntülenen **sunucuları** > **VMM sunucuları** kasadaki.
13. Sunucu kasada göründükten sonra **kaynak** > **kaynağı hazırla** VMM sunucusunu seçin ve Hyper-V konağının bulunduğu Bulutu seçin. Daha sonra, **Tamam**'a tıklayın.


## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Hedef VMM sunucusunu ve Bulutu seçin:

1. Tıklayın **altyapıyı hazırlama** > **hedef**ve hedef VMM sunucusunu seçin.
2. Site Recovery ile eşitlenen VMM bulutlarında görüntülenir. Hedef bulutunu seçin.

   ![Hedef](./media/hyper-v-vmm-disaster-recovery/target-vmm.png)


## <a name="set-up-a-replication-policy"></a>Çoğaltma ilkesi ayarlama

Başlamadan önce ilkeyi kullanan tüm konaklar aynı işletim sistemini sahip olduğunuzdan emin olun. Konaklar farklı Windows Server sürümleri çalıştırıyorsanız, birden fazla çoğaltma ilkesi gerekir.

1. Yeni bir çoğaltma ilkesi oluşturmak için **Altyapıyı hazırlama** > **Çoğaltma Ayarları** > **+Oluştur ve ilişkilendir** seçeneklerine tıklayın.
2. **İlke oluştur ve ilişkilendir** bölümünde bir ilke adı belirtin. Kaynak ve hedef tür **Hyper-V**.
3. İçinde **Hyper-V konak sürümü**, hangi işletim sistemi çalıştıran konakta seçin.
4. İçinde **kimlik doğrulama türü** ve **kimlik doğrulaması bağlantı noktası**, birincil ve kurtarma Hyper-V konak sunucuları arasında trafik nasıl doğrulandığını belirtin.
    - Seçin **sertifika** çalışan bir Kerberos ortam yoksa. Azure Site Recovery, HTTPS kimlik doğrulaması için sertifikalar otomatik olarak yapılandırır. El ile herhangi bir şey yapmanız gerekmez.
    - Varsayılan olarak, bağlantı noktası 8083 ve 8084 (Sertifikalar) Hyper-V konak sunucuları üzerinde Windows Güvenlik duvarında açılacak.
    - Seçerseniz **Kerberos**, bir Kerberos anahtarı konak sunucularının karşılıklı kimlik doğrulaması için kullanılır. Kerberos, yalnızca Windows Server 2012 R2 veya üzeri çalıştıran Hyper-V konak sunucuları için geçerlidir.
1. **Kopyalama sıklığı** kısmında, ilk çoğaltmadan sonra değişim verilerini ne sıklıkta çoğaltacağınızı belirleyin (30 saniyede, 5 veya 15 dakikada bir).
2. İçinde **kurtarma noktası bekletme**, \how belirtin uzun süre (saat), her kurtarma noktası için bekletme süresinin olacaktır. Çoğaltılan makineler pencere içindeki herhangi bir noktaya kurtarılabilir.
3. İçinde **uygulamayla tutarlı anlık görüntü sıklığı**, ne sıklıkla (1-12 saat) içeren uygulamayla tutarlı anlık görüntü kurtarma noktaları belirleyin oluşturulur. Hyper-V iki çeşit anlık görüntü kullanır:
    - **Standart anlık görüntü**: tüm sanal makinenin artımlı anlık görüntüsünü sağlar.
    - **Uygulamayla tutarlı anlık görüntü**: sanal Makinenin içindeki uygulama verilerinin zaman içinde nokta anlık görüntüsünü alır. Birim Gölge Kopyası Hizmeti (VSS) anlık görüntü alınırken uygulamaların tutarlı bir durumda olmasını sağlar. Uygulamayla tutarlı anlık görüntüleri etkinleştirme, kaynak Vm'leri üzerinde uygulama performansını etkiler. Yapılandırdığınız ilave kurtarma noktası sayısından küçük bir değere ayarlayın.
4. İçinde **veri aktarımı sıkıştırma**, aktarılan çoğaltma verilerini sıkıştırılmasının gerekli olup olmadığını belirtin.
5. Seçin **VM çoğaltmayı Sil**kaynak VM için korumayı devre dışı bırakırsanız, çoğaltma sanal makinesinin silinmesi gerektiğini belirtmek için. Kaynak Site Recovery konsolundan kaldırılır VM için korumayı devre dışı bıraktığınızda bu ayarı etkinleştirirseniz, VMM için Site Recovery ayarları VMM konsolundan kaldırılır ve çoğaltma silinir.
6. İçinde **ilk çoğaltma yöntemini**, ağ üzerinden çoğaltma yapıyorsanız ilk çoğaltmanın başlatılacağı veya zamanlamak etkinleştirilip etkinleştirilmeyeceğini belirtin. Ağ bant genişliğinden tasarruf etmek meşgul saatleriniz dışında zamanlama isteyebilirsiniz. Daha sonra, **Tamam**'a tıklayın.

     ![Çoğaltma ilkesi](./media/hyper-v-vmm-disaster-recovery/replication-policy.png)
     
7. Yeni ilke otomatik olarak VMM bulutuyla ilişkilendirilir. İçinde **Çoğaltma İlkesi**, tıklayın **Tamam**. 


## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

1. **Uygulama çoğaltma** > **Kaynak** seçeneğine tıklayın. 
2. İçinde **kaynak**, VMM sunucusunu ve Bulutu, çoğaltmak istediğiniz Hyper-V ana bilgisayarlarının bulunduğu seçin. Daha sonra, **Tamam**'a tıklayın.
3. İçinde **hedef**, Bulut ve ikincil VMM sunucusundaki doğrulayın.
4. İçinde **sanal makineler**, listeden korumak istediğiniz Vm'leri seçin.


**İşler** > **Site Recovery işleri** bölümünde **Korumayı Etkinleştir** eyleminin ilerleme durumunu izleyebilirsiniz. Sonra **korumayı Sonlandır** işi tamamlandığında, ilk çoğaltma tamamlandıktan ve VM yük devretme için hazırdır.

## <a name="next-steps"></a>Sonraki adımlar

[Olağanüstü durum kurtarma tatbikatı çalıştırma](hyper-v-vmm-test-failover.md)
