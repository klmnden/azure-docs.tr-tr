---
title: Ayarlama ve Azure maliyet yönetimi ile AWS maliyet ve kullanım raporu tümleştirmeyi yapılandırma
description: Bu makalede ayarlama ve Azure maliyet yönetimi ile AWS maliyet ve kullanım raporu tümleştirme yapılandırma gösterilmektedir.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 06/06/2019
ms.topic: conceptual
ms.service: cost-management
manager: ormaoz
ms.custom: ''
ms.openlocfilehash: a7a020284f44eda0da62f307866c74b0a8df493d
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65205695"
---
# <a name="set-up-and-configure-aws-cost-and-usage-report-integration"></a>Ayarlama ve AWS maliyet ve kullanım raporu tümleştirmeyi Yapılandır

Amazon Web Services maliyet ve kullanım raporu tümleştirme sayesinde, izleyebilir ve AWS Azure maliyet Yönetimi'nde harcamalarınızı denetim. Tümleştirmesi, tek bir konum burada izleyebilirsiniz Azure portalı ve Azure ve AWS için harcama denetim sağlar. Bu makalede, tümleştirmeyi ayarlamak ve maliyetleri analiz etmek ve bütçe gözden geçirmek için maliyet yönetimi özellikleri kullanmak için yapılandırma açıklanmaktadır.

Yönetim süreçlerini rapor tanımları almak ve rapor GZIP CSV Dosyaları indirmek için AWS erişim kimlik bilgilerinizi kullanarak bir S3 demetini içinde depolanan AWS maliyet ve kullanım raporu maliyet.

## <a name="create-a-cost-and-usage-report-in-aws"></a>AWS'de bir maliyet ve kullanım raporu oluşturma

Maliyet ve kullanım raporu toplamak ve AWS maliyetlerinden işlemek için önerilen yol AWS kullanmaktır. Daha fazla bilgi için [AWS maliyet ve kullanım raporu](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-reports-costusage.html) belge makalesi.

Kullanım **raporları** faturalandırma ve maliyet Yönetimi aşağıdaki adımlarla bir maliyet ve kullanım raporu oluşturmak için AWS konsolunda sayfası.

1. AWS yönetim konsoluna oturum açın ve faturalandırma ve maliyet Yönetimi konsolunu açın https://console.aws.amazon.com/billing.
2. Gezinti bölmesinde **raporları**.
3. Tıklayın **rapor oluşturma**.
4. İçin **rapor adı**, raporunuz için bir ad yazın.
5. İçin **zaman birimi**, seçin **saatlik**.
6. İçin **INCLUDE**raporda her kaynak kimliklerini ekleyin ve seçin **kaynak kimlikleri**.
7. İçin **etkinleştirmek için destek**, hiçbir seçimi gereklidir.
8. İçin **veri yenileme ayarları**seçin **maliyetinizi yenilemesini &amp; kullanım ücretleri ile önceki ay boyunca algılanan raporu faturalar kapalı**.
9. **İleri**’ye tıklayın.
10. İçin **Amazon S3 demetini**, raporlar teslim istediğiniz Amazon S3 demetini adını yazın ve tıklayın **doğrulama**. Demet geçerli olması için uygun izinlere sahip olmalıdır. İzinleri demetine ekleme hakkında daha fazla bilgi için bkz. [ayarı demet ve nesne erişim izinleri](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/set-permissions.html).
11. İçin **rapor yol ön eki**, raporunuzun adına eklenmesini istediğiniz rapor yolu öneki yazın.
12. İçin **sıkıştırma**seçin **GZIP**.
13. **İleri**’ye tıklayın.
14. Raporunuzun tıklatın ayarları gözden geçirdikten sonra **gözden geçirme ve tam**.
    Not **rapor adı**. Sonraki adımlarda kullanır.

Bu raporlar, Amazon S3 demetini için göndermeye başlamak AWS 24 saate kadar sürebilir. Teslim başlatıldıktan sonra AWS AWS maliyet ve kullanım raporu dosyaları günde bir en az bir kez güncelleştirir. Başlamak için teslim beklemeden AWS ortamınızı yapılandırmaya devam edebilirsiniz.

## <a name="create-a-role-and-policy-in-aws"></a>AWS'de bir rol ve ilke oluşturma

