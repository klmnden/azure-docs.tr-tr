1. Tıklatın **yeni** düğmesi bulunan Azure portal, sol üst köşesinde sonra seçin **işlem** > **işlev uygulaması**. 

    ![Azure portalında bir işlev uygulaması oluşturma](./media/functions-create-function-app-portal/function-app-create-flow.png)

2. Görüntünün aşağıdaki tabloda belirtildiği gibi işlev uygulama ayarları kullanın.

    ![Yeni işlev uygulama ayarlarını tanımla](./media/functions-create-function-app-portal/function-app-create-flow2.png)

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Uygulama adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı tanımlayan ad. Geçerli karakterler `a-z`, `0-9`, ve `-`.  | 
    | **Abonelik** | Aboneliğiniz | Bu yeni bir işlev uygulaması oluşturulduğu abonelik. | 
    | **[Kaynak Grubu](../articles/azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | İşlev uygulamanızın oluşturulacağı yeni kaynak grubunun adı. | 
    | **İŞLETİM SİSTEMİ** | Windows | Sunucusuz barındırma şu anda yalnızca Windows üzerinde çalışan olduğunda kullanılabilir. Linux barındırmak için bkz: [Azure CLI kullanarak Linux üzerinde çalışan ilk işlevinizi oluşturma](../articles/azure-functions/functions-create-first-azure-function-azure-cli-linux.md). |
    | **[Barındırma planı](../articles/azure-functions/functions-scale.md)** |   Tüketim planı | Kaynakların işlev uygulamanıza nasıl ayrılacağını tanımlayan barındırma planı. Varsayılan **Tüketim Planı**'nda kaynaklar işlevlerin taleplerine göre dinamik olarak eklenir. Bu [sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) barındırma, yalnızca işlevlerinizi çalışma zamanı için ödeme yaparsınız.   |
    | **Konum** | Batı Avrupa | Seçin bir [bölge](https://azure.microsoft.com/regions/) yakın veya diğer hizmetler yakın işlevlerinizi erişim. |
    | **[Depolama hesabı](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account)** |  Genel olarak benzersiz bir ad |  İşlev uygulamanız tarafından kullanılan yeni depolama hesabının adı. Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir. Var olan bir hesabı da kullanabilirsiniz. |

1. Yeni işlev uygulamasını sağlamak ve dağıtmak için **Oluştur**'a tıklayın. Portalın sağ üst köşedeki bildirim simgesine tıklayarak dağıtım durumunu izleyebilirsiniz. 

    ![Yeni işlev uygulama ayarlarını tanımla](./media/functions-create-function-app-portal/function-app-create-notification.png)

    Tıklatarak **kaynağa gidin** yeni işlev uygulamanız alır.
