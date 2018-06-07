---
title: Azure ve Azure yığını ile karma bulut bağlantı yapılandırma | Microsoft Docs
description: Azure ve Azure yığını ile karma bulut bağlantı yapılandırmayı öğrenin.
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
ms.date: 05/25/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 72c5c4b0f0ab752bb02e6bee7cd038afca44ee1b
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34605204"
---
# <a name="tutorial-configure-hybrid-cloud-connectivity-with-azure-and-azure-stack"></a>Öğretici: Azure ve Azure yığını ile karma bulut bağlantı yapılandırın

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Güvenlik genel Azure ve Azure karma bağlantı desenini kullanarak yığında kaynaklara erişebilir.

Bu öğreticide, bir örnek ortamı için yapı:

> [!div class="checklist"]
> - Veri gizliliği ya da yasal gereksinimleri karşılayacak, ancak genel Azure kaynaklarına erişmesini şirket içi tutun.
> - Eski bir sistemi genel Azure içinde bulut ölçeği uygulama dağıtımı ve kaynaklar kullanırken korur.

## <a name="prerequisites"></a>Önkoşullar

Bazı bileşenler bir karma bağlantı dağıtımı oluşturmak için gereklidir. Uygun şekilde planlamanız gerekir böylece bu bileşenlerin bazıları hazırlamak için uzun sürer.

**Azure Stack**

Bir Azure OEM/donanım iş ortağı üretim Azure yığın dağıtabilirsiniz ve tüm kullanıcıların bir Azure yığın Geliştirme Seti (ASDK) dağıtabilirsiniz.

**Azure yığın bileşenleri**

Bir Azure yığın işleç uygulama hizmeti dağıtmak, planları ve teklifleri oluşturmak, bir kiracı aboneliği oluşturmak ve Windows Server 2016 görüntü eklemeniz gerekir. Bu bileşenlerden bazıları zaten varsa, Bu öğretici başlamadan önce gereksinimleri karşıladığınızdan emin olun.