Azure maliyet yönetimi, maliyet ve kullanım raporu günde birkaç kez bulunduğu S3 demetini erişir. Maliyet yönetimi, yeni verileri denetlemek için kimlik bilgilerini erişmesi gerekir. Maliyet Yönetimi tarafından erişime izin vermek için AWS rolü ve ilke oluşturun.

Rol, rol tabanlı erişim Azure maliyet Yönetimi'nde AWS hesabınız için etkinleştirmek için AWS konsolunda oluşturulur. İhtiyacınız _rol ARN_ ve _Dış kimlik_ AWS Konsolu. Daha sonra bunları Azure maliyet Yönetimi'nde bir AWS bağlayıcı sayfası kullanın.

Bir yeni rol Oluşturma Sihirbazı'nı kullanın:

1. Oturum açma, AWS konsola ve seçin **Hizmetleri**.
2. Hizmetler listesinde seçin **IAM**.
3. Seçin **rolleri** ve ardından **Rol Oluştur**.
4. Sonraki sayfada, seçin **başka bir AWS hesabı**.
5. İçinde **hesap kimliği** , girin _432263259397_.
6. İçinde **seçenekleri**seçin **dış kimliği (üçüncü taraf bu rolünü varsaymadan olduğunda en iyi uygulama) gerektiren**.
7. İçinde **Dış kimlik**, dış kimliği girin. Dış kimlik, AWS rolü ve Azure maliyet Yönetimi arasında paylaşılan bir paroladır. Aynı Dış kimlik, maliyet Yönetimi'nde yeni bir bağlayıcı sayfasındaki de kullanılır. Örneğin, bir dış kimlik benzer _Companyname1234567890123_.
    Seçimini değiştirmeyin **MFA gerektiren**. Temizlenmiş kalmalıdır.
8. Tıklayın **sonraki: İzinleri**.
9. Tıklayın **ilkesi oluşturma**. Yeni bir ilke oluşturduğunuz yeni bir tarayıcı sekmesi açılır.
10. Tıklayın **bir hizmet seçin**.

Maliyet ve kullanım raporu izni yapılandırın:

1. Tür **maliyet ve kullanım raporu**.
2. Seçin **erişim düzeyi**, **okuma** > **DescribeReportDefinitions**. Bu ven tanımlanır ve rapor tanımı önkoşul eşleşip eşleşmediğini belirlemek raporları maliyet Yönetimi okuma sağlar.
3. Tıklayın **ek izinler ayarlamanız**.

S3 demetini ve nesneleri izninizi yapılandırın:

1. Tıklayın **bir hizmet seçin**.
2. Tür _S3_.
3. Seçin **erişim düzeyi**, **listesi** > **ListBucket**. Bu eylem, içinde S3 Demetini nesneleri listesini alır.
4. Seçin **erişim düzeyi**, **okuma** > **GetObject**. Bu eylem, dosyaları indirme faturalandırma sağlar.
5. Seçin **kaynakları**.
6. Seçin **demet – ekleme ARN**.
7. İçinde **demet adı**, bu ven dosyaları depolamak için kullanılan demet girin.
8. Seçin **nesnesi – ekleme ARN**.
9. İçinde **demet adı**, bu ven dosyaları depolamak için kullanılan demet girin.
10. İçinde **nesne adı**seçin **herhangi**.
11. Tıklayın **ek izinler ayarlamanız**.

Maliyet Gezgini izni yapılandırın:

1. Tıklayın **bir hizmet seçin**.
2. Tür _Explorer hizmeti maliyet_.
3. Seçin **tüm maliyet Gezgini hizmet eylemleri (ce:\*)**. Bu eylem, koleksiyon doğru olduğunu doğrular.
4. Tıklayın **ek izinler ayarlamanız**.

Kuruluşların izin ekleyin:

1. Tür **kuruluşlar**.
2. Seçin **erişim düzeyi, liste** > **ListAccounts**. Bu eylem hesapların adlarını alır.
3. İçinde **İnceleme İlkesi**, yeni ilke için bir ad girin. Doğru bilgileri girdiğinizden emin olun ve ardından onay **ilke Oluştur**.
4. Önceki sekmeye dönün ve tarayıcınızın web sayfasını yenileyin. Arama çubuğunda, yeni ilkeniz için arama yapın.
5. Seçin **sonraki: inceleme**.
6. Yeni rol için bir ad girin. Doğru bilgileri girdiğinizden emin olun ve ardından onay **Rol Oluştur**.
    Not **rol ARN** ve **Dış kimlik** rolü oluşturduğunuz zaman önceki adımlarda kullanılır. Azure maliyet Yönetimi Bağlayıcısı'nı ayarlarken daha sonra kullanacaksınız.

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

