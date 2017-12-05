---
title: "Azure Storage Gezgini sorun giderme kılavuzu | Microsoft Docs"
description: "Hata ayıklama özelliği Azure iki genel bakış"
services: virtual-machines
documentationcenter: 
author: Deland-Han
manager: cshepard
editor: 
ms.assetid: 
ms.service: virtual-machines
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/08/2017
ms.author: delhan
ms.openlocfilehash: 3187939fa813f941c2fe12a359df474a6c487c71
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a>Azure Storage Gezgini sorun giderme kılavuzu

Microsoft Azure Storage Gezgini (Önizleme), Windows, macOS ve Linux Azure Storage ile kolayca çalışmanızı sağlayan tek başına bir uygulamadır. Uygulama Azure, Sovereign Bulutlar ve Azure yığın üzerinde barındırılan depolama hesapları bağlanabilir.

Bu kılavuz, depolama Gezgini'nde görülen yaygın sorunlar için çözümleri özetler.

## <a name="sign-in-issues"></a>Oturum açma sorunları

Yalnızca Azure Active Directory (AAD) hesapları desteklenir. Bir ADFS hesabını kullanıyorsanız, Depolama Gezgini için oturum açma işe yaramayacaktır olduğunu beklenir. Devam etmeden önce uygulamanızı yeniden başlatmayı deneyin ve sorunların giderilip giderilemeyeceğini bakın.

### <a name="error-self-signed-certificate-in-certificate-chain"></a>Hatası: Sertifika zincirindeki otomatik olarak imzalanan sertifika

Neden bu hatayla karşılaşabilirsiniz ve en yaygın iki sebepleri şunlardır birkaç nedeni vardır:

1. Uygulama "(örneğin, şirket sunucunuzun) bir sunucu HTTPS trafiği kesintiye uğratan, şifre çözme ve kendinden imzalı bir sertifika kullanarak şifreleme anlamı saydam proxy" bağlandı.

2. Bir uygulama otomatik olarak imzalanan bir SSL sertifikası aldığınız HTTPS iletilere injecting virüsten koruma yazılımı gibi çalışıyor.

Depolama Gezgini sorunları karşılaştığında, artık alınan HTTPS ileti oynanmadığını bilebilirsiniz. Kendinden imzalı bir sertifika bir kopyasına sahipseniz, bu güven Depolama Gezgini izin verebilirsiniz. Sertifika injecting emin değilseniz bulmak için aşağıdaki adımları izleyin:

