---
title: Depolama hesaplarını Azure maliyeti yönetimi için yapılandırma | Microsoft Docs
description: Bu makalede, Azure maliyeti yönetimi için nasıl Azure depolama hesapları ve AWS depolama demet yapılandırma açıklanmaktadır.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 06/07/2018
ms.topic: conceptual
ms.service: cost-management
manager: carmonm
ms.custom: ''
ms.openlocfilehash: e37604e5cd36cfed016ef596060459011ec32d35
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35297844"
---
# <a name="configure-storage-accounts-for-cost-management"></a>Depolama hesapları için maliyet yönetimini yapılandırma

<!--- intent: As a Cost Management user, I want to configure Cost Management to use my cloud service provider storage account to store my reports. -->

Maliyet yönetim raporları Cloudyn portal, Azure depolama veya AWS depolama demet kaydedebilirsiniz. Raporlarınızı Cloudyn Portalı'na kaydetme ücretsizdir. Ancak, bulut hizmeti sağlayıcısının depolama birimine raporlarınızı kaydetme isteğe bağlıdır ve ek maliyet oluşturur. Bu makalede Azure depolama hesapları ve Amazon Web Hizmetleri (AWS) depolama demet raporlarınızı depolamak için yapılandırmanıza yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar

Bir Azure depolama hesabı ya da bir Amazon depolama aralığı olması gerekir.

