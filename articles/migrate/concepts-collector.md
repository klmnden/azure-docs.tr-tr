---
title: Toplayıcı Gereci Azure geçirmek içinde | Microsoft Docs
description: Toplayıcı Gereci ve nasıl yapılandırılacağı genel bakış sağlar.
author: ruturaj
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: ruturajd
services: azure-migrate
ms.openlocfilehash: d0dd310a1f6dff389a4d3dd41dc389b7117272fe
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="collector-appliance"></a>Toplayıcı Gereci

[Azure geçirme](migrate-overview.md) geçiş Azure için şirket içi iş yüklerini değerlendirir. Bu makalede Toplayıcı Gereci kullanma hakkında bilgi sağlar.



## <a name="overview"></a>Genel bakış

Bir Azure geçirmek toplayıcısı şirket içi vCenter ortamınızı keşfetmek için kullanılan hafif bir gereç ' dir. Bu Gereci şirket içi VMware makineleri bulur ve bunlarla ilgili meta verileri Azure geçiş hizmetine gönderir.

Toplayıcı Gereci Azure geçirmek projeden indirebilirsiniz bir OVF ' dir. Bir VMware sanal makineyle 4 çekirdek, 8 GB RAM ve tek bir disk 80 GB başlatır. Windows Server 2012 R2 (64 bit) uygulamasının işletim sistemidir.

