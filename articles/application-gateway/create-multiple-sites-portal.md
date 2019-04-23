---
title: Öğretici - Azure portalını kullanarak birden çok web sitesini barındıran bir uygulama ağ geçidi oluşturma
description: Bu öğreticide, Azure portalını kullanarak birden çok web sitesini barındıran bir uygulama ağ geçidi oluşturma konusunda bilgi edinin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: tutorial
ms.date: 4/18/2019
ms.author: victorh
ms.openlocfilehash: 3e27a79c7a6e3d39679118f532dd464a32463d69
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59999034"
---
# <a name="tutorial-create-and-configure-an-application-gateway-to-host-multiple-web-sites-using-the-azure-portal"></a>Öğretici: Oluşturma ve Azure portalını kullanarak birden çok web sitelerini barındırmak için bir uygulama ağ geçidi yapılandırma

Azure portalında kullandığınız [birden çok web sitesini barındırmayı yapılandırma](multiple-site-overview.md) oluşturduğunuzda bir [uygulama ağ geçidi](overview.md). Bu öğreticide, sanal makineleri kullanarak arka uç adres havuzları tanımlayın. Ardından sahip olduğunuz dinleyicileri ve kuralları, web trafiğinin havuzlardaki uygun sunuculara ulaşması için yapılandırırsınız. Bu öğreticide birden çok etki alanına sahip olduğunuz varsayılır ve *www.contoso.com* ve *www.fabrikam.com* örnekleri kullanılır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Uygulama ağ geçidi oluşturma
> * Arka uç sunucuları için sanal makineler oluşturun
> * Arka uç havuzları ile arka uç sunucular oluşturma
> * Arka uç dinleyicileri oluşturma
> * Yönlendirme kuralları oluşturma
> * Etki alanınızda bir CNAME kaydı oluşturma

![Çok siteli yönlendirme örneği](./media/create-multiple-sites-portal/scenario.png)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Bir sanal ağ, oluşturduğunuz kaynakları arasındaki iletişim için gereklidir. Bu örnekte iki alt ağ oluşturulmuştur: biri uygulama ağ geçidi ve diğeri de arka uç sunucuları içindir. Uygulama ağ geçidini oluştururken aynı zamanda bir sanal makine oluşturabilirsiniz.

1. Tıklayın **yeni** Azure portalının sol üst köşedeki üzerinde bulunamadı.
2. **Ağ** ve ardından Öne Çıkanlar listesinde **Application Gateway**’i seçin.
3. Uygulama ağ geçidi için şu değerleri girin:

   - *myAppGateway* - Uygulama ağ geçidinin adı.
   - *myResourceGroupAG* - Yeni kaynak grubu.

     ![Yeni uygulama ağ geçidi oluşturma](./media/create-multiple-sites-portal/application-gateway-create.png)

4. Diğer ayarların varsayılan değerlerini kabul edin ve sonra **Tamam**’a tıklayın.
5. Tıklayın **bir sanal ağ seçin**, tıklayın **Yeni Oluştur**ve ardından sanal ağ için şu değerleri girin:

   - *myVNet* - Sanal ağın adı.
   - *10.0.0.0/16* - Sanal ağın adres alanı.
   - *myAGSubnet* - Alt ağın adı.
   - *10.0.0.0/24* - Alt ağın adres alanı.

     ![Sanal ağ oluşturma](./media/create-multiple-sites-portal/application-gateway-vnet.png)

6. Sanal ağı ve alt ağı oluşturmak için **Tamam**’a tıklayın.
7. Tıklayın **genel bir IP adresi seçin**, tıklayın **Yeni Oluştur**ve ardından genel IP adresini adını girin. Bu örnekte genel IP adresinin adı *myAGPublicIPAddress* şeklindedir. Diğer ayarların varsayılan değerlerini kabul edin ve sonra **Tamam**’a tıklayın.
8. Dinleyici yapılandırması için varsayılan değerleri kabul edin, Web uygulaması güvenlik duvarı devre dışı bırakın ve ardından **Tamam**.
9. Özet sayfasında ayarları gözden geçirin ve ardından **Tamam** ağ kaynaklarının ve uygulama ağ geçidi oluşturmak için. Bu, uygulama ağ geçidinin oluşturulması, sonraki bölüme geçmeden önce dağıtımın başarıyla tamamlanana kadar bekleyin birkaç dakika sürebilir.

