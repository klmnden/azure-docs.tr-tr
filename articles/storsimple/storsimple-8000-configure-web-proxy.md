---
title: StorSimple 8000 serisi cihaz için web proxy ayarlama | Microsoft Docs
description: StorSimple için Windows PowerShell, StorSimple cihazınız için web proxy ayarlarını yapılandırmak için kullanmayı öğrenin.
services: storsimple
documentationcenter: ''
author: alkohli
manager: ''
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: alkohli
ms.openlocfilehash: be5719d2c383c838ef70c6862c1055c3374e05e5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60362501"
---
# <a name="configure-web-proxy-for-your-storsimple-device"></a>StorSimple cihazınız için Web Proxy'yi Yapılandır

## <a name="overview"></a>Genel Bakış

Bu öğreticide, yapılandırmak ve StorSimple cihazınız için web proxy ayarlarını görüntülemek için StorSimple için Windows PowerShell'i kullanmayı açıklar. Web proxy ayarlarını, bulutla iletişim kurarken StorSimple cihaz tarafından kullanılır. Web proxy sunucusu, güvenlik, filtre içeriğini kolayca bant genişliği gereksinimlerini veya hatta Yardım analizlerle önbellek katmanı eklemek için kullanılır.

Bu öğreticideki yönergeler yalnızca StorSimple 8000 serisi fiziksel cihazlar için geçerlidir. Web proxy yapılandırması, StorSimple bulut Gereci (8010 ve 8020) üzerinde desteklenmiyor.

Web proxy'si bir _isteğe bağlı_ StorSimple cihazınız için yapılandırma. StorSimple için Windows PowerShell aracılığıyla yalnızca web Ara sunucusu yapılandırabilirsiniz. Yapılandırmanın iki adımlı bir işlem aşağıdaki gibidir:

1. İlk StorSimple cmdlet'leri için Kurulum Sihirbazı'nı veya Windows PowerShell aracılığıyla web proxy ayarlarını yapılandırın.
2. Ardından StorSimple cmdlet'leri için Windows PowerShell aracılığıyla yapılandırılan web proxy ayarlarını sağlar.

Web proxy yapılandırması tamamlandıktan sonra yapılandırılmış web proxy ayarlarını hem Microsoft Azure StorSimple cihaz Yöneticisi hizmeti hem de Windows PowerShell için StorSimple görüntüleyebilirsiniz.

Bu öğreticide okuduktan sonra şunları yapabilir:

* Web ara Sunucusu Kurulum Sihirbazı'nı ve cmdlet'lerini kullanarak yapılandırın.
* Web proxy cmdlet'lerini kullanarak etkinleştirirsiniz.
* Web proxy ayarları Azure portalında görüntüleyin.
* Web proxy yapılandırması sırasında hataları giderin.


## <a name="configure-web-proxy-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell aracılığıyla Web proxy yapılandırma

Web proxy ayarlarını yapılandırmak için şunlardan birini kullanın:

* Yapılandırma adımlarınıza kılavuzluk için Kurulum Sihirbazı'nı tıklatın.
* StorSimple için Windows PowerShell cmdlet'leri.

Bu yöntemlerin her biri, aşağıdaki bölümlerde ele alınmıştır.

## <a name="configure-web-proxy-via-the-setup-wizard"></a>Kurulum Sihirbazı aracılığıyla Web Proxy'yi Yapılandır

Web proxy yapılandırma adımlarında size yol göstermesi için Kurulum Sihirbazı'nı kullanın. Cihazınızda Web proxy'sini yapılandırmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-configure-web-proxy-via-the-setup-wizard"></a>Kurulum Sihirbazı aracılığıyla Web proxy'sini yapılandırmak için

1. Seri konsol menüsünde seçeneği 1 **tam erişimle oturum açmak** ve **cihaz Yöneticisi parolası**. Kurulum Sihirbazı oturumu başlatmak için aşağıdaki komutu yazın:
   
    `Invoke-HcsSetupWizard`
2. Bu, cihaz kaydı için Kurulum Sihirbazı'nı kullandığınız ilk kez ise, web proxy yapılandırması gelene kadar tüm gerekli ağ ayarlarını yapılandırmak gerekir. Cihazınız zaten kayıtlıysa, web proxy yapılandırması gelene kadar tüm yapılandırılmış ağ ayarlarını kabul edin. Kurulum Sihirbazı'nda web proxy ayarlarını yapılandırmak isteyip istemediğiniz sorulduğunda yazın **Evet**.
3. İçin **Web Proxy URL'si**, web proxy sunucu IP adresini veya tam etki alanı adı (FQDN) belirtin ve TCP bağlantı noktası numarası bulutla iletişim kurarken kullanması için Cihazınızı istersiniz. Aşağıdaki biçimi kullanın:
   
    `http://<IP address or FQDN of the web proxy server>:<TCP port number>`
   
    Varsayılan olarak, TCP bağlantı noktası 8080 belirtilir.
