---
title: Azure Data Box portalı yönetici kılavuzu | Microsoft Docs
description: Azure portalı kullanarak Azure Data Box'ınızı nasıl yönetebileceğiniz açıklanmaktadır.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: overview
ms.date: 10/19/2018
ms.author: alkohli
ms.openlocfilehash: 1b228a66f2d59b3ff252df266783f7bd5d27139e
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49645448"
---
# <a name="use-the-azure-portal-to-administer-your-data-box"></a>Azure portalı kullanarak Data Box’ınızı yönetme

Bu makalede Data Box ile gerçekleştirilebilen bazı karmaşık iş akışları ve yönetim görevleri anlatılmaktadır. Data Box’ı Azure portal veya yerel web kullanıcı arabirimiyle yönetebilirsiniz. 

Bu makale, Azure portalı kullanarak gerçekleştirebileceğiniz görevlere odaklanmaktadır. Azure portalı kullanarak siparişleri yönetebilir, Data Box’ı yönetebilir ve siparişin durumunu tamamlanana kadar takip edebilirsiniz.


## <a name="cancel-an-order"></a>Siparişi iptal etme

Siparişinizi verdikten sonra çeşitli nedenlerle iptal etmeniz gerekebilir. Siparişi yalnızca sipariş işleme alınmadan önce iptal edebilirsiniz. Sipariş işleme alınıp Data Box hazırlandığında siparişi iptal etmeniz mümkün değildir. 

Bir siparişi iptal etmek için aşağıdaki adımları gerçekleştirin.

1.  **Genel bakış > İptal**'e gidin. 

    ![Siparişi iptal etme 1](media/data-box-portal-admin/cancel-order1.png)

2.  Sipariş iptal etme nedenini belirtin.  

    ![Siparişi iptal etme 2](media/data-box-portal-admin/cancel-order2.png)

3.  Sipariş iptal edildikten sonra portaldaki durumu **İptal edildi** olarak görüntülenir. 

## <a name="clone-an-order"></a>Siparişi kopyalama

Kopyalama belirli durumlarda kullanışlıdır. Örneğin kullanıcı, veri aktarımı için daha önceden Data Box kullanmıştır. Yeni veriler üretildikçe Azure'a aktarmak için diğer bir Data Box’a daha ihtiyaç duyulur. Bu durumda aynı sipariş kopyalanabilir.

Siparişi kopyalamak için aşağıdaki adımları gerçekleştirin.

1.  **Genel bakış > Kopyala**'ya gidin. 

    ![Siparişi kopyalama 1](media/data-box-portal-admin/clone-order1.png)

2.  Siparişin tüm ayrıntıları aynı şekilde korunur. Siparişin adı, özgün siparişin adına *-Kopya* eklenerek oluşturulur. Gizlilik bilgilerini gözden geçirdiğinizi onaylamak için onay kutusunu seçin. **Oluştur**’a tıklayın.

Kopya sipariş birkaç dakikada oluşturulur ve portal yeni siparişi gösterecek şekilde güncelleştirilir.


## <a name="delete-order"></a>Siparişi silme

Tamamlanan siparişleri silmek isteyebilirsiniz. Siparişte adınız, adresiniz ve iletişim bilgileriniz gibi kişisel bilgileriniz yer alır. Sipariş silindiğinde bu kişisel bilgiler de silinir.

Yalnızca tamamlanan veya iptal edilen siparişleri silebilirsiniz. Siparişi silmek için aşağıdaki adımları gerçekleştirin.

1. **Tüm kaynaklar**'a gidin. Siparişinizi arayın.

2. Silmek istediğiniz siparişe tıklayın ve **Genel bakış**'a gidin. Komut çubuğundan **Sil**'e tıklayın.

    ![Data Box siparişini silme 1](media/data-box-portal-admin/delete-order1.png)

3. Siparişi silme işlemini onaylamanız istendiğinde siparişin adını girin. **Sil**'e tıklayın.

## <a name="download-shipping-label"></a>Sevkiyat etiketini indirme

Data Box’unuzun E-ink ekranı çalışmıyorsa ve iade gönderimi etiketini göstermiyorsa gönderim etiketini indirmeniz gerekebilir. 