Toplayıcı adımları izleyerek oluşturabileceğiniz burada - [toplayıcısı VM oluşturma](tutorial-assessment-vmware.md#create-the-collector-vm).

## <a name="collector-communication-diagram"></a>Toplayıcı iletişim diyagramı

![Toplayıcı iletişim diyagramı](./media/tutorial-assessment-vmware/portdiagram.PNG)


| Bileşen      | Şununla iletişim kurmak için   | Gereken bağlantı noktası                            | Neden                                   |
| -------------- | --------------------- | ---------------------------------------- | ---------------------------------------- |
| Toplayıcı      | Azure Geçişi hizmeti | TCP 443                                  | Toplayıcı hizmeti ile SSL bağlantı noktası 443 iletişim kurabilmesi |
| Toplayıcı      | vCenter Server        | Varsayılan 443                             | Toplayıcı vCenter sunucusu ile iletişim kurabilmesi gerekir. VCenter 443 üzerinde varsayılan olarak bağlanır. VCenter farklı bir bağlantı noktasında dinler varsa, bu bağlantı noktasını toplayıcısında giden bağlantı noktası olarak kullanılabilir olması gerekir |
| Toplayıcı      | RDP|   | TCP 3389 | Toplayıcı makinede RDP için kullanabilmek için |





## <a name="collector-pre-requisites"></a>Toplayıcı ön koşullar

Azure geçirmek hizmete bağlanmak ve bulunan verileri karşıya emin olmak için birkaç önkoşul denetimlerini geçemedi Toplayıcı gerekir. Bu makalede her Önkoşullar arar ve neden gerekli olduğunu anlamalısınız.

### <a name="internet-connectivity"></a>İnternet bağlantısı

Toplayıcı Gereci bulunan makineler bilgileri göndermek için Internet'e bağlı olması gerekir. Makine aşağıdaki iki yöntemden birini bir internet'e bağlanabilir.

1. Toplayıcı doğrudan Internet bağlantısına sahip yapılandırabilirsiniz.
2. Toplayıcı bir proxy sunucu bağlanmak için yapılandırabilirsiniz.
    * Proxy sunucusu kimlik doğrulaması gerektiriyorsa, kullanıcı adı ve parola bağlantı ayarlarını belirtebilirsiniz.
    * Proxy sunucusunun IP adresi/FQDN biçiminde olmalıdır http://IPaddress veya http://FQDN. Yalnızca http proxy desteklenir.

> [!NOTE]
> HTTPS tabanlı proxy sunucuları toplayıcısı tarafından desteklenmez.

#### <a name="whitelisting-urls-for-internet-connection"></a>Internet bağlantısı için uygulamaları güvenilir listeye almayı URL'leri

Önkoşul denetimi Toplayıcı sağlanan ayarları üzerinden internet bağlanabiliyorsa başarılı olur. Bağlantı denetimi URL'lerin bir listesini aşağıdaki tabloda verilen bağlanarak doğrulanır. Herhangi bir URL tabanlı bir güvenlik duvarı proxy kullanıyorsanız giden bağlantıyı denetlemek için bu URL'leri gerekli beyaz liste ile emin olun:

**URL** | **Amacı**  
--- | ---
*.portal.azure.com | Azure hizmetiyle bağlantısını denetleyin ve zaman eşitlemesini doğrulamak için gerekli verir.

Buna ek olarak, onay ayrıca aşağıdaki URL'ler bağlantısını doğrulamak çalışır ancak onay erişilebilir değilse başarısız olmaz. Beyaz liste aşağıdaki URL'ler için yapılandırma isteğe bağlıdır ancak Önkoşul denetimi azaltmak için el ile adımlar gerekir.

**URL** | **Amacı**  | **Beyaz liste ne yok**
--- | --- | ---
*.oneget.org:443 | Gerekli powershell indirmek için vCenter Powerclı modülü temel. | Powerclı yükleme başarısız olur. Modülünü el ile yükleyin.
*.windows.net:443 | Gerekli powershell indirmek için vCenter Powerclı modülü temel. | Powerclı yükleme başarısız olur. Modülünü el ile yükleyin.
*.windowsazure.com:443 | Gerekli powershell indirmek için vCenter Powerclı modülü temel. | Powerclı yükleme başarısız olur. Modülünü el ile yükleyin.
*. powershellgallery.com:443 | Gerekli powershell indirmek için vCenter Powerclı modülü temel. | Powerclı yükleme başarısız olur. Modülünü el ile yükleyin.
*.msecnd.net:443 | Gerekli powershell indirmek için vCenter Powerclı modülü temel. | Powerclı yükleme başarısız olur. Modülünü el ile yükleyin.
*.visualstudio.com:443 | Gerekli powershell indirmek için vCenter Powerclı modülü temel. | Powerclı yükleme başarısız olur. Modülünü el ile yükleyin.

### <a name="time-is-in-sync-with-the-internet-server"></a>Zaman Internet sunucusu ile eşitleme

Toplayıcı hizmet isteklerine kimlik doğrulaması sağlamak için Internet saat sunucusuyla eşitlenmiş olması gerekir. Portal.azure.com zaman doğrulanabilmesi için url Toplayıcısından erişilebilir olması gerekir. Makine eşitlenmemiş ise, saatin Toplayıcı VM'de geçerli saati gibi eşleşecek şekilde değiştirmeniz gerekir:

1. VM bir yönetici komut istemi açın.
1. Saat dilimi denetlemek için w32tm /tz çalıştırın.
1. Zaman eşitleme için w32tm/resync çalıştırın.

### <a name="collector-service-should-be-running"></a>Toplayıcı hizmetinin çalışıyor olması gerekir

Azure geçirmek Toplayıcı hizmetinin makinede çalışıyor olması gerekir. Bu hizmet, makine önyüklendiğinde otomatik olarak başlatılır. Hizmet çalışmıyorsa, başlatabilirsiniz *Azure geçirmek Toplayıcı* Denetim Masası aracılığıyla hizmet. Toplayıcı hizmeti vCenter sunucusuna bağlanmak, makine meta veri ve performans verilerini toplama ve hizmete göndermek için sorumludur.

### <a name="vmware-powercli-65"></a>VMware Powerclı 6.5

VMware Powerclı powershell modülü Toplayıcı makine ayrıntılarını ve performans verilerini ve vCenter sunucusu ile iletişim kurabilmesi için yüklü olması gerekir. Powershell modülü otomatik olarak indirilir ve önkoşul denetimi bir parçası olarak yüklenir. Otomatik olarak karşıdan yüklenmesi gerekir ya da sağlayan başarısız birkaç URL'leri Güvenilenler listesine, uygulamaları güvenilir listeye almayı tarafından erişmesine veya modül el ile yükleme.

Aşağıdaki adımları kullanarak el ile modülünü yükleyin:

1. İnternet bağlantısı olmadan Toplayıcısında Powerclı yüklemek için verilen adımları izleyin [bu bağlantıyı](https://blogs.vmware.com/PowerCLI/2017/04/powercli-install-process-powershell-gallery.html) .
2. İnternet erişimi olan başka bir bilgisayara, PowerShell modülü yükledikten sonra dosyaları VMware.* bu makineden Toplayıcı makine kopyalayın.
3. Önkoşul denetimleri yeniden başlatın ve Powerclı yüklü olduğunu onaylayın.

## <a name="connecting-to-vcenter-server"></a>VCenter sunucusuna bağlanma

Toplayıcı, vCenter sunucusuna bağlanmak ve sanal makineler, bunların meta verilerini ve bunların performans sayaçlarını sorgulayabilmesi gerekir. Bu veriler proje tarafından bir değerlendirme hesaplamak için kullanılır.

1. VCenter sunucusuna bağlanmak için aşağıdaki tabloda verilen izinleri salt okunur bir hesapla bulma çalıştırmak için kullanılabilir.

    |Görev  |Gerekli rol/hesap  |İzinler  |
    |---------|---------|---------|
    |Toplayıcı tabanlı gereç bulma    | En az bir salt okunur kullanıcının gerekiyor        |Veri Merkezi nesnesi –> Alt Nesneye Yay, role=Read-only         |

2. Yalnızca belirtilen vCenter hesabı erişilebilir veri merkezleri bulma için erişilebilir.
3. VCenter vCenter sunucusuna bağlanmak için FQDN/IP adresini belirtmeniz gerekir. Varsayılan olarak 443 numaralı bağlantı noktası üzerinden bağlanır. Farklı bir bağlantı noktası üzerinde dinleme vCenter yapılandırdıysanız, sunucu adresi IPAddress:Port_Number veya FQDN:Port_Number biçiminde bir parçası olarak belirtebilirsiniz.
4. Dağıtıma başlamadan önce vCenter sunucusu istatistikleri ayarlarını düzeyi 3 ayarlamanız gerekir. Düzey 3'ten düşükse bulma tamamlanır ancak depolama ve ağ için performans verilerini toplanan olmaz. Değerlendirmesi boyutu önerileri bu durumda CPU ve bellek için performans verilerini ve yalnızca yapılandırma verilerini disk ve ağ bağdaştırıcıları için temel alır. [Daha fazla bilgi](./concepts-collector.md) hangi verileri toplanır ve nasıl değerlendirme etkiler.
5. Toplayıcı bir ağ görüş vCenter sunucusuna sahip olmalıdır.

> [!NOTE]
> Yalnızca vCenter Server 5.5, 6.0 ve sürümleri 6.5 resmi olarak desteklenir.

> [!IMPORTANT]
> Böylece tüm sayaçları doğru toplanan istatistikleri düzeyi için en yüksek ortak düzeyi (3) ayarlamanızı öneririz. Daha düşük düzeyde ayarlamak vCenter varsa, yalnızca birkaç sayaçları tamamen 0 olarak ayarlayın rest ile toplanabilir. Değerlendirme ardından eksik verileri gösterebilir.

### <a name="selecting-the-scope-for-discovery"></a>Keşfi için kapsamı seçme

VCenter bağlandıktan sonra bulmak için bir kapsamı seçebilirsiniz. Bir kapsam seçerek belirtilen vCenter envanteri yolu tüm sanal makinelerden bulur.

1. Kapsam, bir veri merkezi, bir klasör veya ESXi ana bilgisayar olabilir.
2. Aynı anda yalnızca bir kapsamı seçebilirsiniz. Daha fazla sanal makine seçmek için bir bulma tamamlamak ve yeni bir kapsam ile keşif işlemi yeniden başlatın.
3. Yalnızca sahip bir kapsamı seçebilirsiniz *değerinden 1500 sanal makineleri*.

## <a name="specify-migration-project"></a>Geçiş projesi belirtin

Şirket içi vCenter bağlı ve kapsam belirtilen sonra bulma ve değerlendirme için kullanılması gereken geçiş proje ayrıntılarını şimdi belirtebilirsiniz. Proje kimliği ve anahtarı belirtin ve bağlanın.

## <a name="start-discovery-and-view-collection-progress"></a>Bulma ve görünüm koleksiyonu ilerleme Başlat

Bulma başladıktan sonra vCenter sanal makineler bulunan ve meta verileri ve performans verilerini sunucuya gönderilir. İlerleme durumunu, aşağıdaki kimlikleri bildirir:

1. Toplayıcı kimliği: Toplayıcı makinenize verilen benzersiz bir kimliği. Bu kimliği için belirli bir makine üzerinde farklı bulmaların değiştirmez. Hataları durumunda bu kimliği, sorunu Microsoft Support bildirilirken kullanabilirsiniz.
2. Oturum kimliği: Çalışan bir koleksiyon işi için benzersiz bir kimliği. Bulma işi tamamlandığında Portalı'nda aynı oturum kimliği başvurabilirsiniz. Bu kimliği her koleksiyon iş için değiştirir. Hataları durumunda, bu kimliği Microsoft Support bildirebilirsiniz.

### <a name="what-data-is-collected"></a>Hangi veriler toplanır?

Seçili sanal makinelerle ilgili olarak aşağıdaki statik meta veri toplama işi bulur.

1. VM görüntü adına (vCenter)
2. Sanal makinenin envanteri yolu (ana bilgisayar/klasör vcenter)
3. IP adresi
4. MAC adresi
5. İşletim sistemi
5. Çekirdek, diskleri, NIC sayısı
6. Bellek boyutunu, Disk boyutları
7. Ve VM, disk ve aşağıdaki tabloda listelendiği gibi ağ performans sayaçları.

Aşağıdaki tabloda, toplanan ve ayrıca belirli bir sayaç alınamadı, etkilenen değerlendirme sonuçlarını listeler performans sayaçlarını listeler.

|Sayaç                                  |Düzey    |Aygıt başına düzeyi  |Değerlendirme etkisi                               |
|-----------------------------------------|---------|------------------|------------------------------------------------|
|CPU.Usage.average                        | 1       |Yok                |Önerilen VM boyutu ve maliyet                    |
|mem.Usage.average                        | 1       |Yok                |Önerilen VM boyutu ve maliyet                    |
|virtualDisk.read.average                 | 2       |2                 |Disk boyutu, depolama maliyeti ve VM boyutu         |
|virtualDisk.write.average                | 2       |2                 |Disk boyutu, depolama maliyeti ve VM boyutu         |
|virtualDisk.numberReadAveraged.average   | 1       |3                 |Disk boyutu, depolama maliyeti ve VM boyutu         |
|virtualDisk.numberWriteAveraged.average  | 1       |3                 |Disk boyutu, depolama maliyeti ve VM boyutu         |
|NET.Received.average                     | 2       |3                 |VM boyutu ve ağ maliyeti                        |
|NET.transmitted.average                  | 2       |3                 |VM boyutu ve ağ maliyeti                        |

> [!WARNING]
> Daha yüksek bir istatistik düzeyi yeni ayarladıysanız, bu günde bir performans sayaçlarını oluşturmak için kadar sürebilir. Bu nedenle, bir günün ardından bulma çalıştırmanızı öneririz.

### <a name="time-required-to-complete-the-collection"></a>Koleksiyon tamamlamak için gereken süre

Toplayıcı yalnızca makine verileri bulur ve projeye gönderir. Proje bulunan verileri portalda görüntülenir ve bir değerlendirme oluşturmaya başlamadan önce ek zaman alabilir.

Seçilen kapsam içindeki sanal makinelerin sayısına dayalı olarak, fazla 15 dakika projeye statik meta verileri gönder sürer. Statik meta verileri portalda kullanılabilir olduğunda, portal makinelerinizde listesini görmek ve grupları oluşturmaya başlayın. Bir değerlendirme toplama işi tamamlandıktan ve proje verileri işleyene kadar oluşturulamıyor. Bir kez koleksiyonu iş Toplayıcısında tamamlandı, onu değerine kadar sürebilir portalında kullanılabilir olması performans verileri için bir saat seçili kapsamdaki sanal makinelerin sayısına dayalı olarak.

## <a name="locking-down-the-collector-appliance"></a>Toplayıcı Gereci kilitleme
Toplayıcı aygıtındaki sürekli Windows güncelleştirmelerini çalıştıran öneririz. Toplayıcı 45 gün güncelleştirilmezse, Toplayıcı makinesi otomatik olarak kapatılıyor başlar. Bulma çalışıyorsa, 45 gün süresi olsa bile makine kapalı, etkinleştirilmemiş. POST bulma işi tamamlandığında, makine kapatılır. Toplayıcı 45 günden fazla bir süre için kullanıyorsanız, her zaman, çalışan Windows update tarafından güncelleştirilmiş makine tutmanızı önerir.

Ayrıca aygıtınızın güvenli hale getirmek için aşağıdaki adımları öneririz
1. Paylaşma değil veya yönetici parolalarını yetkisiz taraflarla misplace.
2. Kullanılmadığı zaman Gereci kapatın.
3. Güvenli bir ağ Gereci yerleştirin.
4. Geçiş işi tamamlandıktan sonra Gereci örneğini silin. Diskleri üzerlerinde önbelleğe alınan vCenter kimlik bilgileri gerekebilir gibi disk dosyaları (VMDKs), yedekleme de sildiğinizden emin olun.

## <a name="how-to-upgrade-collector"></a>Toplayıcı yükseltme

Toplayıcı OVA yeniden yüklemeden en son sürüme yükseltebilirsiniz.

1. En son karşıdan [yükseltme paketi](https://aka.ms/migrate/col/latestupgrade).
2. İndirilen düzeltme güvenli olduğundan emin olmak için yönetici komut penceresi açın ve ZIP dosyası için karma oluşturmak için aşağıdaki komutu çalıştırın. Üretilen karma karşı belirli sürümü belirtilen karma ile eşleşmesi gerekir:

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

    (örnek kullanım C:\>CertUtil - HashFile C:\AzureMigrate\CollectorUpdate_release_1.0.9.5.zip SHA256)
3. ZIP dosyasının Azure geçirmek Toplayıcı sanal makineye (Toplayıcı Gereci) kopyalayın.
4. Zip dosyasını sağ tıklatın ve tümünü Ayıkla seçin.
5. Setup.ps1 üzerinde sağ tıklayın ve PowerShell ile Çalıştır'ı seçin ve güncelleştirmeyi yüklemek için ekrandaki yönergeleri izleyin.

### <a name="list-of-updates"></a>Güncelleştirmeleri listesi

#### <a name="upgrade-to-version-1097"></a>1.0.9.7 sürüme yükseltin

Yükseltme sürümü 1.0.9.7 indirme için [paketi](https://aka.ms/migrate/col/upgrade_9_7)

**Algoritma** | **Karma değeri**
--- | ---
MD5 | 01ccd6bc0281f63f2a672952a2a25363
SHA1 | 3e6c57523a30d5610acdaa14b833c070bffddbff
SHA256 | e3ee031fb2d47b7881cc5b13750fc7df541028e0a1cc038c796789139aa8e1e6

#### <a name="upgrade-to-version-1095"></a>1.0.9.5 sürüme yükseltin

Yükseltme sürümü 1.0.9.5 indirme için [paketi](https://aka.ms/migrate/col/upgrade_9_5)

**Algoritma** | **Karma değeri**
--- | ---
MD5 | d969ebf3bdacc3952df0310d8891ffdf
SHA1 | f96cc428eaa49d597eb77e51721dec600af19d53
SHA256 | 07c03abaac686faca1e82aef8b80e8ad8eca39067f1f80b4038967be1dc86fa1

## <a name="next-steps"></a>Sonraki adımlar

[Şirket içi VMware Vm'leri için bir değerlendirmesi ayarlayın](tutorial-assessment-vmware.md)
