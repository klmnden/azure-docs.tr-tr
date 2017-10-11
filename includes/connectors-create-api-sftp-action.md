Bir tetikleyici eklediğiniz, bir şeyler için kendi zaman tetik tarafından oluşturulan verilerle ilginç. Eklemek için bu adımları bir **SFTP - extract klasörü** eylem. Tanımlanan koşullar karşılanıyorsa Bu eylem bir dosyanın içeriğini ayıklayın. 

Bu eylem, aşağıdaki bilgileri sağlamanız gerekir. Yeni dosya özelliklerinden bazıları için giriş olarak tetik tarafından oluşturulan kullanımı kolay veri olduğunu fark edersiniz:

| SFTP - extract klasör özelliği | Açıklama |
| --- | --- |
| Kaynak Arşiv dosya yolu |Ayıklanırken dosyasının yolu budur. SFTP sunucunun dosya yolunu bulmak için Gözat veya önceki bir eylemden belirteçlerin seçin. |
| Hedef klasör yolu |Ayıklanan dosyaları yerleştirileceği yolu budur. Belirteçleri birini hedef yolu olarak önceki bir eylem seçin veya SFTP sunucunun göz atın ve bir yol seçin. |
| Üzerine yazılsın mı? |Ayıklanan dosyasıyla aynı ada sahip bir dosya veya varolan dosyanın üzerine, hedef klasör yolu bulunursa gösterir. |

Önceden tanımlanmış bir koşulu değerlendirilirse dosyalarını ayıklamak için eylemi ekleyerek başlayalım *doğru*. 

1. Seçin **Eylem Ekle**.        
   ![SFTP eylem koşulu görüntüsü 6](./media/connectors-create-api-sftp/condition-6.png)   
2. Seçin **SFTP - Extract klasörü** eylem      
   ![SFTP eylem koşulu görüntüsü 7](./media/connectors-create-api-sftp/condition-7.png)   
3. Seçin **kaynak Arşiv dosya yolu**              
   ![SFTP eylem koşulu görüntü 9](./media/connectors-create-api-sftp/condition-9.png)   
4. Seçin **dosya yolu** belirteci. Bu kaynak Arşiv dosya yolu tetikleyici bulunan dosyasının dosya yolunda kullanacağını gösterir.           
   ![SFTP eylem koşulu görüntüsü 10](./media/connectors-create-api-sftp/condition-10.png)   
5. Seçin **hedef klasör yolu**           
   ![SFTP eylem koşulu görüntüsü 11](./media/connectors-create-api-sftp/condition-11.png)   
6. Seçin **dosya yolu** belirteci. Bu tetikleyicinin bulunan dosyasının dosya yolunda ayıklanan dosyaları hedef yolu olarak kullanılacağını gösterir.   
7. Girin *\ExtractedFile* içinde **hedef klasör yolu** denetim. Hedef klasör yolu denetimi dosya yolu belirteç hemen sonra bunu yapabilirsiniz.         
   ![SFTP eylem koşulu görüntüsü 12](./media/connectors-create-api-sftp/condition-12.png)   
8. Girin *True* içinde **üzerine yaz?* ayıklanan dosyalarla aynı adı varsa, var olan dosyaların üzerine olduğunu belirtmek için denetim.      
   ![SFTP eylem koşulu görüntüsü 13](./media/connectors-create-api-sftp/condition-13.png)   
9. İş akışınıza Değişiklikleri Kaydet  

