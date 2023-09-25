# Analise de Estrutura do Código


    import java.sql.Connection;
    import java.sql.DriverManager;
    import java.sql.ResultSet;
    import java.sql.Statement;


        public class User {
        public Connection conectarDB(){
        Connection conn = null;
        try{
        Class.forName("com.mysql.Driver.Manager").newInstance();
        String url = "jdbc:mysql://127.0.0.1/test?user=lopes&password=123";
        conn = DriverManager.getConnection(url);
        }
    
            catch (Exception e){}
            return conn;
        }
        public String nome = "";
        public boolean result = false;
        public boolean VerificarUsuario(String login, String senha){
            String sql = "";
            Connection conn = conectarDB();
    
            sql += "select nome from usuarios ";
            sql += "where login = " + "'" + login + "'";
            sql += " and senha = " + "'" + senha + "';";
            try{
                Statement st = conn.createStatement();
                ResultSet rs = st.executeQuery(sql);
                if(rs.next()){
                    result  = true;
                    nome = rs.getString("nome");
                }
    
            }
            catch (Exception e){
    
            }
            return  result;
        }
    
    }     

## Primieiro Apontamento

<p>
    O Código em si está contido em apenas uma calsse que realiza
    diversas operações como criar a conexão com o Banco de dados
    e realizando e a busca no banco de dados.
</p>
<p>
    Isso está a quebrar alguns padrões e boas práticas, uma classe
    deve ser responsavel apenas por uma unica função, ou seja 
    este código poderia ser dividido em dois, um com resposabilidade
    de se conectar com o banco de dados e outro por realizar operações no banco dedados.
</p>

## Segundo Apontamento
<p>
    Ainda seguindo padrões de projeto, normalmente se cria ‘interfaces’ para que
    classes dependentes possam implementar os seus metodos, seguindo os 'Contratos'
    dito pelas ‘interfaces’, assim criando um padrão que todas devem seguir.
</p>
<p>
    Seguindo essas regras é possivel criar, por exemplo, mais de uma classe
    conectora pro Banco de Dados, conectando a Bancos diferentes, mas como todos têm as suas arquiteturas iguais
    ao ‘interface’, pode-se utilizar a ‘interface’ na referência de metodo, por exemplo, com aplicação decidindo 
    qual classe deve ser aplicada.
</p>

## Terceiro Apontamento 
<p>
    Como vistono código, ele tem a possibilidade de ter algum
    valor nulo passando, por exemplo:
</p>

        Connection conn = null;
        try{
        Class.forName("com.mysql.Driver.Manager").newInstance();
        String url = "jdbc:mysql://127.0.0.1/test?user=lopes&password=123";
        conn = DriverManager.getConnection(url);
        }
    
            catch (Exception e){}
            return conn;

<p>
    Como Visto, ele inicia uma variavel conn para conectar com o banco de dados,
    atribuindo o valor null a ela, em seguida começa um try-catch para realizar a conexão,
    caso a conexão tenha algum problema, a Exception não realizara nenhuma ação, nesse caso
    não tratando a exceção ocorrida, se o programa der continuidade o return acabará enviando 
    uma variavel conn de valor nulo.
</p>

## Quarto Apontamento
<p>
    De acordo com as boas práticas de programação, vê-se a utilidade de comentar 
    um código para explicar algumas partes onde o código em si não é totalmente 
    legivel, alguns exemplos:
</p>

    Class.forName("com.mysql.Driver.Manager").newInstance();

<p>
    Está parte do código não é muito legivel para alguem que não 
    está acostumado ou tenha pouca vivência em abiente java, para 
    aqueles que possam usar este código, seja para melhorias, manuntenção 
    ou para transformação de ferramentas, consiga entender o que essa linha 
    significa para o programa e assim conseguir realizar o seu trabalho de forma
    mais eficiênte.
</p>

## Quinto Apontamento
<p>
    Olhando para a parte de conexão do código, é possivel ver que a conexão 
    com o banco de dados em nenhum momento é fechada
</p>

    public Connection conectarDB(){
        Connection conn = null;
        try{
        Class.forName("com.mysql.Driver.Manager").newInstance();
        String url = "jdbc:mysql://127.0.0.1/test?user=lopes&password=123";
        conn = DriverManager.getConnection(url);
        }
    
            catch (Exception e){}
            return conn;
        }

<p>
    Isso significa que sempre que uma conexão é estabelecida,
    não é fechada com o banco de dados ainda aberto, assim de certa forma  
    o banco de dados pode ficar vunerável a alguns tipo de ataques.
</p>

## Sexto Apontamento
<p>
    Seguindo a legebilidade do código, vendo de certa pesperctiva, 
    é dificil dizer o que essa classe deveia fazer, ela é uma abstração
    de usuario para o código, se era para uma calsse que cria a conexão com o banco
    ou se era a classe resposavel por realizar as operações de usuario com o banco.
</p>



