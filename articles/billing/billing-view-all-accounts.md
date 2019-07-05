---
title: Azure portalında faturalama hesaplarınızı görüntülemek | Microsoft Docs
description: Azure portalında faturalama hesaplarınızı görüntülemeyi öğrenin.
services: ''
documentationcenter: ''
author: amberbhargava
manager: amberb
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/11/2018
ms.author: banders
ms.openlocfilehash: 36430e9b0a4554761d53b537d3c32fa57068eabb
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67490211"
---
# <a name="view-billing-accounts-in-azure-portal"></a>Azure portalında faturalama hesapları görüntüleyin  

Azure'u kullanmak kaydolduğunuzda, bir faturalama hesabı oluşturulur. Maliyetleri izleyin ve fatura hesabınıza ödemeler faturalarınızı yönetmek için kullanın. Faturalandırma birden çok hesaba erişim sağlayabilirsiniz. Örneğin, Azure için kişisel projeleriniz için kaydolmanız. Ayrıca, kuruluşunuzun Kurumsal Anlaşma veya Microsoft Müşteri sözleşmesi aracılığıyla erişimi olabilir. Bu senaryoların her biri için ayrı bir fatura hesap gerekir.

Azure portalı, şu anda fatura hesapları aşağıdaki türünü destekler:

- **Microsoft Online Services programı**: Azure Web sitesi üzerinden Azure'a kaydolmak için bir Microsoft Online Services programı bir faturalama hesabı oluşturulur. Örneğin, oturum açtığınızda için bir [Azure ücretsiz hesabı](https://azure.microsoft.com/offers/ms-azr-0044p/), [Kullandıkça Öde tarifesine göre hesabıyla](https://azure.microsoft.com/offers/ms-azr-0003p/) veya farklı bir [Visual studio abonesi](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/).

- **Kurumsal Anlaşma**: Kuruluşunuz oturum açtığında bir kurumsal anlaşma için bir faturalama hesabı oluşturulduğunda bir [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) Azure kullanılacak.

- **Microsoft Müşteri sözleşmesi**: Kuruluşunuzda bir Microsoft Müşteri Sözleşmesi'ni imzalamak için bir Microsoft temsilcisiyle çalışırken, Microsoft Müşteri sözleşmesi için bir faturalama hesabı oluşturulur. Azure Web sitesi için kaydolma bazı müşteriler bölgelerde, bir [Kullandıkça Öde tarifesine göre hesabıyla](https://azure.microsoft.com/offers/ms-azr-0003p/) veya yükseltme kendi [Azure ücretsiz hesabı](https://azure.microsoft.com/offers/ms-azr-0044p/) bir faturalama hesabı için bir Microsoft Customer olabilir De sözleşme. Daha fazla bilgi için [Microsoft Müşteri sözleşmesi için fatura hesabınıza ile çalışmaya başlama](billing-mca-overview.md).

<!--Todo Add section to identify the type of accounts -->

## <a name="scopes-for-billing-accounts"></a>Faturalama hesaplarının kapsamları
Bir kapsam, kullanıcıların görüntüleyip faturalandırmayı yönetmek için bir faturalama hesabı içinde bir düğümdür. Bu kullanıcılar nerede faturalama verileri, ödemeler, faturalar, yönetmek ve genel hesap yönetimi kuralları olur. 

### <a name="microsoft-online-services-program"></a>Microsoft Online Services Programı

|`Scope`  |Tanım  |
|---------|---------|
|Fatura hesabı     | Tek bir sahibi (Hesap Yöneticisi) için bir veya daha fazla Azure aboneliği temsil eder. Bir Hesap Yöneticisi, abonelikleri oluşturabilir, faturaları görüntülemek veya değiştirmek abonelikler için faturalama gibi çeşitli faturalandırma görevlerini gerçekleştirmek için yetkili.  |
|Abonelik     |  Azure kaynaklarınızın bir gruplandırma temsil eder. Fatura, bu kapsamda oluşturulur. Bu, fatura ödeme için kullanılan, kendi ödeme yöntemleri vardır.|


### <a name="enterprise-agreement"></a>Kurumsal Anlaşma

|`Scope`  |Tanım  |
|---------|---------|
|Fatura hesabı    | Kurumsal Sözleşme temsil eder. Fatura, bu kapsamda oluşturulur. Departmanlar ve kayıt hesapları kullanılarak yapılandırılır.  |
|Departman     |  Kayıt hesaplarını isteğe bağlı gruplandırmasıdır.      |
|Kayıt hesabı     |  Tek bir hesap sahibi temsil eder. Azure Abonelikleri, bu kapsamı altında oluşturulur.  |


### <a name="microsoft-customer-agreement"></a>Microsoft Müşteri Sözleşmesi

|`Scope`  |Görevler  |
|---------|---------|
|Fatura hesabı     |   Bir müşteri sözleşmesi birden çok Microsoft Ürün ve hizmetlerini temsil eder. Fatura profilleri ve fatura bölümler kullanılarak yapılandırılır.   |
|Faturalandırma profili     |  Faturayı ve ödeme yöntemlerini temsil eder. Fatura, bu kapsamda oluşturulur. Birden fazla faturanın bölüm sağlayabilirsiniz.      |
|Fatura bölümü     |   Fatura maliyetlerini oluşan bir grubu temsil eder. Abonelikler ve diğer satın alma işlemleri, bu kapsamıyla ilişkilendirilmiş.    |


## <a name="switch-billing-scope-in-the-azure-portal"></a>Azure portalındaki faturalandırma kapsam geçiş


1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Arama **maliyet Yönetimi + faturalandırma**.

   ![Azure portalı arama gösteren ekran görüntüsü](./media/billing-view-all-accounts/billing-search-cost-management-billing.png)

3. Seçin **tüm faturalandırma kapsamlar** sol taraftaki.

   ![Tüm faturalandırma kapsamlar gösteren ekran görüntüsü](./media/billing-view-all-accounts/billing-list-of-accounts.png)

   ** Görmezsiniz **tüm faturalandırma kapsamlar** yalnızca bir kapsam için erişimi varsa.

4. Ayrıntılarını görüntülemek için bir kapsam seçin.



## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar
- Nasıl başlayacağınızı öğrenin [maliyetlerinizi analiz](../cost-management/quick-acm-cost-analysis.md).