### <a name="add-a-subnet"></a>Alt ağ ekleme

1. Sol taraftaki menüde **Tüm kaynaklar**’a ve sonra kaynaklar listesinden **myVNet** öğesine tıklayın.
2. Tıklayın **alt ağlar**ve ardından **alt**.

    ![Alt ağ oluşturma](./media/create-multiple-sites-portal/application-gateway-subnet.png)

3. Alt ağ adı için *myBackendSubnet* girin ve sonra **Tamam**’a tıklayın.

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Bu örnekte, uygulama ağ geçidi için arka uç sunucular olarak kullanılacak iki sanal makine oluşturacaksınız. Ayrıca trafiği doğru yönlendirmeyi doğrulamak için sanal makinelere IIS yüklersiniz.

1. **Yeni**’ye tıklayın.
2. Tıklayın **işlem** seçip **Windows Server 2016 Datacenter** Öne çıkanlar listesinde.
3. Sanal makine için şu değerleri girin:

    - *contosoVM* - sanal makinenin adı.
    - Yönetici kullanıcı adı için *azureuser*.
    - *Azure123456!* Parola.
    - **Mevcut olanı kullan**’ı seçin ve *myResourceGroupAG* seçeneğini belirleyin.

4. **Tamam** düğmesine tıklayın.
5. Sanal makinenin boyutu için **DS1_V2** seçeneğini belirleyin ve **Seç**’e tıklayın.
6. Sanal ağ için **myVNet** öğesinin seçili olduğundan ve alt ağın **myBackendSubnet** olduğundan emin olun. 
7. Önyükleme tanılamalarını devre dışı bırakmak için **Devre Dışı** seçeneğine tıklayın.
8. **Tamam**’a tıklayın, özet sayfasındaki ayarları gözden geçirin ve sonra **Oluştur**’a tıklayın.

### <a name="install-iis"></a>IIS yükleme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Etkileşimli kabuğu açın ve **PowerShell**’e ayarlandığından emin olun.

    ![Özel uzantıyı yükleme](./media/create-multiple-sites-portal/application-gateway-extension.png)

