Bir tetikleyici eklediğiniz, bir şeyler için kendi zaman tetik tarafından oluşturulan verilerle ilginç. Eklemek için aşağıdaki adımları izleyin **SharePoint Online - dosyası oluşturma** eylem. Bu eylem bir dosyası SharePoint Online'da yeni öğe tetiği harekete her zaman oluşturulur. 

Bu eylem, aşağıdaki bilgileri sağlamanız gerekir. Bu bilgiler gibi bazı yeni dosyasının özellikleri için giriş olarak tetik tarafından oluşturulan kullanımı kolay veri olduğunu görürsünüz:

| Dosya özelliği oluşturma | Açıklama |
| --- | --- |
| Site URL'si |Yeni dosya oluşturmak istediğiniz SharePoint Online site URL'sidir. Site sunulan listeden seçin. |
| Klasör yolu |Bu klasörüdür (Site URL'de önceki adımda seçtiğiniz) burada yeni dosya yerleştirilir. Göz atın ve klasörü seçin. |
| Dosya adı |Oluşturulan dosyanın adıdır. |
| Dosya içeriği |Dosyaya yazılan içerik. |

1. Seçin **+ yeni adım** eylem eklemek için.  
   ![SharePoint online eylem görüntü 1](./media/connectors-create-api-sharepointonline/action-1.png)  
2. Seçin **Eylem Ekle** bağlantı. Bu açılır, herhangi bir işlem arayabileceğiniz arama kutusu yapmak istiyorsunuz. Bu örnekte, SharePoint ilgi eylemlerdir.    
   ![SharePoint online eylem görüntü 2](./media/connectors-create-api-sharepointonline/action-2.png)    
3. Girin *sharepoint* SharePoint ile ilgili eylemler için aranacak.
4. Seçin **SharePoint Online - dosyası oluşturma** yapılacak eylem olarak.   **Not**: SharePoint Online bağlantı daha önce oluşturmadıysanız, SharePoint hesabınıza erişmeniz için mantıksal uygulamanızı yetkilendirmek istenir.    
   ![SharePoint online eylem görüntüsü 3](./media/connectors-create-api-sharepointonline/action-3.png)    
5. **Dosyası oluştur** kontrol açar.   
   ![SharePoint online eylem görüntüsü 4](./media/connectors-create-api-sharepointonline/action-4.png)     
6. Seçin **Site URL'si** ve olduğu gibi dosyayı oluşturmak için site bulmak için Gözat.     
   ![SharePoint online eylem görüntüsü 5](./media/connectors-create-api-sharepointonline/action-5.png)  
7. Seçin **klasör yolu** burada yeni dosya yerleştirilecek klasörü bulmak üzere göz atın.  
   ![SharePoint online eylem görüntüsü 6](./media/connectors-create-api-sharepointonline/action-6.png)  
8. Seçin **dosya adı** denetlemek ve oluşturmak istediğiniz dosyanın adını girin. Burada, dosya adı doğrudan girebilir veya daha önce oluşturduğunuz tetikleyici özelliklerinden birini kullanabilirsiniz. Özellikleri listesinden seçerek bunu **yeni bir öğe oluşturulduğunda gelen çıkışları**. Siz seçtikten sonra bu listede yalnızca görüntüdür **dosya adı** denetim. Bu walkthough içinde kimliği (Yeni liste öğesinin kimliği) tarafından oluşturulan dosya adı olarak seçtiğim **SharePoint Online - dosyası oluşturma** eylem.    
   ![SharePoint online eylem görüntüsü 7](./media/connectors-create-api-sharepointonline/action-7.png)  
9. Seçin **dosya içeriği** denetlemek ve oluşturulacak dosyasına yazılır içeriği girin. Dosya içeriği için daha önce oluşturduğunuz tetikleyici özelliklerinden birini kullanabilirsiniz dikkat edin. Yalnızca özellikleri sunulan listeden seçin. Alternatif olarak, girebilirsiniz **dosya içeriği** doğrudan denetime metin. Bu örnekte, ı seçili bazı özellikleri ve alanları ve her bir özellik arasında tire eklenir.        
   ![SharePoint online eylem görüntüsü 8](./media/connectors-create-api-sharepointonline/action-8.png)  
10. İş akışınıza Değişiklikleri Kaydet  
11. Tebrikler, artık bir SharePoint Online listesine yeni bir öğe eklendiğinde tetikleyen bir tam olarak işlevsel mantıksal uygulama vardır. Uygulama, ardından yeni liste öğesi özelliklerinden bazıları kullanarak, bir dosya oluşturur.  Artık, yeni bir öğe SharePoint listesindeki oluşturarak sınayabilirsiniz. 

