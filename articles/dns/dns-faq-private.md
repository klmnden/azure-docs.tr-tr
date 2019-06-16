---
title: Azure özel DNS SSS
description: Azure özel DNS hakkında sık sorulan sorular
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 6/12/2019
ms.author: victorh
ms.openlocfilehash: c963cb1b6930b41a703b479e0213311d971e6606
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67082852"
---
# <a name="azure-private-dns-faq"></a>Azure özel DNS SSS

> [!IMPORTANT]
> Azure özel DNS şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="does-azure-dns-support-private-domains"></a>Azure DNS özel etki alanları destekliyor mu?

Özel etki alanı için destek, Azure özel DNS bölgeleri özelliğini kullanarak desteklenir. Özel DNS bölgeleri, aynı araçları kullanarak internet'e yönelik Azure DNS bölgelerini yönetilir. Bunlar, belirtilen sanal ağ dns'sinden yalnızca. Daha fazla bilgi için [genel bakış](private-dns-overview.md).

Azure'da diğer iç DNS seçenekleri hakkında daha fazla bilgi için bkz: [VM'ler ve rol örnekleri için ad çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

## <a name="will-azure-dns-private-zones-work-across-azure-regions"></a>Azure bölgeleri arasında Azure DNS özel bölgeleri çalışacak mı?

Evet. Özel bölgeler için DNS çözümlemesi Azure bölgelerindeki sanal ağları arasında desteklenir. Özel bölgeler bile açıkça sanal ağları eşleme olmadan çalışır. Tüm sanal ağları, çözümleme sanal ağları özel bölge olarak belirtilmelidir. Müşteriler, bir bölgeden diğerine Akış TCP/HTTP trafiği için eşlenmiş sanal ağlar gerekebilir.

## <a name="is-connectivity-to-the-internet-from-virtual-networks-required-for-private-zones"></a>Bağlantı, özel bölgeler için gereken sanal ağlardan Internet'e mi?

Hayır. Özel bölgeler, sanal ağlar ile birlikte çalışır. Müşteriler, bunları sanal makineleri veya diğer kaynaklar içinde hem de sanal ağlar arasında etki alanlarını yönetmek için kullanın. Internet bağlantısı ad çözümlemesi için gerekli değildir.

## <a name="can-the-same-private-zone-be-used-for-several-virtual-networks-for-resolution"></a>Aynı özel bölge çözümlemesi için birkaç sanal ağlar için kullanılabilir mi?

Evet. En fazla 1000 sanal ağlar tek bir özel bölge ile ilişkilendirebilirsiniz.

## <a name="can-a-virtual-network-that-belongs-to-a-different-subscription-be-added-as-a-linked-virtual-network-to-a-private-zone"></a>Farklı bir aboneliğe ait bir sanal ağ özel bir bölgeye bağlı bir sanal ağ eklenebilir?

Evet. Sanal ağlar ve özel DNS bölgesi yazma işlemi izni olmalıdır. Birkaç RBAC rolleri için yazma izni verilebilir. Örneğin, Klasik ağ Katılımcısı RBAC rolü sanal ağlar için yazma izinlerine sahiptir. RBAC rolleri hakkında daha fazla bilgi için bkz. [rol tabanlı erişim denetimi](../role-based-access-control/overview.md).

## <a name="will-the-automatically-registered-virtual-machine-dns-records-in-a-private-zone-be-automatically-deleted-when-you-delete-the-virtual-machine"></a>Sanal makine sildiğinizde, özel bir bölge içinde otomatik olarak kayıtlı sanal makinenin DNS kayıtlarını otomatik olarak silinir?

Evet. Otomatik kayıt etkin ile bağlı bir sanal ağ içindeki sanal makineyi silme kayıtlı kayıtları otomatik olarak silinir.

## <a name="can-an-automatically-registered-virtual-machine-record-in-a-private-zone-from-a-linked-virtual-network-be-deleted-manually"></a>Bağlı sanal ağ özel bir bölgedeki bir sanal makine otomatik olarak kayıtlı kaydı el ile silinebilir?

Evet. Bölgesinde el ile oluşturulan DNS kaydını otomatik olarak kayıtlı DNS kayıtlarını üzerine yazabilirsiniz. Bu konuda aşağıdaki soru ve yanıt adresi.

## <a name="what-happens-when-i-try-to-manually-create-a-new-dns-record-into-a-private-zone-that-has-the-same-hostname-as-an-automatically-registered-existing-virtual-machine-in-a-linked-virtual-network"></a>El ile bağlı bir sanal ağda aynı ana bilgisayar adı olarak var olan bir sanal makine otomatik olarak kayıtlı olan bir özel bölge içine yeni bir DNS kaydı oluşturmak çalıştığınızda ne olur?

