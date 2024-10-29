# Menambahkan Button Upload
Button Upload ini nantinya akan digunakan untuk menambahkan data dari file .csv yang kita upload.

## Langkah - Langkah
### 1. Tambah Button baru pada design anda, beri nama *UPLOAD*, 
Menambahkan Button untuk Upload File CSV

### 2. Tambahkan Source Code dibawah ini :
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
                    String[] dataMhs = baris.split(pemisah);
                    String kodeMK = dataMhs[0];
                    String sks = dataMhs[1];
                    String namaMK = dataMhs[2];
                    String semesterAjar = dataMhs[3];
                    if (!kodeMK.isEmpty() && !sks.isEmpty() && !namaMK.isEmpty() && !semesterAjar.isEmpty()) {
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
