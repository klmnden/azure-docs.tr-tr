---
title: Azure Portalı - Uygulama ağ geçidi oluşturma | Microsoft Docs
description: Azure portalı kullanarak bir uygulama ağ geçidi oluşturmayı öğrenin.
services: application-gateway
author: vhorne
manager: jpconnock
editor: ''
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/25/2018
ms.author: victorh
ms.openlocfilehash: 0df71c445d2c5fc6827b69f708203a3b3e6e2b53
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
ms.locfileid: "33202397"
---
# <a name="create-an-application-gateway-using-the-azure-portal"></a>Azure Portalı'nı kullanarak bir uygulama ağ geçidi oluşturma

Azure Portalı'nı oluşturmak veya uygulama ağ geçitleri yönetmek için kullanabilirsiniz. Bu Hızlı Başlangıç, ağ kaynaklarına, arka uç sunucularının ve bir uygulama ağ geçidi nasıl oluşturulacağını gösterir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Azure portalında oturum açın [http://portal.azure.com](http://portal.azure.com)

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Bir sanal ağ, oluşturduğunuz kaynakları arasındaki iletişim için gereklidir. Bu örnekte, iki alt ağ oluşturulur: bir uygulama ağ geçidi ve arka uç sunucular için diğer için. Uygulama ağ geçidi oluşturma aynı anda bir sanal ağ oluşturabilirsiniz.

1. Tıklatın **yeni** Azure portalında sol üst köşesinde bulundu.
2. Seçin **ağ** ve ardından **uygulama ağ geçidi** öne çıkan listesinde.
3. Uygulama ağ geçidi için bu değerleri girin:

    - *myAppGateway* - uygulama ağ geçidi adı.
    - *myResourceGroupAG* - yeni kaynak grubu için.

    ![Yeni uygulama ağ geçidi oluşturma](./media/application-gateway-create-gateway-portal/application-gateway-create.png)

4. Diğer ayarlar için varsayılan değerleri kabul edin ve ardından **Tamam**.
5. Tıklatın **sanal ağ seçin**, tıklatın **Yeni Oluştur**ve ardından sanal ağ için bu değerleri girin:

    - *myVNet* - sanal ağın adı.
    - *10.0.0.0/16* - sanal ağ adres alanı.
    - *myAGSubnet* - alt ağ adı.
    - *10.0.0.0/24* - alt ağ adres alanı.

    ![Sanal ağ oluşturma](./media/application-gateway-create-gateway-portal/application-gateway-vnet.png)

6. Tıklatın **Tamam** sanal ağ ve alt ağ oluşturmak için.
6. Tıklatın **genel bir IP adresi seçin**, tıklatın **Yeni Oluştur**ve ortak IP adresini girin. Bu örnekte adlı ortak IP adresi *myAGPublicIPAddress*. Diğer ayarlar için varsayılan değerleri kabul edin ve ardından **Tamam**.
8. Dinleyici yapılandırması için varsayılan değerleri kabul edin, Web uygulaması güvenlik duvarı devre dışı bırakın ve ardından **Tamam**.
9. Özet sayfasında ayarları gözden geçirin ve ardından **Tamam** sanal ağ, genel IP adresi ve uygulama ağ geçidi oluşturmak için. Oluşturulması, dağıtımı, sonraki bölüme geçmeden önce başarıyla tamamlanana kadar bekleyin uygulama ağ geçidi için birkaç dakika sürebilir.

### <a name="add-a-subnet"></a>Bir alt ağ Ekle

1. Tıklatın **tüm kaynakları** sol taraftaki menüyü ve ardından **myVNet** kaynakları listesinden.
2. Tıklatın **alt ağlar**ve ardından **alt**.

    ![Alt ağ oluşturma](./media/application-gateway-create-gateway-portal/application-gateway-subnet.png)

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

    ![Özel uzantısını yükleyin](./media/application-gateway-create-gateway-portal/application-gateway-extension.png)

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

3. Tıklatın **tüm kaynakları**ve ardından **myAppGateway**.
4. Tıklatın **arka uç havuzları**. Varsayılan bir havuzu uygulama ağ geçidi ile otomatik olarak oluşturuldu. Tıklatın **appGatewayBackendPool**.
5. Tıklatın **Ekle hedef** oluşturduğunuz her bir sanal makine arka uç havuzuna eklemek için.

    ![Arka uç sunucuları ekleme](./media/application-gateway-create-gateway-portal/application-gateway-backend.png)

6. **Kaydet**’e tıklayın.

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidi sınama

1. Genel Bakış ekranında uygulama ağ geçidi için genel IP adresini bulun. Tıklatın **tüm kaynakları** ve ardından **myAGPublicIPAddress**.

    ![Uygulama ağ geçidi genel IP adresi kaydı](./media/application-gateway-create-gateway-portal/application-gateway-record-ag-address.png)

2. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın.

    ![Test uygulama ağ geçidi](./media/application-gateway-create-gateway-portal/application-gateway-iistest.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, kaynak grubu, uygulama ağ geçidi ve tüm ilişkili kaynakları silin. Bunu yapmak için tıklatın ve uygulama ağ geçidi içeren kaynak grubunu seçin **silmek**.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir kaynak grubu, ağ kaynakları ve arka uç sunucuları oluşturdunuz. Bir uygulama ağ geçidi oluşturmak için bu kaynakları kullanılır. Uygulama ağ geçitleri ile ilişkili kaynakları hakkında daha fazla bilgi için nasıl yapılır makaleleri devam edin.
