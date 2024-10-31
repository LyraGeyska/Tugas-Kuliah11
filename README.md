# Membuat Fitur Upload Sederhana Dengan Java Swing
Button Upload ini nantinya akan digunakan untuk menambahkan data dari file .csv yang kita upload.

## Langkah - Langkah :
### 1. Tambah Button baru pada design anda, beri nama *UPLOAD*, 
Buatlah button baru pada Design anda, beri nama *Upload* dan ubah variable nya sesuai selera anda.

### 2. Tambahkan Source Code dibawah ini :
Double klik pada button *Upload* yang sudah anda buat, dan masukkan Source Code dibawah ini :
<pre>
  private void btnUploudActionPerformed(java.awt.event.ActionEvent evt) {                                          
        // TODO add your handling code here:
        JFileChooser jfc = new JFileChooser(FileSystemView.getFileSystemView().getHomeDirectory());
        int returnValue = jfc.showOpenDialog(null);
        if (returnValue == JFileChooser.APPROVE_OPTION) {
            File filePilihan = jfc.getSelectedFile();
            System.out.println("yang dipilih : " + filePilihan.getAbsolutePath());
            try {
                BufferedReader br = new BufferedReader(new FileReader(filePilihan));
                String baris = new String("");
                String pemisah = ";";
                while ((baris = br.readLine()) != null) //returns a Boolean value
                {
</pre>
Pada Source Code berikut merupakan nilai Array untuk menangkap data pada tiap baris pada file.csv nantinya, sesuaikan dengan setiap nama kolom dalam tabel database anda.
<pre>
                    String[] dataMhs = baris.split(pemisah);
                    String kodeMK = dataMhs[0];
                    String sks = dataMhs[1];
                    String namaMK = dataMhs[2];
                    String semesterAjar = dataMhs[3];
                    if (!kodeMK.isEmpty() && !sks.isEmpty() && !namaMK.isEmpty() && !semesterAjar.isEmpty()) {
</pre>
<pre>
                        try {
                            Class.forName(driver);
                            conn = DriverManager.getConnection(koneksi, user, password);
                            conn.setAutoCommit(false);

                            String sql = "INSERT INTO matakuliah VALUES(?,?,?,?)";
                            pstmt = conn.prepareStatement(sql);

                            pstmt.setString(1, kodeMK);
                            pstmt.setLong(2, Long.parseLong(sks));
                            pstmt.setString(3, namaMK);
                            pstmt.setLong(4, Long.parseLong(semesterAjar));

                            pstmt.executeUpdate();
                            conn.commit();
                            pstmt.close();
                            conn.close();
                            bersih();
                            
                            tampil();
                        } catch (ClassNotFoundException | SQLException ex) {
                            JOptionPane.showMessageDialog(null, "Terjadi Kesalahan Saat Pengisian Data");
                        }
                    }
                }
            } catch (FileNotFoundException ex) {
                Logger.getLogger(entitasMatakuliah.class.getName()).log(Level.SEVERE, null, ex);
            } catch (IOException ex) {
                Logger.getLogger(entitasMatakuliah.class.getName()).log(Level.SEVERE, null, ex);
            }
        }
    }     
</pre>

### 3. Membuat file.csv (fake) pada Ms.Excel
File ini nantinya yang akan kita upload pada aplikasi kita, file yang di upload harus memiliki extention .csv dan setiap kolomnya harus sesuai dengan jumlah kolom pada tabel yang ada di database anda.
![image](https://github.com/user-attachments/assets/36d63cd2-2bd5-4887-9f13-37ae66126121)
Simpan dengan Type CSV (MS DOS)
![image](https://github.com/user-attachments/assets/9e0744fa-0b7f-446d-8fd4-2cddc419ef3d)

Anda bisa mencoba menggunakan data berikut :
https://github.com/LyraGeyska/Tugas-Kuliah11/blob/main/dataMatakuliah.csv

## Cara Pengaplikasiannya
### 1. Jalankan aplikasi anda, kemudian coba button *Upload* yang sudah anda buat sebelumnya. 
![image](https://github.com/user-attachments/assets/2eaa28ae-f1fb-4bc5-b591-727e549159a0)
Ketika buuton *Upload* dibuka akan muncul pop-up seperti gambar diatas.

### 2. Masukkan File.csv yang sudah anda simpan.
Cari lokasi dimana file.csv anda simpan, kemudian Open file anda.
![image](https://github.com/user-attachments/assets/ea966a6e-eaa4-4f17-b38c-bbb0c3d77994)

### 3. Data berhasil ditambahkan
Sebelum :
![image](https://github.com/user-attachments/assets/8441f788-e8dd-4509-b934-aeb068a13627)

Setelah :
![image](https://github.com/user-attachments/assets/42574f0a-e45b-42af-af26-c33afc16dac1)

# TERIMA KASIH




