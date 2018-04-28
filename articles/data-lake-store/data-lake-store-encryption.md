---
title: Azure Data Lake Store'da şifreleme | Microsoft Belgeleri
description: Azure Data Lake Store’da şifreleme; verilerinizi koruma, kurumsal güvenlik ilkeleri uygulama ve yasal uyumluluk gereksinimlerini karşılamaya yardımcı olur. Bu makale tasarıma genel bir bakış sunarken uygulamanın birkaç teknik yönünü ele almaktadır.
services: data-lake-store
documentationcenter: ''
author: esung22
manager: ''
editor: ''
ms.assetid: ''
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/26/2018
ms.author: yagupta
ms.openlocfilehash: 2328f7e233025d9f9ee9113aa28fb74754dd9193
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="encryption-of-data-in-azure-data-lake-store"></a>Azure Data Lake Store'da veri şifreleme

Azure Data Lake Store’da şifreleme; verilerinizi koruma, kurumsal güvenlik ilkeleri uygulama ve yasal uyumluluk gereksinimlerini karşılamaya yardımcı olur. Bu makale tasarıma genel bir bakış sunarken uygulamanın birkaç teknik yönünü ele almaktadır.

Data Lake Store, hem bekleyen hem de aktarımdaki verilerin şifrelenmesini destekler. Bekleyen veriler için Data Lake Store, "varsayılan olarak etkin" saydam şifrelemeyi destekler. Bu terimlerin biraz daha ayrıntılı olarak anlamı şudur:

* **Varsayılan olarak etkin**: Yeni bir Data Lake Store hesabı oluştururken varsayılan ayar, şifrelemeyi etkinleştirir. Bundan sonra Data Lake Store'da depolanan veriler her zaman kalıcı medyada depolanmadan önce şifrelenir. Bu durum tüm veriler için geçerlidir ve bir hesap oluşturulduktan sonra değiştirilemez.
* **Şeffaf**: Data Lake Store, kaydetmeden önce verileri şifreler ve almadan öncesinde verilerin şifresini çözer. Bu şifreleme Data Lake Store düzeyinde bir yönetici tarafından yapılandırılır ve yönetilir. Veri erişimi API'lerinde hiçbir değişiklik yapılmaz. Bu nedenle, Data Lake Store ile etkileşimde bulunan uygulamalarda ve hizmetlerde şifreleme için herhangi bir değişiklik yapılması gerekmez.

Data Lake Store'da aktarımdaki (diğer adıyla hareket halindeki) veriler de her zaman şifrelenir. Kalıcı medyaya depolama önce veri şifrelemeye ek olarak, aktarımdaki veriler de her zaman HTTPS kullanılarak korunmaktadır. HTTPS, Data Lake Store REST arabirimleri için desteklenen tek protokoldür. Aşağıdaki diyagramda, Data Lake Store'da verilerin nasıl şifrelendiği gösterilmektedir:

