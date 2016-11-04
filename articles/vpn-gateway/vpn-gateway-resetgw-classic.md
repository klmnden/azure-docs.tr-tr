---
title: Azure VPN Gateway’i Sıfırlama | Microsoft Docs
description: Bu makalede Azure VPN Gateway’i sıfırlama adımları anlatılmaktadır. Makale klasik dağıtım modeli kullanılarak oluşturulmuş VPN ağ geçitleri için geçerlidir.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: carmonm
editor: ''
tags: azure-service-management

ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: cherylmc

---
# PowerShell kullanarak bir Azure VPN Gateway sıfırlama
Bu makalede, PowerShell cmdlet'lerini kullanarak Azure VPN Gateway'inizi nasıl sıfırlayacağınız adım adım açıklanmaktadır. Bu yönergeler klasik dağıtım modeli için geçerlidir. Şu anda, Resource Manager dağıtım modelinde oluşturulan sanal ağ geçitleri sıfırlanamaz.

Bir veya daha fazla S2S VPN tünelinde şirketler arası VPN bağlantısını kaybederseniz Azure VPN ağ geçidinin sıfırlanması yararlıdır. Bu durumda şirket içi VPN cihazlarınızın tümü düzgün çalışır, ancak Azure VPN ağ geçitleriyle IPsec tünelleri kuramaz. *Reset-AzureVNetGateway* cmdlet'ini kullandığınızda bu cmdlet, ağ geçidini yeniden başlatır ve ardından şirket içi ve dışı yapılandırmaları ağ geçidine tekrar uygular. Ağ geçidi, sahip olduğu genel IP adresini tutar. Bu durum VPN yönlendirici yapılandırmasını Azure VPN ağ geçidi için yeni bir ortak IP adresi ile güncelleştirmenizin gerekli olmadığı anlamına gelir.  

Ağ geçidinizi sıfırlamadan önce her bir IPsec Siteden Siteye (S2S) VPN tüneli için aşağıda listelenen anahtar öğeleri doğrulayın. Öğelerdeki uyuşmazlık S2S VPN tünellerinin bağlantısının kesilmesiyle sonuçlanır. Şirket içi ve Azure VPN ağ geçitleriniz için yapılandırmaların doğrulanması ve düzeltilmesi sizi ağ geçitleri üzerinde çalışan diğer bağlantılar için gereksiz yeniden başlatmalardan ve kesintilerden korur.

Ağ geçidinizi sıfırlamadan önce aşağıdaki öğeleri doğrulayın.

* Hem Azure VPN ağ geçidi hem de şirket içi VPN ağ geçidi için İnternet IP adresleri (VIP) hem Azure hem de şirket içi VPN ilkelerinde doğru şekilde yapılandırılmıştır.
* Önceden paylaşılan anahtar Azure ve şirket içi VPN ağ geçitlerinde aynı olmalıdır.
* Şifreleme, karma algoritmaları ve PFS (Kusursuz İletme Gizliliği) gibi belirli IPsec/IKE yapılandırmalarını uygularsanız hem Azure hem de şirket içi VPN ağ geçitlerinin aynı yapılandırmalara sahip olduğundan emin olun.

## PowerShell kullanarak VPN Gateway sıfırlama
Azure VPN ağ geçidini sıfırlamaya yönelik PowerShell cmdlet’i *Reset-AzureVNetGateway* ’dir. Her bir Azure VPN ağ geçidi etkin bekleme yapılandırmasında çalışan iki VM örneğinden oluşur. Komut yayınlandıktan sonra Azure VPN ağ geçidinin o anda etkin olan örneği hemen yeniden başlatılır. Etkin örnekten (yeniden başlatılan) bekleme örneğine yük devretme sırasında kısa bir boşluk oluşur. Boşluk bir dakikadan az olmalıdır. 

Aşağıdaki örnekte, "ContosoVNet" adlı sanal ağ için Azure VPN ağ geçidi sıfırlanmaktadır.

        Reset-AzureVNetGateway –VnetName “ContosoVNet” 

        Error          :
        HttpStatusCode : OK
        Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
        Status         : Successful
        RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
        StatusCode     : OK


İlk yeniden başlatma sonrasında bağlantı geri kazanılmazsa ikinci sanal makine örneğini (yeni etkin ağ geçidi) yeniden başlatmak için aynı komutu yeniden verin. Arka arkaya iki yeniden başlatma istenirse her iki sanal makine örneğinin (etkin ve bekleme) yeniden başlatılma süresi biraz daha uzun olur. Bu, sanal makinelerin yeniden başlatma işlemlerini tamamlaması için VPN bağlantısı üzerinde 2 ila 4 dakikaya varan daha uzun bir boşluğa neden olur.

İki yeniden başlatma sonrasında hala şirketler arası bağlantı sorunları yaşıyorsanız lütfen Klasik Azure Portalı’ndan bir destek talebi açarak Microsoft Azure Desteği ile iletişim kurun.

## Sonraki adımlar
 Bu cmdlet hakkında daha fazla bilgi için bkz. [PowerShell başvurusu](https://msdn.microsoft.com/library/azure/mt270366.aspx).

<!--HONumber=Aug16_HO4-->


