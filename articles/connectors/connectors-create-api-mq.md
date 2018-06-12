---
title: MQ - Azure Logic Apps sunucuya | Microsoft Docs
description: Gönderme ve bir Azure veya şirket içi MQ server ve Azure Logic Apps ile iletileri alma
author: valthom
manager: jeconnoc
ms.author: valthom
ms.date: 06/01/2017
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 6b34bd7b286ca3b206c611343217c90e0d57fbfb
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295919"
---
# <a name="connect-to-an-ibm-mq-server-from-logic-apps-using-the-mq-connector"></a>MQ Bağlayıcısı'nı kullanarak mantığı uygulamalardan bir IBM MQ sunucuya bağlanın 

MQ için Microsoft Connector gönderir ve MQ Server şirket içi veya Azure depolanan iletilerini alır. Bu bağlayıcı bir TCP/IP ağı üzerinden bir uzak IBM MQ sunucuyla iletişim kuran bir Microsoft MQ istemcisi içerir. Bu belge, MQ Bağlayıcısı'nı kullanmak için bir başlangıç kılavuzdur. Tek bir ileti üzerinde bir sıra gözatma ve diğer eylemleri çalışırken başlamadan önerilir.    

MQ bağlayıcı aşağıdaki eylemleri içerir. Hiçbir Tetikleyicileri vardır.

-   Tek bir ileti message IBM MQ sunucudan silmeden Gözat
-   IBM MQ sunucudan iletileri silmeden toplu iletiler Gözat
-   Tek bir ileti alırsınız ve message IBM MQ sunucusundan silin
-   Toplu iletiler almak ve iletileri IBM MQ sunucusundan silin
-   IBM MQ sunucunun tek bir ileti gönderme 

## <a name="prerequisites"></a>Önkoşullar

* Bir şirket içi MQ server kullanıyorsanız [şirket içi veri ağ geçidini yükleyin](../logic-apps/logic-apps-gateway-install.md) ağınızdaki bir sunucuda. MQ Server genel olarak kullanılabilir veya Azure içinde kullanılabilir değilse, ardından veri ağ geçidi kullanılan gerekli veya değil.

    > [!NOTE]
    > Şirket içi veri ağ geçidinin yüklü olduğu sunucunun ayrıca .net olmalıdır Framework 4.6 işlevi MQ bağlayıcı yüklü.

* Şirket içi veri ağ geçidi için-Azure kaynağını oluşturma [veri ağ geçidi bağlantısı kurma](../logic-apps/logic-apps-gateway-connection.md).

* Resmi olarak IBM WebSphere MQ sürümleri desteklenir:
   * MQ 7.5
   * MQ 8.0

## <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma

1. İçinde **Azure başlangıç Panosu**seçin **+** (artı işareti) **Web + mobil**ve ardından **mantıksal uygulama**. 
2. Girin **adı**, MQTestApp gibi **abonelik**, **kaynak grubu**, ve **konumu** (şirket içi veri ağ geçidi bağlantı yapılandırdığınız yerde konum kullanın). Seçin **panoya Sabitle**seçip **oluşturma**.  
![Mantıksal uygulama oluşturma](media/connectors-create-api-mq/Create_Logic_App.png)

## <a name="add-a-trigger"></a>Tetikleyici ekleyin

> [!NOTE]
> MQ Bağlayıcısı'nı hiçbir tetikleyici yok. Bu nedenle, mantıksal uygulamanızı gibi başlatmak için başka bir tetikleyici kullanmanız **yineleme** tetikleyici. 

1. **Logic Apps Tasarımcısı** açar, select **yineleme** ortak Tetikleyicileri listesinde.
2. Seçin **Düzenle** yineleme tetikleyici içinde. 
3. Ayarlama **sıklığı** için **gün**ve **aralığı** için **7**. 

## <a name="browse-a-single-message"></a>Tek bir iletiye Gözat
1. Seçin **+ yeni adım**seçip **Eylem Ekle**.
2. Arama kutusuna `mq`ve ardından **MQ - Gözat ileti**.  
![İleti Gözat](media/connectors-create-api-mq/Browse_message.png)

3. Varolan MQ bağlantısı değilse, bağlantı oluşturun:  

    1. Seçin **Connect şirket içi veri ağ geçidi üzerinden**ve MQ sunucunuz özelliklerini girin.  
    İçin **Server**, MQ sunucu adı girin veya bir iki nokta üst üste ve bağlantı noktası numarasını ardından IP adresini girin. 
    2. **Ağ geçidi** açılır yapılandırılmış var olan tüm ağ geçidi bağlantıları listeler. Ağ geçidi seçin.
    3. Bittiğinde **Oluştur**’u seçin. Bağlantınızı aşağıdakine benzer:   
    ![Bağlantı özellikleri](media/connectors-create-api-mq/Connection_Properties.png)

