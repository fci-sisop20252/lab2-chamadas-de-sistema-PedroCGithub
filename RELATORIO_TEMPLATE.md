# Relatório do Laboratório 2 - Chamadas de Sistema

---

## Exercício 1a - Observação printf() vs 1b - write()

### Comandos executados:
```bash
strace -e write ./ex1a_printf
strace -e write ./ex1b_write
```

### Análise

**1. Quantas syscalls write() cada programa gerou?**
- ex1a_printf: 8 syscalls
- ex1b_write: 7 syscalls

**2. Por que há diferença entre printf() e write()?**

```
    O write() escreve direto no arquivo, cada chamada sua vira exatamente uma syscall (sem buffer intermediário). Já o printf(), usa um buffer para realizar a saída, o que causa de um "\n" caso esteja solitario (basicamente como se fosse 2 enters seguidos), causar uma syscall a mais.
```

**3. Qual implementação você acha que é mais eficiente? Por quê?**

```
    Depende do tamanho do arquivo, caso o arquivo esteja sem \n solitarios, o printf() vai ser melhor pois pode agrupar dados, agora para arquivos maiores, o write() sempre vai ter a mesma quantidade de chamadas que o arquivo ter, coisa que o printf() pode causar mais chamadas. 
```

---

## Exercício 2 - Leitura de Arquivo

### Resultados da execução:
- File descriptor: 3
- Bytes lidos: 127

### Comando strace:
```bash
strace -e open,read,close ./ex2_leitura
```

### Análise

**1. Por que o file descriptor não foi 0, 1 ou 2?**

```
    0:

    1:

    2:
```

**2. Como você sabe que o arquivo foi lido completamente?**

```
[Sua análise aqui]
```

---

## Exercício 3 - Contador com Loop

### Resultados (BUFFER_SIZE = 64):
- Linhas: _____ (esperado: 25)
- Caracteres: _____
- Chamadas read(): _____
- Tempo: _____ segundos

### Experimentos com buffer:

| Buffer Size | Chamadas read() | Tempo (s) |
|-------------|-----------------|-----------|
| 16          |                 |           |
| 64          |                 |           |
| 256         |                 |           |
| 1024        |                 |           |

### Análise

**1. Como o tamanho do buffer afeta o número de syscalls?**

```
[Sua análise aqui]
```

**2. Como você detecta o fim do arquivo?**

```
[Sua análise aqui]
```

---

## Exercício 4 - Cópia de Arquivo

### Resultados:
- Bytes copiados: _____
- Operações: _____
- Tempo: _____ segundos
- Throughput: _____ KB/s

### Verificação:
```bash
diff dados/origem.txt dados/destino.txt
```
Resultado: [ ] Idênticos [ ] Diferentes

### Análise

**1. Por que devemos verificar que bytes_escritos == bytes_lidos?**

```
[Sua análise aqui]
```

**2. Que flags são essenciais no open() do destino?**

```
[Sua análise aqui]
```

---

## Análise Geral

### Conceitos Fundamentais

**1. Como as syscalls demonstram a transição usuário → kernel?**

```
[Sua análise aqui]
```

**2. Qual é o seu entendimento sobre a importância dos file descriptors?**

```
[Sua análise aqui]
```

**3. Discorra sobre a relação entre o tamanho do buffer e performance:**

```
[Sua análise aqui]
```

### Comparação de Performance

```bash
# Teste seu programa vs cp do sistema
time ./ex4_copia
time cp dados/origem.txt dados/destino_cp.txt
```

**Qual foi mais rápido?** _____

**Por que você acha que foi mais rápido?**

```
[Sua análise aqui]
```

---

## Entrega

Certifique-se de ter:
- [ ] Todos os códigos com TODOs completados
- [ ] Traces salvos em `traces/`
- [ ] Este relatório preenchido como `RELATORIO.md`

```bash
strace -e write -o traces/ex1a_trace.txt ./ex1a_printf
strace -e write -o traces/ex1b_trace.txt ./ex1b_write
strace -o traces/ex2_trace.txt ./ex2_leitura
strace -c -o traces/ex3_stats.txt ./ex3_contador
strace -o traces/ex4_trace.txt ./ex4_copia
```