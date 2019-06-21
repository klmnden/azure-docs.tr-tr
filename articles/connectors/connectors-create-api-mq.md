---
title: IBM MQ server - Azure Logic Apps ile bağlantı
description: Gönder ve iletileri bir Azure veya şirket içinde IBM MQ server ve Azure Logic Apps ile alma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: valrobb
ms.author: valthom
ms.reviewer: chrishou, LADocs
ms.topic: article
ms.date: 06/19/2019
tags: connectors
ms.openlocfilehash: a2894799946d069916b27a4f5bcc7bd3244705b2
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67273129"
---
# <a name="connect-to-an-ibm-mq-server-from-azure-logic-apps"></a>Azure Logic Apps'ten bir IBM MQ sunucuya bağlanın

IBM MQ bağlayıcı gönderir ve bir IBM MQ server şirket içinde veya Azure'da depolanan iletilerini alır. Bu bağlayıcı bir TCP/IP ağı üzerinden uzak bir IBM MQ sunucusu ile iletişim kuran bir Microsoft MQ client içerir. Bu makalede MQ bağlayıcıyı kullanmak üzere bir başlangıç kılavuzu sağlanmaktadır. Bir iletinin kuyrukta göz atarak başlayın ve diğer eylemler deneyin.

IBM MQ Bağlayıcısı'nı bu eylemleri içerir, ancak hiçbir Tetikleyiciler sağlar:

- Tek bir ileti IBM MQ sunucudan ileti silmeden göz atın.
- Toplu iletiler IBM MQ sunucudan iletileri silmeden göz atın.
- Tek bir ileti alırsınız ve IBM MQ sunucudan ileti Sil
- Toplu olarak mesaj alır ve IBM MQ sunucudan iletileri sil
- IBM MQ sunucunun tek bir ileti gönderin

## <a name="prerequisites"></a>Önkoşullar

* Bir şirket içi MQ server kullanıyorsanız, [şirket içi veri ağ geçidi yükleme](../logic-apps/logic-apps-gateway-install.md) ağınızdaki bir sunucuda. Ayrıca, şirket içi veri ağ geçidinin yüklü olduğu sunucunun çalışması için .NET Framework 4.6 MQ bağlayıcı için yüklü olması gerekir. Ayrıca, Azure'da şirket içi veri ağ geçidi için bir kaynak oluşturmalısınız. Daha fazla bilgi için [veri ağ geçidi bağlantısı kurma](../logic-apps/logic-apps-gateway-connection.md).

  Ancak, MQ server'ınıza Azure kapsamında sunulan veya genel olarak kullanılabilir, data gateway kullanın gerekmez.

* Resmi olarak desteklenen IBM WebSphere MQ sürümleri:

  * MQ 7.5
  * MQ 8.0
  * MQ 9.0

* MQ eylem eklemek istediğiniz mantıksal uygulaması. Bu mantıksal uygulama, şirket içi veri ağ geçidi bağlantınızı konumun aynısını kullanmanız gerekir ve iş akışınızı başlatan bir tetikleyici zaten olması gerekir. 

  Mantıksal uygulamanızı ilk kez bir tetikleyici eklemelisiniz MQ Bağlayıcısı'nı hiçbir tetikleyici yok. Örneğin, yinelenme tetikleyicisini kullanabilirsiniz. Logic apps kullanmaya yeni başladıysanız, bu deneyin [ilk mantıksal uygulamanızı oluşturmak için Hızlı Başlangıç](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

## <a name="browse-a-single-message"></a>Tek bir ileti Gözat

1. Tetikleyici veya başka bir eylem altında mantıksal uygulamanızı seçin **yeni adım**. 

1. Arama kutusuna "mq" yazın ve şu eylemi seçin: **İleti Gözat**

   ![İleti Gözat](media/connectors-create-api-mq/Browse_message.png)

1. Varolan MQ bağlantısı yoksa, bağlantıyı oluşturun:  

   1. Eylem seçin **şirket içi veri ağ geçidi üzerinden Bağlan**.
   
   1. MQ server'ınız için özelliklerini girin.  

      İçin **sunucu**, MQ server adını veya IP adresini bir iki nokta üst üste ve bağlantı noktası numarası girin.
    
   1. Açık **ağ geçidi** herhangi daha önce yapılandırılmış ağ geçidi bağlantıları gösteren bir liste. Ağ geçidi seçin.
    
   1. İşiniz bittiğinde **Oluştur**’u seçin. 
   
      Bağlantınız şu örnekteki gibi görünür:

      ![Bağlantı özellikleri](media/connectors-create-api-mq/Connection_Properties.png)

