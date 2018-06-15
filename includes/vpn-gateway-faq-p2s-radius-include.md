---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: f9b597427c3731b28ec68a7bc518955eb5f829ce
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
ms.locfileid: "30198001"
---
[!INCLUDE [P2S FAQ All](vpn-gateway-faq-p2s-all-include.md)]

### <a name="is-radius-authentication-supported-on-all-azure-vpn-gateway-skus"></a>RADIUS kimlik doğrulaması tüm Azure VPN Gateway SKU’larında destekleniyor mu?

RADIUS kimlik doğrulaması VpnGw1, VpnGw2 ve VpnGw3 SKU’ları için desteklenir. Eski SKU’ları kullanıyorsanız, RADIUS kimlik doğrulaması Standart ve Yüksek Performans SKU’larında desteklenir. Temel Ağ Geçidi SKU’sunda desteklenmez. 
 
### <a name="is-radius-authentication-supported-for-the-classic-deployment-model"></a>RADIUS kimlik doğrulaması klasik dağıtım modeli için desteklenir mi?
 
Hayır. RADIUS kimlik doğrulaması klasik dağıtım modeli için desteklenmez.
 
### <a name="are-3rd-party-radius-servers-supported"></a>Üçüncü taraf RADIUS sunucuları desteklenir mi?

Evet, üçüncü taraf RADIUS sunucuları desteklenir.
 
### <a name="what-are-the-connectivity-requirements-to-ensure-that-the-azure-gateway-is-able-to-reach-an-on-premises-radius-server"></a>Azure ağ geçidinin şirket içi bir RADIUS sunucusuna ulaşabildiğinden emin olmak için bağlantı gereksinimleri nelerdir?

Şirket içi sitede doğru rotalar yapılandırılmış biçimde bir VPN Siteden Siteye bağlantı gereklidir.  
 
### <a name="can-traffic-to-an-on-premises-radius-server-from-the-azure-vpn-gateway-be-routed-over-an-expressroute-connection"></a>Şirket içi RADIUS sunucusuna gelen trafik (Azure VPN ağ geçidinden) bir ExpressRoute bağlantısı üzerinden yönlendirilebilir mi?

Hayır. Yalnızca bir Siteden Siteye bağlantı üzerinden yönlendirilebilir.
 
### <a name="is-there-a-change-in-the-number-of-sstp-connections-supported-with-radius-authentication-what-is-the-maximum-number-of-sstp-and-ikev2-connections-supported"></a>RADIUS kimlik doğrulaması ile desteklenen SSTP bağlantılarının sayısında bir değişiklik var mı? En fazla kaç SSTP ve IKEv2 bağlantısı desteklenir?

RADIUS kimlik doğrulaması ile bir ağ geçidinde desteklenen en fazla SSTP bağlantısı sayısında bir değişiklik yoktur. Hala 128 bağlantı desteklenmektedir. Ağ geçidinin SSTP, IKEv2 veya her ikisi için yapılandırılmış olması fark etmeksizin, desteklenen en fazla bağlantı sayısı 128’dir.
 
### <a name="what-is-the-difference-between-doing-certificate-authentication-using-a-radius-server-vs-using-azure-native-certificate-authentication-by-uploading-a-trusted-certificate-to-azure"></a>RADIUS sunucusu kullanarak sertifika doğrulaması yapma ve Azure yerel sertifika doğrulaması kullanma (güvenilir bir sertifikayı Azure’a yükleyerek) arasındaki fark nedir?

RADIUS sertifika doğrulamasında, doğrulaması isteği gerçek sertifika doğrulamasını işleyen bir RADIUS sunucusuna iletilir. Zaten mevcut bir sertifika doğrulaması altyapısını RADIUS üzerinden tümleştirmek istiyorsanız bu seçenek yararlı olur.
  
Sertifika doğrulaması için Azure kullanıldığında, Azure VPN ağ geçidi sertifikayı doğrular. Sertifika ortak anahtarınızı ağ geçidine yüklemeniz gerekir. Ayrıca bağlanmasına izin verilmeyen iptal edilmiş sertifikaların bir listesini belirtebilirsiniz.

### <a name="does-radius-authentication-work-with-both-ikev2-and-sstp-vpn"></a>RADIUS kimlik doğrulaması hem IKEv2 hem de SSTP VPN ile çalışır mı?

Evet, RADIUS kimlik doğrulaması hem IKEv2 hem de SSTP VPN için desteklenir.  