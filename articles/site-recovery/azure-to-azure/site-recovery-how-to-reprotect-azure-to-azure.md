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
ms.date: 08/11/2017
ms.author: ruturajd
ms.openlocfilehash: 0004323fee44c1433cdd3c39fc1fa7dad965dbf7
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2017
---
# <a name="reprotect-azure-vms-back-to-the-primary-region"></a>Birincil bölgesine Azure Vm'leri koruyun



>[!NOTE]
>
> Azure sanal makineleri (VM'ler) için Site Recovery çoğaltma şu anda önizlemede değil.


Olduğunda, [yük devri](../site-recovery-failover.md) diğerine sanal makineleri bir Azure bölgesinden devredilen sanal makineleri olan korumasız bir durumda. Birincil bölgesine getirmek istiyorsanız, önce sanal makineleri çoğaltma işlemi başlatma ve baştan başarısız gerekir. Bir yönde veya diğer yük devretme arasında fark yoktur. Benzer şekilde, VM'nin çoğaltma etkinleştirdikten sonra yükü yük devretme sonrasında veya sonrası yeniden çalışma arasında fark yoktur.

Yükü işlemi, birincil sitenin korumalı VM'lerin Doğu Asya bölge, ve kurtarma sitesini Güneydoğu Asya olduğunu varsayıyoruz örnek olarak açıklamak için. Yük devretme sırasında Güneydoğu Asya bölgeye VM'ler devredin. Geri dönecek önce Vm'lerini Güneydoğu Asya gelen geri Doğu Asya çoğaltma gerekir. Bu makalede yükü için gereken adımlar açıklanır.

> [!WARNING]
> Geçişi tamamladıktan ve sanal makineyi başka bir kaynak grubuna taşındı (veya Azure sanal makine silindi varsa) geri kapatamazsınız.

Yükü tamamlandıktan ve sanal makineleri çoğaltmak sonra VM'ler Doğu Asya bölgeye getirmek için bir yük devretme işlemi başlatabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
1. VM kaydedilmiş.
2. Hedef site (Doğu Asya) kullanılabilir olması gerekir ve bu bölgede yeni kaynaklar erişimi/oluşturmak mümkün olması gerekir.

## <a name="reprotect"></a>Koruyun

Varsayılan ayarları kullanarak bir VM yeniden korumak için aşağıdaki adımları izleyin.

1. İçinde **kasa** > **öğeleri çoğaltılan**devredilen VM'ye sağ tıklayın ve ardından **yeniden koruma**. Ayrıca makine tıklatın ve seçin **yeniden koruma** komutu düğmelerle.

  ![Yükü](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

2. Doğrulayın çoğaltma yönünü **Güneydoğu Asya Doğu Asya için**, seçilir.

  ![Koruyun](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotectblade.png)

3. Gözden geçirme **kaynak grubu, ağ, depolama ve kullanılabilirlik kümeleri** bilgileri ve tıklatın **Tamam**. (Yeni) olarak işaretlenmiş kaynakları varsa, bunlar yükü sırasında oluşturulur.

Hedef sitenin en son verilerle çekirdeğini oluşturur ve bu işlemi tamamlandıktan sonra başarısız önce farkları geri Güneydoğu Asya çoğaltan bir yükü işi tetikler.

### <a name="reprotect-customization"></a>Özelleştirme koruyun
Extract depolama hesabı ya da ağ yeniden koruma sırasında seçmek istiyorsanız, bunu yükü sayfada sağlanan Özelleştir seçeneğini kullanarak yapabilirsiniz.

![Seçenek özelleştirme](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

Aşağıdaki özellikler hedef sanal makinenin yükü sırasında özelleştirebilirsiniz.

![Özelleştirme](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Özellik |Notlar  |
|---------|---------|
|Hedef kaynak grubu     | Th sanal makine oluşturulacak hedef kaynak grubu değiştirmek seçebilirsiniz. Parçası olarak yeniden koruma, hedef sanal makine silinir, bu nedenle, yeni bir kaynak grubu altında VM post yük devretme oluşturabilirsiniz seçebilirsiniz         |
|Hedef sanal ağ     | Ağ Yükü sırasında değiştirilemez. Ağ değiştirmek için Ağ eşlemesi yineleyin.         |
|Hedef depolama     | Sanal makine post yük devretme oluşturulacak depolama hesabı değiştirebilirsiniz.         |
|Önbellek depolama     | Çoğaltma sırasında kullanılacak bir önbellek depolama hesabı belirtebilirsiniz. Varsayılan değerlerle giderseniz, zaten yoksa, yeni bir önbellek depolama hesabı oluşturulur.         |
|Kullanılabilirlik Kümesi     |Doğu Asya sanal makineyi bir kullanılabilirlik kümesinin parçası ise, Güneydoğu Asya hedef sanal makine için ayarlanmış kullanılabilirlik seçebilirsiniz. Varsayılanları varolan SEA kullanılabilirlik kümesini bulun ve onu kullanmayı deneyin. Özelleştirme sırasında tamamen yeni bir AV kümesi belirtebilirsiniz.         |


### <a name="what-happens-during-reprotect"></a>Yeniden koruma sırasında ne olur?

İlk etkinleştirdikten sonra koruma, tıpkı aşağıdaki Varsayılanları kullanıyorsanız, oluşturulan ürünleridir.
1. Önbellek depolama hesabı Doğu Asya bölgesinde oluşturulan.
2. Hedef depolama hesabı (Güneydoğu Asya VM özgün depolama hesabı) mevcut değilse yeni bir tane oluşturulur. Adı "ile asr" sonekine Doğu Asya sanal makinenin depolama hesabıdır.
3. Hedef AV kümesi yok ve Varsayılanları algılayan, yeni bir AV kümesi oluşturmak için gereken ardından yükü işinin bir parçası oluşturulur. Yükü özelleştirdiyseniz, seçili AV kümesini kullanılır.


Bir yükü iş tetiklemek yükleyen durum adımlarının listesi verilmiştir. Hedef tarafı sanal makinenin mevcut durumda budur.

1. Gerekli yapıları yeniden koruma bir parçası olarak oluşturulur. Ardından zaten varsa, bunlar yeniden kullanılır.
2. Çalışıyorsa hedef tarafı (Güneydoğu Asya) sanal makine önce devre dışı bırakılır.
3. Hedef tarafı sanal makinenin disk Azure Site Recovery tarafından bir kapsayıcıya çekirdek blob olarak kopyalanır.
4. Hedef tarafı sanal makine ardından silinir.
5. Çekirdek blob geçerli kaynak tarafı (Doğu Asya) sanal makine tarafından çoğaltmak için kullanılır. Bu, yalnızca farkları çoğaltılır sağlar.
6. Kaynak disk çekirdek blob arasındaki önemli değişiklikler eşitlenir. Bu işlemin tamamlanması biraz zaman alabilir.
7. Yükü işi tamamlandıktan sonra değişim çoğaltması ilke başına bir kurtarma noktası oluşturan başlar.

> [!NOTE]
> Bir kurtarma planı düzeyde koruyamaz. Konumundaki yalnızca koruyun bir VM gerçekleştiriliyordu.

Sanal makinenin yükü başarılı olduktan sonra korumalı bir duruma girer.

## <a name="next-steps"></a>Sonraki adımlar

Sanal makinenin korumalı bir duruma geçtiğini sonra bir yük devretme işlemi başlatabilirsiniz. Yük devretme Doğu Asya Azure bölgesinde sanal makineyi kapatın ve ardından oluşturur ve Güneydoğu Asya bölge sanal makine önyükleme. Bu nedenle uygulama için kısa bir kapalı kalma süresi yok. Uygulamanızı bir kapalı kalma süresi dayanabileceği, bu nedenle, yük devretme için saati seçin. Yük devretme testi sanal makinenin ilk çalıştırma, emin olmak için bunu doğru şekilde, bir yük devretmeyi başlatmadan önce geldiği önerilir.

-   [Sanal makinenin yük devretme test adımları başlatmak için](../site-recovery-test-failover-to-azure.md)

-   [Sanal makinenin yük devretme başlatmak için adımları](../site-recovery-failover.md)
