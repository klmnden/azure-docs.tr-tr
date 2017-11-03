---
title: "Azure Active Directory PoC Playbook giriş | Microsoft Docs"
description: "Keşfetmek ve hızlı bir şekilde kimlik ve erişim yönetimi senaryoları uygulayan"
services: active-directory
keywords: "Azure active directory, playbook, kavram kanıtı, PT"
documentationcenter: 
author: dstefanMSFT
manager: asuthar
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: dstefan
ms.openlocfilehash: fb4767bae6a5435f5739162eaf52edec2f0cbf0a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-introduction"></a>Azure Active Directory Playbook kavram kanıtı: Giriş

Bu makalede farklı Azure keşfetmek için yönergeler sağlanmaktadır bir kavram kanıtı (PoC)'de AD özellikleri. Bu belgenin hedef kitle kimlik mimarları, BT uzmanları ve sistem tümleştiricileri olduğu

## <a name="how-to-use-this-playbook"></a>Bu Playbook kullanma

1. Kullanım [tema](active-directory-playbook-ingredients.md#theme) bölümünde ve gereksinimlerinize göre ilgi çizilen seçin.  
2. Kapsam iş hedeflerinize Hizala senaryoları seçerek PoC. Daha kısa iyi olur. Kısa ve bunun farkına karmaşıklığını azaltırken katılımcılara değeri iletmek mümkün olduğunca kısa olarak yapmakta öneririz.  
3. Kullanım [uygulama](active-directory-playbook-implementation.md) senaryolarını ve ne geldiklerini ortamınız için anlamak için bölüm. Her senaryoda, biz kurulacağını açıklar (veririz [yapı taşları](active-directory-playbook-building-blocks.md)) ve nasıl senaryolardan gidin. 
4. Her yapı bloğu tamamlamak için yaklaşık bir saat yanı sıra, gerekli ön koşullar açıklanmaktadır. Bu planlama işlemi sırasında yardımcı olabilir. 
5. 1-3 bağlı olarak, tanımlamak [ortam](active-directory-playbook-ingredients.md#environment) yürütmek üzere. Kullanıcılarınız için iyi bir deneyim yapısını almak bir üretim ortamı için mücadele etmek öneririz. 
6. Çakışan gereksinimlerine sahip olduğunda, bu yararlı kolaylığını matrisi kullanma 
   * Değerinin tema merkezli gösterme  
   * Düzgünlük hazırlamak için Kurulum ve senaryoları yürütmek için 
   * Hedef senaryoları çalıştırmak için en az süre 
   * Olarak üretim kısıtlamaları içindeki uygun olarak yakın 

>[!NOTE]
> Bu makale bazı belirli üçüncü taraf uygulamalar ve kolaylık sağlamak için örnek olarak belirtilen ürünleri görürsünüz. Azure AD destekleyen uygulamalarda binlerce bizim [uygulama Galerisi](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) , gereksinimleri ve ortam bağlı olarak, kullanabilirsiniz. 



[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]