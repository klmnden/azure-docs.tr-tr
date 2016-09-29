<properties
   pageTitle="Klasik Azure portalını kullanarak siteden siteye VPN Gateway bağlantısı ile sanal ağ oluşturma | Microsoft Azure"
   description="Klasik dağıtım modelini kullanarak, şirket içi ve şirket dışı karışık yapılandırmalar ile karma yapılandırmalar için S2S VPN Gateway bağlantısı olan bir VNet oluşturun."
   services="vpn-gateway"
   documentationCenter=""
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="cherylmc"/>


# Klasik Azure portalını kullanarak Siteden Siteye bağlantı ile VNet oluşturma

> [AZURE.SELECTOR]
- [Resource Manager - Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Resource Manager - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Klasik - Klasik Portal](vpn-gateway-site-to-site-create.md)

Bu makalede **klasik dağıtım modelini** ve klasik portalı kullanarak sanal bir ağ ve şirket içi ağınızda siteden siteye VPN bağlantısı oluşturma işlemi adım adım açıklanmaktadır. Siteden Siteye bağlantılar, şirket içi ve dışı karışık yapılandırmalar ve karma yapılandırmalar için kullanılabilir. Şu anda Azure portalını kullanarak klasik dağıtım modeli için uçtan uca bir Siteden Siteye yapılandırması oluşturamazsınız.

![Siteden Siteye diyagram](./media/vpn-gateway-site-to-site-create/site2site.png "site-to-site")


### Siteden Siteye bağlantılar için dağıtım modelleri ve araçları

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)]

VNet'leri birbirine bağlamak istiyorsanız bkz. [Klasik dağıtım modeli için bir VNet - VNet bağlantısını yapılandırma](virtual-networks-configure-vnet-to-vnet-connection.md). 
 
## Başlamadan önce

Yapılandırmaya başlamadan önce aşağıdaki öğelerin bulunduğunu doğrulayın.

- Uyumlu bir VPN cihazı ve bu cihazı yapılandırabilecek biri. Bkz. [VPN Cihazları Hakkında](vpn-gateway-about-vpn-devices.md). VPN cihazınızı yapılandırma konusuyla veya şirket içi ağ yapılandırmanızdaki IP adresi aralıklarıyla ilgili fazla bilginiz yoksa size bu ayrıntıları sağlayabilecek biriyle çalışmanız gerekir.

- VPN cihazınız için dışarıya yönelik genel bir IP adresi. Bu IP adresi bir NAT’nin arkasında olamaz.

- Azure aboneliği. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.


## Sanal ağınızı oluşturma

