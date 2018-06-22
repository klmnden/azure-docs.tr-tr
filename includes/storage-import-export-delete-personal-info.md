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
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36313889"
---
## <a name="deleting-personal-information"></a>Kişisel bilgiler siliniyor

[!INCLUDE [gdpr-intro-sentence.md](gdpr-intro-sentence.md)]

İçeri/dışarı aktarma için ilgili kişisel bilgiler verme işlemleri ve içeri aktarma sırasında (portal ve API) hizmeti. Bu işlemler sırasında kullanılan verileri içerir:

- Kişi adı
- Telefon numarası
- Email
- Açık adres
- Şehir
- Posta kodu
- Durum
- Ülke/bölge/bölge
- Sürücü Kimliği
- Taşıyıcı hesap numarası
- Sevkiyat izleme numarası

İçeri/dışarı aktarma iş oluşturulduğunda, kullanıcılara kişi bilgileri ve teslimat adresi sağlar. Kişisel bilgileri, en fazla iki farklı konumlarda depolanır: İş ve isteğe bağlı olarak portal ayarları. Etiketli, onay kutusunu işaretleyin, kişisel bilgi portal ayarları yalnızca depolanan **taşıyıcı ve dönüş adresi varsayılanı olarak Kaydet** sırasında *dönüş Sevkiyat bilgisi* verme işlemi bölümü.

Kişisel iletişim bilgilerinizin aşağıdaki yollarla silinmiş:

- İşin kaydedilen verilerin işlemiyle silinir. Kullanıcılar işlerini el ile silebilir ve tamamlanan işler 90 gün sonra otomatik olarak silinir. İşlerini REST API veya Azure Portalı aracılığıyla el ile silebilirsiniz. Azure portalında işi silmek için içeri/dışarı aktarma iş'e gidin ve tıklatın *silmek* komut çubuğundan. İçeri/dışarı aktarma işini REST API aracılığıyla silme hakkında ayrıntılar için başvurmak [içeri/dışarı aktarma işini silmek](../articles/storage/common/storage-import-export-cancelling-and-deleting-jobs.md).

- Portal ayarlarını silerek portal ayarlarında kaydedilmiş kişi bilgileri kaldırılmış olabilir. Aşağıdaki adımları izleyerek portal ayarları silebilirsiniz:
  - [Azure Portal](https://portal.azure.com) oturum açın.
  - Tıklayın *ayarları* simgesi ![Azure ayarları simgesi](media/storage-import-export-delete-personal-info/azure-settings-icon.png)
  - Tıklatın *tüm ayarları dışarı* (geçerli ayarlarınızı kaydetmek için bir `.json` dosyası).
  - Tıklatın *tüm ayarları ve özel panolar silmek* kaydedilen kişi bilgileri de dahil olmak üzere tüm ayarlar silinecek.

Daha fazla bilgi için Microsoft Privacy İlkesi gözden [Güven Merkezi](https://www.microsoft.com/trustcenter)