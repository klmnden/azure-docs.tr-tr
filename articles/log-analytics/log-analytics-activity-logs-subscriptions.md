---
title: "Azure etkinlik günlükleri abonelikler arasında günlük analizi toplamak | Microsoft Docs"
description: "Olay hub'ları ve Logic Apps Azure etkinlik günlüğü ' veri toplamak ve bir Azure günlük analizi çalışma alanı farklı bir kiracı içinde göndermek için kullanın."
services: log-analytics, logic-apps, event-hubs
documentationcenter: 
author: richrundmsft
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/08/2018
ms.author: richrund; bwren
ms.openlocfilehash: ded0b4cdcbac747d52435023a24b5719f3c58758
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="collect-azure-activity-logs-into-log-analytics-across-subscriptions"></a>Azure etkinlik günlükleri günlük analizi abonelikler arasında Topla

Bu makalede adımlar Logic Apps için Azure günlük analizi veri toplayıcı Bağlayıcısı'nı kullanarak bir günlük analizi çalışma alanına Azure etkinlik günlükleri toplamak için bir yöntem aracılığıyla. Farklı bir Azure Active Directory'de bir çalışma alanı için günlükleri göndermeniz gerektiğinde bu makalede işlemini kullanın. Örneğin, bir yönetilen hizmet sağlayıcısı, bir müşterinin aboneliğini etkinlik günlükleri toplamak ve bunları kendi aboneliğinizi günlük analizi çalışma depolamak isteyebilirsiniz.

Günlük analizi çalışma alanı aynı Azure aboneliğinden veya farklı bir abonelik ancak aynı Azure Active Directory ise, içindeki adımları kullanın [Azure etkinlik günlüğü çözümünü](../log-analytics/log-analytics-activity.md) Azure etkinlik günlükleri toplamak için.

## <a name="overview"></a>Genel Bakış

