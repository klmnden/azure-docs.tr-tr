---
title: "Yeniden koruma nasıl geri Birincil Azure bölgesi için Azure sanal makineleri devredilir gelen | Microsoft Docs"
description: "Bir Azure bölgesinden VM'lerin başka bir yük devretme sonrasında ters yönde makineleri korumak için Azure Site Recovery kullanabilirsiniz. Adımlar, bir yük devretme yeniden önce yeniden koruma yapmak öğrenin."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/28/2017
ms.author: ruturajd
ms.openlocfilehash: 5822ed90f3ab13bdaf1afef62cf32978101c6609
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="reprotect-from-failed-over-azure-region-back-to-primary-region"></a>Bir Azure bölgesine geri birincil bölge üzerinden gelen yeniden koruma başarısız oldu



>[!NOTE]
>
> Azure sanal makineler için Site Recovery çoğaltma şu anda önizlemede değil.


## <a name="overview"></a>Genel Bakış
Olduğunda, [yük devretme](site-recovery-failover.md) diğerine sanal makineleri bir Azure bölgesinden korumasız bir durumda sanal makinelerdir. Birincil bölgesine getirmek istiyorsanız, önce sanal makineleri ve ardından Yük devretme yeniden korumanız gerekir. Nasıl arasında fark yoktur, yük devretme bir yönü veya diğer. Benzer şekilde, sanal makinelerin korumasını etkinleştir işlemleri sonrasında, yeniden koruma post yük devretme veya sonrası yeniden çalışma arasında fark yoktur.
İş akışlarını yeniden koruma açıklayan ve Karışıklığı önlemek için Doğu Asya bölge ile korunan makinelerin birincil site ve makineler kurtarma sitesi Güneydoğu Asya bölge olarak bakın. Yük devretme sırasında Güneydoğu Asya bölgesinde sanal makineler önyükleme yapmaz. Yeniden çalışma önce sanal makinelere gelen Güneydoğu Asya geri Doğu Asya koruyun gerekir. Bu makalede koruyun nasıl adımları açıklanmaktadır.

> [!WARNING]
> Varsa [tamamlandı geçiş](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration), sanal makineyi başka bir kaynak grubuna taşınmış veya Azure sanal makine silinmiş koruyun olamaz ya da sanal makineyi yeniden çalışma.

Yeniden koruma tamamlandıktan sonra korumalı sanal makineleri çoğaltmak, Doğu Asya bölge'ye getirmek için sanal makinelerde yük devretme işlemi başlatabilirsiniz.

