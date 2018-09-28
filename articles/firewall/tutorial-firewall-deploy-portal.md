---
title: Azure portalı kullanarak Azure Güvenlik Duvarı'nı dağıtma ve yapılandırma
description: Bu öğreticide Azure portalı kullanarak Azure Güvenlik Duvarı'nı dağıtmayı ve yapılandırmayı öğreneceksiniz.
services: firewall
author: vhorne
manager: jpconnock
ms.service: firewall
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 727d38cae6c2f98d2922d5760f116ab85d75b8ac
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46983523"
---
# <a name="tutorial-deploy-and-configure-azure-firewall-using-the-azure-portal"></a>Öğretici: Azure portalı kullanarak Azure Güvenlik Duvarı'nı dağıtma ve yapılandırma

Azure Güvenlik Duvarı'nda giden erişimi denetlemek için iki kural türü bulunur:

- **Uygulama kuralları**

   Bir alt ağdan erişilebilen tam etki alanı adları (FQDN) yapılandırmanızı sağlar. Örneğin alt ağınızdan *github.com* için erişim izni verebilirsiniz.
- **Ağ kuralları**

   Kaynak adresi, protokol, hedef bağlantı noktası ve hedef adres bilgilerine sahip kurallar yapılandırmanızı sağlar. Örneğin alt ağınızdan DNS sunucunuzun IP adresinin 53 (DNS) numaralı bağlantı noktasına giden trafiğe izin verecek bir kural oluşturabilirsiniz.

Ağ trafiğinizi güvenlik duvarından alt ağın varsayılan ağ geçidi olarak yönlendirdiğinizde ağ trafiği yapılandırılan güvenlik duvarı kurallarına tabi tutulur.

Uygulama ve ağ kuralları, *kural koleksiyonları* halinde depolanır. Kural koleksiyonu, aynı eylemi ve önceliği paylaşan kuralların listesidir.  Ağ kuralı koleksiyonu ağ kurallarından, uygulama kuralı koleksiyonu ise uygulama kurallarından oluşan bir listedir.

Azure Güvenlik Duvarında gelen kuralları ve giden kuralları kavramı yoktur. Uygulama kuralları ve ağ kuralları mevcuttur, bunlar güvenlik duvarına gelen tüm trafiğe uygulanır. Önce ağ kuralları, sonrasında uygulama kuralları uygulanır ve kurallar sonlandırıcıdır.

Örneğin bir ağ kuralı eşleşirse paket uygulama kuralları tarafından değerlendirilmez. Ağ kuralı eşleşmesi yoksa ve paket protokolü HTTP/HTTPS ise paket ardından uygulama kuralları tarafından değerlendirilir. Hala eşleşme bulunamamışsa paket, altyapı kural koleksiyonu ile değerlendirilir. Ardından hala eşleşme yoksa paket varsayılan olarak reddedilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Test amaçlı ağ ortamı oluşturma
> * Güvenlik duvarı dağıtma
> * Varsayılan rota oluşturma
> * Uygulama kurallarını yapılandırma
> * Ağ kurallarını yapılandırma
> * Güvenlik duvarını test etme



Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu öğreticide üç alt ağa sahip tek bir sanal ağ oluşturmanız gerekir:
- **FW-SN**: Güvenlik duvarı bu alt ağda yer alır.
- **Workload-SN**: İş yükü sunucusu bu alt ağda yer alır. Bu alt ağın ağ trafiği güvenlik duvarından geçer.
- **Jump-SN**: "Atlama" sunucusu bu alt ağda yer alır. Atlama sunucusu, Uzak Masaüstü ile bağlanabileceğiniz genel IP adresine sahiptir. Buradan iş yükü sunucusuna bağlanabilirsiniz (başka bir Uzak Masaüstü oturumuyla).

![Öğretici ağı altyapısı](media/tutorial-firewall-rules-portal/Tutorial_network.png)

Bu öğreticide kolay dağıtım için basitleştirilmiş bir ağ yapılandırılması kullanılır. Üretim dağıtımları için [hub ve bağlı bileşen modeli](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) önerilir ve bu yapıda güvenlik duvarı kendi sanal ağında, iş yükü sunucuları ise bir veya daha fazla alt ağ ile aynı bölgedeki eş sanal ağlarda bulunur.



