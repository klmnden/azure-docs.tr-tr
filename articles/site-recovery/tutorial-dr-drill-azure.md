---
title: Azure Site Recovery ile şirket içi makineler için olağanüstü durum kurtarma tatbikatı gerçekleştirme | Microsoft Docs
description: Azure Site Recovery ile şirket içinden Azure’a olağanüstü durum kurtarma tatbikatı gerçekleştirme hakkında bilgi alın
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 10/10/2018
ms.author: raynew
ms.openlocfilehash: 3be3631d8d917fe9ff85e8471a35ac2ddece80b7
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49078163"
---
# <a name="run-a-disaster-recovery-drill-to-azure"></a>Azure’da olağanüstü durum kurtarma tatbikatı çalıştırma

Bu makalede, Azure’da bir yük devretme testi kullanarak şirket içi makine için olağanüstü durum kurtarma tatbikatı çalıştırma işlemi gösterilmektedir. Tatbikat, veri kaybı olmadan çoğaltma stratejinizi doğrular.

Bu, şirket içi VMware veya Hyper-V sanal makineleri için Azure’da olağanüstü durum kurtarmanın nasıl ayarlanacağını gösteren serideki dördüncü öğreticidir.

Bu öğreticide, ilk üç öğreticiyi tamamladığınız varsayılır:
    - [Birinci öğreticide](tutorial-prepare-azure.md), VMware olağanüstü durum kurtarma için gerekli Azure bileşenlerini ayarladık.
    - [İkinci öğreticide](vmware-azure-tutorial-prepare-on-premises.md), olağanüstü durum kurtarma için şirket içi bileşenleri hazırladık ve önkoşulları gözden geçirdik.
    - [Üçüncü öğreticide](vmware-azure-tutorial.md), şirket içi VMware sanal makinemiz için çoğaltmayı ayarlayıp etkinleştirdik.
    - Öğreticiler, **bir senaryo için en basit dağıtım yolunu** size göstermek için tasarlanmıştır. Mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Yük devretme testi adımları hakkında daha ayrıntılı bilgi edinmek istiyorsanız, [Nasıl Yapılır Kılavuzu](site-recovery-test-failover-to-azure.md)’nu okuyun.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Yük devretme testi için yalıtılmış bir ağ ayarlama
> * Yük devretme sonrasında Azure VM'ye bağlanmak için hazırlık yapma
> * Tek bir makine için yük devretme testi çalıştırma



## <a name="verify-vm-properties"></a>VM özelliklerini doğrulama

