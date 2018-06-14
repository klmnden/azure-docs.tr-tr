---
title: Azure'da çoğaltma kapasite tahmin | Microsoft Docs
description: Azure Site Recovery kullanarak çoğalttığınızda kapasitesini tahmin etmek için bu makaleyi kullanın
services: site-recovery
documentationcenter: ''
author: rayne-wiselman
manager: jwhit
editor: ''
ms.assetid: 0a1cd8eb-a8f7-4228-ab84-9449e0b2887b
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 03/09/2018
ms.author: nisoneji
ms.openlocfilehash: d9c2645be73c4b6e34d194d6b2444a700e3900d2
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
ms.locfileid: "29875914"
---
# <a name="plan-capacity-for-protecting-hyper-v-vms-with-site-recovery"></a>Hyper-V sanal makinelerini Site Recovery ile korunması için kapasite planlaması

Yeni bir Gelişmiş sürümü [Hyper-V Azure dağıtımı için Azure Site kurtarma dağıtım Planlayıcısı](site-recovery-hyper-v-deployment-planner.md) kullanıma sunulmuştur. Bu, eski aracı yerini alır. Yeni aracı, dağıtım planlama için kullanın.
Aracı aşağıdaki yönergeleri sağlar:

* Diskler, disk boyutu, IOPS, karmaşıklık ve birkaç VM özelliklerini sayısına göre VM uygunluk değerlendirme
* Ağ bant genişliği karşı RPO değerlendirmesi gerekir
* Azure altyapı gereksinimleri
* Şirket içi altyapı gereksinimleri
* İlk çoğaltma Kılavuzu toplu işleme
* Toplam olağanüstü durum kurtarma maliyeti Azure tahmini


Azure Site kurtarma kapasite Planlayıcısı Azure Site Recovery ile Hyper-V sanal makineleri çoğaltırken, kapasite gereksinimlerini belirlemenize yardımcı olur.

Kaynak ortamınız ve iş yükleri çözümlemek için site kurtarma kapasite Planlayıcı'ı kullanın. Bant genişliği gereksinimlerini tahmin etmenize yardımcı olur, kaynak konumu için ihtiyacınız olan sunucu kaynakları ve kaynakları (örneğin, Vm'leri ve depolama) hedef konumda gerekir.

Aracı iki modda çalıştırabilirsiniz:

* **Hızlı planlama**: ortalama bir VM'ler, diskleri, depolama ve değişim oranı sayısına göre ağ ve sunucu tahminler sunar.
* **Planlama ayrıntılı**: her iş yükünün VM düzeyinde ayrıntıları sağlar. VM uyumluluk çözümlemek ve ağ ve sunucu tahminleri alın.

## <a name="before-you-start"></a>Başlamadan önce

