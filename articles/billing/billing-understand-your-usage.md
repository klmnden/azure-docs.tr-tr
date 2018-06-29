---
title: Azure ayrıntılı kullanımınızı anlamak | Microsoft Docs
description: Okuma ve ayrıntılı kullanımınızı CSV Azure aboneliğiniz için bölümlerini anlama hakkında bilgi edinin
services: ''
documentationcenter: ''
author: tonguyen10
manager: tonguyen
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: tonguyen
ms.openlocfilehash: 723e42d2bb2af09eb2236c3cbefeee33987ea45b
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37060809"
---
# <a name="understand-terms-on-your-microsoft-azure-detailed-usage-charges"></a>Microsoft Azure ayrıntılı kullanım ücretlerini koşullarınızda anlama 

Ayrıntılı kullanım ücretlerini CSV dosyası geçerli fatura dönemi için günlük ve ölçüm düzeyi kullanım ücretleri içerir. 

Ayrıntılı kullanım dosyanızı almak için bkz: [faturayı ve günlük kullanım verileri faturalama Azure alma](billing-download-azure-invoice-daily-usage-date.md).
Elektronik tablo uygulamasında açabilirsiniz bir virgülle ayrılmış değerler (.csv) dosya biçiminde kullanılabilir. Kullanılabilir iki sürümlerini görürseniz, sürüm 2 indirin. En güncel bir dosya biçimidir.

Kullanım ücretleri olan toplam **aylık** bir abonelik giderler. Kullanım ücretleri KREDİLERİ veya indirimleri dikkate yok.

