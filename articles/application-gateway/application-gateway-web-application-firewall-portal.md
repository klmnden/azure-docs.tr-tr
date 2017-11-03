---
title: "Bir uygulama ağ geçidi ile bir web uygulaması güvenlik duvarı güncelle | Microsoft Docs"
description: "Portalı kullanarak bir web uygulaması güvenlik duvarı ile bir uygulama ağ geçidi oluşturmayı öğrenin"
services: application-gateway
documentationcenter: na
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: b561a210-ed99-4ab4-be06-b49215e3255a
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: davidmu
ms.openlocfilehash: bfc06c1b44974fd17a3794654503d21d6407a917
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-application-gateway-with-a-web-application-firewall-by-using-the-portal"></a>Portalı kullanarak bir web uygulaması güvenlik duvarı ile bir uygulama ağ geçidi oluşturma

> [!div class="op_single_selector"]
> * [Azure portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Azure CLI](application-gateway-web-application-firewall-cli.md)

Bir web uygulaması Güvenlik Duvarı (WAF) oluşturmayı öğrenin-uygulama ağ geçidi etkin.

Azure uygulama ağ geçidi olarak WAF web uygulamaları, SQL ekleme gibi ortak web tabanlı saldırıları, siteler arası komut dosyası saldırıları ve oturumu ele geçirilmesini korur. Bir WAF birçok OWASP üst 10 ortak web güvenlik açıklarına karşı korur.

## <a name="scenarios"></a>Senaryolar

