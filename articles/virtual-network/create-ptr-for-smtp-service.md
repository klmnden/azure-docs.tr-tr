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
ms.openlocfilehash: 815e3c711850eab11aef63e04a1c512c4510a910
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51684306"
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
    2) Abonelik kapsamında tüm genel IP kaynağı FQDN'sini (CName kayıt zinciri) yoluyla ReverseFqdn çözümler; 
    3) Abonelik kapsamında bir statik genel IP kaynağı (CName ve A kayıt zinciri) yoluyla IP adresine çözümler. 

El ile değiştirirseniz, SMTP başlık varsayılan eşleştirmek için ters FQDN, uzak bir posta sunucusu etki alanı için MX kaydı eşleştirilecek hostitel SMTP başlık bekleyebilir için yine de başarısız olabilir.