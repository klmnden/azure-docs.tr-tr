---
title: Azure Depolama Gezgini sorun giderme kılavuzu | Microsoft Docs
description: Hata ayıklama özelliği Azure iki genel bakış
services: virtual-machines
author: Deland-Han
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.component: common
ms.openlocfilehash: 4f0558f9619aa06557cf89e885154f6326d4b150
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51281787"
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a>Azure Depolama Gezgini sorun giderme kılavuzu

Microsoft Azure Depolama Gezgini Windows, macOS ve Linux'ta Azure depolama verileriyle kolayca çalışmanızı sağlayan bir tek başına bir uygulamadır. Uygulama, Azure, Ulusal Bulutlar ve Azure Stack üzerinde barındırılan depolama hesaplarına bağlanabilir.

Bu kılavuz depolama Gezgini'nde görülen yaygın sorunların çözümleri özetlenmektedir.

## <a name="error-self-signed-certificate-in-certificate-chain-and-similar-errors"></a>Hata: Otomatik olarak imzalanan sertifika, sertifika zinciri (ve benzer hatalar)

Sertifika hataları aşağıdaki durumlardan biri nedeniyle:

1. Uygulamayı "bir sunucuya (örneğin, şirket sunucunuzun) HTTPS trafiğini kesintiye, şifresini ve otomatik olarak imzalanan bir sertifika kullanarak şifreleme saydam proxy" bağlandı.
2. Aldığınız HTTPS iletilerine otomatik olarak imzalanan bir SSL sertifikası çalıştırıyorsunuzdur uygulamanın çalışıyor. Sertifikalar ekleme uygulamalara örnek olarak, virüsten koruma ve ağ trafiğini incelemesi yazılım içerir.

Depolama Gezgini imzalı bir kendi kendini gördüğünde veya güvenilmeyen sertifika artık alınan aldığı HTTPS iletisinin değiştirilmiş olup olmadığını bilebilirsiniz. Otomatik olarak imzalanan sertifikanın bir kopyasını varsa, Depolama Gezgini bildirebilirsiniz aşağıdaki adımları uygulayarak güven:

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
4. Otomatik olarak imzalanan sertifikaları bulun. Kendinden imzalı olduğu emin değilseniz, herhangi bir konu arayın `("s:")` ve veren `("i:")` aynıdır.
5. Herhangi bir otomatik olarak imzalanan sertifika bulduğunuzda, her biri için kopyalayıp her şeyi ilk ve son dahil olmak üzere **---BEGIN CERTIFICATE---** için **---END CERTIFICATE---** için yeni bir .cer dosyası.
6. Depolama Gezgini'ni açın, **Düzenle** > **SSL sertifikaları** > **sertifikaları içeri aktar**ve ardından dosya seçiciyi kullanın bulun, seçin ve oluşturduğunuz .cer dosyalarını açın.

Yukarıdaki adımları kullanarak herhangi bir otomatik olarak imzalanan sertifika bulamazsanız daha fazla yardım için geri bildirim aracı üzerinden bize ulaşın. Alternatif olarak, Depolama Gezgini ile komut satırından başlatmak seçebilirsiniz `--ignore-certificate-errors` bayrağı. Depolama Gezgini ile bu bayrağı başlatıldığında, sertifika hataları göz ardı eder.

## <a name="sign-in-issues"></a>Oturum açma sorunları

### <a name="reauthentication-loop-or-upn-change"></a>Yeniden kimlik doğrulaması döngü veya UPN değiştirme
Yeniden kimlik doğrulamanın bir döngüde olan veya hesaplarınızı birinin UPN'sini değiştirilmiştir aşağıdakileri deneyin:
1. Tüm hesapları kaldırın ve sonra Depolama Gezgini'ni kapatın.
2. Silin. Makinenizden IdentityService klasör. Windows üzerinde klasör konumundaki `C:\users\<username>\AppData\Local`. Mac ve Linux için klasör, kullanıcı dizininizin kökünde bulabilirsiniz.
3. Mac veya Linux bilgisayarda ise, işletim sistemi keystore Microsoft.Developer.IdentityService girişini silmek gerekir. Mac bilgisayarlarda, anahtar deposu "Gnome Anahtarlık" uygulamasıdır. Linux için uygulama genellikle "Kimlik Anahtarlığı" olarak adlandırılır, ancak ad dağıtımınıza bağlı olarak farklı olabilir.

