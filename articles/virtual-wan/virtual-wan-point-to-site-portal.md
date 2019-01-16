---
title: Azure Sanal WAN kullanarak Azure'a Noktadan Siteye bağlantı oluşturma | Microsoft Docs
description: Bu öğreticide Azure Sanal WAN kullanarak Azure'a Noktadan Siteye bağlantı oluşturmayı öğreneceksiniz.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 01/07/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect remote users to my VNets using Virtual WAN and I don't want to go through a Virtual WAN partner.
ms.openlocfilehash: 05f4b0b7accc3b13984864e82589c6fa639ce88d
ms.sourcegitcommit: 3ba9bb78e35c3c3c3c8991b64282f5001fd0a67b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2019
ms.locfileid: "54321662"
---
# <a name="tutorial-create-a-point-to-site-connection-using-azure-virtual-wan-preview"></a>Öğretici: Azure sanal WAN (Önizleme) kullanarak noktadan siteye bağlantı oluşturma

Bu öğreticide Sanal WAN kullanarak Azure'daki kaynaklarınıza bir IPsec/IKE (IKEv2) veya OpenVPN VPN bağlantısı üzerinden bağlanmayı öğreneceksiniz. Bu tür bir bağlantı, istemci bilgisayarda bir istemcinin yapılandırılmış olmasını gerektirir. Sanal WAN hakkında daha fazla bilgi için bkz. [Sanal WAN'a Genel Bakış](virtual-wan-about.md).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * WAN oluşturma
> * P2S yapılandırması oluşturma
> * Hub oluşturma
> * P2S yapılandırmasını bir hub'a uygulama
> * Bir sanal ağı bir hub'a bağlama
> * VPN istemci yapılandırmasını indirme ve uygulama
> * Sanal WAN'ınızı görüntüleme
> * Kaynak durumunu görüntüleme
> * Bir bağlantıyı izleme

> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [Before you begin](../../includes/virtual-wan-tutorial-vwan-before-include.md)]

## <a name="register"></a>Bu özelliği kaydedin

Bu özelliği Azure Cloud Shell kullanarak kolayca kaydetmek için **TryIt** ifadesine tıklayın. Bunun yerine PowerShell yerel olarak çalışır, en son sürümüne sahip ve oturum emin olun **Connect-AzureRmAccount** ve **Select-AzureRmSubscription** komutları.

>[!NOTE]
>Kaydettirmezseniz, özelliği kullanamaz veya portalda göremezsiniz.
>
>

Azure Cloud Shell'i açmak için **TryIt** ifadesine tıkladıktan sonra aşağıdaki komutları kopyalayıp yapıştırın:

```azurepowershell-interactive
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowP2SCortexAccess
```
 
```azurepowershell-interactive
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVnetGatewayOpenVpnProtocol
```

```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowP2SCortexAccess
```

```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVnetGatewayOpenVpnProtocol
```

Özellik kayıtlı olarak göründükten sonra aboneliği Microsoft.Network ad alanına yeniden kaydedin.

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

## <a name="vnet"></a>1. Sanal ağ oluşturma

[!INCLUDE [Create a virtual network](../../includes/virtual-wan-tutorial-vnet-include.md)]

## <a name="openvwan"></a>2. Sanal WAN oluşturma

Bir tarayıcıdan [Azure portala (Önizleme)](https://aka.ms/azurevirtualwanpreviewfeatures) gidin ve Azure hesabınızla oturum açın.

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-vwan-include.md)]

## <a name="hub"></a>3. Hub oluşturma

> [!NOTE]
> Bu adımda "Noktadan siteye ağ geçidini ekle" ayarını seçmeyin.
>

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-hub-include.md)]

## <a name="site"></a>4. P2S yapılandırması oluşturma

P2S yapılandırması, uzak istemcilerin bağlanmasına yönelik parametreleri tanımlar.

1. **Tüm kaynaklar**'a gidin.
2. Oluşturduğunuz sanal WAN'a tıklayın.
3. **Sanal WAN mimarisi** altında **Noktadan siteye yapılandırmalar**’a tıklayın.
4. Sayfanın üst kısmındaki **+Noktadan siteye yapılandırma ekle**’ye tıklayarak **Yeni noktadan siteye yapılandırma oluştur** sayfasını açın.
5. **Yeni noktadan siteye yapılandırma oluştur** sayfasında aşağıdaki alanları doldurun:

  *  **Yapılandırma adı**: Yapılandırmanıza vermek istediğiniz addır.
  *  **Tünel türü** - Tünel için kullanılacak protokol.
  *  **Adres havuzu** - İstemcilerin Is from atanacağı IP adresi havuzudur.
  *  **Kök Sertifika Adı** - Sertifika için açıklayıcı bir ad.
  *  **Kök Sertifika Verileri** - Base-64 kodlamalı X.509 sertifika verileri.

5. Yapılandırmayı oluşturmak için **Oluştur** seçeneğine tıklayın.

## <a name="hub"></a>5. Hub atamasını düzenleme

