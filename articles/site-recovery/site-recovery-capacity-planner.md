---
title: "Azure'da çoğaltma kapasite tahmin | Microsoft Docs"
description: "Azure Site Recovery ile çoğaltırken kapasitesini tahmin etmek için bu makaleyi kullanın"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 0a1cd8eb-a8f7-4228-ab84-9449e0b2887b
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/28/2017
ms.author: nisoneji
ms.openlocfilehash: f504888aac9e8d97e974fb5bec0a12a8ede39c76
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
Yeni gelişmiş sürümü [Hyper-V Azure için Azure Site Recovery dağıtımı Planlayıcısı](site-recovery-hyper-v-deployment-planner.md) kullanıma sunulmuştur ve eski aracı değiştirme. Yeni aracı, dağıtım planlama için kullanın. Aracı kılavuz sağlar:
* Diskler, disk boyutu, IOPS, karmaşıklık ve birkaç VM özelliklerini sayısına göre VM uygunluk değerlendirmesi.
* Ağ bant genişliği karşı RPO değerlendirmesi gerekir.
* Azure altyapı gereksinimleri.
* Şirket içi altyapı gereksinimleri.
* İlk çoğaltma toplu işlem kılavuzu.
* Azure toplam DR maliyetini tahmin.

# <a name="plan-capacity-for-protecting-hyper-v-vms-with-site-recovery"></a>Hyper-V sanal makinelerini Site Recovery ile korunması için kapasite planlaması

Azure Site kurtarma kapasite planlayıcısı aracı, Azure Site Recovery ile Hyper-V sanal makineleri çoğaltırken, kapasite gereksinimlerini anlamaya yardımcı olur.

Kaynak ortamınız ve iş yükleri analiz, bant genişliği gereksinimlerini ve sunucu kaynakları için kaynak konumu gerekir ve hedef konumda gereken kaynaklar (sanal makineler ve depolama vb.) tahmin etmek için Site kurtarma kapasite Planlayıcısı kullanın .

Aracı modları birkaç içinde çalıştırabilirsiniz:

* **Hızlı planlama**: aracı ağ ve sunucu tahminleri almak için bu modda ortalama bir VM'ler, diskleri, depolama ve değişim oranı sayısına göre çalıştır.
* **Planlama ayrıntılı**: Bu modda aracını çalıştırın ve her iş yükü VM düzeyinde ayrıntılarını sağlayın. VM uyumluluk çözümlemek ve ağ ve sunucu tahminleri alın.

## <a name="before-you-start"></a>Başlamadan önce