El ile bağlı bir sanal ağda var olan ve otomatik olarak kayıtlı bir sanal makine olarak aynı ana bilgisayar adı olan bir özel bölge içine yeni bir DNS kaydı oluşturma deneyin. Bunu yaptığınızda yeni DNS kaydını otomatik olarak kayıtlı sanal makine kaydı üzerine yazar. El ile oluşturulan bu DNS kaydını yeniden bölgeden silmeye çalışırsanız, silme başarılı olur. Otomatik kayıt, sanal makine hala mevcut olduğundan ve özel bir IP, kendisine eklenmiş sürece yeniden gerçekleşir. DNS kaydını otomatik olarak bölgede yeniden oluşturulur.

## <a name="what-happens-when-we-unlink-a-linked-virtual-network-from-a-private-zone-will-the-automatically-registered-virtual-machine-records-from-the-virtual-network-be-removed-from-the-zone-too"></a>Size özel bir bölgeden bağlı bir sanal ağ bağlantısını ne olur? Sanal ağdan sanal makine otomatik olarak kayıtlı kayıtları bölgeden çok kaldırılacak?

Evet. Özel bir bölgeden bağlı bir sanal ağ bağlantısını kaldırmak için ilişkili sanal ağ bağlantısını kaldırmak için DNS bölgesini güncelleştirme. Bu işlemde otomatik olarak kaydedilmiş bir sanal makine kayıtları bölgesinden kaldırıldı.

## <a name="what-happens-when-we-delete-a-linked-virtual-network-thats-linked-to-a-private-zone-do-we-have-to-manually-update-the-private-zone-to-unlink-the-virtual-network-as-a-linked-virtual-network-from-the-zone"></a>Bir özel bölgesiyle bağlantılı bağlı bir sanal ağ silmemiz ne olur? Bir bölge sanal ağdan bağlı olarak sanal ağ bağlantısını için özel bölge el ile güncelleştirmek zorunda mıyım?

Evet. Bağlı sanal ağ özel bölgesinden ilk bağlantısını olmadan sildiğinizde, silme işlemi başarılı olur. Ancak sanal ağ özel bölgeden varsa otomatik olarak bağlantısız değildir. El ile özel bölge sanal ağdan bağlantısını kaldırmanız gerekir. Bu nedenle, silmeden önce sanal ağınızdan özel bölgenizi bağlantısını Kaldır.

## <a name="will-dns-resolution-by-using-the-default-fqdn-internalcloudappnet-still-work-even-when-a-private-zone-for-example-privatecontosocom-is-linked-to-a-virtual-network"></a>Hatta özel bir bölgesi (örneğin, private.contoso.com) bir sanal ağa bağlandığında kullanarak FQDN (internal.cloudapp.net) varsayılan DNS çözümlemesi çalışmaya devam eder mi?

Evet. Özel bölgeler için varsayılan DNS çözümleri, Azure tarafından sağlanan internal.cloudapp.net bölge kullanarak yerini almaz. Bir ek özellik veya geliştirme olarak sunulur. Azure tarafından sağlanan internal.cloudapp.net veya kendi özel bölge kullanan, karşı çözümlemek istediğiniz bölgeyi FQDN'sini kullanın.

## <a name="will-the-dns-suffix-on-virtual-machines-within-a-linked-virtual-network-be-changed-to-that-of-the-private-zone"></a>Bağlı sanal ağ içindeki sanal makinelerde DNS soneki, özel bölge değiştirilecek?

Hayır. Sanal makinelere bağlı sanal ağınızdaki DNS soneki, varsayılan Azure tarafından sağlanan sonek olarak kalır ("*. internal.cloudapp.net"). El ile bu DNS soneki üzerindeki sanal makinelerinize, özel bölge değiştirebilirsiniz.

## <a name="what-are-the-usage-limits-for-azure-private-dns"></a>Azure özel DNS kullanım sınırları nelerdir?

Azure özel DNS kullandığınızda aşağıdaki varsayılan sınırlar geçerlidir.

| Resource | Varsayılan limit |
| --- | --- |
|Abonelik başına özel DNS bölgeleri|1000|
|Özel DNS bölge başına kayıt kümeleri|25,000|
|Kayıt kümesi başına kayıt|20|
|Özel DNS bölge başına sanal ağ bağlantıları|1000|
|Otomatik kaydı ile özel DNS bölgelerini başına sanal ağlara bağlantılar etkin|100|
|Bir sanal ağ otomatik etkin kayıt ile bağlantılı özel DNS bölgelerini sayısı|1|
|Özel DNS bölgelerini, bir sanal ağa bağlı sayısı|1000|

## <a name="is-there-portal-support-for-private-zones"></a>Özel bölgeler için portal destek var mı?

Evet ve API'ler, PowerShell, CLI ve SDK'lar önceden oluşturulmuş özel bölgeler Azure portalında görünür.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure özel DNS hakkında daha fazla bilgi edinin](private-dns-overview.md)