1. Sanal WAN'ınızın sayfasında **Hub'lar** öğesine tıklayın.
2. Noktadan siteye yapılandırmasını atamak istediğiniz hub'ı seçin.
3. **"..."** simgesine tıklayıp **Sanal hub'ı düzenle**'yi seçin.
4. **Noktadan siteye ağ geçidini dahil et** seçeneğini işaretleyin.
5. **Ağ geçidi ölçek birimleri**'ni ve **Noktadan siteye yapılandırması** ile istemciler için bir **Adres havuzu** seçin.
6. **Onayla**'ya tıklayın. 
7. İşlemi tamamlamak için en fazla 30 dakika sürebilir.

## <a name="vnet"></a>6. Sanal ağınızı bir hub'a bağlama

Bu adımda hub'ınızla bir sanal ağ arasında eşleme bağlantısı oluşturacaksınız. Bu adımları bağlanmak istediğiniz tüm sanal ağlar için tekrarlayın.

1. Sanal WAN'ınızın sayfasında **Sanal ağ bağlantısı**'na tıklayın.
2. Sanal ağ bağlantısı sayfasında **+Bağlantı ekle**'ye tıklayın.
3. **Bağlantı ekle** sayfasında aşağıdaki alanları doldurun:

    * **Bağlantı adı**: Bağlantınıza bir ad verin.
    * **Hub'lar**: Bu bağlantıyla ilişkilendirmek istediğiniz hub'ı seçin.
    * **Abonelik**: Aboneliği doğrulayın.
    * **Sanal ağ**: Bu hub'a bağlamak istediğiniz sanal ağı seçin. Sanal ağda önceden var olan bir sanal ağ geçidi bulunamaz.

## <a name="device"></a>7. VPN profili indirme

İstemcilerinizi yapılandırmak için VPN profilini kullanın.

1. Sanal WAN'ınızın sayfasında **Hub'lar** öğesine tıklayın.
2. Profilini indirmek istediğiniz hub'ı seçin.
3. **"..."** simgesine tıklayıp **Profili indir**'i seçin. 
4. Dosya oluşturulduktan sonra bağlantıya tıklayarak indirebilirsiniz.
4. Noktadan siteye istemcileri yapılandırmak için profil dosyasını kullanın.

## <a name="device"></a>8. Noktadan siteye istemcileri yapılandırma
Uzak erişim istemcilerini yapılandırmak için indirilen profili kullanın. Her işletim sisteminin yordamı farklıdır. Lütfen aşağıdaki doğru talimatları izleyin:

### <a name="microsoft-windows"></a>Microsoft Windows
#### <a name="openvpn"></a>OpenVPN

1.  Resmi web sitesinden OpenVPN istemcisini indirip yükleyin.
2.  Ağ geçidinin VPN profilini indirin. Bu, Azure portalında noktadan siteye yapılandırmaları sekme veya yeni AzureRmVpnClientConfiguration PowerShell'de yapılabilir.
3.  Profilin sıkıştırmasını açın. OpenVPN klasöründeki vpnconfig.ovpn yapılandırma dosyasını not defterinde açın.
4.  P2S istemci sertifikası bölümünü base64’teki P2S istemci sertifikası genel anahtarı ile doldurun. PEM biçimli bir sertifikada .cer dosyasını açıp base64 anahtarını sertifika üst bilgileri arasına kopyalamanız yeterlidir. Kodlanmış ortak anahtarı almak üzere sertifikayı dışarı aktarma işlemi için buraya bakın.
5.  Özel anahtar bölümünü, base64’teki P2S istemci sertifikası özel anahtarı ile doldurun. Özel anahtarın nasıl ayıklanacağını görmek için buraya bakın.
6.  Başka bir alanı değiştirmeyin. VPN’e bağlanmak için istemci girişinde doldurulmuş yapılandırmayı kullanın.
7.  vpnconfig.ovpn dosyasını C:\Program Files\OpenVPN\config klasörüne kopyalayın.
8.  Sistem tepsisindeki OpenVPN simgesine sağ tıklayın ve Bağlan’a tıklayın.

#### <a name="ikev2"></a>IKEv2

1. Windows bilgisayarın mimarisine karşılık gelen VPN istemcisi yapılandırma dosyalarını seçin. 64 bit işlemci mimarisi için 'VpnClientSetupAmd64' yükleyici paketini seçin. 32 bit işlemci mimarisi için 'VpnClientSetupX86' yükleyici paketini seçin.
2. Yüklemek için pakete çift tıklayın. Bir SmartScreen açılır penceresi görürseniz Daha fazla bilgi’ye ve ardından Yine de çalıştır’a tıklayın.
3. İstemci bilgisayarda Ağ Ayarları’na gidin ve VPN öğesine tıklayın. VPN bağlantısı, bağlandığı sanal ağın adını gösterir.
4. Bağlanmayı denemeden önce, istemci bilgisayara bir istemci sertifikası yüklediğinizi doğrulayın. Yerel Azure sertifika kimlik doğrulaması türü kullanılırken kimlik doğrulaması için bir istemci sertifikası gereklidir. Sertifika oluşturma hakkında daha fazla bilgi için bkz. Sertifika Oluşturma. Bir istemci sertifikasını yükleme hakkında daha fazla bilgi için bkz. İstemci sertifikası yükleme.

