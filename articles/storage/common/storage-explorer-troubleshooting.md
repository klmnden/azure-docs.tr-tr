---
title: Azure Storage Gezgini sorun giderme kılavuzu | Microsoft Docs
description: Hata ayıklama özelliği Azure iki genel bakış
services: virtual-machines
documentationcenter: ''
author: Deland-Han
manager: cshepard
editor: ''
ms.assetid: ''
ms.service: virtual-machines
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/08/2017
ms.author: delhan
ms.openlocfilehash: f58fb5090aba3c5052d1bbdec76225d0ae50e8f2
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a>Azure Storage Gezgini sorun giderme kılavuzu

Microsoft Azure Storage Gezgini, Windows, macOS ve Linux Azure Storage ile kolayca çalışmanızı sağlayan tek başına bir uygulamadır. Uygulama Azure, Ulusal Bulutlar ve Azure yığın üzerinde barındırılan depolama hesapları bağlanabilir.

Bu kılavuz, depolama Gezgini'nde görülen yaygın sorunlar için çözümleri özetler.

## <a name="error-self-signed-certificate-in-certificate-chain-and-similar-errors"></a>Hatası: Sertifika zinciri (ve benzer hatalar) otomatik olarak imzalanan sertifika

Sertifika hataları aşağıdaki durumlardan biri tarafından neden olur:

1. Uygulama "(örneğin, şirket sunucunuzun) bir sunucu HTTPS trafiği kesintiye uğratan, şifre çözme ve kendinden imzalı bir sertifika kullanarak şifreleme anlamı saydam proxy" bağlandı.
2. Otomatik olarak imzalanan bir SSL sertifikası aldığınız HTTPS iletilere injecting uygulamanın çalışıyor. Sertifikaları Ekle uygulamalarına örnek olarak, virüsten koruma ve ağ trafiğini denetleme yazılım içerir.

Depolama Gezgini imzalı kendi gördüğünde veya güvenilmeyen sertifika artık alınan HTTPS ileti değiştirilmiş olup olmadığını bilebilirsiniz. Kendinden imzalı bir sertifika bir kopyasına sahipseniz, Depolama Gezgini söyleyebilirsiniz aşağıdaki adımları uygulayarak güven:

1. Elde sertifikanın X.509 (.cer) kopyasını Base-64 kodlamalı
2. Tıklatın **Düzenle** > **SSL sertifikalarını** > **alma sertifikaları**ve ardından bulmak, seçmek ve .cer dosyasını açmak için dosya seçiciyi kullanın

Bu sorun ayrıca birden çok sertifika (kök ve Ara) sonucu olabilir. Hatayı gidermek için her iki sertifikanın eklenmesi gerekir.

Burada sertifika geldiği emin değilseniz bulmak için aşağıdaki adımları deneyebilirsiniz:

