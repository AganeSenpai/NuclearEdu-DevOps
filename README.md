# NuclearEdu DevOps — Challenge AMAZUL / FIAP

**Aluno:** JOÃO VITOR VALAITIS PAULO — RM576724  
**Disciplina:** DevOps Tools & Cloud Computing — Sprint 1  
**Empresa parceira do Challenge:** AMAZUL — Amazônia Azul Tecnologias de Defesa S.A.

**Repositório:** https://github.com/AganeSenpai/NuclearEdu-DevOps

## Proposta
Prova de conceito independente de API educativa sobre aplicações da tecnologia nuclear. Demonstra Docker e implantação em máquina virtual Linux na Microsoft Azure, sem depender de projetos Java ou Mobile.

## Arquitetura
Navegador/cliente HTTP → IP público da VM Ubuntu no Azure (porta TCP 5000) → container Docker (porta 5000) → API Flask/Gunicorn. Não utiliza banco de dados. Consulte `docs/arquitetura.svg`.

## Requisitos
Conta e assinatura Azure com permissão para criar recursos; Azure CLI autenticada (`az login`); chave SSH; acesso à VM; Docker instalado na VM; Git e acesso ao repositório.

## 1. Provisionar Azure via CLI
Em um terminal Bash com Azure CLI:
```bash
bash scripts/azure-criar.sh
```
Registre capturas da criação da VM, IP público e grupo de recursos. A VM poderá gerar custos; confira limites de cota e disponibilidade de `Standard_B2ats_v2` em `mexicocentral`.

## 2. Instalar Docker e implantar a API

A partir da raiz do projeto no Cloud Shell:

```bash
bash scripts/azure-implantar.sh
```

O script usa Azure Run Command para instalar Docker, enviar o codigo, construir a imagem e iniciar o container na VM. Nao requer abrir SSH ao publico.

## 3. Publicar a API

```bash
bash scripts/azure-publicar.sh
```

Confirme PUBLICAR para abrir apenas TCP 5000. Consulte COMO-EXECUTAR-AUTOMATICO.md.

## 4. Validar externamente
Em outro computador ou no navegador local, acesse:
- `http://IP_PUBLICO:5000/`
- `http://IP_PUBLICO:5000/health`
- `http://IP_PUBLICO:5000/api/topicos`
- `http://IP_PUBLICO:5000/api/topicos/1`

Documente evidências com o IP público da VM e o resultado da API. A porta 5000 e aberta pelo script de publicacao no NSG; não exponha a API por mais tempo que o necessário.

## 5. Apagar recursos (obrigatório)
Depois de registrar o vídeo e as capturas:
```bash
bash scripts/azure-excluir.sh
```
Capture a saída que comprova a exclusão. O comando espera a exclusão terminar e `az group exists` deve retornar `false`.

## Como Docker e Cloud Computing são utilizados?
Docker empacota a API e suas dependências numa imagem reproduzível, reduzindo diferenças entre ambientes. A Azure disponibiliza a VM Linux que hospeda o container e permite acesso externo ao protótipo por IP público. Nesta prova de conceito, a VM é provisionada por Azure CLI e removida ao final para evitar custos desnecessários.

## Entrega
- Repositório GitHub acessível ao professor com README, Dockerfile, scripts e diagrama.
- Vídeo YouTube não listado, aproximadamente 3 minutos, mínimo 720p, com áudio ou legendas e demonstração real da VM, container, API e exclusão.
- PDF final com nome, RM, turma, link GitHub e link YouTube.
- Capturas em `evidencias/` (adicionar após execução real).

**Nota:** os scripts são fornecidos, mas a criação da VM e os testes reais dependem de executar em sua conta Azure. Não declare que os recursos foram provisionados antes da execução.

## Implantacao em um comando

Consulte [COMO-EXECUTAR-AUTOMATICO.md](COMO-EXECUTAR-AUTOMATICO.md) e execute `bash scripts/azure-automatico.sh` a partir da raiz do projeto.

## Execucao real verificada em 03/11/2026

VM vm-nuclearedu, Mexico Central, Standard_B2ats_v2, Ubuntu 22.04 LTS. API implantada em Docker e testada interna e externamente: http://158.23.22.150:5000/health e http://158.23.22.150:5000/api/topicos. As capturas estao em evidencias/. O endereco so funciona enquanto os recursos estiverem ativos. A exclusao ainda nao foi realizada para permitir a gravacao do video.
