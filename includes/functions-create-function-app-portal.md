1. Azure portalının sol üst köşesinde bulunan **Yeni** düğmesine tıklayın.

1. Tıklatın **işlem** > **işlev uygulamanız**. Ardından tabloda belirtilen işlev uygulaması ayarlarını kullanın.

    ![Azure portalında işlev uygulaması oluşturma](./media/functions-create-function-app-portal/function-app-create-flow.png)

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Uygulama adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı tanımlayan ad. Geçerli karakterler `a-z`, `0-9`, ve `-`.  | 
    | **Abonelik** | Aboneliğiniz | Bu yeni bir işlev uygulaması altında oluşturulacağı abonelik. | 
    | **[Kaynak Grubu](../articles/azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | İşlev uygulamanızın oluşturulacağı yeni kaynak grubunun adı. | 
    | **[Barındırma planı](../articles/azure-functions/functions-scale.md)** |   Tüketim planı | Kaynakların işlev uygulamanıza nasıl ayrılacağını tanımlayan barındırma planı. Varsayılan **Tüketim Planı**'nda kaynaklar işlevlerin taleplerine göre dinamik olarak eklenir. Yalnızca işlevlerinizin çalıştığı süre için ödeme yaparsınız.   |
    | **Konum** | Batı Avrupa | Kendinize veya işlevinizin erişeceği diğer hizmetlere yakın bir konum seçin. |
    | **[Depolama hesabı](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account)** |  Genel olarak benzersiz bir ad |  İşlev uygulamanız tarafından kullanılan yeni depolama hesabının adı. Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir. Var olan bir hesabı da kullanabilirsiniz. |

1. Yeni işlev uygulamasını sağlamak ve dağıtmak için **Oluştur**'a tıklayın.
