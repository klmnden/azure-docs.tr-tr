<properties
   pageTitle="Şirket içi bir StorSimple cihazı dağıtma | Microsoft Azure"
   description="StorSimple cihazını ve hizmetini dağıtmak için adımları ve en iyi yöntemleri açıklar. (Microsoft Azure StorSimple sürüm .3 ve önceki sürümler için geçerlidir.)"
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="04/26/2016"
   ms.author="alkohli" />

# Şirket içi StorSimple cihazınızı dağıtma

> [AZURE.SELECTOR]
- [Güncelleştirme 2](../articles/storsimple/storsimple-deployment-walkthrough-u2.md)
- [Güncelleştirme 1](../articles/storsimple/storsimple-deployment-walkthrough-u1.md)
- [GA Sürümü](../articles/storsimple/storsimple-deployment-walkthrough.md)

## Genel Bakış

Microsoft Azure StorSimple cihaz dağıtımına hoş geldiniz. Bu dağıtım öğreticileri StorSimple 8000 Serisi Yayınlanan Sürüm, StorSimple 8000 Serisi Güncelleştirme 0.1, StorSimple 8000 Serisi Güncelleştirme 0.2 ve StorSimple 8000 Serisi Güncelleştirme 0.3 için geçerlidir. Bu öğretici dizisinde StorSimple cihazınızı nasıl yapılandıracağınız açıklanmaktadır. Öğreticiler bir yapılandırma denetim listesi, yapılandırma önkoşulları ve ayrıntılı yapılandırma adımlarını içerir.


Bu öğreticilerdeki bilgiler, güvenlik önlemlerini gözden geçirdiğinizi ve StorSimple cihazınızı kutusundan çıkardığınızı, yerleştirdiğinizi ve kablolarını taktığınızı varsayar. Bu görevleri henüz gerçekleştirmediyseniz, [güvenlik önlemlerini](storsimple-safety.md) inceleyerek başlayın. Cihazınızın modeline bağlı olarak, aşağıdaki yönergeleri izleyerek cihazı kutusundan çıkarabilir, rafa monte edebilir ve kablolarını bağlayabilirsiniz:

- [8100 model cihazınızı kutusundan çıkarma, rafa monte etme ve kablolarını bağlama](storsimple-8100-hardware-installation.md)
- [8600 model cihazınızı kutusundan çıkarma, rafa monte etme ve kablolarını bağlama](storsimple-8600-hardware-installation.md)

Kurulum ve yapılandırma işlemini tamamlamak için yönetici ayrıcalıkları gerekir. Başlamadan önce yapılandırma denetim listesini gözden geçirmenizi öneririz. Dağıtım ve yapılandırma işleminin tamamlanması biraz zaman alabilir.

