---
title: Azure'a VMware vm'lerinin olağanüstü durum kurtarma için Azure Site Recovery dağıtım planlayıcısı hakkında | Microsoft Docs
description: Azure'a VMware vm'lerinin olağanüstü durum kurtarma için Azure Site Recovery dağıtım planlayıcısı hakkında bilgi edinin.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: mayg
ms.openlocfilehash: 195c70cd887d75eebb23cb7632b5fa250f89661c
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65137752"
---
# <a name="about-the-azure-site-recovery-deployment-planner-for-vmware-to-azure"></a>Azure Site Recovery dağıtım planlayıcısı hakkında vmware'den azure'a
Bu makale, VMware’den Azure’a üretim dağıtımları için Azure Site Recovery Dağıtım Planlayıcısı kullanım kılavuzudur.

## <a name="overview"></a>Genel Bakış

Azure Site Recovery kullanarak herhangi bir VMware sanal makinesini (VM) korumaya başlamadan önce, istenen kurtarma noktası hedefini (RPO) karşılayacak günlük veri değişikliği hızınıza göre yeterli bant genişliğini ayırmanız gerekir. Şirket içinde doğru sayıda yapılandırma sunucusu ve işlem sunucusu dağıttığınızdan emin olun.

Ayrıca doğru tür ve sayıda hedef Azure Depolama hesabı oluşturmanız gerekir. Zaman içinde artan kullanım nedeniyle kaynak üretim sunucularınızdaki büyümeyi hesaba katarak standart veya premium depolama hesapları oluşturun. İş yükü özelliklerine (örneğin, saniyede okuma/yazma G/Ç işlemi [IOPS] veya veri değişim sıklığı) ve Site Recovery limitlerine göre VM başına depolama türü seçin.

 Site Recovery Dağıtım Planlayıcısı, hem Hyper-V’den Azure’a hem de VMware’den Azure’a olağanüstü durum kurtarma senaryoları için bir komut satırı aracıdır. Başarılı çoğaltma ve yük devretme testine yönelik bant genişliği ve depolama gereksinimlerini anlamak için, bu aracı kullanarak VMware sanal makinelerinizin profilini uzaktan oluşturabilirsiniz (herhangi bir üretim etkisi olmadan). Şirket içinde herhangi bir Site Recovery bileşeni yüklemeden aracı çalıştırabilirsiniz. Ancak, elde edilen aktarım hızı sonuçlarını doğru şekilde almak için, planlayıcıyı üretim dağıtımının ilk adımlarından birinde dağıtmanız gereken Site Recovery yapılandırma sunucusunun en düşük gereksinimlerini karşılayan bir Windows Server üzerinde çalıştırın.

Araç aşağıdaki bilgileri sağlar:

**Uyumluluk değerlendirmesi**

* Disk sayısı, disk boyutu, IOPS, değişim sıklığı ve önyükleme türü (EFI/BIOS) ve işletim sistemine göre VM uygunluk değerlendirmesi

**Ağ bant genişliği ile RPO değerlendirmesi karşılaştırması**

* Delta çoğaltma için gereken tahmini ağ bant genişliği
* Site Recovery’nin şirket içinden Azure’a alabileceği aktarım hızı
* Belirli bir süre içinde ilk çoğaltmayı tamamlamak için tahmin edilen bant genişliğine göre toplu hale getirilecek VM sayısı
* Belirli bir bant genişliğinde ulaşılabilecek RPO
* Daha düşük bant genişliği sağlandığında istenen RPO üzerinde etki

**Azure altyapı gereksinimleri**

* Her VM için depolama türü (standart veya premium depolama hesabı) gereksinimi
* Çoğaltma için ayarlanacak toplam standart ve premium depolama hesabı sayısı
* Depolama kılavuzuna göre depolama hesabı adlandırma önerileri
* Tüm VM’ler için depolama hesabı yerleşimi
* Abonelik üzerinde yük devretme testi veya yük devretme öncesinde ayarlanacak Azure çekirdek sayısı
* Şirket içindeki her VM için önerilen Azure VM boyutu

