---
title: "Olağanüstü durum kurtarma VMware Vm'lerini veya fiziksel sunucuları ikincil bir siteye ayarlama | Microsoft Docs"
description: "Bu makalede, Azure Site Recovery hizmeti ile ikincil bir siteye şirket içi VMware Vm'leri veya Windows/Linux fiziksel sunucuları çoğaltmak açıklanmaktadır."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: b2cba944-d3b4-473c-8d97-9945c7eabf63
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/05/2017
ms.author: raynew
ms.openlocfilehash: 8cfaa56735c1f4e2e01b58fdde2ad0e77b388762
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="set-up-disaster-recovery-of-vmware-virtual-machines-or-physical-servers-to-a-secondary-site"></a>Olağanüstü durum kurtarma VMware sanal makineleri veya fiziksel sunucuları ikincil bir siteye ayarlama


Azure Site kurtarma Inmage Scout şirket içi VMware siteler arasında gerçek zamanlı çoğaltma sağlar. Inmage Scout Azure Site Recovery hizmeti Aboneliklerde dahil edilir.

Bir Azure aboneliğiniz yoksa [ücretsiz bir hesap oluşturma](https://azure.microsoft.com/pricing/free-trial/) başlamadan önce.


## <a name="create-a-vault"></a>Bir kasa oluşturun
1. [Azure Portal](https://portal.azure.com/) > **Kurtarma Hizmetleri**’nde oturum açın.
2. Yeni tıklayın > Yönetim > Yedekleme ve Site Kurtarma (OMS). Alternatif olarak, Gözat'ı tıklatabilirsiniz > Kurtarma Hizmetleri kasası > Ekle.
3. **Ad** alanında, kasayı tanımlamak için bir kolay ad belirleyin. Birden fazla aboneliğiniz varsa bunlardan birini seçin.
4. İçinde **kaynak grubu** yeni bir kaynak grubu oluşturun veya varolan bir tanesini seçin. Gerekli alanları doldurun yapılacak Azure bölgesini belirtin.
5. İçinde **konumu**, kasa için coğrafi bölgeyi seçin. Desteklenen bölgeleri kontrol etmek için bkz: [Azure Site Recovery fiyatlandırma](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Hızlı bir şekilde erişmek istiyorsanız Pano kasadan panoya PIN'ı tıklatın ve Oluştur'u tıklatın.
7. Yeni kasa panosunda görünür > tüm kaynakları ve ana kurtarma Hizmetleri kasaları sayfası.

## <a name="configure-the-vault-and-download-inmage-scout-components"></a>Kasa yapılandırmak ve Inmage Scout bileşenlerini yükle
1. Kurtarma Hizmetleri kasaları sayfasında kasanızı seçin ve tıklayın **ayarları**.
2. İçinde **ayarları** > **Başlarken** tıklatın **Site Recovery** > 1. adım: **altyapıyı hazırlama**  >  **Koruma hedefi**.
3. İçinde **koruma hedefi** kurtarma sitesini seçin ve VMware vSphere hiper yönetici Evet'i seçin. Daha sonra Tamam'a tıklayın.
4. İçinde **Scout Kurulum**, karşıdan yüklemeyi Inmage Scout 8.0.1 GA yazılım ve kayıt anahtar'ı tıklatın. İndirilen .zip dosyasında gerekli tüm bileşenleri için Kurulum dosyalarıdır.

## <a name="step-3-install-component-updates"></a>3. adım: bileşen güncelleştirmeleri yükleme
Hakkında en son okuma [güncelleştirmeleri](#updates). Güncelleştirme dosyaları sunucularda aşağıdaki sırayla yükleyeceksiniz:

1. RX sunucu varsa
2. Yapılandırma sunucuları
3. İşlem sunucuları
4. Ana hedef sunucusu
5. vContinuum sunucuları
6. Kaynak sunucu (Windows ve Linux sunucusu)

Güncelleştirmeleri gibi yükleyin:

1. Karşıdan [güncelleştirme](https://aka.ms/asr-scout-update5) .zip dosyası. Bu .zip dosyası aşağıdaki dosyaları içerir:

   * RX_8.0.4.0_GA_Update_4_8725872_16Sep16.Tar.gz
   * CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe
   * UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe
   * UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
   * vCon_Windows_8.0.5.0_GA_Update_5_11525767_20Apr17.exe
   * UA update4 BITS RHEL5, OL5, OL6, SUSE 10, SUSE 11: UA_<Linux OS>_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
2. .Zip dosyalarını ayıklayın.<br>
3. **RX sunucusu**: kopyalama **RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz** RX sunucusuna ve ayıklayın. Ayıklanan klasöründe Çalıştır **/yüklemesi**.<br>
4. **Yapılandırma sunucusu/işlem sunucusu için**: kopyalama **CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe** yapılandırma sunucusu ve işlem sunucusu. Çalıştırmak için çift tıklayın.<br>
5. **Windows ana hedef sunucusu için**: Birleşik aracısını güncelleştirmek için kopyalama **UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe** ana hedef sunucusu için. Çalıştırmak için çift tıklayın. Kaynak Update4 güncelleştirilmezse birleşik aracı da kaynak sunucu için geçerli olduğunu unutmayın. Bu yanı, kaynak sunucuda daha sonra bu listede belirtildiği gibi yüklemeniz gerekir.<br>
6. **VContinuum sunucusu**: kopyalama **vCon_Windows_8.0.5.0_GA_Update_5_11525767_20Apr17.exe** vContinuum sunucusu için.  VContinuum Sihirbazı'nı kapattığınız emin olun. Çalıştırmak için dosyaya çift tıklayın.<br>
7. **Linux ana hedef sunucusu için**: Birleşik aracısını güncelleştirmek için kopyalama **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** ana hedef sunucusunda ve ayıklayın. Ayıklanan klasöründe Çalıştır **/yüklemesi**.<br>
8. **Windows kaynak sunucu için**: güncelleştirme 4 zaten çalışıyorsa ve kaynak güncelleştirme 5 aracısı yüklemeniz gerekmez. Güncelleştirme 4 değerinden çalışıyorsa, güncelleştirme 5 Aracısı uygulayın.
Birleşik aracısını güncelleştirmek için kopyalama **UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe** kaynak sunucuda. Çalıştırmak için çift tıklayın. <br>
9. **Linux kaynak sunucu için**: Birleşik aracısını güncelleştirmek için karşılık gelen sürüm UA dosyasının Linux sunucuya kopyalayın ve ayıklayın. Ayıklanan klasöründe Çalıştır **/yüklemesi**.  Örnek: RHEL 6,7 64 bit sunucu için kopyalama **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** sunucuya ve ayıklayın. Ayıklanan klasöründe Çalıştır **/yüklemesi**.

## <a name="step-4-set-up-replication"></a>4. adım: Çoğaltmayı ayarlama
1. VMware siteleri hedef ve kaynak arasında çoğaltmayı ayarlayın.
2. Yönergeler için ürünle birlikte indirilir Inmage Scout belgeleri kullanın. Alternatif olarak, aşağıdaki gibi belgelere erişebilir:

   * [Sürüm notları](https://aka.ms/asr-scout-release-notes)
   * [Uyumluluk matrisi](https://aka.ms/asr-scout-cm)
   * [Kullanıcı Kılavuzu](https://aka.ms/asr-scout-user-guide)
   * [RX Kullanıcı Kılavuzu](https://aka.ms/asr-scout-rx-user-guide)
   * [Hızlı Yükleme Kılavuzu](https://aka.ms/asr-scout-quick-install-guide)

## <a name="updates"></a>Güncelleştirmeler
### <a name="azure-site-recovery-scout-801-update-5"></a>Azure Site Recovery Scout 8.0.1 güncelleştirme 5
Scout güncelleştirme 5 birikmeli bir güncelleştirmedir. Update1 update4 ve aşağıdaki yeni hata düzeltmeleri ve geliştirmeleri kasa tüm düzeltmelerini sahiptir.
Site Recovery Scout update4 update5 için eklenen düzeltmeleri ana hedef ve vContinuum bileşenleri için özeldir. Tüm kaynak sunucularınız, ana hedef, yapılandırma sunucusu, işlem sunucusu ve RX Site Recovery Scout update4 üzerinde zaten varsa yalnızca ana hedef sunucusunda güncelleştirme 5 uygulanması gerekiyor. 

**Yeni platform desteği**
* SUSE Linux Enterprise Server 11 hizmet paketi 4(SP4)

> [!NOTE]
> SLES 11 SP4 64 bit **InMage_UA_8.0.1.0_SLES11-SP4-64_GA_13Apr2017_release.tar.gz** temel Scout GA paketi ile birlikte paketlenmiştir **InMage_Scout_Standard_8.0.1 GA.zip**. Bölümünde belirtildiği gibi portaldan Scout GA paketini indirin [1. adım](#step-1-create-a-vault).
>

**Hata düzeltmeleri ve geliştirmeleri**

* Windows Küme destek güvenilirliğini artırmak
    * Sabit-süre P2V MSCS bazıları hale küme disklerine RAW kurtarma işleminden sonra
    * Disk sırası uyuşmazlığı nedeniyle fixed-P2V MSCS küme kurtarma başarısız olur
    * Fixed-MSCS kümesindeki ile disk boyutu uyumsuzluğu işlem başarısız diskleri ekleme
    * RDM LUN'ları eşleme hazırlık denetimi ile fixed-kaynak MSCS küme boyutu doğrulama başarısız
    * Fixed-tek bir düğümün küme koruma SCSI uyumsuzluğu sorunu nedeniyle başarısız oluyor 
    * Hedef küme diskleri mevcut olup olmadığını Fixed-yeniden koruma P2V Windows Küme sunucusu başarısız olur. 
    
* Seçili MT aynı sunucuda ESXi, korumalı kaynak makinenin (iletme koruma sırasında) değilse, ardından vContinuum yanlış MT geri dönme Kurtarma sırasında seçer ve daha sonra kurtarma işlemi başarısız geri dönme koruma sırasında.

> [!NOTE]
> 
> * P2V küme düzeltmeleri, istemcinin Site Recovery Scout update5 ile korunan yalnızca bu fiziksel MSCS kümeler için geçerlidir. Eski güncelleştirmeleriyle zaten korumalı P2V MSCS kümesindeki Küme kullanılabilir kredi yönelik düzeltmeleri, 12 bölümünde açıklanan yükseltme adımları izlemeniz gerekir, yükseltme korumalı Scout güncelleştirme 5 P2V MSCS kümelerine [sürüm notları](https://aka.ms/asr-scout-release-notes) .
> 
> * Zamanında yeniden koruma varsa, disklerin aynı kümesini yalnızca her küme düğümleri üzerinde etkin başlangıçta korumalı oldukları gibi yeniden koruma fiziksel MSCS kümesindeki var olan hedef disklerin yeniden kullanabilirsiniz. Değil, daha sonra el ile adımlar varsa, 12 bölümünde belirtildiği gibi [sürüm notları](https://aka.ms/asr-scout-release-notes) hedef tarafı disklerin yeniden yeniden koruma sırasında kullanmak için doğru veri deposu yolu taşımak için. Yükseltme adımlarını uymadan P2V modunda MSCS küme koruyun, bunu yeni bir disk hedef ESXi sunucusunda oluşturur. Eski diskleri deposundan el ile silmeniz gerekiyor.
> 
> * SLES11 zaman kaynağı veya herhangi bir hizmet paketi sunucu SLES11 düzgün biçimde yeniden ardından biri el ile işaretleme **kök** CX kullanıcı Arabiriminde bildirilmez gibi yeniden eşitlemek için çoğaltma çiftleri disk. Yeniden eşitleme için kök disk işaretlemeyin veri bütünlüğü (dı) sorunları görebilirsiniz.
> 

### <a name="azure-site-recovery-scout-801-update-4"></a>Azure Site Recovery Scout 8.0.1 güncelleştirme 4
Scout güncelleştirme 4 birikmeli bir güncelleştirmedir. Update1 update3 ve aşağıdaki yeni hata düzeltmeleri ve geliştirmeleri kasa tüm düzeltmelerini sahiptir.

**Yeni platform desteği**

* VCenter/vSphere için 6.0, 6.1 ve 6.2 desteği eklendi
* Aşağıdaki Linux işletim sistemleri için destek eklenmiştir
  * Red Hat Enterprise Linux (RHEL) 7.0, 7.1 ve 7.2
  * CentOS 7.0, 7.1 ve 7.2
  * Red Hat Enterprise Linux (RHEL) 6,8
  * CentOS 6,8

> [!NOTE]
> RHEL/CentOS 7 64-bit **InMage_UA_8.0.1.0_RHEL7-64_GA_06Oct2016_release.tar.gz** temel Scout GA paketi ile birlikte paketlenmiştir **InMage_Scout_Standard_8.0.1 GA.zip**. Bölümünde belirtildiği gibi portaldan Scout GA paketini indirin [1. adım](#step-1-create-a-vault).
>
>

**Hata düzeltmeleri ve geliştirmeleri**

* Geliştirilmiş kapatma yeniden eşitleme istenmeyen sorunları önlemek aşağıdaki Linux işletim sistemleri ve kopyalar için işleme.
  * Red Hat Enterprise Linux (RHEL) 6.x
  * Oracle Linux (OL) 6.x
* Linux için klasör erişim artık yalnızca yerel kullanıcı kısıtlı izinleri birleşik aracı yükleme dizini içinde tamamlayın.
* Windows üzerinde yerel olarak ortak dağıtılmış tutarlılık işareti yoğun olarak sertifika veren çalışırken zaman aşımına sorunu SQL ve Share Point kümeleri gibi dağıtılmış uygulamalar yüklendi.
* Eklenen günlük CX temel yükleyici düzeltme ilgili.
* VMware vCLI 6.0 indirme bağlantısı Windows ana hedef temel yükleyici eklenir.
* Daha fazla denetim ve ağ yapılandırmaları değişiklikleri günlüklerinde yük devretme ve DR ayrıntısına sırasında eklendi.
* Süre bekletme bilgileri CX bildirilmedi.  
* Kaynak birim küçültme oluştuğunda fiziksel küme için birim vContinuum Sihirbazı aracılığıyla yeniden boyutlandır işlemi başarısız olur.
* Küme koruma şu hatayla başarısız oldu "disk imzası bulmak başarısız oldu" Küme diski PRDM disk olduğunda.
* cxps aralık dışı özel durum nedeniyle sunucu kilitlenme taşıma.
* Sunucu adı ve IP sütunları şimdi anında yükleme sihirbazının sayfasında vContinuum yeniden boyutlandırılabilir.
* RX API'si geliştirmeleri
  * Beş en son kullanılabilir ortak tutarlılık noktası (yalnızca garanti etiketleri) sağlar.
  * Korunan tüm aygıtlar için kapasite ve boş alan ayrıntıları sağlar.
  * Kaynak sunucuda Scout sürücü durumunu sağlar.

> [!NOTE]
> * **InMage_Scout_Standard_8.0.1_GA.zip** temel paket şimdi güncelleştirdi CX temel yükleyici **InMage_CX_8.0.1.0_Windows_GA_26Feb2015_release.exe** ve Windows ana hedef temel yükleyici **InMage_ Scout_vContinuum_MT_8.0.1.0_Windows_GA_26Feb2015_release.exe**. Tüm yeni yükleme için yeni CX ve Windows ana hedef GA BITS hizmetini kullanın.
> * Güncelleştirme 4 8.0.1 üzerinde doğrudan da uygulanabilir İST
> * Sistemde uygulanmasıyla geri sonra RX güncelleştirmeleri ve yapılandırma sunucusu alınamaz.
>
>

### <a name="azure-site-recovery-scout-801-update-3"></a>Azure Site Recovery Scout 8.0.1 güncelleştirme 3
Güncelleştirme 3, aşağıdaki hata düzeltmeleri ve geliştirmeleri içerir:

* Yapılandırma sunucusu ve RX proxy'nin arkasında olduğunuzda Site Recovery Kasası'na kaydetmek başarısız.
* Kurtarma noktası hedefi (RPO) karşılanır kurmadı saat sayısını sistem durumu raporu güncelleştirilmez.
* Ağ ayrıntıları veya ESX donanım ayrıntıları tüm UTF-8 karakterler içeriyorsa yapılandırma sunucusu ile RX eşitlemiyor.
* Kurtarma sonrasında önyükleme yapmak Windows Server 2008 R2 etki alanı denetleyicileri başarısız.
* Çevrimdışı eşitleme beklendiği gibi çalışmıyor.
* Sanal makine (VM) yük devretme sonrasında çoğaltma çifti silme CX Arabiriminde uzun bir süredir takılmış ve kullanıcıların yeniden çalışmayı tamamla veya işlemini devam ettirmek.
* Genel Uygulama azaltmaya yardımcı olmak için tutarlılık işi tarafından yapılan işlemleri iyileştirilmiş anlık görüntü gibi SQL istemcilerinin bağlantısını keser.
* Windows üzerinde anlık görüntü oluşturmak için gerekli olan bellek kullanımı azaltarak tutarlılık Aracı (VACP.exe) performansı geliştirilmiştir.
* Parola 16 karakterden daha büyük olduğunda göndermeli Yükleme Hizmeti kilitleniyor.
* vContinuum değil denetleme ve kimlik bilgileri değiştiğinde yeni vCenter kimlik bilgileri.
* Linux üzerinde ana hedef önbellek Yöneticisi'ni (cachemgr) dosyaları çoğaltma çifti azaltma sonuçları işlem sunucusundan yüklüyor değil.
* Fiziksel yük devretme kümesi (MSCS) disk sırası tüm düğümlerde aynı olmadığında, çoğaltma bazı küme birimleri için ayarlanmadı.
  <br/>Küme bu düzeltmenin yararlanmak için korunmuş gerektiğini unutmayın.  
* SMTP işlevselliği RX Scout 8.0.1 Scout 7.1 yükseltildikten sonra beklendiği gibi çalışmıyor.
* Geri alma işlemini tamamlamak için geçen süreyi izlemek için günlüğünde daha fazla istatistikleri eklenmiştir.
* Kaynak sunucuda Linux işletim sistemleri için destek eklenmiştir:
  * Red Hat Enterprise Linux (RHEL) 6 güncelleştirme 7
  * CentOS 6 7 güncelleştirmesi
* Şimdi CX ve RX UI bit eşlem moduna girer çifti için bildirim gösterebilir.
* Aşağıdaki güvenlik düzeltmeleri RX eklenmiştir:

| **Sorun açıklaması** | **Uygulama yordamları** |
| --- | --- |
| Yetkilendirme atlama parametre oynama aracılığıyla |Geçerli olmayan kullanıcılar için kısıtlı erişim. |
| Siteler arası istek sahteciliğine |Her sayfa için rastgele oluşturur sayfa belirteci kavramı uygulanmadı. <br/>Bu, görürsünüz: <li> Yalnızca bir tek oturum açma örneği için aynı kullanıcı yoktur.</li><li>Sayfa yenileme çalışmıyor--panoya yönlendirir.</li> |
| Kötü amaçlı dosya karşıya yükleme |Kısıtlı dosyaları belirli uzantılara. Uzantıları izin verilir: 7z, AIFF, asf, AVI, bmp, csv, belge, docx, fla, flv, GIF, gz, gzip, jpeg, jpg, günlük ortalarında, mov, mp3, mp4, mpc, mpeg, mpg, ods, odt, pdf, png, ppt, pptx, pxd, qt, ram, rar, rm, RMI, rmvb, rtf, sdc, sitd, swf, sxc, sxw, tar , tgz, TIF, TIFF, txt, vsd, wav, wma, wmv, xls, xlsx, xml ve zip. |
| Kalıcı siteler arası komut dosyası |Giriş doğrulamaları eklendi. |

> [!NOTE]
> * Tüm Site Recovery Toplu güncelleştirmelerin. Güncelleştirme 3 güncelleştirme 1 ve güncelleştirme 2 tüm düzeltmeler sahiptir. Güncelleştirme 3 8.0.1 üzerinde doğrudan da uygulanabilir İST
> * Sistemde uygulanmasıyla geri sonra RX güncelleştirmeleri ve yapılandırma sunucusu alınamaz.
>
>

### <a name="azure-site-recovery-scout-801-update-2-update-03dec15"></a>Azure Site Recovery Scout 8.0.1 güncelleştirme 2 (03 Ara 15 güncelleştirmesi)
Güncelleştirme 2'deki düzeltmeler içerir:

* **Yapılandırma sunucusu**: 31 gün boş ölçüm özelliğini yapılandırma sunucusu Site kurtarma için kaydolurken beklendiği gibi çalışmasını engelleyen bir sorun için düzeltme.
* **Birleşik aracı**: 8.0 için 8.0.1 sürümünden yükselttiğinizde ana hedef sunucuda yüklü değil güncelleştirme ile sonuçlandı güncelleştirme 1'deki bir sorun için düzeltme.

### <a name="azure-site-recovery-scout-801-update-1"></a>Azure Site Recovery Scout 8.0.1 güncelleştirme 1
Güncelleştirme 1 aşağıdaki hata düzeltmeleri ve yeni özellikler içerir:

* Sunucu örneği başına ücretsiz koruma 31 gün sayısı. Bu, işlevselliğini test etmek veya bir, kavram ayarlamak sağlar.
  * Yük devretme ve yeniden çalışma, dahil olmak üzere sunucu üzerindeki tüm işlemler bir sunucu ilk Site Recovery Scout ile korunan zamandan itibaren ilk 31 gün için boş değildir.
  * 32nd günden başlayarak, her korumalı sunucu müşteriye ait bir site için Azure Site Recovery koruması için standart örnek oranı ücretlendirilir.
  * Herhangi bir zamanda şu anda ücretlendirilmeden korumalı sunucu sayısını Azure Site Recovery kasası panosu sayfasında kullanılabilir.
* 5.5 güncelleştirme 2 vSphere komut satırı arabirimi (vCLI) için ek destek.
* Linux işletim sistemleri için kaynak sunucuda eklenen desteği:
  * RHEL 6 güncelleştirme 6
  * RHEL 5 güncelleştirme 11
  * CentOS 6 güncelleştirme 6
  * CentOS 5 güncelleştirme 11
* Aşağıdaki sorunların giderilmesine yönelik hata düzeltmeleri:
  * Yapılandırma sunucusu veya RX sunucusu için kasa kayıt başarısız olur.
  * Küme birimleri, bunlar sürdürdüğünüzde kümelenmiş sanal makineler korunmuş beklendiği gibi görünmüyor.
  * Ana hedef sunucusu, şirket içi üretim sanal makinelerden farklı ESXi sunucusunda barındırılıyorsa, yeniden çalışma başarısız olur.
  * Yapılandırma dosyası izinleri, koruma ve işlemleri etkiler 8.0.1 için yükseltirken değiştirilir.
  * Yeniden eşitleme eşik beklendiği gibi çoğaltma tutarsız davranışa yol açar zorlanamaz.
  * RPO ayarlarını yapılandırma sunucusu arabiriminde doğru görünmüyor. Sıkıştırılmamış veri değeri yanlış sıkıştırılmış değerini gösterir.
  * Kaldırma işlemi vContinuum Sihirbazı'nda beklendiği gibi silmez ve çoğaltma yapılandırma sunucusu arabiriminden silinmez.
  * Tıkladığınızda vContinuum Sihirbazı'nda diski otomatik olarak işaretli olmadığından **ayrıntıları** MSCS sanal makinelerin korumasını sırasında disk görünümünde.
  * Fiziksel-sanal (P2V) senaryosu sırasında CIMnotify ve CqMgHost, gibi gerekli HP Hizmetleri, sanal makine kurtarması kılavuzda taşındı değil. Bu ek önyükleme zamanında sonuçlanır.
  * Ana hedef sunucusunda birden fazla 26 diskleri olduğunda Linux sanal makine koruması başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar
Sahip herhangi bir sorunuz sonrası [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).
