---
title: Azure trafik analytics sık sorulan sorular | Microsoft Docs
description: Bazı trafiği analytics hakkında sık sorulan soruların yanıtlarını alın.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/08/2018
ms.author: jdial
ms.openlocfilehash: de85e4295a59c54cb68306bf0cbc516bf5e1f8e2
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36313300"
---
# <a name="traffic-analytics-frequently-asked-questions"></a>Trafik analytics sık sorulan sorular

1.  Trafik analytics kullanmak için ön koşullar nelerdir?

    Trafik Analytics aşağıdaki önkoşulları gerektirir:

    - Ağ İzleyicisi'ni etkin abonelik
    - İzlemek istediğiniz Nsg'ler için etkin NSG akış günlükleri
    - Ham akış günlükleri depolamak için bir Azure Storage hesabı
    - Okuma ve yazma erişimi olan bir günlük analizi (OMS) çalışma alanı
    - Kullanıcıya abonelik düzeyinde aşağıdaki rolleri birini ile atanmalıdır:
    
    1.  Şu Klasik yönetici herhangi biri olması gerekir
    
        - Hesap yöneticisi
        - Hizmet yöneticisi 
        - Ortak yönetici
        
    2.  Hesabınızda herhangi biri şu RBAC rolleri aboneliği kapsamında olması gerekir.
    
        - Sahip
        - Katılımcı
        - Okuyucu
        - Ağ Katılımcısı

    3. Hesabınızda herhangi bir özel RBAC rolü tüm aşağıdaki sözü edilen eylemler için izne sahip abonelik düzeyinde olması gerekir.
            
        - Microsoft.Network/applicationGateways/read
        - Microsoft.Network/connections/read
        - Microsoft.Network/loadBalancers/read 
        - Microsoft.Network/localNetworkGateways/read 
        - Microsoft.Network/networkInterfaces/read 
        - Microsoft.Network/networkSecurityGroups/read 
        - Microsoft.Network/publicIPAddresses/read
        - Microsoft.Network/routeTables/read
        - Microsoft.Network/virtualNetworkGateways/read 
        - Microsoft.Network/virtualNetworks/read
        
    Bir abonelik için bir kullanıcıya atanan rollerin denetlemek için lütfen aşağıdaki adımları izleyin:

    Azure kullanarak oturum açma **Login-AzureRmAccount** 

    Gerekli olan abonelik kullanarak seçin **Select-AzureRmSubscription** 

    Belirtilen bir kullanıcıya atanmış olan tüm rolleri listelemek için artık kullanmak **Get-AzureRmRoleAssignment - SignInName <user email> - IncludeClassicAdministrators** 

    Yürütme commends sonra herhangi bir çıktı görmüyorsanız sonra lütfen ilgili abonelik Yönetici'ye komutları yürütmek için erişim sağlamak için ulaşın.  

    Daha fazla ayrıntı Lütfen başvurun [rol tabanlı erişim denetimini Azure PowerShell ile yönetme](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-powershell)


2.  Hangi Azure bölgeleri trafiği analytics kullanılabilir?

    Önizleme sürümde karşın, trafik analizi için Nsg'ler herhangi birini kullanabilirsiniz **bölgeler desteklenen**: Batı Orta ABD, Doğu ABD, Doğu ABD 2, Kuzey Orta ABD, Orta Güney ABD, Orta ABD, Batı ABD, Batı ABD 2, Batı Avrupa, Kuzey Avrupa Birleşik Krallık Batı, Birleşik Krallık Güney, Avustralya Doğu, Avustralya Güneydoğu ve Güneydoğu Asya. Günlük analizi çalışma alanı, Batı Orta ABD, Doğu ABD, Batı Avrupa, Birleşik Krallık Güney, Avustralya Güneydoğu veya Güneydoğu Asya bölge içinde bulunmalıdır.

3.  Akış etkinleştirebilirim Nsg'ler olabilir günlüklerini olması OMS çalışma Alanım'den farklı bölgelerdeki?

    Evet

4.  Birden çok Nsg'ler tek bir çalışma alanı içinde yapılandırılabilir mi?

    Evet

