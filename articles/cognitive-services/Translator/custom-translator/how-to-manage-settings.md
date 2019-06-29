---
title: Ayarlarını yönetmek nasıl? -Özel Translator
titleSuffix: Azure Cognitive Services
description: Ayarlarını yönetme, çalışma alanı oluşturun, çalışma alanı paylaşın ve özel Translator abonelik anahtarını yönetmek nasıl.
author: swmachan
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: d141b5dea8b0b12889559e6c80770379a6afac63
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448351"
---
# <a name="how-to-manage-settings"></a>Ayarları yönetme

İçinde özel Translator Ayarları sayfasında, yeni bir çalışma alanı oluşturun, çalışma alanınızda, paylaşma ve ekleyebilir veya Microsoft Translation abonelik anahtarınızı değiştirin.

Ayarlar sayfasına erişmek için:

1. Oturum [özel Translator](https://portal.customtranslator.azure.ai/) portalı.
2. Özel Translator portalında Kenar çubuğunda dişli simgesine tıklayın.

    ![Bağlantı ayarlama](media/how-to/how-to-settings.png)

## <a name="associating-microsoft-translator-subscription"></a>Microsoft Translator abonelik ilişkilendirme

Microsoft Translator metin çevirisi API'si, eğitmeniz veya dağıtmanız modelleri çalışma alanınızla ilişkili bir abonelik anahtarı olması gerekir.

Bir aboneliğiniz yoksa, aşağıdaki adımları izleyin:

1. Microsoft Translator metin API'si için abone olun. Bu makalede, Microsoft Translator Text API için abone olunacağı gösterilmektedir.
2. Translator aboneliğiniz için anahtarı not edin. Key1 ya da Key2 kabul edilir.
3. Özel Translator portalına gidin.

### <a name="add-existing-key"></a>Mevcut anahtarı Ekle

1.  Çalışma alanınız için "Ayarlar" sayfasına gidin.
2.  Anahtar Ekle seçeneğine tıklayın

    ![Abonelik anahtarı ekleme](media/how-to/how-to-add-subscription-key.png)

3. İletişim kutusunda, translator aboneliğiniz için anahtarı girin ve ardından "Ekle" düğmesine tıklayın.

    ![Abonelik anahtarı ekleme](media/how-to/how-to-add-subscription-key-dialog.png)
4.  Bir anahtarı ekledikten sonra değiştirmek veya herhangi bir zamanda anahtarı silin.

    ![Abonelik anahtarı Ekle](media/how-to/subscription-key-after-add.png)

## <a name="manage-your-workspace"></a>Çalışma alanınızı yönetme

Bir çalışma alanı oluşturma ve özel çeviri sisteminizi oluşturmak için bir çalışma alanıdır. Bir çalışma alanı, birden çok proje, modelleri ve belgeleri içerebilir.

Farklı bir iş parçası farklı kişilerle paylaşılması gerekiyorsa, sonra birden çok çalışma alanı oluşturmayı yararlı olabilir.

## <a name="create-a-new-workspace"></a>Yeni bir çalışma alanı oluşturma

1.  Çalışma alanı "Ayarlar" sayfasına gidin.
2.  "Yeni çalışma alanı" tıklayın "yeni çalışma alanı oluşturma" bölümünde düğmesi.

    ![Yeni çalışma alanı oluşturma](media/how-to/create-new-workspace.png)

4.  İletişim kutusunda, yeni bir çalışma alanı adını girin.
5.  "Oluştur" a tıklayın.

    ![Yeni çalışma alanı iletişim kutusu oluşturma](media/how-to/create-new-workspace-dialog.png)

## <a name="share-your-workspace"></a>Çalışma alanınızda paylaşın

Farklı bir iş parçası farklı kişilerle paylaşılması gerekiyorsa özel Translator, çalışma alanınızı başkalarıyla paylaşabilirsiniz.

1.  Çalışma alanı "Ayarlar" sayfasına gidin.
2.  "Paylaşımı ayarları" bölümünde "Paylaşım" düğmesine tıklayın.

    ![Çalışma alanı paylaşın](media/how-to/share-workspace.png)

3.  İletişim kutusunda şununla paylaşıldı: Bu çalışma alanını istediğiniz e-posta adreslerini virgülle ayrılmış bir listesini girin. Söz konusu kişinin e-posta adresiyle paylaştığınızdan emin olun özel Translator ile oturum açmak için kullanır. Ardından, uygun paylaşım izni düzeyini seçin.

4.  Çalışma alanınızı varsayılan adı "Çalışma Alanım" hala varsa, çalışma alanınızı paylaşmadan önce değiştirmeniz gerekir.
5.  "Kaydet" e tıklayın.

## <a name="sharing-permissions"></a>Paylaşım izinleri

1.  **Okuyucu:** Çalışma alanında bir okuyucu çalışma alanındaki tüm bilgileri görüntülemek mümkün olacaktır.

2.  **Düzenleyen:** Çalışma alanında bir düzenleyici belgelere ekleme, modelleri eğitme ve belgeler ve projeler silmek mümkün olacaktır. Bir abonelik anahtarı ekleyebilir ancak olamaz değiştirme ile çalışma alanını paylaşan, çalışma alanını silmek veya çalışma alanı adı değiştirin.

3.  **Sahibi:** Sahibi, çalışma alanına tam izinleri vardır.

## <a name="change-sharing-permission"></a>Paylaşım izni Değiştir

Bir çalışma alanı paylaşıldığında, bu çalışma alanı paylaşıldığı tüm e-posta adreslerini "Paylaşımı ayarları" bölümünde gösterilir. Çalışma alanına erişim sahibi olması durumunda her e-posta adresi izinlerini paylaşımı mevcut değiştirebilirsiniz.

1.  Her e-posta "Paylaşımı ayarları" bölümünde geçerli bir izin düzeyi bir açılan menü görüntüler.

2.  Açılan menüsüne tıklayın ve bu e-posta adresine atamak istediğiniz yeni izin düzeyini seçin.

    ![Paylaşım izni ayarları](media/how-to/sharing-permission-settings.png)

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi [çalışma ve projeyi geçirmek nasıl](how-to-migrate.md) gelen [Microsoft Translator Hub](https://hub.microsofttranslator.com)
