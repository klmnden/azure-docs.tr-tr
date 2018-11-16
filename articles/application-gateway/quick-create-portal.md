---
title: 'Hızlı Başlangıç: Azure Application Gateway ile web trafiğini yönlendirme - Azure portalı | Microsoft Docs'
description: Web trafiğini arka uç havuzundaki sanal makinelere yönlendiren bir Azure Application Gateway oluşturmak üzere Azure portalını kullanmayı öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
ms.date: 11/15/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 6ad839b9cf1179e282b9163df5a38e13417408e2
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51706237"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-portal"></a>Hızlı Başlangıç: Azure Application Gateway ile web trafiğini yönlendirme - Azure portalı

Azure Application Gateway ile bağlantı noktalarına dinleyiciler atayarak ve arka uç havuzuna kaynaklar ekleyerek uygulama web trafiğinizi belirli kaynaklara yönlendirebilirsiniz.

Bu hızlı başlangıç, arka uç havuzunda iki sanal makine bulunan bir uygulama ağ geçidini hızla oluşturmak üzere Azure portalını kullanmayı göstermektedir. Ardından doğru bir şekilde çalışıp çalışmadığından emin olmak için test edersiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[http://portal.azure.com](http://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Bir sanal ağ, oluşturduğunuz kaynakları arasındaki iletişim için gereklidir. Bu örnekte iki alt ağ oluşturulmuştur: biri uygulama ağ geçidi ve diğeri de arka uç sunucuları içindir. Uygulama ağ geçidini oluştururken aynı zamanda bir sanal makine oluşturabilirsiniz.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. Tıklayın **ağ** ve ardından **Application Gateway** Öne çıkanlar listesinde.

### <a name="basics"></a>Temel Bilgiler

1. Uygulama ağ geçidi için şu değerleri girin:

    - *myAppGateway* - Uygulama ağ geçidinin adı.
    - *myResourceGroupAG* - Yeni kaynak grubu.

    ![Yeni uygulama ağ geçidi oluşturma](./media/application-gateway-create-gateway-portal/application-gateway-create.png)

2. Diğer ayarların varsayılan değerlerini kabul edin ve sonra **Tamam**’a tıklayın.

### <a name="settings"></a>Ayarlar

1. Tıklayın **bir sanal ağ seçin**, tıklayın **Yeni Oluştur**ve ardından sanal ağ için şu değerleri girin:

    - *myVNet* - Sanal ağın adı.
    - *10.0.0.0/16* - Sanal ağın adres alanı.
    - *myAGSubnet* - Alt ağın adı.
    - *10.0.0.0/24* - alt ağ adres aralığı.

    ![Sanal ağ oluşturma](./media/application-gateway-create-gateway-portal/application-gateway-vnet.png)

6. Tıklayın **Tamam** ayarlar sayfasına geri gidin.
7. Altında **ön uç IP yapılandırması** olun **IP adresi türü** ayarlanır **genel**, altında **genel IP adresi**, sağlamak**Yeni Oluştur** seçilir. Tür *myAGPublicIPAddress* için genel IP adresi adı. Diğer ayarların varsayılan değerlerini kabul edin ve sonra **Tamam**’a tıklayın.

### <a name="summary"></a>Özet

Özet sayfasında ayarları gözden geçirin ve sonra **Tamam**’a tıklayarak sanal ağı, genel IP adresini ve uygulama ağ geçidini oluşturun. Uygulama ağ geçidinin oluşturulması birkaç dakika sürebilir. Sonraki bölüme geçmeden önce dağıtımın başarıyla tamamlanana kadar bekleyin.

## <a name="add-a-subnet"></a>Alt ağ ekleme

1. Sol taraftaki menüde **Tüm kaynaklar**’a ve sonra kaynaklar listesinden **myVNet** öğesine tıklayın.
2. Tıklayın **alt ağlar**ve ardından **+ alt ağ**.

    ![Alt ağ oluşturma](./media/application-gateway-create-gateway-portal/application-gateway-subnet.png)

3. Alt ağ adı için *myBackendSubnet* girin ve sonra **Tamam**’a tıklayın.

## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu örnekte, application gateway için arka uç sunucular olarak kullanan iki sanal makine oluşturun. Uygulama ağ geçidinin başarıyla oluşturulduğunu doğrulamak için sanal makinelere IIS de yükleyin.

### <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

1. Azure portalında **Kaynak oluştur**’a tıklayın.
2. Tıklayın **işlem** seçip **Windows Server 2016 Datacenter** Öne çıkanlar listesinde.
3. Sanal makine için şu değerleri girin:

    - *myResourceGroupAG* kaynak grubu için.
    - *myVM* - Sanal makinenin adı.
    - Yönetici kullanıcı adı için *azureuser*.
    - *Azure123456!* girin.

   Diğer varsayılan değerleri kabul edin ve tıklayın **sonraki: diskleri**.
4. Disk Varsayılanları kabul edin ve tıklayın **sonraki: ağ**.
5. Sanal ağ için **myVNet** öğesinin seçili olduğundan ve alt ağın **myBackendSubnet** olduğundan emin olun.
6. Diğer varsayılan değerleri kabul edin ve tıklayın **sonraki: Yönetim**.
7. Tıklayın **kapalı** önyükleme tanılaması devre dışı bırakmak için. Diğer varsayılan değerleri kabul edin ve tıklayın **gözden geçir + Oluştur**.
8. Özet sayfasında ayarları gözden geçirin ve ardından **Oluştur**.
9. Sanal makine oluşturma işlemi devam etmeden önce tamamlanmasını bekleyin.

### <a name="install-iis"></a>IIS yükleme

1. Etkileşimli bir kabuk açın ve bu ayarlandığından emin olun **PowerShell**.

    ![Özel uzantıyı yükleme](./media/application-gateway-create-gateway-portal/application-gateway-extension.png)

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
4. **Arka uç havuzları** öğesine tıklayın. Uygulama ağ geçidi ile varsayılan bir havuz otomatik olarak oluşturulur. **appGatewayBackendPool** öğesine tıklayın.
5. Altında **hedefleri**, tıklayın **IP adresi veya FQDN** seçin **sanal makine**.
6. Altında **sanal makine**, myVM ve myVM2 sanal makineleri ve bunların ilişkili ağ arabirimleri ekleyin.

    ![Arka uç sunucuları ekleme](./media/application-gateway-create-gateway-portal/application-gateway-backend.png)

6. **Kaydet**’e tıklayın.

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

1. Genel Bakış ekranında uygulama ağ geçidi için genel IP adresini bulun. Tıklayın **tüm kaynakları** ve ardından **myAGPublicIPAddress**.

    ![Uygulama ağ geçidi genel IP adresini kaydetme](./media/application-gateway-create-gateway-portal/application-gateway-record-ag-address.png)

2. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın.

    ![Uygulama ağ geçidini test etme](./media/application-gateway-create-gateway-portal/application-gateway-iistest.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, uygulama ağ geçidi ve tüm ilgili kaynakları silin. Bunu yapmak için, uygulama ağ geçidini içeren kaynak grubunu seçin ve **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure CLI kullanarak bir uygulama ağ geçidi ile web trafiğini yönetme](./tutorial-manage-web-traffic-cli.md)
