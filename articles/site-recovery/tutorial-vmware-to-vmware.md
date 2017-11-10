---
title: "VMware Vm'lerini veya fiziksel sunucuları ikincil bir siteye Azure Site Recovery ile olağanüstü durum kurtarma ayarlama | Microsoft Docs"
description: "VMware Vm'leri veya Windows ve Linux fiziksel sunucuları ikincil bir siteye Azure Site Recovery ile olağanüstü durum kurtarma ayarlanacağını öğrenin."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: gauarvd
editor: 
ms.assetid: 68616d15-398c-4f40-8323-17b6ae1e65c0
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: raynew
ms.openlocfilehash: 503d7060437d08ed35681fca7f1b9306746b7f44
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2017
---
# <a name="set-up-disaster-recovery-of-on-premises-vmware-virtual-machines-or-physical-servers-to-a-secondary-site"></a>Şirket içi VMware sanal makineleri veya fiziksel sunucuları ikincil bir siteye olağanüstü durum kurtarma ayarlama

Inmage Scout [Azure Site Recovery](site-recovery-overview.md) VMware siteleri arasında şirket içi gerçek zamanlı çoğaltma sağlar. Inmage Scout Azure Site Recovery hizmeti Aboneliklerde dahil edilir. 


## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

- [Gözden geçirme](site-recovery-support-matrix-to-sec-site.md) tüm bileşenleri için destek gereksinimleri.
- Çoğaltmak istediğiniz makineleri ile uyumlu olduğundan emin olun [makine desteği çoğaltılan](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions).


## <a name="create-a-vault"></a>Bir kasa oluşturun

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="choose-a-protection-goal"></a>Koruma hedefi seçin

Ne çoğaltılacağı ve için çoğaltma yeri seçin.

1. Tıklatın **Site Recovery** > **altyapıyı hazırlama** > **koruma hedefi**.
2. Seçin **kurtarma sitesine** > **Evet, VMware vSphere hiper yönetici ile**. Daha sonra, **Tamam**'a tıklayın.
3. İçinde **Scout Kurulum**, Inmage Scout 8.0.1 GA yazılım ve kayıt anahtarını indirin. Tüm bileşenler için Kurulum dosyaları indirilen .zip dosyasına dahil edilir.

