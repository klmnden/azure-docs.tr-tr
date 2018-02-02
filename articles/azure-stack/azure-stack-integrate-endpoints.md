---
title: "Azure veri merkezi tümleştirme yığın - uç noktalarını yayımlama"
description: "Azure yığın uç noktaları, veri merkezinizde yayımlama öğrenin"
services: azure-stack
author: jeffgilb
ms.service: azure-stack
ms.topic: article
ms.date: 01/31/2018
ms.author: jeffgilb
ms.reviewer: wamota
keywords: 
ms.openlocfilehash: e368109adc7db4c589ac37b28c4891cb3ec5346f
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="azure-stack-datacenter-integration---publish-endpoints"></a>Azure veri merkezi tümleştirme yığın - uç noktalarını yayımlama

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Azure yığın altyapı rolleri için çeşitli uç (VIP'ler - sanal IP adresleri) ayarlar. Bu Vıp'lerin ortak IP adresi havuzundan ayrılır. Her VIP yazılım tanımlı ağ katmanı bir erişim denetim listesi (ACL) ile korunmaktadır. ACL, fiziksel anahtarlar (TORs ve BMC) arasında daha fazla çözüm sağlamlaştırmak için de kullanılır. Dağıtım sırasında belirtilen dış DNS bölge içindeki her bir uç nokta için bir DNS girişi oluşturulur.


Aşağıdaki Mimari diyagramı ACL'ler ve farklı ağ katmanları gösterilmektedir:

![Mimari diyagramı](media/azure-stack-integrate-endpoints/Integrate-Endpoints-01.png)

## <a name="ports-and-protocols-inbound"></a>Bağlantı noktalarını ve protokolleri (gelen)

Dış ağlara yayımlama Azure yığın uç noktalar için gerekli altyapı VIP'ler aşağıdaki tabloda listelenmiştir. Liste her gösterir uç noktası, gerekli bağlantı noktası ve protokol. SQL kaynak sağlayıcısı ve diğerleri gibi ek kaynak sağlayıcıları için gerekli uç noktaları belirli bir kaynak sağlayıcısı dağıtım belgelerinde ele alınmıştır.

İç altyapı yayımlama Azure yığını için gerekli olmadıklarını olduğundan VIP'ler listelenmez.

> [!NOTE]
> Azure yığın işleci tarafından hiçbir denetimiyle kullanıcıların kendileri tarafından tanımlanan kullanıcı VIP'ler dinamik.


|Endpoint (VIP)|DNS ana bilgisayar bir kaydı|Protokol|Bağlantı Noktaları|
|---------|---------|---------|---------|
|AD FS|Adfs.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Portal (Yönetici)|Adminportal.  *&lt;bölge >.&lt; FQDN >*|HTTPS|443<br>12495<br>12499<br>12646<br>12647<br>12648<br>12649<br>12650<br>13001<br>13003<br>13010<br>13011<br>13020<br>13021<br>13026<br>30015|
|Azure Kaynak Yöneticisi (Yönetici)|Adminmanagement.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>30024|
|Portal (kullanıcı)|Portal.  *&lt;bölge >.&lt; FQDN >*|HTTPS|443<br>12495<br>12649<br>13001<br>13010<br>13011<br>13020<br>13021<br>30015<br>13003|
|Azure Resource Manager (kullanıcı)|Management.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>30024|
|Graf|Graph.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Sertifika iptal listesi|CRL.*&lt;bölge >.&lt; FQDN >*|HTTP|80|
|DNS|&#42;.*&lt;region>.&lt;fqdn>*|TCP VE UDP|53|
|Anahtar kasası (kullanıcı)|&#42;. Kasa.  *&lt;bölge >.&lt; FQDN >*|HTTPS|443|
|Anahtar kasası (Yönetici)|&#42;.adminvault.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Depolama Kuyruğu|&#42;. sıra.  *&lt;bölge >.&lt; FQDN >*|HTTP<br>HTTPS|80<br>443|
|Depolama tablosu|&#42;. Tablo.  *&lt;bölge >.&lt; FQDN >*|HTTP<br>HTTPS|80<br>443|
|Depolama blobu|&#42;.blob.*&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|
|SQL kaynak sağlayıcısı|sqladapter.dbadapter.*&lt;region>.&lt;fqdn>*|HTTPS|44300-44304|
|MySQL kaynak sağlayıcısı|mysqladapter.dbadapter.*&lt;region>.&lt;fqdn>*|HTTPS|44300-44304

## <a name="ports-and-urls-outbound"></a>Bağlantı noktaları ve URL'ler (giden)

Azure yığını yalnızca saydam proxy sunucuları destekler. Bir dağıtımda saydam proxy yukarı bağlantılar burada geleneksel proxy sunucusu için şu bağlantı noktalarını ve URL'ler giden iletişim için izin vermelidir:


|Amaç|URL|Protokol|Bağlantı Noktaları|
|---------|---------|---------|---------|
|Kimlik|login.windows.net<br>login.microsoftonline.com<br>graph.windows.net|HTTP<br>HTTPS|80<br>443|
|Market dağıtım|https://management.azure.com<br>https://&#42;.blob.core.windows.net<br>https://*.azureedge.net<br>https://&#42;.microsoftazurestack.com|HTTPS|443|
|Düzeltme Eki & Güncelleştir|https://&#42;.azureedge.net|HTTPS|443|
|Kayıt|https://management.azure.com|HTTPS|443|
|Kullanım|https://&#42;.microsoftazurestack.com<br>https://*.trafficmanager.com|HTTPS|443|


## <a name="next-steps"></a>Sonraki adımlar
[Azure yığın PKI gereksinimleri](azure-stack-pki-certs.md)