---
title: Azure portal ayarlarını silin veya dışarı | Microsoft Docs
description: Dışarı aktarma veya silebilir, kullanıcı ayarları, özel panolar ve özel ayarları Azure portalında nasıl öğrenin.
services: azure-portal
keywords: ''
author: santhoshsomayajula
ms.date: 04/08/2019
ms.topic: conceptual
ms.service: azure-portal
ms.custom: ''
manager: mtillman
ms.author: kfollis
ms.openlocfilehash: fde7ffbaa3ef4d47eea48302a99948932aeb4f00
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60551676"
---
# <a name="export-or-delete-user-settings"></a>Kullanıcı ayarlarını dışarı aktarma veya silme

Özel bir deneyim oluşturmak için Azure portalında ayarları ve özellikleri kullanabilirsiniz. Özel ayarlarınızla ilgili bilgiler, Azure'da depolanır. Dışarı aktarma veya aşağıdaki kullanıcı verilerini silin:

* Azure portalında özel panolar
* Sık kullanılan abonelikleri veya dizinleri ve son oturum açtığınız dizin gibi kullanıcı ayarları
* Temalar ve diğer özel portal ayarları

Silmeden önce ayarlarınızı gözden geçirin ve dışarı aktarmak iyi bir fikirdir. Pano yeniden veya özel ayarlar yineleme zaman alıcı olabilir.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="export-or-delete-your-portal-settings"></a>Dışarı aktarma veya portal ayarlarınızı Sil

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Portalın üst bilgisindeki seçin **ayarları**.

    ![Portal ayarlar dişli simgesini gösteren ekran görüntüsü](media/azure-portal-export-delete-settings/azure-portal-settings-icon.png)

3. Seçin **tüm ayarları dışarı aktar** veya **tüm ayarları ve özel panoları Sil**.

    ![Portal gösteren ekran görüntüsü dışarı aktarma ve ayarları Sil](media/azure-portal-export-delete-settings/azure-portal-export-delete-settings.png)

      Aşağıdaki tabloda, bu eylemleri açıklar.

      | Eylem | Açıklama |
      | --- | --- |
      | **Tüm ayarları Dışarı Aktar** | Renk teması, Sık Kullanılanlar ve özel panolar gibi kullanıcı ayarlarınızı içeren bir .json dosyası oluşturur.|
      | **Tüm ayarları ve özel panoları Sil** | Portala yaptığınız diğer özel ayarları ve özel panoları tüm bağlantıları siler. |

> [!NOTE]
> Kullanıcı ayarları dinamik doğasını ve verilerin bozulma riskini nedeniyle .json dosyasından ayarlar içeri aktarılamıyor.
>
>


## <a name="next-steps"></a>Sonraki adımlar

* [Azure panoları oluşturma ve paylaşma](azure-portal-dashboard-share-access.md)
* [Ekleme, kaldırma ve sıralama Sık Kullanılanlar](azure-portal-add-remove-sort-favorites.md)