5.  Var olan bir OMS çalışma kullanabilir miyim?

    Evet, var olan bir çalışma öğesini seçerseniz, yeni sorgu dili geçirildiğinden emin olun. Çalışma alanına yükseltmeye istemiyorsanız; Yeni bir tane oluşturmanız gerekir. Yeni sorgu dili hakkında daha fazla bilgi için bkz: [Azure günlük analizi yükseltmek için yeni günlük arama](../log-analytics/log-analytics-log-search-upgrade.md).

6.  Azure depolama Hesabımı bir abonelikte kullanılabilir ve farklı bir abonelikte OMS çalışma Alanım olabilir?

    Evet

7.  Farklı bir abonelik farklı depolama hesabında ham günlükleri depolayabilirsiniz?

    Hayır. Ham günlükleri depolamak bir NSG akış günlükleri için etkin olduğu tüm depolama hesabında ancak, depolama hesabı ve raw günlükleri aynı abonelikte ve bölgede olması gerekir.

8.  I trafiğini analiz için bir NSG yapılandırırken "Bulunamadı" hatasını alırsanız, nasıl ı çözebilmek?

    Soru 2 listelenen desteklenen bir bölge seçin. Desteklenmeyen bir bölge seçerseniz, "Bulunamadı" hatasını alıyorsunuz.

9.  NSG akış günlükleri altında NSG durumu "yüklemek için başarısız", alıyorum ne yapılacağına?

    Microsoft.ınsights sağlayıcısı için düzgün çalışması için günlük akışı kayıtlı olması gerekir. Aboneliğiniz için değil veya Microsoft.ınsights sağlayıcısı kayıtlı olup olmadığından emin değilseniz, Değiştir *xxxxx-xxxxx-xxxxxx-xxxx* aşağıdaki komutu ve aşağıdaki komutları çalıştırın powershell'den:

    ```powershell-interactive
    **Select-AzureRmSubscription** -SubscriptionId xxxxx-xxxxx-xxxxxx-xxxx
    **Register-AzureRmResourceProvider** -ProviderNamespace Microsoft.Insights
    ```

10. Çözüm yapılandırmış olduğunuz. Neden bir şey Panoda görüyorum değil mi?

    Panoyu ilk kez görünmesi 30 dakika kadar sürebilir. Çözümü ilk herhangi bir raporu oluşturulmadan önce anlamlı bilgiler çıkarmanıza kendisine için yeterli veri toplaması gerekir. 

