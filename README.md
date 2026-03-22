# vandessonsantiago.com

Site institucional e de conversão para atendimento jurídico, desenvolvido com Astro e Tailwind.

Repositório: https://github.com/vandessansantiago/vandessonsantiago.com

Este projeto contém componentes para página inicial, FAQ, seção "Sobre", bloco de alerta e botões de contato via WhatsApp (wa.me). O conteúdo foi preparado para orientar visitantes e direcionar contatos; não promete resultados ou garantias.

## Requisitos

- Node.js >= 22.12.0 (conforme `package.json`)
- npm (ou outro gerenciador compatível)

## Como rodar localmente

1. Instale dependências:

```bash
npm install
```

2. Rodar em modo desenvolvimento (hot-reload):

```bash
npm run dev
# abre http://localhost:4321
```

3. Gerar build para produção:

```bash
npm run build

# Pré-visualizar o build localmente
npm run preview
```

## Estrutura principal

- `src/pages/` — páginas do site (Astro)
- `src/components/` — componentes reutilizáveis (Hero, About, FAQ, State, HowItWorks, Alert, Footer)
- `src/layouts/` — layouts (DefaultLayout)
- `src/styles/global.css` — estilos globais e import do Tailwind
- `public/` — imagens e assets públicos (ícones, logos, imagens de fundo)

## Deploy

Este projeto é um site estático gerado pelo Astro. Você pode publicar em qualquer serviço que sirva arquivos estáticos (Netlify, Vercel, GitHub Pages, S3, etc).

Exemplo rápido com GitHub Pages (configuração mínima):

1. Configure um workflow de CI (GitHub Actions) ou defina Pages para servir a pasta `dist/` após o build.
2. No CI, execute `npm ci && npm run build` e publique os arquivos gerados.

Se quiser, eu posso adicionar um workflow de GitHub Actions para deploy automático — diga onde deseja hospedar e eu gero o arquivo.

## Observações importantes

- Todos os CTAs direcionam para conversas via WhatsApp com o número usado no projeto.
- O rodapé contém um disclaimer sobre não vinculação ao Google/Facebook e ausência de oferta de serviços governamentais ou venda de criptoativos.

## Contato

Caso queira que eu faça ajustes, adicione deploy automático ou crie um README mais detalhado para publicação, diga qual opção prefere e eu faço as alterações.
