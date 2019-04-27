---
title: Azure'da bir SMTP Başlık denetimi için geriye doğru arama bölgesini yapılandırma
titlesuffix: Azure Virtual Network
description: Azure'da bir SMTP Başlık denetimi için geriye doğru arama bölgeleri yapılandırılması açıklanmaktadır
services: virtual-network
documentationcenter: virtual-network
author: genlin
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 203c3c5f371af7de891f0949a35378294bb50a0e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60713648"
---
# <a name="configure-reverse-lookup-zones-for-an-smtp-banner-check"></a>Bir SMTP Başlık denetimi için geriye doğru arama bölgesini yapılandırma

Bu makalede, Azure DNS'de bir geriye doğru arama bölgesi kullanın ve SMTP başlık denetlemek için bir geriye doğru DNS (PTR) kaydı oluşturmak açıklar.

## <a name="symptom"></a>Belirti

Microsoft azure'da bir SMTP sunucusu barındırıyorsanız, aşağıdaki hata iletisini alabilirsiniz ne zaman göndereceğini ya da Uzak posta sunuculardan bir ileti alırsınız:

**554: PTR kaydı**

## <a name="solution"></a>Çözüm

Azure'da sanal bir IP adresi için etki alanı bölgeleri değil özel etki alanı bölgelerine ait Microsoft ters kayıtları oluşturulur.

Bölgelere ait Microsoft PTR kayıtları yapılandırmak için Publicıpaddress kaynak - ReverseFqdn özelliğini kullanın. Daha fazla bilgi için [Azure'da barındırılan hizmetler için yapılandırma ters DNS](../dns/dns-reverse-dns-for-azure-services.md).

PTR kayıtlarını yapılandırırken, IP adresi ve ters FQDN, abonelik tarafından ait emin olun. Aboneliğine ait olmayan bir ters FQDN ayarlamaya çalışırsanız, aşağıdaki hata iletisini alıyorsunuz:

    Set-AzPublicIpAddress : ReverseFqdn mail.contoso.com that PublicIPAddress ip01 is trying to use does not belong to subscription <Subscription ID>. One of the following conditions need to be met to establish ownership:
                        
    1) ReverseFqdn abonelik kapsamında tüm genel IP kaynağı FQDN'si eşleşir;
    2) Abonelik kapsamında tüm genel IP kaynağı FQDN'sini (CName kayıt zinciri) yoluyla ReverseFqdn çözümler;
    3) Abonelik kapsamında bir statik genel IP kaynağı (CName ve A kayıt zinciri) yoluyla IP adresine çözümler.

El ile değiştirirseniz, SMTP başlık varsayılan eşleştirmek için ters FQDN, uzak bir posta sunucusu etki alanı için MX kaydı eşleştirilecek hostitel SMTP başlık bekleyebilir için yine de başarısız olabilir.
