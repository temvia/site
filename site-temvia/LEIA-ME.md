# Site institucional temvia

Site público da temvia. **Separado do sistema.** Não carrega Firebase, Firestore,
engine do Gestor nem qualquer dado de operação. É HTML, CSS e um script sem
dependências externas.

```
CNAME              temvia.com.br
.nojekyll          desliga o processamento Jekyll do GitHub Pages
index.html         landing page (arquivo único: HTML + CSS + JS)
privacidade.html   política de privacidade (rascunho estrutural)
robots.txt
sitemap.xml
assets/
  logo-temvia-dark.png     logo para fundo escuro
  logo-temvia-claro.png    logo para fundo claro
  simbolo-temvia.png       só o símbolo
  icon-192.png / icon-512.png / favicon.png / favicon.ico
  tela-gestor.webp + .png        captura do Gestor (hero)
  tela-roteirizador.webp + .png  captura do Roteirizador
  og-temvia.png            imagem de compartilhamento (WhatsApp, LinkedIn)
  fonts/                   Inter auto-hospedada (subconjunto latin, 100 KB)
```

---

## 1. Repositório e publicação

O site **não** deve morar no repositório `temvia/app`. Se morar, ele sobe junto
com o sistema a cada deploy, e um erro no site derruba o deploy do produto (e
vice-versa). Além disso o repositório do app já tem histórico com estrutura de
operação; o site é público e não precisa dele.

**Recomendação: um repositório novo, `temvia/site`.**

1. Criar o repositório `site` na conta `temvia`.
2. Subir todo o conteúdo desta pasta na raiz do repositório.
3. Em *Settings → Pages*, apontar para a branch `main`, pasta `/ (root)`.
4. Em *Settings → Pages → Custom domain*, colocar `temvia.com.br` e marcar
   **Enforce HTTPS**.
5. No painel do domínio (onde está registrado o `temvia.com.br`), criar:

   | Tipo  | Nome  | Valor |
   |-------|-------|-------|
   | A     | `@`   | `185.199.108.153` |
   | A     | `@`   | `185.199.109.153` |
   | A     | `@`   | `185.199.110.153` |
   | A     | `@`   | `185.199.111.153` |
   | CNAME | `www` | `temvia.github.io` |

   O `app.temvia.com.br` continua exatamente como está — não mexer.

Resultado: `temvia.com.br` = site, `app.temvia.com.br` = sistema.

---

## 2. Os únicos endereços editáveis

Estão no topo do `<script>` no fim do `index.html`. Não há URL espalhada pelo resto do código.

```js
const TEMVIA_LOGIN_URL     = 'https://app.temvia.com.br/';
const TEMVIA_EMAIL         = 'contato@temvia.com.br';
const TEMVIA_FORM_ENDPOINT = 'https://formsubmit.co/ajax/contato@temvia.com.br';
```

- **`TEMVIA_LOGIN_URL`** — destino de todos os botões "Entrar". Está apontando
  para a raiz do app (o Portal de Acesso, que já roteia motorista, gestor,
  passageiro e empresa cliente). Se quiser mandar direto para a Redentor, troque
  por `https://app.temvia.com.br/redentor/`.
- **`TEMVIA_EMAIL`** — e-mail comercial. Aparece nos textos e é o destino do formulário.
- **`TEMVIA_FORM_ENDPOINT`** — ver seção 3.

---

## 3. Formulário de contato comercial

O formulário fica na faixa laranja no fim da página (`#contato`). Todos os
botões "Agendar demonstração" e "Contato" levam até ele.

Campos: Nome*, E-mail*, Celular*, Empresa*, Qual a sua demanda?*, Tamanho da
operação, Mensagem, e um aceite de LGPD obrigatório. Validação em português,
com a mensagem "Este campo é obrigatório." abaixo do campo, máscara automática
de celular e uma isca invisível contra robôs.

### Como ele entrega a mensagem

