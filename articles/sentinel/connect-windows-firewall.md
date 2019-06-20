---
title: Windows Güvenlik Duvarı verilere Azure Önizleme Gözcü | Microsoft Docs
description: Azure Gözcü için Windows Güvenlik Duvarı veri bağlanmayı öğreneceksiniz.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 0e41f896-8521-49b8-a244-71c78d469bc3
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2019
ms.author: rkarlin
ms.openlocfilehash: 09e63612d6e0e70b1bb21c23b158f650d4c34080
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190594"
---
# <a name="connect-windows-firewall"></a>Windows güvenlik duvarını bağlama

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Windows Güvenlik Duvarı Bağlayıcısı'nı Azure Gözcü çalışma alanınıza bağlıysa, Windows Güvenlik duvarı günlükleri kolayca bağlanmanızı sağlar. Bu bağlantı, panoları görüntülemesine, özel uyarıları oluşturma ve araştırma geliştirmek sağlar. Bu, kuruluşunuzun ağ daha fazla öngörü sağlar ve güvenlik işlemi yeteneklerinizi geliştirir. Çözüm, Log Analytics aracısını yüklü olduğu Windows makinelerden Windows Güvenlik Duvarı olaylarını toplar. 


> [!NOTE]
> Veriler Azure Gözcü çalıştırıyorsanız çalışma alanının coğrafi konumda depolanır.

## <a name="enable-the-connector"></a>Bağlayıcıyı etkinleştir 

1. Gözcü Azure portalında **veri bağlayıcıları** ve ardından **Windows Güvenlik Duvarı** Döşe. 
1.  Azure'da Windows makineleriniz varsa:
    1. Tıklayın **Azure Windows sanal makine üzerinde aracı yükleme**.
    1. İçinde **sanal makineler** listesinde, istediğiniz Azure Gözcü akışını sağlamak için Windows makine seçin. Bu bir Windows VM olduğundan emin olun.
    1. Bu VM için açılır pencerede **Connect**.  
    1. Tıklayın **etkinleştirme** içinde **Windows Güvenlik Duvarı bağlayıcı** penceresi. 

2. Windows makinenizi Azure VM'deki değilse:
    1. Tıklayın **Azure olmayan makineler aracı yükleme**.
    1. İçinde **doğrudan aracı** penceresinde seçin **indirme Windows aracısını (64 bit)** veya **indirme Windows aracısını (32 bit)** .
    1. Windows makinenizde aracıyı yükleyin. Kopyalama **çalışma alanı kimliği**, **birincil anahtar**, ve **ikincil anahtar** ve yükleme sırasında istendiğinde kullanabilirsiniz.

4. Akışı yapmak istediğiniz veri türlerini seçin.
5. Tıklayın **yükleme çözümü**.
6. İlgili şema Log Analytics'te için Windows Güvenlik Duvarı'nı kullanmak için arama **SecurityEvent**.

## <a name="validate-connectivity"></a>Bağlantıyı doğrula

Çalınıyor Log Analytics'te görünmesini günlüklerinizi başlatana kadar 20 dakika sürebilir. 



## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Gözcü için Windows Güvenlik Duvarı bağlantı öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).

