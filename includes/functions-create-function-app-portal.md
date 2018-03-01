1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** düğmesine tıklayın, ardından **İşlem** > **İşlev Uygulaması** seçeneğini belirleyin. 

    ![Azure portalında işlev uygulaması oluşturma](./media/functions-create-function-app-portal/function-app-create-flow.png)

2. Görüntünün altındaki tabloda belirtilen işlev uygulaması ayarlarını kullanın.

    ![Yeni işlev uygulaması ayarlarını tanımlama](./media/functions-create-function-app-portal/function-app-create-flow2.png)

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Uygulama adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı tanımlayan ad. Geçerli karakterler: `a-z`, `0-9`, ve `-`.  | 
    | **Abonelik** | Aboneliğiniz | Bu yeni işlev uygulamasının oluşturulduğu abonelik. | 
    | **[Kaynak Grubu](../articles/azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | İşlev uygulamanızın oluşturulacağı yeni kaynak grubunun adı. | 
    | **OS** | Windows | Sunucusuz barındırma şu anda yalnızca Windows’ta çalıştırıldığında kullanılabilir. Linux barındırması için bkz. [Azure CLI kullanarak Linux’ta çalışan ilk işlevinizi oluşturma](../articles/azure-functions/functions-create-first-azure-function-azure-cli-linux.md). |
    | **[Barındırma planı](../articles/azure-functions/functions-scale.md)** |   Tüketim planı | Kaynakların işlev uygulamanıza nasıl ayrılacağını tanımlayan barındırma planı. Varsayılan **Tüketim Planı**'nda kaynaklar işlevlerin taleplerine göre dinamik olarak eklenir. Bu [sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) barındırmada, yalnızca işlevlerinizin çalıştığı süre için ödeme yaparsınız.   |
    | **Konum** | Batı Avrupa | Kendinize veya işlevinizin erişeceği diğer hizmetlere yakın bir [bölge](https://azure.microsoft.com/regions/) seçin. |
    | **[Depolama hesabı](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account)** |  Genel olarak benzersiz bir ad |  İşlev uygulamanız tarafından kullanılan yeni depolama hesabının adı. Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir. Var olan bir hesabı da kullanabilirsiniz. |

1. Yeni işlev uygulamasını sağlamak ve dağıtmak için **Oluştur**'a tıklayın. Portalın sağ üst köşesindeki Bildirim simgesine tıklayarak dağıtımın durumunu izleyebilirsiniz. 

    ![Yeni işlev uygulaması ayarlarını tanımlama](./media/functions-create-function-app-portal/function-app-create-notification.png)

    **Kaynağa git**’e tıklamak sizi yeni işlev uygulamanıza götürür.
