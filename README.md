# Conteúdo do app LANNEU

Este repositório alimenta o aplicativo da **Liga Acadêmica de Neurologia e
Neurocirurgia do Amapá**. Editar um arquivo aqui muda o app na próxima vez que
alguém o abrir — **sem precisar de nova versão na App Store**.

## Como editar

1. Abra o arquivo pelo site do GitHub e clique no lápis.
2. Faça a alteração.
3. Clique em **Commit changes**.
4. Espere o ✓ verde aparecer ao lado do commit. **Se aparecer ✗ vermelho, o
   conteúdo tem erro e o app vai ignorar aquele arquivo** — clique no ✗ para ver
   o que está errado e corrija.
   - Se a mensagem já vier assim: `✗ conteudo/eventos.json item 3: falta
     "tema"`, ela mesma diz o arquivo, o item e o que falta — corrija isso.
   - Se em vez disso vier uma frase em **inglês** citando `line` e `column`
     (algo como `Expecting ',' delimiter: line 8 column 7`), é sinal de erro
     de digitação no JSON (aspas ou vírgula faltando). Não precisa entender o
     inglês: olhe só o número depois de `line`, vá até aquela linha no editor
     do GitHub e procure uma vírgula ou aspas faltando ali perto.

### O que não mexer

Cada arquivo é um monte de pares `"campo": "valor"` cercados por chaves `{ }`,
colchetes `[ ]` e vírgulas `,`. Você só precisa trocar o **texto que está entre
aspas**. Não apague aspas `"`, vírgulas `,`, chaves `{ }` ou colchetes `[ ]` —
são eles que seguram a estrutura do arquivo. Se sobrar uma vírgula a mais no
fim de uma lista, ou faltar uma aspas, o ✗ vermelho aparece e o arquivo inteiro
para de funcionar até alguém corrigir.

## Os arquivos

| Arquivo | O que contém |
|---|---|
| `conteudo/liga.json` | Texto institucional, Instagram, e-mail, filiações |
| `conteudo/orientadores.json` | Orientadores, com mini-currículo e foto |
| `conteudo/parceiros.json` | Neurologistas e neurocirurgiões parceiros |
| `conteudo/membros.json` | Diretoria, ligantes ativos e membros extras |
| `conteudo/eventos.json` | Organograma de encontros |
| `conteudo/congressos.json` | Congressos e prazos de submissão |
| `conteudo/artigos.json` | Temas de pesquisa |
| `conteudo/galeria.json` | Álbuns que não são aula |
| `fotos/` | Retratos de orientadores e parceiros |

## Regras que valem para todos

- Toda edição preserva `"schemaVersion": 1`. **Não aumente esse número** — o app
  publicado recusa versões que não conhece.
- Atualize `"atualizadoEm"` se quiser; é informativo.
- Datas são sempre `"AAAA-MM-DD"` (ex.: `"2026-09-17"`), nunca `17/09/2026`.
- Horas são sempre `"HH:mm"` (ex.: `"19:00"`).
- Um item com erro é descartado sozinho, sem derrubar os outros. Um erro de
  **sintaxe** (vírgula ou aspas faltando) invalida o arquivo inteiro — por isso
  espere o ✓ verde.

## Trocar a lista de membros na virada do ano

Edite `conteudo/membros.json`. O arquivo tem três listas: `"diretoria"`,
`"ligantesAtivos"` e `"membrosExtras"`. Cada pessoa precisa de `id` **único
em todo o arquivo** (não repita um `id` que já existe em outra seção) e de
`nome`; `cargo` é só para quem está na diretoria.

**Trocar quem ocupa um cargo** (a pessoa sai, outra assume): troque `id`,
`nome` e `cargo` do bloco dela, sem tocar nas chaves `{ }` nem nas vírgulas
ao redor:

```diff
     "diretoria": [
       {
-        "id": "dir-01-caio-de-souza-nascimento",
-        "nome": "Caio de Souza Nascimento",
-        "cargo": "Presidente"
+        "id": "dir-01-maria-eduarda-lima",
+        "nome": "Maria Eduarda Lima",
+        "cargo": "Presidente"
       },
       {
         "id": "dir-02-camille-kuranishi",
```

**Adicionar uma pessoa nova** numa lista: copie o bloco `{ ... }` de alguém
que já está na lista, cole logo abaixo, e dê um `id` que ainda não existe em
nenhuma das três listas. Antes (só a última pessoa da lista):

