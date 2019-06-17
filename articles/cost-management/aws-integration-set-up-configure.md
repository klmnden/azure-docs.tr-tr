---
title: Ayarlama ve Azure maliyet yönetimi ile AWS maliyet ve kullanım raporu tümleştirmeyi yapılandırma
description: Bu makalede ayarlama ve Azure maliyet yönetimi ile AWS maliyet ve kullanım raporu tümleştirme yapılandırma gösterilmektedir.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/21/2019
ms.topic: conceptual
ms.service: cost-management
manager: ormaoz
ms.custom: ''
ms.openlocfilehash: 951178a82e0975f5f2af71bd48cf0f931246ae37
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66002119"
---
# <a name="set-up-and-configure-aws-cost-and-usage-report-integration"></a>Ayarlama ve AWS maliyet ve kullanım raporu tümleştirmeyi Yapılandır

Amazon Web Services (AWS) ile rapor (geçerli) tümleştirmesi, maliyet ve kullanımı izleyin ve AWS Azure maliyet Yönetimi'nde harcamalarınızı denetleyin. Tümleştirmesi, tek bir konum burada izleyebilirsiniz Azure portalı ve Azure ve AWS için harcama denetim sağlar. Bu makalede, tümleştirmeyi ayarlamak ve maliyetleri analiz etmek ve bütçe gözden geçirmek için Azure maliyet yönetimi özellikleri kullanmak için yapılandırma açıklanmaktadır.

Yönetim süreçlerini rapor tanımları almak ve rapor GZIP CSV Dosyaları indirmek için AWS erişim kimlik bilgilerinizi kullanarak bir S3 demetini içinde depolanan AWS maliyet ve kullanım raporu maliyet.

## <a name="create-a-cost-and-usage-report-in-aws"></a>AWS'de bir maliyet ve kullanım raporu oluşturma

Maliyet ve kullanım raporu kullanarak toplamak ve AWS maliyetlerinden işlemek için AWS tarafından önerilen yoludur. Daha fazla bilgi için [AWS maliyet ve kullanım raporu](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-reports-costusage.html) belgeleri.

Kullanım **maliyet ve kullanım raporları** faturalandırma ve maliyet Yönetimi aşağıdaki adımlarla bir maliyet ve kullanım raporu oluşturmak için AWS konsolunda sayfasında:

