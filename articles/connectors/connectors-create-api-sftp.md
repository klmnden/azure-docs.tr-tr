---
title: Azure Logic Apps'ten SFTP hesabınıza bağlanın | Microsoft Docs
description: Görevler ve izleme, oluşturma, yönetme, göndermek ve Azure Logic Apps kullanarak dosya bir SFTP sunucusu için almak iş akışlarını otomatikleştirin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: 697eb8b0-4a66-40c7-be7b-6aa6b131c7ad
ms.topic: article
tags: connectors
ms.date: 08/24/2018
ms.openlocfilehash: 8f430477883543aa8f87eb3fb0fb49ab31e2d723
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43042047"
---
# <a name="monitor-create-and-manage-sftp-files-by-using-azure-logic-apps"></a>İzleme, oluşturma ve Azure Logic Apps kullanarak SFTP dosyalarını yönetme

Azure Logic Apps ve SFTP Bağlayıcısı ile otomatik görevler ve iş akışlarını izlemek, oluşturun, gönderin ve hesabınız üzerinden dosyaları alma oluşturabilirsiniz bir [SFTP](https://www.ssh.com/ssh/sftp/) örneğin diğer eylemlerin yanı sıra sunucu:

* Dosyaları eklenen veya değiştirilen zamana yönelik İzleyici.
* Al, oluştur, kopyalayın, listesinde güncelleştirmek ve dosyaları silin.
* Dosya içeriğini ve meta verileri alın.
* Arşivi klasöre ayıklayın.

SFTP sunucunuzdan yanıtlar almak ve çıkış diğer eylemler için kullanılabilir Tetikleyicileri kullanabilirsiniz. Logic apps eylemleri, dosyaları SFTP sunucunuzdaki görevleri gerçekleştirmek için kullanabilirsiniz. Ayrıca SFTP eylemleri çıktısını kullanan diğer eylemler olabilir. Örneğin, düzenli olarak dosyaları SFTP sunucunuzdan almak, Office 365 Outlook Bağlayıcısı veya Outlook.com Bağlayıcısı'nı kullanarak bu dosyaları ve bunların içeriğini hakkında e-posta gönderebilirsiniz.
Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

* SFTP konak sunucu adresi ve hesap kimlik bilgilerinizi

   Mantıksal uygulamanızı bir bağlantı oluşturun ve SFTP hesabınıza erişmek için kimlik bilgilerinizi yetkilendirin.

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* SFTP hesabınıza erişmek için istediğiniz mantıksal uygulaması. Bir SFTP tetikleyicisi ile başlatmak için [boş mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). SFTP eylem kullanmak için mantıksal uygulamanız başka bir tetikleyici ile örneğin başlayın, **yinelenme** tetikleyici.

## <a name="connect-to-sftp"></a>SFTP'ye bağlanma

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Oturum [Azure portalında](https://portal.azure.com)ve Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. Boş mantıksal uygulama için arama kutusuna filtreniz olarak "sftp" girin. Tetikleyiciler listesinde istediğiniz tetikleyicisini seçin. 

   -veya-

   Var olan mantıksal uygulamalar, son adım, bir eylem eklemek istediğiniz altında seçin için **yeni adım**. 
   Arama kutusuna filtreniz olarak "sftp" girin. 
   Eylemler listesinde, istediğiniz eylemi seçin.

   Adımlar arasında bir eylem eklemek için işaretçinizi adımlar arasındaki okun üzerine getirin. 
   Artı işaretini seçin (**+**), görünür ve ardından **Eylem Ekle**.

1. Bağlantınız için gerekli bilgileri sağlayın ve ardından **Oluştur**.

1. Seçili tetikleyici veya eylem için gerekli bilgileri sağlayın ve mantıksal uygulamanızın iş akışı oluşturmaya devam edin.

## <a name="examples"></a>Örnekler

### <a name="sftp-trigger-when-a-file-is-added-or-modified"></a>SFTP tetikleyici: ne zaman bir dosya eklendiğinde veya değiştirildiğinde

Bir dosya eklendiğinde veya bir SFTP sunucu üzerinde değiştirilmiş tetikleyici algıladığında, bu tetikleyiciyi bir mantıksal uygulama iş akışı başlatır. Örneğin, dosyanın içeriğini denetler ve söz konusu içeriği almak etkinleştirilip etkinleştirilmeyeceğini karar bir koşul ekleyebilirsiniz içeriğin belirtilen bir koşulu karşılayıp temel. Son olarak, dosyanın içeriğini alır bir eylem ekleme ve içeriği SFTP sunucusunda bir klasöre yerleştirin. 

**Kuruluş örnek**: Bu tetikleyici, müşteri siparişleri temsil eden yeni dosyalar için bir SFTP klasörü izlemek için kullanabilirsiniz. Ardından bir SFTP eylemi gibi kullanabilir **dosya içeriğini Al**, böylece daha ayrıntılı işleme için sipariş içeriklerini almak ve o sırada bir sipariş veritabanında depolayın.

### <a name="sftp-action-get-content"></a>SFTP eylemi: içerik alma

Bu işlem bir SFTP sunucusuna dosya içeriği alır. Örneğin, önceki örnekte ve dosyanın içeriğini karşılaması gereken bir koşul tetikleyici ekleyebilirsiniz. Koşul true ise, içeriği alır eylemi çalıştırabilirsiniz. 

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları hakkında teknik ayrıntılar için bağlayıcının Openapı'nin açıklanmıştır (önceki adıyla Swagger) açıklama, bağlayıcının gözden [başvuru sayfası](/connectors/sftpconnector/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](http://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)