---
title: "Şirket içi ağınızı bir Azure sanal ağına bağlama: Siteden Siteye VPN: Klasik portal | Microsoft Docs"
description: "Şirket içi ağınız ile bir Azure sanal ağı arasında genel İnternet üzerinden bir IPSec bağlantısı oluşturma adımları. Bu adımlar, Klasik portalı ve klasik dağıtım modelini kullanarak Siteden Siteye şirket içi ve dışı karışık VPN Gateway bağlantısı oluşturmanıza yardımcı olur."
services: vpn-gateway
documentationcenter: 
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 024ecb29-64de-4ff1-84f1-1a45a8595f0b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/11/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: ff70484dff03a44d23d2cf34ce115fd57c4b0390
ms.lasthandoff: 04/12/2017


---
# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-classic-portal"></a>Klasik portalı kullanarak Siteden Siteye bağlantı ile sanal ağ oluşturma

Siteden Siteye (S2S) VPN ağ geçidi bağlantısı, IPSec/IKE (IKEv1 veya IKEv2) VPN tüneli üzerinden kurulan bir bağlantıdır. Bu bağlantı türü için, şirket içinde ortak IP adresi atanmış olan ve NAT'nin arkasında bulunmayan bir VPN cihazı gerekir. Siteden Siteye bağlantılar, şirket içi ve dışı karışık yapılandırmalar ve karma yapılandırmalar için kullanılabilir.

![Siteden Siteye şirket içi ve dışı karışık VPN Gateway bağlantısı diyagramı](./media/vpn-gateway-site-to-site-create/site-to-site-connection-diagram.png)

Bu makale, klasik dağıtım modelini ve klasik portalı kullanarak sanal bir ağ ve şirket içi ağınızda konumdan konuma VPN ağ geçidi bağlantısı oluşturma işleminde size yol gösterir. Siteden Siteye bağlantılar, şirket içi ve dışı karışık yapılandırmalar ve karma yapılandırmalar için kullanılabilir. Ayrıca aşağıdaki listeden farklı bir seçenek belirleyerek, Resource Manager dağıtım modeli için bu yapılandırmayı oluşturabilirsiniz:

> [!div class="op_single_selector"]
> * [Resource Manager - Azure portalı](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Klasik - Azure portalı](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Klasik - klasik portal](vpn-gateway-site-to-site-create.md)
>
>

#### <a name="additional-configurations"></a>Ek yapılandırmalar
VNet'leri birbirine bağlamak istiyorsanız bkz. [Klasik dağıtım modeli için bir VNet - VNet bağlantısını yapılandırma](virtual-networks-configure-vnet-to-vnet-connection.md). VNet’e Konumdan Konuma bağlantı eklemek istiyorsanız bkz. [VNet’e mevcut bir VPN ağ geçidi bağlantısıyla S2S bağlantısı ekleme](vpn-gateway-multi-site.md).

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Yapılandırmaya başlamadan önce aşağıdaki öğelerin bulunduğunu doğrulayın:

* Uyumlu bir VPN cihazı ve bu cihazı yapılandırabilecek biri. Bkz. [VPN Cihazları Hakkında](vpn-gateway-about-vpn-devices.md). VPN cihazınızı yapılandırma konusuyla veya şirket içi ağ yapılandırmanızdaki IP adresi aralıklarıyla ilgili fazla bilginiz yoksa size bu ayrıntıları sağlayabilecek biriyle çalışmanız gerekir.
* VPN cihazınız için dışarıya yönelik genel bir IP adresi. Bu IP adresi bir NAT’nin arkasında olamaz.
* Azure aboneliği. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial) için kaydolabilirsiniz.