Bu makalede iki senaryo sunar. İlk senaryoda, bilgi nasıl [WAF ile bir uygulama ağ geçidi oluşturma](#create-an-application-gateway-with-web-application-firewall). İkinci senaryoda, bilgi nasıl [var olan bir uygulama ağ geçidi için bir WAF ekleme](#add-web-application-firewall-to-an-existing-application-gateway).

![Senaryo örneği][scenario]

> [!NOTE]
> Uygulama ağ geçidi için özel sistem durumu araştırmalarının, arka uç havuzu adresleri ve ek kurallar ekleyebilirsiniz. Bu uygulamalar, uygulama ağ geçidi yapılandırıldıktan sonra ve ilk dağıtım sırasında değil yapılandırılır.

## <a name="before-you-begin"></a>Başlamadan önce

 Bir uygulama ağ geçidi, kendi alt gerektirir. Bir sanal ağ oluşturduğunuzda, birden çok alt ağa sahip için yeterli adres alanı bıraktığınızdan emin olun. Bir uygulama ağ geçidi bir alt ağa dağıttıktan sonra yalnızca ek uygulama ağ geçitleri alt ağa eklenebilir.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Var olan bir uygulama ağ geçidi için bir web uygulaması güvenlik duvarı ekleme

Bu örnek bir WAF desteklemek için varolan uygulama ağ geçidi güncelleştirmeleri **önleme** modu.

1. Azure portalında **Sık Kullanılanlar** bölmesinde, **tüm kaynakları**. Üzerinde **tüm kaynakları** dikey penceresinde, var olan uygulama ağ geçidi seçin. Zaten seçili abonelik çeşitli kaynaklar varsa, ad girin **adına göre filtrele** kolay erişim için DNS bölgesi kutusu.

   ![Varolan bir uygulama ağ geçidi seçimi][1]

2. Seçin **Web uygulaması güvenlik duvarı**ve uygulama ağ geçidi ayarlarını güncelleştirin. Güncelleştirme tamamlandığında seçin **kaydetmek**. 

3. Bir WAF desteklemek için varolan uygulama ağ geçidi güncelleştirmek için aşağıdaki ayarları kullanın:

   | **Ayar** | **Değer** | **Ayrıntılar**
   |---|---|---|
   |**WAF katmanına yükseltme**| İşaretli | Bu seçenek, uygulama ağ geçidi WAF katmanına katmanı ayarlar.|
   |**Güvenlik Duvarı durumu**| Etkin | Bu ayar, Güvenlik Duvarı'nı WAF sağlar.|
   |**Güvenlik Duvarı modu** | Önleme | Bu ayar, bir WAF kötü amaçlı trafiği ile nasıl ilgileneceğini değildir. **Algılama** modu yalnızca olayları günlüğe kaydeder. **Önleme** modu olayları günlüğe kaydeder ve kötü amaçlı trafiği durdurur.|
   |**Kural kümesi**|3.0|Bu ayar belirler [çekirdek kural kümesi](application-gateway-web-application-firewall-overview.md#core-rule-sets) arka uç havuzu üyelerini korumak için kullanılır.|
   |**Devre dışı kurallarını yapılandırma**|Değişir|Olası hatalı pozitif sonuç önlemek için bu ayarı belirli devre dışı bırakmak için kullanabileceğiniz [kurallar ve kural gruplarını](application-gateway-crs-rulegroups-rules.md).|

    >[!NOTE]
    > WAF SKU için var olan bir uygulama ağ geçidi yükseltme yaparken, SKU boyutunu değişikliklerini **orta**. Yapılandırma tamamlandıktan sonra bu ayarı yeniden yapılandırabilirsiniz.

    ![Temel ayarlar][2-1]

    > [!NOTE]
    > WAF günlükleri görüntülemek, tanılamayı etkinleştirin ve seçmek için **ApplicationGatewayFirewallLog**. Örnek sayısı seçin **1** yalnızca sınama amaçlı. Bir örnek sayısı altında önermiyoruz **2** SLA kapsamında değildir çünkü. Küçük ağ geçidi, bir WAF kullandığınızda kullanılamaz.

## <a name="create-an-application-gateway-with-a-web-application-firewall"></a>Web uygulaması güvenlik duvarı ile bir uygulama ağ geçidi oluşturma

Bu senaryo aşağıdakileri yapar:

* Orta WAF uygulama ağ geçidi ile iki örnek oluşturun.
* Bir ayrılmış 10.0.0.0/16 CIDR bloğu ile AdatumAppGatewayVNET adlı bir sanal ağ oluşturun.
* Appgatewaysubnet adlı, CIDR bloğu olarak 10.0.0.0/28 kullanan bir alt ağ oluşturacaksınız.
* SSL yük boşaltımı için bir sertifika yapılandırın.

1. [Azure Portal](https://portal.azure.com) oturum açın. Zaten bir hesabınız yoksa, için kaydolabilirsiniz bir [ücretsiz bir aylık deneme](https://azure.microsoft.com/free).

2. İçinde **Sık Kullanılanlar** portalındaki, seçim bölmesi **yeni**.

3. Üzerinde **yeni** dikey penceresinde, select **ağ**. Üzerinde **ağ** dikey penceresinde, select **uygulama ağ geçidi**aşağıdaki görüntüde gösterildiği gibi:

    ![Uygulama ağ geçidi oluşturma][1]

4. Üzerinde **Temelleri** görünür, dikey penceresinde, aşağıdaki değerleri girin ve ardından **Tamam**:

   | **Ayar** | **Değer** | **Ayrıntılar**
   |---|---|---|
   |**Ad**|AdatumAppGateway|Uygulama ağ geçidi adı.|
   |**Katmanı**|WAF|Standart ve WAF değerleri kullanılabilir. Bir WAF hakkında daha fazla bilgi için bkz: [Web uygulaması güvenlik duvarı](application-gateway-web-application-firewall-overview.md).|
   |**SKU boyutu**|Orta|Standart katmanı seçenekleri **küçük**, **orta**, ve **büyük**. WAF katmanı seçenekleri **orta** ve **büyük** yalnızca.|
   |**Örnek sayısı**|2|Yüksek kullanılabilirlik için uygulama ağ geçidi örneği sayısı. Örnek sayısı 1 yalnızca sınama amacıyla kullanın.|
   |**Abonelik**|[Aboneliğiniz]|Uygulama ağ geçidi oluşturmak için kullanılacak bir abonelik seçin.|
   |**Kaynak grubu**|**Yeni Oluştur:** AdatumAppGatewayRG|Bir kaynak grubu oluşturun. Kaynak grubu adı, seçili abonelik içinde benzersiz olmalıdır. Kaynak grupları hakkında daha fazla bilgi için, [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#resource-groups)’a genel bakış makalesini okuyun.|
   |**Konum**|Batı ABD||

   ![Temel ayarlarını yapılandırma][2-2]

5. Üzerinde **ayarları** altında görüntülenen dikey **sanal ağ**seçin **sanal ağ seçin**. Üzerinde **Seç sanal ağ** dikey penceresinde, select **Yeni Oluştur**.

   ![Sanal Ağ Seçimi][2]

6. Üzerinde **oluşturma sanal ağ dikey**, aşağıdaki değerleri girin ve ardından **Tamam**. **Alt** alanını **ayarları** dikey penceresinde, seçtiğiniz alt ağ ile doldurulur.

   |**Ayar** | **Değer** | **Ayrıntılar** |
   |---|---|---|
   |**Ad**|AdatumAppGatewayVNET|Uygulama ağ geçidi adı.|
   |**Adres alanı**|10.0.0.0/16| Bu değer sanal ağ adres alanı olur.|
   |**Alt ağ adı**|AppGatewaySubnet|Uygulama ağ geçidi alt ağı adı.|
   |**Alt ağ adres aralığı**|10.0.0.0/28 | Bu alt ağ, sanal ağ arka uç havuzu üyeleri için daha fazla ek alt ağlar sağlar.|

7. Üzerinde **ayarları** altında dikey **ön uç IP yapılandırmasını**seçin **ortak** olarak **IP adresi türü**.

8. Üzerinde **ayarları** altında dikey **genel IP adresi**seçin **genel bir IP adresi seçin**. Üzerinde **genel IP adresi seçin** dikey penceresinde, select **Yeni Oluştur**.

   ![Ortak IP adresi seçimi][3]

9. Üzerinde **ortak IP adresi oluştur** dikey penceresinde, varsayılan değeri kabul edin ve seçin **Tamam**. **Genel IP adresi** alanı seçtiğiniz ortak IP adresi ile doldurulur.

10. Üzerinde **ayarları** altında dikey **dinleyici Yapılandırması**seçin **HTTP** altında **Protokolü**. Bir sertifika kullanmak için gerekli **HTTPS**. Sertifikanın özel anahtarı gereklidir. Sertifika .pfx verilmesini sağlamak ve dosyası için parola girin.

11. Belirli ayarlarını yapılandırabilirsiniz **WAF**.

   |**Ayar** | **Değer** | **Ayrıntılar** |
   |---|---|---|
   |**Güvenlik Duvarı durumu**| Etkin| Bu ayar WAF üzerinde veya devre dışı bırakır.|
   |**Güvenlik Duvarı modu** | Önleme| Bu ayar, kötü amaçlı trafiği WAF gerçekleştirdiği eylemleri belirler. **Algılama** modu yalnızca trafik günlüğe kaydeder. **Önleme** modu günlüğe kaydeder ve trafik 403 yetkisiz yanıt vermiyor.|


12. Gözden geçirme **Özet** sayfasında ve seçin **Tamam**. Artık uygulama ağ geçidi sıraya ve oluşturuldu.

13. Sonra uygulama ağ geçidi oluşturulup, kendisine uygulama ağ geçidi yapılandırmasının devam edebilmesi için portalında gidin.

    ![Uygulama ağ geçidi kaynağı görünümü][10]

Bu adımları dinleyicisi, arka uç havuzu, arka uç HTTP ayarları ve kuralları için varsayılan ayarlarla temel uygulama ağ geçidi oluşturun. Sağlama tamamlandıktan sonra başarılı bir şekilde, bu ayarları dağıtımınızı uyacak şekilde değiştirebilirsiniz.

> [!NOTE]
> Temel WAF yapılandırma ile oluşturulmuş uygulama ağ geçitleri için koruma CRS 3.0 ile yapılandırılır.

## <a name="next-steps"></a>Sonraki adımlar

Bir özel etki alanı ad yapılandırmak için [genel IP adresi](../dns/dns-custom-domain.md#public-ip-address), Azure DNS veya başka bir DNS sağlayıcınız kullanabilirsiniz.

Tanılama ile WAF algılanan ya da engelledi olayları günlüğe kaydetmeyi yapılandırmak için bkz: [uygulama ağ geçidi tanılama](application-gateway-diagnostics.md).

Özel sistem durumu araştırmalarının oluşturmak için bkz: [bir özel durum araştırması oluşturmak](application-gateway-create-probe-portal.md).

SSL boşaltma yapılandırmak ve web sunucularınızın kapalı maliyetli SSL abonelik almak için bkz: [yapılandırma SSL yük boşaltımı](application-gateway-ssl-portal.md).

<!--Image references-->
[1]: ./media/application-gateway-web-application-firewall-portal/figure1.png
[2]: ./media/application-gateway-web-application-firewall-portal/figure2.png
[2-1]: ./media/application-gateway-web-application-firewall-portal/figure2-1.png
[2-2]: ./media/application-gateway-web-application-firewall-portal/figure2-2.png
[3]: ./media/application-gateway-web-application-firewall-portal/figure3.png
[10]: ./media/application-gateway-web-application-firewall-portal/figure10.png
[scenario]: ./media/application-gateway-web-application-firewall-portal/scenario.png