* VM'ler, VM, disk başına depolama başına diskler dahil olmak üzere ortamınız hakkında bilgi toplayın.
* Çoğaltılan veriler için günlük değişim (dalgalanma) oranı tanımlayın. Karşıdan [Hyper-V kapasite planlama aracı](https://www.microsoft.com/download/details.aspx?id=39057) değişim oranı alınamıyor. [Daha fazla bilgi edinin](site-recovery-capacity-planning-for-hyper-v-replication.md) bu araç hakkında. Bu Aracı'nı ortalamalar yakalamak için bir hafta içinde çalıştırmanızı öneririz.


## <a name="run-the-quick-planner"></a>Hızlı Planlayıcısını çalıştırın
1. Karşıdan yükleyip açmak [Site kurtarma kapasite Planlayıcısı](http://aka.ms/asr-capacity-planner-excel). Makroları çalıştırmanız gerekir. İstendiğinde, düzenlemeyi etkinleştir ve içerik için seçimlerinizi yapın.

2. İçinde **Planlayıcısı türünü seçin** liste kutusunda seçin **hızlı Planlayıcısı**.

   ![başlarken](./media/site-recovery-capacity-planner/getting-started.png)

3. Üzerinde **kapasite Planlayıcısı** çalışma sayfası, gerekli bilgileri girin. Aşağıdaki ekran görüntüsü kırmızı daire içinde tüm alanları doldurun:

   a. İçinde **senaryonuz seçin**, seçin **Hyper-V Azure** veya **VMware/fiziksel Azure**.

   b. İçinde **ortalama günlük veri değişikliği oranı (%)**, toplamak kullanarak bilgi girin [Hyper-V kapasite planlama aracı](site-recovery-capacity-planning-for-hyper-v-replication.md) veya [Site Recovery dağıtımı Planlayıcısı](./site-recovery-deployment-planner.md).

   c. **Sıkıştırma** ayarı Hyper-V Vm'lerini Azure'a çoğaltırken kullanılan değil. Sıkıştırma için Riverbed gibi bir üçüncü taraf uygulaması kullanın.

   d. İçinde **gün bekletme**, çoğaltmaları korumak için ne kadar gün olarak belirtin.

   e. İçinde **sanal makinelerin toplu işlemi için hangi ilk çoğaltma saat sayısını tamamlamanız gereken** ve **ilk çoğaltma toplu iş başına sanal makine sayısını**, hesaplamak için kullanılan ayarlarını girin ilk çoğaltma gereksinimleri. Site Recovery dağıtıldığında, ilk veri kümesinin tamamını yüklenir.

   ![Girişler](./media/site-recovery-capacity-planner/inputs.png)

4. Kaynak ortamı için değerleri girdikten sonra görüntülenen çıktının içerir:

   * **Delta Çoğaltmada (saniye başına megabit) için gereken bant genişliği**: değişim çoğaltması için ağ bant genişliği üzerinde ortalama günlük veri değişikliği hızı hesaplanır.
   * **(İçinde megabit/sn) ilk çoğaltma için gereken bant genişliği**: ilk çoğaltma için ağ bant genişliği, girdiğiniz ilk çoğaltma değerlerine hesaplanır.
   * **Depolama (GB) gerekli**: gerekli toplam Azure depolama.
   * **Toplam IOPS standart depolama**: sayı, 8 K IOPS birim boyutu toplam standart depolama hesaplarında dayanarak hesaplanır. Hızlı Planlayıcısı için günlük veri hızını değiştirmek ve tüm kaynak VM disklerde numarası dayalı olarak hesaplanır. Ayrıntılı Planlayıcısı numarası dayanarak standart Azure VM'ler için eşlenen VM toplam sayısını hesaplanır ve verileri bu VM'lerin oranına değiştirin.
   * **Gerekli standart depolama hesabı sayısı**: standart depolama hesabı Vm'leri korumak için gerekli toplam sayısı. Standart depolama hesabı, standart depolama alanında tüm VM'ler üzerindeki 20000 IOPS basılı tutabilirsiniz. Disk başına en fazla 500 IOPS desteklenir.
   * **Gerekli Blob disk sayısını**: Azure depolama alanında oluşturulan disklerini sayısı.
   * **Gereken premium hesaplar sayısı**: premium depolama hesapları Vm'leri korumak için gerekli toplam sayısı. Bir kaynak VM yüksek IOPS (20. 000 ' büyük) ile bir premium depolama hesabı gerekiyor. Premium depolama hesabı kadar 80.000 IOPS basılı tutabilirsiniz.
   * **Toplam IOPS Premium depolama**: sayı, toplam premium depolama hesaplarında 256 K IOPS birim boyutu dayanarak hesaplanır. Hızlı Planlayıcısı için günlük veri hızını değiştirmek ve tüm kaynak VM disklerde numarası dayalı olarak hesaplanır. Ayrıntılı Planlayıcısı numarası dayanarak premium Azure Vm'lerine (DS ve GS serisi) eşlenen VM toplam sayısını hesaplanır ve verileri bu VM'lerin oranına değiştirin.
   * **Yapılandırma gerekli sunucu sayısını**: kaç yapılandırma sunucusu dağıtımı için gerekli olduğunu gösterir.
   * **Ek işlem gerekli sunucu sayısını**: ek işlem sunucuları yapılandırma sunucusunda varsayılan olarak çalıştığı işlem sunucusuna ek olarak gerekip gerekmediğini gösterir.
   * **% 100 ek depolama kaynağı**: ek depolama alanı kaynak konumda gerekli olup olmadığını gösterir.

      ![Çıktı](./media/site-recovery-capacity-planner/output.png)

## <a name="run-the-detailed-planner"></a>Ayrıntılı Planlayıcısını çalıştırın

1. Karşıdan yükleyip açmak [Site kurtarma kapasite Planlayıcısı](http://aka.ms/asr-capacity-planner-excel). Makroları çalıştırmanız gerekir. İstendiğinde, düzenlemeyi etkinleştir ve içerik için seçimlerinizi yapın.

2. İçinde **Planlayıcısı türünü seçin**seçin **ayrıntılı Planlayıcısı** liste kutusundan.

   ![Başlangıç kılavuzu](./media/site-recovery-capacity-planner/getting-started-2.png)

3. Üzerinde **iş yükü niteliğe** çalışma sayfası, gerekli bilgileri girin. İşaretlenen tüm alanları doldurmanız gerekir.

   a. İçinde **işlemci çekirdeği**, bir kaynak sunucuda çekirdek toplam sayısını belirtin.

   b. İçinde **bellek ayırma (MB) cinsinden**, bir kaynak sunucuda RAM boyutunu belirtin.

   c. İçinde **numarası, NIC**, kaynak sunucuda, ağ bağdaştırıcılarının sayısını belirtin.

   d. İçinde **toplam depolama alanı (GB)**, VM depolama toplam boyutu belirtin. Örneğin, kaynak sunucuda üç disk 500 GB ise, toplam depolama boyutu 1500 GB'tır.

   e. İçinde **bağlı disk sayısı**, kaynak sunucunun disklerini toplam sayısını belirtin.

   f. İçinde **Disk kapasitesi kullanımı (%)**, ortalama kullanım belirtin.

   g. İçinde **günlük veri değişikliği oranı (%)**, günlük veri değişikliği oranı kaynak sunucunun belirtin.

   h. İçinde **eşleme Azure VM boyutu**, eşlemek istediğiniz Azure VM boyutu girin. Bunu el ile yapmak istemiyorsanız, seçin **işlem Iaas Vm'leri**. El ile ayarlama girin ve ardından **işlem Iaas Vm'leri**, el ile ayarlama üzerine yazılmış olabilir. İşlem işlemi otomatik olarak Azure VM boyutu en iyi eşleşmeyi tanımlar.

   ![İş yükü niteliğe çalışma sayfası](./media/site-recovery-capacity-planner/workload-qualification.png)

4. Seçerseniz **işlem Iaas Vm'leri**, işte bunu yapar:

   * Zorunlu giriş doğrular.
   * IOPS hesaplar ve Azure'a çoğaltma için uygun olan her bir VM için en iyi Azure VM boyutu eşleşme önerir. Azure VM algılanamıyor uygun bir boyut, bir hata görüntülenir. Örneğin, bağlı disk sayısı 65 ise, bir Azure VM için en yüksek boyutu 64 olduğundan bir hata görüntüler.
   * Bir Azure VM için kullanılabilir bir depolama hesabı önerir.
   * Standart depolama hesapları ve premium depolama hesapları iş yükü için gereken toplam sayısını hesaplar. Azure depolama türünü ve kaynak sunucu için kullanılabilir depolama hesabı görüntülemek için aşağı kaydırın.
   * Tamamlar ve atanan bir VM ve bağlı disk sayısı için gerekli depolama türü (standart veya premium) temel tablonun geri kalanı sıralar. Azure, sütun gereksinimlerini tüm VM'ler için **tam VM olduğu?** gösterir **Evet**. Azure VM yedeklenemiyor, bir hata görüntülenir.

Sütunları AA AE çıktısı alınır ve her VM için bilgiler sağlar.

![Çıkış sütunları AA AE](./media/site-recovery-capacity-planner/workload-qualification-2.png)

### <a name="example"></a>Örnek
Örneğin, tabloda gösterilen değerleri olan altı VM'ler için aracı hesaplar ve en iyi Azure VM eşleşen ve Azure depolama gereksinimleri atar.

![İş yükü niteliğe atamaları](./media/site-recovery-capacity-planner/workload-qualification-3.png)

* Örnek çıkış şunları unutmayın:

  * İlk sütun VM'ler, diskler ve karmaşıklık için bir doğrulama sütundur.
  * Beş VM'ler için iki standart depolama hesapları ve bir premium depolama hesabı gereklidir.
  * Bir veya daha fazla disk birden fazla 1 TB olduğundan VM3 koruma için uygun değil.
  * VM1 ve VM2 ilk standart depolama hesabı kullanabilirsiniz
  * VM4 ikinci standart depolama hesabı kullanabilirsiniz.
  * Premium depolama hesabı VM5 ve VM6 gerekir ve her ikisi de tek bir hesap kullanabilirsiniz.

    > [!NOTE]
    > Standart ve premium depolama IOPS VM düzeyinde ve disk düzeyinde hesaplanır. Standart bir 500 IOPS disk başına en fazla VM işleyebilir. Bir disk için IOPS 500'den büyükse, premium depolama gerekir. Bir disk için IOPS 500'den fazla ancak IOPS toplam VM diskleri için destek standart Azure VM sınırlarda, Planlayıcısı standart VM ve değil DS veya GS serisi seçer. (Azure VM VM boyutunu, disk, bağdaştırıcılar, CPU ve bellek sayısı sayısı kısıtlamalardır.) Eşleme Azure boyutu hücre uygun DS veya GS serisi ile VM el ile güncelleştirmeniz gerekir.


Tüm bilgileri girdikten sonra seçin **planlayıcısı aracı veri göndermek** kapasite Planlayıcısı açın. İş yükleri için koruma uygun olup olmadığını göstermek için vurgulanır.

### <a name="submit-data-in-capacity-planner"></a>Kapasite Planlayıcısı verileri gönderme
1. Açtığınızda **kapasite Planlayıcısı** çalışma sayfasında, belirttiğiniz ayarlara göre doldurulur. "İş yükü" görünür word **Infra girişleri kaynak** giriş olduğunu göstermek için hücre **iş yükü niteliğe** çalışma sayfası.

2. Değişiklik yapmak istiyorsanız, değiştirmenize gerek **iş yükü niteliğe** çalışma sayfası. Ardından **planlayıcısı aracı veri göndermek** yeniden.

   ![Capacity Planner](./media/site-recovery-capacity-planner/capacity-planner.png)

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamasını nasıl çalıştıracağınızı öğrenin](site-recovery-capacity-planning-for-hyper-v-replication.md) kapasite planlama aracı.
