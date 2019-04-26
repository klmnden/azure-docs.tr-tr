---
title: Azure ayrıntılı kullanımınızı anlayın | Microsoft Docs
description: Okuma ve Azure aboneliğiniz için ayrıntılı kullanım CSV bölümlerini anlama hakkında bilgi edinin
services: ''
documentationcenter: ''
author: bandersmsft
manager: alherz
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: banders
ms.openlocfilehash: a143fc6d9dbd78ae365f943a00ac9f8492d5e51c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60369633"
---
# <a name="understand-terms-on-your-microsoft-azure-detailed-usage-charges"></a>Microsoft Azure ayrıntılı Kullanım ücretlerinizi ilgili koşulları anlama 

Ayrıntılı kullanım ücretlerini CSV dosyası, geçerli fatura dönemi için günlük ve ölçüm düzey kullanım ücretlerini içerir. 

Ayrıntılı kullanım dosyanız almak için bkz. [Azure faturanızı ve günlük kullanım verilerinizi edinme](billing-download-azure-invoice-daily-usage-date.md).
Elektronik tablo uygulamasında açabileceğiniz bir virgülle ayrılmış değerler (.csv) dosya biçiminde kullanılabilir. İki sürüm varsa 2. sürümü indirin. En son dosya biçimidir.

Kullanım ücretleri, toplam **aylık** ücretleri bir Abonelikteki. Kullanım ücretleri, herhangi bir kredi veya indirim dikkate almaz.

