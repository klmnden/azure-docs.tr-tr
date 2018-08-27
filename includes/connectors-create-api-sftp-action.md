Bir tetikleyici ekledikten sonra bir şey yapmak için kendi zamanı Tetikleyici tarafından oluşturulan verilerle ilginç. Eklemek için bu adımları bir **SFTP - extract klasör** eylem. Bu eylem, tanımlanan koşullar karşılandığında bir dosyanın içeriğini ayıklayacaksınız. 

Bunu yapılandırmak için eylem, aşağıdaki bilgileri sağlamanız gerekir. Bazı yeni dosya için özellikler için giriş olarak Tetikleyici tarafından oluşturulan kullanımı kolay veri olduğunu fark edeceksiniz:

| SFTP - extract klasör özelliği | Açıklama |
| --- | --- |
| Kaynak arşiv dosya yolu |Bu dosyanın yoludur. SFTP sunucusuna dosya yolunu bulmak için Gözat veya belirteçlerin daha önceki bir eylem seçin. |
| Hedef klasör yolu |Ayıklanan dosyaları yerleştirileceği yolu budur. Önceki eylem hedef yolu olarak belirteçlerden birini seçin veya SFTP sunucusuna göz atın ve yolu seçin. |
| Üzerine Yazılsın mı? |Ayıklanan dosyasıyla aynı ada sahip bir dosya veya var olan dosyanın üzerine yazılsın, hedef klasör yoluna bulunduğunu gösterir. |

Önceden tanımlanmış bir koşulu değerlendirilirse dosyalarını ayıklamak için eylemi ekleyerek başlayalım *True*. 

1. Seçin **Eylem Ekle**.        
   ![SFTP eylem koşulu görüntüsü 6](./media/connectors-create-api-sftp/condition-6.png)   
2. Seçin **SFTP - Extract klasör** eylemi      
   ![SFTP eylem koşulu görüntüsü 7](./media/connectors-create-api-sftp/condition-7.png)   
3. Seçin **kaynak Arşiv dosya yolu**              
   ![SFTP eylem koşulu görüntü 9](./media/connectors-create-api-sftp/condition-9.png)   
4. Seçin **dosya yolu** belirteci. Bu kaynak Arşiv dosya yolu tetikleyici bulunan dosya dosya yolunu kullanacağını belirtir.           
   ![SFTP eylem koşulu görüntüsü 10](./media/connectors-create-api-sftp/condition-10.png)   
5. Seçin **hedef klasör yolu**           
   ![SFTP eylem koşulu görüntüsü 11](./media/connectors-create-api-sftp/condition-11.png)   
6. Seçin **dosya yolu** belirteci. Bu tetikleyici bulunamadı. dosyanın dosya yolu için ayıklanan dosyaları hedef yolu olarak kullanılacağını gösterir.   
7. Girin *\ExtractedFile* içinde **hedef klasör yolu** denetimi. Hedef klasör yolu denetimi dosya yolu belirteç hemen sonra bunu yapabilirsiniz.         
   ![SFTP eylem koşulu görüntüsü 12](./media/connectors-create-api-sftp/condition-12.png)   
8. Girin *True* içinde **üzerine yaz?* ayıklanan dosyaları aynı adı varsa mevcut dosyaların üzerine yazılması gerektiğini belirtmek için denetimi.      
   ![SFTP eylem koşulu görüntüsü 13](./media/connectors-create-api-sftp/condition-13.png)   
9. Değişiklikleri akışınıza kaydetmek  