## <a name="CreateVNet"></a>Sanal ağınızı oluşturma
1. [Klasik Azure portalında](https://manage.windowsazure.com/) oturum açın.
2. Ekranın sol alt köşesinde **Yeni**’ye tıklayın. Gezinme bölmesinde **Ağ Hizmetleri**’ne, sonra da **Virtual Network**’a tıklayın. Yapılandırma sihirbazını başlatmak için **Özel Oluştur**’a tıklayın.
3. Sanal ağınızı oluşturmak için aşağıdaki sayfalara yapılandırma ayarlarınızı girin:

## <a name="Details"></a>Sanal ağ ayrıntıları sayfası
Aşağıdaki bilgileri girin:

* **Ad**: Sanal ağınıza bir ad verin. Örneğin, *EastUSVNet*. VM’lerinizi ve PaaS örneklerinizi dağıtırken bu sanal ağ adını kullanacağınız için, karışık bir isim vermekten kaçınmak isteyebilirsiniz.
* **Konum**: Konum, kaynaklarınızın (VM’ler) bulunmasını istediğiniz fiziksel konum (bölge) ile doğrudan ilişkilidir. Örneğin, bu sanal ağa dağıttığınız VM’lerin fiziksel olarak *Doğu ABD*’de bulunmasını istiyorsanız, o konumu seçin. Sanal ağınızı oluşturduktan sonra sanal ağınızla ilişkili bölgeyi değiştiremezsiniz.

## <a name="DNS"></a>DNS sunucuları ve VPN bağlantı sayfası
Aşağıdaki bilgileri girin ve sağ alt köşedeki ileri okuna tıklayın.

* **DNS Sunucuları**: DNS sunucusunun adını ve IP adresini girin veya kısayol menüsünden, önceden kaydedilmiş bir DNS sunucusu seçin. Bu ayarla bir DNS sunucusu oluşturulmaz. Söz konusu ayar, bu sanal ağa ilişkin ad çözümlemesi için kullanmak istediğiniz DNS sunucularını belirtmenize olanak sağlar.
* **Siteden Siteye VPN’i Yapılandırma**: **Siteden siteye VPN’i yapılandırma** onay kutusunu seçin.
* **Yerel Ağ**: Yerel ağ, fiziksel şirket içi konumunuzu temsil eder. Daha önce oluşturduğunuz bir yerel ağı seçebilir veya yeni bir yerel ağ oluşturabilirsiniz. Ancak daha önce oluşturduğunuz yerel bir ağı kullanmayı seçerseniz **Yerel Ağlar** yapılandırma sayfasına gidin ve VPN cihazına ilişkin VPN Cihazı IP adresinin (genel kullanıma yönelik IPv4 adresi) doğru olduğundan emin olun.

## <a name="Connectivity"></a>Konumdan Konuma bağlantı sayfası
Yeni bir yerel ağ oluşturuyorsanız **Siteden Siteye Bağlantı** sayfasını görürsünüz. Daha önce oluşturduğunuz bir yerel ağı kullanmak istiyorsanız, sihirbazda bu sayfa görünmez ve bir sonraki bölüme geçebilirsiniz.

Aşağıdaki bilgileri girin ve ileri okuna tıklayın.

* **Adı**: Yerel (şirket içi) ağ sitenize vermek istediğiniz ad.
* **VPN Cihazı IP Adresi**: Azure'a bağlanmak için kullandığınız şirket içi VPN cihazınızın genel kullanıma yönelik IPv4 adresi. VPN cihazı bir NAT’nin arkasına yerleştirilemez.
* **Adres Alanı**: Başlangıç IP’si ve CIDR’si (Adres Sayısı) ekleyin. Sanal ağ geçidinden, yerel şirket içi konumunuza gönderilmesini istediğiniz adres aralığını (veya aralıklarını) burada belirtin. Hedef IP adresi, burada belirttiğiniz aralıkta yer alıyorsa sanal ağ geçidinden yönlendirilir.
* **Adres alanı ekle**: Sanal ağ geçidinden gönderilmesini istediğiniz birden fazla adres aralığınız varsa ekleyeceğiniz tüm adres aralıklarını burada belirtin. Daha sonra **Yerel Ağ** sayfasından aralık ekleyebilir veya kaldırabilirsiniz.

## <a name="Address"></a>Sanal ağ adres alanları sayfası
Sanal ağınız için kullanmak istediğiniz adres aralığını belirtin. Belirlediğiniz adresler, bu sanal ağa dağıtacağınız VM’ler ve diğer rol örneklerine atanacak olan dinamik IP adresleridir (DIPS).

Şirket içi ağınız için kullanılan aralıklardan herhangi biriyle çakışmayan bir aralık seçmeniz çok önemlidir. Ağ yöneticinizle birlikte çalışmanız gerekir. Ağ yöneticinizin şirket içi ağ adresi alanınızdan, sanal ağınızda kullanabilmeniz için IP adresi aralığı ayırması gerekebilir.

Aşağıdaki bilgileri girin ve ağınızı yapılandırmak için sağ alt köşedeki onay işaretine tıklayın.

* **Adres Alanı**: Başlangıç IP’si ve Adres Sayısı ekleyin. Belirlediğiniz adres alanlarının, şirket içi ağınızdaki adres alanlarından herhangi biriyle çakışmadığını doğrulayın.
* **Alt ağ ekle**: Başlangıç IP’si ve Adres Sayısı ekleyin. Ek alt ağlar gerekli değildir, ancak statik DIPS içerecek VM’ler için ayrı bir alt ağ oluşturmak isteyebilirsiniz. Veya VM’lerinizi diğer rol örneklerinizden ayrı bir alt ağda tutmak isteyebilirsiniz.
* **Ağ geçidi alt ağı ekle**: Ağ geçidi alt ağı eklemek için tıklayın. Ağ geçidi alt ağı, yalnızca sanal ağ geçidi için kullanılır ve bu yapılandırma için gereklidir.

Sayfanın altındaki onay işaretine tıkladığınızda sanal ağınız oluşturulmaya başlar. Tamamlandığında, Klasik Azure Portalı’nın **Ağlar** sayfasındaki **Durum**’un altında, **Oluşturuldu** yazısını görürsünüz. VNet oluşturulduktan sonra sanal ağ geçidinizi yapılandırabilirsiniz.

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="VNetGateway"></a>Sanal ağ geçidinizi yapılandırma
Güvenli bir siteden siteye bağlantı oluşturmak için sanal ağ geçidini yapılandırın. Bkz. [Klasik Azure portalında sanal ağ geçidi yapılandırma](vpn-gateway-configure-vpn-gateway-mp.md).

## <a name="next-steps"></a>Sonraki adımlar
 Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).


