---
title: Azure Site Recovery hizmeti ile olağanüstü durum kurtarma için ikincil bir Azure bölgesinde çoğaltılmasını geri Azure Iaas Vm'leri başarısız.
description: Azure Site Recovery hizmeti ile geri Azure Vm'leri başarısız öğrenin.
services: site-recovery
author: sideeksh
manager: rochakm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: sideeksh
ms.custom: mvc
ms.openlocfilehash: d8721f313907f0e0519dca52f5565853f1c44110
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58089718"
---
# <a name="fail-back-azure-vms-between-azure-regions"></a>Azure bölgeleri arasında geri Azure Vm'leri başarısız

[Azure Site Recovery](site-recovery-overview.md) hizmeti yönetme ve çoğaltma, yük devretme ve yeniden şirket içi makinelerin ve Azure sanal makineleri (VM'ler), başarısız işlemlerini olağanüstü durum kurtarma stratejinize katkıda bulunur.

Bu öğreticide, tek bir Azure VM geri dönecek şekilde açıklar. Yük devrettikten sonra, uygun olduğunda birincil bölgeye geri dönersiniz. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> 
> * İkincil VM’yi geri döndürme
> * Birincil VM geri ikincil bölgeye yeniden koruma
> 
> [!NOTE]
> 
> Bu öğreticide, en az özelleştirme geri ile bir hedef bölgeye yük devretme için kullanıcının adım adım kılavuz için tasarlanmıştır; ağ konuları da dahil olmak üzere, yük devretme testiyle ilişkili çeşitli yönleri hakkında daha fazla bilgi istemeniz durumunda Otomasyon veya sorun giderme atın 'Nasıl yapılır' altında belgeleri Azure Vm'leri için.

## <a name="prerequisites"></a>Önkoşullar

> * VM yük devretme yürütüldü durumunda olduğundan emin olun ve birincil bölgenin kullanılabilir olduğunu ve oluşturabilmek ve yeni kaynaklarına erişimi denetleyin.
> * Yeniden koruma etkin olduğundan emin olun.

## <a name="fail-back-to-the-primary-region"></a>Birincil bölgeye geri dönme

Vm'leri yeniden koruma altına olduktan sonra birincil bölge dönün ve aşağıdakileri yapmak istediğinizde başarısız olabilir.

1. Git, Kurtarma Hizmetleri kasası. Çoğaltılan öğeler üzerinde tıklayın ve yeniden koruma altına alındı VM'yi seçin.

2. Aşağıdaki görmeniz gerekir. Birincil bölgede yük devretme testi ve yük devretme için dikey penceresine benzer olduğuna dikkat edin.
![Birincile yeniden çalışma](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback.png)

3. Yük devretme testi, birincil bölgeye geri dön gerçekleştirmek için yük devretme testi tıklayın. Yük devretme testi için sanal ağ ve kurtarma noktası seçin ve Tamam'ı seçin. VM, erişebileceğiniz ve İnceleme birincil bölgede oluşturulan test görebilirsiniz.

4. Sınama Yük Devretmesini tatmin olduktan sonra Yük devretme testi için kaynak bölgede oluşturulan kaynakları temizlemek için yük devretme testini Temizle tıklayabilirsiniz.

5. Çoğaltılan öğe, yük devretme için istediğiniz VM'yi seçin > Yük devretme.

6. Bir yük Devretmede, yük devretme için bir kurtarma noktası seçin. Şu seçeneklerden birini kullanabilirsiniz:
    1. En son (varsayılan): Bu seçenek Site Recovery hizmetindeki tüm verileri işler ve en düşük kurtarma noktası hedefi (RPO) sağlar.
    2. En son işlenen: Bu seçenek Site Recovery hizmeti tarafından işlenen en son kurtarma noktasını sanal makineye geri döner.
    3. Özel: Belirli bir kurtarma noktasına yük devretme için bu seçeneği kullanın. Bu seçenek, yük devretme testi gerçekleştirmek için faydalıdır

7. Kapatma, Site Recovery, yük devretmeyi tetiklemeden önce kaynak sanal makineleri kapatmayı denemek isterseniz yük devretmeye başlamadan önce makineyi seçin. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Site Recovery daha önceden Not Kaynak yük devretme işleminden sonra temizleme.

8. Yük devretme ilerleme işleri sayfasında izleyebilirsiniz.

9. Yük devretmeden sonra, sanal makineyi doğrulamak için makinede oturum açın. Ardından sanal makine için başka bir kurtarma noktasına gitmek istiyorsanız, değişiklik kurtarma noktası seçeneğini kullanabilirsiniz.

10. Memnun olduğunuzda sanal makine yük devretmeyi yürütürsünüz. Yürütme işlemi, hizmette kullanılabilir olan tüm kurtarma noktalarını siler. Değişiklik kurtarma noktası seçeneği artık kullanılabilir.

![Birincil ve ikincil bölgeler VM](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback-vm-view.png)

Önceki ekran, "ContosoWin2016" görürseniz VM Orta ABD Doğu ABD olarak devredilir ve geri Doğu ABD Orta ABD için başarısız oldu.

DR Vm'lerine kapatma paylaştırılmamış durumda kalmasını unutmayın. Azure Site Recovery birincil daha sonra ikincil bölgeye yük devretme yararlı olabilecek bilgiler sanal makinenin gerektirmediğinden, bu davranış tasarım gereğidir. Olduğu gibi tutulmalıdır şekilde paylaştırılmamış sanal makineler için ücretlendirilmezsiniz.

> [!NOTE]
> Bkz: ["nasıl" bölümünde](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect#what-happens-during-reprotection) yeniden koruma iş akışı ve yeniden koruma sırasında ne hakkında daha fazla ayrıntı için.
