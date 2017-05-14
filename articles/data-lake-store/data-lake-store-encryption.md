---
title: "Azure Data Lake Store&quot;da şifreleme | Microsoft Belgeleri"
description: "Şifreleme ve anahtar döndürmenin Azure Data Lake Store&quot;da nasıl çalıştığını anlama"
services: data-lake-store
documentationcenter: 
author: yagupta
manager: 
editor: 
ms.assetid: 
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 4/14/2017
ms.author: yagupta
ms.translationtype: Human Translation
ms.sourcegitcommit: db034a8151495fbb431f3f6969c08cb3677daa3e
ms.openlocfilehash: 9bd9996a4a22b2f57510b6f3b6625e22b3183b1c
ms.contentlocale: tr-tr
ms.lasthandoff: 04/29/2017

---

# <a name="encryption-of-data-in-azure-data-lake-store"></a>Azure Data Lake Store'da Veri Şifreleme

## <a name="overview-of-encryption-in-azure-data-lake-store"></a>Azure Data Lake Store'da Şifrelemeye Genel Bakış

Azure Data Lake Store’da (ADLS) Şifreleme; verilerinizi koruma, kurumsal güvenlik ilkeleri uygulama ve yasal uyumluluk gereksinimlerini karşılama olanağı sağlar. Bu makalede tasarıma genel bir bakış sunulmuş ve Data Lake Store'da veri şifrelemesinin nasıl uygulandığı konusunda teknik bilgiler verilmiştir.

ADLS, bekleyen verilerin şeffaf ve varsayılan olarak etkin bir şekilde şifrelemesini destekler. Bu terimlerin biraz daha ayrıntılı olarak anlamı şudur:

* Varsayılan olarak etkin: Yeni bir Azure Data Lake Store hesabı oluştururken varsayılan ayar şifrelemeyi etkinleştirir. Bundan sonra Data Lake Store'da depolanan veriler her zaman kalıcı medyada depolanmadan önce şifrelenir. Bu durum tüm veriler için geçerlidir ve bir hesap oluşturulduktan sonra değiştirilemez.
* Şeffaf: ADLS, kaydetmeden önce verileri şifreler ve almadan öncesinde verilerin şifresini çözer. Bu şifreleme Data Lake Store düzeyinde bir yönetici tarafından yapılandırılır ve yönetilir. Veri erişim API’lerinde değişiklik yapılmadığından Data Lake Store ile etkileşimde bulunan uygulamalarda ve hizmetlerde şifreleme için herhangi bir değişiklik yapılması gerekmez.

Data Lake Store'da aktarımdaki (hareket halindeki) veriler de her zaman şifrelenir. Kalıcı medyaya depolama önce veri şifrelemeye ek olarak, aktarımdaki veya hareket halindeki veriler de her zaman HTTPS (Güvenli Yuva Katmanı üzerinden HTTP) kullanılarak korunmaktadır. HTTPS, Data Lake Store REST arabirimleri için desteklenen tek protokoldür.

![Şekil 1](./media/data-lake-store-encryption/fig1.png)


## <a name="setting-up-encryption-with-azure-data-lake-store"></a>Azure Data Lake Store ile Şifrelemeyi Ayarlama

Azure Data Lake Store için Şifreleme, hesap oluşturma sırasında ayarlanır ve her zaman varsayılan olarak etkindir. Müşteriler anahtarları yönetebilir ya da Azure Data Lake Store’un anahtarları yönetmesine izin verebilir (varsayılan).

Azure Data Lake Store ile Şifreleme ayarlama hakkında bilgi edinmek için bkz. [Başlarken](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal).

## <a name="under-the-hood--how-encryption-works-in-azure-data-lake-store"></a>Sahne Arkası – Azure Data Lake Store'da Şifreleme nasıl çalışır

### <a name="master-encryption-keys"></a>Ana Şifreleme Anahtarları

Azure Data Lake Store, ana şifreleme anahtarlarının (MEK’ler) yönetimi için iki mod sağlar. Ana şifreleme anahtarlarının kullanımını aşağıda daha ayrıntılı açıklanmıştır. Şimdilik, ana şifreleme anahtarının en üst düzey anahtar olduğunu varsayabiliriz. Data Lake Store'da depolanan verilerin şifresini çözmek için ana şifreleme anahtarına erişim gereklidir.

Ana şifreleme anahtarını yönetmek için kullanılan iki mod şunlardır:

1.    Hizmet Tarafından Yönetilen Anahtarlar
2.    Müşteri Tarafından Yönetilen Anahtarlar

