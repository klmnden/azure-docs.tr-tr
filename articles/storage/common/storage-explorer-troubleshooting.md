---
title: Azure Depolama Gezgini sorun giderme kılavuzu | Microsoft Docs
description: Azure Depolama Gezgini için hata ayıklama tekniklerine genel bakış
services: virtual-machines
author: Deland-Han
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.openlocfilehash: 6ada4a25f24a6dcbb1ebd54daad15b37127f7a21
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65154185"
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a>Azure Depolama Gezgini sorun giderme kılavuzu

Microsoft Azure Depolama Gezgini Windows, macOS ve Linux'ta Azure depolama verileriyle kolayca çalışmanızı sağlayan bir tek başına bir uygulamadır. Uygulama, Azure, Ulusal Bulutlar ve Azure Stack üzerinde barındırılan depolama hesaplarına bağlanabilir.

Bu kılavuz depolama Gezgini'nde görülen yaygın sorunların çözümleri özetlenmektedir.

## <a name="role-based-access-control-permission-issues"></a>Rol tabanlı erişim denetimi izin sorunları

[Rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) izin kümelerini birleştirerek Azure kaynaklarının ayrıntılı erişim yönetimi sağlar _rolleri_. Depolama Gezgini'nde çalışırken RBAC almak için izlemeniz gereken bazı öneriler şunlardır.

### <a name="what-do-i-need-to-see-my-resources-in-storage-explorer"></a>Depolama Gezgini'nde Kaynaklarım görmek ne gerekiyor?

RBAC kullanarak depolama kaynaklarına erişirken sorun yaşıyorsanız, uygun roller atanmadığı için olabilir. Aşağıdaki bölümlerde, Depolama Gezgini depolama kaynaklarınıza erişmek için şu anda gerektirir. izinler açıklanmaktadır.

Uygun roller veya izinlere sahip değilseniz Azure hesap yöneticinize başvurun.

#### <a name="read-listget-storage-accounts"></a>Okuma: Depolama Hesaplarını Listele/Al

Depolama hesapları Listeleme izni olmalıdır. Bu izin, "Okuyucu" rolü tarafından atanan alabilirsiniz.

#### <a name="list-storage-account-keys"></a>Depolama Hesabı Anahtarlarını Listele

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

RBAC ile ilgili bir çözüm henüz şu anda yok. Geçici bir çözüm olarak bir SAS URI'si istek [kaynağınıza ekleme](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=linux#attach-a-service-by-using-a-shared-access-signature-sas).

## <a name="error-self-signed-certificate-in-certificate-chain-and-similar-errors"></a>Hata: Sertifika zinciri (ve benzer hatalar) otomatik olarak imzalanan sertifika

Sertifika hataları aşağıdaki durumlardan biri nedeniyle:

1. Uygulamayı "bir sunucuya (örneğin, şirket sunucunuzun) HTTPS trafiğini kesintiye, şifresini ve otomatik olarak imzalanan bir sertifika kullanarak şifreleme saydam proxy" bağlandı.
2. Aldığınız HTTPS iletilerine otomatik olarak imzalanan bir SSL sertifikası çalıştırıyorsunuzdur uygulamanın çalışıyor. Sertifikalar ekleme uygulamalara örnek olarak, virüsten koruma ve ağ trafiğini incelemesi yazılım içerir.

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
4. Otomatik olarak imzalanan sertifikaları bulun. Hangi sertifikaların otomatik olarak imzalanan emin değilseniz herhangi bir konuyu aramak `("s:")` ve veren `("i:")` aynıdır.
5. Herhangi bir otomatik olarak imzalanan sertifika bulduğunuzda, her biri için kopyalayıp her şeyi ilk ve son dahil olmak üzere **---BEGIN CERTIFICATE---** için **---END CERTIFICATE---** için yeni bir .cer dosyası.
6. Depolama Gezgini'ni açın, **Düzenle** > **SSL sertifikaları** > **sertifikaları içeri aktar**ve ardından dosya seçiciyi kullanın bulun, seçin ve oluşturduğunuz .cer dosyalarını açın.

