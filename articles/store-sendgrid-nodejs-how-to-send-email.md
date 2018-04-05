---
title: SendGrid e-posta hizmetine (Node.js) kullanma | Microsoft Docs
description: Bilgi nasıl Azure üzerinde SendGrid e-posta hizmeti ile e-posta gönderin. Node.js API kullanılarak yazılan kod örnekleri.
services: ''
documentationcenter: nodejs
author: erikre
manager: wpickett
editor: ''
ms.assetid: cac444b4-26b0-45ea-9c3d-eca28d57dacb
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 01/05/2016
ms.author: erikre
ms.openlocfilehash: 327cea3a24cc47a9cc463b37cc2346ebc475ef7f
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="how-to-send-email-using-sendgrid-from-nodejs"></a>SendGrid node.js'den kullanarak e-posta gönderme
Bu kılavuz, Azure üzerinde SendGrid e-posta hizmeti ile genel programlama görevleri gerçekleştirmek gösterilmiştir. Örnekler, Node.js API kullanılarak yazılır. Kapsamdaki senaryolar dahil **e-posta oluşturma**, **e-posta gönderme**, **eklerini ekleme**, **filtreleri kullanarak**, ve **özelliklerini güncelleştirme**. SendGrid ve e-posta gönderme hakkında daha fazla bilgi için bkz: [sonraki adımlar](#next-steps) bölümü.

## <a name="what-is-the-sendgrid-email-service"></a>SendGrid e-posta hizmeti nedir?
SendGrid olan bir [bulut tabanlı e-posta hizmeti] güvenilir sağlayan [işleme uygun e-posta teslimi], ölçeklenebilirlik ve gerçek zamanlı analiz özel tümleştirme kolaylaştırmak esnek API'leri yanı sıra. Ortak SendGrid kullanım senaryoları şunları içerir:

* Giriş müşterileri için otomatik olarak gönderme
* Aylık e ilanları ve özel teklifler müşteriler göndermek için dağıtım yönetme listeler
* Engellenen e-posta ve müşteri yanıtlama gibi için gerçek zamanlı ölçümleri toplama
* Eğilimleri belirlemenize yardımcı olmak için rapor oluşturma
* Müşteri sorguları iletme
* Uygulamanızdan e-posta bildirimleri

Daha fazla bilgi için bkz. [https://sendgrid.com](https://sendgrid.com).

## <a name="create-a-sendgrid-account"></a>SendGrid hesabı oluşturma
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-nodejs-module"></a>SendGrid Node.js modülü başvurusu
SendGrid modül Node.js için aşağıdaki komutu kullanarak (npm) düğüm paketi Yöneticisi yüklenebilir:

    npm install sendgrid

Yükleme tamamlandıktan sonra aşağıdaki kodu kullanarak uygulamanızda modülü gerektirebilir:

    var sendgrid = require('sendgrid')(sendgrid_username, sendgrid_password);

SendGrid modülü aktarır **SendGrid** ve **e-posta** işlevleri.
**SendGrid** Web API aracılığıyla e-posta göndermekten sorumludur sırada **e-posta** bir e-posta iletisi yalıtır.

## <a name="how-to-create-an-email"></a>Nasıl yapılır: bir e-posta oluşturma
SendGrid modülü kullanılarak bir e-posta iletisi oluşturmak önce e-posta işlevini kullanarak ve SendGrid işlevi kullanarak gönderme bir e-posta iletisi oluşturma içerir. E-posta işlevi kullanarak yeni bir ileti oluşturma bir örnek verilmiştir:

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

Html özelliği ayarlanarak destek istemciler için bir HTML iletisi de belirtebilirsiniz. Örneğin:

    html: This is a sample <b>HTML<b> email message.

Metin ve html özellikleri ayarlama normal geri dönüş metin içeriği için HTML iletilerini destekleyemez istemcileri için sağlar.

E-posta işlevi tarafından desteklenen tüm özellikleri hakkında daha fazla bilgi için bkz: [sendgrid nodejs][sendgrid-nodejs].

## <a name="how-to-send-an-email"></a>Nasıl yapılır: bir e-posta Gönder
E-posta işlevini kullanarak bir e-posta iletisi oluşturduktan sonra SendGrid tarafından sağlanan Web API kullanarak gönderebilirsiniz. 

### <a name="web-api"></a>Web API
    sendgrid.send(email, function(err, json){
        if(err) { return console.error(err); }
        console.log(json);
    });

> [!NOTE]
> Yukarıdaki örneklerde bir e-posta nesne ve geri çağırma işlevi geçirme gösterirken, doğrudan e-posta özelliklerini belirterek gönderme işlevi doğrudan da çağırabilirsiniz. Örneğin:  
> 
> `````
> sendgrid.send({
> to: 'john@contoso.com',
> from: 'anna@contoso.com',
> subject: 'test mail',
> text: 'This is a sample email message.'
> });
> `````
> 
> 

## <a name="how-to-add-an-attachment"></a>Nasıl yapılır: bir eki ekleyin
Ekleri eklenebilir bir iletiye yollarda ve dosya adları belirterek **dosyaları** özelliği. Aşağıdaki örnek, bir ek gönderme gösterir:

    sendgrid.send({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.',
        files: [
            {
                filename:     '',           // required only if file.content is used.
                contentType:  '',           // optional
                cid:          '',           // optional, used to specify cid for inline content
                path:         '',           //
                url:          '',           // == One of these three options is required
                content:      ('' | Buffer) //
            }
        ],
    });

> [!NOTE]
> Kullanırken **dosyaları** özelliği, dosya olmalıdır üzerinden erişilebilir [fs.readFile](http://nodejs.org/docs/v0.6.7/api/fs.html#fs.readFile). Eklemek istediğiniz dosya olarak Azure Storage'da bir Blob kapsayıcısını olduğu gibi barındırılıyorsa kullanarak ek olarak gönderilmeden önce ilk dosyasını yerel depolama veya Azure sürücüye kopyalamanız gerekir **dosyaları** özelliği.
> 
> 

## <a name="how-to-use-filters-to-enable-footers-and-tracking"></a>Nasıl yapılır: etkinleştir altbilgiler ve izleme için filtreleri kullanın
SendGrid filtreleri kullanarak ek e-posta işlevselliği sağlar. Bu izleme'ye tıklayın, Google analytics, izleme abonelik ve benzeri etkinleştirme gibi belirli işlevleri etkinleştirmek için e-posta iletisine eklenecek ayarlardır. Filtrelerin tam bir listesi için bkz: [filtresi ayarlarını][Filter Settings].

Filtreler uygulanabilir bir iletiyi kullanarak **filtreleri** özelliği.
Her filtre filtre özgü ayarları içeren bir karma tarafından belirtilir.
Aşağıdaki örnekler altbilgi göstermek ve filtreleri İzleme'yi tıklatın:

### <a name="footer"></a>Alt bilgi
    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

    email.setFilters({
        'footer': {
            'settings': {
                'enable': 1,
                'text/plain': 'This is a text footer.'
            }
        }
    });

    sendgrid.send(email);

### <a name="click-tracking"></a>İzleme'yi tıklatın
    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

    email.setFilters({
        'clicktrack': {
            'settings': {
                'enable': 1
            }
        }
    });

    sendgrid.send(email);

## <a name="how-to-update-email-properties"></a>Nasıl yapılır: güncelleştirme e-posta özellikleri
Bazı e-posta özellikleri kullanılarak üzerine yazılabilir **ayarlamak * özellik*** veya kullanarak eklenmiş **ekleme*özelliği ***. Örneğin, kullanarak ek alıcılar ekleyebilirsiniz.

    email.addTo('jeff@contoso.com');

veya bir filtre kullanarak ayarlayın

    email.addFilter('footer', 'enable', 1);
    email.addFilter('footer', 'text/html', '<strong>boo</strong>');

Daha fazla bilgi için bkz: [sendgrid nodejs][sendgrid-nodejs].

## <a name="how-to-use-additional-sendgrid-services"></a>Nasıl yapılır: ek SendGrid Hizmetleri kullanın
SendGrid Azure uygulamanızı ek SendGrid işlevinden yararlanmak için kullanabileceğiniz web tabanlı API'ler sunar. Ayrıntılar için bkz: [SendGrid API belgelerine][SendGrid API documentation].

## <a name="next-steps"></a>Sonraki Adımlar
SendGrid e-posta hizmeti temel bilgileri öğrendiğinize göre daha fazla bilgi için aşağıdaki bağlantıları izleyin.

* SendGrid Node.js modülü deposu: [sendgrid nodejs][sendgrid-nodejs]
* SendGrid API belgelerine: <https://sendgrid.com/docs>
* SendGrid özel teklif Azure müşteriler için: [http://sendgrid.com/azure.html](https://sendgrid.com/windowsazure.html)

[special offer]: https://sendgrid.com/windowsazure.html
[sendgrid-nodejs]: https://github.com/sendgrid/sendgrid-nodejs
[Filter Settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
[SendGrid API documentation]: https://sendgrid.com/docs
[bulut tabanlı e-posta hizmeti]: https://sendgrid.com/email-solutions
[işleme uygun e-posta teslimi]: https://sendgrid.com/transactional-email