1. VM'ler, VM, disk başına depolama başına diskler dahil olmak üzere ortamınız hakkında bilgi toplayın.
2. Çoğaltılan veriler için günlük değişim (dalgalanma) oranı tanımlayın. Bunu yapmak için indirme [Hyper-V kapasite planlama aracı](https://www.microsoft.com/download/details.aspx?id=39057) değişim oranı alınamıyor. [Daha fazla bilgi edinin](site-recovery-capacity-planning-for-hyper-v-replication.md) bu araç hakkında. Bu aracı ortalamalar yakalamak için bir hafta içinde çalıştırmak öneririz.
   

## <a name="run-the-quick-planner"></a>Hızlı Planlayıcısını çalıştırın
1. Karşıdan yükleyip açmak [Azure Site kurtarma kapasite Planlayıcısı](http://aka.ms/asr-capacity-planner-excel) aracı. Makrolar, düzenleme etkinleştirmek ve istendiğinde içeriği etkinleştirmek için bu nedenle select çalıştırmanız gerekir.
2. İçinde **Planlayıcısı türünü seçin** seçin **hızlı Planlayıcısı** liste kutusundan.

   ![Başlarken](./media/site-recovery-capacity-planner/getting-started.png)
3. İçinde **kapasite Planlayıcısı** çalışma sayfası, gerekli bilgileri girin. Aşağıdaki ekran görüntüsünde kırmızı daire içinde tüm alanları doldurmanız gerekir.

   * İçinde **senaryonuz seçin**, seçin **Hyper-V Azure** veya **VMware/fiziksel Azure**.
   * İçinde **ortalama günlük veri değişikliği oranı (%)**, bilgileri toplamak kullanarak put [Hyper-V kapasite planlama aracı](site-recovery-capacity-planning-for-hyper-v-replication.md) veya [Azure Site Recovery dağıtımı Planlayıcısı](./site-recovery-deployment-planner.md).  
   * **Sıkıştırma** ayarı Hyper-V Vm'lerini Azure'a çoğaltırken kullanılan değil. Sıkıştırma için Riverbed gibi bir üçüncü taraf uygulaması kullanın.
   * İçinde **bekletme girişleri**, ne kadar süreyle çoğaltmalar, saat tutulacağını belirtin.
   * İçinde **sanal makinelerin toplu işlemi için hangi ilk çoğaltma saat sayısını tamamlamanız gereken** ve **ilk çoğaltma toplu iş başına sanal makine sayısını**, ilk çoğaltma gereksinimlerini hesaplamak için kullanılan ayarları girin.  Site Recovery dağıtıldığında, ilk veri kümesinin tamamını yüklenmelidir.

   ![Girişler](./media/site-recovery-capacity-planner/inputs.png)
4. Kaynak ortamı için değerler yerleştirdiğiniz sonra görüntülenen çıktının içerir:

   * **Delta çoğaltma için gereken bant genişliği** (MB/sn). Değişim çoğaltması için ağ bant genişliği üzerinde ortalama günlük veri değişikliği hızı hesaplanır.
   * **İlk çoğaltma için gereken bant genişliği** (MB/sn). İlk çoğaltma için ağ bant genişliği içine ilk çoğaltma değerleri hesaplanır.
   * **Depolama (GB) gerekli** gerekli toplam Azure depolama.
   * **Standart depolama hesaplarında IOPS toplam** 8 K IOPS birim boyutu toplam standart depolama hesaplarında temel alınarak hesaplanır.  Hızlı Planlayıcısı için günlük veri hızını değiştirmek ve tüm kaynak VM'ler disklerde numarası dayalı olarak hesaplanır. Ayrıntılı Planlayıcısı için sayı dayanarak standart Azure VM'ler için eşlenen VM'lerin toplam sayısını hesaplanır ve bu VM'lerin oranına veri değiştirin.
   * **Standart depolama hesabı sayısı** Vm'leri korumak için gerekli standart depolama hesaplarının toplam sayısına sağlar. Standart depolama hesabı bir standart depolama birimindeki tüm VM'ler arasında en fazla 20000 IOPS tutabilir ve disk başına en fazla 500 IOPS desteklenir.
   * **Gerekli blob disk sayısını** Azure depolama alanında oluşturulacak disk sayısını verir.
   * **Premium depolama hesapları gerekli sayısı** premium depolama hesapları Vm'leri korumak için gerekli toplam sayısını sağlar. Bir kaynak VM yüksek IOPS (20000 büyük) ile bir premium depolama hesabı gerekiyor. Premium depolama hesabı kadar 80000 IOPS basılı tutabilirsiniz.
   * **Premium depolama IOPS toplam** 256 K IOPS birim boyutu toplam premium depolama hesaplarında temel alınarak hesaplanır.  Hızlı Planlayıcısı için günlük veri hızını değiştirmek ve tüm kaynak VM'ler disklerde numarası dayalı olarak hesaplanır. Ayrıntılı Planlayıcısı numarası dayanarak premium Azure VM (DS ve GS serisi) eşlenen VM toplam sayısını hesaplanır ve verileri bu VM'lerin hızını değiştirin.
   * **Gerekli yapılandırma sunucularına sayısı** kaç yapılandırma sunucusu dağıtımı için gerekli olduğunu gösterir.
   * **Gerekli ek işlem sunucu sayısını** ek işlem sunucuları yapılandırma sunucusunda varsayılan olarak çalıştığı işlem sunucusuna ek olarak gerekip gerekmediğini gösterir.
   * **% 100 ek depolama kaynağı** ek depolama alanı kaynak konumda gerekli olup olmadığını gösterir.

   ![Çıktı](./media/site-recovery-capacity-planner/output.png)

## <a name="run-the-detailed-planner"></a>Ayrıntılı Planlayıcısını çalıştırın

1. Karşıdan yükleyip açmak [Azure Site kurtarma kapasite Planlayıcısı](http://aka.ms/asr-capacity-planner-excel) aracı. Makrolar, düzenleme etkinleştirmek ve istendiğinde içeriği etkinleştirmek için bu nedenle select çalıştırmanız gerekir.
2. İçinde **Planlayıcısı türünü seçin**seçin **ayrıntılı Planlayıcısı** liste kutusundan.

   ![Başlarken](./media/site-recovery-capacity-planner/getting-started-2.png)
3. İçinde **iş yükü niteliğe** çalışma sayfası, gerekli bilgileri girin. İşaretlenen tüm alanları doldurmanız gerekir.

   * İçinde **işlemci çekirdeği**, bir kaynak sunucuda çekirdek toplam sayısını belirtin.
   * İçinde **MB bellek ayırma**, bir kaynak sunucuda RAM boyutunu belirtin.
   * **Numarası, NIC**, kaynak sunucuda, ağ bağdaştırıcılarının sayısını belirtin.
   * İçinde **toplam depolama (GB) cinsinden**, VM depolama toplam boyutu belirtin. Örneğin, kaynak sunucuda 3 disklerle 500 GB ise, toplam depolama boyutu 1500 GB olur.
   * İçinde **bağlı disk sayısı**, kaynak sunucunun disklerini toplam sayısını belirtin.
   * İçinde **Disk kapasitesi kullanımı**, ortalama kullanım belirtin.
   * İçinde **günlük oranı (%) değiştirme**, günlük veri değişikliği oranı kaynak sunucunun belirtin.
   * İçinde **eşleme Azure boyutu**, eşlemek istediğiniz Azure VM boyutu girin. Bunu el ile yapmak istemiyorsanız tıklayın **işlem Iaas Vm'leri**. El ile ayarlama giriş ve işlem Iaas Vm'leri ardından işlem işlemi otomatik olarak Azure VM boyutu en iyi eşleşmeyi tanımladığından el ile ayarlama üzerine.

   ![İş yükü nitelik](./media/site-recovery-capacity-planner/workload-qualification.png)
4. Tıklatırsanız **işlem Iaas Vm'leri** İşte ne yapar:

   * Zorunlu giriş doğrular.
   * IOPS hesaplar ve en iyi Azure VM aize Azure'a çoğaltma için uygun olan her sanal makineleri eşleşen önerir. Azure VM uygun boyutta bir hata veren algılanamaz ise. Örneğin, disk sayısını içinde 65 bağlıysa, yüksek Azure VM boyutu 64 olduğundan hata verilir.
   * Bir Azure VM için kullanılabilir bir depolama hesabı önerir.
   * Standart depolama hesapları ve premium depolama hesapları iş yükü için gereken toplam sayısını hesaplar. Görünüm Azure depolama türünü ve kaynak sunucu için kullanılabilir depolama hesabı gidin.
   * Tamamlar ve bir VM ve bağlı disk sayısı için atanan gerekli depolama türü (standart veya premium) temel tablonun geri kalanı sıralar. Azure, sütun gereksinimlerini tüm VM'ler için **tam VM olduğu?** gösterir **Evet**. Azure VM yedeklenemiyor durumunda bir hata gösterilir.

Sütunları AA AE çıktısı alınır ve her VM için bilgiler sağlar.

![İş yükü nitelik](./media/site-recovery-capacity-planner/workload-qualification-2.png)

### <a name="example"></a>Örnek
Örneğin, tabloda gösterilen değerleri olan altı VM'ler için aracı hesaplar ve en iyi Azure VM eşleşen ve Azure depolama gereksinimleri atar.

![İş yükü nitelik](./media/site-recovery-capacity-planner/workload-qualification-3.png)

* Örnek çıkış şunları unutmayın:

  * İlk sütun VM'ler, diskler ve karmaşıklık için bir doğrulama sütundur.
  * Beş VM'ler için iki standart depolama hesapları ve bir premium depolama hesabı gereklidir.
  * Bir veya daha fazla disk birden fazla 1 TB olduğundan VM3 koruma için uygun değil.
  * VM1 ve VM2 ilk standart depolama hesabı kullanabilirsiniz
  * VM4 ikinci standart depolama hesabı kullanabilirsiniz.
  * Premium depolama hesabı VM5 ve VM6 gerekir ve her ikisi de tek bir hesap kullanabilirsiniz.

    > [!NOTE]
    > Standart ve premium depolama IOPS VM düzeyinde ve disk düzeyinde hesaplanır. Standart bir sanal makine disk başına en fazla 500 IOPS işleyebilir. Bir disk için IOPS 500'den büyükse, premium depolama gerekir. Ancak, bir disk için IOPS 500'den fazla olan ancak IOPS toplam VM diskleri için destek standart Azure VM sınırları içinde (VM boyutunu, disk, bağdaştırıcılar, CPU, bellek sayısı sayısı), ardından Planlayıcısı standart bir VM ve değil DS veya GS serisi seçer. Eşleme Azure boyutu hücre uygun DS veya GS serisi ile VM el ile güncelleştirmeniz gerekir.


Tüm Ayrıntılar hazır olduktan sonra tıklatın **planlayıcısı aracı veri göndermek** açmak için **kapasite Planlayıcısı** iş yükleri vurgulanır, bunlar veya koruma için uygun olup olmadığını göstermek için.

### <a name="submit-data-in-the-capacity-planner"></a>Kapasite Planlayıcısı verileri gönderme
1. Açtığınızda **kapasite Planlayıcısı** doldurulmuş çalışma göre belirttiğiniz ayarlar. 'İş yükü' görünür word **Infra girişleri kaynak** giriş olduğunu göstermek için hücre **iş yükü niteliğe** çalışma sayfası.
2. Değişiklik yapmak istiyorsanız, değiştirmenize gerek **iş yükü niteliğe** çalışma sayfası ve tıklatın **planlayıcısı aracı veri göndermek** yeniden.  

   ![Kapasite Planlayıcısı](./media/site-recovery-capacity-planner/capacity-planner.png)

## <a name="next-steps"></a>Sonraki adımlar

[Uygulamasını nasıl çalıştıracağınızı öğrenin](site-recovery-capacity-planning-for-hyper-v-replication.md) kapasite planlayıcısı aracı.
