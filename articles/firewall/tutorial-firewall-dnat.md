---
title: Azure portalını kullanarak Azure Güvenlik Duvarı DNAT ile gelen trafiği filtreleme
description: Bu öğreticide Azure portalını kullanarak Azure Güvenlik Duvarı DNAT’yi dağıtmayı ve yapılandırmayı öğreneceksiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.date: 9/25/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 766ad04251fbe404d43734115e41e23ae0a4be28
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46982061"
---
# <a name="tutorial-filter-inbound-traffic-with-azure-firewall-dnat-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak Azure Güvenlik Duvarı DNAT ile gelen trafiği filtreleme

Alt ağlarınıza gelen trafiği çevirmek ve filtrelemek için Azure Güvenlik Duvarı Hedef Ağ Adresi Çevirisi’ni (DNAT) yapılandırabilirsiniz. Azure Güvenlik Duvarı'nda gelen kuralları ve giden kuralları kavramı yoktur. Uygulama kuralları ve ağ kuralları vardır; bunlar güvenlik duvarına gelen tüm trafiğe uygulanır. Önce ağ kuralları, sonrasında uygulama kuralları uygulanır ve kurallar sonlandırıcıdır.

>[!NOTE]
>Güvenlik Duvarı DNAT özelliği şu anda yalnızca Azure PowerShell’de ve REST’de kullanılabilir.

Örneğin bir ağ kuralı eşleşirse paket uygulama kuralları tarafından değerlendirilmez. Ağ kuralı eşleşmesi yoksa ve paket protokolü HTTP/HTTPS ise bu durumda paket uygulama kuralları tarafından değerlendirilir. Hala eşleşme bulunamamışsa paket, [altyapı kural koleksiyonu](infrastructure-fqdns.md) ile değerlendirilir. Ardından hala eşleşme yoksa paket varsayılan olarak reddedilir.