1. Eylemin özelliklerini ayarlayın:

   * **Kuyruk**: Bağlantıyı farklı bir kuyruk belirtin.

   * **MessageID**, **Correlationıd**, **GroupID**ve diğer özellikleri: Farklı MQ ileti özelliklerine bağlı olarak bir ileti için Gözat

   * **IncludeInfo**: Belirtin **True** çıktısında ek ileti bilgileri içerecek şekilde. Ya da belirtin **False** ek ileti bilgileri çıktısında içermeyecek şekilde.

   * **Zaman aşımı**: Bir ileti, boş bir kuyrukta gelmesi için beklenecek süreyi belirlemek için bir değer girin. Hiçbir şey girilirse, sıradaki ilk iletiye alınır ve görüntülenecek bir ileti için beklerken geçen hiçbir zaman yoktur.

     ![İleti özelliklerini göz atın](media/connectors-create-api-mq/Browse_message_Props.png)

1. **Kaydet** yaptığınız değişiklikleri ve ardından **çalıştırma** mantıksal uygulamanızı.

   ![Kaydet ve Çalıştır](media/connectors-create-api-mq/Save_Run.png)

   Çalıştırmanız tamamlandıktan sonra çalıştırma adımları gösterilir ve çıktıyı gözden geçirin.

1. Her bir adımın ayrıntılarını gözden geçirmek için yeşil onay işaretini seçin. Daha fazla bilgi için çıktı verilerini gözden geçirmek için seçin **ham çıkışları görüntüleyin**.

   ![İleti çıkış Gözat](media/connectors-create-api-mq/Browse_message_output.png)  

   Bazı örnek ham çıktı aşağıdaki gibidir:

   ![İleti ham çıkış Gözat](media/connectors-create-api-mq/Browse_message_raw_output.png)

1. Ayarlarsanız **IncludeInfo** true olarak, aşağıdaki çıkış görüntülenir:

   ![Göz atma iletiyi bilgisi Ekle](media/connectors-create-api-mq/Browse_message_Include_Info.png)

## <a name="browse-multiple-messages"></a>Birden çok ileti Gözat

**Gözat iletileri** eylemi içeren bir **BatchSize** kaç iletileri kuyruktan döndürülmesi gerektiğini belirtmek için seçeneği.  Varsa **BatchSize** herhangi bir giriş tüm iletileri döndürülür. Döndürülen çıkış iletileri dizisidir.

1. Eklediğinizde **Gözat iletileri** eylemi, önceden yapılandırılmış bağlantı varsayılan olarak seçilen ilk. Yeni bir bağlantı oluşturmak için seçin **Bağlantıyı Değiştir**. Ya da farklı bir bağlantı seçin.

1. Mantık uygulaması Çalıştır bittikten sonra bazı örnek çıktısı şöyledir **Gözat iletileri** eylem:

   ![İletileri çıkış Gözat](media/connectors-create-api-mq/Browse_messages_output.png)

## <a name="receive-single-message"></a>Tek bir ileti alırsınız.

**Alma ileti** eylem aynı giriş yapılmış ve olarak çıkaran **Gözat ileti** eylem. Kullanırken **alma ileti**, ileti kuyruktan silinir.

## <a name="receive-multiple-messages"></a>Birden çok ileti alma

**İletileri alma** eylem aynı giriş yapılmış ve olarak çıkaran **Gözat iletileri** eylem. Kullanırken **iletileri alma**, iletileri kuyruktan silinir.

Adım varsa ileti kuyrukta bir göz atın veya alma işlemi yapılırken, bu Çıktıyı ile başarısız olur:  

![MQ yok, hata iletisi](media/connectors-create-api-mq/MQ_No_Msg_Error.png)

## <a name="send-message"></a>İleti Gönder

Eklediğinizde **ileti gönderme** eylemi, önceden yapılandırılmış bağlantı varsayılan olarak seçilen ilk. Yeni bir bağlantı oluşturmak için seçin **Bağlantıyı Değiştir**. Ya da farklı bir bağlantı seçin.

1. Geçerli ileti türünü seçin: **Veri birimi**, **yanıt**, veya **iste**  

   ![Msg özellikler Gönder](media/connectors-create-api-mq/Send_Msg_Props.png)

1. Mantıksal uygulama çalışmayı tamamladıktan sonra bazı örnek çıktısı şöyledir **ileti gönder** eylem:

   ![Çıkış iletisi gönder](media/connectors-create-api-mq/Send_Msg_Output.png)

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Eylemler ve sınırları hakkında teknik ayrıntılar için bağlayıcının Openapı'nin açıklanmıştır (önceki adıyla Swagger) açıklama, bağlayıcının gözden [başvuru sayfası](/connectors/mq/).

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