Bir Azure depolama hesabınız yoksa, bir oluşturmanız gerekir. Azure depolama hesabı oluşturma hakkında daha fazla bilgi için bkz: [depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md#create-a-storage-account).

Bir AWS yoksa Basit Depolama Birimi Hizmeti (S3) demet bir oluşturmanız gerekir. S3 demetini oluşturma hakkında daha fazla bilgi için bkz: [kova oluşturma](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html).

## <a name="configure-your-azure-storage-account"></a>Azure depolama hesabınızın yapılandırın

Yapılandırma yönetimi maliyeti tarafından kullanılmak üzere Azure depolama basittir. Depolama hesabı hakkında ayrıntılı bilgi toplamak ve bunları Cloudyn Portalı'nda kopyalayın.

1. http://portal.azure.com adresinden Azure portalında oturum açın.
2. Tıklatın **tüm hizmetleri**seçin **depolama hesapları**, kullanın ve ardından hesap seçmek için istediğiniz depolama hesabına gidin.
3. Depolama hesap sayfanıza altında **ayarları**, tıklatın **erişim tuşları**.
4. Kopyalama, **depolama hesabı adı** ve **bağlantı dizesi** key1 altında.  
![Azure depolama erişim tuşları](./media/storage-accounts/azure-storage-access-keys.png)  
5. Azure portalından Cloudyn portalını açın veya gitmek https://azure.cloudyn.com ve oturum açın.
6. Dişlisine simgesine tıklayın ve ardından **raporları Depolama Yönetimi**.
7. Tıklatın **yeni Ekle +** ve Microsoft Azure'nın seçili olduğundan emin olun. Azure depolama hesap adınızı Yapıştır **adı** alanı. Yapıştırma, **bağlantı dizesi** karşılık gelen alanında. Bir kapsayıcı adı girin ve ardından **kaydetmek**.  
![Azure için yapılandırılmış Cloudyn depolama](./media/storage-accounts/azure-cloudyn-storage.png)

  Yeni Azure rapor depolama girişinizin depolama hesabı listesinde görüntülenir.  
    ![Listede yeni Azure rapor depolama](./media/storage-accounts/azure-storage-entry.png)


Bu gibi durumlarda, raporlar artık Azure depolama alanına kaydedebilirsiniz. Herhangi bir rapora tıklayın **Eylemler** ve ardından **zamanlama rapor**. Rapor adı ve ardından kendi URL'sini ekleyin ya da otomatik olarak oluşturulan URL'yi kullanın. Seçin **depolamasına kaydetmek** ve depolama hesabı seçin. Raporu dosya adına eklenmiş bir önek girin. CSV veya JSON dosya biçimi seçin ve ardından raporu kaydedin.

## <a name="configure-an-aws-storage-bucket"></a>Bir AWS depolama aralığı yapılandırın

Cloudyn varolan AWS kimlik bilgilerini kullanır: kullanıcı veya rol, raporları, sepet kaydetmek için. Erişim sınamak için küçük bir metin dosyası demet dosya adı ile kaydetmek Cloudyn çalışır _onay demet permission.txt_.

Cloudyn rolü ya da kullanıcı PutObject izniyle, sepet sağlar. Ardından, var olan bir demet kullanın veya raporları kaydetmek için yeni bir tane oluşturun. Son olarak, depolama sınıfı yönetmek, yaşam döngüsü kuralları ayarlamak veya gereksiz dosyaları kaldırmak nasıl karar verin.

###  <a name="assign-permissions-to-your-aws-user-or-role"></a>AWS kullanıcı veya rol için izinleri atayın

Yeni bir ilke oluşturduğunuzda, bir S3 demetini bir raporu kaydetmek için gereken tam izinleri sağlar.

1. AWS konsoluna oturum açın ve seçin **Hizmetleri**.
2. Seçin **IAM** Hizmetleri listesinden.
3. Seçin **ilkeleri** konsol ve ardından sol tarafındaki **ilke Oluştur**.
4. Tıklatın **JSON** sekmesi.
5. Aşağıdaki ilke S3 demetini için bir rapor kaydetmenizi sağlar. Aşağıdaki ilke örneğe kopyalayıp **JSON** sekmesi. Değiştir &lt;bucketname&gt; demet ada sahip.

  ```
{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Sid":  "CloudynSaveReport2S3",
        "Effect":      "Allow",
        "Action": [
          "s3:PutObject"
        ],
        "Resource": [
          "arn:aws:s3:::<bucketname>/*"
        ]
      }
    ]
}
```

6. Tıklatın **gözden İlkesi**.  
    ![İlke gözden geçirin](./media/storage-accounts/aws-policy.png)  
7. Gözden geçirme İlkesi sayfasında ilkeniz için bir ad yazın. Örneğin, _CloudynSaveReport2S3_.
8. Tıklatın **ilkesi oluşturma**.

### <a name="attach-the-policy-to-a-cloudyn-role-or-user-in-your-account"></a>Cloudyn rolü ya da kullanıcı hesabınızda ilke ekleme

Yeni ilke eklemek için AWS konsolu açın ve Cloudyn rolü ya da kullanıcı düzenleyin.

1. AWS konsoluna oturum açın ve seçin **Hizmetleri**seçeneğini belirleyip **IAM** Hizmetleri listesinden.
2. Şunlardan birini seçin **rolleri** veya **kullanıcılar** konsolun sol taraftaki.

**Roller için:**

  1. Cloudyn rol adına tıklayın.
  2. Üzerinde **izinleri** sekmesini tıklatın, **ekleme İlkesi**.
  3. Aramak için oluşturduğunuz ilke ve onu seçin ve ardından **ekleme İlkesi**.
    ![AWS - bir rol için ilke ekleme](./media/storage-accounts/aws-attach-policy-role.png)

**Kullanıcılar için:**

1. Cloudyn kullanıcıyı seçin.
2. Üzerinde **izinleri** sekmesini tıklatın, **izinleri eklemek**.
3. İçinde **izni** bölümünde, select **mevcut ilkeleri doğrudan ekleme**.
4. Aramak için oluşturduğunuz ilke ve onu seçin ve ardından **sonraki: gözden geçirme**.
5. Rol adı sayfasına Ekle izinlerini tıklatın **izinleri eklemek**.  
    ![AWS - bir kullanıcı için ilke ekleme](./media/storage-accounts/aws-attach-policy-user.png)


### <a name="optional-set-permission-with-bucket-policy"></a>İsteğe bağlı: demet ilkesiyle izni Ayarla

Ayrıca bir demet İlkesi'ni kullanarak, S3 demetini üzerinde raporları oluşturma izni ayarlayabilirsiniz. Klasik S3 görünümünde:

1. Oluşturun veya varolan bir aralığı seçin.
2. Seçin **izinleri** sekmesini ve sonra **aralığı İlkesi**.
3. Aşağıdaki ilke örnek kopyalayıp yeniden açın. Değiştir &lt;demet\_adı&gt; ve &lt;Cloudyn\_İlkesi&gt; , sepet daha ile. Rol veya Cloudyn tarafından kullanılan kullanıcı daha değiştirin.

  ```
{
  "Id": "Policy1485775646248",
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "SaveReport2S3",
      "Action": [
        "s3:PutObject"
      ],
      "Effect": "Allow",
      "Resource": "<bucket_name>/*",
      "Principal": {
        "AWS": [
          "<Cloudyn_principle>"
        ]
      }
    }
  ]
}
```

4. Demet İlkesi Düzenleyicisi'ni **kaydetmek**.

### <a name="add-aws-report-storage-to-cloudyn"></a>AWS rapor depolama için Cloudyn ekleme

1. Azure portalından Cloudyn portalını açın veya gitmek https://azure.cloudyn.com ve oturum açın.
2. Dişlisine simgesine tıklayın ve ardından **raporları Depolama Yönetimi**.
3. Tıklatın **yeni Ekle +** ve AWS seçili olduğundan emin olun.
4. Bir hesap ve depolama aralığı seçin. AWS depolama demet otomatik olarak doldurulmuş içinde adıdır.  
    ![Rapor depolama AWS sepet için ekleme](./media/storage-accounts/aws-cloudyn-storage.png)  
5. Tıklatın **kaydetmek** ve ardından **Tamam**.

    Yeni AWS rapor depolama girdisi depolama hesabı listesinde görüntülenir.  
    ![Listede yeni AWS rapor depolama](./media/storage-accounts/aws-storage-entry.png)


Bu gibi durumlarda, raporlar artık Azure depolama alanına kaydedebilirsiniz. Herhangi bir rapora tıklayın **Eylemler** ve ardından **zamanlama rapor**. Rapor adı ve ardından kendi URL'sini ekleyin ya da otomatik olarak oluşturulan URL'yi kullanın. Seçin **depolamasına kaydetmek** ve depolama hesabı seçin. Raporu dosya adına eklenmiş bir önek girin. CSV veya JSON dosya biçimi seçin ve ardından raporu kaydedin.

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme [anlama maliyet yönetim raporları](understanding-cost-reports.md) temel yapısını ve maliyet yönetim raporları işlevleri hakkında bilgi edinmek için.
