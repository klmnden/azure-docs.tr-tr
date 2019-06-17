---
title: Azure Site Recovery ile olağanüstü durum kurtarma Hyper-V için kapasite planlaması | Microsoft Docs
description: Azure Site Recovery hizmeti ile olağanüstü durum kurtarmayı ayarlarken kapasitesini tahmin etmek için bu makaleyi kullanın.
author: rayne-wiselman
manager: carmonm
services: site-recovery
ms.service: site-recovery
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: raynew
ms.openlocfilehash: eeadfd6a57ff8a26f3f124e2a807fcd66e77b85f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61036784"
---
# <a name="plan-capacity-for-hyper-v-vm-disaster-recovery"></a>Hyper-V VM'LERİNDE olağanüstü durum kurtarma için kapasite planlama 

Geliştirilmiş yeni bir sürümü [Hyper-V'den Azure dağıtım için Azure Site Recovery dağıtım Planlayıcısı](site-recovery-hyper-v-deployment-planner.md) kullanıma sunulmuştur. Bu, eski aracı yerini alır. Yeni aracı dağıtımı planlama için kullanın.
Aracı, aşağıdaki yönergeleri sağlar:

* Diskler, disk boyutu, IOPS, değişim sıklığı ve bazı VM özelliklerine sayısına göre VM uygunluk değerlendirmesi
* Ağ bant genişliği ile RPO değerlendirmesi karşılaştırması gerekir
* Azure altyapı gereksinimleri
* Şirket içi altyapı gereksinimleri
* İlk çoğaltma işlem grubu oluşturma rehberi
* Tahmini Toplam olağanüstü durum kurtarma maliyeti azure'a


Azure Site Recovery Capacity Planner Azure Site Recovery ile Hyper-V Vm'lerini çoğaltma yaptığınızda, kapasitesi gereksinimlerini belirlemenize yardımcı olur.

Kaynak ortamı ve iş yüklerini analiz etmek için Site Recovery Capacity Planner'ı kullanın. Bant genişliği gereksinimlerini tahmin etmenize yardımcı olur, kaynak konumu için ihtiyacınız olan sunucu kaynakları ve kaynakları (örneğin, VM ve depolama) hedef konumda ihtiyacınız vardır.

Aracı iki modda çalıştırabilirsiniz:

* **Hızlı planlama**: VM'ler, diskler, depolama ve değişim hızı, bir ortalama sayısına göre ağ ve sunucu tahminleri sağlar.
* **Ayrıntılı planlama**: Her iş yükü VM düzeyinde ayrıntılarını sağlar. VM uyumluluğu analiz edin ve ağ ve sunucu tahminlerini alın.

## <a name="before-you-start"></a>Başlamadan önce

* VM'ler, VM, disk başına depolama alanı başına disk sayısı dahil olmak üzere, ortamınız hakkında bilgi toplayın.
* Çoğaltılan veriler için günlük değişim (dalgalanma) hızınıza belirleyin. İndirme [Hyper-V kapasite planlama aracı](https://www.microsoft.com/download/details.aspx?id=39057) değişiklik hızını alınamıyor. Bu araç hakkında [daha fazla bilgi edinin](site-recovery-capacity-planning-for-hyper-v-replication.md). Bu araç ortalamalar yakalamak için bir hafta içerisinde çalıştırmanızı öneririz.


## <a name="run-the-quick-planner"></a>Hızlı Planlayıcısını çalıştırın
1. İndir ve Aç [Site Recovery Capacity Planner](https://aka.ms/asr-capacity-planner-excel). Makrolar çalıştırmanız gerekir. İstendiğinde, düzenlemeyi etkinleştir ve içerik seçim yapın.

2. İçinde **bir planner türü seçin** select listesinde **hızlı Planner**.

   ![başlarken](./media/site-recovery-capacity-planner/getting-started.png)

3. Üzerinde **Capacity Planner** çalışma sayfasına gerekli bilgileri girin. Aşağıdaki ekran görüntüsünde kırmızı daire içinde tüm alanları doldurun:

   a. İçinde **senaryonuzu seçin**, seçin **Hyper-V'den azure'a** veya **VMware/Fizikselden azure'a**.

   b. İçinde **ortalama günlük veri değişikliği oranı (%)** , bilgi toplayın kullanarak girin [Hyper-V kapasite planlama aracı](site-recovery-capacity-planning-for-hyper-v-replication.md) veya [Site Recovery dağıtım Planlayıcısı](./site-recovery-deployment-planner.md).

   c. **Sıkıştırma** ayarı, Hyper-V Vm'lerini Azure'a çoğaltırken kullanılmaz. Sıkıştırma, Riverbed gibi bir üçüncü taraf Gereci kullanın.

   d. İçinde **gün cinsinden bekletme**, çoğaltmaları korumak için ne kadar gün olarak belirtin.

   e. İçinde **hangi ilk çoğaltma toplu sanal makineler için saat sayısını tamamlamanız gereken** ve **ilk çoğaltma toplu iş başına sanal makine sayısı**, hesaplamak için kullanılan ayarları girin ilk çoğaltma gereksinimleri. Site Recovery dağıttığınızda, ilk veri kümesinin tamamı yüklenir.

   ![Girişler](./media/site-recovery-capacity-planner/inputs.png)

4. Değerler için kaynak ortamı girdikten sonra görüntülenen çıktının içerir:

   * **(İçinde megabit/sn) değişiklik çoğaltması için gereken bant genişliği**: Değişiklik çoğaltması için ağ bant genişliğini ortalama günlük veri değişikliği hızınıza hesaplanır.
   * **(Megabit/sn), ilk çoğaltma için gereken bant genişliği**: İlk çoğaltma için ağ bant genişliği, girdiğiniz ilk çoğaltma değerlerine hesaplanır.
   * **Depolama (GB) gerekli**: Gereken toplam Azure depolama alanı.
   * **Standart depolama IOPS toplam**: Sayısı 8 K IOPS birim boyutu toplam standart depolama hesaplarına göre hesaplanır. Hızlı Planlayıcıyı tüm kaynak VM disk numarası dayalı olarak hesaplanır ve günlük veri değişim oranı. Ayrıntılı Planlayıcıyı sayı dayalı olarak standart Azure Vm'lerine eşlenen VM'lerin toplam sayısı hesaplanır ve veri değişim oranı bu vm'lerdeki.
   * **Standart depolama hesabı gerekli**: Standart depolama hesapları Vm'leri korumak için gereken toplam sayısı. Bir standart depolama hesabı, standart depolama alanında tüm sanal makinelerde en fazla 20.000 IOPS basılı tutabilirsiniz. Disk başına en fazla 500 IOPS desteklenir.
   * **Gerekli Blob disk sayısını**: Azure depolama üzerinde oluşturulan disklerini sayısı.
   * **Premium hesap gerekiyor sayısı**: Premium depolama hesapları Vm'leri korumak için gereken toplam sayısı. Kaynak VM ile (20. 000 ' büyük) yüksek IOPS, premium depolama hesabı gerekir. Premium depolama hesabı, en fazla 80.000 IOPS barındırabilir.
   * **Premium depolama IOPS toplam**: Sayı toplam premium depolama hesapları 256 K IOPS birimi boyutuna göre hesaplanır. Hızlı Planlayıcıyı tüm kaynak VM disk numarası dayalı olarak hesaplanır ve günlük veri değişim oranı. Ayrıntılı Planlayıcıyı sayı dayalı olarak premium Azure Vm'lere (DS ve GS serisi) eşlenmiş VM'lerin toplam sayısı hesaplanır ve veri değişim oranı bu vm'lerdeki.
   * **Yapılandırma gereken sunucu sayısına göre**: Kaç tane yapılandırma sunucusu dağıtım için gerekli olduğunu gösterir.
   * **Gereken ek işlem sunucularının sayısı**: Ek işlem sunucularının varsayılan olarak yapılandırma sunucusunda çalıştırılan işlem sunucusu yanı sıra gerekli olup olmadığını gösterir.
   * **% 100 kaynak ek depolama alanı**: Kaynak konumda ek depolama alanı gerekli olup olmadığını gösterir.

      ![Çıktı](./media/site-recovery-capacity-planner/output.png)

## <a name="run-the-detailed-planner"></a>Ayrıntılı Planlayıcısını çalıştırın

1. İndir ve Aç [Site Recovery Capacity Planner](https://aka.ms/asr-capacity-planner-excel). Makrolar çalıştırmanız gerekir. İstendiğinde, düzenlemeyi etkinleştir ve içerik seçim yapın.

2. İçinde **bir planner türü seçin**seçin **ayrıntılı Planner** liste kutusundan.

   ![Başlangıç kılavuzu](./media/site-recovery-capacity-planner/getting-started-2.png)

3. Üzerinde **iş yükü nitelik** çalışma sayfasına gerekli bilgileri girin. Tüm işaretli alanları doldurmanız gerekir.

   a. İçinde **işlemci çekirdeği**, kaynak sunucuda toplam çekirdek sayısını belirtin.

   b. İçinde **bellek ayırma (MB) cinsinden**, kaynak sunucunun RAM boyutu belirtin.

   c. İçinde **sayısı, NIC**, kaynak sunucuda ağ bağdaştırıcılarının sayısını belirtin.

   d. İçinde **toplam depolama alanı (GB) cinsinden**, VM depolama için toplam boyutu belirtin. Örneğin, kaynak sunucu ile 500 GB üç disk varsa, toplam depolama boyutu 1500 GB'tır.

   e. İçinde **bağlı disk sayısı**, kaynak sunucunun disk toplam sayısını belirtin.

   f. İçinde **Disk kapasitesine kullanımı (%)** , ortalama kullanım belirtin.

   g. İçinde **günlük veri değişikliği oranı (%)** , günlük veri değişikliği oranı, kaynak sunucunun belirtin.

   h. İçinde **eşleme Azure VM boyutu**, eşlemek istediğiniz Azure VM boyutu girin. Bu el ile gerçekleştirmek istiyorsanız, seçin **işlem Iaas Vm'leri**. El ile bir ayar girin ve ardından **işlem Iaas Vm'leri**, el ile ayarlama üzerine yazılmış olabilir. İşlem işlem otomatik olarak Azure VM boyutu en iyi eşleşmeye tanımlar.

   ![İş yükü nitelik çalışma](./media/site-recovery-capacity-planner/workload-qualification.png)

4. Seçerseniz **işlem Iaas Vm'leri**, işte ne işe yarar:

   * Zorunlu girişleri doğrular.
   * IOPS hesaplar ve azure'a çoğaltma için uygun olan her bir VM için en iyi Azure VM boyutu eşleşen önerir. Azure VM algılanamıyor uygun bir boyut, bir hata görüntülenir. Örneğin, ekli disklerin sayısı, 65 ise, bir Azure sanal makinesi için en yüksek boyutu 64 olduğundan bir hata görüntüler.
   * Bir Azure sanal makinesi için kullanılabilecek bir depolama hesabı önerir.
   * Standart depolama hesapları ve premium depolama hesapları iş yükü için gereken toplam sayısını hesaplar. Azure depolama türü ve kaynak sunucu için kullanılan depolama hesabını görüntülemek için aşağı kaydırın.
   * Tamamlandıktan ve atanan bir VM ve ekli disklerin sayısı için gerekli depolama türü (standart veya premium) göre tablonun geri kalanı sıralar. Azure, sütun gereksinimlerini tüm sanal makineler için **tam VM olduğu?** gösterir **Evet**. Bir sanal makine, Azure'a yedeklenemez bir hata görüntülenir.

Sütunları AA AE çıktısı alınır ve her VM için bilgileri sağlayın.

![Çıkış sütunları AA AE](./media/site-recovery-capacity-planner/workload-qualification-2.png)

### <a name="example"></a>Örnek
Altı VM'ler tablosunda gösterilen değerleri için örnek olarak araç, hesaplar ve en iyi Azure VM eşleşen ve Azure depolama gereksinimlerini atar.

![İş yükü nitelik atamaları](./media/site-recovery-capacity-planner/workload-qualification-3.png)

* Örnek çıktıda aşağıdakilere dikkat edin:

  * İlk sütun, VM'ler, diskler ve değişim sıklığı için bir doğrulama sütundur.
  * İki standart depolama hesapları ve bir premium depolama hesabı, beş VM'ler için gereklidir.
  * Bir veya daha fazla disk 1 TB'den fazla olduğundan VM3 koruma için uygun değil.
  * VM1 ve VM2 ilk standart depolama hesabı kullanabilirsiniz
  * VM4 ikinci bir standart depolama hesabı kullanabilirsiniz.
  * VM5 ve VM6 bir premium depolama hesabı gerekir ve her ikisi de tek bir hesap kullanabilirsiniz.

    > [!NOTE]
    > Standart ve premium depolama IOPS, VM düzeyinde ve disk düzeyinde hesaplanır. Standart bir 500 IOPS disk başına en fazla VM işleyebilir. 500'den büyük bir disk IOPS, premium depolama gerekir. 500'den fazla IOPS için disk olan ancak IOPS toplam VM diskleri için destek standart Azure VM sınırlarda, standart bir VM ile değil DS veya GS serisi Planlayıcıyı seçer. (Azure VM VM boyutunu, disk sayısı, bağdaştırıcılar, CPU ve bellek sayısı limitlerdir.) Eşleme Azure boyut hücresi uygun DS veya GS serisi ile VM el ile güncelleştirmeniz gerekir.


Tüm bilgileri girdikten sonra seçip **Planlayıcısı aracını veri göndermek** Capacity Planner'ı açın. İş yüklerini koruma için uygun olup olmadığını gösterecek şekilde vurgulanmıştır.

### <a name="submit-data-in-capacity-planner"></a>Capacity Planner veri gönderme
1. Açtığınızda **Capacity Planner** çalışma, belirttiğiniz ayarlara göre doldurulur. Sözcük "İş yükü" görünür **Infra girişleri kaynak** giriş olduğunu göstermek için hücre **iş yükü nitelik** çalışma.

2. Değişiklik yapmak istiyorsanız, değiştirmenize gerek **iş yükü nitelik** çalışma. Ardından **Planlayıcısı aracını veri göndermek** yeniden.

   ![Kapasite Planlayıcısı](./media/site-recovery-capacity-planner/capacity-planner.png)

## <a name="next-steps"></a>Sonraki adımlar
[Çalıştırmayı öğrenin](site-recovery-capacity-planning-for-hyper-v-replication.md) kapasite planlama aracı.
