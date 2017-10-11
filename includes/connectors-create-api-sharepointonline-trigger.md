Bu örnekte, ı size nasıl kullanılacağını gösterir **SharePoint yeni bir öğe oluşturulduğunda çevrimiçi -** yeni bir öğe, bir SharePoint Online listesinde oluşturulduğunda bir mantıksal uygulama iş akışını başlatmak için tetikleyici.

> [!NOTE]
> Değil zaten oluşturduysanız SharePoint hesabınızda oturum açın istenir bir *bağlantı* SharePoint Online.  
> 
> 

1. Girin *sharepoint* arama kutusuna logic apps tasarımcısında seçip **SharePoint yeni bir öğe oluşturulduğunda çevrimiçi -** tetikleyici.  
   ![SharePoint online tetikleyici görüntüsü](./media/connectors-create-api-sharepointonline/trigger-1.png)  
2. **Yeni bir öğe oluşturulduğunda** denetim görüntülenir.  
   ![SharePoint online tetikleyici görüntü 2](./media/connectors-create-api-sharepointonline/trigger-2.png)   
3. Seçin bir **Site URL'si**. Yeni öğeler, iş akışını tetikleyen izlemek istediğiniz SharePoint online sitesi budur.  
   ![SharePoint online tetikleyici görüntü 3](./media/connectors-create-api-sharepointonline/trigger-3.png)   
4. Seçin bir **listesi adı**. Bu iş akışını tetikleyecek yeni öğeler için izlemek istediğiniz SharePoint Online sitesindeki listesidir.  
   ![SharePoint online tetikleyici görüntü 4](./media/connectors-create-api-sharepointonline/trigger-4.png)   

Bu noktada, mantıksal uygulamanızı diğer tetikleyiciler ve Eylemler iş akışı bir farklı çalıştır başlayacak bir tetikleyici ile yapılandırıldı. Bu yeni bir öğe seçtiğiniz SharePoint Online listesinde her oluşturulduğunda gerçekleşir.  