## <a name="set-up-the-network-environment"></a>Ağ ortamını oluşturma
İlk olarak güvenlik duvarını dağıtmak için gerekli olan kaynakları içerecek bir kaynak grubu oluşturun. Ardından sanal ağı, alt ağları ve test sunucularını oluşturun.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
1. [http://portal.azure.com](http://portal.azure.com) adresinden Azure portalında oturum açın.
1. Azure portal giriş sayfasında **Kaynak grupları**'na ve ardından **Ekle**'ye tıklayın.
2. **Kaynak grubu adı** alanına **Test-FW-RG** yazın.
3. **Abonelik** bölümünde aboneliğinizi seçin.
4. **Kaynak grubu konumu** bölümünde bir konum seçin. Bundan sonra oluşturacağınız tüm kaynakların aynı konumda olması gerekir.
5. **Oluştur**’a tıklayın.


### <a name="create-a-vnet"></a>Sanal ağ oluşturma
1. Azure portal giriş sayfasında **Tüm hizmetler**'e tıklayın.
2. **Ağ** bölümünde **Sanal ağlar**'a tıklayın.
3. **Ekle**'ye tıklayın.
4. **Ad** alanına **Test-FW-VN** yazın.
5. **Adres alanı** için **10.0.0.0/16** yazın.
7. **Abonelik** bölümünde aboneliğinizi seçin.
8. **Kaynak grubu** için **Var olanı kullan**’ı ve **Test-FW-RG** girişini seçin.
9. **Konum** alanında önceden kullandığınız konumu seçin.
10. **Alt ağ** bölümünde **Ad** alanına **AzureFirewallSubnet** yazın. Güvenlik duvarı bu alt ağda yer alacaktır ve alt ağ adının **mutlaka** AzureFirewallSubnet olması gerekir.
11. **Adres aralığı** için **10.0.1.0/24** yazın.
12. Diğer alanlar için varsayılan değerleri kullanın ve ardından **Oluştur**'a tıklayın.

> [!NOTE]
> AzureFirewallSubnet için minimum boyut /25 olacaktır.

### <a name="create-additional-subnets"></a>Ek alt ağ oluşturma

Bir sonraki adımda atlama sunucusu için alt ağlar ve iş yükü sunucuları için bir alt ağ oluşturun.

1. Azure portal giriş sayfasında **Kaynak grupları**'na ve ardından **Test-FW-RG** girişine tıklayın.
2. **Test-FW-VN** sanal ağına tıklayın.
3. **Alt ağlar**’a ve ardından **+Alt ağ**’a tıklayın.
4. **Ad** alanına **Workload-SN** yazın.
5. **Adres aralığı** için **10.0.2.0/24** yazın.
6. **Tamam** düğmesine tıklayın.

**Jump-SN** adına ve **10.0.3.0/24** adres aralığına sahip bir alt ağ daha oluşturun.

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Şimdi atlama ve iş yükü sanal makinelerini oluşturup uygun alt ağlara yerleştirin.

1. Azure portal giriş sayfasında **Tüm hizmetler**'e tıklayın.
2. **İşlem** bölümünde **Sanal makineler**'e tıklayın.
3. **Ekle**, **Windows Server**, **Windows Server 2016 Datacenter** ve ardından **Oluştur**'a tıklayın.

**Temel Bilgiler**

1. **Ad** alanına **Srv-Jump** yazın.
5. Bir kullanıcı adı ve parola girin.
6. **Abonelik** bölümünde aboneliğinizi seçin.
7. **Kaynak grubu** için **Var olanı kullan**’a tıklayın ve **Test-FW-RG** girişini seçin.
8. **Konum** alanında önceden kullandığınız konumu seçin.
9. **Tamam** düğmesine tıklayın.

**Boyut**

1. Windows Server çalıştıran test amaçlı sanal makine için uygun bir boyut seçin. Örneğin: **B2ms** (8 GB RAM, 16 GB depolama alanı).
2. **Seç**'e tıklayın.

**Ayarlar**

1. **Ağ** bölümünde **Sanal ağ** için **Test-FW-VN** öğesini seçin.
2. **Alt ağ** için **Jump-SN** öğesini seçin.
3. **Ortak gelen bağlantı noktası seçin** bölümünde **RDP (3389)** girişini seçin. 

    Genel IP adresinize erişimi sınırlamanız ancak atlama sunucusuna uzak masaüstü bağlantısı kurabilmek için 3389 numaralı bağlantı noktasını açmanız gerekir. 
2. Diğer varsayılan ayarları değiştirmeden **Tamam**'a tıklayın.

**Özet**

Özeti gözden geçirin ve **Oluştur**'a tıklayın. İşlemin tamamlanması birkaç dakika sürebilir.

Bu işlemleri tekrarlayarak **Srv-Work** adlı başka bir sanal makine oluşturun.

Srv-Work sanal makinesinin **Ayarlar** sayfasını yapılandırmak için aşağıdaki tabloda yer alan bilgileri kullanın. Yapılandırmanın geri kalanı Srv-Jump sanal makinesiyle aynıdır.


|Ayar  |Değer  |
|---------|---------|
|Alt ağ|Workload-SN|
|Genel IP adresi|None|
|Ortak gelen bağlantı noktası seçin|Ortak gelen bağlantı noktası yok|


## <a name="deploy-the-firewall"></a>Güvenlik duvarını dağıtma

1. Portal giriş sayfasında **Kaynak oluştur**'a tıklayın.
2. **Ağ**'a tıklayın ve **Öne çıkanlar**'ın sonrasında **Tümünü gör**'e tıklayın.
3. **Güvenlik duvarı**'na ve ardından **Oluştur**'a tıklayın. 
4. **Güvenlik duvarı oluştur** sayfasında aşağıdaki ayarları kullanarak güvenlik duvarını yapılandırın:
   
   |Ayar  |Değer  |
   |---------|---------|
   |Adı     |Test-FW01|
   |Abonelik     |\<aboneliğiniz\>|
   |Kaynak grubu     |**Var olanı kullan**: Test-FW-RG |
   |Konum     |Önceden kullandığınız konumu seçin|
   |Bir sanal ağ seçin     |**Var olanı kullan**: Test-FW-VN|
   |Genel IP adresi     |**Yeni oluşturun**. Genel IP adresinin türü Standart SKU olmalıdır.|

2. **Gözden geçir ve oluştur**’a tıklayın.
3. Özeti gözden geçirin ve **Oluştur**'a tıklayarak güvenlik duvarını oluşturun.

   Dağıtma işlemi birkaç dakika sürebilir.
4. Dağıtım tamamlandıktan sonra **Test-FW-RG** kaynak grubuna gidin ve **Test-FW01** güvenlik duvarına tıklayın.
6. Özel IP adresini not edin. Varsayılan rotayı oluştururken bu adresi kullanacaksınız.


## <a name="create-a-default-route"></a>Varsayılan rota oluşturma

**Workload-SN** alt ağında varsayılan giden rotayı güvenlik duvarından geçecek şekilde yapılandıracaksınız.

1. Azure portal giriş sayfasında **Tüm hizmetler**'e tıklayın.
2. **Ağ** bölümünde **Rota tabloları**'na tıklayın.
3. **Ekle**'ye tıklayın.
4. **Ad** alanına **Firewall-route** yazın.
5. **Abonelik** bölümünde aboneliğinizi seçin.
6. **Kaynak grubu** için **Var olanı kullan**’ı ve **Test-FW-RG** girişini seçin.
7. **Konum** alanında önceden kullandığınız konumu seçin.
8. **Oluştur**’a tıklayın.
9. **Yenile**'ye ve ardından **Firewall-route** rota tablosuna tıklayın.
10. **Alt ağlar**’a ve ardından **İlişkilendir**’e tıklayın.
11. **Sanal ağlar**'a tıklayın ve ardından **Test-FW-VN** girişini seçin.
12. **Alt ağ** bölümünde **Workload-SN** girişine tıklayın.
13. **Tamam** düğmesine tıklayın.
14. **Rotalar**'a ve ardından **Ekle**'ye tıklayın.
15. **Rota adı** alanına **FW-DG** yazın.
16. **Adres ön eki** alanına **0.0.0.0/0** yazın.
17. **Sonraki atlama türü** için **Sanal gereç**'i seçin.

    Azure Güvenlik Duvarı, normalde yönetilen bir hizmettir ancak bu durumda sanal gereç kullanılabilir.
1. **Sonraki atlama adresi** alanına önceden not ettiğiniz güvenlik duvarı özel IP adresini yazın.
2. **Tamam** düğmesine tıklayın.


## <a name="configure-application-rules"></a>Uygulama kurallarını yapılandırma


1. **Test-FW-RG** öğesini açın ve **Test-FW01** güvenlik duvarına tıklayın.
2. **Test-FW01** sayfasının **Ayarlar** bölümünde **Kurallar**'a tıklayın.
3. **Uygulama kuralı koleksiyonu ekle**'ye tıklayın.
4. **Ad** alanına **App-Coll01** yazın.
5. **Öncelik** alanına **200** yazın.
6. **Eylem** alanında **İzin ver**'i seçin.
7. **Kurallar** bölümünde **Ad** alanında **AllowGH** yazın.
8. **Kaynak Adresler** alanına **10.0.2.0/24** yazın.
9. **Protokol:bağlantı noktası** alanına **http, https** yazın. 
10. **Hedef FQDNS** alanına **github.com** yazın.
11. **Ekle**'ye tıklayın.

Azure Güvenlik Duvarı'nda varsayılan olarak izin verilen altyapı FQDN'leri için yerleşik bir kural koleksiyonu bulunur. Bu FQDN'ler platforma özgüdür ve başka amaçlarla kullanılamaz. Daha fazla bilgi için bkz. [Altyapı FQDN'leri](infrastructure-fqdns.md).

