---
title: "Öğretici: Azure portalı kullanarak Azure Güvenlik Duvarı'nı dağıtma ve yapılandırma"
description: Bu öğreticide Azure portalı kullanarak Azure Güvenlik Duvarı'nı dağıtmayı ve yapılandırmayı öğreneceksiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.date: 4/9/2019
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: cd7797ae3b79fb874bafc89437943b084020d800
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59492323"
---
# <a name="tutorial-deploy-and-configure-azure-firewall-using-the-azure-portal"></a>Öğretici: Azure portalı kullanarak Azure Güvenlik Duvarı'nı dağıtma ve yapılandırma

Giden ağ erişimini denetleme, genel ağ güvenlik planının önemli bir parçasıdır. Örneğin, web sitelerine erişimi sınırlamak isteyebilirsiniz. Veya giden IP adresleri ve erişilebilen bağlantı noktalarını sınırlamak isteyebilirsiniz.

Azure Güvenlik Duvarı, Azure alt ağından giden ağ erişimini denetlemenin bir yoludur. Azure Güvenlik Duvarı ile şunları yapılandırabilirsiniz:

* Bir alt ağdan erişilebilen tam etki alanı adlarını (FQDN) tanımlayan uygulama kuralları.
* Kaynak adres, protokol, hedef bağlantı noktası ve hedef adresini tanımlayan ağ kuralları.

Ağ trafiğinizi güvenlik duvarından alt ağın varsayılan ağ geçidi olarak yönlendirdiğinizde ağ trafiği yapılandırılan güvenlik duvarı kurallarına tabi tutulur.

Bu öğreticide kolay dağıtım için üç alt ağa sahip tek bir basitleştirilmiş sanal ağ oluşturursunuz. Üretim dağıtımları için bir [merkez ve ışınsal modelini](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) , Güvenlik Duvarı'nı kendi Vnet'ine olduğu önerilir. Bir veya daha fazla alt ağ ile aynı bölgedeki eşlenmiş Vnet'ler içindeki iş yükü sunucularıdır.

* **AzureFirewallSubnet** - güvenlik duvarı bu alt ağdadır.
* **Workload-SN**: İş yükü sunucusu bu alt ağda yer alır. Bu alt ağın ağ trafiği güvenlik duvarından geçer.
* **Jump-SN**: "Atlama" sunucusu bu alt ağda yer alır. Atlama sunucusu, Uzak Masaüstü ile bağlanabileceğiniz genel IP adresine sahiptir. Buradan iş yükü sunucusuna bağlanabilirsiniz (başka bir Uzak Masaüstü oturumuyla).

