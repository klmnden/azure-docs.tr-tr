---
title: P2S ağ geçitleri için ortak CA sertifikaları için geçiş | Azure VPN ağ geçidi | Microsoft Docs
description: Bu makale P2S ağ geçitleri için yeni ortak CA sertifikaları için başarılı bir şekilde geçiş yardımcı olur.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: cherylmc
ms.openlocfilehash: 1d45e1a5e4053ead4330967c5e250c0797c19fe7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65827465"
---
# <a name="transition-to-a-public-ca-gateway-certificate-for-p2s"></a>P2S için genel CA ağ geçidi sertifikasına geçiş

Azure VPN ağ geçidi, ağ geçitleri P2S bağlantıları için artık Azure düzeyinde otomatik olarak imzalanan sertifikalar verir. Verilen sertifikaların artık bir ortak sertifika yetkilisi (CA) tarafından imzalanmıştır. Ancak, bazı eski ağ geçitlerinin yine de otomatik olarak imzalanan sertifikaları kullanıyor olabilir. Bu otomatik olarak imzalanan sertifikalar sona erme tarihlerinin ve ortak CA sertifikaları için geçiş gerekir.

>[!NOTE]
> * P2S istemci kimlik doğrulaması için kullanılan otomatik olarak imzalanan sertifikaları, bu Azure düzey sertifika değişiklikten etkilenmez. Sorun ve otomatik olarak imzalanan sertifikalar normal şekilde kullanmaya devam edebilirsiniz.
>

Bu bağlamda bir ek Azure düzey sertifika sertifikalardır. Kendi otomatik olarak imzalanan kök sertifika ve kimlik doğrulaması için istemci sertifikası oluştururken kullandığınız sertifika zincirleri değiller. Bu sertifikaları yapmanızdan ve tarihler, bunu yapmak için bunları oluşturulan sona erer.

Daha önce (arka planda, Azure tarafından verilen) ağ geçidi için kendinden imzalı bir sertifika her 18 ay güncelleştirilmesi gerekmiyor. VPN istemcisi yapılandırma dosyalarını sonra oluşturulur ve tüm P2S istemcilere imzalanmasını gerekiyordu. Ortak CA sertifikaları taşıyarak bu sınırlama ortadan kaldırır. Sertifikaları için geçiş ek olarak, bu değişiklik platform iyileştirmeleri, daha iyi ölçümleri ve kararlılığı sağlar.

Yalnızca eski ağ geçitleri bu değişiklikten etkilenmez. Ağ geçidi sertifikanızı geçmiş olması gerekiyorsa, Azure portalında iletişimi veya bildirim alırsınız. Ağ geçidiniz bu makaledeki adımları kullanarak etkilenir bakabilirsiniz.

> [!IMPORTANT]
> 12 Mayıs 2019 üzerinde ortak CA sertifikaları geçişi tamamlandı. **Bu belge, 31 Mayıs 2019 üzerinde kaldırılacak.**

## <a name="1-verify-your-certificate"></a>1. Sertifikanızı doğrulayın

### <a name="resource-manager"></a>Resource Manager

1. Bu güncelleştirmeyle etkilenip etkilenmediğini denetleyin. İçindeki adımları kullanarak, geçerli VPN istemci yapılandırması indirme [bu makalede](point-to-site-vpn-client-configuration-azure-cert.md).

2. Açın veya zip dosyasını ayıklayın ve "Genel" klasöre göz atın. Genel klasöründe biri olan iki dosya görürsünüz *VPNSettings.xml*.
3. Açık *VPNSettings.xml* bir xml Görüntüleyici/düzenleyicisinde. Xml dosyasında aşağıdaki alanları ara:

   * `<ServerCertRootCn>DigiCert Global Root CA</ServerCertRootCn>`
   * `<ServerCertIssuerCn>DigiCert Global Root CA</ServerCertIssuerCn>`
4. Varsa *ServerCertRotCn* ve *ServerCertIssuerCn* "DigiCert genel kök CA" olan, bu güncelleştirme ile etkilenmez ve bu makaledeki adımlar ile devam etmek ihtiyacınız yoktur. Ancak, başka bir şey Göster, ağ geçidi sertifikanızı güncelleştirmenin parçası olan ve geçirilirsiniz.

### <a name="classic"></a>Klasik

1. Bir istemci bilgisayara yola gidin `%appdata%/Microsoft/Network/Connections/Cm/<gatewayID>`. Ağ geçidi kimliği klasöründe, sertifikayı görüntüleyebilirsiniz.
2. Sertifika için Genel sekmesinde, sertifika verme yetkilisi "DigiCert genel kök CA" olduğunu doğrulayın. Bu veren yetkili dışındaki herhangi bir şey varsa, ağ geçidi sertifikanızı güncelleştirmenin parçası olan ve yapılacaktır.

## <a name="2-check-certificate-transition-schedule"></a>2. Sertifika geçişi zamanlamayı denetle

Sertifikanızı güncelleştirmenin parçası ise, ağ geçidi sertifikanızı geçirilecektir. Başvurmak **önemli** geçiş süreleri için Not. P2S istemcileri Güncelleştirme tamamlandıktan sonra eski profillerini kullanarak bağlanmak mümkün olmayacaktır. Yeni VPN istemci profilleri oluşturmak ve bunları istemcilere yüklemeniz gerekir.

## <a name="3-generate-vpn-client-configuration-profile"></a>3. VPN istemci yapılandırma profili oluştur

Yeni VPN profili (VPN istemcisi yapılandırma dosyalarını), sertifika geçirileceğini sonra Azure portalından indirin. Adımlar için bkz: [VPN istemcisi yapılandırma dosyalarını oluşturma ve yükleme](point-to-site-vpn-client-configuration-azure-cert.md). Yeni istemci sertifikalarını oluşturmak gerekmez.

## <a name="4-deploy-vpn-client-profile"></a>4. VPN istemci profilini Dağıt

Yeni profili tüm noktadan siteye VPN istemcilerine dağıtın. VPN istemcileri, noktadan siteye bağlantılar için yeni VPN profili indirilir ve istemci cihaza kadar bağlantı kaybedersiniz. VPN istemci bilgisayarlarda yüklü olan istemci sertifikalarını verilmesi gerekmez.

## <a name="5-connect-the-vpn-client"></a>5. VPN istemcisi bağlama

Normalde yaptığınız gibi VPN istemcisinden Azure'a bağlanın.