Yukarıdaki adımları kullanarak herhangi bir otomatik olarak imzalanan sertifika bulamazsanız daha fazla yardım için geri bildirim aracı üzerinden bize ulaşın. Alternatif olarak, Depolama Gezgini ile komut satırından başlatmak seçebilirsiniz `--ignore-certificate-errors` bayrağı. Depolama Gezgini ile bu bayrağı başlatıldığında, sertifika hataları göz ardı eder.

## <a name="sign-in-issues"></a>Oturum açma sorunları

### <a name="blank-sign-in-dialog"></a>Boş oturum açma iletişim kutusu

Boş oturum açma iletişim kutuları genellikle AD FS tarafından Depolama Gezgini isteyen Elektron tarafından desteklenmeyen bir yeniden yönlendirme gerçekleştirmek için neden olur. Bu sorunu çözmek için oturum açmak için cihaz kod akış kullanmayı deneyebilirsiniz. Bunu yapmak için aşağıdaki adımları gerçekleştirin:

1. Menü: Önizleme -> "Cihaz kodunu oturum açma kullan".
2. Bağlan iletişim kutusu (ya da sol dikey çubuk Tak simgesine ya da hesabı panosunda "hesabı ekle" aracılığıyla) açın.
3. Oturum açmak için istediğiniz hangi ortamı seçin.
4. "Oturum Aç" düğmesine tıklayın.
5. Sonraki panelinde yönergeleri izleyin.

Kendiniz varsayılan tarayıcınızı farklı bir hesap zaten imzalı olduğu için kullanmak istediğiniz hesaba imzalama sorun bulursanız, şunları da yapabilirsiniz:

1. El ile özel bir oturuma tarayıcınızın bağlantıyı ve kodu kopyalayın.
2. El ile bağlantıyı ve kodu farklı bir tarayıcıya kopyalayın.

### <a name="reauthentication-loop-or-upn-change"></a>Yeniden kimlik doğrulaması döngü veya UPN değiştirme

Yeniden kimlik doğrulamanın bir döngüde olan veya hesaplarınızı birinin UPN'sini değiştirilmiştir aşağıdakileri deneyin:

1. Tüm hesapları kaldırın ve sonra Depolama Gezgini'ni kapatın.
2. Silin. Makinenizden IdentityService klasör. Windows üzerinde klasör konumundaki `C:\users\<username>\AppData\Local`. Mac ve Linux için klasör, kullanıcı dizininizin kökünde bulabilirsiniz.
3. Mac veya Linux bilgisayarda ise, işletim sistemi keystore Microsoft.Developer.IdentityService girişini silmek gerekir. Mac bilgisayarlarda, anahtar deposu "Gnome Anahtarlık" uygulamasıdır. Linux için uygulama genellikle "Kimlik Anahtarlığı" olarak adlandırılır, ancak ad dağıtımınıza bağlı olarak farklı olabilir.

### <a name="conditional-access"></a>Koşullu Erişim

Depolama Gezgini Windows 10, Linux veya Macos'ta kullanıldığında, koşullu erişim desteklenmez. Depolama Gezgini tarafından kullanılan AAD kitaplığındaki ilgili bir sınırlama nedeniyle budur.

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

* Macos'ta ise ve "kimlik doğrulaması için bekleyen üzerinden..." hiçbir oturum açma penceresi görünür iletişim kutusunda, daha sonra deneyin [adımları](#mac-keychain-errors)
* Depolama Gezgini'ni yeniden başlatın
* Kimlik doğrulama penceresi boş ise, kimlik doğrulaması iletişim kutusunu kapatmadan önce en az bir dakika bekleyin.
* Proxy ve sertifika ayarları, makine ve Depolama Gezgini için düzgün şekilde yapılandırıldığından emin olun.
* Windows üzerinde olan ve Visual Studio 2017 ile aynı makinede erişimi ve oturum açın, Visual Studio 2017'ye açmayı deneyin. Bir başarılı oturum açma işleminden sonra Visual Studio 2017, Depolama Gezgini'ni açın ve hesap panelinde hesabınızı görmeniz mümkün olması gerekir.

