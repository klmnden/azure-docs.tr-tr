---
title: SSH - Azure Logic Apps ile SFTP sunucusuna bağlanmak | Microsoft Docs
description: İzleme, oluşturma, yönetme, göndermek ve SSH ve Azure Logic Apps'ı kullanarak bir SFTP sunucusu için dosyaları alma görevlerini otomatikleştirme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: divswa, LADocs
ms.topic: article
tags: connectors
ms.date: 10/31/2018
ms.openlocfilehash: 336288aaf3817fe267d58a225249bf54cca691bc
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2018
ms.locfileid: "50979106"
---
# <a name="monitor-create-and-manage-sftp-files-by-using-ssh-and-azure-logic-apps"></a>İzleme, oluşturma ve SSH ve Azure Logic Apps kullanarak SFTP dosyaları yönetme

İzleme, oluşturun, gönderin ve dosyaları alma görevlerini otomatikleştirmek için bir [güvenli dosya aktarım protokolünü (SFTP)](https://www.ssh.com/ssh/sftp/) kullanarak sunucu [güvenli Kabuk (SSH)](https://www.ssh.com/ssh/protocol/) protokolü, derleme ve tümleştirme otomatikleştirin Azure Logic Apps ve SFTP-SSH Bağlayıcısı'nı kullanarak iş akışları. SFTP herhangi bir güvenilir veri akışı dosya erişimi, dosya aktarımı ve dosya yönetimi sağlayan bir ağ protokolüdür. Otomatik hale getirebilirsiniz bazı örnek görevler aşağıda verilmiştir: 

* Dosyaları eklenen veya değiştirilen zamana yönelik İzleyici.
* Al, oluştur, kopyalayın, yeniden adlandırma, listesinde güncelleştirmek ve dosyaları silin.
* Klasör oluşturun.
* Dosya içeriğini ve meta verileri alın.
* Arşivi klasöre ayıklayın.

Karşılaştırılan [SFTP Bağlayıcısı](../connectors/connectors-create-api-sftp.md), SFTP-SSH bağlayıcı okuyup okuyamayacağını veya dosyaları en fazla yazma *1 GB* boyutu. 1 GB'den büyük olan dosyalar için SFTP-SSH kullanabilirsiniz bağlayıcı artı [büyük iletileri işlemek için Öbekleme](../logic-apps/logic-apps-handle-large-messages.md). Daha fazla fark için gözden [karşılaştırma SFTP-SSH ve SFTP](#comparison) bu makalenin ilerleyen bölümlerinde.

SFTP sunucunuzdaki olayları izleyen ve çıkış diğer eylemler için kullanılabilir hale getirmek Tetikleyicileri kullanabilirsiniz. SFTP sunucunuzda çeşitli görevler gerçekleştiren eylemlerini kullanabilirsiniz. SFTP eylemleri çıktısını kullanan diğer eylemler mantıksal uygulamanızda da olabilir. Örneğin, düzenli olarak dosyaları SFTP sunucunuzdan almak, dosyaları ve içeriklerini hakkında e-posta uyarıları Office 365 Outlook Bağlayıcısı veya Outlook.com bağlayıcısını kullanarak gönderebilirsiniz.
Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md)

<a name="comparison"></a>

## <a name="compare-sftp-ssh-versus-sftp"></a>SFTP-SSH ve SFTP karşılaştırın

SFTP-SSH Bağlayıcısı ve SFTP-SSH bağlayıcı yeteneklere sahip olduğu SFTP Bağlayıcısı arasındaki diğer temel farklılıklar şunlardır:

* Kullanan <a href="https://github.com/sshnet/SSH.NET" target="_blank"> **SSH.NET** </a> .NET destekleyen bir açık kaynak güvenli Kabuk (SSH) kitaplığı olan bir kitaplık. 

  > [!NOTE]
  >
  > SFTP-SSH bağlayıcısını destekler *yalnızca* bu özel anahtarlar, biçimleri, algoritmaları ve parmak izi:
  > 
  > * **Özel anahtar biçimleri**: RSA (Rivest Shamir Adleman) ve DSA (dijital imza algoritması) biçimleri anahtarları OpenSSH hem ssh.com
  > * **Şifreleme algoritmalarını**: DES EDE3 CBC, DES-EDE3-CFB DES CBC, AES-128-CBC, AES 192 CBC ve AES 256 CBC
  > * **Parmak izi**: MD5

