---
title: Özel markdown kutucuğu üzerinde Azure panolarını kullanın
description: Markdown kutucuğu statik içeriği görüntülemek için bir Azure panosunu eklemeyi öğrenin
services: azure-portal
keywords: ''
author: kfollis
ms.author: kfollis
ms.date: 01/25/2019
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: ec8cbddda4137656a53fd4968c451cd413959274
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60551605"
---
# <a name="use-a-markdown-tile-on-azure-dashboards-to-show-custom-content"></a>Markdown kutucuğu, özel içerik göstermek için Azure panolarında kullanın.

Markdown kutucuğu özel, statik içeriği görüntülemek için Azure panolarınıza ekleyebilirsiniz. Örneğin, temel yönergeler, bir resim veya köprüler markdown kutucuğu içeren bir dizi gösterebilirsiniz.

## <a name="add-a-markdown-tile-to-your-dashboard"></a>Markdown kutucuğunu panonuza ekleme

1. Seçin **Pano** Azure portal kenar. Tüm özel panoları Pano görünümünde oluşturduysanız, aşağı açılan Pano seçmek için özel markdown kutucuğu burada görünmesi gereken kullanın. Düzenle simgesini seçerek açın **kutucuk Galerisi**.

   ![Ekran gösterme Pano düzenleme görünümü](./media/azure-portal-markdown-tile/azure-portal-dashboard-edit.png)

2. İçinde **kutucuk Galerisi**, adlı kutucuğu bulun **Markdown** tıklatıp **Ekle**. Panoya bir kutucuk eklenir ve **Düzenle Markdown** bölmesi açılır.

1. Düzen **başlık**, **alt konu başlığını**, ve **içerik** kutucuğu özelleştirmek için alanları. Burada gösterilen örnekte, özel bir Yardım Masası bilgileri görüntülemek için markdown kutucuğu düzenlendi.

   ![Markdown kutucuğunu düzenleme Görünümü'nü gösteren ekran görüntüsü](./media/azure-portal-markdown-tile/azure-portal-edit-markdown-tile.png)

4. Seçin **Bitti** kapatmak için **Düzenle Markdown** bölmesi. İçeriğinizi ardından sağ alt köşede tutamacı sürükleyerek boyutlandırılabilir Markdown kutucuğu görüntülenir.

   ![Ekran gösteren özel markdown kutucuğu](./media/azure-portal-markdown-tile/azure-portal-custom-markdown-tile.png)

## <a name="markdown-content-capabilities-and-limitations"></a>Markdown içerik özellikleri ve sınırlamaları

Markdown kutucuğa düz metin, Markdown söz dizimi ve HTML içeriğini herhangi bir birleşimini kullanabilirsiniz. Azure portalında adlı bir açık kaynak kitaplığı kullanan _işaretlenmiş_ kutucukta gösterilen HTML içeriğinizi dönüştürmek için. HTML tarafından üretilen _işaretlenmiş_ oluşturulmadan önce önceden portal tarafından işlenir. Bu adım güvenlik veya portalının düzenini özelleştirme etkilenmeyeceğinden emin emin olmaya yardımcı olur. Bu ön işleme sırasında herhangi bir bölümünü olası tehdidi HTML kaldırılır. Aşağıdaki içerik türlerini portal tarafından izin verilmeyen:

* JavaScript – `<script>` etiketleri ve satır içi JavaScript değerlendirmeleri kaldırılacak.
* iframe örnekleri - `<iframe>` etiketleri kaldırılır.
* Style - `<style>` etiketleri kaldırılır. Satır içi stil öznitelikleri HTML öğelerinde resmi olarak desteklenmez. Bazı satır içi stil öğelerini çalıştığınız, ancak bunlar portalının Düzen müdahale, bunlar herhangi bir zamanda çalışmayı durdurabilir bulabilirsiniz. Markdown kutucuğunu portal'ın varsayılan stillerini kullanan temel, statik içerik için tasarlanmıştır.

## <a name="next-steps"></a>Sonraki adımlar

* Özel bir Pano oluşturmak için bkz [Azure portalında panolarını oluşturma ve paylaşma](../azure-portal/azure-portal-dashboards.md)
