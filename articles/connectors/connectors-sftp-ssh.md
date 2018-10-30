---
title: Azure Logic Apps'ten SFTP hesabınıza bağlanın | Microsoft Docs
description: Görevler ve izleme, oluşturma, yönetme, göndermek ve Azure Logic Apps kullanarak dosya bir SFTP sunucusu için almak iş akışlarını otomatikleştirin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
tags: connectors
ms.date: 09/24/2018
ms.openlocfilehash: 2250c6952aeac7b10dcb1a1a35419941e5cad507
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50233217"
---
# <a name="monitor-create-and-manage-sftp-files-by-using-azure-logic-apps-and-sftp-ssh-connector"></a>İzleme, oluşturma ve SFTP dosyalarını Azure Logic Apps ve SFTP-SSH Bağlayıcısı'nı kullanarak yönetme

Azure Logic Apps ve SFTP-SSH Bağlayıcısı ile otomatik görevler ve iş akışlarını izlemek, oluşturun, gönderin ve hesabınız üzerinden dosyaları alma oluşturabilirsiniz bir [SFTP](https://www.ssh.com/ssh/sftp/) örneğin diğer eylemlerin yanı sıra sunucu:

* Dosyaları eklenen veya değiştirilen zamana yönelik İzleyici.
* Al, oluştur, kopyalayın, yeniden adlandırma, listesinde güncelleştirmek ve dosyaları silin.
* Klasör oluşturun.
* Dosya içeriğini ve meta verileri alın.
* Arşivi klasöre ayıklayın.

SFTP sunucunuzdan yanıtlar almak ve çıkış diğer eylemler için kullanılabilir Tetikleyicileri kullanabilirsiniz. Logic apps eylemleri, dosyaları SFTP sunucunuzdaki görevleri gerçekleştirmek için kullanabilirsiniz. Ayrıca SFTP eylemleri çıktısını kullanan diğer eylemler olabilir. Örneğin, düzenli olarak dosyaları SFTP sunucunuzdan almak, Office 365 Outlook Bağlayıcısı veya Outlook.com Bağlayıcısı'nı kullanarak bu dosyaları ve bunların içeriğini hakkında e-posta gönderebilirsiniz.
Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md)

## <a name="sftp-ssh-versus-sftp"></a>SFTP-SSH ve SFTP

SFTP-SSH bağlayıcı arasında bazı önemli farklar şunlardır ve [SFTP](../connectors/connectors-create-api-sftp.md) bağlayıcı. SFTP-SSH bağlayıcı, bu yetenekleri sağlar:

* Kullanan <a href="https://github.com/sshnet/SSH.NET" target="_blank"> **SSH.NET** </a> bir açık kaynak güvenli Kabuk (SSH) kitaplığı için .NET kitaplığı.

* Büyük dosyalar için destek sağlar, en fazla **1 GB**. Bağlayıcı, okuma veya yazma boyutu 1 GB'a kadar olan dosyaları.

* Sağlar **klasör oluştur** eylem, SFTP sunucusundaki belirtilen yolda bir klasör oluşturur.

* Sağlar **dosyayı yeniden adlandır** eylem, SFTP sunucusundaki bir dosyayı yeniden adlandırır.

* Bağlantı performansı artırır ve sunucudaki bağlantı girişimlerinin sayısını azaltan SFTP sunucusuna önbelleğe alır. 

  Bağlantı kurma tarafından önbelleğe almak için kullanılan süreyi denetleyebilirsiniz <a href="http://man.openbsd.org/sshd_config#ClientAliveInterval" target="_blank"> **satırını Clientaliveınterval** </a> SFTP sunucunuzdaki özelliği. 

## <a name="how-trigger-polling-works"></a>Yoklama works nasıl tetikleyin

SFTP dosya sistemi yoklama ve son yoklamadan bu yana değiştirilmiş her dosya için arayarak SFTP-SSH Tetikleyiciler çalışır. Bazı araçlar, dosyaları değiştirdiğinizde, bu nedenle bu gibi durumlarda çalışacak şekilde Tetikleyiciniz için bu özelliği devre dışı gerekir. zaman damgası koruma sağlar. Bazı ortak ayarları şunlardır:

| SFTP istemci | Eylem | 
|-------------|--------| 
| Winscp | Seçenekler → tercihleri... → Aktarımı → Düzenle... → Koru zaman damgası → devre dışı bırak |
| FileZilla | Aktarım → Koru aktarılan dosyaları → zaman damgalarının devre dışı bırak | 
||| 

Tetikleyici yeni bir dosya bulduğunda tetikleyici, yeni dosya eksiksiz ve kısmen yazılmış olup olmadığını denetler. Örneğin, dosya sunucusu tetikleyici iade ederken bir dosya değişiklikleri sürüyor olabilir. Kısmen yazılı bir dosya döndürme önlemek için zaman damgası, son değişiklikler var, ancak bu dosyayı hemen döndürmüyor dosyası için tetikleyici notlar. Tetikleyici, yalnızca sunucu yeniden yoklanırken dosyayı döndürür. Bazı durumlarda, bu davranışı, iki kez tetikleyicinin kadar yoklama aralığı bir gecikmeye neden olabilir. 

Dosya içeriği isterken tetikleyici 50 MB'tan büyük dosyaları almak değil. 50 MB'tan büyük dosyaları almak için bu düzeni izleyin:

* Dosya özellikleri gibi döndüren bir tetikleyici kullanmanız **dosya eklendiğinde veya değiştirildiğinde (yalnızca Özellikler)**. 
* Tam dosya gibi okuyan bir eylemle tetikleyici izleyin **yolunu kullanarak dosya içeriğini Al**.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

* Mantıksal uygulamanızın bağlantı oluşturun ve SFTP hesabınıza erişmesine yetki SFTP konak sunucu adresi ve hesap kimlik bilgilerinizi.

  SSH özel anahtarının tam içeriğini kopyalayıp **SSH özel anahtarı** çok satırlı biçimde izleyerek özelliği. 
  Notepad.exe kullanarak SSH özel anahtarının nasıl gösteren örnek adımlar şunlardır:
    
  1. SSH özel anahtar dosyası içinde Notepad.exe açın
  2. Üzerinde **Düzenle** menüsünde **Tümünü Seç**.
  3. Seçin **Düzenle** > **kopyalama**.
  4. İçinde bağlantı oluştururken **SSH özel anahtarı** özelliği anahtarını yapıştırın. El ile düzenlemeyin **SSH özel anahtarı** özelliği.

     Bağlayıcı, bu SSH özel anahtar biçimleri ve yalnızca MD5 parmak izi destekler SSH.NET kitaplığını kullanır:

     * RSA 
     * DSA

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
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)