1. [Klasik Azure portalında](https://manage.windowsazure.com/) oturum açın.

2. Ekranın sol alt köşesinde **Yeni**’ye tıklayın. Gezinme bölmesinde **Ağ Hizmetleri**’ne, sonra da **Virtual Network**’a tıklayın. Yapılandırma sihirbazını başlatmak için **Özel Oluştur**’a tıklayın.

3. Sanal ağınızı oluşturmak için aşağıdaki sayfalara yapılandırma ayarlarınızı girin:

## Sanal ağ detayları sayfası

Aşağıdaki bilgileri girin:

- **Ad**: Sanal ağınıza bir ad verin. Örneğin, *EastUSVNet*. VM’lerinizi ve PaaS örneklerinizi dağıtırken bu sanal ağ adını kullanacağınız için, karışık bir isim vermekten kaçınmak isteyebilirsiniz.
- **Konum**: Konum, kaynaklarınızın (VM’ler) bulunmasını istediğiniz fiziksel konum (bölge) ile doğrudan ilişkilidir. Örneğin, bu sanal ağa dağıttığınız VM’lerin fiziksel olarak *Doğu ABD*’de bulunmasını istiyorsanız, o konumu seçin. Sanal ağınızı oluşturduktan sonra sanal ağınızla ilişkili bölgeyi değiştiremezsiniz.

## DNS sunucuları ve VPN bağlantı sayfası

Aşağıdaki bilgileri girin ve sağ alt köşedeki ileri okuna tıklayın.

- **DNS Sunucuları**: DNS sunucusunun adını ve IP adresini girin veya kısayol menüsünden, önceden kaydedilmiş bir DNS sunucusu seçin. Bu ayarla bir DNS sunucusu oluşturulmaz. Söz konusu ayar, bu sanal ağa ilişkin ad çözümlemesi için kullanmak istediğiniz DNS sunucularını belirtmenize olanak sağlar.
- **Siteden Siteye VPN’i Yapılandırma**: **Siteden siteye VPN’i yapılandırma** onay kutusunu seçin.
- **Yerel Ağ**: Yerel ağ, fiziksel şirket içi konumunuzu temsil eder. Daha önce oluşturduğunuz bir yerel ağı seçebilir veya yeni bir yerel ağ oluşturabilirsiniz. Ancak daha önce oluşturduğunuz yerel bir ağı kullanmayı seçerseniz **Yerel Ağlar** yapılandırma sayfasına gidin ve VPN cihazına ilişkin VPN Cihazı IP adresinin (genel kullanıma yönelik IPv4 adresi) doğru olduğundan emin olun.

## Siteden Siteye bağlantı sayfası

Yeni bir yerel ağ oluşturuyorsanız **Siteden Siteye Bağlantı** sayfasını görürsünüz. Daha önce oluşturduğunuz bir yerel ağı kullanmak istiyorsanız, sihirbazda bu sayfa görünmez ve bir sonraki bölüme geçebilirsiniz.

Aşağıdaki bilgileri girin ve ileri okuna tıklayın.

-   **Adı**: Yerel (şirket içi) ağ sitenize vermek istediğiniz ad.
-   **VPN Cihazı IP Adresi**: Azure'a bağlanmak için kullandığınız şirket içi VPN cihazınızın genel kullanıma yönelik IPv4 adresi. VPN cihazı bir NAT’nin arkasına yerleştirilemez.
-   **Adres Alanı**: Başlangıç IP’si ve CIDR’si (Adres Sayısı) ekleyin. Sanal ağ geçidinden, yerel şirket içi konumunuza gönderilmesini istediğiniz adres aralığını (veya aralıklarını) burada belirtin. Hedef IP adresi, burada belirttiğiniz aralıkta yer alıyorsa sanal ağ geçidinden yönlendirilir.
-   **Adres alanı ekle**: Sanal ağ geçidinden gönderilmesini istediğiniz birden fazla adres aralığınız varsa ekleyeceğiniz tüm adres aralıklarını burada belirtin. Daha sonra **Yerel Ağ** sayfasından aralık ekleyebilir veya kaldırabilirsiniz.

## Sanal ağ adres alanları sayfası

Sanal ağınız için kullanmak istediğiniz adres aralığını belirtin. Belirlediğiniz adresler, bu sanal ağa dağıtacağınız VM’ler ve diğer rol örneklerine atanacak olan dinamik IP adresleridir (DIPS).

Şirket içi ağınız için kullanılan aralıklardan herhangi biriyle çakışmayan bir aralık seçmeniz çok önemlidir. Ağ yöneticinizle birlikte çalışmanız gerekir. Ağ yöneticinizin şirket içi ağ adresi alanınızdan, sanal ağınızda kullanabilmeniz için IP adresi aralığı ayırması gerekebilir.

Aşağıdaki bilgileri girin ve ağınızı yapılandırmak için sağ alt köşedeki onay işaretine tıklayın.

- **Adres Alanı**: Başlangıç IP’si ve Adres Sayısı ekleyin. Belirlediğiniz adres alanlarının, şirket içi ağınızdaki adres alanlarından herhangi biriyle çakışmadığını doğrulayın.
- **Alt ağ ekle**: Başlangıç IP’si ve Adres Sayısı ekleyin. Ek alt ağlar gerekli değildir, ancak statik DIPS içerecek VM’ler için ayrı bir alt ağ oluşturmak isteyebilirsiniz. Veya VM’lerinizi diğer rol örneklerinizden ayrı bir alt ağda tutmak isteyebilirsiniz.
- **Ağ geçidi alt ağı ekle**: Ağ geçidi alt ağı eklemek için tıklayın. Ağ geçidi alt ağı, yalnızca sanal ağ geçidi için kullanılır ve bu yapılandırma için gereklidir.

Sayfanın altındaki onay işaretine tıkladığınızda sanal ağınız oluşturulmaya başlar. Tamamlandığında, Klasik Azure Portalı’nın **Ağlar** sayfasındaki **Durum**’un altında, **Oluşturuldu** yazısını görürsünüz. VNet oluşturulduktan sonra sanal ağ geçidinizi yapılandırabilirsiniz.

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 

## Sanal ağ geçidinizi yapılandırma

Güvenli bir siteden siteye bağlantı oluşturmak için sanal ağ geçidini yapılandırın. Bkz. [Klasik Azure portalında sanal ağ geçidi yapılandırma](vpn-gateway-configure-vpn-gateway-mp.md).

## Sonraki adımlar

Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. [Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/) belgeleri.



<!--HONumber=Sep16_HO3-->


