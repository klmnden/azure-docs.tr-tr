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
ms.date: 05/07/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 048e2636aabe406728c8fe1b93ef861f13346256
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="tutorial-configure-hybrid-cloud-connectivity-with-azure-and-azure-stack"></a>Öğretici: Azure ve Azure yığını ile karma bulut bağlantı yapılandırın

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Güvenlik genel Azure ve Azure karma bağlantı desenini kullanarak yığında kaynaklara erişebilir. 

Bu öğreticide, bir örnek ortamı için yapı:

> [!div class="checklist"]
> - Verileri şirket içi gizlilik veya yasal gereksinimler için tutmak, ancak genel Azure kaynaklarına erişimi vardır.
> - Eski bir sistemi genel Azure içinde bulut ölçeği uygulama dağıtımı ve kaynaklar kullanırken korur.

## <a name="prerequisites"></a>Önkoşullar

Bazı bileşenler bir karma bağlantı dağıtımı oluşturmak için gerekli ve hazırlamak için biraz zaman alabilir. 

Bir Azure OEM/donanım iş ortağı üretim Azure yığın dağıtabileceğiniz ve tüm kullanıcıların bir ASDK dağıtabilirsiniz. Bir Azure yığın işleç Ayrıca uygulama hizmeti dağıtmak, planları ve teklifleri oluşturmak, bir kiracı aboneliği oluşturmak ve Windows Server 2016 görüntü eklemeniz gerekir. 

Bu bileşenlerden bazıları zaten varsa başlamadan önce gereksinimleri karşıladığınızdan emin olun.

