---
title: Gözcü Azure Önizleme için Windows Güvenlik Olay verileri bağlayın | Microsoft Docs
description: Azure Gözcü için Windows Güvenlik Olay verileri bağlanmayı öğreneceksiniz.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: d51d2e09-a073-41c8-b396-91d60b057e6a
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 88b066818d53fd92e8238e270b9bc785d4275186
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65233985"
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

1. Gözcü Azure portalında **veri bağlayıcıları** ve ardından **Windows Güvenlik olayları** Döşe. 
1. Akışı yapmak istediğiniz veri türlerini seçin.
1. Tıklayın **güncelleştirme**.
6. İlgili şema için Windows Güvenlik olaylarını Log Analytics'e kullanmak için arama **SecurityEvent**.

## <a name="validate-connectivity"></a>Bağlantıyı doğrula

Bu, Log Analytics'te görünmesini günlüklerinizi başlatana kadar yaklaşık 20 dakika sürebilir. 



## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Gözcü için Windows Güvenlik olayları bağlanma öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).

