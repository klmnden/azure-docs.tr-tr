---
title: Azure Stack veri merkezi tümleştirmesi - uç noktalarını yayımlama | Microsoft Docs
description: Veri merkezinizde Azure Stack uç noktalarını yayımlama hakkında bilgi edinin
services: azure-stack
author: jeffgilb
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 08/02/2018
ms.author: jeffgilb
ms.reviewer: wamota
keywords: ''
ms.openlocfilehash: c5d0bc6a0e7e885f61ba28be0575697b03fece09
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42917151"
---
# <a name="azure-stack-datacenter-integration---publish-endpoints"></a>Azure Stack veri merkezi tümleştirmesi - uç noktalarını yayımlama
Azure Stack altyapısını rollerinden sanal IP adresleri (VIP) ayarlar. Bu Vıp'lerin genel IP adresi havuzundan ayrılır. Her bir VIP yazılım tanımlı ağ katmanında bir erişim denetimi listesi (ACL) ile korunmaktadır. ACL, fiziksel anahtarlar (Zleyicileri ve BMC) arasında daha fazla çözüm sağlamlaştırmak için de kullanılır. Dağıtım sırasında belirtilen dış DNS bölge içindeki her bir uç nokta için bir DNS girişi oluşturulur.


Aşağıdaki Mimari diyagram, ACL'ler ve farklı ağ katmanları gösterilmektedir:

![Yapısal resmi](media/azure-stack-integrate-endpoints/Integrate-Endpoints-01.png)

## <a name="ports-and-protocols-inbound"></a>Bağlantı noktaları ve protokoller (gelen)

Bir dizi altyapı VIP'ler dış ağlara Azure Stack uç noktalarını yayımlama için gereklidir. *Uç nokta (VIP)* tabloda gösterilmektedir her uç nokta, gerekli bağlantı noktası ve protokol. SQL kaynak sağlayıcısı gibi ek kaynak sağlayıcıları gerekli uç noktaları için özel kaynak sağlayıcısı dağıtım belgelerine bakın.

Bunlar yayımlama Azure Stack için gerekli değil çünkü VIP'ler listelenmemiş iç altyapı.

> [!NOTE]
> Azure Stack operatörü tarafından hiçbir denetimiyle kullanıcıların kendileri tarafından tanımlanan kullanıcı VIP'ler dinamik.


|Uç nokta (VIP)|DNS ana bilgisayar bir kaydı|Protokol|Bağlantı Noktaları|
|---------|---------|---------|---------|
|AD FS|ADFS.  *&lt;bölge >.&lt; FQDN >*|HTTPS|443|
|Portal (Yönetici)|Adminportal.  *&lt;bölge >.&lt; FQDN >*|HTTPS|443<br>12495<br>12499<br>12646<br>12647<br>12648<br>12649<br>12650<br>13001<br>13003<br>13010<br>13011<br>13012<br>13020<br>13021<br>13026<br>30015|
|Azure Resource Manager (Yönetici)|Adminmanagement.  *&lt;bölge >.&lt; FQDN >*|HTTPS|443<br>30024|
|Portal (kullanıcı)|Portalı.  *&lt;bölge >.&lt; FQDN >*|HTTPS|443<br>12495<br>12649<br>13001<br>13010<br>13011<br>13012<br>13020<br>13021<br>30015<br>13003|
|Azure Resource Manager (kullanıcı)|Yönetimi.  *&lt;bölge >.&lt; FQDN >*|HTTPS|443<br>30024|
|Graf|Grafiği.  *&lt;bölge >.&lt; FQDN >*|HTTPS|443|
|Sertifika iptal listesi|CRL.*&lt;bölge >.&lt; FQDN >*|HTTP|80|
|DNS|&#42;.  *&lt;bölge >.&lt; FQDN >*|TCP VE UDP|53|
|Key Vault (kullanıcı)|&#42;.Vault.  *&lt;bölge >.&lt; FQDN >*|HTTPS|443|
|Key Vault (Yönetici)|&#42;.adminvault.  *&lt;bölge >.&lt; FQDN >*|HTTPS|443|
|Depolama Kuyruğu|&#42;.Queue.  *&lt;bölge >.&lt; FQDN >*|HTTP<br>HTTPS|80<br>443|
|Depolama tablosu|&#42;.Table.  *&lt;bölge >.&lt; FQDN >*|HTTP<br>HTTPS|80<br>443|
|Depolama Blobu|&#42;.BLOB.  *&lt;bölge >.&lt; FQDN >*|HTTP<br>HTTPS|80<br>443|
|SQL kaynak sağlayıcısı|sqladapter.dbadapter.  *&lt;bölge >.&lt; FQDN >*|HTTPS|44300-44304|
|MySQL kaynak sağlayıcısı|mysqladapter.dbadapter.*&lt;region>.&lt;fqdn>*|HTTPS|44300-44304|
|App Service|&#42;.appservice.  *&lt;bölge >.&lt; FQDN >*|TCP|80 (HTTP)<br>443 (HTTPS)<br>8172 (MSDeploy)|
|  |&#42;. scm.appservice.  *&lt;bölge >.&lt; FQDN >*|TCP|443 (HTTPS)|
|  |api.appservice.  *&lt;bölge >.&lt; FQDN >*|TCP|443 (HTTPS)<br>44300 (azure Resource Manager)|
|  |FTP.appservice.  *&lt;bölge >.&lt; FQDN >*|TCP, UDP|21, 1021, 10001-10100 (FTP)<br>990 (FTPS)|
|VPN Gateway’leri|     |     |[VPN gateway SSS bkz](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vpn-faq#can-i-traverse-proxies-and-firewalls-using-point-to-site-capability).|
|     |     |     |     |

## <a name="ports-and-urls-outbound"></a>Bağlantı noktaları ve URL'ler (giden)

Azure Stack, yalnızca saydam proxy sunucuları destekler. Bir dağıtımda saydam proxy yukarı bağlantılar burada geleneksel proxy sunucusu için aşağıdaki bağlantı noktaları ve URL'ler giden iletişime izin vermeniz gerekir:


|Amaç|URL'si|Protokol|Bağlantı Noktaları|
|---------|---------|---------|---------|
|Kimlik|login.windows.net<br>login.microsoftonline.com<br>Graph.Windows.NET<br>https://secure.aadcdn.microsoftonline-p.com<br>Office.com|HTTP<br>HTTPS|80<br>443|
|Market sendikasyonu|https://management.azure.com<br>https://&#42;.blob.core.windows.net<br>https://*.azureedge.net<br>https://&#42;.microsoftazurestack.com|HTTPS|443|
|Düzeltme Eki & Güncelleştir|https://&#42;.azureedge.net|HTTPS|443|
|Kayıt|https://management.azure.com|HTTPS|443|
|Kullanım|https://&#42;.microsoftazurestack.com<br>https://*.trafficmanager.NET|HTTPS|443|
|Windows Defender'ı|. wdcp.microsoft.com<br>. wdcpalt.microsoft.com<br>*. updates.microsoft.com<br>*. download.microsoft.com<br>https://msdl.microsoft.com/download/symbols<br>http://www.microsoft.com/pkiops/crl<br>http://www.microsoft.com/pkiops/certs<br>http://crl.microsoft.com/pki/crl/products<br>http://www.microsoft.com/pki/certs<br>https://secure.aadcdn.microsoftonline-p.com<br>|HTTPS|80<br>443|
|NTP|     |UDP|123|
|DNS|     |TCP<br>UDP|53|
|     |     |     |     |



## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack PKI gereksinimleri](azure-stack-pki-certs.md)
