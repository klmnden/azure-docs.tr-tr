---
title: Gözcü Azure önizlemesinde Windows Güvenlik olay verilerini topla | Microsoft Docs
description: Azure Gözcü içindeki Windows güvenlik olayı verilerini nasıl toplayacağınızı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: d51d2e09-a073-41c8-b396-91d60b057e6a
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 3c79747bf33e1769af5f8d3589904ba15105f216
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58087610"
---
# <a name="connect-windows-security-events"></a>Windows güvenlik olaylarını bağlama 

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Tüm güvenlik olayları Azure Gözcü çalışma alanınıza bağlı Windows sunucularından akışını yapabilirsiniz. Bu bağlantı, panoları görüntülemesine, özel uyarıları oluşturma ve araştırma geliştirmek sağlar. Bu, kuruluşunuzun ağ daha fazla öngörü sağlar ve güvenlik işlemi yeteneklerinizi geliştirir.  Hangi olayların akışı seçebilirsiniz:

- **Tüm olaylar** -tüm Windows Güvenlik ve AppLocker olayları.
- **Ortak** -olayları denetim amacıyla standart bir dizi.
- **En az** -küçük bir olayların olası tehditleri gösterebilir. Bu seçenek etkinleştirildiğinde, tam denetim kaydı mümkün olmayacaktır.
- **Hiçbiri** -hiçbir güvenlik veya AppLocker olayı.

> [!NOTE]
> 
> - Veriler Azure Gözcü çalıştırıyorsanız çalışma alanının coğrafi konumda depolanır.

## <a name="set-up-the-windows-security-events-connector"></a>Windows Güvenlik olayları Bağlayıcısı'nı Ayarla

Tam olarak Windows Güvenlik olaylarınız Gözcü Azure ile tümleştirmek için:

1. Gözcü Azure portalında **veri toplama** ve ardından **Windows Güvenlik olayları** Döşe. 
1. Akışı yapmak istediğiniz veri türlerini seçin.
1. Tıklayın **güncelleştirme**.


## <a name="validate-connectivity"></a>Bağlantıyı doğrula

Bu, Log Analytics'te görünmesini günlüklerinizi başlatana kadar yaklaşık 20 dakika sürebilir. 



## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Gözcü için Windows Güvenlik olayları bağlanma öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).

