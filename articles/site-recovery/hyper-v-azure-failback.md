---
title: Hyper-v sanal makineleri için bir şirket içi site için bir yeniden çalışma çalıştırın | Microsoft Docs
description: Azure Site Recovery, çoğaltma, yük devretme ve sanal makinelerin ve fiziksel sunucuları kurtarma düzenler. Azure'dan şirket içi veri merkezine hakkında bilgi edinin.
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 02/14/2018
ms.author: rajanaki
ms.openlocfilehash: d344174ffa290b55386918ae19e2f792bb801a8a
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2018
ms.locfileid: "29467124"
---
# <a name="run-a-failback-for-hyper-v-vms"></a>Hyper-V VM'ler için bir yeniden çalışma çalıştırın

Bu makalede Site Recovery tarafından korunan geri Hyper-V sanal makineler başarısız açıklar.

## <a name="prerequisites"></a>Önkoşullar
1. Hakkında ayrıntılar okuduğunuzu olun [geri dönme farklı türde](concepts-types-of-failback.md) ve karşılık gelen uyarılar.
1. Birincil site VMM server veya Hyper-V konak sunucusu için Azure bağlı olduğundan emin olun.
2. Gerçekleştirilen **yürütme** sanal makinede.

## <a name="perform-failback"></a>Yeniden çalışma gerçekleştirme
İkincil konuma yük devretme olarak birincil kopyadan sonrasında çoğaltılmış sanal makinelere Site Recovery tarafından korunmayan ve ikincil konum şimdi etkin konumu olarak çalışıyor. Başarısız olmasına için birincil, ikincil siteden planlanmış bir yük devretme gibi çalıştırma bir kurtarma planında, sanal makineleri yedekleyin. 
1. Seçin **kurtarma planları** > *recoveryplan_name*. Tıklatın **yük devretme** > **planlanan yük devretme**.
2. Üzerinde **planlı yük devretme onaylayın** sayfasında, kaynak ve hedef konumların seçin. Yük devretme yönü unutmayın. Birincil yük devretmeyi çalışılan olarak beklediğiniz ve bu yalnızca bilgi amaçlıdır ve ikincil konum içindeki tüm sanal makineleri demektir.
3. Yeniden çalıştırma işlemini gerçekleştiriyorsanız ayarlarında seçin **veri eşitleme**:
    - **Yük devretme (yalnızca Eşitle delta değişiklikleri) önce verileri eşitlemek**— kapatmadan bunları eşitlediği bu seçeneği sanal makineler için kapalı kalma süresi en aza indirir. Aşağıdaki adımları gerçekleştirir:
        - 1. Aşama: Azure'da sanal makinenin anlık görüntüsünü alır ve şirket içi Hyper-V konağına kopyalar. Makine Azure'da çalışan devam eder.
        - 2. Aşama: yeni değişiklik yok gerçekleşmesi Azure sanal makineyi kapatır. Değişiklikler şirket içi sunucusu ve şirket içi sanal makine için aktarılır delta son kümesini başlatılır.

    - **Verileri (tam yükleme) yalnızca yük devretme sırasında eşitleme**— bu seçenek daha hızlıdır.
        - Bu seçenek daha hızlı çünkü çoğu diskin değişti ve sağlama toplamı hesaplama süre beklemesini istemediğiniz bekliyoruz. Diskin yükleme gerçekleştirir. Şirket içi sanal makine silindiğinde de yararlıdır.
        - Kullanıyorsanız bu seçenek Azure bir süre için çalışır durumda öneririz (ayda bir veya daha fazla) veya şirket içi sanal makine silinmiş olabilir. Bu seçenek, sağlama toplamı hesaplamaları gerçekleştirmez.