1. Açık SSL yükleme

    * [Windows](https://slproweb.com/products/Win32OpenSSL.html) (açık sürümlerinin herhangi birinin yeterli olmalıdır)
    * Mac ve Linux: işletim sisteminizle eklenmelidir
2. Açık SSL çalıştırma

    * Windows: yükleme dizinini açın, **/bin/**, çift tıklayın ve ardından **openssl.exe**.
    * Mac ve Linux: çalıştırmak **openssl** bir terminal gelen.
3. `s_client -showcerts -connect microsoft.com:443` yürütme
4. Otomatik olarak imzalanan sertifikaları bulun. Kendinden imzalı olduğu emin değilseniz, herhangi bir yere konu arayın `("s:")` ve sertifikayı veren `("i:")` aynıdır.
5. Herhangi bir otomatik olarak imzalanan sertifika bulduğunuzda, her biri için kopyalayıp her şeyi ilk ve son dahil olmak üzere **---başlangıç sertifika---** için **---son SERTİFİKAYI---** yeni bir .cer dosyasına.
6. Depolama Gezgini'ni açın, **Düzenle** > **SSL sertifikalarını** > **alma sertifikaları**ve ardından dosya seçiciyi kullanın bulun, seçin ve oluşturduğunuz .cer dosyaları açın.

Yukarıdaki adımları kullanarak herhangi bir otomatik olarak imzalanan sertifika bulamazsanız, daha fazla yardım için geri bildirim araçla bize başvurun. Depolama Gezgini ile komut satırından başlatmak alternatif olarak, seçebileceğiniz `--ignore-certificate-errors` bayrağı. Depolama Gezgini ile bu bayrak başlatıldığında sertifika hataları göz ardı eder.

## <a name="sign-in-issues"></a>Oturum açma sorunları

Oturum açın, aşağıdaki sorun giderme yöntemleri deneyin:

* Depolama Gezgini yeniden başlatın
* Kimlik doğrulama penceresi boş ise, kimlik doğrulama iletişim kutusunu kapatmadan önce en az bir dakika bekleyin.
* Proxy ve sertifika ayarlarının, makine ve Depolama Gezgini için düzgün biçimde yapılandırıldığından emin olun
* Windows olan ve Visual Studio 2017 erişiminiz aynı makinesindeki ve oturum açma, Visual Studio 2017 oturum açmayı deneyin

Bu yöntemlerden hiçbiri işe [github'da bir sorun açın](https://github.com/Microsoft/AzureStorageExplorer/issues).

## <a name="unable-to-retrieve-subscriptions"></a>Abonelikler alınamıyor

Başarıyla oturum açtıktan sonra aboneliklerinizi alamadı varsa, aşağıdaki sorun giderme yöntemleri deneyin:

* Hesabınızı beklediğiniz aboneliklere erişimi olduğunu doğrulayın. Kullanmaya çalıştığınız Azure ortamı için portalında oturum açarak erişimi doğrulayabilirsiniz.
* Doğru Azure kullanarak imzaladığınız emin olun (Azure, Azure Çin, Azure Almanya, Azure ABD devlet kurumları veya özel ortam) ortamı.
* Bir proxy'nin arkasında varsa, Depolama Gezgini proxy düzgün şekilde yapılandırdığınızdan emin olun.
* Try kaldırarak ve hesap yeniden ekleniyor.
* Geliştirici Araçları konsolunu izleyin (Yardım > geçiş Geliştirici Araçları) Depolama Gezgini abonelikler yüklenirken. Hata iletileri (kırmızı metin) arayın veya metin içeren herhangi bir iletisi "Kiracı için abonelikler yüklenemedi." Herhangi bir concerning iletisi görürseniz [github'da bir sorun açın](https://github.com/Microsoft/AzureStorageExplorer/issues).

## <a name="cannot-remove-attached-account-or-storage-resource"></a>Ekli hesabı ya da depolama kaynak kaldırılamıyor

Ekli hesabı ya da kullanıcı Arabirimi aracılığıyla depolama kaynağı kaldıramadı varsa, aşağıdaki klasörler silerek tüm bağlı kaynakları el ile silebilirsiniz:

* Windows: `%AppData%/StorageExplorer`
* macOS: `/Users/<your_name>/Library/Applicaiton Support/StorageExplorer`
* Linux: `~/.config/StorageExplorer`

> [!NOTE]
>  Depolama Gezgini yukarıdaki klasörleri silmeden önce kapatın.

> [!NOTE]
>  Şimdiye kadar tüm SSL sertifikaları içe aktardıktan sonra içeriğini yedekleme `certs` dizin. Daha sonra SSL sertifikalarınızı yeniden içeri aktarın için yedekleme kullanabilirsiniz.

## <a name="proxy-issues"></a>Proxy sorunları

İlk olarak, aşağıdaki bilgileri, girdiğiniz tüm doğru olduğundan emin olun:

* Proxy URL'si ve bağlantı noktası numarası * kullanıcı adı ve proxy sunucu tarafından gerekliyse parola

### <a name="common-solutions"></a>Yaygın çözümleri

Sorunları yaşamaya devam ediyorsanız, aşağıdaki sorun giderme yöntemleri deneyin:

* Internet'e bir proxy sunucunuz kullanmadan bağlanabiliyorsa Depolama Gezgini proxy ayarlarının etkin çalıştığını doğrulayın. Bu durumda, proxy ayarlarınızı ile ilgili bir sorun olabilir. Sorunları tanımlamak için proxy yöneticinizle birlikte çalışın.
* Proxy sunucusu kullanan diğer uygulamalar beklendiği gibi çalıştığını doğrulayın.
* Kullanmaya çalıştığınız Azure ortamı için portal bağlanabildiğini doğrulayın
* Hizmet uç noktalarından yanıtları alabilir doğrulayın. Uç nokta URL'leri birini tarayıcınıza girin. Bağlanabiliyorsanız, bir InvalidQueryParameterValue veya benzeri bir XML yanıt almanız gerekir.
* Başka biri de Depolama Gezgini proxy sunucunuz ile kullanıyorsa, bunlar bağlanabildiğinizi doğrulayın. Bağlanabiliyorsanız, proxy sunucusu yöneticinize başvurmanız gerekebilir

### <a name="tools-for-diagnosing-issues"></a>Sorunları tanılamak için Araçlar

Windows Fiddler gibi ağ araçları varsa, aşağıdaki gibi sorunları tanılamak doğrulayabilirsiniz:

* Proxy üzerinden çalışmanız gerekiyorsa, proxy üzerinden bağlanmak için ağ aracınızı yapılandırmanız gerekebilir.
* Ağ aracı tarafından kullanılan bağlantı noktası numarasını denetleyin.
* Proxy ayarları Depolama Gezgini olarak yerel ana bilgisayar URL'si ve ağ aracın bağlantı noktası numarası girin. Doğru yapıldığında, Ağ aracı yönetimi ve hizmet uç noktaları için Depolama Gezgini tarafından yapılan ağ istekleri günlüğünü başlatır. Örneğin, https://cawablobgrs.blob.core.windows.net/ bir tarayıcı ve blob uç noktanızı alacak için bir yanıt bu verilere erişemez olsa da, kaynağın var, öneren aşağıdaki benzer.

![kod örneği](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### <a name="contact-proxy-server-admin"></a>Proxy sunucu yöneticisine başvurun

Proxy ayarlarınızın doğru olduğunu, proxy sunucusu yöneticinize başvurmanız gerekebilir ve

* Proxy Azure Yönetimi'ni veya kaynak uç noktaları için trafiği engellemediğinden emin olun.
* Proxy sunucunuz tarafından kullanılan kimlik doğrulama protokolü doğrulayın. Depolama Gezgini NTLM proxy'leri şu anda desteklemiyor.

## <a name="unable-to-retrieve-children-error-message"></a>"Almak alt oluşturulamıyor" hata iletisi

Azure için bir proxy üzerinden bağlıysanız, proxy ayarlarının doğru olduğundan emin olun. Abonelik ya da hesap sahibinden bir kaynağa erişim verilmiş, okuma veya bu kaynak için izinleri listesinde doğrulayın.

### <a name="issues-with-sas-url"></a>SAS URL ile ilgili sorunları
Bir SAS URL'si kullanarak ve bu hatanın bir hizmete bağlanıyorsanız:

* URL okuma veya kaynakları listelemek için gerekli izinleri sağladığından emin olun.
* URL geçmediğini doğrulayın.
* SAS URL bir erişim ilkesini temel alarak, erişim ilkesi edilmediğini doğrulayın.

Yanlışlıkla geçersiz bir SAS URL'si kullanarak bağlı ve detach yükleyemedi, şu adımları izleyin:
1.  Depolama Gezgini çalıştırırken, geliştirici araçları penceresini açmak için F12 tuşuna basın.
2.  Uygulama sekmesini tıklatın ve ardından yerel depolama > soldaki ağaç file://.
3.  Sorunlu SAS URI'sini hizmet türü ile ilişkili anahtar bulunamıyor. Bozuk bir blob kapsayıcısı için SAS URI'sini ise, örneğin, adlı anahtar için aramak `StorageExplorer_AddStorageServiceSAS_v1_blob`.
4.  Anahtarın değerini bir JSON dizisi olmalıdır. Geçersiz URI'sı ile ilişkili nesneyi bulmak ve kaldırmak.
5.  Depolama Gezgini yeniden yüklemek için CTRL + R tuşuna basın.

## <a name="linux-dependencies"></a>Linux bağımlılıkları

Ubuntu 16.04 dışında Linux distro'lar için bazı bağımlılıklar el ile yüklemeniz gerekebilir. Genel olarak, aşağıdaki paketler gereklidir:
* [.NET core 2.x](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x)
* `libsecret`
* `libgconf-2-4`
* Güncel GCC

Distro bağlı olarak yüklemek için gereken diğer paket olabilir. Depolama Gezgini [sürüm notları](https://go.microsoft.com/fwlink/?LinkId=838275&clcid=0x409) bazı distro'lar için belirli adımlar içerir.

## <a name="next-steps"></a>Sonraki adımlar

Çözümlerin hiçbiri sizin için daha sonra çalışıyorsanız [github'da bir sorun açın](https://github.com/Microsoft/AzureStorageExplorer/issues). GitHub için sayfanın sol üst köşedeki "Rapor sorunu GitHub için" düğmesini kullanarak da hızlı bir şekilde elde edebilirsiniz.

![Geri Bildirim](./media/storage-explorer-troubleshooting/feedback-button.PNG)
