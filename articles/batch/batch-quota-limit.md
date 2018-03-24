---
title: Azure toplu işlem için kotalar ve sınırlar hizmet | Microsoft Docs
description: Varsayılan Azure Batch kotalar, sınırlar ve kısıtlamalar hakkında bilgi edinin ve kota istemek nasıl artırır
services: batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: 28998df4-8693-431d-b6ad-974c2f8db5fb
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3cc833e456571b63fa03574808529c8c501d7ab5
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="batch-service-quotas-and-limits"></a>Batch hizmet kotaları ve limitleri

Diğer Azure hizmetleriyle bulunduğundan sınırları Batch hizmetiyle ilişkili bazı kaynaklar üzerinde. Çoğu bu sınırları abonelik veya hesap düzeyinde Azure tarafından uygulanan varsayılan kotaları bulunur. Bu makalede bu Varsayılanları ele ve kota nasıl istek artırır.

Bu kotalar, tasarım ve ölçek büyütme toplu iş yükleri aklınızda bulundurun. Havuzunuz belirttiğiniz işlem düğümleri sayısını ulaşırsa değil, örneğin, çekirdek kota toplu işlem hesabı için sınırına.

Tek bir Batch hesabında birden fazla Batch iş yükü çalıştırabilir ya da iş yüklerinizi aynı abonelik ve farklı Azure bölgelerindeki Batch hesapları arasında dağıtabilirsiniz.

Üretim iş yükleri toplu işlemde çalıştırmayı planlıyorsanız, bir veya daha fazla varsayılan yukarıda kotaları artırmak gerekebilir. Bir kota yükseltmek istiyorsanız, çevrimiçi açabilirsiniz [müşteri destek isteği](#increase-a-quota) herhangi bir ücret alınmaz.

> [!NOTE]
> Bir kota kapasitesi garantisi bir kredi sınırı ' dir. Büyük ölçekli kapasite gereksinimlerini varsa, lütfen Azure desteğine başvurun.
> 
> 

## <a name="resource-quotas"></a>Kaynak kotaları
[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]


### <a name="cores-quotas-in-user-subscription-mode"></a>Kullanıcı abonelik modunda çekirdek kotaları

Havuzu ayırma modu ayarlamak toplu işlem hesabı oluşturduysanız **kullanıcı aboneliği**, kotalar farklı uygulanır. Bir havuz oluşturduğunuzda bu modda, aboneliğinizde doğrudan toplu VM'ler ve diğer kaynakları oluşturulur. Azure Batch çekirdek kotaları bu modda oluşturulan bir hesap için geçerli değildir. Bunun yerine, bölgesel aboneliğinizin kotalarda çekirdek işlem ve diğer kaynaklara uygulanır. Bu Kotalar hakkında daha fazla bilgi [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../azure-subscription-service-limits.md).

## <a name="other-limits"></a>Diğer sınırları
| **Kaynak** | **Üst Sınır** |
| --- | --- |
| [Eş zamanlı görevleri](batch-parallel-node-tasks.md) işlem düğümü başına |düğüm çekirdeği 4 x sayısı |
| [Uygulamaları](batch-application-packages.md) toplu işlem hesabı başına |20 |
| Uygulama başına uygulama paketleri |40 |
| Uygulama paketi boyutu (her) |Yaklaşık 195 GB'den<sup>1</sup> |
| En fazla başlangıç görevi boyutu | 32768 karakter<sup>2</sup> |
| En fazla görev yaşam süresi | 7 gün<sup>3</sup> |
| İşlem düğümleri havuzunda etkin düğümler arası iletişim | 100 |

<sup>1</sup> en büyük blok blob boyutu için azure depolama sınırı<br />
<sup>2</sup> kaynak dosyaları ve ortam değişkenleri içerir<br />
<sup>3</sup> tamamlandığında, bunu projeye eklendiğinde bir görevin, en uzun kullanım ömrünü 7 gündür. Tamamlanan görevler süresiz olarak kalır. En uzun yaşam süresi içinde tamamlanmamış görevlerin verileri erişilemez.


## <a name="view-batch-quotas"></a>Toplu kotaları görüntüle
Toplu işlem hesabı kotalarda görüntülemek [Azure portal][portal].

1. Seçin **Batch hesapları** portalda, ardından, ilgilendiğiniz toplu işlem hesabı seçin.
2. Seçin **kotaları** toplu işlem hesabının menüsünde.
3. Batch hesabına uygulanmakta kotalarını görüntüleyin
   
    ![Toplu işlem hesabı kotası][account_quotas]



## <a name="increase-a-quota"></a>Kota artırma
Bir kota istemek için aşağıdaki adımları kullanarak veya toplu iş hesabınız için artırmak izleyin [Azure portal][portal]. Kota artışı türünü Batch hesabınıza havuzu ayırma moduna bağlıdır.

### <a name="increase-a-batch-cores-quota"></a>Toplu işlem çekirdek kota artırma 

1. Seçin **Yardım + Destek** döşeme portalı panonuza veya soru işareti (**?**) portalının sağ üst köşedeki.
2. Seçin **yeni destek isteği** > **Temelleri**.
3. İçinde **Temelleri**:
   
    a. **Sorun türü** > **kota**
   
    b. Aboneliğinizi seçin.
   
    c. **Kota türü** > **toplu işlem**
   
    d. **Destek planınız** > **kota desteği - dahil**
   
    **İleri**’ye tıklayın.
4. İçinde **sorun**:
   
    a. Seçin bir **önem** göre [iş etkisi][support_sev].
   
    b. İçinde **ayrıntıları**, değiştirmek istediğiniz her kota, toplu işlem hesabı adı ve yeni sınırını belirtin.
   
    **İleri**’ye tıklayın.
5. İçinde **kişi bilgileri**:
   
    a. Seçin bir **tercih edilen iletişim yöntemi**.
   
    b. Doğrulayın ve gerekli kişi ayrıntılarını girin.
   
    Destek isteğini göndermek için **Oluştur**'a tıklayın.

Destek İsteği gönderdikten sonra Azure destek, sizinle iletişim kuracaktır. İsteği Tamamlanıyor en çok 2 iş günü alabileceğine dikkat edin.


## <a name="related-topics"></a>İlgili konular
* [Azure Portalı'nı kullanarak bir Azure Batch hesabı oluşturma](batch-account-create-portal.md)
* [Azure Batch özelliklerine genel bakış](batch-api-basics.md)
* [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.png