### <a name="conditional-access"></a>Koşullu Erişim
Depolama Gezgini Windows 10, Linux veya Macos'ta kullanıldığında, koşullu erişim desteklenmez. Depolama Gezgini tarafından kullanılan AAD kitaplığındaki ilgili bir sınırlama nedeniyle budur.

## <a name="mac-keychain-errors"></a>Mac Keychain hataları
MacOS Anahtarlık bir duruma neden olan sorunları Storage Explorer'ın kimlik doğrulama kitaplığı için bazen alabilirsiniz. Aşağıdaki adımlar bu durumu deneyin dışında Anahtarlık almak için:
1. Depolama Gezgini'ni kapatın.
2. Açık Anahtarlık (**cmd + Ara çubuğu**yazın Anahtarlıkta, isabet girin).
3. "Login" anahtar zinciri seçin.
4. Asma kilit simgesini (asma kilide kilitli bir konuma tamamlandıktan sonra hangi uygulamaları elinizde açın bağlı olarak birkaç saniye sürebilir animasyon uygular) Anahtarlık kilitlemek için tıklayın.

    ![image](./media/storage-explorer-troubleshooting/unlockingkeychain.png)

5. Depolama Gezgini'ni başlatın.
6. Bir şey "hizmet hub'ı Anahtarlık erişim istediği gibi" ifadesini içeren bir açılır pencere görünmelidir. Ne zaman, Mac yönetici parolasını girin ve tıklatın **her zaman izin ver** (veya **izin** varsa **her zaman izin ver** kullanılamaz).
7. Oturum açmayı deneyin.