### <a name="mac-os-x"></a>Mac (OS X)
#### <a name="openvpn"></a>OpenVPN

1.  https://tunnelblick.net/downloads.html sayfasından TunnelBlik gibi bir OpenVPN istemcisi indirip yükleyin 
2.  Ağ geçidinin VPN profilini indirin. Bu noktadan siteye yapılandırma sekmesinde Azure portalında veya PowerShell New-AzureRmVpnClientConfiguration yapılabilir.
3.  Profilin sıkıştırmasını açın. OpenVPN klasöründeki vpnconfig.ovpn yapılandırma dosyasını not defterinde açın.
4.  P2S istemci sertifikası bölümünü base64’teki P2S istemci sertifikası genel anahtarı ile doldurun. PEM biçimli bir sertifikada .cer dosyasını açıp base64 anahtarını sertifika üst bilgileri arasına kopyalamanız yeterlidir. Kodlanmış ortak anahtarı almak üzere sertifikayı dışarı aktarma işlemi için buraya bakın.
5.  Özel anahtar bölümünü, base64’teki P2S istemci sertifikası özel anahtarı ile doldurun. Özel anahtarın nasıl ayıklanacağını görmek için buraya bakın.
6.  Başka bir alanı değiştirmeyin. VPN’e bağlanmak için istemci girişinde doldurulmuş yapılandırmayı kullanın.
7.  Profil dosyasına çift tıklayarak tunnelblik içinde profili oluşturun
8.  Uygulama klasöründen Tunnelblik uygulamasını başlatın
9.  Sistem tepsisindeki Tunneblik simgesine tıklayın ve Bağlan’ı seçin

#### <a name="ikev2"></a>IKEv2

Azure, yerel Azure sertifika kimlik doğrulaması için mobileconfig dosyası sağlamaz. Yerel IKEv2 VPN istemcisini Azure’a bağlanacak her Mac cihazda el ile yapılandırmanız gerekir. Genel klasöründe yapılandırma için gereken tüm bilgiler bulunur. İndirdiğiniz pakette Genel klasörünü görmüyorsanız, tünel türü olarak IKEv2 seçilmemiş olabilir. IKEv2 seçildikten sonra zip dosyasını tekrar oluşturarak Genel klasörünü alın. Genel klasörü aşağıdaki dosyaları içerir:

- Sunucu adresi ve tünel türü gibi önemli ayarları içeren VpnSettings.xml.
- P2S bağlantı ayarı sırasında Azure VPN Gateway’i doğrulamak için gereken kök sertifikayı içeren VpnServerRoot.cer.

## <a name="viewwan"></a>9. Sanal WAN'ınızı görüntüleme

1. Sanal WAN'a gidin.
2. Genel bakış sayfasında haritadaki her bir nokta bir hub'ı temsil eder. Hub sistem durumu özetini görüntülemek için noktalardan birinin üzerine gidin.
3. Hub'lar ve bağlantılar bölümünde herhangi bir hub'ın durumunu, sitesini, bölgesini, VPN bağlantısı durumunu ve gelen/giden baytları görüntüleyebilirsiniz.

## <a name="viewhealth"></a>10. Kaynak durumunu görüntüleme

1. WAN'ınıza gidin.
2. WAN sayfanızın **Destek ve sorun giderme** bölümünde **Sistem durumu**'na tıklayın ve kaynağınızı görüntüleyin.

## <a name="connectmon"></a>11. Bir bağlantıyı izleme

Bir Azure sanal makinesi ile uzak site arasındaki iletişimi izlemek için bir bağlantı oluşturun. Bağlantı izleyici oluşturma hakkında bilgi almak için bkz. [Ağ iletişimini izleme](~/articles/network-watcher/connection-monitor.md). Kaynak alan Azure'daki sanal makinenin IP adresi, hedef IP ise Sitenin IP adresidir.

## <a name="cleanup"></a>12. Kaynakları temizleme

Bu kaynaklar artık gerekli değilse, [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanarak kaynak grubunu ve içerdiği tüm kaynakları kaldırabilirsiniz. "myResourceGroup" yerine kaynak grubunuzun adını yazın ve aşağıdaki PowerShell komutunu çalıştırın:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * WAN oluşturma
> * Site oluşturma
> * Hub oluşturma
> * Bir hub'ı bir siteye bağlama
> * Bir sanal ağı bir hub'a bağlama
> * VPN cihazı yapılandırmasını indirme ve uygulama
> * Sanal WAN'ınızı görüntüleme
> * Kaynak durumunu görüntüleme
> * Bir bağlantıyı izleme

Sanal WAN hakkında daha fazla bilgi için [Sanal WAN'a Genel Bakış](virtual-wan-about.md) sayfasına bakın.