```json
      {
        "id": "lig-15-adrielly-lima-de-souza",
        "nome": "Adrielly Lima de Souza"
      }
    ],
```

Depois de adicionar a pessoa nova — repare na vírgula que aparece depois do
primeiro `}` (porque agora ele deixou de ser o último bloco) e que o bloco
novo, por ser o último agora, **não** leva vírgula depois do seu `}`:

```json
      {
        "id": "lig-15-adrielly-lima-de-souza",
        "nome": "Adrielly Lima de Souza"
      },
      {
        "id": "lig-16-nome-da-pessoa-nova",
        "nome": "Nome da Pessoa Nova"
      }
    ],
```

Regra prática: todo bloco `{ ... }` de uma lista leva vírgula depois do `}`,
**exceto o último da lista**. Se sobrar (ou faltar) uma vírgula aqui, o ✗
vermelho aparece.

## Mudar a data de um encontro que já existe

Em `conteudo/eventos.json`, ache o encontro pelo `"tema"` e troque só o texto
entre aspas de `"data"` — nada mais na linha muda:

```diff
   "tema": "Neuro-oncologia",
-  "data": "2027-05-13",
+  "data": "2027-05-20",
```

Salvar assim (só o valor entre aspas trocado, a vírgula no fim continua lá) já
é suficiente. O app cancela o lembrete da data antiga e agenda de novo na nova
data automaticamente.

## Adicionar um encontro

Em `conteudo/eventos.json`. Só `id` e `tema` são obrigatórios:

```json
{
  "id": "2027-05-13-neuro-oncologia",
  "tema": "Neuro-oncologia",
  "data": "2027-05-13",
  "hora": "19:00",
  "local": "HU-UNIFAP",
  "responsaveis": ["Fulano", "Beltrano"],
  "materiais": [{ "titulo": "Slides da aula", "url": "https://drive.google.com/..." }],
  "albumFotos": "https://photos.app.goo.gl/...",
  "inscricao": "https://forms.gle/..."
}
```

Assim que a data entrar, o app agenda sozinho o lembrete da véspera (19h) e o do
dia (8h) no celular de quem tem o app instalado. Mudou a data? Ele cancela o
antigo e reagenda. **Data ainda indefinida: use `"data": null`** — aparece como
"Data a definir" e não gera lembrete.

### Campos de um encontro

| Campo | Obrigatório | O que é |
|---|---|---|
| `id` | sim | identificador único do encontro |
| `tema` | sim | título mostrado na lista e na tela do encontro |
| `subtitulo` | não | linha menor abaixo do tema |
| `data` | não | `"AAAA-MM-DD"`, ou `null` para "a definir" |
| `hora` | não | `"HH:mm"` |
| `local` | não | texto livre |
| `responsaveis` | não | lista de nomes |
| `materiais` | não | lista de `{ "titulo", "url" }` |
| `albumFotos` | não | link de álbum de fotos do encontro |
| `inscricao` | não | link de inscrição, para aula aberta. Precisa começar com `http://` ou `https://` — cole o link inteiro do navegador, nunca um texto como `forms.gle/abc` copiado do WhatsApp, ou o ✗ vermelho vai aparecer. Sem este campo, a tela do encontro não mostra nenhum botão de inscrição. |

## Publicar fotos de um evento

Suba as fotos para um álbum do Google Fotos, gere o link de compartilhamento e
cole em `albumFotos` do evento. Para algo que não é aula (confraternização,
estágio), use `conteudo/galeria.json`.

## Editar as filiações da liga

Em `conteudo/liga.json`, a lista `"filiacoes"` mostra as entidades a que a liga
é filiada na aba "A Liga". Só `sigla` é obrigatória:

```json
{
  "filiacoes": [
    { "sigla": "ABLAM", "nome": "Associação Brasileira de Ligas Acadêmicas de Medicina" },
    { "sigla": "SBN", "nome": "Sociedade Brasileira de Neurocirurgia", "url": "https://sbn.org.br" }
  ]
}
```

| Campo | Obrigatório | O que é |
|---|---|---|
| `sigla` | sim | sigla da entidade; item sem `sigla` é descartado sozinho, sem derrubar as outras filiações |
| `nome` | não | nome por extenso. **Não escreva um nome que você não tem certeza** — sigla sozinha é melhor que nome errado de uma entidade real |
| `url` | não | site da entidade. Precisa começar com `http://` ou `https://`, senão o ✗ vermelho vai aparecer |

Sem esta lista (ou com ela vazia), a seção "Filiações" simplesmente não aparece
no app.
