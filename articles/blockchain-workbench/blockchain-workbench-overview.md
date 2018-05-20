---
title: Azure Blockchain Workbench’e genel bakış
description: Azure Blockchain Workbench’e ve özelliklerine genel bakış.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 3/21/2018
ms.topic: overview
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 9cd8ef3977d12364759838b46632ba32e0de6e70
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
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

## <a name="deploy-a-blockchain-network"></a>Bok zinciri ağını dağıtma

Azure Blockchain Workbench, Azure Resource Manager çözüm şablonuyla önceden yapılandırılmış bir çözüm olarak konsorsiyum blok zinciri ağının kurulumunu kolaylaştırır. Şablon, bir konsorsiyumu çalıştırmak için gereken tüm bileşenleri dağıtan, basitleştirilmiş dağıtım sağlar. Şu anda Blockchain Workbench, Ethereum’u desteklemektedir.

## <a name="use-active-directory-login"></a>Active Directory oturum açma adını kullanma

Mevcut blok zinciri protokolleri ile blok zinciri kimlikleri, ağ üzerindeki bir adres olarak temsil edilir. Azure Blockchain Workbench, blok zinciri kimliğini bir Active Directory kimliğiyle ilişkilendirerek soyutlar ve böylece Active Directory kimlikleriyle kurumsal uygulamalar derlenmesini kolaylaştırır.

## <a name="synchronize-on-chain-data-with-off-chain-storage"></a>Zincir üzerindeki verileri, zincir dışı depolama ile eşitleme

Azure Blockchain Workbench, blok zinciri üzerindeki verileri otomatik olarak zincir dışı depolama ile eşitleyerek blok zinciri olaylarının ve verilerinin analiz edilmesini kolaylaştırır. Doğrudan blok zincirinden verileri ayıklamak yerine, SQL Server gibi zincir dışı veritabanı sistemlerini sorgulayabilirsiniz. Veri analizi görevlerini gerçekleştiren son kullanıcıların blok zincirine özgü deneyime sahip olması gerekmez. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench mimarisi](blockchain-workbench-architecture.md)
