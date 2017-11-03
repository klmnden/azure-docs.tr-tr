---
title: "Azure uygulama ağ geçidi ile birden çok sitesi barındırmak | Microsoft Docs"
description: "Bu sayfa, Azure portal ile aynı ağ geçidinde birden çok web uygulamalarını barındırmak için mevcut bir Azure uygulama ağ geçidi yapılandırmak için yönergeler sağlar."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: 95f892f6-fa27-47ee-b980-7abf4f2c66a9
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: davidmu
ms.openlocfilehash: 28a7fcb3e08a9c4b6a27e9fbc8d3ebae309adc62
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-an-existing-application-gateway-for-hosting-multiple-web-applications"></a>Birden çok web uygulamalarını barındırmak için mevcut bir uygulama ağ geçidi yapılandırma

> [!div class="op_single_selector"]
> * [Azure portal](application-gateway-create-multisite-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-multisite-azureresourcemanager-powershell.md)
> 
> 

Birden çok siteyi barındıran aynı uygulama ağ geçidi birden çok web uygulamasına dağıtmanıza olanak tanır. Hangi dinleyicisi trafiği alacağını belirlemek için gelen HTTP isteği, ana bilgisayar üst bilgisi kullanır. Dinleyici sonra ağ geçidi kuralları tanımı içinde yapılandırıldığı gibi uygun arka uç havuzu trafiğini yönlendirir. SSL etkin web uygulamaları, uygulama ağ geçidi web trafiği için doğru dinleyici seçmek için sunucu adı göstergesi (SNI) uzantısı kullanır. Birden çok sitesi barındırmak için bir genel Yük Dengeleme isteklerini farklı web etki alanları için farklı bir arka uç sunucu havuzları için kullanılır. Benzer şekilde aynı kök etki alanının birden çok alt de aynı uygulama ağ geçidinde barındırılan.

## <a name="scenario"></a>Senaryo

Aşağıdaki örnekte, iki arka uç sunucu havuzu ile trafiği contoso.com ve fabrikam.com için uygulama ağ geçidi hizmet: contoso sunucu havuzu ve fabrikam sunucu havuzu. Benzer Kurulum app.contoso.com ve blog.contoso.com gibi ana bilgisayar alt etki alanları için kullanılabilir.

![çok siteli senaryosu][multisite]

## <a name="before-you-begin"></a>Başlamadan önce

Bu senaryo, var olan bir uygulama ağ geçidi için çok siteli desteği ekler. Bu senaryoyu tamamlamak için mevcut bir uygulama ağ geçidi yapılandırmak kullanılabilir olması gerekir. Ziyaret [portalı kullanarak bir uygulama ağ geçidi oluşturma](application-gateway-create-gateway-portal.md) portalda temel uygulama ağ geçidi oluşturma hakkında bilgi edinmek için.

Uygulama ağ geçidi güncelleştirmek için gereken adımlar şunlardır:

1. Her site için kullanılacak arka uç havuzları oluşturun.
2. Dinleyici oluşturmak için her site, uygulama ağ geçidi destekler.
3. Her dinleyicisi uygun arka uç ile eşlemek için kurallar oluşturun.

## <a name="requirements"></a>Gereksinimler

* **Arka uç sunucusu havuzu:** Arka uç sunucularının IP adreslerinin listesi. Listede bulunan IP adresleri sanal ağ alt ağına veya genel IP/VIP’ye ait olmalıdır. FQDN de kullanılabilir.
* **Arka uç sunucu havuzu ayarları**: Her havuzun bağlantı noktası, protokol ve tanımlama bilgisi temelli benzeşim gibi ayarları vardır. Bu ayarlar bir havuza bağlıdır ve havuzdaki tüm sunuculara uygulanır.
* **Ön uç bağlantı noktası:** Bu bağlantı noktası uygulama ağ geçidinde açılan genel bağlantı noktasıdır. Bu bağlantı noktasında trafik olursa arka uç sunuculardan birine yönlendirilir.
* **Dinleyici:** Dinleyicide bir ön uç bağlantı noktası, bir protokol (Http veya Https, bu değerler büyük/küçük harfe duyarlıdır) ve SSL sertifika adı (SSL yük boşaltımı yapılandırılıyorsa) vardır. Çok siteli kullanan bir uygulama ağ geçitleri için ana bilgisayar adı ve SNI göstergeleri de eklenir.
* **Kural:** kural dinleyiciyi arka uç sunucusu havuzunu bağlar ve trafiği için belli bir Dinleyicide trafik olduğunda hangi arka uç sunucu havuzuna yönlendirileceğini belirler. Kuralları listelendikleri sırada işlenir ve trafik belirginliğe bağımsız olarak eşleşen ilk kural üzerinden yönlendirilir. Temel bir dinleyici ve çok siteli dinleyicisi hem aynı bağlantı noktasında kullanan bir kural kullanarak bir kuralı varsa, örneğin, çok siteli dinleyicisi kuralla beklendiği şekilde çalışması için temel dinleyicisi çok siteli kuralı için sırada olan kural önce listelenmiş olması gerekir. 
* **Sertifikalar:** her dinleyicisi benzersiz bir sertifika gerektirir, bu örnekte 2 dinleyicileri için çok siteli oluşturulur. İki .pfx sertifikaları ve bunların parolalarını oluşturulması gerekir.

