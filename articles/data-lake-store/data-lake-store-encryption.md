---
title: Azure Data Lake depolama Gen1 şifreleme | Microsoft Docs
description: Azure Data Lake depolama Gen1 şifreleme, verilerinizi koruma, Kurumsal güvenlik ilkeleri uygulama ve yasal uyumluluk gereksinimlerini karşılamaya yardımcı olur. Bu makale tasarıma genel bir bakış sunarken uygulamanın birkaç teknik yönünü ele almaktadır.
services: data-lake-store
documentationcenter: ''
author: esung22
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: yagupta
ms.openlocfilehash: a009f212bd8baaa353d602dc6090aeeccddd4936
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60878451"
---
# <a name="encryption-of-data-in-azure-data-lake-storage-gen1"></a>Azure Data Lake depolama Gen1 veri şifreleme

Azure Data Lake depolama Gen1 şifreleme, verilerinizi koruma, Kurumsal güvenlik ilkeleri uygulama ve yasal uyumluluk gereksinimlerini karşılamaya yardımcı olur. Bu makale tasarıma genel bir bakış sunarken uygulamanın birkaç teknik yönünü ele almaktadır.

Data Lake depolama Gen1 hem bekleyen hem de Aktarımdaki verilerin şifrelenmesini destekler. Bekleyen veriler için Data Lake depolama Gen1 destekler "üzerinde varsayılan olarak," saydam şifrelemeyi. Bu terimlerin biraz daha ayrıntılı olarak anlamı şudur:

* **Üzerinde varsayılan olarak**: Yeni bir Data Lake depolama Gen1 hesabı oluştururken varsayılan ayar şifrelemeyi etkinleştirir. Bundan sonra Data Lake depolama Gen1 içinde depolanan veriler her zaman kalıcı medyada depolanmadan önce şifrelenir. Bu durum tüm veriler için geçerlidir ve bir hesap oluşturulduktan sonra değiştirilemez.
* **Saydam**: Data Lake depolama Gen1 otomatik olarak kaydetmeden önce verileri şifreler ve almadan öncesinde verilerin şifresini çözer. Şifreleme yapılandırılır ve Data Lake depolama Gen1 hesap düzeyinde bir yönetici tarafından yönetilir. Veri erişimi API'lerinde hiçbir değişiklik yapılmaz. Bu nedenle, değişiklik bulunan uygulamalarda ve hizmetlerde şifreleme Data Lake depolama Gen1 ile etkileşim kuran gerekli değildir.

(Diğer adıyla Hareket halindeki) veriler Aktarımdaki verileri Data Lake depolama Gen1 içinde de her zaman şifrelenir. Kalıcı medyaya depolama önce veri şifrelemeye ek olarak, aktarımdaki veriler de her zaman HTTPS kullanılarak korunmaktadır. HTTPS, Data Lake depolama Gen1 REST arabirimleri için desteklenen tek protokoldür. Aşağıdaki diyagramda, Data Lake depolama Gen1 içinde verilerin nasıl şifrelendiği gösterilmektedir:

![Data Lake depolama Gen1 veri şifreleme diyagramı](./media/data-lake-store-encryption/fig1.png)


## <a name="set-up-encryption-with-data-lake-storage-gen1"></a>Data Lake depolama Gen1 ile şifreleme ayarlama

Şifreleme Data Lake depolama Gen1 için hesap oluşturma sırasında ayarlanır ve her zaman varsayılan olarak etkindir. Anahtarları kendiniz yönetebilir veya Data Lake depolama Gen1 (varsayılan değer budur), bunları yönetmek izin verebilirsiniz.

Daha fazla bilgi için [Başlarken](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal) bölümüne bakın.

## <a name="how-encryption-works-in-data-lake-storage-gen1"></a>Data Lake depolama Gen1 içinde şifreleme nasıl çalışır

Aşağıdaki bilgiler ana şifreleme anahtarlarını yönetme konusunu kapsar ve Data Lake depolama Gen1 için Veri şifrelemede kullanabileceğiniz üç farklı türleri açıklanmaktadır.

### <a name="master-encryption-keys"></a>Ana şifreleme anahtarları

