---
title: Bir yeniden çalışma sırasında Hyper-v vm'lerinin olağanüstü durum Azure'dan şirket içine çalıştırma | Microsoft Docs
description: Başarısız öğrenin Azure Site Recovery hizmeti ile azure'a olağanüstü durum kurtarma sırasında bir şirket içi siteye Hyper-V sanal makineleri yedekleyin.
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: 4030b1905f8d5b50ef6be3ffa61eda74d8a27951
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60552425"
---
# <a name="run-a-failback-for-hyper-v-vms"></a>Hyper-V Vm'leri için bir yeniden çalışma

Bu makalede Site Recovery tarafından korunan arka Hyper-V sanal makine başarısız açıklar.

## <a name="prerequisites"></a>Önkoşullar
1. Hakkında ayrıntılar okuduğunuzu olun [geri dönme farklı türde](concepts-types-of-failback.md) ve karşılık gelen uyarılar.
1. Birincil site VMM sunucusunda veya Hyper-V konak sunucusunu Azure'a bağlı olduğundan emin olun.
2. Gerçekleştirilen **işleme** sanal makinede.

## <a name="perform-failback"></a>Yeniden çalışma gerçekleştirin
İkincil konuma yük devretme birincil sonra çoğaltılan sanal makineler Site Recovery tarafından korunmayan ve ikincil konumdaki artık etkin konum davranır. Başarısız olmaya planlı yük devretme ikincil siteden birincil siteye, aşağıdaki gibi çalıştırın, bir kurtarma planında Vm'leri yedekleyin. 
1. Seçin **kurtarma planları** > *recoveryplan_name*. Tıklayın **yük devretme** > **planlı yük devretme**.
2. Üzerinde **planlı yük devretmeyi Onayla** sayfasında, kaynak ve hedef konumları seçin. Yük devretme yönü unutmayın. Birincilden yük devretme durumunda olarak bekler ve tüm sanal makineler, bu yalnızca bilgi içindir ikincil konumdaki olur.
3. Azure'dan devrediyorsanız Ayarları'nda seçin **veri eşitleme**:
    - **(Delta değişikliklerini yalnızca Eşitle) yük devretmeden önce verileri eşitlemek**— kapatmadan bunları eşitlerken bu seçenek sanal makineler için kapalı kalma süresini en aza indirir. Bunu, aşağıdaki adımları gerçekleştirir:
        - 1\. Aşama: Azure'da sanal makine anlık görüntüsünü alır ve şirket içi Hyper-V konağına kopyalar. Makine Azure'da çalıştırmaya devam eder.
        - 2\. Aşama: Yeni değişiklik var. gerçekleşmesi azure'da sanal makineyi kapatır. Delta değişikliklerin son kümesi şirket içi sunucusuna aktarılır ve şirket içi sanal makine başlatıldı.

    - **Yalnızca Yük devretme sırasında veri (tam yükleme) eşitleme**— bu seçenek daha hızlıdır.
        - Bu seçenek daha hızlı çünkü disk çoğunu değişti ve biz sağlama toplamı hesaplama zaman harcamak istemiyorsanız bekliyoruz. Bu diskin bir yükleme gerçekleştirir. Şirket içi sanal makine silindiğinde de yararlıdır.
        - Bir süredir Azure çalıştırıyorsunuz varsa bu seçeneği kullanmanız önerilir (bir ay veya daha fazlası) veya şirket içi sanal makine silindi. Bu seçenek, herhangi bir sağlama toplamı hesaplama gerçekleştirmez.


