---
title: Azure Depolama Gezgini sorun giderme kılavuzu | Microsoft Docs
description: Azure Depolama Gezgini için hata ayıklama tekniklerine genel bakış
services: virtual-machines
author: Deland-Han
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.openlocfilehash: fd34ab7cd899549962663e8cee8ee2121c39c49e
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840395"
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a>Azure Depolama Gezgini sorun giderme kılavuzu

Microsoft Azure Depolama Gezgini Windows, macOS ve Linux'ta Azure depolama verileriyle kolayca çalışmanızı sağlayan bir tek başına bir uygulamadır. Uygulama, Azure, Ulusal Bulutlar ve Azure Stack üzerinde barındırılan depolama hesaplarına bağlanabilir.

Bu kılavuz depolama Gezgini'nde görülen yaygın sorunların çözümleri özetlenmektedir.

## <a name="role-based-access-control-permission-issues"></a>Rol tabanlı erişim denetimi izin sorunları

[Rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) izin kümelerini birleştirerek Azure kaynaklarının ayrıntılı erişim yönetimi sağlar _rolleri_. Depolama Gezgini'nde çalışırken RBAC almak için izlemeniz gereken bazı öneriler şunlardır.

### <a name="what-do-i-need-to-see-my-resources-in-storage-explorer"></a>Depolama Gezgini'nde Kaynaklarım görmek ne gerekiyor?

RBAC kullanarak depolama kaynaklarına erişirken sorun yaşıyorsanız, uygun roller atanmadığı için olabilir. Aşağıdaki bölümlerde, Depolama Gezgini depolama kaynaklarınıza erişmek için şu anda gerektirir. izinler açıklanmaktadır.

Uygun roller veya izinlere sahip değilseniz Azure hesap yöneticinize başvurun.

#### <a name="read-listget-storage-accounts"></a>Okuma: Depolama hesapları listesinde/Al

Depolama hesapları Listeleme izni olmalıdır. Bu izin, "Okuyucu" rolü tarafından atanan alabilirsiniz.

#### <a name="list-storage-account-keys"></a>Depolama hesabı anahtarlarını Listele

Depolama Gezgini isteklerinin kimliğini doğrulamak için hesap anahtarları da kullanabilirsiniz. "Katılımcı" rolü gibi daha güçlü rolleriyle anahtarlarına erişim elde edebilirsiniz.