Azure etkinlik günlüğü olayları göndermek için bu senaryoda kullanılan strateji olup bir [olay hub'ı](../event-hubs/event-hubs-what-is-event-hubs.md) burada bir [mantıksal uygulama](../logic-apps/logic-apps-overview.md) için günlük analizi çalışma alanınız gönderir. 

![Etkinlik günlüğü için günlük analizi veri akışından görüntüsü](media/log-analytics-activity-logs-subscriptions/data-flow-overview.png)

Bu yaklaşımın avantajları şunlardır:
- Azure etkinlik günlüğü olay Hub'ına akışı beri düşük gecikme süresi.  Mantıksal uygulama sonra tetiklenir ve günlük analizi verilerini gönderir. 
- En az kodu gereklidir ve dağıtmak için hiçbir sunucu altyapısı yok.

Bu makalede nasıl adımlar:
1. Bir olay hub'ı oluşturun. 
2. Etkinlik günlükleri Azure etkinlik günlüğü dışarı aktarma profili kullanarak bir Event Hub verin.
3. Olay Hub'ından okumak ve olayları için günlük analizi göndermek için bir mantıksal uygulama oluşturun.

## <a name="requirements"></a>Gereksinimler
Bu senaryoda kullanılan Azure kaynakları için gereksinimleri verilmiştir.

- Olay hub'ı ad alanı günlükleri yayma abonelik ile aynı abonelikte olması gerekmez. Ayarı yapılandıran kullanıcı hem de abonelikleri için uygun erişim izinleri olmalıdır. Aynı Azure Active Directory'de birden çok aboneliğiniz varsa, etkinlik günlükleri tüm abonelikler için tek bir olay hub'ına gönderebilirsiniz.
- Mantıksal uygulama olay hub'ı farklı bir abonelik olabilir ve aynı Azure Active Directory'de olması gerekmez. Mantıksal uygulama olay Hub'ından olay Hub'ın paylaşılan erişim anahtarı kullanarak okur.
- Günlük analizi çalışma alanı farklı bir abonelik ve mantıksal uygulama Azure Active Directory'den olabilir, ancak kolaylık sağlamak için aynı abonelikte olduklarından emin olan öneririz. Mantıksal uygulama günlük analizi için günlük analizi çalışma alanı kimliği ve anahtarı kullanarak gönderir.



## <a name="step-1---create-an-event-hub"></a>1. adım - bir olay hub'ı oluşturma

<!-- Follow the steps in [how to create an Event Hubs namespace and Event Hub](../event-hubs/event-hubs-create.md) to create your event hub. -->

1. Azure portalında seçin **yeni**> **nesnelerin interneti** > **olay hub'ları**.

   ![Market yeni olay hub'ı](media/log-analytics-activity-logs-subscriptions/marketplace-new-event-hub.png)

3. Altında **ad alanı oluşturma**, ya da yeni bir ad alanı veya mevcut bir seçerek girin. Adın kullanılabilirliği sistem tarafından hemen denetlenir.

   ![Olay hub'ı iletişim görüntüsü oluşturma](media/log-analytics-activity-logs-subscriptions/create-event-hub1.png)

4. Fiyatlandırma Katmanı (temel veya standart), bir Azure aboneliği, kaynak grubu ve yeni kaynak konumunu seçin.  Ad alanını oluşturmak için **Oluştur**’a tıklayın. Tam olarak kaynakları sağlamak üzere sistem için bir kaç dakika beklemeniz gerekebilir.
6. Listeden yeni oluşturduğunuz ad alanına tıklayın.
7. Seçin **paylaşılan erişim ilkeleri**ve ardından **RootManageSharedAccessKey**.

   ![Olay hub'ı paylaşılan erişim ilkeleri görüntüsü](media/log-analytics-activity-logs-subscriptions/create-event-hub7.png)
   
8. **RootManageSharedAccessKey** bağlantı dizesini panoya kopyalamak için kopyala düğmesine tıklayın. 

   ![Olay hub'ı paylaşılan erişim anahtarı görüntüsü](media/log-analytics-activity-logs-subscriptions/create-event-hub8.png)

9. Not Defteri gibi bir geçici konuma olay hub'ı adı ve her iki birincil veya ikincil olay Hub bağlantı dizesine kopyasını tutun. Mantıksal uygulama bu değerleri gerektirir.  Olay hub'ı bağlantı dizesi için kullandığınız **RootManageSharedAccessKey** bağlantı dizesi veya ayrı bir tane oluşturun.  Kullandığınız bağlantı dizesi ile başlamalıdır `Endpoint=sb://` ve sahip bir ilke için **Yönet** erişim ilkesi.


## <a name="step-2---export-activity-logs-to-event-hub"></a>2. adım - verme etkinlik günlükleri Olay Hub'ına

Etkinlik günlüğü akışını etkinleştirmek için bir olay hub'ı Namespace ve bu ad alanı için bir paylaşılan erişim ilkesi seçin. İlk yeni etkinlik günlüğü olay gerçekleştiğinde bir olay hub'ı bu ad alanında oluşturulur. 

Abonelikler aynı Azure Active Directory'de olmalıdır ancak günlükleri, yayma abonelik ile aynı abonelikte değil bir olay hub'ı ad alanını kullanabilirsiniz. Ayar yapılandıran kullanıcı her iki aboneliğin erişmek için uygun RBAC olması gerekir. 

1. Azure portalında seçin **İzleyici** > **etkinlik günlüğü**.
3. Tıklatın **verme** sayfanın üstündeki düğmesi.

   ![Gezinti azure izleyicisinde görüntüsü](media/log-analytics-activity-logs-subscriptions/activity-log-blade.png)

4. Seçin **abonelik** dışarı aktarmanız ve ardından **Tümünü Seç** içinde **bölgeleri** olayları kaynaklar için tüm bölgelerde seçmek için açılır. Tıklatın **verme olay hub'ına** onay kutusu.
7. Tıklatın **hizmet veri yolu ad alanı**ve ardından **abonelik** olay hub'ı ile **olay hub'ı ad**ve bir **olay hub'ı ilke adı**.

    ![Olay hub'ı sayfasına aktarma işlemi görüntüsü](media/log-analytics-activity-logs-subscriptions/export-activity-log2.png)

11. Tıklatın **Tamam** ve ardından **kaydetmek** bu ayarları kaydetmek için. Ayarları olan hemen uygulanması aboneliğinize.

<!-- Follow the steps in [stream the Azure Activity Log to Event Hubs](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md) to configure a log profile that writes activity logs to an event hub. -->

## <a name="step-3---create-logic-app"></a>3. adım - mantıksal uygulama oluşturma

Etkinlik günlükleri Olay hub'ına yazma sonra olay hub'ından günlükleri toplamak ve günlük Analizi'ne yazmak için bir mantıksal uygulama oluşturun.

Mantıksal uygulama aşağıdakileri içerir:
- Bir [olay hub'ı bağlayıcı](https://docs.microsoft.com/connectors/eventhubs/) olay Hub'ından okumak için tetikleyici.
- A [ayrıştırma JSON eylem](../logic-apps/logic-apps-content-type.md) JSON olayları ayıklayın.
- A [eylem oluşturma](../logic-apps/logic-apps-workflow-actions-triggers.md#compose-action) JSON nesneye dönüştürülemiyor.
- A [günlük analizi veri bağlayıcısı Gönder](https://docs.microsoft.com/connectors/azureloganalyticsdatacollector/) için günlük analizi veri göndermek için.

   ![Olay hub'ı tetikleyicisi logic apps içinde ekleme resmi](media/log-analytics-activity-logs-subscriptions/log-analytics-logic-apps-activity-log-overview.png)

### <a name="logic-app-requirements"></a>Mantıksal uygulama gereksinimleri
Mantıksal uygulamanızı oluşturmadan önce aşağıdaki bilgiler önceki adımlardan bulunduğundan emin olun:
- Olay Hub'ı adı
- Olay Hub bağlantı dizesine (birincil veya ikincil) olay hub'ı ad alanı için.
- Günlük analizi çalışma alanı kimliği
- Günlük analizi paylaşılan anahtar

Olay Hub adını ve bağlantı dizesini almak için adımları [olay hub'ı denetleyin ad alanı izinlerinin ve bulma bağlantı dizesi](../connectors/connectors-create-api-azure-event-hubs.md#check-event-hubs-namespace-permissions-and-find-the-connection-string).


### <a name="create-a-new-blank-logic-app"></a>Yeni ve boş bir mantıksal uygulama oluşturma

1. Azure portalında seçin **kaynak oluşturma** > **Kurumsal tümleştirme** > **mantıksal uygulama**.

    ![Market yeni mantıksal uygulama](media/log-analytics-activity-logs-subscriptions/marketplace-new-logic-app.png)

2. Aşağıdaki tabloda ayarlarını girin.

    ![Mantıksal uygulama oluşturma](media/log-analytics-activity-logs-subscriptions/create-logic-app.png)

   |Ayar | Açıklama  |
   |:---|:---|
   | Ad           | Mantıksal uygulama için benzersiz bir ad. |
   | Abonelik   | Mantıksal uygulama içerecek Azure aboneliğini seçin. |
   | Kaynak Grubu | Varolan bir Azure kaynak grubu seçin veya mantıksal uygulama için yeni bir tane oluşturun. |
   | Konum       | Mantıksal uygulamanızın dağıtılacağı veri merkezi bölgesini seçin. |
   | Log Analytics  | Günlük analizi mantıksal uygulamanızı her çalışması durumunu oturum isteyip istemediğinizi seçin.  |

    
3. **Oluştur**’u seçin. Zaman **dağıtımı başarılı oldu** bildirim görüntüler tıklayın **kaynağa gidin** mantıksal uygulamanızı açın.

4. **Şablonlar** bölümünde **Boş Mantıksal Uygulama**'yı seçin. 

Logic Apps Tasarımcısı'nı şimdi, kullanılabilir bağlayıcılar ve mantığı uygulama iş akışını başlatmak için kullandığınız kendi Tetikleyicileri gösterir.

<!-- Learn [how to create a logic app](../logic-apps/quickstart-create-first-logic-app-workflow.md). -->

### <a name="add-event-hub-trigger"></a>Olay hub'ı tetikleyicisi ekleyin

1. Mantıksal Uygulama Tasarımcısı için arama kutusuna yazın *olay hub'ları* filtreniz için. Tetikleyici seçin **olay olayların Event Hub'ında kullanılabilir olduğunda hub -**.

   ![Olay hub'ı tetikleyicisi logic apps içinde ekleme resmi](media/log-analytics-activity-logs-subscriptions/logic-apps-event-hub-add-trigger.png)

2. Kimlik bilgileri istendiğinde, olay hub'ları ad alanınıza bağlanın. Bağlantınızı ve kopyaladığınız bağlantı dizesi için bir ad girin.  **Oluştur**’u seçin.

   ![Olay hub bağlantısı logic apps içinde ekleme resmi](media/log-analytics-activity-logs-subscriptions/logic-apps-event-hub-add-connection.png)

3. Bağlantı oluşturduktan sonra tetikleyici ayarlarını düzenleyin. Başlangıç seçerek **Öngörüler işletimsel-günlükleri** gelen **olay hub'ı adı** açılır.

   ![Olayları kullanılabilir iletişim olduğunda](media/log-analytics-activity-logs-subscriptions/logic-apps-event-hub-read-events.png)

5. Genişletme **Gelişmiş Seçenekleri Göster** değiştirip **içerik türü** için *uygulama/json*

   ![Olay hub'ı yapılandırma iletişim kutusu](media/log-analytics-activity-logs-subscriptions/logic-apps-event-hub-configuration.png)

### <a name="add-parse-json-action"></a>Ayrıştırma JSON Eylem Ekle

Olay hub'ı çıkışı bir JSON yükü kayıtları bir dizisini içerir. [Ayrıştırma JSON](../logic-apps/logic-apps-content-type.md) eylem kayıtları için günlük analizi göndermek için yalnızca bir dizi ayıklamak için kullanılır.

1. Tıklatın **yeni adım** > **Eylem Ekle**
2. Arama kutusuna *json ayrıştırma* filtreniz için. Bir eylem seçin **veri işlemleri - ayrıştırma JSON**.

   ![Logic apps içinde Parse json eylem ekleme](media/log-analytics-activity-logs-subscriptions/logic-apps-add-parse-json-action.png)

3. ' I tıklatın **içerik** alan ve ardından *gövde*.

4. Kopyalama ve yapıştırma aşağıdaki şemaya **şema** alan.  Bu şemayı olay hub'ı eylem çıkışı eşleşir.  

   ![JSON iletişim ayrıştırılamıyor](media/log-analytics-activity-logs-subscriptions/logic-apps-parse-json-configuration.png)

``` json-schema
{
    "properties": {
        "body": {
            "properties": {
                "ContentData": {
                    "type": "string"
                },
                "Properties": {
                    "properties": {
                        "ProfileName": {
                            "type": "string"
                        },
                        "x-opt-enqueued-time": {
                            "type": "string"
                        },
                        "x-opt-offset": {
                            "type": "string"
                        },
                        "x-opt-sequence-number": {
                            "type": "number"
                        }
                    },
                    "type": "object"
                },
                "SystemProperties": {
                    "properties": {
                        "EnqueuedTimeUtc": {
                            "type": "string"
                        },
                        "Offset": {
                            "type": "string"
                        },
                        "PartitionKey": {},
                        "SequenceNumber": {
                            "type": "number"
                        }
                    },
                    "type": "object"
                }
            },
            "type": "object"
        },
        "headers": {
            "properties": {
                "Cache-Control": {
                    "type": "string"
                },
                "Content-Length": {
                    "type": "string"
                },
                "Content-Type": {
                    "type": "string"
                },
                "Date": {
                    "type": "string"
                },
                "Expires": {
                    "type": "string"
                },
                "Location": {
                    "type": "string"
                },
                "Pragma": {
                    "type": "string"
                },
                "Retry-After": {
                    "type": "string"
                },
                "Timing-Allow-Origin": {
                    "type": "string"
                },
                "Transfer-Encoding": {
                    "type": "string"
                },
                "Vary": {
                    "type": "string"
                },
                "X-AspNet-Version": {
                    "type": "string"
                },
                "X-Powered-By": {
                    "type": "string"
                },
                "x-ms-request-id": {
                    "type": "string"
                }
            },
            "type": "object"
        }
    },
    "type": "object"
}
```

>[!TIP]
> Tıklayarak bir örnek yükü alabilirsiniz **çalıştırmak** ve bakarak **ham çıktı** olay hub'dan.  Daha sonra bu çıkışı kullanabilirsiniz **şema üretmek için kullanım örnek yük** içinde **ayrıştırma JSON** şema oluşturmak için etkinlik.

### <a name="add-compose-action"></a>Oluşturma Eylem Ekle
[Oluşturma](../logic-apps/logic-apps-workflow-actions-triggers.md#compose-action) eylem JSON çıktısını alır ve günlük analizi eylemi tarafından kullanılabilen bir nesne oluşturur.

1. Tıklatın **yeni adım** > **Eylem Ekle**
2. Tür *oluşturan* filtre ve ardından eylemi için **veri işlemleri - oluşturan**.

    ![Oluşturma Eylem Ekle](media/log-analytics-activity-logs-subscriptions/logic-apps-add-compose-action.png)

3. Tıklatın **girişleri** alan ve seçin **gövde** altında **ayrıştırma JSON** etkinlik.


### <a name="add-log-analytics-send-data-action"></a>Günlük analizi veri Gönder Eylem Ekle
[Azure günlük analizi Veri Toplayıcı](https://docs.microsoft.com/connectors/azureloganalyticsdatacollector/) eylem oluşturma eyleminin nesnesini alır ve günlük Analizi'ne gönderir.

1. Tıklatın **yeni adım** > **Eylem Ekle**
2. Tür *oturum analytics* filtre ve ardından eylemi için **Azure günlük analizi veri toplayıcı - veri Gönder**.

   ![Ekleme günlük analizi veri eylem logic apps içinde gönderin.](media/log-analytics-activity-logs-subscriptions/logic-apps-send-data-to-log-analytics-connector.png)

3. Bağlantınız için bir ad girin ve yapıştırın **çalışma alanı kimliği** ve **çalışma alanı anahtarı** günlük analizi çalışma alanınız için.  **Oluştur**’a tıklayın.

   ![Logic apps içinde günlük analizi bağlantısı ekleniyor](media/log-analytics-activity-logs-subscriptions/logic-apps-log-analytics-add-connection.png)

4. Bağlantı oluşturduktan sonra aşağıdaki tabloda ayarlarını düzenleyin. 

    ![Gönderme veri eylemini yapılandırın](media/log-analytics-activity-logs-subscriptions/logic-apps-send-data-to-log-analytics-configuration.png)

   |Ayar        | Değer           | Açıklama  |
   |---------------|---------------------------|--------------|
   |JSON istek gövdesi  | **Çıktı** gelen **oluşturma** eylem | Kayıtları oluşturma eylem gövdesinden alır. |
   | Özel günlük adı | AzureActivity | İçeri aktarılan verileri tutmak için günlük analizi oluşturmak için özel günlük tablonun adı. |
   | saat oluşturulan alanı | time | JSON alanı için seçmeyin **zaman** -yalnızca word süreyi yazın. JSON alanı seçerseniz Tasarımcısı koyar **veri Gönder** eyleme bir *her* istemezsiniz olduğu döngü. |




10. Tıklatın **kaydetmek** mantıksal uygulamanızı yaptığınız değişiklikleri kaydedin.

## <a name="step-4---test-and-troubleshoot-the-logic-app"></a>Adım 4 - Test ve mantıksal uygulama sorunlarını giderme
İş akışı ile tam, hatasız çalıştığını doğrulamak için Tasarımcısı'nda test edebilirsiniz.

Mantıksal Uygulama Tasarımcısı'nda tıklayın **çalıştırmak** mantıksal uygulama test etmek için. Mantıksal uygulama her adımda başarıyı gösteren yeşil bir daire içinde beyaz onay işareti olan bir durum simgesi gösterir.

   ![Test mantıksal uygulama](media/log-analytics-activity-logs-subscriptions/test-logic-app.png)

Her adım hakkında ayrıntılı bilgi için genişletmek için adım adına tıklayın. Tıklayın **Göster ham girişleri** ve **Göster ham çıkışları** alınan ve her adımda gönderilen veriler üzerinde daha fazla bilgi için.

## <a name="step-5---view-azure-activity-log-in-log-analytics"></a>Adım 5 - günlük analizi Azure Etkinlik Günlüğü Görüntüle
Veri beklendiği gibi olarak toplanmakta emin olmak için günlük analizi çalışma alanı denetlemek için son adımdır bakın.

1. Azure portalında seçin **günlük analizi**.
2. Çalışma alanınızı seçin ve ardından **günlük arama** döşeme.
3. Arama sorgusu çubuğuna `AzureActivity_CL` ve arama düğmesini tıklatın. Özel günlük adında kaydetmedi *AzureActivity*, seçtiğiniz adı yazın ve ilave `_CL`.

>[!NOTE]
> Yeni bir özel günlüğü için günlük analizi gönderilen ilk kez, aranabilir olması özel günlük için bir saat sürebilir.

>[!NOTE]
> Etkinlik günlükleri için özel bir tablosu yazılır ve içinde gösterme [etkinlik günlüğü çözüm](./log-analytics-activity.md).


![Test mantıksal uygulama](media/log-analytics-activity-logs-subscriptions/log-analytics-results.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir olay Hub'ından Azure etkinlik günlükleri okumak ve bunları analiz için günlük analizi göndermek için bir mantıksal uygulama oluşturdunuz. Panolar, oluşturma dahil günlük analizi veri görselleştirme hakkında daha fazla bilgi için öğretici Görselleştir veri için gözden geçirin.

> [!div class="nextstepaction"]
> [Günlük arama veri öğretici Görselleştirme](./log-analytics-tutorial-dashboards.md)
