---
title: Azure'da geriye doğru arama bölgeleri bir SMTP Başlık denetimi için yapılandırma | Microsoft Docs
description: Geriye doğru arama bölgeleri bir SMTP Başlık denetimi için Azure içinde yapılandırmayı açıklar
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
ms.date: 02/06/2018
ms.author: genli
ms.custom: ''
ms.openlocfilehash: 1e95b00ea08105238a860265e46275c24ed7bfbd
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2018
ms.locfileid: "29151876"
---
#  <a name="configure-reverse-lookup-zones-for-an-smtp-banner-check"></a>Geriye doğru arama bölgeleri bir SMTP Başlık denetimi için yapılandırma

Bu makalede, Azure DNS'de geriye doğru arama bölgesi ve SMTP başlık denetlemek için bir geriye doğru DNS (PTR) kaydı oluşturmak nasıl kullanılacağı açıklanmaktadır. 

## <a name="symptom"></a>Belirti

Microsoft azure'da bir SMTP sunucusu barındırıyorsanız, aşağıdaki hata iletisini alabilirsiniz zaman göndermek ya da Uzak posta sunucularından bir ileti alırsınız:

**554: PTR kaydı** 

## <a name="solution"></a>Çözüm

Azure'da sanal bir IP adresi için etki alanı bölgeler, olmayan özel etki alanı bölgeleri ait Microsoft ters kayıtları oluşturulur.

PTR kayıtları bölgeleri ait Microsoft yapılandırmak için Publicıpaddress kaynakta - ReverseFqdn özelliğini kullanın. Daha fazla bilgi için bkz: [Azure içinde barındırılan hizmetler için yapılandırma ters DNS](../dns/dns-reverse-dns-for-azure-services.md). 

PTR kayıtları yapılandırdığınızda, IP adresi ve geriye doğru FQDN abonelik tarafından ait emin olun. Aboneliğine ait olmayan bir ters FQDN ayarlamaya çalışırsanız, aşağıdaki hata iletisini alıyorsunuz:

    Set-AzureRmPublicIpAddress : ReverseFqdn mail.contoso.com that PublicIPAddress ip01 is trying to use does not belong to subscription <Subscription ID>. One of the following conditions need to be met to establish ownership: 
                        
    1) ReverseFqdn abonelik altındaki herhangi bir genel IP kaynak FQDN'siyle eşleşen; 
    2) Abonelik altındaki herhangi bir genel IP kaynak FQDN'sini (CName kayıt zinciri yoluyla) ReverseFqdn çözümler; 
    3) Bu abonelik altında bir statik genel IP kaynak adresi (CName ve A kayıt zinciri yoluyla) çözümler. 

El ile değiştirmeniz halinde, SMTP başlık varsayılan eşleştirmek için ters FQDN, Uzak posta sunucusu SMTP başlık ana etki alanı için MX kaydı eşleşecek şekilde beklediğiniz çünkü yine başarısız olabilir.