> [!NOTE]
> Erişim anahtarlarını bunları tutan herkese sınırsız izinleri verin. Bu nedenle, bu genellikle bunlar. Bu sayede hesap kullanıcıları için önerilmez. Erişim anahtarlarını iptali gerekiyorsa, bunları yeniden oluşturabilirsiniz [Azure portalı](https://portal.azure.com/).

#### <a name="data-roles"></a>Veri rolleri

Veri kaynaklarına okuma erişimi verir en az bir rol atanması gerekir. Örneğin, liste veya blobları indirmek gerekiyorsa, en az "Depolama Blob verileri okuyucu" rolünü gerekir.

### <a name="why-do-i-need-a-management-layer-role-to-see-my-resources-in-storage-explorer"></a>Depolama Gezgini'nde Kaynaklarım görmek için bir yönetim katmanı rolü neden gerekiyor?

Azure depolama alanına sahip iki erişim katmanı: _Yönetim_ ve _veri_. Abonelikleri ve depolama hesaplarını yönetim katmanı erişilir. Kapsayıcıları, blobları ve diğer veri kaynakları, veri katmanı erişilir. Örneğin, Azure depolama hesaplarınızı listesini almak istiyorsanız, yönetim uç noktasına bir istek gönderin. Bir hesapta blob kapsayıcıları listesi istiyorsanız, uygun bir hizmet uç noktaya bir istek gönderin.

RBAC rollerini yönetim veya veri katmanı erişim izinlerini içerebilir. "Okuyucu" rolünü, örneğin, yönetim katmanı kaynakları salt okunur erişim verir.

NET olarak söylemek gerekirse, "Okuyucu" rolünü hiçbir veri katmanı izinleri sağlar ve veri katmanı erişmek için gerekli değildir.

Depolama Gezgini, Azure kaynaklarınıza bağlanmak için gereken bilgileri toplamak yoluyla kaynaklarınıza erişmek kolaylaştırır. Örneğin, blob kapsayıcıları görüntülemek için blob Hizmeti uç noktası için bir liste kapsayıcıları isteği Depolama Gezgini gönderir. Bu uç noktayı almak üzere aboneliklerin listesi, Depolama Gezgini arar ve depolama hesapları erişebilirsiniz. Ancak, abonelikleri ve depolama hesaplarını bulmak için Depolama Gezgini'ni de yönetim katmanı erişmesi.

Herhangi bir yönetim katmanı izinleri verme rol yoksa, Depolama Gezgini, verileri katmana bağlanmak için gereken bilgileri alınamıyor.

### <a name="what-if-i-cant-get-the-management-layer-permissions-i-need-from-my-administrator"></a>Yönetim katmanı izinleri ne alamıyorum my yöneticisinden gerekiyor?

RBAC ile ilgili bir çözüm henüz şu anda yok. Geçici bir çözüm olarak bir SAS URI'si istek [kaynağınıza ekleme](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=linux#use-a-sas-uri).

## <a name="error-self-signed-certificate-in-certificate-chain-and-similar-errors"></a>Hata: Sertifika zinciri (ve benzer hatalar) otomatik olarak imzalanan sertifika

Sertifika hataları aşağıdaki durumlardan biri nedeniyle:

1. Uygulamayı "bir sunucuya (örneğin, şirket sunucunuzun) HTTPS trafiğini kesintiye, şifresini ve otomatik olarak imzalanan bir sertifika kullanarak şifreleme saydam proxy" bağlandı.
2. Aldığınız HTTPS iletilerine otomatik olarak imzalanan bir SSL sertifikası çalıştırıyorsunuzdur bir uygulaması çalıştırıyorsunuz. Sertifikalar ekleme uygulamalara örnek olarak, virüsten koruma ve ağ trafiğini incelemesi yazılım içerir.

Depolama Gezgini otomatik olarak imzalanan veya güvenilmeyen bir sertifika gördüğünde artık alınan aldığı HTTPS iletisinin değiştirilmiş olup olmadığını bilebilirsiniz. Otomatik olarak imzalanan sertifikanın bir kopyasını varsa, Depolama Gezgini bildirebilirsiniz aşağıdaki adımları uygulayarak güven:

1. Elde Base-64 kodlanmış X.509 (.cer) sertifikanın kopyasını
2. Tıklayın **Düzenle** > **SSL sertifikaları** > **sertifikaları içeri aktar**ve ardından bulmak, seçmek ve .cer dosyasını açmak için dosya seçiciyi kullanın

Bu sorun ayrıca birden çok sertifika (kök ve Ara) sonucu olabilir. Hatayı gidermek için her iki sertifikanın yeniden eklenmesi gerekir.

Nereden sertifika geldiğini emin değilseniz bulmak için aşağıdaki adımları deneyebilirsiniz:

1. Açık SSL yükleme
    * [Windows](https://slproweb.com/products/Win32OpenSSL.html) (Basit sürümlerden birini yeterli olmalıdır)
    * Mac ve Linux: işletim sisteminize eklenmelidir
2. Açık SSL çalıştırma
    * Windows: yükleme dizinini açın, **/bin/** ve çift tıklatarak **openssl.exe**.
    * Mac ve Linux: çalıştırma **openssl** bir terminalden.
3. `s_client -showcerts -connect microsoft.com:443` yürütme
4. Otomatik olarak imzalanan sertifikaları bulun. Hangi sertifikaların otomatik olarak imzalanan emin değilseniz, herhangi bir konuyu aramak `("s:")` ve veren `("i:")` aynıdır.
5. Herhangi bir otomatik olarak imzalanan sertifika bulduğunuzda, her biri için kopyalayıp her şeyi ilk ve son dahil olmak üzere **---BEGIN CERTIFICATE---** için **---END CERTIFICATE---** için yeni bir .cer dosyası.
6. Depolama Gezgini'ni açın, **Düzenle** > **SSL sertifikaları** > **sertifikaları içeri aktar**ve ardından dosya seçiciyi kullanın bulun, seçin ve oluşturduğunuz .cer dosyalarını açın.

Yukarıdaki adımları kullanarak herhangi bir otomatik olarak imzalanan sertifika bulamazsanız daha fazla yardım için geri bildirim aracı üzerinden bize ulaşın. Depolama Gezgini ile komut satırından başlatmak de seçebilirsiniz `--ignore-certificate-errors` bayrağı. Depolama Gezgini ile bu bayrağı başlatıldığında, sertifika hataları göz ardı eder.

## <a name="sign-in-issues"></a>Oturum açma sorunları

### <a name="blank-sign-in-dialog"></a>Boş oturum açma iletişim kutusu

Boş oturum açma iletişim kutuları genellikle AD FS tarafından Depolama Gezgini isteyen Elektron tarafından desteklenmeyen bir yeniden yönlendirme gerçekleştirmek için neden olur. Bu sorunu çözmek için oturum açmak için cihaz kod akış kullanma girişimi. Bunu yapmak için aşağıdaki adımları tamamlayın:

1. Menü: Önizleme -> "Cihaz kodunu oturum açma kullan".
2. Bağlan iletişim kutusu (ya da sol dikey çubuk Tak simgesine ya da hesabı panosunda "hesabı ekle" aracılığıyla) açın.
3. Oturum açmak için istediğiniz hangi ortamı seçin.
4. "Oturum Aç" düğmesine tıklayın.
5. Sonraki panelinde yönergeleri izleyin.

Kendiniz varsayılan tarayıcınızı farklı bir hesap zaten imzalı olduğu için kullanmak istediğiniz hesaba imzalama sorun bulursanız, şunları da yapabilirsiniz:

1. El ile özel bir oturuma tarayıcınızın bağlantıyı ve kodu kopyalayın.
2. El ile bağlantıyı ve kodu farklı bir tarayıcıya kopyalayın.

### <a name="reauthentication-loop-or-upn-change"></a>Yeniden kimlik doğrulaması döngü veya UPN değiştirme

Yeniden kimlik doğrulamanın bir döngüde olduğunuz veya hesaplarınızı birinin UPN'sini değiştirilmiştir, aşağıdaki adımları deneyin:

1. Tüm hesapları kaldırın ve sonra Depolama Gezgini'ni kapatın.
2. Silin. Makinenizden IdentityService klasör. Windows üzerinde klasör konumundaki `C:\users\<username>\AppData\Local`. Mac ve Linux için klasör, kullanıcı dizininizin kökünde bulabilirsiniz.
3. Mac veya Linux üzerinde yöneticisiyseniz, Microsoft.Developer.IdentityService giriş, işletim sistemi keystore silmek gerekir. Mac bilgisayarlarda, anahtar deposu "Gnome Anahtarlık" uygulamasıdır. Linux için uygulama genellikle "Kimlik Anahtarlığı" olarak adlandırılır, ancak ad dağıtımınıza bağlı olarak farklı olabilir.

### <a name="conditional-access"></a>Koşullu Erişim

Depolama Gezgini Windows 10, Linux veya Macos'ta kullanıldığında, koşullu erişim desteklenmez. Depolama Gezgini tarafından kullanılan AAD Kitaplığı'nda bir sınırlama nedeniyle budur.

## <a name="mac-keychain-errors"></a>Mac Keychain hataları

MacOS Anahtarlık bir duruma neden olan sorunları Storage Explorer'ın kimlik doğrulama kitaplığı için bazen alabilirsiniz. Bu durum dışında Anahtarlık almak için aşağıdaki adımları deneyin:

1. Depolama Gezgini'ni kapatın.
2. Açık Anahtarlık (**cmd + Ara çubuğu**yazın Anahtarlıkta, isabet girin).
3. "Login" anahtar zinciri seçin.
4. Asma kilit simgesini (asma kilide kilitli bir konuma tamamlandıktan sonra hangi uygulamaları elinizde açın bağlı olarak birkaç saniye sürebilir animasyon uygular) Anahtarlık kilitlemek için tıklayın.

    ![image](./media/storage-explorer-troubleshooting/unlockingkeychain.png)

5. Depolama Gezgini'ni başlatın.
6. Bir şey "hizmet hub'ı Anahtarlık erişim istediği gibi" ifadesini içeren bir açılır pencere görünmelidir. Ne zaman, Mac yönetici parolasını girin ve tıklatın **her zaman izin ver** (veya **izin** varsa **her zaman izin ver** kullanılamaz).
7. Oturum açmayı deneyin.

### <a name="general-sign-in-troubleshooting-steps"></a>Genel oturum açma sorun giderme adımları

* MacOS üzerinde olduğunuzu ve "kimlik doğrulaması için bekleyen üzerinden..." hiçbir oturum açma penceresi görünür iletişim kutusunda, daha sonra deneyin [adımları](#mac-keychain-errors)
* Depolama Gezgini'ni yeniden başlatın
* Kimlik doğrulama penceresi boş ise, kimlik doğrulaması iletişim kutusunu kapatmadan önce en az bir dakika bekleyin.
* Proxy ve sertifika ayarları, makine ve Depolama Gezgini için düzgün şekilde yapılandırıldığından emin olun.
* Windows üzerinde olduğunuz ve oturum açma ve aynı makineye Visual Studio 2019 erişimi varsa, Visual Studio 2019'için oturum açarken deneyin. Bir başarılı oturum açma işleminden sonra Visual Studio 2019 için Depolama Gezgini'ni açın ve hesabınız hesap panelinde bakın.

Bu yöntemlerin hiçbiri çalışıyorsanız [github'da bir sorun açın](https://github.com/Microsoft/AzureStorageExplorer/issues).

### <a name="missing-subscriptions-and-broken-tenants"></a>Eksik abonelikler ve bozuk kiracılar

Başarıyla oturum açtıktan sonra aboneliklerinizi alınamıyor, aşağıdaki sorun giderme yöntemleri deneyin:

* Hesabınızı beklediğiniz aboneliklerinize erişiminin olduğunu doğrulayın. Kullanmaya çalıştığınız Azure ortamı için portalda oturum açarak erişiminizi doğrulayabilirsiniz.
* Doğru Azure kullanarak oturum açmış emin olun (Azure, Azure Çin 21Vianet, Azure Almanya, Azure ABD kamu veya özel ortam) ortamı.
* Bir ara sunucunun ardından değilseniz, Depolama Gezgini Ara sunucusunu düzgün şekilde yapılandırdığınızdan emin olun.
* Deneyin ve hesabı yeniden eklemeyi.
* "Daha fazla bilgi" bağlantısını varsa, arayın ve başarısız olan kiracılar için hangi hata iletileri bildirilen bakın. Varsa you'ren't emin hatasıyla yapmanız gerekenler bakın, sonra kullanım için ücretsiz iletiniz [github'da bir sorun açın](https://github.com/Microsoft/AzureStorageExplorer/issues).

## <a name="cant-remove-attached-account-or-storage-resource"></a>Ekli hesabı ya da depolama kaynak kaldırılamıyor

Bir ekli hesabı veya kullanıcı Arabirimi aracılığıyla depolama kaynağı kaldırmak zamanınız yoksa aşağıdaki klasörleri silerek tüm ekli kaynakları el ile silebilirsiniz:

* Windows: `%AppData%/StorageExplorer`
* macOS: `/Users/<your_name>/Library/Application Support/StorageExplorer`
* Linux: `~/.config/StorageExplorer`

> [!NOTE]
> Depolama Gezgini, yukarıdaki klasörleri silmeden önce kapatın.

> [!NOTE]
> Şimdiye kadar tüm SSL sertifikaları içe aktardıktan sonra içeriğini yedekleme `certs` dizin. Daha sonra yedekleme, SSL sertifikalarınızı XML'den için kullanabilirsiniz.

## <a name="proxy-issues"></a>Proxy sorunları

İlk olarak, girdiğiniz aşağıdaki bilgilerin doğru olduğundan emin olun:

* Proxy URL'si ve bağlantı noktası numarası
* Kullanıcı adı ve parola tarafından proxy gerekliyse

> [!NOTE]
> Depolama Gezgini Ara sunucu ayarlarını yapılandırmak için proxy otomatik yapılandırma dosyalarını desteklemez.

### <a name="common-solutions"></a>Yaygın çözümleri

Hala sorun yaşıyorsanız, aşağıdaki sorun giderme yöntemleri deneyin:

* Ara sunucunuz kullanmadan Internet'e bağlanabilir, depolama Gezgini'yle etkin proxy ayarları çalışır doğrulayın. Bu durumda, proxy ayarlarınız ile ilgili bir sorun olabilir. Sorunları belirlemek için proxy yöneticinizle birlikte çalışın.
* Proxy sunucusu kullanarak diğer uygulamaları beklendiği gibi çalıştığını doğrulayın.
* Kullanmaya çalıştığınız Azure ortamı için portala bağlanabildiğini doğrulayın
* Hizmet uç noktalarınıza yanıtlar alabilir doğrulayın. Uç nokta URL'nizde tarayıcınıza girin. Bağlantı kurabiliyorsanız, InvalidQueryParameterValue ya da benzer XML yanıtı almanız gerekir.
* Başka biri de Depolama Gezgini proxy sunucunuz ile kullanıyorsa, bunlar bağlanabildiğini doğrulayın. Bağlantı kurabiliyorsanız, proxy sunucu yöneticinize başvurmanız gerekebilir

### <a name="tools-for-diagnosing-issues"></a>Sorunları tanılama araçları

Ağ araçları için fiddler'ı Windows gibi varsa, aşağıdaki gibi sorunları tanılayabilirsiniz:

* Ara sunucunuz çalışmanız gerekiyorsa, proxy sunucusu üzerinden bağlanmak için ağ aracınızı yapılandırmak zorunda kalabilirsiniz.
* Ağ, aracı tarafından kullanılan bağlantı noktası numarasını kontrol edin.
* Depolama Gezgini'nde proxy ayarları olarak yerel ana bilgisayar URL'si ve ağ aracın bağlantı noktası numarası girin. Doğru yapıldığında, yönetim ve hizmet uç noktaları için Depolama Gezgini tarafından yapılan ağ istekleri günlüğe kaydetme ağ aracınızın başlatır. Örneğin, https://cawablobgrs.blob.core.windows.net/ bir tarayıcı ve blob uç noktanız alırsınız için bir yanıt erişim olsa da, kaynağın mevcut, önerir aşağıdaki benzer.

![Kod örneği](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### <a name="contact-proxy-server-admin"></a>Proxy Sunucu Yöneticisi ile iletişime geçin

Proxy ayarlarınız doğru ise proxy sunucu yöneticinize başvurmanız gerekebilir ve

* Proxy yönetim veya kaynak uç noktaları Azure trafiği engellemez emin olun.
* Proxy sunucunuz tarafından kullanılan kimlik doğrulama protokolü doğrulayın. Depolama Gezgini şu anda NTLM proxy'leri desteklemez.

## <a name="unable-to-retrieve-children-error-message"></a>"Alma alt oluşturulamıyor" hata iletisi

Azure'a bir ara sunucu bağlı değilseniz, proxy ayarlarının doğru olduğunu doğrulayın. Bir kaynağa erişim abonelik veya hesap sahibinden izni okuma veya bu kaynak için izinler listesinde doğrulayın.

## <a name="connection-string-doesnt-have-complete-configuration-settings"></a>Bağlantı dizesi değil tam yapılandırma ayarları

Bu hata iletisini alırsanız, depolama hesabı anahtarlarını almak için gerekli izinlere sahip değilsiniz mümkündür. Durumun bu olup olmadığını onaylamak için portala gidin ve depolama hesabınızı bulun. Hızlı depolama hesabınız için düğümde sağ tıklayıp "Portalını açın,"'i tıklatarak bunu yapabilirsiniz. Bunu yaptığınızda, "Erişim anahtarlar" dikey penceresine gidin. Ardından anahtarları görüntüleme izinlerine sahip değilseniz, "Erişiminiz yoksa" iletisini içeren bir sayfa görürsünüz. Bu sorunu geçici olarak çözmek için başka bir kişinin hesap anahtarını edinmeniz ve adı ve anahtarı ile ekleme veya SAS depolama hesabı için birisinden ve depolama hesabı eklemek için kullanın.

Hesap anahtarlarını görürseniz, biz sorunu gidermenize yardımcı olabilmemiz için Github'da bir sorun kaydedebilir.

## <a name="issues-with-sas-url"></a>SAS URL ile ilgili sorunlar

Bir SAS URL'sini kullanarak ve bu hatanın bir hizmeti bağlanıyorsanız:

* URL okuma veya kaynakları listelemek için gerekli izinleri sağladığından emin olun.
* URL sona ermediğinden emin olun.
* SAS URL'sini bir erişim ilkesi alıyorsa, erişim ilkesi edilmediğini doğrulayın.

Yanlışlıkla geçersiz bir SAS URL'si kullanarak bağlı ve ayırma belirleyemiyoruz, şu adımları izleyin:

1. Depolama Gezgini çalıştırırken, geliştirici araçları penceresini açmak için F12 tuşuna basın.
2. Uygulama sekmesini tıklatın ve ardından yerel depolama tıklayın > file:// soldaki ağaç.
3. Sorunlu SAS URI'sini hizmet türü ile ilişkili anahtar bulun. Bozuk bir blob kapsayıcısı için SAS URI'si ise, örneğin, adlı anahtar için konum `StorageExplorer_AddStorageServiceSAS_v1_blob`.
4. Anahtar değerini bir JSON dizisi olmalıdır. Bozuk URI'sı ile ilişkili nesneyi bulmak ve kaldırın.
5. Depolama Gezgini'ni yeniden yüklemek için CTRL + R tuşuna basın.

## <a name="linux-dependencies"></a>Linux bağımlılıkları

<!-- Storage Explorer 1.9.0 and later is available as a snap from the Snap Store. The Storage Explorer snap installs all of its dependencies with no extra hassle.

Storage Explorer requires the use of a password manager, which may need to be connected manually before Storage Explorer will work correctly. You can connect Storage Explorer to your system's password manager with the following command:

```bash
snap connect storage-explorer:password-manager-service :password-manager-service
```

You can also download the application .tar.gz file, but you'll have to install dependencies manually. -->

> [!IMPORTANT]
> Sağlanan Depolama Gezgini. tar.gz indirme yalnızca Ubuntu dağıtımları için desteklenir. Diğer dağıtımları değil doğruladıktan ve farklı veya ek paketleri gerektirebilir.

Bu paketleri, Linux üzerinde Depolama Gezgini için en yaygın gereksinimler verilmiştir:

* [.NET core 2.0 çalışma zamanı](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x)
* `libgconf-2-4`
* `libgnome-keyring0` veya `libgnome-keyring-dev`
* `libgnome-keyring-common`

> [!NOTE]
> Depolama Gezgini sürüm 1.7.0 ve daha önce .NET Core 2.0 gerektirir. .NET Core yüklü bir sürüme sahip sonra yapmanız gerekir [Depolama Gezgini düzeltme eki](#patching-storage-explorer-for-newer-versions-of-net-core). Depolama Gezgini 1.8.0 veya büyük ardından çalıştırıyorsanız, .NET Core 2.2 için kullanılacak mümkün olması gerekir. 2\.2'den sonraki sürümlerde, şu anda çalışması için doğrulanmadı.

# <a name="ubuntu-1904tab1904"></a>[Ubuntu 19.04](#tab/1904)

1. Storage Explorer'ı indirin.
2. Yükleme [.NET Core çalışma zamanı](https://dotnet.microsoft.com/download/linux-package-manager/ubuntu19-04/runtime-current).
3. Şu komutu çalıştırın:
   ```bash
   sudo apt-get install libgconf-2-4 libgnome-keyring0
   ```

# <a name="ubuntu-1804tab1804"></a>[Ubuntu 18.04](#tab/1804)

1. Storage Explorer'ı indirin.
2. Yükleme [.NET Core çalışma zamanı](https://dotnet.microsoft.com/download/linux-package-manager/ubuntu18-04/runtime-current).
3. Şu komutu çalıştırın:
   ```bash
   sudo apt-get install libgconf-2-4 libgnome-keyring-common libgnome-keyring0
   ```

# <a name="ubuntu-1604tab1604"></a>[Ubuntu 16.04](#tab/1604)

1. Depolama Gezgini'ni indirin
2. Yükleme [.NET Core çalışma zamanı](https://dotnet.microsoft.com/download/linux-package-manager/ubuntu16-04/runtime-current).
3. Şu komutu çalıştırın:
   ```bash
   sudo apt install libgnome-keyring-dev
   ```

# <a name="ubuntu-1404tab1404"></a>[Ubuntu 14.04](#tab/1404)

1. Depolama Gezgini'ni indirin
2. Yükleme [.NET Core çalışma zamanı](https://dotnet.microsoft.com/download/linux-package-manager/ubuntu14-04/runtime-current).
3. Şu komutu çalıştırın:
   ```bash
   sudo apt install libgnome-keyring-dev
   ```

### <a name="patching-storage-explorer-for-newer-versions-of-net-core"></a>Depolama Gezgini'ni daha yeni sürümleri .NET Core için düzeltme eki uygulama

Depolama Gezgini 1.7.0 veya daha eski düzeltme eki sürümü, Depolama Gezgini tarafından kullanılan .NET Core'da gerekebilir.

1. StreamJsonRpc 1.5.43 sürümünü indirin [nuget'ten](https://www.nuget.org/packages/StreamJsonRpc/1.5.43). Sayfanın sağ tarafındaki "paketini indirme" bağlantısına bakın.
2. Paket'ı indirdikten sonra dosya uzantısını değiştirme `.nupkg` için `.zip`.
3. Paketin sıkıştırmasını açın.
4. Açık `streamjsonrpc.1.5.43/lib/netstandard1.1/` klasör.
5. Kopyalama `StreamJsonRpc.dll` aşağıdaki konumlara Depolama Gezgini klasör içinde:
   * `StorageExplorer/resources/app/ServiceHub/Services/Microsoft.Developer.IdentityService/`
   * `StorageExplorer/resources/app/ServiceHub/Hosts/ServiceHub.Host.Core.CLR.x64/`

## <a name="open-in-explorer-from-azure-portal-doesnt-work"></a>Açık olarak Gezgini Azure portalı çalışmıyor

Azure portalında "Gezgini'ni açın," düğmesini sizin için işe yaramazsa, uyumlu bir tarayıcıda kullandığınızdan emin olun. Aşağıdaki tarayıcılardan uyumluluk için test edilmiştir.
* Microsoft Edge
* Mozilla Firefox
* Google Chrome
* Microsoft Internet Explorer

## <a name="next-steps"></a>Sonraki adımlar

Hiçbir çözüm, ardından çalışıyorsanız [github'da bir sorun açın](https://github.com/Microsoft/AzureStorageExplorer/issues). GitHub için sol alt köşedeki "Github'a rapor Issue" düğmesini kullanarak da hızlıca elde edebilirsiniz.

![Geri Bildirim](./media/storage-explorer-troubleshooting/feedback-button.PNG)
