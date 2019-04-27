---
title: MQ - Azure Logic Apps sunucusuna | Microsoft Docs
description: Gönder ve iletileri bir Azure veya şirket içi MQ server ve Azure Logic Apps ile alma
author: valrobb
ms.author: valthom
ms.date: 06/01/2017
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 9e6ae5cb0afd75a1e87fe4d4d0cf307abab5a02a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60688869"
---
# <a name="connect-to-an-ibm-mq-server-from-logic-apps-using-the-mq-connector"></a>Bir IBM MQ MQ Bağlayıcısı'nı kullanarak logic apps'ten sunucusuna

MQ için Microsoft Connector gönderir ve MQ Server şirket içi veya Azure'da depolanan iletilerini alır. Bu bağlayıcı bir TCP/IP ağı üzerinden uzak bir IBM MQ sunucusu ile iletişim kuran bir Microsoft MQ client içerir. Bu belge, MQ bağlayıcıyı kullanmak üzere bir başlatıcı kılavuzudur. Bir iletinin kuyrukta göz atma ve ardından diğer eylemleri çalışırken başlamadan önerilir.

MQ Bağlayıcısı aşağıdaki eylemleri içerir. Tetikleyici yoktur.

- Tek bir ileti IBM MQ sunucudan ileti silmeden göz atın.
- Toplu iletiler IBM MQ sunucudan iletileri silmeden göz atın.
- Tek bir ileti alırsınız ve IBM MQ sunucudan ileti Sil
- Toplu olarak mesaj alır ve IBM MQ sunucudan iletileri sil
- IBM MQ sunucunun tek bir ileti gönderin

## <a name="prerequisites"></a>Önkoşullar

* Bir şirket içi MQ server kullanıyorsanız [şirket içi veri ağ geçidi yükleme](../logic-apps/logic-apps-gateway-install.md) ağınızdaki bir sunucuda. MQ Server genel olarak kullanılabilir ve azure'da kullanılabilir ise, ardından veri ağ geçidi kullanılan gerekli veya yok.

    > [!NOTE]
    > Ayrıca, şirket içi veri ağ geçidinin yüklü olduğu sunucunun işlevine MQ Bağlayıcısı yüklü .NET Framework 4.6 olması gerekir.

* Şirket içi veri ağ geçidi - Azure kaynağı oluşturma [veri ağ geçidi bağlantısı kurma](../logic-apps/logic-apps-gateway-connection.md).

* Resmi olarak desteklenen IBM WebSphere MQ sürümleri:
    * MQ 7.5
    * MQ 8.0

## <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma

1. İçinde **Azure başlangıç Panosu**seçin **+** (artı) **Web + mobil**, ardından **mantıksal uygulama**.
2. Girin **adı**, MQTestApp gibi **abonelik**, **kaynak grubu**, ve **konumu** (konumu kullanmak burada şirket içi Veri ağ geçidi bağlantısı yapılandırılır). Seçin **panoya Sabitle**seçip **Oluştur**.  
![Mantıksal uygulama oluşturma](media/connectors-create-api-mq/Create_Logic_App.png)

## <a name="add-a-trigger"></a>Tetikleyici ekleyin

> [!NOTE]
> MQ Bağlayıcısı'nı hiçbir tetikleyici yok. Bu nedenle, mantıksal uygulamanızı gibi başlatmak için başka bir tetikleyici kullanmanız **yinelenme** tetikleyici.

1. **Logic Apps Tasarımcısı'nda** açar, seçim **yinelenme** ortak Tetikleyicileri listesinde.
2. Seçin **Düzenle** içinde yineleme tetikleyicisi.
3. Ayarlama **sıklığı** için **gün**, ayarlayıp **aralığı** için **7**.

## <a name="browse-a-single-message"></a>Tek bir ileti Gözat
1. Seçin **+ yeni adım**seçip **Eylem Ekle**.
2. Arama kutusuna `mq`ve ardından **MQ - Gözat ileti**.  
![İleti Gözat](media/connectors-create-api-mq/Browse_message.png)

3. Varolan MQ bağlantısı yoksa, ardından bağlantı oluşturun:  

    1. Seçin **şirket içi veri ağ geçidi üzerinden Bağlan**ve MQ Server özelliklerini girin.  
    İçin **sunucu**, MQ server adını veya IP adresini bir iki nokta üst üste ve bağlantı noktası numarası girin.
    2. **Ağ geçidi** yapılandırılmış herhangi bir mevcut ağ geçidi bağlantıları açılan listeler. Ağ geçidi seçin.
    3. Bittiğinde **Oluştur**’u seçin. Bağlantınızı aşağıdakine benzer:  
    ![Bağlantı özellikleri](media/connectors-create-api-mq/Connection_Properties.png)

