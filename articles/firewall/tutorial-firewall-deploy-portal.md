---
title: "Öğretici: Azure portalı kullanarak Azure Güvenlik Duvarı'nı dağıtma ve yapılandırma"
description: Bu öğreticide Azure portalı kullanarak Azure Güvenlik Duvarı'nı dağıtmayı ve yapılandırmayı öğreneceksiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.date: 11/15/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 2dd9fc5691c646a72936039b6bcc5949d227c6b5
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57545345"
---
# <a name="tutorial-deploy-and-configure-azure-firewall-using-the-azure-portal"></a>Öğretici: Azure portalı kullanarak Azure Güvenlik Duvarı'nı dağıtma ve yapılandırma

Giden ağ erişimini denetleme, genel ağ güvenlik planının önemli bir parçasıdır. Örneğin web sitelerine erişimi veya erişilebilen giden IP adreslerini ve bağlantı noktalarını kısıtlamak isteyebilirsiniz.

Azure Güvenlik Duvarı, Azure alt ağından giden ağ erişimini denetlemenin bir yoludur. Azure Güvenlik Duvarı ile şunları yapılandırabilirsiniz:

* Bir alt ağdan erişilebilen tam etki alanı adlarını (FQDN) tanımlayan uygulama kuralları.
* Kaynak adres, protokol, hedef bağlantı noktası ve hedef adresini tanımlayan ağ kuralları.

Ağ trafiğinizi güvenlik duvarından alt ağın varsayılan ağ geçidi olarak yönlendirdiğinizde ağ trafiği yapılandırılan güvenlik duvarı kurallarına tabi tutulur.

Bu öğreticide kolay dağıtım için üç alt ağa sahip tek bir basitleştirilmiş sanal ağ oluşturursunuz. Üretim dağıtımları için [hub ve bağlı bileşen modeli](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) önerilir ve bu yapıda güvenlik duvarı kendi sanal ağında, iş yükü sunucuları ise bir veya daha fazla alt ağ ile aynı bölgedeki eş sanal ağlarda bulunur.

- **AzureFirewallSubnet** - güvenlik duvarı bu alt ağdadır.
- **Workload-SN**: İş yükü sunucusu bu alt ağda yer alır. Bu alt ağın ağ trafiği güvenlik duvarından geçer.
- **Jump-SN**: "Atlama" sunucusu bu alt ağda yer alır. Atlama sunucusu, Uzak Masaüstü ile bağlanabileceğiniz genel IP adresine sahiptir. Buradan iş yükü sunucusuna bağlanabilirsiniz (başka bir Uzak Masaüstü oturumuyla).

