# Executando uma LLM localmente com Ollama

## Por que executar um modelo LLM localmente?
Embora seja muito comum e prático interagir com um modelo LLM através de chatbots online, nos quais todo o processamento é feito em datacenters, há vantagens em usar o próprio hardware. Algumas aplicações exigem confidencialidade das informações, seja por questões de negócio ou requisitos legais, e nesse caso um modelo online pode comprometer o sigilo. Também pode ser interessante executar um modelo proprietário ou até um modelo de código aberto treinado para casos de uso específicos. Por último, há a vantagem da garantia de disponibilidade e de desempenho, pois ao executar localmente elimina-se a dependência de conexão com a rede e os possíveis gargalos de vazão e latência dessa conexão.

## Requisitos

- Sistema operacional: Linux, Microsoft, macOS
- Memória: pelo menos 8 GB de RAM para modelos de 7B ou 8B de parâmetros
- Armazenamento:
    - SSD (preferencialmente)
    - 33 MB de disco para a instalação do Ollama (Linux)
    - De 1 GB a 404 GB de disco para modelos de 1.5B a 671B de parâmetros (Deepseek)

## Instalação

### Sistemas Windows e macOS
É possível instalar através de um executável disponível no site do projeto Ollama: [https://ollama.com/](https://ollama.com/)

### Sistemas Linux
A maneira mais simples é baixar e executar o script de instalação fornecido através do comando:
`curl -fsSL https://ollama.com/install.sh | sh`

Vale destacar que é necessário ter a ferramenta `curl` instalada.

## Utilização
O programa é executado através do terminal.

### Baixando um modelo
Para baixar o modelo, basta usar o seguinte comando: `ollama pull <nome_do_modelo>`
Os modelos disponíveis podem ser encontrados em: [https://ollama.com/library](https://ollama.com/library)

### Interagindo através do terminal
Um modelo pode ser invocado através do comando: `ollama run <nome_do_modelo>`
Nesse modo, a interação é feita através de prompts no terminal, sendo possível enviar e receber texto multilinha.

### Interagindo através de API REST
É possível executar um servidor local usando o comando `ollama serve`.
Nesse modo, o Ollama executa um servidor local hospedado em `http://localhost:11434`, sendo possível enviar prompts direcionados para uma LLM específica.

## Integração com Python
É fornecida uma API REST para integração com Python através da biblioteca `ollama`, que pode ser instalada com `pip install ollama`. Nesse caso, é necessário que o Ollama e o modelo LLM já estejam instalados.

Exemplo em código: 

```python
import ollama

# Exemplo de interação com um modelo qwen3 com 8 Bilhões de parâmetrso
response = ollama.chat(model='qwen3:8B', messages=[
    {'role': 'user', 'content': 'O que é Inteligência Artificial?'}
])
print(response['message']['content'])


# Resposta com Streaming, usando o modelo Gemma3 com 4 Bilhões de parâmetros
for part in ollama.chat(model='gemma3:4B', messages=[
    {'role': 'user', 'content': 'Conte uma piada.'}
], stream=True):
    print(part['message']['content'], end='', flush=True)
```
