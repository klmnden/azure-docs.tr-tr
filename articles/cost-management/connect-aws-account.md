---
title: Azure maliyeti Management'a Amazon Web Services hesabınız bağlanma | Microsoft Docs
description: Maliyet Yönetimi raporları maliyet ve kullanım verileri görüntülemek için bir Amazon Web Hizmetleri hesap bağlayın.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 04/06/2018
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: ''
ms.openlocfilehash: 109235718f085ea2912f601f0657e08230e3e91d
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="connect-an-amazon-web-services-account"></a>Amazon Web Services hesabınız Bağlan

Azure maliyeti yönetim Amazon Web Hizmetleri (AWS) hesabınıza bağlamak için iki seçeneğiniz vardır. Salt okunur bir IAM kullanıcı hesabını veya bir IAM rolü ile bağlanabilir. IAM rol temsilci erişimi güvenilir varlıklar için tanımlanan izinlerle izin verdiği için önerilir. IAM rol uzun süreli erişim anahtarları paylaşımı gerektirmez. AWS hesabınız için maliyet yönetim bağlandıktan sonra maliyet ve kullanım verileri Yönetimi maliyeti raporlarında kullanılabilir. Bu belge, her iki seçenek size yol gösterir.

AWS IAM kimlikler hakkında daha fazla bilgi için bkz: [kimlikleri (kullanıcıları, grupları ve rolleri)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html).

Ayrıntılı AWS etkinleştirmesini de raporları faturalama ve bir AWS Basit Depolama Birimi Hizmeti (S3) aralığındaki bilgileri depolar. Ayrıntılı Fatura raporları bir saatlik aralıklarla etiketi ve kaynak bilgileriyle fatura ücretleri içerir. Raporlarını depolama maliyeti, sepet almak ve onun raporlarında bilgileri görüntülemek yönetim sağlar.


## <a name="aws-role-based-access"></a>AWS rol tabanlı erişim

Aşağıdaki bölümlerde, maliyet yönetim erişim sağlamak için bir salt okunur IAM rolü oluşturmada size yol.

### <a name="get-your-cost-management-account-external-id"></a>Maliyet yönetim hesabı dış Kimliğinizi alın

İlk adım, Azure maliyeti Yönetim Portalı'ndan benzersiz bağlantı parola almaktır. AWS kullanılan **Dış kimlik**.