Sevkiyat etiketini indirmek için aşağıdaki adımları gerçekleştirin.

1.  **Genel bakış > Sevkiyat etiketi indir** bölümüne gidin. Bu seçenek yalnızca cihaz gönderildikten sonra kullanılabilir. 

    ![Sevkiyat etiketini indirme](media/data-box-portal-admin/download-shipping-label.png)

2.  Aşağıda gösterilen iade sevkiyat etiketi indirilir. Etiketi kaydedin ve yazdırın. Etiketi katlayıp cihazdaki şeffaf kılıfa koyun. Etiketin görünür olduğundan emin olun. Cihazdaki önceki gönderimden kalan tüm çıkartmaları sökün.

    ![Örnek sevkiyat etiketi](media/data-box-portal-admin/example-shipping-label.png)

## <a name="edit-shipping-address"></a>Teslimat adresini düzenleme

Siparişi verdikten sonra teslimat adresini düzenlemeniz gerekebilir. Bu işlem yalnızca cihaz yola çıkana kadar kullanılabilir. Cihaz yola çıktıktan sonra bu seçenek kullanılamaz.

Siparişi düzenlemek için aşağıdaki adımları gerçekleştirin.

1. **Sipariş ayrıntıları > Teslimat adresini düzenle**'ye gidin.

    ![Teslimat adresini düzenleme 1](media/data-box-portal-admin/edit-shipping-address1.png)

2. Gönderim adresini düzenleyin ve doğrulayın ve sonra değişiklikleri kaydedin.

    ![Teslimat adresini düzenleme 2](media/data-box-portal-admin/edit-shipping-address2.png)

## <a name="edit-notification-details"></a>Bildirim ayrıntılarını düzenleme

Sipariş durumu e-postalarının gönderilmesini istediğiniz kullanıcıları değiştirmek isteyebilirsiniz. Örneğin cihaz teslim edildiğinde veya alındığında bir kullanıcının bilgilendirilmesi gerekebilir. Veriler kaynaktan silinmeden önce Azure depolama hesabındaki verileri doğrulanması amacıyla veri kopyalama işlemi tamamlandığında başka bir kullanıcıya bildirim gönderilmesini isteyebilirsiniz. Bu gibi durumlarda bildirim ayrıntılarını düzenleyebilirsiniz.

Bildirim ayrıntılarını düzenlemek için aşağıdaki adımları gerçekleştirin.

1. **Sipariş ayrıntıları > Bildirim ayrıntılarını düzenle**'ye gidin.

    ![Bildirim ayrıntılarını düzenleme 1](media/data-box-portal-admin/edit-notification-details1.png)

2. Bu sayfada bildirim ayrıntılarını düzenleyebilir ve yaptığınız değişiklikleri kaydedebilirsiniz.
 
    ![Bildirim ayrıntılarını düzenleme 2](media/data-box-portal-admin/edit-notification-details2.png)


## <a name="download-order-history"></a>İndirme sırası geçmişi

Data Box siparişiniz tamamlandıktan sonra cihaz disklerindeki veriler silinir. Cihaz temizleme tamamlandığında, sipariş geçmişini Azure portalında indirebilirsiniz.

Sipariş geçmişini indirmek için aşağıdaki adımları uygulayın.

1. Data Box siparişinizde **Genel Bakış**'a gidin. Siparişin tamamlandığından emin olun. Sipariş ve cihaz temizleme tamamlandıysa, **Sipariş ayrıntıları**'na gidin. **Sipariş geçmişi indirme** seçeneği bulunur.

    ![Sipariş geçmişi indirme](media/data-box-portal-admin/download-order-history-1.png)

