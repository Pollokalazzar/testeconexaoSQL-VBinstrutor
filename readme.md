 
git init 

formulario_instrutor

 Private Sub frmInstrutor_Load(sender As System.Object, e As System.EventArgs) Handles MyBase.Load
        
        preencher_grid()
    End Sub
    Sub preencher_grid()
        gridInstrutor.Rows.Clear()
        'limpar a variável sql 
        sql = ""
        sql = "select * from instrutor order by nome"
        'dim = declaração de uma variável local
        Dim cmd As New SqlClient.SqlCommand(sql, conexao)
        Dim dr As SqlClient.SqlDataReader
      
          Try  
            If conexao.State = ConnectionState.Open Then
                conexao.Close()
            End If
            conexao.Open()
            dr = cmd.ExecuteReader
            Do While dr.Read = True 
                gridInstrutor.Rows.Add(dr("codigo"), dr("nome"), dr("apelido"),
                                        dr("fone"), dr("celular")),
            Catch ex As Exception 
            MessageBox.Show(ex.ToString)
        Finally 
            conexao.Close()
        End Try
    End Sub


    Private Sub btnFechar_Click(sender As System.Object, e As System.EventArgs) Handles btnFechar.Click
        Me.Close()
    End Sub

    Private Sub btnLimpar_Click(sender As System.Object, e As System.EventArgs) Handles btnLimpar.Click
        limpar() 
    End Sub
    
    Sub limpar()
        txtCodigo.Text = ""
        TxtNome.Text = ""
        txtApelido.Text = ""
        txtCelular.Text = ""
        txtTelefone.Text = ""
    End Sub


    Private Sub btnNovo_Click(sender As System.Object, e As System.EventArgs) Handles btnNovo.Click
        salvar = "sim" 
        sql = ""
        sql = "select max(codigo) as 'Maior' from instrutor"
        (aqui declarando as variáveis locais)
        Dim cmd As New SqlClient.SqlCommand(sql, conexao)
        Dim dr As SqlClient.SqlDataReader
        ---------------------------------------------->('neste momento a conexão está aberta executando o comando sql')
        Try
            If conexao.State = ConnectionState.Open Then
                conexao.Close()
            End If
            conexao.Open() 
            dr = cmd.ExecuteReader 
                If dr.Read Then
                txtCodigo.Text = dr("Maior") + 1
            Else
                txtCodigo.Text = 1
            End If

             MessageBox.Show(ex.ToString)
         Finally
            conexao.Close()
        End Try
    End Sub

    Private Sub btnSalvar_Click(sender As System.Object, e As System.EventArgs) Handles btnSalvar.Click
        If Len(TxtNome.Text) = 0 Then
            MessageBox.Show("Favor informar o nome!", "Atenção")
            TxtNome.Focus()
            Exit Sub
        End If
      ------------------------------------------------------->('o apelido também não pode ser nulo)
        If Len(txtApelido.Text) = 0 Then
            MessageBox.Show("Favor informar o apelido!", "Atenção")
            txtApelido.Focus()
            Exit Sub
        End If
        If salvar = "sim" Then
  ---------------------------------------------->('vamos construir o comando sql para inserir o registro limpando a variável sql)
            sql = ""
            sql = "insert into instrutor (codigo, nome, apelido, "
            sql = sql & "fone, celular) values (" & txtCodigo.Text & ",'"
            sql = sql & TxtNome.Text & "','"
            sql = sql & txtApelido.Text & "','"
            sql = sql & txtTelefone.Text & "','"
            sql = sql & txtCelular.Text & "')"
        End If
       ---------------------------------------------------> ('declarando as variáveis locais )
        Dim cmd As New SqlClient.SqlCommand(sql, conexao)

        Try
            If conexao.State = ConnectionState.Open Then
                conexao.Close()
            End If
            conexao.Open() 'neste momento a conexão está aberta
            'Executando o comando sql
            cmd.ExecuteNonQuery()
            limpar() 'neste momento estamos chamando as subrotina limpar'
            preencher_grid()
            ------------------------->('tecla da função f9= break point= ponto de parada escolhid e com isso nós conseguimos 'enxergar(inspecionar) os valores das variaveis para resolver os erros; 'para retira o break point basta precionar novamente o f9 na mesma linha')
            
        Catch ex As Exception
            MessageBox.Show(ex.ToString)
        Finally
            conexao.Close()
        End Try

    End Sub

    Private Sub gridInstrutor_CellClick(sender As Object, e As System.Windows.Forms.DataGridViewCellEventArgs) Handles gridInstrutor.CellClick
        
        If gridInstrutor.CurrentRow.Cells(0).Value = 0 Then
            MessageBox.Show(" Favor digitar uma célula que tenha valor!", "Atenção!",
            MessageBoxButtons.OK,
            MessageBoxIcon.Error)
            limpar()

            Exit Sub
        End If
        txtCodigo.Text = gridInstrutor.CurrentRow.Cells(0).Value
        TxtNome.Text = gridInstrutor.CurrentRow.Cells(1).Value
        txtApelido.Text = gridInstrutor.CurrentRow.Cells(2).Value
        txtTelefone.Text = gridInstrutor.CurrentRow.Cells(3).Value
        txtCelular.Text = gridInstrutor.CurrentRow.Cells(4).Value

    End Sub


   
    Private Sub btnExcluir_Click(sender As System.Object, e As System.EventArgs) Handles btnExcluir.Click
        
        If Val(txtCodigo.Text) = 0 Then
            MessageBox.Show(" Favor clicar no registro a ser excluido!", " ATENÇÃO!",
            MessageBoxButtons.OK,
            MessageBoxIcon.Warning)
            Exit Sub
        End If
        
        If MessageBox.Show("Tem certeza que deseja EXCLUIR? ", "CUIDADO!",
                           MessageBoxButtons.YesNo,
                           MessageBoxIcon.Question,
                           MessageBoxDefaultButton.Button2) = Windows.Forms.DialogResult.Yes Then

            sql = ""
            sql = "DELETE FROM INSTRUTOR WHERE CODIGO=" & txtCodigo.Text
            Try
                If conexao.State = ConnectionState.Open Then
                    conexao.Close()
                End If
                conexao.Open()
                Dim CMD As New SqlClient.SqlCommand(sql, conexao)
                CMD.ExecuteNonQuery()
                limpar()
                preencher_grid()

            Catch ex As Exception

            End Try

        End If
    End Sub