### <a name="general-sign-in-troubleshooting-steps"></a>Genel oturum açma sorun giderme adımları
* MacOS üzerinde olan ve oturum açma penceresinde hiçbir zaman "... kimlik doğrulaması için bekleniyor" iletişim kutusu görüntülenir, daha sonra deneyin [adımları](#mac-keychain-errors)
* Depolama Gezgini'ni yeniden başlatın
* Kimlik doğrulama penceresi boş ise, kimlik doğrulaması iletişim kutusunu kapatmadan önce en az bir dakika bekleyin.
* Proxy ve sertifika ayarları, makine ve Depolama Gezgini için düzgün şekilde yapılandırıldığından emin olun.
* Windows üzerinde olan ve Visual Studio 2017 erişiminiz aynı makine ve oturum açma, Visual Studio 2017'ye açmayı deneyin. Bir başarılı oturum açma işleminden sonra Visual Studio 2017, Depolama Gezgini'ni açın ve hesap panelinde hesabınızı görmeniz mümkün olması gerekir. 

Bu yöntemlerin hiçbiri çalışıyorsanız [github'da bir sorun açın](https://github.com/Microsoft/AzureStorageExplorer/issues).

### <a name="missing-subscriptions-and-broken-tenants"></a>Eksik abonelikler ve bozuk kiracılar

Başarıyla oturum açtıktan sonra aboneliklerinizi alınamıyor, aşağıdaki sorun giderme yöntemleri deneyin:

* Hesabınızı beklediğiniz aboneliklerinize erişiminin olduğunu doğrulayın. Kullanmayı denemekte olduğunuz Azure ortamı için portalda oturum açarak erişimi doğrulayabilirsiniz.
* Doğru Azure kullanarak oturum açmış olduğundan emin olun (Azure, Azure Çin'de, Azure Almanya, Azure ABD kamu veya özel ortam) ortamı.
* Bir proxy'nin arkasındayken, Depolama Gezgini Ara sunucusunu düzgün şekilde yapılandırdığınızdan emin olun.
* Deneyin ve hesabı yeniden eklemeyi.
* Varsa bir "Daha fazla bilgi" bağlantısına bakın ve başarısız olan kiracılar için hangi hata iletileri bildirilen bakın. İle yapmanız gerekenler emin değilseniz hataya bakın, sonra kullanım için ücretsiz iletiniz [github'da bir sorun açın](https://github.com/Microsoft/AzureStorageExplorer/issues).

## <a name="cannot-remove-attached-account-or-storage-resource"></a>Ekli hesabı ya da depolama kaynak kaldırılamıyor

Bir ekli hesabı veya kullanıcı Arabirimi aracılığıyla depolama kaynağı kaldırmak bulamıyorsanız, aşağıdaki klasörleri silerek tüm ekli kaynakları el ile silebilirsiniz:

* Windows: `%AppData%/StorageExplorer`
* macOS: `/Users/<your_name>/Library/Applicaiton Support/StorageExplorer`
* Linux: `~/.config/StorageExplorer`

> [!NOTE]
>  Depolama Gezgini, yukarıdaki klasörleri silmeden önce kapatın.

> [!NOTE]
>  Şimdiye kadar tüm SSL sertifikaları içe aktardıktan sonra içeriğini yedekleme `certs` dizin. Daha sonra yedekleme, SSL sertifikalarınızı XML'den için kullanabilirsiniz.

## <a name="proxy-issues"></a>Proxy sorunları

İlk olarak, girdiğiniz aşağıdaki bilgilerin doğru olduğundan emin olun:

* Proxy URL'si ve bağlantı noktası numarası
* Kullanıcı adı ve parola tarafından proxy gerekliyse

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

Bir ara sunucu Azure'a bağlıysa, proxy ayarlarınızın doğru olduğunu doğrulayın. Bir kaynak için abonelik veya hesap sahibinden erişim verilmiş, okuma veya bu kaynak için izinler listesinde doğrulayın.

## <a name="connection-string-does-not-have-complete-configuration-settings"></a>Bağlantı dizesi tam yapılandırma ayarları yok

Bu hata iletisini alırsanız, depolama hesabı anahtarlarını almak için gerekli izinlere sahip değilsiniz mümkündür. Durumun bu olup olmadığını onaylamak için portala gidin ve depolama hesabınızı bulun. Hızlı depolama hesabınız için bir düğüme sağ tıklayın ve "Portalını açın,"'i tıklatarak bunu yapabilirsiniz. Bunu yaptığınızda, "Erişim anahtarlar" dikey penceresine gidin. Anahtarlarını görüntülemek için izinleri yoksa, "Erişim hakkınız yok" iletisi içeren bir sayfa görürsünüz. Geçici çözüm bu sorunu ya da hesap anahtarı başka birinden alabilir ve adı ve anahtarı ile eklemek veya bir SAS depolama hesabı için birisinden ve depolama hesabı eklemek için kullanın.

Hesap anahtarlarını görürseniz, sorunu gidermenize yardımcı olabiliriz şekilde sonra lütfen bir Github'da sorun kaydedebilir.

## <a name="issues-with-sas-url"></a>SAS URL ile ilgili sorunlar
Bir SAS URL'sini kullanarak ve bu hatanın bir hizmeti bağlanıyorsanız:

* URL okuma veya kaynakları listelemek için gerekli izinleri sağladığından emin olun.
* URL sona ermediğinden emin olun.
* SAS URL'sini bir erişim ilkesi alıyorsa, erişim ilkesi edilmediğini doğrulayın.

Yanlışlıkla geçersiz bir SAS URL'si kullanarak bağlı ve ayırma belirleyemiyoruz, şu adımları izleyin:
1.  Depolama Gezgini çalıştırırken, geliştirici araçları penceresini açmak için F12 tuşuna basın.
2.  Uygulama sekmesini tıklatın ve ardından yerel depolama tıklayın > file:// soldaki ağaç.
3.  Sorunlu SAS URI'sini hizmet türü ile ilişkili anahtar bulun. Bozuk bir blob kapsayıcısı için SAS URI'si ise, örneğin, adlı anahtar için konum `StorageExplorer_AddStorageServiceSAS_v1_blob`.
4.  Anahtar değerini bir JSON dizisi olmalıdır. Bozuk URI'sı ile ilişkili nesneyi bulmak ve kaldırın.
5.  Depolama Gezgini'ni yeniden yüklemek için CTRL + R tuşuna basın.

## <a name="linux-dependencies"></a>Linux bağımlılıkları

Ubuntu 16.04 dışında Linux dağıtımları için bazı bağımlılıkları el ile yüklemeniz gerekebilir. Genel olarak, aşağıdaki paketler gereklidir:
* [.NET core 2.x](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x)
* `libsecret`
* `libgconf-2-4`
* Güncel GCC

Distro bağlı olarak diğer paketleri yüklemeniz gerekiyor olabilir. Depolama Gezgini [sürüm notları](https://go.microsoft.com/fwlink/?LinkId=838275&clcid=0x409) belirli adımlar için bazı dağıtım paketlerini içerir.

## <a name="next-steps"></a>Sonraki adımlar

Hiçbir çözüm, ardından çalışıyorsanız [github'da bir sorun açın](https://github.com/Microsoft/AzureStorageExplorer/issues). GitHub için sol alt köşedeki "Github'a rapor Issue" düğmesini kullanarak da hızlıca elde edebilirsiniz.

![Geri Bildirim](./media/storage-explorer-troubleshooting/feedback-button.PNG)