2. **Sipariş geçmişini indir**'e tıklayın. İndirilen geçmişte kurye takip günlüklerini bir kaydını görürsünüz. Bu günlüğün en altına giderseniz, aşağıdakilere giden bağlantıları görebilirsiniz:
    
    - **Kopyalama günlükleri** - Data Box'tan Azure depolama hesabınıza veri kopyalama sırasında hatalı çıkış veren dosyaların listesini içerir.
    - **Denetim günlükleri** - Azure veri merkezi dışında olduğunda Data Box üzerindeki açma ve paylaşın erişimi hakkında bilgi içerir.
    - **BOM dosyaları** - **Göndermeye hazırlama** sırasında indirebileceğiniz dosyaların listesini (dosya bildirimi olarak da bilinir) ve dosyaların adlarını, boyutlarını ve sağlama toplamlarını içerir.

        ```
        -------------------------------
        Microsoft Data Box Order Report
        -------------------------------
        
        Name                                               : eastusdryrun                                      
        StartTime(UTC)                                     : 9/6/2018 12:54:47 PM +00:00                       
        DeviceType                                         : ImolaPod                                          
        
        -------------------
        Data Box Activities
        -------------------
        
        Time(UTC)             | Activity                       | Status          | Description                                                                                                                                           
        
        9/6/2018 12:54:51 PM  | OrderCreated         | Completed  |                                                                                                                              
        9/11/2018 8:57:38 PM  | DevicePrepared       | Completed  |                                                                                                                                                       
        9/12/2018 7:28:15 PM  | ShippingToCustomer   | InProgress | Pickup Scan. Local Time : 9/12/2018 2:52:31 PM at Chantilly                                                                                           
        9/13/2018 2:33:04 AM  | ShippingToCustomer   | InProgress | Departure Scan. Local Time : 9/12/2018 9:00:00 PM at Chantilly                                                                                                                                                                                                                                                              
        9/13/2018 12:40:31 PM | ShippingToCustomer   | InProgress | Arrival Scan. Local Time : 9/13/2018 5:00:00 AM at Oakland                                                                                            
        9/13/2018 2:42:10 PM  | ShippingToCustomer   | InProgress | Departure Scan. Local Time : 9/13/2018 6:08:00 AM at Oakland                                                                                          
        9/13/2018 3:42:12 PM  | ShippingToCustomer   | InProgress | Destination Scan. Local Time : 9/13/2018 8:14:08 AM at Sunnyvale                                                                                      
        9/13/2018 4:43:05 PM  | ShippingToCustomer   | InProgress | Destination Scan. Local Time : 9/13/2018 8:56:54 AM at Sunnyvale                                                                                      
        9/13/2018 4:43:05 PM  | ShippingToCustomer   | InProgress | Out For Delivery Today. Local Time : 9/13/2018 9:11:21 AM at Sunnyvale                                                                                
        9/13/2018 5:43:07 PM  | ShippingToCustomer   | Completed  | Delivered. Local Time : 9/13/2018 9:44:17 AM at SUNNYVALE                                                                                             
        9/14/2018 11:48:35 PM | ShippingToDataCenter | InProgress | Pickup Scan. Local Time : 9/14/2018 3:55:37 PM at Sunnyvale                                                                                                                                                                                 
        9/15/2018 1:52:35 AM  | ShippingToDataCenter | InProgress | Arrival Scan. Local Time : 9/14/2018 6:31:00 PM at San Jose                                                                                           
        9/15/2018 2:52:39 AM  | ShippingToDataCenter | InProgress | Departure Scan. Local Time : 9/14/2018 7:17:00 PM at San Jose                                                                                                                                                                             
        9/17/2018 8:23:31 AM  | ShippingToDataCenter | InProgress | Destination Scan. Local Time : 9/17/2018 4:14:37 AM at Chantilly                                                                                      
        9/17/2018 12:24:42 PM | ShippingToDataCenter | InProgress | Loaded on Delivery Vehicle. Local Time : 9/17/2018 7:45:36 AM at Chantilly                                                                            
        9/17/2018 1:25:11 PM  | ShippingToDataCenter | InProgress | Out For Delivery Today. Local Time : 9/17/2018 8:27:11 AM at Chantilly                                                                                
        9/17/2018 2:25:51 PM  | ShippingToDataCenter | Completed | Delivered. Local Time : 9/17/2018 9:56:32 AM at STERLING                                                                                              
        9/18/2018 9:55:41 PM  | DeviceBoot           | Completed | Appliance booted up successfully                                                                                                                      
        9/18/2018 11:00:25 PM | DataCopy             | Started   |                                                                                                                                                       
        9/18/2018 11:01:33 PM | DataCopy             | Completed | Copy Completed.                                                                                                                                       
        9/18/2018 11:20:58 PM | SecureErase          | Started   |                                                                                                                                                       
        9/18/2018 11:28:46 PM | SecureErase          | Completed | Azure Data Box:BY506B4B616700 has been sanitized according to NIST 800 -88 Rev 1.                                                                     
        
        ----------------------
        Data Box Job Log Links
        ----------------------
        
        Account Name         : eastusdryrun                                         
        Copy Logs Path       : copylog/copylogd695869a2a294396b7b903296c208388.xml                                                                                                                                                     
        Audit Logs Path      : azuredatabox-chainofcustodylogs\3b4cf163-f1af-475c-a391-f8afea3fa327\by506b4b616700                                                                                                                     
        BOM Files Path       : azuredatabox-chainofcustodylogs\3b4cf163-f1af-475c-a391-f8afea3fa327\by506b4b616700
        ```