4. Kimlik doğrulaması türü olarak seçin **NTLM**, **temel**, veya **hiçbiri**. En az güvenli kimlik doğrulaması için proxy sunucusu yapılandırması, temel üyeliktir. NT LAN Manager (NTLM), bir üç yönlü Mesajlaşma sistemi (bazen dört ek bütünlüğü gerekliyse) kullanan bir yüksek oranda güvenli ve karmaşık kimlik doğrulama protokolü olan bir kullanıcının kimliğini doğrulamak için. Varsayılan kimlik doğrulaması, NTLM olur. Daha fazla bilgi için [temel](http://hc.apache.org/httpclient-3.x/authentication.html) ve [NTLM kimlik doğrulaması](http://hc.apache.org/httpclient-3.x/authentication.html). 
   
   > [!IMPORTANT]
   > **StorSimple cihaz Yöneticisi hizmeti, cihaz izleme grafiklerini, temel sırasında çalışmıyor veya cihaz için proxy sunucusu yapılandırmasını NTLM kimlik doğrulaması etkin. İzleme grafiklerini çalışmak söz konusu kimlik doğrulamasını NONE olarak ayarlandığından emin olmak gerekir.**
  
5. Kimlik doğrulamasını etkinleştirdiyseniz, kaynağı bir **Web Proxy kullanıcı adı** ve **Web ara sunucu parolasını**. Parolayı onaylamanız gerekir.
   
    ![StorSimple cihaz1'de Web Proxy'yi Yapılandır](./media/storsimple-configure-web-proxy/IC751830.png)

Cihazınız ilk kez kaydediyorsanız, kayıt ile devam edin. Cihazınız zaten kayıtlı, sihirbaz kapanır. Yapılandırılmış ayarlar kaydedildi.

Web proxy şimdi etkinleştirildi. Atlayabilirsiniz [web Ara sunucusu etkinleştirmek](#enable-web-proxy) adım ve doğrudan [görünüm web proxy ayarları Azure portalında](#view-web-proxy-settings-in-the-azure-portal).

## <a name="configure-web-proxy-via-windows-powershell-for-storsimple-cmdlets"></a>StorSimple cmdlet'leri için Windows PowerShell aracılığıyla Web Proxy'yi Yapılandır

StorSimple cmdlet'leri için Windows PowerShell aracılığıyla Web proxy ayarlarını yapılandırmak için alternatif bir yolu var. Web proxy'sini yapılandırmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-configure-web-proxy-via-cmdlets"></a>Cmdlet'leri aracılığıyla Web proxy'sini yapılandırmak için
1. Seri konsol menüsünde seçeneği 1 **tam erişimle oturum açmak**. İstendiğinde, sağlayın **cihaz Yöneticisi parolası**. Varsayılan parola `Password1`.
2. Komut istemine şunları yazın:
   
    `Set-HcsWebProxy -Authentication NTLM -ConnectionURI "<http://<IP address or FQDN of web proxy server>:<TCP port number>" -Username "<Username for web proxy server>"`
   
    Sağlayın ve istendiğinde parolayı onaylayın.
   
    ![StorSimple cihaz3 Web Proxy'yi Yapılandır](./media/storsimple-configure-web-proxy/IC751831.png)

Web Ara sunucusu artık yapılandırılmıştır ve etkinleştirilmesi gerekir.

## <a name="enable-web-proxy"></a>Web ara sunucusunu etkinleştirme

Web ara sunucusu, varsayılan olarak devre dışıdır. StorSimple Cihazınızda web proxy ayarlarını yapılandırdıktan sonra web proxy ayarlarını etkinleştirmek için StorSimple için Windows PowerShell kullanın.

> [!NOTE]
> **Web proxy'sini yapılandırmak için Kurulum Sihirbazı'nı kullandıysanız, bu adım gerekli değildir. Web proxy, Kurulum Sihirbazı oturumu sonra otomatik olarak varsayılan olarak etkindir.**


Cihazınızdaki web proxy'sini etkinleştirmek StorSimple için Windows PowerShell içinde aşağıdaki adımları gerçekleştirin:

#### <a name="to-enable-web-proxy"></a>Web proxy'sini etkinleştirmek için
1. Seri konsol menüsünde seçeneği 1 **tam erişimle oturum açmak**. İstendiğinde, sağlayın **cihaz Yöneticisi parolası**. Varsayılan parola `Password1`.
2. Komut istemine şunları yazın:
   
    `Enable-HcsWebProxy`
   
    Şimdi, StorSimple Cihazınızda web proxy yapılandırması etkinleştirdiniz.
   
    ![StorSimple cihaz4 Web Proxy'yi Yapılandır](./media/storsimple-configure-web-proxy/IC751832.png)

## <a name="view-web-proxy-settings-in-the-azure-portal"></a>Web proxy ayarları Azure portalında görüntüleme

Web proxy ayarları Windows PowerShell arabirimi aracılığıyla yapılandırılır ve portalın içinde değiştirilemez. Ancak, bu yapılandırılmış ayarlar portalında görüntüleyebilirsiniz. Web proxy görüntülemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-view-web-proxy-settings"></a>Web proxy ayarlarını görüntülemek için
1. Gidin **StorSimple cihaz Yöneticisi hizmeti > cihazlar**. Seçin ve bir cihaza tıklayın ve ardından Git **cihaz Ayarları > Ağ**.

    ![Ağa tıklayın](./media/storsimple-8000-configure-web-proxy/view-web-proxy-1.png)

2. İçinde **ağ ayarları** dikey penceresinde tıklayın **Web proxy** Döşe.

    ![Web proxy tıklayın](./media/storsimple-8000-configure-web-proxy/view-web-proxy-2.png)

3. İçinde **Web proxy** dikey penceresinde, StorSimple Cihazınızda yapılandırılmış web proxy ayarları gözden geçirin.
   
    ![Görünümü web proxy ayarları](./media/storsimple-8000-configure-web-proxy/view-web-proxy-3.png)


## <a name="errors-during-web-proxy-configuration"></a>Web proxy yapılandırması sırasında hatalar

Web proxy ayarları hatalı yapılandırıldıysa, hata iletileri için Windows PowerShell'de kullanıcı için StorSimple görüntülenir. Aşağıdaki tabloda, bu hata iletileri, olası nedenler ve önerilen eylemler bazıları açıklanmaktadır.

| Seri yok. | HRESULT hata kodu | Olası kök nedeni | Önerilen eylem |
|:--- |:--- |:--- |:--- |
| 1. |0x80070001 |Komutu, pasif denetleyiciden çalıştırın ve etkin denetleyiciyle iletişim kurmak mümkün değildir. |Komut etkin denetleyicide çalıştırın. Pasif denetleyicisinden komutu çalıştırmak için etkin denetleyiciyi pasif gelen bağlantı düzeltmeniz gerekir. Bu bağlantı bozuk ise Microsoft Support etkileşim kurun gerekir. |
| 2. |0x800710dd - işlem tanımlayıcısı geçerli değil |Ara sunucu ayarlarını, StorSimple Cloud Appliance'ta desteklenmez. |Ara sunucu ayarlarını, StorSimple Cloud Appliance'ta desteklenmez. Bu, yalnızca StorSimple fiziksel cihaz üzerindeki yapılandırılabilir. |
| 3. |0x80070057 - geçersiz parametre |Bir proxy ayarları için sağlanan parametreler geçerli değil. |URI, doğru biçimde sağlanmadı. Aşağıdaki biçimi kullanın: `http://<IP address or FQDN of the web proxy server>:<TCP port number>` |
| 4. |0x800706ba - RPC sunucusu kullanılamıyor |Kök nedeni aşağıdakilerden biridir:</br></br>Küme ayarlama değil. </br></br>DataPath hizmeti çalışmıyor.</br></br>Komutu, pasif denetleyiciden çalıştırın ve etkin denetleyiciyle iletişim kurmak mümkün değildir. |Küme çalışır durumda ve datapath hizmetinin çalıştığından emin olmak için Microsoft Support etkileşim kurun.</br></br>Komut etkin denetleyiciden çalıştırın. Pasif denetleyicisinden komutu çalıştırmak istiyorsanız, edilgen denetleyici etkin denetleyiciyle iletişim kurabildiğinden emin olmanız gerekir. Bu bağlantı bozuk ise Microsoft Support etkileşim kurun gerekir. |
| 5. |0X800706be - RPC çağrısı başarısız oldu |Küme çalışmıyor. |Küme olduğundan emin olmak için Microsoft Support etkileşim kurun. |
| 6. |0x8007138f - küme kaynağı bulunamadı |Platform Hizmet küme kaynağı bulunamadı. Bu yükleme uygun olmadığında oluşabilir. |Cihazı fabrika ayarlarına döndürebilirsiniz gerekebilir. Bir platform kaynak oluşturmanız gerekebilir. Sonraki adımlar için Microsoft Desteği'ne başvurun. |
| 7. |0x8007138c - küme kaynağı çevrimiçi değil |Platform veya datapath küme kaynağı çevrimiçi değil. |Datapath ve platform Hizmet kaynağı çevrimiçi olduğundan emin olmak için Microsoft Support başvurun. |

> [!NOTE]
> * Yukarıdaki hata iletilerinin listesi kapsamlı değildir.
> * Web proxy ayarlarıyla ilgili hatalar, StorSimple cihaz Yöneticisi hizmetinizin Azure portalında görüntülenmez. Yapılandırma tamamlandıktan sonra web Ara sunucusu ile ilgili bir sorun olduğunda cihaz durumu değişir **çevrimdışı** Klasik portalda. |

## <a name="next-steps"></a>Sonraki Adımlar
* Cihazınızı dağıtma veya web Ara sunucu ayarlarını yapılandırma herhangi bir sorun yaşarsanız başvurmak [StorSimple cihaz dağıtımınızın sorunlarını giderdikten](storsimple-troubleshoot-deployment.md).
* StorSimple cihaz Yöneticisi hizmetini kullanmayı öğrenmek için Git [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

