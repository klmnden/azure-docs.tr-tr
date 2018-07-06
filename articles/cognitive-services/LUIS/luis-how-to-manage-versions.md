---
title: Azure'da LUIS uygulama sürümlerinde yönetme | Microsoft Docs
description: Language Understanding (LUIS) uygulamaların sürümlerinde yönetmeyi öğrenin.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/29/2017
ms.author: v-geberr
ms.openlocfilehash: ef5dadd94d3612500d3092bdbd601fdaa12d1701
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37868037"
---
# <a name="manage-versions"></a>Sürümleri yönetme

Farklı bir model üzerinde çalıştığınız her zaman Oluştur [sürüm](luis-concept-version.md) uygulama. 

## <a name="set-active-version"></a>Etkin sürümü Ayarla
Sürümlerle çalışma için uygulamanızın adını seçerek açın **uygulamalarım** sayfasında ve ardından **ayarları** üst çubuktaki.

![Sürümleri sayfası](./media/luis-how-to-manage-versions/settings.png)

**Ayarları** sayfası sürümleri ve ortak çalışanlar dahil olmak üzere tüm uygulama için ayarları yapılandırmanıza olanak tanır. 

## <a name="clone-a-version"></a>Sürümü Kopyala
1. Üzerinde **ayarları** sayfasında, sonra uygulama ayarları ve ortak çalışanlar bölümleri, kopyalamak istediğiniz sürümü satırı bulun. Öğesinin üç noktasını (***...*** ) sağda düğmesi. 

    ![Sürüm satır özellikleri](./media/luis-how-to-manage-versions/version-section.png)

2. Seçin **kopya** listeden.

    ![Sürüm satır özellikleri seçim](./media/luis-how-to-manage-versions/version-three-dots-modal.png)

3. İçinde **kopya sürüm** iletişim kutusunda, "0.2" gibi yeni sürümü için bir ad yazın.

   ![Kopya sürüm iletişim kutusu](./media/luis-how-to-manage-versions/version-clone-version-dialog.png)
 
 > [!NOTE]
 > Sürüm kimliği, yalnızca karakterlerinden basamak oluşabilir veya '.' ve 10 karakterden uzun olamaz.
 
 Belirtilen ada sahip yeni bir sürüm oluşturulur ve etkin bir sürüm olarak ayarlayın.
 
  ![Sürümü oluşturulan ve listeye eklendi](./media/luis-how-to-manage-versions/new-version.png)

 > [!NOTE]
 > Önceki görüntüde gösterildiği gibi yayımlanmış bir sürüm burada da yayımlandıysa yuvası türünü gösteren renkli işareti ile ilişkilidir: üretim (yeşil), hazırlama (kırmızı) ve her ikisi de (siyah). Eğitim ve yayımlama tarihler için yayımlanan her sürümü görüntülenir.

## <a name="set-active-version"></a>Etkin sürümü Ayarla
1. Üzerinde **ayarları** sayfasında **sürümleri** listesinde, üç noktayı seçin (***...*** ) en sağdaki düğme.

2. Açılır listesinden **etkin olarak ayarla**.

    ![Etkin sürümü Ayarla](./media/luis-how-to-manage-versions/set-active-version.png)

    Etkin sürüm açık mavi rengi, aşağıdaki ekran görüntüsünde gösterildiği gibi vurgulanır:

    ![Etkin sürümü](./media/luis-how-to-manage-versions/set-active-version-done.png) 


## <a name="import-version"></a>Alma sürümü
Bir JSON dosyasından bir sürümü içeri aktarabilirsiniz. Bir sürümü içeri aktardığınızda, yeni sürümü etkin sürümü haline gelir.

**Bir sürümü içeri aktarmak için:**

1. Üzerinde **ayarları** sayfasında **yeni sürüm içeri aktarma** düğmesi.

    ![İçeri Aktar düğmesi](./media/luis-how-to-manage-versions/import-version.png)

2. Seçin **Gözat** ve JSON dosyasını seçin.

    ![Sürüm iletişim kutusu İçeri Aktar](./media/luis-how-to-manage-versions/import-version-dialog.png)

JSON dosyasındaki sürüm uygulamada zaten varsa, bir sürüm kimliği ayarlamak yeterlidir.

## <a name="export-version"></a>Dışarı aktarma sürümü
Bir sürüm JSON dosyasını dışarı aktarabilirsiniz.

**Bir sürüm dışarı aktarmak için:**

1. Üzerinde **ayarları** sayfasında **sürümleri** listesinde, üç noktayı seçin (***...*** ) en sağdaki düğme.

2. Seçin **dışarı** açılır listesinde, Eylemler ve dosyayı kaydetmek istediğiniz yeri seçin.

## <a name="delete-a-version"></a>Bir sürüm Sil
Sürümleri silebilirsiniz ancak uygulamayı en az bir sürümünü saklamak gerekir. Etkin sürümü hariç tüm sürümler silebilirsiniz. 

1. Üzerinde **ayarları** sayfasında **sürümleri** listesinde, üç noktayı seçin (***...*** ) en sağdaki düğme.

2. Seçin **Sil** açılır listesinde, Eylemler ve dosyayı kaydetmek istediğiniz yeri seçin.

    ![Sürüm onayını Sil](./media/luis-how-to-manage-versions/delete-menu.png) 


## <a name="rename-a-version"></a>Bir sürüm yeniden adlandır
Sürüm adı zaten kullanımda olup olmadığını sürece sürümleri yeniden adlandırabilirsiniz.  

1. Üzerinde **ayarları** sayfasında **sürümleri** listesinde, üç noktayı seçin (***...*** ) en sağdaki düğme.

2. Seçin **Yeniden Adlandır** eylemleri açılır listesinde.

3. Yeni bir sürüm adı girin ve seçin **Bitti**.

    ![Sürüm onayı yeniden adlandır](./media/luis-how-to-manage-versions/rename-popup.png) 