![Öğretici ağı altyapısı](media/tutorial-firewall-rules-portal/Tutorial_network.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Test amaçlı ağ ortamı oluşturma
> * Güvenlik duvarı dağıtma
> * Varsayılan rota oluşturma
> * Www.google.com erişmesine izin vermek için bir uygulama kuralı yapılandırma
> * Dış DNS sunucularına erişime izin vermek için ağ kuralı yapılandırma
> * Güvenlik duvarını test etme

Tercih ederseniz, bu öğreticiyi [Azure PowerShell](deploy-ps.md) kullanarak tamamlayabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="set-up-the-network"></a>Ağı ayarlama

İlk olarak güvenlik duvarını dağıtmak için gerekli olan kaynakları içerecek bir kaynak grubu oluşturun. Ardından sanal ağı, alt ağları ve test sunucularını oluşturun.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, bu öğreticideki tüm kaynakları içerir.

1. [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.
2. Azure portal giriş sayfasında, seçin **kaynak grupları** > **Ekle**.
3. **Kaynak grubu adı** alanına **Test-FW-RG** yazın.
4. **Abonelik** bölümünde aboneliğinizi seçin.
5. **Kaynak grubu konumu** bölümünde bir konum seçin. Bundan sonra oluşturacağınız tüm kaynakların aynı konumda olması gerekir.
6. **Oluştur**’u seçin.

### <a name="create-a-vnet"></a>Sanal ağ oluşturma

Bu sanal ağda üç alt ağ bulunacaktır.

1. Azure portalı giriş sayfasından seçin **kaynak Oluştur**.
2. Altında **ağ**seçin **sanal ağ**.
4. **Ad** alanına **Test-FW-VN** yazın.
5. **Adres alanı** için **10.0.0.0/16** yazın.
6. **Abonelik** bölümünde aboneliğinizi seçin.
7. İçin **kaynak grubu**seçin **Test FW RG**.
8. **Konum** alanında önceden kullandığınız konumu seçin.
9. **Alt ağ** bölümünde **Ad** alanına **AzureFirewallSubnet** yazın. Güvenlik duvarı bu alt ağda yer alacaktır ve alt ağ adının **mutlaka** AzureFirewallSubnet olması gerekir.
10. **Adres aralığı** için **10.0.1.0/24** yazın.
11. Diğer varsayılan ayarları kabul edin ve ardından **Oluştur**.

> [!NOTE]
> En düşük AzureFirewallSubnet alt /26 boyutudur.

### <a name="create-additional-subnets"></a>Ek alt ağ oluşturma

Bir sonraki adımda atlama sunucusu için alt ağlar ve iş yükü sunucuları için bir alt ağ oluşturun.

1. Azure portal giriş sayfasında, seçin **kaynak grupları** > **Test FW RG**.
2. Seçin **Test FW VN** sanal ağ.
3. Seçin **alt ağlar** > **+ alt ağ**.
4. **Ad** alanına **Workload-SN** yazın.
5. **Adres aralığı** için **10.0.2.0/24** yazın.
6. **Tamam**’ı seçin.

**Jump-SN** adına ve **10.0.3.0/24** adres aralığına sahip bir alt ağ daha oluşturun.

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Şimdi atlama ve iş yükü sanal makinelerini oluşturup uygun alt ağlara yerleştirin.

1. Azure Portal'da seçin **kaynak Oluştur**.
2. **İşlem**’i ve sonra Öne Çıkanlar listesinde **Windows Server 2016 Datacenter**’ı seçin.
3. Sanal makine için şu değerleri girin:

   |Ayar  |Değer  |
   |---------|---------|
   |Kaynak grubu     |**Test FW RG**|
   |Sanal makine adı     |**SRV atlama**|
   |Bölge     |Önceki ile aynı|
   |Yönetici kullanıcı adı     |**azureuser**|
   |Parola     |**Azure123456!**|

4. Altında **gelen bağlantı noktası kuralları**, için **ortak gelen bağlantı noktası**seçin **Seçili bağlantı noktalarına izin**.
5. İçin **seçin gelen bağlantı noktalarının**seçin **RDP (3389)**.

6. Diğer Varsayılanları ve select kabul **sonraki: Diskleri**.
7. Seçin ve disk Varsayılanları kabul **sonraki: Ağ**.
8. Emin olun **Test FW VN** sanal ağı ve alt ağ için seçili olduğu **atlama SN**.
9. İçin **genel IP**, varsayılan yeni ortak IP adresi adı (Srv-atlamayı-ip) kabul edin.
11. Diğer Varsayılanları ve select kabul **sonraki: Yönetim**.
12. Seçin **kapalı** önyükleme tanılaması devre dışı bırakmak için. Diğer Varsayılanları ve select kabul **gözden geçir + Oluştur**.
13. Özet sayfasında ayarları gözden geçirin ve ardından **Oluştur**.

Adlı başka bir sanal makineyi yapılandırmak için aşağıdaki tablodaki bilgileri kullanın **Srv iş**. Yapılandırmanın geri kalanı Srv-Jump sanal makinesiyle aynıdır.

|Ayar  |Değer  |
|---------|---------|
|Alt ağ|**Workload-SN**|
|Genel IP|**None**|
|Ortak gelen bağlantı noktası|**None**|

## <a name="deploy-the-firewall"></a>Güvenlik duvarını dağıtma

Güvenlik duvarını sanal ağa dağıtın.

1. Portal giriş sayfasından seçin **kaynak Oluştur**.
2. Tür **Güvenlik Duvarı** tuşuna basın ve arama kutusuna **Enter**.
3. Seçin **Güvenlik Duvarı** seçip **Oluştur**.
4. **Güvenlik duvarı oluştur** sayfasında aşağıdaki ayarları kullanarak güvenlik duvarını yapılandırın:

   |Ayar  |Değer  |
   |---------|---------|
   |Abonelik     |\<aboneliğiniz\>|
   |Kaynak grubu     |**Test FW RG** |
   |Ad     |**Test-FW01**|
   |Konum     |Önceden kullandığınız konumu seçin|
   |Bir sanal ağ seçin     |**Var olanı kullan**: **Test FW VN**|
   |Genel IP adresi     |**Yeni oluşturun**. Genel IP adresinin türü Standart SKU olmalıdır.|

5. **İncele ve oluştur**’u seçin.
6. Özeti gözden geçirin ve ardından **Oluştur** güvenlik duvarı oluşturun.

   Dağıtma işlemi birkaç dakika sürebilir.
7. Dağıtım tamamlandıktan sonra Git **Test FW RG** kaynak grubu ve select **Test FW01** güvenlik duvarı.
8. Özel IP adresini not edin. Varsayılan rotayı oluştururken bu adresi kullanacaksınız.

## <a name="create-a-default-route"></a>Varsayılan rota oluşturma

**Workload-SN** alt ağında varsayılan giden rotayı güvenlik duvarından geçecek şekilde yapılandırın.

1. Azure portalı giriş sayfasından seçin **tüm hizmetleri**.
2. Altında **ağ**seçin **rota tabloları**.
3. **Add (Ekle)** seçeneğini belirleyin.
4. **Ad** alanına **Firewall-route** yazın.
5. **Abonelik** bölümünde aboneliğinizi seçin.
6. İçin **kaynak grubu**seçin **Test FW RG**.
7. **Konum** alanında önceden kullandığınız konumu seçin.
8. **Oluştur**’u seçin.
9. Seçin **Yenile**ve ardından **güvenlik duvarı rota** yol tablosu.
10. Seçin **alt ağlar** seçip **ilişkilendirmek**.
11. Seçin **sanal ağ** > **Test FW VN**.
12. İçin **alt**seçin **iş yükü SN**. Yalnızca seçtiğinizden emin olun **iş yükü SN** bu rota için alt ağ, aksi takdirde, güvenlik duvarınızı çalışmaz doğru.

13. **Tamam**’ı seçin.
14. Seçin **yollar** seçip **Ekle**.
15. İçin **rota adı**, türü **fw dg**.
16. **Adres ön eki** alanına **0.0.0.0/0** yazın.
17. **Sonraki atlama türü** için **Sanal gereç**'i seçin.

    Azure Güvenlik Duvarı, normalde yönetilen bir hizmettir ancak bu durumda sanal gereç kullanılabilir.
18. **Sonraki atlama adresi** alanına önceden not ettiğiniz güvenlik duvarı özel IP adresini yazın.
19. **Tamam**’ı seçin.

## <a name="configure-an-application-rule"></a>Uygulama kuralı yapılandırma

Giden erişime izin www.google.com uygulama kuralıdır.

1. Açık **Test FW RG**seçip **Test FW01** güvenlik duvarı.
2. Üzerinde **Test FW01** sayfasındaki **ayarları**seçin **kuralları**.
3. Seçin **uygulama kuralı koleksiyonu** sekmesi.
4. Seçin **uygulama kuralı koleksiyonu ekleme**.
5. **Ad** alanına **App-Coll01** yazın.
6. **Öncelik** alanına **200** yazın.
7. **Eylem** alanında **İzin ver**'i seçin.
8. Altında **kuralları**, **hedef FQDN**, için **adı**, türü **izin ver-Google**.
9. **Kaynak Adresler** alanına **10.0.2.0/24** yazın.
10. **Protokol:bağlantı noktası** alanına **http, https** yazın.
11. İçin **hedef FQDN**, türü **www.google.com**
12. **Add (Ekle)** seçeneğini belirleyin.

Azure Güvenlik Duvarı'nda varsayılan olarak izin verilen altyapı FQDN'leri için yerleşik bir kural koleksiyonu bulunur. Bu FQDN'ler platforma özgüdür ve başka amaçlarla kullanılamaz. Daha fazla bilgi için bkz. [Altyapı FQDN'leri](infrastructure-fqdns.md).

## <a name="configure-a-network-rule"></a>Ağ kuralını yapılandırma

Bu, bağlantı noktası 53’deki (DNS) iki IP adresine giden erişime izin veren ağ kuralıdır.

1. Seçin **ağ kural koleksiyonu** sekmesi.
2. Seçin **ağ kural koleksiyonu ekleme**.
3. **Ad** alanına **Net-Coll01** yazın.
4. **Öncelik** alanına **200** yazın.
5. **Eylem** alanında **İzin ver**'i seçin.

6. Altında **kuralları**, için **adı**, türü **izin ver-DNS**.
7. **Protokol** alanında **UDP**'yi seçin.
8. **Kaynak Adresler** alanına **10.0.2.0/24** yazın.
9. Hedef adres için **209.244.0.3,209.244.0.4** yazın.
10. **Hedef Bağlantı Noktaları** için **53** yazın.
11. **Add (Ekle)** seçeneğini belirleyin.

### <a name="change-the-primary-and-secondary-dns-address-for-the-srv-work-network-interface"></a>**Srv-Work** ağ arabiriminin birincil ve ikincil DNS adresini değiştirme

Bu öğreticide test amacıyla, sunucunun birincil ve ikincil DNS adreslerini yapılandırın. Bu genel bir Azure güvenlik duvarı gereksinim değildir.

1. Azure portaldan **Test-FW-RG** kaynak grubunu açın.
2. Ağ arabirimi seçin **Srv iş** sanal makine.
3. Altında **ayarları**seçin **DNS sunucuları**.
4. Altında **DNS sunucuları**seçin **özel**.
5. **DNS sunucusu ekle** metin kutusuna **209.244.0.3**, sonraki metin kutusuna da **209.244.0.4** yazın.
6. **Kaydet**’i seçin.
7. **Srv-Work** sanal makinesini yeniden başlatın.

## <a name="test-the-firewall"></a>Güvenlik duvarını test etme

Şimdi beklendiği gibi çalıştığını doğrulamak için Güvenlik Duvarı'nı test edin.

1. Azure portalda **Srv-Work** sanal makinesinin ağ ayarlarını gözden geçirin ve özel IP adresini not edin.
2. Bağlanmak için Uzak Masaüstü **Srv atlama** sanal makine ve oturum açın. Burada, bir Uzak Masaüstü Bağlantısı açın **Srv iş** özel IP adresi.

3. Internet Explorer'ı açın ve http://www.google.com adresine gidin.
4. Seçin **Tamam** > **Kapat** üzerinde Internet Explorer güvenlik uyarıları.

   Google'nın ana sayfası görmeniz gerekir.

5. http://www.microsoft.com adresine gidin.

   Güvenlik duvarının engellemesi gerekir.

Şimdi güvenlik duvarı kuralları çalıştığını doğruladığınıza göre:

* İzin verilen bir FQDN'ye göz atabilir ancak diğerlerine göz atamazsınız.
* Yapılandırılmış dış DNS sunucusunu kullanarak DNS adlarını çözümleyebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Güvenlik duvarı kaynaklarını bir sonraki öğretici için tutabilirsiniz veya artık gerekli değilse **Test-FW-RG** kaynak grubunu silerek güvenlik duvarıyla ilgili tüm kaynakları silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Azure güvenlik duvarı günlüklerini izleyin](./tutorial-diagnostics.md)
