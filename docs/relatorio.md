# Relatório — Utilização de GitHub Secrets

## 1. Objetivo

O objetivo desta atividade foi compreender como utilizar o GitHub Secrets para armazenar informações sensíveis fora do código-fonte e disponibilizá-las de forma segura para uma aplicação executada por meio do GitHub Actions.

## 2. Problema encontrado

Inicialmente, o arquivo `.github/workflows/main.yml` continha a variável `MENSAGEM_SECRETA` com seu valor definido diretamente no arquivo YAML.

Essa prática é inadequada porque o arquivo da pipeline fica armazenado no repositório e seu conteúdo pode ser visualizado por pessoas com acesso ao código.

### 2.1. Onde o segredo estava armazenado?

O valor estava definido diretamente no arquivo `.github/workflows/main.yml`, dentro da variável de ambiente `MENSAGEM_SECRETA`.

### 2.2. Quem poderia visualizar essa informação?

Qualquer pessoa com acesso de leitura ao repositório poderia visualizar o valor registrado no arquivo da pipeline.

### 2.3. Por que essa prática não é recomendada?

Porque informações sensíveis não devem ser armazenadas diretamente no código-fonte ou em arquivos versionados. Isso aumenta o risco de exposição acidental e dificulta o controle de acesso às credenciais.

### 2.4. O que poderia acontecer se fosse uma senha ou um token real?

Uma pessoa não autorizada poderia utilizar a credencial para acessar sistemas, serviços ou bancos de dados, dependendo das permissões concedidas. Isso poderia resultar em vazamento de informações, alterações indevidas e outros incidentes de segurança.

## 3. Configuração do GitHub Secret

Foi definido o seguinte nome para o Secret:

`MENSAGEM_SECRETA`

O valor foi cadastrado nas configurações do repositório, em **Settings → Secrets and variables → Actions → New repository secret**.

O conteúdo do Secret foi mantido fora dos arquivos versionados do projeto.

## 4. Alteração realizada na pipeline

No arquivo `.github/workflows/main.yml`, o valor literal da variável de ambiente foi substituído pela referência ao GitHub Secret:

```yaml
- name: Executar aplicação
  env:
    MENSAGEM_SECRETA: ${{ secrets.MENSAGEM_SECRETA }}
  run: |
    cd app
    node server.js
```

Dessa forma, o workflow obtém o valor cadastrado no GitHub e o disponibiliza para a aplicação por meio de uma variável de ambiente.

## 5. Como o Secret chega até a aplicação?

O fluxo de funcionamento é o seguinte:

1. O valor é cadastrado nas configurações do repositório GitHub.
2. O GitHub Actions executa a pipeline após um push na branch `main`.
3. O workflow acessa o valor por meio de `${{ secrets.MENSAGEM_SECRETA }}`.
4. O valor é disponibilizado como variável de ambiente para o processo Node.js.
5. A aplicação acessa a informação usando `process.env.MENSAGEM_SECRETA`.

O valor não precisa ser escrito diretamente no código-fonte nem no arquivo YAML.

## 6. Validação da execução

A aplicação `app/server.js` utiliza a variável de ambiente `MENSAGEM_SECRETA` e verifica se ela foi configurada.

Quando o valor está disponível, a aplicação registra mensagens de confirmação e a quantidade de caracteres, sem imprimir o conteúdo da mensagem nos logs.

**Resultado esperado da execução:**

```text
Secret recebido pela aplicação com sucesso.
Quantidade de caracteres: [quantidade correspondente à mensagem]
```

A validação final deve ser realizada após o envio das alterações ao GitHub, verificando se a execução da pipeline foi concluída com sucesso.

**Evidência da execução:** inserir uma captura de tela da execução bem-sucedida do workflow na aba Actions do repositório.

## 7. Respostas às questões propostas

### 7.1. Qual é a diferença entre armazenar o valor diretamente no `pipeline.yml` e armazená-lo no GitHub Secrets?