## <a name="create-back-end-pools-for-each-site"></a>Her site için arka uç havuzları oluşturma

Uygulama ağ geçidi destekler gerekirse, bu durumda 2 olması oluşturulur, contoso11.com için diğeri için fabrikam11.com her site için bir arka uç havuzu.

### <a name="step-1"></a>1. Adım

Var olan bir uygulama ağ geçidi (https://portal.azure.com) Azure portalında gidin. Seçin **arka uç havuzları** tıklatıp **Ekle**

![arka uç havuzları ekleme][7]

### <a name="step-2"></a>2. Adım

Arka uç havuzu için bilgileri doldurun **pool1**, FQDN'ler ve IP adresleri arka uç sunucuları için ekleme ve tıklayın **Tamam**

![arka uç havuzu pool1 ayarları][8]

### <a name="step-3"></a>3. Adım

Arka uç havuzları dikey penceresinde tıklatın **Ekle** ek bir arka uç havuzu eklemek için **pool2**, FQDN'ler ve IP adresleri arka uç sunucuları için ekleme ve tıklayın **Tamam**

![arka uç havuzu pool2 ayarları][9]

## <a name="create-listeners-for-each-back-end"></a>Her arka uç için dinleyicileri oluşturma

Application Gateway, aynı genel IP adresinde ve bağlantı noktasında birden çok web sitesini barındırmak için HTTP 1.1 barındırma bilgilerini kullanır. Portalda oluşturulan temel dinleyicisi, bu özelliği içermiyor.

### <a name="step-1"></a>1. Adım

Tıklatın **dinleyicileri** varolan uygulama ağ geçidi ve tıklatın **çok siteli** ilk dinleyicisi eklemek için.

![dinleyicileri genel bakış dikey penceresi][1]

### <a name="step-2"></a>2. Adım

Dinleyici için bilgileri doldurun. Bu örnekte SSL sonlandırma yapılandırılır, yeni bir ön uç bağlantı noktası oluşturun. SSL sonlandırma için kullanılacak .pfx sertifikasını yükleyin. Yalnızca standart temel dinleyicisi dikey penceresine karşılaştırıldığında bu dikey penceredeki hostname farktır.

![Dinleyici özellikleri dikey penceresi][2]

### <a name="step-3"></a>3. Adım

Tıklatın **çok siteli** ve ikinci site için önceki adımda açıklandığı gibi başka bir dinleyici oluşturun. İkinci dinleyici için farklı bir sertifika kullandığınızdan emin olun. Yalnızca standart temel dinleyicisi dikey penceresine karşılaştırıldığında bu dikey penceredeki hostname farktır. ' I tıklatın ve dinleyici için bilgileri doldurun **Tamam**.

![Dinleyici özellikleri dikey penceresi][3]

> [!NOTE]
> Uygulama ağ geçidi için Azure Portalı'nda dinleyicileri oluşturulmasını uzun çalışan bir görev, bu senaryoda iki dinleyicileri oluşturmak için biraz zaman alabilir. Aşağıdaki resimde görüldüğü gibi portal dinleyicileri Göster tamamlandığında:

![Dinleyici genel bakış][4]

## <a name="create-rules-to-map-listeners-to-backend-pools"></a>Arka uç havuzları dinleyicileri eşlemek için kurallar oluşturun

### <a name="step-1"></a>1. Adım

Var olan bir uygulama ağ geçidi (https://portal.azure.com) Azure portalında gidin. Seçin **kuralları** ve mevcut varsayılan kuralı seçme **kuralı 1** tıklatıp **Düzenle**.

### <a name="step-2"></a>2. Adım

Kuralları dikey penceresinde, aşağıdaki resimde görüldüğü gibi doldurun. İlk dinleyici ve ilk havuzu seçme ve tıklayarak **kaydetmek** tamamlandığında.

![Mevcut kuralı düzenleyin][6]

### <a name="step-3"></a>3. Adım

Tıklatın **temel kural** ikinci kuralı oluşturmak için. İkinci dinleyici ve ikinci arka uç havuzu ile formu doldurun ve tıklayın **Tamam** kaydetmek için.

![temel kural dikey ekleme][10]

Bu senaryo, Azure portalı üzerinden çok siteli desteğiyle mevcut bir uygulama ağ geçidi yapılandırma tamamlar.

## <a name="next-steps"></a>Sonraki adımlar

İle Web siteleri korumayı öğrenin [uygulama ağ geçidi - Web uygulaması güvenlik duvarı](application-gateway-webapplicationfirewall-overview.md)

<!--Image references-->
[1]: ./media/application-gateway-create-multisite-portal/figure1.png
[2]: ./media/application-gateway-create-multisite-portal/figure2.png
[3]: ./media/application-gateway-create-multisite-portal/figure3.png
[4]: ./media/application-gateway-create-multisite-portal/figure4.png
[5]: ./media/application-gateway-create-multisite-portal/figure5.png
[6]: ./media/application-gateway-create-multisite-portal/figure6.png
[7]: ./media/application-gateway-create-multisite-portal/figure7.png
[8]: ./media/application-gateway-create-multisite-portal/figure8.png
[9]: ./media/application-gateway-create-multisite-portal/figure9.png
[10]: ./media/application-gateway-create-multisite-portal/figure10.png
[multisite]: ./media/application-gateway-create-multisite-portal/multisite.png
