---
title: "Azure noktadan siteye VPN bağlantıları hakkında | Microsoft Docs"
description: "Bu makalede, noktadan siteye bağlantıları anlamanıza yardımcı olur ve kullanmak için hangi P2S VPN ağ geçidi kimlik doğrulama türü karar vermenize yardımcı olur."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/17/2018
ms.author: cherylmc
ms.openlocfilehash: 74cfa8f54c52463ac0b42c5cc6abab7b0366ac29
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="about-point-to-site-vpn"></a>Noktadan siteye VPN hakkında

Noktadan Siteye (P2S) VPN ağ geçidi bağlantısı, ayrı bir istemci bilgisayardan sanal ağınıza güvenli bir bağlantı oluşturmanıza olanak sağlar. P2S bağlantısı, istemci bilgisayardan başlatılarak oluşturulur. Bu çözüm, Azure sanal ağlarına uzak bir konumdan (örneğin, evden veya bir konferanstan) bağlanmak isteyen uzaktan çalışan kişiler için kullanışlıdır. P2S VPN ayrıca, bir sanal ağa bağlanması gereken yalnızca birkaç istemciniz olduğunda S2S VPN yerine kullanabileceğiniz yararlı bir çözümüdür. Bu tablo Resource Manager dağıtım modelleri için geçerlidir.

## <a name="protocol"></a>Hangi protokolü P2S kullanıyor mu?

Noktadan siteye VPN aşağıdaki protokollerden birini kullanabilirsiniz:

* Güvenli Yuva Tünel Protokolü (SSTP), bir özel SSL tabanlı VPN protokolü. SSL kullanan TCP bağlantı noktası 443, çoğu güvenlik duvarı açık olabileceklerinden SSL VPN çözümü güvenlik duvarları, sızmasını. SSTP yalnızca Windows cihazlarda desteklenir. Azure SSTP (Windows 7 ve üzeri) yüklü Windows tüm sürümlerini destekler.

* IKEv2 VPN, standart tabanlı bir IPsec VPN çözümüdür. IKEv2 VPN, Mac cihazlardan (OSX sürüm 10.11 ve üzeri) bağlantı kurmak için kullanılabilir.

Windows ve Mac cihazları oluşan bir karma istemci ortamınız varsa, SSTP ve Ikev2 yapılandırın.

>[!NOTE]
>P2S için Ikev2 Resource Manager dağıtım modeli için kullanılabilir. Klasik dağıtım modeli için kullanılabilir değil.
>

## <a name="authentication"></a>P2S VPN istemcileri nasıl doğrulanır?

Azure P2S VPN bağlantısı kabul etmeden önce kullanıcının ilk doğrulanması gerekir. Bağlanan kullanıcının kimliğini doğrulamak için Azure sunar iki mekanizma vardır.

### <a name="authenticate-using-native-azure-certificate-authentication"></a>Yerel Azure sertifika kimlik doğrulamasını kullanarak kimlik doğrulaması

Yerel Azure sertifika kimlik doğrulaması kullanırken, aygıtta bir istemci sertifikası bağlanan kullanıcının kimliğini doğrulamak için kullanılır. İstemci sertifikalarını bir güvenilen kök sertifika oluşturulur ve her istemci bilgisayarda yüklü. Kendinden imzalı bir sertifika oluşturabilir veya kurumsal bir çözümü kullanılarak oluşturulan bir kök sertifikası kullanabilirsiniz.

İstemci sertifikası doğrulama P2S VPN bağlantısı kurma sırasında olur ve VPN ağ geçidi tarafından gerçekleştirilir. Kök sertifika doğrulama için gereklidir ve Azure'a yüklenmelidir. 

### <a name="authenticate-using-active-directory-ad-domain-server"></a>Active Directory (AD) etki alanı sunucusu kullanarak kimlik doğrulaması