Quando o valor é definido diretamente no `pipeline.yml`, ele fica registrado no arquivo versionado e pode ser visualizado por pessoas com acesso ao repositório. Quando é armazenado no GitHub Secrets, o valor fica separado do código e pode ser disponibilizado ao workflow por meio de uma referência, sem precisar ser escrito no arquivo.

### 7.2. Onde o valor do Secret fica configurado?

O valor fica cadastrado nas configurações do repositório GitHub, em **Settings → Secrets and variables → Actions**, na seção de Secrets do repositório.

### 7.3. O valor do Secret deve ser colocado no código da aplicação?

Não. O código deve acessar a informação por meio de uma variável de ambiente, sem precisar conhecer ou armazenar diretamente o valor sensível.

### 7.4. Qual é a função de `process.env.MENSAGEM_SECRETA`?

Essa expressão permite que a aplicação Node.js leia o valor da variável de ambiente chamada `MENSAGEM_SECRETA`. Dessa forma, a aplicação consegue utilizar a informação fornecida pelo ambiente de execução sem manter o valor diretamente no código-fonte.

### 7.5. Por que utilizar Secrets é importante em uma pipeline de CI/CD?

Porque permite separar informações sensíveis dos arquivos versionados, reduzindo o risco de exposição acidental de senhas, tokens, chaves e outras credenciais. Também facilita o gerenciamento desses valores nas configurações do repositório.

### 7.6. Por que seria inadequado colocar uma senha real de banco de dados diretamente no arquivo `.yml`?

Porque o arquivo pode ser armazenado no histórico do Git e ficar acessível a pessoas com permissão para consultar o repositório. Se a senha for exposta, pessoas não autorizadas poderão tentar acessar o banco de dados. Utilizar GitHub Secrets reduz esse risco, mantendo a credencial separada do código-fonte.

## 8. Conclusão

A atividade demonstrou como utilizar o GitHub Secrets para separar informações sensíveis do código-fonte e disponibilizá-las à aplicação por meio do GitHub Actions e de variáveis de ambiente.

Essa prática contribui para a segurança das pipelines de CI/CD e pode ser aplicada ao gerenciamento de credenciais utilizadas por aplicações e serviços.
# Relatório — Utilização de GitHub Secrets

## 1. Objetivo

O objetivo desta atividade foi compreender como utilizar o GitHub Secrets para armazenar informações sensíveis fora do código-fonte e disponibilizá-las de forma segura para uma aplicação executada por meio do GitHub Actions.

## 2. Problema encontrado

Inicialmente, o arquivo `.github/workflows/main.yml` continha a variável `MENSAGEM_SECRETA` com seu valor definido diretamente no arquivo YAML.

Essa prática é inadequada porque o arquivo da pipeline fica armazenado no repositório e seu conteúdo pode ser visualizado por pessoas com acesso ao código.

### 2.1. Onde o segredo estava armazenado?

O valor estava definido diretamente no arquivo `.github/workflows/main.yml`, dentro da variável de ambiente `MENSAGEM_SECRETA`.

### 2.2. Quem poderia visualizar essa informação?

Qualquer pessoa com acesso de leitura ao repositório poderia visualizar o valor registrado no arquivo da pipeline.

### 2.3. Por que essa prática não é recomendada?

Porque informações sensíveis não devem ser armazenadas diretamente no código-fonte ou em arquivos versionados. Isso aumenta o risco de exposição acidental e dificulta o controle de acesso às credenciais.

### 2.4. O que poderia acontecer se fosse uma senha ou um token real?

Uma pessoa não autorizada poderia utilizar a credencial para acessar sistemas, serviços ou bancos de dados, dependendo das permissões concedidas. Isso poderia resultar em vazamento de informações, alterações indevidas e outros incidentes de segurança.

## 3. Configuração do GitHub Secret

Foi definido o seguinte nome para o Secret:

`MENSAGEM_SECRETA`

O valor foi cadastrado nas configurações do repositório, em **Settings → Secrets and variables → Actions → New repository secret**.

O conteúdo do Secret foi mantido fora dos arquivos versionados do projeto.

## 4. Alteração realizada na pipeline

No arquivo `.github/workflows/main.yml`, o valor literal da variável de ambiente foi substituído pela referência ao GitHub Secret:

