---
title: Azure geçişi, Toplayıcı gerecini hakkında | Microsoft Docs
description: Azure geçişi, Toplayıcı gerecini hakkında bilgi sağlar.
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: snehaa
services: azure-migrate
ms.openlocfilehash: 224511b9748c540f2cd48a3d8393a9c74f76ce32
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58498426"
---
# <a name="about-the-collector-appliance"></a>Toplayıcı gerecini hakkında

 Bu makalede, Azure geçişi toplayıcısı hakkında bilgi sağlar.

Azure geçişi toplayıcısı bir şirket içi vCenter ortam ile değerlendirme amaçları için keşfetmek için kullanılan basit bir gereçtir [Azure geçişi](migrate-overview.md) service, azure'a geçiş işleminden önce.  

## <a name="discovery-method"></a>Bulma yöntemi

Daha önce Toplayıcı Gereci, tek seferlik bulma ve sürekli bulma için iki seçeneğiniz vardı. Tek seferlik model performans verilerini toplama (Düzey 3 ayarlamak için gerekli istatistik ayarları) için vCenter Server istatistik ayarları yararlandı gibi artık kullanım dışıdır ve ayrıca içinde sonuçlanan ortalama sayaçları (yerine yoğun) toplanan alt boyutlandırma. Sürekli bulma model ayrıntılı veri toplama sağlar ve en yüksek sayaçlarını toplamayı nedeniyle doğru boyutlandırma sonuçlanır. Şekli aşağıda verilmiştir:

Toplayıcı gerecini sürekli olarak Azure geçişi projesine bağlı olan ve sürekli olarak VM'lerin performans verilerini toplar.

- Toplayıcı, gerçek zamanlı kullanım verilerine her 20 saniyede toplamak için şirket içi ortamı sürekli olarak profilleri.
- Gereç 20 saniye örneklerini yapar ve her 15 dakikada bir tek veri noktası oluşturur.
- Verileri oluşturmak için Gereci noktası en yüksek değeri 20 saniye örnekleri seçer ve Azure'a gönderir.
- Bu model, performans verilerini toplamak için vCenter Server istatistik ayarları üzerinde bağımlı değildir.
- Sürekli, dilediğiniz zaman Toplayıcı profil oluşturma durdurabilirsiniz.

**Hızlı değerlendirmesi:** Bulma tamamlandıktan sonra sürekli bulma Gereci ile (birkaç VM sayısına bağlı olarak saat sürer), değerlendirmeler hemen oluşturabilirsiniz. Performans veri toplama başlar hızlı değerlendirmeleri için arıyorsanız, Keşif başlatmadan sonra değerlendirmede boyutlandırma ölçütü seçmelisiniz *şirket içi olarak*. Performans tabanlı değerlendirmeleri için en az bir gün sonra güvenilir boyut önerileri almak için keşif başlatılmadan için beklemeniz önerilir.

Gereç yalnızca performans verilerini sürekli olarak toplar, şirket içi ortamda (yani VM ekleme, silme, disk ekleme vb.) herhangi bir yapılandırma değişikliği algılamaz. Şirket içi ortamda bir yapılandırma değişikliği gerçekleşirse değişikliklerin portala yansıması için aşağıdakileri yapabilirsiniz:

