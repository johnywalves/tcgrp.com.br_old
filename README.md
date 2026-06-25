# Guia de Instalação e Execução do Jekyll (Ambiente Isolado)

Este guia configura o ambiente Ruby, Jekyll e Bundler de forma genérica, instalando as dependências diretamente no diretório do usuário para evitar erros de permissão (`Gem::FilePermissionError`).

## 1. Instalação do Ruby no Sistema

Atualize os pacotes do sistema e instale a versão completa do Ruby:

```bash
sudo apt-get update && sudo apt-get install ruby-full -y
```

## 2. Instalação do Jekyll e Bundler (Escopo do Usuário)

Instale as ferramentas base utilizando a flag --user-install para evitar a necessidade de privilégios sudo:

```bash
gem install jekyll bundler --user-install

## 3. Configuração das Variáveis de Ambiente (PATH)

Execute os comandos abaixo para detectar automaticamente o caminho das gems do usuário e adicioná-lo ao seu arquivo de configuração do terminal (~/.bashrc):

```bash
# Adiciona o caminho dos binários instalados pelo usuário ao PATH de forma dinâmica
echo 'export PATH="$(gem env user_gemhome)/bin:$PATH"' >> ~/.bashrc

# Recarrega as configurações do terminal na sessão atual
source ~/.bashrc
```

## 4. Configuração Local do Bundler

Navegue até a pasta raiz do seu projeto Jekyll e configure o Bundler para armazenar e ler as dependências localmente na pasta de gems do usuário (evitando conflitos com o diretório global /var/lib/gems):

```bash
# Configura o escopo de instalação das gems do projeto
bundle config set --local path "$(gem env user_gemhome)"

# Instala as dependências declaradas no Gemfile
bundle install
```

Nota: Se houver erro de incompatibilidade de versão do Bundler com o seu Gemfile.lock, force a atualização do arquivo executando: bundle update --bundler antes de rodar o bundle install.

## 5. Execução do Servidor de Desenvolvimento

Inicie o servidor do Jekyll. As flags --host e --port garantem que o serviço fique acessível externamente em ambientes de containers ou code-server:

```bash
bundle exec jekyll serve --host 0.0.0.0 --port 8080
```

---

### Principais vantagens desta abordagem em Markdown:

1. **Dynamic Paths**: O uso de `$(gem env user_gemhome)` elimina caminhos fixos (como `/config/...` ou versões estáticas de Ruby `3.2.0`), tornando o bloco reutilizável por qualquer desenvolvedor do projeto.

2. **`source` incluído**: Garante que o fluxo de automação não quebre entre a configuração do ambiente e o download das dependências.

3. **Pronto para Containers**: A flag `--host 0.0.0.0` já previne problemas de rede comuns no mapeamento de portas do `code-server` e Docker.