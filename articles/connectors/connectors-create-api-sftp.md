---
title: SFTP hesabınıza - Azure Logic Apps bağlanmak | Microsoft Docs
description: Görevleri ve izleme, oluşturma, yönetme, göndermek ve Azure Logic Apps'ı kullanarak bir SFTP sunucusuna SSH üzerinden dosyaları alma işlemleri otomatik hale getirin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: divswa, klam, LADocs
ms.assetid: 697eb8b0-4a66-40c7-be7b-6aa6b131c7ad
ms.topic: article
tags: connectors
ms.date: 10/26/2018
ms.openlocfilehash: 42e1ef3e311633f9631163bc9d3df212b608ef3a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60450769"
---
# <a name="monitor-create-and-manage-sftp-files-by-using-azure-logic-apps"></a>İzleme, oluşturma ve Azure Logic Apps kullanarak SFTP dosyalarını yönetme

İzleme, oluşturun, gönderin ve dosyaları alma görevlerini otomatikleştirmek için bir [güvenli dosya aktarım protokolünü (SFTP)](https://www.ssh.com/ssh/sftp/) sunucusu oluşturmak ve Azure Logic Apps ve SFTP Bağlayıcısı'nı kullanarak tümleştirmesi iş akışlarınızı otomatikleştirin. SFTP herhangi bir güvenilir veri akışı dosya erişimi, dosya aktarımı ve dosya yönetimi sağlayan bir ağ protokolüdür. Otomatik hale getirebilirsiniz bazı örnek görevler aşağıda verilmiştir: 

* Dosyaları eklenen veya değiştirilen zamana yönelik İzleyici.
* Al, oluştur, kopyalayın, listesinde güncelleştirmek ve dosyaları silin.
* Dosya içeriğini ve meta verileri alın.
* Arşivi klasöre ayıklayın.

SFTP sunucunuzdaki olayları izleyen ve çıkış diğer eylemler için kullanılabilir hale getirmek Tetikleyicileri kullanabilirsiniz. SFTP sunucunuzda çeşitli görevler gerçekleştiren eylemlerini kullanabilirsiniz. SFTP eylemleri çıktısını kullanan diğer eylemler mantıksal uygulamanızda da olabilir. Örneğin, düzenli olarak dosyaları SFTP sunucunuzdan almak, dosyaları ve içeriklerini hakkında e-posta uyarıları Office 365 Outlook Bağlayıcısı veya Outlook.com bağlayıcısını kullanarak gönderebilirsiniz.
Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md)

## <a name="limits"></a>Limits

* SFTP eylemleri okuma veya yazma dosyaları *50 MB veya daha küçük* kullanılmadıkça [ileti eylemleri Öbekleme](../logic-apps/logic-apps-handle-large-messages.md), hangi sağlar, bu sınırı aşan. Şu anda, SFTP Tetikleyicileri Öbekleme desteklemez.

* Dosyalar için *1 GB'a kadar*, kullanın [SFTP-SSH bağlayıcı](../connectors/connectors-sftp-ssh.md).

* Dosyalar için *1 GB'tan daha büyük*, SFTP-SSH bağlayıcı artı [ileti Öbekleme](../logic-apps/logic-apps-handle-large-messages.md).

SFTP Bağlayıcısı'nı ve SFTP-SSH Bağlayıcısı arasındaki diğer farklılıklardan için gözden [karşılaştırma SFTP-SSH ve SFTP](../connectors/connectors-sftp-ssh.md#comparison) SFTP-SSH makalede.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

