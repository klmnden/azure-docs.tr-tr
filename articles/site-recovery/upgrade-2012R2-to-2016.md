---
title: Windows Server 2012 R2 konakların ve Windows Server 2016 için Azure Site Recovery ile yapılandırılmış SCVMM yükseltme
description: Azure'da olağanüstü durum kurtarma için Azure Stack Vm'leri Azure Site Recovery hizmeti ile nasıl ayarlanacağını öğrenin.
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.topic: conceptual
ms.service: site-recovery
ms.date: 12/03/2018
ms.author: rajanaki
ms.openlocfilehash: b67290f72f762331a6d699fb79aef0c0d7f9fb65
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61275532"
---
# <a name="upgrade-windows-server-2012-r2-hosts-scvmm-2012-r2-configured-with-azure-site-recovery-to-windows-server-2016--scvmm-2016"></a>Windows Server 2012 R2 ana bilgisayarları, Windows Server 2016 & SCVMM 2016 Azure Site Recovery ile yapılandırılmış SCVMM 2012 R2 yükseltme

Bu makalede, Windows Server 2012 R2 konakların ve Azure Site Recovery, Windows Server 2016 & SCVMM 2016 ile yapılandırılmış bir SCVMM 2012 R2 sürümüne yükseltme yapmayı gösterir

Site Recovery, iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize katkı sağlar. VM iş yüklerinizi beklendiği zaman kullanılabilir durumda kalır ve beklenmeyen kesintiler hizmet sağlar.

> [!IMPORTANT]
> Azure Site Recovery çoğaltması için yapılandırılmış olan Windows Server 2012 R2 konak yükselttiğinizde, bu belgede anlatılan adımları izlemelisiniz. Yükseltme için seçilen herhangi bir alternatif yolu desteklenmeyen durumlarda neden olabilir ve bir kesme çoğaltma veya yük devretme gerçekleştirme becerisi neden olabilir.


Bu makalede, ortamınızda aşağıdaki yapılandırmaları sürümüne yükseltme yapmayı öğrenin:

> [!div class="checklist"]
> * **Windows Server 2012 R2 konak, bir SCVMM tarafından yönetilmiyor** 
> * **Tek başına SCVMM 2012 R2 sunucusu tarafından yönetilen Windows Server 2012 R2 konak** 
> * **Yüksek oranda kullanılabilir bir SCVMM 2012 R2 sunucusu tarafından yönetilen Windows Server 2012 R2 konak**


## <a name="prerequisites--factors-to-consider"></a>Önkoşulları & dikkat edilecek noktalar

Yükseltmeden önce aşağıdakilere dikkat edin:-

- SCVMM tarafından yönetilmeyen Windows Server 2012 R2 ana bilgisayarları varsa ve tek başına ortam oluşturma, olacaktır mola Çoğaltmada yükseltme yapmayı denerseniz.
- Seçtiyseniz, "*Anahtarlarımı Active Directory'de dağıtılmış anahtar yönetimi altında depolamamayı*" ilk başta SCVMM 2012 R2'yi yüklerken, yükseltmeleri başarıyla tamamlanmayacaktır.

- System Center 2012 R2 VMM kullanıyorsanız, 

    - VMM veritabanı bilgileri kontrol edin: **VMM konsolunu** -> **ayarları** -> **genel** -> **veritabanı bağlantısı**
    - System Center Virtual Machine Manager Aracısı hizmeti için kullanılan hizmet hesaplarını denetle
    - VMM veritabanının bir yedeği olduğundan emin olun.
    - SCVMM sunucular, veritabanı adı not edin. Bu giderek yapılabilir **VMM konsolunu** -> **ayarları** -> **genel** -> **veritabanı bağlantısı**
    - 2012R2 birincil ve kurtarma VMM sunucuları VMM Kimliğini not alın. Kayıt defterinden "HKLM:\SOFTWARE\Microsoft\Microsoft System Center Virtual Machine Manager Server\Setup" VMM kimliği bulunamadı.
    - Önceden olarak, kümeye eklemek yeni SCVMMs aynı adları geçerli olduğundan emin olun. 