O GitHub Pages é hospedagem estática: não roda código de servidor e, portanto,
não envia e-mail sozinho. O site usa o **FormSubmit**, que recebe o envio e
repassa por e-mail. É gratuito, sem cadastro e sem servidor.

O endpoint já está configurado no código:

```js
const TEMVIA_FORM_ENDPOINT = 'https://formsubmit.co/ajax/contato@temvia.com.br';
```

**Falta uma ativação única, e ela é obrigatória:**

1. Criar a caixa `contato@temvia.com.br`.
2. Publicar o site e enviar o formulário **uma vez**, você mesmo.
3. O FormSubmit manda um e-mail de confirmação para essa caixa. Clicar no link.
4. A partir daí, todo envio cai direto na caixa, sem passar por lugar nenhum.

Enquanto o passo 3 não for feito, os envios **não chegam**. Faça esse teste
antes de divulgar o endereço em qualquer lugar.

Se preferir ter um painel com o histórico dos leads em vez de depender só do
e-mail, o [Formspree](https://formspree.io) funciona com o mesmo código — troca
só a string por `https://formspree.io/f/XXXXXXXX`.

**Se o envio falhar** (rede fora, serviço indisponível), o formulário não perde
o contato: mostra um botão "Enviar por e-mail" já com todos os campos
preenchidos, mais o endereço para contato direto.

---

---

## 4. Política de privacidade

O `privacidade.html` está com a **estrutura completa e o texto em rascunho**.
Tudo que depende de decisão da temvia está marcado em amarelo tracejado na
própria página: razão social, CNPJ, quem é controlador e quem é operador em cada
situação, prazos de retenção, lista final de fornecedores, encarregado (DPO) e o
fluxo de consentimento para transporte escolar.

Duas coisas antes de publicar como documento vigente:

1. **A seção 2 (controlador × operador) é a mais importante e a mais delicada.**
   Ela precisa refletir exatamente o que está escrito no contrato com a Redentor.
   Se o contrato ainda não trata disso, o contrato é que precisa ser ajustado antes.
2. **Revisão por advogado.** Eu montei a estrutura e escrevi o texto técnico com
   base em como o sistema realmente funciona, mas política de privacidade é peça
   jurídica e produz efeito legal. Não publique como documento final sem revisão.

Enquanto estiver em rascunho, o aviso no topo da página deixa isso explícito para
quem visitar.

---

## 5. As telas do produto na página

O **hero** e a seção **Roteirização** usam capturas reais do sistema, em
`assets/tela-gestor.*` e `assets/tela-roteirizador.*`. Cada uma é servida em dois
formatos: o navegador baixa o `.webp` (leve) e cai no `.png` só se não suportar.
Para trocar por versões novas, substitua os quatro arquivos mantendo os nomes.

As telas de **Motorista**, **Passageiro**, **Escolar** e a tabela de
**planejado × realizado** continuam sendo reconstruções em HTML/CSS com dados
fictícios — são responsivas, nítidas em qualquer tela e não expõem ninguém.

**Antes de publicar, confirme uma coisa:** as capturas mostram nomes, endereços
e horários de embarque. Se esses dados vierem da base real do Evamo, não podem
ir para uma página pública indexada pelo Google — é exatamente o problema que a
Fase 1 de segurança fechou. Use sempre a operação de demonstração, com nomes e
endereços inventados.

## 6. Manutenção

- **Nunca** adicionar Firebase, Firestore, chave de API ou qualquer engine aqui.
- Ao mexer em texto, manter **temvia sempre em minúsculas**.
- A paleta é a mesma do sistema (`#0B0E13` de fundo, âmbar `#FBAE17`), e vale a
  regra do âmbar: ele só aparece na marca e na ação principal. Se o âmbar começar
  a aparecer em toda parte, a página perde hierarquia.
- Depois de alterar `index.html`, conferir em celular real — não só no
  redimensionamento do navegador.
- Ao publicar uma página nova, acrescentá-la ao `sitemap.xml`.
