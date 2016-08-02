<properties
   pageTitle="ExpressRoute benimseme için ön koşullar | Microsoft Azure"
   description="Bu sayfada bir Azure ExpressRoute bağlantı hattı sipariş etmeden önce karşılanması gereken gereksinimlerin listesi bulunmaktadır."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/19/2016"
   ms.author="cherylmc"/>


# ExpressRoute önkoşulları ve denetim listesi  

ExpressRoute kullanarak microsoft bulut hizmetlerine bağlanmak için, aşağıdaki bölümlerde listelenen gereksinimlerin karşılanmış olduğunu doğrulamanız gerekir.

## Azure hesabı

- Geçerli ve etkin bir Microsoft Azure hesabı. ExpressRoute bağlantı hattı kurmak için bu gereklidir. ExpressRoute bağlantı hatları Azure aboneliklerinin içindeki kaynaklardır. Bağlantı,Office 365 hizmetleri ve CRM Online gibi Azure olmayan Microsoft bulut hizmetleriyle sınırlı olsa bile bir Azure aboneliği şarttır.
- Etkin bir Office 365 aboneliği (Office 365 hizmetlerini kullanıyorsanız) Daha fazla bilgi için bu makalenin [Office 365 için belli gereksinimler](#office-365-specific-requirements) bölümünü okuyun.

## Bağlantı sağlayıcısı
- Microsoft bulut’a bağlanmak için [ExpressRoute bağlantı ortağı](expressroute-locations.md#partners) ile çalışabilirsiniz. Şirket içi ağınız ve Microsoft arasında bağlantıyı [üç şekilde](expressroute-introduction.md#howtoconnect) kurabilirsiniz. 
- Sağlayıcınız ExpressRoute bağlantı ortağı değilse bile [bulut değişim sağlayıcısı](expressroute-locations.md#nonpartners) yoluyla Microsoft bulut’a bağlanabilirsiniz.

## Ağ gereksinimleri
- **Yedekli bağlantı**: Siz ve sağlayıcınız arasında fiziksel bağlantıda yedekleme gereksinimi yoktur. Microsoft, [bir bulut değişimine yalnızca bir fiziksel bağlantınız](expressroute-faqs.md#onep2plink) olsa bile, yedekli BGP oturumlarının Microsoft yönlendiricileri ve eşleme yönlendiricileri arasında kurulu olmasını gerektirir. 
- **Yönlendirme**: Microsoft Bulut’a nasıl bağlandığınıza bağlı olarak, [yönlendirme etki alanları](expressroute-circuit-peerings.md) için BGP oturumlarının siz veya sağlayıcınız tarafından kurulup yönetilmesi gerekir. Bazı Ethernet bağlantı sunucuları veya bulut değişim sunucuları değer ekleme hizmeti olarak BGP yönetimi sunabilir.
- **NAT**: Microsoft genel IP adreslerini sadece Microsoft eşleme yoluyla kabul eder. Şirket içi ağınızda özel IP adresleri kullanıyorsanız, özel IP adreslerinin [NAT kullanarak](expressroute-nat.md) genel IP adreslerine siz veya sağlayıcınız tarafından çevrilmesi gerekir.
- **QoS**: Skype Kurumsal’da farklı QoS davranışları gerektiren çeşitli hizmetler (ör. ses, video, mesaj) vardır. [Qos gereksinimleri](expressroute-qos.md)ne siz ve sağlayıcınız tarafından uyulmalıdır.
- **Ağ Güvenliği**: ExpressRoute ile Microsoft Bulut’a bağlanırken [ağ güvenliği](../best-practices-network-security.md)ne dikkat edilmelidir.
 
## Office 365

ExpressRoute’ta Office 365’i etkinleştirmeyi planlıyorsanız, lütfen Office 365 gereksinimleri hakkında daha fazla bilgi içeren aşağıdaki belgelere göz atın.


- [Office 365 için ExpressRoute’a Genel Bakış](https://support.office.com/en-us/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)
- [Office 365 için ExpressRoute ile Yönlendirme](https://support.office.com/en-us/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408)
- [Office 365 URL’leri ve IP adres aralıkları](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
- [Office 365 için ağ planlama ve performans ayarlama](https://support.office.com/en-us/article/Network-planning-and-performance-tuning-for-Office-365-e5f1228c-da3c-4654-bf16-d163daee8848)
- [Ağ bant genişliği hesaplayıcıları ve araçları](https://support.office.com/en-us/article/Network-and-migration-planning-for-Office-365-f5ee6c33-bcd7-4b0b-b0f8-dc1d9fb8d132)
- [Office 365’i şirket içi ortamlarla tümleştirme](https://support.office.com/en-us/article/Office-365-integration-with-on-premises-environments-263faf8d-aa21-428b-aed3-2021837a4b65)

## CRM Online 
ExpressRoute ile CRM Online’ı etkinleştirmeyi planlıyorsanız, lütfen CRM Online hakkında daha fazla bilgi içeren aşağıdaki belgelere göz atın.

- [CRM Online URL’leri](https://support.microsoft.com/kb/2655102) ve [IP adres aralıkları](https://support.microsoft.com/kb/2728473)

## Sonraki adımlar

- ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
- ExpressRoute bağlantı sağlayıcısı bulun. Bkz. [ExpressRoute ortakları ve eşleme konumları](expressroute-locations.md).
- [Yönlendirme](expressroute-routing.md), [NAT](expressroute-nat.md) ve [QoS](expressroute-qos.md) için gereksinimlere bakın.
- ExpressRoute bağlantınızı yapılandırın.
    - [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-classic.md)
    - [Yönlendirmeyi yapılandırma](expressroute-howto-routing-classic.md)
    - [ExpressRoute bağlantı hattına bir VNet bağlama](expressroute-howto-linkvnet-classic.md)




<!---HONumber=Jun16_HO2-->