- Her iki kenarı da SCVMMs tarafından yönetilen sitelerinizden ikisi arasında çoğaltıyorsanız, birincil tarafı yükseltmeden önce kurtarma tarafı önce yükseltmeniz emin olun.
  > [!WARNING]
  > İçin Dağıtılmış anahtar yönetimi altında SCVMM 2012 R2 yükseltilirken seçin **şifreleme anahtarlarını Active Directory'de depolamak**. Hizmet hesabını ve dağıtılmış anahtar yönetimi ayarlarını dikkatle seçin. Gibi şablonlardaki parolalar değil yükseltmeden sonra kullanılabilir ve büyük olasılıkla için yaptığınız seçime göre şifrelenmiş veriler Azure Site Recovery ile çoğaltmayı etkiler

> [!IMPORTANT]
> Lütfen ayrıntılı SCVMM belgelerine başvurun [önkoşulları](https://docs.microsoft.com/system-center/vmm/upgrade-vmm?view=sc-vmm-2016#requirements-and-limitations)

## <a name="windows-server-2012-r2-hosts-which-arent-managed-by-scvmm"></a>Windows Server 2012 R2 konak, bir SCVMM tarafından yönetilmiyor 
Kullanıcı yapılandırmasından aşağıda belirtilen adımları listesi uygulandığı [azure'a Hyper-V konakları](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-architecture) takip ederek yürütülen [Öğreticisi](https://docs.microsoft.com/azure/site-recovery/hyper-v-prepare-on-premises-tutorial)

> [!WARNING]
> Önkoşullarda belirtildiği gibi bu adımlar yalnızca kümelenmiş ortam senaryosu ve bir tek başına Hyper-V ana bilgisayar yapılandırması içinde değil geçerlidir.

1. Adımları gerçekleştirmek için [sıralı küme yükseltmesi.](https://docs.microsoft.com/windows-server/failover-clustering/cluster-operating-system-rolling-upgrade#cluster-os-rolling-upgrade-process) sıralı Küme yükseltme işlemini yürütmek için.
2. Kümede kullanıma sunulan her yeni Windows Server 2016 ana bilgisayar ile bir Windows Server 2012 R2 konağı başvurusu, [burada] belirtilen adımları izleyerek Azure Site Recovery'den kaldırın. Bu konak kümesinden Tahliye & Boşalt seçtiniz olmalıdır.
3. Bir kez *güncelleştirme VMVersion* komutu tüm sanal makineler için yürütüldü, yükseltme tamamlandı. 
4. Belirtilen adımları kullanmak [burada](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-tutorial#set-up-the-source-environment) yeni Windows Server 2016 konağı Azure Site recovery'ye kaydetmek için. Hyper-V sitesi halen etkin olduğundan ve yeni konak kümesine kaydetmek yeterlidir lütfen unutmayın. 
5.  Azure portalına gidin ve kurtarma Hizmetleri içinde çoğaltılmış sistem durumunu doğrulayın

## <a name="upgrade-windows-server-2012-r2-hosts-managed-by-stand-alone-scvmm-2012-r2-server"></a>Tek başına SCVMM 2012 R2 sunucusu tarafından yönetilen Windows Server 2012 R2'in konakları yükseltme
Windows Server 2012 R2 konaklarınız yükseltmeden önce SCVMM 2012 R2'yi SCVMM 2016'ya yükseltmeniz gerekir. İzleyin aşağıdaki adımları:-

**Tek başına SCVMM 2012 R2'yi SCVMM 2016'ya yükseltme**

1.  Denetim Masası'na giderek kaldırma ASR sağlayıcısı -> Programlar -> Programlar ve Özellikler -> Microsoft Azure Site Recovery ve üzerinde Kaldır'ı tıklatın
2. [SCVMM veritabanını korumak ve işletim sistemini yükseltme](https://docs.microsoft.com/system-center/vmm/upgrade-vmm?view=sc-vmm-2016#back-up-and-upgrade-the-operating-system)
3. İçinde **Ekle Kaldır**seçin **VMM** > **kaldırma**. b. Seçin **özellikleri Kaldır**ve V ardından**MM yönetim sunucusu ve VMM konsolunu**. c. İçinde **veritabanı seçenekleri**seçin **veritabanını tut**. d. Özeti gözden geçirin ve tıklayın **kaldırma**.

4. [VMM 2016'yı yükleyin](https://docs.microsoft.com/system-center/vmm/upgrade-vmm?view=sc-vmm-2016#install-vmm-2016)
5. SCVMM başlatın ve her bir ana bilgisayar durumunu **dokularını** sekmesi. Tıklayın **Yenile** en son durumu almak için. "Dikkat gerekiyor" durumuna görmeniz gerekir. 
17. Son yükleme [Microsoft Azure Site Recovery sağlayıcısı](https://aka.ms/downloaddra) SCVMM üzerinde.
16. Son yükleme [Microsoft Azure kurtarma hizmeti (MARS) aracısı](https://aka.ms/latestmarsagent) kümenin her bir konakta. SCVMM ana başarıyla sorgulayabilir olduğundan emin olmak için yenileyin.

**Windows Server 2012 R2 konak Windows Server 2016'ya yükseltme**

1. Belirtilen adımları takip [burada](https://docs.microsoft.com/windows-server/failover-clustering/cluster-operating-system-rolling-upgrade#cluster-os-rolling-upgrade-process) sıralı Küme yükseltme işlemini yürütmek için. 
2. Yeni konağın kümeye ekledikten sonra bu güncelleştirilmiş bir konakta VMM aracısını yüklemek için ana bilgisayardan SCVMM konsolunu yenileyin.
3. Yürütme *güncelleştirme VMVersion* sanal makinelerin VM sürümlerinin güncelleştirilecek. 
4.  Azure portalına gidin ve kurtarma Hizmetleri kasası içindeki sanal makinelere çoğaltılmış sistem durumunu doğrulayın. 

## <a name="upgrade-windows-server-2012-r2-hosts-are-managed-by-highly-available-scvmm-2012-r2-server"></a>Yükseltme Windows Server 2012 R2 konak yüksek oranda kullanılabilir bir SCVMM 2012 R2 sunucusu tarafından yönetilir
Windows Server 2012 R2 konaklarınız yükseltmeden önce SCVMM 2012 R2'yi SCVMM 2016'ya yükseltmeniz gerekir. Azure Site Recovery - ek hiçbir VMM sunucusu ile karışık mod & ek VMM sunucuları ile karma mod ile yapılandırılmış SCVMM 2012 R2 sunucusu yükseltilirken aşağıdaki modları yükseltme desteklenir.

**SCVMM 2012 R2'yi SCVMM 2016'ya yükseltme**

1.  Denetim Masası'na giderek kaldırma ASR sağlayıcısı -> Programlar -> Programlar ve Özellikler -> Microsoft Azure Site Recovery ve üzerinde Kaldır'ı tıklatın
2. Belirtilen adımları takip [burada](https://docs.microsoft.com/system-center/vmm/upgrade-vmm?view=sc-vmm-2016#upgrade-a-standalone-vmm-server) tabanlı üzerinde yürütmek istediğiniz yükseltme modu.
3. SCVMM konsolunu başlatın ve her bir ana bilgisayar durumunu **dokularını** sekmesi. Tıklayın **Yenile** en son durumu almak için. "Dikkat gerekiyor" durumuna görmeniz gerekir.
4. Son yükleme [Microsoft Azure Site Recovery sağlayıcısı](https://aka.ms/downloaddra) SCVMM üzerinde.
5. En son güncelleştirme [Microsoft Azure kurtarma hizmeti (MARS) aracısı](https://aka.ms/latestmarsagent) kümenin her bir konakta. SC VMM konakları başarıyla sorgulayabilir olduğundan emin olmak için yenileyin.


**Windows Server 2012 R2 konak Windows Server 2016'ya yükseltme**

1. Belirtilen adımları takip [burada](https://docs.microsoft.com/windows-server/failover-clustering/cluster-operating-system-rolling-upgrade#cluster-os-rolling-upgrade-process) sıralı Küme yükseltme işlemini yürütmek için.
2. Yeni konağın kümeye ekledikten sonra bu güncelleştirilmiş bir konakta VMM aracısını yüklemek için ana bilgisayardan SCVMM konsolunu yenileyin.
3. Yürütme *güncelleştirme VMVersion* sanal makinelerin VM sürümlerinin güncelleştirilecek. 
4.  Azure portalına gidin ve kurtarma Hizmetleri kasası içindeki sanal makinelere çoğaltılmış sistem durumunu doğrulayın. 

## <a name="next-steps"></a>Sonraki adımlar
Konak yükseltme gerçekleştirildikten sonra gerçekleştirebileceğiniz bir [yük devretme testi](tutorial-dr-drill-azure.md) çoğaltma ve olağanüstü durum kurtarma durumunuzu durumunu test etmek için.