>[!VIDEO https://www.youtube.com/embed/p13S350M2Vk]

## <a name="detailed-terms-and-descriptions-of-your-detailed-usage-file"></a>Ayrıntılı hüküm ve ayrıntılı kullanım dosyanızın açıklamaları

Aşağıdaki bölümlerde ayrıntılı kullanım dosyasını 2. sürümünü gösterilen önemli terimler açıklanmaktadır.

### <a name="statement"></a>Bildirim

Ayrıntılı kullanım CSV dosyasının üst kısmında ayın fatura döneminde kullanılan hizmetleri gösterir. Aşağıdaki tabloda terimleri ve bu bölümde gösterilen açıklamaları listeler.

| Sözleşme Dönemi | Açıklama |
| --- | --- |
|Fatura Dönemi |Metre kullanıldığında fatura dönemi |
|Ölçüm Kategorisi |Kullanım için üst düzey hizmet tanımlar |
|Ölçüm Alt Kategorisi |Hızı etkileyebilir Azure hizmet türünü tanımlar |
|Ölçüm Adı |Tüketilen ölçer ölçü tanımlar |
|Ölçüm Bölgesi |Veri merkezi konum temelinde fiyatlandırılır belirli hizmetler için veri merkezi konumunu tanımlar |
|SKU |Her Azure ölçer benzersiz sistem tanımlayıcısını tanımlar |
|Birim |Hizmetin ücretlendirildiği Birimi tanımlar. Örneğin, GB, saat, 10.000 s. |
|Kullanılan Miktar |Fatura döneminde kullanılan ölçer miktarı |
|Dahil Edilen Miktar |Ücret ödemeden, geçerli fatura dönemi içinde bulunan ölçer miktarı |
|Kapasite Aşım Miktarı |Tüketilen miktarı ve dahil edilen miktar arasındaki farkı gösterir. Bu tutar için fatura. Kullandıkça Öde teklifleri için dahil edilen miktar ile teklif ile bu toplam tüketilen miktar ile aynıdır. |
|Taahhüt İçinde |6 veya 12 aylık teklifle ilgili, taahhüdü tutarı çıkartma ölçer ücretleri gösterir. Ölçer ücretleri kronolojik sırada düşülür. |
|Para birimi |Geçerli fatura döneminde kullanılan para birimi |
|Kapasite Aşımı |6 veya 12 aylık teklifle ilgili, taahhüt tutarı aşan ölçer ücretleri gösterir |
|Taahhüt Tarifesi |6 veya 12 aylık teklifle ilgili toplam taahhüdü tutarı temel taahhüt oranı gösterir |
|Fiyat |Faturalanabilir birim başına ücret ödersiniz oranı |
|Değer |Fazla kullanım miktarı sütun oranı sütuna göre çarparak sonucunu gösterir. Tüketilen Miktar dahil edilen miktar aşmadığından, bu sütunda hiçbir ücret yoktur. |

### <a name="daily-usage"></a>Günlük kullanım

CSV dosyasının günlük kullanım bölümü fatura oranları etkileyen kullanım ayrıntılarını gösterir. Aşağıdaki tabloda terimleri ve bu bölümde gösterilen açıklamaları listeler.

| Sözleşme Dönemi | Açıklama |
| --- | --- |
|Kullanım Tarihi |Ölçer ne zaman kullanıldığı tarih |
|Ölçüm Kategorisi |Bu kullanım ait olduğu için üst düzey hizmet tanımlar |
|Ölçüm Kimliği |Fatura kullanım fiyat için kullanılan Faturalanan ölçer tanımlayıcısı |
|Ölçüm Alt Kategorisi |Hızı etkileyebilir Azure hizmet türünü tanımlar |
|Ölçüm Adı |Tüketilen ölçer ölçü tanımlar |
|Ölçüm Bölgesi |Veri merkezi konum temelinde fiyatlandırılır belirli hizmetler için veri merkezi konumunu tanımlar |
|Birim |Ölçer içinde doludur birimi tanımlar. Örneğin, GB, saat, 10.000 s. |
|Kullanılan Miktar |O gün için tüketilen ölçer miktarı |
|Kaynak Konumu |Ölçer çalıştığı datacenter tanımlar |
|Kullanılan Hizmet |Kullandığınız Azure platformu hizmeti |
|Kaynak Grubu |Dağıtılan ölçer çalıştırıldığı, kaynak grubu. <br/><br/>Daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). |
|Örnek Kimliği | Ölçer tanımlayıcısı. <br/><br/> Tanımlayıcı ad oluşturulduğunda ölçer belirtin içerir. Kaynağın adını veya tam nitelikli kaynak kimliği olan Daha fazla bilgi için bkz: [Azure Kaynak Yöneticisi API'si](https://docs.microsoft.com/rest/api/resources/resources). |
|Etiketler | Etiket ölçer atayın. Fatura Kayıtları gruplandırmak için etiketler kullanın.<br/><br/>Örneğin, etiketleri ölçer kullanan bölümünüzün maliyetleri dağıtmak için kullanabilirsiniz. Verme etiketleri destekleyen hizmetlerin olan sanal makineler, depolama ve Ağ Hizmetleri kullanılarak sağlanan [Azure Kaynak Yöneticisi API'si](https://docs.microsoft.com/rest/api/resources/resources). Daha fazla bilgi için bkz: [etiketlerle Azure kaynaklarınızı düzenleme](http://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/). |
|Ek Bilgi |Hizmete özgü meta veriler. Örneğin, bir görüntü türü için bir sanal makine. |
|Hizmet Bilgisi 1 |Hizmet, aboneliğinizde ait proje adı |
|Hizmet Bilgisi 2 |İsteğe bağlı hizmete özgü meta veriler yakalar eski alan |

## <a name="how-do-i-make-sure-that-the-charges-in-my-detailed-usage-file-are-correct"></a>Ücretleri my ayrıntılı kullanımı dosyasında doğru olduğundan emin nasıl emin?
Daha fazla ayrıntı istediğiniz ayrıntılı kullanımı dosyanızı üzerinde bir ücret ise bakın [Microsoft Azure için faturanızı anlamak.](./billing-understand-your-bill.md)

## <a name="external"></a>Dış servis ücretleri nasıldır?
Dış hizmetler (Market siparişleri olarak da bilinir) bağımsız hizmet satıcıları tarafından sağlanan ve ayrı olarak faturalandırılır. Ücretler Azure fatura üzerinde gösterme. Daha fazla bilgi için bkz: [, Azure dış hizmet ücretlerini anlama](billing-understand-your-azure-marketplace-charges.md).

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.
Hala yardıma gereksiniminiz varsa [desteğine başvurun](https://portal.azure.com/?) hızla çözümlenen sorunu almak için.