DNAT’yi yapılandırdığınızda NAT kuralı koleksiyon eylemi **Hedef Ağ Adresi Çevirisi (DNAT)** olarak ayarlanır. Güvenlik duvarı genel IP’si ve bağlantı noktası özel IP adresine ve bağlantı noktasına çevrilir. Daha sonra kurallar, önce ağ kuralları, sonra da uygulama kuralları olmak üzere alışıldığı gibi uygulanır. Örneğin, TCP bağlantı noktası 3389 üzerinden Uzak Masaüstü trafiğine izin verecek şekilde bir ağ kuralı yapılandırmak isteyebilirsiniz. Önce adres çevirisi yapılır ve ardından çevrilen adresler kullanılarak ağ ve uygulama kuralları uygulanır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Test amaçlı ağ ortamı oluşturma
> * Güvenlik duvarı dağıtma
> * Varsayılan rota oluşturma
> * DNAT kuralını yapılandırma
> * Ağ kuralını yapılandırma
> * Güvenlik duvarını test etme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu öğretici için eşlenen iki sanal ağ oluşturuyorsunuz:
- **VN-Hub** - güvenlik duvarı bu sanal ağ içindedir.
- **VN-Spoke** - iş yükü sunucusu bu sanal ağ içindedir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
1. [http://portal.azure.com](http://portal.azure.com) adresinden Azure portalında oturum açın.
1. Azure portal giriş sayfasında **Kaynak grupları**'na ve ardından **Ekle**'ye tıklayın.
2. **Kaynak grubu adı** alanına **RG-DNAT-Test** yazın.
3. **Abonelik** bölümünde aboneliğinizi seçin.
4. **Kaynak grubu konumu** bölümünde bir konum seçin. Bundan sonra oluşturacağınız tüm kaynakların aynı konumda olması gerekir.
5. **Oluştur**’a tıklayın.

## <a name="set-up-the-network-environment"></a>Ağ ortamını oluşturma
Önce sanal ağları oluşturun, sonra da bunları eşleyin.

### <a name="create-the-hub-vnet"></a>Hub sanal ağını oluşturma
1. Azure portal giriş sayfasında **Tüm hizmetler**'e tıklayın.
2. **Ağ** bölümünde **Sanal ağlar**'a tıklayın.
3. **Ekle**'ye tıklayın.
4. **Ad** için **VN-Hub** yazın.
5. **Adres alanı** için **10.0.0.0/16** yazın.
7. **Abonelik** bölümünde aboneliğinizi seçin.
8. **Kaynak grubu** için **Var olanı kullan**’ı ve **RG-DNAT-Test** girişini seçin.
9. **Konum** alanında önceden kullandığınız konumu seçin.
10. **Alt ağ** bölümünde **Ad** alanına **AzureFirewallSubnet** yazın.

     Güvenlik duvarı bu alt ağda yer alacaktır ve alt ağ adının **mutlaka** AzureFirewallSubnet olması gerekir.
     > [!NOTE]
     > AzureFirewallSubnet için minimum boyut /25 olacaktır.
11. **Adres aralığı** için **10.0.1.0/24** yazın.
12. Diğer alanlar için varsayılan değerleri kullanın ve ardından **Oluştur**'a tıklayın.

### <a name="create-a-spoke-vnet"></a>Uç sanal ağını oluşturma

1. Azure portal giriş sayfasında **Tüm hizmetler**'e tıklayın.
2. **Ağ** bölümünde **Sanal ağlar**'a tıklayın.
3. **Ekle**'ye tıklayın.
4. **Ad** için **VN-Spoke** yazın.
5. **Adres alanı** için **192.168.0.0/16** yazın.
7. **Abonelik** bölümünde aboneliğinizi seçin.
8. **Kaynak grubu** için **Var olanı kullan**’ı ve **RG-DNAT-Test** girişini seçin.
9. **Konum** alanında önceden kullandığınız konumu seçin.
10. **Alt ağ** altında, **Ad** için **SN-Workload** yazın.

    Sunucu bu alt ağda yer alacaktır.
1. **Adres aralığı** için **192.168.1.0/24** yazın.
2. Diğer alanlar için varsayılan değerleri kullanın ve ardından **Oluştur**'a tıklayın.

### <a name="peer-the-vnets"></a>Sanal ağları eşleme

Şimdi iki sanal ağı eşleyin.

#### <a name="hub-to-spoke"></a>Merkezden uca

1. **VN-Hub** sanal ağına tıklayın.
2. **Ayarlar**'ın altında **Eşlemeler**'e tıklayın.
3. **Ekle**'ye tıklayın.
4. Ad olarak **Peer-HubSpoke** yazın.
5. Sanal ağ olarak **VN-Spoke**’u seçin.
7. **Tamam** düğmesine tıklayın.

#### <a name="spoke-to-hub"></a>Uçtan merkeze

1. **VN-Spoke** sanal ağına tıklayın.
2. **Ayarlar**'ın altında **Eşlemeler**'e tıklayın.
3. **Ekle**'ye tıklayın.
4. Ad olarak **Peer-SpokeHub** yazın.
5. Sanal ağ olarak **VN-Hub**’ı seçin.
6. **Yönlendirilen trafiğe izin ver**’e tıklayın.
7. **Tamam** düğmesine tıklayın.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

İş yükü sanal makinesi oluşturun ve bunu **SN-Workload** alt ağına yerleştirin.

1. Azure portal giriş sayfasında **Tüm hizmetler**'e tıklayın.
2. **İşlem** bölümünde **Sanal makineler**'e tıklayın.
3. **Ekle**, **Windows Server**, **Windows Server 2016 Datacenter** ve ardından **Oluştur**'a tıklayın.

**Temel Bilgiler**

1. **Ad** için **Srv-Workload** yazın.
5. Bir kullanıcı adı ve parola girin.
6. **Abonelik** bölümünde aboneliğinizi seçin.
7. **Kaynak grubu** için **Var olanı kullan**’a tıklayın ve **RG-DNAT-Test** girişini seçin.
8. **Konum** alanında önceden kullandığınız konumu seçin.
9. **Tamam** düğmesine tıklayın.

**Boyut**

1. Windows Server çalıştıran test amaçlı sanal makine için uygun bir boyut seçin. Örneğin: **B2ms** (8 GB RAM, 16 GB depolama alanı).
2. **Seç**'e tıklayın.

**Ayarlar**

1. **Ağ** bölümünde **Sanal ağ** için **VN-Spoke**'u seçin.
2. **Alt ağ** için **SN-Workload**'u seçin.
3. **Genel IP adresi**’ne ardından ve **Hiçbiri**’ne tıklayın.
4. **Ortak gelen bağlantı noktası seçin** alanında **Ortak gelen bağlantı noktası yok**’u seçin. 
2. Diğer varsayılan ayarları değiştirmeden **Tamam**'a tıklayın.

**Özet**

Özeti gözden geçirin ve **Oluştur**'a tıklayın. İşlemin tamamlanması birkaç dakika sürebilir.

Dağıtım bittikten sonra sanal makineyle ilişkili özel IP adresini not alın. Daha sonra güvenlik duvarını yapılandırdığınızda bu adres kullanılacaktır. Sanal makinenin adına tıklayın ve özel IP adresini bulmak için **Ayarlar**'ın altında **Ağ**’a tıklayın.


## <a name="deploy-the-firewall"></a>Güvenlik duvarını dağıtma

1. Portal giriş sayfasında **Kaynak oluştur**'a tıklayın.
2. **Ağ**'a tıklayın ve **Öne çıkanlar**'ın sonrasında **Tümünü gör**'e tıklayın.
3. **Güvenlik duvarı**'na ve ardından **Oluştur**'a tıklayın. 
4. **Güvenlik duvarı oluştur** sayfasında aşağıdaki ayarları kullanarak güvenlik duvarını yapılandırın:
   
   |Ayar  |Değer  |
   |---------|---------|
   |Adı     |FW-DNAT-test|
   |Abonelik     |\<aboneliğiniz\>|
   |Kaynak grubu     |**Var olanı kullan**: RG-DNAT-Test |
   |Konum     |Önceden kullandığınız konumu seçin|
   |Bir sanal ağ seçin     |**Var olanı kullan**: VN-Hub|
   |Genel IP adresi     |**Yeni oluşturun**. Genel IP adresinin türü Standart SKU olmalıdır.|

2. **Gözden geçir ve oluştur**’a tıklayın.
3. Özeti gözden geçirin ve **Oluştur**'a tıklayarak güvenlik duvarını oluşturun.

   Dağıtma işlemi birkaç dakika sürebilir.
4. Dağıtım tamamlandıktan sonra **RG-DNAT-Test** kaynak grubuna gidin ve **FW-DNAT-test** güvenlik duvarına tıklayın.
6. Özel IP adresini not edin. Varsayılan rotayı oluştururken bu adresi kullanacaksınız.


## <a name="create-a-default-route"></a>Varsayılan rota oluşturma

**SN-Workload** alt ağında varsayılan giden rotayı güvenlik duvarından geçecek şekilde yapılandıracaksınız.

1. Azure portal giriş sayfasında **Tüm hizmetler**'e tıklayın.
2. **Ağ** bölümünde **Rota tabloları**'na tıklayın.
3. **Ekle**'ye tıklayın.
4. **Ad** için **RT-FWroute** yazın.
5. **Abonelik** bölümünde aboneliğinizi seçin.
6. **Kaynak grubu** için **Var olanı kullan**’ı ve **RG-DNAT-Test** girişini seçin.
7. **Konum** alanında önceden kullandığınız konumu seçin.
8. **Oluştur**’a tıklayın.
9. **Yenile**'ye ve ardından **RT-FWroute** yol tablosuna tıklayın.
10. **Alt ağlar**’a ve ardından **İlişkilendir**’e tıklayın.
11. **Sanal ağlar**'a tıklayın ve ardından **VN-Spoke** girişini seçin.
12. **Alt ağ** için **SN-Workload** girişine tıklayın.
13. **Tamam** düğmesine tıklayın.
14. **Rotalar**'a ve ardından **Ekle**'ye tıklayın.
15. **Rota adı** alanına **FW-DG** yazın.
16. **Adres ön eki** alanına **0.0.0.0/0** yazın.
17. **Sonraki atlama türü** için **Sanal gereç**'i seçin.

    Azure Güvenlik Duvarı, normalde yönetilen bir hizmettir ancak bu durumda sanal gereç kullanılabilir.
1. **Sonraki atlama adresi** alanına önceden not ettiğiniz güvenlik duvarı özel IP adresini yazın.
2. **Tamam** düğmesine tıklayın.


## <a name="configure-a-dnat-rule"></a>DNAT kuralını yapılandırma

```azurepowershell-interactive
 $rgName  = "RG-DNAT-Test"
 $firewallName = "FW-DNAT-test"
 $publicip = type the Firewall public ip
 $newAddress = type the private IP address for the Srv-Workload virtual machine 
 
# Get Firewall
    $firewall = Get-AzureRmFirewall -ResourceGroupName $rgName -Name $firewallName
  # Create NAT rule
    $natRule = New-AzureRmFirewallNatRule -Name RL-01 -SourceAddress * -DestinationAddress $publicip -DestinationPort 3389 -Protocol TCP -TranslatedAddress $newAddress -TranslatedPort 3389
  # Create NAT rule collection
    $natRuleCollection = New-AzureRmFirewallNatRuleCollection -Name RC-DNAT-01 -Priority 200 -Rule $natRule
  # Add NAT Rule collection to firewall:
    $firewall.AddNatRuleCollection($natRuleCollection)
  # Save:
    $firewall | Set-AzureRmFirewall
```
## <a name="configure-a-network-rule"></a>Ağ kuralını yapılandırma

1. **RG-DNAT-Test**’i açın ve **FW-DNAT-test** güvenlik duvarına tıklayın.
1. **FW-DNAT-test** sayfasının **Ayarlar** bölümünde **Kurallar**'a tıklayın.
2. **Ağ kuralı koleksiyonu ekle**'ye tıklayın.

Aşağıdaki tabloyu kullanarak kuralı yapılandırın ve **Ekle**’ye tıklayın:


|Parametre  |Değer  |
|---------|---------|
|Adı     |**RC-Net-01**|
|Öncelik     |**200**|
|Eylem     |**İzin ver**|

**Kurallar** altında:

|Parametre  |Ayar  |
|---------|---------|
|Adı     |**RL-RDP**|
|Protokol     |**TCP**|
|Kaynak Adresler     |*|
|Hedef Adresler     |**Srv-Workload** özel IP adresi|
|Hedef bağlantı noktaları|**3389**|


## <a name="test-the-firewall"></a>Güvenlik duvarını test etme

1. Güvenlik duvarı genel IP adresine bir uzak masaüstü bağlayın. **Srv-Workload** sanal makinesine bağlı olmalısınız.
3. Uzak masaüstünü kapatın.
4. **RC-Net-01** ağ kuralı koleksiyon eylemini **Reddet** olarak değiştirin.
5. Güvenlik duvarı genel IP adresine yeniden bağlanmaya çalışın. Bu kez, **Reddetme** kuralı nedeniyle başarısız olması gerekir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Güvenlik duvarı kaynaklarını bir sonraki öğretici için tutabilirsiniz veya artık gerekli değilse **RG-DNAT-Test** kaynak grubunu silerek güvenlik duvarıyla ilgili tüm kaynakları silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Test amaçlı ağ ortamı oluşturma
> * Güvenlik duvarı dağıtma
> * Varsayılan rota oluşturma
> * DNAT kuralını yapılandırma
> * Ağ kuralını yapılandırma
> * Güvenlik duvarını test etme

Şimdi Azure Güvenlik Duvarı günlüklerini izleyebilirsiniz.

> [!div class="nextstepaction"]
> [Öğretici: Azure Güvenlik Duvarı günlüklerini izleme](./tutorial-diagnostics.md)