4. Veri şifreleme bulut için etkinleştirilmişse **şifreleme anahtarı** VMM sunucusunda Sağlayıcı yükleme sırasında veri şifrelemesi etkin olduğunda verilmiş sertifikayı seçin.
5. Yük devretme başlatın. Yük devretme işleminin ilerleyişini izleyin **işleri** sekmesi.
6. Yük devretme önce verileri eşitleme seçeneği seçtiyseniz, ilk veri eşitlemesi tamamlandıktan ve Azure sanal makineleri kapatmak hazır **işleri** planlı yük devretme iş adı **Tamamlamak yük devretme**. Bu Azure makineyi kapatmadan, şirket içi sanal makineye en son değişiklikleri aktarır ve içi VM başlatır.
7. Bunu doğrulamak için sanal makine oturum beklendiği gibi kullanılabilir şimdi kullanabilirsiniz.
8. Sanal makine yürütme bekleme durumuna kullanılıyor. Tıklatın **tamamlama** yük devretme kaydedilemedi.
9. Şimdi yeniden çalışma öğesini tamamlamak için **ters çoğaltma** birincil sitedeki sanal makine korumaya başlamak için.


Özgün birincil sitede yeniden çalıştırmak için bu yordamları izleyin. Bu yordamda, bir kurtarma planı için planlı bir yük devretmeyi çalıştırma açıklanmaktadır. Alternatif olarak, tek bir sanal makine için yük devretme üzerinde çalışabilir **sanal makineleri** sekmesi.


## <a name="failback-to-an-alternate-location-in-hyper-v-environment"></a>Hyper-V ortamında farklı bir konuma geri dönme
Koruması arasında dağıttıktan sonra bir [Hyper-V sitesi ve Azure](site-recovery-hyper-v-site-to-azure.md) geri dönme içi alternatif bir konuma özelliği azure'dan gerekir. Bu yeni şirket içi donanım ayarlama gerektiğinde kullanışlı olur. Bunun nasıl yapılacağı aşağıda verilmiştir.

1. Yeni donanım ayarlıyorsanız Windows Server 2012 R2 ve Hyper-V rolünü sunucuya yükleyin.
2. Özgün sunucuda olan aynı ada sahip bir sanal ağ anahtarı oluşturun.
3. Seçin **korunan öğeler** -> **koruma grubu**  ->  <ProtectionGroupName>  ->  <VirtualMachineName> geri dönecek ve seçmek istediğiniz **planlanmış Yük devretme**.
4. İçinde **planlı yük devretme onaylayın** seçin **oluşturma şirket içi sanal makine değil mevcut değilse**.
5. Ana bilgisayar adında ** istediğiniz sanal makineyi yerleştirmek yeni Hyper-V konak sunucusu seçin.
6. Veri eşitlemesi seçeneğini belirleyin olan öneririz **yük devretme önce verileri eşitlemek**. Kapatmadan bunları eşitlediği, sanal makineler için kapalı kalma süresi daha en aza indirir. Şunları yapar:

    - 1. Aşama: Azure'da sanal makinenin anlık görüntüsünü alır ve şirket içi Hyper-V konağına kopyalar. Makine Azure'da çalışan devam eder.
    - 2. Aşama: yeni değişiklik yok gerçekleşmesi Azure sanal makineyi kapatır. Değişikliklerin son kümesi, şirket içi sunucusuna aktarılır ve şirket içi sanal makine başlatıldı.
    
7. Yük devretmeyi (yeniden çalışma) başlatmak için onay işaretine tıklayın.
8. İlk eşitleme tamamlandıktan sonra Azure sanal makineyi kapatın, tıklatın hazır **işleri** > <planned failover job> > **yük devretmeyi tamamlamak**. Bu Azure makineyi kapatmadan, şirket içi sanal makineye en son değişiklikleri aktarır ve başlar.
9. Her şeyin beklendiği gibi çalıştığını doğrulamak için şirket içi sanal makine oturum açabilir. Ardından **yürütme** yük devretmeyi tamamlamak için. Yürütme Azure sanal makine ve onun diskleri siler ve yeniden korunacak VM hazırlar.
10. Tıklatın **ters çoğaltma** şirket içi sanal makine korumaya başlamak için.

    > [!NOTE]
    > Veri Eşitleme adımda olsa yeniden çalışma işinin iptal ederseniz, şirket içi VM bozuk durumda olacaktır. Veri eşitlemeyi en son verileri Azure VM diskleri için şirket içi veri diskleri kopyalar ve eşitleme tamamlanana kadar disk verileri tutarlı bir durumda olmayabilir nedeni budur. Veri Eşitleme iptal edildikten sonra şirket içi VM önyüklendiğinde, önyüklenemez. Veri Eşitleme işlemini tamamlamak için yük devretme yeniden Tetikle.


