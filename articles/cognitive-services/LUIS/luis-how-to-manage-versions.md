---
title: Azure HALUK uygulamalarında sürümlerde yönetme | Microsoft Docs
description: Dil anlama (HALUK) uygulamaları sürümlerde yönetmeyi öğrenin.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/29/2017
ms.author: v-geberr
ms.openlocfilehash: 672f7991be0fc236e39daf7d1ce1d6080b31815b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351701"
---
# <a name="manage-versions"></a>Sürümleri yönetme

Farklı bir model üzerinde çalıştığınız her zaman oluşturma [sürüm](luis-concept-version.md) uygulamanın. 

## <a name="set-active-version"></a>Set etkin sürüm
Sürümleri ile çalışması için uygulamanızın üzerinde adını seçerek açın **My uygulamaları** sayfasında ve ardından **ayarları** üst çubuğunda.

![Sürümleri sayfası](./media/luis-how-to-manage-versions/settings.png)

**Ayarları** sayfası sürümleri ve ortak çalışanlar dahil olmak üzere tüm uygulama için ayarları yapılandırmanıza olanak sağlar. 

## <a name="clone-a-version"></a>Bir sürüm kopyalama
1. Üzerinde **ayarları** sayfasında, uygulama ayarları ve ortak çalışanlar bölümlerinden sonra bulma kopyalamak istediğiniz sürüm satırı. Şimdiye kadar sağındaki üç nokta (...) seçin. 

    ![Sürüm satır özellikleri](./media/luis-how-to-manage-versions/version-section.png)

2. Seçin **kopya** listeden.

    ![Sürüm satır özellikleri seçimi](./media/luis-how-to-manage-versions/version-three-dots-modal.png)

3. İçinde **kopya sürüm** iletişim kutusu, "0.2" gibi yeni sürümü için bir ad yazın.

   ![Kopya sürüm iletişim kutusu](./media/luis-how-to-manage-versions/version-clone-version-dialog.png)
 
 > [!NOTE]
 > Sürüm kimliği yalnızca karakter rakam oluşabilir veya '.' ve 10 karakterden uzun olamaz.
 
 Belirtilen ada sahip yeni bir sürüm oluşturulur ve etkin sürüm olarak ayarlayın.
 
  ![Sürüm oluşturulur ve listeye eklenir](./media/luis-how-to-manage-versions/new-version.png)

 > [!NOTE]
 > Önceki görüntüde gösterildiği gibi yayımlanmış bir sürüm burada da yayınlandı yuvası türünü gösteren bir renkli işareti ile ilişkilidir: üretim (yeşil), hazırlama (kırmızı) ve her ikisi de (siyah). Eğitim ve yayımlama tarihleri, yayımlanmış her sürüm için görüntülenir.

## <a name="set-active-version"></a>Set etkin sürüm
1. Üzerinde **ayarları** sayfasında **sürümleri** listesinde, sağ uçta üç nokta (...) seçin.

2. Açılır listeden seçin **etkin olarak ayarlanmış**.

    ![Set etkin sürüm](./media/luis-how-to-manage-versions/set-active-version.png)

    Etkin sürüm, aşağıdaki ekran görüntüsünde gösterildiği gibi açık bir mavi renk tarafından vurgulanmıştır:

    ![Etkin sürüm](./media/luis-how-to-manage-versions/set-active-version-done.png) 


## <a name="import-version"></a>İçeri aktarma sürümü
Bir JSON dosyasından bir sürümünü içe aktarabilirsiniz. Bir kez bir sürümünü içe aktardıktan sonra yeni sürümü etkin sürüm haline gelir.

**Bir sürümünü almak için:**

1. Üzerinde **ayarları** sayfasında, **alma yeni sürümü** düğmesi.

    ![İçeri Aktar düğmesi](./media/luis-how-to-manage-versions/import-version.png)

2. Seçin **Gözat** ve JSON dosyası seçin.

    ![İçeri aktarma sürüm iletişim](./media/luis-how-to-manage-versions/import-version-dialog.png)

Yalnızca JSON dosyası sürümünde uygulamada zaten varsa, sürüm kimliği ayarlamanız gerekir.

## <a name="export-version"></a>Sürüm dışarı aktarma
Bir JSON dosyası için bir sürüm dışarı aktarabilirsiniz.

**Bir sürüm dışarı aktarmak için:**

1. Üzerinde **ayarları** sayfasında **sürümleri** listesinde, sağ uçta üç nokta (...) seçin.

2. Seçin **verme** açılır listesinde, eylemleri ve dosyayı kaydetmek istediğiniz yeri seçin.

## <a name="delete-a-version"></a>Bir sürümü Sil
Sürümleri silebilir, ancak en az bir uygulamanın sürümünü tutmam gerekiyor. Etkin sürüm hariç tüm sürümleri silebilirsiniz. 

1. Üzerinde **ayarları** sayfasında **sürümleri** listesinde, sağ uçta üç nokta (...) seçin.

2. Seçin **silmek** açılır listesinde, eylemleri ve dosyayı kaydetmek istediğiniz yeri seçin.

    ![Sürüm onayını Sil](./media/luis-how-to-manage-versions/delete-menu.png) 


## <a name="rename-a-version"></a>Bir sürümünü yeniden adlandırma
Sürüm adı zaten kullanımda olup olmadığını sürece sürümleri yeniden adlandırabilirsiniz.  

1. Üzerinde **ayarları** sayfasında **sürümleri** listesinde, sağ uçta üç nokta (...) seçin.

2. Seçin **yeniden adlandırma** Eylemler açılır listesinde.

3. Yeni bir sürüm adı girin ve **Bitti**.

    ![Sürüm onayı yeniden adlandırma](./media/luis-how-to-manage-versions/rename-popup.png) 