```yaml
- name: Executar aplicação
  env:
    MENSAGEM_SECRETA: ${{ secrets.MENSAGEM_SECRETA }}
  run: |
    cd app
    node server.js
```

Dessa forma, o workflow obtém o valor cadastrado no GitHub e o disponibiliza para a aplicação por meio de uma variável de ambiente.

## 5. Como o Secret chega até a aplicação?

O fluxo de funcionamento é o seguinte:

1. O valor é cadastrado nas configurações do repositório GitHub.
2. O GitHub Actions executa a pipeline após um push na branch `main`.
3. O workflow acessa o valor por meio de `${{ secrets.MENSAGEM_SECRETA }}`.
4. O valor é disponibilizado como variável de ambiente para o processo Node.js.
5. A aplicação acessa a informação usando `process.env.MENSAGEM_SECRETA`.

O valor não precisa ser escrito diretamente no código-fonte nem no arquivo YAML.

## 6. Validação da execução

A aplicação `app/server.js` utiliza a variável de ambiente `MENSAGEM_SECRETA` e verifica se ela foi configurada.

Quando o valor está disponível, a aplicação registra mensagens de confirmação e a quantidade de caracteres, sem imprimir o conteúdo da mensagem nos logs.

**Resultado esperado da execução:**

```text
Secret recebido pela aplicação com sucesso.
Quantidade de caracteres: [quantidade correspondente à mensagem]
```

A validação final deve ser realizada após o envio das alterações ao GitHub, verificando se a execução da pipeline foi concluída com sucesso.

**Evidência da execução:** inserir uma captura de tela da execução bem-sucedida do workflow na aba Actions do repositório.

## 7. Respostas às questões propostas

### 7.1. Qual é a diferença entre armazenar o valor diretamente no `pipeline.yml` e armazená-lo no GitHub Secrets?

Quando o valor é definido diretamente no `pipeline.yml`, ele fica registrado no arquivo versionado e pode ser visualizado por pessoas com acesso ao repositório. Quando é armazenado no GitHub Secrets, o valor fica separado do código e pode ser disponibilizado ao workflow por meio de uma referência, sem precisar ser escrito no arquivo.

### 7.2. Onde o valor do Secret fica configurado?

O valor fica cadastrado nas configurações do repositório GitHub, em **Settings → Secrets and variables → Actions**, na seção de Secrets do repositório.

### 7.3. O valor do Secret deve ser colocado no código da aplicação?

Não. O código deve acessar a informação por meio de uma variável de ambiente, sem precisar conhecer ou armazenar diretamente o valor sensível.

### 7.4. Qual é a função de `process.env.MENSAGEM_SECRETA`?

Essa expressão permite que a aplicação Node.js leia o valor da variável de ambiente chamada `MENSAGEM_SECRETA`. Dessa forma, a aplicação consegue utilizar a informação fornecida pelo ambiente de execução sem manter o valor diretamente no código-fonte.

### 7.5. Por que utilizar Secrets é importante em uma pipeline de CI/CD?

Porque permite separar informações sensíveis dos arquivos versionados, reduzindo o risco de exposição acidental de senhas, tokens, chaves e outras credenciais. Também facilita o gerenciamento desses valores nas configurações do repositório.

### 7.6. Por que seria inadequado colocar uma senha real de banco de dados diretamente no arquivo `.yml`?

Porque o arquivo pode ser armazenado no histórico do Git e ficar acessível a pessoas com permissão para consultar o repositório. Se a senha for exposta, pessoas não autorizadas poderão tentar acessar o banco de dados. Utilizar GitHub Secrets reduz esse risco, mantendo a credencial separada do código-fonte.

## 8. Conclusão

A atividade demonstrou como utilizar o GitHub Secrets para separar informações sensíveis do código-fonte e disponibilizá-las à aplicação por meio do GitHub Actions e de variáveis de ambiente.

Essa prática contribui para a segurança das pipelines de CI/CD e pode ser aplicada ao gerenciamento de credenciais utilizadas por aplicações e serviços.
