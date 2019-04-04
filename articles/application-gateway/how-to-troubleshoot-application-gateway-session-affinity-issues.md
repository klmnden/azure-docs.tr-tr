---
title: Azure Application Gateway oturum benzeşimi sorunlarını giderme
description: Bu makale, Azure Application Gateway oturum benzeşimi sorunları gidermeye ilişkin bilgi sağlar.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/22/2019
ms.author: absha
ms.openlocfilehash: 0c1c466149b4992d99e18cfb1fd5d8416834df35
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58904534"
---
# <a name="troubleshoot-azure-application-gateway-session-affinity-issues"></a>Azure Application Gateway oturum benzeşimi sorunlarını giderme

Azure Application Gateway ile oturum benzeşimi sorunları tanılamak ve gidermek öğrenin.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="overview"></a>Genel Bakış

Tanımlama bilgilerine dayalı oturum benzeşimi özelliği, bir kullanıcı oturumunu aynı sunucuda tutmak istediğinizde kullanışlıdır. Ağ geçidi ile yönetilen tanımlama bilgilerini kullanan Application Gateway, sonraki trafiği işleme amacıyla bir kullanıcı oturumundan aynı sunucuya yönlendirebilir. Bu, oturum durumunun bir kullanıcı oturumuna ait sunucuya yerel olarak kaydedildiği durumlarda önemlidir.

## <a name="possible-problem-causes"></a>Olası bir sorunu neden olur

Tanımlama bilgisi tabanlı oturum benzeşimini sürdürmekten sorun, aşağıdaki ana nedenlerden ötürü oluşabilir:

- "Tanımlama bilgisi temelli benzeşimi" ayarı etkin değil
- Uygulamanızı tanımlama bilgisi temelli benzeşimi işleyemiyor
- Uygulama, ancak yine de arka uç sunucuları arasında geçirmek istekleri tanımlama bilgisi temelli benzeşimi kullanma

### <a name="check-whether-the-cookie-based-affinity-setting-is-enabled"></a>"Tanımlama bilgisi temelli benzeşimi" ayarı etkin olup olmadığını denetleyin

Bazen "Tanımlama bilgisi benzeşimi tabanlı" ayarı etkinleştirmek unuttuklarında oturum benzeşimi sorunlarını ortaya çıkabilir. Azure portalında HTTP ayarları sekmesinde "Tanımlama bilgisi benzeşimi tabanlı" ayarı etkin olup olmadığını belirlemek için yönergeleri izleyin:

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. İçinde **sol gezinti** bölmesinde tıklayın **tüm kaynakları**. Tüm kaynaklar dikey penceresinde Uygulama ağ geçidi adına tıklayın. Zaten seçili aboneliği çeşitli kaynaklar varsa, uygulama ağ geçidi adı girebilirsiniz **ada göre Filtrele...** girebilirsiniz.

3. Seçin **HTTP ayarları** sekmesinde altında **ayarları**.

   ![sorun giderme-oturum-benzeşim-sorunları-1](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-1.png)

4. Tıklayın **appGatewayBackendHttpSettings** seçmiş olduğunuz olup olmadığını denetlemek için sağ taraftaki **etkin** tanımlama bilgisi tabanlı benzeşim için.

   ![sorun giderme-oturum-benzeşim-sorunları-2](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-2.jpg)



Değerini de göz atabilirsiniz "**CookieBasedAffinity**" ayarlanmış *etkin*altında "**backendHttpSettingsCollection**" aşağıdaki yöntemlerden birini kullanarak:

