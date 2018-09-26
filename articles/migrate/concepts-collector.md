---
title: Azure geçişi, Toplayıcı gerecini hakkında | Microsoft Docs
description: Azure geçişi, Toplayıcı gerecini hakkında bilgi sağlar.
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 09/25/2018
ms.author: snehaa
services: azure-migrate
ms.openlocfilehash: 88bc0bdc29d1f578bd0d314c5c7425026dfd2d22
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47160887"
---
# <a name="about-the-collector-appliance"></a>Toplayıcı gerecini hakkında

 Bu makalede, Azure geçişi toplayıcısı hakkında bilgi sağlar.

Azure geçişi toplayıcısı bir şirket içi vCenter ortam ile değerlendirme amaçları için keşfetmek için kullanılan basit bir gereçtir [Azure geçişi](migrate-overview.md) service, azure'a geçiş işleminden önce.  


## <a name="deploying-the-collector"></a>Toplayıcı dağıtımı

Bir OVF şablonunu kullanarak Toplayıcı gerecini dağıtın:

- Azure portalında bir Azure geçişi projesi OVF şablonunu indirin. Vcenter Server VM Toplayıcı gerecini ayarlamak için indirilen dosyayı içeri aktarın.
- OVF 4 çekirdek, 8 GB RAM ve 80 GB'lık bir disk ile VM VMware ayarlar. Windows Server 2012 R2 (64-bit) işletim sistemidir.
- Toplayıcı çalıştırdığınızda, Azure geçişi için toplayıcı bağlanabildiğinden emin olmak için bir dizi önkoşul denetimlerini çalıştırın.
 
