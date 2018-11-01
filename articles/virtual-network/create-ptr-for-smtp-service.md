---
title: Azure'da bir SMTP Başlık denetimi için geriye doğru arama bölgeleri yapılandırma | Microsoft Docs
description: Azure'da bir SMTP Başlık denetimi için geriye doğru arama bölgeleri yapılandırılması açıklanmaktadır
services: virtual-network
documentationcenter: virtual-network
author: genlin
manager: WillChen
editor: ''
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 10/31/2018
ms.author: genli
ms.custom: ''
ms.openlocfilehash: 2164dedc27d81aa488c3e054b4c6df067e96e612
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50418203"
---
#  <a name="configure-reverse-lookup-zones-for-an-smtp-banner-check"></a>Bir SMTP Başlık denetimi için geriye doğru arama bölgesini yapılandırma

Bu makalede, Azure DNS'de bir geriye doğru arama bölgesi kullanın ve SMTP başlık denetlemek için bir geriye doğru DNS (PTR) kaydı oluşturmak açıklar. 

## <a name="symptom"></a>Belirti

Microsoft azure'da bir SMTP sunucusu barındırıyorsanız, aşağıdaki hata iletisini alabilirsiniz ne zaman göndereceğini ya da Uzak posta sunuculardan bir ileti alırsınız:

**554: PTR kaydı** 

## <a name="solution"></a>Çözüm

Azure'da sanal bir IP adresi için etki alanı bölgeleri değil özel etki alanı bölgelerine ait Microsoft ters kayıtları oluşturulur.

Bölgelere ait Microsoft PTR kayıtları yapılandırmak için Publicıpaddress kaynak - ReverseFqdn özelliğini kullanın. Daha fazla bilgi için [Azure'da barındırılan hizmetler için yapılandırma ters DNS](../dns/dns-reverse-dns-for-azure-services.md). 

PTR kayıtlarını yapılandırırken, IP adresi ve ters FQDN, abonelik tarafından ait emin olun. Aboneliğine ait olmayan bir ters FQDN ayarlamaya çalışırsanız, aşağıdaki hata iletisini alıyorsunuz:

    Set-AzureRmPublicIpAddress : ReverseFqdn mail.contoso.com that PublicIPAddress ip01 is trying to use does not belong to subscription <Subscription ID>. One of the following conditions need to be met to establish ownership: 
                        
    1) ReverseFqdn abonelik kapsamında tüm genel IP kaynağı FQDN'si eşleşir; 
    2) Abonelik altındaki herhangi bir genel IP kaynağa FQDN'sini (CName kayıt zinciri) yoluyla ReverseFqdn çözümler; 
    3) Abonelik kapsamında bir statik genel IP kaynağı (CName ve A kayıt zinciri) yoluyla IP adresine çözümler. 

El ile değiştirirseniz, SMTP başlık varsayılan eşleştirmek için ters FQDN, uzak bir posta sunucusu etki alanı için MX kaydı eşleştirilecek hostitel SMTP başlık bekleyebilir için yine de başarısız olabilir.