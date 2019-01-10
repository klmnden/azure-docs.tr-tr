---
title: Bir web uygulaması güvenlik duvarıyla - Azure portalında bir uygulama ağ geçidi oluşturma | Microsoft Docs
description: Azure portalını kullanarak bir web uygulaması güvenlik duvarı ile bir uygulama ağ geçidi oluşturmayı öğrenin.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/26/2018
ms.author: victorh
ms.openlocfilehash: b368ef3b5503d90b0eb928113e8154aabca9c04a
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2019
ms.locfileid: "54157148"
---
# <a name="create-an-application-gateway-with-a-web-application-firewall-using-the-azure-portal"></a>Azure portalını kullanarak bir web uygulaması güvenlik duvarı ile bir uygulama ağ geçidi oluşturma

Azure portalından oluşturmak için kullanabileceğiniz bir [uygulama ağ geçidi](application-gateway-introduction.md) ile bir [web uygulaması güvenlik duvarı](application-gateway-web-application-firewall-overview.md) (WAF). WAF, uygulamanızı korumak için [OWASP](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) kurallarını kullanır. Bu kurallar SQL ekleme, siteler arası betik saldırıları ve oturum ele geçirme gibi saldırılara karşı korumayı içerir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * WAF etkinken bir uygulama ağ geçidi oluşturma
> * Arka uç sunucular olarak kullanılan sanal makine oluşturma
> * Bir depolama hesabı oluşturma ve tanılamaları yapılandırma

![Web uygulaması güvenlik duvarı örneği](./media/application-gateway-web-application-firewall-portal/scenario-waf.png)

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Bir sanal ağ, oluşturduğunuz kaynakları arasındaki iletişim için gereklidir. Bu örnekte iki alt ağ oluşturulmuştur: biri uygulama ağ geçidi ve diğeri de arka uç sunucuları içindir. Uygulama ağ geçidini oluştururken aynı zamanda bir sanal makine oluşturabilirsiniz.

1. Tıklayın **yeni** Azure portalının sol üst köşedeki üzerinde bulunamadı.
2. **Ağ** ve ardından Öne Çıkanlar listesinde **Application Gateway**’i seçin.
3. Uygulama ağ geçidi için şu değerleri girin:

    - *myAppGateway* - Uygulama ağ geçidinin adı.
    - *myResourceGroupAG* - Yeni kaynak grubu.
    - Seçin *WAF* uygulama ağ geçidinin katmanı için.

    ![Yeni uygulama ağ geçidi oluşturma](./media/application-gateway-web-application-firewall-portal/application-gateway-create.png)

4. Diğer ayarların varsayılan değerlerini kabul edin ve sonra **Tamam**’a tıklayın.
5. Tıklayın **bir sanal ağ seçin**, tıklayın **Yeni Oluştur**ve ardından sanal ağ için şu değerleri girin:

    - *myVNet* - Sanal ağın adı.
    - *10.0.0.0/16* - Sanal ağın adres alanı.
    - *myAGSubnet* - Alt ağın adı.
    - *10.0.0.0/24* - Alt ağın adres alanı.

    ![Sanal ağ oluşturma](./media/application-gateway-web-application-firewall-portal/application-gateway-vnet.png)

6. Sanal ağı ve alt ağı oluşturmak için **Tamam**’a tıklayın.
7. Tıklayın **genel bir IP adresi seçin**, tıklayın **Yeni Oluştur**ve ardından genel IP adresini adını girin. Bu örnekte genel IP adresinin adı *myAGPublicIPAddress* şeklindedir. Diğer ayarların varsayılan değerlerini kabul edin ve sonra **Tamam**’a tıklayın.
8. Dinleyici yapılandırması için varsayılan değerleri kabul edin, Web uygulaması güvenlik duvarı devre dışı bırakın ve ardından **Tamam**.
9. Özet sayfasında ayarları gözden geçirin ve ardından **Tamam** ağ kaynaklarının ve uygulama ağ geçidi oluşturmak için. Bu, uygulama ağ geçidinin oluşturulması, sonraki bölüme geçmeden önce dağıtımın başarıyla tamamlanana kadar bekleyin birkaç dakika sürebilir.

### <a name="add-a-subnet"></a>Alt ağ ekleme

1. Sol taraftaki menüde **Tüm kaynaklar**’a ve sonra kaynaklar listesinden **myVNet** öğesine tıklayın.
2. Tıklayın **alt ağlar**ve ardından **alt**.

    ![Alt ağ oluşturma](./media/application-gateway-web-application-firewall-portal/application-gateway-subnet.png)

3. Alt ağ adı için *myBackendSubnet* girin ve sonra **Tamam**’a tıklayın.

## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu örnekte, uygulama ağ geçidi için arka uç sunucular olarak kullanılacak iki sanal makine oluşturacaksınız. Uygulama ağ geçidinin başarıyla oluşturulduğunu doğrulamak için sanal makinelere IIS de yükleyin.

### <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

