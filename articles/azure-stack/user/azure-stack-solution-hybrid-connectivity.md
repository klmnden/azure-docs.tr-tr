---
title: Azure Stack ve Azure ile karma bulut bağlantısı yapılandırma | Microsoft Docs
description: Azure Stack ve Azure ile karma bulut bağlantısı yapılandırmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/14/2019
ms.author: mabrigg
ms.reviewer: anajod
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: a05021255c6226329f1d7a3f0e7fa8c9be756646
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57546730"
---
# <a name="tutorial-configure-hybrid-cloud-connectivity-with-azure-and-azure-stack"></a>Öğretici: Azure ve Azure Stack ile karma bulut bağlantısı yapılandırma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure genel ve karma bağlantı desenini kullanarak, Azure Stack güvenlik ile kaynaklarına erişim sağlayabilir.

Bu öğreticide, bir örnek ortama oluşturacaksınız:

> [!div class="checklist"]
> - Veri gizlilik veya yasal gereksinimleri karşılamak, ancak genel Azure kaynaklarına erişim için şirket içinde tutun.
> - Eski sistem genel Azure'da bulut ölçeği uygulama dağıtımı ve kaynakları kullanırken korur.

> [!Tip]  
> ![karma pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack, Azure'nın bir uzantısıdır. Azure Stack çevikliğini ve yenilik bulut bilgi işlem, şirket içi ortamınıza ve hibrit uygulamaları her yerde oluşturup dağıtmayı olanak tanıyan tek hibrit Bulutu sunar.  
> 
> Teknik incelemeyi [karma uygulamaları için tasarım konuları](https://aka.ms/hybrid-cloud-applications-pillars) tasarlama, dağıtma ve karma işletim (yerleştirme, ölçeklenebilirlik, kullanılabilirlik, dayanıklılık, yönetilebilirlik ve güvenlik) yazılım kalitesinin yapı taşları gözden geçirmeleri uygulamalar. Tasarım konuları, üretim ortamlarında sorunlarını en aza karma uygulama tasarımının en iyi duruma getirme yardımcı olur.


## <a name="prerequisites"></a>Önkoşullar

Bazı bileşenler bir karma bağlantı dağıtımı oluşturmak için gereklidir. Uygun şekilde planlamak gerekir, bu bileşenlerden bazıları hazırlamak için zaman alabilir.

**Azure Stack**

Bir Azure OEM/donanım iş ortağı bir üretim Azure Stack'te dağıtabilir ve tüm kullanıcıların bir Azure Stack geliştirme Seti'ni (ASDK) dağıtabilirsiniz.

**Azure Stack bileşenleri**

Bir Azure Stack operatörü App Service'e dağıtım, planlar ve Teklifler oluşturma, bir kiracı aboneliği oluşturmak ve Windows Server 2016 görüntüsü eklemeniz gerekir. Bu bileşenlerden bazıları zaten varsa, bu öğreticiye başlamadan önce gereksinimleri karşıladığından emin olun.

Bu öğreticide, Azure ve Azure Stack bazı temel bilgi sahibi olduğunuzu varsayar. Bu öğreticiye başlamadan önce daha fazla bilgi için bu makaleleri okuyun:

 - [Azure'a giriş](https://azure.microsoft.com/overview/what-is-azure/)
 - [Azure Stack temel kavramları](https://docs.microsoft.com/azure/azure-stack/azure-stack-key-features)

### <a name="azure"></a>Azure

 - Azure aboneliğiniz yoksa başlamadan önce  [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)  oluşturun.
 - Oluşturma bir [Web uygulaması](https://docs.microsoft.com/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?view=vsts&tabs=vsts) azure'da. Öğreticide gerekeceği için Web App URL'si not edin.

### <a name="azure-stack"></a>Azure Stack

 - Azure Stack üretim kullanın veya Azure Stack geliştirme Seti'nden dağıtma https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1.
   >[!Note]
   >ASDK dağıtma 7 saat kadar sürebilir, bu nedenle buna göre planlayın.

 - Dağıtma [App Service](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy) PaaS Hizmetleri Azure stack'e.
 - [Planlar ve Teklifler oluşturma](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) Azure Stack ortamında.
 - [Kiracı aboneliği oluşturmak](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) Azure Stack ortamında.

### <a name="before-you-begin"></a>Başlamadan önce

Hibrit bulut bağlantısı yapılandırma başlamadan önce aşağıdaki ölçütleri karşıladığınızı doğrulayın:

 - VPN cihazınız için dışarıya yönelik genel bir IPv4 adresi gerekir. Bu IP adresi bir NAT'nin arkasında olamaz
 - Tüm kaynaklar aynı bölge/konum dağıtılır.

#### <a name="tutorial-example-values"></a>Öğretici örneğinde değerleri

Bu öğreticideki örneklerde aşağıdaki değerler kullanılır. Bir test ortamı oluşturabilir veya bir daha iyi örneklerini anlamak için bunlara başvurmak için bu değerleri kullanabilirsiniz. VPN Gateway hakkında daha fazla bilgi için genel olarak, belirleyebilmesini [VPN Gateway ayarları hakkında](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings).

Bağlantı özellikleri:

 - **VPN türü**: Rota tabanlı
 - **Bağlantı türü**: Siteden siteye (IPSec)
 - **Ağ geçidi türü**: VPN
 - **Azure bağlantı adı**: Azure-Gateway-AzureStack-(portalda otomatik doldurulacaktır bu değeri) S2SGateway
 - **Azure Stack bağlantı adı**: AzureStack-Gateway-Azure-(portalda otomatik doldurulacaktır bu değeri) S2SGateway
 - **Paylaşılan anahtar**: İle eşleşen değerler bağlantının her iki tarafındaki VPN donanımına, tüm uyumlu
 - **Abonelik**: Tercih edilen herhangi bir abonelik
 - **Kaynak grubu**: Test-Infra

Ağ ve alt ağ IP adresleri:

| Azure/Azure Stack bağlantı | Ad | Alt ağ | IP Adresi |
|-------------------------------------|---------------------------------------------|---------------------------------------|-----------------------------|
| Azure vNet | ApplicationvNet<br>10.100.102.9/23 | ApplicationSubnet<br>10.100.102.0/24 |  |
|  |  | GatewaySubnet<br>10.100.103.0/24 |  |
| Azure Stack vNet | ApplicationvNet<br>10.100.100.0/23 | ApplicationSubnet <br>10.100.100.0/24 |  |
|  |  | GatewaySubnet <br>10.100101.0/24 |  |
| Azure sanal ağ geçidi | Azure ağ geçidi |  |  |
| Azure Stack sanal ağ geçidi | AzureStack ağ geçidi |  |  |
| Azure genel IP | Azure-GatewayPublicIP |  | Oluşturma sırasında belirlenir. |
| Azure Stack genel IP | AzureStack GatewayPublicIP |  | Oluşturma sırasında belirlenir. |
| Azure yerel ağ geçidi | AzureStack-S2SGateway<br>   10.100.100.0/23 |  | Azure Stack genel IP değeri |
| Azure Stack yerel ağ geçidi | Azure-S2SGateway<br>10.100.102.0/23 |  | Azure genel IP değeri |

## <a name="create-a-virtual-network-in-global-azure-and-azure-stack"></a>Global Azure ve Azure Stack sanal ağ oluşturma

Portalı kullanarak bir sanal ağ oluşturmak için aşağıdaki adımları kullanın. Bunları [örnek değerleri](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#values) öğretici olarak bu makalede kullanıyorsanız. Ancak, bir üretim ortamını yapılandırmak için bu makalede kullanıyorsanız, örnek ayarları kendi değerlerinizle değiştirin.

> [!IMPORTANT]
> Azure'da veya Azure Stack'te sanal ağ adres alanları IP adreslerinden oluşan bir çakışma olmadığından emin olmanız gerekir.

Azure'da vNet oluşturmak için:

1. Bağlanmak için tarayıcınızı kullanırsınız [Azure portalında](https://portal.azure.com/) ve Azure hesabınızla oturum açın.
2. Seçin **kaynak Oluştur**. İçinde **markette Ara** alanına `virtual network`''. Bulma **sanal ağ** sonuç listesini ve ardından **sanal ağ**.
3. Gelen **dağıtım modeli seçin** listesinde, seçin **Resource Manager**ve ardından **Oluştur**.
4. Üzerinde **sanal ağ oluştur**, sanal ağ ayarlarını yapılandırın. Gerekli alan adları, kırmızı bir yıldızla öneki alır.  Geçerli bir değer girin, yıldız işareti yeşil onay işareti değiştirir.

Azure Stack'te bir vNet oluşturmak için:

* (1-4) Azure Stack kullanarak önceki adımları yineleyin **Kiracı portalı**.

## <a name="add-a-gateway-subnet"></a>Ağ geçidi alt ağı ekleme

Sanal ağınız için ağ geçidi bağlanmadan önce bağlanmak istediğiniz sanal ağın ağ geçidi alt ağı oluşturmanız gerekir. Ağ geçidi hizmetlerine ağ geçidi alt ağında belirttiğiniz IP adreslerini kullanır.

İçinde [Azure portalında](https://portal.azure.com/), bir sanal ağ geçidini oluşturmak istediğiniz Resource Manager sanal ağına gidin.

1. Açmak için bir vNet seçin **sanal ağ** sayfası.
2. İçinde **ayarları**seçin **alt ağlar**.
3. Üzerinde **alt ağlar** sayfasında **+ ağ geçidi alt ağı** açmak için **alt ağ Ekle** sayfası.

    ![Ağ geçidi alt ağı ekleme](media/azure-stack-solution-hybrid-connectivity/image4.png)

4.  **Adı** alt otomatik olarak 'GatewaySubnet' değeriyle doldurulur. Azure alt ağın ağ geçidi alt ağı tanınması bu değer gereklidir.
5. Değişiklik **adres aralığı** yapılandırma gereksinimlerinize uyacak ve ardından sağlanan değerler **Tamam**.

## <a name="create-a-virtual-network-gateway-in-azure-and-azure-stack"></a>Azure ve Azure sanal ağ geçidi oluşturma yığını

Azure'da bir sanal ağ geçidi oluşturmak için aşağıdaki adımları kullanın.

1. Portal sayfasının sol taraftan **+**  ve arama alanına 'sanal ağ geçidi' girin.
2. İçinde **sonuçları**seçin **sanal ağ geçidi**.
3. İçinde **sanal ağ geçidi**seçin **Oluştur** açmak için **sanal ağ geçidi Oluştur** sayfası.
4. Üzerinde **sanal ağ geçidi Oluştur**, gösterildiği gibi ağ geçidiniz için değerlerleri belirtin **öğretici örnek değerleri**, aşağıdaki ek değerler:

    - **SKU**: temel
    - **Sanal ağ**: Daha önce oluşturduğunuz sanal ağı seçin. Oluşturduğunuz ağ geçidi alt ağı otomatik olarak seçilir.
    - **İlk IP yapılandırması**:  Ağ geçidinizin genel IP budur.
        - Seçin **ağ geçidi IP yapılandırması oluştur**, size aldığı **genel IP adresi seçin** sayfası.
        - Seçin **+ Yeni Oluştur** açmak için **genel IP adresi oluşturma** sayfası.
        - Girin bir **adı** genel IP adresi için. SKU olarak bırakın **temel**ve ardından **Tamam** yaptığınız değişiklikleri kaydedin.

       > [!Note]
       > Şu anda, VPN ağ geçidi, yalnızca dinamik genel IP adresi ayırmayı desteklemektedir. Ancak, bu IP adresinin VPN ağ geçidinize atandıktan sonra değiştiği anlamına gelmez. Genel IP adresi, yalnızca ağ geçidi silinip yeniden oluşturulduğunda değişir. Yeniden boyutlandırma, sıfırlama veya diğer iç bakım/yükseltme işlemleri VPN ağ geçidiniz için IP adresi değişmez.

4. Ağ geçidi ayarlarınızı doğrulayın.
5. Seçin **Oluştur** VPN ağ geçidi oluşturmak için. Ağ geçidi ayarlar doğrulanır ve "Sanal ağ geçidi dağıtma" kutucuğunu panonuza gösterilir.

   >[!Note]
   >Bir ağ geçidi oluşturma 45 dakika kadar sürebilir. Tamamlanma durumunu görmek için portal sayfanızı yenilemeniz gerekebilir.

    Ağ geçidi oluşturulduktan sonra portalda sanal ağa bakarak atanmış IP adresini görebilirsiniz. Ağ geçidi bağlı bir cihaz gibi görüntülenir. Ağ geçidi hakkında daha fazla bilgi için cihazı seçin.

6. Azure Stack dağıtımınıza (1-5) önceki adımları yineleyin.

## <a name="create-the-local-network-gateway-in-azure-and-azure-stack"></a>Azure ve Azure Stack yerel ağ geçidi oluşturma

Yerel ağ geçidi genellikle şirket içi konumunuz anlamına gelir. Site, Azure'da veya Azure Stack'te bakın ve ardından belirleyin adı verin:

- İçin bir bağlantı oluşturacağınız şirket içi VPN cihazının IP adresidir.
- VPN ağ geçidi VPN cihazına yönlendirilecek IP adresi ön eklerini. Belirttiğiniz adres ön ekleri, şirket içi adresinizde yer alan ön eklerdir.

  >[!Note]
  >Şirket içi ağınız değişirse veya VPN cihazının genel IP adresini değiştirmeniz gerekirse, bu değerler daha sonra kolayca güncelleştirebilirsiniz.

1. Portalında **+ kaynak Oluştur**.
2. Arama kutusuna **yerel ağ geçidi**, ardından **Enter** aranacak. Böylece sonuçların bir listesi döndürülür.
3. Seçin **yerel ağ geçidi**, ardından **Oluştur** açmak için **yerel ağ geçidi Oluştur** sayfası.
4. Üzerinde **yerel ağ geçidi Oluştur**, yerel ağ geçidiniz için değerlerleri belirtin kullanarak bizim **öğretici örnek değerleri**. Aşağıdaki ek değerler içerir.

    - **IP adresi**: Bu, Azure'da veya Azure Stack'te bağlanmak istediğiniz VPN cihazının genel IP adresidir. Azure adresine ulaşabilmeniz için bir NAT'nin arkasında bulunmayan geçerli bir genel IP adresi belirtin. IP adresi şu anda sahip değilseniz, örnek bir değerden bir yer tutucu olarak kullanabilirsiniz, ancak geri dönüp yer tutucu VPN cihazınızın genel IP adresiyle değiştirmeniz gerekir. Azure, geçerli bir adres sağlamak kadar cihaza bağlanamıyor.
    - **Adres alanı**: Bu yerel ağın temsil eden ağ adres aralığı budur. Birden fazla adres alanı aralığı ekleyebilirsiniz. Belirttiğiniz aralıkların, bağlanmak istediğiniz diğer ağların aralıklarıyla çakışmadığından emin olun. Azure, belirttiğiniz adres aralığını şirket içi VPN cihazının IP adresine yönlendirir. Örnek değer, şirket içi sitenize bağlanmak istiyorsanız kendi değerlerinizi kullanın.
    - **BGP ayarları Yapılandır**: Yalnızca BGP'yi yapılandırırken kullanın. Aksi takdirde, bu seçeneği işaretlemeyin.
    - **Abonelik**: Doğru aboneliğin gösterildiğini doğrulayın.
    - **Kaynak grubu**: Kullanmak istediğiniz kaynak grubunu seçin. Yeni bir kaynak grubu oluşturabilir veya önceden oluşturduğunuz bir tane seçin.
    - **Konum**: Bu nesnenin oluşturulacağı konumu seçin. Ağınızın bulunduğu konumu seçebilirsiniz isteyebilirsiniz, ancak bunu gerçekleştirmek için gereken.
5. Gerekli değerler belirten bitirdikten sonra seçin **Oluştur** yerel ağ geçidi oluşturmak için.
6. Bu adımları (1-5) Azure Stack dağıtımınıza tekrarlayın.

## <a name="configure-your-connection"></a>Bağlantınızı yapılandırın

Bir şirket içi ağı ile Siteden Siteye bağlantılar için VPN cihazı gerekir. VPN cihazı yapılandırdığınız bağlantı olarak adlandırılır. Bağlantınızı yapılandırmak için gerekir:

- Paylaşılan bir anahtar. Siteden Siteye VPN bağlantınızı oluştururken belirttiğiniz paylaşılan anahtarın aynısıdır. Bu örneklerde temel bir paylaşılan anahtar kullanılır. Kullanmak için daha karmaşık bir anahtar oluşturmanız önerilir.
- Sanal ağ geçidinizin Genel IP adresi. Azure Portal, PowerShell veya CLI kullanarak genel IP adresini görüntüleyebilirsiniz. Azure portalını kullanarak VPN ağ geçidi genel IP adresini bulmak için sanal ağ geçitleri için gidin, sonra ağ geçidi adını seçin.

Sanal ağ geçidiniz ile şirket içi VPN cihazınız arasında siteden siteye VPN bağlantısı oluşturmak için aşağıdaki adımları kullanın.

1. Azure portalında **+ kaynak Oluştur**.
2. Arama **bağlantıları**.
3. İçinde **sonuçları**seçin **bağlantıları**.
4. Üzerinde **bağlantı**seçin **Oluştur**.
5. Üzerinde **bağlantı oluştur**, aşağıdaki ayarları yapılandırın:

    - **Bağlantı türü**: Siteden siteye (IPSec) seçin.
    - **Kaynak grubu**: Test kaynak grubunuzu seçin.
    - **Sanal ağ geçidi**: Oluşturduğunuz sanal ağ geçidi seçin.
    - **Yerel ağ geçidi**: Oluşturduğunuz yerel ağ geçidi seçin.
    - **Bağlantı adı**: Bu değerleri iki ağ geçidi kullanarak otomatik olarak doldurulur.
    - **Paylaşılan anahtar**: Bu değer, yerel şirket içi VPN Cihazınızda kullanmakta olduğunuz değerle eşleşmelidir. Öğretici örnekte 'abc123', ancak siz daha karmaşık kullanabilirsiniz ve kullanmalısınız. Önemli olan, bu değer VPN Cihazınızı yapılandırırken belirttiğiniz değerle aynı olmalıdır olmasıdır.
    - Değerleri **abonelik**, **kaynak grubu**, ve **konumu** sabittir.

6. Seçin **Tamam** bağlantınızı oluşturmak için.

Bağlantıda gördüğünüz **bağlantıları** sayfasında sanal ağ geçidi. Durum çalışmaya başlayacaktır *bilinmeyen* için *bağlanma*ve ardından *başarılı*.

## <a name="next-steps"></a>Sonraki adımlar

- Azure bulut desenleri hakkında daha fazla bilgi için bkz: [bulut tasarımı desenleri](https://docs.microsoft.com/azure/architecture/patterns).
