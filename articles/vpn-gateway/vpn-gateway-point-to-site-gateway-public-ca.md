---
title: Geçiş otomatik olarak imzalanan ortak CA sertifikaları P2S ağ geçitleri için | Azure VPN ağ geçidi | Microsoft Docs
description: Bu makale P2S ağ geçitleri için yeni ortak CA sertifikaları için başarılı bir şekilde geçiş yardımcı olur.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: cherylmc
ms.openlocfilehash: c6d696f8bf5f649321ee0be93ae13571a7ba3019
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55896740"
---
# <a name="transition-from-self-signed-to-public-ca-certificates-for-p2s-gateways"></a>Geçiş için ortak CA sertifikaları P2S ağ geçitleri için otomatik imzalı

Azure VPN ağ geçidi artık ağ geçitleri P2S bağlantıları için otomatik olarak imzalanan sertifikalar verir. Verilen sertifikaların artık bir ortak sertifika yetkilisi (CA) tarafından imzalanmıştır. Ancak, eski ağ geçidi hala otomatik olarak imzalanan sertifikaları kullanıyor olabilir. Bu otomatik olarak imzalanan sertifikalar sona erme tarihlerinin ve ortak CA sertifikaları için geçiş gerekir.

Daha önce ağ geçidi için kendinden imzalı bir sertifika her 18 ay güncelleştirilmesi gerekmiyor. VPN istemcisi yapılandırma dosyalarını sonra oluşturulur ve tüm P2S istemcilere imzalanmasını gerekiyordu. Ortak CA sertifikaları taşıyarak bu sınırlama ortadan kaldırır. Sertifikaları için geçiş ek olarak, bu değişiklik platform iyileştirmeleri, daha iyi ölçümleri ve kararlılığı sağlar.

Yalnızca eski ağ geçitleri bu değişiklikten etkilenmez. Ağ geçidi sertifikanızı geçmiş olması gerekiyorsa, Azure portalında iletişimi veya bildirim alırsınız. Ağ geçidiniz bu makaledeki adımları kullanarak etkilenir bakabilirsiniz.

>[!IMPORTANT]
>Geçiş için Mart zamanlandı 18:00 UTC'de 09,2019 başlatılıyor. Farklı zaman penceresi tercih ederseniz bir destek talebi oluşturabilirsiniz. Aşağıdaki windows isteyebilirsiniz:
>
>* 25 Şubat 06:00 UTC
>* 25 Şubat 18:00 UTC
>* 1 Mart'ta 06:00 UTC
>* 1 Mart'ta 18:00 UTC
>
>**Kalan tüm ağ geçitleri 09 Mart 18:00 UTC'de başlangıç 2019 tarihinde geçeceğiyle**.
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