Data Lake depolama Gen1 ana şifreleme anahtarlarının (Mek'ler) yönetimi için iki mod sağlar. Şimdilik, ana şifreleme anahtarının en üst düzey anahtar olduğunu varsayabiliriz. Data Lake depolama Gen1 içinde depolanan verilerin şifresini çözmek için ana şifreleme anahtarına erişim gereklidir.

Ana şifreleme anahtarını yönetmek için kullanılan iki mod şunlardır:

*   Hizmet tarafından yönetilen anahtarlar
*   Müşteri tarafından yönetilen anahtarlar

Her iki modda da ana şifreleme anahtarı Azure Key Vault’ta depolanarak güvenlik altına alınır. Azure üzerinde tam olarak yönetilen, yüksek güvenlikli bir hizmet olan Key Vault, şifreleme anahtarlarını korumak için kullanılabilir. Daha fazla bilgi için bkz. [Key Vault](https://azure.microsoft.com/services/key-vault).

MEK’leri yönetmek için kullanılan iki modun sağladığı özelliklerin kısa bir karşılaştırması aşağıdadır.

|  | Hizmet tarafından yönetilen anahtarlar | Müşteri tarafından yönetilen anahtarlar |
| --- | --- | --- |
|Veriler nasıl depolanır?|Depolanmadan önce her zaman şifrelenir.|Depolanmadan önce her zaman şifrelenir.|
|Ana Şifreleme Anahtarı nerede depolanır?|Key Vault|Key Vault|
|Key Vault dışında açıkta saklanan şifreleme anahtarı var mı? |Hayır|Hayır|
|Key Vault’tan MEK alınabilir mi?|Hayır. MEK Key Vault’ta depolandıktan sonra yalnızca şifreleme ve şifre çözme amacıyla kullanılabilir.|Hayır. MEK Key Vault’ta depolandıktan sonra yalnızca şifreleme ve şifre çözme amacıyla kullanılabilir.|
|Key Vault örneği ve MEK kime aittir?|Data Lake depolama Gen1 hizmeti|Kendi Azure aboneliğiniz kapsamında bulunan Key Vault örneği size aittir. Key Vault’taki MEK, yazılım veya donanım tarafından yönetilebilir.|
|Data Lake depolama Gen1 hizmeti için MEK erişimini iptal edebilir mi?|Hayır|Evet. Key vault'taki erişim denetim listelerini yönetebilir ve Data Lake depolama Gen1 hizmeti için hizmet kimliği erişim denetimi girdilerini kaldırın.|
|MEK’i kalıcı olarak silebilir misiniz?|Hayır|Evet. Key Vault'taki MEK'i silerseniz, herkes tarafından Data Lake depolama Gen1 hizmeti de dahil olmak üzere Data Lake depolama Gen1 hesabındaki verilerin şifresi çözülemiyor. <br><br> MEK’i Key Vault'tan silmeden önce özellikle yedeklediyseniz, MEK geri yüklenebilir ve veriler kurtarılabilir. Yukarı MEK'i Key Vault'tan silmeden önce yedeklemediyseniz, ancak Data Lake depolama Gen1 hesabındaki verilerin hiçbir zaman şifresi çözülemez.|


MEK ve MEK’in içinde bulunduğu Key Vault’un kim tarafından yönetildiği konusundaki bu farklılık dışında, tasarımın geri kalanı her iki mod için aynıdır.

Ana şifreleme anahtarları için modu seçtiğinizde aşağıdakileri unutmamanız gerekir:

*   Bir Data Lake depolama Gen1 hesabı sağladığınızda müşteri anahtarlar veya yönetilen hizmet anahtarları yönetilen olmadığını seçebilirsiniz.
*   Bir Data Lake depolama Gen1 hesabı oluşturulduktan sonra mod değiştirilemez.

### <a name="encryption-and-decryption-of-data"></a>Verilerin şifrelenmesi ve şifresinin çözülmesi

Veri şifreleme tasarımında kullanılan üç tür anahtar vardır. Aşağıdaki tabloda bir özet verilmektedir:

| Anahtar                   | Kısaltma | İlişkili olduğu yer: | Depolama konumu                             | Tür       | Notlar                                                                                                   |
|-----------------------|--------------|-----------------|----------------------------------------------|------------|---------------------------------------------------------------------------------------------------------|
| Ana Şifreleme Anahtarı | MEK          | Bir Data Lake depolama Gen1 hesabı | Key Vault                              | Asimetrik | Data Lake depolama Gen1 veya sizin tarafınızdan yönetilebilir.                                                              |
| Veri Şifreleme Anahtarı   | DEK          | Bir Data Lake depolama Gen1 hesabı | Data Lake depolama Gen1 hizmet tarafından yönetilen kalıcı depolama | Simetrik  | DEK, MEK ile şifrelenir. Şifrelenmiş DEK, kalıcı medyada depolanır. |
| Blok Şifreleme Anahtarı  | BEK          | Bir veri bloğu | None                                         | Simetrik  | BEK, DEK’ten ve veri bloğundan türetilir.                                                      |

Aşağıdaki diyagram bu kavramları göstermektedir:

![Veri şifrelemesindeki anahtarlar](./media/data-lake-store-encryption/fig2.png)

#### <a name="pseudo-algorithm-when-a-file-is-to-be-decrypted"></a>Bir dosyanın şifresinin çözülmesi için kullanılan genel algoritma:
1.  Data Lake depolama Gen1 hesabının DEK'inin önbelleğe alınmış ve kullanıma hazır olup olmadığını denetleyin.
    - Bu koşulları karşılamıyorsa, kalıcı depolamadan şifrelenmiş DEK’i okuyun ve şifresinin çözülmesi için Key Vault’a gönderin. Şifresi çözülmüş DEK’i bellekte önbelleğe alın. Artık kullanıma hazırdır.
2.  Dosyadaki her veri bloğu için:
    - Kalıcı depolama alanından şifrelenmiş veri bloğu okunur.
    - DEK’ten ve şifrelenmiş veri bloğundan BEK oluşturulur.
    - Verilerin şifresini çözmek için BEK kullanılır.


#### <a name="pseudo-algorithm-when-a-block-of-data-is-to-be-encrypted"></a>Bir veri bloğu şifrelenecek olduğunda genel algoritma:
1.  Data Lake depolama Gen1 hesabının DEK'inin önbelleğe alınmış ve kullanıma hazır olup olmadığını denetleyin.
    - Bu koşulları karşılamıyorsa, kalıcı depolamadan şifrelenmiş DEK’i okuyun ve şifresinin çözülmesi için Key Vault’a gönderin. Şifresi çözülmüş DEK’i bellekte önbelleğe alın. Artık kullanıma hazırdır.
2.  DEK’ten veri bloğu için benzersiz bir BEK oluşturulur.
3.  Veri bloğu, BEK ile AES-256 şifreleme kullanılarak şifrelenir.
4.  Şifrelenmiş veri bloğu kalıcı depolama alanında depolanır.

> [!NOTE] 
> DEK her zaman, kalıcı medyada veya bellekte önbelleğe alınmış olarak MEK tarafından şifrelenmiş olarak depolanır.

## <a name="key-rotation"></a>Anahtar döndürme

Müşteri tarafından yönetilen anahtarları kullanırken MEK’i döndürebilirsiniz. Müşteri tarafından yönetilen anahtarlarla Data Lake depolama Gen1 hesabınızı hakkında bilgi edinmek için bkz: [Başlarken](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal).

### <a name="prerequisites"></a>Önkoşullar

Data Lake depolama Gen1 hesabınızı ayarlarken kendi anahtarlarınızı kullanmayı seçtiniz. Bu seçenek, hesap oluşturulduktan sonra değiştirilemez. Aşağıdaki adımlarda, müşteri tarafından yönetilen anahtarlar kullandığınız (Key Vault'tan kendi anahtarlarınızı seçtiğiniz) varsayılır.

Şifreleme için varsayılan seçenekleri kullanırsanız, verileriniz her zaman Data Lake depolama Gen1 tarafından yönetilen anahtarlar kullanılarak şifrelendiğini unutmayın. Bu seçenekte, Data Lake depolama Gen1 tarafından yönetildikleri için anahtarları döndüremezsiniz özelliği yok.

### <a name="how-to-rotate-the-mek-in-data-lake-storage-gen1"></a>Data Lake depolama Gen1, MEK döndürme

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Data Lake depolama Gen1 hesabınızla ilişkili anahtarlarınızı depolayan Key Vault örneğine göz atın. **Anahtarlar**’ı seçin.

    ![Key Vault ekran görüntüsü](./media/data-lake-store-encryption/keyvault.png)

3. Data Lake depolama Gen1 hesabınızla ilişkili anahtarı seçin ve bu anahtarın yeni bir sürümünü oluşturun. Data Lake depolama Gen1 şu anda yalnızca bir anahtarın yeni bir sürüme anahtar döndürmeyi desteklediğini unutmayın. Farklı bir anahtara döndürmeyi desteklemez.

   ![Yeni Sürümün vurgulandığı Anahtarlar penceresi ekran görüntüsü](./media/data-lake-store-encryption/keynewversion.png)

4. Data Lake depolama Gen1 hesabına Gözat ve Seç **şifreleme**.

   ![Şifreleme'nin vurgulandığı ekran görüntüsü Data Lake depolama Gen1 hesabı penceresinin](./media/data-lake-store-encryption/select-encryption.png)

5. Yeni bir anahtar sürümünün mevcut olduğu bir ileti ile bildirilir. Anahtarı yeni sürüme güncelleştirmek için **Anahtarı Döndür** seçeneğine tıklayın.

   ![İleti ve anahtarı Döndür seçenekleri vurgulanmış ekran Data Lake depolama Gen1 penceresi](./media/data-lake-store-encryption/rotatekey.png)

Bu işlem iki dakikadan kısa sürer ve anahtar döndürme nedeniyle beklenen kapalı kalma süresi yoktur. İşlem tamamlandıktan sonra anahtarın yeni sürümü kullanılır.

> [!IMPORTANT]
> Anahtar döndürme işlemi tamamlandıktan sonra anahtarın eski sürümü artık verilerinizi şifrelemek için etkin şekilde kullanılmaz.  Ancak verilerinizin yedek kopyalarının etkilendiği nadiren de olsa karşılaşılan beklenmedik hata durumlarında veriler halen eski anahtarı kullanan bir yedeklemeden geri yüklenebilir. Verilerinizin bu tür nadir durumlarda erişilebilir olmasını sağlamak için, şifreleme anahtarınızın önceki sürümünün bir kopyasını saklayın. Bkz: [olağanüstü durum kurtarma kılavuzu için verileri Data Lake depolama Gen1](data-lake-store-disaster-recovery-guidance.md) olağanüstü durum kurtarma planlaması için en iyi uygulamalar için. 