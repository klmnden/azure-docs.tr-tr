---
title: Otomasyon Azure ayırma için API'leri | Microsoft Docs
description: Program aracılığıyla ayırma bilgilerini almak için kullanabileceğiniz Azure API'ler hakkında bilgi.
documentationcenter: ''
author: yashesvi
manager: yashesvi
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/13/2019
ms.author: banders
ms.openlocfilehash: 246278df61d4f13e2634a1cdfc5ff6b635cecbbf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60371215"
---
# <a name="apis-for-azure-reservation-automation"></a>Otomasyon Azure ayırma için API'leri

Program aracılığıyla Azure hizmeti veya yazılımı ayırmaları kuruluşunuz için bilgi almak için Azure API'lerini kullanın.

## <a name="find-reservation-plans-to-buy"></a>Rezervasyon satın alma planlamaktadır Bul

Rezervasyon satın almayı planladığınız öneriler almak için öneri API'si, kuruluşunuzun kullanımına göre ayırma kullanın. Daha fazla bilgi için [ayırma öneriler alın](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation).

Tüketim API kullanım ayrıntılarını kullanarak, kaynak kullanımınızı analiz edebilirsiniz. Daha fazla bilgi için [kullanım ayrıntıları - listesi için faturalama dönemi Faturalama hesabı](/rest/api/consumption/usagedetails/list#billingaccountusagedetailslistforbillingperiod). Azure kaynaklarını tutarlı bir şekilde kullanmak genellikle en iyi için bir ayırma adaydır.

## <a name="buy-a-reservation"></a>Ayırma satın alma

Şu anda bir ayırma program aracılığıyla satın aldığınız olamaz. Rezervasyon satın almak için aşağıdaki makalelere bakın:

Hizmet planları:
- [Sanal makine](../virtual-machines/windows/prepay-reserved-vm-instances.md?toc=/azure/billing/TOC.json)
-  [Cosmos DB](../cosmos-db/cosmos-db-reserved-capacity.md?toc=/azure/billing/TOC.json)
- [SQL Veritabanı](../sql-database/sql-database-reserved-capacity.md?toc=/azure/billing/TOC.json)

Yazılım planları:
- [SUSE Linux yazılım](../virtual-machines/linux/prepay-suse-software-charges.md?toc=/azure/billing/TOC.json)

## <a name="get-reservations"></a>Rezervasyonlar Al

Bir Kurumsal Anlaşma (EA müşterisinin) ile bir Azure müşterisi iseniz, ayırmaları kuruluşunuz kullanarak satın alabilirsiniz [ayrılmış örnek alma işlem ücretleri API](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-charges). Diğer abonelikler için satın aldığınız ayırmaları listesini almak ve API'yi kullanarak görüntüleme izinlerine sahip [rezervasyon siparişi - liste](/rest/api/reserved-vm-instances/reservationorder/list). Varsayılan olarak, hesap sahibi veya rezervasyon satın kişi ayırma görüntüleme izinlerine sahiptir.

## <a name="see-reservation-usage"></a>Ayırma kullanım bakın

Bir EA müşterisiyseniz, rezervasyonları, kuruluşunuzda nasıl kullanıldığını programlı bir şekilde görüntüleyebilirsiniz. Daha fazla bilgi için [Kurumsal müşteriler için alma ayrılmış örnek kullanımını](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage). Diğer abonelikler için API'yi kullanmak [ayırmaları özetleri - listesi tarafından Rezervasyon siparişi ve ayırma](/rest/api/consumption/reservationssummaries/listbyreservationorderandreservation).

Kuruluşunuzun ayırmaları altında kullanılan olduğunu fark ederseniz:

- Kuruluşunuz tarafından oluşturulan sanal makineler üzerinde ayırma VM boyutu eşleştiğinden emin olun.
- Örnek boyutu esneklik olduğundan emin olun. Daha fazla bilgi için [Yönet ayırmaları - değişiklik iyileştirmek için ayrılmış VM örnekleri ayarlama](billing-manage-reserved-vm-instance.md#change-optimize-setting-for-reserved-vm-instances).
- Rezervasyon daha geniş kapsamda geçerli olacak şekilde paylaşılan için kapsamı değiştirin. Daha fazla bilgi için [ayırmalarını yönetme - bir rezervasyon için kapsam değiştirme](billing-manage-reserved-vm-instance.md#change-the-reservation-scope).
- Kullanılmamış miktar exchange. Daha fazla bilgi için [ayırmaları - iptalleri ve değişimleri yönetme](billing-manage-reserved-vm-instance.md#cancellations-and-exchanges).

## <a name="give-access-to-reservations"></a>Ayırmalar için erişim verin

Bir kullanıcının kullanarak erişimi olan tüm ayırmaları'nın listesini alın [ayırma - işlem - listesi API'sini](/rest/api/reserved-vm-instances/reservationorder/list). Rezervasyon için programlı erişim vermek için aşağıdaki makalelerden birine bakın:

- [RBAC ve REST API kullanarak erişimini yönetme](../role-based-access-control/role-assignments-rest.md)
- [RBAC ve Azure PowerShell kullanarak erişimini yönetme](../role-based-access-control/role-assignments-powershell.md)
- [RBAC ve Azure CLI kullanarak erişimini yönetme](../role-based-access-control/role-assignments-cli.md)

## <a name="split-or-merge-reservation"></a>Bölme ve birleştirme ayırma

Birden fazla kaynak örneği içindeki bir ayırma satın sonra bu ayırma örneklerinden farklı aboneliklere atamak isteyebilirsiniz. Ayırma kapsamı, aynı fatura bağlam içinde tüm abonelikleri için geçerli olacak şekilde değiştirebilirsiniz. Ancak, maliyet Yönetimi'ni veya ödemeyle amacıyla, kapsamı "tek bir abonelik" olarak tutun ve ayırma örnekleri için belirli bir aboneliğe atamak isteyebilirsiniz.

Rezervasyon bölmek için API'yi kullanın. [ayırma - bölünmüş](/rest/api/reserved-vm-instances/reservation/split). Ayrıca, PowerShell kullanarak bir ayırma bölebilirsiniz. Daha fazla bilgi için [ayırmaları - bölünmüş ayırma iki ayırmalar halinde yönetme](billing-manage-reserved-vm-instance.md#split-a-single-reservation-into-two-reservations).

Bir ayırma iki ayırma birleştirmek için API'yi kullanın. [ayırma - birleştirme](/rest/api/reserved-vm-instances/reservation/merge).

## <a name="change-scope-for-a-reservation"></a>Rezervasyon için kapsam Değiştir

Ayırma kapsamı tek bir abonelik veya tüm abonelikler, faturalama bağlamında olabilir. Tek bir abonelik için kapsamı ayarlarsanız, seçili Abonelikteki kaynaklar çalışan ayırma eşleştirilir. Paylaşılan kapsam ayarlarsanız, Azure faturalandırma bağlam içinde tüm abonelikleri çalışan kaynaklar ayırmaya eşleşir. Fatura bağlamı, rezervasyon satın almak için kullanılan abonelik üzerinde bağlıdır. Daha fazla bilgi için [ayırmalarını yönetme - Kapsamı Değiştir](billing-manage-reserved-vm-instance.md#change-the-reservation-scope).

Kapsam programlı olarak değiştirmek için API'yi kullanın. [ayırma - güncelleştirme](/rest/api/reserved-vm-instances/reservation/update).

## <a name="learn-more"></a>Daha fazla bilgi edinin

- [Azure için ayırmaları nelerdir](billing-save-compute-costs-reservations.md)
- [VM ayırma indirimi nasıl uygulanacağını anlama](billing-understand-vm-reservation-charges.md)
- [SUSE Linux Enterprise yazılım planı indirim nasıl uygulanacağını anlama](billing-understand-suse-reservation-charges.md)
- [Nasıl diğer ayırma indirimi uygulanmaz anlama](billing-understand-reservation-charges.md)
- [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](billing-understand-reserved-instance-usage.md)
- [Kurumsal kayıt için ayırma kullanımını anlama](billing-understand-reserved-instance-usage-ea.md)
- [Windows yazılım maliyetleri ile ayırmaları dahil değil](billing-reserved-instance-windows-software-costs.md)
- [İş ortağı merkezi bulut çözümü sağlayıcısı (CSP) programında Azure ayırmalar](https://docs.microsoft.com/partner-center/azure-reservations)