## <a name="download-and-install-component-updates"></a>Bileşen güncelleştirmeleri karşıdan yükleyip

 Gözden geçirme ve en son yükleme [güncelleştirmeleri](#updates). Güncelleştirmeleri aşağıdaki sırayla sunucularda yüklü olmalıdır:

1. RX sunucu (varsa)
2. Yapılandırma sunucuları
3. İşlem sunucuları
4. Ana hedef sunucuları
5. vContinuum sunucuları
6. Kaynak sunucu (Windows ve Linux sunucuları)

Güncelleştirmeleri gibi yükleyin:

> [!NOTE]
>Tüm Scout bileşenleri dosya güncelleştirme sürümü güncelleştirme .zip dosyasını aynı olmayabilir. Eski sürümü olduğunu bileşen değişiklik bu güncelleştirme için önceki güncelleştirmeden bu yana gösterir.

Karşıdan [güncelleştirme](https://aka.ms/asr-scout-update6) .zip dosyası. Dosya aşağıdaki bileşenleri içerir: 
  - RX_8.0.4.0_GA_Update_4_8725872_16Sep16.Tar.gz
  - CX_Windows_8.0.6.0_GA_Update_6_13746667_18Sep17.exe
  - UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe
  - UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
  - vCon_Windows_8.0.6.0_GA_Update_6_11525767_21Sep17.exe
  - UA update4 BITS RHEL5, OL5, OL6, SUSE 10, SUSE 11: UA_<Linux OS>_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
1. .Zip dosyalarını ayıklayın.
2. **RX sunucu**: kopyalama **RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz** RX sunucusuna ve ayıklayın. Ayıklanan klasöründe Çalıştır **/yüklemesi**.
3. **Yapılandırma sunucusu ve işlem sunucusu**: kopyalama **CX_Windows_8.0.6.0_GA_Update_6_13746667_18Sep17.exe** yapılandırma sunucusu ve işlem sunucusu. Çalıştırmak için çift tıklayın.<br>
4. **Windows ana hedef sunucusu**: Birleşik aracısını güncelleştirmek için kopyalama **UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe** sunucuya. Çalıştırmak için çift tıklayın. Aynı birleşik aracı güncelleştirme de kaynak sunucu için geçerlidir. Güncelleştirme 4 kaynak üzere güncelleştirilmedi, birleşik aracı güncelleştirmeniz gerekir.
  Güncelleştirme yöneticisinde uygulamak gerekmez hedef hazırlanmış ile **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_10Oct2017_release.exe** tüm son değişikliklerle yeni GA yükleyici olarak.
5. **vContinuum sunucusu**: kopyalama **vCon_Windows_8.0.6.0_GA_Update_6_11525767_21Sep17.exe** sunucuya.  VContinuum Sihirbazı'nı kapattığınız emin olun. Çalıştırmak için dosyaya çift tıklayın.
    Güncelleştirme ile hazırlanmış ana hedefte uygulamak gerekmez **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_10Oct2017_release.exe** tüm son değişikliklerle yeni GA yükleyici olarak.
6. **Linux ana hedef sunucusu**: Birleşik aracısını güncelleştirmek için kopyalama **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** ana hedef sunucusunda ve ayıklayın. Ayıklanan klasöründe Çalıştır **/yüklemesi**.
7. **Windows kaynak sunucu**: Birleşik aracısını güncelleştirmek için kopyalama **UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe** kaynak sunucuda. Çalıştırmak için dosyaya çift tıklayın. 
    Güncelleştirme 4'e zaten güncelleştirildi veya kaynak Aracısı en son temel Yükleyici ile yüklüyse kaynak sunucuda güncelleştirme 5 Aracısı'nı yüklemeniz gerekmez **InMage_UA_8.0.1.0_Windows_GA_28Sep2017_release.exe**.
8. **Linux kaynak sunucu**: Birleşik aracısını güncelleştirmek için birleşik aracı dosyası ilgili sürümü Linux sunucuya kopyalayın ve ayıklayın. Ayıklanan klasöründe Çalıştır **/yüklemesi**.  Örnek: RHEL 6,7 64 bit sunucu için kopyalama **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** sunucusuna ve ayıklayın. Ayıklanan klasöründe Çalıştır **/yüklemesi**.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

1. VMware siteleri hedef ve kaynak arasında çoğaltmayı ayarlayın.
2. Kurulum, koruma ve kurtarma hakkında daha fazla bilgi edinmek için aşağıdaki belgelere bakın:

   * [Sürüm notları](https://aka.ms/asr-scout-release-notes)
   * [Uyumluluk matrisi](https://aka.ms/asr-scout-cm)
   * [Kullanıcı Kılavuzu](https://aka.ms/asr-scout-user-guide)
   * [RX Kullanıcı Kılavuzu](https://aka.ms/asr-scout-rx-user-guide)
   * [Hızlı Yükleme Kılavuzu](https://aka.ms/asr-scout-quick-install-guide)

## <a name="updates"></a>Güncelleştirmeler

### <a name="site-recovery-scout-801-update-6"></a>Site Recovery Scout 8.0.1 güncelleştirme 6 
Güncelleştirilmiş: 12 Ekim 2017

Karşıdan [Scout güncelleştirme 6](https://aka.ms/asr-scout-update6).

Scout güncelleştirme 6 birikmeli bir güncelleştirmedir. Güncelleştirme 1'deki tüm düzeltmeleri güncelleştirme 5 artı yeni düzeltmeler ve aşağıda açıklanan geliştirmeleri içerir. 

#### <a name="new-platform-support"></a>Yeni platform desteği
* Kaynak Windows Server 2016 için destek eklenmiştir
* Aşağıdaki Linux işletim sistemleri için destek eklenmiştir:
    - Red Hat Enterprise Linux (RHEL) 6.9
    - CentOS 6.9
    - Oracle Linux 5.11
    - Oracle Linux 6,8
* VMware merkezi 6.5 için destek eklenmiştir

> [!NOTE]
> * Windows için temel Agent(UA) Birleşik Yükleyici Windows Server 2016 desteklemek için güncelleştirildi. Yeni yükleyici **InMage_UA_8.0.1.0_Windows_GA_28Sep2017_release.exe** temel Scout GA paketi ile birlikte paketlenmiştir (**InMage_Scout_Standard_8.0.1 GA-Oct17.zip**). Aynı yükleyici tüm desteklenen Windows sürümü için kullanılır. 
> * Temel Windows vContinuum & ana hedef yükleyici desteklemek için Windows Server 2016 yenilendi açıldı. Yeni yükleyici **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_10Oct2017_release.exe** temel Scout GA paketi ile birlikte paketlenmiştir (**InMage_Scout_Standard_8.0.1 GA-Oct17.zip**). Aynı yükleyici Windows 2016 ana hedef ve Windows 2012R2 ana hedef dağıtmak için kullanılır.
> * Bölümünde açıklandığı gibi portaldan GA paketini karşıdan yükle [bir kasa oluşturun](#create-a-vault).
>

#### <a name="bug-fixes-and-enhancements"></a>Hata düzeltmeleri ve geliştirmeleri
- Çoğaltılacak disklerin listesini ile Linux VM için yeniden çalışma koruma başarısız config sonunda boş.

### <a name="site-recovery-scout-801-update-5"></a>Site Recovery Scout 8.0.1 güncelleştirme 5
Scout güncelleştirme 5 birikmeli bir güncelleştirmedir. Güncelleştirme 4 için Güncelleştirme 1'deki tüm düzeltmeleri ve aşağıda açıklanan yeni düzeltmeleri içerir.
- Site Recovery Scout güncelleştirme 4'deki düzeltmeleri güncelleştirme 5 özellikle ana hedef ve vContinuum için bileşenleridir.
- Ardından kaynak sunucular, ana hedef, yapılandırma, işlem ve RX sunucuları zaten güncelleştirme 4 çalıştırıyorsanız, yalnızca ana hedef sunucusunda uygulayın. 

#### <a name="new-platform-support"></a>Yeni platform desteği
* SUSE Linux Enterprise Server 11 hizmet paketi 4(SP4)
* SLES 11 SP4 64 bit **InMage_UA_8.0.1.0_SLES11-SP4-64_GA_13Apr2017_release.tar.gz** temel Scout GA paketi ile birlikte paketlenmiştir (**InMage_Scout_Standard_8.0.1 GA.zip**). Bölümünde açıklandığı gibi portaldan GA paketini karşıdan yükle [bir kasa oluşturun](#create-a-vault).


#### <a name="bug-fixes-and-enhancements"></a>Hata düzeltmeleri ve geliştirmeleri

* Artan Windows Küme düzeltmelerin güvenilirlik destekler:
    * Sabit-P2V MSCS bazıları küme diskleri hale RAW kurtarma işleminden sonra.
    * Fixed-P2V MSCS küme kurtarma disk sırası uyumsuzluğundan dolayı başarısız olur.
    * Sabit - MSCS disk boyutu uyuşmazlığı hatası ile başarısız oluyor diskleri ekleme işlemi küme.
    * Sabit hazır olup olmadığını RDM LUN'ları eşleme ile MSCS küme boyutu doğrulama başarısız kaynağı için.
    * Fixed-tek bir düğümün küme koruma SCSI uyumsuzluk sorunları nedeniyle başarısız olur. 
    * Hedef küme diskleri mevcut olup olmadığını P2V Windows Küme sunucusu Fixed-yeniden koruma başarısız olur. 
    
* Sabit: Seçili ana sunucu hedefliyorsanız vContinuum yanlış ana hedef sunucusu yeniden çalışma kurtarma ve Kurtarma sırasında Çekmeleri sonra geri dönme koruma sırasında korumalı kaynak makinesiyle aynı ESXi sunucusunda (iletme koruma sırasında), değil işlem başarısız olur.

> [!NOTE]
> * P2V küme düzeltmeleri, yalnızca yeni Site Recovery Scout güncelleştirme 5 ile korunan fiziksel MSCS kümeler için geçerlidir. Eski güncelleştirmeler ile korunan P2V MSCS kümelerinde küme düzeltmeler yüklemek için 12 bölümünde bahsedilen yükseltme adımları izleyin [Site Recovery Scout sürüm notları](https://aka.ms/asr-scout-release-notes).
> * ardından fiziksel bir MSCS kümesindeki yeniden koruma yalnızca başlangıçta korumalı oldukları gibi yeniden koruma zaman diskleri aynı kümesini etkin her küme düğümü değilse, var olan hedef disklerin yeniden kullanabilirsiniz. Aksi durumda, el ile yapılacak adımlar bölümünde 12 kullanmak [Site Recovery Scout sürüm notları](https://aka.ms/asr-scout-release-notes), hedef tarafı disklerin yeniden koruma sırasında yeniden kullanım için doğru veri deposu yolun taşımak için. Yükseltme adımlarını uymadan P2V modunda MSCS küme koruyun, hedef ESXi sunucusunda yeni bir disk oluşturur. Eski diskleri deposundan el ile silmeniz gerekir.
> * Bir kaynakla SLES11 veya SLES11 (bir hizmet paketi olmayan) sunucusu düzgün bir şekilde yeniden başlatıldığında, el ile işaretleme **kök** disk çoğaltma çiftleri yeniden eşitleme. CX arabiriminde bildirim yoktur. Yeniden eşitleme için kök disk işaretlemeyin, veri bütünlüğü sorunları görebilirsiniz.


### <a name="azure-site-recovery-scout-801-update-4"></a>Azure Site Recovery Scout 8.0.1 güncelleştirme 4
Scout güncelleştirme 4 birikmeli bir güncelleştirmedir. Güncelleştirme 3 güncelleştirme 1'deki tüm düzeltmeleri ve aşağıda açıklanan yeni düzeltmeleri içerir.

#### <a name="new-platform-support"></a>Yeni platform desteği

* VCenter/vSphere için 6.0, 6.1 ve 6.2 desteği eklendi
* Bu Linux işletim sistemleri için destek eklenmiştir:
  * Red Hat Enterprise Linux (RHEL) 7.0, 7.1 ve 7.2
  * CentOS 7.0, 7.1 ve 7.2
  * Red Hat Enterprise Linux (RHEL) 6,8
  * CentOS 6,8

> [!NOTE]
> RHEL/CentOS 7 64-bit **InMage_UA_8.0.1.0_RHEL7-64_GA_06Oct2016_release.tar.gz** temel Scout GA paketi ile birlikte paketlenmiştir **InMage_Scout_Standard_8.0.1 GA.zip**. Bölümünde açıklandığı gibi portaldan Scout GA paketini karşıdan yükle [bir kasa oluşturun](#create-a-vault).

#### <a name="bug-fixes-and-enhancements"></a>Hata düzeltmeleri ve geliştirmeleri

* Geliştirilmiş Kapat: aşağıdaki Linux işletim sistemleri ve istenmeyen yeniden eşitleme sorunlarını önlemek için kopyalayacağı için işleme
    * Red Hat Enterprise Linux (RHEL) 6.x
    * Oracle Linux (OL) 6.x
* Linux için tüm klasör erişim izinleri birleşik aracı yükleme dizini içinde artık yalnızca yerel kullanıcı kısıtlanır.
* Windows, bir zaman aşımının sık karşılaşılan dağıtılmış tutarlılık yer işaretleri verilirken oluştu sorunu için bir düzeltme üzerinde ağır SQL sunucusu ve paylaşım noktası kümeleri gibi dağıtılmış uygulamaların yüklü.
* Bir günlük ilgili düzeltme yapılandırma sunucusu temel yükleyicisi.
* VMware vCLI 6.0 için karşıdan yükleme bağlantısı için Windows ana hedef temel yükleyici eklendi.
* Ek denetimleri ve günlükleri, yük devretme ve olağanüstü durum kurtarma ayrıntılarını sırasında ağ yapılandırması değişikliklerini için eklendi.
* Yapılandırma sunucusuna rapor bekletme bilgileri neden bir sorun için düzeltme.  
* Fiziksel kümeler için birim yeniden boyutlandırma kaynak birim küçültme vContinuum Sihirbazı'nda başarısız olmasına neden olan sorun için düzeltme.
* Hatasıyla başarısız oldu bir küme koruma sorunu için bir düzeltme: "Başarısız disk imzası bulmak", küme diskinin bir PRDM disk olduğunda.
* Bir aralık dışı özel durumdan kaynaklanan bir cxps aktarım sunucusu kilitlenme için düzeltme.
* Sunucu adı ve IP adresi sütunları şimdi yeniden boyutlandırılabilir içinde **anında yükleme** vContinuum sayfası.
* RX API'si geliştirmeleri:
  * Beş en son kullanılabilir ortak tutarlılık şimdi kullanılabilir (yalnızca etiketler garanti) işaret eder.
  * Tüm korumalı aygıtları için kapasite ve boş alan ayrıntıları görüntülenir.
  * Scout sürücü durumu kaynak sunucu üzerindeki kullanılabilir değil.

> [!NOTE]
> * **InMage_Scout_Standard_8.0.1_GA.zip** temel paket vardır:
    * Güncelleştirilmiş yapılandırma sunucusu temel Yükleyicisi (**InMage_CX_8.0.1.0_Windows_GA_26Feb2015_release.exe**)
    * Bir Windows ana hedef temel Yükleyicisi (**InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_26Feb2015_release.exe**).
    * Tüm yeni yüklemeler için yeni yapılandırma sunucusu ve Windows ana hedef GA BITS kullanın.
> * Güncelleştirme 4 doğrudan 8.0.1 üzerinde uygulanabilir İST
> * Geri bunlar uygulanmış sonra RX güncelleştirmeleri ve yapılandırma sunucusu alınamaz.


### <a name="azure-site-recovery-scout-801-update-3"></a>Azure Site Recovery Scout 8.0.1 güncelleştirme 3

Tüm Site Recovery Toplu güncelleştirmelerin. Güncelleştirme 3 güncelleştirme 1 ve güncelleştirme 2'deki tüm düzeltmeleri içerir. Güncelleştirme 3 8.0.1 üzerinde doğrudan da uygulanabilir İST Geri bunlar uygulanmış sonra RX güncelleştirmeleri ve yapılandırma sunucusu alınamaz.

#### <a name="bug-fixes-and-enhancements"></a>Hata düzeltmeleri ve geliştirmeleri
Güncelleştirme 3 aşağıdaki sorunları giderir:

* Proxy'nin arkasında olduğunuzda RX ve yapılandırma sunucusunu kasaya kayıtlı değil.
* Kurtarma noktası hedefi (RPO) ulaştı değildi saat sayısını sistem durumu raporu güncelleştirilmez.
* Yapılandırma sunucusu RX ile eşitleniyor değil zaman ESX donanım ayrıntılarını ya da ağ ayrıntıları, tüm UTF-8 karakterler içerebilirler.
* Windows Server 2008 R2 etki alanı denetleyicileri kurtarma işleminden sonra başlatmayın.
* Çevrimdışı eşitleme beklendiği gibi çalışmıyor.
* VM yük devretme sonrasında çoğaltma çifti silme uzun bir süre yapılandırma sunucusu konsolunda ilerleme değil. Kullanıcılar yeniden çalışmayı tamamla veya işlemlerini sürdürebilirsiniz.
* Genel anlık görüntü işlemleri tutarlılık işi tarafından iyileştirilmiş, uygulama azaltmaya yardımcı olmak için SQL Server istemcileri gibi bağlantısını keser.
* Tutarlılık Aracı (VACP.exe) performansı geliştirilmiştir. Windows üzerinde anlık görüntü oluşturmak için gereken bellek kullanımını azaltılmıştır.
* Parola 16 karakterden daha büyük olduğunda göndermeli Yükleme Hizmeti kilitleniyor.
* vContinuum değil denetleyin ve kimlik bilgileri değiştirildiğinde, yeni vCenter bilgilerinizi sizden isteyebilir.
* Linux üzerinde ana hedef önbellek Yöneticisi'ni (cachemgr) işlem sunucusundan dosyaları indirme değil. Bu, çoğaltma çifti azaltma içinde sonuçlanır.
* Tüm düğümlerde aynı fiziksel yük devretme kümesi (MSCS) disk sırası olmadığı durumlarda, çoğaltma bazı küme birimleri için ayarlanmamış. Bu düzeltmeyi yararlanmak için küme reprotected gerekir.  
* RX Scout 8.0.1 Scout 7.1 yükseltildikten sonra SMTP işlevselliği beklendiği gibi çalışmıyor.
* Daha fazla istatistik tamamlamak için geçen süre izlemek için geri alma işlemi için günlüğünde eklenmiştir.
* Kaynak sunucuda Linux işletim sistemleri için destek eklenmiştir:
  * Red Hat Enterprise Linux (RHEL) 6 güncelleştirme 7
  * CentOS 6 7 güncelleştirmesi
* Yapılandırma sunucusu ve RX konsolları şimdi bit eşlem moduna girer çifti için bildirim göster.
* Aşağıdaki güvenlik düzeltmeleri RX eklenmiştir:
    * Yetkilendirme atlama parametre oynama aracılığıyla: geçerli olmayan kullanıcılar için kısıtlı erişim.
    * Siteler arası istek sahteciliğine: sayfa belirteci kavram uygulanmıştır ve her sayfa için rastgele oluşturur. Bu yalnızca bir tek oturum açma örneği için aynı kullanıcı yoktur ve sayfa yenileme işe yaramazsa anlamına gelir. Bunun yerine, Pano için yeniden yönlendirir.
    * Kötü amaçlı dosya karşıya yükleme: dosyaları için belirli uzantılara Yasak: z AIFF, asf, AVI, bmp, csv, belge, docx, fla, flv, GIF, gz, gzip, jpeg, jpg, günlük ortalarında, mov, mp3, mp4, mpc, mpeg, mpg, ods, odt, pdf, png, ppt, pptx, pxd, qt, ram, rar, rm, RMI, rmvb, rtf , sdc, sitd, swf, sxc, sxw, tar, tgz, TIF, TIFF, txt, vsd, wav, wma, wmv, xls, xlsx, xml ve zip.
    * Kalıcı siteler arası komut dosyası: Giriş doğrulamaları eklendi.

### <a name="azure-site-recovery-scout-801-update-2-update-03dec15"></a>Azure Site Recovery Scout 8.0.1 güncelleştirme 2 (03 Ara 15 güncelleştirmesi)

Güncelleştirme 2'deki düzeltmeler içerir:

* **Yapılandırma sunucusu**: 31 gün boş ölçüm engelledi sorunları yapılandırma sunucusu Site kurtarma için kaydolurken beklendiği gibi çalışmasını özelliği.
* **Birleşik aracı**: ana hedef sunucusunda 8.0 için 8.0.1 sürümünden yükseltme sırasında yüklenen değil güncelleştirme ile sonuçlandı güncelleştirme 1'deki bir sorun için düzeltme.

### <a name="azure-site-recovery-scout-801-update-1"></a>Azure Site Recovery Scout 8.0.1 güncelleştirme 1
Güncelleştirme 1 aşağıdaki hata düzeltmeleri ve yeni özellikler içerir:

* Sunucu örneği başına ücretsiz koruma 31 gün sayısı. Bu, işlevselliğini test etmek veya bir, kavram ayarlamak sağlar.
* Yük devretme ve yeniden çalışma, dahil olmak üzere sunucu üzerindeki tüm işlemler için ilk 31 gün ücretsizdir. Site Recovery Scout ile bir sunucu ilk korunurken süresini başlatır. 32nd günden müşteriye ait bir site için Site Recovery koruması için standart örnek oranı her korumalı sunucu ücretlendirilir.
* Herhangi bir zamanda şu anda ücretlendirilmeden korumalı sunucu sayısını edinilebilir **Pano** kasadaki.
* Destek vSphere komut satırı arabirimi (vCLI) 5.5 güncelleştirme 2 eklendi.
* Kaynak sunucuda bu Linux işletim sistemleri için destek eklendi:
    * RHEL 6 güncelleştirme 6
    * RHEL 5 güncelleştirme 11
    * CentOS 6 güncelleştirme 6
    * CentOS 5 güncelleştirme 11
* Aşağıdaki sorunların giderilmesine yönelik hata düzeltmeleri:
  * Yapılandırma sunucusu veya RX sunucusu için kasa kayıt başarısız olur.
  * Küme birimleri kümelenmiş olduğunda bunlar sürdürme gibi VM'ler korunmuş olarak beklenen görünmüyor.
  * Ana hedef sunucusu sanal makineleri şirket içi üretimden farklı ESXi sunucusunda barındırılıyorsa, yeniden çalışma başarısız olur.
  * Yapılandırma dosyası izinleri 8.0.1 için yükseltirken değiştirilir. Bu değişiklik, koruma ve işlemleri etkiler.
  * Yeniden eşitleme eşik, çoğaltma tutarsız davranışa neden beklendiği zorlanamaz.
  * RPO ayarlarını yapılandırma sunucusu konsolunda doğru görünmüyor. Sıkıştırılmamış veri değeri yanlış sıkıştırılmış değerini gösterir.
  * Kaldırma işlemi vContinuum Sihirbazı'nda beklendiği gibi silmez ve çoğaltma yapılandırması sunucu konsolundan silinmez.
  * Tıkladığınızda vContinuum Sihirbazı'nda diski otomatik olarak işaretli olmadığından **ayrıntıları** MSCS Vm'leri koruma sırasında disk görünümünde.
  * Fiziksel-sanal (P2V) senaryosunda, gerekli HP Hizmetleri (örneğin, CIMnotify ve CqMgHost) el ile VM kurtarma taşındı değil. Bu sorun, ek önyükleme zamanında oluşur.
  * Ana hedef sunucusunda birden fazla 26 diskleri olduğunda Linux VM koruma başarısız olur.

