---
title: Azure VPN ağ geçidi için OpenVPN istemcileri yapılandırma | Microsoft Docs
description: Azure VPN ağ geçidi için OpenVPN istemcileri yapılandırma adımları
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: cherylmc
ms.openlocfilehash: 7d76827a4e4ba4ccb7a146c5f8282000d143eb35
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51624446"
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
2. Ağ geçidinin VPN profilini indirin. Bu noktadan siteye yapılandırma sekmesinde Azure portalında ya da 'New-AzureRmVpnClientConfiguration' PowerShell'de yapılabilir.
3. Profilin sıkıştırmasını açın. Ardından, açık *vpnconfig.ovpn* Not Defteri'ni kullanarak OpenVPN klasöründen yapılandırma dosyası.
4. [Dışarı aktarma](vpn-gateway-certificates-point-to-site.md#clientexport) oluşturduğunuz ve ağ geçidi P2S yapılandırmanıza karşıya sertifika P2S istemci.
5. Özel anahtarı ve base64 parmak izini ayıklamak *.pfx*. Bunu yapmanın birden çok yolu vardır. Makinenizde OpenSSL kullanarak bir yoludur. *Profileinfo.txt* dosyası, özel anahtarı ve parmak izini CA ve istemci sertifikasını içerir. İstemci sertifikası parmak izi kullandığınızdan emin olun.

  ```
  openssl.exe pkcs12 -in "filename.pfx" -nodes -out "profileinfo.txt"
  ```
6. Açık *profileinfo.txt* Defteri'nde. İstemci (alt) sertifikanın parmak izini edinmek için metni seçin (dahil olmak üzere ve arasında) "---BEGIN CERTIFICATE---" ve "---bitiş sertifika---" için alt sertifika ve kopyalayın. Alt sertifika, konu = bakarak belirleyebilirsiniz / satır.
7. Geçiş *vpnconfig.ovpn* dosya 3. adımdaki Not Defteri'nde açılır. Aşağıda gösterilen bölümü bulun ve "cert" arasındaki her şeyi değiştirin ve "/ Sertifika".

  ```
  # P2S client certificate
  # please fill this field with a PEM formatted cert
  <cert>
  $CLIENTCERTIFICATE
  </cert>
  ```
8.  Açık *profileinfo.txt* Defteri'nde. Özel anahtarı almak için metni seçin (dahil olmak üzere ve arasında) "---BEGIN PRIVATE KEY---" ve "---BEGIN PRIVATE KEY---" ve kopyalayın.
9.  Vpnconfig.ovpn dosyasını Not Defteri'nde dönün ve bu bölümü bulun. Arasındaki her şeyi değiştirerek bir özel anahtarı yapıştırın ve "anahtar" ve "/ anahtar".

  ```
  # P2S client root certificate private key
  # please fill this field with a PEM formatted key
  <key>
  $PRIVATEKEY
  </key>
  ```
10. Başka bir alanı değiştirmeyin. VPN’e bağlanmak için istemci girişinde doldurulmuş yapılandırmayı kullanın.
11. vpnconfig.ovpn dosyasını C:\Program Files\OpenVPN\config klasörüne kopyalayın.
12. Sistem tepsisindeki OpenVPN simgesine sağ tıklayın ve Bağlan’a tıklayın.

## <a name="mac"></a>Mac istemcileri

1. Bir OpenVPN istemci gibi yükleyip [TunnelBlik](https://tunnelblick.net/downloads.html). 
2. Ağ geçidinin VPN profilini indirin. Bu, Azure portalında ya da 'New-AzureRmVpnClientConfiguration' PowerShell kullanarak noktadan siteye yapılandırma sekmesinden yapılabilir.
3. Profilin sıkıştırmasını açın. Not Defteri'ni OpenVPN klasöründeki vpnconfig.ovpn yapılandırma dosyasını açın.
4. P2S istemci sertifikası bölümünü base64’teki P2S istemci sertifikası genel anahtarı ile doldurun. PEM biçimli bir sertifikada .cer dosyasını açıp base64 anahtarını sertifika üst bilgileri arasına kopyalamanız yeterlidir. Bkz: [ortak anahtarını dışarı aktarmak](vpn-gateway-certificates-point-to-site.md#cer) kodlanmış ortak anahtarı almak için bir sertifikayı dışarı aktarma hakkında bilgi için.
5. Özel anahtar bölümünü, base64’teki P2S istemci sertifikası özel anahtarı ile doldurun. Bkz: [, özel anahtarı dışarı aktar](https://www.geotrust.eu/en/support/manuals/microsoft/all+windows+servers/export+private+key+or+certificate/) özel anahtarınızı ayıklamanız hakkında bilgi için.
6. Başka bir alanı değiştirmeyin. VPN’e bağlanmak için istemci girişinde doldurulmuş yapılandırmayı kullanın.
7. İçinde tunnelblik profili oluşturmak için profili dosyasına çift tıklayın.
8. Uygulamaları klasöründen Tunnelblik başlatın.
9. Sistem tepsisindeki Tunneblik simgesine tıklayın ve çekme bağlanın.

## <a name="linux"></a>Linux istemcileri

1. Yeni bir Terminal oturumu açın. Aynı anda 'Ctrl + Alt + t' tuşuna basarak yeni bir oturum açabilirsiniz.
2. Gerekli bileşenleri yüklemek için aşağıdaki komutu girin:

  ```
  sudo apt-get install openvpn
  sudo apt-get -y install network-manager-openvpn
  sudo service network-manager restart
  ```
3. Ağ geçidinin VPN profilini indirin. Bu, Azure portalında noktadan siteye yapılandırma sekmesinden yapılabilir.
4.  [Dışarı aktarma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-certificates-point-to-site#clientexport) oluşturduğunuz ve ağ geçidi P2S yapılandırmanıza karşıya sertifika P2S istemci. 
5. Özel anahtarı ve base64 parmak izi .pfx ayıklayın. Bunu yapmanın birden çok yolu vardır. Bilgisayarınızda OpenSSL kullanarak bir yoludur.

    ```
    openssl.exe pkcs12 -in "filename.pfx" -nodes -out "profileinfo.txt"
    ```
  *Profileinfo.txt* dosyasını içerecek özel anahtarı ve parmak izini CA ve istemci sertifikası. İstemci sertifikası parmak izi kullandığınızdan emin olun.

6. Açık *profileinfo.txt* bir metin düzenleyicisinde. İstemci (alt) sertifikanın parmak izini edinmek için de dahil olmak üzere ve "---Başlangıç arasında sertifika---" metin seçin ve "---bitiş sertifika---" için alt sertifika ve kopyalayın. Alt sertifika, konu = bakarak belirleyebilirsiniz / satır.

7.  Açık *vpnconfig.ovpn* dosya ve aşağıda gösterilen bölümü bulun. Arasındaki her şeyi değiştirin ve "cert" ve "/ Sertifika".

    ```
    # P2S client certificate
    # please fill this field with a PEM formatted cert
    <cert>
    $CLIENTCERTIFICATE
    </cert>
    ```
8.  Profileinfo.txt bir metin düzenleyicisinde açın. Özel anahtarı almak için de dahil olmak üzere ve "---Başlangıç arasında özel anahtarı---" metin seçin ve "---BEGIN PRIVATE KEY---" ve kopyalayın.

9.  Vpnconfig.ovpn dosyasını bir metin düzenleyicisinde açın ve bu bölümü bulun. Arasındaki her şeyi değiştirerek bir özel anahtarı yapıştırın ve "anahtar" ve "/ anahtar".

    ```
    # P2S client root certificate private key
    # please fill this field with a PEM formatted key
    <key>
    $PRIVATEKEY
    </key>
    ```

10. Başka bir alanı değiştirmeyin. VPN’e bağlanmak için istemci girişinde doldurulmuş yapılandırmayı kullanın.
11. Komut satırını kullanarak bağlanmak için aşağıdaki komutu yazın:
  
  ```
  sudo openvpn –-config <name and path of your VPN profile file>
  ```
12. GUI kullanarak bağlanmak için sistem ayarlarına gidin.
13. Tıklayın **+** yeni VPN bağlantısı eklemek için.
14. Altında **ekleme VPN**, çekme **dosyadan içeri aktar...**
15. Profil dosyasını ve çift tıklatın veya çekme göz atın **açık**.
16. Tıklayın **Ekle** üzerinde **ekleme VPN** penceresi.
  
  ![Dosyadan içeri aktar](./media/vpn-gateway-howto-openvpn-clients/importfromfile.png)
17. VPN açarak bağlanabilir **ON** üzerinde **ağ ayarlarını** sayfasında, ya da sistem tepsisindeki ağ simgesinin altında.

## <a name="next-steps"></a>Sonraki adımlar

Başka bir vnet (üretim) kaynaklara erişebilmesi için VPN istemcileri istiyorsanız, ardından ilgili yönergeleri uygulayın [VNet-VNet](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) vnet-vnet bağlantı kurmak için makaleyi. Ağ geçitleriniz ve bağlantılarınızı üzerinde BGP etkinleştirdiğinizden emin olun, aksi takdirde trafik değil akar.