Bu öğretici, Azure ve Azure yığın bazı temel bilgiye sahip olduğunuzu varsayar. Öğretici başlamadan önce daha fazla bilgi için aşağıdaki makaleyi okuyun:

 - [Azure giriş](https://azure.microsoft.com/overview/what-is-azure/)
 - [Azure yığın temel kavramları](https://docs.microsoft.com/azure/azure-stack/azure-stack-key-features)

### <a name="azure"></a>Azure

 - Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
 - Oluşturma bir [Web uygulaması](https://docs.microsoft.com/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?view=vsts&tabs=vsts#create-an-azure-web-app-using-the-portal) azure'da. Öğreticide gerekir çünkü Web uygulama URL'sini not edin.

### <a name="azure-stack"></a>Azure Stack

 - Üretim Azure yığın kullanın veya Azure yığın geliştirme Seti'nden dağıtmak https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1.
   >[!Note]
   >ASDK dağıtma 7 saati, böylece uygun şekilde planlayın.

 - Dağıtma [uygulama hizmeti](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy) Azure yığınına PaaS Hizmetleri.
 - [Planları oluşturup teklifleri](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) Azure yığın ortamında.
 - [Kiracı aboneliği oluşturmak](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) Azure yığın ortamında.

### <a name="before-you-begin"></a>Başlamadan önce

Karma bulut bağlantı yapılandırmaya başlamadan önce aşağıdaki ölçütlere uyan doğrulayın:

 - VPN cihazınız için dışarıya dönük bir genel IPv4 adresi gerekir. Bu IP adresi bir NAT'nin arkasında olamaz
 - Tüm kaynakların aynı bölge/konumuna dağıtılır.

#### <a name="tutorial-example-values"></a>Eğitmen örnek değerler

Bu öğreticide örnekler aşağıdaki değerleri kullanın. Bir test ortamı oluşturmak veya bunlara daha iyi örnekleri anlamak için başvurmak için bu değerleri kullanabilirsiniz. Genel olarak VPN Gateway ayarları hakkında daha fazla bilgi için [VPN Gateway Ayarları Hakkında](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings) konusunu inceleyin.

Bağlantı özellikleri:

 - **VPN türü**: rota tabanlı
 - **Bağlantı türü**: siteden siteye (IPSec)
 - **Ağ geçidi türü**: VPN
 - **Azure bağlantı adı**: Azure-Gateway-AzureStack-(portal olacak otomatik doldurma bu değer) S2SGateway
 - **Azure yığın bağlantı adı**: AzureStack-Gateway-Azure-(portal olacak otomatik doldurma bu değer) S2SGateway
 - **Paylaşılan anahtar**: VPN donanım, bağlantının iki tarafındaki değerleri eşleşen herhangi uyumlu
 - **Abonelik**: Abonelik herhangi bir tercih edilen
 - **Kaynak grubu**: Test altyapısı

Ağ ve alt ağ IP adresleri:

| Azure/Azure yığın bağlantı | Ad | Alt ağ | IP Adresi |
|-------------------------------------|---------------------------------------------|---------------------------------------|-----------------------------|
| Azure sanal ağı | ApplicationvNet<br>10.100.102.9/23 | ApplicationSubnet<br>10.100.102.0/24 |  |
|  |  | GatewaySubnet<br>10.100.103.0/24 |  |
| Azure yığın vNet | ApplicationvNet<br>10.100.100.0/23 | ApplicationSubnet <br>10.100.100.0/24 |  |
|  |  | GatewaySubnet <br>10.100101.0/24 |  |
| Azure sanal ağ geçidi | Azure ağ geçidi |  |  |
| Azure yığın sanal ağ geçidi | AzureStack ağ geçidi |  |  |
| Azure genel IP | Azure GatewayPublicIP |  | Oluşturma sırasında belirlenir |
| Azure yığın genel IP | AzureStack GatewayPublicIP |  | Oluşturma sırasında belirlenir |
| Azure yerel ağ geçidi | AzureStack S2SGateway<br>   10.100.100.0/23 |  | Azure yığın genel IP değeri |
| Azure yığın yerel ağ geçidi | Azure S2SGateway<br>10.100.102.0/23 |  | Azure genel IP değeri |

## <a name="create-a-virtual-network-in-global-azure-and-azure-stack"></a>Genel Azure ve Azure yığın bir sanal ağ oluşturma

Portalı kullanarak bir sanal ağ oluşturmak için aşağıdaki adımları kullanın. Bunlar kullanabilirsiniz [örnek değerler](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#values) bu makalede bir öğretici kullanıyorsanız. Ancak, bu makalede, bir üretim ortamını yapılandırmak için kullanıyorsanız, örnek ayarlar kendi değerlerinizle değiştirin.

> [!IMPORTANT]
> Azure veya Azure yığını vNet adres alanları IP adreslerinin bir çakışma olmadığından emin olmanız gerekir.

Azure'da vNet oluşturmak için:

1. Tarayıcınız bağlanmak için kullandığı [Azure portal](http://portal.azure.com/) ve Azure hesabınızla oturum açın.
2. Seçin **kaynak oluşturma**. İçinde **Market arama** alanına, `virtual network`''. Bul **sanal ağ** sonuçları listesi ve ardından **sanal ağ**.
3. Gelen **dağıtım modeli seçin** listesinde, seçin **Resource Manager**ve ardından **oluşturma**.
4. Üzerinde **sanal ağ oluştur**, VNet ayarlarını yapılandırın. Gerekli alan adları kırmızı yıldız işaretiyle öneki alır.  Geçerli bir değer girdiğinizde, yıldız işareti yeşil bir onay işareti değiştirir.

Azure yığınında bir vNet oluşturmak için:

* (1-4) kullanılarak Azure yığın önceki adımları tekrarlayın **Kiracı portalı**.

## <a name="add-a-gateway-subnet"></a>Ağ geçidi alt ağı ekleme

Sanal ağınız için bir ağ geçidi bağlanmadan önce bağlanmak istediğiniz sanal ağ için ağ geçidi alt ağı oluşturmanız gerekir. Ağ Geçidi Hizmetleri ağ geçidi alt ağı içinde belirttiğiniz IP adreslerini kullanır.

İçinde [Azure portal](http://portal.azure.com/), bir sanal ağ geçidi oluşturmak istediğiniz kaynak yöneticisi sanal ağa gidin.

1. Açmak için sanal ağ seçin **sanal ağ** sayfası.
2. İçinde **ayarları**seçin **alt ağlar**.
3. Üzerinde **alt ağlar** sayfasında, **+ ağ geçidi alt ağı** açmak için **alt ağ Ekle** sayfası.

    ![Ağ geçidi alt ağı Ekle](media/azure-stack-solution-hybrid-connectivity/image4.png)

4. **Adı** için alt ağ 'GatewaySubnet' değeri otomatik olarak girilir. Bu değer, alt ağ geçidi alt ağı tanımak Azure için gereklidir.
5. Değişiklik **adres aralığı** yapılandırma gereksinimlerinize uyan ve daha sonra seçmek için sağlanan değerler **Tamam**.

## <a name="create-a-virtual-network-gateway-in-azure-and-azure-stack"></a>Azure ve Azure sanal ağ geçidi oluşturma yığını

Azure'da bir sanal ağ geçidi oluşturmak için aşağıdaki adımları kullanın.

1. Portal sayfasının sol taraftan **+** ve 'sanal ağ geçidi' arama alanına girin.
2. İçinde **sonuçları**seçin **sanal ağ geçidi**.
3. İçinde **sanal ağ geçidi**seçin **oluşturma** açmak için **sanal ağ geçidi Oluştur** sayfası.
4. Üzerinde **sanal ağ geçidi Oluştur**, gösterildiği gibi ağ geçidi için değerleri belirtin **öğretici örnek değerler**ve aşağıdaki ek değerler:

    - **SKU**: temel
    - **Sanal ağ**: daha önce oluşturduğunuz sanal ağı seçin. Oluşturduğunuz ağ geçidi alt ağı otomatik olarak seçilir.
    - **İlk IP yapılandırması**: ağ geçidinizin genel IP budur.
        - Seçin **oluşturma ağ geçidi IP yapılandırması**, size aldığı **genel IP adresi seçin** sayfası.
        - Seçin **+ Yeni Oluştur** açmak için **ortak IP adresi oluştur** sayfası.
        - Girin bir **adı** ortak IP adresi için. SKU olarak bırakın **temel**ve ardından **Tamam** yaptığınız değişiklikleri kaydetmek için.

       > [!Note]
       > Şu anda, VPN ağ geçidi, yalnızca dinamik genel IP adresi ayırma destekler. Ancak, bu VPN ağ geçidinizi için atandıktan sonra IP adresini değiştirse anlamına gelmez. Genel IP adresi, yalnızca ağ geçidi silinip yeniden oluşturulduğunda değişir. Yeniden boyutlandırma, sıfırlama veya diğer iç bakım/yükseltme VPN ağ geçidinizin IP adresini değiştirme.

4. Ağ geçidi ayarlarınızı doğrulayın.
5. Seçin **oluşturma** VPN ağ geçidi oluşturmak için. Ağ Geçidi ayarlarını doğrulanır ve "Dağıtma sanal ağ geçidi" döşeme Panonuzda gösterilir.

   >[!Note]
   >Bir ağ geçidi oluşturma 45 dakika kadar sürebilir. Tamamlanma durumunu görmek için portal sayfanızı yenilemeniz gerekebilir.

    Ağ geçidi oluşturulduktan sonra sanal ağ portalında bakarak atanmış IP adresi görebilirsiniz. Ağ geçidi bağlı bir cihaz gibi görüntülenir. Ağ geçidi hakkında daha fazla bilgi için cihazı seçin.

6. Azure yığın dağıtımınızdaki (1-5) önceki adımları yineleyin.

## <a name="create-the-local-network-gateway-in-azure-and-azure-stack"></a>Azure ve Azure yığın yerel ağ geçidi oluşturma

Yerel ağ geçidi genellikle şirket içi konumunuz anlamına gelir. Site Azure veya Azure yığını bakın ve ardından belirtin bir ad verin:

- Bir bağlantı için oluşturmakta olduğunuz şirket içi VPN cihazı IP adresi.
- VPN cihaz için VPN ağ geçidi üzerinden yönlendirilir IP adresi öneklerini. Belirttiğiniz adres ön ekleri, şirket içi adresinizde yer alan ön eklerdir.

  >[!Note]
  >Şirket içi ağ değişikliklerinizi veya VPN cihazının genel IP adresini değiştirmeniz gerekirse, bu değerleri daha sonra kolayca güncelleştirebilirsiniz.

1. Portalı'nda seçin **+ kaynak oluşturma**.
2. Arama kutusuna **yerel ağ geçidi**seçeneğini belirleyip **Enter** aramak için. Böylece sonuçların bir listesi döndürülür.
3. Seçin **yerel ağ geçidi**seçeneğini belirleyip **oluşturma** açmak için **oluşturma yerel ağ geçidi** sayfası.
4. Üzerinde **oluşturma yerel ağ geçidi**, yerel ağ geçidiniz için değerleri belirtin kullanarak bizim **öğretici örnek değerler**. Aşağıdaki ek değerler içerir.

    - **IP adresi**: Azure veya Azure yığın bağlanmak istediğiniz VPN cihazının genel IP adresi budur. Azure adresine ulaşabilmeniz için bir NAT'nin arkasında değildir geçerli bir ortak IP adresi belirtin. IP adresi şu anda sahip değilseniz, örnek arasında bir değer yer tutucu olarak kullanabilirsiniz, ancak geri gidin ve yer tutucu, VPN cihazınızın genel IP adresi ile değiştirmek zorunda kalırsınız. Geçerli bir adresi sağlanıncaya kadar azure cihaza bağlanamıyor.
    - **Adres alanı**: Bu yerel ağ temsil eden ağ adres aralığı budur. Birden fazla adres alanı aralığı ekleyebilirsiniz. Belirttiğiniz aralıklar aralıkları, bağlanmak istediğiniz diğer ağlar ile çakışmadığından emin olun. Azure, belirttiğiniz adres aralığını şirket içi VPN cihazının IP adresine yönlendirir. Örnek değer, şirket içi siteye bağlanmak isterseniz kendi değerlerini kullanın.
    - **BGP ayarlarını yapılandırmak**: BGP yapılandırırken kullanabilirsiniz. Aksi takdirde, bu seçeneği işaretlemeyin.
    - **Abonelik**: doğru aboneliğin gösterildiğini doğrulayın.
    - **Kaynak grubu**: kullanmak istediğiniz kaynak grubunu seçin. Yeni bir kaynak grubu oluşturabilir veya önceden oluşturduğunuz birini seçin.
    - **Konum**: Bu nesnenin içinde oluşturulacak konumu seçin. Sanal ağınızı bulunan aynı konuma seçmek isteyebilirsiniz, ancak bunu gerçekleştirmek için gereken.
5. Gereken değerleri belirtme bitirdiğinizde seçin **oluşturma** yerel ağ geçidi oluşturmak için.
6. (1-5)'Azure yığın dağıtımınızdaki'e kadar bu adımları yineleyin.

## <a name="configure-your-connection"></a>Bağlantınızı yapılandırın

Bir şirket içi ağı ile Siteden Siteye bağlantılar için VPN cihazı gerekir. Yapılandırdığınız VPN cihazı bağlantı olarak adlandırılır. Bağlantınızı yapılandırmak için gerekir:

- Paylaşılan bir anahtar. Siteden Siteye VPN bağlantınızı oluştururken belirttiğiniz paylaşılan anahtarın aynısıdır. Bu örneklerde temel bir paylaşılan anahtar kullanılır. Kullanmak için daha karmaşık bir anahtar oluşturmanız önerilir.
- Sanal ağ geçidinizin Genel IP adresi. Azure Portal, PowerShell veya CLI kullanarak genel IP adresini görüntüleyebilirsiniz. Azure Portalı'nı kullanarak VPN ağ geçidinizin genel IP adresini bulmak için sanal ağ geçitleri için gidin, sonra ağ geçidi adını seçin.

Sanal ağ geçidiniz ve şirket içi VPN cihazınız arasındaki siteden siteye VPN bağlantısı oluşturmak için aşağıdaki adımları kullanın.

1. Azure portalında seçin **+ kaynak oluşturma**.
2. Arama **bağlantıları**.
3. İçinde **sonuçları**seçin **bağlantıları**.
4. Üzerinde **bağlantı**seçin **oluşturma**.
5. Üzerinde **oluşturma bağlantı**, aşağıdaki ayarları yapılandırın:

    - **Bağlantı türü**: siteden siteye (IPSec) seçin.
    - **Kaynak grubu**: test kaynak grubunu seçin.
    - **Sanal ağ geçidi**: oluşturduğunuz sanal ağ geçidi seçin.
    - **Yerel ağ geçidi**: oluşturduğunuz yerel ağ geçidi seçin.
    - **Bağlantı adı**: Bu iki ağ geçidi değerleri kullanarak otomatik olarak doldurulur.
    - **Paylaşılan anahtar**: Bu değer, yerel şirket içi VPN Aygıtınızın kullanmakta olduğunuz değerle eşleşmelidir. Eğitmen örnek 'abc123' kullanır, ancak daha karmaşık bir şey olabilir ve kullanmanız gerekir. Bu değer, VPN Cihazınızı yapılandırırken belirttiğiniz değerle aynı değere olmalıdır önemli şeydir.
    - Değerleri **abonelik**, **kaynak grubu**, ve **konumu** düzeltilmiştir.

6. Seçin **Tamam** bağlantınızı oluşturmak için.

Bağlantı görebilirsiniz **bağlantıları** sanal ağ geçidinin sayfası. Durum çalışmaya başlayacaktır *bilinmeyen* için *bağlanıyor*ve ardından *başarılı*.

## <a name="next-steps"></a>Sonraki adımlar

- Azure bulut desenleri hakkında daha fazla bilgi için bkz: [bulut tasarım modeli](https://docs.microsoft.com/azure/architecture/patterns).