- Ayrıca öğeleri (VM'ler, diskler ve çekirdek vb.): Azure portalında bu değişiklikleri yansıtacak şekilde gereç keşiften durdurun ve yeniden başlatın. Bu, değişikliklerin Azure Geçişi projesinde güncelleştirilmesini sağlar.

- VM silme: Bulma durdurup bile gereç tasarlandığı şekilde nedeniyle, VM'ler silinmesini yansıtılmaz. Bunun nedeni takip eden keşiflerin eski keşiflerin üzerine yazılması yerine bunlara eklenmesidir. Bu durumda grubunuzdan kaldırarak ve değerlendirmeyi yeniden hesaplayarak portaldaki VM’yi yoksayabilirsiniz.

> [!NOTE]
> Bu yöntem, vCenter Server'ın performans veri noktası kullanılabilirlik için istatistik ayarları yararlandı ve sanal makinelerin Azure'a geçiş için eksik boyutlandırma içinde sonuçlanan ortalama performans sayaçlarının toplanan gibi tek seferlik gereç artık kullanım dışı bırakılmıştır.

## <a name="deploying-the-collector"></a>Toplayıcı dağıtımı

Bir OVF şablonunu kullanarak Toplayıcı gerecini dağıtın:

- Azure portalında bir Azure geçişi projesi OVF şablonunu indirin. Vcenter Server VM Toplayıcı gerecini ayarlamak için indirilen dosyayı içeri aktarın.
- OVF 8 çekirdek, 16 GB RAM ve 80 GB'lık bir disk ile VM VMware ayarlar. Windows Server 2016 (64-bit) işletim sistemidir.
- Toplayıcı çalıştırdığınızda, Azure geçişi için toplayıcı bağlanabildiğinden emin olmak için bir dizi önkoşul denetimlerini çalıştırın.

- [Daha fazla bilgi edinin](tutorial-assessment-vmware.md#create-the-collector-vm) Toplayıcı oluşturma hakkında daha fazla.


## <a name="collector-prerequisites"></a>Toplayıcı önkoşulları

Toplayıcı sağlamak için Azure geçişi hizmetini internet üzerinden bağlanabilir ve verileri karşıya yükleme bulunan birkaç önkoşul denetimleri geçmesi gerekir.

- **Azure bulut doğrulayın**: Toplayıcı, geçirmeyi planlıyorsanız Azure bulut bilmek ister.
    - Azure kamu, Azure kamu bulutuna geçirmeyi planlıyorsanız seçin.
    - Azure ticari buluta geçiş yapmayı planlıyorsanız Azure genel seçin.
    - Gereç, burada belirtilen bulutta bağlı olarak, ilgili Uç noktalara bulunan meta verileri gönderir.
- **İnternet bağlantısı kontrol**: Toplayıcı, doğrudan İnternet'e veya bir ara sunucu aracılığıyla bağlanabilirsiniz.
    - Önkoşul denetimi bağlantıyı doğrular [gerekli ve isteğe bağlı URL'leri](#urls-for-connectivity).
    - İnternet'e doğrudan bir bağlantı varsa, belirli bir eylem, Toplayıcı gerekli URL'lere erişebildiğinden emin emin olma dışında gereklidir.
    - Ara sunucusu üzerinden bağlanıyorsanız, aşağıdaki gereksinimleri dikkate alın.
- **Zaman eşitleme doğrulayın**: Toplayıcı hizmet isteklerine kimlik doğrulaması sağlamak için internet saat sunucusuyla eşitlenmiş.
    - Böylece zaman doğrulanmış portal.azure.com url Toplayıcısından erişilebilir olmalıdır.
    - Makine eşitlenmemiş ise, saatin geçerli saati eşleştirilecek Toplayıcı VM üzerinde değiştirmeniz gerekir. Bunu yapmak için bir yönetici istemi VM'de çalıştırmak açın **w32tm /tz** saat dilimini denetlemek için. Çalıştırma **w32tm/resync** zaman eşitlenecek.
- **Toplayıcı hizmeti çalışıyor denetleyin**:  Azure geçişi Toplayıcısı hizmetinin Toplayıcı VM üzerinde çalışıyor olması gerekir.
    - Makinenin önyükleme işlemi sırasında bu hizmeti otomatik olarak başlatılır.
    - Hizmet çalışmıyorsa başlatın Denetim Masası'ndan.
    - Toplayıcı hizmetinin vCenter Server'a bağlanır, VM meta verileri ve performans verilerini toplar ve Azure geçişi hizmetine gönderir.
- **VMware Powerclı 6.5 yüklendi denetleyin**: VCenter Server ile iletişim kurabilmesi için toplayıcı VM üzerinde VMware Powerclı 6.5 PowerShell modülü yüklenmelidir.
    - Toplayıcı modülü yüklemek için gereken URL'leri erişebiliyorsa, yükleme otomatik olarak Toplayıcı dağıtımı sırasında olur.
    - Toplayıcı, dağıtım sırasında modülü yükleyemezse, bunu el ile yüklemeniz gerekir.
- **VCenter sunucusu bağlantısını denetleyin**: Toplayıcı, vCenter Server ve Vm'leri, meta verileri ve performans sayaçları için sorgu erişebilmelidir. [Önkoşulları doğrulama](#connect-to-vcenter-server) bağlanma.


### <a name="connect-to-the-internet-via-a-proxy"></a>Bir ara sunucu üzerinden İnternet'e bağlanın

- Proxy sunucusu kimlik doğrulaması gerektiriyorsa, Toplayıcı ayarladığınızda, kullanıcı adı ve parola belirtebilirsiniz.
- Proxy sunucusunun IP adresini/FQDN'yi olarak belirtilen *http:\//IPaddress* veya *http:\//FQDN*.
- Yalnızca HTTP proxy’si desteklenir. HTTPS tabanlı Ara sunucuları toplayıcı tarafından desteklenmiyor.
- Araya giren bir proxy Web sunucusunda ise Toplayıcı VM proxy sertifikasını içeri aktarmanız gerekir.
  1. Toplayıcı sanal makinesi, Git **Başlat menüsü** > **bilgisayar sertifikalarını yönetme**.
  2. Sertifika Aracı'nda altında **sertifikalar - yerel bilgisayar**, bulma **Güvenilen Yayımcılar** > **sertifikaları**.

      ![Sertifika aracı](./media/concepts-intercepting-proxy/certificates-tool.png)

  3. Proxy sertifikası, Toplayıcı VM'yi kopyalayın. Ağ yöneticinizden alması gerekebilir
  4. Sertifika açmak için çift tıklatın ve **sertifikayı yükle**.
  5. Sertifika İçeri Aktarma Sihirbazı'nda > Store konumu seçin **yerel makine**.

     ![Sertifika depolama konumu](./media/concepts-intercepting-proxy/certificate-store-location.png)

  6. Seçin **tüm sertifikaları aşağıdaki depolama alanına yerleştir** > **Gözat** > **Güvenilen Yayımcılar**. Tıklayın **son** sertifikayı içeri aktarmak için.

     ![Sertifika deposu](./media/concepts-intercepting-proxy/certificate-store.png)

  7. Sertifika beklendiği gibi alınır ve internet bağlantısı Önkoşul denetimi works olarak beklenen denetleyin.


### <a name="urls-for-connectivity"></a>Bağlantı için URL'leri

Bağlantı denetimi URL'lerin bir listesini bağlanarak doğrulanır.

**URL** | **Ayrıntılar**  | **Önkoşul denetimi**
--- | --- | ---
*.portal.azure.com | Azure genel uygulanabilir. Zaman eşitleme ve Azure hizmet bağlantısını denetler. | Erişim URL'si gereklidir.<br/><br/> Bağlantı yoksa Önkoşul denetimi başarısız olur.
*.portal.azure.us | Yalnızca Azure devlet kurumları için geçerlidir. Zaman eşitleme ve Azure hizmet bağlantısını denetler. | Erişim URL'si gereklidir.<br/><br/> Bağlantı yoksa Önkoşul denetimi başarısız olur.
*.oneget.org:443<br/><br/> *.windows.net:443<br/><br/> *.windowsazure.com:443<br/><br/> *.powershellgallery.com:443<br/><br/> *.msecnd.net:443<br/><br/> *.visualstudio.com:443| VCenter Powerclı PowerShell modülünü yüklemek için kullanılır. | URL'lere erişim gereklidir.<br/><br/> Önkoşul denetimi başarısız olmaz.<br/><br/> Toplayıcı VM üzerinde otomatik Modül yükleme başarısız olur. Modül internet bağlantısı olan bir makinede el ile yükleyin ve ardından modülleri gerecine kopyalamak gerekir. [Bu sorun giderme Kılavuzu'nda #4. adıma giderek daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/troubleshooting-general#error-unhandledexception-internal-error-occurred-systemiofilenotfoundexception).


### <a name="install-vmware-powercli-module-manually"></a>VMware Powerclı modülünü el ile yükleme

1. Modülünü kullanarak yükleme [adımları](https://blogs.vmware.com/PowerCLI/2017/04/powercli-install-process-powershell-gallery.html). Çevrimiçi ve çevrimdışı yükleme adımları açıklanmaktadır.
2. Toplayıcı VM'yi çevrimdışı ve internet erişimi olan farklı bir makinede modülü yüklerseniz, Toplayıcı VM için bu makineden VMware.* dosyaları kopyalamanız gerekir.
3. Yükleme sonrasında Powerclı yüklendiğinden emin olmak için önkoşul denetimlerini yeniden başlatabilirsiniz.

### <a name="connect-to-vcenter-server"></a>VCenter Server'a bağlanma

Toplayıcı, vCenter Server'a bağlanır ve VM meta verileri ve performans sayaçları için sorgular. Bağlantı için ihtiyacınız olanlar aşağıda verilmiştir.

- Yalnızca vCenter Server 5.5, 6.0, 6.5 ve sürümleri 6.7 desteklenir.
- Bulma için aşağıda özetlenen izinleri ile salt okunur bir hesabınız olması gerekir. Bulma için veri merkezlerinden erişilebilir hesapla yalnızca erişilebilir.
- Varsayılan olarak bir FQDN veya IP adresine sahip vCenter Server'a bağlanın. VCenter sunucusu farklı bir bağlantı noktasında dinliyorsa, formu kullanarak bağlanma *IPAddress:Port_Number* veya *FQDN:Port_Number*.
- Toplayıcı, bir ağ görebilmesi için vCenter sunucusuna sahip olmalıdır.

#### <a name="account-permissions"></a>Hesap izinleri

**Hesap** | **İzinler**
--- | ---
En az bir salt okunur kullanıcı hesabı | Veri Merkezi nesnesi –> Alt Nesneye Yay, role=Read-only   


## <a name="collector-communications"></a>Toplayıcı iletişimleri

Toplayıcı, aşağıdaki diyagramda ve tabloda özetlendiği gibi iletişim kurar.

![Toplayıcı iletişim diyagramı](./media/tutorial-assessment-vmware/portdiagram.PNG)


**Toplayıcı ile iletişim kurar** | **Bağlantı Noktası** | **Ayrıntılar**
--- | --- | ---
Azure Geçişi hizmeti | TCP 443 | Toplayıcı SSL 443 üzerinden Azure geçişi hizmeti ile iletişim kurar.
vCenter Server | TCP 443 | Toplayıcı, vCenter Server ile iletişim kurabildiğini olmalıdır.<br/><br/> Varsayılan olarak 443 üzerinden vcenter bağlanır.<br/><br/> VCenter sunucusu farklı bir bağlantı noktasında dinliyorsa, bağlantı noktası Toplayıcı üzerinde giden bağlantı noktası olarak kullanılabilir olması gerekir.
RDP | TCP 3389 |

## <a name="collected-metadata"></a>Toplanan meta verileri

> [!NOTE]
> Toplayıcı gerecini Azure'a geçirirken, doğru boyuta uygulamalarınızı yardımcı olmak için kullanılan Azure geçişi tarafından bulunan meta verileri Azure uygunluk analizi, uygulama bağımlılık analizi ve maliyet planlaması gerçekleştirin. Microsoft, herhangi bir lisans uyumluluk denetimi ile ilgili olarak bu verileri kullanmaz.

Toplayıcı gerecini her VM için aşağıdaki yapılandırma meta verileri bulur. Sanal makineler için yapılandırma verilerini bulma başlattıktan sonra bir saat kullanılabilir.

- VM görünen adı (temel, vCenter sunucusu)
- Sanal makinenin envanteri yolu (konak/klasörü vCenter Server)
- IP adresi
- MAC adresi
- İşletim sistemi
- Çekirdek, disk, NIC sayısı
- Bellek boyutu, Disk boyutları
- VM, disk ve ağ performans sayaçları.

### <a name="performance-counters"></a>Performans sayaçları

 Toplayıcı gerecini 20 saniyelik bir aralıkta ESXi konağından her VM için aşağıdaki performans sayaçlarını toplar. Bu sayaçlardan vCenter sayaçları ve terminolojiyi ortalama diyor olsa da, 20 saniye örnekleri gerçek zamanlı sayaçları. VM'ler için performans verilerini iki saat sonra keşif devreye girdi portalda kullanılabilir hale gelmeden başlatır. İçin en az doğru doğru boyutlandırma önerilerini almak için Değerlendirmeler performans tabanlı oluşturmadan önce bir gün beklemeniz önerilir. Anında sonuç elde etmek için arıyorsanız, boyutlandırma ölçütü ile değerlendirmeler oluşturabilirsiniz *şirket içi olarak* hangi değil dikkate alınır doğru boyutlandırma için performans verileri.

**Counter** |  **Etki değerlendirmesi**
--- | ---
cpu.usage.average | Önerilen VM boyutu ve maliyet  
mem.Usage.average | Önerilen VM boyutu ve maliyet  
virtualDisk.read.average | Disk boyutu, depolama maliyeti, VM boyutunu hesaplar
virtualDisk.write.average | Disk boyutu, depolama maliyeti, VM boyutunu hesaplar
virtualDisk.numberReadAveraged.average | Disk boyutu, depolama maliyeti, VM boyutunu hesaplar
virtualDisk.numberWriteAveraged.average | Disk boyutu, depolama maliyeti, VM boyutunu hesaplar
NET.Received.average | VM boyutunu hesaplar                          
NET.transmitted.average | VM boyutunu hesaplar     

Azure geçişi tarafından toplanan VMware sayaçların tam listesi aşağıda verilmiştir:

**Kategori** |  **Meta verileri** | **vCenter datapoint**
--- | --- | ---
Makine ayrıntıları | VM Kimliği | VM. Config.InstanceUuid
Makine ayrıntıları | VM adı | VM. Config.Name
Makine ayrıntıları | vCenter sunucusu kimliği | VMwareClient.InstanceUuid
Makine ayrıntıları |  VM açıklaması |  VM. Summary.Config.Annotation
Makine ayrıntıları | Lisans ürün adı | VM. Client.ServiceContent.About.LicenseProductName
Makine ayrıntıları | İşletim sistemi türü | VM. Summary.Config.GuestFullName
Makine ayrıntıları | İşletim sistemi sürümü | VM. Summary.Config.GuestFullName
Makine ayrıntıları | Önyükleme türü | VM. Config.Firmware
Makine ayrıntıları | Çekirdek sayısı | vm.Config.Hardware.NumCPU
Makine ayrıntıları | Megabayt belleği | VM. Config.Hardware.MemoryMB
Makine ayrıntıları | Disk sayısı | VM. Config.Hardware.Device.ToList(). FindAll(x => x is VirtualDisk).count
Makine ayrıntıları | Disk boyutu listesi | VM. Config.Hardware.Device.ToList(). FindAll (x = > x VirtualDisk)
Makine ayrıntıları | Ağ bağdaştırıcılarının listesi | VM. Config.Hardware.Device.ToList(). FindAll (x = > x VirtualEthernetCard)
Makine ayrıntıları | CPU kullanımı | cpu.usage.average
Makine ayrıntıları | Bellek kullanımı | mem.Usage.average
Disk ayrıntıları (başına disk) | Disk anahtar değeri | disk. Anahtarı
Disk ayrıntıları (başına disk) | Disk birim sayısı | disk.UnitNumber
Disk ayrıntıları (başına disk) | Disk denetleyici anahtar değeri | disk. ControllerKey.Value
Disk ayrıntıları (başına disk) | Sağlanan gigabayt | virtualDisk.DeviceInfo.Summary
Disk ayrıntıları (başına disk) | Disk adı | Bu değeri, disk kullanarak oluşturulur. UnitNumber, disk. Anahtar ve disk. ControllerKey.Value
Disk ayrıntıları (başına disk) | Saniye başına okuma işlemleri sayısı | virtualDisk.numberReadAveraged.average
Disk ayrıntıları (başına disk) | Saniye başına yazma işlemlerinin sayısı | virtualDisk.numberWriteAveraged.average
Disk ayrıntıları (başına disk) | Saniye başına megabayt okuma aktarım hızı | virtualDisk.read.average
Disk ayrıntıları (başına disk) | Saniye başına megabayt hızının yazma | virtualDisk.write.average
Ağ bağdaştırıcısı ayrıntıları (NIC) başına | Ağ bağdaştırıcısı adı | NIC Anahtarı
Ağ bağdaştırıcısı ayrıntıları (NIC) başına | MAC Adresi | ((VirtualEthernetCard)nic).MacAddress
Ağ bağdaştırıcısı ayrıntıları (NIC) başına | IPv4 Adresleri | vm.Guest.Net
Ağ bağdaştırıcısı ayrıntıları (NIC) başına | IPv6 Adresleri | vm.Guest.Net
Ağ bağdaştırıcısı ayrıntıları (NIC) başına | Saniye başına megabayt okuma aktarım hızı | NET.Received.average
Ağ bağdaştırıcısı ayrıntıları (NIC) başına | Saniye başına megabayt hızının yazma | NET.transmitted.average
Envanteri yolu ayrıntıları | Ad | kapsayıcı. GetType(). Adı
Envanteri yolu ayrıntıları | Alt nesne türü | kapsayıcı. ChildType
Envanteri yolu ayrıntıları | Başvuru ayrıntıları | kapsayıcı. MoRef
Envanteri yolu ayrıntıları | Tam envanteri yolu | kapsayıcı. İle tam yol adı
Envanteri yolu ayrıntıları | Üst ayrıntıları | Container.Parent
Envanteri yolu ayrıntıları | Her VM için klasör ayrıntıları | ((Klasör) kapsayıcısı). ChildEntity.Type
Envanteri yolu ayrıntıları | Her bir sanal makine klasör için veri merkezi ayrıntıları | ((Veri merkezi) kapsayıcısı). VmFolder
Envanteri yolu ayrıntıları | Her bir ana klasör için veri merkezi ayrıntıları | ((Veri merkezi) kapsayıcısı). HostFolder
Envanteri yolu ayrıntıları | Her konak için küme ayrıntıları | ((ClusterComputeResource) kapsayıcısı). Ana bilgisayarı)
Envanteri yolu ayrıntıları | Her VM için ana bilgisayar ayrıntıları | ((HostSystem) kapsayıcısı). VM


## <a name="securing-the-collector-appliance"></a>Toplayıcı gerecini güvenliğini sağlama

Toplayıcı gerecini güvenliğini sağlamak için aşağıdaki adımları öneriyoruz:

- Paylaşma veya yönetici parolaları yetkisiz kişilerle misplace kullanmayın.
- Gereç kullanımda olmadığında kapatın.
- Güvenli bir ağ Gereci yerleştirin.
- Geçiş tamamlandıktan sonra gereç örneğini silin.
- Ayrıca, diskler üzerinde önbelleğe vCenter kimlik bilgilerini olabilir geçişten sonra da disk yedekleme dosyalarının (Vmdk) silin.

## <a name="os-license-in-the-collector-vm"></a>Toplayıcı VM'nin işletim sistemi lisans

Toplayıcı, 180 gün için geçerli olan bir Windows Server 2012 R2 değerlendirme lisansı ile birlikte gelir. Toplayıcı sanal makinesi için değerlendirme süresi sona erecek, yeni OVA indirip yeni bir gereç oluşturmanız önerilir.

## <a name="updating-the-os-of-the-collector-vm"></a>İşletim sistemini, Toplayıcı VM güncelleştiriliyor

Toplayıcı gerecini 180 gün boyunca bir değerlendirme lisansına sahip olsa da, sürekli olarak otomatik kapatma aşağı gereç önlemek için işletim sistemi gereçte güncelleştirmeniz gerekiyor.

- Toplayıcı 60 gün boyunca güncelleştirilmemesi durumunda otomatik olarak makinesi kapatılıyor başlatır.
- Bulma çalışıyorsa, 60 gün geçtiğinde bile makine kapatılmış gerekmez. Bulma tamamlandıktan sonra makine kapatılır.
- 60 günden fazla Toplayıcı kullandıysanız, çalışan Windows update tarafından her zaman güncelleştirme makine tutma öneririz.

## <a name="upgrading-the-collector-appliance-version"></a>Toplayıcı Gereci sürümüne yükseltme

OVA yeniden indirmeden Toplayıcı en son sürüme yükseltebilirsiniz.

1. İndirme [en son listelenen yükseltme paketi](concepts-collector-upgrade.md)
2. İndirilen düzeltme güvenli olmasını sağlamak için yönetici komut penceresi açın ve karma ZIP dosyası oluşturmak için aşağıdaki komutu çalıştırın. Oluşturulan karma karşı belirli bir sürüm belirtilen karma değeri ile eşleşmesi gerekir:

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

    (örnek kullanım C:\>CertUtil - HashFile C:\AzureMigrate\CollectorUpdate_release_1.0.9.14.zip SHA256)
3. Azure geçişi toplayıcısı sanal makineye (Toplayıcı gerecini) zip dosyasını kopyalayın.
4. Zip dosyasını sağ tıklatın ve tümünü Ayıkla'ı seçin.
5. Setup.ps1 üzerinde sağ tıklayın ve PowerShell ile Çalıştır'ı seçin ve güncelleştirmeyi yüklemek için ekrandaki yönergeleri izleyin.

## <a name="discovery-process"></a>Bulma işlemi

Gereç ayarlandıktan sonra bulma çalıştırabilirsiniz. Nasıl çalıştığını şu şekildedir:

- Bir bulma kapsamı ile çalıştırın. Tüm sanal makineler belirtilen vCenter stok yolunda bulunur.
    - Aynı anda bir kapsam ayarladığınız.
    - Kapsam 1500 sanal makine içerebilir veya daha az.
    - Kapsam, bir veri merkezi, klasör ya da ESXi konağı olabilir.
- VCenter Server'ı bağladıktan sonra bir geçiş projesi koleksiyonu için belirterek bağlanın.
- VM'ler bulunduktan ve meta verileri ve performans verilerini Azure'a gönderilir. Bu Eylemler, bir toplama işi bir parçasıdır.
    - Toplayıcı gerecini bulmalar arasında belirli bir makine için kalıcı olan belirli bir Toplayıcı kimliği verilir.
    - Çalışan bir toplama işi belirli bir oturum kimliği verilir. Kimlik her toplama işine değiştirir ve sorun giderme için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

[Şirket içi VMware Vm'leri için bir değerlendirmenin ayarlayın](tutorial-assessment-vmware.md)
