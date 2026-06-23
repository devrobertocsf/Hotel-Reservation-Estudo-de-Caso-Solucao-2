# ⚠️ Hotel Reservation - Estudo de Caso: Solução 2 (Validação com Retorno de String)

Este repositório documenta a evolução do sistema de reservas de hotel para a **Solução 2**. Embora represente uma melhora em relação ao modelo anterior (centralizado no `main`), esta abordagem ainda é tecnicamente classificada como **ruim** devido à estratégia adotada para sinalização de falhas de negócio.

## 📈 Evolução em Relação à Solução 1

* **Delegação de Responsabilidade**: As regras de validação de consistência das datas saíram da classe `Program.java` e foram encapsuladas dentro do método `updateDates` na classe `Reservation.java`. A camada de aplicação não dita mais as regras de validação.

## 🧐 Por que a Solução 2 ainda é Ruim? (Code Smells)

### 1. Semântica de Retorno Mista e Artificial

O método `updateDates` acumula uma lógica dupla indesejada no seu retorno:

```java
public String updateDates(Date checkIn, Date checkOut) {
    if (/* erro */) return "Mensagem de erro";
    // ...
    return null; // Indica sucesso
}

```

Atribuir um significado de "sucesso" ao valor `null` e "falha" a um conteúdo textual quebra a semântica de métodos de modificação de estado, que idealmente deveriam ser do tipo `void`.

### 2. Interface de Usuário Dependente de Checagens Manuais

O controlador principal (`Program.java`) continua obrigado a implementar estruturas condicionais defensivas para interceptar e tratar o retorno do método:

```java
String error = reservation.updateDates(checkIn, checkOut);
if (error != null) {
    System.out.println("Error in reservation: " + error);
}

```

Caso a verificação do objeto `error` seja omitida por falha humana, a aplicação prosseguirá o fluxo fingindo consistência nos dados.

### 3. Rigidez no Fluxo com Datas Passadas

A presença da validação estática utilizando o tempo do sistema hospedeiro (`Date now = new Date();`) inviabiliza testes retroativos locais com massa de dados histórica (como o cenário clássico de testes datados em 2019 fornecidos no curso).

## 📂 Estrutura de Arquivos Cadastrados

* `model.entities/Reservation.java`: Concentra os métodos de mutação de estado retornando String/Null como barreira lógica de proteção.
* `application/Program.java`: Camada de apresentação readequada para interpretar os retornos textuais gerados pelo modelo de domínio.

## 📄 Licença

Este projeto está sob a licença MIT.

---