2. Sanal makineye IIS yüklemek için aşağıdaki komutu çalıştırın: 

    ```azurepowershell-interactive
    $publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/appgatewayurl.ps1");  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }
    Set-AzVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -Location eastus `
      -ExtensionName IIS `
      -VMName contosoVM `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -Settings $publicSettings
    ```

3. İkinci sanal makine oluşturun ve yalnızca tamamlanmış adımları kullanarak IIS yükleyin. Adlarını girin *fabrikamVM* adı ve Set-AzVMExtension içinde VMName değeri.

## <a name="create-backend-pools-with-the-virtual-machines"></a>Arka uç havuzları ile sanal makineleri oluşturma

1. Tıklayın **tüm kaynakları** ve ardından **myAppGateway**.
2. Tıklayın **arka uç havuzları**ve ardından **Ekle**.
3. Bir ad girin *contosoPool* ve ekleme *contosoVM* kullanarak **Ekle hedef**.

    ![Arka uç sunucuları ekleme](./media/create-multiple-sites-portal/application-gateway-multisite-backendpool.png)

4. **Tamam** düğmesine tıklayın.
5. Tıklayın **arka uç havuzları** ve ardından **Ekle**.
6. Oluşturma *fabrikamPool* ile *fabrikamVM* yalnızca tamamlanmış adımları kullanarak.

## <a name="create-backend-listeners"></a>Arka uç dinleyicileri oluşturma

1. Tıklayın **dinleyicileri** ve ardından **çok siteli**.
2. Dinleyici için şu değerleri girin:
    
   - *contosoListener* - dinleyicisinin adını.
   - *www.contoso.com* -bu konak adı örneği, etki alanı adıyla değiştirin.

3. **Tamam** düğmesine tıklayın.
4. Adını kullanarak ikinci bir dinleyici oluşturun *fabrikamListener* ve ikinci etki alanı adınızı kullanın. Bu örnekte, *www.fabrikam.com* kullanılır.

![birden çok siteli dinleyicileri](media/create-multiple-sites-portal/be-listeners.png)

## <a name="create-routing-rules"></a>Yönlendirme kuralları oluşturma

Kurallar listelendikleri sırayla işlenir ve trafik belirginlikten bağımsız olarak eşleşen ilk kural kullanarak yönlendirilir. Örneğin, aynı bağlantı noktasında temel bir dinleyici kullanan bir kuralınız ve çok siteli dinleyici kullanan bir kuralınız varsa çok siteli kuralın beklendiği gibi çalışması için çok siteli dinleyicinin kuralı temel dinleyici kuralından önce listelenmelidir. 

Bu örnekte, iki yeni kurallar oluşturabilir ve uygulama ağ geçidi oluşturduğunuzda oluşturulan varsayılan kuralını silin.

1. Tıklayın **kuralları** ve ardından **temel**.
2. Girin *contosoRule* adı.
3. Seçin *contosoListener* dinleyici için.
4. Seçin *contosoPool* arka uç havuzu için.

    ![Yol tabanlı kural oluşturma](./media/create-multiple-sites-portal/application-gateway-multisite-rule.png)

5. **Tamam** düğmesine tıklayın.
6. Adlarını kullanarak ikinci bir kural oluşturun *fabrikamRule*, *fabrikamListener*, ve *fabrikamPool*.
7. Adlı varsayılan kuralı *bağlanma1* tıklayarak ve ardından tıklatarak **Sil**.

## <a name="create-a-cname-record-in-your-domain"></a>Etki alanınızda bir CNAME kaydı oluşturma

Uygulama ağ geçidi genel IP adresiyle oluşturulduktan sonra DNS adresini alabilir ve etki alanınızda bir CNAME kaydı oluşturmak için kullanabilirsiniz. Uygulama ağ geçidi yeniden başlatıldığında VIP değişebileceğinden A kayıtlarının kullanımı önerilmez.

1. Tıklayın **tüm kaynakları**ve ardından **myAGPublicIPAddress**.

    ![Kayıt uygulama ağ geçidi DNS adresi](./media/create-multiple-sites-portal/application-gateway-multisite-dns.png)

2. DNS adresi kopyalayın ve yeni bir CNAME kaydını etki alanınızda değeri olarak kullanın.

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

1. Tarayıcınızın adres çubuğuna, etki alanı adınızı girin. Örneğin http://www.contoso.com.

    ![Uygulama ağ geçidinde contoso test etme](./media/create-multiple-sites-portal/application-gateway-iistest.png)

2. Adresi diğer etki alanınızla değiştirin, aşağıdaki örneğe benzer bir şey görmeniz gerekir:

    ![Uygulama ağ geçidinde fabrikam sitesini test etme](./media/create-multiple-sites-portal/application-gateway-iistest2.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Application gateway ile oluşturduğunuz kaynaklara artık ihtiyacınız olduğunda, kaynak grubunu kaldırın. Kaynak grubu kaldırarak, ayrıca uygulama ağ geçidi ve tüm ilgili kaynakları kaldırın.

Kaynak grubunu kaldırmak için:

1. Azure portal'ın sol menüsünde **kaynak grupları**.
2. Üzerinde **kaynak grupları** sayfasında, arama **myResourceGroupAG** listesinde seçin.
3. Üzerinde **kaynak grubu sayfasını**seçin **kaynak grubunu Sil**.
4. Girin *myResourceGroupAG* için **kaynak grubu adını yazın** seçip **Sil**

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Application Gateway ile yapabilecekleriniz hakkında daha fazla bilgi edinin](application-gateway-introduction.md)