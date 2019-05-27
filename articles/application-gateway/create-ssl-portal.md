---
title: Öğretici - SSL sonlandırma - Azure portalı ile bir uygulama ağ geçidi yapılandırma
description: Bu öğreticide, bir uygulama ağ geçidi ve Azure portalını kullanarak SSL sonlandırma için bir sertifika ekleme konusunda bilgi edinin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: tutorial
ms.date: 4/17/2019
ms.author: victorh
ms.openlocfilehash: f3ba3eb12dc85a72c4e49c374e62209b83400d33
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66134494"
---
# <a name="tutorial-configure-an-application-gateway-with-ssl-termination-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak SSL sonlandırma ile bir uygulama ağ geçidi yapılandırma

Azure portalında yapılandırmak için kullanabileceğiniz bir [uygulama ağ geçidi](overview.md) arka uç sunucuları için sanal makineler kullanan SSL sonlandırması için bir sertifika ile.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Otomatik olarak imzalanan sertifika oluşturma
> * Sertifikalı bir uygulama ağ geçidi oluşturma
> * Arka uç sunucular olarak kullanılan sanal makine oluşturma
> * Uygulama ağ geçidini test etme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="sign-in-to-azure"></a>Oturum açın: Azure

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika oluşturma

Bu bölümde, kullandığınız [New-SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/new-selfsignedcertificate) otomatik olarak imzalanan bir sertifika oluşturmak için. Dinleyici için uygulama ağ geçidi oluşturduğunuzda, Azure portalında sertifikayı yükleyin.

Yerel bilgisayarınızda yönetici olarak bir Windows PowerShell penceresi açın. Bir sertifika oluşturmak için aşağıdaki komutu çalıştırın:

```powershell
New-SelfSignedCertificate \
  -certstorelocation cert:\localmachine\my \
  -dnsname www.contoso.com
```

Bu yanıt benzer bir şey görmeniz gerekir:

```
PSParentPath: Microsoft.PowerShell.Security\Certificate::LocalMachine\my

Thumbprint                                Subject
----------                                -------
E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630  CN=www.contoso.com

Use [Export-PfxCertificate](https://docs.microsoft.com/powershell/module/pkiclient/export-pfxcertificate) with the Thumbprint that was returned to export a pfx file from the certificate:
```

```powershell
$pwd = ConvertTo-SecureString -String "Azure123456!" -Force -AsPlainText
Export-PfxCertificate \
  -cert cert:\localMachine\my\E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630 \
  -FilePath c:\appgwcert.pfx \
  -Password $pwd
```

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Bir sanal ağ, oluşturduğunuz kaynakları arasındaki iletişim için gereklidir. Bu örnekte iki alt ağ oluşturulmuştur: biri uygulama ağ geçidi ve diğeri de arka uç sunucuları içindir. Uygulama ağ geçidini oluştururken aynı zamanda bir sanal makine oluşturabilirsiniz.

1. Seçin **yeni** Azure portalının sol üst köşedeki üzerinde bulunamadı.
2. **Ağ** ve ardından Öne Çıkanlar listesinde **Application Gateway**’i seçin.
3. Girin *myAppGateway* uygulama ağ geçidinin adını ve *myResourceGroupAG* yeni kaynak grubu için.
4. Diğer ayarlar için varsayılan değerleri kabul edin ve ardından **Tamam**.
5. Seçin **bir sanal ağ seçin**seçin **Yeni Oluştur**ve ardından sanal ağ için şu değerleri girin:

   - *myVNet* - Sanal ağın adı.
   - *10.0.0.0/16* - Sanal ağın adres alanı.
   - *myAGSubnet* - Alt ağın adı.
   - *10.0.0.0/24* - Alt ağın adres alanı.

     ![Sanal ağ oluştur](./media/create-ssl-portal/application-gateway-vnet.png)

6. Seçin **Tamam** sanal ağ ve alt ağ oluşturmak için.
7. Seçin **genel bir IP adresi seçin**seçin **Yeni Oluştur**ve ardından genel IP adresini adını girin. Bu örnekte genel IP adresinin adı *myAGPublicIPAddress* şeklindedir. Diğer ayarlar için varsayılan değerleri kabul edin ve ardından **Tamam**.
8. Seçin **HTTPS** dinleyici protokolü ve bağlantı noktası olarak tanımlandığından emin olun **443**.
9. Klasör simgesini seçin ve göz atın *appgwcert.pfx* karşıya yüklemek için daha önce oluşturduğunuz sertifika.
10. Girin *mycert1* sertifika adını ve *Azure123456!* Parola ve ardından **Tamam**.

    ![Yeni uygulama ağ geçidi oluşturma](./media/create-ssl-portal/application-gateway-create.png)