1. **Yeni**’ye tıklayın.
2. Tıklayın **işlem** seçip **Windows Server 2016 Datacenter** Öne çıkanlar listesinde.
3. Sanal makine için şu değerleri girin:

    - *myVM* - Sanal makinenin adı.
    - Yönetici kullanıcı adı için *azureuser*.
    - *Azure123456!* Parola.
    - **Mevcut olanı kullan**’ı seçin ve *myResourceGroupAG* seçeneğini belirleyin.

4. **Tamam** düğmesine tıklayın.
5. Sanal makinenin boyutu için **DS1_V2** seçeneğini belirleyin ve **Seç**’e tıklayın.
6. Sanal ağ için **myVNet** öğesinin seçili olduğundan ve alt ağın **myBackendSubnet** olduğundan emin olun. 
7. Önyükleme tanılamalarını devre dışı bırakmak için **Devre Dışı** seçeneğine tıklayın.
8. **Tamam**’a tıklayın, özet sayfasındaki ayarları gözden geçirin ve sonra **Oluştur**’a tıklayın.

### <a name="install-iis"></a>IIS yükleme

1. Etkileşimli kabuğu açın ve **PowerShell**’e ayarlandığından emin olun.

    ![Özel uzantıyı yükleme](./media/application-gateway-web-application-firewall-portal/application-gateway-extension.png)

2. Sanal makineye IIS yüklemek için aşağıdaki komutu çalıştırın: 

    ```azurepowershell-interactive
    Set-AzureRmVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -ExtensionName IIS `
      -VMName myVM `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
      -Location EastUS
    ```

3. İkinci bir sanal makine oluşturun ve yeni tamamladığınız adımları kullanarak IIS yükleyin. Ad olarak ve VMName için Set-AzureRmVMExtension komutuna *myVM2* girin.

### <a name="add-backend-servers"></a>Arka uç sunucuları ekleme

1. Tıklayın **tüm kaynakları**ve ardından **myAppGateway**.
2. **Arka uç havuzları** öğesine tıklayın. Uygulama ağ geçidi ile varsayılan bir havuz otomatik olarak oluşturulur. **appGatewayBackendPool** öğesine tıklayın.
3. Tıklayın **Ekle hedef** arka uç havuzu için oluşturduğunuz her sanal makineye eklenecek.

    ![Arka uç sunucuları ekleme](./media/application-gateway-web-application-firewall-portal/application-gateway-backend.png)

4. **Kaydet**’e tıklayın.

## <a name="create-a-storage-account-and-configure-diagnostics"></a>Bir depolama hesabı oluşturma ve tanılamaları yapılandırma

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Bu öğreticide uygulama ağ geçidi, algılama ve önleme amacıyla verileri depolamak için bir depolama hesabı kullanır. Verileri kaydetmek için Log Analytics veya Event Hub hizmetini de kullanabilirsiniz.

1. Tıklayın **yeni** Azure portalının sol üst köşedeki üzerinde bulunamadı.
2. **Depolama**’yı ve sonra **Depolama hesabı - blob, dosya, tablo, kuyruk** öğesini seçin.
3. Select depolama hesabının adını girin **var olanı kullan** seçin ve kaynak grubu için **myResourceGroupAG**. Bu örnekte, depolama hesabının adıdır *myagstore1*. Diğer ayarlar için varsayılan değerleri kabul edin ve ardından **Oluştur**.

## <a name="configure-diagnostics"></a>Tanılama yapılandırma

Tanılamayı ApplicationGatewayAccessLog, ApplicationGatewayPerformanceLog ve ApplicationGatewayFirewallLog günlüklerine verileri kaydedecek şekilde yapılandırın.

1. Sol taraftaki menüde **tüm kaynakları**ve ardından *myAppGateway*.
2. İzleme bölümünden tıklayın **tanılama günlükleri**.
3. Tıklayın **tanılama ayarı ekleme**.
4. Girin *myDiagnosticsSettings* tanılama ayarları için ad olarak.
5. Seçin **bir depolama hesabında arşivle**ve ardından **yapılandırma** seçilecek *myagstore1* daha önce oluşturduğunuz depolama hesabı.
6. Application gateway günlüklerini toplamak ve korumak için seçin.
7. **Kaydet**’e tıklayın.

    ![Tanılama yapılandırma](./media/application-gateway-web-application-firewall-portal/application-gateway-diagnostics.png)

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

1. Genel Bakış ekranında uygulama ağ geçidi için genel IP adresini bulun. Tıklayın **tüm kaynakları** ve ardından **myAGPublicIPAddress**.

    ![Uygulama ağ geçidi genel IP adresini kaydetme](./media/application-gateway-web-application-firewall-portal/application-gateway-record-ag-address.png)

2. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın.

    ![Uygulama ağ geçidini test etme](./media/application-gateway-web-application-firewall-portal/application-gateway-iistest.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, öğrendiğiniz nasıl yapılır:

> [!div class="checklist"]
> * WAF etkinken bir uygulama ağ geçidi oluşturma
> * Arka uç sunucular olarak kullanılan sanal makine oluşturma
> * Bir depolama hesabı oluşturma ve tanılamaları yapılandırma

Uygulama ağ geçitleri ve bunların ilişkili kaynakları hakkında daha fazla bilgi edinmek için nasıl yapılır makaleleriyle devam edin.