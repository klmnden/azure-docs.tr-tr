---
title: "Birden çok VPN ağ geçidi siteden siteye bağlantıları bir Vnet'e eklenecek: Azure Portal: Resource Manager | Microsoft Docs"
description: "Çok siteli S2S bağlantılarının varolan bir bağlantıyı içeren bir VPN ağ geçidi eklemek"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f3e8b165-f20a-42ab-afbb-bf60974bb4b1
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/20/2017
ms.author: cherylmc
ms.openlocfilehash: 7ec57789ee76f4ec54e4f7b68ea75c19522f3d7c
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Mevcut bir VPN ağ geçidi bağlantısı olan bir sanal ağa bir siteden siteye bağlantı Ekle

> [!div class="op_single_selector"]
> * [Azure portalı](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [PowerShell (klasik)](vpn-gateway-multi-site.md)
>
> 

Bu makalede, siteden siteye (S2S) bağlantıları varolan bir bağlantıyı sahip bir VPN ağ geçidi eklemek için Azure portal'ı kullanma aracılığıyla açıklanmaktadır. Bu bağlantı türü genellikle "çok siteli" yapılandırma olarak adlandırılır. S2S bağlantısı, noktadan siteye bağlantı veya VNet-VNet bağlantı zaten bir sanal ağa S2S bağlantısı ekleyebilirsiniz. Bağlantıları eklerken, bazı sınırlamalar vardır. Denetleme [başlamadan önce](#before) yapılandırmanıza başlamadan önce doğrulamak için bu makaledeki bölümü. 

Bu makale RouteBased VPN ağ geçidine sahip Resource Manager dağıtım modeli kullanılarak oluşturulan sanal ağlar için geçerlidir. Bu adımları ExpressRoute /-siteye eşzamanlı bağlantı yapılandırmaları için geçerli değildir. Bkz: [ExpressRoute/S2S arada var olabilen bağlantılar](../expressroute/expressroute-howto-coexist-resource-manager.md) arada var olabilen bağlantılar hakkında bilgi için.

### <a name="deployment-models-and-methods"></a>Dağıtım modelleri ve yöntemleri
[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Bu tabloda yeni makaleler olarak güncelleştiriyoruz ve ek araçlar bu yapılandırma için kullanılabilir hale gelir. Bir makale kullanılabilir olduğunda sizi doğrudan bu tablodan bağlayın.

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <a name="before"></a>Başlamadan önce
Aşağıdaki öğeleri doğrulayın:

* Bir ExpressRoute/S2S eşzamanlı bağlantı oluşturma değil.
* Varolan bir bağlantı Resource Manager dağıtım modeli kullanılarak oluşturulmuş bir sanal ağ var.
* RouteBased Vnet'inizi için sanal ağ geçididir. PolicyBased VPN ağ geçidi varsa, sanal ağ geçidini silin ve yeni bir VPN ağ geçidi RouteBased olarak oluşturun.
* Bu VNet bağlandığı sanal ağlar hiçbirini için adres aralıklarını hiçbiri çakışıyor.
* Uyumlu bir VPN cihazı ve onu yapılandırmanız mümkün olan birisi vardır. Bkz. [VPN Cihazları Hakkında](vpn-gateway-about-vpn-devices.md). VPN cihazınızı yapılandırma konusuyla veya şirket içi ağ yapılandırmanızdaki IP adresi aralıklarıyla ilgili fazla bilginiz yoksa size bu ayrıntıları sağlayabilecek biriyle çalışmanız gerekir.
* VPN cihazınız için dışarıya dönük bir genel IP adresine sahip. Bu IP adresi bir NAT’nin arkasında olamaz.

## <a name="part1"></a>Bölüm 1 - bir bağlantı yapılandırma
1. Tarayıcıdan [Azure portalına](http://portal.azure.com) gidin ve gerekiyorsa Azure hesabınızda oturum açın.
2. Tıklatın **tüm kaynakları** ve bulun, **sanal ağ geçidi** kaynakları listesinden ve tıklatın.
3. Üzerinde **sanal ağ geçidi** dikey penceresinde tıklatın **bağlantıları**.
   
    ![Bağlantılar dikey penceresi](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Connections blade")<br>
4. Üzerinde **bağlantıları** dikey penceresinde tıklatın **+ Ekle**.
   
    ![Add bağlantı düğmesini](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "Ekle bağlantı düğmesi")<br>
5. Üzerinde **Bağlantı Ekle** dikey penceresinde, aşağıdaki alanlar doldururken:
   
   * **Ad:** bağlantısı oluşturmakta siteye vermek istediğiniz adı.
   * **Bağlantı türü:** seçin **siteden siteye (IPSec)**.
     
     ![Ekle bağlantı dikey](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "Ekle bağlantı dikey")<br>

## <a name="part2"></a>Bölüm 2 - bir yerel ağ geçidi ekleme
1. Tıklatın **yerel ağ geçidi** ***bir yerel ağ geçidi seçin***. Bu açılır **Seç yerel ağ geçidi** dikey.
   
    ![Seç yerel ağ geçidi](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "Seç yerel ağ geçidi")<br>
2. Tıklatın **Yeni Oluştur** açmak için **oluşturma yerel ağ geçidi** dikey.
   
    ![Create yerel ağ geçidi dikey](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "oluşturma yerel ağ geçidi")<br>
3. Üzerinde **oluşturma yerel ağ geçidi** dikey penceresinde, aşağıdaki alanlar doldururken:
   
   * **Ad:** yerel ağ geçidi kaynağına vermek istediğiniz adı.
   * **IP adresi:** bağlanmak istediğiniz siteye VPN cihazının genel IP adresi.
   * **Adres alanı:** yeni yerel ağ sitesine yönlendirilmesini istediğiniz adres alanı.
4. Tıklatın **Tamam** üzerinde **oluşturma yerel ağ geçidi** değişiklikleri kaydetmek için dikey.

## <a name="part3"></a>Bölüm 3 - paylaşılan anahtar ekleme ve bağlantı oluşturma
1. Üzerinde **Bağlantı Ekle** dikey penceresinde, bağlantı oluşturmak için kullanmak istediğiniz paylaşılan anahtar ekleyin. VPN cihazınızın paylaşılan anahtarı alma veya bir burada olun ve aynı paylaşılan anahtarı kullanmak üzere VPN Cihazınızı yapılandırın. Önemli şey anahtarları tam olarak aynı değildir.
   
    ![Paylaşılan anahtar](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Shared key")<br>
2. Dikey pencerenin alt kısmındaki tıklatın **Tamam** bağlantı oluşturmak için.

## <a name="part4"></a>4 bölüm - VPN bağlantısını doğrulama


[!INCLUDE [vpn-gateway-verify-connection-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. sanal makineleri [öğrenme yolu](https://azure.microsoft.com/documentation/learning-paths/virtual-machines).