11. Özet sayfasında ayarları gözden geçirin ve ardından **Tamam** ağ kaynaklarının ve uygulama ağ geçidi oluşturmak için. Bu, uygulama ağ geçidinin oluşturulması, sonraki bölüme geçmeden önce dağıtımın başarıyla tamamlanana kadar bekleyin birkaç dakika sürebilir.

### <a name="add-a-subnet"></a>Alt ağ ekleme

1. Seçin **tüm kaynakları** seçin ve soldaki menüden **myVNet** ve kaynak listesinden.
2. Seçin **alt ağlar**ve ardından **alt**.

    ![Alt ağ oluştur](./media/create-ssl-portal/application-gateway-subnet.png)

3. Girin *myBackendSubnet* seçin ve alt ağ adı için **Tamam**.

## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu örnekte, application gateway için arka uç sunucular olarak kullanılan iki sanal makine oluşturun. Ayrıca uygulama ağ geçidinin başarıyla oluşturulduğunu doğrulamak için sanal makinelere IIS yüklersiniz.

### <a name="create-a-virtual-machine"></a>Bir sanal makine oluştur

1. **Yeni**'yi seçin.
2. **İşlem**’i ve sonra Öne Çıkanlar listesinde **Windows Server 2016 Datacenter**’ı seçin.
3. Sanal makine için şu değerleri girin:

    - *myVM* - Sanal makinenin adı.
    - Yönetici kullanıcı adı için *azureuser*.
    - *Azure123456!* Parola.
    - **Mevcut olanı kullan**’ı seçin ve *myResourceGroupAG* seçeneğini belirleyin.

4. **Tamam**’ı seçin.
5. Seçin **DS1_V2** seçin ve sanal makine boyutu için **seçin**.
6. Sanal ağ için **myVNet** öğesinin seçili olduğundan ve alt ağın **myBackendSubnet** olduğundan emin olun. 
7. Önyükleme tanılamalarını devre dışı bırakmak için **Devre Dışı** seçeneğini belirleyin.
8. **Tamam**’a tıklayın, özet sayfasındaki ayarları gözden geçirin ve sonra **Oluştur**’u seçin.

### <a name="install-iis"></a>IIS yükleme

1. Etkileşimli bir kabuk açın ve bu ayarlandığından emin olun **PowerShell**.

    ![Özel uzantıyı yükleme](./media/create-ssl-portal/application-gateway-extension.png)

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

3. İkinci bir sanal makine oluşturun ve yeni tamamladığınız adımları kullanarak IIS yükleyin. Girin *myVM2* adını ve Set-AzVMExtension içinde VMName.

### <a name="add-backend-servers"></a>Arka uç sunucuları ekleme

1. Seçin **tüm kaynakları**ve ardından **myAppGateway**.
1. Seçin **arka uç havuzları**. Uygulama ağ geçidi ile varsayılan bir havuz otomatik olarak oluşturulur. Seçin **appGatewayBackendPool**.
1. Seçin **Ekle hedef** arka uç havuzu için oluşturduğunuz her sanal makineye eklenecek.

    ![Arka uç sunucuları ekleme](./media/create-ssl-portal/application-gateway-backend.png)

1. **Kaydet**’i seçin.

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

1. Seçin **tüm kaynakları**ve ardından **myAGPublicIPAddress**.

    ![Uygulama ağ geçidi genel IP adresini kaydetme](./media/create-ssl-portal/application-gateway-ag-address.png)

2. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın. Otomatik olarak imzalanan bir sertifika kullandıysanız güvenlik uyarısını kabul edin ve ardından Web sayfasına gidin ayrıntıları seçmek için:

    ![Güvenli uyarı](./media/create-ssl-portal/application-gateway-secure.png)

    Güvenli IIS siteniz, sonra aşağıdaki örnekte olduğu gibi görüntülenir:

    ![Temel URL’yi uygulama ağ geçidinde test etme](./media/create-ssl-portal/application-gateway-iistest.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Application Gateway ile yapabilecekleriniz hakkında daha fazla bilgi edinin](application-gateway-introduction.md)
