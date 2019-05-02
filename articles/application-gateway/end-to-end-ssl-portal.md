---
title: Hızlı Başlangıç - Azure Application Gateway - Azure portalı ile uçtan uca SSL şifrelemesini yapılandırma | Microsoft Docs
description: Azure Application Gateway uçtan uca SSL şifrelemesi ile oluşturmak için Azure portalını kullanmayı öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 4/30/2019
ms.author: absha
ms.custom: mvc
ms.openlocfilehash: bd165f81b45e3ae0c121fb8876ed88e68d493195
ms.sourcegitcommit: ed66a704d8e2990df8aa160921b9b69d65c1d887
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64946791"
---
# <a name="configure-end-to-end-ssl-by-using-application-gateway-with-the-portal"></a>Portal ile uygulama ağ geçidi'ni kullanarak uçtan uca SSL'yi yapılandırma

Bu makalede bir uygulama ağ geçidi ile uçtan uca SSL şifrelemesini yapılandırmak için Azure portalını kullanmayı gösterir v1 SKU.  

> [!NOTE]
> Uygulama ağ geçidi v2 SKU etkinleştirme uçtan uca yapılandırma için güvenilen kök sertifika gerektirir. Güvenilen kök sertifika eklemek için portal desteği henüz mevcut değildir. Bu nedenle, durumunda v2 SKU bkz [PowerShell kullanarak uçtan uca SSL'yi yapılandırma](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

Bir uygulama ağ geçidi ile uçtan uca SSL'yi yapılandırmak için bir sertifika ağ geçidi için gereklidir ve arka uç sunucuları için sertifikaları gereklidir. Ağ geçidi sertifikası, bir simetrik anahtar SSL protokolü belirtimi uyarınca türetmek için kullanılır. Simetrik anahtar, ardından şifrelemek ve şifresini çözmek için ağ geçidi gönderilen trafiği için kullanılır. Uçtan uca SSL şifrelemesi için arka uç uygulama ağ geçidiyle Güvenilenler listesinde olmalıdır. Bunu yapmak için uygulama ağ geçidi arka uç sunucuların, kimlik doğrulama sertifikası olarak da bilinir, ortak sertifika karşıya yükleyin. Sertifika ekleme, uygulama ağ geçidi yalnızca bilinen arka uç örnekleriyle iletişim sağlar. Bu daha fazla uçtan uca iletişimin güvenliğini sağlar.

Daha fazla bilgi için bkz. [SSL sonlandırma ve uçtan uca SSL](https://docs.microsoft.com/azure/application-gateway/ssl-overview).

## <a name="create-a-new-application-gateway-with-end-to-end-ssl"></a>Uçtan uca SSL ile yeni bir uygulama ağ geçidi oluşturma

Uçtan uca SSL şifrelemesi ile yeni bir uygulama ağ geçidi oluşturmak için önce yeni bir uygulama ağ geçidi oluşturulurken, SSL sonlandırma etkinleştirmeniz gerekir. Bu, istemci ve uygulama ağ geçidi arasındaki iletişim için SSL şifrelemesini etkinleştirir. Ardından, uçtan uca SSL şifrelemesini yerine getirmeye uygulama ağ geçidi ve arka uç sunucuları arasındaki iletişim için SSL şifrelemesini etkinleştirmek için HTTP ayarlarında arka uç sunucular için güvenilir sertifikaları için gerekir.

### <a name="enable-ssl-termination-while-creating-a-new-application-gateway"></a>Yeni bir uygulama ağ geçidi oluşturulurken, SSL sonlandırmayı etkinleştirin

Anlamak için bu makaleyi okuyun nasıl [yeni bir uygulama ağ geçidi oluşturulurken, SSL sonlandırmayı etkinleştirme](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal).

### <a name="whitelist-certificates-for-backend-servers"></a>Arka uç sunucular için güvenilir sertifikaları

1. Seçin **tüm kaynakları**ve ardından **myAppGateway**.

2. Seçin **HTTP ayarları** sol menüden. Azure otomatik olarak oluşturulan varsayılan HTTP ayarı **appGatewayBackendHttpSettings**, uygulama ağ geçidi oluşturduğunuzda. 

3. Seçin **appGatewayBackendHttpSettings**.

4. Altında **Protokolü**seçin **HTTPS**. Bir bölme için **arka uç kimlik doğrulama sertifikaları** görünür. 

5. Altında **arka uç kimlik doğrulama sertifikaları**, seçin **Yeni Oluştur**.

6. Uygun girin **adı**.

7. Kullanarak sertifikayı karşıya yükleme **karşıya CER sertifikası** kutusunu.![ addcert](./media/end-to-end-ssl-portal/addcert.png)

   > [!NOTE]
   > Bu adımda sağlanan sertifikanın ortak anahtarını .pfx sertifikasını arka uçta mevcut olmalıdır. Talep ve kanıt akıl (CER) biçiminde arka uç sunucuda yüklü sertifika (kök sertifika değil) dışarı aktarma ve bu adımı kullanın. Bu adım beyaz arka uç uygulama ağ geçidiyle.

8. **Kaydet**’i seçin.

## <a name="enable-end-to-end-ssl-for-existing-application-gateway"></a>Mevcut uygulama ağ geçidi için uçtan uca SSL'yi etkinleştirme

Mevcut uygulama ağ geçidi ile uçtan uca SSL şifrelemesini yapılandırmak için ilk etkinleştir SSL sonlandırma, dinleyici gerekir. Bu, istemci ve uygulama ağ geçidi arasındaki iletişim için SSL şifrelemesini etkinleştirir. Ardından, uçtan uca SSL şifrelemesini yerine getirmeye uygulama ağ geçidi ve arka uç sunucuları arasındaki iletişim için SSL şifrelemesini etkinleştirmek için HTTP ayarlarında arka uç sunucular için güvenilir sertifikaları için gerekir.

SSL sonlandırma etkinleştirmek için bir dinleyici HTTPS protokolünü ve sertifika ile kullanmak gerekir. Var olan bir dinleyici Protokolü değiştiremezsiniz. Bu nedenle, HTTPS protokolünü ve sertifika ile var olan bir dinleyici kullanın veya yeni dinleyici oluşturun ya da seçebilirsiniz. Önceki seçtiğiniz uyarıdaki yoksayabilirsiniz adımları aşağıda belirtilen **mevcut application Gateway'de etkinleştirme SSL sonlandırma** ve doğrudan gitme **arka uç sunucular için güvenilir sertifikaları** bölümü. İkinci seçeneği seçerseniz, aşağıdaki adımları kullanın.

### <a name="enable-ssl-termination-in-existing-application-gateway"></a>Mevcut uygulama ağ geçidinde SSL sonlandırmayı etkinleştirin

1. Seçin **tüm kaynakları**ve ardından **myAppGateway**.

2. Seçin **dinleyicileri** sol menüden.

3. Arasında seçim **temel** ve **çok siteli** ihtiyacınıza göre dinleyicisi.

4. Altında **Protokolü**seçin **HTTPS**. Bir bölme için **sertifika** görünür.

5. İstemci ve uygulama ağ geçidi arasında SSL sonlandırma için kullanmayı planladığınız bir PFX sertifikasını karşıya yükleyin.

   > [!NOTE]
   > Test amacıyla otomatik olarak imzalanan bir sertifika kullanabilirsiniz. Üretim iş yükleri için otomatik olarak imzalanan sertifika kullanmamalıdır. Bilgi edinmek için nasıl [otomatik olarak imzalanan bir sertifika oluşturmak](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal#create-a-self-signed-certificate).

6. Eklemek için gerekli diğer ayarları **dinleyici** ihtiyacınıza göre.

7. Kaydetmek için **Tamam**’ı seçin.

### <a name="whitelist-certificates-for-backend-servers"></a>Arka uç sunucular için güvenilir sertifikaları

1. Seçin **tüm kaynakları**ve ardından **myAppGateway**.

2. Seçin **HTTP ayarları** sol menüden. Var olan bir arka uç HTTP ya da güvenilir sertifikaları için ayarlama veya yeni bir HTTP ayarı oluşturun. İçindeki adım varsayılan HTTP ayarı için güvenilir sertifika yapacağız **appGatewayBackendHttpSettings**.

3. Seçin **appGatewayBackendHttpSettings**.

4. Altında **Protokolü**seçin **HTTPS**. Bir bölme için **arka uç kimlik doğrulama sertifikaları** görünür. 

5. Altında **arka uç kimlik doğrulama sertifikaları**, seçin **Yeni Oluştur**.

6. Uygun girin **adı**.

7. Kullanarak sertifikayı karşıya yükleme **karşıya CER sertifikası** kutusunu.![ addcert](./media/end-to-end-ssl-portal/addcert.png)

   > [!NOTE]
   > Bu adımda sağlanan sertifikanın ortak anahtarını .pfx sertifikasını arka uçta mevcut olmalıdır. Talep ve kanıt akıl (CER) biçiminde arka uç sunucuda yüklü sertifika (kök sertifika değil) dışarı aktarma ve bu adımı kullanın. Bu adım beyaz arka uç uygulama ağ geçidiyle.

8. **Kaydet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure CLI kullanarak bir uygulama ağ geçidi ile web trafiğini yönetme](./tutorial-manage-web-traffic-cli.md)