**Şirket içi altyapı gereksinimleri**
* Şirket içinde dağıtılması gereken yapılandırma sunucusu ve işlem sunucusu sayısı

**Azure için tahmini olağanüstü durum kurtarma maliyeti**
* Azure için tahmini olağanüstü durum kurtarma toplam maliyeti: işlem, depolama, ağ ve Site Recovery lisans maliyeti
* VM başına ayrıntılı maliyet analizi


>[!IMPORTANT]
>
>Kullanım zaman içinde çoğalabileceğinden, önceki tüm araç hesaplamaları iş yükü özelliklerinde yüzde 30’luk bir büyüme olduğu varsayılarak yapılır. Hesaplamalarda ayrıca, okuma/yazma IOPS ve değişim sıklığı gibi tüm profil oluşturma ölçümlerinin yüzde 95'lik dilim değeri kullanılır. Büyüme faktörü ve yüzdelik dilim hesaplamasının her ikisi de yapılandırılabilir. Büyüme faktörü hakkında daha fazla bilgi almak için "Büyüme faktörü ile ilgili dikkat edilecek noktalar" bölümüne bakın. Yüzdelik dilim değeri hakkında daha fazla bilgi için "Hesaplama için kullanılan yüzdelik dilim değeri" bölümüne bakın.
>

## <a name="support-matrix"></a>Destek matrisi

| | **Vmware’den Azure’a** |**Hyper-V'den Azure'a**|**Azure'dan Azure'a**|**Hyper-V’den ikincil siteye**|**VMware’den ikincil siteye**
--|--|--|--|--|--
Desteklenen senaryolar |Evet|Evet|Hayır|Evet*|Hayır
Desteklenen sürüm | vCenter 6.7, 6.5, 6.0 or 5.5| Windows Server 2016, Windows Server 2012 R2 | NA |Windows Server 2016, Windows Server 2012 R2|NA
Desteklenen yapılandırma|vCenter, ESXi| Hyper-V kümesi, Hyper-V konağı|NA|Hyper-V kümesi, Hyper-V konağı|NA|
Çalışan Site Recovery Dağıtım Planlayıcısı örneği başına profili oluşturulabilecek sunucu sayısı |Tek (bir vCenter Server ve bir ESXi sunucusuna ait VM’lerin profili aynı anda oluşturulabilir)|Birden çok (birden çok konak veya konak kümesindeki sanal makinelerin profili tek seferde oluşturulabilir)| NA |Birden çok (birden çok konak veya konak kümesindeki sanal makinelerin profili tek seferde oluşturulabilir)| NA

*Bu araç, öncelikli olarak Hyper-V’den Azure’a olağanüstü durum kurtarma senaryosuna yöneliktir. Hyper-V’den ikincil siteye olağanüstü durum kurtarma senaryosunda yalnızca gereken ağ bant genişliği, kaynak Hyper-V sunucuların her birinde gereken boş depolama alanı ve ilk çoğaltmadaki batch numaralarıyla batch açıklamaları gibi kaynak tarafı önerilerin anlaşılması için kullanılabilir. Rapordaki Azure önerilerini ve maliyetleri dikkate almayın. Ayrıca Aktarım Hızı Alma işlemi, Hyper-V’den ikincil siteye olağanüstü durum kurtarma senaryosu için kullanılamaz.

## <a name="prerequisites"></a>Önkoşullar
Araçta başlıca iki aşama vardır: profil oluşturma ve rapor oluşturma. Yalnızca aktarım hızını hesaplamaya yönelik üçüncü bir seçenek de mevcuttur. Profil oluşturma ve aktarım hızı ölçümünün başlatıldığı sunucuya yönelik gereksinimler, aşağıdaki tabloda sunulmuştur.

