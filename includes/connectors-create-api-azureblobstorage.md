### <a name="prerequisites"></a>Ön koşullar
* Bir Azure hesabı; oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free)
* Bir [Azure Blob Storage hesabı](../articles/storage/common/storage-create-storage-account.md) depolama hesabı adı ve erişim anahtarını dahil olmak üzere. Bu bilgiler, Azure Portal'da depolama hesabı özelliklerinde listelenir. Daha fazla bilgi edinin [Azure Storage](../articles/storage/common/storage-introduction.md).

Azure Blob Storage hesabınıza bir mantıksal uygulama kullanmadan önce Azure Blob Storage hesabınıza bağlanın. Kolayca Azure portalındaki mantıksal uygulama içinde bunu yapabilirsiniz.  

Aşağıdaki adımları kullanarak Azure Blob Storage hesabınıza bağlanın:  

1. Bir mantıksal uygulama oluşturun. Logic Apps Tasarımcısı'nda bir tetikleyici ekleyin ve ardından bir eylem ekleyin. Seçin **Göster Microsoft yönetilen API'ler** açılır listesinde ve ardından arama kutusuna "blob" girin. Eylemlerden birini seçin:  
   
    ![Azure Blob Depolama bağlantısı oluşturma adım](./media/connectors-create-api-azureblobstorage/azureblobstorage-1.png)  
2. Daha önce Azure depolama herhangi bir bağlantısı oluşturmadıysanız, bağlantı ayrıntılarını istenir:   
   
    ![Azure Blob Depolama bağlantısı oluşturma adım](./media/connectors-create-api-azureblobstorage/connection-details.png)  
3. Depolama hesabı ayrıntılarını girin. Bir yıldız işareti özelliklerle gereklidir.
   
   | Özellik | Ayrıntılar |
   | --- | --- |
   | Bağlantı adı * |Bağlantınız için herhangi bir ad girin. |
   | Azure depolama hesabı adı * |Depolama hesabı adı girin. Depolama hesabı adı, Azure portalında depolama özellikleri'nde görüntülenir. |
   | Azure depolama hesabı erişim tuşu * |Depolama hesabı anahtarı girin. Erişim tuşları Azure portalında depolama özellikleri görüntülenir. |
   
    Bu kimlik bilgileri, mantıksal uygulamanızı bağlanmak ve verilerinize erişmek üzere yetkilendirmek için kullanılır. 
4. **Oluştur**’u seçin.
5. Bağlantıyı oluşturan dikkat edin. Şimdi, mantıksal uygulamanızı diğer adımlarla devam edin: 
   
    ![Azure Blob Depolama bağlantısı oluşturma adım](./media/connectors-create-api-azureblobstorage/azureblobstorage-3.png)  