4. Veri şifreleme bulut için etkinleştirilmişse **şifreleme anahtarı** VMM sunucusunda sağlayıcı yüklemesi sırasında veri şifrelemesi etkin olduğunda verilmiş sertifikayı seçin.
5. Yük devretmeyi başlatın. Yük devretme işleminin ilerleme durumunu **İşler** sekmesinden takip edebilirsiniz.
6. Yük devretmeden önce verileri eşitlemek için seçeneğini seçtiyseniz, ilk veri eşitlemesi tamamlandıktan sonra Azure sanal makineleri kapatmak hazır **işleri** > İş adı >  **Yük devretme tamamlayın**. Bu Azure makineyi kapatmadan, şirket içi sanal makineye en son değişiklikleri aktarır ve şirket içi VM başlatır.
7. Bunu doğrulamak için sanal makine oturum beklendiği gibi kullanılabilir artık.
8. Yürütme Bekleme durumundaki sanal makinedir. Tıklayın **işleme** için yük devretmeyi yürütürsünüz.
9. Yeniden çalışmayı tamamla için tıklatın **ters çoğaltma** birincil sitedeki sanal makine korumayı başlatmak için.


Özgün birincil sitede yeniden çalıştırmak için bu yordamları izleyin. Bu yordamda, bir kurtarma planı için planlanan yük devretme çalıştırma açıklanmaktadır. Alternatif olarak, yük devretme için tek bir sanal makine üzerinde çalışabilir **sanal makineler** sekmesi.


## <a name="failback-to-an-alternate-location-in-hyper-v-environment"></a>Farklı bir konuma Hyper-V ortamına yeniden çalışma
Arasında korumayı dağıttıysanız bir [Hyper-V site ile Azure](site-recovery-hyper-v-site-to-azure.md) özelliği yeniden çalışma için azure'dan şirket alternatif bir konuma gerekir. Bu yeni şirket içi donanım ayarlama istiyorsanız kullanışlıdır. Bunu nasıl yapacağınız aşağıda verilmiştir.

1. Yeni donanım ayarlıyorsanız, Windows Server 2012 R2 ve Hyper-V rolünü sunucuya yükleyin.
2. Özgün sunucuya vardı aynı ada sahip bir sanal ağ anahtarı oluşturun.
3. Seçin **korunan öğeler** -> **koruma grubu** -> \<ProtectionGroupName > -> \<Sourceserver > yeniden, çalışmak istediğiniz seçip **planlı yük devretme**.
4. İçinde **planlı yük devretmeyi Onayla** seçin **Oluştur şirket içi sanal makine henüz yoksa**.
5. Konak adı ** istediğiniz sanal makineyi yerleştirmek yeni Hyper-V konak sunucusunu seçin.
6. Veri eşitlemede veri yük devretmeden önce eşitleme seçeneği seçtiğiniz öneririz. Bunları kapatılıyor olmadan eşitlendiğinde, sanal makineler için kapalı kalma süresi daha en aza indirir. Şunları yapar:

    - 1\. Aşama: Azure'da sanal makine anlık görüntüsünü alır ve şirket içi Hyper-V konağına kopyalar. Makine Azure'da çalıştırmaya devam eder.
    - 2\. Aşama: Yeni değişiklik var. gerçekleşmesi azure'da sanal makineyi kapatır. Son değişiklik kümesini şirket içi sunucusuna aktarılır ve şirket içi sanal makine başlatıldı.
    
7. (Yeniden çalışma) yük devretmeyi başlatmak için onay işaretine tıklayın.
8. İlk eşitleme tamamlandıktan sonra azure'da sanal makineyi kapatmanız hazırsınız **işleri** > \<planlı yük devretme işi >> **yük devretmeyi tamamlamak** . Bu Azure makineyi kapatmadan, şirket içi sanal makineye en son değişiklikleri aktarır ve başlar.
9. Her şeyin beklendiği gibi çalıştığını doğrulamak için şirket içi sanal makineye oturum açabilirsiniz. Ardından **işleme** yük devretmeyi tamamlamak için. İşleme, Azure sanal makine ve disklerine siler ve VM yeniden koruma için hazırlar.
10. Tıklayın **ters çoğaltma** şirket içi sanal makine korumayı başlatmak için.

    > [!NOTE]
    > Veri Eşitleme adımında, ancak yeniden çalışma işini iptal ederseniz, şirket içi VM bozuk durumda olacaktır. Veri eşitlemeyi en son verileri şirket içi veri diskleri için Azure VM diskleri kopyalar ve eşitleme işlemi tamamlanana kadar disk verilerinin tutarlı bir durumda olmayabilir nedeni budur. Veri eşitlemeyi iptal edildikten sonra şirket içi VM önyüklendiyse önyüklenemez. Veri Eşitleme işlemini tamamlamak için yük devretme yeniden Tetikle.