POST yorumlarınızı ve sorularınızı bu makalenin veya sonunda [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Ön koşullar
1. Sanal makineleri kaydedilmiş.
2. Hedef site - bu durumda Doğu Asya Azure bölgesini kullanılabilir olması gerekir ve bu bölgede yeni kaynaklar erişimi/oluşturmak mümkün olması gerekir.

## <a name="steps-to-reprotect"></a>Yeniden korumak için adımlar

Varsayılan değerleri kullanarak bir sanal makine yeniden korumak için adımlar şunlardır.

1. İçinde **kasa** > **öğeleri çoğaltılan**üzerinden başarısız sanal makineye sağ tıklayın ve ardından **yeniden koruma**. Ayrıca makine tıklatın ve seçin **yeniden koruma** komutu düğmelerle.

![Sağ tıklayarak koruyun](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

2. Dikey penceresinde dikkat koruma, yönünü **Güneydoğu Asya Doğu Asya için**, zaten seçilidir.

![Dikey koruyun](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotectblade.png)

3. Gözden geçirme **kaynak grubu, ağ, depolama ve kullanılabilirlik kümeleri** bilgi ve Tamam'ı tıklatın. (Yeni) olarak işaretlenmiş kaynakları varsa, yeniden koruma bir parçası olarak oluşturulur.

Bu tetikleyici bir işi ilk hedef sitenin (Bu durumda SEA) en son verilerle belirleyeceği iş koruyun ve tamamlar, çoğaltma işlemi sonra Yük devretme önce farkları Güneydoğu Asya yedekleyin.

### <a name="reprotect-customization"></a>Özelleştirme koruyun
Extract depolama hesabı ya da ağ yeniden koruma sırasında seçmek istiyorsanız, bu nedenle yeniden koruma dikey penceresinde sağlanan Özelleştir seçeneğini kullanarak yapabilirsiniz.

![Seçenek özelleştirme](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

Yeniden koruma sırasında he hedef sanal makine aşağıdaki özelliklerini özelleştirebilirsiniz.

![Dikey penceresini özelleştirme](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Özellik |Notlar  |
|---------|---------|
|Hedef kaynak grubu     | Th sanal makine oluşturulacak hedef kaynak grubu değiştirmek seçebilirsiniz. Parçası olarak yeniden koruma, hedef sanal makine silinir, bu nedenle, yeni bir kaynak grubu altında VM post yük devretme oluşturabilirsiniz seçebilirsiniz         |
|Hedef sanal ağ     | Ağ yeniden koruma sırasında değiştirilemez jb. Ağ değiştirmek için Ağ eşlemesi yineleyin.         |
|Hedef depolama     | Sanal makine post yük devretme oluşturulacak depolama hesabı değiştirebilirsiniz.         |
|Önbellek depolama     | Çoğaltma sırasında kullanılacak bir önbellek depolama hesabı belirtebilirsiniz. Varsayılan değerlerle giderseniz, zaten yoksa, yeni bir önbellek depolama hesabı oluşturulur.         |
|Kullanılabilirlik Kümesi     |Doğu Asya sanal makineyi bir kullanılabilirlik kümesinin parçası ise, Güneydoğu Asya hedef sanal makine için ayarlanmış kullanılabilirlik seçebilirsiniz. Varsayılanları varolan SEA kullanılabilirlik kümesini bulun ve onu kullanmayı deneyin. Özelleştirme sırasında tamamen yeni bir AV kümesi belirtebilirsiniz.         |


### <a name="what-happens-during-reprotect"></a>Yeniden koruma sırasında ne olur?

İlk etkinleştirdikten sonra koruma, tıpkı aşağıdaki Varsayılanları kullanıyorsanız, oluşturulan ürünleridir.
1. Önbellek depolama hesabı Doğu Asya bölgesinde oluşturulan.
2. Hedef depolama hesabı (Güneydoğu Asya VM özgün depolama hesabı) mevcut değilse yeni bir tane oluşturulur. Adı "ile asr" sonekine Doğu Asya sanal makinenin depolama hesabıdır.
3. Hedef AV kümesi yok ve Varsayılanları algılayan varsa, yeni bir AV kümesi oluşturmak için gereken sonra yeniden koruma işi bir parçası olarak oluşturulur. Daha sonra yeniden koruma özelleştirdiyseniz, seçili AV kümesini kullanılacaktır.
4.

Bir yeniden koruma işi tetikleyeceğinden yükleyen durum adımlarının listesi verilmiştir. Hedef tarafı sanal makinenin mevcut durumda budur.

1. Gerekli yapıları yeniden koruma bir parçası olarak oluşturulur. Ardından zaten varsa, bunlar yeniden kullanılır.
2. Çalışıyorsa hedef tarafı (Güneydoğu Asya) sanal makine önce devre dışı bırakılır.
3. Hedef tarafı sanal makinenin disk Azure Site Recovery tarafından bir kapsayıcıya çekirdek blob olarak kopyalanır.
4. Hedef tarafı sanal makine ardından silinir.
5. Çekirdek blob geçerli kaynak tarafı (Doğu Asya) sanal makine tarafından çoğaltmak için kullanılır. Bu, yalnızca farkları çoğaltılır sağlar.
6. Kaynak disk çekirdek blob arasındaki önemli değişiklikler eşitlenir. Bu işlemin tamamlanması biraz zaman alabilir.
7. Yeniden koruma işi tamamlandıktan sonra değişim çoğaltması ilke başına bir kurtarma noktası oluşturan başlar.

> [!NOTE]
> Bir kurtarma planı düzeyde koruyamaz. Konumundaki yalnızca koruyun bir VM gerçekleştiriliyordu.

Yeniden koruma işi başarılı sonra sanal makinenin korumalı bir duruma girer.

## <a name="next-steps"></a>Sonraki adımlar

Sanal makinenin korumalı bir duruma geçtiğini sonra bir yük devretme işlemi başlatabilirsiniz. Yük devretme Doğu Asya Azure bölgesinde sanal makineyi kapatın ve ardından oluşturur ve Güneydoğu Asya bölge sanal makine önyükleme. Bu nedenle uygulama için kısa bir kapalı kalma süresi yok. Uygulamanızı bir kapalı kalma süresi dayanabileceği, bu nedenle, yük devretme için saati seçin. Yük devretme, doğru bir şekilde, bir yük devretmeyi başlatmadan önce geldiği emin olmak için sanal makine önce test önerilir.

-   [Sanal makinenin yük devretme test adımları başlatmak için](site-recovery-test-failover-to-azure.md)

-   [Sanal makinenin yük devretme başlatmak için adımları](site-recovery-failover.md)