- [Daha fazla bilgi edinin](tutorial-assessment-vmware.md#create-the-collector-vm) Toplayıcı oluşturma hakkında daha fazla. 


## <a name="collector-prerequisites"></a>Toplayıcı önkoşulları

Toplayıcı sağlamak için Azure geçişi hizmetini internet üzerinden bağlanabilir ve verileri karşıya yükleme bulunan birkaç önkoşul denetimleri geçmesi gerekir. 

- **İnternet bağlantısı kontrol**: Toplayıcı doğrudan İnternet'e veya bir ara sunucu aracılığıyla bağlanabilirsiniz.
    - Önkoşul denetimi bağlantıyı doğrular [gerekli ve isteğe bağlı URL'leri](#connect-to-urls).
    - İnternet'e doğrudan bir bağlantı varsa, belirli bir eylem, Toplayıcı gerekli URL'lere erişebildiğinden emin emin olma dışında gereklidir.
    - Bir ara sunucu bağlanıyorsanız, Not [aşağıdaki gereksinimleri](#connect-via-a-proxy).
- **Zaman eşitleme doğrulayın**: Toplayıcı hizmetine yapılan istekler yetkilendirilmesini sağlamak için internet saat sunucusuyla eşitlenmiş.
    - Böylece zaman doğrulanmış portal.azure.com url Toplayıcısından erişilebilir olmalıdır.
    - Makine eşitlenmemiş ise, saatin geçerli saati eşleştirilecek Toplayıcı VM üzerinde değiştirmeniz gerekir. Bunu yapmak için bir yönetici istemi VM'de çalıştırmak açın **w32tm /tz** saat dilimini denetlemek için. Çalıştırma **w32tm/resync** zaman eşitlenecek.
- **Toplayıcı hizmeti çalışıyor denetleyin**: Azure geçişi Toplayıcısı hizmeti, Toplayıcı VM üzerinde çalıştırıyor olması gerekir.
    - Makinenin önyükleme işlemi sırasında bu hizmeti otomatik olarak başlatılır.
    - Hizmet çalışmıyorsa başlatın Denetim Masası'ndan.
    - Toplayıcı hizmetinin vCenter Server'a bağlanır, VM meta verileri ve performans verilerini toplar ve Azure geçişi hizmetine gönderir.
- **VMware Powerclı 6.5 yüklendi denetleyin**: VMware Powerclı 6.5 PowerShell modülü, vCenter Server ile iletişim kurabilmesi için toplayıcı VM üzerinde yüklenmelidir.
    - Toplayıcı modülü yüklemek için gereken URL'leri erişebiliyorsa, yükleme otomatik olarak Toplayıcı dağıtımı sırasında olur.
    - Toplayıcı, dağıtım sırasında modülü yükleyemezse, şunları yapmalısınız [el ile yükleyin](#install-vwware-powercli-module-manually).
- **VCenter sunucusu bağlantısını denetleyin**: toplayıcı için vCenter Server ve Vm'leri, meta verileri ve performans sayaçları için bir sorgu olmalıdır. [Önkoşulları doğrulama](#connect-to-vcenter-server) bağlanma.


### <a name="connect-to-the-internet-via-a-proxy"></a>Bir ara sunucu üzerinden İnternet'e bağlanın

- Proxy sunucusu kimlik doğrulaması gerektiriyorsa, Toplayıcı ayarladığınızda, kullanıcı adı ve parola belirtebilirsiniz.
- Proxy sunucusunun IP adresini/FQDN'yi olarak belirtilen *http://IPaddress* veya *http://FQDN*.
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

    


### <a name="connect-to-urls"></a>URL'lerle

Bağlantı denetimi URL'lerin bir listesini bağlanarak doğrulanır.

**URL** | **Ayrıntılar**  | **Önkoşul denetimi**
--- | --- | ---
*.portal.azure.com | Zaman eşitleme ve Azure hizmet bağlantısını denetler. | Erişim URL'si gereklidir.<br/><br/> Bağlantı yoksa Önkoşul denetimi başarısız olur.
*.oneget.org:443<br/><br/> *.windows.net:443<br/><br/> *.windowsazure.com:443<br/><br/> *. powershellgallery.com:443<br/><br/> *.msecnd.net:443<br/><br/> *.visualstudio.com:443| VCenter Powerclı PowerShell modülünü yüklemek için kullanılır. | İsteğe bağlı URL'lere erişim.<br/><br/> Önkoşul denetimi başarısız olmaz.<br/><br/> Toplayıcı VM üzerinde otomatik Modül yükleme başarısız olur. Modül el ile yüklemeniz gerekir.
 

### <a name="install-vmware-powercli-module-manually"></a>VMware Powerclı modülünü el ile yükleme

1. Modülünü kullanarak yükleme [adımları](https://blogs.vmware.com/PowerCLI/2017/04/powercli-install-process-powershell-gallery.html). Çevrimiçi ve çevrimdışı yükleme adımları açıklanmaktadır.
2. Toplayıcı VM'yi çevrimdışı ve internet erişimi olan farklı bir makinede modülü yüklerseniz, Toplayıcı VM için bu makineden VMware.* dosyaları kopyalamanız gerekir.
3. Yükleme sonrasında Powerclı yüklendiğinden emin olmak için önkoşul denetimlerini yeniden başlatabilirsiniz.

### <a name="connect-to-vcenter-server"></a>VCenter Server'a bağlanma

Toplayıcı, vCenter Server'a bağlanır ve VM meta verileri ve performans sayaçları için sorgular. Bağlantı için ihtiyacınız olanlar aşağıda verilmiştir.

- Yalnızca vCenter Server sürümleri 5.5, 6.0 ve 6.5 desteklenir.
- Bulma için aşağıda özetlenen izinleri ile salt okunur bir hesabınız olması gerekir. Bulma için veri merkezlerinden erişilebilir hesapla yalnızca erişilebilir.
- Varsayılan olarak bir FQDN veya IP adresine sahip vCenter Server'a bağlanın. VCenter sunucusu farklı bir bağlantı noktasında dinliyorsa, formu kullanarak bağlanma *IPAddress:Port_Number* veya *FQDN:Port_Number*.
- Depolama ve ağ için performans verilerini toplamak için vCenter için istatistik ayarları düzeyini üç sunucu olarak ayarlanması gerekir.
- Düzey üç düşükse bulma performans verileri toplanmaz çalışır. Bazı sayaçları toplanabilir, ancak diğer sıfır olarak ayarlanır.
- Depolama ve ağ için performans verileri toplanmaz, değerlendirme boyut önerileri, CPU ve bellek ve disk ve ağ bağdaştırıcıları için yapılandırma verilerine göre performans verilerini demektir. 
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




## <a name="securing-the-collector-appliance"></a>Toplayıcı gerecini güvenliğini sağlama


Toplayıcı gerecini güvenliğini sağlamak için aşağıdaki adımları öneriyoruz:

- Paylaşma veya yönetici parolaları yetkisiz kişilerle misplace kullanmayın.
- Gereç kullanımda olmadığında kapatın.
- Güvenli bir ağ Gereci yerleştirin.
- Geçiş tamamlandıktan sonra gereç örneğini silin.
- Ayrıca, diskler üzerinde önbelleğe vCenter kimlik bilgilerini olabilir geçişten sonra da disk yedekleme dosyalarının (Vmdk) silin.

## <a name="updating-the-collector-vm"></a>Toplayıcı sanal makinesi güncelleştiriliyor

Toplayıcı gerecini üzerinde sürekli Windows güncelleştirmeleri çalıştırmanızı öneririz.

- Toplayıcı 60 gün boyunca güncelleştirilmemesi durumunda otomatik olarak makinesi kapatılıyor başlatır.
- Bulma çalışıyorsa, 60 gün geçtiğinde bile makine kapatılmış gerekmez. Bulma tamamlandıktan sonra makine kapatılır.
- Toplayıcı 45 gün boyunca kullandıysanız, çalışan Windows update tarafından her zaman güncelleştirme makine tutma öneririz.

## <a name="upgrading-the-collector-appliance-version"></a>Toplayıcı Gereci sürümüne yükseltme

OVA yeniden indirmeden Toplayıcı en son sürüme yükseltebilirsiniz.

1. İndirme [en son listelenen yükseltme paketi](concepts-collector-upgrade.md) 
2. İndirilen düzeltme güvenli olmasını sağlamak için yönetici komut penceresi açın ve karma ZIP dosyası oluşturmak için aşağıdaki komutu çalıştırın. Oluşturulan karma karşı belirli bir sürüm belirtilen karma değeri ile eşleşmesi gerekir:

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

    (örnek kullanım C:\>CertUtil - HashFile C:\AzureMigrate\CollectorUpdate_release_1.0.9.14.zip SHA256)
3. Azure geçişi toplayıcısı sanal makineye (Toplayıcı gerecini) zip dosyasını kopyalayın.
4. Zip dosyasını sağ tıklatın ve tümünü Ayıkla'ı seçin.
5. Setup.ps1 üzerinde sağ tıklayın ve PowerShell ile Çalıştır'ı seçin ve güncelleştirmeyi yüklemek için ekrandaki yönergeleri izleyin.


## <a name="discovery-methods"></a>Bulma yöntemleri

Toplayıcı gerecini bulma, tek seferlik veya sürekli bulma için kullanabileceğiniz iki yöntem vardır.


### <a name="one-time-discovery"></a>Tek seferlik bulma

Toplayıcı, bir kerelik vCenter sanal makineleri ile ilgili meta verileri toplamak için sunucu ile iletişim kurar. Bu yöntemi kullanarak:

- Gerecin Azure geçişi projesi için sürekli olarak bağlı değil.
- Bulma tamamlandıktan sonra Azure geçişi şirket içi ortamda değişiklikleri yansıtılmıyor. Değişiklikleri yansıtacak şekilde, aynı projede aynı ortamı yeniden bulmak gerekir.
- Bu keşif yöntemi için üçüncü düzey için vcenter Server istatistik ayarları ayarlamanız gerekir.
- Düzeyi üç ayarladıktan sonra bunu bir gün için performans sayaçlarını oluşturmak için kapladığı. Bu nedenle, bir günün ardından bulma çalıştırırken öneririz.
- Bir VM için performans verilerini toplamak, Gereci vCenter Server'da depolanan geçmiş performans verilerini kullanır. Bu performans geçmişi için geçtiğimiz ay toplar.
- Azure geçişi, her bir ölçüm için ortalama bir sayaç (yoğun sayacı yerine) toplar.
     


### <a name="continuous-discovery"></a>Sürekli bulma

Toplayıcı gerecini sürekli olarak Azure geçişi projesine bağlıdır.

- Toplayıcı, gerçek zamanlı kullanım verilerine her 20 saniyede toplamak için şirket içi ortamı sürekli olarak profilleri.
- Bu model, performans verilerini toplamak için vCenter Server istatistik ayarları üzerinde bağımlı değildir.
- Gereç 20 saniye örneklerini yapar ve her 15 dakikada bir tek veri noktası oluşturur.
- Verileri oluşturmak için Gereci noktası en yüksek değeri 20 saniye örnekleri seçer ve Azure'a gönderir.
- Sürekli, dilediğiniz zaman Toplayıcı profil oluşturma durdurabilirsiniz.
     
> [!NOTE]
> Sürekli bulma işlevi Önizleme aşamasındadır. Düzey 3 vCenter Server istatistik ayarları ayarlanmamış ise bu yöntem kullanmanızı öneririz.


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


### <a name="collected-metadata"></a>Toplanan meta verileri

Toplayıcı gerecini VM'ler için statik aşağıdaki meta verileri bulur:

- VM görünen adı (temel, vCenter sunucusu)
- Sanal makinenin envanteri yolu (konak/klasörü vCenter Server)
- IP adresi
- MAC adresi
- İşletim sistemi
- Çekirdek, disk, NIC sayısı
- Bellek boyutu, Disk boyutları
- VM, disk ve ağ performans sayaçları.



#### <a name="performance-counters"></a>Performans sayaçları


- **Tek seferlik**: sayaçları için bir kerelik bulma toplandığında, aşağıdakilere dikkat edin: 
        
    - Bu, toplamak ve yapılandırma meta verilerini projeye göndermek için 15 dakika sürebilir.
    - Yapılandırma verileri toplandıktan sonra portalda kullanılabilir olması performans verilerini bir saate kadar sürebilir.
    - Meta verileri portalda kullanılabilir olduktan sonra VM'lerin listesi görüntülenir ve değerlendirmesi için grupları oluşturmaya başlayabilir.
- **Sürekli bulma**: sürekli bulma için aşağıdakilere dikkat edin:
    - Yapılandırma verileri VM için bulmayı Başlat sonraki bir saat kullanılabilir
    - Performans verilerini 2 saat sonra kullanılabilir hale gelmeden başlatır.
    - Bulma başlattıktan sonra ortamı değerlendirmeleri oluşturmadan önce profil cihaz için en az bir gün bekleyin.
    
   

**Sayaç** | **Düzey** | **Cihaz başına düzeyi** | **Etki değerlendirmesi** 
--- | --- | --- | ---
CPU.Usage.average | 1 | NA | Önerilen VM boyutu ve maliyet  
mem.Usage.average | 1 | NA | Önerilen VM boyutu ve maliyet  
virtualDisk.read.average | 2 | 2 | Disk boyutu, depolama maliyeti, VM boyutunu hesaplar
virtualDisk.write.average | 2 | 2  | Disk boyutu, depolama maliyeti, VM boyutunu hesaplar
virtualDisk.numberReadAveraged.average | 1 | 3 |  Disk boyutu, depolama maliyeti, VM boyutunu hesaplar
virtualDisk.numberWriteAveraged.average | 1 | 3 |   Disk boyutu, depolama maliyeti, VM boyutunu hesaplar
NET.Received.average | 2 | 3 |  VM boyutu ve ağ maliyeti hesaplar                        |
NET.transmitted.average | 2 | 3 | VM boyutu ve ağ maliyeti hesaplar    




## <a name="next-steps"></a>Sonraki adımlar

[Şirket içi VMware Vm'leri için bir değerlendirmenin ayarlayın](tutorial-assessment-vmware.md)
