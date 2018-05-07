---
title: Bir web uygulaması güvenlik duvarı ile - Azure portalında bir uygulama ağ geçidi oluşturma | Microsoft Docs
description: Azure portalı kullanarak bir web uygulaması güvenlik duvarı ile bir uygulama ağ geçidi oluşturmayı öğrenin.
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
ms.openlocfilehash: 9967813b193159b68aa0f008dae4440aa6e533dc
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="create-an-application-gateway-with-a-web-application-firewall-using-the-azure-portal"></a>Azure Portalı'nı kullanarak bir web uygulaması güvenlik duvarı ile bir uygulama ağ geçidi oluşturma

Azure Portalı'nı oluşturmak için kullanabileceğiniz bir [uygulama ağ geçidi](application-gateway-introduction.md) ile bir [web uygulaması güvenlik duvarı](application-gateway-web-application-firewall-overview.md) (WAF). WAF kullandığı [OWASP](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) uygulamanızı korumak için kurallar. Bu kurallar SQL ekleme gibi saldırıları, siteler arası komut dosyası saldırıları ve oturumu ele geçirilmesini karşı koruma içerir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Etkin WAF ile bir uygulama ağ geçidi oluşturma
> * Arka uç sunucuları olarak kullanılan sanal makine oluşturma
> * Bir depolama hesabı oluşturmak ve tanılama Yapılandır

