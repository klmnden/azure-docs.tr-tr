---
title: Azure veri merkezi tümleştirme yığın - uç noktalarını yayımlama | Microsoft Docs
description: Azure yığın uç noktaları, veri merkezinizde yayımlama öğrenin
services: azure-stack
author: jeffgilb
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 04/06/2018
ms.author: jeffgilb
ms.reviewer: wamota
keywords: ''
ms.openlocfilehash: 014dbf6ff6d30bfb988c404bfe35693fe68f22fd
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="azure-stack-datacenter-integration---publish-endpoints"></a>Azure veri merkezi tümleştirme yığın - uç noktalarını yayımlama
Azure yığın altyapı rollerini sanal IP adresleri (VIP) ayarlar. Bu Vıp'lerin ortak IP adresi havuzundan ayrılır. Her VIP yazılım tanımlı ağ katmanı bir erişim denetim listesi (ACL) ile korunmaktadır. ACL, fiziksel anahtarlar (TORs ve BMC) arasında daha fazla çözüm sağlamlaştırmak için de kullanılır. Dağıtım sırasında belirtilen dış DNS bölge içindeki her bir uç nokta için bir DNS girişi oluşturulur.


Aşağıdaki Mimari diyagramı ACL'ler ve farklı ağ katmanları gösterilmektedir:

![Yapısal resmi](media/azure-stack-integrate-endpoints/Integrate-Endpoints-01.png)

## <a name="ports-and-protocols-inbound"></a>Bağlantı noktalarını ve protokolleri (gelen)

Altyapı kümesi VIP'ler dış ağlara yayımlama Azure yığın uç noktalar için gereklidir. *Uç noktası (VIP)* tablo gösterir her uç nokta, gerekli bağlantı noktası ve protokol. SQL kaynak sağlayıcısı gibi ek kaynak sağlayıcıları Gerektirdiğiniz için belirli bir kaynak sağlayıcısı dağıtım belgelerine bakın.

VIP'ler yayımlama Azure yığını için gerekli olmadığınız için listelenmeyen iç altyapı.

> [!NOTE]
> Azure yığın işleci tarafından hiçbir denetimiyle kullanıcıların kendileri tarafından tanımlanan kullanıcı VIP'ler dinamik.


|Uç noktası (VIP)|DNS ana bilgisayar bir kaydı|Protokol|Bağlantı Noktaları|
|---------|---------|---------|---------|
|AD FS|ADFS.  *&lt;bölge >.&lt; FQDN >*|HTTPS|443|
|Portal (Yönetici)|adminportal.  *&lt;bölge >.&lt; FQDN >*|HTTPS|443<br>12495<br>12499<br>12646<br>12647<br>12648<br>12649<br>12650<br>13001<br>13003<br>13010<br>13011<br>13012<br>13020<br>13021<br>13026<br>30015|
|Azure Kaynak Yöneticisi (Yönetici)|adminmanagement.  *&lt;bölge >.&lt; FQDN >*|HTTPS|443<br>30024|
|Portal (kullanıcı)|Portal.  *&lt;bölge >.&lt; FQDN >*|HTTPS|443<br>12495<br>12649<br>13001<br>13010<br>13011<br>13012<br>13020<br>13021<br>30015<br>13003|
|Azure Resource Manager (kullanıcı)|yönetimi.  *&lt;bölge >.&lt; FQDN >*|HTTPS|443<br>30024|
|Graf|Grafik.  *&lt;bölge >.&lt; FQDN >*|HTTPS|443|
|Sertifika iptal listesi|CRL.*&lt;bölge >.&lt; FQDN >*|HTTP|80|
|DNS|&#42;.  *&lt;bölge >.&lt; FQDN >*|TCP VE UDP|53|
|Anahtar kasası (kullanıcı)|&#42;.Vault.  *&lt;bölge >.&lt; FQDN >*|HTTPS|443|
|Anahtar kasası (Yönetici)|&#42;.adminvault.  *&lt;bölge >.&lt; FQDN >*|HTTPS|443|
|Depolama Kuyruğu|&#42;.Queue.  *&lt;bölge >.&lt; FQDN >*|HTTP<br>HTTPS|80<br>443|
|Depolama tablosu|&#42;.Table.  *&lt;bölge >.&lt; FQDN >*|HTTP<br>HTTPS|80<br>443|
|Depolama blobu|&#42;.BLOB.  *&lt;bölge >.&lt; FQDN >*|HTTP<br>HTTPS|80<br>443|
|SQL kaynak sağlayıcısı|sqladapter.dbadapter.  *&lt;bölge >.&lt; FQDN >*|HTTPS|44300-44304|
|MySQL kaynak sağlayıcısı|mysqladapter.dbadapter.*&lt;region>.&lt;fqdn>*|HTTPS|44300-44304|
|App Service|&#42;.appservice.  *&lt;bölge >.&lt; FQDN >*|TCP|80 (HTTP)<br>443 (HTTPS)<br>8172 (MSDeploy)|
|  |&#42;. scm.appservice.  *&lt;bölge >.&lt; FQDN >*|TCP|443 (HTTPS)|
|  |api.appservice.  *&lt;bölge >.&lt; FQDN >*|TCP|443 (HTTPS)<br>44300 (azure Resource Manager)|
|  |FTP.appservice.  *&lt;bölge >.&lt; FQDN >*|TCP VE UDP|21, 1021, 10001-101000 (FTP)<br>990 (FTPS)|

## <a name="ports-and-urls-outbound"></a>Bağlantı noktaları ve URL'ler (giden)

Azure yığını yalnızca saydam proxy sunucuları destekler. Bir dağıtımda saydam proxy yukarı bağlantılar burada geleneksel proxy sunucusu için şu bağlantı noktalarını ve URL'ler giden iletişim için izin vermelidir:


|Amaç|URL'si|Protokol|Bağlantı Noktaları|
|---------|---------|---------|---------|
|Kimlik|login.windows.net<br>login.microsoftonline.com<br>Graph.Windows.NET<br>https://secure.aadcdn.microsoftonline-p.com|HTTP<br>HTTPS|80<br>443|
|Market dağıtım|https://management.azure.com<br>https://&#42;.blob.core.windows.net<br>https://*.azureedge.net<br>https://&#42;.microsoftazurestack.com|HTTPS|443|
|Düzeltme Eki & Güncelleştir|https://&#42;.azureedge.net|HTTPS|443|
|Kayıt|https://management.azure.com|HTTPS|443|
|Kullanım|https://&#42;.microsoftazurestack.com<br>https://*.trafficmanager.com|HTTPS|443|
|Windows Defender'ı|. wdcp.microsoft.com<br>. wdcpalt.microsoft.com<br>*. updates.microsoft.com<br>*. download.microsoft.com<br>https://msdl.microsoft.com/download/symbols<br>http://www.microsoft.com/pkiops/crl<br>http://www.microsoft.com/pkiops/certs<br>http://crl.microsoft.com/pki/crl/products<br>http://www.microsoft.com/pki/certs<br>https://secure.aadcdn.microsoftonline-p.com<br>|HTTPS|80<br>443|



## <a name="next-steps"></a>Sonraki adımlar

[Azure yığın PKI gereksinimleri](azure-stack-pki-certs.md)