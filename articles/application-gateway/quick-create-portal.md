---
title: 'Hızlı Başlangıç: Azure Application Gateway ile web trafiğini yönlendirme - Azure portalı | Microsoft Docs'
description: Web trafiğini arka uç havuzundaki sanal makinelere yönlendiren bir Azure Application Gateway oluşturmak üzere Azure portalını kullanmayı öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
ms.date: 1/8/2019
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 16e23f77509d2402f765981b39a30e08a2309f68
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2019
ms.locfileid: "54156536"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-portal"></a>Hızlı Başlangıç: Azure Application Gateway - Azure portalı ile doğrudan web trafiği

Bu hızlı başlangıç, arka uç havuzunda iki sanal makine bulunan bir uygulama ağ geçidini hızla oluşturmak üzere Azure portalını kullanmayı göstermektedir. Ardından doğru bir şekilde çalışıp çalışmadığından emin olmak için test edersiniz. Azure Application Gateway ile belirli kaynaklar tarafından uygulama web trafiği doğrudan: dinleyici bağlantı noktalarına atama, kuralları oluşturma ve kaynakları bir arka uç havuzuna ekleme.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure hesabınızla [Azure portalında](https://portal.azure.com) oturum açın.

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Oluşturduğunuz kaynaklar arasında iletişim kurmak Azure için sanal ağ gerekir. Bu örnekte iki alt ağa oluşturursunuz: bir uygulama ağ geçidi ve diğeri arka uç sunucuları için. Uygulama ağ geçidini oluştururken aynı zamanda bir sanal makine oluşturabilirsiniz.

1. Seçin **kaynak Oluştur** Azure portal'ın sol menüsünde. **Yeni** penceresi görüntülenir.

2. Seçin **ağ** seçip **Application Gateway** içinde **öne çıkan** listesi.

### <a name="basics-page"></a>Temel bilgileri sayfası

1. Üzerinde **Temelleri** sayfasında, aşağıdaki uygulama ağ geçidi ayarları için şu değerleri girin:

    - **Ad**: Girin *myAppGateway* uygulama ağ geçidinin adı.
    - **Kaynak grubu**: Seçin **myResourceGroupAG** kaynak grubu için. Mevcut olmaması halinde seçin **Yeni Oluştur** oluşturun.

    ![Yeni uygulama ağ geçidi oluşturma](./media/application-gateway-create-gateway-portal/application-gateway-create.png)

2. Diğer ayarlar için varsayılan değerleri kabul edin ve ardından **Tamam**.

### <a name="settings-page"></a>Ayarlar sayfası

1. Üzerinde **ayarları** sayfasındaki **alt ağ yapılandırması**seçin **bir sanal ağ seçin**.

2. Üzerinde **sanal ağ Seç** sayfasında **Yeni Oluştur**ve ardından aşağıdaki sanal ağ ayarları için değerleri girin:

    - **Ad**: Girin *myVNet* sanal ağının adı.

    - **Adres alanı**: Girin *10.0.0.0/16* sanal ağ adres alanı için.

    - **Alt ağ adı**: Girin *myAGSubnet* alt ağ adı için.<br>Uygulama ağ geçidi alt ağı, yalnızca uygulama ağ geçitleri içerebilir. Başka kaynaklar izin verilir.

    - **Alt ağ adres aralığı**: Girin *10.0.0.0/24* için alt ağ adres aralığı.

    ![Sanal ağ oluşturma](./media/application-gateway-create-gateway-portal/application-gateway-vnet.png)

3. Seçin **Tamam** dönmek için **ayarları** sayfası.

4. Altında **ön uç IP yapılandırması**, doğrulama **IP adresi türü** ayarlanır **genel**. Altında **genel IP adresi**, doğrulama **Yeni Oluştur** seçilir. 

5. Girin *myAGPublicIPAddress* için genel IP adresi adı. 

6. Diğer ayarlar için varsayılan değerleri kabul edin ve ardından **Tamam**.

### <a name="summary-page"></a>Özet sayfası

Ayarları gözden **özeti** sayfasında ve ardından **Tamam** sanal ağ, genel IP adresini ve uygulama ağ geçidi oluşturun. Bu, Azure uygulama ağ geçidi oluşturmak birkaç dakika sürebilir. Sonraki bölüme geçmeden önce dağıtımın başarıyla tamamlanana kadar bekleyin.

## <a name="add-a-subnet"></a>Alt ağ ekleme

Aşağıdaki adımları izleyerek oluşturduğunuz sanal ağa bir alt ağ ekleyin:

1. Seçin **tüm kaynakları** Azure portalının sol taraftaki menüde girin *myVNet* arama kutusuna ve ardından **myVNet** Arama sonuçlarından.

2. Seçin **alt ağlar** seçin ve soldaki menüden **+ alt ağ**. 

    ![Alt ağ oluşturma](./media/application-gateway-create-gateway-portal/application-gateway-subnet.png)

3. Gelen **alt ağ Ekle** want *myBackendSubnet* için **adı** alt ağ ve ardından **Tamam**.

## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu örnekte, Azure application gateway için arka uç sunucular olarak kullanan iki sanal makine oluşturun. Ayrıca Azure uygulama ağ geçidi başarıyla oluşturuldu doğrulamak için sanal makinelere IIS yüklersiniz.

### <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

1. Azure Portal'da seçin **kaynak Oluştur**. **Yeni** penceresi görüntülenir.

2. Seçin **işlem** seçip **Windows Server 2016 Datacenter** içinde **öne çıkan** listesi. **Sanal makine oluşturma** sayfası görüntülenir.

3. Bu değerleri girin **Temelleri** aşağıdaki sanal makine ayarları için sekmesinde:

    - **Kaynak grubu**: Seçin **myResourceGroupAG** için kaynak grubu adı.
    - **Sanal makine adı**: Girin *myVM* sanal makinenin adı.
    - **Kullanıcı adı**: Girin *azureuser* için yönetici kullanıcı adı.
    - **Parola**: Girin *Azure123456!* Yönetici parolası.

4. Diğer varsayılan değerleri kabul edin ve ardından **sonraki: Diskleri**.  

5. Kabul **diskleri** Varsayılanları sekmesini seçip **sonraki: Ağ**.

6. Üzerinde **ağ** sekmesinde, doğrulayın **myVNet** seçildiğini **sanal ağ** ve **alt** ayarlanır  **myBackendSubnet**. Diğer varsayılan değerleri kabul edin ve ardından **sonraki: Yönetim**.

8. Üzerinde **Yönetim** sekmesinde, belirleyin **önyükleme tanılaması** için **kapalı**. Diğer varsayılan değerleri kabul edin ve ardından **gözden geçir + Oluştur**.

9. Üzerinde **gözden + Oluştur** sekmesinde, ayarları gözden geçirin, tüm doğrulama hatalarını düzeltin ve ardından **Oluştur**.

10. Sanal makine oluşturma işlemi devam etmeden önce tamamlanmasını bekleyin.

### <a name="install-iis"></a>IIS yükleme

1. Açık [Azure PowerShell](https://docs.microsoft.com/azure/cloud-shell/quickstart-powershell). Bunu yapmak için **Cloud Shell** Azure portal ve ardından üst gezinti çubuğundan **PowerShell** aşağı açılan listeden. 

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

3. İkinci sanal makine oluşturma ve daha önce tamamladığınız adımları kullanarak IIS yükleyin. Kullanım *myVM2* ve sanal makine adı için **VMName** ayarıyla **Set-AzureRmVMExtension** cmdlet'i.

### <a name="add-backend-servers"></a>Arka uç sunucuları ekleme

1. Seçin **tüm kaynakları**ve ardından **myAppGateway**.

2. Seçin **arka uç havuzları** sol menüden. Azure, varsayılan bir havuzu otomatik olarak oluşturulan **appGatewayBackendPool**, uygulama ağ geçidi oluşturduğunuzda. 

3. Seçin **appGatewayBackendPool**.

4. Altında **hedefleri**seçin **sanal makine** aşağı açılan listeden.

5. Altında **sanal makine** ve **ağ arabirimleri**seçin **myVM** ve **myVM2** sanal makineler ve bunların ilişkili ağ aşağı açılan listelerden arabirimleri.

    ![Arka uç sunucuları ekleme](./media/application-gateway-create-gateway-portal/application-gateway-backend.png)

6. **Kaydet**’i seçin.

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

1. Uygulama ağ geçidi için genel IP adresini bulmak, **genel bakış** sayfası.

    ![Uygulama ağ geçidi genel IP adresini kaydetme](./media/application-gateway-create-gateway-portal/application-gateway-record-ag-address.png)

    Alternatif olarak, seçebileceğiniz **tüm kaynakları**, girin *myAGPublicIPAddress* arama kutusuna ve ardından arama sonuçlarında seçin. Azure üzerinde genel IP adresini görüntüler **genel bakış** sayfası.

2. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın.

    ![Uygulama ağ geçidini test etme](./media/application-gateway-create-gateway-portal/application-gateway-iistest.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Application gateway ile oluşturduğunuz kaynaklara artık ihtiyacınız olduğunda, kaynak grubunu kaldırın. Kaynak grubu kaldırarak, ayrıca uygulama ağ geçidi ve tüm ilgili kaynakları kaldırın. 

Kaynak grubunu kaldırmak için:
1. Azure portal'ın sol menüsünde **kaynak grupları**.
2. Üzerinde **kaynak grupları** sayfasında, arama **myResourceGroupAG** listesinde seçin.
3. Üzerinde **kaynak grubu sayfasını**seçin **kaynak grubunu Sil**.
4. Girin *myResourceGroupAG* için **kaynak grubu adını yazın** seçip **Sil**

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure CLI kullanarak bir uygulama ağ geçidi ile web trafiğini yönetme](./tutorial-manage-web-traffic-cli.md)
