---
title: Azure tablo | Microsoft Docs
description: Azure tablosu için sağlama yönetimi yapılandırın.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: pbutlerm
ms.openlocfilehash: 60e3e3d81b07bf7ae681b5cef2d6d9681877a35f
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48811795"
---
<a name="lead-management-instructions-for-azure-table"></a>Azure tablosu için yönetim yönergeleri sağlama
============================================

Bu makalede, Azure tablo satışa depolamak için yapılandırma açıklanır. Azure tablo depolamak ve müşteri bilgileri özelleştirmenize olanak sağlar.

## <a name="to-configure-azure-table"></a>Azure tablo yapılandırmak için

1.  Bir Azure hesabınız yoksa, şunları yapabilirsiniz [ücretsiz bir deneme hesabı oluşturma](https://azure.microsoft.com/pricing/free-trial/).

2.  Azure hesabınız etkinleştikten sonra oturum [Azure portalında](https://portal.azure.com).
3.  Azure portalında bir depolama hesabı oluşturun. Sonraki ekran yakalama, bir depolama hesabı oluşturma işlemi gösterilmektedir. Depolama fiyatlandırması hakkında daha fazla bilgi için bkz. [depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).

    ![Bir Azure depolama hesabı oluşturma adımları](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragecreate.png)

4.  Depolama hesabı bağlantı dizesi anahtarı kopyalayın ve yapıştırın **depolama hesabı bağlantı dizesi** bulut iş ortağı portalı ile sekmesindeki. Bir bağlantı dizesi örneğidir `DefaultEndpointsProtocol=https;AccountName=myAccountName;AccountKey=myAccountKey;EndpointSuffix=core.windows.net `
    
    ![Azure depolama anahtarı](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragekeys.png)

Kullanabileceğiniz [Azure Depolama Gezgini](http://azurestorageexplorer.codeplex.com/) veya depolama Tablonuzdaki verileri görmek için başka bir araç. Ayrıca, Azure tablo verileri dışarı aktarabilirsiniz.
veriler.

## <a name="optional-to-use-azure-functions-with-an-azure-table"></a>**(İsteğe bağlı)**  Azure işlevleri ile bir Azure tablosu kullanma

Müşteri adayları, kullanım nasıl alıyorsunuz özelleştirmek istiyorsanız [Azure işlevleri](https://azure.microsoft.com/services/functions/) ile bir Azure tablosu. Azure işlevleri hizmet sağlama oluşturma işlemi otomatikleştirmenizi sağlar.

Aşağıdaki adımları, Zamanlayıcı kullanan bir Azure işlevi oluşturma işlemi gösterilmektedir. Beş dakikada işlevi yeni kayıtlar için Azure tablo arar ve ardından bir e-posta bildirimi göndermek için SendGrid hizmetini kullanır.


1.  [Oluşturma](https://portal.azure.com/#create/SendGrid.SendGrid) Azure aboneliğinizdeki ücretsiz SendGrid hizmet hesabı.

    ![SendGrid oluşturma](./media/cloud-partner-portal-lead-management-instructions-azure-table/createsendgrid.png)

2.  SendGrid API anahtarı oluşturma 
    - Seçin **Yönet** SendGrid kullanıcı Arabirimine gidin
    - Seçin **ayarları**, **API anahtarları**ve ardından e-posta Gönder - bir anahtar oluşturun\> tam erişim
    - API anahtarı Kaydet


    ![SendGrid API anahtarı](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridkey.png)


3.  [Oluşturma](https://portal.azure.com/#create/Microsoft.FunctionApp) barındırma planı seçeneğini kullanarak bir Azure işlev uygulaması adı "Tüketim planı".

    ![Azure işlev uygulaması oluşturma](./media/cloud-partner-portal-lead-management-instructions-azure-table/createfunction.png)


4.  Yeni bir işlev tanımı oluşturun.

    ![Azure işlev tanımı oluşturma](./media/cloud-partner-portal-lead-management-instructions-azure-table/createdefinition.png)
 

5.  Belirli bir zaman üzerinde güncelleştirme göndermek için işlev ulaşmak için **TimerTrigger-CSharp** başlangıç seçeneği olarak.

     ![Azure işlevi zaman tetikleyici seçeneği](./media/cloud-partner-portal-lead-management-instructions-azure-table/timetrigger.png)


6.  "Geliştirme" kodu aşağıdaki kod örneği ile değiştirin. Gönderen ve alıcı için kullanmak istediğiniz adresleri e-posta adresleriyle düzenleyin.

        #r "Microsoft.WindowsAzure.Storage"
        #r "SendGrid"
        using Microsoft.WindowsAzure.Storage.Table;
        using System;
        using SendGrid;
        using SendGrid.Helpers.Mail;
        public class MyRow : TableEntity
        {
            public string Name { get; set; }
        }
        public static void Run(TimerInfo myTimer, IQueryable<MyRow> inputTable, out Mail message, TraceWriter log)
        {
            // UTC datetime that is 5.5 minutes ago while the cron timer schedule is every 5 minutes
            DateTime dateFrom = DateTime.UtcNow.AddSeconds(-(5 * 60 + 30));
            var emailFrom = "YOUR EMAIL";
            var emailTo = "YOUR EMAIL";
            var emailSubject = "Azure Table Notification";
            // Look in the table for rows that were added recently
            var rowsList = inputTable.Where(r => r.Timestamp > dateFrom).ToList();
            // Check how many rows were added
            int rowsCount = rowsList.Count;
            if (rowsCount > 0)
            {
                log.Info($"Found {rowsCount} rows added since {dateFrom} UTC");
                // Configure the email message describing how many rows were added
                message = new Mail
                {
                    From = new Email(emailFrom),
                    Subject = emailSubject + " (" + rowsCount + " new rows)"
                };
                var personalization = new Personalization();
                personalization.AddTo(new Email(emailTo));
                message.AddPersonalization(personalization);
                var content = new Content
                {
                    Type = "text/plain",
                    Value = "Found " + rowsCount + " new rows added since " + dateFrom.ToString("yyyy-MM-dd HH:mm:ss") + " UTC"
                };
                message.AddContent(content);
            }
            else
            {
                // Do not send the email if no new rows were found
                message = null;
            }
        }

    ![Azure işlevi kod parçacığı](./media/cloud-partner-portal-lead-management-instructions-azure-table/code.png)


7.  Seçin **tümleştir** ve **girişleri** Azure tablo bağlantı tanımlamak için.

    ![Azure işlevi tümleştirin](./media/cloud-partner-portal-lead-management-instructions-azure-table/integrate.png)


8.  Tablo adını girin ve seçerek bağlantı dizesi tanımlama **yeni**.


    ![Azure işlevi tablo bağlantısı](./media/cloud-partner-portal-lead-management-instructions-azure-table/configtable.png)

9.  Şimdi çıkış SendGrid tanımlayın ve tüm varsayılanları tutun.

    ![SendGrid çıkış](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridoutput.png)

    ![SendGrid çıkış Varsayılanları](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridoutputdefaults.png)

10. SendGrid API anahtarı ve adını "SendGridApiKey" SendGrid kullanıcı arabiriminde API anahtarları alınan değeri kullanarak bir işlev uygulaması ayarları ekleyin

    ![SendGrid yönetme](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridmanage.png)
    ![SendGrid anahtarı yönetme](./media/cloud-partner-portal-lead-management-instructions-azure-table/sendgridmanagekey.png)

İşlev yapılandırma bitirdikten sonra tümleştir bölümünde kod aşağıdaki gibi görünmelidir.

    {
      "bindings": [
        {
          "name": "myTimer",
          "type": "timerTrigger",
          "direction": "in",
          "schedule": "0 */5 * * * *"
        },
        {
          "type": "table",
          "name": "inputTable",
          "tableName": "MarketplaceLeads",
          "take": 50,
          "connection": "yourstorageaccount_STORAGE",
          "direction": "in"
        },
        {
          "type": "sendGrid",
          "name": "message",
          "apiKey": "SendGridApiKey",
          "direction": "out"
        }
      ],
      "disabled": false
    }

11. İşlev için geliştirme UI gidin ve sonra son adımdır **çalıştırma** zamanlayıcıyı başlatmak için. Şimdi yeni bir müşteri adayı geldiğinde her zaman bir bildirim alırsınız.