1. AWS yönetim konsoluna oturum açın ve açın [faturalandırma ve maliyet Yönetimi Konsolu](https://console.aws.amazon.com/billing).
2. Gezinti bölmesinde seçin **maliyet ve kullanım raporları**.
3. Seçin **rapor oluşturma**.
4. İçin **rapor adı**, raporunuz için bir ad girin.
5. İçin **ek rapor ayrıntıları** , rapor ve seçin, her kaynak kimliklerini dahil edilecek **içeren kaynak kimlikleri**.
6. İçin **veri yenileme ayarları**, AWS para iadesi, KREDİLERİ, geçerliyse yenilemek için AWS maliyet ve kullanım raporu isteyip istemediğinizi seçin veya faturanızı sonlandırılıyor sonra ücretler, hesabınıza destekler. Yeni bir rapor, bir raporu yenilediğinde, Amazon S3 karşıya yüklendi. Bu ayar bırakmak önerilir.
7. **İleri**’yi seçin.
8. İçin **S3 demetini**, seçin **yapılandırma**.
9. S3 Demetini Yapılandır iletişim kutusunda, aşağıdakilerden birini yapın:
    1. Aşağı açılan listeden mevcut bir aralığı seçip seçin **sonraki**.
    2. Bir demet adı ve yeni bir demet oluşturma ve istediğiniz bölgeyi girin **sonraki**.
10. Bu ilke olduğunu doğruladı seçin, düzeltin ve Kaydet'i seçin.
11. (İsteğe bağlı) Rapor yolu ön eki için raporunuzu adına prepended istediğiniz rapor yol ön eki girin.
Bir önek belirtmezseniz varsayılan ön ek, belirttiğiniz 4. adımda rapor ve tarih aralığını şu biçimde bir rapor adıdır: `/report-name/date-range/`
12. İçin **zaman birimi**, seçin **saatlik**.
13. İçin **rapor sürümü oluşturma**, her raporun rapor önceki sürümünün üzerine yazmak veya önceki sürümlere ek olarak teslim edilecek sürümü isteyip istemediğinizi seçin.
14. İçin **etkinleştirmek için veri tümleştirmesi**, hiçbir seçimi gereklidir.
15. İçin **sıkıştırma**seçin **GZIP**.
16. **İleri**’yi seçin.
17. Raporunuz için ayarları gözden geçirdikten sonra seçin **gözden geçirme ve tam**.

    Rapor adını not edin. Sonraki adımda kullanacaksınız.

Bu raporlar, Amazon S3 demetini için göndermeye başlamak AWS 24 saate kadar sürebilir. Teslim başlatıldıktan sonra AWS AWS maliyet ve kullanım raporu dosyaları günde bir en az bir kez güncelleştirir. Başlamak için teslim beklemeden AWS ortamınızı yapılandırmaya devam edebilirsiniz.

## <a name="create-a-role-and-policy-in-aws"></a>AWS'de bir rol ve ilke oluşturma

Azure maliyet yönetimi, maliyet ve kullanım raporu günde birkaç kez bulunduğu S3 demetini erişir. Hizmet, yeni verileri denetlemek için kimlik bilgilerini erişmesi gerekir. AWS maliyet Yönetimi'nin erişim izin vermek için rol ve ilke oluşturun.

AWS hesabı maliyet Yönetimi'nde rol tabanlı erişim sağlamak için rol AWS konsolunda oluşturulur. İhtiyacınız _rol ARN_ ve _Dış kimlik_ AWS Konsolu. Daha sonra bunları şirket kullandığınız **AWS Bağlayıcısı oluşturma** maliyet Yönetimi'nde sayfa.

Yeni Rol Oluşturma Sihirbazı'nı kullanın:

1. AWS konsolunuza oturum açın ve seçin **Hizmetleri**.
2. Hizmetler listesinde seçin **IAM**.
3. Seçin **rolleri** seçip **Rol Oluştur**.
4. Sonraki sayfada seçin **başka bir AWS hesabı**.
5. İçinde **hesap kimliği**, girin **432263259397**.
6. İçinde **seçenekleri**seçin **dış kimliği (üçüncü taraf bu rolünü varsaymadan olduğunda en iyi uygulama) gerektiren**.
7. İçinde **Dış kimlik**, dış kimliği girin. Dış kimlik, AWS rolü ve Azure maliyet Yönetimi arasında paylaşılan bir paroladır. Aynı dış kimliği de kullanılan **yeni bağlayıcı** maliyet Yönetimi'nde sayfa. Örneğin, bir dış kimlik benzer _Companyname1234567890123_.

    > [!NOTE]
    > Seçimini değiştirmeyin **MFA gerektiren**. Temizlenmiş kalmalıdır.
8. Seçin **sonraki: İzinleri**.
9. Seçin **ilkesi oluşturma**. Yeni bir tarayıcı sekmesi açılır. Bir ilke oluşturacağınız olmasıdır.
10. Seçin **bir hizmet seçin**.

Maliyet ve kullanım raporu izinlerini yapılandırın:

1. Girin **maliyet ve kullanım raporu**.
2. Seçin **erişim düzeyi** > **okuma** > **DescribeReportDefinitions**. Bu adım, hangi bu ven raporları tanımlanan okuyun ve rapor tanımı önkoşul eşleşip eşleşmediğini belirlemek maliyet yönetimi sağlar.
3. Seçin **ek izinler ayarlamanız**.

S3 demetini ve nesneleri izinlerini yapılandırın:

1. Seçin **bir hizmet seçin**.
2. Girin **S3**.
3. Seçin **erişim düzeyi** > **listesi** > **ListBucket**. Bu eylem, içinde S3 Demetini nesneleri listesini alır.
4. Seçin **erişim düzeyi** > **okuma** > **GetObject**. Bu eylem, fatura dosyalarını indirilmesini sağlar.
5. Seçin **kaynakları**.
6. Seçin **demet – ekleme ARN**.
7. İçinde **demet adı**, bu ven dosyaları depolamak için kullanılan demet girin.
8. Seçin **nesnesi – ekleme ARN**.
9. İçinde **demet adı**, bu ven dosyaları depolamak için kullanılan demet girin.
10. İçinde **nesne adı**seçin **herhangi**.
11. Seçin **ek izinler ayarlamanız**.

Maliyet Gezgini izinlerini yapılandırın:

1. Seçin **bir hizmet seçin**.
2. Girin **Explorer hizmeti maliyet**.
3. Seçin **tüm maliyet Gezgini hizmet eylemleri (ce:\*)** . Bu eylem, koleksiyon doğru olduğunu doğrular.
4. Seçin **ek izinler ayarlamanız**.

AWS kuruluşlar için izin ekleyin:

1. Girin **kuruluşlar**.
2. Seçin **erişim düzeyi** > **listesi** > **ListAccounts**. Bu eylem hesapların adlarını alır.
3. İçinde **İnceleme İlkesi**, yeni ilke için bir ad girin. Doğru bilgileri girdiğinizden emin olun ve ardından **ilke Oluştur**.
4. Önceki sekmeye dönün ve tarayıcınızın Web sayfasını yenileyin. Arama çubuğunda, yeni ilkeniz için arama yapın.
5. Seçin **sonraki: inceleme**.
6. Yeni rol için bir ad girin. Doğru bilgileri girdiğinizden emin olun ve ardından **Rol Oluştur**.

    Not rol ARN ve Dış kimlik rolü oluşturduğunuz zaman önceki adımlarda kullanılır. Azure maliyet Yönetimi Bağlayıcısı'nı ayarlarken daha sonra kullanacaksınız.

JSON İlkesi, aşağıdaki örneğe benzemelidir. Değiştirin _bucketname_ , S3 demetini adı.

```JSON
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
"organizations:ListAccounts",
            "ce:*",
            "cur:DescribeReportDefinitions"
            ],
            "Resource": "*"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::bucketname",
                "arn:aws:s3:::bucketname/*"
            ]
        }
    ]
}
```

## <a name="set-up-a-new-aws-connector-in-azure"></a>Azure'da yeni bir AWS bağlayıcısını ayarlayın

Bir AWS bağlayıcısı oluşturmak ve AWS maliyetlerinizi izlemeye başlamak için aşağıdaki bilgileri kullanın:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Git **maliyet Yönetimi + faturalandırma** > **maliyet Yönetimi**.
3. Altında **ayarları**seçin **bulut bağlayıcılar (Önizleme)** .  
    ![Bulut bağlayıcılar (Önizleme) gösteren örnek ayar)](./media/aws-integration-setup-configure/cloud-connectors-preview01.png).
