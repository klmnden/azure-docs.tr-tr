---
title: include dosyası
description: include dosyası
services: azure-policy
author: craigshoemaker
ms.service: azure-policy
ms.topic: include
ms.date: 05/18/2018
ms.author: cshoe
ms.custom: include file
ms.openlocfilehash: e6a0ded137162328fd446b65ddb4a15fa6f1db88
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61478795"
---
## <a name="deleting-personal-information"></a>Kişisel bilgi siliniyor

[!INCLUDE [gdpr-intro-sentence.md](gdpr-intro-sentence.md)]

İçeri/dışarı aktarma için ilgili kişisel bilgiler (portal ve API) içeri aktarma sırasında hizmet ve işlem dışarı aktarma. Bu işlemler sırasında kullanılan verileri içerir:

- Kişi adı
- Telefon numarası
- Email
- Açık adres
- Şehir
- Posta kodu
- Durum
- Ülke/İl/Bölge
- Sürücü kimliği
- Taşıyıcı hesap numarası
- Kargo takip numarası

İçeri/dışarı aktarma işi oluşturulduğunda, kullanıcılara kişi bilgileri ve teslimat adresi sağlar. Kişisel bilgileri, en fazla iki farklı konumlarda depolanır: işi ve isteğe bağlı olarak portal ayarları. Etiketli, onay kutusunu işaretlerseniz kişisel bilgileri portal ayarlarında yalnızca depolanan **taşıyıcı ve iade adresini varsayılan olarak Kaydet** sırasında *iade sevkiyat bilgilerini* bölümünü dışa aktarma işlemi.

Kişisel bilgilerini aşağıdaki yollarla silinebilir:

- İş ile kaydedilen veriler iş silinir. Kullanıcılar işleri el ile silebilir ve tamamlanan işleri otomatik olarak 90 gün sonra silinir. REST API ya da Azure portal aracılığıyla işleri el ile silebilirsiniz. Azure portalında işi silmek için içeri/dışarı aktarma işinize gidin ve tıklayın *Sil* komut çubuğundan. Bir REST API aracılığıyla içeri/dışarı aktarma işini silme konusunda daha fazla ayrıntı için başvurmak [içeri/dışarı aktarma işini Sil](../articles/storage/common/storage-import-export-cancelling-and-deleting-jobs.md).

- Kişi bilgileri portal ayarları kaydedildi, portal ayarlarını silerek kaldırılabilir. Portal ayarları, aşağıdaki adımları izleyerek silebilirsiniz:
  - [Azure Portal](https://portal.azure.com) oturum açın.
  - Tıklayarak *ayarları* simgesi ![Azure ayarlar simgesi](media/storage-import-export-delete-personal-info/azure-settings-icon.png)
  - Tıklayın *tüm ayarları dışarı aktar* (geçerli ayarlarınızı kaydetmek için bir `.json` dosyası).
  - Tıklayın *tüm ayarları ve özel panoları Sil* kaydedilmiş ilgili bilgiler dahil olmak üzere, tüm ayarlarını silmek için.

Daha fazla bilgi için Microsoft Privacy ilke gözden geçirme [Güven Merkezi](https://www.microsoft.com/trustcenter)