Bu yöntemlerin hiçbiri çalışıyorsanız [github'da bir sorun açın](https://github.com/Microsoft/AzureStorageExplorer/issues).

### <a name="missing-subscriptions-and-broken-tenants"></a>Eksik abonelikler ve bozuk kiracılar

Başarıyla oturum açtıktan sonra aboneliklerinizi alınamıyor, aşağıdaki sorun giderme yöntemleri deneyin:

* Hesabınızı beklediğiniz aboneliklerinize erişiminin olduğunu doğrulayın. Kullanmayı denemekte olduğunuz Azure ortamı için portalda oturum açarak erişiminizi doğrulayabilirsiniz.
* Doğru Azure kullanarak oturum açmış olduğundan emin olun (Azure, Azure Çin 21Vianet, Azure Almanya, Azure ABD kamu veya özel ortam) ortamı.
* Bir proxy'nin arkasındayken, Depolama Gezgini Ara sunucusunu düzgün şekilde yapılandırdığınızdan emin olun.
* Deneyin ve hesabı yeniden eklemeyi.
* Varsa bir "Daha fazla bilgi" bağlantısına bakın ve başarısız olan kiracılar için hangi hata iletileri bildirilen bakın. İle yapmanız gerekenler emin değilseniz hataya bakın, sonra kullanım için ücretsiz iletiniz [github'da bir sorun açın](https://github.com/Microsoft/AzureStorageExplorer/issues).

## <a name="cannot-remove-attached-account-or-storage-resource"></a>Ekli hesabı ya da depolama kaynak kaldırılamıyor

Bir ekli hesabı veya kullanıcı Arabirimi aracılığıyla depolama kaynağı kaldırmak bulamıyorsanız, aşağıdaki klasörleri silerek tüm ekli kaynakları el ile silebilirsiniz:

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
> Depolama Gezgini, Ara sunucu ayarlarını yapılandırmak için proxy otomatik yapılandırma dosyalarını desteklemez.

### <a name="common-solutions"></a>Yaygın çözümleri

Hala sorun yaşıyorsanız, aşağıdaki sorun giderme yöntemleri deneyin:

* Ara sunucunuz kullanmadan Internet'e bağlanabilir, depolama Gezgini'yle etkin proxy ayarları çalışır doğrulayın. Bu durumda, proxy ayarlarınız ile ilgili bir sorun olabilir. Sorunları belirlemek için proxy yöneticinizle birlikte çalışın.
* Proxy sunucusu kullanarak diğer uygulamaları beklendiği gibi çalıştığını doğrulayın.
* Kullanmayı denemekte olduğunuz Azure ortamı için portala bağlanabildiğini doğrulayın
* Hizmet uç noktalarınıza yanıtlar alabilir doğrulayın. Uç nokta URL'nizde tarayıcınıza girin. Bağlantı kurabiliyorsanız, InvalidQueryParameterValue ya da benzer XML yanıtı almanız gerekir.
* Başka biri de Depolama Gezgini proxy sunucunuz ile kullanıyorsa, bunlar bağlanabildiğini doğrulayın. Bağlantı kurabiliyorsanız, proxy sunucu yöneticinize başvurmanız gerekebilir

### <a name="tools-for-diagnosing-issues"></a>Sorunları tanılama araçları

Ağ araçları için fiddler'ı Windows gibi varsa, aşağıdaki gibi sorunları tanılamak doğrulayabilirsiniz:

* Ara sunucunuz çalışmanız gerekiyorsa, proxy sunucusu üzerinden bağlanmak için ağ aracınızı yapılandırmak zorunda kalabilirsiniz.
* Ağ, aracı tarafından kullanılan bağlantı noktası numarasını kontrol edin.
* Depolama Gezgini'nde proxy ayarları olarak yerel ana bilgisayar URL'si ve ağ aracın bağlantı noktası numarası girin. Doğru yapıldığında, yönetim ve hizmet uç noktaları için Depolama Gezgini tarafından yapılan ağ istekleri günlüğe kaydetme ağ aracınızın başlatır. Örneğin, https://cawablobgrs.blob.core.windows.net/ bir tarayıcı ve blob uç noktanız alırsınız yanıt erişim olsa da, kaynağın mevcut, önerir aşağıdaki benzer.

![Kod örneği](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### <a name="contact-proxy-server-admin"></a>Proxy Sunucu Yöneticisi ile iletişime geçin

Proxy ayarlarınız doğru ise proxy sunucu yöneticinize başvurmanız gerekebilir ve

* Proxy yönetim veya kaynak uç noktaları Azure trafiği engellemediğinden emin olun.
* Proxy sunucunuz tarafından kullanılan kimlik doğrulama protokolü doğrulayın. Depolama Gezgini şu anda NTLM proxy'leri desteklemez.

## <a name="unable-to-retrieve-children-error-message"></a>"Alma alt oluşturulamıyor" hata iletisi

Bir ara sunucu Azure'a bağlıysa, proxy ayarlarınızın doğru olduğunu doğrulayın. Bir kaynak için abonelik veya hesap sahibinden erişim verilir, okuma veya bu kaynak için izinler listesinde doğrulayın.

## <a name="connection-string-does-not-have-complete-configuration-settings"></a>Bağlantı dizesi tam yapılandırma ayarları yok

Bu hata iletisini alırsanız, depolama hesabı anahtarlarını almak için gerekli izinlere sahip değilsiniz mümkündür. Durumun bu olup olmadığını onaylamak için portala gidin ve depolama hesabınızı bulun. Hızlı depolama hesabınız için düğümde sağ tıklayıp "Portalını açın,"'i tıklatarak bunu yapabilirsiniz. Bunu yaptığınızda, "Erişim anahtarlar" dikey penceresine gidin. Ardından anahtarlarını görüntülemek için izniniz yok, "Erişim hakkınız yok" iletisi içeren bir sayfa görürsünüz. Bu sorunu geçici olarak çözmek için başka bir kişinin hesap anahtarını edinmeniz ve adı ve anahtarı ile ekleme veya SAS depolama hesabı için birisinden ve depolama hesabı eklemek için kullanın.

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

Genel olarak, Depolama Gezgini, Linux üzerinde çalıştırmak için aşağıdaki paketler gereklidir:

* [.NET core 2.0 çalışma zamanı](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x) Not: Depolama Gezgini sürüm 1.7.0 ve daha önce .NET Core 2.0 gerektirir. Yüklü .NET Core daha yeni bir sürümü varsa, düzeltme eki Depolama Gezgini (aşağıya bakın) gerekir. Depolama Gezgini 1.8.0 veya büyük ardından çalıştırıyorsanız, .NET Core 2.2 için kullanılacak mümkün olması gerekir. 2.2'den sonraki sürümlerde, şu anda çalışması için doğrulanmadı.
* `libgnome-keyring-common` ve `libgnome-keyring-dev`
* `libgconf-2-4`

Dağıtımınızda bağlı olarak, olabilir farklı veya daha fazla paketleri yüklemeniz gerekir.

Depolama Gezgini, 16.04 ve 14.04 gibi Ubuntu 18.04 üzerinde resmi olarak desteklenmektedir. Yükleme adımlarını temiz makineler için aşağıdaki gibidir:

# <a name="ubuntu-1804tab1804"></a>[Ubuntu 18.04](#tab/1804)

1. Depolama Gezgini'ni indirin
2. .NET Core çalışma zamanı yükleme, en son doğrulanmış sürüme: [2.0.8](https://dotnet.microsoft.com/download/linux-package-manager/ubuntu18-04/runtime-2.0.8) (yeni bir sürümü zaten yüklü değilse, gerekebilir Depolama Gezgini düzeltme eki aşağıya bakın)
3. `sudo apt-get install libgconf-2-4` öğesini çalıştırın
4. `sudo apt install libgnome-keyring-common libgnome-keyring-dev` öğesini çalıştırın

# <a name="ubuntu-1604tab1604"></a>[Ubuntu 16.04](#tab/1604)

1. Depolama Gezgini'ni indirin
2. .NET Core çalışma zamanı yükleme, en son doğrulanmış sürüme: [2.0.8](https://dotnet.microsoft.com/download/linux-package-manager/ubuntu16-04/runtime-2.0.8) (yeni bir sürümü zaten yüklü değilse, gerekebilir Depolama Gezgini düzeltme eki aşağıya bakın)
3. `sudo apt install libgnome-keyring-dev` öğesini çalıştırın

# <a name="ubuntu-1404tab1404"></a>[Ubuntu 14.04](#tab/1404)

1. Depolama Gezgini'ni indirin
2. .NET Core çalışma zamanı yükleme, en son doğrulanmış sürüme: [2.0.8](https://dotnet.microsoft.com/download/linux-package-manager/ubuntu14-04/runtime-2.0.8) (yeni bir sürümü zaten yüklü değilse, gerekebilir Depolama Gezgini düzeltme eki aşağıya bakın)
3. `sudo apt install libgnome-keyring-dev` öğesini çalıştırın

---

### <a name="patching-storage-explorer-for-newer-versions-of-net-core"></a>Depolama Gezgini'ni daha yeni sürümleri .NET Core için düzeltme eki uygulama 
.NET Core 2.0 yüklü ve Depolama Gezgini sürüm 1.7.0 çalışan daha büyük veya daha eski bir sürümü varsa, büyük olasılıkla Depolama Gezgini aşağıdaki adımları izleyerek düzeltme eki gerekir:
1. StreamJsonRpc 1.5.43 sürümünü indirin [nuget'ten](https://www.nuget.org/packages/StreamJsonRpc/1.5.43). Sayfanın sağ tarafındaki "paketini indirme" bağlantısına bakın.
2. Paket'ı indirdikten sonra dosya uzantısını değiştirme `.nupkg` için `.zip`
3. Paketin sıkıştırmasını açın
4. Şuraya gidin: `streamjsonrpc.1.5.43/lib/netstandard1.1/`
5. Kopyalama `StreamJsonRpc.dll` aşağıdaki konumlara Depolama Gezgini klasör içinde:
    1. `StorageExplorer/resources/app/ServiceHub/Services/Microsoft.Developer.IdentityService/`
    2. `StorageExplorer/resources/app/ServiceHub/Hosts/ServiceHub.Host.Core.CLR.x64/`

## <a name="open-in-explorer-from-azure-portal-doesnt-work"></a>Açık olarak Gezgini Azure portalı çalışmıyor

Azure portalında "Gezgini'ni açın," düğmesini sizin için işe yaramazsa, uyumlu bir tarayıcıda kullandığınızdan emin olun. Aşağıdaki tarayıcılardan uyumluluk için test edilmiştir.
* Microsoft Edge
* Mozilla Firefox
* Google Chrome
* Microsoft Internet Explorer

## <a name="next-steps"></a>Sonraki adımlar

Hiçbir çözüm, ardından çalışıyorsanız [github'da bir sorun açın](https://github.com/Microsoft/AzureStorageExplorer/issues). GitHub için sol alt köşedeki "Github'a rapor Issue" düğmesini kullanarak da hızlıca elde edebilirsiniz.

![Geri Bildirim](./media/storage-explorer-troubleshooting/feedback-button.PNG)
