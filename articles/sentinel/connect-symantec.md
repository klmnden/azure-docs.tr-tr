---
title: Azure Önizleme Gözcü Symantec ICDX verilere | Microsoft Docs
description: Azure Gözcü için Symantec ICDX veri bağlanmayı öğreneceksiniz.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: d068223f-395e-46d6-bb94-7ca1afd3503c
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2019
ms.author: rkarlin
ms.openlocfilehash: 3b21371d6321b208b19ca8b2524308736c3ceca9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66244339"
---
# <a name="connect-your-symantec-icdx-appliance"></a>Symantec ICDX gerecinize bağlanma 

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Symantec ICDX Bağlayıcısı, Azure panoları görüntülemesine, özel uyarıları oluşturma ve araştırma geliştirmek için Gözcü ile tüm, Symantec güvenlik çözümü günlüklerini kolayca bağlanmanıza olanak sağlar. Bu, kuruluşunuzun ağ daha fazla öngörü sağlar ve güvenlik işlemi yeteneklerinizi geliştirir. REST API'si kullanın Symantec ICDX ve Azure Gözcü arasında tümleştirme sağlar.


> [!NOTE]
> Veriler Azure Gözcü çalıştırıyorsanız çalışma alanının coğrafi konumda depolanır.

## <a name="configure-and-connect-symantec-icdx"></a>Yapılandırma ve Symantec ICDX bağlanın 

Symantec ICDX, tümleştirme ve doğrudan Azure Gözcü için günlükleri dışarı aktarabilirsiniz.

1. Microsoft Azure (Log Analytics) Gözcü ileticiler eklemek için ICDX Yönetim Konsolu'nu açın.
2. ICDx gezinti çubuğunda Koruma'ya tıklayın **yapılandırma**. 
3. Üst kısmındaki **yapılandırma** ekranında **ileticileri**.
4. Altında **ileticileri**, Microsoft Azure Gözcü yanındaki (Log Analytics), tıklayın **Ekle**. 
4. İçinde **Microsoft Azure (Log Analytics) Gözcü** penceresinde tıklayın **Göster Gelişmiş**. 
5. Genişletilmiş üstündeki Microsoft Azure (Log Analytics) Gözcü penceresi için aşağıdakileri yapın:
    -   **Ad**: En fazla 30 karakterden ileticisi için bir ad yazın. Benzersiz ve anlamlı bir ad seçin. Bu ad iletici listede görünür **yapılandırma** ekran ve panolarda **Pano** ekran. Örneğin: Microsoft Azure Log Analytics Doğu. Bu alan gereklidir.
    -   **Açıklama**: İleticisi için bir açıklama yazın. Bu açıklama üzerinde ileticileri listesinde de görünür **yapılandırma** ekran. İletilen olay türü gibi ayrıntılar ve verileri incelemek için gereken grup içerir.
    -   **Başlangıç türü**: İletici yapılandırması için başlatma yöntemini seçin. Seçenekleriniz şunlardır: elle ve otomatik.<br>Otomatik varsayılandır. 
6. Altında **olayları**, aşağıdakileri yapın: 
    - **Kaynak**: Bir veya daha fazla arşivleri olayları iletmek üzere seçin. Yalnız bırakılmış (ortak arşiv dahil), etkin Toplayıcı arşivleri seçebilirsiniz (diğer bir deyişle, arşivleri sildiğiniz toplayıcıları için) Toplayıcı arşivleri, ICDx alıcı arşivleri veya sistem arşiv. <br>Genel arşiv varsayılandır.
      > [!NOTE]
      > ICDx alıcı arşivleri adına göre ayrı olarak listelenir. 
 
    - **Filtre**: Alt olayları iletmeye kümesini belirten bir filtre ekleyin. Aşağıdakilerden birini yapın:
        - Bir filtre koşulu seçmek için türü, öznitelik, işleç ve değer tıklayın. 
        - Filtre alanda, filtre koşulu gözden geçirin. Alan doğrudan düzenlemek veya gerektiği şekilde silin.
        - ' A tıklayın ve veya veya, filtre koşulu eklemek.
        - Kayıtlı bir sorguyu uygulamak için kaydedilmiş sorgular da tıklayabilirsiniz.
    - **Öznitelikler dahil**: İletilen verilerin dahil edileceği öznitelikleri virgülle ayrılmış listesini yazın. Öznitelikler dahil edildi dışlanan öznitelikleri önceliklidir.
    - **Öznitelikler dışarıda**: İletilen verileri dışlamak için öznitelik virgülle ayrılmış listesini yazın.
    - **Yığın boyutu**: Toplu iş göndermek için olay sayısını seçin. Seçenekleriniz şunlardır: 10, 50, 100, 500 ve 1000.<br>Varsayılan değer 100'dür. 
    - **Hız sınırı**: Hangi olayları, saniyede olarak ifade edilen iletilen hızını seçin. Sınırsız, 500, 1000 Seçenekleriniz şunlardır 5000, 10000. <br> Varsayılan değer 5000'dir. 
7. Altında **Azure hedef**, aşağıdakileri yapın: 
    - **Çalışma alanı kimliği**: Çalışma alanı kimliği aşağıdan yapıştırın. Bu alan gereklidir.
    - **Birincil anahtar**: Birincil anahtarı aşağıdan yapıştırın. Bu alan gereklidir.
    - **Özel günlük adı**: Olayları giderek Microsoft Azure portal Log Analytics çalışma alanında, özel günlük adı yazın. SymantecICDx varsayılandır. Bu alan gereklidir.
8. Tıklayın *Kaydet* ileticisi yapılandırmayı tamamlayın. 
9. Altında ileticisi'ni başlatmak için **seçenekleri**, tıklayın **daha fazla** ardından **Başlat**.
10. İlgili şema için Symantec ICDX olayları Log Analytics'te kullanmak için arama **SymantecICDX_CL**.


## <a name="validate-connectivity"></a>Bağlantıyı doğrula

Çalınıyor Log Analytics'te görünmesini günlüklerinizi başlatana kadar 20 dakika sürebilir. 



## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Gözcü için Symantec ICDX bağlanma hakkında bilgi edindiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).