- Çalıştırma [Get-AzApplicationGatewayBackendHttpSettings](https://docs.microsoft.com/powershell/module/az.network/get-azapplicationgatewaybackendhttpsettings) PowerShell
- Azure Resource Manager şablonu kullanarak bir JSON dosyası aracılığıyla arayın

```
"cookieBasedAffinity": "Enabled", 
```

### <a name="the-application-cannot-handle-cookie-based-affinity"></a>Uygulamanın, tanımlama bilgisi temelli benzeşimi işleyemiyor

#### <a name="cause"></a>Nedeni

Uygulama ağ geçidi, yalnızca bir tanımlama bilgisi kullanarak oturum tabanlı benzeşim gerçekleştirebilirsiniz.

#### <a name="workaround"></a>Geçici çözüm

Uygulamanın, tanımlama bilgisi temelli benzeşimi yapamıyorsa, bir dış veya iç azure yük dengeleyici veya başka bir üçüncü taraf çözümü kullanmanız gerekir.

### <a name="application-is-using-cookie-based-affinity-but-requests-still-bouncing-between-back-end-servers"></a>Uygulama, ancak yine de arka uç sunucuları arasında geçirmek istekleri tanımlama bilgisi temelli benzeşimi kullanma

#### <a name="symptom"></a>Belirti

Internet Explorer'da, örneğin bir kısa ad URL'yi kullanarak uygulama ağ geçidi eriştiğinizde, tanımlama bilgisi tabanlı benzeşim ayarı etkinleştirmiş olmanız gerekir: [ http://website ](http://website/) , istek yine de arka uç sunucuları arasında geçirmek.

Bu sorunu tanımlamak için yönergeleri izleyin:

1. "Uygulama Gateway(We are using Fiddler in this example) arkasında uygulamaya bağlanma istemcide" bir web hata ayıklayıcı izleme yararlanın.
    **İpucu** fiddler'ı kullanmayı bilmiyorsanız seçeneği işaretleyin "**ağ trafiğini toplama ve web hata ayıklayıcıyı kullanarak incelemek istiyorum**" altındaki.

2. Denetleyin ve istemci tarafından sağlanan tanımlama bilgilerini ARRAffinity ayrıntılarını sahip olup olmadığını belirlemek için oturumu günlüklerini analiz edin. ARRAffinity ayrıntıları gibi bulamadığınız, "**ARRAffinity =** *ARRAffinityValue*" düzenleme tanımlama bilgisi ile istemci yanıtlama değil tarafından sağlanan anlamına gelen tanımlama bilgisi kümesi içinde Uygulama ağ geçidi.
    Örneğin:

    ![sorun giderme-oturum-benzeşim-sorunları-3](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-3.png)

        ![troubleshoot-session-affinity-issues-4](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-4.png)

Uygulama yanıtı alır kadar her istekte tanımlama bilgisi ayarlamak denemeye devam eder.

#### <a name="cause"></a>Nedeni

Internet Explorer ve diğer tarayıcılar değil depolayabilen veya tanımlama bilgisinin bir kısa ad URL ile kullanmak için bu sorun oluşur.

#### <a name="resolution"></a>Çözüm

Bu sorunu gidermek için bir FQDN kullanarak uygulama ağ geçidi erişmelidir. Örneğin, [ http://website.com ](http://website.com/) veya [ http://appgw.website.com ](http://appgw.website.com/) .

## <a name="additional-logs-to-troubleshoot"></a>Sorun giderme için ek Günlükler

Ek günlükleri toplayıp bunları sorunları ilgili tanımlama bilgilerine dayalı oturum benzeşimi sorunlarını gidermek için analiz

### <a name="analyze-application-gateway-logs"></a>Application Gateway günlüklerini çözümleme

Application Gateway günlüklerini toplamak için yönergeleri izleyin:

Azure portaldan günlüğe kaydetmeyi etkinleştirme

1. İçinde [Azure portalında](https://portal.azure.com/), kaynağınızı bulun ve ardından **tanılama günlükleri**.

   Application Gateway için üç günlükleri kullanılabilir: Erişim günlüğü, performans günlüğü, güvenlik duvarı günlüğü

2. Veri toplamaya başlamak için tıklayın **tanılamayı Aç**.

   ![sorun giderme-oturum-benzeşim-sorunları-5](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-5.png)

3. **Tanılama ayarları** dikey penceresinde tanılama günlükleri için ayarları sağlar. Bu örnekte, Log Analytics, günlükleri depolar. Tıklayın **yapılandırma** altında **Log Analytics** çalışma alanınızı ayarlamak için. Tanılama günlüklerini kaydetmek için Event Hubs'ı veya depolama hesabını da kullanabilirsiniz.

   ![sorun giderme-oturum-benzeşim-sorunları-6](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-6.png)

4. Ayarları onaylayın ve ardından **Kaydet**.

   ![sorun giderme-oturum-benzeşim-sorunları-7](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-7.png)

#### <a name="view-and-analyze-the-application-gateway-access-logs"></a>Görüntüleme ve uygulama ağ geçidi erişim günlüklerini çözümleme

1. Uygulama ağ geçidi kaynak görünümü altında Azure portalında **tanılama günlükleri** içinde **izleme** bölümü.

   ![sorun giderme-oturum-benzeşim-sorunları-8](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-8.png)

2. Sağ tarafta seçin "**ApplicationGatewayAccessLog**" altındaki aşağı açılan listede **günlük kategorileri.**  

   ![sorun giderme-oturum-benzeşim-sorunları-9](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-9.png)

3. Uygulama ağ geçidi erişim günlüğü listesinde analiz ve dışarı aktarmak istediğiniz günlüğe tıklayın ve ardından JSON dosyasını dışarı aktarın.

4. CSV dosyası için 3. adımda dışarı aktardığınız bir JSON dosyası dönüştürün ve Excel, Power BI veya diğer herhangi bir veri görselleştirme aracını görüntüleyin.

5. Aşağıdaki veriler denetleyin:

- **Clientıp**– istemci IP adresinden bağlanan istemcinin budur.
- **ClientPort** -kaynak bağlantı noktasından bağlanan istemcinin istek için budur.
- **RequestQuery** – Bu, hedef sunucunun isteği aldığı gösterir.
- **Sunucu yönlendirilen**: Arka uç havuzu örneği isteği aldığı.
- **X-AzureApplicationGateway-günlük-ID**: İstek için kullanılan bağıntı kimliği. Arka uç sunucularda trafiği sorunları gidermek için kullanılabilir. Örneğin: AzureApplicationGateway ÖNBELLEK İSABET X 0 = & SERVER YÖNLENDİRİLEN 10.0.2.4 =.

  - **SUNUCU DURUMU**: Application Gateway, arka uçtan alınan HTTP yanıt kodu.

  ![sorun giderme-oturum benzeşimi-sorunları-11](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-11.png)

İki öğe geliyor ve aynı Clientıp ve istemci bağlantı noktası ve aynı arka uç sunucusuna gönderilen görürseniz, bu uygulama ağ geçidi doğru yapılandırılmış anlamına gelir.

İki öğe aynı Clientıp ve istemci bağlantı noktası geliyor ve farklı arka uç sunucularına gönderilmeden görürseniz anlamına isteği arka uç sunucuları arasında seçim geçirmek "**uygulama tanımlama bilgisi temelli benzeşim isteklerini ancak kullanma yine de arka uç sunucuları arasında geçirmek**"altındaki giderileceği.

### <a name="use-web-debugger-to-capture-and-analyze-the-http-or-https-traffics"></a>Web hata ayıklayıcısı yakalamak ve HTTP veya HTTPS traffics çözümlemek için kullanın

Web Fiddler gibi hata ayıklama araçları, Internet ve test bilgisayarlar arasındaki ağ trafiğini yakalayarak web uygulamalarında hata ayıklama yardımcı olabilir. Bu araçlar, tarayıcı alır/bunları gönderir gibi gelen ve giden verileri incelemek etkinleştirin. Fiddler, bu örnekte, özellikle bir sorun kimlik doğrulaması türü için web uygulamaları ile istemci tarafı sorunlarını gidermenize yardımcı olabilecek HTTP yeniden yürütme seçeneği vardır.

Tercih ettiğiniz web hata ayıklayıcıyı kullanın. Bu örnekte fiddler'ı kullanacağız yakalama ve http veya https traffics çözümlemek için yönergeleri izleyin:

1. Fiddler aracı, indirmek <https://www.telerik.com/download/fiddler>.

    > [!NOTE]
    > Yakalama bilgisayarda .NET 4'ün yüklü olduğunda Fiddler4 seçin. Aksi takdirde, Fiddler2 seçin.

2. Kurulum yürütülebilir dosyasına sağ tıklayın ve yüklemek için yönetici olarak çalıştırın.

            ![troubleshoot-session-affinity-issues-12](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-12.png)

3. Fiddler'ı açtığınızda, otomatik olarak (yakalama sol alt köşesinde bir bildirim) trafiğini yakalamaktan başlamalıdır. Başlatma veya durdurma trafik yakalama için F12 tuşuna basın.

        ![troubleshoot-session-affinity-issues-13](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-13.png)

4. Büyük olasılıkla, şifresi çözülmüş HTTPS trafiği ilgilenecek ve HTTPS şifre çözme seçerek etkinleştirebilirsiniz **Araçları** > **Fiddler seçenekleri**ve onay kutusunu " **şifresini çözme HTTPS trafiğini**".

        ![troubleshoot-session-affinity-issues-14](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-14.png)

5. Sorunu yeniden oluştururken tıklayarak önce önceki ilgisiz oturumları kaldırabilirsiniz **X** (simge) > **Tümünü Kaldır** ekran izleyin: 

        ![troubleshoot-session-affinity-issues-15](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-15.png)

6. Sorunu yeniden oluşturduktan sonra dosyayı gözden geçirme seçerek kaydedin **dosya** > **Kaydet** > **tüm oturumlar...** . 

        ![troubleshoot-session-affinity-issues-16](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-16.png)

7. Denetleyin ve sorunun ne olduğunu belirlemek için oturumu günlüklerini analiz edin.

    Örnekler için:

- **Örnek c:** İstek istemci tarafından gönderilen ve uygulama ağ geçidinin genel IP adresine geçer bir oturum bulunamadı, ayrıntıları görüntülemek için bu günlüğü'nü tıklatın.  Sağ tarafta alt kutusundaki ne uygulama ağ geçidi istemciye döndürüyor verilerdir. "Ham" sekmesini seçin ve istemci alıp almadığını belirlemek bir "**Set-Cookie: ARRAffinity =** *ARRAffinityValue*. " Tanımlama bilgisi varsa, oturum benzeşimi ayarlanmamış veya Application Gateway, istemciye tanımlama bilgisi uygulamıyor.

   > [!NOTE]
   > Application Gateway istemcinin belirli bir arka uç sunucusuna gönderilmesi ayarlar tanımlama bilgisi-id ARRAffinity değerdir.

    ![sorun giderme-oturum-benzeşim-sorunları-17](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-17.png)

- **Örnek B:** Sonraki oturum önceki geri uygulama ARRAAFFINITY ayarladı ağ geçidi için yanıt istemci biridir ardından. ARRAffinity tanımlama bilgisi kimliği eşleşiyorsa paket daha önce kullanılan aynı arka uç sunucusuna gönderilmelidir. Http iletişimi için istemcinin ARRAffinity tanımlama bilgisini değiştiriyor olup olmadığını görmek için sonraki birkaç satırlık denetleyin.

    ![sorun giderme-oturum-benzeşim-sorunları-18](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-18.png)

> [!NOTE]
> Aynı iletişim oturum tanımlama bilgisinin değiştirme olmalıdır. Sağ taraftaki ilk kutuyu işaretleyin, istemciye tanımlama bilgisini kullanarak ve geri uygulama ağ geçidine gönderilmeden görmek için "Tanımlama bilgileri" sekmesini seçin. Aksi durumda, istemci tarayıcısı tutulması ve tanımlama bilgisi konuşmaları kullanarak değil. Bazı durumlarda, istemci kaynaklanıyor olabilir.

 

## <a name="next-steps"></a>Sonraki adımlar

Yukarıdaki adımlar sorunu çözmezse, açık bir [destek bileti](https://azure.microsoft.com/support/options/).