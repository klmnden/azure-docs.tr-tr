---
title: Azure VPN ağ geçidi için OpenVPN istemcileri yapılandırma | Microsoft Docs
description: Azure VPN ağ geçidi için OpenVPN istemcileri yapılandırma adımları
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
ms.openlocfilehash: 141f29ea1c7ebdd8208dda6bc39a60f7873dab7d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46977851"
---
# <a name="configure-openvpn-clients-for-azure-vpn-gateway-preview"></a>Azure VPN ağ geçidi (Önizleme) için OpenVPN istemcilerini yapılandırma

Bu makalede OpenVPN istemcileri yapılandırmanıza yardımcı olur.

> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Başlamadan önce

VPN ağ geçidiniz OpenVPN yapılandırma adımları tamamladığınızdan emin olun. Ayrıntılar için bkz [Azure VPN ağ geçidi için yapılandırma OpenVPN](vpn-gateway-howto-openvpn.md).

## <a name="windows"></a>Windows istemcileri

1. OpenVPN istemci resmi yükleyip [OpenVPN Web sitesi](https://openvpn.net/index.php/open-source/downloads.html).
2. Ağ geçidi için VPN profilini indirin. Bu, Azure portalında, noktadan siteye yapılandırma sekmesinden yapılabilir ya da ' New-AzureRmVpnClientConfiguration' PowerShell'de.
3. Profil sıkıştırmasını açın. Ardından, Not Defteri OpenVPN klasöründeki vpnconfig.ovpn yapılandırma dosyasını açın.
4. P2S sertifika konusunun istemcisi P2S istemci sertifika genel anahtarı Base64 ile doldurun. PEM biçimli sertifika sertifika üstbilgileri arasında base64 anahtar üzerinden yalnızca kopyalama ve .cer dosyasını açabilirsiniz. Kodlanmış ortak anahtarı almak için bir sertifikayı dışarı aktarma buraya bakın.
5. Özel anahtar bölümü, P2S istemci sertifikası özel anahtarı Base64 ile doldurun. Özel anahtar ayıklama hakkında daha fazla bilgi için bkz: [anahtarı dışarı aktar](vpn-gateway-certificates-point-to-site.md#clientexport).
6. Diğer alanlar değiştirmeyin. Doldurulmuş istemci giriş yapılandırmasında VPN'ye bağlanmak için kullanın.
7. Vpnconfig.ovpn dosyası C:\Program Files\OpenVPN\config klasörüne kopyalayın.
8. Bağlantı tıklayıp sistem tepsisi simgesi OpenVPN sağ tıklayın.

## <a name="mac"></a>Mac istemcileri

1. Bir OpenVPN istemci gibi yükleyip [TunnelBlik](https://tunnelblick.net/downloads.html). 
2. Ağ geçidi için VPN profilini indirin. Bu, Azure portalında ya da 'New-AzureRmVpnClientConfiguration' PowerShell kullanarak noktadan siteye yapılandırma sekmesinden yapılabilir.
3. Profil sıkıştırmasını açın. Not Defteri'ni OpenVPN klasöründeki vpnconfig.ovpn yapılandırma dosyasını açın.
4. P2S sertifika konusunun istemcisi P2S istemci sertifika genel anahtarı Base64 ile doldurun. PEM biçimli sertifika sertifika üstbilgileri arasında base64 anahtar üzerinden yalnızca kopyalama ve .cer dosyasını açabilirsiniz. Bkz: [ortak anahtarını dışarı aktarmak](vpn-gateway-certificates-point-to-site.md#cer) kodlanmış ortak anahtarı almak için bir sertifikayı dışarı aktarma hakkında bilgi için.
5. Özel anahtar bölümü, P2S istemci sertifikası özel anahtarı Base64 ile doldurun. Bkz: [, özel anahtarı dışarı aktar](https://www.geotrust.eu/en/support/manuals/microsoft/all+windows+servers/export+private+key+or+certificate/) özel anahtarınızı ayıklamanız hakkında bilgi için.
6. Diğer alanlar değiştirmeyin. Doldurulmuş istemci giriş yapılandırmasında VPN'ye bağlanmak için kullanın.
7. İçinde tunnelblik profili oluşturmak için profili dosyasına çift tıklayın.
8. Uygulamaları klasöründen Tunnelblik başlatın.
9. Sistem tepsisindeki Tunneblik simgesine tıklayın ve çekme bağlanın.

## <a name="linux"></a>Linux istemcileri

1. Yeni bir Terminal oturumu açın. Yeni bir oturum 'Ctrl + Alt + t' tuşlarına basarak aynı anda açabilirsiniz
2. Gerekli bileşenleri yüklemek için aşağıdaki komutu girin:

  ```
  sudo apt-get install openvpn
  sudo apt-get -y install network-manager-openvpn
  sudo service network-manager restart
  ```
3. Ağ geçidi için VPN profilini indirin. Bu, Azure portalında ya da 'New-AzureRmVpnClientConfiguration' PowerShell kullanarak noktadan siteye yapılandırma sekmesinden yapılabilir.
4. Özel anahtar bölümü, P2S istemci sertifikası özel anahtarı Base64 ile doldurun. Bkz: [, özel anahtarı dışarı aktar](https://www.geotrust.eu/en/support/manuals/microsoft/all+windows+servers/export+private+key+or+certificate/) özel anahtarınızı ayıklamanız hakkında bilgi için.
5. Komut satırını kullanarak bağlanmak için aşağıdaki komutu yazın:
  
  ```
  Sudo openvpn –config <name and path of your VPN profile file>
  ```
5. GUI kullanarak bağlanmak için sistem ayarlarına gidin.
6. Tıklayın **+** yeni VPN bağlantısı eklemek için.
7. Altında **ekleme VPN**, çekme **dosyadan içeri aktar...**
8. Profil dosyasını ve çift tıklatın veya çekme göz atın **Aç**
9. Tıklayın **Ekle** üzerinde **ekleme VPN** penceresi.
  
  ![Dosyadan içeri aktar](./media/vpn-gateway-howto-openvpn-clients/importfromfile.png)
10. VPN açarak bağlanabilir **ON** üzerinde **ağ ayarlarını** sayfasında, ya da sistem tepsisindeki ağ simgesinin altında.

## <a name="next-steps"></a>Sonraki adımlar

Başka bir vnet (üretim) kaynaklara erişebilmesi için VPN istemcileri istiyorsanız, ardından ilgili yönergeleri uygulayın [Vnet-VNet](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) vnet-vnet bağlantı kurmak için makaleyi. Lütfen ağ geçitleriniz ve bağlantılarınızı üzerinde BGP'yi etkinleştirmek emin olun, aksi takdirde trafik değil akar.