4. Eylem özellikleri şunları yapabilirsiniz:  

    * Kullanım **sıra** bağlantısı içinde tanımlanan daha farklı kuyruk adı erişmek için özelliği
    * Kullanım **MessageID**, **correlationıd değeri**, **GroupID**ve farklı MQ ileti özelliklerine bağlı iletiye göz atmak için diğer özellikleri
    * Ayarlama **IncludeInfo** için **True** çıktıda ek ileti bilgileri içerecek şekilde. Veya ayarlamak **False** ek ileti bilgi çıktıda içermeyecek şekilde.
    * Girin bir **zaman aşımı** boş bir sıraya ulaşması bir ileti için beklenecek süreyi belirlemek için değeri. Hiçbir şey girilirse, sıradaki ilk iletiye alınır ve görüntülenecek bir ileti için bekleyen harcanan zaman yok.  
    ![İleti özelliklerini göz atın](media/connectors-create-api-mq/Browse_message_Props.png)

5. **Kaydet** yaptığınız değişiklikleri ve ardından **çalıştırmak** mantıksal uygulamanızı:  
![Kaydet ve Çalıştır](media/connectors-create-api-mq/Save_Run.png)

6. Birkaç saniye sonra çalışma adımları gösterilir ve çıktıyı bakabilirsiniz. Her bir adımın ayrıntıları görmek için yeşil onay işaretini seçin. Seçin **bkz ham çıkışları** çıktı verileri ek ayrıntıları görmek için.  
![İleti çıkış Gözat](media/connectors-create-api-mq/Browse_message_output.png)  

    Ham çıktı:  
    ![İleti ham çıkış Gözat](media/connectors-create-api-mq/Browse_message_raw_output.png)

7. Zaman **IncludeInfo** seçeneği true, aşağıdaki çıkış görüntülenir:  
![Gözat iletiyi bilgisi Ekle](media/connectors-create-api-mq/Browse_message_Include_Info.png)

## <a name="browse-multiple-messages"></a>Birden çok ileti Gözat
**Gözat iletileri** eylemi içeren bir **BatchSize** kaç iletileri sıradan döndürülmelidir belirtmek için seçenek.  Varsa **BatchSize** giriş yok, tüm iletileri döndürülür. Döndürülen çıkış iletileri dizisidir.

1. Eklerken **Gözat iletileri** eylem, yapılandırılmış ilk bağlantı, varsayılan olarak seçilidir. Seçin **değiştirmek bağlantı** yeni bir bağlantı oluşturun veya farklı bir bağlantı seçin.

2. Gözat çıktısını gösterir iletileri:  
![İletileri çıkış Gözat](media/connectors-create-api-mq/Browse_messages_output.png)

## <a name="receive-a-single-message"></a>Tek bir ileti alırsınız
**Alma iletisi** eylem aynı giriş yapılmış olan ve olarak çıkarır **Gözat ileti** eylem. Kullanırken **alma iletisi**, ileti kuyruktan silinir.

## <a name="receive-multiple-messages"></a>Birden çok ileti alma
**Alma iletileri** eylem aynı giriş yapılmış olan ve olarak çıkarır **Gözat iletileri** eylem. Kullanırken **alma iletileri**, ileti kuyruktan silinir.

Varsa hiçbir ileti sırada bir Gözat veya alma işlemi yaparken, adım ile aşağıdaki çıkış başarısız olur:  
![MQ yok, hata iletisi](media/connectors-create-api-mq/MQ_No_Msg_Error.png)

## <a name="send-a-message"></a>İleti gönderme
1. Eklerken **ileti gönder** eylem, yapılandırılmış ilk bağlantı, varsayılan olarak seçilidir. Seçin **değiştirmek bağlantı** yeni bir bağlantı oluşturun veya farklı bir bağlantı seçin. Geçerli **ileti türleri** olan **veri birimi**, **yanıt**, veya **isteği**.  
![Msg özellik Gönder](media/connectors-create-api-mq/Send_Msg_Props.png)

2. İleti Gönder çıktısı aşağıdakine benzer:  
![Msg çıkış Gönder](media/connectors-create-api-mq/Send_Msg_Output.png)

## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tüm tetikleyiciler ve Eylemler swagger tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınır bkz [Bağlayıcısı ayrıntıları](/connectors/mq/).

## <a name="next-steps"></a>Sonraki adımlar
[Mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Logic Apps diğer kullanılabilir bağlayıcılar keşfedin bizim [API'leri listesi](apis-list.md).