* Okur veya dosyalarını kadar Yazar *1 GB* SFTP Bağlayıcısı karşılaştırıldığında boyutu. 1 GB'den büyük olan dosyaları için kullanan [büyük iletileri işlemek için Öbekleme](../logic-apps/logic-apps-handle-large-messages.md). 

* Sağlar **klasör oluştur** eylem, SFTP sunucusundaki belirtilen yolda bir klasör oluşturur.

* Sağlar **dosyayı yeniden adlandır** eylem, SFTP sunucusundaki bir dosyayı yeniden adlandırır.

* SFTP sunucusuna bağlanırken önbelleğe *için 1 saate kadar*, performansı geliştirir ve konumundaki sunucuya bağlanma denemelerinin sayısını azaltır. Bu süre önbelleğe alma davranışını belirlemek için Düzenle <a href="http://man.openbsd.org/sshd_config#ClientAliveInterval" target="_blank"> **satırını Clientaliveınterval** </a> SSH yapılandırması SFTP sunucunuzdaki bir özellik. 

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

* Mantıksal uygulamanızı SFTP hesabınıza erişmesine olanak sağlayan SFTP sunucu adresi ve hesap kimlik bilgilerinizi. Ayrıca erişim için SSH özel anahtarı ve SSH özel anahtar parolası gerekir. 

  > [!IMPORTANT]
  >
  > SFTP-SSH bağlayıcısını destekler *yalnızca* bu özel anahtar biçimleri, algoritmaları ve parmak izi:
  > 
  > * **Özel anahtar biçimleri**: RSA (Rivest Shamir Adleman) ve DSA (dijital imza algoritması) biçimleri anahtarları OpenSSH hem ssh.com
  > * **Şifreleme algoritmalarını**: DES EDE3 CBC, DES-EDE3-CFB DES CBC, AES-128-CBC, AES 192 CBC ve AES 256 CBC
  > * **Parmak izi**: MD5
  >
  > SFTP-SSH tetikleyici veya eylemi istediğiniz ekledikten sonra mantıksal uygulamanızı oluştururken, SFTP sunucunuzun bağlantı bilgilerini sağlamanız gerekir. 
  > SSH özel anahtarı kullanıyorsanız, emin ***kopyalama*** SSH özel anahtar dosyanıza anahtarından ve ***yapıştırın*** bağlantı ayrıntıları bu anahtara ***el ile girin yok veyaanahtarınıdüzenleyin***, bağlantı başarısız olmasına neden. 
  > Bu makalenin sonraki adımlarda daha fazla bilgi için bkz.

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* SFTP hesabınıza erişmek için istediğiniz mantıksal uygulaması. Bir SFTP-SSH tetikleyici ile başlatmak için [boş mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). SFTP-SSH eylem kullanmak için mantıksal uygulamanızı başka bir tetikleyici ile başlar, **yinelenme** tetikleyici.