Yeni bir AWS bağlayıcısı oluşturmak ve AWS maliyetlerinizi izlemeye başlamak için aşağıdaki bilgileri kullanın.

1. Oturum açma adresinden Azure portalında https://portal.azure.com.
2. Gidin **maliyet Yönetimi + faturalandırma** > **maliyet Yönetimi**.
3. Altında **ayarları**, tıklayın **bulut bağlayıcılar (Önizleme)**.  
    ![Bulut bağlayıcılar (Önizleme ayarı) gösteren örnek](./media/aws-integration-setup-configure/cloud-connectors-preview01.png).
4. Tıklayın **+ Ekle** yeni bir bağlayıcı oluşturmak için sayfanın üst kısmındaki.
5. Oluşturma bir AWS Bağlayıcısı sayfasının, türü bir **görünen ad** Bağlayıcınızı adlandırmak için.  
    ![Bir AWS bağlayıcı sayfası oluşturma örneği](./media/aws-integration-setup-configure/create-aws-connector01.png)
6. İsteğe bağlı olarak, varsayılan yönetim grubu seçin. Tüm bulunan bağlantılı hesaplar, depolar. Bunu daha sonra ayarlayabilirsiniz.
7. Altında **faturalama** bölümünden **%1 genel kullanım aşamasında otomatik olarak ücret** Önizleme süresi dolduğunda işlem sürekli emin olmak istiyorsanız. Otomatik seçeneğini belirlerseniz, bir faturalama seçmelisiniz **abonelik**.
8. Girin **rol ARN**. AWS rolünde ayarlarken kullandığınız değerdir.
9. Girin **dış kimliği**. AWS rolünde ayarlarken kullandığınız değerdir.
10. Girin **rapor adı** AWS'de oluşturduğunuz.
11. Tıklayın **sonraki** ve ardından **Oluştur**.

Bu yeni AWS kapsamlar, birleştirilmiş AWS hesabı ve bağlı AWS hesapları ve maliyet verilerini görünmesi birkaç saat sürebilir.

Bağlayıcı oluşturduktan sonra erişim denetimi için bağlayıcıyı atamanızı öneririz. Kullanıcılar, yeni bulunan kapsamlar için izinleri atanır: AWS hesabı ve AWS birleştirilmiş bağlantılı hesaplar. Bağlayıcı oluşturan kullanıcı, bağlayıcı, birleştirilmiş bir hesap ve tüm bağlı hesapları sahibidir.

Bulma tamamlandıktan sonra bağlayıcı izinlerini kullanıcılara atama izinleri mevcut AWS kapsamına atamaz. Bunun yerine, yalnızca yeni bağlantılı hesaplar izinleri atanır.

## <a name="additional-steps"></a>Ek adımlar

