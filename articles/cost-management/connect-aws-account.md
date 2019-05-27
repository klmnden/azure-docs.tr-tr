---
title: Amazon Web Services hesabınız Azure Cloudyn bağlanın | Microsoft Docs
description: Cloudyn raporlarında maliyet ve kullanım verilerini görüntülemek için bir Amazon Web Services hesabına bağlanın.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/21/2019
ms.topic: conceptual
ms.service: cost-management
manager: benshy
ms.custom: seodec18
ms.openlocfilehash: b39296e18b38180e1081866d6e8197973dc782b1
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66002155"
---
# <a name="connect-an-amazon-web-services-account"></a>Amazon Web Services hesabına bağlanma

Amazon Web Services (AWS) hesabınızı Cloudyn'e bağlamak için iki seçeneğiniz vardır. IAM rolü veya bir salt okunur IAM kullanıcı hesabı ile bağlanabilirsiniz. IAM rol temsilci erişimi güvenilir varlıklar için tanımlanmış izinlerle izin verdiği için önerilir. IAM rolünü uzun süreli erişim anahtarlarınızı paylaşmak gerektirmez. AWS hesabı Cloudyn'e bağlandıktan sonra maliyet ve kullanım verileri, Cloudyn raporlarında kullanılabilir. Bu belge, her iki çalışma seçeneklerde size yol gösterir.

AWS IAM kimlikler hakkında daha fazla bilgi için bkz. [kimlikleri (kullanıcılar, gruplar ve roller)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html).

Ayrıca, AWS ayrıntılı sağlayan raporlar faturalama ve bir AWS basit depolama hizmeti (S3 için) demetine bilgileri depolar. Ayrıntılı bir faturalandırma raporlarını saatlik olarak herhangi bir etiket ve kaynak bilgileriyle faturalandırma ücretleri dahildir. Depolama raporları, bunları, alanı almak ve onun raporlarında bilgileri görüntülemek Cloudyn sağlar.


## <a name="aws-role-based-access"></a>AWS rol tabanlı erişim

Aşağıdaki bölümlerde, Cloudyn'e erişim sağlamak için bir salt okunur IAM rol oluşturmak adım adım açıklanmaktadır.

### <a name="get-your-cloudyn-account-external-id"></a>Cloudyn hesap dış kimliği alma

İlk adım, Cloudyn portaldan benzersiz bağlantı parolası almaktır. AWS kullanılan **dış kimliği**.