1. Azure portalından Cloudyn portalını açın veya gitmek [ https://azure.cloudyn.com ](https://azure.cloudyn.com) ve oturum açın.
2. Dişlisine simgesine tıklayın ve ardından **bulut hesapları**.
3. Hesapları Yönetimi'nde seçin **AWS hesapları** sekmesini ve sonra **yeni Ekle +**.
4. İçinde **AWS hesabı Ekle** iletişim, kopya **Dış kimlik** ve AWS oluşturma adımları sonraki bölümde rolü için değer kaydedin. Dış hesabınıza benzersiz kimliğidir. Aşağıdaki görüntüde, örnek dış kimliğidir _Contoso_ bir sayının. Kimliğinizi farklıdır.  
    ![Dış kimliği](./media/connect-aws-account/external-id.png)

### <a name="add-aws-read-only-role-based-access"></a>AWS salt okunur rol tabanlı erişim Ekle

1. AWS konsoluna oturum açın https://console.aws.amazon.com/iam/home seçip **rolleri**.
2. Tıklatın **Rol Oluştur** ve ardından **başka bir AWS hesap**.
3. İçinde **hesap kimliği** kutusunda, yapıştırma `432263259397`. Bu hesap kimliği, AWS tarafından Cloudyn hizmete atanan maliyet yönetim veri toplayıcı hesabıdır. Gösterilen tam hesap kimliği kullanın.
4. Yanına **seçenekleri**seçin **dış kimliği gerektiren**. Daha önce kopyalanan ve benzersiz değer Yapıştır **dış kimliği** maliyet Yönetimi'nde alan. Ardından **sonraki: izinleri**.  
    ![Rolü oluşturma](./media/connect-aws-account/create-role01.png)
5. Altında **izinleri ilkeleri Attach**, **ilke türü** filtre kutusuna arama, türü `ReadOnlyAccess`seçin **ReadOnlyAccess**, ardından **sonraki: Gözden geçirme**.  
    ![Salt okunur erişim](./media/connect-aws-account/readonlyaccess.png)
6. İnceleme sayfasında, seçimlerinizi doğru ve yazın emin olun. bir **rol adı**. Örneğin, *Azure-maliyet-anahtara Yönetim*. Girin bir **rol açıklaması**. Örneğin, _Azure maliyeti yönetimi için rol ataması_, ardından **rol oluşturma**.
7. İçinde **rolleri** listesinde, oluşturduğunuz rolüne tıklayın ve kopyalama **rol daha** Özet sayfasında değeri. Azure maliyeti Yönetimi'nde yapılandırmanızı kaydederken daha sonra rol daha (Amazon kaynak adı) değeri kullanın.  
    ![Rolü daha](./media/connect-aws-account/role-arn.png)

### <a name="configure-aws-iam-role-access-in-cost-management"></a>AWS IAM rol erişim maliyet yönetimini yapılandırma

1. Azure portalından Cloudyn portalını açın veya gitmek https://azure.cloudyn.com/ ve oturum açın.
2. Dişlisine simgesine tıklayın ve ardından **bulut hesapları**.
3. Hesapları Yönetimi'nde seçin **AWS hesapları** sekmesini ve sonra **yeni Ekle +**.
4. İçinde **hesap adı**, hesap için bir ad yazın.
5. Yanına **erişim türü**seçin **IAM rol**.
6. İçinde **rol daha** alan, daha önce kopyaladığınız değeri yapıştırın ve ardından **kaydetmek**.  
    ![AWS hesap kutusunu Ekle](./media/connect-aws-account/add-aws-account-box.png)


AWS hesabınız hesaplar listesinde görüntülenir. **Sahibinin kimliği** listelenen rol daha değerle eşleşir. **Hesap durumu** Yönetimi maliyeti AWS hesabınız erişebilmesini gösteren yeşil onay işareti simgesi olmalıdır. Ayrıntılı AWS faturalama etkinleştirinceye kadar birleştirme durumunuzu olarak görünür **tek başına**.

![AWS hesap durumu](./media/connect-aws-account/aws-account-status01.png)

Maliyet yönetimi, veri toplama ve raporlar doldurma başlatır. Ardından, [ayrıntılı AWS faturalama etkinleştirmek](#enable-detailed-aws-billing).


## <a name="aws-user-based-access"></a>AWS kullanıcı tabanlı erişim

Aşağıdaki bölümlerde, maliyet yönetim erişim sağlamak için salt okunur bir kullanıcı oluşturmada size yol.

### <a name="add-aws-read-only-user-based-access"></a>AWS salt okunur kullanıcı tabanlı erişim Ekle

1. AWS konsoluna oturum açın https://console.aws.amazon.com/iam/home seçip **kullanıcılar**.
2. Tıklatın **kullanıcı ekleme**.
3. İçinde **kullanıcı adı** alan, bir kullanıcı adı yazın.
4. İçin **erişim türüne**seçin **programlı erişim** tıklatıp **sonraki: izinleri**.  
    ![Kullanıcı Ekle](./media/connect-aws-account/add-user01.png)
5. İzinler için seçin **mevcut ilkeleri doğrudan ekleme**.
6. Altında **izinleri ilkeleri Attach**, **ilke türü** filtre kutusuna arama, türü `ReadOnlyAccess`seçin **ReadOnlyAccess**ve ardından **sonraki : Gözden geçirme**.  
    ![Kullanıcı izinlerini ayarlama](./media/connect-aws-account/set-permission-for-user.png)
7. İnceleme sayfasında, seçimlerinizi doğru tıklatın emin olun ve **kullanıcı oluşturma**.
8. Tamamlandı sayfasında erişim anahtarı kimliği ve parolası erişim anahtarınızı gösterilir. Maliyet Yönetimi'nde kaydı yapılandırmak için bu bilgileri kullanın.
9. Tıklatın **.csv karşıdan** ve credentials.csv dosyayı güvenli bir konuma kaydedin.  
    ![Kimlik bilgilerini indirin.](./media/connect-aws-account/download-csv.png)

### <a name="configure-aws-iam-user-based-access-in-cost-management"></a>AWS IAM kullanıcı tabanlı erişim maliyet yönetimini yapılandırma

1. Azure portalından Cloudyn portalını açın veya gitmek https://azure.cloudyn.com/ ve oturum açın.
2. Dişlisine simgesine tıklayın ve ardından **bulut hesapları**.
3. Hesapları Yönetimi'nde seçin **AWS hesapları** sekmesini ve sonra **yeni Ekle +**.
4. İçin **hesap adı**, bir hesap adı yazın.
5. Yanına **erişim türü**seçin **IAM kullanıcı**.
6. İçinde **erişim tuşu**, yapıştırma **erişim anahtar kimliği** credentials.csv dosyasından değeri.
7. İçinde **gizli anahtar**, yapıştırma **gizli erişim anahtarı** değeri credentials.csv dosyasından ve ardından **kaydetmek**.  

AWS hesabınız hesaplar listesinde görüntülenir. **Hesap durumu** yeşil onay işareti simgesi olmalıdır.

Maliyet yönetimi, veri toplama ve raporlar doldurma başlatır. Ardından, [ayrıntılı AWS faturalama etkinleştirmek](#enable-detailed-aws-billing).

## <a name="enable-detailed-aws-billing"></a>Ayrıntılı AWS faturalama etkinleştir

AWS rol daha almak için aşağıdaki adımları kullanın. Rolü daha fatura kova Okuma izinleri vermek için kullanın.

1. Oturum açtığınızda AWS konsolda https://console.aws.amazon.com seçip **Hizmetleri**.
2. Hizmet arama kutusuna yazın *IAM*ve bu seçeneği belirleyin.
3. Seçin **rolleri** sol taraftaki menüden.
4. Roller listesinde Cloudyn erişim için oluşturduğunuz rolü seçin.
5. Rol Özeti sayfasında kopyalamak için tıklayın **rol daha**. Rolü daha sonraki adımlara elinizin altında tutun.

### <a name="create-an-s3-bucket"></a>S3 demetini oluşturma

Ayrıntılı fatura bilgilerini depolamak için bir S3 demetini oluşturun.

1. Oturum açtığınızda AWS konsolda https://console.aws.amazon.com seçip **Hizmetleri**.
2. Hizmet arama kutusuna yazın *S3*seçip **S3**.
3. Amazon S3 sayfasında, tıklatın **oluşturma demet**.
4. Create demet Sihirbazı'nda bir demet adı ve bölge seçin ve ardından **sonraki**.  
    ![Demet oluştur](./media/connect-aws-account/create-bucket.png)
5. Üzerinde **özelliklerini ayarlama** sayfasında, varsayılan değerleri koruyun ve ardından **sonraki**.
6. İnceleme sayfasında, tıklatın **oluşturma demet**. Demet listesi görüntülenir.
7. Oluşturduğunuz demet tıklayın ve **izinleri** sekmesini ve ardından **demet İlkesi**. Demet İlkesi Düzenleyicisi'ni açar.
8. Aşağıdaki JSON örneği kopyalayın ve demet İlkesi Düzenleyicisi'nde yapıştırın.
  - Değiştir `<BillingBucketName>` S3 demetini adı.
  - Değiştir `<ReadOnlyUserOrRole>` rolü ya da daha önce kopyaladığınız bir kullanıcı daha ile.

  ```
  {
    "Version": "2012-10-17",
    "Id": "Policy1426774604000",
    "Statement": [
        {
            "Sid": "Stmt1426774604000",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::386209384616:root"
            },
            "Action": [
                "s3:GetBucketAcl",
                "s3:GetBucketPolicy"
            ],
            "Resource": "arn:aws:s3:::<BillingBucketName>"
        },
        {
            "Sid": "Stmt1426774604001",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::386209384616:root"
            },
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::<BillingBucketName>/*"
        },
        {
            "Sid": "Stmt1426774604002",
            "Effect": "Allow",
            "Principal": {
                "AWS": "<ReadOnlyUserOrRole>"
            },
            "Action": [
                "s3:List*",
                "s3:Get*"
            ],
            "Resource": "arn:aws:s3:::<BillingBucketName>/*"
        }
    ]
  }
  ```

9. **Kaydet**’e tıklayın.  
    ![Demet İlkesi Düzenleyicisi](./media/connect-aws-account/bucket-policy-editor.png)


### <a name="enable-aws-billing-reports"></a>Raporları faturalama AWS etkinleştir

Oluşturma ve S3 demetini yapılandırdıktan sonra gidin [faturalama Tercihler](https://console.aws.amazon.com/billing/home?#/preference) AWS konsolunda.

1. Tercihler sayfasında seçin **faturalama raporları alma**.
2. Altında **faturalama raporları alma**, oluşturduğunuz demet adını girin ve ardından **doğrula**.  
3. Tüm dört rapor ayrıntı düzeyi seçenekleri ve ardından seçin **Tercihleri kaydetmek**.  
    ![Raporlar etkinleştir](./media/connect-aws-account/enable-reports.png)

Maliyet yönetimi, S3 demetini ayrıntılı fatura bilgilerini alır ve ayrıntılı faturalama etkinleştirildikten sonra raporları doldurur. Ayrıntılı faturalama verisi Cloudyn konsolunda görünene kadar en fazla 24 saat sürebilir. Ayrıntılı fatura veriler kullanılabilir olduğunda, hesap birleştirme durumunuzu olarak görünür **birleştirilmiş**. Hesap durumu görünür olarak **tamamlandı**.

![Birleştirilmiş durum hesabı](./media/connect-aws-account/consolidated-status.png)

Bazı en iyi duruma getirme raporlar verilerinin yeterli veri örnek boyutu için doğru önerileri almak için birkaç gün gerektirebilir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure maliyeti yönetimi hakkında daha fazla bilgi edinmek için devam [gözden kullanım ve maliyetleri](tutorial-review-usage.md) maliyet Yönetimi Öğreticisi.