![Web uygulaması güvenlik duvarı örneği](./media/application-gateway-web-application-firewall-portal/scenario-waf.png)

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Azure portalında oturum açın [http://portal.azure.com](http://portal.azure.com)

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Bir sanal ağ, oluşturduğunuz kaynakları arasındaki iletişim için gereklidir. Bu örnekte, iki alt ağ oluşturulur: bir uygulama ağ geçidi ve arka uç sunucular için diğer için. Uygulama ağ geçidi oluşturma aynı anda bir sanal ağ oluşturabilirsiniz.

1. Tıklatın **yeni** Azure portalında sol üst köşesinde bulundu.
2. Seçin **ağ** ve ardından **uygulama ağ geçidi** öne çıkan listesinde.
3. Uygulama ağ geçidi için bu değerleri girin:

    - *myAppGateway* - uygulama ağ geçidi adı.
    - *myResourceGroupAG* - yeni kaynak grubu için.
    - Seçin *WAF* uygulama ağ geçidi katmanı için.

    ![Yeni uygulama ağ geçidi oluşturma](./media/application-gateway-web-application-firewall-portal/application-gateway-create.png)

4. Diğer ayarlar için varsayılan değerleri kabul edin ve ardından **Tamam**.
5. Tıklatın **sanal ağ seçin**, tıklatın **Yeni Oluştur**ve ardından sanal ağ için bu değerleri girin:

    - *myVNet* - sanal ağın adı.
    - *10.0.0.0/16* - sanal ağ adres alanı.
    - *myAGSubnet* - alt ağ adı.
    - *10.0.0.0/24* - alt ağ adres alanı.

    ![Sanal ağ oluşturma](./media/application-gateway-web-application-firewall-portal/application-gateway-vnet.png)

6. Tıklatın **Tamam** sanal ağ ve alt ağ oluşturmak için.
7. Tıklatın **genel bir IP adresi seçin**, tıklatın **Yeni Oluştur**ve ortak IP adresini girin. Bu örnekte adlı ortak IP adresi *myAGPublicIPAddress*. Diğer ayarlar için varsayılan değerleri kabul edin ve ardından **Tamam**.
8. Dinleyici yapılandırması için varsayılan değerleri kabul edin, Web uygulaması güvenlik duvarı devre dışı bırakın ve ardından **Tamam**.
9. Özet sayfasında ayarları gözden geçirin ve ardından **Tamam** ağ kaynaklarına ve uygulama ağ geçidi oluşturmak için. Oluşturulması, dağıtımı, sonraki bölüme geçmeden önce başarıyla tamamlanana kadar bekleyin uygulama ağ geçidi için birkaç dakika sürebilir.

### <a name="add-a-subnet"></a>Bir alt ağ Ekle

1. Tıklatın **tüm kaynakları** sol taraftaki menüyü ve ardından **myVNet** kaynakları listesinden.
2. Tıklatın **alt ağlar**ve ardından **alt**.

    ![Alt ağ oluşturma](./media/application-gateway-web-application-firewall-portal/application-gateway-subnet.png)

3. Girin *myBackendSubnet* 'ye tıklayın ve alt ağ adı için **Tamam**.

## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu örnekte uygulama ağ geçidi için arka uç sunucuları olarak kullanılacak iki sanal makine oluşturun. Ayrıca uygulama ağ geçidi başarıyla oluşturulduğunu doğrulamak için sanal makinelerde IIS yükleyin.

### <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

1. **Yeni**’ye tıklayın.
2. Tıklatın **işlem** ve ardından **Windows Server 2016 Datacenter** öne çıkan listesinde.
3. Sanal makine için bu değerleri girin:

    - *myVM* - sanal makine adı için.
    - Yönetici kullanıcı adı için *azureuser*.
    - *Azure123456!* parolası.
    - Seçin **var olanı kullan**ve ardından *myResourceGroupAG*.

4. **Tamam**’a tıklayın.
5. Sanal makinenin boyutu için **DS1_V2** seçeneğini belirleyin ve **Seç**’e tıklayın.
6. Olduğundan emin olun **myVNet** sanal ağ ve alt ağ için seçili olan **myBackendSubnet**. 
7. Önyükleme tanılamalarını devre dışı bırakmak için **Devre Dışı** seçeneğine tıklayın.
8. **Tamam**’a tıklayın, özet sayfasındaki ayarları gözden geçirin ve sonra **Oluştur**’a tıklayın.

### <a name="install-iis"></a>IIS yükleme

1. Etkileşimli Kabuğu'nu açın ve onu ayarlandığından emin olun **PowerShell**.

    ![Özel uzantısını yükleyin](./media/application-gateway-web-application-firewall-portal/application-gateway-extension.png)

2. IIS sanal makineye yüklemek için aşağıdaki komutu çalıştırın: 

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

3. İkinci bir sanal makine oluşturun ve yalnızca tamamlandı adımları kullanarak IIS yükleyin. Girin *myVM2* adını ve Set-AzureRmVMExtension VMName.

### <a name="add-backend-servers"></a>Arka uç sunucuları ekleme

1. Tıklatın **tüm kaynakları**ve ardından **myAppGateway**.
2. Tıklatın **arka uç havuzları**. Varsayılan bir havuzu uygulama ağ geçidi ile otomatik olarak oluşturuldu. Tıklatın **appGateayBackendPool**.
3. Tıklatın **Ekle hedef** oluşturduğunuz her bir sanal makine arka uç havuzuna eklemek için.

    ![Arka uç sunucuları ekleme](./media/application-gateway-web-application-firewall-portal/application-gateway-backend.png)

4. **Kaydet**’e tıklayın.

## <a name="create-a-storage-account-and-configure-diagnostics"></a>Bir depolama hesabı oluşturmak ve tanılama Yapılandır

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Bu öğreticide, uygulama ağ geçidi saptamak ve önlemek amacıyla verileri depolamak için bir depolama hesabı kullanır. Veri kaydı için günlük analizi veya olay hub'ı da kullanabilirsiniz.

1. Tıklatın **yeni** Azure portalında sol üst köşesinde bulundu.
2. **Depolama**’yı ve sonra **Depolama hesabı - blob, dosya, tablo, kuyruk** öğesini seçin.
3. Select depolama hesabının adını girin **kullanım varolan** kaynak grubu ve ardından **myResourceGroupAG**. Bu örnekte, depolama hesabı adı olan *myagstore1*. Diğer ayarlar için varsayılan değerleri kabul edin ve ardından **oluşturma**.

## <a name="configure-diagnostics"></a>Tanılama Yapılandır

Tanılama verileri kaydetmek üzere ApplicationGatewayAccessLog, ApplicationGatewayPerformanceLog ve ApplicationGatewayFirewallLog günlüklerine yapılandırın.

1. Sol menüde tıklatın **tüm kaynakları**ve ardından *myAppGateway*.
2. İzleme altında tıklatın **tanılama günlükleri**.
3. Tıklatın **tanılama ayar Ekle**.
4. Girin *myDiagnosticsSettings* tanılama ayarları için bir isim olarak.
5. Seçin **bir depolama hesabı arşive**ve ardından **yapılandırma** seçmek için *myagstore1* daha önce oluşturduğunuz depolama hesabı.
6. Uygulama ağ geçidi günlüklerini toplamak ve korumak için seçin.
7. **Kaydet**’e tıklayın.

    ![Tanılama Yapılandır](./media/application-gateway-web-application-firewall-portal/application-gateway-diagnostics.png)

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidi sınama

1. Genel Bakış ekranında uygulama ağ geçidi için genel IP adresini bulun. Tıklatın **tüm kaynakları** ve ardından **myAGPublicIPAddress**.

    ![Uygulama ağ geçidi genel IP adresi kaydı](./media/application-gateway-web-application-firewall-portal/application-gateway-record-ag-address.png)

2. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın.

    ![Test uygulama ağ geçidi](./media/application-gateway-web-application-firewall-portal/application-gateway-iistest.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, öğrenilen nasıl yapılır:

> [!div class="checklist"]
> * Etkin WAF ile bir uygulama ağ geçidi oluşturma
> * Arka uç sunucuları olarak kullanılan sanal makine oluşturma
> * Bir depolama hesabı oluşturmak ve tanılama Yapılandır

Uygulama ağ geçitleri ile ilişkili kaynakları hakkında daha fazla bilgi için nasıl yapılır makaleleri devam edin.