## <a name="why-is-there-no-button-called-failback"></a>Neden geri dönme adlı bir düğme var mı?
Portalda, yeniden çalışma adlı hiçbir açık hareketi yoktur. Yeniden çalışma, birincil siteye geri dönün burada bir adımdır. Yük devretme için birincil kurtarma sanal makinelerden yedeklediğinizde tarafından yeniden çalışma tanımıdır.

Bir yük devretme başlatın, dikey sanal makinelerin olduğu taşınması için yönü Azure'dan şirket içi, bir yeniden çalışma ise, yönü hakkında sizi bilgilendirir.

## <a name="why-is-there-only-a-planned-failover-gesture-to-failback"></a>Neden yalnızca bir yeniden çalışma için planlanan yük devretme hareketi var mı?
Azure yüksek oranda kullanılabilir bir ortamdır ve sanal makinelerinizin her zaman kullanılabilir. Yeniden çalışma burada iş yüklerini şirket içi yeniden çalıştırmayı başlayabilmeniz için kısa bir kapalı kalma süresi benimsemeye karar planlı bir etkinliktir. Bu, veri kaybı bekliyor. Bu nedenle yalnızca bir planlı yük devretme hareketi kullanılabilir ve, Azure sanal makineleri kapatmak, en son değişiklikleri indirir ve veri kaybı olduğundan emin olun.

## <a name="do-i-need-a-process-server-in-azure-to-failback-to-hyper-v"></a>Hyper-v için yeniden çalışma için azure'da bir işlem sunucusu gerekiyor mu?
Hayır, yalnızca VMware sanal makineleri korurken bir işlem sunucusu gereklidir. Herhangi bir ek bileşeni koruma/yeniden çalışma, Hyper-v sanal makinelerin dağıtılması gereklidir.


## <a name="time-taken-to-failback"></a>Yeniden çalışma için harcanan süre
Veri Eşitleme işlemini tamamlamak ve sanal makineyi önyüklemek için geçen süre, çeşitli etmenlere bağlıdır. Geçen süre bir anlayış vermek için veri eşitleme sırasında neler olduğunu açıklamaktadır.

Veri Eşitleme blok blok denetlemeye başlar sanal makinenin disklerinin bir anlık görüntüsünü alır ve kendi sağlama toplamı hesaplar. Başka bir hesaplanan sağlama toplamı aynı blok şirket içi sağlama toplamı ile karşılaştırmak için şirket içi gönderilir. Sağlama eşleşmesi durumunda veri bloğu aktarılmaz. Eşleşmiyorsa, şirket içi veri bloğu aktarılır. Bu aktarım süresini kullanılabilir bant genişliğine bağlıdır. Sağlama toplamı birkaç GB başına min hızıdır. 

Veri yükleme hızlandırmak için MARS Aracısı indirme paralel hale daha fazla iş parçacığı kullanmak için yapılandırabilirsiniz. Başvurmak [burada belge](https://support.microsoft.com/en-us/help/3056159/how-to-manage-on-premises-to-azure-protection-network-bandwidth-usage) aracı yükleme parçacıklarında değiştirme.


## <a name="next-steps"></a>Sonraki Adımlar

Sonra **yürütme**, bunların başlatabilirsiniz *ters çoğaltma*. Bu sanal makine şirket içinden Azure'a geri koruma başlatır. VM Azure'da devre dışı ve bu nedenle yalnızca değişiklikleri gönderir beri bu değişiklikler yalnızca çoğaltır.