| Sunucu gereksinimi | Açıklama|
|---|---|
|Profil oluşturma ve aktarım hızı ölçümü| <ul><li>İşletim Sistemi: Windows Server 2016 veya Windows Server 2012 R2<br>(En azından [yapılandırma sunucusuna yönelik boyut önerileri](https://aka.ms/asr-v2a-on-prem-components) ile eşleşmesi idealdir)</li><li>Makine Yapılandırması: 8 Vcpu, 16 GB RAM, 300 GB HDD</li><li>[.NET Framework 4.5](https://aka.ms/dotnet-framework-45)</li><li>[VMware vSphere PowerCLI 6.0 R3](https://aka.ms/download_powercli)</li><li>[Visual Studio 2012 için Visual C++ Yeniden Dağıtılabilir](https://aka.ms/vcplusplus-redistributable)</li><li>Bu sunucudan Azure’a İnternet erişimi</li><li>Azure depolama hesabı</li><li>Sunucu üzerinde yönetici erişimi</li><li>En az 100 GB boş disk alanı (30 günlük profili oluşturulmuş ve her biri ortalama üç diske sahip 1.000 VM varsayıldığında)</li><li>VMware vCenter istatistik düzeyi ayarları 1 veya daha yüksek düzey olabilir.</li><li>VCenter bağlantı noktası (varsayılan 443) izin ver: Site Recovery dağıtım Planlayıcısı, vCenter sunucusu/ESXi konağına bağlanmak için bu bağlantı noktasını kullanır.</ul></ul>|
| Rapor oluşturma | Bir Windows PC veya Windows Server'ın Excel 2013 veya üzeri.<li>[.NET Framework 4.5](https://aka.ms/dotnet-framework-45)</li><li>[Visual Studio 2012 için Visual C++ Yeniden Dağıtılabilir](https://aka.ms/vcplusplus-redistributable)</li><li>[VMware vSphere Powerclı 6.0 R3](https://aka.ms/download_powercli) yalnızca geçirdiğinizde - sanal makinelerin en son VM yapılandırma bilgilerini getirmek için rapor oluşturma komutuna kullanıcı seçeneği gereklidir. Dağıtım Planlayıcısı, vCenter sunucusuna bağlanır. VCenter vCenter sunucusuna bağlanmak bağlantı noktası (varsayılan olarak 443) bağlantı noktasına izin verin.</li>|
| Kullanıcı izinleri | Profil oluşturma sırasında VMware vCenter sunucusuna/VMware vSphere ESXi ana bilgisayarına erişmek için kullanılan kullanıcı hesabına yönelik salt okunur izin |

> [!NOTE]
>
>Araç yalnızca VMDK ve RDM disklerine sahip VM’lerin profilini oluşturabilir. iSCSI veya NFS diskleri olan VM’lerin profilini oluşturamaz. Site Recovery, VMware sunucuları için iSCSI ve NFS diskleri destekler. Dağıtım planlayıcısının konuk içinde bulunmaması ve yalnızca vCenter performans sayaçlarını kullanarak profil oluşturması nedeniyle araç bu disk türlerini görüntüleyemez.
>

## <a name="download-and-extract-the-deployment-planner-tool"></a>Dağıtım planlayıcısı aracını indirme ve ayıklama
1. [Site Recovery Dağıtım Planlayıcısı](https://aka.ms/asr-deployment-planner)’nın en son sürümünü indirin.
Araç bir zip klasöründe paketlenmiştir. Aracın geçerli sürümü yalnızca VMware’den Azure’a senaryosunu destekler.

2. Zip klasörünü, aracı çalıştırmak istediğiniz Windows sunucusuna kopyalayın.
Sunucu, profili oluşturulacak VM’leri tutan vCenter sunucusu/vSphere ESXi ana bilgisayarına bağlanmak için ağ erişimine sahipse, aracı Windows Server 2012 R2’den çalıştırabilirsiniz. Ancak, aracı donanım yapılandırması [yapılandırma sunucusu boyutlandırma yönergeleri](https://aka.ms/asr-v2a-on-prem-components) ile eşleşen bir sunucu üzerinde çalıştırmanız önerilir. Site Recovery bileşenlerini şirket içinde zaten dağıttıysanız, aracı yapılandırma sunucusundan çalıştırın.

    Aracı çalıştırdığınız sunucuda, yapılandırma sunucusu (yerleşik bir işlem sunucusuna sahiptir) ile aynı donanım yapılandırmasına sahip olmanız önerilir. Bu tür bir yapılandırma, araç tarafından elde edildiği rapor edilen aktarım hızının, Site Recovery tarafından profil oluşturma sırasında elde edilebilecek gerçek aktarım hızı ile eşleşmesini sağlar. Aktarım hızı hesaplaması, sunucu üzerinde kullanılabilir ağ bant genişliğine ve sunucunun donanım yapılandırmasına (CPU, depolama vb.) bağlıdır. Aracı başka bir sunucudan çalıştırırsanız, bu sunucudan Microsoft Azure’a aktarım hızı hesaplanır. Ayrıca, sunucunun donanım yapılandırması, yapılandırma sunucusundan farklı olabileceği için, aracın elde edildiğini rapor ettiği aktarım hızı hatalı olabilir.

3. .zip klasörünü ayıklayın.
Klasör birden fazla dosya ve alt klasör içerir. Yürütülebilir dosya, üst klasördeki ASRDeploymentPlanner.exe dosyasıdır.

    Örnek: .zip dosyasını E:\ sürücüsüne kopyalayıp ayıklayın.
    E:\ASR Deployment Planner_v2.3.zip

    E:\ASR Deployment Planner_v2.3\ASRDeploymentPlanner.exe

### <a name="update-to-the-latest-version-of-deployment-planner"></a>Dağıtım Planlayıcısı’nı en son sürüme güncelleştirme

Dağıtım Planlayıcısı en son güncelleştirmeleri özetlenmiştir [sürüm geçmişi](site-recovery-deployment-planner-history.md).

Dağıtım Planlayıcısı’nın önceki sürümüne sahipseniz şunlardan birini yapın:
 * En son sürüm bir profil oluşturma düzeltmesi içermiyor ve profil oluşturma planlayıcının geçerli sürümünde devam ediyorsa, profil oluşturmaya devam edin.
 * En son sürüm bir profil oluşturma düzeltmesi içeriyorsa, geçerli sürümünüzde profil oluşturmayı durdurmanız ve profil oluşturma işlemini yeni sürümle yeniden başlatmanız önerilir.


 >[!NOTE]
 >
 >Yeni sürümle profil oluşturma işlemini başlattığınızda, aracın profil verilerini mevcut dosyalara iliştirebilmesi için aynı çıktı dizini yolunu geçirin. Raporu oluşturmak için profili oluşturulmuş verilerin tamamı kullanılır. Farklı bir çıktı dizininden geçerseniz, yeni dosyalar oluşturulur ve profili oluşturulmuş eski veriler rapor oluşturma işleminde kullanılamaz.
 >
 >Her yeni Dağıtım Planlayıcısı, .zip dosyasının toplu bir güncelleştirmesidir. En yeni dosyaları önceki klasöre kopyalamanız gerekmez. Yeni bir klasör oluşturup kullanabilirsiniz.


## <a name="version-history"></a>Sürüm geçmişi
En son Site Recovery dağıtım planlayıcısı aracı sürümü 2.4 ' dir.
Her güncelleştirmede eklenen düzeltmeler için [Site Recovery Dağıtım Planlayıcısı sürüm geçmişi](https://social.technet.microsoft.com/wiki/contents/articles/51049.asr-deployment-planner-version-history.aspx) sayfasına bakın.

## <a name="next-steps"></a>Sonraki adımlar
[Site Recovery Dağıtım Planlayıcısı’nı çalıştırma](site-recovery-vmware-deployment-planner-run.md)
