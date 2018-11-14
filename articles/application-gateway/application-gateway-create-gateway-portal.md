---
title: Azure Portalı - Uygulama ağ geçidi oluşturma
description: Azure portalını kullanarak bir uygulama ağ geçidi oluşturmayı öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 11/14/2018
ms.author: victorh
ms.openlocfilehash: ba4ab7e488de4a8d3c9fa702c6b33f9ea31b2c2a
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51615968"
---
# <a name="create-an-application-gateway-using-the-azure-portal"></a>Azure portalını kullanarak bir uygulama ağ geçidi oluşturma

Azure portalı, oluşturmak veya uygulama ağ geçitleri yönetmek için kullanabilirsiniz. Bu hızlı başlangıçta, ağ kaynakları ve arka uç sunucuları uygulama ağ geçidi oluşturulacağını gösterir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[http://portal.azure.com](http://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Bir sanal ağ, oluşturduğunuz kaynakları arasındaki iletişim için gereklidir. Bu örnekte iki alt ağ oluşturulmuştur: biri uygulama ağ geçidi ve diğeri de arka uç sunucuları içindir. Uygulama ağ geçidini oluştururken aynı zamanda bir sanal makine oluşturabilirsiniz.

1. Tıklayın **yeni** Azure portalının sol üst köşedeki üzerinde bulunamadı.
2. **Ağ** ve ardından Öne Çıkanlar listesinde **Application Gateway**’i seçin.
3. Uygulama ağ geçidi için şu değerleri girin:

    - *myAppGateway* - Uygulama ağ geçidinin adı.
    - *myResourceGroupAG* - Yeni kaynak grubu.

    ![Yeni uygulama ağ geçidi oluşturma](./media/application-gateway-create-gateway-portal/application-gateway-create.png)

4. Diğer ayarların varsayılan değerlerini kabul edin ve sonra **Tamam**’a tıklayın.
5. Tıklayın **bir sanal ağ seçin**, tıklayın **Yeni Oluştur**ve ardından sanal ağ için şu değerleri girin:

    - *myVNet* - Sanal ağın adı.
    - *10.0.0.0/16* - Sanal ağın adres alanı.
    - *myAGSubnet* - Alt ağın adı.
    - *10.0.0.0/24* - Alt ağın adres alanı.

    ![Sanal ağ oluşturma](./media/application-gateway-create-gateway-portal/application-gateway-vnet.png)

6. Sanal ağı ve alt ağı oluşturmak için **Tamam**’a tıklayın.
6. Tıklayın **genel bir IP adresi seçin**, tıklayın **Yeni Oluştur**ve ardından genel IP adresini adını girin. Bu örnekte genel IP adresinin adı *myAGPublicIPAddress* şeklindedir. Diğer ayarların varsayılan değerlerini kabul edin ve sonra **Tamam**’a tıklayın.
8. Dinleyici yapılandırması için varsayılan değerleri kabul edin, Web uygulaması güvenlik duvarı devre dışı bırakın ve ardından **Tamam**.
9. Özet sayfasında ayarları gözden geçirin ve sonra **Tamam**’a tıklayarak sanal ağı, genel IP adresini ve uygulama ağ geçidini oluşturun. Bu, uygulama ağ geçidinin oluşturulması, sonraki bölüme geçmeden önce dağıtımın başarıyla tamamlanana kadar bekleyin birkaç dakika sürebilir.

### <a name="add-a-subnet"></a>Alt ağ ekleme

1. Sol taraftaki menüde **Tüm kaynaklar**’a ve sonra kaynaklar listesinden **myVNet** öğesine tıklayın.
2. Tıklayın **alt ağlar**ve ardından **alt**.

    ![Alt ağ oluşturma](./media/application-gateway-create-gateway-portal/application-gateway-subnet.png)

3. Alt ağ adı için *myBackendSubnet* girin ve sonra **Tamam**’a tıklayın.

## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu örnekte, application gateway için arka uç sunucular olarak kullanan iki sanal makine oluşturun. Uygulama ağ geçidinin başarıyla oluşturulduğunu doğrulamak için sanal makinelere IIS de yükleyin.

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

3. Tıklayın **tüm kaynakları**ve ardından **myAppGateway**.
4. **Arka uç havuzları** öğesine tıklayın. Uygulama ağ geçidi ile varsayılan bir havuz otomatik olarak oluşturulur. **appGatewayBackendPool** öğesine tıklayın.
5. Tıklayın **Ekle hedef** arka uç havuzu için oluşturduğunuz her sanal makineye eklenecek.

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

Bu hızlı başlangıçta bir kaynak grubu, ağ kaynakları ve arka uç sunucuları oluşturdunuz. Bir uygulama ağ geçidi oluşturmak için bu kaynakları kullanılır. Uygulama ağ geçitleri ve bunların ilişkili kaynakları hakkında daha fazla bilgi edinmek için nasıl yapılır makaleleriyle devam edin.