> [AZURE.NOTE] Microsoft Azure Web sitesinde yayınlanan StorSimple dağıtım bilgileri yalnızca StorSimple 8000 serisi cihazlar için geçerlidir. 5000 ve 7000 serisi cihazlar hakkında tam bilgi için şu adresi ziyaret edin: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). 5000 ve 7000 serisi dağıtım bilgileri için bkz. [StorSimple Sistemi Hızlı Başlangıç Kılavuzu](http://onlinehelp.storsimple.com/111_Appliance/).

## Dağıtım adımları

StorSimple cihazınızı yapılandırmak ve StorSimple Yöneticisi hizmetine bağlamak için gerekli adımları gerçekleştirin. Gerekli adımlara ek olarak, dağıtım sırasında ihtiyacınız olabilecek isteğe bağlı adım ve yordamlar vardır. Bu adım adım dağıtım yönergelerinde isteğe bağlı adımların her birini ne zaman gerçekleştirmeniz gerektiği belirtilmiştir.


| Adım                                                                                   | Açıklama                                                                                                                                                   |
|----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ÖN KOŞULLAR**                                                                      | Dağıtım için bu önkoşulların tamamlanması gerekir.                                                                                        |
| Dağıtım yapılandırma denetim listesi.                                                     | Dağıtımdan önce ve dağıtım sırasında bilgi toplamak ve bilgileri kaydetmek için bu denetim listesini kullanın.                                                                       |
| Dağıtım önkoşulları.                                                               | Bunlar, ortamın dağıtım için hazır olduğunu doğrular.                                                                                                     |
|                                                                                        |                                                                                                                                                               |
| **ADIM ADIM DAĞITIM**                                                                   | Bu adımlar, StorSimple cihazınızı üretim ortamına dağıtmak için gereklidir.                                                                                      |
| 1. Adım: Yeni bir hizmet oluşturun.                                                         | StorSimple cihazınız için bulut yönetimi ve depolamayı ayarlayın. Başka StorSimple cihazlar için bir hizmetiniz varsa, bu adımı atlayın.                |
| 2. Adım: Hizmet kayıt anahtarını alın.                                               | StorSimple cihazınızı kaydetmek ve yönetim hizmetine bağlamak için bu anahtarı kullanın.                                                                         |
| 3. Adım: StorSimple için Windows PowerShell üzerinden cihazı yapılandırın ve kaydedin.    | Yönetim hizmetini kullanarak kurulumu tamamlamak için cihazı ağınıza bağlayın ve Azure ile kaydedin.                                            |
| 4. Adım: Minimum cihaz kurulumunu tamamlayın</br>İsteğe bağlı: StorSimple cihazınızı güncelleştirin.      | Depolama alanı sağlamak için, yönetim hizmetini kullanarak cihaz kurulumunu tamamlayın ve etkinleştirin.                                                                      |
| 5. Adım: Birim kapsayıcısı oluşturun.                                                      | Birimleri sağlamak için bir kapsayıcı oluşturun. Birim kapsayıcısı, kapsadığı tüm birimler için depolama hesabı, bant genişliği ve şifreleme ayarlarını içerir.    |
| 6. Adım: Birim oluşturun.                                                                | Sunucularınız için StorSimple cihazında depolama birimleri sağlayın.                                                                                        |
| 7. Adım: Bir birimi bağlayın, başlatın ve biçimlendirin.</br>İsteğe bağlı: MPIO’yu yapılandırın.            | Sunucularınızı cihaz tarafından sağlanan iSCSI depolama alanına bağlayın. İsteğe bağlı olarak, sunucularınızın bağlantı, ağ ve arabirim hatalarından etkilenmemesini sağlamak için MPIO’yu yapılandırın.                                                                                                                                                              |
| 8. Adım: Yedekleyin.                                                                  | Verilerinizi korumak için yedekleme ilkenizi ayarlayın                                                                                                                 |
|                                                                                        |                                                                                                                                                               |
| **DİĞER YORDAMLAR**                                                                   | Çözümünüzü dağıtırken bu yordamlara başvurmanız gerekebilir.                                                                                        |
| Hizmet için yeni bir depolama hesabı yapılandırın.                                      |                                                                                                                                                               |
| Cihaz seri konsoluna bağlanmak için PuTTY kullanın.                                    |                                                                                                                                                               |
| Bir Windows Server konağının IQN’ini alın.                                                   |                                                                                                                                                               |
| El ile yedekleme oluşturun.                                                                 |


## Dağıtım yapılandırma denetim listesi

Aşağıdaki dağıtım yapılandırma denetim listesinde, StorSimple cihazınızda yazılım yapılandırmadan önce ve yapılandırma sırasında toplamanız gereken bilgiler açıklanmıştır. Bu bilgilerin bir bölümünü önceden hazırlamak, ortamınızda StorSimple cihazını dağıtma işlemini kolaylaştırmaya yardımcı olur. Cihazınızı dağıtırken yapılandırma ayrıntılarını not etmek için de bu denetim listesini kullanın.

| Aşama                                  | Parametre                                         | Ayrıntılar                                                                                                                                                                | Değerler |
|----------------------------------------|---------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------|
| **Cihazınızın kablolarını bağlama**                      | Seri erişim                                     | İlk cihaz yapılandırması                                                                  | Evet/Hayır |
|   |   |  |  |
| **Cihazı yapılandırma ve kaydetme**          | Veri 0 ağ ayarları                           | Veri 0 IP Adresi:</br>Alt ağ maskesi:</br>Ağ geçidi:</br>Birincil DNS sunucusu:</br>Birincil NTP sunucusu:</br>Web proxy sunucusu IP/FQDN (isteğe bağlı):</br>Web proxy bağlantı noktası:|        |
|                                        | Cihaz yöneticisi parolası                     | Parola 8 ile 15 karakter arasında olmalı ve küçük harfler, büyük harfler, sayısal ve özel karakterler içermelidir. |        |
|                                        | StorSimple Snapshot Manager parolası              | Parola 14 veya 15 karakterden oluşmalı ve küçük harfler, büyük harfler, sayısal ve özel karakterler içermelidir.|        |
|                                        | Hizmet Kayıt Anahtarı                          | Bu anahtar Azure klasik portalından oluşturulur.    |        |
|                                        | Hizmeti Verileri Şifreleme Anahtarı                       | Bu anahtar, cihaz StorSimple için Windows PowerShell üzerinden yönetim hizmetine kaydedildiğinde oluşturulur. Bu anahtarı kopyalayın ve güvenli bir konuma kaydedin.|  |
|   |   |  |  |
| **Minimum cihaz kurulumunu tamamlayın**          | Cihazınızın kolay adı                     | Bu cihaz için açıklayıcı bir addır. |        |
|                                        | Saat dilimi                                          | Cihazınız zamanlanan tüm işlemler için bu saat dilimini kullanır.  |        |
|                                        | İkincil DNS sunucusu                              | Bu gerekli bir yapılandırmadır.                                  |        |
|                                        | Ağ arabirimi: Veri 0 denetleyicisi sabit IP'leri                                     | Bu IP'ler İnternet'e yönlendirilebilir olmalıdır.</br>Denetleyici 0 sabit IP adresi:</br>Denetleyici 1 sabit IP adresi:|
|   |   |  |  |
| **Ek ağ arabirimi ayarları**  | Ağ arabirimi: Veri 1</br>iSCSI etkinse, Ağ geçidini yapılandırmayın.      | Amaç: Bulut/iSCSI/Kullanılmıyor</br>IP adresi:</br>Alt ağ maskesi:</br>Ağ geçidi:|
|                                        | Ağ arabirimi: Veri 2</br>iSCSI etkinse, Ağ geçidini yapılandırmayın.      | Amaç: Bulut/iSCSI/Kullanılmıyor</br>IP adresi:</br>Alt ağ maskesi:</br>Ağ geçidi:|
|                                        | Ağ arabirimi: Veri 3</br>iSCSI etkinse, Ağ geçidini yapılandırmayın.      | Amaç: Bulut/iSCSI/Kullanılmıyor</br>IP adresi:</br>Alt ağ maskesi:</br>Ağ geçidi:|
|                                        | Ağ arabirimi: Veri 4</br>iSCSI etkinse, Ağ geçidini yapılandırmayın.      | Amaç: Bulut/iSCSI/Kullanılmıyor</br>IP adresi:</br>Alt ağ maskesi:</br>Ağ geçidi:|
|                                        | Ağ arabirimi: Veri 5</br>iSCSI etkinse, Ağ geçidini yapılandırmayın.      | Amaç: Bulut/iSCSI/Kullanılmıyor</br>IP adresi:</br>Alt ağ maskesi:</br>Ağ geçidi:|
|   |   |  |  |
| **Birim kapsayıcısı oluşturun**                      | Birim kapsayıcısı adı:                            | Kapsayıcı adı                                                                                                                                                 |        |
|                                        | Azure Storage hesabı:                            | Bu birim kapsayıcısı ile ilişkilendirilecek depolama hesabı adı ve erişim anahtarı                                                                                              |        |
|                                        | Bulut depolama şifreleme anahtarı:                     | Her bir kapsayıcıdaki depolama için şifreleme anahtarı                                                                                                                           |        |
|   |   |  |  |
| **Birim oluşturun**                        | Her birim için ayrıntılar                           | Birim adı:                                                                                                                                                           |        |
|                                        |                                                   | Boyut:                                                                                                                                                                  |        |
|                                        |                                                   | Kullanım türü:                                                                                                                                                            |        |
|                                        |                                                   | ACR adı:                                                                                                                                                              |        |
|                                        |                                                   | Varsayılan yedekleme ilkesi:                                                                                                                                                 |        |
|   |   |  |  |
| **Bir birimi bağlayın, başlatın ve biçimlendirin** | Depolama alanına bağlanan her konak sunucusu için ayrıntılar | Windows Server adı:                                                                                                                                                   |        |
|                                        |                                                   | Windows Server IQN:                                                                                                                                                    |        |
|                                        |                                                   | Windows Server birim adı:                                                                                                                                                   |        |
|                                        |                                                   | NTFS bağlama noktası/Sürücü harfi:                                                                                                                                      |        |

## Dağıtım önkoşulları

Aşağıdaki bölümlerde, StorSimple Yöneticisi hizmetiniz, StorSimple cihazınız ve veri merkezinizdeki ağ yapılandırması için önkoşullar açıklanmaktadır.

### StorSimple Yöneticisi hizmeti için

Başlamadan önce aşağıdakilerden emin olun:

- Erişim kimlik bilgilerine sahip bir Microsoft hesabınız var.

- Erişim kimlik bilgilerine sahip bir Microsoft Azure Storage hesabınız var.

- Microsoft Azure aboneliğiniz StorSimple Yöneticisi hizmeti için etkinleştirildi. Aboneliğinizin [Kurumsal Anlaşma](https://azure.microsoft.com/pricing/enterprise-agreement/) aracılığıyla satın alınmış olması gerekir.

- PuTTY gibi bir terminal öykünme yazılımına erişiminiz var.

### Veri merkezindeki cihaz için

Cihazı yapılandırmadan önce aşağıdakilerden emin olun:

- Cihazınız tam olarak açılmış, bir rafa monte edilmiş ve güç, ağ ve seri erişim için kablolar aşağıdaki şekilde tam olarak bağlanmış:

    -  [8100 model cihazınızı kutusundan çıkarma, rafa monte etme ve kablolarını bağlama](storsimple-8100-hardware-installation.md)
    -  [8600 model cihazınızı kutusundan çıkarma, rafa monte etme ve kablolarını bağlama](storsimple-8600-hardware-installation.md)


### Veri merkezindeki ağ için

Başlamadan önce aşağıdakilerden emin olun:

- Veri merkezi güvenlik duvarınızdaki bağlantı noktaları iSCSI ve bulut trafiğine izin vermek için [StorSimple cihazınız için ağ gereksinimleri](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device) bölümünde belirtildiği şekilde açık.
- Veri merkezinizdeki cihaz dış ağa bağlanabiliyor. Dış ağ bağlantısını doğrulamak için aşağıdaki [Windows PowerShell 4.0](http://www.microsoft.com/download/details.aspx?id=40855) cmdlet’lerini (aşağıdaki tabloda verilmiştir) çalıştırın. Bu doğrulamayı Azure’a bağlı olan ve StorSimple cihazınızı dağıtacağınız (veri merkezi ağındaki) bir bilgisayarda gerçekleştirin.  

| Bu parametre için...       | Geçerliliğini denetlemek için...                                                                                                                                                                                | Bu komutları/cmdlet'leri çalıştırın.                                                                                                                                                             |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **IP**</br>**Alt ağ**</br>**Ağ geçidi** | Bu geçerli bir IPv4 veya IPv6 adresi mi?</br>Bu geçerli bir alt ağ mı?</br>Bu geçerli bir ağ geçidi mi?</br>Bu ağ üzerinde yinelenen bir IP mi?                                                                          | `ping ip`</br>`arp -a`</br> `ping` ve `arp` komutları, veri merkezinde bu IP’yi kullanan bir cihaz olmadığını belirtmek için başarısız olmalıdır.
|                           |                                                                                                                                                                    |                                                                                                                                                                                         |
| **DNS**                       | Bu Azure URL’lerini çözebilen geçerli bir DNS mi?                                                                                                                    | `Resolve-DnsName -Name www.bing.com -Server <DNS server IP address>` </br>Kullanılabilir alternatif bir komut şöyledir:</br>`nslookup --dns-ip=<DNS server IP address> www.bing.com`      |
|                           | Bağlantı noktası 53’ün açık olup olmadığını denetleyin. Bu, yalnızca cihazınız için bir dış DNS kullanıyorsanız geçerlidir. İç DNS, dış URL'leri otomatik olarak çözmelidir.  | `Test-Port -comp dc1 -port 53 -udp -UDPtimeout 10000`  </br>[Bu cmdlet hakkında daha fazla bilgi](http://learn-powershell.net/2011/02/21/querying-udp-ports-with-powershell/)|
|                           |                                                                                                                                                                    |                                                                                                                                                                                         |
| **NTP**                       | NTP sunucusu girildikten hemen sonra bir saat eşitlemesi tetikleriz. `time.windows.com` veya ortak saat sunucuları girdiğinizde UDP bağlantı noktası 123’ün açık olup olmadığını denetleyin). | [Bu betiği indirin ve kullanın](https://gallery.technet.microsoft.com/scriptcenter/Get-Network-NTP-Time-with-07b216ca).                                                                                                                                                           |
|                           |                                                                                                                                                                    |                                                                                                                                                                                         |
| **Proxy (isteğe bağlı)**          | Bu geçerli bir proxy URI’si ve bağlantı noktası mı? </br> Kimlik doğrulama modu doğru mu?                                                                                                                                | <code>wget http://bing.com &#124; % {$_.StatusCode}</code></br>Bu komut, web proxy’si yapılandırdıktan hemen sonra çalıştırılmalıdır. Durum kodu 200 döndürülmesi bağlantının başarılı olduğunu gösterir.                                                                                                                                 |
|                           | Trafik proxy üzerinden yönlendirilebilir mi?                                                                                                                                 | Cihazınızda proxy yapılandırmasını tamamladıktan sonra DNS doğrulaması, NTP denetimi veya HTTP denetimini çalıştırın. Bu, trafiğin proxy veya başka bir engele takılıp takılmadığını gösterir.                                                                                                                      |
|                           |                                                                                                                                                                    |                                                                                                                                                                                         |
| **Kayıt**              | Giden TCP bağlantı noktası 443, 80 ve 9354’ün açık olup olmadığını denetleyin.                                                                                                                |  `Test-NetConnection -Port   443 -InformationLevel Detailed`</br>[Test-NetConnection cmdlet’i hakkında daha fazla bilgi](https://technet.microsoft.com/library/dn372891.aspx)                                                                           |

## Adım adım dağıtım

StorSimple cihazınızı veri merkezinde dağıtmak için aşağıdaki adım adım yönergeleri kullanın.

## 1. Adım: Yeni bir hizmet oluşturun

Bir StorSimple Yöneticisi hizmeti birden çok StorSimple cihazını yönetebilir. İlk StorSimple cihazınızın dağıtımı için yeni bir StorSimple Yöneticisi hizmeti oluşturmanız gerekir.

> [AZURE.IMPORTANT] Mevcut bir StorSimple Yöneticisi hizmetiniz varsa ve StorSimple cihazınızı bu hizmetle dağıtmak istiyorsanız bu adımı atlayın.

StorSimple Yöneticisi hizmetinin yeni bir örneğini oluşturmak için aşağıdaki adımları gerçekleştirin.

[AZURE.INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

> [AZURE.IMPORTANT] Hizmetinizle birlikte bir depolama hesabının otomatik olarak oluşturulmasını etkinleştirmediyseniz, bir hizmeti başarıyla oluşturduktan sonra en az bir depolama hesabı oluşturmanız gerekir. Bir birim kapsayıcısı oluşturduğunuzda, bu depolama hesabı kullanılacaktır.
>
> Otomatik olarak bir depolama hesabı oluşturmadıysanız, ayrıntılı yönergeler için [Hizmet için yeni bir depolama hesabı yapılandırma](#configure-a-new-storage-account-for-the-service) bölümüne gidin.
> Bir depolama hesabının otomatik olarak oluşturulmasını etkinleştirdiyseniz, [2. Adım: Hizmet kayıt anahtarını alın](#step-2:-get-the-service-registration-key) bölümüne gidin.

## 2. Adım: Hizmet kayıt anahtarını alın

StorSimple Yöneticisi hizmeti çalışır duruma geldikten sonra, hizmet kayıt anahtarını almanız gerekir. Bu anahtar StorSimple cihazınızı kaydetmek ve hizmete bağlamak için kullanılır.

Azure klasik portalında aşağıdaki adımları gerçekleştirin.

[AZURE.INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]


## 3. Adım: StorSimple için Windows PowerShell üzerinden cihazı yapılandırın ve kaydedin

> [AZURE.IMPORTANT] Bu yapılandırmayı gerçekleştirmeden önce, her iki denetleyicide de (aktif ve pasif) VERİ 0 hariç tüm ağ arabirimlerini çıkarın.

Aşağıdaki yordamda açıklandığı gibi StorSimple cihazınızın ilk kurulumu tamamlamak üzere StorSimple için Windows PowerShell kullanın. Bu adımı tamamlamak için bir terminal öykünme yazılımı kullanmanız gerekir. Daha fazla bilgi için bkz. [Cihaz seri konsoluna bağlanmak için PuTTY kullanma](#use-putty-to-connect-to-the-device-serial-console).

[AZURE.INCLUDE [storsimple-configure-and-register-device](../../includes/storsimple-configure-and-register-device.md)]

## 4. Adım: Minimum cihaz kurulumunu tamamlayın

StorSimple cihazınız için en düşük cihaz yapılandırması için aşağıdakileri yapmanız gerekir:

- İkincil DNS sunucusunu ayarlayın.
- En az bir ağ arabiriminde iSCSI’ı etkinleştirin.
- İki denetleyiciye de sabit IP adresleri atayın.

En düşük cihaz kurulumunu tamamlamak için Azure klasik portalında aşağıdaki adımları gerçekleştirin.

[AZURE.INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup.md)]

Cihaz yapılandırması tamamlandıktan sonra, güncelleştirmeleri taramanız ve kullanılabilir güncelleştirme varsa yüklemeniz gerekir. Güncelleştirmelerin tamamlanması birkaç saat sürebilir. [Güncelleştirmeleri tarayın ve uygulayın](#scan-for-and-apply-updates) bölümündeki yönergeleri izleyin.


## 5. Adım: Birim kapsayıcısı oluşturun

Birim kapsayıcısı, kapsadığı tüm birimler için depolama hesabı, bant genişliği ve şifreleme ayarlarını içerir. StorSimple cihazınızda birimleri sağlamaya başlamadan önce bir birim kapsayıcısı oluşturmanız gerekir.

Birim kapsayıcısı oluşturmak için Azure klasik portalında aşağıdaki adımları gerçekleştirin.

[AZURE.INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## 6. Adım: Birim oluşturun

Bir birim kapsayıcısı oluşturduktan sonra, sunucularınız için StorSimple cihazında bir depolama birimi sağlayabilirsiniz. Birim oluşturmak için Azure klasik portalında aşağıdaki adımları gerçekleştirin.

> [AZURE.IMPORTANT] StorSimple Yöneticisi yalnızca ölçülü kaynak kullanan birimler oluşturabilir.  Tamamen veya kısmen sağlanan birimler oluşturamazsınız.

[AZURE.INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume.md)]

## 7. Adım: Bir birimi bağlayın, başlatın ve biçimlendirin

> [AZURE.IMPORTANT]

> - StorSimple çözümünüzün yüksek oranda kullanılabilirliğini sağlamak için, Windows Server konağınızda iSCSI’ı yapılandırmadan önce Windows Server konağınızda (isteğe bağlı) MPIO’yu yapılandırmanızı öneririz. Konak sunucuları üzerinde MPIO yapılandırması sunucuların bağlantı, ağ veya arabirim hatalarına dayanabileceğinden emin olunmasını sağlar.

> - MPIO ve iSCSI yükleme ve yapılandırma yönergeleri için [StorSimple cihazınız için MPIO yapılandırma](storsimple-configure-mpio-windows-server.md) bölümüne gidin. Bu adımlar StorSimple birimlerini bağlamayı, başlatmayı ve biçimlendirmeyi de içerir.

MPIO yapılandırmamaya karar verirseniz, StorSimple birimlerinizi bağlamak, başlatmak ve biçimlendirmek için aşağıdaki adımları gerçekleştirin.

[AZURE.INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## 8. Adım: Yedekleyin

Yedeklemeler, birimlerin zaman noktası korumasını sağlar ve geri yükleme sürelerini azaltırken kurtarılabilirliği iyileştirir. StorSimple cihazınızda iki tür yedekleme oluşturabilirsiniz: yerel anlık görüntüler ve bulut anlık görüntüleri. Bu yedekleme türlerinin her biri **Zamanlanmış** veya **El ile** olabilir.

Zamanlanmış yedekleme oluşturmak için Azure klasik portalında aşağıdaki adımları gerçekleştirin.

[AZURE.INCLUDE [storsimple-take-backup](../../includes/storsimple-take-backup.md)]

İstediğiniz zaman el ile yedekleme oluşturabilirsiniz. Yordamlar için, [El ile yedekleme oluşturun](#Create-a-manual-backup) bölümüne gidin.

## Hizmet için yeni bir depolama hesabı yapılandırın

Bu yalnızca hizmetinizle bir depolama hesabının otomatik olarak oluşturulmasını etkinleştirmediyseniz gerçekleştirmeniz gereken isteğe bağlı bir adımdır. StorSimple birim kapsayıcısı oluşturmak için bir Microsoft Azure Storage hesabı gereklidir.

Farklı bir bölgede bir Azure Storage hesabı oluşturmanız gerekiyorsa, adım adım yönergeler için bkz. [Azure Storage hesapları hakkında](../storage/storage-create-storage-account.md).

Azure klasik portalında **StorSimple Yöneticisi hizmeti** sayfasında aşağıdaki adımları gerçekleştirin.

[AZURE.INCLUDE [storsimple-configure-new-storage-account](../../includes/storsimple-configure-new-storage-account.md)]


## Cihaz seri konsoluna bağlanmak için PuTTY kullanın

StorSimple için Windows PowerShell’e bağlanmak için PuTTY gibi bir terminal öykünme yazılımı kullanmanız gerekir. Cihaza seri konsolu aracılığıyla doğrudan veya uzak bir bilgisayardan telnet oturumu açarak eriştiğinizde PuTTY kullanabilirsiniz.

[AZURE.INCLUDE [Use PuTTY to connect to the device serial console](../../includes/storsimple-use-putty.md)]

## Güncelleştirmeleri tarama ve güncelleştirmeleri uygulama

Cihazınızın güncelleştirilmesi 1-4 saat sürebilir. Cihazınızda güncelleştirmeleri taramak ve güncelleştirmeleri uygulamak için aşağıdaki adımları gerçekleştirin.

> [AZURE.NOTE] Veri 0 dışında bir ağ arabiriminde yapılandırılmış bir ağ geçidiniz varsa, güncelleştirmeyi yüklemeden önce Veri 2 ve Veri 3 ağ arabirimlerini devre dışı bırakmanız gerekir. **Cihazlar > Yapılandırma**’ya giderek Veri 2 ve Veri 3 arabirimlerini devre dışı bırakın. Cihaz güncelleştirildikten sonra bu arabirimleri yeniden etkinleştirmeniz gerekir.

#### Cihazınızı güncelleştirmek için
1.  Cihaz **Hızlı Başlangıç** sayfasında, **Cihazlar**’a tıklayın. Fiziksel cihazı seçin, **Bakım**’a tıklayın ve ardından **Güncelleştirmeleri Tara**’ya tıklayın.  
2.  Kullanılabilir güncelleştirmeleri taramak için bir iş oluşturulur. Güncelleştirmeler varsa, **Güncelleştirmeleri Tara**, **Güncelleştirmeleri Yükle** olarak değişir. **Güncelleştirmeleri Yükle**’ye tıklayın. Güncelleştirmeleri yüklemeden önce Veri 2 ve Veri 3’ü devre dışı bırakmanız istenebilir. Bu ağ arabirimlerini devre dışı bırakmazsanız güncelleştirmeler başarısız olabilir.
3.  Bir güncelleştirme işi oluşturulur. Güncelleştirme durumunu izlemek için **İşler**’e gidin.

    > [AZURE.NOTE] Güncelleştirme işi başladığında, durum anında yüzde 50 olarak görüntülenir. Daha sonra durum yalnızca güncelleştirme işi tamamlandıktan sonra yüzde 100 olarak değişir. Güncelleştirme süreci için gerçek zamanlı durum sağlanmaz.

4.  Cihaz başarıyla güncelleştirildikten sonra, Veri 2 ve Veri 3 ağ arabirimleri devre dışı bırakılmışsa bunları etkinleştirin.



## Bir Windows Server konağının IQN’ini alın

Windows Server 2012 çalıştıran bir Windows konağının iSCSI Tam Adını (IQN) almak için aşağıdaki adımları gerçekleştirin.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-get-iqn.md)]


## El ile yedekleme oluşturun

StorSimple cihazınızda tek bir birim için bir isteğe bağlı el ile yedekleme oluşturmak üzere Azure klasik portalında aşağıdaki adımları gerçekleştirin.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-create-manual-backup.md)]


## Sonraki adımlar

- [Sanal cihaz](storsimple-virtual-device-u2.md) yapılandırın.

- StorSimple cihazınızı yönetmek için [StorSimple Yöneticisi hizmetini](https://msdn.microsoft.com/library/azure/dn772396.aspx) kullanın.



<!---HONumber=Jun16_HO2-->