* Mantıksal uygulamanızı SFTP hesabınıza erişmesine olanak sağlayan SFTP sunucu adresi ve hesap kimlik bilgilerinizi. Kullanılacak [güvenli Kabuk (SSH)](https://www.ssh.com/ssh/protocol/) protokolü, ayrıca erişim için SSH özel anahtarı ve SSH özel anahtar parolası gerekir. 

  > [!NOTE]
  > 
  > SFTP Bağlayıcısı'nı bu özel anahtar biçimlerini destekler: OpenSSH, ssh.com ve PuTTY
  > 
  > SFTP tetikleyici veya eylemi istediğiniz ekledikten sonra mantıksal uygulamanızı oluştururken, SFTP sunucunuzun bağlantı bilgilerini sağlamanız gerekir. 
  > SSH özel anahtarı kullanıyorsanız, emin ***kopyalama*** SSH özel anahtar dosyanıza anahtarından ve ***yapıştırın*** bağlantı ayrıntıları bu anahtara ***el ile girin yok veyaanahtarınıdüzenleyin***, bağlantı başarısız olmasına neden. 
  > Bu makalenin sonraki adımlarda daha fazla bilgi için bkz.

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* SFTP hesabınıza erişmek için istediğiniz mantıksal uygulaması. Bir SFTP tetikleyicisi ile başlatmak için [boş mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). SFTP eylem kullanmak için mantıksal uygulamanız başka bir tetikleyici ile örneğin başlayın, **yinelenme** tetikleyici.

## <a name="connect-to-sftp"></a>SFTP'ye bağlanma

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Oturum [Azure portalında](https://portal.azure.com)ve Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. Boş mantıksal uygulama için arama kutusuna filtreniz olarak "sftp" girin. Tetikleyiciler listesinde istediğiniz tetikleyicisini seçin. 

   veya

   Var olan mantıksal uygulamalar, son adım, bir eylem eklemek istediğiniz altında seçin için **yeni adım**. 
   Arama kutusuna filtreniz olarak "sftp" girin. 
   Eylemler listesinde, istediğiniz eylemi seçin.

   Adımlar arasında bir eylem eklemek için işaretçinizi adımlar arasındaki okun üzerine getirin. 
   Artı işaretini seçin ( **+** ), görünür ve ardından **Eylem Ekle**.

1. Bağlantınız için gerekli bilgileri sağlayın.

   > [!IMPORTANT] 
   >
   > SSH özel anahtarınızı girdiğinizde **SSH özel anahtarı** özelliği, bu özellik için değer tam ve doğru girdiğinizden emin olun yardımcı şu ek adımları izleyin. 
   > Geçersiz anahtar bağlantı başarısız olmasına neden olur.
   
   Herhangi bir metin düzenleyicisi kullanabilirsiniz, ancak doğru kopyalamak ve örnek olarak Notepad.exe kullanarak anahtarınızı yapıştırın işlemini gösteren örnek adımlar aşağıda verilmiştir.
    
   1. SSH özel anahtar dosyanıza bir metin düzenleyicisinde açın. 
   Bu adımlar örnek olarak Not Defteri'ni kullanabilirsiniz.

   1. Not Defteri'ni'nın üzerinde **Düzenle** menüsünde **Tümünü Seç**.

   1. Seçin **Düzenle** > **kopyalama**.

   1. SFTP tetikleyici veya eylemi eklediğiniz yapıştırın *tam* içine kopyaladığınız anahtar **SSH özel anahtarı** özelliği birden çok satır destekler. 
   ***Yapıştırdığınız emin*** anahtarı. ***El ile girmek yoksa veya anahtarı düzenlemek***.

1. Bitirdiğinizde bağlantı ayrıntıları girerek, seçin **Oluştur**.

1. Seçili tetikleyici veya eylem için gerekli bilgileri sağlayın ve mantıksal uygulamanızın iş akışı oluşturmaya devam edin.

## <a name="trigger-limits"></a>Tetikleyici sınırları

SFTP, SFTP dosya sistemi yoklama ve son yoklamadan bu yana değiştirilmiş her dosya için arayarak çalışma tetikler. Bazı araçlar, dosyaları değiştirdiğinizde, zaman damgası koruma sağlar. Bu gibi durumlarda, Tetikleyiciniz çalışabilmesi için bu özelliği devre dışı gerekir. Bazı ortak ayarları şunlardır:

| SFTP istemci | Eylem | 
|-------------|--------| 
| Winscp | Git **seçenekleri** > **tercihleri** > **aktarım** > **Düzenle**  >  **Korumak zaman damgası** > **devre dışı bırak** |
| FileZilla | Git **aktarım** > **korumak aktarılan dosyaların zaman damgaları** > **devre dışı bırak** | 
||| 

Tetikleyici yeni bir dosya bulduğunda tetikleyici, yeni dosya eksiksiz ve kısmen yazılmış olup olmadığını denetler. Örneğin, dosya sunucusu tetikleyici iade ederken bir dosya değişiklikleri sürüyor olabilir. Kısmen yazılı bir dosya döndürme önlemek için zaman damgası, son değişiklikler var, ancak bu dosyayı hemen döndürmüyor dosyası için tetikleyici notlar. Tetikleyici, yalnızca sunucu yeniden yoklanırken dosyayı döndürür. Bazı durumlarda, bu davranışı, iki kez tetikleyicinin kadar yoklama aralığı bir gecikmeye neden olabilir. 

Dosya içeriği isterken Tetikleyicileri 50 MB'tan büyük dosyaları uygulanmaz. 50 MB'tan büyük dosyaları almak için bu düzeni izleyin: 

* Dosya özellikleri gibi döndüren bir tetikleyici kullanmanız **dosya eklendiğinde veya değiştirildiğinde (yalnızca Özellikler)** .

* Tam dosya gibi okuyan bir eylemle tetikleyici izleyin **yolunu kullanarak dosya içeriğini Al**, ve kullanmak eyleme sahip [ileti Öbekleme](../logic-apps/logic-apps-handle-large-messages.md).

## <a name="examples"></a>Örnekler

<a name="file-add-modified"></a>

### <a name="sftp-trigger-when-a-file-is-added-or-modified"></a>SFTP tetikleyici: Dosya eklendiğinde veya değiştirildiğinde

Bir dosya eklendiğinde veya bir SFTP sunucu üzerinde değiştirilmiş bu tetikleyiciyi bir mantıksal uygulama iş akışı başlatır. Örneğin, dosyanın içeriğini denetler ve içerik belirtilen bir koşulu karşılayıp içeriği alan bir koşul ekleyebilirsiniz. Ardından, dosyanın içeriğini alır ve bu içeriği SFTP sunucusunda bir klasöre koyar, bir eylem ekleyebilirsiniz. 

**Kuruluş örnek**: Bu tetikleyici, bir müşteri siparişleri temsil eden yeni dosyalar için SFTP klasörü izlemek için kullanabilirsiniz. Ardından bir SFTP eylemi gibi kullanabilir **dosya içeriğini Al** daha ayrıntılı işleme için sipariş içeriklerini almak ve o sırada bir sipariş veritabanında depolayın.

Dosya içeriği isterken Tetikleyicileri 50 MB'tan büyük dosyaları uygulanmaz. 50 MB'tan büyük dosyaları almak için bu düzeni izleyin: 

* Dosya özellikleri gibi döndüren bir tetikleyici kullanmanız **dosya eklendiğinde veya değiştirildiğinde (yalnızca Özellikler)** .

* Tam dosya gibi okuyan bir eylemle tetikleyici izleyin **yolunu kullanarak dosya içeriğini Al**, ve kullanmak eyleme sahip [ileti Öbekleme](../logic-apps/logic-apps-handle-large-messages.md).

<a name="get-content"></a>

### <a name="sftp-action-get-content"></a>SFTP eylemi: İçerik alın

Bu işlem bir SFTP sunucusuna dosya içeriği alır. Örneğin, önceki örnekte ve dosyanın içeriğini karşılaması gereken bir koşul tetikleyici ekleyebilirsiniz. Koşul true ise, içeriği alır eylemi çalıştırabilirsiniz. 

Dosya içeriği isterken Tetikleyicileri 50 MB'tan büyük dosyaları uygulanmaz. 50 MB'tan büyük dosyaları almak için bu düzeni izleyin: 

* Dosya özellikleri gibi döndüren bir tetikleyici kullanmanız **dosya eklendiğinde veya değiştirildiğinde (yalnızca Özellikler)** .

* Tam dosya gibi okuyan bir eylemle tetikleyici izleyin **yolunu kullanarak dosya içeriğini Al**, ve kullanmak eyleme sahip [ileti Öbekleme](../logic-apps/logic-apps-handle-large-messages.md).

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları hakkında teknik ayrıntılar için bağlayıcının Openapı'nin açıklanmıştır (önceki adıyla Swagger) açıklama, bağlayıcının gözden [başvuru sayfası](/connectors/sftpconnector/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
