# AplikasiPasien
Imports System.Data.Odbc
'koneksi data
Public Class Form1
    Dim Conn As OdbcConnection
    Dim Cmd As OdbcCommand
    Dim Ds As DataSet
    Dim Da As OdbcDataAdapter
    Dim Ra As OdbcDataReader
    Dim MyDB As String
    Dim Rd As OdbcDataReader

    Sub koneksi()
        MyDB = "Driver={MySQL ODBC 3.51 Driver};Database=db_pasien;Server=localhost;uid=root"
        Conn = New OdbcConnection(MyDB)
        If Conn.State = ConnectionState.Closed Then Conn.Open()
    End Sub
    'falidasi data
    Private Sub Form1_Load(sender As Object, e As EventArgs) Handles MyBase.Load
        Call KondisiAwal()
    End Sub
    Sub KondisiAwal()
        TextBox1.Text = ""
        TextBox2.Text = ""
        TextBox3.Text = ""
        TextBox4.Text = ""
        TextBox5.Text = ""
        TextBox6.Text = ""
        Button1.Text = "SIMPAN"
        Button2.Text = "UBAH"
        Button3.Text = "HAPUS"
        Button4.Text = "TUTUP"
        Call koneksi()
        Da = New OdbcDataAdapter("Select * From tbl_pasien", Conn)
        Ds = New DataSet
        Da.Fill(Ds, "tbl_pasien")
        DataGridView1.DataSource = Ds.Tables("tbl_pasien")
    End Sub
    'imput data
    Private Sub Button1_Click(sender As Object, e As EventArgs) Handles Button1.Click
        If TextBox1.Text = "" Or TextBox2.Text = "" Or TextBox3.Text = "" Or TextBox4.Text = "" Or TextBox5.Text = "" Or TextBox6.Text = "" Then
            MsgBox("Pastikan Semuah kolom terisi")
        Else
            Call koneksi()
            Dim ImputData As String = "Insert into tbl_pasien Values ('" & TextBox1.Text & "','" & TextBox2.Text & "','" & TextBox3.Text & "','" & TextBox4.Text & "','" & TextBox5.Text & "','" & TextBox6.Text & "')"
            Cmd = New OdbcCommand(ImputData, Conn)
            Cmd.ExecuteNonQuery()
            MsgBox("Imput Data Sukses Bro n sis")
            Call KondisiAwal()
        End If
    End Sub
    'fom ubah data
    Private Sub Button2_Click(sender As Object, e As EventArgs) Handles Button2.Click
        If TextBox1.Text = "" Or TextBox2.Text = "" Or TextBox3.Text = "" Or TextBox4.Text = "" Or TextBox5.Text = "" Or TextBox6.Text = "" Then
            MsgBox("Pastikan Semuah kolom terisi")
        Else
            Call koneksi()
            Dim EditData As String = "Update tbl_pasien set nama='" & TextBox2.Text & "',alamat='" & TextBox3.Text & "',notlp='" & TextBox4.Text & "',keterangan='" & TextBox5.Text & "',hasil_periksa='" & TextBox6.Text & "' where norm='" & TextBox1.Text & "'"
            Cmd = New OdbcCommand(EditData, Conn)
            Cmd.ExecuteNonQuery()
            MsgBox("Rubah Data Sukses Bro n sis")
            Call KondisiAwal()
        End If
    End Sub
    'pecarian data
    Private Sub TextBox1_KeyPress(sender As Object, e As KeyPressEventArgs) Handles TextBox1.KeyPress
        If e.KeyChar = Chr(13) Then
            Call koneksi()
            Cmd = New OdbcCommand("Select * From tbl_pasien where norm ='" & TextBox1.Text & "'", Conn)
            Rd = Cmd.ExecuteReader
            Rd.Read()
            If Rd.HasRows Then
                TextBox2.Text = Rd.Item("nama")
                TextBox3.Text = Rd.Item("alamat")
                TextBox4.Text = Rd.Item("notlp")
                TextBox5.Text = Rd.Item("keterangan")
                TextBox6.Text = Rd.Item("hasil_periksa")
            Else
                MsgBox("Ora ene datane")
            End If

        End If
    End Sub
    'hapus data
    Private Sub Button3_Click(sender As Object, e As EventArgs) Handles Button3.Click
        If TextBox1.Text = "" Or TextBox2.Text = "" Or TextBox3.Text = "" Or TextBox4.Text = "" Or TextBox5.Text = "" Or TextBox6.Text = "" Then
            MsgBox("Pastikan Semuah kolom terhapus")
        Else
            Call koneksi()
            Dim HapusData As String = "delete From tbl_pasien where norm='" & TextBox1.Text & "'"
            Cmd = New OdbcCommand(HapusData, Conn)
            Cmd.ExecuteNonQuery()
            MsgBox("Hapus Data Sukses Bro n sis")
            Call KondisiAwal()
        End If
    End Sub
    'keluar dari aplikasi
    Private Sub Button4_Click(sender As Object, e As EventArgs) Handles Button4.Click
        End
    End Sub

