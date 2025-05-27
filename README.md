# **Debugando um Formatador de Nome**  

## **Sobre o desafio:**  

Em um sistema de **cadastro online**, os nomes dos usuários precisam ser **formatados corretamente** antes de serem armazenados no banco de dados.  

Para isso, devemos criar um programa que:  
✅ **Receba um nome completo como entrada** (ex: `"joão da silva"`).  
✅ **Transforme cada palavra**, iniciando com **letra maiúscula**, enquanto o restante permanece em **minúsculo**.  

No entanto, o código original contém um **erro lógico**, impedindo que a capitalização funcione corretamente. **Sua missão é depurar e corrigir o código!**  

💡 **Dica:** O **GitHub Copilot** pode ajudar sugerindo correções e melhorias.  

---

## **Entrada**

📌 O programa recebe **uma única string**, representando o **nome completo do usuário**.  
📌 O nome pode conter entre **1 e 5 palavras**.  
📌 As letras podem estar **em caixa baixa, alta ou mista**.  

## **Saída**

O programa deverá imprimir o nome formatado corretamente, garantindo que:  
✅ **Cada palavra inicie com letra maiúscula**.  
✅ **O restante das letras esteja em minúsculo**.  

---

## **Exemplos:**

A tabela abaixo apresenta exemplos de entrada e saída esperadas:

| Entrada | Saída |
|---------|------|
| `joão silva`  | **João Silva**  |
| `MARIA cOSTA` | **Maria Costa** |
| `lUcAs pIRES` | **Lucas Pires** |
| `João da Silva` | **João da Silva** |

---

## **Código antes da correção:**

Este código contém um **erro lógico**, pois **transforma todas as letras em maiúsculas**, sem formatar corretamente cada palavra:

```python
full_name = input()

def format_name(name):
    return name.upper()  # ❌ Problema: Todas as letras ficam maiúsculas!

formatted = format_name(full_name)
print(formatted)
```

### **Problema:**

 O código **não capitaliza corretamente cada palavra**, apenas transforma **todo o texto em maiúsculas**.  

---

## ✅ **Código corrigido e otimizado:**

Agora, o código usa regras avançadas para **formatar corretamente cada palavra**, respeitando conectores como `"da", "de", "dos"`, e validando a entrada do usuário.

```python
import re

# Função para validar a entrada do nome
def validar_nome(nome):
    """Verifica se o nome contém apenas letras e espaços."""
    return bool(re.match(r"^[A-Za-zÀ-ÖØ-öø-ÿ\s]+$", nome))

# Função para formatar corretamente o nome completo
def format_name(name):
    conectores = {"da", "de", "do", "dos", "das", "e"}
    palavras = name.split()
    
    return " ".join(
        palavra.capitalize() if palavra.lower() not in conectores else palavra.lower()
        for palavra in palavras
    )

# Entrada do usuário
full_name = input("Digite seu nome completo: ")

if not validar_nome(full_name):
    print("Erro: O nome deve conter apenas letras e espaços.")
else:
    formatted = format_name(full_name)
    print(formatted)
```

### 🔧 **Correção aplicada:**

✔️ **Corrigimos o erro lógico**, garantindo que cada palavra começa com maiúscula.  
✔️ **Evitamos caracteres inválidos**, impedindo números ou símbolos indesejados.  
✔️ **Respeitamos nomes compostos**, mantendo conectores em minúsculas.  

---

## 🧪 **Testes Automatizados:**

Para garantir que o código funciona corretamente, criamos **testes automatizados** utilizando `pytest`.  

📌 **Crie o arquivo 

`test_format_name.py`** e adicione os testes:

```python
import pytest
from src.main import format_name, validar_nome

def test_formatacao_correta():
    assert format_name("joão silva") == "João Silva"
    assert format_name("MARIA cOSTA") == "Maria Costa"
    assert format_name("lUcAs pIRES") == "Lucas Pires"
    assert format_name("joão da silva") == "João da Silva"

def test_nomes_com_uma_palavra():
    assert format_name("joão") == "João"
    assert format_name("maria") == "Maria"

def test_validacao_entrada():
    assert validar_nome("João123") == False
    assert validar_nome("Maria-Silva") == False
    assert validar_nome("Carlos") == True

pytest.main()
```

Para rodar os testes, basta executar:  
```bash
pytest tests/test_format_name.py
```

Se todos os testes passarem, significa que o código está funcionando corretamente! 

---

## **Criando uma API com Flask:**

Caso queira transformar o **formatador de nomes em um serviço**, podemos criar uma **API Flask** para receber nomes e formatá-los via requisições HTTP.  

```python
from flask import Flask, request, jsonify

app = Flask(__name__)

@app.route("/formatar", methods=["POST"])
def formatar_nome():
    dados = request.json
    nome = dados.get("nome", "")

    if not nome or not nome.replace(" ", "").isalpha():
        return jsonify({"erro": "Nome inválido"}), 400

    return jsonify({"nome_formatado": format_name(nome)})

if __name__ == "__main__":
    app.run(debug=True)
```

🔹 **Agora, o formatador pode ser usado via API em sistemas de cadastro online!**   

---

## **Estrutura do Projeto:**

Para manter tudo organizado, o repositório segue esta estrutura:

```
formatador-nome/
├── src/               # Código fonte
│   ├── main.py        # Arquivo principal
├── tests/             # Testes automatizados
│   ├── test_format_name.py   # Testes do código
├── api/               # Código da API Flask
│   ├── app.py         # Serviço da API
├── README.md          # Documentação do projeto
├── requirements.txt   # Dependências do projeto
├── .gitignore         # Arquivos ignorados pelo Git
```

---

## **Licença**  
Este projeto está sob a licença **MIT**, permitindo que qualquer pessoa possa usá-lo e contribuir. 💡  

---

## **Conclusão:**  
Agora temos um **formatador de nomes poderoso**, que pode ser **usado em cadastros online, aplicações web e até como um serviço via API**! 🚀  

✅ **Validação da entrada**  Evitando caracteres inválidos.  

✅ **Correção de nomes compostos**  Mantendo `"da", "de"` corretamente em minúsculas.  

✅ **Testes automatizados** Assegurando que a lógica funciona.  
✅ **Criação de uma API** 
Permite integração com sistemas web.  

✅ **Estrutura organizada** 
Fácil manutenção e expansão.  

Este programa pode ser integrado a diversos projetos e aplicado em sistemas reais. 