Bir yük devretme testi çalıştırmadan önce VM özelliklerini doğrulayın ve [Hyper-V VM](hyper-v-azure-support-matrix.md#replicated-vms), [VMware VM](vmware-physical-azure-support-matrix.md#replicated-machines)'nin Azure gereksinimlerine uygun olduğundan emin olun.

1. **Korunan Öğeler**’de **Çoğaltılan Öğeler** > VM seçeneğine tıklayın.
2. **Çoğaltılan öğe** bölmesinde VM bilgileri ile sistem durumunun bir özeti ve kullanılabilen son kurtarma noktaları yer alır. Daha fazla ayrıntı görüntülemek için **Özellikler**’e tıklayın.
3. **İşlem ve Ağ** bölümünde Azure adını, kaynak grubunu, hedef boyutunu, kullanılabilirlik kümesini ve yönetilen disk ayarlarını değiştirebilirsiniz.
4. Ağ ayalarını, yük devretmeden sonra Azure VM’nin yerleştirileceği ağ/alt ağ ve VM’ye atanacak IP adresi dahil olmak üzere görüntüleyebilir ve değiştirebilirsiniz.
5. **Diskler** bölümünde VM’deki işletim sistemi ve veri diskleriyle ilgili bilgileri görüntüleyebilirsiniz.

## <a name="create-a-network-for-test-failover"></a>Yük devretme testi için ağ oluşturma

Yük devretme testi için her VM'nin **İşlem ve Ağ** ayarlarından üretim kurtarma sitesi ağınızdan yalıtılmış bir ağ seçmenizi öneririz. Varsayılan olarak yeni oluşturduğunuz Azure sanal ağları diğer ağlardan yalıtılmış olur. Test ağı üretim ağınıza benzer olmalıdır:

- Test ağında üretim ağınızdaki sayıda alt ağ olmalıdır. Alt ağların adı aynı olmalıdır.
- Test ağında aynı IP adresi aralığı kullanılmalıdır.
- Test ağının DNS sunucusunu **İşlem ve Ağ** ayarlarında DNS VM için belirtilen IP adresiyle güncelleştirin. Daha fazla ayrıntı için [Active Directory için yük devretme testi ile ilgili dikkat edilmesi gerekenler](site-recovery-active-directory.md#test-failover-considerations) sayfasını okuyun.

## <a name="run-a-test-failover-for-a-single-vm"></a>Tek bir VM için yük devretme testi çalıştırma

Yük devretme testi çalıştırdığınızda şunlar olur:

1. Yük devretme için gerekli tüm koşulların karşılandığından emin olmak için bir önkoşul denetimi çalıştırılır.
2. Yük devretme, bir Azure sanal makinesi oluşturulabilmesi için verileri işler. En son kurtarma noktasını seçerseniz verilerden bir kurtarma noktası oluşturulur.
3. Önceki adımda işlenen veriler kullanılarak bir Azure sanal makinesi oluşturulur.

Yük devretme testini aşağıdaki gibi çalıştırın:

1. **Ayarlar** > **Çoğaltılmış Öğeler** bölümünde, CM > **+Yük Devretme Testi**’ne tıklayın.
2. Bu öğretici için **Son işlenen** kurtarma noktasını seçin. Bu işlem VM yükünü zamandaki en son kullanılabilir noktaya devreder. Zaman damgası gösterilir. Bu seçenekte, verileri işlemek için zaman harcanmadığından düşük bir RTO (kurtarma süresi hedefi) sağlanır.
3. **Yük Devretme Testi** bölümünde, yük devretme gerçekleştikten sonra Azure VM’lerinin bağlanacağı hedef Azure ağını seçin.
4. Yük devretmeyi başlatmak için **Tamam**'a tıklayın. Sanal makine özelliklerini açmak için sanal makineye tıklayarak ilerleme durumunu izleyebilirsiniz. Ya da kasa adı > **Ayarlar** > **İşler** >
   **Site Recovery işleri** bölümünde **Yük Devretme Testi** işine tıklayabilirsiniz.
5. Yük devretme bittikten sonra, çoğaltma Azure VM, Azure portalı > **Sanal Makineler** bölümünde görünür. Sanal makinenin uygun boyutta olduğundan, doğru ağa bağlandığından ve çalıştığından emin olun.
6. Şimdi Azure’da çoğaltılan sanal makineye bağlanabiliyor olmanız gerekir.
7. Yük devretme testi sırasında oluşturulan Azure sanal makinelerini silmek için, VM’de **Yük devretme testini temizle**’ye tıklayın. Yük devretme testiyle ilişkili gözlemlerinizi **Notlar**’da kaydedin veya saklayın.

Bazı senaryolarda yük devretme için sekiz ila on dakikada tamamlanan ek işlem gerekir. VMware Linux makinelerinde, DHCP hizmeti etkinleştirilmemiş VMware VM’lerinde ve storvsc, vmbus, storflt, intelide, atapi önyükleme sürücülerine sahip olmayan VMware VM’lerinde uzun yük devretme testi süreleriyle karşılaşabilirsiniz.

## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Yük devretme sonrasında RDP/SSH kullanarak Azure VM'lerine bağlanmak istiyorsanız [buradaki](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover) tabloda özetlenen gereksinimleri izleyin.

Yük devretme sonrasında karşılaştığınız bağlantı sorunlarını gidermek için [burada](site-recovery-failover-to-azure-troubleshoot.md) anlatılan adımları izleyin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Şirket içi VMware VM’ler arasında yük devretme ve yeniden çalışma işlemi gerçekleştirme](vmware-azure-tutorial-failover-failback.md).
> [Şirket içi Hyper-V VM’ler arasında yük devretme ve yeniden çalışma işlemi gerçekleştirme](hyper-v-azure-failover-failback-tutorial.md).