>[!VIDEO https://www.youtube.com/embed/p13S350M2Vk]

## <a name="detailed-terms-and-descriptions-of-your-detailed-usage-file"></a>Ayrıntılı hüküm ve açıklamaları, ayrıntılı kullanım dosyası

Aşağıdaki bölümlerde ayrıntılı kullanım dosyası 2. sürümünü gösterilen önemli koşullar açıklanmaktadır.

### <a name="statement"></a>Bildirim

Ayrıntılı kullanım CSV dosyasının üst kısmında, aya ait fatura dönemi boyunca kullanılan hizmetler gösterilmektedir. Aşağıdaki tabloda, koşulları ve bu bölümde gösterilen açıklamalarını listeler.

| Sözleşme Dönemi | Açıklama |
| --- | --- |
|Fatura Dönemi |Ölçümleri kullanıldığında fatura dönemi |
|Ölçüm Kategorisi |Kullanımı için üst düzey hizmeti belirtir |
|Ölçüm Alt Kategorisi |Hızı etkileyen Azure hizmet türünü tanımlar |
|Ölçüm Adı |Tüketilen ölçüm için ölçü birimini belirtir |
|Ölçüm Bölgesi |Veri Merkezi konumuna göre ücretlendirilen belirli hizmetler için veri merkezinin konumunu belirtir |
|SKU |Her Azure ölçüm için benzersiz sistem tanımlayıcıyı belirtir |
|Birim |Hizmetin ücretlendirildiği Birimi tanımlar. Örneğin GB, saat, 10.000 s. |
|Kullanılan Miktar |Fatura dönemi boyunca kullanılan ölçüm miktarı |
|Dahil Edilen Miktar |Geçerli fatura döneminize ücretsiz dahil olan ölçüm miktarı |
|Kapasite Aşım Miktarı |Tüketilen Miktar dahil edilen miktar arasındaki fark gösterir. Bu miktar için faturalandırılırsınız. Kullandıkça Öde teklifleri için dahil edilen miktar ile teklif, bu toplam tüketilen miktar ile aynıdır. |
|Taahhüt İçinde |6 veya 12 aylık teklifinizle ilişkili olan taahhüt miktarınızdan çıkartılır ölçüm ücretlerini gösterir. Ölçüm ücretleri kronolojik sırayla düşülür. |
|Para birimi |Geçerli fatura döneminize kullanılan para birimi |
|Kapasite Aşımı |Ölçüm ücretleri, 6 veya 12 aylık teklifinizle ilişkili olan taahhüt miktarınızı aşan gösterir |
|Taahhüt Tarifesi |6 veya 12 aylık teklifinizle ilişkili olan toplam taahhüt miktarınıza bağlı taahhüt ücretini gösterir |
|Fiyat |Faturalanabilir birim başına ücretlendirildiğiniz oranı |
|Değer |Fazla kullanım miktarı sütununun ücret sütunuyla çarpılmasıyla elde edilen sonucu gösterir. Tüketilen Miktar dahil edilen miktarı aşmıyorsa, bu sütunda ücret alınmaz. |

### <a name="daily-usage"></a>Günlük kullanım

CSV dosyasının günlük kullanım bölümüne faturalandırma ücretleri etkileyen kullanım ayrıntılarını gösterir. Aşağıdaki tabloda, koşulları ve bu bölümde gösterilen açıklamalarını listeler.

| Sözleşme Dönemi | Açıklama |
| --- | --- |
|Kullanım Tarihi |Ölçüm kullanıldığında tarihi |
|Ölçüm Kategorisi |Bu kullanımın ait olduğu için üst düzey hizmeti belirtir |
|Ölçüm Kimliği |Fatura kullanımını fiyatlandırmak için kullanılan Faturalanan ölçüm tanımlayıcısı |
|Ölçüm Alt Kategorisi |Hızı etkileyen Azure hizmet türü tanımlar. |
|Ölçüm Adı |Tüketilen ölçüm için ölçü birimini belirtir |
|Ölçüm Bölgesi |Veri Merkezi konumuna göre ücretlendirilen belirli hizmetler için veri merkezinin konumunu belirtir |
|Birim |Ölçüm ücretlendirildiği birimi tanımlar. Örneğin GB, saat, 10.000 s. |
|Kullanılan Miktar |Belirli bir günde tüketilen ölçüm miktarı |
|Kaynak Konumu |Ölçüm çalıştığı veri merkezini tanımlar |
|Kullanılan Hizmet |Kullandığınız bir Azure platform hizmetini |
|Kaynak Grubu |Kaynak grubu içinde dağıtılan ölçüm çalıştığı. <br/><br/>Daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). |
|Örnek Kimliği | Ölçüm tanımlayıcısı. <br/><br/> Tanımlayıcı oluşturulduğunda, ölçüm için belirttiğiniz adı içerir. Kaynağın adını veya tam kaynak kimliği olan Daha fazla bilgi için [Azure Resource Manager API'si](https://docs.microsoft.com/rest/api/resources/resources). |
|Etiketler | Etiket için ölçüm atayın. Fatura kayıtların gruplandırılması için etiketleri kullanın.<br/><br/>Örneğin, ölçüm kullanan departmana göre maliyetleri dağıtmak için etiketleri kullanabilirsiniz. Etiket göstermeyi destekleyen hizmetleri, sanal makineler, depolama ve Ağ Hizmetleri kullanılarak sağlanan [Azure Resource Manager API'si](https://docs.microsoft.com/rest/api/resources/resources). Daha fazla bilgi için [etiketlerle Azure kaynaklarınızı düzenleme](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/). |
|Ek Bilgi |Hizmete özgü meta veriler. Örneğin, bir sanal makine için görüntü türü. |
|Hizmet Bilgisi 1 |Aboneliğinizdeki hizmetin ait olduğu proje adı |
|Hizmet Bilgisi 2 |İsteğe bağlı hizmete özgü meta veriler yakalanır eski alan |

## <a name="how-do-i-make-sure-that-the-charges-in-my-detailed-usage-file-are-correct"></a>Ayrıntılı kullanım dosyamı ücretleri doğru olduğundan emin nasıl emin olabilirim?
Daha fazla ayrıntı istediğiniz ayrıntılı kullanım dosyanızdaki bir ücret yoksa bkz [Microsoft Azure için faturanızı anlayın.](./billing-understand-your-bill.md)

## <a name="external"></a>Dış hizmet ücretlerini hakkında neler diyeceksiniz?
Dış hizmetlere (Market siparişlerini olarak da bilinir) bağımsız hizmeti satıcısı tarafından sağlanır ve ayrı olarak faturalandırılır. Ücretler Azure fatura üzerinde gösterme. Daha fazla bilgi için bkz. [Azure, dış hizmet ücretlerini anlama](billing-understand-your-azure-marketplace-charges.md).

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).
