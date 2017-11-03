---
title: "StorSimple 8000 serisi cihaz için web proxy ayarlama | Microsoft Docs"
description: "StorSimple cihazınız için web proxy ayarlarını yapılandırmak için StorSimple için Windows PowerShell'i kullanmayı öğrenin."
services: storsimple
documentationcenter: 
author: alkohli
manager: 
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: alkohli
ms.openlocfilehash: 1109e44ed9c6aa8a0f7305b8a50410316711589c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-web-proxy-for-your-storsimple-device"></a>StorSimple cihazınız için Web Proxy'yi Yapılandır

## <a name="overview"></a>Genel Bakış

Bu öğretici, yapılandırmak ve StorSimple cihazınız için web proxy ayarlarını görüntülemek için StorSimple için Windows PowerShell'i kullanmayı açıklar. Web proxy ayarları, bulut ile iletişim kurarken StorSimple cihaz tarafından kullanılır. Web proxy sunucusu, başka bir güvenlik katmanı, filtre içerik kolaylığı bant genişliği gereksinimlerini veya hatta Yardım analytics ile önbelleğine eklemek için kullanılır.

Bu öğreticideki yönergeler yalnızca StorSimple 8000 serisi fiziksel cihazlar için geçerlidir. Web proxy yapılandırması (8010 hem de 8020) StorSimple bulut uygulaması üzerinde desteklenmiyor.

Web proxy'si, bir _isteğe bağlı_ StorSimple cihazınız için yapılandırma. StorSimple için Windows PowerShell aracılığıyla yalnızca web sunucusu yapılandırabilir. Yapılandırmanın iki adımlı bir işlem aşağıdaki gibidir:

1. İlk StorSimple cmdlet'leri Kurulum Sihirbazı'nı veya Windows PowerShell aracılığıyla web proxy ayarlarını yapılandırın.
2. Ardından StorSimple cmdlet'leri için Windows PowerShell aracılığıyla yapılandırılmış web proxy ayarlarını sağlar.

Web proxy yapılandırması tamamlandıktan sonra yapılandırılmış web proxy ayarlarını hem Microsoft Azure StorSimple cihaz Yöneticisi hizmeti hem de Windows PowerShell için StorSimple görüntüleyebilirsiniz.

Bu öğretici okuduktan sonra aşağıdakileri gerçekleştirebilirsiniz:

* Web ara Sunucusu Kurulum Sihirbazı'nı ve cmdlet'lerini kullanarak yapılandırın.
* Web proxy cmdlet'leri kullanarak etkinleştirin.
* Azure portalında Web proxy ayarları görüntüleyin.
* Web proxy yapılandırması sırasında hataları giderin.


## <a name="configure-web-proxy-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell aracılığıyla Web Proxy'yi Yapılandır

Web proxy ayarlarını yapılandırmak için aşağıdakilerden birini kullanın:

* Yapılandırma adımlarınıza kılavuzluk için Kurulum Sihirbazı'nı tıklatın.
* StorSimple için Windows PowerShell cmdlet'leri.

Bu yöntemlerin her biri aşağıdaki bölümlerde ele alınmıştır.

## <a name="configure-web-proxy-via-the-setup-wizard"></a>Kurulum Sihirbazı aracılığıyla Web Proxy'yi Yapılandır

Web proxy yapılandırması adımlarında size yol göstermesi için Kurulum Sihirbazı'nı kullanın. Web proxy aygıtınızda yapılandırmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-configure-web-proxy-via-the-setup-wizard"></a>Kurulum Sihirbazı aracılığıyla Web proxy yapılandırmak için

1. Seri konsol menüsünde seçeneği 1, **oturum oturum tam erişim** ve sağlamak **cihaz Yöneticisi parolası**. Kurulum Sihirbazı'nı oturumu başlatmak için aşağıdaki komutu yazın:
   
    `Invoke-HcsSetupWizard`
2. Cihaz kaydı için kullanılan Kurulum Sihirbazı'nı ilk kez kullanıyorsanız, web proxy yapılandırması gelene kadar tüm gerekli ağ ayarlarını yapılandırmanız gerekir. Aygıtınız zaten kayıtlı değilse, web proxy yapılandırması gelene kadar tüm yapılandırılmış ağ ayarlarını kabul edin. Kurulum Sihirbazı'nda web proxy ayarlarını yapılandırmak isteyip istemediğiniz sorulduğunda yazın **Evet**.
3. İçin **Web Proxy URL'si**, IP adresini veya tam etki alanı adı (FQDN), web proxy sunucusu belirtin ve TCP bağlantı noktası numarası Bulutu ile iletişim kurarken kullanması için Cihazınızı istersiniz. Aşağıdaki biçimi kullanın:
   
    `http://<IP address or FQDN of the web proxy server>:<TCP port number>`
   
    Varsayılan olarak, TCP bağlantı noktası 8080 belirtilir.