Daha sonra depolama hesabınıza gidebilir ve kopyalama günlüklerini görüntüleyebilirsiniz.

![Depolama hesaplarındaki günlükler](media/data-box-portal-admin/logs-in-storage-acct-2.png)

Ayrıca denetim günlüklerini ve BOM dosyalarını içeren gözetim günlükleri zincirini de görüntüleyebilirsiniz.

![Depolama hesaplarındaki günlükler](media/data-box-portal-admin/logs-in-storage-acct-1.png)

## <a name="view-order-status"></a>Sipariş durumunu görüntüleme

Cihaz durumu portalda değiştiğinde bu, size e-posta ile bildirilir.

|Sipariş durumu |Açıklama |
|---------|---------|
|Sipariş edildi     | Sipariş başarıyla oluşturuldu. <br>Cihaz kullanılabilir durumdaysa Microsoft tarafından gönderilecek cihaz belirlenir ve cihaz hazırlanır. <br> Cihaz o anda mevcut değilse sipariş cihaz mevcut olduğunda işleme alınır. Siparişin işleme alınması birkaç gün ile birkaç ay sürebilir. Sipariş 90 gün içinde gerçekleştirilemiyorsa sipariş iptal edilir ve bu size bildirilir.         |
|İşlendi     | Siparişin işlenmesi tamamlandı. Siparişiniz uyarınca cihaz gönderilmek üzere veri merkezinde hazırlanır.         |
|Yola çıktı     | Sipariş sevk edildi. Gönderiyi takip etmek için portalda, siparişinizde görüntülenen takip kimliğini kullanın.        |
|Teslim Edildi     | Gönderim, belirtilen adrese teslim edildi.        |
|Teslim alındı     |İade gönderiniz teslim alındı ve kurye tarafından tarandı.         |
|Alındı     | Cihazınız alındı ve Azure veri merkezinde tarandı. <br> Gönderi incelendikten sonra cihaz karşıya yüklemesi başlar.      |
|Veri kopyalama     | Veri kopyalama işlemi devam ediyor. Azure portal’da siparişinizin kopyalama ilerleme durumunu takip edin. <br> Veri kopyalama işlemi tamamlanana kadar bekleyin. |
|Tamamlandı       |Sipariş başarıyla tamamlandı.<br> Şirket içi verilerini sunuculardan silmeden önce verilerinizin Azure’a kopyalandığından emin olun.         |
|Hatalarla tamamlandı| Veri kopyalama tamamlandı ancak kopyalama sırasında hatalar oluştu. <br> Azure portalda belirtilen yolu kullanarak kopyalama günlüklerini gözden geçirin.   |
|İptal edildi            |Sipariş iptal edildi. <br> Siparişi iptal ettiniz veya bir hatayla karşılaşıldı ve sipariş, hizmet tarafından iptal edildi. Sipariş 90 gün içinde gerçekleştirilemiyorsa sipariş iptal edilir ve bu size bildirilir.     |
|Temizleme | Cihaz sürücülerindeki veriler silinir. Cihaz temizleme; sipariş geçmişi Azure portalından indirilmeye hazır olduğunda tamamlanmış olarak değerlendirilir.|



## <a name="next-steps"></a>Sonraki adımlar

- [Data Box sorunlarını giderme](data-box-faq.md) hakkında bilgi edinin.
