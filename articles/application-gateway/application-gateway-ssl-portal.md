---
title: "SSL sonlandırma - Azure portal ile bir uygulama ağ geçidi oluşturma | Microsoft Docs"
description: "Bir uygulama ağ geçidi oluşturmak ve Azure portalını kullanarak SSL sonlandırma için bir sertifika eklemek öğrenin."
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/26/2018
ms.author: davidmu
ms.openlocfilehash: daab3ada5ef0cc20883130e4c12b1dc3570e63b1
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="create-an-application-gateway-with-ssl-termination-using-the-azure-portal"></a>İle SSL sonlandırma Azure portalını kullanarak bir uygulama ağ geçidi oluşturma

Azure Portalı'nı oluşturmak için kullanabileceğiniz bir [uygulama ağ geçidi](application-gateway-introduction.md) arka uç sunucuları için sanal makineler kullanan SSL sonlandırma için bir sertifika ile.

Bu makalede, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Otomatik olarak imzalanan sertifika oluşturma
> * Sertifika ile bir uygulama ağ geçidi oluşturma
> * Arka uç sunucuları olarak kullanılan sanal makine oluşturma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Oturum açtığınızda Azure portalında [http://portal.azure.com](http://portal.azure.com)

## <a name="create-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika oluşturma

Bu bölümde, kullandığınız [yeni SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/new-selfsignedcertificate) uygulama ağ geçidi için dinleyici oluşturduğunuzda, Azure portalına karşıya otomatik olarak imzalanan bir sertifika oluşturmak için.

Yerel bilgisayarınızda, bir yönetici olarak bir Windows PowerShell penceresi açın. Bir sertifika oluşturmak için aşağıdaki komutu çalıştırın:

```powershell
New-SelfSignedCertificate \
  -certstorelocation cert:\localmachine\my \
  -dnsname www.contoso.com
```

Bu yanıt gibi bir şey görmeniz gerekir:

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

Bir sanal ağ, oluşturduğunuz kaynakları arasındaki iletişim için gereklidir. Bu örnekte, iki alt ağ oluşturulur: bir uygulama ağ geçidi ve arka uç sunucular için diğer için. Uygulama ağ geçidi oluşturma aynı anda bir sanal ağ oluşturabilirsiniz.

1. Tıklatın **yeni** Azure portalında sol üst köşesinde bulundu.
2. Seçin **ağ** ve ardından **uygulama ağ geçidi** öne çıkan listesinde.
3. Girin *myAppGateway* uygulama ağ geçidi adını ve *myResourceGroupAG* yeni kaynak grubu için.
4. Diğer ayarlar için varsayılan değerleri kabul edin ve ardından **Tamam**.
5. Tıklatın **sanal ağ seçin**, tıklatın **Yeni Oluştur**ve ardından sanal ağ için bu değerleri girin:

    - *myVNet* - sanal ağın adı.
    - *10.0.0.0/16* - sanal ağ adres alanı.
    - *myAGSubnet* - alt ağ adı.
    - *10.0.0.0/24* - alt ağ adres alanı.

    ![Sanal ağ oluşturma](./media/application-gateway-ssl-portal/application-gateway-vnet.png)

6. Tıklatın **Tamam** sanal ağ ve alt ağ oluşturmak için.
7. Tıklatın **genel bir IP adresi seçin**, tıklatın **Yeni Oluştur**ve ortak IP adresini girin. Bu örnekte adlı ortak IP adresi *myAGPublicIPAddress*. Diğer ayarlar için varsayılan değerleri kabul edin ve ardından **Tamam**.
8. Tıklatın **HTTPS** dinleyici protokolü ve bağlantı noktası olarak tanımlandığından emin olun **443**.
9. Klasör simgesine tıklayın ve Gözat *appgwcert.pfx* dosyayı karşıya yüklemeyi daha önce oluşturduğunuz sertifika.
10. Girin *mycert1* sertifika adını ve *Azure123456!* Parola ve ardından **Tamam**.

    ![Yeni uygulama ağ geçidi oluşturma](./media/application-gateway-ssl-portal/application-gateway-create.png)

11. Özet sayfasında ayarları gözden geçirin ve ardından **Tamam** ağ kaynaklarını ve uygulama ağ geçidi oluşturmak için. Oluşturulması, dağıtımı, sonraki bölüme geçmeden önce başarıyla tamamlanana kadar bekleyin uygulama ağ geçidi için birkaç dakika sürebilir.

### <a name="add-a-subnet"></a>Bir alt ağ Ekle

1. Tıklatın **tüm kaynakları** sol taraftaki menüyü ve ardından **myVNet** kaynakları listesinden.
2. Tıklatın **alt ağlar**ve ardından **alt**.

    ![Alt ağ oluşturma](./media/application-gateway-ssl-portal/application-gateway-subnet.png)

3. Girin *myBackendSubnet* 'ye tıklayın ve alt ağ adı için **Tamam**.

## <a name="create-backend-servers"></a>Arka uç sunucuları oluşturun

Bu örnekte uygulama ağ geçidi için arka uç sunucuları olarak kullanılacak iki sanal makine oluşturun. Ayrıca uygulama ağ geçidi başarıyla oluşturulduğunu doğrulamak için sanal makinelerde IIS yükleyin.

### <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

1. **Yeni**’ye tıklayın.
2. Tıklatın **işlem** ve ardından **Windows Server 2016 Datacenter** öne çıkan listesinde.
3. Sanal makine için bu değerleri girin:

    - *myVM* - sanal makine adı için.
    - *azureuser* - yönetici kullanıcı adı.
    - *Azure123456!* parolası.
    - Seçin **var olanı kullan**ve ardından *myResourceGroupAG*.

4. **Tamam**’a tıklayın.
5. Seçin **DS1_V2** tıklatın ve sanal makine boyutu için **seçin**.
6. Olduğundan emin olun **myVNet** sanal ağ ve alt ağ için seçili olan **myBackendSubnet**. 
7. Tıklatın **devre dışı** önyükleme tanılaması devre dışı bırakmak için.
8. Tıklatın **Tamam**, Özet sayfasında ayarları gözden geçirin ve ardından **oluşturma**.

### <a name="install-iis"></a>IIS yükleme

1. Etkileşimli Kabuğu'nu açın ve onu ayarlandığından emin olun **PowerShell**.

    ![Özel uzantısını yükleyin](./media/application-gateway-ssl-portal/application-gateway-extension.png)

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
4. Tıklatın **arka uç havuzları**. Varsayılan bir havuzu uygulama ağ geçidi ile otomatik olarak oluşturuldu. Tıklatın **appGateayBackendPool**.
5. Tıklatın **Ekle hedef** oluşturduğunuz her bir sanal makine arka uç havuzuna eklemek için.

    ![Arka uç sunucuları ekleme](./media/application-gateway-ssl-portal/application-gateway-backend.png)

6. **Kaydet**’e tıklayın.

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidi sınama

1. Tıklatın **tüm kaynakları**ve ardından **myAGPublicIPAddress**.

    ![Uygulama ağ geçidi genel IP adresi kaydı](./media/application-gateway-ssl-portal/application-gateway-ag-address.png)

2. Genel IP adresini kopyalayın ve ardından, tarayıcınızın adres çubuğuna yapıştırın. Kendinden imzalı bir sertifika kullanıyorsa uyarı güvenlik kabul etmek için Ayrıntılar'ı seçin ve ardından Web sayfasına gidin:

    ![Güvenli uyarı](./media/application-gateway-ssl-portal/application-gateway-secure.png)

    Güvenli, IIS Web sitesi sonra aşağıdaki örnekte olduğu gibi görüntülenir:

    ![Temel uygulama ağ geçidi URL'de test](./media/application-gateway-ssl-portal/application-gateway-iistest.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Otomatik olarak imzalanan sertifika oluşturma
> * Sertifika ile bir uygulama ağ geçidi oluşturma
> * Arka uç sunucuları olarak kullanılan sanal makine oluşturma

Uygulama ağ geçitleri ile ilişkili kaynakları hakkında daha fazla bilgi için nasıl yapılır makaleleri devam edin.