4. Kimlik doğrulaması türü olarak seçin **NTLM**, **temel**, veya **hiçbiri**. Proxy sunucu yapılandırması için en az güvenli kimlik doğrulaması temel kullanılır. NT LAN Yöneticisi (NTLM) olan bir üç yönlü bir ileti sistemi (bazen dört ek bütünlük gerekliyse) kullanan bir yüksek güvenlikli ve karmaşık kimlik doğrulama protokolü bir kullanıcının kimliğini doğrulamak için. Varsayılan kimlik doğrulaması, NTLM olur. Daha fazla bilgi için bkz: [temel](http://hc.apache.org/httpclient-3.x/authentication.html) ve [NTLM kimlik doğrulaması](http://hc.apache.org/httpclient-3.x/authentication.html). 
   
   > [!IMPORTANT]
   > **StorSimple cihaz Yöneticisi hizmeti cihaz izleme grafikleri basit çalışmıyor veya NTLM kimlik doğrulaması cihaz için proxy sunucusu yapılandırmasını etkinleştirilir. İzleme grafikleri çalışması, kimlik doğrulamasının NONE olarak ayarlandığından emin olmak gerekir.**
  
5. Kimlik doğrulaması etkinleştirilirse, kaynağı bir **Web Proxy Kullanıcı** ve **Web Proxy parola**. Parolayı onaylamak gerekir.
   
    ![StorSimple cihaz1 Web Proxy'yi Yapılandır](./media/storsimple-configure-web-proxy/IC751830.png)

İlk kez Cihazınızı kaydediyorsanız kayıt ile devam edin. Aygıtınız zaten kayıtlı değilse, sihirbaz kapanır. Yapılandırılmış ayarları kaydedilir.

Web proxy şimdi etkinleştirildi. Atlayabilirsiniz [etkinleştirmek web proxy](#enable-web-proxy) adım ve doğrudan gitmek [Azure portalında web proxy ayarlarını görüntülemek](#view-web-proxy-settings-in-the-azure-portal).

## <a name="configure-web-proxy-via-windows-powershell-for-storsimple-cmdlets"></a>StorSimple cmdlet'leri için Windows PowerShell aracılığıyla Web Proxy'yi Yapılandır

Web proxy ayarlarını yapılandırmak için alternatif StorSimple cmdlet'leri için Windows PowerShell aracılığıyla bir yoludur. Web proxy yapılandırmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-configure-web-proxy-via-cmdlets"></a>Web proxy cmdlet'leri aracılığıyla yapılandırmak için
1. Seri konsol menüsünde seçeneği 1, **oturum oturum tam erişim**. İstendiğinde, sağlayın **cihaz Yöneticisi parolası**. Varsayılan parola `Password1`.
2. Komut istemine yazın:
   
    `Set-HcsWebProxy -Authentication NTLM -ConnectionURI "<http://<IP address or FQDN of web proxy server>:<TCP port number>" -Username "<Username for web proxy server>"`
   
    Sağlayın ve istendiğinde parolayı onaylayın.
   
    ![Üzerinde StorSimple cihaz3'te Web Proxy'yi Yapılandır](./media/storsimple-configure-web-proxy/IC751831.png)

Web proxy yapılandırılmıştır ve etkinleştirilmesi gerekir.

## <a name="enable-web-proxy"></a>Web ara sunucusunu etkinleştirme

Web proxy varsayılan olarak devre dışıdır. StorSimple Cihazınızda web proxy ayarlarını yapılandırdıktan sonra StorSimple için Windows PowerShell web proxy ayarlarını etkinleştirmek için kullanın.

> [!NOTE]
> **Web proxy yapılandırmak için Kurulum Sihirbazı'nı kullandıysanız, bu adım gerekli değildir. Web ara Sunucusu Kurulum Sihirbazı oturumu sonra otomatik olarak varsayılan olarak etkindir.**


Web proxy Cihazınızda etkinleştirmek StorSimple için Windows PowerShell içinde aşağıdaki adımları gerçekleştirin:

#### <a name="to-enable-web-proxy"></a>Web proxy etkinleştirmek için
1. Seri konsol menüsünde seçeneği 1, **oturum oturum tam erişim**. İstendiğinde, sağlayın **cihaz Yöneticisi parolası**. Varsayılan parola `Password1`.
2. Komut istemine yazın:
   
    `Enable-HcsWebProxy`
   
    StorSimple Cihazınızda web proxy yapılandırması şimdi etkinleştirdiniz.
   
    ![Üzerinde StorSimple cihaz4'te Web Proxy'yi Yapılandır](./media/storsimple-configure-web-proxy/IC751832.png)

## <a name="view-web-proxy-settings-in-the-azure-portal"></a>Azure portalında Web proxy ayarlarını görüntüleme

Web proxy ayarları Windows PowerShell arabirimi üzerinden yapılandırılır ve portalın içinde değiştirilemez. Ancak, bu yapılandırılmış ayarları Portalı'nda görüntüleyebilirsiniz. Web proxy görüntülemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-view-web-proxy-settings"></a>Web proxy ayarlarını görüntülemek için
1. Gidin **StorSimple cihaz Yöneticisi hizmeti > aygıtları**. Seçin ve bir cihaza tıklayın ve ardından Git **Aygıt Ayarları > Ağ**.

    ![Ağ'ı tıklatın](./media/storsimple-8000-configure-web-proxy/view-web-proxy-1.png)

2. İçinde **ağ ayarlarını** dikey penceresinde tıklatın **Web proxy** döşeme.

    ![Web proxy'yi tıklatın](./media/storsimple-8000-configure-web-proxy/view-web-proxy-2.png)

3. İçinde **Web proxy** dikey penceresinde, StorSimple Cihazınızda yapılandırılmış web proxy ayarlarını gözden geçirin.
   
    ![Görünümü web proxy ayarları](./media/storsimple-8000-configure-web-proxy/view-web-proxy-3.png)


## <a name="errors-during-web-proxy-configuration"></a>Web proxy yapılandırması sırasında hatalar

Web proxy ayarları yanlış yapılandırılmış hata iletileri kullanıcı Windows PowerShell için StorSimple için görüntülenir. Aşağıdaki tabloda bazı bu hata iletileri, olası nedenler ve önerilen eylemler açıklanmaktadır.

| Seri yok. | HRESULT hata kodu | Olası temel nedeni | Önerilen eylem |
|:--- |:--- |:--- |:--- |
| 1. |0x80070001 |Pasif denetleyicisinden komutu çalıştırın ve etkin denetleyicisi ile iletişim kurabildiğinden değil. |Komutu etkin denetleyicisinde çalıştırın. Pasif denetleyicisinden komutu çalıştırmak için bağlantısını pasif active denetleyicisi düzeltmeniz gerekir. Bu bağlantı bozuk ise Microsoft Support devreye gerekir. |
| 2. |0x800710dd - işlemi tanımlayıcısı geçerli değil |Proxy ayarlarını StorSimple bulut uygulaması üzerinde desteklenmez. |Proxy ayarlarını StorSimple bulut uygulaması üzerinde desteklenmez. Bu, yalnızca bir StorSimple fiziksel cihazda yapılandırılabilir. |
| 3. |0x80070057 - geçersiz parametre |İçin proxy ayarlarını sağlanan parametrelerden biri geçerli değil. |URI doğru biçimde sağlanmadı. Aşağıdaki biçimi kullanın:`http://<IP address or FQDN of the web proxy server>:<TCP port number>` |
| 4. |0x800706ba - RPC sunucusu kullanılamıyor |Kök nedeni aşağıdakilerden biridir:</br></br>Küme ayarlama değil. </br></br>DataPath hizmeti çalışmıyor.</br></br>Pasif denetleyicisinden komutunu çalıştırın ve etkin denetleyicisi ile iletişim kurabildiğinden değil. |Kümenin çalışır durumda ve datapath hizmetinin çalıştığından emin olmak için Microsoft Support göster.</br></br>Komutu etkin denetleyicisinden çalıştırın. Pasif denetleyicisinden komutu çalıştırmak istiyorsanız, pasif denetleyiciyi etkin denetleyicisi ile iletişim kurabildiğinden emin olmalısınız. Bu bağlantı bozuk ise Microsoft Support devreye gerekir. |
| 5. |0X800706be - RPC çağrısı başarısız oldu |Küme çalışmıyor. |Kümenin çalışır durumda olduğundan emin olmak için Microsoft Support göster. |
| 6. |0x8007138f - küme kaynak bulunamadı |Platform Hizmet küme kaynağı bulunamadı. Bu durum yükleme doğru değildi ortaya çıkar. |Cihazda bir Fabrika sıfırlaması gerçekleştirmeniz gerekebilir. Bir platform kaynak oluşturmanız gerekebilir. Sonraki adımlar için Microsoft Support başvurun. |
| 7. |0x8007138c - küme kaynağı çevrimiçi değil |Platform veya datapath küme kaynaklarının çevrimiçi değildir. |Datapath ve platform Hizmet kaynağı çevrimiçi olduğundan emin olmak için Microsoft Support başvurun. |

> [!NOTE]
> * Hata iletilerini Yukarıdaki liste geniş kapsamlı değildir.
> * Web proxy ayarlarıyla ilgili hatalar StorSimple Aygıt Yöneticisi'ni hizmetinizde Azure portalında görüntülenmez. Web proxy ile ilgili bir sorun varsa Yapılandırma tamamlandıktan sonra cihaz durumunu değiştirir **çevrimdışı** Klasik portalda. |

## <a name="next-steps"></a>Sonraki Adımlar
* Cihazınızı dağıtma veya web proxy ayarlarını yapılandırma sırasında herhangi bir sorunla karşılaşırsanız, başvurmak [StorSimple cihaz dağıtımına sorun giderme](storsimple-troubleshoot-deployment.md).
* StorSimple cihaz Yöneticisi hizmeti kullanmayı öğrenmek için şu adrese gidin [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

