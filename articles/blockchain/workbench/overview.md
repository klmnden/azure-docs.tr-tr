---
title: Azure Blockchain Workbench’e genel bakış
description: Azure Blockchain Workbench’e ve özelliklerine genel bakış.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 01/14/2019
ms.topic: overview
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 58fd09726f05ba442c66387ecbd6cfad37f598e1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60896167"
---
# <a name="what-is-azure-blockchain-workbench"></a>Azure Blockchain Workbench nedir?

Azure Blockchain Workbench, iş süreçlerini ve verilerini diğer kuruluşlarla paylaşmak için blok zinciri uygulamaları oluşturmanıza ve dağıtmanıza yardımcı olacak şekilde tasarlanmış Azure hizmetleri ve özelliklerinden oluşan bir koleksiyondur. Azure Blockchain Workbench, blok zinciri uygulamaları derlemeye ilişkin altyapı iskeleti sağlayarak geliştiricilerin iş mantığı ve akıllı sözleşmeler oluşturmaya odaklanmasına imkan tanır. Ayrıca genel geliştirme görevlerinin otomatikleştirilmesine yardımcı olacak birçok Azure hizmetini ve özelliğini tümleştirerek blok zinciri uygulamaları oluşturulmasını da kolaylaştırır.

## <a name="create-blockchain-applications"></a>Blok zinciri uygulamaları oluşturma

Blockchain Workbench ile yapılandırmayı kullanarak ve akıllı sözleşme kodu yazarak blok zinciri uygulamaları tanımlayabilirsiniz. Blok zinciri uygulama geliştirme aşamasına atlayabilir ve iskele derleme ve destekleyici hizmetler ayarlama yerine sözleşmenizi tanımlamaya ve iş mantığı yazmaya odaklanabilirsiniz.

## <a name="manage-applications-and-users"></a>Uygulamaları ve kullanıcıları yönetme

Azure Blockchain Workbench, blok zinciri uygulamalarını ve kullanıcılarını yönetmek için bir web uygulaması ve REST API’leri sağlar. Blockchain Workbench yöneticileri, uygulama erişimini yönetebilir ve kullanıcılarınızı uygulama rollerine atayabilir. Azure AD kullanıcıları otomatik olarak uygulamadaki üyelere eşlenir.

## <a name="integrate-blockchain-with-applications"></a>Blok zincirini uygulamalarla tümleştirme

Mevcut sistemlerle tümleştirmek için Blockchain Workbench REST API’lerini ve ileti tabanlı API’leri kullanabilirsiniz. API’ler, birden fazla dağıtılmış defter teknolojisinin, depolama ve veritabanı teklifinin değiştirilmesine veya kullanılmasına imkan tanıyacak bir arabirim sağlar.

Blockchain Workbench, ileti tabanlı API’sine gönderilen iletileri, blok zincirinin yerel API’sinin beklediği bir biçimde derleme işlemlerine dönüştürebilir.  Workbench, işlemleri imzalayıp uygun blok zincirine yönlendirebilir. 

Workbench, aşağı akış tüketicilerine ileti göndermek için Service Bus ve Event Grid’e otomatik olarak olaylar sunar. Geliştiriciler, işlemleri desteklemek ve sonuçlara bakmak için bu ileti sistemlerinin herhangi biriyle tümleştirme gerçekleştirebilir.

## <a name="deploy-a-blockchain-network"></a>Blok zinciri ağını dağıtma

Azure Blockchain Workbench, Azure Resource Manager çözüm şablonuyla önceden yapılandırılmış bir çözüm olarak konsorsiyum blok zinciri ağının kurulumunu kolaylaştırır. Şablon, bir konsorsiyumu çalıştırmak için gereken tüm bileşenleri dağıtan, basitleştirilmiş dağıtım sağlar. Şu anda Blockchain Workbench, Ethereum’u desteklemektedir.

## <a name="use-active-directory-login"></a>Active Directory oturum açma adını kullanma

Mevcut blok zinciri protokolleri ile blok zinciri kimlikleri, ağ üzerindeki bir adres olarak temsil edilir. Azure Blockchain Workbench, blok zinciri kimliğini bir Active Directory kimliğiyle ilişkilendirerek soyutlar ve böylece Active Directory kimlikleriyle kurumsal uygulamalar derlenmesini kolaylaştırır.

## <a name="synchronize-on-chain-data-with-off-chain-storage"></a>Zincir üzerindeki verileri, zincir dışı depolama ile eşitleme

Azure Blockchain Workbench, blok zinciri üzerindeki verileri otomatik olarak zincir dışı depolama ile eşitleyerek blok zinciri olaylarının ve verilerinin analiz edilmesini kolaylaştırır. Doğrudan blok zincirinden verileri ayıklamak yerine, SQL Server gibi zincir dışı veritabanı sistemlerini sorgulayabilirsiniz. Veri analizi görevlerini gerçekleştiren son kullanıcıların blok zincirine özgü deneyime sahip olması gerekmez. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench mimarisi](architecture.md)
