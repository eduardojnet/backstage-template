
# POC com Backstage

Este README documenta o processo que segui para explorar e compreender o Backstage em uma POC, com foco em entender sua estrutura interna, as integrações de IDP e GitHub, e o processo de criação de repositórios a partir de templates no catálogo.

---

## Objetivos da POC

1. **Compreender o Backstage e sua Estrutura Interna**: Focar em como o frontend e o backend operam juntos em uma única aplicação.
2. **Entender as Integrações de IDP e GitHub**: Explorar as possibilidades de autenticação e interações com GitHub.
3. **Aprender a Criar Repositórios a Partir de Templates do Catálogo**: Automatizar a criação de novos repositórios usando templates predefinidos no Backstage.

---

## 1. Compreendendo o Backstage e sua Estrutura Interna

O Backstage é uma plataforma unificada criada pela Spotify, projetada para centralizar e simplificar a gestão de ferramentas, documentação e integrações para desenvolvedores.

### Estrutura Básica da Aplicação

Eu pude perceber que o Backstage é uma aplicação monolítica que integra **backend** e **frontend** em um único projeto. Ele utiliza Node.js e React, suportando uma série de plugins e integrações externas para configurar ambientes de desenvolvimento completos.

- **Backend**: Processa requisições, gerencia a autenticação, integra-se com APIs externas (como GitHub) e gerencia o catálogo de templates.
- **Frontend**: Implementado com React e carregado pelo Webpack Dev Server, oferece a interface para interagir com o backend e acessar o catálogo de templates, a documentação e plugins.

### Configuração do Ambiente e Estrutura de Diretórios

Para iniciar o projeto Backstage no Docker, eu utilizei o comando:

```bash
docker run -it --rm --network="host" -v $(pwd)/dev:/app -w /app/brqpoc -e GITHUB_TOKEN=<meu_token> node:18 bash
```

Dentro do Backstage, identifiquei algumas pastas principais:

- **/packages/app**: Onde está o frontend, gerenciado pelo Webpack.
- **/packages/backend**: Contém a lógica do backend e configurações de plugins.
- **/packages/catalog**: Contém os templates de catálogo para criação de novos repositórios.

<<<<<<< HEAD
Depois de instalar as dependências com `yarn install` e iniciar o servidor com `yarn dev`, eu consegui acessar o Backstage em `http://localhost:3000`.

### Observações Importantes

Para garantir que o container se comunicasse corretamente com o host, eu precisei definir a rede `--network="host"`. Esse ajuste foi essencial para que as portas funcionassem corretamente, evitando problemas de conexão.

---

## 2. Integrações de IDP e GitHub

Com o Backstage, é possível configurar autenticação usando provedores de identidade (IDP) e integrar com o GitHub para manipular repositórios, além de facilitar a autenticação dos usuários.

### Configuração do IDP (exemplo com GitHub)

Eu configurei o GitHub como provedor de identidade (IDP) para permitir que os usuários façam login diretamente no Backstage. No arquivo `app-config.yaml`, eu adicionei:

```yaml
auth:
  environment: development
  providers:
    github:
      development:
        clientId: <MEU_CLIENT_ID>
        clientSecret: <MEU_CLIENT_SECRET>
```

### Integração com GitHub para Manipulação de Repositórios

Para realizar a integração com GitHub e permitir a criação de repositórios, eu defini um token de acesso (GITHUB_TOKEN) com permissões de leitura e escrita. Esse token foi configurado dentro do container:

```bash
export GITHUB_TOKEN=<meu_token>
```

E então eu adicionei a integração com GitHub no `app-config.yaml`:

```yaml
integrations:
  github:
    - host: github.com
      token: ${GITHUB_TOKEN}
```

Com essa integração, o Backstage consegue interagir com o GitHub para criar, clonar e listar repositórios automaticamente.

---

## 3. Criação de Repositórios a Partir de Templates no Catálogo

O Backstage permite automatizar a criação de novos repositórios a partir de templates YAML definidos no catálogo. Esses templates servem como modelos predefinidos para projetos, facilitando a padronização e a automação.

### Estrutura de um Template

O template é configurado como um arquivo `catalog-info.yaml`, e um exemplo básico é o seguinte:

```yaml
apiVersion: backstage.io/v1alpha1
kind: Template
metadata:
  name: my-template
spec:
  type: service
  owner: my-team
  path: /path/to/template
```

### Processamento de Templates e Criação de Repositórios

Para configurar a criação de repositórios, eu ativei o plugin `scaffolder` no `app-config.yaml`:

```yaml
scaffolder:
  actions:
    - id: create:github:repo
      name: Create GitHub Repository
      description: Creates a new repository on GitHub
```

Depois de selecionar o template no catálogo pelo frontend e preencher os parâmetros necessários (como nome e descrição do repositório), o Backstage usa o token de acesso e a API do GitHub para gerar o novo repositório automaticamente.

### Testes e Resultados

Realizei a criação de repositórios com templates variados, e a aplicação foi bem-sucedida em todas as tentativas. A integração se mostrou confiável e consistente, automatizando o processo de configuração de novos repositórios com base em modelos predefinidos.

---

## Conclusão da POC e Avaliação Pessoal

Durante esta POC, eu pude compreender a estrutura interna do Backstage e como o backend e o frontend trabalham juntos para oferecer uma plataforma robusta para desenvolvedores. A configuração inicial, especialmente em ambiente Docker, exigiu ajustes específicos para permitir a comunicação de rede, mas esses desafios foram superados com o uso da opção `--network="host"`.

A integração com IDP e GitHub foi simples e efetiva, permitindo autenticação segura e manipulação automatizada de repositórios. A capacidade do Backstage de criar novos repositórios a partir de templates demonstra o potencial da plataforma para automação de projetos e padronização de práticas de desenvolvimento.

### Desempenho e Conclusão

Este processo foi enriquecedor e embora eu já tivesse conhecimento do uso da ferramenta pude rever e proporcionar novamente uma visão clara do funcionamento e potencial do Backstage. 
Embora tenha enfrentado algumas dificuldades relativas à tratativas de IPD e, em especial acessos ao GITHUB, acredito que, ao compreender estes detalhes técnicos da configuração, da arquitetura e das integrações, estou mais preparado para explorar e implementar soluções utilizando o Backstage em projetos reais.
=======
Para mais informações, entre em contato com o administrador do Backstage da organização ou o time de desenvolvimento responsável.
v1
>>>>>>> 33e1a91 (Subindo alteração backstate)