4. Seçin **+ Ekle** bir bağlayıcı oluşturmak için sayfanın üst kısmındaki.
5. Üzerinde **AWS Bağlayıcısı oluşturma** sayfasında **görünen ad**, Bağlayıcınız için bir ad girin.  
    ![Bir AWS Bağlayıcısı oluşturma sayfası örneği](./media/aws-integration-setup-configure/create-aws-connector01.png)
6. İsteğe bağlı olarak, varsayılan yönetim grubu seçin. Tüm bulunan bağlantılı hesaplar, depolar. Bunu daha sonra ayarlayabilirsiniz.
7. İçinde **faturalama** bölümünden **%1 genel kullanım aşamasında otomatik olarak ücret** Önizleme süresi dolduğunda işlem sürekli emin olmak istiyorsanız. Otomatik seçeneğini belirlerseniz, bir faturalama aboneliği seçmeniz gerekir.
8. İçin **rol ARN**, AWS rolünde ayarlarken kullandığınız değeri girin.
9. İçin **Dış kimlik**, AWS rolünde ayarlarken kullandığınız değeri girin.
10. İçin **rapor adı**, AWS'de oluşturulan bir ad girin.
11. Seçin **sonraki** seçip **Oluştur**.

Yeni AWS kapsamlar için birkaç saat sürebilir, AWS hesabı ve bağlı AWS hesapları ve maliyet verilerini görünmesini birleştirilir.

Bağlayıcı oluşturduktan sonra erişim denetimi için atamanızı öneririz. Kullanıcılar, yeni bulunan kapsamlar için izinleri atanır: AWS hesabı ve bağlı AWS hesapları birleştirilir. Bağlayıcı oluşturan kullanıcı, bağlayıcı, birleştirilmiş hesabı ve bağlı tüm hesapları sahibidir.

Bulma tamamlandıktan sonra bağlayıcı izinlerini kullanıcılara atama izinleri mevcut AWS kapsamına atamaz. Bunun yerine, yalnızca yeni bağlantılı hesaplar izinleri atanır.

## <a name="take-additional-steps"></a>Ek adımlar uygulayın