> [!Note]
> FQDN etiketleri şu anda yalnızca Azure PowerShell ve REST ile yapılandırılabilir. Daha fazla bilgi için [buraya](https://aka.ms/firewallapplicationrule) tıklayın. 

## <a name="configure-network-rules"></a>Ağ kurallarını yapılandırma

1. **Ağ kuralı koleksiyonu ekle**'ye tıklayın.
2. **Ad** alanına **Net-Coll01** yazın.
3. **Öncelik** alanına **200** yazın.
4. **Eylem** alanında **İzin ver**'i seçin.

6. **Kurallar** bölümünde **Ad** alanında **AllowDNS** yazın.
8. **Protokol** alanında **UDP**'yi seçin.
9. **Kaynak Adresler** alanına **10.0.2.0/24** yazın.
10. Hedef adres için **209.244.0.3,209.244.0.4** yazın.
11. **Hedef Bağlantı Noktaları** için **53** yazın.
12. **Ekle**'ye tıklayın.

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

1. Azure portalda **Srv-Work** sanal makinesinin ağ ayarlarını gözden geçirin ve özel IP adresini not edin.
2. **Srv-Jump** sanal makinesine uzak masaüstü bağlantısı kurun ve oradan da **Srv-Work** özel IP adresine uzak masaüstü bağlantısı açın.

5. Internet Explorer'ı açın ve http://github.com adresine gidin.
6. Güvenlik uyarılarında **Tamam**'a ve **Kapat**'a tıklayın.

   GitHub giriş sayfasını görmeniz gerekir.

7. http://www.msn.com adresine gidin.

   Güvenlik duvarının engellemesi gerekir.

Güvenlik duvarı kurallarının çalıştığını doğruladığınıza göre:

- İzin verilen bir FQDN'ye göz atabilir ancak diğerlerine göz atamazsınız.
- Yapılandırılmış dış DNS sunucusunu kullanarak DNS adlarını çözümleyebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Güvenlik duvarı kaynaklarını bir sonraki öğretici için tutabilirsiniz veya artık gerekli değilse **Test-FW-RG** kaynak grubunu silerek güvenlik duvarıyla ilgili tüm kaynakları silebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Ağı ayarlama
> * Güvenlik duvarı oluşturma
> * Varsayılan rota oluşturma
> * Uygulama ve ağ güvenlik duvarı kurallarını yapılandırma
> * Güvenlik duvarını test etme

Şimdi Azure Güvenlik Duvarı günlüklerini izleyebilirsiniz.

> [!div class="nextstepaction"]
> [Öğretici: Azure Güvenlik Duvarı günlüklerini izleme](./tutorial-diagnostics.md)
