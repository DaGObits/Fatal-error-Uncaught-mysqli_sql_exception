<?php
$servername = "localhost";
$username = "root";
$password = "";
$dbname = "cadastro";

//criar conexão

$conn = new mysqli($servername, $username, $password, $dbname);

//verifica a conexao

if ($conn->connect_error) {
    die("Falha na conexão: " . $conn->connect_error);
}
//verificação de metodo

if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $nome = $_POST['nome'];
    $email = $_POST['email'];
    $senha = $_POST['senha'];

    //verificação se nao ta vazio os campos

    if (empty($nome) || empty($email) || empty($senha)) {
        die("Todos os campos são obrigatórios.");
    }

    //criptografia senha

    $hashed_senha = password_hash($senha, PASSWORD_BCRYPT);

    //inserir os dados 
    
    $stmt = $conn->prepare("INSERT INTO usuarios (nome, email, senha) VALUES (?, ?, ?)");
    $stmt->bind_param("sss", $nome, $email, $hashed_senha);

//conexao foi feita de forma correta 

    if ($stmt->execute()) {
        echo "Novo registro criado com sucesso";
    } else {
        echo "Erro: " . $stmt->error;
    }

    $stmt->close();
}

$conn->close();
?>