- [Yönetim gruplarını ayarlama](../governance/management-groups/index.md#initial-setup-of-management-groups), henüz yapmadıysanız.
- Yeni kapsamlar kapsam seçicinizde eklenen denetleyin. Seçin **Yenile** en son verileri görüntülemek için.
- Üzerinde **bulut bağlayıcılarını** sayfası, bağlayıcınızın seçip **fatura hesabınıza gidin** hesabın bağlı yönetim gruplarına atamak için.

## <a name="manage-cloud-connectors"></a>Bulut bağlayıcılarını yönetme

Seçtiğinizde, bir bağlayıcı üzerinde **bulut bağlayıcılarını** sayfasında, şunları yapabilirsiniz:

- Seçin **fatura hesabınıza gidin** AWS bilgilerini görüntülemek için hesap birleştirilir.
- Seçin **erişim denetimi** Bağlayıcısı rol atamasını yönetmek için.
- Seçin **Düzenle** Bağlayıcısı'nı güncelleştirmek için. Rol ARN göründüğünden, AWS hesap numarası değiştiremezsiniz. Ancak, yeni bir bağlayıcı oluşturabilirsiniz.
- Seçin **doğrulama** maliyet Yönetimi Bağlayıcısı ayarlarını kullanarak veri toplayabilir emin olmak için doğrulama testi yeniden çalıştırın.

![Örnek oluşturulan AWS bağlayıcıların listesi](./media/aws-integration-setup-configure/list-aws-connectors.png)

## <a name="set-up-azure-management-groups"></a>Azure Yönetim gruplarını ayarlama

Bulutlar arası sağlayıcısı bilgilerini görüntülemek için tek bir yerde oluşturmak için Azure Abonelikleriniz ve bağlı AWS hesapları aynı yönetim grubunda yerleştirmek gerekir. Yönetim grubuyla Azure ortamınızda önce yapılandırmadıysanız, bkz. [ilk kurulumu yönetim gruplarının](../governance/management-groups/index.md#initial-setup-of-management-groups).

Maliyetleri ayırmak istiyorsanız, yalnızca bağlantılı AWS hesapları içeren bir yönetim grubu oluşturabilirsiniz.

## <a name="set-up-an-aws-consolidated-account"></a>Hesap bir AWS ayarlama birleştirilmiş

Faturalandırma ve ödeme birden çok AWS hesapları için birleştirilmiş AWS hesabı birleştirir. Ayrıca, AWS bağlı hesabı da çalışır.

![AWS örnek ayrıntılarını hesabı birleştirilir.](./media/aws-integration-setup-configure/aws-consolidated-account01.png)

Sayfasında, şunları yapabilirsiniz:

- Seçin **güncelleştirme** ilişkisini AWS hesapları ile bir yönetim grubu bağlı toplu güncelleştirme.
- Seçin **erişim denetimi** rol ataması kapsam için ayarlanacak.

### <a name="permissions-for-an-aws-consolidated-account"></a>Hesap bir AWS izinlerini birleştirilmiş

Varsayılan olarak, hesap oluşturma, AWS bağlayıcı izinlere göre birleştirilmiş AWS hesabı için izinler ayarlanır. Bağlayıcı oluşturan sahiptir.

Erişim düzeyi kullanarak yönettiğiniz **erişim düzeyi** AWS sayfasının birleştirilmiş hesabı. Ancak, AWS bağlantılı hesapları yoksa birleştirilmiş AWS hesabı izinleri devralır.

## <a name="set-up-an-aws-linked-account"></a>Bir AWS bağlı hesabınızı oluşturun

AWS bağlı AWS kaynaklarını nerede oluşturulabilir ve yönetilebilir hesaptır. Bağlı bir hesabı da bir güvenlik sınırı olarak davranır.

Bu sayfadan şunları yapabilirsiniz:

- Seçin **güncelleştirme** AWS ilişkisini güncelleştirmek için bir yönetim grubu hesabıyla bağlantılı.
- Seçin **erişim denetimi** bir rol ataması kapsam için ayarlanacak.

![AWS bağlı hesabı sayfası örneği](./media/aws-integration-setup-configure/aws-linked-account01.png)

### <a name="permissions-for-an-aws-linked-account"></a>AWS izinlerini hesabı bağlı

Varsayılan olarak, bağlı AWS hesabı için izinler oluşturma, AWS bağlayıcı izinlere göre ayarlanır. Bağlayıcı oluşturan sahiptir. Erişim düzeyi kullanarak yönettiğiniz **erişim düzeyi** AWS sayfasının hesabına bağlı. Bağlantılı AWS hesapları birleşik AWS hesabınız izinleri devralır yok.

AWS hesapları bağlı her zaman ait oldukları yönetim grubundan izinleri devralır.

## <a name="next-steps"></a>Sonraki adımlar

- Ayarlama ve AWS maliyet ve kullanım raporu tümleştirmesi yapılandırılmış göre devam [yönetme AWS maliyetlerinden ve kullanım](aws-integration-manage.md).
- Maliyet Analizi ile bilmiyorsanız bkz [maliyet analizi ile maliyetleri analiz](quick-acm-cost-analysis.md) hızlı başlangıç.
- Azure'da bütçelerle emin değilseniz bkz [oluşturun ve Azure bütçelerini yönetin](tutorial-acm-create-budgets.md).