![Data Lake Store'da veri şifreleme diyagramı](./media/data-lake-store-encryption/fig1.png)


## <a name="set-up-encryption-with-data-lake-store"></a>Data Lake Store ile şifrelemeyi ayarlama

Data Lake Store için Şifreleme, hesap oluşturma sırasında ayarlanır ve her zaman varsayılan olarak etkindir. Anahtarları kendiniz yönetebilir veya Data Lake Store’un sizin için yönetmesine izin verebilirsiniz (varsayılan değer).

Daha fazla bilgi için [Başlarken](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal) bölümüne bakın.

## <a name="how-encryption-works-in-data-lake-store"></a>Data Lake Store'da şifreleme nasıl çalışır

Aşağıdaki bilgiler ana şifreleme anahtarlarını yönetme konusunu kapsar ve Data Lake Store için veri şifrelemede kullanabileceğiniz üç farklı türü açıklar.

### <a name="master-encryption-keys"></a>Ana şifreleme anahtarları

Data Lake Store, ana şifreleme anahtarlarının (MEK’ler) yönetimi için iki mod sağlar. Şimdilik, ana şifreleme anahtarının en üst düzey anahtar olduğunu varsayabiliriz. Data Lake Store'da depolanan verilerin şifresini çözmek için ana şifreleme anahtarına erişim gereklidir.

Ana şifreleme anahtarını yönetmek için kullanılan iki mod şunlardır:

*   Hizmet tarafından yönetilen anahtarlar
*   Müşteri tarafından yönetilen anahtarlar

Her iki modda da ana şifreleme anahtarı Azure Key Vault’ta depolanarak güvenlik altına alınır. Azure üzerinde tam olarak yönetilen, yüksek güvenlikli bir hizmet olan Key Vault, şifreleme anahtarlarını korumak için kullanılabilir. Daha fazla bilgi için bkz. [Key Vault](https://azure.microsoft.com/services/key-vault).

MEK’leri yönetmek için kullanılan iki modun sağladığı özelliklerin kısa bir karşılaştırması aşağıdadır.

|  | Hizmet tarafından yönetilen anahtarlar | Müşteri tarafından yönetilen anahtarlar |
| --- | --- | --- |
|Veriler nasıl depolanır?|Depolanmadan önce her zaman şifrelenir.|Depolanmadan önce her zaman şifrelenir.|
|Ana Şifreleme Anahtarı nerede depolanır?|Anahtar Kasası|Anahtar Kasası|
|Key Vault dışında açıkta saklanan şifreleme anahtarı var mı? |Hayır|Hayır|
|Key Vault’tan MEK alınabilir mi?|Hayır. MEK Key Vault’ta depolandıktan sonra yalnızca şifreleme ve şifre çözme amacıyla kullanılabilir.|Hayır. MEK Key Vault’ta depolandıktan sonra yalnızca şifreleme ve şifre çözme amacıyla kullanılabilir.|
|Key Vault örneği ve MEK kime aittir?|Data Lake Store hizmeti|Kendi Azure aboneliğiniz kapsamında bulunan Key Vault örneği size aittir. Key Vault’taki MEK, yazılım veya donanım tarafından yönetilebilir.|
|Data Lake Store hizmeti için MEK erişimini iptal edebilir misiniz?|Hayır|Evet. Azure Key Vault’taki erişim denetimi listelerini yönetebilir ve Data Lake Store hizmeti için hizmet kimliğine erişim denetimi girdilerini kaldırabilirsiniz.|
|MEK’i kalıcı olarak silebilir misiniz?|Hayır|Evet. Key Vault'taki MEK’i silerseniz, Data Lake Store hesabındaki verilerin şifresi Data Lake Store hizmeti de dahil olmak üzere hiç kimse tarafından çözülemez. <br><br> MEK’i Key Vault'tan silmeden önce özellikle yedeklediyseniz, MEK geri yüklenebilir ve veriler kurtarılabilir. Ancak, MEK’i Key Vault'tan silmeden önce yedeklemediyseniz, Data Lake Store hesabındaki verilerin şifresi asla çözülemez.|


MEK ve MEK’in içinde bulunduğu Key Vault’un kim tarafından yönetildiği konusundaki bu farklılık dışında, tasarımın geri kalanı her iki mod için aynıdır.

Ana şifreleme anahtarları için modu seçtiğinizde aşağıdakileri unutmamanız gerekir:

*   Bir Data Lake Store hesabı sağladığınızda müşteri tarafından veya hizmet tarafından yönetilen anahtarları kullanacağınızı seçebilirsiniz.
*   Data Lake Store hesabı oluşturulduktan sonra mod değiştirilemez.

### <a name="encryption-and-decryption-of-data"></a>Verilerin şifrelenmesi ve şifresinin çözülmesi

Veri şifreleme tasarımında kullanılan üç tür anahtar vardır. Aşağıdaki tabloda bir özet verilmektedir:

| Anahtar                   | Kısaltma | İlişkili olduğu yer: | Depolama konumu                             | Tür       | Notlar                                                                                                   |
|-----------------------|--------------|-----------------|----------------------------------------------|------------|---------------------------------------------------------------------------------------------------------|
| Ana Şifreleme Anahtarı | MEK          | Bir Data Lake Store hesabı | Anahtar Kasası                              | Asimetrik | Data Lake Store veya sizin tarafınızdan yönetilebilir.                                                              |
| Veri Şifreleme Anahtarı   | DEK          | Bir Data Lake Store hesabı | Kalıcı depolama, Data Lake Store hizmeti tarafından yönetilir | Simetrik  | DEK, MEK ile şifrelenir. Şifrelenmiş DEK, kalıcı medyada depolanır. |
| Blok Şifreleme Anahtarı  | BEK          | Bir veri bloğu | Yok                                         | Simetrik  | BEK, DEK’ten ve veri bloğundan türetilir.                                                      |

Aşağıdaki diyagram bu kavramları göstermektedir:

![Veri şifrelemesindeki anahtarlar](./media/data-lake-store-encryption/fig2.png)

#### <a name="pseudo-algorithm-when-a-file-is-to-be-decrypted"></a>Bir dosyanın şifresinin çözülmesi için kullanılan genel algoritma:
1.  Data Lake Store hesabının DEK’inin önbelleğe alınmış ve kullanıma hazır olup olmadığını denetleyin.
    - Bu koşulları karşılamıyorsa, kalıcı depolamadan şifrelenmiş DEK’i okuyun ve şifresinin çözülmesi için Key Vault’a gönderin. Şifresi çözülmüş DEK’i bellekte önbelleğe alın. Artık kullanıma hazırdır.
2.  Dosyadaki her veri bloğu için:
    - Kalıcı depolama alanından şifrelenmiş veri bloğu okunur.
    - DEK’ten ve şifrelenmiş veri bloğundan BEK oluşturulur.
    - Verilerin şifresini çözmek için BEK kullanılır.


#### <a name="pseudo-algorithm-when-a-block-of-data-is-to-be-encrypted"></a>Bir veri bloğu şifrelenecek olduğunda genel algoritma:
1.  Data Lake Store hesabının DEK’inin önbelleğe alınmış ve kullanıma hazır olup olmadığını denetleyin.
    - Bu koşulları karşılamıyorsa, kalıcı depolamadan şifrelenmiş DEK’i okuyun ve şifresinin çözülmesi için Key Vault’a gönderin. Şifresi çözülmüş DEK’i bellekte önbelleğe alın. Artık kullanıma hazırdır.
2.  DEK’ten veri bloğu için benzersiz bir BEK oluşturulur.
3.  Veri bloğu, BEK ile AES-256 şifreleme kullanılarak şifrelenir.
4.  Şifrelenmiş veri bloğu kalıcı depolama alanında depolanır.

> [!NOTE] 
> DEK her zaman, kalıcı medyada veya bellekte önbelleğe alınmış olarak MEK tarafından şifrelenmiş olarak depolanır.

## <a name="key-rotation"></a>Anahtar döndürme

Müşteri tarafından yönetilen anahtarları kullanırken MEK’i döndürebilirsiniz. Müşteri tarafından yönetilen anahtarlarla Data Lake Store hesabınızı ayarlama hakkında bilgi edinmek için bkz. [Başlarken](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal).

### <a name="prerequisites"></a>Ön koşullar

Data Lake Store hesabınızı ayarlarken kendi anahtarlarınızı kullanmayı seçtiniz. Bu seçenek, hesap oluşturulduktan sonra değiştirilemez. Aşağıdaki adımlarda, müşteri tarafından yönetilen anahtarlar kullandığınız (Key Vault'tan kendi anahtarlarınızı seçtiğiniz) varsayılır.

Şifreleme için varsayılan seçenekleri kullanıyorsanız, verilerinizin her zaman Data Lake Store tarafından yönetilen anahtarlar kullanılarak şifrelendiğini unutmayın. Bu seçenekte, Data Lake Store tarafından yönetildikleri için anahtarları döndüremezsiniz.

### <a name="how-to-rotate-the-mek-in-data-lake-store"></a>Data Lake Store’da MEK döndürme

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Data Lake Store hesabınızla ilişkili anahtarlarınızı depolayan Key Vault örneğine göz atın. **Anahtarlar**’ı seçin.

    ![Key Vault ekran görüntüsü](./media/data-lake-store-encryption/keyvault.png)

3.  Data Lake Store hesabınızla ilişkili anahtarı seçin ve bu anahtarın yeni bir sürümünü oluşturun. Data Lake Store şu anda yalnızca bir anahtarın yeni bir sürümüne anahtar döndürmeyi desteklemektedir. Farklı bir anahtara döndürmeyi desteklemez.

   ![Yeni Sürümün vurgulandığı Anahtarlar penceresi ekran görüntüsü](./media/data-lake-store-encryption/keynewversion.png)

4.  Data Lake Store depolama hesabına gidip **Şifreleme**’yi seçin.

    ![Şifreleme’nin vurgulandığı Data Lake Store depolama hesabı penceresinin ekran görüntüsü](./media/data-lake-store-encryption/select-encryption.png)

5.  Yeni bir anahtar sürümünün mevcut olduğu bir ileti ile bildirilir. Anahtarı yeni sürüme güncelleştirmek için **Anahtarı Döndür** seçeneğine tıklayın.

    ![İleti ve Anahtarı Döndür seçenekleri vurgulanmış Data Lake Store penceresinin ekran görüntüsü](./media/data-lake-store-encryption/rotatekey.png)

Bu işlem iki dakikadan kısa sürer ve anahtar döndürme nedeniyle beklenen kapalı kalma süresi yoktur. İşlem tamamlandıktan sonra anahtarın yeni sürümü kullanılır.

> [!IMPORTANT]
> Anahtar döndürme işlemi tamamlandıktan sonra anahtarın eski sürümü artık verilerinizi şifrelemek için etkin şekilde kullanılmaz.  Ancak verilerinizin yedek kopyalarının etkilendiği nadiren de olsa karşılaşılan beklenmedik hata durumlarında veriler halen eski anahtarı kullanan bir yedeklemeden geri yüklenebilir. Verilerinizin bu tür nadir durumlarda erişilebilir olmasını sağlamak için, şifreleme anahtarınızın önceki sürümünün bir kopyasını saklayın. Olağanüstü durum kurtarma planlamanıza yönelik en iyi uygulamalar için bkz. [Data Lake Store’da veriler için olağanüstü durum kurtarma rehberi](data-lake-store-disaster-recovery-guidance.md). 