Her iki modda da ana şifreleme anahtarı Azure Key Vault’ta depolanarak güvenlik altına alınır. Azure üzerinde tam olarak yönetilen, yüksek güvenlikli bir hizmet olan Azure Key Vault, şifreleme anahtarlarını korumak için kullanılabilir. Azure Key Vault hakkında daha fazla bilgiye [buradan](https://azure.microsoft.com/services/key-vault) ulaşabilirsiniz.

MEK’leri yönetmek için kullanılan iki modun sağladığı özelliklerin kısa bir karşılaştırması aşağıdadır.

|  | Hizmet Tarafından Yönetilen Anahtarlar | Müşteri Tarafından Yönetilen Anahtarlar |
| --- | --- | --- |
|Veriler nasıl depolanır?|Depolanmadan önce her zaman şifrelenir|Depolanmadan önce her zaman şifrelenir|
|Ana Şifreleme Anahtarı nerede depolanır?|Azure Key Vault|Azure Key Vault|
|Azure Key Vault dışında açıkta saklanan şifreleme anahtarı var mı?|Hayır|Hayır|
|Azure Key Vault’tan MEK alınabilir mi?|Hayır. Key Vault’ta bir kez depolandıktan sonra yalnızca şifreleme ve şifre çözme amacıyla kullanılabilir.|Hayır. Key Vault’ta bir kez depolandıktan sonra yalnızca şifreleme ve şifre çözme amacıyla kullanılabilir.|
|Azure Key Vault ve MEK kime aittir?|Azure Data Lake Store hizmetine.|Azure Key Vault Azure aboneliğine ve dolayısıyla müşteriye aittir. Key Vault’taki MEK, yazılım veya donanım (HSM) tarafından yönetilebilir.|
|Müşteri, Azure Data Lake Store hizmetinin MEK’ine erişimi iptal edebilir mi?|Hayır|Evet. Azure Key Vault’taki erişim denetim listelerini yönetebilir ve Azure Data Lake Store hizmeti için hizmet kimliği erişim denetimi girdilerini kaldırabilirler.|
|Müşteri MEK’i kalıcı olarak silebilir mi?|Hayır|Evet. Müşteri Azure Key Vault'taki MEK’i silerse, ADLS hesabındaki verilerin şifresi Azure Data Lake Store hizmeti de dahil olmak üzere hiç kimse tarafından şifresi çözülemez. <br><br> MEK, Azure Key Vault'tan silinmeden önce müşteri tarafından özellikle yedeklendiyse geri yüklenebilir ve veriler kurtarılabilir. Ancak MEK, Azure Key Vault'tan silinmeden önce müşteri tarafından yedeklenmezse ADLS hesabındaki verilerin şifresi asla çözülemez.|


MEK ve MEK’in içinde bulunduğu Key Vault’un kim tarafından yönetildiği konusundaki üst düzey farklılıklar dışında, ikisinin de tasarımı aynıdır.

Ana şifreleme anahtarları için modu seçme açısından anımsaması gereken birkaç önemli konu vardır.

1.    Müşteri tarafından mı yoksa ADLS tarafından mı yönetilen anahtarlar kullanacağınızı seçebilirsiniz.
2.    ADLS hesabı oluşturulduktan sonra mod değiştirilemez.

### <a name="encryption-and-decryption-of-data"></a>Verilerin Şifrelenmesi ve Şifresinin Çözülmesi

Azure Data Lake’in veri şifreleme tasarımında kullanılan üç tür anahtarlar vardır. Aşağıdaki tabloda rolleri açıklanmıştır:

| Anahtar                   | Kısaltma | İlişkisi | Depolama Konumu                             | Tür       | Notlar                                                                                                   |
|-----------------------|--------------|-----------------|----------------------------------------------|------------|---------------------------------------------------------------------------------------------------------|
| Ana Şifreleme Anahtarı | MEK          | ADLS hesabı | Azure Key Vault                              | Asimetrik | ADLS tarafından veya müşteri tarafından yönetilen                                                              |
| Veri Şifreleme Anahtarı   | DEK          | ADLS hesabı | Kalıcı depolama – ADLS hizmeti tarafından yönetilen | Simetrik  | DEK, MEK tarafından şifrelenir ve hizmetteki kalıcı medyada şifrelenmiş DEK depolanır. |
| Blok Şifreleme Anahtarı  | BEK          | Bir veri bloğu | None                                         | Simetrik  | BEK, DEK’ten ve veri bloğundan türetilir.                                                      |

Aşağıdaki diyagram bu kavramları göstermektedir:

![Şekil 2](./media/data-lake-store-encryption/fig2.png)

#### <a name="pseudo-algorithm-when-a-file-is-to-be-decrypted"></a>Bir dosyanın şifresinin çözülmesi için kullanılan genel algoritma:
1.    ADLS hesabının DEK’inin önbelleğe alınmış ve kullanıma hazır olup olmadığı denetlenir.
    * Bu koşulları karşılamıyorsa, kalıcı depolamadan şifrelenmiş DEK okunur ve şifresinin çözülmesi için Azure Key Vault’a gönderilir. Şifresi çözülmüş DEK önbelleğe alınır ve kullanmak artık hazırdır.
2.    Veri dosyasındaki her blok için
    * Kalıcı depolama alanından şifrelenmiş veri bloğu okunur.
    * DEK’ten ve şifrelenmiş veri bloğundan BEK oluşturulur.
    * Verilerin şifresini çözmek için BEK kullanılır.
#### <a name="pseudo-algorithm-when-a-block-of-data-is-to-be-encrypted"></a>Bir veri bloğu şifrelenecek olduğunda genel algoritma:
1.    ADLS hesabının DEK’inin önbelleğe alınmış ve kullanıma hazır olup olmadığı denetlenir.
    * Bu koşulları karşılamıyorsa, kalıcı depolamadan şifrelenmiş DEK okunur ve şifresinin çözülmesi için Azure Key Vault’a gönderilir. Şifresi çözülmüş DEK önbelleğe alınır ve kullanmak artık hazırdır.
2.    DEK’ten veri bloğu için benzersiz bir BEK oluşturulur.
3.    Veri bloğu, BEK ile AES-256 şifreleme kullanılarak şifrelenir.
4.    Şifrelenmiş veri bloğu kalıcı depolama alanında depolanır.

> [!NOTE] 
> Performansı artırmak için, açıktaki Veri Şifreleme Anahtarı (DEK) kısa bir süreliğine önbelleğe alınır ve ilgili süre sona erdiğinde hemen silinir. Kalıcı ortamda her zaman Ana Şifreleme Anahtarı (MEK) tarafından şifrelenmiş olarak depolanır.

## <a name="key-rotation"></a>Anahtar Döndürme

Azure Data Lake Store, müşteri tarafından yönetilen anahtarlar kullanılırken Ana Şifreleme Anahtarı’nın (MEK) döndürülmesine olanak verir. Müşteri tarafından yönetilen anahtarlarla ADLS hesabınızı ayarlama hakkında bilgi edinmek için bkz. [Başlarken](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal) sayfası.

### <a name="pre-requisites"></a>Ön koşullar

Azure Data Lake hesabı ayarlarken, müşteriler kendi anahtarlarını kullanmayı seçti. Bu seçenek, hesap oluşturulduktan sonra değiştirilemez. Şifreleme için varsayılan seçenekleri kullanırsanız, verileriniz Azure Data Lake tarafından yönetilen anahtarlar kullanılarak her zaman şifrelenir, bu seçenekte anahtarlar Azure Data Lake tarafından yönetildiği için müşterinin anahtarları döngü haline getirme olanağı yoktur. Aşağıdaki adımlarda, müşteri tarafından yönetilen anahtarlar kullandığınız (Key Vault'ta kendi anahtarlarınızı seçtiğiniz) varsayılır.

### <a name="how-to-rotate-the-key-mek-in-azure-data-lake-store"></a>Azure Data Lake Store (MEK) anahtarında döngü

1. Yeni [Azure Portal](https://portal.azure.com/)'da oturum açın.
2. Azure Data Lake Store hesabınızla ilişkili anahtarlarınızı depolayan Key Vault’a gidin ve anahtarları seçin.

    ![Anahtarlar](./media/data-lake-store-encryption/keyvault.png)

3.    Azure Data Lake Store hesabınızla ilişkili anahtarı seçin ve bu anahtarın yeni bir sürümünü oluşturun.
  
   Bu noktada, Azure Data Lake yalnızca yeni bir sürüme anahtar döndürmeyi destekler, farklı bir anahtara döndürmeyi desteklemiyoruz.

   ![newversion](./media/data-lake-store-encryption/keynewversion.png)

4.    Azure Data Lake Depolama hesabına gidin ve Şifreleme’yi seçin.

    ![newversion](./media/data-lake-store-encryption/select-encryption.png)

5.    Anahtarın yeni bir sürümü olduğunu bildiren bir not ve anahtarı bu yeni sürüme döndürmenizi sağlayacak bir düğme görürsünüz. Anahtarı yeni sürüme güncelleştirmek için anahtarı döndür seçeneğine tıklayın.

    ![done](./media/data-lake-store-encryption/rotatekey.png)

6. Bu işlem 2 dakikadan kısa sürer ve anahtar döndürme nedeniyle beklenen kapalı kalma süresi yoktur. İşlem tamamlandıktan sonra anahtarın yeni sürümü kullanılır.