11.  Aşağıdaki ileti alırsanız: "hiçbir veri bu çalışma alanında seçilen zaman aralığı için bulamadık. Zaman aralığını değiştirmeyi deneyin veya farklı bir çalışma alanı seçin"nasıl onu giderebilirim?

        Aşağıdaki seçenekleri deneyin:
        - Zaman aralığı üst çubuğunda değiştirmeyi deneyin.
        - Üst araç çubuğunda farklı bir günlük analizi çalışma alanı seçin
        - Yakın zamanda etkinleştirildiyse trafiği Analytics 30 dakika sonra erişmeyi deneyin
    
        Sorun devam ederse, sorunları Yükselt [kullanıcı ses Forumu](https://feedback.azure.com/forums/217313-networking?category_id=195844).

12.  Aşağıdaki ileti alırsanız: "1), NSG çözümleme akış günlükleri ilk kez. Bu işlemin tamamlanması 20-30 dakika sürebilir. Bir süre sonra yeniden denetleyin. 2) yukarıdaki adım çalışmaz ve çalışma alanınızı ücretsiz SKU ardından bir kotanın üzerinde doğrulamak için çalışma alanı kullanımınızı denetleyin, aksi takdirde sık sorulan sorular için daha fazla bilgi için başvurun", nasıl onu giderebilirim?

        Aşağıdaki nedenlerle hata iletisini alabilirsiniz:
        - Trafiğini analiz son etkinleştirilmiş ve henüz yeterli daha anlamlı bilgiler çıkarmaya toplanmış verilerine değil.
        - Ücretsiz SKU OMS çalışma alanınız olduğundan ve kota sınırlarının ihlal. Bu durumda, bir çalışma alanında bir SKU daha büyük kapasitede kullanmanız gerekebilir.
    
        Sorun devam ederse, sorunları Yükselt [kullanıcı ses Forumu](https://feedback.azure.com/forums/217313-networking?category_id=195844).
    
13.  Aşağıdaki ileti alırsanız: "sahip olduğumuz kaynakları verileri (topoloji) ve akış bilgisi yok gibi görünüyor. Bu sırada, buraya tıklayın ve sık sorulan sorular için daha fazla bilgi için bkz kaynakları verileri görmek için", nasıl giderebilirim onu?

        Panoda kaynak bilgileri görüyorsunuz; Ancak, hiçbir akışıyla ilgili istatistikleri mevcut değildir. Veri kaynakları arasında hiçbir iletişim akışını nedeniyle bulunmuyor olabilir. İçin 60 dakika bekleyin ve durum yeniden denetleyin. Kaynaklar arasında iletişimi akışları mevcut sonra içindeki sorunları Yükselt emin olduğunuzda [kullanıcı ses Forumu](https://feedback.azure.com/forums/217313-networking?category_id=195844).

14. PowerShell veya Azure Resource Manager şablonu kullanarak trafiği analytics yapılandırabilir miyim?

        Yes, traffic analytics configuration using windows powershell is supported from version 6.2.1 onwards, however Azure Resource Manager template support is not available at present. To learn more, how PowerShell can be used to configure traffic analytics please refer following [documentation](https://docs.microsoft.com/en-us/powershell/module/azurerm.network/set-azurermnetworkwatcherconfigflowlog?view=azurermps-6.2.0). 

15.  Trafik analytics nasıl fiyatlandırılır?

        Hizmet tarafından işlenir ve günlük analizi çalışma alanında sonuçlandı Gelişmiş günlüklerini depolamak akışı günlük verileri için trafiği analytics ölçülen. Plan fiyatlandırması hakkında daha fazla Lütfen bilmeniz [burayı tıklatın](https://azure.microsoft.com/en-us/pricing/details/network-watcher/) 

16.  Coğrafi harita görünümünde klavyeyi kullanarak nasıl gidebilirsiniz?

        Coğrafi harita sayfa iki ana bölümleri içerir:
    
        - **Başlık**: coğrafi harita üst yerleştirilen başlık Deployment/No dağıtım/etkin/devre dışı/trafik Analytics etkin/trafik Analytics değil/trafiği etkin gibi düğmeleri üzerinden trafik dağıtım filtreleri seçin yeteneği sağlar Ülke/Benign/kötü amaçlı/izin verilen kötü amaçlı trafiği ve gösterge bilgileri. Tanımlı düğmeleri seçimde harita dağıtımınızı "Active" Veri merkezlerindeki vurgular sonra gibi bir kullanıcı başlığı altında "Active" filtre düğmesini seçerse bu haritada ilgili filtre uygulanır.
        - **Harita**: Başlık yerleştirilen eşlemesi bölümü Azure veri merkezleri ile ülkelerde arasında trafik dağılımı gösterir.
    
        **Başlık çubuğunda klavye gezinme**
    
        - Varsayılan olarak, başlık coğrafi harita sayfasında "Azure DC'leri" düğmesine filtre seçimdir.
        - Başka bir filtre düğmesine gitmek için kullanabilir `Tab` veya `Right arrow` sonraki taşımak için anahtar. Geriye doğru gidin, kullanın ya da `Shift+Tab` veya `Left arrow` anahtarı. İleri gezinti yönü öncelik sağa sol alta üst tarafından izlenen.
        - Tuşuna `Enter` veya `Down` seçilen filtre uygulamak için ok tuşu. Filtre seçimini ve dağıtım bağlı olarak, bir veya birden çok düğüm eşlemesi bölümü altında vurgulanır.
            - Arasında geçiş yapmak için **başlık** ve **harita**, basın `Ctrl+F6`.
        
        **Klavye gezinti haritaya**
    
        - Başlıktaki herhangi bir filtre seçili ve basılı sonra `Ctrl+F6`, odağı vurgulanan düğümlerinden biri için taşır (**Azure veri merkezi** veya **ülke/bölge**) eşlemesi görünümünde.
        - Vurgulanan diğer düğümlere eşlemesindeki gitmek için kullanabilir `Tab` veya `Right arrow` anahtarları İleri hareketi için ve `Shift+Tab` veya `Left arrow` geriye dönük taşıma için anahtar.
        - Vurgulanan düğümlerinden eşlemesinde seçmek için kullanın `Enter` veya `Down arrow` anahtarı.
        - Bu tür bir düğüm seçimde odağı taşır **bilgileri araç kutusu** düğümü için. Varsayılan olarak kapalı düğmesi odağı taşır **bilgileri araç kutusu**. Daha fazla içinde gezinmek için **kutusunu** görüntülemek için kullanmak `Right` ve `Left arrow` ileriye ve geriye doğru sırasıyla taşımak için anahtarları. Tuşuna basarak `Enter` odaklanmış düğmesini seçerek aynı etkiye sahiptir **bilgileri araç kutusu**.
        - Tuşuna basarak `Tab` odağı açıkken **bilgileri araç kutusu**, Seçili düğümün olarak aynı kıtada uç noktaları odağı taşır. Kullanabileceğiniz `Right` ve `Left arrow` Bu uç noktalar arasında gezinmek için anahtarları.
        - Diğer akış uç noktalar/Kıtalar kümeye gidin `Tab` İleri hareketi için ve `Shift+Tab` geriye dönük taşıma için.
        - Odağı sonra `Continent clusters`, kullanın `Enter` veya `Down` kıtadan geçmeden küme içindeki uç noktaları vurgulamak için ok tuşlarını. Uç noktaları ve kıtadan geçmeden kümesinin bilgileri kutusunu Kapat düğmesine gezinmek için kullanın ya da `Right` veya `Left arrow` ileriye ve geriye doğru taşıma için sırasıyla anahtar. Herhangi bir noktadaki kullandığınız `Shift+L` seçili düğümden uç noktası için bağlantı hattı geçmek için. Tuşuna basarak `Shift+L` yeniden seçili uç noktasına gider.
        
        Her aşamada:
    
        - `ESC` Genişletilmiş seçimi daraltır.
        - `UP Arrow` Anahtar ile aynı eylemi gerçekleştirir `ESC`. `Down arrow` Anahtar ile aynı eylemi gerçekleştirir `Enter`.
        - Kullanım `Shift+Plus` , yakınlaştırma ve `Shift+Minus` uzaklaştırmak için.

17. Klavyeyi kullanarak VNet topoloji görünümünde nasıl gidebilirsiniz?

    Sanal ağ topolojisi sayfasında, iki ana bölümleri içerir:
    
    - **Başlık**: sanal ağlar topolojisinin üst yerleştirilen başlık bağlı sanal ağlar/bağlantı kesildi sanal ağlar/etkin/devre dışı/üzerinde-şirket içi/Azure bölgesi/genel IP'ler gibi düğmeleri üzerinden trafik dağıtım filtreleri seçin yeteneği sağlar / Ağır/Orta/düşük/izin verilen/engellenen ve gösterge bilgileri. Tanımlı düğmeleri seçimde topoloji dağıtımınızda bulunan "Active" sanal ağlar vurgular sonra gibi bir kullanıcı başlığı altında "Active" filtre düğmesini seçerse topoloji, ilgili filtre uygulanır.
    - **Topoloji**: topolojisi Bölümü başlık yerleştirilen sanal ağlar arasındaki trafik dağılımı gösterir.
    
    **Başlık çubuğunda klavye gezinme**
    
    - Varsayılan olarak, başlık sanal ağ topolojisi sayfasında "Bağlı sanal ağlar" düğmesine filtre seçimdir.
    - Başka bir filtre düğmesine gitmek için kullanabileceğiniz `Tab` sonraki taşımak için anahtar. Geriye doğru gidin, `Shift+Tab` anahtarı. İleri gezinti yönü öncelik sağa sol alta üst tarafından izlenen.
    - Tuşuna `Enter` seçilen filtre uygulamak için ok tuşu. Filtre seçimini ve dağıtım bağlı olarak, bir veya birden çok düğüm (VNet) topolojisi bölümü altında vurgulanır.
        - Arasında geçiş yapmak için **başlık** ve **topoloji**, basın `Ctrl+F6`.
        
    **Klavye gezinti topolojisi hakkında**
    
    - Başlıktaki herhangi bir filtre seçili ve basılı sonra `Ctrl+F6`, odağı vurgulanan düğümlerinden biri için taşır (**VNet**) topoloji görünümünde.
    - Diğer gitmek için kullanabileceğiniz topoloji görünümü düğümler vurgulanmış `Shift+Right arrow` İleri hareketi için anahtar. 
    - Odağı vurgulanan düğümlerinde taşır **bilgileri araç kutusu** düğümü için. Varsayılan olarak, odak "Ayrıntılar" düğmesine taşır **bilgileri araç kutusu**. Daha fazla içinde gezinmek için **kutusunu** görüntülemek için kullanmak `Right` ve `Left arrow` ileriye ve geriye doğru sırasıyla taşımak için anahtarları. Tuşuna basarak `Enter` odaklanmış düğmesini seçerek aynı etkiye sahiptir **bilgileri araç kutusu**.
    - Bu tür bir düğüm, buna ait tüm bağlantıları ziyaret, tek tek, basarak seçimini `Shift+Left arrow` anahtarı. Odağı taşır **bilgileri araç kutusu** o bağlantı. Herhangi bir noktada odağı düğüme geri tuşlarına basarak gölgeye `Shift+Right arrow` yeniden.
    

18. Alt ağ topolojisi görünümünde klavyeyi kullanarak nasıl gidebilirsiniz?

    Sanal ağlarla topolojisi sayfasında, iki ana bölümleri içerir:
    
    - **Başlık**: sanal ağlarla topolojisinin üst yerleştirilen başlık trafik dağıtım filtreleri etkin/devre dışı/dış bağlantıları/üzerinde-şirket içi/Azure bölgesi/genel IP'ler gibi düğmeleri aracılığıyla seçmek için yeteneği sağlar/etkin akar / Ağır/Orta/düşük/kötü amaçlı trafiği/izin verilen/engellenen, ağ geçidi alt ağları/arka uç alt ağları ve ön uç alt ağları. Tanımlı düğmeleri seçimde dağıtımınızda bulunan "Active" sanal alt ağ topolojisi vurgular sonra gibi bir kullanıcı başlığı altında "Active" filtre düğmesini seçerse topoloji, ilgili filtre uygulanır.
    - **Topoloji**: topolojisi Bölümü başlık yerleştirilen sanal alt ağlar arasındaki trafik dağılımı gösterir.
    
    **Başlık çubuğunda klavye gezinme**
    
    - Varsayılan olarak, başlık sanal ağlarla topolojisi sayfasında "Alt" düğmesine filtre seçimdir.
    - Başka bir filtre düğmesine gitmek için kullanabileceğiniz `Tab` sonraki taşımak için anahtar. Geriye doğru gidin, `Shift+Tab` anahtarı. İleri gezinti yönü öncelik sağa sol alta üst tarafından izlenen.
    - Tuşuna `Enter` seçilen filtre uygulamak için ok tuşu. Filtre seçimini ve dağıtım bağlı olarak, bir veya birden çok düğüm (alt ağ) topolojisi bölümü altında vurgulanır.
        - Arasında geçiş yapmak için **başlık** ve **topoloji**, basın `Ctrl+F6`.
        
    **Klavye gezinti topolojisi hakkında**
    
    - Başlıktaki herhangi bir filtre seçili ve basılı sonra `Ctrl+F6`, odağı vurgulanan düğümlerinden biri için taşır (**alt**) topoloji görünümünde.
    - Diğer gitmek için kullanabileceğiniz topoloji görünümü düğümler vurgulanmış `Shift+Right arrow` İleri hareketi için anahtar. 
    - Odağı vurgulanan düğümlerinde taşır **bilgileri araç kutusu** düğümü için. Varsayılan olarak, odak "Ayrıntılar" düğmesine taşır **bilgileri araç kutusu**. Daha fazla içinde gezinmek için **kutusunu** görüntülemek için kullanmak `Right` ve `Left arrow` ileriye ve geriye doğru sırasıyla taşımak için anahtarları. Tuşuna basarak `Enter` odaklanmış düğmesini seçerek aynı etkiye sahiptir **bilgileri araç kutusu**.
    - Bu tür bir düğüm, buna ait tüm bağlantıları ziyaret, tek tek, basarak seçimini `Shift+Left arrow` anahtarı. Odağı taşır **bilgileri araç kutusu** o bağlantı. Herhangi bir noktada odağı düğüme geri tuşlarına basarak gölgeye `Shift+Right arrow` yeniden.    