1. Açık SSL yükleyin

    - [Windows](https://slproweb.com/products/Win32OpenSSL.html) (açık sürümlerinin herhangi birinin yeterli olmalıdır)

    - Mac ve Linux: işletim sisteminizle eklenmelidir

2. Açık SSL çalıştırın

    - Windows: yükleme dizinini açın, **/bin/**, çift tıklayın ve ardından **openssl.exe**.
    - Mac ve Linux: çalıştırmak **openssl** bir terminal gelen.

3. S_client - showcerts yürütme-microsoft.com:443 Bağlan

4. Otomatik olarak imzalanan sertifikalar arayın. Kendinden imzalı olduğu emin değilseniz, herhangi bir yerde ("%s") konu arayın ve veren ("i:") aynıdır.

5. Herhangi bir otomatik olarak imzalanan sertifika bulduğunuzda, her biri için kopyalayıp her şeyi ilk ve son dahil olmak üzere **---başlangıç sertifika---** için **---son SERTİFİKAYI---** yeni bir .cer dosyasına.

6. Depolama Gezgini'ni açın, **Düzenle** > **SSL sertifikalarını** > **alma sertifikaları**ve ardından dosya seçiciyi kullanın bulun, seçin ve oluşturduğunuz .cer dosyaları açın.

Yukarıdaki adımları kullanarak herhangi bir otomatik olarak imzalanan sertifika bulamazsanız, daha fazla yardım için geri bildirim araçla bize başvurun.

### <a name="unable-to-retrieve-subscriptions"></a>Abonelik alınamadı

Başarıyla oturum açtıktan sonra aboneliklerinizi alamadı varsa, bu sorunu gidermek için aşağıdaki adımları izleyin:

- Azure portalında oturum açarak hesabınızı aboneliklere erişimi olduğunu doğrulayın.

- (Azure, Azure Çin, Azure Almanya, Azure ABD devlet kurumları veya özel ortam/Azure yığın) doğru ortamlarındaki açmış emin olun.

- Bir proxy'nin arkasında varsa, Depolama Gezgini proxy düzgün şekilde yapılandırdığınızdan emin olun.

- Try kaldırarak ve hesap yeniden ekleniyor.

- Kök dizin (diğer bir deyişle, C:\Users\ContosoUser) aşağıdaki dosyaları silerek ve hesap yeniden eklemeyi deneyin:

    - .adalcache

    - .devaccounts

    - .extaccounts

- Herhangi bir hata iletisi için oturum açarken (basarak, F12) geliştirici araçları konsol izleme:

![Geliştirici Araçları](./media/storage-explorer-troubleshooting/4022501_en_2.png)

### <a name="unable-to-see-the-authentication-page"></a>Kimlik doğrulaması sayfası Görülemedi

Kimlik doğrulama sayfasına bakın, sorunu gidermek için aşağıdaki adımları izleyin:

- Bağlantınızın hızına bağlı olarak biraz yüklenemedi, kimlik doğrulama iletişim kutusunu kapatmadan önce en az bir dakika bekleyin oturum açma sayfasına ilişkin devam edebilir.

- Bir proxy'nin arkasında varsa, Depolama Gezgini proxy düzgün şekilde yapılandırdığınızdan emin olun.

- Geliştirici Konsolu F12 tuşuna basarak görüntüleyin. Geliştirici konsolundan yanıtlarını izleyin ve tüm ipucu için neden Bul olup olmadığını görmek kimlik doğrulaması çalışmıyor.

### <a name="cannot-remove-account"></a>Hesap kaldırılamıyor

Bir hesap kaldıramadı ya da yeniden kimlik doğrula bağlantı herhangi bir şey yapmanız durumunda, bu sorunu gidermek için aşağıdaki adımları izleyin:

- Kök dizininden aşağıdaki dosyaları silerek ve hesap yeniden ekleniyor deneyin:

    - .adalcache

    - .devaccounts

    - .extaccounts

- Kaldırmak isterseniz, SAS depolama kaynaklarını eklenen, aşağıdaki dosyaları silin:

    - Windows için %appdata%/StorageExplorer klasör

    - /Users/ < adiniz >/kitaplık/uygulaması destek/StorageExplorer Mac için

    - Linux için ~/.config/StorageExplorer

> [!NOTE]
>  Bu dosyaları silerseniz, tüm kimlik bilgilerinizi yeniden girmeniz gerekir.

## <a name="proxy-issues"></a>Proxy sorunları

İlk olarak, aşağıdaki bilgileri, girdiğiniz tüm doğru olduğundan emin olun:

- Proxy URL'si ve bağlantı noktası numarası

- Kullanıcı adı ve proxy sunucu tarafından gerekliyse parola

### <a name="common-solutions"></a>Yaygın çözümleri

Sorunları yaşamaya devam ediyorsanız, bunları gidermek için aşağıdaki adımları izleyin:

- Internet'e bir proxy sunucunuz kullanmadan bağlanabiliyorsa Depolama Gezgini proxy ayarlarının etkin çalıştığını doğrulayın. Bu durumda, proxy ayarlarınızı ile ilgili bir sorun olabilir. Sorunları tanımlamak için proxy yöneticinizle birlikte çalışın.

- Proxy sunucusu kullanan diğer uygulamalar beklendiği gibi çalıştığını doğrulayın.

- Web tarayıcınızı kullanarak Microsoft Azure portalına bağlanabildiğini doğrulayın

- Hizmet uç noktalarından yanıtları alabilir doğrulayın. Uç nokta URL'leri birini tarayıcınıza girin. Bağlanabiliyorsanız, bir InvalidQueryParameterValue veya benzeri bir XML yanıt almanız gerekir.

- Başka biri de Depolama Gezgini proxy sunucunuz ile kullanıyorsa, bunlar bağlanabildiğinizi doğrulayın. Bağlanabiliyorsanız, proxy sunucusu yöneticinize başvurmanız gerekebilir

### <a name="tools-for-diagnosing-issues"></a>Sorunları tanılamak için Araçlar

Windows Fiddler gibi ağ araçları varsa, aşağıdaki gibi sorunları tanılamak doğrulayabilirsiniz:

- Proxy üzerinden çalışmanız gerekiyorsa, proxy üzerinden bağlanmak için ağ aracınızı yapılandırmanız gerekebilir.

- Ağ aracı tarafından kullanılan bağlantı noktası numarasını denetleyin.

- Proxy ayarları Depolama Gezgini olarak yerel ana bilgisayar URL'si ve ağ aracın bağlantı noktası numarası girin. Bu doğru gerçekleştirilir, Ağ aracı yönetimi ve hizmet uç noktaları için Depolama Gezgini tarafından yapılan ağ istekleri günlüğünü başlatır. Örneğin, bir tarayıcıda blob uç noktanız için https://cawablobgrs.blob.core.windows.net/ girin ve alırsınız yanıt bu verilere erişemez olsa da, kaynağın var, önerir aşağıdakilere benzer.

![kod örneği](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### <a name="contact-proxy-server-admin"></a>Proxy sunucu yöneticisine başvurun

Proxy ayarlarınızın doğru olduğunu, proxy sunucusu yöneticinize başvurmanız gerekebilir ve

- Proxy Azure Yönetimi'ni veya kaynak uç noktaları için trafiği engellemediğinden emin olun.

- Proxy sunucunuz tarafından kullanılan kimlik doğrulama protokolü doğrulayın. Depolama Gezgini NTLM proxy'leri şu anda desteklemiyor.

## <a name="unable-to-retrieve-children-error-message"></a>"Almak alt oluşturulamıyor" hata iletisi

Azure için bir proxy üzerinden bağlıysanız, proxy ayarlarının doğru olduğundan emin olun. Abonelik ya da hesap sahibinden bir kaynağa erişim verilmiş, okuma veya bu kaynak için izinleri listesinde doğrulayın.

### <a name="issues-with-sas-url"></a>SAS URL ile ilgili sorunları
Bir SAS URL'si kullanarak ve bu hatanın bir hizmete bağlanıyorsanız:

- URL okuma veya kaynakları listelemek için gerekli izinleri sağladığından emin olun.

- URL geçmediğini doğrulayın.

- SAS URL bir erişim ilkesini temel alarak, erişim ilkesi edilmediğini doğrulayın.

Varsa, yanlışlıkla geçersiz bir SAS URL'si bağlı ve ayrılamadı, lütfen şu adımları izleyin:
1.  Depolama Gezgini çalıştırırken, geliştirici araçları penceresini açmak için F12 tuşuna basın.
2.  Uygulama sekmesini tıklatın ve ardından yerel depolama > soldaki ağaç file://.
3.  Sorunlu SAS URI'sini hizmet türü ile ilişkili anahtar bulunamıyor. Örneğin, hatalı bir blob kapsayıcısı için SAS URI'sini ise, "StorageExplorer_AddStorageServiceSAS_v1_blob" adlı anahtar için arayın.
4.  Anahtarın değerini bir JSON dizisi olmalıdır. Geçersiz URI'sı ile ilişkili nesneyi bulmak ve kaldırmak.
5.  Depolama Gezgini yeniden yüklemek için CTRL + R tuşuna basın.


## <a name="next-steps"></a>Sonraki adımlar

Çözümlerin hiçbiri sizin için çalışıyorsanız, sorununuzu geri bildirim aracı e-posta ile göndermek ve böylece biz, sorunu düzeltmek için başvurabilirsiniz sayıda ayrıntılarını dahil ettiğiniz sorunu olabilir.

Bunu yapmak için tıklatın **yardımcı** menüsüne ve ardından **geri bildirim gönder**.

![Geri Bildirim](./media/storage-explorer-troubleshooting/4022503_en_1.png)