Bu konu ayrıca bazı Azure ve Azure yığın bilgisine sahip olduğunuzu varsayar. Devam etmeden önce daha fazla bilgi edinmek istiyorsanız, bu konularda başlatmak emin olun:
 - [Azure giriş](https://azure.microsoft.com/overview/what-is-azure/)
 - [Azure yığın temel kavramları](https://docs.microsoft.com/azure/azure-stack/azure-stack-key-features)

### <a name="azure"></a>Azure
 - Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
 - Oluşturma bir [Web uygulaması](https://docs.microsoft.com/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?view=vsts&tabs=vsts#create-an-azure-web-app-using-the-portal) azure'da. Daha sonra kullanılmak üzere yeni Web uygulaması URL'sini not edin.

### <a name="azure-stack"></a>Azure Stack
 - Üretim Azure yığın veya Azure yığın geliştirme aşağıda bağlı Seti dağıtmak kullanın:
    - https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1 
    - Yükleme genellikle tamamlamak için bu nedenle uygun şekilde planlamanız birkaç saat sürer.
 - Dağıtma [uygulama hizmeti](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy) Azure yığınına PaaS Hizmetleri. 
 - [Plan/teklifleri oluşturmak](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) Azure yığın ortamında. 
 - [Kiracı aboneliği oluşturmak](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) Azure yığın ortamında. 


### <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmanıza başlamadan önce aşağıdaki ölçütleri karşıladığınızı doğrulayın:

 - VPN cihazınız için dışarıya dönük genel bir IPv4 adresi olduğunu doğrulayın. Bu IP adresi bir NAT’nin arkasında olamaz.
 - Tüm kaynakların aynı bölge/konum dağıtılmış olduğundan emin olun.

#### <a name="example-values"></a>Örnek değerler

Bu makaledeki örneklerde aşağıdaki değerler kullanılır. Bir test ortamı oluşturmak veya bunları bu makaledeki örneklerde daha iyi anlamak için başvurmak için bu değerleri kullanabilirsiniz. Genel olarak VPN Gateway ayarları hakkında daha fazla bilgi için [VPN Gateway Ayarları Hakkında](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings) konusunu inceleyin.

Bağlantı özellikleri:
 - **VPN türü**: rota tabanlı
 - **Bağlantı türü**: siteden siteye (IPSec)
 - **Ağ geçidi türü**: VPN
 - **Azure bağlantı adı**: Azure-Gateway-AzureStack-(portal olacak otomatik doldurma bu değer) S2SGateway
 - **Azure yığın bağlantı adı**: AzureStack-Gateway-Azure-(portal olacak otomatik doldurma bu değer) S2SGateway
 - **Paylaşılan anahtar**: VPN donanım, bağlantının iki tarafındaki değerleri eşleşen herhangi uyumlu
 - **Abonelik**: Abonelik herhangi bir tercih edilen
 - **Kaynak grubu**: Test altyapısı

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

> [!Note]  
> Azure veya Azure yığını vNet adres alanları IP'leri örtüşmez olduğundan emin olmalısınız. 

Azure Portalı'nı kullanarak Resource Manager dağıtım modelinde bir vNet oluşturmak için. Bu adımları öğretici olarak uyguluyorsanız [örnek değerleri](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#values) kullanın. Bu adımları öğretici olarak uygulamıyorsanız değerleri kendi değerlerinizle değiştirmeyi unutmayın. 

1. Bir tarayıcıdan [Azure portalına](http://portal.azure.com/) gidin ve Azure hesabınızla oturum açın.
2. **Kaynak oluştur**’a tıklayın. İçinde **Market arama** alanına, `virtual network`''. Bulun **sanal ağ** döndürülen listesinden ve açık **sanal ağ** sayfası.
3. Gelen sanal ağ sayfasının alt kısmındaki yakınında **dağıtım modeli seçin** listesinde, seçin **Resource Manager**ve ardından **oluşturma**. Bu işlem "Sanal ağ geçidi oluştur" sayfasını açar.
4. **Sanal ağ oluştur** sayfasında sanal ağ ayarlarını yapılandırın. Alanları doldururken, alana girilen karakterler geçerliyse kırmızı ünlem işareti yeşil onay işaretine dönüşür.
5. Bu adımları yineleyin **Kiracı portalı** Azure yığınının.

## <a name="add-a-gateway-subnet"></a>Ağ geçidi alt ağı ekleme

Sanal ağınızı bir ağ geçidine bağlamadan önce, bağlamak istediğiniz sanal ağ için ağ geçidi alt ağını oluşturmanız gerekir. Ağ geçidi hizmetleri, ağ geçidi alt ağında belirtilen IP adreslerini kullanır.

[Portal](http://portal.azure.com/)’da, sanal ağ geçidini oluşturmak istediğiniz Resource Manager sanal ağına gidin.

1. İçinde **ayarları** seçin, VNet sayfanızı bölümünü **alt ağlar** genişletmek için **alt ağlar** sayfası.
2. Üzerinde **alt ağlar** sayfasında, **+ ağ geçidi alt ağı** açmak için **alt ağ Ekle** sayfası.

    ![](media/azure-stack-solution-hybrid-connectivity/image4.png)

3. Alt ağınız için **Ad** alanı otomatik olarak ‘GatewaySubnet’ değeriyle doldurulur. Bu değer, alt ağ geçidi alt ağı tanımak Azure için gereklidir. Otomatik doldurulmuş ayarlamak **adres aralığı** yapılandırma gereksinimlerinize uygun değerleri seçip **Tamam** alt ağ oluşturmak için sayfanın altındaki.

## <a name="create-a-virtual-network-gateway-in-azure-and-azure-stack"></a>Azure ve Azure sanal ağ geçidi oluşturma yığını

1. Portal sayfasının sol taraftan + ve aramada 'Sanal ağ geçidi' girin. İçinde **sonuçları**, bulun ve seçin **sanal ağ geçidi**.
2. Ekranın alt kısmındaki **sanal ağ geçidi** sayfasında, **oluşturma**. Bu işlem **Sanal ağ geçidi oluşturma** sayfasını açar.
3. Üzerinde **sanal ağ geçidi Oluştur** sayfasında, örnek değerleri açıklanmış aşağıda açıklanan ek değerler ayrıntılı olarak sanal ağ geçidiniz için değerleri belirtin.
    - **SKU**: temel
    - **Sanal ağ**: daha önce oluşturduğunuz sanal ağı seçin. Ayrıca, daha önce oluşturduğunuz ağ geçidi alt ağı otomatik olarak seçer. 
    - **İlk IP yapılandırması**: ağ geçidinizin genel IP budur. 
        - Tıklatın **oluşturma ağ geçidi IP yapılandırması** ve bu gideceksiniz **genel IP adresi seçin** sayfası.
        - Tıklatın **+ Yeni Oluştur** açmak için **ortak IP adresi oluştur** sayfası.
        - Girin bir **adı** ortak IP adresi için. SKU olarak bırakın **temel**seçeneğini belirleyip **Tamam** yaptığınız değişiklikleri kaydetmek için bu sayfanın sonundaki.

    > [!Note]  
    > VPN ağ geçidi şu anda yalnızca dinamik genel IP adresi ayırma destekler. Ancak, bu durum IP adresinin VPN ağ geçidinize atandıktan sonra değiştiği anlamına gelmez. Genel IP adresi, yalnızca ağ geçidi silinip yeniden oluşturulduğunda değişir. VPN ağ geçidiniz üzerinde gerçekleştirilen yeniden boyutlandırma, sıfırlama veya diğer iç bakım/yükseltme işlemleri sırasında değişmez.

4. Ayarları doğrulayın. 
5. VPN ağ geçidi oluşturmaya başlamak için **Oluştur**'a tıklayın. Ayarlar doğrulandıktan sonra, panoda "Sanal ağ geçidi dağıtma" kutucuğunu görürsünüz. Bir ağ geçidi oluşturma 45 dakika kadar sürebilir. Tamamlanma durumunu görmek için portal sayfanızı yenilemeniz gerekebilir.

    Ağ geçidi oluşturulduktan sonra, portalda sanal ağa bakarak bu ağ geçidine atanmış IP adresini görüntüleyin. Ağ geçidi bağlı bir cihaz gibi görüntülenir. Daha fazla bilgi görüntülemek için bağlı aygıt (sanal ağ geçidiniz) seçebilirsiniz.
6. Azure yığın dağıtımınızdaki bu adımları yineleyin.

## <a name="create-the-local-network-gateway-in-azure-and-azure-stack"></a>Azure ve Azure yığın yerel ağ geçidi oluşturma

Yerel ağ geçidi genellikle şirket içi konumunuz anlamına gelir. Site olarak Azure veya Azure yığını başvurduğu sonra kullanabilirsiniz bağlantı oluşturacağınız şirket içi VPN cihazının IP adresini belirtin bir ad verin. Ayrıca, VPN ağ geçidi üzerinden VPN cihazına yönlendirilecek IP adresi ön eklerini de belirtirsiniz. Belirttiğiniz adres ön ekleri, şirket içi adresinizde yer alan ön eklerdir. Şirket içi ağınız değişirse veya VPN cihazının genel IP adresini değiştirmeniz gerekirse değerleri daha sonra kolayca güncelleştirebilirsiniz.

1. Portalı'nda seçin **+ kaynak oluşturma**.
2. Arama kutusuna **yerel ağ geçidi**, tuşuna basarak **Enter** aramak için. Böylece sonuçların bir listesi döndürülür. Tıklatın **yerel ağ geçidi**seçeneğini belirleyip **oluşturma** açmak için düğmeye **oluşturma yerel ağ geçidi** sayfası.
3. Üzerinde **Oluştur yerel ağ geçidi sayfası**, bizim örnek değerler artı aşağıda açıklanan ek değerler ayrıntılı olarak yerel ağ geçidinizin değerlerini belirtin.
    - **IP adresi**: Azure veya Azure yığın bağlanmak istediğiniz VPN cihazının genel IP adresi budur. Geçerli bir genel IP adresi belirtin. IP adresi, NAT'nin ardında olamaz ve Azure tarafından erişilebilir olması gerekir. IP adresini şu anda bilmiyorsanız örnekte gösterilen değerleri kullanabilirsiniz ancak geri dönüp yer tutucu IP adresinizi VPN cihazınızın genel IP adresiyle değiştirmeniz gerekir. Aksi halde Azure bağlantı kuramaz.
    - **Adres Alanı**, bu yerel ağın temsil ettiği ağa ilişkin adres aralıkları anlamına gelir. Birden fazla adres alanı aralığı ekleyebilirsiniz. Burada belirttiğiniz aralıkların, bağlanmak istediğiniz diğer ağların aralıklarıyla çakışmadığından emin olun. Azure, belirttiğiniz adres aralığını şirket içi VPN cihazının IP adresine yönlendirir. Örnekte gösterilen değerleri, şirket içi siteye bağlanmak isterseniz kendi değerlerinizi burada kullanın.
    - **BGP ayarlarını yapılandırmak**: BGP yapılandırırken kullanabilirsiniz. Aksi takdirde, bu seçeneği işaretlemeyin.
    - **Abonelik**: doğru aboneliğin gösterildiğini doğrulayın.
    - **Kaynak grubu**: kullanmak istediğiniz kaynak grubunu seçin. Yeni bir kaynak grubu oluşturabilir veya önceden oluşturduğunuz birini seçebilirsiniz.
    - **Konum**: Bu nesnenin içinde oluşturulacak konumu seçin. VNet'inizin bulunduğu konumu seçebilirsiniz ancak bu zorunlu değildir.
4. Değerleri belirtmeyi tamamladığınızda seçin **oluşturma** yerel ağ geçidi oluşturmak için sayfanın altındaki düğmesini.
5. Azure yığın dağıtımınızdaki bu adımları yineleyin.

## <a name="configure-your-connection"></a>Bağlantınızı yapılandırın

Bir şirket içi ağı ile Siteden Siteye bağlantılar için VPN cihazı gerekir. Bu adımda, bir bağlantı olarak bilinen VPN Cihazınızı yapılandırın. Bağlantınızı yapılandırırken, aşağıdakiler gerekir:
    - Paylaşılan bir anahtar. Siteden Siteye VPN bağlantınızı oluştururken belirttiğiniz paylaşılan anahtarın aynısıdır. Bu örneklerde temel bir paylaşılan anahtar kullanılır. Kullanmak için daha karmaşık bir anahtar oluşturmanız önerilir.
    - Sanal ağ geçidinizin Genel IP adresi. Azure Portal, PowerShell veya CLI kullanarak genel IP adresini görüntüleyebilirsiniz. Azure Portalı'nı kullanarak VPN ağ geçidinizin genel IP adresini bulmak için sanal ağ geçitleri için gidin, sonra ağ geçidi adını seçin.
Sanal ağ geçidiniz ile şirket içi VPN cihazınız arasında Siteden Siteye VPN bağlantısı oluşturun.
1. Portalı'nda seçin **+ kaynak oluşturma**.
2. Arama kutusuna **bağlantıları**, tuşuna basarak **Enter** aramak için. Böylece sonuçların bir listesi döndürülür. Tıklatın **bağlantıları**, açmak için Oluştur düğmesine seçip **bağlantıları oluşturma** sayfa.
3. Üzerinde **bağlantıları oluşturma** sayfasında, bağlantınızı değerlerini yapılandırın.
     - Temel Kavramları:
        1. **Bağlantı türü**: siteden siteye (IPSec) seçin.
        2. **Kaynak grubu**: (test kaynak grubunuzu seçin)
     - Ayarlar:
        1. **Sanal ağ geçidi**: daha önce oluşturduğunuz sanal ağ geçidi seçin.
        2. **Yerel ağ geçidi**: daha önce oluşturduğunuz yerel ağ geçidi seçin. 
        3. **Bağlantı adı**: Bu otomatik iki ağ geçidi değerlerle doldurun. 
        4. **Paylaşılan anahtar**: Buradaki değer yerel şirket içi VPN cihazınız için kullanmakta olduğunuz değerle eşleşmelidir. Örnekte 'abc123' değeri kullanılmıştır, ancak siz daha karmaşık bir değer kullanabilirsiniz (ve kullanmalısınız). Önemli olan, burada belirttiğiniz değerin VPN cihazınızı yapılandırırken belirttiğiniz değerle aynı olmasıdır.
    - **Abonelik**, **Kaynak Grubu** ve **Konum** için kalan değerler sabittir.
4. Bağlantınızı oluşturmak için **Tamam**’a tıklayın. Ekranda flash bağlantı oluşturma görürsünüz.
5. Bağlantı görüntüleyebilirsiniz ** sanal ağ geçidi bağlantıları sayfası. Durum çalışmaya başlayacaktır **bilinmeyen** için **bağlanıyor**ve ardından **başarılı**.

## <a name="next-steps"></a>Sonraki adımlar

- Azure bulut desenleri hakkında daha fazla bilgi için bkz: [bulut tasarım modeli](https://docs.microsoft.com/azure/architecture/patterns).
