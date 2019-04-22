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
ms.openlocfilehash: 42d3bd2285574b4416ec06af13006353880a7ca5
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58903531"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-portal"></a>Hızlı Başlangıç: Azure Application Gateway - Azure portalı ile doğrudan web trafiği

Bu hızlı başlangıçta bir uygulama ağ geçidi oluşturmak için Azure portalını kullanmayı gösterir.  Uygulama ağ geçidi oluşturduktan sonra düzgün çalıştığından emin olmak için test edin. Azure Application Gateway ile bağlantı noktalarına dinleyicileri atama, kuralları oluşturma ve arka uç havuzu için kaynak ekleme, uygulama web trafiği belirli kaynaklara doğrudan. Basitleştirmek amacıyla, bu makalede bir genel ön uç IP ile basit bir Kurulum, konağa tek bir sitede bu uygulama ağ geçidinde temel dinleyiciyi arka uç havuzunu ve temel istek yönlendirme kuralı için kullanılan iki sanal makine kullanılmaktadır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure hesabınızla [Azure portalında](https://portal.azure.com) oturum açın.

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Oluşturduğunuz kaynaklar arasında iletişim kurmak Azure için sanal ağ gerekir. Yeni bir sanal ağ oluşturma veya var olanı kullanın. Bu örnekte, yeni bir sanal ağ oluşturacağız. Uygulama ağ geçidini oluştururken aynı zamanda bir sanal makine oluşturabilirsiniz. Uygulama ağ geçidi örnekleri ayrı alt ağlara oluşturulur. Bu örnekte iki alt ağa oluşturursunuz: bir uygulama ağ geçidi ve diğeri arka uç sunucuları için.

1. Seçin **kaynak Oluştur** Azure portal'ın sol menüsünde. **Yeni** penceresi görüntülenir.

2. Seçin **ağ** seçip **Application Gateway** içinde **öne çıkan** listesi.

### <a name="basics-page"></a>Temel bilgileri sayfası

1. Üzerinde **Temelleri** sayfasında, aşağıdaki uygulama ağ geçidi ayarları için şu değerleri girin:

   - **Ad**: Girin *myAppGateway* uygulama ağ geçidinin adı.
   - **Kaynak grubu**: Seçin **myResourceGroupAG** kaynak grubu için. Mevcut olmaması halinde seçin **Yeni Oluştur** oluşturun.

     ![Yeni uygulama ağ geçidi oluşturma](./media/application-gateway-create-gateway-portal/application-gateway-create.png)

2. Diğer ayarlar için varsayılan değerleri kabul edin ve ardından **Tamam**.

### <a name="settings-page"></a>Ayarlar sayfası

1. Üzerinde **ayarları** sayfasındaki **alt ağ yapılandırması**seçin **bir sanal ağ seçin**. <br>

2. Üzerinde **sanal ağ Seç** sayfasında **Yeni Oluştur**ve ardından aşağıdaki sanal ağ ayarları için değerleri girin:

   - **Ad**: Girin *myVNet* sanal ağının adı.

   - **Adres alanı**: Girin *10.0.0.0/16* sanal ağ adres alanı için.

   - **Alt ağ adı**: Girin *myAGSubnet* alt ağ adı için.<br>Uygulama ağ geçidi alt ağı, yalnızca uygulama ağ geçitleri içerebilir. Başka kaynaklar izin verilir.

   - **Alt ağ adres aralığı**: Girin *10.0.0.0/24* için alt ağ adres aralığı.

     ![Sanal ağ oluşturma](./media/application-gateway-create-gateway-portal/application-gateway-vnet.png)

3. Seçin **Tamam** dönmek için **ayarları** sayfası.

4. Seçin **ön uç IP yapılandırması**. Altında **ön uç IP yapılandırması**, doğrulama **IP adresi türü** ayarlanır **genel**. Altında **genel IP adresi**, doğrulama **Yeni Oluştur** seçilir. <br>Ön uç IP, genel veya özel kullanım Örneğinize göre olacak şekilde yapılandırabilirsiniz. Bu örnekte, genel bir ön uç IP seçeceğiz. 

5. Girin *myAGPublicIPAddress* için genel IP adresi adı. 

6. Diğer ayarlar için varsayılan değerleri kabul edin ve ardından **Tamam**.<br>Varsayılan değerleri kolaylık olması için bu makaledeki seçeceğiz, ancak özel değerler diğer ayarlar için kullanım Örneğinize bağlı olarak yapılandırabilirsiniz 

### <a name="summary-page"></a>Özet sayfası

Ayarları gözden **özeti** sayfasında ve ardından **Tamam** sanal ağ, genel IP adresini ve uygulama ağ geçidi oluşturun. Bu, Azure uygulama ağ geçidi oluşturmak birkaç dakika sürebilir. Sonraki bölüme geçmeden önce dağıtımın başarıyla tamamlanana kadar bekleyin.

## <a name="add-backend-pool"></a>Arka uç havuzu ekle

Arka uç havuzu, isteği sunan arka uç sunucuları istekleri yönlendirmek için kullanılır. Arka uç havuzları, ağ, sanal makine ölçek kümeleri, genel IP'ler birleştirilebilir, iç IP'ler, tam etki alanı adlarını (FQDN) ve çok kiracılı arka-Azure App Service gibi biter. Arka uç hedeflerinizi bir arka uç havuzuna eklemeniz gerekir.

Bu örnekte, hedef arka uç olarak sanal makineler kullanacağız. Biz varolan sanal makineleri kullanabilir veya yeni bir tane oluşturun. Bu örnekte, Azure application gateway için arka uç sunucular olarak kullanan iki sanal makine oluşturacağız. Bunu yapmak için yapacağız:

1. Yeni bir alt ağ oluşturma *myBackendSubnet*, yeni sanal makinelerin oluşturulması. 
2. 2 yeni VM oluşturma *myVM* ve *myVM2*, arka uç sunucular olarak kullanılacak.
3. Uygulama ağ geçidi başarıyla oluşturulduğunu doğrulamak için sanal makinelere IIS yüklersiniz.
4. Arka uç sunucularının arka uç havuzuna ekleyin.

### <a name="add-a-subnet"></a>Alt ağ ekleme

Aşağıdaki adımları izleyerek oluşturduğunuz sanal ağa bir alt ağ ekleyin:

1. Seçin **tüm kaynakları** Azure portalının sol taraftaki menüde girin *myVNet* arama kutusuna ve ardından **myVNet** Arama sonuçlarından.

2. Seçin **alt ağlar** seçin ve soldaki menüden **+ alt ağ**. 

   ![Alt ağ oluşturma](./media/application-gateway-create-gateway-portal/application-gateway-subnet.png)

3. Gelen **alt ağ Ekle** want *myBackendSubnet* için **adı** alt ağ ve ardından **Tamam**.

### <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

1. Azure Portal'da seçin **kaynak Oluştur**. **Yeni** penceresi görüntülenir.
2. Seçin **işlem** seçip **Windows Server 2016 Datacenter** içinde **öne çıkan** listesi. **Sanal makine oluşturma** sayfası görüntülenir.<br>Uygulama ağ geçidi, sanal makinenin kendi arka uç havuzunda kullanılan herhangi bir türü için trafiği yönlendirebilirsiniz. Bu örnekte, bir Windows Server 2016 Datacenter kullanın.
3. Bu değerleri girin **Temelleri** aşağıdaki sanal makine ayarları için sekmesinde:

    - **Kaynak grubu**: Seçin **myResourceGroupAG** için kaynak grubu adı.
    - **Sanal makine adı**: Girin *myVM* sanal makinenin adı.
    - **Kullanıcı adı**: Girin *azureuser* için yönetici kullanıcı adı.
    - **Parola**: Girin *Azure123456!* Yönetici parolası.
4. Diğer varsayılan değerleri kabul edin ve ardından **sonraki: Diskleri**.  
5. Kabul **diskleri** Varsayılanları sekmesini seçip **sonraki: Ağ**.
6. Üzerinde **ağ** sekmesinde, doğrulayın **myVNet** seçildiğini **sanal ağ** ve **alt** ayarlanır  **myBackendSubnet**. Diğer varsayılan değerleri kabul edin ve ardından **sonraki: Yönetim**.<br>Uygulama ağ geçidi örnekleri dışında içinde bir sanal ağ ile iletişim kurabilir, ancak biz IP bağlantısı olduğundan emin olmanız gerekir. 
7. Üzerinde **Yönetim** sekmesinde, belirleyin **önyükleme tanılaması** için **kapalı**. Diğer varsayılan değerleri kabul edin ve ardından **gözden geçir + Oluştur**.
8. Üzerinde **gözden + Oluştur** sekmesinde, ayarları gözden geçirin, tüm doğrulama hatalarını düzeltin ve ardından **Oluştur**.
9. Sanal makine oluşturma işlemi devam etmeden önce tamamlanmasını bekleyin.

### <a name="install-iis-for-testing"></a>Test etmek için IIS yükleme

Bu örnekte biz yalnızca Azure uygulama ağ geçidi başarıyla oluşturuldu doğrulamak amacıyla sanal makinelere IIS yüklüyorsunuz. 

1. Açık [Azure PowerShell](https://docs.microsoft.com/azure/cloud-shell/quickstart-powershell). Bunu yapmak için **Cloud Shell** Azure portal ve ardından üst gezinti çubuğundan **PowerShell** aşağı açılan listeden. 

    ![Özel uzantıyı yükleme](./media/application-gateway-create-gateway-portal/application-gateway-extension.png)

2. Sanal makineye IIS yüklemek için aşağıdaki komutu çalıştırın: 

    ```azurepowershell-interactive
    Set-AzVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -ExtensionName IIS `
      -VMName myVM `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
      -Location EastUS
    ```

3. İkinci sanal makine oluşturma ve daha önce tamamladığınız adımları kullanarak IIS yükleyin. Kullanım *myVM2* ve sanal makine adı için **VMName** ayarıyla **kümesi AzVMExtension** cmdlet'i.

### <a name="add-backend-servers-to-backend-pool"></a>Arka uç sunucularının arka uç havuzu Ekle

1. Seçin **tüm kaynakları**ve ardından **myAppGateway**.

2. Seçin **arka uç havuzları** sol menüden. Azure, varsayılan bir havuzu otomatik olarak oluşturulan **appGatewayBackendPool**, uygulama ağ geçidi oluşturduğunuzda. 

3. Seçin **appGatewayBackendPool**.

4. Altında **hedefleri**seçin **sanal makine** aşağı açılan listeden.

5. Altında **sanal makine** ve **ağ arabirimleri**seçin **myVM** ve **myVM2** sanal makineler ve bunların ilişkili ağ aşağı açılan listelerden arabirimleri.

    ![Arka uç sunucuları ekleme](./media/application-gateway-create-gateway-portal/application-gateway-backend.png)

6. **Kaydet**’i seçin.

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

IIS uygulama ağ geçidi oluşturmak için gerekli değildir, ancak bu hızlı başlangıçta Azure uygulama ağ geçidi başarıyla oluşturulmuş olup olmadığını doğrulamak için yüklü. IIS, uygulama ağ geçidi test etmek için kullanın:

1. Uygulama ağ geçidi için genel IP adresini bulmak, **genel bakış** sayfası.![ Uygulama ağ geçidi genel IP adresi kaydı](./media/application-gateway-create-gateway-portal/application-gateway-record-ag-address.png)alternatif olarak, seçebileceğiniz **tüm kaynakları**, girin *myAGPublicIPAddress* arama kutusuna ve ardından aramaya seçin Sonuç. Azure üzerinde genel IP adresini görüntüler **genel bakış** sayfası.
2. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın.
3. Yanıt denetleyin. Uygulama ağ geçidi başarıyla oluşturuldu ve arka uç ile başarıyla bağlanabilmek için geçerli bir yanıt doğrular.![Uygulama ağ geçidini test etme](./media/application-gateway-create-gateway-portal/application-gateway-iistest.png)


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