1. Cloudyn portalını Azure portalından açın veya gidin [ https://azure.cloudyn.com ](https://azure.cloudyn.com) ve oturum açın.
2. Dişli simgesine tıklayın ve ardından **bulut hesapları**.
3. Hesapları Yönetimi'nde seçin **AWS hesapları** sekmesine ve ardından **yeni Ekle +**.
4. İçinde **AWS hesabı Ekle** iletişim kutusunda, kopyalama **Dış kimlik** ve AWS oluşturma adımları sonraki bölümde rol için değer kaydedin. Dış hesabınıza benzersiz kimliğidir. Aşağıdaki görüntüde, örneğin dış kimliğidir _Contoso_ bir sayı takip eder. Kimliğinizi farklıdır.  
    ![AWS hesabı Ekle iletişim kutusunda gösterilen dış kimliği](./media/connect-aws-account/external-id.png)

### <a name="add-aws-read-only-role-based-access"></a>AWS salt okunur rol tabanlı erişim Ekle

1. Oturum açmak için AWS konsolunda https://console.aws.amazon.com/iam/home seçip **rolleri**.
2. Tıklayın **Rol Oluştur** seçip **başka bir AWS hesabı**.
3. İçinde **hesap kimliği** kutusu, yapıştırma `432263259397`. Bu hesap kimliği, Cloudyn hizmete AWS tarafından atanan Cloudyn veri toplayıcı hesabıdır. Tam hesap kimliği kullanın.
4. Yanındaki **seçenekleri**seçin **dış ID gerektir**. Öğesinden daha önce kopyaladığınız benzersiz değerinizi yapıştırın **Dış kimlik** Cloudyn alanındaki. Ardından **sonraki: İzinleri**.  
    ![Cloudyn dış kimliği Oluştur rol sayfasında yapıştırın](./media/connect-aws-account/create-role01.png)
5. Altında **izin ilkeleriyle ekleme**, **ilke türü** filtre kutusuna arama, türü `ReadOnlyAccess`seçin **ReadOnlyAccess**, ardından **sonraki: Gözden geçirme**.  
    ![salt okunur erişim ilke adları listesinden seçin](./media/connect-aws-account/readonlyaccess.png)
6. İnceleme sayfasında, seçimlerinizi doğru olduğundan ve tür olun bir **rol adı**. Örneğin, *maliyet-Azure-anahtara Yönetim*. Girin bir **rol açıklamasını**. Örneğin, _Cloudyn için rol ataması_, ardından **rol oluşturma**.
7. İçinde **rolleri** listesinde, oluşturduğunuz role tıklayın ve kopyalama **rol ARN** Özet sayfasından değeri. Rol ARN (Amazon kaynak adı) değeri, daha sonra yapılandırmanızı Cloudyn'de kaydettiğinizde kullanın.  
    ![Rol ARN Özet sayfasından kopyalayın](./media/connect-aws-account/role-arn.png)

### <a name="configure-aws-iam-role-access-in-cloudyn"></a>Cloudyn'de AWS IAM rol erişimini yapılandırma

1. Cloudyn portalını Azure portalından açın veya gidin https://azure.cloudyn.com/ ve oturum açın.
2. Dişli simgesine tıklayın ve ardından **bulut hesapları**.
3. Hesapları Yönetimi'nde seçin **AWS hesapları** sekmesine ve ardından **yeni Ekle +**.
4. İçinde **hesap adı**, hesap için bir ad yazın.
5. Yanındaki **erişim türü**seçin **IAM rol**.
6. İçinde **rol ARN** alan, daha önce kopyaladığınız değeri yapıştırın ve ardından **Kaydet**.  
    ![AWS hesabı Ekle kutusunu rol ARN yapıştırın](./media/connect-aws-account/add-aws-account-box.png)


AWS hesabınız hesapları listesinde görünür. **Sahibinin kimliği** listelenen, rol ARN değeri eşler. **Hesap durumu** Cloudyn AWS hesabınız erişebilirsiniz belirten yeşil onay işareti simgesi olmalıdır. Ayrıntılı AWS faturalamayı etkinleştirene kadar birleştirme durumunuzu olarak görünür **tek başına**.

![Hesap Yönetimi sayfasında gösterilen AWS hesabı durumu](./media/connect-aws-account/aws-account-status01.png)

Cloudyn, veri toplama ve raporları doldurma başlatır. Ardından, [ayrıntılı AWS faturalamayı etkinleştirme](#enable-detailed-aws-billing).


## <a name="aws-user-based-access"></a>AWS kullanıcı tabanlı erişim

Aşağıdaki bölümlerde, Cloudyn'e erişim sağlamak için salt okunur bir kullanıcı oluşturmak adım adım açıklanmaktadır.

### <a name="add-aws-read-only-user-based-access"></a>AWS salt okunur kullanıcı tabanlı erişim Ekle

1. Oturum açmak için AWS konsolunda https://console.aws.amazon.com/iam/home seçip **kullanıcılar**.
2. Tıklayın **kullanıcı ekleme**.
3. İçinde **kullanıcı adı** alanında, bir kullanıcı adı yazın.
4. İçin **erişim türü**seçin **programlı erişim** tıklatıp **sonraki: İzinleri**.  
    ![Kullanıcı Ekle sayfasında bir kullanıcı adı girin](./media/connect-aws-account/add-user01.png)
5. İzinler için seçin **mevcut ilkeleri doğrudan Ekle**.
6. Altında **izin ilkeleriyle ekleme**, **ilke türü** filtre kutusuna arama, türü `ReadOnlyAccess`seçin **ReadOnlyAccess**ve ardından **İleri : Gözden geçirme**.  
    ![kullanıcı izinlerini ayarlamak için ReadOnlyAccess seçin](./media/connect-aws-account/set-permission-for-user.png)
7. İnceleme sayfasında, seçimlerinizi doğru tıklayın emin olun ve **kullanıcı oluşturma**.
8. Tam sayfada erişim anahtarı kimliği ve parolası erişim anahtarınızı gösterilir. Cloudyn'de kaydını yapılandırmak için bu bilgileri kullanın.
9. Tıklayın **csv'i indir** credentials.csv dosyayı güvenli bir konuma kaydedin.  
    ![İndirme .csv kimlik bilgilerini kaydetmek için tıklatın](./media/connect-aws-account/download-csv.png)

### <a name="configure-aws-iam-user-based-access-in-cloudyn"></a>AWS IAM kullanıcı tabanlı erişim Cloudyn'de yapılandırın

1. Cloudyn portalını Azure portalından açın veya https://azure.cloudyn.com/ sayfasına gidip oturum açın.
2. Dişli simgesine tıklayın ve ardından **bulut hesapları**.
3. Hesapları Yönetimi'nde seçin **AWS hesapları** sekmesine ve ardından **yeni Ekle +**.
4. İçin **hesap adı**, hesap adını yazın.
5. Yanındaki **erişim türü**seçin **IAM kullanıcı**.
6. İçinde **erişim anahtarı**, Yapıştır **erişim anahtarı kimliği** credentials.csv dosyasından değer.
7. İçinde **gizli anahtar**, Yapıştır **gizli erişim anahtarı** credentials.csv dosyasından değer ve ardından **Kaydet**.  

AWS hesabınız hesapları listesinde görünür. **Hesap durumu** yeşil onay işareti simgesi olmalıdır.

Cloudyn, veri toplama ve raporları doldurma başlatır. Ardından, [ayrıntılı AWS faturalamayı etkinleştirme](#enable-detailed-aws-billing).

## <a name="enable-detailed-aws-billing"></a>Ayrıntılı AWS faturalamayı etkinleştirme

AWS rol ARN almak için aşağıdaki adımları kullanın. Rol ARN fatura bir demet için Okuma izinleri vermek için kullanın.

1. Oturum açmak için AWS konsolunda https://console.aws.amazon.com seçip **Hizmetleri**.
2. Hizmet arama kutusuna *IAM*ve bu seçeneği belirleyin.
3. Seçin **rolleri** sol taraftaki menüden.
4. Rolleri listesinde, oluşturduğunuz Cloudyn erişimi için rolü seçin.
5. Rol Özeti sayfasında kopyalamak için tıklayın **rol ARN**. Rol daha sonraki adımlar için kullanışlı tutun.

### <a name="create-an-s3-bucket"></a>Bir S3 demetini oluşturma

Bir S3 demetini ayrıntılı faturalandırma bilgileri depolamak için oluşturduğunuz.

1. Oturum açmak için AWS konsolunda https://console.aws.amazon.com seçip **Hizmetleri**.
2. Hizmet arama kutusuna *S3*seçip **S3**.
3. Amazon S3 sayfasında tıklayın **Oluştur demet**.
4. Oluşturma demet Sihirbazı'nda bir demet adı ve bölge seçin ve ardından **sonraki**.  
    ![Örnek bilgiler bir oluşturma demetine sayfası](./media/connect-aws-account/create-bucket.png)
5. Üzerinde **özelliklerini ayarlama** sayfasında varsayılan değerleri koruyun ve ardından **sonraki**.
6. İnceleme sayfasında tıklayın **Oluştur demet**. Demet listenizde görüntülenir.
7. Oluşturduğunuz demetine tıklayıp **izinleri** sekmesini seçip **demet ilke**. Demet İlke Düzenleyicisi açılır.
8. Aşağıdaki JSON örneği kopyalayın ve demet İlkesi Düzenleyicisi'nde yapıştırın.
   - Değiştirin `<BillingBucketName>` , S3 demetini adı.
   - Değiştirin `<ReadOnlyUserOrRole>` rolü veya kullanıcı ARN, daha önce kopyaladığınız.

   ```json
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
    ![Demet İlkesi Düzenleyicisi'nde Kaydet'e tıklayın](./media/connect-aws-account/bucket-policy-editor.png)


### <a name="enable-aws-billing-reports"></a>AWS raporları faturalama etkinleştir

Oluşturup S3 demetini yapılandırdıktan sonra gidin [faturalama tercihleri](https://console.aws.amazon.com/billing/home?#/preference) AWS konsolunda.

1. Tercihler sayfasında **faturalama raporları alma**.
2. Altında **faturalama raporları alma**, oluşturduğunuz demet adını girin ve ardından **doğrulama**.  
3. Dört rapor ayrıntı düzeyi seçenekleri ve ardından seçin **Tercihleri Kaydet**.  
    ![raporları etkinleştirmek için ayrıntı düzeyi seçin](./media/connect-aws-account/enable-reports.png)

Cloudyn, S3 demetini ayrıntılı fatura bilgilerini alır ve ayrıntılı bir faturalandırma etkinleştirildikten sonra raporları doldurur. Uygulamanın ayrıntılı faturalama verileri Cloudyn konsolunda görünene kadar en fazla 24 saat sürebilir. Ayrıntılı bir faturalandırma veriler kullanılabilir olduğunda, hesap birleştirme durumunuzu olarak görünür **birleştirilmiş**. Hesap durumu olarak görünür **tamamlandı**.

![AWS hesapları sekmesinde gösterilen birleştirme durumu](./media/connect-aws-account/consolidated-status.png)

Bazı iyileştirme raporlar, birkaç günlük bir yeterli veri örnek boyutu için doğru öneriler almak için veri gerektirebilir.

## <a name="next-steps"></a>Sonraki adımlar

- Cloudyn hakkında daha fazla bilgi için devam [kullanımı ve maliyetleri gözden geçirme](tutorial-review-usage.md) Cloudyn Öğreticisi.
