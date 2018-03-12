---
title: "Azure maliyeti Management'a Amazon Web Services hesabınız bağlanma | Microsoft Docs"
description: "Maliyet Yönetimi raporları maliyet ve kullanım verileri görüntülemek için bir Amazon Web Hizmetleri hesap bağlayın."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 02/08/2018
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: 
ms.openlocfilehash: 4a0280420132aad9f1e0b17d5998ec225bb0eaa1
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="connect-an-amazon-web-services-account"></a>Amazon Web Services hesabınız Bağlan

Azure maliyeti yönetim Amazon Web Hizmetleri (AWS) hesabınıza bağlamak için iki seçeneğiniz vardır. Salt okunur bir IAM kullanıcı hesabını veya bir IAM rolü ile bağlanabilir. IAM rol temsilci erişimi güvenilir varlıklar için tanımlanan izinlerle izin verdiği için önerilir. IAM rol uzun süreli erişim anahtarları paylaşımı gerektirmez. AWS hesabınız için maliyet yönetim bağlandıktan sonra maliyet ve kullanım verileri Yönetimi maliyeti raporlarında kullanılabilir. Bu belge, her iki seçenek size yol gösterir.

AWS IAM kimlikler hakkında daha fazla bilgi için bkz: [kimlikleri (kullanıcıları, grupları ve rolleri)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html).

## <a name="aws-role-based-access"></a>AWS rol tabanlı erişim

Aşağıdaki bölümlerde, maliyet yönetim erişim sağlamak için bir salt okunur IAM rolü oluşturmada size yol.

### <a name="get-your-cost-management-account-external-id"></a>Maliyet yönetim hesabı dış Kimliğinizi alın

İlk adım, Azure maliyeti Yönetim Portalı'ndan benzersiz bağlantı parola almaktır. AWS kullanılan **Dış kimlik**.

1. Azure portalından Cloudyn portalını açın veya gitmek [https://azure.cloudyn.com](https://azure.cloudyn.com) ve oturum açın.
2. Tıklatın **ayarları** (dişli simgesi) ve ardından **bulut hesapları**.
3. Hesapları Yönetimi'nde seçin **AWS hesapları** sekmesini ve sonra **yeni Ekle +**.
4. İçinde **AWS hesabı Ekle** iletişim, kopya **Dış kimlik** ve AWS oluşturma adımları sonraki bölümde rol için bu değeri kaydedin. Aşağıdaki görüntüde, örnek kimliği: _Cloudyn_. Kimliğinizi farklıdır.  
    ![Dış kimliği](./media/connect-aws-account/external-id.png)

### <a name="add-aws-read-only-role-based-access"></a>AWS salt okunur rol tabanlı erişim Ekle

1. Oturum https://console.aws.amazon.com/iam/home ve select AWS konsoluna **rolleri**.
2. Tıklatın **Rol Oluştur** ve ardından **başka bir AWS hesap**.
3. Kimlik Yapıştır `432263259397` içinde **hesap kimliği** alan. Bu hesap kimliği kullanmanız Yönetimi maliyeti veri toplayıcı hesabıdır.
4. Yanına **seçenekleri**seçin **dış kimliği gerektiren** içinde daha önce kopyaladığınız değeri yapıştırın **dış kimliği** alan ve ardından **sonraki: İzinleri**.  
    ![Rolü oluşturma](./media/connect-aws-account/create-role01.png)
5. Altında **izinleri ilkeleri Attach**, **ilke türü** filtre kutusuna arama, türü `ReadOnlyAccess`seçin **ReadOnlyAccess**, ardından **sonraki: Gözden geçirme**.  
    ![Salt okunur erişim](./media/connect-aws-account/readonlyaccess.png)
6. İnceleme sayfasında, seçimlerinizi doğru ve yazın emin olun. bir **rol adı**. Örneğin, *Azure-maliyet-anahtara Yönetim*. Girin bir **rol açıklaması**. Örneğin, _Azure maliyeti yönetimi için rol ataması_, ardından **rol oluşturma**.
7. İçinde **rolleri** listesinde, oluşturduğunuz rolüne tıklayın ve kopyalama **rol daha** Özet sayfasında değeri. Azure maliyeti Yönetimi'nde yapılandırmanızı kaydederken daha sonra rol daha değeri kullanın.  
    ![Rolü daha](./media/connect-aws-account/role-arn.png)

### <a name="configure-aws-iam-role-access-in-cost-management"></a>AWS IAM rol erişim maliyet yönetimini yapılandırma

1. Azure portalından Cloudyn portalını açın veya https://azure.cloudyn.com/ için gidin ve oturum açın.
2. Tıklatın **ayarları** (dişli simgesi) ve ardından **bulut hesapları**.
3. Hesapları Yönetimi'nde seçin **AWS hesapları** sekmesini ve sonra **yeni Ekle +**.
4. İçinde **hesap adı**, hesap için bir ad yazın.
5. Yanına **erişim türü**seçin **IAM rol**.
6. İçinde **rol daha** alan, daha önce kopyaladığınız değeri yapıştırın ve ardından **kaydetmek**.  
    ![AWS hesap durumu](./media/connect-aws-account/aws-account-status01.png)

AWS hesabınız hesaplar listesinde görüntülenir. **Sahibinin kimliği** listelenen rol daha değerle eşleşir. **Hesap durumu** yeşil onay işareti simgesi olmalıdır.

Maliyet yönetimi, veri toplama ve raporlar doldurma başlatır. Ancak, bazı iyileştirme raporları doğru önerileri oluşturulmadan önce birkaç gün verilerden gerektirebilir.

## <a name="aws-user-based-access"></a>AWS kullanıcı tabanlı erişim

Aşağıdaki bölümlerde, maliyet yönetim erişim sağlamak için salt okunur bir kullanıcı oluşturmada size yol.

### <a name="add-aws-read-only-user-based-access"></a>AWS salt okunur kullanıcı tabanlı erişim Ekle

1. Oturum https://console.aws.amazon.com/iam/home ve select AWS konsoluna **kullanıcılar**.
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

1. Azure portalından Cloudyn portalını açın veya https://azure.cloudyn.com/ için gidin ve oturum açın.
2. Tıklatın **ayarları** (dişli simgesi) ve ardından **bulut hesapları**.
3. Hesapları Yönetimi'nde seçin **AWS hesapları** sekmesini ve sonra **yeni Ekle +**.
4. İçin **hesap adı**, bir hesap adı yazın.
5. Yanına **erişim türü**seçin **IAM kullanıcı**.
6. İçinde **erişim tuşu**, yapıştırma **erişim anahtar kimliği** credentials.csv dosyasından değeri.
7. İçinde **gizli anahtar**, yapıştırma **gizli erişim anahtarı** değeri credentials.csv dosyasından ve ardından **kaydetmek**.  
    ![AWS kullanıcı hesabı durumu](./media/connect-aws-account/aws-user-account-status.png)

AWS hesabınız hesaplar listesinde görüntülenir. **Hesap durumu** yeşil onay işareti simgesi olmalıdır.

Maliyet yönetimi, veri toplama ve raporlar doldurma başlatır. Ancak, bazı iyileştirme raporları doğru önerileri oluşturulmadan önce birkaç gün verilerden gerektirebilir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure maliyeti yönetimi hakkında daha fazla bilgi edinmek için devam [gözden kullanım ve maliyetleri](tutorial-review-usage.md) maliyet Yönetimi Öğreticisi.