## <a name="why-is-there-no-button-called-failback"></a>Neden yeniden adlandırılan hiçbir bir düğme var mı?
Portalda, yeniden çalışma adında hiçbir açık hareket yoktur. Yeniden çalışma, birincil siteye geri dönün burada bir adımdır. Tanımı gereği, yük devretme için birincil kurtarma sanal makinelerden yeniden geri dönme andır.

Bir yük devretme başlatın, dikey penceresinde, sanal makinelerin olduğu taşınması için yön Azure'dan şirket içine, bir yeniden çalışma ise, yönü hakkında sizi bilgilendirir.

## <a name="why-is-there-only-a-planned-failover-gesture-to-failback"></a>Neden yalnızca bir yeniden çalışma için planlanan yük devretme hareket var mı?
Azure yüksek oranda kullanılabilir bir ortamdır ve sanal makinelerinizin her zaman kullanılabilir. Yeniden çalışma burada iş yükleri şirket içine yeniden çalışmaya başlayabilmeniz için küçük bir kapalı kalma süresi benimsemeye karar planlı bir etkinliktir. Bu, veri kaybı olmadan bekliyor. Bu nedenle yalnızca planlı yük devretme hareket kullanılabilir, Azure sanal makineleri kapatmak, en son değişiklikleri indirir ve veri kaybı olduğundan emin olun.

## <a name="do-i-need-a-process-server-in-azure-to-failback-to-hyper-v"></a>Azure'da Hyper-V'ye yeniden çalışma işlem sunucusu ihtiyacım var?
Hayır, yalnızca VMware sanal makinelerini korurken bir işlem sunucusu gereklidir. Korumak/yeniden çalışma, Hyper-v sanal makinelerin dağıtılması için hiçbir ek bileşenler gereklidir.


## <a name="time-taken-to-failback"></a>Yeniden çalışma için harcanan süre
Veri eşitlemeyi tamamlayın ve sanal makineyi önyüklemek için harcanan süre çeşitli etkenlere bağlıdır. Geçen süre bir anlayış vermek için veri eşitleme sırasında neler açıklanmaktadır.

Veri eşitleme, sanal makinenin disklerinin anlık görüntüsünü alır ve blok blok denetlemeye başlar ve toplamını hesaplar. Başka bir hesaplanan sağlama toplamı, şirket içi aynı blok şirket içi sağlama toplamı ile karşılaştırmak için gönderilir. Sağlama toplamları eşleşmesi durumunda, veri bloğu aktarılmaz. Eşleşmiyorsa, veri bloğu şirket içine aktarılır. Bu aktarım süresini kullanılabilir bant genişliğine bağlıdır. Sağlama birkaç GB başına min hızıdır. 

Veri indirme hızlandırmak için MARS Aracısı indirme paralel hale getirmek için daha fazla iş parçacığı kullanmak için yapılandırabilirsiniz. Başvurmak [burada yer veremeyeceğimiz](https://support.microsoft.com/en-us/help/3056159/how-to-manage-on-premises-to-azure-protection-network-bandwidth-usage) Aracısı'nı indirme iş parçacıklarının değiştirme.


## <a name="next-steps"></a>Sonraki Adımlar

Sonra **işleme**, bunların başlatabilirsiniz *ters çoğaltma*. Bu, sanal makine şirket içinden Azure'a yeniden koruma başlatır. VM Azure'da kapatıldı ve bu nedenle yalnızca değişiklikleri gönderir bu yalnızca değişiklikler çoğaltılır.