4. Eylem özellikleri şunları yapabilirsiniz:  

    * Kullanım **kuyruk** bağlantı tanımlanan değerinden farklı bir kuyruk adı erişmek için özelliği
    * Kullanım **MessageID**, **Correlationıd**, **GroupID**ve diğer özelliklere göre farklı MQ ileti özellikleri bir ileti göz atmak için
    * Ayarlama **IncludeInfo** için **True** çıktısında ek ileti bilgileri içerecek şekilde. Veya ayarlayın **False** ek ileti bilgileri çıktısında içermeyecek şekilde.
    * Girin bir **zaman aşımı** bir ileti, boş bir kuyrukta gelmesi için beklenecek süreyi belirlemek için değer. Hiçbir şey girilirse, sıradaki ilk iletiye alınır ve görüntülenecek bir ileti için beklerken geçen hiçbir zaman yoktur.  
    ![İleti özelliklerini göz atın](media/connectors-create-api-mq/Browse_message_Props.png)

5. **Kaydet** yaptığınız değişiklikleri ve ardından **çalıştırma** mantıksal uygulamanızı:  
![Kaydet ve Çalıştır](media/connectors-create-api-mq/Save_Run.png)

6. Birkaç saniye sonra çalıştırma adımları gösterilir ve çıktıyı bakabilirsiniz. Her adımın ayrıntıları görmek için yeşil onay işaretini seçin. Seçin **bkz ham çıkışları** çıkış verileri ek ayrıntıları görmek için.  
![İleti çıkış Gözat](media/connectors-create-api-mq/Browse_message_output.png)  

    Ham çıkış:  
    ![İleti ham çıkış Gözat](media/connectors-create-api-mq/Browse_message_raw_output.png)

7. Zaman **IncludeInfo** seçeneği true, aşağıdaki çıktıyı görüntülenir:  
![Göz atma iletiyi bilgisi Ekle](media/connectors-create-api-mq/Browse_message_Include_Info.png)

## <a name="browse-multiple-messages"></a>Birden çok ileti Gözat
**Gözat iletileri** eylemi içeren bir **BatchSize** kaç iletileri kuyruktan döndürülmesi gerektiğini belirtmek için seçeneği.  Varsa **BatchSize** herhangi bir giriş tüm iletileri döndürülür. Döndürülen çıkış iletileri dizisidir.

1. Eklerken **Gözat iletileri** eylemi, yapılandırılan ilk bağlantı, varsayılan olarak seçilidir. Seçin **Bağlantıyı Değiştir** yeni bir bağlantı oluşturun veya farklı bir bağlantı seçin.

2. Göz atma çıktısını gösterir iletileri:  
![İletileri çıkış Gözat](media/connectors-create-api-mq/Browse_messages_output.png)

## <a name="receive-a-single-message"></a>Tek bir ileti alırsınız.
**Alma ileti** eylem aynı giriş yapılmış ve olarak çıkaran **Gözat ileti** eylem. Kullanırken **alma ileti**, ileti kuyruktan silinir.

## <a name="receive-multiple-messages"></a>Birden çok ileti alma
**İletileri alma** eylem aynı giriş yapılmış ve olarak çıkaran **Gözat iletileri** eylem. Kullanırken **iletileri alma**, iletileri kuyruktan silinir.

Adım varsa ileti kuyrukta bir göz atın veya alma işlemi yaparken, aşağıdaki çıktıyı ile başarısız olur:  
![MQ yok, hata iletisi](media/connectors-create-api-mq/MQ_No_Msg_Error.png)

## <a name="send-a-message"></a>İleti gönderme
1. Eklerken **ileti gönder** eylemi, yapılandırılan ilk bağlantı, varsayılan olarak seçilidir. Seçin **Bağlantıyı Değiştir** yeni bir bağlantı oluşturun veya farklı bir bağlantı seçin. Geçerli **ileti türlerini** olan **veri birimi**, **yanıt**, veya **istek**.  
![Msg özellikler Gönder](media/connectors-create-api-mq/Send_Msg_Props.png)

2. İleti Gönder çıktısı aşağıdakine benzer olacaktır:  
![Çıkış iletisi gönder](media/connectors-create-api-mq/Send_Msg_Output.png)

## <a name="connector-specific-details"></a>Özel bağlayıcı ayrıntıları

Tetikleyiciler ve eylemlerin swagger'da tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınırlama Bkz [bağlayıcı ayrıntıları](/connectors/mq/).

## <a name="next-steps"></a>Sonraki adımlar
[Mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Logic Apps diğer kullanılabilir bağlayıcılar keşfedin bizim [API listesi](apis-list.md).