AD etki alanı kimlik doğrulama, kuruluş etki alanı kimlik bilgilerini kullanarak Azure'a bağlanmasına olanak sağlar. AD sunucusu ile tümleşen bir RADIUS sunucusu gerektirir. Kuruluşlar, kullanıcıların varolan RADIUS dağıtım da kullanabilirsiniz.   
 RADIUS sunucusu şirket içinde dağıtılabilir olabilir veya Azure sanal. Kimlik doğrulaması sırasında Azure VPN ağ geçidi geçiş ve ileten kimlik doğrulama iletileri ve geriye ve RADIUS sunucusu arasında bir bağlantı aygıtı gibi davranır. Bu nedenle RADIUS sunucusu ağ geçidi ulaşılabilirlik önemlidir. Daha sonra RADIUS sunucusu mevcut şirket içi ise, Azure VPN S2S bağlantısından şirket içi siteye ulaşılabilirlik için gereklidir.  
 RADIUS sunucusu ayrıca AD Sertifika Hizmetleri ile tümleştirebilirsiniz. Bu, RADIUS sunucusu ve kuruluş sertifika dağıtımınızı Azure sertifika kimlik doğrulaması için bir alternatif olarak P2S sertifika kimlik doğrulaması kullanmanızı sağlar. Kök sertifikaları ve iptal edilen sertifikaları Azure'a yüklemeniz gerekmez avantajı sağlamasıdır.

Bir RADIUS sunucusu, ayrıca diğer dış kimlik sistemleri ile tümleştirebilirsiniz. Bu, çok faktörlü seçenekleri de dahil olmak üzere, P2S VPN için kimlik doğrulama seçenekleri bolca yukarı açar.

! [noktası siteye]] (./media/point-to-site-about/p2s.png "Noktadan siteye")

### <a name="configuration-requirements-for-client-devices"></a>İstemci cihazları için yapılandırma gereksinimleri

Kullanıcılar yerel VPN istemcileri, Windows ve Mac cihazlarda P2S için kullanır. Azure VPN istemcisi yapılandırma zip sağlar Azure'a bağlanmak için bu yerel istemciler tarafından gerekli ayarları içeren dosya.

* Windows cihazları için kullanıcıların cihazlarına yüklemesi bir yükleyici paketi, VPN istemci yapılandırmasında oluşur.
* Mac cihazlar için kullanıcıların cihazlarına yüklemesi mobileconfig dosya oluşur.

Zip dosyası bazı önemli ayarları değerleri bu aygıtlar için kendi profili oluşturmak için kullanabileceğiniz Azure tarafında de sağlar. Bazı değerleri, VPN ağ geçidi adresi, yapılandırılmış tünel türünü, yollar ve ağ geçidi doğrulaması için kök sertifikasını içerir.

### <a name="gwsku"></a>Hangi ağ geçidi SKU'ları destek P2S VPN?

[!INCLUDE [p2s-skus](../../includes/vpn-gateway-table-point-to-site-skus-include.md)]

* Toplam Aktarım Hızı Kıyaslaması, tek bir ağ geçidi üzerinde yer alan birden fazla tünelin ölçümlerine bağlıdır. Garantili verimlilik Internet trafiği koşulları ve uygulama davranışları nedeniyle değil.
* Fiyatlandırma bilgileri Fiyatlandırma sayfasında bulunabilir. 
* SLA (hizmet düzeyi sözleşmesi) bilgileri SLA sayfasında bulunabilir.

>[!NOTE]
>Temel SKU Ikev2 veya RADIUS kimlik doğrulamasını desteklemez.
>

## <a name="configure"></a>P2S bağlantısı nasıl yapılandırırım?

P2S yapılandırma oldukça birkaç belirli adım gerektirir. Aşağıdaki makaleler, P2S yapılandırması ve VPN istemci cihazları yapılandırmak için bağlantıları yol için adımları içerir:

* [P2S bağlantısı - RADIUS kimlik doğrulamasını yapılandırma](point-to-site-how-to-radius-ps.md)

* [P2S bağlantısı - Azure yerel sertifika kimlik doğrulamasını yapılandırma](vpn-gateway-howto-point-to-site-rm-ps.md)

## <a name="faqcert"></a>Yerel Azure sertifika kimlik doğrulaması hakkında SSS

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-faq-p2s-azurecert-include.md)]

## <a name="faqradius"></a>RADIUS kimlik doğrulaması hakkında SSS

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-faq-p2s-radius-include.md)]

## <a name="next-steps"></a>Sonraki Adımlar

* [P2S bağlantısı - RADIUS kimlik doğrulamasını yapılandırma](point-to-site-how-to-radius-ps.md)

* [P2S bağlantısı - Azure yerel sertifika kimlik doğrulamasını yapılandırma](vpn-gateway-howto-point-to-site-rm-ps.md)