- [Yönetim gruplarını ayarlama](../governance/management-groups/index.md#initial-setup-of-management-groups), henüz yapmadıysanız.
- Yeni kapsamlar kapsam seçicinizde eklenen denetleyin. Tıklamayı unutmayın **Yenile** en son verileri görüntülemek için.
- Bulut Bağlayıcısı Sayfası'nda Bağlayıcınızı seçin ve **fatura hesabınıza gidin** hesabın bağlı yönetim gruplarına atamak için.

## <a name="manage-cloud-connectors"></a>Bulut bağlayıcılarını yönetme

Bir bağlayıcı bulut bağlayıcılar sayfasında seçtiğinizde, şunları yapabilirsiniz:

- Tıklayın **fatura hesabınıza gidin** AWS birleştirilmiş hesap bilgilerini görüntülemek için.
- Tıklayın **erişim denetimi** Bağlayıcısı rol atamasını yönetmek için.
- Tıklayın **Düzenle** Bağlayıcısı'nı güncelleştirmek için. Rol ARN içinde göründüğü gibi AWS hesabı değiştiremezsiniz. Ancak, yeni bir bağlayıcı oluşturabilirsiniz.
- Tıklayın **doğrulama** maliyet Yönetimi Bağlayıcısı ayarlarını kullanarak veri toplayabilir emin olmak için doğrulama testi yeniden çalıştırın.

![Oluşturulan AWS bağlayıcıların listesini gösteren örnek](./media/aws-integration-setup-configure/list-aws-connectors.png)

## <a name="role-of-azure-management-groups"></a>Azure yönetim gruplarının rolü

Bulutlar arası sağlayıcısı bilgilerini görüntülemek için tek bir yerde oluşturmak için Azure Abonelikleriniz ve bağlı AWS hesapları aynı yönetim grubunda yerleştirmek gerekir. Yönetim grubuyla Azure ortamınızda önce yapılandırmadıysanız, bkz. [ilk kurulumu yönetim gruplarının](../governance/management-groups/index.md#initial-setup-of-management-groups).

Maliyetleri ayırmak istiyorsanız, yalnızca bağlantılı AWS hesapları içeren bir yönetim grubu oluşturabilirsiniz.

## <a name="aws-consolidated-account"></a>AWS birleştirilmiş hesabı

AWS birleştirilmiş hesap, faturalama ve ödeme için birden çok AWS hesapları birleştirmek için kullanılır. AWS birleştirilmiş hesabınıza da AWS bağlı hesabınız davranır.

![AWS hesabı birleştirilmiş Ayrıntılar örneği](./media/aws-integration-setup-configure/aws-consolidated-account01.png)

Sayfadan şunları yapabilirsiniz:

- Tıklayın **güncelleştirme** için toplu güncelleştirme bir yönetim grubu ile AWS bağlantılı hesaplar ilişkilendirme.
- Tıklayın **erişim denetimi** rol ataması kapsam için ayarlanacak.

### <a name="aws-consolidated-account-permissions"></a>AWS birleştirilmiş hesap izinleri

Varsayılan olarak, AWS hesabı oluşturma, AWS bağlayıcı izinlere göre ayarlanmış olan izinlere birleştirilir. Bağlayıcı oluşturan sahiptir.

Erişim düzeyi, erişim düzeyi sayfasında birleştirilmiş AWS hesabı kullanarak yönetin. Ancak, AWS izinleri hesabı birleştirilmiş AWS bağlantılı hesaplar tarafından devralınmaz.

## <a name="aws-linked-account"></a>AWS bağlı hesabı

AWS bağlı AWS kaynaklarını nerede oluşturulabilir ve yönetilebilir hesaptır. Bağlı bir hesabı da bir güvenlik sınırı olarak davranır.

Bu sayfadan şunları yapabilirsiniz:

- Tıklayın **güncelleştirme** bir yönetim grubuyla bağlantılı AWS hesabı ilişkisi güncelleştirilemedi.
- Tıklayın **erişim denetimi** bir rol ataması kapsam için ayarlanacak.

![AWS bağlı hesabı sayfası örneği](./media/aws-integration-setup-configure/aws-linked-account01.png)

### <a name="aws-linked-account-permissions"></a>AWS bağlantılı hesap izinleri

Varsayılan olarak, AWS bağlantılı hesap izinlerini oluşturma, AWS bağlayıcı izinlere göre ayarlanır. Bağlayıcı oluşturan sahiptir. Erişim düzeyi, AWS bağlantılı hesabının erişim düzeyi sayfasını kullanarak yönetin. AWS birleştirilmiş hesabının izinlerini AWS bağlantılı hesaplar tarafından devralınmaz.

AWS bağlantılı hesaplar, ait oldukları yönetim grubundan her zaman izinleri devralır.

## <a name="next-steps"></a>Sonraki adımlar

- Ayarlama ve AWS maliyet ve kullanım raporu tümleştirmesi yapılandırılmış göre devam [yönetme AWS maliyetlerinden ve kullanım](aws-integration-manage.md).
- Maliyet Analizi ile bilmiyorsanız bkz [maliyet analizi ile maliyetleri analiz](quick-acm-cost-analysis.md) hızlı başlangıç.
- Azure'da bütçelerle emin değilseniz bkz [oluşturun ve Azure bütçelerini yönetin](tutorial-acm-create-budgets.md) öğretici.
