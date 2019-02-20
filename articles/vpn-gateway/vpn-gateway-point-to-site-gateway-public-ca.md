---
title: Geçiş otomatik olarak imzalanan ortak CA sertifikaları P2S ağ geçitleri için | Azure VPN ağ geçidi | Microsoft Docs
description: Bu makale P2S ağ geçitleri için yeni ortak CA sertifikaları için başarılı bir şekilde geçiş yardımcı olur.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: cherylmc
ms.openlocfilehash: e5a75826730219adc643d7c6ca300a38c8640006
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2019
ms.locfileid: "56428373"
---
# <a name="transition-to-a-public-ca-gateway-certificate-for-p2s"></a>P2S için genel CA ağ geçidi sertifikasına geçiş

Azure VPN ağ geçidi, ağ geçitleri P2S bağlantıları için artık Azure düzeyinde otomatik olarak imzalanan sertifikalar verir. Verilen sertifikaların artık bir ortak sertifika yetkilisi (CA) tarafından imzalanmıştır. Ancak, bazı eski ağ geçitlerinin yine de otomatik olarak imzalanan sertifikaları kullanıyor olabilir. Bu otomatik olarak imzalanan sertifikalar sona erme tarihlerinin ve ortak CA sertifikaları için geçiş gerekir.

Bu bağlamda bir ek Azure düzey sertifika sertifikalardır. Kendi otomatik olarak imzalanan kök sertifika ve kimlik doğrulaması için istemci sertifikası oluştururken kullandığınız sertifika zincirleri değiller. Bu sertifikaları yapmanızdan ve tarihler, bunu yapmak için bunları oluşturulan sona erer.

>[!NOTE]
> P2S istemci kimlik doğrulaması için kullanılan otomatik olarak imzalanan sertifikaları, bu Azure düzey sertifika değişiklikten etkilenmez. Sorun ve otomatik olarak imzalanan sertifikalar normal şekilde kullanmaya devam edebilirsiniz.
>

Daha önce (arka planda, Azure tarafından verilen) ağ geçidi için kendinden imzalı bir sertifika her 18 ay güncelleştirilmesi gerekmiyor. VPN istemcisi yapılandırma dosyalarını sonra oluşturulur ve tüm P2S istemcilere imzalanmasını gerekiyordu. Ortak CA sertifikaları taşıyarak bu sınırlama ortadan kaldırır. Sertifikaları için geçiş ek olarak, bu değişiklik platform iyileştirmeleri, daha iyi ölçümleri ve kararlılığı sağlar.

Yalnızca eski ağ geçitleri bu değişiklikten etkilenmez. Ağ geçidi sertifikanızı geçmiş olması gerekiyorsa, Azure portalında iletişimi veya bildirim alırsınız. Ağ geçidiniz bu makaledeki adımları kullanarak etkilenir bakabilirsiniz.

>[!IMPORTANT]
>Geçiş için Mart zamanlandı 18:00 UTC'de 12,2019 başlatılıyor. Farklı zaman penceresi tercih ederseniz bir destek talebi oluşturabilirsiniz. Yapın ve en az 24 saat önceden isteğiniz sonlandır.  Aşağıdaki windows isteyebilirsiniz:
>
>* 25 Şubat 06:00 UTC
>* 25 Şubat 18:00 UTC
>* 1 Mart'ta 06:00 UTC
>* 1 Mart'ta 18:00 UTC
>
>**Kalan tüm ağ geçitleri 12 Mart 18:00 UTC'de başlangıç 2019 tarihinde geçeceğiyle**.
>

## <a name="1-verify-your-certificate"></a>1. Sertifikanızı doğrulayın

1. Bu güncelleştirmeyle etkilenip etkilenmediğini denetleyin. İçindeki adımları kullanarak, geçerli VPN istemci yapılandırması indirme [bu makalede](point-to-site-vpn-client-configuration-azure-cert.md).

2. Açın veya zip dosyasını ayıklayın ve "Genel" klasöre göz atın. Genel klasöründe biri olan iki dosya görürsünüz *VPNSettings.xml*.
3. Açık *VPNSettings.xml* bir xml Görüntüleyici/düzenleyicisinde. Xml dosyasında aşağıdaki alanları ara:

  * `<ServerCertRootCn>DigiCert Global Root CA</ServerCertRootCn>`
  * `<ServerCertIssuerCn>DigiCert Global Root CA</ServerCertIssuerCn>`
4. Varsa *ServerCertRotCn* ve *ServerCertIssuerCn* "DigiCert genel kök CA" olan, bu güncelleştirme ile etkilenmez ve bu makaledeki adımlar ile devam etmek ihtiyacınız yoktur. Ancak, başka bir şey Göster, ağ geçidi sertifikanızı güncelleştirmenin parçası olan ve geçirilirsiniz.

## <a name="2-check-certificate-transition-schedule"></a>2. Sertifika geçişi zamanlamayı denetle

Sertifikanızı güncelleştirmenin parçası ise, ağ geçidi sertifikanızı geçirilecektir. Başvurmak **önemli** geçiş süreleri için Not. P2S istemcileri Güncelleştirme tamamlandıktan sonra eski profillerini kullanarak bağlanmak mümkün olmayacaktır. Yeni VPN istemci profilleri oluşturmak ve bunları istemcilere yüklemeniz gerekir.

## <a name="3-generate-vpn-client-configuration-profile"></a>3. VPN istemci yapılandırma profili oluştur

Yeni VPN profili (VPN istemcisi yapılandırma dosyalarını), sertifika geçirileceğini sonra Azure portalından indirin. Adımlar için bkz: [VPN istemcisi yapılandırma dosyalarını oluşturma ve yükleme](point-to-site-vpn-client-configuration-azure-cert.md). Yeni istemci sertifikalarını oluşturmak gerekmez.

## <a name="4-deploy-vpn-client-profile"></a>4. VPN istemci profilini Dağıt

Yeni profili tüm noktadan siteye VPN istemcilerine dağıtın. VPN istemcileri, noktadan siteye bağlantılar için yeni VPN profili indirilir ve istemci cihaza kadar bağlantı kaybedersiniz. VPN istemci bilgisayarlarda yüklü olan istemci sertifikalarını verilmesi gerekmez.

## <a name="5-connect-the-vpn-client"></a>5. VPN istemcisi bağlama

Normalde yaptığınız gibi VPN istemcisinden Azure'a bağlanın.