## <a name="connect-to-sftp-with-ssh"></a>SFTP SSH ile bağlanma

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Oturum [Azure portalında](https://portal.azure.com)ve Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. Boş mantıksal uygulamaları, arama kutusuna girin "sftp ssh" filtreniz olarak. Tetikleyiciler listesinde istediğiniz tetikleyicisini seçin. 

   -veya-

   Var olan mantıksal uygulamalar, son adım, bir eylem eklemek istediğiniz altında seçin için **yeni adım**. 
   Arama kutusuna "sftp ssh" filtreniz olarak. 
   Eylemler listesinde, istediğiniz eylemi seçin.

   Adımlar arasında bir eylem eklemek için işaretçinizi adımlar arasındaki okun üzerine getirin. 
   Artı işaretini seçin (**+**), görünür ve ardından **Eylem Ekle**.

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

   1. SFTP SSH tetikleyici veya eylemi eklediğiniz yapıştırın *tam* içine kopyaladığınız anahtar **SSH özel anahtarı** özelliği birden çok satır destekler. 
   ***Yapıştırdığınız emin*** anahtarı. ***El ile girmek yoksa veya anahtarı düzenlemek***.

1. Bitirdiğinizde bağlantı ayrıntıları girerek, seçin **Oluştur**.

1. Artık seçili tetikleyici veya eylem için gerekli bilgileri sağlayın ve mantıksal uygulamanızın iş akışı oluşturmaya devam edin.

## <a name="trigger-limits"></a>Tetikleyici sınırları

SFTP dosya sistemi yoklama ve son yoklamadan bu yana değiştirilmiş her dosya için arayarak SFTP-SSH Tetikleyiciler çalışır. Bazı araçlar, dosyaları değiştirdiğinizde, zaman damgası koruma sağlar. Bu gibi durumlarda, Tetikleyiciniz çalışabilmesi için bu özelliği devre dışı gerekir. Bazı ortak ayarları şunlardır:

| SFTP istemci | Eylem | 
|-------------|--------| 
| Winscp | Git **seçenekleri** > **tercihleri** > **aktarım** > **Düzenle**  >  **Korumak zaman damgası** > **devre dışı bırak** |
| FileZilla | Git **aktarım** > **korumak aktarılan dosyaların zaman damgaları** > **devre dışı bırak** | 
||| 

Tetikleyici yeni bir dosya bulduğunda tetikleyici, yeni dosya eksiksiz ve kısmen yazılmış olup olmadığını denetler. Örneğin, dosya sunucusu tetikleyici iade ederken bir dosya değişiklikleri sürüyor olabilir. Kısmen yazılı bir dosya döndürme önlemek için zaman damgası, son değişiklikler var, ancak bu dosyayı hemen döndürmüyor dosyası için tetikleyici notlar. Tetikleyici, yalnızca sunucu yeniden yoklanırken dosyayı döndürür. Bazı durumlarda, bu davranışı, iki kez tetikleyicinin kadar yoklama aralığı bir gecikmeye neden olabilir. 

Dosya içeriği isterken tetikleyici 50 MB'tan büyük dosyaları almak değil. 50 MB'tan büyük dosyaları almak için bu düzeni izleyin:

* Dosya özellikleri gibi döndüren bir tetikleyici kullanmanız **dosya eklendiğinde veya değiştirildiğinde (yalnızca Özellikler)**. 
* Tam dosya gibi okuyan bir eylemle tetikleyici izleyin **yolunu kullanarak dosya içeriğini Al**.

## <a name="examples"></a>Örnekler

### <a name="sftp---ssh-trigger-when-a-file-is-added-or-modified"></a>SFTP - SSH tetikleyin: ne zaman bir dosya eklendiğinde veya değiştirildiğinde

Bir dosya eklendiğinde veya bir SFTP sunucu üzerinde değiştirilmiş bu tetikleyiciyi bir mantıksal uygulama iş akışı başlatır. Örneğin, dosyanın içeriğini denetler ve içerik belirtilen bir koşulu karşılayıp içeriği alan bir koşul ekleyebilirsiniz. Ardından, dosyanın içeriğini alır ve bu içeriği SFTP sunucusunda bir klasöre koyar, bir eylem ekleyebilirsiniz. 

**Kuruluş örnek**: Bu tetikleyici, müşteri siparişleri temsil eden yeni dosyalar için bir SFTP klasörü izlemek için kullanabilirsiniz. Ardından bir SFTP eylemi gibi kullanabilir **dosya içeriğini Al** daha ayrıntılı işleme için sipariş içeriklerini almak ve o sırada bir sipariş veritabanında depolayın.

### <a name="sftp---ssh-action-get-content"></a>SFTP - SSH eylem: içerik alma

Bu işlem bir SFTP sunucusuna dosya içeriği alır. Örneğin, önceki örnekte ve dosyanın içeriğini karşılaması gereken bir koşul tetikleyici ekleyebilirsiniz. Koşul true ise, içeriği alır eylemi çalıştırabilirsiniz. 

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları hakkında teknik ayrıntılar için bağlayıcının Openapı'nin açıklanmıştır (önceki adıyla Swagger) açıklama, bağlayıcının gözden [başvuru sayfası](/connectors/sftpconnector/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)