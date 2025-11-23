using static System.Console ; 
public class UX
{ 
    private readonly Banco _banco; 
    private readonly string _titulo; 
    
    public UX(string titulo, Banco banco) 
    { 
        _titulo = titulo; 
        _banco = banco; 
    } 
    
    public void Executar() 
    { 
        CriarTitulo(_titulo); 
        WriteLine(" [1] Criar Conta"); 
        WriteLine(" [2] Listar Contas"); 
        WriteLine(" [3] Efetuar Saque"); 
        WriteLine(" [4] Efetuar Depósito"); 
        WriteLine(" [5] Aumentar Limite"); 
        WriteLine(" [6] Diminuir Limite"); 
        ForegroundColor = ConsoleColor.Red; 
        WriteLine("\n [9] Sair"); 
        ForegroundColor = ConsoleColor.White; 
        CriarLinha(); 
        ForegroundColor = ConsoleColor.Yellow; 
        Write(" Digite a opção desejada: "); 
        var opcao = ReadLine() ?? ""; 
        ForegroundColor = ConsoleColor.White; 
        
        switch (opcao) 
        { 
            case "1": CriarConta(); break; 
            case "2": MenuListarContas(); break; 
            case "3": MenuSaque(); break; 
            case "4": MenuDeposito(); break; 
            case "5": MenuAumentarLimite(); break; 
            case "6": MenuDiminuirLimite(); break; 
        } 
        
        if (opcao != "9") 
        { 
            Executar(); 
        } 
        
        _banco.SaveContas(); 
    } 
    
    private void MenuSaque() 
    { 
        CriarTitulo(_titulo + " - Efetuar Saque"); 
        Write(" Número da conta: "); 
        int numero = Convert.ToInt32(ReadLine()); 
        
        var conta = _banco.BuscarConta(numero); 
        
        if (conta == null) 
        { 
            CriarRodape("Conta não encontrada!"); 
            return; 
        } 
        
        Write(" Valor do saque: "); 
        decimal valor = Convert.ToDecimal(ReadLine()); 
        
        if (conta.Sacar(valor)) 
            CriarRodape("Saque realizado com sucesso!"); 
        else 
            CriarRodape("Falha ao sacar. Verifique saldo e limite.");
    } 
    
    private void MenuDeposito() 
    { 
        CriarTitulo(_titulo + " - Efetuar Depósito"); 
        Write(" Número da conta: "); 
        int numero = Convert.ToInt32(ReadLine()); 
        
        var conta = _banco.BuscarConta(numero); 
        
        if (conta == null) 
        { 
            CriarRodape("Conta não encontrada!"); 
            return; 
        } 
        
        Write(" Valor do depósito: "); 
        decimal valor = Convert.ToDecimal(ReadLine()); 
        
        conta.Depositar(valor); 
        
        CriarRodape("Depósito realizado com sucesso!"); 
    } 
    
    private void MenuAumentarLimite() 
    { 
        CriarTitulo(_titulo + " - Aumentar Limite"); 
        
        Write(" Número da conta: "); 
        int numero = Convert.ToInt32(ReadLine()); 
        
        var conta = _banco.BuscarConta(numero); 
        if (conta == null) 
        { 
            CriarRodape("Conta não encontrada!"); 
            return; 
        } 
        
        Write(" Valor para aumentar o limite: "); 
        decimal valor = Convert.ToDecimal(ReadLine()); 
        
        conta.AumentarLimite(valor); 
        CriarRodape("Limite aumentado com sucesso!"); 
    }
    
    private void MenuDiminuirLimite()
    { 
        CriarTitulo(_titulo + " - Diminuir Limite"); 
        Write(" Número da conta: "); 
        int numero = Convert.ToInt32(ReadLine()); 
        
        var conta = _banco.BuscarConta(numero); 
        
        if (conta == null) 
        { 
            CriarRodape("Conta não encontrada!"); 
            return; 
        } 
        
        Write(" Valor para diminuir o limite: "); 
        decimal valor = Convert.ToDecimal(ReadLine()); 
        
        if (conta.DiminuirLimite(valor)) 
            CriarRodape("Limite reduzido com sucesso!"); 
        else 
            CriarRodape("Não é possível reduzir o limite para um valor negativo."); 
    }
