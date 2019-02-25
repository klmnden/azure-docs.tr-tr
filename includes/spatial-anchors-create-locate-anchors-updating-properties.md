## <a name="updating-properties-on-an-existing-cloud-spatial-anchor"></a>Bulut için var olan bir uzamsal bağlantı özellikleri güncelleştiriliyor

Bir bağlantı özelliklerini güncelleştirmek için UpdateAnchorPropertiesAsync yöntemi kullanın. Aynı anda aynı bağlantı özelliklerini güncelleştirmek iki veya daha fazla cihaz denerseniz bir iyimser eşzamanlılık modeli kullanırız. İlk Yazımdan kazanacak anlamına gelir.  Diğer tüm yazma işlemlerini bir "Eşzamanlılık" hata iletisiyle karşılaşırsınız: tekrar denemeden önce özelliklerini yenilenmesini gerekli olur.
