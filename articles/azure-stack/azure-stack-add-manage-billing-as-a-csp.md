---
title: Kullanım ve fatura Azure yığını için bir bulut hizmeti sağlayıcısı olarak yönetme | Microsoft Docs
description: Bir bulut sağlayıcısı olarak Azure yığın kaydetme ve müşteriler ekleme aracılığıyla geçiliyor.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2018
ms.author: mabrigg
ms.reviewer: alfredo
ms.openlocfilehash: 21a52af4943004789b0a9bdbe4695ab1a603c046
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34796708"
---
# <a name="manage-usage-and-billing-for-azure-stack-as-a-cloud-service-provider"></a>Kullanım ve fatura Azure yığını için bir bulut hizmeti sağlayıcısı olarak yönetme 

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Bu makalede, bir bulut sağlayıcısı (CSP) olarak Azure yığın kaydetme ve müşteriler ekleme anlatılmaktadır.

Bir CSP Azure yığın kullanarak birçok farklı müşterinin etkileyebilir. Her müşterinin Azure'da bir CSP abonelik ve her kullanıcının abonelik, Azure yığından kullanımı yönlendirmek gerekir.

Aşağıdaki diyagram, paylaşılan hizmetler hesabınızı seçin ve azure hesabı hesabınıza kaydetmeniz gerekir adımları gösterir. Bu işiniz bittiğinde, son müşterilerinizin gerçekleştirebilir.

**Kullanımı bir CSP izleme ekleme adımları**

![Kullanım ve bir bulut hizmeti sağlayıcısı Yönetimi etkinleştirme işlemi.](media\azure-stack-add-manage-billing-as-a-csp\process-add-useage-as-a-csp.png)

## <a name="create-a-csp-or-cspss-subscription"></a>CSP veya CSPSS Abonelik Oluştur

### <a name="cloud-service-provider-subscription-types"></a>Bulut hizmeti sağlayıcısı abonelik türleri

Azure yığını için kullandığınız paylaşılan hizmet hesabı türünü seçin gerekecektir. Çok kullanıcılı Azure yığın kaydı için kullanılabilir abonelikleri türleri şunlardır:

 - Bulut hizmeti sağlayıcısı 
 - İş ortağı paylaşılan hizmetler abonelik 

#### <a name="csp-shared-services"></a>CSP paylaşılan hizmetler

Bulut hizmeti sağlayıcısı paylaşılan hizmetler (CSPSS) abonelikleri doğrudan bir CSP zaman kaydı için tercih edilen seçenek ya da CSP dağıtıcı Azure yığın çalışır.

CSPSS abonelik paylaşılan hizmetler Kiracı ile ilişkilendirilmiş. Azure yığın kaydolduğunuzda, aboneliğin sahibi olan bir hesabın kimlik bilgilerini sağlayın gerekir. Azure yığın kaydetmek için kullandığınız hesabın, dağıtım için kullandığınız yönetici hesabının farklı olabilir; iki yapmak *değil* aynı etki alanına ait olmaları gerekir. Diğer bir deyişle, zaten kullanıyorsanız kiracıyı kullanarak dağıtabilirsiniz. Örneğin, ContosoCSP.onmicrosoft.com kullanın, sonra farklı bir kiracı, örneğin IURContosoCSP.onmicrosoft.com kullanarak kaydedin. Yapmak için gün Azure yığın yönetim yaptığınızda ContosoCSP.onmicrosoft.com kullanarak oturum unutmayın gerekecektir. Kayıt işlemleri yapmanız gerektiğinde IURContosoCSP.onmicrosoft.com kullanarak Azure'da ne zaman oturum açın.

Abonelik oluşturma konusunda açıklamasını CSPSS abonelikleri ve yönergeler için aşağıdaki başvurmak [Azure ortak paylaşılan hizmetler](https://msdn.microsoft.com/partner-center/shared-services).

#### <a name="csp-subscriptions"></a>CSP abonelikleri

Bulut hizmeti sağlayıcısı (CSP) abonelikleri bir CSP satıcı zaman kaydı için tercih edilen seçimdir veya bir son müşteriye Azure yığın çalışır.

## <a name="register-azure-stack"></a>Azure yığın kaydetme

Azure yığın ile kaydetmek için bkz: [kaydetmek Azure yığın Azure aboneliğinizle](azure-stack-registration.md).

## <a name="add-end-customer"></a>Son müşteri ekleme

Yeni bir kiracı kaynaklarına kullandığında kullanımları bulut hizmeti sağlayıcısı (CSP) aboneliğini raporlanır böylece Azure yığın yapılandırmak için bkz [Kiracı kullanımı için ekleyin ve Azure yığınına faturalama](azure-stack-csp-howto-register-tenants.md).

## <a name="charge-the-right-subscriptions"></a>Sağ abonelikleri gider

Azure yığın kayıt adında bir özellik kullanır. Bir kayıt için belirli bir Azure yığınına kaydedilecek kullanmak için hangi Azure aboneliklerinden belgeler Azure içinde depolanan bir nesne değil. Bu bölümde kayıt önemini giderir.

Kayıt kullanarak Azure yığın yapabilirsiniz:
 - Azure ticaret Azure yığın kullanım verilerini iletmek ve bir Azure aboneliği fatura.
 - Çok kullanıcılı bir Azure yığın dağıtım ile farklı bir abonelik her müşterinin kullanım raporlama. Azure farklı kuruluşlarda aynı Azure yığın örneğinde desteklemek için yığın çoklu müşteri mimarisi sağlar.

Her Azure yığın için bir varsayılan abonelik yok ve çoğu abonelikleri gerektiğinde Kiracı olarak. Varsayılan abonelik Kiracı özgü abonelik ise doludur bir Azure aboneliğinizin olduğundan. Kaydedilecek ilk olmalıdır. Çalışmak için raporlama çok Kiracı kullanımı için abonelik bir CSP veya CSPSS abonelik olmalıdır.

Ardından, kayıt Azure yığın kullanacak her bir kiracı için bir Azure aboneliği ile güncelleştirilir. Kiracı aboneliklerine CSP türünde olması gerekir ve varsayılan abonelik sahibi iş ortağı biriken gerekir. Diğer bir deyişle, başka birinin müşteriler kaydedilemiyor.

Azure yığın kullanım bilgileri için genel Azure gönderdiğinde, bir hizmet olarak kayıt bakar ve her bir kiracının kullanım için uygun Kiracı aboneliği eşler. Bir kiracı kayıtlı değil, bu kullanım, kaynaklandığı Azure yığın örneği için varsayılan abonelik gider.

Kiracı aboneliklerine CSP abonelikler olduğundan, kendi fatura CSP ortağına gönderilir ve kullanım bilgilerini son müşteriye görünür değil.



## <a name="next-steps"></a>Sonraki adımlar

 - CSP program hakkında daha fazla bilgi için bkz: [bulut çözümü sağlayıcısı programı](https://partnercenter.microsoft.com/en-us/partner/programs).
 - Azure yığınından kaynak kullanım bilgilerini alma hakkında daha fazla bilgi için bkz: [kullanım ve fatura Azure yığınında](azure-stack-billing-and-chargeback.md).