![Öğretici ağı altyapısı](media/tutorial-firewall-rules-portal/Tutorial_network.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Test amaçlı ağ ortamı oluşturma
> * Güvenlik duvarı dağıtma
> * Varsayılan rota oluşturma
> * Msn.com erişmesine izin vermek için bir uygulama yapılandırma
> * Dış DNS sunucularına erişime izin vermek için ağ kuralı yapılandırma
> * Güvenlik duvarını test etme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="set-up-the-network"></a>Ağı ayarlama

İlk olarak güvenlik duvarını dağıtmak için gerekli olan kaynakları içerecek bir kaynak grubu oluşturun. Ardından sanal ağı, alt ağları ve test sunucularını oluşturun.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, bu öğreticideki tüm kaynakları içerir.

1. [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.
2. Azure portalı giriş sayfasında **Kaynak grupları** > **Ekle**'ye tıklayın.
3. **Kaynak grubu adı** alanına **Test-FW-RG** yazın.
4. **Abonelik** bölümünde aboneliğinizi seçin.
5. **Kaynak grubu konumu** bölümünde bir konum seçin. Bundan sonra oluşturacağınız tüm kaynakların aynı konumda olması gerekir.
6. **Oluştur**’a tıklayın.

### <a name="create-a-vnet"></a>Sanal ağ oluşturma

Bu sanal ağda üç alt ağ bulunacaktır.

1. Azure portal giriş sayfasında **Tüm hizmetler**'e tıklayın.
2. **Ağ** bölümünde **Sanal ağlar**'a tıklayın.
3. **Ekle**'ye tıklayın.
4. **Ad** alanına **Test-FW-VN** yazın.
5. **Adres alanı** için **10.0.0.0/16** yazın.
6. **Abonelik** bölümünde aboneliğinizi seçin.
7. **Kaynak grubu** olarak **Var olanı kullan** > **Test-FW-RG**’yi seçin.
8. **Konum** alanında önceden kullandığınız konumu seçin.
9. **Alt ağ** bölümünde **Ad** alanına **AzureFirewallSubnet** yazın. Güvenlik duvarı bu alt ağda yer alacaktır ve alt ağ adının **mutlaka** AzureFirewallSubnet olması gerekir.
10. **Adres aralığı** için **10.0.1.0/24** yazın.
11. Diğer alanlar için varsayılan değerleri kullanın ve ardından **Oluştur**'a tıklayın.

> [!NOTE]
> En düşük AzureFirewallSubnet alt /26 boyutudur.

### <a name="create-additional-subnets"></a>Ek alt ağ oluşturma

Bir sonraki adımda atlama sunucusu için alt ağlar ve iş yükü sunucuları için bir alt ağ oluşturun.

1. Azure portalı giriş sayfasında **Kaynak grupları** > **Test-FW-RG**'ye tıklayın.
2. **Test-FW-VN** sanal ağına tıklayın.
3. **Alt ağlar** > **+Alt ağ**’a tıklayın.
4. **Ad** alanına **Workload-SN** yazın.
5. **Adres aralığı** için **10.0.2.0/24** yazın.
6. **Tamam** düğmesine tıklayın.

**Jump-SN** adına ve **10.0.3.0/24** adres aralığına sahip bir alt ağ daha oluşturun.

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Şimdi atlama ve iş yükü sanal makinelerini oluşturup uygun alt ağlara yerleştirin.

1. Azure portalında **Kaynak oluştur**’a tıklayın.
2. Tıklayın **işlem** seçip **Windows Server 2016 Datacenter** Öne çıkanlar listesinde.
3. Sanal makine için şu değerleri girin:

    - *Test FW RG* kaynak grubu için.
    - *SRV atlama* - sanal makinenin adı.
    - Yönetici kullanıcı adı için *azureuser*.
    - *Azure123456!* girin.

4. Altında **gelen bağlantı noktası kuralları**, için **ortak gelen bağlantı noktası**, tıklayın **Seçili bağlantı noktalarına izin**.
5. İçin **seçin gelen bağlantı noktalarının**seçin **RDP (3389)**.

6. Diğer varsayılan değerleri kabul edin ve tıklayın **sonraki: Diskleri**.
7. Disk Varsayılanları kabul edin ve tıklayın **sonraki: Ağ**.
8. Emin olun **Test FW VN** sanal ağı ve alt ağ için seçili olduğu **atlama SN**.
9. İçin **genel IP**, tıklayın **Yeni Oluştur**.
10. Tür **Srv atlama PIP** genel IP adresi adı ve tıklatın **Tamam**.
11. Diğer varsayılan değerleri kabul edin ve tıklayın **sonraki: Yönetim**.
12. Tıklayın **kapalı** önyükleme tanılaması devre dışı bırakmak için. Diğer varsayılan değerleri kabul edin ve tıklayın **gözden geçir + Oluştur**.
13. Özet sayfasında ayarları gözden geçirin ve ardından **Oluştur**.

Bu işlemleri tekrarlayarak **Srv-Work** adlı başka bir sanal makine oluşturun.

Bilgileri aşağıdaki tabloda Srv iş sanal makineyi yapılandırmak için kullanın. Yapılandırmanın geri kalanı Srv-Jump sanal makinesiyle aynıdır.

|Ayar  |Değer  |
|---------|---------|
|Alt ağ|Workload-SN|
|Genel IP|None|
|Ortak gelen bağlantı noktası|None|

## <a name="deploy-the-firewall"></a>Güvenlik duvarını dağıtma

Güvenlik duvarını sanal ağa dağıtın.

1. Portal giriş sayfasında **Kaynak oluştur**'a tıklayın.
2. **Ağ**'a tıklayın ve **Öne çıkanlar**'ın sonrasında **Tümünü gör**'e tıklayın.
3. **Güvenlik duvarı** > **Oluştur**’a tıklayın. 
4. **Güvenlik duvarı oluştur** sayfasında aşağıdaki ayarları kullanarak güvenlik duvarını yapılandırın:

   |Ayar  |Değer  |
   |---------|---------|
   |Ad     |Test-FW01|
   |Abonelik     |\<aboneliğiniz\>|
   |Kaynak grubu     |**Var olanı kullan**: Test FW RG |
   |Konum     |Önceden kullandığınız konumu seçin|
   |Bir sanal ağ seçin     |**Var olanı kullan**: Test FW VN|
   |Genel IP adresi     |**Yeni oluşturun**. Genel IP adresinin türü Standart SKU olmalıdır.|

5. **Gözden geçir ve oluştur**’a tıklayın.
6. Özeti gözden geçirin ve **Oluştur**'a tıklayarak güvenlik duvarını oluşturun.

   Dağıtma işlemi birkaç dakika sürebilir.
7. Dağıtım tamamlandıktan sonra **Test-FW-RG** kaynak grubuna gidin ve **Test-FW01** güvenlik duvarına tıklayın.
8. Özel IP adresini not edin. Varsayılan rotayı oluştururken bu adresi kullanacaksınız.

## <a name="create-a-default-route"></a>Varsayılan rota oluşturma

**Workload-SN** alt ağında varsayılan giden rotayı güvenlik duvarından geçecek şekilde yapılandırın.

1. Azure portal giriş sayfasında **Tüm hizmetler**'e tıklayın.
2. **Ağ** bölümünde **Rota tabloları**'na tıklayın.
3. **Ekle**'ye tıklayın.
4. **Ad** alanına **Firewall-route** yazın.
5. **Abonelik** bölümünde aboneliğinizi seçin.
6. **Kaynak grubu** için **Var olanı kullan**’ı ve **Test-FW-RG** girişini seçin.
7. **Konum** alanında önceden kullandığınız konumu seçin.
8. **Oluştur**’a tıklayın.
9. **Yenile**'ye ve ardından **Firewall-route** rota tablosuna tıklayın.
10. **Alt ağlar** > **İlişkilendir**’e tıklayın.
11. **Sanal ağ** > **Test-FW-RG**’ye tıklayın.
12. **Alt ağ** bölümünde **Workload-SN** girişine tıklayın. Bu rota için yalnızca **Workload-SN** alt ağını seçtiğinizden emin olun. Aksi takdirde güvenlik duvarınız düzgün çalışmaz.

13. **Tamam** düğmesine tıklayın.
14. **Yollar** > **Ekle**’ye tıklayın.
15. **Rota adı** alanına **FW-DG** yazın.
16. **Adres ön eki** alanına **0.0.0.0/0** yazın.
17. **Sonraki atlama türü** için **Sanal gereç**'i seçin.

    Azure Güvenlik Duvarı, normalde yönetilen bir hizmettir ancak bu durumda sanal gereç kullanılabilir.
18. **Sonraki atlama adresi** alanına önceden not ettiğiniz güvenlik duvarı özel IP adresini yazın.
19. **Tamam** düğmesine tıklayın.

## <a name="configure-an-application-rule"></a>Uygulama kuralı yapılandırma

Bu MSN.com giden erişime izin veren uygulama kuralıdır.

1. **Test-FW-RG** öğesini açın ve **Test-FW01** güvenlik duvarına tıklayın.
2. **Test-FW01** sayfasının **Ayarlar** bölümünde **Kurallar**'a tıklayın.
3. Tıklayın **uygulama kuralı koleksiyonu** sekmesi.
4. **Uygulama kuralı koleksiyonu ekle**'ye tıklayın.
5. **Ad** alanına **App-Coll01** yazın.
6. **Öncelik** alanına **200** yazın.
7. **Eylem** alanında **İzin ver**'i seçin.
8. Altında **kuralları**, **hedef FQDN**, için **adı**, türü **AllowGH**.
9. **Kaynak Adresler** alanına **10.0.2.0/24** yazın.
10. **Protokol:bağlantı noktası** alanına **http, https** yazın.
11. İçin **hedef FQDN**, türü **msn.com**
12. **Ekle**'ye tıklayın.

Azure Güvenlik Duvarı'nda varsayılan olarak izin verilen altyapı FQDN'leri için yerleşik bir kural koleksiyonu bulunur. Bu FQDN'ler platforma özgüdür ve başka amaçlarla kullanılamaz. Daha fazla bilgi için bkz. [Altyapı FQDN'leri](infrastructure-fqdns.md).

## <a name="configure-a-network-rule"></a>Ağ kuralını yapılandırma

Bu, bağlantı noktası 53’deki (DNS) iki IP adresine giden erişime izin veren ağ kuralıdır.

1. Tıklayın **ağ kural koleksiyonu** sekmesi.
2. **Ağ kuralı koleksiyonu ekle**'ye tıklayın.
3. **Ad** alanına **Net-Coll01** yazın.
4. **Öncelik** alanına **200** yazın.
5. **Eylem** alanında **İzin ver**'i seçin.

6. **Kurallar** bölümünde **Ad** alanında **AllowDNS** yazın.
7. **Protokol** alanında **UDP**'yi seçin.
8. **Kaynak Adresler** alanına **10.0.2.0/24** yazın.
9. Hedef adres için **209.244.0.3,209.244.0.4** yazın.
10. **Hedef Bağlantı Noktaları** için **53** yazın.
11. **Ekle**'ye tıklayın.

### <a name="change-the-primary-and-secondary-dns-address-for-the-srv-work-network-interface"></a>**Srv-Work** ağ arabiriminin birincil ve ikincil DNS adresini değiştirme

Bu öğreticide birincil ve ikincil DNS adreslerini test amacıyla yapılandırıyorsunuz. Bu durum Azure Güvenlik Duvarı'nın genel gereksinimlerinden biri değildir.

1. Azure portaldan **Test-FW-RG** kaynak grubunu açın.
2. **Srv-Work** sanal makinesinin ağ arabirimine tıklayın.
3. **Ayarlar** bölümünde **DNS sunucuları**'na tıklayın.
4. **DNS sunucuları** bölümünde **Özel**'e tıklayın.
5. **DNS sunucusu ekle** metin kutusuna **209.244.0.3**, sonraki metin kutusuna da **209.244.0.4** yazın.
6. **Kaydet**’e tıklayın. 
7. **Srv-Work** sanal makinesini yeniden başlatın.

## <a name="test-the-firewall"></a>Güvenlik duvarını test etme

Şimdi beklendiği gibi çalıştığını doğrulamak için güvenlik duvarını test edin.

1. Azure portalda **Srv-Work** sanal makinesinin ağ ayarlarını gözden geçirin ve özel IP adresini not edin.
2. **Srv-Jump** sanal makinesine uzak masaüstü bağlantısı kurun ve oradan da **Srv-Work** özel IP adresine uzak masaüstü bağlantısı açın.

3. Internet Explorer'ı açın ve https://msn.com adresine gidin.
4. Güvenlik uyarılarında **Tamam** > **Kapat**'a tıklayın.

   MSN Giriş sayfasını görmeniz gerekir.

5. https://www.msn.com adresine gidin.

   Güvenlik duvarının engellemesi gerekir.

Güvenlik duvarı kurallarının çalıştığını doğruladığınıza göre:

- İzin verilen bir FQDN'ye göz atabilir ancak diğerlerine göz atamazsınız.
- Yapılandırılmış dış DNS sunucusunu kullanarak DNS adlarını çözümleyebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Güvenlik duvarı kaynaklarını bir sonraki öğretici için tutabilirsiniz veya artık gerekli değilse **Test-FW-RG** kaynak grubunu silerek güvenlik duvarıyla ilgili tüm kaynakları silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Azure güvenlik duvarı günlüklerini izleyin](./tutorial-diagnostics.md)
