# Tripê 📍

Um app de viagem em grupo, instalável direto na tela de início do celular (PWA), com sincronização em tempo real entre todo mundo do grupo — sem precisar de servidor próprio.

Feito originalmente para acompanhar a viagem da família aos EUA (out/2026), mas pensado pra virar uma base reutilizável em viagens futuras.

## ✨ Funcionalidades

- **Hoje** — timeline do dia atual (calculada pela data), com itens que "acendem" conforme são concluídos. Dias de parque mostram só a entrada do parque; tocar nela abre a lista de atrações/prioridades escolhidas pelo grupo.
- **Mapa** — trajeto ilustrado da viagem, com marcador indicando em qual parada o grupo está, de acordo com o cronograma.
- **Extras** — lista de restaurantes, pontos turísticos e lojas que o grupo quer encaixar, sem dia definido até serem atribuídos a um dia específico.
- **Gastos** — registro de despesas com divisão automática entre os participantes (saldo líquido entre cada par de pessoas).
- **Memória** — "momento do dia" (foto + frase); o álbum completo fica bloqueado até uma data definida após o fim da viagem.
- **Documentos** — voos, carro alugado e hotéis de cada trecho, pesquisável, com o item do dia atual destacado.
- **Modo de edição** — permite corrigir/adicionar itens da timeline direto pelo app, sem precisar editar código.

## 🛠️ Stack

- HTML + CSS + JavaScript puro (sem build, sem framework, sem dependências de npm)
- [Firebase](https://firebase.google.com/) — Firestore (banco de dados em tempo real) + Authentication (anônima, sem login/senha)
- PWA instalável — manifest e ícone gerados dinamicamente dentro do próprio HTML

Tudo num único arquivo (`index.html`), de propósito — fica fácil de hospedar em qualquer lugar (GitHub Pages, Netlify, Vercel) sem precisar de processo de build.

## 🚀 Rodando/publicando sua própria versão

1. Clone ou baixe este repositório
2. Crie um projeto no [Firebase Console](https://console.firebase.google.com):
   - Ative o **Firestore Database** (edição **Standard**, modo de teste)
   - Ative **Authentication → Sign-in method → Anônimo**
   - Em **Configurações do projeto → Seus apps**, registre um app Web e copie o bloco `firebaseConfig`
3. No `index.html`, substitua os valores do objeto `firebaseConfig` (procure por `COLE_AQUI` caso esteja resetando, ou pelas chaves de exemplo) pelos seus
4. Nas **Regras** do Firestore, cole:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /tripe_eua2026/{doc} {
         allow read, write: if request.auth != null;
       }
     }
   }
   ```
5. Suba o `index.html` pro seu repositório e ative o **GitHub Pages** (Settings → Pages → branch `main` → root)
6. Edite o roteiro (array `DAYS`), a rota (`ROUTE_NODES`), os documentos (`DOCS`) e a lista de extras (`DEFAULTS.extras`) direto no código, com os dados da sua própria viagem

## ⚠️ Nota sobre dados e privacidade

Este projeto guarda os dados da viagem (voos, hotéis, roteiro) **direto no código**, e o repositório é público — ou seja, qualquer pessoa com o link consegue ver essas informações (localizadores, PINs de reserva etc.), mesmo não aparecendo em buscadores.

A chave do Firebase (`apiKey` e afins) **não é secreta** — é assim que o Firebase funciona por natureza. Quem protege os dados de verdade são as regras do Firestore acima, que exigem autenticação (mesmo que anônima) pra ler ou escrever.

Se for reusar este projeto pra outra viagem, vale considerar mover dados sensíveis pro Firestore em vez de deixá-los fixos no código, ou simplesmente manter o repositório privado (exige GitHub Pro).

## 🗺️ Possíveis próximos passos

- Suporte a múltiplas viagens (trocar entre elas dentro do app)
- Autenticação por pessoa (em vez de todo mundo anônimo compartilhando o mesmo espaço)
- Notificações reais (exigiria migrar pra um app nativo ou configurar Web Push)

---

Feito com ☕ e um roteiro de 21 dias pelos EUA.
